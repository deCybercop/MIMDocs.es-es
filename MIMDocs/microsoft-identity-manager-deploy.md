---
title: Pasos necesarios para implementar Microsoft Identity Manager 2016 | Microsoft Docs
description: Obtenga la lista completa de los pasos necesarios para implementar Microsoft Identity Manager 2016, desde la preparación del entorno hasta la configuración de los portales.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3460436682054acf5e9e1b186c3fa39faaa40a43
ms.sourcegitcommit: 8316fa41f06f137dba0739a8700910116b5575d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/04/2018
---
# <a name="deploy-microsoft-identity-manager-2016-sp1"></a>Implementación de Microsoft Identity Manager 2016 SP1
Los artículos de esta sección proporcionan instrucciones paso a paso para implementar Microsoft Identity Manager (MIM) 2016 para escenarios de autoservicio de usuario final en un servidor en el que no se haya implementado previamente FIM ni MIM.

> [!NOTE]
> La topología de implementación descrita en esta sección está pensada solo para la iniciación y el aprendizaje de MIM.  La [guía de planeación de la capacidad](capacity-planning-guide.md) proporciona más información sobre topologías para implementaciones de producción.  Se recomienda revisar esa documentación antes de implementar MIM en un entorno o uso de producción.

El escenario de administración de acceso con privilegios se implementa de forma diferente que en los demás escenarios de MIM, ya que requiere un entorno de bosque bastión dedicado.  Para saber más sobre la implementación de MIM para Privileged Identity Management, consulte [Configuración del entorno MIM para Privileged Access Management](./pam/configuring-mim-environment-for-pam.md).

El proceso de implementación de MIM es muy similar al proceso de su predecesor, FIM 2010 R2. Si quiere echar un vistazo a la documentación de FIM, vea la [Guía de implementación de Forefront Identity Manager 2010 R2](https://technet.microsoft.com/library/jj134310).

## <a name="first-prepare-a-domain"></a>Primero: preparación de un dominio
MIM funciona con Active Directory (AD); siga estos pasos para configurar el controlador de dominio de AD.
- [Configuración de dominio](preparing-domain.md)

## <a name="next-prepare-an-identity-management-servers"></a>Segundo: preparación de un servidor de administración de identidades
Una vez que el dominio está activado y configurado, prepare el servidor de administración de identidad corporativa. Esto incluye la configuración de los siguientes elementos:
- [Windows Server 2016](prepare-server-ws2016.md)
- [SQL Server 2016](prepare-server-sql2016.md)
- [SharePoint 2016](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (opcional)

## <a name="finally-install-microsoft-identity-manager-2016-sp1-components"></a>Tercero: instalación de los componentes de Microsoft Identity Manager 2016 SP1
Una vez que haya configurado el dominio y el servidor, ya está listo para instalar los componentes de MIM y configurarlos para que se sincronicen con AD.
- [MIM Synchronization Service](install-mim-sync.md)
- [Servicio y portal de MIM](install-mim-service-portal.md)
- [Sincronización de las bases de datos de Active Directory y del Servicio MIM](install-mim-sync-ad-service.md)
- [Plataformas compatibles con MIM 2016 o superior](microsoft-identity-manager-2016-supported-platforms.md)
