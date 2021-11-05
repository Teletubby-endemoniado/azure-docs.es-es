---
title: Uso de la licencia de Microsoft Defender para punto de conexión incluida con Microsoft Defender for Cloud
description: Obtenga información acerca de Microsoft Defender para punto de conexión e implemente esta solución desde Microsoft Defender for Cloud.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 10/08/2021
ms.author: memildin
ms.custom: ignite-fall-2021
ms.openlocfilehash: 9441d285a97ca4c3a1ee46ab40c49f71f5d405f3
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131017464"
---
# <a name="protect-your-endpoints-with-defender-for-clouds-integrated-edr-solution-microsoft-defender-for-endpoint"></a>Protección de los puntos de conexión con la solución EDR integrada de Defender for Cloud: Microsoft Defender para punto de conexión

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

Microsoft Defender para punto de conexión es una solución integral de seguridad de punto de conexión que se entrega en la nube. Sus principales características son:

- Evaluación y administración de vulnerabilidades basadas en riesgos. 
- Reducción de la superficie expuesta a ataques
- Protección basada en el comportamiento y con tecnología de nube.
- Detección y respuesta de puntos de conexión (EDR).
- Investigación y corrección automatizadas.
- Servicios de búsqueda administrados.

> [!TIP]
> Originalmente se inició como **ATP de Windows Defender**, pero se cambió el nombre de este producto de Detección y respuesta de puntos de conexión (EDR) en 2019 a **ATP de Microsoft Defender**.
>
> En Ignite 2020, lanzamos el [conjunto Microsoft Defender XDR](https://www.microsoft.com/security/business/threat-protection) y se cambió el nombre del componente EDR a **Microsoft Defender para punto de conexión**.


## <a name="availability"></a>Disponibilidad

| Aspecto                                       | Detalles                                                                                                                                                                                                                                                                               |
|----------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Estado de la versión:                               | • Integración con Defender para punto de conexión para Windows, disponibilidad general (GA)<br> • Integración con Defender para punto de conexión para Linux, versión preliminar                                                                                                                                     |
| Precios:                                     | Requiere [Microsoft Defender para servidores](defender-for-servers-introduction.md).                                                                                                                                                                                                           |
| Entornos admitidos:                      | :::image type="icon" source="./media/icons/yes-icon.png"::: Máquinas de Azure Arc que ejecutan Windows o Linux<br>:::image type="icon" source="./media/icons/yes-icon.png":::Máquinas virtuales de Azure que ejecutan Linux ([versiones admitidas](/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint-linux))<br>:::image type="icon" source="./media/icons/yes-icon.png"::: VM de Azure que ejecutan Windows Server 2022, 2019, 2016, 2012 R2 o 2008 R2 SP1, [Windows Virtual Desktop (WVD)](../virtual-desktop/overview.md) o la [Sesión múltiple de Windows 10 Enterprise](../virtual-desktop/windows-10-multisession-faq.yml) (anteriormente Enterprise para escritorios virtuales [EVD]).<br>:::image type="icon" source="./media/icons/no-icon.png"::: Máquinas virtuales de Azure que ejecutan Windows 10 (excepto EVD o WVD)           |
| Roles y permisos necesarios:              | • Para habilitar o deshabilitar la integración: **Administrador de seguridad** o **Propietario**<br>• Para ver alertas de Defender para punto de conexión en Defender for Cloud:**Lector de seguridad**, **Lector**, **Colaborador de grupo de recursos**, **Propietario de grupo de recursos**, **Administrador de seguridad**, **Propietario de suscripción** o **Colaborador de suscripción** |
| Nubes:                                      | :::image type="icon" source="./media/icons/yes-icon.png"::: Nubes comerciales<br>:::image type="icon" source="./media/icons/yes-icon.png"::: Azure Government<br>:::image type="icon" source="./media/icons/no-icon.png"::: Azure China 21Vianet                                                         |
|                                              |                                                                                                                                                                                                                                                                                       |

## <a name="benefits-of-integrating-microsoft-defender-for-endpoint-with-defender-for-cloud"></a>Ventajas de integrar Microsoft Defender para punto de conexión con Defender for Cloud

Microsoft Defender para punto de conexión proporciona lo siguiente:

- **Sensores de detección avanzados posteriores a las brechas**. Los sensores de Defender para punto de conexión recopilan una amplia gama de señales de comportamiento de sus máquinas.

- **Evaluación de vulnerabilidades desde la solución de administración de amenazas y vulnerabilidades de Microsoft**. Con Microsoft Defender para punto de conexión habilitado, Defender for Cloud puede mostrar las vulnerabilidades detectadas por el módulo de administración de amenazas y vulnerabilidades, así como ofrecer este módulo como una solución de evaluación de vulnerabilidades compatible. Obtenga más información en [Investigación de puntos débiles con la solución de administración de amenazas y vulnerabilidades de Microsoft Defender para punto de conexión](deploy-vulnerability-assessment-tvm.md).

    Este módulo también incluye las características de inventario de software descritas en [Acceso a un inventario de software](asset-inventory.md#access-a-software-inventory) y se puede habilitar automáticamente para las máquinas compatibles con la [configuración de implementación automática](auto-deploy-vulnerability-assessment.md).

- **Detección posterior a las brechas basada en el análisis y con tecnología de nube**. Defender para punto de conexión se adapta rápidamente a las amenazas cambiantes. Utiliza análisis avanzados y macrodatos. Asimismo, se amplía gracias a la eficacia de Intelligent Security Graph con señales a través de Windows, Azure y Office para detectar amenazas desconocidas. Proporciona alertas que requieren intervención y le permite responder rápidamente.

- **Información sobre amenazas**. Defender para punto de conexión crea alertas cuando identifica las herramientas, las técnicas y los procedimientos de los atacantes. Usa datos generados por los equipos de seguridad y los buscadores de amenazas de Microsoft, con la ayuda de mecanismos de inteligencia que aportan los partners.

Integrando Defender para punto de conexión con Defender for Cloud, se beneficiará de las siguientes funcionalidades extras:

- **Incorporación automatizada**. Defender for Cloud habilita automáticamente el sensor de Defender para punto de conexión en todas las máquinas admitidas que estén conectadas a Defender for Cloud.

- **Panel único**. En las páginas del portal de Defender for Cloud se muestran alertas de Defender para punto de conexión. Para obtener más detalles, use las páginas del portal de Microsoft Defender para punto de conexión, donde verá información adicional, como el árbol de procesos de alerta y el gráfico de incidentes. También puede ver una escala de tiempo de la máquina detallada que muestra cada comportamiento durante un período histórico de hasta seis meses.

    :::image type="content" source="./media/integration-defender-for-endpoint/microsoft-defender-security-center.png" alt-text="Security Center en Microsoft Defender para punto de conexión" lightbox="./media/integration-defender-for-endpoint/microsoft-defender-security-center.png":::

## <a name="what-are-the-requirements-for-the-microsoft-defender-for-endpoint-tenant"></a>¿Cuáles son los requisitos del inquilino de Microsoft Defender para punto de conexión?

Al usar Defender for Cloud para supervisar sus máquinas, se crea automáticamente un inquilino de  Defender para punto de conexión.

- **Ubicación:** los datos que recopila Defender para punto de conexión se almacenan en la ubicación geográfica del inquilino identificada durante el aprovisionamiento. Los datos de cliente en formato seudonimizado también se pueden almacenar en los sistemas de procesamiento y almacenamiento central en el Estados Unidos. Una vez configurada la ubicación, no se puede cambiar. Si tiene su propia licencia de Microsoft Defender para punto de conexión y necesita trasladar los datos a otra ubicación, [póngase en contacto con el Soporte técnico de Microsoft](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) para restablecer el inquilino.
- **Mover suscripciones:** si movió la suscripción de Azure entre inquilinos de Azure, se requieren algunos pasos de preparación manuales para que Defender for Cloud pueda implementar Defender para punto de conexión. Para obtener más información, [póngase en contacto con el soporte técnico de Microsoft](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).


## <a name="enable-the-microsoft-defender-for-endpoint-integration"></a>Habilitación de la integración de Microsoft Defender para punto de conexión

### <a name="prerequisites"></a>Requisitos previos

Compruebe que el equipo cumple los requisitos necesarios para Defender para punto de conexión:

1. Asegúrese de que la máquina está conectada a Azure y a Internet según sea necesario:

    - **Máquinas virtuales de Azure (Windows o Linux)** : configure las opciones de red que se describen en "Configuración del proxy de dispositivo y la configuración de conectividad a Internet" ([Windows](/windows/security/threat-protection/microsoft-defender-atp/configure-proxy-internet) o [Linux](/microsoft-365/security/defender-endpoint/linux-static-proxy-configuration)).

    - **Máquinas locales**: conecte sus máquinas de destino a Azure Arc como se explica en [Conexión de máquinas híbridas con servidores habilitados para Azure Arc](../azure-arc/servers/learn/quick-enable-hybrid-vm.md).

1. Habilitar **Microsoft Defender para servidores**. Consulte [Inicio rápido: Habilitación de las características de seguridad mejoradas de Defender for Cloud](enable-enhanced-security.md).

    > [!IMPORTANT]
    > La integración de Defender for Cloud con Microsoft Defender para punto de conexión está habilitada de manera predeterminada. Por lo tanto, al habilitar las características de seguridad mejoradas, da su consentimiento para que Microsoft Defender para servidores acceda a los datos de Microsoft Defender para punto de conexión relacionados con vulnerabilidades, software instalado y alertas de sus puntos de conexión.

1. Si ha movido la suscripción entre inquilinos de Azure, también se requieren algunos pasos de preparación manuales. Para obtener más información, [póngase en contacto con el soporte técnico de Microsoft](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).




### <a name="enable-the-integration"></a>Habilitación de la integración

### <a name="windows"></a>[**Windows**](#tab/windows)

1. En el menú de Defender for Cloud, seleccione **Configuración del entorno** y, a continuación, la suscripción con las máquinas de Windows en las que quiera recibir Defender para punto de conexión.

1. Seleccione **Integraciones**.

1. Seleccione **Permitir que Microsoft Defender for Endpoint acceda a mis datos** y luego **Guardar**.

    :::image type="content" source="./media/integration-defender-for-endpoint/enable-integration-with-edr.png" alt-text="Habilitar la integración entre Microsoft Defender for Cloud y la solución EDR de Microsoft, Microsoft Defender para punto de conexión":::

    Microsoft Defender for Cloud incorporará automáticamente sus máquinas en Microsoft Defender para punto de conexión. Este proceso de incorporación puede tardar hasta 24 horas.

### <a name="linux-preview"></a>[**Linux** (versión preliminar)](#tab/linux)

Durante el período de versión preliminar, puede implementar Defender para punto de conexión en sus máquinas de Linux de una de estas dos maneras, en función de si ya lo ha implementado en las máquinas de Windows:

- [Usuarios existentes con las características de seguridad mejorada de Defender para la nube habilitadas y Microsoft Defender para punto de conexión para Windows](#existing-users-with-defender-for-clouds-enhanced-security-features-enabled-and-microsoft-defender-for-endpoint-for-windows)
- [Nuevos usuarios que no han habilitado nunca la integración con Microsoft Defender para punto de conexión para Windows](#new-users-whove-never-enabled-the-integration-with-microsoft-defender-for-endpoint-for-windows)


### <a name="existing-users-with-defender-for-clouds-enhanced-security-features-enabled-and-microsoft-defender-for-endpoint-for-windows"></a>Usuarios existentes con las características de seguridad mejorada de Defender para la nube habilitadas y Microsoft Defender para punto de conexión para Windows

Si ya ha habilitado la integración con **Defender para punto de conexión para Windows**, tiene plena capacidad de decisión sobre si implementar Defender para punto de conexión en sus máquinas de **Linux** y cuándo hacerlo.

1. En el menú de Defender for Cloud, seleccione **Configuración del entorno** y, a continuación, la suscripción con las máquinas de Linux en las que quiera recibir Defender para punto de conexión.

1. Seleccione **Integraciones**. Sabrá que la integración está habilitada si la casilla **Permitir que Microsoft Defender para punto de conexión acceda a mis datos** está seleccionada como se muestra a continuación:

    :::image type="content" source="./media/integration-defender-for-endpoint/integration-enabled.png" alt-text="La integración entre Microsoft Defender for Cloud y la solución EDR de Microsoft, Microsoft Defender para punto de conexión, se ha habilitado":::

    > [!NOTE]
    > Si no está seleccionada, siga las instrucciones que se indican en [Nuevos usuarios que no han habilitado nunca la integración con Microsoft Defender para punto de conexión para Windows](#new-users-whove-never-enabled-the-integration-with-microsoft-defender-for-endpoint-for-windows).

    Con esta versión preliminar, se ha agregado un nuevo botón, **Habilitar para máquinas de Linux**, debajo de esta casilla:

    :::image type="content" source="./media/integration-defender-for-endpoint/deploy-to-linux.png" alt-text="La versión preliminar introduce un botón con el que se puede controlar manualmente cuándo implementar Defender para punto de conexión para Linux.":::

1. Para agregar sus máquinas de Linux a la integración

    1. Seleccione **Habilitar para máquinas de Linux**.
    1. Seleccione **Guardar**.
    1. En el mensaje de confirmación, verifique la información y seleccione **Habilitar** si desea continuar. 

    :::image type="content" source="./media/integration-defender-for-endpoint/enable-for-linux-result.png" alt-text="Confirmación de la integración entre Defender for Cloud y la solución EDR de Microsoft, Microsoft Defender para punto de conexión para Linux":::

    Microsoft Defender for Cloud hará lo siguiente:

    - Incorporar automáticamente sus máquinas de Linux en Defender para punto de conexión.
    - Omitir las máquinas que ejecuten otras soluciones basadas en fanotify. (Consulte los detalles de la `fanotify`opción de kernel necesaria en los [requisitos del sistema Linux](/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint-linux#system-requirements)).
    - Detectar instalaciones anteriores de Defender para punto de conexión y volver a configurarlas para que se integren con Defender for Cloud.

    Este proceso de incorporación puede tardar hasta 1 hora.

    > [!NOTE]
    > La próxima vez que vuelva a esta página de Azure Portal, no se mostrará el botón **Habilitar para máquinas de Linux**. Para deshabilitar la integración para Linux, deberá deshabilitarla también para Windows desactivando la casilla **Permitir que Microsoft Defender para punto de conexión acceda a mis datos** y seleccionando **Guardar**.


1. Para comprobar la instalación de Defender para punto de conexión en una máquina de Linux, ejecute el siguiente comando de shell en las máquinas:

    `mdatp health`

    Si está instalado Microsoft Defender para punto de conexión, aparecerá su estado de mantenimiento:

    `healthy : true`

    `licensed: true`

    Además, en Azure Portal, aparecerá una nueva extensión de Azure en las máquinas denominada `MDE.Linux`.

### <a name="new-users-whove-never-enabled-the-integration-with-microsoft-defender-for-endpoint-for-windows"></a>Nuevos usuarios que no han habilitado nunca la integración con Microsoft Defender para punto de conexión para Windows

Si no ha habilitado nunca la integración para Windows, la opción **Permitir que Microsoft Defender para punto de conexión acceda a mis datos** permitirá que Defender for Cloud implemente Defender para punto de conexión en sus máquinas de Windows *y* Linux.

1. En el menú de Defender for Cloud, seleccione **Configuración del entorno** y, a continuación, la suscripción con las máquinas de Linux en las que quiera recibir Defender para punto de conexión.

1. Seleccione **Integraciones**.

1. Seleccione **Permitir que Microsoft Defender for Endpoint acceda a mis datos** y luego **Guardar**.

    Microsoft Defender for Cloud hará lo siguiente:

    - Incorporar automáticamente sus máquinas de Windows y Linux en Defender para punto de conexión.
    - Omitir las máquinas de Linux que ejecuten otras soluciones basadas en fanotify. (Consulte los detalles de la `fanotify`opción de kernel necesaria en los [requisitos del sistema Linux](/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint-linux#system-requirements)).
    - Detectar instalaciones anteriores de Defender para punto de conexión y volver a configurarlas para que se integren con Defender for Cloud.

    Este proceso de incorporación puede tardar hasta 1 hora.

1. Para comprobar la instalación de Defender para punto de conexión en una máquina de Linux, ejecute el siguiente comando de shell en las máquinas:

    `mdatp health`

    Si está instalado Microsoft Defender para punto de conexión, aparecerá su estado de mantenimiento:

    `healthy : true`

    `licensed: true`

    Además, en Azure Portal, aparecerá una nueva extensión de Azure en las máquinas, denominada `MDE.Linux`.
--- 

## <a name="access-the-microsoft-defender-for-endpoint-portal"></a>Acceso al portal de Microsoft Defender para punto de conexión

1. Asegúrese de que la cuenta de usuario tenga los permisos necesarios. Obtenga más información en [Asignar acceso de usuario al Centro de seguridad de Microsoft Defender](/windows/security/threat-protection/microsoft-defender-atp/assign-portal-access).

1. Compruebe si tiene un proxy o firewall que esté bloqueando el tráfico anónimo. El sensor de Defender para punto de conexión se conecta desde el contexto del sistema, por lo que se debe permitir el tráfico anónimo. Para garantizar un acceso sin obstáculos al portal de Defender para punto de conexión, siga las instrucciones de [Habilitar el acceso a las direcciones URL de servicio en el servidor proxy](/windows/security/threat-protection/microsoft-defender-atp/configure-proxy-internet#enable-access-to-microsoft-defender-atp-service-urls-in-the-proxy-server).

1. Abra el [portal de Security Center en Defender para punto de conexión](https://securitycenter.windows.com/). Obtenga más información sobre las características e iconos del portal, en [Información general del portal de Security Center en Defender para punto de conexión](/windows/security/threat-protection/microsoft-defender-atp/portal-overview). 






## <a name="send-a-test-alert"></a>Envío de una alerta de prueba

Para generar una alerta de prueba benigna desde Defender para punto de conexión, seleccione la pestaña del sistema operativo correspondiente del punto de conexión:

### <a name="windows"></a>[**Windows**](#tab/windows)

En el caso de puntos de conexión que ejecuten Windows:

1. Cree una carpeta "C:\test-MDATP-test".
1. Use el Escritorio remoto para acceder a su máquina.
1. Abra una ventana de línea de comandos.
1. En el símbolo del sistema, copie el siguiente comando y ejecútelo. La ventana del símbolo del sistema se cerrará automáticamente.

    ```powershell
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-MDATP-test\\invoice.exe'); Start-Process 'C:\\test-MDATP-test\\invoice.exe'
    ```
    :::image type="content" source="./media/integration-defender-for-endpoint/generate-edr-alert.png" alt-text="Ventana del símbolo del sistema con el comando para generar una alerta de prueba.":::

    Si el comando se ejecuta correctamente, verá una nueva alerta en el panel de Microsoft Defender for Cloud y el portal de Microsoft Defender para punto de conexión. Esta alerta puede tardar unos minutos en aparecer.
1. Para revisar la alerta en Defender for Cloud, vaya a **Alertas de seguridad** > **Línea de comandos de PowerShell sospechosa**.
1. En la ventana de la investigación, seleccione el vínculo para ir al portal de Microsoft Defender para punto de conexión.

    > [!TIP]
    > La alerta se desencadena con gravedad **Información**.


### <a name="linux"></a>[**Linux**](#tab/linux)

En el caso de puntos de conexión que ejecuten Linux:

1. Descargue la herramienta de alertas de prueba desde https://aka.ms/LinuxDIY.
1. Extraiga el contenido del archivo ZIP y ejecute este script de shell:

    `./mde_linux_edr_diy`

    Si el comando se ejecuta correctamente, verá una nueva alerta en el panel de Microsoft Defender for Cloud y el portal de Microsoft Defender para punto de conexión. Esta alerta puede tardar unos minutos en aparecer.

1. Para revisar la alerta en Defender for Cloud, vaya a **Alertas de seguridad** > **Enumeración de archivos con datos confidenciales**.
1. En la ventana de la investigación, seleccione el vínculo para ir al portal de Microsoft Defender para punto de conexión.

    > [!TIP]
    > La alerta se desencadena con una gravedad **Baja**.

--- 



## <a name="faq---defender-for-clouds-integration-with-microsoft-defender-for-endpoint"></a>Preguntas frecuentes: Integración de Defender for Cloud con Microsoft Defender para punto de conexión

- [¿Qué es esta extensión "MDE.Windows"/"MDE.Linux" que se está ejecutando en mi máquina?](#whats-this-mdewindows--mdelinux-extension-running-on-my-machine)
- [¿Cuáles son los requisitos de licencia de Microsoft Defender para punto de conexión?](#what-are-the-licensing-requirements-for-microsoft-defender-for-endpoint)
- [Si ya tengo una licencia de Microsoft Defender para punto de conexión, ¿puedo obtener un descuento para Microsoft Defender para servidores?](#if-i-already-have-a-license-for-microsoft-defender-for-endpoint-can-i-get-a-discount-for-microsoft-defender-for-servers)
- [¿Cómo puedo cambiar una herramienta de EDR de terceros?](#how-do-i-switch-from-a-third-party-edr-tool)

### <a name="whats-this-mdewindows--mdelinux-extension-running-on-my-machine"></a>¿Qué es esta extensión "MDE.Windows"/"MDE.Linux" que se está ejecutando en mi máquina?

En el pasado, el agente de Log Analytics aprovisionaba Microsoft Defender para punto de conexión. Cuando [ampliamos la compatibilidad para incluir Windows Server 2019](release-notes-archive.md#microsoft-defender-for-endpoint-integration-with-azure-defender-now-supports-windows-server-2019-and-windows-10-virtual-desktop-wvd-released-for-general-availability-ga) y Linux, también agregamos una extensión para realizar una incorporación automática. 

Defender for Cloud implementa automáticamente la extensión en máquinas que ejecutan los siguientes sistemas operativos:

- Windows Server 2019
- Windows 10 Virtual Desktop (WVD)
- Otras versiones de Windows Server si Defender for Cloud no reconoce la versión del sistema operativo (por ejemplo, cuando se usa una imagen de máquina virtual personalizada). En este caso, el agente de Log Analytics sigue aprovisionando Microsoft Defender para punto de conexión.
- Linux.

> [!IMPORTANT]
> Si elimina la extensión MDE.Windows, no quitará Microsoft Defender para punto de conexión. en "retirar", consulte [Retirada de los servidores de Windows](/microsoft-365/security/defender-endpoint/configure-server-endpoints).

### <a name="what-are-the-licensing-requirements-for-microsoft-defender-for-endpoint"></a>¿Cuáles son los requisitos de licencia de Microsoft Defender para punto de conexión?
Defender para punto de conexión se incluye sin coste adicional con **Microsoft Defender para servidores**. Como alternativa, se puede adquirir por separado para 50 máquinas o más.

### <a name="if-i-already-have-a-license-for-microsoft-defender-for-endpoint-can-i-get-a-discount-for-microsoft-defender-for-servers"></a>Si ya tengo una licencia de Microsoft Defender para punto de conexión, ¿puedo obtener un descuento para Microsoft Defender para servidores?
Si ya tiene una licencia de Microsoft Defender para punto de conexión, no tendrá que pagar esa parte de la licencia de Microsoft Defender para servidores.

Para solicitar el descuento, [póngase en contacto con el equipo de soporte técnico de Defender for Cloud](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). Tendrá que proporcionar la información pertinente de identificador de área de trabajo, región y número de licencias de Microsoft Defender para punto de conexión aplicadas a las máquinas del área de trabajo dada.

El descuento será efectivo a partir de la fecha de aprobación y no se aplicará con carácter retroactivo.

### <a name="how-do-i-switch-from-a-third-party-edr-tool"></a>¿Cómo puedo cambiar una herramienta de EDR de terceros?
Las instrucciones completas para cambiarse de una solución de punto de conexión que no es de Microsoft están disponibles en la documentación de Microsoft Defender para punto de conexión: [Información general sobre la migración](/windows/security/threat-protection/microsoft-defender-atp/switch-to-microsoft-defender-migration).


## <a name="next-steps"></a>Pasos siguientes

- [Plataformas y características compatibles con Microsoft Defender for Cloud](security-center-os-coverage.md)
- [Descubra cómo las recomendaciones le ayudan a proteger sus recursos de Azure.](review-security-recommendations.md)
