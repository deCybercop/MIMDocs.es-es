---
title: Obtener PIN propuesto de tarjeta inteligente | Microsoft Docs
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
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 08d28819402dd56f996d39aa03b8ac829bef0838
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-proposed-pin"></a>Get Smartcard Proposed PIN
Obtiene el PIN de usuario generado por el servidor.

**Nota**: El servidor establecerá el PIN solo si la directiva de plantilla de perfil indica que se debe hacer. De lo contrario, deberá proporcionarlo el usuario.

**Nota**: Las direcciones URL que se muestran en este tema son relativas con respecto al nombre de host elegido durante la implementación de la API, como, por ejemplo: `https://api.contoso.com`.
##<a name="request"></a>Solicitud


Método  |URL de solicitud  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards/{id}/serverproposedpen

###<a name="url-parameters"></a>Parámetros de URL
Parámetro | Descripción
---------|------------
id | Identificador de la tarjeta inteligente (específico de MIM CM). Obtenido del objeto Microsft.Clm.Shared.Smartcard.
###<a name="query-parameters"></a>Parámetros de consulta
Parámetro | Descripción
---------|------------
atr | Cadena ATR de la tarjeta.
cardid | Identificador de la tarjeta.
challenge | Cadena codificada en base64 que representa el desafío emitido por la tarjeta inteligente.

###<a name="request-headers"></a>Encabezados de solicitud
Para información sobre los encabezados de solicitud habituales, consulte [Encabezados de solicitud y respuesta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) en *Detalles de servicio de la API de REST de CM*.
###<a name="request-body"></a>Cuerpo de la solicitud
ninguno

##<a name="response"></a>Respuesta
###<a name="response-codes"></a>Códigos de respuesta
Código  |Descripción  
---------|---------
200     | Aceptar
204 | Sin contenido
403 | prohibido
500 | Error interno

###<a name="response-headers"></a>Encabezados de respuesta
Para información sobre los encabezados de respuesta habituales, consulte [Encabezados de solicitud y respuesta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) en *Detalles de servicio de la API de REST de CM*.
###<a name="response-body"></a>Cuerpo de respuesta
Si se ejecuta correctamente, devuelve una cadena que representa el código PIN propuesto por el servidor.

##<a name="example"></a>Ejemplo

###<a name="request"></a>Solicitud
```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```
###<a name="response"></a>Respuesta
```
HTTP/1.1 200 OK

... body coming soon
```       
