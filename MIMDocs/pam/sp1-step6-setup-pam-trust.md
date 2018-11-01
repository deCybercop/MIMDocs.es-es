---
title: 'Paso 6: Configuración la confianza de PAM'
description: Paso 6 de configuración de PAM con scripts. Esta sección trata sobre cómo configurar la confianza necesaria entre los dominios corp y priv
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
ms.openlocfilehash: ad1482718693c9ae7004a71334013de68f7c20da
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379386"
---
# <a name="step-6-set-up-the-pam-trust"></a>Paso 6: Configurar la confianza de PAM

> [!div class="step-by-step"]
> [«Paso 5](sp1-step5-configuring-pam.md)
> [paso 7»](sp1-step7-setup-sidhistory-sidfiltering.md)

**Esto no es necesario para un único entorno de PRIV** Inicio de sesión de PAMServer con la cuenta MIMAdmin.

1. Iniciar sesión en PAMServer con la cuenta de MIMAdmin
2. Ejecutar PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. seleccione la opción de menú 6 (configuración de confianza de PAM)

   Cuando se le solicite, escriba las credenciales de la cuenta de administrador de CORP. Después de proporcionar las credenciales, se establecerá la confianza y la configuración estará completa.

> [!div class="step-by-step"]
> [«Paso 5](sp1-step5-configuring-pam.md)
> [paso 7»](sp1-step7-setup-sidhistory-sidfiltering.md)
