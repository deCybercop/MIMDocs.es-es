---
title: Actualización de cuentas de servicios específicos de MIM para notificaciones y aprobaciones cuando gMSA está habilitado | Microsoft Docs
description: Tema en que se describen los pasos básicos para configurar gMSA.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: af293106b11651dfd720c03165938f8079a3e49b
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358233"
---
# <a name="update-of-mim-specific-service-account-for-notifications-to-gmsa"></a>Actualización de la cuenta de servicio específico de MIM para notificaciones a gMSA
===========================================

Actualice la contraseña para las cuentas almacenadas; a continuación se indica el comando de PowerShell para que los clientes no tengan que ejecutar el modo de cambio.

PowerShell: Actualización de cuenta de Office 365:

```powershell
#O365update.ps1
#author&contributers: David Steadman, Anthony Ho
###############################
# Script for changing Exchange password
# 1. Could be used for Exchange online and normal account
# 2. Could be used for Exchange Online or On-Premise for managed accounts
###############################
Write-host "Is service run under managed account (Default is No)" -ForegroundColor Yellow 
$answer = Read-Host " ( y / n ) " 
$isManaged = $false;
if ($answer -eq "Y") { 
  $isManaged = $true;
} 

if($isManaged)
{
    Write-host "Changing Exchange (Online or On-Premise) password for service running under managed account" -ForegroundColor Green
    Write-host "You should be logged as someone with local admin privilege to set the value in registry" -ForegroundColor Green
    Write-host "NOTE: In case script fails, please ensure that MIMKeyProtectService is uninstalled (uninstall it if it is necessary)" -ForegroundColor Green    
    ###############################
    #Script executes following steps
    # 1. Install service to encrypt password 
    # 2. Run the service under service account
    # 3. Send password to the service
    # 4. Receive encrypted password
    # 5. Stop and uninstall service
    # 6. Save password in registry
    ###############################
    # NOTE: In case script fails, please ensure that MIMKeyProtectService is uninstalled (uninstall it if it is necessary)
    # If MIMKeyProtectService remains installed, installer will fail when it tries to use the service next time
    ###############################
    $account = Read-Host "Please enter managed account with $ symbol at the end (format: domain\managedaccount$)"
    $securePssword = Read-Host "Please enter the password" -AsSecureString
    $secureConfirmPassword = Read-Host "Please confirm the password" -AsSecureString
    $bstr = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($securePssword)
    $password = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($bstr)
    $bstr = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($secureConfirmPassword)
    $confirmPassword = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($bstr)
    if ($password -ne $confirmPassword) {
    throw "Password does not match"
    }

    #Create service
    sc.exe create MIMKeyProtectService binpath= "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\Microsoft.IdentityManagement.KeyProtectService.exe" displayname= "MIM Key Protect Service" obj= "$account"
    #sc.exe description MIMKeyProtectService "Key protection/unprotection service for Microsoft Identity Manager. It is used for group managed accounts"

    #Start service with parameter "e" - Encrypt
    $service = Get-Service "MIMKeyProtectService"
    $service.Start("e");
    $service.WaitForStatus("Running", '00:00:30');
    if ($service.Status -eq "Running") {
    Write-Host "Service is running"

    #Send password
    $pipe = new-object System.IO.Pipes.NamedPipeClientStream '.','keypipe','Out';
    $pipe.Connect();
    $sw = new-object System.IO.StreamWriter $pipe;
    $sw.AutoFlush = $true;
    $sw.Write($password);
    $sw.Dispose();
    $pipe.Dispose();

    #Receive encrypted password
    $pipe = new-object System.IO.Pipes.NamedPipeClientStream '.','keypipe','In';
    $pipe.Connect();
    $sr = new-object System.IO.StreamReader $pipe;
    $encryptedData = $sr.ReadToEnd();
    $sr.Dispose();
    $pipe.Dispose();
    }

    #Stop and delete service
    $service.Stop();
    $service.WaitForStatus("Stopped", '00:00:30');
    if ($service.Status -eq "Stopped") {
    Write-Host "Service is stopped"
    sc.exe delete MIMKeyProtectService
    }

    Write-Host "Encrypted Password" -ForegroundColor Cyan
    Write-Host ($encryptedData) -ForegroundColor DarkGreen
    Set-ItemProperty -Path HKLM:"\SOFTWARE\Microsoft\Forefront Identity Manager\2010\Service" -Name EncryptedExchangeOnlineAccountPassword -Value $encryptedData   
}
else
{
    Write-host "Changing Exchange Online password for service running under normal account" -ForegroundColor Green
    Write-host "You should be logged as FIMService service account to encrypt the pwd" -ForegroundColor Green
    Write-host "If account don't have rights to write in registry login as someone with local admin privilege to set the value" -ForegroundColor Green
    ## RUNAS  /user:contoso\MIMService "powershell"
    #Login as mimservice account and then impersonate to update office365 login
    #We need to do the following:
    #1.    Login as FIMService service account to encrypt the pwd
    #2.    Login as someone with local admin privilege to set the value in registry
    ###############################
    Add-Type -AssemblyName System.Security
    #$o365user = Read-Host "Please enter office 365 email"
    $securePssword = Read-Host "Please enter the password" -AsSecureString
    $secureConfirmPassword = Read-Host "Please confirm the password" -AsSecureString
    $bstr = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($securePssword)
    $password = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($bstr)
    $bstr = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($secureConfirmPassword)
    $confirmPassword = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($bstr)
    if ($password -ne $confirmPassword) {
    throw "Password does not match"
    }
    # Convert a plain text string to a character array
    # and cast it to a byte array.
    $bytes = [System.Text.Encoding]::Unicode.GetBytes($password)
    # Encrtyped the byte array.
    $encryptedBytes = [System.Security.Cryptography.ProtectedData]::Protect(
    $bytes,
    $null,
    [System.Security.Cryptography.DataProtectionScope]::CurrentUser)
    $encryptedData = [Convert]::ToBase64String($encryptedBytes)
    Write-Host "Encrypted Password" -ForegroundColor Cyan
    Write-Host ($encryptedData) -ForegroundColor DarkGreen
    Set-ItemProperty -Path HKLM:"\SOFTWARE\Microsoft\Forefront Identity Manager\2010\Service" -Name EncryptedExchangeOnlineAccountPassword -Value $encryptedData   
}
```
