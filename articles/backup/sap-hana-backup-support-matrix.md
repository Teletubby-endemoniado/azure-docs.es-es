---
title: Matriz de compatibilidad de SAP HANA
description: En este artículo se proporciona información sobre los escenarios admitidos y las limitaciones al usar Azure Backup para realizar copias de seguridad de bases de datos de SAP HANA en máquinas virtuales de Azure.
ms.topic: conceptual
ms.date: 10/22/2021
ms.custom: references_regions
ms.openlocfilehash: b9d642d0c70b06677aedcce99c1f21ac4b3a7d34
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130244547"
---
# <a name="support-matrix-for-backup-of-sap-hana-databases-on-azure-vms"></a>Matriz de compatibilidad de la copia de seguridad de bases de datos de SAP HANA en máquinas virtuales de Azure

Azure Backup admite la realización de copias de seguridad de bases de datos de SAP HANA en Azure. En este artículo, se resumen los escenarios admitidos y las limitaciones presentes al usar Azure Backup para realizar copias de seguridad de bases de datos de SAP HANA en máquinas virtuales de Azure.

> [!NOTE]
> La frecuencia de la copia de seguridad de registros ahora puede establecerse en un mínimo de 15 minutos. Las copias de seguridad de los registros comienzan el flujo solo después de que se haya realizado correctamente una copia de seguridad completa de la base de datos.

## <a name="scenario-support"></a>Compatibilidad con los escenarios

| **Escenario**               | **Configuraciones admitidas**                                | **Configuraciones no admitidas**                              |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Topología**               | SAP HANA que solo se ejecuta en máquinas virtuales Linux de Azure                    | Instancias grandes de HANA (HLI)                                   |
| **Regiones**                   | **DISPONIBILIDAD GENERAL:**<br> **Américas**: Centro de EE. UU., Este de EE. UU. 2, Este de EE. UU., Centro-norte de EE. UU., Centro-sur de EE. UU., Oeste de EE. UU. 2, Centro-oeste de EE. UU., Oeste de EE. UU., Centro de Canadá, Este de Canadá, Sur de Brasil <br> **Asia Pacífico**: Centro de Australia, Centro de Australia 2, Este de Australia, Sudeste de Australia, Este de Japón, Oeste de Japón, Centro de Corea, Sur de Corea del Sur, Asia Oriental, Sudeste Asiático, Centro de la India, India del Sur, Oeste de la India, Este de China, Norte de China, Este de China 2, Norte de China 2 <br> **Europa**: Oeste de Europa, Norte de Europa, Centro de Francia, Sur de Reino Unido, Oeste de Reino Unido, Norte de Alemania, Centro-oeste de Alemania, Norte de Suiza, Oeste de Suiza, Centro-norte de Suiza, Este de Noruega, Oeste de Noruega <br> **África/ME**: Norte de Sudáfrica, Oeste de Sudáfrica, Norte de Emiratos Árabes Unidos, Centro de Emiratos Árabes Unidos  <BR>  **Regiones de Azure Government** | Sur de Francia, Centro de Alemania, Nordeste de Alemania, US Gov IOWA |
| **Versiones del SO**            | SLES 12 con SP2, SP3, SP4 y SP5; SLES 15 con SP0, SP1, SP2 y SP3 <br><br>  RHEL 7.4, 7.6, 7.7, 7.9, 8.1, 8.2 y 8.4                |                                             |
| **Versiones de HANA**          | SDC en HANA 1.x, MDC en HANA 2.x, SPS04, SPS05 Rev <= 55 (aún pendiente de validación para los escenarios habilitados para el cifrado)      |                                                            |
| **Implementaciones de HANA**       | SAP HANA en una sola máquina virtual de Azure: solo escalado vertical. <br><br> En el caso de las implementaciones de alta disponibilidad, ambos nodos de las dos máquinas diferentes se tratan como individuales con cadenas de datos independientes.               | Escalado horizontal <br><br> En las implementaciones de alta disponibilidad, la copia de seguridad no conmutará por error automáticamente al nodo secundario. La configuración de la copia de seguridad debe realizarse por separado para cada nodo.                                           |
| **Instancias de HANA**         | Una sola instancia de SAP HANA en una sola máquina virtual de Azure: solo escalado vertical | Varias instancias de SAP HANA en una sola VM. Solo puede proteger una de estas instancias múltiples a la vez.                  |
| **Tipos de base de datos HANA**    | Contenedor de base de datos única (SDC) en la versión 1.x, contenedor de varias bases de datos (MDC) en la versión 2.x | MDC en HANA 1.x                                              |
| **Tamaño de base de datos de HANA**     | Bases de datos de HANA de tamaño menor o igual que 8 TB (este tamaño no es el de la memoria del sistema de HANA)               |                                                              |
| **Tipos de copia de seguridad**           | Copias de seguridad completas, diferenciales, incrementales y de registros                          |  Instantáneas                                       |
| **Tipos de restauración**          | Consulte la nota SAP HANA [1642148](https://launchpad.support.sap.com/#/notes/1642148) para obtener información sobre los tipos de restauración admitidos |                                                              |
| **Límites de copia de seguridad**          | Copias de seguridad completas de hasta 8 TB por instancia de SAP HANA (límite temporal)         |                                                              |
| **Número de copias de seguridad completas al día**     |   Una copia de seguridad programada.  <br><br>   Tres copias de seguridad a petición. <br><br> Se recomienda no desencadenar más de tres copias de seguridad al día. Sin embargo, para permitir reintentos del usuario en caso de intentos fallidos, el límite máximo de copias de seguridad a petición se establece en nueve intentos.  |
| **Configuraciones especiales** |                                                              | SAP HANA y Dynamic Tiering <br>  Clonación a través de LaMa        |

------

>[!NOTE]
>Azure Backup no se ajusta automáticamente a los cambios del horario de verano cuando realiza la copia de seguridad de una base de datos SAP HANA en una máquina virtual de Azure.
>
>Modifique la directiva de forma manual según sea necesario.

> [!NOTE]
> Ahora, puede [supervisar los trabajos de copia de seguridad y restauración](./sap-hana-db-manage.md#monitor-manual-backup-jobs-in-the-portal) (en la misma máquina) desencadenados por clientes nativos de HANA (SAP HANA Studio/Cockpit/DBA Cockpit) en Azure Portal.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [hacer copias de seguridad de bases de datos de SAP HANA que se ejecutan en VM de Azure](./backup-azure-sap-hana-database.md).
* Obtenga información sobre cómo [restaurar bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure](./sap-hana-db-restore.md).
* Obtenga información acerca de cómo [administrar bases de datos de SAP HANA de las que se realizan copias de seguridad mediante Azure Backup](sap-hana-db-manage.md).
* Obtenga información acerca de cómo [solucionar problemas comunes al realizar copias de seguridad de bases de datos de SAP HANA](./backup-azure-sap-hana-database-troubleshoot.md).
