---
title: "Paso 2: Configuración del dominio de CORP"
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
ms.openlocfilehash: b7acc475c505b29559c510fd3fa350fed1c3157b


---

# <a name="step-2-configuring-the-corp-domain"></a>Paso 2: Configuración del dominio de CORP

>[!div class="step-by-step"]
[« Paso 1](sp1-step1-configuring-priv-domain.md)
[Paso 3 »](sp1-step3-installing-configuring-sql.md)

Una vez que SIDs.txt se copia en el CORPDC **, no se necesita para las implementaciones de PRIVOnly**

1. Iniciar sesión en el CORPDC como administrador
2. Ejecutar PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. seleccionar opción de menú 2 (configuración del bosque de CORP)

>[!div class="step-by-step"]
[« Paso 1](sp1-step1-configuring-priv-domain.md)
[Paso 3 »](sp1-step3-installing-configuring-sql.md)



<!--HONumber=Nov16_HO2-->


