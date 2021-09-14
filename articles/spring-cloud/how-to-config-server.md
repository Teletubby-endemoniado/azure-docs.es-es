---
title: Configuración de una instancia de Config Server en Azure Spring Cloud
description: Aprenda a configurar una instancia de Config Server para Azure Spring Cloud en Azure Portal
ms.service: spring-cloud
ms.topic: how-to
ms.author: karler
author: karlerickson
ms.date: 10/18/2019
ms.custom: devx-track-java
ms.openlocfilehash: 0de08976f0391c995004265ac1b1a33cf4a5c491
ms.sourcegitcommit: d858083348844b7cf854b1a0f01e3a2583809649
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2021
ms.locfileid: "122835792"
---
# <a name="set-up-a-spring-cloud-config-server-instance-for-your-service"></a>Configuración de una instancia de Config Server en Spring Cloud para su servicio

**Este artículo se aplica a:** ✔️ Java ✔️ C#

En este tutorial se muestra cómo conectar una instancia de Config Server en Spring Cloud con el servicio Azure Spring Cloud.

Spring Cloud Config ofrece soporte técnico para servidor y cliente para las configuraciones externalizadas de un sistema distribuido. La instancia de Config Server es un lugar centralizado donde puede administrar las propiedades externas de las aplicaciones de todos los entornos. Para más información al respecto, consulte la [Guía de referencia de Config Server en Spring Cloud](https://spring.io/projects/spring-cloud-config).

## <a name="prerequisites"></a>Requisitos previos

* Suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
* Un servicio de Azure Spring Cloud aprovisionado y en ejecución. Para configurar e iniciar el servicio Azure Spring Cloud, consulte [Inicio rápido: Inicio de una aplicación Java Spring mediante la CLI de Azure](./quickstart.md).

## <a name="restriction"></a>Restricción

Cuando se usa Config Server con un back-end de Git, hay algunas restricciones. Algunas propiedades se insertan automáticamente en el entorno de una aplicación para acceder a Config Server y a Service Discovery. Si también configura esas propiedades desde los archivos de Config Server, pueden aparecer conflictos y un comportamiento inesperado. Estas propiedades incluyen:

```yaml
eureka.client.service-url.defaultZone
eureka.client.tls.keystore
server.port
spring.cloud.config.tls.keystore
spring.application.name
spring.jmx.enabled
```

> [!CAUTION]
> Se recomienda encarecidamente _no_ colocar las propiedades anteriores en los archivos de aplicación de Config Server.

## <a name="create-your-config-server-files"></a>Creación de los archivos del servidor de configuración

Azure Spring Cloud admite Azure DevOps, GitHub, GitLab y Bitbucket para almacenar los archivos del servidor de configuración. Cuando el repositorio esté preparado, cree los archivos de configuración con las siguientes instrucciones y almacénelos ahí.

Además, algunas propiedades configurables solo están disponibles para ciertos tipos. En las siguientes subsecciones se enumeran las propiedades de cada tipo de repositorio.

### <a name="public-repository"></a>Repositorio público

Si se usa un repositorio público, las propiedades configurables son más limitadas.

En la siguiente tabla se enumeran todas las propiedades configurables que se usan para configurar el repositorio de Git público:

> [!NOTE]
> El uso de un guion (-) para separar palabras es la única convención de nomenclatura que se admite actualmente. Por ejemplo, se puede usar *default-label*, pero no *defaultLabel*.

| Propiedad        | Obligatorio | Característica                                                      |
| :-------------- | -------- | ------------------------------------------------------------ |
| `uri`           | Sí    | El identificador URI del repositorio de Git que se usa como back-end de Config Server comienza por *http://* , *https://* , *git@* o *ssh://* . |
| `default-label` | No     | La etiqueta predeterminada del repositorio de Git debe ser el *nombre de rama*, el *nombre de etiqueta* o el  *identificador de confirmación* del repositorio. |
| `search-paths`  | No     | Matriz de cadenas que se usan para buscar en subdirectorios del repositorio de Git. |

------

### <a name="private-repository-with-ssh-authentication"></a>Repositorio privado con autenticación SSH

En la siguiente tabla se enumeran todas las propiedades configurables que se usan para configurar el repositorio de Git privado con SSH:

> [!NOTE]
> El uso de un guion (-) para separar palabras es la única convención de nomenclatura que se admite actualmente. Por ejemplo, se puede usar *default-label*, pero no *defaultLabel*.

| Propiedad                   | Obligatorio | Característica                                                      |
| :------------------------- | -------- | ------------------------------------------------------------ |
| `uri`                      | Sí    | El identificador URI del repositorio de Git que se usa como back-end de Config Server debe empezar por *http://* , *https://* , *git@* o *ssh://* . |
| `default-label`            | No     | La etiqueta predeterminada del repositorio de Git debe ser el *nombre de rama*, el *nombre de etiqueta* o el  *identificador de confirmación* del repositorio. |
| `search-paths`             | No     | Matriz de cadenas que se usa para buscar en subdirectorios del repositorio de Git. |
| `private-key`              | No     | La clave privada de SSH para acceder al repositorio de Git; se _requiere_ cuando el identificador URI comienza por *git@* o *ssh://* . |
| `host-key`                 | No     | La clave de host del servidor de repositorio de Git no debe incluir el prefijo del algoritmo, tal y como se describe en `host-key-algorithm`. |
| `host-key-algorithm`       | No     | El algoritmo de la clave de host debe ser *ssh-dss*, *ssh-rsa*, *ecdsa-sha2-nistp256*, *ecdsa-sha2-nistp384* o *ecdsa-sha2-nistp521*. *Se requiere* solo si `host-key` existe. |
| `strict-host-key-checking` | No     | Indica si la instancia de Config Server no se inicia al utilizar el valor de `host-key` de tipo privado. Debería ser *true* (valor predeterminado) o *false*. |

> [!NOTE]
> Config Server toma `master` (en el propio Git) como etiqueta predeterminada si no se especifica. Sin embargo, GitHub ha cambiado recientemente la rama predeterminada de `master` a `main`. Para evitar errores de Config Server en Azure Spring Cloud, preste atención a la etiqueta predeterminada al configurar Config Server con GitHub, especialmente para los nuevos repositorios creados.

-----

### <a name="private-repository-with-basic-authentication"></a>Repositorio privado con autenticación básica

A continuación se enumeran todas las propiedades configurables que se usan para configurar el repositorio de Git privado con autenticación básica.

> [!NOTE]
> El uso de un guion (-) para separar palabras es la única convención de nomenclatura que se admite actualmente. Por ejemplo, use *default-label*, no *defaultLabel*.

| Propiedad        | Obligatorio | Característica                                                      |
| :-------------- | -------- | ------------------------------------------------------------ |
| `uri`           | Sí    | El identificador URI del repositorio de Git que se usa como back-end de Config Server debe empezar por *http://* , *https://* , *git@* o *ssh://* . |
| `default-label` | No     | La etiqueta predeterminada del repositorio de Git debe ser el *nombre de rama*, el *nombre de etiqueta* o el  *identificador de confirmación* del repositorio. |
| `search-paths`  | No     | Matriz de cadenas que se usa para buscar en subdirectorios del repositorio de Git. |
| `username`      | No     | El nombre de usuario que se utiliza para acceder al servidor del repositorio de Git; _se requiere_ cuando el servidor del repositorio de GIT admite `Http Basic Authentication`. |
| `password`      | No     | La contraseña o el token de acceso personal que se emplea para acceder al servidor del repositorio de Git; _se requiere_ cuando el servidor del repositorio de Git admite `Http Basic Authentication`. |

> [!NOTE]
> Muchos servidores de repositorio de `Git` admiten el uso de tokens, en lugar de contraseñas, para la autenticación HTTP básica. Algunos repositorios permiten que los tokens se conserven indefinidamente. Pero algunos servidores de repositorio de Git, incluido Azure DevOps Server, obligan a que los tokens expiren en unas horas. Los repositorios que hacen que los tokens expiren no deben usar la autenticación basada en tokens con Azure Spring Cloud.
> Github ha eliminado la compatibilidad con la autenticación de contraseña, por lo que debe usar un token de acceso personal en lugar de la autenticación de contraseña en GitHub. Para obtener más información, vea [Autenticación de token](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/).

### <a name="git-repositories-with-pattern"></a>Repositorios de Git con patrón

A continuación se enumeran todas las propiedades configurables que se usan para configurar los repositorios GIT con patrones.

> [!NOTE]
> El uso de un guion (-) para separar palabras es la única convención de nomenclatura que se admite actualmente. Por ejemplo, use *default-label*, no *defaultLabel*.

| Propiedad                           | Obligatorio         | Característica                                                      |
| :--------------------------------- | ---------------- | ------------------------------------------------------------ |
| `repos`                            | No             | Mapa que consta de los valores de un repositorio de Git con un nombre determinado. |
| `repos."uri"`                      | Sí en `repos` | El identificador URI del repositorio de Git que se usa como back-end de Config Server debe empezar por *http://* , *https://* , *git@* o *ssh://* . |
| `repos."name"`                     | Sí en `repos` | Nombre que se identifica en el repositorio de Git; _se requiere_ solo si `repos` existe. Por ejemplo, *equipo-A* o *equipo-B*. |
| `repos."pattern"`                  | No             | Matriz de cadenas que se utiliza para coincidir con un nombre de aplicación. Para cada patrón, use el formato `{application}/{profile}` con caracteres comodín. |
| `repos."default-label"`            | No             | La etiqueta predeterminada del repositorio de Git debe ser el *nombre de rama*, el *nombre de etiqueta* o el  *identificador de confirmación* del repositorio. |
| `repos."search-paths`"             | No             | Matriz de cadenas que se usa para buscar en subdirectorios del repositorio de Git. |
| `repos."username"`                 | No             | El nombre de usuario que se utiliza para acceder al servidor del repositorio de Git; _se requiere_ cuando el servidor del repositorio de GIT admite `Http Basic Authentication`. |
| `repos."password"`                 | No             | La contraseña o el token de acceso personal que se emplea para acceder al servidor del repositorio de Git; _se requiere_ cuando el servidor del repositorio de Git admite `Http Basic Authentication`. |
| `repos."private-key"`              | No             | La clave privada de SSH para acceder al repositorio de Git, _se requiere_ cuando el identificador URI comienza por *git@* o *ssh://* . |
| `repos."host-key"`                 | No             | La clave de host del servidor de repositorio de Git no debe incluir el prefijo del algoritmo, tal y como se describe en `host-key-algorithm`. |
| `repos."host-key-algorithm"`       | No             | El algoritmo de la clave de host debe ser *ssh-dss*, *ssh-rsa*, *ecdsa-sha2-nistp256*, *ecdsa-sha2-nistp384* o *ecdsa-sha2-nistp521*. *Se requiere* solo si `host-key` existe. |
| `repos."strict-host-key-checking"` | No             | Indica si la instancia de Config Server no se inicia al utilizar el valor de `host-key` de tipo privado. Debería ser *true* (valor predeterminado) o *false*. |

## <a name="attach-your-config-server-repository-to-azure-spring-cloud"></a>Conexión de un repositorio de Config Server a Azure Spring Cloud

Una vez que los archivos de configuración se han guardado en un repositorio, es preciso conectarlo a Azure Spring Cloud.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. Vaya a la página **Información general** de Azure Spring Cloud.

3. Seleccione **Config Server** en el panel de navegación izquierdo.

4. En la sección **Default repository** (Repositorio predeterminado), en **URI** seleccione "https://github.com/Azure-Samples/piggymetrics-config".

5. Seleccione **Validar**.

    ![Ir al servidor de configuración](media/spring-cloud-quickstart-launch-app-portal/portal-config.png)

6. Cuando finalice la validación, seleccione **Aplicar** para guardar los cambios.

    ![Validación del servidor de configuración](media/spring-cloud-quickstart-launch-app-portal/validate-complete.png)

7. La actualización de la configuración puede tardar unos minutos.

    ![Actualización del servidor de configuración](media/spring-cloud-quickstart-launch-app-portal/updating-config.png)

8. Cuando se haya completado la configuración, debería recibir una notificación.

### <a name="enter-repository-information-directly-to-the-azure-portal"></a>Especificación de la información del repositorio directamente en Azure Portal

#### <a name="default-repository"></a>Repositorio predeterminado

* **Repositorio público**: en la sección **Repositorio predeterminado**, en el cuadro **URI**, pegue el identificador URI del repositorio.  En el campo **Etiqueta**, seleccione **config**. Asegúrese de que el valor de **Autenticación** es **Pública** y, después, seleccione **Aplicar** para finalizar.

* **Repositorio privado**: Azure Spring Cloud admite la autenticación básica basada en contraseñas o en tokens, y SSH.

    * **Autenticación básica**: en la sección **Default repository** (Repositorio predeterminado), en el cuadro **URI**, pegue el identificador URI del repositorio y, después, seleccione el botón de **autenticación** (icono de "lápiz"). En el panel **Edit Authentication** (Editar autenticación), en la lista desplegable **Authentication type** (Tipo de autenticación), seleccione **HTTP Basic** (HTTP básica) y, después, escriba el nombre de usuario y la contraseña o el token para conceder acceso a Azure Spring Cloud. Seleccione **OK** (Aceptar) y **Apply** (Aplicar) para terminar de configurar la instancia de Config Server.

    ![Autenticación básica en el panel Editar autenticación](media/spring-cloud-tutorial-config-server/basic-auth.png)

    > [!CAUTION]
    > Algunos servidores de repositorio de Git usan un *token personal* o un *token de acceso*, como una contraseña, para la **autenticación básica**. Esos tipos de token se pueden usar como contraseña en Azure Spring Cloud, ya que nunca expiran. Pero en el caso de otros servidores de repositorio de Git, como Bitbucket y Azure DevOps Server, el *token de acceso* expira en una o dos horas. Esto significa que la opción no es viable cuando se usan estos servidores de repositorio con Azure Spring Cloud.
    > Github ha eliminado la compatibilidad con la autenticación de contraseña, por lo que debe usar un token de acceso personal en lugar de la autenticación de contraseña en GitHub. Para obtener más información, vea [Autenticación de token](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/).

    * **SSH**: en la sección **Default repository** (Repositorio predeterminado), en el cuadro **URI**, pegue el identificador URI del repositorio y, después, seleccione el botón de **autenticación** (icono de "lápiz"). En el panel **Edit Authentication** (Editar autenticación), en la lista desplegable **Authentication type** (Tipo de autenticación), seleccione **SSH** y, después, escriba su **clave privada**. También puede especificar los valores de **Host key** (Clave de host) y **Host key algorithm** (Algoritmo de claves de host). Asegúrese de incluir la clave pública en el repositorio de Config Server. Seleccione **OK** (Aceptar) y **Apply** (Aplicar) para terminar de configurar la instancia de Config Server.

    ![Autenticación SSH en el panel Editar autenticación](media/spring-cloud-tutorial-config-server/ssh-auth.png)

#### <a name="pattern-repository"></a>Repositorio de patrones

Si quiere usar un **Repositorio de patrones** opcional para configurar el servicio, especifique el **URI** y la **Autenticación** de la misma manera que el **Repositorio predeterminado**. En **Name** (Nombre), asigne un nombre al patrón y, después, seleccione **Apply** (Aplicar) para asociarlo a la instancia.

### <a name="enter-repository-information-into-a-yaml-file"></a>Introducción de la información del repositorio en un archivo YAML

Si ha escrito un archivo YAML con la configuración del repositorio, puede importarlo directamente desde la máquina local a Azure Spring Cloud. Un archivo YAML simple para un repositorio privado con autenticación básica tendrá un aspecto similar al siguiente:

```yaml
spring:
    cloud:
        config:
            server:
                git:
                    uri: https://github.com/azure-spring-cloud-samples/config-server-repository.git
                    username: <username>
                    password: <password/token>

```

Seleccione el botón **Import settings** (Importar configuración) y, después, seleccione el archivo YAML en el directorio del proyecto. Seleccione **Import** (Importar) y aparecerá una de las `async`operaciones de **Notifications** (Notificaciones). Después de uno o dos minutos, debería informar de que se ha realizado correctamente.

![Panel Notifications (Notificaciones) de Config Server](media/spring-cloud-tutorial-config-server/local-yml-success.png)

La información del archivo YAML debería mostrarse en Azure Portal. Seleccione **Apply** (Aplicar) para finalizar.

## <a name="using-azure-repos-for-azure-spring-cloud-configuration"></a>Uso de Azure Repos para la configuración de Azure Spring Cloud

Azure Spring Cloud puede acceder a los repositorios de Git públicos, protegidos mediante SSH o protegidos mediante la autenticación HTTP básica. Usaremos esa última opción, ya que es más fácil de crear y administrar con Azure Repos.

### <a name="get-repo-url-and-credentials"></a>Obtención de URL y credenciales del repositorio

1. En el portal de Azure Repos de su proyecto, seleccione el botón **Clonar**:

    ![Botón Clonar](media/spring-cloud-tutorial-config-server/clone-button.png)

1. Copie la dirección URL de clonación del cuadro de texto. Esta dirección URL normalmente tendrá el siguiente formato:

    ```text
    https://<organization name>@dev.azure.com/<organization name>/<project name>/_git/<repository name>
    ```

    Quite todo lo que hay después de `https://` y antes de `dev.azure.com`, incluido `@`. La dirección URL resultante debe tener este formato:

    ```text
    https://dev.azure.com/<organization name>/<project name>/_git/<repository name>
    ```

    Guarde esta dirección URL para usarla en la siguiente sección.

1. Seleccione **Generar credenciales de GIT**. Aparecerá un nombre de usuario y una contraseña. Guárdelos para su uso en la sección siguiente.

### <a name="configure-azure-spring-cloud-to-access-the-git-repository"></a>Configuración de Azure Spring Cloud para acceder al repositorio de GIT

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Vaya a la página **Información general** de Azure Spring Cloud.

1. Seleccione el servicio que desea configurar.

1. En el panel izquierdo de la página del servicio, en **Settings** (Configuración), seleccione la pestaña **Config Server** (Servidor de configuraciones). Configure el repositorio que hemos creado anteriormente:

   - Agregue la dirección URL del repositorio que ha guardado en la sección anterior.
   - Seleccione **Autenticación** y, a continuación, **Básica HTTP**.
   - __username__ es el nombre de usuario que se ha guardado en la sección anterior.
   - __password__ es la contraseña que se ha guardado en la sección anterior.
   - Seleccione **Aplicar** y espere a que la operación se realice correctamente.

   ![Servidor de Spring Cloud Config](media/spring-cloud-tutorial-config-server/config-server-azure-repos.png)

## <a name="delete-your-configuration"></a>Eliminación de la configuración

Puede seleccionar el botón **Restablecer** que aparece en la pestaña **Config Server** para borrar completamente la configuración existente. Elimine la configuración de Config Server si desea conectar la instancia de Config Server a otro origen, por ejemplo, para pasarla de GitHub a Azure DevOps.

## <a name="config-server-refresh"></a>Actualización de Config Server

Cuando se cambian las propiedades, es necesario notificarlo a los servicios que las consumen antes de que se puedan realizar cambios. La solución predeterminada para Spring Cloud Config es desencadenar manualmente el [evento de actualización](https://spring.io/guides/gs/centralized-configuration/), lo que puede no ser factible si hay muchas instancias de aplicación. Como alternativa, en Azure Spring Cloud puede actualizar automáticamente los valores del servidor de configuración al dejar que el cliente de configuración sondee los cambios en función de una actualización interna.

- En primer lugar, incluya spring-cloud-starter-azure-spring-cloud-client en la sección de dependencias del archivo pom.xml.

  ```xml
  <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
      <version>2.4.0</version>
  </dependency>
  ```

- En segundo lugar, habilite la actualización automática y establezca el intervalo de actualización adecuado en application.yml. En este ejemplo, el cliente sondeará los cambios de configuración cada 5 segundos, que es el valor mínimo que puede establecer para el intervalo de actualización.
De forma predeterminada, la actualización automática se establece en false y el intervalo de actualización se establece en 60 segundos.

  ```yml
  spring:
    cloud:
      config:
        auto-refresh: true
        refresh-interval: 5
  ```

- Por último, agregue @refreshScope en el código. En este ejemplo, la variable connectTimeout se actualizará automáticamente cada 5 segundos.

  ```java
  @RestController
  @RefreshScope
  public class HelloController {
      @Value("${timeout:4000}")
      private String connectTimeout;
  }
  ```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a habilitar y configurar una instancia de Config Server de Spring Cloud. Para más información sobre la administración de la aplicación, consulte [Escalado de una aplicación en Azure Spring Cloud](./how-to-scale-manual.md).
