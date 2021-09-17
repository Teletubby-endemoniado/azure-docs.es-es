---
title: 'Tutorial: Creación de un inquilino de Azure Active Directory B2C'
description: Siga este tutorial para aprender a preparar el registro de las aplicaciones mediante la creación de un inquilino de Azure Active Directory B2C desde Azure Portal.
services: B2C
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 12/03/2020
ms.author: mimart
ms.subservice: B2C
ms.custom: b2c-support
ms.openlocfilehash: e8f5405e55c3443b020265e68d68737f6d266a17
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2021
ms.locfileid: "122179310"
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

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-an-azure-ad-b2c-tenant"></a>Creación de un inquilino de Azure AD B2C

1. Inicie sesión en [Azure Portal](https://portal.azure.com/). Inicie sesión con una cuenta de Azure a la que se haya asignado al menos el rol [Colaborador](../role-based-access-control/built-in-roles.md) dentro de la suscripción o un grupo de recursos dentro de la suscripción.

1. Elija el directorio que contiene la suscripción.

    En la barra de herramientas de Azure Portal, seleccione el icono **Directorio + suscripción** y, luego, seleccione el directorio que contiene la suscripción. Se trata de un directorio diferente al que contendrá al inquilino de Azure AD B2C.

    ![Inquilino de suscripción: filtro Directorio + suscripción con el inquilino de suscripción seleccionado](media/tutorial-create-tenant/portal-01-pick-directory.png)

1. Agregue **Microsoft.AzureActiveDirectory** como proveedor de recursos para la suscripción de Azure que utiliza ([más información](../azure-resource-manager/management/resource-providers-and-types.md?WT.mc_id=Portal-Microsoft_Azure_Support#register-resource-provider-1)):

    1. En el menú de Azure Portal o en la **página principal**, seleccione **Suscripciones**.
    2. Seleccione la suscripción y luego seleccione **Proveedores de recursos**.
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
1. Revise la configuración del directorio. Seleccione **Crear**. Para [solución de problemas de implementación](../azure-resource-manager/templates/common-deployment-errors.md).

Puede vincular varios inquilinos de Azure AD B2C a una única suscripción de Azure con fines de facturación. Para vincular un inquilino, debe ser administrador en el inquilino de Azure AD B2C y tener asignado al menos un rol de colaborador en la suscripción de Azure. Consulte [Vinculación de un inquilino de Azure AD B2C a una suscripción](billing.md#link-an-azure-ad-b2c-tenant-to-a-subscription).

## <a name="select-your-b2c-tenant-directory"></a>Selección de su directorio de inquilinos B2C

Para empezar a usar su nuevo inquilino de Azure AD B2C, debe cambiar al directorio que contiene el inquilino.

Seleccione el filtro **Directorio y suscripción** en el menú superior de Azure Portal y seleccione el directorio que contiene el inquilino de Azure AD B2C.

Si al principio no ve el nuevo inquilino de Azure B2C en la lista en la lista, actualice la ventana del explorador y seleccione el filtro **Directorio y suscripción** de nuevo en el menú superior.

![Directorio que contiene el inquilino B2C seleccionado en Azure Portal](media/tutorial-create-tenant/portal-07-select-tenant-directory.png)

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