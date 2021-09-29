---
title: Azure Lighthouse y Azure Managed Applications
description: Comprenda cómo se pueden usar Azure Lighthouse y aplicaciones administradas de Azure de forma conjunta.
ms.date: 09/08/2021
ms.topic: conceptual
ms.openlocfilehash: 629f9f1f6b6db69816d26cc7eed1e305dd55923b
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124768579"
---
# <a name="azure-lighthouse-and-azure-managed-applications"></a>Azure Lighthouse y Azure Managed Applications

Tanto Azure Managed Applications como Azure Lighthouse funcionan habilitando un proveedor de servicios para acceder a los recursos que residen en el inquilino del cliente. Puede ser útil comprender las diferencias en cuanto a su funcionamiento y los escenarios que ayudan a habilitar, así como la forma en que se pueden usar conjuntamente.

> [!TIP]
> Aunque en este tema nos referiremos a los proveedores de servicios y clientes, las [empresas que administren varios inquilinos](enterprise.md) pueden usar el mismo proceso y herramientas.

## <a name="comparing-azure-lighthouse-and-azure-managed-applications"></a>Comparación de Azure Lighthouse y Azure Managed Applications

En esta tabla se muestran algunas diferencias generales que pueden afectar a la posibilidad de usar Azure Lighthouse o aplicaciones administradas de Azure. Como se indica a continuación, también puede diseñar una solución en la que se usen conjuntamente.

|Consideración  |Azure Lighthouse  |Aplicaciones administradas de Azure  |
|---------|---------|---------|
|Usuario típico     |Proveedores de servicios o empresas que administran varios inquilinos         |Fabricantes independientes de software (ISV)         |
|Ámbito del acceso entre inquilinos     |Suscripción o grupo de recursos         |Grupo de recursos (con ámbito en una sola aplicación)         |
|Se puede comprar en Azure Marketplace     |No (las ofertas se pueden publicar en Azure Marketplace, pero a los clientes se les factura por separado)        |Sí         |
|Protección IP     |Sí (la IP puede permanecer en el inquilino del proveedor de servicios)        |Sí (por diseño, el grupo de recursos está bloqueado para los clientes)         |
|Asignaciones de denegación     |No         |Sí        |

### <a name="azure-lighthouse"></a>Azure Lighthouse

Con [Azure Lighthouse](../overview.md), un proveedor de servicios puede realizar una amplia variedad de tareas de administración directamente en la suscripción de un cliente (o en el grupo de recursos). Este acceso se logra a través de una proyección lógica, lo que permite a los proveedores de servicios iniciar sesión en su propio inquilino y acceder a los recursos que pertenecen al inquilino del cliente. El cliente puede determinar qué suscripciones o grupos de recursos se van a delegar en el proveedor de servicios. Además, este mantiene el acceso completo a esos recursos. También pueden quitar el acceso del proveedor de servicios en cualquier momento.

Para usar Azure Lighthouse, los clientes se incorporan mediante la [implementación de plantillas de ARM](../how-to/onboard-customer.md) o a través de una [oferta de servicio administrado en Azure Marketplace](managed-services-offers.md). Puede realizar un seguimiento del impacto en las interacciones con los clientes [vinculando el id. de partner](../how-to/partner-earned-credit.md).

Azure Lighthouse se utiliza normalmente cuando un proveedor de servicios realiza tareas de administración para un cliente de forma continuada. Para obtener más información sobre cómo funciona Azure Lighthouse a nivel técnico, consulte [Arquitectura de Azure Lighthouse](architecture.md).

### <a name="azure-managed-applications"></a>Aplicaciones administradas de Azure

Las aplicaciones de [Azure Managed Applications](../../azure-resource-manager/managed-applications/overview.md) permiten a un proveedor de servicios o a un ISV ofrecer a los clientes soluciones en la nube fáciles de implementar y usar en sus propias suscripciones.

En una aplicación administrada, los recursos que usa la aplicación se agrupan e implementan en un grupo de recursos que está administrado por el editor. Este grupo de recursos está presente en la suscripción del cliente, pero una identidad en el inquilino del editor tiene acceso al mismo. El ISV sigue administrando y manteniendo la aplicación administrada, mientras que el cliente no tiene acceso directo para trabajar en su grupo de recursos, ni tampoco tiene acceso a sus recursos.

Las aplicaciones administradas admiten [experiencias de Azure Portal personalizadas](../../azure-resource-manager/managed-applications/concepts-view-definition.md) y la [integración con proveedores personalizados](../../azure-resource-manager/managed-applications/tutorial-create-managed-app-with-custom-provider.md). Estas opciones se pueden usar para ofrecer una experiencia más personalizada e integrada, lo que facilita a los clientes la realización de algunas tareas de administración.

Las aplicaciones administradas se pueden [publicar en Azure Marketplace](../../marketplace/azure-app-offer-setup.md), ya sea como una oferta privada para el uso de un cliente específico o como ofertas públicas que pueden comprar varios clientes. También se pueden entregar a los usuarios de la organización [publicando aplicaciones administradas en el catálogo de servicios](../../azure-resource-manager/managed-applications/publish-service-catalog-app.md). Puede implementar instancias de Marketplace y el catálogo de servicios mediante plantillas de ARM, que pueden incluir el identificador único de un asociado de Marketplace comercial para realizar el seguimiento de la [atribución de uso del cliente](../../marketplace/azure-partner-customer-usage-attribution.md).

Las aplicaciones de Azure Managed Applications se usan normalmente para una necesidad específica del cliente que se puede lograr a través de una solución inmediata totalmente administrada por el proveedor de servicios.

## <a name="using-azure-lighthouse-and-azure-managed-applications-together"></a>Uso de Azure Lighthouse y Azure Managed Applications de forma conjunta

Aunque Azure Lighthouse y Azure Managed Applications usan distintos mecanismos de acceso para lograr objetivos diferentes, puede haber escenarios en los que tenga sentido que un proveedor de servicios use ambos con el mismo cliente.

Por ejemplo, un cliente podría querer servicios administrados ofrecidos por un proveedor de servicios a través de Azure Lighthouse, de modo que pueda tener visibilidad de las acciones del asociado junto con el control continuado de su suscripción delegada. Sin embargo, es posible que el proveedor de servicios no quiera que el cliente tenga acceso a determinados recursos que se almacenarán en el inquilino del cliente o que permitan acciones personalizadas en esos recursos. Para cumplir estos objetivos, el proveedor de servicios puede publicar una oferta privada como aplicación administrada. La aplicación administrada puede incluir un grupo de recursos que se implementa en el inquilino del cliente, pero al que no puede tener acceso el cliente de forma directa.

Los clientes también podrían estar interesados en aplicaciones administradas de varios proveedores de servicios, independientemente de si también usan o no servicios administrados a través de Azure Lighthouse de cualquiera de esos proveedores de servicios. Además, los asociados del programa Proveedor de soluciones en la nube (CSP) pueden revender algunas aplicaciones administradas publicadas por otros ISV a los clientes que admiten a través de Azure Lighthouse. Con una amplia gama de opciones, los proveedores de servicios pueden elegir el equilibrio adecuado para satisfacer las necesidades de sus clientes mientras se restringe el acceso a los recursos cuando corresponda.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre [Azure Managed Applications](../../azure-resource-manager/managed-applications/overview.md).
- Obtenga información sobre cómo [Incorporar una suscripción a Azure Lighthouse](../how-to/onboard-customer.md).
- Obtenga información sobre [Escenarios de ISV con Azure Lighthouse](isv-scenarios.md).