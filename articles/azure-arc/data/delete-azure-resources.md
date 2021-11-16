---
title: Eliminación de recursos de Azure
description: Eliminación de recursos de Azure
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 11/03/2021
ms.topic: how-to
ms.openlocfilehash: ad92c16b70fd2d9f2e137558db1e70c387815a8f
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131564145"
---
# <a name="delete-resources-from-azure"></a>Eliminación de recursos de Azure

En este artículo se describe cómo eliminar de Azure recursos del servicio de datos habilitado para Azure Arc.

> [!WARNING]
> Al eliminar recursos como se describe en este artículo, estas acciones son irreversibles.

## <a name="before"></a>Antes

Antes de eliminar un recurso como una instancia administrada de SQL de Azure Arc o un controlador de datos de Azure Arc, debe exportar y cargar la información de uso en Azure para realizar un cálculo de facturación preciso siguiendo las instrucciones descritas en [Carga de datos de facturación en Azure: modo conectado indirectamente](view-billing-data-in-azure.md#upload-billing-data-to-azure---indirectly-connected-mode).

## <a name="direct-connectivity-mode"></a>Modo de conectividad directa

Cuando un clúster esté conectado a Azure con el modo de conectividad directa, use Azure Portal para administrar los recursos. Use el portal para todas las operaciones de creación, lectura, actualización y eliminación (CRUD) para el controlador de datos, Instancia administrada y grupos de PostgreSQL.

Desde Azure Portal:
1. Vaya al grupo de recursos y elimine el controlador de datos de Azure Arc.
2. Seleccione el clúster de Kubernetes habilitado para Azure Arc y vaya a la página de información general.
    - En Configuración, seleccione **Extensiones**.
    - En la página Extensiones, seleccione la extensión de servicios de datos de Azure Arc (de tipo microsoft.arcdataservices) y haga clic en **Desinstalar**.
3. De manera opcional, elimine la ubicación personalizada en la que se ha implementado el controlador de datos de Azure Arc.
4. Como alternativa, también puede eliminar el espacio de nombres en el clúster de Kubernetes si no se ha creado ningún otro recurso en el espacio de nombres.



Consulte [Administración de recursos de Azure con Azure Portal](../../azure-resource-manager/management/manage-resources-portal.md).

## <a name="indirect-connectivity-mode"></a>Modo de conectividad indirecta

En el modo de conexión indirecto, la eliminación de una instancia de Kubernetes no la elimina de Azure y la eliminación de una instancia de Azure no la elimina de Kubernetes. En el modo de conexión indirecto, la eliminación de un recurso es un proceso de dos pasos y se mejorará en el futuro. Kubernetes será el origen de verdad y el portal se actualizará para reflejarlo.

En algunos casos, puede que tenga que eliminar manualmente en Azure los recursos de los servicios de datos habilitados para Azure Arc.  Puede eliminar estos recursos con cualquiera de las siguientes opciones.

- [Eliminación de recursos de Azure](#delete-resources-from-azure)
  - [Eliminación de un grupo de recursos completo](#delete-an-entire-resource-group)
  - [Eliminación de recursos específicos del grupo de recursos](#delete-specific-resources-in-the-resource-group)
  - [Eliminación de recursos mediante la CLI de Azure](#delete-resources-using-the-azure-cli)
    - [Eliminación de recursos de SQL Managed Instance mediante la CLI de Azure](#delete-sql-managed-instance-resources-using-the-azure-cli)
    - [Eliminación de los recursos del grupo de servidores de Hiperescala de PostgreSQL mediante la CLI de Azure](#delete-postgresql-hyperscale-server-group-resources-using-the-azure-cli)
    - [Eliminación de los recursos del controlador de datos de Azure Arc mediante la CLI de Azure](#delete-azure-arc-data-controller-resources-using-the-azure-cli)
    - [Eliminación de un grupo de recursos mediante la CLI de Azure](#delete-a-resource-group-using-the-azure-cli)


## <a name="delete-an-entire-resource-group"></a>Eliminación de un grupo de recursos completo

Si ha utilizado un grupo de recursos específico y dedicado para los servicios de datos habilitados para Azure Arc y quiere eliminar *todo* lo que hay dentro del grupo de recursos, puede eliminar el grupo de recursos, lo que eliminará todo lo que contiene.  

Para eliminar un grupo de recursos en Azure Portal, realice los pasos siguientes:

- En Azure Portal, vaya al grupo de recursos en el que se han creado los recursos de los servicios de datos habilitados para Azure Arc.
- Haga clic en el botón **Eliminar grupo de recursos**.
- Para confirmar la eliminación, escriba el nombre del grupo de recursos y haga clic en **Eliminar**.

## <a name="delete-specific-resources-in-the-resource-group"></a>Eliminación de recursos específicos del grupo de recursos

Para eliminar recursos específicos de los servicios de datos habilitados para Azure Arc de un grupo de recursos en Azure Portal, siga los pasos a continuación:

- En Azure Portal, vaya al grupo de recursos en el que se han creado los recursos de los servicios de datos habilitados para Azure Arc.
- Seleccione todos los recursos que se van a eliminar.
- Haga clic en el botón Eliminar.
- Escriba "Sí" para confirmar la eliminación y haga clic en **Eliminar**.

## <a name="delete-resources-using-the-azure-cli"></a>Eliminación de recursos mediante la CLI de Azure

Puede eliminar recursos específicos de los servicios de datos habilitados para Azure Arc mediante la CLI de Azure.

### <a name="delete-sql-managed-instance-resources-using-the-azure-cli"></a>Eliminación de recursos de SQL Managed Instance mediante la CLI de Azure

Para eliminar de Azure los recursos de SQL Managed Instance mediante la CLI de Azure, reemplace los valores de marcador de posición en el comando siguiente y ejecútelo.

```azurecli
az resource delete --name <sql instance name> --resource-type Microsoft.AzureArcData/sqlManagedInstances --resource-group <resource group name>

#Example
#az resource delete --name sql1 --resource-type Microsoft.AzureArcData/sqlManagedInstances --resource-group rg1
```

### <a name="delete-postgresql-hyperscale-server-group-resources-using-the-azure-cli"></a>Eliminación de los recursos del grupo de servidores de Hiperescala de PostgreSQL mediante la CLI de Azure

Para eliminar de Azure un recurso del grupo de servidores de Hiperescala de PostgreSQL mediante la CLI de Azure, reemplace los valores de marcador de posición en el comando siguiente y ejecútelo.

```azurecli
az resource delete --name <postgresql instance name> --resource-type Microsoft.AzureArcData/postgresInstances --resource-group <resource group name>

#Example
#az resource delete --name pg1 --resource-type Microsoft.AzureArcData/postgresInstances --resource-group rg1
```

### <a name="delete-azure-arc-data-controller-resources-using-the-azure-cli"></a>Eliminación de los recursos del controlador de datos de Azure Arc mediante la CLI de Azure

> [!NOTE]
> Antes de eliminar un controlador de datos de Azure Arc, debe eliminar todos los recursos de instancia de base de datos que administra.

Para eliminar de Azure un controlador de datos de Azure Arc mediante la CLI de Azure, reemplace los valores de marcador de posición en el comando siguiente y ejecútelo.

```azurecli
az resource delete --name <data controller name> --resource-type Microsoft.AzureArcData/dataControllers --resource-group <resource group name>

#Example
#az resource delete --name dc1 --resource-type Microsoft.AzureArcData/dataControllers --resource-group rg1
```

### <a name="delete-a-resource-group-using-the-azure-cli"></a>Eliminación de un grupo de recursos mediante la CLI de Azure

También puede usar la CLI de Azure para [eliminar un grupo de recursos](../../azure-resource-manager/management/delete-resource-group.md).
