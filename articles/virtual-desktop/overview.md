---
title: ¿Qué es Azure Virtual Desktop? - Azure
description: Una introducción a Azure Virtual Desktop.
author: Heidilohr
ms.topic: overview
ms.date: 07/14/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: 9025b32aa2ea6fd8fefa91d89b608c0e5d26b45e
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "129992912"
---
# <a name="what-is-azure-virtual-desktop"></a>¿Qué es Azure Virtual Desktop?

Azure Virtual Desktop es un servicio de virtualización de escritorio y de aplicaciones que se ejecuta en la nube.

A continuación, se indica lo que se puede hacer al ejecutar Azure Virtual Desktop en Azure:

* Configurar una implementación de Windows 10 de sesión múltiple que ofrezca toda la funcionalidad de Windows 10 con escalabilidad
* Virtualizar las aplicaciones de Microsoft 365 para la empresa y optimizarlo para ejecutarse en escenarios virtuales multiusuario
* Proporcionar escritorios virtuales de Windows 7 con actualizaciones gratuitas de seguridad ampliada
* Llevar sus aplicaciones y escritorios existentes de Servicios de Escritorio remoto (RDS) y Windows Server a cualquier equipo
* Virtualizar escritorios y aplicaciones
* Administrar escritorios y aplicaciones de Windows 10, Windows Server y Windows 7 con una experiencia de administración unificada

## <a name="introductory-video"></a>Vídeo de introducción

En este vídeo, obtendrá información sobre Azure Virtual Desktop, y conocerá no solo por qué es único, sino también cuáles son sus novedades:

<br></br><iframe src="https://www.youtube.com/embed/NQFtI3JLtaU" width="640" height="320" allowFullScreen="true" frameBorder="0"></iframe>

Vea más vídeos sobre Azure Virtual Desktop en [nuestra lista de reproducción](https://www.youtube.com/watch?v=NQFtI3JLtaU&list=PLXtHYVsvn_b8KAKw44YUpghpD6lg-EHev).

## <a name="key-capabilities"></a>Principales capacidades

Con Azure Virtual Desktop, puede configurar un entorno flexible y escalable:

* Cree un entorno de virtualización de escritorio completo en su suscripción de Azure sin ejecutar ningún servidor de puerta de enlace adicional.
* Publique todos los grupos host que necesite para dar cabida a diversas cargas de trabajo.
* Aporte su propia imagen para las cargas de trabajo de producción o de prueba desde la Galería de Azure.
* Reduzca los costos con recursos de sesión múltiple agrupados. Con la nueva funcionalidad de sesión múltiple de Windows 10 Enterprise exclusiva de Azure Virtual Desktop y del rol de host de sesión de Escritorio remoto (RDSH) de Windows Server, puede reducir considerablemente el número de máquinas virtuales y la sobrecarga del sistema operativo (SO), mientras se proporcionan los mismos recursos a los usuarios.
* Asigne propiedad individual mediante escritorios personales (permanentes).

Puede implementar y administrar escritorios virtuales:

* Use las interfaces de Azure Portal, Azure Virtual Desktop PowerShell y REST para configurar los grupos de hosts, crear grupos de aplicaciones, asignar usuarios y publicar recursos.
* Publique aplicaciones de escritorio completo o aplicaciones remotas individuales desde un único grupo host, cree grupos de aplicaciones individuales para distintos conjuntos de usuarios o incluso asigne usuarios a varios grupos de aplicaciones para reducir el número de imágenes.
* A medida que administre su entorno, usará el acceso delegado integrado para asignar roles y recopilar diagnósticos con el fin de comprender diversos errores de configuración o de los usuarios.
* Use el nuevo servicio de diagnóstico para solucionar los errores.
* Administre únicamente la imagen y las máquinas virtuales y olvídese de la infraestructura. No es necesario que administre personalmente los roles de Escritorio remoto, como hace con los Servicios de Escritorio remoto, sino solo las máquinas virtuales de su suscripción de Azure.

También puede asignar y conectar a los usuarios a los escritorios virtuales:

* Una vez asignados, los usuarios pueden iniciar cualquier cliente de Azure Virtual Desktop para conectar a sus aplicaciones y escritorios de Windows publicados. Conéctese desde cualquier dispositivo mediante una aplicación nativa de su dispositivo o por medio del cliente web HTML5 de Azure Virtual Desktop.
* Establezca de forma segura los usuarios mediante conexiones inversas al servicio, así nunca tendrá que dejar los puertos de entrada abiertos.

## <a name="requirements"></a>Requisitos

Es necesario cumplir una serie de requisitos para configurar Azure Virtual Desktop y conectar correctamente sus usuarios a los escritorios y aplicaciones de Windows.

Admitimos los siguientes sistemas operativos, por lo que debe asegurarse de que tiene las [licencias apropiadas](https://azure.microsoft.com/pricing/details/virtual-desktop/) para sus usuarios en función del escritorio y de las aplicaciones que planee implementar:

|SO|Licencia necesaria|
|---|---|
|Sesión múltiple de Windows 10 Enterprise o Windows 10 Enterprise|Microsoft 365 E3, E5, A3, A5, F3, Business Premium<br>Windows E3, E5, A3, A5|
|Windows 7 Enterprise |Microsoft 365 E3, E5, A3, A5, F3, Business Premium<br>Windows E3, E5, A3, A5|
|Windows Server 2012 R2, 2016, 2019|Licencia de acceso de cliente (CAL) de RDS con Software Assurance|

Su infraestructura necesita cumplir los siguientes requisitos para ser compatible con Azure Virtual Desktop:

* [Azure Active Directory](../active-directory/index.yml).
* Una instancia de Windows Server Active Directory sincronizada con Azure Active Directory. Se puede configurar mediante Azure AD Connect (para organizaciones híbridas) o Azure AD Domain Services (para organizaciones híbridas o en la nube).
  * Una instancia de Windows Server AD sincronizada con Azure Active Directory. El usuario tiene como origen Windows Server AD y la máquina virtual con Azure Virtual Desktop está unida al dominio de Windows Server AD.
  * Una instancia de Windows Server AD sincronizada con Azure Active Directory. El usuario tiene como origen Windows Server AD y la máquina virtual con Azure Virtual Desktop está unida al dominio de Azure AD Domain Services.
  * Un dominio de Azure AD Domain Services. El usuario tiene como origen Azure Active Directory y la máquina virtual con Azure Virtual Desktop está unida al dominio de Azure AD Domain Services.
* Una suscripción a Azure, cuyo elemento primario es el mismo inquilino de Azure AD, que contenga una red virtual que contenga Windows Server Active Directory o una instancia de Azure AD DS, o esté conectada a ellos.

Requisitos de usuario para conectarse a Azure Virtual Desktop:

* El origen del usuario debe ser el mismo Active Directory que está conectado a Azure AD. Azure Virtual Desktop no admite cuentas B2B o MSA.
* El nombre principal de usuario que se usa para suscribirse a Azure Virtual Desktop debe existir en el dominio de Active Directory al que se une la máquina virtual.

Las máquinas virtuales de Azure que cree para Azure Virtual Desktop deben cumplir estos requisitos:

* Estar [unidas a un dominio estándar](../active-directory-domain-services/compare-identity-solutions.md) o a un [dominio híbrido](../active-directory/devices/hybrid-azuread-join-plan.md). Hay disponibles máquinas virtuales en versión preliminar [unidas a Azure AD](deploy-azure-ad-joined-vm.md).
* Deben ejecutar una de las siguientes [imágenes de sistema operativo admitidas](#supported-virtual-machine-os-images).

>[!NOTE]
>Si necesita una suscripción a Azure, puede [registrarse para obtener una evaluación gratuita por un mes](https://azure.microsoft.com/free/). Si usa la versión de evaluación gratuita de Azure, debe utilizar Azure AD Domain Services para mantener sincronizada su instancia de Windows Server Active Directory con Azure Active Directory.

Para obtener una lista de direcciones URL que debe desbloquear para que la implementación de Azure Virtual Desktop funcione según lo previsto, consulte nuestra [lista de direcciones URL requeridas](safe-url-list.md).

Azure Virtual Desktop incluye los escritorios y las aplicaciones de Windows que entrega a los usuarios y de la solución de administración, que Microsoft hospeda en Azure como un servicio. Se pueden implementar escritorios y aplicaciones en máquinas virtuales (VM) de cualquier región de Azure, y la solución de administración y los datos de estas VM residirán en Estados Unidos. Esto puede dar lugar a la transferencia de datos a Estados Unidos.

Para obtener un rendimiento óptimo, asegúrese de que la red cumple los requisitos siguientes:

* La latencia de ida y vuelta (RTT) desde la red del cliente hasta la región de Azure donde se han implementado grupos host debe ser inferior a 150 ms. Use el [estimador de experiencia](https://azure.microsoft.com/services/virtual-desktop/assessment) para ver el estado de la conexión y la región de Azure recomendada.
* El tráfico de red puede fluir fuera de las fronteras del país o la región si las máquinas virtuales que hospedan los escritorios y las aplicaciones se conectan al servicio de administración.
* Para optimizar el rendimiento de la red, se recomienda que las máquinas virtuales del host de sesión se coloquen en la región de Azure más cercana al usuario.

En nuestra [documentación de la arquitectura](/azure/architecture/example-scenario/wvd/windows-virtual-desktop) puede ver la configuración de una arquitectura típica de Azure Virtual Desktop para la empresa.

## <a name="supported-remote-desktop-clients"></a>Clientes compatibles de Escritorio remoto

Los clientes de Escritorio remoto siguientes admiten Azure Virtual Desktop:

* [Escritorio de Windows](./user-documentation/connect-windows-7-10.md)
* [Web](./user-documentation/connect-web.md)
* [macOS](./user-documentation/connect-macos.md)
* [iOS](./user-documentation/connect-ios.md)
* [Android](./user-documentation/connect-android.md)
* Cliente de Microsoft Store

> [!IMPORTANT]
> Azure Virtual Desktop no es compatible con el cliente de Conexión de RemoteApp y Escritorio (RADC) ni con el cliente de Conexión a Escritorio remoto (MSTSC).

Para más información sobre las direcciones URL que debe desbloquear para usar los clientes, consulte la [lista de direcciones URL seguras](safe-url-list.md).

## <a name="supported-virtual-machine-os-images"></a>Imágenes de SO de máquinas virtuales admitidas

Azure Virtual Desktop sigue la [directiva de ciclo de vida de Microsoft](/lifecycle/) y admite las siguientes imágenes de sistema operativo x64:

* Windows 11 Enterprise de sesión múltiple (versión preliminar)
* Windows 11 Enterprise (versión preliminar)
* Sesión múltiple de Windows 10 Enterprise
* Windows 10 Enterprise
* Windows 7 Enterprise
* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

Azure Virtual Desktop no admite imágenes de los sistemas operativos x86 (32 bits), Windows 10 Enterprise N, Windows 10 LTSB, Windows 10 LTSC, Windows 10 Pro o Windows 10 Enterprise KN. Windows 7 tampoco admite las soluciones de perfil basadas en VHD o VHDX hospedadas en Azure Storage administrado debido a un límite de tamaño del sector.

Las opciones de automatización y de implementación disponibles dependen del sistema operativo y la versión que elija, tal como se muestra en la tabla siguiente:

|Sistema operativo|Galería de imágenes de Azure|Implementación manual de la máquina virtual|Integración de la plantilla de Azure Resource Manager|Aprovisionamiento de grupos host en Azure Marketplace|
|--------------------------------------|:------:|:------:|:------:|:------:|
|Windows 11 Enterprise de sesión múltiple (versión preliminar)|Sí|Sí|Sí|Sí|
|Windows 11 Enterprise (versión preliminar)|Sí|Sí|Sí|Sí|
|Sesión múltiple de Windows 10 Enterprise, versión 1909 o superior|Sí|Sí|Sí|Sí|
|Windows 10 Enterprise, versión 1909 o superior|Sí|Sí|Sí|Sí|
|Windows 7 Enterprise|Sí|Sí|No|No|
|Windows Server 2019|Sí|Sí|No|No|
|Windows Server 2016|Sí|Sí|Sí|Sí|
|Windows Server 2012 R2|Sí|Sí|No|No|

## <a name="next-steps"></a>Pasos siguientes

Si usa Azure Virtual Desktop (clásico), puede empezar a trabajar con el tutorial de [Creación de un inquilino en Azure Virtual Desktop](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md).

Si usa Azure Virtual Desktop con la integración de Azure Resource Manager, deberá crear un grupo de hosts. Vaya al siguiente tutorial para comenzar.

> [!div class="nextstepaction"]
> [Creación de un grupo de hosts con Azure Portal](create-host-pools-azure-marketplace.md)
