---
title: Procedimientos recomendados de Microsoft Identity Manager 2016 | Microsoft Docs
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 05/11/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: a0d00c7e5d99e43d3fb0b3011a3851f7194bfdf2
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="microsoft-identity-manager-2016-best-practices"></a>Procedimientos recomendados de Microsoft Identity Manager 2016

En este tema se describen los procedimientos recomendados para implementar y trabajar con Microsoft Identity Manager 2016 (MIM).

## <a name="sql-setup"></a>Configuración SQL
>[!NOTE]
En las siguientes recomendaciones para configurar un servidor que ejecuta SQL se presupone la existencia de una instancia de SQL dedicada a FIMService y una instancia de SQL dedicada a la base de datos FIMSynchronizationService. Si está ejecutando FIMService en un entorno consolidado, tendrá que realizar los ajustes adecuados para su configuración.

La configuración del servidor de lenguaje de consulta estructurado (SQL) es fundamental para obtener un rendimiento óptimo del sistema. Obtener un rendimiento óptimo de MIM en implementaciones a gran escala depende de la aplicación de los procedimientos recomendados para un servidor que ejecuta SQL. Para obtener más información, vea los siguientes temas sobre los procedimientos recomendados de SQL:

-   [Los diez procedimientos principales de almacenamiento recomendados](http://go.microsoft.com/fwlink/?LinkID=183663)

-   [Optimizar el rendimiento de tempdb](http://go.microsoft.com/fwlink/?LinkID=188267)

-   [Artículo de procedimientos recomendados de SQL Server](http://go.microsoft.com/fwlink/?LinkID=188268)

-   [Reorganizar y volver a generar índices](http://go.microsoft.com/fwlink/?LinkID=188269)

### <a name="presize-data-and-log-files"></a>Predimensionar datos y archivos de registro

No se base en el crecimiento automático. En su lugar, administre el crecimiento de estos archivos manualmente. Puede dejar el crecimiento automático por razones de seguridad, pero debe administrar de manera proactiva el crecimiento de los archivos de datos. Para obtener tamaños de ejemplo de la base de datos de MIM, vea la [Guía de planeación de capacidades de FIM](http://go.microsoft.com/fwlink/?LinkID=185246).

### <a name="to-presize-sql-data-and-log-files"></a>Para predimensionar datos de SQL y archivos de registro

1.  Inicie SQL Server Management Studio.

2.  Vaya a la base de datos FIMService, haga clic con el botón derecho en FIMService y, después, haga clic en Propiedades.

3.  En la página Archivos, amplíe los archivos de la base de datos al tamaño necesario.

### <a name="isolate-log-from-data-files"></a>Aislar el registro de los archivos de datos

Siga los procedimientos recomendados del servidor de SQL Server para aislar los archivos de registro de transacciones y datos de las bases de datos en discos físicos independientes.

Crear archivos tempdb adicionales

Para obtener un rendimiento óptimo, le recomendamos que cree un archivo de datos por núcleo de CPU en el archivo tempdb.

### <a name="to-create-additional-tempdb-files"></a>Para crear archivos tempdb adicionales

1.  Inicie SQL Server Management Studio.

2.  Vaya a la base de datos tempdb en las bases de datos del sistema, haga clic con el botón derecho en tempdb y, después, haga clic en Propiedades.

3.  En la página Archivos, cree un archivo de datos para cada núcleo de CPU. Asegúrese de separar los archivos de registro y datos tempdb en diferentes unidades y ejes.

### <a name="ensure-adequate-space-for-log-files"></a>Garantizar un espacio adecuado para los archivos de registro

Es importante comprender los requisitos del disco del modelo de recuperación. El modo de recuperación sencilla puede ser adecuado durante la carga inicial del sistema para limitar el uso del espacio en disco, pero los datos creados después de la copia de seguridad más reciente están expuestos a la pérdida de datos. Al usar el modo de recuperación completa, necesita administrar el uso del disco a través de copias de seguridad que incluye copias de seguridad frecuentes del registro de transacciones para evitar un uso elevado del espacio en disco. Para obtener más información, vea [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370) (Información general del modelo de recuperación).

### <a name="limit-sql-server-memory"></a>Limitar la memoria del servidor de SQL Server

Dependiendo de la cantidad de memoria que tenga en su servidor de SQL Server y de si comparte el servidor con otros servicios (es decir, el servicio MIM 2016 y el servicio de sincronización MIM 2016), puede que quiera restringir el consumo de memoria de SQL. Puede realizar esto mediante los pasos siguientes.

1.  Inicie el Administrador corporativo de SQL Server.

2.  Seleccione Nueva consulta.

3.  Ejecute la siguiente consulta:

  ```SQL
  USE master

  EXEC sp_configure 'show advanced options', 1

  RECONFIGURE WITH OVERRIDE

  USE master

  EXEC sp_configure 'max server memory (MB)', 12000--- max=12G RECONFIGURE
  WITH OVERRIDE
  ```

  En este ejemplo se vuelve a configurar el servidor de SQL Server para no usar más de 12 gigabytes (GB) de memoria.

4.  Compruebe la configuración con la consulta siguiente:

  ```SQL
  USE master

  EXEC sp_configure 'max server memory (MB)'--- verify the setting

  USE master

  EXEC sp_configure 'show advanced options', 0

  RECONFIGURE WITH OVERRIDE
  ```

### <a name="backup-and-recovery-configuration"></a>Copia de seguridad y configuración de recuperación

En general, debe realizar copias de seguridad de la base de datos según la directiva de copia de seguridad de su organización. Si no se han planeado copias de seguridad de registro incremental, la base de datos debe establecerse en el modo de recuperación sencilla. Asegúrese de que entiende las implicaciones de los diferentes modelos de recuperación antes de implementar la estrategia de copia de seguridad así como los requisitos de espacio en disco para estos modelos. El modelo de recuperación completa necesita copias de seguridad del registro frecuentes para evitar un uso elevado del espacio en disco. Para obtener más información, vea [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370) (Información general del modelo de recuperación) y [FIM 2010 Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864) (Guía de restauración y copia de seguridad de FIM 2010).

## <a name="create-a-backup-administrator-account-for-the-fimservice-after-installation"></a>Crear una cuenta de administrador de copias de seguridad para FIMService después de la instalación


>[!IMPORTANT]
Los miembros del conjunto de administradores de FIMService tienen permisos únicos esenciales para la operación de la implementación de FIM. Si no puede iniciar sesión como parte del conjunto de administradores, la única solución es retroceder a una copia de seguridad anterior del sistema. Para mitigar esta situación, le recomendamos que agregue otros usuarios al conjunto administrativo de FIM como parte de su configuración posterior a la instalación.

## <a name="fim-service"></a>Servicio de FIM


### <a name="configuring-fim-service-service-exchange-mailbox"></a>Configurar el buzón de Exchange de servicio del servicio FIM

A continuación se muestran los procedimientos recomendados para configurar Microsoft Exchange Server para la cuenta de servicio del servicio de MIM 2016.

- Configure la cuenta de servicio para que solo pueda aceptar correo de direcciones internas de correo electrónico. Concretamente, el buzón de la cuenta de servicio nunca podrá recibir correo de servidores SMTP externos.

#### <a name="to-configure-the-service-account"></a>Para configurar la cuenta de servicio

1.  En la Consola de administración de Exchange, seleccione la **cuenta de servicio del servicio FIM**.

2.  Seleccione Propiedades, seleccione Configuración del flujo de correo y, después, seleccione **Restricciones en la entrega de correo**.

3.  Seleccione la casilla **Pedir la autenticación de todos los remitentes**.

Para obtener más información, vea [Configure Message Delivery Restrictions](http://go.microsoft.com/fwlink/?LinkID=183625) (Configurar las restricciones en la entrega de correo).

-   Configure la cuenta de servicio de manera que rechace el correo con un tamaño superior a 1 MB. Siga los procedimientos recomendados para [Configure Message Size Limits](http://go.microsoft.com/fwlink/?LinkID=183626) (Configurar los límites de tamaño de los mensajes) de un buzón o de una carpeta pública habilitada para correo.

-   Configure la cuenta de servicio de manera que tenga una cuota de almacenamiento del buzón de 5 GB. Para obtener unos resultados óptimos, siga los procedimientos recomendados que se muestran en [Configure Storage Quotas for a Mailbox](http://go.microsoft.com/fwlink/?LinkID=156929) (Configurar las cuotas de almacenamiento de un buzón).

## <a name="mim-portal"></a>Portal MIM


### <a name="disable-sharepoint-indexing"></a>Deshabilitar la indexación de SharePoint

Le recomendamos que deshabilite la indexación de Microsoft Office SharePoint®. No existen documentos que necesiten indexarse, y la indexación provoca muchas entradas de registro de errores y posibles problemas de rendimiento con FIM 2010. Para deshabilitar la indexación de SharePoint

1.  En el servidor que hospeda el Portal de MIM 2016, haga clic en Inicio.

2.  Haga clic en Todos los programas.

3.  En la lista Todos los programas, haga clic en Herramientas administrativas.

4.  En Herramientas administrativas, haga clic en Administración central de SharePoint.

5.  En la página Administración central, haga clic en Operaciones.

6.  En la página Operaciones, en Configuración global, haga clic en Definiciones de trabajos del temporizador.

7.  En la página Definiciones de trabajos del temporizador, haga clic en Actualización de SharePoint Services Search.

8.  En la página Editar trabajo del temporizador, haga clic en Deshabilitar.

## <a name="mim-2016-initial-data-load"></a>Carga de datos inicial de MIM 2016

En esta sección se enumeran una serie de pasos para aumentar el rendimiento de la carga de datos inicial desde el sistema externo en FIM 2010. Es importante comprender que algunos de estos pasos son temporales durante el rellenado inicial del sistema y deben restablecerse tras su finalización. Esta es una operación de un solo uso y no es una sincronización continua.

>[!NOTE]
Para obtener más información sobre la sincronización de usuarios entre FIM 2010 y Active Directory Domain Services (AD DS), vea [How do I Synchronize Users from Active Directory to FIM](http://go.microsoft.com/fwlink/?LinkID=188277) (Cómo sincronizo usuarios de Active Directory en FIM) en la documentación de FIM.

>[!IMPORTANT]
Asegúrese de que ha aplicado los procedimientos recomendados que se tratan en la sección de configuración SQL de esta guía.                                                                                                                                                      |

### <a name="step-1-configure-the-sql-server-for-initial-data-load"></a>Paso 1: Configurar el servidor de SQL Server para la carga de datos inicial
Cuando planee cargar inicialmente muchos datos, puede reducir el tiempo que se tarda en rellenar la base de datos desactivando temporalmente la búsqueda de texto completo y activándola de nuevo después de que se haya completado la exportación en el agente de administración de MIM 2016 (FIM MA).

Para desactivar temporalmente la búsqueda de texto completo:

1.  Inicie SQL Server Management Studio.

2.  Seleccione Nueva consulta.

3.  Ejecute las siguientes instrucciones SQL:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING =
MANUAL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = MANUAL
```

Es importante que entienda los requisitos del disco para el modelo de recuperación del servidor de SQL Server. Dependiendo de la programación de las copias de seguridad, puede considerar la posibilidad de usar el modo de recuperación sencilla durante la carga inicial del sistema para limitar el uso del espacio en disco, pero necesita entender las implicaciones desde una perspectiva de pérdida de datos.
Al usar el modo de recuperación completa, necesita administrar el uso del disco a través de copias de seguridad que incluye copias de seguridad frecuentes del registro de transacciones para evitar un uso elevado del espacio en disco.

>[!IMPORTANT]
Si estos procedimientos no se implementan puede provocarse un uso elevado del espacio en disco, lo que posiblemente hará que se quede sin espacio en este. Puede encontrar información adicional sobre este tema en [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370) (Información general del modelo de recuperación). [The FIM Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864) (La Guía de restauración y copia de seguridad de FIM) contiene información adicional.

### <a name="step-2-apply-the-minimum-necessary-mim-configuration-during-the-load-process"></a>Paso 2: Aplicar la configuración mínima de MIM necesaria durante el proceso de carga

Durante el proceso de carga inicial, solo debe aplicar la configuración mínima necesaria en su configuración de FIM para sus reglas de directiva de administración (MPR) y definiciones de conjunto. Después de que finalice la carga de datos, cree los conjuntos adicionales necesarios para su implementación. Use la configuración Ejecutar al actualizar la directiva en los flujos de trabajo de acción para aplicar esas directivas de manera retroactiva en los datos cargados.

### <a name="step-3-configure-and-populate-the-fim-service-with-external-identity-data"></a>Paso 3: Configurar y rellenar el servicio FIM con datos de identidad externos


En este punto debe seguir los procedimientos descritos en la guía How do I Synchronize Users from Active Directory

Domain Services to FIM (Cómo sincronizo usuarios de Active Directory Domain Services con FIM) para configurar y sincronizar su sistema con usuarios de Active Directory. Si necesita sincronizar la información de grupo, los procedimientos para ese proceso se describen en la Guía Cómo sincronizar grupos de Active Directory Domain Services en FIM.

#### <a name="synchronization-and-export-sequences"></a>Secuencias de exportación y sincronización

Para optimizar el rendimiento, ejecute una exportación después de una ejecución de sincronización que provoque un gran número de operaciones de exportación pendientes en un espacio conector.

Después, ejecute una ejecución de importación de confirmación en el agente de administración que está asociado con el espacio conector afectado. Por ejemplo, cuando necesite ejecutar perfiles de ejecución de sincronización en varios agentes de administración como parte de una carga de datos inicial, debe ejecutar una exportación seguida de una importación delta después de cada ejecución de sincronización individual.

En cada agente de administración de origen que forma parte de su ciclo de inicialización, realice los pasos siguientes:

1.  Importación completa en un agente de administración de origen.

2.  Sincronización completa en el agente de administración de origen.

3.  Exportación de todos los agentes de administración de destino afectados con operaciones de exportación preconfiguradas.

4.  Importación delta de todos los agentes de administración de destino afectados con operaciones de exportación preconfiguradas.

### <a name="step-4-apply-your-full-mim-configuration"></a>Paso 4: Aplicar su configuración de MIM completa


Una vez que su carga de datos inicial finalice, debe aplicar la configuración de MIM completa para su implementación.

Dependiendo de los escenarios, esto puede incluir la creación de conjuntos, MPR y flujos de trabajo adicionales. Para cualquier directiva que necesite aplicar de manera retroactiva en todos los objetos existentes del sistema, use la configuración Ejecutar al actualizar la directiva en los flujos de trabajo de acción para aplicar esas directivas de manera retroactiva en los datos cargados.

### <a name="step-5-reconfigure-sql-to-previous-settings"></a>Paso 5: Volver a configurar SQL a la configuración anterior


Recuerde cambiar la configuración SQL a su configuración normal. Esto incluye:

-   Activar la búsqueda de texto completo

-   Actualizar la directiva de copia de seguridad según la directiva de la organización

Una vez que haya completado la carga de datos inicial, necesita activar la búsqueda de texto completo de nuevo. Ejecute las siguientes instrucciones SQL para activar la búsqueda de texto completo de nuevo:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = AUTO

ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = AUTO
```

Si tiene que cambiar al modo de recuperación sencilla, asegúrese de que vuelve a configurar su programación de copia de seguridad según la directiva de copia de seguridad de la organización. Se encuentra disponible información adicional de las programaciones de copia de seguridad de FIM en la [FIM Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864) (Guía de restauración y copia de seguridad de FIM).

## <a name="configuration-migration"></a>Migración de la configuración


### <a name="avoid-changing-display-names"></a>Evitar cambiar los nombres para mostrar

Para muchos tipos de objetos como MPR, el script syncproduction.ps1 usa el nombre para mostrar como el único atributo delimitador entre dos sistemas. Por lo tanto, un cambio en un nombre para mostrar de MPR existente provoca la eliminación de la MPR existente, seguido de la creación de una nueva MPR. Este resultado se produce porque el proceso de migración no puede unir MPR correctamente cuyos criterios de combinación han cambiado. Para evitar este problema, puede enlazar un atributo personalizado a todos los tipos de objetos de configuración y usar ese atributo como los criterios de combinación. Esto le permite modificar los nombres para mostrar sin que afecte al proceso de migración.

### <a name="avoid-changing-the-content-of-intermediate-files"></a>Evitar cambiar el contenido de los archivos intermedios

Aunque el formato de archivo y la interfaz de programación de aplicaciones (API) de los objetos de bajo nivel son públicos y los desarrolladores admiten las manipulaciones, no le recomendamos cambiar el contenido de los formatos intermedios durante la migración. En cambio, puede que sea necesario quitar el objeto ImportObjects completo de changes.xml o realizar operaciones de buscar y reemplazar en pilot.xml para reemplazar los números de versión o la información del Sistema de nombres de dominio (DNS) piloto por la información DNS de producción.

### <a name="ensure-that-the-version-number-is-correct-in-pilotxml-when-migrating-across-versions"></a>Garantizar que el número de versión es correcto en pilot.xml al migrar entre versiones

Aunque las migraciones entre números de versión no se recomiendan ni se admiten, a menudo puede realizar esto reemplazando el número de versión piloto por el número de versión de producción en pilot.xml. Concretamente, los objetos WorkflowDefinition y

ActivityInformationConfiguration necesitan el número de versión para hacer referencia de manera precisa a las actividades de flujo de trabajo en el entorno de producción. Un error al reemplazar el número de versión provoca que el cmdlet Compare-FIMConfig identifique las diferencias entre los atributos de lenguaje de marcado de objetos extensible (XOML) en WorkflowDefinitions y migre el número de la versión piloto. El servicio FIM de producción puede provocar un error al iniciar las actividades de flujo de trabajo con el número de versión incorrecto.

### <a name="avoid-cyclic-references"></a>Evitar referencias cíclicas

En general, las referencias cíclicas no se recomiendan en una configuración de MIM.
En cambio, a veces se producen ciclos cuando el Conjunto A se refiere al Conjunto B y el Conjunto B también se refiere al Conjunto A. Para evitar problemas con las referencias cíclicas, debe cambiar la definición del Conjunto A o el Conjunto B de manera que no se hagan referencia entre sí. Después, reinicie el proceso de migración. Si tiene referencias cíclicas y el cmdlet Compare-FIMConfig provoca un error como resultado, es necesario interrumpir el ciclo manualmente. Como el cmdlet Compare-FIMConfig genera una lista de cambios en orden de precedencia, necesita que no exista ningún ciclo entre las referencias de los objetos de configuración.

## <a name="security"></a>Seguridad

### <a name="mim-ma-account"></a>Cuenta del MA de MIM

La cuenta del MA de MIM no se considera una cuenta de servicio y debe ser una cuenta de usuario normal. Las cuentas deben poder iniciar sesión localmente para que la cuenta de servicio del Servicio de sincronización FIM pueda suplantarlo.

Para permitir que la cuenta del MA de MIM inicie sesión localmente

1.  Haga clic en Inicio, en Herramientas administrativas y, después, en Directiva de seguridad local.

2.  Abra el nodo de directivas locales y, después, haga clic en Asignación de derechos de usuario.

3.  En la directiva Permitir el inicio de sesión local, asegúrese de que la cuenta del MA de FIM se especifica explícitamente, o agréguela a uno de los grupos a los que ya se les ha concedido acceso.

### <a name="fim-synchronization-service-and-fim-services-accounts"></a>Cuentas del Servicio de sincronización FIM y de los Servicios FIM

Para configurar los servidores que ejecutan los componentes de servidor de MIM de manera segura deben restringirse las cuentas de servicio. Con el procedimiento anterior para activar la cuenta del MA de MIM, establezca las siguientes restricciones en las cuentas del Servicio de sincronización FIM y de los Servicios FIM:

-   Denegar el inicio de sesión como trabajo por lotes

-   Denegar el inicio de sesión local

-   Denegar el acceso a este equipo desde la red

Las cuentas de servicio no deben ser un miembro del grupo local de administradores.

La cuenta de servicio del Servicio de sincronización FIM no debe ser un miembro de los grupos de seguridad usados para controlar el acceso al Servicio de sincronización FIM (grupos que comienzan por FIMSync, por ejemplo, FIMSyncAdmins, y así sucesivamente).

>[!IMPORTANT]
 Si selecciona las opciones para usar la misma cuenta para las cuentas de servicio y separa el Servicio FIM y el Servicio de sincronización FIM, entonces no puede establecer el acceso Denegar en este equipo desde la red en el servidor del Servicio de sincronización para MMS. Si se deniega el acceso, eso impediría que el Servicio FIM se pusiera en contacto con el Servicio de sincronización FIM para cambiar la configuración y administrar contraseñas.

### <a name="password-reset-deployed-to-kiosk-like-computers-should-set-local-security-to-clear-virtual-memory-pagefile"></a>El restablecimiento de contraseñas implementado en los equipos de quiosco debe establecer la seguridad local para borrar el archivo de paginación de memoria virtual

Al implementar el restablecimiento de contraseña de FIM en una estación de trabajo diseñada para ser un quiosco, recomendamos que se active la configuración de la directiva de seguridad local Apagado: borrar el archivo de paginación de la memoria virtual para garantizar que la información confidencial de la memoria de proceso no está disponible para los usuarios no autorizados.

### <a name="implementing-ssl-for-the-fim-portal"></a>Implementar SSL en el Portal de FIM

Es muy recomendable que use Capa de sockets seguros (SSL) en el servidor del Portal de FIM para proteger el tráfico entre los clientes y el servidor.

Para implementar SSL:

1.  En el servidor del Portal de MIM, abra Administrador de IIS.

2.  Haga clic en el nombre del equipo local.

3.  Haga clic en Certificados de servidor.

4.  Haga clic en Crear una solicitud de certificado.

5.  En el cuadro de texto Nombre común, escriba el nombre del servidor.

6.  Haga clic en Siguiente y, después, haga clic en Siguiente.

7.  Guarde el archivo en cualquier ubicación. Necesitará tener acceso a esta ubicación en los pasos posteriores.

8.  En Windows Internet Explorer®, vaya a https://nombreDeServidor/certsrv. Reemplace nombreDeServidor por el nombre del servidor que emite certificados.

9.  Haga clic en Solicitar un nuevo certificado.

10. Haga clic en Enviar una solicitud avanzada.

11. Haga clic en Enviar una solicitud de certificado con un codificado base 64.

12. Pegue el contenido del archivo que ha guardado en el paso anterior.

13. En la plantilla de certificado, seleccione Servidor web.

14. Haga clic en Enviar.

15. Guarde el certificado en el escritorio.

16. En el Administrador de IIS, haga clic en Completar solicitud de certificación.

17. Indique al Administrador de IIS el certificado que acaba de guardar en el escritorio.

18. En Nombre descriptivo, escriba el nombre del servidor.

19. Haga clic en Sitios y, después, seleccione SharePoint – 80.

20. Haga clic en Enlaces y, después, haga clic en Agregar.

21. Seleccione HTTPS.

22. Para el certificado, seleccione el que tiene el mismo nombre que el servidor (este es el certificado que acaba de importar).

23. Haga clic en Aceptar.

24. Quite el enlace HTTP.

25. Haga clic en Configuración de SSL y, después, marque Requerir SSL.

26. Guarde la configuración.

27. Haga clic en Inicio, en Herramientas administrativas y, después, en Administración central de SharePoint 3.0.

28. Haga clic en Operaciones y, después, haga clic en Asignaciones de acceso alternativas.

29. Haga clic en http://servername.

30. Cambie http://servername por https://servername y, después, haga clic en Aceptar.

31. Haga clic en Inicio, haga clic en Ejecutar, escriba iisreset y, después, haga clic en Aceptar.

## <a name="performance"></a>Pruebas

Para obtener una configuración de rendimiento óptimo:

-   Aplique los procedimientos recomendados de configuración SQL como se describen en la sección de configuración SQL de este documento.

-   Desactive la indexación de SharePoint en el sitio del Portal de FIM 2010 R2. Para obtener más información, vea la sección Deshabilitar la indexación de SharePoint de este documento.

## <a name="feature-specific-best-practices--i-want-to-remove-this-and-collapse-this-section-and-just-have-the-specific-features-at-header-2-level-versus-3"></a>Procedimientos recomendados de características específicas (Quiero quitar esto y contraer esta sección y tener solo las características específicas en el encabezado de nivel 2 en lugar del 3)


### <a name="request-management"></a>Administración de solicitudes

De manera predeterminada, MIM 2016 purga los objetos del sistema caducados, que incluye solicitudes completas con aprobaciones asociadas, respuestas de aprobación e instancias de flujo de trabajo en un intervalo de 30 días. Si su organización necesita un historial de solicitud más largo, debe exportar solicitudes de MIM y almacenarlas en una base de datos auxiliar para conservarlas más allá de la ventana de 30 días. Aunque la ventana de eliminación de solicitudes de 30 días se puede configurar, ampliar esta ventana puede afectar negativamente al rendimiento debido a los objetos adicionales del sistema.

### <a name="management-policy-rules"></a>Reglas de directivas de administración

#### <a name="use-the-appropriate-mpr-type"></a>Usar el tipo de MPR adecuado

MIM proporciona dos tipos de MPR, solicitud y establecimiento de transición:

-   MPR de solicitud (RMPR)

 - Se usa para definir la directiva de control de acceso (autenticación, autorización y acción) para las operaciones Create, Read, Update o Delete (CRUD) en los recursos.
 - Se aplica cuando se emite una operación CRUD en un recurso de destino en FIM.
   - Se determina por los criterios de coincidencia definidos en la regla, es decir, en las solicitudes CRUD que se aplica la regla.


-   MPR de establecimiento de transición (TMPR)
 - Se usa para definir directivas independientemente de cómo ha especificado el objeto el estado actual representado mediante el establecimiento de transición. Use TMPR en las directivas de derechos de modelo.
 - Se aplica cuando un recurso entra o sale de un conjunto asociado.
 - Se limita a los miembros del conjunto.

>[NOTA] Para obtener información adicional, vea [Designing Business Policy Rules](http://go.microsoft.com/fwlink/?LinkID=183691) (Diseñar reglas de directiva empresarial).

#### <a name="only-enable-mprs-as-necessary"></a>Habilitar las MPR solo cuando sea necesario

Use el principio con privilegios mínimos al aplicar la configuración. Las MPR controlan la directiva de acceso a su implementación de FIM. Habilite solo las características usadas por la mayoría de los usuarios. Por ejemplo, no todos los usuarios usan FIM para la administración de grupos, por lo que las MPR de administración de grupos asociadas deben deshabilitarse. De manera predeterminada, FIM se entrega con la mayoría de los permisos que no sean de administrador deshabilitados.

#### <a name="duplicate-built-in-mprs-instead-of-directly-modifying"></a>Duplicar las MPR integradas en lugar de modificarlas directamente

Cuando necesite modificar las MPR integradas, debe crear una nueva MPR con la configuración necesaria y desactivar la MPR integrada. Esto garantiza que cualquier cambio futuro en las MPR integradas que se presente mediante el proceso de actualización no afecte de manera negativa a la configuración del sistema.

#### <a name="end-user-permissions-should-use-explicit-attribute-lists-scoped-to-users-business-needs"></a>Los permisos de usuario final deben usar listas de atributos explícitas destinadas a las necesidades empresariales de los usuarios

Usar listas de atributos explícitas ayuda a evitar la concesión accidental de permisos a usuarios sin privilegios cuando los atributos se agregan a los objetos.
Los administradores deben necesitar explícitamente conceder el acceso a atributos nuevos en lugar de intentar quitar el acceso.

El acceso a los datos debe destinarse a las necesidades empresariales de los usuarios. Por ejemplo, los miembros del grupo no deben tener acceso al atributo de filtro del grupo del que son miembros. El filtro puede revelar accidentalmente datos de la organización a los que el usuario no tendrá acceso normalmente.

#### <a name="mprs-should-reflect-effective-permissions-in-the-system"></a>Las MPR deben reflejar los permisos efectivos del sistema

Evite la concesión de permisos a atributos que el usuario nunca puede usar. Por ejemplo, no debe conceder permiso para modificar atributos de recursos fundamentales como objectType. A pesar de la MPR, el sistema deniega cualquier intento de modificar un tipo de recurso después de su creación.

#### <a name="read-permissions-should-be-separate-from-modify-and-create-permissions-when-using-explicit-attributes-in-mprs"></a>Los permisos de lectura deben ser independientes de los permisos de creación o modificación al usar atributos explícitos en las MPR

Al mostrar atributos explícitamente en las MPR, los atributos necesarios para Crear y Modificar normalmente son diferentes de los que están disponibles para Leer. Por ejemplo, Leer puede concederse en atributos del sistema como Creator u objectId, mientras que Crear o Modificar no puede especificarse para los atributos del sistema.

#### <a name="create-permissions-should-be-separate-from-modify-permissions-when-using-explicit-attributes-in-rules"></a>Los permisos de creación deben ser independientes de los permisos de modificación al usar atributos explícitos en las reglas

La operación Create necesita que el usuario seleccione el objectType como parte de su operación. Este es un atributo del sistema fundamental que no puede modificarse después de una operación Create.

#### <a name="use-one-request-mpr-for-all-attributes-with-the-same-access-requirements"></a>Usar una MPR de solicitud para todos los atributos con los mismos requisitos de acceso

Para los atributos con los mismos requisitos de acceso que no se espera que cambien, puede combinarlos en una única MPR de solicitud para mejorar la eficacia.

#### <a name="avoid-giving-unrestricted-access-even-to-selected-principal-groups"></a>Evitar proporcionar acceso sin restricciones incluso a los grupos principales seleccionados

En FIM, los permisos se definen como una aserción positiva. Como FIM no admite permisos de denegación, proporcionar acceso sin restricciones a un recurso complica la prestación de cualquier exclusión en los permisos. Como procedimiento recomendado, conceda solo los permisos necesarios.

>[!NOTE]
A continuación se muestra la sección Derechos. Me pregunto cómo combinarlas para evitar crear encabezados de 5 niveles
#### <a name="use-tmprs-to-define-custom-entitlements"></a>Usar las TMPR para definir derechos personalizados

Use MPR de establecimiento de transición (TMPR) en lugar de las RMPR para definir derechos personalizados.
Las TMPR proporcionan un modelo basado en el estado para asignar o quitar derechos basados en la pertenencia en los conjuntos de transición definidos, o en los roles, y las actividades de flujo de trabajo complementarias. Las TMPR siempre deben definirse en pares, una para los recursos de transición de entrada y otra para los de salida. Además, cada MPR de transición debe contener flujos de trabajo independientes para el aprovisionamiento y desaprovisionamiento de actividades.

>[!NOTE]
Cualquier flujo de trabajo de desaprovisionamiento debe garantizar que el atributo Ejecutar al actualizar la directiva se establece en True.

#### <a name="enable-the-set-transition-in-mpr-last"></a>Habilitar la última In MPR de establecimiento de transición

Al crear un par de TMPR, active la última In MPR de establecimiento de transición. Este orden garantiza que ningún recurso se conserva con el derecho si este se agrega y se quita del conjunto mientras In MPR está activada pero antes de que se active Out MPR.

#### <a name="workflows-in-tmpr-should-check-target-resource-state-first"></a>Los flujos de trabajo en TMPR deben comprobar primero el estado de los recursos de destino

El aprovisionamiento de flujos de trabajo debe comprobarse primero para determinar si el recurso de destino ya se ha aprovisionado de acuerdo con el derecho. Si lo ha hecho, después no puede hacer nada.

El desaprovisionamiento de flujos de trabajo debe comprobarse primero para determinar si el recurso de destino se ha aprovisionado. Si lo ha hecho, entonces debe desaprovisionar el recurso de destino.
De otro modo, no haga nada.

#### <a name="select-run-on-policy-update-for-tmprs"></a>Seleccionar Ejecutar al actualizar la directiva para TMPR

Esto garantiza que el comportamiento de aprovisionamiento correcto se aplica cuando las actualizaciones de directivas se implementan y usa la marca Ejecutar al actualizar la directiva en los flujos de trabajo de acción asociados con las TMPR. Esto garantiza que los cambios en las definiciones de directiva aplican los flujos de trabajo de acción en los miembros nuevos del establecimiento de transición.

#### <a name="avoid-associating-the-same-entitlement-with-two-different-transition-sets"></a>Evitar asociar el mismo derecho con dos establecimientos de transición diferentes

Asociar el mismo derecho con dos establecimientos de transición diferentes puede provocar una revocación innecesaria y volver a conceder derechos si el recurso se mueve de un establecimiento a otro. Como procedimiento recomendado, asegúrese de que un establecimiento contiene todos los recursos que necesita el derecho asociado. Esto garantiza una relación uno a uno entre el establecimiento de transición y la concesión de derechos del flujo de trabajo.

#### <a name="use-an-appropriate-sequence-of-operations-when-removing-entitlements-in-the-system"></a>Usar una secuencia de operaciones adecuada al quitar derechos del sistema

El orden de los pasos realizados al quitar derechos del sistema puede producir dos resultados operativos diferentes. Asegúrese de que entiende qué orden se aplica al efecto que quiere.

Para quitar un derecho del sistema (y revocarlo de todos los miembros que tienen actualmente el derecho):

1.  Deshabilite la T-In MPR. Esto evita nuevas concesiones.

2.  Elimine el filtro T-Set o cámbielo de manera que el establecimiento quede vacío. Esto hace que todos los miembros existentes inicien una transición y se aplica la directiva de transición, incluidos los flujos de trabajo de desaprovisionamiento configurados asociados con el derecho.

3.  Deshabilite la T-Out MPR.

Para quitar un derecho pero mantener solo los miembros actuales (por ejemplo, dejar de usar FIM para administrar el derecho):

1.  Deshabilite la T-In MPR. Esto evita nuevas concesiones.

2.  Deshabilite la T-Out MPR.

3.  Elimine el filtro T-Set o cámbielo de manera que el establecimiento quede vacío. Como el establecimiento ya no está vinculado a una TMPR, no se aplica ningún flujo de trabajo de desaprovisionamiento.

### <a name="sets"></a>Establece

Al aplicar los procedimientos recomendados en los conjuntos, necesita tener en cuenta el impacto de las optimizaciones en la manejabilidad y facilidad de la administración futura.
Debe realizarse una prueba apropiada en la escala de producción esperada para identificar el equilibrio correcto entre el rendimiento y la manejabilidad antes de aplicar estas recomendaciones.

>[!NOTE]
Se aplican todas las directrices siguientes en los conjuntos dinámicos y grupos dinámicos.


#### <a name="minimize-the-use-of-dynamic-nesting"></a>Minimizar el uso de anidamiento dinámico

Esto hace referencia al filtro de un conjunto que hace referencia al atributo ComputedMember de otro conjunto. Un motivo común de los conjuntos de anidamiento es evitar duplicar una condición de pertenencia en muchos conjuntos. Aunque este enfoque puede provocar una manejabilidad mejor de los conjuntos, existe una compensación de rendimiento. Puede optimizar el rendimiento duplicando las condiciones de pertenencia de un conjunto anidado en lugar de anidar el propio conjunto.

Puede encontrar casos en los que no pueda evitar anidar conjuntos para cumplir un requisito funcional. Estas son las situaciones principales en las que debe anidar conjuntos. Por ejemplo, para definir el conjunto de todos los grupos sin los propietarios de empleados a jornada completa, el anidamiento de conjuntos debe usarse de la manera siguiente: `/Group[not(Owner =
/Set[ObjectID = ‘X’]/ComputedMember]`, donde "X" es el ObjectID del conjunto de todos los empleados a jornada completa.

#### <a name="minimize-the-use-of-negative-conditions"></a>Minimizar el uso de condiciones negativas

Las condiciones negativas son las condiciones de pertenencia que hacen uso de los siguientes operadores o funciones: `!=`, `not()`, `\<`, `\<=`. Para optimizar el rendimiento, donde sea posible, exprese la condición que quiera con varias condiciones positivas en lugar de como una condición negativa.

#### <a name="minimize-the-use-of-membership-conditions-based-on-multivalued-reference-attributes"></a>Minimizar el uso de condiciones de pertenencia basándose en atributos de referencia de varios valores

El uso de condiciones basadas en atributos de referencia de varios valores debe minimizarse porque un gran número de estos conjuntos puede afectar al rendimiento de operaciones en el atributo que se ha usado en la condición de pertenencia.

### <a name="password-reset"></a>Restablecimiento de contraseña

#### <a name="kiosk-like-computers-that-are-used-for-password-reset-should-set-local-security-to-clear-the-virtual-memory-pagefile"></a>Los equipos de quiosco que se usan para el restablecimiento de contraseña deben establecer la seguridad local para borrar el archivo de paginación de memoria virtual

Al implementar el restablecimiento de contraseña de FIM 2010 en una estación de trabajo diseñada para ser un quiosco, recomendamos que se active la configuración de la directiva de seguridad local Apagado: borrar el archivo de paginación de la memoria virtual para garantizar que la información confidencial de la memoria de proceso no está disponible para los usuarios no autorizados.

#### <a name="users-should-always-register-for-a-password-reset-on-a-computer-that-they-are-logged-on-to"></a>Los usuarios siempre deben registrarse para un restablecimiento de contraseña en un equipo en el que hayan iniciado sesión

Cuando un usuario intenta registrarse para un restablecimiento de contraseña a través de un portal web, FIM 2010 siempre inicia el registro en nombre del usuario que ha iniciado sesión, independientemente de quién tenga la sesión iniciada en el sitio web. Los usuarios siempre deben registrarse para un restablecimiento de contraseña en un equipo en el que hayan iniciado sesión.

#### <a name="do-not-set-the-avoidpdconwan-registry-key-to-true"></a>No establezca la clave del Registro AvoidPdcOnWan en True

Al usar el restablecimiento de contraseña de MIM 2016, no establezca la clave del Registro AvoidPdcOnWan en True.

Si esta clave del Registro se establece en True, probablemente el usuario recorra las puertas de contraseña, tenga su restablecimiento de contraseña en el controlador de dominio principal (PDC) e intente iniciar sesión. Debido a esta clave del Registro, el controlador de dominio local no realiza la validación secundaria con el PDC, por lo que se deniega la solicitud de inicio de sesión. Si el usuario se deniega el número de veces suficiente, puede bloquearse fuera del dominio y necesitará llamar al soporte técnico.

#### <a name="do-not-turn-on-logging-of-clear-text-passwords"></a>No active el registro de contraseñas no cifradas

Es posible registrar contraseñas no cifradas al activar el seguimiento de nivel de servicio en Windows

Communication Foundation (WCF). Esta opción no está activada de manera predeterminada, y se desaconseja que la active en entornos de producción. Estas contraseñas son visibles como elementos no cifrados dentro de un mensaje de Protocolo simple de acceso a objetos (SOAP) cifrado cuando los usuarios se registran para un restablecimiento de contraseña. Para obtener más información, vea [Configuring Message Logging](http://go.microsoft.com/fwlink/?LinkID=168572) (Configurar el registro de mensajes).

#### <a name="do-not-map-an-authorization-workflow-to-the-password-reset-process"></a>No asigne un flujo de trabajo de autorización al proceso de restablecimiento de contraseñas

No debe adjuntar un flujo de trabajo de autorización en una operación de restablecimiento de contraseña.
El restablecimiento de contraseña necesita una respuesta sincrónica y los flujos de autorización que contienen actividades como la actividad de aprobación son asincrónicos.

#### <a name="do-not-map-multiple-action-activities-to-password-reset"></a>No asigne varias actividades de acción al restablecimiento de contraseñas

No debe adjuntar un flujo de trabajo que contenga más de una actividad de acción en una operación de restablecimiento de contraseña. Un escenario de ejemplo sería adjuntar una segunda actividad de restablecimiento de contraseña de AD DS en una MPR de restablecimiento de contraseña. En este escenario no se admite.

#### <a name="require-reregistration-when-adding-removing-or-changing-the-order-of-activities-in-an-existing-workflow"></a>Requerir un nuevo registro al agregar, quitar o cambiar el orden de actividades en un flujo de trabajo existente

Al agregar, quitar o cambiar el orden de las actividades de autenticación en un flujo de trabajo existente, seleccione siempre la opción para requerir un registro nuevo. Los usuarios que intentan autenticarse para el restablecimiento de contraseña después de que se haya agregado o quitado una actividad de un flujo de trabajo, pero antes de que se hayan vuelto a registrar, pueden detectar consecuencias no deseadas.

### <a name="portal-configuration-and-resource-control-display-configuration"></a>Configuración del portal y configuración de la visualización de control de recursos

#### <a name="consider-adding-a-privacy-disclaimer-to-the-user-profile-page"></a>Considere la posibilidad de agregar un aviso de privacidad en la página de perfil del usuario

En MIM, de manera predeterminada, alguna información de perfil del usuario puede mostrarse a otros usuarios. Como cortesía para los usuarios, los administradores deben considerar la opción de agregar texto personalizado coherente con sus directivas de la empresa en la página de perfil del usuario. Para obtener más información sobre cómo agregar texto personalizado a una página del Portal de MIM, vea la introducción de [Configuring and Customizing the FIM Portal](http://go.microsoft.com/fwlink/?LinkID=165848) (Configurar y personalizar el Portal de FIM).

### <a name="schema"></a>Schema

#### <a name="do-not-delete-person-or-group-resource-types"></a>No elimine los tipos de recurso de persona o grupo

Aunque los tipos de recurso de persona y grupo no están marcados como tipos de recurso fundamentales, los propios recursos o los atributos asignados a estos no deben eliminarse. La interfaz de usuario (IU) del Portal de MIM necesita que los tipos de recurso de persona y grupo y sus atributos estén presentes.

#### <a name="do-not-modify-the-core-attributes"></a>No modifique los atributos fundamentales

Existen 13 atributos fundamentales asignados a todos los tipos de recurso. De ningún modo debe modificar su relación con cualquier tipo de recurso. Los 13 atributos fundamentales son:

-   CreatedTime

-   Creator

-   DeletedTime

-   Descripción

-   DetectedRulesList • DisplayName

-   ExpectedRulesList

-   ExpirationTime

-   Configuración regional

-   MVObjectID

-   ObjectID

-   ObjectType

-   ResourceTime

No elimine el recurso de esquema con una dependencia en los requisitos de auditoría

No debe eliminar sus recursos de esquema mientras siga teniendo requisitos de auditoría para estos.

#### <a name="making-regular-expressions-case-insensitive"></a>Hacer que las expresiones regulares no distingan mayúsculas de minúsculas

En FIM, puede resultar útil hacer que algunas expresiones regulares no distingan mayúsculas de minúsculas. Puede ignorar las mayúsculas y minúsculas en un grupo con ?!:. Por ejemplo, para el tipo de empleado, use

`\^(?!:contractor\|full time employee)%.`

#### <a name="calculation-of-the-member-attribute"></a>Cálculo del atributo de miembro

El atributo de miembro expuesto en el motor de sincronización está asignado realmente a ComputedMembers. Es una combinación de miembros basados en criterios y de miembros seleccionados manualmente. Incluso si agrega los tres atributos (Filter, ExplicitMembers y ComputedMembers), el cálculo dinámico del atributo de miembro no se produce para los tipos de recurso diferentes del de grupo y conjunto.

#### <a name="leading-and-trailing-spaces-in-strings-are-ignored"></a>Los espacios iniciales y finales de las cadenas se ignoran

En FIM, puede escribir cadenas con espacios iniciales y finales, pero el sistema FIM ignora esos espacios. Si envía una cadena con un espacio inicial y final, el motor de sincronización y los servicios web ignoran esos espacios.

#### <a name="empty-strings-do-not-equal-null"></a>Las cadenas vacías no son iguales a NULL

Las cadenas vacías no son iguales a NULL en esta versión de FIM. La entrada de una cadena vacía se considera un valor válido. No presente se considera como NULL.

### <a name="workflow-and-request-processing"></a>Flujo de trabajo y procesamiento de solicitudes

#### <a name="do-not-delete-default-workflows-that-are-shipped-with-mim-2016"></a>No elimine los flujos de trabajo predeterminados que se entregan con MIM 2016

Los siguientes flujos de trabajo se entregan con FIM 2010 y no deben eliminarse:

-   Flujo de trabajo de caducidad

-   Flujo de trabajo de validación de filtro para administradores

-   Flujo de trabajo de validación de filtro para los que no sean administradores

-   Flujo de trabajo de notificación de expiración de grupos

-   Flujo de trabajo de validación de grupo

-   Flujo de trabajo de aprobación del propietario

-   Flujo de trabajo de acción de restablecimiento de contraseña

-   Flujo de trabajo de autenticación de restablecimiento de contraseña

-   Validación de solicitante con autorización de propietario

-   Validación de solicitante sin autorización de propietario

-   Flujo de trabajo del sistema necesario para el Registro

#### <a name="do-not-run-two-or-more-approvalactivities-in-parallel"></a>No ejecute dos o más ApprovalActivities en paralelo

No debe ejecutar dos o más ApprovalActivities en paralelo. Hacer esto puede provocar que la solicitud se bloquee en la fase de autorización. Para varias aprobaciones, o incluir una lista más grande de aprobadores en la aprobación o secuenciar las dos actividades opuestas.

#### <a name="authorization-activities-should-not-modify-mim-resources-data"></a>Las actividades de autorización no deben modificar los datos de recursos de MIM

Evite usar actividades que modifiquen los recursos de MIM, como la Actividad de evaluador de funciones, como parte de los flujos de trabajo en los flujos de trabajo de autorización. Como la solicitud no se ha confirmado en el punto de autorización del procesamiento, cualquier modificación que se realice en la información de identidad puede aplicarse a pesar de que la solicitud posiblemente se rechace.

### <a name="understanding-fim-service-partitions"></a>Entender las particiones del servicio FIM

El objetivo de FIM es procesar solicitudes que puedan iniciarse por varios clientes de FIM como el servicio de sincronización de FIM y los componentes de autoservicio según sus directivas empresariales configuradas. Por diseño, cada instancia del servicio FIM pertenece a un grupo lógico que consta de una o más instancias del servicio FIM, que también se conoce como partición del servicio FIM. Si solo tiene una instancia del servicio FIM implementada para controlar todas las solicitudes, es posible que experimente latencias de procesamiento. Algunas operaciones pueden incluso superar los valores de tiempo de espera predeterminados que son adecuados para las operaciones de autoservicio. Las particiones del servicio FIM pueden ayudarle a tratar este problema. Para obtener información adicional, vea Entender las particiones del servicio FIM.
