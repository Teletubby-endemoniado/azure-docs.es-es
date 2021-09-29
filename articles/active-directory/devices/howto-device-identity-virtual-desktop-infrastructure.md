---
title: 'Identidad del dispositivo y virtualización del escritorio: Azure Active Directory'
description: Obtenga información sobre cómo se pueden usar en conjunto las identidades de dispositivos de Azure AD y VDI
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 09/14/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca70009eea6f8988fadd8f09a0460911fac299de
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128630326"
---
# <a name="device-identity-and-desktop-virtualization"></a>Identidad del dispositivo y virtualización del escritorio

Por lo general, los administradores implementan plataformas de infraestructura de escritorio virtual (VDI) que hospedan los sistemas operativos Windows en sus organizaciones. Los administradores implementan VDI para:

- Simplificar la administración.
- Reducir los costos a través de la consolidación y la centralización de los recursos.
- Ofrecer a los usuarios finales movilidad y libertad para acceder a los escritorios virtuales en cualquier momento, desde cualquier lugar y en cualquier dispositivo.

Hay dos tipos principales de escritorios virtuales:

- Persistente
- No persistente

Las versiones persistentes usan una imagen de escritorio única para cada usuario o grupo de usuarios. Estos escritorios únicos se pueden personalizar y guardar para su uso futuro. 

Las versiones no persistentes usan una colección de equipos de escritorio a los que los usuarios pueden tener acceso según sea necesario. Estos escritorios no persistentes se revierten al estado original. En el caso de los dispositivos de Windows actual<sup>1</sup>, esto sucede cuando una máquina virtual pasa por un proceso de apagado, reinicio o restablecimiento del sistema operativo; y en el caso de dispositivos de Windows de nivel inferior<sup>2</sup> esto sucede cuando un usuario cierra sesión.

Se ha producido un aumento en las implementaciones de VDI no persistentes, ya que el trabajo remoto sigue siendo la norma. A medida que los clientes implementan un entorno VDI no persistente, es importante asegurarse de administrar los dispositivos obsoletos que se crean como consecuencia del registro frecuente de dispositivos sin una estrategia adecuada para la administración del ciclo de vida de los dispositivos.

> [!IMPORTANT]
> Si los dispositivos obsoletos no se administran debidamente, podría provocar una mayor presión en el consumo de cuota por parte de los inquilinos, además de un riesgo potencial de interrupción del servicio si la cuota de los inquilinos se agota. Para evitar esta situación, siga las directrices que se incluyen a continuación cuando implemente entornos VDI no persistentes.

En este artículo se tratan las guías de Microsoft para los administradores sobre la compatibilidad con la identidad del dispositivo y VDI. Para más información sobre la identidad del dispositivo, consulte el artículo [¿Qué es una identidad de dispositivo?](overview.md)

## <a name="supported-scenarios"></a>Escenarios admitidos

Antes de configurar las identidades de dispositivo en Azure AD para el entorno de VDI, familiarícese con los escenarios admitidos. En la tabla siguiente se muestran los escenarios de aprovisionamiento que se admiten. El aprovisionamiento en este contexto implica que un administrador puede configurar identidades de dispositivo a escala sin requerir ninguna interacción del usuario final.

| Tipo de identidad del dispositivo | Infraestructura de identidad | Dispositivos Windows | Versión de la plataforma de VDI | Compatible |
| --- | --- | --- | --- | --- |
| Híbrido unido a Azure AD | Federada<sup>3</sup> | Windows actual y Windows de nivel inferior | Persistente | Sí |
|   |   | Windows actual | No persistente | Sí<sup>5</sup> |
|   |   | Dispositivos de Windows de nivel inferior | No persistente | Sí<sup>6</sup> |
|   | Administrada<sup>4</sup> | Windows actual y Windows de nivel inferior | Persistente | Sí |
|   |   | Windows actual | No persistente | No |
|   |   | Dispositivos de Windows de nivel inferior | No persistente | Sí<sup>6</sup> |
| Unido a Azure AD | Federado | Windows actual | Persistente | Limitada<sup>7</sup> |
|   |   |   | No persistente | No |
|   | Administrado | Windows actual | Persistente | Limitada<sup>7</sup> |
|   |   |   | No persistente | No |
| Registrado en Azure AD | Federada/administrada | Windows actual/Windows de nivel inferior | Persistente/no persistente | No es aplicable |

<sup>1</sup> Los dispositivos **Windows actuales** representan a Windows 10, Windows Server 2016 v1803 o superior, y Windows Server 2019.

<sup>2</sup> Los dispositivos de **Windows de nivel inferior** representan a Windows 7, Windows 7, Windows Server 8.1, Windows Server 2008 R2, Windows Server 2012 y Windows Server 2012 R2. Para obtener información de soporte técnico sobre Windows 7, consulte [El soporte técnico para Windows 7 está llegando a su fin](https://www.microsoft.com/microsoft-365/windows/end-of-windows-7-support). Para información de soporte técnico sobre Windows Server 2008 R2, consulte [Preparación para la finalización del soporte técnico de Windows Server 2008](https://www.microsoft.com/cloud-platform/windows-server-2008).

<sup>3</sup> Un entorno de infraestructura de identidad **federada** representa un entorno con un proveedor de identidades como AD FS u otro IdP de terceros.

<sup>4</sup> Un entorno de infraestructura de identidad **administrada** representa un entorno con Azure AD como proveedor de identidades implementado mediante la [sincronización de hash de contraseñas (PHS)](../hybrid/whatis-phs.md) o la [autenticación de paso a través (PTA)](../hybrid/how-to-connect-pta.md) con [inicio de sesión único sin interrupciones](../hybrid/how-to-connect-sso.md).

<sup>5</sup> La **compatibilidad sin persistencia con Windows actual** requiere una consideración adicional, tal y como se documenta en la sección de directrices. Para este escenario se requiere Windows 10, 1803, Windows Server 2019 o Windows Server (canal semianual), a partir de la versión 1803.

<sup>6</sup> La **compatibilidad sin persistencia con Windows de nivel inferior** requiere una consideración adicional, tal y como se documenta a continuación en la sección de directrices.

<sup>7</sup> La **compatibilidad con Unión de Azure AD** solo está disponible con Azure Virtual Desktop y Windows 365

## <a name="microsofts-guidance"></a>Guías de Microsoft

Los administradores deben consultar los artículos siguientes, en función de su infraestructura de identidad, para aprender a configurar la Unión a Azure AD híbrido.

- [Configuración de la unión a Azure Active Directory híbrido para un entorno federado](hybrid-azuread-join-federated-domains.md)
- [Configuración de la unión a Azure Active Directory híbrido para un entorno administrado](hybrid-azuread-join-managed-domains.md)

### <a name="non-persistent-vdi"></a>VDI no persistente

A la hora de implementar un entorno VDI no persistente, Microsoft recomienda que los administradores de TI sigan las directrices que se ofrecen a continuación. De no hacerlo así, su directorio incluirá muchos dispositivos obsoletos unidos a Azure AD híbrido que se registraron desde la plataforma VDI no persistente, lo que provocará una mayor presión en la cuota de los inquilinos y el riesgo de interrupción del servicio si la cuota de los inquilinos se acaba.

- Si se basa en la herramienta de preparación del sistema (sysprep.exe) y utiliza para la instalación una imagen anterior a Windows 10 1809, asegúrese de que esa imagen no corresponda a un dispositivo que ya está registrado en Azure AD como unido a Azure AD híbrido.
- Si se basa en la instantánea de una máquina virtual (VM) para crear otras VM, asegúrese de que esa instantánea no sea de una VM que ya se haya registrado en Azure AD como unión a Azure AD híbrido.
- Cree un prefijo asociado al nombre para mostrar del equipo (por ejemplo, NPVDI-) y utilícelo para indicar que se trata de un escritorio basado en VDI no persistente.
- En el caso de Windows de nivel inferior:
   - Implemente el comando **autoworkplacejoin /leave** como parte del script de cierre de sesión. Este comando se debe desencadenar en el contexto del usuario y ejecutarse antes de que el usuario cierre la sesión completamente, mientras aún haya conectividad de red.
- En el caso de Windows actual en un entorno federado (por ejemplo, AD FS):
   - Implemente **dsregcmd /join** como parte de la secuencia u orden de arranque de la máquina virtual y antes de que el usuario inicie sesión.
   - **NO** ejecute el comando dsregcmd /leave como parte del proceso de apagado o reinicio de la máquina virtual.
- Defina e implemente el proceso para [administrar dispositivos obsoletos](manage-stale-devices.md).
   - Una vez que disponga de una estrategia para identificar los dispositivos no persistentes que se hayan unido a Azure AD híbrido, puede efectuar una limpieza más concienzuda de estos dispositivos para garantizar que su directorio no se quede sin espacio debido al acceso de muchos dispositivos obsoletos.
   - En el caso de las implementaciones de VDI no persistentes en Windows actual y Windows de nivel inferior, se deben eliminar los dispositivos con más de 15 días de antigüedad en **ApproximateLastLogonTimestamp**.

> [!NOTE]
> Al usar VDI no persistente, si quiere evitar la incorporación de una cuenta profesional o educativa, asegúrese de que la siguiente clave del Registro esté establecida:  
> `HKLM\SOFTWARE\Policies\Microsoft\Windows\WorkplaceJoin: "BlockAADWorkplaceJoin"=dword:00000001`    
>
> Asegúrese de que está ejecutando la versión 1803 o superior de Windows 10.  
>
> No se admite la itinerancia de datos en la ruta de acceso `%localappdata%`. Si decide mover el contenido de `%localappdata%`, asegúrese de que el contenido de las siguientes carpetas y claves del Registro no deje **nunca** el dispositivo, bajo ningún concepto. Por ejemplo: las herramientas de migración de perfiles deben omitir las siguientes carpetas y claves:
>
> * `%localappdata%\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy`
> * `%localappdata%\Packages\Microsoft.Windows.CloudExperienceHost_cw5n1h2txyewy`
> * `%localappdata%\Packages\<any app package>\AC\TokenBroker`
> * `%localappdata%\Microsoft\TokenBroker`
> * `HKEY_CURRENT_USER\SOFTWARE\Microsoft\IdentityCRL`
> * `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\AAD`
> * `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows NT\CurrentVersion\WorkplaceJoin`
>
> No se admite la movilidad del certificado de dispositivo de la cuenta profesional. El certificado, emitido por "MS-Organization-Access", se almacena en el almacén de certificados Personal (MI) del usuario actual y en la máquina local.


### <a name="persistent-vdi"></a>VDI persistente

A la hora de implementar un entorno VDI persistente, Microsoft recomienda que los administradores de TI sigan las directrices que se ofrecen a continuación. Si no lo hace, se producirán problemas de implementación y autenticación. 

- Si se basa en la herramienta de preparación del sistema (sysprep.exe) y utiliza para la instalación una imagen anterior a Windows 10 1809, asegúrese de que esa imagen no corresponda a un dispositivo que ya está registrado en Azure AD como unido a Azure AD híbrido.
- Si se basa en la instantánea de una máquina virtual (VM) para crear otras VM, asegúrese de que esa instantánea no sea de una VM que ya se haya registrado en Azure AD como unión a Azure AD híbrido.

Además, se recomienda implementar el proceso para [administrar los dispositivos obsoletos](manage-stale-devices.md). Esto garantizará que el directorio no se consume con muchos dispositivos obsoletos Si restablece periódicamente las máquinas virtuales.
 
## <a name="next-steps"></a>Pasos siguientes

[Configuración de la unión a Azure Active Directory híbrido para un entorno federado](hybrid-azuread-join-federated-domains.md)
