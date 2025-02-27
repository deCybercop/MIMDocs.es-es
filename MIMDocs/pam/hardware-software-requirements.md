---
title: Requisitos de software de PAM | Microsoft Docs
description: Vea los requisitos de hardware y software necesarios para una correcta implementación de Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/06/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4838e9e8a495866902a78e713bb3b226eaf9def1
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379818"
---
# <a name="hardware-and-software-requirements"></a>Requisitos de hardware y de software para SharePoint 2013

Privileged Access Management no tiene ningún requisito de hardware aparte de los que puedan tener las plataformas de software subyacente. Asegúrese de tener suficiente memoria o espacio en disco y conectividad de red.

> [!IMPORTANT]
> En este artículo se proporcionan los requisitos mínimos para una implementación básica. No está pensado para demostrar rendimiento, escalabilidad o alta disponibilidad. No representa una topología de implementación recomendada para grandes empresas o entornos de producción.

## <a name="installing-from-software-packages"></a>Instalación con paquetes de software

El software siguiente puede descargarse desde TechNet Evaluation Center o MSDN:

- Microsoft Identity Manager 2016
  - Servicio y portal: contiene el programa instalador para el servicio MIM y el portal de MIM y para el escenario PAM.
  - Complementos y extensiones: contiene el programa de instalación para los cmdlets de PowerShell del solicitante.

Puede descargar el siguiente software de GitHub:

- [PAMSamplePortal](https://github.com/Azure/identity-management-samples): contiene la aplicación web de ejemplo para la API de REST.

## <a name="required-software"></a>Software requerido

- Windows Server 2012 R2
- Windows 10 Enterprise
- SQL Server 2012 Service Pack 1 o SQL Server 2014

## <a name="evaluation-software"></a>Software de evaluación

Si no tiene licencias para Windows, SQL Server o Windows Server, puede descargar las versiones de evaluación.

### <a name="technet-evaluation-center"></a>TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)

### <a name="microsoft-download-center"></a>Centro de descarga de Microsoft

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 y sus requisitos previos](https://www.microsoft.com/download/details.aspx?id=42039)

## <a name="hardware-requirements"></a>Requisitos de hardware

Para cada componente de PAM, consulte los requisitos del sistema de los productos de software.

Para CORPDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) o versiones anteriores

Para CORPWKSTN:

- [Windows 10](https://technet.microsoft.com/windows/dn798752.aspx)

Para PRIVDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

Para PAMSRV:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) o [SQL Server 2014](https://msdn.microsoft.com/library/ms143506(v=sql.120).aspx)
