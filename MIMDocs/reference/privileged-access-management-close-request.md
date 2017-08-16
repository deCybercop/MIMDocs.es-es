---
title: Cerrar solicitud de PAM | Microsoft Docs
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
ms.assetid: ca3a1a68-9a2b-47da-bfc1-eaa360cbe609
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 181bf1a2953e461925d1b7efc5da4a2fa0c86914
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="close-pam-request"></a>Cerrar solicitud de PAM
Usada por una cuenta con privilegios, aprueba, cierra o rechaza una solicitud de elevación a un rol de PAM iniciada por esa cuenta.

**Nota**: Las direcciones URL que se muestran en este tema son relativas al nombre de host elegido durante la implementación de la API, como, por ejemplo: `http://api.contoso.com`.
##<a name="request"></a>Solicitud


Método  |URL de solicitud  
---------|---------
POST     |/api/pamresources/pamrequests({requestId)/Close

###<a name="url-parameters"></a>Parámetros de URL
Parámetro | Descripción
----------|-----------
requestId | Identificador (GUID) de la solicitud de PAM que se desea cerrar, especificado como se indica a continuación: `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

###<a name="query-parameters"></a>Parámetros de consulta
Parámetro | Descripción
----------|--------------
v | Opcional. Versión de la API. Si no se incluye, se usará la versión actual (la más reciente) de la API. Para más información, consulte [Control de versiones en Detalles de servicio de la API de REST de PAM](privileged-access-management-rest-api-service-details.md#versioning).

###<a name="request-headers"></a>Encabezados de solicitud
Para información sobre los encabezados de solicitud habituales, consulte [Encabezados de solicitud y respuesta HTTP](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) en *Detalles de servicio de la API de REST de PAM*.
###<a name="request-body"></a>Cuerpo de la solicitud
ninguno

##<a name="response"></a>Respuesta
###<a name="response-codes"></a>Códigos de respuesta
Código  |Descripción  
---------|---------
200 | Aceptar
401 | Sin autorización
403 | prohibido
408 | Tiempo de espera de solicitud   
500 | error interno del servidor
503 | servicio no disponible

###<a name="response-headers"></a>Encabezados de respuesta
Para información sobre los encabezados de respuesta comunes, consulte [Encabezados de solicitud y respuesta HTTP](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) en *Detalles de servicio de la API de REST de PAM*.
###<a name="response-body"></a>Cuerpo de respuesta
ninguno
##<a name="example"></a>Ejemplo

###<a name="request"></a>Solicitud
```
POST /api/pamresources/pamrequests(guid'5ec10e61-cdd1-404e-a18e-740467d87dbf')/Close HTTP/1.1


```
###<a name="response"></a>Respuesta
```
HTTP/1.1 200 OK

```       
