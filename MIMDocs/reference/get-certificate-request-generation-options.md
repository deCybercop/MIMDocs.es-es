---
title: "Obtener opciones de generación de solicitudes de certificado | Microsoft Docs"
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
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5822da1b9cf8558becccff815fd0208923fcaaa0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="get-certificate-request-generation-options"></a>Obtener opciones de generación de solicitud de certificado

Obtiene los parámetros para la generación de solicitud de certificado del lado cliente.

**Nota**: Las direcciones URL que se muestran en este tema son relativas con respecto al nombre de host elegido durante la implementación de la API, como, por ejemplo: `https://api.contoso.com`.
##<a name="request"></a>Solicitud


Método  |URL de solicitud  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{requestid}/certificaterequestgenerationoptions

###<a name="url-parameters"></a>Parámetros de URL
Parámetro | Descripción
--------|--------------
requestid| Obligatorio. Identificador GUID de la solicitud de MIM CM para la que se deben recuperar los parámetros de generación de solicitud de certificado.

###<a name="request-headers"></a>Encabezados de solicitud
Para información sobre los encabezados de solicitud habituales, consulte [Encabezados de solicitud y respuesta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) en *Detalles de servicio de la API de REST de CM*.
###<a name="request-body"></a>Cuerpo de la solicitud
Ninguno.


##<a name="response"></a>Respuesta
###<a name="response-codes"></a>Códigos de respuesta
Código  |Descripción  
---------|---------
200 | Aceptar
204 | Sin contenido
403 | prohibido
500 | Error interno

###<a name="response-headers"></a>Encabezados de respuesta
Para información sobre los encabezados de respuesta habituales, consulte [Encabezados de solicitud y respuesta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) en *Detalles de servicio de la API de REST de CM*.
###<a name="response-body"></a>Cuerpo de respuesta
Si se ejecuta correctamente, devuelve la lista de objetos CertificateRequestGenerationOptions. Cada objeto CertificateRequestGenerationOptions corresponde a una única solicitud de certificado que el cliente tiene que generar y tiene las siguientes propiedades:

Propiedad| Descripción
--------|-----------
Exportable | Valor que especifica si se puede exportar la clave privada creada para la solicitud.
FriendlyName | Nombre para mostrar del certificado inscrito.
HashAlgorithmName | Algoritmo hash usado al crear la firma de la solicitud de certificado.
KeyAlgorithmName | Algoritmo de clave pública.
KeyProtectionLevel | Nivel de protección de clave de alta seguridad.
KeySize | Tamaño en bits de la clave privada que se va a generar.
KeyStorageProviderNames | Lista con los proveedores de almacenamiento de claves (KSP) aceptables que se pueden usar para generar la clave privada. En caso de que el primer KSP no se pueda usar para generar la solicitud de certificado, se puede usar cualquiera de los KSP especificados hasta que uno de ellos lo consiga.
KeyUsages | Operación que puede realizar la clave privada creada para esta solicitud de certificado. El valor predeterminado es Firma.
Firmante | Nombre del firmante.

**Nota**: Puede encontrar más información acerca de estas propiedades en la descripción de la [clase Windows.Security.Cryptography.Certificates.CertificateRequestProperties](https://msdn.microsoft.com/library/windows/apps/br212079.aspx), pero tenga en cuenta que no hay una correspondencia exacta entre esta clase y los objetos CertificateRequestGenerationOptions.

##<a name="example"></a>Ejemplo

###<a name="request"></a>Solicitud
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1

```
###<a name="response"></a>Respuesta
```
HTTP/1.1 200 OK

[
    {
        "Subject":"",
        "KeyAlgorithmName":"RSA",
        "KeySize":2048,
        "FriendlyName":"",
        "HashAlgorithmName":"SHA1",
        "KeyStorageProviderNames":[
            "Contoso Smart Card Key Storage Provider"
        ],
        "Exportable":0,
        "KeyProtectionLevel":0,
        "KeyUsages":3
    }
]
```       
