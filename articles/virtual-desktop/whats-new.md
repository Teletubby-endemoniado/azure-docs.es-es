---
title: Novedades de Azure Virtual Desktop - Azure
description: Nuevas características y actualizaciones de productos para Azure Virtual Desktop.
author: Heidilohr
ms.topic: overview
ms.date: 09/27/2021
ms.author: helohr
ms.reviewer: thhickli; darank
manager: femila
ms.custom: references_regions
ms.openlocfilehash: 1781a566f84825971ac3728a360b52fcd1270e0a
ms.sourcegitcommit: 362359c2a00a6827353395416aae9db492005613
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2021
ms.locfileid: "132490238"
---
# <a name="whats-new-in-azure-virtual-desktop"></a>Novedades de Azure Virtual Desktop

Azure Virtual Desktop se actualiza periódicamente. En este artículo encontrará información sobre:

- Las actualizaciones más recientes
- Nuevas características
- Mejoras de las características existentes
- Corrección de errores

Este artículo se actualiza mensualmente. Asegúrese de volver aquí con frecuencia para mantenerse al día con las nuevas actualizaciones.

## <a name="client-updates"></a>Actualizaciones de clientes

Consulte estos artículos para obtener información acerca de las actualizaciones de nuestros clientes para Azure Virtual Desktop y Servicios de Escritorio remoto:

- [Windows](/windows-server/remote/remote-desktop-services/clients/windowsdesktop-whatsnew)
- [macOS](/windows-server/remote/remote-desktop-services/clients/mac-whatsnew)
- [iOS](/windows-server/remote/remote-desktop-services/clients/ios-whatsnew)
- [Android](/windows-server/remote/remote-desktop-services/clients/android-whatsnew)
- [Web](/windows-server/remote/remote-desktop-services/clients/web-client-whatsnew)

## <a name="azure-virtual-desktop-agent-updates"></a>Actualizaciones del agente de Azure Virtual Desktop

El agente de Azure Virtual Desktop se actualiza al menos una vez al mes.

Estos son los cambios que se han realizado en el agente Azure Virtual Desktop:

- Versión 1.0.3719.1700: esta actualización se publicó en noviembre de 2021 e incorpora los siguientes cambios:
    - Los mensajes de error del agente se han actualizado.
    - Corrige un problema que provocaba que el agente se reiniciara cada vez que se actualizaba la pila en paralelo.
    - Mejoras generales del agente.
- Versión 1.0.3583.2600: esta actualización se publicó en octubre de 2021 y corrige un problema por el que la actualización de Windows 10 a Windows 11 deshabilitaba la pila en paralelo.
- Versión 1.0.3373.2605: esta actualización se publicó en septiembre de 2021 y corrige un problema que causaba que la anulación del registro de paquetes se bloqueara al usar la asociación de aplicaciones en formato MSIX.
- Versión 1.0.3373.2600: esta actualización se publicó en septiembre de 2021 e incorpora los siguientes cambios:
    - Mejoras generales del agente.
    - Corrige problemas con el reinicio del agente en máquinas virtuales con Windows 7.
    - Corrige un problema por el que los campos de la tabla WVDAgentHealthStatus no se muestran correctamente.
- Versión 1.0.3130.2900: esta actualización se publicó en julio de 2021 e incorpora los siguientes cambios:
    - Mejoras y correcciones de errores generales.
    - Corrige un problema con la obtención de la ruta de acceso de grupo de hosts para el registro de Intune.
    - Se ha agregado el registro para diagnosticar mejor los problemas del agente.
    - Corrige un problema con los tiempos de espera de orquestación.
- Versión 1.0.3050.2500: esta actualización se publicó en julio de 2021 e incorpora los siguientes cambios:
    - Monitores internos actualizados para el estado del agente.
    - Se ha actualizado la lógica de reintento para el estado de la pila.
- Versión 1.0.2990.1500: esta actualización se publicó en abril de 2021 e incorpora los siguientes cambios:
    - Los mensajes de error del agente se han actualizado.
    - Se ha agregado una excepción que impide instalar agentes que no son de Windows 7 en máquinas virtuales Windows 7.
    - Se ha actualizado la lógica del servicio de latido.
- Versión 1.0.2944.1400: esta actualización se publicó en abril de 2021 e incorpora los siguientes cambios:
    - Se han incluido vínculos a la guía de solución de problemas del Agente de Azure Virtual Desktop en los registros del visor de eventos para los errores del agente.
    - Se ha agregado una nueva excepción para tener un mejor control de los errores.
    - Se ha agregado WVDAgentUrlTool.exe, que permite a los clientes comprobar las direcciones URL necesarias a las pueden acceder.
-   Versión 1.0.2866.1500: esta actualización se publicó en marzo de 2021 y corrige un problema relacionado con la comprobación del estado de la pila.
-   Versión 1.0.2800.2802: esta actualización se publicó en marzo de 2021 e incorpora correcciones de errores y mejoras generales.
-   Versión 1.0.2800.2800: esta actualización se publicó en marzo de 2021 y corrige un problema de conexión inversa.
-   Versión 1.0.2800.2700: esta actualización se publicó en febrero de 2021 y corrige un problema de orquestación de acceso denegado.

## <a name="fslogix-updates"></a>Actualizaciones de FSLogix

¿Desea conocer las últimas actualizaciones de FSLogix? Consulte las [novedades de FSLogix](/fslogix/whats-new).

## <a name="october-2021"></a>Octubre de 2021

Estos son los cambios que han tenido lugar en octubre de 2021:

### <a name="azure-virtual-desktop-support-for-windows-11"></a>Compatibilidad de Azure Virtual Desktop con Windows 11

La compatibilidad de Azure Virtual Desktop con Windows 11 ahora está disponible con carácter general para implementaciones de una y varias sesiones. Ya puede usar imágenes de Windows 11 al crear grupos host en Azure Portal. Para obtener más información, consulte [nuestra entrada de blog](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/windows-11-is-now-generally-available-on-azure-virtual-desktop/ba-p/2810545).

### <a name="rdp-shortpath-now-generally-available"></a>RDP Shortpath ya disponible con carácter general

Remote Desktop Protocol (RDP) Shortpath para redes administradas ya está disponible con carácter general. RDP Shortpath establece una conexión directa entre el cliente de Escritorio remoto y el host de sesión. Esta conexión directa reduce la dependencia de las puertas de enlace, mejora la confiabilidad de la conexión y aumenta el ancho de banda disponible para cada sesión de usuario. Para obtener más información, consulte [nuestra entrada de blog](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/rdp-shortpath-for-managed-networks-is-generally-available/m-p/2861468).

### <a name="screen-capture-protection-updates"></a>Actualizaciones de protección de capturas de pantalla

La protección de capturas de pantalla ahora se admite en el cliente macOS y en las nubes de Azure Government y Azure China. Para obtener más información, consulte [nuestra entrada de blog](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/screen-capture-protection-for-macos-client-and-support-for/m-p/2840089#M7940).

### <a name="azure-active-directory-domain-join"></a>Unión a un dominio de Azure Active Directory 

La unión a un dominio de Azure Active Directory para máquinas virtuales de Azure Virtual Desktop ahora está disponible en las nubes de Azure Government y Azure China. Microsoft Endpoint Manager (Intune) solo se admite actualmente en la nube pública de Azure. Puede encontrar más información en [Implementación de máquinas virtuales unidas a Azure AD en Azure Virtual Desktop](deploy-azure-ad-joined-vm.md).

### <a name="breaking-change-in-azure-virtual-desktop-azure-resource-manager-template"></a>Cambio importante en la plantilla de Azure Resource Manager para Azure Virtual Desktop

Se ha introducido un cambio importante en la plantilla de Azure Resource Manager para Azure Virtual Desktop. Si va a usar código que dependa del cambio, deberá seguir las instrucciones de [nuestra entrada de blog](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/azure-virtual-desktop-arm-template-change-removal-of-script/m-p/2851538#M7971) para solucionar el problema.

### <a name="autoscale-preview-public-preview"></a>Versión preliminar pública de la escalabilidad automática

La escalabilidad automática para Azure Virtual Desktop está ahora en versión preliminar pública. Esta característica activa o desactiva de forma nativa las máquinas virtuales (VM) de los grupos de host agrupados en función de las necesidades de disponibilidad. Al programar cuándo las máquinas virtuales se activan y desactivan, se optimizan los costos de implementación. Además, esta característica también ofrece opciones de programación flexibles en función de sus necesidades. Una vez que haya configurado el rol de control de acceso basado en rol (RBAC) personalizado necesario, puede empezar a configurar el plan de escalado. Para más información, consulte [Escalabilidad automática (versión preliminar) para grupos de hosts de Azure Virtual Desktop](autoscale-scaling-plan.md).

## <a name="september-2021"></a>Septiembre de 2021

Estos son los cambios que se han producido en septiembre de 2021.

### <a name="azure-portal-updates"></a>Actualizaciones de Azure Portal

Ahora puede usar plantillas de Azure Resource Manager para cualquier actualización que quiera aplicar a los hosts de sesión después de la implementación. Para acceder a esta característica, seleccione la pestaña **Máquinas virtuales** al crear un grupo de hosts.

Ahora también puede establecer la configuración de diagnóstico del grupo de host, el grupo de aplicaciones y el área de trabajo al crear grupos de hosts en lugar de después. Al configurar estas opciones durante el proceso de creación del grupo de hosts, también se configuran automáticamente los datos de informes para Azure Virtual Desktop Insights.

### <a name="azure-active-directory-domain-join"></a>Unión a un dominio de Azure Active Directory

La unión a un dominio de Azure Active Directory ya está disponible con carácter general. Este servicio le permite unir los hosts de sesión a Azure Active Directory. La unión a un dominio también le permite realizar la inscripción automática en Intune como parte de Microsoft Endpoint Manager. Puede acceder a esta característica en la nube pública de Azure, pero no en la nube de Azure Government ni Azure China. Para obtener más información, consulte [nuestra entrada de blog](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/announcing-general-availability-of-azure-ad-joined-vms-support/ba-p/2751083).

### <a name="azure-china"></a>Azure China

Azure Virtual Desktop ya está disponible con carácter general en la nube de Azure China. Para obtener más información, consulte [nuestra entrada de blog](https://azure.microsoft.com/updates/general-availability-azure-virtual-desktop-is-now-available-in-the-azure-china-cloud/).

### <a name="automatic-migration-module-tool"></a>Herramienta del módulo de migración automática

Con la herramienta de migración automática, puede mover la organización de Azure Virtual Desktop (clásico) a Azure Virtual Desktop con solo unos pocos comandos de PowerShell. Esta característica se encuentra actualmente en versión preliminar pública y puede encontrar más información en [Migración automática](automatic-migration.md).

## <a name="august-2021"></a>Agosto de 2021

Estos son los cambios que se han producido en agosto de 2021:

### <a name="windows-11-preview-for-azure-virtual-desktop"></a>Windows 11 (versión preliminar) para Azure Virtual Desktop

Las imágenes de Windows 11 (versión preliminar) ya están disponibles en Azure Marketplace para que los clientes las prueben y validen con Azure Virtual Desktop. Para obtener más información, consulte [nuestro anuncio](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/windows-11-preview-is-now-available-on-azure-virtual-desktop/ba-p/2666468).

### <a name="multimedia-redirection-is-now-in-public-preview"></a>El redireccionamiento multimedia ahora está en versión preliminar pública.

El redireccionamiento multimedia proporciona una reproducción de vídeo fluida mientras ve vídeos en el explorador web de Azure Virtual Desktop y funciona con Microsoft Edge y Google Chrome. En [nuestra entrada de blog](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/public-preview-announcing-public-preview-of-multimedia/m-p/2663244#M7692) encontrará más información.

### <a name="windows-defender-application-control-and-azure-disk-encryption-support"></a>Compatibilidad con el control de aplicaciones de Windows Defender y Azure Disk Encryption

Azure Virtual Desktop ahora admite el control de aplicaciones de Windows Defender para controlar qué controladores y aplicaciones pueden ejecutarse en las máquinas virtuales Windows y Azure Disk Encryption usa Windows BitLocker para proporcionar cifrado de volumen para los discos del sistema operativo y los discos de datos de las máquinas virtuales. Para obtener más información, consulte [nuestro anuncio](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/support-for-windows-defender-application-control-and-azure-disk/m-p/2658633#M7685).
 
### <a name="signing-into-azure-ad-using-smart-cards-are-now-supported-in-azure-virtual-desktop"></a>El inicio de sesión en Azure AD con tarjetas inteligentes ya se admite en Azure Virtual Desktop

Aunque esta no es una característica nueva para Azure AD, ahora Azure Virtual Desktop admite la configuración de los Servicios de federación de Active Directory (AD FS) para iniciar sesión con tarjetas inteligentes. Para obtener más información, consulte [nuestro anuncio](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/signing-in-to-azure-ad-using-smart-cards-now-supported-in-azure/m-p/2654209#M7671).

### <a name="screen-capture-protection-is-now-generally-available"></a>La protección de captura de pantalla ya está disponible con carácter general

Evite que el software que se ejecuta en los puntos de conexión de cliente capture información confidencial con la protección de captura de pantalla de Azure Virtual Desktop. En [nuestra entrada de blog](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/announcing-general-availability-of-screen-capture-protection-for/m-p/2699684) encontrará más información.

## <a name="july-2021"></a>Julio de 2021

A continuación, se incluyen los cambios de julio de 2021:

### <a name="azure-virtual-desktop-images-now-include-optimized-teams"></a>Las imágenes de Azure Virtual Desktop ahora incluyen Teams optimizado

Todas las imágenes disponibles en la galería de imágenes de Azure Virtual Desktop que incluyen Aplicaciones de Microsoft 365 para Enterprise ahora tienen la versión optimizada para medios de Teams para Azure Virtual Desktop preinstalada. Para obtener más información, consulte [nuestro anuncio](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/media-optimization-for-microsoft-teams-now-part-of-win10/m-p/2550054#M7442).

### <a name="azure-active-directory-domain-join-for-session-hosts-is-in-public-preview"></a>La unión a un dominio de Azure Active Directory para hosts de sesión está en versión preliminar pública

Ahora puede unir las máquinas virtuales (VM) de Azure Virtual Desktop directamente a Azure Active Directory (Azure AD). Esta característica le permite conectarse a las máquinas virtuales desde cualquier dispositivo con credenciales básicas. También puede inscribir automáticamente las máquinas virtuales con Microsoft Endpoint Manager. En determinados escenarios, esto ayudará a eliminar la necesidad de un controlador de dominio, reducir los costos y simplificar la implementación. Obtenga más información sobre la [Implementación de máquinas virtuales unidas a Azure AD en Azure Virtual Desktop](deploy-azure-ad-joined-vm.md).

### <a name="fslogix-version-2105-is-now-available"></a>La versión 2105 de FSLogix ya está disponible

La versión 2105 de FSLogix ya está disponible con carácter general. Esta versión incluye tiempos de inicio de sesión mejorados y correcciones de errores que no estaban disponibles en la versión preliminar pública (versión 2105). Para obtener información más detallada, puede ver las [notas de la versión de FSLogix](/fslogix/whats-new) y [nuestra entrada de blog](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/announcing-general-availability-of-fslogix-2105-2-9-7838-44263/m-p/2539491#M7412).

### <a name="azure-virtual-desktop-in-china-has-entered-public-preview"></a>Azure Virtual Desktop en China ha entrado en versión preliminar pública

Con Azure Virtual Desktop disponible en China, ahora tenemos una cobertura global más desarrollada que ayuda a las organizaciones a respaldar a los clientes de esta región con un rendimiento y una latencia mejorados. Obtenga más información en [nuestra página de anuncios](https://azure.microsoft.com/updates/azure-virtual-desktop-is-now-available-in-the-azure-china-cloud-in-preview/).
 
### <a name="the-getting-started-feature-for-azure-virtual-desktop"></a>Característica de Introducción para Azure Virtual Desktop

Esta característica ofrece una experiencia de incorporación simplificada en Azure Portal para configurar el entorno de Azure Virtual Desktop. Puede usar esta característica para crear implementaciones que cumplan los requisitos del sistema para Azure Active Directory Domain Services automatizado de una manera sencilla. Para más información, consulte [nuestra entrada en el blog](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/getting-started-wizard-in-azure-virtual-desktop/m-p/2451385).

### <a name="start-vm-on-connect-is-now-generally-available"></a>El inicio de la máquina virtual al conectarse ya está disponible con carácter general

La característica de inicio de la máquina virtual al conectarse ya está disponible con carácter general. Esta característica le ayuda a optimizar los costos al permitirle desactivar las máquinas virtuales desasignadas o detenidas, lo que permite que la implementación sea flexible con las demandas de los usuarios. Para más información, consulte [Inicio de la máquina virtual al conectarse](start-virtual-machine-connect.md).

### <a name="remote-app-streaming-documentation"></a>Documentación de transmisión de aplicaciones remotas

Recientemente hemos anunciado una nueva opción de precios para la transmisión de aplicaciones remotas para usar Azure Virtual Desktop para ofrecer aplicaciones como servicio a sus clientes y asociados empresariales. Por ejemplo, los proveedores de software pueden usar la transmisión de aplicaciones remotas para ofrecer aplicaciones como una solución de software como servicio (SaaS) a la que pueden acceder sus clientes. Para obtener más información sobre la transmisión de aplicaciones remotas, consulte [nuestra documentación](./remote-app-streaming/overview.md).

Desde el 14 de julio de 2021 hasta el 31 de diciembre de 2021, proporcionamos a los clientes que usan la transmisión de aplicaciones remotas una oferta promocional que permite a sus asociados comerciales y clientes acceder a Azure Virtual Desktop sin cargo alguno. Esta oferta solo se aplica a los derechos de acceso de usuarios externos. La facturación normal se reanudará el 1 de enero de 2022. Mientras tanto, puede seguir usando los derechos de licencia de Windows existentes que se encuentran en licencias como Microsoft 365 E3 o Windows E3. Para más información sobre esta oferta, consulte la [página de precios de Azure Virtual Desktop](https://azure.microsoft.com/pricing/details/virtual-desktop/).

### <a name="new-azure-virtual-desktop-handbooks"></a>Nuevos manuales de Azure Virtual Desktop

Recientemente hemos publicado cuatro nuevos manuales para ayudarle a diseñar e implementar Azure Virtual Desktop en diferentes escenarios: 

- [Administración de aplicaciones](https://azure.microsoft.com/resources/azure-virtual-desktop-handbook-application-management/) le mostrará cómo modernizar la entrega de aplicaciones y simplificar la administración de TI.  
- En [Recuperación ante desastres](https://azure.microsoft.com/resources/azure-virtual-desktop-handbook-disaster-recovery/), aprenda a reforzar la resistencia empresarial mediante el desarrollo de una estrategia de recuperación ante desastres.  
- Obtenga más valor de las inversiones de Citrix con la guía de migración de [Citrix Cloud con Azure Virtual Desktop](https://azure.microsoft.com/resources/migration-guide-citrix-cloud-with-azure-virtual-desktop/).
- Obtenga más valor de las inversiones de VMware existentes con la guía de migración de [VMware Horizon con Azure Virtual Desktop](https://azure.microsoft.com/resources/migration-guide-vmware-horizon-cloud-and-azure-virtual-desktop/).

## <a name="june-2021"></a>Junio de 2021

A continuación, se incluyen los cambios de junio de 2021:

### <a name="windows-virtual-desktop-is-now-azure-virtual-desktop"></a>Windows Virtual Desktop ahora es Azure Virtual Desktop

Para un mejor alineamiento con nuestra visión de una plataforma de aplicaciones remotas y de escritorio en la nube flexible, hemos cambiado el nombre Windows Virtual Desktop a Azure Virtual Desktop. Obtenga más información en [la publicación de anuncio de nuestro blog](https://azure.microsoft.com/blog/azure-virtual-desktop-the-desktop-and-app-virtualization-platform-for-the-hybrid-workplace/).

### <a name="eu-uk-and-canada-geographies-are-now-generally-available"></a>Las zonas geográficas de Europa, Reino Unido y Canadá ya están disponibles con carácter general

El servicio de metadatos de la Unión Europea, Reino Unido y Canadá se encuentra actualmente disponible con carácter general. Estas nuevas ubicaciones son muy importantes para la soberanía de los datos fuera de EE. UU. Para obtener más información, consulte [nuestra entrada de blog](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/announcing-public-preview-of-azure-virtual-desktop-service/m-p/2478401#M7314).

### <a name="the-getting-started-tool-is-now-in-public-preview"></a>La herramienta Introducción está ahora en versión preliminar pública

Hemos creado la herramienta Introducción de Azure Virtual Desktop para facilitar el proceso de implementación a los nuevos usuarios. Al simplificar y automatizar el proceso de implementación, esperamos que esta herramienta ayude a que la adopción de Azure Virtual Desktop sea más rápida y accesible para una amplia variedad de usuarios. En [nuestra entrada de blog](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/getting-started-wizard-in-azure-virtual-desktop/m-p/2451385) encontrará más información.

### <a name="azure-virtual-desktop-pricing-calculator-updates"></a>Actualizaciones de la calculadora de precios de Azure Virtual Desktop

Hemos realizado algunas actualizaciones importantes para mejorar la experiencia de precios de Azure Virtual Desktop en la calculadora de precios de Azure, incluidas las siguientes:  
  
- Hemos actualizado el nombre del servicio a Azure Virtual Desktop  
- También hemos actualizado el diseño con los siguientes elementos nuevos:  
   - Una sección Storage con disco administrado y ancho de banda de almacenamiento de archivos  
   - Una sección personalizada que muestra el costo por usuario

Puede acceder a la calculadora de precios en [esta página](https://azure.microsoft.com/pricing/calculator/).

### <a name="single-sign-on-sso-using-active-directory-federation-services-ad-fs"></a>Inicio de sesión único (SSO) mediante Servicios de federación de Active Directory (AD FS)

La característica de inicio de sesión único de AD FS ya está disponible con carácter general. Esta característica permite a los clientes usar AD FS para proporcionar una experiencia de inicio de sesión único en Windows y clientes web. Para más información, consulte [Configuración del inicio de sesión único de AD FS para Azure Virtual Desktop](configure-adfs-sso.md).

## <a name="may-2021"></a>Mayo de 2021

Estas son las novedades de mayo de 2021:

### <a name="smart-card-authentication"></a>Autenticación con tarjeta inteligente

Ahora hemos publicado oficialmente las propiedades de Protocolo de escritorio remoto (RDP) del Centro de distribución de claves (KDC). Estas propiedades habilitan la autenticación Kerberos para la parte de RDP de una sesión de Azure Virtual Desktop, lo que incluye permitir la autenticación a nivel de red sin contraseña. En [nuestra entrada de blog](https://techcommunity.microsoft.com/t5/windows-virtual-desktop/new-feature-smart-card-authentication-for-windows-virtual/m-p/2323226) encontrará más información.

### <a name="the-web-client-now-supports-file-transfer"></a>El cliente web ahora admite la transferencia de archivos.

A partir de la versión preliminar pública del cliente web, versión 1.0.24.7 (versión preliminar), los usuarios ahora pueden transferir archivos entre su sesión remota y el equipo local. Para cargar archivos en la sesión remota, seleccione el icono de carga en el menú de la parte superior de la página del cliente web. Para descargar archivos, busque la **unidad virtual de Escritorio remoto** en el menú Inicio de la sesión remota. Después de abrir la unidad virtual, solo tiene que arrastrar y colocar los archivos en la carpeta Descargas y el explorador comenzará a descargar los archivos en el equipo local.

### <a name="start-vm-on-connect-support-updates"></a>Actualizaciones de Iniciar VM al establecer la conexión

Iniciar VM al establecer la conexión (versión preliminar) ahora admite grupos de hosts agrupados y Azure Government Cloud. Para más información, lea nuestra [entrada de blog](https://techcommunity.microsoft.com/t5/windows-virtual-desktop/leverage-start-vm-on-connect-for-pooled-host-pools-and-azure-gov/m-p/2349866).

### <a name="latency-improvements-for-the-united-arab-emirates-region"></a>Mejoras de latencia para la región de Emiratos Árabes Unidos

Hemos ampliado la presencia del plano de control de Azure a los Emiratos Árabes Unidos (Emiratos Árabes Unidos), por lo que los clientes de esa región ahora pueden experimentar una latencia mejorada. Obtenga más información en nuestro [mapa de ruta de Azure Virtual Desktop](https://www.microsoft.com/microsoft-365/roadmap?filters=Windows%20Virtual%20Desktop&searchterms=64545).

### <a name="ending-internet-explorer-11-support"></a>Finalización de la compatibilidad con Internet Explorer 11

El 30 de septiembre de 2021, el cliente web de Azure Virtual Desktop ya no admitirá Internet Explorer 11. Se recomienda empezar a usar el explorador [Microsoft Edge](https://www.microsoft.com/edge?form=MY01R2&OCID=MY01R2&r=1) para el cliente web y las sesiones remotas en su lugar. Para obtener más información, consulte el anuncio de esta [entrada de blog](https://techcommunity.microsoft.com/t5/windows-virtual-desktop/windows-virtual-desktop-web-client-to-end-support-for-internet/m-p/2369007).

### <a name="microsoft-endpoint-manager-public-preview"></a>Versión preliminar pública de Microsoft Endpoint Manager

Hemos iniciado la versión preliminar pública de la compatibilidad de Microsoft Endpoint Manager en sesión múltiple de Windows 10 Enterprise. Esta nueva característica le permitirá administrar las máquinas virtuales de Windows 10 con las mismas herramientas que los dispositivos locales. Obtenga más información en la [documentación de Microsoft Endpoint Manager](/mem/intune/fundamentals/windows-virtual-desktop-multi-session).

### <a name="fslogix-agent-public-preview"></a>Versión preliminar pública del agente de FSLogix

Hemos publicado una versión preliminar pública de la versión más reciente del agente de FSLogix. Consulte nuestra [entrada de blog](https://techcommunity.microsoft.com/t5/windows-virtual-desktop/public-preview-fslogix-release-2105-is-now-available-in-public/m-p/2380996/thread-id/7105) para obtener más información y enviar el formulario que necesitará para acceder a la versión preliminar.

### <a name="may-2021-updates-for-teams-for-azure-virtual-desktop"></a>Actualizaciones de mayo de 2021 para Teams para Azure Virtual Desktop

Para esta actualización, se ha resuelto un problema que provocaba que la pantalla se quedara negra al compartir vídeo. También se ha corregido un error de coincidencia en las resoluciones de vídeo entre el cliente de la sesión y el servidor de Teams. Teams en Azure Virtual Desktop ahora debe cambiar la resolución y las velocidades de bits en función de la entrada del servidor de Teams.

### <a name="azure-portal-deployment-updates"></a>Actualizaciones de la implementación de Azure Portal

Hemos realizado las siguientes actualizaciones en el proceso de implementación en Azure Portal:

- Se han agregado nuevas imágenes (incluida GEN2) al cuadro de lista desplegable de "imagen" al crear una nueva máquina virtual de host de sesión de Azure Virtual Desktop.
- Ahora puede configurar diagnósticos de arranque para máquinas virtuales al crear un grupo de hosts.
- Se ha agregado una sugerencia de herramientas al proxy de RDP en la pestaña de propiedades de RDP del grupo de hosts avanzado.
- Se ha agregado una burbuja de información para la ruta de acceso del icono al agregar una aplicación desde un paquete MSIX.
- Ya no puede realizar diagnósticos de arranque administrados con un disco no administrado.
- Se ha actualizado la plantilla para crear un grupo de hosts en Azure Resource Manager para que Azure Portal ahora pueda admitir la creación de grupos de hosts con imágenes de Marketplace de terceros.

### <a name="single-sign-on-using-active-directory-federation-services-public-preview"></a>Inicio de sesión único con la versión preliminar pública de Servicios de federación de Active Directory (AD FS)

Hemos iniciado una versión preliminar pública para la compatibilidad de Servicios de federación de Active Directory (AD FS) (AD FS) con el inicio de sesión único (SSO) por grupo de hosts. Obtenga más información en [Configuración del inicio de sesión único de AD FS para Azure Virtual Desktop](configure-adfs-sso.md). 

### <a name="enterprise-scale-support"></a>Compatibilidad a escala empresarial

Hemos publicado una sección actualizada de Cloud Adoption Framework para la compatibilidad a escala empresarial con Azure Virtual Desktop. Para más información, consulte [Compatibilidad a escala empresarial con el conjunto de construcción de Azure Virtual Desktop](/azure/cloud-adoption-framework/scenarios/wvd/enterprise-scale-landing-zone).

### <a name="customer-adoption-kit"></a>Kit de adopción del cliente

Recientemente hemos publicado el kit de adopción del cliente de Azure Virtual Desktop para ayudar a los clientes y asociados a configurar Azure Virtual Desktop para sus clientes. Puede descargar el kit [aquí](https://www.microsoft.com/azure/partners/resources/customer-adoption-kit-windows-virtual-desktop).

## <a name="april-2021"></a>Abril de 2021

Novedades de abril:

### <a name="use-the-start-vm-on-connect-feature-preview-in-the-azure-portal"></a>Use la característica Iniciar máquina virtual al conectar (versión preliminar) en Azure Portal

Ya puede configurar Iniciar máquina virtual al conectar (versión preliminar) en Azure Portal. Con esta actualización, los usuarios pueden acceder a sus máquinas virtuales desde los clientes Android y macOS. Para más información, consulte la sección sobre [Iniciar máquina virtual al conectar](start-virtual-machine-connect.md#use-the-azure-portal).

### <a name="required-url-check-tool"></a>Herramienta de comprobación de direcciones URL requeridas 

El agente de Azure Virtual Desktop, versión 1.0.2944.400, incluye una herramienta que valida las direcciones URL y muestra si la máquina virtual puede acceder a las direcciones URL que necesita para funcionar. Si se puede acceder a las direcciones URL necesarias, la herramienta las enumerará para que pueda desbloquearlas, en caso de que sea necesario. Obtenga más información en nuestra [lista de direcciones URL seguras](safe-url-list.md#required-url-check-tool).

### <a name="updates-to-the-azure-portal-ui-for-azure-virtual-desktop"></a>Cambios en la interfaz de usuario de Azure Portal para Azure Virtual Desktop

Esto es lo que ha cambiado en la última actualización de la interfaz de usuario de Azure Portal para Azure Virtual Desktop:

- Se ha corregido un problema que provocaba que apareciera un error al recuperar el host de sesión mientras el modo de purga está habilitado.
- Se ha actualizado el SDK del portal a la versión 7.161.0.
- Se ha corregido un problema que provocaba que el identificador de recurso que faltaba aparezca en la pestaña Sesiones de usuario.
- Azure Portal ahora muestra mensajes detallados de subestado para los hosts de sesión.

### <a name="april-2021-updates-for-teams-on-azure-virtual-desktop"></a>Actualizaciones de abril de 2021 para Teams en Azure Virtual Desktop

Estas son las novedades de Teams en Azure Virtual Desktop:

- Se ha agregado aceleración de hardware para el procesamiento de vídeo de secuencias de vídeo salientes para clientes basados en Windows 10.
- Al unirse a una reunión con una cámara frontal y una cámara trasera o externa, la cámara frontal será la que se seleccionará de forma predeterminada.
- Se ha resuelto un problema que hacía que Teams se bloqueara en máquinas basadas en x86.
- Se ha resuelto un problema que provocaba estrías mientras se compartía la pantalla.
- Se ha resuelto un problema que impedía a los miembros de la reunión ver el vídeo entrante o compartir pantalla.

### <a name="msix-app-attach-is-now-generally-available"></a>La asociación de aplicaciones en formato MSIX ya está disponible de forma general.

La asociación de aplicaciones en formato MSIX para Azure Virtual Desktop ha salido de la versión preliminar pública y está disponible para todos los usuarios. Obtenga más información sobre la asociación de aplicaciones en formato MSIX en [nuestro anuncio de TechCommunity](https://techcommunity.microsoft.com/t5/windows-virtual-desktop/msix-app-attach-is-now-generally-available/m-p/2270468).

### <a name="the-macos-client-now-supports-apple-silicon-and-big-sur"></a>El cliente macOS ahora admite Apple Silicon y Big Sur.

El cliente macOS de Azure Virtual Desktop ahora admite Apple Silicon y Big Sur. La lista completa de actualizaciones está disponible en [Novedades del cliente de macOS](/windows-server/remote/remote-desktop-services/clients/mac-whatsnew).

## <a name="march-2021"></a>Marzo de 2021

A continuación, se incluyen los cambios de marzo de 2021.

### <a name="updates-to-the-azure-portal-ui-for-azure-virtual-desktop"></a>Cambios en la interfaz de usuario de Azure Portal para Azure Virtual Desktop

Hemos efectuado los siguientes cambios en Azure Virtual Desktop para Azure Portal:

- Hemos habilitado nuevas opciones de disponibilidad (zonas y conjunto de disponibilidad) para que los flujos de trabajo creen grupos host y agreguen máquinas virtuales.
- Hemos corregido un problema por el que un host cuyo estado indica que "necesita asistencia" aparecía como no disponible. Ahora aparecerá un icono de advertencia junto al host.
- Hemos habilitado la ordenación de sesiones activas.
- Ahora es posible enviar mensajes a usuarios específicos o cerrar la sesión en la pestaña de detalles del host.
- Hemos cambiado el campo de límite máximo de sesión.
- Hemos agregado una ruta de validación de unidades organizativas (UO) al flujo de trabajo para crear un grupo host.
- Ahora se puede usar la versión más reciente de la imagen Windows 10 al crear un grupo host personal.

### <a name="generation-2-images-and-trusted-launch"></a>Imágenes de generación 2 e inicio seguro

Azure Marketplace ahora tiene imágenes de generación 2 para Windows 10 Enterprise y la sesión múltiple de Windows 10 Enterprise. Estas imágenes le permitirán usar máquinas virtuales con inicio seguro. Obtenga más información sobre las máquinas virtuales de generación 2 en [¿Debería crear una máquina virtual de generación 1 o 2?](../virtual-machines/generation-2.md) Para obtener información sobre el aprovisionamiento de máquinas virtuales de con inicio seguro de Azure Virtual Desktop, consulte [nuestra publicación de TechCommunity](https://techcommunity.microsoft.com/t5/windows-virtual-desktop/windows-virtual-desktop-support-for-trusted-launch/m-p/2206170).

### <a name="fslogix-is-now-preinstalled-on-windows-10-enterprise-multi-session-images"></a>FSLogix se ha preinstalado en imágenes multisesión de Windows 10 Enterprise

Teniendo en cuenta los comentarios de los clientes, hemos incorporado una nueva versión de la imagen multisesión de Windows 10 Enterprise que incluye una versión preinstalada y sin configurar de FSLogix. De este modo, esperamos que la implementación de Azure Virtual Desktop sea más sencilla.

### <a name="azure-monitor-for-azure-virtual-desktop-is-now-in-general-availability"></a>Azure Monitor para Azure Virtual Desktop se encuentra ahora disponible con carácter general

Azure Monitor para Azure Virtual Desktop se encuentra ahora disponible con carácter general para el público. Esta característica es un servicio automatizado que supervisa las implementaciones y permite ver eventos y sugerencias de solución de problemas y mantenimiento en un mismo lugar. Para obtener más información, consulte [nuestra documentación](azure-monitor.md) o la [publicación de TechCommunity](https://techcommunity.microsoft.com/t5/windows-virtual-desktop/azure-monitor-for-windows-virtual-desktop-is-generally-available/m-p/2242861).

### <a name="march-2021-updates-for-teams-on-azure-virtual-desktop"></a>Actualizaciones de marzo de 2021 para Teams en Azure Virtual Desktop

Hemos efectuado los siguientes cambios para Teams en Azure Virtual Desktop:

- Hemos mejorado la calidad de vídeo para las llamadas y el modo 2x2.
- Hemos reducido el uso de CPU entre un 5 % y un 10 % (dependiendo de la generación de CPU) mediante la descarga de hardware del procesamiento de vídeo (XVP).
- Las máquinas más antiguas ahora pueden usar XVP y la decodificación de hardware para mostrar eficazmente más secuencias de vídeo entrantes en el modo 2x2.
- Hemos actualizado la pila de WebRTC de M74 a M88 para mejorar la sincronización de AV y reducir los problemas de transición.
- Hemos reemplazado nuestro codificador de software H264 por OpenH264 (OSS utilizado por Teams en Internet), que ha aumentado la calidad del vídeo para la cámara de salida.
- Hemos habilitado el modo 2x2 para Teams Server para el público general desde el 30 de marzo. El modo 2x2 muestra hasta cuatro secuencias de vídeo entrantes al mismo tiempo.

### <a name="start-vm-on-connect-public-preview"></a>Iniciar VM al establecer la conexión (versión preliminar pública)

La nueva configuración del grupo host, Iniciar VM al establecer la conexión, ya está disponible en versión preliminar pública. Esta configuración permite activar las máquinas virtuales siempre que las necesite. Si desea ahorrar costos, desasigne las máquinas virtuales mediante la configuración de Azure Compute. Para más información, consulte [nuestra entrada de blog](https://aka.ms/wvdstartvmonconnect) y la [documentación](start-virtual-machine-connect.md).

### <a name="azure-virtual-desktop-specialty-certification"></a>Certificación especializada de Azure Virtual Desktop

Hemos publicado una versión beta del examen AZ-140 que le permitirá acreditar sus conocimientos sobre Azure Virtual Desktop en Azure. Para más información, consulte [nuestra publicación de TechCommunity](https://techcommunity.microsoft.com/t5/microsoft-learn-blog/beta-exam-prove-your-expertise-in-windows-virtual-desktop-on/ba-p/2147107).

## <a name="february-2021"></a>Febrero de 2021

Estos son los cambios que han tenido lugar en febrero de 2021.

### <a name="portal-experience"></a>Experiencia del portal

Hemos mejorado la experiencia de Azure Portal de las siguientes maneras:

- Modo de purga masiva en hosts en la pestaña de cuadrícula del host de sesión. 
- La asociación de aplicaciones en formato MSIX está ahora disponible en versión preliminar pública.
- Información general del grupo de hosts fijo para el modo oscuro.

### <a name="eu-metadata-storage-now-in-public-preview"></a>Almacenamiento de metadatos de EU ahora en versión preliminar pública

Ahora hospedamos una versión preliminar pública de la geografía de Europa (EU) como opción de almacenamiento para los metadatos de servicio en Azure Virtual Desktop. Los clientes pueden elegir entre Oeste o Norte de Europa al crear sus objetos de servicio. Los objetos de servicio y los metadatos de los grupos de hosts se almacenarán en la geografía de Azure asociada a cada región. Para obtener más información, lea [la entrada de blog que anuncia la versión preliminar pública](https://techcommunity.microsoft.com/t5/windows-virtual-desktop/announcing-public-preview-of-windows-virtual-desktop-service/m-p/2143939).

### <a name="teams-on-azure-virtual-desktop-plugin-updates"></a>Actualizaciones del complemento de Teams en Azure Virtual Desktop

Ha mejorado la calidad de las llamadas de vídeo en el complemento Azure Virtual Desktop gracias a haber solucionado los problemas que se notifican con más frecuencia, como el hecho de que la pantalla se quede oscura de repente o que la imagen y el sonido dejen de estar sincronizados. Estas mejoras deberían aumentar el rendimiento de la vista de un solo vídeo con cambio de altavoz activo. También se ha corregido un problema que hacía que los dispositivos de hardware con caracteres especiales no estuvieran disponibles en Teams.

## <a name="january-2021"></a>Enero de 2021

Estos son los cambios que han tenido lugar en enero de 2021:

### <a name="new-azure-virtual-desktop-offer"></a>Nueva oferta de Azure Virtual Desktop

Los clientes nuevos se ahorrarán un 30 % en los costos de Azure Virtual Desktop para las máquinas virtuales de las series D y B hasta un máximo de 90 días al usar la solución nativa de Microsoft. Puede canjear esta oferta en Azure Portal antes del 31 de marzo de 2021. Obtenga más información en nuestra [página de ofertas de Azure Virtual Desktop](https://azure.microsoft.com/services/virtual-desktop/offer/).

### <a name="networksecuritygrouprules-value-change"></a>Cambio del valor de networkSecurityGroupRules 

En la plantilla anidada de Azure Resource Manager, hemos cambiado el valor predeterminado de networkSecurityGroupRules de un objeto a una matriz. Esto evitará que se produzcan errores al usar managedDisks-customimagevm.json sin especificar un valor para networkSecurityGroupRules. No se trata de un cambio significativo y es compatible con versiones anteriores.

### <a name="fslogix-hotfix-update"></a>Actualización de revisión de FSLogix

Hemos publicado FSLogix, versión 2009 HF_01 (2.9.7654.46150) para resolver problemas de la versión anterior (2.9.7621.30127). Se recomienda dejar de usar la versión anterior y actualizar FSLogix lo antes posible.

Para obtener más información, consulte las notas de la versión en las [novedades de FSLogix](/fslogix/whats-new#fslogix-apps-2009-hf_01-29765446150).

### <a name="azure-portal-experience-improvements"></a>Mejoras de la experiencia en Azure Portal

Hemos efectuado las siguientes mejoras de la experiencia en Azure Portal:

- Ahora se pueden agregar directamente credenciales de administrador local de máquina virtual, en lugar de tener que agregar una cuenta local creada con las credenciales de la cuenta de unión de dominio de Active Directory.
- Los usuarios ahora pueden mostrar asignaciones individuales y de grupo en pestañas separadas para usuarios y grupos individuales.
- El número de versión del agente de Azure Virtual Desktop ahora es visible en la información general de la máquina virtual para los grupos host.
- Eliminación masiva agregada para grupos de hosts y grupos de aplicaciones.
- Ahora es posible habilitar o deshabilitar el modo de purga para múltiples hosts de sesión en un grupo de hosts.
- El campo de IP pública se ha eliminado de la página de detalles de máquina virtual.

### <a name="azure-virtual-desktop-agent-troubleshooting"></a>Solución de problemas del agente de Azure Virtual Desktop

Recientemente se ha redactado la [guía de solución de problemas del agente de Azure Virtual Desktop](troubleshoot-agent.md) para ayudar a los clientes que han experimentado problemas comunes.

### <a name="microsoft-defender-for-endpoint-integration"></a>Integración de Microsoft Defender for Endpoint

La integración de Microsoft Defender for Endpoint ya está disponible con carácter general. Esta característica permite que las máquinas virtuales de Azure Virtual Desktop proporcionen la misma experiencia de investigación que una máquina local de Windows 10. Si usa Windows 10 Enterprise multisesión, Microsoft Defender for Endpoint admitirá hasta 50 conexiones de usuario simultáneas. De este modo, ahorrará en el costo de Windows 10 multisesión y disfrutará de la seguridad de Microsoft Defender for Endpoint. Para más información, consulte [nuestra entrada en el blog](https://techcommunity.microsoft.com/t5/microsoft-defender-for-endpoint/windows-virtual-desktop-support-is-now-generally-available/ba-p/2103712).

### <a name="azure-security-baseline-for-azure-virtual-desktop"></a>Línea de base de seguridad de Azure para Azure Virtual Desktop

Recientemente hemos publicado un [artículo sobre la línea de base de seguridad de Azure](security-baseline.md) para Azure Virtual Desktop que nos gustaría que consultara. Estas directrices incluyen información sobre cómo aplicar Azure Security Benchmark 2.0 en Azure Virtual Desktop. Azure Security Benchmark describe la configuración y las procedimientos que recomendamos para usar las soluciones en la nube de forma segura en Azure.

## <a name="december-2020"></a>Diciembre de 2020

Estos son los cambios que han tenido lugar en diciembre de 2020: 

### <a name="azure-monitor-for-azure-virtual-desktop"></a>Azure Monitor para Azure Virtual Desktop

Ya está disponible la versión preliminar pública de Azure Monitor para Azure Virtual Desktop. Esta nueva característica incluye un sólido panel basado en los libros de Azure Monitor que ayuda a los profesionales de TI a conocer sus entornos de Azure Virtual Desktop. Para más información, consulte [el anuncio en nuestro blog](https://techcommunity.microsoft.com/t5/windows-virtual-desktop/azure-monitor-for-windows-virtual-desktop-public-preview/m-p/1946587). 

### <a name="azure-resource-manager-template-change"></a>Cambio de plantilla de Azure Resource Manager 

En la actualización más reciente, hemos quitado todos los parámetros de dirección IP pública de la plantilla de Azure Resource Manager para crear y aprovisionar grupos de hosts. Se recomienda encarecidamente evitar el uso de IP públicas para Azure Virtual Desktop, con el fin de mantener la implementación segura. Si para la implementación se han usado IP públicas, será preciso cambiar la configuración para que use direcciones IP privadas; de lo contrario, la implementación no funcionará correctamente.

### <a name="msix-app-attach-public-preview"></a>Versión preliminar pública de conexión de aplicaciones MSIX 

Conexión de aplicaciones MSIX es otro servicio que ha comenzado su versión preliminar pública este mes. Conexión de aplicaciones MSIX es un servicio que presenta dinámicamente aplicaciones MSIX en las máquinas virtuales de host de sesión de Azure Virtual Desktop. Para más información, consulte [el anuncio en nuestro blog](https://techcommunity.microsoft.com/t5/windows-virtual-desktop/msix-app-attach-azure-portal-integration-public-preview/m-p/1986231). 

### <a name="screen-capture-protection"></a>Protección de la captura de pantalla 

Este mes también ha marcado el comienzo de la versión preliminar de la protección de la captura de pantalla. Esta característica se puede usar para evitar que se capture información confidencial en los puntos de conexión de cliente. Para probar la protección de la captura de pantalla, vaya a [esta página](https://aka.ms/WVDScreenCaptureProtection).  

### <a name="built-in-roles"></a>Roles integrados

Hemos agregado nuevos roles integrados a Azure Virtual Desktop para permisos de administrador. Para más información, consulte [Roles integrados de Azure Virtual Desktop](rbac.md). 

### <a name="application-group-limit-increase"></a>Aumento del límite de grupos de aplicaciones

Hemos aumentado el límite predeterminado de grupos de aplicaciones por inquilino de Azure Active Directory a 200 grupos.

### <a name="client-updates-for-december-2020"></a>Actualizaciones del cliente de diciembre de 2020

Hemos lanzado nuevas versiones de los siguientes clientes: 

- Android
- macOS
- Windows

Para más información sobre las actualizaciones de los clientes, consulte [Actualizaciones de clientes](whats-new.md#client-updates).

## <a name="november-2020"></a>Noviembre de 2020

### <a name="azure-portal-experience"></a>Experiencia de Azure Portal

Hemos corregido dos errores en Azure Portal:

- El nombre descriptivo de la aplicación de escritorio ya no se sobrescribe en el flujo de trabajo "Agregar máquina virtual".
- La pestaña del host de sesión se cargará si los hosts de sesión forman parte de conjuntos de escalado.

### <a name="fslogix-client-version-2009"></a>Cliente de FSLogix, versión 2009 

Se ha publicado una nueva versión del cliente FSLogix con muchas correcciones y mejoras. En [nuestra entrada de blog](https://social.msdn.microsoft.com/Forums/en-US/defe5828-fba4-4715-a68c-0e4d83eefa6b/release-notes-for-fslogix-apps-release-2009-29762130127?forum=FSLogix) encontrará más información.

### <a name="rdp-shortpath-public-preview"></a>Versión preliminar pública de RDP Shortpath

RDP Shortpath introduce una conectividad directa con el host de sesión de Azure Virtual Desktop tanto mediante redes virtuales privadas de punto a sitio y de sitio a sitio, como mediante ExpressRoute. También presenta el protocolo de transporte URCP. RDP Shortpath está diseñado para reducir la latencia y los saltos en la red, y así mejorar la experiencia del usuario. Obtenga más información al respecto en [RDP Shortpath de Azure Virtual Desktop](shortpath.md).

### <a name="azdesktopvirtualization-version-201"></a>Az.DesktopVirtualization, versión 2.0.1

Se ha publicado la versión 2.0.1 de los cmdlets de Azure Virtual Desktop. Esta actualización incluye cmdlets que permitirán administrar la asociación de aplicaciones en formato .MSIX. La nueva versión se puede descargar en [la galería de PowerShell](https://www.powershellgallery.com/packages/Az.DesktopVirtualization/2.0.1).

### <a name="azure-advisor-updates"></a>Actualizaciones de Azure Advisor

Azure Advisor ya tiene una nueva recomendación para la guía de proximidad en Azure Virtual Desktop y una nueva recomendación para optimizar el rendimiento en los grupos de hosts con equilibrio de carga con equilibrio de carga en profundidad. Obtenga más información en [el sitio web de Azure](https://azure.microsoft.com/updates/new-recommendations-from-azure-advisor/).

## <a name="october-2020"></a>Octubre de 2020

Estos son los cambios que han tenido lugar en octubre de 2020:

### <a name="improved-performance"></a>rendimiento mejorado.

- Hemos optimizado el rendimiento mediante la reducción de la latencia de conexión en las siguientes ubicaciones geográficas de Azure:
    - Suiza
    - Canadá

Ahora puede usar el [estimador de experiencia](https://azure.microsoft.com/services/virtual-desktop/assessment/) para estimar la calidad de la experiencia del usuario en estas áreas.

### <a name="azure-government-cloud-availability"></a>Disponibilidad de Azure Government Cloud

Azure Government Cloud ya está disponible con carácter general. En [nuestra entrada de blog](https://azure.microsoft.com/updates/windows-virtual-desktop-is-now-generally-available-in-the-azure-government-cloud/) encontrará más información.

### <a name="azure-virtual-desktop-azure-portal-updates"></a>Actualizaciones de Azure Virtual Desktop en Azure Portal

Se han realizado varias actualizaciones de Azure Virtual Desktop en Azure Portal:

- Se corrigió un error de resourceID que impedía a los usuarios abrir la pestaña "Sesiones".
- Se ha simplificado la interfaz de usuario en la pestaña "Hosts de sesión".
- Se han corregido los valores "Valores predeterminados", "Facilidad de uso" y "Restaurar valores predeterminados" en las propiedades del RDP.
- Las funciones "Quitar" y "Eliminar" aparecen en todas las pestañas.
- El portal ahora valida los nombres de las aplicaciones en el flujo de trabajo "Agregar una aplicación".
- Se corrigió el problema de que los datos exportados del host de sesión no estaban alineados en las columnas.
- Se corrigió el problema de que el portal no puede recuperar las sesiones del usuario.
- Se corrigió un problema en la recuperación del host de sesión que se producía cuando la máquina virtual se creaba en otro grupo de recursos.
- Se actualizó la pestaña "Host de sesión", ahora muestra tanto las sesiones activas como las desconectadas.
- La pestaña "Aplicaciones" ahora tiene páginas.
- Se corrigió el problema de que el texto "requiere línea de comandos" no se mostraba correctamente en la pestaña "Lista de aplicaciones".
- Se corrigió el problema de que el portal no podía implementar grupos de hosts ni máquinas virtuales si se usaba la versión en alemán de Shared Image Gallery.

### <a name="client-updates-for-october-2020"></a>Actualizaciones del cliente de octubre de 2020

Hemos lanzado nuevas versiones de los clientes. Para obtener más información, vea estos artículos:

- [Windows](/windows-server/remote/remote-desktop-services/clients/windowsdesktop-whatsnew)
- [iOS](/windows-server/remote/remote-desktop-services/clients/ios-whatsnew)

Para más información sobre los otros clientes, consulte [Actualizaciones de clientes](#client-updates).

## <a name="september-2020"></a>Septiembre de 2020

Estos son los cambios que se han producido en septiembre de 2020:

- Hemos optimizado el rendimiento mediante la reducción de la latencia de conexión en las siguientes ubicaciones geográficas de Azure:
    - Alemania
    - Sudáfrica (solo para entornos de validación)

Ahora puede usar el [estimador de experiencia](https://azure.microsoft.com/services/virtual-desktop/assessment/) para estimar la calidad de la experiencia del usuario en estas áreas.

- Se ha publicado la versión 1.2.1364 del cliente de escritorio de Windows para Azure Virtual Desktop. En esta actualización hemos realizado los cambios siguientes:
    - Se corrigió un problema por el que el inicio de sesión único (SSO) no funcionaba en Windows 7.
    - Se corrigió un problema que hacía que el cliente se desconectara cuando un usuario que habilitaba la optimización de medios para los equipos intentaba llamar o unirse a una reunión de Teams mientras otra aplicación tenía una secuencia de audio abierta en modo exclusivo.
    - Se corrigió un problema por el que Teams no enumeraba dispositivos de audio o vídeo cuando se habilitaba la optimización de medios para Teams.
    - A agregó un vínculo "¿Necesita ayuda con la configuración?" a la página de configuración del escritorio.
    - Se corrigió un problema con el botón "Suscribirse" que se producía al usar los temas oscuros de contraste alto.
    
- Gracias a la enorme ayuda de nuestros usuarios, hemos corregido dos problemas críticos para el Cliente de Escritorio remoto de Microsoft Store. Seguiremos revisando los comentarios y solucionaremos los problemas a medida que vayamos ampliando nuestra versión en fases del cliente a más usuarios por todo el mundo.
    
- Hemos agregado una nueva característica que le permite cambiar la ubicación de la máquina virtual, la imagen, el grupo de recursos, el nombre del prefijo y la configuración de red como parte del flujo de trabajo de incorporación de una máquina virtual a la implementación de Azure Portal.

- Los profesionales de TI ahora pueden administrar máquinas virtuales híbridas de Windows 10 Enterprise conectadas a Azure Active Directory con Microsoft Endpoint Manager. Para más información, consulte nuestra [entrada del blog](https://techcommunity.microsoft.com/t5/microsoft-endpoint-manager-blog/microsoft-endpoint-manager-announces-support-for-windows-virtual/ba-p/1681048).

## <a name="august-2020"></a>Agosto de 2020

Estos son los cambios que se han producido en agosto de 2020:

- Hemos mejorado el rendimiento para reducir la latencia de conexión en las siguientes regiones de Azure: 

    - Reino Unido
    - Francia
    - Noruega
    - Corea del Sur

   Puede usar el [Estimador de experiencia](https://azure.microsoft.com/services/virtual-desktop/assessment/) para obtener una idea general de la forma en que estos cambios afectarán a sus usuarios.

- El cliente de Escritorio remoto de Microsoft Store (v 10.2.1522 +) ya está disponible con carácter general. Esta versión del cliente de Escritorio remoto de Microsoft Store es compatible con Azure Virtual Desktop. También hemos incorporado flujos de interfaz de usuario actualizados para mejorar las experiencias de los usuarios. Esta actualización incluye un diseño fluido, modos claros y oscuros, y muchos otros cambios atractivos. También hemos reescrito el cliente para que use el mismo motor de protocolo de escritorio remoto (RDP) subyacente que los clientes iOS, macOS y Android, lo que nos permite ofrecer nuevas características a mayor velocidad en todas las plataformas. [Descargue el cliente](https://www.microsoft.com/p/microsoft-remote-desktop/9wzdncrfj3ps?rtc=1&activetab=pivot:overviewtab) y pruébelo.

- Hemos solucionado un problema en el cliente de escritorio de Teams (versión 1.3.00.21759) en el que el cliente solo mostraba la zona horaria UTC en el chat, los canales y el calendario. El cliente actualizado muestra ahora la zona horaria de la sesión remota.

- Ahora, Azure Advisor forma parte de Azure Virtual Desktop. Al acceder a Azure Virtual Desktop a través de Azure Portal, puede ver recomendaciones para optimizar el entorno de Azure Virtual Desktop. Más información en [Azure Advisor](azure-advisor.md).

- La CLI de Azure ahora admite Azure Virtual Desktop (`az desktopvirtualization`) para ayudarle a automatizar las implementaciones de Azure Virtual Desktop. Consulte [desktopvirtualization](/cli/azure/desktopvirtualization) para ver una lista de los comandos de la extensión.

- Hemos actualizado nuestras plantillas de implementación para que sean totalmente compatibles con las interfaces de Azure Resource Manager de Azure Virtual Desktop. Puede encontrar las plantillas en [GitHub](https://github.com/Azure/RDS-Templates/tree/master/ARM-wvd-templates).

- El portal de US Gov de Azure Virtual Desktop se encuentra ahora en versión preliminar pública. Para más información, consulte [nuestro anuncio](https://azure.microsoft.com/updates/windows-virtual-desktop-is-now-available-in-the-azure-government-cloud-in-preview/).

## <a name="july-2020"></a>Julio de 2020  

En julio se ha iniciado la disponibilidad con carácter general de Azure Virtual Desktop con integración de Azure Resource Management.

Esto es lo que ha cambiado con esta nueva versión: 

- La "versión Otoño 2019" se conoce ahora como "Azure Virtual Desktop (clásico)", mientras que la "versión de primavera de 2020" es ahora simplemente "Azure Virtual Desktop". Para más información, consulte [esta entrada de blog](https://azure.microsoft.com/blog/new-windows-virtual-desktop-capabilities-now-generally-available/). 

Para más información acerca de las nuevas características, consulte [esta entrada de blog](https://techcommunity.microsoft.com/t5/itops-talk-blog/windows-virtual-desktop-spring-update-enters-public-preview/ba-p/1340245). 

### <a name="autoscaling-tool-update"></a>Actualización de la herramienta de escalado automático

La versión más reciente de la herramienta de escalado automático que estaba en versión preliminar ya está disponible con carácter general. Esta herramienta usa una cuenta de Azure Automation y la aplicación lógica de Azure para apagar y reiniciar automáticamente las máquinas virtuales del host de sesión dentro de un grupo de hosts, lo que reduce los costos de infraestructura. Más información en [Escalado de hosts de sesión con Azure Automation](set-up-scaling-script.md).

### <a name="azure-portal"></a>Azure Portal

Ahora puede hacer lo siguiente con Azure Portal en Azure Virtual Desktop: 

- Asignación directa de usuarios a hosts de sesión de escritorio personal.  
- Cambio de la configuración del entorno de validación para grupos host. 

### <a name="diagnostics"></a>Diagnóstico

Hemos publicado algunas nuevas consultas precompiladas para el área de trabajo de Log Analytics. Para obtener acceso a las consultas, vaya a **Registros** y, en **Categoría**, seleccione **Azure Virtual Desktop**. Más información en [Uso de Log Analytics para la característica de diagnóstico](diagnostics-log-analytics.md).

### <a name="update-for-remote-desktop-client-for-android"></a>Actualización para el cliente de Escritorio remoto para Android

El [cliente de Escritorio remoto para Android](https://play.google.com/store/apps/details?id=com.microsoft.rdc.androidx) ahora admite las conexiones de Azure Virtual Desktop. A partir de la versión 10.0.7, el cliente de Android ofrece una nueva interfaz de usuario para mejorar la experiencia del usuario. El cliente también se integra con Microsoft Authenticator en los dispositivos Android para habilitar el acceso condicional cuando se realiza una suscripción a áreas de trabajo de Azure Virtual Desktop.  

La versión anterior del cliente de Escritorio remoto se denomina ahora "Escritorio remoto 8". Las conexiones que ya tenga en la versión anterior del cliente se transferirán sin problemas al nuevo cliente. El nuevo cliente se ha vuelto a escribir en el mismo motor de RDP Core subyacente que los clientes de iOS y macOS, lo que agiliza la versión de las nuevas características en todas las plataformas. 

### <a name="teams-update"></a>Actualización de Teams

Hemos realizado algunas mejoras en Microsoft Teams para Azure Virtual Desktop. Lo más importante es que Azure Virtual Desktop ahora admite la optimización de audio y vídeo para el cliente del escritorio de Windows. El redireccionamiento mejora la latencia, con la creación de rutas de acceso directas entre usuarios cuando estos realizan llamadas de audio o vídeo y conferencias. Una Menor distancia significa menos saltos, lo que mejora tanto la imagen como el sonido en las llamadas. Más información en [Uso de Teams en Azure Virtual Desktop](./teams-on-avd.md).

## <a name="june-2020"></a>Junio de 2020

El mes pasado, presentamos la versión preliminar de la integración de Azure Virtual Desktop con Azure Resource Manager. Esta actualización tiene muchas características nuevas y atractivas de las que nos encantaría informarle. Estas son las novedades de esta versión de Azure Virtual Desktop.

### <a name="azure-virtual-desktop-is-now-integrated-with-azure-resource-manager"></a>Azure Virtual Desktop ahora se integra con Azure Resource Manager

Azure Virtual Desktop ahora se integra en Azure Resource Manager. En la última actualización, todos los objetos de Azure Virtual Desktop pasan a ser recursos de Azure Resource Manager. Esta actualización también se integra con los controles de acceso basado en rol (Azure RBAC) de Azure. Para más información, consulte [¿Qué es Azure Resource Manager?](../azure-resource-manager/management/overview.md).

Esto es lo que hace este cambio:

- Azure Virtual Desktop ahora está integrado en Azure Portal, lo que significa que puede administrar todo directamente en el portal, no se requieren PowerShell, aplicaciones web o herramientas de terceros. Para empezar, consulte nuestro tutorial en [Creación de un grupo de hosts con Azure Portal](create-host-pools-azure-marketplace.md).

- Antes de esta actualización, solo podía publicar RemoteApps y Desktops para usuarios individuales. Con Azure Resource Manager, ahora puede publicar recursos en grupos de Azure Active Directory.

- La versión anterior de Azure Virtual Desktop tenía cuatro roles de administrador integrados que podría asignar a un inquilino o a un grupo de hosts. Estos roles están ahora en el [Control de acceso basado en rol de Azure (Azure RBAC)](../role-based-access-control/overview.md). Estos roles se pueden a todos los objetos de Azure Resource Manager de Azure Virtual Desktop, lo que le permite tener un modelo completo de delegación enriquecida.

- En esta actualización, ya no preciso ejecutar Azure Marketplace o la plantilla de GitHub varias veces para expandir un grupo de hosts. Lo único que se necesita para expandir un grupo de hosts es ir a él en Azure Portal y seleccionar **+ Agregar** para implementar hosts de sesión adicionales.

- Ahora, la implementación de los grupos de hosts está completamente integrada en [Azure Shared Image Gallery](../virtual-machines/shared-image-galleries.md). Shared Image Gallery es un servicio de Azure independiente que almacena definiciones de imágenes de máquina virtual, incluido el control de versiones de las imágenes. También puede usar la replicación global para copiar y enviar las imágenes a otras regiones de Azure para su implementación local.

- Las funciones de supervisión que solían realizarse a través de PowerShell o de la aplicación web del servicio de diagnóstico se han desplazado a Log Analytics en Azure Portal. Ahora también tiene dos opciones para visualizar los informes. Puede ejecutar consultas Kusto y usar Workbooks para crear informes visuales.

- Ya no es preciso completar el consentimiento de Azure Active Directory (Azure AD) para usar Azure Virtual Desktop. En esta actualización, el inquilino de Azure AD de su suscripción de Azure autentica a los usuarios y proporciona controles de Azure RBAC a los administradores.

### <a name="powershell-support"></a>Compatibilidad con PowerShell

Hemos agregado nuevos cmdlets AzWvd al módulo Az de Azure PowerShell con esta actualización. Este nuevo módulo es compatible con PowerShell Core, que se ejecuta en .NET Core.

Para instalar el módulo, siga las instrucciones de [Configuración del módulo de PowerShell para Azure Virtual Desktop](powershell-module.md).

También puede ver una lista de los comandos disponibles en la [referencia de AzWvd PowerShell](/powershell/module/az.desktopvirtualization/#desktopvirtualization).

Para más información acerca de las nuevas características, consulte [nuestra entrada del blog](https://techcommunity.microsoft.com/t5/itops-talk-blog/windows-virtual-desktop-spring-update-enters-public-preview/ba-p/1340245).

### <a name="additional-gateways"></a>Puertas de enlace adicionales

Hemos agregado un nuevo clúster de puerta de enlace en Sudáfrica para reducir la latencia de conexión.

### <a name="microsoft-teams-on-azure-virtual-desktop-preview"></a>Microsoft Teams en Azure Virtual Desktop (versión preliminar)

Hemos realizado algunas mejoras en Microsoft Teams para Azure Virtual Desktop. Y lo que es más importante, Azure Virtual Desktop ahora admite el redireccionamiento de audio y visual para las llamadas. El redireccionamiento mejora la latencia, ya que crea rutas de acceso directas entre usuarios cuando llaman mediante audio o vídeo. Una Menor distancia significa menos saltos, lo que mejora tanto la imagen como el sonido en las llamadas.

Para más información, consulte nuestra [entrada del blog](https://azure.microsoft.com/updates/windows-virtual-desktop-media-optimization-for-microsoft-teams-is-now-available-in-public-preview/).

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre el futuro de [Azure Virtual Desktop en el plan de desarrollo de Microsoft 365](https://www.microsoft.com/microsoft-365/roadmap?filters=Windows%20Virtual%20Desktop).
