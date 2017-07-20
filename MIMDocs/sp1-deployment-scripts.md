---
title: "Scripts de implementación de MIM2016 SP1 PAM"
description: "Esta página forma parte de la serie de artículos acerca de cómo configurar Privileged Identity Manager mediante scripts. Incluye una lista de los supuestos sobre el entorno."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 07/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: ae8f6a87f57c95e073b40d3cda944c71f1bf7247
ms.sourcegitcommit: 0cb8269f07a5f419d2d1cd760d9cc78b8a1c8aa9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/14/2017
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>Scripts de implementación de MIM2016 SP1 PAM

En este Service Pack estamos introduciendo un conjunto de scripts de implementación para facilitar la implementación de PAM. Estos scripts están disponibles en el centro de descarga. Antes de intentar utilizar los scripts es importante que se asegure de que las suposiciones siguientes se aplican a su entorno.

Suposiciones importantes:
1. El sistema operativo en todos los equipos debe ser como mínimo Windows Server 2012 R2. Si prueba Windows Server 2016 Technical Preview 5, el controlador de dominio de PRIV debe estar instalado con la compilación TP5.
2. DNS debe configurarse para que la resolución de nombres entre los controladores de dominio y servidores de componentes sea automática.
3. Los archivos binarios de instalación deben estar disponibles localmente en los servidores designados para la instalación de SQL, SharePoint y MIM.
4. El entorno tiene tres máquinas dedicadas (físicas o virtuales) que ejecutan independientemente CORPDC, PRIVDC y PAMSERVER.
5. Para la opción de validación, se asume que existe una máquina cliente dedicada para ejecutar este paso.

>[!NOTE]
>Si surge cualquier problema con la ejecución del script, debe echar un vistazo a los registros. Todos los registros de scripts se guardan en % AppData%\MIMPAMInstall. Comprima la carpeta en un archivo ZIP, e inclúyela, junto con los detalles de la operación y el error, en su incidencia.

¿Está listo para empezar a trabajar con scripts de implementación de PAM? Empiece en [Configurar PAM mediante scripts](./pam/sp1-pam-configure-using-scripts.md).
