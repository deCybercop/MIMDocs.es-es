---
title: "Introducción al entorno de PAM | Microsoft Identity Manager"
description: "Averigüe qué cantidad y configuración de máquinas virtuales se requiere para implementar correctamente Privileged Access Management."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 3057618c609ed251efe1f6cc6b2d3694ac61eafd


---

# Introducción al entorno

Privileged Access Management funciona con máquinas virtuales (VM) con unidades independientes que están conectadas entre sí en una red compartida. Estas máquinas virtuales pueden hospedarse en Windows 8.1, Windows Server 2012 R2 o en otras plataformas de sistema operativo.

![Servidores de PAM: relaciones y plataformas compatibles: diagrama](media/pam-test-lab-architecture.png)

Necesitará un mínimo de tres máquinas virtuales.  Si todavía no tiene un dominio de AD para PAM para administrarlo, necesitará una VM adicional para actuar como un controlador de dominio de CORP.  Si quiere configurar el software PRIV para alta disponibilidad, también necesitará dos VM adicionales.

Las unidades donde se almacenarán las imágenes de disco de máquina virtual necesitan al menos 120 GB de espacio libre en disco para contener todas las VM.  Si planea implementar para obtener alta disponibilidad, asegúrese de que el subsistema de disco cumple los requisitos para el almacenamiento compartido de SQL.  El almacenamiento compartido puede encontrarse en forma de discos de clúster de los clústeres de conmutación por error de Windows Server, en forma de discos en una red de área de almacenamiento (SAN) o como recursos compartidos de archivos en un servidor de SMB. Tenga en cuenta que deben dedicarse al entorno bastión; el almacenamiento de uso compartido con otras cargas de trabajo fuera del entorno bastión no está recomendado, ya que podría poner en peligro la integridad de este.

> [!NOTE]
> La vista previa técnica del cliente (CTP) de MIM actual no es compatible con el contenido del directorio o la base de datos de la CTP anterior. Si ha estado evaluando previamente MIM para PAM u otros escenarios, cree una copia de seguridad y archive las máquinas virtuales que use para esa prueba e inicie la implementación con imágenes de máquinas virtuales nuevas que no se hayan usado previamente para escenarios MIM.



<!--HONumber=Jul16_HO3-->


