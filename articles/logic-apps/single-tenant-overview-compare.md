---
title: Azure Logic Apps de inquilino único frente a multiinquilino
description: Conozca las diferencias entre las opciones de un solo inquilino, multiinquilino y de entorno del servicio de integración (ISE) para Azure Logic Apps.
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, azla
ms.topic: conceptual
ms.date: 09/13/2021
ms.openlocfilehash: 46b5503e6c2c99c2c99f5cd18dc695ecb16275d1
ms.sourcegitcommit: 01dcf169b71589228d615e3cb49ae284e3e058cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2021
ms.locfileid: "130166846"
---
# <a name="single-tenant-versus-multi-tenant-and-integration-service-environment-for-azure-logic-apps"></a>Comparación de las opciones de un solo inquilino, multiinquilino y entorno del servicio de integración para Azure Logic Apps

Azure Logic Apps es una plataforma basada en la nube para crear y ejecutar *flujos de trabajo de aplicaciones lógicas* automatizados que integren sus aplicaciones, datos, servicios y sistemas. Esta plataforma permite desarrollar rápidamente soluciones de integración altamente escalables para escenarios intraempresariales y de negocio a negocio (B2B). Para crear una aplicación lógica, use el tipo de recurso **Logic Apps (consumo)** o el tipo de recurso **Logic Apps (estándar)** . El tipo de recurso de consumo se ejecuta en Azure Logic Apps *multiinquilino* o en el *entorno del servicio de integración*, mientras que el tipo de recurso estándar se ejecuta en el entorno de Azure Logic Apps de un *solo inquilino*.

Antes de elegir el tipo de recurso que se va a usar, revise este artículo para obtener información sobre cómo se comparan entre sí los tipos de recursos y los entornos de servicio. A continuación, puede decidir qué tipo es mejor usar, en función de las necesidades del escenario, los requisitos de la solución y el entorno donde quiere implementar, hospedar y ejecutar los flujos de trabajo.

Si no está familiarizado con Azure Logic Apps, revise la siguiente documentación:

* [¿Qué es Azure Logic Apps?](logic-apps-overview.md)
* [¿Qué es un *flujo de trabajo de aplicación lógica*?](logic-apps-overview.md#logic-app-concepts)

<a name="resource-environment-differences"></a>

## <a name="resource-types-and-environments"></a>Tipos de recursos y entornos

Para crear flujos de trabajo de aplicación lógica, elija el tipo de recurso **Logic Apps** en función del escenario, los requisitos de la solución, las funcionalidades que desea y el entorno donde quiere ejecutar los flujos de trabajo.

En la tabla siguiente se resumen brevemente las diferencias entre el tipo de recurso **Logic Apps (estándar)** y el tipo de recurso **Logic Apps (consumo)** . También aprenderá en qué se diferencia la opción de *un solo inquilino* de la opción *multiinquilino* y el *entorno del servicio de integración (ISE)* para implementar, hospedar y ejecutar los flujos de trabajo de la aplicación lógica.

[!INCLUDE [Logic app resource type and environment differences](../../includes/logic-apps-resource-environment-differences-table.md)]

<a name="resource-type-introduction"></a>

## <a name="logic-app-standard-resource"></a>Recurso Logic Apps (estándar)

El tipo de recurso **Logic Apps (estándar)** está equipado con la tecnología de runtime rediseñada de Azure Logic Apps de un solo inquilino. Este runtime usa el [modelo de extensibilidad de Azure Functions](../azure-functions/functions-bindings-register.md) y se hospeda como una extensión en el runtime de Azure Functions. Este diseño proporciona portabilidad, flexibilidad y más rendimiento para los flujos de trabajo de aplicaciones lógicas, además de otras funcionalidades y ventajas heredadas de la plataforma Azure Functions y el ecosistema Azure App Service. Por ejemplo, puede crear, implementar y ejecutar aplicaciones lógicas basadas en un solo inquilino y sus flujos de trabajo en [Azure App Service Environment v3](../app-service/environment/overview.md).

El tipo de recurso estándar presenta una estructura de recursos que puede hospedar varios flujos de trabajo, igual que una aplicación de funciones de Azure puede hospedar varias funciones. Con una asignación de uno a varios, los flujos de trabajo de la misma aplicación lógica y el inquilino comparten recursos de proceso y procesamiento, lo que proporciona un mejor rendimiento debido a su proximidad. Esta estructura difiere del recurso **Logic Apps (consumo)** , donde tiene una asignación de uno a uno entre un recurso de aplicación lógica y un flujo de trabajo.

Para obtener más información sobre las mejoras de portabilidad, flexibilidad y rendimiento, continúe con las secciones siguientes. O bien, para obtener más información sobre el runtime de Azure Logic Apps de un solo inquilino o la extensibilidad de Azure Functions, revise la siguiente documentación:

* [Azure Logic Apps en ejecución en cualquier ubicación: el runtime en profundidad](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-running-anywhere-runtime-deep-dive/ba-p/1835564)
* [Introducción a Azure Functions](../azure-functions/functions-overview.md)
* [Enlaces y desencadenadores de Azure Functions](../azure-functions/functions-triggers-bindings.md)

<a name="portability"></a>
<a name="flexibility"></a>

### <a name="portability-and-flexibility"></a>Portabilidad y flexibilidad

Al crear aplicaciones lógicas con el tipo de recurso **Aplicación lógica (Estándar)**, puede implementar y ejecutar los flujos de trabajo en otros entornos, como [Azure App Service Environment v3](../app-service/environment/overview.md). Si usa Visual Studio Code con la extensión **Azure Logic Apps (estándar)**, puede desarrollar, compilar y ejecutar *localmente* los flujos de trabajo en el entorno de desarrollo sin tener que realizar la implementación en Azure. Si el escenario requiere contenedores, [cree aplicaciones lógicas basadas en un solo inquilino mediante Logic Apps habilitado para Azure Arc](azure-arc-enabled-logic-apps-create-deploy-workflows.md). Para más información, consulte [¿Qué es Azure Logic Apps habilitado para Azure Arc?](azure-arc-enabled-logic-apps-overview.md)

Esta funcionalidad proporciona mejoras importantes y ventajas considerables en comparación con el modelo multiinquilino, que requiere el desarrollo en un recurso en ejecución existente en Azure. Además, el modelo multiinquilino para automatizar la implementación de recursos de **Logic Apps (consumo)** se basa completamente en plantillas de Azure Resource Manager (plantillas de ARM), que combinan y controlan el aprovisionamiento de recursos para las aplicaciones y la infraestructura.

Con el tipo de recurso **Logic Apps (estándar)** , la implementación es más fácil porque puede separar la implementación de las aplicaciones de la implementación de la infraestructura. Puede empaquetar los flujos de trabajo y el runtime de Azure Logic Apps de un solo inquilino como parte de la aplicación lógica. Puede usar pasos genéricos o tareas que compilan, ensamblan y comprimen los recursos de la aplicación lógica en artefactos listos para implementarse. Para implementar la infraestructura, todavía puede usar plantillas de ARM para aprovisionar por separado esos recursos junto con otros procesos y canalizaciones que usa para esos fines.

Para implementar la aplicación, copie los artefactos en el entorno host y, a continuación, inicie las aplicaciones para ejecutar los flujos de trabajo. O bien, integre los artefactos en canalizaciones de implementación mediante las herramientas y los procesos que ya conoce y usa. De este modo, puede realizar la implementación con herramientas propias que elija, independientemente de la pila de tecnología que use para el desarrollo.

Mediante las opciones de compilación e implementación estándar, puede centrarse en el desarrollo de las aplicaciones por separado de la implementación de la infraestructura. Como resultado, obtiene un modelo de proyecto más genérico en el que se pueden aplicar muchas opciones de implementación similares o las mismas que se usan para una aplicación genérica. También se beneficia de una experiencia más coherente para crear canalizaciones de implementación en torno a los proyectos de aplicación, y para ejecutar las pruebas y validaciones necesarias antes de la publicación en producción.

<a name="performance"></a>

### <a name="performance"></a>Rendimiento

Con el tipo de recurso **Logic Apps (estándar)** , puede crear y ejecutar varios flujos de trabajo en la misma aplicación y el mismo inquilino únicos. Con esta asignación de uno a varios, estos flujos de trabajo comparten recursos, como los de proceso, procesamiento, almacenamiento y red, lo que proporciona un mejor rendimiento debido a su proximidad.

El tipo de recurso **Logic Apps (estándar)** y el runtime de Azure Logic Apps de un solo inquilino proporcionan otra mejora significativa al hacer que los conectores administrados más populares estén disponibles como operaciones integradas. Por ejemplo, puede usar operaciones integradas para Azure Service Bus, Azure Event Hubs, SQL y otros. Mientras tanto, las versiones del conector administrado siguen estando disponibles y siguen funcionando.

Cuando usa las nuevas operaciones integradas, se crean conexiones denominadas *conexiones integradas* o *conexiones de proveedor de servicios*. Sus homólogos de conexión administrada se denominan *conexiones de API*, que se crean y ejecutan por separado como recursos de Azure que también tiene que implementar mediante plantillas de ARM. Las operaciones integradas y sus conexiones se ejecutan localmente en el mismo proceso que ejecuta los flujos de trabajo. Ambos se hospedan en el runtime de Azure Logic Apps de un solo inquilino. Como resultado, las operaciones integradas y sus conexiones proporcionan un mejor rendimiento debido a la proximidad con sus flujos de trabajo. Este diseño también funciona bien con las canalizaciones de implementación porque las conexiones del proveedor de servicios se empaquetan en el mismo artefacto de compilación.

<a name="data-residency"></a>

### <a name="data-residency"></a>Residencia de datos

Los recursos de aplicación lógica creados con el tipo de recurso **Aplicación lógica (Estándar)** se hospedan en instancias de Azure Logic Apps de un único inquilino, que [no almacenan, procesan ni replican datos fuera de la región donde se implementan estos recursos de aplicación lógica](https://azure.microsoft.com/global-infrastructure/data-residency), lo que significa que los datos de los flujos de trabajo de la aplicación lógica permanecen en la misma región donde se crean e implementan sus recursos primarios.

## <a name="create-build-and-deploy-options"></a>Opciones de creación, compilación e implementación

Para crear una aplicación lógica basada en el entorno que desee, tiene varias opciones, como las siguientes:

**Entorno de un solo inquilino**

| Opción | Recursos y herramientas | Más información |
|--------|---------------------|------------------|
| Azure Portal | Tipo de recurso **Logic Apps (estándar)** | [Creación de flujos de trabajo de integración para Logic Apps de un solo inquilino: Azure Portal](create-single-tenant-workflows-azure-portal.md) |
| Visual Studio Code | [Extensión **Azure Logic Apps (estándar)**](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurelogicapps) | [Creación de flujos de trabajo de integración para Logic Apps de un solo inquilino: Visual Studio Code](create-single-tenant-workflows-visual-studio-code.md) |
| Azure CLI | Extensión Logic Apps de la CLI de Azure | No disponible todavía |
||||

**Entorno multiinquilino**

| Opción | Recursos y herramientas | Más información |
|--------|---------------------|------------------|
| Azure Portal | Tipo de recurso **Logic Apps (consumo)** | [Inicio rápido. Creación de flujos de trabajo de integración en Azure Logic Apps multiinquilino: Azure Portal](quickstart-create-first-logic-app-workflow.md) |
| Visual Studio Code | [Extensión **Azure Logic Apps (consumo)**](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-logicapps) | [Inicio rápido. Creación de flujos de trabajo de integración en Azure Logic Apps multiinquilino: Visual Studio Code](quickstart-create-logic-apps-visual-studio-code.md)
| Azure CLI | [Extensión **Logic Apps de la CLI de Azure**](https://github.com/Azure/azure-cli-extensions/tree/master/src/logic) | - [Inicio rápido. Creación y administración de flujos de trabajo de integración en Azure Logic Apps multiinquilino: CLI de Azure](quickstart-logic-apps-azure-cli.md) <p><p>- [az logic](/cli/azure/logic) |
| Azure Resource Manager | [**Creación de una aplicación lógica** mediante una plantilla de ARM](https://azure.microsoft.com/resources/templates/logic-app-create/) | [Inicio rápido. Creación e implementación de flujos de trabajo de integración en Azure Logic Apps multiinquilino: plantilla de ARM](quickstart-create-deploy-azure-resource-manager-template.md) |
| Azure PowerShell | [Módulo Az.LogicApp](/powershell/module/az.logicapp) | [Introducción a Azure PowerShell](/powershell/azure/get-started-azureps) |
| API REST de Azure | [API de REST de Azure Logic Apps](/rest/api/logic) | [Introducción a la referencia de la API de REST de Azure](/rest/api/azure) |
||||

**entorno de servicio de integración**

| Opción | Recursos y herramientas | Más información |
|--------|---------------------|------------------|
| Azure Portal | Tipo de recurso **Logic Apps (consumo)** con un recurso ISE existente | Igual que [Inicio rápido: Creación de flujos de trabajo de integración en Azure Logic Apps multiinquilino: Azure Portal](quickstart-create-first-logic-app-workflow.md), pero debe seleccionar un ISE en lugar de una región multiinquilino. |
||||

Aunque las experiencias de desarrollo difieren en función de si crea recursos de aplicación lógica de **consumo** o **estándar**, puede buscar y todas las aplicaciones lógicas implementadas en su suscripción de Azure y acceder a estas.

Por ejemplo, en Azure Portal, la página **Aplicaciones lógicas** muestra los tipos de recursos de aplicación lógica de **consumo** y **estándar**. En Visual Studio Code, las aplicaciones lógicas implementadas aparecen en la suscripción de Azure, pero se agrupan por la extensión usada, es decir, **Azure: Logic Apps (consumo)** y **Azure: Logic Apps (Estándar)** .

<a name="stateful-stateless"></a>

## <a name="stateful-and-stateless-workflows"></a>Flujos de trabajo con y sin estado

Con el tipo de recurso **Logic Apps (estándar)** , puede crear estos tipos de flujo de trabajo dentro de la misma aplicación lógica:

* *Con estado*

  Cree un flujo de trabajo con estado cuando necesite mantener o revisar datos de eventos anteriores, o hacer referencia a ellos. Estos flujos de trabajo guardan y transfieren todas las entradas y las salidas de cada acción y sus estados en el almacenamiento externo, lo que facilita la revisión y ejecución de los detalles y el historial al término de cada ejecución. Los flujos de trabajo con estado proporcionan una alta resistencia en caso de interrupciones. Una vez restaurados los servicios y sistemas, puede reconstruir las ejecuciones interrumpidas a partir del estado guardado y volver a ejecutar los flujos de trabajo hasta su finalización. Los flujos de trabajo con estado pueden seguir ejecutándose durante mucho más tiempo que los flujos de trabajo sin estado.

  De manera predeterminada, los flujos de trabajo con estado de instancias multiinquilino y de un solo inquilino de Azure Logic Apps se ejecutan de forma asincrónica. Todas las acciones basadas en HTTP siguen el [patrón de operación asincrónica](/azure/architecture/patterns/async-request-reply) estándar. Este patrón especifica que, después de que una acción HTTP llame a o envíe una solicitud a un punto de conexión, servicio, sistema o API, el receptor devolverá inmediatamente una respuesta ["202 ACCEPTED"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.3). Este código confirma que el receptor aceptó la solicitud, pero no ha finalizado el procesamiento. La respuesta puede incluir un encabezado `location` que especifica el URI y un identificador de actualización que el autor de la llamada puede usar para sondear o comprobar el estado de la solicitud asincrónica hasta que el receptor detiene el procesamiento y devuelve una respuesta de operación correcta ["200 OK"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.1) u otra respuesta que no sea 202. Sin embargo, el autor de la llamada no tiene que esperar a que la solicitud finalice el procesamiento y puede continuar ejecutando la siguiente acción. Para más información, consulte [Diseño de la comunicación entre servicios para microservicios](/azure/architecture/microservices/design/interservice-communication#synchronous-versus-asynchronous-messaging).

* *Sin estado*

  Cree un flujo de trabajo sin estado cuando no necesite conservar ni revisar datos de eventos anteriores, ni hacer referencia a ellos, en un almacenamiento externo después de que finaliza cada ejecución para su posterior revisión. Estos flujos de trabajo guardan todas las entradas y las salidas de cada acción y sus estados *solo en la memoria*, no en un almacenamiento externo. Como consecuencia, los flujos de trabajo sin estado tienen ejecuciones más cortas que normalmente duran menos de 5 minutos, un rendimiento más rápido con menores tiempos de respuesta, mayor rendimiento y costos de ejecución menores, ya que los detalles y el historial de ejecución no se guardan en el almacenamiento externo. Pero si se producen interrupciones, las ejecuciones interrumpidas no se restauran automáticamente, por lo que el autor de la llamada tiene que volver a enviarlas manualmente.

  > [!IMPORTANT]
  > Un flujo de trabajo sin estado proporciona el mejor rendimiento al administrar datos o contenido, como un archivo, que no supera los 64 KB de tamaño *total*. Los tamaños de contenido más grande, como varios datos adjuntos grandes, pueden ralentizar significativamente el rendimiento del flujo de trabajo o incluso provocar que el flujo de trabajo se bloquee debido a excepciones de memoria insuficiente. Si existe la posibilidad de que el flujo de trabajo tenga que controlar tamaños de contenido más grandes, use un flujo de trabajo con estado.

  Los flujos de trabajo sin estado solo se ejecutan de manera sincrónica, por lo que no usan el [patrón de operación asincrónica](/azure/architecture/patterns/async-request-reply) estándar que emplean los flujos de trabajo con estado. En su lugar, todas las acciones basadas en HTTP que devuelven una respuesta \["202 ACCEPTED"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.3) avanzan al paso siguiente de la ejecución del flujo de trabajo. Si la respuesta incluye un encabezado `location`, un flujo de trabajo sin estado no sondea al URI especificado para comprobar el estado. Para seguir el patrón de operación asincrónica estándar, use en su lugar un flujo de trabajo con estado.

  Para facilitar la depuración, puede habilitar el historial de ejecución de un flujo de trabajo sin estado, lo que tiene algún impacto en el rendimiento, y luego deshabilitar el historial de ejecución cuando haya terminado. Para obtener más información, vea [Creación de flujos de trabajo basados en un solo inquilino en Visual Studio Code](create-single-tenant-workflows-visual-studio-code.md#enable-run-history-stateless) o [Creación de flujos de trabajo basados en un solo inquilino en Azure Portal](create-single-tenant-workflows-visual-studio-code.md#enable-run-history-stateless).

  > [!NOTE]
  > Actualmente, los flujos de trabajo sin estado solo admiten *acciones* de [conectores administrados](../connectors/managed.md) que se implementan en Azure, y no desencadenadores. Para iniciar el flujo de trabajo, seleccione el [desencadenador integrado Solicitud, Event Hubs o Service Bus](../connectors/built-in.md). Estos desencadenadores se ejecutan de forma nativa en el runtime de Azure Logic Apps. Para obtener más información sobre los desencadenadores, las acciones y los conectores limitados, no disponibles o no admitidos, vea [Capacidades modificadas, limitadas, no disponibles o no admitidas](#limited-unavailable-unsupported).

<a name="nested-behavior"></a>

### <a name="nested-behavior-differences-between-stateful-and-stateless-workflows"></a>Diferencias de comportamiento anidado entre flujos de trabajo con y sin estado

Puede [hacer que se pueda llamar a un flujo de trabajo](logic-apps-http-endpoint.md) desde otros flujos de trabajo que existan en el mismo recurso de **Logic Apps (estándar)** mediante el [desencadenador Solicitud](../connectors/connectors-native-reqres.md), el [desencadenador Webhook HTTP](../connectors/connectors-native-webhook.md) o los desencadenadores de conectores administrados que tienen el [tipo ApiConnectionWebhook](logic-apps-workflow-actions-triggers.md#apiconnectionwebhook-trigger) y pueden recibir solicitudes HTTPS.

Estos son los patrones de comportamiento que los flujos de trabajo anidados pueden seguir después de que un flujo de trabajo primario llame a uno secundario:

* Patrón de sondeo asincrónico

  El flujo de trabajo primario no espera una respuesta a su llamada inicial, pero comprueba continuamente el historial de ejecución del flujo de trabajo secundario hasta que este termina de ejecutarse. De manera predeterminada, los flujos de trabajo con estado siguen este patrón, que es ideal para flujos de trabajo secundarios de ejecución prolongada que podrían superar los [límites de tiempo de espera de las solicitudes](logic-apps-limits-and-config.md).

* Patrón sincrónico ("desencadenar y olvidar")

  El flujo de trabajo secundario confirma la llamada devolviendo inmediatamente una respuesta `202 ACCEPTED`, y el flujo de trabajo primario continúa con la siguiente acción sin esperar los resultados del secundario. En su lugar, el primario recibe los resultados cuando el secundario termina de ejecutarse. Los flujos de trabajo con estado secundarios que no incluyen una acción Respuesta siempre siguen el patrón sincrónico. En el caso de los flujos de trabajo con estado secundarios, el historial de ejecución está disponible para su revisión.

  Para habilitar este comportamiento, en la definición de JSON del flujo de trabajo, establezca la propiedad `operationOptions` en `DisableAsyncPattern`. Para obtener más información, consulte [Tipos de desencadenador y de acción: Opciones de operación](logic-apps-workflow-actions-triggers.md#operation-options).

* Desencadenar y esperar

  Para un flujo de trabajo sin estado secundario, el primario espera una respuesta que devuelve los resultados del secundario. Este patrón funciona de forma similar al uso del [desencadenador o acción HTTP](../connectors/connectors-native-http.md) integrados para llamar a un flujo de trabajo secundario. Los flujos de trabajo sin estado secundarios que no incluyen una acción Respuesta devuelven inmediatamente una respuesta `202 ACCEPTED`, pero el primario espera a que el secundario finalice antes de continuar con la siguiente acción. Estos comportamientos solo se aplican a los flujos de trabajo sin estado secundarios.

En esta tabla se especifica el comportamiento del flujo de trabajo secundario en función de si los flujos de trabajo primario y secundario son con estado, sin estado o tipos de flujo de trabajo mixtos:

| Flujo de trabajo primario | Flujo de trabajo secundario | Comportamiento del secundario |
|-----------------|----------------|----------------|
| Con estado | Con estado | Asincrónico o sincrónico con la configuración `"operationOptions": "DisableAsyncPattern"` |
| Con estado | Sin estado | Desencadenar y esperar |
| Sin estado | Con estado | Sincrónico |
| Sin estado | Sin estado | Desencadenar y esperar |
||||

<a name="other-capabilities"></a>

## <a name="other-single-tenant-model-capabilities"></a>Otras funcionalidades del modelo de un solo inquilino

El modelo de un solo inquilino y el tipo de recurso **Logic Apps (estándar)** incluyen muchas funcionalidades nuevas y actuales, como las siguientes:

* Creación de aplicaciones lógicas y sus flujos de trabajo a partir de [más de 400 conectores administrados](/connectors/connector-reference/connector-reference-logicapps-connectors) para aplicaciones y servicios de Software como servicio (SaaS) y Plataforma como servicio (PaaS), además de conectores para sistemas locales.

  * Ahora hay más conectores administrados disponibles como operaciones integradas y se ejecutan de forma similar a otras operaciones integradas, como Azure Functions. Las operaciones integradas se ejecutan de forma nativa en el runtime de Azure Logic Apps de un solo inquilino. Por ejemplo, las nuevas operaciones integradas incluyen Azure Service Bus, Azure Event Hubs, SQL Server, MQ, DB2 e IBM Host File.

    > [!NOTE]
    > En el caso de la versión integrada de SQL Server, solo la acción **Ejecutar consulta** puede conectarse directamente a redes virtuales de Azure sin necesitar la [puerta de enlace de datos local](logic-apps-gateway-connection.md).

  * Puede crear sus propios conectores integrados para cualquier servicio que necesite mediante el [marco de extensibilidad de Azure Logic Apps de un solo inquilino](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-running-anywhere-built-in-connector/ba-p/1921272). De forma similar a las operaciones integradas, como Azure Service Bus y SQL Server, pero a diferencia de los [conectores administrados personalizados](../connectors/apis-list.md#custom-apis-and-connectors), que no se admiten actualmente, los conectores integrados personalizados proporcionan mayor rendimiento, baja latencia y conectividad local, porque se ejecutan en el mismo proceso que el runtime de un solo inquilino.

    La funcionalidad de creación solo está disponible actualmente en Visual Studio Code, pero no está habilitada de manera predeterminada. Para crear estos conectores, [cambie el proyecto de la extensión basada en agrupación (Node.js) a basado en paquetes NuGet (.NET)](create-single-tenant-workflows-visual-studio-code.md#enable-built-in-connector-authoring). Para obtener más información, vea [Azure Logic Apps en ejecución en cualquier ubicación: extensibilidad de los conectores integrados](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-running-anywhere-built-in-connector/ba-p/1921272).

  * Puede usar las acciones siguientes en operaciones de Liquid y XML sin ninguna cuenta de integración. Estas operaciones incluyen las acciones siguientes:

    * XML: **Transformar XML** y **Validación XML**

    * Liquid: **Transformar de JSON a JSON**, **Transformar de JSON a TEXT**, **Transformar de XML a JSON** y **Transformar de XML a TEXTO**

    > [!NOTE]
    > Para usar estas acciones en Azure Logic Apps de inquilino único (Estándar), debe tener mapas de Liquid, mapas de XML o esquemas de XML. Puede cargar estos artefactos en Azure Portal desde el menú de recursos de la aplicación lógica, en **Artifacts**, que incluye las secciones **Schemas** y **Maps**. O bien, puede agregar estos artefactos a la carpeta **Artifacts** de su proyecto de Visual Studio Code mediante las carpetas **Maps** y **Schemas** respectivas. A continuación, puede usar estos artefactos en varios flujos de trabajo dentro del *mismo recurso de aplicación lógica*.

  * Los recursos **Logic App (Standard)** pueden ejecutarse en cualquier lugar, ya que Azure Logic Apps genera cadenas de conexión de Firma de acceso compartido (SAS) que estas aplicaciones lógicas pueden usar para enviar solicitudes al punto de conexión en tiempo de ejecución de conexión a la nube. El servicio Azure Logic Apps guarda estas cadenas de conexión con otras opciones de configuración de la aplicación para que pueda almacenar fácilmente estos valores en Azure Key Vault al realizar la implementación en Azure.

    > [!NOTE]
    > De manera predeterminada, el tipo de recurso **Logic App (estándar)** tiene la [identidad administrada asignada por el sistema](create-managed-service-identity.md) habilitada automáticamente para autenticar conexiones en tiempo de ejecución. Esta identidad se diferencia de las credenciales de autenticación o de la cadena de conexión que se usan al crear una conexión. Si deshabilita esta identidad, las conexiones no funcionarán en tiempo d ejecución. Para ver este valor, en el menú de la aplicación lógica, en **Configuración**, seleccione **Identidad**.
    >
    > La identidad administrada asignada por el usuario no está disponible actualmente en el tipo de recurso **Logic App (Standard)** .

* Ejecute, pruebe y depure las aplicaciones lógicas y sus flujos de trabajo localmente en el entorno de desarrollo de Visual Studio Code.

  Antes de ejecutar y probar la aplicación lógica, puede facilitar la depuración si agrega y usa puntos de interrupción dentro del archivo **workflow.json** de un flujo de trabajo. Pero los puntos de interrupción solo se admiten en las acciones en este momento, no en los desencadenadores. Para obtener más información, vea [Creación de flujos de trabajo basados en un solo inquilino en Visual Studio Code](create-single-tenant-workflows-visual-studio-code.md#manage-breakpoints).

* Publique o implemente directamente las aplicaciones lógicas y sus flujos de trabajo desde Visual Studio Code en varios entornos de hospedaje, como Azure y Logic Apps habilitado para Azure Arc.

* Habilite las capacidades de registro de diagnóstico y seguimiento de la aplicación lógica mediante [Application Insights](../azure-monitor/app/app-insights-overview.md) cuando lo admitan la configuración de la aplicación lógica y la suscripción de Azure.

* Con el [plan prémium de Azure Functions](../azure-functions/functions-premium-plan.md) puede acceder a funcionalidades de red, como la conexión e integración de forma privada con redes virtuales de Azure, de manera similar a Azure Functions al crear e implementar las aplicaciones lógicas. Para más información, revise la siguiente documentación:

  * [Opciones de redes de Azure Functions](../azure-functions/functions-networking-options.md)

  * [Azure Logic Apps en ejecución en cualquier ubicación: posibilidades de conexión de red con Azure Logic Apps](https://techcommunity.microsoft.com/t5/integrations-on-azure/logic-apps-anywhere-networking-possibilities-with-logic-app/ba-p/2105047)

* Regenere las claves de acceso para las conexiones administradas usadas por flujos de trabajo individuales en un recurso **Logic Apps (estándar)** . Para esta tarea, [siga los mismos pasos que para el recurso **Logic Apps (consumo)** , pero en el nivel de flujo de trabajo individual](logic-apps-securing-a-logic-app.md#regenerate-access-keys), no en el nivel de recursos de la aplicación lógica.

<a name="limited-unavailable-unsupported"></a>

## <a name="changed-limited-unavailable-or-unsupported-capabilities"></a>Capacidades modificadas, limitadas, no disponibles o no admitidas

Para el recurso **Logic Apps (estándar)** , estas funcionalidades han cambiado, o bien están limitadas, no están disponibles o no se admiten:

* **Desencadenadores y acciones**: los desencadenadores y las acciones integrados se ejecutan de forma nativa en Azure Logic Apps, mientras que los conectores administrados se hospedan y se ejecutan en Azure. Algunos desencadenadores y acciones integrados no están disponibles, como Ventana deslizante, Batch, Azure App Service y Azure API Management. Para iniciar un flujo de trabajo con o sin estado, use el [desencadenador integrado Periodicidad, Solicitud, HTTP, HTTP Webhook, Event Hubs o Service Bus](../connectors/apis-list.md). En el diseñador, las acciones y los desencadenadores integrados aparecen en la pestaña **Integrados**.

  En los flujos de trabajo *con estado*, los [desencadenadores y las acciones del conector administrado](../connectors/managed.md) aparecen en la pestaña **Azure**, excepto las operaciones no disponibles que se enumeran a continuación. En el caso de los flujos de trabajo *sin estado*, la pestaña **Azure** no aparece cuando se selecciona un desencadenador. Solo puede seleccionar [*acciones* del conector administrado, no desencadenadores](../connectors/managed.md). Aunque puede habilitar conectores administrados hospedados por Azure para flujos de trabajo sin estado, el diseñador no muestra ningún desencadenador de conector administrado para que pueda agregarlo.

  > [!NOTE]
  > Para ejecutar localmente en Visual Studio Code, los desencadenadores y las acciones basados en webhook requieren configuración adicional. Para obtener más información, vea [Creación de flujos de trabajo basados en un solo inquilino en Visual Studio Code](create-single-tenant-workflows-visual-studio-code.md#webhook-setup).

  * Estos desencadenadores y acciones han cambiado o están limitados, no se admiten o no están disponibles:

    * Los [*desencadenadores* de la puerta de enlace de datos local](../connectors/managed.md#on-premises-connectors) no están disponibles, pero las acciones *sí*.

    * La acción integrada [Azure Functions: Elegir una función de Azure](logic-apps-azure-functions.md) es ahora **Operaciones de Azure Functions: Llamar a una función de Azure**. Esta acción solo funciona con las funciones que se crean a partir de la plantilla **Desencadenador HTTP**.

      En Azure Portal, puede seleccionar una función de desencadenador HTTP a la que tenga acceso mediante la creación de una conexión por medio de la experiencia del usuario. Si inspecciona la definición JSON de la acción de la función en la vista de código o el archivo **workflow.json** mediante Visual Studio Code, la acción se refiere a la función mediante una referencia `connectionName`. Esta versión abstrae la información de la función como una conexión que puede encontrar en el archivo **connections.json** del proyecto de aplicación lógica, que está disponible después de crear una conexión en Visual Studio Code.

      > [!NOTE]
      > En la versión de un solo inquilino, la acción de la función solo admite la autenticación de cadena de consulta. Azure Logic Apps obtiene la clave predeterminada de la función al realizar la conexión, la almacena en la configuración de la aplicación y la usa para la autenticación cuando se llama a la función.
      >
      > Al igual que con el modelo multiinquilino, si renueva esta clave, por ejemplo, mediante la experiencia de Azure Functions del portal, la acción de la función ya no funciona debido a la clave no válida. Para corregir este problema, debe volver a crear la conexión a la función a la que quiere llamar o actualizar la configuración de la aplicación con la nueva clave.

    * El nombre de la acción integrada, [Código en línea](logic-apps-add-run-inline-code.md), se ha cambiado por el **Operaciones de código en línea**; ya no requiere una cuenta de integración y se han [actualizado los límites](logic-apps-limits-and-config.md).

    * La acción integrada [Azure Logic Apps: Elegir un flujo de trabajo de aplicación lógica](logic-apps-http-endpoint.md) es ahora **Operaciones de flujo de trabajo: Invocar un flujo de trabajo en esta aplicación de flujo de trabajo**.

    * Algunos [desencadenadores y acciones de cuentas de integración](../connectors/managed.md#integration-account-connectors) no están disponibles, como las acciones de archivo plano, las acciones AS2 (V2) y las acciones RosettaNet.

    * Actualmente, no se admiten [conectores administrados personalizados](../connectors/apis-list.md#custom-apis-and-connectors). Sin embargo, puede crear *operaciones integradas personalizadas* si usa Visual Studio Code. Para obtener más información, revise [Creación de flujos de trabajo basados en un solo inquilino mediante Visual Studio Code](create-single-tenant-workflows-visual-studio-code.md#enable-built-in-connector-authoring).

* **Autenticación**: los siguientes tipos de autenticación no están disponibles actualmente para el tipo de recurso **Logic App (Standard)** :

  * Azure Active Directory Open Authentication (Azure AD OAuth) para llamadas entrantes a desencadenadores basados en solicitudes, como el desencadenador de solicitud y el desencadenador de webhook HTTP.

  * Identidad administrada asignada por el usuario. Actualmente, solo la identidad administrada asignada por el sistema está disponible y habilitada automáticamente.

* **Transformación XML**: la compatibilidad con las referencias a ensamblados desde mapas no está disponible actualmente. Además, actualmente solo se admite XSLT 1.0.

* **Depuración de puntos de interrupción en Visual Studio Code**: aunque puede agregar y usar puntos de interrupción dentro del archivo **workflow.json** de un flujo de trabajo, los puntos de interrupción solo se admiten en las acciones en este momento, no en los desencadenadores. Para obtener más información, vea [Creación de flujos de trabajo basados en un solo inquilino en Visual Studio Code](create-single-tenant-workflows-visual-studio-code.md#manage-breakpoints).

* **Historial de desencadenadores e historial de ejecución**: para el tipo de recurso **Logic Apps (estándar)** , el historial de desencadenadores y el historial de ejecución en Azure Portal aparecen en el nivel del flujo de trabajo, no en el nivel de la aplicación lógica. Para obtener más información, consulte [Creación de flujos de trabajo basados en un solo inquilino mediante Azure Portal](create-single-tenant-workflows-azure-portal.md).

* **Control de zoom**: el control de zoom no está disponible actualmente en el diseñador.

* **Destinos de implementación**: no se puede implementar el tipo de recurso **Logic Apps (estándar)** en un [entorno del servicio de integración (ISE)](connect-virtual-network-vnet-isolated-environment-overview.md) ni en ranuras de implementación de Azure.

* **Azure API Management**: actualmente no se puede importar el tipo de recurso **Logic Apps (estándar)** en Azure API Management. Sin embargo, puede importar el tipo de recurso **Logic Apps (consumo)** .

<a name="firewall-permissions"></a>

## <a name="strict-network-and-firewall-traffic-permissions"></a>Permisos estrictos de tráfico de red y firewall

Si el entorno tiene requisitos de red estrictos o firewalls que limitan el tráfico, debe permitir el acceso de las conexiones del desencadenador o la acción en los flujos de trabajo de la aplicación lógica. Para buscar los nombres de dominio completos (FQDN) de estas conexiones, revise las secciones correspondientes de estos temas:

* [Permisos de firewall para aplicaciones lógicas de inquilino único: Visual Studio Code](create-single-tenant-workflows-visual-studio-code.md#firewall-setup)
* [Permisos de firewall para aplicaciones lógicas de inquilino único: Azure Portal](create-single-tenant-workflows-azure-portal.md#firewall-setup)

## <a name="next-steps"></a>Pasos siguientes

* [Creación de flujos de trabajo basados en un solo inquilino en Azure Portal](create-single-tenant-workflows-azure-portal.md)
* [Creación de flujos de trabajo basados en un solo inquilino en Visual Studio Code](create-single-tenant-workflows-visual-studio-code.md)

También nos gustaría conocer sus experiencias con Azure Logic Apps de un solo inquilino.

* En caso de errores o problemas, [dirija los problemas a GitHub](https://github.com/Azure/logicapps/issues).
* Si tiene preguntas, solicitudes, ideas y comentarios, [use este formulario de comentarios](https://aka.ms/lafeedback).
