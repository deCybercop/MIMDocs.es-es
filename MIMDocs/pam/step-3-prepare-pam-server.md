---
title: "Implementación de PAM, paso 3: servidor de PAM | Microsoft Identity Manager"
description: "Prepare un servidor de PAM que hospede tanto SQL como SharePoint para la implementación de Privileged Access Management."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 68ec2145-6faa-485e-b79f-2b0c4ce9eff7
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 1a21399df9528f689b811400a660543853d88472


---

# Paso 3: Preparar un servidor de PAM

>[!div class="step-by-step"]
[« Paso 2](step-2-prepare-priv-domain-controller.md)
[Paso 4 »](step-4-install-mim-components-on-pam-server.md)

## Instalación de Windows Server 2012 R2
En una tercera máquina virtual, instale Windows Server 2012 R2, concretamente Windows Server 2012 R2 Standard (servidor con una GUI) x64, para crear *PAMSRV*. Puesto que se instalarán SQL Server y SharePoint 2013 en este equipo, debe tener al menos 8 GB de RAM.

1. Seleccione **Windows Server 2012 R2 Standard (servidor con una GUI) x64**.

    ![Captura de pantalla de la selección de Windows Server Standard con GUI](media/PAM_GS_Select_WS2012.png)

2. Revise los términos de licencia y acéptelos.

3.  Como el disco estará vacío, seleccione **Personalizada: Instalar solo Windows** y use el **espacio de disco que no se ha inicializado**.

4.  Inicie sesión en ese nuevo equipo como su administrador.  Mediante el Panel de control, asígnele una dirección IP estática en la red virtual, configure esa interfaz de red para que envíe consultas DNS a la dirección IP de PRIVDC y establezca el nombre del equipo en *PAMSRV*.  Esto requerirá el reinicio del servidor.

5.  Si la red virtual no proporciona conectividad a Internet, agregue una interfaz de red adicional al equipo que proporciona una conexión a Internet.  Esto será necesario para instalar SharePoint y se podrá deshabilitar cuando se complete este paso.

6.  Una vez reiniciado el servidor, inicie sesión como administrador. Mediante el Panel de control, configure el equipo para que compruebe si hay actualizaciones y para que instale todas las actualizaciones necesarias.  Esto podría precisar el reinicio del servidor.

7.  Una vez que el servidor se haya reiniciado, inicie sesión como administrador, abra el Panel de control y una PAMSRV al dominio PRIV (priv.contoso.local).  Para ello se deberá proporcionar el nombre de usuario y las credenciales de un administrador de dominio PRIV (PRIV\Administrador). Después de que aparezca el mensaje de bienvenida, cierre el cuadro de diálogo y reinicie este servidor.


### Incorporación de los roles de servidor web (IIS) y servidor de aplicaciones
Agregue los roles Servidor Web (IIS) y Servidor de aplicaciones, las características de .NET Framework 3.5, el módulo de Active Directory para Windows PowerShell y otras características requeridas por SharePoint.

1.  Inicie sesión como administrador de dominio PRIV (PRIV\Administrador) e inicie PowerShell.

2.  Escriba los siguientes comandos: Tenga en cuenta que puede que sea necesario especificar una ubicación diferente para los archivos de origen de las características de .NET Framework 3.5. Normalmente, estas características no están presentes cuando se instala Windows Server, pero están disponibles en la carpeta en paralelo (SxS) de la carpeta de orígenes de disco de instalación del sistema operativo, por ejemplo, “*d:\Sources\SxS”.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,
    rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,
    Windows-Identity-Foundation,Server-Media-Foundation,
    Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

### Configuración de la directiva de seguridad del servidor
Configure la directiva de seguridad de servidor para que permita que las cuentas recién creadas se ejecuten como servicios.

1.  Inicie el programa **Directiva de seguridad local** .   
2.  Vaya a **Directivas locales** > **Asignación de derechos de usuario**.  
3.  En el panel de detalles haga clic con el botón secundario en **Iniciar sesión como servicio**y seleccione **Propiedades**.  
4.  Haga clic en **Agregar usuario o grupo** y, en Nombres de usuario y grupo, escriba *priv\mimmonitor; priv\MIMService; priv\SharePoint; priv\mimcomponent; priv\SqlServer*. Haga clic en **Comprobar nombres** y luego en **Aceptar**.  

5.  Haga clic en **Aceptar** para cerrar la ventana Propiedades.  
6.  En el panel de detalles, haga clic con el botón derecho en **Denegar el acceso desde la red a este equipo** y seleccione **Propiedades**.  
7.  Haga clic en **Agregar usuario o grupo** y, en Nombres de usuario y grupo, escriba *priv\mimmonitor; priv\MIMService; priv\mimcomponent* y haga clic en **Aceptar**.  
8.  Haga clic en **Aceptar** para cerrar la ventana Propiedades.  

9. En el panel de detalles, haga clic con el botón derecho en **Denegar el inicio de sesión localmente** y seleccione **Propiedades**.  
10. Haga clic en **Agregar usuario o grupo** y, en Nombres de usuario y grupo, escriba *priv\mimmonitor; priv\MIMService; priv\mimcomponent* y haga clic en **Aceptar**.  
11. Haga clic en **Aceptar** para cerrar la ventana Propiedades.  
12. Cierre la ventana Directiva de seguridad local.  

13. Abra el Panel de control y cambie a **Cuentas de usuario**.  
14. Haga clic en **Conceder acceso al equipo a otros usuarios**.  
15. Haga clic en **Agregar**, escriba el usuario *MIMADMIN* en el dominio *PRIV* y, en la siguiente pantalla del asistente, haga clic en **Agregar este usuario como administrador**.  
16. Haga clic en **Agregar**, escriba el usuario *SharePoint* en el dominio *PRIV* y, en la siguiente pantalla del asistente, haga clic en **Agregar este usuario como administrador**.  
17. Cierre el Panel de control.  

### Cambio de la configuración de IIS
Hay dos formas de cambiar la configuración de IIS para permitir que las aplicaciones usen el modo de autenticación de Windows. Asegúrese de haber iniciado sesión como MIMAdmin y luego siga una de estas opciones.

Si quiere usar PowerShell:
1.  Haga clic con el botón derecho en PowerShell y seleccione **Ejecutar como administrador**.  
2.  Detenga IIS y desbloquee la configuración de host de aplicación con estos comandos  
    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```  

Si quiere usar un editor de texto como el Bloc de notas:   
1. Abra el archivo **C:\Windows\System32\inetsrv\config\applicationHost.config**   
2. Desplácese hasta la línea 82 de ese archivo. El valor de etiqueta de **overrideModeDefault** debería ser **<section name="windowsAuthentication" overrideModeDefault="Deny" />**  
3. Cambie el valor de **overrideModeDefault** a *Permitir*  
4. Guarde el archivo y reinicie IIS con el comando de PowerShell `iisreset /START`

## Instalar SQL Server
Si SQL Server aún no está en el entorno bastión, instale SQL Server 2012 (Service Pack 1 o posterior) o SQL Server 2014. En los siguientes pasos se supone que se ha instalado SQL 2014.

1. Asegúrese de iniciar sesión como MIMAdmin.
2. Haga clic con el botón derecho en PowerShell y seleccione **Ejecutar como administrador**.   
3. Vaya al directorio donde se encuentra el programa de instalación de SQL Server.  
4. Escriba el siguiente comando:  
    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="PRIV\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="PRIV\MIMAdmin"
    ```

## Instalación de SharePoint Foundation 2013

Mediante el instalador de SharePoint Foundation 2013 con SP1, instale los requisitos previos de software de SharePoint en PAMSRV.

> [!NOTE]
> El programa de instalación necesita una conexión a Internet para descargar los requisitos previos. Después de su instalación, el servidor se reiniciará.

1. Haga clic con el botón derecho en PowerShell y seleccione **Ejecutar como administrador**.  
2. Cambie al directorio donde se desempaquetó SharePoint.  
3. Escriba el comando `.\prerequisiteinstaller.exe`.

Una vez que se hayan instalado los requisitos previos de SharePoint, instale SharePoint Foundation 2013 con SP1.

1.  Haga clic con el botón derecho en PowerShell y seleccione **Ejecutar como administrador**.  
2.  Cambie al directorio donde se desempaquetó SharePoint.  
3.  Escriba el comando `.\setup.exe`.  
4.  Seleccione el tipo de **servidor completo** .  
5.  Una vez completada la instalación, seleccione esta opción para que se ejecute el asistente.  

### Configuración de SharePoint
Ejecute el Asistente para configuración de Productos de SharePoint para configurar SharePoint.

1.  En la pestaña Conectar a una granja de servidores, cambie a **Crear un nuevo conjunto de servidores**.  
2.  Especifique **PAMSRV** como servidor de base de datos de la base de datos de configuración y **PRIV\SharePoint** como cuenta de acceso a la base de datos que usará SharePoint.  
3.  Especifique una contraseña como la frase de contraseña de seguridad de la granja de servidores (no se usará más adelante en este tutorial).  
4.  Por ahora, acepte el resto de la configuración predeterminada del Asistente para la configuración de SharePoint para crear una granja de un solo servidor.    
5.  Cuando el Asistente para la configuración complete la tarea de configuración 10 de 10, haga clic en **Finalizar** y se abrirá un explorador web.  
6.  En la ventana emergente de Internet Explorer, autentíquese como administrador de dominio (PRIV\MIMAdmin) para continuar.  
7.  Inicie el asistente de la aplicación web para configurar la granja de SharePoint.  
8.  Seleccione usar la cuenta administrada existente (PRIV\SharePoint), desactive para deshabilitar cualquier servicio opcional y haga clic en **Siguiente**.  
9. Una vez que aparezca la ventana de creación de una colección de sitios, haga clic en **Omitir** y luego en **Finalizar**.  

## Creación de una aplicación web de SharePoint Foundation 2013
Cuando se completen los asistentes, use PowerShell para crear una aplicación web de SharePoint Foundation 2013 que hospede el Portal de MIM. Dado que este tutorial tiene fines demostrativos, no se habilitará SSL.

1.  Haga clic con el botón derecho en Consola de administración de SharePoint 2013, seleccione **Ejecutar como administrador** y ejecute el siguiente script de PowerShell:

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity PRIV\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"            -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://PAMSRV.priv.contoso.local
    ```

2. Aparecerá un mensaje de advertencia que indica que se está usando el método de autenticación clásico de Windows y el comando final puede tardar varios minutos en responder.  Cuando se complete, el resultado indicará la dirección URL del nuevo portal.

> [!NOTE]
> Mantenga abierta la ventana Consola de administración de SharePoint 2013 para usarla en el siguiente paso.

## Creación de una colección de sitios de SharePoint
A continuación, cree una Colección de sitios de SharePoint asociada a esa aplicación web para hospedar el Portal de MIM.

1.  Inicie la **Consola de administración de SharePoint 2013** si todavía no está abierta y ejecute el siguiente script de PowerShell

    ```
    $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
    $w = Get-SPWebApplication http://pamsrv.priv.contoso.local:82
    New-SPSite -Url $w.Url -Template $t -OwnerAlias PRIV\MIMAdmin                -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias PRIV\BackupAdmin
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
    ```

    Compruebe que la variable **CompatibilityLevel** está establecida en *14*. Si devuelve *15*, no se creó la colección de sitios para la versión de la experiencia 2010. Elimine la colección de sitios y vuelva a crearla.

2.  Ejecute los siguientes comandos de PowerShell en la **Consola de administración de SharePoint 2013**. Así se deshabilitará el estado de vista del lado servidor de SharePoint y la tarea **Trabajo de análisis de mantenimiento (Cada hora, Temporizador de Microsoft SharePoint Foundation, Todos los servidores)** de SharePoint.

    ```
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

## Cambio de la configuración de actualización

1. Abra el Panel de control, vaya a **Windows Update** y haga clic en **Cambiar configuración**.  
2. Cambie la configuración para recibir actualizaciones de Windows Update y otros productos desde Microsoft Update.  
3. Compruebe si hay nuevas actualizaciones y asegúrese de que todas las instalaciones importantes pendientes estén instaladas antes de continuar.

## Establecimiento del sitio web como la intranet local

1. Inicie Internet Explorer y abra una nueva pestaña del explorador web.
2. Vaya a http://pamsrv.priv.contoso.local:82/ e inicie sesión como PRIV\MIMAdmin.  Se mostrará un sitio de SharePoint vacío denominado "Portal de MIM".  
3. En Internet Explorer, abra **Opciones de Internet**, cambie a la pestaña **Seguridad**, seleccione **Intranet local** y agregue la dirección URL `http://pamsrv.priv.contoso.local:82/`.

Si se produce un error de inicio de sesión, puede que sea necesario actualizar los SPN de Kerberos que se crearon en el [paso 2](step-2-prepare-priv-domain-controller.md).

## Inicio del servicio de administración de SharePoint

Mediante **Servicios** (que se encuentra en Herramientas administrativas), inicie el servicio **Administración de SharePoint** si aún no se está ejecutando.

En el paso 4, se iniciará la instalación de los componentes de MIM en el servidor PAM.

>[!div class="step-by-step"]
[« Paso 2](step-2-prepare-priv-domain-controller.md)
[Paso 4 »](step-4-install-mim-components-on-pam-server.md)



<!--HONumber=Oct16_HO2-->


