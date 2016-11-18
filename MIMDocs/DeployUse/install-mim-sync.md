---
title: "Instalación del servicio de sincronización de MIM | Microsoft Docs"
description: "Comience a utilizar los componentes de MIM 2016 instalando y configurando el servicio de sincronización."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 08/11/2016
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: dc1f7ff40ed5f657c24e7293ff76241c3a7082f1


---

# <a name="install-mim-2016-mim-synchronization-service"></a>Instalación de MIM 2016: servicio de sincronización de MIM

>[!div class="step-by-step"]
[« Exchange Server](prepare-server-exchange.md)
[Servicio y portal de MIM »](install-mim-service-portal.md)

> [!NOTE]
> Este tutorial usa los valores y nombres de ejemplo de una empresa llamada Contoso. Reemplácelos con sus propios valores. Por ejemplo:
> - Nombre del controlador de dominio: **mimservername**
> - Nombre de dominio: **contoso**
> - Contraseña - **Pass@word1**

Antes de instalar los componentes de Microsoft Identity Manager 2016, configure el paquete de instalación.

1. Inicie sesión como *contoso\Administrador* en el servidor que usa para la administración de identidades.

2. Desempaquete el paquete de instalación de MIM o monte el DVD con la imagen de MIM.

## <a name="install-mim-2016-synchronization-service"></a>Instalar el Servicio de sincronización de MIM 2016

1. En la carpeta de instalación de MIM, vaya a la carpeta **Synchronization Service** .

2. Ejecute el **instalador del servicio de sincronización de MIM**. Siga las instrucciones del instalador y complete la instalación.

3. En la pantalla de bienvenida, haga clic en **Siguiente**.

    ![Imagen de la página principal del Asistente para instalación de MIM](media/MIM-Install1.png)

4. Revise los términos de licencia y haga clic en **Siguiente** para aceptarlos.

5. En la pantalla **Instalación personalizada**, haga clic en **Siguiente**.

    ![Imagen de instalación personalizada](media/MIM-Install2.png)

6.  En la pantalla de configuración de la base de datos del servicio de sincronización, seleccione:

    1.  El SQL Server se encuentra en: **Este equipo**.

    2.  La instancia de SQL Server es: **La instancia predeterminada**.

    ![Imagen de conexión de base de datos](media/MIM-Install3.png)

7.  Configure la cuenta de Servicio de sincronización de acuerdo con la cuenta que creó anteriormente:

    1.  Cuenta de servicio: *MIMSync*

    2.  Contraseña:*Pass@word1*

    3.  Dominio de la cuenta de servicio o nombre del equipo local: *contoso*

    ![Imagen de cuenta de servicio](media/MIM-Install4.png)

8.  Proporcione al instalador del servicio de sincronización de MIM los grupos de seguridad pertinentes:

    1. Administrador = *contoso\MIMSyncAdmins*

    2. Operador = *contoso\MIMSyncOperators*

    3. Unión = *contoso\MIMSyncJoiners*

    4. Búsqueda de conector = *contoso\MIMSyncBrowse*

    5. Administración de contraseñas de WMI = *contoso\MIMSyncPasswordReset*

    ![Imagen de grupos de seguridad](media/MIM-Install5.png)

9. En la pantalla de configuración de seguridad, active **Enable firewall rules for inbound RPC communications** (Habilitar reglas de firewall para las comunicaciones RPC entrantes) y haga clic en **Siguiente**.

10. Haga clic en **Instalar** para empezar la instalación del servicio de sincronización de MIM.

    1. Puede que se muestre una advertencia relacionada con la cuenta del Servicio de sincronización de MIM. Si se muestra, haga clic en **Aceptar**.

    2. Se instalará el servicio de sincronización de MIM.

    3. Se mostrará un aviso sobre la creación de una copia de seguridad de la clave de cifrado. Haga clic en **Aceptar** y seleccione una carpeta para almacenar la copia de seguridad de la clave de cifrado.

        ![Imagen de aviso de copia de seguridad de la clave de cifrado del servicio de sincronización de MIM](media/MIM-Install7.png)

    4. Cuando el instalador complete la instalación correctamente, haga clic en **Finalizar**.

    5. Debe cerrar la sesión y volver a iniciarla para que surtan efecto los cambios de pertenencia a grupo. Haga clic en **Sí** para cerrar la sesión.

>[!div class="step-by-step"]  
[« Exchange Server](prepare-server-exchange.md)
[Servicio y portal de MIM »](install-mim-service-portal.md)



<!--HONumber=Nov16_HO2-->


