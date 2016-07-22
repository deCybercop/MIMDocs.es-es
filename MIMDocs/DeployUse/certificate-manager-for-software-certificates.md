---
title: "Creación de certificados de software | Microsoft Identity Manager"
description: "Descubra cómo usar el Administrador de certificados para crear y renovar certificados de software con las plantillas de perfil."
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fed5ada9-d80f-4825-aad7-4172ac5d71d3
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: d5c8fc4f9a3eaab95441f7a915f7e02d55042ae9


---

# Creación de certificados de software con el Administrador de certificados
Para inscribir y renovar certificados de software no necesita ser un administrador ni tener una tarjeta inteligente virtual. Debe saber que en algún momento se le pedirá que permita una operación de certificado, algo que es normal.

## Creación de un plantilla de perfil de certificado de software en el Administrador de certificados de MIM 2016

1.  Cree una plantilla para el certificado que solicitará para la tarjeta inteligente virtual. Abra MMC.

2.  Haga clic en **Archivo** y, a continuación, en **Agregar o quitar complemento**.

3.  En la lista complementos disponibles, haga clic en **Plantillas de certificado** y, a continuación, haga clic en **Agregar**.

4.  Ahora verá**Plantillas de certificado** bajo Raíz de consola en MMC. Haga doble clic sobre esa opción para ver todas las plantillas de certificado disponibles.

5.  Haga clic con el botón derecho en **Plantilla de usuario** y después haga clic en **Plantilla duplicada**.

6.  En la pestaña **Compatibilidad**, en Entidad de certificación, seleccione Windows Server 2008, y en Destinatario del certificado, Windows 8.1 / Windows Server 2012 R2.

    1.  En la pestaña **General** , en el campo de nombre de pantalla, escriba **Plantilla de certificado archivado**.

    2.  b.  En la pestaña **Tratamiento de la solicitud** ,

        1.  Establezca **Propósito** como Firma y cifrado.

        2.  Seleccione **Incluir los algoritmos simétricos que permite el sujeto**.

        3.  Si desea archivar la clave, consulte **Archivar clave privada de cifrado de sujeto**.

        4.  En Hacer lo siguiente..., seleccione **Preguntar al usuario durante la inscripción**.

    3.  En la pestaña **Criptografía** ,

        1.  en Categorías de proveedor, seleccione **Proveedor de almacenamiento de claves**

        2.  Seleccione **Las solicitudes pueden usar cualquier proveedor disponible en el equipo del sujeto**.

    4.  En la pestaña **Seguridad** , agregue el grupo de seguridad al que quiera proporcionar acceso para **Inscribir** . Por ejemplo, si quiere proporcionar acceso a todos los usuarios, seleccione el grupo **Usuarios autenticados** y después seleccione **Inscribir** para ellos.

    5.  En la pestaña **Nombre del sujeto** ,

        1.  Desactive **Incluir el nombre de correo electrónico en el nombre del sujeto**.

        2.  En **Incluir esta información en un nombre de sujeto alternativo**, desactive **Nombre de correo electrónico**.

    6.  Haga clic en **Aceptar** para dar por finalizados los cambios y crear la plantilla nueva. La plantilla nueva debe aparecer ahora en la lista Plantillas de certificado.

    7.  Seleccione **Archivo** y haga clic en **Agregar o quitar complemento** para agregar el complemento Entidad de certificación a la consola MMC. Cuando se le pregunte qué equipo quiere administrar, seleccione **Equipo local**.

    8.  En el panel izquierdo de MMC, expanda **Entidad de certificación (local)**y después expanda su Entidad de certificación en la lista Entidad de certificación.

    9. Haga clic con el botón derecho en **Plantillas de certificado**, en **Nueva** y, a continuación, en la **Plantilla de certificado** para emitirla.

    10. En la lista, seleccione la plantilla que acaba de crear (**Plantilla de certificado archivado**) y, a continuación, haga clic en **Aceptar**.

## Crear la plantilla de perfil

1.  Inicie sesión en el portal de CM como usuario con privilegios administrativos.

2.  Vaya a **Administración &gt; Administrar plantillas de perfil** y asegúrese de que esté marcada la casilla situada junto a **Plantilla del perfil de inicio de sesión de la tarjeta inteligente de ejemplo de MIM CM** y, después, haga clic en **Copiar una plantilla de perfil seleccionada**.

3.  Escriba el nombre de la plantilla de perfil y haga clic en **Aceptar**.

4.  En la siguiente pantalla, haga clic en **Agregar una nueva plantilla de certificado** y asegúrese de marcar la casilla situada junto al nombre de la entidad de certificación.

5.  Active la casilla situada junto al nombre del certificado de software archivado y haga clic en **Agregar**.

6.  Quite la plantilla de usuario marcando la casilla situada junto a ella y haciendo clic después en **Eliminar plantillas de certificado seleccionadas** y en **Aceptar**.

7.  Haga clic en **Cambiar la configuración general**.

8.  Active las casillas a la izquierda de **Generar claves de cifrado en el servidor** y haga clic en **Aceptar**. En el panel izquierdo, haga clic en **Directiva de recuperación**.

9. Haga clic en **Cambiar la configuración general**.

10. Si desea volver a emitir los certificados archivados, active las casillas a la izquierda de **Volver a emitir certificados archivados** y haga clic en **Aceptar**.

11. Si usa el CM de tarjeta inteligente virtual, debe deshabilitar los elementos de recopilación de datos porque no funciona con la recopilación de datos activada. Deshabilite los elementos de recopilación de datos de cada una de las directivas haciendo clic en la directiva en el panel izquierdo, marcando después la casilla situada junto a **Elemento de datos de ejemplo** y haciendo clic después en **Eliminar elementos de recopilación de datos**. A continuación, haga clic en **Aceptar**.



<!--HONumber=Jul16_HO3-->


