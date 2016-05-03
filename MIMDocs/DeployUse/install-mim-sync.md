---
# required metadata

title: Instalación de MIM 2016&#58; servicio de sincronización de MIM | Microsoft Identity Manager
description: Comience a utilizar los componentes de MIM 2016 instalando y configurando el servicio de sincronización.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Instalación de MIM 2016: servicio de sincronización de MIM

>[!div class="step-by-step"]
[« Exchange Server](prepare-server-exchange.md)
[Servicio y portal de MIM »](install-mim-service-portal.md)

> [!NOTE]
> En todos los ejemplos siguientes, **mimservername** representa el nombre del controlador de dominio; **contoso**, el nombre de dominio y **Pass@word1**, una contraseña de ejemplo.

Antes de instalar los componentes de Microsoft Identity Manager 2016:

1. Inicie sesión como *contoso\Administrador* en el servidor CORPIDM que usa para la administración de identidades.

2. Desempaquete el paquete de instalación de MIM o monte el DVD con la imagen de MIM.

## Instalación del servicio de sincronización de MIM 2016

1. En la carpeta de instalación de MIM, vaya a la carpeta **Synchronization Service** .

2. Ejecute el **instalador del servicio de sincronización de MIM**. Siga las instrucciones del instalador y complete la instalación.

3. En la pantalla de bienvenida, haga clic en **Siguiente**.

    ![Imagen de la página principal del Asistente para instalación de MIM](media/MIM-Install1.png)

4. Revise los términos de licencia y si los acepta, haga clic en **Siguiente**.

5. En la pantalla **Instalación personalizada**, haga clic en **Siguiente**.

    ![Imagen de instalación personalizada](media/MIM-Install2.png)

6.  En la pantalla de configuración de la base de datos de sincronización, seleccione:

    1.  El SQL Server se encuentra en: **Este equipo**.

    2.  La instancia de SQL Server es: **La instancia predeterminada**.

    ![Imagen de conexión de base de datos](media/MIM-Install3.png)

7.  Configure la cuenta de Servicio de sincronización de acuerdo con la cuenta que creó anteriormente:

    1.  Cuenta de servicio: *MIMSync*

    2.  Contraseña: *Pass@word1*

    3.  Dominio de la cuenta de servicio o nombre del equipo local: *contoso*

    ![Imagen de cuenta de servicio](media/MIM-Install4.png)

8.  Proporcione al instalador del Servicio de sincronización de MIM los grupos de seguridad pertinentes:

    1.  Administrador = *contoso\MIMSyncAdmins*

    2.  Operador = *contoso\MIMSyncOperators*

    3.  Unión = *contoso\MIMSyncJoiners*

    4.  Búsqueda de conector = *contoso\MIMSyncBrowse*

    5.  Administración de contraseñas de WMI = *contoso\MIMSyncPasswordReset*

    ![Imagen de grupos de seguridad](media/MIM-Install5.png)

9. En la pantalla de configuración de seguridad, active **Enable firewall rules for inbound RPC communications** (Habilitar reglas de firewall para las comunicaciones RPC entrantes) y haga clic en **Siguiente**.

10. Haga clic en **Instalar** para empezar la instalación del servicio de sincronización de MIM.

    1.  Puede que se muestre una advertencia relacionada con la cuenta del Servicio de sincronización de MIM. Si se muestra, haga clic en **Aceptar**.

    2.  El Servicio de sincronización de MIM se instalará ahora.

        ![Imagen del estado de la instalación del servicio de sincronización de MIM](media/MIM-Install6.png)

    3.  Se mostrará un aviso sobre la creación de una copia de seguridad de la clave de cifrado. Haga clic en **Aceptar** y seleccione una carpeta para almacenar la copia de seguridad de la clave de cifrado.

        ![Imagen de aviso de copia de seguridad de la clave de cifrado del servicio de sincronización de MIM](media/MIM-Install7.png)

    4.  Cuando el instalador complete la instalación correctamente, haga clic en **Finalizar**.

        ![Imagen de instalación correcta del servicio de sincronización de MIM](media/MIM-Install8.png)

    5.  Debe cerrar la sesión y volver a iniciarla para que surtan efecto los cambios de pertenencia a grupo. Haga clic en **Sí** para cerrar la sesión.

>[!div class="step-by-step"]  
[« Exchange Server](prepare-server-exchange.md)
[Servicio y portal de MIM »](install-mim-service-portal.md)


<!--HONumber=Apr16_HO2-->


