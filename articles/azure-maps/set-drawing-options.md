---
title: Módulo de herramientas de dibujo | Microsoft Azure Maps
description: En este artículo, obtendrá información sobre cómo establecer datos de opciones de dibujo mediante el SDK web de Microsoft Azure Maps
author: anastasia-ms
ms.author: v-stharr
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-maps
ms.custom: devx-track-js
ms.openlocfilehash: fd861e4ab92235ec4f2b3ec8051e854a08effd3c
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131072377"
---
# <a name="use-the-drawing-tools-module"></a>Uso del módulo de herramientas de dibujo

El SDK web de Azure Maps proporciona un *módulo de herramientas de dibujo*. Este módulo facilita el dibujo y la edición de formas en el mapa mediante un dispositivo de entrada, como un mouse o la pantalla táctil. La clase principal de este módulo es el [administrador de dibujos](/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager#setoptions-drawingmanageroptions-). El administrador de dibujos proporciona todas las funcionalidades necesarias para dibujar y editar formas en el mapa. Se puede usar directamente y se integra con una interfaz de usuario de barra de herramientas personalizada. También puede usar la clase integrada [drawing toolbar](/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar).

## <a name="loading-the-drawing-tools-module-in-a-webpage"></a>Carga del módulo de herramientas de dibujo en una página web

1. Cree un archivo HTML e [implemente el mapa como de costumbre](./how-to-use-map-control.md).
2. Cargue el módulo de herramientas de dibujo de Azure Maps. Puede realizar ese procedimiento de alguna de estas dos formas:
    - Utilizar la versión de Azure Content Delivery Network hospedada globalmente del módulo de servicios de Azure Maps. Agregue una referencia a la hoja de estilos de JavaScript y CSS en el elemento `<head>` del archivo:

        ```html
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/drawing/1/atlas-drawing.min.css" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/javascript/drawing/1/atlas-drawing.min.js"></script>
        ```

    - Otra alternativa es cargar localmente el módulo de herramientas de dibujo para el código fuente del SDK web de Azure Maps mediante el paquete npm [azure-maps-drawing-tools](https://www.npmjs.com/package/azure-maps-drawing-tools) y, luego, hospedarlo con la aplicación. Este paquete también incluye las definiciones de TypeScript. Use este comando:

      `npm install azure-maps-drawing-tools`

      Luego, agregue una referencia a la hoja de estilos de JavaScript y CSS en el elemento `<head>` del archivo:

      ```html
      <link rel="stylesheet" href="node_modules/azure-maps-drawing-tools/dist/atlas-drawing.min.css" type="text/css" />
      <script src="node_modules/azure-maps-drawing-tools/dist/atlas-drawing.min.js"></script>
      ```

## <a name="use-the-drawing-manager-directly"></a>Uso del administrador de dibujos directamente

Una vez que el módulo de herramientas de dibujo se carga en la aplicación, puede habilitar funcionalidades de dibujo y edición mediante el [administrador de dibujos](/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager#setoptions-drawingmanageroptions-). Puede especificar opciones para el administrador de dibujo al crear una instancia de él, o bien usar la función `drawingManager.setOptions()`.

### <a name="set-the-drawing-mode"></a>Establecimiento del modo de dibujo

En el código siguiente crea una instancia del administrador de dibujos y establece la opción **modo** de dibujo. 

```javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon"
});
```

El código siguiente es un ejemplo de ejecución completo de cómo establecer un modo de dibujo del administrador de dibujos. Haga clic en el mapa para empezar a dibujar un polígono.

<br/>

<iframe height="500" scrolling="no" title="Dibuje un polígono" src="//codepen.io/azuremaps/embed/YzKVKRa/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/YzKVKRa/'>Dibujar un polígono</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="set-the-interaction-type"></a>Establecimiento del tipo de interacción

El administrador de dibujo admite tres formas diferentes de interactuar con el mapa para dibujar formas.

* `click`: se agregan coordenadas cuando se hace clic en el mouse o en la entrada táctil.
* `freehand `: se agregan coordenadas cuando se arrastra el mouse o la entrada táctil en el mapa. 
* `hybrid`: se agregan coordenadas cuando se hace clic o se arrastra el mouse o la entrada táctil.

El código siguiente habilita el modo de dibujo de polígonos y establece el tipo de interacción de dibujo que debe seguir el administrador de dibujo en `freehand`. 

```javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon",
    interactionType: "freehand"
});
```

 En este ejemplo de código se implementa la funcionalidad de dibujar un polígono en el mapa. Simplemente mantenga presionado el botón primario del mouse y arrastre por donde quiera.

<br/>

<iframe height="500" scrolling="no" title="Dibujo a mano alzada" src="//codepen.io/azuremaps/embed/ZEzKoaj/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/ZEzKoaj/'>Dibujo a mano alzada</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="customizing-drawing-options"></a>Personalizar las opciones de dibujo

En los ejemplos anteriores se ha mostrado cómo personalizar las opciones de dibujo al crear una instancia del administrador de dibujos. También puede establecer las opciones del administrador de dibujo mediante la función `drawingManager.setOptions()`. A continuación se muestra una herramienta para probar la personalización de todas las opciones del administrador de dibujos mediante la función setOptions.

<br/>

<iframe height="685" title="Personalizar el administrador de dibujos" src="//codepen.io/azuremaps/embed/LYPyrxR/?height=600&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">Consulte el Pen <a href='https://codepen.io/azuremaps/pen/LYPyrxR/'>Obtención de datos de forma</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="put-a-shape-into-edit-mode"></a>Puesta de una forma en modo de edición

Ponga una forma existente en modo de edición mediante programación pasándola a la función `edit` del administrador de dibujos. Si la forma es una característica GeoJSON, inclúyala entre la clase `atls.Shape` antes de pasarla.

Para sacar una forma del modo de edición mediante programación, establezca el modo del administrador de dibujos en `idle`.

```javascript
//If you are starting with a GeoJSON feature, wrap it with the atlas.Shape class.
var feature = { 
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [0,0]
        },
    "properties":  {}
};

var shape = new atlas.Shape(feature);

//Pass the shape into the edit function of the drawing manager.
drawingManager.edit(shape);

//Later, to programmatically take shape out of edit mode, set mode to idle. 
drawingManager.setOptions({ mode: 'idle' });
```

> [!NOTE]
> Cuando se pasa una forma a la función `edit` del administrador de dibujos, se agrega al origen de datos que mantiene el administrador. Si la forma estaba anteriormente en otro origen de datos, se eliminará de dicho origen.

Si desea agregar al administrador de dibujos formas que el usuario final pueda ver y editar, pero no quiere ponerlas en modo de edición mediante programación, recupere el origen de datos del administrador y agréguele las formas.

```javascript
//The shape(s) you want to add to the drawing manager so 
var shape = new atlas.Shape(feature);

//Retrieve the data source from the drawing manager.
var source = drawingManager.getSource();

//Add your shape.
source.add(shape);

//Alternatively, load in a GeoJSON feed using the sources importDataFromUrl function.
source.importDataFromUrl('yourFeatures.json');
```

En la tabla siguiente se muestra el tipo de edición que admiten diferentes tipos de características de forma.

| Característica de forma | Editar puntos | Girar | Eliminar forma |
|---------------|:-----------:|:------:|:------------:|
| Punto         | ✓           |        | ✓           |
| LineString    | ✓           | ✓      | ✓           |
| Polygon       | ✓           | ✓      | ✓           |
| MultiPoint    |             | ✓      | ✓           |
| MultiLineString |           | ✓      | ✓           |
| MultiPolygon  |             | ✓      | ✓           |
| Circle        | ✓           |        | ✓           |
| Rectángulo     | ✓           | ✓      | ✓           |

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo usar características adicionales del módulo de herramientas de dibujo:

> [!div class="nextstepaction"]
> [Incorporación de una barra de herramientas de dibujo](map-add-drawing-toolbar.md)

> [!div class="nextstepaction"]
> [Obtención de datos de forma](map-get-shape-data.md)

> [!div class="nextstepaction"]
> [Reacción a eventos de dibujo](drawing-tools-events.md)

> [!div class="nextstepaction"]
> [Tipos de interacción y métodos abreviados de teclado](drawing-tools-interactions-keyboard-shortcuts.md)

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](/javascript/api/azure-maps-control/atlas.map)

> [!div class="nextstepaction"]
> [Administrador de dibujo](/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager)

> [!div class="nextstepaction"]
> [Barra de herramientas de dibujo](/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar)
