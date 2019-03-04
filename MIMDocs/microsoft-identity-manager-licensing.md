---
title: Administración de licencias y descargas de Microsoft Identity Manager | Microsoft Docs
description: En este artículo se describen los enfoques para la administración de licencias de Microsoft Identity Manager (MIM) 2016, con indicaciones a los puntos de descarga del software.
keywords: ''
author: markwahl-msft
ms.author: markwahl-msft
manager: femila
ms.date: 02/25/2019
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.reviewer: billmath
ms.suite: ems
ms.openlocfilehash: 9e74b7ccf332c1572ce395fad18b8629673c756a
ms.sourcegitcommit: 4f0b2883922bcb8fbef6b4284c35c6ca62c11565
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56954535"
---
# <a name="microsoft-identity-manager-2016-licensing-and-downloads"></a>Administración de licencias y descargas de Microsoft Identity Manager 2016

En este artículo se describen los enfoques para la administración de licencias de Microsoft Identity Manager (MIM) 2016, con indicaciones a los puntos de descarga del software.

## <a name="licensing-mim-for-your-organization"></a>Administración de licencias de MIM para su organización

Las licencias de Microsoft Identity Manager 2016 se conceden por usuario.  Los detalles sobre las licencias están incluidos en los términos del producto y documentos relacionados, que pueden descargarse en la página de [términos de licencia](https://www.microsoft.com/en-us/licensing/product-licensing/products.aspx).

### <a name="licensing-for-azure-ad-premium-customers"></a>Administración de licencias para los clientes de Azure AD Premium

Microsoft Identity Manager 2016 se incluye con Azure Active Directory Premium (P1 y P2), que forma parte de Enterprise Mobility + Security.

Azure AD Premium está disponible a través de un [contrato Enterprise de Microsoft](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), el [programa de licencias por volumen abierto](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx) y el programa de [proveedores de soluciones en la nube](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409). Los suscriptores de Azure y Office 365 también pueden comprar Azure Active Directory Premium P1 y P2 en línea.  Obtenga más información en la [página de precios de Azure Active Directory](https://azure.microsoft.com/en-us/pricing/details/active-directory/).

### <a name="mim-cals"></a>CAL de MIM

Si no tiene suscripciones de Azure Active Directory Premium para los usuarios y está usando otras funcionalidades de MIM aparte de la sincronización, se necesita una [licencia de acceso de cliente (CAL)](https://www.microsoft.com/en-us/licensing/product-licensing/client-access-license.aspx) para cada usuario cuya identidad se administre en MIM. Si desea que usuarios externos —como socios comerciales, contratistas externos o clientes— tengan acceso a MIM, puede adquirir CAL para cada uno de los usuarios externos o bien adquirir licencias de External Connector (EC). Las CAL de Microsoft Identity Manager 2016 no se necesitan para los usuarios cuya identidad está solo en el servicio de sincronización de Microsoft Identity Manager y no se administra en ningún otro componente de MIM.

### <a name="licenses-for-platform-components"></a>Licencias para los componentes de plataforma

Se necesita una licencia de Windows Server para usar el software de servidor de Microsoft Identity Manager 2016 como un complemento de Windows Server. Y una implementación de MIM también requiere una instalación de SQL Server.  Las licencias de Windows Server y SQL Server no están incluidas en MIM.

## <a name="obtaining-mim-software"></a>Obtención del software de MIM

Antes de iniciar una nueva instalación de MIM o una actualización desde una versión anterior, asegúrese de que tiene las versiones más recientes.

Si va a iniciar una instalación nueva, deberá descargar los archivos de instalación para cada componente de MIM que sea pertinente para su escenario. A continuación, descargue las actualizaciones para esos archivos y, luego, descargue los componentes adicionales que sean descargas independientes desde el Centro de descarga.


| Escenario | Componente | ¿Se necesita para su situación? | Nombre de carpeta ISO de DVD | Comentarios |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|Sincronización| Servicio de sincronización (incluido el conector a AD) | Sí | `Synchronization Service` | |
| Sincronización | PCNS | No | `Password Change Notification Service` |  Para instalar en controladores de dominio |
| Sincronización | Conectores para LDAP, SQL, Servicios web, PowerShell, Lotus Domino, Graph | No | No aplicable | Distribuido a través del Centro de descarga |
| Privileged Access Management | Servicio MIM | Sí | `Service and Portal` | |
| Autoservicio | Servicio y portal de MIM | Sí | `Service and Portal` | |
| Autoservicio | Extensiones y complementos | No | `Add-ins and extensions` | Para instalar en los equipos del usuario final |
| Autoservicio | Informes de SCSM | No | `Data Warehouse Support Scripts` | |
| Autoservicio | Agente de informes híbridos | No | No aplicable | Distribuido a través del Centro de descarga |
| Autoservicio | Paquetes de idioma | No | `LANGUAGE Packs` | |
| Administración de certificados | CM | Sí | `Certificate Management` | |
| Administración de certificados | Cliente en masa de CM | No | `CM Bulk Client` | |
| Administración de certificados | Cliente de CM | No | `CM Client`  | |
| Administración de certificados | Aplicación de CM para Windows | No | `FIMCMModernApp*` | | |

### <a name="obtaining-windows-installer-packages"></a>Obtención de paquetes de Windows Installer

En el caso de una instalación nueva, la mayoría de las organizaciones descargan los paquetes de instalación de MIM desde el [Centro de servicios de licencias por volumen](https://www.microsoft.com/licensing/servicecenter/default.aspx). 


El archivo ISO de DVD contiene una carpeta para cada componente de MIM: servicio de sincronización, servicio y portal, etc. Si va a instalar el software en un equipo diferente del equipo en que efectuó la descarga, asegúrese de copiar todo el archivo ISO o la carpeta para el componente: no se limite a copiar un único archivo MSI de una carpeta sin el resto de los archivos y subcarpetas.

Si no tiene acceso al Centro de servicios de licencias por volumen, los clientes con una suscripción de desarrollador adecuada también pueden descargar MIM 2016 SP1 como un archivo ISO de las [descargas de mis ventajas de Visual Studio](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%201&pgroup=).  Busque "Microsoft Identity Manager 2016 con Service Pack 1".  

Si no tiene acceso al Centro de servicios de licencias por volumen y simplemente desea probar el software MIM durante un tiempo limitado, puede descargar una [versión de evaluación de MIM 2016](https://www.microsoft.com/en-us/download/details.aspx?id=48244). Este software no está pensado para su uso en producción y dejará de funcionar 180 días después de la primera instalación, sin posibilidad de actualización. La versión de evaluación requiere Windows Server 2008 R2, Windows Server 2012 o Windows Server 2012 R2 para la instalación.  Si no está familiarizado con MIM y está aprendiendo la tecnología, tenga en cuenta que todos los escenarios de MIM requieren un dominio de Active Directory, un servidor de Windows Server y SQL Server. Si no dispone de Windows Server o SQL Server, le recomendamos que pruebe a [aprovisionar una máquina virtual con SQL Server 2016 y Windows Server 2016](https://azure.microsoft.com/en-us/blog/azure-images-sql-server-2016-on-windows-server-2016/).

### <a name="obtaining-updates"></a>Obtención de actualizaciones

Después de instalar MIM desde MSI, debe instalar las revisiones necesarias.

Compruebe el [historial de versiones de Identity Manager](./reference/version-history.md) para ver cuál es la versión de actualización más reciente, que tiene un vínculo al sitio de descarga para los archivos de revisión del instalador.

Para determinar qué archivos de actualización son necesarios, esta tabla enumera los componentes y el nombre del archivo de revisión correspondiente (MSP) en una actualización.

| Escenario | Componente | Nombre de carpeta ISO de DVD | Nombre de archivo de revisión de actualización correspondiente |
|----------|-----------|-   |-------------------|----------|--------------|
|Sincronización| Servicio de sincronización | `Synchronization Service` | `FIMSyncService_x64*.msp` |
| Autoservicio | Servicio y portal de MIM | `Service and Portal` | `FIMService_x64*msp` |
| Autoservicio | Extensiones y complementos | `Add-ins and extensions` | `FIMAddinsExtensions*msp` |
| Autoservicio | Paquetes de idioma | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| Administración de accesos (BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| Administración de certificados | CM |  `Certificate Management` | `FIMCM*.msp` |
| Administración de certificados | Cliente en masa de CM |  `CM Bulk Client` |`FIMCMBulkClient*msp` |
| Administración de certificados | Cliente de CM | Cliente de CM |`FIMCMClient*msp` |

Asegúrese de leer las notas de la versión asociadas a la actualización antes de instalar el archivo MSP.

Las actualizaciones de [BHOLD](https://www.microsoft.com/en-us/download/details.aspx?id=55950) no se distribuyen como archivos MSP, solo como instaladores MSI.

### <a name="additional-downloads"></a>Descargas adicionales

Las descargas siguientes también pueden ser pertinentes:

- [Agente de informes híbridos de MIM](https://www.microsoft.com/download/details.aspx?id=55112)

- [Conector LDAP genérico, conector de SQL genérico, conector de Graph, conector de Lotus Domino, conector de PowerShell, conector de servicios web](http://go.microsoft.com/fwlink/?LinkId=717495)

- [Conector para el almacén del perfil de usuario de SharePoint](https://www.microsoft.com/en-us/download/details.aspx?id=41164)

- Si aún no tiene un dominio de Active Directory y está configurando un escenario de PAM con fines de experimentación, consulte los [scripts de implementación de PAM de MIM 2016 SP1](sp1-deployment-scripts.md).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre escenarios posibles de [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).
- Lea la [guía de planificación de capacidad](capacity-planning-guide.md).
- Implemente MIM para un [escenario de sincronización](microsoft-identity-manager-deploy.md) o el [escenario de administración de acceso con privilegios](./pam/privileged-identity-management-for-active-directory-domain-services.md).

