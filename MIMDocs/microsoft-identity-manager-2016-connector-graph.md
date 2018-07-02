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
ms.openlocfilehash: a66d424e8388005855ac8e64623f5a00f89682e9
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34479373"
---
<a name="the-microsoft-identity-manager-management-agent-for-microsoft-graph-public-preview"></a>Descarga del Agente de administración de Microsoft Identity Manager para Microsoft Graph (versión preliminar)
=======================================================================================

<a name="summary"></a>Resumen 
=======

El [Agente de administración de Microsoft Identity Manager para Microsoft Graph (versión preliminar)](http://go.microsoft.com/fwlink/?LinkId=717495) permite escenarios de integración adicionales para los clientes de Azure AD Premium.

[Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) se integra en directorios locales con Azure AD y se asegura de que los usuarios tienen una identidad común y una autenticación coherente en todas las aplicaciones de AD DS, Office 365, Azure y SaaS integradas con Azure AD, mediante la sincronización de usuarios y grupos de directorios locales en Azure AD.   Este agente de administración se puede implementar para operaciones de administración de acceso e identidades especializadas más allá de la sincronización de usuarios y grupos en Azure AD.  Este agente de administración se expone en los objetos adicionales del metaverso de sincronización de MIM obtenidos de la [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 y beta. 

<a name="scenarios-covered"></a>Escenarios descritos
=================

<a name="b2b-account-lifecycle-management"></a>Administración del ciclo de vida cuentas de B2B
--------------------------------

El escenario inicial en la versión preliminar para el Agente de administración de Microsoft Identity Manager para Microsoft Graph (versión preliminar) es la administración del ciclo de vida de las cuentas de AD del usuario externo. En este escenario, una organización tiene invitados en su directorio de Azure AD y quiere conceder acceso a esos invitados a aplicaciones basadas en Kerberos o en autenticación integrada de Windows local, mediante el proxy de la [aplicación de Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) u otros mecanismos de puerta de enlace. El proxy de la aplicación de Azure AD requiere que cada usuario tenga su propia cuenta de AD DS, con fines de identificación y delegación

Es posible que se agreguen más escenarios en el futuro y se [documenten aquí](./microsoft-identity-manager-2016-graph-b2b-scenario.md).

<a name="determining-your-deployment-topology"></a>Determinación de la topología de implementación
====================================


<a name="preparing-to-use-the-management-agentma-for-microsoft-graph"></a>Preparación para el uso del Agente de administración de Microsoft Graph
=============================================================

<a name="authorizing-the-ma-to-manage-your-azure-ad-directory"></a>Autorización del Agente de administración para administrar su directorio de Azure AD
----------------------------------------------------

1.  El Agente de administración de Graph requiere que la aplicación web o la aplicación de API se creen en AzureAD.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Imagen 1. Nuevo registro de aplicaciones

2.  Abra la aplicación creada y use el identificador de la aplicación como identificador de cliente en la página de conectividad del agente de administración:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Imagen 2. Id. de la aplicación

2.  Genere un nuevo secreto de cliente abriendo Toda la configuración -\> Claves. Establezca una descripción de la clave y seleccione la duración necesaria. Guarde los cambios. Un valor secreto no estará disponible después de salir de la página.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Imagen 3. Nuevo secreto de cliente

3.  Agregue "Microsoft Graph API" a la aplicación abriendo "Permisos necesarios".

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Imagen 4. Agregar nueva API

El permiso siguiente debe agregarse a "Microsoft Graph API":

| Operación con objeto | Permiso necesario                                                                  | Tipo de permiso |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Importar grupo          | Group.Read.All o Group.ReadWrite.All                                                | Aplicación     |
| Importar usuario           | User.Read.All, User.ReadWrite.All, Directory.Read.All o Directory.ReadWrite.All | Aplicación     |

Encontrará más detalles sobre los permisos necesarios [aquí](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

1.  Cree el conector con el identificador de aplicación y el secreto de cliente generado. Cada agente de administración debe tener su propia aplicación en AzureAD para evitar que la importación se ejecute en paralelo para la misma aplicación. El conector de Graph es compatible con la siguiente lista de tipos de objeto:

-   Usuario

    -   Importación completa o diferencial

    -   Exportación (agregar, actualizar y eliminar)

-   Group

    -   Importación completa o diferencial

    -   Exportación (agregar, actualizar y eliminar)


Lista de tipos de atributo que se admiten:

-   Edm.Boolean

-   Edm.String

-   Edm.DateTimeOffset (string en el espacio conector)

-   microsoft.graph.directoryObject (referencia en el espacio conector a cualquiera de los objetos admitidos)

-   microsoft.graph.contact

También se admiten atributos con varios valores (colección) para cualquier tipo de formulario de la lista anterior.

El conector de Graph usa atributo "id" para el delimitador y DN para todos los objetos.

El cambio de nombre no se admite en este momento, porque GraphAPI no permite que el objeto cambie el atributo "id" del objeto existente.

<a name="access-token-lifetime"></a>Duración del token de acceso
=====================

Una aplicación de Graph requiere un token de acceso para tener acceso a la GraphAPI. Un conector solicitará un nuevo token de acceso para cada iteración de importación (la iteración de importación depende del tamaño de página). Por ejemplo:

-   AzureAD contiene 10 000 objetos.

-   El tamaño de página configurado en el conector es 5000.

En este caso, habrá dos iteraciones durante la importación, cada una de ellas devolverá 5000 objetos para realizar la sincronización. Por lo tanto, un nuevo token de acceso se solicitará dos veces.

Tenga en cuenta que, durante la exportación, se solicitará un nuevo token de acceso para cada objeto que se deba agregar/actualizar/eliminar.

<a name="installing-the-connector"></a>Instalación del conector
========================

Antes de usar el conector, asegúrese de que dispone de lo siguiente en el servidor de sincronización: Microsoft .NET 4.5.2 Framework o versiones posteriores. Microsoft Identity Manager 2016 SP1 debe usar la revisión 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) o versiones posteriores.

Los conectores de Microsoft Identity Manager 2016 SP1, el conector de Graph está disponible como descarga desde el [Centro de descarga de Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

<a name="connector-configuration"></a>Configuración del conector
=======================

Página Conectividad:

![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Imagen 5. Página Conectividad

La página Conectividad (imagen 1) contiene la versión de Graph API que se utiliza y el nombre del inquilino. El identificador de cliente y el secreto del cliente representan el identificador de la aplicación y el valor de clave de la aplicación WebAPI que debe crearse en AzureAD.

Página Parámetros globales:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Imagen 6. Página Parámetros globales

La página Parámetros globales contiene las siguientes opciones:

Formato de fecha y hora: formato que se utiliza para cualquier atributo con el tipo Edm.DateTimeOffset. Todas las fechas se convierten en cadenas mediante ese formato durante la importación. El formato de conjunto se aplica a cualquier atributo, lo que evita la fecha.

Tiempo de espera HTTP (segundos): tiempo de espera en segundos que se usará durante cada llamada HTTP a la aplicación WebAPI.

Obliga a cambiar la contraseña del usuario que se cree en el próximo inicio de sesión: esta opción se utiliza para el nuevo usuario que se va a crear durante la exportación. Si la opción está habilitada, la propiedad [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) se establecerá en true; en caso contrario, será false.

<a name="troubleshooting"></a>Solucionar problemas
===============

**Habilitar registros**

Si hay algún problema en Graph, pueden usarse los registros para localizar el problema. El conector de Graph utiliza el mismo origen que todos los conectores genéricos. Por lo que pueden habilitarse seguimientos [de la misma manera que para los conectores genéricos; vea la wiki](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). O bien, simplemente agregando lo siguiente a miiserver.exe.config (en la sección system.diagnostics/sources):

\<source name="ConnectorsLog" switchValue="Verbose"\>

\<listeners\>

>   \<add initializeData="ConnectorsLog"   type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,   Culture=neutral, PublicKeyToken=b77a5c561934e089"   name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,   DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>

Tenga en cuenta que, si se habilita "Run this management agent in a separate process" (Ejecutar este agente de administración en un proceso independiente), debe usarse dllhost.exe.config en lugar de miiserver.exe.config.

**Error de token de acceso expirado**

El conector podría devolver el error HTTP 401 No autorizado, mensaje "Access token has expired." (El token de acceso expiró.):

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Imagen 7. "Access token has expired". (El token de acceso expiró.) Error

La causa de este problema podría ser la configuración de duración del token de acceso desde Azure. De forma predeterminada, el token de acceso expira después de una hora. Para aumentar el tiempo de expiración, visite [este artículo](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Ejemplo de esto mediante el uso de [la versión preliminar pública del módulo de Azure AD para PowerShell](https://www.powershellgallery.com/packages/AzureADPreview).

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy -Definition \@('{"TokenLifetimePolicy":{"Version":1, **"AccessTokenLifetime":"5:00:00"**}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault \$true -Type "TokenLifetimePolicy"

<a name="next-steps"></a>Pasos siguientes
----------
- [Explorador de gráficos (perfecto para solucionar problemas de llamadas HTTP)]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Directivas de control de versiones, compatibilidad y cambios importantes de Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Descarga del Agente de administración de Microsoft Identity Manager para Microsoft Graph (versión preliminar)](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-supported-guides"></a>Guías admitidas específicas del escenario
----------------------------------
[Implementación de un extremo a otro de B2B para MIM]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
