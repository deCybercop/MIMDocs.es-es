---
title: Implementación del administrador de certificados de Microsoft Identity Manager | Microsoft Docs
description: Instalar el administrador de certificados de Microsoft Identity Manager 2016
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/19/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 25a511dc590b02019c65a688c9b2c8dc821fff50
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290091"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Implementación del administrador de certificados de Microsoft Identity Manager 2016 (MIM CM)

La instalación del administrador de certificados de Microsoft Identity Manager 2016 (MIM CM) implica una serie de pasos. Para simplificar el proceso, lo dividiremos en varias fases. Hay pasos preliminares que se deben realizar antes de los pasos reales de MIM CM. Sin el trabajo preliminar es probable que se produzca un error en la instalación.

En el diagrama siguiente se muestra un ejemplo del tipo de entorno que se puede usar. Los sistemas con números se incluyen en la lista debajo del diagrama y son necesarios para completar correctamente los pasos descritos en este artículo. Por último, se usan servidores Datacenter de Windows 2016:

![Diagrama del entorno](media/mim-cm-deploy/image001.png)

1. CORPDC – Controlador de dominio
2. CORPCM – Servidor de MIM CM
3. CORPCA – Entidad de certificación
4. CORPCMR – Web de API de Rest de MIM CM – Portal de CM para la API de Rest – Se usa posteriormente
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 – Dispositivo unido a un dominio de Windows 10

## <a name="deployment-overview"></a>Información general sobre la implementación

- Instalación del sistema operativo base

    El laboratorio consta de servidores Datacenter de Windows 2016.

    >[!NOTE]
    >Para obtener más detalles sobre las plataformas compatibles con MIM 2016 vea el artículo titulado [Plataformas compatibles con MIM 2016](/microsoft-identity-manager/microsoft-identity-manager-2016-supported-platforms.md).

1. Pasos previos a la implementación

    - [Extensión del esquema](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Creación de cuentas de servicio

    - [Creación de plantillas de certificado](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Configuración de Kerberos

    - Pasos relacionados con la base de datos

        - Requisitos de configuración de SQL

        - Permisos de base de datos

2. Implementación

## <a name="pre-deployment-steps"></a>Pasos previos a la implementación

El asistente de configuración de MIM CM requiere que se proporcione información a lo largo del proceso para que se complete correctamente.

![diagrama](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Extensión del esquema

El proceso de extensión del esquema es sencillo, pero deberá llevarse a cabo con precaución debido a su naturaleza irreversible.

>[!NOTE]
>Este paso requiere que la cuenta que se use tenga derechos de administrador de esquema.

1. Vaya a la ubicación de los medios MIM y luego a la carpeta \\Certificate Management\\x64.

2. Copie la carpeta Schema en CORPDC y, después, vaya hasta ella.

    ![diagrama](media/mim-cm-deploy/image005.png)

3. Ejecute el escenario de bosque único del script resourceForestModifySchema.vbs. Para el escenario de bosque de recursos ejecute los scripts:
   - DomainA: ubicación de los usuarios (userForestModifySchema.vbs)
   - ResourceForestB: ubicación de instalación de CM (resourceForestModifySchema.vbs).

     >[!NOTE]
     >Los cambios de esquema son una operación unidireccional y requieren la reversión de una recuperación de bosque, por lo que debe asegurarse de tener las copias de seguridad necesarias. Para obtener más información sobre los cambios realizados en el esquema mediante esta operación, revise el artículo [Forefront Identity Manager 2010 Certificate Management Schema Changes](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx) (Cambios de esquema de Forefront Identity Manager 2010 Certificate Management).

     ![diagrama](media/mim-cm-deploy/image007.png)

4. Una vez que se ejecute el script debería recibir un mensaje de operación correcta.

    ![Mensaje de operación correcta](media/mim-cm-deploy/image009.png)

Ahora se extiende el esquema de AD para admitir MIM CM.

### <a name="creating-service-accounts-and-groups"></a>Creación de cuentas y grupos de servicio

En la tabla siguiente se resumen las cuentas y los permisos necesarios para MIM CM. Puede permitir que MIM CM cree automáticamente las cuentas siguientes, o bien puede crearlas antes de la instalación. Los nombres reales de las cuentas se pueden cambiar. Si crea las cuentas usted mismo, considere la posibilidad de asignar nombres a las cuentas de usuario de manera que sea fácil hacer coincidir el nombre de la cuenta de usuario con su función.

Usuarios:

![Diagrama](media/mim-cm-deploy/image010.png)

![Diagrama](media/mim-cm-deploy/image012.png)

| **Rol**                   | **Nombre de inicio de sesión del usuario** |
|----------------------------|---------------------|
| Agente MIM CM               | MIMCMAgent          |
| Key Recovery Agent MIM CM  | MIMCMKRAgent        |
| Agente de autorización de MIM CM | MIMCMAuthAgent      |
| Agente del administrador de CA de MIM CM    | MIMCMManagerAgent   |
| Web Pool Agent de MIM CM      | MIMCMWebAgent       |
| Agente de inscripción de MIM CM    | MIMCMEnrollAgent    |
| Servicio de actualización de MIM CM      | MIMCMService        |
| Cuenta de instalación de MIM        | MIMINSTALL          |
| Agente de soporte técnico            | CMHelpdesk1-2       |
| Administrador de CM                 | CMManager1-2        |
| Usuario suscriptor            | CMUser1-2           |

Grupos:

| **Rol**               | **Grupo**         |
|------------------------|-------------------|
| Miembros del departamento de soporte técnico de CM    | MIMCM-Helpdesk    |
| Miembros de administrador de CM     | MIMCM-Managers    |
| Miembros de suscriptores de CM | MIMCM-Subscribers |

PowerShell: cuentas de agente:

```powershell
import-module activedirectory
## Agent accounts used during setup
$cmagents = @{
"MIMCMKRAgent" = "MIM CM Key Recovery Agent"; 
"MIMCMAuthAgent" = "MIM CM Authorization Agent"
"MIMCMManagerAgent" = "MIM CM CA Manager Agent";
"MIMCMWebAgent" = "MIM CM Web Pool Agent";
"MIMCMEnrollAgent" = "MIM CM Enrollment Agent";
"MIMCMService" = "MIM CM Update Service";
"MIMCMAgent" = "MIM CM Agent";
}
##Groups Used for CM Management
$cmgroups = @{
"MIMCM-Managers" = "MIMCM-Managers"
"MIMCM-Helpdesk" = "MIMCM-Helpdesk"
"MIMCM-Subscribers" = "MIMCM-Subscribers" 
}
##Users Used during testlab
$cmusers = @{
"CMManager1" = "CM Manager1"
"CMManager2" = "CM Manager2"
"CMUser1" = "CM User1"
"CMUser2" = "CM User2"
"CMHelpdesk1" = "CM Helpdesk1"
"CMHelpdesk2" = "CM Helpdesk2"
}

## OU Paths
$aoupath = "OU=Service Accounts,DC=contoso,DC=com" ## Location of Agent accounts
$oupath = "OU=CMLAB,DC=contoso,DC=com" ## Location of Users and Groups for CM Lab


#Create Agents – Update UserprincipalName
$cmagents.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -UserPrincipalName ($_.Name + "@contoso.com")  -Path $aoupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}


#Create Users
$cmusers.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -Path $oupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}
```

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Actualizar la directiva local de servidor **CORPCM** para las cuentas de agente 

| **Nombre de inicio de sesión del usuario** | **Descripción y permisos**   |
|------|---------------------|
| MIMCMAgent          | Proporciona los siguientes servicios: </br>- Recupera claves privadas cifradas de la entidad de certificación. </br>- Protege la información del PIN de tarjeta inteligente en la base de datos de FIM CM. </br>- Protege la comunicación entre FIM CM y la entidad de certificación. </br></br> Esta cuenta de usuario requiere la siguiente configuración de control de acceso:</br>-   Derecho de usuario **Permitir inicio de sesión local**.</br>-   Derecho de usuario **Emitir y administrar certificados**. </br>- Permiso de lectura y escritura en la carpeta Temp del sistema en la siguiente ubicación: %WINDIR%\\Temp.</br>- Un certificado de cifrado y firma digital emitido e instalado en el almacén de usuario.
|MIMCMKRAgent        | Recupera las claves privadas archivadas de la entidad de certificación. Esta cuenta de usuario requiere la siguiente configuración de control de acceso:</br> -   Derecho de usuario **Permitir inicio de sesión local**.</br>- Pertenencia al grupo **Administradores** local. </br>- Permiso de inscripción en la plantilla de certificado **KeyRecoveryAgent**. </br>- El certificado de Key Recovery Agent se emite e instala en el almacén de usuario. El certificado debe agregarse a la lista de los agentes de recuperación de claves de la entidad de certificación. </br>- Permiso de lectura y escritura en la carpeta Temp del sistema en la siguiente ubicación: ```%WINDIR%\\Temp.```.                                                                                                                     |
| MIMCMAuthAgent      | Determina los derechos y permisos para usuarios y grupos. Esta cuenta de usuario requiere la siguiente configuración de control de acceso: </br>- Pertenencia al grupo de dominio Acceso compatible con versiones anteriores de Windows 2000. </br> - Se concede el derecho de usuario **Generar auditorías de seguridad**.             |
| MIMCMManagerAgent   | Realiza actividades de administración de entidad de certificación. </br> Este usuario debe tener asignado el permiso Administrar CA.        |
| MIMCMWebAgent       | Proporciona la identidad para el grupo de aplicaciones de IIS. FIM CM se ejecuta dentro de un proceso de interfaz de programación de aplicaciones de Microsoft Win32® que usa las credenciales de este usuario. </br> Esta cuenta de usuario requiere la siguiente configuración de control de acceso:</br> - Pertenencia al grupo **IIS_WPG, windows 2016 = IIS_IUSRS** local. </br>- Pertenencia al grupo **Administradores** local.</br>- Se concede el derecho de usuario **Generar auditorías de seguridad**. </br>- Se concede el derecho de usuario **Actuar como parte del sistema operativo**. </br>- Se concede el derecho de usuario **Reemplazar un símbolo (token) de nivel de proceso**.</br>- Se asigna como la identidad del grupo de aplicaciones de IIS, **CLMAppPool**. </br>- Se concede el permiso de lectura en la clave del Registro **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\CLM\\v1.0\\Server\\WebUser**. </br>- Esta cuenta también debe ser de confianza para la delegación.|
| MIMCMEnrollAgent    | Realiza la inscripción en nombre del usuario. Esta cuenta de usuario requiere la siguiente configuración de control de acceso:</br>- Un certificado de agente de inscripción que se emite e instala en el almacén de usuario.</br>-   Derecho de usuario **Permitir inicio de sesión local**. </br>- Permiso de inscripción en la plantilla de certificado **Agente de inscripción** (o en la plantilla personalizada, si se usa).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Creación de plantillas de certificado para las cuentas de servicio de MIM CM

Tres de las cuentas de servicio que se usan en MIM CM requieren un certificado y el asistente de configuración requiere que se proporcione el nombre de las plantillas de certificado que se deben usar para solicitar certificados para ellas.

Las cuentas de servicio que requieren certificados son las siguientes:

- MIMCMAgent: esta cuenta necesita un certificado de usuario.

- MIMCMEnrollAgent: esta cuenta necesita un certificado de agente de inscripción.

- MIMCMEnrollAgent: esta cuenta necesita un certificado de **Agente de recuperación de claves**.

En AD ya hay plantillas, pero es necesario crear versiones propias para trabajar con MIM CM. Será necesario realizar modificaciones en las plantillas de línea de base originales.

Las tres cuentas anteriores tendrán derechos elevados dentro de la organización y deben usarse con cuidado.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Crear la plantilla de certificado de firma de MIM CM

1. Desde **Herramientas administrativas**, abra **Entidad de certificación**.

2. En la consola **Entidad de certificación**, en el árbol de consola, expanda **Contoso-CorpCA** y después haga clic en **Plantillas de certificado**.

3. Haga clic con el botón secundario en **Plantillas de certificado**y, a continuación, haga clic en **Administrar**.

4. En la **consola Plantillas de certificado**, en el panel de **detalles**, seleccione y haga clic con el botón derecho en **Usuario** y después haga clic en **Plantilla duplicada**.

5. En el cuadro de diálogo **Plantilla duplicada**, seleccione **Windows Server 2003 Enterprise** y, después, haga clic en **Aceptar**.

    ![Mostrar los cambios resultantes](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >MIM CM no funciona con certificados basados en plantillas de certificado de la versión 3. Debe crear una plantilla de certificado de Windows Server® 2003 Enterprise (versión 2). Vea los [detalles de V3](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) para obtener más información.

6. En el cuadro de diálogo **Propiedades de plantilla nueva**, en el cuadro **Nombre para mostrar de plantilla** de la pestaña **General**, escriba **Firma de MIM CM**. Cambie el **Período de validez** a **2 años** y, después, desactive la casilla **Publicar certificado en Active Directory**.

7. En la pestaña **Tratamiento de la solicitud**, asegúrese de que la casilla **Permitir que la clave privada se pueda exportar** está activada y, después, haga clic en la **pestaña Criptografía**.

8. En el cuadro de diálogo **Selección de criptografía**, deshabilite **Proveedor de cifrado mejorado de Microsoft v1.0**, habilite **Proveedor de servicios de cifrado RSA y AES mejorado de Microsoft** y después haga clic en **Aceptar**.

9. En la pestaña **Nombre del sujeto**, desactive las casillas **Incluir el nombre de correo electrónico en el nombre del sujeto** y **Nombre de correo electrónico**.

10. En la pestaña **Extensiones**, en la lista **Extensiones incluidas en esta plantilla**, asegúrese de que la opción **Directivas de aplicación** está seleccionada y, después, haga clic en **Editar**.

11. En el cuadro de diálogo **Editar extensión de directivas de aplicación**, seleccione las directivas de aplicación **Sistema de cifrado de archivos** y **Correo seguro**. Haga clic en **Quitar**y, a continuación, haga clic en **Aceptar**.

12. En la pestaña **Seguridad**, siga estos pasos:

    - Quite **Administrador**.

    - Quite **Admins. del dominio**.

    - Quite **Usuarios del dominio**.

    - Asigne permisos de solo **Lectura** y **Escritura** a **Administradores de empresa**.

    - Agregue **MIMCMAgent**.

    - Asigne los permisos **Lectura** e **Inscripción** a **MIMCMAgent**.

13. En el cuadro de diálogo **Propiedades de plantilla nueva**, haga clic en **Aceptar**.

14. Deje abierta la **Consola de plantillas de certificado**.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Crear la plantilla de certificado de agente de inscripción de MIM CM

1. En la **consola Plantillas de certificado**, en el panel de **detalles**, seleccione y haga clic con el botón derecho en **Agente de inscripción** y después haga clic en **Plantilla duplicada**.

2. En el cuadro de diálogo **Plantilla duplicada**, seleccione **Windows Server 2003 Enterprise** y, después, haga clic en **Aceptar**.

3. En el cuadro de diálogo **Propiedades de plantilla nueva**, en el cuadro **Nombre para mostrar de plantilla** de la pestaña **General**, escriba **Agente de inscripción de MIM CM**. Asegúrese de que el **Período de validez** es **2 años**.

4. En la pestaña **Tratamiento de la solicitud**, active la casilla **Permitir que la clave privada se pueda exportar** y, después, haga clic en la **pestaña CSP o criptografía**.

5. En el cuadro de diálogo **Selección de CSP**, deshabilite **Microsoft Base Cryptographic Provider v1.0**, deshabilite **Proveedor de cifrado mejorado de Microsoft v1.0**, habilite **Proveedor de servicios de cifrado RSA y AES mejorado de Microsoft**, y después haga clic en **Aceptar**.

6. En la pestaña **Seguridad**, siga estos pasos:

    - Quite **Administrador**.

    - Quite **Admins. del dominio**.

    - Asigne permisos de solo **Lectura** y **Escritura** a **Administradores de empresa**.

    - Agregue **MIMCMEnrollAgent**.

    - Asigne los permisos **Lectura** e **Inscripción** a **MIMCMEnrollAgent**.

7. En el cuadro de diálogo **Propiedades de plantilla nueva**, haga clic en **Aceptar**.

8. Deje abierta la **Consola de plantillas de certificado**.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>Crear la plantilla de certificado de agente de recuperación de claves de MIM CM

1. En la consola **Plantillas de certificado**, en el panel de **detalles**, seleccione y haga clic con el botón derecho en **Agente de recuperación de claves** y después haga clic en **Plantilla duplicada**.

2. En el cuadro de diálogo **Plantilla duplicada**, seleccione **Windows Server 2003 Enterprise** y, después, haga clic en **Aceptar**.

3. En el cuadro de diálogo **Propiedades de plantilla nueva**, en el cuadro **Nombre para mostrar de plantilla** de la pestaña **General**, escriba **Agente de recuperación de claves MIM CM**. Asegúrese de que el **Período de validez** es **2 años** en la pestaña **Criptografía**.

4. En el cuadro de diálogo **Selección de proveedores**, deshabilite **Proveedor de cifrado mejorado de Microsoft v1.0**, habilite **Proveedor de servicios de cifrado RSA y AES mejorado de Microsoft** y después haga clic en **Aceptar**.

5. En la pestaña **Requisitos de emisión**, asegúrese de que **Aprobación del administrador de certificados de entidad de certificación** está **deshabilitado**.

6. En la pestaña **Seguridad**, siga estos pasos:

    - Quite **Administrador**.

    - Quite **Admins. del dominio**.

    - Asigne permisos de solo **Lectura** y **Escritura** a **Administradores de empresa**.

    - Agregue **MIMCMKRAgent**.

    - Asigne los permisos **Lectura** e **Inscripción** a **KRAgent**.

7. En el cuadro de diálogo **Propiedades de plantilla nueva**, haga clic en **Aceptar**.

8. Cierre la **Consola de plantillas de certificado**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Publicar las plantillas de certificado requeridas en la entidad de certificación

1. Restaure la consola **Entidad de certificación**.

2. En la consola **Entidad de certificación**, en el árbol de consola, haga clic con el botón derecho en **Plantillas de certificado**, haga clic en **Nueva** y después en **Plantilla de certificado que se va a emitir**.

3. En el cuadro de diálogo **Habilitar plantillas de certificados**, seleccione **Agente de inscripción de MIM CM**, **MIM CM Key Recovery Agent** y **Firma de MIM CM**. Haga clic en **Aceptar**.

4. En el árbol de consola, haga clic en **Plantillas de certificado**.

5. Compruebe que las tres plantillas nuevas aparecen en el panel de **detalles** y después cierre **Entidad de certificación**.

    ![Firma de MIM CM](media/mim-cm-deploy/image016.png)

6. Cierre todas las ventanas abiertas y cierre la sesión.

### <a name="iis-configuration"></a>Configuración de IIS

Para hospedar el sitio web para CM, instale y configure IIS.

#### <a name="install-and-configure-iis"></a>Instalar y configurar IIS

1. Inicie sesión en **CORLog in** con la cuenta **MIMINSTALL**.

    >[!IMPORTANT]
    >La cuenta de instalación de MIM debe ser un administrador local.

2. Abra PowerShell y ejecute el siguiente comando.

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>Con IIS 7 se instala de manera predeterminada un sitio denominado Sitio web predeterminado. Si ese sitio se ha cambiado de nombre o ha eliminado, debe estar disponible un sitio con el nombre Sitio web predeterminado para poder instalar MIM CM.

#### <a name="configuring-kerberos"></a>Configuración de Kerberos

La cuenta de MIMCMWebAgent ejecutará el portal de MIM CM. De forma predeterminada en IIS se usa la autenticación de modo kernel. Se deshabilitará la autenticación de modo kernel de Kerberos y en su lugar se configurarán SPN en la cuenta de MIMCMWebAgent. Algunos comandos requerirán permisos elevados en Active Directory y el servidor CORPCM.

![Diagrama](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**Actualización de IIS en CORPCM**

![diagrama](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>Debe agregar un registro A de DNS para "cm.contoso.com" y apuntar a CORPCM IP.

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>Requerir SSL en el portal de MIM CM

Se recomienda encarecidamente requerir SSL en el portal de MIM CM. Si no lo hace, el asistente mostrará una advertencia al respecto.

1. Inscriba el certificado web para asignar **cm.contoso.com** al sitio predeterminado.

2. Abra el **Administrador de IIS** y vaya a **Administración de certificados**.

3. En Vista Características, haga doble clic en Configuración de SSL.

4. En la página Configuración de SSL, seleccione **Requerir SSL**.

5. En el panel Acciones, haga clic en **Aplicar**.

### <a name="database-configuration-corpsql-for-mim-cm"></a>Configuración de base de datos **CORPSQL** para MIM CM

1. Asegúrese de que está conectado al servidor CORPSQL01.

2. Asegúrese de que ha iniciado sesión como SQL DBA.

3. Ejecute el siguiente script de T-SQL para permitir que la cuenta CONTOSO\\MIMINSTALL cree la base de datos cuando se dirija al paso de configuración.

    >[!NOTE]
    >Tendrá que volver a SQL cuando esté preparado para el módulo de salida y directivas.

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![Mensaje de error del Asistente para la configuración de MIM CM](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Implementación del administrador de certificados de Microsoft Identity Manager 2016

1. Asegúrese de que está conectado al servidor CORPCM y que la cuenta **MIMINSTALL** es miembro del grupo de **administradores locales**.

2. Asegúrese de que ha iniciado sesión como Contoso\\MIMINSTALL.

3. Monte la imagen ISO de Microsoft Identity Manager SP1.

4. **Abra** el directorio **Certificate Management\\x64**.

5. En la ventana **x64**, haga clic con el botón derecho en **Setup** y, después, haga clic en **Ejecutar como administrador**.

6. En la página Welcome to the Microsoft Identity Manager Certificate Management Setup Wizard (Bienvenido al Asistente para instalación de Microsoft Identity Manager Certificate Management), haga clic en **Siguiente**.

7. En la página Contrato de licencia para el usuario final, lea el contrato de licencia, active la **casilla** Acepto los términos del contrato de licencia y, después, haga clic en Siguiente.

8. En la página Instalación personalizada, asegúrese de que los componentes **Portal de MIM CM** y **Servicio de actualización de MIM CM** están configurados para instalarse y, después, haga clic en **Siguiente**.

9. En la página Carpeta web virtual, asegúrese de que el nombre de la carpeta virtual es **CertificateManagement** y, después, haga clic en **Siguiente**.

10. En la página Instalar Microsoft Identity Manager Certificate Management, haga clic en **Instalar**.

11. En la página **Completed** the Microsoft Identity Manager Certificate Management Setup Wizard (Asistente para instalación de Microsoft Identity Manager Certificate Management completado), haga clic en **Finalizar**.

![Asistente de MIM CM completado](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Asistente para la configuración del administrador de certificados de Microsoft Identity Manager 2016

Antes de iniciar sesión en CORPCM, agregue MIMINSTALL al grupo **administradores de dominio, administradores de esquema y administradores locales** para el asistente de configuración. Esto se puede quitar posteriormente una vez completada la configuración.

![Mensaje de error](media/mim-cm-deploy/image028.png)

1. Desde el menú **Inicio**, haga clic en **Asistente para configurar Certificate Management**. Ejecute como **Administrador**.

2. En la página **Asistente para configuración**, haga clic en **Siguiente**.

3. En la página **Configuración de CA**, asegúrese de que la entidad de certificación seleccionada es **Contoso-CORPCA-CA** y el servidor seleccionado es **CORPCA.CONTOSO.COM** y después haga clic en **Siguiente**.

4. En la página **Configurar la base de datos de Microsoft® SQL Server®**, en el cuadro **Nombre del servidor SQL Server**, escriba **CORPSQL1**, active la casilla **Usar mis credenciales para crear la base de datos** y, después, haga clic en **Siguiente**.

5. En la página **Configuración de base de datos**, acepte el nombre de base de datos predeterminado de **FIMCertificateManagement**, asegúrese de que la opción **Autenticación integrada de SQL** está seleccionada y, después, haga clic en **Siguiente**.

6. En la página **Configurar Active Directory**, acepte el nombre predeterminado proporcionado para el punto de conexión de servicio y, después, haga clic en **Siguiente**.

7. En la página **Método de autenticación**, confirme que la opción **Autenticación integrada de Windows** está seleccionada y después haga clic en **Siguiente**.

8. En la página **Agentes: FIM CM**, desactive la casilla **Usar la configuración predeterminada de FIM CM** y, después, haga clic en **Cuentas personalizadas**.

9. En el cuadro de diálogo de varias pestañas **Agentes: FIM CM**, escriba la información siguiente en cada pestaña:

   - Nombre de usuario: **Update**

   - Contraseña: **Pass\@word1**

   - Confirmar contraseña: **Pass\@word1**

   - Usar un usuario existente: **Enabled**

     >[!NOTE]
     >Estas cuentas se crearon anteriormente. Asegúrese de que se repiten los procedimientos del paso 8 para las seis pestañas de cuenta de agente.

     ![Cuentas de MIM CM](media/mim-cm-deploy/image030.png)

10. Cuando se complete toda la información de cuentas de agente, haga clic en **Aceptar**.

11. En la página **Agentes: MIM CM**, haga clic en **Siguiente**.

12. En la página **Configurar certificados de servidor**, habilite las siguientes plantillas de certificado:

    - Plantilla de certificado que se va a usar para el certificado de Key Recovery Agent del agente de recuperación: **MIMCMKeyRecoveryAgent**.

    - Plantilla de certificado que se va a usar para el certificado de agente FIM CM: **MIMCMSigning**.

    - Plantilla de certificado que se va a usar para el certificado de agente de inscripción: **FIMCMEnrollmentAgent**.

13. En la página **Configurar certificados de servidor**, haga clic en **Siguiente**.

14. En la página de **configuración del servidor de correo electrónico, impresión de documentos**, en el cuadro **Especifique el nombre del servidor SMTP que desee usar para enviar notificaciones de registro de correo electrónico**, haga clic en **Siguiente**.

15. En la página **Listo para configurar**, haga clic en **Configurar**.

16. En el cuadro de diálogo de advertencia **Asistente de configuración: Microsoft Forefront Identity Manager 2010 R2**, haga clic en **Aceptar** para confirmar que no se ha habilitado SSL en el directorio virtual de IIS.

    ![media/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >No haga clic en el botón Finalizar hasta que termine la ejecución del asistente de configuración. El registro del asistente se puede encontrar aquí: **%programfiles%\\Microsoft Forefront Identity Management\\2010\\Certificate Management\\config.log**.

17. Haga clic en **Finalizar**.

    ![Asistente de MIM CM completado](media/mim-cm-deploy/image033.png)

18. Cierre todas las ventanas abiertas.

19. Agregue https://cm.contoso.com/certificatemanagement a la zona de intranet local del explorador.

20. Visite el sitio del servidor CORPCM https://cm.contoso.com/certificatemanagement.  

    ![diagrama](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Comprobar el servicio de aislamiento de claves CNG

1. Desde **Herramientas administrativas**, abra **Servicios**.

2. En el panel de **detalles**, haga doble clic en **Aislamiento de claves CNG**.

3. En la pestaña **General**, cambie **Tipo de inicio** a **Automático**.

4. En la pestaña **General**, inicie el servicio si no está en un estado iniciado.

5. En la pestaña **General**, haga clic en **Aceptar**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Instalación y configuración de los módulos de CA:

En este paso, se instalarán y configurarán los módulos de CA de FIM CM en la entidad de certificación.

1. Configure FIM CM para que solo se inspeccionen permisos de usuario para las operaciones de administración.

2. En la ventana **C:\\Archivos de programa\\Microsoft Forefront Identity Manager\\2010\\Certificate Management\\web** haga una copia de **web.config** con el nombre **web.1.config**.

3. En la ventana **Web**, haga clic con el botón derecho en **Web.config** y después haga clic en **Abrir**.

    >[!Note]
    >El archivo Web.config se abre en el Bloc de notas.

4. Presione CTRL+F cuando se abra el archivo.

5. En el cuadro de diálogo **Buscar y reemplazar**, en el cuadro **Buscar**, escriba **UseUser** y después haga clic tres veces en **Buscar siguiente**.

6. Cierre el cuadro de diálogo **Buscar y reemplazar**.

7. Debería estar en la línea **\<add key="Clm.RequestSecurity.Flags" value="UseUser,UseGroups" /\>**. Cambie la línea para que se lea **\<add key="Clm.RequestSecurity.Flags" value="UseUser" /\>**.

8. Cierre el archivo y guarde todos los cambios.

9. Cree una cuenta para el equipo de CA en el servidor SQL \<sin script\>.

10. Asegúrese de que está conectado al servidor **CORPSQL01**.

11. Asegúrese de que ha iniciado sesión como **DBA**.

12. Desde el menú **Inicio**, inicie **SQL Server Management Studio**.

13. En el cuadro de diálogo **Conectar a servidor**, en el cuadro **Nombre del servidor**, escriba **CORPSQL01** y después haga clic en **Conectar**.

14. En el árbol de consola, expanda **Seguridad** y después haga clic en **Inicios de sesión**.

15. Haga clic con el botón secundario en **Inicios de sesión**y, a continuación, haga clic en **Nuevo inicio de sesión**.

16. En la página **General**, en el cuadro **Nombre de inicio de sesión**, escriba **contoso\\CORPCA\$**. Seleccione **Autenticación de Windows**. La base de datos predeterminada es **FIMCertificateManagement**.

17. En el panel de la izquierda, seleccione **Asignación de usuarios**. En el panel de la derecha, haga clic en la casilla de la columna **Asignar** situada junto a **FIMCertificateManagement**. En la lista **Pertenencia al rol de la base de datos para: FIMCertificateManagement**, habilite el rol **clmApp**.

18. Haga clic en **Aceptar**.

19. Cierre **Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Instalar los módulos de CA de FIM CM en la entidad de certificación

1. Asegúrese de que está conectado al servidor **CORPCA**.

2. En la ventana **X64**, haga clic con el botón derecho en **Setup.exe** y, después, haga clic en **Ejecutar como administrador**.

3. En la página **Welcome to the Microsoft Identity Manager Certificate Management Setup Wizard** (Bienvenido al Asistente para instalación de Microsoft Identity Manager Certificate Management), haga clic en **Siguiente**.

4. En la página **Contrato de licencia para el usuario final**, lea los términos de la licencia. Active la casilla **Acepto los términos del contrato de licencia** y, después, haga clic en **Siguiente**.

5. En la página **Instalación personalizada**, seleccione **Portal de MIM CM** y, después, haga clic en **Esta característica no estará disponible**.

6. En la página **Instalación personalizada**, seleccione **Servicio de actualización de MIM CM** y, después, haga clic en **Esta característica no estará disponible**.

    >[!Note]
    >Esto dejará los archivos de entidad de certificación de MIM CM como la única característica habilitada para la instalación.

7. En la página **Instalación personalizada**, haga clic en **Siguiente**.

8. En la página **Instalar Microsoft Identity Manager Certificate Management**, haga clic en **Instalar**.

9. En la página **Completed the Microsoft Identity Manager Certificate Management Setup Wizard** (Asistente para instalación de Microsoft Identity Manager Certificate Management completado), haga clic en **Finalizar**.

10. Cierre todas las ventanas abiertas.

### <a name="configure-the-mim-cm-exit-module"></a>Configurar el módulo de salida de MIM CM

1. Desde **Herramientas administrativas**, abra **Entidad de certificación**.

2. En el árbol de consola, haga clic con el botón derecho en **contoso-CORPCA-CA** y, después, haga clic en **Propiedades**.

3. En la pestaña **Módulo de salida**, seleccione **Módulo de salida de FIM CM** y, después, haga clic en **Propiedades**.

4. En el cuadro **Especificar cadena de conexión a la base de datos de CM**, escriba **Connect Timeout=15;Persist Security Info=True; Integrated Security=sspi;Initial Catalog=FIMCertificateManagement;Data Source=CORPSQL01**. Deje la casilla **Cifrar la cadena de conexión** activada y después haga clic en **Aceptar**.
5. En el cuadro de mensaje **Microsoft FIM Certificate Management**, haga clic en **Aceptar**.

6. En el cuadro de diálogo **Propiedades de contoso-CORPCA-CA**, haga clic en **Aceptar**.

7. Haga clic con el botón derecho en **contoso-CORPCA-CA** *,* seleccione **Todas las tareas** y luego haga clic en **Detener servicio**. Espere hasta que Servicios de certificados de Active Directory se detenga.

8. Haga clic con el botón derecho en **contoso-CORPCA-CA** *,* seleccione **Todas las tareas** y luego haga clic en **Iniciar servicio**.

9. Minimice la consola **Entidad de certificación**.

10. Desde **Herramientas administrativas**, abra **Visor de eventos**.

11. En el árbol de consola, expanda **Registros de aplicaciones y servicios** y, después, haga clic en **FIM Certificate Management**.

12. En la lista de eventos, compruebe que los eventos más recientes *no* incluyen ningún evento **Advertencia** o **Error** desde el último reinicio de Servicios de certificados.

    >[!NOTE] 
    >El último evento debería indicar que el módulo de salida se ha cargado con la configuración de `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`.

13. Minimice el **Visor de eventos**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Copiar la huella digital del certificado MIMCMAgent al Portapapeles de Windows®

1. Restaure la consola **Entidad de certificación**.

2. En el árbol de consola, expanda **contoso-CORPCA-CA** y, después, haga clic en **Certificados emitidos**.

3. En el panel de **detalles**, haga doble clic en el certificado con **CONTOSO\\MIMCMAgent** en la columna **Nombre del solicitante** y con **Firma de FIM CM** en la columna **Plantilla de certificado**.

4. En la pestaña **Detalles** , seleccione el campo **Huella digital** .

5. Seleccione la huella digital y después presione CTRL+C.

    >[!NOTE]
    >**No** incluya el espacio inicial en la lista de caracteres de huella digital.

6. En el cuadro de diálogo **Certificado**, haga clic en **Aceptar**.

7. Desde el menú **Inicio**, escriba **Bloc de notas** en el cuadro **Buscar programas y archivos** y después presione ENTRAR.

8. En **Bloc de notas**, desde el menú **Edición**, haga clic en **Pegar**.

9. En el menú **Edición**, haga clic en **Reemplazar**.

10. En el cuadro **Buscar**, escriba un carácter de espacio y después haga clic en **Reemplazar todo**.

    >[!Note]
    >Esto quita todos los espacios entre los caracteres de la huella digital.

11. En el cuadro de diálogo **Reemplazar**, haga clic en **Cancelar**.

12. Seleccione la *cadena de huella digital* convertida y después presione CTRL+C.

13. Cierre el **Bloc de notas** sin guardar los cambios.

### <a name="configure-the-fim-cm-policy-module"></a>Configurar el módulo de directivas de FIM CM

1. Restaure la consola **Entidad de certificación**.

2. Haga clic con el botón derecho en **contoso-CORPCA-CA** y, después, haga clic en **Propiedades**.

3. En el cuadro de diálogo **Propiedades de contoso-CORPCA-CA**, en la pestaña **Módulo de directivas**, haga clic en **Propiedades**.

    - En la pestaña **General**, asegúrese de que la opción **Pasar solicitudes no FIM CM al módulo de directivas predeterminado para procesarlas** está seleccionada.

    - En la pestaña **Certificados de firma**, haga clic en **Agregar**.

    - En el cuadro de diálogo Certificado, haga clic con el botón derecho en el cuadro **Especifique el hash del certificado con codificación hexadecimal** y, después, haga clic en **Pegar**.

    - En el cuadro de diálogo **Certificado**, haga clic en **Aceptar**.

        >[!Note]
        >Si el botón **Aceptar** no está habilitado, se incluyó accidentalmente un carácter oculto en la cadena de huella digital cuando se copió del certificado clmAgent. Repita todos los pasos a partir de **Tarea 4: Copiar la huella digital del certificado MIMCMAgent al Portapapeles de Windows** en este ejercicio.

4. En el cuadro de diálogo **Propiedades de configuración**, asegúrese de que la huella digital aparece en la lista **Certificados de firma válidos** y, después, haga clic en **Aceptar**.

5. En el cuadro de mensaje **FIM Certificate Management**, haga clic en **Aceptar**.

6. En el cuadro de diálogo **Propiedades de contoso-CORPCA-CA**, haga clic en **Aceptar**.

7. Haga clic con el botón derecho en **contoso-CORPCA-CA** *,* seleccione **Todas las tareas** y luego haga clic en **Detener servicio**.

8. Espere hasta que Servicios de certificados de Active Directory se detenga.

9. Haga clic con el botón derecho en **contoso-CORPCA-CA** *,* seleccione **Todas las tareas** y luego haga clic en **Iniciar servicio**.

10. Cierre la consola **Entidad de certificación**.

11. Cierre todas las ventanas abiertas y después cierre la sesión.

El **último paso en la implementación** consiste en asegurarse de que los administradores de CONTOSO\\MIMCM pueden implementar y crear plantillas, y configurar el sistema sin ser administradores de esquema ni de dominio. El script siguiente incluirá en una ACL los permisos de las plantillas de certificado mediante dsacls. Ejecute con una cuenta que tenga permisos completos para cambiar los permisos de seguridad de lectura y escritura en todas las plantillas de certificado existentes en el bosque.

Primeros pasos: **configuración de permisos de punto de conexión de servicio y grupo de destino, y delegación de la administración de plantillas de perfil**

1. Configurar permisos en el punto de conexión de servicio (SCP).

2. Configurar la administración de plantillas de perfil delegadas.

3. Configurar permisos en el punto de conexión de servicio (SCP). **\<sin script\>**

4.   Asegúrese de que está conectado al servidor virtual **CORPDC**.

5. Inicie sesión como **contoso\\corpadmin**.

6. Desde **Herramientas administrativas**, abra **Usuarios y equipos de Active Directory**.

7. En **Usuarios y equipos de Active Directory**, en el menú **Ver**, asegúrese de que la opción **Características avanzadas** está seleccionada.

8. En el árbol de consola, expanda **Contoso.com** \| **System** \| **Microsoft** \| **Certificate Lifecycle Manager** y, después, haga clic en **CORPCM**.

9. Haga clic con el botón derecho en **CORPCM** y, después, haga clic en **Propiedades**.

10. En el cuadro de diálogo **Propiedades de CORPCM**, en la pestaña **Seguridad**, agregue los grupos siguientes con los permisos correspondientes:

    | Group          | Permisos      |
    |----------------|------------------|
    | mimcm-Managers | Lectura </br> FIM CM Audit</br> FIM CM Enrollment Agent</br> FIM CM Request Enroll</br> FIM CM Request Recover</br> FIM CM Request Renew</br> FIM CM Request Revoke </br> FIM CM Request Unblock Smart Card |
    | mimcm-HelpDesk | Lectura</br> FIM CM Enrollment Agent</br> FIM CM Request Revoke</br> FIM CM Request Unblock Smart Card |

11. En el cuadro de diálogo **Propiedades de CORPDC**, haga clic en **Aceptar**.

12. Mantenga abierto **Usuarios y equipos de Active Directory**.

**Configurar permisos en los objetos de usuario descendiente**

1. Asegúrese de que sigue en la consola de **Usuarios y equipos de Active Directory**.

2. En el árbol de consola, haga clic con el botón derecho en **Contoso.com** y, después, haga clic en **Propiedades**.

3. En la pestaña **Seguridad**, haga clic en **Opciones avanzadas**.

4. En el cuadro de diálogo **Configuración de seguridad avanzada para Contoso**, haga clic en **Agregar**.

5. En el cuadro de diálogo **Seleccionar usuario, equipo, cuenta de servicio o grupo**, escriba **mimcm-Managers** en el cuadro de texto **Escriba el nombre del objeto para seleccionar** y después haga clic en **Aceptar**.

6. En el cuadro de diálogo **Entrada de permiso para Contoso**, en la lista **Se aplica a**, seleccione **Objetos de usuario descendiente** y, después, active la casilla **Permitir** de los **permisos** siguientes:

    - **Leer todas las propiedades**

    - **Permisos de lectura**

    - **FIM CM Audit**

    - **FIM CM Enrollment Agent**

    - **FIM CM Request Enroll**

    - **FIM CM Request Recover**

    - **FIM CM Request Renew**

    - **FIM CM Request Revoke**

    - **FIM CM Request Unblock Smart Card**

7. En el cuadro de diálogo **Entrada de permiso para Contoso**, haga clic en **Aceptar**.

8. En el cuadro de diálogo **Configuración de seguridad avanzada para Contoso**, haga clic en **Agregar**.

9. En el cuadro de diálogo **Seleccionar usuario, equipo, cuenta de servicio o grupo**, escriba **mimcm-HelpDesk** en el cuadro de texto **Escriba el nombre del objeto para seleccionar** y después haga clic en **Aceptar**.

10. En el cuadro de diálogo **Permission Entry for Contoso** (Entrada de permiso para Contoso), en la lista **Apply to** (Aplicar a), seleccione **Descendant User objects** (Objetos de usuario descendiente) y, después, active la casilla **Permitir** de los **permisos** siguientes:

    - **Leer todas las propiedades**

    - **Permisos de lectura**

    - **FIM CM Enrollment Agent**

    - **FIM CM Request Revoke**

    - **FIM CM Request Unblock Smart Card**

11. En el cuadro de diálogo **Entrada de permiso para Contoso**, haga clic en **Aceptar**.

12. En el cuadro de diálogo **Configuración de seguridad avanzada para Contoso**, haga clic en **Aceptar**.

13. En el cuadro de diálogo **Propiedades de contoso.com**, haga clic en **Aceptar**.

14. Mantenga abierto **Usuarios y equipos de Active Directory**.

**Configurar permisos en los objetos de usuario descendiente \<sin script\>**

1. Asegúrese de que sigue en la consola de **Usuarios y equipos de Active Directory**.

2. En el árbol de consola, haga clic con el botón derecho en **Contoso.com** y, después, haga clic en **Propiedades**.

3. En la pestaña **Seguridad**, haga clic en **Opciones avanzadas**.

4. En el cuadro de diálogo **Configuración de seguridad avanzada para Contoso**, haga clic en **Agregar**.

5. En el cuadro de diálogo **Seleccionar usuario, equipo, cuenta de servicio o grupo**, escriba **mimcm-Managers** en el cuadro de texto **Escriba el nombre del objeto para seleccionar** y después haga clic en **Aceptar**.

6. En el cuadro de diálogo **Permission Entry for CONTOSO** (Entrada de permiso para CONTOSO), en la lista **Apply to** (Aplicar a), seleccione **Descendant User objects** (Objetos de usuario descendiente) y, después, active la casilla **Permitir** de los **permisos** siguientes:

    - **Leer todas las propiedades**

    - **Permisos de lectura**

    - **FIM CM Audit**

    - **FIM CM Enrollment Agent**

    - **FIM CM Request Enroll**

    - **FIM CM Request Recover**

    - **FIM CM Request Renew**

    - **FIM CM Request Revoke**

    - **FIM CM Request Unblock Smart Card**

7. En el cuadro de diálogo **Entrada de permiso para CONTOSO**, haga clic en **Aceptar**.

8. En el cuadro de diálogo **Configuración de seguridad avanzada para CONTOSO**, haga clic en **Agregar**.

9. En el cuadro de diálogo **Seleccionar usuario, equipo, cuenta de servicio o grupo**, escriba **mimcm-HelpDesk** en el cuadro de texto **Escriba el nombre del objeto para seleccionar** y después haga clic en **Aceptar**.

10. En el cuadro de diálogo **Permission Entry for CONTOSO** (Entrada de permiso para CONTOSO), en la lista **Apply to** (Aplicar a), seleccione **Descendant User objects** (Objetos de usuario descendiente) y, después, active la casilla **Permitir** de los **permisos** siguientes:

    - **Leer todas las propiedades**

    - **Permisos de lectura**

    - **FIM CM Enrollment Agent**

    - **FIM CM Request Revoke**

    - **FIM CM Request Unblock Smart Card**

11. En el cuadro de diálogo **Entrada de permiso para contoso**, haga clic en **Aceptar**.

12. En el cuadro de diálogo **Configuración de seguridad avanzada para Contoso**, haga clic en **Aceptar**.

13. En el cuadro de diálogo **Propiedades de contoso.com**, haga clic en **Aceptar**.

14. Mantenga abierto **Usuarios y equipos de Active Directory**.

Segundo paso: **Delegación de permisos de administración de plantillas de certificado \<script\>**

- Delegación de permisos en el contenedor Plantillas de certificado.

- Delegación de permisos en el contenedor OID.

- Delegación de permisos en las plantillas de certificado existentes.

Definir permisos en el contenedor Plantillas de certificado

1. Restaure la consola **Sitios y servicios de Active Directory**.

2. En el árbol de consola, expanda **Servicios**, expanda **Servicios de clave pública** y, después, haga clic en **Plantillas de certificado**.

3. En el árbol de consola, haga clic con el botón derecho en **Plantillas de certificado** y, después, haga clic en **Delegar control**.

4. En el **Asistente para delegación de control**, haga clic en **Siguiente**.

5. En la página **Usuarios o grupos**, haga clic en **Agregar**.

6. En el cuadro de diálogo **Select Users, Computers, or Groups** (Seleccionar usuarios, equipos o grupos), en el cuadro **Enter the object names to select** (Escriba los nombres de objeto para seleccionar), escriba **mimcm-Managers** y haga clic en **Aceptar**.

7. En la página **Usuarios o grupos**, haga clic en **Siguiente**.

8. En la página **Tareas que se delegarán**, haga clic en **Crear una tarea personalizada para delegar** y después en **Siguiente**.

9.  En la página **Tipo de objeto de Active Directory**, asegúrese de que está seleccionada la opción **This folder, existing objects in this folder, and creation of new objects in this folder** (Esta carpeta, los objetos contenidos en la misma y la creación de nuevos objetos en esta carpeta) y haga clic en **Siguiente**.

10. En la página **Permisos**, en la lista **Permisos**, active la casilla **Control total** y, después, haga clic en **Siguiente**.

11. En la página **Finalización del Asistente para delegación de control**, haga clic en **Finalizar**.

Definir permisos en el contenedor OID:

1. En el árbol de consola, haga clic con el botón derecho en **OID** y, después, haga clic en **Propiedades**.

2. En la pestaña **Seguridad** del cuadro de diálogo **Propiedades de OID**, haga clic en **Opciones avanzadas**.

3. En el cuadro de diálogo **Configuración de seguridad avanzada para OID**, haga clic en **Agregar**.

4. En el cuadro de diálogo **Seleccionar usuario, equipo, cuenta de servicio o grupo**, escriba **mimcm-Managers** en el cuadro de texto **Escriba el nombre del objeto para seleccionar** y después haga clic en **Aceptar**.

5. En el cuadro de diálogo **Entrada de permisos para OID**, asegúrese de que los permisos se aplican a **Este objeto y todos los descendientes**, haga clic en **Control total** y después en **Aceptar**.

6. En el cuadro de diálogo **Configuración de seguridad avanzada para OID**, haga clic en **Aceptar**.

7. En el cuadro de diálogo **Propiedades de OID**, haga clic en **Aceptar**.

8. Cierre **Sitios y servicios de Active Directory**.

**Scripts: Permisos en el contenedor OID, Plantilla de perfil y Plantillas de certificado**

![diagrama](media/mim-cm-deploy/image021.png)

```powershell
import-module activedirectory
$adace = @{
"OID" = "AD:\\CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"CT" = "AD:\\CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"PT" = "AD:\\CN=Profile Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com"
}
$adace.GetEnumerator() | **Foreach-Object** {
$acl = **Get-Acl** *-Path* $_.Value
$sid=(**Get-ADGroup** "MIMCM-Managers").SID
$p = **New-Object** System.Security.Principal.SecurityIdentifier($sid)
##https://msdn.microsoft.com/library/system.directoryservices.activedirectorysecurityinheritance(v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule
($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow,
[DirectoryServices.ActiveDirectorySecurityInheritance]::All)
$acl.AddAccessRule($ace)
**Set-Acl** *-Path* $_.Value *-AclObject* $acl
}
```

**Scripts: Delegación de permisos en las plantillas de certificado existentes.**  

![diagrama](media/mim-cm-deploy/image039.png)

```shell
dsacls "CN=Administrator,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CAExchange,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CEPEncryption,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ClientAuth,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CodeSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CrossCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CTLSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DirectoryEmailReplication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainController,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainControllerAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFS,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFSRecovery,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgentOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMKeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOnline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KerberosAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Machine,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=MachineEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OCSPResponseSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OfflineRouter,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=RASAndIASServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardLogon,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SubCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=User,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=UserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=WebServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Workstation,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO
```
