---
title: "Paso 4: Configuración de SharePoint"
description: "Este es el paso 4 de la configuración de PAM con scripts. En este paso, configura SharePoint para que se pueda utilizar como parte de la implementación de PAM."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: f8d033bec440c6efed26dd959acc713638258dd3
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2017
---
# <a name="step-4-configuring-sharepoint"></a>Paso 4: Configuración de SharePoint

>[!div class="step-by-step"]
[« Paso 3](sp1-step3-installing-configuring-sql.md)
[Paso 5 »](sp1-step5-configuring-pam.md)

SharePoint debe ser SharePoint Foundation 2013 con SP1.

En servidores unidos a dominios, inicie sesión como MIMAdmin

1. Ejecutar PowerShell como administrador
2.  .\PAMDeployment.ps1
3.  Seleccionar opción de menú 4 (Configuración de SharePoint)


Para servidores de grupo de trabajo

1. Ejecutar PowerShell como administrador
2.  cd $env:SYSTEMDRIVE\PAM
3.  .\PAMDeployment.ps1
4. seleccionar opción de menú 4 (Configuración de SharePoint)

El equipo se reiniciará varias veces cuando se instale SharePoint. El programa de instalación SharePoint debe volverse a ejecutar cada vez y asegurarse de que inicia sesión con la cuenta MIMAdmin.
Si la máquina que instala SharePoint no tiene conectividad a Internet para descargar los requisitos previos, pueden descargarse de forma independiente y colocarlos en una carpeta local. **Esta ruta de acceso de la carpeta local debe actualizarse en el archivo PAMConfiguration.xml en <PrerequisitesBinaryLocation/>.** Consulte el Anexo 5 de los vínculos para descargar los archivos.
Después de la instalación, la GUI de configuración de SharePoint se abrirá y le guiará por los pasos para completar la instalación de SharePoint. Seleccione servidor completo y recorra el resto de la interfaz de usuario. Después de la instalación, se le solicitará que ejecute el Asistente de configuración. Complete los pasos que se indican a continuación.

1. En la pestaña **Conectar a una granja de servidores**, cambie **para crear una nueva granja de servidores.**
2. Especifique **SQLServer** como servidor de bases de datos para la base de datos de configuración y **SharePoint ServiceAccount** como la cuenta de acceso a la base de datos que usará SharePoint.
3. Especifique una contraseña como la frase de contraseña de seguridad de la granja de servidores **(no se usará más adelante)**.
4. Acepte el resto de la configuración predeterminada del Asistente para la configuración de SharePoint para crear una granja de un solo servidor.

Puede encontrar los detalles en la sección **Configurar SharePoint** en el [Paso 3: Preparar un servidor de PAM](/microsoft-identity-manager/pam/step-3-prepare-pam-server) Cuando se complete, ejecute el script ".\PAMDeployment.ps1" de nuevo y seleccione la opción 4 (Configuración de SharePoint) para completar este paso.

>[!div class="step-by-step"]
[« Paso 3](sp1-step3-installing-configuring-sql.md)
[Paso 5 »](sp1-step5-configuring-pam.md)
