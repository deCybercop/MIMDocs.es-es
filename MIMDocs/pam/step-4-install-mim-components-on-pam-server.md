---
title: 'Implementación de PAM, paso 4: instalación de MIM | Microsoft Docs'
description: Instale y configure el servicio y el portal de MIM en el servidor y las estaciones de trabajo de Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ef605496-7ed7-40f4-9475-5e4db4857b4f
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2b5340ef3f98ba94904e595c3526d09bdac3f95f
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379929"
---
# <a name="step-4--install-mim-components-on-pam-server-and-workstation"></a>Paso 4: Instalar componentes de MIM en un servidor y estación de trabajo PAM

> [!div class="step-by-step"]
> [« Paso 3](step-3-prepare-pam-server.md)
> [Paso 5 »](step-5-establish-trust-between-priv-corp-forests.md)

En PAMSRV, inicie sesión como PRIV\Administrador para poder instalar el servicio y el portal de MIM y la aplicación web de ejemplo de portal.

  > [!NOTE]
  > Debe ser administrador de dominio; si no está ejecutando los comandos siguientes como administrador de dominio, las comprobaciones de validación de confianza del paso siguiente no se completarán.

Si ha descargado MIM, descomprima el archivo de instalación de MIM en una carpeta nueva.

## <a name="run-the-service-and-portal-install-program"></a>Ejecute el programa de instalación del servicio y portal de MIM.

Siga las instrucciones del instalador y complete la instalación.

1. Al seleccionar las características de los componentes, incluya el servicio MIM (con Privileged Access Management, pero no informes MIM) y el portal MIM  

   ![Captura de pantalla de configuración personalizada](./media/PAM_GS_MIM_2015_Service_Portal.png)

2. Al configurar los servicios comunes y la conexión a la base de datos MIM, especifique **Crear una nueva base de datos**.

   > [!NOTE]
   > Si instala el servicio MIM varias veces para lograr una alta disponibilidad, especifique **Usar base de datos existente** para todas las instalaciones posteriores.

3. Al configurar una conexión de servidor de correo, establezca el servidor de correo en el nombre de host de un servidor Exchange o SMTP para el entorno CORP (use corpdc.contoso.local si no tiene ningún servidor de correo) y desactive las casillas **Usar SSL** y **El servidor de correo electrónico es Exchange Server 2007 o Exchange Server 2010**.

4. Seleccione generar un nuevo certificado autofirmado.

5. Establezca las credenciales de cuenta siguientes:
   - Nombre de la cuenta de servicio: *MIMService*  
   - Contraseña de la cuenta de servicio: <em>Pass@word1</em> (o la contraseña que creó en el paso 2)  
   - Dominio de la cuenta de servicio: *PRIV*  
   - Cuenta de servicio de correo electrónico: <em>MIMService@priv.contoso.local</em>  

6. Acepte los valores predeterminados para el nombre de host del servidor de sincronización y especifique la cuenta del agente de administración de MIM como *PRIV\MIMMA*. Aparecerá un mensaje advirtiendo de que el servicio de sincronización de MIM no existe. Esto está bien, ya que el servicio de sincronización de MIM no se usa en este escenario.

7. Establezca *PAMSRV* como dirección del servidor del servicio MIM.

8. Defina *http://pamsrv.priv.contoso.local:82* como dirección URL de la colección de sitios de SharePoint.

9. Deje en blanco la dirección URL del portal de registro.

10. Active la casilla para abrir los puertos 5725 y 5726 del firewall y la casilla para conceder acceso al sitio del portal MIM a todos los usuarios autenticados.

11. Deje vacío el nombre de host de la API de REST de PAM y establezca *8086* como número de puerto.

    ![Captura de pantalla de información de enlace de la API de REST de PAM](./media/PAM_GS_MIM_2015_Service_Portal_configure_application_pool.png)

12. Configure la cuenta de la API de REST de MIM PAM para usar la misma cuenta que SharePoint (ya que el portal MIM se encuentra en este servidor):
    - Nombre de la cuenta del grupo de aplicaciones: *SharePoint*  
    - Contraseña de la cuenta del grupo de aplicaciones: <em>Pass@word1</em> (o la contraseña que creó en el paso 2)  
    - Dominio de la cuenta del grupo de aplicaciones: *PRIV*  

    ![Captura de pantalla de credenciales de cuenta de grupo de aplicaciones](./media/PAM_GS_Configure_Component_Service.png)

    Puede aparecer una advertencia que indique que la cuenta del servicio no es segura en su configuración actual. Eso está bien.

13. Configure el servicio de componente MIM PAM:
    - Nombre de la cuenta de servicio: *MIMComponent*
    - Contraseña de la cuenta de servicio: <em>Pass@word1</em> (o la contraseña que creó en el paso 2)  
    - Dominio de la cuenta de servicio: *PRIV*

    ![Captura de pantalla de credenciales de cuenta de servicio de componente PAM](./media/PAM_GS_Configure_MIM_PAM_component_service.png)

14. Configure el servicio de supervisión de PAM:
    - Nombre de la cuenta de servicio: *MIMMonitor*  
    - Contraseña de la cuenta de servicio: <em>Pass@word1</em> (o la contraseña que creó en el paso 2)  
    - Dominio de la cuenta de servicio: *PRIV*  

    ![Captura de pantalla de credenciales de cuenta de servicio de supervisión de PAM](./media/PAM_GS_Configur_PAM_Monitoring_service.png)

15. En la página Escribir la información para el portal de contraseñas de MIM, deje las casillas sin activar y continúe. Haga clic en **Siguiente** para continuar con la instalación.

Una vez finalizada la instalación, el servidor se reiniciará. Compruebe a continuación que el portal de MIM está activo y permite a los usuarios ver su propio recurso de objeto en MIM.

## <a name="set-up-mim-portal-management-policy-rules"></a>Configuración de las reglas de directivas de administración del portal MIM

1. Una vez reiniciado PAMSRV, inicie sesión como PRIV\Administrador.

2. Inicie Internet Explorer y conéctese al portal de MIM en http://pamsrv.priv.contoso.local:82/identitymanagement. Puede haber una breve demora la primera vez que se busca esta página.

3. Si fuese necesario, inicie sesión como PRIV\Administrador en Internet Explorer.

4. En Internet Explorer, abra **Opciones de Internet**, cambie a la pestaña **Seguridad** y agregue el sitio a **Zona de Intranet local** si todavía no está ahí. Cierre el diálogo Opciones de Internet.

5. Mientras usa Internet Explorer para ver el portal MIM, haga clic en **Reglas de directiva de administración**.

6. Busque la regla de directivas de administración **Administración de usuarios: Los usuarios pueden leer atributos por sí mismos**.

7. Seleccione esta regla de directiva de administración, desactive **La directiva está deshabilitada**, haga clic en **Aceptar** y luego en **Enviar**.

## <a name="verify-the-firewall-connections"></a>Comprobación de las conexiones del firewall

El firewall debería permitir las conexiones entrantes al puerto TCP 5725, 5726, 8086 y 8090.

1.  Inicie **Firewall de Windows con seguridad avanzada** (se encuentra en Herramientas administrativas).  
2.  Haga clic en **Reglas de entrada**.  
3.  Compruebe que aparecen estas dos reglas:  
    - Servicio Forefront Identity Manager (STS).
    - Servicio Forefront Identity Manager (servicio web).  
4.  Haga clic en **Nueva regla** > **Puerto** > **TCP** y escriba los puertos locales concretos *8086* y *8090*. Haga clic en el asistente y acepte los valores predeterminados, asigne un nombre a la regla y haga clic en **Finalizar**.  
5.  Después de completar el asistente, cierre la aplicación Firewall de Windows.

6.  Inicie el **Panel de Control**.  
7.  En Red e Internet, seleccione **Ver el estado y las tareas de red**.  
8.  Compruebe que haya una red activa con el nombre priv.contoso.local como y una red de dominios.  
9. Cierre el **Panel de control**.

## <a name="set-up-the-sample-web-application"></a>Configuración de la aplicación web de ejemplo

En esta sección se instalará y configurará la aplicación web de ejemplo para la API de REST de MIM PAM.

1. Desde el archivo de la aplicación web de ejemplo, descargue los [ejemplos de Identity Management](https://github.com/Azure/identity-management-samples) como un archivo zip.

2. Extraiga el contenido de la carpeta **identity-management-samples-master\Privileged-Access-Management-Portal\src** en una nueva carpeta **C:\Archivos de programa\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal**.

3. Cree un nuevo sitio web en IIS con el nombre de sitio Portal de ejemplo de Privileged Access Management MIM, la ruta de acceso física C:\Archivos de programa\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal y el puerto 8090.  Puede hacerlo mediante el siguiente comando de PowerShell:

   ```PowerShell
   New-WebSite -Name "MIM Privileged Access Management Example Portal" -Port 8090   -PhysicalPath "C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\"
   ```

4. Configure la aplicación web de ejemplo de modo que pueda redirigir a los usuarios a la API de REST de MIM PAM. Con un editor de texto como el Bloc de notas, edite el archivo **C:\Archivos de programa\Microsoft Forefront Identity Manager\2010\Privileged Access Management REST API\web.config**. En la sección **<system.webServer>**, agregue las siguientes líneas:

   ```XML
   <httpProtocol>
   <customHeaders>
     <add name="Access-Control-Allow-Credentials" value="true"  />
     <add name="Access-Control-Allow-Headers" value="content-type" />
     <add name="Access-Control-Allow-Origin" value="http://pamsrv:8090" />  
   </customHeaders>
   </httpProtocol>
   ```

5. Configure la aplicación web de ejemplo. Con un editor de texto como el Bloc de notas, edite el archivo **C:\Archivos de programa\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\js\utils.js**. Establezca el valor de **pamRespApiUrl** en *http://pamsrv.priv.contoso.local:8086/api/pamresources/*.

6. Reinicie IIS con el siguiente comando para que estos cambios surtan efecto.

   ```cmd
   iisreset
   ```

7. (Opcional) Compruebe que el usuario puede autenticarse en la API de REST. Abra un explorador web como administrador en PAMSRV.  Navegue hasta la dirección URL del sitio web http://pamsrv.priv.contoso.local:8086/api/pamresources/pamroles/, autentíquese,si es necesario, y asegúrese de que se produzca una descarga.

## <a name="install-the-mim-pam-requestor-cmdlets"></a>Instalación de los cmdlets de solicitante de MIM PAM

Instale los cmdlets de solicitante de MIM PAM en la estación de trabajo configurada en el paso 1.

1.  Inicie sesión en CORPWKSTN como administrador.

2.  Descargue los **complementos y extensiones** en el equipo CORPWKSTN si todavía no están ahí.

3.  Desempaquete del archivo la carpeta **Complementos y extensiones** en una nueva carpeta.

4.  Ejecute el instalador **setup.exe**.

5.  En la instalación personalizada, especifique que el **cliente de PAM** se va a instalar, pero no el **complemento MIM para Outlook** ni las **extensiones de contraseña y autenticación de MIM**.

6.  En la dirección del servidor PAM, especifique como nombre de host del servidor PRIV MIM *pamsrv.priv.contoso.local*.

Una vez finalizada la instalación, reinicie CORPWKSTN para completar el registro del nuevo módulo de PowerShell.

En el siguiente paso se establecerá la confianza entre bosques PRIV y CORP.

> [!div class="step-by-step"]
> [« Paso 3](step-3-prepare-pam-server.md)
> [Paso 5 »](step-5-establish-trust-between-priv-corp-forests.md)
