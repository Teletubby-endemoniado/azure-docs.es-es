---
title: Administración de copias de seguridad de recursos compartidos de archivos de Azure
description: En este artículo se describen las tareas comunes para administrar y supervisar los recursos compartidos de archivos de Azure de los que Azure Backup realiza una copia de seguridad.
ms.topic: conceptual
ms.date: 10/08/2021
ms.openlocfilehash: 421162387b28777acf1c4f86288796d8066468a6
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130216055"
---
# <a name="manage-azure-file-share-backups"></a>Administración de copias de seguridad de recursos compartidos de archivos de Azure

En este artículo se describen las tareas comunes para administrar y supervisar los recursos compartidos de archivos de Azure de los que [Azure Backup](./backup-overview.md) realiza una copia de seguridad. Aprenderá a realizar tareas de administración en el almacén de Recovery Services.

## <a name="monitor-jobs"></a>Supervisión de trabajos

Al desencadenar operaciones de copia de seguridad o restauración, el servicio de copia de seguridad crea un trabajo para realizar un seguimiento. Puede supervisar el progreso de todos los trabajos en la página **Backup Jobs** (Trabajos de copia de seguridad).

Para abrir la página **Backup Jobs** (Trabajos de copia de seguridad), siga estos pasos:

1. Abra el almacén de Recovery Services que usó para configurar la copia de seguridad de los recursos compartidos de archivos. En el panel **Información general**, seleccione **Trabajos de copia de seguridad** en la sección **Supervisión**.

   ![Trabajos de copia de seguridad en la sección Supervisión](./media/manage-afs-backup/backup-jobs.png)

1. Después de seleccionar **Aceptar**, el panel **Trabajos de copia de seguridad** muestra el estado de todos los trabajos. Seleccione el nombre de la carga de trabajo correspondiente al recurso compartido de archivos que desea supervisar.

   ![Nombre de la carga de trabajo](./media/manage-afs-backup/workload-name.png)

## <a name="monitor-using-azure-backup-reports"></a>Supervisión mediante informes de Azure Backup

Azure Backup proporciona una solución de informes que usa [registros de Azure Monitor](../azure-monitor/logs/log-analytics-tutorial.md) y [libros de Azure](../azure-monitor/visualize/workbooks-overview.md). Estos recursos le ayudan a obtener mejores conclusiones sobre las copias de seguridad. Puede aprovechar estos informes para obtener visibilidad de los elementos de copia de seguridad de Azure Files, los trabajos en el nivel de elemento y los detalles de las directivas activas. Con la característica "Informe de correo electrónico" disponible en los Informes de Backup, puede crear tareas automatizadas para recibir informes periódicos por correo electrónico. [Aprenda](./configure-reports.md#get-started) cómo configurar y ver los informes de Azure Backup.

## <a name="create-a-new-policy"></a>Creación de una nueva directiva

Puede crear una nueva directiva para realizar una copia de seguridad de los recursos compartidos de archivos de Azure desde la sección **Directivas de copia de seguridad** del almacén de Recovery Services. Todas las directivas que se crean al configurar la copia de seguridad de los recursos compartidos de archivos se muestran con **Tipo de directiva** establecido en **Recurso compartido de archivos de Azure**.

Para crear una nueva directiva de copia de seguridad, siga estos pasos:

1. En el panel **Directivas de copia de seguridad** del almacén de Recovery Services, seleccione **+ Agregar**.

   :::image type="content" source="./media/manage-afs-backup/new-backup-policy.png" alt-text="Captura de pantalla que muestra la opción para empezar a crear una nueva directiva de copia de seguridad.":::

1. En el panel **Agregar**, seleccione **Recurso compartido de archivos de Azure** como **Tipo de directiva**.

   :::image type="content" source="./media/manage-afs-backup/define-policy-type.png" alt-text="Captura de pantalla que muestra la selección de Recurso compartido de archivos de Azure como tipo de directiva.":::

1. Cuando se abra el panel **Directiva de copia de seguridad** de **Recurso compartido de archivos de Azure**, especifique el nombre de la directiva.

1. En **Programación de copia de seguridad**, seleccione una frecuencia adecuada para las copias de seguridad: **Diaria** o **Cada hora**.

   :::image type="content" source="./media/manage-afs-backup/backup-frequency-types.png" alt-text="Captura de pantalla que muestra los tipos de frecuencia de las copias de seguridad.":::

   - **Diaria**: desencadena una copia de seguridad al día. Para la frecuencia diaria, seleccione los valores adecuados para:

     - **Hora**: marca de tiempo en la que se debe desencadenar el trabajo de copia de seguridad.
     - **Zona horaria**: zona horaria correspondiente del trabajo de copia de seguridad.

   - **Cada hora**: desencadena varias copias de seguridad al día. Para la frecuencia de cada hora, seleccione los valores adecuados para:
   
     - **Programación**: intervalo de tiempo (en horas) entre las copias de seguridad consecutivas.
     - **Hora de inicio**: hora a la que se debe desencadenar el primer trabajo de copia de seguridad del día.
     - **Duración**: representa la ventana de copia de seguridad (en horas), es decir, el intervalo de tiempo en el que se deben desencadenar los trabajos de copia de seguridad según la programación seleccionada.
     - **Zona horaria**: zona horaria correspondiente del trabajo de copia de seguridad.
     
     Por ejemplo, tiene el requisito de RPO (objetivo de punto de recuperación) de 4 horas y el horario de trabajo es de 9 a. m. a 9 p. m. Para cumplir estos requisitos, la configuración de la programación de copia de seguridad sería:
    
     - Programación: cada 4 horas
     - Hora de inicio: 9 a. m. 
     - Duración: 12 horas 
     
     :::image type="content" source="./media/manage-afs-backup/hourly-backup-frequency-values-scenario.png" alt-text="Captura de pantalla que muestra un ejemplo de valores de frecuencia de copia de seguridad de cada hora.":::

     En función de la selección, los detalles del trabajo de copia de seguridad (las marcas de tiempo en las que se desencadenaría el trabajo de copia de seguridad) se muestran en la hoja de la directiva de copia de seguridad.

1. En **Duración de retención**, especifique los valores de retención adecuados para las copias de seguridad, etiquetados como diaria, semanal, mensual o anual.

1. Después de definir todos los atributos de la directiva, haga clic en **Crear**.
  
### <a name="view-policy"></a>Visualización de directiva

Para ver las directivas de copia de seguridad existentes:

1. Abra el almacén de Recovery Services que usó para configurar la copia de seguridad de los recursos compartidos de archivos. En el menú del almacén de Recovery Services, seleccione **Directivas de copia de seguridad** en la sección **Administrar**. Se mostrarán todas las directivas de copia de seguridad configuradas en el almacén.

   :::image type="content" source="./media/manage-afs-backup/all-backup-policies.png" alt-text="Captura de pantalla que muestra todas las directivas de copia de seguridad.":::

1. Para ver las directivas específicas de **Recurso compartido de archivos de Azure**, seleccione **Recurso compartido de archivos de Azure** en la lista desplegable de la parte superior derecha.

   :::image type="content" source="./media/manage-afs-backup/azure-file-share.png" alt-text="Captura de pantalla que muestra el proceso para seleccionar el recurso compartido de archivos de Azure.":::

## <a name="modify-policy"></a>Modificación de directivas

Puede modificar una directiva de copia de seguridad para cambiar la frecuencia de las copias de seguridad o la duración de retención.

Para modificar una directiva:

1. Abra el almacén de Recovery Services que usó para configurar la copia de seguridad de los recursos compartidos de archivos. En el menú del almacén de Recovery Services, seleccione **Directivas de copia de seguridad** en la sección **Administrar**. Se mostrarán todas las directivas de copia de seguridad configuradas en el almacén.

   ![Todas las directivas de copia de seguridad del almacén](./media/manage-afs-backup/all-backup-policies-modify.png)

1. Para ver las directivas específicas de un recurso compartido de archivos de Azure, seleccione **Recurso compartido de archivos de Azure** en la lista desplegable de la parte superior derecha. Seleccione la directiva de copia de seguridad que desee modificar.

   ![Recurso compartido de archivos de Azure que se va a modificar](./media/manage-afs-backup/azure-file-share-modify.png)

1. Se abrirá el panel **Programación**. Edite la **Programación de copia de seguridad** y la **Duración de retención** según sea necesario y seleccione **Guardar**. Verá el mensaje "Actualización en curso" en el panel. Una vez que se actualicen correctamente los cambios en la directiva, verá el mensaje "La directiva de copia de seguridad se actualizó correctamente".

   ![Directiva modificada y guardada](./media/manage-afs-backup/save-policy.png)

## <a name="stop-protection-on-a-file-share"></a>Detención de la protección en un recurso compartido de archivos

Hay dos maneras de dejar de proteger recursos compartidos de archivos de Azure:

* Detener todos los trabajos futuros de copia de seguridad y *eliminar todos los puntos de recuperación*.
* Detener todos los trabajos futuros de copia de seguridad pero *dejar los puntos de recuperación*.

Puede que dejar los puntos de recuperación en el almacenamiento conlleve un costo asociado, dado que las instantáneas subyacentes creadas por Azure Backup se conservarán. La ventaja de dejarlos es que puede restaurar el recurso compartido de archivos más adelante. Para más información sobre el costo de dejar los puntos de recuperación, consulte la [información sobre precios](https://azure.microsoft.com/pricing/details/backup/). Si opta por eliminar todos los puntos de recuperación, no podrá restaurar el recurso compartido de archivos.

Para dejar de proteger recursos compartidos de archivos de Azure, siga estos pasos:

1. Abra el almacén de Recovery Services que contiene los puntos de recuperación del recurso compartido de archivos. Seleccione **Elementos de copia de seguridad** en la sección **Elementos protegidos**. Aparecerá la lista de tipos de elementos de copia de seguridad.

   ![Elementos de copia de seguridad](./media/manage-afs-backup/backup-items.png)

1. En la lista **Backup Management Type** (Tipo de administración de copia de seguridad), seleccione **Azure Storage (Azure Files)** . Aparecerá la lista **Elementos de copia de seguridad (Azure Storage [Azure Files])** .

   ![Selección de Azure Storage (Azure Files)](./media/manage-afs-backup/azure-storage-azure-files.png)

1. En la lista **Elementos de copia de seguridad (Azure Storage [Azure Files])** , seleccione el elemento de copia de seguridad que desee dejar de proteger.

1. Seleccione la opción **Detener copia de seguridad**.

   ![Seleccionar Detener copia de seguridad](./media/manage-afs-backup/stop-backup.png)

1. En el panel **Detener copia de seguridad**, seleccione **Retener datos de copia de seguridad** o **Eliminar datos de la copia de seguridad**. Luego, seleccione **Detener copia de seguridad**.

    ![Selección de Retener datos de copia de seguridad o Eliminar datos de la copia de seguridad](./media/manage-afs-backup/retain-or-delete-backup-data.png)

## <a name="resume-protection-on-a-file-share"></a>Reanudación de la protección en un recurso compartido de archivos

Si seleccionó la opción **Retener datos de copia de seguridad** al detener la protección del recurso compartido de archivos, es posible reanudar la protección. Si seleccionó la opción **Eliminar datos de la copia de seguridad**, no se puede reanudar la protección del recurso compartido de archivos.

Para reanudar la protección del recurso compartido de archivos de Azure:

1. Abra el almacén de Recovery Services que contiene los puntos de recuperación del recurso compartido de archivos. Seleccione **Elementos de copia de seguridad** en la sección **Elementos protegidos**. Aparecerá la lista de tipos de elementos de copia de seguridad.

   ![Elementos de copia de seguridad para reanudar](./media/manage-afs-backup/backup-items-resume.png)

1. En la lista **Backup Management Type** (Tipo de administración de copia de seguridad), seleccione **Azure Storage (Azure Files)** . Aparecerá la lista **Elementos de copia de seguridad (Azure Storage [Azure Files])** .

   ![Lista de Azure Storage (Azure Files)](./media/manage-afs-backup/azure-storage-azure-files.png)

1. En la lista **Elementos de copia de seguridad (Azure Storage [Azure Files])** , seleccione el elemento de copia de seguridad para el que desee reanudar la protección.

1. Seleccione la opción **Reanudar copia de seguridad**.

   ![Selección de Reanudar copia de seguridad](./media/manage-afs-backup/resume-backup.png)

1. Se abrirá el panel **Directiva de copia de seguridad**. Seleccione la directiva que desee para reanudar la copia de seguridad.

1. Después de seleccionar una directiva de copia de seguridad, seleccione **Guardar**. Verá el mensaje "Actualización en curso" en el portal. Después de que la copia de seguridad se reanude correctamente, verá el mensaje "La directiva de copia de seguridad del recurso compartido de archivos de Azure protegido se actualizó correctamente".

   ![Directiva de copia de seguridad actualizada correctamente](./media/manage-afs-backup/successfully-updated.png)

## <a name="delete-backup-data"></a>Eliminación de datos de copia de seguridad

Puede eliminar la copia de seguridad de un recurso compartido de archivos durante el trabajo **Detener copia de seguridad** o en cualquier momento después de detener la protección. Podría ser conveniente esperar días o semanas antes de eliminar los puntos de recuperación. Al eliminar datos de copia de seguridad, no se pueden elegir los puntos de recuperación específicos que se van a eliminar. Si decide eliminar los datos de copia de seguridad, se eliminarán todos los puntos de recuperación asociados con el recurso compartido de archivos.

En el procedimiento siguiente se presupone que se ha detenido la protección del recurso compartido de archivos.

Para eliminar los datos de copia de seguridad del recurso compartido de archivos de Azure:

1. Una vez detenido el trabajo de copia de seguridad, dispondrá de las opciones **Reanudar copia de seguridad** y **Eliminar datos de la copia de seguridad** en el panel **Elemento de copia de seguridad**. Seleccione la opción **Eliminar datos de la copia de seguridad**.

   ![Eliminación de datos de copia de seguridad](./media/manage-afs-backup/delete-backup-data.png)

1. Se abrirá el panel **Eliminar datos de la copia de seguridad**. Escriba el nombre del recurso compartido de archivos para confirmar la eliminación. Opcionalmente, puede proporcionar más información en los cuadros **Motivo** o **Comentarios**. Cuando esté seguro de querer eliminar los datos de copia de seguridad, seleccione **Eliminar**.

   ![Confirmación de la eliminación de los datos](./media/manage-afs-backup/confirm-delete-data.png)

## <a name="unregister-a-storage-account"></a>Anulación del registro de una cuenta de almacenamiento

Para proteger los recursos compartidos de archivos en una cuenta de almacenamiento determinada mediante un almacén de Recovery Services diferente, primero debe [detener la protección de todos los recursos compartidos de archivos](#stop-protection-on-a-file-share) en esa cuenta de almacenamiento. A continuación, anule el registro de la cuenta del almacén de Recovery Services que se usa actualmente para la protección.

En el procedimiento siguiente se presupone que se ha detenido la protección de todos los recursos compartidos de archivos de la cuenta de almacenamiento cuyo registro desea anular.

Para anular el registro de la cuenta de almacenamiento:

1. Abra el almacén de Recovery Services en el que está registrada la cuenta de almacenamiento.
1. En el panel **Información general**, seleccione la opción **Infraestructura de Backup** en la sección **Administrar**.

   ![Seleccionar Infraestructura de Backup](./media/manage-afs-backup/backup-infrastructure.png)

1. El panel **Infraestructura de Backup** se abrirá. Seleccione **Cuentas de almacenamiento** en la sección **Cuentas de Azure Storage**.

   ![Selección de Cuentas de almacenamiento](./media/manage-afs-backup/storage-accounts.png)

1. Después de seleccionar **Cuentas de almacenamiento**, se mostrará una lista de las cuentas de almacenamiento registradas en el almacén.
1. Haga clic con el botón derecho en la cuenta cuyo registro desee eliminar y seleccione **Anular registro**.

   ![Selección de Anular registro](./media/manage-afs-backup/select-unregister.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información, vea [Solución de problemas de las copias de seguridad de recursos compartidos de archivos de Azure](./troubleshoot-azure-files.md).