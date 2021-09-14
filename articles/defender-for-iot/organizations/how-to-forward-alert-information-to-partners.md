---
title: Reenvío de la información de las alertas
description: Puede enviar información de alertas a los sistemas asociados mediante las reglas de reenvío.
ms.date: 08/29/2021
ms.topic: how-to
ms.openlocfilehash: 2136a58a383bb623edca69cb03c1c9c5530a107f
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2021
ms.locfileid: "123432406"
---
# <a name="forward-alert-information"></a>Reenvío de la información de las alertas

Puede enviar información de alertas a los asociados que están integrados con Azure Defender para IoT, a los servidores de syslog, a las direcciones de correo electrónico y más. Trabajar con reglas de reenvío le permite enviar rápidamente información de alertas a las partes interesadas en seguridad.

Defina los criterios por los que se desencadenará una regla de reenvío. El uso de criterios en las reglas de reenvío ayuda a identificar y administrar el volumen de información que se envía del sensor a los sistemas externos.

Syslog y otras acciones de reenvío predeterminadas se entregan a través del sistema. Es posible que haya más acciones de reenvío disponibles al integrarse con proveedores asociados, como Microsoft Azure Sentinel, ServiceNow o Splunk.

:::image type="content" source="media/how-to-work-with-alerts-sensor/alert-information-screen.png" alt-text="Información de alertas.":::

Los administradores de Defender para IoT tienen permiso para usar reglas de reenvío.

## <a name="about-forwarded-alert-information"></a>Acerca de la información de alertas reenviada

Las alertas proporcionan información acerca de una amplia gama de eventos operativos y de seguridad. Por ejemplo:

- Fecha y hora de la alerta

- Motor que detectó el evento

- Título de la alerta y mensaje descriptivo

- Gravedad de la alerta

- Nombre y dirección IP del origen y destino

- Tráfico sospechoso detectado

:::image type="content" source="media/how-to-work-with-alerts-sensor/address-scan-detected-screen.png" alt-text="Se detectó un análisis de direcciones.":::

La información pertinente se envía a los sistemas asociados cuando se han creado reglas de reenvío.

## <a name="about-forwarding-rules-and-certificates"></a>Acerca de las reglas de reenvío y los certificados

Algunas reglas de reenvío permiten el cifrado y la validación de certificados entre el sensor o la consola de administración local, y el agente del proveedor integrado.

En estos casos, el sensor o la consola de administración local es el cliente e iniciador de la sesión.  Los certificados se reciben normalmente del servidor, o bien usan el cifrado asimétrico, donde se proporcionará un certificado específico para configurar la integración.

El sistema Defender para IoT se ha configurado para validar certificados o omitir la validación de certificados.  Vea [Acerca de la validación de certificados](how-to-deploy-certificates.md#about-certificate-validation) para información sobre cómo habilitar y deshabilitar la validación.

Si la validación está habilitada y no se puede comprobar el certificado, se detendrá la comunicación entre Defender para IoT y el servidor.  El sensor mostrará un mensaje de error que indica el error de validación.  Si la validación está deshabilitada y el certificado no es válido, se seguirá llevando a cabo la comunicación.

Las siguientes reglas de reenvío permiten el cifrado y la validación de certificados:
- Syslog CEF
- Azure Sentinel
- QRadar

## <a name="create-forwarding-rules"></a>Creación de reglas de reenvío

**Para crear una regla de reenvío en un sensor**:

1. Inicie sesión en el sensor.

1. Seleccione **Reenvío** en el menú lateral.

1. Seleccione **Create Forwarding Rule** (crear una regla de reenvío).

   :::image type="content" source="media/how-to-work-with-alerts-sensor/create-forwarding-rule-screen.png" alt-text="Icono para crear una regla de reenvío.":::

1. Escriba el nombre de la regla de reenvío.

1. Seleccione el nivel de gravedad.

   este es el incidente mínimo que se reenviará, en términos de nivel de gravedad. Por ejemplo, si selecciona **leve**, se reenviarán las alertas de gravedad leve y todas las alertas por encima de este nivel de gravedad. Los niveles están predefinidos.

1. Seleccione los protocolos que se aplicarán.

   la regla de reenvío solo se activa si el tráfico detectado se estaba ejecutando a través de protocolos específicos. Seleccione los protocolos necesarios en la lista desplegable o selecciónelos todos.

1. Seleccione a qué motores se debe aplicar la regla.

   Seleccione los motores necesarios o selecciónelos todos. Se enviarán las alertas de los motores seleccionados. 

1. Seleccione la acción que se va a aplicar y rellene los parámetros necesarios para la acción seleccionada.

   Las acciones de regla de reenvío indican al sensor que reenvíe la información de alertas a los proveedores o servidores asociados. Puede crear varias acciones para cada regla de reenvío.

1. Agregue otra acción si lo desea.

1. Seleccione **Submit** (Enviar).

### <a name="email-address-action"></a>Acción de dirección de correo electrónico

Envíe un correo que incluya la información de la alerta. Puede especificar una dirección de correo electrónico por regla.

Para definir el correo electrónico de la regla de reenvío, haga lo siguiente:

1. Escriba una sola dirección de correo electrónico. Si tiene que agregar más de un correo electrónico, deberá crear otra acción para cada dirección de correo electrónico.

1. Escriba la zona horaria para la marca de tiempo de la detección de alertas en SIEM.

1. Seleccione **Submit** (Enviar).

### <a name="syslog-server-actions"></a>Acciones de servidor de Syslog

Se admiten los siguientes formatos:

- Mensajes de texto

- Mensajes CEF

- Mensajes LEEF

- Mensajes de objeto

:::image type="content" source="media/how-to-work-with-alerts-sensor/create-actions-rule.png" alt-text="Creación de las acciones de regla de reenvío.":::

Escriba los siguientes parámetros:

- Nombre de host y puerto de syslog.

- Protocolo TCP y UDP.

- Zona horaria para la marca de tiempo de la detección de alertas en SIEM.

- Archivo de certificado de cifrado TLS y archivo de clave para los servidores CEF (opcional).

:::image type="content" source="media/how-to-work-with-alerts-sensor/configure-encryption.png" alt-text="Configuración del cifrado para la regla de reenvío.":::

| Campos de salida del mensaje de texto de syslog | Descripción |
|--|--|
| Fecha y hora | Fecha y hora en que el equipo servidor de syslog recibió la información. |
| Priority | User.Alert |
| Hostname | Dirección IP del sensor |
| Protocolo | TCP o UDP |
| Message | Sensor: el nombre del sensor.<br /> Alerta: el título de la alerta.<br /> Escriba:  el tipo de la alerta. Puede ser **infracción del protocolo**, **infracción de la directiva**, **malware**, **anomalía** u **operativa**.<br /> Gravedad: Gravedad de la alerta. Puede ser **advertencia**, **leve**, **grave** o **crítica**.<br /> Origen: el nombre del dispositivo de origen.<br /> IP de origen: la dirección IP del dispositivo de origen.<br /> Destino: el nombre del dispositivo de destino.<br /> IP de destino: la dirección IP del dispositivo de destino.<br /> Mensaje: el mensaje de la alerta.<br /> Grupo de alertas: el grupo de alertas asociado a la alerta. |

| Salida del objeto syslog | Descripción |
|--|--|
| Fecha y hora | Fecha y hora en que el equipo servidor de syslog recibió la información. |  
| Priority | User.Alert |
| Hostname | Dirección IP del sensor |
| Message | Nombre del sensor: el nombre del dispositivo. <br /> Hora de la alerta: el momento en que se detectó la alerta: puede variar en función de la hora de la máquina del servidor de syslog y depende de la configuración de zona horaria de la regla de reenvío. <br /> Título de la alerta: el título de la alerta. <br /> Mensaje de alerta: el mensaje de la alerta. <br /> Gravedad de la alerta: la gravedad de la alerta: **Advertencia**, **leve**, **grave** o **crítica**. <br /> Tipo de alerta: **Infracción del protocolo**, **infracción de la directiva**, **malware**, **anomalía** u **operativa**. <br /> Protocolo: el protocolo de la alerta.  <br /> **Source_MAC**: dirección IP, nombre, proveedor o sistema operativo del dispositivo de origen. <br /> Destination_MAC: dirección IP, nombre, proveedor o sistema operativo del destino. Si faltan datos, el valor será **N/D**. <br /> alert_group: el grupo de alertas asociado a la alerta. |

| Formato de salida CEF de syslog | Descripción |
|--|--|
| Fecha y hora | Fecha y hora en que el equipo servidor de syslog recibió la información. |
| Priority | User.Alert |
| Hostname | Dirección IP del sensor |
| Message | CEF:0 <br />Azure Defender para IoT <br />Nombre del sensor: el nombre del dispositivo de sensor. <br />Versión del sensor <br />Título de la alerta: el título de la alerta. <br />msg: el mensaje de la alerta. <br />protocol: el protocolo de la alerta. <br />severity:  **advertencia**, **leve**, **grave** o **crítica**. <br />type:  **infracción del protocolo**, **infracción de la directiva**, **malware**, **anomalía** u **operativa**. <br /> start: el momento en que se detectó la alerta. <br />Puede variar en función de la hora de la máquina del servidor de syslog y depende de la configuración de zona horaria de la regla de reenvío. <br />src_ip: dirección IP del dispositivo de origen.  <br />dst_ip: dirección IP del dispositivo de destino.<br />cat: el grupo de alertas asociado a la alerta.  |

| Formato de salida LEEF de syslog | Descripción |
|--|--|
| Fecha y hora | Fecha y hora en que el equipo servidor de syslog recibió la información. |  
| Priority | User.Alert |
| Hostname | Dirección IP del sensor |
| Message | Nombre del sensor: el nombre del dispositivo de Azure Defender para IoT. <br />LEEF:1.0 <br />Azure Defender para IoT <br />Sensor  <br />Versión del sensor <br />Alerta de Azure Defender para IoT <br />título: el título de la alerta. <br />msg: el mensaje de la alerta. <br />protocol: el protocolo de la alerta.<br />severity:  **advertencia**, **leve**, **grave** o **crítica**. <br />type: el tipo de la alerta: **Infracción del protocolo**, **infracción de la directiva**, **malware**, **anomalía** u **operativa**. <br />start: la hora de la alerta.Puede ser diferente de la hora en la máquina del servidor de syslog. (Esto depende de la configuración de zona horaria). <br />src_ip: dirección IP del dispositivo de origen.<br />dst_ip: dirección IP del dispositivo de destino. <br />cat: el grupo de alertas asociado a la alerta. |

Después de escribir la información, seleccione **Enviar**.

### <a name="webhook-server-action"></a>Acción del servidor de webhooks

Envíe información de alertas a un servidor de webhooks. El trabajo con servidores de webhooks permite configurar integraciones que se suscriben a eventos de alerta con Defender para IoT. Cuando se desencadena un evento de alerta, la consola de administración envía una carga HTTP POST a la dirección URL configurada del webhook. Los webhooks se pueden usar para actualizar un sistema SIEM externo, sistemas SOAR, sistemas de administración de incidentes, etc.

**Para definir en una acción de webhook:**

1. Seleccione la acción de webhook.

   :::image type="content" source="media/how-to-work-with-alerts-sensor/webhook.png" alt-text="Defina una regla de reenvío de webhook.":::

1. Escriba la dirección del servidor en el campo **Dirección URL**.

1. En los campos **Clave** y **Valor**, personalice el encabezado HTTP con una definición de clave y valor. Las claves solo pueden contener letras, números, guiones y subrayados. Los valores solo pueden contener un espacio inicial o uno final.

1. Seleccione **Guardar**.

### <a name="webhook-extended"></a>Webhook extendido

Webhook extendido se puede usar para enviar datos adicionales al punto de conexión. La característica extendida incluye toda la información de la alerta de webhook y agrega la información siguiente al informe:

- sensorID
- sensorName
- zoneID
- zoneName
- siteID
- siteName
- sourceDeviceAddress
- destinationDeviceAddress
- remediationSteps
- handled
- additionalInformation

**Para definir una acción extendida de webhook**:

1. En la consola de administración, seleccione **Reenviar** en el panel de la izquierda.

1. Seleccione el botón :::image type="icon" source="media/how-to-forward-alert-information-to-partners/add-icon.png" border="false"::: para agregar una regla de reenvío.

1. Agregue un nombre descriptivo para la alerta de reenvío.

1. Seleccione un nivel de gravedad

1. Seleccione **Agregar**.

1. En la ventana desplegable Seleccionar tipo seleccione **Webhook Extended** (Webhook extendido).

   :::image type="content" source="media/how-to-forward-alert-information-to-partners/webhook-extended.png" alt-text="Selección de la opción webhook extended (webhook extendido) en el menú desplegable de opciones Seleccionar tipo.":::

1. Agregue la dirección URL de datos del punto de conexión en el campo URL.

1. (Opcional) Personalice el encabezado HTTP con una definición de clave y valor. Seleccione el botón :::image type="icon" source="media/how-to-forward-alert-information-to-partners/add-header.png" border="false"::: para agregar encabezados adicionales.

1. Seleccione **Guardar**.

Una vez que se configura la regla de reenvío Webhook extendido, puede probar la alerta desde la pantalla Reenvío de la consola de administración.

**Para probar la regla de reenvío Webhook extendido**:

1. En la consola de administración, seleccione **Reenviar** en el panel de la izquierda.

1. Seleccione el botón **ejecutar** para probar la alerta.

    :::image type="content" source="media/how-to-forward-alert-information-to-partners/run-button.png" alt-text="Selección del botón ejecutar para probar la regla de reenvío.":::

Sabrá que la regla de reenvío funciona si ve que aparece la notificación Correcto.

:::image type="content" source="media/how-to-forward-alert-information-to-partners/success.png" alt-text="La alerta ha funcionado correctamente si aparece la notificación Correcto.":::

### <a name="netwitness-action"></a>Acción de NetWitness

Envíe información de alertas a un servidor de NetWitness.

Para definir los parámetros de reenvío de NetWitness, haga lo siguiente:

1. Escriba la información de **nombre de host** y **puerto** de NetWitness.

1. Escriba la zona horaria para la marca de tiempo de la detección de alertas en SIEM.

   :::image type="content" source="media/how-to-work-with-alerts-sensor/add-timezone.png" alt-text="Agregue una zona horaria a la regla de reenvío.":::

1. Seleccione **Submit** (Enviar).

### <a name="integrated-vendor-actions"></a>Acciones de proveedor integradas

Es posible que haya integrado su sistema con un proveedor de seguridad, administración de dispositivos u otro sector. Estas integraciones le permiten hacer lo siguiente:

  - Enviar la información de las alertas.

  - Enviar la información del inventario de dispositivos.

  - Comunicarse con los firewalls del proveedor.

Las integraciones ayudan a conectar soluciones de seguridad que antes estaban aisladas, mejorar la visibilidad del dispositivo y acelerar la respuesta en todo el sistema para mitigar los riesgos con mayor rapidez.

Use la sección Acciones para especificar las credenciales y otra información necesaria para comunicarse con los proveedores integrados.

Para obtener más información sobre cómo configurar las reglas de reenvío para las integraciones, consulte los artículos de integración de asociados correspondientes.

## <a name="test-forwarding-rules"></a>Prueba de reglas de reenvío

Pruebe la conexión entre el sensor y el servidor de asociado que está definido en las reglas de reenvío:

1. Seleccione la regla en el cuadro de diálogo **Regla de reenvío**.

1. Seleccione el cuadro **Más**.

1. Seleccione **Enviar mensaje de prueba**.

1. Vaya al sistema de asociado para comprobar que se ha recibido la información enviada por el sensor.

## <a name="edit-and-delete-forwarding-rules"></a>Edición y eliminación de reglas de reenvío 

**Para editar una regla de reenvío**:

- En la pantalla **Regla de reenvío**, seleccione **Editar** en el menú desplegable **Más**. Realice los cambios necesarios y seleccione **Enviar**.

**Para eliminar una regla de reenvío**:

- En la pantalla **Regla de reenvío**, seleccione **Eliminar** en el menú desplegable **Más**. En el cuadro de diálogo **Advertencia**, seleccione **Aceptar**.

## <a name="forwarding-rules-and-alert-exclusion-rules"></a>Reglas de reenvío y reglas de exclusión de alertas

Puede que el administrador haya definido reglas de exclusión de alertas. Dichas reglas ayudan a los administradores a lograr un control más detallado sobre la activación de las alertas, ya que indican al sensor que omita los eventos de la alerta según varios parámetros. Estos parámetros pueden incluir direcciones de dispositivo, nombres de alerta o sensores específicos.

Es decir, que las reglas de reenvío que defina podrían omitirse en función de las reglas de exclusión que haya creado el administrador. Las reglas de exclusión se definen en la consola de administración local.

## <a name="see-also"></a>Consulte también

[Aceleración de los flujos de trabajo de alertas](how-to-accelerate-alert-incident-response.md)
