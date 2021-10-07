---
title: Revisión del acceso de un paquete de acceso de administración de derechos de Azure AD
description: Aprenda a realizar una revisión de acceso de los paquetes de acceso de administración de derechos en las revisiones de acceso de Azure Active Directory.
services: active-directory
documentationCenter: ''
author: amsliu
manager: KarenH444
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 09/15/2021
ms.author: amsliu
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9b86ddd01b155b54eaa954df2a67df907901b288
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128639686"
---
# <a name="review-access-of-an-access-package-in-azure-ad-entitlement-management"></a>Revisión del acceso de un paquete de acceso de administración de derechos de Azure AD

La administración de derechos de Azure AD simplifica el modo en que las empresas administran el acceso a grupos, aplicaciones y sitios de SharePoint. En este artículo se describe cómo realizar revisiones de acceso para otros usuarios que están asignados a un paquete de acceso como revisor designado.

## <a name="prerequisites"></a>Prerrequisitos

Para revisar las asignaciones de paquetes de acceso activas de los usuarios, el creador de una revisión debe cumplir estos requisitos previos:
- Azure AD Premium P2
- Administrador global, administrador de Identity Governance o administrador de usuarios

Para obtener más información, consulte [Requisitos de licencia](entitlement-management-overview.md#license-requirements).

>[!NOTE]
>El revisor puede ser cualquier usuario que decida el creador de una revisión (propietario del grupo, administrador de usuarios, el propio usuario o cualquier usuario o grupo seleccionado).


## <a name="open-the-access-review"></a>Abrir la revisión de acceso

Siga estos pasos para buscar y abrir la revisión de acceso:

1. Es posible que reciba un correo electrónico de Microsoft en el que se le pide que revise el acceso. Busque el correo electrónico para abrir la revisión de acceso. Este es un ejemplo de correo electrónico para revisar el acceso:
    
    ![Correo electrónico del revisor de la revisión de acceso](./media/entitlement-management-access-reviews-review-access/review-access-reviewer-email.png)

1. Haga clic en el vínculo **Review user access** (Revisar acceso de usuario) para abrir la revisión de acceso. 

1. Si no tiene el correo electrónico, puede encontrar las revisiones de acceso pendientes navegando directamente a https://myaccess.microsoft.com.  (Para el Gobierno de EE. UU., utilice en su lugar `https://myaccess.microsoft.us`.)

1. Haga clic en **Revisiones de acceso** en la barra de navegación de la izquierda para ver una lista de las revisiones de acceso pendientes que tiene asignadas.
    
    ![Selección de revisiones de acceso en Mi acceso](./media/entitlement-management-access-reviews-review-access/review-access-myaccess-select-access-review.png)

1. Haga clic en la revisión que quiera empezar.
    
    ![Selección de la revisión de acceso](./media/entitlement-management-access-reviews-review-access/review-access-select-access-review.png)

## <a name="perform-the-access-review"></a>Realización de la revisión de acceso

Una vez que abra la revisión de acceso, verá los nombres de los usuarios cuyo acceso tiene que revisar. Hay dos maneras de aprobar o denegar el acceso:
- Puede aprobar o denegar manualmente el acceso de uno o más usuarios.
- Puede aceptar las recomendaciones del sistema

### <a name="manually-approve-or-deny-access-for-one-or-more-users"></a>Aprobación o denegación manual del acceso de uno o varios usuarios
1. Revise la lista de usuarios y determine los usuarios que deben seguir teniendo acceso.

    ![Lista de usuarios que hay que revisar](./media/entitlement-management-access-reviews-review-access/review-access-list-of-users.png)

1. Para aprobar o denegar el acceso, seleccione el botón de radio a la izquierda del nombre del usuario.

1. Seleccione **Aprobar** o **Denegar** en la barra situada encima de los nombres de usuario.

    ![Seleccione el usuario.](./media/entitlement-management-access-reviews-review-access/review-access-select-users.png)

1. Si no está seguro, puede hacer clic en el botón **No sé**.

    Si realiza esta selección, el usuario conserva el acceso y esta selección se incluye en los registros de auditoría. El registro muestra cualquier otro revisor cuya revisión se haya completado.

1. Es posible que deba indicar un motivo para su decisión. Escriba un motivo y haga clic en **Enviar**.

    ![Aprobación o denegación de acceso](./media/entitlement-management-access-reviews-review-access/review-access-decision-approve.png)

1. Puede cambiar la decisión en cualquier momento antes de que finalice la revisión. Para ello, seleccione el usuario en la lista y cambie la decisión. Por ejemplo, puede aprobar el acceso de un usuario que previamente ha denegado.

Si hay varios revisores, se registra la última respuesta enviada. Considere un ejemplo donde un administrador designa dos revisores: Alice y Bob. Alice abre primero la revisión y aprueba el acceso. Antes de que finalice la revisión, Bob abre la revisión y deniega el acceso. En este caso, se registra la última decisión de denegación de acceso.

>[!NOTE]
>Si a un usuario se le deniega el acceso en la revisión, no se le quita inmediatamente del paquete de acceso. Se quitará cuando se apliquen los resultados de la revisión una vez que se haya cerrado esta. La revisión se cerrará automáticamente cuando finalice su duración o puede cerrarse antes, si un administrador la detiene manualmente. 

### <a name="approve-or-deny-access-using-the-system-generated-recommendations"></a>Aprobación o denegación del acceso con las recomendaciones generadas por el sistema

Para revisar el acceso de varios usuarios más rápidamente, puede usar las recomendaciones generadas por el sistema y aceptar las recomendaciones con un solo clic. Las recomendaciones se generan en función de actividad de inicio de sesión del usuario.

1.  En la barra azul de la parte superior de la página, haga clic en **Aceptar recomendaciones**.
    
    ![Selección de Aceptar recomendaciones](./media/entitlement-management-access-reviews-review-access/review-access-use-recommendations.png)
    
    Verá un resumen de las acciones recomendadas.

1.  Haga clic en **Enviar** para aceptar las recomendaciones.

## <a name="next-steps"></a>Pasos siguientes

- [Autorrevisión de paquetes de acceso](entitlement-management-access-reviews-self-review.md)
