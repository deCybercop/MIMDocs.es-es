---
title: Trabajar con el Autoservicio de restablecimiento de contraseña | Microsoft Docs
description: Consulte las novedades del autoservicio de restablecimiento de contraseña en MIM 2016, incluido el funcionamiento de SSPR con la autenticación multifactor.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/11/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 90452391170114270765e9a7fe08e98eea0747e4
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690700"
---
# <a name="self-service-password-reset-deployment-options"></a>Opciones de implementación del Autoservicio de restablecimiento de contraseña

Para los clientes nuevos que tienen [licencia de Azure Active Directory Premium](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-licensing), se recomienda usar el [Autoservicio de restablecimiento de contraseña de Azure AD](/azure/active-directory/authentication/concept-sspr-howitworks) para proporcionar la experiencia del usuario final.  El Autoservicio de restablecimiento de contraseña de Azure AD proporciona tanto una experiencia web como una experiencia integrada en Windows para que un usuario restablezca su propia contraseña, y admite muchas de las funciones de MIM, incluido el correo electrónico alternativo y las puertas de preguntas y respuestas.  Al implementar el Autoservicio de restablecimiento de contraseña de Azure AD, Azure AD Connect admite la [escritura no simultánea de las nuevas contraseñas en AD DS](/azure/active-directory/authentication/concept-sspr-writeback) y puede usarse el [Servicio de notificación de cambio de contraseña](deploying-mim-password-change-notification-service-on-domain-controller.md) de MIM para reenviar las contraseñas a otros sistemas, como el servidor de directorios de otro proveedor.  Para implementar MIM para [la administración de contraseñas](infrastructure/mim2016-password-management.md) no es necesario implementar el servicio MIM ni los portales de registro o de autoservicio de restablecimiento de contraseña.  En su lugar, puede seguir estos pasos:

- Primero, si tiene que enviar contraseñas a directorios distintos de Azure AD y AD DS, implemente la sincronización de MIM con conectores para servicios de dominio de Active Directory y cualquier sistema de destino adicional, configure MIM para la [administración de contraseñas](infrastructure/mim2016-password-management.md) e implemente el [servicio de notificación de cambio de contraseña](deploying-mim-password-change-notification-service-on-domain-controller.md).
- Después, si tiene que enviar contraseñas a directorios distintos de Azure AD, configure Azure AD Connect para [escribir de manera no simultánea las nuevas contraseñas en AD DS](/azure/active-directory/authentication/concept-sspr-writeback).
- Si lo desea, [registre previamente los usuarios](/azure/active-directory/authentication/howto-sspr-authenticationdata).
- Por último, [despliegue el Autoservicio de restablecimiento de contraseña de Azure AD en los usuarios finales](/azure/active-directory/authentication/howto-sspr-deployment).

Para los clientes existentes que ya habían implementado Forefront Identity Manager (FIM) para el Autoservicio de restablecimiento de contraseña y tienen licencia para Azure Active Directory Premium, se recomienda planear la transición al Autoservicio de restablecimiento de contraseña de Azure AD.  Puede realizar la transición de los usuarios finales al Autoservicio de restablecimiento de contraseña de Azure AD sin necesidad de registrarlos previamente, tan solo [sincronizando o configurando a través de PowerShell una dirección de correo electrónico alternativa o un número de teléfono móvil](/azure/active-directory/authentication/howto-sspr-authenticationdata). Una vez que los usuarios están registrados en el Autoservicio de restablecimiento de contraseña de Azure AD, puede retirar el portal de restablecimiento de contraseña de FIM.

Para los clientes que aún no han implementado el Autoservicio de restablecimiento de contraseña de Azure AD para sus usuarios, MIM también proporciona portales de autoservicio de restablecimiento de contraseña.  Comparado con FIM, MIM 2016 incluye los siguientes cambios:

- El portal de Autoservicio de restablecimiento de contraseña de MIM y la pantalla de inicio de sesión de Windows permiten a los usuarios desbloquear sus cuentas sin cambiar sus contraseñas.
- Se ha agregado una nueva puerta de autenticación a MIM, la puerta de teléfono. Esto permite que los usuarios se autentiquen a través de una llamada de teléfono en el Servicio Microsoft Azure Multi-Factor Authentication (MFA).

Las compilaciones de versiones de MIM 2016 hasta la versión 4.5.26.0 dependían de que el cliente descargara el Kit de desarrollo de software de Microsoft Azure Multi-factor Authentication (SDK de Azure MFA).  Este SDK está en desuso y el SDK de Azure MFA se admitirá para los clientes existentes solo hasta la fecha de retirada, el 14 de noviembre de 2018. Hasta entonces, los clientes deben ponerse en contacto con el soporte al cliente de Azure para recibir su paquete de credenciales del servicio MFA generado, ya que no podrán descargar el SDK de Azure MFA. 

#### <a name="new-update-current-azure-mfa-configuration-to-azure-multi-factor-authentication-server"></a>¡NUEVO! Actualizar la configuración actual de Azure MFA al Servidor Azure Multi-Factor Authentication

En este [artículo](working-with-mfaserver-for-mim.md) se describe cómo actualizar la implementación del portal de autoservicio de restablecimiento de contraseña de MIM y la configuración de PAM mediante el Servidor Microsoft Azure Multi-Factor Authentication para la autenticación multifactor.

## <a name="deploying-mim-self-service-password-reset-portal-using-azure-mfa-for-multi-factor-authentication"></a>Implementar el portal de autoservicio de restablecimiento de contraseña de MIM mediante Azure MFA para la autenticación multifactor

En la siguiente sección se describe cómo implementar el portal de autoservicio de restablecimiento de contraseña de MIM mediante Azure MFA para la autenticación multifactor.  Estos pasos solo son necesarios para los clientes que no usen el autoservicio de restablecimiento de contraseña de Azure AD para sus usuarios.

Microsoft Azure Multi-Factor Authentication es un servicio de autenticación que requiere que los usuarios se identifiquen en sus intentos de inicio de sesión con una aplicación móvil, una llamada de teléfono o un mensaje de texto. Está disponible para su uso con Microsoft Azure Active Directory y como un servicio para las aplicaciones empresariales en la nube y locales.

Azure MFA proporciona un mecanismo de autenticación adicional que puede reforzar los procesos de autenticación existentes; por ejemplo, con el proceso que realiza MIM para la asistencia de inicio de sesión de autoservicio.

Cuando se utiliza Azure MFA, los usuarios se autentican con el sistema para comprobar su identidad al intentar recuperar el acceso a sus cuentas y recursos. La autenticación se puede realizar a través de SMS o de una llamada de teléfono.   Cuanto más sólida es la autenticación, mayor será la certeza de que la persona que intenta obtener acceso es realmente quien posee esa identidad. Una vez autenticado, el usuario puede elegir una contraseña nueva para reemplazar la antigua.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>Requisitos previos para configurar el desbloqueo de la cuenta de autoservicio y el restablecimiento de contraseña mediante MFA

En esta sección se supone que ha descargado y completado la implementación de los [componentes Sincronización de MIM, Servicio MIM y Portal de MIM](microsoft-identity-manager-deploy.md) de Microsoft Identity Manager 2016, incluidos los siguientes componentes y servicios:

-   Un servidor Windows Server 2008 R2 o posterior configurado como servidor de Active Directory, incluidos los servicios de dominio y el controlador de dominio de AD con un dominio designado (un dominio "corporativo").

-   Una directiva de grupo definida para el bloqueo de cuenta.

-   Servicio de sincronización de MIM 2016 instalado y en ejecución en un servidor que esté unido al dominio de AD.

-   El servicio &amp; y portal de MIM 2016, incluido el portal de autoservicio de registro y el de autoservicio de restablecimiento de contraseña, instalados y en ejecución en un servidor (podría estar ubicado en el mismo servidor que el servicio de sincronización)

-   El servicio de sincronización de MIM debe estar configurado para la sincronización de identidades de AD-MIM. Esto incluye lo siguiente:

    -   Configurar el agente de administración de Active Directory (ADMA) para que tenga conexión con el servidor de dominio de AD y capacidad para importar y exportar datos de identidad de Active Directory.

    -   Configurar el agente de administración de MIM (MIM MA) para que tenga conexión con la base de datos del servicio FIM y capacidad para importar y exportar datos de identidad de la base de datos de FIM.

    -   Configurar reglas de sincronización en el portal de MIM que permitan la sincronización de datos de usuario y faciliten las actividades de sincronización del servicio MIM.

-   Los complementos y extensiones de MIM 2016 &amp;, incluido el cliente integrado de inicio de sesión de Windows para autoservicio de restablecimiento de contraseña, implementados en el servidor o en un equipo cliente distinto.

Para este escenario es necesario tener las CAL de MIM para los usuarios, así como la suscripción para Azure MFA.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>Preparación de MIM para que funcione con la autenticación multifactor
Configure MIM Sync para que admita la funcionalidad de restablecimiento de contraseña y desbloqueo de cuenta. Para obtener más información, consulte los artículos [Installing the FIM Add-ins nd Extensions](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx) (Instalación de complementos y extensiones de FIM), [Installing FIM SSPR](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx) (Instalación de FIM SSPR), [SSPR Authentication Gates](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) (Puertas de autenticación de SSPR) y [la guía del laboratorio de pruebas de SSPR](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx).

En la siguiente sección, configurará el proveedor de Azure MFA en Microsoft Azure Active Directory. Como parte de esto, se generará un archivo que incluye el material de autenticación que requiere MFA para poder establecer contacto con Azure MFA.  Para poder continuar, necesitará una suscripción de Azure.

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>Registro del proveedor de autenticación multifactor en Azure

1.  Cree un [proveedor de MFA](/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider).

2. Abra un caso de soporte técnico y solicite el SDK directo para ASP.net 2.0 C#. El SDK solo se proporcionará a los usuarios actuales de MIM con MFA porque el SDK directo ha quedado en desuso. Los nuevos clientes deben adoptar la próxima versión de MIM que se integre con el servidor MFA.

3. Copie el archivo ZIP resultante en cada sistema donde esté instalado el servicio de MIM.  Tenga en cuenta que el archivo ZIP contiene el material de claves que se usa para autenticar el servicio de Azure MFA.

### <a name="update-the-configuration-file"></a>Actualizar el archivo de configuración

1. Inicie la sesión del usuario que instaló MIM en el equipo en el que está instalado el servicio MIM.

2. Cree una nueva carpeta de directorio ubicada en el directorio donde se instaló el servicio MIM, como **C:\Archivos de programa\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**.

3. En el Explorador de Windows, navegue a la carpeta **pf\\certs** del archivo ZIP que descargó en la sección anterior y copie el archivo **cert\_key.p12** en el nuevo directorio.

4.  En el archivo zip del SDK, abra el archivo **pf_auth.cs** de la carpeta **\pf**.

5.  Busque estos tres parámetros: `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`.

    ![Imagen del código pf_auth.cs](media/MIM-SSPR-pFile.png)

6.  En **C:\Archivos de programa\Microsoft Forefront Identity Manager\2010\Service**, abra este archivo: **MfaSettings**.xml.

7.  Copie los valores de los parámetros `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` del archivo pf_aut.cs en los elementos xml correspondientes del archivo MfaSettings.xml.

8.  En el archivo zip del SDK, bajo \pf\certs, extraiga el archivo **cert_key.p12** y escriba la ruta de acceso completa en el archivo MfaSettings.xml del elemento xml `<CertFilePath>` .

9. En el elemento `<username>` , escriba cualquier nombre de usuario.

10. En el elemento `<DefaultCountryCode>` escriba su código de país predeterminado. En el caso de que se registren números de teléfono de usuarios sin código de país, este es el código de país que se aplicará. En el caso de que el usuario tenga un código de país internacional, este se debe incluir en el número de teléfono registrado.

11. Guarde el archivo MfaSettings.xml con el mismo nombre y en la misma ubicación.

#### <a name="configure-the-phone-gate-or-the-one-time-password-sms-gate"></a>Configurar la puerta de teléfono o la puerta de SMS de contraseña de un solo uso

1.  Inicie Internet Explorer y desplácese hasta el Portal de MIM, identifíquese como administrador de MIM y haga clic en  **Flujos de trabajo** en la barra de navegación izquierda.

    ![Imagen de navegación del portal de MIM](media/MIM-SSPR-workflow.jpg)

2.  Seleccione **Flujo de trabajo de autenticación de restablecimiento de contraseña**.

    ![Imagen de los flujos de trabajo del portal de MIM](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Haga clic en la pestaña **Actividades** y desplácese hacia abajo hasta **Agregar actividad**.

4.  Seleccione **Puerta de teléfono** o **Puerta de SMS para contraseña de un solo uso**, haga clic en **Seleccionar** y, a continuación, en **Aceptar**.

Nota: Si usa el servidor Azure MFA u otro proveedor que genere la contraseña de un solo uso, asegúrese de que el campo de longitud configurado anteriormente tiene la misma longitud que el generado por el proveedor de MFA.  Esta longitud debe ser de seis en el caso del servidor Azure MFA.  El servidor Azure MFA también genera su propio texto de mensaje, por lo que el mensaje de texto SMS se omite.

Ahora los usuarios de su organización podrán registrase para restablecer su contraseña.  Durante este proceso, deberán especificar su número de teléfono del trabajo o el número de teléfono móvil para que el sistema sepa cómo llamarlos (o enviarles mensajes SMS).

#### <a name="register-users-for-password-reset"></a>Registrar usuarios para restablecer la contraseña

1.  El usuario debería iniciar un navegador web y desplazarse hasta el Portal de registro de restablecimiento de contraseñas de MIM.  (Normalmente este portal se configurará con la autenticación de Windows).  En el portal, deberán proporcionar su nombre de usuario y la contraseña de nuevo para confirmar su identidad.

    Deben visitar el portal de registro de contraseñas y autenticarse con su nombre de usuario y contraseña.

2.  En el campo **Número de teléfono** o **Teléfono móvil**  , deben escribir un código de país, un espacio y el número de teléfono, y hacer clic en **Siguiente**.

    ![Imagen de la comprobación por teléfono de MIM](media/MIM-SSPR-PhoneVerification.JPG)

    ![Imagen de la comprobación por teléfono móvil de MIM](media/MIM-SSPR-mobilephoneverification.JPG)

## <a name="how-does-it-work-for-your-users"></a>¿Cómo funciona para los usuarios?
Ahora que está todo configurado y funcionando, querrá saber lo que tienen que hacer los usuarios si restablecen la contraseña justo antes de las vacaciones y descubren a su vuelta que han olvidado la contraseña por completo.

Los usuarios pueden usar la funcionalidad de restablecimiento de contraseña y desbloqueo de cuenta de dos maneras: desde la pantalla de inicio de sesión de Windows o desde el portal de autoservicio.

Al instalar las extensiones y complementos de MIM en un equipo unido a un dominio conectado a través de la red de la organización al servicio MIM, los usuarios podrán recuperar una contraseña olvidada en la experiencia de inicio de sesión de escritorio.  Los siguientes pasos le guiarán a través del proceso.

#### <a name="windows-desktop-login-integrated-password-reset"></a>Restablecer la contraseña integrada de inicio de sesión de escritorio de Windows

1.  Si el usuario escribe una contraseña incorrecta varias veces en la pantalla de inicio de sesión, tendrá la opción de hacer clic en **¿Tiene problemas para iniciar sesión?** . .

    ![Imagen de la pantalla de inicio de sesión](media/MIM-SSPR-problemsloggingin.JPG)

    Este vínculo lleva a la pantalla de restablecimiento de contraseña de MIM, donde puede cambiar su contraseña o desbloquear su cuenta.

    ![Imagen de restablecimiento de contraseña de MIM](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  El usuario será dirigido a la autenticación. Si se ha configurado MFA, el usuario recibirá una llamada telefónica.

3.  Lo que sucede en segundo plano es que Azure MFA hace llamada de teléfono al número que proporcionó el usuario cuando se registró en el servicio.

4.  Cuando el usuario responde al teléfono, se le pide que pulse la tecla almohadilla # en el teléfono. A continuación, el usuario hace clic en **Siguiente** en el portal.

    Si configura también otras puertas, se pedirá al usuario que proporcione más información en las pantallas siguientes.

    > [!NOTE]
    > Si el usuario se impacienta y hace clic en **Siguiente** antes de presionar la tecla almohadilla (#), se produce un error de autenticación.

5.  Cuando la autenticación es correcta, el usuario tiene dos opciones: desbloquear su cuenta y mantener la contraseña actual o establecer una contraseña nueva.

6.  Después, el usuario debe escribir la contraseña nueva dos veces y se restablece la contraseña.

#### <a name="access-from-the-self-service-portal"></a>Acceso desde el portal de autoservicio

1.  Los usuarios pueden abrir un explorador web, acceder al **Portal de restablecimiento de contraseña** , escribir su nombre de usuario y hacer clic en **Siguiente**.

    Si se ha configurado MFA, el usuario recibirá una llamada telefónica. Lo que sucede en segundo plano es que Azure MFA hace llamada de teléfono al número que proporcionó el usuario cuando se registró en el servicio.

    Cuando el usuario responde al teléfono, se le pide que presione la tecla almohadilla # en el teléfono. A continuación, el usuario hace clic en **Siguiente** en el portal.

2.  Si configura también otras puertas, se pedirá al usuario que proporcione más información en las pantallas siguientes.

    > [!NOTE]
    > Si el usuario se impacienta y hace clic en **Siguiente** antes de presionar la tecla almohadilla (#), se produce un error de autenticación.

3.  El usuario tendrá que elegir si desea restablecer su contraseña o desbloquear su cuenta. Si elige desbloquear la cuenta, su cuenta se desbloqueará.

    ![Imagen de desbloqueo de cuenta del Asistente para inicio de sesión de MIM](media/MIM-SSPR-accountUnlock.JPG)

4.  Cuando la autenticación es correcta, el usuario tiene dos opciones: mantener la contraseña actual o establecer una contraseña nueva.

5.  ![imagen de cuenta de MIM
6.  desbloqueada correctamente](media/MIM-SSPR-account-unlock.JPG)

6.  Si el usuario decide restablecer su contraseña, tendrá que escribir una contraseña nueva dos veces y hacer clic en **Siguiente** para cambiar la contraseña.
