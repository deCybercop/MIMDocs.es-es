---
title: Obtener solicitudes de PAM | Microsoft Docs
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
ms.assetid: 620eebd6-e4c3-473b-b824-ee6cfe83e509
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 00467b6443d3e6e0511ee1817a945ffe569b99b0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-requests"></a>Obtener solicitudes de PAM
Lo usa una cuenta con privilegios para devolver un historial de solicitudes de PAM enviadas previamente.

**Nota**: Las direcciones URL que se muestran en este tema son relativas con respecto al nombre de host elegido durante la implementación de la API, como, por ejemplo: `http://api.contoso.com`.
##<a name="request"></a>Solicitud


Método  |URL de solicitud  
---------|---------
GET     |/api/pamresources/pamrequests

###<a name="query-parameters"></a>Parámetros de consulta
Parámetro | Descripción
----------|--------------
$filter | Opcional. Especifique cualquiera de las propiedades de solicitud de PAM en una expresión de filtro para devolver una lista filtrada de respuestas. Para más información acerca de los operadores compatibles, consulte [Filtrado en Detalles de servicio de la API de REST de PAM](privileged-access-management-rest-api-service-details.md#filtering).
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
Una respuesta correcta contiene una lista de objetos de solicitudes de PAM con las siguientes propiedades.

Propiedad | Descripción
--------|-------------
IdSolicitud | Identificador único (GUID) para la solicitud de PAM.
CreatorID | Identificador único (GUID) para la cuenta de Active Directory que creó la solicitud de PAM.
Justification | Motivo de la elevación.
DisplayName | Nombre para mostrar de la solicitud de PAM en MIM.
CreationTime | Hora de creación de la solicitud.
CreationMétodo | Método usado para crear la solicitud.
ExpirationTime | Hora de expiración de la solicitud.
RoleID| Identificador único (GUID) del rol de PAM.
SolicitudedTTL | Tiempo de espera de expiración solicitado en segundos.
RequestedTime | Tiempo solicitado para la elevación.
SolicitudedStatus | Estado de la solicitud. Los valores posibles son: "Activa", "Cerrada", "Cerrando", "Expirada", "Pendiente de aprobación" y "Rechazada".

##<a name="example"></a>Ejemplo

###<a name="request"></a>Solicitud
```
GET /api/pamresources/pamrequests?$filter=CreationTime%20gt%20datetime'2015-06-12T04:49:32.431Z'%20and%20CreationTime%20lt%20datetime'2015-07-13T04:49:32.432Z' HTTP/1.1
```

###<a name="response"></a>Respuesta
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests",
    "value":[
        {
            "RequestId":"b22e1343-9a2b-4e33-a70a-1bb7b2d405b9",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-06-23T11:34:38.58Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-06-23T12:34:38.847Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-23T11:34:36.417Z",
            "RequestStatus":"Expired"
        },
        {
            "RequestId":"3a98d1c7-524d-4b72-9da7-bd9f907eab55",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Reason for Request",
            "CreationTime":"2015-07-12T04:35:14.433Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T04:43:51.95Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:35:00Z",
            "RequestStatus":"Closed"
        },
        {
            "RequestId":"f5e80be1-e9a3-42c4-81f8-4be5a4a429f4",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-07-12T04:48:17.46Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T05:48:17.853Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-12T04:48:14.057Z",
            "RequestStatus":"Active"
        },
        {
            "RequestId":"b0f0ddc0-c809-4770-9d39-0d706f97a2de",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"",
            "CreationTime":"2015-06-30T07:01:13.147Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-30T07:01:13.119Z",
            "RequestStatus":"Rejected"
        },
        {
            "RequestId":"5ec10e61-cdd1-404e-a18e-740467d87dbf",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Example Reason",
            "CreationTime":"2015-07-12T04:49:09.963Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:50:00Z",
            "RequestStatus":"PendingApproval"
        }
    ]
}
```       
