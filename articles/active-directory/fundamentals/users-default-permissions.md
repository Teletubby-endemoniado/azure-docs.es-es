---
title: 'Permisos de usuario predeterminados: Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre los distintos permisos de usuario disponibles en Azure Active Directory.
services: active-directory
author: ajburnle
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2021
ms.author: ajburnle
ms.reviewer: vincesm
ms.custom: it-pro, seodec18, contperf-fy21q1
ms.collection: M365-identity-device-management
ms.openlocfilehash: f89be09e665b6bea096b81db41d4262f51426eb3
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132719720"
---
# <a name="what-are-the-default-user-permissions-in-azure-active-directory"></a>¿Cuales son los permisos de usuario predeterminados en Azure Active Directory?
En Azure Active Directory (Azure AD), a todos los usuarios se les otorga un conjunto de permisos predeterminados. El acceso de un usuario consta del tipo de usuario, sus [asignaciones de roles](active-directory-users-assign-role-azure-portal.md) y su propiedad de objetos individuales. En este artículo se describen dichos permisos predeterminados y contiene una comparación de los valores predeterminados de los usuarios miembros e invitados. Los permisos de usuario predeterminados solo se pueden cambiar en la configuración de usuario de Azure AD.

## <a name="member-and-guest-users"></a>Usuarios miembros e invitados
El conjunto de permisos predeterminados recibido depende de si el usuario es miembro nativo del inquilino (usuario miembro) o si el usuario se incorpora desde otro directorio como un invitado de la colaboración B2B (usuario invitado). Vea [¿Qué es el acceso de usuarios invitados de B2B?](../external-identities/what-is-b2b.md) para más información sobre la adición de usuarios invitados.
* Los usuarios miembro pueden registrar aplicaciones, administrar el número de teléfono móvil y la fotografía de su propio perfil, cambiar su contraseña e invitar a los invitados de B2B. Además, los usuarios pueden leer toda la información del directorio (con algunas excepciones). 
* Los usuarios invitados tienen permisos de directorio restringidos. Pueden administrar su propio perfil, cambiar su propia contraseña y recuperar algo de información sobre otros usuarios, grupos y aplicaciones, pero no pueden leer toda la información del directorio. Por ejemplo, los usuarios invitados no pueden enumerar la lista de todos los usuarios, los grupos y otros objetos de directorio. Los invitados se pueden agregar a los roles de administrador, que les conceden permisos completos de lectura y escritura incluidos en el rol. Los invitados también pueden invitar a otros usuarios.

## <a name="compare-member-and-guest-default-permissions"></a>Comparación de los permisos predeterminados de miembros e invitados

**Ámbito** | **Permisos de usuarios miembros** | **Permisos de usuarios invitados predeterminados** | **Permisos de usuarios administrados restringidos**
------------ | --------- | ---------- | ----------
Usuarios y contactos | <ul><li>Enumerar la lista de todos los usuarios y contactos<li>Leer todas las propiedades públicas de usuarios y contactos</li><li>Invitar a los invitados<li>Cambiar la contraseña propia<li>Administrar el número de teléfono móvil propio<li>Administrar la fotografía propia<li>Invalidar tokens de actualización propios</li></ul> | <ul><li>Leer las propiedades propias<li>Leer el nombre para mostrar, el correo electrónico, el nombre de inicio de sesión, la fotografía, el nombre principal de usuario y las propiedades de tipo de usuario de otros usuarios y contactos<li>Cambiar la contraseña propia<li>Buscar otro usuario por ObjectId (si se permite)<li>Leer información de administrador y subordinado directo de otros usuarios</li></ul> | <ul><li>Leer las propiedades propias<li>Cambiar la contraseña propia</li><li>Administrar el número de teléfono móvil propio</li></ul>
Grupos | <ul><li>Crear grupos de seguridad<li>Crear grupos de Microsoft 365<li>Enumerar una lista de todos los grupos<li>Leer todas las propiedades de los grupos<li>Leer las pertenencias a grupos no ocultos<li>Leer las pertenencias a grupos de Microsoft 365 ocultos para los grupos a los que se ha unido<li>Administrar las propiedades, la propiedad y la pertenencia de los grupos propiedad del usuario<li>Agregar invitados a los grupos que se poseen<li>Administrar la configuración de pertenencia dinámica<li>Eliminar los grupos que se poseen<li>Restaurar los grupos de Microsoft 365 que se poseen</li></ul> | <ul><li>Leer las propiedades de los grupos no ocultos, incluida la pertenencia y la propiedad (incluso de grupos no unidos)<li>Leer las pertenencias a grupos de Microsoft 365 ocultos para los grupos a los que se ha unido<li>Buscar grupos por Nombre para mostrar o Id. de objeto (si se permite)</li></ul> | <ul><li>Leer el id. de objeto de los grupos unidos<li>Leer la pertenencia y la propiedad de los grupos unidos en algunas aplicaciones de Microsoft 365 (si se permite)</li></ul>
Aplicaciones | <ul><li>Registrar aplicaciones nuevas<li>Enumerar la lista de todas las aplicaciones<li>Leer las propiedades de las aplicaciones registradas y empresariales<li>Administrar las propiedades, asignaciones y credenciales de las aplicaciones que se poseen<li>Crear o eliminar contraseña de la aplicación para el usuario<li>Eliminar las aplicaciones que se poseen<li>Restaurar las aplicaciones que se poseen</li></ul> | <ul><li>Leer las propiedades de las aplicaciones registradas y empresariales</li></ul> | <ul><li>Leer las propiedades de las aplicaciones registradas y empresariales
Dispositivos</li></ul> | <ul><li>Enumerar lista de todos los dispositivos<li>Leer todas las propiedades de los dispositivos<li>Administrar todas las propiedades de los dispositivos que se poseen</li></ul> | Sin permisos | Sin permisos
Directorio | <ul><li>Leer toda la información de la compañía<li>Leer todos los dominios<li>Leer todos los contratos de los asociados</li></ul> | <ul><li>Leer el nombre para mostrar de la empresa<li>Leer todos los dominios</li></ul> | <ul><li>Leer el nombre para mostrar de la empresa<li>Leer todos los dominios</li></ul>
Roles y ámbitos | <ul><li>Leer todos los roles y las pertenencias administrativas<li>Leer todas las propiedades y la pertenencia de las unidades administrativas</li></ul> | Sin permisos | Sin permisos
Suscripciones | <ul><li>Leer todas las suscripciones<li>Habilitar a miembro del plan de servicio</li></ul> | Sin permisos | Sin permisos
Directivas | <ul><li>Leer todas las propiedades de las directivas<li>Administrar todas las propiedades de las directivas que se poseen</li></ul> | Sin permisos | Sin permisos

## <a name="restrict-member-users-default-permissions"></a>Restringir los permisos predeterminados de los usuarios miembros 

Es posible agregar restricciones a los permisos predeterminados de los usuarios. Puede que algunas organizaciones tengan que restringir el acceso de los usuarios al portal. Esta característica se usará si no quiere que todos los usuarios del directorio tengan acceso al directorio o al portal de administración de Azure AD. 

Por ejemplo, una universidad tendrá muchos usuarios dentro de su directorio, y es posible que el administrador no quiera que todos los alumnos del directorio puedan ver el directorio completo e infrinjan la privacidad de otros alumnos. El uso de esta característica es opcional y queda a criterio del administrador de Azure AD. Los permisos predeterminados de los usuarios miembros se pueden restringir de las maneras siguientes:

Permiso | Explicación del valor
---------- | ------------
Los usuarios pueden registrar aplicaciones | Si se selecciona No en esta opción, se impide que los usuarios creen registros de aplicaciones. La capacidad puede ser devuelta a individuos específicos agregándolos al rol Desarrollador de aplicaciones.
Permitir a los usuarios conectar su cuenta profesional o educativa con LinkedIn | Si se selecciona No en esta opción, se impide que los usuarios conecten su cuenta profesional o educativa con su cuenta de LinkedIn. Para más información, consulte [Consentimiento y uso compartido de datos de conexiones de cuentas de LinkedIn](../enterprise-users/linkedin-user-consent.md).
Capacidad para crear grupos de seguridad | Si se selecciona No en esta opción, se impide que los usuarios creen grupos de seguridad. Tanto los administradores globales como los administradores de tipo usuario pueden seguir creando grupos de seguridad. Para aprender a hacerlo, consulte [Cmdlets de Azure Active Directory para configurar las opciones de grupo](../enterprise-users/groups-settings-cmdlets.md).
Capacidad de crear grupos de Microsoft 365 | Si esta opción se establece en No, se impide que los usuarios creen grupos de Microsoft 365. Si esta opción se establece en Algunos, se permite que un conjunto de usuarios creen grupos de Microsoft 365. Tanto los administradores globales como los administradores de usuarios pueden seguir creando grupos de Microsoft 365. Para aprender a hacerlo, consulte [Cmdlets de Azure Active Directory para configurar las opciones de grupo](../enterprise-users/groups-settings-cmdlets.md).
Restringir el acceso al portal de administración de Azure AD | <p>Si esta opción se establece en No, los usuarios que no son administradores pueden usar el portal de administración de Azure AD para leer y administrar recursos de Azure AD. Si se elige Sí, los usuarios que no son administradores no podrán acceder a ningún dato de Azure AD en el portal de administración.</p><p>**Nota**: Esta configuración no restringe el acceso a los datos de Azure AD mediante PowerShell u otros clientes como Visual Studio. Cuando se establece en Sí, para conceder a un usuario no administrador específico la capacidad de usar el portal de administración de Azure AD, asigne cualquier rol administrativo, como el rol Lectores de directorio.</p><p>**Nota**: Esta configuración impedirá que los usuarios no administradores, que sean propietarios de grupos o aplicaciones utilicen Azure Portal administrar los recursos de su propiedad.</p><p>Este rol permite leer información básica del directorio, que los usuarios miembros tienen de forma predeterminada (los invitados y las entidades de servicio, no).</p><p>**Nota**: esta configuración no restringe el acceso siempre y cuando a un usuario se le asigne un rol personalizado o cualquier rol en ese sentido y no sea solo un usuario.</p>
Capacidad para leer otros usuarios | Esta configuración solo está disponible en PowerShell. Si establece esta marca en $false, se impide que quienes no son administradores lean la información de los usuarios desde el directorio. Esta marca no impide que puedan leer la información de los usuarios en otros servicios de Microsoft, como Exchange Online. Esta configuración está pensada para circunstancias especiales y no se recomienda establecer esta marca en $false.

>[!NOTE]
>Se supone que el usuario medio solo utilizará el portal para acceder a Azure AD y no usará PowerShell ni la CLI para acceder a sus recursos. Actualmente, la restricción del acceso a los permisos predeterminados de los usuarios solo tiene lugar cuando el usuario intenta acceder al directorio dentro de Azure Portal.

## <a name="restrict-guest-users-default-permissions"></a>Restringir los permisos predeterminados de los usuarios invitados

Los permisos predeterminados de los usuarios invitados se pueden restringir de las maneras siguientes:

>[!NOTE]
>La configuración de restricciones de acceso para los usuarios invitados reemplazó a la opción **Los permisos de los usuarios invitados están limitados**. Para instrucciones sobre cómo usar esta característica, consulte [Restricción de los permisos de acceso de usuario invitado en Azure Active Directory](../enterprise-users/users-restrict-guest-permissions.md).

Permiso | Explicación del valor
---------- | ------------
Restricciones de acceso de usuarios invitados | Establecer esta opción en **Los usuarios invitados tienen el mismo acceso que los miembros** concede de manera predeterminada todos los permisos de usuario miembro a los usuarios invitados.<p>Establecer esta opción en **El acceso de los usuarios invitados está restringido a las propiedades y pertenencias de sus propios objetos de directorio** restringe de manera predeterminada el acceso de un invitado solo a su propio perfil de usuario. Ya no se permite el acceso a otros usuarios ni siquiera al buscar por Nombre principal de usuario, Id. de objeto o Nombre para mostrar. Además, ya no se permite el acceso a la información de grupos, incluidas las pertenencias a grupos.<p>**Nota**: Esta configuración no impide el acceso a unidos grupos de otros servicios de Microsoft 365 como Microsoft Teams. Consulte [Acceso de invitado en Microsoft Teams](/MicrosoftTeams/guest-access) para más información.<p>Los usuarios invitados se pueden seguir agregando a los roles Administrador independientemente de esta configuración de permiso.
Los invitados pueden invitar | Establecer esta opción en Sí permite que los invitados inviten a otros usuarios. Consulte [Delegación de invitaciones de la colaboración B2B de Azure Active Directory](../external-identities/delegate-invitations.md#configure-b2b-external-collaboration-settings) para más información.
Los miembros pueden invitar | Establecer esta opción en Sí permite que miembros que no son administradores de su directorio inventen a otros usuarios. Consulte [Delegación de invitaciones de la colaboración B2B de Azure Active Directory](../external-identities/delegate-invitations.md#configure-b2b-external-collaboration-settings) para más información.
Los administradores y los usuarios del rol de invitador de personas pueden invitar | Establecer esta opción en Sí permite que los administradores y usuarios con el rol "Invitador de usuarios invitados" inviten a otros usuarios. Cuando se establece en Sí, los usuarios con el rol Invitador de usuarios invitados todavía podrán invitar a otros usuarios, independientemente de la configuración Los miembros pueden invitar. Consulte [Delegación de invitaciones de la colaboración B2B de Azure Active Directory](../external-identities/delegate-invitations.md#assign-the-guest-inviter-role-to-a-user) para más información.

## <a name="object-ownership"></a>Propiedad del objeto

### <a name="application-registration-owner-permissions"></a>Permisos de propietario del registro de una aplicación
Cuando un usuario registra una aplicación, se agrega automáticamente como propietario de la misma. Como propietario, puede administrar los metadatos de la aplicación, como el nombre y los permisos que solicita la aplicación. También pueden administrar la configuración específica del inquilino de la aplicación, como la configuración de SSO y las asignaciones de usuarios. Un propietario también puede agregar o quitar otros propietarios. A diferencia de los administradores globales, los propietarios solo pueden administrar las aplicaciones que poseen.

### <a name="enterprise-application-owner-permissions"></a>Permisos de propietario de las aplicaciones empresariales
Cuando un usuario agrega una aplicación empresarial, se agrega automáticamente como propietario. Como propietario, también puede administrar la configuración específica del inquilino de la aplicación, como la configuración de SSO, el aprovisionamiento y las asignaciones de usuarios. Un propietario también puede agregar o quitar otros propietarios. A diferencia de los administradores globales, los propietarios solo pueden administrar las aplicaciones que poseen.

### <a name="group-owner-permissions"></a>Permisos de propietario de grupo
Cuando un usuario crea un grupo, se agrega automáticamente como propietario de dicho grupo. Como propietario, puede administrar las propiedades del grupo, como el nombre, así como administrar la pertenencia al grupo. Un propietario también puede agregar o quitar otros propietarios. A diferencia de los administradores globales y los administradores de tipo usuario, los propietarios solo pueden administrar los grupos que poseen. Para asignar un propietario de grupo, consulte [Administración de propietarios de un grupo](active-directory-accessmanagement-managing-group-owners.md).

### <a name="ownership-permissions"></a>Permisos de propiedad
En las tablas siguientes se describen los permisos específicos de Azure Active Directory que los usuarios miembro tienen sobre los objetivos que poseen. El usuario solamente tiene estos permisos en objetos que posee.

#### <a name="owned-application-registrations"></a>Registros de aplicación que se poseen
Los usuarios pueden realizar las siguientes acciones en los registros de aplicación que poseen.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/applications/audience/update | Actualiza la propiedad applications.audience en Azure Active Directory. |
| microsoft.directory/applications/authentication/update | Actualiza la propiedad applications.authentication en Azure Active Directory. |
| microsoft.directory/applications/basic/update | Actualiza las propiedades básicas de las aplicaciones en Azure Active Directory. |
| microsoft.directory/applications/credentials/update | Actualiza la propiedad applications.credentials en Azure Active Directory. |
| microsoft.directory/applications/delete | Elimina aplicaciones en Azure Active Directory. |
| microsoft.directory/applications/owners/update | Actualiza la propiedad applications.owners en Azure Active Directory. |
| microsoft.directory/applications/permissions/update | Actualiza la propiedad applications.permissions en Azure Active Directory. |
| microsoft.directory/applications/policies/update | Actualiza la propiedad applications.policies en Azure Active Directory. |
| microsoft.directory/applications/restore | Restaura aplicaciones en Azure Active Directory. |

#### <a name="owned-enterprise-applications"></a>Aplicaciones empresariales que se poseen
Los usuarios pueden realizar las siguientes acciones en las aplicaciones empresariales que poseen. Una aplicación empresarial se compone de la entidad de servicio, una o varias directivas de aplicación y, a veces, un objeto de aplicación en el mismo inquilino que la entidad de servicio.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/auditLogs/allProperties/read | Lee todas las propiedades (incluidas las propiedades con privilegios) en auditLogs en Azure Active Directory. |
| microsoft.directory/policies/basic/update | Actualiza las propiedades básicas en las directivas de Azure Active Directory. |
| microsoft.directory/policies/delete | Elimina directivas en Azure Active Directory. |
| microsoft.directory/policies/owners/update | Actualiza la propiedad policies.owners en Azure Active Directory. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/update | Actualiza la propiedad servicePrincipals.appRoleAssignedTo en Azure Active Directory. |
| microsoft.directory/servicePrincipals/appRoleAssignments/update | Actualiza la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/servicePrincipals/audience/update | Actualiza la propiedad servicePrincipals.audience en Azure Active Directory. |
| microsoft.directory/servicePrincipals/authentication/update | Actualiza la propiedad servicePrincipals.authentication en Azure Active Directory. |
| microsoft.directory/servicePrincipals/basic/update | Actualiza las propiedades básicas de servicePrincipals en Azure Active Directory. |
| microsoft.directory/servicePrincipals/credentials/update | Actualiza la propiedad servicePrincipals.credentials en Azure Active Directory. |
| microsoft.directory/servicePrincipals/delete | Elimina servicePrincipals in Azure Active Directory. |
| microsoft.directory/servicePrincipals/owners/update | Actualiza la propiedad servicePrincipals.owners en Azure Active Directory. |
| microsoft.directory/servicePrincipals/permissions/update | Actualiza la propiedad servicePrincipals.permissions en Azure Active Directory. |
| microsoft.directory/servicePrincipals/policies/update | Actualiza la propiedad servicePrincipals.policies en Azure Active Directory. |
| microsoft.directory/signInReports/allProperties/read | Lee todas las propiedades (incluidas las propiedades con privilegios) en signInReports en Azure Active Directory. |

#### <a name="owned-devices"></a>Dispositivos que se poseen
Los usuarios pueden realizar las siguientes acciones en los dispositivos que poseen.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/devices/bitLockerRecoveryKeys/read | Lee la propiedad devices.bitLockerRecoveryKeys en Azure Active Directory. |
| microsoft.directory/devices/disable | Deshabilita dispositivos en Azure Active Directory. |

#### <a name="owned-groups"></a>Grupos que se poseen
Los usuarios pueden realizar las siguientes acciones en los grupos que poseen.

> [!NOTE]
> Los propietarios de grupos dinámicos deben tener un rol de Administrador global, Administrador de grupo, Administrador de Intune o Administrador de usuarios para editar las reglas de pertenencia a grupos. Para obtener más información, vea [Creación o actualización de un grupo dinámico en Azure Active Directory](../enterprise-users/groups-create-rule.md).

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/groups/appRoleAssignments/update | Actualiza la propiedad groups.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/groups/basic/update | Actualiza las propiedades básicas de los grupos en Azure Active Directory. |
| microsoft.directory/groups/delete | Elimina grupos en Azure Active Directory. |
| microsoft.directory/groups/members/update | Actualiza la propiedad groups.members en Azure Active Directory. |
| microsoft.directory/groups/owners/update | Actualiza la propiedad groups.owners en Azure Active Directory. |
| microsoft.directory/groups/restore | Restaura grupos en Azure Active Directory. |
| microsoft.directory/groups/settings/update | Actualiza la propiedad groups.settings en Azure Active Directory. |

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la configuración de restricciones de acceso para los usuarios invitados, consulte [Restricción de los permisos de acceso de usuario invitado en Azure Active Directory](../enterprise-users/users-restrict-guest-permissions.md).
* Para obtener más información sobre cómo asignar roles de administrador de Azure AD, vea [Asignación de roles de administrador a un usuario en Azure Active Directory](active-directory-users-assign-role-azure-portal.md)
* Para más información acerca de cómo se controla el acceso a los recursos en Microsoft Azure, consulte [Descripción de acceso a los recursos de Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Para más información acerca de cómo se relaciona Azure Active Directory con la suscripción de Azure, consulte [Asociación de las suscripciones de Azure con Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* [Administrar usuarios](add-users-azure-active-directory.md)
