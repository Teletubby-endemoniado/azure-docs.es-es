---
title: Creación de planes para una oferta de máquina virtual en Azure Marketplace
description: Cree planes para una oferta de máquina virtual en Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 11/10/2021
ms.openlocfilehash: b8464f8f919b05647ef053c78573692975fd0b63
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132305909"
---
# <a name="create-plans-for-a-virtual-machine-offer"></a>Creación de planes para una oferta de máquina virtual

En la página **Información general del plan** (selecciónela en el menú de navegación de la izquierda en el Centro de partners), puede proporcionar diversas opciones de plan dentro de la misma oferta. Una oferta requiere al menos un plan (anteriormente denominada una SKU), que puede variar en términos de audiencia de monetización, regiones de Azure, características o imágenes de máquina virtual.

Puede crear hasta 100 planes para cada oferta, de los cuales hasta 45 pueden ser privados. Más información sobre los planes privados en [Ofertas privadas en el marketplace comercial de Microsoft](private-offers.md).

Después de crear los planes, la pestaña **Plan overview** (Información general del plan) muestra lo siguiente:

- Nombres de los planes
- Modelos de licencia
- Audiencia (pública o privada)
- Estado de publicación actual
- Acciones disponibles

Las acciones disponibles en este panel varían en función del estado actual del plan.

- Si el estado del plan es un borrador, seleccione **Eliminar borrador**.
- Si el estado del plan se publica en directo, seleccione **Deprecate plan** (Plan en desuso) o **Sync private audience** (Sincronizar audiencia privada).

## <a name="create-a-new-plan"></a>Creación de un nuevo plan

Seleccione **+ Crear nuevo plan** en la parte superior.

En el cuadro de diálogo **Nuevo plan**, escriba un **identificador de plan** único para cada plan de esta oferta. Este identificador será visible para los clientes en la dirección web del producto. Use solo letras minúsculas y números, guiones o caracteres de subrayado, y un máximo de 50 caracteres.

> [!NOTE]
> El id. de oferta no se puede cambiar después de seleccionar **Crear**.

Escriba un **nombre de plan**. Los clientes verán este nombre al decidir qué plan van a seleccionar en su oferta. Cree un nombre único que destaque claramente las diferencias entre los planes. Por ejemplo, puede especificar **Windows Server** con los planes *Pago por uso*, *BYOL*, *Avanzado* y *Enterprise*.

Seleccione **Crear**. Se abrirá la página **Configuración del plan**.

## <a name="plan-setup"></a>Configuración del plan

Establezca la configuración de alto nivel para el tipo de plan, especifique si reutiliza la configuración técnica de otro plan e identifique las regiones de Azure en las que debe estar disponible el plan. Las selecciones que se realicen aquí afectarán a los campos mostrados en otros paneles para el mismo plan.

### <a name="azure-regions"></a>Regiones de Azure

El plan debe estar disponible al menos en una región de Azure.

Seleccione **Azure global** para que el plan esté disponible para los clientes de todas las regiones globales de Azure que tengan la integración de marketplace comercial. Para más información, consulte [Disponibilidad geográfica y compatibilidad con monedas para Marketplace comercial](marketplace-geo-availability-currencies.md).

Seleccione **Azure Government** para que el plan esté disponible en la región de [Azure Government](../azure-government/documentation-government-welcome.md). Esta región proporciona acceso controlado para los clientes de entidades tribales, locales, estatales o federales de Estados Unidos, así como para los asociados aptos para atenderlas. Como publicador, es responsable de los controles de cumplimiento, las medidas de seguridad y los procedimientos recomendados. Azure Government usa redes y centros de datos aislados físicamente (ubicados solo en Estados Unidos).

Antes de publicar en [Azure Government](../azure-government/documentation-government-manage-marketplace-partners.md), pruebe y valide el plan en el entorno, ya que algunos puntos de conexión pueden ser diferentes. Para configurar y probar el plan, solicite una cuenta de prueba desde la página [Prueba de Microsoft Azure Government](https://azure.microsoft.com/global-infrastructure/government/request/).

> [!NOTE]
> Una vez que el plan esté publicado y disponible en una región de Azure específica, no se puede quitar esa región.

### <a name="azure-government-certifications"></a>Certificaciones de Azure Government

Esta opción solo está visible si ha seleccionado **Azure Government** como la región de Azure en la sección anterior.

Los servicios de Azure Government controlan datos que están sujetos a determinados reglamentos y requisitos gubernamentales. Por ejemplo, FedRAMP, NIST 800.171 (DIB), ITAR, IRS 1075, DoD L4 y CJIS. Para dar a conocer sus certificaciones de estos programas, puede proporcionar hasta 100 vínculos que las describan. Pueden ser vínculos a su anuncio en el programa directamente o a descripciones de su cumplimiento de estos en sus propios sitios web. Estos vínculos solo son visibles para los clientes de Azure Government.

Seleccione **Guardar borrador** antes de pasar a la pestaña siguiente del menú de navegación de la izquierda Plan, **Lista del plan**.

## <a name="plan-listing"></a>Lista del plan

Configure los detalles de la lista del plan. Este panel muestra información específica que puede variar de otros planes de la misma oferta.

### <a name="plan-name"></a>Nombre del plan

Este campo se rellena automáticamente con el nombre que asignó al plan cuando lo creó. Este nombre aparece en Azure Marketplace como el título de este plan. Tiene un límite de 100 caracteres.

### <a name="plan-summary"></a>Resumen del plan

Proporcione un breve resumen del plan, no de la oferta. Este resumen se limita a 100 caracteres.

### <a name="plan-description"></a>Description del plan

Describa lo que hace que este plan sea único, así como las diferencias entre los planes de la oferta. Describa solo el plan, no la oferta. La descripción del plan puede contener hasta 2 000 caracteres.

Seleccione **Guardar borrador** antes de pasar a la pestaña siguiente del menú de navegación de la izquierda Plan, **Precios y disponibilidad**.

## <a name="pricing-and-availability"></a>Precios y disponibilidad

En este panel, configura:

- Mercados en los que este plan está disponible. Todos los planes debe estar disponible al menos en un [mercado](marketplace-geo-availability-currencies.md).
- Precio por hora.
- Si desea que el plan sea visible para todos los usuarios o solo para clientes específicos (audiencia privada).

### <a name="markets"></a>Mercados

Todos los planes debe estar disponible al menos en un mercado. La mayoría de los mercados se seleccionan de manera predeterminada. Para editar la lista, seleccione **Editar mercados** y active o desactive las casillas correspondientes a cada ubicación de mercado donde este plan debería (o no debería) estar disponible para compra. Los usuarios de los mercados seleccionados pueden seguir implementando la oferta en todas las regiones de Azure seleccionadas en la sección ["Configuración del plan"](#plan-setup).

Seleccione **Select only Microsoft Tax Remitted** (Seleccionar solo Recaudación fiscal por parte de Microsoft) para seleccionar solo los países o regiones en los que Microsoft remite las ventas y el impuesto sobre el consumo en su nombre. La publicación en China se limita a planes que sean *Gratis* o *Traiga su propia licencia* (BYOL).

Si ya se han establecido los precios del plan en dólares de Estados Unidos (USD) y agrega otra ubicación de mercado, el precio del nuevo mercado se calcula en función de los tipos de cambio actuales. Revise siempre el precio de cada mercado antes de la publicación. Revise los precios seleccionando **Export prices (xlsx)** [Exportar precios (xlsx)] después de guardar los cambios.

Al eliminar un mercado, los clientes de ese mercado que usan implementaciones activas no podrán crear nuevas implementaciones ni escalar verticalmente las implementaciones existentes. Esto no afecta a las implementaciones existentes.

Seleccione **Guardar** para continuar.

### <a name="pricing"></a>Precios

Para el **Modelo de licencias**, seleccione **Plan de facturación mensual en función del uso** para configurar los precios de este plan o **Traiga su propia licencia** para que los clientes puedan usar este plan con su licencia existente.

Para un plan de facturación mensual en función del uso, utilice una de las tres opciones de entrada de precios siguientes:

- **Por núcleo**: proporcione precios por núcleo en USD. Microsoft calcula los precios por tamaño de núcleo y los convierte a las monedas locales con la tasa de cambio actual.
- **Por tamaño de núcleo**: proporcione los precios por tamaño de núcleo en USD. Microsoft calcula los precios y los convierte a las monedas locales con la tasa de cambio actual.
- **Por mercado y tamaño de núcleo**: proporcione los precios para cada tamaño de núcleo para todos los mercados. Puede importar los precios desde una hoja de cálculo.

Especifique un **Precio por núcleo** y, luego, seleccione **Precio por tamaño de núcleo** para ver una tabla de cálculos de precio/hora.

> [!NOTE]
> Guarde los cambios de los precios para habilitar la exportación de los datos de precios. Después de que se publica un precio para un mercado en el plan, no se puede cambiar más adelante. Asegúrese de que los precios sean correctos antes de publicarlos; para ello, exporte la hoja de cálculo de precios y revise los precios en cada mercado.

### <a name="free-trial"></a>Versión de prueba gratuita

Puede ofrecer a los clientes una **evaluación gratuita** de uno, tres o seis meses.

### <a name="plan-visibility"></a>Visibilidad del plan

Puede diseñar cada plan para que sea visible para todos los usuarios o solo para una audiencia preseleccionada. Asigne las pertenencias a esta audiencia restringida mediante identificadores de suscripción de Azure, de inquilino o ambos.

**Pública**: todos los usuarios pueden ver el plan.

**Privada**: haga que el plan sea visible solo para una audiencia preseleccionada. Una vez publicado como un plan privado, puede actualizar la audiencia o cambiarlo a público. Después de hacer que un plan sea público, debe seguir siendo público. No se puede volver a cambiar a un plan privado.

Asigne la audiencia que va a tener acceso a este plan privado mediante *identificadores de suscripción de Azure*, *identificadores de inquilino* o ambos. Opcionalmente, incluya una **Descripción** de cada identificador de suscripción de Azure o de inquilino que asigne. Agregue hasta diez identificadores de suscripción e inquilino manualmente o importe una hoja de cálculo CSV si se requieren más de diez.

> [!NOTE]
> Una audiencia privada o restringida es diferente de la audiencia preliminar que definió en la pestaña **Versión preliminar**. A una audiencia preliminar se le permite acceder a la oferta *antes* de que se publique en Azure Marketplace. Mientras que la designación de una audiencia privada solo se aplica a un plan específico, la audiencia preliminar puede ver todos los planes privados y públicos con fines de validación.

Las ofertas privadas no son compatibles con las suscripciones de Azure que se establecen a través de un revendedor del programa Proveedor de soluciones en la nube (CSP).

### <a name="hide-plan"></a>Ocultar plan

Si la máquina virtual está diseñada para utilizarla solo indirectamente cuando se hace referencia a ella desde otra plantilla de solución o aplicación administrada, seleccione esta casilla para publicar la máquina virtual, pero ocultarla para los clientes que puedan buscarla directamente.

Cualquier cliente de Azure puede implementar la oferta mediante PowerShell o la CLI.  Si quiere que esta oferta esté disponible para un conjunto limitado de clientes, establezca el plan en **Privado**.

Los planes ocultos no generan vínculos de vista previa. Pero puede probarlos [si sigue estos pasos](azure-vm-create-faq.yml#how-do-i-test-a-hidden-preview-image-).

Seleccione **Guardar borrador** antes de pasar a la pestaña siguiente del menú de navegación de la izquierda Plan, **Configuración técnica**.

## <a name="technical-configuration"></a>Configuración técnica

Proporcione las imágenes y otras propiedades técnicas asociadas al plan.

### <a name="reuse-technical-configuration"></a>Reutilización de la configuración técnica

Esta opción permite usar los mismos valores de configuración técnica en los planes de la misma oferta y, por tanto, sacar provecho del mismo conjunto de imágenes. Si habilita la opción de _reutilización de la configuración técnica_, el plan heredará las mismas opciones de configuración técnica que el plan base que seleccione.  Al cambiar el plan base, los cambios se reflejan en el plan que reutiliza la configuración.

Entre las razones habituales para reutilizar las opciones de configuración técnica de otro plan se incluyen:

1. Las mismas imágenes están disponibles para *Pago por uso* y *Traiga su propia licencia*.
2. Para reutilizar la misma configuración técnica de un plan público para un plan privado con un precio diferente. 
3. La solución se comporta de forma diferente en función del plan que el usuario decida implementar. Por ejemplo, el software es el mismo, pero las características varían según el plan.

Aproveche [Azure Instance Metadata Service](../virtual-machines/windows/instance-metadata-service.md) (IMDS) para identificar en qué plan se implementa la solución para validar la licencia o habilitar las características adecuadas.

Si más adelante decide publicar cambios diferentes entre los planes, puede separarlos. Desasocie el plan que está reutilizando la configuración técnica mediante la anulación de la selección de esta opción con el plan. Una vez desasociado, el plan llevará las mismas opciones de configuración técnica en lugar de la última configuración y los planes podrán diferir en la configuración. Un plan que se ha publicado de forma independiente en el pasado no puede reutilizar una configuración técnica más adelante.

### <a name="operating-system"></a>Sistema operativo

Seleccione la familia de sistema operativo **Windows** o **Linux**.

Seleccione la **versión** de Windows o el **proveedor** de Linux.

Escriba un **nombre descriptivo de SO** para el sistema operativo. Este nombre es visible para los clientes.

### <a name="recommended-vm-sizes"></a>Recommended VM Sizes (Tamaños de máquina virtual recomendados)

Seleccione el vínculo para elegir hasta seis tamaños de máquina virtual recomendados para que se muestren en Azure Marketplace.

### <a name="open-ports"></a>Abrir puertos

Agregue puertos públicos o privados abiertos en una máquina virtual implementada.

### <a name="properties"></a>Propiedades

Esta es una lista de las propiedades que se pueden seleccionar para la máquina virtual.

- **Admite copia de seguridad**: habilite esta propiedad si las imágenes admiten la copia de seguridad de máquinas virtuales de Azure. Más información sobre la [copia de seguridad de máquinas virtuales de Azure](../backup/backup-azure-vms-introduction.md).

- **Admite redes aceleradas**: habilite esta propiedad si las imágenes de máquina virtual de este plan admiten la virtualización de E/S de raíz única (SR-IOV) en una máquina virtual, lo que permite una latencia baja y un alto rendimiento en la interfaz de red. Más información sobre las [redes aceleradas](https://go.microsoft.com/fwlink/?linkid=2124513).

- **Admite la configuración de cloud-init**: habilite esta propiedad si las imágenes de este plan admiten scripts posteriores a la implementación de cloud-init. Más información sobre la [configuración de cloud-init](../virtual-machines/linux/using-cloud-init.md).

- **Admite la revisión en caliente**: las ediciones de Windows Server para Azure admiten la revisión en caliente. Más información sobre [la revisión en caliente](../automanage/automanage-hotpatch.md).

- **Admite extensiones**: habilite esta propiedad si las imágenes de este plan admiten extensiones. Las extensiones son aplicaciones pequeñas que proporcionan automatización y configuración posterior a la implementación en VM de Azure. Más información sobre las [extensiones de máquina virtual de Azure](./azure-vm-create-certification-faq.yml#vm-extensions).

- **Es una aplicación virtual de red**: habilite esta propiedad si este producto es una aplicación virtual de red. Una aplicación virtual de red es un producto que realiza una o varias funciones de red, como un equilibrador de carga, una puerta de enlace de VPN, un firewall o una puerta de enlace de aplicación. Más información sobre las [aplicaciones virtuales de red](https://go.microsoft.com/fwlink/?linkid=2155373).

- **Escritorio remoto o SSH deshabilitado**: habilite esta propiedad si las máquinas virtuales implementadas con estas imágenes no permiten a los clientes el acceso mediante Escritorio remoto o SSH. Más información sobre [imágenes de máquina virtual bloqueadas](./azure-vm-create-certification-faq.yml#locked-down-or-ssh-disabled-offer).

- **Requiere una plantilla de ARM personalizada para la implementación**: habilite esta propiedad si las imágenes de este plan solo se pueden implementar mediante una plantilla de ARM personalizada. Para obtener más información, vea la sección [Plantillas personalizadas de Solución de problemas de certificación de máquinas virtuales](./azure-vm-create-certification-faq.yml#custom-templates).

### <a name="generations"></a>Generaciones

La generación de una máquina virtual define el hardware virtual que usa. En función de las necesidades de sus clientes, puede publicar una máquina virtual de generación 1, una máquina virtual de generación 2 o ambas.

1. Al crear una oferta, seleccione un **tipo de generación** y escriba los detalles solicitados:

    :::image type="content" source="./media/create-vm/azure-vm-generations-image-details-1.png" alt-text="Una vista de la sección de detalles de la generación del Centro de partners":::.

2. Para agregar otra generación a un plan, seleccione **Agregar generación**…

    :::image type="content" source="./media/create-vm/azure-vm-generations-add.png" alt-text="Vista del enlace &quot;Add Generation&quot; (Agregar generación)":::.

    … y escriba los detalles solicitados:

    :::image type="content" source="./media/create-vm/azure-vm-generations-image-details-3.png" alt-text="Una vista de la ventana de los detalles de la generación":::.

<!--    The **Generation ID** you choose will be visible to customers in places such as product URLs and ARM templates (if applicable). Use only lowercase, alphanumeric characters, dashes, or underscores; it cannot be modified once published.
-->
3. Para actualizar una máquina virtual existente que tiene una generación 1 ya publicada, edite los detalles en la página **Configuración técnica**.

Para obtener más información sobre las diferencias entre las funcionalidades de la generación 1 y generación 2, vea [Compatibilidad para máquinas virtuales de generación 2 en Azure](../virtual-machines/generation-2.md).

> [!NOTE]
> Una generación publicada requiere que al menos una versión de la imagen permanezca disponible para los clientes. Para quitar todo el plan (junto con todas sus generaciones e imágenes), seleccione **Plan en desuso** en la página **Información general del plan** (consulte la primera sección de este artículo).

### <a name="vm-images"></a>Imágenes de VM

Proporcione una versión de disco y el identificador URI de firma de acceso compartido (SAS) para las imágenes de máquina virtual. Agregue hasta 16 discos de datos para cada imagen de máquina virtual. Proporcione solo una nueva versión de imagen por plan en un envío especificado. Una vez publicada una imagen no se puede editar, pero se puede eliminar. La eliminación de una versión impide que los usuarios nuevos y existentes implementen una nueva instancia de la versión eliminada.

Estos dos campos obligatorios se muestran en la imagen anterior:

- **Versión del disco**: es la versión de la imagen que se va a proporcionar.
- **Vínculo VHD del sistema operativo**: La imagen capturada en Azure Compute Gallery (anteriormente denominada Shared Image Gallery). Aprenda cómo capturar la imagen en una [Azure Compute Gallery](azure-vm-create-using-approved-base.md#capture-image).

Los discos de datos (seleccione **Agregar disco de datos (máximo 16)** ) también son identificadores URI de firma de acceso compartido de discos duros virtuales que se almacenan en sus cuentas de Azure Storage. Agregue solo una imagen por envío en un plan.

Independientemente del sistema operativo que use, agregue solo el número mínimo de discos de datos que necesite la solución. Durante la implementación, los usuarios no pueden eliminar los discos que forman parte de una imagen, pero siempre pueden agregar discos durante o después de la implementación.

> [!NOTE]
> Si proporciona las imágenes mediante SAS y tiene discos de datos, también debe proporcionarlas como URI de SAS. Si usa una imagen compartida, se capturan como parte de la imagen en Azure Compute Gallery. Una vez publicada la oferta en Azure Marketplace, puede eliminar la imagen del almacenamiento de Azure o de Azure Compute Gallery.

Seleccione **Guardar borrador** y, luego, seleccione **← Información general del plan** en la parte superior izquierda para ver el plan que acaba de crear.

Una vez publicada la imagen de máquina virtual, puede eliminarla de Azure Storage.

## <a name="next-steps"></a>Pasos siguientes

- [Revender mediante los CSP](azure-vm-create-resell-csp.md)
