---
title: Personalizaciones del portal de Microsoft Identity Manager 2016 | Microsoft Docs
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: bebb299ece077a6ab2e0d75f3b30ad1776678303
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2017
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Personalización del portal de Microsoft Identity Manager 2016


>[!Warning]
Asegúrese de borrar la caché del explorador cuando se realicen personalizaciones de CSS.

En Microsoft Identity Manager 2016 (MIM), puede personalizar los elementos seleccionados de los portales de contraseña, incluidos el logotipo del banner, los recursos de cadena y las hojas de estilos en cascada.

Para ello, se requieren algunas cosas según el nivel de personalización. La siguiente es una lista de elementos que intervienen en la personalización de los portales de registro y restablecimiento de contraseñas de MIM 2016.

-   Carpeta de personalizaciones: esta es la carpeta que comprueba MIM 2016 antes de usar los valores predeterminados. Cada portal que se va a personalizar requiere una carpeta de personalización. Las personalizaciones solo se deben realizar en esta carpeta porque el programa de instalación no las sobrescribe en las actualizaciones, en las instalaciones en modo de cambio o en las instalaciones en modo de reparación.

-   Strings.resources: este es un archivo basado en XML que le permite modificar las cadenas que aparecen en el portal. Este archivo debe residir en la carpeta Personalizaciones.

-   Style.css: esta es la hoja de estilo en cascada que se usa en los portales para la personalización. Esta hoja de estilos se debe crear y modificar para cambiar el logotipo o se puede reemplazar por completo por sus propias personalizaciones.

Para obtener instrucciones paso a paso sobre la personalización de los portales de restablecimiento y registro de contraseñas, consulte Test Lab Guide: Demonstrating MIM 2016 Password Registration and Reset Portal Customization (Guía de Test Lab: demostración de la personalización del portal de registro y restablecimiento de contraseñas de MIM 2016).

>[ADVERTENCIA] Para que MIM reconozca los cambios personalizados, debe reiniciar IIS mediante la ejecución de iisreset.


## <a name="creating-the-customizations-folder"></a>Creación de la carpeta Personalizaciones

Al inicio, MIM busca el archivo Strings.resources en la carpeta Personalizaciones antes de usar los valores predeterminados. Debe crear una carpeta Personalizaciones en el directorio del portal que desea personalizar (es decir, el Portal de registro de contraseñas o el Portal de restablecimiento de contraseñas). Si quiere personalizar ambos portales, deberá crear una carpeta Personalizaciones en cada uno de los siguientes directorios:

-   C:\\Archivos de programa\\Microsoft Forefront Identity Manager\\2010\\Portal de registro de contraseñas

-   C:\\Archivos de programa\\Microsoft Forefront Identity Manager\\2010\\Portal de restablecimiento de contraseñas

### <a name="to-create-the-customization-folder"></a>Para crear la carpeta de personalización, siga estos pasos:

1.  Vaya a la carpeta C:\\Archivos de programa\\Microsoft Forefront Identity Manager\\2010\\Portal de registro de contraseñas.

2.  Cree una carpeta denominada Personalizaciones.

3.  Retroceda un nivel hasta la carpeta Portal de restablecimiento de contraseñas y cree una carpeta llamada Personalizaciones.

## <a name="customizing-strings"></a>Personalización de cadenas

Muchas de las cadenas de la interfaz de usuario del portal se pueden personalizar mediante la creación de un archivo Strings.resources y la adición de este archivo a la carpeta Personalizaciones. Deberá crear un archivo Strings.resources para cada portal que quiera personalizar.

### <a name="to-customize-strings"></a>Para personalizar las cadenas, siga estos pasos:

1.  Mediante el Bloc de notas, copie el código siguiente en él y guárdelo en la carpeta Personalizaciones como Strings.resources

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <root>
     <resheader name="resmimetype">
       <value>text/microsoft-resx</value>
     </resheader>
     <resheader name="version">
       <value>2.0</value>
     </resheader>
     <resheader name="reader">
       <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
    </resheader>
     <resheader name="writer">
       <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
     </resheader>

    <!-- Customizations begin here -->
     <data name=" QAGateResetTitle " xml:space="preserve">
       <value>Contoso Question and Answer Reset</value>
     </data>
     <data name="ResetPageTitle" xml:space="preserve">
       <value>Contoso Self-Service Password Reset</value>
     </data>
   </root>

   ```
2.  En la sección `<!-- Customizations begin here -->`, cambie el nombre de los datos para que coincida con las cadenas que quiere personalizar y escriba el valor para esa cadena entre las etiquetas <value></value>. Consulte en la lista siguiente las cadenas que se pueden personalizar y sus valores predeterminados.

>[!NOTE]
El archivo **Strings.resources** es independiente del idioma. Para crear cadenas personalizadas específicas del idioma, debe tener instalado ese paquete de idioma y guardar el archivo en el formato de cadenas. `<language>-<culture>.resources`, por ejemplo, Strings.en-us.resources.

La siguiente es una lista de cadenas de portal que se pueden personalizar.

<a name="portal-strings"></a>Cadenas de portal
--------------

| Nombre                                                     | Valor predeterminado                                                                                                                                                                                                                                                                                                                                         | Comentario                                                                                                                                                                                                                                            |
|---------------------------|-------------------|--------------|
| AboutLinkText                                            | Acerca de         |       |
| ButtonCancel                                             | Cancelar                                                                                 |     |
| ButtonFinish                                             | Finalizar    |    |
| ButtonNext                                               | Siguiente    |    |
| ButtonOk                                                 | Aceptar     |   |
| CancelFinishedMessage                                    | La sesión ya no está activa. Puede cerrar la ventana o hacer clic en el vínculo siguiente para reiniciar.         |      |
| CancelFinishedTitle                                      | Sesión finalizada                                                              |      |
| ErrorDescription_3000                                    | Error. Inténtelo de nuevo y, si el problema persiste, póngase en contacto con el departamento de soporte técnico o el administrador del sistema. (Error 3000)       |          |
| ErrorDescription_3001                                    | Asegúrese de que haber escrito correctamente el nombre de usuario. Si sigue sin poder restablecer la contraseña, póngase en contacto el      |           |
|                                                          | departamento de soporte técnico para solicitar ayuda. (Error 3001)   |                   |
| ErrorDescription_3002                                    | La sesión finalizó. Regrese a la página principal para volver a empezar. (Error 3002)                                     |                |
| ErrorDescription_3003                                    | Forefront Identity Manager no reconoce la cuenta de usuario actual. Póngase en contacto con el departamento de soporte técnico o el administrador del sistema. (Error 3003)           |               |
| ErrorDescription_3004                                    | No está autorizado a registrarse para el restablecimiento de contraseña. Póngase en contacto con el departamento de soporte técnico o el administrador del sistema. (Error 3004)     |              |
| ErrorDescription_3005                                    | Una o varias de las respuestas proporcionadas no coinciden con las respuestas especificadas durante el proceso de registro de la contraseña. Para poder restablecer la contraseña, las respuestas que proporcione ahora deben ser las mismas que las proporcionadas durante el registro. Puede empezar de nuevo desde la página principal, o bien ponerse en contacto con el departamento de soporte técnico o el administrador del sistema. (Error 3005) |           |
| ErrorDescription_3006                                    | La contraseña que ha escrito no es correcta. Para poder registrarse al restablecimiento de contraseña, debe escribir la contraseña correcta. (Error 3006)            |              |
| ErrorDescription_3007                                    | Se le ha prohibido temporalmente restablecer la contraseña. Inténtelo de nuevo más tarde o póngase en contacto con el departamento de soporte técnico para solicitar ayuda. (Error 3007)     |         |
| ErrorDescription_3008                                    | Error. Inténtelo de nuevo y, si el problema persiste, póngase en contacto con el departamento de soporte técnico o el administrador del sistema. (Error 3008)        |          |
| ErrorDescription_3009                                    | La entrada contiene texto en un formato no permitido. Inténtelo de nuevo con otra entrada o póngase en contacto con el soporte técnico o el administrador del sistema. (Error 3009)     |             |
| ErrorDescription_3010_Registration                       | No están habilitados los scripts en el explorador. Habilite los scripts y vuelva a la página de inicio Restablecimiento de contraseña o póngase en contacto con el soporte técnico para obtener ayuda.      |               |
| ErrorDescription_3010_Reset                              | No están habilitados los scripts en el explorador. Habilite los scripts y vuelva a la página de inicio Registro de la contraseña o póngase en contacto con el soporte técnico para obtener ayuda.     |            |
| ErrorDescription_3011                                    | Este sitio usa cookies. Configure el explorador para que acepte cookies e inténtelo de nuevo o póngase en contacto con el soporte técnico para obtener ayuda.          |                                           |
| ErrorDescription_3012                                    | Los datos especificados no concuerdan con el código de seguridad que se le envió. Puede volver a intentar restablecer la contraseña o bien puede ponerse en contacto con el departamento de soporte técnico para obtener ayuda.      |                                                                                                                                                                                                                                                    |
| ErrorDescription_3013                                    | No se puede enviar un código de seguridad. Póngase en contacto con el departamento de soporte técnico para obtener ayuda.                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameFormat                         | Escriba su nombre de usuario en el formato correcto.                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameRequired                       | Escriba un nombre de usuario para continuar.                                              |                         |
| ErrorMessagePasswordRequired                             | Escriba una contraseña.              |                                                |
| ErrorMessagePasswordsDoNotMatch                          | Asegúrese de que las dos contraseñas coincidan.                                |           |
| ErrorPageDefaultHeading                                  | Error de la aplicación                                            |               |
| ErrorPageServerTime                                      | Hora del servidor: {0:T}                     | {0} es la hora a la que se detectó la excepción. La "T" hace que el tiempo pasado se formatee como "hora larga". Acabará mostrando la hora, los minutos, los segundos y, posiblemente, la designación de a.m./p.m. (según la cultura actual). |
| ErrorPageTitle                                           | Forefront Identity Management: error de contraseña                     |   |
| ErrorTitle_3000                                          | Error                                  |  |
| ErrorTitle_3001                                          | acceso denegado                          |  |
| ErrorTitle_3002                                          | Sesión finalizada                          |  |
| ErrorTitle_3003                                          | Usuario no reconocido                      |  |
| ErrorTitle_3004                                          | Usuario no autorizado                      |  |
| ErrorTitle_3005                                          | Las respuestas no coinciden                    |  |
| ErrorTitle_3006                                          | Contraseña incorrecta                     |  |  
| ErrorTitle_3007                                          | Acceso temporalmente denegado              |  |
| ErrorTitle_3008                                          | Error de comunicación                    |  |
| ErrorTitle_3009                                          | Entrada prohibida                       |  |
| ErrorTitle_3010                                          | Error de configuración del explorador            |  |
| ErrorTitle_3011                                          | Error de configuración del explorador            |  |
| ErrorTitle_3012                                          | Error de verificación                    |  |
| ErrorTitle_3013                                          | No se puede enviar el código de seguridad   |  |
| FinalizeRegistrationHeading1                             | Para restablecer su contraseña:        |   |
| FinalizeRegistrationSubHeading1                          | Vaya al portal de restablecimiento de contraseñas   |   |
| FinalizeRegistrationSubHeading2                          | Compruebe su identidad   |   |
| FinalizeRegistrationSubHeading3                          | Elija la nueva contraseña    |     |
| FinishingDescription                                     | Elija la nueva contraseña        |    |
| FinishingTitle                                           | Restablecimiento de contraseña:      |     |
| GotoPortalPrefix                                         | Vaya a     |        |
| GotoPortalSuffix                                         | página principal     |      |
| LabelTroubleshootingLinkText                             | Ver detalles           |    |
| LoadingText                                              | Cargando...     |  |
| NoScriptTagErrorMessage                                  | No están habilitados los scripts en el explorador. Habilite los scripts y vuelva a la página de inicio o póngase en contacto con el soporte técnico para obtener ayuda.      |   |
| PasswordResetOperationGeneralErrorMessage                | Error al intentar restablecer la contraseña.       |   |
| PasswordResetOperationPolicyViolationErrorMessage            | La contraseña no cumple con las directivas de contraseñas de la organización.             |       |
| PasswordResetOperationUserCantChangePasswordErrorMessage | Error al restablecer la contraseña. El usuario no puede cambiar la contraseña.    |   |
| PrivacyStatement                                         | Declaración de privacidad                                                      |    |
| RegistrationDescription                                  | Autoservicio de registro de la contraseña     |    |
| RegistrationMission                                      | Si alguna vez olvida la contraseña, se puede restablecer manualmente sin llamar al departamento de soporte técnico.   |      |
| RegistrationPageTitle                                    | Forefront Identity Management: registro de contraseñas                                          |   |
| RegistrationSteps                                        | Haga clic en "Siguiente" para comenzar el proceso de registro.   |      |
| RegistrationSuccessDescription                           | Ya está registrado.        |   |
| RegistrationSuccessTitle                                 | Finalizado:   |    |
| RegistrationWelcomeTitle                                 | Registro de contraseña:    | |
| ResetDescription                                         | Autoservicio de restablecimiento de contraseña  |    |
| ResetEnterNamePrompt                                     | Escriba su nombre de usuario a continuación     |     |
| ResetEnterPassword                                       | Escriba una nueva contraseña:                                                  |   |
| ResetExample1                                            | contoso\\mmeyers                                                                            |      |
| ResetExample2                                            | mmeyers\@contoso.com     |      |
| ResetExamples                                            | Ejemplos:  |    |
| ResetPageTitle                                           | Forefront Identity Management: restablecimiento de contraseñas       |     |
| ResetReenterPassword                                     | Vuelva a escribir la contraseña:    | |
| ResetSuccessDescription                                  | Se ha restablecido su contraseña    |    |
| ResetSuccessTitle                                        | Correcto:                                |    |
| ResetUseNewPassword                                      | Ahora puede usar la nueva contraseña para iniciar sesión.     |      |
| ResetUsernameTextFormat                                  | (Restableciendo la contraseña para {0})       | {0} es el inicio de sesión del usuario             |
| ResetWelcomeTitle                                        | Restablecimiento de contraseña:     |      |
| TroubleshootingEmailSubject                              | Detalles de error de procesamiento de solicitudes de FIM     |       |
| TroubleshootingLabelAttributes                           | Atributos:    |    |
| TroubleshootingLabelCloseButton                          | Cerrar       |    |
| TroubleshootingLabelCopyToClipboard                      | Copiar al Portapapeles     |     |
| TroubleshootingLabelCorrelationId                        | Id. de correlación:      |          |
| TroubleshootingLabelDetails                              | Detalles:                                                             |    |
| TroubleshootingLabelPostCopyClipboardMessage             | La información se ha copiado en el Portapapeles.           |    |
| TroubleshootingLabelRequestId                            | Id. de solicitud:                  |    |
| TroubleshootingLabelSendEmail                            | Enviar información por correo electrónico                            |    |
| TroubleshootingLabelSource                               | Motivo:                                                                     |    |
| TroubleshootingLabelViewRequestDetails                   | Ver detalles de la solicitud        |              |                                                    
| TroubleshootingLinkText                                  | Información sobre solución de problemas          |                  | |



La siguiente es una lista de cadenas de la puerta de autenticación que se pueden personalizar.

<a name="authentication-gate-strings"></a>Cadenas de la puerta de autenticación
---------------------------

| Nombre                                          | Valor predeterminado                                                                                                                                                                                                                | Comentario |
|-----------|--------------|----------|
| OTPEmailRegistraionEmailTextboxLabel          | Dirección de correo electrónico:             |          |
| OTPEmailRegistrationEmailRequiredErrorMessage | El campo de dirección de correo electrónico no puede estar vacío.     |          |
| OTPEmailRegistrationFooterReadOnly            | Para actualizar la dirección de correo electrónico, siga el proceso que haya definido la organización o póngase en contacto con el soporte técnico.                   |          |
| OTPEmailRegistrationFooterReadWrite           | La dirección de correo electrónico la almacena la organización en Forefront Identity Manager.                       |          |
| OTPEmailRegistrationGateTitle                 | Verificación por dirección de correo electrónico   |          |
| OTPEmailRegistrationHeaderReadOnly            | Para restablecer su contraseña, se enviará un código de seguridad de verificación a su correo electrónico. Si la dirección de correo electrónico que se indica a continuación no es correcta, deberá actualizarla antes de poder usar el restablecimiento de contraseña de autoservicio. |          |
| OTPEmailRegistrationHeaderReadWrite           | Escriba su dirección de correo electrónico a continuación. Para restablecer su contraseña, se enviará un código de verificación a su correo electrónico.                 |          |
| OTPEmailResetGateTitle                        | Verificar la identidad: verificación por correo electrónico         |          |
| OTPEmailResetHeader                           | Escriba el código de seguridad a continuación. Se envió un código de seguridad a la dirección de correo electrónico registrada para esta organización.                                                                                                             |          |
| OTPRegularExpressionErrorMessage              | El valor especificado no coincide con el formato esperado.                   |          |
| OTPResetOneTimePasswordRequiredErrorMessage   | El campo de código de seguridad no puede estar vacío.        |          |
| OTPResetVerificationLabel                     | Código de seguridad:                    |          |
| OTPSmsRegistrationFooterReadOnly                      | Para actualizar el número de teléfono móvil, siga el proceso que haya definido la organización o póngase en contacto con el soporte técnico.   ||
| OTPSmsRegistrationFooterReadWrite                     | El número de teléfono móvil los almacena la organización en Forefront Identity Manager.                                                                                                                                                     |   |
| OTPSmsRegistrationGateTitle                           | Verificación por teléfono móvil                      |   |
| OTPSmsRegistrationHeaderReadOnly                      | Para restablecer su contraseña, se enviará un código de seguridad de verificación a su teléfono móvil. Si el número de teléfono móvil que se indica a continuación no es correcto, deberá actualizarlo antes de poder usar el restablecimiento de contraseña de autoservicio. |   |
| OTPSmsRegistrationHeaderReadWrite                     | Escriba su número de teléfono móvil a continuación. Para restablecer su contraseña, se enviará un código de verificación a su teléfono móvil.                                                                                                     |   |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | El campo de número de teléfono móvil no puede estar vacío.      |   |
| OTPSmsRegistrationSMSTextBoxLabel                     | Teléfono móvil:                    |   |
| OTPSmsResetGateTitle                                  | Verificar la identidad: verificación por teléfono móvil         |   |
| OTPSmsResetHeader                                     | Escriba el código de seguridad a continuación. Se envió un código de seguridad al teléfono móvil registrado para esta organización.                                                                                                                           |   |
| PasswordGateDescriptionText                           | Escriba a continuación la contraseña actual y haga clic en "Siguiente".        |   |
| PasswordGateErrorMessagePasswordRequired              | Escriba la contraseña actual.                  |   |
| PasswordGateGateTitle                                 | La contraseña actual               |   |
| PasswordGatePasswordLabelText                         | Contraseña:                 |   |
| PasswordGateUsernameTextFormat                        | `<i>`(ha iniciado sesión como:`<b>{0}</b>)</i>`                                                          |   |
| QAGateErrorNotEnoughQuestionsAnswered                 | Como mínimo debe responder a {0} preguntas para registrarse.        |   |
| QAGateIncorrectAnswer                                 | Las respuestas no son correctas.       |   |
| QAGatePrivacyNotice                                   | Su organización almacena las respuestas facilitadas en Forefront Identity Manager.                                                                                                                                                  |   |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | Como mínimo debe responder a {0} preguntas para registrarse.     |   |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | Una o más respuestas no cumplen con las directivas.      |   |
| QAGateRegistrationThisAnswerValidationFailed          | Esta respuesta no cumple con la directiva.      |   |
| QAGateRegistrationTitle                               | Registrar las respuestas         |   |
| QAGateResetNumberOfQuestionsExplanation_Format        | Debe contestar a {0} de las siguientes preguntas de {1}.   |   |
| QAGateResetTitle                                      | Compruebe su identidad: enviar respuestas                                  |   |



## <a name="customizing-the-logo-banner"></a>Personalización del banner del logotipo

Se puede personalizar el banner predeterminado de las páginas del portal según los requisitos de su organización.
Para personalizar el banner del logotipo:

1.  Cree los banners personalizados y guárdelos como archivos .png. Los archivos deben cumplir las siguientes recomendaciones:

 - Tamaño: 490 x 50 píxeles.

 - Profundidad de bits: 32

2.  Copie los archivos en la \\carpeta Personalizaciones en cada portal que quiera personalizar.

3.  Cree un archivo Style.css en cada carpeta. Haga que apunte a la carpeta Personalizaciones y al nuevo logotipo. Puede cambiar el nombre de logotipo si corresponde (es decir, /Customizations/contosologo.png). El código debe ser similar al siguiente:

   **Ejemplo 1:**

  `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4.  Si usa Internet Explorer 6.0, debe proporcionar un logotipo alternativo no transparente y agregar el código siguiente al Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

  **Ejemplo 2:**

  `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`

Si usa Internet Explorer 6.0, debe proporcionar un logotipo alternativo no transparente y agregar el código siguiente al Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/contosologo-ie6.png);}`

## <a name="customizing-image-for-smartphones"></a>Personalización de la imagen para smartphones

Puede personalizar la imagen para smartphones utilizando lo siguiente. Para personalizar la imagen de un smartphone:

1.  Cree la imagen y guárdela como archivos .png. Los archivos deben cumplir las siguientes recomendaciones:

    - Tamaño: 190 x 50 píxeles.
    - Profundidad de bits: 32

2.  Copie los archivos en la \\carpeta Personalizaciones en cada portal que quiera personalizar.

2.  Cree un archivo Style.css en cada carpeta. Haga que apunte a la carpeta Personalizaciones y al nuevo logotipo. Puede cambiar el nombre de logotipo si corresponde (es decir, /Customizations/contosologo.png). El código debe ser similar al siguiente:

  **Ejemplo 1:**

```css
@media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="customizing-style-sheets"></a>Personalización de las hojas de estilos

Puede modificar el diseño y el estilo de los portales de contraseña mediante el uso de hojas de estilos en cascada personalizadas (CSS). Para usar una CSS personalizada, siga estos pasos:

1.  Cree los archivos CSS personalizados y guárdelos como Style.css.

2.  Copie los archivos en la \\carpeta Personalizaciones en cada portal que quiera personalizar.

A continuación se muestra un ejemplo básico de un archivo Style.css:

```css
body
{
  font: 15px Algerian;
  color: \#303030;
  background: white;
}

.pad
{
  padding: 30px;
  padding-top: 50px;
  background: white;
}

.backgroundWhite
{
  border: \#e9e9e9 2px solid;
} .
title-block
{
background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;
}
```


>[!IMPORTANT]
Para que MIM reconozca los cambios personalizados, debe reiniciar IIS mediante la ejecución de iisreset.                                                                                                                                                                                       

A continuación se muestra un ejemplo más avanzado de un archivo Style.css: Este archivo proporciona información específica de smartphone e iPad para mostrar los portales en estos dispositivos.

```css
/****************
BASE
*****************/

body {
    font-size: 14px; /*Customizeable- Body Font Size */
    background-color: #ced5ec; /*Customizeable- Backgound Color behind the product */
}

body, button, input, select, textarea {
    font-family: Segoe UI, Arial, Verdana, Sans-Serif, Helvetica; /*Customizeable- Body Font Family */
    color: #595959; /*Customizeable- Body Font Color */
}

/****************
LINKS
*****************/

a { color: #396faf; text-decoration: none; } /*Customizeable- Link Color and Underline */
a:visited { color: #396faf; text-decoration: none; } /*Customizeable- After Link is clicked color and underline */
a:hover { color: #6486ae; text-decoration: none; } /*Customizeable- Hover mouse over Link color and underline */
a:focus { outline: thin dotted; } /*Customizeable- Keyboard event to Link and Link is in focus outline*/
a:hover, a:active { outline: 0; } /*Customizeable- Hover and Active Link outline */

/****************
Typography
*****************/

hr { border-top: 1px solid #acd9ec; } /*Customizeable- Horizontal Rule Color Above the Footer */

/****************
Layout
*****************/
#wrapper {
    background: url(../images/bg-top-slice.png) repeat-x 0 0; /*Customizeable-remove this line to remove top gradient */
}

#container {
    background: url(../images/bg-bottom-slice.png) repeat-x 100% 100% transparent;  /*Customizeable-remove this line to remove bottom gradient */
}

.title-block {
    background: url("../images/fimlogo.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 600px or less in width. Logo must be 50px or less in height. */
    border-bottom: 2px solid #acd9ec;/*Customizeable- 2px border color under logo */
}

.ie6 .title-block {
    background-image: url(../images/fimlogo-ie6.png);   /*Customizeable- Can make a non-transparent image for IE6 only */
}

h2 {
    color: #578e4c; /*Customizeable- h2 page header color */
}

h3 {
    color: #999; /*Customizeable- h3 page header color */
}

input[type=text]:focus, input[type=password]:focus {
    border: #82bd3b 2px solid; /*Customizeable- Highlight color around textbox when cursor is inside */
}

.chromeButton, .chromeButton:visited {
    background-color: #333; /*Customizeable- Color of button */
    color: #fff; /*Customizeable- Color of text on the button */
    border: 1px solid #666; /*Customizeable- Border color of button */
}

.chromeButton:hover {
    background-color: #666; /*Customizeable- Hover color of button */
    border: 1px solid #999; /*Customizeable- Hover border color of button */
}

.qcol /*Style from QAgate.css */ {
    color: #7a7a7a; /*Customizeable- Font color of Q&A container */
    background-color:#e6e7e9; /*Customizeable- Background color of Q&A container */
}

/****************
Media Queries
*****************/

/* Smartphones ----------- */
@media only screen and (max-width: 480px) {
    body {
        font-size:12px; /*Customizeable- Body Font Size for devices */
    }

    .title-block {
        background: url("../images/fim-logo-portrait.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 190px (landscape) or less in width. Logo must be 50px or less in height. */
    }
    h2, h3 {
        font-size:14px; /*Customizeable- H2 and H3 Heading Size for devices */
    }
}


/* iPads (landscape) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : landscape)
{
}

/* iPads (portrait) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : portrait)
{
}
```


## <a name="common-customization-issues"></a>Problemas comunes de personalización

En la tabla siguiente se muestra una lista de problemas comunes conocidos que pueden producirse con la actualización del servicio y el portal de FIM. También se incluyen soluciones para estos problemas.

|Problema |Solución                                                                    |
|------|------------------------------|
| He realizado la personalización de una cadena, pero no se refleja en la interfaz de usuario         | Las personalizaciones de cadenas en strings.resources siempre requieren iisreset.         |
| Después de realizar un cambio en strings.resources, no veo ninguno de los cambios de mi cadena     | Es probable que el formato de Strings.resources sea incorrecto, de ahí que el portal lo ignore. Compruebe el registro de eventos en Registros de Windows – Registros de aplicaciones y servicios – Forefront Identity Manager                        |
| La primera vez que agregué Style.css, no aparecieron los cambios de estilo en el portal            | La primera vez que se introduce un archivo Style.css, es necesario realizar una acción iisreset.     |
| Los nuevos estilos se agregan o modifican en Style.css, pero los cambios no se ven en el explorador.      | Borre la caché del explorador y actualice la página. <br/> Compruebe la sintaxis de CSS.    |
| Cambié directamente el contenido de la carpeta de CSS en path_to_sspr_portal\\css\\\*.css o el logotipo del banner en path_to_sspr_portal\\images\\fimlogo.png y perdí estos cambios al actualizar | Para comenzar, nunca debe cambiar estos archivos. Use únicamente la carpeta Personalizaciones como una manera de proporcionar un logotipo de banner y personalizaciones de estilo de CSS en Style.css. Las actualizaciones principales no sobrescriben deliberadamente la carpeta Personalizaciones. <br/><br/>No intente usar herramientas como ILSpy y Reflector para cambiar cadenas de los ensamblados del portal. Use strings.resources para invalidar las cadenas predeterminadas. Los ensamblados se reemplazan al actualizar.  |
| El logotipo del banner no se muestra en los portales/aún aparece el logotipo de FIM     | El nombre/ruta de acceso de la imagen en Style.css no es válida o l caché del explorador no se ha borrado.          |
| El logotipo del banner no es atractivo en IE6       | Deberá proporcionar una imagen no transparente para IE6 y un estilo especial complementario en style.css.        |
