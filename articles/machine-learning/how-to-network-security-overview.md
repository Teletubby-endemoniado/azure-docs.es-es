---
title: Protección de los recursos del área de trabajo mediante redes virtuales (redes virtuales)
titleSuffix: Azure Machine Learning
description: Proteja los entornos de proceso y los recursos del área de trabajo de Azure Machine Learning mediante una instancia de Azure Virtual Network (VNet) aislada.
services: machine-learning
ms.service: machine-learning
ms.subservice: enterprise-readiness
ms.reviewer: larryfr
ms.author: peterlu
author: peterclu
ms.date: 09/29/2021
ms.topic: how-to
ms.custom: devx-track-python, references_regions, contperf-fy21q1,contperf-fy21q4,FY21Q4-aml-seo-hack, security
ms.openlocfilehash: ef84fea20ce59af11abf2f76de409f1363db94c9
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131051085"
---
<!-- # Virtual network isolation and privacy overview -->
# <a name="secure-azure-machine-learning-workspace-resources-using-virtual-networks-vnets"></a>Protección de los recursos del área de trabajo de Azure Machine Learning con redes virtuales (VNet)

Proteja los entornos de proceso y los recursos del área de trabajo de Azure Machine Learning mediante redes virtuales (VNet) aisladas. En este artículo se usa un escenario de ejemplo para mostrar cómo configurar una red virtual completa.

> [!TIP]
> Este artículo forma parte de una serie sobre la protección de un flujo de trabajo de Azure Machine Learning. Consulte los demás artículos de esta serie:
>
> * [Protección de los recursos de un área de trabajo](how-to-secure-workspace-vnet.md)
> * [Protección del entorno de entrenamiento](how-to-secure-training-vnet.md)
> * [Protección del entorno de inferencia](how-to-secure-inferencing-vnet.md)
> * [Habilitación de la función de Studio](how-to-enable-studio-virtual-network.md)
> * [Uso de un DNS personalizado](how-to-custom-dns.md)
> * [Uso de un firewall](how-to-access-azureml-behind-firewall.md)

## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por hecho que está familiarizado con los siguientes temas:
+ [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
+ [Redes de IP](../virtual-network/ip-services/public-ip-addresses.md)
+ [Área de trabajo de Azure Machine Learning con un punto de conexión privado](how-to-configure-private-link.md)
+ [Grupos de seguridad de red (NSG)](../virtual-network/network-security-groups-overview.md)
+ [Firewalls de red](../firewall/overview.md)
## <a name="example-scenario"></a>Escenario de ejemplo

En esta sección, aprenderá cómo se configura un escenario de red común para proteger la comunicación de Azure Machine Learning con las direcciones IP privadas.

En la tabla siguiente se compara el modo en que los servicios acceden a diferentes partes de una red de Azure Machine Learning con y sin una red virtual:

| Escenario | Área de trabajo |  Recursos asociados | Entrenamiento del entorno de proceso | Inferencia del entorno de proceso |
|-|-|-|-|-|-|
|**Sin red virtual**| Dirección IP pública | Dirección IP pública | Dirección IP pública | Dirección IP pública |
|**Área de trabajo pública, todos los demás recursos de una red virtual** | Dirección IP pública | IP pública (punto de conexión de servicio) <br> **- o -** <br> IP privada (punto de conexión privado) | Dirección IP privada | Dirección IP privada  |
|**Protección de recursos en una red virtual**| IP privada (punto de conexión privado) | IP pública (punto de conexión de servicio) <br> **- o -** <br> IP privada (punto de conexión privado) | Dirección IP privada | Dirección IP privada  | 

* **Área de trabajo**: creación de un punto de conexión privado para su área de trabajo. El punto de conexión privado conecta el área de trabajo a la red virtual a través de varias direcciones IP privadas.
    * **Acceso público**: opcionalmente, puede habilitar el acceso público para un área de trabajo protegida.
* **Recurso asociado**: use puntos de conexión de servicio o puntos de conexión privados para conectarse a recursos del área de trabajo como Azure Storage o Azure Key Vault. En el caso de Azure Container Services, use un punto de conexión privado.
    * Los **puntos de conexión de servicio** proporcionan la identidad de la red virtual al servicio de Azure. Una vez que habilita puntos de conexión de servicio en su red virtual, puede agregar una regla de red virtual para proteger los recursos de los servicios de Azure en la red virtual. Los puntos de conexión de servicio usan direcciones IP públicas.
    * Los **puntos de conexión privados** son interfaces de red que le permiten conectarse de forma segura a un servicio con la tecnología de Azure Private Link. El punto de conexión privado usa una dirección IP privada de la red virtual, y coloca el servicio de manera eficaz en su red virtual.
* **Acceso al proceso de entrenamiento**: acceda a destinos de proceso de entrenamiento como la instancia y los clústeres de proceso de Azure Machine Learning con direcciones IP públicas (versión preliminar).
* **Acceso al proceso de inferencia**: acceda a los clústeres de proceso de Azure Kubernetes Services (AKS) con direcciones IP privadas.


En las secciones siguientes se muestra cómo proteger el escenario de red descrito anteriormente. Para proteger la red, debe hacer lo siguiente:

1. Proteger el [**área de trabajo y los recursos asociados**](#secure-the-workspace-and-associated-resources).
1. Proteger el [**entorno de entrenamiento**](#secure-the-training-environment).
1. Proteger el [**entorno de inferencia**](#secure-the-inferencing-environment).
1. Opcional: [**habilitar la funcionalidad de Studio**](#optional-enable-studio-functionality).
1. Configurar el [**firewall**](#configure-firewall-settings).
1. Configuración de la [**resolución de nombres DNS**](#custom-dns).

## <a name="public-workspace-and-secured-resources"></a>Área de trabajo pública y recursos protegidos

Si quiere acceder al área de trabajo a través de la red pública de Internet y mantener todos los recursos asociados protegidos de una red virtual, siga estos pasos:

1. Cree una instancia de [Azure Virtual Networks](../virtual-network/virtual-networks-overview.md) que contendrá los recursos que usa el área de trabajo.
1. Use __una__ de las siguientes opciones para crear un área de trabajo de acceso público:

    * Cree un área de trabajo de Azure Machine Learning que __no__ use la red virtual. Para obtener más información, consulte [Administrar áreas de trabajo de Azure Machine Learning](how-to-manage-workspace.md).
    * Cree una [área de trabajo habilitada para Private Link](how-to-secure-workspace-vnet.md#secure-the-workspace-with-private-endpoint) para habilitar la comunicación entre la red virtual y el área de trabajo. A continuación, [habilite el acceso público al área de trabajo](#optional-enable-public-access).

1. Agregue los siguientes servicios a la red virtual _ya sea_ mediante un __punto de conexión de servicio__ o mediante un __punto de conexión privado__. También debe permitir que los servicios de confianza de Microsoft accedan a estos servicios:

    | Servicio | Información de punto de conexión | Información sobre los servicios de confianza permitidos |
    | ----- | ----- | ----- |
    | __Azure Key Vault__| [Punto de conexión de servicio](../key-vault/general/overview-vnet-service-endpoints.md)</br>[Punto de conexión privado](../key-vault/general/private-link-service.md) | [Permite que los servicios de Microsoft de confianza omitan este firewall](how-to-secure-workspace-vnet.md#secure-azure-key-vault) |
    | __Cuenta de Azure Storage__ | [Punto de conexión privado y de servicio](how-to-secure-workspace-vnet.md?tabs=se#secure-azure-storage-accounts)</br>[Punto de conexión privado](how-to-secure-workspace-vnet.md?tabs=pe#secure-azure-storage-accounts) | [Concesión de acceso a servicios de Azure de confianza](../storage/common/storage-network-security.md#grant-access-to-trusted-azure-services) |
    | __Azure Container Registry__ | [Punto de conexión privado](../container-registry/container-registry-private-link.md) | [Permitir servicios de confianza](../container-registry/allow-access-trusted-services.md) |

## <a name="secure-the-workspace-and-associated-resources"></a>Proteger el área de trabajo y los recursos asociados

Realice los pasos siguientes para proteger el área de trabajo y los recursos asociados. Estos pasos permiten que los servicios se comuniquen en la red virtual.

1. Cree una instancia de [Azure Virtual Networks](../virtual-network/virtual-networks-overview.md) que contendrá el área de trabajo y otros recursos.
1. Cree una [área de trabajo habilitada para Private Link](how-to-secure-workspace-vnet.md#secure-the-workspace-with-private-endpoint) para habilitar la comunicación entre la red virtual y el área de trabajo.
1. Agregue los siguientes servicios a la red virtual _ya sea_ mediante un __punto de conexión de servicio__ o mediante un __punto de conexión privado__. También debe permitir que los servicios de confianza de Microsoft accedan a estos servicios:

    | Servicio | Información de punto de conexión | Información sobre los servicios de confianza permitidos |
    | ----- | ----- | ----- |
    | __Azure Key Vault__| [Punto de conexión de servicio](../key-vault/general/overview-vnet-service-endpoints.md)</br>[Punto de conexión privado](../key-vault/general/private-link-service.md) | [Permite que los servicios de Microsoft de confianza omitan este firewall](how-to-secure-workspace-vnet.md#secure-azure-key-vault) |
    | __Cuenta de Azure Storage__ | [Punto de conexión privado y de servicio](how-to-secure-workspace-vnet.md?tabs=se#secure-azure-storage-accounts)</br>[Punto de conexión privado](how-to-secure-workspace-vnet.md?tabs=pe#secure-azure-storage-accounts) | [Concesión de acceso a instancias de recursos de Azure](../storage/common/storage-network-security.md#grant-access-from-azure-resource-instances-preview)</br>**or**</br>[Concesión de acceso a servicios de Azure de confianza](../storage/common/storage-network-security.md#grant-access-to-trusted-azure-services) |
    | __Azure Container Registry__ | [Punto de conexión privado](../container-registry/container-registry-private-link.md) | [Permitir servicios de confianza](../container-registry/allow-access-trusted-services.md) |


![Diagrama de arquitectura que muestra cómo el área de trabajo y los recursos asociados se comunican entre sí a través de puntos de conexión de servicio o puntos de conexión privados en una red virtual](./media/how-to-network-security-overview/secure-workspace-resources.png)

Para obtener instrucciones detalladas sobre cómo completar estos pasos, consulte [Protección de un área de trabajo de Azure Machine Learning](how-to-secure-workspace-vnet.md). 

### <a name="limitations"></a>Limitaciones

La protección del área de trabajo y los recursos asociados en una red virtual presenta las siguientes limitaciones:
- El uso de un área de trabajo de Azure Machine Learning con un punto de conexión privado no es posible en las regiones de Azure China 21Vianet.
- Todos los recursos deben estar detrás de la misma red virtual. Sin embargo, se permiten subredes en la misma red virtual.

## <a name="secure-the-training-environment"></a>Protección del entorno de entrenamiento

En esta sección, aprenderá a proteger el entorno de entrenamiento en Azure Machine Learning. También aprenderá cómo Azure Machine Learning completa un trabajo de entrenamiento para comprender el funcionamiento conjunto de las configuraciones de red.

Para proteger el entorno de entrenamiento, siga estos pasos:

1. Cree una [instancia de proceso y un clúster de proceso](how-to-secure-training-vnet.md#compute-cluster) de Azure Machine Learning en la red virtual para ejecutar el trabajo de entrenamiento.
1. [Permita la comunicación entrante](how-to-secure-training-vnet.md#required-public-internet-access) para que los servicios de administración puedan enviar trabajos a los recursos de proceso. 

![Diagrama de arquitectura que muestra cómo proteger las instancias y los clústeres de proceso administrados](./media/how-to-network-security-overview/secure-training-environment.png)

Para obtener instrucciones detalladas sobre cómo completar estos pasos, consulte [Protección de un entorno de entrenamiento](how-to-secure-training-vnet.md). 

### <a name="example-training-job-submission"></a>Ejemplo de envío de trabajo de entrenamiento 

En esta sección, aprenderá cómo Azure Machine Learning se comunica de forma segura entre los servicios para enviar un trabajo de entrenamiento. Muestra cómo funcionan todas las configuraciones de forma conjunta para proteger la comunicación.

1. El cliente carga scripts y datos de entrenamiento en las cuentas de almacenamiento protegidas con un punto de conexión de servicio o privado.

1. El cliente envía un trabajo de entrenamiento al área de trabajo de Azure Machine Learning a través del punto de conexión privado.

1. El servicio Azure Batch recibe el trabajo del área de trabajo. Después, envía el trabajo de entrenamiento al entorno de proceso mediante el equilibrador de carga público para el recurso de proceso. 

1. El recurso de proceso recibe el trabajo e inicia el entrenamiento. El recurso de proceso accede a cuentas de almacenamiento seguras para descargar archivos de entrenamiento y cargar la salida.

### <a name="limitations"></a>Limitaciones

- La instancia de proceso de Azure y los clústeres de proceso de Azure deben estar en la misma red virtual, región y suscripción que el área de trabajo y sus recursos asociados. 

## <a name="secure-the-inferencing-environment"></a>Protección del entorno de inferencia

En esta sección, aprenderá las opciones disponibles para proteger un entorno de inferencia. Se recomienda usar los clústeres de Azure Kubernetes Services (AKS) para las implementaciones de producción a gran escala.

Tiene dos opciones para los clústeres de AKS en una red virtual:

- Implementar o conectar un clúster de AKS predeterminado a la red virtual.
- Conectar un clúster de AKS privado a la red virtual.

Los **clústeres de AKS predeterminados** tienen un plano de control con las direcciones IP públicas. Puede agregar un clúster de AKS predeterminado a la red virtual durante la implementación o conectar un clúster después de su creación.

Los **clústeres de AKS privados** tienen un plano de control, al que solo se puede acceder a través de direcciones IP privadas. Los clústeres de AKS privados se deben conectar una vez creado el clúster.

Para obtener instrucciones detalladas sobre cómo agregar clústeres predeterminados y privados, consulte [Protección de un entorno de inferencia](how-to-secure-inferencing-vnet.md). 

En el diagrama de red siguiente se muestra un área de trabajo de Azure Machine Learning protegida con un clúster de AKS privado conectado a la red virtual.

![Diagrama de arquitectura que muestra cómo conectar un clúster de AKS privado a la red virtual. El plano de control de AKS se coloca fuera de la red virtual del cliente.](./media/how-to-network-security-overview/secure-inferencing-environment.png)

### <a name="limitations"></a>Limitaciones

- El área de trabajo debe tener un punto de conexión privado en la misma red virtual que el clúster de AKS. Por ejemplo, cuando se usan varios puntos de conexión privados con el área de trabajo, un punto de conexión privado puede estar en la red virtual de AKS y otro en la red virtual que contiene los servicios de dependencia del área de trabajo.

## <a name="optional-enable-public-access"></a>Opcional: Habilitación del acceso público

Puede proteger el área de trabajo detrás de una red virtual mediante un punto de conexión privado y seguir permitiendo el acceso a través de la red pública de Internet. La configuración inicial equivale a [proteger el área de trabajo y los recursos asociados](#secure-the-workspace-and-associated-resources). 

Después de proteger el área de trabajo con un punto de conexión privado, siga estos pasos para permitir que los clientes desarrollen de forma remota mediante el SDK o Estudio de Azure Machine Learning:

1. [Habilite el acceso público](how-to-configure-private-link.md#enable-public-access) al área de trabajo.
1. [Configure el firewall de Azure Storage](../storage/common/storage-network-security.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#grant-access-from-an-internet-ip-range) para permitir la comunicación con la dirección IP de los clientes que se conectan a través de la red pública de Internet.

## <a name="optional-enable-studio-functionality"></a>Opcional: habilitación de la funcionalidad de Studio

[Proteger el área de trabajo](#secure-the-workspace-and-associated-resources) > [Proteger el entorno de entrenamiento](#secure-the-training-environment) > [Proteger el entorno de inferencia](#secure-the-inferencing-environment) > **Habilitar la funcionalidad de Studio** > [Configurar el firewall](#configure-firewall-settings)

Si el almacenamiento se encuentra en una red virtual, debe realizar pasos de configuración adicionales para habilitar la funcionalidad completa en Studio. De forma predeterminada, las características siguientes están deshabilitadas:

* Vista previa de los datos en Studio.
* Visualización de los datos en el diseñador.
* Implementación de un modelo en el diseñador.
* Envío de un experimento de AutoML.
* Inicio de un proyecto de etiquetado.

Para habilitar la funcionalidad de Studio completa, consulte [Uso de Azure Machine Learning Studio en una red virtual](how-to-enable-studio-virtual-network.md).

### <a name="limitations"></a>Limitaciones

El [etiquetado de datos asistido por ML](how-to-create-image-labeling-projects.md#use-ml-assisted-data-labeling) no es compatible con las cuentas de almacenamiento predeterminadas en una red virtual. En su lugar, use una cuenta de almacenamiento que no sea la predeterminada para el etiquetado de datos asistido por ML. 

> [!TIP]
> Siempre que no sea la cuenta de almacenamiento predeterminada, la cuenta usada por el etiquetado de datos se puede proteger en la red virtual. 

## <a name="configure-firewall-settings"></a>Configuración del firewall

Configure el firewall para controlar el tráfico entre los recursos del área de trabajo de Azure Machine Learning y la red pública de Internet. Aunque se recomienda usar Azure Firewall, también puede usar otros productos de firewall. 

Para obtener más información sobre la configuración del firewall, consulte [Uso de áreas de trabajo detrás de un firewall](how-to-access-azureml-behind-firewall.md).

## <a name="custom-dns"></a>DNS personalizado

Si necesita usar una solución DNS personalizada para la red virtual, debe agregar registros de host para el área de trabajo.

Para obtener más información sobre los nombres de dominio y las direcciones IP que se requieren, consulte [Uso de un área de trabajo con un servidor DNS personalizado](how-to-custom-dns.md).

## <a name="next-steps"></a>Pasos siguientes

Este artículo forma parte de una serie sobre la protección de un flujo de trabajo de Azure Machine Learning. Consulte los demás artículos de esta serie:

* [Protección de los recursos de un área de trabajo](how-to-secure-workspace-vnet.md)
* [Protección del entorno de entrenamiento](how-to-secure-training-vnet.md)
* [Protección del entorno de inferencia](how-to-secure-inferencing-vnet.md)
* [Habilitación de la función de Studio](how-to-enable-studio-virtual-network.md)
* [Uso de un DNS personalizado](how-to-custom-dns.md)
* [Uso de un firewall](how-to-access-azureml-behind-firewall.md)