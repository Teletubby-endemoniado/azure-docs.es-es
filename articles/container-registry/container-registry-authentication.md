---
title: Opciones de autenticación de registro
description: Opciones de autenticación de una instancia privada de Azure Container Registry, incluido el inicio de sesión con una identidad de Azure Active Directory, mediante entidades de servicio y credenciales de administrador opcionales.
ms.topic: article
ms.date: 06/16/2021
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 7c6510247ff6900145241834841964271db6530b
ms.sourcegitcommit: 96deccc7988fca3218378a92b3ab685a5123fb73
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131578971"
---
# <a name="authenticate-with-an-azure-container-registry"></a>Autenticación con un registro de contenedor de Azure

Hay varias maneras de autenticar con un Azure Container Registry que se pueden aplicar a uno o más escenarios de uso de registros.

Entre las formas recomendadas se incluyen las siguientes:

* La autenticación en un registro directamente mediante el [inicio de sesión individual](#individual-login-with-azure-ad)
* Las aplicaciones y los orquestadores de contenedores pueden realizar la autenticación desatendida o "sin notificación" mediante una [entidad de servicio](#service-principal) de Azure Active Directory (Azure AD)

Si usa un registro de contenedor con Azure Kubernetes Service (AKS) o con otro clúster de Kubernetes, consulte [Escenarios para autenticarse con Azure Container Registry desde Kubernetes](authenticate-kubernetes-options.md).

## <a name="authentication-options"></a>Opciones de autenticación

En la tabla siguiente se enumeran los métodos de autenticación disponibles y los escenarios típicos. Consulte el contenido vinculado para obtener información.

| Método                               | Cómo autenticarse                                           | Escenarios                                                            | Control de acceso basado en roles de Azure (RBAC de Azure)                             | Limitaciones                                |
|---------------------------------------|-------------------------------------------------------|---------------------------------------------------------------------|----------------------------------|--------------------------------------------|
| [Identidad de AD individual](#individual-login-with-azure-ad)                | `az acr login` en la CLI de Azure<br/><br/> `Connect-AzContainerRegistry` en Azure PowerShell                             | Inserción/extracción interactiva por parte de los desarrolladores y evaluadores                                    | Sí                              | El token de AD se debe renovar cada 3 horas.     |
| [Entidad de servicio de AD](#service-principal)                  | `docker login`<br/><br/>`az acr login` en la CLI de Azure<br/><br/> `Connect-AzContainerRegistry` en Azure PowerShell<br/><br/> Configuración de inicio de sesión del registro en API o herramientas<br/><br/> [Secreto de extracción de Kubernetes](container-registry-auth-kubernetes.md)                                           | Inserción desatendida desde la canalización de CI/CD<br/><br/> Extracción desatendida a Azure o a servicios externos  | Sí                              | La expiración predeterminada de la contraseña de SP es de 1 año.       |
| [Identidad administrada para recursos de Azure](container-registry-authentication-managed-identity.md)  | `docker login`<br/><br/> `az acr login` en la CLI de Azure<br/><br/> `Connect-AzContainerRegistry` en Azure PowerShell                                       | Inserción desatendida desde la canalización de CI/CD de Azure<br/><br/> Extracción desatendida a servicios de Azure<br/><br/>   | Sí                              | Uso solo desde servicios de Azure seleccionados que [admiten identidades administradas de recursos de Azure](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-managed-identities-for-azure-resources)              |
| [Identidad administrada por el clúster de AKS](../aks/cluster-container-registry-integration.md?toc=/azure/container-registry/toc.json&bc=/azure/container-registry/breadcrumb/toc.json)                    | Asocie un registro al crear o actualizar el clúster de AKS.  | Extracción desatendida en el clúster de AKS en la misma suscripción o en otra distinta                                                 | No, solo acceso de extracción             | Solo disponible con el clúster de AKS<br/><br/>No se puede usar para la autenticación entre inquilinos.            |
| [Entidad de servicio del clúster de AKS](authenticate-aks-cross-tenant.md)                    | Habilitación al crear o actualizar el clúster de AKS  | Extracción desatendida en el clúster de AKS desde el registro en otro inquilino de AD                                                  | No, solo acceso de extracción             | Solo disponible con el clúster de AKS            |
| [Usuario administrador](#admin-account)                            | `docker login`                                          | Inserción/extracción interactiva por parte de un desarrollador o evaluador<br/><br/>Implementación del portal de la imagen desde el registro a Azure App Service o Azure Container Instances                      | No, siempre acceso de extracción e inserción  | Una sola cuenta por registro; no se recomienda para varios usuarios.         |
| [Token de acceso con ámbito de repositorio](container-registry-repository-scoped-permissions.md)               | `docker login`<br/><br/>`az acr login` en la CLI de Azure<br/><br/> `Connect-AzContainerRegistry` en Azure PowerShell<br/><br/> [Secreto de extracción de Kubernetes](container-registry-auth-kubernetes.md)    | Inserción/extracción interactiva al repositorio por parte de un desarrollador o evaluador<br/><br/> Incorporación de cambios desatendida del repositorio mediante un sistema individual o dispositivo externo                  | Sí                              | No se integra actualmente con la identidad de AD  |

## <a name="individual-login-with-azure-ad"></a>Inicio de sesión individual con Azure AD

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Cuando trabaje con el registro directamente (por ejemplo, para realizar la extracción e inserción de imágenes en la estación de trabajo de desarrollo para un registro que ha creado), autentíquese con su identidad individual de Azure. Inicie sesión en la [CLI de Azure](/cli/azure/install-azure-cli) con [az login](/cli/azure/reference-index#az_login) y, luego, ejecute el comando [az acr login](/cli/azure/acr#az_acr_login):

```azurecli
az login
az acr login --name <acrName>
```

Cuando inicie sesión con `az acr login`, la CLI utiliza el token creado cuando ejecuta `az login` para autenticar sin problemas la sesión con su registro. Para completar el flujo de autenticación, la CLI y el demonio de Docker deben estar instalados y ejecutarse en el entorno. `az acr login` usa el cliente de Docker para establecer un token de Azure Active Directory en el archivo `docker.config`. Una vez que haya iniciado sesión de este modo, las credenciales se almacenan en caché y los comandos `docker` posteriores de la sesión no requieren un nombre de usuario o una contraseña.

> [!TIP]
> Use también `az acr login` para autenticar una identidad individual cuando quiera insertar o extraer artefactos que no sean imágenes de Docker en el registro, como [artefactos OCI](container-registry-oci-artifacts.md).

Para acceder al registro, el token utilizado por `az acr login` es válido durante **3 horas**, por lo que se recomienda que inicie siempre la sesión en el registro antes de ejecutar un comando `docker`. Si el token expira, puede actualizarlo con el comando `az acr login` de nuevo para volver a autenticar.

El uso de `az acr login` con identidades de Azure proporciona un [control de acceso basado en roles de Azure (RBAC)](../role-based-access-control/role-assignments-portal.md). En algunos escenarios, puede que quiera iniciar sesión en un registro con su propia identidad individual de Azure AD o configurar otros usuarios de Azure con [roles y permisos de Azure](container-registry-roles.md) específicos. También puede iniciar sesión con una [identidad administrada para recursos de Azure](container-registry-authentication-managed-identity.md) en los escenarios de varios servicios o a fin de satisfacer las necesidades de un grupo de trabajo o un flujo de trabajo de desarrollo donde no quiera administrar el acceso individual.

### <a name="az-acr-login-with---expose-token"></a>az acr login con --expose-token

En algunos casos, debe autenticarse con `az acr login` cuando el demonio de Docker no se ejecuta en su entorno. Por ejemplo, puede que necesite ejecutar `az acr login` en un script en Azure Cloud Shell, que proporciona la CLI de Docker, pero no ejecuta el demonio de Docker.

En este escenario, ejecute `az acr login` primero con el parámetro `--expose-token`. Esta opción expone un token de acceso en lugar de iniciar sesión a través de la CLI de Docker.

```azurecli
az acr login --name <acrName> --expose-token
```

La salida muestra el token de acceso, abreviado aquí:

```console
{
  "accessToken": "eyJhbGciOiJSUzI1NiIs[...]24V7wA",
  "loginServer": "myregistry.azurecr.io"
}
```
Para la autenticación de registro, se recomienda almacenar la credencial de token en una ubicación segura y seguir las prácticas recomendadas para administrar las credenciales de [inicio de sesión de Docker](https://docs.docker.com/engine/reference/commandline/login/). Por ejemplo, almacene el valor del token en una variable de entorno:

```bash
TOKEN=$(az acr login --name <acrName> --expose-token --output tsv --query accessToken)
```

A continuación, ejecute `docker login`, pasando `00000000-0000-0000-0000-000000000000` como nombre de usuario y usando el token de acceso como contraseña:

```console
docker login myregistry.azurecr.io --username 00000000-0000-0000-0000-000000000000 --password $TOKEN
```
Del mismo modo, puede usar el token que ha devuelto `az acr login` mediante el comando `helm registry login` para autenticarse con el registro:

```console
echo $TOKEN | helm registry login myregistry.azurecr.io \
            --username 00000000-0000-0000-0000-000000000000 \
            --password-stdin
```

### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

Cuando trabaje con el registro directamente (por ejemplo, para realizar la extracción e inserción de imágenes en la estación de trabajo de desarrollo para un registro que ha creado), autentíquese con su identidad individual de Azure. Inicie sesión en [Azure PowerShell](/powershell/azure/uninstall-az-ps) con [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) y, a continuación, ejecute el cmdlet [Connect-AzContainerRegistry](/powershell/module/az.containerregistry/connect-azcontainerregistry):

```azurepowershell
Connect-AzAccount
Connect-AzContainerRegistry -Name <acrName>
```

Cuando inicie sesión con `Connect-AzContainerRegistry`, PowerShell utiliza el token creado cuando ejecutó `Connect-AzAccount` para autenticar sin problemas la sesión con su registro. Para completar el flujo de autenticación, la CLI y el demonio de Docker deben estar instalados y ejecutarse en el entorno. `Connect-AzContainerRegistry` usa el cliente de Docker para establecer un token de Azure Active Directory en el archivo `docker.config`. Una vez que haya iniciado sesión de este modo, las credenciales se almacenan en caché y los comandos `docker` posteriores de la sesión no requieren un nombre de usuario o una contraseña.

> [!TIP]
> Use también `Connect-AzContainerRegistry` para autenticar una identidad individual cuando quiera insertar o extraer artefactos que no sean imágenes de Docker en el registro, como [artefactos OCI](container-registry-oci-artifacts.md).

Para acceder al registro, el token utilizado por `Connect-AzContainerRegistry` es válido durante **3 horas**, por lo que se recomienda que inicie siempre la sesión en el registro antes de ejecutar un comando `docker`. Si el token expira, puede actualizarlo con el comando `Connect-AzContainerRegistry` de nuevo para volver a autenticar.

El uso de `Connect-AzContainerRegistry` con identidades de Azure proporciona un [control de acceso basado en roles de Azure (RBAC)](../role-based-access-control/role-assignments-portal.md). En algunos escenarios, puede que quiera iniciar sesión en un registro con su propia identidad individual de Azure AD o configurar otros usuarios de Azure con [roles y permisos de Azure](container-registry-roles.md) específicos. También puede iniciar sesión con una [identidad administrada para recursos de Azure](container-registry-authentication-managed-identity.md) en los escenarios de varios servicios o a fin de satisfacer las necesidades de un grupo de trabajo o un flujo de trabajo de desarrollo donde no quiera administrar el acceso individual.

---

## <a name="service-principal"></a>Entidad de servicio

Si asigna una [entidad de servicio](../active-directory/develop/app-objects-and-service-principals.md) en el Registro, la aplicación o el servicio pueden usarla para la autenticación desatendida. Las entidades de servicio permiten el [control de acceso basado en roles (RBAC de Azure)](../role-based-access-control/role-assignments-portal.md) en un registro, y puede asignar varias entidades de seguridad de servicio a un registro. Las distintas entidades de servicio le permiten definir un acceso diferente para distintas aplicaciones.

Los roles disponibles para un Registro de contenedor incluyen:

* **AcrPull**: incorporación de cambios

* **AcrPush**: incorporación y envío de cambios

* **Propietario**: extracción, inserción y asignación de roles a otros usuarios

Para obtener una lista completa de roles, consulte [Roles y permisos de Azure Container Registry](container-registry-roles.md).

Para que los scripts de la CLI creen una entidad de servicio para la autenticación con Azure Container Registry y obtener más instrucciones, consulte [Autenticación de Azure Container Registry con entidades de servicio](container-registry-auth-service-principal.md).

## <a name="admin-account"></a>Cuenta de administrador

Cada registro de contenedor incluye una cuenta de usuario administrador, que está deshabilitada de forma predeterminada. Puede habilitar el usuario administrador y administrar sus credenciales en Azure Portal, mediante la CLI de Azure, Azure PowerShell o con otras herramientas de Azure. La cuenta de administrador tiene todos los permisos para el registro.

La cuenta de administrador se necesita actualmente en algunos escenarios para implementar una imagen de un registro de contenedor en determinados servicios de Azure. Por ejemplo, la cuenta de administrador es necesaria cuando se usa Azure Portal para implementar una imagen de contenedor desde un registro directamente en [Azure Container Instances](../container-instances/container-instances-using-azure-container-registry.md#deploy-with-azure-portal) o [Azure Web App for Containers](container-registry-tutorial-deploy-app.md).

> [!IMPORTANT]
> La cuenta de administrador está diseñada para que un solo usuario acceda al registro, principalmente con fines de prueba. No se recomienda compartir las credenciales de cuenta de administrador entre varios usuarios. Todos los usuarios que se autentican con la cuenta de administrador aparecen como un único usuario con acceso de inserción y extracción en el registro. Al cambiar o deshabilitar esta cuenta se deshabilita el acceso de registro para todos los usuarios que utilizan sus credenciales. Se recomienda la identidad individual para usuarios y entidades de servicio para escenarios desatendidos.
>

A la cuenta de administrador se le proporcionan dos contraseñas, y las dos se pueden regenerar. Las dos contraseñas le permiten mantener la conexión con el registro mediante una contraseña mientras se regenera la otra. Si la cuenta de administrador está habilitada, puede pasar el nombre de usuario y cualquier contraseña al comando `docker login` cuando se le solicite para la autenticación básica en el registro. Por ejemplo:

```
docker login myregistry.azurecr.io
```

Si quiere ver los procedimientos recomendados para administrar credenciales de inicio de sesión, consulte la referencia del comando [docker login](https://docs.docker.com/engine/reference/commandline/login/).

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para habilitar el usuario administrador para un registro existente, puede usar el parámetro `--admin-enabled` del comando [az acr update](/cli/azure/acr#az_acr_update) en la CLI de Azure:

```azurecli
az acr update -n <acrName> --admin-enabled true
```

### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

Para habilitar el usuario administrador para un registro existente, puede usar el parámetro `EnableAdminUser` del comando [Update-AzContainerRegistry](/powershell/module/az.containerregistry/update-azcontainerregistry) en Azure PowerShell:

```azurepowershell
Update-AzContainerRegistry -Name <acrName> -ResourceGroupName myResourceGroup -EnableAdminUser
```

---

Puede habilitar el usuario administrador en Azure Portal dirigiéndose al registro, seleccionando **Claves de acceso** en **Configuración** y **Habilitar** en **Usuario administrador**.

![Activación de la interfaz de usuario administrador en Azure Portal][auth-portal-01]

## <a name="next-steps"></a>Pasos siguientes

* [Push your first image using the Azure CLI](container-registry-get-started-azure-cli.md) (Inserción de la primera imagen mediante la CLI de Azure)

* [Inicio rápido: Creación de un registro de contenedor privado con Azure PowerShell](container-registry-get-started-powershell.md)

<!-- IMAGES -->
[auth-portal-01]: ./media/container-registry-authentication/auth-portal-01.png
