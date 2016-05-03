---
# required metadata

title: Guía de planificación de capacidad | Microsoft Identity Manager
description: Use esta guía para comprender las variables que deben considerarse antes de implementar MIM 2016, incluidos los niveles de carga y las decisiones de directivas.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Guía de planificación de capacidad

Esta guía se ha creado para ayudar en la planificación de clientes, pero no debe usarse únicamente para determinar el hardware, las topologías o los servidores adecuados que se requieren para implementar Microsoft Identity Manager (MIM). Se recomienda a las organizaciones y se espera de ellas que configuren sus propios entornos de prueba para calcular la capacidad y el rendimiento con más exactitud. Microsoft no garantiza a las organizaciones las mismas características de capacidad o rendimiento, incluso si los componentes de MIM 2016 se han implementado y configurado de forma idéntica a los componentes que se describen en esta guía.

Para preparar correctamente la implementación de MIM, simule el entorno de producción previsto en un ámbito experimental y póngalo a prueba. Puede decidir si desea probar diferentes topologías en ejecución en distintos tipos de hardware y, a continuación, poner en marcha distintas pruebas de escenarios de escala y de carga que pueden ayudarle a calcular mejor lo que puede ocurrir al implementar MIM 2016 en su entorno.


## Información general
Existe una serie de variables que puede afectar a la capacidad y al rendimiento general de la implementación de Microsoft Identity Manager. Las formas en las que implementa físicamente los componentes de MIM (topología), así como el hardware en el que dichos componentes se ejecutan, son factores importantes para determinar el rendimiento y la capacidad que puede esperar de la implementación de MIM. La cantidad y la complejidad de los objetos de configuración de directivas de MIM pueden resultar menos obvias, pero siguen siendo factores importantes que se deben tener en cuenta al planear la capacidad. Finalmente, la escala prevista de la implementación y la carga que espera asignarle son normalmente factores más evidentes que afectan al rendimiento y capacidad.

En la tabla siguiente se describen los principales factores que afectan a la capacidad y al rendimiento que se puede esperar de una implementación de MIM 2016.

| Factor de diseño | Consideraciones |
| ------------- | -------------- |
| Topología | La distribución de los servicios MIM en equipos de la red. Por ejemplo, ¿se alojará el servicio de sincronización de MIM 2016 en el mismo equipo que hospeda la base de datos? |
| Hardware | Las especificaciones de hardware físico y de cualquier hardware virtualizado que ejecute para cada componente de MIM. Esto incluye la CPU, la memoria, los adaptadores de red y la configuración de la unidad de disco duro. |
| Objetos de configuración de directivas de MIM | La cantidad y el tipo de objetos de configuración de directivas de MIM, que incluyen conjuntos, reglas de directivas de administración (MPR) y flujos de trabajo. Por ejemplo, ¿cuántos flujos de trabajo se activan para las operaciones? ¿Cuántas definiciones de conjuntos existen y cuál es la complejidad relativa de cada una de ellas? |
| Escalar | La cantidad de usuarios, grupos, grupos calculados y tipos de objetos personalizados, como los equipos que MIM 2016 administrará. Además, tenga en cuenta la complejidad de los grupos dinámicos y asegúrese de tener presente el anidamiento de grupos. |
| Cargar | Frecuencia de uso previsto. Por ejemplo, la cantidad de nuevos grupos o de usuarios creados, contraseñas restablecidas o visitas al portal en un período de tiempo determinado. Tenga en cuenta que la carga puede variar en el transcurso de una hora, un día, una semana o un año. En función del componente, deberá adaptar el diseño para cargas máximas o cargas medias.


## Hospedaje de componentes de Microsoft Identity Manager
Microsoft Identity Manager presenta muchos componentes distintos. En muchos casos, estos componentes no se encuentran ubicados en el mismo equipo. Cuando piensa en MIM desde el punto de vista de la capacidad, los componentes y los equipos físicos (y, posiblemente, máquinas virtuales) en los que se hospedan son consideraciones importantes. Existen muchos factores potenciales que pueden afectar al rendimiento del entorno de MIM; por ejemplo, la configuración de disco físico para el equipo en el que se ejecuta la base de datos SQL del servicio MIM 2016. El número de ejes que componen la configuración de disco o la distribución de los archivos de registro y de datos pueden afectar considerablemente al rendimiento del sistema. Además, tenga en cuenta los factores externos en la configuración. Si usa una SAN como configuración de la base de datos del servicio MIM 2016, ¿qué otras aplicaciones comparten dicha SAN? ¿Cómo afectan estas aplicaciones al rendimiento de bases de datos en la disputa por los recursos de disco compartido en la SAN? Cuando varias aplicaciones compiten por los mismos recursos de disco, pueden producirse cuellos de botella y rendimientos de disco irregulares.


## Usuarios y grupos
El número de usuarios y grupos de su entorno es una consideración habitual al pensar en la escala de una implementación. Sin embargo, existen otras consideraciones pertinentes que también debe tener en cuenta para la planificación. Estas son algunas de dichas consideraciones:

- ¿Pueden crear grupos los usuarios? Si es así, sopese la posibilidad de calcular de qué forma afectará que los usuarios creen nuevos grupos al crecimiento de grupos del entorno.

- ¿Se implementarán grupos dinámicos?
  - ¿Qué tipos de grupos dinámicos se implementarán?
  - ¿Cuántos grupos dinámicos se espera implementar?


## Niveles de carga previstos
También debe pensar en el tipo de carga que se asignará a los componentes de MIM. Estas son algunas de las cuestiones importantes que debe tener en cuenta:

- ¿Con qué frecuencia espera que una solicitud se una a un grupo o lo abandone?

- ¿Con qué frecuencia prevé que un usuario cree un grupo estático o dinámico?

- ¿Puede obtener esta información de las aplicaciones existentes en su entorno?

- ¿Cuánta carga espera por parte de operaciones no controladas por los usuarios, como la sincronización de cambios de sistemas externos? Asegúrese de que dispone de capacidad suficiente para la carga que genera la sincronización de operaciones de información de identidades con sistemas externos.

- ¿Qué tipo de escenarios planea implementar? La existencia de diferentes escenarios supone diferentes patrones de carga. Por ejemplo, los equipos cliente con el cliente de MIM 2016 instalado comprueban periódicamente si es necesario el registro al iniciar sesión, lo que incrementa la carga del sistema.

- ¿Espera grandes variaciones en los niveles de carga, de carga normal a máxima? Por ejemplo, ¿espera una gran cantidad de restablecimientos de contraseña tras los períodos de vacaciones? Asegúrese de llevar a cabo las programaciones de sincronización y mantenimiento del sistema fuera de los picos de uso previstos. Cuando planifique la capacidad, asegúrese de tener en cuenta los períodos de carga máxima.


## Objetos de configuración de directivas

Los objetos de configuración de directivas de Microsoft Identity Manager representan la lógica de negocios para la implementación de MIM. Se trata de un área en la que es probable que cada implementación de MIM sea única puesto que la configuración de directivas se realiza de forma específica para las necesidades de cada implementación. Los objetos de configuración de directivas de MIM incluyen MPR, conjuntos, flujos de trabajo y reglas de sincronización para implementaciones concretas. Estas son algunas de las consideraciones de rendimiento más importantes relacionadas con los objetos de configuración de directivas de MIM:

- **Conjuntos** Cada operación del sistema se debe evaluar con las pertenencias a conjuntos y las actualizaciones de estos que provocan cambios en la pertenencia a conjuntos. Por ejemplo, un cambio tan sencillo como el del número de edificio de la oficina de una persona no puede significar un gran impacto. Sin embargo, otros cambios pueden suponer impactos en cascada, como el cambio de un administrador, que puede afectar a varios objetos en varios niveles.

- **Reglas de directiva de administración** Las MPR se emplean para controlar las reglas de control de acceso y la activación de flujos de trabajo. A medida que cree las MPR, puede descubrir que existe la necesidad de aumentar el número de conjuntos de manera que pueda capturar distintos estados de transición de objeto. Puede que estos conjuntos adicionales activen flujos de trabajo adicionales, y que cada flujo de trabajo asigne solicitudes únicas en el sistema. Como consecuencia, esto supone otro elemento que debe incluir en la planificación de la capacidad.

Cuando trabaje con objetos de configuración de directivas de MIM, también debe considerar lo siguiente:

- ¿Va a aprovisionar principios de seguridad externos en varios bosques de servicios de dominio de Active Directory (AD DS)? Si lo hace, se generarán más solicitudes y flujos de trabajo, lo que supondrá una carga adicional para el sistema.

- ¿Usará aprovisionamiento sin código? Si es así, afectará al número de entradas de reglas previstas, así como a las solicitudes asociadas y a los flujos de trabajo del sistema.


## Consulte también
- Puede descargar el documento [Forefront Identity Manager (FIM) 2010 Capactity Planning Guide](http://go.microsoft.com/fwlink/?LinkId=200180) (Guía de planeación de la capacidad de Forefront Identity Manager 2010 [FIM]); en él se ofrecen más detalles sobre la compilación de prueba y los resultados de pruebas de rendimiento.


<!--HONumber=Apr16_HO2-->


