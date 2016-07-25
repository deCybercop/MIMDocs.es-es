---
title: Modelo de niveles del entorno PAM | Microsoft Identity Manager
description: "Obtenga información sobre el modelo de niveles que aísla el sistema según la vulnerabilidad y el riesgo."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 1a750bedee2aac667c84113d2d08daa20428c260


---

# Modelo de niveles para el particionamiento de los privilegios administrativos

Con el volumen de amenazas actual, no se trata de si un atacante va a acceder a los sistemas, sino de cuándo. Esto significa que la seguridad interna es tan importante como una defensa perimetral potente. En este artículo se describe un modelo de seguridad diseñado para proteger frente a la elevación de privilegios mediante la separación de las actividades con muchos privilegios de las zonas de alto riesgo. Este modelo proporciona una buena experiencia de usuario y se ajusta a los procedimientos recomendados y a los principios de seguridad.

## Elevación de privilegios en bosques de Active Directory

Las cuentas de usuarios, servicios o aplicaciones con privilegios administrativos permanentes en los bosques de Windows Server Active Directory (AD) plantean un nivel considerable de riesgo para la misión y el negocio de la organización. Estas cuentas suelen ser objetivo de los atacantes porque, si se ven en peligro, el atacante puede conectarse a otros servidores o aplicaciones del dominio.

El modelo de niveles crea divisiones entre administradores en función de los recursos que administran. Los administradores con control sobre las estaciones de trabajo de los usuarios están separados de los que controlan aplicaciones o administran identidades de empresa. Obtenga información sobre este modelo en [Securing privileged access reference material (Protección del material de referencia de acceso con privilegios)](http://aka.ms/tiermodel).

## Restricción de la exposición de las credenciales con restricciones de inicio de sesión

La disminución del riesgo de robo de credenciales de las cuentas administrativas generalmente exige reformular los procedimientos administrativos para limitar la exposición a los atacantes. Como primer paso, se recomienda que las organizaciones:

- Limiten la cantidad de hosts en los que se exponen las credenciales administrativas.
- Limiten los privilegios de rol al mínimo requerido.
- Garanticen que las tareas administrativas no se realizan en hosts usados para las actividades de usuario estándar (por ejemplo, correo electrónico y exploración web).

El próximo paso es implementar restricciones de inicio de sesión y habilitar los procesos y prácticas para cumplir con los requisitos del modelo en niveles. De forma ideal, la exposición de las credenciales debería además limitarse al menor privilegio posible exigido para el rol dentro de cada nivel.

Se deben aplicar restricciones de inicio de sesión para garantizar que las cuentas con privilegios elevados no tengan acceso a los recursos menos seguros. Por ejemplo:

- Los administradores de dominio (nivel 0) no pueden iniciar sesión en servidores empresariales (nivel 1) ni en estaciones de trabajo de usuario estándar (nivel 2).
- Los administradores de servidor (nivel 1) no pueden iniciar sesión en estaciones de trabajo de usuario estándar (nivel 2).

>[!NOTE]
> Los administradores de servidor no deben estar en el grupo de administradores de dominio. El personal que tiene responsabilidades de administración tanto de controladores de dominio como de servidores empresariales debe tener cuentas independientes.

Las restricciones de inicio de sesión se pueden aplicar con lo siguiente:

- Restricciones de derechos de inicio de sesión de directivas de grupo, que incluyen:  
    - Denegar el acceso a este equipo desde la red  
    - Denegar el inicio de sesión como trabajo por lotes  
    - Denegar el inicio de sesión como servicio  
    - Denegar el inicio de sesión local  
    - Denegar el inicio de sesión mediante la configuración de Escritorio remoto  
- Silos y directivas de autenticación, si usa Windows Server 2012 o posterior.
- Autenticación selectiva, si la cuenta está en un bosque administrativo dedicado.

En el siguiente artículo, [Planificación de un entorno bastión](planning-bastion-environment.md), se explica cómo agregar un bosque administrativo dedicado para que Microsoft Identity Manager establezca las cuentas administrativas.



<!--HONumber=Jul16_HO3-->


