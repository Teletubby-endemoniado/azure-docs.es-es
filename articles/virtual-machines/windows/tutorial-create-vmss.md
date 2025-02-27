---
title: 'Tutorial: Creación de un conjunto de escalado de máquinas virtuales Windows'
description: Aprenda a crear e implementar una aplicación de alta disponibilidad en máquinas virtuales Windows con un conjunto de escalado de máquinas virtuales.
author: ju-shim
ms.author: jushiman
ms.topic: tutorial
ms.service: virtual-machines
ms.collection: windows
ms.date: 10/15/2021
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: 217e87d654d88109e4d8ab8a0fadab612f5a6aed
ms.sourcegitcommit: 01dcf169b71589228d615e3cb49ae284e3e058cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2021
ms.locfileid: "130167437"
---
# <a name="tutorial-create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a>Tutorial: Creación de un conjunto de escalado de máquinas virtuales e implementación de una aplicación de alta disponibilidad en Windows
**Se aplica a:** :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado flexibles

Los conjuntos de escalado de máquinas virtuales con [orquestación flexible](../flexible-virtual-machine-scale-sets.md) permiten crear y administrar un grupo de máquinas virtuales con equilibrio de carga. El número de instancias de máquina virtual puede aumentar o disminuir automáticamente según la demanda, o de acuerdo a una programación definida.

En este tutorial, implementará un conjunto de escalado de máquinas virtuales en Azure y aprenderá cómo:

> [!div class="checklist"]
> * Cree un grupo de recursos.
> * Cree un conjunto de escalado flexible con un equilibrador de carga.
> * Agregue IIS a las instancias del conjunto de escalado mediante **Ejecutar comando**.
> * Abra el puerto 80 al tráfico HTTP.
> * Pruebe un conjunto de escalado.


## <a name="scale-set-overview"></a>Introducción al conjunto de escalado

Los conjuntos de escalado ofrecen las siguientes ventajas principales:
- Facilitan la creación y administración de varias máquinas virtuales
- Proporciona alta disponibilidad y resistencia de aplicaciones mediante la distribución de máquinas virtuales entre dominios de error.
- Permiten a la aplicación escalar automáticamente a medida que cambia la demanda de recursos
- Funciona a gran escala

Con la orquestación flexible, Azure proporciona una experiencia unificada en todo el ecosistema de máquinas virtuales de Azure. La orquestación flexible ofrece garantías de alta disponibilidad (hasta mil máquinas virtuales) mediante la propagación de máquinas virtuales entre dominios de error en una región o en una zona de disponibilidad, lo que permite escalar horizontalmente la aplicación a la vez que se mantiene el aislamiento del dominio de error, algo que es esencial para ejecutar cargas de trabajo basadas en cuórum o con estado, entre las que se incluyen:
- Cargas de trabajo basadas en cuórum
- Bases de datos de código abierto
- Aplicaciones con estado
- Servicios que requieren alta disponibilidad y gran escala
- Servicios que desean combinar tipos de máquina virtual o que aprovechan máquinas virtuales de acceso puntual y a petición conjuntamente
- Aplicaciones del conjunto de disponibilidad existentes

Obtenga más información sobre las diferencias entre conjuntos de escalado uniformes y conjuntos de escalado flexibles en [modos de orquestación](../../virtual-machine-scale-sets/virtual-machine-scale-sets-orchestration-modes.md).



## <a name="create-a-scale-set"></a>Creación de un conjunto de escalado

Use Azure Portal para crear un conjunto de escalado flexible.

1. Abra [Azure Portal](https://portal.azure.com).
1. Busque y seleccione **Conjuntos de escalado de máquinas virtuales**.
1. Seleccione **Crear** en la página **Conjuntos de escalado de máquinas virtuales**. Se abre la página **Crear un conjunto de escalado de máquinas virtuales**.
1. En **Suscripción**, seleccione la suscripción que quiera usar.
1. En **Grupo de recursos**, seleccione **Crear nuevo** y escriba *myVMSSRG* como nombre y seleccione **Aceptar**.
    :::image type="content" source="media/tutorial-create-vmss/flex-project-details.png" alt-text="Detalles del proyecto.":::
1. En **Nombre del conjunto de escalado de máquinas virtuales**, escriba *myVMSS*.
1. En **Región**, seleccione una región cercana a su área como, por ejemplo, *Este de EE. UU.*
    :::image type="content" source="media/tutorial-create-vmss/flex-details.png" alt-text="Nombre y región.":::
1. Deje **Zona de disponibilidad** en blanco para este ejemplo.
1. En **Orchestration mode** (Modo de orquestación), seleccione **Flexible**.
1. Deje el valor predeterminado *1* para el recuento de dominios de error o elija otro valor en la lista desplegable.
   :::image type="content" source="media/tutorial-create-vmss/flex-orchestration.png" alt-text="Seleccione el modo de orquestación flexible.":::
1. En **Imagen**, seleccione *Windows Server 2019 Datacenter - Gen1*.
1. En **Tamaño**, deje el valor predeterminado o seleccione un tamaño como *Standard_E2s_V3*.
1. En **Nombre de usuario**, escriba el nombre que desea utilizar para la cuenta de administrador como, por ejemplo *azureuser*.
1. En **Contraseña** y en **Confirmar contraseña**, escriba una contraseña segura para la cuenta de administrador.
1. En la pestaña **Redes**, en **Equilibrio de carga**, seleccione **Usar un equilibrador de carga**.
1. En **Opciones de equilibrio de carga**, deje el valor predeterminado: **Azure Load Balancer**.
1. En **Seleccionar un equilibrador de carga**, seleccione **Crear nuevo**. 
    :::image type="content" source="media/tutorial-create-vmss/load-balancer-settings.png" alt-text="Configuración de equilibrador de carga.":::
1. En la página **Crear un equilibrador de carga**, escriba un nombre para el equilibrador de carga y un **Nombre de dirección IP pública**.
1. En **Etiqueta de nombre de dominio**, escriba el nombre que desea utilizar como prefijo para el nombre de dominio. El nombre debe ser único.
1. Cuando termine, seleccione **Crear**.
    :::image type="content" source="media/tutorial-create-vmss/flex-load-balancer.png" alt-text="Cree un equilibrador de carga.":::
1. De vuelta en la pestaña **Redes**, deje el nombre predeterminado para el grupo de back-end.
1. En la pestaña **Escalado**, deje el valor del recuento predeterminado de instancias en *2* o agregue su propio valor. Este es el número de máquinas virtuales que se crearán, por lo que debe tener en cuenta los costos y los límites de la suscripción si cambia este valor.
1. Deje la opción **Directiva de escalado** establecida en *Manual*.
    :::image type="content" source="media/tutorial-create-vmss/flex-scaling.png" alt-text="Configuración de la directiva de escalado.":::
1. Cuando haya terminado, seleccione **Revisar y crear**.
1. Una vez que compruebe que la validación ha finalizado, puede seleccionar **Crear** en la parte inferior de la página para implementar el conjunto de escalado.
1. Una vez que la implementación finalice, seleccione **Ir al recurso** para abrir el nuevo conjunto de escalado.

## <a name="view-the-vms-in-your-scale-set"></a>Visualización de las máquinas virtuales en el conjunto de escalado

En la página del conjunto de escalado, seleccione **Instancias** en el menú izquierdo. 

Verá una lista de máquinas virtuales que forman parte del conjunto de escalado. La lista incluye lo siguiente:

- Nombre de la máquina virtual
- Nombre de equipo usado por la máquina virtual.
- Estado actual de la máquina virtual, por ejemplo *En ejecución*.
- *Estado de aprovisionamiento* de la máquina virtual, por ejemplo *Correcto*.

:::image type="content" source="media/tutorial-create-vmss/instances.png" alt-text="Tabla de información sobre las instancias del conjunto de escalado.":::

## <a name="enable-iis-using-runcommand"></a>Habilitar IIS mediante RunCommand

Para probar el conjunto de escalado, podemos habilitar IIS en cada una de las máquinas virtuales mediante la opción [Ejecutar comando](../windows/run-command.md).

1. Seleccione la primera máquina virtual de la lista de **Instancias**.
1. En el menú izquierdo, en **Operaciones**, seleccione **Ejecutar comando**. Se abrirá la página **Ejecutar comando**.
1. Seleccione **RunPowerShellScript** en la lista de comandos. Se abrirá la página **Ejecutar script de comando**.
1. En **Script de PowerShell**, pegue el siguiente fragmento de código:

    ```powershell
    Add-WindowsFeature Web-Server
    Set-Content -Path "C:\inetpub\wwwroot\Default.htm" -Value "Hello world from host $($env:computername) !"
    ```
1. Cuando haya terminado, seleccione **Ejecutar**. Verá el progreso en la ventana **Salida**.
1. Una vez completado el script en la primera máquina virtual, puede seleccionar **X** en la esquina superior derecha para cerrar la página.
1. Vuelva a la lista de instancias de conjuntos de escalado y use **Ejecutar comando** en cada máquina virtual del conjunto de escalado.

## <a name="open-port-80"></a>Apertura del puerto 80 

Para abrir el puerto 80 en el conjunto de escalado, agregue una regla de entrada al grupo de seguridad de red.

1. En la página del conjunto de escalado, seleccione **Redes** en el menú izquierdo. Se abrirá la página **Redes**.
1. Seleccione **Agregar regla de puerto de entrada**. Se abrirá la página **Agregar regla de seguridad de entrada**.
1. En **Servicio**, seleccione *HTTP* y, después, **Agregar** en la parte inferior de la página.

## <a name="test-your-scale-set"></a>Prueba del conjunto de escalado

Pruebe el conjunto de escalado conectándose a él desde un explorador.

1. En la página **Información general** del conjunto de escalado, copie la dirección IP pública.
1. Abra otra pestaña en el explorador web y pegue la dirección IP en la barra de direcciones.
1. Cuando se cargue la página, tome nota del nombre de proceso que se muestra. 
1. Actualice la página hasta que vea que cambia el nombre del equipo. 

## <a name="delete-your-scale-set"></a>Eliminación del conjunto de escalado

Cuando haya terminado, debe eliminar el grupo de recursos, con lo que se eliminará todo lo que implementó para el conjunto de escalado.

1. En la página del conjunto de escalado, seleccione **Grupo de recursos**. Se abrirá la página del grupo de recursos.
1. En la parte superior de la página, seleccione **Eliminar grupo de recursos**.
1. En la página **¿Seguro que desea eliminar?** , escriba el nombre del grupo de recursos y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha creado un conjunto de escalado de máquinas virtuales. Ha aprendido a:

> [!div class="checklist"]
> * Cree un grupo de recursos.
> * Cree un conjunto de escalado flexible con un equilibrador de carga.
> * Agregue IIS a las instancias del conjunto de escalado mediante **Ejecutar comando**.
> * Abra el puerto 80 al tráfico HTTP.
> * Pruebe un conjunto de escalado.

Pase al siguiente tutorial para obtener más información sobre el concepto de equilibrio de carga de las máquinas virtuales.

> [!div class="nextstepaction"]
> [Load balance virtual machines](tutorial-load-balancer.md) (Equilibrio de carga de máquinas virtuales)
