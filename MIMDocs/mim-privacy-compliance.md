---
title: Control de datos de Microsoft Identity Manager | Microsoft Docs
description: Comprenda el control de datos de Microsoft Identity Manager para identificar los datos del entorno e informar sobre ellos y tomar medidas en el sistema en cuestión basadas en requisitos y funciones operativas.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 12/02/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: f75eb69360852c9f629b60d4900638c8b51e068a
ms.sourcegitcommit: 9e420840815adb133ac014a8694de9af4d307815
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "52825797"
---
# <a name="microsoft-identity-manager-data-handling"></a>Control de datos de Microsoft Identity Manager 

En este artículo se le proporcionarán instrucciones acerca de cómo las organizaciones toman decisiones que se pueden aplicar a muchos orígenes de datos conectados.  Esto puede conseguirse mediante la operaciones de búsqueda, eliminación, actualización e informe.  Antes de decidir el enfoque de eliminación o actualización, es fundamental comprender el diseño y la configuración actuales del sistema de administración de identidades (MIM). 

A continuación se muestran algunos escenarios que los clientes deberán tener en cuenta y responder a las siguientes preguntas: 

- ¿Qué datos necesita para la administración de identidades para ayudar con el proceso empresarial?
- ¿Dónde se van a almacenar los datos actuales en MIM?
- ¿Cómo usará estos datos en el sistema?
- ¿Va a compartir estos datos con cualquier origen de datos de asociado externo (exportación)?
- ¿Cuál es el origen autoritativo para los datos y su procesamiento?
- ¿Qué plan de retención y eliminación de datos se implementará?
- ¿Ha identificado toda la tecnología que necesita para procesar y administrar los datos?

Para ayudarle a entender un entorno de MIM actual, puede usar la herramienta siguiente para documentar su entorno de MIM o aplazar los documentos de diseño de implementación.
- [Documentador de MIM: permite exportar la configuración actual](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Buscar e identificar datos personales
La búsqueda de datos dentro de MIM dependerá de la configuración. La mayoría de los entornos están interconectados pero, para mayor claridad, los descompusimos por componente de alto nivel.

### <a name="synchronization-service"></a>Servicio de sincronización

Todos los datos de MIM relacionados con los usuarios se derivan de Active Directory (AD) y orígenes de datos de recursos humanos. Cuando busque datos personales, el primer lugar en el que debe buscar es AD o en los orígenes de datos conectados. 

Si no está seguro del origen de autoridad, puede realizar un seguimiento de este usuario desde la consola de Synchronization Service Manager de MIM. Haga clic en la barra de búsqueda de metaverso para ver los datos de identificación personales que se almacenan en la base de datos. Los usuarios pueden buscar un usuario o atributo específico.

- Para realizar una revisión o una búsqueda de datos de objetos de usuario
    - Abra el cliente del servicio de sincronización
        - Mediante el diseñador de metaverso puede ver las importaciones del flujo de atributos y la precedencia.
![mim-privacy-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Mediante la búsqueda de metaverso puede buscar en cualquier objeto y atributo dentro de la base de datos ![mim-privacy-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG).
 
Después de encontrar el objeto, al hacer clic en el objeto se abrirá la página de perfil de usuario. Los detalles de objetos proporcionan detalles completos sobre el objeto, sus atributos, la última modificación y el origen de autoridad, así como el origen de datos conectado relacionado que se deriva del ejemplo de configuración del agente de administración siguiente.

![mim-privacy-compliance.PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Servicio y portal/PAM
Es importante tener una instancia del servicio y portal o PAM instalada capaz de buscar usuarios. 

Si ha instalado el portal, puede utilizar la interfaz de usuario para buscar en cualquier atributo o consulta para un usuario determinado.

Si solo tiene instalado el servidor de servicio (sin la interfaz de usuario del portal), puede ejecutar una sintaxis de búsqueda basándose en [FIMAutomation PSSnapin]. Puede encontrar el ejemplo [aquí](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

PAM puede usar la misma sintaxis anterior o puede utilizar el [módulo MIMPAM](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1), específicamente el cmdlet get-pamuser para buscar el usuario en el entorno de PAM.

En el portal y el servicio hay otras opciones de informes para buscar en los datos disponibles.
- [Creación de informes híbridos](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Creación de informes con SCSM](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
El servicio Core de Bhold tiene una interfaz de usuario que permite buscar un usuario o atributos. 

![búsqueda de bhold](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Si va a sincronizar BHOLD con [Access Management Connector](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install) para el servicio de sincronización podrá ver los objetos de usuario conectados y los atributos que envía a Core de BHOLD.

También puede cargar el módulo de informes de BHOLD.

- [BHOLD Reporting](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Administración de certificados
La búsqueda del servicio de administración de certificados está integrada en la interfaz de usuario. El administrador se iniciará y seleccionará “Buscar un usuario para ver o administrar su información”.  

![búsqueda de administración de certificados](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Exportar datos personales
Dado que los datos relacionados con las entidades de MIM se derivarán de varios orígenes, la mayoría de los datos se almacenan en la base de datos del servicio de sincronización. Por este motivo, debe exportar los datos relacionados con el objeto desde la sincronización de MIM o puede determinar el propietario de dichos datos.

### <a name="synchronization-service"></a>Servicio de sincronización
Lo servicios de sincronización de exportación de datos simplemente seleccionan los datos de la interfaz de usuario de búsqueda y los copian y pegan en un formato csv o de su preferencia. Otra forma de exportar estos datos consiste en crear un agente de administración basado en archivos para quitar los datos actuales necesarios acerca de un usuario marcado de interés. [Aquí](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/) encontrará un ejemplo del uso de agente de administración basado en archivos.


### <a name="service-and-portal--pam"></a>Servicio y portal y PAM
El servicio y portal junto con PAM pueden exportar estos datos ejecutando una sintaxis de búsqueda basándose en [FIMAutomation PSSnapin]. Puede encontrar el ejemplo [aquí](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) y canalizarlo a [csv](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

PAM puede usar la misma sintaxis anterior o puede utilizar el [módulo MIMPAM](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1), específicamente get-pamuser para buscar el usuario en el entorno de PAM y canalizarlo a csv.

- [Ejemplo de consulta del servicio MIM mediante PowerShell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Los datos de Bhold se pueden exportar utilizando el módulo de informes de bhold en su formato preferido.

### <a name="certificate-management"></a>Administración de certificados
Los datos de administración de certificados relacionados con datos personales están conectados a Active Directory. Un administrador puede exportar estos datos mediante Active Directory PowerShell.

## <a name="updating-personal-data"></a>Actualizar datos personales

Normalmente, los datos personales sobre los usuarios u objetos en las soluciones de MIM se derivan del objeto del usuario en orígenes de datos conectados de la organización. Todos los cambios realizados en el perfil de usuario en el origen de recursos humanos u otro sistema autoritativo del registro, por ejemplo, AD, se reflejan en el servicio de sincronización de MIM.

### <a name="synchronization-service"></a>Servicio de sincronización

Para poder realizar operaciones de administración, los administradores deben formar parte de las operaciones de sincronización o de la administración definidas [aquí](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10)).

La actualización de datos se realiza mediante la definición de reglas desde el origen de autoridad. La consola de administración ayuda a identificar el origen de autoridad para actualizarlo en el origen. Otra opción es crear una regla de sincronización o extensión de regla para controlar la actualización de datos si el origen, como datos de recursos humanos, todavía tiene que mantenerse. Estas son las admitidas disponibles.

Para detalles sobre las diferentes formas de actualizar atributos, consulte la siguiente información. 

- [Using Rules Extensions](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx) (Uso de extensiones de reglas)
- [Understanding Data Synchronization with External Systems](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10)) (Descripción de la sincronización de datos con sistemas externos)

### <a name="service-and-portal--pam"></a>Servicio y portal y PAM

El servicio y portal para incluir datos de PAM se pueden actualizar mediante los cmdlets FIMAutomation o PAM. Si tiene el portal, también puede actualizar directamente mediante la búsqueda y modificación del objeto. Hay que tener en cuenta que la simple actualización desde el portal no implica que se conserve (además, depende de la configuración). Como origen de autoridad, depende en gran medida de configuración general.

### <a name="bhold"></a>BHOLD

Los usuarios se pueden actualizar directamente con la interfaz de usuario de Core de BHOLD o el conector de administración de acceso.

### <a name="certificate-management"></a>Administración de certificados

Los usuarios del servicio de administración de certificados son todos un reflejo de Active Directory. Para realizar la actualización, use Active Directory para cambiar los detalles del objeto.

## <a name="deleting-personal-data"></a>Eliminar datos personales

>[!Note] 
> En este artículo se proporcionan instrucciones sobre las formas de eliminar datos personales de Microsoft Identity Manager y se puede utilizar para dar soporte a sus obligaciones exigidas por el RGPD. Si busca información general sobre RPGD, consulte la [sección del RGPD del Portal de confianza de servicios](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Los datos de MIM se sincronizan y siempre se actualizan desde su origen de datos conectado. Cuando un objeto se elimina en el destino, los datos de dicho objeto en MIM pueden mantenerse para fines de investigación de seguridad. La eliminación del objeto se configura según las reglas de origen de datos conectado o extension de la regla (código) y/o reglas de eliminación de objetos.

### <a name="synchronization-service"></a>Servicio de sincronización
El servicio de sincronización es una de las muchas maneras de controlar o eliminar datos en función de los procesos empresariales. Para ayudar a entenderlo, a continuación figuran algunos artículos para ayudar a comprender las opciones de eliminación y actualización de atributos: 

- [Understanding Deprovisioning](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx) (Descripción del desaprovisionamiento)
- [Using Rules Extensions](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx) (Uso de extensiones de reglas)
- [Procedimientos recomendados de Microsoft Identity Manager 2016](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Servicio y portal y PAM

Se recomienda que para el servicio y el portal mantenga la configuración predeterminada de retención de recursos del sistema de 30 días. Esto indica al servicio cuándo eliminará, no solo los datos de la solicitud, sino también cualquier objeto que se deba borrar del sistema. Una vez que se produce el proceso, se eliminarán todos los datos vinculados a este objeto, lo que incluye todos los datos de registro de autoservicio de restablecimiento de contraseña (SSPR). Esto se reproduce en la configuración de eliminación de objetos anterior. Tenemos una tabla donde almacenamos el GUID de los objetos. Para reducir el tamaño total de la tabla en la compilación 4.4.1459, agregamos un proceso denominado FIM_DeleteExpiredSystemObjectsJob. Puede encontrar detalles sobre este proceso [aquí](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![mim-privacy-compliance-srrc.PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Bhold, al igual que la mayoría de los sistemas conectados al servicio de sincronización, se puede configurar para eliminarse una vez que el objeto de origen, como recursos humanos, se quita. Esto se configura en el agente de administración y se controla mediante las reglas de eliminación de objetos tal y como se describe en las características de servicios de sincronización.

Otra opción es quitar el objeto de usuario directamente desde la interfaz de usuario de Core de BHOLD. Según la configuración, esto podría funcionar correctamente, pero tenga en cuenta que la lógica de aprovisionamiento podría volver a crear este usuario si no se elimina en el origen.
![mim-privacy-compliance-bholdr.PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Administración de certificados
Para quitar un usuario de la administración de certificados, elimínelo en Active Directory.

La administración de certificados solo guardará el UID de perfil de servicios de certificado con sAMAccountName de dominio. Una vez que el usuario se elimina de AD, la memoria caché de usuario solo está presente para los certificados que han inscrito. No se recomienda eliminar nada de la base de datos, ya que puede causar daños generales en el funcionamiento del entorno.

## <a name="opt-out-of-telemetry"></a>Desactivación de la telemetría
Las compilaciones anteriores de FIM/MIM se usaban para recopilar telemetría anonimizada sobre todas las implementaciones y transmitir estos datos a través de HTTPS a servidores de Microsoft. Microsoft usaba estos datos para ayudar a mejorar las versiones futuras de FIM/MIM en el pasado.

>[!Note] 
> En las versiones posteriores a 4.5.x.x , la recopilación de datos se deshabilitará.

Para deshabilitar la recopilación de datos en la versión anterior, ejecute el modo de cambio y anule la selección del símbolo del sistema siguiente:

![mim-privacy-compliance-ceip.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

o edite el Registro y establezca el valor en 0: (Componente)CEIP HKLM\SOFTWARE\Microsoft\Forefront Identity Manager\2010

![mim-privacy-compliance-ceip2.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Pasos siguientes 
- [Centro de seguridad para el motor de base de datos SQL Server y la base de datos SQL Azure](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [Sección de RGPD del Portal de confianza de servicios](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [FIM 2010 Archive: Ramp Up - Implementing Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx) (Archivado de FIM 2010: aumento. Implementación de Forefront Identity Manager 2010)
