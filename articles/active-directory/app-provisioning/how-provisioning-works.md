---
title: Descripción del aprovisionamiento de aplicaciones en Azure Active Directory
description: Describa el funcionamiento del aprovisionamiento de aplicaciones en Azure Active Directory.
services: active-directory
author: kenwith
manager: mtillman
ms.service: active-directory
ms.subservice: app-provisioning
ms.topic: conceptual
ms.workload: identity
ms.date: 06/11/2021
ms.author: kenwith
ms.reviewer: arvinh
ms.openlocfilehash: 957ee0b5a7301fa1959b3a88450dd047bfacaad5
ms.sourcegitcommit: 87de14fe9fdee75ea64f30ebb516cf7edad0cf87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2021
ms.locfileid: "129353678"
---
# <a name="how-application-provisioning-works-in-azure-active-directory"></a>Funcionamiento del aprovisionamiento de aplicaciones en Azure Active Directory

El aprovisionamiento automático hace referencia a la creación de identidades y roles de usuario en las aplicaciones en la nube a las que los usuarios necesitan acceder. Además de crear identidades de usuario, el aprovisionamiento automático incluye el mantenimiento y la eliminación de identidades de usuario a medida que el estado o los roles cambian. Antes de iniciar una implementación, puede revisar este artículo para obtener información sobre cómo funciona el aprovisionamiento de Azure AD y cómo obtener recomendaciones de configuración. 

El **servicio de aprovisionamiento de Azure AD** aprovisiona usuarios para las aplicaciones SaaS y otros sistemas mediante la conexión a un punto de conexión de la API de administración de usuarios de System for Cross-Domain Identity Management (SCIM) 2.0 que proporciona el proveedor de la aplicación. Este punto de conexión SCIM permite a Azure AD crear, actualizar y quitar usuarios mediante programación. Para las aplicaciones seleccionadas, el servicio de aprovisionamiento también puede crear, actualizar y quitar objetos adicionales relacionados con la identidad, como los grupos y los roles. El canal usado para el aprovisionamiento entre Azure AD y la aplicación se cifra mediante el cifrado HTTPS TLS 1.2.


![Servicio de aprovisionamiento de Azure AD](./media/how-provisioning-works/provisioning0.PNG)
*Figura 1: Servicio de aprovisionamiento de Azure AD*

![Flujo de trabajo de aprovisionamiento de usuarios salientes](./media/how-provisioning-works/provisioning1.PNG)
*Figura 2: Flujo de trabajo "saliente" del aprovisionamiento de usuarios desde Azure AD a aplicaciones SaaS populares*

![Flujo de trabajo de aprovisionamiento de usuarios salientes](./media/how-provisioning-works/provisioning2.PNG)
*Figura 3: Flujo de trabajo "entrante" del aprovisionamiento de usuarios desde aplicaciones populares de administración del capital humano (HCM) a Azure Active Directory y Windows Server Active Directory*

## <a name="provisioning-using-scim-20"></a>Aprovisionamiento mediante SCIM 2.0

El servicio de aprovisionamiento de Azure AD usa el [protocolo SCIM 2.0](https://techcommunity.microsoft.com/t5/Identity-Standards-Blog/bg-p/IdentityStandards) para el aprovisionamiento automático. El servicio se conecta al punto de conexión de SCIM para la aplicación y utiliza el esquema de objetos de usuario SCIM y las API REST para automatizar el aprovisionamiento y el desaprovisionamiento de usuarios y grupos. Se proporciona un conector de aprovisionamiento basado en SCIM para la mayoría de las aplicaciones en la galería de Azure AD. Al crear aplicaciones para Azure AD, los desarrolladores pueden usar la API de administración de usuarios de SCIM 2.0 para construir un punto de conexión SCIM que integre Azure AD para el aprovisionamiento. Para detalles, consulte [Aprovisionamiento de usuarios de SCIM con Azure Active Directory (Azure AD)](../app-provisioning/use-scim-to-provision-users-and-groups.md).

Para solicitar un conector de aprovisionamiento automático de Azure AD para una aplicación que actualmente no tiene uno, complete una [solicitud de aplicación de Azure Active Directory](https://aka.ms/aadapprequest).

## <a name="authorization"></a>Authorization

Se requieren credenciales para que Azure AD se conecte a la API de administración de usuarios de la aplicación. Cuando configure el aprovisionamiento automático de usuarios para una aplicación, deberá especificar credenciales válidas. En el caso de las aplicaciones de la galería, puede encontrar los tipos de credenciales y los requisitos para la aplicación al consultar el tutorial de la aplicación. En el caso de las aplicaciones que no son de la galería, puede consultar la documentación de [SCIM](./use-scim-to-provision-users-and-groups.md#authorization-to-provisioning-connectors-in-the-application-gallery) para saber más sobre los tipos de credenciales y sus requisitos. En Azure Portal, podrá probar las credenciales al hacer que Azure AD intente conectarse a la aplicación de aprovisionamiento de la aplicación con las credenciales proporcionadas.

## <a name="mapping-attributes"></a>Asignación de atributos

Cuando habilita el aprovisionamiento de usuarios para una aplicación SaaS de terceros, Azure Portal controla sus valores de atributo en forma de una asignación de atributos. Las asignaciones determinan los atributos de usuario que fluyen entre Azure AD y la aplicación de destino cuando las cuentas de usuario se aprovisionan o se actualizan.

Hay un conjunto preconfigurado de atributos y asignaciones de atributos entre los objetos de usuario de Azure AD y los objetos de usuario de cada aplicación SaaS. Además de los usuarios, algunas aplicaciones administran otros tipos de objetos, como los grupos.

Al configurar el aprovisionamiento, es importante revisar y configurar las asignaciones de atributos y los flujos de trabajo que definen qué propiedades de usuario (o de grupo) fluyen de Azure AD a la aplicación. Revise y configure la propiedad de coincidencia (**Hacer coincidir objetos con este atributo** ) que se usa para identificar de forma exclusiva y emparejar a usuarios y grupos entre ambos sistemas.

Puede personalizar las asignaciones de atributos predeterminadas según sus necesidades empresariales. Esto significa que puede cambiar o eliminar asignaciones de atributos existentes o crear nuevas asignaciones de atributos. Para detalles, consulte [Personalización de asignaciones de atributos de aprovisionamiento de usuarios para aplicaciones SaaS en Azure Active Directory](./customize-application-attributes.md).

Al configurar el aprovisionamiento para una aplicación SaaS, uno de los tipos de asignaciones de atributos que puede especificar es una asignación de expresiones. Para estas asignaciones, debe escribir una expresión similar a un script que permite transformar los datos de los usuarios en formatos más aceptables para la aplicación SaaS. Para detalles, consulte [Escritura de expresiones para la asignación de atributos en Azure Active Directory](functions-for-customizing-application-data.md).

## <a name="scoping"></a>Ámbito 
### <a name="assignment-based-scoping"></a>Ámbito basado en asignaciones

Para el aprovisionamiento saliente desde Azure AD a una aplicación SaaS, confiar en las [asignaciones de usuarios o grupos](../manage-apps/assign-user-or-group-access-portal.md) es la forma más común de determinar qué usuarios se encuentran en el ámbito para el aprovisionamiento. Debido a que las asignaciones de usuarios también se usan para habilitar el inicio de sesión único, se puede usar el mismo método para administrar tanto el acceso como el aprovisionamiento. El ámbito basado en la asignación no se aplica a escenarios de aprovisionamiento entrante como Workday y Successfactors.

* **Grupos.** Con un plan de licencia de Azure AD Premium, puede usar grupos para asignar acceso a una aplicación SaaS. Después, cuando el ámbito de aprovisionamiento se establezca en **Sincronizar solo los usuarios y grupos asignados**, el servicio de aprovisionamiento de Azure AD aprovisionará o desaprovisionará los usuarios en función de si son miembros de un grupo que está asignado a la aplicación. El objeto de grupo en sí no se aprovisiona a menos que la aplicación admita objetos de grupo. Asegúrese de que los grupos asignados a la aplicación tienen la propiedad "SecurityEnabled" establecida en "True".

* **Grupos dinámicos.** El servicio de aprovisionamiento de usuarios de Azure AD puede leer y aprovisionar usuarios en [grupos dinámicos](../enterprise-users/groups-create-rule.md). Tenga en cuenta estas advertencias y recomendaciones:

  * Los grupos dinámicos puede afectar al rendimiento del aprovisionamiento de un extremo a otro desde Azure AD a las aplicaciones SaaS.

  * La velocidad con la se aprovisiona o desaprovisiona un usuario de un grupo dinámico en una aplicación SaaS depende de la rapidez con la que el grupo dinámico pueda evaluar los cambios de pertenencia. Para información acerca de cómo comprobar el estado de procesamiento de un grupo dinámico, consulte [Creación de un grupo dinámico y comprobación de su estado](../enterprise-users/groups-create-rule.md).

  * Cuando un usuario pierde la pertenencia en el grupo dinámico, se considera un evento de desaprovisionamiento. Tenga en cuenta este escenario cuando cree reglas para grupos dinámicos.

* **Grupos anidados.** El servicio de aprovisionamiento de usuarios de Azure AD no puede leer o aprovisionar usuarios en grupos anidados. El servicio solo puede leer y aprovisionar aquellos usuarios que son miembros inmediatos de un grupo asignado explícitamente. Esta limitación de las "asignaciones basadas en grupos a aplicaciones" también afecta al inicio de sesión único (consulte [Uso de un grupo para administrar el acceso a las aplicaciones SaaS](../enterprise-users/groups-saasapps.md)). En lugar de ello, asigne explícitamente o defina de otro modo el [ámbito en](define-conditional-rules-for-provisioning-user-accounts.md) los grupos que contengan los usuarios que se deban aprovisionar.

### <a name="attribute-based-scoping"></a>Ámbito basado en atributos 

Puede usar filtros de ámbito para definir reglas basadas en atributos que determinen qué usuarios se aprovisionan en una aplicación. Este método se suele utilizar para aprovisionamiento de entrada desde aplicaciones HCM a Azure AD y Active Directory. Los filtros de ámbito se configuran como parte de las asignaciones de atributos de cada conector de aprovisionamiento de usuarios de Azure AD. Para detalles sobre la configuración de filtros de ámbito basados en atributos, consulte [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](define-conditional-rules-for-provisioning-user-accounts.md).

### <a name="b2b-guest-users"></a>Usuarios B2B (invitados)

El servicio de aprovisionamiento de usuarios de Azure AD se puede usar para aprovisionar usuarios de B2B (o invitados) en Azure AD para aplicaciones SaaS. Sin embargo, para que los usuarios de B2B inicien sesión en la aplicación SaaS mediante Azure AD, esta debe tener su funcionalidad de inicio de sesión único basado en SAML configurada de una forma concreta. Para más información acerca de cómo configurar aplicaciones SaaS para admitir inicios de sesión de usuarios de B2B, consulte [Configuración de aplicaciones de SaaS para la colaboración B2B](../external-identities/configure-saas-apps.md).

> [!NOTE]
El elemento userPrincipalName de un usuario invitado suele mostrarse como "alias#EXT#@domain.com". Si el elemento userPrincipalName se incluye en las asignaciones de atributos como atributo de origen, #EXT# se quita de dicho elemento. Si necesita que el elemento #EXT# esté presente, reemplace userPrincipalName por originalUserPrincipalName como atributo de origen. 

userPrincipalName = alias@domain.com originalUserPrincipalName = alias#EXT#@domain.com

## <a name="provisioning-cycles-initial-and-incremental"></a>Ciclos de aprovisionamiento: inicial e incremental.

Si Azure AD es el sistema de origen, el servicio de aprovisionamiento utiliza [Usar la consulta delta para realizar el seguimiento de los cambios en datos de Microsoft Graph](/graph/delta-query-overview) para supervisar usuarios y grupos. El servicio de aprovisionamiento ejecuta un ciclo inicial en el sistema de origen y el sistema de destino, seguido de ciclos incrementales periódicos.

### <a name="initial-cycle"></a>Ciclo inicial

Cuando se inicia el servicio de aprovisionamiento, en el primer ciclo:

1. Se consultan todos los usuarios y grupos del sistema de origen y se recuperan todos los atributos definidos en la [asignación de atributos](customize-application-attributes.md).

2. Se filtran los usuarios y grupos devueltos mediante cualquier [asignación](../manage-apps/assign-user-or-group-access-portal.md) o [filtro de ámbito basado en atributos](define-conditional-rules-for-provisioning-user-accounts.md) configurados.

3. Cuando un usuario se ha asignado o está en ámbito para el aprovisionamiento, el servicio consulta el sistema de destino en busca de un usuario coincidente mediante los [atributos coincidentes](customize-application-attributes.md#understanding-attribute-mapping-properties) especificados. Ejemplo: Si el nombre de userPrincipal en el sistema de origen es el atributo coincidente y se asigna a userName en el sistema de destino, el servicio de aprovisionamiento consulta el sistema de destino en busca de valores userName que coincidan con los valores de nombre del sistema de origen.

4. Si no se encuentra un usuario coincidente en el sistema de destino, se crea mediante los atributos que devuelve el sistema de origen. Una vez que se crea la cuenta de usuario, el servicio de aprovisionamiento detecta y almacena en caché el identificador del sistema de destino del nuevo usuario. Este identificador se usa para ejecutar todas las futuras operaciones en dicho usuario.

5. Si se encuentra un usuario coincidente, se actualiza mediante los atributos que proporciona el sistema de origen. Una vez que la cuenta de usuario coincide, el servicio de aprovisionamiento detecta y almacena en caché el identificador del sistema de destino para el nuevo usuario. Este identificador se usa para ejecutar todas las futuras operaciones en dicho usuario.

6. Si las asignaciones de atributos contienen atributos de "referencia", el servicio realiza actualizaciones adicionales en el sistema de destino para crear y vincular los objetos a los que se hace referencia. Por ejemplo, un usuario puede tener un atributo "Administrador" en el sistema de destino, que está vinculado a otro usuario creado en el sistema de destino.

7. Se conserva una marca de agua al final del ciclo inicial, que proporciona el punto de partida para los ciclos incrementales posteriores.

Algunas aplicaciones, como ServiceNow, G Suite y Box no solo admiten el aprovisionamiento de usuarios, sino también el de los grupos y sus miembros. En esos casos, si el aprovisionamiento de grupos está habilitado en las [asignaciones](customize-application-attributes.md), el servicio de aprovisionamiento sincroniza los usuarios y los grupos, y luego las pertenencias del grupo.

### <a name="incremental-cycles"></a>Ciclos incrementales

Después del ciclo inicial, todos los demás ciclos harán lo siguiente:

1. Se consulta el sistema de origen de los usuarios y grupos que se actualizaron desde la última marca de agua almacenada.

2. Se filtran los usuarios y grupos devueltos mediante cualquier [asignación](../manage-apps/assign-user-or-group-access-portal.md) o [filtro de ámbito basado en atributos](define-conditional-rules-for-provisioning-user-accounts.md) configurados.

3. Cuando un usuario se ha asignado o está en ámbito para el aprovisionamiento, el servicio consulta el sistema de destino en busca de un usuario coincidente mediante los [atributos coincidentes](customize-application-attributes.md#understanding-attribute-mapping-properties) especificados.

4. Si no se encuentra un usuario coincidente en el sistema de destino, se crea mediante los atributos que devuelve el sistema de origen. Una vez que se crea la cuenta de usuario, el servicio de aprovisionamiento detecta y almacena en caché el identificador del sistema de destino del nuevo usuario. Este identificador se usa para ejecutar todas las futuras operaciones en dicho usuario.

5. Si se encuentra un usuario coincidente, se actualiza mediante los atributos que proporciona el sistema de origen. Si es una cuenta recién asignada que coincide, el servicio de aprovisionamiento detecta y almacena en caché el identificador del sistema de destino para el nuevo usuario. Este identificador se usa para ejecutar todas las futuras operaciones en dicho usuario.

6. Si las asignaciones de atributos contienen atributos de "referencia", el servicio realiza actualizaciones adicionales en el sistema de destino para crear y vincular los objetos a los que se hace referencia. Por ejemplo, un usuario puede tener un atributo "Administrador" en el sistema de destino, que está vinculado a otro usuario creado en el sistema de destino.

7. Si un usuario que estaba anteriormente en ámbito del aprovisionamiento se quita del ámbito, lo que incluye estar sin asignar, el servicio deshabilita al usuario en el sistema de destino mediante una actualización.

8. Si un usuario que estaba anteriormente en ámbito del aprovisionamiento se deshabilita o se elimina temporalmente en el sistema de origen, el servicio deshabilita al usuario en el sistema de destino mediante una actualización.

9. Si un usuario que estaba anteriormente en ámbito del aprovisionamiento se elimina permanentemente en el sistema de origen, el servicio elimina al usuario en el sistema de destino. En Azure AD, los usuarios se eliminan permanentemente 30 días después de que se hayan eliminado temporalmente.

10. Se conserva una nueva marca de agua al final del ciclo incremental, que proporciona el punto de partida para los ciclos incrementales posteriores.

> [!NOTE]
> Opcionalmente, puede deshabilitar las operaciones **Crear**, **Actualizar** o **Eliminar** mediante las casillas **Acciones del objeto de destino** de la sección [Asignaciones](customize-application-attributes.md). La lógica para deshabilitar un usuario durante una actualización también se controla mediante una asignación de atributos desde un campo como "accountEnabled".

El servicio de aprovisionamiento sigue ejecutando ciclos incrementales opuestos de manera indefinida, según intervalos definidos en el [tutorial específico de cada aplicación](../saas-apps/tutorial-list.md). Los ciclos incrementales continúan hasta que se produzca uno de los siguientes eventos:

- El servicio se detiene manualmente mediante Azure Portal o por medio del comando de Microsoft Graph API adecuado.
- Se desencadena un nuevo ciclo inicial mediante la opción **Reiniciar aprovisionamiento** de Azure Portal o por medio del comando de Microsoft Graph API adecuado. Esta acción también borra cualquier marca de agua almacenada y hace que todos los objetos de origen se evalúen de nuevo.
- Se desencadena un nuevo ciclo inicial debido a un cambio en las asignaciones de atributos o los filtros de ámbito. Esta acción también borra cualquier marca de agua almacenada y hace que todos los objetos de origen se evalúen de nuevo.
- El proceso de aprovisionamiento entra en cuarentena (ver a continuación) debido a una alta tasa de errores y permanece en cuarentena durante más de cuatro semanas. En este caso, el servicio se deshabilita automáticamente.

### <a name="errors-and-retries"></a>Errores y reintentos

Si un error en el sistema de destino evita que un usuario individual se agregue, actualice o elimine en el sistema, la operación se reintenta en el siguiente ciclo de sincronización. Los errores se reintentan continuamente y la frecuencia de los reintentos se escala de forma gradual. Para resolver el error, los administradores deben comprobar los [registros de aprovisionamiento](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context) para determinar la causa principal y realizar la acción apropiada. Algunos errores comunes son:

- Usuarios que no tienen relleno un atributo en el sistema de origen que es necesario en el sistema de destino
- Usuarios que tienen un valor de atributo en el sistema de origen para el que existe una restricción única en el sistema de destino, y el mismo valor está presente en otro registro de usuario

Resuelva estos errores ajustando los valores de atributo del usuario afectado en el sistema de origen, o por medio del ajuste de las asignaciones de atributos para que no provoquen conflictos.

### <a name="quarantine"></a>Cuarentena

Si todas o la mayoría de las llamadas realizadas al sistema de destino no tienen éxito sistemáticamente debido a un error (por ejemplo, en el caso de credenciales de administrador no válidas), el trabajo de aprovisionamiento entra en estado de "cuarentena". Este estado se indica en el [informe de resumen de aprovisionamiento](./check-status-user-account-provisioning.md) y por correo electrónico si se han configurado las notificaciones por correo electrónico en Azure Portal.

En cuarentena, la frecuencia de los ciclos incrementales se reduce gradualmente a una vez al día.

El trabajo de aprovisionamiento sale de la cuarentena después de que se hayan resuelto todos los errores causantes y se inicie el siguiente ciclo de sincronización. Si el trabajo de aprovisionamiento permanece en cuarentena durante más de cuatro semanas, se deshabilita. [Aquí](./application-provisioning-quarantine-status.md) encontrará más información sobre el estado de cuarentena.

### <a name="how-long-provisioning-takes"></a>¿Cuánto tiempo tarda el aprovisionamiento?

El rendimiento depende de si el trabajo de aprovisionamiento ejecuta un ciclo de aprovisionamiento inicial o un ciclo incremental. Para obtener detalles sobre el tiempo que tarda el aprovisionamiento y cómo supervisar el estado del servicio de aprovisionamiento, consulte [Averiguar cuándo un usuario específico podrá acceder a una aplicación](application-provisioning-when-will-provisioning-finish-specific-user.md).

### <a name="how-to-tell-if-users-are-being-provisioned-properly"></a>¿Cómo sé si los usuarios se aprovisionan correctamente?

Todas las operaciones que ejecute el servicio de aprovisionamiento de usuarios se registran en los [registros de aprovisionamiento (versión preliminar)](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context) de Azure AD. Los registros incluyen todas las operaciones de lectura y escritura realizadas en los sistemas de origen y de destino, así como los datos del usuario que se leyeron o escribieron durante cada operación. Para obtener información sobre cómo leer los registros de aprovisionamiento en Azure Portal, consulte la [guía de informes de aprovisionamiento](./check-status-user-account-provisioning.md).

## <a name="de-provisioning"></a>Desaprovisionamiento
El servicio de aprovisionamiento de Azure AD mantiene los sistemas de origen y de destino sincronizados mediante el desaprovisionamiento de cuentas cuando se quita el acceso de usuario.

El servicio de aprovisionamiento admite la eliminación y deshabilitación de usuarios (opción a veces denominada "eliminación temporal"). La definición exacta de las opciones de deshabilitación y eliminación varía en función de la implementación de la aplicación de destino, pero normalmente una deshabilitación indica que el usuario no puede iniciar sesión. Una eliminación, en cambio, indica que el usuario ha eliminado completamente la aplicación. En el caso de las aplicaciones de SCIM, la deshabilitación es una solicitud para establecer la propiedad *Active* como "false" en un usuario. 

**Configuración de la aplicación para deshabilitar un usuario**

Asegúrese de que ha seleccionado la casilla de actualizaciones.

Asegúrese de que tiene la asignación en estado *Activa* de la aplicación. Si usa una aplicación de la galería de aplicaciones, la asignación puede ser ligeramente diferente. Asegúrese de usar la asignación predeterminada o predefinida para las aplicaciones de la galería.

:::image type="content" source="./media/how-provisioning-works/disable-user.png" alt-text="Deshabilitar un usuario" lightbox="./media/how-provisioning-works/disable-user.png":::


**Configuración de la aplicación para eliminar un usuario**

En los siguientes ejemplos se desencadenará una deshabilitación o una eliminación: 
* Un usuario se elimina temporalmente en Azure AD (esto es, se envía a la papelera de reciclaje o a la propiedad AccountEnabled establecida en "false").
    30 días después de que un usuario se elimine en Azure AD, se eliminará permanentemente del inquilino. En este punto, el servicio de aprovisionamiento enviará una solicitud DELETE para eliminar permanentemente al usuario en la aplicación. En cualquier momento durante la ventana de treinta días, puede [eliminar manualmente un usuario de forma permanente](../fundamentals/active-directory-users-restore.md), lo que envía una solicitud de eliminación a la aplicación.
* Un usuario se elimina o se quita permanentemente de la papelera de reciclaje en Azure AD.
* Un usuario no está asignado a una aplicación.
* Un usuario sale del ámbito establecido (deja de pasar el filtro de ámbito).

:::image type="content" source="./media/how-provisioning-works/delete-user.png" alt-text="Eliminar un usuario" lightbox="./media/how-provisioning-works/delete-user.png":::

De manera predeterminada, el servicio de aprovisionamiento de Azure AD elimina o deshabilita los usuarios que quedan fuera del alcance. Si desea invalidar este comportamiento predeterminado, puede establecer una marca para [omitir las eliminaciones fuera del ámbito](skip-out-of-scope-deletions.md).

Si se produce uno de los cuatro eventos anteriores y la aplicación de destino no admite eliminaciones temporales, el servicio de aprovisionamiento enviará una solicitud DELETE para eliminar permanentemente al usuario de la aplicación.

Si ve un atributo IsSoftDeleted en sus asignaciones de atributos, se utiliza para determinar el estado del usuario y si se debe enviar una solicitud de actualización con active = false para eliminar al usuario temporalmente.

**Restricciones conocidas**

* Si un usuario que haya administrado previamente el servicio de aprovisionamiento deja de estar asignado a una aplicación o a un grupo asignado a una aplicación, se enviará una solicitud de deshabilitación. En ese momento, el servicio deja de administrar al usuario y no se enviará una solicitud de eliminación cuando se elimine del directorio.
* No se admite el aprovisionamiento de un usuario que esté deshabilitado en Azure AD. Debe estar activo en Azure AD antes de que aprovisionarlo.
* Cuando un usuario pasa de ser eliminado de forma temporal a un estado "activo", el servicio de aprovisionamiento de Azure AD activará el usuario en la aplicación de destino, pero no restaurará automáticamente las pertenencias a grupos. La aplicación de destino debe mantener las pertenencias a grupos del usuario en estado inactivo. Si la aplicación de destino no lo admite, puede reiniciar el aprovisionamiento para actualizar las pertenencias a grupos. 

**Recomendación**

Al desarrollar una aplicación, admita siempre eliminaciones permanentes y temporales. Gracias a esto, los clientes pueden recuperarse cuando se deshabilita un usuario accidentalmente.


## <a name="next-steps"></a>Pasos siguientes

[Planeamiento de una implementación del aprovisionamiento automático de usuarios](../app-provisioning/plan-auto-user-provisioning.md)

[Configuración del aprovisionamiento en una aplicación de la galería](./configure-automatic-user-provisioning-portal.md)

[Compilación de un punto de conexión de SCIM y configuración del aprovisionamiento cuando crea su propia aplicación](../app-provisioning/use-scim-to-provision-users-and-groups.md)

[Problema al configurar el aprovisionamiento de usuarios para una aplicación de la galería de Azure AD](./application-provisioning-config-problem.md).
