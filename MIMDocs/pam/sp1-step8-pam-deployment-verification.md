---
title: "Paso 8: Comprobación de la implementación de PAM"
description: "La implementación generada por script de PAM incluye scripts de comprobación que pueden ejecutar un escenario de PAM para validar si la implementación de PAM funciona según lo esperado."
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
ms.openlocfilehash: 2f4306dc50ecb869a3c917dfaf320ad80dddedd1
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="step-8-pam-deployment-verification"></a>Paso 8: Comprobación de la implementación de PAM

>[!div class="step-by-step"]
[«Paso 7](sp1-step7-setup-sidhistory-sidfiltering.md)
[Anexo»](sp1-pam-deployment-addendum.md)

El paquete de implementación incluye scripts de comprobación que pueden ejecutar un escenario de PAM para validar si la implementación de PAM funciona según lo esperado.
Para usar la comprobación de la implementación, modifique la sección PAMDeploymentConfig.xml llamada <PamValidation/> .

>[!NOTE]
>La validación requiere un dominio del equipo cliente unido al dominio de CORP con los componentes del lado cliente de PAM instalados. Consulte el anexo de scripts sobre cómo para instalar un cliente.

El nombre del equipo cliente debe actualizarse en la etiqueta <PAMValidationClient/> de PAMDeploymentConfig.xml. El resto de los datos del nodo <PAMValidation/> debe modificarse únicamente si entra en conflicto con los usuarios o grupos existentes, ya que esta validación intentará crearlos.
Siga los pasos siguientes para realizar la validación:

Paso 1:

1. Iniciar sesión en el CORPDC como un administrador de dominio de CORP
2. Ejecutar PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. Import-module .\PAMValidation.psm1
5. Create-PAMValidationonCORPDCConfig

Esto creará los usuarios y grupos necesarios para la validación

Paso 2:

1. Iniciar sesión en el servidor PAM y MIMAdmin
2. Ejecutar PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. move-PAMVAlidationUsersToPAM

Este paso migra a los usuarios y grupos al entorno de PAM

Paso 3:

1. Iniciar sesión como cliente CORP como administrador local
2. Ejecutar PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. Enable-PAMUsersCORPClientRemote


Este paso le solicitará la credencial de CORPAdmin. Una vez proporcionada, agregará los usuarios requeridos en los grupos Usuarios de escritorio remoto y Usuarios de administración remota.
En el cliente CORP, use el siguiente comando para abrir PowerShell como usuario de PRIV que va a validar. </br></br>
**Runas /u:<PRIV domain>\PRIV.pamRequestor powershell.exe**  </br></br>
En la ventana de PowerShell, escriba:

1. cd $env:SYSTEMDRIVE\PAM
2. import-module .\PAMValidation.psm1
3. test-PAMValidationScenarioNoApprovalRequest


  Esto mostrará el estado de la solicitud.
  Inicialmente, el usuario no tendrá acceso al recurso. Una vez que el usuario agregue Just-In-Time al rol, se le concederá acceso. Una vez que caduque la duración de la solicitud, el usuario dejará de tener acceso.
  El script utiliza el valor predeterminado (11 minutos) para que la solicitud expire.

>[!div class="step-by-step"]
[«Paso 7](sp1-step7-setup-sidhistory-sidfiltering.md)
[Anexo»](sp1-pam-deployment-addendum.md)
