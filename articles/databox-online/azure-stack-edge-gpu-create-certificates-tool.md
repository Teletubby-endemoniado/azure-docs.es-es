---
title: Creación de certificados para Azure Stack Edge Pro con GPU mediante la herramienta Azure Stack Hub Readiness Checker
description: Describe cómo crear solicitudes de certificado y, después, obtener e instalar certificados en un dispositivo Azure Stack Edge Pro con GPU mediante la herramienta Azure Stack Hub Readiness Checker.
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 10/01/2021
ms.author: alkohli
ms.openlocfilehash: 5c6782c3b4644607f292587beafccb86fd7b8119
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131057806"
---
# <a name="create-certificates-for-your-azure-stack-edge-pro-gpu-using-azure-stack-hub-readiness-checker-tool"></a>Creación de certificados para Azure Stack Edge Pro con GPU mediante la herramienta Azure Stack Hub Readiness Checker 

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

En este artículo se describe cómo crear certificados para Azure Stack Edge Pro mediante la herramienta Azure Stack Hub Readiness Checker. 

## <a name="using-azure-stack-hub-readiness-checker-tool"></a>Uso de la herramienta Azure Stack Hub Readiness Checker

Use la herramienta Azure Stack Hub Readiness Checker para crear solicitudes de firma de certificado (CSR) para la implementación de un dispositivo Azure Stack Edge Pro. Estas solicitudes se pueden crear después de hacer un pedido del dispositivo Azure Stack Edge Pro y esperar a que llegue el dispositivo.

> [!NOTE]
> Use esta herramienta solo con fines de prueba o desarrollo, y no para dispositivos de producción. 

Puede usar la herramienta Azure Stack Hub Readiness Checker (AzsReadinessChecker) para solicitar los siguientes certificados:

- Certificado de Azure Resource Manager
- Certificado de UI local
- Certificado de nodo
- Certificado de blob
- Certificado de VPN


## <a name="prerequisites"></a>Requisitos previos

Para crear solicitudes de firma de certificado para la implementación de un dispositivo Azure Stack Edge Pro, asegúrese de lo siguiente: 

- Tiene un cliente que ejecuta Windows 10 o Windows Server 2016 o posterior. 
- Ha descargado la herramienta Microsoft Azure Stack Hub Readiness Checker [de la Galería de PowerShell](https://aka.ms/AzsReadinessChecker) en este sistema.
- Tiene la siguiente información para los certificados:
  - Nombre de dispositivo
  - Número de serie del nodo
  - Nombres de dominio completos (FQDN) externos

## <a name="generate-certificate-signing-requests"></a>Generación de solicitudes de firma de certificado

Siga estos pasos para preparar los certificados de dispositivo Azure Stack Edge Pro:

1. Ejecute PowerShell como administrador (5.1 o posterior).
2. Instale la herramienta Azure Stack Hub Readiness Checker. En el símbolo del sistema de PowerShell, escriba: 

    ```azurepowershell
    Install-Module -Name Microsoft.AzureStack.ReadinessChecker
    ```

    Para obtener la versión instalada, escriba:  

    ```azurepowershell
    Get-InstalledModule -Name Microsoft.AzureStack.ReadinessChecker  | ft Name, Version 
    ```

3. Cree un directorio para todos los certificados si aún no tiene uno. Escriba: 
    
    ```azurepowershell
    New-Item "C:\certrequest" -ItemType Directory
    ``` 
    
4. Para crear una solicitud de certificados, indique la siguiente información. Si va a generar un certificado de VPN, no se aplican algunas de estas entradas.
    
    |Entrada |Descripción  |
    |---------|---------|
    |`OutputRequestPath`|Ruta de acceso del archivo en el cliente local en el que quiere que se creen las solicitudes de certificados.        |
    |`DeviceName`|Nombre del dispositivo en la página **Dispositivos** en la interfaz de usuario web local del dispositivo. <br> Este campo no es obligatorio para un certificado de VPN.         |
    |`NodeSerialNumber`|Número de serie del nodo de dispositivo en la página **Red** en la interfaz de usuario web local del dispositivo. <br> Este campo no es obligatorio para un certificado de VPN.       |
    |`ExternalFQDN`|Para obtener esta dirección URL, vaya a la página **Dispositivos** en la interfaz de usuario web local del dispositivo.         |
    |`RequestType`|El tipo de solicitud puede ser para `MultipleCSR`, distintos certificados para los distintos puntos de conexión, o `SingleCSR`, un único certificado para todos los puntos de conexión. <br> Este campo no es obligatorio para un certificado de VPN.     |

    Para todos los certificados excepto el certificado de VPN, escriba: 
    
    ```azurepowershell
    $edgeCSRparams = @{
        CertificateType = 'AzureStackEdgeDEVICE'
        DeviceName = 'myTEA1'
        NodeSerialNumber = 'VM1500-00025'
        externalFQDN = 'azurestackedge.contoso.com'
        requestType = 'MultipleCSR'
        OutputRequestPath = "C:\certrequest"
    }
    New-AzsCertificateSigningRequest @edgeCSRparams
    ```

    Si va a crear un certificado de VPN, escriba: 

    ```azurepowershell
    $edgeCSRparams = @{
        CertificateType = 'AzureStackEdgeVPN'
        externalFQDN = 'azurestackedge.contoso.com'
        OutputRequestPath = "C:\certrequest"
    }
    New-AzsCertificateSigningRequest @edgeCSRparams
    ```

    
5. Encontrará los archivos de solicitud de certificados en el directorio que especificó en el parámetro OutputRequestPath anterior. Al usar el parámetro `MultipleCSR`, verá los cuatro archivos siguientes con la extensión `.req`:

    
    |Nombres de archivo  |Tipo de solicitud de certificados  |
    |---------|---------|
    |Comenzando con su `DeviceName`     |Solicitud de certificados de interfaz de usuario web local      |
    |Comenzando con su `NodeSerialNumber`     |Solicitud de certificado de nodo         |
    |A partir de `login`     |Solicitud de certificado de punto de conexión de Azure Resource Manager       |
    |A partir de `wildcard`     |Solicitud de certificado de almacenamiento de blobs. Contiene un carácter comodín porque abarca todas las cuentas de almacenamiento que se pueden crear en el dispositivo.          |
    |A partir de `AzureStackEdgeVPNCertificate`     |Solicitud de certificado de cliente VPN.         |

    También verá una carpeta INF. Contiene una administración.\<edge-devicename\> archivo de información en texto no cifrado que explica los detalles del certificado.  


6. Envíe estos archivos a su entidad de certificación (ya sea interna o pública). Asegúrese de que la entidad de certificación genere certificados con su solicitud generada que cumple los requisitos de certificados de Azure Stack Edge Pro para [certificados de nodos](azure-stack-edge-gpu-certificates-overview.md#node-certificates), [certificados de puntos de conexión](azure-stack-edge-gpu-certificates-overview.md#endpoint-certificates) y [certificados de interfaces de usuario locales](azure-stack-edge-gpu-certificates-overview.md#local-ui-certificates).

## <a name="prepare-certificates-for-deployment"></a>Preparación de los certificados para la implementación

Los archivos de certificado que obtiene de la entidad de certificación deben importarse y exportarse de forma que las propiedades coincidan con los requisitos de certificado del dispositivo Azure Stack Edge Pro. Complete los pasos siguientes en el mismo sistema donde generó las solicitudes de firma de certificados.

- Para importar los certificados, siga los pasos que se indican en [Importación de certificados en los clientes que acceden al dispositivo Azure Stack Edge Pro](azure-stack-edge-gpu-manage-certificates.md#import-certificates-on-the-client-accessing-the-device).

- Para exportar los certificados, siga los pasos que se indican en [Exportación de certificados de los clientes que acceden al dispositivo Azure Stack Edge Pro](azure-stack-edge-gpu-prepare-certificates-device-upload.md#export-certificates-as-pfx-format-with-private-key).


## <a name="validate-certificates"></a>Validación de certificados

En primer lugar, generará una estructura de carpetas adecuada y colocará los certificados en las carpetas correspondientes. Solo entonces se validarán los certificados con la herramienta.

1. Ejecute PowerShell como administrador.

2. Para generar la estructura de carpetas adecuada, en el símbolo del sistema, escriba:

    `New-AzsCertificateFolder -CertificateType AzureStackEdgeDevice -OutputPath "$ENV:USERPROFILE\Documents\AzureStackCSR"`

3. Convierta la contraseña PFX en una cadena segura. Escriba:       

    `$pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString` 

4. A continuación, valide los certificados. Escriba:

    `Invoke-AzsCertificateValidation -CertificateType AzureStackEdgeDevice -DeviceName mytea1 -NodeSerialNumber VM1500-00025 -externalFQDN azurestackedge.contoso.com -CertificatePath $ENV:USERPROFILE\Documents\AzureStackCSR\AzureStackEdge -pfxPassword $pfxPassword`

## <a name="next-steps"></a>Pasos siguientes

[Carga de certificados en el dispositivo](azure-stack-edge-gpu-manage-certificates.md).
