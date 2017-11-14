---
title: "Instalación de BHOLD FIM/MIM Integration | Microsoft Docs"
description: "El módulo BHOLD FIM/MIM Integration agrega capacidades de administración de roles de autoservicio a MIM y FIM"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: ef68de19bd0eabd6d9203469ecc991d496f05846
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2017
---
# <a name="bhold-fimmim-integration-installation"></a>Instalación de BHOLD FIM/MIM Integration

El módulo BHOLD FIM Integration agrega a Microsoft Identity Manager capacidades de administración de roles de autoservicio, lo que permite a los usuarios solicitar roles adicionales y exigir quién puede asumir dichos roles. El módulo BHOLD FIM Integration amplía el portal de FIM para que resulte fácil administrar los roles de los usuarios como parte de la administración general de FIM. En este tema se describe cómo se debe configurar la infraestructura de red para poder instalar y usar el módulo BHOLD FIM Integration. También se describe cómo instalar el módulo BHOLD FIM Integration y la configuración necesaria después de instalar el módulo BHOLD FIM Integration.

## <a name="bhold-fim-integration-installation-requirements"></a>Requisitos de instalación de BHOLD FIM Integration

El módulo BHOLD FIM Integration amplía el portal de FIM y el servicio FIM para permitir a los usuarios administrar sus roles en el portal de FIM. Por esta razón, es fundamental tener instalado y configurado el módulo BHOLD Core y las características necesarias de FIM antes de instalar el módulo BHOLD FIM Integration.
Estos son los componentes de software que deben estar presentes en el equipo antes de instalar el módulo BHOLD FIM Integration:

- Portal y servicio de Microsoft Identity Manager 2016
- Microsoft Silverlight 3 o posterior
- Internet information Services y ASP.NET
- Microsoft Silverlight Tools

Además, los módulos BHOLD Core y Access Management Connector deben estar implementados en un servidor del entorno y FIM debe configurarse con uno o varios agentes de administración de BHOLD. Para más información sobre la instalación y la configuración del módulo BHOLD Core, vea [Instalación de BHOLD Core](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). Para más información sobre cómo instalar y usar el módulo Access Management Connector, vea [Access Management Connector Installation](https://technet.microsoft.com/en-us/library/jj874042(v=ws.10).aspx) (Instalación de Access Management Connector) y [Test Lab Guide: BHOLD Access Management Connector](https://technet.microsoft.com/en-us/library/jj853085(v=ws.10).aspx) (Guía del laboratorio de pruebas: BHOLD Access Management Connector).

>[!IMPORTANT]
El nombre de la base de datos del servicio FIM debe ser FIMService. Si no se instala FIM con el nombre predeterminado de la base de datos del servicio FIM, se producirá un error en la instalación de BHOLD FIM Integration.

## <a name="before-you-begin"></a>Antes de comenzar

Antes de empezar a instalar el módulo BHOLD FIM Integration, debe crear un directorio de BHOLD en el directorio raíz de la unidad de disco C: (C:\BHOLD).

Además, deberá estar preparado para proporcionar la información que el Asistente para la instalación de BHOLD FIM Integration le pide para completar la instalación. Esta hoja de cálculo le ayudará a anotar esta información y, así, tenerla preparada para cuando se le pida.

### <a name="bholdfim-account-settings"></a>Configuración de la cuenta BHOLDFim

| **Elemento**                            | **Descripción**                                                                                                                                                                                                               | **Valor**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Security Provider on Domain (Usar proveedor de seguridad en dominio)** | Cuando se selecciona, especifica que la seguridad de Active Directory Domain Services controlará el acceso a BHOLD Core.                                                                                                                    | Active la casilla de verificación. **Importante:** se producirá un error en la instalación si no se activa esta casilla.                                                                                                                                                                                                                   |
| **Dominio**                          | Especifica el dominio que contiene la **cuenta de servicio** que se creó al instalar BHOLD Core. Para más información, vea [Instalación de BHOLD Core](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | El nombre de dominio lo proporciona automáticamente el asistente. Cambie el nombre solamente si es incorrecto. **Importante:** especifique el nombre de dominio mediante el nombre de NetBIOS (corto), en lugar del nombre de dominio completo (FQDN). Por ejemplo, si el FQDN del dominio es fabrikam.com, especifique el nombre de dominio como FABRIKAM. |
| **Nombre de usuario**                        | Especifica el nombre de inicio de sesión de la cuenta de usuario del servicio de BHOLD Core.                                                                                                                                                              | Escriba aquí el nombre de la cuenta de usuario.                                                                                                                                                                                                                                                                                    |
| **Contraseña**                        | Especifica la contraseña de la cuenta de usuario del servicio.                                                                                                                                                                           | Escriba aquí la contraseña. **Importante:** no olvide guardar esta contraseña en una ubicación segura y oculta.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>Configuración del servicio FIM

| **Elemento**            | **Descripción**                                                                                                                                                                                                                               | **Valor**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **User**            | Especifica el nombre de inicio de sesión de una cuenta con privilegios de administrador para FIM. Microsoft recomienda encarecidamente que no se use la cuenta asociada con el usuario raíz en BHOLD Core (de forma predeterminada, la cuenta usada para instalar BHOLD Core). | Escriba aquí el nombre de la cuenta de usuario.                                                                   |
| **Contraseña**        | Especifica la contraseña de la cuenta de usuario del administrador de FIM.                                                                                                                                                                                 | Escriba aquí la contraseña. **Importante:** no olvide guardar esta contraseña en una ubicación segura y oculta. |
| **FIM Database** (Base de datos de FIM)    | Especifica el nombre de la base de datos del servicio FIM.                                                                                                                                                                                               | FIMService                                                                                          |
| **Website IP/Port (Puerto/Dirección IP del sitio web)** | Especifica el nombre o la dirección IP del servidor del portal de FIM y el puerto del sitio web.                                                                                                                                                               | Escriba aquí el nombre de servidor o la dirección y el puerto.                                                     |

### <a name="bhold-core-connection"></a>Conexión de BHOLD Core

| **Elemento**               | **Descripción**                                                                                                                                                                                                                                                                                                                                                                               | **Valor**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Dominio**             | Especifica el nombre de dominio de la cuenta especificada en **Usuario**, la opción siguiente. Especifique el dominio con el formato NetBIOS (corto).                                                                                                                                                                                                                                                                   | Escriba aquí el nombre de dominio de la cuenta de usuario.                                                            |
| **User**               | Especifica el nombre de inicio de sesión de la cuenta de **un usuario de BHOLD que sea supervisor** de todos los usuarios y roles, y que tenga permiso para vincular y desvincular roles de usuario. Microsoft recomienda encarecidamente que no se use la cuenta asociada con el usuario raíz en BHOLD Core (de forma predeterminada, la cuenta usada para instalar BHOLD Core). Esta cuenta puede ser la misma que usa para conectarse a FIM. | Escriba aquí el nombre de la cuenta de usuario.                                                                   |
| **Contraseña**           | Especifica la contraseña de la cuenta de usuario especificada en **Usuario**.                                                                                                                                                                                                                                                                                                                             | Escriba aquí la contraseña. **Importante:** no olvide guardar esta contraseña en una ubicación segura y oculta. |
| **IP/Machine Address (Dirección IP/de máquina)** | Especifica la dirección IP del servidor del sitio web de BHOLD Core. No use el nombre de servidor.                                                                                                                                                                                                                                                                                                        | Escriba aquí la dirección IP.                                                                          |
| **Número de puerto**        | Especifica el número de puerto del sitio web de BHOLD Core.                                                                                                                                                                                                                                                                                                                                          | Escriba aquí el número de puerto.                                                                         |

## <a name="bhold-fim-integration-setup"></a>Instalación de BHOLD FIM Integration

Para instalar el módulo BHOLD FIM Integration, inicie sesión como miembro del grupo Administradores del dominio, descargue el archivo siguiente y ejecútelo como administrador en el servidor en el que quiere instalar el módulo BHOLD FIM Integration:

- BholdFIMIntegration*\<versión\>*\_Release.msi

Reemplace *\<versión\>* con el número de la versión de BHOLD FIM Integration que va a instalar.

Para ejecutar el archivo de programa como administrador, haga clic con el botón derecho en el archivo y luego haga clic en **Ejecutar como administrador**.

![msi en ejecución](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Tareas posteriores a la instalación

Después de instalar BHOLD FIM Integration, debe configurar Microsoft SharePoint para que conceda permisos de propietario del sitio a la cuenta de servicio de BHOLD. Además, si el portal de FIM está configurado para usar la Capa de sockets seguros (SSL), debe modificar los archivos que contienen referencias a las direcciones de las páginas de BHOLD agregadas al portal de FIM.

### <a name="configuring-sharepoint"></a>Configuración de SharePoint

Para que BHOLD FIM Integration funcione correctamente, es necesario que la cuenta de servicio de BHOLD tenga permisos de miembro del sitio en Microsoft SharePoint.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Para conceder permisos de miembro del sitio a la cuenta de servicio de BHOLD

1.  Inicie sesión en el servidor que ejecuta BHOLD FIM Integration con privilegios de administrador.

2.  Haga clic en **Inicio** y elija **Internet Explorer**.

3.  En la barra de direcciones, escriba <https://localhost> si SharePoint está configurado para usar seguridad SSL. En caso contrario, escriba <http://localhost>.

4.  En el lado izquierdo de la página **Sitio de grupo**, haga clic en **Personas y grupos**.

5.  En **Grupos**, haga clic en **Team Site Members** (Miembros de sitio de grupo) y, en la barra de herramientas del panel central, haga clic en **Nuevo** y en **Agregar usuarios**.

6.  En la página **Add Users: Team Site** (Agregar usuarios: sitio de grupo), en **Usuarios/Grupos**, escriba BHOLDApplicationGroup y haga clic en el botón Comprobar nombres debajo del cuadro **Ususarios/Grupos**. Debe resolver el nombre del grupo para que incluya el nombre de dominio.

7.  Haga clic en **Give users permissions directly** (Conceder permisos a los usuarios directamente), seleccione **Full Control – Has full control** (Control total - Tiene control total) y luego haga clic en **Aceptar**.

8.  Compruebe que BHOLDApplicationGroup aparece en la lista **Permissions: Team Site** (Permisos: Sitio de grupo) y después cierre Internet Explorer.

![msi en ejecución](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>Configuración de BHOLD para admitir SSL

Si el portal de FIM está configurado para usar la seguridad SSL, debe modificar los archivos en el servidor FIM para que se abran vínculos a páginas de BHOLD. Los archivos se encuentran en esta carpeta: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

En la tabla siguiente se enumeran los archivos y las versiones originales y cambiadas de las cadenas que se van a editar.

| **Archivo**                  | **Cadena original**                                                                                                                   | **Cadena cambiada**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<Servidor_BHOLD>/bhold/Analytics/index_fim.html | https://<FQDN_servidor_BHOLD>/bhold/Analytics/index_fim.html       |
| AttestationCampaigns.aspx |    http://<Servidor_BHOLD>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<FQDN_servidor_BHOLD>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<Servidor_BHOLD>/bhold/Attestation/Dashboard.aspx?hideMenu=1        | https://<FQDN_servidor_BHOLD>/bhold/Attestation/Dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<Servidor_BHOLD>/bhold/Reporting/index_fim.html |  https://<FQDN_servidor_BHOLD>/bhold/Reporting/index_fim.html |
| Selfservice.aspx          | RoleExchangePoint=http://\<*Servidor_FIM*\>: \<*Puerto_FIM*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport | RoleExchangePoint=https://\<*FQDN_Servidor_FIM*\>: \<*Puerto_SSL_FIM\>*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport |

Donde:

-   *\<Servidor_BHOLD\>* especifica el nombre del servidor BHOLD tal y como se encuentra en la versión original del archivo.

-   *\<Servidor_FIM\>* especifica el nombre del servidor FIM tal y como se encuentra en la versión original del archivo.

-   *\<FQDN_Servidor_BHOLD\>* especifica el nombre de dominio completo (FQDN) del servidor BHOLD.

-   *\<Puerto_FIM\>* especifica el número de puerto del servidor FIM tal y como se encuentra en la versión original del archivo.

-   *\<FQDN_Servidor_FIM\>* especifica el FQDN del servidor FIM.

-   *\<Puerto_SSL_FIM\>* especifica un puerto diferente para usarlo con SSL en el servidor FIM.

### <a name="enable-approval-workflows-in-bhold-core"></a>Habilitar flujos de trabajo de aprobación en BHOLD Core

Cuando FIM y BHOLD están integrados para autoservicio, los flujos de trabajo para aprobaciones se ejecutan en el servicio FIM. Esto es similar al modelo de flujo de trabajo para las solicitudes que se originan en el portal de FIM, como cuando un usuario envía una solicitud para unirse a una lista de distribución. Pero hay diferencias esenciales entre los flujos de trabajo del rol BHOLD y otros flujos de trabajo hospedados en el servicio FIM. En el caso de BHOLD, los parámetros de flujo de trabajo que especifican qué usuarios deben aprobar una solicitud de rol se originan en BHOLD, en lugar de almacenarse en las definiciones de flujo de trabajo de la base de datos del servicio FIM. BHOLD proporciona estos parámetros para el servicio FIM cuando se realiza la primera solicitud y un flujo de trabajo de acción comunica los resultados a BHOLD Core.

BHOLD selecciona un aprobador para una solicitud de autoservicio de una estas tres maneras:

-   **Administrador de línea como aprobador: selección basada en roles para una unidad organizativa** Si un rol tiene un atributo denominado roletype que se establece en Aprobador o Escalador, y si ese rol está vinculado a uno o más usuarios en el contexto de una unidad organizativa, las solicitudes de los usuarios dentro de esa unidad organizativa deben estar aprobadas por uno de los usuarios que están vinculados al rol con el roletype Aprobador o Escalador.

-   **Administrador de línea como aprobador: selección basada en atributos para una unidad organizativa** Cada unidad organizativa puede tener uno o varios atributos que especifican el alias de los usuarios que pueden aprobar asignaciones de roles para otros usuarios en la unidad organizativa. Estos atributos se nombran como sigue: approver1, approver2, etc. Cuando un usuario de la unidad organizativa solicita una asignación de rol, BHOLD enruta la solicitud (a través de FIM) a los usuarios especificados por los atributos del aprobador de la unidad organizativa. Si una unidad organizativa no tiene establecido alguno de estos atributos, BHOLD comprueba las unidades organizativas primarias hasta la unidad organizativa raíz.

-   **Administrador de roles como aprobador: selección basada en atributos para un rol** Un rol puede tener uno o varios atributos (que también se nombran como approver1, etc.) que especifican el alias de los usuarios que pueden aprobar la asignación del rol. Cuando un usuario solicita que se le asigne un rol que tiene establecidos estos atributos de aprobador, BHOLD enruta la solicitud a los usuarios especificados por los atributos.

Si no se especifica un aprobador para una solicitud de rol de autoservicio siguiendo uno de estos métodos, BHOLD asigna el rol automáticamente sin necesidad de aprobación. Por esta razón, inmediatamente después de instalar BHOLD FIM Integration, debe configurar la unidad organizativa raíz con el alias de un aprobador como la cuenta raíz. Esto impedirá que se conceda un rol a un usuario accidentalmente antes de poder implementar una directiva de aprobación más completa.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Para configurar un aprobador para la unidad organizativa raíz

1.  Inicie sesión en el servidor de BHOLD Core como administrador.

2.  Haga clic en **Inicio** y elija **Internet Explorer**.

3.  En la barra de direcciones de Internet Explorer, escriba <http://localhost:5151/bhold/core> y presione Entrar.

4.  En la página de inicio de BHOLD Core, en **Attribute def** (Definición de atributos), haga clic en **Tipos de atributos**.

5.  En la página **Tipo de atributo**, haga clic en **Agregar**.

6.  En la página **Add attribute type** (Agregar tipo de atributo), en **Identidad**, escriba approver1. En la lista **Tipo de datos**, haga clic en **Alfanumérico**. En **Longitud máxima**, escriba 255 y en **Lista de valores**, haga clic en **No**. En el tipo **Inglés**, escriba approver1, haga clic en **Aceptar** y luego en **Listo**.

7.  En el panel izquierdo, bajo **Attribute def** (Definición de atributos), haga clic en **Attribute type sets** (Conjuntos de tipos de atributos).

8.  En la página **Attribute type sets** (Conjuntos de tipos de atributos), haga clic en **Agregar**.

9.  En la página **Add attribute type set** (Agregar conjunto de tipos de atributos), en **Descripción**, escriba OrgUnit Attributes (Atributos de unidad organizativa) y haga clic en **Aceptar**.

10. En la página **OrgUnit Attributes** (Atributos de unidad organizativa), expanda **Tipos de atributos** y haga clic en **Modificar**.

11. En la lista **Tipo de atributo**, haga clic en **approver1**, haga clic en **Agregar** y luego en **Listo**.

12. En el panel izquierdo, haga clic en **Tipos de objeto**.

13. En la página **Tipos de objeto**, haga clic en **OrgUnit** (Unidad organizativa).

14. En la página **Object type/OrgUnit** (Tipo de objeto/Unidad organizativa), expanda **Attribute type sets** (Conjuntos de tipos de atributos) y haga clic en **Modificar**.

15. En la página **Link attribute type set/OrgUnit** (Vincular conjunto de tipos de atributos/unidad organizativa) en **Orden**, escriba 10. En la lista **Attribute type set** (Conjunto de tipos de atributos), haga clic en **OrgUnit Attributes** (Atributos de unidad organizativa), haga clic en **Agregar** y luego en **Listo**.

16. En el panel izquierdo, en **Modelo**, haga clic en **Unidades organizativas**.

17. En la página **Unidades organizativas**, haga clic en **raíz**.

18. En la página **Organizational unit/root** (Unidad organizativa/raíz), haga clic en **Modificar**.

19. En la página **Modify organizational unit attributes/root** (Modificar atributos de unidad organizativa/raíz), en **Aprobador**, escriba el dominio y el nombre de usuario del usuario que aprobará las solicitudes de asignación de roles, en el formato *\<dominio\>*\\*\<usuario\>*, donde *\<dominio\>* es el nombre de dominio NetBIOS (corto) y *\<usuario\>* es el nombre de inicio de sesión del usuario.
20. Haga clic en **Aceptar**.

>[!IMPORTANT]
El nombre de dominio y el usuario deben coincidir con el alias predeterminado de un usuario en la base de datos de BHOLD Core.

En lugar de especificar un aprobador para las unidades organizativas, puede especificar un aprobador para roles propuestos en la base de datos de BHOLD Core. Para hacerlo, cree el atributo approver1, agréguelo a un conjunto de tipos de atributos asociado con el tipo de objeto Rol, y modifique cada rol propuesto para especificar el aprobador.

Para proporcionar mayor seguridad en el flujo de trabajo, además de los aprobadores deberá designar usuarios y otros modos de aprobación. Para hacerlo, cree y rellene los siguientes atributos para las unidades organizativas y los roles:

- escalator*\<n\>*

- owner*\<n\>*

- securityOfficer*\<n\>*

- notification*\<n\>*

donde *\<n\>* indica un sufijo numérico opcional para proporcionar varios atributos del mismo tipo.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Comprobación de flujos de trabajo de aprobación configurados en el servicio FIM

La instalación de BHOLD FIM Integration crea conjuntos, definiciones de flujo de trabajo y reglas de directiva de administración (MPR) para el servicio FIM. Si había personalizado la implementación de FIM para cambiar los conjuntos de administradores o los conjuntos de usuarios que pueden realizar solicitudes, debe asegurarse de que las MPR hacen referencia a los conjuntos de usuarios correctos.

>[!NOTE]
Antes de que los usuarios del portal de FIM puedan usar las características de autoservicio proporcionadas por BHOLD, deben sincronizarse las cuentas de los usuarios en la base de datos de BHOLD desde el servicio de sincronización de FIM. En concreto, debe haber un registro de usuario en la base de datos de BHOLD Core y en la base de datos del servicio FIM para cada usuario que pueda realizar una solicitud de autoservicio o esté especificado como aprobador o escalador para solicitudes de autoservicio.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre cómo instalar el portal de FIM y otras características de FIM, vea [Planning and Architecture](https://technet.microsoft.com/library/ee808044.aspx) (Planeación y arquitectura) en la biblioteca técnica de Microsoft Forefront.
- [Guía de instalación de BHOLD](bhold-installation-guide.md)
- [Referencia para desarrolladores de BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historial de versiones de BHOLD](../reference/version-bhold-history.md)
