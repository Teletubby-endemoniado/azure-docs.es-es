---
title: Copia de seguridad y restauración de VM de Azure cifradas
description: Se describe cómo realizar una copia de seguridad de máquinas virtuales de Azure cifradas, y cómo restaurarlas, con el servicio Azure Backup.
ms.topic: conceptual
ms.date: 07/27/2021
ms.openlocfilehash: ebd8280b24c0f99474f3847d7549db0da9a27516
ms.sourcegitcommit: 512e6048e9c5a8c9648be6cffe1f3482d6895f24
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2021
ms.locfileid: "132157523"
---
# <a name="back-up-and-restore-encrypted-azure-virtual-machines"></a>Copia de seguridad y restauración de máquinas virtuales de Azure cifradas

En este artículo se describe cómo realizar una copia de seguridad de máquinas virtuales (VM) Windows o Linux de Azure con discos cifrados, y cómo restaurarlas, mediante el servicio [Azure Backup](backup-overview.md). Para más información, consulte [Cifrado de copias de seguridad de VM de Azure](backup-azure-vms-introduction.md#encryption-of-azure-vm-backups).

## <a name="encryption-using-platform-managed-keys"></a>Cifrado mediante claves administradas por la plataforma

De forma predeterminada, todos los discos de las máquinas virtuales se cifran automáticamente en reposo mediante claves administradas por la plataforma (PMK) que usan el [cifrado del servicio de almacenamiento](../storage/common/storage-service-encryption.md). Puede realizar una copia de seguridad de estas máquinas virtuales mediante Azure Backup sin ninguna acción específica necesaria para admitir el cifrado en su extremo. Para más información sobre el cifrado con claves administradas por la plataforma, [consulte este artículo](../virtual-machines/disk-encryption.md#platform-managed-keys).

![Discos cifrados](./media/backup-encryption/encrypted-disks.png)

## <a name="encryption-using-customer-managed-keys"></a>Cifrado con claves administradas por el cliente

Cuando cifre discos con claves administradas por el cliente (CMK), la clave que se utiliza para cifrar los discos se almacena en Azure Key Vault y la administra el usuario. El cifrado de Storage Service Encryption (SSE) mediante CMK difiere del cifrado de Azure Disk Encryption (ADE). ADE utiliza las herramientas de cifrado del sistema operativo. SSE cifra los datos en el servicio de almacenamiento, lo que permite utilizar cualquier sistema operativo o imagen para las máquinas virtuales.

No es necesario realizar ninguna acción explícita para la copia de seguridad o la restauración de máquinas virtuales que utilizan claves administradas por el cliente para cifrar los discos. Los datos de copia de seguridad de estas máquinas virtuales almacenadas en el almacén se cifrarán con los mismos métodos que el [cifrado que se usa en el almacén](encryption-at-rest-with-cmk.md).

Para más información sobre cifrado de discos administrados con claves administradas por el cliente, consulte [este artículo](../virtual-machines/disk-encryption.md#customer-managed-keys).

## <a name="encryption-support-using-ade"></a>Compatibilidad con cifrado mediante ADE

Azure Backup admite la copia de seguridad de máquinas virtuales de Azure que tengan cifrados sus discos del sistema operativo o de datos con Azure Disk Encryption (ADE). ADE usa BitLocker para el cifrado de máquinas virtuales Windows y la característica dm-crypt para máquinas virtuales Linux. ADE se integra con Azure Key Vault para administrar las claves de cifrado y los secretos del disco. Las claves de cifrado de claves (KEK) de Key Vault pueden usarse para agregar una capa adicional de seguridad, de forma que se cifran los secretos de cifrado antes de escribirlos en Key Vault.

Azure Backup puede realizar copias de seguridad de máquinas virtuales de Azure, y restaurarlas, mediante ADE con y sin la aplicación Azure AD, como se resume en la tabla siguiente.

**Tipo de disco de máquina virtual** | **ADE (BEK/dm-crypt)** | **ADE y KEK**
--- | --- | ---
**No administrado** | Sí | Sí
**Administrado**  | Sí | Sí

- Aprenda más sobre [ADE](../security/fundamentals/azure-disk-encryption-vms-vmss.md), [Key Vault](../key-vault/general/overview.md) y [KEK](../virtual-machine-scale-sets/disk-encryption-key-vault.md#set-up-a-key-encryption-key-kek).
- Lea las [preguntas más frecuentes](../security/fundamentals/azure-disk-encryption-vms-vmss.md) sobre el cifrado de discos de máquina virtual de Azure.

### <a name="limitations"></a>Limitaciones

- Puede realizar una copia de seguridad de VM cifradas con ADE, y restaurarlas, en la misma suscripción.
- Azure Backup admite máquinas virtuales cifradas con claves independientes. Actualmente no se admite ninguna clave que forme parte de un certificado usado para cifrar una máquina virtual.
- Azure Backup admite la restauración entre regiones de máquinas virtuales de Azure cifradas en las regiones emparejadas de Azure. Para obtener más información, vea la [matriz de compatibilidad](./backup-support-matrix.md#cross-region-restore).
- Las VM cifradas con ADE no se pueden recuperar a nivel de archivo o carpeta. Deberá recuperar toda la máquina virtual para restaurar archivos y carpetas.
- Al restaurar una VM, no puede usar la opción para [reemplazar la VM existente](backup-azure-arm-restore-vms.md#restore-options) en las VM cifradas con ADE. Esta opción solo se admite con discos administrados sin cifrar.

## <a name="before-you-start"></a>Antes de comenzar

Antes de empezar, haga lo siguiente:

1. Asegúrese de que tiene una o varias máquinas virtuales [Windows](../virtual-machines/linux/disk-encryption-overview.md) o [Linux](../virtual-machines/linux/disk-encryption-overview.md) con ADE habilitado.
2. [Revise la matriz de compatibilidad](backup-support-matrix-iaas.md) para la copia de seguridad de máquinas virtuales de Azure.
3. [Cree](backup-create-rs-vault.md) un almacén de Backup de Recovery Services si aún no tiene uno.
4. Si habilita el cifrado para máquinas virtuales que ya están habilitadas para la copia de seguridad, solo debe proporcionar al servicio Backup permisos para acceder al almacén de claves para que las copias de seguridad puedan continuar sin interrupciones. [Más información](#provide-permissions) sobre la asignación de estos permisos.

Además, hay un par de cosas que puede que deba hacer en algunas circunstancias:

- **Instalar el agente de máquina virtual en la máquina virtual**: Azure Backup realiza una copia de seguridad de máquinas virtuales de Azure instalando una extensión en el agente de máquina virtual de Azure que se ejecuta en la máquina. Si la VM se creó a partir de una imagen de Azure Marketplace, el agente se instala y se ejecuta. Si crea una máquina virtual personalizada o migra una máquina local, es posible que deba [instalar el agente manualmente](backup-azure-arm-vms-prepare.md#install-the-vm-agent).

## <a name="configure-a-backup-policy"></a>Configuración de una directiva de copia de seguridad

1. Si todavía no ha creado un almacén de copia de seguridad de Recovery Services, siga [estas instrucciones](backup-create-rs-vault.md).
1. Vaya al Centro de copias de seguridad y haga clic en **+ Copia de seguridad** en la pestaña **Información general**.

    ![Panel de copia de seguridad](./media/backup-azure-vms-encryption/select-backup.png)

1. Seleccione **Azure Virtual Machines** en **Tipo de origen de datos** y seleccione el almacén que ha creado, a continuación, haga clic en **Continuar**.

     ![Panel de escenario](./media/backup-azure-vms-encryption/select-backup-goal-one.png)

1. Seleccione la directiva que quiere asociar al almacén y, luego, seleccione **Aceptar**.
   - Una directiva de copia de seguridad especifica cuándo se realizan las copias de seguridad y cuánto tiempo se almacenan.
   - Los detalles de la directiva predeterminada se muestran en el menú desplegable.

   ![Elegir directiva de copia de seguridad](./media/backup-azure-vms-encryption/select-backup-goal-two.png)
    
1. Si no quiere usar la directiva predeterminada, seleccione **Crear nueva** y [Crear una directiva personalizada](backup-azure-arm-vms-prepare.md#create-a-custom-policy).

1. En **Máquinas virtuales**, seleccione **Agregar**.

    ![Agregar máquinas virtuales](./media/backup-azure-vms-encryption/add-virtual-machines.png)

1. Elija las máquinas virtuales cifradas que quiere copiar mediante la directiva seleccionada y seleccione **Aceptar**.

      ![Selección de las máquinas virtuales cifradas](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)

1. Si usa Azure Key Vault, en la página del almacén, verá un mensaje que indica que Azure Backup necesita acceso de solo lectura a las claves y los secretos de Key Vault.

    - Si recibe este mensaje, no necesita hacer nada.

        ![Acceso correcto](./media/backup-azure-vms-encryption/access-ok.png)

    - Si recibe este mensaje, debe establecer los permisos tal como se describe en el [siguiente procedimiento](#provide-permissions).

        ![Advertencia de acceso](./media/backup-azure-vms-encryption/access-warning.png)

1. Haga clic en **Habilitar copia de seguridad** para implementar la directiva de copia de seguridad en el almacén y habilitar la copia de seguridad para las máquinas virtuales seleccionadas.

## <a name="trigger-a-backup-job"></a>Desencadenamiento de un trabajo de copia de seguridad

La copia de seguridad inicial se ejecutará según la programación, peor puede ejecutarla inmediatamente de la manera siguiente:

1. Vaya al **Centro de copias de seguridad** y seleccione el elemento de menú **Instancias de Backup**.
1. Seleccione **Azure Virtual Machines** como **Tipo de origen de datos** y busque la VM que configuró para la copia de seguridad.
1. Haga clic con el botón derecho en la fila pertinente o seleccione el icono de más (…) y haga clic en **Realizar copia de seguridad ahora**.
1. En **Realizar copia de seguridad ahora**, use el control del calendario para seleccionar el último día que debería retenerse el punto de recuperación. Después, seleccione **Aceptar**.
1. Supervise las notificaciones del portal.
   Para supervisar el progreso del trabajo, vaya a **Centro de copias de seguridad** > **Trabajos de copia de seguridad** y filtre la lista por trabajos **En curso**.
   Según el tamaño de la máquina virtual, la creación de la copia de seguridad inicial puede tardar un tiempo.

## <a name="provide-permissions"></a>Proporcionar los permisos

Azure Backup necesita acceso de solo lectura para realizar la copia de seguridad de las claves y los secretos, junto con las máquinas virtuales asociadas.

- El almacén de claves está asociado con el inquilino de Azure AD de la suscripción de Azure. Si es un **usuario miembro**, Azure Backup adquiere el acceso al almacén de claves sin realizar más acciones.
- Si es un **usuario invitado**, debe proporcionar permisos para que Azure Backup acceda al almacén de claves. Debe tener acceso a los almacenes de claves para configurar Backup para VM cifradas.

Para establecer los permisos:

1. En Azure Portal, seleccione **Todos los servicios** y busque **Almacenes de claves**.
1. Seleccione el almacén de claves asociado a la máquina virtual cifrada que va a copiar.

    >[!TIP]
    >Para identificar el almacén de claves asociado a una VM, use el siguiente comando de PowerShell. Sustituya el nombre del grupo de recursos y el nombre de la VM:
    >
    >`Get-AzVm -ResourceGroupName "MyResourceGroup001" -VMName "VM001" -Status`
    >
    > Busque el nombre del almacén de claves en esta línea:
    >
    >`SecretUrl            : https://<keyVaultName>.vault.azure.net`
    >

1. Seleccione **Directivas de acceso** > **Agregar directiva de acceso**.

    ![Agregar directiva de acceso](./media/backup-azure-vms-encryption/add-access-policy.png)

1. En **Agregar directiva de acceso** > **Configurar a partir de una plantilla (opcional)** , seleccione **Azure Backup**.
    - Los permisos necesarios se rellenan previamente en **Permisos clave** y **Permisos de secretos**.
    - Si la máquina virtual está cifrada con **solo BEK**, quite la selección de **permisos de clave** puesto que solo necesita permisos para secretos.

    ![Selección de Azure Backup](./media/backup-azure-vms-encryption/select-backup-template.png)

1. Seleccione **Agregar**. **Backup Management Service** (Servicio de administración de copias de seguridad) se agrega a **Directivas de acceso**.

    ![Directivas de acceso](./media/backup-azure-vms-encryption/backup-service-access-policy.png)

1. Seleccione **Guardar** para proporcionar a Azure Backup los permisos.

## <a name="next-steps"></a>Pasos siguientes

[Copia de seguridad y restauración de máquinas virtuales de Azure cifradas](restore-azure-encrypted-virtual-machines.md)

Si experimenta algún problema, consulte estos artículos:

- [Errores comunes](backup-azure-vms-troubleshoot.md) al realizar una copia de seguridad de máquinas virtuales de Azure cifradas y restaurarlas.
- Problemas de la [extensión de copia de seguridad o del agente de máquina virtual de Azure](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md).
