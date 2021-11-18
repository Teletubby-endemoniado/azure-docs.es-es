---
title: Uso de los certificados TLS para la entrada
titleSuffix: Azure Kubernetes Service
description: Aprenda a instalar y configurar un controlador de entrada NGINX que use sus propios certificados en un clúster de Azure Kubernetes Service (AKS).
services: container-service
ms.topic: article
ms.date: 04/23/2021
ms.openlocfilehash: a52edc88ba1e6b5ca87d8679bbb844e4f8633eb6
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132553085"
---
# <a name="create-an-https-ingress-controller-and-use-your-own-tls-certificates-on-azure-kubernetes-service-aks"></a>Creación de un controlador de entrada HTTPS y uso de sus propios certificados TLS en Azure Kubernetes Service (AKS)

Un controlador de entrada es un software que proporciona el proxy inverso, el enrutamiento del tráfico configurable y la terminación de TLS para los servicios de Kubernetes. Los recursos de entrada de Kubernetes se usan para configurar las reglas de entrada y las rutas de los distintos servicios de Kubernetes. Mediante reglas de entrada y un controlador de entrada, se puede usar una sola dirección IP para enrutar el tráfico a varios servicios en un clúster de Kubernetes.

En este artículo se muestra cómo implementar el [controlador de entrada NGINX][nginx-ingress] en un clúster de Azure Kubernetes Service (AKS). Generará sus propios certificados y creará un secreto de Kubernetes para su uso con la ruta de entrada. Por último, en el clúster de AKS se ejecutan dos aplicaciones, a las que se puede acceder con una sola dirección IP.

También puede:

- [Creación de un controlador de entrada básico con conectividad de red externa][aks-ingress-basic]
- [Habilitación del complemento de enrutamiento de aplicación HTTP][aks-http-app-routing]
- [Creación de un controlador de entrada que use una red privada interna y una dirección IP][aks-ingress-internal]
- Creación de un controlador de entrada que use Let's Encrypt para generar certificados TLS de forma automática [con una dirección IP pública dinámica][aks-ingress-tls] o [con una dirección IP pública estática][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se usa [Helm 3][helm] para instalar el controlador de entrada NGINX en [una versión de Kubernetes compatible][versiones admitidas por AKS]. Asegúrese de usar la versión más reciente de Helm y de tener acceso al repositorio *ingress-nginx* de Helm. Es posible que los pasos descritos en este artículo no sean compatibles con versiones anteriores del gráfico de Helm, el controlador de entrada NGINX o Kubernetes.

Para obtener más información sobre cómo configurar y usar Helm, consulte [Instalación de aplicaciones con Helm en Azure Kubernetes Service (AKS)][use-helm].

En este artículo también se requiere que ejecute la versión 2.0.64 de la CLI de Azure o una versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli-install].

Además, en este artículo se da por supuesto que tiene un clúster de AKS existente con un ACR integrado. Para obtener más información sobre cómo crear un clúster de AKS con un ACR integrado, consulte [Autenticación con Azure Container Registry desde Azure Kubernetes Service][aks-integrated-acr].

## <a name="import-the-images-used-by-the-helm-chart-into-your-acr"></a>Importación de las imágenes usadas por el gráfico de Helm en el ACR

En este artículo se usa el [gráfico de Helm del controlador de entrada de NGINX][ingress-nginx-helm-chart], que se basa en tres imágenes de contenedor. Use `az acr import` para importar esas imágenes en el ACR.

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

## <a name="create-an-ingress-controller"></a>Crear un controlador de entrada

Para crear el controlador de entrada, use `Helm` para instalar *nginx-ingress*. Para obtener redundancia adicional, se implementan dos réplicas de los controladores de entrada NGINX con el parámetro `--set controller.replicaCount`. Para sacar el máximo provecho de las réplicas en ejecución del controlador de entrada, asegúrese de que hay más de un nodo en el clúster de AKS.

El controlador de entrada también debe programarse en un nodo de Linux. Los nodos de Windows Server no deben ejecutar el controlador de entrada. Un selector de nodos se especifica mediante el parámetro `--set nodeSelector` para indicar al programador de Kubernetes que ejecute el controlador de entrada NGINX en un nodo basado en Linux.

> [!TIP]
> En el siguiente ejemplo se crea un espacio de nombres de Kubernetes para los recursos de entrada denominado *ingress-basic* y está diseñada para funcionar en el espacio de nombres. Especifique un espacio de nombres para su propio entorno según sea necesario. Si su clúster de AKS no tiene RBAC de Kubernetes habilitado, agregue `--set rbac.create=false` a los comandos de Helm.

> [!TIP]
> Si quiere habilitar la [conservación de direcciones IP de origen del cliente][client-source-ip] para las solicitudes a los contenedores de su clúster, agregue `--set controller.service.externalTrafficPolicy=Local` al comando de instalación de Helm. La dirección IP de origen del cliente se almacena en el encabezado de la solicitud en *X-Forwarded-For*. Al usar un controlador de entrada con la conservación de direcciones IP de origen del cliente habilitada, el paso a través de TLS no funciona.

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

Durante la instalación se crea una dirección IP pública de Azure para el controlador de entrada. Esta dirección IP pública es estática durante el período de vida del controlador de entrada. Si elimina el controlador de entrada, se pierde la asignación de dirección IP pública. Si después crea un controlador de entrada adicional, se asigna una dirección IP pública nueva. Si quiere conservar el uso de la dirección IP pública, en su lugar puede [crear un controlador de entrada con una dirección IP pública estática][aks-ingress-static-tls].

Para obtener la dirección IP pública, use el comando `kubectl get service`.

```console
kubectl --namespace ingress-basic get services -o wide -w nginx-ingress-ingress-nginx-controller
```

La asignación de la dirección IP al servicio puede tardar hasta un minuto.

```
$ kubectl --namespace ingress-basic get services -o wide -w nginx-ingress-ingress-nginx-controller

NAME                                     TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)                      AGE   SELECTOR
nginx-ingress-ingress-nginx-controller   LoadBalancer   10.0.74.133   EXTERNAL_IP     80:32486/TCP,443:30953/TCP   44s   app.kubernetes.io/component=controller,app.kubernetes.io/instance=nginx-ingress,app.kubernetes.io/name=ingress-nginx
```

Anote esta dirección IP pública, ya que se utiliza en el último paso para probar la implementación.

Todavía no se creó ninguna regla de entrada. Si navega a la dirección IP pública, se muestra la página 404 predeterminada del controlador de entrada NGINX.

## <a name="generate-tls-certificates"></a>Generación de certificados TLS

En este artículo, vamos a generar un certificado autofirmado con `openssl`. Para su uso en producción, debe solicitar un certificado de confianza firmado mediante un proveedor o su propia entidad de certificación (CA). En el paso siguiente, generará un *secreto* de Kubernetes utilizando el certificado TLS y la clave privada que ha generado OpenSSL.

En el ejemplo siguiente, se genera un certificado X509 RSA de 2048 bits válido durante 365 días denominado *aks-ingress-tls.crt*. El archivo de clave privada se denomina *aks-ingress-tls.key*. Un secreto de Kubernetes TLS requiere ambos archivos.

Este artículo utiliza el nombre común del asunto *demo.azure.com*, que no es necesario cambiar. Para su uso en producción, especifique los valores de su propia organización para el parámetro `-subj`:

```console
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -out aks-ingress-tls.crt \
    -keyout aks-ingress-tls.key \
    -subj "/CN=demo.azure.com/O=aks-ingress-tls"
```

## <a name="create-kubernetes-secret-for-the-tls-certificate"></a>Creación de un secreto de Kubernetes para el certificado TLS

Para permitir que Kubernetes use el certificado TLS y la clave privada para el controlador de entrada, debe crear y usar un secreto. El secreto se define una vez y usa el certificado y el archivo de clave que creó en el paso anterior. A continuación, hará referencia a este secreto al definir las rutas de entrada.

En el ejemplo siguiente se crea un secreto llamado *aks-ingress-tls*:

```console
kubectl create secret tls aks-ingress-tls \
    --namespace ingress-basic \
    --key aks-ingress-tls.key \
    --cert aks-ingress-tls.crt
```

## <a name="run-demo-applications"></a>Ejecución de aplicaciones de demostración

Se han configurado un controlador de entrada y un secreto con su certificado. Ahora vamos a ejecutar dos aplicaciones de demostración en el clúster de AKS. En este ejemplo, Helm se usa para implementar dos instancias de una aplicación "Hola mundo" sencilla.

Para ver el controlador de entrada en acción, ejecute dos aplicaciones de demostración en el clúster de AKS. En este ejemplo, usar `kubectl apply` para implementar dos instancias de una aplicación *Hola mundo* sencilla.

Crear un archivo *aks-helloworld.yaml* y copiarlo en el ejemplo siguiente de YAML:

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aks-helloworld
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aks-helloworld
  template:
    metadata:
      labels:
        app: aks-helloworld
    spec:
      containers:
      - name: aks-helloworld
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
  name: aks-helloworld
spec:
  type: ClusterIP
  ports:
  - port: 80
  selector:
    app: aks-helloworld
```

Crear un archivo *ingress-demo.yaml* y copiarlo en el ejemplo siguiente de YAML:

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ingress-demo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ingress-demo
  template:
    metadata:
      labels:
        app: ingress-demo
    spec:
      containers:
      - name: ingress-demo
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
  name: ingress-demo
spec:
  type: ClusterIP
  ports:
  - port: 80
  selector:
    app: ingress-demo
```

Ejecutar las dos aplicaciones de demostración mediante `kubectl apply`:

```console
kubectl apply -f aks-helloworld.yaml --namespace ingress-basic
kubectl apply -f ingress-demo.yaml --namespace ingress-basic
```

## <a name="create-an-ingress-route"></a>Creación de una ruta de entrada

Ambas aplicaciones se ejecutan ahora en el clúster de Kubernetes, sin embargo, están configuradas con un servicio de tipo `ClusterIP`. Por lo tanto, no se puede acceder a ellas desde Internet. Para que estén disponibles de manera pública, cree un recurso de entrada de Kubernetes. El recurso de entrada configura las reglas de enrutamiento del tráfico a una de las dos aplicaciones.

En el ejemplo siguiente, el tráfico a la dirección `https://demo.azure.com/` se enruta al servicio denominado `aks-helloworld`. El tráfico a la dirección `https://demo.azure.com/hello-world-two` se enruta al servicio `ingress-demo`. En este artículo, no es necesario cambiar los nombres de host de demostración. Para su uso en producción, proporcione los nombres especificados como parte del proceso de solicitud y generación del certificado.

> [!TIP]
> Si el nombre de host especificado durante el proceso de solicitud de certificado (el nombre CN) no coincide con el host definido en la ruta de entrada, el controlador de entrada muestra la advertencia *Certificado falso de controlador de entrada de Kubernetes*. Asegúrese de que los nombres de host de la ruta de entrada y el certificado coinciden.

La sección *tls* indica la ruta de entrada para usar el secreto llamado *aks-ingress-tls* para el host *demo.azure.com*. De nuevo, para su uso en producción, especifique su propia dirección de host.

Cree un archivo denominado `hello-world-ingress.yaml` y cópielo en el ejemplo siguiente de YAML.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-world-ingress
  namespace: ingress-basic
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/use-regex: "true"
    nginx.ingress.kubernetes.io/rewrite-target: /$2
spec:
  tls:
  - hosts:
    - demo.azure.com
    secretName: aks-ingress-tls
  rules:
  - host: demo.azure.com
    http:
      paths:
      - path: /hello-world-one(/|$)(.*)
        pathType: Prefix
        backend:
          service:
            name: aks-helloworld
            port:
              number: 80
      - path: /hello-world-two(/|$)(.*)
        pathType: Prefix
        backend:
          service:
            name: ingress-demo
            port:
              number: 80
      - path: /(.*)
        pathType: Prefix
        backend:
          service:
            name: aks-helloworld
            port:
              number: 80
```

Cree el recurso de entrada con el comando `kubectl apply -f hello-world-ingress.yaml`.

```console
kubectl apply -f hello-world-ingress.yaml
```

La salida de ejemplo muestra que se crea el recurso de entrada.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-configuration"></a>Prueba de la configuración de entrada

Para probar los certificados con nuestro host *demo.azure.com* falso, utilice `curl` y especifique el parámetro *--resolver*. Este parámetro le permite asignar el nombre *demo.azure.com* a la dirección IP pública de su controlador de entrada. Especifique la dirección IP pública de su propio controlador de entrada, como se muestra en el ejemplo siguiente:

```
curl -v -k --resolve demo.azure.com:443:EXTERNAL_IP https://demo.azure.com
```

No proporcionó ninguna ruta de acceso adicional con la dirección, por lo que el controlador de entrada se establece de manera predeterminada en la ruta */* . Se devuelve la primera aplicación de demostración, tal como se muestra en la siguiente salida de ejemplo reducido:

```
$ curl -v -k --resolve demo.azure.com:443:EXTERNAL_IP https://demo.azure.com

[...]
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>Welcome to Azure Kubernetes Service (AKS)</title>
[...]
```

El parámetro *-v* de nuestro comando `curl` genera información detallada, incluyendo el certificado TLS recibido. A mitad de la salida de cURL, puede comprobar que se ha utilizado su propio certificado TLS. El parámetro *-k* continúa cargando la página, aunque se esté usando un certificado autofirmado. En el siguiente ejemplo se muestra el certificado *issuer: CN=demo.azure.com; O=aks-ingress-tls* que se ha utilizado:

```
[...]
* Server certificate:
*  subject: CN=demo.azure.com; O=aks-ingress-tls
*  start date: Oct 22 22:13:54 2018 GMT
*  expire date: Oct 22 22:13:54 2019 GMT
*  issuer: CN=demo.azure.com; O=aks-ingress-tls
*  SSL certificate verify result: self signed certificate (18), continuing anyway.
[...]
```

A continuación, agregue la ruta de acceso */hello-world-two* a la dirección, como `https://demo.azure.com/hello-world-two`. Se devuelve la segunda aplicación de demostración con el título personalizado, tal como se muestra en la siguiente salida de ejemplo reducido:

```
$ curl -v -k --resolve demo.azure.com:443:EXTERNAL_IP https://demo.azure.com/hello-world-two

[...]
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>AKS Ingress Demo</title>
[...]
```

## <a name="clean-up-resources"></a>Limpieza de recursos

En este artículo, se usa Helm para instalar los componentes de entrada y las aplicaciones de ejemplo. Al implementar un gráfico de Helm, se crean algunos recursos de Kubernetes. Estos recursos incluyen pods, implementaciones y servicios. Para limpiar estos recursos, puede eliminar el espacio de nombres de ejemplo completo o los recursos individuales.

### <a name="delete-the-sample-namespace-and-all-resources"></a>Eliminación del espacio de nombres de ejemplo y de todos los recursos

Para eliminar el espacio de nombres de ejemplo completo, use el comando `kubectl delete` y especifique el nombre del espacio de nombres. Todos los recursos del espacio de nombres se eliminan.

```console
kubectl delete namespace ingress-basic
```

### <a name="delete-resources-individually"></a>Eliminación de recursos individualmente

Como alternativa, un enfoque más pormenorizado consiste en eliminar los recursos individuales creados. Despliegue una lista de las versiones de Helm con el comando `helm list`. 

```console
helm list --namespace ingress-basic
```

Buscar el gráfico denominado *nginx-ingress* como se muestra en la salida del ejemplo siguiente:

```
$ helm list --namespace ingress-basic

NAME                    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION
nginx-ingress           ingress-basic   1               2020-01-06 19:55:46.358275 -0600 CST    deployed        nginx-ingress-1.27.1    0.26.1 
```

Desinstalar las versiones con el `helm uninstall` comando. 

```console
helm uninstall nginx-ingress --namespace ingress-basic
```

En el ejemplo siguiente se desinstala la implementación de entrada de NGINX.

```
$ helm uninstall nginx-ingress --namespace ingress-basic

release "nginx-ingress" uninstalled
```

Luego, quitar las dos aplicaciones de ejemplo:

```console
kubectl delete -f aks-helloworld.yaml --namespace ingress-basic
kubectl delete -f ingress-demo.yaml --namespace ingress-basic
```

Elimine la ruta de entrada que dirige el tráfico a las aplicaciones de ejemplo:

```console
kubectl delete -f hello-world-ingress.yaml
```

Elimine el secreto del certificado:

```console
kubectl delete secret aks-ingress-tls --namespace ingress-basic
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

- [Creación de un controlador de entrada básico con conectividad de red externa][aks-ingress-basic]
- [Habilitación del complemento de enrutamiento de aplicación HTTP][aks-http-app-routing]
- [Creación de un controlador de entrada que use una red privada interna y una dirección IP][aks-ingress-internal]
- Creación de un controlador de entrada que use Let's Encrypt para generar certificados TLS de forma automática [con una dirección IP pública dinámica][aks-ingress-tls] o [con una dirección IP pública estática][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: ./kubernetes-helm.md
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[helm]: https://helm.sh/
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm
[ingress-nginx-helm-chart]: https://github.com/kubernetes/ingress-nginx/tree/main/charts/ingress-nginx

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az_aks_show
[az-network-public-ip-create]: /cli/azure/network/public-ip#az_network_public_ip_create
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-tls]: ingress-tls.md
[client-source-ip]: concepts-network.md#ingress-controllers
[aks-integrated-acr]: cluster-container-registry-integration.md?tabs=azure-cli#create-a-new-aks-cluster-with-acr-integration
[acr-helm]: ../container-registry/container-registry-helm-repos.md