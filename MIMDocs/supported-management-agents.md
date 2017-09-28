---
title: Conectores compatibles | Microsoft Docs
description: "Utilice conectores para administrar la transferencia de datos entre MIM y sus orígenes de datos conectados."
keywords: 
author: fimguy
ms.author: fimguy
manager: bhu
ms.date: 09/26/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 99e98f3f9cb5e68fde0e3018856bf613c082325d
ms.sourcegitcommit: ba4cd133f7b49752c5470c9fc46e7e302cc99b49
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="connect-to-your-directories"></a>Conexión a los directorios

Los conectores vinculan orígenes de datos conectados específicos a Microsoft Identity Manager SP1 (MIM). Un conector mueve los datos desde un origen de datos conectado a MIM. Si se modifican los datos de MIM, el conector también puede exportarlos al origen de datos conectado para mantenerlo sincronizado con MIM. Por lo general, hay al menos un conector para cada directorio conectado.

En Forefront Identity Manager, los conectores eran conocidos como agentes de administración. Ese término aún se utiliza en algunos artículos o partes del producto, pero tenga en cuenta que ambos términos hacen referencia al mismo concepto.

En este artículo se tratan los conectores compatibles incluidos en MIM, pero el conector para Extensible Connectivity 2.0 hace posible conectar con aún más orígenes de datos. Algunos asociados han creado sus propios conectores de esta manera, y una lista completa está disponible en la wiki en [FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx) (FIM 2010: Agentes de administración de asociados).

## <a name="supported-connectors-in-mim-2016-sp1"></a>Conectores compatibles en MIM 2016 SP1

| Nombre | Versiones compatibles del origen de datos conectado |
| ---- | ----------------------------------------------- |
| Servicios de dominio de Active Directory | Active Directory 2012, 2016 |
| Active Directory Lightweight Directory Services (ADLDS) | Active Directory Lightweight Directory Services (ADLDS) |
| Lista global de direcciones (LGD) de Active Directory | Lista global de direcciones (LGD) de Active Directory: Exchange 2013, 2016 |
| Extensible Connectivity 2.0 | Cualquier origen de datos basado en llamadas o en archivos |
| Servicio de FIM | La versión del agente de administración de servicios de FIM (servicio de sincronización) debe ser la misma que la de Forefront Identity Manager Service. |
| IBM DB2 Universal Database | Versión 9.5 o 9.7 de IBM DB2; versión 9.5 de IBM DB2 OLEDB FP5 o 9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory versiones 8.7.3, 8.8.5 y 8.8.6 |
| Base de datos de Oracle | Oracle Database 10g o 11g, cliente de 64 bits |
| Microsoft SQL Server | SQL Server 2012, 2014, 2016 |
| Oracle (anteriormente Sun y Netscape) Directory Servers | Sun Directory Server 6.x, 7.x y Oracle 11 |
| [Conector de Windows PowerShell para FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 o posterior |
| [Conector Microsoft Azure Active Directory para FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Microsoft AzureActive Directory |
| [Conector LDAP genérico para FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | Servidor LDAP v3 (compatible con RFC 4510) |
| [Conector para Lotus Domino](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Versión 8.5.x de Lotus Notes |
| [SharePoint Services Connector UPA](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | SharePoint server 2013 o 2016 con la aplicación de servicio de perfiles de usuario (UPA) |
| [Conector para servicios web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | SAP ECC 5.0 o 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 |
| [Archivo de texto de par atributo-valor](https://technet.microsoft.com/en-us/library/cc708644(v=ws.10).aspx) | Archivos de texto de par atributo-valor |
| [Archivo de texto delimitado](https://technet.microsoft.com/en-us/library/cc720612(v=ws.10).aspx) | Archivos de texto delimitado |
| [Lenguaje de marcado de servicios de directorio (DSML)](https://technet.microsoft.com/en-us/library/cc720660(v=ws.10).aspx) | Lenguaje de marcado de servicios de directorio (DSML) 2.0 |
| [Archivo de texto de ancho fijo](https://technet.microsoft.com/en-us/library/cc720633(v=ws.10).aspx) | Archivos de texto de ancho fijo |
| [Formato de intercambio de datos LDAP (LDIF)](https://technet.microsoft.com/en-us/library/cc708662(v=ws.10).aspx) | Formato de intercambio de datos LDAP (LDIF) |

## <a name="related-topics"></a>Temas relacionados

[Agentes de administración de FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
