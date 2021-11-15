---
title: App Service en Azure Arc
description: Introducción a la integración de App Service con Azure Arc para operadores de Azure.
ms.topic: article
ms.date: 11/02/2021
ms.openlocfilehash: 74cf4063d92aa5d563df881f2210e2ca5d08543e
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131440362"
---
# <a name="app-service-functions-and-logic-apps-on-azure-arc-preview"></a>App Service, Functions y Logic Apps en Azure Arc (versión preliminar)

Puede ejecutar App Service, Functions y Logic Apps en un clúster de Kubernetes habilitado para Azure Arc. El clúster de Kubernetes puede ser local o estar hospedado en una nube de terceros. Este enfoque permite a los desarrolladores de aplicaciones aprovechar las características de App Service. Al mismo tiempo, permite a los administradores de TI mantener el cumplimiento corporativo hospedando las aplicaciones de App Service en la infraestructura interna. También permite que otros operadores de TI protejan sus inversiones anteriores en otros proveedores de nube mediante la ejecución de App Service en clústeres de Kubernetes existentes.

> [!NOTE]
> Para obtener información sobre cómo configurar el clúster de Kubernetes para App Service, Functions y Logic Apps, consulte [Creación de un entorno de Kubernetes de App Service (versión preliminar)](manage-create-arc-environment.md).

En la mayoría de los casos, los desarrolladores de aplicaciones no necesitan saber nada más que cómo implementar en la región correcta de Azure que representa el entorno de Kubernetes implementado. Para los operadores que proporcionan el entorno y mantienen la infraestructura subyacente de Kubernetes, debe tener en cuenta los siguientes recursos de Azure:

- El clúster conectado, que es una proyección en Azure de la infraestructura de Kubernetes. Para más información, consulte [¿Qué es Kubernetes habilitado para Azure Arc?](../azure-arc/kubernetes/overview.md).
- Una extensión de clúster, que es un subrecurso del recurso de clúster conectado. La extensión de App Service [instala los pods necesarios en el clúster conectado](#pods-created-by-the-app-service-extension). Para más información sobre las extensiones de clúster, consulte [Extensiones de clúster en Kubernetes habilitado para Azure Arc](../azure-arc/kubernetes/conceptual-extensions.md).
- Una ubicación personalizada, que agrupa un grupo de extensiones y las asigna a un espacio de nombres para los recursos creados. Para más información, consulte [Ubicaciones personalizadas basadas en Kubernetes habilitado para Azure Arc](../azure-arc/kubernetes/conceptual-custom-locations.md).
- Un entorno de Kubernetes de App Service, que permite una configuración común entre aplicaciones pero no relacionada con las operaciones del clúster. Conceptualmente, se implementa en el recurso de ubicación personalizada y los desarrolladores de aplicaciones crean aplicaciones en este entorno. Esto se describe con más detalle en [Entorno de Kubernetes de App Service](#app-service-kubernetes-environment).

## <a name="public-preview-limitations"></a>Limitaciones de la vista preliminar pública

Las siguientes limitaciones de la versión preliminar pública se aplican los entornos de Kubernetes de App Service. Se actualizarán a medida que haya cambios disponibles.

| Limitación                                              | Detalles                                                                               |
|---------------------------------------------------------|---------------------------------------------------------------------------------------|
| Regiones de Azure compatibles                                 | Este de EE. UU., Oeste de Europa                                                                  |
| Requisito de redes del clúster                          | Debe admitir el tipo de servicio `LoadBalancer` y proporcionar una dirección IP estática direccionable públicamente |
| Requisito de almacenamiento del clúster                             | Debe tener la clase de almacenamiento asociada al clúster disponible para que la extensión la use a fin de admitir la implementación y la compilación de aplicaciones basadas en código cuando corresponda.                      |
| Característica: redes                                     | [No disponible (se basa en las redes del clúster)](#are-networking-features-supported)      |
| Característica: identidades administradas                             | [No disponible](#are-managed-identities-supported)                                    |
| Característica: referencias de Key Vault                           | No disponible (depende de las identidades administradas)                                         |
| Característica: extracción de imágenes de ACR con identidad administrada     | No disponible (depende de las identidades administradas)                                         |
| Característica: edición en el portal para Functions y Logic Apps | No disponible                                                                         |
| Característica: publicación FTP                                 | No disponible                                                                         |
| Registros                                                    | Log Analytics se debe configurar con la extensión de clúster; no por cada sitio.                 |

## <a name="pods-created-by-the-app-service-extension"></a>Pods creados por la extensión de App Service

Cuando la extensión de App Service se instala en el clúster de Kubernetes habilitado para Azure Arc, verá varios pods creados en el espacio de nombres de versión que se especificó. Estos pods permiten que el clúster de Kubernetes sea una extensión del proveedor de recursos `Microsoft.Web` en Azure y ayudan en la administración y el funcionamiento de las aplicaciones. Opcionalmente, puede optar por que la extensión instale [KEDA](https://keda.sh/) para el escalado controlado por eventos.
 <!-- You can only have one installation of KEDA on the cluster. If you have one already, you must disable this behavior during installation of the cluster extension `TODO`. -->

En la tabla siguiente se describe el rol de cada pod que se crea de manera predeterminada:

| Pod                                   | Descripción                                                                                                                       |
|---------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| `<extensionName>-k8se-app-controller` | Pod de operador principal que crea recursos en el clúster y mantiene el estado de los componentes.                                |
| `<extensionName>-k8se-envoy`          | Una capa de proxy de front-end para todas las solicitudes del plano de datos. Enruta el tráfico entrante a las aplicaciones correctas.                           |
| `<extensionName>-k8se-activator`      | Un destino de enrutamiento alternativo para ayudar con las aplicaciones que se han escalado a cero mientras el sistema obtiene la primera instancia disponible. |
| `<extensionName>-k8se-build-service`  | Admite operaciones de implementación y ofrece la [característica Herramientas avanzadas](resources-kudu.md).                                        |
| `<extensionName>-k8se-http-scaler`    | Supervisa el volumen de solicitudes entrantes para proporcionar información de escalado a [KEDA](https://keda.sh).                               |
| `<extensionName>-k8se-img-cacher`     | Extrae imágenes de marcador de posición y de aplicación en una caché local en el nodo.                                                                  |
| `<extensionName>-k8se-log-processor`  | Recopila registros de las aplicaciones y otros componentes y los envía a Log Analytics.                                                      |
| `placeholder-azure-functions-*`       | Se usa para acelerar los arranques en frío para Azure Functions.                                                                                 |

## <a name="app-service-kubernetes-environment"></a>Entorno de Kubernetes de App Service

El recurso de entorno de Kubernetes de App Service es necesario antes de que se puedan crear aplicaciones. Habilita la configuración común de las aplicaciones de la ubicación personalizada, como el sufijo DNS predeterminado.

Solo se puede crear un recurso de entorno de Kubernetes en una ubicación personalizada. En la mayoría de los casos, un desarrollador que crea e implementa aplicaciones no necesita conocer directamente el recurso. Se puede deducir directamente del identificador de ubicación personalizada proporcionado. Sin embargo, al definir plantillas de Azure Resource Manager, cualquier recurso de plan debe hacer referencia directamente al identificador de recurso del entorno. Los valores de ubicación personalizada del plan y el entorno especificado deben coincidir.

## <a name="faq-for-app-service-functions-and-logic-apps-on-azure-arc-preview"></a>Preguntas frecuentes sobre App Service, Functions y Logic Apps en Azure Arc (versión preliminar)

- [¿Cuánto cuesta?](#how-much-does-it-cost)
- [¿Se admiten aplicaciones windows y Linux?](#are-both-windows-and-linux-apps-supported)
- [¿Qué pilas de aplicaciones integradas se admiten?](#which-built-in-application-stacks-are-supported)
- [¿Se admiten todos los tipos de implementación de aplicaciones?](#are-all-app-deployment-types-supported)
- [¿Qué características de App Service se admiten?](#which-app-service-features-are-supported)
- [¿Se admiten las características de redes?](#are-networking-features-supported)
- [¿Se admiten identidades administradas?](#are-managed-identities-supported)
- [¿Existen límites de escalado?](#are-there-any-scaling-limits)
- [¿Qué registros se recopilan?](#what-logs-are-collected)
- [¿Qué debo hacer si veo un error de registro del proveedor?](#what-do-i-do-if-i-see-a-provider-registration-error)
- [¿Puedo implementar la extensión de servicios de aplicación en un clúster basado en ARM64?](#can-i-deploy-the-application-services-extension-on-an-arm64-based-cluster)

### <a name="how-much-does-it-cost"></a>¿Cuánto cuesta?

App Service en Azure Arc es gratis durante la versión preliminar pública.

### <a name="are-both-windows-and-linux-apps-supported"></a>¿Se admiten aplicaciones windows y Linux?

Solo se admiten aplicaciones basadas en Linux, tanto de código como de contenedores personalizados. No se admiten aplicaciones de Windows.

### <a name="which-built-in-application-stacks-are-supported"></a>¿Qué pilas de aplicaciones integradas se admiten?

Se admiten todas las pilas de Linux integradas.

### <a name="are-all-app-deployment-types-supported"></a>¿Se admiten todos los tipos de implementación de aplicaciones?

No se admite la implementación mediante FTP. Actualmente tampoco se admite `az webapp up`. Se admiten otros métodos de implementación, como Git, ZIP, CI/CD, Visual Studio y Visual Studio Code.

### <a name="which-app-service-features-are-supported"></a>¿Qué características de App Service se admiten?

Durante el período de versión preliminar, se están validando determinadas características de App Service. Cuando se admitan, se activarán sus opciones en la barra de navegación izquierda de Azure Portal. Las características que aún no se admiten permanecen atenuadas.

### <a name="are-networking-features-supported"></a>¿Se admiten las características de redes?

No. No se admiten características de redes, como las conexiones híbridas, la integración de red virtual o las restricciones de dirección IP. Las redes se deben controlar directamente en las reglas de red del propio clúster de Kubernetes.

### <a name="are-managed-identities-supported"></a>¿Se admiten identidades administradas?

No. No se pueden asignar identidades administradas a las aplicaciones cuando se ejecutan en Azure Arc. Si la aplicación necesita una identidad para trabajar con otro recurso de Azure, considere la posibilidad de usar una [entidad de servicio de aplicación](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) en su lugar.

### <a name="are-there-any-scaling-limits"></a>¿Existen límites de escalado?

Todas las aplicaciones implementadas con Azure App Service en Kubernetes con Azure Arc se pueden escalar dentro de los límites del clúster de Kubernetes subyacente.  Si el clúster de Kubernetes subyacente se queda sin recursos de proceso disponibles (CPU y memoria principalmente), las aplicaciones solo podrán escalarse al número de instancias de la aplicación que Kubernetes pueda programar con el recurso disponible.

### <a name="what-logs-are-collected"></a>¿Qué registros se recopilan?

Los registros de los componentes del sistema y las aplicaciones se escriben en la salida estándar. Ambos tipos de registro se pueden recopilar para su análisis mediante las herramientas estándar de Kubernetes. También puede configurar la extensión de clúster de App Service con un [área de trabajo de Log Analytics](../azure-monitor/logs/log-analytics-overview.md) y se enviarán todos los registros a esa área de trabajo.

De manera predeterminada, los registros de los componentes del sistema se envían al equipo de Azure. Los registros de aplicación no se envían. Para evitar que estos registros se transfieran, establezca `logProcessor.enabled=false` como un valor de configuración de extensión. Esto también deshabilitará el reenvío de la aplicación al área de trabajo de Log Analytics. Deshabilitar el procesador de registro puede afectar al tiempo necesario para cualquier caso de soporte técnico y se le pedirá que recopile registros de la salida estándar mediante otros medios.

### <a name="what-do-i-do-if-i-see-a-provider-registration-error"></a>¿Qué debo hacer si veo un error de registro del proveedor?

Al crear un recurso de entorno de Kubernetes, es posible que se muestre el error "No registered resource provider found" (No se encontró ningún proveedor de recursos registrado) para algunas suscripciones. Los detalles del error pueden incluir un conjunto de ubicaciones y de versiones de API que se consideran válidas. Si esto ocurre, puede ser que deba volver a registrarse la suscripción en el proveedor Microsoft.Web, una operación que no afecta a las aplicaciones o a las API existentes. Para volver a registrarse, use la CLI de Azure para ejecutar `az provider register --namespace Microsoft.Web --wait`. A continuación, vuelva a intentar ejecutar el comando del entorno de Kubernetes.

### <a name="can-i-deploy-the-application-services-extension-on-an-arm64-based-cluster"></a>¿Puedo implementar la extensión de servicios de aplicación en un clúster basado en ARM64?

Los clústeres basados en ARM64 no se admiten en este momento.  

## <a name="extension-release-notes"></a>Notas de la versión de la extensión

### <a name="application-services-extension-v-090-may-2021"></a>Extensión de servicios de aplicación v. 0.9.0 (mayo de 2021)

- Versión preliminar pública inicial de la extensión de los servicios de aplicación.
- Compatibilidad con el código y las implementaciones basadas en contenedores de aplicaciones web, de funciones y lógicas.
- Compatibilidad con el entorno de ejecución de aplicaciones web: .NET 3.1 y 5.0; Node JS 12 y 14; Python 3.6, 3.7, y 3.8; PHP 7.3 y 7.4; Ruby 2.5, 2.5.5, 2.6, y 2.6.2; Java SE 8u232, 8u242, 8u252, 11.05, 11.06 y 11.07; Tomcat 8.5, 8.5.41, 8.5.53, 8.5.57, 9.0, 9.0.20, 9.0.33 y 9.0.37.

### <a name="application-services-extension-v-0100-november-2021"></a>Extensión de servicios de aplicación v. 0.10.0 (noviembre de 2021)

Si su extensión estaba en la versión estable y la actualización automática de la versión secundaria está habilitada, la extensión se actualizará automáticamente.  Para actualizar manualmente la extensión a la versión más reciente de la extensión, puede ejecutar el comando siguiente.

- Se ha eliminado el requisito de la dirección IP estática preasignada necesaria para la asignación al punto final de Envoy
- Actualización de Keda a v2.4.0
- Actualización de Envoy a v1.19.0
- Actualización del entorno de ejecución de Azure Function a v3.3.1
- Establezca el recuento de réplicas predeterminado de App Controller y Envoy Controller en 2 para agregar más estabilidad.

Si su extensión estaba en la versión estable y la actualización automática de la versión secundaria está configurada en true, la extensión se actualizará automáticamente.  Para actualizar manualmente la extensión a la versión más reciente, puede ejecutar el comando siguiente.

```azurecli-interactive
    az k8s-extension update --cluster-type connectedClusters -c <clustername> -g <resource group> -n <extension name> --release-train stable --version 0.10.0
```


## <a name="next-steps"></a>Pasos siguientes

[Creación de un entorno de Kubernetes de App Service (versión preliminar)](manage-create-arc-environment.md)
