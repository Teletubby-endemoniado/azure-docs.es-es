---
title: Introducción a los conectores para Azure Logic Apps
description: Obtenga información sobre los conectores y cómo le ayudan a crear flujos de trabajo de integración automatizados de forma rápida y sencilla mediante Azure Logic Apps.
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, azla
ms.topic: conceptual
ms.date: 09/13/2021
ms.custom: contperf-fy21q4
ms.openlocfilehash: cccd744e8c123cd9441ff9aca47d2341ea9d80fb
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128679870"
---
# <a name="about-connectors-in-azure-logic-apps"></a>Acerca de los conectores en Azure Logic Apps

Al crear flujos de trabajo mediante Azure Logic Apps, puede usar *conectores* para acceder de forma rápida y sencilla a datos, eventos y recursos en otras aplicaciones, servicios, sistemas, protocolos y plataformas, a menudo sin escribir código. Un conector proporciona operaciones precompiladas que puede usar como pasos en los flujos de trabajo. Azure Logic Apps proporciona cientos de conectores que puede utilizar. Si no hay ningún conector disponible para el recurso al que desea acceder, puede usar la operación HTTP genérica para comunicarse con el servicio, o puede crear un [conector personalizado](#custom-apis-and-connectors).

Esta información general ofrece una introducción a los conectores, cómo funcionan normalmente y los conectores más populares y usados con más frecuencia en Azure Logic Apps. Para más información, revise la siguiente documentación:

* [Información general de conectores para Azure Logic Apps, Microsoft Power Automate y Microsoft Power Apps](/connectors)
* [Referencia de conectores para Azure Logic Apps](/connectors/connector-reference/connector-reference-logicapps-connectors)
* [Modelos de precios y facturación en Azure Logic Apps](../logic-apps/logic-apps-pricing.md)
* [Detalles sobre los precios de Azure Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/)

## <a name="what-are-connectors"></a>¿Qué son los conectores?

Técnicamente, un conector es un proxy o un contenedor alrededor de una API que el servicio subyacente usa para comunicarse con Azure Logic Apps. Este conector proporciona operaciones que se usan en los flujos de trabajo para realizar tareas. Una operación está disponible como *desencadenador* o *acción* con las propiedades que puede configurar. Algunos desencadenadores y acciones también requieren que primero [cree y configure una conexión](#connection-configuration) con el servicio o sistema subyacente, por ejemplo, para que pueda autenticar el acceso a una cuenta de usuario.

### <a name="triggers"></a>Desencadenadores

Un *desencadenador* especifica el evento que inicia el flujo de trabajo y siempre es el primer paso de cualquier flujo de trabajo. Todos los desencadenadores también siguen un patrón de activación específico que controla cómo el desencadenador supervisa y responde a los eventos. Normalmente, un desencadenador sigue el patrón de *sondeo* o el patrón de *inserción*, pero a veces, un desencadenador está disponible en ambas versiones.

- Los *desencadenadores de sondeo* comprueban periódicamente un servicio o sistema específico según una programación especificada, para comprobar si hay nuevos datos o un evento específico. Si hay nuevos datos disponibles o se produce un evento específico, estos desencadenadores crean y ejecutan una nueva instancia del flujo de trabajo. Esta nueva instancia puede usar los datos que se pasan como entrada.
- Los *desencadenadores de inserción* escuchan datos nuevos o si se produce un evento, sin tener que realizar un sondeo. Si hay nuevos datos disponibles o se produce un evento, estos desencadenadores crean y ejecutan una nueva instancia del flujo de trabajo. Esta nueva instancia puede usar los datos que se pasan como entrada.

Por ejemplo, es posible que quiera crear un flujo de trabajo que se active cuando se carga un archivo en el servidor FTP. Como primer paso del flujo de trabajo, puede usar el desencadenador FTP denominado **Cuando se agrega o modifica un archivo**, que sigue el patrón de sondeo. A continuación, puede especificar una programación para comprobar periódicamente si hay eventos de carga.

Un desencadenador también pasa todas las entradas y otros datos necesarios al flujo de trabajo, donde las acciones posteriores pueden hacer referencia a los datos y usarlos a lo largo del flujo de trabajo. Por ejemplo, supongamos que quiere usar el desencadenador de Office 365 Outlook denominado **Cuando llega un nuevo correo electrónico** para iniciar un flujo de trabajo cuando recibe un nuevo correo electrónico. Puede configurar este desencadenador para que se active según el contenido de cada nuevo correo electrónico, como el remitente, la línea de asunto, el cuerpo, los datos adjuntos, etc. A continuación, el flujo de trabajo puede procesar esa información con otras acciones.

### <a name="actions"></a>Acciones

Una *acción* es una operación que sigue al desencadenador y realiza algún tipo de tarea en el flujo de trabajo. Puede usar varias acciones en el flujo de trabajo. Por ejemplo, puede iniciar el flujo de trabajo con un desencadenador de SQL que detecte nuevos datos de cliente en una base de datos SQL. Después del desencadenador, el flujo de trabajo puede tener una acción SQL que obtendrá los datos del cliente. Después de la acción SQL, el flujo de trabajo puede tener otra acción, no necesariamente SQL, que procese los datos.

## <a name="connector-categories"></a>Categorías del conector

En Azure Logic Apps, la mayoría de los desencadenadores y acciones están disponibles en una versión *integrada* o de *conector administrado*. Existen unos pocos desencadenadores y acciones disponibles en ambas versiones. Las versiones disponibles dependen de si va a crear una aplicación lógica multiinquilino o de inquilino único, que actualmente solo está disponible en [Azure Logic Apps de inquilino único](../logic-apps/single-tenant-overview-compare.md).

Los [desencadenadores y las acciones integrados](built-in.md) se ejecutan de forma nativa en el entorno de ejecución de Logic Apps, y no requieren la creación de conexiones; asimismo, realizan estos tipos de tareas:

- [Ejecución del código en los flujos de trabajo](built-in.md#run-code-from-workflows).
- [Organización y control de los datos](built-in.md#control-workflow).
- [Administración o manipulación de los datos](built-in.md#manage-or-manipulate-data).

Microsoft se encarga de implementar, hospedar y administrar los [conectores administrados](managed.md). Estos conectores proporcionan desencadenadores y acciones para los servicios en la nube, los sistemas locales o ambos. Los conectores administrados están disponibles en estas categorías:

- [Conectores locales](managed.md#on-premises-connectors), que le ayudan a obtener acceso a los datos y recursos en sistemas locales.
- [Conectores empresariales](managed.md#enterprise-connectors) que proporcionan acceso a sistemas empresariales.
- [Conectores de cuentas de integración](managed.md#integration-account-connectors), que admiten escenarios de comunicación de negocio a negocio (B2B).
- [Conectores del Entorno del servicio de integración (ISE)](managed.md#ise-connectors), que son un pequeño grupo de [conectores administrados disponibles solo para ISE](#ise-and-connectors).

<a name="connection-configuration"></a>

## <a name="connection-configuration"></a>Configuración de la conexión

Si desea crear o administrar conexiones y recursos de aplicaciones lógicas, necesita ciertos permisos que se proporcionan a través de roles mediante el [control de acceso basado en rol de Azure (RBAC de Azure)](../role-based-access-control/role-assignments-portal.md). Puede asignar roles integrados o personalizados a los miembros que tienen acceso a la suscripción de Azure. Azure Logic Apps tiene estos roles específicos:

* [Colaborador de aplicación lógica](../role-based-access-control/built-in-roles.md#logic-app-contributor): Le permite administrar aplicaciones lógicas, pero no puede cambiar el acceso a ellas.

* [Operador de aplicación lógica](../role-based-access-control/built-in-roles.md#logic-app-operator): Le permite leer, habilitar y deshabilitar aplicaciones lógicas, pero no puede editarlas ni actualizarlas.

* [Colaborador](../role-based-access-control/built-in-roles.md#contributor): concede acceso completo para administrar todos los recursos, pero no le permite asignar roles en RBAC de Azure, administrar asignaciones en Azure Blueprints ni compartir galerías de imágenes.

  Por ejemplo, imagine que tiene que trabajar con una aplicación lógica que no ha creado y autenticar las conexiones usadas que usa el flujo de trabajo de esa aplicación lógica. La suscripción de Azure necesita permisos de colaborador para el grupo de recursos que contiene el recurso de esa aplicación lógica. Si crea un recurso de aplicación lógica, tendrá acceso de colaborador automáticamente.

Antes de que pueda usar los desencadenadores o las acciones de un conector en el flujo de trabajo, la mayoría de los conectores requieren que primero cree una *conexión* al servicio o sistema de destino. Para crear una conexión desde un flujo de trabajo de aplicación lógica, debe autenticar su identidad con las credenciales de cuenta y, a veces, usar otra información de conexión. Por ejemplo, para que el flujo de trabajo obtenga acceso a la cuenta de correo electrónico de Office 365 Outlook y trabaje con ella, debe autorizar una conexión a esa cuenta. Si se trata de una pequeña cantidad de operaciones integradas y conectores administrados, puede [configurar una identidad administrada para la autenticación y usarla](../logic-apps/create-managed-service-identity.md#triggers-actions-managed-identity) en lugar de proporcionar sus credenciales.

<a name="connection-security-encyrption"></a>

### <a name="connection-security-and-encryption"></a>Cifrado y seguridad de conexión

Los detalles de la configuración de la conexión, como la dirección del servidor, el nombre de usuario y la contraseña, las credenciales y los secretos [se cifran y almacenan en el entorno protegido de Azure](../security/fundamentals/encryption-overview.md). Esta información solo se puede usar en los recursos de la aplicación lógica y solo pueden usarla los clientes que tienen permisos para el recurso de conexión, que se aplica mediante comprobaciones de acceso vinculado. Las conexiones que utilizan Azure Active Directory Open Authentication (Azure AD OAuth), como Office 365, Salesforce y GitHub, requieren que inicie sesión, pero Azure Logic Apps solo almacena tokens de acceso y actualización como secretos, no credenciales de inicio de sesión.

Las conexiones establecidas pueden obtener acceso al servicio o sistema de destino, siempre que ese servicio o sistema lo permita. En el caso de los servicios que usan conexiones de Azure AD OAuth, como Office 365 y Dynamics, el servicio Logic Apps actualiza los tokens de acceso de forma indefinida. Es posible que otros servicios tengan límites con respecto a cuánto tiempo puede usar Logic Apps un token sin actualizar. Algunas acciones invalidarán todos los tokens de acceso como, por ejemplo, el cambio de la contraseña.

Aunque cree conexiones desde un flujo de trabajo, las conexiones son recursos de Azure independientes que cuentan con sus propias definiciones de recursos. Para revisar estas definiciones de recursos de conexión, puede [descargar la aplicación lógica desde Azure en Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md). Este método es la forma más sencilla de crear una plantilla válida de aplicación lógica con parámetros que esté prácticamente lista para la implementación.

> [!TIP]
> Si su organización no le permite acceder a recursos específicos a través de los conectores de Logic Apps, puede [bloquear la capacidad de crear dichas conexiones](../logic-apps/block-connections-connectors.md) mediante [Azure Policy](../governance/policy/overview.md).

Para más información sobre cómo proteger conexiones y aplicaciones lógicas, consulte [Protección del acceso y los datos en Azure Logic Apps](../logic-apps/logic-apps-securing-a-logic-app.md).

<a name="firewall-access"></a>

### <a name="firewall-access-for-connections"></a>Acceso de firewall para conexiones

Si usa un firewall que limita el tráfico y los flujos de trabajo de la aplicación lógica necesitan comunicarse a través de dicho firewall, debe configurarlo para permitir el acceso a las direcciones IP de [entrada](../logic-apps/logic-apps-limits-and-config.md#inbound) y [salida](../logic-apps/logic-apps-limits-and-config.md#outbound) que usa el servicio Logic Apps o el entorno de ejecución en la región de Azure donde se encuentran los flujos de trabajo de la aplicación lógica. Si los flujos de trabajo también usan conectores administrados, como el conector de Office 365 Outlook o el conector de SQL, o emplean conectores personalizados, el firewall también debe permitir el acceso a *todas* las [direcciones IP de salida del conector administrado](/connectors/common/outbound-ip-addresses#azure-logic-apps) en la región de Azure de la aplicación lógica. Para obtener más información, revise [Configuración de firewall](../logic-apps/logic-apps-limits-and-config.md#firewall-configuration-ip-addresses-and-service-tags).

## <a name="recurrence-behavior"></a>Comportamiento de periodicidad

Los desencadenadores integrados recurrentes, como el [Desencadenador de periodicidad](connectors-native-recurrence.md), se ejecutan de forma nativa en el entorno de ejecución de Logic Apps y son diferentes de los desencadenadores periódicos basados en conexiones, como el desencadenador del conector de Office 365 Outlook, donde primero debe crear una conexión.

Sin embargo, para ambos tipos de desencadenadores, si una periodicidad no proporciona una fecha y hora de inicio específicas, la primera periodicidad se ejecuta inmediatamente al guardar o implementar la aplicación lógica, independientemente de la configuración de periodicidad del desencadenador. Para evitar este comportamiento, proporcione una fecha y hora de inicio para cuando quiera que se ejecute la primera periodicidad.

### <a name="recurrence-for-built-in-triggers"></a>Periodicidad de los desencadenadores integrados

Los desencadenadores periódicos integrados siguen la programación que establezca, incluida cualquier zona horaria especificada. Sin embargo, si una periodicidad no especifica ninguna otra opción de programación avanzada, como horas específicas para ejecutar futuras repeticiones, esas repeticiones se basan en la última ejecución del desencadenador. Como resultado, las horas de inicio de estas periodicidades pueden cambiar debido a factores como la latencia durante las llamadas de almacenamiento.

Para más información, revise la siguiente documentación:

* [Programación y ejecución de tareas, procesos y flujos de trabajo automatizados y periódicos con Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md)
* [Creación, programación y ejecución de tareas y flujos de trabajo periódicos con el desencadenador de periodicidad](connectors-native-recurrence.md)
* [Solución de problemas de periodicidad](#recurrence-issues)

### <a name="recurrence-for-connection-based-triggers"></a>Periodicidad de los desencadenadores basados en conexión

En cuanto a los desencadenadores periódicos basados en conexiones, como Office 365 Outlook, la programación no es el único controlador que administra la ejecución. Asimismo, la zona horaria solo determina la hora de inicio inicial. Las ejecuciones posteriores dependen de la programación de periodicidad, de la última ejecución del desencadenador, y de otros factores que pueden provocar que haya un desfase o un comportamiento inesperado en los tiempos de ejecución, por ejemplo:

* Si el desencadenador tiene acceso a un servidor que tiene más datos, que el desencadenador intenta capturar inmediatamente.
* Los errores o reintentos en que incurre el desencadenador.
* La latencia durante las llamadas de almacenamiento.
* No mantener la programación especificada cuando se inicia y finaliza el horario de verano (DST).
* Otros factores que pueden afectar al siguiente tiempo de ejecución.

Para más información, revise la siguiente documentación:

* [Programación y ejecución de tareas, procesos y flujos de trabajo automatizados y periódicos con Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md)
* [Solución de problemas de periodicidad](#recurrence-issues)

<a name="recurrence-issues"></a>

### <a name="troubleshooting-recurrence-issues"></a>Solución de problemas de periodicidad

Para asegurarse de que el flujo de trabajo se ejecuta a la hora de inicio especificada y no pierde periodicidad, especialmente cuando la frecuencia se especifica en días o unidades superiores, pruebe con estas soluciones:

* Cuando se aplica el horario de verano, ajuste manualmente la periodicidad para que el flujo de trabajo siga ejecutándose en el momento esperado. De lo contrario, la hora de inicio se desplazará una hora hacia delante cuando se inicie el DST y una hora hacia atrás cuando finalice el DST. Para obtener más información y ejemplos, revise [Periodicidad de horario de verano y hora estándar](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#daylight-saving-standard-time).

* Si usa un desencadenador de **periodicidad**, especifique una zona horaria y una fecha y hora de inicio. Asimismo, configure las horas específicas en las que se ejecutarán las repeticiones posteriores mediante las propiedades denominadas **A estas horas** y **En estos minutos**, que solo están disponibles para las frecuencias **Día** y **Semana**. Sin embargo, es posible que algunas ventanas de tiempo sigan provocando problemas cuando se cambia la hora.

* Considere la posibilidad de usar un desencadenador de [**Ventana deslizante**](connectors-native-sliding-window.md) en lugar de un desencadenador de **periodicidad** para evitar periodicidades perdidas.

## <a name="custom-apis-and-connectors"></a>Conectores y API personalizadas

Para llamar a las API que ejecutan código personalizado o que no están disponibles como conectores, puede extender la plataforma de Logic Apps mediante la [creación de aplicaciones de API personalizadas](../logic-apps/logic-apps-create-api-app.md). También puede [crear conectores personalizados](../logic-apps/custom-connector-overview.md) para cualquier API REST o SOAP, para que esas API estén disponibles para cualquier aplicación lógica en su suscripción de Azure. Para que las aplicaciones de API o los conectores personalizados sean públicos y cualquier persona pueda usarlos en Azure, [envíe los conectores para que Microsoft los certifique](/connectors/custom-connectors/submit-certification).

## <a name="ise-and-connectors"></a>ISE y conectores

En el caso de los flujos de trabajo que necesitan acceso directo a los recursos de una red virtual de Azure, puede crear un [entorno del servicio de integración (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) dedicado en el que puede compilar, implementar y ejecutar los flujos de trabajo en recursos dedicados. Para más información sobre cómo crear ISE, revise [Conexión a redes virtuales de Azure desde Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).

Los conectores personalizados creados dentro de un ISE no funcionan con la puerta de enlace de datos local. Pero estos conectores pueden acceder directamente a orígenes de datos locales que están conectados a una red virtual de Azure en la que se hospeda el ISE. Por tanto, es muy probable que las aplicaciones lógicas en un ISE no necesiten la puerta de enlace de datos cuando se comuniquen con esos recursos. Si tiene conectores personalizados que creó fuera de un ISE que requieran una puerta de enlace de datos local, las aplicaciones lógicas de un ISE pueden usar esos conectores.

En el diseñador de Logic Apps, al examinar los desencadenadores y acciones integrados o los conectores administrados que quiere usar para las aplicaciones lógicas en un ISE, la etiqueta **CORE** aparece en los desencadenadores y acciones integrados, mientras que la etiqueta **ISE** aparece en los conectores administrados diseñados para funcionar con un ISE.

:::row:::
    :::column:::
        ![Conector de ejemplo CORE](./media/apis-list/example-core-connector.png)
        \
        \
        **CORE**
        \
        \
        Los desencadenadores y acciones integrados con esta etiqueta se ejecutan en el mismo ISE que las aplicaciones lógicas.
    :::column-end:::
    :::column:::
        ![Ejemplo de conector de ISE](./media/apis-list/example-ise-connector.png)
        \
        \
        **ISE**
        \
        \
        Los conectores administrados con esta etiqueta se ejecutan en el mismo ISE que las aplicaciones lógicas. 
        \
        \
        Si tiene un sistema local que está conectado a una red virtual de Azure, un ISE permite a los flujos de trabajo obtener acceso directo a dicho sistema sin usar la [puerta de enlace de datos local](../logic-apps/logic-apps-gateway-connection.md). En su lugar, puede usar el conector de **ISE** si está disponible, una acción HTTP o un [conector personalizado](#custom-apis-and-connectors).
        \
        \
        En el caso de los sistemas locales que no tienen conectores de **ISE**, use la puerta de enlace de datos local. Para buscar los conectores de ISE disponibles, revise [conectores de ISE](#ise-and-connectors).
    :::column-end:::
    :::column:::
        ![Ejemplo de conector que no es ISE](./media/apis-list/example-multi-tenant-connector.png)
        \
        \
        Sin etiqueta     \
        \
        Todos los demás conectores que no tienen una etiqueta que puede seguir usando, se ejecutan en el servicio Logic Apps global multiinquilino.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

## <a name="known-issues"></a>Problemas conocidos

En la siguiente tabla se muestran problemas conocidos de los conectores de Logic Apps.

| Mensaje de error| Descripción | Solución |
|--------------|-------------|------------|
| `Error: BadGateway. Client request id: '{GUID}'` | Este error se produce cuando se actualizan las etiquetas de una aplicación lógica en la que una o varias conexiones no admiten la autenticación OAuth de Azure Active Directory (Azure AD), como SFTP ad SQL, con lo que se interrumpen esas conexiones. | Para evitar este comportamiento, evite actualizar esas etiquetas. |
||||

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de API personalizadas que se pueden llamar desde Logic Apps](../logic-apps/logic-apps-create-api-app.md)
