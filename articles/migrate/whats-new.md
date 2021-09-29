---
title: Novedades de Azure Migrate
description: Obtenga información sobre las novedades y actualizaciones recientes del servicio Azure Migrate.
ms.topic: overview
author: anvar-ms
ms.author: anvar
ms.manager: bsiva
ms.date: 08/04/2021
ms.custom: mvc
ms.openlocfilehash: 99216bd26bfff76c4342f1533ff4ced070a69e92
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124735972"
---
# <a name="whats-new-in-azure-migrate"></a>Novedades de Azure Migrate

[Azure Migrate](migrate-services-overview.md) le ayuda a detectar, evaluar y migrar servidores, aplicaciones y datos locales a la nube de Microsoft Azure. En este artículo se resumen las nuevas versiones y características de Azure Migrate.

## <a name="update-august-2021"></a>Actualización (agosto de 2021)

- La detección y evaluación a gran escala de las aplicaciones web de ASP.NET que se ejecutan en servidores IIS en el entorno de VMware se encuentra ahora en versión preliminar. [Más información](concepts-azure-webapps-assessment-calculation.md). Consulte los tutoriales sobre [detección](tutorial-discover-vmware.md) y [evaluación](tutorial-assess-webapps.md) para comenzar.
- Compatibilidad con [discos Ultra](../virtual-machines/disks-types.md#ultra-disk) de Azure en la recomendación de evaluación de máquinas virtuales de Azure.
- Disponibilidad general de análisis de dependencias sin agente e inventario de software a gran escala para máquinas virtuales de VMware.
- Actualizaciones del dispositivo de Azure Migrate:
    - "Diagnosticar y resolver" en el dispositivo para ayudar a los usuarios a identificar y autoevaluar cualquier problema con el dispositivo.
    - Script de instalador unificado: script común en el que los usuarios deben seleccionar entre las opciones de escenario, nube y conectividad para implementar un dispositivo con la configuración deseada.
    - Compatibilidad para agregar una cuenta de usuario con acceso "sudo" en el administrador de configuración del dispositivo para realizar la detección de servidores Linux (como alternativa a proporcionar la cuenta raíz o habilitar los permisos setcap).
    - Compatibilidad para editar las propiedades de conexión de SQL Server en el administrador de configuración del dispositivo.

## <a name="update-july-2021"></a>Actualización (julio 2021)

- Azure Migrate: la herramienta de contenedorización de aplicaciones ahora le permite empaquetar aplicaciones que se ejecutan en servidores en una imagen de contenedor e implementar la aplicación en contenedores de Azure App Service, ademas de Azure Kubernetes Service. También puede integrar automáticamente la supervisión de aplicaciones para las aplicaciones de Java con Azure Application Insights y usar Azure Key Vault para administrar secretos de aplicación, como certificados y configuraciones parametrizadas. Para más información, consulte los tutoriales [Contenedorización de aplicaciones de ASP.NET y migración a Azure App Service](tutorial-app-containerization-aspnet-app-service.md) y [Contenedorización de aplicaciones web de Java y migración a Azure App Service](tutorial-app-containerization-java-app-service.md) para empezar a trabajar.

## <a name="update-june-2021"></a>Actualización (junio de 2021)

- Azure Migrate ahora admite nuevas regiones y zonas geográficas de la nube pública. [Más información](migrate-support-matrix.md#supported-geographies-public-cloud)
- Azure Migrate permite registrar servidores que ejecutan SQL Server en el RP de máquina virtual de SQL durante la replicación para instalar automáticamente la extensión del Agente IaaS de SQL. Esta característica está disponible para VMware sin agente, Hyper-V sin agente y migraciones basadas en agente.
- La importación del archivo CSV para la evaluación ahora admite hasta 20 discos. Anteriormente, se limitaba a ocho discos por servidor.

## <a name="update-may-2021"></a>Actualización (mayo de 2021)

- Ahora se admite la migración de máquinas virtuales y servidores físicos con discos del sistema operativo de hasta 4 TB mediante el método de migración basado en agente.

## <a name="update-march-2021"></a>Actualización (marzo de 2021)

- Compatibilidad para proporcionar varias credenciales de servidor en un dispositivo de Azure Migrate y detectar las aplicaciones instaladas (inventario de software), realizar análisis de dependencias sin agente y detectar instancias y bases de datos de SQL Server en el entorno de VMware. [Más información](tutorial-discover-vmware.md#provide-server-credentials)
- La herramienta de detección y evaluación de las bases de datos e instancias de SQL Server que se ejecutan en el entorno de VMware se encuentran ahora en versión preliminar. [Más información](concepts-azure-sql-assessment-calculation.md) Consulte los tutoriales sobre [detección](tutorial-discover-vmware.md) y [evaluación](tutorial-assess-sql.md) para empezar a trabajar.
- La migración de VMware sin agente ahora admite una replicación simultánea de 500 máquinas virtuales por vCenter.
- Azure Migrate: la herramienta de contenedorización de aplicaciones ahora le permite empaquetar aplicaciones que se ejecutan en servidores en una imagen de contenedor e implementar la aplicación en contenedores en Azure Kubernetes Service.  
Para más información, consulte los tutoriales [Contenedorización de aplicaciones de ASP.NET y migración a Azure Kubernetes Service](tutorial-app-containerization-aspnet-kubernetes.md) y [Contenedorización de aplicaciones web de Java y migración a Azure Kubernetes Service](tutorial-app-containerization-java-kubernetes.md) para empezar a trabajar.

## <a name="update-january-2021"></a>Actualización (enero de 2021)

- Azure Migrate: Server Migration ahora permite migrar máquinas virtuales de VMware, servidores físicos y máquinas virtuales de otras nubes a máquinas virtuales de Azure con discos con cifrado del lado servidor con claves administradas por el cliente (CMK).

## <a name="update-december-2020"></a>Actualización (diciembre de 2020)

- Azure Migrate ahora instala automáticamente el agente de máquina virtual de Azure en las máquinas virtuales de VMware al migrarlas a Azure mediante el método sin agente de la migración de VMware. (Windows Server 2008 R2 y versiones posteriores)
- La migración de máquinas virtuales de VMware a máquinas virtuales de Azure que usan discos cifrados mediante el cifrado del lado servidor (SSE) con claves administradas por el cliente (CMK) mediante Azure Migrate Server Migration (replicación sin agente) ahora está disponible mediante Azure Portal.

## <a name="update-september-2020"></a>Actualización (septiembre de 2020)

- Ahora se admite la migración de servidores a Availability Zones.
- Ahora se admite la migración de máquinas virtuales y servidores físicos basados en UEFI a máquinas virtuales de Azure de segunda generación. Con esta versión, la herramienta Azure Migrate: Server Migration no realizará la conversión de máquinas virtuales de segunda generación a máquinas virtuales de primera generación durante la migración.
- Hay disponible un nuevo panel de evaluación de Power BI para Azure Migrate que ayuda a comparar los costos en diferentes configuraciones de evaluación. El panel incluye una utilidad de PowerShell que crea automáticamente las evaluaciones que se conectan al panel de Power BI. [Más información.](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/assessment-utility)
- El análisis de dependencias (sin agente) ahora se puede ejecutar simultáneamente en mil máquinas virtuales.
- El análisis de dependencias (sin agente) ahora se puede habilitar o deshabilitar a escala mediante scripts de PowerShell. [Más información.](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/dependencies-at-scale)
- Visualice las conexiones de red en Power BI con los datos recopilados mediante el análisis de dependencias (sin agente). [Más información.](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/dependencies-at-scale)
- Ahora se admite la migración de máquinas virtuales de VMware con un tamaño de disco de datos de hasta 32 TB mediante Azure Migrate: Método de migración de VMware sin agente de Server Migration.

## <a name="update-august-2020"></a>Actualización (agosto de 2020)

- Mejor experiencia de incorporación, en la que se genera una clave de proyecto de Azure Migrate desde el portal y se usa para completar el registro del dispositivo.
- Opción para descargar archivos OVA/VHD o los scripts del instalador desde el portal para configurar los dispositivos de VMware y Hyper-V, respectivamente.
- Se ha actualizado el administrador de configuración de aplicaciones con una experiencia de usuario mejorada.
- Compatibilidad con múltiples credenciales para la detección de máquinas virtuales con Hyper-V.

## <a name="update-july-2020"></a>Actualización (julio 2020)

- La migración de VMware sin agente ahora admite la replicación simultánea de 300 máquinas virtuales por vCenter.

## <a name="update-june-2020"></a>Actualización (junio de 2020)

- Ahora se admiten las evaluaciones para migrar VM de VMware locales a [Azure VMware Solution (AVS)](./concepts-azure-vmware-solution-assessment-calculation.md). [Más información](how-to-create-azure-vmware-solution-assessment.md)
- Compatibilidad con varias credenciales en el dispositivo para la detección de servidores físicos.
- Compatibilidad para permitir el inicio de sesión de Azure desde el dispositivo para el inquilino en el que se ha configurado la restricción de inquilino.

## <a name="update-april-2020"></a>Actualización (abril de 2020)

Azure Migrate admite implementaciones en Azure Government.

- Puede descubrir y evaluar máquinas virtuales de VMware, máquinas virtuales de Hyper-V y servidores físicos.
- Puede migrar máquinas virtuales de VMware locales, máquinas virtuales Hyper-V y servidores físicos a Azure.
- En el caso de VMware, puede realizar una migración con agente o sin agente. [Más información](server-migrate-overview.md).
- [Examine](migrate-support-matrix.md#supported-geographies-azure-government) regiones y zonas geográficas compatibles para obtener Azure Government.
- El [análisis de dependencias basado en agente](concepts-dependency-visualization.md#agent-based-analysis) no está disponible en Azure Government.
- Se admiten las características en versión preliminar en Azure Government, el [análisis de dependencias sin agente ](concepts-dependency-visualization.md#agentless-analysis) y la [detección de aplicaciones](how-to-discover-applications.md).

## <a name="update-march-2020"></a>Actualización (marzo de 2020)

Ahora hay disponible una instalación basada en scripts para configurar el [dispositivo de Azure Migrate](migrate-appliance.md):

- La instalación basada en script es una alternativa a la instalación .OVA (VMware)/VHD (Hyper-V) del dispositivo.
- Proporciona un script de instalador de PowerShell que se puede usar para configurar el dispositivo para VMware/Hyper-V en una máquina existente que ejecute Windows Server 2016.

## <a name="update-november-2019"></a>Actualización (noviembre de 2019)

Se han agregado muchas características nuevas a Azure Migrate:

- **Evaluación de servidores físicos**. Ahora se admite la evaluación de servidores físicos locales, además de la migración de servidores físicos que ya se admite.
- **Evaluación basada en la importación**. Ahora se admite la evaluación de máquinas con metadatos y datos de rendimiento proporcionados en un archivo CSV.
- **Detección de aplicaciones**: Azure Migrate admite ahora la detección en el nivel de aplicación de aplicaciones, roles y características mediante el uso del dispositivo Azure Migrate. Actualmente solo se admite para máquinas virtuales de VMware y solo se limita a la detección (actualmente no se admite la evaluación). [Más información](how-to-discover-applications.md)
- **Visualización de dependencias sin agentes**: Ya no necesita instalar agentes explícitamente para la visualización de dependencias. Ahora se admiten tanto sin agente como basados en agente.
- **Virtual Desktop**: Use herramientas de ISV para evaluar y migrar la infraestructura de escritorio virtual (VDI) local a Windows Virtual Desktop en Azure.
- **Aplicación web**: Migration Assistant de Azure App Service, que se usa para evaluar y migrar aplicaciones web, ahora se integra en Azure Migrate.

Se han agregado a Azure Migrate nuevas herramientas para la evaluación y la migración:

- **RackWare**: servicios de migración a la nube.
- **Movere**: Evaluación de ofertas.

[Más información](migrate-services-overview.md) sobre el uso de herramientas y ofertas de ISV para la evaluación y la migración en Azure Migrate.

## <a name="azure-migrate-current-version"></a>Versión actual de Azure Migrate

La versión actual de Azure Migrate (publicada en julio de 2019) proporciona muchas características nuevas:

- **Plataforma de migración unificada**: Ahora, Azure Migrate ofrece un único portal para centralizar, administrar y realizar el seguimiento del proceso de migración a Azure, con un flujo de implementación mejorado y una experiencia de portal.
- **Herramientas de evaluación y migración**: Azure Migrate proporciona herramientas nativas y se integra con otros servicios de Azure, así como con herramientas de proveedores de software independientes (ISV). [Más información](migrate-services-overview.md#isv-integration) acerca de la integración de ISV.
- **Evaluación de Azure Migrate**: Puede usar la herramienta de evaluación de servidores de Azure Migrate para evaluar VM de VMware y de Hyper-V para la migración a Azure. También puede evaluar la migración con otros servicios de Azure y herramientas de ISV.
- **Migración de Azure Migrate**: La herramienta de migración de servidores de Azure Migrate permite migrar VM de VMware locales y VM de Hyper-V a Azure, así como servidores físicos, otros servidores virtualizados y VM de nube privada o pública. Además, puede migrar a Azure mediante herramientas de ISV.
- **Dispositivo con Azure Migrate**: Azure Migrate implementa un dispositivo ligero para la detección y evaluación de VM de VMware locales y VM de Hyper-V.
    - La evaluación y la migración de servidores de Azure Migrate usan este dispositivo para realizar una migración sin agente.
    - El dispositivo detecta continuamente los metadatos de servidor y los datos de rendimiento con fines de evaluación y migración.  
- **Migración de VM de VMware**:  La migración de servidores de Azure Migrate proporciona un par de métodos para migrar VM de VMware locales a Azure.  Una migración sin agente mediante el dispositivo de Azure Migrate y una migración basada en agente que usa un dispositivo de replicación e implementa un agente en cada VM que quiere migrar. [Más información](server-migrate-overview.md)
 - **Valoración y migración de bases de datos**: desde Azure Migrate, puede evaluar las bases de datos locales para la migración a Azure mediante Azure Database Migration Assistant. Puede migrar bases de datos con Azure Database Migration Service.
- **Migración de aplicaciones web**: puede evaluar las aplicaciones web con una dirección URL de punto de conexión pública mediante Azure App Service. Para la migración de aplicaciones de .NET internas, puede descargar y ejecutar App Service Migration Assistant.
- **Data Box**: importe grandes cantidades de datos sin conexión a Azure mediante Azure Data Box en Azure Migrate.

## <a name="azure-migrate-previous-version"></a>Versión anterior de Azure Migrate

Si usaba la versión anterior de Azure Migrate (solo se admitía la valoración de máquinas virtuales de VMware locales), ahora debería usar la versión actual. En la versión anterior, ya no puede crear nuevos proyectos de Azure Migrate ni realizar nuevas detecciones. Todavía puede obtener acceso a los proyectos existentes. Para hacerlo, en Azure Portal > **Todos los servicios**, busque **Azure Migrate**. En las notificaciones de Azure Migrate, hay un vínculo para acceder a antiguos proyectos de Azure Migrate.

## <a name="next-steps"></a>Pasos siguientes

- [Más información](https://azure.microsoft.com/pricing/details/azure-migrate/) sobre los precios de Azure Migrate.
- [Repase las preguntas más frecuentes](resources-faq.md) sobre Azure Migrate.
- Pruebe nuestros tutoriales para evaluar las [VM de VMware](./tutorial-assess-vmware-azure-vm.md) y [las VM de Hyper-V](tutorial-assess-hyper-v.md).