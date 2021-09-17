---
title: 'Tutorial: Acceso a la nube privada'
description: Aprenda a acceder a una nube privada de Azure VMware Solution.
ms.topic: tutorial
ms.date: 08/13/2021
ms.openlocfilehash: 6b4798bf5257be82475986c040b04b63ff5a483b
ms.sourcegitcommit: e7d500f8cef40ab3409736acd0893cad02e24fc0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2021
ms.locfileid: "122070795"
---
# <a name="tutorial-access-an-azure-vmware-solution-private-cloud"></a>Tutorial: Acceso a una nube privada de Azure VMware Solution

Azure VMware Solution no permite administrar la nube privada con la instancia local de vCenter. En lugar de eso, tendrá que conectarse a la instancia de vCenter de Azure VMware Solution a través de un jumpbox. 

En este tutorial, creará un jumpbox en el grupo de recursos que creó en el [tutorial anterior](tutorial-configure-networking.md) e iniciará sesión en vCenter de Azure VMware Solution. Este jumpbox es una máquina virtual (VM) de Windows que se encuentra en la misma red virtual que creó.  Proporciona acceso tanto a vCenter como a NSX Manager. 

En este tutorial aprenderá a:

> [!div class="checklist"]
> * Crear una VM Windows para acceder a vCenter de Azure VMware Solution
> * Iniciar sesión en vCenter desde esta VM

## <a name="create-a-new-windows-virtual-machine"></a>Creación de una nueva máquina virtual Windows.

1. En el grupo de recursos, seleccione **Agregar**, busque **Microsoft Windows 10**, y selecciónelo. Seleccione **Crear**.

   :::image type="content" source="media/tutorial-access-private-cloud/ss8-azure-w10vm-create.png" alt-text="Captura de pantalla que muestra cómo agregar una nueva máquina virtual Windows 10 para un jumpbox.":::

1. Escriba la información requerida en los campos y seleccione **Revisar y crear**. 

   Para más información sobre los campos, consulte la tabla siguiente.

   | Campo | Value |
   | --- | --- |
   | **Suscripción** | El valor se rellena previamente con la suscripción a la que pertenece el grupo de recursos. |
   | **Grupos de recursos** | El valor se rellena previamente para el grupo de recursos actual que creó en el tutorial anterior.  |
   | **Nombre de la máquina virtual** | Introduzca un nombre único para la máquina virtual. |
   | **Región** | Seleccione la ubicación geográfica de la máquina virtual. |
   | **Opciones de disponibilidad** | Deje el valor predeterminado seleccionado. |
   | **Imagen** | Seleccione la imagen de la máquina virtual. |
   | **Tamaño** | Deje el valor de tamaño predeterminado. |
   | **Tipo de autenticación**  | Seleccione **Contraseña**. |
   | **Nombre de usuario** | Escriba el nombre de usuario para iniciar sesión en la máquina virtual. |
   | **Contraseña** | Escriba la contraseña para iniciar sesión en la máquina virtual. |
   | **Confirmar contraseña** | Escriba la contraseña para iniciar sesión en la máquina virtual. |
   | **Puertos de entrada públicos** | Seleccione **Ninguno**. <ul><li>Puede usar el [acceso JIT](../security-center/security-center-just-in-time.md#jit-configure) para controlar el acceso a la VM solo cuando quiera acceder a ella.</li><li>Para acceder de forma segura al servidor jumpbox desde Internet sin exponer ningún puerto de red, use una instancia de [Azure Bastion](../bastion/tutorial-create-host-portal.md).</li></ul>  |


1. Una vez superada la validación, seleccione **Crear** para iniciar el proceso de creación de la máquina virtual.

## <a name="connect-to-the-local-vcenter-of-your-private-cloud"></a>Conexión a la instancia local de vCenter de la nube privada.

1. En el jumpbox, inicie sesión en el cliente de vSphere con el inicio de sesión único de VMware vCenter con un nombre de usuario de administrador de la nube y compruebe que la interfaz de usuario se muestra correctamente.

1. En Azure Portal, seleccione su nube privada y haga clic en **Administrar** > **Identidad**. 

   Se muestran las direcciones URL y las credenciales de usuario para vCenter y NSX-T Manager de la nube privada.

   :::image type="content" source="media/tutorial-access-private-cloud/ss4-display-identity.png" alt-text="Captura de pantalla que muestra las direcciones URL y las credenciales de NSX Manager y vCenter de la nube privada." lightbox="media/tutorial-access-private-cloud/ss4-display-identity.png":::

1. Vaya a la máquina virtual que creó en el paso anterior y conéctese a ella. 

   Si necesita ayuda para conectarse a la máquina virtual, consulte [Conexión a una máquina virtual](../virtual-machines/windows/connect-logon.md#connect-to-the-virtual-machine) para los detalles.

1. En la VM Windows, abra un explorador y vaya a las direcciones URL del administrador de NSX-T y vCenter en dos pestañas. 

1. En la pestaña de vCenter, escriba las credenciales del usuario `cloudadmin@vsphere.local` del paso anterior.

   :::image type="content" source="media/tutorial-access-private-cloud/ss5-vcenter-login.png" alt-text="Captura de pantalla que la página de inicio de sesión de VMware vSphere." border="true":::

   :::image type="content" source="media/tutorial-access-private-cloud/ss6-vsphere-client-home.png" alt-text="Captura de pantalla que muestra un resumen del clúster 1 en el cliente de vSphere." border="true":::

1. En la segunda pestaña del explorador, inicie sesión en el administrador de NSX-T.

   :::image type="content" source="media/tutorial-access-private-cloud/ss10-nsx-manager-home.png" alt-text="Captura de pantalla de la página de información general de NSX-T Manager" border="true":::.



## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Crear una VM Windows para conectarse a vCenter
> * Iniciar sesión en vCenter desde la VM

Continúe con el siguiente tutorial para aprender a crear una red virtual a fin de configurar la administración local de los clústeres de nube privada.

> [!div class="nextstepaction"]
> [Creación de una red virtual](tutorial-configure-networking.md)

