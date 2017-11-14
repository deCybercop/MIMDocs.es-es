---
title: "Instalación de BHOLD Reporting | Microsoft Docs"
description: "El módulo BHOLD Reporting permite generar informes sobre los roles y las directivas de autorización"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: aa6a263daadc4abdcad0eaaba554b6bc739fbd5f
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2017
---
# <a name="bhold-reporting-installation"></a>Instalación de BHOLD Reporting

Con el módulo BHOLD Reporting, puede generar informes sobre los roles y otras directivas de autorización de BHOLD. Estos informes suelen ser útiles para auditorías o para demostrar el cumplimiento de requisitos normativos. Además, este módulo amplía la capacidad de administrar la autorización dentro de la organización, ya que proporciona a los usuarios la información necesaria para analizar la pertenencia a los roles. Puede restringirse la vista de los informes para garantizar que solo se muestre la información pertinente a los usuarios que crean informes.

## <a name="bhold-reporting-installation-requirements"></a>Requisitos de instalación de BHOLD Reporting

Antes de instalar el módulo BHOLD Reporting, debe instalar el módulo BHOLD Core en el servidor en el que tiene previsto instalar el módulo BHOLD Reporting. Para más información sobre la instalación del módulo BHOLD Core, vea [Instalación de BHOLD Core](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx).

>[!IMPORTANT]
Si va a instalar los dos módulos BHOLD Reporting y BHOLD Attestation, debe instalar primero BHOLD Reporting y luego BHOLD Attestation.

## <a name="before-you-begin"></a>Antes de comenzar

Antes de empezar a instalar el módulo BHOLD Reporting, debe prepararse para proporcionar la información que el Asistente para la instalación de BHOLD Reporting le pide para completar la instalación. Esta hoja de cálculo le ayudará a anotar esta información y, así, tenerla preparada para cuando se le pida.

| **Elemento**                                    | **Descripción**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Security Provider on Domain/Machine (Usar proveedor de seguridad en dominio/máquina)** | Cuando se selecciona, especifica que la seguridad de Active Directory Domain Services controlará el acceso a BHOLD Core.                                                                                                                | Active la casilla de verificación. </br>**Importante:** se producirá un error en la instalación si no se activa esta casilla.                                                                                                                                                                                                                   |
| **Dominio**                                  | Especifica el dominio que contiene la cuenta de servicio que se creó al instalar BHOLD Core. Para más información, vea [Instalación de BHOLD Core](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | El nombre de dominio lo proporciona automáticamente el asistente. Cambie el nombre solamente si es incorrecto. **Importante:** especifique el nombre de dominio mediante el nombre de NetBIOS (corto), en lugar del nombre de dominio completo (FQDN). Por ejemplo, si el FQDN del dominio es fabrikam.com, especifique el nombre de dominio como FABRIKAM. |
| **User**                                    | Especifica el nombre de inicio de sesión de la cuenta de usuario del servicio de BHOLD Core.                                                                                                                                                          | Escriba aquí el nombre de la cuenta de usuario.                                                                                                                                                                                                                                                                                    |
| **Contraseña**                                | Especifica la contraseña de la cuenta de usuario del servicio.                                                                                                                                                                       | Escriba aquí la contraseña. </br>**Importante:** no olvide guardar esta contraseña en una ubicación segura y oculta.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>Instalación de BHOLD Reporting

Para instalar el módulo BHOLD Reporting, inicie sesión como miembro del grupo Administradores del dominio, descargue el archivo siguiente y ejecútelo como administrador en el servidor en el que quiere instalar el módulo BHOLD Reporting:

- BholdReporting*\<versión\>*\_Release.msi

Reemplace *\<versión\>* con el número de la versión de BHOLD Reporting que va a instalar.

Para ejecutar el archivo de programa como administrador, haga clic con el botón derecho en el archivo y luego haga clic en **Ejecutar como administrador**.

## <a name="next-steps"></a>Pasos siguientes

- [Guía de instalación de BHOLD](bhold-installation-guide.md)
- [Referencia para desarrolladores de BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historial de versiones de BHOLD](../reference/version-bhold-history.md)