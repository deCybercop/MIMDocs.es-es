---
title: "Paso 1: Configuración del dominio de Priv"
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
ms.openlocfilehash: 8c1127b81676dfa40dc9bca515b4c3c6d66a1298


---
# <a name="step-1-configuring-the-priv-domain"></a>Paso 1: Configuración del dominio de Priv

>[!div class="step-by-step"]
[Paso 2 »](sp1-step2-configuring-corp-domain.md)

1. Iniciar sesión en el PRIVDC como administrador
  * Si se trata de un entorno de solo PRIV, inicie sesión en el CORPDC
2. Ejecutar PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Seleccionar opción de menú 1 (configuración del bosque de PRIV)


Las cuentas de servicio necesarias para la administración de SQL/SharePoint y MIM se crean automáticamente si no están ya presentes en el dominio. Se le pedirá que escriba las contraseñas para la creación de estas cuentas de servicio durante la ejecución del script.
Si el dominio de PRIV es Windows Server 2016, con el nivel funcional establecido en Windows Server 2016 Technical Preview 5, el script solicitará la habilitación de la función Privileged Access Management Feature de Active Directory requerida por PAM. Seleccione Sí para continuar.
Para los niveles funcionales debajo de Windows Server 2016, descarte la advertencia de que no se realizará ninguna configuración adicional. Deberá volver a ejecutar la configuración del bosque de PAM y PAMDeployment.ps1 una vez que el administrador eleve el nivel funcional a Windows Server 2016.

>[!NOTE]
>Los pasos siguientes no son necesarios para las configuraciones de PRIVOnly

Copie SIDs.txt que se genera en $env: SYSTEMDRIVE\PAM a la carpeta similar en el CORPDC. Esto es necesario para que CORPDC configure los permisos para los usuarios de PRIV lean las propiedades de usuario de CORP.
Una vez completado el script, se le solicitará que reinicie el equipo para que surtan efecto los cambios.

>[!div class="step-by-step"]
[Paso 2 »](sp1-step2-configuring-corp-domain.md)



<!--HONumber=Nov16_HO2-->


