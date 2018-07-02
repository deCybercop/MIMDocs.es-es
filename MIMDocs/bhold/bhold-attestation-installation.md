---
title: Instalación de BHOLD Attestation | Microsoft Docs
description: El módulo BHOLD Attestation permite designar revisores y revisiones
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 6838355c05f7c19436d8a83839044ea5f4e2533d
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290346"
---
# <a name="bhold-attestation-installation"></a>Instalación de BHOLD Attestation

El módulo BHOLD Attestation permite designar revisores y realizar revisiones periódicas de las relaciones entre los usuarios y los permisos y cuentas por aplicación.

## <a name="bhold-attestation-installation-requirements"></a>Requisitos de instalación de BHOLD Attestation

Antes de instalar el módulo BHOLD Attestation, debe instalar el módulo BHOLD Core en el servidor en el que tiene previsto instalar el módulo BHOLD Attestation. Para más información sobre la instalación del módulo BHOLD Core, vea [Instalación de BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Dado que el módulo BHOLD Attestation envía correos electrónicos a los usuarios, es necesario que el entorno tenga un servidor de correo electrónico de Protocolo simple de transferencia de correo (SMTP), como Microsoft Exchange Server.

> [!IMPORTANT]
> Si va a instalar los dos módulos BHOLD Reporting y BHOLD Attestation, debe instalar primero BHOLD Reporting y luego BHOLD Attestation.

## <a name="before-you-begin"></a>Antes de comenzar

Antes de empezar con la instalación del módulo BHOLD Attestation, debe prepararse para proporcionar la información que el Asistente para la instalación de BHOLD Attestation le pide para completar la instalación. Esta hoja de cálculo le ayudará a anotar esta información y, así, tenerla preparada para cuando se le pida.

| **Elemento**                                    | **Descripción**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Security Provider on Domain/Machine (Usar proveedor de seguridad en dominio/máquina)** | Cuando se selecciona, especifica que la seguridad de Active Directory Domain Services controlará el acceso a BHOLD Core.                                                                                                                | Active la casilla de verificación. **Importante:** se producirá un error en la instalación si no se activa esta casilla.                                                                                                                                                                                                                   |
| **Dominio**                                  | Especifica el dominio que contiene la cuenta de servicio que se creó al instalar BHOLD Core. Para más información, vea [Instalación de BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | El nombre de dominio lo proporciona automáticamente el asistente. Cambie el nombre solamente si es incorrecto. **Importante:** especifique el nombre de dominio mediante el nombre de NetBIOS (corto), en lugar del nombre de dominio completo (FQDN). Por ejemplo, si el FQDN del dominio es fabrikam.com, especifique el nombre de dominio como FABRIKAM. |
| **User**                                    | Especifica el nombre de inicio de sesión de la cuenta de usuario del servicio de BHOLD Core.                                                                                                                                                          | Escriba aquí el nombre de la cuenta de usuario.                                                                                                                                                                                                                                                                                    |
| **Contraseña**                                | Especifica la contraseña de la cuenta de usuario del servicio.                                                                                                                                                                       | Escriba aquí la contraseña. **Importante:** no olvide guardar esta contraseña en una ubicación segura y oculta.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>Instalación de BHOLD Attestation

Para instalar el módulo BHOLD Attestation, inicie sesión como miembro del grupo Administradores del dominio, descargue el archivo siguiente y ejecútelo como administrador en el servidor en el que quiere instalar el módulo BHOLD Attestation:

- BholdAttestation<em>\<versión\></em>\_Release.msi

Reemplace *\<versión\>* con el número de la versión de BHOLD Attestation que va a instalar.

Para ejecutar el archivo de programa como administrador, haga clic con el botón derecho en el archivo y luego haga clic en **Ejecutar como administrador**.

## <a name="next-steps"></a>Pasos siguientes

- [Guía de instalación de BHOLD](bhold-installation-guide.md)
- [Referencia para desarrolladores de BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historial de versiones de BHOLD](../reference/version-bhold-history.md)