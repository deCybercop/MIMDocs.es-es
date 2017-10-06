---
title: "Qué son los informes híbridos | Microsoft Docs"
description: "Los informes de actividad de auditoría híbridos de Azure Active Directory le permiten ver los eventos auditados tanto en el entorno local como en la nube."
keywords: 
author: fimguy
ms.author: fimguy
manager: bhu
ms.date: 09/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: fimguy
ms.suite: ems
ms.openlocfilehash: 151fbd26011ca76901d181131a88ded8a718a27a
ms.sourcegitcommit: 0f99de31fe6b52ec692b3886073909f549a451d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2017
---
# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory---public-previewrefresh"></a>Informes de auditoría de administración de identidades híbridas de Azure Active Directory: versión preliminar pública (actualización)
Con los informes de actividad de auditoría de Azure Active Directory (AD), puede ver un único informe para supervisar la actividad de administración de identidades que ocurre en el entorno local o en la nube. Esta característica le permite administrar todos los datos de identidad y acceso en un solo lugar, lo que ahorra tiempo y reduce los costos generales.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>¿Qué es la generación de informes híbridos de Azure Active Directory?
La generación de informes de auditoría híbridos ayuda a los profesionales de TI a hacer frente a desafíos comunes de los informes de administración de identidades.

1. **Recopilar actividades de administración de identidades entre diferentes sistemas** Los informes híbridos le muestran la actividad de administración de identidades de Azure AD y de Identity Manager.

2. **Exportar datos de informes y crear informes personalizados** Además de ver los informes en el portal de Azure, también puede exportar los datos para generar sus propias vistas personalizadas.

3. **Reducir el costo de infraestructura del sistema de informes** Con la creación de informes híbridos en la nube, puede eliminar la infraestructura local de almacenamiento de datos de informes.

## <a name="how-does-it-work"></a>¿Cómo funciona?

Para recopilar los datos locales, instale primero un agente de informes en el servidor de Identity Manager 2016. El agente de informes se descarga desde la página de descarga de Microsoft, a la que puede acceder desde [aquí](https://www.microsoft.com/en-us/download/details.aspx?id=55112).

El proceso de creación de informes híbridos sigue estos pasos:
1. Una vez que se haya instalado el agente de informes, los datos de actividad de Identity Manager se envían al Registro de eventos de Windows.
2. El agente de informes procesa los eventos diferenciales cada 10 minutos o en el reinicio del servicio en el Registro de eventos de Windows y los carga en Azure Portal.
3. Azure Portal procesa los datos recibidos dentro de 1 hora después de recibirlos
4. Los datos de actividad se almacenan en Azure durante un mes.
5. Azure Portal recupera los datos de informes de auditoría y los representa como la auditoría en la hoja Informes de auditoría de Azure.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre [Working with Identity Manager Hybrid Reporting](working-with-identity-manager-hybrid-reporting.md) (Trabajar con la creación de informes híbridos de Identity Manager)
- Obtenga más información sobre [Informes de actividad de auditoría en el portal de Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs)
- Obtenga más información acerca de las [directivas de retención de informes](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-retention).
- Obtenga más información acerca de la [integración de registro de Microsoft Azure (SIEM)](https://docs.microsoft.com/en-us/azure/security/security-azure-log-integration-overview).
- Obtenga más información acerca de la [API de generación de informes de Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-api-getting-started).
