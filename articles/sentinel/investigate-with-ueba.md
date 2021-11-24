---
title: Investigación de incidentes con datos de UEBA | Microsoft Docs
description: Aprenda a usar datos de UEBA durante la investigación para obtener un contexto mayor de la actividad potencialmente malintencionada que se produce en su organización.
services: sentinel
documentationcenter: na
author: batamig
manager: rkarlin
editor: ''
ms.service: microsoft-sentinel
ms.subservice: microsoft-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2021
ms.author: bagol
ms.custom: ignite-fall-2021
ms.openlocfilehash: 5cf711abf0373c2c7681d71426c0590b9561708e
ms.sourcegitcommit: 2ed2d9d6227cf5e7ba9ecf52bf518dff63457a59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132522885"
---
# <a name="tutorial-investigate-incidents-with-ueba-data"></a>Tutorial: Investigación de incidentes con datos de UEBA

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

En este artículo se describen métodos comunes y procedimientos de ejemplo para usar el [análisis de comportamiento de entidades de usuario (UEBA)](identify-threats-with-entity-behavior-analytics.md) en los flujos de trabajo de investigación normales.

> [!IMPORTANT]
>
> Actualmente, las características indicadas están en **VERSIÓN PRELIMINAR**. Consulte [Términos de uso complementarios para las Versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para conocer los términos legales adicionales que se aplican a las características de Azure que se encuentran en la versión beta, en versión preliminar o que todavía no se han publicado para que estén disponibles con carácter general.
>

> [!NOTE]
> En este tutorial se proporcionan procedimientos basados en escenarios para una tarea de cliente importante: investigación con datos de UEBA. Para más información, consulte el documento [Investigación de incidentes con Microsoft Sentinel](investigate-cases.md).
>
## <a name="prerequisites"></a>Requisitos previos

Para poder usar datos de UEBA en las investigaciones, debe [habilitar Análisis de comportamiento de usuarios y entidades (UEBA) en Microsoft Sentinel](enable-entity-behavior-analytics.md).

Empiece a buscar información con tecnología de la máquina aproximadamente una semana después de habilitar UEBA.

## <a name="run-proactive-routine-searches-in-entity-data"></a>Ejecución de búsquedas proactivas y rutinarias en datos de entidad

Se recomienda ejecutar búsquedas proactivas regulares a través de la actividad del usuario para crear clientes potenciales para realizar una investigación más exhaustiva.

Puede usar el libro [Análisis de comportamiento de usuarios y entidades](identify-threats-with-entity-behavior-analytics.md#hunting-queries-and-exploration-queries) de Microsoft Sentinel para consultar los datos, por ejemplo:

- **Usuarios de mayor riesgo**, con anomalías o incidentes asociados.
- **Datos de usuarios específicos**, para determinar si el sujeto realmente ha estado en peligro o si hay una amenaza interna debido a una acción que se desvía del perfil del usuario.

Además, capture acciones no rutinarias en el libro UEBA y úselas para buscar actividades anómalas y prácticas potencialmente no compatibles.

### <a name="investigate-an-anomalous-sign-in"></a>Investigación de un inicio de sesión anómalo

Por ejemplo, los pasos que se muestran a continuación siguen la investigación de un usuario que se conectó a una VPN que nunca había usado antes, lo que es una actividad anómala.

1. En el área **Workbooks** (Libros) de Sentinel, busque el libro **User and Entity Behavior Analytics** (Análisis de comportamiento de usuarios y entidades) y ábralo.
1. Busque el nombre de usuario concreto que va a investigar y seleccione su nombre en la tabla **Top users to investigate** (Principales usuarios para investigar).
1. Desplácese hacia abajo por las tablas **Incidents Breakdown** (Desglose de incidentes) y **Anomalies Breakdown** (Desglose de anomalías) para ver los incidentes y anomalías asociados al usuario seleccionado.
1. En la anomalía, como una llamada **Anomalous Successful Logon** (Inicio de sesión correcto anómalo), revise los detalles que se muestran en la tabla para investigar. Por ejemplo:

    |Paso  |Descripción  |
    |---------|---------|
    |**Anote la descripción de la derecha**     |    Cada anomalía tiene una descripción, con un vínculo para obtener más información en la [knowledge base MITRE ATT&CK](https://attack.mitre.org/). <br>Por ejemplo: <br><br>        **_Acceso inicial_* _<br>_El adversario intentando acceder a su red.* <br>* Acceso inicial consta de técnicas que usan varios vectores de entrada para acceder por primera vez a una red. Las técnicas que se usan para acceder por primera vez al phishing de objetivo definido dirigido y la explotación de puntos débiles en servidores web de acceso público. Los puntos de apoyo obtenidos a través del acceso inicial pueden permitir el acceso continuo, como las cuentas válidas y el uso de servicios remotos externos, o bien pueden tener un uso limitado debido al cambio de contraseñas.*     |
    |**Observe el texto de la columna Descripción**     |   En la fila de anomalías, desplácese a la derecha para ver una descripción adicional. Seleccione el vínculo para ver el texto completo. Por ejemplo: <br><br> *Los adversarios pueden robar las credenciales de un usuario o cuenta de servicio específicos mediante técnicas de acceso a credenciales, o bien capturar credenciales anteriormente en su proceso de reconocimiento a través de la ingeniería social para obtener acceso inicial. Por ejemplo, APT33 ha usado cuentas válidas para el acceso inicial. La consulta siguiente genera una salida de inicio de sesión correcto realizado por un usuario desde una nueva ubicación geográfica desde la que ni el usuario ni ninguno de sus pares se habían conectado nunca.*     |
    |**Fíjese en los datos de UsersInsights**     |  Desplácese más a la derecha en la fila de anomalías para ver los datos de información del usuario, como el nombre para mostrar de la cuenta y el identificador de objeto de la cuenta. Seleccione el texto para ver los datos completos a la derecha.         |
    |**Observe los datos de Evidence**     |  Desplácese más a la derecha de la fila de anomalías para ver los datos de evidencia de la anomalía. Seleccione la vista de texto de los datos completos a la derecha, como los campos siguientes: <br><br>-  **ActionUncommonlyPerformedByUser** <br>- **UncommonHighVolumeOfActions** <br>- **FirstTimeUserConnectedFromCountry** <br>- **CountryUncommonlyConnectedFromAmongPeers** <br>- **FirstTimeUserConnectedViaISP** <br>- **ISPUncommonlyUsedAmongPeers** <br>- **CountryUncommonlyConnectedFromInTenant** <br>- **ISPUncommonlyUsedInTenant** |
    |     |         |

Use los datos que se encuentran en el libro **User and Entity Behavior Analytics** (Análisis de comportamiento de usuarios y entidades) para determinar si la actividad del usuario es sospechosa y requiere más acciones.

## <a name="use-ueba-data-to-analyze-false-positives"></a>Uso de datos de UEBA para analizar falsos positivos

A veces, un incidente capturado en una investigación es un falso positivo.

Un ejemplo común de falso positivo es cuando se detecta una actividad de viaje imposible, como un usuario que ha iniciado sesión en una aplicación o portal desde Nueva York y Londres con menos de 60 minutos de diferencia. Aunque Microsoft Sentinel anota el viaje imposible como una anomalía, una investigación con el usuario podría aclarar que se usó una VPN con una ubicación alternativa a donde estaba realmente el usuario.

### <a name="analyze-a-false-positive"></a>Análisis de un falso positivo

Por ejemplo, en el caso de un incidente de un **viaje imposible**, después de confirmar con el usuario que se ha usado una VPN, vaya desde el incidente a la página de la entidad de usuario. Use los datos que se muestran en ella para determinar si las ubicaciones capturadas se incluyen en las ubicaciones más conocidas del usuario.

Por ejemplo:

[ ![Abra la página de entidad de usuario de un incidente.](media/ueba/open-entity-pages.png) ](media/ueba/open-entity-pages.png#lightbox)

Esta página también tiene un vínculo en la propia [página del incidente](investigate-cases.md#how-to-investigate-incidents) y el [gráfico de investigación](investigate-cases.md#use-the-investigation-graph-to-deep-dive).

> [!TIP]
> Después de confirmar los datos del usuario específico asociado al incidente que se encuentran en la página de entidad de usuario, vaya al área de **Búsqueda** de Microsoft Sentinel para saber si los pares del usuario normalmente se conectan desde las mismas ubicaciones. Si es así, este conocimiento sería un caso aún más seguro para un falso positivo.
>
> En el área de **búsqueda**, ejecute la consulta **Anomalous Geo Location Logon** (Inicio de sesión de ubicación geográfica anómala). Para obtener más información, consulte [Búsqueda de amenazas con Microsoft Sentinel](hunting.md).
>

### <a name="embed-identityinfo-data-in-your-analytics-rules-public-preview"></a>Inserción de datos de IdentityInfo en las reglas de análisis (versión preliminar pública)

Dado que los atacantes suelen usar las propias cuentas de usuario y servicio de la organización, los datos sobre esas cuentas de usuario, incluidos la identificación y los privilegios de usuario, son fundamentales para los analistas en el proceso de una investigación.

Inserte datos de la **tabla IdentityInfo** para ajustar las reglas de análisis a sus casos de uso a fin de reducir los falsos positivos y, posiblemente, acelerar el proceso de investigación.

Por ejemplo:

- Para correlacionar los eventos de seguridad con la tabla **IdentityInfo** en una alerta que se desencadena si alguien accede a un servidor fuera del departamento de **TI**:

    ```kusto
    SecurityEvent
    | where EventID in ("4624","4672")
    | where Computer == "My.High.Value.Asset"
    | join kind=inner  (
        IdentityInfo
        | summarize arg_max(TimeGenerated, *) by AccountObjectId) on $left.SubjectUserSid == $right.AccountSID
    | where Department != "IT"
    ```

- Para correlacionar registros de inicio de sesión de Azure AD con la tabla **IdentityInfo** en una alerta que se desencadena si alguien que no es miembro de un grupo de seguridad específico accede a una aplicación:

    ```kusto
    SigninLogs
    | where AppDisplayName == "GithHub.Com"
    | join kind=inner  (
        IdentityInfo
        | summarize arg_max(TimeGenerated, *) by AccountObjectId) on $left.UserId == $right.AccountObjectId
    | where GroupMembership !contains "Developers"
    ```

La tabla **IdentifyInfo** se sincroniza con el área de trabajo de Azure AD para crear una instantánea de los datos del perfil de usuario, como los metadatos de usuario, la información del grupo y los roles de Azure AD asignados a cada usuario. Para obtener más información, consulte [Tabla IdentityInfo](ueba-enrichments.md#identityinfo-table-public-preview) en la referencia de enriquecimientos de UEBA.

## <a name="identify-password-spray-and-spear-phishing-attempts"></a>Identificación de intentos de difusión de contraseña y phishing de objetivo definido

Sin la autenticación multifactor (MFA) habilitada, las credenciales de usuario son vulnerables a los atacantes que buscan ataques peligrosos con la realización de intentos de [difusión de contraseña](https://www.microsoft.com/security/blog/2020/04/23/protecting-organization-password-spray-attacks/) y [phishing de objetivo definido](https://www.microsoft.com/security/blog/2019/12/02/spear-phishing-campaigns-sharper-than-you-think/).

### <a name="investigate-a-password-spray-incident-with-ueba-insights"></a>Investigación de un incidente de difusión de contraseña con información de UEBA

Por ejemplo, para investigar un incidente de difusión de contraseña con información de UEBA, puede realizar las siguientes acciones para obtener más información:

1. En el incidente, en la parte inferior izquierda, seleccione **Investigate** para ver las cuentas, las máquinas y otros puntos de datos que podrían ser el objetivo de un ataque.

    Al examinar los datos, es posible que vea una cuenta de administrador con un número relativamente grande de errores de inicio de sesión. Aunque esto es sospechoso, es posible que no quiera restringir la cuenta sin más confirmación.

1. Seleccione la entidad de usuario administrativo en el mapa y, después, seleccione **Insights** (Información) a la derecha para encontrar más detalles, como el grafo de inicios de sesión a lo largo del tiempo.

1. Seleccione **Info** (Información) a la derecha y, después, seleccione **View full details** (Ver detalles completos) para ir a la [página de la entidad de usuario](identify-threats-with-entity-behavior-analytics.md#entity-pages) para profundizar más. 

    Por ejemplo, fíjese si este es el primer incidente de potencial difusión de contraseña del usuario o vea el historial de inicio de sesión del usuario para saber si los errores fueron anómalos.

> [!TIP]
> También puede ejecutar la [consulta de búsqueda](hunting.md) **Anomalous Failed Logon** (Inicio de sesión con errores anómalo) para supervisar todos los inicios de sesión con errores anómalos de una organización. Use los resultados de la consulta para iniciar investigaciones sobre posibles ataques de difusión de contraseña.
>

## <a name="url-detonation-public-preview"></a>Detonación de direcciones URL (versión preliminar pública)

Cuando hay direcciones URL en los registros ingeridos en Microsoft Sentinel, esas direcciones URL se detonan automáticamente para ayudar a acelerar el proceso de evaluación de prioridades. 

El gráfico Investigación incluye un nodo para la dirección URL detonada, así como los detalles siguientes:

- **DetonationVerdict**. Determinación booleana de alto nivel de la detonación. Por ejemplo, **Bad** significa que el lado se clasificó como contenido de malware o suplantación de identidad (phishing) de hospedaje.
- **DetonationFinalURL**. Dirección URL final de la página de aterrizaje observada, después de todos los redireccionamientos desde la dirección URL original.
- **DetonationScreenshot**. Captura de pantalla del aspecto de la página en el momento en que se desencadenó la alerta. Seleccione la captura de pantalla que desea ampliar.

Por ejemplo:

:::image type="content" source="media/investigate-with-ueba/url-detonation-example.png" alt-text="Detonación de la dirección URL de ejemplo que se muestra en el gráfico Investigación.":::

> [!TIP]
> Si no ve direcciones URL en los registros, compruebe que el registro de direcciones URL, también conocido como registro de amenazas, está habilitado para sus puertas de enlace web seguras, servidores proxy web, firewalls o IDS/IPS heredados.
>
> También puede crear registros personalizados para canalizar direcciones URL específicas de interés en Microsoft Sentinel para una investigación más detallada.
>

## <a name="next-steps"></a>Pasos siguientes

Más información sobre UEBA, investigaciones y búsqueda:

- [Identificación de amenazas avanzadas con el Análisis de comportamiento de usuarios y entidades (UEBA) en Microsoft Sentinel](identify-threats-with-entity-behavior-analytics.md)
- [Referencia de enriquecimientos de UEBA de Microsoft Sentinel](ueba-enrichments.md)
- [Tutorial: Investigación de incidentes con Microsoft Sentinel](investigate-cases.md)
- [Búsqueda de amenazas con Microsoft Sentinel](hunting.md)
