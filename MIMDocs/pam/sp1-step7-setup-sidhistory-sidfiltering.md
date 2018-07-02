---
title: 'Paso 7: Configuración del filtrado de SID/historial de SID'
description: Este es el paso 7 de la configuración de Privileged Identity Manager mediante scripts. Este paso trata cómo configurar el filtrado de SID y el historial de SID.
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: d2512690ce648767157a7417e5b41095c970b8eb
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289115"
---
# <a name="step-7-set-up-sid-historysid-filtering"></a>Paso 7: Configuración del filtrado de SID/historial de SID

> [!div class="step-by-step"]
> [«Paso 6](sp1-step6-setup-pam-trust.md)
> [paso 8»](sp1-step8-pam-deployment-verification.md)

**Esto no es necesario para un único entorno de PRIV** Inicio de sesión de PAMServer con la cuenta MIMAdmin.

1. Iniciar sesión en el controlador de dominio de CORP como administrador
2. Ejecutar PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. seleccione la opción de menú 8 (Configuración del filtrado de SID/historial de SID)

Si la ejecución es correcta, verá el mensaje siguiente:<br/></br>
Para el filtrado de SID: <br/></br>
"Estableciendo la confianza para no filtrar los SID" o "No está habilitado el filtrado por SID para esta confianza". </br></br>
Para el historial de SID: </br></br>
"Habilitando historial de SID para esta confianza" o "El historial de SID ya está habilitado para esta confianza".

> [!div class="step-by-step"]
> [«Paso 6](sp1-step6-setup-pam-trust.md)
> [paso 8»](sp1-step8-pam-deployment-verification.md)
