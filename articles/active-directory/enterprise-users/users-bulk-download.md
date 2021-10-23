---
title: Descargar una lista de usuarios en el portal de Azure Active Directory | Microsoft Docs
description: Descargue los registros de usuarios de forma masiva en el Centro de administración de Azure en Azure Active Directory.
services: active-directory
author: curtand
ms.author: curtand
manager: KarenH444
ms.date: 01/04/2021
ms.topic: how-to
ms.service: active-directory
ms.subservice: enterprise-users
ms.workload: identity
ms.custom: it-pro
ms.reviewer: krbain
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5f094bfa6f7be93631d04c3e14c91b2ad47fefa9
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "129985397"
---
# <a name="download-a-list-of-users-in-azure-active-directory-portal"></a>Descarga de una lista de usuarios en el portal de Azure Active Directory

Azure Active Directory (Azure AD) admite operaciones (creación) de importación masiva de usuarios.

## <a name="required-permissions"></a>Permisos necesarios

Para descargar la lista de usuarios del Centro de administración de Azure AD, debe haber iniciado sesión con un usuario asignado a uno o más roles de administrador de nivel de organización en Azure AD (el administrador de usuario es el rol mínimo necesario). Los invitadores de usuarios invitados y los desarrolladores de aplicaciones no se consideran roles de administrador.

## <a name="to-download-a-list-of-users"></a>Para descargar una lista de usuarios

1. [Inicie sesión en la organización de Azure AD](https://aad.portal.azure.com) con una cuenta de administrador de usuarios de la organización.
2. Vaya a Azure Active Directory > Usuarios. A continuación, seleccione los usuarios que desea incluir en la descarga. Para ello, active la casilla en la columna izquierda junto a cada usuario. Nota: En este momento, no hay ninguna manera de seleccionar todos los usuarios para la exportación. Cada una de ellas debe estar seleccionada de manera individual.
3. En Azure AD, seleccione **Usuarios** > **Descargar usuarios**.
4. En la página **Descargar usuarios**, seleccione **Iniciar** para recibir un archivo .csv que enumera las propiedades de perfil del usuario. Si hay errores, puede descargar y ver el archivo de resultados en la página de resultados de la operación masiva. El archivo contiene el motivo de cada error.

   ![Seleccione dónde quiere la lista de usuarios que quiere descargar.](./media/users-bulk-download/bulk-download.png)
   
>[!NOTE]
>El archivo de descarga contendrá la lista filtrada de usuarios en función del ámbito de los filtros aplicados.

   Se incluyen los atributos de usuario siguientes:

   - userPrincipalName
   - DisplayName
   - surname
   - mail
   - givenName
   - objectId
   - userType
   - jobTitle
   - department
   - accountEnabled
   - usageLocation
   - streetAddress
   - state
   - country
   - physicalDeliveryOfficeName
   - city
   - postalCode
   - telephoneNumber
   - mobile
   - authenticationAlternativePhoneNumber
   - authenticationEmail
   - alternateEmailAddress
   - ageGroup
   - consentProvidedForMinor
   - legalAgeGroupClassification

## <a name="check-status"></a>Comprobar estado

Puede ver el estado de las solicitudes masivas pendientes en la página **Resultados de la operación masiva**.

[![Comprobación del estado en la página Resultados de la operación masiva.](./media/users-bulk-download/bulk-center.png)](./media/users-bulk-download/bulk-center.png#lightbox)

## <a name="bulk-download-service-limits"></a>Límites del servicio de descarga de forma masiva

La ejecución de cada actividad en bloque para crear una lista de usuarios puede durar hasta una hora. Esto permite la creación y descarga de una lista de hasta 500 000 usuarios.

## <a name="next-steps"></a>Pasos siguientes

- [Adición masiva de usuarios](users-bulk-add.md)
- [Eliminación masiva de usuarios](users-bulk-delete.md)
- [Restauración de usuarios masiva](users-bulk-restore.md)
