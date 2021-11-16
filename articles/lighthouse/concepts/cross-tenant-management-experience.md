---
title: Experiencias de administración entre inquilinos
description: Azure Lighthouse habilita y mejora las experiencias entre inquilinos en muchos servicios de Azure.
ms.date: 10/21/2021
ms.topic: conceptual
ms.openlocfilehash: a9dbf51a965c1aa73489fdd2990736914b27177c
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132312464"
---
# <a name="cross-tenant-management-experiences"></a>Experiencias de administración entre inquilinos

Como proveedor de servicios, puede usar [Azure Lighthouse](../overview.md) para administrar los recursos para varios clientes desde su propio inquilino de Azure Active Directory (Azure AD). Muchas tareas y servicios se pueden realizar en inquilinos administrados mediante la [administración de recursos delegados de Azure](../concepts/architecture.md).

> [!TIP]
> Azure Lighthouse también se puede usar [en una empresa que tenga varios inquilinos de Azure AD propios](enterprise.md) para simplificar la administración entre inquilinos.

## <a name="understanding-tenants-and-delegation"></a>Descripción de los inquilinos y la delegación

Un inquilino de Azure AD es la representación de una organización. Se trata de una instancia dedicada de Azure AD que una organización recibe cuando crea una relación con Microsoft al registrarse en Azure, Microsoft 365 u otros servicios. Cada inquilino de Azure AD es distinto e independiente de los demás inquilinos de Azure AD y tiene su propio identificador de inquilino (un GUID). Para más información, consulte [¿Qué es Azure Active Directory?](../../active-directory/fundamentals/active-directory-whatis.md)

Normalmente, para administrar los recursos de Azure para un cliente, los proveedores de servicios tendrían que iniciar sesión en Azure Portal mediante una cuenta asociada al inquilino de ese cliente, lo que obliga a que un administrador del inquilino del cliente cree y administre cuentas de usuario. para el proveedor de servicios.

Con Azure Lighthouse, el proceso de incorporación especifica los usuarios del inquilino del proveedor de servicios que podrán trabajar con suscripciones delegadas y grupos de recursos en el inquilino del cliente. A continuación, estos usuarios pueden iniciar sesión en Azure Portal con sus propias credenciales. En Azure Portal, pueden administrar los recursos que pertenecen a todos los clientes a los que tienen acceso. Puede hacerse desde la página [Mis clientes](../how-to/view-manage-customers.md) de Azure Portal o trabajando directamente en el contexto de la suscripción de ese cliente, ya sea en Azure Portal o a través de las API.

Azure Lighthouse permite una mayor flexibilidad para administrar los recursos de varios clientes sin tener que iniciar sesión en diferentes cuentas en distintos inquilinos. Por ejemplo, un proveedor de servicios puede tener dos clientes con diferentes responsabilidades y niveles de acceso. Con Azure Lighthouse, los usuarios autorizados pueden iniciar sesión en el inquilino del proveedor de servicios para acceder a estos recursos.

![Diagrama que muestra los recursos de cliente administrados mediante un inquilino de proveedor de servicios.](../media/azure-delegated-resource-management-service-provider-tenant.jpg)

## <a name="apis-and-management-tool-support"></a>Compatibilidad de las API y herramientas de administración

Puede realizar tareas de administración en recursos delegados directamente en el portal o mediante las API y herramientas de administración (como la CLI de Azure y Azure PowerShell). Todas las API existentes se pueden usar al trabajar con recursos delegados, siempre y cuando la funcionalidad sea compatible con la administración entre inquilinos y el usuario tenga los permisos adecuados.

De forma predeterminada, el [cmdlet Get-AzSubscription](/powershell/module/Az.Accounts/Get-AzSubscription) de Azure PowerShell mostrará el valor `TenantId` del inquilino de administración. Puede usar los atributos `HomeTenantId` y `ManagedByTenantIds` de cada suscripción, lo que le permite identificar si una suscripción devuelta pertenece a un inquilino administrado o a un inquilino que administra.

Del mismo modo, los comandos de la CLI de Azure como [az account list](/cli/azure/account#az_account_list) muestran los atributos `homeTenantId` y `managedByTenants`. Si no ve estos valores al usar la CLI de Azure, intente borrar la memoria caché ejecutando `az account clear` seguido de `az login --identity`.

En la API REST de Azure, los comandos [Subscriptions - Get](/rest/api/resources/subscriptions/get) y [Subscriptions - List](/rest/api/resources/subscriptions/list) incluyen `ManagedByTenant`.

> [!NOTE]
> Además de la información de inquilinos relacionada con Azure Lighthouse, los inquilinos que se muestran en estas API también pueden reflejar los inquilinos de asociados de Azure Databricks o de las aplicaciones administradas de Azure.

También se proporcionan API específicas para realizar tareas de administración de Azure Lighthouse. Para obtener más información, consulte la sección **Referencia**.

## <a name="enhanced-services-and-scenarios"></a>Servicios y escenarios mejorados

La mayoría de las tareas y servicios se pueden realizar en recursos delegados a través de inquilinos administrados. A continuación se muestran algunos de los escenarios clave en los que la administración entre inquilinos puede ser especialmente eficaz.

[Azure Arc](../../azure-arc/index.yml):

- Administración de servidores híbridos a gran escala - [Servidores habilitados para Azure Arc](../../azure-arc/servers/overview.md):
  - [Administre máquinas con Windows Server o Linux fuera de Azure conectadas](../../azure-arc/servers/onboard-portal.md) a suscripciones o grupos de recursos delegados en Azure.
  - Administre las máquinas conectadas mediante construcciones de Azure, como Azure Policy y etiquetado
  - Asegúrese de que se aplica el mismo conjunto de directivas a los entornos híbridos de los clientes
  - Uso de Microsoft Defender for Cloud para supervisar el cumplimiento en entornos híbridos de clientes
- Administración de clústeres híbridos de Kubernetes a gran escala: [Kubernetes habilitado para Azure Arc (versión preliminar)](../../azure-arc/kubernetes/overview.md):
  - [Administre clústeres de Kubernetes conectados](../../azure-arc/kubernetes/quickstart-connect-cluster.md) a suscripciones o grupos de recursos delegados en Azure.
  - [Use GitOps](../../azure-arc/kubernetes/tutorial-use-gitops-connected-cluster.md) para los clústeres conectados
  - Aplique directivas en los clústeres conectados

[Azure Automation](../../automation/index.yml):

- Use cuentas de Automation para acceder a recursos delegados y trabajar con ellos.

[Azure Backup](../../backup/index.yml):

- Realice una copia de seguridad de los datos de clientes y restáurelos [desde cargas de trabajo locales, máquinas virtuales de Azure, recursos compartidos de archivos de Azure y mucho más](../..//backup/backup-overview.md#what-can-i-back-up).
- Consulte datos de todos los recursos de cliente delegados en el [Centro de copia de seguridad](../../backup/backup-center-overview.md).
- Use el [Explorador de copia de seguridad](../../backup/monitor-azure-backup-with-backup-explorer.md) para ayudar a ver la información operativa de los elementos de copia de seguridad (incluidos los recursos de Azure que todavía no están configurados para la copia de seguridad) y la información de supervisión (trabajos y alertas) de las suscripciones delegadas. El Explorador de Backup solo está disponible actualmente para los datos de máquinas virtuales de Azure.
- Use [informes de copia de seguridad](../../backup/configure-reports.md) a través de suscripciones delegadas para realizar un seguimiento de tendencias históricas, analizar el consumo de almacenamiento de copia de seguridad y auditar copias de seguridad y restauraciones.

[Azure Blueprints](../../governance/blueprints/index.yml).

- Use Azure Blueprints para organizar la implementación de plantillas de recursos y otros artefactos (requiere [acceso adicional](https://www.wesleyhaakman.org/preparing-azure-lighthouse-customer-subscriptions-for-azure-blueprints/) para preparar la suscripción de cliente).

[Azure Cost Management y facturación](../../cost-management-billing/index.yml):

- En el inquilino de administración, los asociados de CSP pueden ver, administrar y analizar los costos de consumo antes de impuestos (sin incluir compras) de los clientes que se encuentran en el plan de Azure. El costo se basará en las tarifas comerciales y el acceso de control de acceso basado en rol de Azure (RBAC de Azure) que tiene el asociado para la suscripción del cliente. Actualmente, puede ver los costos de consumo de las tarifas comerciales de cada suscripción de cliente individual en función del acceso RBAC de Azure.

[Azure Key Vault](../../key-vault/general/index.yml):

- Creación de almacenes de claves en inquilinos de clientes
- Uso de una identidad administrada para crear almacenes de claves en inquilinos de clientes

[Azure Kubernetes Service (AKS)](../../aks/index.yml):

- administre los entornos de Kubernetes hospedados e implemente y administre aplicaciones en contenedores en inquilinos de clientes
- Implementación y administración de clústeres en inquilinos de clientes
- Uso de Azure Monitor para que los contenedores supervisen el rendimiento en los inquilinos de cliente

[Azure Migrate](../../migrate/index.yml):

- Crear proyectos de migración en el inquilino del cliente y migrar máquinas virtuales

[Azure Monitor](../../azure-monitor/index.yml):

- Ver las alertas de las suscripciones delegadas, con la capacidad de ver y actualizar alertas en todas las suscripciones
- Ver los detalles del registro de actividad para las suscripciones delegadas
- [Log Analytics](../../azure-monitor/logs/service-providers.md): consulte datos de áreas de trabajo remotas situadas en varios inquilinos (tenga en cuenta que las cuentas de Automation que se usan para acceder a datos desde áreas de trabajo que se encuentran en inquilinos de cliente deben crearse en el mismo inquilino).
- Cree, vea y administre [alertas de métricas](../../azure-monitor/alerts/alerts-metric.md), [alertas de registro](../../azure-monitor/alerts/alerts-log.md) y [alertas de registros de actividad](../../azure-monitor/alerts/alerts-activity-log.md) en los inquilinos del cliente.
- Cree alertas en inquilinos de cliente que desencadenen la automatización, como los runbooks de Azure Automation o Azure Functions, en el inquilino que realiza la administración a través de webhooks
- Creación de una [configuración de diagnóstico](../..//azure-monitor/essentials/diagnostic-settings.md) en áreas de trabajo creadas en inquilinos de cliente para enviar registros de recursos a áreas de trabajo en el inquilino de administración
- Para las cargas de trabajo de SAP, [supervise las métricas de soluciones de SAP con una vista agregada a través de los inquilinos del cliente](https://techcommunity.microsoft.com/t5/running-sap-applications-on-the/using-azure-lighthouse-and-azure-monitor-for-sap-solutions-to/ba-p/1537293).
- Para Azure AD B2C, [enrute los registros de auditoría e inicios de sesión](../../active-directory-b2c/azure-monitor.md) a otras soluciones de supervisión.

[Redes de Azure](../../networking/fundamentals/networking-overview.md):

- Implementación y administración de [Azure Virtual Network](../../virtual-network/index.yml) y las tarjetas de interfaz de red virtual (vNIC) en inquilinos administrados
- Implementación y configuración de [Azure Firewall](../../firewall/overview.md) para proteger los recursos de Virtual Network de los clientes
- Administración de los servicios de conectividad como [Azure Virtual WAN](../../virtual-wan/virtual-wan-about.md), [ExpressRoute](../../expressroute/expressroute-introduction.md) y [VPN Gateway](../../vpn-gateway/vpn-gateway-about-vpngateways.md)
- Uso de Azure Lighthouse para admitir escenarios clave para el [programa MSP de redes de Azure](../../networking/networking-partners-msp.md)

[Azure Policy](../../governance/policy/index.yml):

- Creación y edición de definiciones de directivas en una suscripción delegada
- Implemente definiciones y asignaciones de directivas en varios inquilinos.
- Asignación de definiciones de directivas definidas por el cliente en suscripciones delegadas
- Los clientes ven las directivas creadas por el proveedor de servicios junto con las directivas que hayan creado ellos mismos.
- Puede [corregir deployIfNotExists o modificar asignaciones en el inquilino administrado](../how-to/deploy-policy-remediation.md)
- Tenga en cuenta que actualmente no se admite la visualización de detalles de cumplimiento de recursos no compatibles en inquilinos de clientes.

[Azure Resource Graph](../../governance/resource-graph/index.yml)

- Ahora incluye el identificador de inquilino en los resultados de la consulta devueltos, lo que le permite identificar si una suscripción pertenece a un inquilino administrado.

[Microsoft Defender for Cloud](../../security-center/index.yml):

- Visibilidad entre inquilinos
  - Supervise el cumplimiento de las directivas de seguridad y garantice la cobertura de seguridad en todos los recursos de los inquilinos.
  - Supervisión continua del cumplimiento normativo en varios inquilinos en una sola vista
  - Supervisión, evaluación de prioridades y priorización de recomendaciones de seguridad procesables con el cálculo de puntuaciones seguras
- Administración de posiciones de seguridad entre inquilinos
  - Administrar directivas de seguridad
  - Acciones sobre los recursos que no cumplen las recomendaciones de seguridad procesables
  - Recopilación y almacenamiento de datos relacionados con la seguridad
- Detección y protección de amenazas entre inquilinos
  - Detección de amenazas entre los recursos de los inquilinos
  - Aplicación de controles de protección contra amenazas avanzados, como el acceso a VM Just-in-Time (JIT)
  - Protección de la configuración de grupos de seguridad de red con refuerzo de redes adaptable
  - Comprobación de que los servidores ejecutan solo las aplicaciones y los procesos que deben con controles de aplicaciones adaptables
  - Supervisar los cambios en archivos importantes y entradas del registro con la supervisión de la integridad de los archivos (FIM)
- Tenga en cuenta que toda la suscripción se debe delegar en el inquilino de administración; no se admiten escenarios de Microsoft Defender for Cloud con grupos de recursos delegados

[Microsoft Sentinel](../../sentinel/multiple-tenants-service-providers.md):

- Administración de recursos de Microsoft Sentinel [en inquilinos del cliente](../../sentinel/multiple-tenants-service-providers.md)
- [Seguimiento de ataques y visualización de alertas de seguridad en varios inquilinos](https://techcommunity.microsoft.com/t5/azure-sentinel/using-azure-lighthouse-and-azure-sentinel-to-monitor-across/ba-p/1043899)
- [Visualización de incidentes](../../sentinel/multiple-workspace-view.md) en varias áreas de trabajo de Microsoft Sentinel distribuidas entre inquilinos

[Azure Service Health](../../service-health/index.yml):

- Supervisar el estado de los recursos del cliente con Azure Resource Health
- Hacer un seguimiento del estado de los servicios de Azure que usan los clientes

[Azure Site Recovery](../../site-recovery/index.yml):

- Administración de opciones de recuperación ante desastres para máquinas virtuales de Azure en inquilinos de clientes (tenga en cuenta que no puede usar cuentas `RunAs` para copiar extensiones de VM)

[Azure Virtual Machines](../../virtual-machines/index.yml):

- Uso de extensiones de máquina virtual para la configuración posterior a la implementación y la automatización de tareas en máquinas virtuales de Azure
- Uso de diagnósticos de arranque para solucionar problemas de máquinas virtuales de Azure
- Acceso a máquinas virtuales con la consola serie
- Integración de máquinas virtuales con Azure Key Vault para obtener contraseñas, secretos o claves criptográficas para el cifrado de discos mediante la [identidad administrada a través de directivas](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/create-keyvault-secret), lo que garantizará que los secretos se almacenen en una instancia de Key Vault de los inquilinos administrados
- Tenga en cuenta que no puede usar Azure Active Directory para el inicio de sesión remoto en las máquinas virtuales

Solicitudes de soporte técnico:

- [Apertura de solicitudes de soporte desde la sección **Ayuda y soporte técnico**](../../azure-portal/supportability/how-to-create-azure-support-request.md#getting-started) en Azure Portal para los recursos delegados (selección del plan de soporte técnico disponible para el ámbito delegado)
- Use [Azure Quota API](/rest/api/reserved-vm-instances/quotaapi) para ver y administrar las cuotas de servicio de Azure para los recursos de cliente delegados

## <a name="current-limitations"></a>Limitaciones actuales

Con todos los escenarios, tenga en cuenta las siguientes limitaciones actuales:

- Las solicitudes controladas por Azure Resource Manager pueden realizarse mediante Azure Lighthouse. Los URI de operación de estas solicitudes comienzan por `https://management.azure.com`. Sin embargo, las solicitudes que se administran mediante una instancia de un tipo de recurso (por ejemplo, acceso a los secretos de Key Vault o acceso a datos de almacenamiento) no son compatibles con Azure Lighthouse. Los URI de la operación para estas solicitudes suelen empezar con una dirección que es única para la instancia, como `https://myaccount.blob.core.windows.net` o `https://mykeyvault.vault.azure.net/`. Esta última también suelen ser operaciones de datos, en lugar de operaciones de administración.
- Las asignaciones de roles deben usar [roles integrados de Azure](../../role-based-access-control/built-in-roles.md). Actualmente se admiten todos los roles integrados con Azure Lighthouse, excepto el rol de Propietario y los roles integrados con el permiso [`DataActions`](../../role-based-access-control/role-definitions.md#dataactions). El rol administrador de acceso de usuario solo se admite para uso limitado en la [asignación de roles a identidades administradas](../how-to/deploy-policy-remediation.md#create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant).  No se admiten los roles personalizados ni [los roles de administrador de suscripciones clásicas](../../role-based-access-control/classic-administrators.md).
- Aunque puede incorporar suscripciones que usan Azure Databricks, los usuarios del inquilino de administración no pueden iniciar áreas de trabajo de Azure Databricks en una suscripción delegada en este momento.
- Aunque puede incorporar suscripciones y grupos de recursos que tengan bloqueos de recursos, dichos bloqueos no impedirán que los usuarios realicen acciones en el inquilino de administración. Las [asignaciones de denegación](../../role-based-access-control/deny-assignments.md) que protegen los recursos administrados por el sistema, como los que crean Azure Managed Applications o Azure Blueprints (asignaciones de denegación asignadas por el sistema), impiden que los usuarios del inquilino de administración actúen en esos recursos; sin embargo, actualmente, los usuarios del inquilino del cliente no pueden crear sus propias asignaciones de denegación (asignaciones de denegación asignadas por el usuario).
- No se admite la delegación de suscripciones entre una [nube nacional](../../active-directory/develop/authentication-national-cloud.md) y la nube pública de Azure o entre dos nubes nacionales independientes.

## <a name="next-steps"></a>Pasos siguientes

- Incorpore los clientes a Azure Lighthouse, ya sea [mediante plantillas de Azure Resource Manager](../how-to/onboard-customer.md) o [publicando una oferta de servicios administrados privada o pública en Azure Marketplace](../how-to/publish-managed-services-offers.md).
- Puede [ver y administrar clientes](../how-to/view-manage-customers.md) desde **Mis clientes**, en Azure Portal.
