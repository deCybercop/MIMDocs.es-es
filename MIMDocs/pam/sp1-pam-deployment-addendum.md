---
title: Anexo
description: Preparar el dominio CORP con identidades nuevas o existentes para ser administrado por Privileged Identity Manager mediante scripts
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: 7f859a74d13a6741dbaf08a1641a73ae986c8343


---
# <a name="pam-deployment-scripts-addendum"></a>Anexo de scripts de implementación de PAM:

## <a name="addendum-1-setting-up-the-priv-domain"></a>Anexo 1: Configuración del dominio de PRIV

Después de descomprimir el archivo comprimido en la carpeta $env:SYSTEMDRIVE\PAM, edite PAMDeploymentConfig.xml para proporcionar información sobre el bosque de PRIV. Actualice el DNSName, el NetbiosName, el nombre del controlador de dominio, la ruta de acceso de registro/base de datos y la ruta de acceso de Sysvol. Actualice también el dominio y ForestMode. Si va a probar Windows Server Technical Preview 5, establezca DomainMode y ForestMode en WinThreshold.

1. Iniciar sesión en el controlador de dominio de PRIV como administrador
2. Ejecutar PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. seleccionar opción de menú 9 (configuración del bosque de PRIV)


El controlador de dominio se reiniciará automáticamente después de la finalización. La contraseña de administrador del Modo de restauración de servicios de directorio (DSRM) debe coincidir con los criterios siguientes:

  * Longitud de contraseña de 15 caracteres como mínimo
  * La contraseña contiene al menos un carácter en minúscula
  * La contraseña contiene al menos un carácter en mayúscula
  * La contraseña contiene al menos un dígito o un carácter especial

## <a name="addendum-2-setting-up-the-corp-domain"></a>Anexo 2: Configuración del dominio de CORP

Si simplemente está empezando a usar PAM y desea configurar un entorno de prueba, el script también permite la configuración de un dominio de CORP. Después de descomprimir el archivo comprimido en la carpeta $env:SYSTEMDRIVE\PAM, edite PAMDeploymentConfig.xml agregando la información sobre el bosque de CORP. Actualice DNSName, NetbiosName, el nombre del controlador de dominio, la ruta de acceso de registro/base de datos y la ruta de acceso de Sysvol. El nivel funcional de bosque debe ser Windows Server 2012 R2 como mínimo.

1. Iniciar sesión en el controlador de dominio de CORP como administrador
2. Ejecutar PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. seleccionar opción de menú 10 (configuración del bosque de CORP)

El controlador de dominio se reiniciará automáticamente una vez completado

## <a name="addendum-3-setting-up-a-corp-client-to-do-the-validation"></a>Apéndice 3: Configuración de un cliente CORP para realizar la validación

ClientBinaryLocation en el archivo de configuración debe apuntar a la ubicación donde se encuentra setup.exe.
Inicie sesión en el cliente como administrador local y ejecute los siguientes comandos en una ventana de PowerShell con privilegios elevados:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Seleccione la opción de menú 7 (configuración de cliente de MIM PAM)


Si el equipo no está unido a un dominio, le pedirá las credenciales de administrador de CORP para realizar la unión a un dominio. Debe reiniciar el equipo después de unirse a un dominio. Inicie sesión en el cliente de nuevo como administrador local y ejecute los siguientes comandos desde una ventana de PowerShell con privilegios elevados:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Seleccione la opción de menú 7 (configuración de cliente de MIM PAM)

Continúe con el paso 8 proporcionado anteriormente.

## <a name="addendum-4-if-something-goes-wrong"></a>Anexo 4: Si algo va mal

Todos los registros de scripts se guardan en % AppData%\MIMPAMInstall. Comprima la carpeta en un archivo zip y envíela por correo electrónico a [mim2016@microsoft.com](mailto:mim2016@microsoft.com) junto con los detalles de la operación y el error.



<!--HONumber=Jan17_HO1-->


