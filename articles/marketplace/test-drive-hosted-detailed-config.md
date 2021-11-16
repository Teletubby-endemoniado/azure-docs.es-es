---
title: Configuración detallada de una versión de prueba hospedada en Azure Marketplace
description: Explicación de los pasos de configuración de una versión de prueba hospedada en el marketplace comercial
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: trkeya
ms.author: trkeya
ms.date: 10/26/2021
ms.openlocfilehash: 5fde0e1d9dc78c735ae9e889af5fbd889a588540
ms.sourcegitcommit: 8946cfadd89ce8830ebfe358145fd37c0dc4d10e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2021
ms.locfileid: "131851028"
---
# <a name="detailed-configuration-for-hosted-test-drives"></a>Configuración detallada de las versiones de prueba hospedadas

En este artículo se describe cómo configurar una versión de prueba hospedada para Dynamics 365 for Customer Engagement y Power Apps o Dynamics 365 for Operations.

> [!TIP]
> Para ver la vista del cliente de la versión de prueba en el marketplace comercial, consulte [¿Qué es Azure Marketplace?](/marketplace/azure-marketplace-overview#take-action-on-a-listing) y [¿Qué es Microsoft AppSource?](/marketplace/appsource-overview).

## <a name="configure-for-dynamics-365-customer-engagement--power-apps"></a>Configuración para Dynamics 365 Customer Engagement y Power Apps

[!INCLUDE [Workspaces view note](./includes/preview-interface.md)]

#### <a name="workspaces-view"></a>[Vista de áreas de trabajo](#tab/workspaces-view)

1. Inicie sesión en el [Centro de partners](https://partner.microsoft.com/dashboard/commercial-marketplace/overview).
2. Si no puede acceder al vínculo anterior, debe enviar una solicitud [aquí](https://appsource.microsoft.com/partners/list-an-app) para publicar la aplicación. Una vez que revisemos la solicitud, se le concederá acceso para iniciar el proceso de publicación.
3. Busque una oferta de **Dynamics 365 for Customer Engagement y Power Apps** o cree una nueva oferta de **Dynamics 365 for Customer Engagement y Power Apps**.
4. En la página **Configuración de la oferta**, active la casilla **Habilitar una versión de prueba**, seleccione un **tipo de versión de prueba** (consulte la viñeta siguiente) y seleccione **Guardar borrador**.

    [![Muestra la activación de la casilla "Habilitar una versión de prueba".](./media/test-drive/enable-test-drive-check-box-workspaces.png)](./media/test-drive/enable-test-drive-check-box-workspaces.png#lightbox)

    - **Tipo de versión de prueba**: elija la opción **Microsoft Hosted (Dynamics 365 for Customer Engagement & PowerApps)** [Hospedada en Microsoft (Dynamics 365 for Customer Engagement y PowerApps)]. Esto indica que Microsoft hospedará y mantendrá el servicio que realiza el aprovisionamiento y desaprovisionamiento de usuarios de la versión de prueba.

5. Conceda permiso a Microsoft AppSource para aprovisionar y desaprovisionar los usuarios de la versión de prueba del inquilino mediante [estas instrucciones](./test-drive-azure-subscription-setup.md). En este paso, generará los valores **Azure AD App ID** (Identificador de aplicación de Azure AD) y **Azure AD App Key** (Clave de aplicación de Azure AD) que se mencionan a continuación.
6. Complete estos campos en la página **Configuración técnica** > **de la versión de prueba**.

    [![Muestra la página de configuración técnica de la versión de prueba.](media/test-drive/technical-config-details-workspaces.png)](media/test-drive/technical-config-details-workspaces.png#lightbox)

    - **Máximo de versiones de prueba simultáneas**: número de usuarios simultáneos que pueden tener una versión de prueba activa en ejecución al mismo tiempo. Cada usuario consumirá una licencia de Dynamics mientras su versión de prueba esté activa, por lo que debe asegurarse de tener número de licencias de Dynamics disponibles para los usuarios de la versión de prueba. La cantidad recomendada es de 3 a 5.
    - **Duración de la versión de prueba**: número de horas que estará activa la versión de prueba del usuario. Una vez transcurrido este tiempo, el usuario se desaprovisionará del inquilino. Se recomiendan entre 2 y 24 horas dependiendo de la complejidad de la aplicación. El usuario siempre puede solicitar otra versión de prueba si se agota el tiempo y quiere volver a acceder a la versión de prueba.
    - **URL de la instancia**: URL a la que se dirigirá al usuario de la versión de prueba cuando la inicie. Esta suele ser la dirección URL de la instancia de Dynamics 365 en la que se han instalado la aplicación y los datos de ejemplo. Valor de ejemplo: `https://testdrive.crm.dynamics.com`.
    - **URL de la instancia de la API web**: dirección URL de la API web para la instancia de Dynamics 365. Para recuperar este valor, inicie sesión en la instancia de Microsoft Dynamics 365 y vaya a **Configuración** > **Personalización** > **Recursos de desarrollador** > **API web de la instancia** y copie la dirección (URL). Valor de ejemplo:

        :::image type="content" source="./media/test-drive/sample-web-api-url.png" alt-text="Ejemplo de API web de instancia.":::

    - **Nombre del rol**: nombre del rol de Dynamics 365 Security personalizado que creó para la versión de prueba. También puede usar un rol existente. Un nuevo rol debe tener los privilegios mínimos necesarios agregados para iniciar sesión en una instancia de Customer Engagement. Consulte [Privilegios mínimos necesarios para iniciar sesión en Microsoft Dynamics 365](https://community.dynamics.com/crm/b/crminogic/archive/2016/11/24/minimum-privileges-required-to-login-microsoft-dynamics-365). Este es el rol que se asignará a los usuarios durante su versión de prueba. Valor de ejemplo: `testdriverole`.
    
        > [!IMPORTANT]
        > Asegúrese de que la comprobación del grupo de seguridad no está agregada. Esto permite que el usuario se sincronice con la instancia de Customer Engagement.

    - **Id. de inquilino de Azure Active Directory**: identificador del inquilino de Azure para su instancia de Dynamics 365. Para recuperar este valor, inicie sesión en Azure Portal y vaya a **Azure Active Directory** > **Propiedades** y copie el id. de directorio. Valor de ejemplo: 172f988bf-86f1-41af-91ab-2d7cd01112341.
    - **Nombre de inquilino de Azure Active Directory**: nombre del inquilino de Azure para su instancia de Dynamics 365. Use el formato `<tenantname>.onmicrosoft.com`. Valor de ejemplo: `testdrive.onmicrosoft.com`.
    - **Id. de aplicación de Azure Active Directory**: identificador de la aplicación de Azure Active Directory (AD) que creó en el paso 5. Valor de ejemplo: `53852862-a2ae-4e43-9461-faa49650a096`.
    - **Secreto de cliente de la aplicación de Azure Active Directory**: secreto de la aplicación de Azure AD que creó en el paso 5. Valor de ejemplo: `IJUgaIOfq9b9LbUjeQmzNBW4VGn6grr1l/n3aMrnfdk=`.

7. Complete los campos de la página **Listas de Marketplace de la versión de prueba**.

    [ ![Muestra la página de detalles del listado de Marketplace.](./media/test-drive/marketplace-listing-details-workspaces.png) ](./media/test-drive/marketplace-listing-details-workspaces.png#lightbox)

    - **Descripción**: introducción de la versión de prueba. Este texto se mostrará al usuario durante el aprovisionamiento de la versión de prueba. Este campo es compatible con HTML, si desea proporcionar contenido con formato (requerido).
    - **Manual del usuario**: manual del usuario en PDF que ayuda a los usuarios de la versión de prueba a aprender a usar la aplicación (requerido).
    - **Vídeo de demostración de la versión de prueba**: vídeo que presenta la aplicación (opcional).

#### <a name="current-view"></a>[Vista actual](#tab/current-view)

1. Inicie sesión en el [Centro de partners](https://partner.microsoft.com/dashboard/commercial-marketplace/overview).
2. Si no puede acceder al vínculo anterior, debe enviar una solicitud [aquí](https://appsource.microsoft.com/partners/list-an-app) para publicar la aplicación. Una vez que revisemos la solicitud, se le concederá acceso para iniciar el proceso de publicación.
3. Busque una oferta de **Dynamics 365 for Customer Engagement y Power Apps** o cree una nueva oferta de **Dynamics 365 for Customer Engagement y Power Apps**.
4. Active la casilla **Habilitar una versión de prueba** y seleccione un **Tipo de versión de prueba** (consulte la siguiente ilustración) y, a continuación, seleccione **Guardar**.

    [![Activación de la casilla "Habilitar una versión de prueba".](media/test-drive/enable-test-drive-check-box.png)](media/test-drive/enable-test-drive-check-box.png#lightbox)

    - **Tipo de versión de prueba**: elija la opción **Microsoft Hosted (Dynamics 365 for Customer Engagement & PowerApps)** (Hospedada en Microsoft [Dynamics 365 for Customer Engagement y PowerApps]). Esto indica que Microsoft hospedará y mantendrá el servicio que realiza el aprovisionamiento y desaprovisionamiento de usuarios de la versión de prueba.

5. Conceda permiso a Microsoft AppSource para aprovisionar y desaprovisionar los usuarios de la versión de prueba del inquilino mediante [estas instrucciones](./test-drive-azure-subscription-setup.md). En este paso, generará los valores **Azure AD App ID** (Identificador de aplicación de Azure AD) y **Azure AD App Key** (Clave de aplicación de Azure AD) que se mencionan a continuación.
6. Rellene estos campos en la página **Test drive technical configuration** (Configuración técnica de la versión de prueba).

    [![Página de configuración técnica de la versión de prueba.](media/test-drive/technical-config-details.png)](media/test-drive/technical-config-details.png#lightbox)

    - **Máximo de versiones de prueba simultáneas**: número de usuarios simultáneos que pueden tener una versión de prueba activa en ejecución al mismo tiempo. Cada usuario consumirá una licencia de Dynamics mientras su versión de prueba esté activa, por lo que debe asegurarse de tener número de licencias de Dynamics disponibles para los usuarios de la versión de prueba. La cantidad recomendada es de 3 a 5.
    - **Duración de la versión de prueba**: número de horas que estará activa la versión de prueba del usuario. Una vez transcurrido este tiempo, el usuario se desaprovisionará del inquilino. Se recomiendan entre 2 y 24 horas dependiendo de la complejidad de la aplicación. El usuario siempre puede solicitar otra versión de prueba si se agota el tiempo y quiere volver a acceder a la versión de prueba.
    - **Dirección URL de la instancia**
        - *Compromiso con el cliente*: URL a la que se dirigirá al usuario de la versión de prueba cuando la inicie. Esta suele ser la dirección URL de la instancia de Dynamics 365 en la que se han instalado la aplicación y los datos de ejemplo. Valor de ejemplo: `https://testdrive.crm.dynamics.com`.
        - *Aplicación de lienzo (Power Apps)*
            1. Abra la página del **portal de PowerApps** e inicie sesión.
            2. Seleccione **Aplicaciones** y, luego, los puntos suspensivos de la aplicación.
            4. Seleccione **Detalles**.
            5. Copie el **vínculo web** de la pestaña **Detalles**:

                [ ![Muestra la ventana Aplicación de lienzo de TestDrive.](./media/test-drive/testdrive-canvas-app.png) ](./media/test-drive/testdrive-canvas-app.png#lightbox)

    - **Dirección URL de API web de la instancia**
        - *Compromiso con el cliente*: la dirección URL de API web para la instancia de Dynamics 365. Para recuperar este valor, inicie sesión en la instancia de Microsoft Dynamics 365 y seleccione **Configuración** > **Personalización** > **Recursos de desarrollador** > **API web de la instancia** y copie la dirección (URL). Valor de ejemplo:

            :::image type="content" source="./media/test-drive/sample-web-api-url.png" alt-text="Ejemplo de API web de instancia.":::

        - *Aplicación de lienzo (Power Apps)* : si no usa CE/Dataverse como back-end para la aplicación de lienzo, utilice `https://localhost` como marcador de posición.

    - **Nombre de rol**
        - *Compromiso del cliente*: nombre del rol de seguridad personalizado de Dynamics 365 que creó para la versión de prueba. También puede usar un rol existente. Un nuevo rol debe tener los privilegios mínimos necesarios agregados para iniciar sesión en una instancia de Customer Engagement. Consulte [Privilegios mínimos necesarios para iniciar sesión en Microsoft Dynamics 365](https://community.dynamics.com/crm/b/crminogic/archive/2016/11/24/minimum-privileges-required-to-login-microsoft-dynamics-365). Este es el rol que se asignará a los usuarios durante su versión de prueba. Valor de ejemplo: `testdriverole`.
        - *Aplicación de lienzo (Power Apps)* : use "ND" si no usa CE/Dataverse como origen de datos de back-end.
    
    > [!IMPORTANT]
    > Asegúrese de que la comprobación del grupo de seguridad no está agregada. Esto permite que el usuario se sincronice con la instancia de Customer Engagement.

    - **Id. de inquilino de Azure Active Directory**: identificador del inquilino de Azure para su instancia de Dynamics 365. Para recuperar este valor, inicie sesión en Azure Portal y vaya a **Azure Active Directory** > **Propiedades** y copie el id. de directorio. Valor de ejemplo: 172f988bf-86f1-41af-91ab-2d7cd01112341.
    - **Nombre de inquilino de Azure Active Directory**: nombre del inquilino de Azure para su instancia de Dynamics 365. Use el formato `<tenantname>.onmicrosoft.com`. Valor de ejemplo: `testdrive.onmicrosoft.com`.
    - **Id. de aplicación de Azure Active Directory**: identificador de la aplicación de Azure Active Directory (AD) que creó en el paso 5. Valor de ejemplo: `53852862-a2ae-4e43-9461-faa49650a096`.
    - **Secreto de cliente de la aplicación de Azure Active Directory**: secreto de la aplicación de Azure AD que creó en el paso 5. Valor de ejemplo: `IJUgaIOfq9b9LbUjeQmzNBW4VGn6grr1l/n3aMrnfdk=`.

7. Proporcione los detalles del listado de marketplace. Seleccione **Idioma** para ver más campos obligatorios.

    [ ![Muestra la página de detalles del listado de Marketplace.](./media/test-drive/marketplace-listing-details-workspaces.png) ](./media/test-drive/marketplace-listing-details-workspaces.png#lightbox)

    - **Descripción**: introducción de la versión de prueba. Este texto se mostrará al usuario durante el aprovisionamiento de la versión de prueba. Este campo es compatible con HTML, si desea proporcionar contenido con formato (requerido).
    - **Manual del usuario**: manual del usuario en PDF que ayuda a los usuarios de la versión de prueba a aprender a usar la aplicación (requerido).
    - **Vídeo de demostración de la versión de prueba**: vídeo que presenta la aplicación (opcional).

---

## <a name="configure-for-dynamics-365-operations"></a>Configuración para Dynamics 365 Operations

[!INCLUDE [Workspaces view note](./includes/preview-interface.md)]

#### <a name="workspaces-view"></a>[Vista de áreas de trabajo](#tab/workspaces-view)

1. Inicie sesión en el [Centro de partners](https://partner.microsoft.com/dashboard/commercial-marketplace/overview).
2. Si no puede acceder al vínculo anterior, debe enviar una solicitud [aquí](https://appsource.microsoft.com/partners/list-an-app) para publicar la aplicación. Una vez que revisemos la solicitud, se le concederá acceso para iniciar el proceso de publicación.
3. Busque una oferta de **Dynamics 365 for Operations** existente o cree una nueva.
4. En la página **Configuración de la oferta**, active la casilla **Habilitar una versión de prueba**, seleccione un **tipo de versión de prueba** (consulte la viñeta siguiente) y seleccione **Guardar borrador**.

    [![Activación de la casilla "Habilitar una versión de prueba".](media/test-drive/enable-test-drive-check-box-operations-workspaces.png)](media/test-drive/enable-test-drive-check-box-operations-workspaces.png#lightbox)

    - **Tipo de versión de prueba**: elija la opción **Dynamics 365 for Operations**. Esto significa que Microsoft hospedará y mantendrá el servicio que realiza el aprovisionamiento y desaprovisionamiento de usuarios de la versión de prueba.

5. Conceda permiso a Microsoft AppSource para aprovisionar y desaprovisionar los usuarios de la versión de prueba del inquilino mediante [estas instrucciones](https://github.com/Microsoft/AppSource/blob/master/Microsoft%20Hosted%20Test%20Drive/Setup-your-Azure-subscription-for-Dynamics365-Microsoft-Hosted-Test-Drives.md). En este paso, generará los valores **Azure AD App ID** (Identificador de aplicación de Azure AD) y **Azure AD App Key** (Clave de aplicación de Azure AD) que se mencionan a continuación.
6. Complete estos campos en la página **Configuración técnica** >  **de la versión de prueba**.

    [ ![Muestra la página de configuración técnica de Marketplace.](./media/test-drive/technical-config-details-operations-workspaces.png) ](./media/test-drive/technical-config-details-operations-workspaces.png#lightbox)

    - **Máximo de versiones de prueba simultáneas**: número de usuarios simultáneos que pueden tener una versión de prueba activa en ejecución al mismo tiempo. Cada usuario consumirá una licencia de Dynamics mientras su versión de prueba esté activa, por lo que debe asegurarse de tener número de licencias de Dynamics disponibles para los usuarios de la versión de prueba. La cantidad recomendada es de 3 a 5.
    - **Duración de la versión de prueba**: número de horas que estará activa la versión de prueba del usuario. Una vez transcurrido este tiempo, el usuario se desaprovisionará del inquilino. Se recomiendan entre 2 y 24 horas dependiendo de la complejidad de la aplicación. El usuario siempre puede solicitar otra versión de prueba si se agota el tiempo y quiere volver a acceder a la versión de prueba.
    - **URL de la instancia**: URL a la que se dirigirá al usuario de la versión de prueba cuando la inicie. Esta suele ser la dirección URL de la instancia de Dynamics 365 en la que se han instalado la aplicación y los datos de ejemplo. Valor de ejemplo: `https://testdrive.crm.dynamics.com`.
    - **Id. de inquilino de Azure Active Directory**: identificador del inquilino de Azure para su instancia de Dynamics 365. Para recuperar este valor, inicie sesión en Azure Portal y vaya a **Azure Active Directory** > **Propiedades** y copie el id. de directorio. Valor de ejemplo: 172f988bf-86f1-41af-91ab-2d7cd01112341.
    - **Nombre de inquilino de Azure Active Directory**: nombre del inquilino de Azure para su instancia de Dynamics 365. Use el formato `<tenantname>.onmicrosoft.com`. Valor de ejemplo: `testdrive.onmicrosoft.com`.
    - **Id. de aplicación de Azure Active Directory**: identificador de la aplicación de Azure Active Directory (AD) que creó en el paso 5. Valor de ejemplo: `53852862-a2ae-4e43-9461-faa49650a096`.
    - **Secreto de cliente de la aplicación de Azure Active Directory**: secreto de la aplicación de Azure AD que creó en el paso 5. Valor de ejemplo: `IJUgaIOfq9b9LbUjeQmzNBW4VGn6grr1l/n3aMrnfdk=`.
    - **Entidad legal de la prueba**: proporcione una entidad legal para asignar un usuario de prueba. Puede crear una nueva en [Creación o modificación de una entidad jurídica](/dynamicsax-2012/appuser-itpro/create-or-modify-a-legal-entity).
    - **Nombre del rol**: nombre del árbol de objetos de aplicación (AOT) del rol de seguridad de Dynamics 365 personalizado que creó para la versión de prueba. Este es el rol que se asignará a los usuarios durante su versión de prueba.

        :::image type="content" source="./media/test-drive/security-config.png" alt-text="Página de configuración de seguridad.":::

7. Complete los campos de la página **Listas de Marketplace de la versión de prueba**.

    [![Página de detalles del listado de marketplace.](media/test-drive/marketplace-listing-details-ops-workspaces.png)](media/test-drive/marketplace-listing-details-workspaces.png#lightbox)

    - **Descripción**: introducción de la versión de prueba. Este texto se mostrará al usuario durante el aprovisionamiento de la versión de prueba. Este campo es compatible con HTML, si desea proporcionar contenido con formato (requerido).
    - **Manual del usuario**: manual del usuario en PDF que ayuda a los usuarios de la versión de prueba a aprender a usar la aplicación (requerido).
    - **Vídeo de demostración de la versión de prueba**: vídeo que presenta la aplicación (opcional).

#### <a name="current-view"></a>[Vista actual](#tab/current-view)

1. Inicie sesión en el [Centro de partners](https://go.microsoft.com/fwlink/?linkid=2165507).
2. Si no puede acceder al vínculo anterior, debe enviar una solicitud [aquí](https://appsource.microsoft.com/partners/list-an-app) para publicar la aplicación. Una vez que revisemos la solicitud, se le concederá acceso para iniciar el proceso de publicación.
3. Busque una oferta de **Dynamics 365 for Operations** existente o cree una nueva.
4. Active la casilla **Habilitar una versión de prueba** y seleccione un **Tipo de versión de prueba** (consulte la siguiente ilustración) y, a continuación, seleccione **Guardar borrador**.

    [![Activación de la casilla "Habilitar una versión de prueba".](media/test-drive/enable-test-drive-check-box-operations.png)](media/test-drive/enable-test-drive-check-box-operations.png#lightbox)

    - **Tipo de versión de prueba**: elija la opción **Dynamics 365 for Operations**. Esto significa que Microsoft hospedará y mantendrá el servicio que realiza el aprovisionamiento y desaprovisionamiento de usuarios de la versión de prueba.

5. Conceda permiso a Microsoft AppSource para aprovisionar y desaprovisionar los usuarios de la versión de prueba del inquilino mediante [estas instrucciones](https://github.com/Microsoft/AppSource/blob/master/Microsoft%20Hosted%20Test%20Drive/Setup-your-Azure-subscription-for-Dynamics365-Microsoft-Hosted-Test-Drives.md). En este paso, generará los valores **Azure AD App ID** (Identificador de aplicación de Azure AD) y **Azure AD App Key** (Clave de aplicación de Azure AD) que se mencionan a continuación.
6. Rellene estos campos en la página **Test drive technical configuration** (Configuración técnica de la versión de prueba).

    [![Página de configuración técnica de marketplace.](media/test-drive/technical-config-details.png)](media/test-drive/technical-config-details.png#lightbox)

    - **Máximo de versiones de prueba simultáneas**: número de usuarios simultáneos que pueden tener una versión de prueba activa en ejecución al mismo tiempo. Cada usuario consumirá una licencia de Dynamics mientras su versión de prueba esté activa, por lo que debe asegurarse de tener número de licencias de Dynamics disponibles para los usuarios de la versión de prueba. La cantidad recomendada es de 3 a 5.
    - **Duración de la versión de prueba**: número de horas que estará activa la versión de prueba del usuario. Una vez transcurrido este tiempo, el usuario se desaprovisionará del inquilino. Se recomiendan entre 2 y 24 horas dependiendo de la complejidad de la aplicación. El usuario siempre puede solicitar otra versión de prueba si se agota el tiempo y quiere volver a acceder a la versión de prueba.
    - **URL de la instancia**: URL a la que se dirigirá al usuario de la versión de prueba cuando la inicie. Esta suele ser la dirección URL de la instancia de Dynamics 365 en la que se han instalado la aplicación y los datos de ejemplo. Valor de ejemplo: `https://testdrive.crm.dynamics.com`.
    - **Id. de inquilino de Azure Active Directory**: identificador del inquilino de Azure para su instancia de Dynamics 365. Para recuperar este valor, inicie sesión en Azure Portal y vaya a **Azure Active Directory** > **Propiedades** y copie el id. de directorio. Valor de ejemplo: 172f988bf-86f1-41af-91ab-2d7cd01112341.
    - **Nombre de inquilino de Azure Active Directory**: nombre del inquilino de Azure para su instancia de Dynamics 365. Use el formato `<tenantname>.onmicrosoft.com`. Valor de ejemplo: `testdrive.onmicrosoft.com`.
    - **Id. de aplicación de Azure Active Directory**: identificador de la aplicación de Azure Active Directory (AD) que creó en el paso 5. Valor de ejemplo: `53852862-a2ae-4e43-9461-faa49650a096`.
    - **Secreto de cliente de la aplicación de Azure Active Directory**: secreto de la aplicación de Azure AD que creó en el paso 5. Valor de ejemplo: `IJUgaIOfq9b9LbUjeQmzNBW4VGn6grr1l/n3aMrnfdk=`.
    - **Entidad legal de la prueba**: proporcione una entidad legal para asignar un usuario de prueba. Puede crear una nueva en [Creación o modificación de una entidad jurídica](/dynamicsax-2012/appuser-itpro/create-or-modify-a-legal-entity).
    - **Nombre del rol**: nombre del árbol de objetos de aplicación (AOT) del rol de seguridad de Dynamics 365 personalizado que creó para la versión de prueba. Este es el rol que se asignará a los usuarios durante su versión de prueba.

        :::image type="content" source="./media/test-drive/security-config.png" alt-text="Página de configuración de seguridad.":::

7. Proporcione los detalles del listado de marketplace. Seleccione **Idioma** para ver más campos obligatorios.

    [![Página de detalles del listado de marketplace.](media/test-drive/marketplace-listing-details.png)](media/test-drive/marketplace-listing-details.png#lightbox)

    - **Descripción**: introducción de la versión de prueba. Este texto se mostrará al usuario durante el aprovisionamiento de la versión de prueba. Este campo es compatible con HTML, si desea proporcionar contenido con formato.
    - **Manual del usuario**: manual del usuario en PDF que ayuda a los usuarios de la versión de prueba a aprender a usar la aplicación.
    - **Vídeo de demostración de la versión de prueba**: vídeo que presenta la aplicación (opcional).

---

<!--
## Next steps

- [Set up your Azure subscription](test-drive-azure-subscription-setup.md) -->
