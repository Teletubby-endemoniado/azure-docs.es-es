---
title: Procedimientos a fin de usar Microsoft Defender para registros de contenedor
description: Obtenga información sobre el uso de Microsoft Defender para registros de contenedor a fin de examinar imágenes de Linux en los registros hospedados en Linux.
author: memildin
ms.author: memildin
ms.date: 10/21/2020
ms.topic: how-to
ms.service: security-center
manager: rkarlin
ms.custom: ignite-fall-2021
ms.openlocfilehash: 9925fcb9f6a4d4284c7c6784a45e896ea825abc0
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131056098"
---
# <a name="use-microsoft-defender-for-container-registries-to-scan-your-images-for-vulnerabilities"></a>Uso de Microsoft Defender para registros de contenedor a fin de examinar las imágenes en busca de vulnerabilidades

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

En esta página se explica cómo usar el analizador de vulnerabilidades integrado para examinar las imágenes de contenedor almacenadas en la instancia de Azure Container Registry basada en Azure Resource Manager.

Cuando **Microsoft Defender para registros de contenedor** está habilitado, cualquier imagen que inserte en el registro se examinará de inmediato. Además, también se analizan las imágenes extraídas en los últimos 30 días. 

Cuando el escáner notifica vulnerabilidades a Defender for Cloud, este último presenta los resultados y la información relacionada como recomendaciones. Además, los resultados incluyen información relacionada, como pasos de corrección, CVE pertinentes, puntuaciones de CVSS, etc. Se pueden ver las vulnerabilidades identificadas para una o varias suscripciones, o para un registro específico.

> [!TIP]
> También puede examinar las imágenes de contenedor en busca de vulnerabilidades a medida que las imágenes se integran en los flujos de trabajo de CI/CD de GitHub. Obtenga más información en [Identificación de imágenes de contenedor vulnerables en los flujos de trabajo de CI/CD](defender-for-container-registries-cicd.md).


## <a name="identify-vulnerabilities-in-images-in-azure-container-registries"></a>Identificación de vulnerabilidades en las imágenes de los registros de contenedor de Azure 

Para habilitar exámenes de vulnerabilidades de las imágenes almacenadas en la instancia de Azure Container Registry basada en Azure Resource Manager:

1. Habilite **Microsoft Defender para registros de contenedor** para su suscripción. Defender for Cloud ya está listo para examinar imágenes en los registros.

    >[!NOTE]
    > Esta característica se cobra por imagen.

1. Los recorridos de imagen se desencadenan en cada una de las inserciones o importaciones, y si la imagen se ha extraído en los últimos 30 días. 

    Cuando finaliza el examen (al cabo de 2 minutos aproximadamente, aunque puede tardar hasta 15 minutos), los resultados están disponibles como recomendaciones de Defender for Cloud.

1. [Vea y corrija los resultados tal y como se explica a continuación](#view-and-remediate-findings).

## <a name="identify-vulnerabilities-in-images-in-other-container-registries"></a>Identificación de vulnerabilidades en imágenes en otros registros de contenedor 

1. Use las herramientas de ACR para incorporar imágenes al registro desde Docker Hub o Microsoft Container Registry.  Una vez completada la importación, la solución de evaluación de vulnerabilidades integrada examina las imágenes importadas.

    Obtenga más información en [Importación de imágenes de contenedor en un registro de contenedor](../container-registry/container-registry-import-images.md).

    Cuando finaliza el examen (al cabo de 2 minutos aproximadamente, aunque puede tardar hasta 15 minutos), los resultados están disponibles como recomendaciones de Defender for Cloud.

1. [Vea y corrija los resultados tal y como se explica a continuación](#view-and-remediate-findings).


## <a name="view-and-remediate-findings"></a>Visualización y corrección de resultados

1. Para ver los resultados, vaya a la página **Recomendaciones**. Si se encontraron problemas, verá la recomendación **Las vulnerabilidades de las imágenes de Azure Container Registry deben corregirse**.

    ![Recomendación para corregir problemas.](media/monitor-container-security/acr-finding.png)

1. Seleccione la recomendación. 

    Se abre la página de detalles de la recomendación con información adicional. Esta información incluye la lista de registros con imágenes vulnerables ("recursos afectados") y los pasos de corrección. 

1. Seleccione un registro específico para ver los repositorios que contienen repositorios vulnerables.

    ![Seleccione un registro.](media/monitor-container-security/acr-finding-select-registry.png)

    Se abre la página Detalles del registro con la lista de repositorios afectados.

1. Seleccione un repositorio específico para ver los repositorios que contienen imágenes vulnerables.

    ![Seleccione uno.](media/monitor-container-security/acr-finding-select-repository.png)

    Se abre la página Detalles del repositorio. Muestra las imágenes vulnerables junto con una evaluación de la gravedad de las conclusiones.

1. Seleccione una imagen específica para ver las vulnerabilidades.

    ![Seleccione imágenes.](media/monitor-container-security/acr-finding-select-image.png)

    Se abre la lista de resultados de la imagen seleccionada.

    ![Lista de resultados.](media/monitor-container-security/acr-findings.png)

1. Para más información sobre un resultado, selecciónelo. 

    Se abre el panel de detalles de los resultados.

    [![Panel de detalles de los resultados.](media/monitor-container-security/acr-finding-details-pane.png)](media/monitor-container-security/acr-finding-details-pane.png#lightbox)

    En este panel se incluye una descripción detallada del problema y vínculos a recursos externos para ayudar a mitigar las amenazas.

1. Siga los pasos de la sección de corrección de este panel.

1. Cuando haya realizado los pasos necesarios para corregir el problema de seguridad, reemplace la imagen en el registro:

    1. Inserte la imagen actualizada. Esto desencadenará un examen. 
    
    1. Consulte en la página de recomendaciones la recomendación "Las vulnerabilidades de las imágenes de Azure Container Registry deben corregirse". 
    
        Si la recomendación sigue apareciendo y la imagen que ha corregido todavía aparece en la lista de imágenes vulnerables, vuelva a comprobar los pasos de corrección.

    1. Cuando esté seguro de que la imagen actualizada se ha insertado y examinado y que ya no aparece en la recomendación, elimine la imagen vulnerable "antigua" del registro.


## <a name="disable-specific-findings-preview"></a>Deshabilitación de resultados específicos (versión preliminar)

> [!NOTE]
> [!INCLUDE [Legalese](../../includes/security-center-preview-legal-text.md)]

Si tiene la necesidad organizativa de omitir un resultado, en lugar de corregirlo, tiene la opción de deshabilitarlo. Los resultados deshabilitados no afectan a la puntuación de seguridad ni generan ruido no deseado.

Cuando un resultado coincide con los criterios que ha definido en las reglas de deshabilitación, no aparecerá en la lista de resultados. Entre los escenarios típicos se incluyen:

- Deshabilitar resultados con gravedad inferior a media
- Deshabilitar resultados que no se pueden revisar
- Deshabilitar resultados con una puntuación de CVSS por debajo de 6,5
- Deshabilitar resultados con texto específico en la categoría o la comprobación de seguridad (por ejemplo, "RedHat", "actualización de seguridad de CentOS para sudo")

> [!IMPORTANT]
> Para crear una regla, necesita permisos para editar directivas en Azure Policy.
>
> Obtenga más información en [Permisos de Azure RBAC en Azure Policy](../governance/policy/overview.md#azure-rbac-permissions-in-azure-policy).

Puede utilizar cualquiera de los criterios siguientes: 

- Identificador del resultado 
- Category
- Comprobación de seguridad 
- Puntuaciones de CVSS v3
- Gravedad 
- Estado revisable 

Para crear una regla:

1. En la página de detalles de recomendaciones de **Las vulnerabilidades de las imágenes de Azure Container Registry deben corregirse**, seleccione **Deshabilitar regla**.
1. Seleccione el ámbito pertinente.
1. Defina los criterios.
1. Seleccione **Aplicar regla**.

    :::image type="content" source="./media/defender-for-container-registries-usage/new-disable-rule-for-registry-finding.png" alt-text="Creación de una regla de deshabilitación para los resultados de VA en el registro.":::

1. Para ver, invalidar o eliminar una regla: 
    1. Seleccione **Deshabilitar regla**.
    1. En la lista de ámbitos, las suscripciones con reglas activas se muestran como **Regla aplicada**.
        :::image type="content" source="./media/remediate-vulnerability-findings-vm/modify-rule.png" alt-text="Modificación o eliminación de una regla existente.":::
    1. Para ver o eliminar la regla, seleccione el menú de puntos suspensivos ("...").


## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre [los planes de protección avanzados de Microsoft Defender](defender-for-cloud-introduction.md).
