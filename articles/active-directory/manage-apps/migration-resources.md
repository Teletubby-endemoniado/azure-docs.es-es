---
title: Recursos para migrar aplicaciones a Azure Active Directory | Microsoft Docs
description: Recursos para ayudarle a migrar el acceso a la aplicación y la autenticación a Azure Active Directory (Azure AD).
services: active-directory
author: davidmu1
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.workload: identity
ms.date: 02/29/2020
ms.author: davidmu
ms.reviewer: alamaral
ms.openlocfilehash: 05c7b2f668565fe4ab37ed01ad65bef7cb6d95d6
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2021
ms.locfileid: "123437929"
---
# <a name="resources-for-migrating-applications-to-azure-active-directory"></a>Recursos para migrar aplicaciones a Azure Active Directory

Recursos para ayudarle a migrar el acceso a la aplicación y la autenticación a Azure Active Directory (Azure AD).

| Resource  | Descripción  |
|:-----------|:-------------|
|[Migrar sus aplicaciones a Azure AD](https://aka.ms/migrateapps/whitepaper) | En este artículo se presentan las ventajas de la migración y se describe cómo planear la migración en cuatro fases bien definidas: detección, clasificación, migración y administración continua. Se le guiará sobre cómo debe pensar en el proceso y desglosar el proyecto en partes fáciles de consumir. Este documento incluye vínculos a recursos importantes que le ayudarán a lo largo de este proceso. |
|[Guía de la solución: migración de aplicaciones de Servicios de federación de Active Directory (AD FS) a Azure AD](./migrate-adfs-apps-to-azure.md) | Esta guía de la solución le guiará a través de la mismas cuatro fases de planeamiento y ejecución de un proyecto de migración de aplicaciones que se describen en un nivel más alto en las notas del producto de migración. Con esta guía, aprenderá a aplicar estas fases al objetivo concreto de mover una aplicación de Servicios de federación de Active Directory (AD FS) a Azure AD.|
|[Tutorial para desarrolladores: Guía de migración de aplicaciones de AD FS a Azure AD para desarrolladores](https://aka.ms/adfsplaybook) | Este conjunto de ejemplos de código y tutoriales complementarios de ASP.NET le ayudará a aprender a migrar con seguridad las aplicaciones integradas con Servicios de federación de Active Directory (AD FS) a Azure Active Directory (Azure AD). Este tutorial se centra en los desarrolladores que no solo necesitan obtener información sobre la configuración de aplicaciones en AD FS y Azure AD, sino que también son conscientes y están seguros de los cambios que su base de código necesitará en este proceso.|
| [Herramienta: script de preparación para la migración de Servicios de federación de Active Directory](https://aka.ms/migrateapps/adfstools) | Se trata de un script que puede ejecutar en el servidor local de Servicios de federación de Active Directory (AD FS) para determinar la preparación de las aplicaciones para la migración a Azure AD.|
| [Plan de implementación: migración de AD FS a la sincronización de hash de contraseña](https://aka.ms/ADFSTOPHSDPDownload) | Con la sincronización de hash de contraseña, se sincronizan los valores hash de las contraseñas de los usuarios de Active Directory local con Azure AD. Esto permite a Azure AD autenticar a los usuarios sin interactuar con la instancia local de Active Directory.|
| [Plan de implementación: migración de AD FS a autenticación de paso a través](https://aka.ms/ADFSTOPTADPDownload)|La autenticación de paso a través de Azure AD ayuda a los usuarios a iniciar sesión en aplicaciones locales y en la nube con la misma contraseña. Esta característica proporciona a los usuarios una mejor experiencia, puesto que tienen una contraseña menos que recordar. También reduce los costos del departamento de soporte técnico, ya que es menos probable que olviden cómo iniciar sesión si solo necesitan recordar una contraseña. Cuando los usuarios inician sesión con Azure AD, esta característica valida sus contraseñas directamente con la instancia de Active Directory local.|
| [Plan de implementación: habilitación del inicio de sesión único en una aplicación SaaS con Azure AD](https://aka.ms/SSODPDownload) | El inicio de sesión único (SSO) le ayuda a acceder a todas las aplicaciones y los recursos que necesita para hacer negocios, iniciando sesión una sola vez con una única cuenta de usuario. Por ejemplo, después de iniciar la sesión, el usuario puede cambiar entre Microsoft Office, Salesforce o Box sin necesidad de volver a autenticarse (por ejemplo, mediante una contraseña).
| [Plan de implementación: ampliación de las aplicaciones a Azure AD con el proxy de aplicación](https://aka.ms/AppProxyDPDownload)| Para proporcionar acceso a los portátiles de los empleados y otros dispositivos a las aplicaciones locales, siempre se han usado redes privadas virtuales (VPN) o redes perimetrales (DMZ). No obstante, estas soluciones no solo son complejas y difíciles de proteger, sino también costosas de configurar y administrar. Azure AD Application Proxy facilita el acceso a las aplicaciones locales. |
| [Planes de implementación](../fundamentals/active-directory-deployment-plans.md) | Encuentre más planes de implementación para implementar características como la autenticación multifactor, el acceso condicional, el aprovisionamiento de usuarios, SSO de conexión directa, el autoservicio de restablecimiento de contraseña y mucho más. |
| [Migración de aplicaciones de Symantec SiteMinder a Azure AD](https://azure.microsoft.com/mediahandler/files/resourcefiles/migrating-applications-from-symantec-siteminder-to-azure-active-directory/Migrating-applications-from-Symantec-SiteMinder-to-Azure-Active-Directory.pdf) | Obtenga instrucciones paso a paso sobre las opciones de integración y migración de aplicaciones con un ejemplo que le guiará a través de la migración de aplicaciones de Symantec SiteMinder a Azure AD. |
| [Migración de aplicaciones de Okta a Azure AD](migrate-applications-from-okta-to-azure-active-directory.md) | Obtenga instrucciones paso a paso sobre la migración de aplicaciones de Okta a Azure AD. |
| [Migración de la federación de Okta a la autenticación administrada de Azure AD](migrate-okta-federation-to-azure-active-directory.md) | Obtenga información sobre cómo federar los inquilinos de Office 365 existentes con Okta para conseguir funcionalidades de inicio de sesión único. |
| [Tutorial: Migración del aprovisionamiento de la sincronización de Okta a la sincronización basada en Azure Active Directory Connect](migrate-okta-sync-provisioning-to-azure-active-directory.md) | Guía paso a paso para las organizaciones que usan actualmente el aprovisionamiento de usuarios de Okta a Azure AD, relativa a la migración de la sincronización de usuarios o la sincronización universal a Azure AD Connect. |
| [Migración de las directivas de inicio de sesión de Okta al acceso condicional de Azure AD](migrate-okta-sign-on-policies-to-azure-active-directory-conditional-access.md) | Obtenga instrucciones paso a paso sobre cómo migrar de las directivas de inicio de sesión globales o de nivel de aplicación en Okta a las directivas de acceso condicional de Azure AD para proteger el acceso de los usuarios en Azure AD y las aplicaciones conectadas. |

