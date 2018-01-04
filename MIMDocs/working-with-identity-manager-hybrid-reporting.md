---
title: "Trabajo con la creación de informes híbridos en Azure mediante MIM 2016 | Microsoft Docs"
description: "Descubra cómo combinar datos locales y en la nube en informes híbridos en Azure, y cómo administrar y ver estos informes."
keywords: 
author: fimguy
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 17745bfdba831364d32bc2786cc2a38191fe6cc7
ms.sourcegitcommit: e52bab207117390997c6fa8450de24335b502673
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/20/2017
---
# <a name="working-with-identity-manager-hybrid-reporting---public-preview-refresh"></a>Trabajar con la creación de informes híbrida de Identity Manager: versión preliminar pública (actualización)

## <a name="available-hybrid-reports"></a>Informes híbridos disponibles
Los tres primeros informes de Microsoft Identity Manager (MIM) disponibles en Azure AD son **Actividad de restablecimiento de contraseña**, **Registro para el restablecimiento de contraseña** y **Actividad de los grupos de autoservicio**.

-   El informe de Actividad de restablecimiento de contraseña muestra cada instancia en la que un usuario restableció la contraseña mediante el autoservicio de restablecimiento de contraseña (SSPR) y proporciona las puertas o **Métodos** usados para la autenticación.

-   El informe de Registro para el restablecimiento de contraseña muestra cada vez que un usuario se registra para el SSPR y los **Métodos** usados para la autenticación, como, por ejemplo, un número de teléfono móvil o preguntas y respuestas.
    Tenga en cuenta que en el informe de Registro para el restablecimiento de contraseña no se diferencia entre la puerta SMS y la puerta MFA: las dos se consideran como **Teléfono móvil**.

-   La actividad de los grupos de autoservicio muestra cada intento que alguien realiza para agregarse o eliminarse de un grupo, así como la creación del grupo.

    ![Imagen de actividad de restablecimiento de contraseña de la creación de informes híbridos de Azure](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> Los informes presentan actualmente datos de hasta un mes atrás.</br>
> Se debe desinstalar el agente híbrido anterior</br>
> Si quiere desinstalar los informes híbridos, desinstale el agente MIMreportingAgent.msi.

## <a name="prerequisites"></a>Requisitos previos

1.  Instale el servicio de Microsoft Identity Manager 2016 RTM o SP1 MIM.

2.  Asegúrese de que haya un inquilino de Azure AD Premium con un administrador con licencia en el directorio.

3.  Asegúrese de tener conectividad a Internet saliente desde el servidor de Microsoft Identity Manager hasta Azure.

## <a name="requirements"></a>Requisitos
En la tabla siguiente se muestra una lista de los requisitos para usar la creación de informes híbrida de Microsoft Identity Manager.

| Requisito | Descripción |
| --- | --- |
| Azure AD Premium | Creación de informes híbrida es una característica de Azure AD Premium y requiere que esté instalado. </br></br>Para más información, consulte [Introducción a Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium) </br>Para comenzar una prueba gratuita de 30 días, consulte [Iniciar una versión de prueba](https://azure.microsoft.com/trial/get-started-active-directory/). |
| Debe ser administrador global de la instancia de Azure AD para comenzar |De manera predeterminada, solo los administradores globales pueden instalar y configurar los agentes para comenzar, tener acceso al portal y realizar cualquier operación en Azure. </br></br>**Importante:** La cuenta usada para instalar los agentes debe ser una cuenta profesional o educativa. No puede ser una cuenta Microsoft. Para más información, consulte [Suscribirse a Azure como organización](https://docs.microsoft.com/azure/active-directory/sign-up-organization) |
| El agente híbrido de Microsoft Identity Manager está instalado en cada servidor del servicio MIM de destino | La generación de informes híbrida requiere que los agentes estén instalados y configurados en servidores de destino para recibir los datos y proporcionar las funcionalidades de análisis y supervisión </br>|
| Conectividad saliente a los extremos del servicio de Azure | Durante la instalación y el tiempo de ejecución, el agente requiere conectividad a los puntos de conexión del servicio de Azure. Si la conectividad saliente está bloqueada debido a la presencia de firewalls, asegúrese de agregar los puntos de conexión siguientes a la lista de elementos permitidos: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net, puerto: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|Conectividad saliente basada en direcciones IP | Para información sobre el filtrado basado en direcciones IP en los firewalls, consulte los [intervalos IP de Azure](https://www.microsoft.com/download/details.aspx?id=41653).|
| La inspección de SSL del tráfico de salida está filtrada o deshabilitada | Las operaciones de carga de datos o el paso de registro del agente pueden generar errores si existe inspección de SSL o la finalización del tráfico de salida en el nivel de red. |
| Puertos de firewall en el servidor que ejecuta el agente. |El agente requiere que los siguientes puertos de firewall estén abiertos para poder comunicarse con los puntos de conexión del servicio de Azure.</br></br><li>Puerto TCP 443</li><li>Puerto TCP 5671</li> |
| Admitir los siguientes sitios web si la seguridad mejorada de Internet Explorer está habilitada |Si la seguridad mejorada de IE está habilitada, los siguientes sitios web deben permitirse en el servidor donde se va a instalar el agente.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>El servidor de federación de su organización de confianza para Azure Active Directory. Por ejemplo: https://sts.contoso.com</li> |
</BR>

## <a name="install-microsoft-identity-manager-reporting-agent-in-azure-ad"></a>Instalación del agente de informes de Microsoft Identity Manager en Azure AD
Una vez que se ha instalado el agente de informes, los datos de actividad de Microsoft Identity Manager se exportan de MIM al registro de eventos de Windows. El agente de informes de MIM procesa los eventos y los carga en Azure. En Azure los eventos se analizan, descifran y filtran para los informes requeridos.

1.  Instale Microsoft Identity Manager 2016.

2.  Descargue los agentes de informes de Microsoft Identity Manager:

    1.  Inicie sesión en el portal de administración de Azure AD y haga clic en el icono de Active Directory.

    2.  Haga doble clic en el directorio del que sea administrador global y para el que disponga de una suscripción de Azure AD Premium.

    3.  Haga clic en **Configuración** y descargue el agente de informes.

3.  Instale el agente de informes de Microsoft Identity Manager:

    1.  Descargue [MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) en el servidor de servicio de Microsoft Identity Manager.
    2.  Ejecute `MIMHReportingAgentSetup.exe`: 
    3.  Ejecute el instalador del agente.

    4.  Asegúrese de que se esté ejecutando el servicio del agente de informes de MIM.

    5.  Reinicie el servicio MIM.

4.  Compruebe que Informes de Microsoft Identity Manager funcione en Azure.

    Puede crear datos de informes mediante el Portal de autoservicio de restablecimiento de contraseña de Microsoft Identity Manager para restablecer la contraseña de un usuario. Asegúrese de que el restablecimiento de la contraseña se completó correctamente y compruebe después que los datos se muestran en el portal de administración de Azure AD.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Vista de informes híbridos en Azure Portal

1.  Inicie sesión en [Azure Portal](https://portal.azure.com/) con su cuenta de administrador global del inquilino.

2.  Haga clic en el icono de **Azure Active Directory** .

3.  Seleccione el directorio del inquilino de la lista de directorios disponibles para su suscripción.

4.  Haga clic en **Registros de auditoría**.

5.  Asegúrese de seleccionar **Servicio MIM** en el menú desplegable Categoría.

> [!WARNING]
> Puede que tarden un poco en mostrarse los datos de auditoría de Microsoft Identity Manager en Azure Portal.

## <a name="stop-creating-hybrid-reports"></a>Detención de la creación de informes híbridos
Si quiere que se dejen de cargar datos de auditoría de informes de Microsoft Identity Manager en Azure Active Directory, desinstale el agente de informes híbridos. Mediante la herramienta **Agregar o quitar programas**, desinstale la herramienta de creación de informes híbridos de Microsoft Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Eventos de Windows empleados para la creación de informes híbridos
Los eventos generados por Microsoft Identity Manager se guardan en el registro de eventos de Windows y pueden verse en el Visor de eventos, en Registros de aplicaciones y servicios-&gt; **Registro de solicitudes de Identity Manager**. Cada solicitud de MIM se exporta como un evento en el Registro de eventos de Windows en la estructura de JSON. Esto se puede exportar a su SIEM.

|Tipo de evento|ID|Detalles del evento|
|--------------|------|-----------------|
|Información de|4121|Datos de evento de MIM que incluyen todos los datos de solicitud.|
|Información de|4137|Extensión 4121 de evento de MIM, en caso de que haya demasiados datos para un evento único. El encabezado de este evento tiene la siguiente forma: `"Request: <GUID> , message <xxx> out of <xxx>`|
