---
title: "¿Qué son los informes híbridos de Azure AD? | Microsoft Docs"
description: "Los informes de actividad de auditoría híbridos de Azure Active Directory permiten ver los eventos auditados tanto en el entorno local como en la nube."
keywords: 
author: davidste
ms.author: davidste
manager: bhu
ms.date: 02/20/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: eb9725df484fb5ac2ee44bd9a0423bdb4fbe7e86
ms.sourcegitcommit: b4a39928c5fa1d7718046563c0809bcbf11d833d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2018
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Informes de auditoría de administración de identidades híbridas de Azure Active Directory
Con los informes de actividad de auditoría de Azure Active Directory (Azure AD), puede supervisar la actividad de administración de identidades en el entorno local o en la nube. Con la administración de todos los datos de acceso e identidades en un único informe, puede ahorrar tiempo y reducir los costos generales.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>¿Qué es la generación de informes híbridos de Azure Active Directory?
Los informes de auditoría híbridos ayudan a los profesionales de TI a hacer frente a desafíos comunes de los informes de administración de identidades, como:

* **Recopilar actividades de administración de identidades entre diferentes sistemas**. Los informes híbridos le muestran la actividad de administración de identidades de Azure AD y de Identity Manager.

* **Exportar datos de informes y crear informes personalizados**. Además de ver los informes en Azure Portal, puede exportar los datos para generar sus propias vistas personalizadas.

* **Reducir el costo de infraestructura del sistema de informes**. Los informes híbridos de la nube permiten eliminar los costos asociados con la infraestructura local de almacenamiento de datos.

## <a name="how-does-it-work"></a>¿Cómo funciona?

Para recopilar los datos locales, instale primero un agente de informes en el servidor de Identity Manager 2016. [Descargue el agente de informes híbridos de Microsoft Identity Manager](https://www.microsoft.com/download/details.aspx?id=55112).

Los informes híbridos siguen este proceso:
1. Una vez instalado el agente de informes, los datos de actividad de Identity Manager se envían al registro de eventos de Windows.
2. El agente de informes procesa los eventos diferenciales cada diez minutos o cuando se reinicia el servicio del registro de eventos de Windows. Después, el agente carga los eventos en Azure Portal.
3. Azure Portal procesa los datos recibidos en el plazo de una hora tras su recepción.
4. Los datos de actividad se almacenan en Azure durante un mes.
5. Azure Portal recupera los datos de informes de auditoría y los muestra en la ventana Informes de auditoría de Azure.

## <a name="next-steps"></a>Pasos siguientes
Más información acerca de:
- [Trabajar con la creación de informes híbrida de Identity Manager](working-with-identity-manager-hybrid-reporting.md)
- [Informes de actividad de auditoría en el portal de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Directivas de retención de informes](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Microsoft Azure Log Integration (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [API de generación de informes de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
