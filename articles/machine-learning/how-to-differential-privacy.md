---
title: Procedimiento de privacidad diferencial para SmartNoise (versión preliminar)
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo aplicar procedimientos recomendados de privacidad diferencial a los modelos de Azure Machine Learning mediante las bibliotecas de código abierto de SmartNoise.
services: machine-learning
ms.service: machine-learning
ms.subservice: enterprise-readiness
ms.topic: how-to
ms.custom: responsible-ml
ms.author: slbird
author: slbird
ms.reviewer: luquinta
ms.date: 10/21/2021
ms.openlocfilehash: 98790b7e4e78d98936f80dd46f1ba83b7a4e945a
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131561544"
---
# <a name="use-differential-privacy-in-azure-machine-learning-preview"></a>Uso de la privacidad diferencial en Azure Machine Learning (versión preliminar)

Obtenga información sobre cómo aplicar procedimientos recomendados de privacidad diferencial a los modelos de Azure Machine Learning mediante las bibliotecas de código abierto Python de SmartNoise.

La privacidad diferencial es la definición de referencia de la privacidad. Los sistemas que se adhieren a esta definición de privacidad proporcionan garantías sólidas contra una amplia gama de ataques de reidentificación y reconstrucción de datos, incluidos los ataques de los adversarios que poseen información complementaria. Obtenga información sobre [cómo funciona la privacidad diferencial](./concept-differential-privacy.md).


## <a name="prerequisites"></a>Prerrequisitos

- Si no tiene una suscripción de Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://azure.microsoft.com/free/).
- [Python 3](https://www.python.org/downloads/)

## <a name="install-smartnoise-python-libraries"></a>Instalación de las bibliotecas Python de SmartNoise

### <a name="standalone-installation"></a>Instalación independiente

Las bibliotecas están diseñadas para funcionar desde clústeres de Spark distribuidos y se pueden instalar como cualquier otro paquete.

En las instrucciones siguientes se supone que los comandos `python` y `pip` están asignados a `python3` y `pip3`.

Use PIP para instalar los [paquetes de Python de SmartNoise](https://pypi.org/project/opendp-smartnoise/).

`pip install opendp-smartnoise`

Para comprobar que los paquetes están instalados, inicie un símbolo del sistema de Python y escriba:

```python
import opendp.smartnoise.core
import opendp.smartnoise.sql
```

Si las importaciones se realizan correctamente, las bibliotecas se instalan y están listas para usar.

### <a name="docker-image-installation"></a>Instalación de una imagen de Docker

También puede usar paquetes de SmartNoise con Docker.

Extraiga la imagen de `opendp/smartnoise` para usar las bibliotecas dentro de un contenedor de Docker que incluye Spark, Jupyter y código de ejemplo.

[!INCLUDE [pull-image-include](../../includes/pull-image-include.md)]

```sh
docker pull opendp/smartnoise:privacy
```

Una vez que haya extraído la imagen, inicie el servidor de Jupyter:

```sh
docker run --rm -p 8989:8989 --name smartnoise-run opendp/smartnoise:privacy
```

Se inicia un servidor de Jupyter en el puerto `8989` en su `localhost`, con la contraseña `pass@word99`. Suponiendo que ha usado la línea de comandos anterior para iniciar el contenedor con el nombre `smartnoise-privacy`, puede abrir un terminal de bash en el servidor de Jupyter mediante el siguiente comando:

```sh
docker exec -it smartnoise-run bash
```

La instancia de Docker borra todo el estado al apagar, por lo que perderá todos los cuadernos que cree en la instancia en ejecución. Para solucionarlo, puede realizar un montaje de enlace de una carpeta local al contenedor cuando lo inicie:

```sh
docker run --rm -p 8989:8989 --name smartnoise-run --mount type=bind,source=/Users/your_name/my-notebooks,target=/home/privacy/my-notebooks opendp/smartnoise:privacy
```

Los cuadernos que cree en la carpeta *my-notebooks* se almacenarán en el sistema de archivos local.

## <a name="perform-data-analysis"></a>Realización de análisis de datos

Para preparar una versión privada diferencial, debe elegir un origen de datos, una estadística y algunos parámetros de privacidad, que indican el nivel de protección de la privacidad.  

Este ejemplo hace referencia a Public Use Microdata Sample (PUMS) de California, que representa los registros anonimizados de datos demográficos de los ciudadanos:

```python
import os
import sys
import numpy as np
import opendp.smartnoise.core as sn

data_path = os.path.join('.', 'data', 'PUMS_california_demographics_1000', 'data.csv')
var_names = ["age", "sex", "educ", "race", "income", "married", "pid"]
```

En este ejemplo, se calculan la media y la varianza de la edad.  Usamos un `epsilon` total de 1,0 (epsilon es nuestro parámetro de privacidad, que redistribuye nuestro presupuesto de privacidad entre las dos cantidades que queremos calcular. Obtenga más información sobre las [métricas de privacidad](concept-differential-privacy.md#differential-privacy-metrics).

```python
with sn.Analysis() as analysis:
    # load data
    data = sn.Dataset(path = data_path, column_names = var_names)

    # get mean of age
    age_mean = sn.dp_mean(data = sn.cast(data['age'], type="FLOAT"),
                          privacy_usage = {'epsilon': .65},
                          data_lower = 0.,
                          data_upper = 100.,
                          data_n = 1000
                         )
    # get variance of age
    age_var = sn.dp_variance(data = sn.cast(data['age'], type="FLOAT"),
                             privacy_usage = {'epsilon': .35},
                             data_lower = 0.,
                             data_upper = 100.,
                             data_n = 1000
                            )
analysis.release()

print("DP mean of age: {0}".format(age_mean.value))
print("DP variance of age: {0}".format(age_var.value))
print("Privacy usage: {0}".format(analysis.privacy_usage))
```

El resultado debería ser similar al que se muestra a continuación:

```text
DP mean of age: 44.55598845931517
DP variance of age: 231.79044646429134
Privacy usage: approximate {
  epsilon: 1.0
}
```

Hay algunos aspectos importantes que hay que tener en cuenta sobre este ejemplo.  En primer lugar, el objeto `Analysis` representa un gráfico de procesamiento de datos.  En este ejemplo, la media y la varianza se calculan a partir del mismo nodo de origen.  Sin embargo, puede incluir expresiones más complejas que combinen entradas con salidas de maneras arbitrarias.

El gráfico de análisis incluye los metadatos `data_upper` y `data_lower`, que especifican los límites inferior y superior de las edades.  Estos valores se usan para calibrar con precisión el ruido a fin de garantizar la privacidad diferencial.  Estos valores también se usan en algunos controles de valores atípicos o valores que faltan.

Por último, el gráfico de análisis realiza un seguimiento del presupuesto total de privacidad empleado.

Puede usar la biblioteca para crear gráficos de análisis más complejos, con varios mecanismos, estadísticas y funciones de utilidad:

| Estadísticas    | Mecanismos | Sectores públicos  |
| ------------- |------------|------------|
| Count         | Gausiano   | Conversión       |
| Histograma     | Geométrico  | Sujeción   |
| Media          | Laplace    | Digitalización   |
| Cuantiles     |            | Filter     |
| Sum           |            | Imputación |
| Varianza/covarianza |      | Transformación  |

Consulte el [cuaderno de análisis de datos](https://github.com/opendifferentialprivacy/smartnoise-samples/blob/master/analysis/basic_data_analysis.ipynb) para obtener más información.

## <a name="approximate-utility-of-differentially-private-releases"></a>Utilidad aproximada de versiones privadas de forma diferencial

Dado que la privacidad diferencial funciona mediante la calibración del ruido, la utilidad de versiones puede variar en función del riesgo de privacidad.  Por lo general, el ruido necesario para proteger a cada individuo se vuelve insignificante a medida que los tamaños de ejemplo aumentan de tamaño, pero desbordan el resultado de las versiones que tienen como destino una sola persona.  Los analistas pueden revisar la información de precisión de una versión para determinar el grado de utilidad de la versión:

```python
with sn.Analysis() as analysis:
    # load data
    data = sn.Dataset(path = data_path, column_names = var_names)

    # get mean of age
    age_mean = sn.dp_mean(data = sn.cast(data['age'], type="FLOAT"),
                          privacy_usage = {'epsilon': .65},
                          data_lower = 0.,
                          data_upper = 100.,
                          data_n = 1000
                         )
analysis.release()

print("Age accuracy is: {0}".format(age_mean.get_accuracy(0.05)))
```

El resultado de esa operación debe ser similar al siguiente:

```text
Age accuracy is: 0.2995732273553991
```

En este ejemplo se calcula la media con el procedimiento anterior y se usa la función `get_accuracy` para solicitar precisión en `alpha` de 0,05. Un valor de `alpha` de 0,05 representa un intervalo del 95 %, en que el valor publicado estará dentro de los límites de precisión indicados aproximadamente el 95 % del tiempo.  En este ejemplo, la precisión indicada es 0,3, lo que significa que el valor publicado estará dentro de un intervalo de ancho 0,6, aproximadamente el 95 % del tiempo.  No es correcto pensar en este valor como una barra de error, ya que el valor publicado quedará fuera del intervalo de precisión indicado en la tasa especificada por `alpha`, y los valores fuera del intervalo pueden estar fuera en cualquier dirección.

Los analistas pueden consultar `get_accuracy` para obtener los distintos valores de `alpha` y obtener intervalos de confianza más estrechos o más amplios, sin incurrir en costos de privacidad adicionales.

## <a name="generate-a-histogram"></a>Generación de un histograma

La función integrada `dp_histogram` crea histogramas privados de forma diferencial sobre cualquiera de los siguientes tipos de datos:

- Una variable continua, donde el conjunto de números tiene que dividirse en intervalos.
- Una variable booleana o dicotómica que solo puede tomar dos valores.
- Una variable categórica, donde hay categorías distintas enumeradas como cadenas.

A continuación se muestra un ejemplo de `Analysis` que especifica los intervalos de un histograma de variables continuas:

```python
income_edges = list(range(0, 100000, 10000))

with sn.Analysis() as analysis:
    data = sn.Dataset(path = data_path, column_names = var_names)

    income_histogram = sn.dp_histogram(
            sn.cast(data['income'], type='int', lower=0, upper=100),
            edges = income_edges,
            upper = 1000,
            null_value = 150,
            privacy_usage = {'epsilon': 0.5}
        )
```

Dado que los individuos se particionan de forma separada entre los intervalos del histograma, el costo de la privacidad se incurre solo una vez por histograma, aunque el histograma incluya muchos intervalos.

Para obtener más información sobre los histogramas, consulte el [cuaderno de histogramas](https://github.com/opendifferentialprivacy/smartnoise-samples/blob/master/analysis/histograms.ipynb).

## <a name="generate-a-covariance-matrix"></a>Generación de una matriz de covarianza

SmartNoise ofrece tres funcionalidades diferentes con su función `dp_covariance`:

- Covarianza entre dos vectores
- Matriz de covarianza de una matriz
- Matriz de covarianza cruzada de un par de matrices

Este es un ejemplo de cómo calcular una covarianza escalar:

```python
with sn.Analysis() as analysis:
    wn_data = sn.Dataset(path = data_path, column_names = var_names)

    age_income_cov_scalar = sn.dp_covariance(
      left = sn.cast(wn_data['age'], 
      type = "FLOAT"), 
      right = sn.cast(wn_data['income'], 
      type = "FLOAT"), 
      privacy_usage = {'epsilon': 1.0},
      left_lower = 0., 
      left_upper = 100., 
      left_n = 1000, 
      right_lower = 0., 
      right_upper = 500_000.,
      right_n = 1000)
```

Para más información, consulte el [cuaderno de covarianza](
https://github.com/opendifferentialprivacy/smartnoise-samples/blob/master/analysis/covariance.ipynb).

## <a name="next-steps"></a>Pasos siguientes

- Explore los [cuadernos de ejemplo de SmartNoise](https://github.com/opendifferentialprivacy/smartnoise-samples/tree/master/analysis).
