---
# required metadata

title: Creación de informes híbridos de Identity Manager en Azure | Microsoft Identity Manager
description: La creación de informes híbridos de Azure Active Directory le permite crear informes personalizados que incluyen eventos de nube y locales.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Creación de informes híbrida de Identity Manager en Azure
Ahora, si tiene una suscripción a Azure, puede crear fácilmente informes de eventos tanto locales como en la nube. Estos informes se pueden ver en el portal de Azure. Y lo que es mejor, los informes se combinan con las actividades de Azure Active Directory. Con la creación de informes híbrida de Identity Manager, el portal de administración de Azure AD puede mostrar informes de las actividades de administración de identidades de las actividades que tengan lugar tanto de forma local como en la nube. Esta funcionalidad de informes ofrece lo siguiente:

-   Una experiencia unificada: informes unificados de las actividades de IAM locales y en la nube.

-   Unos costes reducidos: al dejar de necesitar una infraestructura local de almacén de datos de informes.

-   Unos datos que son suyos: los datos de informes se pueden exportar fácilmente desde Identity Manager local o desde Azure AD y se pueden usar para generar informes de vista personalizada.

## ¿Qué es la creación de informes híbrida de Azure AD?
Gracias a los informes híbridos, el portal de administración de Azure AD puede mostrar informes unificados de las actividades de administración de identidades. Y todo ello sin importar dónde se llevó a cabo la actividad, quién es el administrador de identidades o de qué tipo de suscripción de Azure AD se dispone. Por ejemplo, si quiere saber quién se registró en el autoservicio de restablecimiento de contraseña (SSPR) el mes pasado, puede verlo todo en el portal de administración de Azure AD. Este informe mostrará los usuarios que se registraron en el SSPR tanto mediante el [panel de acceso de las aplicaciones](https://myapps.microsoft.com) como mediante Identity Manager.

![Imagen de actividad de restablecimiento de contraseña de Azure](media/MIM-Hybrid-passwordreset.jpg)

## ¿Por qué debo utilizarlos?
Los informes híbridos ayudan a los profesionales de TI a hacer frente a algunos de los desafíos comunes de los informes de administración de identidades.

1.  Informar sobre actividades de administración de identidades que se realizaron en diferentes sistemas: ahora puede ver los informes de administración de identidades de las actividades en Azure AD e Identity Manager en el portal de administración de Azure AD.

2.  Exportar datos de informes y crear informes personalizados: además de los informes en Azure AD, con esta nueva funcionalidad hemos agregado eventos de Windows que reflejan la actividad de Identity Manager. Esto hace que sea mucho más fácil que nunca integrar sistemas SIEM, ver la actividad de Identity Manager y crear informes personalizados.

3.  Minimizar el coste de la infraestructura del sistema de informes: para implementar esta nueva funcionalidad solo necesitará unos minutos. Todo lo que tiene que hacer es instalar un agente de informes en el servidor de Identity Manager.

El agente de informes se descarga desde el portal de administración de Azure AD, en la pantalla de configuración de directorio:

![Imagen de descarga del agente de informes de MIM](media/MIM-Hybrid-downloadReportAgent.jpg)

## ¿Cómo funciona?
Una vez que se haya instalado el agente de informes, los datos de actividad de Identity Manager se envían al Registro de eventos de Windows. El agente de informes procesa los eventos y los carga en Azure. Actualmente en Azure se almacenan los datos de actividad de un mes. Al recuperar el informe, los eventos de actividad se analizan y se filtran para los informes requeridos. Por último, el portal de administración de Azure recupera los datos de informes y los representa como el informe de actividades.

![Diagrama de informes híbridos](media/MIM-Hybrid-howitworks.png)


<!--HONumber=Apr16_HO2-->


