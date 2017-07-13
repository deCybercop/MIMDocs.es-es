---
title: "Paso 3: Configuración de SQL"
description: "Este artículo es el paso 3 de la serie de artículos que trata sobre cómo configurar Privileged Identity Manager mediante scripts y describe los pasos de configuración de SQL Server."
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
ms.openlocfilehash: 93ae9f198d73d21ae966fe3c3b22e47435bd5608
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# Paso 3: Configuración de SQL
<a id="step-3-configuring-sql" class="xliff"></a>

>[!div class="step-by-step"]
[« Paso 2](sp1-step2-configuring-corp-domain.md)
[Paso 4 »](sp1-step4-configuring-sharepoint.md)

Antes de continuar con los pasos siguientes, asegúrese de que está utilizando SQL Server 2012 SP1 o posterior o SQL Server 2014. Para servidores unidos a dominio, inicie sesión con la cuenta MIMAdmin. De lo contrario, inicie sesión como administrador local
1. Ejecutar PowerShell como administrador
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Seleccionar la opción de menú 3 (Configuración de SQL Server)

  Si el servidor no tiene un dominio unido al dominio de PRIV, se le solicitarán credenciales y se unirá el servidor al dominio.
  Después de unirse a un dominio, el equipo se reiniciará. Una vez que se haya reiniciado correctamente, inicie sesión en el servidor con la cuenta MIMAdmin.

1. Ejecutar PowerShell como administrador
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Seleccionar la opción de menú 3 (instalación de SQL Server)

Cuando se le solicite, proporcione la contraseña de la cuenta de servicio MIMAdmin y permita que la instalación continúe. Una vez completada, pase al paso 4.

>[!div class="step-by-step"]
[« Paso 2](sp1-step2-configuring-corp-domain.md)
[Paso 4 »](sp1-step4-configuring-sharepoint.md)
