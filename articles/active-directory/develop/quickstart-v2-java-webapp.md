---
title: 'Inicio rápido: Incorporación del inicio de sesión con Microsoft a una aplicación web de Java | Azure'
titleSuffix: Microsoft identity platform
description: En este inicio rápido, aprenderá a agregar el inicio de sesión con Microsoft en una aplicación web de Java mediante OpenID Connect.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: portal
ms.workload: identity
ms.date: 11/22/2021
ROBOTS: NOINDEX
ms.author: marsma
ms.custom: aaddev, "scenarios:getting-started", "languages:Java", devx-track-java, mode-api
ms.openlocfilehash: fa85851bba852448550aa3a4c392a329af012eaf
ms.sourcegitcommit: 3f20f370425cb7d51a35d0bba4733876170a7795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2022
ms.locfileid: "137804900"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-a-java-web-app"></a>Inicio rápido: Adición de inicio de sesión con Microsoft a una aplicación web de Java

En este inicio rápido descargará y ejecutará un código de ejemplo que muestra cómo una aplicación web de Java puede realizar el inicio de sesión de usuarios y llamar a Microsoft Graph API. Los usuarios de cualquier organización de Azure Active Directory (Azure AD) pueden iniciar sesión en la aplicación.

 Para obtener información general, consulte el [diagrama del funcionamiento del ejemplo](#how-the-sample-works).

## <a name="prerequisites"></a>Requisitos previos

Para ejecutar esta muestra, necesita:

- [Kit de desarrollo de Java (JDK)](https://openjdk.java.net/) 8 o superior.
- [Maven](https://maven.apache.org/).


#### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal

Para usar el código de ejemplo en este inicio rápido:

1. Agregar las direcciones URL de respuesta `https://localhost:8443/msal4jsample/secure/aad` y `https://localhost:8443/msal4jsample/graph/me`.
1. Crear un secreto de cliente.
> [!div class="nextstepaction"]
> [Realizar estos cambios por mí]()

> [!div class="alert alert-info"]
> ![Ya configurada](media/quickstart-v2-aspnet-webapp/green-check.png) La aplicación está configurada con estos atributos.

#### <a name="step-2-download-the-code-sample"></a>Paso 2: Descargar el código de ejemplo

Descargue el proyecto y extraiga el archivo .zip en una carpeta cerca de la raíz de la unidad. Por ejemplo, *C:\Azure-Samples*.

Para usar HTTPS con localhost, especifique las propiedades de `server.ssl.key`. Para generar un certificado autofirmado, use la utilidad keytool (que se incluye en JRE).

Este es un ejemplo:
```
  keytool -genkeypair -alias testCert -keyalg RSA -storetype PKCS12 -keystore keystore.p12 -storepass password

  server.ssl.key-store-type=PKCS12
  server.ssl.key-store=classpath:keystore.p12
  server.ssl.key-store-password=password
  server.ssl.key-alias=testCert
  ```
  Coloque el archivo del almacén de claves generado en la carpeta *resources*.

> [!div class="sxs-lookup nextstepaction"]
> [Descargar el código de ejemplo](https://github.com/Azure-Samples/ms-identity-java-webapp/archive/master.zip)

> [!div class="sxs-lookup"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div class="sxs-lookup"]

#### <a name="step-3-run-the-code-sample"></a>Paso 3: Ejecución del ejemplo de código

Para ejecutar el proyecto, realice uno de estos pasos:

- Ejecútelo directamente desde el IDE, para lo que debe usar el servidor de Spring Boot insertado.
- Empaquételo en un archivo WAR mediante [Maven](https://maven.apache.org/plugins/maven-war-plugin/usage.html) y, a continuación, impleméntelo en una solución de contenedor de J2EE como [Apache Tomcat](http://tomcat.apache.org/).

##### <a name="running-the-project-from-an-ide"></a>Ejecución del proyecto desde un entorno de desarrollo integrado

Para ejecutar la aplicación web en un entorno de desarrollo integrado, seleccione la ejecución y vaya a la página principal del proyecto. En este ejemplo, la dirección URL de la Página principal estándar es https://localhost:8443.

1. En la página de información, seleccione el botón **Inicio de sesión** para redirigir los usuarios a Azure Active Directory y solicite a los usuarios sus credenciales.

1. Una vez autenticados los usuarios, se les redirige a `https://localhost:8443/msal4jsample/secure/aad`. Ya han iniciado sesión y la página mostrará información sobre la cuenta de usuario. La interfaz de usuario de ejemplo tiene estos botones:
    - **Sign Out** (Cerrar sesión): Cierra la sesión del usuario actual en la aplicación y lo redirige a la página principal.
    - **Mostrar información de usuario**: Adquiere un token para Microsoft Graph y llama a Microsoft Graph con una solicitud que contiene el token, que devuelve información básica sobre el usuario que inició sesión.

##### <a name="running-the-project-from-tomcat"></a>Ejecución del proyecto desde Tomcat

Si desea implementar el ejemplo web en Tomcat, realice un par de cambios en el código fuente.

1. Abra *ms-identity-java-webapp/src/main/java/com.microsoft.azure.msalwebsample/MsalWebSampleApplication*.

    - Elimine todo el código fuente y reemplácelo por este:

      ```Java
       package com.microsoft.azure.msalwebsample;

       import org.springframework.boot.SpringApplication;
       import org.springframework.boot.autoconfigure.SpringBootApplication;
       import org.springframework.boot.builder.SpringApplicationBuilder;
       import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

       @SpringBootApplication
       public class MsalWebSampleApplication extends SpringBootServletInitializer {

        public static void main(String[] args) {
         SpringApplication.run(MsalWebSampleApplication.class, args);
        }

        @Override
        protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
         return builder.sources(MsalWebSampleApplication.class);
        }
       }
      ```

2.   El puerto HTTP predeterminado de Tomcat es el 8080, pero se necesita una conexión HTTPS en el puerto 8443. Para configurar este valor:
        - Vaya a *tomcat/conf/server.xml*.
        - Busque la etiqueta `<connector>` y reemplace el conector existente por este:

          ```xml
          <Connector
                   protocol="org.apache.coyote.http11.Http11NioProtocol"
                   port="8443" maxThreads="200"
                   scheme="https" secure="true" SSLEnabled="true"
                   keystoreFile="C:/Path/To/Keystore/File/keystore.p12" keystorePass="KeystorePassword"
                   clientAuth="false" sslProtocol="TLS"/>
          ```

3. Abra una ventana de símbolo del sistema. Vaya a la carpeta raíz de este ejemplo (donde se encuentra el archivo pom.xml) y ejecute `mvn package` para compilar el proyecto.
    - Este comando generará el archivo *msal-web-sample-0.1.0.war* en el directorio */targets*.
    - Cambie el nombre de este archivo a *msal4jsample.war*.
    - Implemente el archivo .WAR mediante Tomcat, o cualquier otra solución de contenedor de J2EE.
        - Para implementar el archivo msal4jsample.war, cópielo en el directorio */webapps* de la instalación de Tomcat e inicie el servidor de Tomcat.

4. Una vez implementado el archivo, vaya a https://localhost:8443/msal4jsample mediante un explorador.

> [!IMPORTANT]
> Esta aplicación de inicio rápido usa un secreto de cliente para identificarse como un cliente confidencial. Dado que el secreto de cliente se agrega como texto sin formato a los archivos del proyecto, por motivos de seguridad se recomienda utilizar un certificado, en lugar de un secreto de cliente, antes de usar la aplicación en un entorno de producción. Para más información sobre cómo usar un certificado, consulte [Credenciales de certificado para la autenticación de la aplicación](./active-directory-certificate-credentials.md).

## <a name="more-information"></a>Más información

### <a name="how-the-sample-works"></a>Funcionamiento del ejemplo
![Diagrama que muestra el funcionamiento de la aplicación de ejemplo que se ha generado en este inicio rápido.](media/quickstart-v2-java-webapp/java-quickstart.svg)

### <a name="get-msal"></a>Obtención de MSAL

MSAL for Java (MSAL4J) es la biblioteca de Java que se usa para que inicien sesión los usuarios y solicitar los tokens que se usan para acceder a una API que está protegida por la plataforma de Microsoft Identity.

Agregue MSAL4J a la aplicación con Maven o Gradle para administrar las dependencias al realizar los cambios siguientes en el archivo pom.xml (Maven) o build.gradle (Gradle) de la aplicación.

En pom.xml:

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>msal4j</artifactId>
    <version>1.0.0</version>
</dependency>
```

En build.gradle:

```$xslt
compile group: 'com.microsoft.azure', name: 'msal4j', version: '1.0.0'
```

### <a name="initialize-msal"></a>Inicializar MSAL

Agregue una referencia a MSAL for Java, para lo que debe incorporar el código siguiente al principio del archivo en el que va a usar MSAL4J:

```Java
import com.microsoft.aad.msal4j.*;
```

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Pasos siguientes

Para obtener información más detallada sobre la compilación de aplicaciones web que inicien sesión de los usuarios en la plataforma de identidad de Microsoft, consulte la serie de escenarios de varias partes:

> [!div class="nextstepaction"]
> [Escenario: Aplicación web que permite iniciar sesión a los usuarios](scenario-web-app-sign-user-overview.md?tabs=java)
