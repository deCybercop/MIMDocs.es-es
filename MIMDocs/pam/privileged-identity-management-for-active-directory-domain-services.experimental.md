---
title: "¿Qué es PAM para AD DS? | Microsoft Docs"
description: Privileged Access Management (PAM) ayuda a las organizaciones a restringir el acceso con privilegios en un entorno existente de Active Directory.
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 03/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: cf3796f7-bc68-4cf7-b887-c5b14e855297
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 00bae1f1c202e4808e641068c13a40f50ffd479a
ms.sourcegitcommit: 2be26acadf35194293cef4310950e121653d2714
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2017
---
# <a name="privileged-access-management-for-active-directory-domain-services"></a>Privileged Access Management para los Servicios de dominio de Active Directory

Privileged Access Management (PAM) ayuda a las organizaciones a restringir el acceso con privilegios en un entorno existente de Active Directory.

![Pasos de PAM: preparación, protección, funcionamiento, supervisión: diagrama](media/MIM_PIM_SetupProcess.png)

Al concentrarse en un ciclo de preparación, protección y supervisión del entorno Privileged Access Management logra dos objetivos:

- Vuelve a establecer el control sobre el entorno de Active Directory en peligro al mantener un entorno bastión independiente que se conoce por no verse afectado por los ataques malintencionados.  
- Aísla el uso de cuentas con privilegios para reducir el riesgo de que roben dichas credenciales.

> [!NOTE]
> PAM es una instancia de [Privileged Identity Management](https://azure.microsoft.com/documentation/articles/active-directory-privileged-identity-management-configure/) (PIM) que se implementa mediante Microsoft Identity Manager (MIM).

## <a name="what-problems-does-pam-help-solve"></a>¿Qué problemas soluciona PAM?

Una preocupación real de las empresas de hoy en día es el acceso a los recursos en un entorno de Active Directory. Resultan especialmente preocupantes las noticias relativas a las vulnerabilidades, las elevaciones de privilegios no autorizadas y otros tipos de acceso no autorizado, como los ataques pass-the-hash, los ataques pass-the-ticket, la suplantación de identidad (phishing) de objetivos específicos y la pérdida de confidencialidad de Kerberos.

Hoy en día, es muy fácil que los atacantes obtengan las credenciales de las cuentas de administradores de dominio, y es muy difícil detectar estos ataques después de que se produzcan. El objetivo de PAM es reducir las oportunidades de que los usuarios malintencionados obtengan acceso, al tiempo que aumenta su control y conocimiento del entorno.

PAM hará que a los atacantes les resulte más difícil penetrar una red y obtener acceso a cuentas con privilegios. PAM agrega protección a los grupos con privilegios que controlan el acceso a una serie de equipos unidos a un dominio y a las aplicaciones de dichos equipos. También agrega mayor supervisión y visibilidad, así como controles más precisos, para que las organizaciones puedan ver quiénes son sus administradores con privilegios y qué están haciendo. PAM permite a las organizaciones comprender mejor cómo se usan las cuentas administrativas en el entorno.

## <a name="how-is-pam-set-up"></a>¿Cómo se configura PAM?

PAM se basa en el principio de la administración Just-In-Time, que se relaciona con [Just Enough Administration (JEA)](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DCIM-B362). JEA es un kit de herramientas de Windows PowerShell que define un conjunto de comandos para realizar actividades con privilegios y un extremo donde los administradores pueden obtener autorización para ejecutar dichos comandos. En JEA, un administrador decide qué usuarios con un privilegio determinado pueden realizar una tarea determinada. Cada vez que un usuario apto necesita realizar esa tarea, le habilitan ese permiso. Los permisos expiran después de un período de tiempo especificado, por lo que un usuario malintencionado no puede apropiarse del acceso.

La configuración y el funcionamiento de PAM consta de cuatro pasos.

1. **Preparación**: identifique los grupos del bosque existente que tengan privilegios importantes. Vuelva a crear estos grupos sin miembros en el bosque bastión.

2. **Protección**: configure el ciclo de vida y la protección de la autenticación, como Multi-Factor Authentication (MFA), cuando los usuarios soliciten administración Just-In-Time. MFA ayuda a evitar ataques programáticos por parte de software malintencionado o el robo subsiguiente de credenciales.

3. **Funcionamiento**: una vez que se cumplen los requisitos de autenticación y se aprueba una solicitud, se agrega una cuenta de usuario temporalmente a un grupo con privilegios en el bosque bastión. Durante un período de tiempo establecido previamente, el administrador tiene todos los privilegios y permisos de acceso que están asignados a ese grupo. Después de ese tiempo, la cuenta se quita del grupo.

4. **Supervisión**: PAM agrega auditoría, alertas e informes de las solicitudes de acceso con privilegios. Puede revisar el historial de acceso con privilegios y ver quién realizó una actividad. Puede decidir si la actividad es válida o no e identificar fácilmente actividades no autorizadas, como un intento de agregar un usuario directamente a un grupo con privilegios en el bosque original. Este paso es importante no solo para identificar el software malintencionado, sino para llevar un seguimiento de los ataques desde dentro de la organización.

## <a name="how-does-pam-work"></a>¿Cómo funciona PAM?

PAM se basa en las funcionalidades nuevas de AD DS, especialmente en el caso de la autenticación y la autorización de cuentas de dominio, y en otras nuevas de Microsoft Identity Manager. PAM separa las cuentas con privilegios de un entorno de Active Directory existente. Cuando es necesario usar una cuenta con privilegios, primero hay que solicitarlo y aprobarlo. Tras la aprobación, la cuenta con privilegios recibe permiso a través de un grupo de entidades de seguridad externas en un nuevo bosque bastión, en lugar de en el bosque actual del usuario o la aplicación. El uso de un bosque bastión ofrece a la organización un mayor control, por ejemplo, en lo relativo a cuándo un usuario puede pertenecer a un grupo con privilegios y cómo debe autenticarse dicho usuario.

Active Directory, el servicio de MIM y otros componentes de esta solución también pueden implementarse en una configuración de alta disponibilidad.

En el ejemplo siguiente se muestra con más detalle el funcionamiento de PIM.

![Participantes y procesos de PIM: diagrama](media/MIM_PIM_howitworks.png)

El bosque bastión emite pertenencias a grupos de tiempo limitado que, a su vez, producen vales concedidos por el servicio de concesión de vales (TGTs) durante un tiempo limitado. Los servicios o aplicaciones basados en Kerberos pueden respetar y aplicar estos TGT si las aplicaciones y los servicios existen en bosques que confían en el bosque bastión.

Las cuentas de usuario diarias no necesitan moverse a un bosque nuevo. Lo mismo pasa con los equipos, las aplicaciones y los grupos. Pueden permanecer en el lugar del bosque donde se encuentren actualmente. Un ejemplo sería el caso de una organización preocupada por los problemas de ciberseguridad de hoy en día, pero que no tiene planes inmediatos para actualizar la infraestructura de servidores a la próxima versión de Windows Server. Dicha organización puede aprovechar igualmente esta solución combinada si usa MIM y un nuevo bosque bastión para poder controlar mejor el acceso a los recursos.

PAM ofrece las siguientes ventajas:

- **Aislamiento y ámbito de privilegios**: los usuarios no tienen privilegios en las cuentas que se usan también para las tareas sin privilegios como la comprobación del correo electrónico o la exploración en Internet. Los usuarios deben solicitar los privilegios pertinentes. Las solicitudes se aprobarán o se denegarán en función de las directivas de MIM que defina un administrador de PAM. Mientras no se apruebe una solicitud, el acceso con privilegios no estará disponible.

- **Actualización a una edición superior y pruebas**: se trata de nuevos desafíos de autenticación y autorización que ayudan a administrar el ciclo de vida de las cuentas administrativas independientes. El usuario puede solicitar la elevación de una cuenta administrativa. A continuación, dicha solicitud pasará por los flujos de trabajo de MIM.

- **Inicio de sesión adicional**: junto con los flujos de trabajo integrados de MIM, hay un inicio de sesión adicional para PAM que identifica la solicitud, cómo se autorizó y los eventos que se producen tras la aprobación.

- **Flujo de trabajo personalizable**: es posible configurar los flujos de trabajo de MIM para diferentes escenarios, y pueden usarse varios flujos de trabajo según los parámetros del usuario que realiza la solicitud o de las funciones solicitadas.

## <a name="how-do-users-request-privileged-access"></a>¿Cómo solicitan los usuarios acceso con privilegios?

Hay varias maneras en que un usuario puede enviar una solicitud, incluidos:  

- API de servicios web de servicios MIM  
- Un extremo de REST  
- Windows PowerShell (`New-PAMRequest`)

## <a name="what-workflows-and-monitoring-options-are-available"></a>¿Qué flujos de trabajo y opciones de supervisión están disponibles?

Por poner un ejemplo, supongamos que un usuario pertenecía a un grupo administrativo antes de que se configurase PIM. Como parte de la configuración de PIM, el usuario se quitó del grupo administrativo y se creó una directiva en MIM. La directiva especifica que, si ese usuario solicita privilegios administrativos y se autentica mediante MFA, la solicitud se aprueba y se agrega una cuenta independiente para el usuario en el grupo con privilegios del bosque bastión.

Suponiendo que la solicitud se apruebe, el flujo de trabajo de acción se comunica directamente con el bosque bastión de Active Directory para incluir a un usuario en un grupo. Por ejemplo, cuando Jen solicita administrar la base de datos de recursos humanos, la cuenta administrativa de Jen se agrega al grupo con privilegios del bosque bastión en cuestión de segundos. Su pertenencia a la cuenta administrativa de ese grupo expirará después de un límite de tiempo. Con Windows Server Technical Preview, esta pertenencia está asociada en Active Directory a un límite de tiempo; con Windows Server 2012 R2 en el bosque bastión, ese límite de tiempo se aplica mediante MIM.

> [!NOTE]
> Cuando se agrega un nuevo miembro a un grupo, el cambio debe replicarse en otros controladores de dominio del bosque bastión. La latencia de replicación puede afectar a la capacidad de los usuarios para acceder a los recursos. Para obtener información sobre la latencia de replicación, vea [How Active Directory Replication Topology Works (Funcionamiento de la topología de replicación de Active Directory)](https://technet.microsoft.com/library/cc755994.aspx).
> En cambio, el Administrador de cuentas de seguridad (SAM) evalúa en tiempo real un vínculo expirado. Aunque la adición de un miembro del grupo necesita replicarse mediante el controlador de dominio que recibe la solicitud de acceso, la eliminación de un miembro del grupo se evalúa al instante en cualquier controlador de dominio.

Este flujo de trabajo está pensado específicamente para estas cuentas administrativas. Los administradores (o incluso los scripts) que solo necesitan un acceso ocasional para grupos con privilegios pueden solicitar el acceso. MIM registra la solicitud y los cambios en Active Directory, y puede verlos en el Visor de eventos o enviar los datos a las soluciones de supervisión empresarial como System Center 2012, Servicios de recopilación de auditorías (ACS) de Operations Manager u otras herramientas de terceros.
