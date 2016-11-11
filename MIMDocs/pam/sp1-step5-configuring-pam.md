---
title: "Paso 5: Instalación/configuración de PAM"
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
ms.openlocfilehash: c641865548f753a609ccee8dbf12c329bb6a1c9f


---
# <a name="step-5-installingconfiguring-pam"></a>Paso 5: Instalación/configuración de PAM

>[!div class="step-by-step"]
[« Paso 4](sp1-step4-configuring-sharepoint.md)
[Paso 6 »](sp1-step6-setup-pam-trust.md)

En un PAMServer de dominio unido, inicie sesión como MIMAdmin. También puede iniciar sesión como administrador local.
1. Ejecutar PowerShell como administrador
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. seleccione la opción de menú 5 (configuración de MIM PAM)

>[!NOTE]
>Si el equipo no tiene ya un dominio unido a PRIV, le solicitará las credenciales. Después de la unión del dominio, el equipo se reiniciará.

Una vez reiniciado PAMServer, inicie sesión de nuevo en el equipo con la cuenta MIMAdmin.

1. Ejecutar PowerShell como administrador
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. seleccione la opción de menú 5 (configuración de MIM PAM)

  Cuando se le solicite, escriba la contraseña en la cuenta de monitor de MIM, cuenta de componente de MIM, cuenta de MIM MA, cuenta de servicio de MIN, cuenta de administrador de MIM y cuenta de SharePoint.
  Una vez completada la instalación, el equipo se reiniciará.

>[!div class="step-by-step"]
[« Paso 4](sp1-step4-configuring-sharepoint.md)
[Paso 6 »](sp1-step6-setup-pam-trust.md)



<!--HONumber=Nov16_HO2-->


