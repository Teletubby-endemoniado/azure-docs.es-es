---
title: Creación y administración de una máquina virtual de Azure mediante Java
description: Use Java y Azure Resource Manager para implementar una máquina virtual y todos sus recursos de apoyo.
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure
ms.topic: how-to
ms.date: 10/09/2021
ms.custom: devx-track-java
ms.author: cynthn
ms.openlocfilehash: daf1c7738539f225f116edbb839d86965f4e38cd
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130234025"
---
# <a name="create-and-manage-windows-vms-in-azure-using-java"></a>Creación y administración de máquinas virtuales Windows en Azure mediante Java

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Windows 

Las [máquinas virtuales de Azure](overview.md) necesitan varios recursos de Azure compatibles. En este artículo se trata la creación, la administración y la eliminación de recursos de máquina virtual con Java. Aprenderá a:

> [!div class="checklist"]
> * Creación de un proyecto de Maven
> * Adición de dependencias
> * Crear credenciales
> * Crear recursos
> * Realizar tareas de administración
> * Eliminar recursos
> * Ejecución de la aplicación

Tardará unos 20 minutos en realizar estos pasos.

## <a name="create-a-maven-project"></a>Creación de un proyecto de Maven

1. Si aún no lo ha hecho, instale [Java](/azure/developer/java/fundamentals/java-support-on-azure).
2. Instale [Maven](https://maven.apache.org/download.cgi).
3. Cree una nueva carpeta y el proyecto:
    
    ```
    mkdir java-azure-test
    cd java-azure-test
    
    mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

## <a name="add-dependencies"></a>Adición de dependencias

1. En la carpeta `testAzureApp`, abra el archivo `pom.xml` y agregue la configuración de compilación al &lt;proyecto&gt; para habilitar la compilación de la aplicación:

    ```xml
    <build>
      <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <configuration>
                <mainClass>com.fabrikam.testAzureApp.App</mainClass>
            </configuration>
        </plugin>
      </plugins>
    </build>
    ```

2. Agregue las dependencias necesarias para acceder al SDK de Java de Azure.

    ```xml
    <dependency>
      <groupId>com.azure</groupId>
      <artifactId>azure-identity</artifactId>
      <version>1.3.6</version>
    </dependency>
    <dependency>
      <groupId>com.azure.resourcemanager</groupId>
      <artifactId>azure-resourcemanager</artifactId>
      <version>2.8.0</version>
    </dependency>
    ```

3. Guarde el archivo.

## <a name="set-up-authentication"></a>Configuración de la autenticación

Obtenga información sobre cómo [configurar la autenticación](/azure/developer/java/sdk/get-started#set-up-authentication).

## <a name="create-the-management-client"></a>Creación del cliente de administración

1. Abra el archivo `App.java` en `src\main\java\com\fabrikam` y asegúrese de que esta instrucción del paquete esté en la parte superior:

    ```java
    package com.fabrikam.testAzureApp;
    ```

2. Cree AzureResourceManager:
   
    ```java
    TokenCredential credential = new EnvironmentCredentialBuilder()
                .authorityHost(AzureAuthorityHosts.AZURE_PUBLIC_CLOUD)
                .build();

    // Please finish 'Set up authentication' step first to set the four environment variables: AZURE_SUBSCRIPTION_ID, AZURE_CLIENT_ID, AZURE_CLIENT_SECRET, AZURE_TENANT_ID
    AzureProfile profile = new AzureProfile(AzureEnvironment.AZURE);

    AzureResourceManager azureResourceManager = AzureResourceManager.configure()
            .withLogLevel(HttpLogDetailLevel.BASIC)
            .authenticate(credential, profile)
            .withDefaultSubscription();
    ```

## <a name="create-resources"></a>Crear recursos

### <a name="create-the-resource-group"></a>Creación del grupo de recursos

Todos los recursos deben encontrarse en un [grupo de recursos](../../azure-resource-manager/management/overview.md).

Para especificar los valores de la aplicación y crear el grupo de recursos, agregue este código al bloque Try del método Main:

```java
System.out.println("Creating resource group...");
ResourceGroup resourceGroup = azure.resourceGroups()
    .define("myResourceGroup")
    .withRegion(Region.US_EAST)
    .create();
```

### <a name="create-the-availability-set"></a>Creación del conjunto de disponibilidad

Los [conjuntos de disponibilidad](tutorial-availability-sets.md) facilitan el mantenimiento de las máquinas virtuales que utiliza la aplicación.

Para crear el conjunto de disponibilidad, agregue este código al bloque Try del método Main:

```java
System.out.println("Creating availability set...");
AvailabilitySet availabilitySet = azure.availabilitySets()
    .define("myAvailabilitySet")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .create();
```
### <a name="create-the-public-ip-address"></a>Crear la dirección IP pública

Se necesita una [dirección IP pública](../../virtual-network/ip-services/public-ip-addresses.md) para la comunicación con la máquina virtual.

Para crear la dirección IP pública de la máquina virtual, agregue este código al bloque Try del método Main:

```java
System.out.println("Creating public IP address...");
PublicIpAddress publicIPAddress = azure.publicIpAddresses()
    .define("myPublicIP")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withDynamicIP()
    .create();
```

### <a name="create-the-virtual-network"></a>Crear la red virtual

Debe haber una máquina virtual en una subred de una [red virtual](../../virtual-network/virtual-networks-overview.md).

Para crear una subred y una red virtual, agregue este código al bloque Try del método Main:

```java
System.out.println("Creating virtual network...");
Network network = azure.networks()
    .define("myVN")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withAddressSpace("10.0.0.0/16")
    .withSubnet("mySubnet", "10.0.0.0/24")
    .create();
```

### <a name="create-the-network-interface"></a>Creación de la interfaz de red

Una máquina virtual requiere una interfaz de red para comunicarse en la red virtual que acaba de crear.

Para crear una interfaz de red, agregue este código al bloque Try del método Main:

```java
System.out.println("Creating network interface...");
NetworkInterface networkInterface = azure.networkInterfaces()
    .define("myNIC")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetwork(network)
    .withSubnet("mySubnet")
    .withPrimaryPrivateIPAddressDynamic()
    .withExistingPrimaryPublicIPAddress(publicIPAddress)
    .create();
```

### <a name="create-the-virtual-machine"></a>Creación de la máquina virtual

Ahora que ha creado todos los recursos auxiliares, puede crear una máquina virtual.

Para crear la máquina virtual, agregue este código al bloque Try del método Main:

```java
System.out.println("Creating virtual machine...");
VirtualMachine virtualMachine = azure.virtualMachines()
    .define("myVM")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetworkInterface(networkInterface)
    .withLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .withAdminUsername("azureuser")
    .withAdminPassword("Azure12345678")
    .withComputerName("myVM")
    .withExistingAvailabilitySet(availabilitySet)
    .withSize("Standard_DS1")
    .create();
Scanner input = new Scanner(System.in);
System.out.println("Press enter to get information about the VM...");
input.nextLine();
```

> [!NOTE]
> En este tutorial se crea una máquina virtual donde se ejecuta una versión del sistema operativo Windows Server. Para más información sobre cómo seleccionar otras imágenes, consulte [Seleccione y navegue por imágenes de máquina virtual de Azure con PowerShell y la CLI de Azure](../linux/cli-ps-findimage.md).
> 
>

Si desea usar un disco existente en lugar de una imagen de Marketplace, use este código: 

```java
Disk managedDisk = azure.disks().define("myosdisk")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withWindowsFromVhd("https://mystorage.blob.core.windows.net/vhds/myosdisk.vhd")
    .withStorageAccountName("mystorage")
    .withSizeInGB(128)
    .withSku(DiskSkuTypes.PREMIUM_LRS)
    .create();

azure.virtualMachines().define("myVM")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetworkInterface(networkInterface)
    .withSpecializedOSDisk(managedDisk, OperatingSystemTypes.WINDOWS)
    .withExistingAvailabilitySet(availabilitySet)
    .withSize(VirtualMachineSizeTypes.STANDARD_DS1)
    .create();
```

## <a name="perform-management-tasks"></a>Realizar tareas de administración

Durante el ciclo de vida de una máquina virtual, puede ejecutar tareas de administración como iniciar, detener o eliminar una máquina virtual. Además, puede crear código para automatizar tareas repetitivas o complejas.

Cuando tenga que hacerlas con la máquina virtual, deberá obtener una instancia de ella. Agregue este código al bloque Try del método Main:

```java
VirtualMachine vm = azure.virtualMachines().getByResourceGroup("myResourceGroup", "myVM");
```

### <a name="get-information-about-the-vm"></a>Obtención de información acerca de la máquina virtual

Para obtener información sobre la máquina virtual, agregue este código al bloque Try del método Main:

```java
System.out.println("hardwareProfile");
System.out.println("    vmSize: " + vm.size());
System.out.println("storageProfile");
System.out.println("  imageReference");
System.out.println("    publisher: " + vm.storageProfile().imageReference().publisher());
System.out.println("    offer: " + vm.storageProfile().imageReference().offer());
System.out.println("    sku: " + vm.storageProfile().imageReference().sku());
System.out.println("    version: " + vm.storageProfile().imageReference().version());
System.out.println("  osDisk");
System.out.println("    osType: " + vm.storageProfile().osDisk().osType());
System.out.println("    name: " + vm.storageProfile().osDisk().name());
System.out.println("    createOption: " + vm.storageProfile().osDisk().createOption());
System.out.println("    caching: " + vm.storageProfile().osDisk().caching());
System.out.println("osProfile");
System.out.println("    computerName: " + vm.osProfile().computerName());
System.out.println("    adminUserName: " + vm.osProfile().adminUsername());
System.out.println("    provisionVMAgent: " + vm.osProfile().windowsConfiguration().provisionVMAgent());
System.out.println(
        "    enableAutomaticUpdates: " + vm.osProfile().windowsConfiguration().enableAutomaticUpdates());
System.out.println("networkProfile");
System.out.println("    networkInterface: " + vm.primaryNetworkInterfaceId());
System.out.println("vmAgent");
System.out.println("  vmAgentVersion: " + vm.instanceView().vmAgent().vmAgentVersion());
System.out.println("    statuses");
for (InstanceViewStatus status : vm.instanceView().vmAgent().statuses()) {
    System.out.println("    code: " + status.code());
    System.out.println("    displayStatus: " + status.displayStatus());
    System.out.println("    message: " + status.message());
    System.out.println("    time: " + status.time());
}
System.out.println("disks");
for (DiskInstanceView disk : vm.instanceView().disks()) {
    System.out.println("  name: " + disk.name());
    System.out.println("  statuses");
    for (InstanceViewStatus status : disk.statuses()) {
        System.out.println("    code: " + status.code());
        System.out.println("    displayStatus: " + status.displayStatus());
        System.out.println("    time: " + status.time());
    }
}
System.out.println("VM general status");
System.out.println("  provisioningStatus: " + vm.provisioningState());
System.out.println("  id: " + vm.id());
System.out.println("  name: " + vm.name());
System.out.println("  type: " + vm.type());
System.out.println("VM instance status");
for (InstanceViewStatus status : vm.instanceView().statuses()) {
    System.out.println("  code: " + status.code());
    System.out.println("  displayStatus: " + status.displayStatus());
}
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="stop-the-vm"></a>Parada de la máquina virtual

Puede detener una máquina virtual y mantener toda su configuración, pero se le seguirá cobrando, o puede detener una máquina virtual y desasignarla. Cuando se desasigna una máquina virtual, todos los recursos asociados a ella también se desasignan y se le deja de cobrar por ellos.

Para detener la máquina virtual sin desasignarla, agregue este código al bloque Try del método Main:

```java
System.out.println("Stopping vm...");
vm.powerOff();
System.out.println("Press enter to continue...");
input.nextLine();
```

Si desea desasignar la máquina virtual, cambie la llamada de PowerOff por este código:

```java
vm.deallocate();
```

### <a name="start-the-vm"></a>Inicio de la máquina virtual

Para iniciar la máquina virtual, agregue este código al bloque Try del método Main:

```java
System.out.println("Starting vm...");
vm.start();
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="resize-the-vm"></a>Cambio de tamaño de la máquina virtual

Para decidir un tamaño de máquina virtual, se deben considerar muchos aspectos de la implementación. Para más información, consulte el artículo sobre los [tamaños de máquina virtual](../sizes.md).  

Para cambiar el tamaño de la máquina virtual, agregue este código al bloque Try del método Main:

```java
System.out.println("Resizing vm...");
vm.update()
    .withSize(VirtualMachineSizeTypes.STANDARD_DS2)
    .apply();
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="add-a-data-disk-to-the-vm"></a>Incorporación de un disco de datos a la máquina virtual

Para agregar un disco de datos a la máquina virtual, agregue este código al bloque Try del método Main para agregar un disco de datos de 2 GB de tamaño, tener un LUN de 0 y un tipo de almacenamiento en caché de lectura y escritura:

```java
System.out.println("Adding data disk...");
vm.update()
    .withNewDataDisk(2, 0, CachingTypes.READ_WRITE)
    .apply();
System.out.println("Press enter to delete resources...");
input.nextLine();
```

## <a name="delete-resources"></a>Eliminar recursos

Dado que se le cobrará por los recursos utilizados en Azure, siempre es conveniente eliminar los recursos que ya no sean necesarios. Si quiere eliminar las máquinas virtuales y todos los recursos auxiliares, lo único que tiene que hacer es eliminar el grupo de recursos.

1. Para eliminar el grupo de recursos, agregue este código al bloque Try del método Main:
   
    ```java
    System.out.println("Deleting resources...");
    azure.resourceGroups().deleteByName("myResourceGroup");
    ```

2. Guarde el archivo App.java.

## <a name="run-the-application"></a>Ejecución de la aplicación

Esta aplicación de consola tardará unos cinco minutos en ejecutarse completamente de principio a fin.

1. Para ejecutar la aplicación, use este comando de Maven:

    ```
    mvn compile exec:java
    ```

2. Antes de presionar **Entrar** para comenzar la eliminación de recursos, puede dedicar unos minutos a comprobar la creación de los recursos en Azure Portal. Haga clic en el estado de implementación para ver información de la implementación.


## <a name="next-steps"></a>Pasos siguientes
* Para obtener más información sobre cómo usar las [bibliotecas de Azure para Java](/java/azure/java-sdk-azure-overview).