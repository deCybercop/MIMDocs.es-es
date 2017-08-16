---
title: Crear solicitud | Microsoft Docs
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
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d5af8767583cb6999de399de58b321147846291a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="create-request"></a>Crear solicitud
Cree una solicitud de MIM CM.

**Nota**: Las direcciones URL que se muestran en este tema son relativas con respecto al nombre de host elegido durante la implementación de la API, como, por ejemplo: `https://api.contoso.com`.
##<a name="request"></a>Solicitud


Método  |URL de solicitud  
---------|---------
POST     |/CertificateManagement/api/v1.0/requests

###<a name="url-parameters"></a>Parámetros de URL
ninguno

###<a name="request-headers"></a>Encabezados de solicitud
Para información sobre los encabezados de solicitud habituales, consulte [Encabezados de solicitud y respuesta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) en *Detalles de servicio de la API de REST de CM*.
###<a name="request-body"></a>Cuerpo de la solicitud
El cuerpo de la solicitud contiene las siguientes propiedades.

Propiedad | Descripción
---------|-----------
profiletemplateuuid | Obligatorio. GUID de la plantilla de perfil para el que el usuario está creando la solicitud.
datacollection | Obligatorio. Recopilación de pares nombre-valor que representan los datos que debe proporcionar el inscrito. La recopilación de datos necesarios que deben proporcionarse se puede recuperar a partir de la directiva de flujo de trabajo de la plantilla de perfil. Se puede especificar una recopilación vacía.
target | Opcional. GUID del usuario de destino para el que se va a crear la solicitud. Si no se especifica, el valor predeterminado es el usuario actual.
tipo | Obligatorio. El tipo de solicitud que se está creando. Los tipos de solicitud disponibles son: "Enroll", "Duplicate", "OfflineUnblock", "OnlineUpdate", "Renew", "Recover", "RecoverOnBehalf", "Reinstate", "Retire", "Revoke", "TemporaryCards" y "Unblock".<br/>**Nota**: no todos los tipos de solicitudes son compatibles con todas las plantillas de perfil. Por ejemplo, no puede especificar una operación de desbloqueo en una plantilla de perfil de software.
comment | Obligatorio. Cualquier comentario que pueda escribir el usuario. La directiva de flujo de trabajo definirá si esto es necesario. Se puede especificar una cadena vacía.
priority | Opcional. Prioridad de la solicitud. Si no se especifica, se usará la prioridad de solicitud predeterminada según lo determinado por la configuración de la plantilla de perfil.


##<a name="response"></a>Respuesta
###<a name="response-codes"></a>Códigos de respuesta
Código  |Descripción  
---------|---------
201     | Creado
403 | prohibido
500 | Error interno

###<a name="response-headers"></a>Encabezados de respuesta
Para información sobre los encabezados de respuesta habituales, consulte [Encabezados de solicitud y respuesta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) en *Detalles de servicio de la API de REST de CM*.
###<a name="response-body"></a>Cuerpo de respuesta
Si se ejecuta correctamente, devuelve el URI de la solicitud recién creada.
##<a name="example"></a>Ejemplo

###<a name="request-1"></a>Solicitud 1
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```
###<a name="response-1"></a>Respuesta 1
```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```
###<a name="request-2"></a>Solicitud 2
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```
###<a name="response-2"></a>Respuesta 2
```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

###<a name="request-3"></a>Solicitud 3
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "profiletemplateuuid" : "97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
    "datacollection":
    [
        {"name" : "pavle"},
        {"city" : "seattle"}
    ],
    "target" : "97CC3493-F556-4C9B-9D8B-982434201527",
    "type" : "Enroll",
    "comment" : "LALALALA",
    "priority" :  "4"
}
```
##<a name="see-also"></a>Véase también

- [Método Microsoft.Clm.Provision.RequestOperations.InitiateEnroll](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Método Microsoft.Clm.Provision.RequestOperations.InitiateOfflineUnblock](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Método Microsoft.Clm.Provision.RequestOperations.InitiateRecover](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Método Microsoft.Clm.Provision.RequestOperations.InitiateRetire](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Método Microsoft.Clm.Provision.RequestOperations.InitiateUnblock](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
