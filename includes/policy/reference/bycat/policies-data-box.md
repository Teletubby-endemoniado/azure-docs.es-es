---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/17/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: ced65b2f28d5958909433b642a51f2a68c1a8a52
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128553936"
---
|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Los trabajos de Azure Data Box deben habilitar el cifrado doble para los datos en reposo en el dispositivo](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc349d81b-9985-44ae-a8da-ff98d108ede8) |Habilite una segunda capa de cifrado basado en software para los datos en reposo en el dispositivo. El dispositivo ya está protegido mediante el Estándar de cifrado avanzado de 256 bits para datos en reposo. Esta opción agrega una segunda capa de cifrado de datos. |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Box/DataBox_DoubleEncryption_Audit.json) |
|[Los trabajos de Azure Data Box deben usar una clave administrada por el cliente para cifrar la contraseña de desbloqueo del dispositivo](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F86efb160-8de7-451d-bc08-5d475b0aadae) |Use una clave administrada por el cliente para controlar el cifrado de la contraseña de desbloqueo del dispositivo para Azure Data Box. Las claves administradas por el cliente también ayudan a administrar el acceso a la contraseña de desbloqueo del dispositivo por parte del servicio Data Box para preparar el dispositivo y copiar los datos de forma automatizada. Los datos del propio dispositivo ya están cifrados en reposo con el Estándar de cifrado avanzado cifrado de 256 bits y la contraseña de desbloqueo del dispositivo se cifra de forma predeterminada con una clave administrada por Microsoft. |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Box/DataBox_CMK_Audit.json) |
