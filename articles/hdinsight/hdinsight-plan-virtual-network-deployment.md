---
title: Planificación de una red virtual para Azure HDInsight
description: Aprenda a planificar la implementación de una red virtual de Azure para conectar HDInsight con otros recursos en la nube o con recursos de su centro de datos.
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,seoapr2020
ms.date: 01/12/2021
ms.openlocfilehash: 0c152521c0762bed400150d1adc285d43ed9f347
ms.sourcegitcommit: a9f131fb59ac8dc2f7b5774de7aae9279d960d74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2021
ms.locfileid: "110191297"
---
# <a name="plan-a-virtual-network-for-azure-hdinsight"></a>Planificación de una red virtual para Azure HDInsight

En este artículo se proporciona información general acerca del uso de [Azure Virtual Networks](../virtual-network/virtual-networks-overview.md) (VNets) con Azure HDInsight. También se explican las decisiones de diseño e implementación que se deben realizar para poder implementar una red virtual para un clúster de HDInsight. Una vez que finalice la fase de planificación, puede pasar a [Creación de redes virtuales para clústeres de Azure HDInsight](hdinsight-create-virtual-network.md). Para más información acerca de las direcciones IP de administración de HDInsight que son necesarias para configurar correctamente los grupos de seguridad de red (NSG) y las rutas definidas por el usuario, consulte [Direcciones IP de administración de HDInsight](hdinsight-management-ip-addresses.md).

El uso de una instancia de Azure Virtual Network permite los siguientes escenarios:

* Conectarse a HDInsight directamente desde una red local.
* Conectar HDInsight a almacenes de datos en una instancia de Azure Virtual Network.
* Obtener acceso directo a los servicios de Apache Hadoop que no están disponibles públicamente en Internet. Por ejemplo, las API de Apache Kafka o la API Apache HBase de Java.

> [!IMPORTANT]
> La creación de un clúster de HDInsight en una red virtual creará varios recursos de red, como NIC y equilibradores de carga. **No** elimine estos recursos de red, ya que son necesarios para que el clúster funcione correctamente con la red virtual.
>
> A partir del 28 de febrero de 2019, los recursos de redes (por ejemplo, NIC, equilibradores de carga, etc) para los NUEVOS clústeres de HDInsight creados en una red virtual se aprovisionarán en el mismo grupo de recursos del clúster de HDInsight. Anteriormente, estos recursos se aprovisionaban en el grupo de recursos de la red virtual. No hay cambios en los clústeres en ejecución actuales y los clústeres creados sin una red virtual.

## <a name="planning"></a>Planificación

Debe responder a las preguntas siguientes cuando planifique instalar HDInsight en una red virtual:

* ¿Necesita instalar HDInsight en una red virtual existente? ¿O bien está creando una red nueva?

    Si está usando una red virtual existente, puede que tenga que modificar la configuración de red antes de poder instalar HDInsight. Para más información, vea la sección [Agregar HDInsight a una red virtual existente](#existingvnet).

* ¿Quiere conectar la red virtual que contiene HDInsight a otra red virtual o a la red local?

    Para trabajar fácilmente con recursos entre redes, puede que tenga que crear un DNS personalizado y configurar el reenvío de DNS. Para más información, vea la sección [Conexión de varias redes](#multinet).

* ¿Quiere restringir o redirigir el tráfico de entrada o salida a HDInsight?

    HDInsight debe tener comunicación sin restricciones con direcciones IP específicas en el centro de datos de Azure. También hay varios puertos que deben permitirse a través de los firewalls para la comunicación del cliente. Para más información, consulte [Control del tráfico de red](./control-network-traffic.md).

## <a name="add-hdinsight-to-an-existing-virtual-network"></a><a id="existingvnet"></a>Agregar HDInsight una red virtual existente

Use los pasos de esta sección para saber cómo agregar un nuevo HDInsight a una instancia de Azure Virtual Network existente.

> [!NOTE]  
> - No se puede agregar un clúster de HDInsight existente a una red virtual.
> - La red virtual y el clúster que se van a crear deben estar en la misma suscripción.

1. ¿Está usando un modelo de implementación clásico o de Resource Manager para la red virtual?

    HDInsight 3.4 y versiones posteriores requieren una red virtual de Resource Manager. Las versiones anteriores de HDInsight requieren una red virtual clásica.

    Si la red existente es una red virtual clásica, debe crear una red virtual de Resource Manager y después conectar las dos. [Conexión de redes virtuales clásicas a redes virtuales nuevas](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

    Una vez combinadas, HDInsight instalado en la red de Resource Manager puede interactuar con los recursos de la red clásica.

2. ¿Usa grupos de seguridad de red, rutas definidas por el usuario o dispositivos de red virtual para restringir el tráfico de entrada y salida de la red virtual?

    Como servicio administrado, HDInsight requiere acceso sin restricciones a varias direcciones IP en el centro de datos de Azure. Para permitir la comunicación con estas direcciones IP, actualice las rutas definidas por el usuario o los grupos de seguridad de red existentes.

    HDInsight hospeda varios servicios, que usan varios puertos. No bloquee el tráfico a estos puertos. Para obtener una lista de puertos que se pueden permitir mediante los firewalls de los dispositivos virtuales, consulte la sección Seguridad.

    Para buscar la configuración de seguridad existente, use los siguientes comandos de Azure PowerShell o de la CLI de Azure:

    * Grupos de seguridad de red

        Reemplace `RESOURCEGROUP` por el nombre del grupo de recursos que contiene la red virtual y, a continuación, escriba el comando:

        ```powershell
        Get-AzNetworkSecurityGroup -ResourceGroupName  "RESOURCEGROUP"
        ```

        ```azurecli
        az network nsg list --resource-group RESOURCEGROUP
        ```

        Para más información, vea el documento sobre [Solución de problemas de los grupos de seguridad de red](../virtual-network/diagnose-network-traffic-filter-problem.md).

        > [!IMPORTANT]  
        > Se aplican reglas de grupo de seguridad de red en orden según la prioridad de la regla. Se aplica la primera regla que coincide con el patrón de tráfico y no se aplica ninguna otra para el tráfico. El orden de las reglas es de más a menos permisiva. Para más información, vea el documento [Filtrar el tráfico de red con grupos de seguridad de red](../virtual-network/network-security-groups-overview.md).

    * Rutas definidas por el usuario

        Reemplace `RESOURCEGROUP` por el nombre del grupo de recursos que contiene la red virtual y, a continuación, escriba el comando:

        ```powershell
        Get-AzRouteTable -ResourceGroupName "RESOURCEGROUP"
        ```

        ```azurecli
        az network route-table list --resource-group RESOURCEGROUP
        ```

        Para más información, vea el documento sobre [Solución de problemas de rutas](../virtual-network/diagnose-network-routing-problem.md).

3. Cree un clúster de HDInsight y seleccione la instancia de Azure Virtual Network durante la configuración. Para entender el proceso de creación de clúster, use los pasos descritos en los siguientes documentos:

    * [Creación de HDInsight con Azure Portal](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [Creación de HDInsight con Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
    * [Creación de clústeres de HDInsight mediante la CLI de Azure clásica](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [Creación de una instancia de HDInsight mediante el uso de una plantilla de Azure Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

   > [!IMPORTANT]  
   > Agregar HDInsight a una red virtual es un paso de configuración opcional. Asegúrese de seleccionar la red virtual al configurar el clúster.

## <a name="connecting-multiple-networks"></a><a id="multinet"></a>Conectar varias redes

El mayor desafío con una configuración de varias redes es la resolución de nombres entre las redes.

Azure proporciona resolución de nombres para los servicios de Azure instalados en una red virtual. Esta resolución de nombres integrada permite conectar HDInsight a los siguientes recursos mediante un nombre de dominio completo (FQDN):

* Cualquier recurso que está disponible en Internet. Por ejemplo, microsoft.com, windowsupdate.com.

* Cualquier recurso que se encuentre en la misma instancia de Azure Virtual Network, mediante el uso del __nombre DNS interno__ del recurso. Por ejemplo, cuando se usa la resolución de nombres predeterminada, los siguientes son ejemplos de nombres DNS internos que se asignan a nodos de trabajo de HDInsight:

  * \<workername1>.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net
  * \<workername2>.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net

    Estos dos nodos pueden comunicarse directamente entre sí y con otros nodos de HDInsight, mediante el uso de nombres DNS internos.

La resolución de nombres predeterminada __no__ permite que HDInsight resuelva los nombres de recursos en redes que se unen a la red virtual. Por ejemplo, es habitual unir la red local a la red virtual. Con solo la resolución de nombres predeterminada, HDInsight no puede acceder a los recursos de la red local por su nombre. Lo contrario también es cierto, los recursos de la red local no pueden acceder a los recursos de la red virtual por su nombre.

> [!WARNING]  
> Debe crear el servidor DNS personalizado y configurar la red virtual para usarla antes de crear el clúster de HDInsight.

Para habilitar la resolución de nombres entre la red virtual y los recursos en redes combinadas, debe realizar las siguientes acciones:

1. Cree un servidor DNS personalizado en la instancia de Azure Virtual Network en la que planifica instalar HDInsight.

2. Configure la red virtual para usar el servidor DNS personalizado.

3. Busque el sufijo DNS que Azure asigna a la red virtual. Este valor es similar a `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`. Para información sobre cómo buscar el sufijo DNS, consulte la sección [Ejemplo: DNS personalizado](hdinsight-create-virtual-network.md#example-dns).

4. Configure el reenvío entre los servidores DNS. La configuración depende del tipo de red remota.

   * Si la red remota es una red local, configure DNS como sigue:

     * __DNS personalizado__ (en la red virtual):

         * Reenvíe las solicitudes para el sufijo DNS de la red virtual a la resolución recursiva de Azure (168.63.129.16). Azure administra las solicitudes de recursos en la red virtual.

         * Reenvíe las demás solicitudes al servidor DNS local. El servidor DNS local administra todas las solicitudes de resolución de nombres, incluso las de recursos de Internet como Microsoft.com.

     * __DNS local__: reenvíe las solicitudes para el sufijo DNS de red virtual al servidor DNS personalizado. El servidor DNS personalizado reenvía a la resolución recursiva de Azure.

       Esta configuración dirige las solicitudes de nombres de dominio completos que contienen el sufijo DNS de la red virtual al servidor DNS personalizado. Todas las demás solicitudes (incluso para las direcciones públicas de Internet) se controlan mediante el servidor DNS local.

   * Si la red remota es otra instancia de Azure Virtual Network, configure DNS como sigue:

     * __DNS personalizado__ (en cada red virtual):

         * Las solicitudes para el sufijo DNS de las redes virtuales se reenvían a los servidores DNS personalizados. El DNS de cada red virtual es el responsable de resolver los recursos dentro de su red.

         * Todas las demás solicitudes se reenvían a la resolución recursiva de Azure. La resolución recursiva es responsable de resolver los recursos locales y de Internet.

       El servidor DNS de cada red reenvía las solicitudes al otro, según el sufijo DNS. Las demás solicitudes se resuelven mediante la resolución recursiva de Azure.

     Para obtener un ejemplo de cada configuración, consulte la sección [Ejemplo: DNS personalizado](hdinsight-create-virtual-network.md#example-dns).

Para más información, vea el documento [Resolución de nombres para máquinas virtuales e instancias de rol](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name="directly-connect-to-apache-hadoop-services"></a>Conexión directa a los servicios de Apache Hadoop

Puede conectarse al clúster en `https://CLUSTERNAME.azurehdinsight.net`. Esta dirección usa una dirección IP pública, que podría no ser accesible en caso de haber usado grupos de seguridad de red para restringir el tráfico entrante de Internet. Además, al implementar el clúster en una red virtual puede acceder a ella mediante el punto de conexión privado `https://CLUSTERNAME-int.azurehdinsight.net`. Este punto de conexión se resuelve en una dirección IP privada dentro de la red virtual para el acceso al clúster.

Para conectarse a Apache Ambari y otras páginas web a través de la red virtual, siga estos pasos:

1. Para detectar los nombres de dominio completos (FQDN) internos de los nodos de clúster de HDInsight, use uno de los métodos siguientes:

    Reemplace `RESOURCEGROUP` por el nombre del grupo de recursos que contiene la red virtual y, a continuación, escriba el comando:

    ```powershell
    $clusterNICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP" | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group RESOURCEGROUP --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

    En la lista de nodos que se devuelve, busque el FQDN de los nodos principales y use los FQDN para conectarse a Ambari y otros servicios web. Por ejemplo, use `http://<headnode-fqdn>:8080` para tener acceso a Ambari.

    > [!IMPORTANT]  
    > Algunos servicios hospedados en los nodos principales solo están activos en un nodo a la vez. Si intenta obtener acceso a un servicio en un nodo principal y se devuelve un error 404, cambie al otro nodo principal.

2. Para determinar el nodo y el puerto en que está disponible un servicio, vea el documento [Puertos utilizados por los servicios Hadoop en HDInsight](./hdinsight-hadoop-port-settings-for-services.md).

## <a name="load-balancing"></a>Equilibrio de carga

Al crear un clúster de HDInsight, también se crea un equilibrador de carga. El tipo de este equilibrador de carga se encuentra en el [nivel de SKU básico](../load-balancer/skus.md), que tiene ciertas restricciones. Una de estas restricciones es que si tiene dos redes virtuales en diferentes regiones, no puede conectarse a equilibradores de carga básicos. Consulte las [P+F sobre redes virtuales: restricciones en el emparejamiento de VNET global](../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers) para obtener más información.

## <a name="next-steps"></a>Pasos siguientes

* Para ver ejemplos de código y ejemplos de creación de redes virtuales de Azure, consulte [Creación de redes virtuales para clústeres de Azure HDInsight](hdinsight-create-virtual-network.md).
* Para obtener un ejemplo integral de la configuración de HDInsight para conectarse a una red local, vea [Conexión de HDInsight a la red local](./connect-on-premises-network.md).
* Para más información sobre las redes virtuales de Azure, vea la [información general sobre Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Para más información sobre los grupos de seguridad de red, vea [Grupos de seguridad de red](../virtual-network/network-security-groups-overview.md).
* Para obtener más información sobre las rutas definidas por el usuario, consulte [Rutas definidas por el usuario y reenvío de IP](../virtual-network/virtual-networks-udr-overview.md).
* Para más información sobre el control del tráfico que incluye la integración de firewall, consulte [Control del tráfico de red](./control-network-traffic.md).
