---
title: 'Registros de información de inicio de sesión en Azure Active Directory: versión preliminar | Microsoft Docs'
description: Información general de los registros de información de inicio de sesión en Azure Active Directory, lo que incluye características nuevas que están en versión preliminar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: karenhoran
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 06/23/2021
ms.author: markvi
ms.reviewer: besiler
ms.collection: M365-identity-device-management
ms.openlocfilehash: 209b9f66a9d7d8d004162e465c85311886b423fa
ms.sourcegitcommit: 27ddccfa351f574431fb4775e5cd486eb21080e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2021
ms.locfileid: "131997217"
---
# <a name="sign-in-logs-in-azure-active-directory---preview"></a>Registros de información de inicio de sesión en Azure Active Directory: versión preliminar

Como administrador de TI, quiere saber cómo funciona el entorno de TI. La información sobre el estado del sistema le permite evaluar si es necesario responder a posibles problemas y cómo hacerlo. 

Para ayudarle a conseguir este objetivo, el portal de Azure Active Directory le proporciona acceso a tres registros de actividad:

- **[Inicios de sesión](concept-sign-ins.md)** : Información sobre los inicios de sesión y sobre la forma en que los usuarios emplean los recursos.
- **[Auditoría](concept-audit-logs.md)** : información sobre los cambios aplicados al inquilino, como la administración de usuarios y grupos o las actualizaciones aplicadas a los recursos del inquilino.
- **[Aprovisionamiento](concept-provisioning-logs.md)** : actividades realizadas por el servicio de aprovisionamiento, como la creación de un grupo en ServiceNow o un usuario importado de Workday.


El registro de la información de inicio de sesión clásico de Azure Active Directory proporciona información general sobre los inicios de sesión de usuario interactivos. Además, ahora tiene acceso a tres registros de información de inicio de sesión adicionales que están en versión preliminar:

- Inicios de sesión de usuario no interactivos

- Inicios de sesión de entidad de servicio

- Inicios de sesión de identidades administradas para recursos de Azure

En este artículo se ofrece información general sobre el informe de actividad de inicio de sesión con la versión preliminar de los inicios de sesión no interactivos, de aplicaciones y de identidades administradas para recursos de Azure. Para obtener información sobre el informe de inicio de sesión sin las características en versión preliminar, consulte [Registros de información de inicio de sesión en Azure Active Directory](concept-sign-ins.md).



## <a name="what-can-you-do-with-it"></a>¿Para qué sirven el informe?

El registro de la información de inicio de sesión proporciona respuestas a preguntas como las siguientes:

- ¿Cuál es el patrón de inicio de sesión de un usuario, una aplicación o un servicio?

- ¿Cuántos usuarios, aplicaciones o servicios han iniciado sesión durante una semana?

- ¿Cuál es el estado de estos inicios de sesión?


## <a name="who-can-access-the-data"></a>¿Quién puede acceder a los datos?

- Usuarios de los roles Administrador de seguridad, Lector de seguridad o Lector de informes

- Administradores globales

- Cualquier usuario (no administradores) puede acceder a sus propios inicios de sesión 

## <a name="what-azure-ad-license-do-you-need"></a>¿Qué licencia de Azure AD necesita?

El inquilino debe tener una licencia de Azure AD Premium asociada para ver las actividades de inicio de sesión. Consulte [Introducción a Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) para actualizar la edición de Azure Active Directory. Los datos tardarán un par de días en aparecer en los registros después de actualizar a una licencia Premium sin actividades de datos antes de la actualización.




## <a name="where-can-you-find-it-in-the-azure-portal"></a>¿Dónde puede encontrarlo en Azure Portal?

Azure Portal ofrece varias opciones para acceder al registro. Por ejemplo, en el menú Azure Active Directory, puede abrir el registro en la sección **Supervisión**.  

![Apertura de los registros de inicios de sesión](./media/concept-sign-ins/sign-ins-logs-menu.png)

Además, puede acceder directamente al registro de la información de inicio de sesión mediante este vínculo: [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns)

En la página de inicios de sesión, puede cambiar entre:

- **Inicios de sesión de usuario interactivos**: inicios de sesión en los que un usuario proporciona un factor de autenticación, como una contraseña, una respuesta mediante una aplicación de MFA, un factor biométrico o un código QR.

- **Inicios de sesión de usuario no interactivos**: inicios de sesión realizados por un cliente en nombre de un usuario. Estos inicios de sesión no requieren ninguna interacción ni factor de autenticación del usuario. Por ejemplo, la autenticación y la autorización mediante tokens de acceso y actualización, que no requieren que un usuario escriba las credenciales.

- **Inicios de sesión de entidad de servicio**: inicios de sesión realizados por aplicaciones y entidades de servicio que no impliquen a ningún usuario. En estos inicios de sesión, la aplicación o el servicio proporcionan una credencial en su propio nombre para autenticarse o acceder a los recursos.

- **Inicios de sesión de identidades administradas para recursos de Azure**: inicios de sesión realizados por recursos de Azure que tienen secretos administrados por Azure. Para obtener más información, consulte [¿Qué son las identidades administradas para recursos de Azure?](../managed-identities-azure-resources/overview.md) 


![Tipos de registro de información de inicio de sesión](./media/concept-all-sign-ins/sign-ins-report-types.png)



Cada pestaña de la página de inicios de sesión muestra las columnas predeterminadas que aparecen a continuación. Algunas pestañas tienen columnas adicionales:

- Fecha de inicio de sesión

- Id. de solicitud

- Nombre de usuario o identificador de usuario

- Nombre de aplicación o identificador de aplicación

- Estado del inicio de sesión

- Dirección IP del dispositivo usado para el inicio de sesión



### <a name="interactive-user-sign-ins"></a>Inicios de sesión de usuario interactivos


Los inicios de sesión de usuario interactivos son inicios de sesión en los que un usuario proporciona un factor de autenticación para Azure AD o interactúa directamente con Azure AD o una aplicación auxiliar, como la aplicación Microsoft Authenticator. Los factores que los usuarios proporcionan incluyen las contraseñas, las respuestas a los desafíos de MFA, los factores biométricos o los códigos QR que un usuario proporciona a Azure AD o a una aplicación auxiliar.

> [!NOTE]
> Este registro también incluye los inicios de sesión federados de proveedores de identidades que están federados con Azure AD.  



> [!NOTE] 
> El registro de la información de inicio de sesión de usuario interactivo se usa para incluir algunos inicios de sesión no interactivos de los clientes de Microsoft Exchange. Aunque esos inicios de sesión no son interactivos, se han incluido en el registro de información de inicio de sesión de usuarios interactivos para obtener mayor visibilidad. Una vez que el registro de la información de inicio de sesión de usuarios no interactivos se introdujo en la versión preliminar pública en noviembre de 2020, los registros de la información de inicio de sesión no interactivos se movieron al informe de inicio de sesión de usuario no interactivo para aumentar la precisión. 


**Tamaño del informe:** pequeño <br> 
**Ejemplos:**

- Un usuario proporciona el nombre de usuario y la contraseña en la pantalla de inicio de sesión de Azure AD.

- Un usuario pasa un desafío de MFA por SMS.

- Un usuario proporciona un gesto biométrico para desbloquear su PC Windows con Windows Hello para empresas.

- Un usuario que está federado con Azure AD con una aserción de SAML de AD FS.


Además de los campos predeterminados, el registro de la información de inicio de sesión interactiva también muestra: 

- La ubicación de inicio de sesión

- Si se ha aplicado el acceso condicional



Puede personalizar la vista de lista, haga clic en **Columnas** en la barra de herramientas.

![Columnas de inicio de sesión de usuario interactivo](./media/concept-all-sign-ins/columns-interactive.png "Columnas del inicio de sesión de usuario interactivo")





La personalización de la vista permite mostrar campos adicionales o eliminar campos que ya se muestran.

![Todas las columnas interactivas](./media/concept-all-sign-ins/all-interactive-columns.png)


Seleccione un elemento de la vista de lista para obtener información más detallada sobre el inicio de sesión relacionado.

![Actividad de inicio de sesión](./media/concept-all-sign-ins/interactive-user-sign-in-details.png "Inicios de sesión de usuario interactivos")



### <a name="non-interactive-user-sign-ins"></a>Inicios de sesión de usuario no interactivos

Los inicios de sesión de usuario no interactivos son inicios de sesión que se realizaron mediante una aplicación cliente o componentes del sistema operativo en nombre de un usuario. Al igual que los inicios de sesión de usuario interactivos, estos inicios de sesión se realizan en nombre de un usuario. A diferencia de los inicios de sesión de usuario interactivos, estos inicios de sesión no requieren que el usuario proporcione un factor de autenticación. En su lugar, el dispositivo o la aplicación cliente usa un token o un código para autenticarse o acceder a un recurso en nombre de un usuario. En general, el usuario percibe estos inicios de sesión como algo que sucede en segundo plano de la actividad del usuario.


**Tamaño del informe:** grande <br>
**Ejemplos:** 

- Una aplicación cliente usa un token de actualización de OAuth 2.0 para obtener un token de acceso.

- Un cliente utiliza un código de autorización de OAuth 2.0 para obtener un token de acceso y un token de actualización.

- Un usuario realiza el inicio de sesión único (SSO) en una aplicación web o Windows en un equipo unido a Azure AD (sin proporcionar un factor de autenticación ni interactuar con un símbolo del sistema de Azure AD).

- Un usuario inicia sesión en una segunda aplicación de Microsoft Office mientras tiene una sesión en un dispositivo móvil que usa FOCI (Familia de identificadores de cliente).




Además de los campos predeterminados, el registro de la información de inicio de sesión no interactiva también muestra: 

- Id. de recurso

- Número de inicios de sesión agrupados




No se pueden personalizar los campos que se muestran en este informe.


![Columnas deshabilitadas](./media/concept-all-sign-ins/disabled-columns.png "Columnas deshabilitadas")

Para facilitar la síntesis de los datos, se agrupan los eventos de inicio de sesión no interactivos. A menudo, los clientes crean muchos inicios de sesión no interactivos en nombre del mismo usuario en un breve período de tiempo, que comparten las mismas características excepto la hora en que se intentó iniciar sesión. Por ejemplo, un cliente puede obtener un token de acceso una vez por hora en nombre de un usuario. Si el usuario o el cliente no cambian de estado, la dirección IP, el recurso y el resto de la información es la misma para cada solicitud de token de acceso. Cuando Azure AD registra varios inicios de sesión idénticos salvo por la fecha y la hora, dichos inicios de sesión de la misma entidad se agregan en una sola fila. Una fila con varios inicios de sesión idénticos (excepto por la fecha y la hora de emisión) tendrá un valor mayor que 1 en la columna Número de inicios de sesión. Puede expandir la fila para ver todos los inicios de sesión diferentes y sus diferentes marcas de tiempo. Los inicios de sesión se agregan para los usuarios no interactivos cuando coinciden los datos siguientes:


- Application

- Usuario

- Dirección IP

- Status

- Id. de recurso


Puede:

- Expandir un nodo para ver los elementos individuales de un grupo.  

- Hacer clic en un elemento individual para ver todos los detalles. 


![Detalles de los inicios de sesión de usuario no interactivos](./media/concept-all-sign-ins/non-interactive-sign-ins-details.png)




## <a name="service-principal-sign-ins"></a>Inicios de sesión de entidad de servicio

A diferencia de los inicios de sesión de usuario interactivos y no interactivos, los inicios de sesión de entidad de servicio no implican a un usuario. En su lugar, son inicios de sesión realizados por cualquier cuenta que no sea de usuario, como las aplicaciones o las entidades de servicio (excepto el inicio de sesión de identidad administrada, que se incluyen solo en el registro de información de inicio de sesión de identidad administrada). En estos inicios de sesión, la aplicación o el servicio proporcionan su propia credencial, como un certificado o un secreto, para autenticarse o acceder a los recursos.


**Tamaño del informe:** grande <br>
**Ejemplos:**

- Una entidad de servicio utiliza un certificado para autenticarse y acceder a Microsoft Graph. 

- Una aplicación utiliza un secreto de cliente para autenticarse en el flujo de credenciales del cliente de OAuth. 


Este informe tiene una vista de lista predeterminada que muestra:

- Fecha de inicio de sesión

- Id. de solicitud

- Nombre o identificador de la entidad de servicio

- Status

- Dirección IP

- Nombre del recurso

- Id. de recurso

- Número de inicios de sesión

No se pueden personalizar los campos que se muestran en este informe.

![Columnas deshabilitadas](./media/concept-all-sign-ins/disabled-columns.png "Columnas deshabilitadas")

Para facilitar la síntesis de los datos de los registros de inicio de sesión de la entidad de servicio, se agrupan los eventos de inicio de sesión de la entidad de servicio. Los inicios de sesión de la misma entidad en las mismas condiciones se agregan en una sola fila. Puede expandir la fila para ver todos los inicios de sesión diferentes y sus diferentes marcas de tiempo. Los inicios de sesión se agregan en el informe por entidad de servicio cuando coinciden los datos siguientes:

- Nombre o identificador de la entidad de servicio

- Status

- Dirección IP

- Nombre o identificador del recurso

Puede:

- Expandir un nodo para ver los elementos individuales de un grupo.  

- Hacer clic en un elemento individual para ver todos los detalles. 


![Detalles de las columnas](./media/concept-all-sign-ins/service-principals-sign-ins-view.png "Detalles de las columnas")




## <a name="managed-identity-for-azure-resources-sign-ins"></a>Inicios de sesión de identidad administrada para recursos de Azure 

Los inicios de sesión de identidad administrada para recursos de Azure son inicios de sesión realizados por los recursos cuyos secretos son administrados por Azure para simplificar la administración de credenciales.

**Tamaño del informe:** Pequeña <br> 
**Ejemplos:**

Una máquina virtual con credenciales administradas usa Azure AD para obtener un token de acceso.   


Este informe tiene una vista de lista predeterminada que muestra:


- Identificador de la identidad administrada

- Nombre de la identidad administrada

- Resource

- Id. de recurso

- Número de inicios de sesión agrupados

No se pueden personalizar los campos que se muestran en este informe.

Para que sea más fácil sintetizar los datos, se agrupan los eventos no interactivos en los registros de inicio de sesión de identidades administradas para recursos de Azure. Los inicios de sesión de la misma entidad se agregan en una sola fila. Puede expandir la fila para ver todos los inicios de sesión diferentes y sus diferentes marcas de tiempo. Los inicios de sesión se agregan en el informe de identidades administradas cuando coinciden los datos siguientes:

- Nombre o identificador de la identidad administrada

- Status

- Nombre o identificador del recurso

Seleccione un elemento en la vista de lista para mostrar todos los inicios de sesión que se han agrupado en un nodo.

Seleccione un elemento agrupado para ver todos los detalles del inicio de sesión. 


## <a name="sign-in-error-code"></a>Código de error de inicio de sesión

Si se produce un error al iniciar sesión, puede obtener más información sobre el motivo en la sección **Información básica** del elemento de registro relacionado. 

![Captura de pantalla que muestra una vista de información detallada.](./media/concept-all-sign-ins/error-code.png)
 
Aunque el elemento de registro proporciona un motivo del error, hay casos en los que puede obtener más información con la [herramienta de búsqueda de errores de inicio de sesión](https://login.microsoftonline.com/error). Por ejemplo, si está disponible, esta herramienta proporciona los pasos de corrección.  

![Herramienta de búsqueda de códigos de error](./media/concept-all-sign-ins/error-code-lookup-tool.png)



## <a name="filter-sign-in-activities"></a>Filtrado de las actividades de inicio de sesión

Al establecer un filtro, puede restringir el ámbito de los datos de inicio de sesión devueltos. Azure AD proporciona una amplia variedad de filtros adicionales que puede establecer. Al establecer el filtro, siempre debe prestar especial atención al filtro de intervalo del campo **Fecha** configurado. Un filtro de intervalo de fechas adecuado garantiza que Azure AD solo devuelva los datos que realmente le interesen.     

El filtro de intervalo del campo **Fecha** permite definir un período de tiempo para los datos devueltos.
Los valores posibles son:

- Un mes

- Siete días

- Veinticuatro horas

- Personalizado

![Filtro de intervalo de fechas](./media/concept-all-sign-ins/date-range-filter.png)





### <a name="filter-user-sign-ins"></a>Filtrado de los inicios de sesión de usuario

El filtro para los inicios de sesión interactivos y no interactivos es el mismo. Por este motivo, el filtro que haya configurado para los inicios de sesión interactivos se conserva para los inicios de sesión no interactivos y viceversa. 






## <a name="access-the-new-sign-in-activity-logs"></a>Acceso a los nuevos registro de actividad de inicio de sesión 

El informe actividad de inicios de sesión de Azure Portal proporciona un método sencillo para activar y desactivar el informe de la versión preliminar. Si tiene habilitados los registros de la versión preliminar, obtendrá un nuevo menú que le proporcionará acceso a todos los tipos de informes de actividad de inicio de sesión.     


Para acceder a los nuevos registros de información de inicio de sesión con los inicios de sesión no interactivos y de aplicaciones: 

1. En [Azure Portal](https://portal.azure.com), seleccione **Azure Active Directory**.

    ![Selección de Azure AD](./media/concept-all-sign-ins/azure-services.png)

2. En la sección **Supervisión**, haga clic en **Inicios de sesión**.

    ![Selección de Inicios de sesión](./media/concept-all-sign-ins/sign-ins.png)

3. Haga clic en la barra **Versión preliminar**.

    ![Habilitación de la nueva vista](./media/concept-all-sign-ins/enable-new-preview.png)

4. Para volver a la vista predeterminada, vuelva a hacer clic en la barra **Versión preliminar**. 

    ![Restauración de la vista clásica](./media/concept-all-sign-ins/switch-back.png)







## <a name="download-sign-in-activity-logs"></a>Descarga de los registros de actividad de inicio de sesión

Cuando se descarga un informe de actividad de inicio de sesión, se cumple lo siguiente:

- Puede descargar el informe de inicio de sesión como archivo CSV o JSON.

- Puede descargar hasta 100 000 registros. Si desea descargar más datos, use la API de generación de informes.

- La descarga se basa en la selección de filtros que ha realizado.

- El número de registros que se puede descargar también está restringido por las [directivas de retención de informes de Azure Active Directory](reference-reports-data-retention.md). 


![Descarga de registros](./media/concept-all-sign-ins/download-reports.png "Descarga de registros")


Cada descarga en formato CSV consta de seis archivos diferentes:

- Inicios de sesión interactivos

- Detalles de autenticación de los inicios de sesión interactivos

- Inicios de sesión no interactivos

- Detalles de autenticación de los inicios de sesión no interactivos

- Inicios de sesión de entidad de servicio

- Inicios de sesión de identidad administrada para recursos de Azure

Cada descarga en formato JSON consta de cuatro archivos diferentes:

- Inicios de sesión interactivos (incluye los detalles de autenticación)

- Inicios de sesión no interactivos (incluye los detalles de autenticación)

- Inicios de sesión de entidad de servicio

- Inicios de sesión de identidad administrada para recursos de Azure

![Descarga de archivos](./media/concept-all-sign-ins/download-files.png "Descarga de archivos")


## <a name="return-log-data-with-microsoft-graph"></a>Devolución de datos de registro con Microsoft Graph

Además de usar Azure Portal, puede consultar los registros de la información de inicio de sesión mediante Microsoft Graph API para devolver distintos tipos de información de inicio de sesión. Para evitar posibles problemas de rendimiento, el ámbito de la consulta debe ser solo los datos que le importen. 

En el ejemplo siguiente, el ámbito de la consulta se realiza por los registros de número, por un período concreto y por tipo de evento de inicio de sesión:

```msgraph-interactive
GET https://graph.microsoft.com/beta/auditLogs/signIns?$top=100&$filter=createdDateTime ge 2020-09-10T06:00:00Z and createdDateTime le 2020-09-17T06:00:00Z and signInEventTypes/any(t: t eq 'nonInteractiveUser')
```

Los parámetros de consulta del ejemplo proporcionan los siguientes resultados:

- El parámetro [$top](/graph/query-parameters#top-parameter) devuelve los cien primeros resultados.
- El parámetro [$filter](/graph/query-parameters#filter-parameter) limita el período de tiempo para devolver los resultados y usa la propiedad signInEventTypes para devolver solo inicios de sesión de usuario no interactivos.

Los siguientes valores están disponibles para filtrar por diferentes tipos de inicio de sesión: 

- interactiveUser
- nonInteractiveUser
- servicePrincipal 
- managedIdentity

## <a name="next-steps"></a>Pasos siguientes

* [Códigos de error de los informes de actividad de inicio de sesión](./concept-sign-ins.md)
* [Directivas de retención de datos de Azure AD](reference-reports-data-retention.md)
* [Latencias de informes de Azure AD](reference-reports-latencies.md)
