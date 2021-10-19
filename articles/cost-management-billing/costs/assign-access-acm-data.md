---
title: Asignar acceso a los datos de Cost Management
description: Este artículo le guiará a través del proceso para asignar permisos a los datos de Cost Management para obtener varios ámbitos de acceso.
author: bandersmsft
ms.author: banders
ms.date: 10/07/2021
ms.topic: how-to
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: adwise
ms.custom: secdec18
ms.openlocfilehash: dddb6292530687e75e4b5e697f4fc754d98040fb
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2021
ms.locfileid: "129706244"
---
# <a name="assign-access-to-cost-management-data"></a>Asignar acceso a los datos de Cost Management

En el caso de los usuarios con Contratos Enterprise de Azure, una combinación de permisos otorgados en Azure Portal y Enterprise Portal (Contrato Enterprise) define el nivel de acceso de un usuario a los datos de Cost Management. Para los usuarios con otros tipos de cuenta de Azure, la definición del nivel de acceso de un usuario a los datos de Cost Management es más sencilla con el control de acceso basado en rol de Azure (Azure RBAC). Este artículo le guiará a través del proceso para asignar acceso a los datos de Cost Management. Una vez que se asigna la combinación de permisos, el usuario verá los datos en Cost Management en función de su ámbito de acceso y del ámbito que seleccione en Azure Portal.

El ámbito que un usuario selecciona se usa en Cost Management para proporcionar la consolidación de datos y controlar el acceso a la información de costos. Cuando se usan los ámbitos, los usuarios no pueden seleccionar varios de ellos. En su lugar, deben seleccionar un ámbito más amplio que abarque los ámbitos secundarios y filtrar lo que quieran ver. Es importante comprender la consolidación de datos porque algunas personas no deberían tener acceso a un ámbito principal que abarque los ámbitos secundarios.

Vea el vídeo [Control de acceso de Cost Management](https://www.youtube.com/watch?v=_uQzQ9puPyM) para conocer la asignación de acceso y ver los costos y los gastos del control de acceso basado en rol de Azure (Azure RBAC). Para ver otros vídeos, visite el [canal de YouTube de Cost Management](https://www.youtube.com/c/AzureCostManagement).

>[!VIDEO https://www.youtube.com/embed/_uQzQ9puPyM]

## <a name="cost-management-scopes"></a>Ámbitos de Cost Management

Cost Management es compatible con una variedad de tipos de cuenta de Azure. Para ver la lista completa de tipos de cuenta compatibles, consulte [Understand Cost Management data](understand-cost-mgt-data.md) (Información sobre los datos de Cost Management). El tipo de cuenta determina los ámbitos disponibles.

### <a name="azure-ea-subscription-scopes"></a>Ámbitos de suscripción de Azure para Contrato Enterprise

Un usuario debe tener, como mínimo, acceso de lectura a uno o varios de los siguientes ámbitos para ver los datos de costos de las suscripciones de Azure para Contrato Enterprise.

| **Ámbito** | **Definido en** | **Acceso requerido para ver los datos** | **Configuración de EA como requisito previo** | **Consolida los datos en** |
| --- | --- | --- | --- | --- |
| Cuenta de facturación<sup>1</sup> | [https://ea.azure.com](https://ea.azure.com/) | Administrador de organización | None | Todas las suscripciones del contrato Enterprise |
| department | [https://ea.azure.com](https://ea.azure.com/) | Administrador de departamento | **AD: ver los cargos** habilitado | Todas las suscripciones que pertenecen a una cuenta de inscripción que esté vinculada al departamento |
| Cuenta de inscripción<sup>2</sup> | [https://ea.azure.com](https://ea.azure.com/) | Propietario de cuenta | **PC: ver los cargos** habilitado | Todas las suscripciones de la cuenta de inscripción |
| Grupo de administración | [https://portal.azure.com](https://portal.azure.com/) | Lector (o Colaborador) de Cost Management | **PC: ver los cargos** habilitado | Todas las suscripciones por debajo del grupo de administración |
| Subscription | [https://portal.azure.com](https://portal.azure.com/) | Lector (o Colaborador) de Cost Management | **PC: ver los cargos** habilitado | Todos los grupos de recursos o recursos en la suscripción |
| Resource group | [https://portal.azure.com](https://portal.azure.com/) | Lector (o Colaborador) de Cost Management | **PC: ver los cargos** habilitado | Todos los recursos del grupo de recurso |

<sup>1</sup>La cuenta de facturación también se conoce como inscripción o Contrato Enterprise.

<sup>2</sup> La cuenta de inscripción también se denomina como Propietario de la cuenta.


## <a name="other-azure-account-scopes"></a>Otros ámbitos de la cuenta de Azure

Un usuario debe tener, como mínimo, acceso de lectura a uno o varios de los siguientes ámbitos para ver los datos de costos de otras suscripciones de Azure:

- Grupo de administración
- Subscription
- Resource group

Hay varios ámbitos disponibles después de que los asociados incorporen a los clientes a un contrato de cliente de Microsoft. Los clientes de CSP pueden usar las características de Cost Management cuando están habilitadas por su asociado de CSP. Para más información, consulte [Introducción a Cost Management para asociados](get-started-partners.md).

## <a name="enable-access-to-costs-in-the-azure-portal"></a>Habilitación del acceso a los costos en Azure Portal

El ámbito de departamento requiere que la opción **Los administradores del departamento pueden ver los cargos** (DA view charges) (El administrador del departamento ve los cargos) esté establecida en **Activado**. Configure la opción en Azure Portal o en el portal de EA. En el caso de todos los demás ámbitos, es necesario que la opción **Los propietarios de la cuenta pueden ver los cargos** (AO View Charges) (El propietario de la cuenta ve los cargos) esté establecida en **Activado**.

Para habilitar una opción en Azure Portal:

1. Inicie sesión en Azure Portal en https://portal.azure.com con una cuenta de administrador de empresa.
1. Seleccione el elemento de menú **Administración de costos + facturación**.
1. Seleccione **Ámbitos de facturación** para ver una lista de los ámbitos y las cuentas de facturación disponibles.
1. Seleccione su **cuenta de facturación** en la lista de cuentas de facturación disponibles.
1. En **Configuración**, seleccione el elemento de menú **Directivas** y, luego, configure el valor.  
    ![Directivas de ámbito de facturación que muestran las opciones de vista de cargos](./media/assign-access-acm-data/azure-portal-policies-view-charges.png)

Después de habilitar las opciones para ver los cargos, la mayoría de los ámbitos también requieren configurar el permiso de control de acceso basado en roles de Azure (Azure RBAC) en Azure Portal.

## <a name="enable-access-to-costs-in-the-ea-portal"></a>Habilitar el acceso a los costos en el portal de EA

El ámbito del departamento requiere que la opción **AD: ver los cargos** esté **habilitada** en el portal de EA. Configure la opción en Azure Portal o en el portal de EA. Todos los demás ámbitos deben tener la opción **PC: ver los cargos** **habilitada** en el portal de EA.

Para habilitar una opción en el portal de EA:

1. Inicie sesión en el portal de EA en [https://ea.azure.com](https://ea.azure.com) con una cuenta de administrador de empresa.
2. Seleccione **Manage** (Administrar) en el panel izquierdo.
3. En cuanto a los ámbitos de administración de costos a los que desea proporcionar acceso, habilite la opción **AD: ver los cargos** o **PC: ver los cargos**.  
    ![Pestaña de inscripción que muestra las opciones para ver los cargos AD y PC](./media/assign-access-acm-data/ea-portal-enrollment-tab.png)

Después de habilitar las opciones para ver los cargos, la mayoría de los ámbitos también requieren configurar el permiso de control de acceso basado en roles de Azure (Azure RBAC) en Azure Portal.

## <a name="enterprise-administrator-role"></a>Rol de administrador de empresa

De forma predeterminada, un administrador de empresa puede acceder a la cuenta de facturación (Contrato Enterprise o inscripción) y al resto de ámbitos que son ámbitos secundarios. El administrador de empresa se encarga de asignar acceso a los ámbitos a otros usuarios. Como procedimiento recomendado para la continuidad del negocio, siempre debe tener dos usuarios con acceso de administrador de empresa. En las siguientes secciones encontrará ejemplos de cómo el administrador de empresa procede a asignar acceso a los ámbitos a otros usuarios.

## <a name="assign-billing-account-scope-access"></a>Asignar acceso al ámbito de la cuenta de facturación

Para obtener acceso al ámbito de la cuenta de facturación es necesario tener permiso de administrador de empresa en el portal de EA. El administrador de empresa puede ver los costos de toda la inscripción en el Contrato Enterprise o de varias inscripciones. No se requiere ninguna acción en Azure Portal para obtener el ámbito de la cuenta de facturación.

1. Inicie sesión en el portal de EA en [https://ea.azure.com](https://ea.azure.com) con una cuenta de administrador de empresa.
2. Seleccione **Manage** (Administrar) en el panel izquierdo.
3. En la pestaña **Enrollment** (Inscripción), seleccione la inscripción que quiere administrar.  
    ![selección de la inscripción en el portal de EA](./media/assign-access-acm-data/ea-portal.png)
4. Seleccione **+ Add Administrator** (+ Agregar administrador).
5. En el cuadro Add Administrator (Agregar administrador), seleccione el tipo de autenticación y escriba la dirección de correo electrónico del usuario.
6. Si el usuario debe tener acceso de solo lectura a los datos de costo y uso, en **Read-only** (Solo lectura), seleccione **Yes** (Sí).  De lo contrario, seleccione **No**.
7. Seleccione **Add** (Agregar) para crear la cuenta.  
    ![información de ejemplo que se muestra en el cuadro Agregar administrador](./media/assign-access-acm-data/add-admin.png)

Pueden pasar hasta 30 minutos antes de que el nuevo usuario pueda obtener acceso a los datos de Cost Management.

### <a name="assign-department-scope-access"></a>Asignar acceso al ámbito del departamento

Para obtener acceso al ámbito del departamento es necesario tener acceso de administrador del departamento (AD: ver los cargos) en el portal de EA. El administrador del departamento puede ver los datos de costos y uso asociados con uno o varios departamentos. Los datos del departamento incluyen todas las suscripciones que pertenecen a una cuenta de inscripción que esté vinculada al departamento. No es necesario hacer nada más en Azure Portal.

1. Inicie sesión en el portal de EA en [https://ea.azure.com](https://ea.azure.com) con una cuenta de administrador de empresa.
2. Seleccione **Manage** (Administrar) en el panel izquierdo.
3. En la pestaña **Enrollment** (Inscripción), seleccione la inscripción que quiere administrar.
4. Seleccione la pestaña **Department** (Departamento) y, luego, elija **Add Administrator** (Agregar administrador).
5. En el cuadro Add Department Administrator (Agregar administrador de departamento), seleccione el tipo de autenticación y escriba la dirección de correo electrónico del usuario.
6. Si el usuario debe tener acceso de solo lectura a los datos de costo y uso, en **Read-only** (Solo lectura), seleccione **Yes** (Sí).  De lo contrario, seleccione **No**.
7. Seleccione los departamentos a los que quiera otorgar el permiso administrativo de departamento.
8. Seleccione **Add** (Agregar) para crear la cuenta.  
    ![escriba la información necesaria en el cuadro Agregar administrador de departamento](./media/assign-access-acm-data/add-depart-admin.png)

## <a name="assign-enrollment-account-scope-access"></a>Asignar acceso al ámbito de la cuenta de inscripción

Para obtener acceso al ámbito de la cuenta de inscripción es necesario tener acceso de propietario de la cuenta (PC: ver los cargos) en el portal de EA. El propietario de la cuenta puede ver los datos de costos y de uso asociados con las suscripciones que se crean a partir de esa cuenta de inscripción. No es necesario hacer nada más en Azure Portal.

1. Inicie sesión en el portal de EA en [https://ea.azure.com](https://ea.azure.com) con una cuenta de administrador de empresa.
2. Seleccione **Manage** (Administrar) en el panel izquierdo.
3. En la pestaña **Enrollment** (Inscripción), seleccione la inscripción que quiere administrar.
4. Seleccione la pestaña **Account** (Cuenta) y, luego, elija **Add Account** (Agregar cuenta).
5. En el cuadro Add Account (Agregar cuenta), seleccione el **departamento** que quiere asociar la cuenta, o déjela sin asignar.
6. Seleccione el tipo de autenticación y escriba el nombre de cuenta.
7. Escriba la dirección de correo electrónico del usuario y, opcionalmente, escriba el centro de coste.
8. Seleccione **Add** (Agregar) para crear la cuenta.  
    ![escriba la información necesaria en el cuadro Agregar cuenta para agregar una cuenta de inscripción](./media/assign-access-acm-data/add-account.png)

Después de completar los pasos anteriores, la cuenta de usuario se convierte en una cuenta de inscripción en Enterprise Portal y puede crear suscripciones. El usuario puede tener acceso a los datos de costo y uso para las suscripciones que crea.

## <a name="assign-management-group-scope-access"></a>Asignar el acceso al ámbito de grupo de administración

El acceso para ver el ámbito de grupo de administración requiere al menos el permiso de lector de Cost Management (o lector). Puede configurar los permisos de un grupo de administración en Azure Portal. Debe tener al menos el permiso de acceso de usuario administrador (o propietario) para el grupo de Administrador de acceso de usuario (o Propietario) del grupo de administración para habilitar el acceso de otros usuarios. En el caso de las cuentas de Azure para EA, también debe haber habilitado la opción de configuración **PC: ver los cargos** en el portal de EA.


- Asigne el rol Lector de Cost Management a un usuario del ámbito del grupo de administración.  
     Para asignar roles, consulte [Asignación de roles de Azure mediante Azure Portal](../../role-based-access-control/role-assignments-portal.md).

## <a name="assign-subscription-scope-access"></a>Asignar acceso de ámbito de suscripción

Para obtener acceso a la suscripción, es necesario tener al menos permiso de Lector de Cost Management (o Lector). Puede configurar los permisos a una suscripción en Azure Portal. Debe tener al menos el permiso de Administrador de acceso de usuario (o Propietario) de la suscripción para habilitar el acceso de otros usuarios. En el caso de las cuentas de Azure para EA, también debe haber habilitado la opción de configuración **PC: ver los cargos** en el portal de EA.

- Asigne el rol Lector de Cost Management a un usuario del ámbito de la suscripción.  
     Para asignar roles, consulte [Asignación de roles de Azure mediante Azure Portal](../../role-based-access-control/role-assignments-portal.md).

## <a name="assign-resource-group-scope-access"></a>Asignar el acceso al ámbito de grupo de recursos

Para obtener acceso al grupo de recursos es necesario tener al menos permiso de Lector de Cost Management (o Lector). Puede configurar los permisos de un grupo de recursos en Azure Portal. Debe tener al menos el permiso de Administrador de acceso de usuario (o Propietario) del grupo de recursos para habilitar el acceso de otros usuarios. En el caso de las cuentas de Azure para EA, también debe haber habilitado la opción de configuración **PC: ver los cargos** en el portal de EA.


- Asigne el rol Lector de Cost Management a un usuario del ámbito del grupo de recursos.  
     Para asignar roles, consulte [Asignación de roles de Azure mediante Azure Portal](../../role-based-access-control/role-assignments-portal.md).

## <a name="cross-tenant-authentication-issues"></a>Problemas de autenticación a través de inquilinos

Actualmente, Cost Management tiene compatibilidad limitada con la autenticación entre inquilinos. En algunas circunstancias cuando intenta autenticarse en varios inquilinos, puede recibir un error de **acceso denegado** en el análisis de costos. Este problema puede producirse si configura el control de acceso basado en roles de Azure (Azure RBAC) para la suscripción de otro inquilino y, a continuación, intenta ver los datos de costos.

*Para solucionar el problema*: Después de configurar Azure RBAC entre inquilinos, espere una hora. A continuación, intente ver el análisis de costos o conceder acceso a Cost Management a los usuarios en ambos inquilinos.  


## <a name="next-steps"></a>Pasos siguientes

- Si aún no ha completado la primera guía rápida de Cost Management, léala en [Start analyzing costs](quick-acm-cost-analysis.md) (Comenzar a analizar los costos).
