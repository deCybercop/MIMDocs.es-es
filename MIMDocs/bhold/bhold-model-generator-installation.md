---
title: "Instalación de BHOLD Model Generator | Microsoft Docs"
description: "El modelo de BHOLD permite estructurar los datos de varios orígenes"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 96363fb3b0067ff5c8f8c2f32e9a855464038653
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-model-generator-installation"></a>Instalación de BHOLD Model Generator

Mediante el módulo BHOLD Model Generator, puede estructurar datos de orígenes de autoridad, que contienen información sobre el usuario y la organización, junto con listas de control de acceso (ACL) en un modelo que puede usarse en la administración de BHOLD.

## <a name="bhold-model-generator-installation-requirements"></a>Requisitos de instalación de BHOLD Model Generator 

Antes de instalar el módulo BHOLD Model Generator, siga estos pasos:

1. Instale el módulo BHOLD Core en el servidor en el que tiene previsto instalar el módulo BHOLD Model Generator. Para más información sobre la instalación del módulo BHOLD Core, vea [Instalación de BHOLD Core](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx).

2. Instale el proveedor OLE DB de Microsoft para Microsoft Jet. Para más información, vea este [artículo](http://support.microsoft.com/kb/271908).

>[!WARNING]
No instale BHOLD Model Generator en la red de producción. BHOLD Model Generator está diseñado para usarse sin conexión en un entorno de ensayo para crear un modelo de roles normalizado que se pueda importar en el modelo de roles de la empresa. Si ejecuta BHOLD Model Generator en la red de producción podría ocasionar la pérdida del modelo de roles existente.

## <a name="before-you-begin"></a>Antes de comenzar

Antes de instalar el módulo BHOLD Model Generator, debe prepararse para proporcionar la información que el Asistente para la instalación de BHOLD Model Generator le pide para completar la instalación. Esta hoja de cálculo le ayudará a anotar esta información y, así, tenerla preparada para cuando se le pida. También deberá asegurarse de que

Motor de base de datos redistribuible de Microsoft Access 2010

 

*De \<*<http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>*\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Configuración de la cuenta**

| **Elemento**                                    | **Descripción**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Security Provider on Domain/Machine (Usar proveedor de seguridad en dominio/máquina)** | Cuando se selecciona, especifica que la seguridad de Active Directory Domain Services controlará el acceso a BHOLD Core.                                                                                                                | Active la casilla de verificación. **Importante:** se producirá un error en la instalación si no se activa esta casilla.                                                                                                                                                                                                                   |
| **Dominio**                                  | Especifica el dominio que contiene la cuenta de servicio que se creó al instalar BHOLD Core. Para más información, vea [Instalación de BHOLD Core](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | El nombre de dominio lo proporciona automáticamente el asistente. Cambie el nombre solamente si es incorrecto. **Importante:** especifique el nombre de dominio mediante el nombre de NetBIOS (corto), en lugar del nombre de dominio completo (FQDN). Por ejemplo, si el FQDN del dominio es fabrikam.com, especifique el nombre de dominio como FABRIKAM. |
| **User**                                    | Especifica el nombre de inicio de sesión de la cuenta de usuario del servicio de BHOLD Core.                                                                                                                                                          | Escriba aquí el nombre de la cuenta de usuario.                                                                                                                                                                                                                                                                                    |
| **Contraseña**                                | Especifica la contraseña de la cuenta de usuario del servicio.                                                                                                                                                                       | Escriba aquí la contraseña. **Importante:** no olvide guardar esta contraseña en una ubicación segura y oculta.                                                                                                                                                                                                                  |

**Configuración de base de datos de copias de seguridad**

| Elemento                                        | Descripción                                                                                                                                                                                                                                                                                                                                                                                                                  | Valor                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar seguridad integrada**                 | Especifica que se usa la autenticación de Windows para obtener acceso a la base de datos.                                                                                                                                                                                                                                                                                                                                                        | Active esta casilla si se usa la autenticación de Windows para conectarse a SQL Server. Desactive esta casilla si se usa la autenticación de SQL Server. Si se usa la autenticación de SQL Server, debe crearse la base de datos antes de ejecutar la instalación de BHOLD Core. **Nota:** si se usa la autenticación de Windows, debe haber iniciado sesión con una cuenta que tenga el rol de servidor sysadmin en el servidor de base de datos. **Importante:** use la autenticación de SQL Server únicamente en entornos de prueba. Microsoft recomienda usar la autenticación de Windows en las implementaciones de producción. |
| **Usuario de la base de datos** y **Contraseña de la base de datos** | Especifica el nombre de usuario y la contraseña de un usuario con el rol de servidor sysadmin en el servidor de base de datos. Estos valores se proporcionan únicamente cuando se usa la autenticación de SQL Server.                                                                                                                                                                                                                                                  | Escriba aquí el nombre de usuario de SQL Server; escriba aquí la contraseña de usuario de SQL Server. </br></br> **Importante:** no olvide guardar esta contraseña en una ubicación segura y oculta.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Servidor de bases de datos** y **Nombre de base de datos**   | Especifica el nombre de NetBIOS del servidor de base de datos y el nombre de la base de datos de copias de seguridad que creará el programa de instalación de BHOLD Model Generator. Si no va a usar la instancia del servidor de base de datos predeterminada, especifique la instancia del servidor de base de datos con el formato *\<servidor\>*\\*\<instancia\>*.  Microsoft recomienda asignar un nombre a la base de datos de copia de seguridad con el nombre de la base de datos de BHOLD Core seguido de \_BACKUP. Por ejemplo, B1_BACKUP. | Escriba aquí el nombre de servidor (o el servidor y la instancia). </br> Escriba aquí el nombre de la base de datos.

## <a name="bhold-model-generator-setup"></a>Instalación de BHOLD Model Generator

Para instalar el módulo BHOLD Model Generator, inicie sesión como miembro del grupo Administradores del dominio, descargue el archivo siguiente y ejecútelo como administrador en el servidor en el que quiere instalar el módulo BHOLD Core:

- BholdModelGenerator *\<versión\>*\_Release.msi

Reemplace *\<versión\>* con el número de la versión de BHOLD Model Generator que va a instalar.

Para ejecutar el archivo de programa como administrador, haga clic con el botón derecho en el archivo y luego haga clic en **Ejecutar como administrador**.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre cómo crear archivos de entrada, vea [Microsoft BHOLD Suite Technical Reference](https://technet.microsoft.com/en-us/library/jj134935(v=ws.10).aspx) (Referencia técnica de Microsoft BHOLD Suite).
- [Guía de instalación de BHOLD](bhold-installation-guide.md)
- [Referencia para desarrolladores de BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historial de versiones de BHOLD](../reference/version-bhold-history.md)