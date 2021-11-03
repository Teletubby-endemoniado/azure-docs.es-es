---
title: Asignación de roles de administrador de Azure AD con Microsoft Graph API | Microsoft Docs
description: Asignación y eliminación de roles de administrador de Azure AD con Graph API en Azure Active Directory
services: active-directory
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: how-to
ms.date: 05/14/2021
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5ec368b0bc53cb70dd948669d855670ee603b12
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131057407"
---
# <a name="assign-custom-admin-roles-using-the-microsoft-graph-api-in-azure-active-directory"></a>Asignación de roles de administrador personalizados mediante Microsoft Graph API en Azure Active Directory 

Puede automatizar la asignación de roles a las cuentas de usuario con Microsoft Graph API. En este artículo se tratan las operaciones POST, GET y DELETE en roleAssignments.

## <a name="prerequisites"></a>Requisitos previos

- Una licencia de Azure AD Premium P1 o P2
- Administrador global o administrador de roles con privilegios
- Consentimiento del administrador al usar el Probador de Graph de Microsoft Graph API

Para más información, consulte [Requisitos previos para usar PowerShell o Probador de Graph](prerequisites.md).

## <a name="post-operations-on-roleassignment"></a>Operaciones POST en RoleAssignment

### <a name="example-1-create-a-role-assignment-between-a-user-and-a-role-definition"></a>Ejemplo 1: Creación de una asignación de roles entre un usuario y una definición de roles

POST

``` HTTP
POST https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
Content-type: application/json
```

Body

``` HTTP
{
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"194ae4cb-b126-40b2-bd5b-6091b380977d",
    "directoryScopeId":"/"  // Don't use "resourceScope" attribute in Azure AD role assignments. It will be deprecated soon.
}
```

Response

``` HTTP
HTTP/1.1 201 Created
```

### <a name="example-2-create-a-role-assignment-where-the-principal-or-role-definition-does-not-exist"></a>Ejemplo 2: Creación de una asignación de roles donde la entidad de seguridad o la definición de roles no existe.

POST

``` HTTP
POST https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
```

Body

``` HTTP
{
    "principalId":" 2142743c-a5b3-4983-8486-4532ccba12869",
    "roleDefinitionId":"194ae4cb-b126-40b2-bd5b-6091b380977d",
    "directoryScopeId":"/"  //Don't use "resourceScope" attribute in Azure AD role assignments. It will be deprecated soon.
}
```

Response

``` HTTP
HTTP/1.1 404 Not Found
```
### <a name="example-3-create-a-role-assignment-on-a-single-resource-scope"></a>Ejemplo 3: Creación de una asignación de roles en un ámbito de recurso único.

POST

``` HTTP
POST https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
```

Body

``` HTTP
{
    "principalId":" 2142743c-a5b3-4983-8486-4532ccba12869",
    "roleDefinitionId":"e9b2b976-1dea-4229-a078-b08abd6c4f84",    //role template ID of a custom role
    "directoryScopeId":"/13ff0c50-18e7-4071-8b52-a6f08e17c8cc"  //object ID of an application
}
```

Response

``` HTTP
HTTP/1.1 201 Created
```

### <a name="example-4-create-an-administrative-unit-scoped-role-assignment-on-a-built-in-role-definition-which-is-not-supported"></a>Ejemplo 4: Creación de una asignación de roles con ámbito de unidad administrativa en una definición de roles integrada que no se admite.

POST

``` HTTP
POST https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
```

Body

``` HTTP
{
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"29232cdf-9323-42fd-ade2-1d097af3e4de",    //role template ID of Exchange Administrator
    "directoryScopeId":"/administrativeUnits/13ff0c50-18e7-4071-8b52-a6f08e17c8cc"    //object ID of an administrative unit
}
```

Response

``` HTTP
HTTP/1.1 400 Bad Request
{
    "odata.error":
    {
        "code":"Request_BadRequest",
        "message":
        {
            "message":"The given built-in role is not supported to be assigned to a single resource scope."
        }
    }
}
```

Solo se permite un subconjunto de roles integrados para el ámbito de la unidad administrativa. Consulte [esta documentación](admin-units-assign-roles.md) para ver la lista de roles integrados que se admiten en una unidad administrativa.

## <a name="get-operations-on-roleassignment"></a>Operaciones GET en RoleAssignment

### <a name="example-5-get-role-assignments-for-a-given-principal"></a>Ejemplo 5: Obtención de asignaciones de roles para una entidad de seguridad determinada.

GET

``` HTTP
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments?$filter=principalId+eq+'<object-id-of-principal>'
```

Response

``` HTTP
HTTP/1.1 200 OK
{
"value":[
            { 
                "id":"mhxJMipY4UanIzy2yE-r7JIiSDKQoTVJrLE9etXyrY0-1"
                "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
                "roleDefinitionId":"10dae51f-b6af-4016-8d66-8c2a99b929b3",
                "directoryScopeId":"/"  
            } ,
            {
                "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
                "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
                "roleDefinitionId":"fe930be7-5e62-47db-91af-98c3a49a38b1",
                "directoryScopeId":"/"
            }
        ]
}
```

### <a name="example-6-get-role-assignments-for-a-given-role-definition"></a>Ejemplo 6: Obtención de asignaciones de roles para una definición de rol determinada.

GET

``` HTTP
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments?$filter=roleDefinitionId+eq+'<object-id-or-template-id-of-role-definition>'
```

Response

``` HTTP
HTTP/1.1 200 OK
{
"value":[
            {
                "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
                "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
                "roleDefinitionId":"fe930be7-5e62-47db-91af-98c3a49a38b1",
                "directoryScopeId":"/"
            }
     ]
}
```

### <a name="example-7-get-a-role-assignment-by-id"></a>Ejemplo 7: Obtención de una asignación de roles por identificador.

GET

``` HTTP
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

Response

``` HTTP
HTTP/1.1 200 OK
{ 
    "id":"mhxJMipY4UanIzy2yE-r7JIiSDKQoTVJrLE9etXyrY0-1",
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"10dae51f-b6af-4016-8d66-8c2a99b929b3",
    "directoryScopeId":"/"
}
```

### <a name="example-8-get-role-assignments-for-a-given-scope"></a>Ejemplo 8: Obtención de asignaciones de roles para un ámbito determinado.


GET

``` HTTP
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments?$filter=directoryScopeId+eq+'/d23998b1-8853-4c87-b95f-be97d6c6b610'
```

Response

``` HTTP
HTTP/1.1 200 OK
{
"value":[
            { 
                "id":"mhxJMipY4UanIzy2yE-r7JIiSDKQoTVJrLE9etXyrY0-1"
                "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
                "roleDefinitionId":"10dae51f-b6af-4016-8d66-8c2a99b929b3",
                "directoryScopeId":"/d23998b1-8853-4c87-b95f-be97d6c6b610"
            } ,
            {
                "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
                "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
                "roleDefinitionId":"3671d40a-1aac-426c-a0c1-a3821ebd8218",
                "directoryScopeId":"/d23998b1-8853-4c87-b95f-be97d6c6b610"
            }
        ]
}
```

## <a name="delete-operations-on-roleassignment"></a>Operaciones DELETE en RoleAssignment

### <a name="example-9-delete-a-role-assignment-between-a-user-and-a-role-definition"></a>Ejemplo 9: Eliminación de una asignación de roles entre un usuario y una definición de roles.

Delete

``` HTTP
DELETE https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

Response
``` HTTP
HTTP/1.1 204 No Content
```

### <a name="example-10-delete-a-role-assignment-that-no-longer-exists"></a>Ejemplo 10: Eliminación de una asignación de roles que ya no existe.

Delete

``` HTTP
DELETE https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

Response

``` HTTP
HTTP/1.1 404 Not Found
```

### <a name="example-11-delete-a-role-assignment-between-self-and-global-administrator-role-definition"></a>Ejemplo 11: Eliminación de una asignación de roles entre la definición de roles propia y del administrador global.

Delete

``` HTTP
DELETE https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

Response

``` HTTP
HTTP/1.1 400 Bad Request
{
    "odata.error":
    {
        "code":"Request_BadRequest",
        "message":
        {
            "lang":"en",
            "value":"Removing self from Global Administrator built-in role is not allowed"},
            "values":null
        }
    }
}
```

Impedimos que los usuarios eliminen su propio rol de administrador global para evitar una situación donde un inquilino no tenga ningún administrador global. Se permite quitar otros roles asignados al propio.

## <a name="next-steps"></a>Pasos siguientes

* No dude en hacernos llegar sus comentarios en el [foro de roles administrativos de Azure AD](https://feedback.azure.com/d365community/forum/22920db1-ad25-ec11-b6e6-000d3a4f0789).
* Para más información acerca de los permisos de roles, consulte [Roles integrados en Azure AD](permissions-reference.md).
* Para conocer los permisos predeterminados de usuario, consulte [Comparación de los permisos predeterminados de usuario miembro e invitado](../fundamentals/users-default-permissions.md).

