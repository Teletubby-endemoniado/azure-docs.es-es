---
title: Traducción de vínculos y direcciones URL con Application Proxy de Azure Active Directory
description: Obtenga información sobre cómo redirigir vínculos codificados de manera rígida de aplicaciones publicadas con Application Proxy de Azure Active Directory.
services: active-directory
author: kenwith
manager: mtillman
ms.service: active-directory
ms.subservice: app-proxy
ms.workload: identity
ms.topic: how-to
ms.date: 04/27/2021
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: 08782f57ecf82924f48e25a1e8f1a62a1a94264b
ms.sourcegitcommit: 1d56a3ff255f1f72c6315a0588422842dbcbe502
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2021
ms.locfileid: "129613840"
---
# <a name="redirect-hard-coded-links-for-apps-published-with-azure-active-directory-application-proxy"></a>Redireccionamiento de vínculos codificados de manera rígida de aplicaciones publicadas con Application Proxy de Azure Active Directory

El Proxy de aplicación de Azure AD permite que las aplicaciones locales estén disponibles para los usuarios remotos o en sus propios dispositivos. Algunas aplicaciones, sin embargo, se desarrollaron con vínculos locales insertados en el código HTML. Estos vínculos no funcionan correctamente si la aplicación se usa de forma remota. Cuando tiene varias aplicaciones locales que se señalan entre sí, sus usuarios esperan que los vínculos sigan funcionando mientras no se encuentran en la oficina. 

La mejor forma de asegurarse de que los vínculos funcionan del mismo modo tanto dentro como fuera de la red corporativa es configurar las direcciones URL externas de sus aplicaciones para que sean iguales que las direcciones URL internas. Use [dominios personalizados](application-proxy-configure-custom-domain.md) para configurar las direcciones URL externas de modo que tengan su nombre de dominio corporativo en lugar del dominio del proxy de aplicación predeterminado.


Si no puede usar dominios personalizados en el inquilino, hay algunas otras opciones para proporcionar esta funcionalidad. Todas ellas también son compatibles con los dominios personalizados y entre sí, de forma que pueda configurar dominios personalizados y otras soluciones si es necesario.

> [!NOTE]
> No se admite la traducción de vínculos para las direcciones URL internas codificadas de forma rígida y generadas a través de JavaScript.

**Opción 1: usar Microsoft Edge**. Esta solución solo es aplicable si tiene previsto recomendar o requerir que los usuarios accedan a la aplicación a través del explorador Microsoft Edge. Controlará todas las URL publicadas. 

**Opción 2: usar la extensión MyApps**. Esta solución requiere que los usuarios instalen una extensión de explorador del lado cliente, pero controlará todas las URL publicadas y funciona con los exploradores más populares. 

**Opción 3: usar el valor de traducción de vínculos**. Se trata de un valor del lado administrador que es invisible para los usuarios. En cambio, administrará las URL solo en HTML y CSS.   

Estas tres características mantendrán sus vínculos en funcionamiento independientemente de dónde estén los usuarios. Cuando tiene aplicaciones que apunta directamente a puertos o puntos de conexión internos, puede asignar estas direcciones URL internas a las direcciones URL del proxy de aplicación externas publicadas. 

 
> [!NOTE]
> La última opción es solo para inquilinos que, por cualquier motivo, no pueden usar dominios personalizados para tener las mismas URL internas y externas para sus aplicaciones. Antes de habilitar esta característica, vea si los [dominios personalizados en el Proxy de aplicación de Azure AD](application-proxy-configure-custom-domain.md) son adecuados para usted. 
> 
> O, en caso de que la aplicación que necesita configurar con la traducción de vínculos sea SharePoint, consulte [Configurar las asignaciones alternativas de acceso en SharePoint 2013](/SharePoint/administration/configure-alternate-access-mappings) para otro enfoque para la asignación de vínculos. 

 
### <a name="option-1-microsoft-edge-integration"></a>Opción 1: Integración con Microsoft Edge 

Puede usar Microsoft Edge para proteger aún más la aplicación y su contenido. Para usar esta solución, debe requerir o recomendar a los usuarios que accedan a la aplicación a través de Microsoft Edge. Microsoft Edge reconocerá todas las URL internas publicadas con Application Proxy y las redirigirá a la URL externa correspondiente. Esto garantiza que todas las URL internas codificadas de forma rígida funcionan y, si un usuario entra en el explorador y escribe directamente la URL interna, funciona incluso si el usuario es remoto.  

Para obtener más información, incluido cómo configurar esta opción, consulte la documentación [Administración del acceso web mediante Microsoft Edge para iOS y Android con Microsoft Intune](/mem/intune/apps/manage-microsoft-edge).  

### <a name="option-2-myapps-browser-extension"></a>Opción 2: Extensión de explorador de MyApps 

La extensión de explorador de MyApps reconoce todas las URL internas publicadas con Application Proxy y las redirige a la URL externa correspondiente. Esto garantiza que todas las URL internas codificadas de forma rígida funcionan y, si un usuario va a la barra de direcciones del explorador y escribe directamente la URL interna, funciona incluso si el usuario es remoto.  

Para usar esta característica, el usuario debe descargar la extensión e iniciar sesión. Los usuarios o administradores no tienen que configurar nada más. 

Para obtener más información, incluido cómo configurar esta opción, vea la documentación de [Extensión de explorador de MyApps](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510#download-and-install-the-my-apps-secure-sign-in-extension).

> [!NOTE]
> La extensión del navegador MyApps no admite la traducción de vínculos de direcciones URL con caracteres comodín.

### <a name="option-3-link-translation-setting"></a>Opción 3: Valor de traducción de vínculos 

Si está habilitada la traducción de vínculos, el servicio Application Proxy busca a través de HTML y CSS los vínculos internos publicados y los traduce de forma que los usuarios reciban una experiencia sin interrupciones. El uso de la extensión de explorador MyApps es preferible a la configuración de la traducción de vínculos, ya que proporciona una experiencia con mejor rendimiento a los usuarios.

> [!NOTE]
> Si está utilizando las opciones 2 o 3, solo debe habilitarse una de ellas a la vez.

## <a name="how-link-translation-works"></a>Funcionamiento de la traducción de vínculos

Después de la autenticación, cuando el servidor proxy pasa los datos de aplicación al usuario, Application Proxy busca vínculos codificados de manera rígida en la aplicación y los reemplaza por sus direcciones URL externas publicadas correspondientes.

El Proxy de aplicación da por supuesto que las aplicaciones están codificadas en UTF-8. Si no es así, especifique el tipo de codificación en un encabezado de respuesta HTTP, como `Content-Type:text/html;charset=utf-8`.

### <a name="which-links-are-affected"></a>¿Qué vínculos se ven afectados?

La característica de traducción de vínculos solo busca los vínculos que estén en las etiquetas de código del cuerpo de una aplicación. El Proxy de aplicación tiene una característica independiente para traducir cookies o direcciones URL en los encabezados. 

Hay dos tipos comunes de vínculos internos en aplicaciones locales:

- **Vínculos internos relativos** que apuntan a un recurso compartido en una estructura de archivos local como `/claims/claims.html`. Estos vínculos funcionan automáticamente en aplicaciones que se publican mediante el Proxy de aplicación y siguen funcionando con o sin traducción de vínculos. 
- **Vínculos internos codificados de manera rígida** a otras aplicaciones locales como `http://expenses`, o archivos publicados como `http://expenses/logo.jpg`. La característica de traducción de vínculos funciona en vínculos internos codificados de manera rígida y los modifica para que apunten a las direcciones URL por las que deben pasar los usuarios remotos.

La lista completa de atributos de las etiquetas de código HTML para las que el proxy de aplicación admite la traducción de vínculos es la siguiente:
* a (href)
* audio (src)
* base (href)
* button (formaction)
* div (data-background, style, data-src)
* embed (src)
* form (action)
* frame (src)
* head (profile)
* html (manifest)
* iframe (longdesc, src)
* img (longdesc, src)
* input (formaction, src, value)
* link (href)
* menuitem (icon)
* meta (content)
* object (archive, data, codebase)
* script (src)
* source (src)
* track (src)
* video (src, poster)

Además, también se traduce el atributo URL en CSS.

### <a name="how-do-apps-link-to-each-other"></a>¿Cómo se vinculan entre sí las aplicaciones?

La traducción de vínculos está habilitada para cada aplicación, para que se tenga control sobre la experiencia del usuario en el nivel por aplicación. Active la traducción de vínculos para una aplicación cuando desee que se traduzcan los vínculos *desde* esa aplicación y no los vínculos *hacia* ella. 

Por ejemplo, imagine que tiene tres aplicaciones publicadas a través del Proxy de aplicación que se vinculan entre sí: Beneficios, Gastos y Viajes. Existe una cuarta aplicación, Comentarios, que no se publica mediante el Proxy de aplicación.

Cuando habilita la traducción de vínculos para la aplicaciones Beneficios, los vínculos a Gastos y Viajes se redirigen a las direcciones URL externas de esas aplicaciones, pero el vínculo a Comentarios no se redirige porque no hay ninguna dirección URL externa. Los vínculos de Gastos y Viajes a Beneficios no funcionan, porque la traducción de vínculos no está habilitada para esas dos aplicaciones.

![Vínculos desde Beneficios a las otras aplicaciones cuando está habilitada la traducción de vínculos](./media/application-proxy-configure-hard-coded-link-translation/one_app.png)

### <a name="which-links-arent-translated"></a>¿Qué vínculos no se traducen?

Para mejorar el rendimiento y la seguridad, no se traducen algunos vínculos:

- Los vínculos que no están dentro de las etiquetas de código. 
- Vínculos que no son de HTML o CSS. 
- Vínculos en formato con código de dirección URL.
- Los vínculos internos que se abren desde otros programas. No se traducirán los vínculos que se envían a través de correos electrónicos o mensajes instantáneos o que se incluyen en otros documentos. Los usuarios deben saber ir a la dirección URL externa.

Si necesita admitir uno de estos dos escenarios, use las mismas direcciones URL internas y externas en lugar de la traducción de vínculos.  

## <a name="enable-link-translation"></a>Habilitación de la traducción de vínculos

Comenzar a trabajar con la traducción de vínculos es tan fácil como hacer clic en un botón:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador.
2. Vaya a **Azure Active Directory** > **Aplicaciones empresariales** > **Todas las aplicaciones** > seleccione la aplicación que desea administrar > **Proxy de aplicación**.
3. Establezca **Translate URLs in application body** (Traducir direcciones URL en el cuerpo de la aplicación) en **Sí**.

   ![Seleccionar Sí para traducir las direcciones URL en el cuerpo de la aplicación](./media/application-proxy-configure-hard-coded-link-translation/select_yes.png)
4. Seleccione **Guardar** para aplicar los cambios.

Ahora, cuando los usuarios accedan a esta aplicación, el proxy examinará automáticamente las direcciones URL que se hayan publicado a través del Proxy de aplicación en el inquilino.

## <a name="next-steps"></a>Pasos siguientes
[Uso de dominios personalizados con el Proxy de aplicación de Azure AD](application-proxy-configure-custom-domain.md) para tener la misma dirección URL interna y externa

[Configurar las asignaciones alternativas de acceso en SharePoint 2013](/SharePoint/administration/configure-alternate-access-mappings)