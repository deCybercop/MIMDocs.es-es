---
title: "Obtener la información de sesión de PAM | Microsoft Docs"
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
ms.assetid: bc30e455-9a9c-413a-b8ca-9669286299cf
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 150bc7e863fc679e785526935155611fb75cd69b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-session-info"></a>Obtener la información de sesión de PAM
Utilizada por una cuenta con privilegios, obtiene el nombre de usuario de la cuenta que ha iniciado la sesión.

**Nota**: Las direcciones URL que se muestran en este tema son relativas al nombre de host elegido durante la implementación de la API, como, por ejemplo: `http://api.contoso.com`.
##<a name="request"></a>Solicitud


Método  |URL de solicitud  
---------|---------
GET     |/api/session/sessionenfo

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
Una respuesta correcta devuelve un objeto de sesión de PAM con las siguientes propiedades.

Propiedad| Descripción
--------|-------------
Nombre de usuario| Nombre de usuario de la cuenta que ha iniciado esta sesión.

##<a name="example"></a>Ejemplo

###<a name="request"></a>Solicitud
```
GET /api/session/sessioninfo/ HTTP/1.1
```
###<a name="response"></a>Respuesta
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#sessioninfo",
    "value":[
        {
            "Username":"FIMCITONEBOXAD\\Jen"
        }
    ]
}
```       
