---
title: Instalación de BHOLD SP1 | Microsoft Docs
description: Documentación de la instalación de BHOLD SP1
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 11cde4e3b2779f9c32d9849a47713acf5f120b3c
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289703"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Guía de instalación de Microsoft BHOLD Suite SP1 (6.0)

Microsoft® BHOLD Suite Service Pack 1 (SP1) es un grupo de aplicaciones que, cuando se usa con Microsoft Identity Manager 2016 SP1 (MIM), agrega capacidades eficaces de administración de roles, análisis y atestación a MIM. Microsoft BHOLD Suite SP1 consta de los siguientes módulos:

- BHOLD Core
- Access Management Connector
- BHOLD FIM/MIM Integration
- BHOLD Model Generator
- BHOLD Analytics
- BHOLD Reporting
- BHOLD Attestation


> [!NOTE]
> **Se aplica a**: Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>Aspectos tratados en este documento

En este documento se explica cómo planear la implementación de BHOLD para satisfacer sus necesidades de negocio y cómo instalar cada módulo de BHOLD. Para cada módulo se detallan datos relacionados con los requisitos de hardware, software e infraestructura relevantes, la configuración de red previa a la instalación, la información que necesita durante la instalación y los pasos posteriores a la instalación, si los hay.

## <a name="pre-requisite-knowledge"></a>Requisitos previos de conocimientos

En este documento se da por hecho que tiene un conocimiento básico de cómo instalar software en equipos de servidor. También se supone que tiene conocimientos básicos de Active Directory® Domain Services, Microsoft Identity Manager SP1 (FIM) y el software de base de datos Microsoft SQL Server 2008. En este documento no se describe cómo instalar y configurar tecnologías dependientes, como AD DS y FIM. Para más información sobre las funciones que realizan los módulos de Microsoft BHOLD, vea [Microsoft BHOLD Suite Concepts Guide](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx) (Guía de conceptos de Microsoft BHOLD Suite).

## <a name="audience"></a>Público

Este documento está dirigido a planeadores de TI, arquitectos de sistemas, responsables de tecnología, consultores, planeadores de infraestructura y personal de TI que planea implementar Microsoft BHOLD Suite.

## <a name="bhold-infrastructure-considerations"></a>Consideraciones sobre la infraestructura de BHOLD

A menudo, BHOLD y FIM se usan en un entorno de infraestructura de gran tamaño. Puede personalizar la arquitectura de BHOLD y FIM para adaptarla a sus necesidades de negocio concretas. En las secciones siguientes se proporcionan algunas posibles soluciones de arquitectura. En esta introducción no se ofrece una lista completa de todas las opciones posibles, sino que se sugieren modos de implementación de BHOLD en la red.
 
En esta sección se tratan los siguientes temas:

- Arquitectura de un solo servidor
- Arquitectura de dos servidores
- Arquitectura de dos niveles
- Recomendaciones de SQL Server

### <a name="single-server-architecture"></a>Arquitectura de un solo servidor

Para la implementación en organizaciones pequeñas o para fines de desarrollo, puede instalar BHOLD y FIM en el mismo servidor que SQL Server y AD DS, como se muestra abajo.
 
![Arquitectura de un solo servidor](media/bhold-installation-guide/single.png)

Cuando BHOLD Suite SP1 se instala junto con el portal de FIM en un solo servidor, debe crear distintos alias de host (registros A o CNAME) en DNS para BHOLD y para FIM. Esto permite crear distintos nombres de entidad de seguridad de servicio (SPN) para los servicios de BHOLD y de FIM. Para más información, vea [Instalación de BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
Si necesita instrucciones para instalar FIM en una configuración de servidor único, vea [Common Configuration for Getting Started Guides](https://technet.microsoft.com/library/ff575965.aspx) (Configuración común para guías de introducción) en la Biblioteca de TechNet de Microsoft.

### <a name="dual-server-architecture"></a>Arquitectura de dos servidores

Al instalar BHOLD Core y FIM en servidores independientes, consigue un mayor rendimiento y flexibilidad para las organizaciones de tamaño medio que no requieren una implementación más compleja, como la proporcionada por las arquitecturas de varios niveles. En la siguiente ilustración se muestra BHOLD y FIM instalados en sus propios servidores; el servidor de FIM también ejecuta SQL Server para proporcionar servicios de base de datos a BHOLD y FIM. El servicio de sincronización de FIM que se ejecuta en el servidor de FIM sincroniza los cambios entre las bases de datos de FIM y de BHOLD. Tenga en cuenta que si se requiere autoservicio por parte del usuario final, debe instalarse el módulo BHOLD FIM Integration en el mismo servidor que el servicio de FIM y el portal de FIM. El módulo BHOLD FIM Integration requiere que el servicio de FIM y el módulo BHOLD FIM Integration estén instalados en el mismo servidor.

![Arquitectura de dos servidores](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> La característica de informes del módulo BHOLD FIM Integration requiere que las bases de datos de BHOLD y FIM estén instaladas en la misma instancia de SQL Server y la cuenta de servicio de BHOLD debe tener derechos de acceso a la base de datos del servicio de FIM.

### <a name="two-tier-architecture"></a>Arquitectura de dos niveles

En la mayoría de los entornos, sobre todo en aquellos en los que el rendimiento es importante, debe ejecutar BHOLD Suite SP1, FIM y SQL Server en servidores independientes (arquitectura de dos niveles). Con una arquitectura de dos niveles, se dispone de recursos de CPU y de memoria dedicados para cada nivel. En la ilustración siguiente se muestra una posible manera de configurar una arquitectura de dos niveles. El servicio de sincronización de FIM que se ejecuta en el servidor de FIM sincroniza los cambios entre las bases de datos de FIM y de BHOLD. Tenga en cuenta que si se requiere autoservicio por parte del usuario final, debe instalarse el módulo BHOLD FIM Integration en el mismo servidor que el servicio y el portal de FIM.

![Arquitectura de dos niveles](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>Recomendaciones de SQL Server

Si va a implementar BHOLD en una organización grande, le recomendamos seguir estas instrucciones para configurar la base de datos de Microsoft SQL Server:

- Implementar SQL Server en un servidor independiente de cualquier servicio de FIM o de BHOLD.
- Aislar el archivo de registro del archivo de datos en el nivel de disco físico.
- Si usa RAID para proporcionar redundancia de almacenamiento, use el nivel RAID 10 (1 + 0). No use el nivel RAID 5.
- Asegúrese de establecer la configuración correcta cuando use más de 2 GB de memoria física para el servidor que ejecuta SQL Server.
- Para un rendimiento óptimo de BHOLD, use Microsoft SQL Server 2008 R2 o una versión posterior.

Para más información sobre los procedimientos recomendados de SQL Server, vea [Storage Top 10 Best Practices](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) (Diez mejores procedimientos recomendados de almacenamiento) en la Biblioteca de TechNet de Microsoft.

### <a name="trusted-certificates-list-update"></a>Actualización de lista de certificados de confianza

Se puede configurar Windows para que valide cadenas de certificados antes de iniciar un servicio. En sistemas con esta configuración, no se puede iniciar un servicio si el código ejecutable del servicio se firmó con un certificado que no se encuentra en la lista de certificados de confianza (TCL) del servidor. El código del software de Microsoft BHOLD Suite SP1 se firma mediante una cadena de certificados de firma de código que se origina con el certificado de la entidad de certificación raíz de Microsoft 2010.
Se puede configurar Windows para que recupere certificados raíz de Microsoft a través de una conexión a Internet. Pero en un sistema desconectado, Windows Server incluye solo aquellos certificados que estaban presentes en el programa con anterioridad al lanzamiento de Windows. En las versiones de Windows Server anteriores a Windows Server 2010, estos certificados no incluirán el certificado raíz necesario para validar la cadena de certificados de firma de código de BHOLD Suite SP1. Si piensa instalar uno o más módulos de Microsoft BHOLD Suite SP1 en un sistema que podría no tener un TCL actualizado, antes de instalar un módulo de BHOLD Suite SP1 debe descargar e instalar la actualización raíz o usar la directiva de grupo para instalar la actualización raíz. Para más información, vea [Configurar raíces de confianza y certificados no permitidos](http://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>Paso necesario de la instalación de BHOLD Suite SP1 en Windows Server 2012/2016 

![IIS instala BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

Si instala BHOLD Suite SP1 en Windows Server 2012 o 2016, las páginas web de BHOLD no estarán disponibles hasta que modifique el archivo applicationHost.config ubicado en ```C:\Windows\System32\inetsrv\config```. En la sección ```<globalModules>```, agregue ```preCondition="bitness64``` a la entrada que comienza por ```<add name="SPNativeRequestModule"``` para que quede como sigue:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Después de editar y guardar el archivo, ejecute el comando iisreset para restablecer el servidor IIS.


## <a name="upgrading-bhold-suite"></a>Actualizar BHOLD Suite

No se puede actualizar una instalación existente de BHOLD Suite. Primero es necesario desinstalar una instalación existente de BHOLD Suite para poder actualizar los módulos de BHOLD. Si tiene un modelo de roles de BHOLD existente, puede actualizar la base de datos de BHOLD y usarla cuando instale el módulo BHOLD Core actualizado. Para más información, vea [Replacing BHOLD Suite with BHOLD Suite SP1](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx) (Reemplazar BHOLD Suite con BHOLD Suite SP1).


## <a name="next-steps"></a>Pasos siguientes

- [Referencia para desarrolladores de BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historial de versiones de BHOLD](../reference/version-bhold-history.md)
