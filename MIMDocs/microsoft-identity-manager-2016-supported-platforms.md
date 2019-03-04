---
title: Plataformas de software compatibles | Microsoft Docs
description: Buscar los productos y las versiones compatibles con cada uno de los componentes de MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: c6da739349f8c4ba4016635e326ed30d5f5954c6
ms.sourcegitcommit: 67e2de99f86e762125979233f6ee80afcd78dc4d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2019
ms.locfileid: "56795424"
---
# <a name="supported-platforms-for-mim-2016"></a>Plataformas compatibles con MIM 2016

En esta tabla se describen las plataformas compatibles y la versión para cada componente de Microsoft Identity Manager 2016. Las versiones marcadas con * solo se admiten en MIM 2016 Service Pack 1 con la revisión más reciente.  Las versiones marcadas con "NR", de no recomendadas, se admiten pero no se recomiendan si se inicia una nueva implementación de esa plataforma para MIM.


| **Componente de MIM** | **Plataforma** | **Versión** |
|-------------------|--------------|--------------|
| **Sincronización de MIM** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | Nivel funcional de Active Directory para aprovisionamiento de usuarios, PCNS y sincronización de la LGD | Windows 2000 (NR)<br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 *
| | Base de datos de sincronización de MIM | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Active Directory para aprovisionamiento de usuarios, PCNS y sincronización de la LGD (opcional)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Exchange para aprovisionamiento de buzones de correo y sincronización de la LGD (opcional)|Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016 * |
| | Entorno de desarrollo (opcional) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017 * |
| | Sistema adicional conectado (opcional) | Servicios de dominio de Active Directory<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2008 o posterior<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Otros productos de terceros |
| **Servicio y portal de MIM** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |Escenario PAM:  Windows Server | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * |
| |Escenario PAM: Active Directory para el bosque PAM del entorno bastión | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * |
| |Escenario PAM: Active Directory para bosques existentes del escenario PAM (CORP) | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | Base datos de Servicio MIM | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 |
| | SharePoint | SharePoint Foundation 2010 (NR)<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | Servidor de correo para correos electrónicos de administración de grupo y aprobación del Servicio MIM (opcional) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (notificación solo antes de la compilación [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490) |
| | Explorador | Todos los exploradores principales * (limitado Dispositivos móviles)|
| **Informes de Servicio MIM** | Windows Server |  Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR) <br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Almacenamiento de datos | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager <br/> System Center 2016 Service Manager * (con 4.4.1459)<br/> [Compatibilidad de versión de SQL Server para System Center 2016](https://docs.microsoft.com/system-center/scsm/upgrade-to-sm-2016) |
| **Portales de registro y restablecimiento de contraseñas de MIM** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Explorador web | Principales exploradores admitidos |
| **Extensiones y complementos de MIM** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Integración de Outlook (opcional) | Outlook 2010 (en Windows, excepto Hacer clic y ejecutar)<br/>Outlook 2013 (en Windows, excepto Hacer clic y ejecutar) <br/> Outlook 2016 (en Windows 10, excepto Hacer clic y ejecutar) * |
| | Cmdlets de solicitante de PowerShell de PAM (opcional) | Windows 8.1<br/>Windows 10 |
| **MIM Certificate Management** (integración de servidor y la entidad emisora de certificados) | Windows server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Entidad de certificación | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Base de datos de MIM CM | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| **MIM Certificate Management** (aplicación) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM Certificate Management** (cliente en masa) | Windows | Windows 7 |
| **MIM Certificate Management** (tarjeta inteligente cliente basada en ActiveX) | Windows | Windows 7 <br/> Windows 8 <br/> Windows 8.1 <br/> Windows 10 |
| **Conjunto de aplicaciones BHOLD de MIM** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Base de datos de BHOLD | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * <br/> SQL Server 2016 * |
| | Servidor de correo (opcional) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Explorador web | Exploradores admitidos Internet Explorer con Silverlight |
