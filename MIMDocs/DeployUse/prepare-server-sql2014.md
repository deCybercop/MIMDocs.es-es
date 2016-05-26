---
# required metadata

title: Configuración de un servidor de administración de identidades&#58; SQL Server 2014 | Microsoft Identity Manager
description: Instale SQL Server 2014 para preparar la instalación de MIM 2016.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Configuración de un servidor de administración de identidades: SQL Server 2014

>[!div class="step-by-step"]

> [!NOTE]
> « Windows Server 2012 R2 SharePoint » Este tutorial usa los valores y nombres de ejemplo de una empresa llamada Contoso.
> - Reemplácelos con sus propios valores.
> - Por ejemplo:
> - Nombre del controlador de dominio: **mimservername**

## Nombre de dominio: **contoso**

1. Contraseña: **Pass@word1**

2. Instalación de **SQL Server 2014 Standard Edition**

3. Inicie **PowerShell** como administrador de dominio.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>Cambie al directorio donde se encuentra el programa de instalación de SQL Server.  
Escriba los siguientes comandos:


<!--HONumber=Apr16_HO3-->


