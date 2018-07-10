---
title: Trabajo con informes híbridos en Azure con Identity Manager 2016 | Microsoft Docs
description: Descubra cómo combinar datos locales y en la nube en informes híbridos en Azure, y cómo administrar y ver estos informes.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 2/20/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: 3c9e8c0fa0a0de3cf9710003d4d7f4ed9c0b03bd
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289652"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Trabajo con informes híbridos en Identity Manager

En este artículo se explica cómo combinar datos locales y en la nube en informes híbridos en Azure, y cómo administrar y ver estos informes.

## <a name="available-hybrid-reports"></a>Informes híbridos disponibles
Los tres primeros informes de Microsoft Identity Manager disponibles en Azure Active Directory (Azure AD) son los siguientes:

- **Actividad de restablecimiento de contraseña**: muestra cada instancia en la que un usuario restableció la contraseña mediante el autoservicio de restablecimiento de contraseña (SSPR) y proporciona las puertas o métodos usados para la autenticación.

- **Registro de restablecimiento de contraseña**: muestra cada vez que un usuario se registra para SSPR y los métodos de autenticación utilizados, como, por ejemplo, un número de teléfono móvil o preguntas y respuestas.
   > [!NOTE]
   > En los informes de *registro de restablecimiento de contraseña*, no se establece ninguna diferencia entre la puerta SMS y la puerta MFA. Ambas se consideran como métodos de teléfono móvil.

- **Actividad de los grupos de autoservicio**: muestra cada intento que alguien realiza para agregarse o eliminarse de un grupo y creación de un grupo.

    ![Imagen de actividad de restablecimiento de contraseña de la creación de informes híbridos de Azure](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * Los informes presentan actualmente datos de hasta un mes de actividad.
> * Se debe desinstalar el agente para informes híbridos anterior.
> * Para desinstalar los informes híbridos, desinstale el agente MIMreportingAgent.msi.

## <a name="prerequisites"></a>Requisitos previos

* Servicio Identity Manager de Identity Manager 2016 SP1, compilación recomendada [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager).

* Un inquilino de Azure AD Premium con un administrador con licencia en el directorio.

* Conectividad a Internet saliente desde el servidor de Identity Manager hasta Azure.

## <a name="requirements"></a>Requisitos
En la tabla siguiente se muestra una lista de los requisitos para usar informes híbridos en Identity Manager:


|                                         Requisito                                         |                                                                                                                                                                                                                                                                                    Descripción                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Azure AD Premium                                       |                                                                                                        Los informes híbridos son una característica de Azure AD Premium y requiere que esté instalado. </br>Para más información, vea [Introducción a Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Obtenga una [evaluación gratuita de treinta días de Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).                                                                                                         |
|                     Debe ser administrador global de Azure AD                     |                                                                   De manera predeterminada, solo los administradores globales pueden instalar y configurar los agentes para comenzar, acceder al portal y realizar cualquier operación en Azure. </br>**Importante**: La cuenta usada para instalar los agentes debe ser una cuenta profesional o educativa. No puede ser una cuenta Microsoft. Para más información, vea [Suscribirse a Azure como organización](https://docs.microsoft.com/azure/active-directory/sign-up-organization).                                                                   |
| El agente híbrido de Identity Manager está instalado en cada servidor de destino del servicio Identity Manager |                                                                                                                                                                                                       Para recibir los datos y proporcionar las funcionalidades de análisis y supervisión, la generación de informes híbrida requiere que los agentes estén instalados y configurados en servidores de destino.  </br>                                                                                                                                                                                                       |
|                    Conectividad saliente a los extremos del servicio de Azure                     | Durante la instalación y el tiempo de ejecución, el agente requiere conectividad a los puntos de conexión del servicio de Azure. Si los firewalls bloquean la conectividad saliente, asegúrese de agregar los puntos de conexión siguientes a la lista de elementos permitidos:<ul><li>\*.blob.core.windows.net </li><li>\*.servicebus.windows.net, puerto: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         Conectividad saliente basada en direcciones IP                         |                                                                                                                                                                                                                      Para información sobre el filtrado basado en direcciones IP en los firewalls, consulte los [intervalos IP de Azure](https://www.microsoft.com/download/details.aspx?id=41653).                                                                                                                                                                                                                      |
|                 La inspección de SSL del tráfico de salida está filtrada o deshabilitada                 |                                                                                                                                                                                                               Las operaciones de carga de datos o el paso de registro del agente pueden generar errores si existe inspección de SSL o la finalización del tráfico de salida en el nivel de red.                                                                                                                                                                                                                |
|                      Puertos de firewall en el servidor que ejecuta el agente                       |                                                                                                                                                                                                          Para poder comunicarse con los puntos de conexión de servicio de Azure, el agente requiere que los siguientes puertos de firewall estén abiertos:<ul><li>Puerto TCP 443</li><li>Puerto TCP 5671</li></ul>                                                                                                                                                                                                          |
|          Permitir determinados sitios web si la seguridad mejorada de Internet Explorer está habilitada           |                                                                                Si la seguridad mejorada de Internet Explorer está habilitada, los siguientes sitios web deben permitirse en el servidor donde se instaló el agente:<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>El servidor de federación de su organización de confianza para Azure Active Directory; por ejemplo, <https://sts.contoso.com>.</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Instalación del agente de informes de Identity Manager en Azure AD
Una vez instalado el agente de informes, los datos de actividad de Identity Manager se exportan de Identity Manager al registro de eventos de Windows. El agente de informes de Identity Manager procesa los eventos y los carga en Azure. En Azure los eventos se analizan, descifran y filtran para los informes requeridos.

1.  Instale Identity Manager 2016.

2.  Descargue el agente de informes de Identity Manager y, después, haga lo siguiente:

    a. Inicie sesión en el portal de administración de Azure AD y seleccione **Active Directory**.

    b. Haga doble clic en el directorio del que sea administrador global y para el que disponga de una suscripción de Azure AD Premium.

    c. Seleccione **Configuración** y descargue el agente de informes.

3.  Siga estos pasos para instalar el agente de informes:

    a.  Descargue [MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) en el servidor del servicio Identity Manager.

    b.  Ejecute `MIMHReportingAgentSetup.exe`. 

    c.  Ejecute el instalador del agente.

    d.  Asegúrese de que se está ejecutando el servicio del agente de informes de Identity Manager.

    e.  Reinicie el servicio Identity Manager.

4.  Verifique que el agente de informes de Identity Manager funcione en Azure.

    Puede crear datos de informes mediante el portal de autoservicio de restablecimiento de contraseña de Identity Manager para restablecer la contraseña de un usuario. Asegúrese de que el restablecimiento de la contraseña se completó correctamente y compruebe después que los datos se muestran en el portal de administración de Azure AD.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Vista de informes híbridos en Azure Portal

1.  Inicie sesión en [Azure Portal](https://portal.azure.com/) con su cuenta de administrador global del inquilino.

2.  Seleccione **Azure Active Directory**.

3.  En la lista de directorios disponibles de la suscripción, selecciona el directorio del inquilino.

4.  Seleccione **Registros de auditoría**.

5.  En la lista desplegable **Categoría**, asegúrese de que el **Servicio MIM** esté seleccionado.

> [!IMPORTANT]
> Puede que los datos de auditoría de Identity Manager tarden un poco en mostrarse en Azure Portal.

## <a name="stop-creating-hybrid-reports"></a>Detención de la creación de informes híbridos
Si quiere que se dejen de cargar datos de auditoría de informes de Identity Manager en Azure AD, desinstale el agente de informes híbridos. Use la herramienta Agregar o quitar programas de Windows para desinstalar los informes híbridos de Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Eventos de Windows empleados para la creación de informes híbridos
Los eventos generados por Identity Manager se almacenan en el registro de eventos de Windows. Puede ver los eventos en el **Visor de eventos**; para ello, seleccione **Registros de aplicaciones y servicios** > **Registro de solicitudes de Identity Manager**. Cada solicitud de Identity Manager se exporta como un evento en el registro de eventos de Windows en la estructura JSON. Puede exportar el resultado a su sistema de Administración de eventos e información de seguridad (SIEM).

|Tipo de evento|ID|Detalles del evento|
|--------------|------|-----------------|
|Información de|4121|Datos de evento de Identity Manager que incluyen todos los datos de solicitud.|
|Información de|4137|Extensión 4121 de evento de Identity Manager, en caso de que haya demasiados datos para un evento único. El encabezado de este evento se muestra con el siguiente formato: `"Request: <GUID> , message <xxx> out of <xxx>`.|
