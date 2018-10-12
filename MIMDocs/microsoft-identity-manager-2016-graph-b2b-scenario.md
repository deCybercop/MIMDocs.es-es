---
title: Configurar el conector de Microsoft Identity Manager para Microsoft Graph para B2B| Microsoft Docs
author: billmath
description: Conector de Microsoft Graph es la administración del ciclo de vida de las cuentas de AD del usuario externo. En este escenario, una organización tiene invitados en su directorio de Azure AD y quiere conceder acceso a esos invitados a aplicaciones basadas en Kerberos o en autenticación integrada de Windows local
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: b7623da4c210711bd09882a8222377e725162991
ms.sourcegitcommit: 032b3cdd8a88b1ccfb30c0070f216020feee6293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045673"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>Colaboración de negocio a negocio (B2B) de Azure AD con Microsoft Identity Manager(MIM) 2016 SP1 con el proxy de la aplicación de Azure
============================================================================================================================

El escenario inicial es la administración del ciclo de vida de las cuentas de AD del usuario externo.   En este escenario, una organización tiene invitados en su directorio de Azure AD y quiere conceder acceso a esos invitados a aplicaciones basadas en Kerberos o en autenticación integrada de Windows local, mediante el proxy de la [aplicación de Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) u otros mecanismos de puerta de enlace. El proxy de la aplicación de Azure AD necesita que cada usuario tenga su propia cuenta de AD DS, con fines de identificación y delegación.

## <a name="scenario-specific-guidance"></a>Guía específica del escenario

Algunos supuestos hechos en la configuración de B2B con MIM y Azure AD Application Proxy:

-   Ya ha implementado AD en el entorno local, Microsoft Identity Manager está instalado y se ha establecido la configuración básica del servicio MIM, el Portal de MIM, el agente de administración de Active Directory (ADMA) y el agente de administración de FIM (FIM MA).
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   Ha seguido las instrucciones que aparecen en el artículo sobre cómo descargar e instalar el [conector de Graph](microsoft-identity-manager-2016-connector-graph.md).

-   Ha configurado Azure AD Connect para que los usuarios y los grupos se sincronicen con Azure AD.

-   Ha configurado Azure AD Connect para que los grupos de Office se sincronicen para controlar la aplicación [de nuevo en AD DS en el entorno local](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory).

-   Ya ha configurado conectores y grupos de conectores de Azure Application Proxy, si no, puede ir [aquí](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) para saber cómo instalarlos y configurarlos

-   Ha publicado una o más aplicaciones, que se basan en la autenticación integrada de Windows o en cuentas individuales de AD mediante Azure AD App Proxy.

-   Ha invitado a una o más personas, lo que ha llevado a la creación de uno o más usuarios en Azure AD <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>.



## <a name="b2b-end-to-end-deployment-example-scenario"></a>Escenario de ejemplo de implementación de B2B de un extremo a otro

Esta guía parte del escenario siguiente:

Contoso Pharmaceuticals trabaja con Trey Research Inc. como parte de su departamento de I+D. Los empleados de Trey Research han de tener acceso a la aplicación de generación de informes de investigación que facilita Contoso Pharmaceuticals.

-   Contoso Pharmaceuticals está en su propio inquilino, que ha configurado un dominio personalizado.

-   Alguien ha invitado un usuario externo al inquilino de Contoso Pharmaceuticals.
    Este usuario ha aceptado la invitación y tiene acceso a recursos compartidos.

-   Contoso Pharmaceuticals ha publicado una aplicación a través de App Proxy. En este escenario, la aplicación de ejemplo es el Portal de MIM. Esto permitiría a un usuario invitado participar en los procesos de MIM, por ejemplo, en escenarios del departamento de soporte técnico o para solicitar acceso a los grupos de MIM.


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>Configurar AD y Azure AD Connect para excluir los usuarios agregados desde Azure AD

De forma predeterminada, Azure AD Connect supondrá que los usuarios de Active Directory sin derechos administrativos deben sincronizarse en Azure AD.  Si Azure AD Connect encuentra un usuario existente en Azure AD que coincide con el usuario de AD local, Azure AD Connect hará coincidir las dos cuentas y asumirá que se trata de una sincronización anterior del usuario, lo que hará que AD local sea autoritativo.  Pero este comportamiento predeterminado no es adecuado para el flujo de B2B, donde se origina la cuenta de usuario de Azure AD. 

Por lo tanto, los usuarios que MIM ha incorporado a AD DS desde Azure AD deben almacenarse de forma que Azure AD no intente volver a sincronizarlos con Azure AD.
Una forma de hacerlo es crear una nueva unidad organizativa en AD DS y configurar Azure AD Connect para que excluya esa unidad organizativa.  

Puede encontrar más información en [Azure AD Connect Sync: configuración del filtrado](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering). 
 

## <a name="create-the-azure-ad-application"></a>Crear la aplicación Azure AD 


Nota: Antes de crear el agente de administración en MIM Sync para el conector de Graph, asegúrese de que ha consultado la guía para implementar el [conector de Graph](microsoft-identity-manager-2016-connector-graph.md) y que ha creado una aplicación con un identificador de cliente y un secreto.
Asegúrese de que la aplicación ha autorizado al menos uno de estos permisos: `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` o `Directory.ReadWrite.All`. 

## <a name="create-the-new-management-agent"></a>Crear un nuevo agente de administración


En la interfaz de usuario de Synchronization Service Manager, seleccione **Conectores** y **Crear**.
Seleccione **Graph (Microsoft)** y asígnele un nombre descriptivo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Conectividad

En la página de conectividad, debe especificar la versión de Graph API. La PAI de producción es **V 1.0**, la de no producción es **Beta**.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>Parámetros globales

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Configure Provisioning Hierarchy (Configurar la jerarquía de aprovisionamiento)

Esta página se utiliza para asignar el componente DN, por ejemplo, OU, al tipo de objeto que debe estar aprovisionado, por ejemplo, organizationalUnit. Esto no es necesario para este escenario, así que puede dejarlo como valor predeterminado y hacer clic en Siguiente.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Configurar particiones y jerarquías

En la página de particiones y jerarquías, seleccione todos los espacios de nombres con objetos que se van a importar y exportar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Seleccionar tipos de objeto

En la página de tipos de objeto, seleccione los tipos de objeto que va a importar. Debe seleccionar al menos “Usuario”.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Seleccionar atributos

En la pantalla Seleccionar atributos, seleccione los atributos de Azure AD que serán necesarios para administrar los usuarios de B2B en AD. Se necesita el atributo "ID".  Los atributos `userPrincipalName` y `userType` se usarán más adelante en esta configuración.  Los demás atributos son opcionales, por ejemplo:

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Configurar delimitadores

En la pantalla de configuración del delimitador, es obligatorio configurar el atributo de delimitador. De forma predeterminada, se usa el atributo id. para la asignación de usuario.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Configurar filtro de conector

En la página de configuración del filtro del conector, MIM le permite filtrar los objetos en función del filtro de los atributos. En este escenario de B2B, el objetivo es incluir solamente los usuarios cuyo valor del atributo `userType` sea igual a `Guest` y no a los usuarios con userType igual a `member`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Configurar reglas de unión y proyección

Esta guía presupone que va a crear una regla de sincronización.  Como la configuración de reglas de unión y proyección se controla mediante la regla de sincronización, no hay que identificar ninguna unión ni ninguna proyección en el propio conector. Deje la configuración predeterminada y haga clic en Aceptar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Configurar el flujo de atributos

Esta guía presupone que va a crear una regla de sincronización.  La proyección no es necesaria para definir el flujo de atributos en MIM Sync, ya que se controla mediante la regla de sincronización que se va a crear más adelante. Deje la configuración predeterminada y haga clic en Aceptar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Configurar el desaprovisionamiento

La opción para configurar el desaprovisionamiento le permite configurar la sincronización de MIM para eliminar el objeto, siempre que se elimine el objeto de metaverso. En este escenario, los convertimos en desconectores, puesto que el objetivo es dejarlos en Azure AD. De esta forma, no se exporta nada a Azure AD y el conector se configura solo para la importación.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Configurar extensiones

La configuración de extensiones en este agente de administración es opcional, no obligatoria, ya que se usa una regla de sincronización. Si en el flujo de atributos anterior se decidió usar una regla avanzada, definir la extensión de las reglas sería opcional.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>Ampliar el esquema de metaverso


Antes de crear la regla de sincronización, es necesario crear un atributo llamado userPrincipalName enlazado al objeto Person con el diseñador de máquinas virtuales.

En el cliente de sincronización, seleccione el diseñador de metaverso

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Después, seleccione el tipo de objeto Person

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Luego, en Acciones, haga clic en Agregar atributo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Después, rellene los siguientes datos

Nombre del atributo: **userPrincipalName**

Tipo de atributo: **String (indizable)**

Indizada = **True**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>Creación de reglas de sincronización del servicio MIM

En los pasos siguientes se inicia la asignación de cuenta de invitado de B2B y el flujo de atributos. Aquí se supone que ya tiene configurado el Agente de administración de Active Directory y el Agente de administración de FIM para dirigir a los usuarios al servicio y portal de MIM.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

Para los pasos siguientes será necesario llevar a cabo una configuración mínima adicional para los agentes de administración de FIM y de AD.

Aquí encontrará más detalles para la configuración <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx>: How Do I Provision Users to AD DS (Cómo se aprovisionan usuarios en AD DS).

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Regla de sincronización: Importación de usuario invitado en MV al metaverso del servicio de sincronización de Azure Active Directory<br>

Navegue hasta el portal de MIM, seleccione Reglas de sincronización y haga clic en Nuevo.  Cree una regla de sincronización de entrada para el flujo de B2B a través del conector de Graph.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

En el paso de los criterios de relación, seleccione “Crear recurso en FIM”.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Configure las siguientes reglas de flujo de atributo entrante.  Asegúrese de rellenar los atributos `accountName`, `userPrincipalName` y `uid`, ya que se usarán más adelante en este escenario:

| **Solo flujo inicial** | **Usar como prueba de existencia** | **Flujo (valor de origen ⇒ atributo de FIM)**                          |
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

Esta regla de sincronización crea el usuario en Active Directory.  El flujo para `dn` debe colocar el usuario en la unidad organizativa que se ha excluido de Azure AD Connect.  Actualice también el flujo para `unicodePwd` de modo que cumpla la directiva de contraseñas de AD: no es necesario que el usuario conozca la contraseña.  Tenga en cuenta que el valor de `262656` para `userAccountControl` codifica las marcas `SMARTCARD_REQUIRED` y `NORMAL_ACCOUNT`.

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
| **S**                 |                           | ["CN="+uid+",OU=B2BGuest,DC=contoso,DC=com"⇒dn](javascript:void(0);) |
| **S**                 |                           | [RandomNum(0,999)+userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **S**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Regla de sincronización opcional: importar SID de objetos de usuario invitado de B2B para permitir el inicio de sesión en MIM 

Esta regla de sincronización de entrada vuelve a incluir el atributo del SID del usuario de Active Directory en MIM, para que el usuario pueda acceder al Portal de MIM.  El Portal de MIM necesita que el usuario tenga los atributos `samAccountName`, `domain` y `objectSid` rellenados en la base de datos del servicio MIM.

Configure el sistema de origen externo como `ADMA`, porque AD establecerá automáticamente el atributo `objectSid` cuando MIM cree el usuario.
 
Tenga en cuenta que si configura los usuarios para que se creen en el servicio MIM, debe asegurarse de que no estén en el ámbito de los conjuntos destinados a reglas de la directiva de administración de SSPR de empleados.  Es posible que deba cambiar las definiciones de conjunto para excluir los usuarios que ha creado el flujo de B2B. 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **Solo flujo inicial** | **Usar como prueba de existencia** | **Flujo (valor de origen ⇒ atributo de FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO"⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |


## <a name="run-the-synchronization-rules"></a>Ejecutar las reglas de sincronización

Después, se invita al usuario y luego se ejecutan las reglas de sincronización del agente de administración en el orden siguiente:

-   Importación y sincronización completas en el Agente de administración de `MIMMA`.  Esto garantiza que MIM Sync tenga configuradas las reglas de sincronización más recientes.

-   Importación y sincronización completas en el Agente de administración de `ADMA`.  Esto garantiza que MIM y Active Directory sean coherentes.  En este momento, todavía no hay ninguna exportación pendiente para los invitados.

-   Importación y sincronización completas en el Agente de administración de Graph para B2B.  Esto dirige a los usuarios invitados al metaverso.  En este punto, la exportación de `ADMA` estará pendiente en una o más cuentas.  Si no hay ninguna exportación pendiente, compruebe que los usuarios invitados se hayan importado en el espacio del conector y que las reglas se hayan configurado para que puedan asignarse a cuentas del Agente de administración.

-   Exportación, importación diferencial y sincronización en el Agente de administración de `ADMA`.  Si se produjo un error al exportar, compruebe la configuración de la regla y determine si falta algún requisito del esquema. 

-   Exportación, importación diferencial y sincronización en el Agente de administración de `MIMMA`.  Cuando finalice, ya no debería haber ninguna exportación pendiente.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>Opcional: proxy de aplicación para invitados de B2B que inicien sesión en el Portal de MIM

Ahora que hemos creado las reglas de sincronización de MIM. En la configuración de App Proxy, defina el uso de la entidad de seguridad de la nube para que permita KCD en el proxy de la aplicación.
Además, a continuación se agrega el usuario manualmente a la administración de usuarios y grupos. Las opciones para que no se muestre el usuario hasta que la creación se ha producido en MIM para agregar el invitado a un grupo de Office una vez aprovisionado requiere un poco más de configuración que no se trata en este documento.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


Cuando esté todo configurado, inicie sesión de usuario en B2B y vea la aplicación.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Pasos siguientes
----------

[How Do I Provision Users to AD DS](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx) (Cómo aprovisionar usuarios en AD DS)

[Functions Reference for FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx) (Referencia de funciones para FIM 2010)

[Provisión de acceso remoto seguro a aplicaciones locales.](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Download Microsoft Identity Manager connector for Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495) (Descarga del conector de Microsoft Identity Manager para Microsoft Graph)
