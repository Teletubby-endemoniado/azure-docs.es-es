---
title: 'Delegación de roles por tarea de administrador: Azure Active Directory | Microsoft Docs'
description: Roles a delegar para tareas de identidad en Azure Active Directory
services: active-directory
documentationcenter: ''
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: reference
ms.date: 11/05/2020
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: df3c243970709ad7ae4bc52f179117affc2d8613
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124776442"
---
# <a name="administrator-roles-by-admin-task-in-azure-active-directory"></a>Roles de administrador por tarea de administrador en Azure Active Directory

En este artículo, puede encontrar la información necesaria para restringir los permisos de administrador de un usuario mediante la asignación de roles con privilegios mínimos en Azure Active Directory (Azure AD). Encontrará las tareas de administrador organizadas por área de características y el rol con privilegios mínimos necesario para realizar cada tarea, junto con roles de administrador no global que pueden realizar la tarea.

## <a name="application-proxy"></a>Proxy de aplicación

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Configurar aplicación de proxy de aplicación | Administrador de aplicaciones |  |
> | Configurar propiedades del grupo de conectores | Administrador de aplicaciones |  |
> | Crear registro de aplicación cuando se deshabilita la capacidad para todos los usuarios | Desarrollador de aplicaciones | Administrador de aplicaciones en la nube<br/>Administrador de aplicaciones |
> | Crear grupo de conectores | Administrador de aplicaciones |  |
> | Eliminar grupo de conectores | Administrador de aplicaciones |  |
> | Deshabilitar el proxy de aplicación | Administrador de aplicaciones |  |
> | Descargar servicio de conector | Administrador de aplicaciones |  |
> | Leer toda la configuración | Administrador de aplicaciones |  |

## <a name="external-identitiesb2c"></a>Identidades externas o B2C

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Crear directorios de Azure AD B2C | Todos los usuarios que no sean invitados ([consulte la documentación](../fundamentals/users-default-permissions.md)) |  |
> | Crear aplicaciones B2C | Administrador global |  |
> | Crear aplicaciones empresariales | Administrador de aplicaciones en la nube | Administrador de aplicaciones |
> | Crear, leer, actualizar y eliminar directivas de B2C | Administrador de directivas B2C con IEF |  |
> | Crear, leer, actualizar y eliminar proveedores de identidades | Administrador de proveedor de identidades externo |  |
> | Crear, leer, actualizar y eliminar flujos de usuario de restablecimiento de contraseña | Administrador de flujos de usuarios con id. externo |  |
> | Crear, leer, actualizar y eliminar flujos de usuario de edición de perfiles | Administrador de flujos de usuarios con id. externo |  |
> | Crear, leer, actualizar y eliminar flujos de usuario de inicio de sesión | Administrador de flujos de usuarios con id. externo |  |
> | Crear, leer, actualizar y eliminar flujos de usuario de registro |Administrador de flujos de usuarios con id. externo |  |
> | Crear, leer, actualizar y eliminar atributos de usuario | Administrador de atributos de flujos de usuarios con id. externo |  |
> | Crear, leer, actualizar y eliminar usuarios | Administrador de usuarios |  |
> | Configuración de los valores de colaboración externa B2B | Administrador global |  |
> | Leer toda la configuración | Lector global |  |
> | Leer registros de auditoría de B2C | Lector global ([consulte la documentación](../../active-directory-b2c/faq.yml)) |  |

> [!NOTE]
> Los administradores globales de Azure AD B2C no tienen los mismos permisos que los administradores globales de Azure AD. Si tiene privilegios de administrador global de Azure AD B2C, asegúrese de que se encuentra en un directorio de Azure AD B2C, no de Azure AD.

## <a name="company-branding"></a>Personalización de marca de empresa

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Configuración de la personalización de marca de la compañía | Administrador global |  |
> | Leer toda la configuración | Lectores de directorios | Rol de usuario predeterminado ([consulte la documentación](../fundamentals/users-default-permissions.md)) |

## <a name="company-properties"></a>Propiedades de la empresa

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Configurar propiedades de la empresa | Administrador global |  |

## <a name="connect"></a>Conectar

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Autenticación de paso a través | Administrador global |  |
> | Leer toda la configuración | Lector global | Administrador global |
> | Inicio de sesión único de conexión directa | Administrador global |  |

## <a name="cloud-provisioning"></a>Aprovisionamiento en la nube

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Autenticación de paso a través | Administrador de identidades híbridas |  |
> | Leer toda la configuración | Lector global | Administrador de identidades híbridas |
> | Inicio de sesión único de conexión directa | Administrador de identidades híbridas |  |

## <a name="connect-health"></a>Connect Health

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Agregar o eliminar servicios | Propietario ([consulte la documentación](../hybrid/how-to-connect-health-operations.md)) |  |
> | Aplicar correcciones de errores de sincronización | Colaborador ([consulte la documentación](../fundamentals/users-default-permissions.md?context=azure%2factive-directory%2fusers-groups-roles%2fcontext%2fugr-context)) | Propietario |
> | Configuración de notificaciones | Colaborador ([consulte la documentación](../fundamentals/users-default-permissions.md?context=azure%2factive-directory%2fusers-groups-roles%2fcontext%2fugr-context)) | Propietario |
> | Definición de configuración | Propietario ([consulte la documentación](../hybrid/how-to-connect-health-operations.md)) |  |
> | Configurar notificaciones de sincronización | Colaborador ([consulte la documentación](../fundamentals/users-default-permissions.md?context=azure%2factive-directory%2fusers-groups-roles%2fcontext%2fugr-context)) | Propietario |
> | Leer informes de seguridad de ADFS | Lector de seguridad | Colaborador<br/>Propietario
> | Leer toda la configuración | Lector ([consulte la documentación](../fundamentals/users-default-permissions.md?context=azure%2factive-directory%2fusers-groups-roles%2fcontext%2fugr-context)) | Colaborador<br/>Propietario |
> | Leer errores de sincronización | Lector ([consulte la documentación](../fundamentals/users-default-permissions.md?context=azure%2factive-directory%2fusers-groups-roles%2fcontext%2fugr-context)) | Colaborador<br/>Propietario |
> | Leer servicios de sincronización | Lector ([consulte la documentación](../fundamentals/users-default-permissions.md?context=azure%2factive-directory%2fusers-groups-roles%2fcontext%2fugr-context)) | Colaborador<br/>Propietario |
> | Ver métricas y alertas | Lector ([consulte la documentación](../fundamentals/users-default-permissions.md?context=azure%2factive-directory%2fusers-groups-roles%2fcontext%2fugr-context)) | Colaborador<br/>Propietario |
> | Ver métricas y alertas | Lector ([consulte la documentación](../fundamentals/users-default-permissions.md?context=azure%2factive-directory%2fusers-groups-roles%2fcontext%2fugr-context)) | Colaborador<br/>Propietario |
> | Ver métricas y alertas del servicio de sincronización | Lector ([consulte la documentación](../fundamentals/users-default-permissions.md?context=azure%2factive-directory%2fusers-groups-roles%2fcontext%2fugr-context)) | Colaborador<br/>Propietario |

## <a name="custom-domain-names"></a>Nombres de dominio personalizados

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Administrar dominios | Administrador de nombres de dominio |  |
> | Leer toda la configuración | Lectores de directorios | Rol de usuario predeterminado ([consulte la documentación](../fundamentals/users-default-permissions.md)) |

## <a name="domain-services"></a>Servicios de dominio

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Crear una instancia de Azure AD Domain Services | Administrador global |  |
> | Realizar todas las tareas de Azure AD Domain Services | Grupo de administradores de DC de Azure AD ([consulte la documentación](../../active-directory-domain-services/tutorial-create-management-vm.md#administrative-tasks-you-can-perform-on-a-managed-domain)) |  |
> | Leer toda la configuración | Lector en la suscripción de Azure que contiene el servicio AD DS |  |

## <a name="devices"></a>Dispositivos

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Deshabilitar dispositivo | Administrador de dispositivos en la nube |  |
> | Habilitar dispositivo | Administrador de dispositivos en la nube |  |
> | Leer configuración básica | Rol de usuario predeterminado ([consulte la documentación](../fundamentals/users-default-permissions.md)) |  |
> | Leer claves de BitLocker | Lector de seguridad | Administrador de contraseñas<br/>Administrador de seguridad |

## <a name="enterprise-applications"></a>Aplicaciones empresariales

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Dar consentimiento a los permisos delegados | Administrador de aplicaciones en la nube | Administrador de aplicaciones |
> | Dar consentimiento a permisos de aplicación sin incluir a Microsoft Graph | Administrador de aplicaciones en la nube | Administrador de aplicaciones |
> | Dar consentimiento a permisos de aplicación para Microsoft Graph | Administrador de roles con privilegios |  |
> | Dar consentimiento a aplicaciones que acceden a datos propios | Rol de usuario predeterminado ([consulte la documentación](../fundamentals/users-default-permissions.md)) |  |
> | Crear aplicación empresarial | Administrador de aplicaciones en la nube | Administrador de aplicaciones |
> | Administrar proxy de aplicación | Administrador de aplicaciones |  |
> | Administrar configuración de usuario | Administrador global |  |
> | Leer revisión de acceso de un grupo o de una aplicación | Lector de seguridad | Administrador de seguridad<br/>Administrador de usuarios |
> | Leer toda la configuración | Rol de usuario predeterminado ([consulte la documentación](../fundamentals/users-default-permissions.md)) |  |
> | Actualizar asignaciones de aplicaciones empresariales | Propietario de la aplicación empresarial ([consulte la documentación](../fundamentals/users-default-permissions.md)) | Administrador de aplicaciones en la nube<br/>Administrador de aplicaciones |
> | Actualizar propietarios de aplicaciones empresariales | Propietario de la aplicación empresarial ([consulte la documentación](../fundamentals/users-default-permissions.md)) | Administrador de aplicaciones en la nube<br/>Administrador de aplicaciones |
> | Actualizar propiedades de aplicaciones empresariales | Propietario de la aplicación empresarial ([consulte la documentación](../fundamentals/users-default-permissions.md)) | Administrador de aplicaciones en la nube<br/>Administrador de aplicaciones |
> | Actualizar aprovisionamiento de aplicaciones empresariales | Propietario de la aplicación empresarial ([consulte la documentación](../fundamentals/users-default-permissions.md)) | Administrador de aplicaciones en la nube<br/>Administrador de aplicaciones |
> | Actualizar autoservicio de aplicaciones empresariales | Propietario de la aplicación empresarial ([consulte la documentación](../fundamentals/users-default-permissions.md)) | Administrador de aplicaciones en la nube<br/>Administrador de aplicaciones |
> | Actualizar propiedades del inicio de sesión único | Propietario de la aplicación empresarial ([consulte la documentación](../fundamentals/users-default-permissions.md)) | Administrador de aplicaciones en la nube<br/>Administrador de aplicaciones |

## <a name="entitlement-management"></a>Administración de derechos

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Adición de recursos a un catálogo | Administrador de Identity Governance | Con la administración de derechos, puede delegar esta tarea en el propietario del catálogo ([consulte la documentación](../governance/entitlement-management-catalog-create.md#add-more-catalog-owners)). |
> | Adición de sitios de SharePoint Online al catálogo | Administrador de SharePoint |  |

## <a name="groups"></a>Grupos

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Asignación de la licencia | Administrador de usuarios |  |
> | Crear grupo | Administrador de grupos | Administrador de usuarios |
> | Crear, actualizar o eliminar revisión de acceso de un grupo o de una aplicación | Administrador de usuarios |  |
> | Administrar expiración de grupos | Administrador de usuarios |  |
> | Administración de la configuración de grupo | Administrador de grupos | Administrador de usuarios |
> | Leer toda la configuración (excepto pertenencia oculta) | Lectores de directorios | Rol de usuario predeterminado ([consulte la documentación](../fundamentals/users-default-permissions.md)) |
> | Leer pertenencias ocultas | Miembro del grupo | Propietario del grupo<br/>Administrador de contraseñas<br/>Administrador de Exchange<br/>Administrador de SharePoint<br/>Administrador de Teams<br/>Administrador de usuarios |
> | Leer pertenencia a grupos con pertenencia oculta | Administrador del departamento de soporte técnico | Administrador de usuarios<br/>Administrador de Teams |
> | Revocar licencia | Administrador de licencias | Administrador de usuarios |
> | Actualizar pertenencia a grupo | Propietario del grupo ([consulte la documentación](../fundamentals/users-default-permissions.md)) | Administrador de usuarios |
> | Actualizar propietarios de grupo | Propietario del grupo ([consulte la documentación](../fundamentals/users-default-permissions.md)) | Administrador de usuarios |
> | Actualizar propiedades de grupo | Propietario del grupo ([consulte la documentación](../fundamentals/users-default-permissions.md)) | Administrador de usuarios |
> | Eliminar grupo | Administrador de grupos | Administrador de usuarios |

## <a name="identity-protection"></a>Protección de identidad

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Configurar notificaciones de alerta| Administrador de seguridad |  |
> | Configurar y habilitar o deshabilitar la directiva de MFA| Administrador de seguridad |  |
> | Configurar y habilitar o deshabilitar la directiva de riesgo de inicio de sesión| Administrador de seguridad |  |
> | Configurar y habilitar o deshabilitar la directiva de riesgo de usuario | Administrador de seguridad |  |
> | Configurar resúmenes semanales | Administrador de seguridad |  |
> | Descartar todas las detecciones de riesgo | Administrador de seguridad |  |
> | Corregir o descartar vulnerabilidad | Administrador de seguridad |  |
> | Leer toda la configuración | Lector de seguridad |  |
> | Leer todas las detecciones de riesgo | Lector de seguridad |  |
> | Leer vulnerabilidades | Lector de seguridad |  |

## <a name="licenses"></a>Licencias

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Asignación de la licencia | Administrador de licencias | Administrador de usuarios |
> | Leer toda la configuración | Lectores de directorios | Rol de usuario predeterminado ([consulte la documentación](../fundamentals/users-default-permissions.md)) |
> | Revocar licencia | Administrador de licencias | Administrador de usuarios |
> | Probar o comprar suscripción | Administrador de facturación |  |

## <a name="monitoring---audit-logs"></a>Supervisión: registros de auditoría

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Leer registros de auditoría | Lector de informes | Lector de seguridad<br/>Administrador de seguridad |

## <a name="monitoring---sign-ins"></a>Supervisión: inicios de sesión

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Leer registros de inicio de sesión | Lector de informes | Lector de seguridad<br/>Administrador de seguridad<br/> Lector global |

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Eliminar todas las contraseñas de aplicación existentes generadas por los usuarios seleccionados | Administrador global |  |
> | Deshabilitar MFA | Administrador de autenticación (a través de PowerShell) | Administrador de autenticación con privilegios (a través de PowerShell) |
> | Habilitar MFA | Administrador de autenticación (a través de PowerShell) | Administrador de autenticación con privilegios (a través de PowerShell) | 
> | Administrar la configuración del servicio MFA | Administrador de directivas de autenticación |  |
> | Requerir a los usuarios seleccionados que vuelvan a proporcionar métodos de contacto | Administrador de autenticación |  |
> | Restaurar autenticación multifactor en todos los dispositivos recordados  | Administrador de autenticación |  |

## <a name="mfa-server"></a>Servidor MFA

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Bloqueo y desbloqueo de usuarios | Administrador de directivas de autenticación |  |
> | Configurar bloqueo de cuentas | Administrador de directivas de autenticación |  |
> | Configurar reglas de caché | Administrador de directivas de autenticación |  |
> | Configurar alerta de fraude | Administrador de directivas de autenticación |  |
> | Configuración de notificaciones | Administrador de directivas de autenticación |  |
> | Configurar la omisión por única vez | Administrador de directivas de autenticación |  |
> | Configurar la configuración de la llamada de teléfono | Administrador de directivas de autenticación |  |
> | Configurar proveedores | Administrador de directivas de autenticación |  |
> | Configuración del servidor | Administrador de directivas de autenticación |  |
> | Leer informe de actividades | Lector global |  |
> | Leer toda la configuración | Lector global |  |
> | Leer estado del servidor | Lector global |  |

## <a name="organizational-relationships"></a>Relaciones organizativas

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Administrar proveedores de identidad | Administrador de proveedor de identidades externo |  |
> | Administración de la configuración | Administrador global |  |
> | Administrar términos de uso | Administrador global |  |
> | Leer toda la configuración | Lector global |  |

## <a name="password-reset"></a>Restablecimiento de contraseña

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Configurar métodos de autenticación | Administrador global |  |
> | Configurar personalización | Administrador global |  |
> | Configurar notificación | Administrador global |  |
> | Configurar integración local | Administrador global |  |
> | Configurar las propiedades de restablecimiento de contraseña | Administrador de usuarios | Administrador global |
> | Configurar registro | Administrador global |  |
> | Leer toda la configuración | Administrador de seguridad | Administrador de usuarios |

## <a name="privileged-identity-management"></a>Privileged Identity Management

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Asignación de usuarios a roles | Administrador de roles con privilegios |  |
> | Configuración de los roles | Administrador de roles con privilegios |  |
> | Ver actividad de auditoría | Lector de seguridad |  |
> | Ver pertenencias a roles | Lector de seguridad |  |

## <a name="roles-and-administrators"></a>Roles y administradores

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Administración de asignaciones de roles | Administrador de roles con privilegios |  |
> | Leer revisión de acceso de un rol de Azure AD  | Lector de seguridad | Administrador de seguridad<br/>Administrador de roles con privilegios |
> | Leer toda la configuración | Rol de usuario predeterminado ([consulte la documentación](../fundamentals/users-default-permissions.md)) |  |

## <a name="security---authentication-methods"></a>Seguridad: métodos de autenticación

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Configurar métodos de autenticación | Administrador global |  |
> | Configuración de la protección con contraseña | Administrador de seguridad |  |
> | Configuración del bloqueo inteligente | Administrador de seguridad |
> | Leer toda la configuración | Lector global |  |

## <a name="security---conditional-access"></a>Seguridad: acceso condicional

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Configurar direcciones IP de confianza de MFA | Administrador de acceso condicional |  |
> | Crear controles personalizados | Administrador de acceso condicional | Administrador de seguridad |
> | Crear ubicaciones con nombre | Administrador de acceso condicional | Administrador de seguridad |
> | Creación de directivas | Administrador de acceso condicional | Administrador de seguridad |
> | Crear términos de uso | Administrador de acceso condicional | Administrador de seguridad |
> | Crear certificado de conectividad VPN | Administrador de acceso condicional | Administrador de seguridad |
> | Eliminar directiva clásica | Administrador de acceso condicional | Administrador de seguridad |
> | Eliminar términos de uso | Administrador de acceso condicional | Administrador de seguridad |
> | Eliminar certificado de conectividad VPN | Administrador de acceso condicional | Administrador de seguridad |
> | Deshabilitar directivas clásicas | Administrador de acceso condicional | Administrador de seguridad |
> | Administrar controles personalizados | Administrador de acceso condicional | Administrador de seguridad |
> | Administrar ubicaciones con nombre | Administrador de acceso condicional | Administrador de seguridad |
> | Administrar términos de uso | Administrador de acceso condicional | Administrador de seguridad |
> | Leer toda la configuración | Lector de seguridad | Administrador de seguridad |
> | Leer ubicaciones con nombre | Lector de seguridad | Administrador de acceso condicional<br/>Administrador de seguridad |

## <a name="security---identity-security-score"></a>Seguridad: puntuación de seguridad de identidad

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales | 
> | ---- | --------------------- | ---------------- |
> | Leer toda la configuración | Lector de seguridad | Administrador de seguridad |
> | Leer puntuación de seguridad | Lector de seguridad | Administrador de seguridad |
> | Actualizar estado del evento | Administrador de seguridad |  |

## <a name="security---risky-sign-ins"></a>Seguridad: inicios de sesión de riesgo

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Leer toda la configuración | Lector de seguridad |  |
> | Leer inicios de sesión de riesgo | Lector de seguridad |  |

## <a name="security---users-flagged-for-risk"></a>Seguridad: usuarios marcados en riesgo

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Descartar todos los eventos | Administrador de seguridad |  |
> | Leer toda la configuración | Lector de seguridad |  |
> | Leer usuarios marcados en riesgo | Lector de seguridad |  |

## <a name="users"></a>Usuarios

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Agregar usuario a rol de directorio | Administrador de roles con privilegios |  |
> | Agregar usuario a un grupo | Administrador de usuarios |  |
> | Asignación de la licencia | Administrador de licencias | Administrador de usuarios |
> | Crear usuario invitado | Invitador de usuarios | Administrador de usuarios |
> | Crear usuario | Administrador de usuarios |  |
> | Eliminación de usuarios | Administrador de usuarios |  |
> | Invalidar tokens de actualización de los administradores limitados (consulte la documentación) | Administrador de usuarios |  |
> | Invalidar tokens de actualización de usuarios no administradores (consulte la documentación) | Administrador de contraseñas | Administrador de usuarios |
> | Invalidar tokens de actualización de administradores con privilegios (consulte la documentación) | Administrador de autenticación con privilegios |  |
> | Leer configuración básica | Rol de usuario predeterminado ([consulte la documentación](../fundamentals/users-default-permissions.md)) |  |
> | Restablecer contraseñas para administradores limitados (consulte la documentación) | Administrador de usuarios |  |
> | Restablecer contraseñas de usuarios no administradores (consulte la documentación) | Administrador de contraseñas | Administrador de usuarios |
> | Restablecer contraseñas de administradores con privilegios | Administrador de autenticación con privilegios |  |
> | Revocar licencia | Administrador de licencias | Administrador de usuarios |
> | Actualizar todas las propiedades excepto el nombre principal de usuario | Administrador de usuarios |  |
> | Actualizar el nombre principal de usuario para administradores limitados (consulte la documentación) | Administrador de usuarios |  |
> | Actualizar la propiedad nombre principal de usuario en administradores con privilegios (consulte la documentación) | Administrador global |  |
> | Actualizar configuración de usuario | Administrador global |  |
> | Actualizar métodos de autenticación | Administrador de autenticación | Administrador de autenticación con privilegios<br/>Administrador global |

## <a name="support"></a>Soporte técnico

> [!div class="mx-tableFixed"]
> | Tarea | Rol con privilegios mínimos | Roles adicionales |
> | ---- | --------------------- | ---------------- |
> | Enviar incidencia de soporte técnico | Administrador del soporte técnico del servicio | Administrador de aplicaciones<br/>Administrador de Azure Information Protection<br/>Administrador de facturación<br/>Administrador de aplicaciones en la nube<br/>Administrador de cumplimiento<br/>Administrador de Dynamics 365<br/>Administrador de análisis de escritorio<br/>Administrador de Exchange<br/>Administrador de Intune<br/>Administrador de contraseñas<br/>Administrador de Power BI<br/>Administrador de autenticación con privilegios<br/>Administrador de SharePoint<br/>Administrador de Skype Empresarial<br/>Administrador de Teams<br/>Administrador de comunicaciones de Teams<br/>Administrador de usuarios<br/>Administrador de Workplace Analytics |

## <a name="next-steps"></a>Pasos siguientes

* [Asignación o eliminación de roles de administrador de Azure AD](manage-roles-portal.md)
* [Roles integrados de Azure AD](permissions-reference.md)
