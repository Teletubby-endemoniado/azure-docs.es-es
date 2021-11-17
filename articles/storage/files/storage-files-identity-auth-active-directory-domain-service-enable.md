---
title: Uso de Azure AD Domain Services para autorizar el acceso a los datos de archivo a través de SMB
description: Aprenda a habilitar la autenticación basada en identidades a través del Bloque de mensajes del servidor (SMB) para Azure Files mediante Azure Active Directory Domain Services. Las máquinas virtuales Windows unidas a un dominio pueden acceder entonces a los recursos compartidos de archivos de Azure con las credenciales de Azure AD.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 07/22/2021
ms.author: rogarana
ms.subservice: files
ms.custom: contperf-fy21q1, devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 9823470a097035134f152002c35782aed2210afb
ms.sourcegitcommit: c434baa76153142256d17c3c51f04d902e29a92e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2021
ms.locfileid: "132180157"
---
# <a name="enable-azure-active-directory-domain-services-authentication-on-azure-files"></a>Habilitación de la autenticación de Azure Active Directory Domain Services en Azure Files

[Azure Files](storage-files-introduction.md) admite la autenticación basada en la identidad a través del protocolo Bloque de mensajes del servidor (SMB) mediante dos tipos de servicios de dominio: Active Directory Domain Services local (AD DS) y Azure Active Directory Domain Services (Azure AD DS). Le recomendamos encarecidamente que revise la sección [Funcionamiento](./storage-files-active-directory-overview.md#how-it-works) para seleccionar el servicio de dominio adecuado para la autenticación. La instalación es diferente en función del servicio de dominio que se elija. Este artículo se centra en la habilitación y configuración de Azure AD DS local para la autenticación con recursos compartidos de archivos de Azure.

Si no está familiarizado con los recursos compartidos de archivos de Azure, se recomienda leer la [guía de plan](storage-files-planning.md) antes de leer la siguiente serie de artículos.

> [!NOTE]
> Azure Files admite la autenticación de Kerberos con Azure AD DS mediante el cifrado RC4-HMAC y AES-256.
>
> Azure Files admite la autenticación de Azure AD DS con sincronización completa con Azure AD. Si se ha habilitado la sincronización de ámbito en Azure AD DS, que solo sincroniza un conjunto limitado de identidades de Azure AD, no se admite la autenticación ni la autorización.

## <a name="applies-to"></a>Se aplica a
| Tipo de recurso compartido de archivos | SMB | NFS |
|-|:-:|:-:|
| Recursos compartidos de archivos Estándar (GPv2), LRS/ZRS | ![Sí](../media/icons/yes-icon.png) | ![No](../media/icons/no-icon.png) |
| Recursos compartidos de archivos Estándar (GPv2), GRS/GZRS | ![Sí](../media/icons/yes-icon.png) | ![No](../media/icons/no-icon.png) |
| Recursos compartidos de archivos Premium (FileStorage), LRS/ZRS | ![Sí](../media/icons/yes-icon.png) | ![No](../media/icons/no-icon.png) |

## <a name="prerequisites"></a>Requisitos previos

Antes de habilitar Azure AD a través de SMB para recursos compartido de archivos para recursos compartido de archivos de Azure, asegúrese de haber completado los requisitos previos siguientes:

1.  **Seleccione o cree un inquilino de Azure AD.**

    Puede usar un inquilino nuevo o uno existente para la autenticación de Azure AD a través de SMB. El inquilino y el recurso compartido de archivos a los que desea acceder deben asociarse con la misma suscripción.

    Para crear un nuevo inquilino de Azure AD, puede [agregar un inquilino de Azure AD y una suscripción de Azure AD](/windows/client-management/mdm/add-an-azure-ad-tenant-and-azure-ad-subscription). Si ya tiene un inquilino de Azure AD, pero desea crear otro para usarlo con los recursos compartido de archivos de Azure, consulte [Creación de un inquilino de Azure Active Directory](/rest/api/datacatalog/create-an-azure-active-directory-tenant).

1.  **Habilite Azure AD Domain Services en el inquilino de Azure AD.**

    Para admitir la autenticación con las credenciales de Azure AD, debe habilitar Azure AD Domain Services para su inquilino de Azure AD. Si no es el administrador del inquilino de Azure AD, póngase en contacto con el administrador y siga las instrucciones paso a paso para [habilitar Azure Active Directory Domain Services mediante Azure Portal](../../active-directory-domain-services/tutorial-create-instance.md).

    Una implementación de Azure AD DS suele tardar unos 15 minutos en completarse. Confirme que el estado de mantenimiento de Azure AD muestra **En ejecución**, con la sincronización de hash de contraseña habilitada, antes de continuar con el paso siguiente.

1.  **Unión al dominio de una máquina virtual de Azure con Azure AD DS.**

    Para acceder a un recurso compartido de archivos mediante las credenciales de Azure AD desde una máquina virtual, esta debe unirse a un dominio con Azure AD DS. Para más información sobre cómo unir a un dominio una máquina virtual, consulte [Unión de una máquina virtual de Windows Server a un dominio administrado](../../active-directory-domain-services/join-windows-vm.md).

    > [!NOTE]
    > La autenticación de Azure AD DS a través de SMB con recursos compartido de archivos de Azure solo se admite en máquinas virtuales de Azure que se ejecutan en versiones del sistema operativo posteriores a Windows 7 o Windows Server 2008 R2.

1.  **Seleccione o cree un recurso compartido de archivos**.

    Seleccione un recurso compartido de archivos nuevo o existente que esté asociado con la misma suscripción que el inquilino de Azure AD. Para información acerca de cómo crear un nuevo recurso compartido de archivos, consulte [Creación de un recurso compartido de archivos en Azure Files](storage-how-to-create-file-share.md).
    Para obtener un rendimiento óptimo, se recomienda que el recurso compartido de archivos esté en la misma región que la máquina virtual desde la que vaya a acceder al recurso compartido.

1.  **Compruebe la conectividad de Azure Files; para ello, monte los recursos compartidos de archivos de Azure con la clave de su cuenta de almacenamiento.**

    Para comprobar que el recurso compartido de archivos y de la máquina virtual están configurados correctamente, intente montar el recurso compartido de archivos con la clave de la cuenta de almacenamiento. Para más información, consulte [Montaje de un recurso compartido de archivos de Azure y acceso al recurso compartido en Windows](storage-how-to-use-files-windows.md).

## <a name="regional-availability"></a>Disponibilidad regional

La autenticación de Azure Files con Azure AD DS está disponible en [todas las regiones públicas, de Azure Government y de China](https://azure.microsoft.com/global-infrastructure/locations/).

## <a name="overview-of-the-workflow"></a>Información general del flujo de trabajo

Antes de habilitar la autenticación de Azure AD DS a través de SMB para los recursos compartido de archivos de Files, compruebe que los entornos de Azure Storage y Azure AD se han configurado correctamente. Se recomienda que revise los [requisitos previos](#prerequisites) para asegurarse de que ha completado todos los pasos necesarios.

Luego, realice estas acciones para conceder acceso a los recursos de Azure Files con las credenciales de Azure AD:

1. Habilite la autenticación de Azure AD DS a través de SMB en la cuenta de almacenamiento a fin de registrarla con la implementación de Azure AD DS asociada.
2. Asigne permisos de acceso para un recurso compartido a una identidad de Azure AD (usuario, grupo o entidad de servicio).
3. Configure los permisos NTFS a través de SMB para archivos y directorios.
4. Monte un recurso compartido de archivos de Azure desde una máquina virtual unida al dominio.

En el diagrama siguiente se ilustra el flujo de trabajo de un extremo a otro para habilitar la autenticación de Azure AD DS a través de SMB para Azure Files.

![Diagrama que muestra Azure AD a través de SMB para el flujo de trabajo de Azure Files](media/storage-files-active-directory-enable/azure-active-directory-over-smb-workflow.png)

## <a name="enable-azure-ad-ds-authentication-for-your-account"></a>Habilitación de la autenticación de Azure AD DS para la cuenta

Para habilitar la autenticación de Azure AD DS a través de SMB para Azure Files, puede establecer una propiedad en las cuentas de almacenamiento mediante Azure Portal, Azure PowerShell o la CLI de Azure. Al establecer esta propiedad "une a un dominio" implícitamente la cuenta de almacenamiento con la implementación de Azure AD DS asociada. La autenticación de Azure AD DS a través de SMB se habilita entonces para todos los recursos compartidos de archivos nuevos y existentes de la cuenta de almacenamiento.

Tenga en cuenta que solo puede habilitar la autenticación de Azure AD DS a través de SMB después de haber implementado correctamente Azure AD DS en el inquilino de Azure AD. Para más información, consulte la sección los [requisitos previos](#prerequisites).

# <a name="portal"></a>[Portal](#tab/azure-portal)

Para habilitar la autenticación de Azure AD DS a través de SMB mediante [Azure Portal](https://portal.azure.com), siga estos pasos:

1. En Azure Portal, vaya a la cuenta de almacenamiento existente o [cree una](../common/storage-account-create.md).
1. En la sección **Recursos compartidos**, seleccione **Active Directory: No configurado**.

    :::image type="content" source="media/storage-files-active-directory-enable/files-azure-ad-enable-storage-account-identity.png" alt-text="Captura de pantalla del panel Recursos compartidos en su cuenta de almacenamiento, Active Directory aparece resaltado." lightbox="media/storage-files-active-directory-enable/files-azure-ad-enable-storage-account-identity.png":::

1. Seleccione **Azure Active Directory Domain Services** y cambie el botón de alternancia a **Habilitado**.
1. Seleccione **Guardar**.

    :::image type="content" source="media/storage-files-active-directory-enable/files-azure-ad-highlight.png" alt-text="Captura de pantalla del panel Active Directory en la que Azure Active Directory Domain Services está habilitado." lightbox="media/storage-files-active-directory-enable/files-azure-ad-highlight.png":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Para habilitar la autenticación de Azure AD DS a través de SMB con Azure PowerShell, instale el módulo Az más reciente (2.4 o posterior) o el módulo Az.Storage (1.5 o posterior). Para más información sobre cómo instalar PowerShell, consulte [Instalación de Azure PowerShell en Windows con PowerShellGet](/powershell/azure/install-Az-ps):

Para crear una cuenta de almacenamiento, llame a [New-AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount) y establezca el parámetro **EnableAzureActiveDirectoryDomainServicesForFile** en **true**. En el ejemplo siguiente, no olvide reemplazar los valores de marcador de posición por los suyos propios. (Si ha estado usando el módulo de versión preliminar anterior, el parámetro para habilitar la característica es **EnableAzureFilesAadIntegrationForSMB**).

```powershell
# Create a new storage account
New-AzStorageAccount -ResourceGroupName "<resource-group-name>" `
    -Name "<storage-account-name>" `
    -Location "<azure-region>" `
    -SkuName Standard_LRS `
    -Kind StorageV2 `
    -EnableAzureActiveDirectoryDomainServicesForFile $true
```

Para habilitar esta característica en cuentas de almacenamiento existentes, use el siguiente comando:

```powershell
# Update a storage account
Set-AzStorageAccount -ResourceGroupName "<resource-group-name>" `
    -Name "<storage-account-name>" `
    -EnableAzureActiveDirectoryDomainServicesForFile $true
```


# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para habilitar la autenticación de Azure AD mediante SMB con la CLI de Azure, instale la versión más reciente de la CLI (versión 2.0.70 o posterior). Para más información sobre cómo instalar la CLI de Azure, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

Para crear una cuenta de almacenamiento, llame a [az storage account create](/cli/azure/storage/account#az_storage_account_create) y establezca el argumento `--enable-files-aadds`. En el ejemplo siguiente, no olvide reemplazar los valores de marcador de posición por los suyos propios. (Si ha estado usando el módulo de versión preliminar anterior, el parámetro para la habilitación de características es **file-aad**).

```azurecli-interactive
# Create a new storage account
az storage account create -n <storage-account-name> -g <resource-group-name> --enable-files-aadds
```

Para habilitar esta característica en cuentas de almacenamiento existentes, use el siguiente comando:

```azurecli-interactive
# Update a new storage account
az storage account update -n <storage-account-name> -g <resource-group-name> --enable-files-aadds
```
---

[!INCLUDE [storage-files-aad-permissions-and-mounting](../../../includes/storage-files-aad-permissions-and-mounting.md)]

Ya ha habilitado correctamente la autenticación de Azure AD DS a través de SMB y ha asignado un rol personalizado que proporciona acceso a un recurso compartido de archivos de Azure con una identidad de Azure AD. Para conceder a otros usuarios acceso al recurso compartido de archivos, siga las instrucciones de las secciones [Asignación de permisos de acceso](#assign-access-permissions-to-an-identity) para usar una identidad y [Configuración de los permisos NTFS en secciones de SMB](#configure-ntfs-permissions-over-smb).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Files y el uso de Azure AD a través de SMB, consulte estos recursos:

- [Introducción a la compatibilidad de la autenticación basada en la identidad de Azure Files con el acceso SMB](storage-files-active-directory-overview.md)
- [P+F](storage-files-faq.md)
