---
title: Obtener plantillas de perfil | Microsoft Docs
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
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8736b328926445f6434bd037a1ecf2e5de9ee466
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-templates"></a>Obtener plantillas de perfil
Obtiene una lista de plantillas de perfil con las que se puede inscribir el usuario especificado. Este método devuelve una vista limitada de la plantilla de perfil. Los datos de la plantilla de perfil devueltos deben ser suficientes para que el usuario solicitante pueda decidir con qué plantilla de perfil necesita inscribirse, si corresponde. Si no se especifican el flujo de trabajo y el permiso, se devolverán todas las plantillas de perfil visibles para el usuario.

**Nota**: Las direcciones URL que se muestran en este tema son relativas con respecto al nombre de host elegido durante la implementación de la API, como, por ejemplo: `https://api.contoso.com`.
##<a name="request"></a>Solicitud


Método  |URL de solicitud  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates?\[targetuser\] 

###<a name="url-parameters"></a>Parámetros de URL
Parámetro| Descripción
--------|-------------
targetuser| Opcional. Especifica el usuario de destino al que devolver las plantillas de perfil. Si no se especifica, se usará la identidad del usuario actual. <br/>**Nota**: Actualmente, solo se admite el usuario actual.

###<a name="request-headers"></a>Encabezados de solicitud
Vea el tema de servicio para los encabezados de solicitud comunes
###<a name="request-body"></a>Cuerpo de la solicitud
ninguno

##<a name="response"></a>Respuesta
###<a name="response-codes"></a>Códigos de respuesta
Código  |Descripción  
---------|---------
200     | Aceptar
204 | Sin contenido
500 | Error interno

###<a name="response-headers"></a>Encabezados de respuesta
Vea el tema de servicio para los encabezados de respuesta comunes
###<a name="response-body"></a>Cuerpo de respuesta
Si se ejecuta correctamente, devuelve una lista de objetos ProfileTemplateLimitedView con las siguientes propiedades.

Propiedad| Tipo| Descripción
--------|-----|--------
Nombre| cadena| Nombre para mostrar de la plantilla de perfil.
Descripción| cadena| Descripción de la plantilla de perfil.
Uuid| GUID| Identificador de la plantilla de perfil.
IsSmartcardProfileTemplate| bool| Indica si la plantilla es una plantilla de perfil de tarjeta inteligente.
IsVirtualSmartcardProfileTemplate| bool| Indica si la plantilla de perfil requiere una tarjeta inteligente virtual.

##<a name="example"></a>Ejemplo

###<a name="request"></a>Solicitud
```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```
###<a name="response"></a>Respuesta
```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]

```       
