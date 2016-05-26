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

> [!NOTE]
> « Exchange Server Servicio y portal de MIM » Este tutorial usa los valores y nombres de ejemplo de una empresa llamada Contoso.
> - Reemplácelos con sus propios valores.
> - Por ejemplo:
> - Nombre del controlador de dominio: **mimservername**

Nombre de dominio: **contoso**

1. Contraseña: **Pass@word1**

2. Antes de instalar los componentes de Microsoft Identity Manager 2016, configure el paquete de instalación.

## Inicie sesión como *contoso\Administrador* en el servidor que usa para la administración de identidades.

1. Desempaquete el paquete de instalación de MIM o monte el DVD con la imagen de MIM.

2. Instalar el Servicio de sincronización de MIM 2016 En la carpeta de instalación de MIM, vaya a la carpeta **Synchronization Service** .

3. Ejecute el **instalador del servicio de sincronización de MIM**.

    ![Siga las instrucciones del instalador y complete la instalación.](media/MIM-Install1.png)

4. En la pantalla de bienvenida, haga clic en **Siguiente**.

5. Imagen de la página principal del Asistente para instalación de MIM

    ![Revise los términos de licencia y haga clic en **Siguiente** para aceptarlos.](media/MIM-Install2.png)

6.  En la pantalla **Instalación personalizada**, haga clic en **Siguiente**.

    1.  Imagen de instalación personalizada

    2.  En la pantalla de configuración de la base de datos de sincronización, seleccione:

    ![El SQL Server se encuentra en: **Este equipo**.](media/MIM-Install3.png)

7.  La instancia de SQL Server es: **La instancia predeterminada**.

    1.  Imagen de conexión de base de datos

    2.  Configure la cuenta de Servicio de sincronización de acuerdo con la cuenta que creó anteriormente:

    3.  Cuenta de servicio: *MIMSync*

    ![Contraseña: *Pass@word1*](media/MIM-Install4.png)

8.  Dominio de la cuenta de servicio o nombre del equipo local: *contoso*

    1. Imagen de cuenta de servicio

    2. Proporcione al instalador del Servicio de sincronización de MIM los grupos de seguridad pertinentes:

    3. Administrador = *contoso\MIMSyncAdmins*

    4. Operador = *contoso\MIMSyncOperators*

    5. Unión = *contoso\MIMSyncJoiners*

    ![Búsqueda de conector = *contoso\MIMSyncBrowse*](media/MIM-Install5.png)

9. Administración de contraseñas de WMI = *contoso\MIMSyncPasswordReset*

10. Imagen de grupos de seguridad

    1. En la pantalla de configuración de seguridad, active **Enable firewall rules for inbound RPC communications** (Habilitar reglas de firewall para las comunicaciones RPC entrantes) y haga clic en **Siguiente**.

    2. Haga clic en **Instalar** para empezar la instalación del servicio de sincronización de MIM.

    3. Puede que se muestre una advertencia relacionada con la cuenta del Servicio de sincronización de MIM. Si se muestra, haga clic en **Aceptar**.

        ![Se instalará MIM Sync.](media/MIM-Install7.png)

    4. Se mostrará un aviso sobre la creación de una copia de seguridad de la clave de cifrado. Haga clic en **Aceptar** y seleccione una carpeta para almacenar la copia de seguridad de la clave de cifrado.

    5. Imagen de aviso de copia de seguridad de la clave de cifrado del servicio de sincronización de MIM Cuando el instalador complete la instalación correctamente, haga clic en **Finalizar**.

>Debe cerrar la sesión y volver a iniciarla para que surtan efecto los cambios de pertenencia a grupo.  
Haga clic en **Sí** para cerrar la sesión.


<!--HONumber=Apr16_HO3-->


