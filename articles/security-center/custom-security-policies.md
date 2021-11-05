---
title: Creación de directivas de seguridad personalizadas en Microsoft Defender for Cloud | Microsoft Docs
description: Definiciones de directivas personalizadas de Azure supervisadas por Microsoft Defender for Cloud.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 02/25/2021
ms.author: memildin
zone_pivot_groups: manage-asc-initiatives
ms.custom: ignite-fall-2021
ms.openlocfilehash: e596312807e2f6d5292c4910a7e2c3e9e7d75b3f
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131009992"
---
# <a name="create-custom-security-initiatives-and-policies"></a>Creación de directivas e iniciativas de seguridad personalizadas

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

Para ayudar a proteger los sistemas y el entorno, Microsoft Defender for Cloud genera recomendaciones de seguridad. Estas recomendaciones se basan en los procedimientos recomendados del sector, que se incorporan a la directiva de seguridad predeterminada genérica que se proporciona a todos los clientes. También pueden provenir de los conocimientos de Defender for Cloud acerca del sector y de los estándares normativos.

Gracias a esta característica, puede agregar sus propias iniciativas *personalizadas*. A continuación, recibirá recomendaciones si el entorno no sigue las directivas que creó. Cualquier iniciativa personalizada que cree aparecerá junto a las iniciativas integradas en el panel de cumplimiento normativo, según se describe en el tutorial [Mejora del cumplimiento normativo](regulatory-compliance-dashboard.md).

Como se explicó en la [documentación de Azure Policy](../governance/policy/concepts/definition-structure.md#definition-location), cuando se especifica una ubicación para la iniciativa personalizada, debe ser un grupo de administración o una suscripción. 

> [!TIP]
> Para obtener información general sobre los conceptos clave de esta página, consulte [¿Qué son las directivas de seguridad, las iniciativas y las recomendaciones?](security-policy-concept.md)

::: zone pivot="azure-portal"

## <a name="to-add-a-custom-initiative-to-your-subscription"></a>Agregar una iniciativa personalizada a la suscripción 

1. En el menú de Defender for Cloud, seleccione **Directiva de seguridad**.

1. Seleccione una suscripción o un grupo de administración al que quiera agregar una iniciativa personalizada.

    [![Selección de la suscripción para la que se va a crear una directiva personalizada.](media/custom-security-policies/custom-policy-selecting-a-subscription.png)](media/custom-security-policies/custom-policy-selecting-a-subscription.png#lightbox)

    > [!NOTE]
    > Para que las iniciativas personalizadas se evalúen y se muestren en Defender for Cloud, debe agregarlas en el nivel de suscripción (o superior). Se recomienda seleccionar el ámbito más amplio disponible.

1. En la página Directiva de seguridad, en Sus iniciativas personalizadas, haga clic en **Agregar una iniciativa personalizada**.

    [![Haga clic en Agregar una iniciativa personalizada.](media/custom-security-policies/custom-policy-add-initiative.png)](media/custom-security-policies/custom-policy-add-initiative.png#lightbox)

    Aparecerá la página siguiente:

    ![Crear o agregar una directiva.](media/custom-security-policies/create-or-add-custom-policy.png)

1. En la página para agregar iniciativas personalizadas, revise la lista de directivas personalizadas ya creadas en la organización. Si ve una que quiera asignar a la suscripción, haga clic en **Agregar**. Si no hay una iniciativa en la lista que satisfaga sus necesidades, omita este paso.

1. Para crear una nueva iniciativa personalizada:

    1. Haga clic en **Crear nueva**.
    1. Escriba la ubicación y el nombre de la definición.
    1. Seleccione las directivas que quiera incluir y haga clic en **Agregar**.
    1. Escriba los parámetros que quiera.
    1. Haga clic en **Save**(Guardar).
    1. En la página Adición de iniciativas personalizadas, haga clic en Actualizar. La nueva iniciativa se mostrará como disponible.
    1. Haga clic en **Agregar** y asígnela a su suscripción.

    > [!NOTE]
    > Recuerde que la creación de nuevas iniciativas requiere credenciales de propietario de la suscripción. Para obtener más información sobre los roles de Azure, consulte [Permisos de Microsoft Defender for Cloud](permissions.md).

    Su nueva iniciativa se aplica y puede ver el impacto de las dos formas siguientes:

    * En el menú de Defender for Cloud, seleccione **Cumplimiento normativo**. El panel de cumplimiento se abre para mostrar su nueva iniciativa personalizada junto a las iniciativas integradas.
    
    * Empezará a recibir recomendaciones si el entorno no sigue las directivas que ha definido.

1. Para ver las recomendaciones resultantes de la directiva, haga clic en **Recomendaciones** de la barra lateral para abrir la página recomendaciones. Las recomendaciones aparecerán con una etiqueta "Personalizada" y estarán disponibles en aproximadamente una hora.

    [![Recomendaciones personalizadas.](media/custom-security-policies/custom-policy-recommendations.png)](media/custom-security-policies/custom-policy-recommendations-in-context.png#lightbox)

::: zone-end

::: zone pivot="rest-api"

## <a name="configure-a-security-policy-in-azure-policy-using-the-rest-api"></a>Configuración de una directiva de seguridad mediante la API de REST

Como parte de la integración nativa con Azure Policy, Microsoft Defender for Cloud le permite aprovechar la API de REST de Azure Policy para crear asignaciones de directivas. Las siguientes instrucciones le guiarán en la creación de asignaciones de directivas, así como en la personalización de asignaciones existentes. 

Conceptos importantes de Azure Policy: 

- Una **definición de directiva** es una regla. 

- Una **iniciativa** es una colección de definiciones de directivas (reglas). 

- Una **asignación** es una aplicación de una iniciativa o una directiva para un ámbito concreto (grupo de administración o suscripción, entre otros). 

Defender for Cloud tiene una iniciativa integrada, [Azure Security Benchmark](/security/benchmark/azure/introduction), que incluye todas sus directivas de seguridad. Para evaluar las directivas de Defender for Cloud sobre los recursos de Azure, debe crear una asignación en el grupo de administración o en la suscripción que quiera evaluar.

De manera predeterminada, la iniciativa integrada tiene todas las directivas de Defender for Cloud habilitadas. Puede optar por deshabilitar determinadas directivas de la iniciativa integrada. Por ejemplo, para aplicar todas las directivas de Defender for Cloud excepto el **firewall de aplicaciones web**, cambie el valor del parámetro de efecto de directiva a **Deshabilitado**.

## <a name="api-examples"></a>Ejemplos de API

En los siguientes ejemplos, reemplace estas variables:

- **{scope}** escriba el nombre de la suscripción o el grupo de administración al que va a aplicar la directiva.
- **{policyAssignmentName}** escriba el nombre de la asignación de directiva pertinente.
- **{name}** escriba su nombre o el nombre del administrador que aprobó el cambio de directiva.

En este ejemplo se muestra cómo asignar la iniciativa integrada de Defender for Cloud a una suscripción o un grupo de administración
 
 ```
    PUT  
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 

    Request Body (JSON) 

    { 

      "properties":{ 

    "displayName":"Enable Monitoring in Microsoft Defender for Cloud", 

    "metadata":{ 

    "assignedBy":"{Name}" 

    }, 

    "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 

    "parameters":{}, 

    } 

    } 
 ```

En este ejemplo se muestra cómo asignar la iniciativa integrada de Defender for Cloud a una suscripción, con las directivas siguientes deshabilitadas: 

- Actualizaciones del sistema ("systemUpdatesMonitoringEffect") 

- Configuraciones de seguridad ("systemConfigurationsMonitoringEffect") 

- Protección de extremos ("endpointProtectionMonitoringEffect") 

 ```
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 
    
    Request Body (JSON) 
    
    { 
    
      "properties":{ 
    
    "displayName":"Enable Monitoring in Microsoft Defender for Cloud", 
    
    "metadata":{ 
    
    "assignedBy":"{Name}" 
    
    }, 
    
    "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 
    
    "parameters":{ 
    
    "systemUpdatesMonitoringEffect":{"value":"Disabled"}, 
    
    "systemConfigurationsMonitoringEffect":{"value":"Disabled"}, 
    
    "endpointProtectionMonitoringEffect":{"value":"Disabled"}, 
    
    }, 
    
     } 
    
    } 
 ```
En este ejemplo se muestra cómo quitar una asignación:
 ```
    DELETE   
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 
 ```

::: zone-end


## <a name="enhance-your-custom-recommendations-with-detailed-information"></a>Mejora de las recomendaciones personalizadas con información detallada

Las recomendaciones integradas que se suministran con Microsoft Defender for Cloud incluyen detalles como los niveles de gravedad y las instrucciones de corrección. Si desea agregar este tipo de información a las recomendaciones personalizadas para que aparezca en Azure Portal o donde acceda a las recomendaciones, debe usar la API de REST. 

Los dos tipos de información que puede agregar son:

- **RemediationDescription**, cadena
- **Severity**, enumeración [Low, Medium, High]

Los metadatos deben agregarse a la definición de la directiva para una directiva que forme parte de la iniciativa personalizada. Debe estar en la propiedad "securityCenter", como se muestra a continuación:

```json
 "metadata": {
    "securityCenter": {
        "RemediationDescription": "Custom description goes here",
        "Severity": "High"
    },
```

A continuación se muestra un ejemplo de una directiva personalizada que incluye la propiedad metadata/securityCenter:

  ```json
  {
"properties": {
    "displayName": "Security - ERvNet - AuditRGLock",
    "policyType": "Custom",
    "mode": "All",
    "description": "Audit required resource groups lock",
    "metadata": {
        "securityCenter": {
            "RemediationDescription": "Resource Group locks can be set via Azure Portal -> Resource Group -> Locks",
            "Severity": "High"
        }
    },
    "parameters": {
        "expressRouteLockLevel": {
            "type": "String",
            "metadata": {
                "displayName": "Lock level",
                "description": "Required lock level for ExpressRoute resource groups."
            },
            "allowedValues": [
                "CanNotDelete",
                "ReadOnly"
            ]
        }
    },
    "policyRule": {
        "if": {
            "field": "type",
            "equals": "Microsoft.Resources/subscriptions/resourceGroups"
        },
        "then": {
            "effect": "auditIfNotExists",
            "details": {
                "type": "Microsoft.Authorization/locks",
                "existenceCondition": {
                    "field": "Microsoft.Authorization/locks/level",
                    "equals": "[parameters('expressRouteLockLevel')]"
                }
            }
        }
    }
}
}
  ```

Para ver otro ejemplo del uso de la propiedad de securityCenter, consulte [esta sección de la documentación de la API de REST](/rest/api/securitycenter/assessmentsmetadata/createinsubscription#examples).


## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a crear directivas de seguridad personalizadas. 

Para obtener material relacionado, consulte los siguientes artículos: 

- [Introducción a las directivas de seguridad](tutorial-security-policy.md)
- [Lista de directivas de seguridad integradas](./policy-reference.md)
