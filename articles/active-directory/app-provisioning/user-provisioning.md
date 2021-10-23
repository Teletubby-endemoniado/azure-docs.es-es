---
title: ¿Qué es el aprovisionamiento automatizado de usuarios de aplicaciones SaaS en Azure Active Directory?
description: Una introducción al uso de Azure Active Directory para el aprovisionamiento, el desaprovisionamiento y la actualización continua de cuentas de usuario de manera automática en varias aplicaciones SaaS de terceros.
services: active-directory
author: kenwith
manager: karenh444
ms.service: active-directory
ms.subservice: app-provisioning
ms.topic: overview
ms.workload: identity
ms.date: 05/28/2021
ms.author: kenwith
ms.reviewer: arvinh
ms.openlocfilehash: 78533b519c2410744b3ec8adc7dd3269e018715f
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "129990565"
---
# <a name="what-is-app-provisioning-in-azure-active-directory"></a>¿Qué es el aprovisionamiento de aplicaciones en Azure Active Directory?

En Azure Active Directory (Azure AD), el término *aprovisionamiento de aplicaciones* hace referencia a la creación automática de identidades y roles de usuario para las aplicaciones.
    
![Diagrama que muestra escenarios de aprovisionamiento.](../governance/media/what-is-provisioning/provisioning.png)

El aprovisionamiento de aplicaciones de Azure AD para software como servicio (SaaS) hace referencia a la creación automática de identidades y roles de usuario en las aplicaciones de nube ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) a las que los usuarios necesitan acceder. Además de crear identidades de usuario, el aprovisionamiento automático incluye el mantenimiento y la eliminación de identidades de usuario a medida que el estado o los roles cambian. Algunos escenarios comunes son el aprovisionamiento de un usuario de Azure AD en aplicaciones como [Dropbox](../../active-directory/saas-apps/dropboxforbusiness-provisioning-tutorial.md), [Salesforce](../../active-directory/saas-apps/salesforce-provisioning-tutorial.md) y [ServiceNow](../../active-directory/saas-apps/servicenow-provisioning-tutorial.md), entre otras.

Azure AD admite el aprovisionamiento de usuarios en aplicaciones SaaS y en aplicaciones hospedadas en el entorno local o en una solución de infraestructura como servicio (IaaS), como una máquina virtual. Puede tener una aplicación heredada que se base en un almacén de usuarios LDAP o en una base de datos SQL. Al usar el servicio de aprovisionamiento de Azure AD, puede crear, actualizar y eliminar usuarios en aplicaciones locales sin tener que abrir firewalls ni trabajar con puertos TCP. 

Con agentes ligeros, puede aprovisionar usuarios en las aplicaciones locales y regular el acceso. Cuando se usa Azure AD con el proxy de aplicación, puede administrar el acceso a la aplicación local y proporcionar aprovisionamiento automático de usuarios (con el servicio de aprovisionamiento) e inicio de sesión único (con el proxy de aplicación). 

El aprovisionamiento de aplicaciones le permite:

- **Aprovisionamiento automatizado**: Crear cuentas automáticamente en los sistemas adecuados para los usuarios nuevos cuando se unen a su equipo u organización.
- **Desaprovisionamiento automatizado**: desactivar las cuentas automáticamente en los sistemas adecuados cuando los usuarios dejan el equipo o la organización.
- **Sincronización de datos entre sistemas**: asegurarse de que las identidades de las aplicaciones y sistemas se mantienen actualizadas con los cambios del directorio o el sistema de recursos humanos.
- **Aprovisionamiento de grupos:** aprovisionar grupos para las aplicaciones que los admitan.
- **Control del acceso**: supervise y audite a quién se ha aprovisionado en las aplicaciones.
- **Implementación completa en escenarios que parten de recursos existentes:** hacer coincidir las identidades actuales entre los sistemas y permitir una integración sencilla, incluso cuando los usuarios ya existen en el sistema de destino.
- **Uso de numerosos elementos de personalización:** aprovechar las asignaciones de atributos personalizables que definen qué datos del usuario deben fluir del sistema de origen al sistema de destino.
- **Obtención de alertas para eventos críticos:** el servicio de aprovisionamiento proporciona alertas para eventos críticos y permite la integración de Log Analytics, donde puede definir alertas personalizadas a la medida de las necesidades de su empresa.

## <a name="what-is-scim"></a>¿Qué es SCIM?

Para ayudar a automatizar el aprovisionamiento y el desaprovisionamiento, las aplicaciones exponen las API de grupo y de usuario propietario. Sin embargo, cualquier persona que intente administrar usuarios en más de una aplicación le indicará que cada una intenta realizar las mismas acciones, como crear o actualizar usuarios, agregar usuarios a grupos o desaprovisionar a usuarios. Aún así, todas estas acciones se implementan de forma ligeramente diferente, con distintas rutas de acceso de puntos de conexión, distintos métodos para especificar la información del usuario y un esquema distinto para representar cada elemento de información.

Para abordar estos desafíos, la especificación de System for Cross-domain Identity Management (SCIM) proporciona un esquema de usuario común para ayudar a los usuarios a entrar, salir o moverse por las aplicaciones. SCIM se está convirtiendo en el estándar de facto para aprovisionar y, cuando se usa junto con estándares de federación, como el Lenguaje de marcado de aserción de seguridad (SAML) u OpenID Connect (OIDC), ofrece a los administradores una solución de un extremo a otro basada en estándares para la administración del acceso.

Para ver instrucciones detalladas acerca de cómo desarrollar un punto de conexión de SCIM para automatizar el aprovisionamiento y el desaprovisionamiento de usuarios y grupos en una aplicación, consulte [Creación de un punto de conexión de SCIM y configuración del aprovisionamiento de usuarios](use-scim-to-provision-users-and-groups.md). En el caso de las aplicaciones previamente integradas en la galería, como Slack, Azure Databricks y Snowflake, puede omitir la documentación para desarrolladores y usar los tutoriales que se proporcionan en [Tutoriales para integrar aplicaciones SaaS con Azure Active Directory](../../active-directory/saas-apps/tutorial-list.md).

## <a name="manual-vs-automatic-provisioning"></a>Aprovisionamiento manual y automático

Las aplicaciones de la galería de Azure AD admiten uno de los dos modos de aprovisionamiento:

* El aprovisionamiento **manual** indica que todavía no hay ningún conector de aprovisionamiento de Azure AD automático para la aplicación. Las cuentas de usuario deben crearse manualmente. Por ejemplo, agregar usuarios directamente en el portal administrativo de la aplicación o cargar una hoja de cálculo con los detalles de estas cuentas. Consulte la documentación suministrada por la aplicación o póngase en contacto con su desarrollador para determinar qué mecanismos hay disponibles.
* **Automático** significa que se ha desarrollado un conector de aprovisionamiento de Azure AD para esta aplicación. Siga el tutorial de configuración específico para configurar el aprovisionamiento de la aplicación. Los tutoriales sobre aplicaciones están disponibles en [Lista de tutoriales para integrar aplicaciones SaaS con Azure Active Directory](../../active-directory/saas-apps/tutorial-list.md).

El modo de aprovisionamiento compatible con una aplicación también se puede ver en la pestaña **Aprovisionamiento** una vez que se ha agregado la aplicación a las aplicaciones empresariales.

## <a name="benefits-of-automatic-provisioning"></a>Ventajas del aprovisionamiento automático

A medida que el número de aplicaciones usadas en las organizaciones modernas sigue creciendo, los administradores de TI tienen tareas de administración de acceso a escala. Estándares como SAML u OIDC permiten a los administradores configurar rápidamente el inicio de sesión único (SSO), pero el acceso también requiere el aprovisionamiento de los usuarios en la aplicación. Para muchos administradores, el aprovisionamiento significa crear manualmente cada cuenta de usuario o cargar los archivos CSV cada semana. Estos procesos son lentos, caros y propensos a errores Se han adoptado soluciones como Just-In-Time (JIT) de SAML para automatizar el aprovisionamiento. Las empresas también necesitan una solución que permita desaprovisionar a los usuarios cuando abandonan la organización o ya no necesitan acceso a determinadas aplicaciones según el cambio de roles.

Algunos de los motivos comunes para usar el aprovisionamiento automático incluyen:

- Maximizar la eficacia y la precisión de los procesos de aprovisionamiento.
- El ahorro en los costos asociados al hospedaje y el mantenimiento de scripts y soluciones de aprovisionamiento desarrollados de forma personalizada.
- Proteger su organización mediante la eliminación instantánea de las identidades de los usuarios de aplicaciones SaaS claves cuando estos abandonan la organización.
- Importar fácilmente una gran cantidad de usuarios a una aplicación o sistema SaaS concretos.
- Tener un único conjunto de directivas para determinar quién se aprovisiona y quién puede iniciar sesión en una aplicación.

El aprovisionamiento de usuarios de Azure AD puede ayudar a resolver estos desafíos. Para más información sobre cómo usan los clientes el aprovisionamiento de usuarios de Azure AD, lea el [caso práctico de ASOS](https://aka.ms/asoscasestudy). El vídeo siguiente proporciona información general sobre el aprovisionamiento de usuarios en Azure AD.

> [!VIDEO https://www.youtube.com/embed/_ZjARPpI6NI]

## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>¿Qué aplicaciones y sistemas puedo usar con el aprovisionamiento automático de usuarios de Azure AD?

Azure AD ofrece compatibilidad preintegrada con muchas aplicaciones SaaS y sistemas de recursos humanos populares, así como compatibilidad genérica con aplicaciones que implementan determinadas partes del [estándar SCIM 2.0](https://techcommunity.microsoft.com/t5/Identity-Standards-Blog/Provisioning-with-SCIM-getting-started/ba-p/880010).

* **Aplicaciones previamente integradas (aplicaciones SaaS de la galería)** : puede encontrar todas las aplicaciones para las que Azure AD admite un conector de aprovisionamiento integrado previamente en [Tutoriales para integrar aplicaciones SaaS con Azure Active Directory](../saas-apps/tutorial-list.md). Las aplicaciones integradas previamente que se enumeran en la galería suelen usar las API de administración de usuarios basadas en SCIM 2.0 para el aprovisionamiento. 

   ![Imagen que muestra logotipos de DropBox, Salesforce y otros.](./media/user-provisioning/gallery-app-logos.png)

   Si quiere solicitar una nueva aplicación para el aprovisionamiento, puede [solicitar que la aplicación se integre con la galería de aplicaciones](../develop/v2-howto-app-gallery-listing.md). Si se da una solicitud de aprovisionamiento de usuarios, es necesario que la aplicación tenga un punto de conexión compatible con SCIM. Solicite al proveedor de la aplicación que siga el estándar SCIM para que podamos incorporar la aplicación a nuestra plataforma rápidamente.

* **Aplicaciones que admiten SCIM 2.0**: para obtener información sobre cómo conectar genéricamente aplicaciones que implementan API de administración de usuarios basadas en SCIM 2.0, vea [Tutorial: Desarrollo y planeación del aprovisionamiento de un punto de conexión de SCIM en Azure Active Directory](use-scim-to-provision-users-and-groups.md).

## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>¿Cómo configuro el aprovisionamiento automático para una aplicación?

En el caso de las aplicaciones preintegradas que se enumeran en la galería, se ofrece una guía paso a paso para configurar el aprovisionamiento automático. Vea [Tutoriales para integrar aplicaciones SaaS con Azure Active Directory](../saas-apps/tutorial-list.md). En el vídeo siguiente se muestra cómo configurar el aprovisionamiento automático de usuarios para SalesForce.

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

Para otras aplicaciones que admiten SCIM 2.0, siga los pasos descritos en [Tutorial: Desarrollo y planeación del aprovisionamiento de un punto de conexión de SCIM en Azure Active Directory](use-scim-to-provision-users-and-groups.md).


## <a name="next-steps"></a>Pasos siguientes

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS](../saas-apps/tutorial-list.md)
- [Personalización de asignaciones de atributos para el aprovisionamiento de usuarios](customize-application-attributes.md)
- [Filtros de ámbito para el aprovisionamiento de usuarios](define-conditional-rules-for-provisioning-user-accounts.md)
