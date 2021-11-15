---
title: Disponibilidad de características en la nube de servicios de Azure para clientes de la Administración Pública de Estados Unidos
description: Incluya una lista de la disponibilidad de características para los servicios de seguridad de Azure, como Azure Sentinel para los clientes de la Administración Pública de Estados Unidos
author: TerryLanfear
ms.author: terrylan
ms.service: security
ms.topic: reference
ms.date: 09/13/2021
ms.openlocfilehash: 46d7b4d08a7181b56859c5639ccd5a187e526474
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131564354"
---
# <a name="cloud-feature-availability-for-us-government-customers"></a>Disponibilidad de las características en la nube para clientes de la Administración Pública de Estados Unidos

En este artículo se describe la disponibilidad de características en las nubes de Microsoft Azure y Azure Government para los siguientes servicios de seguridad:

- [Azure Information Protection](#azure-information-protection)
- [Azure Security Center](#azure-security-center)
- [Azure Sentinel](#azure-sentinel)
- [Azure Defender para IoT](#azure-defender-for-iot)
- [Azure Attestation](#azure-attestation)

> [!NOTE]
> Pronto se agregarán servicios de seguridad adicionales a este artículo.
>

## <a name="azure-government"></a>Azure Government

Azure Government usa las mismas tecnologías subyacentes que Azure (a veces denominadas Azure Commercial o Azure Public), que incluye los componentes principales de la infraestructura como servicio (IaaS), la plataforma como servicio (PaaS) y el software como servicio (SaaS). Tanto Azure como Azure Government cuentan con controles de seguridad integrales y el compromiso de Microsoft para la protección de los datos de los clientes.

Azure Government es un entorno en la nube aislado físicamente que está dedicado a los gobiernos federales, estatales, locales y tribales de Estados Unidos, así como a sus asociados. Mientras que ambos entornos en la nube se evalúan y autorizan en el nivel de impacto FedRAMP High, Azure Government proporciona un nivel adicional de protección a los clientes a través de compromisos contractuales relacionados con el almacenamiento de los datos de clientes en Estados Unidos y la limitación del posible acceso a los sistemas que procesen los datos del cliente a personas seleccionadas de Estados Unidos. Estos compromisos pueden ser de interés para los clientes que usan la nube a fin de almacenar o procesar datos sujetos a las regulaciones de control de exportación de Estados Unidos, como EAR, ITAR y 10 CFR parte 810 del Departamento de Energía de Estados Unidos.

Para obtener más información acerca de Azure Government, consulte [¿Qué es Azure Government?](../../azure-government/documentation-government-welcome.md)

## <a name="microsoft-365-integration"></a>Integración de Microsoft 365

Las integraciones entre productos se basan en la interoperabilidad entre las plataformas de Azure y Office. Las ofertas hospedadas en el entorno de Azure son accesibles desde las plataformas Microsoft 365 Enterprise y Microsoft 365 Government. Office 365 y Office 365 GCC trabajan con Azure Active Directory (Azure AD) en Azure. Office 365 GCC High y Office 365 DoD se trabajan con Azure AD en Azure Government.

En el diagrama siguiente se muestra la jerarquía de nubes de Microsoft y cómo se relacionan entre sí.

:::image type="content" source="media/microsoft-365-cloud-integration.png" alt-text="Integración de nube de Microsoft 365.":::

El entorno de Office 365 GCC ayuda a los clientes a cumplir los requisitos de la Administración Pública de Estados Unidos, incluidos FedRAMP High, CJIS y la publicación 1075 de IRS. Los entornos de Office 365 GCC High y DoD admiten a los clientes que necesitan cumplir con DoD IL4/5, DFARS 7012, NIST 800-171 e ITAR.

Para obtener más información acerca de los entornos de Office 365 para la Administración Pública de Estados Unidos, consulte:

- [Office 365 GCC](/office365/servicedescriptions/office-365-platform-service-description/office-365-us-government/gcc)
- [Office 365 GCC High y DoD](/office365/servicedescriptions/office-365-platform-service-description/office-365-us-government/gcc-high-and-dod)


En las siguientes secciones se identifica cuándo un servicio tiene una integración con Microsoft 365 y la disponibilidad de características para Office 365 GCC, Office 365 GCC High y Office 365 DoD.

## <a name="azure-information-protection"></a>Azure Information Protection

Azure Information Protection (AIP) es una solución basada en la nube que permite a las organizaciones descubrir, clasificar y proteger documentos y correos electrónicos mediante la aplicación de etiquetas al contenido.

AIP forma parte de la solución Microsoft Information Protection (MIP) y amplía la funcionalidad de [etiquetado](/microsoft-365/compliance/sensitivity-labels) y [clasificación](/microsoft-365/compliance/data-classification-overview) proporcionada por Microsoft 365.

Para obtener más información, consulte la [documentación del producto Azure Information Protection](/azure/information-protection/).

- Office 365 GCC trabaja con Azure Active Directory (Azure AD) en Azure. Office 365 GCC High y Office 365 DoD se trabajan con Azure AD en Azure Government. Asegúrese de prestar atención al entorno de Azure para comprender dónde [es posible la interoperabilidad](#microsoft-365-integration). En la tabla siguiente, la interoperabilidad que *no* es posible está marcada con un guion (-) a fin de indicar que la compatibilidad no es pertinente.

- Se requieren configuraciones adicionales para clientes de GCC High y DoD. Para obtener más información, consulte [Descripción del servicio Azure Information Protection Premium para Administración Pública](/enterprise-mobility-security/solutions/ems-aip-premium-govt-service-description).

> [!NOTE]
> En las notas al pie de la tabla se muestran más detalles sobre el soporte técnico para los clientes de la administración pública. 
> 
> Se requieren pasos adicionales para configurar Azure Information Protection para clientes de GCC High y DoD. Para obtener más información, consulte la [Descripción del servicio de Azure Information Protection Premium para la Administración Pública](/enterprise-mobility-security/solutions/ems-aip-premium-govt-service-description).
>

|Característica o servicio  |Azure  |Azure Government  |
|---------|---------|---------|
|**[Analizador de Azure Information Protection](/azure/information-protection/deploy-aip-scanner)** <sup>[1](#aipnote1)</sup>       |         |         |
| - Office 365 GCC | GA | - |
| - Office 365 GCC High | - | GA |
| - Office 365 DoD | - | GA |
|**Administración**     |         |         |
|[Portal de Azure Information Protection para la administración del analizador](/azure/information-protection/deploy-aip-scanner-configure-install?tabs=azure-portal-only)     |         |         |
| - Office 365 GCC | GA | - |
| - Office 365 GCC High | - | GA |
| - Office 365 DoD | - | GA |
| **Clasificación y etiquetado** <sup>[2](#aipnote2)</sup>   |         |         |
| [Analizador de AIP para aplicar una *etiqueta predeterminada* a todos los archivos de un repositorio o servidor de archivos local](/azure/information-protection/deploy-aip-scanner-configure-install?tabs=azure-portal-only)    |         |         |
| - Office 365 GCC | GA | - |
| - Office 365 GCC High | - | GA |
| - Office 365 DoD | - | GA |
| [Analizador de AIP para la clasificación, el etiquetado y la protección automatizados de archivos locales compatibles](/azure/information-protection/deploy-aip-scanner)    |         |         |
| - Office 365 GCC | GA | - |
| - Office 365 GCC High | - | GA |
| - Office 365 DoD | - | GA |
| |  |  |

<sup><a name="aipnote1" /></a>1</sup> El analizador puede funcionar sin Office 365 para examinar archivos únicamente. El analizador no puede aplicar etiquetas a los archivos sin Office 365.

<sup><a name="aipnote2" /></a>2</sup> El complemento de clasificación y etiquetado solo se admite para los clientes de la administración pública con Aplicaciones Microsoft 365 (versión 9126.1001 o posterior), incluidas las versiones Profesional Plus (ProPlus) y Hacer clic y ejecutar (C2R). No se admite Office 2010, Office 2013 ni otras versiones de Office 2016.

### <a name="office-365-features"></a>Características de Office 365

|Característica o servicio  |Office 365 GCC  |Office 365 GCC High |Office 365 DoD  |
|---------|---------|---------|---------|
|**Administración**     |         |         | |
|- [PowerShell para la administración del servicio RMS](/powershell/module/aipservice/)      |  Disponibilidad general       |    GA     |   GA      |
|- [PowerShell para operaciones masivas de cliente de etiquetado unificado de AIP](/powershell/module/azureinformationprotection/)      |         |         |         |
|**SDK**     |         |         |         |
|- [Kit de desarrollo de software (SDK) para MIP y AIP](/information-protection/develop/)     |     GA       |    GA     |   GA  |
|**Personalizaciones**     |         |         |         |
|- [Seguimiento y revocación de documentos](/azure/information-protection/rms-client/track-and-revoke-admin)      |   Disponibilidad general      |  No disponible       |     No disponible    |
|**Administración de claves**      |         |         |         |
|- [Bring Your Own Key (BYOK)](/azure/information-protection/byok-price-restrictions)      |   GA       |    GA     |   GA   |
|- [Cifrado de doble clave (DKE)](/azure/information-protection/plan-implement-tenant-key)     |    GA       |    GA     |   GA    |
|**Archivos de Office** <sup>[3](#aipnote6)</sup>      |         |         |         |
|- [Protección para Microsoft Exchange Online, Microsoft Office SharePoint Online y Microsoft OneDrive para la Empresa](/azure/information-protection/requirements-applications)      |     Disponibilidad general    |  GA <sup>[4](#aipnote3)</sup>       |   GA <sup>[4](#aipnote3)</sup>      |
|- [Protección del contenido de Exchange y SharePoint local a través del conector de Rights Management](/azure/information-protection/deploy-rms-connector)     |    GA <sup>[5](#aipnote5)</sup>      |  No disponible       |     No disponible         |
|- [Cifrado de mensajes de Office 365](/microsoft-365/compliance/set-up-new-message-encryption-capabilities)      |     GA       |    GA     |   GA        |
|- [Establecer etiquetas para aplicar automáticamente la protección de M/MIME preconfigurado en Outlook](/azure/information-protection/rms-client/clientv2-admin-guide-customizations)      |         GA       |    GA     |   GA        |
|- [Control del uso compartido excesivo de información al usar Outlook](/azure/information-protection/rms-client/clientv2-admin-guide-customizations)     |      Disponibilidad general   |  GA <sup>[6](#aipnote6)</sup>        |    GA <sup>[6](#aipnote6)</sup>      |
|**Clasificación y etiquetado** <sup>[2](#aipnote2) / [7](#aipnote7)</sup>      |         |         |         |
|- Plantillas personalizadas, incluidas las plantillas de departamento     |     GA       |    GA     |   GA         |
|- Clasificación manual, predeterminada y obligatoria de documentos     |       GA       |    GA     |   GA       |
|- Configuración de condiciones para la clasificación automática y recomendada       |    GA     |   GA        |
|- [Protección para formatos de archivo distintos de Microsoft Office, incluidos PTXT, PJPG y PFILE (protección genérica)](/azure/information-protection/rms-client/clientv2-admin-guide-file-types)     |        GA       |    GA     |   GA       |
|     |         |         |         |


<sup><a name="aipnote3" /></a>3</sup> La extensión de dispositivos móviles para AD RMS no está disponible actualmente para los clientes de la administración pública.

<sup><a name="aipnote4" /></a>4</sup> Information Rights Management con SharePoint Online (sitios y bibliotecas protegidos por IRM) no está disponible actualmente.

<sup><a name="aipnote5" /></a>5</sup> Information Rights Management (IRM) solo se admite para Aplicaciones de Microsoft 365 (versión 9126.1001 o posterior), incluidas las versiones Profesional Plus (ProPlus) y Hacer clic y ejecutar (C2R). No se admite Office 2010, Office 2013 ni otras versiones de Office 2016.

<sup><a name="aipnote6" /></a>6</sup> Actualmente no está disponible el uso compartido de documentos y correos electrónicos protegidos desde nubes gubernamentales con los usuarios de la nube comercial. Incluye usuarios de Aplicaciones de Microsoft 365 en la nube comercial, usuarios distintos de Aplicaciones de Microsoft 365 en la nube comercial y usuarios con una licencia de RMS for Individuals.

<sup><a name="aipnote7" /></a>7</sup> El número de [tipos de información confidencial](/microsoft-365/compliance/sensitive-information-type-entity-definitions) del Centro de seguridad y cumplimiento de Microsoft 365 puede variar en función de la región.

## <a name="azure-security-center"></a>Azure Security Center

Azure Security Center es un sistema unificado de administración de seguridad de la infraestructura que fortalece la posición de seguridad de los centros de datos y proporciona una protección contra amenazas avanzada de todas las cargas de trabajo híbridas que se encuentran en la nube, ya sea que estén en Azure o no, así como también en el entorno local.

Para más información, consulte la [documentación del producto de Azure Security Center](../../security-center/security-center-introduction.md).

En la tabla siguiente se muestra la disponibilidad actual de características de Security Center en Azure y Azure Government.


| Característica o servicio                                                                                                                                                               | Azure          | Azure Government               |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------|--------------------------------|
| **Características gratuitas de Security Center**                                                                                                                                             |                |                                |
| - [Exportación continua](../../security-center/continuous-export.md)                                                                                                             | GA             | GA                             |
| - [Automatización de flujos de trabajo](../../security-center/continuous-export.md)                                                                                                           | GA             | GA                             |
| - [Reglas de exención de recomendaciones](../../security-center/exempt-resource.md)                                                                                                  | Vista previa pública | No disponible                  | 
| - [Reglas de eliminación de alertas](../../security-center/alerts-suppression-rules.md)                                                                                                | GA             | GA                             | 
| - [Notificaciones de correo electrónico para alertas de seguridad](../../security-center/security-center-provide-security-contact-details.md)                                                        | GA             | GA                             | 
| - [Aprovisionamiento automático de agentes y extensiones](../../security-center/security-center-enable-data-collection.md)                                                              | GA             | GA                             | 
| - [Inventario de recursos](../../security-center/asset-inventory.md)                                                                                                                 | GA             | GA                             | 
| - [Informes de libros de Azure Monitor en la galería de libros de Azure Security Center](../../security-center/custom-dashboards-azure-workbooks.md)                                  | GA             | GA                             | 
| **Planes y extensiones de Azure Defender**                                                                                                                                       |                |                                | 
| - [Azure Defender para servidores](../../security-center/defender-for-servers-introduction.md)                                                                                    | GA             | GA                             | 
| - [Azure Defender para App Service](../../security-center/defender-for-app-service-introduction.md)                                                                            | GA             | No disponible                  | 
| - [Azure Defender para DNS](../../security-center/defender-for-dns-introduction.md)                                                                                            | GA             | GA                             | 
| - [Azure Defender para registros de contenedor](../../security-center/defender-for-container-registries-introduction.md) <sup>[1](#footnote1)</sup>                               | Disponibilidad general             | GA  <sup>[2](#footnote2)</sup> | 
| - [Azure Defender para registros de contenedor que examinan imágenes en flujos de trabajo de CI/CD](../../security-center/defender-for-container-registries-cicd.md) <sup>[3](#footnote3)</sup> | Vista previa pública | No disponible                  | 
| - [Azure Defender para Kubernetes](../../security-center/defender-for-kubernetes-introduction.md) <sup>[4](#footnote4)</sup>                                                   | GA             | GA                             | 
| - [Extensión de Azure Defender para clústeres de Kubernetes habilitados para Azure Arc](../../security-center/defender-for-kubernetes-azure-arc.md) <sup>[5](#footnote5)</sup>                 | Vista previa pública | No disponible                  | 
| - [Azure Defender para servidores de Azure SQL Database](../../security-center/defender-for-sql-introduction.md)                                                                     | GA             | GA                             | 
| - [Azure Defender para servidores SQL en máquinas](../../security-center/defender-for-sql-introduction.md)                                                                        | GA             | GA                             |
| - [Azure Defender para bases de datos relacionales de código abierto](../../security-center/defender-for-databases-introduction.md)                                                         | GA             | No disponible                  |
| - [Azure Defender para Key Vault](../../security-center/defender-for-key-vault-introduction.md)                                                                                | GA             | No disponible                  |
| - [Azure Defender para Resource Manager](../../security-center/defender-for-resource-manager-introduction.md)                                                                  | Disponibilidad general             | GA                             |
| - [Azure Defender para Storage](../../security-center/defender-for-storage-introduction.md) <sup>[6](#footnote6)</sup>                                                         | GA             | GA                             |
| - [Protección contra amenazas para Cosmos DB](../../security-center/other-threat-protections.md#threat-protection-for-azure-cosmos-db-preview)                                          | Vista previa pública | No disponible                  |
| - [Protección de cargas de trabajo de Kubernetes](../../security-center/kubernetes-workload-protections.md)                                                                                  | GA             | GA                             |
| - [Sincronización de alertas bidireccional con Sentinel](../../sentinel/connect-azure-security-center.md)                                                                       | Vista previa pública | No disponible                  | 
| **Azure Defender para características de servidores** <sup>[7](#footnote7)</sup>                                                                                                            |                |                                |
| - [Acceso de máquina virtual Just-In-Time](../../security-center/security-center-just-in-time.md)                                                                                             | GA             | GA                             |
| - [Supervisión de la integridad de los archivos](../../security-center/security-center-file-integrity-monitoring.md)                                                                             | GA             | GA                             |
| - [Controles de aplicaciones adaptables](../../security-center/security-center-adaptive-application.md)                                                                              | GA             | GA                             |
| - [Protección de red adaptable](../../security-center/security-center-adaptive-network-hardening.md)                                                                           | GA             | No disponible                  |
| - [Protección de hosts de Docker](../../security-center/harden-docker-hosts.md)                                                                                                       | GA             | GA                             |
| - [Evaluación integrada de vulnerabilidades para las máquinas](../../security-center/deploy-vulnerability-assessment-vm.md)                                                             | GA             | No disponible                  |
| - [Panel e informes de cumplimiento normativo](../../security-center/security-center-compliance-dashboard.md) <sup>[8](#footnote8)</sup>                                       | GA             | GA                             |
| - [Implementación de Microsoft Defender para punto de conexión y licencia integrada](../../security-center/security-center-wdatp.md)                                                         | GA             | GA                             |
| - [Conexión de cuentas de AWS](../../security-center/quickstart-onboard-aws.md)                                                                                                      | GA             | No disponible                  |
| - [Conexión de cuentas de GCP](../../security-center/quickstart-onboard-gcp.md)                                                                                                      | GA             | No disponible                  |
|                                                                                                                                                                               |                |                                |

<sup> <a name="footnote1" /> </a> 1</sup> Parcialmente en disponibilidad general: la capacidad de deshabilitar los resultados específicos de los exámenes de vulnerabilidades está en versión preliminar pública.

<sup><a name="footnote2" /></a>2</sup> Los exámenes de vulnerabilidades de los registros de contenedor en Azure Gov solo se pueden realizar con la característica de examen en inserción.

<sup><a name="footnote3" /></a>3</sup> Requiere Azure Defender para registros de contenedor.

<sup><a name="footnote4" /></a>4</sup> Parcialmente en disponibilidad general: la compatibilidad con clústeres habilitados para Azure Arc está en versión preliminar pública y no está disponible en Azure Government.

<sup><a name="footnote5" /></a>5</sup> Requiere Azure Defender para Kubernetes.

<sup><a name="footnote6" /></a> 6</sup> Parcialmente en disponibilidad general: algunas de las alertas de protección contra amenazas de Azure Defender para Storage están en versión preliminar pública.

<sup><a name="footnote7" /></a>7</sup> Todas estas características requieren [Azure Defender para servidores](../../security-center/defender-for-servers-introduction.md).

<sup><a name="footnote8" /></a>8</sup> Puede haber diferencias en los estándares ofrecidos por cada tipo de nube.

## <a name="azure-sentinel"></a>Azure Sentinel

Microsoft Azure Sentinel es una solución de administración de eventos de información de seguridad (SIEM) y respuesta automatizada de orquestación de seguridad (SOAR) que es escalable y nativa de la nube. Azure Sentinel ofrece análisis de seguridad inteligente e inteligencia frente a amenazas en toda la empresa, de forma que proporciona una única solución para la detección de alertas, la visibilidad de amenazas, la búsqueda proactiva y la respuesta a amenazas.

Para más información, consulte la [documentación del producto de Azure Sentinel](../../sentinel/overview.md).

En las siguientes tablas se muestra la disponibilidad de características de Azure Sentinel actuales en Azure y Azure Government.


| Característica | Azure | Azure Government  |
| ----- | ----- | ---- |
|- [Regalas de automatización](../../sentinel/automate-incident-handling-with-automation-rules.md) | Vista previa pública | Vista previa pública |
|- [Traiga su propio aprendizaje automático (BYO-ML)](../../sentinel/bring-your-own-ml.md) | Vista previa pública | Vista previa pública |
| - [Vista de incidentes entre inquilinos o áreas de trabajo](../../sentinel/multiple-workspace-view.md) |Vista previa pública | Vista previa pública |
| - [Conclusiones sobre la entidad](../../sentinel/enable-entity-behavior-analytics.md) | Disponibilidad general | Vista previa pública |
| - [Fusion](../../sentinel/fusion.md)<br>Detección avanzada de ataques de varias fases <sup>[1](#footnote1)</sup> | GA | GA |
| - [Búsqueda](../../sentinel/hunting.md) | GA | GA |
|- [Cuadernos](../../sentinel/notebooks.md) | GA | GA |
|- [Métricas de auditoría de incidentes de centro de operaciones de seguridad](../../sentinel/manage-soc-with-incident-metrics.md) | GA | GA |
|- [Listas de reproducción](../../sentinel/watchlists.md) | GA | GA |
| **Compatibilidad con inteligencia sobre amenazas** | | |
| - [Inteligencia sobre amenazas: conector de datos TAXII](../../sentinel/understand-threat-intelligence.md)  | GA | GA |
| - [Conector de datos de la plataforma de inteligencia sobre amenazas](../../sentinel/understand-threat-intelligence.md)  | Vista previa pública | No disponible |
| - [Hoja de investigación de la inteligencia sobre amenazas](https://techcommunity.microsoft.com/t5/azure-sentinel/what-s-new-threat-intelligence-menu-item-in-public-preview/ba-p/1646597)  | GA | GA |
| - [Detonación de direcciones URL](https://techcommunity.microsoft.com/t5/azure-sentinel/using-the-new-built-in-url-detonation-in-azure-sentinel/ba-p/996229) | Vista previa pública | No disponible |
| - [Libro de inteligencia sobre amenazas](/azure/architecture/example-scenario/data/sentinel-threat-intelligence)  | GA | GA |
|**Compatibilidad con la detección** | | |
| - [Detección de acceso anómalo al recurso compartido de archivos de Windows](../../sentinel/fusion.md)  | Vista previa pública | No disponible |
| - [Detección de inicios de sesión anómalos mediante RDP](../../sentinel/connect-windows-security-events.md#configure-the-security-events--windows-security-events-connector-for-anomalous-rdp-login-detection)<br>Detección de ML integrada | Vista previa pública | No disponible |
| - [Detección de inicios de sesión anómalos mediante SSH](../../sentinel/connect-syslog.md#configure-the-syslog-connector-for-anomalous-ssh-login-detection)<br>Detección de ML integrada | Vista previa pública | No disponible |
| **Conectores de servicio de Azure** |  |  |
| - [Registros de actividad de Azure](../../sentinel/data-connectors-reference.md#azure-activity) | GA | GA |
| - [Azure Active Directory](../../sentinel/connect-azure-active-directory.md) | GA | GA |
| - [Azure ADIP](../../sentinel/data-connectors-reference.md#azure-active-directory-identity-protection) | GA | GA |
| - [Azure DDoS Protection](../../sentinel/data-connectors-reference.md#azure-ddos-protection) | GA | GA |
| - [Azure Defender](../../sentinel/connect-azure-security-center.md) | GA | GA |
| - [Azure Defender para IoT](../../sentinel/data-connectors-reference.md#azure-defender-for-iot) | Vista previa pública | No disponible |
| - [Azure Firewall ](../../sentinel/data-connectors-reference.md#azure-firewall) | GA | GA |
| - [Azure Information Protection](../../sentinel/data-connectors-reference.md#azure-information-protection) | Vista previa pública | No disponible |
| - [Azure Key Vault ](../../sentinel/data-connectors-reference.md#azure-key-vault) | Vista previa pública | No disponible |
| - [Azure Kubernetes Services (AKS)](../../sentinel/data-connectors-reference.md#azure-kubernetes-service-aks) | Vista previa pública | No disponible |
| - [Instancias de Azure SQL Database](../../sentinel/data-connectors-reference.md#azure-sql-databases) | GA | GA |
| - [Firewall de aplicaciones web de Azure](../../sentinel/data-connectors-reference.md#azure-web-application-firewall-waf) | GA | GA |
| **Conectores de Windows** |  |  |
| - [Firewall de Windows](../../sentinel/data-connectors-reference.md#windows-firewall) | GA | GA |
| - [Eventos de seguridad de Windows](../../sentinel/connect-windows-security-events.md) | GA | GA |
| **Conectores externos** |  |  |
| - [Agari Phishing Defense y Agari Brand Protection](../../sentinel/data-connectors-reference.md#agari-phishing-defense-and-brand-protection-preview) | Vista previa pública | Vista previa pública |
| - [Analista de IA de Darktrace](../../sentinel/connect-data-sources.md) | Vista previa pública | Vista previa pública |
| - [Detección de IA Vectra](../../sentinel/data-connectors-reference.md#ai-vectra-detect-preview) | Vista previa pública | Vista previa pública |
| - [Akamai Security Events](../../sentinel/data-connectors-reference.md#akamai-security-events-preview) | Vista previa pública | Vista previa pública |
| - [Alcide kAudit](../../sentinel/data-connectors-reference.md#alcide-kaudit) | Vista previa pública | No disponible |
| - [Alsid para Active Directory](../../sentinel/data-connectors-reference.md#alsid-for-active-directory) | Vista previa pública | No disponible |
| - [Servidor HTTP de Apache](../../sentinel/data-connectors-reference.md#apache-http-server) | Vista previa pública | No disponible |
| - [Aruba ClearPass](../../sentinel/data-connectors-reference.md#aruba-clearpass-preview) | Vista previa pública | Vista previa pública |
| - [AWS](../../sentinel/connect-data-sources.md) | GA | GA |
| - [Firewall de Barracuda CloudGen](../../sentinel/data-connectors-reference.md#barracuda-cloudgen-firewall) | GA | GA |
| - [Firewall de aplicación web de Barracuda](../../sentinel/data-connectors-reference.md#barracuda-waf) | GA | GA |
| - [BETTER Mobile Threat Defense MTD](../../sentinel/data-connectors-reference.md#better-mobile-threat-defense-mtd-preview) | Vista previa pública | No disponible |
| - [Beyond Security beSECURE](../../sentinel/data-connectors-reference.md#beyond-security-besecure) | Vista previa pública | No disponible |
| - [Blackberry CylancePROTECT](../../sentinel/connect-data-sources.md) | Vista previa pública | Vista previa pública |
| - [DLP de Broadcom Symantec](../../sentinel/data-connectors-reference.md#broadcom-symantec-data-loss-prevention-dlp-preview) | Vista previa pública | Vista previa pública |
| - [Check Point](../../sentinel/data-connectors-reference.md#check-point) | GA | GA |
| - [Cisco ASA](../../sentinel/data-connectors-reference.md#cisco-asa) | GA | GA |
| - [Cisco Meraki](../../sentinel/data-connectors-reference.md#cisco-meraki-preview) | Vista previa pública | Vista previa pública |
| - [Cisco Umbrella](../../sentinel/data-connectors-reference.md#cisco-umbrella-preview) | Vista previa pública | Vista previa pública |
| - [Cisco UCS](../../sentinel/data-connectors-reference.md#cisco-unified-computing-system-ucs-preview) | Vista previa pública | Vista previa pública |
| - [Cisco Firepower eStreamer](../../sentinel/connect-data-sources.md) | Vista previa pública | Vista previa pública |
| - [Citrix Analytics WAF](../../sentinel/data-connectors-reference.md#citrix-web-app-firewall-waf-preview) | GA | GA |
| - [Common Event Format (CEF)](../../sentinel/connect-common-event-format.md) | GA | GA |
| - [Eventos de CyberArk Enterprise Password Vault (EPV)](../../sentinel/data-connectors-reference.md#cyberark-enterprise-password-vault-epv-events-preview) | Vista previa pública | Vista previa pública |
| - [ESET Enterprise Inspector](../../sentinel/connect-data-sources.md)                       | Vista previa pública | No disponible      |
| - [Eset Security Management Center](../../sentinel/connect-data-sources.md)                  | Vista previa pública | No disponible      |
| - [ExtraHop Reveal(x)](../../sentinel/data-connectors-reference.md#extrahop-revealx)                               | GA             | GA             |
| - [F5 BIG-IP ](../../sentinel/data-connectors-reference.md#f5-big-ip)                                       | GA             | GA             |
| - [F5 Networks](../../sentinel/data-connectors-reference.md#f5-networks-asm)                                     | GA             | GA             |
| - [Forcepoint NGFW](../../sentinel/data-connectors-reference.md#forcepoint-cloud-access-security-broker-casb-preview)                                  | Vista previa pública | Vista previa pública |
| - [Forcepoint CASB](../../sentinel/data-connectors-reference.md#forcepoint-cloud-access-security-broker-casb-preview)                                  | Vista previa pública | Vista previa pública |
| - [Forcepoint DLP](../../sentinel/data-connectors-reference.md#forcepoint-data-loss-prevention-dlp-preview)                                   | Vista previa pública | No disponible      |
| - [Common Audit de ForgeRock para CEF](../../sentinel/connect-data-sources.md)                  | Vista previa pública | Vista previa pública |
| - [Fortinet](../../sentinel/data-connectors-reference.md#fortinet)                                         | GA             | GA             |
| - [Google Workspace (G Suite) ](../../sentinel/data-connectors-reference.md#google-workspace-g-suite-preview)                      | Vista previa pública | No disponible      |
| - [Illusive Attack Management System](../../sentinel/data-connectors-reference.md#illusive-attack-management-system-ams-preview)                | Vista previa pública | Vista previa pública |
| - [Imperva WAF Gateway](../../sentinel/data-connectors-reference.md#imperva-waf-gateway-preview)                             | Vista previa pública | Vista previa pública |
| - [Infoblox NIOS](../../sentinel/data-connectors-reference.md#infoblox-network-identity-operating-system-nios-preview)                                    | Vista previa pública | Vista previa pública |
| - [Juniper SRX](../../sentinel/data-connectors-reference.md#juniper-srx-preview)                                      | Vista previa pública | Vista previa pública |
| - [Morphisec UTPP](../../sentinel/connect-data-sources.md)                                   | Vista previa pública | Vista previa pública |
| - [Netskope](../../sentinel/connect-data-sources.md)                                         | Vista previa pública | Vista previa pública |
| - [NXLog DNS para Windows](../../sentinel/data-connectors-reference.md#nxlog-dns-logs-preview)                                             | Vista previa pública | No disponible      |
| - [NXLog LinuxAudit](../../sentinel/data-connectors-reference.md#nxlog-linuxaudit-preview)                                 | Vista previa pública | No disponible      |
| - [Inicio de sesión único de Okta](../../sentinel/data-connectors-reference.md#okta-single-sign-on-preview)                              | Vista previa pública | Vista previa pública |
| - [Onapsis Platform](../../sentinel/connect-data-sources.md)                                 | Vista previa pública | Vista previa pública |
| - [One Identity Safeguard](../../sentinel/data-connectors-reference.md#one-identity-safeguard-preview)                          | GA             | GA             |
| - [Alertas de Orca Security](../../sentinel/data-connectors-reference.md#orca-security-preview)                            | Vista previa pública | No disponible      |
| - [Palo Alto Networks](../../sentinel/data-connectors-reference.md#palo-alto-networks)                               | GA             | GA             |
| - [Registros de actividad de Perimeter 81](../../sentinel/data-connectors-reference.md#perimeter-81-activity-logs-preview)                      | GA             | No disponible      |
| - [Proofpoint On Demand Email Security](../../sentinel/data-connectors-reference.md#proofpoint-on-demand-pod-email-security-preview)             | Vista previa pública | No disponible      |
| - [Proofpoint TAP](../../sentinel/data-connectors-reference.md#proofpoint-targeted-attack-protection-tap-preview)                                   | Vista previa pública | Vista previa pública |
| - [Pulse Connect Secure](../../sentinel/data-connectors-reference.md#proofpoint-targeted-attack-protection-tap-preview)                             | Vista previa pública | Vista previa pública |
| - [Qualys Vulnerability Management](../../sentinel/data-connectors-reference.md#qualys-vulnerability-management-vm-preview)                  | Vista previa pública | Vista previa pública |
| - [Salesforce Service Cloud](../../sentinel/data-connectors-reference.md#salesforce-service-cloud-preview)                         | Vista previa pública | No disponible      |
| - [SonicWall Firewall ](../../sentinel/data-connectors-reference.md#sophos-cloud-optix-preview)                              | Vista previa pública | Vista previa pública |
| - [Sophos Cloud Optix](../../sentinel/data-connectors-reference.md#sophos-cloud-optix-preview)                               | Vista previa pública | No disponible      |
| - [Sophos XG Firewall](../../sentinel/data-connectors-reference.md#sophos-xg-firewall-preview)                               | Vista previa pública | Vista previa pública |
| - [secRMM de Squadra Technologies](../../sentinel/data-connectors-reference.md#squadra-technologies-secrmm)               | GA             | GA             |
| - [Squid Proxy](../../sentinel/data-connectors-reference.md#squid-proxy-preview)                                      | Vista previa pública | No disponible      |
| - [Symantec Integrated Cyber Defense Exchange](../../sentinel/data-connectors-reference.md#symantec-integrated-cyber-defense-exchange-icdx)       | GA             | GA             |
| - [Symantec ProxySG](../../sentinel/data-connectors-reference.md#symantec-proxysg-preview)                                | Vista previa pública | Vista previa pública |
| - [Symantec VIP](../../sentinel/data-connectors-reference.md#symantec-vip-preview)                                     | Vista previa pública | Vista previa pública |
| - [Syslog](../../sentinel/connect-syslog.md)                                           | GA             | GA             |
| - [Thycotic Secret Server](../../sentinel/data-connectors-reference.md#thycotic-secret-server-preview)                          | Vista previa pública | Vista previa pública |
| - [Deep Security de Trend Micro](../../sentinel/data-connectors-reference.md#trend-micro-deep-security)                       | GA             | GA             |
| - [TippingPoint de Trend Micro](../../sentinel/data-connectors-reference.md#trend-micro-tippingpoint-preview)                         | Vista previa pública | Vista previa pública |
| - [Trend Micro XDR](../../sentinel/connect-data-sources.md)                                  | Vista previa pública | No disponible      |
| - [VMware Carbon Black Endpoint Standard](../../sentinel/data-connectors-reference.md#vmware-carbon-black-endpoint-standard-preview)           | Vista previa pública | Vista previa pública |
| - [VMware ESXi](../../sentinel/data-connectors-reference.md#vmware-esxi-preview)                                      | Vista previa pública | Vista previa pública |
| - [WireX Network Forensics Platform](../../sentinel/data-connectors-reference.md#wirex-network-forensics-platform-preview)                | Vista previa pública | Vista previa pública |
| - [Zimperium Mobile Threat Defense](../../sentinel/data-connectors-reference.md#zimperium-mobile-thread-defense-preview)                  | Vista previa pública | No disponible      |
| - [Zscaler](../../sentinel/data-connectors-reference.md#zscaler)                                         | GA             | GA             |
| | | |


<sup><a name="footnote1" /></a>1</sup> Las detecciones de SSH y RDP no son compatibles con las nubes soberanas porque la plataforma de aprendizaje automático de Databricks no está disponible.

### <a name="microsoft-365-data-connectors"></a>Conectores de datos de Microsoft 365

Office 365 GCC trabaja con Azure Active Directory (Azure AD) en Azure. Office 365 GCC High y Office 365 DoD se trabajan con Azure AD en Azure Government.

> [!TIP]
> Asegúrese de prestar atención al entorno de Azure para comprender dónde [es posible la interoperabilidad](#microsoft-365-integration). En la tabla siguiente, la interoperabilidad que *no* es posible está marcada con un guion (-) a fin de indicar que la compatibilidad no es pertinente.
>

| Conector | Azure | Azure Government |
|--|--|--|
| **[Dynamics 365](../../sentinel/data-connectors-reference.md#dynamics-365)** |  |  |
| - Office 365 GCC | Vista previa pública | - |
| - Office 365 GCC High | - | No disponible |
| - Office 365 DoD | - | No disponible |
| **[Microsoft 365 Defender](../../sentinel/connect-microsoft-365-defender.md)** |  |  |
| - Office 365 GCC | Vista previa pública | - |
| - Office 365 GCC High | - | No disponible |
| - Office 365 DoD | - | No disponible |
| **[Microsoft Cloud App Security (MCAS)](../../sentinel/data-connectors-reference.md#microsoft-cloud-app-security-mcas)** |  |  |
| - Office 365 GCC | GA | - |
| - Office 365 GCC High | - | GA |
| - Office 365 DoD | - | GA |
| **[Microsoft Cloud App Security (MCAS)](../../sentinel/data-connectors-reference.md#microsoft-cloud-app-security-mcas)** <br>Registros de Shadow IT |  |  |
| - Office 365 GCC | Vista previa pública | - |
| - Office 365 GCC High | - | Vista previa pública |
| - Office 365 DoD | - | Vista previa pública |
| **[Microsoft Cloud App Security (MCAS)](../../sentinel/data-connectors-reference.md#microsoft-cloud-app-security-mcas)**                  <br>Alertas |  |  |
| - Office 365 GCC | Vista previa pública | - |
| - Office 365 GCC High | - | Vista previa pública |
| - Office 365 DoD | - | Vista previa pública |
| **[Microsoft Defender para punto de conexión](../../sentinel/data-connectors-reference.md#microsoft-defender-for-endpoint)** |  |  |
| - Office 365 GCC | GA | - |
| - Office 365 GCC High | - | GA |
| - Office 365 DoD | - | GA |
| **[Microsoft Defender for Identity](../../sentinel/data-connectors-reference.md#microsoft-defender-for-identity)** |  |  |
| - Office 365 GCC | Vista previa pública | - |
| - Office 365 GCC High | - | No disponible |
| - Office 365 DoD | - | No disponible |
| **[Microsoft Defender para Office 365](../../sentinel/data-connectors-reference.md#microsoft-defender-for-office-365)** |  |  |
| - Office 365 GCC | Vista previa pública | - |
| - Office 365 GCC High | - | No disponible |
| - Office 365 DoD | - | No disponible |
| **[Office 365](../../sentinel/data-connectors-reference.md#microsoft-office-365)** |  |  |
| - Office 365 GCC | GA | - |
| - Office 365 GCC High | - | GA |
| - Office 365 DoD | - | GA |
|  |  |

## <a name="azure-defender-for-iot"></a>Azure Defender para IoT

Azure Defender para IoT le permite acelerar la innovación de IoT/OT con seguridad integral en todos los dispositivos de IoT/OT.Para las organizaciones de usuarios finales, Azure Defender para IoT ofrece una seguridad de capa de red sin agentes que se implementa rápidamente, funciona con diversos equipos industriales, e interopera con Azure Sentinel y otras herramientas de SOC. Se puede implementar de forma local o en entornos conectados a Azure.Para los fabricantes de dispositivos de IoT, los agentes de seguridad de Azure Defender para IoT permiten compilar la seguridad directamente en los nuevos dispositivos IoT y en proyectos de Azure IoT. El micro-agente tiene opciones de implementación flexibles, incluida la capacidad de implementarse como un paquete binario o de modificar código fuente. Y está disponible para los sistemas operativos de IoT estándar como Linux y Azure RTOS. Para más información, consulte la [documentación del producto de Azure Defender para IoT](../../defender-for-iot/index.yml).

En la tabla siguiente se muestra la disponibilidad actual de características de Azure Defender para IoT en Azure y Azure Government.

### <a name="for-organizations"></a>Para organizaciones

| Característica | Azure | Azure Government |
|--|--|--|
| [Inventario y detección de dispositivos locales](../../defender-for-iot/how-to-investigate-all-enterprise-sensor-detections-in-a-device-inventory.md) | GA | GA |
| [Administración de vulnerabilidades](../../defender-for-iot/how-to-create-risk-assessment-reports.md) | GA | GA |
| [Detección de amenazas con IoT, y análisis de comportamiento de OT](../../defender-for-iot/how-to-work-with-alerts-on-your-sensor.md) | GA | GA |
| [Actualizaciones manuales y automáticas de inteligencia sobre amenazas](../../defender-for-iot/how-to-work-with-threat-intelligence-packages.md) | GA | GA |
| **Unificación de la seguridad de TI y OT con SIEM, SOAR y XDR** |  |  |
| [Active Directory](../../defender-for-iot/organizations/how-to-create-and-manage-users.md#integrate-with-active-directory-servers) | GA | GA |
| [ArcSight](../../defender-for-iot/organizations/how-to-accelerate-alert-incident-response.md#accelerate-incident-workflows-by-using-alert-groups) | GA | GA |
| [ClearPass (alertas e inventario)](../../defender-for-iot/organizations/how-to-install-software.md#attach-a-span-virtual-interface-to-the-virtual-switch) | GA | GA |
| [CyberArk PSM](../../defender-for-iot/organizations/concept-key-concepts.md#integrations) | GA | GA |
| [Correo electrónico](../../defender-for-iot/organizations/how-to-forward-alert-information-to-partners.md#email-address-action) | GA | GA |
| [FortiGate](../../defender-for-iot/organizations/tutorial-fortinet.md) | GA | GA |
| [FortiSIEM](../../defender-for-iot/organizations/tutorial-fortinet.md) | GA | GA |
| [Microsoft Sentinel](../../defender-for-iot/organizations/how-to-configure-with-sentinel.md) | Vista previa pública | Vista previa pública |
| [NetWitness](../../defender-for-iot/organizations/how-to-forward-alert-information-to-partners.md#netwitness-action) | GA | GA |
| [Palo Alto NGFW](../../defender-for-iot/organizations/tutorial-palo-alto.md) | GA | GA |
| [Palo Alto Panorama](../../defender-for-iot/organizations/tutorial-palo-alto.md) | GA | GA |
| [ServiceNow (alertas e inventario)](../../defender-for-iot/organizations/tutorial-servicenow.md) | GA | GA |
| [Supervisión de MIB del protocolo simple de administración de redes](../../defender-for-iot/organizations/how-to-set-up-snmp-mib-monitoring.md) | GA | GA |
| [Splunk](../../defender-for-iot/organizations/tutorial-splunk.md) | GA | GA |
| [Servidor de SYSLOG (formato CEF)](../../defender-for-iot/organizations/how-to-forward-alert-information-to-partners.md#syslog-server-actions) | GA | GA |
| [Servidor de SYSLOG (formato LEEF)](../../defender-for-iot/organizations/how-to-forward-alert-information-to-partners.md#syslog-server-actions) | GA | GA |
| [Servidor de SYSLOG (objeto)](../../defender-for-iot/organizations/how-to-forward-alert-information-to-partners.md#syslog-server-actions) | GA | GA |
| [Servidor SYSLOG (mensaje de texto)](../../defender-for-iot/organizations/how-to-forward-alert-information-to-partners.md#syslog-server-actions) | GA | GA |
| [Devolución de llamada web (webhook)](../../defender-for-iot/organizations/how-to-forward-alert-information-to-partners.md#webhook-server-action) | GA | GA |

### <a name="for-device-builders"></a>Para fabricantes de dispositivos

| Característica | Azure | Azure Government |
|--|--|--|
| [Microagente para Azure RTOS](../../defender-for-iot/iot-security-azure-rtos.md) | GA | GA |
| [Configuración de Sentinel con Azure Defender para IoT](../../defender-for-iot/how-to-configure-with-sentinel.md) | Vista previa pública | Vista previa pública |
| **Microagente independiente para Linux** |  |  |
| [Instalación binaria de agentes independientes](../../defender-for-iot/quickstart-standalone-agent-binary-installation.md) | Vista previa pública | Vista previa pública |

## <a name="azure-attestation"></a>Azure Attestation

Microsoft Azure Attestation es una solución unificada para comprobar de forma remota la confiabilidad de una plataforma y la integridad de los archivos binarios que se ejecutan en ella. El servicio recibe evidencia de la plataforma, la valida según estándares de seguridad, la evalúa con directivas configurables y genera un token de atestación para aplicaciones basadas en notificaciones (por ejemplo, usuarios de confianza o entidades de auditoría). 

Azure Attestation está disponible actualmente en varias regiones en las nubes públicas y de administración pública de Azure. En Azure Government, el servicio está disponible en versión preliminar en US Gov Virginia y US Gov Arizona. 

Para obtener más información, consulte la [documentación pública](/azure/attestation/overview) de Azure Attestation. 

| Característica | Azure | Azure Government |
|--|--|--|
| [Experiencia del portal](/azure/attestation/quickstart-portal) para realizar operaciones de plano de control y plano de datos | Disponibilidad general | - |
| [Experiencia del PowerShell](/azure/attestation/quickstart-powershell) para realizar operaciones de plano de control y plano de datos  | Disponibilidad general | GA |
| Cumplimiento de TLS 1.2   | GA | GA |
| Compatibilidad con BCDR   | Disponibilidad general | - |
| [Integración de etiquetas de servicio](/azure/virtual-network/service-tags-overview) | GA | GA |
| [Almacenamiento de registros inmutable](/azure/attestation/audit-logs) | GA | GA |
| Aislamiento de red mediante vínculo privado | Vista previa pública | - |
| [Certificación FedRAMP High](/azure/azure-government/compliance/azure-services-in-fedramp-auditscope) | GA | - |
| Caja de seguridad del cliente | Disponibilidad general | - |

## <a name="next-steps"></a>Pasos siguientes

- Información sobre el [modelo de responsabilidad compartida](shared-responsibility.md), las tareas de seguridad que administra el proveedor de nube y las tareas que administra el usuario.
- Información sobre las capacidades de la [nube de Azure Government](../../azure-government/documentation-government-welcome.md) y de la seguridad y el diseño de confianza que se usa para apoyar el cumplimiento aplicable a organizaciones locales, estatales, federales y sus asociados.
- Información sobre el [plan de Office 365 Administración Pública](/office365/servicedescriptions/office-365-platform-service-description/office-365-us-government/office-365-us-government#about-office-365-government-environments).
- Información sobre el [cumplimiento en Azure](../../compliance/index.yml) de los estándares legales y normativos.
