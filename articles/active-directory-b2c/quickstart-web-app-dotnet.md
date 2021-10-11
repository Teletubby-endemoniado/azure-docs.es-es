---
title: 'Inicio rápido: Configuración del inicio de sesión de una aplicación web de ASP.NET'
titleSuffix: Azure AD B2C
description: En este inicio rápido, se ejecuta una aplicación web de ASP.NET de ejemplo que usa Azure Active Directory B2C para proporcionar el inicio de sesión de las cuentas.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.topic: quickstart
ms.custom: devx-track-csharp, mvc
ms.date: 10/01/2021
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 3c39780f1f8e84ed3fe58973f46274bad727d10e
ms.sourcegitcommit: 87de14fe9fdee75ea64f30ebb516cf7edad0cf87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2021
ms.locfileid: "129351380"
---
# <a name="quickstart-set-up-sign-in-for-an-aspnet-application-using-azure-active-directory-b2c"></a>Inicio rápido: Configuración del inicio de sesión en una aplicación ASP.NET con Azure Active Directory B2C

Azure Active Directory B2C (Azure AD B2C) proporciona administración de identidades en la nube para mantener la protección de su aplicación, empresa y clientes. Azure AD B2C permite que las aplicaciones puedan autenticarse con cuentas de redes sociales y cuentas de empresa mediante protocolos estándar abiertos. 

En esta guía de inicio rápido, se utiliza una aplicación de ASP.NET para iniciar sesión mediante un proveedor de identidades de redes sociales y llamar a una API web protegida por Azure AD B2C.



## <a name="prerequisites"></a>Requisitos previos

- [Visual Studio 2019](https://www.visualstudio.com/downloads/) con la carga de trabajo **ASP.NET y desarrollo web**.
- Una cuenta de redes sociales de Facebook, Google o Microsoft.
- [Descargue un archivo zip](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) o clone la aplicación web de ejemplo desde GitHub.

    ```
    git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
    ```

    Hay dos proyectos en la solución de ejemplo:

    - **TaskWebApp**: una aplicación web que crea y edita una lista de tareas. La aplicación web utiliza el flujo de usuario de **registro o de inicio de sesión** para el registro o el inicio de sesión.
    - **TaskService**: API web que admite la funcionalidad de creación, lectura, actualización y eliminación de la lista de tareas. Azure AD B2C protege la API web y la aplicación web la llama.

## <a name="run-the-application-in-visual-studio"></a>Ejecución de la aplicación en Visual Studio

1. En la carpeta de proyecto de la aplicación de ejemplo, abra la solución **B2C-WebAPI-DotNet.sln** en Visual Studio.
2. Para esta guía de inicio rápido, debe ejecutar los proyectos **TaskWebApp** y **TaskService** al mismo tiempo. Haga clic con el botón derecho en la solución **B2C-WebAPI-DotNet** del Explorador de soluciones y, a continuación, seleccione **Establecer proyectos de inicio**.
3. Seleccione **Proyectos de inicio múltiples** y cambie la **Acción** de ambos proyectos a **Inicio**.
4. Seleccione **Aceptar**.
5. Presione **F5** para depurar las dos aplicaciones. Cada aplicación se abre en su propia pestaña del explorador:

    - `https://localhost:44316/`: la aplicación web ASP.NET. Interactúe directamente con esta aplicación en la guía de inicio rápido.
    - `https://localhost:44332/`: la API web a la que llama la aplicación web ASP.NET.

## <a name="sign-in-using-your-account"></a>Inicio de sesión mediante su cuenta

1. Haga clic en **Registro o inicio de sesión** en la aplicación web ASP.NET para iniciar el flujo de trabajo.

    ![Ejemplo de aplicación web de ASP.NET en el explorador con el vínculo de registro y firma resaltado](./media/quickstart-web-app-dotnet/web-app-sign-in.png)

    El ejemplo admite varias opciones de registro: usar un proveedor de identidades de redes sociales o crear una cuenta local con una dirección de correo electrónico. Para este inicio rápido, use una cuenta de proveedor de identidades sociales de Facebook, Google o Microsoft.

2. Azure AD B2C presenta una página de inicio de sesión para una empresa ficticia llamada Fabrikam para la aplicación web de ejemplo. Para registrarse con un proveedor de identidades de redes sociales, seleccione el botón del proveedor de identidades que desee usar.

    ![Página de inicio de sesión o registro que muestra los botones del proveedor de identidades](./media/quickstart-web-app-dotnet/sign-in-or-sign-up-web.png)

    Debe autenticarse (iniciar sesión) con las credenciales de su cuenta de redes sociales y autorizar a la aplicación para que lea la información de su cuenta de redes sociales. Al conceder acceso, la aplicación puede recuperar la información del perfil de la cuenta de redes sociales como el nombre y la ciudad.

3. Finalice el proceso de inicio de sesión para el proveedor de identidades.

## <a name="edit-your-profile"></a>Edición del perfil

Azure Active Directory B2C proporciona funcionalidad para permitir a los usuarios actualizar sus perfiles. La aplicación web de ejemplo usa un flujo de usuario del perfil de edición de Azure AD B2C para el flujo de trabajo.

1. En la barra de menús de la aplicación, haga clic en el nombre de perfil y seleccione **Editar perfil** para editar el perfil que ha creado.

    ![Aplicación web de ejemplo en el explorador con el vínculo Editar perfil resaltado](./media/quickstart-web-app-dotnet/edit-profile-web.png)

2. Cambie su **nombre para mostrar** o la **ciudad** y, a continuación, haga clic en **Continuar** para actualizar el perfil.

    Los datos cambiados aparecen en la parte superior derecha de la página principal de la aplicación web.

## <a name="access-a-protected-api-resource"></a>Acceso a un recurso de API protegido

1. Haga clic en la **lista de tareas pendientes** para escribir y modificar los elementos de la lista de tareas pendientes.

2. En el cuadro de texto **Nuevo elemento**, escriba el texto. Haga clic en **Agregar** para llamar a la API web protegida de Azure AD B2C que agrega un elemento de la lista de tareas pendientes.

    ![Aplicación web de ejemplo en el explorador con la opción para agregar un elemento de lista de tareas](./media/quickstart-web-app-dotnet/add-todo-item-web.png)

    La aplicación web ASP.NET incluye un token de acceso de Azure AD en la solicitud al recurso de API web protegido para realizar operaciones en los elementos de la lista de tareas pendientes del usuario.

Ha utilizado correctamente su cuenta de usuario de Azure AD B2C para realizar una llamada autorizada a una API web de Azure AD B2C protegida.


## <a name="next-steps"></a>Pasos siguientes

[Creación de un inquilino de Azure Active Directory B2C en Azure Portal](tutorial-create-tenant.md)
