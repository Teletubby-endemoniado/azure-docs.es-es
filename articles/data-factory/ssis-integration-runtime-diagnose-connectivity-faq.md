---
title: Uso de la característica Diagnosticar conectividad en SSIS Integration Runtime
description: Solucione problemas de conexión en SSIS Integration Runtime mediante la característica Diagnosticar conectividad.
ms.service: data-factory
ms.subservice: integration-services
ms.topic: conceptual
ms.author: meiyl
author: meiyl
ms.reviewer: sawinark
ms.date: 06/21/2021
ms.openlocfilehash: e1d1b8a876d55698db83b0c7d331f45797c3c1b3
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124743174"
---
# <a name="use-the-diagnose-connectivity-feature-in-the-ssis-integration-runtime"></a>Uso de la característica Diagnosticar conectividad en SSIS Integration Runtime

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Es posible que encuentre problemas de conectividad al ejecutar paquetes de SQL Server Integration Services (SSIS) en SSIS Integration Runtime. Estos problemas se producen especialmente si SSIS Integration Runtime se une a la instancia de Azure Virtual Network.

Solucione los problemas de conectividad mediante el uso de la característica *Diagnosticar conectividad* para probar las conexiones. La característica se encuentra en la página de supervisión de SSIS Integration Runtime del portal de Azure Data Factory.

 :::image type="content" source="media/ssis-integration-runtime-diagnose-connectivity-faq/ssis-monitor-diagnose-connectivity.png" alt-text="Página Supervisión: Diagnosticar conectividad":::

 :::image type="content" source="media/ssis-integration-runtime-diagnose-connectivity-faq/ssis-monitor-test-connection.png" alt-text="Página Supervisión: Probar conexión":::

Use las secciones siguientes para obtener información sobre los errores más comunes que se producen al probar las conexiones. En cada sección se describe:

- Código de error
- Mensaje de error
- Causas posibles del error
- Soluciones recomendadas

## <a name="error-code-invalidinput"></a>Código de error: InvalidInput

- **Mensaje de error**: "Compruebe que la entrada es correcta".
- **Causa posible**: la entrada es incorrecta.
- **Recomendación:** Compruebe la entrada.

## <a name="error-code-firewallornetworkissue"></a>Código de error: FirewallOrNetworkIssue

- **Mensaje de error**: "Compruebe que este puerto está abierto en el firewall, el servidor o el grupo de seguridad de red y que la red es estable".
- **Causas posibles:**
  - El servidor no abre el puerto.
  - El grupo de seguridad de red tiene denegado el tráfico de salida en el puerto.
  - La solicitud NVA, Azure Firewall o el firewall local no abren el puerto.
- **Recomendaciones:**
  - Abra el puerto en el servidor.
  - Actualice el grupo de seguridad de red para permitir el tráfico de salida en el puerto.
  - Abra el puerto en la solicitud NVA, Azure Firewall o el firewall local.

## <a name="error-code-misconfigureddnssettings"></a>Código de error: MisconfiguredDnsSettings

- **Mensaje de error**: "Si usa su propio servidor DNS en la red virtual unida por Azure-SSIS IR, compruebe que puede resolver el nombre de host".
- **Causas posibles:**
  -  Hay un problema con el DNS personalizado.
  -  No usa un nombre de dominio completo (FQDN) para el nombre de host privado.
- **Recomendaciones:**
  -  Corrija el problema de DNS personalizado para asegurarse de que puede resolver el nombre de host.
  -  Use el nombre de dominio completo. Azure-SSIS IR no anexará automáticamente su sufijo DNS. Por ejemplo, utilice **<su_servidor_privado>.contoso. com** en lugar de **<su_servidor_privado>** .

## <a name="error-code-servernotallowremoteconnection"></a>Código de error: ServerNotAllowRemoteConnection

- **Mensaje de error**: "Compruebe que el servidor permite las conexiones TCP remotas a través de este puerto".
- **Causas posibles:**
  -  El firewall del servidor no permite conexiones TCP remotas.
  -  El servidor no está en línea.
- **Recomendaciones:**
  -  Permita las conexiones TCP remotas en el firewall del servidor.
  -  Inicie el servidor.
   
## <a name="error-code-misconfigurednsgsettings"></a>Código de error: MisconfiguredNsgSettings

- **Mensaje de error**: "Compruebe que el NSG de la red virtual permite el tráfico de salida a través de este puerto. Si usa Azure ExpressRoute o una UDR, compruebe que este puerto está abierto en el firewall o el servidor".
- **Causas posibles:**
  -  El grupo de seguridad de red tiene denegado el tráfico de salida en el puerto.
  -  La solicitud NVA, Azure Firewall o el firewall local no abren el puerto.
- **Recomendación:**
  -  Actualice el grupo de seguridad de red para permitir el tráfico de salida en el puerto.
  -  Abra el puerto en la solicitud NVA, Azure Firewall o el firewall local.

## <a name="error-code-genericissues"></a>Código de error: GenericIssues

- **Mensaje de error**: "Error en la conexión de prueba debido a problemas genéricos".
- **Causa posible**: la conexión de prueba encontró un problema temporal general.
- **Recomendación:** Vuelva a intentar la conexión de prueba más tarde. Si el reintento no sirve, póngase en contacto con el equipo de soporte técnico de Azure Data Factory.

## <a name="error-code-pspingexecutiontimeout"></a>Código de error: PSPingExecutionTimeout

- **Mensaje de error**: "Se agotó el tiempo de espera de la conexión de prueba. Inténtelo de nuevo más tarde".
- **Causa posible**: se agotó el tiempo de espera de la conectividad de prueba.
- **Recomendación:** Vuelva a intentar la conexión de prueba más tarde. Si el reintento no sirve, póngase en contacto con el equipo de soporte técnico de Azure Data Factory.

## <a name="error-code-networkinstable"></a>Código de error: NetworkInstable

- **Mensaje de error**: "La conexión de prueba se realizó correctamente, pero con irregularidades, debido a la inestabilidad de la red".
- **Causa posible**: problema de red transitorio.
- **Recomendación:** Compruebe si la red del servidor o del firewall es estable.

## <a name="next-steps"></a>Pasos siguientes

- [Migración de trabajos de SSIS con SSMS](how-to-migrate-ssis-job-ssms.md)
- [Ejecutar paquetes de SSIS en Azure con SSDT](how-to-invoke-ssis-package-ssdt.md)
- [Programar paquetes SSIS en Azure](how-to-schedule-azure-ssis-integration-runtime.md)
