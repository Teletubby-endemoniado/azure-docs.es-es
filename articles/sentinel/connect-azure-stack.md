---
title: Incorporación de máquinas virtuales de Azure Stack Hub a Microsoft Sentinel | Microsoft Docs
description: En este artículo se muestra cómo aprovisionar la extensión de máquina virtual de Azure Monitor, Update and Configuration Management en máquinas virtuales de Azure Stack Hub y cómo empezar a supervisarlas con Microsoft Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2021
ms.author: yelevin
ms.custom: ignite-fall-2021
ms.openlocfilehash: 3f8367a723cb11ab1c6260bc323240d08181dddc
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132715214"
---
# <a name="connect-azure-stack-hub-virtual-machines-to-microsoft-sentinel"></a>Conexión de máquinas virtuales de Azure Stack Hub a Microsoft Sentinel

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

Con Microsoft Sentinel, puede supervisar las máquinas virtuales que se ejecutan en Azure y Azure Stack Hub en un solo lugar. Para incorporar sus máquinas de Azure Stack a Microsoft Sentinel, primero debe agregar la extensión de máquina virtual a las máquinas virtuales de Azure Stack Hub existentes. 

Después de conectar las máquinas de Azure Stack Hub, elija de una galería de paneles que exponen información basada en los datos. Estos paneles se pueden personalizar fácilmente en función de sus necesidades.

## <a name="add-the-virtual-machine-extension"></a>Adición de la extensión de máquina virtual 

Agregue la extensión de máquina virtual **Azure Monitor, Update and Configuration Management** a las máquinas virtuales que se ejecutan en Azure Stack Hub. 

1. En una nueva pestaña del explorador, inicie sesión en el portal de [Azure Stack Hub](/azure-stack/user/azure-stack-use-portal#access-the-portal).

1. Vaya a la página **Máquinas virtuales** y seleccione la máquina virtual que desea proteger con Microsoft Sentinel. Para obtener información sobre cómo crear una máquina virtual en Azure Stack Hub, consulte [Creación de una máquina virtual Windows Server con el portal de Azure Stack Hub](/azure-stack/user/azure-stack-quick-windows-portal) o [Creación de una VM de servidor Linux mediante el portal de Azure Stack Hub](/azure-stack/user/azure-stack-quick-linux-portal).

1. Seleccione **Extensiones**. Se muestra la lista de extensiones de máquina virtual instaladas en esta máquina virtual.

1. Seleccione la pestaña **Agregar**. Se abre la hoja del menú **Nuevo recurso**, que muestra la lista de extensiones de máquina virtual disponibles. 

1. Seleccione la extensión **Azure Monitor, Update and Configuration Management** y luego **Crear**. Se abre la ventana de configuración **Instalar extensión**.

   ![Configuración de Azure Monitor, Update, and Configuration Management](./media/connect-azure-stack/azure-monitor-extension-fix.png)  

   >[!NOTE]
   > Si no ve la extensión **Azure Monitor, Update and Configuration Management** en Marketplace, póngase en contacto con su operador de Azure Stack Hub para que se la proporcione.

1. En el menú de Microsoft Sentinel, seleccione **Configuración del área de trabajo** y, después, **Avanzado** y copie el **Id. del área de trabajo** y la **Clave de área de trabajo (clave principal)** . 

1. En la ventana **Instalar extensión** de Azure Stack Hub, péguelos en los campos indicados y seleccione **Aceptar**.

1. Una vez que se complete la instalación de la extensión, su estado se mostrará como **Aprovisionamiento realizado correctamente**. La máquina virtual puede tardar hasta una hora en aparecer en el portal de Microsoft Sentinel.

Para más información sobre cómo instalar y configurar el agente para Windows, vea [Conexión de equipos Windows](../azure-monitor/agents/agent-windows.md#install-agent-using-setup-wizard).

Para solucionar problemas en Linux del agente, consulte [Solución de problemas relacionados con el agente Azure Log Analytics para Linux](../azure-monitor/agents/agent-linux-troubleshoot.md).

En el portal de Microsoft Sentinel en Azure, en **Máquinas virtuales**, dispone de información general de todas las máquinas virtuales y los equipos, junto con sus estados. 

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando deje de ser necesaria, la extensión se puede eliminar de la máquina virtual desde el portal de Azure Stack Hub.

Para eliminar la extensión:

1. Abra el **portal de Azure Stack Hub**.

1. Vaya a la página **Máquinas virtuales** y seleccione la máquina virtual de la que desea quitar la extensión.

1. Seleccione **Extensions** (Extensiones) y, después, la extensión **Microsoft.EnterpriseCloud.Monitoring**.

1. Seleccione **Desinstalar** y confirme la selección.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre Microsoft Sentinel, consulte los siguientes artículos:

- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](get-visibility.md).
- Empiece a [detectar amenazas con Microsoft Sentinel](detect-threats-built-in.md).
- Transmita datos desde [dispositivos de formato de evento común](connect-common-event-format.md) a Microsoft Sentinel.
