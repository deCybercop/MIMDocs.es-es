---
title: "Informes de administración de identidades híbridas | Microsoft Identity Manager"
description: "La creación de informes híbridos de Azure Active Directory le permite crear informes personalizados que incluyen eventos de nube y locales."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 05/13/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 7e61e201b277a2e8ec9fee785e9e34fca3b1cb29
ms.openlocfilehash: b3f3982ade46932b18fb730fe5c16d52cde188a1


---

# Informes de administración de identidades híbridas en Azure
Con Azure Active Directory (AD), puede crear un único informe para supervisar la actividad de administración de identidades que ocurre de forma local o en la nube. Esta característica le permite administrar todos los datos de identidad y acceso en un solo lugar, lo que ahorra tiempo y reduce los costos generales.

## ¿Qué es la creación de informes híbridos de Azure AD?
La creación de informes híbridos ayuda a los profesionales de TI a hacer frente a desafíos comunes de los informes de administración de identidades.

1. **Recopilar actividades de administración de identidades entre diferentes sistemas.** Los informes híbridos le muestran la actividad de administración de identidades de Azure AD y de Identity Manager.

2. **Exportar datos de informes y crear informes personalizados.** Además de ver los informes en el portal de Azure, también puede exportar los datos para generar sus propias vistas personalizadas.

3. **Reducir el costo de infraestructura del sistema de informes.** Con la creación de informes híbridos en la nube, puede eliminar la infraestructura local de almacenamiento de datos de informes.

## ¿Cómo funciona?

Para recopilar los datos locales, instale primero un agente de informes en el servidor de Identity Manager. El agente de informes se descarga en la página de configuración de su directorio en el [portal de Azure clásico](https://manage.windowsazure.com/).

El proceso de creación de informes híbridos sigue estos pasos:
1. Una vez que se haya instalado el agente de informes, los datos de actividad de Identity Manager se envían al Registro de eventos de Windows.
2. El agente de informes procesa los eventos en el Registro de eventos de Windows y los carga en Azure.
3. Los datos de actividad se almacenan en Azure durante un mes.
4. Al solicitar un informe, los eventos de actividad se analizan y se filtran para los informes requeridos.
5. Por último, el portal de Azure clásico recupera los datos de informes y los representa como el informe de actividades.

## Consulte también
- Obtenga más información sobre [Working with Identity Manager Hybrid Reporting](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting) (Trabajar con la creación de informes híbridos de Identity Manager)



<!--HONumber=Jun16_HO4-->


