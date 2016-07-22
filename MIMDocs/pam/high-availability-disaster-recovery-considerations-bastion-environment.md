---
title: "Consideraciones de alta disponibilidad y recuperación ante desastres para el entorno bastión | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/17/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 03e521cd-cbf0-49f8-9797-dbc284c63018
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 9e5f51d5ca731b3564b8262db0f4cddeb850231a
ms.openlocfilehash: 1d9e005bfb3e26f9a2b818667f14acd3e5239523


---

# Consideraciones de alta disponibilidad y recuperación ante desastres para el entorno bastión
En este artículo se describen las consideraciones para la alta disponibilidad y recuperación ante desastres al implementar los Servicios de dominio de Active Directory (AD DS) y Microsoft Identity Manager 2016 (MIM) para Privileged Access Management (PAM).

Las empresas se centran en la alta disponibilidad y recuperación ante desastres en las cargas de trabajo de Windows Server, SQL Server y Active Directory. Pero la disponibilidad segura del entorno bastión para Privileged Access Management también es importante. El entorno bastión es una parte fundamental de la infraestructura de TI de la organización, ya que los usuarios interactúan con sus componentes para asumir roles administrativos. Para obtener más información sobre la alta disponibilidad en general, puede descargar las notas del producto [Información general de la alta disponibilidad de Microsoft](http://download.microsoft.com/download/3/B/5/3B51A025-7522-4686-AA16-8AE2E536034D/Microsoft%20High%20Availability%20Strategy%20White%20Paper.doc).

## Escenarios de alta disponibilidad y recuperación ante desastres

Cuando planee la alta disponibilidad y recuperación ante desastres tenga en cuenta las siguientes preguntas:

- ¿Qué funciones podrían verse afectadas por una interrupción?
- ¿Qué funciones son fundamentales para la empresa o para las operaciones de TI?
- ¿Cuáles son los riesgos que podrían provocar una interrupción en estos sistemas?

El ámbito de estas consideraciones afecta al costo total de la implementación y las operaciones, por lo que las organizaciones pueden priorizar determinadas funciones más que otras y aceptar también el riesgo de interrupciones temporales para las funciones de prioridad inferior. En la siguiente tabla se describe una clasificación potencial de prioridades que podría usar una organización:

| **Función del bosque bastión** | **Prioridad relativa durante la recuperación** | **Mitigación si la función no está disponible** |
| --------------------------- | --------------------- | -------------- |
| Establecimiento de confianza         | Bajo | Esperar hasta que se restaure el entorno bastión |
| Mitigación de usuario y de grupo   | Bajo | Esperar hasta que se restaure el entorno bastión |
| Administración de MIM          | Bajo | Esperar hasta que se restaure el entorno bastión |
| Activación de roles con privilegios  | Mediana | Cuentas dedicadas respaldadas en una tarjeta inteligente para agregar manualmente los usuarios a los grupos administrativos |
| Administración de recursos         | Alto | Cuentas dedicadas respaldadas en una tarjeta inteligente para agregar manualmente los usuarios a los grupos administrativos |
| Supervisión de usuarios y grupos en un bosque existente | Bajo | Esperar hasta que se restaure el entorno bastión |

Ahora vamos a echar un vistazo a cada una de estas funciones de bosque bastión.

### Establecimiento de confianza

Es necesario que exista una confianza de bosque entre los dominios del bosque existente y el bosque del entorno bastión. Este es el motivo por el que los usuarios que se autentican en el entorno bastión pueden administrar recursos en los bosques existentes. Puede ser necesaria una configuración adicional, por ejemplo, para permitir la migración de usuarios de dominios existentes en versiones anteriores de Windows Server.

El establecimiento de confianza requiere que los controladores de dominio del bosque existente estén en línea, así como los componentes de AD y MIM del entorno bastión.  Si existe una interrupción de cualquiera de estos durante el establecimiento de confianza, el administrador puede volver a intentarlo una vez que se haya tratado la interrupción.  En caso de que los controladores de dominio del bosque existente o el entorno bastión se hayan recuperado después de una interrupción, MIM también incluye los cmdlets de PowerShell `Test-PAMTrust` y `Test-PAMDomainConfiguration` que pueden usarse para comprobar que la confianza todavía está en vigor.

### Migración de usuario y de grupo

Una vez que se ha establecido la confianza, pueden crearse grupos de instantáneas en el entorno bastión, así como cuentas de usuario para los miembros de esos grupos y aprobadores. Esto habilita a esos usuarios a activar los roles con privilegios y a volver a obtener pertenencias a grupos vigentes.

La migración de usuario y de grupo requiere que los controladores de dominio del bosque existente estén en línea, así como los componentes de AD y MIM del entorno bastión.   Si los controladores de dominio del bosque existente no se encuentran accesibles, no pueden agregarse grupos ni usuarios adicionales al entorno bastión pero los grupos y usuarios existentes no se ven afectados. Si se produce una interrupción de cualquiera de los componentes durante la migración, el administrador puede volver a intentarlo una vez que se haya tratado la interrupción.

### Administración de MIM
Una vez que los usuarios y los grupos se hayan migrado, el administrador puede seguir configurando en MIM las asignaciones de roles que vinculan a los usuarios como candidatos para la activación de los roles.  También pueden configurar las directivas de MIM para su aprobación y Azure MFA.  

La administración de MIM requiere que los componentes de MIM y AD del entorno bastión estén en línea.

### Activación de roles con privilegios
Cuando un usuario quiere activar el rol con privilegios, deben autenticarse en el dominio del entorno bastión y enviar una solicitud a MIM.  MIM incluye SOAP y las API de REST así como interfaces de usuario en PowerShell y en una página web.

La activación de roles con privilegios requiere que los componentes de MIM y AD del entorno bastión estén en línea.  Además, si MIM está configurado para usar [Azure MFA para la activación](use-azure-mfa-for-activation.md) del rol seleccionado, el acceso a Internet es necesario para ponerse en contacto con el servicio de Azure MFA.

### Administración de recursos
Cuando un usuario se ha activado correctamente en el rol, el controlador de dominio puede generar un vale Kerberos para este que puede usarse por los controladores de dominio de los dominios existentes y que reconocerá las nuevas pertenencias a grupos temporales del usuario.

La administración de recursos requiere que un controlador de dominio para el dominio de recursos se encuentre en línea, así como el controlador de dominio del entorno bastión.  Una vez que el usuario se ha activado, emitir el vale Kerberos no requiere que MIM o SQL se encuentren en línea en el entorno bastión.  (Tenga en cuenta que con Windows Server 2012 R2 como nivel funcional del entorno bastión, MIM necesita estar en línea para finalizar la pertenencia a grupos temporales)

### Supervisión de usuarios y grupos en el bosque existente
MIM también incluye un servicio de supervisión de PAM que comprueba regularmente los usuarios y los grupos en los dominios existentes y actualiza la base de datos de MIM y AD como corresponde.  Este servicio no necesita estar en línea para la activación de roles ni durante la administración de recursos.

La supervisión requiere que los controladores de dominio del bosque existente estén en línea, así como los componentes de AD y MIM del entorno bastión.  

## Opciones de implementación
La [información general del entorno](environment-overview.md) ilustra una topología básica adecuada para conocer la tecnología que no está diseñada para la alta disponibilidad. En esta sección se describe cómo ampliar esa topología para proporcionar una alta disponibilidad para las organizaciones con un solo sitio así como aquellos con varios sitios existentes.

### Redes

El tráfico de red entre los equipos del entorno bastión debería estar aislado de las redes existentes, por ejemplo, mediante el uso de una red virtual o física diferente.  Dependiendo de los riesgos en el entorno bastión, también puede ser necesario tener interconexiones físicas independientes entre los equipos.  Determinadas tecnologías de clústeres de conmutación por error tienen requisitos adicionales en las interfaces de red.

Los equipos que hospedan los Servicios de dominio de Active Directory y los que hospedan los servicios MIM en el entorno bastión requieren una conectividad bidireccional para los recursos del bosque existente para:
- los usuarios que se autentiquen mediante los controladores de dominio del bosque PRIV
- los usuarios que soliciten la activación
- los usuarios que tengan vales Kerberos consumibles por recursos del bosque existente
- MIM para supervisar los dominios de bosque existentes
- MIM para enviar correos electrónicos mediante los servidores de correo ubicados en el bosque existente.

### Topologías mínimas de alta disponibilidad
Una organización puede seleccionar qué funciones de su entorno bastión requieren alta disponibilidad, con las siguientes restricciones:

- La alta disponibilidad para cualquier función que proporcione el entorno bastión requiere al menos dos controladores de dominio.  
- La alta disponibilidad para las solicitudes de activación requiere al menos dos equipos que hospeden el servicio MIM y también requiere una alta disponibilidad para SQL Server.
- La alta disponibilidad de SQL Server con clústeres de conmutación por error requiere al menos dos servidores que proporcionen SQL Server y estos no pueden ser los mismos que los del controlador de dominio.
- El servicio MIM no debería instalarse en el controlador de dominio para minimizar la superficie expuesta a ataques de cada servidor.

La topología de alta disponibilidad más pequeña para todas las funciones del entorno bastión comprende al menos cuatro servidores y almacenamiento compartido. Dos de los servidores deben configurarse como controladores de dominio y proporcionar Servicios de dominio de Active Directory. Los otros dos servidores pueden configurarse como un clúster de conmutación por error que proporcione SQL Server y el servicio MIM.

Además, una implementación típica del entorno bastión también incluirá una estación de trabajo de administración con privilegios para la administración de estos servidores, así como un componente de supervisión

En el siguiente diagrama se ilustra una posible arquitectura:

![topología bastión: diagrama](media/bastion1.png)

Los servidores adicionales pueden configurarse para cada una de estas funciones para proporcionar un rendimiento más alto en condiciones de carga o para la redundancia geográfica como se describe a continuación.

### Implementaciones que admiten varios sitios
Elegir la topología de implementación adecuada para los recursos que se implementan en varios sitios depende de tres factores:  
- Objetivos y riesgos para la alta disponibilidad y recuperación ante desastres  
- La capacidad de hardware para hospedar el entorno bastión  
- El modelo de trabajo administrativo para cada sitio.

Uno de los enfoques más sencillos sería hospedar el entorno bastión como un sitio en particular.  En condiciones normales, los usuarios se conectarán a la implementación de MIM en el entorno bastión de ese sitio y solicitarán la activación. Las activaciones se efectuarán en los recursos de cada sitio.  En caso de que el vínculo de red no funcione o que el sitio que hospeda el entorno bastión no esté disponible, se puede tener acceso a las credenciales sin conexión en otro sitio para realizar una administración temporal hasta que la red vuelva a conectarse.  Este enfoque puede ser adecuado en situaciones en las que se prevé que la administración local de un sitio en particular, como una sucursal, sea poco frecuente y se limite a volver a conectar ese sitio al resto de la red de la organización.

![Bastión único para una topología de varios sitios: diagrama](media/bastion2.png)

Para obtener una alta disponibilidad y recuperación ante desastres en los sitios, también es posible implementar los componentes del entorno bastión en cada sitio al compartir un directorio PRIV común y una Base de datos SQL común.  En esta topología, si el vínculo de red no funciona, los usuarios de cada sitio pueden seguir trabajando de manera independiente.

![Varios bastiones para una topología de varios sitios: diagrama](media/bastion3.png)

Una restricción de este enfoque de implementación es que SQL Server requiere un clúster que comprenda ambos sitios, lo que puede ser difícil de implementar. En esa situación, considere solo la replicación de Active Directory (bosque PRIV) del entorno bastión como alternativa.  En caso de que exista una interrupción de red entre los sitios, los usuarios del sitio B que hayan activado previamente sus roles con privilegios podrán seguir trabajando para administrar los recursos en el sitio B.

![Bastión replicado para una topología de varios sitios: diagrama](media/bastion4.png)

Si cada sitio representa un límite administrativo independiente, también es posible implementar varios entornos bastión independientes.  Aunque cada entorno bastión tendrá el mismo software, los nombres de dominio de cada uno serán diferentes y no habrá coincidencia entre los directorios y las bases de datos de cada entorno bastión. Un usuario que quiera administrar recursos en un sitio en particular activará una cuenta de usuario en el entorno bastión de ese sitio.

![Bastiones independientes para una topología de varios sitios: diagrama](media/bastion5.png)

Por último, son posibles implementaciones más complejas como entornos de varios bastiones que pueden configurarse de manera independiente para administrar recursos en un dominio determinado.

![Bastión complejo para una topología de varios sitios: diagrama](media/bastion6.png)

### Entorno bastión hospedado
Algunas organizaciones también han considerado el establecimiento de un entorno bastión independiente de cualquiera de sus sitios existentes. El software del entorno bastión puede hospedarse en una plataforma de virtualización dentro de las redes de la organización o en un proveedor de host externo.  Al evaluar este enfoque, tenga en cuenta que:

- Para protegerse de los ataques que se originan en los dominios existentes, la administración del entorno bastión debe estar aislada de las cuentas administrativas del dominio existente.
- El entorno bastión requiere una conectividad TCP/IP para los controladores de dominio del dominio existente.  Puede encontrarse una lista de puertos en [How to configure a firewall for domains and trusts (Cómo configurar un firewall para dominios y confianzas)](https://support.microsoft.com/kb/179442).
- Una implementación virtualizada de los Servicios de dominio de Active Directory requiere características específicas de la plataforma de virtualización, como se describe en [Implementación y configuración de controladores de dominio virtualizados](https://technet.microsoft.com/library/jj574223.aspx).
- Una implementación de alta disponibilidad de SQL Server para el servicio MIM requiere una configuración de almacenamiento especializada, como se describe en la sección siguiente [Almacenamiento de Base de datos SQL Server](#sql-server-database-storage).  No todos los proveedores de host pueden ofrecer actualmente hospedaje de Windows Server con configuraciones de disco adecuadas para los clústeres de conmutación por error de SQL Server.

## Preparación de la implementación y procedimientos de recuperación
Preparar una implementación del entorno bastión que esté lista para la alta disponibilidad y la recuperación ante desastres requiere considerar cómo instalar Windows Server Active Directory, SQL Server y su base de datos en un almacenamiento compartido así como el servicio MIM y sus componentes de PAM.

### Windows Server
Windows Server contiene una característica integrada de alta disponibilidad, lo que permite que varios equipos trabajen juntos como un clúster de conmutación por error. Los servidores en clúster están conectados con cables físicos y con software. Si se produce un error en uno o más de los nodos del clúster, otro nodo comienza a dar servicio (proceso que se denomina conmutación por error).   Puede encontrarse más información en [Información general sobre clústeres de conmutación por error](https://technet.microsoft.com/library/hh831579.aspx).

Asegúrese de que el sistema operativo y las aplicaciones del entorno bastión reciben actualizaciones para los problemas de seguridad. Algunas de estas actualizaciones pueden requerir el reinicio del servidor, por lo que coordine las horas en las que se aplican las actualizaciones en los servidores para evitar interrupciones prolongadas. Un enfoque consiste en usar la [actualización compatible con clústeres](https://technet.microsoft.com/library/hh831694.aspx) para los servidores de un clúster de conmutación por error de Windows Server.

Los servidores del entorno bastión se unirán al dominio y dependerán de los servicios de dominio. Asegúrese de que no se configuran accidentalmente con una dependencia en un controlador de dominio determinado para servicios como DNS.

### Entorno bastión de Active Directory
Los Servicios de dominio de Windows Server Active Directory incluyen de forma nativa compatibilidad para la alta disponibilidad y recuperación ante desastres.

#### Preparación
Una implementación de producción típica de Privileged Access Management incluye al menos dos controladores de dominio en el entorno bastión. Las instrucciones para la configuración del primer controlador de dominio en el entorno bastión se incluyen en el paso 2 de los artículos de implementación [Prepare the PRIV domain controller (Preparar el controlador de dominio PRIV)](step-2-prepare-priv-domain-controller.md).

El procedimiento para agregar un controlador de dominio adicional puede encontrarse en [Instalar una réplica del controlador de dominio de Windows Server 2012 en un dominio existente (nivel 200)](https://technet.microsoft.com/library/jj574134.aspx).  

>[!NOTE] 
> Si el controlador de dominio va a hospedarse en una plataforma de virtualización como Hyper-V, revise las advertencias en [Implementación y configuración de controladores de dominio virtualizados](https://technet.microsoft.com/library/jj574223.aspx).

#### Recuperación
Después de una interrupción, asegúrese de que al menos un controlador de dominio está disponible en el entorno bastión antes de reiniciar los otros servidores.

Dentro de un dominio, Active Directory distribuye los roles de Operaciones de maestro único flexible (FSMO) en los controladores de dominio, como se describe en [How Operations Masters Work (Cómo funcionan los maestros de operaciones)](https://technet.microsoft.com/library/cc780487.aspx).  Si se ha producido un error en un controlador de dominio, puede que sea necesario transferir uno o más [Roles de controlador de dominio](https://technet.microsoft.com/library/cc786438.aspx) que ese controlador de dominio tiene asignados.

Después de determinar que un controlador de dominio no volverá a la producción, asegúrese de comprobar si algún rol estaba asignado a ese controlador de dominio y vuelva a asignarlo en caso necesario. Las instrucciones pueden encontrarse en [View the Current Operations Master Role Holders (Ver los contenedores de roles del maestro de operaciones actual)](https://technet.microsoft.com/library/cc816893.aspx) y en los artículos relacionados.

También se recomienda comprobar la configuración DNS de los equipos unidos al entorno bastión, así como los controladores de dominio de los dominios de CORP que tengan una relación de confianza con ese controlador de dominio, para asegurarse de que ninguno está codificado de forma rígida con una dependencia en la dirección IP del equipo de ese controlador de dominio.

### Almacenamiento de Base de datos SQL Server
Una implementación de alta disponibilidad requiere clústeres de conmutación por error e instancias de clúster de conmutación por error de SQL Server que respondan al almacenamiento compartido entre todos los nodos para el almacenamiento del registro y de la base de datos. El almacenamiento compartido puede encontrarse en forma de discos de clúster de los clústeres de conmutación por error de Windows Server, en forma de discos en una red de área de almacenamiento (SAN) o como recursos compartidos de archivos en un servidor de SMB.  Tenga en cuenta que deben dedicarse al entorno bastión; el almacenamiento de uso compartido con otras cargas de trabajo fuera del entorno bastión no está recomendado, ya que podría poner en peligro la integridad de este.

### SQL Server
El servicio MIM requiere una implementación de SQL Server en el entorno bastión.   Para la alta disponibilidad, SQL puede implementarse mediante una instancia de clúster de conmutación por error (FCI). A diferencia de las instancias independientes, en las FCI la alta disponibilidad de SQL Server está protegida mediante la presencia de nodos redundantes en la FCI. En caso de un error o de una actualización planeada, la pertenencia al grupo de recursos se mueve a otro nodo de clúster de conmutación por error de Windows Server.

Si solo necesita compatibilidad para la recuperación ante desastres pero no para la alta disponibilidad, entonces puede usar el trasvase de registros, la replicación de transacciones, la replicación de instantáneas o la creación de reflejo de la base de datos en lugar de los clústeres de conmutación por error.   

#### Preparación
Al instalar SQL Server en el entorno bastión, este debe ser independiente de cualquier SQL Server que ya se encuentre en los bosques de CORP.  Además, se recomienda que SQL Server se implemente en un servidor dedicado, diferente del servidor del controlador de dominio.
Puede encontrarse más información en la guía de SQL Server en [Instancias de clúster de conmutación por error de AlwaysOn](https://msdn.microsoft.com/library/ms189134.aspx).

#### Recuperación
Si se ha configurado SQL Server para la recuperación ante desastres mediante el trasvase de registros, deben adoptarse medidas para actualizar SQL Server durante la recuperación.  Además, se necesita reiniciar cada instancia del servicio MIM.

Si se ha producido un error en SQL Server o la conectividad entre SQL Server y el servicio MIM se ha perdido, después de que SQL Server se restaure es recomendable reiniciar cada servicio MIM.  Esto asegurará que el servicio MIM vuelva a establecer su conexión a SQL Server.

### Servicio MIM
El servicio MIM se requiere para procesar las solicitudes de activación.  Para que un equipo que hospeda el servicio MIM pueda desactivarse por razones de mantenimiento mientras las solicitudes de activación todavía se están recibiendo, pueden implementarse varios equipos de servicio MIM.  Tenga en cuenta que el servicio MIM no está implicado en las operaciones de Kerberos una vez que el usuario se ha agregado a un grupo.  

#### Preparación
Se recomienda implementar el servicio MIM en varios servidores unidos al dominio PRIV.
Para la alta disponibilidad, vea los documentos de Windows Server de [Requisitos de hardware de los clústeres de conmutación por error y opciones de almacenamiento](https://technet.microsoft.com/library/jj612869.aspx) y [Creating a Windows Server 2012 Failover Cluster (Crear un clúster de conmutación por error en Windows Server 2012)](http://blogs.msdn.com/b/clustering/archive/2012/05/01/10299698.aspx).

Para la implementación de producción en varios servidores, puede usar Equilibrio de carga de red (NLB) para distribuir la carga de procesamiento.  También debería tener un único alias (por ejemplo, registros A o CNAME) de forma que se exponga un nombre común al usuario.

>[!IMPORTANT] 
> Si usa una tecnología de equilibrio de carga que no sea la característica NLB en Windows Server 2012 R2, asegúrese de que su solución redirigirá una sesión al mismo servidor y no a un servidor aleatorio.

En una implementación MIM de varios servidores, cada servicio MIM tiene un nombre de host externo, un nombre de servicio y un nombre de partición del servicio.  El valor predeterminado del nombre de servicio es el nombre del equipo y el valor predeterminado del nombre de host externo y del nombre de partición del servicio se configuran durante la instalación del servicio MIM en la pantalla que solicita la dirección del servidor del servicio MIM. Estos tres nombres se almacenan en el archivo %ProgramFiles%\Microsoft Forefront Identity Manager\Service\Microsoft.ResourceManagementService.exe.config como atributos `externalHostName`, `serviceName` y `servicePartitionName` del nodo de configuración de `resourceManagementService`.  

Cuando un servicio MIM recibe una solicitud, el nombre de partición del servicio se almacena como un atributo en esa solicitud.   Posteriormente, solo se permite que otras instalaciones del servicio MIM que tengan el mismo nombre de partición del servicio interactúen con esa solicitud.  Como resultado, si el escenario de PAM incluye aprobaciones manuales u otro procesamiento de solicitud de larga duración, asegúrese de que cada servicio MIM tenga el mismo atributo `servicePartitionName` en ese archivo de configuración.

#### Recuperación
Después de una interrupción, asegúrese de que al menos un controlador de dominio de Active Directory y SQL Server están disponibles en el entorno bastión antes de reiniciar el servicio MIM.  

Una instancia de flujo de trabajo solo puede completarse mediante un servidor del servicio MIM que tenga el mismo nombre de partición del servicio y el mismo nombre de servicio que el servidor del servicio MIM que la ha iniciado.  Si un equipo determinado produce un error al hospedar un servicio MIM que estaba procesando solicitudes, y ese equipo no volverá al servicio, será necesario instalar el servicio MIM en un equipo nuevo. En el nuevo servicio MIM después de la instalación, edite el archivo *resourcemanagementservice.exe.config* y establezca los atributos `serviceName` y `servicePartitionName` de la nueva implementación MIM para que sean los mismos que el nombre de host y el nombre de partición del servicio del equipo que ha producido el error.

### Componentes MIM PAM
El servicio MIM y el instalador del portal también incorporan componentes de PAM adicionales, incluidos módulos de PowerShell y dos servicios.

#### Preparación
Los componentes de Privileged Access Management deben instalarse en cada equipo en el entorno bastión donde se instala el servicio MIM.  No se pueden agregar posteriormente.

#### Recuperación
Tras la recuperación de una interrupción, asegúrese de que el servicio MIM está ejecutándose en al menos un servidor.  Después, asegúrese de que el servicio de supervisión MIM PAM también está ejecutándose en ese servidor mediante `net start "PAM Monitoring service"`.

Si el nivel funcional del bosque del entorno bastión es Windows Server 2012 R2, asegúrese de que el servicio de componentes MIM PAM también está ejecutándose en ese servidor mediante el comando `net start "PAM Component service"`.



<!--HONumber=Jun16_HO5-->


