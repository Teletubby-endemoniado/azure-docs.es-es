---
title: Administración de dispositivos con Azure Portal | Microsoft Docs
description: Aprenda a usar Azure Portal para administrar dispositivos.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: how-to
ms.date: 03/23/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: hafowler
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ef9dca3604be281ccedf4d6f1af3e7861273e6b
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128590729"
---
# <a name="manage-device-identities-using-the-azure-portal"></a>Administración de identidades de dispositivos con Azure Portal

Azure AD proporciona una ubicación central para administrar las identidades de los dispositivos.

La página **Todos los dispositivos** le permite:

- Identificar los dispositivos, incluidos:
   - Dispositivos que se han unido o registrado en Azure AD.
   - Dispositivos implementados mediante [Windows Autopilot](/windows/deployment/windows-autopilot/windows-autopilot).
   - Impresoras con [Impresión universal](/universal-print/fundamentals/universal-print-getting-started).
- Realizar tareas de administración de identidades de dispositivo como habilitar, deshabilitar, eliminar o administrar.
   - Las [impresoras](/universal-print/fundamentals/) y los dispositivos implementados con [Windows Autopilot](/windows/deployment/windows-autopilot/windows-autopilot) tienen opciones de administración limitadas en Azure AD. Se deben administrar desde sus respectivas interfaces de administración.
- Configurar los valores de la identidad del dispositivo.
- Habilitar o deshabilitar Enterprise State Roaming.
- Consultar los registros de auditoría relacionados con el dispositivo
- Descargar dispositivos (versión preliminar)

[![Vista Todos los dispositivos en Azure Portal](./media/device-management-azure-portal/all-devices-azure-portal.png)](./media/device-management-azure-portal/all-devices-azure-portal.png#lightbox)

Puede acceder al portal de dispositivos mediante los siguientes pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Vaya a **Azure Active Directory** > **Dispositivos**.

## <a name="manage-devices"></a>Administración de dispositivos

Hay dos ubicaciones para administrar dispositivos en Azure AD:

- **Azure Portal** > **Azure Active Directory** > **Dispositivos**
- **Azure Portal** > **Azure Active Directory** > **Usuarios** > Seleccionar un usuario > **Dispositivos**

Ambas opciones permiten a los administradores:

- Buscar dispositivos.
- Ver los detalles del dispositivo, incluidos:
    - Nombre de dispositivo
    - Id. de dispositivo
    - Sistema operativo y versión
    - Tipo de combinación
    - Propietario
    - Cumplimiento y administración de dispositivos móviles
    - Clave de recuperación de BitLocker
- Realizar tareas de administración de identidades de dispositivo como habilitar, deshabilitar, eliminar o administrar.
   - Las [impresoras](/universal-print/fundamentals/) y los dispositivos implementados con [Windows Autopilot](/windows/deployment/windows-autopilot/windows-autopilot) tienen opciones de administración limitadas en Azure AD. Se deben administrar desde sus respectivas interfaces de administración.

> [!TIP]
> - Los dispositivos Windows 10 híbridos unidos a Azure AD no tienen un propietario. Si busca un dispositivo por propietario y no lo encuentra, busque por el identificador de dispositivo.
>
> - Si ve un dispositivo "unido a Azure AD híbrido" con un estado "Pendiente" en la columna REGISTRADO, indica que el dispositivo se ha sincronizado desde Azure AD Connect y está esperando a completar el registro del cliente. Lea más información sobre cómo [planear la implementación de la unión a Azure AD híbrido](hybrid-azuread-join-plan.md). Puede encontrar más información en el artículo [Preguntas más frecuentes sobre la administración de dispositivos de Azure Active Directory](faq.yml).
>
> - Para algunos dispositivos iOS, los nombres de dispositivos que contienen apóstrofos podrían usar caracteres diferentes similares a los apóstrofos. Por tanto, buscar esos dispositivos resulta un poco complicado. Si no ve correctamente los resultados de la búsqueda, asegúrese de que la cadena de búsqueda contiene caracteres de apóstrofo que coincidan.

### <a name="manage-an-intune-device"></a>Administración de un dispositivo de Intune

Si es administrador de Intune, puede administrar los dispositivos en los que MDM está marcado como **Microsoft Intune**. Si el dispositivo no está inscrito con Microsoft Intune, la opción "Administrar" estará atenuada.

### <a name="enable-or-disable-an-azure-ad-device"></a>Habilitación o deshabilitación de un dispositivo de Azure AD

Para habilitar o deshabilitar dispositivos, tiene dos opciones:

- La barra de herramientas de la página **Todos los dispositivos**, después de seleccionar uno o más dispositivos.
- La barra de herramientas después de profundizar en un dispositivo específico.

> [!IMPORTANT]
> - Para habilitar o deshabilitar un dispositivo, tiene que ser administrador global, un administrador de Intune o administrador de dispositivos en la nube de Azure AD. 
> - Cuando se deshabilita un dispositivo, ya no puede autenticarse correctamente con Azure AD, lo que evita que pueda acceder a los recursos de Azure AD que están protegidos por el acceso condicional basado en dispositivos o mediante las credenciales de Windows Hello para empresas.
> - Al deshabilitar un dispositivo, se revocarán el token de actualización principal (PRT) y todos los tokens de actualización (RT) del dispositivo.
> - Las impresoras no se pueden habilitar ni deshabilitar en Azure AD.

### <a name="delete-an-azure-ad-device"></a>Eliminar un dispositivo de Azure AD

Para eliminar un dispositivo, tiene dos opciones:

- La barra de herramientas de la página **Todos los dispositivos**, después de seleccionar uno o más dispositivos.
- La barra de herramientas después de profundizar en un dispositivo específico.

> [!IMPORTANT]
> - Debe tener asignado el rol de administrador de dispositivos en la nube, administrador de Intune o administrador global en Azure AD para eliminar un dispositivo.
> - No se pueden eliminar impresoras ni dispositivos implementados con Windows Autopilot en Azure AD.
> - La eliminación de un dispositivo:
>    - Evita que un dispositivo acceda a los recursos de Azure AD.
>    - Quita todos los detalles asociados al dispositivo como, por ejemplo, las claves de BitLocker de los dispositivos Windows.  
>    - Representa una actividad no recuperable y no se recomienda a menos que sea necesaria.

Si es otra entidad de administración la que administra un dispositivo (por ejemplo, Microsoft Intune), asegúrese de que el dispositivo se haya borrado o retirado antes de eliminarlo de Azure AD. Revise cómo [administrar dispositivos obsoletos](manage-stale-devices.md) antes de eliminar cualquier dispositivo.

### <a name="view-or-copy-device-id"></a>Ver o copiar el id. del dispositivo

Puede usar un id. de dispositivo para verificar los detalles del id.de dispositivo en ese dispositivo o usar PowerShell durante la solución de problemas. Para acceder a la opción de copia, haga clic en el dispositivo.

![Visualización de identificador de dispositivo](./media/device-management-azure-portal/35.png)
  
### <a name="view-or-copy-bitlocker-keys"></a>Ver o copiar las claves de BitLocker

Puede ver y copiar las claves de BitLocker para permitir a los usuarios recuperar unidades cifradas. Estas claves solo están disponibles para dispositivos Windows cifrados y con las claves almacenadas en Azure AD. Puede encontrar estas claves al acceder a los detalles de un dispositivo mediante la selección de **Mostrar clave de recuperación**. Al seleccionar **Mostrar clave de recuperación**, se generará un registro de auditoría que puede encontrar en la categoría `KeyManagement`.

![Visualización de claves de BitLocker](./media/device-management-azure-portal/device-details-show-bitlocker-key.png)

Para ver o copiar las claves de BitLocker, debe ser el propietario del dispositivo, o bien un usuario que tenga al menos uno de los siguientes roles asignados:

- Administrador de dispositivos en la nube
- Administrador global
- Administrador del departamento de soporte técnico
- Administrador de servicios de Intune
- Administrador de seguridad
- Lector de seguridad

### <a name="device-list-filtering-preview"></a>Filtrado de la lista de dispositivos (versión preliminar)

Antes, solo se podía filtrar la lista de dispositivos por actividad y estado habilitado. Esta vista previa permite ahora filtrar la lista de dispositivos de acuerdo con los siguientes atributos en un dispositivo:

- Estado habilitado
- Estado de cumplimiento
- Tipo de combinación (unido a Azure AD, unido a Azure AD híbrido, registrado en Azure AD)
- Marca de tiempo de actividad
- SO
- Tipo de dispositivo (impresoras, máquinas virtuales seguras, dispositivos compartidos, dispositivos registrados)

Para habilitar la funcionalidad de filtrado de versión preliminar en la vista **Todos los dispositivos**:

![Habilitación de la funcionalidad de versión preliminar de filtrado](./media/device-management-azure-portal/device-filter-preview-enable.png)

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Vaya a **Azure Active Directory** > **Dispositivos**.
1. Seleccione el banner que dice **Pruebe las mejoras de la nueva experiencia de filtrado de dispositivos. Haga clic para habilitar la versión preliminar.**

Ahora podrá **agregar filtros** a la vista **Todos los dispositivos**.

### <a name="download-devices-preview"></a>Descargar dispositivos (versión preliminar)

Los administradores de dispositivos en la nube, los administradores de Intune y los administradores globales pueden usar la opción **Descargar dispositivos (versión preliminar)** para exportar un archivo CSV de los dispositivos en función de los filtros aplicados. Si no se aplica ningún filtro a la lista, se exportarán todos los dispositivos. Una exportación puede ejecutarse durante un período máximo de una hora, en función de 

La lista exportada incluye los siguientes atributos de identidad del dispositivo:

`accountEnabled, approximateLastLogonTimeStamp, deviceOSType, deviceOSVersion, deviceTrustType, dirSyncEnabled, displayName, isCompliant, isManaged, lastDirSyncTime, objectId, profileType, registeredOwners, systemLabels, registrationTime, mdmDisplayName`

## <a name="configure-device-settings"></a>Configuración de dispositivo

Para administrar las identidades de los dispositivos en el portal de Azure AD, los dispositivos deben estar [registrados o unidos](overview.md) a Azure AD. Como administrador, puede controlar el proceso de registro y unión de dispositivos mediante la configuración de los siguientes valores del dispositivo.

Debe tener asignado uno de los roles siguientes para ver o administrar la configuración del dispositivo en Azure Portal:

- Administrador global
- Administrador de dispositivos en la nube
- Lector global
- Lector de directorios

![La configuración de dispositivos en relación con Azure AD](./media/device-management-azure-portal/device-settings-azure-portal.png)

- **Los usuarios pueden unir sus dispositivos a Azure AD**: esta opción le permite seleccionar qué usuarios podrán registrar sus dispositivos como dispositivos de unión a Azure AD. El valor predeterminado es **Todos**.

> [!NOTE]
> La opción **Los usuarios pueden inscribir dispositivos en Azure AD**  solo puede utilizarse en Unión a Azure AD en Windows 10. Esta configuración no se aplica a los dispositivos unidos a Azure AD híbrido, a las [máquinas virtuales unidas a Azure AD en Azure](./howto-vm-sign-in-azure-ad-windows.md#enabling-azure-ad-login-in-for-windows-vm-in-azure) ni a dispositivos unidos a Azure AD mediante el [modo de autoimplementación de Windows Autopilot](/mem/autopilot/self-deploying), ya que estos métodos funcionan en contextos sin usuario.

- **Administradores locales adicionales en dispositivos unidos a Azure AD**: puede seleccionar a qué usuarios se conceden derechos de administrador local en un dispositivo. Estos usuarios se agregan al rol *Administradores de dispositivos* en Azure AD. De forma predeterminada, a los administradores globales de Azure AD y a los propietarios de dispositivos se les conceden derechos de administrador local. Esta opción es una capacidad de la edición Premium disponible en productos como Azure AD Premium o Enterprise Mobility Suite (EMS).
- **Los usuarios pueden registrar sus dispositivos con Azure AD**: esta opción se debe configurar para que los dispositivos con Windows 10 personal, iOS, Android y macOS se puedan registrar en Azure AD. Si selecciona **Ninguno**, los dispositivos no podrán registrarse con Azure AD. La inscripción en Microsoft Intune o Administración de dispositivos móviles (MDM) para Microsoft 365 exige registrarse. Si ha configurado alguno de estos servicios, se selecciona **TODOS** y **NINGUNO** no está disponible.
- **Requerir Multi-Factor Authentication para registrar o unir dispositivos con Azure AD**: puede elegir si se va a exigir a los usuarios que proporcionen un factor de autenticación adicional para unir su dispositivo a Azure AD o registrarlo ahí. El valor predeterminado es **No**. Se recomienda exigir Multi-Factor Authentication al registrar o unir un dispositivo. Antes de habilitar Multi-Factor Authentication para este servicio, debe asegurarse de que está configurado para los usuarios que registran sus dispositivos. Para obtener más información sobre los distintos servicios de Azure AD Multi-Factor Authentication, consulte [Introducción a Azure AD Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md). 

> [!NOTE]
> La opción **Requerir Multi-Factor Authentication para registrar o unir dispositivos con Azure AD** se aplica a los dispositivos que están unidos a Azure AD (con algunas excepciones) o registrados en Azure AD. Esta configuración no se aplica a los dispositivos unidos a Azure AD híbrido, a las [máquinas virtuales unidas a Azure AD en Azure](./howto-vm-sign-in-azure-ad-windows.md#enabling-azure-ad-login-in-for-windows-vm-in-azure) ni a dispositivos unidos a Azure AD mediante el [modo de autoimplementación de Windows Autopilot](/mem/autopilot/self-deploying).

> [!IMPORTANT]
> - Se recomienda usar la [acción del usuario "Registrar o unir dispositivos"](../conditional-access/concept-conditional-access-cloud-apps.md#user-actions) en el acceso condicional para exigir la autenticación multifactor para la unión o registro de un dispositivo. 
> - Debe establecer esta opción en **No** si usa la directiva de acceso condicional para exigir la autenticación multifactor. 

- **Número máximo de dispositivos**: esta opción permite seleccionar el número máximo de dispositivos unidos a Azure AD o registrados en Azure AD que puede tener un usuario en Azure AD. Si un usuario alcanza esta cuota, no puede agregar dispositivos adicionales hasta que se quitan uno o varios de los dispositivos existentes. El valor predeterminado es **50**. Puede aumentar el valor hasta 100 y, si escribe un valor mayor que 100, Azure AD lo establecerá en 100. También puede usar un valor ilimitado para no aplicar otros límites aparte de los de cuota existentes.

> [!NOTE]
> La configuración del **número máximo de dispositivos** se aplica a los dispositivos que están unidos a Azure AD o registrados en Azure AD. Este parámetro no se aplica a los dispositivos unidos a Azure AD híbrido.

- [Enterprise State Roaming](enterprise-state-roaming-overview.md)

## <a name="audit-logs"></a>Registros de auditoría

Las actividades de un dispositivo están disponibles a través de los registros de actividad. Estos registros contienen las actividades desencadenadas por el servicio de registro de dispositivos y los usuarios:

- Creación de dispositivos y adición de propietarios o usuarios al dispositivo.
- Cambios en la configuración de un dispositivo
- Operaciones de dispositivo como eliminar o actualizar un dispositivo

El punto de entrada a los datos de auditoría está en **Registros de auditoría**, que se encuentra en la sección **Actividad** de la página **Dispositivos**.

El registro de auditoría tiene una vista de lista predeterminada que muestra:

- La fecha y hora de la repetición
- Los destinos
- El iniciador o actor (quién) de una actividad
- La actividad (qué)

:::image type="content" source="./media/device-management-azure-portal/63.png" alt-text="Captura de pantalla de una tabla en la sección de actividad de la página de dispositivos en la que se enumeran la fecha, el destino, el actor y la actividad de cuatro registros de auditoría" border="false":::.

Puede personalizar la vista de lista, haga clic en **Columnas** en la barra de herramientas.

:::image type="content" source="./media/device-management-azure-portal/64.png" alt-text="Captura de pantalla en la que se muestra la barra de herramientas de la página de dispositivos. Está resaltado el elemento de columna" border="false":::.

Para restringir los datos del informe a un nivel que se adapte a sus necesidades, puede filtrar los datos de auditoría con los siguientes campos:

- Category
- Tipo de recurso de actividad
- Actividad
- Intervalo de fechas
- Destino
- Iniciado por (actor)

Además de los filtros, puede buscar entradas concretas.

:::image type="content" source="./media/device-management-azure-portal/65.png" alt-text="Captura de pantalla de controles de filtro de datos de auditoría, con los campos categoría, tipo de recurso de actividad, actividad, intervalo de fechas, destino y actor y un campo de búsqueda" border="false":::.

## <a name="next-steps"></a>Pasos siguientes

[Procedimiento para administrar los dispositivos obsoletos en Azure AD](manage-stale-devices.md)

[Enterprise State Roaming](enterprise-state-roaming-overview.md)
