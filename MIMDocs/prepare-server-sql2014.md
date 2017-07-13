---
title: "Configuración de SQL Server para Microsoft Identity Manager 2016 | Microsoft Docs"
description: "Instale SQL Server 2014 para preparar la instalación de MIM 2016."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 105d2320ed5a0d610e8e6c5f459366680e3f8a77
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# Configuración de un servidor de administración de identidades: SQL Server 2014
<a id="set-up-an-identity-management-server-sql-server-2014" class="xliff"></a>

>[!div class="step-by-step"]
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> Este tutorial usa los valores y nombres de ejemplo de una empresa llamada Contoso. Reemplácelos con sus propios valores. Por ejemplo:
> - Nombre del controlador de dominio: **mimservername**
> - Nombre de dominio: **contoso**
> - Contraseña - **Pass@word1**

## Instalación de **SQL Server 2014 Standard Edition**
<a id="install-sql-server-2014-standard-edition" class="xliff"></a>

1. Inicie **PowerShell** como administrador de dominio.

2. Cambie al directorio donde se encuentra el programa de instalación de SQL Server.

3. Escriba los siguientes comandos:

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="step-by-step"]  
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)
