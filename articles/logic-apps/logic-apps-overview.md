---
title: Introducción a Azure Logic Apps
description: Azure Logic Apps es una plataforma en la nube para la automatización de flujos de trabajo que integran aplicaciones, datos, servicios y sistemas con un código mínimo o sin código. Los flujos de trabajo se pueden ejecutar en un entorno dedicado, multiinquilino o de inquilino único.
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, azla
ms.topic: overview
ms.custom: mvc, contperf-fy21q4
ms.date: 08/18/2021
ms.openlocfilehash: a5b1b0f6d4d51874dd330c2336e5f342aaedeec9
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131085874"
---
# <a name="what-is-azure-logic-apps"></a>¿Qué es Azure Logic Apps?

[Azure Logic Apps](https://azure.microsoft.com/services/logic-apps) es una plataforma basada en la nube para crear y ejecutar [*flujos de trabajo*](#workflow) automatizados que integren sus aplicaciones, datos, servicios y sistemas. Esta plataforma permite desarrollar rápidamente soluciones de integración altamente escalables para escenarios intraempresariales y de negocio a negocio (B2B). Azure Logic Apps forma parte de [Azure Integration Services](https://azure.microsoft.com/product-categories/integration/), por lo que simplifica la forma de conectar sistemas heredados, modernos y vanguardistas a través de entornos híbridos, locales y en la nube.

En la lista siguiente se describen solo algunas tareas, procesos empresariales y cargas de trabajo de ejemplo que puede automatizar con el servicio Azure Logic Apps:

* Programación y envío de notificaciones por correo electrónico mediante Office 365 cuando se produce un evento específico, por ejemplo, cuando se ha cargado un archivo nuevo.

* Procesamiento y enrutamiento de pedidos entre sistemas locales y servicios en la nube.

* Traslado de archivos cargados de un servidor SFTP o FTP a Azure Storage.

* Supervisión de tuits, análisis de opiniones y creación de alertas o tareas para los elementos que se deben revisar.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Go-serverless-Enterprise-integration-with-Azure-Logic-Apps/player]

En función del tipo de recurso de aplicación lógica que elija y cree, las aplicaciones lógicas se ejecutan en una instancia de Azure Logic Apps multiinquilino, en [Azure Logic Apps de un solo inquilino](single-tenant-overview-compare.md) o en un [entorno de servicio de integración](connect-virtual-network-vnet-isolated-environment-overview.md) dedicado cuando acceden a una red virtual de Azure. Para ejecutar las aplicaciones lógicas en contenedores, [cree aplicaciones lógicas basadas en un solo inquilino mediante Logic Apps habilitado para Azure Arc](azure-arc-enabled-logic-apps-create-deploy-workflows.md). Para más información, consulte [¿Qué es Logic Apps habilitado para Azure Arc?](azure-arc-enabled-logic-apps-overview.md) y [Diferencias entre el tipo de recurso y el entorno de host para las aplicaciones lógicas](#resource-environment-differences).

Para acceder y ejecutar operaciones de forma segura en tiempo real en varios orígenes de datos, puede elegir [*conectores administrados*](#managed-connector) de un [ecosistema de más de 400 conectores de Azure y cada vez mayor](/connectors/connector-reference/connector-reference-logicapps-connectors) para usarlos en los flujos de trabajo, por ejemplo:

* Servicios de Azure, como Blob Storage y Service Bus

* Servicios de Office 365, como Outlook, Excel y SharePoint

* Servidores de bases de datos, como SQL y Oracle

* Sistemas empresariales, como SAP e IBM MQ

* Recursos compartidos de archivos, como FTP y SFTP

Para comunicarse con cualquier punto de conexión de servicio, ejecutar su propio código, organizar el flujo de trabajo o manipular los datos, puede usar desencadenadores y acciones [*integrados*](#built-in-operations), que se ejecutan de forma nativa dentro del servicio Azure Logic Apps. Por ejemplo, los desencadenadores integrados incluyen Solicitud, HTTP y Periodicidad. Las acciones integradas incluyen Condición, Bucle For each, Ejecutar código JavaScript y operaciones que llaman a funciones de Azure, aplicaciones web o aplicaciones de API hospedadas en Azure y otros flujos de trabajo de Azure Logic Apps.

Para escenarios de integración B2B, Azure Logic Apps incluye funcionalidades de [BizTalk Server](/biztalk/core/introducing-biztalk-server). Para definir artefactos de negocio a negocio (B2B), cree una [*cuenta de integración*](#integration-account) donde almacene estos artefactos. Después de vincular esta cuenta a la aplicación lógica, los flujos de trabajo pueden usar estos artefactos B2B e intercambiar mensajes que cumplan con los estándares de intercambio electrónico de datos (EDI) y Enterprise Application Integration (EAI).

Para más información sobre las formas en las que los flujos de trabajo pueden acceder y trabajar con aplicaciones, datos, servicios y sistemas, consulte la siguiente documentación:

* [Conectores para Azure Logic Apps](../connectors/apis-list.md)

* [Desencadenadores y acciones integrados para Logic Apps](../connectors/built-in.md)

* [Conectores administrados para Logic Apps](../connectors/managed.md)

* [Soluciones de integración empresarial B2B con Azure Logic Apps y Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md)

<a name="logic-app-concepts"></a>

## <a name="key-terms"></a>Términos clave

Los términos siguientes son conceptos importantes en el servicio Azure Logic Apps.

### <a name="logic-app"></a>Aplicación lógica

Una *aplicación lógica* es un recurso de Azure que se crea cuando se quiere desarrollar un flujo de trabajo. Hay [varios tipos de recursos de aplicación lógica que se ejecutan en entornos diferentes](#resource-environment-differences).

### <a name="workflow"></a>Flujo de trabajo

Un *flujo de trabajo* es una serie de pasos que definen una tarea o un proceso. Cada flujo de trabajo comienza con un único desencadenador, después del cual debe agregar una o varias acciones.

### <a name="trigger"></a>Desencadenador

Un *desencadenador* es siempre el primer paso de cualquier flujo de trabajo y especifica la condición para ejecutar los demás pasos de ese flujo de trabajo. Por ejemplo, un evento de desencadenador podría recibir un correo electrónico en la bandeja de entrada o detectar un nuevo archivo en una cuenta de almacenamiento.

### <a name="action"></a>Acción

Una *acción* es cada paso de un flujo de trabajo después del desencadenador. Cada acción ejecuta alguna operación en un flujo de trabajo.

### <a name="built-in-operations"></a>Operaciones integradas

Un desencadenador o una acción *integrados* son una operación que se ejecuta de forma nativa en Azure Logic Apps. Por ejemplo, las operaciones integradas proporcionan maneras de controlar la programación o estructura del flujo de trabajo, ejecutar su propio código, administrar y manipular datos, enviar o recibir solicitudes en un punto de conexión y llevar a cabo otras tareas del flujo de trabajo.

La mayoría de las operaciones integradas no están asociadas a ningún servicio o sistema, pero algunas operaciones integradas están disponibles para servicios específicos, como Azure Functions o Azure App Service. Muchas tampoco requieren que primero cree una conexión desde el flujo de trabajo y autentique su identidad. Para obtener más información y ejemplos, consulte [Desencadenadores y acciones integrados para Logic Apps](../connectors/built-in.md).

Por ejemplo, puede iniciar casi cualquier flujo de trabajo según una programación mediante el desencadenador Periodicidad. O bien, puede hacer que el flujo de trabajo espere hasta que se le llame mediante el desencadenador Solicitud.

### <a name="managed-connector"></a>Conector administrado

Un *conector administrado* es un proxy o contenedor precompilado para una API REST que puede usar para acceder a una aplicación, datos, un servicio o un sistema específicos. Para poder crear la mayoría de los conectores administrados, primero debe crear una conexión desde el flujo de trabajo y autenticar su identidad. Microsoft se encarga de publicar, hospedar y administrar los conectores administrados. Para más información, consulte [Conectores administrados para Logic Apps](../connectors/managed.md).

Por ejemplo, puede iniciar un flujo de trabajo con un desencadenador o ejecutar una acción que funcione con un servicio como Office 365, Salesforce o servidores de archivos.

### <a name="integration-account"></a>cuenta de integración

Una *cuenta de integración* es un recurso de Azure que se crea cuando se desea definir y almacenar artefactos B2B para usarlos en flujos de trabajo. Después de [crear y vincular una cuenta de integración](logic-apps-enterprise-integration-create-integration-account.md) con la aplicación lógica, los flujos de trabajo pueden usar estos artefactos B2B. Los flujos de trabajo también pueden intercambiar mensajes que sigan los estándares de intercambio electrónico de datos (EDI) e integración de aplicaciones empresariales (EAI).

Por ejemplo, puede definir socios comerciales, contratos, esquemas, mapas y otros artefactos B2B. Puede crear flujos de trabajo que usen estos artefactos e intercambiar mensajes a través de protocolos como AS2, EDIFACT, X12 y RosettaNet.

<a name="how-do-logic-apps-work"></a>

## <a name="how-logic-apps-work"></a>Cómo funcionan las aplicaciones lógicas

En una aplicación lógica, cada flujo de trabajo siempre se inicia con un único [desencadenador](#trigger). Un desencadenador se desencadena cuando se cumple una condición, por ejemplo, cuando se produce un evento específico o cuando los datos cumplen criterios específicos. Muchos desencadenadores incluyen [funcionalidades de programación](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md) que controlan la frecuencia con la que se ejecuta el flujo de trabajo. Después del desencadenador, hay una o varias [acciones](#action) que ejecutan operaciones que, por ejemplo, procesan, controlan o convierten datos que recorren el flujo de trabajo, o que avanzan el flujo de trabajo al paso siguiente.

En la siguiente captura de pantalla, se muestra parte de un flujo de trabajo empresarial de ejemplo. Este flujo de trabajo usa condiciones y modificadores para determinar la acción siguiente. Supongamos que tiene un sistema de pedidos y el flujo de trabajo procesa los pedidos entrantes. Quiere revisar manualmente los pedidos que estén por encima de un costo determinado. El flujo de trabajo ya tiene pasos anteriores que determinan cuánto cuesta un pedido entrante. Por tanto, crea una condición inicial basada en ese valor del costo. Por ejemplo:

* Si el pedido está por debajo de una cantidad determinada, la condición es false. En este caso, el flujo de trabajo procesa el pedido.

* Si la condición es true, el flujo de trabajo envía un correo electrónico para su revisión manual. Un modificador determina el paso siguiente.

  * Si el revisor lo aprueba, el flujo de trabajo continúa procesando el pedido.

  * Si el revisor remite el pedido a otra instancia, el flujo de trabajo envía un correo electrónico de remisión para obtener más información sobre el pedido.

    * Si se cumplen los requisitos de la remisión, la condición de respuesta es true. Por tanto, se procesa el pedido.

    * Si la condición de respuesta es false, se envía un correo electrónico sobre el problema.

:::image type="content" source="./media/logic-apps-overview/example-enterprise-workflow.png" alt-text="Captura de pantalla donde se muestra el diseñador de flujos de trabajo y un flujo de trabajo empresarial de ejemplo que usa modificadores y condiciones." lightbox="./media/logic-apps-overview/example-enterprise-workflow.png":::

Puede crear visualmente flujos de trabajo mediante el diseñador de flujos de trabajo de Azure Logic Apps en Azure Portal, Visual Studio Code o Visual Studio. Cada flujo de trabajo también tiene una definición subyacente que se describe mediante notación de objetos JavaScript (JSON). Si lo prefiere, puede editar flujos de trabajo cambiando la definición de este JSON. Para algunas tareas de creación y administración, Azure Logic Apps proporciona compatibilidad con comandos de Azure PowerShell y la CLI de Azure. Para la implementación automatizada, Azure Logic Apps admite plantillas de Azure Resource Manager.

<a name="resource-environment-differences"></a>

## <a name="resource-type-and-host-environment-differences"></a>Diferencias entre el tipo de recurso y el entorno de host

Para crear flujos de trabajo de aplicación lógica, elija el tipo de recurso **Logic Apps** en función del escenario, los requisitos de la solución, las funcionalidades que desea y el entorno donde quiere ejecutar los flujos de trabajo.

En la tabla siguiente se resumen brevemente las diferencias entre el tipo de recurso original **Logic Apps (consumo)** y el tipo de recurso **Logic Apps (estándar)** . También aprenderá las diferencias entre el *entorno de inquilino único*, el *entorno multiinquilino*, el *entorno del servicio de integración (ISE)* y *App Service Environment v3 (ASEv3)* a la hora de implementar, hospedar y ejecutar los flujos de trabajo de la aplicación lógica.

[!INCLUDE [Logic app resource type and environment differences](../../includes/logic-apps-resource-environment-differences-table.md)]

## <a name="why-use-azure-logic-apps"></a>Motivos de uso de Azure Logic Apps

La plataforma de integración de Azure Logic Apps proporciona conectores de API administrados por Microsoft creados previamente y operaciones integradas, con el fin de que pueda conectar e integrar aplicaciones, datos, servicios y sistemas de forma más fácil y rápida. Puede centrarse más en el diseño e implementación de la funcionalidad y lógica de negocios de la solución, no en averiguar cómo acceder a los recursos.

Normalmente, no tendrá que escribir código. Sin embargo, si necesita escribir código, puede utilizar [Azure Functions ](../azure-functions/functions-overview.md) para crear fragmentos de código y ejecutar ese código desde un flujo de trabajo. También puede crear fragmentos de código que se ejecuten en el flujo de trabajo mediante la acción [**Código en línea**](logic-apps-add-run-inline-code.md). Si el flujo de trabajo necesita interactuar con eventos de servicios de Azure, aplicaciones personalizadas u otras soluciones, puede supervisar, enrutar y publicar eventos mediante [Azure Event Grid](../event-grid/overview.md).

Microsoft Azure administra completamente Azure Logic Apps, lo que le permite despreocuparse de la compilación, hospedaje, escalado, administración, supervisión y mantenimiento de estos servicios. Si usa estas funcionalidades para crear [aplicaciones y soluciones "sin servidor"](../logic-apps/logic-apps-serverless-overview.md), puede centrarse en la lógica de negocios y en la funcionalidad. Estos servicios se escalan automáticamente para satisfacer sus necesidades, agilizar las integraciones y ayudarle a crear sólidas aplicaciones en la nube con poco código, o sin código alguno.

Para ver la forma en que otras empresas han mejorado su agilidad y se han centrado en sus negocios principales al combinar Azure Logic Apps con otros servicios de Azure y productos de Microsoft, consulte estos [testimonios de clientes](https://aka.ms/logic-apps-customer-stories).

Las secciones siguientes proporcionan más información acerca de las funcionalidades y beneficios de Azure Logic Apps:

#### <a name="visually-create-and-edit-workflows-with-easy-to-use-tools"></a>Creación y edición visual de flujos de trabajo con herramientas fáciles de usar

Ahorre tiempo y simplifique los procesos complejos mediante el uso de herramientas de diseño visual de Azure Logic Apps. Cree flujos de trabajo de principio a fin mediante el diseñador de flujos de trabajo de Azure Logic Apps en Azure Portal, Visual Studio Code o Visual Studio. Inicie su flujo de trabajo con un desencadenador y agregue varias acciones desde la [galería de conectores](/connectors/connector-reference/connector-reference-logicapps-connectors).

Si va a crear una aplicación lógica basada en multiinquilino, empiece a trabajar más rápidamente al [crear un flujo de trabajo desde la galería de plantillas](../logic-apps/logic-apps-create-logic-apps-from-templates.md). Estas plantillas están disponibles para patrones de flujo de trabajo comunes, que van desde una conectividad sencilla para aplicaciones de software como servicio (SaaS) hasta soluciones B2B avanzadas, además de plantillas "solo para disfrutar".

#### <a name="connect-different-systems-across-various-environments"></a>Conexión de distintos sistemas en varios entornos

Determinados patrones y flujos de trabajo son fáciles de describir pero difíciles de implementar en el código. La plataforma Azure Logic Apps le ayuda a conectar sin problemas sistemas dispares en entornos híbridos, locales y en la nube. Por ejemplo, puede conectar una solución de marketing de la nube con un sistema de facturación local, o bien centralizar la mensajería a través de las API y los sistemas mediante Azure Service Bus. Azure Logic Apps ofrece una forma rápida, fiable y coherente de proporcionar soluciones que se pueden volver a utilizar y configurar para estos escenarios.

#### <a name="write-once-reuse-often"></a>Escriba una vez y úselo tanto como quiera

Cree aplicaciones lógicas como plantillas de Azure Resource Manager para que pueda [configurar y automatizar las implementaciones](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md) en varios entornos y regiones.

#### <a name="first-class-support-for-enterprise-integration-and-b2b-scenarios"></a>Soporte de primera clase para escenarios B2B y de integración empresarial

Las empresas y organizaciones se comunican electrónicamente entre sí mediante el uso de protocolos y formatos de mensajes estándares del sector, pero diferentes, como EDIFACT, AS2, X12 y RosettaNet. Mediante el uso de las [funcionalidades de integración empresarial](../logic-apps/logic-apps-enterprise-integration-overview.md) compatibles con Azure Logic Apps, puede crear flujos de trabajo que transformen los formatos de mensaje que usan las entidades en formatos que los sistemas de su organización puedan interpretar y procesar. Azure Logic Apps administra estos intercambios de forma fácil y segura mediante cifrado y firmas digitales.

Empiece poco a poco con sus servicios y sistemas actuales y crezca de forma gradual a su propio ritmo. Cuando esté listo, la plataforma Azure Logic Apps le ayudará a implementar y realizar un escalado vertical a escenarios de integración más avanzados mediante estas funcionalidades y muchas más:

* Integre y compile a partir de [Microsoft BizTalk Server](/biztalk/core/introducing-biztalk-server), [Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md), [Azure Functions](../azure-functions/functions-overview.md), [Azure API Management](../api-management/api-management-key-concepts.md), etc.

* Intercambie mensajes mediante los protocolos [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md), [AS2,](../logic-apps/logic-apps-enterprise-integration-as2.md) [X12](../logic-apps/logic-apps-enterprise-integration-x12.md)y [RosettaNet](logic-apps-enterprise-integration-rosettanet.md).

* Procese [mensajes XML](../logic-apps/logic-apps-enterprise-integration-xml.md) y [archivos planos](../logic-apps/logic-apps-enterprise-integration-flatfile.md).

* Cree una [cuenta de integración](./logic-apps-enterprise-integration-create-integration-account.md) para almacenar y administrar artefactos B2B, como [entidades](../logic-apps/logic-apps-enterprise-integration-partners.md), [acuerdos](../logic-apps/logic-apps-enterprise-integration-agreements.md), [mapas de transformación](../logic-apps/logic-apps-enterprise-integration-maps.md), [esquemas de validación](../logic-apps/logic-apps-enterprise-integration-schemas.md), etc.

Por ejemplo, si usa Microsoft BizTalk Server, los flujos de trabajo pueden comunicarse con BizTalk Server mediante el [conector de BizTalk Server](../connectors/managed.md#on-premises-connectors). Luego, puede ampliar o realizar operaciones similares a las de BizTalk en los flujos de trabajo mediante los [conectores de la cuenta de integración](../connectors/managed.md#integration-account-connectors). Por otra parte, BizTalk Server puede comunicarse con sus flujos de trabajo mediante el [adaptador de Microsoft BizTalk Server Adapter para Azure Logic Apps](https://www.microsoft.com/download/details.aspx?id=54287). Aprenda a [configurar y usar el adaptador de BizTalk Server](/biztalk/core/logic-app-adapter).

#### <a name="built-in-extensibility"></a>Extensibilidad integrada

Si no hay ningún conector adecuado disponible para ejecutar el código que desee, puede crear y llamar a sus propios fragmentos de código desde el flujo de trabajo mediante [Azure Functions](../azure-functions/functions-overview.md). O bien, cree sus propias [API](../logic-apps/logic-apps-create-api-app.md) y [conectores personalizados](../logic-apps/custom-connector-overview.md), a los que puede llamar desde sus flujos de trabajo.

#### <a name="access-resources-inside-azure-virtual-networks"></a>Acceso a recursos dentro de redes virtuales de Azure

Los flujos de trabajo de aplicaciones lógicas pueden acceder a recursos protegidos, como máquinas virtuales y otros sistemas o servicios dentro de una [red virtual de Azure](../virtual-network/virtual-networks-overview.md) al crear un [*entorno del servicio de integración* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Un ISE es una instancia dedicada del servicio Azure Logic Apps que usa recursos dedicados y se ejecuta de forma independiente del servicio Azure Logic Apps público, global y multiinquilino.

La ejecución de aplicaciones lógicas en una instancia dedicada ayuda a reducir el impacto que podrían tener otros inquilinos de Azure en el rendimiento de las aplicaciones, también conocido como el [efecto "vecinos ruidosos"](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors). Un ISE también proporciona estas ventajas:

* Direcciones IP estáticas propias, que son independientes de las direcciones IP estáticas que comparten las aplicaciones lógicas en el servicio multiinquilino. Puede configurar una dirección IP de salida única, pública, estática y predecible para comunicarse con los sistemas de destino. De ese modo, no es necesario configurar aperturas adicionales del firewall en los sistemas de destino para cada ISE.

* Se aumentan los límites de duración de ejecución, retención de almacenamiento, rendimiento, solicitud HTTP y tiempos de espera de respuesta, tamaños de mensaje y solicitudes de conector personalizado. Para más información, consulte el artículo sobre los [límites y la configuración de Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md).

Cuando se crea un ISE, Azure *inserta* o implementa dicho ISE en su red virtual de Azure. A continuación, puede usar ese ISE como la ubicación de las aplicaciones lógicas y cuentas de integración que necesitan acceso. Para más información sobre cómo crear un ISE, consulte [Conexión a redes virtuales de Azure desde Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).

#### <a name="pricing-options"></a>Opciones de precios

Cada tipo de aplicación lógica, que difiere en función de las funcionalidades y dónde se ejecutan (multiinquilino, inquilino único, entorno de servicio de integración), tiene un [modelo de precios](../logic-apps/logic-apps-pricing.md) diferente. Por ejemplo, las aplicaciones lógicas basadas en multiinquilino usan precios basados en el consumo, mientras que las que están en un Entorno del servicio de integración usan precios fijos. Obtenga más información sobre [precios y medición](../logic-apps/logic-apps-pricing.md) de Azure Logic Apps.

## <a name="how-does-azure-logic-apps-differ-from-functions-webjobs-and-power-automate"></a>¿En qué se diferencia Azure Logic Apps de Functions, WebJobs y Power Automate?

Todos estos servicios le ayudan a conectarse y a reunir sistemas dispares. Cada servicio tiene sus ventajas y beneficios, por lo que la combinación de sus funcionalidades es la mejor manera de crear rápidamente un sistema de integración escalable y completo. Para más información, consulte el artículo en el que se proporciona la información necesaria para [elegir entre Logic Apps, Functions, WebJobs y Power Automate](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md).

## <a name="get-started"></a>Primeros pasos

Para poder empezar a usar Azure Logic Apps, necesita una suscripción de Azure. Si aún no tiene una, [regístrese para obtener una cuenta de Azure gratuita](https://azure.microsoft.com/free/).

Cuando esté listo, pruebe una o varias de las siguientes guías de inicio rápido para Azure Logic Apps. Vea cómo crear un flujo de trabajo básico que supervise una fuente RSS y envíe un correo electrónico para obtener contenido nuevo.

* [Creación de un flujo de trabajo de integración con Azure Logic Apps multiinquilino en Azure Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* [Creación de flujos de trabajo de integración automatizados con Azure Logic Apps multiinquilino y Visual Studio](quickstart-create-logic-apps-with-visual-studio.md)

* [Cree y administre definiciones de flujo de trabajo de aplicación lógica con Azure Logic Apps multiinquilino y Visual Studio Code](quickstart-create-logic-apps-visual-studio-code.md)

Quizá le interese también explorar otras guías de inicio rápido de Azure Logic Apps:

* [Creación e implementación de un flujo de trabajo de aplicaciones lógicas mediante una plantilla de ARM](quickstart-create-deploy-azure-resource-manager-template.md)

* [Creación y administración de flujos de trabajo mediante la CLI de Azure en instancias de Azure Logic Apps multiinquilino](quickstart-create-deploy-azure-resource-manager-template.md)

## <a name="other-resources"></a>Otros recursos

Obtenga más información sobre la plataforma de Azure Logic Apps con estos vídeos introductorios:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Connect-and-extend-your-mainframe-to-the-cloud-with-Logic-Apps/player]

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Creación del primer flujo de trabajo de Logic Apps en Azure Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md)
