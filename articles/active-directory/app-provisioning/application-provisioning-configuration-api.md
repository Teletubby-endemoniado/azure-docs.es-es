---
title: Configuración del aprovisionamiento mediante las Microsoft Graph API
description: Aprenda a ahorrar tiempo mediante el uso de Microsoft Graph API para automatizar la configuración del aprovisionamiento automático.
services: active-directory
author: kenwith
manager: karenh444
ms.service: active-directory
ms.subservice: app-provisioning
ms.topic: conceptual
ms.workload: identity
ms.date: 06/03/2021
ms.author: kenwith
ms.reviewer: arvinh
ms.openlocfilehash: b9d337af752d7b463ffc4922b903e4b6064c6881
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131028089"
---
# <a name="configure-provisioning-using-microsoft-graph-apis"></a>Configuración del aprovisionamiento mediante las Microsoft Graph API

Azure Portal ofrece una manera cómoda de configurar el aprovisionamiento de aplicaciones individuales de una en una. Pero si va a crear varias instancias de una aplicación, o incluso cientos, puede ser más fácil automatizar la creación y la configuración de las aplicaciones con Microsoft Graph API. En este artículo se explica cómo automatizar la configuración de aprovisionamiento a través de las API. Este método se usa normalmente para aplicaciones como [Amazon Web Services](../saas-apps/amazon-web-service-tutorial.md#configure-azure-ad-sso).

**Información general de los pasos para usar las Microsoft Graph API para automatizar la configuración del aprovisionamiento**


|Paso  |Detalles  |
|---------|---------|
|[Paso 1: Crear la aplicación de galería](#step-1-create-the-gallery-application)     |Iniciar sesión en el cliente API <br> Recuperar la plantilla de la aplicación de galería <br> Crear la aplicación de galería         |
|[Paso 2: Crear el trabajo de aprovisionamiento basado en la plantilla](#step-2-create-the-provisioning-job-based-on-the-template)     |Recuperar la plantilla para el conector de aprovisionamiento <br> Crear el trabajo de aprovisionamiento         |
|[Paso 3: Autorizar el acceso](#step-3-authorize-access)     |Probar la conexión con la aplicación <br> Guardar las credenciales         |
|[Paso 4: Iniciar el trabajo de aprovisionamiento](#step-4-start-the-provisioning-job)     |Inicio del trabajo         |
|[Paso 5: Supervisar el aprovisionamiento](#step-5-monitor-provisioning)     |Comprobar el estado del trabajo de aprovisionamiento <br> Recuperar los registros de aprovisionamiento         |

## <a name="step-1-create-the-gallery-application"></a>Paso 1: Crear la aplicación de galería

### <a name="sign-in-to-microsoft-graph-explorer-recommended-postman-or-any-other-api-client-you-use"></a>Iniciar sesión en el Explorador de Microsoft Graph (recomendado), Postman o cualquier otro cliente de API que use

1. Inicie el [Explorador de Microsoft Graph](https://developer.microsoft.com/graph/graph-explorer).
1. Seleccione el botón "Iniciar sesión con Microsoft" e inicie sesión con las credenciales de administrador de la aplicación o de administrador global de Azure AD.
1. Una vez haya iniciado sesión correctamente, verá los detalles de la cuenta de usuario en el panel izquierdo.

### <a name="retrieve-the-gallery-application-template-identifier"></a>Recuperar el identificador de plantilla de la aplicación de galería
Las aplicaciones de la galería de aplicaciones de Azure AD tienen una [plantilla de aplicación](/graph/api/applicationtemplate-list?tabs=http&view=graph-rest-beta&preserve-view=true) cada una, que describe los metadatos de esa aplicación. Con esta plantilla, puede crear una instancia de la aplicación y la entidad de servicio en el inquilino para la administración. Recupere el identificador de la plantilla de aplicación para **AWS Single-Account Access** y, a partir de la respuesta, registre el valor de la propiedad **id** para usarlo más adelante en este tutorial.

#### <a name="request"></a>Solicitud

```msgraph-interactive
GET https://graph.microsoft.com/beta/applicationTemplates?$filter=displayName eq 'AWS Single-Account Access'
```
#### <a name="response"></a>Response

<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.applicationTemplate",
  "isCollection": true
} -->

```http
HTTP/1.1 200 OK
Content-type: application/json

{
  "value": [
  {
    "id": "8b1025e4-1dd2-430b-a150-2ef79cd700f5",
        "displayName": "AWS Single-Account Access",
        "homePageUrl": "http://aws.amazon.com/",
        "supportedSingleSignOnModes": [
             "password",
             "saml",
             "external"
         ],
         "supportedProvisioningTypes": [
             "sync"
         ],
         "logoUrl": "https://az495088.vo.msecnd.net/app-logo/aws_215.png",
         "categories": [
             "developerServices"
         ],
         "publisher": "Amazon",
         "description": "Federate to a single AWS account and use SAML claims to authorize access to AWS IAM roles. If you have many AWS accounts, consider using the AWS Single Sign-On gallery application instead."    

}
```

### <a name="create-the-gallery-application"></a>Crear la aplicación de galería

Use el id. de plantilla recuperado para la aplicación en el último paso para [crear una instancia](/graph/api/applicationtemplate-instantiate?tabs=http&view=graph-rest-beta&preserve-view=true) de la aplicación y la entidad de servicio en el inquilino.

#### <a name="request"></a>Solicitud


```msgraph-interactive
POST https://graph.microsoft.com/beta/applicationTemplates/{id}/instantiate
Content-type: application/json

{
  "displayName": "AWS Contoso"
}
```

#### <a name="response"></a>Response

```http
HTTP/1.1 201 OK
Content-type: application/json

{
    "application": {
        "objectId": "cbc071a6-0fa5-4859-8g55-e983ef63df63",
        "appId": "92653dd4-aa3a-3323-80cf-e8cfefcc8d5d",
        "applicationTemplateId": "8b1025e4-1dd2-430b-a150-2ef79cd700f5",
        "displayName": "AWS Contoso",
        "homepage": "https://signin.aws.amazon.com/saml?metadata=aws|ISV9.1|primary|z",
        "replyUrls": [
            "https://signin.aws.amazon.com/saml"
        ],
        "logoutUrl": null,
        "samlMetadataUrl": null,
    },
    "servicePrincipal": {
        "objectId": "f47a6776-bca7-4f2e-bc6c-eec59d058e3e",
        "appDisplayName": "AWS Contoso",
        "applicationTemplateId": "8b1025e4-1dd2-430b-a150-2ef79cd700f5",
        "appRoleAssignmentRequired": true,
        "displayName": "My custom name",
        "homepage": "https://signin.aws.amazon.com/saml?metadata=aws|ISV9.1|primary|z",
        "replyUrls": [
            "https://signin.aws.amazon.com/saml"
        ],
        "servicePrincipalNames": [
            "93653dd4-aa3a-4323-80cf-e8cfefcc8d7d"
        ],
        "tags": [
            "WindowsAzureActiveDirectoryIntegratedApp"
        ],
    }
}
```

## <a name="step-2-create-the-provisioning-job-based-on-the-template"></a>Paso 2: Crear el trabajo de aprovisionamiento basado en la plantilla

### <a name="retrieve-the-template-for-the-provisioning-connector"></a>Recuperar la plantilla para el conector de aprovisionamiento

Las aplicaciones de la galería que están habilitadas para el aprovisionamiento tienen plantillas para simplificar la configuración. Use la solicitud siguiente para [recuperar la plantilla para la configuración del aprovisionamiento](/graph/api/synchronization-synchronizationtemplate-list?tabs=http&view=graph-rest-beta&preserve-view=true). Tenga en cuenta que tendrá que proporcionar el identificador. El identificador hace referencia al recurso anterior, que en este caso es el recurso servicePrincipal. 

#### <a name="request"></a>Solicitud

```msgraph-interactive
GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/templates
```

#### <a name="response"></a>Response
```http
HTTP/1.1 200 OK

{
    "value": [
        {
            "id": "aws",
            "factoryTag": "aws",
            "schema": {
                    "directories": [],
                    "synchronizationRules": []
                    }
        }
    ]
}
```

### <a name="create-the-provisioning-job"></a>Crear el trabajo de aprovisionamiento
Para habilitar el aprovisionamiento, primero deberá [crear un trabajo](/graph/api/synchronization-synchronizationjob-post?tabs=http&view=graph-rest-beta&preserve-view=true). Use la solicitud siguiente para crear un trabajo de aprovisionamiento. Use el elemento templateId del paso anterior al especificar la plantilla que se va a usar para el trabajo.

#### <a name="request"></a>Solicitud

```msgraph-interactive
POST https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs
Content-type: application/json

{ 
    "templateId": "aws"
}
```

#### <a name="response"></a>Response
```http
HTTP/1.1 201 OK
Content-type: application/json

{
    "id": "{jobId}",
    "templateId": "aws",
    "schedule": {
        "expiration": null,
        "interval": "P10675199DT2H48M5.4775807S",
        "state": "Disabled"
    },
    "status": {
        "countSuccessiveCompleteFailures": 0,
        "escrowsPruned": false,
        "synchronizedEntryCountByType": [],
        "code": "NotConfigured",
        "lastExecution": null,
        "lastSuccessfulExecution": null,
        "lastSuccessfulExecutionWithExports": null,
        "steadyStateFirstAchievedTime": "0001-01-01T00:00:00Z",
        "steadyStateLastAchievedTime": "0001-01-01T00:00:00Z",
        "quarantine": null,
        "troubleshootingUrl": null
    }
}
```

## <a name="step-3-authorize-access"></a>Paso 3: Autorizar el acceso

### <a name="test-the-connection-to-the-application"></a>Probar la conexión con la aplicación

Pruebe la conexión con la aplicación de otro fabricante. El ejemplo siguiente es para una aplicación que requiere un secreto de cliente y un token secreto. Cada aplicación tiene sus propios requisitos. Las aplicaciones suelen usar una dirección base en lugar de un secreto de cliente. Para determinar qué credenciales requiere la aplicación, vaya a la página de configuración de aprovisionamiento de la aplicación y, en el modo de desarrollador, haga clic en **Probar conexión**. El tráfico de red mostrará los parámetros que se usan para las credenciales. Para obtener una lista completa de las credenciales, vea [synchronizationJob: validateCredentials](/graph/api/synchronization-synchronizationjob-validatecredentials?tabs=http&view=graph-rest-beta&preserve-view=true). La mayoría de las aplicaciones, como Azure Databricks, se basan en BaseAddress y SecretToken. BaseAddress se conoce como una dirección URL de inquilino en Azure Portal. 

#### <a name="request"></a>Solicitud
```msgraph-interactive
POST https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{id}/validateCredentials

{ 
    "credentials": [ 
        { 
            "key": "ClientSecret", "value": "xxxxxxxxxxxxxxxxxxxxx" 
        },
        {
            "key": "SecretToken", "value": "xxxxxxxxxxxxxxxxxxxxx"
        }
    ]
}
```
#### <a name="response"></a>Response
```http
HTTP/1.1 204 No Content
```

### <a name="save-your-credentials"></a>Guardar las credenciales

Para configurar el aprovisionamiento, es necesario establecer una relación de confianza entre Azure AD y la aplicación. Autorice el acceso a la aplicación de terceros. El ejemplo siguiente es para una aplicación que requiere un secreto de cliente y un token secreto. Cada aplicación tiene sus propios requisitos. Revise la [documentación de la API](/graph/api/synchronization-synchronizationjob-validatecredentials?tabs=http&view=graph-rest-beta&preserve-view=true) para ver las opciones disponibles. 

#### <a name="request"></a>Solicitud
```msgraph-interactive
PUT https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/secrets 

{ 
    "value": [ 
        { 
            "key": "ClientSecret", "value": "xxxxxxxxxxxxxxxxxxxxx"
        },
        {
            "key": "SecretToken", "value": "xxxxxxxxxxxxxxxxxxxxx"
        }
    ]
}
```

#### <a name="response"></a>Response
```http
HTTP/1.1 204 No Content
```

## <a name="step-4-start-the-provisioning-job"></a>Paso 4: Iniciar el trabajo de aprovisionamiento
Ahora que el trabajo de aprovisionamiento está configurado, use el siguiente comando para [iniciar el trabajo](/graph/api/synchronization-synchronizationjob-start?tabs=http&view=graph-rest-beta&preserve-view=true). 


#### <a name="request"></a>Solicitud

```http
POST https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/start
```


#### <a name="response"></a>Response
```http
HTTP/1.1 204 No Content
```


## <a name="step-5-monitor-provisioning"></a>Paso 5: Supervisar el aprovisionamiento

### <a name="monitor-the-provisioning-job-status"></a>Supervisar el estado del trabajo de aprovisionamiento

Ahora que el trabajo de aprovisionamiento está en ejecución, use el siguiente comando para realizar el seguimiento del progreso del ciclo de aprovisionamiento actual, así como las estadísticas hasta la fecha, como el número de usuarios y grupos que se han creado en el sistema de destino. 

#### <a name="request"></a>Solicitud
```msgraph-interactive
GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/
```

#### <a name="response"></a>Response
```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "id": "{jobId}",
    "templateId": "aws",
    "schedule": {
        "expiration": null,
        "interval": "P10675199DT2H48M5.4775807S",
        "state": "Disabled"
    },
    "status": {
        "countSuccessiveCompleteFailures": 0,
        "escrowsPruned": false,
        "synchronizedEntryCountByType": [],
        "code": "Paused",
        "lastExecution": null,
        "lastSuccessfulExecution": null,
        "progress": [],
        "lastSuccessfulExecutionWithExports": null,
        "steadyStateFirstAchievedTime": "0001-01-01T00:00:00Z",
        "steadyStateLastAchievedTime": "0001-01-01T00:00:00Z",
        "quarantine": null,
        "troubleshootingUrl": null
    },
    "synchronizationJobSettings": [
      {
          "name": "QuarantineTooManyDeletesThreshold",
          "value": "500"
      }
    ]
}
```


### <a name="monitor-provisioning-events-using-the-provisioning-logs"></a>Supervisar eventos de aprovisionamiento mediante los registros de aprovisionamiento
Además de supervisar el estado del trabajo de aprovisionamiento, puede usar [registros de aprovisionamiento](/graph/api/provisioningobjectsummary-list?tabs=http&view=graph-rest-beta&preserve-view=true) para consultar todos los eventos que se están produciendo. Por ejemplo, consulte un usuario determinado y determine si se aprovisionó correctamente.

#### <a name="request"></a>Solicitud
```msgraph-interactive
GET https://graph.microsoft.com/beta/auditLogs/provisioning
```
#### <a name="response"></a>Response
```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "@odata.context": "https://graph.microsoft.com/beta/$metadata#auditLogs/provisioning",
    "value": [
        {
            "id": "gc532ff9-r265-ec76-861e-42e2970a8218",
            "activityDateTime": "2019-06-24T20:53:08Z",
            "tenantId": "7928d5b5-7442-4a97-ne2d-66f9j9972ecn",
            "cycleId": "44576n58-v14b-70fj-8404-3d22tt46ed93",
            "changeId": "eaad2f8b-e6e3-409b-83bd-e4e2e57177d5",
            "action": "Create",
            "durationInMilliseconds": 2785,
            "sourceSystem": {
                "id": "0404601d-a9c0-4ec7-bbcd-02660120d8c9",
                "displayName": "Azure Active Directory",
                "details": {}
            },
            "targetSystem": {
                "id": "cd22f60b-5f2d-1adg-adb4-76ef31db996b",
                "displayName": "AWS Contoso",
                "details": {
                    "ApplicationId": "f2764360-e0ec-5676-711e-cd6fc0d4dd61",
                    "ServicePrincipalId": "chc46a42-966b-47d7-9774-576b1c8bd0b8",
                    "ServicePrincipalDisplayName": "AWS Contoso"
                }
            },
            "initiatedBy": {
                "id": "",
                "displayName": "Azure AD Provisioning Service",
                "initiatorType": "system"
            }
            ]
       }
    ]
}
```
## <a name="see-also"></a>Vea también

- [Revisar la documentación de sincronización de Microsoft Graph](/graph/api/resources/synchronization-overview?view=graph-rest-beta&preserve-view=true)
- [Integración de una aplicación SCIM personalizada con Azure AD](./use-scim-to-provision-users-and-groups.md)
