---
# required metadata

title: Configuración de un servidor de administración de identidades&#58; Windows Server 2012 R2 | Microsoft Identity Manager
description: Obtenga los pasos y los requisitos mínimos para preparar Windows Server 2012 RS de modo que funcione con MIM 2016.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Configuración de un servidor de administración de identidades: Windows Server 2012 R2

>[!div class="step-by-step"]
[« Preparación de un dominio](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)

> [!NOTE]
> En todos los ejemplos siguientes, **mimservername** representa el nombre del controlador de dominio; **contoso**, el nombre de dominio y **Pass@word1**, una contraseña de ejemplo.

## Unión de Windows Server 2012 R2 al dominio

1. Cree una máquina de Windows Server 2012 R2 con un mínimo de 8 GB de RAM. Al instalar, especifique la edición "Windows Server 2012 R2 Standard (servidor con una GUI) x64".

2. Inicie sesión en el nuevo equipo como su administrador.

3. Mediante el Panel de control, asigne al equipo una dirección IP estática en la red. Configure esa interfaz de red para que envíe consultas de DNS a la dirección IP del controlador de dominio del paso anterior y establezca **CORPIDM** como nombre del equipo.  Esto requerirá el reinicio del servidor.

4. Si el equipo está en una red virtual que no proporciona conectividad a Internet, agregue una interfaz de red adicional al equipo que proporciona conexión a Internet.  Debe cumplirse este requisito para instalar SharePoint; lo podrá deshabilitar cuando se complete este paso.

5. Abra el Panel de control y una el equipo al dominio que configuró en el último paso, *contoso.local*.  Ello significa proporcionar el nombre de usuario y las credenciales de un administrador de dominio como *Contoso\Administrador*.  Cuando aparezca el mensaje de bienvenida, cierre el cuadro de diálogo y vuelva a reiniciar este servidor.

6. Inicie sesión en el equipo *CorpIDM* en calidad de administrador de dominio como *Contoso\Administrador*.

7. Inicie una ventana de PowerShell como administrador y escriba el siguiente comando para actualizar el equipo con la configuración de directiva de grupo.

    ```
    gpupdate /force /target:computer
    ```

    Finalizará en un máximo de un minuto y se mostrará el mensaje "La actualización de la directiva de equipo se completó correctamente".

8. Agregue los roles **Servidor Web (IIS)** y **Servidor de aplicaciones**, las características de **.NET Framework** 3.5, 4.0 y 4.5, y **Módulo de Active Directory para Windows PowerShell**.

    ![Imagen de las características de PowerShell](media/MIM-DeployWS2.png)

9. Todavía como administrador, escriba en PowerShell los siguientes comandos. Tenga en cuenta que puede que sea necesario especificar una ubicación diferente para los archivos de origen de las características de **.NET Framework** 3.5. Normalmente, estas características no están presentes cuando se instala Windows Server, pero están disponibles en la carpeta en paralelo (SxS) de la carpeta de orígenes de disco de instalación del sistema operativo, por ejemplo, “*d:\Sources\SxS\*”.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## Configuración de la directiva de seguridad del servidor

Configure la directiva de seguridad de servidor para que permita que las cuentas recién creadas se ejecuten como servicios.

1. Inicie el programa Directiva de seguridad local.

2. Vaya a **Directivas locales > Asignación de derechos de usuario**.

3. En el panel de detalles haga clic con el botón secundario en **Iniciar sesión como servicio**y seleccione **Propiedades**.

    ![Imagen de la directiva de seguridad local](media/MIM-DeployWS3.png)

4. Haga clic en **Agregar usuario o grupo** y escriba `contoso\mimsync; contoso\mimma; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\mimsspr` en el cuadro de texto; después, haga clic en **Comprobar nombres** y en **Aceptar**.

5. Haga clic en **Aceptar** para cerrar la ventana **Propiedades de Iniciar sesión como servicio**.

6.  En el panel de detalles, haga clic con el botón derecho en **Denegar el acceso desde la red a este equipo** y seleccione **Propiedades**.

7. Haga clic en **Agregar usuario o grupo** y escriba `contoso\MIMSync; contoso\MIMService` en el cuadro de texto; a continuación, haga clic en **Aceptar**.

8. Haga clic en **Aceptar** para cerrar la ventana **Propiedades de Denegar el acceso desde la red a este equipo**.

9. En el panel de detalles, haga clic con el botón derecho en **Denegar el inicio de sesión localmente** y seleccione **Propiedades**.

10. Haga clic en **Agregar usuario o grupo** y escriba `contoso\MIMSync; contoso\MIMService` en el cuadro de texto; a continuación, haga clic en **Aceptar**.

11. Haga clic en **Aceptar** para cerrar la ventana **Propiedades de Denegar el inicio de sesión localmente**.

12. Cierre la ventana Directiva de seguridad local.


## Cambie el modo autenticación de Windows IIS.

1.  Abra una ventana de PowerShell.

2.  Detenga IIS mediante el comando *iisreset /STOP*.

        ```
        iisreset /STOP
        C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
        iisreset /START
        ```

>[!div class="step-by-step"]  
[« Preparación de un dominio](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)


<!--HONumber=Apr16_HO2-->


