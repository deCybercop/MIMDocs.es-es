---
title: Guía de planeamiento de capacidad | Microsoft Docs
description: Use esta guía para comprender las variables que deben considerarse antes de implementar MIM 2016, incluidos los niveles de carga y las decisiones de directivas.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 32cdf03ffa0d0d282a6277af766f97e93e3a3f3a
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289020"
---
# <a name="capacity-planning-guide"></a>Guía de planificación de capacidad

Microsoft Identity Manager (MIM) le permite crear, actualizar y quitar cuentas de usuario en toda la organización. También les ofrece a los usuarios finales la capacidad de administrar sus propias características de autoservicio de cuentas. Incluso en un entorno pequeño, todas estas acciones pueden sumar rápidamente.

Antes de comenzar a trabajar con MIM, use esta guía, junto con los entornos de prueba, para comprender el ámbito adecuado para la implementación. Este artículo le guía a través de muchos factores comunes que debe tener en cuenta. Aunque cada implementación es única, probar los escenarios en un laboratorio sigue siendo la mejor manera de determinar los servidores, hardware o topologías adecuados para sus necesidades.

Si aún no está familiarizado con MIM 2016 y sus componentes, infórmese sobre [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md) antes de continuar.

## <a name="overview"></a>Introducción

Existe una serie de factores que puede afectar a la capacidad y al rendimiento general de la implementación de Microsoft Identity Manager:

- Formas de implementar físicamente los componentes de MIM (topología).
- Hardware en el que se ejecutan dichos componentes.
- La cantidad y la complejidad de los objetos de configuración de directivas de MIM son factores importantes que se deben tener en cuenta al planear la capacidad.
- La escala prevista de la implementación y la carga esperada son normalmente factores más evidentes que afectan al rendimiento y a la capacidad.

En la tabla siguiente se enumeran los principales factores que afectan a la capacidad y el rendimiento de una implementación de MIM 2016:

| Factor de diseño | Consideraciones |
| ------------- | -------------- |
| Topología | La distribución de los servicios MIM en equipos de la red. |
| Hardware | Hardware físico (físico o virtual) de cada componente de MIM, como CPU, memoria, adaptador de red y configuración de unidad de disco duro. |
| Objetos de configuración de directivas de MIM | La cantidad y el tipo de objetos de configuración de directivas de MIM, que incluyen conjuntos, reglas de directivas de administración (MPR) y flujos de trabajo. |
| Escalar | Usuarios, grupos, grupos calculados y tipos de objetos personalizados para administrarlos con MIM 2016. Además, tenga en cuenta la complejidad de los grupos dinámicos y asegúrese de tener presente el anidamiento de grupos. |
| Cargar | Frecuencia de uso. Operaciones como la creación de grupos o usuarios, el restablecimiento de contraseñas o visitas al portal por minuto u hora. Tenga en cuenta que la carga puede variar en el transcurso de una hora, un día, una semana o un año. En función del componente, podrá adaptar el diseño para cargas máximas o cargas medias. |

## <a name="hosting-microsoft-identity-manager-components"></a>Hospedaje de componentes de Microsoft Identity Manager

Los componentes de Microsoft Identity Manager no tienen que estar ubicados en el mismo equipo. Pensar sobre estos componentes y las máquinas físicas o virtuales que los hospedarán es una parte importante del planeación de la capacidad.

Los factores de hardware pueden afectar al rendimiento del entorno de MIM. Por ejemplo:

- ¿Qué es la configuración de disco físico para el equipo que ejecuta la Base de datos de SQL del Servicio MIM 2016? El número de ejes que componen la configuración de disco o la distribución de los archivos de registro y de datos pueden afectar considerablemente al rendimiento del sistema.

Además, tenga en cuenta los factores externos en la configuración. Por ejemplo:

- Si usa una SAN como configuración de la base de datos del servicio MIM 2016, ¿qué otras aplicaciones comparten dicha SAN? Estas aplicaciones pueden afectar al rendimiento de bases de datos si compiten por los recursos de disco compartido en la SAN.

## <a name="users-and-groups"></a>Usuarios y grupos

El número de usuarios y grupos de su entorno es una consideración habitual al pensar en la escala de una implementación. Sin embargo, existen otras consideraciones pertinentes que también debe tener en cuenta para la planificación.

- ¿Pueden crear grupos los usuarios? Si es así, sopese la posibilidad de calcular de qué forma afectará que los usuarios creen nuevos grupos al crecimiento de grupos del entorno.

- ¿Se implementarán grupos dinámicos? Averigüe cuántos y qué tipos de grupos dinámicos se esperan en el entorno.

## <a name="expected-load-levels"></a>Niveles de carga previstos

También debe pensar en el tipo de carga que se asignará a los componentes de MIM. Debe calcular la carga examinando las aplicaciones actuales en su entorno. Estas son algunas de las cuestiones importantes que debe tener en cuenta:

- ¿Con qué frecuencia espera que una solicitud se una a un grupo o lo abandone?

- ¿Con qué frecuencia prevé que un usuario cree un grupo estático o dinámico?

- ¿Cuántas operaciones no controladas por los usuarios espera, como la sincronización de cambios de sistemas externos? Asegúrese de que dispone de capacidad suficiente para la carga que genera la sincronización de información de identidades con sistemas externos.

- ¿Qué tipo de escenarios planea implementar? La existencia de diferentes escenarios supone diferentes patrones de carga. Por ejemplo, los equipos cliente con el cliente de MIM 2016 instalado comprueban periódicamente si es necesario el registro al iniciar sesión.

- ¿Espera grandes variaciones en los niveles de carga, de carga normal a máxima? Por ejemplo, suele haber varios restablecimientos de contraseña después de períodos de vacaciones. Asegúrese de llevar a cabo las programaciones de sincronización y mantenimiento del sistema fuera de los picos de uso previstos. Cuando planifique la capacidad, asegúrese de tener en cuenta los períodos de carga máxima.

## <a name="policy-configuration-objects"></a>Objetos de configuración de directivas

Los objetos de configuración de directivas de MIM incluyen MPR, conjuntos, flujos de trabajo y reglas de sincronización para implementaciones. Las implementaciones de MIM son únicas para cada cliente porque la configuración de directiva cambia para ajustarse a las necesidades de cada implementación. Estos objetos de configuración de directivas de MIM conforman algunas de las consideraciones de rendimiento más importantes:

- **Conjuntos** Cada operación del sistema se debe evaluar con las pertenencias a conjuntos y las actualizaciones de estos que provocan cambios en la pertenencia a conjuntos. Por ejemplo, es posible que un cambio en el número de edificio de la oficina de una persona no suponga un gran impacto. Sin embargo, otros cambios pueden suponer impactos en cascada, como el cambio de un administrador, que puede afectar a varios objetos en varios niveles.

- **Reglas de directiva de administración** Las MPR administran las reglas de control de acceso y activan flujos de trabajo. La creación de MPR puede generar la necesidad de aumentar el número de conjuntos de manera que pueda capturar distintos estados de transición de objeto. Puede que estos conjuntos adicionales activen flujos de trabajo adicionales, y que cada flujo de trabajo asigne solicitudes únicas en el sistema. Como consecuencia, esto supone otro elemento que debe incluir en la planificación de la capacidad.

La configuración de directiva de MIM también incluye decisiones sobre el aprovisionamiento en el entorno. Asegúrese de pensar en lo siguiente:

- ¿Va a aprovisionar principios de seguridad externos en varios bosques de servicios de dominio de Active Directory (AD DS)? Si lo hace, se generarán más solicitudes y flujos de trabajo, lo que supondrá una carga adicional para el sistema.

- ¿Usará aprovisionamiento sin código? Si es así, afectará al número de entradas de reglas previstas, así como a las solicitudes asociadas y a los flujos de trabajo del sistema.

## <a name="next-steps"></a>Pasos siguientes

- [Consideraciones de topología a la hora de implementar MIM](topology-considerations.md)
- Puede descargar el documento [Forefront Identity Manager (FIM) 2010 Capacity Planning Guide](http://go.microsoft.com/fwlink/?LinkId=200180) (Guía de planeación de la capacidad de Forefront Identity Manager 2010 [FIM]). En él se ofrecen más detalles sobre la compilación de prueba y los resultados de pruebas de rendimiento.
