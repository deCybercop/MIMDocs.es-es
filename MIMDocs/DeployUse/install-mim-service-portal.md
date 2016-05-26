---
# required metadata

title: Instalación de MIM 2016&#58; servicio y portal de MIM | Microsoft Identity Manager
description: Obtención de los pasos para configurar e instalar el servicio y el portal de MIM de Microsoft Identity Manager 2016
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Instalación de MIM 2016: servicio y portal de MIM

>[!div class="step-by-step"]

> [!NOTE]
> « Servicio de sincronización de MIM. Sincronizar bases de datos » Este tutorial usa los valores y nombres de ejemplo de una empresa llamada Contoso.
> - Reemplácelos con sus propios valores.
> - Por ejemplo:
> - Nombre del controlador de dominio: **mimservername**
> - Nombre de dominio: **contoso**

Contraseña: **Pass@word1**


## Nombre de la cuenta de servicio: **MIMService**

1. Si no ha configurado el paquete de instalación de MIM en el último paso, vuelva e instale los componentes de Microsoft Identity Manager 2016 antes de continuar.

2. Configuración del servicio y el portal de MIM para la instalación

3. Ejecute el **instalador del servicio y el portal de MIM** desde la subcarpeta **Service and Portal** (Servicio y portal) desempaquetada.

4. En la pantalla de bienvenida, haga clic en **Siguiente**.

5. Lea el contrato de licencia para el usuario final y haga clic en **Siguiente** si acepta los términos de licencia. En la pantalla **MIM Customer Experience Improvement Program** (Programa para la mejora de la experiencia del usuario de MIM), haga clic en **Siguiente**.

6. Al seleccionar las características del componente en esta implementación, debe incluir tanto las del servicio MIM (salvo para los informes de MIM) como del portal de MIM.

    ![También puede seleccionar el portal de restablecimiento de contraseña de MIM y el servicio de notificación de cambio de contraseña de MIM.](media/MIM-Install10.png)

7. En la página **Configure the MIM database connection** (Configurar la conexión de la base de datos de MIM), elija **Create a new database** (Crear una base de datos nueva). Imagen de configuración de la conexión de base de datos de MIM En **Configure mail server connection** (Configurar conexión del servidor de correo), indique el nombre de su servidor Exchange como **Servidor de correo**.

    ![Si no tiene configurado ningún servidor de correo, use **localhost** como el nombre del servidor de correo y desactive las dos casillas superiores.](media/MIM-Install11.png)

8. Haga clic en **Siguiente**.

9. Imagen de configuración de la conexión del servidor de correo

    ![Especifique que desea generar un nuevo certificado autofirmado o seleccione el certificado correspondiente.](media/MIM-Install12.png)

10. Especifique el nombre de la cuenta de servicio que se debe usar, como, por ejemplo, *MIMService*, y la contraseña de dicha cuenta, como, por ejemplo, *Pass@word1*, e indique también el dominio de la cuenta de servicio, como *contoso*, y la cuenta de correo electrónico de servicio, como, por ejemplo, *contoso*.

11. Imagen de configuración de la cuenta de servicio MIM

    ![Tenga en cuenta que puede aparecer una advertencia que indique que la cuenta de servicio no es segura en su configuración actual.](media/MIM-Install13.png)

12. Acepte los valores predeterminados para la ubicación del servidor de sincronización y especifique la cuenta del agente de administración de MIM como *contoso\MIMsync*.

13. Imagen de configuración del servicio y el portal de MIM

14. Especifique *CORPIDM* (nombre de este equipo) como dirección del servidor de servicio MIM para el portal de MIM.

15. Especifique *http://CorpIDM.contoso.local:82* como la dirección URL de la colección de sitios de SharePoint.

16. Especifique *http://CorpIDM.contoso.local:8080* como la dirección URL de registro de contraseñas.

## Especifique *http://CorpIDM.contoso.local:8088* como la dirección URL de restablecimiento de contraseñas.

1.  Seleccione la casilla para abrir los puertos 5725 y 5726 en el firewall y la casilla para conceder acceso al Portal de MIM a todos los usuarios autenticados.

2.  Configuración del portal de registro de contraseñas de MIM Establezca el nombre de la cuenta de servicio del registro de SSPR como *contoso\MIMSSPR* y su contraseña, como *Pass@word1*.

    ![Especifique *CORPIDM* como el nombre de host para el registro de contraseñas de MIM y establezca el puerto en **8080**.](media/MIM-Install14.png)

3.  Habilite la opción **Open port in firewall** (Abrir puerto en el firewall).

4. Imagen de especificación de la información de configuración que usa IIS

## Se mostrará una advertencia, léala y haga clic en **Siguiente**.

1.  En la siguiente pantalla de configuración del portal de registro de contraseñas de MIM, especifique *http://CorpIDM.contoso.local* como dirección del servidor de servicio MIM para el portal de registro de contraseñas.

2.  Configuración del portal de restablecimiento de contraseña de MIM Establezca el nombre de la cuenta de servicio del registro de SSPR como *Contoso\MIMSSPRService* y su contraseña como *Pass@word1*.

    ![Especifique *CORPIDM* como el nombre de host para el registro de contraseñas de MIM y establezca el puerto en **8080**.](media/MIM-Install15.png)

3.  Habilite la opción **Open port in firewall** (Abrir puerto en el firewall).

4. Imagen de especificación de la información de configuración que usa IIS

## Se mostrará una advertencia, léala y haga clic en **Siguiente**.

En la siguiente pantalla de configuración del portal de registro de contraseñas de MIM, especifique *CorpIDname  http://CorpIDname.domain.local* como dirección del servidor de servicio MIM para el portal de restablecimiento de contraseñas.

Instalar el Servicio y el Portal de MIM

1. Cuando todas las definiciones de preinstalación estén listas, haga clic en **Instalar** para empezar a instalar los componentes **servicio y portal** seleccionados. Una vez finalizada la instalación, compruebe que el Portal de MIM esté activo.

    - Inicie Internet Explorer y conéctese al portal de MIM en *http://corpidm.contoso.local:82/identitymanagement*.

2. Tenga en cuenta que puede haber un breve retraso la primera vez que se visite esta página.  Si fuese necesario, autentíquese como *contoso\Administrador* en Internet Explorer.

3. En Internet Explorer, abra el cuadro de diálogo **Opciones de Internet**, vaya a la pestaña **Seguridad** y agregue el sitio a la zona **Intranet local** si todavía no está en ella.

    1.  Cierre el cuadro de diálogo **Opciones de Internet** .

    2.  Permita a los usuarios ver su propia entrada en MIM.

    3.  Mediante Internet Explorer, en **Portal de MIM**, haga clic en **Reglas de directiva de administración**.

    4.  Busque la regla de directiva de administración **User management: Users can read attributes of their own** (Administración de usuarios: los usuarios pueden leer sus propios atributos).

4.  Seleccione esta regla de directiva de administración; desactive la opción **La directiva está deshabilitada**.

    1.  Haga clic en **Aceptar** y después en **Guardar**.

    2.  Compruebe que el firewall permita conexiones entrantes para el puerto TCP 5725 y 5726.

    3.  Inicie **Herramientas administrativas » Firewall de Windows** con **Seguridad avanzada**.

        -   Haga clic en **Reglas de entrada**.

        -   Compruebe que aparezcan las dos reglas siguientes:

    4.  Servicio Forefront Identity Manager (STS).

    5.  Servicio Forefront Identity Manager (Servicio web).

    6.  Complete el asistente y cierre la aplicación **Firewall de Windows** .

    7.  Inicie **Panel de control » Red e Internet » Ver el estado y las tareas de red**.

> Compruebe que se muestre una red activa como contoso.local como red de dominios.

>Cierre el **Panel de control**.  
Opcional: en este momento puede instalar extensiones y complementos de MIM.


<!--HONumber=Apr16_HO3-->


