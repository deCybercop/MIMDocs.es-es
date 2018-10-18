---
title: Scripts de implementación de MIM2016 SP1 PAM
description: Esta página forma parte de la serie de artículos acerca de cómo configurar Privileged Identity Manager mediante scripts. Incluye una lista de los supuestos sobre el entorno.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/17/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 139ff94ecc38de37ac8eb6536d1b4d2a42909536
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358057"
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>Scripts de implementación de MIM2016 SP1 PAM

En este Service Pack estamos introduciendo un conjunto de scripts de implementación para facilitar la implementación de PAM. Estos scripts están disponibles en el centro de descarga. Antes de intentar utilizar los scripts es importante asegurarse de cumplir con los siguientes requisitos:

1. El sistema operativo en todos los servidores debe ser como mínimo Windows Server 2012 R2.
2. DNS debe configurarse para habilitar la resolución de nombres entre los controladores de dominio y servidores de componentes.
3. Los archivos binarios de instalación deben estar disponibles localmente en los servidores designados para la instalación de SQL, SharePoint y MIM.
4. El entorno tiene tres máquinas dedicadas (físicas o virtuales) que ejecutan independientemente CORPDC, PRIVDC y PAMSERVER.
5. Para la opción de validación, debe tener una estación de trabajo dedicada.

>[!NOTE]
>Si surge cualquier problema con la ejecución del script, debe echar un vistazo a los registros. Todos los registros de scripts se guardan en % AppData%\MIMPAMInstall. Comprima la carpeta en un archivo ZIP, e inclúyela, junto con los detalles de la operación y el error, en su incidencia.

¿Está listo para empezar a trabajar con scripts de implementación de PAM? Empiece en [Configurar PAM mediante scripts](./pam/sp1-pam-configure-using-scripts.md).
