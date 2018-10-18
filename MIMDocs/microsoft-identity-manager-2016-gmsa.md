---
title: Conversión de servicios específicos de MIM a gMSA | Microsoft Docs
description: Tema en que se describen los pasos básicos para configurar gMSA.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: d3c0b6677c42d4f14d4f6255a2a661d3ef23661d
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358302"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>Conversión de servicios específicos de MIM a gMSA

En esta guía se describen los pasos básicos para configurar gMSA para los servicios compatibles. El proceso de conversión a gMSA es sencillo una vez que se ha preconfigurado el entorno.

Revisión necesaria: \<vínculo al KB más reciente\>

Se admiten:

-   Sincronización de MIM service(FIMSynchronizationService)
-   MIM Service(FIMService)
-   Registro de contraseña de MIM
-   Restablecimiento de contraseña de MIM
-   Supervisión de PAM Service(PamMonitoringService)
-   Servicio de componentes de PAM (PrivilegeManagementComponentService)

No se admiten:

-   No se admite el Portal de MIM; forma parte del entorno de SharePoint y necesita implementar en modo de granja y [configurar el cambio de contraseña automático en SharePointServer](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
-   Todos los agentes de administración
-   Administración de certificados de Microsoft
-   BHOLD

<a name="general-information"></a>Información general 
--------------------

Lectura necesaria para completar la configuración y obtener información

-   [Información general de las cuentas de servicio administradas de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/library/jj128430(v=ws.11).aspx>

Primer paso en el controlador de dominio de Windows

1.  Cree la clave raíz del Servicio de distribución de claves (KDS), solo una vez por cada dominio, en caso de que sea necesario. El servicio KDS usa la clave raíz en los controladores de dominio (junto con otra información) para generar las contraseñas.

    -   Add-KDSRootKey –EffectiveImmediately

    -   "–EffectiveImmediately" supone esperar hasta \~10 horas o lo que resulte necesario para replicar en todos los controladores de dominio. Este procedimiento tardó aproximadamente 1 hora para dos controladores de dominio.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Servicio de sincronización
-----------------------

1.  Cree un grupo llamado “MIMSync_Servers” y agregue todos los servidores de sincronización a este grupo.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  Desde Windows PowerShell, ejecute el comando siguiente como administrador de dominio con una cuenta de equipo que ya esté unida al dominio.

    -   New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Obtenga detalles de gMSA para la sincronización:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Si ejecuta el servicio PCNS, necesitará actualizar la delegación.

    -   Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames \@{Add="PCNSCLNT/mimsync.contoso.com"}

3. A continuación, en los servicios de sincronización, asegúrese de realizar una copia de seguridad de la clave de cifrado, ya que se solicitará al instalar el modo de cambio.

    -   En el servidor en que está instalado el servicio de sincronización, localice la herramienta de administración de claves del servicio de sincronización.

    -   De forma predeterminada, la opción** Exportar conjunto de claves** ya está seleccionada.

    -   Haga clic en **Siguiente**.

    -   Ahora se le pedirá que escriba la información de la cuenta de sincronización existente.

    -   Escriba y verifique la información de la cuenta de sincronización de FIM.

        -   Nombre de cuenta: nombre de la cuenta del servicio de sincronización utilizada durante la instalación inicial.

        -   Contraseña: contraseña de la cuenta del servicio de sincronización.

        -   Dominio: dominio del que forma parte la cuenta del servicio de sincronización.

    -   Haga clic en **Siguiente**.

    -   Si escribió algo incorrectamente, recibirá el siguiente error:

    -   Ahora que ha escrito correctamente la información de la cuenta, se le presentará una opción para cambiar el destino (ubicación del archivo de exportación) de la clave de cifrado de copia de seguridad.

        -   La ubicación predeterminada del archivo de exportación es **C:\\Windows\\system32**\\miiskeys-1.bin.

4. Instale Microsoft Identity Manager SP1 Synchronization Service, compilación 4.4.1302.0. Puede encontrarse en el centro de descarga de licencias por volumen o en el sitio de descargas de MSDN. Una vez completada la instalación, asegúrese de guardar el conjunto de claves miiskeys.bin.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Instale la última [revisión 4.5.x.x](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) o versiones posteriores.

- Una vez aplicada la revisión, detenga el servicio de sincronización de FIM.
- Programas y características del Panel de Control en Microsoft Identity Manager
- Servicio de sincronización; Cambiar -\> Siguiente -\> Configurar -\> Siguiente

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  Borre el nombre de la cuenta.
-  Escriba el nombre de la cuenta de servicio **MIMSyncGMSA** con el símbolo \$ como en la captura de pantalla.
- Deje la contraseña vacía.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Siguiente -\> Siguiente -\> Instalar.
- Restaure el conjunto de claves a partir del archivo .bin guardado.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> El permiso de SQL agregado a la cuenta se crea para el inicio de sesión; por tanto, debe permitir que el usuario aplique el permiso de modo de cambio para agregar la cuenta y el dbo a la base de datos del servicio de sincronización.

## <a name="mim-service"></a>Servicio MIM
-----------

>[!IMPORTANT]
>El proceso siguiente debe usarse al convertir primero las cuentas relacionadas con el servicio MIM en cuentas de gMSA. Los cmdlets de PowerShell indicados en el apéndice solo se pueden usar para cambiar la información de la cuenta una vez completada la configuración inicial.*

1.  Cree cuentas administradas de grupo para el servicio MIM, la API de REST de PAM, el servicio de supervisión de PAM, el servicio de componentes de PAM, el portal de registro de SSPR y el portal de restablecimiento de SSPR.

    -   Asegúrese de actualizar la delegación de gMSA y SPN.
        -   Set-ADServiceAccount -Identity \<account\> -ServicePrincipalNames \@{Add="\<SPN\>"}
        -   Delegación
            -   Set-ADServiceAccount -Identity \<gsmaaccount\> -TrustedForDelegation \$true
        -   Delegación restringida
            -   \$delspns = 'http/mim', 'http/mim.contoso.com'
            -   New-ADServiceAccount -Name \<gsmaaccount\> -DNSHostName \<gsmaaccount\>.contoso.com -PrincipalsAllowedToRetrieveManagedPassword \<group\> -ServicePrincipalNames \$spns -OtherAttributes \@{'msDS-AllowedToDelegateTo'=\$delspns }

2.  Agregue la cuenta para el servicio MIM en grupos de sincronización. Es necesario para SSPR.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **NOTA**.  Es un problema conocido que los servicios que usan cuentas administradas se bloquean después de reiniciar el servidor debido a que el servicio de distribución de claves de Microsoft no se inicia después de reiniciar Windows. No se pudo iniciar el servicio y tampoco se pudo reiniciar Windows. El problema es reproducible al menos en Windows Server 2012 R2. Para dar una solución alternativa a este problema, ejecute el comando 

-   **sc triggerinfo kdssvc start/networkon**

    para iniciar el servicio de distribución de claves de Microsoft cuando la red está activada (normalmente en una fase temprana del ciclo de arranque).

    Vea la explicación sobre un problema similar: <https://social.technet.microsoft.com/Forums/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>.

4.  Ejecute el MSI elevado del servicio MIM y seleccione Cambiar.

5.  En la página “Configure main server connection” (Configurar conexión de servidor principal), marque la casilla “Use different account for Exchange (for managed accounts)” (Usar cuenta diferente para Exchange para cuentas administradas). De esta forma, tendrá la opción de usar la cuenta anterior que tiene un buzón de correo o de usar un buzón de correo en la nube.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  En la página “Configure MIM Service account” (Configurar cuenta de servicio de MIM), escriba la cuenta de servicio con el símbolo \$ al final. Escriba también la contraseña de la cuenta de correo electrónico del servicio. La contraseña de la cuenta de servicio debe estar deshabilitada.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Como la función LogonUser no funciona para las cuentas administradas, en la página siguiente se advertirá lo siguiente “Please check if Service Account is secure in its current configuration” (Compruebe si la cuenta de servicio está protegida en su configuración actual).

![cid:image007.png\@01D36EB7.562E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  En la página “Configure Privileged Access Management REST API” (Configurar la API de REST de la administración de acceso con privilegios), escriba el nombre de la cuenta del grupo de aplicaciones con el símbolo \$ al final y deje el campo Contraseña vacío.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  En la página “Configure PAM Component Service” (Configurar servicio de componentes de PAM), escriba el nombre de la cuenta de servicio con el símbolo \$ al final y deje el campo Contraseña vacío.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![cid:image010.png\@01D36EB8.A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  En la página “Configure Privileged Access Management Monitoring Service” (Configurar servicio de supervisión de Privileged Access Management), escriba el nombre de la cuenta de servicio con el símbolo \$ al final y deje el campo Contraseña vacío.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  En la página “Configure MIM Password Registration Portal” (Configurar portal de registro de contraseña de MIM), escriba el nombre de la cuenta con el símbolo \$ al final y deje el campo Contraseña vacío.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  En la página “Configure MIM Password Reset Portal” (Configurar portal de restablecimiento de contraseña de MIM), escriba el nombre de la cuenta con el símbolo \$ al final y deje el campo Contraseña vacío.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Complete la instalación.

Nota:

-  Después de la instalación, se crean dos claves en el registro mediante la ruta de acceso
    - “HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Forefront Identity
    - Manager\\2010\\Service” para almacenar la contraseña de Exchange cifrada. Una para
    - Exchange Online y otra para Exchange local (una de ellas debe estar
    - vacía).

![cid:image014.jpg\@01D36F53.303D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- Para actualizar la contraseña, proporcionamos un script [descargar aquí](microsoft-identity-manager-2016-gmsascript.md), para que el cliente no tenga que ejecutar el modo de cambio.

- Para cifrar la contraseña de Exchange, el instalador crea un servicio adicional y
    - lo ejecuta en la cuenta administrada. Los mensajes siguientes se agregarán en el
    - registro de eventos de la aplicación durante la instalación.

![cid:image016.jpg\@01D36F53.303D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
