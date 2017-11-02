---
title: "Guía de topología para implementación | Microsoft Docs"
description: "Conozca los componentes de MIM 2016 y reciba sugerencias acerca de cómo implementarlas en su entorno."
keywords: 
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e257a2e64225a4bc545d8a9384167819412e939b
ms.sourcegitcommit: f077508b5569e2a96084267879c5b6551e1e0905
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2017
---
# <a name="topology-considerations"></a>Consideraciones relativas a la topología
Puede implementar los componentes de Microsoft Identity Manager (MIM) en el mismo servidor o entre varios servidores en diversas configuraciones. La topología que seleccione para la implementación afecta al rendimiento que se puede lograr de MIM. Este artículo presenta varias topologías de implementación que puede utilizar.


>[!NOTE]
Estas opciones son aplicables a las implementaciones que usan exclusivamente la sincronización de MIM, el servicio MIM y el portal de MIM para la administración de identidades.  Las implementaciones que usan MIM CM, el conjunto de aplicaciones BHOLD de MIM o Privileged Access Management tienen otras opciones de implementación.


## <a name="mim-components"></a>Componentes de MIM
Al diseñar la topología de implementación, es importante saber lo que hace y cómo interactúa cada componente.

- <a name="mim-portal---an-interface-for-password-resets-group-management-and-administrative-operations"></a>**Portal de MIM**: una interfaz para los restablecimientos de contraseña, la administración de grupos y las operaciones administrativas.
    -
- **Servicio MIM**: un servicio web que implementa la funcionalidad de administración de identidades de MIM 2016.
- **Servicio de sincronización de MIM**: sincroniza los datos con otros sistemas de identidad.
- **Microsoft SQL Server**: tanto el servicio MIM como el servicio de sincronización de MIM almacenan los datos en bases de datos SQL.

La siguiente tabla muestra las opciones para hospedar cada uno de los componentes de MIM. Pueden hospedarse en el mismo equipo o distribuirse en varios servidores y clústeres.

| | Portal MIM | Servicio MIM | Servicio de sincronización de MIM | SQL Server |
| --- | --- | --- | --- | --- |
| Mismo equipo | Sí | Sí | Sí | Sí |
| Servidor distinto | Sí | Sí | Sí | Sí |
| Clúster de equilibrio de carga de red | Sí | Sí | | |
| Clúster de servidores | | | | Sí |


## <a name="multitier-topology"></a>Topología de varios niveles
La topología de varios niveles es la más utilizada. Ofrece la máxima flexibilidad. El portal de MIM, el servicio MIM y las bases de datos se dividen en niveles y se implementan en varios equipos. Esta topología aporta flexibilidad a la escalabilidad de los diferentes componentes de MIM. Por ejemplo, puede escalar el portal de MIM en horizontal agregando servidores adicionales en un clúster de equilibrio de carga de red (NLB). Del mismo modo, puede escalar el servicio MIM mediante un clúster NLB y aumentando el número de equipos (nodos) del clúster según corresponda.

En la topología de varios niveles, se asigna un equipo dedicado para hospedar cada base de datos SQL (uno para el servicio MIM y otro para el servicio de sincronización de MIM). La escalabilidad del rendimiento de los equipos que hospedan las bases de datos SQL puede aumentarse agregando hardware o actualizándolo; por ejemplo, mediante la actualización de las CPU, la adición de CPU adicionales, el aumento de la capacidad de la memoria de acceso aleatorio (RAM) o la actualización de esta, o bien la actualización de las configuraciones de disco duro para incrementar el acceso de lectura y escritura y reducir la latencia.

![Diagrama de la topología de varios niveles de MIM](media/MIM-topo-multitier.png)

En esta configuración, el servicio de sincronización de MIM y su base de datos se hospedan en el mismo equipo. Sin embargo, debería poder lograr un rendimiento similar si existe una conexión de red dedicada de un gigabit entre el servicio de sincronización de MIM y su base de datos cuando se hospedan en equipos distintos.


## <a name="multitier-topology-with-multiple-mim-services"></a>Topología de varios niveles con diversos servicios MIM
Sincronizar datos con sistemas externos puede llevar mucho tiempo y agregar una carga considerable para el sistema durante ese período. Si la configuración de sincronización deriva en la activación de directivas con flujos de trabajo, estas se disputarán los recursos con los flujos de trabajo de usuario final. Estos problemas pueden agravarse con flujos de trabajo de autenticación, como el restablecimiento de contraseñas, que se realizan en tiempo real mientras un usuario final espera a que finalice el proceso. Si proporciona una instancia del servicio MIM para operaciones de usuario final y un portal independiente para la sincronización de datos administrativos, podrá obtener una mejor capacidad de respuesta en las operaciones de usuario final.

![Diagrama de topología de varios niveles de diversos servicios MIM](media/MIM-topo-multitier-multiservice.png)

Igual que con la topología estándar de varios niveles, puede aumentar el rendimiento del portal de MIM mediante un clúster NLB e incrementando el número de nodos del clúster según sea necesario.

Los equipos en los que se ejecuta SQL Server y que hospedan MIM Synchronization Service y la base de datos del Servicio MIM influirán drásticamente en el rendimiento general de la implementación de MIM. Por lo tanto, siga las recomendaciones de la documentación de SQL Server para optimizar el rendimiento de la base de datos. Para obtener más información, consulte los siguientes documentos:

## <a name="see-also"></a>Consulte también
- Puede descargar el documento [Forefront Identity Manager (FIM) 2010 Capactity Planning Guide](http://go.microsoft.com/fwlink/?LinkId=200180) (Guía de planeación de la capacidad de Forefront Identity Manager 2010 [FIM]); en él se ofrecen más detalles sobre la compilación de prueba y los resultados de pruebas de rendimiento.
