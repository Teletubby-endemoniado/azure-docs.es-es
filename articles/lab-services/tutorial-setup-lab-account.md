---
title: Configuración de una cuenta de laboratorio con Azure Lab Services | Microsoft Docs
description: Aprenda a configurar una cuenta de laboratorio con Azure Lab Services, a agregar un creador de laboratorios y a especificar las imágenes de Marketplace que van a usar los laboratorios en la cuenta de laboratorio.
ms.topic: tutorial
ms.date: 07/26/2021
ms.custom: subject-rbac-steps
ms.openlocfilehash: d6107c1a70b22682636b63c0fb0f7374a0d96873
ms.sourcegitcommit: 9f1a35d4b90d159235015200607917913afe2d1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2021
ms.locfileid: "122634019"
---
# <a name="tutorial-set-up-a-lab-account-with-azure-lab-services"></a>Tutorial: Configuración de una cuenta de laboratorio con Azure Lab Services
En Azure Lab Services, una cuenta de laboratorio sirve como cuenta central en la que se administran los laboratorios de una organización. En su cuenta de laboratorio, puede conceder permiso a otros usuarios para crear laboratorios y establecer las directivas que se aplican a todos los laboratorios de la cuenta de laboratorio. En este tutorial, aprenderá a crear una cuenta de laboratorio. 

En este tutorial realizará lo siguiente:

> [!div class="checklist"]
> * Creación de una cuenta de laboratorio
> * Incorporación de un usuario al rol Creador de laboratorio

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="create-a-lab-account"></a>Creación de una cuenta de laboratorio
Los pasos siguientes muestran cómo usar Azure Portal para crear una cuenta de laboratorio con Azure Lab Services. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** en el menú de la izquierda. Seleccione **DevOps** en **Categorías**. A continuación, seleccione **Lab Services**. Si selecciona la estrella (`*`) junto a **Lab Services**, se agrega a la sección **FAVORITOS** en el menú de la izquierda. A partir de la próxima vez, seleccione **Lab Services** en **FAVORITOS**.

    ![Todos los servicios -> Lab Services](./media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. En la página **Lab Services**, seleccione **Agregar** en la barra de herramientas o seleccione el botón **Crear una cuenta de laboratorio** en la página. 

    ![Seleccione Agregar en la página Cuentas de laboratorio](./media/tutorial-setup-lab-account/add-lab-account-button.png)
4. En la pestaña **Conceptos básicos** de la página **Crear una cuenta de laboratorio**, realice las siguientes acciones: 
    1. En **Lab account name** (Nombre de la cuenta de laboratorio), escriba un nombre. 
    2. Seleccione la **suscripción de Azure** donde desea crear la cuenta de laboratorio.
    3. En **Grupo de recursos**, seleccione un grupo de recursos existente o **Crear nuevo** y escriba un nombre para el grupo de recursos.
    4. En **Ubicación**, seleccione la ubicación o región en la que desea crear la cuenta de laboratorio. 

        ![Cuenta de laboratorio: página Conceptos básicos](./media/tutorial-setup-lab-account/lab-account-basics-page.png)
    5. Seleccione **Revisar + crear**.
    6. Revise el resumen y luego seleccione **Crear**. 

        ![Revisar y crear -> Crear](./media/tutorial-setup-lab-account/create-button.png)    
5. Cuando la implementación se complete, expanda **Próximos pasos** y seleccione **Ir al recurso**. 

    ![Ir a la página de la cuenta de laboratorio](./media/tutorial-setup-lab-account/go-to-lab-account.png)
6. Confirme que ve la página **Cuenta de laboratorio**. 

    ![Página de la cuenta de laboratorio](./media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Incorporación de un usuario al rol Creador de laboratorio
Para configurar un laboratorio de clase en una cuenta de laboratorio, el usuario debe ser miembro del rol **Creador de laboratorio** en la cuenta de laboratorio. Para proporcionar a los educadores el permiso para crear laboratorios para sus clases agréguelos al rol **Creador de laboratorios**. Para obtener los pasos detallados, consulte [Asignación de roles de Azure mediante Azure Portal](../role-based-access-control/role-assignments-portal.md).

> [!NOTE]
> La cuenta que usó para crear la cuenta de laboratorio se agrega automáticamente a este rol. Si pretende usar la misma cuenta de usuario para crear un laboratorio de clase en este tutorial, puede omitir este paso. 


1. En la página **Cuenta de laboratorio**, seleccione **Control de acceso (IAM)** .

1. Seleccione **Agregar** > **Agregar asignación de roles (versión preliminar)** .

    ![Página Control de acceso (IAM) con el menú Agregar asignación de roles abierto.](../../includes/role-based-access-control/media/add-role-assignment-menu-generic.png)

1. En la pestaña **Rol**, seleccione el rol **Creador de laboratorios**.

    ![Página Agregar asignación de roles con la pestaña Rol seleccionada.](../../includes/role-based-access-control/media/add-role-assignment-role-generic.png)

1. En la pestaña **Miembros**, seleccione el usuario que quiere agregar al rol Creador de laboratorios.

1. En la pestaña **Revisión y asignación**, seleccione **Revisión y asignación** para asignar el rol.


## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha creado una cuenta de laboratorio. Para aprender a crear un laboratorio educativo como formador, pase al siguiente tutorial:

> [!div class="nextstepaction"]
> [Configuración de un laboratorio de clase](tutorial-setup-classroom-lab.md)

