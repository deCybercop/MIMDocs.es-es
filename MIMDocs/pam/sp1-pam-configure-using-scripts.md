---
title: Configurar PAM mediante scripts
description: Este artículo forma parte de la serie para la configuración de PAM mediante scripts. Abarca la modificación del archivo XML que van a utilizar los scripts de implementación de PAM.
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 07/20/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 3d10d8642694873512e9cc85cc90a75ce45c3361
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332974"
---
# <a name="configure-pam-using-scripts"></a>Configurar PAM mediante scripts

Si opta por instalar SQL y SharePoint en servidores independientes, se deben configurar con las siguientes instrucciones. Si se instalan componentes de SQL, SharePoint y PAM en el mismo equipo, deben ejecutarse los pasos indicados a continuación desde ese equipo.

En los pasos siguientes se presupone que un dominio PRIV ya está configurado. Para obtener instrucciones para configurar un dominio de PRIV, vea el anexo al final del documento.

pasos:

1. Descargar [Scripts de implementación de PAM](https://www.microsoft.com/download/details.aspx?id=53941)
2. Descomprima el archivo comprimido "PAMDeploymentScripts.zip" en la carpeta %SYSTEMDRIVE%\PAM en todos los equipos.
3. En cualquiera de los equipos, abra el archivo **PAMDeploymentConfig.xml** y actualice los detalles con el siguiente gráfico o las instrucciones dentro del propio archivo XML. Si los bosques de CORP y PRIV ya están configurados, todo lo que necesita actualizar es **DNSName** y **NetbiosName.**
4. En la sección Roles, actualice la **cuenta de servicio**, los **detalles del equipo** y la **ubicación de los archivos binarios de instalación** para los roles de SQL Server, SharePoint y MIM.
    1. La ubicación de los archivos binarios de MIM debe apuntar al directorio que contiene la carpeta de "Servicio y Portal". La ubicación de los archivos binarios de cliente debe apuntar al directorio que contiene "Add-ins and Extensions.msi".

5. Si se trata de un entorno de PRIVOnly, la etiqueta de PRIVOnly debe establecerse en True.
    1. Para entornos de PRIVOnly, actualice **DNSName** y **NetbiosName** del dominio de PRIV para que coincida con el dominio de CORP. Asegúrese de que los sufijos de máquina son correctos para los equipos donde se instalará SQL, SharePoint y MIM, puesto que el archivo de plantillas predeterminado presupone una configuración de CORP y PRIV.
    2. Haga clic aquí para obtener más información acerca de los entornos de PRIVOnly.

6. Copiar el mismo PAMDeploymentConfig.xml en la carpeta %SYSTEMDRIVE%\PAM en todas las máquinas, CORPDC, PRIVDC, PAM Server, SQL Server y SharePoint Server.


## <a name="deployment-worksheet"></a>Hoja de cálculo de implementación

Antes de continuar, actualice PAMDeploymentConfig.xml y coloque la copia actualizada en todos los equipos.

### <a name="setup"></a>Setup

|virtual   | Quién ejecutar como   |Comandos   |
|---|---|---|
|  PRIVDC |Administrador de dominio de PRIV   | .\PAMDeployment.ps1: Seleccionar opción de menú 1 (Configuración del bosque de PRIV)   |
|   |   |  En el paso anterior se genera SIDs.txt. Este archivo debe copiarse en $envDrive:PAM de CORPDC antes de ejecutar el paso siguiente. |
| CORPDC  |Administrador de dominio de CORP   | .\PAMDeployment.ps1: Seleccionar opción de menú 2 (Configuración del bosque de CORP)   |
| PAMServer (o SQL Server)   |Administrador de dominio de CORP   |  .\PAMDeployment.ps1: Seleccionar opción de menú 2 (Configuración del bosque de CORP)  |
|  PAMServer |  Administrador local (administración de MIM después de unirse a un dominio) |  .\PAMDeployment.ps1: Seleccionar opción de menú 4 (Configuración de SharePoint)  |
| PAMServer  | Administrador local (administración de MIM después de unirse a un dominio)  | .\PAMDeployment.ps1: Seleccionar opción de menú 5 (Configuración de MIM PAM)   |
|  PAMServer |MIMAdmin   | .\PAMDeployment.ps1: Seleccionar opción de menú 6 (Configuración de confianza de PAM) .\PAMDeployment.ps1: Seleccionar opción de menú 6 (Configuración de confianza de PAM) |

### <a name="validation"></a>Validación

|  virtual | Quién ejecutar como   | Comandos   |
|---|---|---|
| CORPClient  | Usuario de CORP (administrador local)  |   .\PAMDeployment.ps1: Seleccionar opción de menú 7 (Configuración de cliente de MIM PAM)  |
| CORPDC  | Administrador de dominio de CORP   | Import-module .\PAMValidation.psm1 ; Create-PAMValidationCORPDCConfig   |
| PAMServer   | MIMAdmin  | Import-module .\PAMValidation.psm1 ; Move-PAMValidationUsersToPAM  |
| CORPClient  | Usuario de CORP (administrador local)   |   Import-module .\PAMValidation.psm1 ; Enable-PAMUsersCORPClientRemote |
|  CORPClient | <PRIV>Usuario de \PRIV.pamRequestor y en el caso de PRIVOnly : <CORP>\pamrequestor   | Import-module .\PAMValidation.psm1 ; Test-PAMValidationScenarioNoApprovalRequest  |


> [!div class="step-by-step"]
> [Inicio »](sp1-step1-configuring-priv-domain.md)
