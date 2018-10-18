---
title: 'Implementación de PAM, paso 5: vínculo de bosque | Microsoft Docs'
description: Establezca la confianza entre los bosques PRIV y CORP para que los usuarios con privilegios en PRIV puedan seguir teniendo acceso a los recursos de CORP.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 11/29/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: a18ddac2c324cbefa0c87e24cec654175c9021dc
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333390"
---
# <a name="step-5--establish-trust-between-priv-and-corp-forests"></a>Paso 5: Establecer la confianza entre bosques PRIV y CORP

> [!div class="step-by-step"]
> [« Paso 4](step-4-install-mim-components-on-pam-server.md)
> [Paso 6 »](step-6-transition-group-to-pam.md)

En cada dominio CORP, como contoso.local, los controladores de dominio CONTOSO y PRIV deben estar enlazados mediante una relación de confianza. Esto permite a los usuarios del dominio PRIV acceder a los recursos del dominio CORP.

## <a name="connect-each-domain-controller-to-its-counterpart"></a>Conexión de cada controlador de dominio a su equivalente

Antes de establecer la confianza, cada controlador de dominio debe configurarse para la resolución de nombres DNS de su equivalente, en función de la dirección IP del otro controlador de dominio o servidor DNS.

1.  Si los controladores de dominio o el servidor con el software MIM están implementados como máquinas virtuales, asegúrese de que no haya ningún otro servidor DNS que esté proporcionando servicios de nombres de dominio a estos equipos.
    - Si las máquinas virtuales tienen varias interfaces de red, incluidas interfaces de red conectadas a redes públicas, puede que tenga que deshabilitar esas conexiones de forma temporal o invalidar la configuración de interfaz de red de Windows. Es importante asegurarse de que ninguna máquina virtual use una dirección de servidor DNS proporcionada por DHCP.

2.  Asegúrese de que todos los controladores de dominio CORP existentes puedan enrutar nombres al bosque PRIV. Inicie PowerShell en cada controlador de dominio situado fuera del bosque PRIV, como CORPDC, y escriba el siguiente comando:

    ```cmd
    nslookup -qt=ns priv.contoso.local.
    ```
    Compruebe que la salida indica un registro nameserver para el dominio PRIV con la dirección IP correcta.

3.  Si el controlador de dominio no puede enrutar el dominio PRIV, use **Administrador de DNS** (se encuentra en **Iniciar** > **Herramientas de aplicación** > **DNS**) para configurar el reenvío de nombres DNS del dominio PRIV a la dirección IP de PRIVDC. Si es un dominio superior (por ejemplo, contoso.local), expanda los nodos de este controlador de dominio y su dominio, como **CORPDC** > **Zonas de búsqueda directa** > **contoso.local**, y asegúrese de que haya una clave denominada **priv** como un tipo de servidor DNS.

    ![Captura de pantalla de estructura de archivo de clave priv](./media/PAM_GS_DNS_Manager.png)

## <a name="establish-trust-on-pamsrv"></a>Establecimiento de la confianza en PAMSRV

En PAMSRV, establezca confianza unidireccional con cada dominio como CORPDC para que los controladores de dominio CORP confíen en el bosque PRIV.

1. Inicie sesión como administrador de dominio PRIV (PRIV\Administrador) en PAMSRV.

2.  Inicie PowerShell.

3.  Escriba los siguientes comandos de PowerShell para cada bosque existente. Cuando se le pida, escriba la credencial del administrador de dominio CORP (CONTOSO\Administrador).

    ```PowerShell
    $ca = get-credential
    New-PAMTrust -SourceForest "contoso.local" -Credentials $ca
    ```

4.  Escriba los siguientes comandos de PowerShell para cada dominio de los bosques existentes. Cuando se le pida, escriba la credencial del administrador de dominio CORP (CONTOSO\Administrador).

    ```PowerShell
    $ca = get-credential
    New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials $ca
    ```

## <a name="give-forests-read-access-to-active-directory"></a>Concesión de acceso de lectura a los bosques en Active Directory

En cada bosque existente, habilite el acceso de lectura a AD para los administradores de PRIV y el servicio de supervisión.

1. Inicie sesión en el controlador de dominio de bosque CORP existente, (CORPDC), como administrador de dominio del dominio de nivel superior de ese bosque (Contoso\Administrador).  
2. Inicie **Usuarios y equipos de Active Directory**.  
3. Haga clic con el botón derecho en el dominio **contoso.local** y seleccione **Delegar control**.  
4. En la pestaña Usuarios y grupos seleccionados, haga clic en **Agregar**.  
5. En la ventana Seleccionar usuarios, equipos o grupos, haga clic en **Ubicaciones** y cambie la ubicación a *priv.contoso.local*.  En el nombre de objeto, escriba *Admins. del dominio* y haga clic en **Comprobar nombres**. Cuando aparezca una ventana emergente, escriba el nombre de usuario *priv\administrador* y su contraseña.  
6. Después de Admins. del dominio, agregue "*; MIMMonitor*". Una vez que los nombres **Admins. del dominio** y **MIMMonitor** estén subrayados, haga clic en **Aceptar** y luego en **Siguiente**.  
7. En la lista de tareas comunes, seleccione **Leer toda la información del usuario** y haga clic en **Siguiente** y en **Finalizar**.  
8. Cierre Usuarios y equipos de Active Directory.

9. Abra una ventana de PowerShell.
10. Use `netdom` para asegurarse de que el historial de SID está habilitado y el filtrado por SID deshabilitado. Tipo:
    ```cmd
    netdom trust contoso.local /quarantine:no /domain priv.contoso.local
    netdom trust /enablesidhistory:yes /domain priv.contoso.local
    ```
    La salida debe indicar **Habilitando el historial de SID para esta confianza** o **El historial de SID ya está habilitado para esta confianza**.

    La salida también debe indicar **No está habilitado el filtrado por SID para esta confianza**. Consulte [Disable SID filter quarantining (Deshabilitar la cuarentena del filtrado por SID)](http://technet.microsoft.com/library/cc772816.aspx) para obtener más información.

## <a name="start-the-monitoring-and-component-services"></a>Inicio de los servicios de supervisión y de componentes

1.  Inicie sesión como administrador de dominio PRIV (PRIV\Administrador) en PAMSRV.

2.  Inicie PowerShell.

3.  Escriba los siguientes comandos de PowerShell.

    ```cmd
    net start "PAM Component service"
    net start "PAM Monitoring service"
    ```

En el paso siguiente se moverá un grupo a PAM.

> [!div class="step-by-step"]
> [« Paso 4](step-4-install-mim-components-on-pam-server.md)
> [Paso 6 »](step-6-transition-group-to-pam.md)
