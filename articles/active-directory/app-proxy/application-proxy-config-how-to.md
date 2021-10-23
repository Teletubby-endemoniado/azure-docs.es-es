---
title: Cómo configurar una aplicación de Azure Active Directory Application Proxy
description: Aprenda a crear y configurar una aplicación de Azure Active Directory Application Proxy en unos pocos pasos sencillos
services: active-directory
author: kenwith
manager: karenh444
ms.service: active-directory
ms.subservice: app-proxy
ms.workload: identity
ms.topic: troubleshooting
ms.date: 04/27/2021
ms.author: kenwith
ms.reviewer: asteen
ms.openlocfilehash: 40377abd1faffb51e686099a2574785db3811701
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "129990337"
---
# <a name="how-to-configure-an-application-proxy-application"></a>Configuración de una aplicación de proxy de aplicación

Este artículo lo ayuda a entender cómo configurar una aplicación de proxy de aplicación en Azure AD para exponer las aplicaciones locales a la nube.

## <a name="recommended-documents"></a>Documentos recomendados

Para aprender sobre las configuraciones iniciales y la creación de una aplicación de proxy de aplicación mediante el portal de administración, consulte [Publicación de aplicaciones mediante el proxy de aplicación de Azure AD](application-proxy-add-on-premises-application.md).

Para ver detalles sobre la configuración de conectores, consulte [Habilitación del proxy de aplicación en Azure Portal](application-proxy-add-on-premises-application.md).

Para información sobre cómo cargar certificados y usar dominios personalizados, consulte [Uso de dominios personalizados en el proxy de la aplicación de Azure AD](application-proxy-configure-custom-domain.md).

## <a name="create-the-applicationsetting-the-urls"></a>Creación de la aplicación y establecimiento de las direcciones URL

Si está siguiendo los pasos de la documentación [Publicación de aplicaciones mediante el proxy de aplicación de Azure AD](application-proxy-add-on-premises-application.md) y obtiene un error al crear la aplicación, consulte los detalles del error para ver información y sugerencias para corregir la aplicación. La mayoría de los mensajes de error incluyen una sugerencia de corrección. Para evitar errores habituales, compruebe que:

- Sea un administrador con permiso para crear una aplicación de proxy de aplicación;
- La dirección URL interna sea única;
- La dirección URL externa sea única;
- Las direcciones URL empiecen por http o https y terminen en "/";
- La dirección URL debe ser un nombre de dominio y no una dirección IP.

El mensaje de error debería aparecer en la esquina superior derecha cuando cree la aplicación. También puede seleccionar el icono de notificación para ver los mensajes de error.

![Muestra dónde encontrar el mensaje de notificación en Azure Portal](./media/application-proxy-config-how-to/error-message.png)

## <a name="configure-connectorsconnector-groups"></a>Configuración de conectores y grupos de conectores

Si tiene dificultades para configurar la aplicación a causa de la advertencia sobre los conectores y grupos de conectores, consulte las instrucciones para habilitar el proxy de aplicación, donde se ofrecen más detalles sobre cómo descargar conectores. Si desea más información acerca de los conectores, consulte la [documentación sobre los conectores](application-proxy-connectors.md).

Si los conectores están inactivos, esto significa que no pueden alcanzar el servicio. Esto suele ocurrir porque no están abiertos todos los puertos necesarios. Para ver una lista de los puertos necesarios, consulte la sección Requisitos previos de la documentación sobre la habilitación del proxy de aplicación.

## <a name="upload-certificates-for-custom-domains"></a>Carga de certificados para dominios personalizados

Los dominios personalizados le permiten especificar el dominio de las direcciones URL externas. Para usar dominios personalizados, debe cargar el certificado para ese dominio. Para información sobre cómo usar certificados y dominios personalizados, consulte [Uso de dominios personalizados en el proxy de la aplicación de Azure AD](application-proxy-configure-custom-domain.md).

Si se están produciendo problemas al cargar el certificado, busque los mensajes de error en el portal para más información sobre el problema con el certificado. Algunos problemas habituales con los certificados son:

- Certificado expirado
- Certificado autofirmado
- Certificado que carece de clave privada

El mensaje de error se muestra en la esquina superior derecha cuando se intenta cargar el certificado. También puede seleccionar el icono de notificación para ver los mensajes de error.

## <a name="next-steps"></a>Pasos siguientes

[Publicación de aplicaciones mediante el proxy de aplicación de Azure AD](application-proxy-add-on-premises-application.md)
