---
title: "Qué son los informes híbridos | Microsoft Docs"
description: "Los informes de actividad de auditoría híbridos de Azure Active Directory le permiten ver los eventos auditados tanto en el entorno local como en la nube."
keywords: 
author: kgremban
ms.author: fimguy
manager: femila
ms.date: 04/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3144ee195675df5dc120896cc801a7124ee12214
ms.openlocfilehash: 8ca0af93f2d72ccf2e314b20d13323b631eb02bc
ms.lasthandoff: 04/27/2017


---

# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory"></a>Informes de auditoría de administración de identidades híbridas de Azure Active Directory
Con los informes de actividad de auditoría de Azure Active Directory (AD), puede ver un único informe para supervisar la actividad de administración de identidades que ocurre en el entorno local o en la nube. Esta característica le permite administrar todos los datos de identidad y acceso en un solo lugar, lo que ahorra tiempo y reduce los costos generales.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>¿Qué es la generación de informes híbridos de Azure Active Directory?
La generación de informes de auditoría híbridos ayuda a los profesionales de TI a hacer frente a desafíos comunes de los informes de administración de identidades.

1. **Recopilar actividades de administración de identidades entre diferentes sistemas** Los informes híbridos le muestran la actividad de administración de identidades de Azure AD y de Identity Manager.

2. **Exportar datos de informes y crear informes personalizados** Además de ver los informes en el portal de Azure, también puede exportar los datos para generar sus propias vistas personalizadas.

3. **Reducir el costo de infraestructura del sistema de informes** Con la creación de informes híbridos en la nube, puede eliminar la infraestructura local de almacenamiento de datos de informes.

## <a name="how-does-it-work"></a>¿Cómo funciona?

Para recopilar los datos locales, instale primero un agente de informes en el servidor de Identity Manager 2016. El agente de informes se descarga desde la página de descarga de Microsoft, a la que puede acceder desde [aquí](https://www.microsoft.com/en-us/download/details.aspx?id=####/).

El proceso de creación de informes híbridos sigue estos pasos:
1. Una vez que se haya instalado el agente de informes, los datos de actividad de Identity Manager se envían al Registro de eventos de Windows.
2. El agente de informes procesa los eventos en el Registro de eventos de Windows y los carga en Azure Portal.
3. Los datos de actividad se almacenan en Azure durante un mes.
4. Al solicitar un informe, los eventos de actividad se analizan y se filtran para los informes requeridos.
5. Azure Portal recupera los datos de informes y los representa como la auditoría en el informe de actividades.

## <a name="see-also"></a>Consulte también
- Obtenga más información sobre [Working with Identity Manager Hybrid Reporting](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting) (Trabajar con la creación de informes híbridos de Identity Manager)
- Obtenga más información sobre [Informes de actividad de auditoría en el portal de Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs)

