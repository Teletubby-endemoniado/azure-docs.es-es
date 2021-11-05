---
title: Aplicación de la directiva de nomenclatura de grupos en Azure Active Directory | Microsoft Docs
description: Configuración de una directiva de nomenclatura para grupos de Microsoft 365 en Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: KarenH444
ms.service: active-directory
ms.subservice: enterprise-users
ms.workload: identity
ms.topic: how-to
ms.date: 09/02/2021
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5a0cd6b33caea0cd8f9a46a2f967a1291c5c94ce
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131052643"
---
# <a name="enforce-a-naming-policy-on-microsoft-365-groups-in-azure-active-directory"></a>Aplicación de una directiva de nomenclatura en los grupos de Microsoft 365 en Azure Active Directory

Para aplicar convenciones de nomenclatura coherentes a los grupos de Microsoft 365 creados o editados por los usuarios, configure una directiva de nomenclatura de grupos para sus organizaciones en Azure Active Directory (Azure AD). Por ejemplo, podría usar la directiva de nomenclatura para comunicar la función de un grupo, la pertenencia, la región geográfica o quién creó el grupo. También puede usarla para ayudar a clasificar los grupos en la libreta de direcciones. O, para impedir que palabras específicas se usen en los nombres y alias de grupo.

> [!IMPORTANT]
> Para usar la directiva de nomenclatura de Azure AD para los grupos de Microsoft 365, es necesario que posea una licencia P1 de Azure Active Directory Premium o una licencia EDU de Azure AD Basic para cada usuario que sea miembro de uno o varios grupos de Microsoft 365.

La directiva de nomenclatura se aplica a la creación o edición de grupos creados en cargas de trabajo (por ejemplo, Outlook, Microsoft Teams, SharePoint, Exchange o Planner), incluso si no se realizan cambios de edición. Se aplica tanto al nombre como al alias del grupo. Si configuró su directiva de nomenclatura en Azure AD y tiene una directiva de nomenclatura de grupos de Exchange existente, se aplica la directiva de nomenclatura de Azure AD.

Cuando se configure la directiva de nomenclatura de grupos, se aplicará a los nuevos grupos de Microsoft 365 creados por los usuarios finales. La directiva de nomenclatura no se aplica a ciertos roles de directorio, como Administrador global o Administrador de usuarios (consulte a continuación la lista completa de los roles exentos de la directiva de nomenclatura de grupos). En el caso de los grupos existentes de Microsoft 365, la directiva no se aplicará de inmediato en el momento de la configuración. Una vez que el propietario del grupo edite el nombre de estos grupos, se aplicará la directiva de nomenclatura, incluso si no realizan cambios.

## <a name="naming-policy-features"></a>Características de la directiva de nomenclatura

Puede aplicar la directiva de nomenclatura a grupos de dos maneras diferentes:

- **Directiva de nomenclatura de prefijo-sufijo**: puede definir prefijos o sufijos que se agregan automáticamente para aplicar una convención de nomenclatura a los grupos (por ejemplo, en el nombre de grupo "GRP\_JAPAN\_My Group\_Engineering", GRP\_JAPAN\_ es el prefijo y \_Engineering es el sufijo). 

- **Palabras bloqueadas personalizadas**: puede cargar un conjunto de palabras bloqueadas específicas de su organización para que se bloqueen en los grupos creados por los usuarios (por ejemplo, "CEO, Nómina, RRHH").

### <a name="prefix-suffix-naming-policy"></a>Directiva de nomenclatura de prefijo-sufijo

La estructura general de la convención de nomenclatura es "Prefijo[NombreDeGrupo]Sufijo". Aunque puede definir varios prefijos y sufijos, solo puede tener una instancia del [NombreDeGrupo] en la configuración. Los prefijos o sufijos pueden ser cadenas fijas o atributos de usuario como \[Department\], que se sustituyen en función del usuario que crea el grupo. El número total de caracteres permitidos para las cadenas de prefijo y sufijo, incluido el nombre del grupo, es de 53.

Los prefijos y sufijos pueden contener caracteres especiales que se admiten en el nombre y el alias del grupo. Los caracteres de prefijo o sufijo que no se admiten en el alias de grupo se siguen aplicando al nombre de grupo, pero se quitan del alias de grupo. Debido a esta restricción, los prefijos y sufijos aplicados al nombre de grupo pueden ser diferentes de los que se aplican al alias de grupo.

#### <a name="fixed-strings"></a>Cadenas fijas

Puede usar cadenas para que sea más fácil examinar y diferenciar grupos en la lista global de direcciones y en los vínculos de navegación izquierdos de las cargas de trabajo de grupo. Algunos de los prefijos comunes son palabras clave como "Grp\_Name", "\#Name" y "\_Name".

#### <a name="user-attributes"></a>Atributos de usuario

Puede usar atributos como ayuda para identificar para qué departamento, oficina o región geográfica se creó el grupo. Por ejemplo, si define la directiva de nomenclatura como `PrefixSuffixNamingRequirement = "GRP [GroupName] [Department]"` y `User’s department = Engineering`, un nombre de grupo aplicado podría ser "GRP My Group Engineering". Atributos de Azure AD admitidos son \[Department\], \[Company\], \[Office\], \[StateOrProvince\], \[CountryOrRegion\], \[Title\]. Los atributos de usuario no admitido se tratan como cadenas fijas; por ejemplo, "\[postalCode\]". No se admiten atributos de extensión ni atributos personalizados.

Se recomienda usar atributos con valores ya rellenados para todos los usuarios de la organización y no usar atributos con valores largos.

### <a name="custom-blocked-words"></a>Palabras bloqueadas personalizadas

Una lista de palabras bloqueadas es una lista separada por comas de frases que se bloquearán en los nombres y alias de grupo. No se realiza ninguna búsqueda de subcadenas. Para desencadenar un error se requiere una coincidencia exacta entre el nombre de grupo y una o varias de las palabras bloqueadas personalizadas. Como no se realiza la búsqueda de subcadenas, los usuarios pueden usar palabras comunes como "Class" aunque "lass" sea una palabra bloqueada.

Reglas de la lista de palabras bloqueadas:

- Las palabras bloqueadas no distinguen mayúsculas de minúsculas.
- Cuando un usuario escribe una palabra bloqueada que forma parte de un nombre de grupo, verá un mensaje de error con la palabra bloqueada.
- No hay ninguna restricción de caracteres en las palabras bloqueadas.
- Hay un límite de 5000 frases que se pueden configurar en la lista de palabras bloqueadas. 

### <a name="roles-and-permissions"></a>Roles y permisos

Para configurar la directiva de nomenclatura, se necesita uno de los roles siguientes:

- Administrador global
- Administrador del grupo
- Escritor de directorios

Algunos roles de administradores pueden quedar excluidos de estas directivas, en todas las cargas de trabajo de grupo y puntos de conexión, de forma que pueden crear grupos mediante palabras bloqueadas y con sus propias convenciones de nomenclatura. A continuación se muestra la lista de roles de administrador excluidos de la directiva de nomenclatura de grupos:

- Administrador global
- Administrador de usuarios

## <a name="configure-naming-policy-in-azure-portal"></a>Configuración de la directiva de nomenclatura en Azure Portal

1. Inicie sesión en el [Centro de administración de Azure AD](https://aad.portal.azure.com) con una cuenta de Administrador del grupo.
1. Seleccione **Grupos**, a continuación, seleccione **Directiva de nomenclatura** para abrir la página de la directiva de nomenclatura.

    ![apertura de la página de la directiva de nomenclatura en el centro de administración](./media/groups-naming-policy/policy.png)

### <a name="view-or-edit-the-prefix-suffix-naming-policy"></a>Visualización o edición de la directiva de nomenclatura de prefijo-sufijo

1. En la página **Directiva de nomenclatura**, seleccione **Directiva de nomenclatura de grupos**.
1. Puede ver o editar las directivas de nomenclatura de prefijo o sufijo actuales individualmente seleccionando los atributos o cadenas que desea aplicar como parte de la directiva de nomenclatura.
1. Para quitar un prefijo o sufijo de la lista, seleccione el prefijo o sufijo y luego seleccione **Eliminar**. Se pueden eliminar varios elementos al mismo tiempo.
1. Guarde los cambios para que la nueva directiva entre en vigor; para ello, seleccione **Guardar**.

### <a name="edit-custom-blocked-words"></a>Edición de las palabras bloqueadas personalizadas

1. En la página **Directiva de nomenclatura**, seleccione **Palabras bloqueadas**.

    ![edición y carga de la lista de palabras bloqueadas para la directiva de nomenclatura](./media/groups-naming-policy/blockedwords.png)

1. Vea o edite la lista actual de palabras bloqueadas personalizadas seleccionando **Descargar**. Se deben agregar nuevas entradas a las entradas existentes.
1. Cargue la nueva lista de palabras bloqueadas personalizadas seleccionando el icono del archivo.
1. Guarde los cambios para que la nueva directiva entre en vigor; para ello, seleccione **Guardar**.

## <a name="install-powershell-cmdlets"></a>Instalación de los cmdlets de PowerShell

Asegúrese de desinstalar cualquier versión anterior del módulo Azure Active Directory PowerShell for Graph para Windows PowerShell y de instalar [Azure Active Directory PowerShell for Graph - Public Preview Release 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) antes de ejecutar los comandos de PowerShell.

1. Abra la aplicación Windows PowerShell como administrador.
2. Desinstale cualquier versión anterior de AzureADPreview.
  
   ``` PowerShell
   Uninstall-Module AzureADPreview
   ```

3. Instale la versión más reciente de AzureADPreview.
  
   ``` PowerShell
   Install-Module AzureADPreview
   ```

   Si se le pregunta sobre si desea acceder a un repositorio en el que no se confía, escriba **Y** (S). El nuevo módulo puede tardar varios minutos en instalarse.

## <a name="configure-naming-policy-in-powershell"></a>Configuración de la directiva de nomenclatura en PowerShell

1. Abra una ventana de Windows PowerShell en el equipo. La puede abrir sin privilegios elevados.

1. Ejecute los comandos siguientes para prepararse para ejecutar los cmdlets.
  
   ``` PowerShell
   Import-Module AzureADPreview
   Connect-AzureAD
   ```

   En la pantalla **Sign in to your Account** (Iniciar sesión en su cuenta) que se abre, escriba la cuenta y la contraseña de administrador para conectarse al servicio y seleccione **Sign in** (Iniciar sesión).

1. Siga los pasos de [Cmdlets de Azure Active Directory para configurar las opciones de grupo](../enterprise-users/groups-settings-cmdlets.md) si desea crear una configuración de grupo para esta organización.

### <a name="view-the-current-settings"></a>Visualización de la configuración actual

1. Obtenga la directiva de nomenclatura actual para ver la configuración actual.
  
   ``` PowerShell
   $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
   ```
  
1. Muestre la configuración actual del grupo.
  
   ``` PowerShell
   $Setting.Values
   ```
  
### <a name="set-the-naming-policy-and-custom-blocked-words"></a>Establecimiento de la directiva de nomenclatura y de las palabras bloqueadas personalizadas

1. Establezca los prefijos y sufijos de nombre de grupo en Azure AD PowerShell. Para que la característica funcione correctamente, [nombre de grupo] debe estar incluido en la configuración.
  
   ``` PowerShell
   $Setting["PrefixSuffixNamingRequirement"] =“GRP_[GroupName]_[Department]"
   ```
  
1. Establezca las palabras bloqueadas personalizadas que quiere restringir. En el ejemplo siguiente se muestra cómo puede agregar sus propias palabras personalizadas.
  
   ``` PowerShell
   $Setting["CustomBlockedWordsList"]=“Payroll,CEO,HR"
   ```
  
1. Guarde la configuración de la nueva directiva para que se aplique, como en el ejemplo siguiente.
  
   ``` PowerShell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```
  
Eso es todo. Ha establecido la directiva de nomenclatura y ha agregado sus palabras bloqueadas.

## <a name="export-or-import-custom-blocked-words"></a>Exportación o importación de la lista de palabras bloqueadas personalizadas

Para más información, consulte el artículo [Cmdlets de Azure Active Directory para configurar las opciones de grupo](../enterprise-users/groups-settings-cmdlets.md).

Este es un script de PowerShell de ejemplo para exportar varias palabras bloqueadas:

``` PowerShell
$Words = (Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value CustomBlockedWordsList -EQ 
Add-Content "c:\work\currentblockedwordslist.txt" -Value $words.value.Split(",").Replace("`"","")  
```

Este es un script de PowerShell de ejemplo para importar varias palabras bloqueadas:

``` PowerShell
$BadWords = Get-Content "C:\work\currentblockedwordslist.txt"
$BadWords = [string]::join(",", $BadWords)
$Settings = Get-AzureADDirectorySetting | Where-Object {$_.DisplayName -eq "Group.Unified"}
if ($Settings.Count -eq 0)
    {$Template = Get-AzureADDirectorySettingTemplate | Where-Object {$_.DisplayName -eq "Group.Unified"}
    $Settings = $Template.CreateDirectorySetting()
    New-AzureADDirectorySetting -DirectorySetting $Settings
    $Settings = Get-AzureADDirectorySetting | Where-Object {$_.DisplayName -eq "Group.Unified"}}
$Settings["CustomBlockedWordsList"] = $BadWords
Set-AzureADDirectorySetting -Id $Settings.Id -DirectorySetting $Settings 
```

## <a name="remove-the-naming-policy"></a>Elimine la directiva de nomenclatura

### <a name="remove-the-naming-policy-using-azure-portal"></a>Eliminación de la directiva de nomenclatura mediante Azure Portal

1. En la página **Directiva de nomenclatura**, seleccione **Eliminar directiva**.
1. Después de confirmar la eliminación, se quita la directiva de nomenclatura, incluida toda la directiva de nomenclatura de prefijos y sufijos y cualquier palabra bloqueada personalizada.

### <a name="remove-the-naming-policy-using-azure-ad-powershell"></a>Eliminación de la directiva de nomenclatura mediante Azure AD PowerShell

1. Vacíe los prefijos y sufijos de nombre de grupo en Azure AD PowerShell.
  
   ``` PowerShell
   $Setting["PrefixSuffixNamingRequirement"] =""
   ```
  
1. Vacíe las palabras bloqueadas personalizadas.
  
   ``` PowerShell
   $Setting["CustomBlockedWordsList"]=""
   ```
  
1. Guarde la configuración
  
   ``` PowerShell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```

## <a name="experience-across-microsoft-365-apps"></a>Experiencia entre aplicaciones de Microsoft 365

Después de establecer una directiva de nomenclatura de grupo en Azure AD, cuando un usuario crea un grupo en una aplicación de Microsoft 365, puede ver:

- Una vista previa del nombre de acuerdo con la directiva de nomenclatura (con prefijos y sufijos) tan pronto como el usuario escribe el nombre del grupo
- Si el usuario escribe palabras bloqueadas, verá un mensaje de error para que pueda quitar las palabras bloqueadas.

Carga de trabajo | Cumplimiento normativo
----------- | -------------------------------
Portales de Azure Active Directory | El portal de Azure AD y el portal Panel de acceso muestran la directiva de nomenclatura aplicada cuando el usuario escribe un nombre de grupo al crear o editar un grupo. Cuando un usuario escribe una palabra bloqueada personalizada, aparece un mensaje de error con la palabra bloqueada para que el usuario pueda quitarla.
Outlook Web Access (OWA) | Outlook Web Access muestra el nombre aplicado por la directiva de nomenclatura cuando el usuario escribe un nombre o alias de grupo. Cuando un usuario escribe una palabra bloqueada personalizada, se muestra un mensaje de error en la interfaz de usuario junto con la palabra bloqueada para que el usuario pueda quitarla.
Escritorio de Outlook | Los grupos creados en el escritorio de Outlook son compatibles con la configuración de directivas de nomenclatura. La aplicación de escritorio de Outlook no muestra aún la vista previa del nombre de grupo aplicado y no devuelve errores de palabras bloqueadas personalizadas cuando el usuario escribe el nombre del grupo. Sin embargo, la directiva de nomenclatura se aplica automáticamente al crear o editar un grupo, y los usuarios ven mensajes de error si hay palabras bloqueadas personalizadas en el nombre o alias del grupo.
Equipos de Microsoft | Microsoft Teams muestra el nombre aplicado por la directiva de nomenclatura de grupo cuando el usuario escribe un nombre de equipo. Cuando un usuario escribe una palabra bloqueada personalizada, se muestra un mensaje de error junto con la palabra bloqueada para que el usuario pueda quitarla.
SharePoint | SharePoint muestra el nombre aplicado por la directiva de nomenclatura cuando el usuario escribe el nombre de un sitio o la dirección de correo electrónico de un grupo. Cuando un usuario escribe una palabra bloqueada personalizada, se muestra un mensaje de error junto con la palabra bloqueada para que el usuario pueda quitarla.
Microsoft Stream | Microsoft Stream muestra el nombre aplicado por la directiva de nomenclatura de grupos cuando el usuario escribe el nombre de un grupo o el alias de la dirección de correo electrónico del grupo. Cuando un usuario escribe una palabra bloqueada personalizada, se muestra un mensaje de error con la palabra bloqueada para que el usuario pueda quitarla.
Aplicación de Outlook iOS y Android | Los grupos creados en las aplicaciones de Outlook son compatibles con la directiva de nomenclatura configurada. La aplicación móvil de Outlook no muestra aún la vista previa del nombre aplicado por la directiva de nomenclatura y no devuelve errores de palabras bloqueadas personalizadas cuando el usuario escribe el nombre del grupo. Sin embargo, la directiva de nomenclatura se aplica automáticamente al hacer clic para crear o editar, y los usuarios ven mensajes de error si hay palabras bloqueadas personalizadas en el nombre o alias del grupo.
Aplicación móvil Grupos | Los grupos creados en la aplicación móvil Grupos son compatibles con la directiva de nomenclatura. Esta aplicación no muestra la vista previa de la directiva de nomenclatura y no devuelve errores de palabras bloqueados personalizadas cuando el usuario escribe el nombre del grupo. Sin embargo, la directiva de nomenclatura se aplica automáticamente al crear o editar un grupo, y los usuarios ven los errores correspondientes si hay palabras bloqueadas personalizadas en el nombre o alias del grupo.
Planner | Planner es compatible con la directiva de nomenclatura. Planner muestra la versión preliminar de la directiva de nomenclatura al escribir el nombre del plan. Cuando un usuario escribe una palabra bloqueada personalizada, se muestra un mensaje de error al crear el plan.
Dynamics 365 for Customer Engagement | Dynamics 365 for Customer Engagement es compatible con la directiva de nomenclatura. Dynamics 365 muestra el nombre aplicado por la directiva de nomenclatura cuando el usuario escribe el nombre o el alias de correo electrónico de un grupo. Cuando el usuario escribe una palabra bloqueada personalizada, se muestra un mensaje de error con la palabra bloqueada para que el usuario pueda quitarla.
School Data Sync (SDS) | Los grupos creados mediante SDS cumplen la directiva de nomenclatura, pero esta no se aplica automáticamente. Los administradores de SDS tienen que anexar los prefijos y sufijos a los nombres de clase para los que es necesario crear grupos y luego cargarlos en SDS. De lo contrario, la creación o edición de los grupos produciría error.
Aplicación Aula | Los grupos creados en la aplicación Aula cumplen la directiva de nomenclatura, pero esta no se aplica automáticamente y los usuarios no tienen una vista previa de ella al escribir el nombre de un grupo del aula. Los usuarios deben escribir el nombre de grupo del aula aplicado con prefijos y sufijos. Si no, las operaciones para crear o editar el grupo del aula producirán error.
Power BI | Las áreas de trabajo de Power BI son compatibles con la directiva de nomenclatura.
Yammer | Cuando un usuario que ha iniciado sesión en Yammer con su cuenta de Azure Active Directory crea un grupo o edita un nombre de grupo, el nombre del grupo cumplirá la directiva de nomenclatura. Esto se aplica tanto a los grupos conectados de Microsoft 365 como a todos los demás grupos de Yammer.<br>Si se ha creado un grupo conectado de Microsoft 365 antes de aplicar la directiva de nomenclatura, el nombre del grupo no seguirá automáticamente las directivas de nomenclatura. Cuando un usuario edita el nombre del grupo, se le pedirá que agregue el prefijo y el sufijo.
StaffHub  | Los equipos de StaffHub no siguen la directiva de nomenclatura, pero el grupo de Microsoft 365 subyacente sí lo hace. El nombre de equipo de StaffHub no aplica los prefijos y sufijos y no busca palabras bloqueadas personalizadas. Sin embargo, StaffHub aplica los prefijos y sufijos y quita las palabras bloqueadas del grupo de Microsoft 365 subyacente.
Exchange PowerShell | Los cmdlets de Exchange PowerShell son compatibles con la directiva de nomenclatura. Los usuarios reciben los correspondientes mensajes de error con sugerencias de prefijos y sufijos y con las palabras bloqueadas personalizadas si no siguen la directiva de nomenclatura en el nombre o alias del grupo (mailNickname).
Cmdlets de PowerShell para Azure Active Directory | Los cmdlets de PowerShell para Azure Active Directory son compatibles con la directiva de nomenclatura. Los usuarios reciben los correspondientes mensajes de error con sugerencias de prefijos y sufijos y con las palabras bloqueadas personalizadas si no siguen la convención de nomenclatura tanto en los nombres de grupo como en los alias de grupo.
Centro de administración de Exchange | El centro de administración de Exchange es compatible con la directiva de nomenclatura. Los usuarios reciben los correspondientes mensajes de error con sugerencias de prefijos y sufijos y con las palabras bloqueadas personalizadas si no siguen la convención de nomenclatura en el nombre o alias del grupo.
Centro de administración de Microsoft 365 | El centro de administración de Microsoft 365 es compatible con la directiva de nomenclatura. Cuando un usuario crea o edita nombres de grupo, la directiva de nomenclatura se aplica automáticamente y los usuarios reciben los correspondientes errores al escribir palabras bloqueadas personalizadas. El centro de administración de Microsoft 365 no muestra aún una versión preliminar de la directiva de nomenclatura y no devuelve errores de palabras bloqueadas personalizadas cuando el usuario escribe el nombre del grupo.

## <a name="next-steps"></a>Pasos siguientes

En estos artículos se proporciona información adicional sobre los grupos de Azure AD.

- [Consulta de los grupos existentes](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Directiva de caducidad para grupos de Microsoft 365](groups-lifecycle.md)
- [Administración de la configuración de un grupo](../fundamentals/active-directory-groups-settings-azure-portal.md)
- [Administrar miembros de un grupo](../fundamentals/active-directory-groups-members-azure-portal.md)
- [Administrar la pertenencia a grupos](../fundamentals/active-directory-groups-membership-azure-portal.md)
- [Administrar reglas dinámicas de los usuarios de un grupo](groups-dynamic-membership.md)
