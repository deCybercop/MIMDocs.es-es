---
title: 'Implementación de PAM, paso 6: desplazamiento del grupo | Microsoft Docs'
description: Migre un grupo al bosque PRIV de modo que se pueda administrar con Privileged Access Management.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3a7359c664e1c4aeacbc571242c2b348be186a89
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289591"
---
# <a name="step-6--transition-a-group-to-privileged-access-management"></a>Paso 6: Realizar la transición de un grupo a Privileged Access Management

> [!div class="step-by-step"]
> [«Paso 5 ](step-5-establish-trust-between-priv-corp-forests.md)
> [Paso 7»](step-7-elevate-user-access.md)

La creación de cuentas con privilegios en el bosque PRIV se realiza mediante cmdlets de PowerShell. Estos cmdlets realizan las siguientes funciones:

- Cree un nuevo grupo en el bosque PRIV con el mismo identificador de seguridad (SID) que un grupo del bosque CORP.  
- Cree un objeto en la base de datos del servicio MIM correspondiente al grupo del bosque PRIV.  
- Por cada cuenta de usuario, cree dos objetos en la base de datos del servicio MIM que correspondan al usuario del bosque CORP y a la nueva cuenta de usuario del bosque PRIV.  
- Crean un objeto de rol de PAM en la base de datos del Servicio MIM.  

Los cmdlets se deben ejecutar una vez para cada grupo y una vez para cada miembro de un grupo. Los cmdlets de migración no cambian ni modifican ningún usuario ni grupo del bosque CORP: eso lo hará posteriormente y de forma manual el administrador PAM.

1. Inicie sesión en PAMSRV, ya sea directamente o desde una estación de trabajo PRIV, como *PRIV\MIMAdmin*.

2.  Inicie PowerShell y escriba los siguientes comandos.

```PowerShell
   Import-Module MIMPAM
   Import-Module ActiveDirectory
```

3. Cree una cuenta de usuario correspondiente en PRIV para una cuenta de usuario de un bosque existente para fines demostrativos.

   En PowerShell, escriba los siguientes comandos.  Si no usó el nombre *Jen* para crear el usuario en contoso.local anteriormente, cambie los parámetros del comando según corresponda. La contraseña 'Pass@word1' es solo un ejemplo y debe cambiarse por un valor de contraseña único.

   ```PowerShell
       $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen
       $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
       Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
       Set-ADUser –identity priv.Jen –Enabled 1
   ```

4. Copie un grupo y su miembro, Jen, desde CONTOSO en el dominio PRIV para fines demostrativos.

    Ejecute los siguientes comandos, especificando la contraseña de administrador del dominio CORP (CONTOSO\Administrador) cuando se le pida:

   ```PowerShell
        $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
        $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca
        $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
   ```

    Como referencia, el comando **New-PAMGroup** toma los siguientes parámetros:

     -   El nombre de dominio del bosque CORP en forma NetBIOS.  
     -   El nombre del grupo a copiar desde ese dominio.  
     -   El nombre NetBIOS del controlador de dominio del bosque CORP.  
     -   Las credenciales de un usuario administrador de dominio del bosque CORP.  

5. (Opcional) En CORPDC, quite la cuenta de Jen del grupo **CONTOSO CorpAdmins**, si sigue ahí.  Esto solo es necesario con fines demostrativos para ilustrar cómo se pueden asociar los permisos con cuentas creadas en el bosque PRIV.

   1.  Inicie sesión en CORPDC como *CONTOSO\Administrador*.

   2.  Inicie PowerShell, ejecute el siguiente comando y confirme el cambio.

       ```PowerShell
       Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
       ```


Si quiere demostrar que los derechos de acceso entre bosques son eficaces para la cuenta de administrador del usuario, vaya al paso siguiente.

> [!div class="step-by-step"]
> [«Paso 5 ](step-5-establish-trust-between-priv-corp-forests.md)
> [Paso 7»](step-7-elevate-user-access.md)
