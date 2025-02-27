---
title: Configuración de MIM 2016 para Privileged Access Management | Microsoft Docs
description: La guía de instalación y MIM, y su configuración para Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: fd444f9c67f1daeaf33883a988f54a97e12e685c
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379522"
---
# <a name="configure-the-mim-environment-for-privileged-access-management"></a>Configuración del entorno MIM para Privileged Access Management

Se deben completar siete pasos al configurar el entorno para el acceso entre bosques, instalar y configurar Active Directory y Microsoft Identity Manager y demostrar una solicitud de acceso de Just-In-Time.

Estos pasos aparecen de forma que pueda empezar desde cero y crear un entorno de pruebas. Si está aplicando PAM en un entorno existente, puede usar sus propios controladores de dominio o cuentas de usuario en lugar de crear unas nuevas para que coincidan con los ejemplos.

1. Preparar el servidor *CORPDC* como controlador de dominio y *CORPWKSTN* como estación de trabajo miembro.

2. Preparar el servidor *PRIVDC* como controlador de dominio.

3.  Preparar el servidor *PAMSRV* en el bosque *PRIV* .

4.  Instalar los componentes de MIM en *PAMSRV* y los cmdlets en una estación de trabajo miembro del bosque *CONTOSO* , y prepararlos para Privileged Access Managment.

5.  Establecer la confianza entre los bosques *PRIV* y *CONTOSO* .

6.  Preparar grupos de seguridad con privilegios de acceso a recursos protegidos y cuentas de miembro para Privileged Access Management Just-In-Time.

7.  Demostrar la solicitud, la recepción y el uso de acceso elevado con privilegios a un recurso protegido.

> [!div class="step-by-step"]
> [Inicio »](step-1-prepare-corp-domain.md)
