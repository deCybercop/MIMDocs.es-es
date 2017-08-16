---
title: Obtener solicitudes pendientes de PAM |Microsoft Docs
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
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: aa1e8cd48b1bcca6e856bd553b6b92a062a08ff4
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="get-pending-pam-requests"></a>Obtener solicitudes pendientes de PAM
Lo usa una cuenta con privilegios para devolver una lista de solicitudes pendientes que necesitan aprobación.

**Nota**: Las direcciones URL que se muestran en este tema son relativas al nombre de host elegido durante la implementación de la API, como, por ejemplo: `http://api.contoso.com`.
##<a name="request"></a>Solicitud

Método  |URL de solicitud  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove

###<a name="query-parameters"></a>Parámetros de consulta
Parámetro | Descripción
----------|--------------
$filter | Opcional. Especifique cualquiera de las propiedades de solicitud pendiente de PAM en una expresión de filtro para devolver una lista filtrada de respuestas. Para más información acerca de los operadores compatibles, consulte [Filtrado en Detalles de servicio de la API de REST de PAM](privileged-access-management-rest-api-service-details.md#filtering).
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
Una respuesta correcta contiene una lista de objetos de aprobación de solicitudes de PAM con las siguientes propiedades.

Propiedad | Descripción
---------|-------------
RoleName | Nombre para mostrar del rol para el que se necesita aprobación.
Solicitante | Nombre de usuario del solicitante que se debe aprobar.
Justification | Justificación proporcionada por el usuario.
RequestedTTL | Tiempo de expiración solicitado en segundos.
SolicitudedTime | Tiempo solicitado para la elevación.
CreationTime | Hora de creación de la solicitud.
FIMSolicitudID | Contiene un único elemento, "Valor", con el identificador único (GUID) de la solicitud de PAM.
SolicitudorID | Contiene un único elemento, "Valor", con el identificador único (GUID) para la cuenta de Active Directory que creó la solicitud de PAM.
ApprovalObjectID | Contiene un único elemento, "Valor", con el identificador único (GUID) del Objeto de aprobación.

##<a name="example"></a>Ejemplo

###<a name="request"></a>Solicitud
```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```
###<a name="response"></a>Respuesta
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       
