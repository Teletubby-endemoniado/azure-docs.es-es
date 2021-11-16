---
title: Registros de inicios de sesión en Azure Active Directory | Microsoft Docs
description: Información general de los registros de inicios de sesión en Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 10/25/2021
ms.author: markvi
ms.reviewer: besiler
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d783fcd44fac0c22b83023e1e50b24c209fed8c
ms.sourcegitcommit: 96deccc7988fca3218378a92b3ab685a5123fb73
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131577199"
---
# <a name="sign-in-logs-in-azure-active-directory"></a>Registros de inicios de sesión en Azure Active Directory

Como administrador de TI, quiere saber cómo funciona el entorno de TI. La información sobre el estado del sistema le permite evaluar si es necesario responder a posibles problemas y cómo hacerlo. 

Para ayudarle a conseguir este objetivo, el portal de Azure Active Directory le proporciona acceso a tres registros de actividad:

- **[Inicios de sesión](concept-sign-ins.md)** : Información sobre los inicios de sesión y cómo los usuarios emplean los recursos.
- **[Auditoría](concept-audit-logs.md)** : información sobre los cambios aplicados al inquilino, como la administración de usuarios y grupos o las actualizaciones aplicadas a los recursos del inquilino.
- **[Aprovisionamiento](concept-provisioning-logs.md)** : actividades realizadas por el servicio de aprovisionamiento, como la creación de un grupo en ServiceNow o un usuario importado de Workday.

En este artículo se ofrece una visión general del reporte de inicios de sesión.


## <a name="what-can-you-do-with-it"></a>¿Para qué sirve?

Puede usar el registro de inicios de sesión para encontrar respuestas a preguntas como:

- ¿Cuál es el patrón de inicio de sesión de un usuario?

- ¿Cuántos usuarios han iniciado sesión en una semana?

- ¿Cuál es el estado de estos inicios de sesión?


## <a name="who-can-access-it"></a>¿Quién puede acceder a ellos?

Siempre puede acceder a su propio historial de inicios de sesión mediante este vínculo: [https://mysignins.microsoft.com](https://mysignins.microsoft.com)

Para acceder al registro de inicios de sesión, debe ser:

- Un administrador global

- Un usuario con alguno de los siguientes roles:
    - Administrador de seguridad

    - Lector de seguridad

    - Lector global

    - Lector de informes



## <a name="what-azure-ad-license-do-you-need"></a>¿Qué licencia de Azure AD necesita?

El informe de actividad de inicio de sesión está disponible en [todas las ediciones de Azure AD](reference-reports-data-retention.md#how-long-does-azure-ad-store-the-data). Si tiene una licencia de Azure Active Directory P1 o P2, también puede acceder al informe de actividad de inicio de sesión mediante Microsoft Graph API.


## <a name="where-can-you-find-it-in-the-azure-portal"></a>¿Dónde puede encontrarlo en Azure Portal?

Azure Portal ofrece varias opciones para acceder al registro. Por ejemplo, en el menú Azure Active Directory, puede abrir el registro en la sección **Supervisión**.  

![Apertura de los registros de inicios de sesión](./media/concept-sign-ins/sign-ins-logs-menu.png)

Además, puede acceder directamente a los registros de inicios de sesión mediante este vínculo: [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns)


## <a name="what-is-the-default-view"></a>¿Cuál es la vista predeterminada?

Un registro de inicios de sesión tiene una vista de lista predeterminada que muestra:

- La fecha de inicio de sesión
- El usuario relacionado
- La aplicación en la que el usuario ha iniciado sesión
- El estado de inicio de sesión
- El estado de la detección de riesgos
- El estado del requisito de la autenticación multifactor (MFA)

![Captura de pantalla que muestra los inicios de Office 365 SharePoint Online.](./media/concept-sign-ins/sign-in-activity.png "Actividad de inicio de sesión")

Puede personalizar la vista de lista, haga clic en **Columnas** en la barra de herramientas.

![Captura de pantalla que muestra la opción Columnas en la página Inicios de sesión.](./media/concept-sign-ins/19.png "Actividad de inicio de sesión")

En el cuadro de diálogo **Columnas** se proporciona acceso a los atributos seleccionables. En un informe de inicio de sesión, no puede tener campos que tengan más de un valor para una solicitud de inicio de sesión determinada como columna. Esto es así, por ejemplo, para los detalles de autenticación, los datos de acceso condicional y la ubicación de red.   

![Captura de pantalla que muestra el cuadro de diálogo Columnas, donde puede seleccionar atributos.](./media/concept-sign-ins/columns.png "Actividad de inicio de sesión")

Seleccione un elemento de la vista de lista para obtener información más detallada.

![Captura de pantalla que muestra una vista de información detallada.](./media/concept-sign-ins/basic-sign-in.png "Actividad de inicio de sesión")



## <a name="sign-in-error-code"></a>Código de error de inicio de sesión

Si se produce un error al iniciar sesión, puede obtener más información sobre el motivo en la sección **Información básica** del elemento de registro relacionado. 

![Código de error de inicio de sesión](./media/concept-all-sign-ins/error-code.png)
 
Aunque el elemento de registro proporciona un motivo del error, hay casos en los que puede obtener más información con la [herramienta de búsqueda de errores de inicio de sesión](https://login.microsoftonline.com/error). Por ejemplo, si está disponible, esta herramienta proporciona los pasos de corrección.  

![Herramienta de búsqueda de códigos de error](./media/concept-all-sign-ins/error-code-lookup-tool.png)



## <a name="filter-sign-in-activities"></a>Filtrado de las actividades de inicio de sesión


Puede filtrar los datos de un registro para restringirlo a un nivel que funcione en su caso:

![Captura de pantalla que muestra la opción Agregar filtros.](./media/concept-sign-ins/04.png "Actividad de inicio de sesión")

**Identificador de solicitud**: el identificador de la solicitud que le interesa.

**Usuario**: el nombre o el nombre principal de usuario (UPN) del usuario que le interesa.

**Aplicación**: el nombre de la aplicación de destino.
 
**Estado**: el estado de inicio de sesión que le interesa:

- Correcto

- Error

- Interrumpido


**Dirección IP**: la dirección IP del dispositivo que se utiliza para conectarse al inquilino.

**Ubicación**: la ubicación desde la que se inició la conexión:

- City

- Estado/provincia

- País/región


**Recurso**: el nombre del servicio que se usa para el inicio de sesión.


**Identificador de recurso**: el identificador del servicio que se utiliza para el inicio de sesión.


**Aplicación cliente**: el tipo de aplicación cliente que se usa para conectarse al inquilino:

![Filtro de aplicación cliente](./media/concept-sign-ins/client-app-filter.png)


> [!NOTE]
> Debido a compromisos de privacidad, Azure AD rellena este campo en el inquilino principal en el caso de un escenario entre inquilinos.


|Nombre|Autenticación moderna|Descripción|
|---|:-:|---|
|SMTP autenticado| |Utilizado por clientes POP e IMAP para enviar mensajes de correo electrónico.|
|Detección automática| |Usada por clientes Outlook y EAS para buscar y conectarse a buzones en Exchange Online.|
|Exchange ActiveSync| |Este filtro muestra todos los intentos de inicio de sesión en los que se ha intentado el protocolo EAS.|
|Browser|![Marca de verificación azul.](./media/concept-sign-ins/check.png)|Muestra todos los intentos de inicio de sesión de los usuarios mediante exploradores web.|
|Exchange ActiveSync| | Muestra todos los intentos de inicio de sesión de los usuarios con aplicaciones cliente que usan Exchange ActiveSync para conectarse a Exchange Online.|
|PowerShell de Exchange Online| |Se usa para conectarse a Exchange Online con PowerShell remoto. Si bloquea la autenticación básica para PowerShell de Exchange Online, debe usar el módulo de PowerShell de Exchange Online para conectarse. Para obtener instrucciones, consulte [Conexión a Exchange Online PowerShell con autenticación multifactor](/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/mfa-connect-to-exchange-online-powershell).|
|Servicios web de Exchange| |Una interfaz de programación que se usa en Outlook, Outlook para Mac y aplicaciones de terceros.|
|IMAP4| |Un cliente de correo heredado que utiliza IMAP para recuperar el correo electrónico.|
|MAPI sobre HTTP| |Utilizado por Outlook 2010 y versiones posteriores.|
|Aplicaciones móviles y aplicaciones de escritorio|![Marca de verificación azul.](./media/concept-sign-ins/check.png)|Muestra todos los intentos de inicio de sesión de los usuarios que usan aplicaciones móviles y clientes de escritorio.|
|Libreta de direcciones sin conexión| |Una copia de las colecciones de listas de direcciones que Outlook descarga y usa.|
|Outlook en cualquier lugar (RPC sobre HTTP)| |Utilizado por Outlook 2016 y versiones anteriores.|
|Servicio Outlook| |Usado por la aplicación de correo electrónico y calendario de Windows 10.|
|POP3| |Un cliente de correo heredado que utiliza POP3 para recuperar el correo electrónico.|
|Servicios web de creación de informes| |Se usan para recuperar datos de informes en Exchange Online.|
|Otros clientes| |Muestra todos los intentos de inicio de sesión de los usuarios en los que la aplicación cliente no está incluida o es desconocida.|







**Sistema operativo**: el sistema operativo que se ejecuta en el dispositivo utilizado para iniciar sesión en el inquilino. 


**Explorador de dispositivos**: si la conexión se inició desde un explorador, este campo le permite filtrar por nombre de explorador.


**Id. de correlación**: el identificador de correlación de la actividad.




**Acceso condicional**: el estado de las reglas de acceso condicional aplicadas

- **No aplicado**: No se aplica ninguna directiva al usuario y a la aplicación durante el inicio de sesión.

- **Correcto**: Una o varias directivas de acceso condicional aplicadas al usuario y a la aplicación (pero no necesariamente a las demás condiciones) durante el inicio de sesión. 

- **Error**: El inicio de sesión cumplió la condición de usuario y aplicación de al menos una directiva de acceso condicional, y los controles de concesión no se han cumplido o se han establecido para bloquear el acceso.









## <a name="download-sign-in-activities"></a>Descarga de actividades de inicio de sesión

Haga clic en **Descargar** para crear un archivo CSV o JSON de los 250 000 registros más recientes. Empiece por [descargar los datos de inicios de sesión](./howto-download-logs.md) si quiere trabajar con ellos fuera de Azure Portal.  

![Descargar](./media/concept-sign-ins/71.png "Descargar")

> [!IMPORTANT]
> El número de registros que se puede descargar también está restringido por las [directivas de retención de informes de Azure Active Directory](reference-reports-data-retention.md).  


## <a name="sign-ins-data-shortcuts"></a>Accesos directos a los datos de inicios de sesión

Tanto Azure AD como Azure Portal proporcionan puntos de entrada adicionales para los datos de inicios de sesión:

- Información general sobre la protección de la seguridad de la identidad
- Usuarios
- Grupos
- Aplicaciones empresariales

### <a name="users-sign-ins-data-in-identity-security-protection"></a>Datos de inicio de sesión de usuarios en la protección de la identidad de la seguridad

El gráfico de inicio de sesión de usuario en la página de información general de la **protección de la seguridad de identidad** muestra agregaciones semanales de inicios de sesión. El valor predeterminado para el período es 30 días.

![Captura de pantalla que muestra un gráfico de los inicios de sesión de un mes.](./media/concept-sign-ins/06.png "Actividad de inicio de sesión")

Al hacer clic en un día del gráfico de inicio de sesión, obtiene una vista general de las actividades de inicio de sesión de ese día.

Cada fila de la lista de actividades de inicio de sesión muestra:

* ¿Quién ha iniciado sesión?
* ¿Qué aplicación era el destino del inicio de sesión?
* ¿Cuál es el estado del inicio de sesión?
* ¿Cuál es el estado de MFA del inicio de sesión?

Si hace clic en un elemento, obtendrá más detalles sobre la operación de inicio de sesión:

- Id. de usuario
- Usuario
- Nombre de usuario
- Identificador de aplicación
- Application
- Remoto
- Location
- Dirección IP
- Date
- Se requiere MFA
- Estado de inicio de sesión

> [!NOTE]
> Las direcciones IP se emiten de forma que no haya ninguna conexión definitiva entre una dirección IP y donde se encuentre físicamente el equipo con esa dirección. La asignación de direcciones IP se complica por el hecho de que los proveedores de dispositivos móviles y las VPN emiten direcciones IP desde grupos centrales que, a menudo, están muy lejos de donde el dispositivo cliente se usa realmente. Actualmente, la conversión de la dirección IP en una ubicación física es un esfuerzo notable basado en seguimientos, datos de registro, búsquedas inversas y otra información.

En la página **Usuarios**, puede obtener una vista general completa de todos los inicios de sesión del usuario haciendo clic en **Inicios de sesión** en la sección **Actividad**.

![Captura de pantalla que muestra la sección Actividad, donde puede seleccionar Inicios de sesión.](./media/concept-sign-ins/08.png "Actividad de inicio de sesión")

## <a name="authentication-details"></a>Detalles de la autenticación

La pestaña **Detalles de autenticación** que se encuentra en el informe de inicios de sesión proporciona la siguiente información para cada intento de autenticación:

- Una lista de las directivas de autenticación aplicadas (por ejemplo, acceso condicional, MFA por usuario, valores predeterminados de seguridad).
- Una lista de las directivas de duración de sesión aplicadas (por ejemplo, frecuencia de inicio de sesión, Recordar MFA, Vigencia del token configurable)
- La secuencia de los métodos de autenticación usados para iniciar sesión.
- Si el intento de autenticación se ha realizado correctamente o no.
- Detalles sobre por qué el intento de autenticación se ha realizado correctamente o no.

Esta información permite a los administradores solucionar problemas de cada paso en el inicio de sesión de un usuario y realizar un seguimiento de los siguientes aspectos:

- Volumen de inicios de sesión protegidos por la autenticación multifactor. 
- Motivo de la solicitud de autenticación basada en las directivas de duración de la sesión
- Tasas de uso y éxito de cada método de autenticación. 
- Uso de métodos de autenticación sin contraseña (como el inicio de sesión con teléfono sin contraseña, FIDO2 y Windows Hello para empresas). 
- Con qué frecuencia se cumplen los requisitos de autenticación mediante notificaciones de token (donde a los usuarios no se les pide interactivamente que escriban una contraseña, escriban un SMS OTP, y así sucesivamente).

Al ver el informe de inicios de sesión, seleccione la pestaña **Detalles de autenticación**: 

![Captura de pantalla de la pestaña Detalles de autenticación](media/concept-sign-ins/auth-details-tab.png)

>[!NOTE]
>El **código de verificación OATH** se registra como el método de autenticación para tokens de hardware y software OATH (por ejemplo, la aplicación Microsoft Authenticator).

>[!IMPORTANT]
>La pestaña **Detalles de autenticación** puede mostrar inicialmente datos incompletos o inexactos, hasta que la información del registro se agregue por completo. Algunos ejemplos conocidos son: 
>- Un mensaje de **notificación de cumplimiento en el token** se muestra incorrectamente cuando se registran inicialmente eventos de inicio de sesión. 
>- La fila **autenticación principal** no se registra inicialmente. 


## <a name="usage-of-managed-applications"></a>Uso de las aplicaciones administradas

Con una vista centrada en la aplicación de los datos de inicio de sesión, puede responder a preguntas tales como:

* ¿Quién está usando mis aplicaciones?
* ¿Cuáles son las tres aplicaciones principales en su organización?
* ¿Cómo es el rendimiento de la aplicación más reciente?

El punto de entrada a estos datos son las tres aplicaciones principales de su organización. Los datos se incluyen en el informe de los últimos 30 días en la sección **Información general** en **Aplicaciones empresariales**.

![Captura de pantalla que muestra dónde puede seleccionar Información general.](./media/concept-sign-ins/10.png "Actividad de inicio de sesión")

Agregaciones semanales de los gráficos de uso de la aplicación de inicios de sesión para las tres aplicaciones principales en un período determinado. El valor predeterminado para el período es 30 días.

![Captura de pantalla que muestra el uso de la aplicación durante un período de un mes.](./media/concept-sign-ins/graph-chart.png "Actividad de inicio de sesión")

Si lo desea, puede establecer el foco en una aplicación específica.

![Reporting](./media/concept-sign-ins/single-app-usage-graph.png "Notificación")

Al hacer clic en un día del gráfico de uso de la aplicación, obtendrá una lista detallada de las actividades de inicio de sesión.

La opción **Inicios de sesión** ofrece una descripción completa de todos los eventos de inicio de sesión para sus aplicaciones.

## <a name="microsoft-365-activity-logs"></a>Registros de actividad de Microsoft 365

Puede ver los registros de actividad de Microsoft 365 desde el [Centro de administración de Microsoft 365](/office365/admin/admin-overview/about-the-admin-center). Tenga en cuenta que la actividad de Microsoft 365 y los registros de actividad de Azure AD comparten una cantidad significativa de los recursos del directorio. Solo el centro de administración de Microsoft 365 proporciona una vista completa de los registros de actividad de Microsoft 365. 

También puede tener acceso a los registros de actividad de Microsoft 365 mediante programación con las [API de administración de Office 365](/office/office-365-management-api/office-365-management-apis-overview).

## <a name="next-steps"></a>Pasos siguientes

* [Directivas de retención de datos de Azure AD](reference-reports-data-retention.md)
* [Latencias de informes de Azure AD](reference-reports-latencies.md)
* [Aplicaciones de Microsoft en el informe de inicios de sesión](/troubleshoot/azure/active-directory/verify-first-party-apps-sign-in#application-ids-for-commonly-used-microsoft-applications)
