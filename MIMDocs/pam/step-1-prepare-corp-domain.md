---
title: "Implementación de PAM, paso 1: dominio CORP | Microsoft Docs"
description: Prepare el dominio CORP con identidades nuevas o existentes para ser administrado por Privileged Identity Manager.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 1164e7efb70d911497b08248b68f8d929bc6d3fb
ms.contentlocale: es-es
ms.lasthandoff: 05/02/2017


---

# <a name="step-1---prepare-the-host-and-the-corp-domain"></a>Paso 1: preparar el host y el dominio CORP

>[!div class="step-by-step"]
[Paso 2 »](step-2-prepare-priv-domain-controller.md)


En este paso se realizará la preparación para hospedar el entorno bastión. En caso necesario, también se creará un controlador de dominio y una estación de trabajo miembro en un dominio y un bosque (el bosque *CORP*) nuevos con identidades que administrará el entorno bastión. Este bosque CORP simula un bosque existente con recursos que se van a administrar. Este documento incluye un recurso de ejemplo que se va a proteger, un recurso compartido de archivos.

Si ya tiene un dominio de Active Directory (AD) existente con un controlador de dominio con Windows Server 2012 R2 o posterior en el que es administrador de dominio, puede usar ese dominio en su lugar.  

## <a name="prepare-the-corp-domain-controller"></a>Preparación del controlador de dominio CORP

En esta sección se explica cómo configurar un controlador de dominio para un dominio CORP. En el dominio CORP, el entorno bastión administra los usuarios administrativos. El nombre DNS (Sistema de nombres de dominio) del dominio CORP usado en este ejemplo es *contoso.local*.

### <a name="install-windows-server"></a>Instalación de Windows Server

Instale Windows Server 2012 R2 o Windows Server 2016 Technical Preview 4 o posterior en una máquina virtual para crear un equipo denominado *CORPDC*.

1. Seleccione **Windows Server 2012 R2 Standard (servidor con una GUI) x64** o **Windows Server 2016 Technical Preview (servidor con experiencia de escritorio)**.

2. Revise los términos de licencia y acéptelos.

3. Como el disco estará vacío, seleccione **Personalizada: instalar solo Windows** y use el espacio en disco sin inicializar.

4. Inicie sesión en ese nuevo equipo como su administrador. Vaya al Panel de control. Establezca el nombre del equipo en *CORPDC* y asígnele una dirección IP estática en la red virtual. Reinicie el servidor.

5. Una vez reiniciado el servidor, inicie sesión como administrador. Vaya al Panel de control. Configure el equipo para que compruebe si hay actualizaciones e instale todas las necesarias. Reinicie el servidor.

### <a name="add-roles-to-establish-a-domain-controller"></a>Adición de roles para establecer un controlador de dominio

En esta sección se agregarán Servicios de dominio de Active Directory (AD DS), los roles de servidor DNS y servidor de archivos (parte de la sección Servicios de archivos y almacenamiento) y se promoverá este servidor a controlador de dominio del nuevo bosque contoso.local.

> [!NOTE]  
> Si ya cuenta con un dominio para usarlo como dominio CORP y ese dominio usa Windows Server 2012 R2 o posterior como controlador de dominio, puede ir directamente a [Creación de usuarios y grupos adicionales para fines demostrativos](#create-additional-users-and-groups-for-demonstration-purposes).

1. Con la sesión iniciada como administrador, inicie PowerShell.

2. Escriba los siguientes comandos:

  ```
  import-module ServerManager

  Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools

  Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork
  ```

  Se le solicitará una contraseña para el administrador del modo seguro. Tenga en cuenta que aparecerán mensajes de advertencia de delegación de DNS y configuración de criptografía. Estos mensajes son normales.

3. Una vez finalizada la creación del bosque, cierre la sesión. El servidor se reiniciará automáticamente.

4. Una vez reiniciado el servidor, inicie sesión en CORPDC como administrador del dominio. Suele ser el usuario CONTOSO\\Administrador, que tendrá la contraseña que se creó al instalar Windows en CORPDC.

### <a name="create-a-group"></a>Crear un grupo

Si no existe uno, cree un grupo para que Active Directory lo audite. El nombre del grupo debe ser el nombre del dominio NetBIOS seguido por tres signos de dólar, por ejemplo, *CONTOSO$$$*.

Inicie sesión en un controlador de dominio como administrador de dominio y realice los pasos siguientes para cada dominio:

1. Inicie PowerShell.

2. Escriba los comandos siguientes, pero sustituya "CONTOSO" por el nombre NetBIOS del dominio.

  ```
  import-module activedirectory

  New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'
  ```

En algunos casos puede que ya exista el grupo. Esto es normal si el dominio también se usó en escenarios de migración de AD.

### <a name="create-additional-users-and-groups-for-demonstration-purposes"></a>Creación de usuarios y grupos adicionales para fines demostrativos

Si creó un nuevo dominio CORP, entonces debería crear usuarios y grupos adicionales para ilustrar el escenario PAM. El usuario y el grupo para fines demostrativos no deben ser administradores de dominio ni estar controlados por la configuración de adminSDHolder en AD.

> [!NOTE]
> Si ya tiene un dominio que va a usar como dominio CORP y este tiene un usuario y un grupo que puede usar para fines demostrativos, puede ir directamente a la sección [Configuración de la auditoría](#configure-auditing).

Ahora se va a crear un grupo de seguridad denominado *CorpAdmins* y un usuario denominado *Jen*. Puede usar otros nombres si quiere.

1. Inicie PowerShell.

2. Escriba los siguientes comandos: Reemplace la contraseña "Pass@word1" por una cadena de contraseña diferente.

  ```
  import-module activedirectory

  New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins

  New-ADUser –SamAccountName Jen –name Jen

  Add-ADGroupMember –identity CorpAdmins –Members Jen

  $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  Set-ADAccountPassword –identity Jen –NewPassword $jp

  Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"
  ```

### <a name="configure-auditing"></a>Configuración de la auditoría

Para establecer la configuración PAM en los bosques existentes, tiene que habilitar las auditorías en ellos.  

Inicie sesión en un controlador de dominio como administrador de dominio y realice los pasos siguientes para cada dominio:

1. Vaya a **Iniciar** > **Herramientas administrativas** (o bien, en Windows Server 2016, **Herramientas administrativas de Windows**) e inicie **Administración de directivas de grupo**.

2. Vaya a la directiva de controladores de dominio de este dominio.  Si creó un nuevo dominio para contoso.local, vaya a **Bosque: contoso.local** > **Dominios** > **contoso.local** > **Controladores de dominio** > **Directiva predeterminada de controladores de dominio**. Aparece un mensaje informativo.

3. Haga clic con el botón derecho en **Directiva predeterminada de controladores de dominio** y seleccione **Editar**. Aparece una nueva ventana.

4. En la ventana Editor de administración de directivas de grupo, en el árbol Directiva predeterminada de controladores de dominio, vaya a **Configuración del equipo** > **Directivas** > **Configuración de Windows** > **Configuración de seguridad** > **Directivas locales** > **Directiva de auditoría**.

5. En el panel de detalles , haga clic con el botón derecho en **Auditar la administración de cuentas** y seleccione **Propiedades**. Seleccione **Definir esta configuración de directiva**, active **Correcto**, active **Error**, haga clic en **Aplicar** y en **Aceptar**.

6. En el panel de detalles, haga clic con el botón derecho en **Auditar el acceso del servicio de directorio** y seleccione **Propiedades**. Seleccione **Definir esta configuración de directiva**, active **Correcto**, **Error**, haga clic en **Aplicar** y en **Aceptar**.

7. Cierre las ventanas Editor de administración de directivas de grupo y Administración de directivas de grupo.

8. Aplique la configuración de auditoría al iniciar una ventana de PowerShell y escribir:

  ```
  gpupdate /force /target:computer
  ```

Transcurridos unos minutos, debería aparecer el mensaje **La actualización de la directiva de equipo se completó correctamente**.

### <a name="configure-registry-settings"></a>Configuración del registro

En esta sección se configurarán los valores del Registro necesarios para la migración del historial de SID, que se usará para la creación del grupo de Privileged Access Management.

1. Inicie PowerShell.

2. Escriba los siguientes comandos para configurar el dominio de origen para permitir el acceso de la llamada a procedimiento remoto (RPC) a la base de datos del administrador de cuentas de seguridad (SAM).

  ```
  New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1

  Restart-Computer
  ```

Con esto se reiniciará el controlador de dominio, CORPDC. Para obtener más información sobre la configuración del Registro, consulte [Cómo solucionar problemas de la migración de sIDHistory entre bosques con ADMTv2](http://support.microsoft.com/kb/322970).

## <a name="prepare-a-corp-workstation-and-resource"></a>Preparación de una estación de trabajo y un recurso CORP

Si aún no dispone de un equipo de estación de trabajo unido al dominio, siga estas instrucciones para preparar uno.  

> [!NOTE]
> Si ya dispone de una estación de trabajo unida al dominio, vaya a [Creación de un recurso para fines demostrativos](#create-a-resource-for-demonstration-purposes).

### <a name="install-windows-81-or-windows-10-enterprise-as-a-vm"></a>Instalación de Windows 8.1 o Windows 10 Enterprise como una máquina virtual

En otra máquina virtual nueva sin ningún software instalado, instale Windows 8.1 Enterprise o Windows 10 Enterprise para crear un equipo *CORPWKSTN*.

1. Use la configuración rápida durante la instalación.

2. Tenga en cuenta que es posible que la instalación no pueda conectarse a Internet. Seleccione **Crear una cuenta local**. Especifique otro nombre de usuario; no utilice "Administrador" o "Jen".

3. Mediante el Panel de control, dé a este equipo una dirección IP estática en la red virtual y configure el servidor DNS preferido de la interfaz para que sea el del servidor CORPDC.

4. Mediante el Panel de control, una el equipo CORPWKSTN al dominio contoso.local. Tendrá que proporcionar las credenciales de administrador del dominio Contoso. Cuando se complete, reinicie el equipo CORPWKSTN.

### <a name="create-a-resource-for-demonstration-purposes"></a>Creación de un recurso para fines demostrativos

Se necesita un recurso para ilustrar el control de acceso basado en grupos de seguridad con PAM.  Si aún no dispone de uno, puede usar una carpeta de archivos con fines demostrativos.  Esta usará los objetos de AD "Jen" y "CorpAdmins" que creó en el dominio contoso.local.

1. Conéctese a la estación de trabajo CORPWKSTN. Haga clic en el icono **Cambiar de usuario** y luego en **Otro usuario**. Asegúrese de que el usuario CONTOSO\\Jen pueda iniciar sesión en CORPWKSTN.

2. Cree una carpeta nueva denominada *CorpFS* y compártala con el grupo *CorpAdmins*.

3. Abra PowerShell como administrador.

4. Escriba los siguientes comandos:

  ```
  mkdir c:\corpfs

  New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins

  $acl = Get-Acl c:\corpfs

  $car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")

  $acl.SetAccessRule($car)

  Set-Acl c:\corpfs $acl
  ```

En el siguiente paso, se preparará el controlador de dominio PRIV.

>[!div class="step-by-step"]
[Paso 2 »](step-2-prepare-priv-domain-controller.md)

