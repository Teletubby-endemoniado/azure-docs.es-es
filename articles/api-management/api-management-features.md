---
title: Comparación de características de los planes de tarifa de Azure API Management | Microsoft Docs
description: En este artículo se comparan los planes de tarifa de API Management según las características que ofrecen. Verá una tabla donde se resumen las características principales disponibles en cada plan de tarifa.
services: api-management
documentationcenter: ''
author: dlepow
ms.service: api-management
ms.topic: article
ms.date: 11/15/2021
ms.author: danlep
ms.openlocfilehash: 6159b5c5d677dd66dda12573feeb3aad9faa9764
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132556541"
---
# <a name="feature-based-comparison-of-the-azure-api-management-tiers"></a>Comparación de características de los planes de tarifa de Azure API Management

Cada [plan de tarifa](https://aka.ms/apimpricing) de API Management ofrece un conjunto diferente de características y [capacidad](api-management-capacity.md) por unidad. En la tabla siguiente se resumen las características clave disponibles en cada uno de los planes. Algunas características podrían funcionar de manera diferente o tener funcionalidades diferentes según el plan. En tales casos, se indican las diferencias en los artículos de documentación que describen estas características individuales.

> [!IMPORTANT]
> Tenga en cuenta que el nivel Desarrollador no es para evaluaciones ni casos de uso que no son de producción. No proporciona Acuerdos de Nivel de Servicio.

| Característica                                                                                      | Consumo | Desarrollador | Básico | Estándar | Premium |
| -------------------------------------------------------------------------------------------- | ----------- | --------- | ----- | -------- | ------- |
| Integración de Azure AD<sup>1</sup>                                                             | No          | Sí       | No    | Sí      | Sí     |
| Compatibilidad con redes virtuales (VNet)                                                               | No          | Sí       | No    | No       | Sí     |
| Implementación en varias regiones                                                                      | No          | No        | No    | No       | Sí     |
| Zonas de disponibilidad                                                                           | No          | No        | No    | No       | Sí     |
| Múltiples nombres de dominio personalizados                                                                 | No          | Sí        | No    | No       | Sí     |
| Portal para desarrolladores<sup>2</sup>                                                                 | No          | Sí       | Sí   | Sí      | Sí     |
| Memoria caché integrada                                                                               | No          | Sí       | Sí   | Sí      | Sí     |
| Análisis integrado                                                                           | No          | Sí       | Sí   | Sí      | Sí     |
| [Puerta de enlace autohospedada](self-hosted-gateway-overview.md)<sup>3</sup>                           | No          | Sí       | No    | No       | Sí     |
| [Configuración de TLS](api-management-howto-manage-protocols-ciphers.md)                             | Sí         | Sí       | Sí   | Sí      | Sí     |
| [Caché externa](./api-management-howto-cache-external.md)                                                    | Sí         | Sí       | Sí   | Sí      | Sí     |
| [Autenticación de certificados de clientes](api-management-howto-mutual-certificates-for-clients.md) | Sí         | Sí       | Sí   | Sí      | Sí     |
| [Copia de seguridad y restauración](api-management-howto-disaster-recovery-backup-restore.md)               | No          | Sí       | Sí   | Sí      | Sí     |
| [Administración a través de Git](api-management-configuration-repository-git.md)                        | No          | Sí       | Sí   | Sí      | Sí     |
| API de administración directa                                                                        | No          | Sí       | Sí   | Sí      | Sí     |
| Métricas y registros de Azure Monitor                                                               | No          | Sí       | Sí   | Sí      | Sí     |
| IP estática                                                                                    | No          | Sí       | Sí   | Sí      | Sí     |
| [API de WebSocket](websocket-api.md)                                                                                    | No          | Sí       | Sí   | Sí      | Sí     |
| [API de GraphQL (versión preliminar)](graphql-api.md)                                                                               | Sí          | Sí       | Sí   | Sí      | Sí     |

<sup>1</sup> Habilita el uso de Azure AD (y Azure AD B2C) como proveedor de identidades para el inicio de sesión del usuario en el portal para desarrolladores.<br/>
<sup>2</sup> Incluye funciones relacionadas, por ejemplo, plantillas y notificaciones de usuarios, grupos, problemas, aplicaciones y correo electrónico.<br/>
<sup>3</sup> En el nivel de desarrollador, las puertas de enlace autohospedadas están limitadas a un solo nodo de puerta de enlace.<br/>
