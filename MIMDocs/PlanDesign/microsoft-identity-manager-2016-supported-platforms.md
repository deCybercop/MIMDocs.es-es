---
title: Plataformas de software compatibles | Microsoft Docs
description: Buscar los productos y las versiones compatibles con cada uno de los componentes de MIM 2016
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 03/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f1faed3a09023e288a68a8950dae43725f19eb3e
ms.openlocfilehash: 33c84afa4d6fd2ed7bde33de39dd151f83f07fd4
ms.lasthandoff: 04/12/2017


---

# <a name="supported-platforms-for-mim-2016"></a>Plataformas compatibles con MIM 2016

En esta tabla se describen las plataformas compatibles y la versión para cada componente de Microsoft Identity Manager 2016. Las versiones marcadas con * solo se admiten en MIM 2016 Service Pack 1.


| **Componente de MIM** | **Plataforma** | **Versión** |
|-------------------|--------------|-------------|
| **Sincronización de MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | Base de datos de sincronización de MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Active Directory para aprovisionamiento de usuario, PCNS y sincronización de la LGD (opcional)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Exchange para aprovisionamiento de buzones de correo y sincronización de la LGD (opcional)|Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016 * |
| | Entorno de desarrollo (opcional) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017 * |
| | Sistema adicional conectado (opcional) | Servicios de dominio de Active Directory<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2000 o posterior<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Otros productos de terceros |
| **Servicio MIM** (excepción el escenario de PAM) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Base datos de Servicio MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Exchange para correos electrónicos de administración de grupo y aprobación del Servicio MIM (opcional) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (solo notificaciones) |
| **Portal y Servicio MIM** (solo el escenario de PAM)| Windows Server | Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Active Directory para el bosque PAM del entorno bastión | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Active Directory para bosques existentes | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | Base datos de Servicio MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | Servidor de correo para correos electrónicos de administración de grupo y aprobación del Servicio MIM (opcional) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (solo notificaciones) |
| | Explorador | Todos los exploradores principales |
| **Informes de Servicio MIM** | Windows Server | Windows Server 2012 <br/> Windows Server 2016 * |
| | Almacenamiento de datos | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager </br> System Center 2016 Service Manager * (con 4.4.1459)<br/> [Compatibilidad de versión de SQL Server para System Center 2016](https://technet.microsoft.com/en-us/system-center-docs/system-requirements/sql-server-version-compatibility)
 |
| **Portales de registro y restablecimiento de contraseñas de MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Explorador web | Todos los exploradores principales |
| **Extensiones y complementos de MIM** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Integración de Outlook (opcional) | Outlook 2007 SP2<br/>Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (en Windows 10) * |
| | Cmdlets de solicitante de PowerShell de PAM (opcional) | Windows 8.1<br/>Windows 10 |
| **MIM Certificate Management** (integración de servidor y la entidad emisora de certificados) | Windows server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Entidad de certificación | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Base de datos de MIM CM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| **MIM Certificate Management** (aplicación) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM Certificate Management** (cliente y cliente en masa) | Windows | Windows 7 |
| **Conjunto de aplicaciones BHOLD de MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Base de datos de BHOLD | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * <br/> SQL Server 2016 * |
| | Servidor de correo (opcional) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Explorador web | Internet Explorer 7, 8, 9, 10 u 11 con Silverlight |

