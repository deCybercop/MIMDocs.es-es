---
# required metadata

title: Implementación de MIM 2016 | Microsoft Identity Manager
description: Obtenga la lista completa de los pasos necesarios para implementar Microsoft Identity Manager 2016, desde la preparación del entorno hasta la configuración de los portales.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Implementación de MIM 2016
Los artículos de esta sección proporcionan instrucciones paso a paso para implementar Microsoft Identity Manager (MIM) 2016 para escenarios de autoservicio de usuario final en un servidor en el que no se haya implementado previamente FIM ni MIM.

> [!NOTE]
> La topología de implementación descrita en esta sección está pensada solo para la iniciación y el aprendizaje de MIM.  La [guía de planificación de capacidad](/microsoft-identity-manager/PlanDesign/capacity-planning-guide) proporciona más información sobre topologías para implementaciones de producción.  Se recomienda revisar esa documentación antes de implementar MIM en un entorno o uso de producción.

<!---
Comment: Restore after PAM content is included

The privileged access management scenario is deployed differently than other MIM scenarios, as it requires a dedicated bastion forest environment.  If you want to learn more about deploying MIM for Privileged Identity Management, see [Getting Started with Privileged Access Management](privileged-access-management-get-started.md).
--->

## Primero: preparación de un dominio
MIM funciona con Active Directory (AD); siga estos pasos para configurar el controlador de dominio de AD.
- [Configuración de dominio](preparing-domain.md)

## Segundo: preparación de un servidor de administración de identidades
Una vez que el dominio está activado y configurado, prepare el servidor de administración de identidad corporativa. Esto incluye la configuración de los siguientes elementos:
- [Configuración del servidor: Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [Configuración del servidor: SQL Server 2014](prepare-server-sql2014.md)
- [Configuración del servidor: SharePoint](prepare-server-sharepoint.md)
- [Configuración del servidor: Exchange Server](prepare-server-exchange.md) (opcional)

## Tercero: instalación de los componentes de Microsoft Identity Manager 2016
Una vez que haya configurado el dominio y el servidor, ya está listo para instalar los componentes de MIM y configurarlos para que se sincronicen con AD.
- [Instalación de MIM: servicio de sincronización de MIM](install-mim-sync.md)
- [Instalación de MIM: servicio y portal de MIM](install-mim-service-portal.md)
- [Instalación de MIM: sincronización de las bases de datos de Active Directory y del servicio MIM](install-mim-sync-ad-service.md)


<!--HONumber=Apr16_HO2-->


