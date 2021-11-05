---
title: Limitaciones de colaboración B2B de Azure Active Directory | Microsoft Docs
description: Limitaciones actuales de la colaboración B2B de Azure Active Directory
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b2df7a9fb6ff0cf56846ba6672f90fb62349e2d
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131049470"
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Limitaciones de colaboración B2B de Azure AD
La colaboración B2B de Active Directory (Azure AD) de Azure está sujeta actualmente a las limitaciones descritas en este artículo.

## <a name="possible-double-multi-factor-authentication"></a>Posibilidad de doble autenticación multifactor
Con B2B de Azure AD, puede aplicar la autenticación multifactor en la organización del recurso (la organización que invita). Los motivos de este enfoque se detallan en [Acceso condicional para usuarios de colaboración B2B](conditional-access.md). Si un asociado ya tiene instalada la autenticación multifactor y la exige, los usuarios del asociado podrían tener que realizar la autenticación una vez en su organización principal y otra vez en la suya.

## <a name="instant-on"></a>Activación instantánea
En los flujos de colaboración B2B, hemos agregado usuarios al directorio y los hemos actualizado dinámicamente durante el canje de invitación, la asignación de aplicaciones y así sucesivamente. Las actualizaciones y escrituras se producen normalmente en una instancia de directorio y se deben replicar en todas las instancias. La replicación se completa una vez que se actualicen todas las instancias. En ocasiones, cuando el objeto se escribe o se actualiza en una instancia y la llamada para recuperar este objeto es a otra instancia, pueden producirse latencias en la replicación. Si esto sucede, actualice o vuelva a intentarlo. Si está escribiendo una aplicación mediante la API, los reintentos con algún retroceso es una buena práctica defensiva para mitigar este problema.

## <a name="azure-ad-directories"></a>Directorios de Azure AD
Azure AD B2B está sujeto a los límites de directorio del servicio Azure AD. Para obtener más información sobre el número de directorios que puede crear un usuario y el número de directorios al que puede pertenecer un usuario o usuario invitado, consulte [Restricciones y límites del servicio Azure AD](../enterprise-users/directory-service-limits-restrictions.md).

## <a name="national-clouds"></a>Nubes nacionales
Las [nubes nacionales](../develop/authentication-national-cloud.md) son instancias físicamente aisladas de Azure. La colaboración B2B no se admite más allá de los límites de la nube nacional. Por ejemplo, si el inquilino de Azure está en la nube global pública, no puede invitar a un usuario cuya cuenta está en una nube nacional. Para colaborar con el usuario, pídale otra dirección de correo electrónico o cree una cuenta de usuario miembro para él en el directorio.

## <a name="azure-us-government-clouds"></a>Nubes de Azure US Government
Dentro de la nube de Azure US Government, la colaboración B2B solo se admite entre inquilinos que se encuentran dentro de la nube de Azure US Government y que admiten colaboración B2B. Los inquilinos de Azure US Government que admiten la colaboración B2B también pueden colaborar con usuarios sociales mediante Microsoft, cuentas de Google o cuentas de correo electrónico con código de acceso de un solo uso. Si invita a un usuario de fuera de estos grupos (por ejemplo, si el usuario es un inquilino que no forma parte de la nube de Azure US Government o que todavía no admite la colaboración B2B), se produce un error en la invitación o el usuario no puede canjear la invitación. En el caso de las cuentas de Microsoft (MSA), existen limitaciones conocidas con respecto al acceso a Azure Portal: los nuevos invitados de MSA no pueden canjear invitaciones de vínculo directo a Azure Portal y los invitados de MSA existentes no pueden iniciar sesión en Azure Portal. Para obtener más información sobre otras limitaciones, consulte las [variaciones P1 y P2 de Azure Active Directory Premium](../../azure-government/compare-azure-government-global-azure.md#azure-active-directory-premium-p1-and-p2).

### <a name="how-can-i-tell-if-b2b-collaboration-is-available-in-my-azure-us-government-tenant"></a>¿Cómo puedo saber si la colaboración B2B está disponible en mi inquilino de Azure US Government?
Para averiguar si su inquilino de la nube de Azure US Government admite la colaboración B2B, haga lo siguiente:

1. En un explorador, vaya a la siguiente dirección URL, y sustituya *&lt;tenantname&gt;* por el nombre de su inquilino:

   `https://login.microsoftonline.com/<tenantname>/v2.0/.well-known/openid-configuration`

2. Busque `"tenant_region_scope"` en la respuesta JSON:

   - Si aparece `"tenant_region_scope":"USGOV”`, se admite B2B.
   - Si aparece `"tenant_region_scope":"USG"`, no se admite B2B.

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos sobre la colaboración de B2B de Azure AD:

- [¿Qué es la colaboración B2B de Azure AD?](what-is-b2b.md)
- [Delegación de las invitaciones de colaboración B2B](delegate-invitations.md)
