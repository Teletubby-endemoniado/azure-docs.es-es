---
title: Parámetros en plantillas
description: Se describe cómo definir parámetros en una plantilla de Azure Resource Manager (plantilla de ARM).
ms.topic: conceptual
ms.date: 05/14/2021
ms.openlocfilehash: 5c94dc3f4d37fa6c08e29e03e88dd3ba54e68271
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128625532"
---
# <a name="parameters-in-arm-templates"></a>Parámetros en plantillas de ARM

En este artículo se describe cómo definir y usar parámetros en una plantilla de Azure Resource Manager (plantilla de ARM). Si proporciona valores diferentes para los parámetros, podrá volver a usar una plantilla para distintos entornos.

Resource Manager resuelve los valores de parámetro antes de iniciar las operaciones de implementación. Siempre que el parámetro se use en la plantilla, Resource Manager lo reemplaza por el valor resuelto.

Cada parámetro debe establecerse en uno de los [tipos de datos](data-types.md).

## <a name="minimal-declaration"></a>Declaración mínima

Como mínimo, cada parámetro necesita un nombre y un tipo.

Al implementar una plantilla mediante Azure Portal, los nombres de parámetro con mayúsculas y minúsculas Camel se convierten en nombres separados por espacios. Por ejemplo, *demoString* en el ejemplo siguiente se muestra como *cadena de demostración*. Para más información, consulte [Usar un botón de implementación para implementar plantillas desde el repositorio de GitHub](./deploy-to-azure-button.md) e [Implementación de recursos con plantillas de ARM y Azure Portal](./deploy-portal.md).

```json
"parameters": {
  "demoString": {
    "type": "string"
  },
  "demoInt": {
    "type": "int"
  },
  "demoBool": {
    "type": "bool"
  },
  "demoObject": {
    "type": "object"
  },
  "demoArray": {
    "type": "array"
  }
}
```

## <a name="secure-parameters"></a>Parámetros seguros

Puede marcar los parámetros de objeto o cadena como seguros. El valor de un parámetro seguro no se guarda en el historial de implementaciones y no se registra.

```json
"parameters": {
  "demoPassword": {
    "type": "secureString"
  },
  "demoSecretObject": {
    "type": "secureObject"
  }
}
```

## <a name="allowed-values"></a>Valores permitidos

Puede definir valores permitidos para un parámetro. Los valores permitidos se proporcionan en una matriz. Se produce un error en la implementación durante la validación si se pasa un valor para el parámetro que no es uno de los valores permitidos.

```json
"parameters": {
  "demoEnum": {
    "type": "string",
    "allowedValues": [
      "one",
      "two"
    ]
  }
}
```

## <a name="default-value"></a>Valor predeterminado

Puede especificar un valor predeterminado para un parámetro. El valor predeterminado se usa cuando no se proporciona un valor durante la implementación.

```json
"parameters": {
  "demoParam": {
    "type": "string",
    "defaultValue": "Contoso"
  }
}
```

Para especificar un valor predeterminado junto con otras propiedades para el parámetro, use la sintaxis siguiente.

```json
"parameters": {
  "demoParam": {
    "type": "string",
    "defaultValue": "Contoso",
    "allowedValues": [
      "Contoso",
      "Fabrikam"
    ]
  }
}
```

Puede usar expresiones con el valor predeterminado. La función no puede usar la función [reference](template-functions-resource.md#reference) ni ninguna de las funciones [list](template-functions-resource.md#list) de la sección de parámetros. Estas funciones obtienen el estado de tiempo de ejecución de un recurso y no se pueden ejecutar antes de la implementación cuando se resuelven parámetros.

No se permiten expresiones con otras propiedades de parámetro.

```json
"parameters": {
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]"
  }
}
```

Puede usar otro valor de parámetro para compilar un valor predeterminado. La plantilla siguiente crea un nombre de plan de host a partir del nombre del sitio.

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]"
  }
}
```

## <a name="length-constraints"></a>Restricciones de longitud

Puede especificar la longitud mínima y máxima de los parámetros de matriz y de cadena. Puede establecer una o las dos restricciones. Para las cadenas, la longitud indica el número de caracteres. Para las matrices, la longitud indica el número de elementos de la matriz.

En el ejemplo siguiente se declaran dos parámetros. Un parámetro es para un nombre de cuenta de almacenamiento que debe tener entre 3 y 24 caracteres. El otro parámetro es una matriz que debe tener entre 1 y 5 elementos.

```json
"parameters": {
  "storageAccountName": {
    "type": "string",
    "minLength": 3,
    "maxLength": 24
  },
  "appNames": {
    "type": "array",
    "minLength": 1,
    "maxLength": 5
  }
}
```

## <a name="integer-constraints"></a>Restricciones de enteros

Puede establecer valores mínimos y máximos para los parámetros de entero. Puede establecer una o las dos restricciones.

```json
"parameters": {
  "month": {
    "type": "int",
    "minValue": 1,
    "maxValue": 12
  }
}
```

## <a name="description"></a>Descripción

Puede agregar una descripción a un parámetro para ayudar a los usuarios de la plantilla a comprender el valor que deben proporcionar. Al implementar la plantilla a través del portal, el texto que proporciona en la descripción se usa automáticamente como sugerencia para ese parámetro. Agregue solo una descripción cuando el texto proporcione más información de la que se puede deducir del nombre del parámetro.

```json
"parameters": {
  "virtualMachineSize": {
    "type": "string",
    "metadata": {
      "description": "Must be at least Standard_A3 to support 2 NICs."
    },
    "defaultValue": "Standard_DS1_v2"
  }
}
```

## <a name="use-parameter"></a>Uso del parámetro

Para hacer referencia al valor de un parámetro, use la función [parameters](template-functions-deployment.md#parameters). En el ejemplo siguiente se usa un valor de parámetro para un nombre de almacén de claves.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vaultName": {
      "type": "string",
      "defaultValue": "[format('keyVault{0}', uniqueString(resourceGroup().id))]"
    }
  },
  "resources": [
    {
      "type": "Microsoft.KeyVault/vaults",
      "apiVersion": "2021-06-01-preview",
      "name": "[parameters('vaultName')]",
      ...
    }
  ]
}
```

## <a name="objects-as-parameters"></a>Objetos como parámetros

Para organizar los valores relacionados, páselos en un objeto. Con este enfoque también se reduce el número de parámetros de la plantilla.

En el ejemplo siguiente se muestra un parámetro que es un objeto. El valor predeterminado muestra las propiedades esperadas para el objeto. Esas propiedades se usan al definir el recurso que se va a implementar.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vNetSettings": {
      "type": "object",
      "defaultValue": {
        "name": "VNet1",
        "location": "eastus",
        "addressPrefixes": [
          {
            "name": "firstPrefix",
            "addressPrefix": "10.0.0.0/22"
          }
        ],
        "subnets": [
          {
            "name": "firstSubnet",
            "addressPrefix": "10.0.0.0/24"
          },
          {
            "name": "secondSubnet",
            "addressPrefix": "10.0.1.0/24"
          }
        ]
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Network/virtualNetworks",
      "apiVersion": "2021-02-01",
      "name": "[parameters('vNetSettings').name]",
      "location": "[parameters('vNetSettings').location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[parameters('vNetSettings').addressPrefixes[0].addressPrefix]"
          ]
        },
        "subnets": [
          {
            "name": "[parameters('vNetSettings').subnets[0].name]",
            "properties": {
              "addressPrefix": "[parameters('vNetSettings').subnets[0].addressPrefix]"
            }
          },
          {
            "name": "[parameters('vNetSettings').subnets[1].name]",
            "properties": {
              "addressPrefix": "[parameters('vNetSettings').subnets[1].addressPrefix]"
            }
          }
        ]
      }
    }
  ]
}
```

## <a name="example-templates"></a>Plantillas de ejemplo

En los siguientes ejemplos se muestran escenarios para usar parámetros.

|Plantilla  |Descripción  |
|---------|---------|
|[parámetros con funciones para los valores predeterminados](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Muestra cómo utilizar las funciones de plantilla al definir valores predeterminados para parámetros. La plantilla no implementa ningún recurso. Genera valores de parámetro y devuelve dichos valores. |
|[objeto de parámetro](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Muestra cómo utilizar un objeto para un parámetro. La plantilla no implementa ningún recurso. Genera valores de parámetro y devuelve dichos valores. |

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre las propiedades disponibles para parámetros, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](./syntax.md).
* Para más información sobre cómo pasar valores de parámetro como un archivo, consulte [Creación de un archivo de parámetros de Resource Manager](parameter-files.md).
* Para obtener recomendaciones sobre cómo crear parámetros, consulte [Procedimientos recomendados: parámetros](./best-practices.md#parameters).
