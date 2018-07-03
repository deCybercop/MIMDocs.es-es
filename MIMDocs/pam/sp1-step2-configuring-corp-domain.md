---
title: 'Paso 2: Configuración del dominio de CORP'
description: Este artículo describe el segundo paso necesario para configurar el dominio corp que implica ejecutar un script después de copiar sids.txt se en CORPDC
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
ms.openlocfilehash: 775abc1546bc9eb93842b69edf64ac032518d66c
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289785"
---
# <a name="step-2-configuring-the-corp-domain"></a>Paso 2: Configuración del dominio de CORP

> [!div class="step-by-step"]
> [« Paso 1](sp1-step1-configuring-priv-domain.md)
> [Paso 3 »](sp1-step3-installing-configuring-sql.md)

Una vez que SIDs.txt se copia en el CORPDC **, no se necesita para las implementaciones de PRIVOnly**

1. Iniciar sesión en el CORPDC como administrador
2. Ejecutar PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. seleccionar opción de menú 2 (configuración del bosque de CORP)

> [!div class="step-by-step"]
> [« Paso 1](sp1-step1-configuring-priv-domain.md)
> [Paso 3 »](sp1-step3-installing-configuring-sql.md)
