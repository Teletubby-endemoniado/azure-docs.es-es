---
title: Planeamiento de una implementación de revisiones de acceso de Azure Active Directory
description: Guía de planeamiento para la correcta implementación de revisiones de acceso.
services: active-directory
documentationCenter: ''
author: ajburnle
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 04/16/2021
ms.author: ajburnle
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: fbfd88d0ddb75b96e6d8ef48790aefdea76833dc
ms.sourcegitcommit: 2ed2d9d6227cf5e7ba9ecf52bf518dff63457a59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132517641"
---
# <a name="plan-an-azure-active-directory-access-reviews-deployment"></a>Planeamiento de una implementación de revisiones de acceso de Azure Active Directory

[Las revisiones de acceso de Azure Active Directory (Azure AD)](access-reviews-overview.md) ayudan a su organización a mantener la red más segura mediante la administración del [ciclo de vida de acceso a los recursos](identity-governance-overview.md). Las revisiones de acceso permiten:

* Programar revisiones periódicas o hacer revisiones ad hoc para ver quién tiene acceso a recursos específicos (por ejemplo, aplicaciones y grupos).
* Hacer un seguimiento de las revisiones con fines de información, cumplimiento normativo o directivas.
* Delegar las revisiones a administradores específicos, propietarios de empresas o usuarios que puedan atestiguar su necesidad de acceso continuado.
* Usar la información para determinar eficazmente si los usuarios deben seguir teniendo acceso.
* Automatizar los resultados de las revisiones (por ejemplo, eliminar el acceso de los usuarios a los recursos).

  ![Diagrama que muestra el flujo de revisiones de acceso.](./media/deploy-access-review/1-planning-review.png)

Las revisiones de acceso son una funcionalidad de [Azure AD Identity Governance](identity-governance-overview.md). Otras funcionalidades son la [administración de derechos](entitlement-management-overview.md), la [Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md) y las [condiciones de uso](../conditional-access/terms-of-use.md). Juntas, le ayudarán a abordar estas cuatro preguntas:

* ¿Qué usuarios deben tener acceso a qué recursos?
* ¿Qué hacen esos usuarios con el acceso concedido?
* ¿La organización dispone de un sistema de control eficaz para administrar el acceso?
* ¿Los auditores pueden comprobar que los controles funcionan?

Planear la implementación de revisiones de acceso es esencial, pues así podrá garantizar el cumplimiento de la estrategia de gobernanza prevista para los usuarios de su organización.

### <a name="key-benefits"></a>Ventajas principales

Las principales ventajas de habilitar las revisiones de acceso son:

* **Control de la colaboración:** las revisiones de acceso permiten administrar el acceso a todos los recursos que necesitan los usuarios. Cuando los usuarios comparten información y colaboran, usted tiene la garantía de que únicamente los usuarios autorizados manejan la información.
* **Administración del riesgo**: Las revisiones de acceso le ofrecen una forma de revisar el acceso a los datos y a las aplicaciones. Así se reduce el riesgo de pérdida o filtración de datos. Además, tiene la capacidad de revisar periódicamente el acceso de los asociados externos a los recursos corporativos.
* **Cumplimiento y gobernanza**: con las revisiones de acceso, puede regular y volver a certificar el ciclo de vida del acceso a grupos, aplicaciones y sitios. Es posible regular y controlar las revisiones con fines de cumplimiento o para aplicaciones sensibles al riesgo específicas de su organización.
* **Reducción del coste**: Las revisiones de acceso se crean en la nube y son compatibles de forma nativa con otros recursos en la nube (por ejemplo, grupos, aplicaciones y paquetes de acceso). El uso de revisiones de acceso es más barato que desarrollar herramientas propias o actualizar su conjunto de herramientas local.

### <a name="training-resources"></a>Recursos de aprendizaje

Estos vídeos le ayudarán a obtener información sobre las revisiones de acceso:

* [¿Qué son las revisiones de acceso de Azure AD?](https://youtu.be/kDRjQQ22Wkk)
* [Cómo crear revisiones de acceso en Azure AD](https://youtu.be/6KB3TZ8Wi40)
* [Cómo crear revisiones de acceso automáticas para todos los usuarios invitados con acceso a grupos de Microsoft 365 en Azure AD](https://www.youtube.com/watch?v=3D2_YW2DwQ8)
* [Cómo habilitar las revisiones de acceso en Azure AD](https://youtu.be/X1SL2uubx9M)
* [Cómo revisar el acceso mediante Mi acceso](https://youtu.be/tIKdQhdHLXU)

### <a name="licenses"></a>Licencias

Se necesita una licencia válida de Azure AD Premium (P2) para cada persona que quiera crear o hacer revisiones de acceso, exceptuando a los administradores globales o a los administradores de usuarios. Para más información, consulte los [requisitos de licencia de las revisiones de acceso](access-reviews-overview.md).

Además, es posible que necesite utilizar otras características de Identity Governance, por ejemplo la [administración del ciclo de vida de los derechos](entitlement-management-overview.md) o la Privileged Identity Management (PIM). En tal caso, puede que también necesite licencias relacionadas. Para más información, consulte los [precios de Azure Active Directory](https://www.microsoft.com/security/business/identity-access-management/azure-ad-pricing).

## <a name="plan-the-access-reviews-deployment-project"></a>Planeamiento del proyecto de implementación de revisiones de acceso

A la hora de determinar la estrategia de implementación de las revisiones de acceso en su entorno, tenga en cuenta las necesidades de su organización.

### <a name="engage-the-right-stakeholders"></a>Interactuar con las partes interesadas adecuadas

Cuando fracasan los proyectos tecnológicos, normalmente se debe a expectativas incorrectas relacionadas con el impacto, los resultados y las responsabilidades. Para evitar estos problemas, [asegúrese de que interactúa con las partes interesadas adecuadas](../fundamentals/active-directory-deployment-plans.md) y que los roles del proyecto están claros.

En el caso de las revisiones de acceso, probablemente tenga que incluir a representantes de los siguientes equipos de su organización:

* **Administración de TI**: administra la infraestructura de TI y las inversiones de su empresa en la nube, junto con las aplicaciones de software como servicio (SaaS). Este equipo:

   * Revisa el acceso con privilegios a la infraestructura y las aplicaciones, incluidos Microsoft 365 y Azure AD.
   * Programa y ejecuta revisiones de acceso en grupos que se usan para mantener listas de excepciones o en proyectos piloto de TI para mantener las listas de acceso actualizadas.
   * Se asegura de que el acceso mediante programación (scripts) a los recursos a través de entidades de servicio se regule y revise.

* **Equipos de desarrollo**: desarrollan y mantienen las aplicaciones para su organización. Este equipo:

   * Controla quién puede acceder a los componentes de SaaS y a los recursos de plataforma como servicio (PaaS) y de infraestructura como servicio (IaaS) que componen las soluciones desarrolladas.
   * Administra los grupos que tienen acceso a aplicaciones y herramientas para el desarrollo de aplicaciones internas.
   * Exige las identidades con privilegios que tienen acceso a software de producción o a soluciones que están hospedadas para sus clientes.

* **Unidades de negocio**: administran proyectos y tienen aplicaciones propias. Este equipo:

   * Revisa y aprueba, o bien deniega, el acceso a grupos y aplicaciones para usuarios internos y externos.
   * Programa y hace revisiones para atestiguar la necesidad de acceso continuado de empleados e identidades externas (por ejemplo, asociados).

* **Gobernanza corporativa**: garantiza que la organización sigue las directivas internas y cumple la normativa. Este equipo:

   * Solicita o programa nuevas revisiones de acceso.
   * Evalúa los procesos y procedimientos para revisar el acceso (por ejemplo, la documentación y el mantenimiento de registros con fines de cumplimiento).
   * Examina los resultados de las revisiones anteriores para los recursos más críticos.

> [!NOTE]
> Para las revisiones que requieren evaluaciones manuales, hay que disponer de revisores y ciclos de revisión adecuados de acuerdo con sus necesidades de cumplimiento y directivas. Si los ciclos de revisión son demasiado frecuentes, o hay pocos revisores, la calidad podría disminuir. Esto podría llevar a que demasiadas o muy pocas personas tengan acceso.

### <a name="plan-communications"></a>Planeamiento de las comunicaciones

La comunicación es fundamental para que cualquier nuevo proceso de negocio tenga éxito. Hay que comunicar proactivamente a los usuarios cómo y cuándo cambiará su experiencia. Indíqueles cómo obtener soporte técnico si tienen problemas.

#### <a name="communicate-changes-in-accountability"></a>Comunicar cambios que afectan a la rendición de cuentas

Las revisiones de acceso permiten a los propietarios de empresa cambiar la responsabilidad de revisar y tomar medidas en relación con el acceso continuado. Las decisiones sobre el acceso son más precisas al desvincularlas del departamento de TI. Esto supone un cambio cultural en términos de rendición de cuentas y responsabilidad del propietario del recurso. Comunique de forma proactiva este cambio y asegúrese de que los propietarios de recursos estén preparados y capacitados para utilizar la información con el fin de tomar buenas decisiones.

El departamento de TI querrá mantener el control sobre las decisiones de acceso a toda la infraestructura. También sobre la asignación de roles con privilegios.

#### <a name="customize-email-communication"></a>Comunicación personalizada por correo electrónico

Al programar una revisión, hay que designar a los usuarios que se encargarán de hacerla. A continuación, se notifica a estos revisores, por correo electrónico, que tienen nuevas revisiones asignadas. También reciben recordatorios antes de que expire la revisión que tienen asignada.

El correo electrónico enviado a los revisores puede personalizarse para incluir un breve mensaje que les anime a trabajar en la revisión. Use el texto adicional para:

* Incluir un mensaje personal para los revisores. Así sabrán que se lo envía el departamento de TI o de cumplimiento.
* Incluir una referencia a información interna sobre cuáles son las expectativas de la revisión, junto con material de referencia o aprendizaje adicional.

  ![Captura de pantalla que muestra un correo electrónico del revisor.](./media/deploy-access-review/2-plan-reviewer-email.png)

Después de seleccionar **Iniciar revisión**, los revisores serán dirigidos al [portal Mi acceso](https://myapplications.microsoft.com/) para las revisiones de acceso de grupos y aplicaciones. El portal les proporciona una visión general de todos los usuarios que tienen acceso al recurso que están revisando, junto con recomendaciones del sistema basadas en información sobre el último inicio de sesión y acceso.

### <a name="plan-a-pilot"></a>Planeamiento de un piloto

Se recomienda a los clientes que inicialmente prueben las revisiones de acceso con un grupo pequeño y que se centren en recursos que no sean críticos. La prueba piloto puede ayudar a ajustar los procesos y las comunicaciones según sea necesario. Puede ayudar a aumentar la capacidad de los usuarios y los revisores para cumplir con los requisitos de seguridad y cumplimiento.

Para la prueba piloto, se recomienda lo siguiente:

* Comenzar con revisiones en las que los resultados no se apliquen automáticamente, y que permitan controlar las repercusiones.
* Asegurarse de que todos los usuarios tienen direcciones de correo electrónico válidas en Azure AD. Confirmar que reciben la comunicación por correo electrónico para realizar la acción adecuada.
* Tomar nota de cualquier acceso que se haya eliminado como parte de la prueba piloto por si necesita restaurarlo rápidamente.
* Supervisar los registros de auditoría para asegurarse de que todos los eventos se auditan correctamente.

Para más información, consulte los [procedimientos recomendados para una prueba piloto](../fundamentals/active-directory-deployment-plans.md).

## <a name="introduction-to-access-reviews"></a>Introducción a las revisiones de acceso

En esta sección se presentan los conceptos relacionados con las revisiones de acceso que debería conocer antes de planear revisiones.

### <a name="what-resource-types-can-be-reviewed"></a>¿Qué tipos de recursos se pueden revisar?

Después de integrar los recursos de su organización con Azure AD (por ejemplo, usuarios, aplicaciones y grupos), se pueden administrar y revisar.

Entre los objetivos típicos de revisión, se incluyen:

* [Aplicaciones integradas con Azure AD para el inicio de sesión único](../manage-apps/what-is-application-management.md) (por ejemplo, aplicaciones SaaS o de línea de negocio).
* La [pertenencia](../fundamentals/active-directory-manage-groups.md?context=azure%2factive-directory%2fusers-groups-roles%2fcontext%2fugr-context) a un grupo, sincronizado con Azure AD o creado en Azure AD o Microsoft 365, incluido Microsoft Teams.
* Un [paquete de acceso](./entitlement-management-overview.md) que agrupa recursos (por ejemplo, grupos, aplicaciones y sitios) en un único paquete para administrar el acceso.
* [Roles de Azure AD y roles de recursos de Azure](../privileged-identity-management/pim-resource-roles-assign-roles.md), tal y como se definen en la Privileged Identity Management (PIM).

### <a name="who-will-create-and-manage-access-reviews"></a>¿Quién puede crear y administrar las revisiones de acceso?

El rol administrativo que se necesita para crear, administrar o leer una revisión de acceso depende del tipo de recurso que se esté revisando. La tabla siguiente muestra los roles necesarios para cada tipo de recurso.

| Tipo de recurso| Crear y administrar revisiones de acceso (creadores)| Leer los resultados de la revisión de acceso |
| - | - | -|
| Grupo o aplicación| Administrador global <p>Administrador de usuarios<p>Administrador de Identity Governance<p>Administrador de roles con privilegios (solo hace revisiones para grupos de Azure AD que puedan tener roles asignados)<p>Propietario del grupo ([si lo habilita un administrador)]( create-access-review.md#allow-group-owners-to-create-and-manage-access-reviews-of-their-groups-preview)| Administrador global<p>Lector global<p>Administrador de usuarios<p>Administrador de Identity Governance<p>Administrador de roles con privilegios<p>Lector de seguridad<p>Propietario del grupo ([si lo habilita un administrador)]( create-access-review.md#allow-group-owners-to-create-and-manage-access-reviews-of-their-groups-preview) |
|Roles de Azure AD| Administrador global <p>Administrador de roles con privilegios|  Administrador global<p>Lector global<p>Administrador de usuarios<p>Administrador de roles con privilegios<p> <p>Lector de seguridad |
| Roles de recursos de Azure| Administrador global<p>Propietario del recurso| Administrador global<p>Lector global<p>Administrador de usuarios<p>Administrador de roles con privilegios<p> <p>Lector de seguridad  |
| Paquete de acceso| Administrador global<p>Administrador de usuarios<p>Administrador de Identity Governance| Administrador global<p>Lector global<p>Administrador de usuarios<p>Administrador de Identity Governance<p> <p>Lector de seguridad  |

Para más información, consulte los [permisos del rol de administrador en Azure AD](../roles/permissions-reference.md).

### <a name="who-will-review-the-access-to-the-resource"></a>¿Quién revisará el acceso al recurso?

El creador de la revisión de acceso decide, en el momento de crearla, quién hará la revisión. Esta configuración no se puede cambiar después de que se inicie la revisión. Los revisores se representan mediante:

* Los propietarios de recursos, que son las empresas propietarias del recurso.
* Los delegados, seleccionados de forma individual por el administrador de revisiones de acceso.
* Los usuarios, que atestiguarán su necesidad de acceso continuado.
* Los administradores revisan el acceso de sus informes directos al recurso.

Al crear una revisión de acceso, los administradores pueden elegir uno o más revisores. Todos los revisores pueden iniciar y llevar a cabo una revisión; y elegir los usuarios que seguirán teniendo acceso a un recurso, o bien eliminarlos.

### <a name="components-of-an-access-review"></a>Componentes de una revisión de acceso

Antes de implementar las revisiones de acceso, debe planear los tipos de revisión relevantes para su organización. Para ello, deberá tomar decisiones empresariales sobre lo que desea revisar y las medidas que deberán tomarse en función de esas revisiones.

Para crear una directiva de revisión de acceso, debe disponer de la información siguiente:

* ¿Cuáles son los recursos que se van a revisar?
* ¿Qué acceso se va revisar?
* ¿Con qué frecuencia se debe realizar la revisión?
* ¿Quién hará la revisión?

   * ¿Cómo se le notificará la revisión?
   * ¿Cuáles son las escalas de tiempo que se aplicarán a la revisión?

* ¿Qué medidas automáticas deberían aplicarse en función de la revisión?
   * ¿Qué ocurre si el revisor no responde a tiempo?

* ¿Qué medidas manuales deberán aplicarse como consecuencia de los resultados de la revisión?
* ¿Qué comunicaciones deben enviarse en función de las medidas adoptadas?

#### <a name="example-access-review-plan"></a>Ejemplo de un plan de revisión de acceso

| Componente| Valor |
| - | - |
| Recursos para revisar| Acceso a Microsoft Dynamics. |
| Frecuencia de revisión| Mensual. |
| Quién hace la revisión| Administradores de programas del grupo de negocios de Dynamics. |
| Notificación| El correo electrónico se envía al principio de una revisión al alias Dynamics-Pms.<p>Incluir un mensaje personalizado que motive a los revisores para garantizar su aceptación. |
| Escala de tiempo| 48 horas después de la notificación. |
|Acciones automáticas| Se elimina el acceso a cualquier cuenta que no haya tenido un inicio de sesión interactivo en 90 días. Para ello, quite al usuario del grupo de seguridad dynamics-access. <p>*Tome medidas si la revisión no se realiza dentro de la escala de tiempo prevista.* |
| Medidas manuales| Si lo desean, los revisores pueden aprobar eliminaciones antes de la medida automatizada. |

### <a name="automate-actions-based-on-access-reviews"></a>Automatización de medidas basadas en las revisiones de acceso

Para automatizar la eliminación de accesos, establezca la opción **Aplicar automáticamente los resultados al recurso** en **Habilitar**.

  ![Captura de pantalla que muestra el planeamiento de las revisiones de acceso.](./media/deploy-access-review/3-automate-actions-settings.png)

Cuando la revisión esté completa y finalizada, los usuarios que no hayan recibido aprobación del revisor se eliminarán automáticamente del recurso o seguirán con acceso continuado. Esto podría llevarse a cabo con la eliminación de su pertenencia a grupos o de su asignación a aplicaciones. También con la revocación del derecho a obtener un rol con privilegios.

### <a name="take-recommendations"></a>Aceptar recomendaciones

Las recomendaciones se muestran a los revisores como parte de su labor. Señalan el último inicio de sesión de un usuario en la cuenta empresarial o su último acceso a una aplicación. Esta información ayuda a los revisores a tomar la decisión de acceso adecuada. Si se selecciona la opción **Aceptar recomendaciones**, se aplicarán las recomendaciones de la revisión de acceso. Al final de una revisión de acceso, el sistema aplica estas recomendaciones automáticamente a los usuarios que no hayan sido gestionados por los revisores.

Las recomendaciones se basan en los criterios de la revisión de acceso. Por ejemplo, si la revisión se configura para que elimine el acceso en caso de que no se haya producido un inicio de sesión interactivo durante 90 días, se recomendará eliminar a todos los usuarios que coincidan con esos criterios. Microsoft trabaja continuamente para mejorar las recomendaciones.

### <a name="review-guest-user-access"></a>Revisión del acceso de usuarios invitados

Se pueden utilizar las revisiones de acceso para revisar y limpiar las identidades de los asociados de organizaciones externas. Configurar una revisión por cada asociado podría satisfacer los requisitos de cumplimiento.

A las identidades externas se les puede conceder acceso a los recursos de la empresa. Pueden ser:

* Agregadas a un grupo.
* Invitadas a Teams.
* Asignadas a una aplicación empresarial o a un paquete de acceso.
* Asignadas a un rol con privilegios en Azure AD o en una suscripción de Azure.

Para más información, consulte este[script de ejemplo](https://github.com/microsoft/access-reviews-samples/tree/master/ExternalIdentityUse). El script mostrará dónde se usan las identidades externas invitadas a la cuenta empresarial. Puede consultar la pertenencia a grupos, las asignaciones de roles y las asignaciones de aplicaciones del usuario externo en Azure AD. El script no mostrará ninguna asignación fuera de Azure AD, como la asignación de derechos directos a los recursos de SharePoint, sin utilizar grupos.

Al crear una revisión de acceso para grupos o aplicaciones, podrá permitir que el revisor se centre en **Todos los usuarios con acceso** o que compruebe **Solo usuarios invitados**. Al seleccionar **Solo usuarios invitados**, los revisores recibirán una lista específica de las identidades externas de Azure AD B2B (colaboración entre negocios) que tienen acceso al recurso.

 ![Captura de pantalla que muestra la revisión de los usuarios invitados.](./media/deploy-access-review/4-review-guest-users-admin-ui.png)

> [!IMPORTANT]
> Esta lista *no incluirá* a miembros externos que tengan el **atributo** **userType**. Esta lista *tampoco incluirá* usuarios invitados fuera de Azure AD B2B (colaboración entre negocios). Un ejemplo son los usuarios que tienen acceso al contenido compartido directamente a través de SharePoint.

## <a name="plan-access-reviews-for-access-packages"></a>Planeamiento de revisiones de acceso para paquetes de acceso

Los [paquetes de acceso](entitlement-management-overview.md) pueden simplificar enormemente su estrategia de gobernanza y de revisión de acceso. Un paquete de acceso agrupa todos los recursos e incluye el acceso que un usuario necesita para trabajar en un proyecto o hacer sus funciones. Por ejemplo, tal vez desee crear un paquete de acceso que incluya todas las aplicaciones que los desarrolladores de su organización necesitan, o bien todas las aplicaciones a las que los usuarios externos deberían tener acceso. A continuación, un administrador, o un administrador de paquetes de acceso delegado, agrupará los recursos (grupos, aplicaciones y sitios) y los roles que los usuarios necesitan para utilizar esos recursos.

Al [crear un paquete de acceso](entitlement-management-access-package-create.md), podrá definir una o varias directivas de acceso para establecer las condiciones en las que los usuarios pueden solicitar un paquete de acceso, determinar cómo será el proceso de aprobación y especificar la frecuencia con la que un usuario tendrá que volver a solicitar acceso. Las revisiones de acceso se configuran al crear o editar una directiva de paquete de acceso.

Seleccione la pestaña **Ciclo de vida** y desplácese hacia abajo hasta Revisiones de acceso.

 ![Captura de pantalla que muestra la pestaña Ciclo de vida.](./media/deploy-access-review/5-plan-access-packages-admin-ui.png)

## <a name="plan-access-reviews-for-groups"></a>Planeamiento de revisiones de acceso para grupos

Además de los paquetes de acceso, revisar la pertenencia a los grupos es la forma más eficaz de regular el acceso. Se pueden asignar accesos a los recursos a través de [Grupos de seguridad o Grupos de Microsoft 365](../fundamentals/active-directory-manage-groups.md). Agregue usuarios a esos grupos para obtener acceso.

Un único grupo puede obtener acceso a todos los recursos pertinentes. Se puede asignar acceso al grupo para recursos individuales, o bien a un paquete de acceso que agrupe aplicaciones y otros recursos. Este método permite revisar el acceso al grupo en lugar del acceso de un usuario a cada aplicación.

La pertenencia a grupos pueden revisarla los siguientes perfiles:

* Administradores.
* Propietarios del grupo.
* Usuarios seleccionados, con capacidad de revisión delegada al crear la revisión.
* Miembros del grupo, que atestiguarán su necesidad de acceso.
* Los administradores que revisan el acceso de sus informes directos.

### <a name="group-ownership"></a>Propiedad del grupo

Los propietarios del grupo revisan la pertenencia porque están mejor calificados para saber quién necesita acceso. La propiedad de los grupos cambia según el tipo de grupo:

* Los grupos creados en Microsoft 365 y Azure AD tienen uno o más propietarios bien definidos. En la mayoría de los casos, estos propietarios son los revisores idóneos para sus grupos, ya que saben quién debe tener acceso.

   Por ejemplo, Microsoft Teams usa grupos de Microsoft 365 como el modelo de autorización subyacente para conceder a los usuarios acceso a los recursos disponibles en SharePoint, Exchange, OneNote u otros servicios de Microsoft 365. El creador del equipo se convierte automáticamente en propietario y debería ser responsable de atestiguar la pertenencia a ese grupo.

* Los grupos que se crean manualmente en el portal de Azure AD o mediante scripting a través de Microsoft Graph no necesitan tener propietarios definidos. Puede definirlos mediante el portal de Azure AD, en la sección **Propietarios** del grupo, o a través de Microsoft Graph.

* Los grupos que se sincronizan desde una instancia local de Active Directory no pueden tener un propietario en Azure AD. Al crear una revisión de acceso para esos grupos, seleccione a las personas más adecuadas para decidir quién debe pertenecer a ellos.

> [!NOTE]
> Defina directivas empresariales que determinen cómo se crean los grupos para establecer su propiedad y rendición de cuentas de forma clara. Así se podrá revisar periódicamente la pertenencia a estos grupos.

### <a name="review-membership-of-exclusion-groups-in-conditional-access-policies"></a>Revisión de la pertenencia a grupos de exclusión en las directivas de acceso condicional

Para obtener información sobre cómo revisar la pertenencia a grupos de exclusión, consulte [Usar las revisiones de acceso de Azure AD para administrar los usuarios excluidos de las directivas de acceso condicional](conditional-access-exclusion.md).

### <a name="review-guest-users-group-memberships"></a>Revisión de la pertenencia a grupos de usuarios invitados

Para obtener información sobre cómo revisar el acceso de los usuarios invitados a pertenencias a grupos, consulte [Administración del acceso de los invitados con las revisiones de acceso de Azure AD](./manage-guest-access-with-access-reviews.md).

### <a name="review-access-to-on-premises-groups"></a>Revisión del acceso a grupos locales

Las revisiones de acceso no permiten cambiar la pertenencia a grupos que se sincronicen desde el entorno local con [Azure AD Connect](../hybrid/whatis-azure-ad-connect.md). Esta restricción se debe a que el origen de la autoridad es local.

No obstante, puede utilizar las revisiones de acceso para programar y mantener revisiones periódicas de los grupos locales. Posteriormente, los revisores tomarán las medidas pertinentes respecto al grupo local. Esta estrategia utiliza las revisiones de acceso como una herramienta para todas las revisiones.

Puede usar los resultados de una revisión de acceso en grupos locales y procesarlos aún más. Para ello:

* Descargue el informe CSV de la revisión de acceso y aplique la medida manualmente.
* Utilice Microsoft Graph para acceder mediante programación a los resultados y las decisiones de las revisiones de acceso completadas.

Por ejemplo, para acceder a los resultados de un grupo administrado mediante Windows AD, utilice este [script de ejemplo de PowerShell](https://github.com/microsoft/access-reviews-samples/tree/master/AzureADAccessReviewsOnPremises). El script describe las llamadas necesarias a Microsoft Graph y exporta los comandos de Windows AD-PowerShell para hacer los cambios.

## <a name="plan-access-reviews-for-applications"></a>Planeamiento de revisiones de acceso para aplicaciones

Al revisar el acceso a una aplicación, se revisa el acceso de empleados e identidades externas a la información y los datos de la aplicación. Lleve a cabo una revisión cuando necesite saber quién tiene acceso a una aplicación específica, en lugar de a un paquete de acceso o a un grupo.

Planifique revisiones para aplicaciones en los escenarios siguientes:

* Los usuarios tienen acceso directo a una aplicación (al margen de un grupo o un paquete de acceso).
* La aplicación expone información crítica o confidencial.
* La aplicación tiene requisitos específicos de cumplimiento que usted debe atestiguar.
* Existen sospechas de acceso inadecuado.

Para crear revisiones de acceso para una aplicación, establezca la opción **¿Se requiere la asignación de usuarios?** en **Sí**. Si se establece en **No**, todos los usuarios del directorio, incluidas las identidades externas, podrán acceder a la aplicación y usted no podrá revisar el acceso a la aplicación.

 ![Captura de pantalla que muestra el planeamiento de asignaciones para aplicaciones.](./media/deploy-access-review/6-plan-applications-assignment-required.png)

A continuación,[asigne los usuarios y grupos](../manage-apps/assign-user-or-group-access-portal.md) a los que quiera conceder el acceso.

### <a name="reviewers-for-an-application"></a>Revisores de una aplicación

Las revisiones de acceso pueden ser para los miembros de un grupo o para los usuarios que fueron asignados a una aplicación. Las aplicaciones de Azure AD no necesitan tener un propietario. Esta es la razón por la que no se puede elegir al propietario de la aplicación como revisor. El ámbito de una revisión se puede limitar aún más para revisar únicamente a los usuarios invitados asignados a una aplicación, en lugar de revisar todos los accesos.

## <a name="plan-review-of-azure-ad-and-azure-resource-roles"></a>Planeamiento de una revisión de roles de Azure AD y de Azure Resource

[Privileged Identity Management](../privileged-identity-management/pim-configure.md) simplifica el modo en que las empresas administran el acceso con privilegios a los recursos de Azure AD. El uso de PIM reduce en gran medida la lista de roles con privilegios en [Azure AD](../roles/permissions-reference.md)y en[recursos de Azure](../../role-based-access-control/built-in-roles.md). También aumenta la seguridad general del directorio.

Las revisiones de acceso permiten a los revisores atestiguar si los usuarios todavía necesitan pertenecer a un rol. Al igual que sucede con las revisiones de acceso para los paquetes de acceso, las revisiones de roles en Azure AD y en recursos de Azure se integran en la experiencia de usuario del administrador de PIM.

Revise periódicamente las siguientes asignaciones de roles:

* Administrador global
* Administrador de usuarios
* Administrador de autenticación con privilegios
* Administrador de acceso condicional
* Administrador de seguridad
* Todos los roles de administración del servicio Dynamics y de Microsoft 365

Los roles que se revisan incluyen asignaciones permanentes y elegibles.

En la sección **Revisores**, seleccione una o más personas para que revisen a todos los usuarios. También puede seleccionar **Miembros (autoasignado)** para que los miembros revisen su propio acceso.

 ![Captura de pantalla que muestra la selección de revisores.](./media/deploy-access-review/7-plan-azure-resources-reviewers-selection.png)

## <a name="deploy-access-reviews"></a>Implementación de revisiones de acceso

Después de preparar una estrategia y un plan para revisar el acceso a los recursos integrados con Azure AD, implemente y administre las revisiones mediante los siguientes recursos.

### <a name="review-access-packages"></a>Revisión de paquetes de acceso

Para reducir el riesgo de acceso por parte de dispositivos obsoletos, los administradores pueden habilitar revisiones periódicas de los usuarios que tienen asignaciones activas en un paquete de acceso. Siga las instrucciones de los artículos enumerados en la tabla.

| Artículos de procedimientos| Descripción |
| - | - |
| [Crear revisiones de acceso](entitlement-management-access-reviews-create.md)| Habilite las revisiones de un paquete de acceso. |
| [Hacer revisiones de acceso](entitlement-management-access-reviews-review-access.md)| Haga revisiones de acceso para otros usuarios asignados a un paquete de acceso. |
| [Autorrevisar paquetes de acceso asignado](entitlement-management-access-reviews-self-review.md)| Haga una autorrevisión de paquetes de acceso asignado. |

> [!NOTE]
> Los usuarios que, en una autorrevisión, informan de que ya no necesitan acceso, no son eliminados del paquete de acceso inmediatamente. Se les elimina del paquete de acceso cuando la revisión finaliza o si un administrador la detiene.

### <a name="review-groups-and-apps"></a>Revisión de grupos y aplicaciones

Las necesidades de acceso a grupos y aplicaciones para empleados e invitados probablemente cambien a lo largo del tiempo. Para reducir el riesgo asociado a las asignaciones de acceso obsoletas, los administradores pueden crear revisiones de acceso para los miembros de un grupo o para el acceso a aplicaciones. Siga las instrucciones de los artículos enumerados en la tabla.

| Artículos de procedimientos| Descripción |
| - | - |
| [Crear revisiones de acceso](create-access-review.md)| Puede crear una o varias revisiones de acceso para los miembros de un grupo o para el acceso a aplicaciones. |
| [Hacer revisiones de acceso](perform-access-review.md)| Haga una revisión de acceso para los miembros de un grupo o para los usuarios con acceso a una aplicación. |
| [Autorrevisión del acceso](review-your-access.md)| Permita que los miembros revisen su propio acceso a un grupo o a una aplicación. |
| [Finalización de una revisión de acceso](complete-access-review.md)| Visualice una revisión de acceso y aplique los resultados. |
| [Tomar medidas para los grupos locales](https://github.com/microsoft/access-reviews-samples/tree/master/AzureADAccessReviewsOnPremises)| Use un script de ejemplo de PowerShell para decidir según los resultados de las revisiones de acceso para grupos locales. |

### <a name="review-azure-ad-roles"></a>Revisión de roles de Azure AD

Para reducir el riesgo asociado a las asignaciones de roles obsoletas, revise periódicamente el acceso a roles de Azure AD con privilegios.

![Captura de pantalla que muestra la lista "Pertenencia a la revisión" de los roles de Azure AD.](./media/deploy-access-review/8-review-azure-ad-roles-picker.png)

Siga las instrucciones de los artículos enumerados en la tabla.

| Artículos de procedimientos | Descripción |
| - | - |
 [Crear revisiones de acceso](../privileged-identity-management/pim-create-azure-ad-roles-and-resource-roles-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)| Puede crear revisiones de acceso para los roles de Azure AD con privilegios en PIM. |
| [Autorrevisión del acceso](../privileged-identity-management/pim-perform-azure-ad-roles-and-resource-roles-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)| Si se le ha asignado un rol administrativo, apruebe o deniegue el acceso al rol. |
| [Finalización de una revisión de acceso](../privileged-identity-management/pim-complete-azure-ad-roles-and-resource-roles-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)| Visualice una revisión de acceso y aplique los resultados. |

### <a name="review-azure-resource-roles"></a>Revisión de roles de los recursos de Azure

Para reducir el riesgo asociado a las asignaciones de roles obsoletas, revise periódicamente el acceso a roles de recursos de Azure con privilegios.

![Captura de pantalla que muestra la revisión de los roles de Azure AD.](./media/deploy-access-review/9-review-azure-roles-picker.png)

Siga las instrucciones de los artículos enumerados en la tabla.

| Artículos de procedimientos| Descripción |
| - | -|
| [Crear revisiones de acceso](../privileged-identity-management/pim-create-azure-ad-roles-and-resource-roles-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)| Puede crear revisiones de acceso para roles de recursos de Azure con privilegios en PIM. |
| [Autorrevisión del acceso](../privileged-identity-management/pim-perform-azure-ad-roles-and-resource-roles-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)| Si se le ha asignado un rol administrativo, apruebe o deniegue el acceso al rol. |
| [Finalización de una revisión de acceso](../privileged-identity-management/pim-complete-azure-ad-roles-and-resource-roles-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)| Visualice una revisión de acceso y aplique los resultados. |

## <a name="use-the-access-reviews-api"></a>Uso de la API de revisiones de acceso

Para interactuar con los recursos revisables y administrarlos, consulte los [métodos de Graph API](/graph/api/resources/accessreviewsv2-root?view=graph-rest-beta&preserve-view=true) y las [comprobaciones de autorización de roles y aplicaciones](/graph/api/resources/accessreviewsv2-root?view=graph-rest-beta&preserve-view=true). Los métodos de revisiones de acceso de Microsoft Graph API están disponibles para contextos de aplicación y usuario. Al ejecutar scripts en el contexto de aplicación, se debe conceder el permiso "AccessReview.Read.All" a la cuenta utilizada para ejecutar la API (la entidad de servicio) para consultar la información sobre revisiones de acceso.

Algunas tareas de revisiones de acceso que se suelen automatizar mediante Microsoft Graph API son:

* Crear e iniciar una revisión de acceso.
* Finalizar manualmente una revisión de acceso antes de la finalización programada.
* Enumerar todas las revisiones de acceso en ejecución e indicar su estado.
* Ver el historial de una serie de revisiones, junto con las decisiones y las medidas tomadas en cada revisión.
* Recopilar las decisiones de una revisión de acceso.
* Recopilar las decisiones de revisiones completadas en las que el revisor tomó una decisión distinta a la recomendada por el sistema.

Al crear nuevas consultas de Microsoft Graph API para la automatización, use [Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer). Puede compilar y explorar las consultas de Microsoft Graph antes de incluirlas en scripts y código. Esto puede ayudarle a iterar rápidamente la consulta para que obtenga exactamente los resultados que espera, sin cambiar el código del script.

## <a name="monitor-access-reviews"></a>Supervisión de revisiones de acceso

Las actividades de revisión de acceso se registran y están disponibles en los [registros de auditoría de Azure AD](../reports-monitoring/concept-audit-logs.md). Puede filtrar los datos de auditoría según la categoría, el tipo de actividad y el intervalo de fechas. Esta es una consulta de ejemplo.

| Categoría| Directiva |
| - | - |
| Tipo de actividad| Creación de revisión de acceso |
| | Actualizar revisión de acceso |
| | Revisión de acceso finalizada |
| | Eliminación de revisión de acceso |
| | Aprobar decisión |
| | Denegar decisión |
| | Restablecer decisión |
| | Aplicar decisión |
| Intervalo de fechas| Siete días |

Para consultas y análisis más avanzados de las revisiones de acceso, y para hacer un seguimiento de los cambios y la finalización de las revisiones, exporte los registros de auditoría de Azure AD a [Azure Log Analytics](../reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md) o a Azure Event Hubs. Cuando los registros de auditoría se almacenan en Log Analytics, es posible usar el [lenguaje de análisis eficaz](../reports-monitoring/howto-analyze-activity-logs-log-analytics.md) y crear sus propios paneles.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las siguientes tecnologías relacionadas:

* [¿Qué es la administración de derechos de Azure AD?](entitlement-management-overview.md)
* [¿Qué es Azure AD Privileged Identity Management?](../privileged-identity-management/pim-configure.md)