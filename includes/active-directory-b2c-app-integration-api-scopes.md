---
author: kengaderdus
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 08/04/2021
ms.author: kengaderdus
ms.openlocfilehash: 83ca731da109b945a654fd36406f4c523c957ef7
ms.sourcegitcommit: 91915e57ee9b42a76659f6ab78916ccba517e0a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2021
ms.locfileid: "131007629"
---
1. Seleccione la aplicación **my-api1** que creó (**id. de aplicación: 2**) para abrir la página **Información general**.

1. En **Administrar**, seleccione **Exponer una API**.
1. Junto a **URI de id. de aplicación**, seleccione el vínculo **Establecer**. Reemplace el valor predeterminado (GUID) por un nombre único (por ejemplo, **tasks-api**) y, luego, seleccione **Guardar**. 
 
   Cuando la aplicación web solicite un token de acceso para la API web, deberá agregar este URI como prefijo para cada ámbito que se defina para la API.
1. En **Ámbitos definidos con esta API**, seleccione **Agregar un ámbito**.
1. Para crear un ámbito que defina el acceso de lectura a la API, siga estos pasos:

    1. Para **Nombre de ámbito**, escriba **tasks.read**.  
    1. Para **Nombre para mostrar del consentimiento del administrador**, escriba **Acceso de lectura a la API de tareas**.  
    1. Para **Descripción del consentimiento del administrador**, escriba **Permite el acceso de lectura a la API de tareas**.

1. Seleccione la opción **Agregar un ámbito**.

1. Seleccione **Agregar un ámbito** y agregue una opción que defina el acceso de escritura en la API: 

    1. Para **Nombre de ámbito**, escriba **tasks.write**.  
    1. Para **Nombre para mostrar del consentimiento del administrador**, escriba **Acceso de escritura a la API de tareas**.
    1. Para **Descripción del consentimiento del administrador**, escriba **Permite el acceso de escritura a la API de tareas**.
    
1. Seleccione la opción **Agregar un ámbito**.
