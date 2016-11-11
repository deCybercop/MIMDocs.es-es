---
title: "Paso 6: Configuración la confianza de PAM"
description: Preparar el dominio CORP con identidades nuevas o existentes para ser administrado por Privileged Identity Manager mediante scripts
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/25/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: 5bcf4f4ef201236746ec1bf75c1c8900841a6c79


---

# <a name="step-6-set-up-the-pam-trust"></a>Paso 6: Configurar la confianza de PAM

>[!div class="step-by-step"]
[«Paso 5](sp1-step5-configuring-pam.md)
[paso 7»](sp1-step7-setup-sidhistory-sidfiltering.md)

**Esto no es necesario para un único entorno de PRIV** Inicio de sesión de PAMServer con la cuenta MIMAdmin.

1. Iniciar sesión en PAMServer con la cuenta de MIMAdmin
2. Ejecutar PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. seleccione la opción de menú 6 (configuración de confianza de PAM)

  Cuando se le solicite, escriba las credenciales de la cuenta de administrador de CORP. Después de proporcionar las credenciales, se establecerá la confianza y la configuración estará completa.

>[!div class="step-by-step"]
[«Paso 5](sp1-step5-configuring-pam.md)
[paso 7»](sp1-step7-setup-sidhistory-sidfiltering.md)



<!--HONumber=Nov16_HO2-->


