---
title: Microsoft Identity Manager 2016 Service Pack 1 | Microsoft Docs
description: "Comprenda el funcionamiento de MIM 2016 para crear una experiencia de administración de identidades más segura y más cómoda en la nube y en ubicaciones locales."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f0947f186b5206d06a67140706ada33a5bc0e016
ms.openlocfilehash: 4f293a349916ae1a55d8551ef949758cab851b74
ms.lasthandoff: 01/11/2017


---
# <a name="whats-new-for-microsoft-identity-manager-2016-service-pack-1"></a>Novedades en Microsoft Identity Manager 2016 Service Pack 1 #

Como parte del ciclo de lanzamiento regular para el mantenimiento y actualización de Microsoft Identity Manager, nos complace anunciar [Microsoft Identity Manager (MIM) 2016 Service Pack 1 (SP1)](https://msdn.microsoft.com/subscriptions/downloads/?fileid=70212#searchTerm=&Languages=en&PageSize=10&PageIndex=0&FileId=70212). En este documento se describen las actualizaciones, las mejoras, las características y los cambios incluidos en esta versión.

Si encuentra problemas durante la implementación de producción de MIM SP1, póngase en contacto con el soporte técnico de Microsoft.

Queremos conocer su opinión. Si tiene algún comentario o preocupación para el equipo del producto, envíenos un correo electrónico a [mim2016@microsoft.com.](mailto:mim2016@microsoft.com)



## <a name="updates-in-this-service-pack"></a>Actualizaciones en este Service Pack #

### <a name="mim"></a>MIM

- **Compatibilidad entre exploradores del portal de MIM para el autoservicio del usuario final:** en este Service Pack estamos introduciendo soporte técnico para la mayoría de los exploradores principales. Los usuarios ahora pueden obtener acceso al portal de MIM e interactuar con él para grupos de autoservicio y la administración de perfiles desde Edge, Chrome y Safari.

- **Compatibilidad con el servicio de MIM para Exchange Online:** el servicio MIM lleva mucho tiempo admitiendo el envío y la recepción de correos electrónicos para las notificaciones y aprobaciones. Antes de SP1 MIM, solo se admite Exchange Server o SMTP. Con Service Pack 1, el servicio MIM puede enviar y recibir solicitudes, así como las notificaciones de correo electrónico con una cuenta en línea de Office 365 Exchange.

- **Validación de formato de archivo de carga de la imagen:** MIM hora es capaz de validar el formato de archivo de imágenes cuando se cargan en el portal de MIM.

### <a name="privileged-access-managementpam"></a>Privileged Access Management (PAM)

- **Compatibilidad con el bosque de "PRIV" (bastión) de PAM para el nivel funcional de Windows Server 2016:** El servicio MIM PAM puede configurarse en un entorno con los controladores de dominio que se ejecutan en el nivel funcional del bosque de Active Directory Domain Services de Windows Server 2016. Cuando se configura, el vale de Kerberos de un usuario estará limitado en el tiempo al tiempo restante de la activación del rol.

    >[!Note]
    Si opta por mantener el nivel funcional del bosque de Windows Server 2012 R2 en el dominio de CORP, se recomienda instalar [2919442 KB](https://support.microsoft.com/en-us/kb/2919442) y [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) en el controlador de dominio de CORP.

- **Elevación de cuentas con privilegios exclusivos para el bosque de "PRIV" (bastión):** ahora, los administradores pueden informar al servicio de MIM de grupos y usuarios exclusivos para el bosque de "PRIV". Esto permite a estos usuarios y grupos incluirse en los roles de PAM.  Pueden, a continuación, activarse para un rol y asignar una pertenencia a grupos en el bosque de "PRIV".

- **Scripts de implementación de PAM:** los scripts de implementación de PAM permiten a los administradores simplificar la instalación del entorno de PAM.

- **Cmdlets de PAM para la configuración del silo de directivas de autenticación:** Service pack 1 introduce nuevos cmdlets para reforzar la seguridad de su bosque bastión. Estos cmdlets crean automáticamente un silo de directivas de autenticación vinculado a una plantilla de directiva de autenticación.

    >[!Note]
    Estos cmdlets se ejecutan automáticamente como parte de los scripts de las implementaciones.


## <a name="platform-support"></a>Compatibilidad de plataformas
Puede encontrar información actualizada de compatibilidad con la plataforma en el documento denominado [Plataformas compatibles con MIM 2016](/microsoft-identity-manager/plan-design/microsoft-identity-manager-2016-supported-platforms).  Entre las nuevas plataformas compatibles con este Service Pack se incluyen SQL Server 2016 y SharePoint 2016

## <a name="issues-fixed-in-this-release-from-mim-2016-general-availability"></a>Problemas corregidos en esta versión de disponibilidad general de MIM 2016

### <a name="pam"></a>PAM
- New-PAMGroup no ha creado objetos de MIM para grupos locales de dominio en el bosque de PRIV
- New-PAMDomainConfiguration fallaría con un mensaje de error "netdom"
- El servicio de supervisión de PAM registró advertencias para los grupos en el bosque de PRIV

## <a name="how-to-upgrade-to-service-pack-1"></a>Cómo actualizar a Service Pack 1

Los clientes que actualizan a Microsoft Identity Manager 2016 Service Pack 1 deben seguir las directrices siguientes de todos los servicios aplicables a su implementación.

>[!Note]
>Los clientes que ejecutan Forefront Identity Manager 2010 R2 SP1 o una versión anterior deben actualizar primero su entorno a Microsoft Identity Manager 2016, que se publicó en agosto de 2015 y a continuación, seguir los pasos siguientes.

Antes de comenzar

Debe actualizar el motor de sincronización de MIM antes de actualizar el portal y el servicio de MIM.
Debe hacer una copia de seguridad de las bases de datos MIMService y MIM Sync.

  1. Desinstale el componente de Microsoft Identity Manager que va a actualizar
  2. Una vez completada la desinstalación, abra la página inicial de los medios de instalación "FIMSplash.htm"
  3. Seleccione el componente de MIM que actualizar
  4. Continúe con la instalación según las indicaciones
    * Instalación del portal y el servicio de MIM: al elegir Exchange Online como cuenta de correo electrónico, escriba la dirección de correo electrónico y las credenciales de la cuenta de Exchange Online en la pantalla siguiente.

