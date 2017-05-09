---
title: "Implementación de la aplicación de Windows del Administrador de certificados de MIM | Microsoft Docs"
description: "Descubra cómo implementar la aplicación Certificate Manager para permitir a los usuarios administrar sus propios derechos de acceso."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 66060045-d0be-4874-914b-5926fd924ede
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: d714a58796d3a86fc82ed1eb6dc29bdc45920933
ms.lasthandoff: 01/24/2017


---

# <a name="working-with-the-mim-certificate-manager"></a>Trabajar con MIM Certificate Manager
Una vez que estén en funcionamiento MIM 2016 y Certificate Manager, puede implementar la aplicación de la Tienda Windows MIM Certificate Manager para que los usuarios puedan administrar fácilmente sus certificados de software, tarjetas inteligentes físicas y tarjetas inteligentes virtuales. Los pasos para implementar la aplicación MIM CM son los siguientes:

1.  Cree una plantilla de certificados.

2.  Cree una plantilla de perfil.

3.  Prepare la aplicación.

4.  Implemente la aplicación mediante SCCM o Intune.

## <a name="create-a-certificate-template"></a>Crear una plantilla de certificado
Una plantilla de certificado para la aplicación CM se crea como de costumbre, salvo que es necesario asegurarse de que la versión de la plantilla de certificados sea la versión 3 o una versión posterior.

1.  Inicie sesión en el servidor que ejecuta AD CS (el servidor de certificados).

2.  Abra MMC.

3.  Haga clic en **Archivo &gt; Agregar o quitar complemento**.

4.  En la lista Complementos disponibles, haga clic en **Plantillas de certificado** y después en **Agregar**.

5.  Ahora verá **Plantillas de certificado** bajo **Raíz de consola** en MMC. Haga doble clic sobre esa opción para ver todas las plantillas de certificado disponibles.

6.  Haga clic con el botón derecho en la plantilla **Inicio de sesión de tarjeta inteligente** y en **Plantilla duplicada**.

7.  En la pestaña Compatibilidad, bajo Entidad de certificación, seleccione Windows Server 2008 y bajo Destinatario del certificado, seleccione Windows 8.1/Windows Server 2012 R2.
    Este paso es fundamental, ya que se asegura de que la versión de la plantilla de certificado sea la versión 3 (o posterior), puesto que la versión 3 es la única que funciona con la aplicación del administrador de certificados. Dado que la versión se establece cuando se crea y guarda por primera vez la plantilla de certificado, si no la creó de este modo, no hay forma alguna de cambiarla por la versión correcta y deberá crear una nueva antes de continuar.

8.  En la pestaña **General** , escriba en el campo **Nombre para mostrar** el nombre que quiere que aparezca en la interfaz de usuario de la aplicación, como **Inicio de sesión de tarjeta inteligente virtual**.

9. En la pestaña **Tratamiento de la solicitud**, establezca el **Propósito** en **Firma y cifrado**; en **Hacer lo siguiente**, seleccione **Preguntar al usuario durante la inscripción**.

10. En la pestaña **Criptografía** , bajo **Categoría del proveedor**, seleccione **El proveedor de almacenamiento de claves y las solicitudes pueden usar cualquier proveedor disponible en el equipo del firmante**.

    > [!NOTE]
    > Solo verá Proveedor de almacenamiento de claves como una opción si la versión de la plantilla es la versión 3. Si no ve dicha opción, probablemente se deba a que no creó correctamente la plantilla de certificado con la versión adecuada. Vuelva a empezar desde el paso 5, expuesto anteriormente.

11. En la pestaña **Seguridad** , agregue el grupo de seguridad al que quiera proporcionar acceso para **Inscribir** . Por ejemplo, si quiere proporcionar acceso a todos los usuarios, seleccione el grupo **Usuarios autenticados** y después seleccione **Permisos de inscripción** para ellos.

12. Haga clic en **Aceptar** para dar por finalizados los cambios y crear la plantilla nueva. Debería poder ver la nueva plantilla en la lista Plantillas de certificado.

13. Seleccione **Archivo** y haga clic en **Agregar o quitar complemento** para agregar el complemento Entidad de certificación a la consola MMC. Cuando se le pregunte qué equipo quiere administrar, seleccione **Equipo local**.

14. En el panel izquierdo de MMC, expanda **Entidad de certificación (local)** y después expanda su Entidad de certificación en la lista Entidad de certificación.

15. Haga clic con el botón derecho en **Plantillas de certificado** y luego haga clic en **Nueva &gt; Plantilla de certificado que se va a emitir**.

16. Seleccione en la lista la nueva plantilla que creó y haga clic en **Aceptar**.

## <a name="create-a-profile-template"></a>Crear una plantilla de perfil
Asegúrese de que al crear una plantilla de perfil, la establezca para crear/destruir la VSC y quitar la recolección de datos. La aplicación CM no puede controlar los datos recopilados, por lo que es importante deshabilitarla, tal como se indica a continuación.

1.  Inicie sesión en el portal de CM como usuario con privilegios administrativos.

2.  Vaya a Administración &gt; Administrar plantillas de perfil y asegúrese de que la casilla situada junto a Plantilla del perfil de inicio de sesión de la tarjeta inteligente de ejemplo de MIM CM esté activada. A continuación haga clic en Copiar una plantilla de perfil seleccionada.

3.  Escriba el nombre de la plantilla de perfil y haga clic en **Aceptar**.

4.  En la siguiente pantalla, haga clic en **Agregar una nueva plantilla de certificado** y asegúrese de marcar la casilla situada junto al nombre de la entidad de certificación.

5.  Marque la casilla situada junto al nombre de la plantilla de perfil **Inicio de sesión** y haga clic en **Agregar**.

6.  Quite la plantilla InicioSesiónTarjetaInteligente marcando la casilla situada junto a ella y haciendo clic después en **Eliminar plantillas de certificado seleccionadas** y en **Aceptar**.

7.  Desplácese hasta la parte inferior y haga clic en **Cambiar configuración**.

8.  Active las casillas situadas junto a **Crear/destruir tarjeta inteligente virtual** y **Diversificar clave de administración**.

9. En **Directiva de PIN de usuario** seleccione **Proporcionado por el usuario**.

10. En el panel izquierdo, haga clic en **Directiva de renovación &gt; Cambiar configuración general**. Seleccione **Volver a usar la tarjeta al renovar** y haga clic en **Aceptar**.

11. Tiene que deshabilitar elementos de recolección de datos de cada una de las directivas haciendo clic en la directiva en el panel izquierdo y marcando después la casilla situada junto a **Elemento de datos de ejemplo** y después en **Eliminar elementos de recopilación de datos**. A continuación, haga clic en **Aceptar**.

## <a name="prepare-the-cm-app-for-deployment"></a>Preparar la aplicación CM para la implementación

1.  Ejecute en el símbolo del sistema el siguiente comando para desempaquetar la aplicación y extraer su contenido en una nueva subcarpeta denominada appx y crear una copia para que no modifique el archivo original.

    ```
    makeappx unpack /l /p <app package name>.appx /d ./appx
    ren <app package name>.appx <app package name>.appx.original
    cd appx
    ```

2.  En la carpeta appx, cambie el nombre del archivo llamado CustomDataExample.xml por Custom.data.

3.  Abra el archivo Custom.data y modifique los parámetros en la medida que sea necesario.

    |||
    |-|-|
    |MIMCM URL|Nombre de dominio completo (FQDN) del portal que usó para configurar CM. Por ejemplo, https://mimcmServerAddress/certificatemanagement|
    |ADFS URL|Si va a usar AD FS, inserte su URL de AD FS. Por ejemplo, https://adfsServerSame/adfs|
    |PrivacyUrl|Puede incluir una dirección URL a una página web que en la que se explique qué hacer con los detalles de usuario recopilados para la inscripción de certificado.|
    |SupportMail|Puede incluir una dirección de correo electrónico para problemas de soporte técnico.|
    |LobComplianceEnable|Puede establecer este parámetro en true o false. Está establecido en true de manera predeterminada.|
    |MinimumPinLength|Está establecido en 6 de manera predeterminada.|
    |NonAdmin|Puede establecer este parámetro en true o false. Está establecido en false de manera predeterminada. Solo debe cambiar el valor si quiere que puedan inscribir y renovar certificados usuarios que no sean administradores en sus equipos.|

4.  Guarde el archivo y salga del editor.

5.  Al firmar el paquete, se crea un archivo de firma, por lo que debe eliminar el archivo de firma original, denominado AppxSignature.p7x.

6.  El archivo AppxManifest.xml especifica el nombre del firmante del certificado de firma. Abra este archivo para editarlo.

7.  Tiene que obtener un certificado de firma antes de iniciar esta sección. Vea más adelante el paso 1 de la sección Enabling smartcard renewal for non-admins in MIM 2016 Certificate Manager.

8.  En el elemento &lt;Identity&gt;, modifique el valor del atributo Publicador para que sea idéntico al firmante que aparece en el certificado de firma, por ejemplo, "CN=FIRMANTE".

9. Guarde el archivo y salga del editor.

10. En el símbolo del sistema, ejecute el siguiente comando para volver a empaquetar y firmar el archivo .appx.

    ```
    cd ..
    makeappx pack /l /d .\appx /p <app package name>.appx
    ```
    Donde app package name es el mismo nombre que usó al crear la copia.

    ```
    signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.ap
    px
    ```
    Esto proporciona el nuevo archivo .appx. El archivo pfx proporciona una ubicación para el certificado de firma y una contraseña para el archivo .pfx.

11. Para trabajar con la autenticación de AD FS:

    -   Abra la aplicación Tarjeta inteligente Virtual. De este modo podrá encontrar más fácilmente los valores necesarios para el siguiente paso.

    -   Para agregar la aplicación como cliente al servidor de AD FS y configurar CM en el servidor en el servidor de AD FS, abra Windows PowerShell y ejecute el comando `ConfigureMimCMClientAndRelyingParty.ps1 –redirectUri <redirectUriString> -serverFQDN <MimCmServerFQDN>`.

        El siguiente script es el script ConfigureMIimCMClientAndRelyingParty.ps1:

        ```
        # HELP

        <#
        .SYNOPSIS
                        Configure ADFS for CM client app and server.
        .DESCRIPTION
           What the Script does:
                                        a. Registers the MIM CM client app on the ADFS server.
                                        b. Registers the MIM CM server relying party (Tells the ADFS server that it issues tokens for the CM server).
                        For parameter information, see 'get-help -detailed'
        .PARAMETER redirectUri
                        The redirectUri for CM client app. Will be added to ADFS server.
                        It can be found as follows:
                        1. Open the settings panel. Under settings, there is a "Redirect Uri" text box (an ADFS server address must be configured in order for the text to display).
                        2. Open MIM CM client app. Navigate to 'C:\Users\<your_username>\AppData\Local\Packages\CmModernAppv.<version>\LocalState', and open 'Logs_Virtual Smart Card Certificate Manager_<version>'. Search for "Redirect URI".
        .PARAMETER serverFqdn
                        Your deployed MIM CM server’s FQDN.
        .EXAMPLE
           .\ConfigureMimCMClientAndRelyingParty.ps1 -redirectUri ms-app://s-1-15-2-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789/ -serverFqdn WIN-TRUR24L4CFS.corp.cmteam.com
        #>

        # Parameter declaration
        [CmdletBinding()]
        Param(
          [Parameter(Mandatory=$True)]
           [string]$redirectUri,

           [Parameter(Mandatory=$True)]
           [string]$serverFqdn
        )

        Write-Host "Configuring ADFS Objects for OAuth.."

        #Configure SSO to get persistent sign on cookie
        Set-ADFSProperties -SsoLifetime 2880

        #Configure Authentication Policy
        #Intranet to use Kerberos
        #Extranet to use U/P

        #Create Client Objects

        Write-Host "Creating Client Objects..."

        $existingClient = Get-ADFSClient -Name "MIM CM Modern App"

        if ($existingClient -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Modern App, removing"
            Remove-ADFSClient -TargetName "MIM CM Modern App"
            Write-Host "Client object removed"
        }

        Write-Host "Adding Client Object for MIM CM Modern App client"
        Add-ADFSClient -Name "MIM CM Modern App" -ClientId "70A8B8B1-862C-4473-80AB-4E55BAE45B4F" -RedirectUri $redirectUri
        Write-Host "Client Object for MIM CM Modern App client Created"

        #Create Relying Parties
        Write-Host "Creating Relying Party Objects"

        $existingRp = Get-ADFSRelyingPartyTrust -Name "MIM CM Service"
        if ($existingRp -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Service RP, removing"
            Remove-ADFSRelyingPartyTrust -TargetName "MIM CM Service"
            Write-Host "RP object Removed"
        }

        $authorizationRules =
        "@RuleTemplate = `"AllowAllAuthzRule`"
        => issue(Type = `"http://schemas.microsoft.com/authorization/claims/permit`", Value = `"true`");"

        $issuanceRules =
        "@RuleTemplate = `"LdapClaims`"
        @RuleName = `"Emit UPN and common name`"
        c:[Type == `"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`", Issuer == `"AD AUTHORITY`"]
        => issue(store = `"Active Directory`", types =
        (`"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`",
        `"http://schemas.xmlsoap.org/claims/CommonName`"), query =
        `";userPrincipalName,cn;{0}`", param = c.Value);

        @RuleTemplate = `"PassThroughClaims`"
        @RuleName = `"Pass through Windows Account Name`"
        c:[Type ==`"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`"] => issue(claim = c);"

        Write-Host "Creating RP Trust for MIM CM Service"
        Add-ADFSRelyingPartyTrust -Name "MIM CM Service" -Identifier ("https://"+$serverFqdn+"/certificatemanagement") -IssuanceAuthorizationRules $authorizationRules -IssuanceTransformRules $issuanceRules
        Write-Host "RP Trust for MIM CM Service has been created"
        ```

    -   Actualice los valores de redirectUri y serverFQDN.

    -   Para encontrar la redirectUri, en la aplicación Tarjeta inteligente virtual abra el panel de configuración de la aplicación y haga clic en **Configuración**. La URI de redirección debería aparecer bajo la barra de direcciones del servidor de AD FS. La URI solo aparecerá si se ha configurado una dirección de servidor de ADFS.

    -   El serverFQDN solo es el nombre completo de equipo del servidor de MIMCM.

    -   Para obtener ayuda con el script **ConfigureMIimCMClientAndRelyingParty.ps1** , ejecute `get-help  -detailed ConfigureMimCMClientAndRelyingParty.ps1`.

## <a name="deploy-the-app"></a>Implementar la aplicación
Al configurar la aplicación CM, descargue el archivo MIMDMModernApp_&lt;version&gt;_AnyCPU_Test.zip desde el Centro de descarga y extraiga todo su contenido. El archivo .appx es el instalador. Puede implementarla del mismo modo que suele implementar aplicaciones de la Tienda Windows mediante [System Center Configuration Manager](https://technet.microsoft.com/library/dn613840.aspx) o [Intune](https://technet.microsoft.com/library/dn613839.aspx) para transferir localmente la aplicación con el fin de que los usuarios puedan acceder a ella mediante el portal de empresa. De lo contrario, se colocará directamente en sus equipos.

