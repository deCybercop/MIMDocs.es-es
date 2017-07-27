---
title: "Implementación de PAM, paso 7: acceso de usuario | Microsoft Docs"
description: "Como paso final, conceda acceso temporal a un usuario con privilegios para comprobar que la implementación de Privileged Access Management se haya realizado correctamente."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 89d9b38177b91f64e746fea583684abcecc9d7ff
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="step-7--elevate-a-users-access"></a>Paso 7: Elevar los privilegios de acceso de un usuario

>[!div class="step-by-step"]
[« Paso 6](step-6-transition-group-to-pam.md)


En este paso se muestra que un usuario puede solicitar acceso a un rol a través de MIM.

## <a name="verify-that-jen-cannot-access-the-privileged-resource"></a>Comprobación de que Jen no acceder al recurso con privilegios
Sin privilegios elevados, Jen no puede acceder al recurso con privilegios del bosque CORP.

1. Cierre la sesión de CORPWKSTN para quitar cualquier conexión abierta almacenada en caché.
2. Inicie sesión en CORPWKSTN como *CONTOSO\Jen* y cambie a la vista **Escritorio**.
3. Abra un símbolo del sistema DOS.
4. Escriba el comando `dir \\corpwkstn\corpfs`. Debería aparecer el mensaje de error **Acceso denegado**.
5. Deje abierta la ventana de símbolo del sistema.

## <a name="request-privileged-access-from-mim"></a>Solicite acceso con privilegios a MIM.
1. En CORPWKSTN, aún como CONTOSO\Jen, escriba el siguiente comando.

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. Cuando se le pida, escriba la contraseña de la cuenta PRIV.Jen. Se mostrará una nueva ventana de símbolo del sistema.
3. Cuando aparezca la ventana de PowerShell, escriba los siguientes comandos.

    > [!NOTE]
    > Después de ejecutar estos comandos, todos los pasos siguientes están sujetos a limitación temporal.

    ```
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. Una vez completado, cierre la ventana de PowerShell.
5. En la ventana Comandos de DOS, escriba el siguiente comando:

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Escriba la contraseña de la cuenta PRIV.Jen. Se mostrará una nueva ventana de símbolo del sistema.

## <a name="validate-the-elevated-access"></a>Valide el acceso con privilegios elevados.
En la ventana abierta recientemente, escriba el siguiente comando:

```
whoami /groups
dir \\corpwkstn\corpfs
```

Si el comando dir produce un error con el mensaje **Acceso denegado**, vuelva a comprobar la relación de confianza.

## <a name="activate-the-privileged-role"></a>Activación del rol con privilegios
Para realizar la activación, solicite acceso con privilegios mediante el portal de ejemplo de PAM.

1. En CORPWKSTN, asegúrese de que ha iniciado sesión como CORP\Jen.
2. En la ventana de símbolo del sistema de DOS, escriba el siguiente comando.

    ```
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. Cuando se le pida, escriba la contraseña de la cuenta PRIV.Jen. Se mostrará una nueva ventana del explorador web.
4. Vaya a http://pamsrv.priv.contoso.local:8090 y asegúrese de que se vea una página web del portal de ejemplo.
5. En Internet Explorer, seleccione **Herramientas** > **Opciones de Internet** y haga clic en la pestaña **Seguridad**.
6. Haga clic en **Zona de intranet local** > **Sitios** > **Opciones avanzadas** y agregue el sitio web a la zona.
7. Cierre el cuadro de diálogo **Opciones de Internet** .
8. En la pestaña de la izquierda, haga clic en **Activar**. Seleccione el **rol de PAM** y haga clic en **Activar**.

> [!Note]
> En este entorno también puede aprender a desarrollar aplicaciones que usan la API de REST de PAM, que se describe en [Privileged Access Management REST API Reference](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference) (Referencia de la API de REST de Privileged Access Management).

## <a name="summary"></a>Resumen
Una vez que haya completado los pasos de este tutorial, habrá mostrado un escenario de Privileged Access Management en el que los privilegios de usuario se elevan durante un período limitado de tiempo, lo que permite al usuario acceder a recursos protegidos con una cuenta con privilegios independiente. Desde el momento en que finalice la sesión de elevación, la cuenta con privilegios no podrá acceder al recurso protegido. El administrador de PAM es quien decide qué grupos de seguridad representan roles con privilegios. Una vez que los derechos de acceso se migran al sistema de Privileged Access Management, el acceso que anteriormente era posible con la cuenta de usuario original ahora solo es posible si se inicia sesión con una cuenta con privilegios especial que se obtiene previa solicitud. Como resultado, las pertenencias a grupos para grupos con muchos privilegios están en vigor durante un período de tiempo limitado.

>[!div class="step-by-step"]
[« Paso 6](step-6-transition-group-to-pam.md)
