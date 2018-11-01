---
title: Descripción de los componentes de PAM | Microsoft Docs
description: Privileged Access Management comparte algunos componentes con MIM y dispone de algunos propios. Obtenga información sobre cómo funcionan conjuntamente.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 6498f68f-36d3-448c-8fe6-649ad5a7f97d
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b0a101a06acfdd5b95deb576a7fddfda124a3853
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379703"
---
# <a name="understand-the-components-of-pam"></a>Descripción de los componentes de PAM

Privileged Access Management mantiene el acceso administrativo independiente de las cuentas de usuario diarias. Esta solución se basa en bosques en paralelo:

- *CORP*: el bosque corporativo de uso general que incluye uno o varios dominios. Aunque puede tener varios bosques de CORP, los ejemplos de estos artículos suponen que hay un único bosque con un solo dominio para simplificar.  
- *PRIV*: un bosque dedicado que se ha creado especialmente para este escenario de PAM. Este bosque incluye un dominio para acomodar cuentas y grupos con privilegios que se duplican de uno o varios dominios de CORP.

La solución MIM tal como está configurada para PAM incluye los siguientes componentes:  

- **Servicio de MIM**: implementa la lógica de negocios para realizar operaciones de administración de identidades y acceso, incluidas la administración de cuentas con privilegios y de solicitudes de elevación.
- **Portal de MIM**: portal basado en SharePoint, hospedado por SharePoint 2013, que proporciona una interfaz de usuario para la configuración y la administración de administradores.
- **Base de datos del servicio MIM**: almacenada en SQL Server 2012 o 2014, almacena metadatos y datos de identidad necesarios para el servicio MIM.
- **Servicio de supervisión de PAM** y **Servicio de componente de PAM**: dos servicios que administran el ciclo de vida de las cuentas con privilegios y ayudan a PRIV AD en lo relativo al ciclo de vida de pertenencia a grupos.
- **Cmdlets de PowerShell**: para rellenar el servicio MIM y PRIV AD con usuarios y grupos correspondientes a usuarios y grupos del bosque de CORP para administradores de PAM y para usuarios finales que soliciten el uso de privilegios "Just-In-Time" (JIT) en una cuenta administrativa.
- **API de REST de PAM y portal de ejemplo**: para los desarrolladores que integren MIM en el escenario de PAM con clientes personalizados para realizar operaciones de elevación sin necesidad de usar PowerShell o SOAP. El uso de la API de REST se demuestra con una aplicación web de ejemplo.

Una vez que se haya instalado y configurado, cada grupo creado mediante el procedimiento de migración en el bosque PRIV es un grupo de seguridad basado en SIDHistory duplicado (o en una actualización posterior con Windows Server vNext, un grupo de entidades de seguridad externas) que refleja el SID de un grupo en el bosque CORP original. Además, cuando el Servicio MIM agrega miembros a estos grupos en el bosque PRIV, esas pertenencias serán de tiempo limitado.

Como resultado, cuando un usuario solicita la elevación mediante los cmdlets de PowerShell y se aprueba su solicitud, el Servicio MIM agregará su cuenta del bosque PRIV a un grupo del bosque PRIV. Cuando el usuario inicia sesión con su cuenta con privilegios, su token de Kerberos contendrá un identificador de seguridad (SID) idéntico al SID del grupo del bosque CORP. Puesto que el bosque CORP está configurado para confiar en el bosque PRIV, se muestra la cuenta con privilegios elevados que se usa para acceder a un recurso en el bosque CORP, para que un recurso que comprueba las pertenencias al grupo Kerberos sea miembro de los grupos de seguridad de ese recurso. Esto se proporciona a través de la autenticación entre bosques de Kerberos.

Además, estas pertenencias son de tiempo limitado para que después de un intervalo preconfigurado de tiempo, la cuenta administrativa del usuario deje de formar parte del grupo en el bosque PRIV. Como resultado, dicha cuenta ya no se podrá usar para acceder a otros recursos.
