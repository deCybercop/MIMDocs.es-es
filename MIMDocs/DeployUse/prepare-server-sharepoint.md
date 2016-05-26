---
# required metadata

title: Configuración de un servidor de administración de identidades&#58; SharePoint | Microsoft Identity Manager
description: Instale y configure SharePoint Foundation para que pueda hospedar la página del portal de MIM.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Configuración de un servidor de administración de identidades: SharePoint

>[!div class="step-by-step"]

> [!NOTE]
> « SQL Server 2014 Exchange Server » Este tutorial usa los valores y nombres de ejemplo de una empresa llamada Contoso.
> - Reemplácelos con sus propios valores.
> - Por ejemplo:
> - Nombre del controlador de dominio: **mimservername**


## Nombre de dominio: **contoso**

> [!NOTE]
> Contraseña: **Pass@word1** Instale **SharePoint Foundation 2013 con SP1**. El programa de instalación requiere una conexión a Internet para descargar los requisitos previos.

Si el equipo está en una red virtual que no proporciona conectividad a Internet, agregue una interfaz de red adicional al equipo que proporciona conexión a Internet. Se puede deshabilitar una vez completada la instalación.

1.  Siga estos pasos para instalar SharePoint Foundation 2013 SP1.

    -   Después de finalizar la instalación, el servidor se reiniciará.

    -   Inicie **PowerShell** como administrador de dominio.

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Cambie al directorio donde se desempaquetó SharePoint.

    ```
    .\setup.exe
    ```

3.  Escriba el siguiente comando:

4.  Una vez instalados los requisitos previos de **SharePoint** , escriba el siguiente comando para instalar **SharePoint Foundation 2013 con SP1** :

## Seleccione el tipo de servidor completo.

Cuando finalice la instalación, ejecute el asistente.

1. Ejecución del Asistente para configuración de SharePoint

2. Siga los pasos que se indican en el **Asistente para configuración de productos de SharePoint** para configurar SharePoint de forma que funcione con MIM.

3. En la pestaña **Conectar a una granja de servidores** , cambie para crear una nueva granja de servidores.

4. Especifique este servidor como servidor de bases de datos para la base de datos de configuración y *Contoso\SharePoint* como la cuenta de acceso a la base de datos que usará SharePoint.

5. Cree una contraseña para la frase de contraseña de seguridad de la granja de servidores.

6. Cuando el Asistente para la configuración complete la tarea de configuración 10 de 10, haga clic en Finalizar y se abrirá un explorador web.

7. En la ventana emergente de Internet Explorer, autentíquese como *Contoso\Administrador* (o la cuenta de administrador de dominio equivalente) para continuar.

8. Inicie el asistente (dentro de la aplicación web) para configurar la granja de SharePoint.  Seleccione la opción de usar la cuenta administrada existente (*Contoso\SharePoint*) y haga clic en **Siguiente**.

## En la ventana de **creación de colección de sitios** , haga clic en **Omitir**.

> [!NOTE]
> A continuación, haga clic en **Finalizar**. Preparación de SharePoint para hospedar el portal de MIM

1. Inicialmente, no se configurará SSL.

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"
    -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
    ```

    > [!NOTE] Asegúrese de configurar SSL o un equivalente antes de habilitar el acceso a este portal. Inicie el **Shell de administración de SharePoint 2013** y ejecute el siguiente script de PowerShell para crear una **aplicación web de SharePoint Foundation 2013**. Aparecerá un mensaje de advertencia que indicará que se está usando el método de autenticación clásico de Windows y el comando final puede tardar varios minutos en responder.

2. Cuando se complete, el resultado indicará la dirección URL del nuevo portal.

  ```
  $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
  $w = Get-SPWebApplication http://corpidm.contoso.local:82
  New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
  -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
  $s = SpSite($w.Url)
  $s.AllowSelfServiceUpgrade = $false
  $s.CompatibilityLevel
  ```

  > [!NOTE] Mantenga abierta la ventana del **Shell de administración de SharePoint 2013** para consultarla más tarde. Inicie el Shell de administración de SharePoint 2013 y ejecute el siguiente script de PowerShell para crear una **colección de sitios de SharePoint** asociada a esa aplicación web.

3. Compruebe que el resultado de la variable *CompatibilityLevel* es "14".

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. Si el resultado es "15", la colección de sitios no se creó para la versión de la experiencia 2010. Elimine la colección de sitios y vuelva a crearla.  Deshabilite el **estado de vista del lado servidor** y la tarea "Trabajo de análisis de mantenimiento (Cada hora, Temporizador de Microsoft SharePoint Foundation, Todos los servidores)" de SharePoint ejecutando los siguientes comandos de PowerShell en la **Consola de administración de SharePoint 2013**:

    ![En el servidor de administración de identidades, abra una nueva pestaña del explorador web, vaya a http://localhost:82/ e inicie sesión como *contoso\Administrador*.](media/MIM-DeploySP1.png)

5. Se mostrará un sitio de SharePoint vacío denominado *Portal de MIM* .

    ![Imagen del portal de MIM en http://localhost:82/](media/MIM-DeploySP2.png)

6. Copie la dirección URL; a continuación, abra **Opciones de Internet** en Internet Explorer, vaya a la pestaña **Seguridad**, seleccione **Intranet local** y haga clic en **Sitios**. Imagen de Opciones de Internet

7. En la ventana **Intranet local**, haga clic en **Opciones avanzadas** y pegue la dirección URL que copió en el cuadro de texto **Agregar este sitio web a la zona de**.

>Haga clic en **Agregar** y, a continuación, cierre las ventanas.  
Abra el programa **Herramientas administrativas**, vaya a **Servicios**, busque el servicio de administración de SharePoint e inícielo si aún no se está ejecutando.


<!--HONumber=Apr16_HO3-->


