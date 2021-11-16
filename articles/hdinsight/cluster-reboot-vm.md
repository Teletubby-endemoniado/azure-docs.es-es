---
title: Reinicio de máquinas virtuales para los clústeres de Azure HDInsight
description: Obtenga información sobre cómo reiniciar las máquinas virtuales que no responden para los clústeres de Azure HDInsight.
ms.custom: hdinsightactive, devx-track-azurepowershell
ms.service: hdinsight
ms.topic: how-to
ms.date: 06/22/2020
ms.openlocfilehash: 8e49d85ed4646f337a492ac92be4e31edfa3b9c4
ms.sourcegitcommit: 362359c2a00a6827353395416aae9db492005613
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2021
ms.locfileid: "132487070"
---
# <a name="reboot-vms-for-hdinsight-clusters"></a>Reinicio de máquinas virtuales para los clústeres de HDInsight

Los clústeres de Azure HDInsight contienen grupos de máquinas virtuales (VM) como nodos de clúster. En el caso de los clústeres de larga duración, es posible que estos nodos dejen de responder por diversas razones. En este artículo, se describe cómo reiniciar máquinas virtuales que no respondan en un clúster de HDInsight.

## <a name="when-to-reboot"></a>Cuándo hay que reiniciar

> [!WARNING]
> Cuando se reinician las máquinas virtuales de un clúster, el nodo no está disponible para su uso y los servicios del nodo deben reiniciarse.

Mientras el nodo se reinicia, el clúster puede pasar a un estado incorrecto, y que los trabajos se ralenticen o se produzca un error en ellos. Si intenta reiniciar el nodo principal activo, se detendrán todos los trabajos en ejecución. No podrá enviar trabajos al clúster hasta que los servicios estén de nuevo en funcionamiento. Por estos motivos, solo debe reiniciar las máquinas virtuales cuando sea necesario. Considere la posibilidad de reiniciar las máquinas virtuales cuando:

- No se puede usar SSH para entrar en el nodo, pero este responde a los pings.
- El nodo de trabajo está fuera de servicio sin latido en la interfaz de usuario de Ambari.
- El disco temporal está lleno en el nodo.
- La tabla de procesos de la máquina virtual tiene muchas entradas en las que el proceso se ha completado, pero aparece con el estado "Finalizado".

> [!NOTE]
> El reinicio de las máquinas virtuales no se admite para los clústeres de **HBase** y **Kafka**, ya que podría provocar la pérdida de datos.

## <a name="use-powershell-to-reboot-vms"></a>Uso de PowerShell para reiniciar máquinas virtuales

Para la operación de reinicio del nodo se requieren dos pasos: enumerar los nodos y reiniciarlos.

1. Enumerar los nodos. Puede obtener la lista de nodos de clúster en [Get-AzHDInsightHost](/powershell/module/az.hdinsight/get-azhdinsighthost).

      ```
      Get-AzHDInsightHost -ClusterName myclustername
      ```

1. Reiniciar los hosts. Después de obtener los nombres de los nodos que desea reiniciar, reinicie los nodos mediante [Restart-AzHDInsightHost](/powershell/module/az.hdinsight/restart-azhdinsighthost).

      ```
      Restart-AzHDInsightHost -ClusterName myclustername -Name wn0-myclus, wn1-myclus
      ```

## <a name="use-a-rest-api-to-reboot-vms"></a>Uso de la API de REST para reiniciar máquinas virtuales

Puede usar la característica **Probar** en el documento de la API para enviar solicitudes a HDInsight. Para la operación de reinicio del nodo se requieren dos pasos: enumerar los nodos y reiniciarlos.

1. Enumerar los nodos. Puede obtener la lista de nodos de clúster en la API REST o en Ambari. Para más información, consulte [Operación de enumeración de host HDInsight de la API de REST](/rest/api/hdinsight/2021-06-01/virtual-machines/list-hosts).

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/listHosts?api-version=2018-06-01-preview
    ```

1. Reiniciar los hosts. Después de obtener los nombres de los nodos que desea reiniciar, reinicie los nodos mediante la API de REST. El nombre del nodo sigue el patrón *NodeType (wn/hn/zk)*  + *x* + *primeros seis caracteres del nombre del clúster*. Para más información, consulte [Operación de reinicio de hosts HDInsight de la API REST](/rest/api/hdinsight/2021-06-01/virtual-machines/restart-hosts).

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/restartHosts?api-version=2018-06-01-preview
    ```

Los nombres reales de los nodos que desea reiniciar se especifican en una matriz JSON en el cuerpo de la solicitud.

```json
[
  "wn0-abcdef",
  "zk1-abcdef"
]
```

## <a name="next-steps"></a>Pasos siguientes

* [Restart-AzHDInsightHost](/powershell/module/az.hdinsight/restart-azhdinsighthost)
* [API de REST de máquinas virtuales de HDInsight](/rest/api/hdinsight/2021-06-01/virtual-machines)
* [API REST de HDInsight](/rest/api/hdinsight/)
