---
title: Conectores compatibles | Microsoft Identity Manager
description: Utilice conectores para administrar la transferencia de datos entre MIM y los directorios.
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: 28847b6d494cf5166e22be31b63fdfb027f96ed0


---

# Conexión a los directorios

Los conectores vinculan orígenes de datos conectados específicos a Microsoft Identity Manager (MIM). Un conector mueve los datos desde un origen de datos conectado a MIM. Si se modifican los datos de MIM, el conector también puede exportarlos al origen de datos conectado para mantenerlo sincronizado con MIM. Por lo general, hay al menos un conector para cada directorio conectado.

En Forefront Identity Manager, los conectores eran conocidos como agentes de administración. Ese término aún se utiliza en algunos artículos o partes del producto, pero tenga en cuenta que ambos términos hacen referencia al mismo concepto.

Este artículo trata sobre los conectores incluidos en MIM, pero el conector para Extensible Connectivity 2.0 hace posible conectar con aún más orígenes de datos. Algunos asociados han creado sus propios conectores de esta manera, y una lista completa está disponible en la wiki en [FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx) (FIM 2010: Agentes de administración de asociados).

## Conectores compatibles en MIM 2016

| Nombre | Versiones compatibles del origen de datos conectado |
| ---- | ----------------------------------------------- |
| Servicios de dominio de Active Directory | Active Directory 2000, 2003, 2003 R2, 2008, 2008 R2, 2012 |
| Active Directory Lightweight Directory Services (ADLDS) | Active Directory Lightweight Directory Services (ADLDS) |
| Lista global de direcciones (LGD) de Active Directory | Lista global de direcciones (LGD) de Active Directory: Exchange 2000, 2003, 2007, 2010, 2013 |
| Extensible Connectivity 2.0 | Cualquier origen de datos basado en llamadas o en archivos |
| Servicio MIM | Microsoft Identity Manager 2016 |
| IBM DB2 Universal Database | Versiones 9.1, 9.5 o 9.7 de IBM DB2; IBM DB2 OLEDB v9.5 FP5 o v9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory versiones 8.7.3, 8.8.5 y 8.8.6 |
| Base de datos de Oracle | Oracle Database 10g o 11g, cliente de 64 bits |
| Microsoft SQL Server | SQL Server 2000, 2005, 2008, 2008 R2, 2012 |
| Oracle (anteriormente Sun y Netscape) Directory Servers | Sun Directory Server 6.x, 7.x y Oracle 11 |
| [Conector de Windows PowerShell para FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 o posterior |
| [Conector Microsoft Azure Active Directory para FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Microsoft AzureActive Directory |
| [Conector LDAP genérico para FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | Servidor LDAP v3 (compatible con RFC 4510) |
| [Conector para Lotus Domino](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Versiones v8.0.x o v8.5.x de Lotus Notes |
| [SharePoint Services Connector para FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | SharePoint server 2013 o 2016 con la aplicación de servicio de perfiles de usuario (UPA) |
| [Conector para servicios web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | SAP ECC 5.0 o 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 |
| Archivo de texto de par atributo-valor | Archivos de texto de par atributo-valor |
| Archivo de texto delimitado | Archivos de texto delimitado |
| Lenguaje de marcado de servicios de directorio (DSML) | Lenguaje de marcado de servicios de directorio (DSML) 2.0 |
| Archivo de texto de ancho fijo | Archivos de texto de ancho fijo |
| Formato de intercambio de datos LDAP (LDIF) | Formato de intercambio de datos LDAP (LDIF) |



<!--HONumber=Jul16_HO3-->


