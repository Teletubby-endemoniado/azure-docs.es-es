---
title: Implementación de la recuperación ante desastres mediante copias de seguridad y restauración en API Management
titleSuffix: Azure API Management
description: Obtenga información acerca de cómo usar las tareas de copias de seguridad y restauración para llevar a cabo la recuperación ante desastres en Azure API Management.
services: api-management
author: dlepow
ms.service: api-management
ms.topic: article
ms.date: 08/20/2021
ms.author: danlep
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: e00882764283fec7ec9ab3252b5997f682411557
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128652688"
---
# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Procedimiento para implementar la recuperación ante desastres mediante copias de seguridad y restauración del servicio en Azure API Management

Si publica y administra las API a través de Azure API Management, podrá aprovechar numerosas funcionalidades de infraestructura y tolerancia a errores que, de lo contrario, debería diseñar, implementar y administrar de manera manual. La plataforma Azure mitiga una gran cantidad de posibles errores a un costo reducido.

Para recuperarse de problemas de disponibilidad que afectan la región que hospeda el servicio de API Management, esté preparado para reconstituir el servicio en otra región en cualquier momento. Según el objetivo de tiempo de recuperación, puede que quiera mantener un servicio en espera en una o varias regiones. También, puede que quiera intentar mantener sincronizados la configuración y el contenido con el servicio activo según el objetivo de punto de recuperación. Las características de copia de seguridad y restauración del servicio ofrecen los bloques de creación necesarios para implementar una estrategia de recuperación ante desastres.

Las operaciones de copia de seguridad y restauración también pueden usarse para replicar la configuración del servicio API Management entre entornos operativos, por ejemplo, desarrollo y almacenamiento provisional. Sepa que los datos del entorno de ejecución, como los usuarios y las suscripciones, también se copiarán, lo que no siempre podría ser deseable.

En esta guía se muestra cómo automatizar las operaciones de copia de seguridad y restauración y cómo garantizar la correcta autenticación de las solicitudes de copia de seguridad y restauración mediante Azure Resource Manager.

> [!IMPORTANT]
> La operación de restauración no cambia la configuración del nombre de host personalizado del servicio de destino. Se recomienda usar el mismo nombre de host personalizado y el mismo certificado TLS para los servicios activos y en espera, de modo que, una vez finalizada la operación de restauración, el tráfico pueda redirigirse a la instancia en espera mediante un simple cambio de DNS CNAME.
>
> La operación de copia de seguridad no captura los datos de registro previamente agregados usados en los informes mostrados en la hoja Analytics de Azure Portal.

> [!WARNING]
> Cada copia de seguridad expira después de treinta días. Si intenta restaurar una copia de seguridad una vez transcurrido el período de expiración de treinta días, se producirá un error en la restauración con un mensaje `Cannot restore: backup expired`.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="authenticating-azure-resource-manager-requests"></a>Solicitudes de autenticación del Administrador de recursos de Azure

> [!IMPORTANT]
> La API de REST para copia de seguridad y restauración utiliza el Administrador de recursos de Azure y tiene un mecanismo de autenticación diferente que las API de REST para administrar las entidades de la administración de API. Los pasos de esta sección describen cómo autenticar las solicitudes del Administrador de recursos de Azure. Par obtener más información, consulte [Solicitudes de autenticación del Administrador de recursos de Azure](/rest/api/index).

Todas las tareas que se realizan en los recursos mediante Azure Resource Manager deben autenticarse con Azure Active Directory con los siguientes pasos:

-   Agregue una aplicación al inquilino de Azure Active Directory.
-   Establezca permisos para la aplicación que ha agregado.
-   Obtenga el token para autenticar solicitudes al Administrador de recursos de Azure.

### <a name="create-an-azure-active-directory-application"></a>Creación de una aplicación de Azure Active Directory

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Mediante la suscripción que contiene la instancia de servicio de API Management, vaya a [Azure Portal - Registros de aplicaciones](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) para registrar una aplicación en Active Directory.
    > [!NOTE]
    > Si el directorio predeterminado de Azure Active Directory no está visible en su cuenta, póngase en contacto con el administrador de la suscripción de Azure para que le conceda los permisos necesarios para su cuenta.
1. Seleccione **+ Nuevo registro**.
1. En la página **Registrar una aplicación**, establezca los valores de la manera siguiente:
    
    * Establezca **Nombre** en un nombre con sentido;
    * Establezca **Tipos de cuenta admitidos** en **Solo las cuentas de este directorio organizativo**. 
    * En **URI de redirección**, escriba una dirección URL de marcador de posición, como `https://resources`. Aunque es un campo obligatorio, el valor no se usa más adelante. 
    * Seleccione **Registrar**.

### <a name="add-permissions"></a>Adición de permisos

1. Una vez creada la aplicación, seleccione **Permisos de la API** >  **+ Agregar un permiso**.
1. Seleccione **API de Microsoft**.
1. Seleccione **Administración de servicio de Azure**.

    :::image type="content" source="./media/api-management-howto-disaster-recovery-backup-restore/add-app-permission.png" alt-text="Captura de pantalla que muestra cómo agregar permisos de aplicación":::. 

1. Haga clic en **Permisos delegados** al lado de la aplicación recién agregada y active la casilla **Acceso a la administración de servicios de Azure como usuarios de la organización (versión preliminar)** .

    :::image type="content" source="./media/api-management-howto-disaster-recovery-backup-restore/delegated-app-permission.png" alt-text="Captura de pantalla que muestra la incorporación de permisos de aplicación delegados":::.

1. Seleccione **Agregar permisos**.

### <a name="configure-your-app"></a>Configurar la aplicación

Antes de llamar a las API que generan la copia de seguridad y restauran, debe obtener un token. En el ejemplo siguiente se utiliza el paquete de NuGet [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) para recuperar el token.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace GetTokenResourceManagerRequests
{
    class Program
    {
        static void Main(string[] args)
        {
            var authenticationContext = new AuthenticationContext("https://login.microsoftonline.com/{tenant id}");
            var result = authenticationContext.AcquireTokenAsync("https://management.azure.com/", "{application id}", new Uri("{redirect uri}"), new PlatformParameters(PromptBehavior.Auto)).Result;

            if (result == null) {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

Reemplace `{tenant id}`, `{application id}` y `{redirect uri}` mediante las siguientes instrucciones:

1. Reemplace `{tenant id}` por el identificador del inquilino de la aplicación de Azure Active Directory que se creó. Para acceder al identificador, haga clic en **Registros de aplicaciones** -> **Puntos de conexión**.

    ![Puntos de conexión][api-management-endpoint]

2. Reemplace `{application id}` con el valor obtenido en la página **Configuración**.
3. Reemplace el valor `{redirect uri}` por el valor de la pestaña **Redirect URIs** (URI de redireccionamiento) de la aplicación de Azure Active Directory.

    Una vez especificados los valores, el ejemplo de código debe devolver un token similar al ejemplo siguiente:

    ![Token][api-management-arm-token]

    > [!NOTE]
    > El token puede expirar tras un período determinado. Vuelva a ejecutar el ejemplo de código para generar un token nuevo.

## <a name="calling-the-backup-and-restore-operations"></a>Llamada a operaciones de copia de seguridad y restauración

Las API REST son [servicio API Management: Copia de seguridad](/rest/api/apimanagement/2020-12-01/api-management-service/backup) y [servicio API Management: Restauración](/rest/api/apimanagement/2020-12-01/api-management-service/restore).

> [!NOTE]
> También se pueden realizar operaciones de copia de seguridad y restauración con los comandos [_Backup-AzApiManagement_](/powershell/module/az.apimanagement/backup-azapimanagement) y [_Restore-AzApiManagement_](/powershell/module/az.apimanagement/restore-azapimanagement) de PowerShell, respectivamente.

Antes de llamar a las operaciones de "copia de seguridad y restauración" descritas en las secciones siguientes, establezca el encabezado de solicitud de autorización para la llamada REST.

```csharp
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

### <a name="back-up-an-api-management-service"></a><a name="step1"> </a>Crear una copia de seguridad del servicio API Management

Para crear una copia de seguridad del servicio API Management, emita esta solicitud HTTP:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}
```

donde:

-   `subscriptionId`: identificador de la suscripción que contiene el servicio API Management del que intenta crear una copia de seguridad
-   `resourceGroupName` : nombre del grupo de recursos del servicio Azure API Management
-   `serviceName` : el nombre del servicio API Management del que desea crear una copia de seguridad que se especificó durante su creación
-   `api-version`: reemplace por una versión compatible de la API REST, como `2020-12-01`

En el cuerpo de la solicitud, especifique el nombre de la copia de seguridad, el nombre del contenedor de blobs, la clave de acceso y el nombre de la cuenta de almacenamiento de Azure de destino:

```json
{
    "storageAccount": "{storage account name for the backup}",
    "accessKey": "{access key for the account}",
    "containerName": "{backup container name}",
    "backupName": "{backup blob name}"
}
```

Establezca el valor del encabezado de solicitud `Content-Type` en `application/json`.

La creación de una copia de seguridad es una operación de larga ejecución que puede tardar más de un minuto en completarse. Si la solicitud se realizó correctamente y empezó el proceso de copia de seguridad, recibirá un código de estado de respuesta `202 Accepted` con un encabezado `Location`. Realice solicitudes "GET" en la URL del encabezado `Location` para averiguar el estado de la operación. Mientras se crea la copia de seguridad, recibirá el código de estado "202 Aceptado". El código de respuesta `200 OK` indica que la operación de copia de seguridad se ha completado correctamente.

### <a name="restore-an-api-management-service"></a><a name="step2"> </a>Restaurar el servicio API Management

Para restaurar el servicio API Management de una copia de seguridad anterior, realice esta solicitud HTTP:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}
```

donde:

-   `subscriptionId`: identificador de la suscripción que contiene el servicio API Management en el que se restaura una copia de seguridad.
-   `resourceGroupName`: nombre del grupo de recursos que contiene el servicio Azure API Management en el que se restaura una copia de seguridad.
-   `serviceName`: el nombre del servicio API Management que desea restaurar que se especificó durante su creación.
-   `api-version`: reemplazar por `api-version=2020-12-01`

En el cuerpo de la solicitud, especifique la ubicación del archivo de copia de seguridad. Es decir, agregue el nombre de la cuenta de almacenamiento de Azure, la clave de acceso, el nombre del contenedor de blob y el nombre de la copia de seguridad:

```json
{
    "storageAccount": "{storage account name for the backup}",
    "accessKey": "{access key for the account}",
    "containerName": "{backup container name}",
    "backupName": "{backup blob name}"
}
```

Establezca el valor del encabezado de solicitud `Content-Type` en `application/json`.

La restauración es una operación de larga duración que puede tardar 30 minutos o más en completarse. Si la solicitud se realizó correctamente y empezó el proceso de restauración, recibirá un código de estado de respuesta `202 Accepted` con un encabezado `Location`. Realice solicitudes "GET" en la URL del encabezado `Location` para averiguar el estado de la operación. Mientras se realiza la restauración, recibirá el código de estado "202 Aceptado". El código de respuesta `200 OK` indica que la operación de restauración se ha completado correctamente.

> [!IMPORTANT]
> **La SKU** en la que desea restaurar el servicio **debe coincidir** con la SKU del servicio del que ha creado una copia de seguridad que desea restaurar.
>
> Los **cambios** que se realicen en la configuración del servicio (por ejemplo, en la API, las directivas o la apariencia del portal para desarrolladores) con la operación de restauración en curso **pueden sobrescribirse**.

## <a name="constraints-when-making-backup-or-restore-request"></a>Restricciones al realizar una solicitud de copia de seguridad o restauración

-   Mientras la copia de seguridad esté en curso, **evite hacer cambios de administración en el servicio**, como una actualización o un cambio a una versión anterior de una SKU, el cambio en un nombre de dominio, etc.
-   La restauración de una **copia de seguridad se garantiza solo durante 30 días** a partir del momento en que esta se crea.
-   Es posible que los **cambios** que se realicen en la configuración del servicio (por ejemplo, las API, las directivas y la apariencia del portal para desarrolladores) mientras se está realizando la operación de copia de seguridad **no se incluyan en la copia de seguridad y se pierdan**.
-   Si la cuenta de Azure Storage está habilitada para [firewall][azure-storage-ip-firewall], el cliente debe **permitir** el conjunto de [direcciones IP del plano de control de Azure API Management][control-plane-ip-address] en su cuenta de almacenamiento para que su copia de seguridad o la restauración funcionen. La cuenta de Azure Storage puede estar en cualquier región de Azure excepto en la que se encuentra el servicio API Management. Por ejemplo, si el servicio API Management está en Oeste de EE. UU., la cuenta de Azure Storage puede estar en Oeste de EE. UU. 2 y el cliente debe abrir la dirección IP del plano de control 13.64.39.16 (dirección IP del plano de control de API Management de Oeste de EE. UU.) en el firewall. Esto se debe a que a las solicitudes a Azure Storage no se les aplica SNAT a una dirección IP pública desde Compute > (plano de control de Azure API Management) en la misma región de Azure. Ala solicitud de almacenamiento entre regiones se le aplicará SNAT a la dirección IP pública.
-   [Uso compartido de recursos entre orígenes (CORS)](/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) **no** debería estar habilitado en Blob Storage en la cuenta de Azure Storage.
-   **La SKU** en la que desea restaurar el servicio **debe coincidir** con la SKU del servicio del que ha creado una copia de seguridad que desea restaurar.

## <a name="what-is-not-backed-up"></a>De lo que no se hace una copia de seguridad
-   Los **datos de uso** con los que se crean informes de análisis **no se incluyen** en la copia de seguridad. La [API de REST de Azure API Management][azure api management rest api] permite recibir de forma periódica informes de análisis para guardarlos en un lugar seguro.
-   Certificados [TLS/SSL de dominio personalizado](configure-custom-domain.md).
-   [Certificado de entidad de certificación personalizado](api-management-howto-ca-certificates.md), que incluye certificados raíz o intermedios cargados por el cliente.
-   Configuración de integración de [red virtual](api-management-using-with-vnet.md).
-   Configuración de [identidad administrada](api-management-howto-use-managed-service-identity.md).
-   Configuración de [diagnóstico de Azure Monitor](api-management-howto-use-azure-monitor.md).
-   Configuración de [protocolos y cifrado](api-management-howto-manage-protocols-ciphers.md).
-   Contenido del [portal para desarrolladores](developer-portal-faq.md#is-the-portals-content-saved-with-the-backuprestore-functionality-in-api-management).

La frecuencia con la que se crean las copias de seguridad afecta al objetivo de punto de recuperación. Para minimizarlo, se recomienda implementar copias de seguridad habituales y realizar copias de seguridad a petición después de hacer cambios en el servicio API Management.

## <a name="next-steps"></a>Pasos siguientes

Consulte los recursos siguientes para ver distintos tutoriales del proceso de copia de seguridad y restauración.

-   [Replicate Azure API Management Accounts (Réplica de cuentas de Administración de API de Azure)](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
-   [Automating API Management Backup and Restore with Logic Apps](https://github.com/Azure/api-management-samples/tree/master/tutorials/automating-apim-backup-restore-with-logic-apps) (Automatización de la copia de seguridad y restauración de API Management con Logic Apps)
-   [Azure API Management: Backing Up and Restoring Configuration](/archive/blogs/stuartleeks/azure-api-management-backing-up-and-restoring-configuration) (Azure API Management: copia de seguridad y restauración de la configuración)
    _El enfoque detallado por Stuart no coincide con la orientación oficial, pero es interesante._

[backup an api management service]: #step1
[restore an api management service]: #step2
[azure api management rest api]: /rest/api/apimanagement/apimanagementrest/api-management-rest
[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png
[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
[control-plane-ip-address]: api-management-using-with-vnet.md#control-plane-ip-addresses
[azure-storage-ip-firewall]: ../storage/common/storage-network-security.md#grant-access-from-an-internet-ip-range
