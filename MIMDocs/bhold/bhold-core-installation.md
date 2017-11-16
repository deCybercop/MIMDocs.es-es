---
title: "Instalación de BHOLD Core | Microsoft Docs"
description: "Documento principal de instalación de BHOLD Suite"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 33fbe63528d5d7c543ae286f934654538782b4d5
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-core-installation"></a>Instalación de BHOLD Core

El módulo BHOLD Core ofrece las principales características de BHOLD Suite en su entorno. Primero debe instalar y configurar el módulo BHOLD Core en un servidor en la red de área local para poder instalar otros módulos de BHOLD Suite.

## <a name="bhold-core-installation-requirements"></a>Requisitos de instalación de BHOLD Core

El módulo BHOLD Core constituye la base de Microsoft BHOLD Suite. Primero debe instalar el módulo BHOLD Core para poder instalar los demás módulos de Microsoft BHOLD Suite.

### <a name="bhold-core-hardware-requirements"></a>Requisitos de hardware de BHOLD Core

El módulo BHOLD Core constituye la base de Microsoft BHOLD Suite. Primero debe instalar el módulo BHOLD Core para poder instalar los demás módulos de Microsoft BHOLD Suite.

|          |        |          |
|----------|--------|----------|
|**Componente** |**Mínimo** | **Recomendado** |
|Procesador | Procesador de 64 bits | Procesador de 64 bits de varios núcleos |
| Memory |3 GB | 6 GB o más |
|Almacenamiento| 30 GB disponible |Depende del tamaño de la implementación |
|Adaptador de red| Conexión de 100 Mbps al servidor Forefront Identity Manager (FIM) y SQL server | Conexión de 1 Gbps al servidor de FIM y SQL server|

Estas recomendaciones se basan en las implementaciones típicas y no tienen en cuenta otras aplicaciones que se ejecutan en el servidor. Es posible que tenga que usar componentes de mayor rendimiento según las particularidades de su entorno.

### <a name="bhold-core-software-requirements"></a>Requisitos de software de BHOLD Core

El módulo BHOLD Core debe instalarse en un equipo que cumpla los requisitos siguientes:

- El servidor debe ejecutar Windows Server 2012 (64 bits) o Windows Server 2016 
- El servidor debe ser miembro de un dominio de Active Directory Domain Services (AD DS). En entornos de prueba, el servidor puede ser un controlador de dominio de AD DS.
- La instalación de BHOLD Core debe realizarla un usuario que haya iniciado sesión con una cuenta en el mismo dominio que el servidor y que pertenezca al grupo Administradores del dominio en el dominio y al grupo Administradores en el servidor.
- Debe instalarse Microsoft Internet Information Services (IIS) con ASP.NET en el servidor. IIS debe configurarse con la autenticación de Windows habilitada. Si se va a instalar BHOLD Core en Windows Server 2012/2016, se debe instalar Compatibilidad con la administración de IIS 6. Si se va a instalar BHOLD Core en Windows Server 2012, deben instalarse las herramientas de scripting de IIS 6.
- Debe instalarse .NET 3.5 Framework.
  - Dado que BHOLD Core requiere .NET 3.5, no se puede instalar en Server Core.
- Otros módulos de BHOLD requieren Silverlight 4, por lo que se recomienda instalarlo antes de instalar BHOLD Core.
- Debe instalarse Microsoft SQL Server 2014 o Microsoft SQL Server 2016 en el servidor de BHOLD Core o en otro servidor de la LAN. 

Los equipos cliente de Windows deben ejecutar Microsoft Internet Explorer versión 6 o posterior, y Microsoft Silverlight 4 o posterior.

## <a name="before-you-begin"></a>Antes de comenzar

Para usar el módulo BHOLD Core se necesita una cuenta de usuario con la que se autenticará y autorizará el servicio de BHOLD Core en el servidor y otras entidades de red. En esta sección se explica cómo crear y configurar esa cuenta de usuario y se proporciona una hoja de cálculo previa a la instalación en la que podrá recopilar la información que necesita para realizar la instalación de BHOLD Core.

>{IMPORTANTE} Cuando instala BHOLD Core, puede usar una base de datos de SQL Server existente para que sea la base de datos de BHOLD Core, o puede permitir que el Asistente para la instalación de BHOLD Core cree automáticamente la base de datos de BHOLD Core. Si decide usar una base de datos existente, debe asegurarse de que ningún otro usuario pueda obtener acceso a la base de datos durante la instalación de BHOLD Core. Antes de instalar BHOLD Core, compruebe que los controles de acceso de la base de datos de SQL Server no permiten el acceso a nadie que no sea el usuario que está instalando BHOLD Core.

## <a name="required-user-and-group"></a>Usuario y grupo necesarios

El módulo BHOLD Core debe poder iniciar sesión en el dominio con una cuenta de usuario que esté dedicada a tal fin y que pertenezca a dos grupos de seguridad específicos, incluido uno que se haya creado específicamente para el módulo BHOLD Core. Para realizar este procedimiento es necesario pertenecer al grupo Administradores del dominio.
**Para crear y configurar el grupo de seguridad y de usuarios de BHOLD Core**

1.  En un controlador de dominio, haga clic en **Inicio**, elija **Herramientas administrativas** y haga clic en **Usuarios y equipos de Active Directory**.

2.  En el árbol de consola, expanda el dominio donde se va a crear la cuenta, haga clic con el botón derecho en **Usuarios**, elija **Nuevo** y luego haga clic en **Grupo**.

3.  En el cuadro de diálogo **New Object – Group** (Nuevo objeto – Grupo), en **Nombre de grupo**, escriba el nombre del grupo (el valor predeterminado para BHOLD es BHOLDApplicationGroup) y luego haga clic en **Aceptar**.

4.  Haga clic con el botón secundario en **Usuarios**, seleccione **Nuevo**y haga clic en **Usuario**.

5.  En **Nombre completo**, escriba un nombre que le ayude a identificar la cuenta, como por ejemplo, Cuenta de servicio de BHOLD Core.

6.  En **Nombre de inicio de sesión de usuario**, escriba el nombre de usuario de la cuenta de servicio de BHOLD Core (el valor predeterminado para BHOLD es b1user) y, después, haga clic en **Siguiente**.

7.  En **Contraseña** y **Confirmar contraseña**, escriba la contraseña de la cuenta de servicio.

8.  Desactive la casilla **El usuario debe cambiar la contraseña en el siguiente inicio de sesión**, active **El usuario no puede cambiar la contraseña** y **La contraseña nunca expira**, haga clic en **Siguiente** y, después, en **Finalizar**.

9.  En el panel de resultados de la consola, haga clic con el botón derecho en la cuenta de usuario y elija **Agregar a un grupo**.

10. En el cuadro de diálogo **Seleccionar grupos**, escriba el nombre para mostrar del grupo que creó anteriormente, escriba un punto y coma (;) y después escriba IIS_IUSRS.

11. Haga clic en **Comprobar nombres** y luego haga clic en **Aceptar**.  

El siguiente procedimiento debe realizarse en el equipo donde se instalará el módulo BHOLD Core. Para realizar este procedimiento, debe iniciar sesión como miembro del grupo Administradores del dominio.

## <a name="bhold-core-installation-worksheet"></a>Hoja de cálculo de instalación de BHOLD Core

Antes de empezar a instalar el módulo BHOLD Core, debe prepararse para proporcionar la información que el Asistente para la instalación de BHOLD Core le pide para completar la instalación. Esta hoja de cálculo le ayudará a anotar esta información y, así, tenerla preparada para cuando se le pida. Cada sección corresponde a una página del Asistente para la instalación de BHOLD Core.

### <a name="account-settings"></a>Configuración de la cuenta

| **Elemento**                                    | **Descripción**                                                                                                                                                                                                                                                                                             | **Valor**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Security Provider on Domain/Machine (Usar proveedor de seguridad en dominio/máquina)** | Cuando se selecciona, especifica que la seguridad de Active Directory Domain Services controlará el acceso a BHOLD Core.                                                                                                                                                                                                  | Active la casilla de verificación. **Importante:** se producirá un error en la instalación si no se activa esta casilla.                                                                 |
| **Dominio**                                  | Especifica el dominio que contiene el servidor BHOLD, la cuenta de servicio y el grupo de aplicaciones. **Importante:** especifique el nombre de dominio mediante el nombre de NetBIOS (corto), en lugar del nombre de dominio completo (FQDN). Por ejemplo, si el FQDN del dominio es fabrikam.com, especifique el nombre de dominio como CONTOSO. | Escriba aquí el nombre de dominio.                                                                                                                                        |
| **Grupo de aplicaciones**                       | Especifica el nombre del grupo de seguridad que creó anteriormente en [Required user and group](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx#rug) (Usuario y grupo necesarios).                                                                                                                                  | Escriba aquí el nombre del grupo.                                                                                                                                         |
| **Usuario del servicio**                            | Especifica el nombre de inicio de sesión de la cuenta de usuario del servicio que creó anteriormente en [Required user and group](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx#rug) (Usuario y grupo necesarios).                                                                                                                      | Escriba aquí el nombre de la cuenta de usuario.                                                                                                                                  |
| **Contraseña**                                | Especifica la contraseña de la cuenta de usuario del servicio de BHOLD Core.                                                                                                                                                                                                                                              | Escriba aquí la contraseña. **Importante:** no olvide guardar esta contraseña en una ubicación segura y oculta.                                                                |
| **Website IP/Port (Puerto/Dirección IP del sitio web)**                         | Especifica el número de puerto y la dirección IP del sitio web que se va a crear en el servidor de la intranet. Cambie el valor predeterminado (\*) solamente si no va a usar la misma dirección IP que el sitio web predeterminado. Cambie el número de puerto a un puerto disponible solamente si el puerto predeterminado (5151) ya se está usando.             | Si el sitio web predeterminado usa una dirección IP no predeterminada, escríbala aquí. Si el número de puerto predeterminado ya se está usando, escriba aquí el número de puerto del sitio web de BHOLD. |

### <a name="database-settings"></a>Configuración de la base de datos

| **Elemento**                                       | **Descripción**                                                                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar seguridad integrada**                    | Especifica que se usa la autenticación de Windows para obtener acceso a la base de datos.                                                                                                                                                                                                     | Active esta casilla si se usa la autenticación de Windows para conectarse a SQL Server. Desactive esta casilla si se usa la autenticación de SQL Server. Si se usa la autenticación de SQL Server, debe crearse la base de datos antes de ejecutar la instalación de BHOLD Core. **Nota:** si se usa la autenticación de Windows, debe haber iniciado sesión con una cuenta que tenga el rol de servidor sysadmin en el servidor de base de datos. |
| **Usuario de la base de datos** y **Contraseña de la base de datos** | Especifica el nombre de usuario y la contraseña de un usuario con el rol de servidor sysadmin en el servidor de base de datos. Estos valores se proporcionan únicamente cuando se usa la autenticación de SQL Server.                                                                                               | Escriba aquí el nombre de usuario de SQL Server. Escriba aquí la contraseña de usuario de SQL Server. **Nota:** no olvide guardar esta contraseña en una ubicación segura y oculta.                                                                                                                                                                                                                                                  |
| **Servidor de base de datos** y **Nombre de base de datos**   | Especifica el nombre de NetBIOS del servidor de base de datos y el nombre de la base de datos (valor predeterminado: b1) que creará el programa de instalación de BHOLD Core. Si no va a usar la instancia del servidor de base de datos predeterminada, especifique la instancia del servidor de base de datos con el formato *\<servidor\>*\\*\<instancia\>*. | Escriba aquí el nombre de servidor (o el servidor y la instancia). Escriba aquí el nombre de la base de datos.                                                                                                                                                                                                                                                                                                                   |
| **Make restrictions for the database user (Realizar restricciones para el usuario de la base de datos)**    | Obsoleto.                                                                                                                                                                                                                                                                 | No cambie el valor predeterminado.                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>Instalación de BHOLD Core

Para instalar el módulo BHOLD Core, inicie sesión como miembro del grupo Administradores del dominio, descargue el archivo siguiente y ejecútelo como administrador en el servidor en el que quiere instalar el módulo BHOLD Core: 

- BholdCore *\<versión\>*\_Release.msi

Reemplace *\<versión\>* con el número de la versión de BHOLD Core que va a instalar.

Para ejecutar el archivo de programa como administrador, haga clic con el botón derecho en el archivo y luego haga clic en **Ejecutar como administrador**.

### <a name="postinstallation-settings"></a>Configuración posterior a la instalación

Una vez terminada la instalación de BHOLD Core, debe configurar Firewall de Windows y cambiar la configuración avanzada del grupo de aplicaciones de BHOLD Core en Internet Information Services para completar la configuración de BHOLD Core. Si es necesario, también deberá cambiar los atributos del sistema de BHOLD para adaptarlos a sus necesidades.

#### <a name="configuring-windows-firewall"></a>Configuración de Firewall de Windows

Si los usuarios van a obtener acceso a BHOLD usando exploradores web en equipos remotos, debe configurar Firewall de Windows en el servidor de BHOLD Core para permitir las conexiones entrantes al puerto del sitio web que especificó cuando instaló BHOLD Core.

Para llevar a cabo este procedimiento, debe pertenecer al grupo de administradores del equipo local.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>Para permitir las conexiones entrantes al sitio web de BHOLD

1.  Haga clic en **Iniciar**, seleccione **Herramientas administrativas**, haga clic con el botón derecho en **Firewall de Windows con seguridad avanzada** y, después, elija **Ejecutar como administrador**.

2.  En el panel izquierdo, haga clic en **Reglas de entrada** y, en el panel derecho, haga clic en **Nueva regla**.

3.  En el Asistente para nueva regla de entrada, haga clic en **Puerto** y en **Siguiente**.

4.  Asegúrese de que **TCP** está seleccionado. En **Puertos locales específicos** escriba el número de puerto predeterminado de BHOLD Core (5151) o el número de puerto que especificó cuando instaló BHOLD Core y luego haga clic en **Siguiente**.

5.  Asegúrese de que esté seleccionada la opción **Permitir la conexión** y luego haga clic en **Siguiente**.

6.  En la página **Perfil**, desactive las casillas para las ubicaciones desde las que no quiera obtener acceso al sitio web de BHOLD y luego haga clic en **Siguiente**.

7.  En la página **Nombre**, escriba un nombre para la regla (por ejemplo, Permitir conexiones de entrada a BHOLD Core) y luego haga clic en **Finalizar**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>Habilitar aplicaciones de 32 bits para el grupo de aplicaciones de BHOLD Core

Para permitir que IIS funcione correctamente con el módulo BHOLD Core, debe configurar el grupo de aplicaciones de BHOLD Core para que admita aplicaciones de 32 bits. Para realizar este procedimiento, debe iniciar sesión con la cuenta que usó para instalar el módulo BHOLD Core.

**Para habilitar la compatibilidad con aplicaciones de 32 bits para el grupo de aplicaciones de BHOLD Core**

1.  Abra el Administrador de IIS haciendo clic en **Inicio**, **Herramientas administrativas** y **Administrador de Internet Information Services (IIS)**.

2.  En el árbol de consola, expanda el nombre de servidor y haga clic en **Grupos de aplicaciones**.

3.  En la lista **Grupos de aplicaciones**, haga clic con el botón derecho en **CoreAppPool** y luego en **Configuración avanzada**.

4.  En el cuadro de diálogo **Configuración avanzada**, en **Habilitar aplicaciones de 32 bits**, seleccione **True** y después haga clic en **Aceptar**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Establecer el nombre de entidad de seguridad de servicio para el sitio web BHOLD

Si el nombre de red que se usa para contactar con el sitio web de BHOLD no es el mismo que el nombre de host del servidor, debe establecer un nombre de entidad de seguridad de servicio (SPN) para HTTP. Por ejemplo, si usa un registro de recurso CNAME en DNS para especificar un alias para el servidor o si usa el equilibrio de carga de red, debe registrar estas direcciones de red adicionales en Active Directory. Si no lo hace, Internet Explorer no podrá usar el protocolo Kerberos al contactar con el sitio web de BHOLD.

>[!IMPORTANT]
Si el módulo BHOLD Core está instalado en el mismo equipo que el portal de FIM, debe crear registros de recursos de DNS (CNAME o A) con distintos nombres de host para los servidores que ejecutan BHOLD Core y el servidor que ejecuta el portal de FIM. Solo se puede establecer un SPN para un determinado par de tipo de servicio/alias de servidor, por lo que BHOLD Core y el portal de FIM requieren SPN independientes porque normalmente se ejecutan bajo cuentas distintas. El comando de setspn notifica un error si un SPN ya se ha establecido en otra cuenta.

Para completar este procedimiento, se requiere como mínimo pertenecer al grupo **Administradores del dominio**.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Para establecer el SPN del sitio web de BHOLD

1.  En el controlador de dominio de Active Directory Domain Services, haga clic en **Inicio**, **Todos los programas** y **Accesorios**. Haga clic con el botón derecho en **Símbolo del sistema** y elija **Ejecutar como administrador**.

2.  En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR: setspn –S HTTP/ *\<aliasDeRed\> \<dominio\>* \\ *\<nombreDeCuenta\>*, donde:

    -   *\<aliasDeRed\>* es la dirección que los clientes usan para contactar con el sitio web de BHOLD

    -   *\<dominio\>*\\*\<nombreDeCuenta\>* es el dominio y el nombre de usuario de la cuenta de servicio de BHOLD Core que creó al instalar BHOLD Core.

3.  Repita el paso anterior para el resto de nombres que los clientes usan para contactar con el sitio web de BHOLD, como por ejemplo, alias CNAME, nombres que incluyan un nombre de dominio completo o nombres que incluyan un nombre de dominio (corto) de NetBIOS.

#### <a name="setting-bhold-system-attributes"></a>Configurar atributos del sistema de BHOLD

Para confirmar que la instalación del módulo BHOLD Core se realizó correctamente, abra el portal de BHOLD Core y vea los atributos del sistema. Además, para asegurarse de que el módulo BHOLD Core funciona correctamente en el entorno, puede modificar los siguientes atributos del sistema de BHOLD, según corresponda:

| **Atributo**                | **Descripción**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NoHistory**                | Si el sitio web de BHOLD se ejecuta en un servicio web en clúster, establezca este atributo en Y para asegurarse de que los elementos mostrados recientemente funcionen correctamente. Se establece en N si el sitio web de BHOLD se ejecuta en un servidor IIS independiente.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Establezca este atributo en Y para asegurarse de que las unidades organizativas se puedan mover únicamente a otras unidades organizativas con el mismo tipo de organización que la unidad organizativa primaria. Por ejemplo, esto impide que la unidad organizativa de un proyecto se mueva a la unidad organizativa de un departamento. Se establece en N para permitir que una unidad organizativa se ubique en otra unidad organizativa de distinto tipo. |
| **DaysbetweenABArun**     | Indique un entero de dos dígitos para especificar el intervalo (en días) entre dos ejecuciones de autorización basada en atributos (ABA). Por ejemplo, para especificar que las ejecuciones de ABA estén separadas por dos días, escriba 02.                                                                                                                     |
| **StarthourofABArun**    | Indique un entero de dos dígitos para especificar la hora del día en la que se producirá una ejecución de ABA. Por ejemplo, para especificar que la ejecución de ABA tenga lugar a las 11:00 p.m. (23:00), escriba 23.                                                                                                             |
| **SystemCardinality**       | Establezca este atributo en N si no quiere que se realice una comprobación de cardinalidad del sistema en BHOLD. El valor predeterminado es Y.                                                                                                                                                                                                                             |
| **Logging**                  | Establezca este atributo en N si no quiere que se registren los cambios. El valor predeterminado es Y.                                                                                                                                                                                                                                            |
| **SystemQueueProcessing**   | Establezca este atributo en N si no quiere que se realice el procesamiento de cola del sistema. No cambie este valor a menos que el soporte técnico le indique que lo haga.                                                                                                                                                                                           |

Para realizar este procedimiento, debe iniciar sesión como miembro del grupo Administradores del dominio.

**Para establecer un atributo del sistema de BHOLD**

1.  Haga clic en **Inicio**, elija **Todos los programas** y haga clic en **Internet Explorer**.

2.  En el cuadro de dirección, escriba http://<servidor>:<puerto>/bhold/core, donde *\<servidor\>* es el nombre de servidor del sitio web de BHOLD y *\<puerto\>* es el número de puerto enlazado al sitio web.

3.  Haga clic en **inicio**, elija **Valores** y haga clic en **Modificar**.

4.  Busque el nombre del atributo que quiere cambiar, escriba el nuevo valor en el cuadro situado junto al nombre de atributo y haga clic en **Aceptar**.

## <a name="next-steps"></a>Pasos siguientes

Después de instalar BHOLD Core y confirmar que la instalación es correcta, ya puede instalar otros módulos. En este momento, la base de datos de BHOLD estará básicamente vacía y solo incluirá una cuenta de usuario (la cuenta raíz) y una unidad organizativa (la unidad organizativa raíz). Para agregar más usuarios a la base de datos de BHOLD, puede instalar el módulo Access Management Connector o el módulo BHOLD Model Generator, en función de sus necesidades. Puede usar el módulo Access Management Connector para importar datos de usuarios desde el servicio de sincronización de FIM, o puede usar BHOLD Model Generator para importar datos de usuarios desde un conjunto de archivos estructurados. Para más información sobre cómo usar Access Management Connector, vea [Test Lab Guide: BHOLD Access Management Connector](https://technet.microsoft.com/en-us/library/jj853085(v=ws.10).aspx) (Guía del laboratorio de pruebas: BHOLD Access Management Connector).

Para más información sobre cómo usar el módulo BHOLD Model Generator, vea:

- [Microsoft BHOLD Suite Concepts Guide](https://technet.microsoft.com/en-us/library/jj134102(v=ws.10).aspx) (Guía de conceptos de Microsoft BHOLD Suite)
- [Microsoft BHOLD Suite Technical Reference](https://technet.microsoft.com/en-us/library/jj134935(v=ws.10).aspx) (Referencia técnica de Microsoft BHOLD Suite).
