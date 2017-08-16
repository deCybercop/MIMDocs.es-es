---
title: Referencia de la API de REST de Privileged Access Management | Microsoft Docs
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 02de09a7de843cb42080daa4ce43a5a0c74f2c18
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="privileged-access-management-rest-api-reference"></a>Referencia de la API de REST de Privileged Access Management
Microsoft Identity Manager (MIM) 2016 agrega un escenario nuevo denominado Privileged Access Management (PAM). PAM permite a las organizaciones tener un mayor control sobre los derechos de acceso a recursos confidenciales de las cuentas de usuario con privilegios elevados, como es el caso de los administradores de sistema o servicio. PAM controla el acceso de las cuentas de privilegios elevados mediante el suministro de derechos de acceso de tiempo limitado, justo a tiempo (JIT), cuando se necesitan los derechos de acceso.

Un usuario puede solicitar al Servicio MIM derechos de acceso con privilegios (elevación) de una de estas dos maneras:

- Mediante la API de REST de PAM.
- Mediante el cmdlet New-PAMRequest de PowerShell de PAM.

Los temas de esta guía describen la API de REST de PAM. Para obtener más información acerca de cómo usar el cmdlet de PowerShell, consulte la Guía del laboratorio de pruebas: Demostración de la administración de acceso con privilegios mediante Microsoft Identity Manager, disponible en el sitio de Connect.

##<a name="pam-rest-api-resources-and-operations"></a>Operaciones y recursos de la API de REST de PAM
La API de REST de PAM opera en los siguientes recursos
- **Rol de PAM**: Un rol de PAM asocia una colección de usuarios a una colección de derechos de acceso. Los derechos de acceso se definen por referencia a grupos de seguridad.  Cada rol de PAM tiene una lista de cuentas de usuario, denominados candidatos, que tienen derecho a elevar sus privilegios al rol de PAM. Puede realizar las siguientes operaciones en los roles de PAM:

    - [Obtener roles de PAM](privileged-access-management-get-roles.md)

- **Solicitud de PAM**: Un usuario que quiera elevar sus privilegios a los derechos de acceso de rol de PAM tiene que enviar una solicitud de PAM y obtener la aprobación de la solicitud para poder elevar sus privilegios. El objeto de solicitud de PAM realiza un seguimiento del ciclo de vida de esta solicitud en el Servicio MIM. Puede realizar las siguientes operaciones en las solicitudes de PAM:

    - [Crear solicitud de PAM](privileged-access-management-create-request.md)
    - [Obtener solicitudes de PAM](privileged-access-management-get-requests.md)
    - [Cerrar solicitud de PAM](privileged-access-management-close-request.md)

- **Solicitud pendiente de PAM**: Se usa para aprobar o rechazar las solicitudes de PAM enviadas por usuarios. Puede realizar las siguientes operaciones en las solicitudes pendientes de PAM:

    - [Obtener solicitudes pendientes de PAM](privileged-access-management-get-pending-requests.md)
    - [Aprobar o rechazar una solicitud pendiente de PAM](privileged-access-management-approve-reject-pending-request.md)

- **Sesión de PAM**: Cuando se usa la API de REST de PAM, el cliente (por ejemplo, un explorador web) tiene una sesión con el extremo de la API de REST de PAM. En esta sesión el cliente se autentica para el extremo de la API de REST. Puede realizar las siguientes operaciones en las sesiones de PAM:

     - [Obtener la información de sesión de PAM](privileged-access-management-get-session-info.md)

Para obtener más información acerca del servicio, consulte [Detalles de servicio de la API de REST de PAM](privileged-access-management-rest-api-service-details.md).

##<a name="pam-sample-portal-on-github"></a>Portal de ejemplo de PAM en GitHub
Una forma de aprender a usar la API de REST de PAM consiste en usar el portal de ejemplo de PAM, una aplicación web de ejemplo que usa esta API. Puede encontrar el código del portal de ejemplo de PAM en el [repositorio de ejemplo de PAM en GitHub](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409). Puede aprender a implementar el portal de ejemplo en la Guía del laboratorio de pruebas de PAM.
