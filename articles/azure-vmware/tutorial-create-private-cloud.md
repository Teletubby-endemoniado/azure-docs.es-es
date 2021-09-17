---
title: 'Tutorial: Implementación de una nube privada de Azure VMware Solution'
description: Aprenda a crear y administrar una nube privada de Azure VMware Solution.
ms.topic: tutorial
ms.date: 06/11/2021
ms.openlocfilehash: d91e9fe9261aa4a04f5e5dffd3505742d9886623
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2021
ms.locfileid: "121730355"
---
# <a name="tutorial-deploy-an-azure-vmware-solution-private-cloud"></a>Tutorial: Implementación de una nube privada de Azure VMware Solution

La nube privada de Azure VMware Solution permite implementar un clúster de vSphere en Azure. De manera predeterminada, hay un clúster vSAN para cada nube privada que se crea. Los clústeres se pueden agregar, eliminar y escalar.  El número mínimo de hosts por clúster es tres. Puede agregar más hosts de uno en uno, hasta un máximo de 16 por clúster. El número máximo de clústeres por nube privada es cuatro.  La implementación inicial de Azure VMware Solution incluye tres hosts. 

Los clústeres de prueba están disponibles con fines de evaluación y están limitados a tres hosts. Hay un solo clúster de prueba por cada nube privada. Puede escalar un clúster de prueba en un solo host durante el período de evaluación.

Se usa vSphere y NSX-T Manager para administrar la mayoría de los otros aspectos de la configuración o del funcionamiento de los clústeres. Todo el almacenamiento local de cada host de un clúster está bajo el control de vSAN.

>[!TIP]
>Si necesita aumentar el número de implementación inicial, siempre puede extender el clúster y agregar clústeres adicionales más adelante.

Dado que Azure VMware Solution no permite administrar la nube privada con la instancia local de vCenter en el inicio, deberá seguir pasos adicionales en la configuración.  En este tutorial se tratan estos pasos y los requisitos previos relacionados.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una nube privada de Azure VMware Solution
> * Comprobar la nube privada implementada

## <a name="prerequisites"></a>Prerrequisitos

- Permisos y derechos administrativos adecuados para crear una nube privada. Debe tener como mínimo un nivel de colaborador en la suscripción.
- Consulte la información recopilada en el tutorial de [planeamiento](plan-private-cloud-deployment.md) para implementar Azure VMware Solution.
- Asegúrese de tener correctamente configuradas las opciones de red, tal y como se describe en la [lista de comprobación del planeamiento de red](tutorial-network-checklist.md).
- Los hosts se han aprovisionado y [se ha registrado el proveedor de recursos](deploy-azure-vmware-solution.md#register-the-microsoftavs-resource-provider) Microsoft.AVS.

## <a name="create-a-private-cloud"></a>Creación de una nube privada

[!INCLUDE [create-avs-private-cloud-azure-portal](includes/create-private-cloud-azure-portal-steps.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido cómo:

> [!div class="checklist"]
> * Creación de una nube privada de Azure VMware Solution
> * Comprobar la nube privada implementada
> * Eliminación de una nube privada de Azure VMware Solution

Continúe con el siguiente tutorial para aprender a crear un jumpbox. Use el jumpbox para conectarse a su entorno y administrar la nube privada localmente.


> [!div class="nextstepaction"]
> [Acceso a una nube privada de Azure VMware Solution](tutorial-access-private-cloud.md)
