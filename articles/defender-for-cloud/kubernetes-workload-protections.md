---
title: Protecciones de cargas de trabajo para las cargas de trabajo de Kubernetes
description: Obtener información sobre cómo usar el conjunto de recomendaciones de seguridad de protección de cargas de trabajo de Kubernetes de Microsoft Defender for Cloud
services: security-center
author: memildin
manager: rkarlin
ms.service: defender-for-cloud
ms.topic: how-to
ms.date: 11/09/2021
ms.author: memildin
ms.openlocfilehash: 5f8bf1f34ba33a4a6c3d258e8cb813d062f1f98f
ms.sourcegitcommit: 2ed2d9d6227cf5e7ba9ecf52bf518dff63457a59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132525780"
---
# <a name="protect-your-kubernetes-workloads"></a>Protección de las cargas de trabajo de Kubernetes

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

En esta página se describe cómo usar el conjunto de recomendaciones de seguridad de Microsoft Defender for Cloud dedicadas a la protección de cargas de trabajo de Kubernetes.

Obtenga más información sobre estas características en [Procedimientos recomendados de protección de cargas de trabajo con el control de admisión de Kubernetes](container-security.md#workload-protection-best-practices-using-kubernetes-admission-control)

Defender for Cloud ofrece más características de seguridad de contenedores si habilita Microsoft Defender para Kubernetes. Específicamente:

- Analice sus registros de contenedor para detectar vulnerabilidades con [Microsoft Defender para registros de contenedor](defender-for-container-registries-introduction.md)
- Obtenga alertas de detección de amenazas para los clústeres K8s en [Microsoft Defender para Kubernetes](defender-for-kubernetes-introduction.md)

> [!TIP]
> Para obtener una lista de *todas* las recomendaciones de seguridad que pueden aparecer para los clústeres y nodos de Kubernetes, consulte la [sección de proceso](recommendations-reference.md#recs-compute) de la tabla de referencia de recomendaciones.



## <a name="availability"></a>Disponibilidad

| Aspecto                          | Detalles                                                                                                                                      |
|---------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------|
| Estado de la versión:                  | Disponibilidad general (GA)                                                                                                                    |
| Precios:                        | Gratuito                                                                                                                                         |
| Roles y permisos necesarios: | **Propietario** o **administrador de seguridad** para editar una asignación<br>**Lector** para ver las recomendaciones                                              |
| Requisitos del entorno:       | Se requiere la versión 1.14 (o posterior) de Kubernetes<br>Ningún recurso PodSecurityPolicy (antiguo modelo de PSP) en los clústeres<br>No se admiten nodos de Windows |
| Nubes:                         | :::image type="icon" source="./media/icons/yes-icon.png"::: Nubes comerciales<br>:::image type="icon" source="./media/icons/yes-icon.png"::: Nacionales (Azure Government, Azure China 21Vianet) |
|                                 |                                                                                                                                              |


## <a name="set-up-your-workload-protection"></a>Configuración de la protección de cargas de trabajo

Microsoft Defender for Cloud incluye un conjunto de recomendaciones que están disponibles cuando ha instalado el **complemento de Azure Policy para Kubernetes**.

### <a name="step-1-deploy-the-add-on"></a>Paso 1: Implementación del complemento

Para configurar las recomendaciones, instale el **complemento de Azure Policy para Kubernetes**. 

- También puede implementar automáticamente este complemento tal y como se explica en [Habilitación del aprovisionamiento automático de agentes y extensiones de Log Analytics](enable-data-collection.md#auto-provision-mma). Cuando el aprovisionamiento automático del complemento esté establecido en "activado", la extensión se habilitará de forma predeterminada en todos los clústeres existentes y futuros (que cumplan los requisitos de instalación del complemento).

    :::image type="content" source="media/defender-for-kubernetes-usage/policy-add-on-auto-provision.png" alt-text="Uso de la herramienta de aprovisionamiento automático de Defender for Cloud para instalar el complemento de directiva para Kubernetes":::

- Para implementar manualmente el complemento:

    1. En la página de recomendaciones, busque la recomendación "**Azure Policy add-on for Kubernetes should be installed and enabled on your clusters**" (El complemento Azure Policy para Kubernetes debería estar instalado y habilitado en sus clústeres). 

        :::image type="content" source="./media/defender-for-kubernetes-usage/recommendation-to-install-policy-add-on-for-kubernetes.png" alt-text="Recomendación **El complemento Azure Policy para Kubernetes debería estar instalado y habilitado en sus clústeres**":::

        > [!TIP]
        > La recomendación se incluye en cinco controles de seguridad diferentes y no importa cuál seleccione en el siguiente paso.

    1. En cualquiera de los controles de seguridad, seleccione la recomendación para ver los recursos en los que puede instalar el complemento.
    1. Seleccione el clúster correspondiente y elija **Corregir**.

        :::image type="content" source="./media/defender-for-kubernetes-usage/recommendation-to-install-policy-add-on-for-kubernetes-details.png" alt-text="Página de detalles de recomendación para **El complemento Azure Policy para Kubernetes debería estar instalado y habilitado en sus clústeres**":::

### <a name="step-2-view-and-configure-the-bundle-of-recommendations"></a>Paso 2: Vista y configuración del conjunto de recomendaciones

1. Aproximadamente 30 minutos después de completarse la instalación del complemento, Defender for Cloud muestra el estado de mantenimiento de los clústeres de las siguientes recomendaciones, cada uno en el control de seguridad pertinente como se muestra a continuación:

    > [!NOTE]
    > Si va a instalar el complemento por primera vez, estas recomendaciones aparecerán como nuevas adiciones en la lista de recomendaciones. 

    > [!TIP]
    > Algunas recomendaciones tienen parámetros que deben personalizarse a través de Azure Policy para usarlos de forma eficaz. Por ejemplo, para beneficiarse de la recomendación **Las imágenes de contenedor solo deben implementarse desde registros de confianza**, tendrá que definir sus registros de confianza.
    > 
    > Si no especifica los parámetros necesarios para las recomendaciones que requieren configuración, las cargas de trabajo aparecerán como incorrectas.

    | Nombre de la recomendación                                                         | Control de seguridad                         | Configuración requerida |
    |-----------------------------------------------------------------------------|------------------------------------------|------------------------|
    | Los contenedores solo deben escuchar en los puertos permitidos.                              | Restricción de los accesos de red no autorizados     | **Sí**                |
    | Los servicios solo deben escuchar en los puertos permitidos.                                | Restricción de los accesos de red no autorizados     | **Sí**                |
    | El uso de puertos y redes de hosts debe estar restringido.                     | Restricción de los accesos de red no autorizados     | **Sí**                |
    | La opción de reemplazar o deshabilitar el perfil de AppArmor de los contenedores debe estar restringida. | Corrección de configuraciones de seguridad        | **Sí**                |
    | Las imágenes de contenedor solo deben implementarse desde registros de confianza.            | Corrección de vulnerabilidades                | **Sí**                |
    | Deben aplicarse funcionalidades de Linux con privilegios mínimos para los contenedores.       | Administración de acceso y permisos            | **Sí**                |
    | El uso de montajes de volúmenes HostPath de pod debe estar restringido a una lista conocida.    | Administración de acceso y permisos            | **Sí**                |
    | Deben evitarse los contenedores con privilegios.                                     | Administración de acceso y permisos            | No                     |
    | Debe evitar los contenedores con elevación de privilegios.                       | Administración de acceso y permisos            | No                     |
    | Los clústeres de Kubernetes deben deshabilitar las credenciales de la API de montaje automático             | Administración de acceso y permisos            | No                     |
    | El sistema de archivos raíz inmutable (de solo lectura) debe aplicarse para los contenedores.     | Administración de acceso y permisos            | No                     |
    | Debe evitar los contenedores con elevación de privilegios.                       | Administración de acceso y permisos            | No                     |
    | Debe evitar la ejecución de contenedores como usuario raíz.                           | Administración de acceso y permisos            | No                     |
    | Deben evitarse los contenedores que comparten espacios de nombres de host confidenciales.              | Administración de acceso y permisos            | No                     |
    | Debe aplicar los límites de CPU y memoria de los contenedores.                          | Protección de aplicaciones contra ataques DDoS | No                     |
    | Los clústeres de Kubernetes solo deben ser accesibles mediante HTTPS                    | Cifrado de los datos en tránsito                  | No                     |
    | Los clústeres de Kubernetes no deben usar el espacio de nombres predeterminado                    | Implementación de procedimientos recomendados de seguridad        | No                     |
    ||||


1. Para las recomendaciones con parámetros que deben personalizarse, establezca los parámetros:

    1. En el menú de Defender for Cloud, seleccione **Directiva de seguridad**.
    1. Seleccione la suscripción correspondiente.
    1. En la sección **Directiva predeterminada de Defender for Cloud**, seleccione **Ver directiva efectiva**.
    1. Seleccione la directiva predeterminada para el ámbito que va a actualizar.
    1. Abra la pestaña **Parámetros** y modifique los valores según sea necesario.

        :::image type="content" source="media/kubernetes-workload-protections/containers-parameter-requires-configuration.png" alt-text="Modificar los parámetros de una de las recomendaciones del conjunto de protección de cargas de trabajo de Kubernetes":::.

    1. Seleccione **Revisar y guardar**.
    1. Seleccione **Guardar**.


1. Para aplicar cualquiera de las recomendaciones, 

    1. abra la página de detalles de recomendaciones y haga clic en **Denegar**:

        :::image type="content" source="./media/defender-for-kubernetes-usage/enforce-workload-protection-example.png" alt-text="Opción Denegar del parámetro de Azure Policy":::

        Se abrirá el panel donde se establece el ámbito. 

    1. Cuando lo haya establecido, seleccione **Change to deny** (Cambiar a denegar).

1. Para ver qué recomendaciones se aplican a los clústeres:

    1. Abra la página [Inventario de recursos](asset-inventory.md) de Defender for Cloud y use el filtro de tipo de recurso en **Servicios de Kubernetes**.

    1. Seleccione un clúster para investigar y revise las recomendaciones disponibles para este. 

1. Al ver una recomendación de la protección de cargas de trabajo establecida, verá el número de pods afectados ("componentes de Kubernetes") mostrados junto con el clúster. Para ver una lista de los pods específicos, seleccione el clúster y, a continuación, seleccione **Realizar acción**.

    :::image type="content" source="./media/defender-for-kubernetes-usage/view-affected-pods-for-recommendation.gif" alt-text="Visualización de los pods afectados para una recomendación de K8s"::: 

1. Para probar la aplicación, use las dos implementaciones de Kubernetes siguientes:

    - Una es para una implementación correcta, compatible con el conjunto de recomendaciones de protección de cargas de trabajo.
    - La otra es para una implementación incorrecta, no compatible con *cualquiera* de las recomendaciones.

    Implemente los archivos .yaml de ejemplo tal cual o úselos como referencia para corregir su propia carga de trabajo (paso VIII)  


## <a name="healthy-deployment-example-yaml-file"></a>Archivo .yaml de ejemplo de implementación correcta

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-healthy-deployment
  labels:
    app: redis
spec:
  replicas: 3
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
      annotations:
        container.apparmor.security.beta.kubernetes.io/redis: runtime/default
    spec:
      containers:
      - name: redis
        image: <customer-registry>.azurecr.io/redis:latest
        ports:
        - containerPort: 80
        resources:
          limits:
            cpu: 100m
            memory: 250Mi
        securityContext:
          privileged: false
          readOnlyRootFilesystem: true
          allowPrivilegeEscalation: false
          runAsNonRoot: true
          runAsUser: 1000
---
apiVersion: v1
kind: Service
metadata:
  name: redis-healthy-service
spec:
  type: LoadBalancer
  selector:
    app: redis
  ports:
  - port: 80
    targetPort: 80
```

## <a name="unhealthy-deployment-example-yaml-file"></a>Archivo .yaml de ejemplo de implementación incorrecta

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-unhealthy-deployment
  labels:
    app: redis
spec:
  replicas: 3
  selector:
    matchLabels:
      app: redis
  template:
    metadata:      
      labels:
        app: redis
    spec:
      hostNetwork: true
      hostPID: true 
      hostIPC: true
      containers:
      - name: redis
        image: redis:latest
        ports:
        - containerPort: 9001
          hostPort: 9001
        securityContext:
          privileged: true
          readOnlyRootFilesystem: false
          allowPrivilegeEscalation: true
          runAsUser: 0
          capabilities:
            add:
              - NET_ADMIN
        volumeMounts:
        - mountPath: /test-pd
          name: test-volume
          readOnly: true
      volumes:
      - name: test-volume
        hostPath:
          # directory location on host
          path: /tmp
---
apiVersion: v1
kind: Service
metadata:
  name: redis-unhealthy-service
spec:
  type: LoadBalancer
  selector:
    app: redis
  ports:
  - port: 6001
    targetPort: 9001
```



## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a configurar la protección de cargas de trabajo de Kubernetes. 

Para obtener material relacionado, consulte las páginas siguientes: 

- [Recomendaciones de Defender for Cloud para procesos](recommendations-reference.md#recs-compute)
- [Alertas del nivel de clúster de AKS](alerts-reference.md#alerts-k8scluster)
- [Alertas del nivel de host de contenedor](alerts-reference.md#alerts-containerhost)
