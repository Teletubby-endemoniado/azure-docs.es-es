---
title: Administración de servidores mediante Azure Portal en Azure Database for MySQL
description: Aprenda a administrar un servidor de Azure Database for MySQL desde Azure Portal.
author: Bashar-MSFT
ms.author: bahusse
ms.service: mysql
ms.topic: how-to
ms.date: 1/26/2021
ms.openlocfilehash: 5e6f21b2ae49539a8728a307eb33bfdba33f9688
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128633058"
---
# <a name="manage-an-azure-database-for-mysql-server-using-the-azure-portal"></a>Administración de un servidor de Azure Database for MySQL mediante Azure Portal

[!INCLUDE[applies-to-mysql-single-server](includes/applies-to-mysql-single-server.md)]

En este artículo se muestra cómo administrar los servidores de Azure Database for MySQL. Entre las tareas de administración se incluyen el escalado de proceso y almacenamiento, el restablecimiento de contraseñas de administración y la visualización de detalles del servidor.

> [!NOTE]
> Este artículo contiene referencias al término *esclavo*, un término que Microsoft ya no usa. Cuando se elimine el término del software, se eliminará también de este artículo.
>

## <a name="sign-in"></a>Iniciar sesión

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="create-a-server"></a>Creación de un servidor

Visite el [inicio rápido](quickstart-create-mysql-server-database-using-azure-portal.md) para obtener información sobre cómo crear un servidor de Azure Database for MySQL y empezar a trabajar con él.

## <a name="scale-compute-and-storage"></a>Escalado de proceso y almacenamiento

Tras crear el servidor, puede escalar entre los niveles De uso general y Con optimización para memoria a medida que cambien sus necesidades. También puede escalar el proceso y la memoria aumentando o reduciendo núcleos virtuales. El almacenamiento se puede escalar verticalmente (pero no reducir).

### <a name="scale-between-general-purpose-and-memory-optimized-tiers"></a>Escalado entre niveles De uso general y Con optimización de memoria

Puede escalar desde De uso general a Optimizada para memoria y viceversa. No se permite el cambio desde un plan Básico después de haber creado el servidor.

1. Seleccione el servidor en Azure Portal. Seleccione **Plan de tarifa**, que se encuentra en la sección **Configuración**.

2. Seleccione **De uso general** o **Con optimización de memoria**, dependiendo de lo que esté escalando.

   :::image type="content" source="./media/howto-create-manage-server-portal/change-pricing-tier.png" alt-text="Captura de pantalla de Azure Portal para elegir el nivel Básico, De uso general u Optimizado para memoria en Azure Database for MySQL":::

   > [!NOTE]
   > El cambio de los niveles provoca un reinicio del servidor.

3. Seleccione **Aceptar** para guardar los cambios.

### <a name="scale-vcores-up-or-down"></a>Escalado o reducción vertical de los núcleos virtuales

1. Seleccione el servidor en Azure Portal. Seleccione **Plan de tarifa**, que se encuentra en la sección **Configuración**.

2. Cambie el valor de **vCore** moviendo el control deslizante hasta alcanzar el valor deseado.

    :::image type="content" source="./media/howto-create-manage-server-portal/scaling-compute.png" alt-text="Captura de pantalla de Azure Portal para elegir la opción de núcleo virtual en Azure Database for MySQL":::

    > [!NOTE]
    > El escalado de los núcleos virtuales provoca un reinicio del servidor.

3. Seleccione **Aceptar** para guardar los cambios.

### <a name="scale-storage-up"></a>Escalado vertical del almacenamiento

1. Seleccione el servidor en Azure Portal. Seleccione **Plan de tarifa**, que se encuentra en la sección **Configuración**.

2. Cambie el valor de **Almacenamiento** moviendo el control deslizante hacia arriba hasta alcanzar el valor que se quiera.

   :::image type="content" source="./media/howto-create-manage-server-portal/scaling-storage.png" alt-text="Captura de pantalla de Azure Portal para elegir la escala de almacenamiento en Azure Database for MySQL":::

   > [!NOTE]
   > El almacenamiento no se puede reducir verticalmente.

3. Seleccione **Aceptar** para guardar los cambios.

## <a name="update-admin-password"></a>Actualización de la contraseña del administrador

La contraseña del rol de administrador se puede cambiar en Azure Portal.

1. Seleccione el servidor en Azure Portal. En la ventana **Información general**, seleccione **Restablecer contraseña**.

   :::image type="content" source="./media/howto-create-manage-server-portal/overview-reset-password.png" alt-text="Captura de pantalla de Azure Portal para restablecer la contraseña en Azure Database for MySQL":::

2. Escriba la contraseña nueva y confírmela. El cuadro de texto le pedirá los requisitos de complejidad de la contraseña.

   :::image type="content" source="./media/howto-create-manage-server-portal/reset-password.png" alt-text="Captura de pantalla de Azure Portal para restablecer la contraseña y guardarla en Azure Database for MySQL":::

3. Seleccione **Aceptar** para guardar la nueva contraseña.
 

> [!IMPORTANT]
> Al restablecer la contraseña de administrador del servidor, se restablecerán automáticamente los privilegios de administrador del servidor a los valores predeterminados. Considere la posibilidad de restablecer la contraseña del administrador del servidor si ha revocado accidentalmente uno o varios de los privilegios de administrador del servidor.
   
> [!NOTE]
> De forma predeterminada, el usuario administrador del servidor tiene los siguientes privilegios: SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT y TRIGGER

## <a name="delete-a-server"></a>Eliminación de un servidor

Puede eliminar el servidor cuando ya no lo necesite.

1. Seleccione el servidor en Azure Portal. En la ventana **Información general**, seleccione **Eliminar**.

   :::image type="content" source="./media/howto-create-manage-server-portal/overview-delete.png" alt-text="Captura de pantalla de Azure Portal para eliminar el servidor en Azure Database for MySQL":::

2. Escriba el nombre del servidor en el cuadro de entrada para confirmar que es el servidor que quiere eliminar.

   :::image type="content" source="./media/howto-create-manage-server-portal/confirm-delete.png" alt-text="Captura de pantalla de Azure Portal para confirmar la eliminación del servidor en Azure Database for MySQL":::

   > [!NOTE]
   > La eliminación de un servidor es irreversible.

3. Seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre [copias de seguridad y restauración del servidor](howto-restore-server-portal.md).
- Obtenga más información sobre las opciones de [supervisión y ajuste en Azure Database for MySQL](concepts-monitoring.md).
