---
title: Usar el SDK del Servidor Microsoft Azure Multi-factor Authentication para escenarios de activar PAM o SSPR | Microsoft Docs
description: Configure el SDK del Servidor Azure Multi-Factor Authentication como una segunda capa de seguridad cuando los usuarios activen roles en Privileged Access Management y en el Autoservicio de restablecimiento de contraseña.
keywords: ''
author: fimguy
ms.author: billmath
manager: mtillman
ms.date: 09/02/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 7191e445688cc9e3c5c02b9c6852c869a28a937a
ms.sourcegitcommit: ad0690bd57e3d056397108bf1c8417965d69a32c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2018
ms.locfileid: "43772685"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Usar el SDK del Servidor Azure Multi-factor Authentication para escenarios de activar PAM o SSPR
En el siguiente documento se describe cómo configurar el servidor Azure MFA como una segunda capa de seguridad cuando los usuarios activan roles en Priviledge Access Management o en el Autoservicio de restablecimiento de contraseña.

> [!IMPORTANT]
> Debido al anuncio de desuso del kit de desarrollo de software de Azure Multi-Factor Authentication, Se admitirá el SDK de Azure MFA para los clientes existentes hasta la fecha de retirada, el 14 de noviembre de 2018. Los clientes nuevos y actuales ya no podrán descargar el SDK a través del Portal de Azure clásico. En su lugar, para realizar la descarga, debe ponerse en contacto con el servicio de atención al cliente de Azure para recibir el paquete de credenciales de servicio generado para MFA. <br> El equipo de desarrollo de Microsoft está trabajando en los cambios de MFA mediante la integración con el SDK del Servidor Azure Multi-Factor Authentication.

En el siguiente artículo se describe la actualización de la configuración y los pasos para permitir un cambio sencillo del SDK de Azure MFA al SDK del Servidor Microsoft Azure Multi-Factor Authentication cuando se lance, ya que se incluirá en una revisión próxima (consulte el [historial de versiones](/reference/version-history.md) para ver los anuncios). 

## <a name="prerequisites"></a>Requisitos previos

Para usar el Servidor Azure Multi-Factor Authentication con MIM, necesita:

- Acceso a Internet desde cada servicio MIM o Servidor MFA que proporcione PAM y SSPR, para ponerse en contacto con el servicio Azure MFA.
- Una suscripción de Azure.
- La instalación ya usa el SDK de Azure MFA
- Licencias de Azure Active Directory Premium para los usuarios candidatos o un medio de licencia alternativo para Azure MFA.
- Números de teléfono de todos los usuarios candidatos.
- Revisión 4.5 (o posterior) de MIM; consulte el [historial de versiones](/reference/version-history.md) para ver los anuncios

## <a name="azure-multi-factor-authentication-server-configuration"></a>Configuración del Servidor Azure Multi-Factor Authentication 
> [!NOTE] 
> En la configuración necesitará un certificado SSL válido instalado para el SDK. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Paso 1: Descargar el Servidor Azure Multi-Factor Authentication del portal de Azure 
Inicie sesión en [Microsoft Azure Portal](https://portal.azure.com/) y descargue el Servidor Azure MFA.
![working-with-mfaserver-for-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>Paso 2: Generar credenciales de activación
Use el vínculo **Generar credenciales de activación para iniciar el uso** para generar las credenciales de activación. Cuando se hayan generado, guárdelas para usarlas más tarde.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>Paso 3: Instalar el Servidor Azure Multi-Factor Authentication
Una vez que haya descargado el servidor, [instálelo](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server).  Necesitará las credenciales de activación. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>Paso 4: Crear la aplicación web de IIS que hospedará el SDK
1. Abra el Administrador de IIS ![working-with-mfaserver-for-mim_iis.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Cree nueva llamada de sitio web "MIM MFASDK" y vincúlela a un directorio vacío ![working-with-mfaserver-for-mim_sdkweb.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Abra la consola de Microsoft Azure Multi-Factor Authentication y haga clic en el SDK del servicio web ![working-with-mfaserver-for-mim_sdkinstall.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. El asistente le guiará por el proceso de configuración. Seleccione "MIM MFASDK" y el grupo de aplicaciones.

> [!NOTE] 
> El asistente le pedirá que cree un grupo de administración. Puede encontrar más información en la documentación del Servidor Microsoft Azure Multi-factor Authentication (MFA).

5. Después, es necesario importar la cuenta del servicio MIM. Abra la consola de Microsoft Azure Multi-Factor Authentication y seleccione "Usuarios". Haga clic en "Importar desde Active Directory". Vaya a la cuenta de servicio, también conocida como "contoso\mimservice". Haga clic en "Importar" y "Cerrar" ![working-with-mfaserver-for-mim_importmimserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG). 
6. Edite la cuenta del servicio MIM para habilitar la consola de Microsoft Azure Multi-Factor Authentication ![working-with-mfaserver-for-mim_enableserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG).
7. Actualice la autenticación de IIS en el sitio web de "MIM MFASDK". Primero, deshabilite la "Autenticación anónima". Después, habilite la "Autenticación de Windows" ![working-with-mfaserver-for-mim_iisconfig.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG).
8. Por último, agregue la cuenta del servicio MIM para "PhoneFactor Admins" ![working-with-mfaserver-for-mim_addservicetomfaadmin.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG).

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>Configurar el servicio MIM para el Servidor Azure Multi-Factor Authentication 

### <a name="step-1-patch-server-to-452020"></a>Paso 1: Cambie la revisión del servidor a 4.5.202.0.
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Paso 2: Realice una copia de seguridad de MfaSettings.xml, ubicado en "C:\Archivos de programa\Microsoft Forefront Identity Manager\2010\Service" y ábralo.

### <a name="step-3-update-the-following-lines"></a>Paso 3: Agregue las siguientes líneas:
1. Quite o borre todas las siguientes líneas de entradas de configuración <br>
<LICENSE_KEY></LICENSE_KEY><br>
<GROUP_KEY></GROUP_KEY><br>
<CERT_PASSWORD></CERT_PASSWORD><br>
<CertFilePath></CertFilePath><br>

2. Actualice o agregue las líneas siguientes al MfaSettings.xml siguiente <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. Reinicie el servicio MIM y pruebe las funciones con el Servidor Azure Multi-factor Authentication.

> [!NOTE] 
> Para revertir la configuración, reemplace MfaSettings.xml con el archivo de copia de seguridad en el paso 2.


## <a name="next-steps"></a>Pasos siguientes

-    [Introducción al Servidor Azure Multi-Factor Authentication](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [¿Qué es Azure Multi-Factor Authentication?](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Use Custom Multi-Factor Authentication API to activate PAM or SSPR](Working-with-custommfaserver-for-mim.md) (Usar la API personalizada de Multi-factor Authentication para activar PAM o SSPR)
- [MIM version release history](./reference/version-history.md) (Historial de lanzamiento de versiones de MIM)