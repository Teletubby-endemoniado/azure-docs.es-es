---
author: georgewallace
ms.service: azure-policy
ms.topic: include
ms.date: 10/11/2021
ms.author: gwallace
ms.custom: generated
ms.openlocfilehash: 373e956bbbab5a9f4269ba3175f25b5bae9cdb47
ms.sourcegitcommit: 54e7b2e036f4732276adcace73e6261b02f96343
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2021
ms.locfileid: "129810150"
---
|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure API for FHIR debe usar una clave administrada por el cliente para cifrar los datos en reposo.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F051cba44-2429-45b9-9649-46cec11c7119) |Use claves administradas por el cliente para controlar el cifrado en reposo de los datos almacenados en Azure API for FHIR cuando exista un requisito normativo o de cumplimiento. Las claves administradas por el cliente también proporcionan cifrado doble, ya que agregan una segunda capa de cifrado además de la capa predeterminada que se creó mediante las claves administradas por el servicio. |audit, disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/API%20for%20FHIR/HealthcareAPIs_EnableByok_Audit.json) |
|[Azure API for FHIR debe usar un vínculo privado.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1ee56206-5dd1-42ab-b02d-8aae8b1634ce) |Azure API for FHIR debe tener al menos una conexión de punto de conexión privado aprobada. Los clientes de una red virtual pueden acceder de forma segura a los recursos que tengan conexiones de punto de conexión privadas mediante vínculos privados. Para más información, visite [https://aka.ms/fhir-privatelink](../../../../articles/healthcare-apis/azure-api-for-fhir/configure-private-link.md). |Audit, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/API%20for%20FHIR/HealthcareAPIs_PrivateLink_Audit.json) |
|[CORS no debe permitir que todos los dominios accedan a la API para FHIR](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0fea8f8a-4169-495d-8307-30ec335f387d). |El uso compartido de recursos entre orígenes (CORS) no debe permitir que todos los dominios accedan a API for FHIR. Para proteger la instancia de API for FHIR, elimine el acceso de todos los dominios y defina explícitamente los dominios que tienen permiso para conectarse. |audit, disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/API%20for%20FHIR/HealthcareAPIs_RestrictCORSAccess_Audit.json) |