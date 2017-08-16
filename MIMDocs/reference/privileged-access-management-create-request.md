---
title: Crear solicitud de PAM | Microsoft Docs
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
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: de28af5c49eb98c5a1ccfbd8ed9353cf202e9e66
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="create-pam-request"></a>Crear solicitud de PAM
Utilizada por una cuenta con privilegios, la eleva a un rol de PAM.

**Nota**: Las direcciones URL que se muestran en este tema son relativas al nombre de host elegido durante la implementación de la API, como, por ejemplo: `http://api.contoso.com`.
##<a name="request"></a>Solicitud


Método  |URL de solicitud  
---------|---------
POST     |/api/pamresources/pamrequests

###<a name="query-parameters"></a>Parámetros de consulta
Parámetro | Descripción
--------|-------------
Justification | Opcional. Motivo para la solicitud de elevación proporcionado por el usuario.
RoleId| Obligatorio. Identificador único (GUID) del rol de PAM al que se elevarán los privilegios.
RequestedTTL| Obligatorio. Tiempo de expiración solicitado, en segundos.
SolicitudedTime | Opcional. Tiempo para elevar los privilegios.  
v | Opcional. Versión de la API. Si no se incluye, se usará la versión actual (la más reciente) de la API. Para más información, consulte [Control de versiones en Detalles de servicio de la API de REST de PAM](privileged-access-management-rest-api-service-details.md#versioning).

**Nota**: Puede especificar los parámetros *Justification*, *RoleId*, *RequestedTTL*y *RequestedTime* como propiedades en el cuerpo de la solicitud, en lugar de como parámetros de consulta. El parámetro *v* solo puede especificarse como parámetro de consulta.

###<a name="request-headers"></a>Encabezados de solicitud
Para información sobre los encabezados de solicitud habituales, consulte [Encabezados de solicitud y respuesta HTTP](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) en *Detalles de servicio de la API de REST de PAM*.
###<a name="request-body"></a>Cuerpo de la solicitud
Opcional. Como se menciona anteriormente, los parámetros *Justification*, *RoleId*, *RequestedTTL*y *RequestedTime* se pueden especificar como propiedades de un cuerpo de solicitud en vez de especificarlos en la dirección URL de la cadena de consulta.

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
Una respuesta correcta contiene un objeto de solicitud de PAM con las siguientes propiedades.

Propiedad | Descripción
--------|-------------
IdSolicitud | Identificador único (GUID) para la solicitud de PAM.
CreatorID | Identificador único (GUID) en el servicio MIM para la cuenta que creó la solicitud.
Justification | Motivo de la elevación.
CreationTime | Hora de creación de la solicitud.
CreationMétodo | Método usado para crear la solicitud.
ExpirationTime | Hora de expiración de la solicitud.
RoleID| Identificador único (GUID) del rol de PAM.
SolicitudedTTL | Tiempo de espera de expiración solicitado en segundos.
RequestedTime | Tiempo solicitado para la elevación.
RequestStatus | Estado de la solicitud. Los valores posibles son: "Procesando", "Activa", "Cerrada", "Cerrando", "Expirada", "Pendiente de aprobación", "Pendiente de MFA" y "Rechazada".

##<a name="example"></a>Ejemplo

###<a name="request-1"></a>Solicitud 1
```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```
###<a name="response-1"></a>Respuesta 1
```
HTTP/1.1 201 Created

{  
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"c0112f13-b16b-40ad-b547-07f23a7fba52",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":"Sample Reason",
    "CreationTime":"2015-07-11T23:38:09.036164-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"7200",
    "RequestedTime":"2015-07-12T06:40:00Z",
    "RequestStatus":"PendingApproval"
}
```       

###<a name="request-2"></a>Solicitud 2
```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```
###<a name="response-2"></a>Respuesta 2
```
HTTP/1.1 201 Created

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"504f9c49-00db-42bd-a157-ee5664617189",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":null,
    "CreationTime":"2015-07-11T23:07:30.2200123-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"3600",
    "RequestedTime":"2015-07-12T06:07:27.7229894Z",
    "RequestStatus":"PendingApproval"
}
```       
