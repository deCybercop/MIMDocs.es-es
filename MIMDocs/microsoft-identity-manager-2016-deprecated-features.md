---
title: "Características en desuso de MIM y planeación para el futuro | Microsoft Docs"
description: "En este artículo se documentan las características en desuso de MIM Identity Manager 2016 SP1."
keywords: 
author: barclayn
ms.author: davidste
manager: mbaldwin
ms.date: 1/31/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: d1e016c45261be5fa9c4dba67ead7f88aa14890b
ms.sourcegitcommit: 24746cf23b4688b3f8290519527259fc469e0373
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2018
---
# <a name="deprecated-features"></a>Características en desuso

En este artículo se describen las características en desuso de Microsoft Identity Manager 2016 SP1. Si la característica todavía está presente en Microsoft Identity Manager, implica que sigue recibiendo soporte. No se recomienda el uso de características en nuevas implementaciones, ya que es posible que se quiten en próximas versiones.  En el caso de los desarrolladores, no se recomienda el uso de características en desuso en ninguna aplicación ni solución nueva.

>[!NOTE]
Las características y funcionalidades quitadas en Microsoft Identity Manager SP1 están marcadas con dos asteriscos (**). <br>
Para obtener más información sobre soporte, consulte el [ciclo de vida de Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010).


## <a name="bhold"></a>BHOLD 

Microsoft no recomienda a los clientes que inicien nuevas implementaciones de los componentes del conjunto de aplicaciones Microsoft BHOLD. Las implementaciones existentes de BHOLD seguirán recibiendo soporte. Ahora, Azure AD proporciona [revisiones de acceso](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), que reemplazan algunas de las características de campaña de atestación de BHOLD.

## <a name="certificate-management"></a>Administración de certificados 
| **Categoría**                | **Característica en desuso**              | **Reemplazo y comentario**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Agentes de administración | **FIM Certificate Management | El agente FIM Certificate Management se ha quitado en MIM 2016.                                                             |

## <a name="service-and-portal"></a>Servicio y portal

| **Categoría**                | **Característica en desuso**              | **Reemplazo y comentario**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Configuración mediante programación | Interfaz de configuración del servicio web (ma-data y mv-data) | La posibilidad de configurar el servicio de sincronización de FIM mediante el servicio web de FIM se quitará en la siguiente versión.
|

## <a name="synchronization-service"></a>Servicio de sincronización 

| **Categoría**                | **Característica en desuso**              | **Reemplazo y comentario**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Configuración mediante programación | Interfaz de configuración del servicio web | La posibilidad de configurar el servicio de sincronización de FIM mediante el servicio de FIM se quitará en la siguiente versión.                                                          |
| Agentes de administración           | Agentes de administración integrados                        | Se han quitado los siguientes agentes de administración de MIM 2016: </br> 1. ** Agente de administración para FIM Certificate Management </br>2. ** Agente de administración para Lotus Notes</br> 3. ** Agente de administración para SAP R/3 </br> Los agentes de administración para Lotus Notes y SAP R/3 se han reemplazado por nuevas versiones. Para obtener más información, vea [Historial de versiones de conectores](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history).                                                                                                                                                                                                                                              |
| Agentes de administración           | ECMA1                               | El marco de extensibilidad de ECMA1/XMA se ha reemplazado por la versión 2.0 de ECMA. Se requiere la actualización de agentes de administración existentes de ECMA1 con conectores de ECMA 2.0.                                                                                                                                          |
| Agentes de administración           | Ejecución de conectores fuera de procesos      | Esta característica no se reemplazará. El servicio de sincronización llamará siempre al conector en el mismo proceso. Iniciar y administrar el otro proceso es responsabilidad del conector. |
| Agentes de administración           | Configuración del nombre para mostrar de la partición    | Esta característica no se reemplazará. Esta opción solo se usaba para proporcionar un nombre alternativo para una partición en las interfaces de WMI.                                                                                                                                                                       |
| Perfiles de ejecución                | Perfiles combinados                   | Se quitarán los perfiles combinados importación diferencial/sincronización, importación completa/sincronización diferencial e importación completa/sincronización. En su lugar debe usar perfiles de ejecución con dos pasos. 

>[!NOTE]
Solo debe mantener los perfiles de ejecución combinados en entornos en los que el rendimiento se vaya a ver afectado por un gran número de desconectores existentes.


| **Categoría**                | **Característica en desuso**              | **Reemplazo y comentario**           |
|--------|-------|---|    
| Prioridad de los atributos | Varios dominios/misma prioridad                       | Se quitará la misma prioridad. Esta característica no tendrá ningún reemplazo. Debe configurar la prioridad manual en su lugar. Puede seguir usando esta característica si su entorno tiene implementado un agente de administración del servicio FIM. Este agente de administración no proporciona prioridad manual para evitar export-no-precedent para el aprovisionamiento declarativo. |
| Reglas de unión           | Unión en el tipo de objeto "Any"                             | Esta característica no se reemplazará. Todas las reglas de unión deberían definir explícitamente el tipo de objeto de metaverso al que se están intentando unir.       |
| Flujos de atributo      | Anule la selección de "Permitir valores null" para los valores exportados            | Esta característica no se reemplazará. "Permitir valores null" siempre estará seleccionada. Debe asegurarse de haber seleccionado "Permitir valores null" en el entorno actual.  |
| Flujos de atributo      | "No recuperar atributos"                            | Esta característica no se reemplazará. Los atributos siempre se recuperarán, que es el procedimiento recomendado.  |
| Extensión de reglas      | Ejecución del metaverso y extensión de reglas de agente de administración fuera de procesos | Esta característica no se reemplazará. Las reglas de flujo de atributos y metaverso se ejecutarán en el mismo proceso que el motor de sincronización.       |
| Extensión de reglas      | Propiedades de transacción                                | Esta característica no se reemplazará. Debe evitar el paso de datos entre sincronización saliente, de aprovisionamiento y entrante con esta clase de utilidad.  |
| Extensión de reglas      | ExchangeUtils: métodos Create55\*                     | Los métodos para crear objetos para servidores de Exchange 5.5 se quitará.        |
| Interfaz            | Mms_Metaverse                                        | Todos los miembros de clase ClmUtils se quitarán en una versión próxima.   |

## <a name="next-steps"></a>Pasos siguientes
Más información acerca de:

- Microsoft Identity Manager sigue estrechamente relacionado con su predecesor, Forefront Identity Manager. Si sigue usando FIM o quiere consultar documentación adicional, eche un vistazo a la [Guía básica de documentación de FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Consideraciones de topología a la hora de implementar MIM](topology-considerations.md) Este artículo presenta varias topologías de implementación que pueden utilizarse.
- [Guía de planeación de capacidad](capacity-planning-guide.md) Puede usar esta guía con los entornos de prueba para comprender el ámbito adecuado de la implementación.
