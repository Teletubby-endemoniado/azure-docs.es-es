---
title: Cifrado de discos de sistema operativo mediante claves administradas por el cliente
description: Aprenda a cifrar discos de sistema operativo (SO) mediante claves administradas por el cliente en Azure DevTest Labs.
ms.topic: how-to
ms.date: 09/01/2020
ms.openlocfilehash: 2da0e89c5a3f34d9ba967088ea3881f6a30bc1e2
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128632355"
---
# <a name="encrypt-operating-system-os-disks-using-customer-managed-keys-in-azure-devtest-labs"></a>Cifrado de discos de sistema operativo (SO) mediante claves administradas por el cliente en Azure DevTest Labs
El cifrado del lado servidor (SSE) protege los datos y le ayuda a cumplir los compromisos de cumplimiento y seguridad de la organización. SSE cifra automáticamente los datos almacenados en discos administrados de Azure (discos de datos y de SO) en reposo de forma predeterminada cuando los conserva en la nube. Obtenga más información acerca del [cifrado de disco](../virtual-machines/disk-encryption.md) en Azure. 

En DevTest Labs, todos los discos de sistema operativo y de datos que se hayan creado como parte de un laboratorio, se cifran mediante claves administradas por la plataforma. De todas formas, como propietario de un laboratorio, puede optar por cifrar los discos de sistema operativo de la máquina virtual del laboratorio con sus propias claves. Si opta por administrar el cifrado con sus propias claves, puede especificar una **clave administrada por el cliente** que se usará para cifrar los datos de los discos de sistema operativo. Para más información sobre el cifrado del lado servidor (SSE) con claves administradas por el cliente y otros tipos de cifrado de disco administrado, consulte [Claves administradas por el cliente](../virtual-machines/disk-encryption.md#customer-managed-keys). Consulte también las [restricciones en el uso de claves administradas por el cliente](../virtual-machines/disks-enable-customer-managed-keys-portal.md#restrictions).

> [!NOTE]
> - Actualmente, el cifrado de disco con una clave administrada por el cliente solo se admite para los discos de sistema operativo de DevTest Labs. 
> 
> - La configuración se aplica a los discos de sistema operativo recién creados en el laboratorio. Si decide cambiar el conjunto de cifrado de disco en algún momento, los discos más antiguos del laboratorio seguirán estando cifrados con el conjunto de cifrado de disco anterior. 

En la sección siguiente se muestra cómo un propietario del laboratorio puede configurar el cifrado mediante una clave administrada por el cliente.

## <a name="pre-requisites"></a>Requisitos previos

1. Si no tiene un conjunto de cifrado de disco, siga este artículo para [configurar una instancia de Key Vault y un conjunto de cifrado de disco](../virtual-machines/disks-enable-customer-managed-keys-portal.md). Tenga en cuenta los siguientes requisitos para el conjunto de cifrado de disco: 

    - El conjunto de cifrado de disco tiene que estar **en la misma región y suscripción que el laboratorio**. 
    - Asegúrese de que el propietario del laboratorio tenga al menos un **acceso de nivel de lector** al conjunto de cifrado de disco que se usará para cifrar los discos de sistema operativo del laboratorio. 
1. En el caso de los laboratorios creados antes del 1 de agosto de 2020, el propietario del laboratorio deberá asegurarse de que la identidad asignada por el sistema del laboratorio está habilitada. Para ello, el propietario del laboratorio puede ir a su laboratorio, hacer clic en **Configuración y directivas**, hacer clic en la hoja **Identidad (versión preliminar)** , cambiar el **estado** de la identidad asignada por el sistema a **Activado** y hacer clic en **Guardar**. En los nuevos laboratorios creados después del 1 de agosto de 2020, la identidad asignada por el sistema del laboratorio se habilitará de forma predeterminada. 

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/encrypt-disks-customer-managed-keys/managed-keys.png" alt-text="Claves administradas":::
1. Para que el laboratorio administre el cifrado de todos los discos de sistema operativo del laboratorio, el propietario del laboratorio debe conceder explícitamente el rol de lector de **identidad asignada por el sistema** en el conjunto de cifrado de disco, así como el rol de colaborador de la máquina virtual en la suscripción de Azure subyacente. Para hacerlo, el propietario del laboratorio puede seguir estos pasos:

   
    1. Asegúrese de que es miembro de un [rol de administrador de acceso de usuario](../role-based-access-control/built-in-roles.md#user-access-administrator) en el nivel de suscripción de Azure con el fin de poder administrar el acceso de los usuarios a los recursos de Azure. 
    1. En la página **Conjunto de cifrado de disco**, seleccione **Control de acceso (IAM)** en el menú de la izquierda. 
    1. Seleccione **+ Agregar** en la barra de herramientas y, después, **Agregar una asignación de roles**.  

        :::image type="content" source="./media/encrypt-disks-customer-managed-keys/add-role-management-menu.png" alt-text="Agregar administración de roles: menú":::
    1. En la página **Agregar asignación de roles**, seleccione el rol **Lector** u otro que permita más acceso. 
    1. Escriba el nombre del laboratorio para el que se usará el conjunto de cifrado de disco y seleccione el nombre del laboratorio (identidad asignada por el sistema para el laboratorio) en la lista desplegable. 
    
        :::image type="content" source="./media/encrypt-disks-customer-managed-keys/select-lab.png" alt-text="Seleccionar la identidad administrada por el sistema del laboratorio":::        
    1. Seleccione **Guardar** en la barra de herramientas. 

        :::image type="content" source="./media/encrypt-disks-customer-managed-keys/save-role-assignment.png" alt-text="Guardar asignación de roles":::
3. Agregue la **identidad asignada por el sistema** del laboratorio al rol **Colaborador de la máquina virtual** mediante la página **Suscripción** -> **Control de acceso (IAM)** . Los pasos son similares a los anteriores. 

    
    1. Vaya a la página **Suscripción** de Azure Portal. 
    1. Seleccione **Access Control (IAM)** . 
    1. Seleccione **+ Agregar** en la barra de herramientas y seleccione **Agregar una asignación de roles**. 
    
        :::image type="content" source="./media/encrypt-disks-customer-managed-keys/subscription-access-control-page.png" alt-text="Suscripción -> Página Control de acceso (IAM)":::
    1. En la página **Agregar asignación de roles**, seleccione **Colaborador de la máquina virtual** para el rol.
    1. Escriba el nombre del laboratorio y seleccione el **nombre del laboratorio** (identidad asignada por el sistema para el laboratorio) en la lista desplegable. 
    1. Seleccione **Guardar** en la barra de herramientas. 

## <a name="encrypt-lab-os-disks-with-a-customer-managed-key"></a>Cifrado de discos del sistema operativo del laboratorio con una clave administrada por el cliente 

1. En la página principal del laboratorio en Azure Portal, seleccione **Configuración y directivas** en el menú de la izquierda. 
1. En la página **Configuración y directivas**, seleccione **Discos (versión preliminar)** en la sección **Cifrado**. De forma predeterminada, **Tipo de cifrado** se establece en **Cifrado en reposo con una clave administrada por la plataforma**.

    :::image type="content" source="./media/encrypt-disks-customer-managed-keys/disks-page.png" alt-text="Pestaña Discos de la página Configuración y directivas":::
1. En la pestaña **Tipo de cifrado**, seleccione **Cifrado en reposo con una clave administrada por el cliente** en la lista desplegable. 
1. Para **Conjunto de cifrado de disco**, seleccione el conjunto de cifrado de disco que creó anteriormente. Es el mismo conjunto de cifrado de disco al que puede tener acceso la identidad asignada por el sistema del laboratorio.
1. Seleccione **Guardar** en la barra de herramientas. 

    :::image type="content" source="./media/encrypt-disks-customer-managed-keys/disk-encryption-set.png" alt-text="Habilitación del cifrado con claves administradas por el cliente":::
1. En el cuadro de mensaje con el siguiente texto: *This setting will apply to newly created machines in the lab. Old OS disk will remain encrypted with the old disk encryption set* (Esta configuración se aplicará a las máquinas recién creadas en el laboratorio. El disco del sistema operativo antiguo permanecerá cifrado con el conjunto de cifrado de disco antiguo), seleccione **Aceptar**. 

    Una vez configurados, los discos del sistema operativo del laboratorio se cifrarán con la clave administrada por el cliente proporcionada mediante el conjunto de cifrado de disco. 
   
## <a name="how-to-validate-if-disks-are-being-encrypted"></a>Validación de si se están cifrando los discos

1. Vaya a una máquina virtual de laboratorio creada después de habilitar el cifrado de discos con una clave administrada por el cliente en el laboratorio.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/encrypt-disks-customer-managed-keys/enabled-encryption-vm.png" alt-text="Máquina virtual con cifrado de disco habilitado":::
1. Haga clic en el grupo de recursos de la máquina virtual y haga clic en el disco del sistema operativo.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/encrypt-disks-customer-managed-keys/vm-resource-group.png" alt-text="Grupo de recursos de máquina virtual":::
1. Vaya a Cifrado y valide si el cifrado está establecido en clave administrada por el cliente con el conjunto de cifrado de disco seleccionado.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/encrypt-disks-customer-managed-keys/validate-encryption.png" alt-text="Validación del cifrado":::
  
## <a name="next-steps"></a>Pasos siguientes

Vea los artículos siguientes: 

- [Azure Disk Encryption](../virtual-machines/disk-encryption.md). 
- [Claves administradas por el cliente](../virtual-machines/disk-encryption.md#customer-managed-keys)