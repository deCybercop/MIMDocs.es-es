---
title: "Configuración de MIM 2016 para Privileged Access Management | Microsoft Docs"
description: "La guía de instalación y MIM, y su configuración para Privileged Access Management."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ce1ce0c67dfd39433ff01dabd542e862c557c787
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# Configuración del entorno MIM para Privileged Access Management
<a id="configure-the-mim-environment-for-privileged-access-management" class="xliff"></a>
Se deben completar siete pasos al configurar el entorno para el acceso entre bosques, instalar y configurar Active Directory y Microsoft Identity Manager y demostrar una solicitud de acceso de Just-In-Time.

Estos pasos aparecen de forma que pueda empezar desde cero y crear un entorno de pruebas. Si está aplicando PAM en un entorno existente, puede usar sus propios controladores de dominio o cuentas de usuario en lugar de crear unas nuevas para que coincidan con los ejemplos.

1.  Preparar el servidor *CORPDC* como controlador de dominio y *CORPWKSTN* como estación de trabajo miembro.

2.  Preparar el servidor *PRIVDC* como controlador de dominio.

3.  Preparar el servidor *PAMSRV* en el bosque *PRIV* .

4.  Instalar los componentes de MIM en *PAMSRV* y los cmdlets en una estación de trabajo miembro del bosque *CONTOSO* , y prepararlos para Privileged Access Managment.

5.  Establecer la confianza entre los bosques *PRIV* y *CONTOSO* .

6.  Preparar grupos de seguridad con privilegios de acceso a recursos protegidos y cuentas de miembro para Privileged Access Management Just-In-Time.

7.  Demostrar la solicitud, la recepción y el uso de acceso elevado con privilegios a un recurso protegido.

>[!div class="step-by-step"]
[Inicio »](step-1-prepare-corp-domain.md)
