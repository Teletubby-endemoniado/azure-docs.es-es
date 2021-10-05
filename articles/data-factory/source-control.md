---
title: Control de código fuente
description: Aprenda a configurar el control de código fuente en Azure Data Factory
ms.service: data-factory
ms.subservice: ci-cd
author: nabhishek
ms.author: abnarain
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/22/2021
ms.openlocfilehash: 06bdd49df0f8a4d79ffece298fee2ea2691b0796
ms.sourcegitcommit: e8c34354266d00e85364cf07e1e39600f7eb71cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129219218"
---
# <a name="source-control-in-azure-data-factory"></a>Control de código fuente en Azure Data Factory
[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

De forma predeterminada, la creación en la experiencia de la interfaz de usuario de Azure Data Factory tiene lugar directamente con los datos de factoría de datos. Esta experiencia tiene las siguientes limitaciones:

- El servicio Data Factory no incluye un repositorio que permita almacenar las entidades JSON para los cambios. La única manera de guardar los cambios es mediante el botón **Publicar todo** y todos los cambios se publican directamente en el servicio Data Factory.
- El servicio Data Factory no está optimizado para la colaboración ni el control de versiones.
- No se incluye la plantilla de Azure Resource Manager necesaria para implementar Data Factory.

Para proporcionar una mejor experiencia de creación, Azure Data Factory le permite configurar un repositorio de Git con Azure Repos o GitHub. Git es un sistema de control de versiones que facilita la colaboración y el seguimiento de cambios. En este artículo se describe cómo configurar y trabajar en un repositorio de Git junto con los procedimientos recomendados de resaltado y una guía de solución de problemas.

> [!NOTE]
> Hemos agregado compatibilidad pública con GitHub en Azure Gov, Azure China. Consulte el [blog del anuncio](https://techcommunity.microsoft.com/t5/azure-data-factory/cicd-improvements-with-github-support-in-azure-government-and/ba-p/2686918).

Para obtener más información sobre cómo se integra Azure Data Factory con Git, vea el siguiente vídeo del tutorial de 15 minutos:

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4GNKv]

## <a name="advantages-of-git-integration"></a>Ventajas de la integración de Git

La siguiente es una lista de algunas de las ventajas que ofrece la integración de Git a la experiencia de creación:

-   **Control de código fuente:** A medida que las cargas de trabajo de la factoría de datos se vuelven cruciales, le convendría integrar la factoría con Git para aprovechar varias ventajas de control de código fuente como las siguientes:
    -   Capacidad para realizar el seguimiento y la auditoría de los cambios.
    -   Capacidad para revertir los cambios que presentaron errores.
-   **Operaciones de guardado parcial:** al crear en el servicio Data Factory, no se pueden guardar los cambios como borrador y todas las publicaciones deben superar la validación de Data Factory. Tanto si las canalizaciones no están terminadas como si simplemente no desea perder los cambios en caso de que se bloquee el equipo, la integración de Git permite cambios incrementales de los recursos de Data Factory, con independencia del estado en el que se encuentren. La configuración de un repositorio de Git le permite guardar los cambios, de forma que solo realiza una publicación cuando ha probado los cambios y está satisfecho con ellos.
-   **Colaboración y control:** si varios miembros del equipo colaboran en la misma factoría, puede que le convenga permitir que sus compañeros de equipo colaboren entre sí mediante un proceso de revisión de código. También puede configurar la factoría de modo que no todos los colaboradores tengan los mismos permisos. Es posible que algunos miembros del equipo solo puedan hacer cambios a través de Git, pero que solo algunas personas del equipo puedan publicar los cambios en la factoría.
-   **Mejor proceso de CI/CD:**  si va a realizar implementaciones en varios entornos con un [proceso de entrega continua](continuous-integration-delivery.md), la integración de Git facilita ciertas acciones. Algunas de estas acciones incluyen:
    -   Configurar la canalización de versión para que se desencadene automáticamente en cuanto se realicen cambios en la factoría de desarrollo.
    -   Personalizar las propiedades de la factoría que están disponibles como parámetros en la plantilla de Resource Manager. Puede ser útil mantener solo el conjunto requerido de propiedades como parámetros, y tener todo lo demás codificado.
-   **Mejor rendimiento:** por término medio, una factoría con la integración de Git realiza cargas 10 veces más rápido que una creación en el servicio Data Factory. Esta mejora del rendimiento se debe a que los recursos se descargan mediante Git.

> [!NOTE]
> La creación directa con el servicio Data Factory está deshabilitada en la experiencia de usuario de Azure Data Factory cuando se configura un repositorio de Git. Los cambios realizados a través de PowerShell o un SDK se publican directamente en el servicio Data Factory y no se escriben en Git.

## <a name="connect-to-a-git-repository"></a>Conexión a un repositorio de Git

Hay cuatro maneras diferentes de conectar un repositorio de Git a la factoría de datos tanto para Azure Repos como para GitHub. Después de conectarse a un repositorio de Git, puede ver y administrar la configuración en el [concentrador de administración](author-management-hub.md), en la **configuración de Git** de la sección de **control de código fuente**.

### <a name="configuration-method-1-home-page"></a>Método de configuración 1: Página de inicio

En la página principal de Azure Data Factory, seleccione **Set up code repository** (Configurar el repositorio de código) en la parte superior.

:::image type="content" source="media/doc-common-process/set-up-code-repository.png" alt-text="Configuración de un repositorio de código desde la página principal":::

### <a name="configuration-method-2-authoring-canvas"></a>Método de configuración 2: Lienzo de creación

En el lienzo de creación de la experiencia de usuario de Azure Data Factory, seleccione el menú desplegable **Data Factory** y, luego, **Set up code repository** (Configurar el repositorio de código).

:::image type="content" source="media/author-visually/configure-repo-2.png" alt-text="Configuración de los parámetros del repositorio de código desde la creación":::

### <a name="configuration-method-3-management-hub"></a>Método de configuración 3: Centro de administración

Vaya al centro de administración en la experiencia de usuario de ADF. Seleccione **Configuración de Git** en la sección **Control de código fuente**. Si no tiene ningún repositorio conectado, haga clic en **Configurar**.

:::image type="content" source="media/author-visually/configure-repo-3.png" alt-text="Configuración del repositorio de código en el centro de administración":::

### <a name="configuration-method-4-during-factory-creation"></a>Método de configuración 4: Durante la creación de la factoría

Al crear una nueva factoría de datos en Azure Portal, puede configurar la información del repositorio de Git en la pestaña **Configuración de Git**.

> [!NOTE]
> Al configurar Git en Azure portal, parámetros como el nombre del proyecto y el nombre del repositorio deben escribirse manualmente (no forman parte de una lista desplegable).

:::image type="content" source="media/author-visually/configure-repo-4.png" alt-text="Configuración de los parámetros del repositorio de código desde Azure Portal":::

## <a name="author-with-azure-repos-git-integration"></a>Creación con la integración de Git de Azure Repos

La creación visual con la integración de Git de Azure Repos admite el control del código fuente y la colaboración para trabajar en canalizaciones de factoría de datos. Puede asociar una factoría de datos con un repositorio de organizaciones de Git de Azure Repos para el control del código fuente, la colaboración, el control de versiones, etc. Una sola organización de Git de Azure Repos puede tener varios repositorios, pero un repositorio de Git de Azure Repos solo puede asociarse a una factoría de datos. Si no tiene un repositorio o una organización de Azure Repos, siga [estas instrucciones](/azure/devops/organizations/accounts/create-organization-msa-or-work-student) para crear los recursos.

> [!NOTE]
> Puede almacenar archivos de datos y script en un repositorio de Git de Azure Repos. Sin embargo, tiene que cargar manualmente los archivos en Azure Storage. Una canalización de factoría de datos no carga automáticamente archivos de script ni datos en un repositorio de Git de Azure Repos en Azure Storage.

### <a name="azure-repos-settings"></a>Configuración de Azure Repos

:::image type="content" source="media/author-visually/repo-settings.png" alt-text="Configuración del repositorio de código":::

El panel de configuración muestra la siguiente configuración del repositorio de código de Azure Repos:

| Configuración | Descripción | Value |
|:--- |:--- |:--- |
| **Tipo de repositorio** | Tipo de repositorio de código de Azure Repos.<br/> | Git o GitHub de Azure DevOps |
| **Azure Active Directory** | El nombre de su inquilino de Azure AD. | `<your tenant name>` |
| **Organización de Azure Repos** | Nombre de la organización de Azure Repos. Puede buscar el nombre de la organización de Azure Repos en `https://{organization name}.visualstudio.com`. Puede [iniciar sesión en la organización de Azure Repos](https://www.visualstudio.com/team-services/git/) para acceder a su perfil de Visual Studio y ver sus proyectos y repositorios. | `<your organization name>` |
| **ProjectName** | Nombre de proyecto de Azure Repos. Puede buscar el nombre de proyecto de Azure Repos en `https://{organization name}.visualstudio.com/{project name}`. | `<your Azure Repos project name>` |
| **RepositoryName** | Nombre del repositorio de código de Azure Repos. Los proyectos de Azure Repos contienen repositorios de Git para administrar el código fuente a medida que crece el proyecto. Puede crear un repositorio nuevo o usar uno existente en el proyecto. | `<your Azure Repos code repository name>` |
| **Rama de colaboración** | Rama de colaboración de Azure Repos que se usa para la publicación. De forma predeterminada, es `main`. Cámbiela en caso de que desee publicar recursos de otra rama. | `<your collaboration branch name>` |
| **Carpeta raíz** | Carpeta raíz de la rama de colaboración de Azure Repos. | `<your root folder name>` |
| **Import existing Data Factory resources to repository** (Importar recursos existentes de Data Factory en el repositorio). | Especifica si se deben importar los recursos de la factoría de datos existente del **lienzo de creación** de UX en un repositorio Git de Azure Repos. Active la casilla para importar los recursos de la factoría de datos en el repositorio Git asociado en formato JSON. Esta acción exporta cada recurso individualmente (es decir, los servicios vinculados y los conjuntos de datos se exportan a archivos JSON independientes). Cuando esta casilla no está activada, no se importan los recursos existentes. | Activada (valor predeterminado) |
| **Rama donde importar recursos** | Especifica en qué rama se importan los recursos de la factoría de datos (canalizaciones, conjuntos de datos, servicios vinculados etc.). Puede importar recursos en una de las siguientes ramas: a. Colaboración b. Crear nuevo c. Usar existente |  |

> [!NOTE]
> Si usa Microsoft Edge y no ve ningún valor en el menú desplegable de la cuenta de Azure DevOps, agregue https://*.visualstudio.com a la lista de sitios de confianza.

### <a name="use-a-different-azure-active-directory-tenant"></a>Uso de un inquilino de Azure Active Directory distinto

El repositorio Git de Azure Repos puede estar en un inquilino de Azure Active Directory distinto. Para especificar otro inquilino de Azure AD, deberá tener permisos de administrador para la suscripción de Azure que utilice. Para más información, consulte [Agregar o cambiar los administradores de la suscripción de Azure](../cost-management-billing/manage/add-change-subscription-administrator.md#to-assign-a-user-as-an-administrator).

> [!IMPORTANT]
> Para conectarse a otro directorio de Azure Active Directory, el usuario que ha iniciado sesión debe formar parte de ese directorio de Active Directory. 

### <a name="use-your-personal-microsoft-account"></a>Uso de la cuenta de Microsoft personal

Para usar una cuenta de Microsoft personal para la integración de Git, puede vincular el repositorio de Azure personal al entorno de Active Directory de su organización.

1. Agregue la cuenta de Microsoft personal al entorno de Active Directory de su organización como invitado. Para más información, vea [Incorporación de usuarios de colaboración B2B de Azure Active Directory en Azure Portal](../active-directory/external-identities/add-users-administrator.md).

2. Inicie sesión en Azure Portal con la cuenta de Microsoft personal. A continuación, cambie al entorno de Active Directory de su organización.

3. Vaya a la sección de Azure DevOps, donde verá ahora el repositorio personal. Seleccione el repositorio y establezca la conexión con Active Directory.

Después de estos pasos de configuración, su repositorio personal está disponible al configurar la integración de Git en la interfaz de usuario de Data Factory.

Para más información sobre cómo conectar Azure Repos al entorno de Active Directory de su organización, vea [Connect your Azure DevOps organization to Azure Active Directory](/azure/devops/organizations/accounts/connect-organization-to-azure-ad) (Conexión de la organización de Azure DevOps a Azure Active Directory).

## <a name="author-with-github-integration"></a>Creación con la integración de GitHub

La creación visual con la integración de GitHub admite el control del código fuente y la colaboración para trabajar en canalizaciones de factoría de datos. Puede asociar una factoría de datos con un repositorio de cuentas de GitHub para el control del código fuente, la colaboración y el control de versiones. Una sola cuenta de GitHub puede tener varios repositorios, pero un repositorio de GitHub solo puede asociarse a una factoría de datos. Si no tiene un repositorio o una cuenta de GitHub, siga [estas instrucciones](https://github.com/join) para crear los recursos.

La integración de GitHub con Data Factory admite tanto GitHub público (es decir, [https://github.com](https://github.com)) como GitHub Enterprise. Puede usar los repositorios de GitHub públicos y privados con Data Factory siempre que tenga permisos de lectura y escritura para el repositorio en GitHub.

Para configurar un repositorio de GitHub, deberá tener permisos de administrador para la suscripción de Azure que use.

### <a name="github-settings"></a>Configuración de GitHub

:::image type="content" source="media/author-visually/github-integration-image2.png" alt-text="Configuración del repositorio de GitHub":::

En el panel configuración se muestra la siguiente configuración del repositorio de GitHub:

| **Configuración** | **Descripción**  | **Valor**  |
|:--- |:--- |:--- |
| **Tipo de repositorio** | Tipo de repositorio de código de Azure Repos. | GitHub |
| **Use GitHub Enterprise** (Usar GitHub Enterprise) | Casilla para seleccionar GitHub Enterprise | no seleccionado (valor predeterminado) |
| **GitHub Enterprise URL** (URL de GitHub Enterprise) | La dirección URL de GitHub Enterprise (debe ser HTTPS para el servidor local de GitHub Enterprise). Por ejemplo: `https://github.mydomain.com`. Solo es necesario si se selecciona **Use GitHub Enterprise** (Usar GitHub Enterprise). | `<your GitHub enterprise url>` |                                                           
| **Cuenta de GitHub** | El nombre de la cuenta de GitHub. Este nombre se puede encontrar en https:\//github.com/{nombre de cuenta}/{nombre de repositorio}. Para ir a esta página se le pide que introduzca credenciales de OAuth de GitHub en la cuenta de GitHub. | `<your GitHub account name>` |
| **Nombre del repositorio**  | El nombre del repositorio de código de GitHub. Las cuentas de GitHub contienen repositorios de Git para administrar el código fuente. Puede crear un repositorio o usar uno existente que ya se encuentre en su cuenta. | `<your repository name>` |
| **Rama de colaboración** | La rama de colaboración de GitHub que se usa para la publicación. De forma predeterminada, es la principal. Cámbiela en caso de que desee publicar recursos de otra rama. | `<your collaboration branch>` |
| **Carpeta raíz** | La carpeta raíz de la rama de colaboración de GitHub. |`<your root folder name>` |
| **Import existing Data Factory resources to repository** (Importar recursos existentes de Data Factory en el repositorio). | Especifica si se deben importar los recursos existentes de la factoría de datos desde el lienzo de creación de la experiencia de usuario hasta un repositorio de GitHub. Active la casilla para importar los recursos de la factoría de datos en el repositorio Git asociado en formato JSON. Esta acción exporta cada recurso individualmente (es decir, los servicios vinculados y los conjuntos de datos se exportan a archivos JSON independientes). Cuando esta casilla no está activada, no se importan los recursos existentes. | Activada (valor predeterminado) |
| **Rama donde importar recursos** | Especifica en qué rama se importan los recursos de la factoría de datos (canalizaciones, conjuntos de datos, servicios vinculados etc.). Puede importar recursos en una de las siguientes ramas: a. Colaboración b. Crear nuevo c. Usar existente |  |

### <a name="github-organizations"></a>Organizaciones de GitHub

La conexión a una organización de GitHub requiere que la organización conceda permiso a Azure Data Factory. Un usuario con permisos de administrador en la organización debe realizar los pasos siguientes para permitir que se conecte la factoría de datos.

#### <a name="connecting-to-github-for-the-first-time-in-azure-data-factory"></a>Conexión a GitHub por primera vez en Azure Data Factory

Si se va a conectar a GitHub desde Azure Data Factory por primera vez, siga estos pasos para conectarse a una organización de GitHub.

1. En el panel de configuración de Git, escriba el nombre de la organización en el campo *Cuenta de GitHub*. Aparecerá una solicitud para iniciar sesión en GitHub. 
1. Inicie sesión con sus credenciales de usuario.
1. Se le pedirá que autorice a Azure Data Factory como una aplicación llamada *AzureDataFactory*. En esta pantalla, verá una opción para conceder permiso a ADF para tener acceso a la organización. Si no ve la opción para conceder el permiso, pida a un administrador que conceda el permiso manualmente a través de GitHub.

Una vez que siga estos pasos, su factoría podrá conectarse a repositorios públicos y privados dentro de la organización. Si no puede conectarse, pruebe a borrar la memoria caché del explorador y vuelva a intentarlo.

#### <a name="already-connected-to-github-using-a-personal-account"></a>Ya está conectado a GitHub con una cuenta personal

Si ya se ha conectado a GitHub y solo se le ha concedido permiso para obtener acceso a una cuenta personal, siga estos pasos para conceder permisos a una organización. 

1. Vaya a GitHub y abra **Configuración**.

    :::image type="content" source="media/author-visually/github-settings.png" alt-text="Apertura de la configuración de GitHub":::

1. Seleccione **Aplicaciones**. En la pestaña **Aplicaciones de OAuth autorizadas**, verá *AzureDataFactory*.

    :::image type="content" source="media/author-visually/github-organization-select-application.png" alt-text="Selección de aplicaciones de OAuth":::

1. Seleccione la aplicación y conceda a la aplicación acceso a la organización.

    :::image type="content" source="media/author-visually/github-organization-grant.png" alt-text="Conceder acceso":::

Una vez que siga estos pasos, su factoría podrá conectarse a repositorios públicos y privados dentro de la organización. 

### <a name="known-github-limitations"></a>Limitaciones conocidas de GitHub

- Puede almacenar archivos de datos y script en un repositorio de GitHub. Sin embargo, tiene que cargar manualmente los archivos en Azure Storage. Una canalización de Data Factory no carga automáticamente archivos de script o datos en un repositorio de GitHub en Azure Storage.

- GitHub Enterprise con una versión anterior a 2.14.0 no funciona en el explorador Microsoft Edge.

- La integración de GitHub con las herramientas de creación visual de Data Factory solo funciona con la versión de Data Factory disponible con carácter general.


- Se puede capturar un máximo de 1 000 entidades por tipo de recurso (por ejemplo, canalizaciones y conjuntos de valores) desde una sola rama de GitHub. Si se alcanza este límite, se recomienda dividir los recursos en factorías independientes. GIT de Azure DevOps no presenta esta limitación.

## <a name="version-control"></a>Control de versiones

Los sistemas de control de versiones, conocidos también como _de control de código fuente_, permiten a los desarrolladores colaborar en el código y llevar a cabo el seguimiento de los cambios realizados en la base de código. El control del código fuente es una herramienta esencial para proyectos de varios desarrolladores.

### <a name="creating-feature-branches"></a>Creación de ramas de características

Tan pronto como cada repositorio Git de Azure Repos se asocia a una factoría de datos, tiene una rama de colaboración. (`main` es la rama de colaboración predeterminada). Los usuarios también pueden crear ramas de características; para ello, pueden hacer clic en **+ New Branch** (+ Nueva rama) en la lista desplegable de ramas. 

:::image type="content" source="media/author-visually/new-branch.png" alt-text="Creación de una rama":::

Una vez que aparezca el panel de la nueva rama, escriba el nombre de la rama de características y seleccione una rama en la que se basará el trabajo.

:::image type="content" source="media/author-visually/create-branch-from-private-branch.png" alt-text="Captura de pantalla que muestra cómo crear una rama basada en la rama privada.":::

Cuando esté listo para combinar los cambios de su rama de características con la rama de colaboración, haga clic en la lista desplegable de ramas y seleccione **Create pull request** (Crear solicitud de incorporación de cambios). Esto le llevará al Git de Azure Repos, donde podrá generar la solicitud de incorporación de cambios, revisar el código y combinar los cambios en su rama de colaboración. (`main` es el valor predeterminado). Solo tiene permitido publicar en el servicio Data Factory de la rama de colaboración. 

:::image type="content" source="media/author-visually/create-pull-request.png" alt-text="Creación de una nueva solicitud de incorporación de cambios":::

### <a name="configure-publishing-settings"></a>Configurar los valores de publicación

De forma predeterminada, Data Factory genera las plantillas de Resource Manager de la factoría publicada y las guarda en una rama llamada `adf_publish`. Para configurar una rama de publicación personalizada, agregue un archivo `publish_config.json` a la carpeta raíz de la rama de colaboración. Durante la publicación, ADF lee este archivo, busca el campo `publishBranch` y guarda todas las plantillas de Resource Manager en la ubicación especificada. Si no existe la rama, Data Factory la crea automáticamente. A continuación, se muestra un ejemplo de cuál es el aspecto de este archivo:

```json
{
    "publishBranch": "factory/adf_publish"
}
```

Azure Data Factory solo puede tener una rama de publicación cada vez. Al especificar una nueva rama de publicación, Data Factory no elimina la rama de publicación anterior. Si quiere quitar la rama de publicación anterior, tiene que hacerlo manualmente.

> [!NOTE]
> Data Factory solo lee el archivo `publish_config.json` cuando se carga la factoría. Si ya tiene la factoría cargada en el portal, actualice el explorador para que los cambios surtan efecto.

### <a name="publish-code-changes"></a>Cambios de código publicando

Después de haber combinado los cambios en la rama de colaboración (`main` es el valor predeterminado), seleccione **Publish** (Publicar) para publicar manualmente los cambios de código realizados en la rama principal en el servicio Data Factory.

:::image type="content" source="media/author-visually/publish-changes.png" alt-text="Publicación de cambios en el servicio Data Factory":::

Se abrirá un panel lateral en el que se confirma que la rama de publicación y los cambios pendientes son correctos. Cuando haya comprobado los cambios, haga clic en **OK** (Aceptar) para confirmar la publicación.

:::image type="content" source="media/author-visually/configure-publish-branch.png" alt-text="Confirmar la rama de publicación correcta":::

> [!IMPORTANT]
> La rama principal no es representativa de lo que se implementa en el servicio Data Factory. La rama principal se *debe* publicar manualmente en el servicio Data Factory.



## <a name="best-practices-for-git-integration"></a>Procedimientos recomendados para la integración de Git

### <a name="permissions"></a>Permisos

Lo normal es que no quiera que todos los miembros del equipo tengan permisos para actualizar Data Factory. Se recomienda la siguiente configuración de permisos:

*   Todos los miembros del equipo deberían tener permisos de lectura en la instancia de Data Factory.
*   Solo se debe permitir que un conjunto seleccionado de personas publique en Data Factory. Para ello, deben tener el rol **colaborador de Data Factory** en el **grupo de recursos** que contiene la instancia de Data Factory. Para más información sobre los permisos, consulte [Roles y permisos para Azure Data Factory](concepts-roles-permissions.md).

Se recomienda no permitir inserciones directas en el repositorio en la rama de colaboración. Esta restricción puede ayudarle a evitar errores, ya que cada inserción en el repositorio pasará por un proceso de solicitud de incorporación de cambios, tal como se describe en [Crear una rama de características](source-control.md#creating-feature-branches).

### <a name="using-passwords-from-azure-key-vault"></a>Uso de contraseñas de Azure Key Vault

Se recomienda usar Azure Key Vault para almacenar las cadenas de conexión o las contraseñas para la autenticación de identidad administrada en los servicios vinculados de Data Factory. Por motivos de seguridad, Data Factory no almacena secretos en Git. Los cambios en los servicios vinculados que contienen secretos, como contraseñas, se publican inmediatamente en el servicio Azure Data Factory.

El uso de Key Vault o de la autenticación MSI también facilita la integración y la implementación continuas, ya que no tendrá que proporcionar estos secretos durante la implementación de plantillas de Resource Manager.

## <a name="troubleshooting-git-integration"></a>Solución de problemas de la integración de Git

### <a name="stale-publish-branch"></a>Rama de publicación obsoleta

A continuación, se muestran algunos ejemplos de situaciones que pueden provocar una rama de publicación obsoleta:

- Un usuario tiene varias ramas. En una rama de características, este eliminó un servicio vinculado que no está asociado a AKV (los servicios vinculados que no son AKV se publican inmediatamente, independientemente de si están en GIT o no) y nunca combinó la rama de características con la rama de colaboración.
- Un usuario modificó la factoría de datos mediante el SDK o PowerShell.
- Un usuario desplazó todos los recursos a una nueva rama e intentó publicarlos por primera vez. Los servicios vinculados se deben crear manualmente al importar los recursos.
- Un usuario carga un servicio vinculado que no es AKV o un archivo JSON de Integration Runtime manualmente. Hace referencia a ese recurso desde otro recurso, como un conjunto de datos, un servicio vinculado o una canalización. Un servicio vinculado que no es AKV creado través de la experiencia de usuario se publica inmediatamente porque se deben cifrar las credenciales. Si carga un conjunto de datos que haga referencia a ese servicio vinculado e intenta publicarlo, la experiencia de usuario lo permitirá porque existe en el entorno de GIT. Sin embargo, se rechazará en el momento de la publicación, ya que no existe en el servicio de la factoría de datos.

Si la rama de publicación no está sincronizada con la rama principal y contiene recursos no actualizados a pesar de una publicación reciente, puede usar alguna de las siguientes soluciones:

#### <a name="option-1-use-overwrite-live-mode-functionality"></a>Opción 1: uso de la funcionalidad **Sobrescribir modo real**

Publica o sobrescribe el código de la rama de colaboración en el modo real. Considerará el código del repositorio como el origen de la verdad. 

<u>*Flujo de código:*</u> ***Rama de colaboración -> Modo real***

:::image type="content" source="media/author-visually/force-publish-changes-from-collaboration-branch.png" alt-text="forzar la publicación de código desde la rama de colaboración":::

#### <a name="option-2-disconnect-and-reconnect-git-repository"></a>Opción 2: desconexión y reconexión del repositorio de Git

Importa el código del modo real a la rama de colaboración. Considera el código en modo real como origen de la verdad. 

<u>*Flujo de código:*</u> ***Modo real -> Rama de colaboración***  

1. Quite el repositorio de Git actual.
1. Vuelva a configurar Git con los mismos valores, pero asegúrese de que la opción **Import existing Data Factory resources to repository** (Importar recursos existentes de Data Factory en el repositorio) esté seleccionada y elija **New branch** (Nueva rama).
1. Cree una solicitud de incorporación de cambios para combinar los cambios con la rama de colaboración. 

Elija cualquiera de los métodos según sea necesario. 

## <a name="switch-to-a-different-git-repository"></a>Cambio a un repositorio de Git diferente

Para cambiar a un repositorio de Git diferente, vaya a la página de configuración de Git en el centro de administración en **Control de código fuente**. Seleccione **Desconectar**. 

:::image type="content" source="media/author-visually/remove-repository.png" alt-text="Icono de Git":::

Escriba el nombre de la factoría de datos y haga clic en **Confirm** (Confirmar) para quitar el repositorio de Git asociado a la factoría de datos.

:::image type="content" source="media/author-visually/remove-repository-2.png" alt-text="Elimine la asociación con el repositorio de Git actual.":::

Después de quitar la asociación con el repositorio actual, puede configurar las opciones de Git para usar un repositorio diferente y, luego, importar los recursos existentes de Data Factory en el nuevo repositorio.

> [!IMPORTANT]
> La eliminación de la configuración de Git de una factoría de datos no elimina nada del repositorio. La factoría contendrá todos los recursos publicados. Puede seguir editando la factoría directamente en el servicio.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la supervisión y la administración de canalizaciones, consulte el artículo [Supervisión y administración de canalizaciones mediante programación](monitor-programmatically.md).
* Para implementar la integración e implementación continuas, consulte [Integración y entrega continuas (CI/CD) en Azure Data Factory](continuous-integration-delivery.md).
