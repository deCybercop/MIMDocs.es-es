---
title: 'Paso 5: Instalación/configuración de PAM'
description: Este es el paso 5 de configuración de Privileged Identity Manager mediante scripts. Trata los pasos de implementación en el servidor de PAM.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 03c5b1cb52f16090e6b241d588bbded6be4c6687
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379369"
---
# <a name="step-5-installingconfiguring-pam"></a>Paso 5: Instalación/configuración de PAM

> [!div class="step-by-step"]
> [« Paso 4](sp1-step4-configuring-sharepoint.md)
> [Paso 6 »](sp1-step6-setup-pam-trust.md)

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

> [!div class="step-by-step"]
> [« Paso 4](sp1-step4-configuring-sharepoint.md)
> [Paso 6 »](sp1-step6-setup-pam-trust.md)
