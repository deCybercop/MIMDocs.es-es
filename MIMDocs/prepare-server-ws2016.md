---
title: Configuración de Windows Server 2016 para MIM 2016 SP1 | Microsoft Docs
description: Conozca los pasos y los requisitos mínimos para preparar Windows Server 2016 para funcionar con MIM 2016 SP1.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 7c77ed0ceb541b9b00ebb9954ce65a53f0f44442
ms.sourcegitcommit: 32d9a963a4487a8649210745c97a3254645e8744
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2018
---
# <a name="set-up-an-identity-management-servers-windows-server-2016"></a>Configuración de un servidor de administración de identidades: Windows Server 2016

>[!div class="step-by-step"]
[«Preparación de un dominio](preparing-domain.md)
[SQL Server 2016»](prepare-server-sql2016.md)

> [!NOTE]
> Este tutorial usa los valores y nombres de ejemplo de una empresa llamada Contoso. Reemplácelos con sus propios valores. Por ejemplo:
> - Nombre del controlador de dominio: **corpdc**
> - Nombre de dominio: **contoso**
> - Nombre de servidor del servicio MIM: **corpservice**
> - Nombre del servidor de sincronización de MIM: **corpsync**
> - Nombre de SQL Server: **corpsql**
> - Contraseña - **Pass@word1**

## <a name="join-windows-server-2016-to-your-domain"></a>Unión de Windows Server 2016 al dominio

Empiece con una máquina de Windows Server 2016 con entre 8 y 12 GB de memoria RAM como mínimo. Al instalar, especifique la edición "Windows Server 2016 Standard/Datacenter (Servidor con una GUI) x64".

1. Inicie sesión en el nuevo equipo como su administrador.

2. Mediante el Panel de control, asigne al equipo una dirección IP estática en la red. Configure esa interfaz de red para enviar consultas DNS a la dirección IP del controlador de dominio del paso anterior y establezca **CORPSERVICE** como nombre del equipo.  Esto requerirá el reinicio del servidor.

3. Abra el Panel de control y una el equipo al dominio que configuró en el último paso, *contoso.com*.  Ello significa proporcionar el nombre de usuario y las credenciales de un administrador de dominio como *Contoso\Administrador*.  Cuando aparezca el mensaje de bienvenida, cierre el cuadro de diálogo y vuelva a reiniciar este servidor.

4. Inicie sesión en el equipo *CORPSERVICE* con una cuenta de dominio con el administrador de máquina local *Contoso\MIMINSTALL*.


5. Inicie una ventana de PowerShell como administrador y escriba el siguiente comando para actualizar el equipo con la configuración de directiva de grupo.

    ```
    gpupdate /force /target:computer
    ```

    Finalizará en un máximo de un minuto y se mostrará el mensaje "La actualización de la directiva de equipo se completó correctamente".

6. Agregue los roles **Servidor Web (IIS)** y **Servidor de aplicaciones**, las características de **.NET Framework** 3.5, 4.0 y 4.5, y **Módulo de Active Directory para Windows PowerShell**.

    ![Imagen de las características de PowerShell](media/MIM-DeployWS2.png)

7. En PowerShell, escriba los siguientes comandos. Tenga en cuenta que puede que sea necesario especificar una ubicación diferente para los archivos de origen de las características de **.NET Framework** 3.5. Normalmente, estas características no están presentes cuando se instala Windows Server, pero están disponibles en la carpeta en paralelo (SxS) de la carpeta de orígenes de disco de instalación del sistema operativo, por ejemplo, “\*d:\Sources\SxS\*”.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>Configuración de la directiva de seguridad del servidor

Configure la directiva de seguridad de servidor para que permita que las cuentas recién creadas se ejecuten como servicios.
> [!NOTE] 
> Según el servidor único de configuración (todo en uno) o el servidor distribuido, solo debe agregar usuarios o grupos en función del rol de la máquina miembro, por ejemplo, servidor de sincronización. 

1. Inicie el programa Directiva de seguridad local.

2. Vaya a **Directivas locales > Asignación de derechos de usuario**.

3. En el panel de detalles haga clic con el botón secundario en **Iniciar sesión como servicio**y seleccione **Propiedades**.

    ![Imagen de la directiva de seguridad local](media/MIM-DeployWS3.png)

4. Haga clic en **Agregar usuario o grupo** y, en el cuadro de texto, escriba lo siguiente según el rol `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`, haga clic en **Comprobar nombres** y haga clic en **Aceptar**.

5. Haga clic en **Aceptar** para cerrar la ventana **Propiedades de Iniciar sesión como servicio**.

6.  En el panel de detalles, haga clic con el botón derecho en **Deny access to this computer from the network** (Denegar acceso a este equipo desde la red) y seleccione **Propiedades**.

[!NOTE] Si se separan los servidores de roles, este paso interrumpirá parte de la funcionalidad, como la característica SSPR.

7. Haga clic en **Agregar usuario o grupo** y escriba `contoso\MIMSync; contoso\MIMService` en el cuadro de texto; a continuación, haga clic en **Aceptar**.

8. Haga clic en **Aceptar** para cerrar la ventana **Propiedades de Denegar el acceso desde la red a este equipo**.

9. En el panel de detalles, haga clic con el botón derecho en **Denegar el inicio de sesión localmente** y seleccione **Propiedades**.

10. Haga clic en **Agregar usuario o grupo** y escriba `contoso\MIMSync; contoso\MIMService` en el cuadro de texto; a continuación, haga clic en **Aceptar**.

11. Haga clic en **Aceptar** para cerrar la ventana **Propiedades de Denegar el inicio de sesión localmente**.

12. Cierre la ventana Directiva de seguridad local.


## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>Cambie el modo de autenticación de Windows de IIS, si es necesario.

1.  Abra una ventana de PowerShell.

2.  Detenga IIS mediante el comando *iisreset /STOP*.

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

>[!div class="step-by-step"]  
[«Preparación de un dominio](preparing-domain.md)
[SQL Server 2016»](prepare-server-sql2016.md)
