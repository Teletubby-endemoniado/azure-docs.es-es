---
title: Versiones de Kubernetes compatibles en Azure Kubernetes Service
description: Obtener información sobre la directiva de soporte técnico de la versión de Kubernetes y el ciclo de vida de los clústeres en Azure Kubernetes Service (AKS)
services: container-service
ms.topic: article
ms.date: 08/09/2021
author: palma21
ms.author: jpalma
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: ea1112f614e9c08bde1ff0427af706aae4c14896
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131063131"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Versiones de Kubernetes compatibles en Azure Kubernetes Service (AKS)

La Comunidad de Kubernetes libera versiones secundarias aproximadamente cada tres meses. Recientemente, la comunidad de Kubernetes ha [aumentado el período de soporte técnico de todas las versiones de 9 a 12 meses](https://kubernetes.io/blog/2020/08/31/kubernetes-1-19-feature-one-year-support/), a partir de la versión 1.19.

Las versiones secundarias incluyen nuevas características y mejoras. Las versiones de revisión son más frecuentes (a veces semanales) y están previstas para correcciones de errores críticos en una versión secundaria. Las versiones de revisión incluyen correcciones para vulnerabilidades de seguridad o errores importantes.

## <a name="kubernetes-versions"></a>Versiones de Kubernetes

Kubernetes usa el esquema de versiones estándar de [SemVer](https://semver.org/) en todas las versiones:

```
[major].[minor].[patch]

Example:
  1.17.7
  1.17.8
```

Cada número en la versión indica compatibilidad general con la versión anterior:

* Las **versiones principales** cambian cuando las actualizaciones de la API no son compatibles o la compatibilidad con versiones anteriores deja de funcionar.
* Las **versiones secundarias** cambian cuando se realizan actualizaciones de la funcionalidad que son compatibles las versiones anteriores de las restantes versiones secundarias.
* Las **versiones de revisión** cambian cuando se hacen correcciones de errores compatibles con versiones anteriores.

El objetivo es ejecutar la versión de revisión más reciente de la versión secundaria que se ejecuta. Por ejemplo, el clúster de producción está en **`1.17.7`** . **`1.17.8`** es la versión de revisión más reciente disponible para la serie *1.17*. Debe realizar la actualización a **`1.17.8`** lo antes posible para asegurarse de que el clúster tiene todas las revisiones y es compatible.

## <a name="kubernetes-version-support-policy"></a>Directiva de soporte técnico de versión de Kubernetes

AKS define una versión disponible con carácter general como una versión habilitada en todas las medidas del SLO o SLA y disponible en todas las regiones. AKS es compatible con tres versiones secundarias en disponibilidad general de Kubernetes:

* La versión secundaria más reciente en disponibilidad general publicada en AKS (a la que nos referiremos como N).
* Dos versiones secundarias anteriores.
    * Cada versión secundaria compatible también admite un máximo de dos (2) revisiones estables.

AKS también puede admitir versiones preliminares que se etiquetan explícitamente y están sujetas a los [términos y condiciones de la versión preliminar][preview-terms].

> [!NOTE]
> AKS usa prácticas de implementación segura que implican la implementación gradual de regiones. Esto significa que pueden pasar hasta 10 días hábiles hasta que haya una nueva versión disponible en todas las regiones.

La ventana admitida de las versiones de Kubernetes en AKS se conoce como "N-2": (N [versión más reciente] - 2 [versiones secundarias]).

Por ejemplo, si AKS presenta *1.17.a* hoy, también se proporciona compatibilidad para las versiones siguientes:

Nueva versión secundaria    |    Lista de versiones admitidas
-----------------    |    ----------------------
1.17.a               |    1.17.a, 1.17.b, 1.16.c, 1.16.d, 1.15.e, 1.15.f

Donde ".letter" es representativo de las versiones de revisión.

Cuando se introduce una nueva versión secundaria, la versión secundaria y la versión de revisión compatibles más antiguas quedan obsoletas y se retiran. Por ejemplo, la lista de versiones que se admiten actualmente es:

```
1.17.a
1.17.b
1.16.c
1.16.d
1.15.e
1.15.f
```

Versiones 1.18.\* de AKS, se eliminan todas las versiones 1.15.\* que queden fuera del soporte técnico en 30 días.

> [!NOTE]
> Si los clientes ejecutan una versión incompatible de Kubernetes, se les pedirá que la actualicen al solicitar soporte técnico para el clúster. Los clústeres que ejecutan versiones de Kubernetes no admitidas no están cubiertos por el [las directivas de soporte técnico de AKS](./support-policies.md).

Además de lo anterior, AKS admite un máximo de dos versiones de **revisión** de una versión secundaria determinada. Así pues, dadas las siguientes versiones admitidas:

```
Current Supported Version List
------------------------------
1.17.8, 1.17.7, 1.16.10, 1.16.9
```

Si AKS publica `1.17.9` y `1.16.11`, las versiones de revisión más antiguas quedan en desuso y se eliminan, y la lista de versiones admitidas pasa a ser:

```
New Supported Version List
----------------------
1.17.*9*, 1.17.*8*, 1.16.*11*, 1.16.*10*
```

### <a name="supported-kubectl-versions"></a>Versiones de `kubectl` admitidas

Puede usar una versión secundaria de `kubectl` que sea inmediatamente anterior o posterior a la versión de *kube-apiserver* y que sea coherente con la [directiva de compatibilidad de Kubernetes para kubectl](https://kubernetes.io/docs/setup/release/version-skew-policy/#kubectl).

Por ejemplo, si la versión de *kube-apiserver* es *1.17*, puede usar las versiones de `kubectl` comprendidas entre *1.16* y *1.18* con esa versión de *kube-apiserver*.

Para instalar o actualizar `kubectl` a la versión más reciente, ejecute:

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli
az aks install-cli
```

### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```powershell
Install-AzAksKubectl -Version latest
```
---

## <a name="release-and-deprecation-process"></a>Proceso de publicación y desuso

Puede consultar las próximas publicaciones de versiones y las que se quedarán obsoletas en el [Calendario publicación de AKS Kubernetes](#aks-kubernetes-release-calendar).

En el caso de las nuevas versiones **secundarias** de Kubernetes:
  * AKS publica un anuncio previo con la fecha planificada de la nueva publicación de una versión y el respectivo desuso de una versión antigua en las [notas de la versión de AKS](https://aka.ms/aks/releasenotes) al menos 30 días antes de la eliminación.
  * AKS usa [Azure Advisor](../advisor/advisor-overview.md) alertar a los usuarios si una nueva versión causará problemas en su clúster debido a las API en desuso. Azure Advisor también se usa para alertar al usuario si actualmente no tiene soporte técnico.
  * AKS publica una [notificación de estado del servicio](../service-health/service-health-overview.md) disponible para todos los usuarios con acceso al portal y AKS, y envía un correo electrónico a los administradores de suscripciones con las fechas de eliminación de versión planeadas.

    > [!NOTE]
    > Para averiguar quién es el administrador de la suscripción o cambiarlo, consulte [Administración de suscripciones de Azure](../cost-management-billing/manage/add-change-subscription-administrator.md#assign-a-subscription-administrator).

  * Los usuarios tienen **30 días** a partir de la eliminación de una versión para actualizar a una versión secundaria compatible para seguir recibiendo soporte técnico.

En el caso de las nuevas versiones de **revisión** de Kubernetes:
  * Dada la naturaleza urgente de las versiones de revisión, se pueden introducir en el servicio en cuanto estén disponibles.
  * En general, AKS no difunde profusamente el lanzamiento de las versiones de revisión. Sin embargo, AKS supervisa y valida constantemente las revisiones de CVE disponibles para admitirlas en AKS de manera puntual. Si se encuentra una revisión crítica o se requiere una acción del usuario, AKS enviará una notificación a los usuarios para que actualicen a la revisión recién disponible.
  * Desde que se quita de AKS una versión de revisión los usuarios tienen **30 días** para actualizarla a una revisión compatible sin perder el soporte técnico.

### <a name="supported-versions-policy-exceptions"></a>Excepciones de directiva de versiones admitidas

AKS se reserva el derecho de agregar o eliminar las versiones nuevas o existentes con uno o varios problemas de seguridad o errores críticos que afecten a la producción sin previo aviso.

Se pueden omitir versiones de revisión concretas o se puede acelerar su lanzamiento en función de la gravedad del error o del problema de seguridad.

## <a name="azure-portal-and-cli-versions"></a>Versiones de Azure Portal y CLI

Cuando se implementa un clúster de AKS en Azure Portal, con la CLI de Azure o con Azure PowerShell, los valores predeterminados del clúster son la versión secundaria n-1 y la revisión más reciente. Por ejemplo, si AKS admite *1.17.a*, *1.17.b*, *1.16.c*, *1.16.d*, *1.15.e* y *1.15.f*, la versión predeterminada seleccionada es la *1.16.c*.

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para averiguar qué versiones están disponibles actualmente para su suscripción y región, utilice el comando [az aks get-versions][az-aks-get-versions]. En el ejemplo siguiente se enumeran las versiones de Kubernetes disponibles para la región *EastUS*:

```azurecli-interactive
az aks get-versions --location eastus --output table
```


### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

Para averiguar qué versiones están disponibles actualmente para su suscripción y región, utilice el cmdlet [Get-AzAksVersion][get-azaksversion]. En el ejemplo siguiente se enumeran las versiones de Kubernetes disponibles para la región *EastUS*:

```azurepowershell-interactive
Get-AzAksVersion -Location eastus
```

---

## <a name="aks-kubernetes-release-calendar"></a>Calendario de publicación de AKS Kubernetes

Para ver el historial de versiones anteriores, vea [Kubernetes](https://en.wikipedia.org/wiki/Kubernetes#History).

|  Versión de K8s | Versión anterior  | Versión preliminar de AKS  | Disponibilidad general de AKS  | Final de la vida útil |
|--------------|-------------------|--------------|---------|-------------|
| 1.19  | 4 de agosto de 2020  | Septiembre de 2020   | Noviembre de 2020  | 1.22 disponibilidad general |
| 1.20  | 8 de diciembre de 2020  | Enero de 2021   | Marzo de 2021  | 1.23 Disponibilidad general |
| 1.21  | 8 de abril de 2021 | Mayo de 2021   | Julio de 2021  | 1.24 disponibilidad general |
| 1,22  | 4 de agosto de 2021 | Septiembre de 2021   | Noviembre de 2021  | 1.25 disponibilidad general |
| 1.23  | Diciembre de 2021 | Enero de 2022   | Febrero de 2022  | 1.26 disponibilidad general |

## <a name="faq"></a>Preguntas más frecuentes

**¿Cómo me notifica Microsoft las nuevas versiones de Kubernetes?**

El equipo de AKS publica anuncios previos con las fechas planeadas de las nuevas versiones de Kubernetes en nuestra documentación, en [GitHub,](https://github.com/Azure/AKS/releases) así como en los correos electrónicos a los administradores de suscripciones que poseen clústeres que van a dejar de recibir soporte técnico.  Además de los anuncios, AKS también usa [Azure Advisor](../advisor/advisor-overview.md) para notificar al cliente de Azure Portal que alerte a los usuarios si no tienen soporte técnico, así como alertarlos de las API en desuso que afectarán a su aplicación o proceso de desarrollo.

**¿Con qué frecuencia debo planear actualizar las versiones de Kubernetes para mantenerme con soporte técnico?**

A partir de Kubernetes 1.19, la [comunidad de código abierto ha ampliado el soporte técnico a 1 año](https://kubernetes.io/blog/2020/08/31/kubernetes-1-19-feature-one-year-support/). AKS se compromete a habilitar unas revisiones y un soporte técnico que cumplan los compromisos ascendentes. En el caso de los clústeres de AKS de la versión 1.19, y las versiones superiores, podrá actualizar al menos una vez al año para permanecer en una versión con soporte técnico.

**¿Qué ocurre cuando un cliente actualiza un clúster de Kubernetes con una versión secundaria que no es compatible?**

Si está en el versión *n-3* o anterior, está fuera del soporte técnico y se le pedirá que la actualice. Si la actualización de la versión n-3 a n-2 se completa correctamente, estará dentro de nuestras directivas de soporte técnico. Por ejemplo:

- Si la versión de AKS admitida más antigua es la *1.15.a* y la suya es la *1.14.b* o anterior, no es compatible.
- Si la actualización de la versión *1.14.b* a la *1.15.a*, o a cualquier otra versión superior, se realiza correctamente, estará de nuevo dentro de nuestras directivas de soporte técnico.

No se admite el cambio a una versión anterior.

**¿Qué significa "fuera de soporte técnico"?**

"Fuera del soporte técnico" significa lo siguiente:
* La versión que utiliza está fuera de la lista de versiones admitidas.
* Se le pedirá que actualice el clúster a una versión compatible cuando solicite soporte técnico, a menos que estén en el período de gracia de 30 días después de que la versión haya quedado en desuso.

Además, AKS no ofrece ningún entorno de ejecución ni otras garantías para los clústeres que estén fuera de la lista de versiones admitidas.

**¿Qué ocurre cuando un cliente escala un clúster de Kubernetes con una versión secundaria que no es compatible?**

Tanto la reducción como el escalado horizontales deberían seguir funcionando en las versiones secundarias que no son compatibles con AKS. Dado que no hay garantías de Calidad de servicio, se recomienda realizar la actualización para que el clúster vuelva tener soporte técnico.

**¿Puede un cliente permanecer en una versión de Kubernetes para siempre?**

Si un clúster ha estado sin soporte técnico durante más de tres (3) versiones secundarias y se ha descubierto que presenta riesgos para la seguridad, Azure se pondrá en contacto con usted de forma proactiva para que lo actualice con antelación. Si no hace nada, Azure se reserva el derecho de actualizar automáticamente el clúster en su nombre.

**¿Qué versión admite el plano de control si el grupo de nodos no está en una de las versiones de AKS compatibles?**

El plano de control debe estar dentro de una ventana de versiones de todos los grupos de nodos. Para obtener más información sobre la actualización del plano de control o de los grupos de nodos, visite la documentación sobre [cómo actualizar grupos de nodos](use-multiple-node-pools.md#upgrade-a-cluster-control-plane-with-multiple-node-pools).

**¿Puedo saltarme varias versiones de AKS durante la actualización del clúster?**

Cuando se actualiza un clúster de AKS compatible, no pueden omitirse las versiones secundarias de Kubernetes. Por ejemplo, las actualizaciones entre las siguientes versiones:
  * *1.12.x* -> *1.13.x*: permitida.
  * *1.13.x* -> *1.14.x*: permitida.
  * *1.12.x* -> *1.14.x*: no permitida.

Para actualizar de *1.12.x* -> *1.14.x*:
1. Actualización de *1.12.x* -> *1.14.x*.
1. Actualización de *1.12.x* -> *1.14.x*.

Solo se pueden omitir varias versiones cuando se actualiza de una versión que no es compatible a otra que sí lo es. Por ejemplo, puede realizar la actualización de la versión *1.10.x*, que no es compatible, a la versión *1.15.x*, que lo es.

**¿Puedo crear un nuevo clúster 1.xx.x durante su período de soporte técnico de 30 días?**

No. Una vez que una versión está en desuso o se ha quitado, no se puede crear un clúster con esa versión. A medida que el cambio se implementa, comenzará a ver que la versión anterior se quita de la lista de versiones. Este proceso puede tardar hasta dos semanas desde el anuncio, y será progresivo por región.

**Utilizo una versión que acaba de entrar en desuso, ¿puedo seguir agregando nuevos grupos de nodos o tendré que actualizar?**

No. No se le permitirá agregar grupos de nodos de la versión en desuso al clúster. Puede agregar grupos de nodos de una nueva versión. Sin embargo, puede requerir que primero actualice el plano de control.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo actualizar el clúster, vea [Actualización de un clúster de Azure Kubernetes Service (AKS)][aks-upgrade].

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[azure-update-channel]: https://azure.microsoft.com/updates/?product=kubernetes-service

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md
[az-aks-get-versions]: /cli/azure/aks#az_aks_get_versions
[preview-terms]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[get-azaksversion]: /powershell/module/az.aks/get-azaksversion
