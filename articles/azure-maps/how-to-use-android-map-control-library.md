---
title: Introducción al control de mapa de Android | Microsoft Azure Maps
description: Familiarícese con el SDK para Android de Azure Maps. Vea cómo crear un proyecto en Android Studio, instalar el SDK y crear un mapa interactivo.
author: stevemunk
ms.author: v-munksteve
ms.date: 2/26/2021
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: eriklind
zone_pivot_groups: azure-maps-android
ms.openlocfilehash: 137b0d56a566d2e62f5559a30b391e6418139e59
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "130005079"
---
# <a name="getting-started-with-azure-maps-android-sdk"></a>Introducción a Android SDK para Azure Maps

Android SDK para Azure Maps es una biblioteca de mapas vectoriales para Android. En este artículo se ofrece orientación sobre los procesos para instalar Android SDK para Azure Maps y cargar un mapa.

## <a name="prerequisites"></a>Prerrequisitos

Asegúrese de completar los pasos descritos en el artículo [Inicio rápido: creación de una aplicación para Android](quick-android-map.md).

## <a name="localizing-the-map"></a>Localización del mapa

El SDK de Android de Azure Maps proporciona tres formas de establecer el idioma y la vista regional del mapa. En el código siguiente se muestra cómo establecer el idioma en francés ("fr-FR") y la vista regional en "Auto".

1. Pase el idioma y la información de la vista regional en la clase `AzureMaps` mediante las propiedades `setLanguage` y `setView` estáticas. De esta forma, se establecen las propiedades de idioma predeterminado y vista regional en la aplicación.
    
    ::: zone pivot="programming-language-java-android"
    
    ```java
    static {
        //Alternatively use Azure Active Directory authenticate.
        AzureMaps.setAadProperties("<Your aad clientId>", "<Your aad AppId>", "<Your aad Tenant>");
    
        //Set your Azure Maps Key.
        //AzureMaps.setSubscriptionKey("<Your Azure Maps Key>");   
    
        //Set the language to be used by Azure Maps.
        AzureMaps.setLanguage("fr-FR");
    
        //Set the regional view to be used by Azure Maps.
        AzureMaps.setView("Auto");
    }
    ```
    
    ::: zone-end
    
    ::: zone pivot="programming-language-kotlin"
    
    ```kotlin
    companion object {
        init {
            //Alternatively use Azure Active Directory authenticate.
            AzureMaps.setAadProperties("<Your aad clientId>", "<Your aad AppId>", "<Your aad Tenant>");
    
            //Set your Azure Maps Key.
            //AzureMaps.setSubscriptionKey("<Your Azure Maps Key>");
        
            //Set the language to be used by Azure Maps.
            AzureMaps.setLanguage("fr-FR");
        
            //Set the regional view to be used by Azure Maps.
            AzureMaps.setView("Auto");
        }
    }
    ```
    
    ::: zone-end

1. También puede pasar la información de idioma y vista regional al XML del control de mapa.

    ```XML
    <com.azure.android.maps.control.MapControl
        android:id="@+id/myMap"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:azure_maps_language="fr-FR"
        app:azure_maps_view="Auto"
        />
    ```
1. La última manera de establecer mediante programación las propiedades de idioma y vista regional usa el método `setStyle` de mapas. Esta opción puede realizarse en cualquier momento para cambiar el idioma y la vista regional del mapa.

    ::: zone pivot="programming-language-java-android"

    ```java
    mapControl.onReady(map -> {
        map.setStyle(
            language("fr-FR"),
            view("Auto")
        );
    });
    ```

    ::: zone-end

    ::: zone pivot="programming-language-kotlin"

    ```kotlin
    mapControl.onReady(OnReady { map: AzureMap ->
        map.setStyle(
            language("fr-FR"),
            view("Auto")
        )
    })
    ```

    ::: zone-end

Este es un ejemplo de Azure Maps con el idioma establecido en "fr-FR" y la vista regional establecida en "Auto".

![Imagen del mapa de Azure Maps que muestra las etiquetas en francés](media/how-to-use-android-map-control-library/android-localization.png)

Para obtener una lista completa de los idiomas admitidos y las vistas regionales, consulte [Compatibilidad con la localización en Azure Maps](supported-languages.md).

## <a name="navigating-the-map"></a>Navegación por el mapa

Hay varias maneras de ampliar, desplazar lateralmente, girar e inclinar el mapa. A continuación se detallan todas las formas de navegar por el mapa.

### <a name="zoom-the-map"></a>Acercamiento/alejamiento del mapa

* Toque el mapa con dos dedos y acérquelos para alejarse o aléjelos para acercarse.
* Pulse dos veces el mapa para acercarse un nivel.
* Pulse dos veces con dos dedos para alejar el mapa un nivel.
* Pulse dos veces; en la segunda pulsación, mantenga presionado el dedo en el mapa y arrástrelo hacia arriba o hacia abajo para acercar o alejar la imagen, respectivamente.

### <a name="pan-the-map"></a>Desplazamiento lateral del mapa

* Toque el mapa y arrastre en cualquier dirección.

### <a name="rotate-the-map"></a>Giro del mapa

* Toque el mapa con dos dedos y gírelos.

### <a name="pitch-the-map"></a>Inclinación del mapa

* Toque el mapa con dos dedos y arrástrelos hacia arriba o hacia abajo.

## <a name="azure-government-cloud-support"></a>Compatibilidad con la nube de Azure Government

Android SDK de Azure Maps es compatible con la nube de Azure Government. Se accede a Android SDK de Azure Maps desde el mismo repositorio de Maven. Se deben realizar las siguientes tareas para conectarse a la versión en la nube de Azure Government de la plataforma de Azure Maps.

En el mismo lugar donde se especifican los detalles de autenticación de Azure Maps, agregue la línea de código siguiente para indicar al mapa que use el dominio de nube del gobierno de Azure Maps.

::: zone pivot="programming-language-java-android"

```java
AzureMaps.setDomain("atlas.azure.us");
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
AzureMaps.setDomain("atlas.azure.us")
```

::: zone-end

No olvide usar los detalles de autenticación de Azure Maps de la plataforma en la nube de Azure Government cuando autentique el mapa y los servicios.

## <a name="migrating-from-a-preview-version"></a>Migración desde una versión preliminar

Con el paso de la versión preliminar a la disponibilidad general, se introdujeron algunos cambios importantes en Android SDK de Azure Maps. Estos son los detalles clave:

* El identificador de Maven ha cambiado de `"com.microsoft.azure.maps:mapcontrol:0.7"` a `"com.azure.android:azure-maps-control:1.0.0"`. El espacio de nombres y el número de versión principal han cambiado.
* El espacio de nombres importado ha cambiado de `com.microsoft.azure.maps.mapcontrol` a `com.azure.android.maps.control`.
* Se ha reemplazado el texto `mapcontrol_` por `azure_maps_` en los nombres de recurso para las opciones XML, los recursos de color y los recursos de imagen.

    **Antes:**

    ```xml
    <com.microsoft.azure.maps.mapcontrol.MapControl
        android:id="@+id/myMap"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:mapcontrol_language="fr-FR"
        app:mapcontrol_view="Auto"
        app:mapcontrol_centerLat="47.602806"
        app:mapcontrol_centerLng="-122.329330"
        app:mapcontrol_zoom="12"
    />
    ```

    **Después:**

    ```xml
    <com.azure.android.maps.control.MapControl
        android:id="@+id/myMap"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:azure_maps_language="fr-FR"
        app:azure_maps_view="Auto"
        app:azure_maps_centerLat="47.602806"
        app:azure_maps_centerLng="-122.329330"
        app:azure_maps_zoom="12"
    />
    ```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo agregar datos de superposición en el mapa:

> [!div class="nextstepaction"]
> [Administración de la autenticación en Azure Maps](how-to-manage-authentication.md)

> [!div class="nextstepaction"]
> [Cambio de estilos de mapa en mapas de Android](set-android-map-styles.md)

> [!div class="nextstepaction"]
> [Adición de una capa de símbolo](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Adición de una capa de línea](android-map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Adición de una capa de polígono](how-to-add-shapes-to-android-map.md)
