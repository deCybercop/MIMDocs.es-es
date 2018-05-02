---
title: Agente de administración de Microsoft Identity Manager para Microsoft Graph | Microsoft Docs
author: fimguy
description: Microsoft Graph (versión preliminar) es la administración del ciclo de vida de las cuentas de AD del usuario externo. En este escenario, una organización tiene invitados en su directorio de Azure AD y quiere conceder acceso a esos invitados a aplicaciones basadas en Kerberos o en autenticación integrada de Windows local
keywords: ''
ms.author: davidste
manager: bhu
ms.date: 04/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 77d322f447546897ad18f0981e5faad12efafef1
ms.sourcegitcommit: 637988684768c994398b5725eb142e16e4b03bb3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2018
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy-public-preview"></a>Colaboración de negocio a negocio (B2B) de Azure AD con Microsoft Identity Manager(MIM) 2016 SP1 con Azure Application Proxy (versión preliminar pública)
============================================================================================================================

El escenario inicial de la versión preliminar es la administración del ciclo de vida de las cuentas de AD del usuario externo.   En este escenario, una organización tiene invitados en su directorio de Azure AD y quiere conceder acceso a esos invitados a aplicaciones basadas en Kerberos o en autenticación integrada de Windows local, mediante el proxy de la [aplicación de Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-publish) u otros mecanismos de puerta de enlace. El proxy de la aplicación de Azure AD requiere que cada usuario tenga su propia cuenta de AD DS, con fines de identificación y delegación

## <a name="scenario-specific-supported-guidance"></a>Instrucciones admitidas específicas del escenario

En este escenario, una organización tiene invitados en su directorio de Azure AD y quiere conceder acceso a esos invitados a Windows local. Aplicaciones basadas en Kerberos o en la autenticación integrada, mediante el proxy de la [aplicación de Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-publish) u otros mecanismos de puerta de enlace. El proxy de la aplicación de Azure AD requiere que cada usuario tenga su propia cuenta de AD DS, con fines de identificación y delegación

Algunos supuestos hechos en la configuración de B2B con MIM y Azure Application Proxy

-   Ya ha instalado el [Agente de administración de Graph](microsoft-identity-manager-2016-connector-graph.md).

-   Dispone de una instancia de AD local y de Azure AD Connect para sincronizar usuarios y grupos con Azure AD.

    -   Grupos de Office que controlan el acceso a las aplicaciones con [Azure AD Connect](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)

-   Ya ha configurado conectores y grupos de conectores de Azure Application Proxy, si no, puede ir [aquí](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) para saber cómo instalarlos y configurarlos

-   Se han publicado una o más aplicaciones, que se basan en la autenticación integrada de Windows o en cuentas individuales de AD mediante Azure AD App Proxy

-   Ha invitado a una o más personas, que se crean en Azure AD <https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-self-service-portal>

-   Microsoft Identity Manager está instalado con la configuración básica del servicio y el portal y el Agente de administración de Active Directory.
    <https://docs.microsoft.com/en-us/microsoft-identity-manager/microsoft-identity-manager-deploy>

## <a name="b2b-end-to-end-deployment"></a>Implementación de un extremo a otro de B2B

Escenario

Contoso Pharmaceuticals trabaja con Trey Research Inc. como parte de su departamento de I+D. Los empleados de Trey Research han de tener acceso a la aplicación de generación de informes de investigación que facilita Contoso Pharmaceuticals.

-   Contoso Pharmaceuticals está en un inquilino independiente, que ha configurado un dominio personalizado.

-   Se ha invitado un usuario externo al inquilino de Contoso Pharmaceuticals.
    Este usuario ha aceptado la invitación y tiene acceso a recursos compartidos.

-   Se ha publicado una aplicación mediante App Proxy y, en este escenario, como ejemplo de uso del servicio y portal de MIM para que el invitado participe en escenarios del departamento de soporte técnico de ejemplo y de proceso de MIM.

## <a name="create-the-graph-management-agent"></a>Creación del Agente de administración de Graph

Nota: Antes de crear el conector, compruebe que ha revisado el [Agente de administración de Graph](microsoft-identity-manager-2016-connector-graph.md).

En la interfaz de usuario de Synchronization Service Manager, seleccione **Conectores** y **Crear**.
Seleccione **Graph (Microsoft)** y asígnele un nombre descriptivo

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Conectividad

En la página Conectividad, debe especificar que la versión de Graph API lista para producción es **V 1.0** y para no producción es **Beta**.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="capabilities"></a>Funcionalidades

En la página Parámetros globales, configure el DN en el registro de cambios diferenciales y características adicionales de LDAP. La página se rellena automáticamente con la información proporcionada por el servidor LDAP.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="global-parameters"></a>Parámetros globales

En la página Parámetros globales, configure el DN en el registro de cambios diferenciales y características adicionales de LDAP. La página se rellena automáticamente con la información proporcionada por el servidor LDAP.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Configure Provisioning Hierarchy (Configurar la jerarquía de aprovisionamiento)

Esta página se utiliza para asignar el componente DN, por ejemplo, OU, al tipo de objeto que debe estar aprovisionado, por ejemplo, organizationalUnit. Deje este valor predeterminado y haga clic en siguiente.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Configurar particiones y jerarquías

En la página de particiones y jerarquías, seleccione todos los espacios de nombres con objetos que se van a importar y exportar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Seleccionar tipos de objeto

En la página de particiones y jerarquías, seleccione todos los espacios de nombres con objetos que se van a importar y exportar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Seleccionar atributos

En la pantalla Seleccionar atributos, seleccione los atributos necesarios para administrar los usuarios de B2B. Se requiere el atributo "id".

-   id

-   displayName

-   mail

-   givenName

-   surname

-   userPrincipalName

-   userType

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Configurar delimitadores

Con la opción de configuración de delimitadores, configurar el delimitador es un paso necesario. De forma predeterminada, se usa el atributo id para la asignación.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Configurar filtro de conector

La página de configuración del filtro de conector le permite filtrar los objetos en función del filtro de atributos. En este escenario de B2B, el objetivo es incluir únicamente a los usuarios con el userType igual a invitado que no sean miembros.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Configurar reglas de unión y proyección

La configuración de reglas de unión y proyección se controla mediante la regla de sincronización, no hay que identificar ninguna unión ni ninguna proyección en el propio conector. Deje la configuración predeterminada y haga clic en Aceptar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Configurar el flujo de atributos

Al igual que en la unión y proyección, no es necesario definir el flujo de atributos, ya que se controla mediante la regla de sincronización que se va a crear más adelante. Deje la configuración predeterminada y haga clic en Aceptar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Configurar el desaprovisionamiento

La configuración del desaprovisionamiento le permite eliminar el objeto si se elimina el objeto de metaverso. En esta prueba, se convierten en desconectores, ya que el objetivo es dejarlos en Azure, tampoco se exporta nada a Azure ya que es de solo importación.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Configurar extensiones

La configuración de extensiones en este agente de administración es opcional, no obligatoria, ya que se usa una regla de sincronización. Si en el flujo de atributos anterior se decidió una regla avanzada, esta definición sería opcional.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="creating-mim-service-synchronization-rules"></a>Creación de reglas de sincronización del servicio MIM

En los pasos siguientes se inicia la asignación de cuenta de invitado de B2B y el flujo de atributos. Aquí se supone que ya tiene configurado el Agente de administración de Active Directory y el Agente de administración de FIM para comunicarse con el servicio y portal de MIM.

Antes de crear la regla de sincronización, es necesario crear un atributo llamado userPrincipalName enlazado al objeto Person con el diseñador de máquinas virtuales.

En el cliente de sincronización, seleccione el diseñador de metaverso

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Después, seleccione el tipo de objeto Person

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Luego, en Acciones, haga clic en Agregar atributo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Por último, rellene los siguientes datos

Nombre del atributo: **userPrincipalName**

Tipo de atributo: **String (indizable)**

Indizada = **True**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

Los pasos siguientes requerirán la configuración mínima del Agente de administración del servicio FIM y el Agente de administración de Active Directory Domain Services.

Aquí encontrará más detalles para la configuración <https://technet.microsoft.com/en-us/library/ff686263(v=ws.10).aspx>: How Do I Provision Users to AD DS (Cómo se aprovisionan usuarios en AD DS).

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Regla de sincronización: Importación de usuario invitado en MV al metaverso del servicio de sincronización de Azure Active Directory<br>

Navegue hasta el servicio y portal de MIM, seleccione reglas de sincronización y haga clic en Nuevo.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png) Seleccione "Crear recurso en FIM" ![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Reglas de flujo:

| **Solo flujo inicial** | **Usar como prueba de existencia** | **Flujo (Valor de FIM ⇒Atributo de destino)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName](javascript:void(0);)                        |
|                       |                           | [Left(id,20)⇒accountName](javascript:void(0);)                        |
|                       |                           | [id⇒uid](javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType](javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [surname⇒sn](javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
|                       |                           | [id⇒cn](javascript:void(0);)                                          |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone](javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Regla de sincronización: Crear cuenta de usuario invitado en Active Directory 

Esta regla de sincronización crea el usuario invitado en Active Directory

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Reglas de flujo:

| **Solo flujo inicial** | **Usar como prueba de existencia** | **Flujo (Valor de FIM ⇒Atributo de destino)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **S**                 |                           | ["CN="+uid+",OU=B2BGuest,DC=scontoso,DC=com"⇒dn](javascript:void(0);) |
| **S**                 |                           | [RandomNum(0,999)+userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **S**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Regla de sincronización: importar SID de objetos de usuario invitado de B2B para permitir el inicio de sesión en MIM 

Esta regla de sincronización crea el usuario invitado en Active Directory

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)

Por último, se invita al usuario y luego se ejecuta la administración en el orden siguiente:

-   Importación y sincronización completas en el Agente de administración de MIMMA

-   Importación y sincronización completas en el Agente de administración de ADMA_SCONTOSO_B2B

-   Importación y sincronización completas en el Agente de administración de Graph para B2B

-   Exportación, importación diferencial y sincronización en el Agente de administración de ADMA_SCONTOSO_B2B

-   Exportación, importación diferencial y sincronización en el Agente de administración de MIMMA

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)

## <a name="finally-application-proxy-with-b2b-guest-and-logging-into-mim"></a>Por último: proxy de la aplicación con invitado de B2B e inicio de sesión en MIM

Ahora que hemos creado las reglas de sincronización de MIM. En la configuración de App Proxy, defina el uso del principio de la nube para permitir KCD en el proxy de la aplicación.
Además, a continuación se agrega el usuario manualmente a la administración de usuarios y grupos. Las opciones para que no se muestre el usuario hasta que la creación se ha producido en MIM para agregar el invitado a un grupo de Office una vez aprovisionado requiere un poco más de configuración que no se trata en este documento.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)

| **Solo flujo inicial** | **Usar como prueba de existencia** | **Flujo (Valor de FIM ⇒Atributo de destino)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO"⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |

Configuración de todo una vez

Por último, inicie sesión de usuario en B2B y vea la aplicación.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Pasos siguientes
----------

[How Do I Provision Users to AD DS](https://technet.microsoft.com/en-us/library/ff686263(v=ws.10).aspx) (Cómo aprovisionar usuarios en AD DS)

[Functions Reference for FIM 2010](https://technet.microsoft.com/en-us/library/ff800820(v=ws.10).aspx) (Referencia de funciones para FIM 2010)

[Provisión de acceso remoto seguro a aplicaciones locales.](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-get-started)

[Descarga del Agente de administración de Microsoft Identity Manager para Microsoft Graph (versión preliminar)](http://go.microsoft.com/fwlink/?LinkId=717495)
