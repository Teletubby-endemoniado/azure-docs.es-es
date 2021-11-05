---
title: Conexión de máquinas que no son de Azure a Microsoft Defender for Cloud
description: Aprenda a conectar las máquinas que no son de Azure a Microsoft Defender for Cloud.
author: memildin
ms.author: memildin
ms.date: 07/12/2021
ms.topic: quickstart
ms.service: security-center
manager: rkarlin
zone_pivot_groups: non-azure-machines
ms.custom: ignite-fall-2021
ms.openlocfilehash: 0e5c44033a5f03726dc7e1f1129287edef5350aa
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131014435"
---
# <a name="connect-your-non-azure-machines-to-microsoft-defender-for-cloud"></a>Conexión de máquinas que no son de Azure a Microsoft Defender for Cloud

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

Defender for Cloud puede supervisar la situación de seguridad de los equipos que no son de Azure, pero para ello antes hay que conectarlos a Azure.

Puede conectar equipos que no son de Azure de alguna de las maneras siguientes:

- Mediante servidores habilitados para Azure Arc (**recomendado**)
- Desde las páginas de Cloud en Azure Portal (**Introducción** e **Inventario**)

En esta página se describen cada una de ellas.

::: zone pivot="azure-arc"

## <a name="add-non-azure-machines-with-azure-arc"></a>Incorporación de máquinas que no son de Azure con Azure Arc

La manera preferida de agregar máquinas que no son de Azure a Microsoft Defender for Cloud es con los [servidores habilitados para Azure Arc](../azure-arc/servers/overview.md).

Una máquina con servidores habilitados para Azure Arc se convierte en un recurso de Azure y, si ha instalado el agente de Log Analytics, aparece en Defender for Cloud con recomendaciones igual que los otros recursos de Azure.

Además, los servidores habilitados para Azure Arc proporcionan funcionalidades mejoradas, como la opción de habilitar directivas de configuración de invitados en la máquina, simplificar la implementación con otros servicios de Azure y muchas más. Para obtener información general sobre las ventajas, consulte [Operaciones en la nube admitidas](../azure-arc/servers/overview.md#supported-cloud-operations).

> [!NOTE]
> Las herramientas de implementación automática de Defender for Cloud para implementar el agente de Log Analytics no admiten máquinas que ejecutan Azure Arc. Cuando haya conectado las máquinas mediante Azure Arc, use la recomendación de Defender for Cloud pertinente para implementar el agente y beneficiarse de la gama completa de protecciones que ofrece Defender for Cloud:
>
> - [El agente de Log Analytics se debe instalar en las máquinas de Azure Arc basadas en Linux](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/720a3e77-0b9a-4fa9-98b6-ddf0fd7e32c1)
> - [El agente de Log Analytics se debe instalar en las máquinas de Azure Arc basadas en Windows](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/27ac71b1-75c5-41c2-adc2-858f5db45b08)

Más información sobre los [servidores habilitados para Azure Arc](../azure-arc/servers/overview.md).

**Para implementar Azure Arc:**

- En el caso de una máquina, siga las instrucciones de [Inicio rápido: Conexión de máquinas híbridas con servidores habilitados para Azure Arc](../azure-arc/servers/learn/quick-enable-hybrid-vm.md).
- Consulte [Conexión de máquinas híbridas a Azure a gran escala](../azure-arc/servers/onboard-service-principal.md) para conectar varias máquinas a gran escala a servidores habilitados para Azure Arc.

> [!TIP]
> Si va a incorporar máquinas que se ejecutan en Amazon Web Services (AWS), el conector para AWS de Defender for Cloud controla de forma transparente y automática la implementación de Azure Arc. Obtenga más información en [Conectar cuentas de AWS a Microsoft Defender for Cloud](quickstart-onboard-aws.md).

::: zone-end

::: zone pivot="azure-portal"

## <a name="add-non-azure-machines-from-the-azure-portal"></a>Adición de máquinas que no son de Azure desde Azure Portal

1. En el menú de Defender for Cloud, abra la página **Introducción**.
1. Seleccione la pestaña **Introducción**.
1. En **Servidores que no son de Azure**, seleccione **Configurar**.

    :::image type="content" source="./media/quickstart-onboard-machines/onboarding-get-started-tab.png" alt-text="Pestaña Introducción de la página de introducción." lightbox="./media/quickstart-onboard-machines/onboarding-get-started-tab.png":::

    > [!TIP]
    > También puede abrir el menú para agregar máquinas en el botón **Agregar servidores que no sean de Azure** de la página **Inventario**.
    > 
    > :::image type="content" source="./media/quickstart-onboard-machines/onboard-inventory.png" alt-text="Incorporación de máquinas que no son de Azure desde la página de inventario de recursos.":::

    Aparecerá una lista de las áreas de trabajo de Log Analytics. La lista incluye, si procede, el área de trabajo predeterminada que Defender for Cloud crea automáticamente si el aprovisionamiento automático está habilitado. Seleccione esta área de trabajo u otra que desee usar.

    Puede agregar equipos a un área de trabajo existente o crear un área de trabajo.

1. Opcionalmente, para crear un área de trabajo, seleccione **Crear área de trabajo nueva**.

1. En la lista de áreas de trabajo, seleccione **Agregar servidores** para el área de trabajo correspondiente.
    Se mostrará la página **Administración de agentes**.

    Aquí, elija el procedimiento correspondiente que se indica a continuación en función del tipo de máquinas en las que se va a incorporar:

    - [Incorporación de máquinas virtuales de Azure Stack Hub](#onboard-your-azure-stack-hub-vms)
    - [Incorporación de máquinas Linux](#onboard-your-linux-machines)
    - [Incorporación de máquinas Windows](#onboard-your-windows-machines)

### <a name="onboard-your-azure-stack-hub-vms"></a>Incorporación de máquinas virtuales de Azure Stack Hub

Para agregar máquinas virtuales de Azure Stack Hub, se necesita no solo la información de la página **Administración de agentes**, sino también configurar la extensión de máquina virtual **Azure Monitor, Update and Configuration Management** en las máquinas virtuales que se ejecutan en la instancia de Azure Stack Hub.

1. En la página **Administración de agentes**, copie los valores de **Id. del área de trabajo** y **Clave principal** en el Bloc de notas.
1. Inicie sesión en el portal de **Azure Stack Hub** y abra la página **Máquinas virtuales**.
1. Seleccione la máquina virtual que desea proteger con Defender for Cloud.
    >[!TIP]
    > Para obtener información acerca de cómo crear una máquina virtual en Azure Stack Hub, consulte [este inicio rápido para máquinas virtuales Windows](/azure-stack/user/azure-stack-quick-windows-portal) o [este inicio rápido para máquinas virtuales Linux](/azure-stack/user/azure-stack-quick-linux-portal).
1. Seleccione **Extensiones**. Se muestra la lista de extensiones de máquina virtual instaladas en esta máquina virtual.
1. Seleccione la pestaña **Agregar**. En el menú **Nuevo recurso** se muestra la lista de extensiones de máquina virtual disponibles.
1. Seleccione la extensión **Azure Monitor, Update and Configuration Management** y seleccione **Crear**. Se abre la página de configuración **Instalar extensión**.
    >[!NOTE]
    > Si no ve la extensión **Azure Monitor, Update and Configuration Management** en Marketplace, póngase en contacto con su operador de Azure Stack Hub para que se la proporcione.
1. En la página de configuración **Instalar extensión**, pegue el **Id. del área de trabajo** y la **Clave del área de trabajo (clave principal)** que copió en el Bloc de notas en el paso anterior.
1. Una vez completada la configuración, seleccione **Aceptar**. El estado de la extensión se mostrará como **Aprovisionamiento realizado correctamente**. La máquina virtual puede tardar hasta una hora en aparecer en Defender for Cloud.

### <a name="onboard-your-linux-machines"></a>Incorporación de máquinas Linux

Para agregar máquinas Linux, necesita el comando WGET de la página **Administración de agentes**.

1. Desde la página **Administración de agentes**, copie el comando **WGET** en el Bloc de notas. Guarde este archivo en una ubicación a la que se pueda acceder desde el equipo Linux.
1. En el equipo Linux, abra el archivo con el comando WGET. Seleccione todo el contenido, cópielo y péguelo en una consola del terminal.
1. Una vez finalizada la instalación, puede validar que *omsagent* está instalado mediante la ejecución del comando *pgrep*. El comando devolverá el PID de *omsagent*.
    Los registros del agente se pueden encontrar en: */var/opt/microsoft/omsagent/\<workspace id>/log/* . La nueva máquina Linux puede tardar hasta 30 minutos en aparecer en Defender for Cloud.

### <a name="onboard-your-windows-machines"></a>Incorporación de máquinas Windows

Para agregar máquinas Windows, necesita la información de la página **Administración de agentes** y descargar el archivo de agente adecuado (32/64 bits).
1. Seleccione el vínculo **Descargar Agente para Windows** correspondiente a su tipo de procesador del equipo para descargar el archivo del programa de instalación.
1. En la página **Administración de agentes**, copie los valores de **Id. del área de trabajo** y **Clave principal** en el Bloc de notas.
1. Copie el archivo de instalación descargado en el equipo de destino y ejecútelo.
1. Siga el Asistente para instalación (**Siguiente**, **Acepto**, **Siguiente**, **Siguiente**).
    1. En la página **Azure Log Analytics**, pegue el **identificador del área de trabajo** y la **clave del área de trabajo (clave principal)** que copió en el Bloc de notas.
    1. Si el equipo tiene que notificar a un área de trabajo de Log Analytics en Azure Government Cloud, seleccione **Azure Gobierno de EE.UU.** en la lista desplegable **Azure Cloud**.
    1. Si el equipo necesita comunicarse a través de un servidor proxy con el servicio de Log Analytics, seleccione **Avanzado** y proporcione la dirección URL y el número de puerto del servidor proxy.
    1. Una establecida toda la configuración, seleccione **Siguiente**.
    1. En la página **Listo para instalar**, revise la configuración que se va a aplicar y seleccione **Instalar**.
    1. En la página **Configuración completada correctamente**, seleccione **Finalizar**.

Una vez completado el proceso, el **Agente de administración de Microsoft** aparece en el **Panel de control**. Puede revisar ahí la configuración y verificar que el agente esté conectado.

Para más información sobre cómo instalar y configurar el agente, vea [Conexión de máquinas Windows](../azure-monitor/agents/agent-windows.md#install-agent-using-setup-wizard).

::: zone-end

## <a name="verifying"></a>Comprobando

¡Enhorabuena! Ahora puede ver las máquinas de Azure y las que no son de Azure en un solo lugar. Abra la [página del inventario de recursos](asset-inventory.md) y filtre por los tipos de recursos correspondientes. Estos dos iconos distinguen los tipos:

  ![Icono de ASC para una máquina que no es de Azure.](./media/quickstart-onboard-machines/security-center-monitoring-icon1.png) Máquina que no es de Azure

  ![Icono de ASC para una máquina de Azure.](./media/quickstart-onboard-machines/security-center-monitoring-icon2.png) Azure VM

  ![Icono de ASC para un servidor de Azure Arc.](./media/quickstart-onboard-machines/arc-enabled-machine-icon.png) Servidor habilitado para Azure Arc

## <a name="next-steps"></a>Pasos siguientes

En esta página se ha mostrado cómo agregar las máquinas que no son de Azure a Microsoft Defender for Cloud. Para supervisar su estado, utilice las herramientas de inventario, como se explica en la siguiente página:

- [Exploración y administración de los recursos con Asset Inventory](asset-inventory.md)
