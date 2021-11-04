---
title: 'Tutorial: Creación de un inquilino de Azure Active Directory B2C'
description: Siga este tutorial para aprender a preparar el registro de las aplicaciones mediante la creación de un inquilino de Azure Active Directory B2C desde Azure Portal.
services: B2C
author: kengaderdus
manager: CelesteDG
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 10/26/2021
ms.author: kengaderdus
ms.subservice: B2C
ms.custom: b2c-support
ms.openlocfilehash: 48577b7625b80954d856d02fcee9e0696393bc55
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131036264"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Tutorial: Creación de un inquilino de Azure Active Directory B2C

Para que sus aplicaciones puedan interactuar con Azure Active Directory B2C (Azure AD B2C), deben estar registradas en un inquilino que administre. 

> [!NOTE]
> Se pueden crear hasta 20 inquilinos por suscripción. Este límite ayuda a proteger los recursos frente a amenazas, como ataques por denegación de servicio. Además, se aplica en Azure Portal y en la API de creación de inquilinos subyacente. Si necesita crear más de 20 inquilinos, póngase en contacto con el [Soporte técnico de Microsoft](support-options.md).
> 
> Si desea volver a utilizar el nombre de un inquilino que intentó eliminar anteriormente pero aparece el error "ya está en uso por otro directorio" al escribir el nombre de dominio, debe [seguir primero estos pasos para eliminar por completo el inquilino](./faq.yml?tabs=app-reg-ga#how-do-i-delete-my-azure-ad-b2c-tenant-). Se necesita al menos un rol de administrador de la suscripción. Para poder reutilizar el nombre de dominio, es posible que, después de eliminar el inquilino, también tenga que cerrar la sesión e iniciarla de nuevo.

En este artículo aprenderá a:

> [!div class="checklist"]
> * Creación de un inquilino de Azure AD B2C
> * Vinculación del inquilino a la suscripción
> * Cambio al directorio que contiene el inquilino de Azure AD B2C
> * Incorporación del recurso de Azure AD B2C como **Favorito** en Azure Portal

En el siguiente tutorial aprenderá a registrar una aplicación.

## <a name="prerequisites"></a>Requisitos previos

- Suscripción a Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

- Se requiere una cuenta de Azure a la que se haya asignado al menos el rol de [Colaborador](../role-based-access-control/built-in-roles.md) dentro de la suscripción o un grupo de recursos dentro de la suscripción. 

## <a name="create-an-azure-ad-b2c-tenant"></a>Creación de un inquilino de Azure AD B2C

1. Inicie sesión en [Azure Portal](https://portal.azure.com/). 

1. Vaya al directorio que contiene la suscripción:
    1. En la barra de herramientas de Azure Portal, seleccione el icono de filtro **Directorios y suscripciones**. 
    
        ![Icono de filtro Directorios y suscripciones](media/tutorial-create-tenant/directories-subscription-filter-icon.png)

    1. Busque el directorio que contiene la suscripción y seleccione el botón **Cambiar** situado al lado. Al cambiar un directorio, se vuelve a cargar el portal.

        ![Directorios y suscripciones con el botón Cambiar](media/tutorial-create-tenant/switch-directory.png)

1. Agregue **Microsoft.AzureActiveDirectory** como proveedor de recursos para la suscripción de Azure que utiliza ([más información](../azure-resource-manager/management/resource-providers-and-types.md?WT.mc_id=Portal-Microsoft_Azure_Support#register-resource-provider-1)):

    1. En Azure Portal, busque y seleccione **Suscripciones**.
    2. Seleccione la suscripción y luego **Proveedores de recursos** en el menú izquierdo. Si no ve el menú izquierdo, seleccione el icono **Mostrar el menú de <nombre de la suscripción>** en la parte superior izquierda de la página para abrirlo.
    3. Asegúrese de que la fila **Microsoft.AzureActiveDirectory** muestre el estado **Registrado**. Si no es así, seleccione la fila y, a continuación, seleccione **Registrar**.

1. En el menú de Azure Portal o en la **página principal**, seleccione **Crear un recurso**.

   ![Selección del botón Crear un recurso](media/tutorial-create-tenant/create-a-resource.png)

1. Busque **Azure Active Directory B2C** y, a continuación, seleccione **Crear**.
2. Seleccione **Crear un nuevo inquilino de Azure AD B2C**.

    ![Creación de un nuevo inquilino de Azure AD B2C seleccionado en Azure Portal](media/tutorial-create-tenant/portal-02-create-tenant.png)

1. En la página **Crear directorio**, escriba lo siguiente:

   - **Nombre de la organización**: escriba un nombre para el inquilino de Azure AD B2C.
   - **Nombre de dominio inicial**: escriba un nombre de dominio para el inquilino de Azure AD B2C.
   - **País o región**: seleccione el país o región. Esta selección no se puede cambiar más adelante.
   - **Suscripción**: seleccione su suscripción en la lista.
   - **Grupo de recursos**: seleccione o busque el grupo de recursos que contendrá el inquilino.

    ![Creación de un formulario del inquilino con valores de ejemplo en Azure Portal](media/tutorial-create-tenant/review-and-create-tenant.png)

1. Seleccione **Revisar + crear**.
1. Revise la configuración del directorio. Seleccione **Crear**. Obtenga más información sobre la [solución de problemas de implementación](../azure-resource-manager/templates/common-deployment-errors.md).

Puede vincular varios inquilinos de Azure AD B2C a una única suscripción de Azure con fines de facturación. Para vincular un inquilino, debe ser administrador en el inquilino de Azure AD B2C y tener asignado al menos un rol de colaborador en la suscripción de Azure. Consulte [Vinculación de un inquilino de Azure AD B2C a una suscripción](billing.md#link-an-azure-ad-b2c-tenant-to-a-subscription).

## <a name="select-your-b2c-tenant-directory"></a>Selección de su directorio de inquilinos B2C

Para empezar a usar su nuevo inquilino de Azure AD B2C, debe cambiar al directorio que contiene el inquilino:
1. En la barra de herramientas de Azure Portal, seleccione el icono de filtro **Directorios y suscripciones**.
1. En la pestaña **Todos los directorios**, busque el directorio que contiene el inquilino de Azure AD B2C y, a continuación, seleccione el botón **Cambiar** justo al lado.

Si al principio no ve el nuevo inquilino de Azure B2C en la lista, actualice la ventana del explorador o cierre la sesión y vuelva a iniciarla. A continuación, en la barra de herramientas de Azure Portal, seleccione de nuevo el filtro **Directorios y suscripciones**.


## <a name="add-azure-ad-b2c-as-a-favorite-optional"></a>Incorporación de Azure AD B2C como favorito (opcional)

Este paso opcional facilita la selección de su inquilino de Azure AD B2C en los siguientes tutoriales (y todos los posteriores).

En lugar de buscar *Azure AD B2C* en **Todos los servicios** cada vez que desee trabajar con el inquilino, puede establecer el recurso como favorito. A continuación, puede seleccionarlo en la sección **Favoritos** del menú del portal para ir rápidamente al inquilino de Azure AD B2C.

Solo tiene que realizar esta operación una vez. Antes de realizar estos pasos, asegúrese de que ha cambiado al directorio que contiene su inquilino de Azure AD B2C como se describe en la sección anterior, [Selección de su directorio de inquilinos B2C](#select-your-b2c-tenant-directory).

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. En el menú de Azure Portal, seleccione **Todos los servicios**.
1. En el cuadro de búsqueda **Todos los servicios**, busque **Azure AD B2C**, mantenga el mouse sobre el resultado de la búsqueda y, a continuación, seleccione el icono de estrella en la información sobre herramientas. Ahora **Azure AD B2C** aparece en Azure Portal bajo **Favoritos**.
1. Si desea cambiar la posición del nuevo favorito, vaya al menú de Azure Portal, seleccione **Azure AD B2C** y arrástrelo hacia arriba o hacia abajo hasta la posición deseada.

    ![Azure AD B2C, menú Favoritos, Microsoft Azure Portal](media/tutorial-create-tenant/portal-08-b2c-favorite.png)

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo:

> [!div class="checklist"]
> * Creación de un inquilino de Azure AD B2C
> * Vinculación del inquilino a la suscripción
> * Cambio al directorio que contiene el inquilino de Azure AD B2C
> * Incorporación del recurso de Azure AD B2C como **Favorito** en Azure Portal

A continuación, aprenda a registrar una aplicación web en el nuevo inquilino.

> [!div class="nextstepaction"]
> [Registro de las aplicaciones >](tutorial-register-applications.md)
