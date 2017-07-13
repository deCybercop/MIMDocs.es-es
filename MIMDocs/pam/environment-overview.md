---
title: "Introducción al entorno de PAM | Microsoft Docs"
description: "Averigüe qué cantidad y configuración de máquinas virtuales se requiere para implementar correctamente Privileged Access Management."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3e6c5a70c6b9ed140a56135676bbd14a84504317
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# Introducción al entorno
<a id="environment-overview" class="xliff"></a>

Privileged Access Management funciona con máquinas virtuales (VM) con unidades independientes que están conectadas entre sí en una red compartida. Estas máquinas virtuales pueden hospedarse en Windows 8.1, Windows Server 2012 R2 o en otras plataformas de sistema operativo.

![Servidores de PAM: relaciones y plataformas compatibles: diagrama](media/pam-test-lab-architecture.png)

Necesitará un mínimo de tres máquinas virtuales.  Si todavía no tiene un dominio de AD para PAM para administrarlo, necesitará una VM adicional para actuar como un controlador de dominio de CORP.  Si quiere configurar el software PRIV para alta disponibilidad, también necesitará dos VM adicionales.

Las unidades donde se almacenarán las imágenes de disco de máquina virtual necesitan al menos 120 GB de espacio libre en disco para contener todas las VM.  Si planea implementar para obtener alta disponibilidad, asegúrese de que el subsistema de disco cumple los requisitos para el almacenamiento compartido de SQL.  El almacenamiento compartido puede encontrarse en forma de discos de clúster de los clústeres de conmutación por error de Windows Server, en forma de discos en una red de área de almacenamiento (SAN) o como recursos compartidos de archivos en un servidor de SMB. Tenga en cuenta que deben dedicarse al entorno bastión; el almacenamiento de uso compartido con otras cargas de trabajo fuera del entorno bastión no está recomendado, ya que podría poner en peligro la integridad de este.

> [!NOTE]
> La vista previa técnica del cliente (CTP) de MIM actual no es compatible con el contenido del directorio o la base de datos de la CTP anterior. Si ha estado evaluando previamente MIM para PAM u otros escenarios, cree una copia de seguridad y archive las máquinas virtuales que use para esa prueba e inicie la implementación con imágenes de máquinas virtuales nuevas que no se hayan usado previamente para escenarios MIM.
