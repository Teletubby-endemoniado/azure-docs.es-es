---
title: Supervisión de la integridad de los archivos en Microsoft Defender para la nube
description: Obtenga información sobre cómo comparar líneas de base con la supervisión de integridad de archivos en Microsoft Defender para la nube.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: c8a2a589-b737-46c1-b508-7ea52e301e8f
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/17/2021
ms.author: memildin
ms.custom: ignite-fall-2021
ms.openlocfilehash: 0d5cceba40478759e36d08430dc2b2e4b30855f3
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131091633"
---
# <a name="compare-baselines-using-file-integrity-monitoring-fim"></a>Comparación de las líneas de base mediante la supervisión de la integridad de los archivos (FIM)

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

La supervisión de la integridad de los archivos (FIM) le informa cuando se producen cambios en zonas sensibles de los recursos, de manera que pueda investigar y abordar la actividad no autorizada. FIM supervisa archivos de Windows, registros y archivos de Linux.

En este tema se explica cómo habilitar FIM en los archivos y registros. Para obtener más información sobre FIM, vea [Supervisión de la integridad de los archivos en Microsoft Defender para la nube.](file-integrity-monitoring-overview.md)

## <a name="why-use-fim"></a>¿Por qué usar FIM?

El sistema operativo, las aplicaciones y las configuraciones asociadas controlan el estado de seguridad y el comportamiento de los recursos. Por lo tanto, los atacantes dirigen sus esfuerzos contra los archivos que controlan los recursos para superar al sistema operativo de un recurso o ejecutar actividades sin que se detecten.

De hecho, muchos estándares de cumplimiento normativo, como PCI-DSS e ISO 17799, requieren la implementación de controles de FIM.  

## <a name="enable-built-in-recursive-registry-checks"></a>Habilitación de comprobaciones de registro integradas recursivas

Los valores predeterminados del súbarbol del registro de FIM permiten supervisar de manera práctica los cambios recursivos dentro de las áreas de seguridad comunes.  Por ejemplo, un adversario puede configurar un script para ejecutarlo en el contexto LOCAL_SYSTEM mediante la configuración de una ejecución en el inicio o el apagado.  Para supervisar los cambios de este tipo, habilite la comprobación integrada.  

![Registro.](./media/file-integrity-monitoring-usage/baselines-registry.png)

>[!NOTE]
> Las comprobaciones recursivas solo se aplican a los subárboles de seguridad recomendados, no a las rutas de acceso al registro personalizadas.  

## <a name="add-a-custom-registry-check"></a>Incorporación de una comprobación de registro personalizada

Las líneas de base de FIM comienzan identificando las características de un estado correcto conocido para el sistema operativo y admitiendo la aplicación.  En este ejemplo, nos centraremos en las configuraciones de directiva de contraseña para Windows Server 2008 y versiones posteriores.


|Nombre de la directiva                 | Configuración del registro|
|---------------------------------------|-------------|
|Controlador de dominio: rechazar cambios de contraseña de cuenta de máquina| MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\RefusePasswordChange|
|Miembro de dominio: cifrar o firmar digitalmente datos de canal seguros (siempre)|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\RequireSignOrSeal|
|Miembro de dominio: cifrar digitalmente datos de canal seguros (cuando sea posible)|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\SealSecureChannel|
|Miembro de dominio: firmar digitalmente datos de canal seguros (cuando sea posible)|MACHINE\System\CurrentControlSet\Services   \Netlogon\Parameters\SignSecureChannel|
|Miembro de dominio: deshabilitar cambios de contraseña de cuenta de máquina|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\DisablePasswordChange|
|Miembro de dominio: antigüedad máxima de la contraseña de cuenta de máquina|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\MaximumPasswordAge|
|Miembro de dominio: requerir clave de sesión segura (Windows 2000 o posterior)|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\RequireStrongKey|
|Seguridad de redes: restringir NTLM:  autenticación NTLM en este dominio|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\RestrictNTLMInDomain|
|Seguridad de redes: restringir NTLM: agregar excepciones de servidor en este dominio|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\DCAllowedNTLMServers|
|Seguridad de redes: restringir NTLM: auditar autenticación NTLM en este dominio|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\AuditNTLMInDomain|

> [!NOTE]
> Para obtener más información sobre la configuración del registro compatible con las distintas versiones del sistema operativo, consulte la [hoja de cálculo de referencia de configuración de directiva de grupo](https://www.microsoft.com/download/confirmation.aspx?id=25250).

Para configurar FIM para supervisar las líneas de base del registro:

1. En la ventana **Agregar Registro de Windows para el seguimiento de cambios**, en el cuadro de texto **Clave del Registro de Windows**, escriba la siguiente clave del Registro.

    ```
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters
    ```

    :::image type="content" source="./media/file-integrity-monitoring-usage/baselines-add-registry.png" alt-text="Habilitación de FIM en un registro":::

## <a name="track-changes-to-windows-files"></a>Seguimiento de cambios en los archivos de Windows

1. En la ventana **Agregar Registro de Windows para el seguimiento de cambios**, en el cuadro de texto **Indicar ruta de acceso**, especifique la carpeta que contiene los archivos de los que quiere hacer un seguimiento. En el ejemplo de la siguiente ilustración, **Contoso Web App** reside en la unidad D:\ dentro de la estructura de carpetas **ContosWebApp**.  
1. Cree una entrada de archivo de Windows personalizada especificando un nombre de la clase de configuración, habilitando la recursión y especificando la carpeta principal con un sufijo de carácter comodín (*).

    :::image type="content" source="./media/file-integrity-monitoring-usage/baselines-add-file.png" alt-text="Habilitación de FIM en un archivo":::

## <a name="retrieve-change-data"></a>Recuperación de datos modificados

Los datos de la supervisión de la integridad de archivos residen en el conjunto de tablas de Azure Log Analytics/ConfigurationChange.  

 1. Establezca un intervalo de tiempo para recuperar un resumen de los cambios por recurso.
En el siguiente ejemplo, recuperaremos todos los cambios realizados en los últimos 14 días en las categorías de registro y archivos:

    <code>

    > ConfigurationChange

    > | where TimeGenerated > ago(14d)

    > | where ConfigChangeType in ('Registry', 'Files')

    > | summarize count() by Computer, ConfigChangeType

    </code>

1. Para ver los detalles de los cambios del registro:

    1. Quite **Files** de la cláusula **where**. 
    1. Quite la línea de resumen y reemplácela por una cláusula de ordenación:

    <code>

    > ConfigurationChange

    > | where TimeGenerated > ago(14d)

    > | where ConfigChangeType in ('Registry')

    > | order by Computer, RegistryKey

    </code>

Los informes se pueden exportar a CSV para archivado o canalizar a un informe de Power BI.  

![Datos de FIM](./media/file-integrity-monitoring-usage/baselines-data.png)
