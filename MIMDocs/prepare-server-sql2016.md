---
title: Configuración de SQL Server para Microsoft Identity Manager 2016 SP1 | Microsoft Docs
description: Instale SQL Server 2016 para preparar la instalación de MIM 2016.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2dc7c1ffecbd4f52ab031512f501922f458324a3
ms.sourcegitcommit: 4ae62943aea3aab622195896956c5c88ecd9e97c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2019
ms.locfileid: "53815909"
---
# <a name="set-up-an-identity-management-server-sql-server-2016"></a>Configuración de un servidor de administración de identidades: SQL Server 2016

> [!div class="step-by-step"]
> [«Windows Server 2016](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
> 
> [!NOTE]
> Este tutorial usa los valores y nombres de ejemplo de una empresa llamada Contoso. Reemplácelos con sus propios valores. Por ejemplo:
> - Nombre del controlador de dominio: **corpdc**
> - Nombre de dominio: **contoso**
> - Nombre de servidor del servicio MIM: **corpservice**
> - Nombre del servidor de sincronización de MIM: **corpsync**
> - Nombre de SQL Server: **corpsql**
> - Contraseña - <strong>Pass@word1</strong>

## <a name="install-sql-server-2016-standardenterprise-edition"></a>Instale **SQL Server 2016 Standard/Enterprise Edition**.

1. Inicie **PowerShell** como administrador de dominio.

2. Cambie al directorio donde se encuentra el programa de instalación de SQL Server.

3. Escriba los siguientes comandos:

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```
    
Para obtener más información sobre las cuentas y los servicios de implementación de SQL, consulte [este vínculo](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)
> [!NOTE]
> SSMS ya no se incluye en SQL 2016. Los detalles de la descarga se pueden encontrar en [este vínculo](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017).
> 
> [!div class="step-by-step"]  
> [«Windows Server 2016](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
