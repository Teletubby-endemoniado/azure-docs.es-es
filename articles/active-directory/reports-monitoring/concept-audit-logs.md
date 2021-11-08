---
title: Registros de auditoría en Azure Active Directory | Microsoft Docs
description: Información general de los registros de auditoría en Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: karenhoran
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/30/2021
ms.author: markvi
ms.reviewer: besiler
ms.collection: M365-identity-device-management
ms.openlocfilehash: d551124491c33cd74f3fbdf1f4b401ada99399d4
ms.sourcegitcommit: 27ddccfa351f574431fb4775e5cd486eb21080e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2021
ms.locfileid: "131997179"
---
# <a name="audit-logs-in-azure-active-directory"></a>Registros de auditoría en Azure Active Directory 

Como administrador de TI, quiere saber cómo funciona el entorno de TI. La información sobre el estado del sistema le permite evaluar si es necesario responder a posibles problemas y cómo hacerlo. 

Para ayudarle a conseguir este objetivo, el portal de Azure Active Directory le proporciona acceso a tres registros de actividad:

- **[Inicios de sesión](concept-sign-ins.md)** : Información sobre los inicios de sesión y cómo los usuarios emplean los recursos.
- **[Auditoría](concept-audit-logs.md)** : información sobre los cambios aplicados al inquilino, como la administración de usuarios y grupos o las actualizaciones aplicadas a los recursos del inquilino.
- **[Aprovisionamiento](concept-provisioning-logs.md)** : actividades realizadas por el servicio de aprovisionamiento, como la creación de un grupo en ServiceNow o un usuario importado de Workday.

En este artículo se proporciona información general de los informes de auditoría.


## <a name="what-is-it"></a>¿Qué es?

Con los registros de auditoría de Azure AD, obtiene acceso a registros de actividades del sistema para el cumplimiento.
Las vistas más comunes de este registro se basan en las siguientes categorías:

- Administración de usuarios

- Administración de grupos
 
- Administración de aplicaciones  


Con una vista centrada en los usuarios, puede obtener respuestas a preguntas como:

- ¿Qué tipos de actualizaciones se han aplicado a los usuarios?

- ¿Cuántos usuarios han cambiado?

- ¿Cuántas contraseñas han cambiado?

- ¿Qué ha hecho un administrador en un directorio?


Con una vista centrada en los grupos, puede obtener respuestas a preguntas como:

- ¿Cuáles son los grupos que se han agregado?

- ¿Hay grupos con cambios de pertenencia?

- ¿Se han cambiado los propietarios del grupo?

- ¿Qué licencias se han asignado a un grupo o un usuario?

Con una vista centrada en la aplicación, puede obtener respuestas a preguntas como:

- ¿Qué aplicaciones se han agregado o actualizado?

- ¿Qué aplicaciones se han quitado?

- ¿Ha cambiado la entidad de servicio de una aplicación?

- ¿Se han cambiado los nombres de las aplicaciones?

- ¿Quién dio el consentimiento a una aplicación?

 
## <a name="what-license-do-i-need"></a>¿Qué licencia necesito?

El informe de actividad de auditoría está disponible en todas las ediciones de Azure AD.

## <a name="who-can-access-it"></a>¿Quién puede acceder a ellos?

Para acceder a los registros de auditoría, debe tener uno de los siguientes roles: 

- Administrador de seguridad
- Lector de seguridad
- Lector de informes
- Lector global
- Administrador global

## <a name="where-can-i-find-it"></a>¿Dónde se encuentra?

Azure Portal ofrece varias opciones para acceder al registro. Por ejemplo, en el menú Azure Active Directory, puede abrir el registro en la sección **Supervisión**.  

![Apertura de registros de auditoría](./media/concept-audit-logs/audit-logs-menu.png)

Además, puede ir directamente a los registros de auditoría mediante [este vínculo](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/ProvisioningEvents).


También puede acceder al registro de auditoría mediante Microsoft Graph API.


## <a name="what-is-the-default-view"></a>¿Cuál es la vista predeterminada?

Un registro de auditoría tiene una vista de lista predeterminada que muestra:

- la fecha y hora de la repetición
- el servicio que ha registrado la repetición
- la categoría y el nombre de la actividad (*qué*) 
- el estado de la actividad (correcto o incorrecto)
- el destino
- el iniciador o actor (quién) de una actividad

![Registros de auditoría](./media/concept-audit-logs/listview.png "Registros de auditoría")

Puede personalizar la vista de lista, haga clic en **Columnas** en la barra de herramientas.

![Columnas de auditoría](./media/concept-audit-logs/columns.png "Columnas de auditoría")

Esto le permite mostrar los campos adicionales o quitar los campos que ya se están mostrando.

![Quitar campos](./media/concept-audit-logs/columnselect.png "Quitar campos")

Seleccione un elemento de la vista de lista para obtener información más detallada.

![seleccionar elemento](./media/concept-audit-logs/details.png "Seleccionar elemento")

## <a name="filtering-audit-logs"></a>Filtrado de registros de auditoría

Puede filtrar los datos de auditoría por los siguientes campos:

- Servicio
- Category
- Actividad
- Status
- Destino
- Iniciado por (actor)
- Intervalo de fechas

![Objeto de filtro](./media/concept-audit-logs/filter.png "Filter, objeto")

El filtro **Service** (Servicio) le permite seleccionar los siguientes servicios de una lista desplegable:

- All
- Experiencia de usuario de administración de AAD
- Revisiones de acceso
- Account Provisioning (Aprovisionamiento de cuentas)
- Proxy de aplicación
- Métodos de autenticación
- B2C
- Acceso condicional
- Core Directory (Directorio principal)
- Administración de derechos
- Autenticación híbrida
- Protección de identidad
- Invited Users (Usuarios invitados)
- MIM Service (Servicio MIM)
- MyApps
- PIM
- Self-service Group Management (Administración de grupos de autoservicio)
- Self-service Password Management (Administración de contraseñas de autorservicio)
- Términos de uso

El filtro **Category** (Categoría) le permite seleccionar uno de los filtros siguientes:

- All
- AdministrativeUnit
- ApplicationManagement
- Authentication
- Authorization
- Contacto
- Dispositivo
- DeviceConfiguration
- DirectoryManagement
- EntitlementManagement
- GroupManagement
- KerberosDomain
- KeyManagement
- Etiqueta
- Otros
- PermissionGrantPolicy
- Directiva
- ResourceManagement
- RoleManagement
- UserManagement

El filtro **Activity** (Actividad) se basa en la selección de la categoría y el tipo de recurso de actividad que realice. Puede seleccionar la actividad específica que desea ver o elegir todas. 

Puede obtener la lista de todas las actividades de auditoría mediante Graph API: `https://graph.windows.net/<tenantdomain>/activities/auditActivityTypesV2?api-version=beta`

El filtro **Status** (Estado) le permite filtrar en función del estado de una operación de auditoría. El estado puede ser uno de los siguientes:

- All
- Correcto
- Error

El filtro **Target** (Destino) le permite buscar un destino determinado por nombre o nombre principal de usuario (UPN). El nombre de destino y el UPN distinguen mayúsculas de minúsculas. 

El filtro **Iniciado por** (Initiated by) le permite definir por qué empieza el nombre de un actor o un nombre principal universal (UPN). El nombre y el UPN distinguen mayúsculas de minúsculas.

El filtro **Date range** (Intervalo de fechas) permite definir un período de tiempo para los datos devueltos.  
Los valores posibles son:

- 7 días
- 24 horas
- Personalizado

Cuando se selecciona un intervalo de tiempo personalizado, puede configurar una hora de inicio y una hora de finalización.

También puede descargar los datos filtrados, hasta 250 000 registros, si selecciona el botón **Download** (Descargar). Puede descargar los registros en formato CSV o JSON. El número de registros que se puede descargar también está restringido por las [directivas de retención de informes de Azure Active Directory](reference-reports-data-retention.md).

![Descarga de datos](./media/concept-audit-logs/download.png "Descarga de datos")



## <a name="microsoft-365-activity-logs"></a>Registros de actividad de Microsoft 365

Puede ver los registros de actividad de Microsoft 365 desde el [Centro de administración de Microsoft 365](/office365/admin/admin-overview/about-the-admin-center). Aunque los registros de actividad de Microsoft 365 y Azure AD comparten muchos de los recursos del directorio, solo el Centro de administración de Microsoft 365 proporciona una vista completa de los registros de actividad de Microsoft 365. 

También puede acceder a los registros de actividad de Microsoft 365 mediante programación con las [API de administración de Office 365](/office/office-365-management-api/office-365-management-apis-overview).

> [!NOTE]
> La mayoría de las suscripciones independientes o agrupadas de Microsoft 365 tienen dependencias de back-end en algunos subsistemas dentro del límite del centro de datos de Microsoft 365. Las dependencias necesitan la escritura diferida de cierta información para mantener los directorios sincronizados y, básicamente, para ayudar a habilitar la incorporación sin complicaciones en una suscripción para participar en Exchange Online. Para estas escrituras diferidas, las entradas del registro de auditoría muestran las acciones realizadas por "Microsoft Substrate Management". Estas entradas del registro de auditoría hacen referencia a las operaciones de creación, actualización y eliminación ejecutadas por Exchange Online para Azure AD. Las entradas son informativas y no se necesita ninguna acción.

## <a name="next-steps"></a>Pasos siguientes

- [Referencia sobre actividades de auditoría de Azure AD](reference-audit-activities.md)
- [Referencia sobre la retención de registros de Azure AD](reference-reports-data-retention.md)
- [Referencia sobre las latencias de registro de Azure AD](reference-reports-latencies.md)
- [Actores desconocidos en el informe de auditoría](/troubleshoot/azure/active-directory/unknown-actors-in-audit-reports)
