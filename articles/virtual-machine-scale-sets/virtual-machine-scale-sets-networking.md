---
title: Redes para conjuntos de escalado de máquinas virtuales de Azure
description: Cómo configurar algunas de las propiedades de redes más avanzadas para conjuntos de escalado de máquinas virtuales de Azure.
author: ju-shim
ms.author: jushiman
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: networking
ms.date: 06/25/2020
ms.reviewer: mimckitt
ms.custom: mimckitt, devx-track-azurepowershell
ms.openlocfilehash: ea17d86bb4e5e4a6b5c5106c7d831c6691018b12
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130214745"
---
# <a name="networking-for-azure-virtual-machine-scale-sets"></a>Redes para conjuntos de escalado de máquinas virtuales de Azure

**Se aplica a:** :heavy_check_mark: Conjuntos de escalado uniformes

Al implementar el conjunto de escalado de máquinas virtuales de Azure a través del portal, algunas propiedades de red se establecen de forma predeterminada, por ejemplo, una instancia de Azure Load Balancer mediante reglas NAT de entrada. En este artículo se describe cómo utilizar algunas de las características más avanzadas de redes que puede configurar con conjuntos de escalado.

Puede configurar todas las características que se tratan en este artículo usando plantillas de Azure Resource Manager. También se incluyen ejemplos de la CLI de Azure y PowerShell para las características seleccionadas.

## <a name="accelerated-networking"></a>Redes aceleradas
Azure Accelerated Networking mejora el rendimiento de la red al permitir la virtualización de E/S de raíz única (SR-IOV) en una máquina virtual. Para obtener más información sobre el uso de redes acelerada, consulte los temas sobre redes aceleradas para máquinas virtuales [Windows](../virtual-network/create-vm-accelerated-networking-powershell.md) o [Linux](../virtual-network/create-vm-accelerated-networking-cli.md). Para usar las redes aceleradas con conjuntos de escalado, establezca enableAcceleratedNetworking en **true** en los valores de networkInterfaceConfigurations de su conjunto de escalado. Por ejemplo:

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
      "name": "niconfig1",
      "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
          ...
        ]
      }
    }
   ]
}
```

## <a name="azure-virtual-machine-scale-sets-with-azure-load-balancer"></a>Conjuntos de escalado de máquinas virtuales de Azure con Azure Load Balancer
Consulte [Azure Load Balancer y Virtual Machine Scale Sets](../load-balancer/load-balancer-standard-virtual-machine-scale-sets.md) para más información sobre cómo configurar Standard Load Balancer con Virtual Machine Scale Sets según su escenario.

## <a name="create-a-scale-set-that-references-an-application-gateway"></a>Creación de un conjunto de escalado que hace referencia a una puerta de enlace de aplicaciones
Para crear un conjunto de escalado que usa una puerta de enlace de aplicaciones, haga referencia al grupo de direcciones de back-end de la puerta de enlace de aplicaciones en la sección ipConfigurations del conjunto de escalado, como en esta configuración de plantilla ARM:

```json
"ipConfigurations": [{
  "name": "{config-name}",
  "properties": {
  "subnet": {
    "id": "{subnet-id}"
  },
  "ApplicationGatewayBackendAddressPools": [{
    "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/applicationGateways/{gateway-name}/backendAddressPools/{pool-name}"
  }]
}]
```

>[!NOTE]
> Tenga en cuenta que la puerta de enlace de aplicaciones debe estar en la misma red virtual que el conjunto de escalado, pero en una subred diferente a la de este.


## <a name="configurable-dns-settings"></a>Valores de DNS configurables
De forma predeterminada, los conjuntos de escalado adoptan los valores de DNS específicos de la red virtual y la subred en las que se crearon. No obstante, puede configurar los valores de DNS para un conjunto de escalado directamente.

### <a name="creating-a-scale-set-with-configurable-dns-servers"></a>Creación de un conjunto de escalado con servidores DNS configurables
Para crear un conjunto de escalado con una configuración de DNS personalizada mediante la CLI de Azure, agregue el argumento **--dns-servers** al comando **vmss create**, seguido por las direcciones ip del servidor separadas por espacio. Por ejemplo:

```bash
--dns-servers 10.0.0.6 10.0.0.5
```

Para configurar servidores DNS personalizados en una plantilla de Azure, agregue una propiedad dnsSettings a la sección de networkInterfaceConfigurations del conjunto de escalado. Por ejemplo:

```json
"dnsSettings":{
    "dnsServers":["10.0.0.6", "10.0.0.5"]
}
```

### <a name="creating-a-scale-set-with-configurable-virtual-machine-domain-names"></a>Creación de un conjunto de escalado con nombres de dominio de máquinas virtuales configurables
Para crear un conjunto de escalado con un nombre DNS personalizado para máquinas virtuales mediante la CLI, agregue el argumento **--vm-domain-name** al comando **virtual machine scale set create**, seguido por una cadena que representa el nombre de dominio.

Para establecer el nombre de dominio en una plantilla de Azure, agregue una propiedad **dnsSettings** a la sección de **networkInterfaceConfigurations** del conjunto de escalado. Por ejemplo:

```json
"networkProfile": {
  "networkInterfaceConfigurations": [
    {
    "name": "nic1",
    "properties": {
      "primary": true,
      "ipConfigurations": [
      {
        "name": "ip1",
        "properties": {
          "subnet": {
            "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
          },
          "publicIPAddressconfiguration": {
            "name": "publicip",
            "properties": {
            "idleTimeoutInMinutes": 10,
              "dnsSettings": {
                "domainNameLabel": "[parameters('vmssDnsName')]"
              }
            }
          }
        }
      }
    ]
    }
}
```

El resultado, para un nombre DNS de máquina virtual individual, tendría la forma siguiente:

```output
<vm><vmindex>.<specifiedVmssDomainNameLabel>
```

## <a name="public-ipv4-per-virtual-machine"></a>Dirección IPv4 pública por máquina virtual
En general, las máquinas virtuales del conjunto de escalado de Azure no requieren direcciones IP públicas propias. En la mayoría de los escenarios, resulta más económico y seguro asociar una dirección IP pública a un equilibrador de carga o a una máquina virtual individual (también conocida como jumpbox), que luego enruta las conexiones entrantes a máquinas virtuales del conjunto de escalado según sea necesario (por ejemplo, a través de reglas NAT de entrada).

Sin embargo, algunos escenarios requieren que las máquinas virtuales del conjunto de escalado tengan sus propias direcciones IP públicas. Un ejemplo son los juegos, en los que se necesita una consola para tener una conexión directa a una máquina virtual en la nube, que esté realizando el procesamiento físico de los juegos. Otro ejemplo es cuando las máquinas virtuales necesitan realizar conexiones externas entre sí a través de regiones en una base de datos distribuida.

### <a name="creating-a-scale-set-with-public-ip-per-virtual-machine"></a>Creación de un conjunto de escalado con una dirección IP pública por máquina virtual
Para crear un conjunto de escalado que asigne una dirección IP pública a cada máquina virtual con la CLI, agregue el parámetro **--public-ip-per-vm** al comando **vmss create**.

Para crear un conjunto de escalado mediante una plantilla de Azure, asegúrese de que la versión de API del recurso Microsoft.Compute/virtualMachineScaleSets sea al menos **2017-03-30** y agregue una propiedad JSON **publicIpAddressConfiguration** a la sección ipConfigurations del conjunto de escalado. Por ejemplo:

```json
"publicIpAddressConfiguration": {
    "name": "pub1",
    "properties": {
      "idleTimeoutInMinutes": 15
    }
}
```

Ejemplo de plantilla: [vmss-public-ip-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.compute/vmss-public-ip-linux)

### <a name="querying-the-public-ip-addresses-of-the-virtual-machines-in-a-scale-set"></a>Consulta de las direcciones IP públicas de las máquinas virtuales en un conjunto de escalado
Para enumerar las direcciones IP públicas asignadas para escalar máquinas virtuales del conjunto mediante la CLI, use el comando **az vmss list-instance-public-ips**.

Para mostrar las direcciones IP públicas del conjunto de escalado con PowerShell, use el comando _Get-AzPublicIpAddress_. Por ejemplo:

```powershell
Get-AzPublicIpAddress -ResourceGroupName myrg -VirtualMachineScaleSetName myvmss
```

También puede consultar las direcciones IP públicas haciendo referencia directamente al identificador de recurso de la configuración de la dirección IP pública. Por ejemplo:

```powershell
Get-AzPublicIpAddress -ResourceGroupName myrg -Name myvmsspip
```

También puede mostrar las direcciones IP públicas asignadas a las máquinas virtuales del conjunto de escalado mediante una consulta a [Azure Resource Explorer](https://resources.azure.com) o a la API REST de Azure con la versión **2017-03-30** o superior.

Para realizar la consulta en [Azure Resource Explorer](https://resources.azure.com):

1. Abra [Azure Resource Explorer](https://resources.azure.com) en una ventana del explorador.
1. Para expandir las *suscripciones* del lado izquierdo, haga clic en el signo *+* situado junto a estas. Si solo tiene un elemento en *suscripciones*, puede que ya esté expandido.
1. Expanda la suscripción.
1. Expanda el grupo de recursos.
1. Expanda los *proveedores*.
1. Expanda *Microsoft.Compute*.
1. Expanda *virtualMachineScaleSets*.
1. Expanda el conjunto de escalado.
1. Haga clic en *publicipaddresses*.

Para consultar la API REST de Azure:

```bash
GET https://management.azure.com/subscriptions/{your sub ID}/resourceGroups/{RG name}/providers/Microsoft.Compute/virtualMachineScaleSets/{scale set name}/publicipaddresses?api-version=2017-03-30
```

Ejemplo de salida de [Azure Resource Explorer](https://resources.azure.com) y API REST de Azure:

```json
{
  "value": [
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/pipvmss/virtualMachines/0/networkInterfaces/pipvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"a64060d5-4dea-4379-a11d-b23cd49a3c8d\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "ee8cb20f-af8e-4cd6-892f-441ae2bf701f",
        "ipAddress": "13.84.190.11",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/0/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    },
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"5f6ff30c-a24c-4818-883c-61ebd5f9eee8\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "036ce266-403f-41bd-8578-d446d7397c2f",
        "ipAddress": "13.84.159.176",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    }
```

## <a name="multiple-ip-addresses-per-nic"></a>Varias direcciones IP por NIC
Cada NIC asociada a una VM en un conjunto de escalado puede tener una o varias configuraciones de IP asociadas con ella. Se asigna a cada configuración una dirección IP privada. Cada configuración también puede tener un recurso de dirección IP pública asociado con ella. Para saber cuántas direcciones IP se pueden asignar a una NIC y cuántas direcciones IP públicas se pueden usar en una suscripción de Azure, consulte los [límites de Azure](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="multiple-nics-per-virtual-machine"></a>Varias NIC por máquina virtual
Puede tener hasta 8 NIC por máquina virtual, según el tamaño de la máquina. El número máximo de NIC por máquina está disponible en el artículo sobre [Tamaños de las máquinas virtuales](../virtual-machines/sizes.md). Todas las NIC conectadas a una instancia de máquina virtual deben conectarse a la misma red virtual. Las NIC se pueden conectar a distintas subredes, pero todas las subredes deben formar parte de la misma red virtual.

El ejemplo siguiente es un perfil de red del conjunto de escalado que muestra varias entradas NIC y varias direcciones IP públicas por máquina virtual:

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
        "name": "nic1",
        "properties": {
            "primary": true,
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        },
        {
        "name": "nic2",
        "properties": {
            "primary": false,
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        }
    ]
}
```

## <a name="nsg--asgs-per-scale-set"></a>NSG y ASG por conjunto de escalado
Los [grupos de seguridad de red](../virtual-network/network-security-groups-overview.md) le permiten filtrar el tráfico hacia los recursos de una red virtual de Azure y desde esta mediante reglas de seguridad. Los [grupos de seguridad de la aplicación](../virtual-network/network-security-groups-overview.md#application-security-groups) le permiten administrar la seguridad de red de los recursos de Azure y agruparlos como una extensión de la estructura de la aplicación.

Los grupos de seguridad de red se pueden aplicar directamente a un conjunto de escalado agregando una referencia a la sección de configuración de la interfaz de red de las propiedades de máquinas virtuales del conjunto de escalado.

Los grupos de seguridad de red también se pueden especificar directamente para un conjunto de escalado agregando una referencia a la sección de configuración IP de la interfaz de red de las propiedades de máquinas virtuales del conjunto de escalado.

Por ejemplo:

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": true,
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            },
                            "applicationSecurityGroups": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/applicationSecurityGroups/', variables('asgName'))]"
                                }
                            ],
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

Para comprobar que el grupo de seguridad de red está asociado con el conjunto de escalado, use el comando `az vmss show`. El siguiente ejemplo usa `--query` para filtrar los resultados y que solo aparezca la sección pertinente de la salida.

```azurecli
az vmss show \
    -g myResourceGroup \
    -n myScaleSet \
    --query virtualMachineProfile.networkProfile.networkInterfaceConfigurations[].networkSecurityGroup

[
  {
    "id": "/subscriptions/.../resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/nsgName",
    "resourceGroup": "myResourceGroup"
  }
]
```

Para comprobar que el grupo de seguridad de la aplicación está asociado con el conjunto de escalado, use el comando `az vmss show`. El siguiente ejemplo usa `--query` para filtrar los resultados y que solo aparezca la sección pertinente de la salida.

```azurecli
az vmss show \
    -g myResourceGroup \
    -n myScaleSet \
    --query virtualMachineProfile.networkProfile.networkInterfaceConfigurations[].ipConfigurations[].applicationSecurityGroups

[
  [
    {
      "id": "/subscriptions/.../resourceGroups/myResourceGroup/providers/Microsoft.Network/applicationSecurityGroups/asgName",
      "resourceGroup": "myResourceGroup"
    }
  ]
]
```

## <a name="make-networking-updates-to-specific-instances"></a>Realización de actualizaciones de red en instancias específicas

Puede realizar actualizaciones de red en instancias específicas del conjunto de escalado de máquinas virtuales.

Puede `PUT` en la instancia para actualizar la configuración de red. Se puede usar para realizar acciones como agregar o quitar tarjetas de interfaz de red (NIC) o quitar una instancia de un grupo de back-end.

```
PUT https://management.azure.com/subscriptions/.../resourceGroups/vmssnic/providers/Microsoft.Compute/virtualMachineScaleSets/vmssnic/virtualMachines/1/?api-version=2019-07-01
```

En el ejemplo siguiente se muestra cómo agregar una segunda configuración de IP a la NIC.

1. `GET`: detalles de una instancia específica del conjunto de escalado de máquinas virtuales.

    ```
    GET https://management.azure.com/subscriptions/.../resourceGroups/vmssnic/providers/Microsoft.Compute/virtualMachineScaleSets/vmssnic/virtualMachines/1/?api-version=2019-07-01
    ```

    *Lo siguiente se ha simplificado para mostrar solo los parámetros de red para este ejemplo.*

    ```json
    {
      ...
      "properties": {
        ...
        "networkProfileConfiguration": {
          "networkInterfaceConfigurations": [
            {
              "name": "vmssnic-vnet-nic01",
              "properties": {
                "primary": true,
                "enableAcceleratedNetworking": false,
                "networkSecurityGroup": {
                  "id": "/subscriptions/123a1a12-a123-1ab1-12a1-12a1a1234ab1/resourceGroups/vmssnic/providers/Microsoft.Network/networkSecurityGroups/basicNsgvmssnic-vnet-nic01"
                },
                "dnsSettings": {
                  "dnsServers": []
                },
                "enableIPForwarding": false,
                "ipConfigurations": [
                  {
                    "name": "vmssnic-vnet-nic01-defaultIpConfiguration",
                    "properties": {
                      "publicIPAddressConfiguration": {
                        "name": "publicIp-vmssnic-vnet-nic01",
                        "properties": {
                          "idleTimeoutInMinutes": 15,
                          "ipTags": [],
                          "publicIPAddressVersion": "IPv4"
                        }
                      },
                      "primary": true,
                      "subnet": {
                        "id": "/subscriptions/123a1a12-a123-1ab1-12a1-12a1a1234ab1/resourceGroups/vmssnic/providers/Microsoft.Network/virtualNetworks/vmssnic-vnet/subnets/default"
                      },
                      "privateIPAddressVersion": "IPv4"
                    }
                  }
                ]
              }
            }
          ]
        },
        ...
      }
    }
    ```

2. `PUT` en la instancia de, actualizando para agregar la configuración de IP adicional. Esto es similar para agregar `networkInterfaceConfiguration` adicionales.


    ```
    PUT https://management.azure.com/subscriptions/.../resourceGroups/vmssnic/providers/Microsoft.Compute/virtualMachineScaleSets/vmssnic/virtualMachines/1/?api-version=2019-07-01
    ```

    *Lo siguiente se ha simplificado para mostrar solo los parámetros de red para este ejemplo.*

    ```json
      {
      ...
      "properties": {
        ...
        "networkProfileConfiguration": {
          "networkInterfaceConfigurations": [
            {
              "name": "vmssnic-vnet-nic01",
              "properties": {
                "primary": true,
                "enableAcceleratedNetworking": false,
                "networkSecurityGroup": {
                  "id": "/subscriptions/123a1a12-a123-1ab1-12a1-12a1a1234ab1/resourceGroups/vmssnic/providers/Microsoft.Network/networkSecurityGroups/basicNsgvmssnic-vnet-nic01"
                },
                "dnsSettings": {
                  "dnsServers": []
                },
                "enableIPForwarding": false,
                "ipConfigurations": [
                  {
                    "name": "vmssnic-vnet-nic01-defaultIpConfiguration",
                    "properties": {
                      "publicIPAddressConfiguration": {
                        "name": "publicIp-vmssnic-vnet-nic01",
                        "properties": {
                          "idleTimeoutInMinutes": 15,
                          "ipTags": [],
                          "publicIPAddressVersion": "IPv4"
                        }
                      },
                      "primary": true,
                      "subnet": {
                        "id": "/subscriptions/123a1a12-a123-1ab1-12a1-12a1a1234ab1/resourceGroups/vmssnic/providers/Microsoft.Network/virtualNetworks/vmssnic-vnet/subnets/default"
                      },
                      "privateIPAddressVersion": "IPv4"
                    }
                  },
                  {
                    "name": "my-second-config",
                    "properties": {
                      "subnet": {
                        "id": "/subscriptions/123a1a12-a123-1ab1-12a1-12a1a1234ab1/resourceGroups/vmssnic/providers/Microsoft.Network/virtualNetworks/vmssnic-vnet/subnets/default"
                      },
                      "privateIPAddressVersion": "IPv4"
                    }
                  }
                ]
              }
            }
          ]
        },
        ...
      }
    }
    ```


## <a name="explicit-network-outbound-connectivity-for-flexible-scale-sets"></a>Conectividad de salida de red explícita para conjuntos de escalado flexibles 

Para mejorar la seguridad de red predeterminada, los [conjuntos de escalado de máquinas virtuales con orquestación flexible](..\virtual-machines\flexible-virtual-machine-scale-sets.md) exigen que las instancias creadas de manera implícita por medio del perfil de escalado automático tengan conectividad de salida definida de forma explícita gracias a alguno de los métodos siguientes: 

- En la mayoría de los escenarios, se recomienda [NAT Gateway adjunto a la subred](../virtual-network/nat-gateway/tutorial-create-nat-gateway-portal.md).
- En escenarios con requisitos de seguridad elevados, o si se usa Azure Firewall o Network Virtual Appliance (NVA), puede especificar una ruta definida por el usuario personalizada como próximo salto a través del firewall. 
- Las instancias están en el grupo de back-end de una instancia de Azure Load Balancer de SKU estándar. 
- Adjunte una dirección IP pública a la interfaz de red de la instancia. 

Con máquinas virtuales de instancia única y conjuntos de escalado de máquinas virtuales con orquestación uniforme, la conectividad de salida se proporciona automáticamente. 

Entre los escenarios comunes que requieren conectividad de salida explícita se incluyen: 

- La activación de una máquina virtual Windows requiere que se haya definido la conectividad de salida desde la instancia de máquina virtual al Servicio de administración de claves (KMS) de activación de Windows. Vea [Solución de problemas de activación de máquinas virtuales Windows](/troubleshoot/azure/virtual-machines/troubleshoot-activation-problems) para obtener más información.  
- Acceda a cuentas de almacenamiento o Key Vault. La conectividad con los servicios de Azure también se puede establecer por medio de [Private Link](../private-link/private-link-overview.md). 

Vea [Acceso de salida predeterminado en Azure](../virtual-network/ip-services/default-outbound-access.md) para obtener más detalles sobre cómo definir conexiones de salida seguras.


## <a name="next-steps"></a>Pasos siguientes
Para más información acerca de redes virtuales, consulte la [Introducción a Azure Virtual Network](../virtual-network/virtual-networks-overview.md).