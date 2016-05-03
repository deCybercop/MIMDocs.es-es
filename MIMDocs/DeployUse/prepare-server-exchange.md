---
# required metadata

title: Configuración de un servidor de administración de identidades&#58; Exchange | Microsoft Identity Manager
description: Como paso opcional, implemente Exchange Server para permitir que MIM 2016 envíe correos y cree buzones de correo. 
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 34a8c16e-3bed-4e16-939b-b9fe17dd834b

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Configuración de un servidor de administración de identidades: Exchange

>[!div class="step-by-step"]
[« SharePoint](prepare-server-sharepoint.md)
[Servicio de sincronización de MIM »](install-mim-sync.md)

> [!NOTE]
> En todos los ejemplos siguientes, **mimservername** representa el nombre del controlador de dominio; **contoso**, el nombre de dominio y **Pass@word1**, una contraseña de ejemplo.

## Implementar Microsoft Exchange Server
Si desea configurar MIM para enviar y recibir correo electrónico o aprovisionar buzones, es necesario tener Exchange presente en el entorno. Si no tiene Exchange implementado, puede instalar una versión de prueba con fines de evaluación.

1. Descargue e instale Microsoft Office 2010 Filter Packs versión 2.0 y Microsoft Office 2010 Filter Packs versión 2.0 SP1.

    - [MS Office10 FP2.0](http://www.microsoft.com/en-us/download/details.aspx?id=17062)

    - [MS Office10 FP2.0 SP1](http://www.microsoft.com/en-us/download/details.aspx?id=26604)

2. Descargue e instale [Microsoft Unified Communications Managed API 4.0, Core Runtime de 64 bits](http://www.microsoft.com/en-us/download/details.aspx?id=34992).

3. Descargue e instale la [versión de prueba de 180 días de MS Exchange Server 2013](http://www.microsoft.com/en-us/evalcenter/evaluate-exchange-server-2013).

>[!div class="step-by-step"]  
[« SharePoint](prepare-server-sharepoint.md)
[Servicio de sincronización de MIM »](install-mim-sync.md)


<!--HONumber=Apr16_HO2-->


