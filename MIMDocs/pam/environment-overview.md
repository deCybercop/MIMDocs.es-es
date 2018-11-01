---
title: Introducción al entorno de PAM | Microsoft Docs
description: Averigüe qué cantidad y configuración de máquinas virtuales se requiere para implementar correctamente Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6f4b6e224b6b50bf2190688a994f35159d273713
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379505"
---
# <a name="environment-overview"></a>Introducción al entorno

Privileged Access Management funciona con máquinas virtuales (VM) con unidades independientes que están conectadas entre sí en una red compartida. Estas máquinas virtuales pueden hospedarse en Windows 8.1, Windows Server 2012 R2 o en otras plataformas de sistema operativo.

![Servidores de PAM: relaciones y plataformas compatibles: diagrama](media/pam-test-lab-architecture.png)

Necesitará un mínimo de tres máquinas virtuales.  Si todavía no tiene un dominio de AD para PAM para administrarlo, necesitará una máquina virtual adicional para actuar como un controlador de dominio de CORP.  Si quiere configurar el software PRIV para alta disponibilidad, también necesita dos máquinas virtuales adicionales.

Las unidades donde se almacenarán las imágenes de disco de máquina virtual necesitan al menos 120 GB de espacio libre.  Si planea implementar para obtener alta disponibilidad, asegúrese de que el subsistema de disco cumple los requisitos para el almacenamiento compartido de SQL.  El almacenamiento compartido puede encontrarse en forma de discos de clúster de los clústeres de conmutación por error de Windows Server, en forma de discos en una red de área de almacenamiento (SAN) o como recursos compartidos de archivos en un servidor de SMB.

> [!IMPORTANT]
> El almacenamiento debe estar dedicado al entorno bastión. El almacenamiento de uso compartido con otras cargas de trabajo fuera del entorno bastión no está recomendado, ya que podría poner en peligro la integridad de este.

## <a name="next-steps"></a>Pasos siguientes

- [Privileged Access Management para Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) es una introducción a PAM y cómo funciona.
- [Descripción de los componentes de PAM](principles-of-operation.md) es una introducción a los distintos componentes de PAM.
