---
title: "Implementación de PAM, paso 2: controlador de dominio PRIV | Microsoft Docs"
description: "Prepare el controlador de dominio PRIV que proporcionará el entorno bastión donde Privileged Access Management está aislado."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 0e9993a0-b8ae-40e2-8228-040256adb7e2
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: f84229908f31242b6d2f7636a7c67ca669de45b3


---

# <a name="step-2-prepare-the-first-priv-domain-controller"></a>Paso 2: preparar el controlador de dominio PRIV

>[!div class="step-by-step"]
[« Paso 1](step-1-prepare-corp-domain.md)
[Paso 3 »](step-3-prepare-pam-server.md)

En este paso se creará un nuevo dominio que proporcionará el entorno bastión para la autenticación de administrador.  Este bosque necesitará al menos un controlador de dominio y como mínimo un servidor miembro. El servidor miembro se configurará en el paso siguiente.

## <a name="create-a-new-privileged-access-management-domain-controller"></a>Creación de un nuevo controlador de dominio de Privileged Access Management

En esta sección se configurará una máquina virtual para que actúe como controlador de dominio de un bosque nuevo

### <a name="install-windows-server-2012-r2"></a>Instalación de Windows Server 2012 R2
En otra máquina virtual nueva sin software instalado, instale Windows Server 2012 R2 para crear un equipo "PRIVDC".

1. Seleccione la opción de instalación personalizada (y no la de actualización) de Windows Server. Cuando realice la instalación, especifique **Windows Server 2012 R2 Standard (servidor con una GUI) x64**; _no seleccione_ **Data Center ni Server Core**.

2. Revise los términos de licencia y acéptelos.

3. Como el disco estará vacío, seleccione **Personalizada: instalar solo Windows** y use el espacio en disco sin inicializar.

4. Después de instalar la versión del sistema operativo, inicie sesión en este nuevo equipo como nuevo administrador. Use el Panel de control para establecer el nombre del equipo en *PRIVDC*, asignarle una dirección IP estática en la red virtual y configurar el servidor DNS para que sea el del controlador de dominio instalado en el paso anterior. Esto requerirá el reinicio del servidor.

5. Una vez reiniciado el servidor, inicie sesión como administrador. Mediante el Panel de control, configure el equipo para que compruebe si hay actualizaciones y para que instale todas las actualizaciones necesarias. Esto podría precisar el reinicio del servidor.

### <a name="add-roles"></a>Incorporación de roles
Agregue los roles Servicios de dominio de Active Directory (AD DS) y Servidor DNS.

1. Inicie PowerShell como administrador.

2. Escriba los comandos siguientes para preparar una instalación de Windows Server Active Directory.

  ```
  import-module ServerManager

  Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools
  ```

### <a name="configure-registry-settings-for-sid-history-migration"></a>Configuración de los valores del Registro para la migración del historial de SID

Inicie PowerShell y escriba el siguiente comando para configurar el dominio de origen de modo que permita el acceso de llamada a procedimiento remoto (RPC) a la base de datos del administrador de cuentas de seguridad (SAM).

```
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

## <a name="create-a-new-privileged-access-management-forest"></a>Creación de un nuevo bosque de Privileged Access Management

A continuación, promueva el servidor a controlador de dominio de un bosque nuevo.

En este documento, el nombre priv.contoso.local se usa como nombre de dominio del nuevo bosque.  El nombre del bosque no es crítico y no es necesario que esté subordinado a un nombre de bosque existente en la organización. Sin embargo, los nombres de NetBIOS y de dominio del bosque nuevo deben ser únicos y diferentes a los de cualquier otro dominio existente en la organización.  

### <a name="create-a-domain-and-forest"></a>Creación de un dominio y un bosque

1. En una ventana de PowerShell, escriba los siguientes comandos para crear el nuevo dominio.  Así también se creará una delegación DNS en un dominio superior (contoso.local) que se creó en un paso anterior.  Si piensa configurar DNS más tarde, omita los parámetros `CreateDNSDelegation -DNSDelegationCredential $ca`.

  ```
  $ca= get-credential

  Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca
  ```

2. Cuando aparezca la ventana emergente, proporcione las credenciales de administrador del bosque CORP (p. ej., el nombre de usuario CONTOSO\\Administrador y la contraseña correspondiente del paso 1).

3. La ventana de PowerShell le solicitará una contraseña de administrador de modo seguro. Escriba una contraseña nueva dos veces. Aparecerán mensajes de advertencia de delegación DNS y configuración de criptografía. Estos mensajes son normales.

Cuando se haya terminado de crear el bosque, el servidor se reiniciará automáticamente.

### <a name="create-user-and-service-accounts"></a>Creación de cuentas de usuario y servicio
Cree las cuentas de usuario y servicio para la instalación del servicio y el portal MIM. Estas cuentas se ubicarán en el contenedor Usuarios del dominio priv.contoso.local.

1. Después del reinicio del servidor, inicie sesión en PRIVDC como administrador del dominio (PRIV\\Administrador).

2. Inicie PowerShell y escriba los siguientes comandos. La contraseña 'Pass@word1' es solo un ejemplo, así que se debe usar otra contraseña para las cuentas.

  ```
  import-module activedirectory

  $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  New-ADUser –SamAccountName MIMMA –name MIMMA

  Set-ADAccountPassword –identity MIMMA –NewPassword $sp

  Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor

  Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp

  Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent

  Set-ADAccountPassword –identity MIMComponent –NewPassword $sp

  Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMSync –name MIMSync

  Set-ADAccountPassword –identity MIMSync –NewPassword $sp

  Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMService –name MIMService

  Set-ADAccountPassword –identity MIMService –NewPassword $sp

  Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SharePoint –name SharePoint

  Set-ADAccountPassword –identity SharePoint –NewPassword $sp

  Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SqlServer –name SqlServer

  Set-ADAccountPassword –identity SqlServer –NewPassword $sp

  Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName BackupAdmin –name BackupAdmin

  Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp

  Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1

  New-ADUser -SamAccountName MIMAdmin -name MIMAdmin

  Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp

  Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1

  Add-ADGroupMember "Domain Admins" SharePoint

  Add-ADGroupMember "Domain Admins" MIMService
  ```

### <a name="configure-auditing-and-logon-rights"></a>Configuración de los derechos de auditoría e inicio de sesión

Debe configurar la auditoría para que la configuración PAM se establezca en los bosques.  

1. Asegúrese de que ha iniciado sesión como administrador del dominio (PRIV\\Administrador).

2. Vaya a **Inicio** > **Herramientas administrativas** > **Administración de directivas de grupo**.

3. Vaya a **Bosque: priv.contoso.local** > **Dominios** > **priv.contoso.local** > **Controladores de dominio** > **Directiva predeterminada de controladores de dominio**. Aparecerá un mensaje de advertencia.

4. Haga clic con el botón derecho en **Directiva predeterminada de controladores de dominio** y seleccione **Editar**.

5. En el árbol de la consola Editor de administración de directivas de grupo, vaya a **Configuración del equipo** > **Directivas** > **Configuración de Windows** > **Configuración de seguridad** > **Directivas locales** > **Directiva de auditoría**.

6. En el panel Detalles, haga clic con el botón derecho en **Auditar la administración de cuentas** y seleccione **Propiedades**. Haga clic en **Definir esta configuración de directiva**, marque la casilla **Éxito** y la casilla **Error** , haga clic en **Aplicar** y, luego, en **Aceptar**.

7. En el panel Detalles, haga clic con el botón derecho en **Auditar el acceso del servicio de directorio** y seleccione **Propiedades**. Haga clic en **Definir esta configuración de directiva**, marque la casilla **Éxito** y la casilla **Error** , haga clic en **Aplicar** y, luego, en **Aceptar**.

8. Vaya a **Configuración del equipo** > **Directivas** > **Configuración de Windows** > **Configuración de seguridad** > **Directivas de cuenta** > **Directiva Kerberos**.

9. En el panel Detalles, haga clic con el botón derecho en **Vigencia máxima del vale de usuario** y seleccione **Propiedades**. Haga clic en **Definir esta configuración de directiva**, establezca la cantidad de horas en *1*, haga clic en **Aplicar** y, luego, en **Aceptar**. Tenga en cuenta que las otras definiciones de la ventana también cambiarán.

10. En la ventana Administración de directivas de grupo, seleccione **Directiva predeterminada de dominio**, haga clic con el botón derecho y seleccione **Editar**.

11. Expanda **Configuración del equipo** > **Directivas** > **Configuración de Windows** > **Configuración de seguridad** > **Directivas locales** y seleccione **Asignación de derechos de usuario**.

12. En el panel Detalles, haga clic con el botón derecho en **Denegar el inicio de sesión como trabajo por lotes** y seleccione **Propiedades**.

13. Active la casilla **Definir esta configuración de directiva**, haga clic en **Agregar usuario o grupo** y, en el campo Nombres de usuario y grupo, escriba *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* y haga clic en **Aceptar**.

14. Haga clic en **Aceptar** para cerrar la ventana.

15. En el panel Detalles, haga clic con el botón derecho en **Denegar inicio de sesión a través de Servicios de Escritorio remoto** y seleccione **Propiedades**.

16. Haga clic en **Definir esta configuración de directiva**, **Agregar usuario o grupo** y, en el campo Nombres de usuario y grupo, escriba *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* y haga clic en **Aceptar**.

17. Haga clic en **Aceptar** para cerrar la ventana.

18. Cierre las ventanas Editor de administración de directivas de grupo y Administración de directivas de grupo.

19. Inicie una ventana de PowerShell como administrador y escriba el siguiente comando para actualizar el controlador de dominio a partir de la configuración de directiva de grupo.

  ```
  gpupdate /force /target:computer
  ```

  Transcurrido un minuto, se completará y se mostrará el mensaje "La actualización de la directiva de equipo se completó correctamente".


### <a name="configure-dns-name-forwarding-on-privdc"></a>Configuración del reenvío de nombres DNS

Cuando use PowerShell en PRIVDC, configure el reenvío de nombres DNS para que el dominio PRIV reconozca otros bosques existentes.

1. Inicie PowerShell.

2. Para cada dominio de la parte superior de cada bosque existente, escriba el siguiente comando, especificando el dominio DNS existente (por ejemplo, contoso.local) y la dirección IP del servidor maestro de ese dominio.  

  Si creó un dominio contoso.local en el paso anterior, especifique *10.1.1.31* como dirección IP de red virtual del equipo CORPDC.

  ```
  Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31
  ```

> [!NOTE]
> Los otros bosques también deben ser capaces de enrutar consultas DNS del bosque PRIV a este controlador de dominio.  Si tiene varios bosques de Active Directory existentes, también debe agregar un reenviador condicional DNS a cada uno de esos bosques.

### <a name="configure-kerberos"></a>Configuración de Kerberos

1. Cuando use PowerShell, agregue SPN para que SharePoint, la API de REST de PAM y el servicio MIM puedan usar autenticación Kerberos.

  ```
  setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint
  setspn -S http/pamsrv PRIV\SharePoint
  setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService
  setspn -S FIMService/pamsrv PRIV\MIMService
  ```

> [!NOTE]
> En los pasos siguientes de este documento se explica cómo instalar componentes de servidor de MIM 2016 en un único equipo. Si planea agregar otro servidor para lograr una alta disponibilidad, necesitará configuración adicional de Kerberos, como se describe en [FIM 2010: Kerberos Authentication Setup (FIM 2010: configuración de autenticación de Kerberos)](http://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx).

### <a name="configure-delegation-to-give-mim-service-accounts-access"></a>Configuración de la delegación para conceder acceso a las cuentas de servicio de MIM

Realice los pasos siguientes en PRIVDC como administrador de dominio.

1. Inicie **Usuarios y equipos de Active Directory**.  
2. Haga clic con el botón derecho en el dominio **priv.contoso.local** y seleccione **Delegar control**.  
3. En la pestaña Usuarios y grupos seleccionados, haga clic en **Agregar**.  
4. En la ventana Seleccionar usuarios, equipos o grupos, escriba *mimcomponent; mimmonitor; mimservice* y haga clic en **Comprobar nombres**. Una vez que se hayan subrayado los nombres, haga clic en **Aceptar** y después en **Siguiente**.  
5. En la lista de tareas comunes, seleccione **Crear, eliminar y administrar cuentas de usuario** y **Modificar la pertenencia de un grupo**; luego, haga clic en **Siguiente** y en **Finalizar**.

6. De nuevo, haga clic con el botón derecho en el dominio **priv.contoso.local** y seleccione **Delegar control**.  
7. En la pestaña Usuarios y grupos seleccionados, haga clic en **Agregar**.  
8. En la ventana Seleccionar usuarios, equipos o grupos, escriba *MIMAdmin* y haga clic en **Comprobar nombres**. Una vez que se hayan subrayado los nombres, haga clic en **Aceptar** y después en **Siguiente**.  
9. Seleccione **Tarea personalizada**, aplique a **Esta carpeta**con **Permisos generales**.    
10. En la lista de permisos, seleccione lo siguiente:  
  - **Lectura**  
  - **Escritura**  
  - **Crear todos los objetos secundarios**  
  - **Eliminar todos los objetos secundarios**  
  - **Leer todas las propiedades**  
  - **Escribir todas las propiedades**  
  - **Migrar historial de Id. de seguridad**  
  Haga clic en **Siguiente** y, luego, en **Finalizar**.

11. Una vez más, haga clic con el botón derecho en el dominio **priv.contoso.local** y seleccione **Delegar control**.  
12. En la pestaña Usuarios y grupos seleccionados, haga clic en **Agregar**.  
13. En la ventana Seleccionar usuarios, equipos o grupos, escriba *MIMAdmin* y luego haga clic en **Comprobar nombres**. Una vez que se hayan subrayado los nombres, haga clic en **Aceptar**y después en **Siguiente**.  
14. Seleccione **Tarea personalizada**, aplique a **Esta carpeta**y, luego, haga clic en **solo objetos de usuario**.    
15. En la lista de permisos, seleccione **Cambiar contraseña** y **Restablecer contraseña**. Luego, haga clic en **Siguiente** y en **Finalizar**.  
16. Cierre Usuarios y equipos de Active Directory.

17. Abra un símbolo del sistema.  
18. Revise la lista de control de acceso en el objeto contenedor Admin SD de los dominios PRIV. Por ejemplo, si el dominio es "priv.contoso.local", escriba el comando  
  ```
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local"
  ```
19. Actualice la lista de control de acceso según sea necesario para garantizar que el servicio MIM y el servicio de componentes MIM puedan actualizar las pertenencias a grupos protegidos por esta ACL.  Escriba el comando:  
  ```
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimservice:WP;"member"  
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimcomponent:WP;"member"
  ```
20. Reinicie el servidor PRIVDC para que se apliquen los cambios.

## <a name="prepare-a-priv-workstation"></a>Preparación de una estación de trabajo PRIV

Si aún no dispone de un equipo de estación de trabajo que se vaya a unir al dominio PRIV para realizar el mantenimiento de los recursos PRIV (como MIM), siga estas instrucciones para preparar una estación de trabajo.  

### <a name="install-windows-81-or-windows-10-enterprise"></a>Instalación de Windows 8.1 o Windows 10 Enterprise

En otra máquina virtual nueva sin software instalado, instale Windows 8.1 Enterprise o Windows 10 Enterprise para crear un equipo *"PRIVWKSTN"*.

1. Use la configuración rápida durante la instalación.

2. Tenga en cuenta que es posible que la instalación no pueda conectarse a Internet. Haga clic en **Crear una cuenta local**. Especifique otro nombre de usuario; no utilice "Administrador" o "Jen".

3. Mediante el Panel de control, dé a este equipo una dirección IP estática en la red virtual y configure el servidor DNS preferido de la interfaz para que sea el del servidor PRIVDC.

4. Mediante el Panel de control, una el equipo PRIVWKSTN al dominio priv.contoso.local. Será necesario proporcionar credenciales de administrador del dominio PRIV. Cuando se complete, reinicie el equipo PRIVWKSTN.

Si quiere obtener más detalles, consulte [Securing privileged access workstations (Proteger estaciones de trabajo de acceso con privilegios)](https://technet.microsoft.com/en-us/library/mt634654.aspx).

En el siguiente paso se preparará un servidor PAM.

>[!div class="step-by-step"]
[« Paso 1](step-1-prepare-corp-domain.md)
[Paso 3 »](step-3-prepare-pam-server.md)



<!--HONumber=Nov16_HO2-->


