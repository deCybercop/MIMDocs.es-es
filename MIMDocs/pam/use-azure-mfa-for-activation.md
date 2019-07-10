---
title: Uso de Azure MFA para la activación de PAM | Microsoft Docs
description: Configure Azure MFA como segunda capa de seguridad si sus usuarios activan roles en Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 07/06/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.openlocfilehash: 72dd1d3cf34e28567fa672b747a04347b150797e
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690782"
---
# <a name="using-azure-mfa-for-activation"></a>Uso de Azure MFA para la activación
> [!IMPORTANT]
> Debido al anuncio de desuso del kit de desarrollo de software de Azure Multi-Factor Authentication, Se admitirá el SDK de Azure MFA para los clientes existentes hasta la fecha de retirada, el 14 de noviembre de 2018. Los clientes nuevos y actuales ya no podrán descargar el SDK a través del Portal de Azure clásico. En su lugar, para realizar la descarga, debe ponerse en contacto con el servicio de atención al cliente de Azure para recibir el paquete de credenciales de servicio generado para MFA. <br> El equipo de desarrollo de Microsoft está trabajando en los cambios de MFA mediante la integración del SDK del servidor MFA.  Se incluirá en una próxima revisión; vea el [historial de versiones](../reference/version-history.md) para consultar los anuncios. 


Al configurar un rol de PAM, puede decidir cómo autorizar a los usuarios que solicitan activar el rol. Las opciones que implementa la actividad de autorización de PAM son:

- Aprobación del propietario de rol
- [Azure Multi-Factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)

Si ninguna de las comprobaciones está habilitada, los usuarios candidatos se activan automáticamente para el rol correspondiente.

Microsoft Azure Multi-Factor Authentication (MFA) es un servicio de autenticación que requiere que los usuarios verifiquen sus intentos de inicio de sesión con una aplicación móvil, una llamada de teléfono o un mensaje de texto. Está disponible para su uso con Microsoft Azure Active Directory y como servicio para aplicaciones empresariales en la nube y locales. En el escenario de PAM, Azure MFA proporciona un mecanismo de autenticación adicional. Puede usarse Azure MFA para la autorización, con independencia de cómo se autentica un usuario en el dominio PRIV de Windows.

## <a name="prerequisites"></a>Requisitos previos

Para usar Azure MFA con MIM, necesita:

- Acceso a Internet desde cada servicio MIM que proporcione PAM, para ponerse en contacto con el servicio Azure MFA.
- Una suscripción de Azure.
- Licencias de Azure Active Directory Premium para los usuarios candidatos o un medio de licencia alternativo para Azure MFA.
- Números de teléfono de todos los usuarios candidatos.

## <a name="creating-an-azure-mfa-provider"></a>Creación de un proveedor de Azure MFA

En esta sección, vamos a configurar el proveedor de Azure MFA en Microsoft Azure Active Directory.  Si ya usa Azure MFA, tanto de forma independiente como configurado con Azure Active Directory Premium, vaya a la sección siguiente.

1.  Abra un explorador web y conéctese al [portal clásico de Azure](https://manage.windowsazure.com) como administrador de suscripciones de Azure.

2.  En la esquina inferior izquierda, haga clic en **Nuevo**.

3.  Haga clic en **Servicios de aplicaciones > Active Directory > Proveedor de autenticación multifactor > Creación rápida**.

4.  En el campo **Nombre** , escriba **PAM**y, en el campo Modelo de uso, seleccione Por usuario habilitado. Si ya tiene un directorio de Azure AD, seleccione ese directorio. Por último, haga clic en **Crear**.

## <a name="downloading-the-azure-mfa-service-credentials"></a>Descarga de las credenciales del servicio Azure MFA

A continuación, se generará un archivo que incluye el material de autenticación para que PAM se ponga en contacto con Azure MFA.

1. Abra un explorador web y conéctese al [portal clásico de Azure](https://manage.windowsazure.com) como administrador de suscripciones de Azure.

2.  Haga clic en **Active Directory** en el menú del portal de Azure y, a continuación, haga clic en la pestaña **Proveedores de autenticación multifactor** .

3.  Haga clic en el proveedor de Azure MFA que va a usar para PAM y, a continuación, haga clic en **Administrar**.

4.  En el panel izquierdo de la ventana nueva, bajo **Configurar**, haga clic en **Configuración**.

5.  En la ventana **Azure Multi-Factor Authentication** , haga clic en **SDK** bajo **Descargas**.

6.  Haga clic en el vínculo **Descargar** de la columna ZIP del archivo con lenguaje **SDK para ASP.net 2.0 C\#** .

![Captura de pantalla de la descarga de un SDK de Multi-Factor Authentication](media/PAM-Azure-MFA-Activation-Image-1.png)

7.  Copie el archivo ZIP resultante en cada sistema donde esté instalado el servicio de MIM. 

>[!NOTE]
> El archivo ZIP contiene material de claves que se usa para autenticar el servicio Azure MFA.

## <a name="configuring-the-mim-service-for-azure-mfa"></a>Configuración del servicio MIM para Azure MFA

1.  Inicie sesión en el equipo donde está instalado el servicio MIM como administrador o como el usuario que instaló MIM.

2.  Cree una nueva carpeta de directorio en el directorio donde se instaló el servicio MIM, como ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts```.

3.  En el Explorador de Windows, vaya a la carpeta ```pf\certs``` del archivo ZIP que descargó en la sección anterior. Copie el archivo ```cert\_key.p12``` en el nuevo directorio.

4.  Con el Explorador de Windows, vaya a la carpeta ```pf``` del archivo ZIP y abra el archivo ```pf\_auth.cs``` en un editor de texto como WordPad.

5. Busque estos tres parámetros: ```LICENSE\_KEY```, ```GROUP\_KEY```, ```CERT\_PASSWORD```.

![Captura de pantalla del copiado de los valores del archivo pf\_auth.cs](media/PAM-Azure-MFA-Activation-Image-2.png)

6. Con el Bloc de notas, abra **MfaSettings.xml**, que se encuentra en ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service```.

7. Copie los valores de los parámetros LICENSE\_KEY, GROUP\_KEY, CERT\_PASSWORD del archivo pf\_auth.cs en los elementos xml correspondientes del archivo MfaSettings.xml.

8. En el elemento XML **<CertFilePath>** , especifique el nombre completo de la ruta de acceso del archivo cert\_key.p12 extraído anteriormente.

9. En el elemento **<username>** , escriba cualquier nombre de usuario.

10. En el elemento **<DefaultCountryCode>** , escriba el código de país para llamar a los usuarios, por ejemplo, 1 para los Estados Unidos y Canadá. Este valor se utiliza en caso de que los usuarios se registren con números de teléfono que no tengan código de país. Si el número de teléfono de un usuario tiene un código de país internacional distinto del configurado para la organización, ese código de país debe incluirse en el número de teléfono que se va a registrar.

11. Guarde y sobrescriba el archivo **MfaSettings.xml** en la carpeta del servicio MIM ```C:\Program Files\Microsoft Forefront Identity Manager\2010\\Service```.

> [!NOTE]
> Al final del proceso, asegúrese de que el archivo **MfaSettings.xml**, o cualquier copia de este o del archivo ZIP, no se pueda leer públicamente.

## <a name="configure-pam-users-for-azure-mfa"></a>Configuración de usuarios de PAM para Azure MFA

Para que un usuario active un rol que requiere Azure MFA, el número de teléfono del usuario debe estar almacenado en MIM. Hay dos formas de establecer este atributo.

Con la primera, el comando `New-PAMUser` copia un atributo de número de teléfono de la entrada de directorio del usuario del dominio CORP en la base de datos del servicio MIM. Tenga en cuenta que se trata de una operación única.

Con la segunda, el comando `Set-PAMUser` actualiza el atributo de número de teléfono en la base de datos del servicio MIM. Por ejemplo,el código siguiente reemplaza un número de teléfono existente de un usuario de PAM en el servicio MIM. La entrada de directorio no se modifica.

```PowerShell
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```

## <a name="configure-pam-roles-for-azure-mfa"></a>Configuración de roles de PAM para Azure MFA

Una vez almacenados los números de teléfono de todos los usuarios candidatos para un rol de PAM en la base de datos del servicio MIM, el rol se puede habilitar de modo que exija Azure MFA. Esto se hace mediante los comandos `New-PAMRole` o `Set-PAMRole`. Por ejemplo,

```PowerShell
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

Azure MFA puede deshabilitarse para un rol si se especifica el parámetro "-MFAEnabled 0" en el comando `Set-PAMRole`.

## <a name="troubleshooting"></a>Solucionar problemas

Los siguientes eventos pueden encontrarse en el registro de eventos de Privileged Access Management:

| ID  | Gravedad | Generado por | Descripción |
|-----|----------|--------------|-------------|
| 101 | Error       | Servicio MIM            | El usuario no completó Azure MFA (por ejemplo, no respondió al teléfono). |
| 103 | Información de | Servicio MIM            | El usuario completó Azure MFA durante la activación.                       |
| 825 | Advertencia     | Servicio de supervisión de PAM | El número de teléfono ha cambiado.                                |

Para obtener más información sobre las llamadas telefónicas con errores (evento 101), también puede ver o descargar un informe de Azure MFA.

1.  Abra un explorador web y conéctese al [portal clásico de Azure](https://manage.windowsazure.com) como administrador global de Azure AD.

2.  Seleccione **Active Directory** en el menú del portal de Azure y luego la pestaña **Proveedores de autenticación multifactor**.

3.  Seleccione el proveedor de Azure MFA que usa para PAM y luego haga clic en **Administrar**.

4.  En la nueva ventana, haga clic en **Uso**y, luego, en **Detalles de usuario**.

5.  Seleccione el intervalo de tiempo y active la casilla situada junto a **Nombre** en la columna del informe adicional. Haga clic en **Exportar a CSV**.

6.  Una vez generado el informe, podrá verlo en el portal. En caso de que sea extenso, descárguelo en un archivo CSV. Los valores **SDK** de la columna **TIPO DE AUTENT.** indican las filas que son relevantes como solicitudes de activación de PAM: se trata de eventos procedentes de MIM o de otro software local. El campo **NOMBRE DE USUARIO** es el GUID del objeto de usuario en la base de datos del servicio MIM. Si una llamada no se ha realizado correctamente, el valor de la columna **AUTHD** será **No** y el de la columna **RESULTADO DE LLAMADA** contendrá los detalles del motivo del error.

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es Azure Multi-Factor Authentication?](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Cree hoy mismo su cuenta de Azure gratis](https://azure.microsoft.com/free/)
