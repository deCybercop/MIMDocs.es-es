---
title: "Paso 7: Configuración del filtrado de SID/historial de SID"
description: "Este es el paso 7 de la configuración de Privileged Identity Manager mediante scripts. Este paso trata cómo configurar el filtrado de SID y el historial de SID."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: e608593f40759e3bc995daa56c4575510a71e987
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# Paso 7: Configuración del filtrado de SID/historial de SID
<a id="step-7-set-up-sid-historysid-filtering" class="xliff"></a>

>[!div class="step-by-step"]
[«Paso 6](sp1-step6-setup-pam-trust.md)
[paso 8»](sp1-step8-pam-deployment-verification.md)

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

>[!div class="step-by-step"]
[«Paso 6](sp1-step6-setup-pam-trust.md)
[paso 8»](sp1-step8-pam-deployment-verification.md)
