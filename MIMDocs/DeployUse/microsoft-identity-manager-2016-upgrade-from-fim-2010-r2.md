---
# required metadata

title: Actualización desde Forefront Identity Manager 2010 R2 | Microsoft Identity Manager
description: Obtenga información acerca de cómo actualizar los componentes de FIM 2010 R2 para instalar después los nuevos en MIM 2016.
keywords:
author: kgremban
manager: stevenpo
ms.date: 05/13/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Actualización desde Forefront Identity Manager 2010 R2

Si tiene un entorno de Forefront Identity Manager (FIM) 2010 R2 y quiere probar Microsoft Identity Manager (MIM) 2016, use este artículo como guía. Hay tres fases en esta actualización:

1.  Instale MIM Synchronization Service (Sync) en un servidor que esté unido al dominio de Active Directory (AD). Esto reemplaza la instancia de Sync de FIM 2010 R2.

2.  Instale el servicio y el portal de MIM. En este momento, también puede elegir instalar el portal de registro y el portal de servicio de autoservicio de restablecimiento de contraseña (SSPR). y, al excluir el conjunto de características de Privileged Access Management, se instalará.

3.  Implemente las extensiones y complementos de MIM en un equipo cliente independiente. Esto incluye el cliente de inicio de sesión de Windows de SSPR integrado.


Esta guía asume que ya ha configurado lo siguiente:
- FIM 2010 R2 implementado en un entorno de prueba
- Servidores ejecutándose en Windows Server 2012, Windows Server 2012 R2 o Windows Server 2008 R2
- Requisitos previos locales y de entorno (SQL Server, Exchange Server, SharePoint Services, etc.) que están configurados para FIM 2010 R2.


## Preparación

1.  Realice una copia de la base de datos del servicio FIM, de la base de datos de FIM Sync y de la configuración y el software de FIM Sync y el servicio FIM.

2.  Inicie sesión como Contoso\Administrador en cada uno de los servidores que tengan instalados los componentes de FIM 2010 R2; por ejemplo, *CORPIDM*. En este ejemplo de implementación, los se necesitan derechos administrativos para actualizar FIM 2010 R2 a **MIM**.

3.  Descargue o desempaquete el software de MIM.

## Actualización del servicio de sincronización

1.  Inicie sesión como administrador en un servidor en el que esté implementado el servicio de sincronización de FIM 2010 R2 ("Sync").

2.  Asegúrese de realizar una copia de la base de datos antes de iniciar este procedimiento.

3.  Abra la consola **Servicios** , busque el **servicio Sincronización de Forefront Identity Manager**y deténgalo.

    ![Imagen de la consola de servicios](media/MIM-UpgFIM1.PNG)

4.  Ejecute el **instalador del servicio de sincronización de MIM**. El programa de instalación detectará la versión de Sync existente y sugerirá una actualización. Haga clic en el botón **Actualizar** para continuar.

5.  Si acepta los términos de licencia, haga clic en **Siguiente** para continuar.

6.  Escriba la contraseña de la cuenta de servicio que utiliza Sync y haga clic en **Siguiente**.

    ![Imagen de configuración de la cuenta del servicio de sincronización de MIM](media/MIM-UpgFIM3.png)

7.  Compruebe que los nombres del grupo de seguridad son correctos y haga clic en **Siguiente**.

    ![Imagen de configuración de los grupos de seguridad del servicio de sincronización de MIM](media/MIM-UpgFIM4.png)

8.  Deje sin cambiar la casilla de las reglas de firewall para las comunicaciones RPC entrantes.

9. El instalador ya está preparado para actualizar Sync de FIM 2010 R2 a MIM. Haga clic en **Actualizar** para iniciar el proceso de actualización.

10. La actualización está en curso. No salga del instalador ni reinicie el equipo mientras la actualización esté en curso.

    ![Imagen del estado de la instalación del servicio de sincronización de MIM](media/MIM-UpgFIM7.png)

11. Durante la actualización, se muestra una advertencia referente a la actualización de la base de datos de Sync. Se recomienda hacer una copia de seguridad de la base de datos antes de comenzar.

12. Cuando la actualización finalice correctamente, haga clic en **Finalizar**.

    ![Imagen de instalación correcta del servicio de sincronización de MIM](media/MIM-UpgSP1.png)

13. Observe que el **servicio de sincronización** se ha reiniciado.

## Actualización del servicio y el portal

1.  Inicie sesión como administrador en un servidor en el que esté implementado el servicio y portal de FIM 2010 R2.

2.  Abra la consola **Servicios** , busque el **servicio Forefront Identity Manager**y deténgalo.

    ![Imagen de la consola de servicios](media/MIM-UpgFIM9.PNG)

3.  Ejecute el instalador del servicio y portal de MIM. Haga clic en el botón **Instalar** para continuar.

    ![Imagen de instalación del servicio y el portal de MIM](media/MIM-UpgSP2.png)

4.  Si acepta los términos de licencia, haga clic en **Siguiente** para continuar.

5.  En la pantalla Programa para la mejora de la experiencia del usuario de MIM, haga clic en **Siguiente** para continuar.

6.  Seleccione las características y los componentes de MIM que desea instalar y, tras ello, haga clic en **Siguiente**.

    ![Imagen de instalación personalizada](media/MIM-UpgSP4.png)

    1.  **Servicio MIM:** esta característica es obligatoria al menos en un servidor y requiere un servidor de bases de datos de SQL Server, ya sea en una ubicación compartida o en otro servidor.

    2.  **Portal MIM:** Esta característica es obligatoria en al menos un servidor y requiere SharePoint 2013 Foundation.

    3.  **Portal de registro de contraseñas de MIM**: esta característica es necesaria para el autoservicio de restablecimiento de contraseña.

    4.  **Portal de restablecimiento de contraseñas de MIM:** Esta característica es necesaria para restablecer las contraseñas.

7.  Proporcione los detalles del servidor SQL Server que se utiliza para la base de datos del servicio FIM. Seleccione la opción de volver a usar la base de datos existente y conservar los datos. Haga clic en **Siguiente** para continuar.

8. Si se ha seleccionado la opción de volver a usar la base de datos existente, aparece un aviso para realizar una copia de seguridad de la base de datos.

9. Escriba los detalles del servidor de correo. Si el servidor de correo se encuentra en el servidor actual, escriba "localhost" como ubicación del servidor de correo. Haga clic en **Siguiente** para continuar.

    ![Imagen de configuración de la conexión del servidor de correo](media/MIM-UpgSP6.png)

10. Seleccione un certificado para el servicio que se usará para validar clientes. Debe usar el certificado existente desde el almacén de certificados local que usaba el servicio FIM.

    ![Imagen de configuración del certificado del servicio](media/MIM-UpgSP7.png)

    1.  Si está seleccionada la opción de almacén de certificados local, haga clic en el botón **Seleccionar certificado** y elija uno en la lista de la ventana emergente. Haga clic en **Aceptar** y, a continuación, en **Siguiente**.

        ![Imagen de la ventana emergente Seleccione un certificado](media/MIM-UpgSP8.PNG)

11. Configure las credenciales de la cuenta de servicio para el servicio MIM. Tenga en cuenta que la cuenta de servicio no puede ser la misma que utiliza el servicio de sincronización. Debe ser la misma cuenta que utiliza el servicio FIM.

    ![Imagen de configuración de la cuenta de servicio MIM](media/MIM-UpgSP9.png)

12. Configure los detalles del servidor de sincronización de MIM según la implementación del servicio MIM que configuró en un paso anterior.

    ![Imagen de configuración de la sincronización del servicio y el portal de MIM](media/MIM-UpgSP10.png)

13. Al instalar el portal de MIM, proporcione la dirección del servidor del servicio MIM. Haga clic en **Siguiente**.

14. Al instalar el portal de MIM, proporcione la dirección URL de la colección de sitios de SharePoint en la que se aloja actualmente el portal de FIM. Haga clic en **Siguiente**.

## Instalación del portal de registro de contraseñas de MIM

1. Si va a instalar el portal de registro de contraseñas de MIM, proporcione la dirección URL solicitada para el portal de registro de contraseñas. Haga clic en **Siguiente**.

2. Configure la capacidad de los clientes y usuarios finales para utilizar el portal y el servicio.

    1.  Seleccione si desea **abrir los puertos 5725 y 5726 en el firewall**.

    2.  Active, si lo desea, la opción **Grant authenticated users access to the MIM Portal site** (Permitir que los usuarios autenticados tengan acceso al sitio del portal de MIM).

    3.  Haga clic en **Siguiente**.

3. Proporcione los datos de acceso y las credenciales para el registro de contraseñas de MIM.

    ![Imagen de configuración del portal de registro de contraseñas de MIM](media/MIM-UpgSP15.png)

    1.  Proporcione el nombre de la cuenta de servicio (incluido el dominio) y la contraseña para el registro de contraseñas de MIM.

    2.  Proporcione los detalles del host: nombre y puerto (por ejemplo, 8080) del portal de registro de contraseñas.

    3.  Seleccione la opción de **abrir puertos en el firewall** .

    4.  Haga clic en **Siguiente**.

4. En la siguiente pantalla de configuración del registro de contraseñas de MIM:

    1.  Indique al registro de contraseña de MIM dónde está instalado el servicio MIM; normalmente, en el mismo sistema.

    2.  Determine si pueden acceder a este portal los usuarios de la extranet y los de la intranet o solo los usuarios de la intranet, tal como lo configurara anteriormente para el restablecimiento de contraseñas de FIM.

## Instalación del portal de restablecimiento de contraseñas de MIM

1. Si va a instalar el portal de restablecimiento de contraseñas de MIM, proporcione los datos y las credenciales de acceso al restablecimiento de contraseñas de MIM.

    ![Imagen de configuración del portal de restablecimiento de contraseñas de MIM](media/MIM-UpgSP17.png)

    1.  Proporcione el nombre de la cuenta de servicio (incluido el dominio) y la contraseña para el restablecimiento de contraseñas de MIM.

    2.  Proporcione los detalles del host: nombre y puerto (por ejemplo, 8088) del portal de restablecimiento de contraseñas.

    3.  Seleccione la opción de **abrir puertos en el firewall** .

    4.  Haga clic en **Siguiente**.

2. En la siguiente pantalla de configuración del restablecimiento de contraseñas de MIM:

    1.  Indique al restablecimiento de contraseña de MIM dónde está instalado el servicio MIM.

    2.  Especifique si pueden acceder a este portal los usuarios de la extranet y los de la intranet o solo los usuarios de la intranet.

## Finalización de la instalación y la actualización

1. Cuando haya realizado correctamente todas las definiciones de configuración, aparecerá una pantalla de instalación. Haga clic en **Instalar** para comenzar la instalación y actualización del servicio y portal de MIM.

2. La instalación y la actualización del servicio y el portal de MIM están en curso. No cancele el programa de instalación ni reinicie el equipo durante la instalación.

3. Una vez completada correctamente la instalación (o actualización) del servicio y el portal de MIM, aparecerá una pantalla de confirmación. Haga clic en **Finalizar** para completar la instalación y salir del instalador.

4. Observe que el **servicio Forefront Identity Manager** se reinicia.

Nota: Si las extensiones y los complementos de FIM están actualmente implementados en los equipos de los usuarios para SSPR, no configure las nuevas puertas de teléfono de MFA para el restablecimiento de contraseñas mientras no se hayan actualizado todos los complementos y extensiones de FIM a MIM 2016.  Dado que los complementos y las extensiones de FIM 2010 y de FIM 2010 R2 no reconocen las nuevas puertas, se generará un error y no podrá finalizar el restablecimiento de las contraseñas.


<!--HONumber=May16_HO3-->


