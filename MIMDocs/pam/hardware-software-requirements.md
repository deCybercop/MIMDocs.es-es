---
title: Requisitos de hardware y software | Microsoft Identity Manager
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: a6bdf1b947ee3ebc4c9e89e74b2912697ebf1f60
ms.openlocfilehash: 77e7174e94ea8032c4e57155db489f493ce18177


---

No hay requisitos de hardware más allá de los que correspondan a las plataformas de software subyacente, suficiente memoria o espacio de disco y conectividad de red. En este artículo se proporcionan los requisitos mínimos para una implementación básica. No está pensado para demostrar el rendimiento, la escalabilidad o la alta disponibilidad y no representa una topología de implementación recomendada para grandes empresas o entornos de producción.

## Instalación con paquetes de software

El software siguiente puede descargarse desde TechNet Evaluation Center o MSDN:  
- Microsoft Identity Manager 2016
  - Servicio y portal: contiene el programa instalador para el servicio MIM y el portal de MIM y para el escenario PAM.
  - Complementos y extensiones: contiene el programa de instalación para los cmdlets de PowerShell del solicitante.

Puede descargar el siguiente software de GitHub:  
- PAMSamplePortal: contiene la aplicación web de ejemplo para la API de REST.

## Software requerido

- Windows Server 2012 R2  
- Windows 8.1 Enterprise o Windows 10 Enterprise  
- SQL Server 2012 Service Pack 1 o SQL Server 2014  

## Software de evaluación

Si no tiene licencias para Windows, SQL Server o Windows Server, puede descargar las versiones de evaluación.

### TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8.1 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### Centro de descarga de Microsoft

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 y sus requisitos previos](https://www.microsoft.com/download/details.aspx?id=42039)

## Requisitos de hardware

Para cada componente de PAM, consulte los requisitos del sistema de los productos de software.

Para CORPDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) o versiones anteriores

Para CORPWKSTN:  
- [Windows 8.1](http://windows.microsoft.com/windows-8/system-requirements)

Para PRIVDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

Para PAMSRV:
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)  
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) o [SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms143506(v=sql.120).aspx)



<!--HONumber=Jun16_HO3-->


