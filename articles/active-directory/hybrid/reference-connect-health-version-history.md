---
title: Historial de versiones de Azure AD Connect Health
description: Este documento describe las versiones de Azure AD Connect Health y lo que se ha incluido en dichas versiones.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: daveba
editor: curtand
ms.assetid: 8dd4e998-747b-4c52-b8d3-3900fe77d88f
ms.service: active-directory
ms.subservice: hybrid
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 08/10/2020
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: dc0495e4ddd3e266b375a9d6a137497f457803da
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131049261"
---
# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect Health: Historial de lanzamiento de versiones
El equipo de Azure Active Directory actualiza periódicamente Azure AD Connect Health con nuevas características y funciones. En este artículo se enumeran las versiones y características que se han publicado.  

> [!NOTE]
> Los agentes de Connect Health se actualizan de forma automática cuando se publica una nueva versión. Asegúrese de que la configuración de actualización automática está habilitada en Azure Portal.
>

Azure AD Connect Health para Sync se integra con la instalación de Azure AD Connect. Más información sobre el [historial de versiones de Azure AD Connect](./reference-connect-version-history.md). Para enviar comentarios sobre las características, vote en el [canal de UserVoice para Connect Health](https://feedback.azure.com/d365community/forum/22920db1-ad25-ec11-b6e6-000d3a4f0789)

## <a name="september-2021"></a>Septiembre de 2021
**Actualización del agente**
- Agente de Azure AD Connect Health para AD FS (versión 3.1.113.0)
  - Corrección para extraer información del dispositivo, como el cumplimiento del dispositivo y el estado administrado, el sistema operativo del dispositivo y la versión del sistema operativo del dispositivo a partir de auditorías de AD FS en determinados escenarios de autenticación basada en dispositivos.
  - Corrección para rellenar la información de la aplicación de OAuth en casos de error y categorizar los errores de OAuth con códigos de error más específicos.
  - Corrección de alertas en llamadas WMI interrumpidas en el equipo cliente. Ahora, estas llamadas al resultado o el estado se establecerían en "notRun".

## <a name="may-2021"></a>Mayo de 2021
**Actualización del agente**
- Agente de Azure AD Connect Health para AD FS (versión 3.1.99.0)
  - Corrección para un valor de recuento de usuarios único bajo del informe de actividad de la aplicación AD FS
  - Corrección de los inicios de sesión con CorrelationId de GUID vacíos o predeterminados

## <a name="march-2021"></a>Marzo de 2021
**Actualización del agente**

- Agente de Azure AD Connect Health para AD FS (versión 3.1.95.0)

  - Corrección para resolver el nombre de usuario con formato NT4 en un UPN durante los eventos de inicio de sesión.
  - Corrección para identificar escenarios incorrectos de identificador de la aplicación con un código de error dedicado.
  - Cambios para agregar una nueva propiedad para el identificador de cliente de OAuth.
  - Corrección para mostrar los valores correctos en los campos **Protocolo** y **Tipo de autenticación** del informe de inicio de sesión de Azure AD para determinados escenarios de inicio de sesión.
  - Corrección para mostrar en orden las direcciones IP en el campo de la cadena de IP del informe de inicio de sesión de Azure AD de la solicitud.
  - Cambios para introducir un nuevo campo para diferenciar si se solicitó la autenticación secundaria durante un inicio de sesión.
  - Corrección de la propiedad de identificador de la aplicación de AD FS que se va a mostrar en el informe de inicio de sesión de Azure AD.

## <a name="april-2020"></a>Abril de 2020
**Actualización del agente**

- Agente de Azure AD Connect Health para AD FS (versión 3.1.77.0)

   - Corrección de errores para la alerta "El nombre de entidad de seguridad de servicio (SPN) no es válido para la cuenta de servicio de AD FS", para el que la alerta informaba incorrectamente.


## <a name="july-2019"></a>Julio de 2019
**Actualización del agente**
* Agente de Azure AD Connect Health para AD FS (versión 3.1.59.0) 
   1. Cambio de texto en TestWindowsTransport
   2. Cambios en la carga de RP de AD FS
   
* Agente de Azure AD Connect Health para AD FS (versión 3.1.56.0) 
   1. Adición de prueba TestWindowsTransport y eliminación de las comprobaciones de puntos de conexión de WsTrust en la prueba CheckOffice365Endpoints
   2. Registro de información de .NET y del sistema operativo
   3. Aumento del tamaño de carga de mensajes de configuración de RP a 1 MB
   4. Corrección de errores
   
* Agente de Azure AD Connect Health para AD DS (versión 3.1.56.0) 
   1. Registro de información de .NET y del sistema operativo 
   2. Corrección de errores

## <a name="may-2019"></a>Mayo de 2019
**Actualización del agente:** 
* Agente de Azure AD Connect Health para AD FS (versión 3.1.51.0) 
   1. Corrección de errores para distinguir entre varios inicios de sesión que comparten el mismo identificador de solicitud de cliente.
   2. Corrección de errores para analizar los errores de nombre de usuario o contraseña incorrectos en los servidores traducidos.   

## <a name="april-2019"></a>Abril de 2019
**Actualización del agente:** 
* Agente de Azure AD Connect Health para AD FS (versión 3.1.46.0) 
   1. Corrección del proceso de alerta de SPN de duplicación de comprobación para ADFS

## <a name="march-2019"></a>Marzo de 2019
**Actualización del agente:** 
* Agente de Azure AD Connect Health para AD DS (versión 3.1.41.0)  
   1. Colección de la versión de .NET
   2. Mejora de la recopilación de contadores de rendimiento cuando faltan determinadas categorías
   3. Corrección de errores en la prevención de creación de varias instancias del agente de supervisión

* Agente de Azure AD Connect Health para AD FS (versión 3.1.41.0) 
   1. Integración y actualización de scripts de prueba de AD FS mediante ADFSToolBox
   2. Implementación de la colección de la versión de .NET
   3. Mejora de la recopilación de contadores de rendimiento cuando faltan determinadas categorías
   4. Corrección de errores en la prevención de creación de varias instancias del agente de supervisión


## <a name="november-2018"></a>Noviembre de 2018
**Nuevas características de disponibilidad general:** 
* Agente de Azure AD Connect Health para sincronización: diagnosticar y corregir errores de sincronización de atributo duplicado desde el portal

**Actualización del agente:** 
* Agente de Azure AD Connect Health para AD DS (versión 3.1.24.0) 
   1. Cumplimiento del protocolo de seguridad de la capa de transporte (TLS) versión 1.2
   2. Reducción del ruido de alertas en el catálogo global
   3. Correcciones de errores de registro del agente de Health

* Agente de Azure AD Connect Health para AD FS (versión 3.1.24.0)  
   1. Cumplimiento del protocolo de seguridad de la capa de transporte (TLS) versión 1.2
   2. Compatibilidad de Test-ADFSRequestToken para el sistema operativo localizado
   3. Resolución del problema de bloqueo de EventHandler en el agente de diagnóstico
   4. Correcciones de errores de registro del agente de Health

## <a name="august-2018"></a>Agosto de 2018 
*  Agente de Azure AD Connect Health para Sync (versión 3.1.7.0), publicado con Azure AD Connect versión 1.1.880.0    
   1. Revisión para el [problema de uso alto de CPU del agente de supervisión con las versiones de KB de .NET Framework](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)

## <a name="june-2018"></a>Junio de 2018 
**Nuevas características de la versión preliminar:** 
* Agente de Azure AD Connect Health para sincronización: diagnosticar y corregir errores de sincronización de atributo duplicado desde el portal 

**Actualización del agente:** 
* Agente de Azure AD Connect Health para AD DS (versión 3.1.7.0)    
  1. Revisión para el [problema de uso alto de CPU del agente de supervisión con las versiones de KB de .NET Framework](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)
   
* Agente de Azure AD Connect Health para AD FS (versión 3.1.7.0)  
  1. Revisión para el [problema de uso alto de CPU del agente de supervisión con las versiones de KB de .NET Framework](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)
  2. Correcciones de los resultados de pruebas en el servidor secundario de AD FS Server 2016
   
* Agente de Azure AD Connect Health para AD FS (versión 3.1.2.0)  
  1. Revisión para la administración de memoria de agente y alertas relacionadas específicamente para la versión 3.0.244.0


## <a name="may-2018"></a>Mayo de 2018
**Actualización del agente:**
* Agente de Azure AD Connect Health para AD DS (versión 3.0.244.0)
  1. Mejora de la privacidad de agente  
  2. Correcciones de errores y mejoras generales

* Agente de Azure AD Connect Health para AD FS (versión 3.0.244.0)
  1. Servicio de diagnóstico de agente y mejoras del módulo de PowerShell relacionadas
  2. Mejora de la privacidad de agente  
  3. Correcciones de errores y mejoras generales

* Agente de Azure AD Connect Health para sincronización (versión 3.0.164.0), publicado con Azure AD Connect versión 1.1.819.0 
  1. Mejora de la privacidad de agente  
  2. Correcciones de errores y mejoras generales


## <a name="march-2018"></a>Marzo de 2018
**Nuevas características de la versión preliminar:**
* Azure AD Connect Health para AD FS: informe de IP de riesgo y de alerta.

**Actualización del agente:**

* Agente de Azure AD Connect Health para AD DS (versión 3.0.176.0)
  1. Mejoras de disponibilidad de agente 
  2. Correcciones de errores y mejoras generales
* Agente de Azure AD Connect Health para AD FS (versión 3.0.176.0)
  1. Mejoras de disponibilidad de agente 
  2. Correcciones de errores y mejoras generales
* Agente de Azure AD Connect Health para Sync (versión 3.0.129.0), publicado con Azure AD Connect versión 1.1.750.0  
  1. Mejoras de disponibilidad de agente 
  2. Correcciones de errores y mejoras generales

## <a name="december-2017"></a>Diciembre de 2017
**Actualización del agente:**

* Agente de Azure AD Connect Health para AD DS (versión 3.0.145.0)
  1. Mejoras de disponibilidad de agente 
  2. Se han agregado nuevos comandos para solucionar problemas de agente
  3. Correcciones de errores y mejoras generales
* Agente de Azure AD Connect Health para AD FS (versión 3.0.145.0)
  1. Se han agregado nuevos comandos para solucionar problemas de agente
  2. Mejoras de disponibilidad de agente 
  3. Correcciones de errores y mejoras generales
  
## <a name="october-2017"></a>Octubre de 2017
**Actualización del agente:**

 * Agente de Azure AD Connect Health para sincronización (versión 3.0.129.0), publicado con Azure AD Connect versión 1.1.649.0
<br></br> Se ha solucionado un problema de compatibilidad de versiones entre Azure AD Connect y el agente Azure AD Connect Health para sincronización. Este problema afecta a los clientes que realizan la actualización local de Azure AD Connect a la versión 1.1.647.0, pero actualmente tienen la versión del agente de Health 3.0.127.0. Después de la actualización, el agente de Health ya no puede enviar datos de estado sobre el servicio de sincronización de Azure AD Connect al servicio Azure AD Health. Con esta corrección, la versión 3.0.129.0 del agente de mantenimiento se instala durante la actualización local de Azure AD Connect. La versión 3.0.129.0 del agente de Health no tiene el problema de compatibilidad con la versión 1.1.649.0 de Azure AD Connect.

## <a name="july-2017"></a>Julio de 2017
**Actualización del agente:**

* Agente de Azure AD Connect Health para AD DS (versión 3.0.68.0)
  1. Correcciones de errores y mejoras generales
  2. Compatibilidad con nubes independientes
* Agente de Azure AD Connect Health para AD FS (versión 3.0.68.0)
  1. Correcciones de errores y mejoras generales
  2. Compatibilidad con nubes independientes
* Agente de Azure AD Connect Health para sincronización (versión 3.0.68.0), publicado con Azure AD Connect versión 1.1.614.0
  1. Se ha agregado compatibilidad para Microsoft Azure Government Cloud y Microsoft Cloud Germany

## <a name="april-2017"></a>Abril de 2017      
**Actualización del agente:**

* Agente de Azure AD Connect Health para AD FS (versión 3.0.12.0)
  1. Correcciones de errores y mejoras generales
* Agente de Azure AD Connect Health para AD DS (versión 3.0.12.0)
  1. Mejoras de carga de contadores de rendimiento
  2. Correcciones de errores y mejoras generales

## <a name="october-2016"></a>Octubre de 2016
**Actualización del agente:**

* Agente de Azure AD Connect Health para AD FS (versión 2.6.408.0)
* Mejoras en la detección de direcciones IP de cliente en las solicitudes de autenticación
* Correcciones de errores relacionados con las alertas
* Agente de Azure AD Connect Health para AD DS (versión 2.6.408.0)
* Correcciones de errores relacionados con las alertas
* Agente de Azure AD Connect Health para sincronización (versión 2.6.353.0), publicado con Azure AD Connect versión 1.1.281.0
* Proporcionar los datos necesarios para los informes de errores de sincronización
* Correcciones de errores relacionados con las alertas

**Nuevas características de la versión preliminar:**

* Informes de errores de sincronización de Azure AD Connect

**Nuevas características:**

* Azure AD Connect Health para AD FS: campo de dirección IP está disponible en el informe de los 50 usuarios principales con el nombre de usuario o contraseña incorrectos.

## <a name="july-2016"></a>Julio de 2016
**Nuevas características de la versión preliminar:**

* [Azure AD Connect Health para AD DS](how-to-connect-health-adds.md).

## <a name="january-2016"></a>Enero de 2016
**Actualización del agente:**

* Agente de Azure AD Connect Health para AD FS (versión 2.6.91.1512)

**Nuevas características:**

* [Herramienta de conexión de prueba para agentes de Azure AD Connect Health](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)

## <a name="november-2015"></a>Noviembre de 2015
**Nuevas características:**

* Compatibilidad con [control de acceso basado en roles de Azure (Azure RBAC)](how-to-connect-health-operations.md#manage-access-with-azure-rbac)

**Nuevas características de la versión preliminar:**

* [Azure AD Connect Health para sincronización](how-to-connect-health-sync.md)

**Problemas corregidos:**

* Correcciones de errores detectados durante los registros del agente.

## <a name="september-2015"></a>Septiembre de 2015
**Nuevas características:**

* Informe de contraseña de nombre de usuario incorrecto para AD FS
* Soporte técnico para configurar el proxy de HTTP sin autenticar
* Soporte técnico para configurar el agente en Server Core
* Mejoras en las alertas de AD FS
* Mejoras en el agente de Azure AD Connect Health para AD FS en la conectividad y carga de datos.

**Problemas corregidos:**

* Correcciones de errores en el uso de tipos de errores de AD FS.

## <a name="june-2015"></a>Junio de 2015
**Versión inicial de Azure AD Connect Health para AD FS y el proxy de AD FS.**

**Nuevas características:**

* Alertas de supervisión de AD FS y servidores proxy de AD FS con notificaciones de correo electrónico.
* Fácil acceso a la topología y patrones de AD FS en los contadores de rendimiento de AD FS.
* Tendencia de las solicitudes correctas de token en servidores de AD FS agrupados por aplicaciones, métodos de autenticación, ubicación de la red de solicitud, etc.
* Tendencias en las solicitudes con error en los servidores de AD FS agrupados por aplicaciones, tipos de error, etc.
* Implementación más sencilla del agente mediante credenciales de administrador global de Azure AD.  

## <a name="next-steps"></a>Pasos siguientes
Conozca más detalles acerca de la [Supervisión de la infraestructura de identidad local y los servicios de sincronización en la nube](./whatis-azure-ad-connect.md).
