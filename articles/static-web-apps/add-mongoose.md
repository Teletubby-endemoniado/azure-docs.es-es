---
title: 'Tutorial: Acceso a datos en Cosmos DB mediante Mongoose con Azure Static Web Apps'
description: Obtenga información acerca de cómo acceder a datos en Cosmos DB mediante Mongoose desde una función API de Azure Static Web Apps.
author: GeekTrainer
ms.author: chrhar
ms.service: static-web-apps
ms.topic: tutorial
ms.date: 01/25/2021
ms.openlocfilehash: a048937226f979db58996eb4bf996b9f254d9182
ms.sourcegitcommit: 147910fb817d93e0e53a36bb8d476207a2dd9e5e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2021
ms.locfileid: "130132466"
---
# <a name="tutorial-access-data-in-cosmos-db-using-mongoose-with-azure-static-web-apps"></a>Tutorial: Acceso a datos en Cosmos DB mediante Mongoose con Azure Static Web Apps

[Mongoose](https://mongoosejs.com/) es el cliente de ODM (asignación de documentos de objetos) más popular para Node.js. Al permitirle diseñar una estructura de datos y aplicar la validación, Mongoose proporciona todas las herramientas necesarias para interactuar con bases de datos que admiten API de Mongoose. [Cosmos DB](../cosmos-db/mongodb-introduction.md) admite las API de Mongoose necesarias y está disponible como una opción de servidor back-end en Azure.

En este tutorial aprenderá a:

> [!div class="checklist"]
> - Creación de una cuenta sin servidor de Cosmos DB
> - Creación de una instancia de Azure Static Web Apps
> - Actualización de la configuración de la aplicación para almacenar la cadena de conexión

Si no tiene ninguna suscripción a Azure, cree una [cuenta de evaluación gratuita](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Requisitos previos

- Una [cuenta de Azure](https://azure.microsoft.com/free/)
- Una [cuenta de GitHub](https://github.com/join)

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="create-a-cosmos-db-serverless-database"></a>Creación de una base de datos sin servidor de Cosmos DB

Comience por crear una cuenta [sin servidor de Cosmos DB](../cosmos-db/serverless.md). Al usar una cuenta sin servidor, solo paga por los recursos que se usan y no es necesario crear una infraestructura completa.

1. Vaya a [https://portal.azure.com](https://portal.azure.com).
2. Haga clic en **Crear un recurso**.
3. Escriba **Azure Cosmos DB** en el cuadro de búsqueda.
4. Haga clic en **Azure Cosmos DB**.
5. Haga clic en **Crear**
6. En **Azure Cosmos DB API for MongoDB** (API de Azure Cosmos DB para MongoDB), seleccione **Crear**
7. Configure su cuenta de Azure Cosmos DB con la siguiente información:
    - Suscripción: seleccione la suscripción que desea usar.
    - Recurso: haga clic en **Crear nuevo** y establezca el nombre **aswa-mongoose**.
    - Nombre de cuenta: se requiere un valor único.
    - Ubicación: **Oeste de EE. UU. 2**.
    - Modo de capacidad: **sin servidor (versión preliminar)** .
    - Versión: **4.0**
:::image type="content" source="media/add-mongoose/cosmos-db.png" alt-text="Creación de una instancia de Cosmos DB":::
7. Haga clic en **Revisar y crear**
8. Haga clic en **Crear**

El proceso de creación tardará unos minutos. Los pasos posteriores volverán a la base de datos para recopilar la cadena de conexión.

## <a name="create-a-static-web-app"></a>Creación de una aplicación web estática

En este tutorial se usa un repositorio de plantillas de GitHub para ayudarle a crear la aplicación.

1. Vaya a la [plantilla de inicio](https://github.com/login?return_to=/staticwebdev/mongoose-starter/generate).
2. Elija el **propietario** (si usa una organización que no sea la cuenta principal).
3. Asigne al repositorio el nombre **aswa-mongoose-tutorial**.
4. Haga clic en **Create repository from template** (Crear repositorio a partir de plantilla).
5. Vuelva a [Azure Portal](https://portal.azure.com).
6. Haga clic en **Crear un recurso**.
7. Escriba **aplicación web estática** en el cuadro de búsqueda.
8. Seleccione **Static Web Apps**.
9. Haga clic en **Crear**
10. Configure la instancia de Azure Static Web Apps con la siguiente información:
    - Suscripción: elija la misma suscripción que antes.
    - Grupo de recursos: seleccione **aswa-mongoose**.
    - Nombre: **aswa-mongoose-tutorial**.
    - Región: **Oeste de EE. UU. 2**.
    - Haga clic en **Iniciar sesión con GitHub**.
    - Haga clic en **Autorizar** si se le pide que permita a Azure Static Web Apps crear la acción de GitHub para habilitar la implementación.
    - Organización: nombre de la cuenta de GitHub.
    - Repositorio: **aswa-mongoose-tutorial**.
    - Rama: **principal**.
    - Valores preestablecidos de compilación: elija **Personalizado**.
    - Ubicación de la aplicación: **/public**.
    - Ubicación de la API: **api**.
    - Ubicación de salida: *dejar en blanco*
    :::image type="content" source="media/add-mongoose/azure-static-web-apps.png" alt-text="Formulario de Azure Static Web Apps completado":::
11. Haga clic en **Revisar y crear**.
12. Haga clic en **Crear**
13. El proceso de creación tarda unos minutos; una vez aprovisionada la aplicación, puede hacer clic en **Ir al recurso**.

## <a name="configure-database-connection-string"></a>Configuración de la cadena de conexión de base de datos

Para permitir que la aplicación web se comunique con la base de datos, la cadena de conexión de la base de datos se almacena como una [configuración de la aplicación](application-settings.md). Los valores de configuración son accesibles en Node.js mediante el objeto `process.env`.

1. Haga en **Inicio** en la esquina superior izquierda de Azure Portal (o vuelva a [https://portal.azure.com](https://portal.azure.com)).
2. Haga clic en **Grupos de recursos**.
3. Haga clic en **aswa-mongoose**.
4. Haga clic en el nombre de la cuenta de base de datos; tendrá un tipo de **Azure Cosmos DB API for Mongo DB** (API de Azure Cosmos DB para Mongo DB).
5. En **Configuración**, haga clic en **Cadena de conexión**.
6. Copie la cadena de conexión que aparece en **CADENA DE CONEXIÓN PRINCIPAL**.
7. En las rutas de navegación, haga clic en **aswa-mongoose**.
8. Haga clic en **aswa-mongoose-tutorial** para volver a la instancia del sitio web.
9. En **Configuración**, haga clic en **Configuración**.
10. Haga clic en **Agregar** y cree una nueva configuración de la aplicación con los valores siguientes:
    - Nombre: **CONNECTION_STRING**.
    - Valor: pegue la cadena de conexión copiada anteriormente.
11. Haga clic en **Aceptar**
12. Haga clic en **Guardar**

## <a name="navigate-to-your-site"></a>Navegación en el sitio

Ahora puede explorar su aplicación web estática.

1. Haga clic en **Información general**.
1. Haga clic en la dirección URL que se muestra en la esquina superior derecha.
    1. Será similar a `https://calm-pond-05fcdb.azurestaticapps.net`.
1. Haga clic en **Please login to see your list of tasks** (Inicie sesión para ver la lista de tareas).
1. Haga clic en **Otorgar consentimiento** para acceder a la aplicación.
1. Para crear una nueva tarea, escriba un título y haga clic en **Agregar tarea**.
1. Confirme que se muestra la tarea (puede tardar un momento).
1. **Haga clic en la casilla** para marcar la tarea como completada.
1. **Actualice la página** para confirmar que se está utilizando una base de datos.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a seguir usando esta aplicación, elimine el grupo de recursos mediante los siguientes pasos:

1. Vuelva a [Azure Portal](https://portal.azure.com).
2. Haga clic en **Grupos de recursos**.
3. Haga clic en **aswa-mongoose**.
4. Haga clic en **Eliminar grupo de recursos**.
5. Escriba **aswa-mongoose** en el cuadro de texto.
6. Clic en **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

Pase al siguiente artículo, donde aprenderá a configurar el desarrollo local.
> [!div class="nextstepaction"]
> [Configuración del desarrollo local](./local-development.md)
