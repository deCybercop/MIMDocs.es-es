---
title: Instalación de BHOLD Analytics | Microsoft Docs
description: El módulo BHOLD Analytics ofrece pruebas de acceso a los datos, que están basadas en reglas
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: ec7069156aa033b33a139ae83e26abcdea7b482a
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358336"
---
# <a name="bhold-analytics-installation"></a>Instalación de BHOLD Analytics

El módulo BHOLD Analytics proporciona pruebas de acceso a los datos, que están basadas en reglas y permiten asegurarse de que la organización controle el acceso a los datos de manera eficaz y cumpla los requisitos de acceso interno y externo. El análisis de impacto automatizado que genera el módulo BHOLD Analytics ofrece una visión general donde se muestra el número de usuarios que se verían afectados por la aplicación de una regla propuesta, tanto los usuarios que cumplirían la regla como aquellos que la infringirían. El módulo BHOLD Analytics también puede proporcionar una lista detallada de los usuarios que cumplirían la regla y de aquellos que la infringirían.

## <a name="bhold-analytics-installation-requirements"></a>Requisitos de instalación de BHOLD Analytics

Antes de instalar el módulo BHOLD Analytics, debe instalar el módulo BHOLD Core en el servidor en el que tiene previsto instalar el módulo BHOLD Analytics. Para más información sobre la instalación del módulo BHOLD Core, vea [Instalación de BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Antes de comenzar

Antes de empezar a instalar el módulo BHOLD Analytics, debe prepararse para proporcionar la información que el Asistente para la instalación de BHOLD Analytics le pide para completar la instalación. Esta hoja de cálculo le ayudará a anotar esta información y, así, tenerla preparada para cuando se le pida.

| **Elemento**                                    | **Descripción**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Security Provider on Domain/Machine (Usar proveedor de seguridad en dominio/máquina)** | Cuando se selecciona, especifica que la seguridad de Active Directory Domain Services controlará el acceso a BHOLD Core.                                                                                                                | Active la casilla de verificación. **Importante:** se producirá un error en la instalación si no se activa esta casilla.                                                                                                                                                                                                                   |
| **Dominio**                                  | Especifica el dominio que contiene la cuenta de servicio que se creó al instalar BHOLD Core. Para más información, vea [Instalación de BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | El nombre de dominio lo proporciona automáticamente el asistente. Cambie el nombre solamente si es incorrecto. **Importante:** especifique el nombre de dominio mediante el nombre de NetBIOS (corto), en lugar del nombre de dominio completo (FQDN). Por ejemplo, si el FQDN del dominio es fabrikam.com, especifique el nombre de dominio como FABRIKAM. |
| **User**                                    | Especifica el nombre de inicio de sesión de la cuenta de usuario del servicio de BHOLD Core.                                                                                                                                                          | Escriba aquí el nombre de la cuenta de usuario.                                                                                                                                                                                                                                                                                    |
| **Contraseña**                                | Especifica la contraseña de la cuenta de usuario del servicio.                                                                                                                                                                       | Escriba aquí la contraseña. **Importante:** no olvide guardar esta contraseña en una ubicación segura y oculta.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>Instalación de BHOLD Analytics

Para instalar el módulo BHOLD Analytics, inicie sesión como miembro del grupo Administradores del dominio, descargue el archivo siguiente y ejecútelo como administrador en el servidor en el que quiere instalar el módulo BHOLD Analytics:

- BholdAnalytics<em>\<versión\></em>\_Release.msi

Reemplace *\<versión\>* con el número de la versión de BHOLD Analytics que va a instalar.

Para ejecutar el archivo de programa como administrador, haga clic con el botón derecho en el archivo y luego haga clic en **Ejecutar como administrador**.

# <a name="next-steps"></a>Pasos siguientes

- [Instalación de BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
- [Guía de instalación de BHOLD](bhold-installation-guide.md)
- [Historial de versiones de BHOLD](../reference/version-bhold-history.md)
