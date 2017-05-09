---
title: "Instalación del servicio y el portal de Microsoft Identity Manager | Microsoft Docs"
description: "Obtención de los pasos para configurar e instalar el servicio y el portal de MIM de Microsoft Identity Manager 2016"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 54d03fbd03f6c44298139324ea2dc7d945f008bc
ms.openlocfilehash: eb572bc3f062763265a9f24910a82be0cac77a5e
ms.lasthandoff: 01/24/2017


---

# <a name="install-mim-2016-mim-service-and-portal"></a>Instalación de MIM 2016: servicio y portal de MIM

>[!div class="step-by-step"]
[« MIM Synchronization Service](install-mim-sync.md)
[Sincronizar bases de datos »](install-mim-sync-ad-service.md)

> [!NOTE]
> Este tutorial usa los valores y nombres de ejemplo de una empresa llamada Contoso. Reemplácelos con sus propios valores. Por ejemplo:
> - Nombre del controlador de dominio: **mimservername**
> - Nombre de dominio: **contoso**
> - Contraseña - **Pass@word1**
> - Nombre de la cuenta de servicio: **MIMService**

Si no ha configurado el paquete de instalación de MIM en el último paso, vuelva e instale los componentes de Microsoft Identity Manager 2016 antes de continuar.


## <a name="configure-mim-service-and-portal-for-installation"></a>Configuración del servicio y el portal de MIM para la instalación

1. Ejecute el **instalador del servicio y el portal de MIM** desde la subcarpeta **Service and Portal** (Servicio y portal) desempaquetada.

2. En la pantalla de bienvenida, haga clic en **Siguiente**.

3. Lea el contrato de licencia para el usuario final y haga clic en **Siguiente** si acepta los términos de licencia.

4. En la pantalla **MIM Customer Experience Improvement Program** (Programa para la mejora de la experiencia del usuario de MIM), haga clic en **Siguiente**.

5. Al seleccionar las características del componente en esta implementación, debe incluir tanto las del servicio MIM (salvo para los informes de MIM) como del portal de MIM. También puede seleccionar el portal de restablecimiento de contraseña de MIM y el servicio de notificación de cambio de contraseña de MIM.

6. En la página **Configure the MIM database connection** (Configurar la conexión de la base de datos de MIM), elija **Create a new database** (Crear una base de datos nueva).

    ![Imagen de configuración de la conexión de base de datos de MIM](media/MIM-Install10.png)

7. En **Configure mail server connection** (Configurar conexión del servidor de correo), indique el nombre de su servidor Exchange como **Servidor de correo**. Si no tiene configurado ningún servidor de correo, use **localhost** como el nombre del servidor de correo y desactive las dos casillas superiores. Haga clic en **Siguiente**.

    ![Imagen de configuración de la conexión del servidor de correo](media/MIM-Install11.png)

8. Especifique que desea generar un nuevo certificado autofirmado o seleccione el certificado correspondiente.

9. Especifique el nombre de la cuenta de servicio que se debe usar, como, por ejemplo, *MIMService*, y la contraseña de dicha cuenta, como, por ejemplo, *Pass@word1*, e indique también el dominio de la cuenta de servicio, como *contoso*, y la cuenta de correo electrónico de servicio, como, por ejemplo, *contoso*.

    ![Imagen de configuración de la cuenta de servicio MIM](media/MIM-Install12.png)

10. Tenga en cuenta que puede aparecer una advertencia que indique que la cuenta de servicio no es segura en su configuración actual.

11. Acepte los valores predeterminados para la ubicación del servidor de sincronización y especifique la cuenta del agente de administración de MIM como *contoso\MIMsync*.

    ![Imagen de configuración del servicio y el portal de MIM](media/MIM-Install13.png)

12. Especifique *CORPIDM* (nombre de este equipo) como dirección del servidor de servicio MIM para el portal de MIM.

13. Especifique *http://CorpIDM.contoso.local:82* como la dirección URL de la colección de sitios de SharePoint.

14. Especifique *http://CorpIDM.contoso.local:8080* como la dirección URL de registro de contraseñas.

15. Especifique *http://CorpIDM.contoso.local:8088* como la dirección URL de restablecimiento de contraseñas.

16. Seleccione la casilla para abrir los puertos 5725 y 5726 en el firewall y la casilla para conceder acceso al Portal de MIM a todos los usuarios autenticados.

## <a name="configure-mim-password-registration-portal"></a>Configuración del portal de registro de contraseñas de MIM

1.  Establezca el nombre de la cuenta de servicio del registro de SSPR como *contoso\MIMSSPR* y su contraseña, como *Pass@word1*.

2.  Especifique *CORPIDM* como el nombre de host para el registro de contraseñas de MIM y establezca el puerto en **8080**. Habilite la opción **Open port in firewall** (Abrir puerto en el firewall).

    ![Imagen de especificación de la información de configuración que usa IIS](media/MIM-Install14.png)

3.  Se mostrará una advertencia, léala y haga clic en **Siguiente**.

4. En la siguiente pantalla de configuración del portal de registro de contraseñas de MIM, especifique *http://CorpIDM.contoso.local* como dirección del servidor de servicio MIM para el portal de registro de contraseñas.

## <a name="configure-mim-password-reset-portal"></a>Configuración del portal de restablecimiento de contraseña de MIM

1.  Establezca el nombre de la cuenta de servicio del registro de SSPR como *Contoso\MIMSSPRService* y su contraseña, como *Pass@word1*.

2.  Especifique *CORPIDM* como el nombre de host para el portal de restablecimiento de contraseñas de MIM y establezca el puerto en **8088**. Habilite la opción **Open port in firewall** (Abrir puerto en el firewall).

    ![Imagen de especificación de la información de configuración que usa IIS](media/MIM-Install15.png)

3.  Se mostrará una advertencia, léala y haga clic en **Siguiente**.

4. En la siguiente pantalla de configuración del portal de registro de contraseñas de MIM, especifique *CorpIDname  http://CorpIDname.domain.local* como dirección del servidor de servicio MIM para el portal de restablecimiento de contraseñas.

## <a name="install-mim-service-and-portal"></a>Instalar el Servicio y el Portal de MIM

Cuando todas las definiciones de preinstalación estén listas, haga clic en **Instalar** para empezar a instalar los componentes **servicio y portal** seleccionados.

Una vez finalizada la instalación, compruebe que el Portal de MIM esté activo.

1. Inicie Internet Explorer y conéctese al portal de MIM en *http://corpidm.contoso.local:82/identitymanagement*. Tenga en cuenta que puede haber un breve retraso la primera vez que se visite esta página.

    - Si fuese necesario, autentíquese como *contoso\Administrador* en Internet Explorer.

2. En Internet Explorer, abra el cuadro de diálogo **Opciones de Internet**, vaya a la pestaña **Seguridad** y agregue el sitio a la zona **Intranet local** si todavía no está en ella.  Cierre el cuadro de diálogo **Opciones de Internet** .

3. Permita a los usuarios ver su propia entrada en MIM.

    1.  Mediante Internet Explorer, en **Portal de MIM**, haga clic en **Reglas de directiva de administración**.

    2.  Busque la regla de directiva de administración **User management: Users can read attributes of their own** (Administración de usuarios: los usuarios pueden leer sus propios atributos).

    3.  Seleccione esta regla de directiva de administración; desactive la opción **La directiva está deshabilitada**.

    4.  Haga clic en **Aceptar** y después en **Guardar**.

4.  Compruebe que el firewall permita conexiones entrantes para el puerto TCP 5725 y 5726.

    1.  Inicie **Herramientas administrativas » Firewall de Windows** con **Seguridad avanzada**.

    2.  Haga clic en **Reglas de entrada**.

    3.  Compruebe que aparezcan las dos reglas siguientes:

        -   Servicio Forefront Identity Manager (STS).

        -   Servicio Forefront Identity Manager (Servicio web).

    4.  Complete el asistente y cierre la aplicación **Firewall de Windows** .

    5.  Inicie **Panel de control » Red e Internet » Ver el estado y las tareas de red**.

    6.  Compruebe que se muestre una red activa como contoso.local como red de dominios.

    7.  Cierre el **Panel de control**.

> [!NOTE]
> Opcional: en este momento puede instalar extensiones y complementos de MIM.

>[!div class="step-by-step"]  
[« MIM Synchronization Service](install-mim-sync.md)
[Sincronizar bases de datos »](install-mim-sync-ad-service.md)

