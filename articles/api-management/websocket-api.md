---
title: Importación de la API de WebSocket con Azure Portal | Microsoft Docs
titleSuffix: ''
description: Obtenga información sobre cómo API Management admite WebSocket, agregue una API de WebSocket y conozca las limitaciones de WebSocket.
ms.service: api-management
author: dlepow
ms.author: danlep
ms.topic: how-to
ms.date: 11/2/2021
ms.custom: template-how-to, ignite-fall-2021
ms.openlocfilehash: 027a87a7502f551b7fb97d52a732a90bc0b8aa45
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131065691"
---
# <a name="import-a-websocket-api"></a>Importación de WebSocket API

Con la solución de la API de WebSocket de API Management, ahora puede administrar, proteger, observar y exponer tanto las API de WebSocket como las API REST con API Management, así como proporcionar un centro de conectividad para detectar y consumir todas las API. Los publicadores de API pueden agregar rápidamente una API de WebSocket en API Management a través de:
* Un gesto sencillo en Azure Portal, y 
* La API de administración y Azure Resource Manager. 

Puede proteger las API de WebSocket aplicando las directivas de control de acceso existentes, como la [validación de JWT](./api-management-access-restriction-policies.md#ValidateJWT). También puede probar las API de WebSocket mediante las consolas de prueba de las API en Azure Portal y el portal para desarrolladores. A través de las funcionalidades de observabilidad existentes, API Management proporciona métricas y registros para supervisar y solucionar problemas de las API de WebSocket. 

En este artículo:
> [!div class="checklist"]
> * Comprenderá el flujo de tráfico de Websocket.
> * Agregará una API de WebSocket a la instancia de API Management.
> * Probará la API de WebSocket.
> * Visualizará las métricas y los registros de la API de WebSocket.
> * Conocerá las limitaciones de la API de WebSocket.

## <a name="prerequisites"></a>Requisitos previos

- Tener una instancia de API Management existente. [Cree una suscripción si todavía no lo ha hecho](get-started-create-service-instance.md).
- Una API de WebSocket. 

## <a name="websocket-passthrough"></a>Tráfico de WebSocket

API Management admite el tráfico de WebSocket. 

:::image type="content" source="./media/websocket-api/websocket-api-passthrough.png" alt-text="Ilustración visual del flujo de tráfico de WebSocket":::

Durante el tráfico de WebSocket, la aplicación cliente establece una conexión de WebSocket con la puerta de enlace de API Management, que a su vez establece una conexión con los servicios back-end correspondientes. A continuación, API Management envía mediante proxy los mensajes entre cliente y servidor de WebSocket.

1. La aplicación cliente envía una solicitud de protocolo de enlace WebSocket a la puerta de enlace de APIM al invocar la operación onHandshake.
1. La puerta de enlace de APIM envía una solicitud de protocolo de enlace WebSocket al servicio back-end correspondiente.
1. El servicio back-end actualiza una conexión a WebSocket.
1. La puerta de enlace de APIM actualiza la conexión correspondiente a WebSocket.
1. Una vez establecido el par de conexión, APIM gestionará los mensajes entre la aplicación cliente y el servicio back-end.
1. La aplicación cliente envía un mensaje a la puerta de enlace de APIM.
1. La puerta de enlace de APIM reenvía el mensaje al servicio back-end.
1. El servicio back-end envía un mensaje a la puerta de enlace de APIM.
1. La puerta de enlace de APIM reenvía el mensaje a la aplicación cliente.
1. Cuando cualquiera de los lados se desconecta, APIM termina la conexión correspondiente.

> [!NOTE]
> Las conexiones del cliente y del back-end constan de una asignación uno a uno. 

## <a name="onhandshake-operation"></a>Operación onHandshake

Según el [protocolo WebSocket](https://tools.ietf.org/html/rfc6455), cuando una aplicación cliente intenta establecer una conexión WebSocket con un servicio back-end, primero enviará una [solicitud de protocolo de enlace de apertura](https://tools.ietf.org/html/rfc6455#page-6). Cada API de WebSocket en API Management tiene una operación onHandshake. La operación onHandshake es una operación del sistema inmutable, que no se puede eliminar y que se crea automáticamente. La operación onHandshake permite a los publicadores de API interceptar estas solicitudes de protocolo de enlace y aplicarles las directivas de API Management.

:::image type="content" source="./media/websocket-api/onhandshake-screen.png" alt-text="Ejemplo de pantalla de onHandshake":::

## <a name="add-a-websocket-api"></a>Adición de una API de WebSocket

1. Navegue hasta su instancia de API Management.
1. En el menú de navegación lateral, en la sección **API**, seleccione **API**.
1. En **Definir una API**, seleccione el icono **WebSocket**.
1. En el cuadro de diálogo, seleccione **Completo** y rellene los campos necesarios del formulario.

    | Campo | Descripción |
    |----------------|-------|
    | Nombre para mostrar | Nombre con el que se mostrará la API de WebSocket. |
    | Nombre | Nombre sin procesar de la API de WebSocket. Se rellena automáticamente a medida que escribe el nombre para mostrar. |
    | URL de WebSocket | Dirección URL base con el nombre de WebSocket. Por ejemplo: *ws://example.com/nombre_del_socket* |
    | Esquema URL | Acepte el valor predeterminado. |
    | Sufijo de dirección URL de API| Agregue un sufijo de URL para identificar esta API específica en esta instancia de API Management. Debe ser exclusivo en esta instancia de APIM. |
    | Productos | Asocie la API de WebSocket a un producto para publicarla. |
    | Puertas de enlace | Asocie la API de WebSocket a las puertas de enlace existentes. |
 
1. Haga clic en **Crear**.

## <a name="test-your-websocket-api"></a>Prueba de la API de WebSocket

1. Navegue hasta la API de WebSocket.
1. En la API de WebSocket, seleccione la operación onHandshake.
1. Seleccione la pestaña **Prueba** para acceder a la consola Prueba. 
1. Opcionalmente, proporcione los parámetros de cadena de consulta necesarios para el protocolo de enlace WebSocket.

    :::image type="content" source="./media/websocket-api/test-websocket-api.png" alt-text="Ejemplo de API de prueba":::

1. Haga clic en **Conectar**.
1. Vea el estado de la conexión en **Salida**.
1. Escriba el valor en **Carga**. 
1. Haga clic en **Enviar**.
1. Vea los mensajes recibidos en **Salida**.
1. Repita los pasos anteriores para probar diferentes cargas.
1. Cuando se complete la prueba, seleccione **Desconectar**.

## <a name="view-metrics-and-logs"></a>Visualización de métricas y registros

Use características estándar de API Management y Azure Monitor para [supervisar](api-management-howto-use-azure-monitor.md) las API de WebSocket:

* Visualización de métricas de API en Azure Monitor
* Opcionalmente, habilite la configuración de diagnóstico para recopilar y ver los registros de puerta de enlace de API Management, que incluyen operaciones de API de WebSocket.

Por ejemplo, en la captura de pantalla siguiente se muestran las respuestas recientes de la API de WebSocket con código `101` de la tabla **ApiManagementGatewayLogs**. Estos resultados indican el cambio correcto de las solicitudes de TCP al protocolo WebSocket.

:::image type="content" source="./media/websocket-api/query-gateway-logs.png" alt-text="Consulta de registros para solicitudes de API de WebSocket":::

## <a name="limitations"></a>Limitaciones

A continuación se muestran las restricciones actuales de compatibilidad con WebSocket en API Management:

* Las API de WebSocket no se admiten aún en el nivel de consumo.
* Las API de WebSocket no se admiten aún en la [puerta de enlace autohospedada](./self-hosted-gateway-overview.md).
* La CLI de Azure, PowerShell y el SDK no admiten actualmente operaciones de administración de las API de WebSocket.

### <a name="unsupported-policies"></a>Directivas no admitidas

Las siguientes directivas no son compatibles con la operación onHandshake y no se pueden aplicar a ella:
*  Similar respuesta
* Obtención desde memoria caché
* Almacenar en caché
* Permitir llamadas entre dominios
* CORS
* JSONP
*  Establecimiento de método de solicitud
* Establecer cuerpo
* Convertir XML a JSON
* Convertir JSON a XML
* Transformar XML mediante XSLT
* Validar contenido
* Validación de parámetros
* Validación de encabezados
* Validación de código de estado

> [!NOTE]
> Si aplicó las directivas en ámbitos superiores (es decir, a nivel global o de producto) y una API de WebSocket las heredó a través de la directiva, se omitirán en tiempo de ejecución.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Transformación y protección de una API publicada](transform-api.md)
