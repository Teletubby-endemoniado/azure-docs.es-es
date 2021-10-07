---
title: Detalles del cumplimiento normativo de CIS Microsoft Azure Foundations Benchmark 1.1.0
description: Detalles de la iniciativa integrada de cumplimiento normativo de CIS Microsoft Azure Foundations Benchmark 1.1.0. Cada control se corresponde a una o varias definiciones de Azure Policy que ayudan en la evaluación.
ms.date: 09/17/2021
ms.topic: sample
ms.custom: generated
ms.openlocfilehash: 64d435173809ac2af16c52f8b7999afd15c2d117
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128646182"
---
# <a name="details-of-the-cis-microsoft-azure-foundations-benchmark-110-regulatory-compliance-built-in-initiative"></a>Detalles de la iniciativa integrada de cumplimiento normativo de CIS Microsoft Azure Foundations Benchmark 1.1.0.

En el siguiente artículo se detalla la correspondencia entre los **dominios de cumplimiento** y los **controles** de la definición de la iniciativa integrada del cumplimiento normativo de Azure Policy en CIS Microsoft Azure Foundations Benchmark 1.1.0.
Para más información sobre este estándar de cumplimiento, consulte [CIS Microsoft Azure Foundations Benchmark 1.1.0](https://www.cisecurity.org/benchmark/azure/). Para entender el concepto de _propiedad_, consulte [Definición de directivas de Azure Policy](../concepts/definition-structure.md#type) y [Responsabilidad compartida en la nube](../../../security/fundamentals/shared-responsibility.md).

Las siguientes asignaciones son para los controles de **CIS Microsoft Azure Foundations Benchmark 1.1.0**. Use el panel de navegación de la derecha para ir directamente a un **dominio de cumplimiento** específico. Muchos de los controles se implementan con una definición de iniciativa de [Azure Policy](../overview.md). Para revisar la definición de iniciativa completa, abra **Policy** en Azure Portal y seleccione la página **Definiciones**.
Busque y seleccione la definición de la iniciativa integrada de cumplimiento normativo de **CIS Microsoft Azure Foundations Benchmark 1.0**.

Esta iniciativa integrada se implementa como parte del [ejemplo de plano técnico de CIS Microsoft Azure Foundations Benchmark 1.1.0](../../blueprints/samples/cis-azure-1-1-0.md).

> [!IMPORTANT]
> Cada control que se muestra a continuación está asociado a una o varias definiciones de [Azure Policy](../overview.md).
> Estas directivas pueden ayudarle a [evaluar el cumplimiento](../how-to/get-compliance-data.md) mediante el control. Sin embargo, con frecuencia no hay una correspondencia completa o exacta entre un control y una o varias directivas. Como tal, el **cumplimiento** en Azure Policy solo se refiere a las propias definiciones de directiva; esto no garantiza que se cumpla totalmente con todos los requisitos de un control. Además, el estándar de cumplimiento incluye controles que no se abordan con las definiciones de Azure Policy en este momento. Por lo tanto, el cumplimiento en Azure Policy es solo una vista parcial del estado general de cumplimiento. Las asociaciones entre los dominios de cumplimiento, los controles y las definiciones de Azure Policy para este estándar de cumplimiento pueden cambiar con el tiempo. Para ver el historial de cambios, consulte el [historial de confirmación de GitHub](https://github.com/Azure/azure-policy/commits/master/built-in-policies/policySetDefinitions/Regulatory%20Compliance/CISv1_1_0.json).

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

### <a name="ensure-that-multi-factor-authentication-is-enabled-for-all-privileged-users"></a>Asegúrese de que la autenticación multifactor esté habilitada para todos los usuarios con privilegios.

**Identificador**: CIS Azure 1.1 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[MFA debe estar habilitado en las cuentas con permisos de escritura de la suscripción.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F9297c21d-2ed6-4474-b48f-163f75654ce3) |Multi-Factor Authentication (MFA) debe estar habilitada para todas las cuentas de la suscripción que tengan permisos de escritura, a fin de evitar una brecha de seguridad en las cuentas o los recursos. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableMFAForWritePermissions_Audit.json) |
|[MFA debe estar habilitada en las cuentas con permisos de propietario en la suscripción](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Faa633080-8b72-40c4-a2d7-d00c03e80bed) |Multi-Factor Authentication (MFA) debe estar habilitada para todas las cuentas de la suscripción que tengan permisos de propietario, a fin de evitar una brecha de seguridad en las cuentas o los recursos. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableMFAForOwnerPermissions_Audit.json) |

### <a name="ensure-that-multi-factor-authentication-is-enabled-for-all-non-privileged-users"></a>Asegúrese de que la autenticación multifactor esté habilitada para todos los usuarios sin privilegios.

**Identificador**: CIS Azure 1.2 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[MFA debe estar habilitada en las cuentas con permisos de lectura en la suscripción](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe3576e28-8b17-4677-84c3-db2990658d64) |Multi-Factor Authentication (MFA) debe estar habilitada para todas las cuentas de la suscripción que tengan permisos de lectura, a fin de evitar una brecha de seguridad en las cuentas o los recursos. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableMFAForReadPermissions_Audit.json) |

### <a name="ensure-that-there-are-no-guest-users"></a>Asegúrese de que no hay ningún usuario invitado.

**Identificador**: CIS Azure 1.3 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las cuentas externas con permisos de propietario deben quitarse de la suscripción](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff8456c1c-aa66-4dfb-861a-25d127b775c9) |Las cuentas externas con permisos de propietario deben quitarse de la suscripción a fin de evitar el acceso no supervisado. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_RemoveExternalAccountsWithOwnerPermissions_Audit.json) |
|[Las cuentas externas con permisos de lectura deben quitarse de la suscripción](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5f76cf89-fbf2-47fd-a3f4-b891fa780b60) |Las cuentas externas con privilegios de lectura deben quitarse de la suscripción a fin de evitar el acceso no supervisado. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_RemoveExternalAccountsReadPermissions_Audit.json) |
|[Las cuentas externas con permisos de escritura deben quitarse de la suscripción](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5c607a2e-c700-4744-8254-d77e7c9eb5e4) |Las cuentas externas con privilegios de escritura deben quitarse de la suscripción a fin de evitar el acceso no supervisado. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_RemoveExternalAccountsWritePermissions_Audit.json) |

### <a name="ensure-that-no-custom-subscription-owner-roles-are-created"></a>Asegúrese de que no existe ningún rol de propietario de suscripción personalizado.

**Identificador**: CIS Azure 1.23 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[No deben existir roles de propietario de suscripción personalizados](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F10ee2ea2-fb4d-45b8-a7e9-a2e770044cd9). |Esta directiva garantiza que no existe ningún rol de propietario de suscripción personalizado. |Audit, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/CustomSubscription_OwnerRole_Audit.json) |

## <a name="security-center"></a>Security Center

### <a name="ensure-that-standard-pricing-tier-is-selected"></a>Asegúrese de que el plan de tarifa estándar está seleccionado.

**Identificador**: CIS Azure 2.1 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Se debe habilitar Azure Defender para App Service](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F2913021d-f2fd-4f3d-b958-22354e2bdbcb) |Azure Defender para App Service aprovecha la escalabilidad de la nube, y la visibilidad que ofrece Azure como proveedor de servicios en la nube, para supervisar si se producen ataques comunes a aplicaciones web. |AuditIfNotExists, Disabled |[1.0.3](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableAdvancedThreatProtectionOnAppServices_Audit.json) |
|[Se debe habilitar Azure Defender para servidores de Azure SQL Database](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7fe3b40f-802b-4cdd-8bd4-fd799c948cc2) |Azure Defender para SQL proporciona funcionalidad para mostrar y mitigar posibles vulnerabilidades de base de datos, detectar actividades anómalas que podrían indicar amenazas para bases de datos SQL, y detectar y clasificar datos confidenciales. |AuditIfNotExists, Disabled |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableAdvancedDataSecurityOnSqlServers_Audit.json) |
|[Se debe habilitar Azure Defender para registros de contenedor](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc25d9a16-bc35-4e15-a7e5-9db606bf9ed4) |Azure Defender para registros de contenedor proporciona análisis de vulnerabilidades de las imágenes extraídas en los últimos 30 días, insertadas en el registro o importadas, y expone los hallazgos detallados por imagen. |AuditIfNotExists, Disabled |[1.0.3](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableAdvancedThreatProtectionOnContainerRegistry_Audit.json) |
|[Se debe habilitar Azure Defender para Key Vault](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0e6763cc-5078-4e64-889d-ff4d9a839047) |Azure Defender para Key Vault proporciona un nivel de protección adicional de inteligencia de seguridad, ya que detecta intentos inusuales y potencialmente dañinos de obtener acceso a las cuentas de Key Vault o aprovechar sus vulnerabilidades de seguridad. |AuditIfNotExists, Disabled |[1.0.3](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableAdvancedThreatProtectionOnKeyVaults_Audit.json) |
|[Se debe habilitar Azure Defender para Kubernetes](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F523b5cd1-3e23-492f-a539-13118b6d1e3a) |Azure Defender para Kubernetes proporciona protección en tiempo real contra amenazas para entornos en contenedores y genera alertas en caso de actividad sospechosa. |AuditIfNotExists, Disabled |[1.0.3](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableAdvancedThreatProtectionOnKubernetesService_Audit.json) |
|[Se debe habilitar Azure Defender para servidores](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F4da35fc9-c9e7-4960-aec9-797fe7d9051d) |Azure Defender para servidores proporciona protección en tiempo real contra amenazas para las cargas de trabajo del servidor y genera recomendaciones de protección, así como alertas sobre la actividad sospechosa. |AuditIfNotExists, Disabled |[1.0.3](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableAdvancedThreatProtectionOnVM_Audit.json) |
|[Se debe habilitar Azure Defender para servidores SQL Server en las máquinas](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F6581d072-105e-4418-827f-bd446d56421b) |Azure Defender para SQL proporciona funcionalidad para mostrar y mitigar posibles vulnerabilidades de base de datos, detectar actividades anómalas que podrían indicar amenazas para bases de datos SQL, y detectar y clasificar datos confidenciales. |AuditIfNotExists, Disabled |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableAdvancedDataSecurityOnSqlServerVirtualMachines_Audit.json) |
|[Se debe habilitar Azure Defender para Storage](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F308fbb08-4ab8-4e67-9b29-592e93fb94fa) |Azure Defender para Storage detecta intentos inusuales y potencialmente perjudiciales de acceder a las cuentas de almacenamiento o de vulnerarlas. |AuditIfNotExists, Disabled |[1.0.3](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableAdvancedThreatProtectionOnStorageAccounts_Audit.json) |

### <a name="ensure-that-automatic-provisioning-of-monitoring-agent-is-set-to-on"></a>Asegúrese de que "Aprovisionamiento automático del agente de supervisión" esté establecido en "Activado".

**Identificador**: CIS Azure 2.2 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[El aprovisionamiento automático del agente de Log Analytics debe estar habilitado en la suscripción](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F475aae12-b88a-4572-8b36-9b712b2b3a17) |A fin de supervisar las amenazas y vulnerabilidades de seguridad, Azure Security Center recopila datos de las máquinas virtuales de Azure. El agente de Log Analytics, anteriormente conocido como Microsoft Monitoring Agent (MMA), recopila los datos al leer distintas configuraciones relacionadas con la seguridad y distintos registros de eventos de la máquina y copiar los datos en el área de trabajo de Log Analytics para analizarlos. Se recomienda habilitar el aprovisionamiento automático para implementar automáticamente el agente en todas las máquinas virtuales de Azure admitidas y en las nuevas que se creen. |AuditIfNotExists, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_Automatic_provisioning_log_analytics_monitoring_agent.json) |

### <a name="ensure-asc-default-policy-setting-monitor-system-updates-is-not-disabled"></a>Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar las actualizaciones del sistema" no es "Deshabilitado".

**Identificador**: CIS Azure 2.3 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Se deben instalar actualizaciones del sistema en las máquinas](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F86b3d65f-7626-441e-b690-81a8b71cff60) |Azure Security Center supervisará las actualizaciones del sistema de seguridad que faltan en los servidores como recomendaciones. |AuditIfNotExists, Disabled |[4.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_MissingSystemUpdates_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-os-vulnerabilities-is-not-disabled"></a>Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar las vulnerabilidades del sistema operativo" no sea "Deshabilitado".

**Identificador**: CIS Azure 2.4 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Se deben corregir las vulnerabilidades en la configuración de seguridad en las máquinas](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe1e5fd5d-3e4c-4ce1-8661-7d1873ae6b15) |Azure Security Center supervisará los servidores que no cumplan la línea de base configurada como recomendaciones. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_OSVulnerabilities_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-endpoint-protection-is-not-disabled"></a>Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar Endpoint Protection" no sea "Deshabilitado".

**Identificador**: CIS Azure 2.5 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Supervisar la falta de Endpoint Protection en Azure Security Center](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Faf6cd1bd-1635-48cb-bde7-5b15693900b9) |Azure Security Center supervisará los servidores sin un agente de Endpoint Protection instalado como recomendaciones. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_MissingEndpointProtection_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-disk-encryption-is-not-disabled"></a>Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar el cifrado de disco" no sea "Deshabilitado".

**Identificador**: CIS Azure 2.6 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las máquinas virtuales deben cifrar los discos temporales, las cachés y los flujos de datos entre los recursos de Proceso y Almacenamiento](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0961003e-5a0a-4549-abde-af6a37f2724d) |De manera predeterminada, los discos del sistema operativo y de datos de una máquina virtual se cifran en reposo mediante claves administradas por la plataforma. Los discos temporales, las cachés de datos y los datos que fluyen entre el proceso y el almacenamiento no se cifran. Ignore esta recomendación si usa el cifrado en el host o El cifrado del lado servidor en Managed Disks cumple sus requisitos de seguridad. Obtenga más información en [Cifrado del lado servidor de Azure Disk Storage](../../../virtual-machines/disk-encryption.md) y [Diferentes ofertas de cifrado de disco](../../../virtual-machines/disk-encryption-overview.md#comparison). |AuditIfNotExists, Disabled |[2.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_UnencryptedVMDisks_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-network-security-groups-is-not-disabled"></a>Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar los grupos de seguridad de red" no sea "Deshabilitado".

**Identificador**: CIS Azure 2.7 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las recomendaciones de protección de red adaptable se deben aplicar en las máquinas virtuales accesibles desde Internet](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F08e6af2d-db70-460a-bfe9-d5bd474ba9d6) |Azure Security Center analiza los patrones de tráfico de máquinas virtuales orientadas a Internet y proporciona recomendaciones de reglas de grupo de seguridad de red que reducen la superficie de ataque potencial. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_AdaptiveNetworkHardenings_Audit.json) |

### <a name="ensure-asc-default-policy-setting-enable-next-generation-firewallngfw-monitoring-is-not-disabled"></a>Asegúrese de que la configuración de la directiva predeterminada de ASC "Habilitar la supervisión de firewalls de última generación (NGFW)" no sea "Deshabilitado".

**Identificador**: CIS Azure 2.9 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las máquinas virtuales accesibles desde Internet deben estar protegidas con grupos de seguridad de red](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff6de0be7-9a8a-4b8a-b349-43cf02d22f7c) |Proteja sus máquinas virtuales de posibles amenazas limitando el acceso a ellas con grupos de seguridad de red (NSG). Más información sobre cómo controlar el tráfico con los grupos de seguridad de red en [https://aka.ms/nsg-doc](../../../virtual-network/network-security-groups-overview.md). |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_NetworkSecurityGroupsOnInternetFacingVirtualMachines_Audit.json) |
|[Las subredes deben estar asociadas con un grupo de seguridad de red.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe71308d3-144b-4262-b144-efdc3cc90517) |Proteja la subred de posibles amenazas mediante la restricción del acceso con un grupo de seguridad de red (NSG). Estos grupos contienen las reglas de la lista de control de acceso (ACL) que permiten o deniegan el tráfico de red a la subred. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_NetworkSecurityGroupsOnSubnets_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-vulnerability-assessment-is-not-disabled"></a>Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar la evaluación de vulnerabilidades" no sea "Deshabilitado".

**Identificador**: CIS Azure 2.10 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Debe habilitarse una solución de evaluación de vulnerabilidades en sus máquinas virtuales](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F501541f7-f7e7-4cd6-868c-4190fdad3ac9) |Audita las máquinas virtuales para detectar si ejecutan una solución de evaluación de vulnerabilidades admitida. Un componente fundamental de cada programa de seguridad y riesgo cibernético es la identificación y el análisis de las vulnerabilidades. El plan de tarifa estándar de Azure Security Center incluye el análisis de vulnerabilidades de las máquinas virtuales sin costo adicional. Además, Security Center puede implementar automáticamente esta herramienta. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_ServerVulnerabilityAssessment_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-jit-network-access-is-not-disabled"></a>Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar el acceso de red JIT" no sea "Deshabilitado".

**Identificador**: CIS Azure 2.12 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Los puertos de administración de las máquinas virtuales deben protegerse con el control de acceso de red Just-In-Time](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb0f33259-77d7-4c9e-aac6-3aabcfae693c). |Azure Security Center supervisará el posible acceso de red Just-In-Time (JIT) como recomendaciones. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_JITNetworkAccess_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-adaptive-application-whitelisting-is-not-disabled"></a>Asegúrese de que la configuración de la directiva predeterminada de ASC "Monitor Adaptive Application Whitelisting" (Supervisar las listas blancas de aplicaciones adaptables) no sea "Deshabilitado".

**Identificador**: CIS Azure 2.13 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Los controles de aplicaciones adaptables para definir aplicaciones seguras deben estar habilitados en las máquinas](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F47a6b606-51aa-4496-8bb7-64b11cf66adc) |Habilite controles de aplicaciones para definir la lista de aplicaciones seguras conocidas que se ejecutan en las máquinas, y recibir avisos cuando se ejecuten otras aplicaciones. Esta directiva también ayuda a proteger las máquinas frente al malware. Para simplificar el proceso de configuración y mantenimiento de las reglas, Security Center usa el aprendizaje automático para analizar las aplicaciones que se ejecutan en cada máquina y sugerir la lista de aplicaciones seguras conocidas. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_AdaptiveApplicationControls_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-sql-auditing-is-not-disabled"></a>Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar la auditoría de SQL" no sea "Deshabilitado".

**Identificador**: CIS Azure 2.14 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[La auditoría de SQL Server debe estar habilitada](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa6fb4358-5bf4-4ad7-ba82-2cd2f41ce5e9) |La auditoría debe estar habilitada en SQL Server para realizar un seguimiento de las actividades de todas las bases de datos del servidor y guardarlas en un registro de auditoría. |AuditIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServerAuditing_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-sql-encryption-is-not-disabled"></a>Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar el cifrado SQL" no sea "Deshabilitado".

**Identificador**: CIS Azure 2.15 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[El cifrado de datos transparente en bases de datos SQL debe estar habilitado](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F17k78e20-9358-41c9-923c-fb736d382a12) |El cifrado de datos transparente debe estar habilitado para proteger los datos en reposo y satisfacer los requisitos de cumplimiento. |AuditIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlDBEncryption_Audit.json) |

### <a name="ensure-that-security-contact-emails-is-set"></a>Asegúrese de que se ha establecido "Correos electrónicos de contacto de seguridad".

**Identificador**: CIS Azure 2.16 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las suscripciones deben tener una dirección de correo electrónico de contacto para los problemas de seguridad](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F4f4f78b8-e367-4b10-a341-d9a4ad5cf1c7) |Para asegurarse de que las personas pertinentes de la organización reciban una notificación cuando se produzca una vulneración de seguridad potencial en una de las suscripciones, establezca un contacto de seguridad para la recepción de notificaciones por correo electrónico de Security Center. |AuditIfNotExists, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_Security_contact_email.json) |

### <a name="ensure-that-send-email-notification-for-high-severity-alerts-is-set-to-on"></a>Asegúrese de que "Enviar notificaciones sobre alertas de gravedad alta por correo electrónico" se ha establecido en "Activado".

**Identificador**: CIS Azure 2.18 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[La opción para enviar notificaciones por correo electrónico para alertas de gravedad alta debe estar habilitada.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F6e2593d9-add6-4083-9c9b-4b7d2188c899) |Para asegurarse de que las personas pertinentes de la organización reciban una notificación cuando se produzca una vulneración de seguridad potencial en una de las suscripciones, habilite las notificaciones por correo electrónico de alertas de gravedad alta en Security Center. |AuditIfNotExists, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_Email_notification.json) |

### <a name="ensure-that-send-email-also-to-subscription-owners-is-set-to-on"></a>Asegúrese de que "Enviar correo electrónico también a los propietarios de la suscripción" esté establecido en "Activado".

**Identificador**: CIS Azure 2.19 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[La opción para enviar notificaciones por correo electrónico al propietario de la suscripción en relación a alertas de gravedad alta debe estar habilitada.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0b15565f-aa9e-48ba-8619-45960f2c314d) |Para asegurarse de que los propietarios de suscripciones reciban una notificación cuando se produzca una vulneración de seguridad potencial en sus suscripciones, establezca notificaciones por correo electrónico a los propietarios de las suscripciones de alertas de gravedad alta en Security Center. |AuditIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_Email_notification_to_subscription_owner.json) |

## <a name="storage-accounts"></a>Cuentas de almacenamiento

### <a name="ensure-that-secure-transfer-required-is-set-to-enabled"></a>Asegúrese de la opción "Se requiere transferencia segura" esté establecida en "Habilitada".

**Identificador**: CIS Azure 3.1 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Se debe habilitar la transferencia segura a las cuentas de almacenamiento](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F404c3081-a854-4457-ae30-26a93ef643f9) |Permite auditar el requisito de transferencia segura en la cuenta de almacenamiento. La transferencia segura es una opción que obliga a la cuenta de almacenamiento a aceptar solamente solicitudes de conexiones seguras (HTTPS). El uso de HTTPS garantiza la autenticación entre el servidor y el servicio, y protege los datos en tránsito de ataques de nivel de red, como los de tipo "Man in the middle", interceptación y secuestro de sesión |Audit, Deny, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_AuditForHTTPSEnabled_Audit.json) |

### <a name="ensure-that-public-access-level-is-set-to-private-for-blob-containers"></a>Asegúrese de que "Nivel de acceso público" se haya establecido en Privado para los contenedores de blobs

**Identificador**: CIS Azure 3.6 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[No se debe permitir el acceso público a la cuenta de almacenamiento](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F4fa4b6c0-31ca-4c0d-b10d-24b96f62a751) |El acceso de lectura público anónimo a contenedores y blobs de Azure Storage es una manera cómoda de compartir datos, pero también puede plantear riesgos para la seguridad. Para evitar las infracciones de datos producidas por el acceso anónimo no deseado, Microsoft recomienda impedir el acceso público a una cuenta de almacenamiento a menos que su escenario lo requiera. |audit, deny, disabled |[3.0.1-preview](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/ASC_Storage_DisallowPublicBlobAccess_Audit.json) |

### <a name="ensure-default-network-access-rule-for-storage-accounts-is-set-to-deny"></a>Asegúrese de que la regla de acceso de red predeterminada para las cuentas de almacenamiento esté configurada para denegar.

**Identificador**: CIS Azure 3.7 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Se debe restringir el acceso de red a las cuentas de almacenamiento](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F34c877ad-507e-4c82-993e-3452a6e0ad3c) |El acceso de red a las cuentas de almacenamiento debe estar restringido. Configure reglas de red, solo las aplicaciones de redes permitidas pueden acceder a la cuenta de almacenamiento. Para permitir conexiones desde clientes específicos locales o de Internet, se puede conceder acceso al tráfico procedente de redes virtuales de Azure específicas o a intervalos de direcciones IP de Internet públicas. |Audit, Deny, Disabled |[1.1.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_NetworkAcls_Audit.json) |

### <a name="ensure-trusted-microsoft-services-is-enabled-for-storage-account-access"></a>Asegúrese de que "Servicios de Microsoft de confianza" está habilitado para el acceso a la cuenta de almacenamiento.

**Identificador**: CIS Azure 3.8 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las cuentas de almacenamiento deben permitir el acceso desde los servicios de Microsoft de confianza](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc9d007d0-c057-4772-b18c-01e546713bcd) |Algunos servicios de Microsoft que interactúan con las cuentas de almacenamiento funcionan desde redes a las que no se puede conceder acceso a través de reglas de red. Para ayudar a que este tipo de servicio funcione según lo previsto, permita que el conjunto de servicios de Microsoft de confianza omita las reglas de red. Estos servicios usarán luego una autenticación sólida para acceder a la cuenta de almacenamiento. |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/StorageAccess_TrustedMicrosoftServices_Audit.json) |

## <a name="database-services"></a>Servicios de base de datos

### <a name="ensure-that-auditing-is-set-to-on"></a>Asegúrese de que la opción "Auditando" esté establecida en "Activada".

**Identificador**: CIS Azure 4.1 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[La auditoría de SQL Server debe estar habilitada](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa6fb4358-5bf4-4ad7-ba82-2cd2f41ce5e9) |La auditoría debe estar habilitada en SQL Server para realizar un seguimiento de las actividades de todas las bases de datos del servidor y guardarlas en un registro de auditoría. |AuditIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServerAuditing_Audit.json) |

### <a name="ensure-that-auditactiongroups-in-auditing-policy-for-a-sql-server-is-set-properly"></a>Asegúrese de que la opción "AuditActionGroups" de la directiva de auditoría para un servidor SQL Server esté configurada correctamente.

**Identificador**: CIS Azure 4.2 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[La configuración de auditoría de SQL debe tener grupos de acción configurados para capturar actividades críticas](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7ff426e2-515f-405a-91c8-4f2333442eb5) |La propiedad AuditActionsAndGroups debe contener al menos SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP FAILED_DATABASE_AUTHENTICATION_GROUP, BATCH_COMPLETED_GROUP para garantizar un registro de auditoría exhaustivo. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServerAuditing_ActionsAndGroups_Audit.json) |

### <a name="ensure-that-auditing-retention-is-greater-than-90-days"></a>Asegúrese de que la retención de "Auditoría" sea "más de 90 días".

**Identificador**: CIS Azure 4.3 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Los servidores SQL Server con auditoría en el destino de la cuenta de almacenamiento se deben configurar con una retención de 90 días o superior.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F89099bee-89e0-4b26-a5f4-165451757743) |Con fines de investigación de incidentes, se recomienda establecer la retención de datos de auditoría de las instancias de SQL Server en el destino de la cuenta de almacenamiento en al menos 90 días. Confirme que cumple las reglas de retención necesarias para las regiones en las que trabaja. A veces, es necesario para cumplir con los estándares normativos. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServerAuditingRetentionDays_Audit.json) |

### <a name="ensure-that-advanced-data-security-on-a-sql-server-is-set-to-on"></a>Asegúrese de que "Advanced Data Security" en un servidor SQL Server esté establecido en "Activado".

**Identificador**: CIS Azure 4.4 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Se debe habilitar Azure Defender para SQL en las instancias de Azure SQL Server desprotegidas](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fabfb4388-5bf4-4ad7-ba82-2cd2f41ceae9) |Auditoría de los servidores de SQL sin Advanced Data Security |AuditIfNotExists, Disabled |[2.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServer_AdvancedDataSecurity_Audit.json) |
|[Azure Defender para SQL debe habilitarse en las instancias de SQL Managed Instances desprotegidas](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fabfb7388-5bf4-4ad7-ba99-2cd2f41cebb9) |Permite auditr cada servicio SQL Managed Instance sin Advanced Data Security. |AuditIfNotExists, Disabled |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlManagedInstance_AdvancedDataSecurity_Audit.json) |

### <a name="ensure-that-azure-active-directory-admin-is-configured"></a>Asegúrese de que el administrador de Azure Active Directory está configurado.

**Identificador**: CIS Azure 4.8 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[El administrador de Azure Active Directory debe aprovisionarse para servidores SQL Server](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1f314764-cb73-4fc9-b863-8eca98ac36e9) |Permite aprovisionar un administrador de Azure Active Directory para SQL Server a fin de habilitar la autenticación de Azure AD. La autenticación de Azure AD permite la administración simplificada de permisos y la administración centralizada de identidades de usuarios de base de datos y otros servicios de Microsoft |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SQL_DB_AuditServerADAdmins_Audit.json) |

### <a name="ensure-that-data-encryption-is-set-to-on-on-a-sql-database"></a>Asegúrese de que la opción "Cifrado de datos" esté establecida en "Activado" en una base de datos SQL Database.

**Identificador**: CIS Azure 4.9 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[El cifrado de datos transparente en bases de datos SQL debe estar habilitado](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F17k78e20-9358-41c9-923c-fb736d382a12) |El cifrado de datos transparente debe estar habilitado para proteger los datos en reposo y satisfacer los requisitos de cumplimiento. |AuditIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlDBEncryption_Audit.json) |

### <a name="ensure-sql-servers-tde-protector-is-encrypted-with-byok-use-your-own-key"></a>Asegúrese de que el protector de TDE de SQL Server esté cifrado con BYOK (use su propia clave).

**Identificador**: CIS Azure 4.10 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las instancias administradas de SQL deben usar claves administradas por el cliente para cifrar los datos en reposo](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F048248b0-55cd-46da-b1ff-39efd52db260) |La implementación de Cifrado de datos transparente (TDE) con una clave propia proporciona una mayor transparencia y control sobre el protector de TDE, ofrece mayor seguridad con un servicio externo respaldado con HSM y permite la separación de tareas. Esta recomendación se aplica a las organizaciones con un requisito de cumplimiento relacionado. |AuditIfNotExists, Disabled |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlManagedInstance_EnsureServerTDEisEncryptedWithYourOwnKey_Audit.json) |
|[Los servidores SQL deben usar claves administradas por el cliente para cifrar los datos en reposo](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0d134df8-db83-46fb-ad72-fe0c9428c8dd) |La implementación de Cifrado de datos transparente (TDE) con una clave propia proporciona una mayor transparencia y control sobre el protector de TDE, ofrece mayor seguridad con un servicio externo respaldado con HSM y permite la separación de tareas. Esta recomendación se aplica a las organizaciones con un requisito de cumplimiento relacionado. |AuditIfNotExists, Disabled |[2.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServer_EnsureServerTDEisEncryptedWithYourOwnKey_Audit.json) |

### <a name="ensure-enforce-ssl-connection-is-set-to-enabled-for-mysql-database-server"></a>Asegúrese de que "Aplicar conexión SSL" esté establecido en "HABILITADO" para el servidor de bases de datos MySQL.

**Identificador**: CIS Azure 4.11 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Exigir una conexión SSL debe estar habilitado en los servidores de bases de datos MySQL](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe802a67a-daf5-4436-9ea6-f6d821dd0c5d) |Azure Database for MySQL permite conectar el servidor de Azure Database for MySQL con aplicaciones cliente mediante Capa de sockets seguros (SSL). La aplicación de conexiones SSL entre el servidor de bases de datos y las aplicaciones cliente facilita la protección frente a ataques de tipo "Man in the middle" al cifrar el flujo de datos entre el servidor y la aplicación. Esta configuración exige que SSL esté siempre habilitado para el acceso al servidor de bases de datos. |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MySQL_EnableSSL_Audit.json) |

### <a name="ensure-server-parameter-log_checkpoints-is-set-to-on-for-postgresql-database-server"></a>Asegúrese de que "log_checkpoints" del parámetro del servidor está establecido en "ACTIVADO" para el servidor de bases de datos PostgreSQL.

**Identificador**: CIS Azure 4.12 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Los puntos de control del registro se deben habilitar para los servidores de base de datos de PostgreSQL](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Feb6f77b9-bd53-4e35-a23d-7f65d5f0e43d) |Esta directiva ayuda a realizar una auditoría de las bases de datos de PostgreSQL del entorno sin habilitar la opción log_checkpoints. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/PostgreSQL_EnableLogCheckpoint_Audit.json) |

### <a name="ensure-enforce-ssl-connection-is-set-to-enabled-for-postgresql-database-server"></a>Asegúrese de que "Aplicar conexión SSL" esté establecido en "HABILITADO" para el servidor de bases de datos PostgreSQL.

**Identificador**: CIS Azure 4.13 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[La aplicación de la conexión SSL debe estar habilitada para los servidores de base de datos PostgreSQL](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd158790f-bfb0-486c-8631-2dc6b4e8e6af) |Azure Database for PostgreSQL permite conectar el servidor de Azure Database for PostgreSQL a las aplicaciones cliente mediante la Capa de sockets seguros (SSL). La aplicación de conexiones SSL entre el servidor de bases de datos y las aplicaciones cliente facilita la protección frente a ataques de tipo "Man in the middle" al cifrar el flujo de datos entre el servidor y la aplicación. Esta configuración exige que SSL esté siempre habilitado para el acceso al servidor de bases de datos. |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/PostgreSQL_EnableSSL_Audit.json) |

### <a name="ensure-server-parameter-log_connections-is-set-to-on-for-postgresql-database-server"></a>Asegúrese de que "log_connections" del parámetro del servidor está establecido en "ACTIVADO" para el servidor de bases de datos PostgreSQL.

**Identificador**: CIS Azure 4.14 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las conexiones del registro deben estar habilitadas para los servidores de bases de datos de PostgreSQL](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Feb6f77b9-bd53-4e35-a23d-7f65d5f0e442) |Esta directiva ayuda a realizar una auditoría de las bases de datos de PostgreSQL del entorno sin habilitar la opción log_connections. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/PostgreSQL_EnableLogConnections_Audit.json) |

### <a name="ensure-server-parameter-log_disconnections-is-set-to-on-for-postgresql-database-server"></a>Asegúrese de que "log_disconnections" del parámetro del servidor está establecido en "ACTIVADO" para el servidor de bases de datos PostgreSQL.

**Identificador**: CIS Azure 4.15 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las desconexiones se deben registrar para los servidores de base de datos de PostgreSQL.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Feb6f77b9-bd53-4e35-a23d-7f65d5f0e446) |Esta directiva ayuda a realizar una auditoría de las bases de datos de PostgreSQL del entorno sin habilitar la opción log_disconnections. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/PostgreSQL_EnableLogDisconnections_Audit.json) |

### <a name="ensure-server-parameter-connection_throttling-is-set-to-on-for-postgresql-database-server"></a>Asegúrese de que "connection_throttling" del parámetro del servidor está establecido en "ACTIVADO" para el servidor de bases de datos PostgreSQL.

**Identificador**: CIS Azure 4.17 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[La limitación de conexiones debe estar habilitada para los servidores de bases de datos PostgreSQL](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5345bb39-67dc-4960-a1bf-427e16b9a0bd) |Esta directiva permite realizar una auditoría de las bases de datos de PostgreSQL del entorno sin la limitación de conexiones habilitada. Esta configuración habilita la limitación de conexiones temporales por IP si hay demasiados errores de inicio de sesión con una contraseña no válida. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/PostgreSQL_ConnectionThrottling_Enabled_Audit.json) |

## <a name="logging-and-monitoring"></a>Registro y supervisión

### <a name="ensure-that-a-log-profile-exists"></a>Asegúrese de que existe un perfil de registro.

**Identificador**: CIS Azure 5.1.1 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las suscripciones a Azure deben tener un perfil de registro para el registro de actividad](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7796937f-307b-4598-941c-67d3a05ebfe7) |Esta directiva garantiza que un perfil de registro esté habilitado para la exportación de registros de actividad. Audita si no hay ningún perfil de registro creado para exportar los registros a una cuenta de almacenamiento o a un centro de eventos. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/Logprofile_activityLogs_Audit.json) |

### <a name="ensure-that-activity-log-retention-is-set-365-days-or-greater"></a>Asegúrese de que el período de retención del registro de actividad está establecido en 365 días o más.

**Identificador**: CIS Azure 5.1.2 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[El registro de actividad debe conservarse durante al menos un año](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb02aacc0-b073-424e-8298-42b22829ee0a) |Esta directiva audita el registro de actividad si la retención no se estableció en 365 días o en siempre (días de retención establecidos en 0). |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLogRetention_365orGreater.json) |

### <a name="ensure-audit-profile-captures-all-the-activities"></a>Asegúrese de que el perfil de auditoría captura todas las actividades.

**Identificador**: CIS Azure 5.1.3 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[El perfil de registro de Azure Monitor debe recopilar los registros de las categorías "write", "delete" y "action"](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1a4e592a-6a6e-44a5-9814-e36264ca96e7) |Esta directiva garantiza que un perfil de registro recopile registros para las categorías "write", "delete" y "action". |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_CaptureAllCategories.json) |

### <a name="ensure-the-log-profile-captures-activity-logs-for-all-regions-including-global"></a>Asegúrese de que el perfil de registro captura los registros de actividad de todas las regiones, incluida la global.

**Identificador**: CIS Azure 5.1.4 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure Monitor debe recopilar los registros de actividad de todas las regiones](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F41388f1c-2db0-4c25-95b2-35d7f5ccbfa9) |Esta directiva audita el perfil de registro de Azure Monitor que no exporta actividades de todas las regiones admitidas de Azure, incluida la global. |AuditIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_CaptureAllRegions.json) |

### <a name="ensure-the-storage-container-storing-the-activity-logs-is-not-publicly-accessible"></a>Asegúrese de que el contenedor de almacenamiento donde se almacenan los registros de actividad no sea accesible públicamente

**Identificador**: CIS Azure 5.1.5 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[No se debe permitir el acceso público a la cuenta de almacenamiento](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F4fa4b6c0-31ca-4c0d-b10d-24b96f62a751) |El acceso de lectura público anónimo a contenedores y blobs de Azure Storage es una manera cómoda de compartir datos, pero también puede plantear riesgos para la seguridad. Para evitar las infracciones de datos producidas por el acceso anónimo no deseado, Microsoft recomienda impedir el acceso público a una cuenta de almacenamiento a menos que su escenario lo requiera. |audit, deny, disabled |[3.0.1-preview](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/ASC_Storage_DisallowPublicBlobAccess_Audit.json) |

### <a name="ensure-the-storage-account-containing-the-container-with-activity-logs-is-encrypted-with-byok-use-your-own-key"></a>Asegúrese de que la cuenta de almacenamiento que contiene el contenedor con los registros de actividad está cifrada con BYOK (use su propia clave).

**Identificador**: CIS Azure 5.1.6 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[La cuenta de almacenamiento que contiene el contenedor con los registros de actividad debe estar cifrada con BYOK](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffbb99e8e-e444-4da0-9ff1-75c92f5a85b2) |Esta directiva audita si la cuenta de almacenamiento que contiene el contenedor con los registros de actividad está cifrada con BYOK. Esta directiva solo funciona si la cuenta de almacenamiento se encuentra en la misma suscripción que los registros de actividad por diseño. Se puede encontrar más información sobre el cifrado de Azure Storage en reposo en [https://aka.ms/azurestoragebyok](../../../storage/common/customer-managed-keys-configure-key-vault.md).  |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_StorageAccountBYOK_Audit.json) |

### <a name="ensure-that-logging-for-azure-keyvault-is-enabled"></a>Asegúrese de que el registro de Azure KeyVault esté habilitado.

**Identificador**: CIS Azure 5.1.7 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Los registros de recursos del HSM administrado de Azure Key Vault deben estar habilitados.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa2a5b911-5617-447e-a49e-59dbe0e0434b) |Para volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o cuando la red se ve comprometida, es posible que desee realizar auditorías habilitando los registros de recursos en HSM administrados. Siga las instrucciones que encontrará aquí: [https://docs.microsoft.com/azure/key-vault/managed-hsm/logging](../../../key-vault/managed-hsm/logging.md). |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Key%20Vault/ManagedHsm_AuditDiagnosticLog_Audit.json) |
|[Los registros de recursos de Key Vault deben estar habilitados](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fcf820ca0-f99e-4f3e-84fb-66e913812d21) |Habilitación de la auditoría de los registros de recursos. De esta forma, puede volver a crear seguimientos de actividad con fines de investigación en caso de incidentes de seguridad o riesgos para la red. |AuditIfNotExists, Disabled |[5.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Key%20Vault/KeyVault_AuditDiagnosticLog_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-create-policy-assignment"></a>Asegúrese de que existe una alerta de registro de actividad para la creación de una asignación de directiva.

**Identificador**: CIS Azure 5.2.1 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Debe existir una alerta de registro de actividad para las operaciones específicas de la directiva](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc5447c04-a4d7-4ba8-a263-c9ee321a6858) |Esta directiva audita las operaciones específicas de la directiva sin alertas de registro de actividad configuradas. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_PolicyOperations_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-create-or-update-network-security-group"></a>Asegúrese de que existe una alerta de registro de actividad para crear o actualizar el grupo de seguridad de red.

**Identificador**: CIS Azure 5.2.2 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Debe existir una alerta de registro de actividad para operaciones administrativas específicas](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb954148f-4c11-4c38-8221-be76711e194a) |Esta directiva audita operaciones administrativas específicas sin alertas de registro de actividad configuradas. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_AdministrativeOperations_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-delete-network-security-group"></a>Asegúrese de que existe una alerta de registro de actividad para eliminar el grupo de seguridad de red.

**Identificador**: CIS Azure 5.2.3 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Debe existir una alerta de registro de actividad para operaciones administrativas específicas](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb954148f-4c11-4c38-8221-be76711e194a) |Esta directiva audita operaciones administrativas específicas sin alertas de registro de actividad configuradas. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_AdministrativeOperations_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-create-or-update-network-security-group-rule"></a>Asegúrese de que existe una alerta de registro de actividad para la regla Crear o actualizar grupo de seguridad de red.

**Identificador**: CIS Azure 5.2.4 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Debe existir una alerta de registro de actividad para operaciones administrativas específicas](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb954148f-4c11-4c38-8221-be76711e194a) |Esta directiva audita operaciones administrativas específicas sin alertas de registro de actividad configuradas. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_AdministrativeOperations_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-the-delete-network-security-group-rule"></a>Asegúrese de que existe una alerta de registro de actividad para la regla Eliminar grupo de seguridad de red.

**Identificador**: CIS Azure 5.2.5 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Debe existir una alerta de registro de actividad para operaciones administrativas específicas](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb954148f-4c11-4c38-8221-be76711e194a) |Esta directiva audita operaciones administrativas específicas sin alertas de registro de actividad configuradas. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_AdministrativeOperations_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-create-or-update-security-solution"></a>Asegúrese de que existe una alerta de registro de actividad para la solución de seguridad de creación o actualización.

**Identificador**: CIS Azure 5.2.6 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Debe existir una alerta de registro de actividad para las operaciones específicas de seguridad](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3b980d31-7904-4bb7-8575-5665739a8052) |Esta directiva audita las operaciones específicas de seguridad sin alertas de registro de actividad configuradas. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_SecurityOperations_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-delete-security-solution"></a>Asegúrese de que existe una alerta de registro de actividad para la solución de seguridad de eliminación.

**Identificador**: CIS Azure 5.2.7 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Debe existir una alerta de registro de actividad para las operaciones específicas de seguridad](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3b980d31-7904-4bb7-8575-5665739a8052) |Esta directiva audita las operaciones específicas de seguridad sin alertas de registro de actividad configuradas. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_SecurityOperations_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-create-or-update-or-delete-sql-server-firewall-rule"></a>Asegúrese de que existe una alerta de registro de actividad para la regla de firewall Crear, actualizar o eliminar SQL Server.

**Identificador**: CIS Azure 5.2.8 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Debe existir una alerta de registro de actividad para operaciones administrativas específicas](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb954148f-4c11-4c38-8221-be76711e194a) |Esta directiva audita operaciones administrativas específicas sin alertas de registro de actividad configuradas. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_AdministrativeOperations_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-update-security-policy"></a>Asegúrese de que existe una alerta de registro de actividad para la actualización de la directiva de seguridad.

**Identificador**: CIS Azure 5.2.9 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Debe existir una alerta de registro de actividad para las operaciones específicas de seguridad](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3b980d31-7904-4bb7-8575-5665739a8052) |Esta directiva audita las operaciones específicas de seguridad sin alertas de registro de actividad configuradas. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_SecurityOperations_Audit.json) |

## <a name="networking"></a>Redes

### <a name="ensure-that-rdp-access-is-restricted-from-the-internet"></a>Asegúrese de que el acceso RDP está restringido desde Internet.

**Identificador**: CIS Azure 6.1 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[El acceso RDP desde Internet debe estar bloqueado](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe372f825-a257-4fb8-9175-797a8a8627d6) |Esta directiva audita cualquier regla de seguridad de red que permita el acceso RDP desde Internet. |Audit, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/NetworkSecurityGroup_RDPAccess_Audit.json) |

### <a name="ensure-that-ssh-access-is-restricted-from-the-internet"></a>Asegúrese de que el acceso SSH está restringido desde Internet.

**Identificador**: CIS Azure 6.2 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[El acceso SSH desde Internet debe estar bloqueado](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F2c89a2e5-7285-40fe-afe0-ae8654b92fab) |Esta directiva audita cualquier regla de seguridad de red que permita el acceso SSH desde Internet. |Audit, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/NetworkSecurityGroup_SSHAccess_Audit.json) |

### <a name="ensure-that-network-watcher-is-enabled"></a>Asegúrese de que Network Watcher está "Habilitado".

**Identificador**: CIS Azure 6.5 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Network Watcher debe estar habilitado](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb6e2945c-0b7b-40f5-9233-7a5323b5cdc6) |Network Watcher es un servicio regional que permite supervisar y diagnosticar problemas en un nivel de escenario de red mediante Azure. La supervisión del nivel de escenario permite diagnosticar problemas en una vista de nivel de red de un extremo a otro. Es preciso que se haya creado un grupo de recursos de Network Watcher en todas las regiones en las que haya una red virtual. Si algún grupo de recursos de Network Watcher no está disponible en una región determinada, se habilita una alerta. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/NetworkWatcher_Enabled_Audit.json) |

## <a name="virtual-machines"></a>Virtual Machines

### <a name="ensure-that-os-disk-are-encrypted"></a>Asegúrese de que "Disco del SO" esté cifrado.

**Identificador**: CIS Azure 7.1 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las máquinas virtuales deben cifrar los discos temporales, las cachés y los flujos de datos entre los recursos de Proceso y Almacenamiento](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0961003e-5a0a-4549-abde-af6a37f2724d) |De manera predeterminada, los discos del sistema operativo y de datos de una máquina virtual se cifran en reposo mediante claves administradas por la plataforma. Los discos temporales, las cachés de datos y los datos que fluyen entre el proceso y el almacenamiento no se cifran. Ignore esta recomendación si usa el cifrado en el host o El cifrado del lado servidor en Managed Disks cumple sus requisitos de seguridad. Obtenga más información en [Cifrado del lado servidor de Azure Disk Storage](../../../virtual-machines/disk-encryption.md) y [Diferentes ofertas de cifrado de disco](../../../virtual-machines/disk-encryption-overview.md#comparison). |AuditIfNotExists, Disabled |[2.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_UnencryptedVMDisks_Audit.json) |

### <a name="ensure-that-data-disks-are-encrypted"></a>Asegúrese de que la opción "Discos de datos" esté cifrada.

**Identificador**: CIS Azure 7.2 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las máquinas virtuales deben cifrar los discos temporales, las cachés y los flujos de datos entre los recursos de Proceso y Almacenamiento](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0961003e-5a0a-4549-abde-af6a37f2724d) |De manera predeterminada, los discos del sistema operativo y de datos de una máquina virtual se cifran en reposo mediante claves administradas por la plataforma. Los discos temporales, las cachés de datos y los datos que fluyen entre el proceso y el almacenamiento no se cifran. Ignore esta recomendación si usa el cifrado en el host o El cifrado del lado servidor en Managed Disks cumple sus requisitos de seguridad. Obtenga más información en [Cifrado del lado servidor de Azure Disk Storage](../../../virtual-machines/disk-encryption.md) y [Diferentes ofertas de cifrado de disco](../../../virtual-machines/disk-encryption-overview.md#comparison). |AuditIfNotExists, Disabled |[2.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_UnencryptedVMDisks_Audit.json) |

### <a name="ensure-that-unattached-disks-are-encrypted"></a>Asegúrese de que la opción "Unattached disks" (Discos no asignados) esté cifrada.

**Identificador**: CIS Azure 7.3 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Los discos sin asignar deben cifrarse](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F2c89a2e5-7285-40fe-afe0-ae8654b92fb2) |Esta directiva audita cualquier disco sin adjuntar y que no tenga el cifrado habilitado. |Audit, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Compute/UnattachedDisk_Encryption_Audit.json) |

### <a name="ensure-that-only-approved-extensions-are-installed"></a>Asegúrese de que solo están instaladas las extensiones aprobadas.

**Identificador**: CIS Azure 7.4 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Solo deben instalarse las extensiones de máquina virtual aprobadas](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc0e996f8-39cf-4af9-9f45-83fbde810432) |Esta directiva rige las extensiones de máquina virtual que no están aprobadas. |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Compute/VirtualMachines_ApprovedExtensions_Audit.json) |

### <a name="ensure-that-the-latest-os-patches-for-all-virtual-machines-are-applied"></a>Asegúrese de que se aplican las revisiones del sistema operativo más recientes para todas las máquinas virtuales.

**Identificador**: CIS Azure 7.5 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Se deben instalar actualizaciones del sistema en las máquinas](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F86b3d65f-7626-441e-b690-81a8b71cff60) |Azure Security Center supervisará las actualizaciones del sistema de seguridad que faltan en los servidores como recomendaciones. |AuditIfNotExists, Disabled |[4.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_MissingSystemUpdates_Audit.json) |

### <a name="ensure-that-the-endpoint-protection-for-all-virtual-machines-is-installed"></a>Asegúrese de que Endpoint Protection esté instalado para todas las máquinas virtuales.

**Identificador**: CIS Azure 7.6 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Supervisar la falta de Endpoint Protection en Azure Security Center](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Faf6cd1bd-1635-48cb-bde7-5b15693900b9) |Azure Security Center supervisará los servidores sin un agente de Endpoint Protection instalado como recomendaciones. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_MissingEndpointProtection_Audit.json) |

## <a name="other-security-considerations"></a>Otras consideraciones de seguridad

### <a name="ensure-that-the-expiration-date-is-set-on-all-keys"></a>Asegúrese de que la fecha de expiración se haya establecida en todas las claves

**Identificador**: CIS Azure 8.1 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Las claves de Key Vault deben tener una fecha de expiración](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F152b15f7-8e1f-4c1f-ab71-8c010ba5dbc0) |Las claves criptográficas deben tener una fecha de expiración definida y no ser permanentes. Las claves que no expiran proporcionan a los posibles atacantes más tiempo para hacerse con ellas. Por ello, se recomienda como práctica de seguridad establecer fechas de expiración en las claves criptográficas. |Audit, Deny, Disabled |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Key%20Vault/Keys_ExpirationSet.json) |

### <a name="ensure-that-the-expiration-date-is-set-on-all-secrets"></a>Asegúrese de que la fecha de expiración se haya establecido en todos los secretos

**Identificador**: CIS Azure 8.2 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Los secretos de Key Vault deben tener una fecha de expiración](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F98728c90-32c7-4049-8429-847dc0f4fe37) |Los secretos deben tener una fecha de expiración definida y no ser permanentes. Los secretos que no expiran proporcionan a un posible atacante más tiempo para ponerlos en peligro. Por ello, se recomienda como práctica de seguridad establecer fechas de expiración en los secretos. |Audit, Deny, Disabled |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Key%20Vault/Secrets_ExpirationSet.json) |

### <a name="ensure-the-key-vault-is-recoverable"></a>Asegúrese de que el almacén de claves se puede recuperar.

**Identificador**: CIS Azure 8.4 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[El HSM administrado de Azure Key Vault debe tener habilitada la protección contra purgas.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc39ba22d-4428-4149-b981-70acb31fc383) |La eliminación malintencionada de un HSM administrado de Azure Key Vault puede provocar una pérdida de datos permanente. Un usuario malintencionado de la organización puede eliminar y purgar HSM administrados de Azure Key Vault. La protección contra purgas le protege frente a ataques internos mediante la aplicación de un período de retención obligatorio para HSM administrados de Azure Key Vault eliminados temporalmente. Ningún usuario de su organización o Microsoft podrá purgar HSM administrados de Azure Key Vault durante el período de retención de eliminación temporal. |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Key%20Vault/ManagedHsm_Recoverable_Audit.json) |
|[Los almacenes de claves deben tener habilitada la protección contra operaciones de purga](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0b60c0b2-2dc2-4e1c-b5c9-abbed971de53) |La eliminación malintencionada de un almacén de claves puede provocar una pérdida de datos permanente. Un usuario malintencionado de la organización puede eliminar y purgar los almacenes de claves. La protección contra purgas le protege frente a ataques internos mediante la aplicación de un período de retención obligatorio para almacenes de claves eliminados temporalmente. Ningún usuario de su organización o Microsoft podrá purgar los almacenes de claves durante el período de retención de eliminación temporal. |Audit, Deny, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Key%20Vault/KeyVault_Recoverable_Audit.json) |

### <a name="enable-role-based-access-control-rbac-within-azure-kubernetes-services"></a>Habilite el control de acceso basado en rol (RBAC) en Azure Kubernetes Services.

**Identificador**: CIS Azure 8.5 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Se debe usar el control de acceso basado en rol (RBAC) en los servicios de Kubernetes](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fac4a19c2-fa67-49b4-8ae5-0b2e78c49457) |Para proporcionar un filtrado detallado de las acciones que los usuarios pueden realizar, use el control de acceso basado en rol (RBAC) para administrar los permisos en los clústeres de Kubernetes Service y configurar las directivas de autorización correspondientes. |Audit, Disabled |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableRBAC_KubernetesService_Audit.json) |

## <a name="appservice"></a>AppService

### <a name="ensure-app-service-authentication-is-set-on-azure-app-service"></a>Asegúrese de que la autenticación de App Service está establecida en Azure App Service.

**Identificador**: CIS Azure 9.1 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[La autenticación debe estar habilitada en la aplicación de API](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc4ebc54a-46e1-481a-bee2-d4411e95d828) |La autenticación de Azure App Service es una característica que puede impedir que solicitudes HTTP anónimas lleguen a la aplicación de API o autenticar aquellas que tienen tokens antes de que lleguen a la aplicación de API. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_Authentication_ApiApp_Audit.json) |
|[La autenticación debe estar habilitada en la aplicación de funciones](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc75248c1-ea1d-4a9c-8fc9-29a6aabd5da8) |La autenticación de Azure App Service es una característica que puede impedir que solicitudes HTTP anónimas lleguen a la aplicación de funciones o autenticar aquellas que tienen tokens antes de que lleguen a la aplicación de funciones. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_Authentication_functionapp_Audit.json) |
|[La autenticación debe estar habilitada en la aplicación web](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F95bccee9-a7f8-4bec-9ee9-62c3473701fc) |La autenticación de Azure App Service es una característica que puede impedir que solicitudes HTTP anónimas lleguen a la aplicación web o autenticar aquellas que tienen tokens antes de que lleguen a la aplicación web. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_Authentication_WebApp_Audit.json) |

### <a name="ensure-web-app-redirects-all-http-traffic-to-https-in-azure-app-service"></a>Asegúrese de que la aplicación web redirige todo el tráfico HTTP a HTTPS en Azure App Service.

**Identificador**: CIS Azure 9.2 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Acceso a la aplicación web solo a través de HTTPS](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa4af4a39-4135-47fb-b175-47fbdf85311d) |El uso de HTTPS garantiza la autenticación del servicio y el servidor, y protege los datos en tránsito frente a ataques de intercepción de nivel de red. |Audit, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppServiceWebapp_AuditHTTP_Audit.json) |

### <a name="ensure-web-app-is-using-the-latest-version-of-tls-encryption"></a>Asegúrese de que la aplicación web usa la versión más reciente del cifrado TLS.

**Identificador**: CIS Azure 9.3 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Debe usarse la versión más reciente de TLS en la aplicación de API](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F8cb6aa8b-9e41-4f4e-aa25-089a7ac2581e) |Actualiza a la versión más reciente de TLS. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_RequireLatestTls_ApiApp_Audit.json) |
|[Debe usarse la versión más reciente de TLS en la aplicación de funciones](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff9d614c5-c173-4d56-95a7-b4437057d193) |Actualiza a la versión más reciente de TLS. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_RequireLatestTls_FunctionApp_Audit.json) |
|[Debe usarse la versión más reciente de TLS en la aplicación web](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff0e6e85b-9b9f-4a4b-b67b-f730d42f1b0b) |Actualiza a la versión más reciente de TLS. |AuditIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_RequireLatestTls_WebApp_Audit.json) |

### <a name="ensure-the-web-app-has-client-certificates-incoming-client-certificates-set-to-on"></a>Asegúrese de que la aplicación web tenga la opción "Client Certificates (Incoming client certificates)" activada.

**Identificador**: CIS Azure 9.4 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Asegúrese de que la aplicación de API tenga la opción "Client Certificates (Incoming client certificates)" activada](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0c192fe8-9cbb-4516-85b3-0ade8bd03886) |Los certificados de cliente permiten que la aplicación solicite un certificado para las solicitudes entrantes. Solo los clientes que tienen un certificado válido podrán acceder a la aplicación. |Audit, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_ApiApp_Audit_ClientCert.json) |
|[Asegúrese de que la aplicación web tenga la opción "Client Certificates (Incoming client certificates)" activada](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5bb220d9-2698-4ee4-8404-b9c30c9df609) |Los certificados de cliente permiten que la aplicación solicite un certificado para las solicitudes entrantes. Solo los clientes que tienen un certificado válido podrán acceder a la aplicación. |Audit, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_Webapp_Audit_ClientCert.json) |
|[Las aplicaciones de funciones deben tener la opción "Certificados de cliente (certificados de cliente entrantes)" habilitada](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Feaebaea7-8013-4ceb-9d14-7eb32271373c) |Los certificados de cliente permiten que la aplicación solicite un certificado para las solicitudes entrantes. Solo los clientes con certificados válidos pueden acceder a la aplicación. |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_FunctionApp_Audit_ClientCert.json) |

### <a name="ensure-that-register-with-azure-active-directory-is-enabled-on-app-service"></a>Asegúrese de que el registro con Azure Active Directory esté habilitado en App Service.

**Identificador**: CIS Azure 9.5 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[La identidad administrada debe usarse en la aplicación de API](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc4d441f8-f9d9-4a9e-9cef-e82117cb3eef) |Usa una identidad administrada para la seguridad de autenticación mejorada. |AuditIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_UseManagedIdentity_ApiApp_Audit.json) |
|[La identidad administrada debe usarse en la aplicación de funciones](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0da106f2-4ca3-48e8-bc85-c638fe6aea8f) |Usa una identidad administrada para la seguridad de autenticación mejorada. |AuditIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_UseManagedIdentity_FunctionApp_Audit.json) |
|[La identidad administrada debe usarse en la aplicación web](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F2b9ad585-36bc-4615-b300-fd4435808332) |Usa una identidad administrada para la seguridad de autenticación mejorada. |AuditIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_UseManagedIdentity_WebApp_Audit.json) |

### <a name="ensure-that-php-version-is-the-latest-if-used-to-run-the-web-app"></a>Asegúrese de que la "Versión de PHP" es la más reciente si se usa para ejecutar la aplicación web.

**Identificador**: CIS Azure 9.7 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Asegurarse de que la "Versión de PHP" es la más reciente, si se usa como parte de la aplicación de API](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1bc1795e-d44a-4d48-9b3b-6fff0fd5f9ba) |A causa de errores de seguridad o para incluir funcionalidades, se publican de forma periódica versiones más recientes del software de PHP. Para las aplicaciones de API, se recomienda usar la versión más reciente de PHP con el fin de aprovechar las correcciones de seguridad, de haberlas, o las nuevas funcionalidades de la versión más reciente. Actualmente, esta directiva solo se aplica a las aplicaciones web de Linux. |AuditIfNotExists, Disabled |[2.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_ApiApp_Audit_PHP_Latest.json) |
|[Asegúrese de que la "Versión de PHP" es la más reciente, si se usa como parte de la aplicación web](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7261b898-8a84-4db8-9e04-18527132abb3) |A causa de errores de seguridad o para incluir funcionalidades, se publican de forma periódica versiones más recientes del software de PHP. Para las aplicaciones web, se recomienda usar la versión más reciente de PHP con el fin de aprovechar las correcciones de seguridad, de haberlas, o las nuevas funcionalidades de la versión más reciente. Actualmente, esta directiva solo se aplica a las aplicaciones web de Linux. |AuditIfNotExists, Disabled |[2.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_Webapp_Audit_PHP_Latest.json) |

### <a name="ensure-that-python-version-is-the-latest-if-used-to-run-the-web-app"></a>Asegúrese de que la "Versión de Python" es la más reciente si se usa para ejecutar la aplicación web.

**Identificador**: CIS Azure 9.8 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Asegurarse de que la "Versión de Python" es la más reciente, si se usa como parte de la aplicación de API](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F74c3584d-afae-46f7-a20a-6f8adba71a16) |A causa de errores de seguridad o para incluir funcionalidades, se publican de forma periódica versiones más recientes del software de Python. Para las aplicaciones de API, se recomienda usar la versión más reciente de Python con el fin de aprovechar las correcciones de seguridad, de haberlas, o las nuevas funcionalidades de la versión más reciente. Actualmente, esta directiva solo se aplica a las aplicaciones web de Linux. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_ApiApp_Audit_python_Latest.json) |
|[Asegúrese de que la "Versión de Python" es la más reciente, si se usa como parte de la aplicación de funciones](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7238174a-fd10-4ef0-817e-fc820a951d73) |A causa de errores de seguridad o para incluir funcionalidades, se publican de forma periódica versiones más recientes del software de Python. Para las aplicaciones de funciones, se recomienda usar la versión más reciente de Python con el fin de aprovechar las correcciones de seguridad, de haberlas, o las nuevas funcionalidades de la versión más reciente. Actualmente, esta directiva solo se aplica a las aplicaciones web de Linux. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_FunctionApp_Audit_python_Latest.json) |
|[Asegúrese de que la "Versión de Python" es la más reciente, si se usa como parte de la aplicación web](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7008174a-fd10-4ef0-817e-fc820a951d73) |A causa de errores de seguridad o para incluir funcionalidades, se publican de forma periódica versiones más recientes del software de Python. Para las aplicaciones web, se recomienda usar la versión más reciente de Python con el fin de aprovechar las correcciones de seguridad, de haberlas, o las nuevas funcionalidades de la versión más reciente. Actualmente, esta directiva solo se aplica a las aplicaciones web de Linux. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_WebApp_Audit_python_Latest.json) |

### <a name="ensure-that-java-version-is-the-latest-if-used-to-run-the-web-app"></a>Asegúrese de que la "Versión de Java" es la más reciente si se usa para ejecutar la aplicación web.

**Identificador**: CIS Azure 9.9 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Asegurarse de que la "Versión de Java" es la más reciente, si se usa como parte de la aplicación de API](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F88999f4c-376a-45c8-bcb3-4058f713cf39) |A causa de errores de seguridad o para incluir funcionalidades, se publican de forma periódica versiones más recientes de Java. Para las aplicaciones de API, se recomienda usar la versión más reciente de Python con el fin de aprovechar las correcciones de seguridad, de haberlas, o las nuevas funcionalidades de la versión más reciente. Actualmente, esta directiva solo se aplica a las aplicaciones web de Linux. |AuditIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_ApiApp_Audit_java_Latest.json) |
|[Asegúrese de que la versión de Java sea la más reciente, si se usa como parte de la aplicación de funciones](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F9d0b6ea4-93e2-4578-bf2f-6bb17d22b4bc) |A causa de errores de seguridad o para incluir funcionalidades, se publican de forma periódica versiones más recientes del software de Java. Para las aplicaciones de funciones, se recomienda usar la versión más reciente de Java con el fin de aprovechar las correcciones de seguridad, de haberlas, o las nuevas funcionalidades de la versión más reciente. Actualmente, esta directiva solo se aplica a las aplicaciones web de Linux. |AuditIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_FunctionApp_Audit_java_Latest.json) |
|[Asegúrese de que la "Versión de Java" es la más reciente, si se usa como parte de la aplicación web](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F496223c3-ad65-4ecd-878a-bae78737e9ed) |A causa de errores de seguridad o para incluir funcionalidades, se publican de forma periódica versiones más recientes del software de Java. Para las aplicaciones web, se recomienda usar la versión más reciente de Java con el fin de aprovechar las correcciones de seguridad, de haberlas, o las nuevas funcionalidades de la versión más reciente. Actualmente, esta directiva solo se aplica a las aplicaciones web de Linux. |AuditIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_WebApp_Audit_java_Latest.json) |

### <a name="ensure-that-http-version-is-the-latest-if-used-to-run-the-web-app"></a>Asegúrese de que la "Versión de HTTP" es la más reciente, si se usa para ejecutar la aplicación web.

**Identificador**: CIS Azure 9.10 **Propiedad**: Customer

|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Asegurarse de que la "Versión de HTTP" es la más reciente, si se usa para ejecutar la aplicación de API](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F991310cd-e9f3-47bc-b7b6-f57b557d07db) |A causa de errores de seguridad o para incluir funcionalidades, se publican de forma periódica versiones más recientes de HTTP. Para las aplicaciones web, use la versión más reciente de HTTP con el fin de aprovechar las correcciones de seguridad, de haberlas, o las nuevas funcionalidades de la versión más reciente. Actualmente, esta directiva solo se aplica a las aplicaciones web de Linux. |AuditIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_ApiApp_Audit_HTTP_Latest.json) |
|[Asegúrese de que la "Versión de HTTP" es la más reciente, si se usa para ejecutar la aplicación de funciones](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe2c1c086-2d84-4019-bff3-c44ccd95113c) |A causa de errores de seguridad o para incluir funcionalidades, se publican de forma periódica versiones más recientes de HTTP. Para las aplicaciones web, use la versión más reciente de HTTP con el fin de aprovechar las correcciones de seguridad, de haberlas, o las nuevas funcionalidades de la versión más reciente. Actualmente, esta directiva solo se aplica a las aplicaciones web de Linux. |AuditIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_FunctionApp_Audit_HTTP_Latest.json) |
|[Asegúrese de que la "Versión de HTTP" es la más reciente, si se usa para ejecutar la aplicación web](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F8c122334-9d20-4eb8-89ea-ac9a705b74ae) |A causa de errores de seguridad o para incluir funcionalidades, se publican de forma periódica versiones más recientes de HTTP. Para las aplicaciones web, use la versión más reciente de HTTP con el fin de aprovechar las correcciones de seguridad, de haberlas, o las nuevas funcionalidades de la versión más reciente. Actualmente, esta directiva solo se aplica a las aplicaciones web de Linux. |AuditIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_WebApp_Audit_HTTP_Latest.json) |

## <a name="next-steps"></a>Pasos siguientes

Artículos adicionales sobre Azure Policy:

- Introducción al [Cumplimiento normativo](../concepts/regulatory-compliance.md).
- Consulte la [estructura de definición de la iniciativa](../concepts/initiative-definition-structure.md).
- Consulte más ejemplos en [Ejemplos de Azure Policy](./index.md).
- Vea la [Descripción de los efectos de directivas](../concepts/effects.md).
- Obtenga información sobre cómo [corregir recursos no compatibles](../how-to/remediate-resources.md).