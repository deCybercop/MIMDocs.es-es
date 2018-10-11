---
title: Conector de Microsoft Identity Manager para Microsoft Graph | Microsoft Docs
author: fimguy
description: El conector de Microsoft Identity Manager para Microsoft Graph permite la administración del ciclo de vida de cuentas de AD de usuarios externos. En este escenario, una organización tiene invitados en su directorio de Azure AD y quiere conceder acceso a esos invitados a aplicaciones basadas en Kerberos o en autenticación integrada de Windows local
keywords: ''
ms.author: billmath
manager: bhu
ms.date: 10/02/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 09052bc9f5cecd0a599e9a93ee43cc44ce435671
ms.sourcegitcommit: 032b3cdd8a88b1ccfb30c0070f216020feee6293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045656"
---
<a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>Conector de Microsoft Identity Manager para Microsoft Graph
=======================================================================================

<a name="summary"></a>Resumen 
=======

El [Conector de Microsoft Identity Manager para Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495) permite escenarios de integración adicionales para los clientes de Azure AD Premium.  Se expone en los objetos adicionales del metaverso de sincronización de MIM obtenidos de la [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 y beta.

<a name="scenarios-covered"></a>Escenarios descritos
=================

<a name="b2b-account-lifecycle-management"></a>Administración del ciclo de vida cuentas de B2B
--------------------------------

El escenario inicial para el conector de Microsoft Identity Manager para Microsoft Graph es como un conector para ayudar a automatizar la administración de ciclo de vida de cuentas de AD DS para usuarios externos. En este escenario, una organización está sincronizando a los empleados a Azure AD desde AD DS mediante Azure AD Connect y también tiene invitados en su directorio de Azure AD. Invitar a un usuario invitado da como resultado un objeto de usuario externo en el directorio de Azure AD de la organización, que no está en el AD DS de la organización. Después la organización quiere conceder acceso a esos invitados a aplicaciones basadas en Kerberos o en autenticación integrada de Windows local, mediante el proxy de la [aplicación de Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) u otros mecanismos de puerta de enlace. El proxy de la aplicación de Azure AD requiere que cada usuario tenga su propia cuenta de AD DS, con fines de identificación y delegación.  

Para obtener información sobre cómo configurar la sincronización de MIM para crear y mantener cuentas de AD DS para los invitados, después de leer las instrucciones de este artículo, siga leyendo el artículo [Colaboración de negocio a negocio (B2B) de Azure AD con Microsoft Identity Manager(MIM) 2016 SP1 con Azure Application Proxy (versión preliminar pública)](~/microsoft-identity-manager-2016-graph-b2b-scenario.md).  Ese artículo muestra las reglas de sincronización necesarias para el conector.

<a name="other-identity-management-scenarios"></a>Otros escenarios de administración de identidad
---------------

El conector puede usarse para otros escenarios de administración de identidad específica que implican crear, leer, actualizar y eliminar objetos de contactos, grupos o usuarios en Azure AD, más allá de la sincronización de grupos y usuarios a Azure AD. Al evaluar los posibles escenarios, tenga en cuenta: este conector no puede realizar operaciones en un escenario, lo que tendría como resultado un conflicto de sincronización potencial o real de un flujo de datos que se superpone con una implementación de Azure AD Connect.  [Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) es el enfoque recomendado para integrar directorios locales con Azure AD, mediante la sincronización de usuarios y grupos de directorios locales a Azure AD.  Azure AD Connect tiene muchas más características de sincronización y permite escenarios como la escritura diferida de dispositivo y contraseña, que no son posibles para los objetos creados por MIM. Si se incorporan datos a AD DS, por ejemplo, asegúrese de que se excluye la posibilidad de que Azure AD Connect intente hacer coincidir esos objetos con el directorio de Azure AD.  Tampoco puede usarse este conector para realizar cambios en objetos de Azure AD que se crearon mediante Azure AD Connect.



<a name="preparing-to-use-the-connector-for-microsoft-graph"></a>Preparación para usar el conector para Microsoft Graph
=============================================================

<a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>Autorizar el conector para recuperar o administrar objetos de directorio de Azure AD
----------------------------------------------------

1.  El conector requiere la creación de una aplicación web o una aplicación de API en Azure AD, para que se pueda autorizar con los permisos adecuados para operar en objetos de Azure AD a través de Microsoft Graph.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Imagen 1. Nuevo registro de aplicaciones

2.  En Azure Portal, abra la aplicación creada y guarde el identificador de aplicación como un identificador de cliente para usar más adelante en la página de conectividad del agente de administración:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Imagen 2. Id. de la aplicación

3.  Genere un nuevo secreto de cliente abriendo Toda la configuración -\> Claves. Establezca una descripción de la clave y seleccione la duración necesaria. Guarde los cambios. Un valor secreto no estará disponible después de salir de la página.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Imagen 3. Nuevo secreto de cliente

4.  Agregue "Microsoft Graph API" a la aplicación abriendo "Permisos necesarios".

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Imagen 4. Agregar nueva API

El siguiente permiso debe agregarse a la aplicación para que pueda usar la “API de Microsoft Graph”, según el escenario:

| Operación con objeto | Permiso necesario                                                                  | Tipo de permiso |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Importar grupo          | `Group.Read.All` o `Group.ReadWrite.All`                                                | Aplicación     |
| Importar usuario           | `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` o `Directory.ReadWrite.All` | Aplicación     |

Encontrará más detalles sobre los permisos necesarios [aquí](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

5. Conceda los permisos necesarios a la aplicación.


<a name="installing-the-connector"></a>Instalación del conector
========================

6.  Antes de instalar el conector, asegúrese de que tiene los siguientes elementos en el servidor de sincronización: 

 - Microsoft .NET 4.5.2 Framework o posterior
 - Microsoft Identity Manager 2016 SP1 y debe usar la revisión 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) o una versión posterior.

7. El conector de Microsoft Graph, junto con otros conectores de Microsoft Identity Manager 2016 SP1, está disponible como descarga desde el [Centro de descarga de Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

8.  Reinicie el servicio de sincronización de MIM.
 
<a name="connector-configuration"></a>Configuración del conector
=======================


9.  En la interfaz de usuario de Synchronization Service Manager, seleccione **Conectores** y **Crear**.
Seleccione **Graph (Microsoft)**, cree un conector y asígnele un nombre descriptivo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. En la interfaz de usuario del servicio de sincronización de MIM, especifique el identificador de aplicación y el secreto de cliente generado. Cada agente de administración configurado en la sincronización de MIM debe tener su propia aplicación en Azure AD para evitar la ejecución de importación en paralelo para la misma aplicación.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Imagen 5. Página Conectividad

La página Conectividad (imagen 5) contiene la versión de Graph API que se usa y el nombre del inquilino. El identificador de cliente y el secreto del cliente representan el identificador de la aplicación y el valor de clave de la aplicación WebAPI que debe crearse en Azure AD.

11. Realice los cambios necesarios en la página Parámetros globales:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Imagen 6. Página Parámetros globales

La página Parámetros globales contiene las siguientes opciones:

- Formato de fecha y hora: formato que se utiliza para cualquier atributo con el tipo Edm.DateTimeOffset. Todas las fechas se convierten en cadenas mediante ese formato durante la importación. El formato de conjunto se aplica a cualquier atributo, lo que evita la fecha.

 - Tiempo de espera HTTP (segundos): tiempo de espera en segundos que se usará durante cada llamada HTTP a la aplicación WebAPI.

 - Obliga a cambiar la contraseña del usuario que se cree en el próximo inicio de sesión: esta opción se utiliza para el nuevo usuario que se va a crear durante la exportación. Si la opción está habilitada, la propiedad [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) se establecerá en true; en caso contrario, será false.

<a name="configuring-the-connector-schema-and-operations"></a>Configuración de las operaciones y el esquema del conector
=========================

12.   Configure el esquema.  El conector es compatible con la siguiente lista de tipos de objeto:

-   Usuario

    -   Importación completa o diferencial

    -   Exportación (agregar, actualizar y eliminar)

-   Group

    -   Importación completa o diferencial

    -   Exportación (agregar, actualizar y eliminar)


Lista de tipos de atributo que se admiten:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset` (cadena en el espacio conector)

-   `microsoft.graph.directoryObject` (referencia en el espacio conector a cualquiera de los objetos admitidos)

-   `microsoft.graph.contact`

También se admiten atributos con varios valores (colección) para cualquier tipo de formulario de la lista anterior.

El conector usa atributo “`id`” para el delimitador y DN para todos los objetos.  Por lo tanto, el cambio de nombre no es necesario, puesto que Graph API no admite que un objeto cambie su atributo “id”.


<a name="access-token-lifetime"></a>Duración del token de acceso
=====================

Una aplicación de Graph requiere un token de acceso para tener acceso a la Graph API. Un conector solicitará un nuevo token de acceso para cada iteración de importación (la iteración de importación depende del tamaño de página). Por ejemplo:

-   Azure AD contiene 10 000 objetos

-   El tamaño de página configurado en el conector es 5000.

En este caso, habrá dos iteraciones durante la importación, cada una de ellas devolverá 5000 objetos para realizar la sincronización. Por lo tanto, un nuevo token de acceso se solicitará dos veces.

Durante la exportación se solicitará un nuevo token de acceso para cada objeto que se deba agregar, actualizar o eliminar.

<a name="troubleshooting"></a>Solucionar problemas
===============

**Habilitar registros**

Si hay algún problema en Graph, pueden usarse los registros para localizar el problema. Por lo que pueden habilitarse seguimientos [de la misma manera que para los conectores genéricos](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). O simplemente agregando lo siguiente a `miiserver.exe.config` (dentro de la sección `system.diagnostics/sources`):


\<source name="ConnectorsLog" switchValue="Verbose"\>

\<listeners\>

>   \<add initializeData="ConnectorsLog"   type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,   Culture=neutral, PublicKeyToken=b77a5c561934e089"   name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,   DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>

>[!NOTE]
>Si la opción “Ejecutar este agente de administración en un proceso independiente” está habilitada, `dllhost.exe.config` debe usarse en lugar de `miiserver.exe.config`.

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
- [Probador de Graph, perfecto para solucionar problemas de llamadas HTTP]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Directivas de control de versiones, compatibilidad y cambios importantes de Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Download Microsoft Identity Manager connector for Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495) (Descarga del conector de Microsoft Identity Manager para Microsoft Graph)

<a name="scenario-specific-guides"></a>Guías específica del escenario
----------------------------------
[Implementación de un extremo a otro de B2B para MIM]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
