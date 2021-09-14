---
title: 'Administración de certificados de dispositivo: Azure IoT Edge | Microsoft Docs'
description: Cree certificados de prueba, instálelos y adminístrelos en un dispositivo Azure IoT Edge para preparar la implementación de producción.
author: kgremban
ms.author: kgremban
ms.date: 08/24/2021
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 3866217f0aa90cd3450d0f74e35eaa68cea3c559
ms.sourcegitcommit: 7854045df93e28949e79765a638ec86f83d28ebc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2021
ms.locfileid: "122866549"
---
# <a name="manage-certificates-on-an-iot-edge-device"></a>Administración de certificados en un dispositivo IoT Edge

[!INCLUDE [iot-edge-version-201806-or-202011](../../includes/iot-edge-version-201806-or-202011.md)]

Todos los dispositivos IoT Edge usan certificados para crear conexiones seguras entre el entorno de ejecución y los módulos que se ejecutan en el dispositivo. Los dispositivos IoT Edge que funcionan como puertas de enlace usan estos mismos certificados para conectarse también a sus dispositivos de nivel inferior.

## <a name="install-production-certificates"></a>Instalar certificados de producción

La primera vez que instala IoT Edge y aprovisiona el dispositivo, el dispositivo se configura con certificados temporales para que pueda probar el servicio.
Estos certificados temporales expiran en 90 días, o bien se puede reiniciar el equipo para restablecerlos.
Una vez realizada la migración a un escenario de producción, o si quiere crear un dispositivo de puerta de enlace, tendrá que proporcionar certificados propios.
En este artículo se muestran los pasos para instalar certificados en los dispositivos IoT Edge.

Para más información sobre los diferentes tipos de certificados y sus roles, vea [Información sobre los certificados de Azure IoT Edge](iot-edge-certs.md).

>[!NOTE]
>El término "entidad de certificación raíz" que se usa en este artículo hace referencia al certificado público de la entidad de nivel superior de la cadena de certificados de la solución de IoT. No es necesario usar la raíz del certificado de una entidad de certificación sindicada o la raíz de la entidad de certificación de la organización. En muchos casos, se trata realmente de un certificado público intermedio de la entidad de certificación.

### <a name="prerequisites"></a>Requisitos previos

* Un dispositivo IoT Edge.

  Si no tiene un dispositivo IoT Edge configurado, puede crear uno en una máquina virtual de Azure. Siga los pasos de alguno de los artículos de inicio rápido para [Crear un dispositivo virtual Linux](quickstart-linux.md) o [Crear un dispositivo virtual Windows](quickstart.md).

* Tener un certificado de entidad de certificación (CA) raíz, ya sea autofirmado o comprado a través de una entidad de certificación comercial de confianza como Baltimore, Verisign, DigiCert o GlobalSign.

  Si todavía no tiene una entidad de certificación raíz pero quiere probar características de IoT Edge que requieren certificados de producción (por ejemplo, escenarios de puerta de enlace), puede [crear certificados de demostración para probar las características del dispositivo IoT Edge](how-to-create-test-certificates.md).

### <a name="create-production-certificates"></a>Creación de certificados de producción

Debe usar una entidad de certificación propia para crear los archivos siguientes:

* Entidad de certificación raíz
* Certificado de entidad de certificación de dispositivo
* Clave privada de entidad de certificación del dispositivo

En este artículo, lo que se denomina *entidad de certificación raíz* no es la entidad de certificación de nivel superior de una organización. Es la entidad de certificación de nivel superior para el escenario de IoT Edge, que el módulo de IoT Edge Hub, los módulos de usuario y los dispositivos de nivel inferior usan para establecer la confianza entre sí.

<!-- 1.1 -->
:::moniker range="iotedge-2018-06"

> [!NOTE]
> Actualmente, una limitación en libiothsm impide el uso de certificados que expiran el 1 de enero de 2038 o en una fecha posterior.

:::moniker-end

Para ver un ejemplo de estos certificados, revise los scripts que crean los certificados de demostración en [Administración de certificados de entidad de certificación de prueba para ejemplos y tutoriales](https://github.com/Azure/iotedge/tree/master/tools/CACertificates).

### <a name="install-certificates-on-the-device"></a>Instalación de certificados en el dispositivo

Instale la cadena de certificados en el dispositivo IoT Edge y configure el entorno de ejecución de IoT Edge para que haga referencia a los nuevos certificados.

Copie los tres archivos de certificado y clave en el dispositivo IoT Edge. 

Si ha usado los scripts de ejemplo para [crear certificados de demostración](how-to-create-test-certificates.md), los tres archivos de certificado y clave se encuentran en las rutas de acceso siguientes:

* Certificado de entidad de certificación del dispositivo: `<WRKDIR>\certs\iot-edge-device-MyEdgeDeviceCA-full-chain.cert.pem`
* Clave privada de entidad de certificación del dispositivo: `<WRKDIR>\private\iot-edge-device-MyEdgeDeviceCA.key.pem`
* Entidad de certificación raíz: `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

Puede usar un servicio como [Azure Key Vault](../key-vault/index.yml) o una función como [Protocolo de copia segura](https://www.ssh.com/ssh/scp/) para mover los archivos de certificado. Si ha generado los certificados en el propio dispositivo IoT Edge, puede omitir este paso y usar la ruta de acceso al directorio de trabajo.

Si usa IoT Edge para Linux en Windows, debe utilizar la clave SSH que se encuentra en el archivo `id_rsa` de Azure IOT Edge para autenticar las transferencias de archivos entre el sistema operativo host y la máquina virtual Linux. Recupere la dirección IP de la máquina virtual Linux mediante el comando `Get-EflowVmAddr`. Luego, puede realizar un SCP autenticado con el siguiente comando:

   ```powershell
   C:\WINDOWS\System32\OpenSSH\scp.exe -i 'C:\Program Files\Azure IoT Edge\id_rsa' <PATH_TO_SOURCE_FILE> iotedge-user@<VM_IP>:<PATH_TO_FILE_DESTINATION>
   ```

### <a name="configure-iot-edge-with-the-new-certificates"></a>Configuración de IoT Edge con los nuevos certificados

<!-- 1.1 -->
:::moniker range="iotedge-2018-06"

# <a name="linux-containers"></a>[Contenedores de Linux](#tab/linux)

1. Abra el archivo de configuración del demonio de seguridad de IoT Edge: `/etc/iotedge/config.yaml`

1. Establezca las propiedades de **certificado** de config.yaml en la ruta de acceso del URI de archivo de los archivos de certificado y de clave del dispositivo IoT Edge. Elimine el carácter `#` antes de las propiedades de certificado para quitar la marca de comentario de las cuatro líneas. Asegúrese de que la línea **certificates:** no tiene ningún espacio en blanco delante y de que los elementos anidados muestran una sangría de dos espacios. Por ejemplo:

   ```yaml
   certificates:
      device_ca_cert: "file:///<path>/<device CA cert>"
      device_ca_pk: "file:///<path>/<device CA key>"
      trusted_ca_certs: "file:///<path>/<root CA cert>"
   ```

1. Asegúrese de que el usuario **iotedge** tiene permisos de lectura para el directorio que contiene los certificados.

1. Si anteriormente ha usado cualquier otro certificado para IoT Edge en el dispositivo, elimine los archivos de los dos directorios siguientes antes de iniciar o reiniciar IoT Edge:

   * `/var/lib/iotedge/hsm/certs`
   * `/var/lib/iotedge/hsm/cert_keys`

1. Reinicie IoT Edge.

   ```bash
   sudo iotedge system restart
   ```

# <a name="windows-containers"></a>[Contenedores de Windows](#tab/windows)

1. Abra el archivo de configuración del demonio de seguridad de IoT Edge: `C:\ProgramData\iotedge\config.yaml`

1. Establezca las propiedades de **certificado** de config.yaml en la ruta de acceso del URI de archivo de los archivos de certificado y de clave del dispositivo IoT Edge. Elimine el carácter `#` antes de las propiedades de certificado para quitar la marca de comentario de las cuatro líneas. Asegúrese de que la línea **certificates:** no tiene ningún espacio en blanco delante y de que los elementos anidados muestran una sangría de dos espacios. Por ejemplo:

   ```yaml
   certificates:
      device_ca_cert: "file:///C:/<path>/<device CA cert>"
      device_ca_pk: "file:///C:/<path>/<device CA key>"
      trusted_ca_certs: "file:///C:/<path>/<root CA cert>"
   ```

1. Si anteriormente ha usado cualquier otro certificado para IoT Edge en el dispositivo, elimine los archivos de los dos directorios siguientes antes de iniciar o reiniciar IoT Edge:

   * `C:\ProgramData\iotedge\hsm\certs`
   * `C:\ProgramData\iotedge\hsm\cert_keys`

1. Reinicie IoT Edge.

   ```powershell
   Restart-Service iotedge
   ```
---

:::moniker-end
<!-- end 1.1 -->

<!-- 1.2 -->
:::moniker range=">=iotedge-2020-11"

1. Abra el archivo de configuración del demonio de seguridad de IoT Edge: `/etc/aziot/config.toml`

1. Busque el parámetro `trust_bundle_cert` al principio del archivo. Quite la marca de comentario de esta línea y proporcione el URI del archivo al certificado de la CA raíz en el dispositivo.

   ```toml
   trust_bundle_cert = "file:///<path>/<root CA cert>"
   ```

1. Busque la sección `[edge_ca]` en el archivo config.toml. Quite las marcas de comentario de las líneas de esta sección y proporcione las rutas de acceso del URI de archivo para los archivos de certificado y clave en el dispositivo IoT Edge.

   ```toml
   [edge_ca]
   cert = "file:///<path>/<device CA cert>"
   pk = "file:///<path>/<device CA key>"
   ```

1. Asegúrese de que el servicio tenga permisos de lectura para los directorios que contiene los certificados y las claves.

   * El archivo de clave privada debe ser propiedad del grupo **aziotks**.
   * Los archivos de certificado deben ser propiedad del grupo **aziotcs**.

   >[!TIP]
   >Si el certificado es de solo lectura, lo que significa que lo ha creado y no quiere que el servicio IoT Edge lo rote, establezca el archivo de clave privada en el modo 0440 y el archivo de certificado en el modo 0444. Si ha creado los archivos iniciales y luego ha configurado el servicio de certificado para rotar el certificado en el futuro, establezca el archivo de clave privada en el modo 0660 y el archivo de certificado en el modo 0664.

1. Si anteriormente ha usado cualquier otro certificado para IoT Edge en el dispositivo, elimine los archivos del directorio siguiente. IoT Edge los vuelve a crear con el nuevo certificado de CA que ha proporcionado.

   * `/var/lib/aziot/certd/certs`

1. Aplique los cambios en la configuración.

   ```bash
   sudo iotedge config apply
   ```

:::moniker-end
<!-- end 1.2 -->

## <a name="customize-certificate-lifetime"></a>Personalizar vigencia del certificado

IoT Edge genera automáticamente certificados en el dispositivo en varios casos, entre los que se incluyen:

* Si no proporciona sus propios certificados de producción al instalar y aprovisionar IoT Edge, el administrador de seguridad de IoT Edge genera automáticamente un **certificado de CA del dispositivo**. Este certificado autofirmado está concebido únicamente para escenarios de desarrollo y pruebas, no para producción. Este certificado expira después de 90 días.
* El administrador de seguridad de IoT Edge también genera un **certificado de CA de carga de trabajo** firmado por el certificado de CA del dispositivo

Para obtener más información sobre la función de los distintos certificados en un dispositivo IoT Edge, consulte [Información sobre los certificados de Azure IoT Edge](iot-edge-certs.md).

En el caso de estos dos certificados generados automáticamente, tiene la opción de establecer una marca en el archivo de configuración para configurar el número de días para la vigencia de los certificados.

>[!NOTE]
>Hay un tercer certificado generado automáticamente que crea el administrador de seguridad de IoT Edge, el **certificado de servidor del centro de IoT Edge**. Este certificado siempre tiene una vigencia de 90 días, pero siempre se renueva automáticamente antes de expirar. El valor de duración de CA generado automáticamente establecido en el archivo de configuración no afecta a este certificado.

Al expirar una vez transcurrido el número de días especificado, IoT Edge debe reiniciarse para volver a generar el certificado de CA del dispositivo. El certificado de CA del dispositivo no se renovará automáticamente.

<!-- 1.1. -->
:::moniker range="iotedge-2018-06"

# <a name="linux-containers"></a>[Contenedores de Linux](#tab/linux)

1. Para configurar la expiración de certificados en un valor distinto a los 90 días predeterminados, agregue el valor en días a la sección **certificates** del archivo de configuración.

   ```yaml
   certificates:
     device_ca_cert: "<ADD URI TO DEVICE CA CERTIFICATE HERE>"
     device_ca_pk: "<ADD URI TO DEVICE CA PRIVATE KEY HERE>"
     trusted_ca_certs: "<ADD URI TO TRUSTED CA CERTIFICATES HERE>"
     auto_generated_ca_lifetime_days: <value>
   ```

   > [!NOTE]
   > Actualmente, una limitación en libiothsm impide el uso de certificados que expiran el 1 de enero de 2038 o en una fecha posterior.

1. Elimine el contenido de la carpeta `hsm` para quitar los certificados generados previamente.

   * `/var/lib/iotedge/hsm/certs`
   * `/var/lib/iotedge/hsm/cert_keys`

1. Reinicie el servicio IoT Edge.

   ```bash
   sudo systemctl restart iotedge
   ```

1. Confirme el valor de vigencia.

   ```bash
   sudo iotedge check --verbose
   ```

   Compruebe la salida de la comprobación **preparación de producción: certificados**, que muestra el número de días hasta la expiración de los certificados de CA del dispositivo generados automáticamente.

# <a name="windows-containers"></a>[Contenedores de Windows](#tab/windows)

1. Para configurar la expiración de certificados en un valor distinto a los 90 días predeterminados, agregue el valor en días a la sección **certificates** del archivo de configuración.

   ```yaml
   certificates:
     device_ca_cert: "<ADD URI TO DEVICE CA CERTIFICATE HERE>"
     device_ca_pk: "<ADD URI TO DEVICE CA PRIVATE KEY HERE>"
     trusted_ca_certs: "<ADD URI TO TRUSTED CA CERTIFICATES HERE>"
     auto_generated_ca_lifetime_days: <value>
   ```

   > [!NOTE]
   > Actualmente, una limitación en libiothsm impide el uso de certificados que expiran el 1 de enero de 2038 o en una fecha posterior.

1. Elimine el contenido de la carpeta `hsm` para quitar los certificados generados previamente.

   * `C:\ProgramData\iotedge\hsm\certs`
   * `C:\ProgramData\iotedge\hsm\cert_keys`

1. Reinicie el servicio IoT Edge.

   ```powershell
   Restart-Service iotedge
   ```

1. Confirme el valor de vigencia.

   ```powershell
   iotedge check --verbose
   ```

   Compruebe la salida de la comprobación **preparación de producción: certificados**, que muestra el número de días hasta la expiración de los certificados de CA del dispositivo generados automáticamente.

---

:::moniker-end
<!-- end 1.1 -->

<!-- 1.2 -->
:::moniker range=">=iotedge-2020-11"

1. Para configurar la expiración del certificado en un valor distinto a los 90 días predeterminados, agregue el valor en días a la sección **Certificado de CA de Edge (inicio rápido)** del archivo de configuración.

   ```toml
   [edge_ca]
   auto_generated_edge_ca_expiry_days = <value>
   ```

1. Elimine el contenido de las carpetas `certd` y `keyd` para quitar los certificados generados previamente: `/var/lib/aziot/certd/certs` `/var/lib/aziot/keyd/keys`

1. Aplique los cambios en la configuración.

   ```bash
   sudo iotedge config apply
   ```

1. Confirme el nuevo valor de vigencia.

   ```bash
   sudo iotedge check --verbose
   ```

   Compruebe la salida de la comprobación **preparación de producción: certificados**, que muestra el número de días hasta la expiración de los certificados de CA del dispositivo generados automáticamente.
:::moniker-end
<!-- end 1.2 -->

## <a name="next-steps"></a>Pasos siguientes

La instalación de certificados en un dispositivo IoT Edge es un paso necesario antes de implementar la solución en producción. Obtenga más información sobre cómo [preparar la implementación de la solución de IoT Edge en producción](production-checklist.md).
