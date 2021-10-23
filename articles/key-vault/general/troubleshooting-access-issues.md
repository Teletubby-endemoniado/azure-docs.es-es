---
title: Solución de problemas de las directivas de acceso de Azure Key Vault
description: Solución de problemas de las directivas de acceso de Azure Key Vault
author: sebansal
ms.author: sebansal
ms.date: 08/10/2020
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.openlocfilehash: c4d82e0193d891f423245ce2743ad34ac7bcf5d1
ms.sourcegitcommit: d2875bdbcf1bbd7c06834f0e71d9b98cea7c6652
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2021
ms.locfileid: "129856494"
---
# <a name="troubleshooting-azure-key-vault-access-policy-issues"></a>Solución de problemas de las directivas de acceso de Azure Key Vault

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="i-am-not-able-to-list-or-get-secretskeyscertificate-i-am-seeing-something-went-wrong-error"></a>No puedo mostrar u obtener secretos, claves o certificados. Veo que se ha producido un error... Error.
Si tiene problemas con la enumeración, la obtención y la creación del secreto o con el acceso a este, asegúrese de tener definida la directiva de acceso para realizar esa operación: [Autenticación en Key Vault con una directiva de control de acceso](./assign-access-policy-cli.md)

### <a name="how-can-i-identify-how-and-when-key-vaults-are-accessed"></a>¿De qué modo puedo identificar cómo y cuándo se accede a los almacenes de claves?

Después de crear uno o varios almacenes de claves, es probable que desee supervisar cómo y cuándo se accede a los almacenes de claves y quién lo hace. Para ello, puede habilitar el registro para Azure Key Vault. Si quiere consultar la guía paso a paso para habilitar el registro, [lea más información](./logging.md).

### <a name="how-can-i-monitor-vault-availability-service-latency-periods-or-other-performance-metrics-for-key-vault"></a>¿Cómo se puede supervisar la disponibilidad del almacén, los períodos de latencia del servicio u otras métricas de rendimiento para el almacén de claves?

Cuando empiece a escalar el servicio, aumentará el número de solicitudes que se envían al almacén de claves. Es probable que dicha demanda aumente la latencia de las solicitudes y, en casos extremos, puede hacer que las solicitudes se limiten, lo cual afectará al rendimiento del servicio. Puede supervisar las métricas de rendimiento del almacén de claves y recibir alertas de umbrales específicos en la guía paso a paso para configurar la supervisión [aquí](./alert.md).

### <a name="i-am-not-able-to-modify-access-policy-how-can-it-be-enabled"></a>No puedo modificar la directiva de acceso, ¿cómo se puede habilitar?
El usuario debe tener permisos de AAD suficientes para modificar la directiva de acceso. En este caso, el usuario debe tener un rol de colaborador o superior.

### <a name="i-am-seeing-unknown-policy-error-what-does-that-mean"></a>Aparece el error "Directiva desconocida". ¿Qué significa?
Hay dos posibilidades diferentes de ver la directiva de acceso en la sección Desconocido:
* Puede haber un usuario anterior que tuviera acceso y, por alguna razón, que el usuario no exista.
* Cuando la directiva de acceso se ha agregado mediante PowerShell y para el identificador de objeto de la aplicación en lugar de para la entidad de servicio.

### <a name="how-can-i-assign-access-control-per-key-vault-object"></a>¿Cómo se puede asignar el control de acceso para cada objeto de almacén de claves? 

El modelo de permisos de RBAC de Key Vault permite permisos por cada objeto. Los permisos de claves, secretos y certificados individuales solo deben usarse para escenarios concretos:

-   Aplicaciones multicapa que necesitan un control de acceso independiente entre las distintas capas

-   Compartir un secreto individual entre varias aplicaciones


### <a name="how-can-i-provide-key-vault-authenticate-using-access-control-policy"></a>¿Cómo se puede proporcionar la autenticación del almacén de claves mediante la directiva de control de acceso?

La manera más sencilla de autenticar una aplicación basada en la nube en Key Vault es con una identidad administrada. Consulte [Autenticación en Azure Key Vault](authentication.md) para más información.
Si va a crear una aplicación local, al realizar el desarrollo local (o si no puede usar una identidad administrada), puede registrar manualmente una entidad de servicio y proporcionar acceso a su almacén de claves mediante una directiva de control de acceso. Consulte [Asignación de una directiva de control de acceso](assign-access-policy-portal.md).

### <a name="how-can-i-give-the-ad-group-access-to-the-key-vault"></a>¿Cómo se puede conceder acceso al grupo de AD al almacén de claves?

Conceda al grupo de AD permisos en el almacén de claves mediante el comando `az keyvault set-policy` de la CLI de Azure o el cmdlet Set-AzKeyVaultAccessPolicy de Azure PowerShell. Consulte [Asignación de una directiva de acceso: CLI](assign-access-policy-cli.md) y [Asignación de una directiva de acceso: PowerShell](assign-access-policy-powershell.md).

La aplicación también necesita al menos un rol de Administración de identidades y acceso (IAM) asignado al almacén de claves. De lo contrario, no podrá iniciar sesión y se producirá un error por derechos insuficientes para acceder a la suscripción. Grupos de Azure AD con identidades administradas puede requerir hasta ocho horas para actualizar el token y entrar en vigor.

### <a name="how-can-i-redeploy-key-vault-with-arm-template-without-deleting-existing-access-policies"></a>¿Cómo se puede volver a implementar Key Vault con la plantilla ARM sin eliminar las directivas de acceso existentes?

Actualmente, al volver a implementar Key Vault, eliminará todas las directivas de acceso en Key Vault y las reemplazará por la directiva de acceso de la plantilla de ARM. No hay ninguna opción incremental para las directivas de acceso de Key Vault. Para conservar las directivas de acceso en Key Vault, necesita leer las directivas de acceso existentes en Key Vault y rellenar la plantilla de ARM con esas directivas a fin de evitar interrupciones en el acceso.

Otra opción que puede ayudar en este escenario es usar RBAC de Azure y roles como alternativa a las directivas de acceso. Con RBAC de Azure, puede volver a implementar el almacén de claves sin especificar la directiva de nuevo. Para más información acerca de esta solución, vaya [aquí](./rbac-guide.md).

### <a name="recommended-troubleshooting-steps-for-following-error-types"></a>Pasos de solución de problemas recomendados para los tipos de errores

* HTTP 401: Solicitud no autenticada: [Pasos de solución de problemas](rest-error-codes.md#http-401-unauthenticated-request)
* HTTP 403: Permisos insuficientes: [Pasos de solución de problemas](rest-error-codes.md#http-403-insufficient-permissions)
* HTTP 429: Demasiadas solicitudes: [Pasos de solución de problemas](rest-error-codes.md#http-429-too-many-requests)
* Compruebe si tiene permiso de acceso de eliminación en el almacén de claves: Consulte [Asignación de una directiva de acceso: CLI](assign-access-policy-cli.md), [Asignación de una directiva de acceso: PowerShell](assign-access-policy-powershell.md) o [Asignación de una directiva de acceso: Portal](assign-access-policy-portal.md).
* Si tiene problemas con la autenticación en el almacén de claves en el código, use el [SDK de autenticación](https://azure.github.io/azure-sdk/posts/2020-02-25/defaultazurecredentials.html)

### <a name="what-are-the-best-practices-i-should-implement-when-key-vault-is-getting-throttled"></a>¿Cuáles son los procedimientos recomendados que debo implementar cuando se esté limitando el almacén de claves?
Siga los procedimientos que se recomiendan [aquí](overview-throttling.md#how-to-throttle-your-app-in-response-to-service-limits).

## <a name="next-steps"></a>Pasos siguientes

Aprenda a solucionar errores de autenticación de un almacén de claves: [Guía de solución de problemas de Key Vault](rest-error-codes.md).
