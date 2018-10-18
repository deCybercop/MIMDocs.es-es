---
title: Configuración de SharePoint para Microsoft Identity Manager 2016 | Microsoft Docs
description: Instale y configure SharePoint Foundation para que pueda hospedar la página del portal de MIM.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 466f5eb7d4aee27336948e15f96087d6ba898170
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358642"
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Configuración de un servidor de administración de identidades: SharePoint

> [!div class="step-by-step"]
> [«SQL Server 2016](prepare-server-sql2016.md)
> [Exchange Server»](prepare-server-exchange.md)
> 
> [!NOTE]
> Este tutorial usa los valores y nombres de ejemplo de una empresa llamada Contoso. Reemplácelos con sus propios valores. Por ejemplo:
> - Nombre del controlador de dominio: **corpdc**
> - Nombre de dominio: **contoso**
> - Nombre de servidor del servicio MIM: **corpservice**
> - Nombre del servidor de sincronización de MIM: **corpsync**
> - Nombre de SQL Server: **corpsql**
> - Contraseña - <strong>Pass@word1</strong>


## <a name="install-sharepoint-2016"></a>Instale **SharePoint 2016**.

> [!NOTE]
> El programa de instalación requiere una conexión a Internet para descargar los requisitos previos. Si el equipo está en una red virtual que no proporciona conectividad a Internet, agregue una interfaz de red adicional al equipo que proporciona conexión a Internet. Se puede deshabilitar una vez completada la instalación.

Siga estos pasos para instalar SharePoint 2016. Después de finalizar la instalación, el servidor se reiniciará.

1.  Inicie **PowerShell** con una cuenta de dominio con administrador local en **corpservice** y **sysadmin** en el servidor de base de datos SQL; se usará **contoso\miminstall**.

    -   Cambie al directorio donde se desempaquetó SharePoint.

    -   Escriba el siguiente comando:

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Una vez instalados los requisitos previos de **SharePoint**, escriba el siguiente comando para instalar **SharePoint 2016**:

    ```
    .\setup.exe
    ```

3.  Seleccione el tipo de servidor completo.

4.  Cuando finalice la instalación, ejecute el asistente.

## <a name="run-the-wizard-to-configure-sharepoint"></a>Ejecución del Asistente para configuración de SharePoint

Siga los pasos que se indican en el **Asistente para configuración de productos de SharePoint** para configurar SharePoint de forma que funcione con MIM.

1. En la pestaña **Conectar a una granja de servidores** , cambie para crear una nueva granja de servidores.

2. Especifique este servidor como servidor de bases de datos, por ejemplo, **corpsql** para la base de datos de configuración y *Contoso\SharePoint* como la cuenta de acceso a la base de datos que se usará en SharePoint.
3. Cree una contraseña para la frase de contraseña de seguridad de la granja de servidores.

4. En el Asistente para configuración, se recomienda seleccionar [MinRole](https://docs.microsoft.com/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server-2016) como tipo de **Front-end**.

5. Cuando el asistente para la configuración complete la última tarea de configuración (10 de 10), haga clic en Finalizar y se abrirá un explorador web.

6. Si en la ventana emergente de Internet Explorer se le solicita que lo haga, autentíquese como *Contoso\miminstall* (o la cuenta de administrador equivalente) para continuar.

7. En el Asistente Web (dentro de la aplicación web), haga clic en **Cancelar/Omitir**.


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>Preparación de SharePoint para hospedar el portal de MIM

> [!NOTE]
> Inicialmente, no se configurará SSL. Asegúrese de configurar SSL o un equivalente antes de habilitar el acceso a este portal.

1. Inicie la **Consola de administración de SharePoint 2016** y ejecute el siguiente script de PowerShell para crear una **aplicación web de SharePoint 2016**.

    ```
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > Aparecerá un mensaje de advertencia que indicará que se está usando el método de autenticación clásico de Windows y el comando final puede tardar varios minutos en responder. Cuando se complete, el resultado indicará la dirección URL del nuevo portal. Mantenga abierta la ventana de la **Consola de administración de SharePoint 2016** para consultarla más tarde.

2. Inicie el Shell de administración de SharePoint 2016 y ejecute el siguiente script de PowerShell para crear una **colección de sitios de SharePoint** asociada a esa aplicación web.

   ```
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.CompatibilityLevel
   ```

   > [!NOTE]
   > Compruebe que el resultado de la variable *CompatibilityLevel* es "15". Si el resultado no es "15", la colección de sitios no se creó para la versión correcta de la experiencia. Elimine la colección de sitios y vuelva a crearla.

3. Deshabilite el **estado de vista del lado servidor** y la tarea "Trabajo de análisis de mantenimiento (Cada hora, Temporizador de Microsoft SharePoint Foundation, Todos los servidores)" de SharePoint ejecutando los siguientes comandos de PowerShell en la **Consola de administración de SharePoint 2016**:

   ```
   $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
   $contentService.ViewStateOnServer = $false;
   $contentService.Update();
   Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
   ```

4. En el servidor de administración de identidades, abra una nueva pestaña de explorador web, vaya a http://mim.contoso.com/ e inicie sesión con *contoso\miminstall*.  Se mostrará un sitio de SharePoint vacío denominado *Portal de MIM* .

    ![Imagen del portal de MIM en http://mim.contoso.com/](media/prepare-server-sharepoint/MIM_DeploySP1new.png)

5. Copie la dirección URL; a continuación, abra **Opciones de Internet** en Internet Explorer, vaya a la pestaña **Seguridad**, seleccione **Intranet local** y haga clic en **Sitios**.

    ![Imagen de Opciones de Internet](media/MIM-DeploySP2.png)

6. En la ventana **Intranet local**, haga clic en **Opciones avanzadas** y pegue la dirección URL que copió en el cuadro de texto **Agregar este sitio web a la zona de**. Haga clic en **Agregar** y, a continuación, cierre las ventanas.

7. Abra el programa **Herramientas administrativas**, vaya a **Servicios**, busque el servicio de administración de SharePoint e inícielo si aún no se está ejecutando.

> [!div class="step-by-step"]  
> [«SQL Server 2016](prepare-server-sql2016.md)
> [Exchange Server»](prepare-server-exchange.md)
