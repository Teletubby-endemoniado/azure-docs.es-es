---
title: Unión de una máquina virtual RHEL a Azure AD Domain Services | Microsoft Docs
description: Aprenda a configurar y unir una máquina virtual Red Hat Enterprise Linux a un dominio administrado de Azure AD Domain Services.
services: active-directory-ds
author: justinha
manager: daveba
ms.assetid: 16100caa-f209-4cb0-86d3-9e218aeb51c6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 07/13/2020
ms.author: justinha
ms.openlocfilehash: c883e3e5cb82107b22bf06176092ec9cfae46d23
ms.sourcegitcommit: 096e7972e2a1144348f8d648f7ae66154f0d4b39
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2021
ms.locfileid: "112516700"
---
# <a name="join-a-red-hat-enterprise-linux-virtual-machine-to-an-azure-active-directory-domain-services-managed-domain"></a>Join a Red Hat Enterprise Linux virtual machine to an Azure AD Domain Services managed domain (Unión de una máquina virtual con Red Hat Enterprise Linux a un dominio administrado de Azure Active Directory Domain Services)

Para que los usuarios puedan iniciar sesión en máquinas virtuales (VM) en Azure con un único conjunto de credenciales, puede unir VM a un dominio administrado de Azure Active Directory Domain Services (Azure AD DS). Al unir una máquina virtual a un dominio administrado de Azure AD DS, se pueden usar las cuentas de usuario y las credenciales del dominio para iniciar sesión y administrar servidores. También se aplican las pertenencias a grupos del dominio administrado para permitirle controlar el acceso a los archivos o servicios de la VM.

Este artículo muestra cómo unir una máquina virtual Red Hat Enterprise Linux (RHEL) a un dominio administrado.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará los siguientes recursos y privilegios:

* Una suscripción de Azure activa.
    * Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Un inquilino de Azure Active Directory asociado a su suscripción, ya sea sincronizado con un directorio en el entorno local o con un directorio solo en la nube.
    * Si es necesario, [cree un inquilino de Azure Active Directory][create-azure-ad-tenant] o [asocie una suscripción a Azure con su cuenta][associate-azure-ad-tenant].
* Un dominio administrado de Azure Active Directory Domain Services habilitado y configurado en su inquilino de Azure AD.
    * Si es necesario, el primer tutorial [crea y configura un dominio administrado de Azure Active Directory Domain Services][create-azure-ad-ds-instance].
* Una cuenta de usuario que forme parte del dominio administrado.
* Nombres de máquina virtual Linux únicos con un máximo de 15 caracteres para evitar nombres truncados que podrían causar conflictos en Active Directory.

## <a name="create-and-connect-to-a-rhel-linux-vm"></a>Creación y conexión a una máquina virtual RHEL Linux

Si ya tiene una máquina virtual RHEL Linux en Azure, conéctese a ella mediante SSH y luego continúe con el paso siguiente para [empezar a configurar la máquina virtual](#configure-the-hosts-file).

Si necesita crear una máquina virtual RHEL Linux o desea crear una máquina virtual de prueba para usarla con este artículo, puede utilizar uno de los métodos siguientes:

* [Azure Portal](../virtual-machines/linux/quick-create-portal.md)
* [CLI de Azure](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

Al crear la VM, preste atención a la configuración de la red virtual para asegurarse de que la VM puede comunicarse con el dominio administrado:

* Implemente la máquina virtual en la misma red virtual, o en otra emparejada, en la que haya habilitado Azure AD Domain Services.
* Implemente la VM en una subred diferente a la del dominio administrado de Azure AD Domain Services.

Una vez implementada la máquina virtual, siga los pasos para conectarse a la máquina virtual mediante SSH.

## <a name="configure-the-hosts-file"></a>Configuración del archivo hosts

Para asegurarse de que el nombre de host de la máquina virtual está configurado correctamente para el dominio administrado, edite el archivo */etc/hosts* y establezca el nombre de host:

```console
sudo vi /etc/hosts
```

En el archivo *hosts*, actualice la dirección *localhost*. En el ejemplo siguiente:

* *aaddscontoso.com* es el nombre de dominio DNS del dominio administrado.
* *rhel* es el nombre de host de la máquina virtual RHEL que se va a unir al dominio administrado.

Actualice estos nombres con sus propios valores:

```console
127.0.0.1 rhel rhel.aaddscontoso.com
```

Cuando haya terminado, guarde y salga del archivo *hosts* mediante el comando `:wq` del editor.

## <a name="install-required-packages"></a>Instalación de los paquetes requeridos

La máquina virtual necesita algunos paquetes adicionales para unirse al dominio administrado. Para instalar y configurar estos paquetes, actualice e instale las herramientas de unión a dominio mediante `yum`. Hay algunas diferencias entre RHEL 7.x y RHEL 6.x, por lo que debe usar los comandos adecuados para la versión de distribución en las secciones restantes de este artículo.

**RHEL 7**

```console
sudo yum install realmd sssd krb5-workstation krb5-libs oddjob oddjob-mkhomedir samba-common-tools
```

**RHEL 6**

```console
sudo yum install adcli sssd authconfig krb5-workstation
```

## <a name="join-vm-to-the-managed-domain"></a>Unión de una máquina virtual al dominio administrado

Ahora que los paquetes necesarios están instalados en la máquina virtual, únala al dominio administrado. De nuevo, use los pasos adecuados para la versión de distribución de RHEL.

### <a name="rhel-7"></a>RHEL 7

1. Use el comando `realm discover` para detectar el dominio administrado. En el ejemplo siguiente, se detecta el dominio kerberos *AADDSCONTOSO.COM*. Especifique su propio nombre de dominio administrado CON TODAS LAS LETRAS EN MAYÚSCULAS:

    ```console
    sudo realm discover AADDSCONTOSO.COM
    ```

   Si el comando `realm discover` no se encuentra en el dominio administrado, repase los siguientes pasos de solución de problemas:

    * Asegúrese de que el dominio sea accesible desde la máquina virtual. Pruebe `ping aaddscontoso.com` para ver si se devuelve una respuesta positiva.
    * Compruebe que la VM se ha implementado en la misma red virtual (o en otra emparejada) en la que el dominio administrado está disponible.
    * Confirme que se ha actualizado la configuración del servidor DNS de la red virtual para que apunte a los controladores de dominio del dominio administrado.

1. Ahora, inicialice Kerberos mediante el comando `kinit`. Especifique un usuario que forme parte del dominio administrado. Si es necesario, [agregue una cuenta de usuario a un grupo en Azure AD](../active-directory/fundamentals/active-directory-groups-members-azure-portal.md).

    Una vez más, el nombre del dominio administrado se debe escribir CON TODAS LAS LETRAS EN MAYÚSCULAS. En el ejemplo siguiente, la cuenta denominada `contosoadmin@aaddscontoso.com` se usa para inicializar Kerberos. Introduzca su cuenta de usuario que forma parte del dominio administrado:

    ```console
    kinit contosoadmin@AADDSCONTOSO.COM
    ```

1. Por último, una la VM al dominio administrado con el comando `realm join`. Use la misma cuenta de usuario que forma parte del dominio administrado que especificó en el comando `kinit` anterior, como `contosoadmin@AADDSCONTOSO.COM`:

    ```console
    sudo realm join --verbose AADDSCONTOSO.COM -U 'contosoadmin@AADDSCONTOSO.COM'
    ```

La máquina virtual tarda unos segundos en unirse al dominio administrado. La siguiente salida de ejemplo muestra que la máquina virtual se ha unido correctamente al dominio administrado:

```output
Successfully enrolled machine in realm
```

### <a name="rhel-6"></a>RHEL 6

1. Use el comando `adcli info` para detectar el dominio administrado. En el ejemplo siguiente se detecta el dominio kerberos *ADDDSCONTOSO.COM*. Especifique su propio nombre de dominio administrado CON TODAS LAS LETRAS EN MAYÚSCULAS:

    ```console
    sudo adcli info aaddscontoso.com
    ```

   Si el comando `adcli info` no se encuentra en el dominio administrado, repase los siguientes pasos de solución de problemas:

    * Asegúrese de que el dominio sea accesible desde la máquina virtual. Pruebe `ping aaddscontoso.com` para ver si se devuelve una respuesta positiva.
    * Compruebe que la VM se ha implementado en la misma red virtual (o en otra emparejada) en la que el dominio administrado está disponible.
    * Confirme que se ha actualizado la configuración del servidor DNS de la red virtual para que apunte a los controladores de dominio del dominio administrado.

1. En primer lugar, únase al dominio utilizando el comando `adcli join`. Este comando también crea un archivo keytab para autenticar la máquina. Use una cuenta de usuario que forme parte del dominio administrado.

    ```console
    sudo adcli join aaddscontoso.com -U contosoadmin
    ```

1. Ahora, configure el archivo `/ect/krb5.conf` y cree el archivo `/etc/sssd/sssd.conf` para que use el dominio `aaddscontoso.com` de Active Directory.
   No olvide cambiar `AADDSCONTOSO.COM` por el nombre de su dominio:

    Abra el archivo `/ect/krb5.conf` en un editor:

    ```console
    sudo vi /etc/krb5.conf
    ```

    Actualice el archivo `krb5.conf` para que sea igual que el ejemplo siguiente:

    ```console
    [logging]
     default = FILE:/var/log/krb5libs.log
     kdc = FILE:/var/log/krb5kdc.log
     admin_server = FILE:/var/log/kadmind.log
    
    [libdefaults]
     default_realm = AADDSCONTOSO.COM
     dns_lookup_realm = true
     dns_lookup_kdc = true
     ticket_lifetime = 24h
     renew_lifetime = 7d
     forwardable = true
    
    [realms]
     AADDSCONTOSO.COM = {
     kdc = AADDSCONTOSO.COM
     admin_server = AADDSCONTOSO.COM
     }
    
    [domain_realm]
     .AADDSCONTOSO.COM = AADDSCONTOSO.COM
     AADDSCONTOSO.COM = AADDSCONTOSO.COM
    ```
    
   Cree el archivo `/etc/sssd/sssd.conf`:
    
    ```console
    sudo vi /etc/sssd/sssd.conf
    ```

    Actualice el archivo `sssd.conf` para que sea igual que el ejemplo siguiente:

    ```console
    [sssd]
     services = nss, pam, ssh, autofs
     config_file_version = 2
     domains = AADDSCONTOSO.COM
    
    [domain/AADDSCONTOSO.COM]
    
     id_provider = ad
    ```

1. Compruebe que los permisos de `/etc/sssd/sssd.conf` son 600 y que son propiedad del usuario raíz:

    ```console
    sudo chmod 600 /etc/sssd/sssd.conf
    sudo chown root:root /etc/sssd/sssd.conf
    ```

1. Use `authconfig` para informar a la máquina virtual sobre la integración de AD Linux:

    ```console
    sudo authconfig --enablesssd --enablesssdauth --update
    ```

1. Inicie y habilite el servicio sssd:

    ```console
    sudo service sssd start
    sudo chkconfig sssd on
    ```

Si la máquina virtual no puede finalizar correctamente el proceso de unión al dominio, asegúrese de que el grupo de seguridad de red de la máquina virtual permita el tráfico Kerberos saliente en el puerto TCP + UDP 464 a la subred de la red virtual para el dominio administrado.

Ahora compruebe si puede consultar la información de AD del usuario utilizando `getent`

```console
sudo getent passwd contosoadmin
```

## <a name="allow-password-authentication-for-ssh"></a>Permitir autenticación de contraseña para SSH

De forma predeterminada, los usuarios solo pueden iniciar sesión en una máquina virtual mediante la autenticación basada en clave pública de SSH. La autenticación basada en contraseñas produce un error. Cuando une la máquina virtual a un dominio administrado, esas cuentas de dominio deben usar la autenticación basada en contraseñas. Actualice la configuración de SSH para permitir la autenticación basada en contraseñas como se indica a continuación.

1. Abra el archivo *sshd_conf* con un editor:

    ```console
    sudo vi /etc/ssh/sshd_config
    ```

1. Actualice la línea de *PasswordAuthentication* a *yes* (sí):

    ```console
    PasswordAuthentication yes
    ```

    Cuando haya terminado, guarde y salga del archivo *sshd_conf* mediante el comando `:wq` del editor.

1. Para aplicar los cambios y permitir que los usuarios inicien sesión con una contraseña, reinicie el servicio SSH para su versión de distribución de RHEL:

   **RHEL 7**

    ```console
    sudo systemctl restart sshd
    ```

   **RHEL 6**

    ```console
    sudo service sshd restart
    ```

## <a name="grant-the-aad-dc-administrators-group-sudo-privileges"></a>Conceda privilegios de sudo al grupo "Administradores de controlador de dominio de AAD"

Para conceder privilegios administrativos a los miembros del grupo *Administradores del controlador de dominio de AAD* en la máquina virtual RHEL, puede agregar una entrada a */etc/sudoers*. Una vez agregada, los miembros del grupo *Administradores del controlador de dominio de AAD* pueden usar el comando `sudo` en la máquina virtual RHEL.

1. Abra el archivo *sudoers* para editarlo:

    ```console
    sudo visudo
    ```

1. Agregue la siguiente entrada al final del archivo */etc/sudoers*. El grupo *Administradores del controlador de dominio de AAD* contiene un espacio en blanco en el nombre, por lo que debe incluir el carácter de escape de barra diagonal inversa en el nombre del grupo. Agregue su propio nombre de dominio, por ejemplo, *aaddscontoso.com*:

    ```console
    # Add 'AAD DC Administrators' group members as admins.
    %AAD\ DC\ Administrators@aaddscontoso.com ALL=(ALL) NOPASSWD:ALL
    ```

    Cuando haya terminado, guarde y salga del editor mediante el comando `:wq`.

## <a name="sign-in-to-the-vm-using-a-domain-account"></a>Inicio de sesión en la máquina virtual mediante una cuenta de dominio

Para comprobar que la VM se ha unido correctamente al dominio administrado, inicie una nueva conexión SSH con una cuenta de usuario de dominio. Confirme que se ha creado un directorio particular y que se ha aplicado la pertenencia a grupos del dominio.

1. Cree una nueva conexión SSH desde la consola. Use una cuenta de dominio que pertenezca al dominio administrado mediante el comando `ssh -l`, por ejemplo, `contosoadmin@aaddscontoso.com` y, a continuación, escriba la dirección de la VM, por ejemplo: *rhel.aaddscontoso.com*. Si usa Azure Cloud Shell, use la dirección IP pública de la máquina virtual en lugar del nombre DNS interno.

    ```console
    ssh -l contosoadmin@AADDSCONTOSO.com rhel.aaddscontoso.com
    ```

1. Cuando se haya conectado correctamente a la máquina virtual, compruebe que el directorio particular se ha inicializado correctamente:

    ```console
    pwd
    ```

    Debe estar en el directorio */home* con su propio directorio que coincide con la cuenta de usuario.

1. Ahora, compruebe que las pertenencias a grupos se están resolviendo correctamente:

    ```console
    id
    ```

    Debería ver las pertenencias a grupos del dominio administrado.

1. Si ha iniciado sesión en la máquina virtual como miembro del grupo *Administradores del controlador de dominio de AAD*, compruebe que puede usar correctamente el comando `sudo`:

    ```console
    sudo yum update
    ```

## <a name="next-steps"></a>Pasos siguientes

Si tiene problemas para conectar la VM al dominio administrado o al iniciar sesión con una cuenta de dominio, consulte [Solución de problemas de unión al dominio](join-windows-vm.md#troubleshoot-domain-join-issues).

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
