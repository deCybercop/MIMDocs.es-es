---
title: Configurar el entorno de MIM para Privileged Access Management | Microsoft Identity Manager
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: 9cf126d898c93faf89d7119136cce4e4963bb63d
ms.openlocfilehash: c9f2cf2ba1f42ea1513ae38d8089839d85ae5553


---

# Configurar el entorno de MIM para Privileged Access Management
Se deben completar siete pasos al configurar el entorno para el acceso entre bosques, instalar y configurar Active Directory y Microsoft Identity Manager y demostrar una solicitud de acceso de Just-In-Time.

Estos pasos aparecen de forma que pueda empezar desde cero y crear un entorno de pruebas. Si está aplicando PAM en un entorno existente, puede usar sus propios controladores de dominio o cuentas de usuario en lugar de crear unas nuevas para que coincidan con los ejemplos.

1.  Preparar el servidor *CORPDC* como controlador de dominio y *CORPWKSTN* como estación de trabajo miembro.

2.  Preparar el servidor *PRIVDC* como controlador de dominio.

3.  Preparar el servidor *PAMSRV* en el bosque *PRIV* .

4.  Instalar los componentes de MIM en *PAMSRV* y los cmdlets en una estación de trabajo miembro del bosque *CONTOSO* , y prepararlos para Privileged Access Managment.

5.  Establecer la confianza entre los bosques *PRIV* y *CONTOSO* .

6.  Preparar grupos de seguridad con privilegios de acceso a recursos protegidos y cuentas de miembro para Privileged Access Management Just-In-Time.

7.  Demostrar la solicitud, la recepción y el uso de acceso elevado con privilegios a un recurso protegido.

>[!div class="step-by-step"] [Inicio »](step-1-prepare-corp-domain.md)



<!--HONumber=Jun16_HO3-->


