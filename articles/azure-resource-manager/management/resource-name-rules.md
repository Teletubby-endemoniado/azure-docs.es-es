---
title: Restricciones de nomenclatura de recursos
description: Muestra las reglas y las restricciones de nomenclatura de recursos de Azure.
ms.topic: conceptual
ms.date: 11/09/2021
ms.openlocfilehash: 868fa30779048447014fa0e7b048d6de8051931c
ms.sourcegitcommit: 838413a8fc8cd53581973472b7832d87c58e3d5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2021
ms.locfileid: "132137540"
---
# <a name="naming-rules-and-restrictions-for-azure-resources"></a>Reglas y restricciones de nomenclatura para los recursos de Azure

En este artículo se resumen las reglas y las restricciones de nomenclatura de los recursos de Azure. Para obtener recomendaciones sobre cómo asignar nombres a los recursos, vea [Convenciones de nomenclatura y etiquetado recomendados](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging).

En este artículo se enumeran los recursos por espacio de nombres del proveedor de recursos. Para obtener una lista de cómo se asocian los proveedores de recursos a los servicios de Azure, consulte [Proveedores de recursos para servicios de Azure](azure-services-resource-providers.md).

Los nombres de los recursos no distinguen mayúsculas de minúsculas, a menos que se indique en la columna de caracteres válidos.

> [!NOTE]
> Al recuperar los nombres de los recursos con distintas API, los valores devueltos pueden mostrar valores de mayúsculas y minúsculas diferentes a los que aparecen en la tabla de caracteres válidos.

En las tablas siguientes, el término carácter alfanumérico se refiere a:

* **a** a la **z** (letras minúsculas)
* **A** a la **Z** (letras mayúsculas)
* **0** a **9** (números)

> [!NOTE]
> No todos los recursos con un punto de conexión público pueden incluir palabras reservadas o marcas comerciales en el nombre. Para ver una lista de las palabras bloqueadas, consulte [Resolución de errores en los nombres de recursos reservados](../templates/error-reserved-resource-name.md).

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | servers | resource group | 3-63 | Letras minúsculas y números.<br><br>Comience con una letra minúscula. |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | service | global | 1-50 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/apis | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/apis/issues | api | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/apis/issues/attachments | issue | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/apis/issues/comments | issue | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/apis/operations | api | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/apis/operations/tags | operation | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/apis/releases | api | 1-80 | Caracteres alfanuméricos, de subrayado y guiones.<br><br>Comience y termine con un carácter alfanumérico o de subrayado. |
> | service/apis/schemas | api | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/apis/tagDescriptions | api | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/apis/tags | api | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/api-version-sets | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/authorizationServers | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/backends | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/certificates | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/diagnostics | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/groups | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/groups/users | group | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/identityProviders | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/loggers | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/notifications | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/notifications/recipientEmails | notificación | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/openidConnectProviders | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/policies | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/products | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/products/apis | product | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/products/groups | product | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/products/tags | product | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/properties | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/subscriptions | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/tags | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/templates | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | service/users | service | 1-80 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | configurationStores | resource group | 5-50 | Caracteres alfanuméricos, de subrayado y guiones. |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | locks | ámbito de asignación | 1-90 | Caracteres alfanuméricos, de subrayado, puntos, guiones y paréntesis.<br><br>No puede terminar con un punto. |
> | policyAssignments | ámbito de asignación | 1-128 nombre para mostrar<br><br>1-64 nombre de recurso<br><br>1-24 nombre del recurso en el ámbito del grupo de administración | El nombre para mostrar puede contener cualquier carácter.<br><br>El nombre del recurso no puede usar:<br>`<>*%&:\?.+/` ni caracteres de control. <br><br>No puede terminar con un punto ni un espacio. |
> | policyDefinitions | ámbito de definición | 1-128 nombre para mostrar<br><br>1-64 nombre de recurso | El nombre para mostrar puede contener cualquier carácter.<br><br>El nombre del recurso no puede usar:<br>`<>*%&:\?.+/` ni caracteres de control. <br><br>No puede terminar con un punto ni un espacio. |
> | policySetDefinitions | ámbito de definición | 1-128 nombre para mostrar<br><br>1-64 nombre de recurso<br><br>1-24 nombre del recurso en el ámbito del grupo de administración | El nombre para mostrar puede contener cualquier carácter.<br><br>El nombre del recurso no puede usar:<br>`<>*%&:\?.+/` ni caracteres de control. <br><br>No puede terminar con un punto ni un espacio. |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | automationAccounts | grupo de recursos y región <br>(consulte la nota a continuación) | 6-50 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra y termine con un carácter alfanumérico. |
> | automationAccounts/certificates | cuenta de Automation | 1-128 | No puede usar:<br> `<>*%&:\?.+/` ni caracteres de control <br><br>No puede terminar con un espacio.  |
> | automationAccounts/connections | cuenta de Automation | 1-128 | No puede usar:<br> `<>*%&:\?.+/` ni caracteres de control <br><br>No puede terminar con un espacio. |
> | automationAccounts/credentials | cuenta de Automation | 1-128 | No puede usar:<br> `<>*%&:\?.+/` ni caracteres de control <br><br>No puede terminar con un espacio. |
> | automationAccounts/runbooks | cuenta de Automation | 1-63 | Caracteres alfanuméricos, de subrayado y guiones.<br><br>Comience con una letra.  |
> | automationAccounts/schedules | cuenta de Automation | 1-128 | No puede usar:<br> `<>*%&:\?.+/` ni caracteres de control <br><br>No puede terminar con un espacio. |
> | automationAccounts/variables | cuenta de Automation | 1-128 | No puede usar:<br> `<>*%&:\?.+/` ni caracteres de control <br><br>No puede terminar con un espacio. |
> | automationAccounts/watchers | cuenta de Automation | 1-63 |  Caracteres alfanuméricos, de subrayado y guiones.<br><br>Comience con una letra. |
> | automationAccounts/webhooks | cuenta de Automation | 1-128 | No puede usar:<br> `<>*%&:\?.+/` ni caracteres de control <br><br>No puede terminar con un espacio. |

> [!NOTE]
> Los nombres de la cuenta de Automation son únicos en cada región y grupo de recursos. Es posible que los nombres de las cuentas de Automation eliminadas no estén disponibles de inmediato.

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | batchAccounts | Region | 3-24 | Letras minúsculas y números. |
> | batchAccounts/applications | cuenta de Batch | 1-64 | Caracteres alfanuméricos, de subrayado y guiones. |
> | batchAccounts/certificates | cuenta de Batch | 5-45 | Caracteres alfanuméricos, de subrayado y guiones. |
> | batchAccounts/pools | cuenta de Batch | 1-64 | Caracteres alfanuméricos, de subrayado y guiones. |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | blockchainMembers | global | 2-20 | Letras minúsculas y números.<br><br>Comience con una letra minúscula. |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | botServices | global | 2-64 |  Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. |
> | botServices/channels | servicio de bots | 2-64 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. |
> | botServices/Connections | servicio de bots | 2-64 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. |
> | enterpriseChannels | resource group | 2-64 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | Redis | global | 1-63 | Caracteres alfanuméricos y guiones.<br><br>Comience y termine con un carácter alfanumérico. No se permiten guiones consecutivos. |
> | Redis/firewallRules | Redis | 1-256 | Caracteres alfanuméricos |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | profiles | resource group | 1-260 | Caracteres alfanuméricos y guiones.<br><br>Comience y termine con un carácter alfanumérico. |
> | profiles/endpoints | global | 1-50 | Caracteres alfanuméricos y guiones.<br><br>Comience y termine con un carácter alfanumérico. |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | certificateOrders | resource group | 3-30 | Caracteres alfanuméricos. |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | accounts | resource group | 2-64 | Caracteres alfanuméricos y guiones.<br><br>Comience y termine con un carácter alfanumérico. |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | availabilitySets | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | cloudservices | resource group | 1-15 <br><br>Vea la nota siguiente. | No se pueden usar espacios, caracteres de control ni estos caracteres:<br> `~ ! @ # $ % ^ & * ( ) = + _ [ ] { } \ | ; : . ' " , < > / ?`<br><br>No puede comenzar con un carácter de subrayado. No puede terminar con un punto ni un guion. |
> | diskEncryptionSets | resource group | 1-80 | Caracteres alfanuméricos, de subrayado y guiones. |
> | disks | resource group | 1-80 | Caracteres alfanuméricos, de subrayado y guiones. |
> | galleries | resource group | 1-80 | Caracteres alfanuméricos y puntos.<br><br>Comience y termine con un carácter alfanumérico. |
> | galleries/applications | galería | 1-80 | Caracteres alfanuméricos, guiones y puntos.<br><br>Comience y termine con un carácter alfanumérico. |
> | galleries/applications/versions | application | Entero de 32 bits | Números y puntos. |
> | galleries/images | galería | 1-80 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience y termine con un carácter alfanumérico. |
> | galleries/images/versions | imagen | Entero de 32 bits | Números y puntos. |
> | images | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | snapshots | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | virtualMachines | resource group | 1-15 (Windows)<br>1-64 (Linux)<br><br>Vea la nota siguiente. | No se pueden usar espacios, caracteres de control ni estos caracteres:<br> `~ ! @ # $ % ^ & * ( ) = + _ [ ] { } \ | ; : . ' " , < > / ?`<br><br>Las máquinas virtuales no pueden incluir ningún punto ni terminar con un guion.<br><br>Las máquinas virtuales Linux no pueden terminar con un punto ni un guion. |
> | virtualMachineScaleSets | resource group | 1-15 (Windows)<br>1-64 (Linux)<br><br>Vea la nota siguiente. | No se pueden usar espacios, caracteres de control ni estos caracteres:<br> `~ ! @ # $ % ^ & * ( ) = + _ [ ] { } \ | ; : . ' " , < > / ?`<br><br>No puede comenzar con un carácter de subrayado. No puede terminar con un punto ni un guion. |

> [!NOTE]
> Las máquinas virtuales de Azure tienen dos nombres distintos: el nombre de recurso y el nombre de host. Cuando se crea una máquina virtual en el portal, se usa el mismo valor para ambos nombres. Las restricciones de la tabla anterior son para el nombre de host. El nombre de recurso real puede tener hasta 64 caracteres.

## <a name="microsoftcommunication"></a>Microsoft.Communication

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | communicationServices | global | 1-63 | Caracteres alfanuméricos y guiones.<br><br>No se pueden usar caracteres de subrayado. |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | budgets | suscripción o grupo de recursos | 1-63 | Caracteres alfanuméricos, de subrayado y guiones. |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | containerGroups | resource group | 1-63 | Letras minúsculas, números y guiones.<br><br>No puede comenzar ni terminar con un guion. No se permiten guiones consecutivos. |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | registries | global | 5-50 | Caracteres alfanuméricos. |
> | registries/buildTasks | Registro | 5-50 | Caracteres alfanuméricos. |
> | registries/buildTasks/steps | tarea de compilación | 5-50 | Caracteres alfanuméricos. |
> | registries/replications | Registro | 5-50 | Caracteres alfanuméricos. |
> | registries/scopeMaps | Registro | 5-50 | Caracteres alfanuméricos, de subrayado y guiones. |
> | registries/tasks | Registro | 5-50 | Caracteres alfanuméricos, de subrayado y guiones. |
> | registries/tokens | Registro | 5-50 | Caracteres alfanuméricos, de subrayado y guiones. |
> | registries/webhooks | Registro | 5-50 | Caracteres alfanuméricos. |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | managedClusters | resource group | 1-63 | Caracteres alfanuméricos, de subrayado y guiones.<br><br>Comience y termine con un carácter alfanumérico. |
> | openShiftManagedClusters | resource group | 1-30 | Caracteres alfanuméricos. |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | hubs | resource group | 1-64 | Caracteres alfanuméricos.<br><br>Comience con una letra.  |
> | hubs/authorizationPolicies | centro | 1-50 | Caracteres alfanuméricos, de subrayado y puntos.<br><br>Comience y termine con un carácter alfanumérico. |
> | hubs/connectors | centro | 1-128 | Caracteres alfanuméricos y de subrayado.<br><br>Comience con una letra. |
> | hubs/connectors/mappings | Conector | 1-128 | Caracteres alfanuméricos y de subrayado.<br><br>Comience con una letra. |
> | hubs/interactions | centro | 1-128 | Caracteres alfanuméricos y de subrayado.<br><br>Comience con una letra. |
> | hubs/kpi | centro | 1-512 | Caracteres alfanuméricos y de subrayado.<br><br>Comience con una letra. |
> | hubs/links | centro | 1-512 | Caracteres alfanuméricos y de subrayado.<br><br>Comience con una letra. |
> | hubs/predictions | centro | 1-512 | Caracteres alfanuméricos y de subrayado.<br><br>Comience con una letra. |
> | hubs/profiles | centro | 1-128 | Caracteres alfanuméricos y de subrayado.<br><br>Comience con una letra. |
> | hubs/relationshipLinks | centro | 1-512 | Caracteres alfanuméricos y de subrayado.<br><br>Comience con una letra. |
> | hubs/relationships | centro | 1-512 | Caracteres alfanuméricos y de subrayado.<br><br>Comience con una letra. |
> | hubs/roleAssignments | centro | 1-128 | Caracteres alfanuméricos y de subrayado.<br><br>Comience con una letra. |
> | hubs/views | centro | 1-512 | Caracteres alfanuméricos y de subrayado.<br><br>Comience con una letra. |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | associations | resource group | 1-180 | No puede usar:<br>`%&\\?/` ni caracteres de control<br><br>No puede terminar con un punto ni un espacio. |
> | resourceProviders | resource group | 3-64 | No puede usar:<br>`%&\\?/` ni caracteres de control<br><br>No puede terminar con un punto ni un espacio. |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | jobs | resource group | 3-24 | Caracteres alfanuméricos, de subrayado, guiones y puntos. |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | workspaces | resource group | 3-64 | Caracteres alfanuméricos, de subrayado y guiones. |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | factories | global | 3-63 | Caracteres alfanuméricos y guiones.<br><br>Comience y termine con un carácter alfanumérico. |
> | factories/dataflows | fábrica | 1-260 | No puede usar:<br>`<>*#.%&:\\+?/` ni caracteres de control<br><br>Comience con un carácter alfanumérico. |
> | factories/datasets | fábrica | 1-260 | No puede usar:<br>`<>*#.%&:\\+?/` ni caracteres de control<br><br>Comience con un carácter alfanumérico. |
> | factories/integrationRuntimes | fábrica | 3-63 | Caracteres alfanuméricos y guiones.<br><br>Comience y termine con un carácter alfanumérico. |
> | factories/linkedservices | fábrica | 1-260 | No puede usar:<br>`<>*#.%&:\\+?/` ni caracteres de control<br><br>Comience con un carácter alfanumérico. |
> | factories/pipelines | fábrica | 1-260 | No puede usar:<br>`<>*#.%&:\\+?/` ni caracteres de control<br><br>Comience con un carácter alfanumérico. |
> | factories/triggers | fábrica | 1-260 | No puede usar:<br>`<>*#.%&:\\+?/` ni caracteres de control<br><br>Comience con un carácter alfanumérico. |
> | factories/triggers/rerunTriggers | desencadenador | 1-260 | No puede usar:<br>`<>*#.%&:\\+?/` ni caracteres de control<br><br>Comience con un carácter alfanumérico. |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | accounts | global | 3-24 | Letras minúsculas y números. |
> | accounts/computePolicies | account | 3-60 | Caracteres alfanuméricos, de subrayado y guiones. |
> | accounts/dataLakeStoreAccounts | account | 3-24 | Letras minúsculas y números. |
> | accounts/firewallRules | account | 3-50 | Caracteres alfanuméricos, de subrayado y guiones. |
> | accounts/storageAccounts | account | 3-60 | Caracteres alfanuméricos, de subrayado y guiones. |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | accounts | global | 3-24 | Letras minúsculas y números. |
> | accounts/firewallRules | account | 3-50 | Caracteres alfanuméricos, de subrayado y guiones. |
> | accounts/virtualNetworkRules | account | 3-50 | Caracteres alfanuméricos, de subrayado y guiones. |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | services | resource group | 2-62 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience con un carácter alfanumérico. |
> | services/projects | service | 2-57 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience con un carácter alfanumérico. |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | servers | global | 3-63 | Letras minúsculas, guiones y números.<br><br>No puede comenzar ni terminar con un guion. |
> | servers/databases | servers | 1-63 | Caracteres alfanuméricos y guiones. |
> | servers/firewallRules | servers | 1-128 | Caracteres alfanuméricos, de subrayado y guiones. |
> | servers/virtualNetworkRules | servers | 1-128 | Caracteres alfanuméricos y guiones. |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | servers | global | 3-63 | Letras minúsculas, guiones y números.<br><br>No puede comenzar ni terminar con un guion. |
> | servers/databases | servers | 1-63 | Caracteres alfanuméricos y guiones. |
> | servers/firewallRules | servers | 1-128 | Caracteres alfanuméricos, de subrayado y guiones. |
> | servers/virtualNetworkRules | servers | 1-128 | Caracteres alfanuméricos y guiones. |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | servers | global | 3-63 | Letras minúsculas, guiones y números.<br><br>No puede comenzar ni terminar con un guion. |
> | servers/databases | servers | 1-63 | Caracteres alfanuméricos y guiones. |
> | servers/firewallRules | servers | 1-128 | Caracteres alfanuméricos, de subrayado y guiones. |
> | servers/virtualNetworkRules | servers | 1-128 | Caracteres alfanuméricos y guiones. |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | IotHubs | global | 3-50 | Caracteres alfanuméricos y guiones.<br><br>No puede terminar con un guion. |
> | IotHubs/certificates | centro de IoT | 1-64 | Caracteres alfanuméricos, de subrayado, guiones y puntos. |
> | IotHubs/eventHubEndpoints/ConsumerGroups | eventHubEndpoints | 1-50 | Caracteres alfanuméricos, de subrayado, guiones y puntos. |
> | provisioningServices | resource group | 3-64 | Caracteres alfanuméricos y guiones.<br><br>Termine con un carácter alfanumérico. |
> | provisioningServices/certificates | provisioningServices | 1-64 | Caracteres alfanuméricos, de subrayado, guiones y puntos. |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | labs | resource group | 1-50 | Caracteres alfanuméricos, de subrayado y guiones. |
> | labs/customimages | laboratorio | 1-80 | Caracteres alfanuméricos, de subrayado, guiones y paréntesis. |
> | labs/formulas | laboratorio | 1-80 | Caracteres alfanuméricos, de subrayado, guiones y paréntesis. |
> | labs/virtualmachines | laboratorio | 1-15 (Windows)<br>1-64 (Linux) | Caracteres alfanuméricos y guiones.<br><br>Comience y termine con un carácter alfanumérico. No puede constar de números en su totalidad. |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | databaseAccounts | global | 3-44 | Letras minúsculas, números y guiones.<br><br>Comience con una letra minúscula o un número. |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | domains | resource group | 3-50 | Caracteres alfanuméricos y guiones. |
> | domains/topics | dominio | 3-50 | Caracteres alfanuméricos y guiones. |
> | eventSubscriptions | resource group | 3-64 | Caracteres alfanuméricos y guiones. |
> | topics | resource group | 3-50 | Caracteres alfanuméricos y guiones. |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | clusters | resource group | 6-50 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra. Termine con una letra o un número. |
> | espacios de nombres | global | 6-50 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra. Termine con una letra o un número. |
> | namespaces/AuthorizationRules | espacio de nombres | 1-50 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience y termine con una letra o un número. |
> | namespaces/disasterRecoveryConfigs | global | 6-50 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra. Termine con un carácter alfanumérico. |
> | namespaces/eventhubs | espacio de nombres | 1-50 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience y termine con una letra o un número. |
> | namespaces/eventhubs/authorizationRules | centro de eventos | 1-50 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience y termine con una letra o un número. |
> | namespaces/eventhubs/consumergroups | centro de eventos | 1-50 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience y termine con una letra o un número. |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | clusters | global | 3-59 | Caracteres alfanuméricos y guiones.<br><br>Comience y termine con una letra o un número. |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | jobs | resource group | 2-64 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra. |

## <a name="microsoftinsights"></a>Microsoft.Insights

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | actionGroups | resource group | 1-260 | No puede usar:<br>`:<>+/&%\?` ni caracteres de control <br><br>No puede terminar con un espacio ni punto.  |
> | components | resource group | 1-260 | No puede usar:<br>`%&\?/` ni caracteres de control <br><br>No puede terminar con un espacio ni punto.  |
> | scheduledQueryRules | resource group | 1-260 | No puede usar:<br>`*<>%{}&:\\?/#` ni caracteres de control <br><br>No puede terminar con un espacio ni punto.  |
> | metricAlerts | resource group | 1-260 | No puede usar:<br>`*#&+:<>?@%{}\/` ni caracteres de control <br><br>No puede terminar con un espacio ni punto.  |
> | activityLogAlerts | resource group | 1-260 | No puede usar:<br>`<>*%{}&:\\?+/#` ni caracteres de control <br><br>No puede terminar con un espacio ni punto.  |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | IoTApps | global | 2-63 | Letras minúsculas, números y guiones.<br><br>Comience con una letra minúscula o un número. |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | vaults | global | 3-24 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra. Termine con una letra o un dígito. No puede contener guiones consecutivos. |
> | vaults/secrets | Almacén | 1-127 | Caracteres alfanuméricos y guiones. |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | clusters | global | 4-22 | Letras minúsculas y números.<br><br>Comience con una letra. |
> | /clusters/databases | cluster | 1-260 | Caracteres alfanuméricos, guiones, espacios y puntos. |
> | /clusters/databases/dataConnections | database | 1-40 | Caracteres alfanuméricos, guiones, espacios y puntos. |
> | /clusters/databases/eventhubconnections | database | 1-40 | Caracteres alfanuméricos, guiones, espacios y puntos. |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | integrationAccounts | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos, guiones y paréntesis. |
> | integrationAccounts/assemblies | cuenta de integración | 1-80 | Caracteres alfanuméricos, de subrayado, puntos, guiones y paréntesis. |
> | integrationAccounts/batchConfigurations | cuenta de integración | 1-20 | Caracteres alfanuméricos. |
> | integrationAccounts/certificates | cuenta de integración | 1-80 | Caracteres alfanuméricos, de subrayado, puntos, guiones y paréntesis. |
> | integrationAccounts/maps | cuenta de integración | 1-80 | Caracteres alfanuméricos, de subrayado, puntos, guiones y paréntesis. |
> | integrationAccounts/partners | cuenta de integración | 1-80 | Caracteres alfanuméricos, de subrayado, puntos, guiones y paréntesis. |
> | integrationAccounts/rosettanetprocessconfigurations | cuenta de integración | 1-80 | Caracteres alfanuméricos, de subrayado, puntos, guiones y paréntesis. |
> | integrationAccounts/schemas | cuenta de integración | 1-80 | Caracteres alfanuméricos, de subrayado, puntos, guiones y paréntesis. |
> | integrationAccounts/sessions | cuenta de integración | 1-80 | Caracteres alfanuméricos, de subrayado, puntos, guiones y paréntesis. |
> | integrationServiceEnvironments | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, guiones y puntos. |
> | integrationServiceEnvironments/managedApis | entorno de servicio de integración | 1-80 | Caracteres alfanuméricos, de subrayado, guiones y puntos. |
> | workflows | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos, guiones y paréntesis. |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | commitmentPlans | resource group | 1-260 | No puede usar:<br>`<>*%&:?+/\\` ni caracteres de control<br><br>No puede terminar con un espacio. |
> | webServices | resource group | 1-260 | No puede usar:<br>`<>*%&:?+/\\` ni caracteres de control<br><br>No puede terminar con un espacio. |
> | workspaces | resource group | 1-260 | No puede usar:<br>`<>*%&:?+/\\` ni caracteres de control<br><br>No puede terminar con un espacio. |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | workspaces | resource group | 3-33 | Caracteres alfanuméricos y guiones. |
> | workspaces/computes | área de trabajo | 2-16 | Caracteres alfanuméricos y guiones. |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | userAssignedIdentities | resource group | 3-128 | Caracteres alfanuméricos, de subrayado y guiones.<br><br>Comience con una letra o un número. |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | accounts | resource group | 1-98 (para nombre de grupo de recursos y nombre de cuenta) | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | mediaservices | resource group | 3-24 | Letras minúsculas y números. |
> | mediaservices/liveEvents | Servicio multimedia | 1-32 | Caracteres alfanuméricos y guiones.<br><br>Comience con un carácter alfanumérico. |
> | mediaservices/liveEvents/liveOutputs | Evento en directo | 1-256 | Caracteres alfanuméricos y guiones.<br><br>Comience con un carácter alfanumérico. |
> | mediaservices/streamingEndpoints | Servicio multimedia | 1-24 | Caracteres alfanuméricos y guiones.<br><br>Comience con un carácter alfanumérico. |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | applicationGateways | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | applicationSecurityGroups | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | azureFirewalls | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | bastionHosts | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | connections | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | dnsZones | resource group | 1 a 63 caracteres<br><br>2 a 34 etiquetas<br><br>Cada etiqueta es un conjunto de caracteres separados por un punto. Por ejemplo, **contoso.com** tiene dos etiquetas. | Cada etiqueta puede contener caracteres alfanuméricos, de subrayado y guiones.<br><br>Cada etiqueta está separada por un punto. |
> | expressRouteCircuits | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | firewallPolicies | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | firewallPolicies/ruleGroups | directiva de firewall | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | frontDoors | global | 5-64 | Caracteres alfanuméricos y guiones.<br><br>Comience y termine con un carácter alfanumérico. |
> | frontdoorWebApplicationFirewallPolicies | resource group | 1-128 | Caracteres alfanuméricos.<br><br>Comience con una letra. |
> | loadBalancers | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | loadBalancers/inboundNatRules | load balancer | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | localNetworkGateways | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | networkInterfaces | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | networkSecurityGroups | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | networkSecurityGroups/securityRules | grupo de seguridad de red | 1-80 |  Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | networkWatchers | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | privateDnsZones | resource group | 1 a 63 caracteres<br><br>2 a 34 etiquetas<br><br>Cada etiqueta es un conjunto de caracteres separados por un punto. Por ejemplo, **contoso.com** tiene dos etiquetas. | Cada etiqueta puede contener caracteres alfanuméricos, de subrayado y guiones.<br><br>Cada etiqueta está separada por un punto. |
> | privateDnsZones/virtualNetworkLinks | zona DNS privada | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | publicIPAddresses | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | publicIPPrefixes | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | routeFilters | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | routeFilters/routeFilterRules | filtro de ruta | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | routeTables | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | routeTables/routes | tabla de rutas | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | serviceEndpointPolicies | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | trafficmanagerprofiles | global | 1-63 | Caracteres alfanuméricos, guiones y puntos.<br><br>Comience y termine con un carácter alfanumérico. |
> | virtualNetworkGateways | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | virtualNetworks | resource group | 2-64 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | virtualnetworks/subnets | red virtual | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | virtualNetworks/virtualNetworkPeerings | red virtual | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | virtualWans | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | vpnGateways | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | vpnGateways/vpnConnections | puerta de enlace de VPN | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |
> | vpnSites | resource group | 1-80 | Caracteres alfanuméricos, de subrayado, puntos y guiones.<br><br>Comience con un carácter alfanumérico. Termine con un carácter alfanumérico o de subrayado. |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | espacios de nombres | global | 6-50 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra. Termine con un carácter alfanumérico. |
> | namespaces/AuthorizationRules | espacio de nombres | 1-256 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience con carácter alfanumérico. |
> | namespaces/notificationHubs | espacio de nombres | 1-260 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience con carácter alfanumérico. |
> | namespaces/notificationHubs/AuthorizationRules | centro de notificaciones | 1-256 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience con carácter alfanumérico. |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | clusters | resource group | 4-63 | Caracteres alfanuméricos y guiones.<br><br>Comience y termine con un carácter alfanumérico. |
> | workspaces | global | 4-63 | Caracteres alfanuméricos y guiones.<br><br>Comience y termine con un carácter alfanumérico. |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | solutions | área de trabajo | N/D | En el caso de las soluciones creadas por Microsoft, el nombre debe seguir el patrón:<br>`SolutionType(WorkspaceName)`<br><br>En el caso de las soluciones creadas por terceros, el nombre debe seguir el patrón:<br>`SolutionType[WorkspaceName]`<br><br>Por ejemplo, un nombre válido es:<br>`AntiMalware(contoso-IT)`<br><br>El tipo de solución distingue entre mayúsculas y minúsculas. |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | dashboards | resource group | 3-160 | Caracteres alfanuméricos y guiones.<br><br>Para usar caracteres restringidos, agregue una etiqueta denominada **hidden-title** con el nombre del panel que desee usar. El portal muestra ese nombre al mostrar el panel. |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | workspaceCollections | region | 3-63 | Caracteres alfanuméricos y guiones.<br><br>No puede comenzar con un guion. No puede usar guiones consecutivos. |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | capacities | region | 3-63 | Letras minúsculas o números.<br><br>Comience con una letra minúscula. |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | vaults | resource group | 2-50 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra. |
> | vaults/backupPolicies | almacén | 3-150 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra. No puede terminar con un guion. |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | espacios de nombres | global | 6-50 | Caracteres alfanuméricos y guiones.<br><br>Empiece con una letra. Termine con una letra o un número. |
> | namespaces/AuthorizationRules | espacio de nombres | 1-50 |  Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience y termine con un carácter alfanumérico. |
> | namespaces/HybridConnections | espacio de nombres | 1-260 | Caracteres alfanuméricos, de subrayado, puntos, guiones y barras.<br><br>Comience y termine con un carácter alfanumérico. |
> | namespaces/HybridConnections/authorizationRules | conexión híbrida | 1-50 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience y termine con un carácter alfanumérico. |
> | namespaces/WcfRelays | espacio de nombres | 1-260 | Caracteres alfanuméricos, de subrayado, puntos, guiones y barras.<br><br>Comience y termine con un carácter alfanumérico. |
> | namespaces/WcfRelays/authorizationRules | Wcf relay | 1-50 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience y termine con un carácter alfanumérico. |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | deployments | resource group | 1-64 | Caracteres alfanuméricos, de subrayado, puntos, guiones y paréntesis. |
> | resourcegroups | subscription | 1-90 | Caracteres alfanuméricos, de subrayado, paréntesis, guiones, puntos y caracteres Unicode que coincidan con la [documentación de regex](/rest/api/resources/resourcegroups/createorupdate).<br><br>No puede terminar con un punto. |
> | tagNames | resource | 1-512 | No puede usar:<br>`<>%&\?/` ni caracteres de control |
> | tagNames/tagValues | nombre de etiqueta | 1-256 | Todos los caracteres. |
> | templateSpecs | resource group | 1-90 | Caracteres alfanuméricos, de subrayado, puntos, guiones y paréntesis. |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | advancedThreatProtectionSettings | resource group | ver valor | Debe ser `current` |
> | alertsSuppressionRules | subscription | 1-260 | Caracteres alfanuméricos, de subrayado y guiones. |
> | assessmentMetadata | Tipo de evaluación | 1-260 | Caracteres alfanuméricos, de subrayado y guiones. |
> | assessments | Tipo de evaluación | 1-260 | Caracteres alfanuméricos, de subrayado y guiones. |
> | automations | resource group | 1-260 | Caracteres alfanuméricos, de subrayado y guiones. |
> | autoProvisioningSettings | subscription | 1-260 | Caracteres alfanuméricos, de subrayado y guiones. |
> | conectores | subscription | 1-260 | Caracteres alfanuméricos, de subrayado y guiones. |
> | deviceSecurityGroups | resource group | 1-260 | Caracteres alfanuméricos, de subrayado y guiones. |
> | informationProtectionPolicies | resource group | ver valores | Elija uno:<br>`custom`<br>`effective` | 
> | iotSecuritySolutions | resource group | 1-260 | Caracteres alfanuméricos, de subrayado y guiones. |
> | locations/applicationwhitelistings | subscription | 1-260 | Caracteres alfanuméricos, de subrayado y guiones. |
> | locations/jitnetworkaccesspolicies | resource group | 1-260 | Caracteres alfanuméricos, de subrayado y guiones. |
> | ingestionSettings | subscription | 1-260 | Caracteres alfanuméricos, de subrayado y guiones. |
> | pricings | subscription | 1-260 | Caracteres alfanuméricos, de subrayado y guiones. |
> | securityContacts | subscription | 1-260 | Caracteres alfanuméricos, de subrayado y guiones. |
> | configuración | subscription | ver valores | Elija uno:<br>`MCAS`<br>`Sentinel`<br>`WDATP`<br>`WDATP_EXCLUDE_LINUX_PUBLIC_PREVIEW` |
> | serverVulnerabilityAssessments | tipo de recurso | ver valor | Debe ser `Default` |
> | sqlVulnerabilityAssessments/baselineRules | Evaluación de vulnerabilidades | 1-260 | Caracteres alfanuméricos, de subrayado y guiones. |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | espacios de nombres | global | 6-50 | Caracteres alfanuméricos y guiones.<br><br>Empiece con una letra. Termine con una letra o un número.<br><br>Para más información, consulte [Creación de un espacio de nombres](/rest/api/servicebus/create-namespace). |
> | namespaces/AuthorizationRules | espacio de nombres | 1-50 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience y termine con un carácter alfanumérico. |
> | namespaces/disasterRecoveryConfigs | global | 6-50 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra. Termine con un carácter alfanumérico. |
> | namespaces/migrationConfigurations | espacio de nombres |  | Debe ser siempre **$default**. |
> | namespaces/queues | espacio de nombres | 1-260 | Caracteres alfanuméricos, de subrayado, puntos, guiones y barras.<br><br>Comience y termine con un carácter alfanumérico. |
> | namespaces/queues/authorizationRules | cola | 1-50 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience y termine con un carácter alfanumérico. |
> | namespaces/topics | espacio de nombres | 1-260 | Caracteres alfanuméricos, de subrayado, puntos, guiones y barras.<br><br>Comience y termine con un carácter alfanumérico. |
> | namespaces/topics/authorizationRules | topic | 1-50 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience y termine con un carácter alfanumérico. |
> | namespaces/topics/subscriptions | topic | 1-50 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience y termine con un carácter alfanumérico. |
> | namespaces/topics/subscriptions/rules | subscription | 1-50 | Caracteres alfanuméricos, de subrayado, guiones y puntos.<br><br>Comience y termine con un carácter alfanumérico. |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | clusters | region | 4-23 | Letras minúsculas, números y guiones.<br><br>Comience con una letra minúscula. Termine con una letra minúscula o un número. |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | signalR | global | 3-63 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra. Termine con una letra o un número.  |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | managedInstances | global | 1-63 | Letras minúsculas, números y guiones.<br><br>No puede comenzar ni terminar con un guion. <br><br> No puede contener caracteres especiales, como `@`. |
> | servers | global | 1-63 | Letras minúsculas, números y guiones.<br><br>No puede comenzar ni terminar con un guion. |
> | servers/administrators | server |  | Debe ser `ActiveDirectory`. |
> | servers/databases | server | 1-128 | No puede usar:<br>`<>*%&:\/?` ni caracteres de control<br><br>No puede terminar con un punto ni un espacio. |
> | servers/databases/syncGroups | database | 1-150 | Caracteres alfanuméricos, de subrayado y guiones. |
> | servers/elasticPools | server | 1-128 | No puede usar:<br>`<>*%&:\/?` ni caracteres de control<br><br>No puede terminar con un punto ni un espacio. |
> | servers/failoverGroups | global | 1-63 | Letras minúsculas, números y guiones.<br><br>No puede comenzar ni terminar con un guion. |
> | servers/firewallRules | server | 1-128 | No puede usar:<br>`<>*%&:;\/?` ni caracteres de control<br><br>No puede terminar con un punto. |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | storageAccounts | global | 3-24 | Letras minúsculas y números. |
> | storageAccounts/blobServices | Cuenta de almacenamiento |  | Debe ser `default`. |
> | storageAccounts/blobServices/containers | Cuenta de almacenamiento | 3-63 | Letras minúsculas, números y guiones.<br><br>Comience con una letra minúscula o un número. No puede usar guiones consecutivos. |
> | storageAccounts/fileServices | Cuenta de almacenamiento |  | Debe ser `default`. |
> | storageAccounts/fileServices/shares | Cuenta de almacenamiento | 3-63 | Letras minúsculas, números y guiones.<br><br>No puede comenzar ni terminar con un guion. No puede usar guiones consecutivos. |
> | storageAccounts/managementPolicies | Cuenta de almacenamiento |  | Debe ser `default`. |
> | blob | contenedor | 1-1024 | Cualquier carácter de dirección URL, distingue mayúsculas de minúsculas. |
> | cola | Cuenta de almacenamiento | 3-63 | Letras minúsculas, números y guiones.<br><br>No puede comenzar ni terminar con un guion. No puede usar guiones consecutivos. |
> | table | Cuenta de almacenamiento | 3-63 | Caracteres alfanuméricos.<br><br>Comience con una letra. |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | storageSyncServices | resource group | 1-260 | Caracteres alfanuméricos, de subrayado, espacios, guiones y puntos.<br><br>No puede terminar con un punto ni un espacio. |
> | storageSyncServices/syncGroups | servicio de sincronización de almacenamiento | 1-260 | Caracteres alfanuméricos, de subrayado, espacios, guiones y puntos.<br><br>No puede terminar con un punto ni un espacio. |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | managers | resource group | 2-50 | Caracteres alfanuméricos y guiones.<br><br>Comience con una letra. Termine con un carácter alfanumérico. |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | streamingjobs | resource group | 3-63 | Caracteres alfanuméricos, de subrayado y guiones. |
> | streamingjobs/functions | trabajo de streaming | 3-63 | Caracteres alfanuméricos, de subrayado y guiones. |
> | streamingjobs/inputs | trabajo de streaming | 3-63 | Caracteres alfanuméricos, de subrayado y guiones. |
> | streamingjobs/outputs | trabajo de streaming | 3-63 | Caracteres alfanuméricos, de subrayado y guiones. |
> | streamingjobs/transformations | trabajo de streaming | 3-63 | Caracteres alfanuméricos, de subrayado y guiones. |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | environments | resource group | 1-90 | No puede usar:<br>`'<>%&:\?/#` ni caracteres de control |
> | environments/accessPolicies | Environment | 1-90 | No puede usar:<br> `'<>%&:\?/#` ni caracteres de control |
> | environments/eventSources | Environment | 1-90 | No puede usar:<br>`'<>%&:\?/#` ni caracteres de control |
> | environments/referenceDataSets | Environment | 3-63 | Caracteres alfanuméricos |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Entidad | Ámbito | Length | Caracteres válidos |
> | --- | --- | --- | --- |
> | certificates | resource group | 1-260 | No puede usar:<br>`/` <br><br>No puede terminar con un espacio ni punto.  | 
> | serverfarms | resource group | 1-40 | Caracteres alfanuméricos y guiones. |
> | sites | global o por dominio. Vea la nota siguiente. | 2-60 | Contiene caracteres alfanuméricos y guiones.<br><br>No puede comenzar ni terminar con un guion. |
> | sites/slots | site | 2-59 | Caracteres alfanuméricos y guiones. |

> [!NOTE]
> Un sitio web debe tener una dirección URL globalmente única. Cuando se crea un sitio web que utiliza un plan de hospedaje, la dirección URL es `http://<app-name>.azurewebsites.net`. El nombre de la aplicación debe ser único globalmente. Cuando se crea un sitio web que usa una instancia de App Service Environment, el nombre de la aplicación debe ser único dentro del [dominio de la instancia de App Service Environment](../../app-service/environment/using-an-ase.md#app-access). En ambos casos, la dirección URL del sitio es globalmente única.
>
> Azure Functions tiene las mismas reglas y restricciones de nomenclatura que Microsoft.Web/sites.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener recomendaciones sobre cómo asignar nombre a los recursos, consulte [Prepárese: Convenciones recomendadas de nomenclatura y etiquetado](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging).

* No todos los recursos con un punto de conexión público pueden incluir palabras reservadas o marcas comerciales en el nombre. Para ver una lista de las palabras bloqueadas, consulte [Resolución de errores en los nombres de recursos reservados](../templates/error-reserved-resource-name.md).
