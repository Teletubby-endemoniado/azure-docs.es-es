---
title: Crear un controlador de entrada
titleSuffix: Azure Kubernetes Service
description: Aprenda a instalar y configurar un controlador de entrada NGINX básico en un clúster de Azure Kubernetes Service (AKS).
services: container-service
ms.topic: article
ms.date: 04/23/2021
ms.openlocfilehash: 9353f3f19e2c8939600ebcc937d145ee72863819
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132553655"
---
# <a name="create-an-ingress-controller-in-azure-kubernetes-service-aks"></a>Creación de un controlador de entrada en Azure Kubernetes Service (AKS)

Un controlador de entrada es un software que proporciona el proxy inverso, el enrutamiento del tráfico configurable y la terminación de TLS para los servicios de Kubernetes. Los recursos de entrada de Kubernetes se usan para configurar las reglas de entrada y las rutas de los distintos servicios de Kubernetes. Mediante reglas de entrada y un controlador de entrada, se puede usar una sola dirección IP para enrutar el tráfico a varios servicios en un clúster de Kubernetes.

En este artículo se muestra cómo implementar el [controlador de entrada NGINX][nginx-ingress] en un clúster de Azure Kubernetes Service (AKS). Se ejecutan dos aplicaciones en el clúster de AKS, a las que se puede acceder con una sola dirección IP.

> [!NOTE]
> Hay dos controladores de entrada de código abierto para Kubernetes basados en Nginx: uno lo mantiene la comunidad de Kubernetes ([kubernetes/ingress-nginx][nginx-ingress]) y el otro lo mantiene NGINX, Inc. ([nginxinc/kubernetes-ingress]). En este artículo se usa el controlador de entrada de la comunidad de Kubernetes. 

Como alternativa, puede:

- [Habilitación del complemento de enrutamiento de aplicación HTTP][aks-http-app-routing]
- [Creación de un controlador de entrada que use una red privada interna y una dirección IP][aks-ingress-internal]
- [Crear un controlador de entrada que usa sus propios certificados TLS][aks-ingress-own-tls]
- Creación de un controlador de entrada que use Let's Encrypt para generar certificados TLS de forma automática [con una dirección IP pública dinámica][aks-ingress-tls] o [con una dirección IP pública estática][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se usa [Helm 3][helm] para instalar el controlador de entrada NGINX en una [versión de Kubernetes compatible][aks-supported versions]. Asegúrese de usar la versión más reciente de Helm y de tener acceso al repositorio *ingress-nginx* de Helm. Es posible que los pasos descritos en este artículo no sean compatibles con versiones anteriores del gráfico de Helm, el controlador de entrada NGINX o Kubernetes.

En este artículo también se requiere que ejecute la versión 2.0.64 de la CLI de Azure o una versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli-install].

Además, en este artículo se da por supuesto que tiene un clúster de AKS existente con un ACR integrado. Para más información sobre cómo crear un clúster de AKS con un ACR integrado, consulte [Autenticación con Azure Container Registry desde Azure Kubernetes Service][aks-integrated-acr].

## <a name="basic-configuration"></a>Configuración básica
Para crear un controlador de entrada NGINX simple sin personalizar los valores predeterminados, usará Helm.

```console
NAMESPACE=ingress-basic

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

helm install ingress-nginx ingress-nginx/ingress-nginx --create-namespace --namespace $NAMESPACE 
```

Observe que, por motivos de simplicidad, en la configuración anterior se usa la opción "lista para su uso".  Si es necesario, podría agregar parámetros para personalizar la implementación, por ejemplo, `--set controller.replicaCount=3`.  En la sección siguiente se mostrará un ejemplo del controlador de entrada con un alto nivel de personalización.

## <a name="customized-configuration"></a>Configuración personalizada
Como alternativa a la configuración básica presentada en la sección anterior, el siguiente conjunto de pasos mostrará cómo implementar un controlador de entrada personalizado.
### <a name="import-the-images-used-by-the-helm-chart-into-your-acr"></a>Importación de las imágenes usadas por el gráfico de Helm en el ACR

Para controlar las versiones de la imagen, deberá importarlas en su propia instancia de Azure Container Registry.  El [gráfico de Helm de controladores de entrada NGINX][ingress-nginx-helm-chart] se basa en tres imágenes de contenedor. Use `az acr import` para importar esas imágenes en el ACR.

```azurecli
REGISTRY_NAME=<REGISTRY_NAME>
SOURCE_REGISTRY=k8s.gcr.io
CONTROLLER_IMAGE=ingress-nginx/controller
CONTROLLER_TAG=v1.0.4
PATCH_IMAGE=ingress-nginx/kube-webhook-certgen
PATCH_TAG=v1.1.1
DEFAULTBACKEND_IMAGE=defaultbackend-amd64
DEFAULTBACKEND_TAG=1.5

az acr import --name $REGISTRY_NAME --source $SOURCE_REGISTRY/$CONTROLLER_IMAGE:$CONTROLLER_TAG --image $CONTROLLER_IMAGE:$CONTROLLER_TAG
az acr import --name $REGISTRY_NAME --source $SOURCE_REGISTRY/$PATCH_IMAGE:$PATCH_TAG --image $PATCH_IMAGE:$PATCH_TAG
az acr import --name $REGISTRY_NAME --source $SOURCE_REGISTRY/$DEFAULTBACKEND_IMAGE:$DEFAULTBACKEND_TAG --image $DEFAULTBACKEND_IMAGE:$DEFAULTBACKEND_TAG
```

> [!NOTE]
> Además de importar imágenes de contenedor en el ACR, también puede importar gráficos de Helm en el ACR. Para obtener más información, consulte [Inserción y extracción de gráficos de Helm en Azure Container Registry][acr-helm].

### <a name="create-an-ingress-controller"></a>Crear un controlador de entrada

Para crear el controlador de entrada, use Helm para instalar *nginx-ingress*. Para obtener redundancia adicional, se implementan dos réplicas de los controladores de entrada NGINX con el parámetro `--set controller.replicaCount`. Para sacar el máximo provecho de las réplicas en ejecución del controlador de entrada, asegúrese de que hay más de un nodo en el clúster de AKS.

El controlador de entrada también debe programarse en un nodo de Linux. Los nodos de Windows Server no deben ejecutar el controlador de entrada. Un selector de nodos se especifica mediante el parámetro `--set nodeSelector` para indicar al programador de Kubernetes que ejecute el controlador de entrada NGINX en un nodo basado en Linux.

> [!TIP]
> En el siguiente ejemplo se crea un espacio de nombres de Kubernetes para los recursos de entrada denominado *ingress-basic* y está diseñada para funcionar en el espacio de nombres. Especifique un espacio de nombres para su propio entorno según sea necesario.
>  
> Si quiere habilitar la [conservación de direcciones IP de origen del cliente][client-source-ip] para las solicitudes a los contenedores de su clúster, agregue `--set controller.service.externalTrafficPolicy=Local` al comando de instalación de Helm. La dirección IP de origen del cliente se almacena en el encabezado de la solicitud en *X-Forwarded-For*. Al usar un controlador de entrada con la conservación de direcciones IP de origen del cliente habilitada, el paso a través de SSL no funcionará.

```console
# Add the ingress-nginx repository
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

# Set variable for ACR location to use for pulling images
ACR_URL=<REGISTRY_URL>

# Use Helm to deploy an NGINX ingress controller
helm install nginx-ingress ingress-nginx/ingress-nginx \
    --namespace ingress-basic --create-namespace \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."kubernetes\.io/os"=linux \
    --set controller.image.registry=$ACR_URL \
    --set controller.image.image=$CONTROLLER_IMAGE \
    --set controller.image.tag=$CONTROLLER_TAG \
    --set controller.image.digest="" \
    --set controller.admissionWebhooks.patch.nodeSelector."kubernetes\.io/os"=linux \
    --set controller.admissionWebhooks.patch.image.registry=$ACR_URL \
    --set controller.admissionWebhooks.patch.image.image=$PATCH_IMAGE \
    --set controller.admissionWebhooks.patch.image.tag=$PATCH_TAG \
    --set controller.admissionWebhooks.patch.image.digest="" \
    --set defaultBackend.nodeSelector."kubernetes\.io/os"=linux \
    --set defaultBackend.image.registry=$ACR_URL \
    --set defaultBackend.image.image=$DEFAULTBACKEND_IMAGE \
    --set defaultBackend.image.tag=$DEFAULTBACKEND_TAG \
    --set defaultBackend.image.digest=""
```

## <a name="check-the-load-balancer-service"></a>Comprobación del servicio de equilibrador de carga

Cuando se crea el servicio del equilibrador de carga de Kubernetes para el controlador de entrada NGINX, se asigna la dirección IP pública dinámica, como se muestra en la salida del ejemplo siguiente:

```
$ kubectl --namespace ingress-basic get services -o wide -w nginx-ingress-ingress-nginx-controller

NAME                                     TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)                      AGE   SELECTOR
nginx-ingress-ingress-nginx-controller   LoadBalancer   10.0.74.133   EXTERNAL_IP     80:32486/TCP,443:30953/TCP   44s   app.kubernetes.io/component=controller,app.kubernetes.io/instance=nginx-ingress,app.kubernetes.io/name=ingress-nginx
```

No se han creado reglas de entrada aún, por lo que aparece la página 404 predeterminada del controlador de entrada NGINX si va a la dirección IP interna. Las reglas de entrada se configuran en los pasos siguientes.

## <a name="run-demo-applications"></a>Ejecución de aplicaciones de demostración

Para ver el controlador de entrada en acción, ejecute dos aplicaciones de demostración en el clúster de AKS. En este ejemplo, use `kubectl apply` para implementar dos instancias de una aplicación *Hola mundo* sencilla.

Cree un archivo *aks-helloworld-one.yaml* y cópielo en el ejemplo siguiente de YAML:

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aks-helloworld-one  
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aks-helloworld-one
  template:
    metadata:
      labels:
        app: aks-helloworld-one
    spec:
      containers:
      - name: aks-helloworld-one
        image: mcr.microsoft.com/azuredocs/aks-helloworld:v1
        ports:
        - containerPort: 80
        env:
        - name: TITLE
          value: "Welcome to Azure Kubernetes Service (AKS)"
---
apiVersion: v1
kind: Service
metadata:
  name: aks-helloworld-one  
spec:
  type: ClusterIP
  ports:
  - port: 80
  selector:
    app: aks-helloworld-one
```

Cree un archivo *aks-helloworld-two.yaml* y cópielo en el ejemplo siguiente de YAML:

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aks-helloworld-two  
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aks-helloworld-two
  template:
    metadata:
      labels:
        app: aks-helloworld-two
    spec:
      containers:
      - name: aks-helloworld-two
        image: mcr.microsoft.com/azuredocs/aks-helloworld:v1
        ports:
        - containerPort: 80
        env:
        - name: TITLE
          value: "AKS Ingress Demo"
---
apiVersion: v1
kind: Service
metadata:
  name: aks-helloworld-two  
spec:
  type: ClusterIP
  ports:
  - port: 80
  selector:
    app: aks-helloworld-two
```

Ejecute las dos aplicaciones de demostración mediante `kubectl apply`:

```console
kubectl apply -f aks-helloworld-one.yaml --namespace ingress-basic
kubectl apply -f aks-helloworld-two.yaml --namespace ingress-basic
```

## <a name="create-an-ingress-route"></a>Creación de una ruta de entrada

Ambas aplicaciones ahora se ejecutan en el clúster de Kubernetes. Para enrutar el tráfico a cada aplicación, cree un recurso de entrada de Kubernetes. El recurso de entrada configura las reglas de enrutamiento del tráfico a una de las dos aplicaciones.

En el ejemplo siguiente, el tráfico a la dirección *EXTERNAL_IP* se enruta al servicio denominado `aks-helloworld-one`. El tráfico a la dirección *EXTERNAL_IP/hello-world-two* se enruta al servicio `aks-helloworld-two`. El tráfico a la dirección *EXTERNAL_IP/static* se enruta al servicio denominado `aks-helloworld-one` para los recursos estáticos.

Cree un archivo denominado *hello-world-ingress.yaml* y cópielo en el ejemplo siguiente de YAML.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/use-regex: "true"
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  rules:
  - http:
      paths:
      - path: /hello-world-one(/|$)(.*)
        pathType: Prefix
        backend:
          service:
            name: aks-helloworld-one
            port:
              number: 80
      - path: /hello-world-two(/|$)(.*)
        pathType: Prefix
        backend:
          service:
            name: aks-helloworld-two
            port:
              number: 80
      - path: /(.*)
        pathType: Prefix
        backend:
          service:
            name: aks-helloworld-one
            port:
              number: 80
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-world-ingress-static
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/rewrite-target: /static/$2
spec:
  rules:
  - http:
      paths:
      - path:
        pathType: Prefix
        backend:
          service:
            name: aks-helloworld-one
            port: 
              number: 80
        path: /static(/|$)(.*)
```

Cree el recurso de entrada con el comando `kubectl apply -f hello-world-ingress.yaml`.

```
$ kubectl apply -f hello-world-ingress.yaml --namespace ingress-basic

ingress.extensions/hello-world-ingress created
ingress.extensions/hello-world-ingress-static created
```

## <a name="test-the-ingress-controller"></a>Prueba del controlador de entrada

Para probar las rutas para el controlador de entrada, vaya a las dos aplicaciones. Abra un explorador web en la dirección IP de su controlador de entrada NGINX, como *EXTERNAL_IP*. La primera aplicación de demostración aparece en el explorador web, como se muestra en el ejemplo siguiente:

![Primera aplicación ejecutándose detrás del controlador de entrada](media/ingress-basic/app-one.png)

A continuación, agregue la ruta de acceso */hello-world-two* a la dirección IP, como *EXTERNAL_IP/hello-world-two*. Se muestra la segunda aplicación de demostración con el título personalizado:

![Segunda aplicación ejecutándose detrás del controlador de entrada](media/ingress-basic/app-two.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

En este artículo, se usa Helm para instalar los componentes de entrada y las aplicaciones de ejemplo. Al implementar un gráfico de Helm, se crean algunos recursos de Kubernetes. Estos recursos incluyen pods, implementaciones y servicios. Para limpiar estos recursos, puede eliminar el espacio de nombres de ejemplo completo o los recursos individuales.

### <a name="delete-the-sample-namespace-and-all-resources"></a>Eliminación del espacio de nombres de ejemplo y de todos los recursos

Para eliminar el espacio de nombres de ejemplo completo, use el comando `kubectl delete` y especifique el nombre del espacio de nombres. Todos los recursos del espacio de nombres se eliminan.

```console
kubectl delete namespace ingress-basic
```

### <a name="delete-resources-individually"></a>Eliminación de recursos individualmente

Como alternativa, un enfoque más pormenorizado consiste en eliminar los recursos individuales creados. Despliegue una lista de las versiones de Helm con el comando `helm list`. Busque los gráficos denominados *nginx-ingress* y *aks-helloworld*, tal y como se muestra en la salida del ejemplo siguiente:

```
$ helm list --namespace ingress-basic

NAME                    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION
nginx-ingress           ingress-basic   1               2020-01-06 19:55:46.358275 -0600 CST    deployed        nginx-ingress-1.27.1    0.26.1  
```

Desinstale las versiones con el comando `helm uninstall`. En el ejemplo siguiente se desinstala la implementación de entrada de NGINX.

```
$ helm uninstall nginx-ingress --namespace ingress-basic

release "nginx-ingress" uninstalled
```

Luego, quitar las dos aplicaciones de ejemplo:

```console
kubectl delete -f aks-helloworld-one.yaml --namespace ingress-basic
kubectl delete -f aks-helloworld-two.yaml --namespace ingress-basic
```

Elimine la ruta de entrada que dirige el tráfico a las aplicaciones de ejemplo:

```console
kubectl delete -f hello-world-ingress.yaml
```

Por último, puede eliminar el propio espacio de nombres. Use el comando `kubectl delete` y especifique el nombre del espacio de nombres:

```console
kubectl delete namespace ingress-basic
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo se incluyen algunos componentes externos a AKS. Para más información sobre estos componentes, consulte las siguientes páginas del proyecto:

- [CLI de Helm][helm-cli]
- [Controlador de entrada NGINX][nginx-ingress]

También puede:

- [Habilitación del complemento de enrutamiento de aplicación HTTP][aks-http-app-routing]
- [Creación de un controlador de entrada que use una red privada interna y una dirección IP][aks-ingress-internal]
- [Crear un controlador de entrada que usa sus propios certificados TLS][aks-ingress-own-tls]
- Creación de un controlador de entrada que use Let's Encrypt para generar certificados TLS de forma automática [con una dirección IP pública dinámica][aks-ingress-tls] o [con una dirección IP pública estática][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm]: https://helm.sh/
[helm-cli]: ./kubernetes-helm.md
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[ingress-nginx-helm-chart]: https://github.com/kubernetes/ingress-nginx/tree/main/charts/ingress-nginx
[nginxinc/kubernetes-ingress]: https://github.com/nginxinc/kubernetes-ingress

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
[client-source-ip]: concepts-network.md#ingress-controllers
[aks-supported versions]: supported-kubernetes-versions.md
[aks-integrated-acr]: cluster-container-registry-integration.md?tabs=azure-cli#create-a-new-aks-cluster-with-acr-integration
[acr-helm]: ../container-registry/container-registry-helm-repos.md