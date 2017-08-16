---
title: Obtener roles de PAM | Microsoft Docs
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
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4cb4bbc6c7696f354e5759a677ece88a4e606544
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-roles"></a>Obtener roles de PAM
Usado por una cuenta con privilegios, enumera los roles de PAM para los que es candidata la cuenta.

**Nota**: Las direcciones URL que se muestran en este tema son relativas al nombre de host elegido durante la implementación de la API, como, por ejemplo: `http://api.contoso.com`.
##<a name="request"></a>Solicitud


Método  |URL de solicitud  
---------|---------
GET     |/api/pamresources/pamroles

###<a name="query-parameters"></a>Parámetros de consulta
Parámetro | Descripción
----------|--------------
$filter | Opcional. Especifique cualquiera de las propiedades de PAM Role en una expresión de filtro para devolver una lista filtrada de respuestas. Para más información acerca de los operadores compatibles, consulte [Filtrado en Detalles de servicio de la API de REST de PAM](privileged-access-management-rest-api-service-details.md#filtering).
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
Una respuesta correcta contiene una colección de uno o varios roles de PAM, cada uno de los cuales tiene las siguientes propiedades.

Propiedad | Descripción
--------|-------------
RoleID | Identificador único (GUID) del rol de PAM.
DisplayName | Nombre para mostrar del rol de PAM en el servicio MIM.
Descripción | Descripción del rol de PAM en el servicio MIM.
TTL | Tiempo de espera máximo, en segundos, para la caducidad de los derechos de acceso del rol.
AvailableFrom | Primera hora del día en que se activará una solicitud.
AvailableTo | Última hora del día en que se activará una solicitud.
MFAEnabled | Valor booleano que indica si las solicitudes de activación para este rol requieren un desafío MFA.
ApprovalEnabled | Valor booleano que indica si las solicitudes de activación para este rol requieren la aprobación de un propietario del rol.
AvailibitlyWendowEnabled | Valor booleano que indica si el rol solo puede activarse durante un intervalo de tiempo especificado.

##<a name="example"></a>Ejemplo

###<a name="request"></a>Solicitud
```
GET /api/pamresources/pamroles HTTP/1.1
```
###<a name="response"></a>Respuesta
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamroles",
    "value":[
        {
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "DisplayName":"Allow AD Access ",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":false,
            "AvailabilityWindowEnabled":false
        },
        {
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "DisplayName":"ApprovalRole",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":true,
            "AvailabilityWindowEnabled":false
        }
    ]
}
```       
