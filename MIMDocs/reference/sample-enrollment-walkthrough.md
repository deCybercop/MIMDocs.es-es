---
title: "Tutorial de ejemplo de inscripción | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 574a4d6fc2d5d4a51483a371cf96c2d48c582a8b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="sample-enrollment-walkthrough"></a>Tutorial de inscripción de ejemplo
En este tema se muestran los pasos necesarios para realizar una inscripción de autoservicio de tarjeta inteligente virtual. Muestra una solicitud aprobada automáticamente con un PIN establecido por el usuario.
1.  El cliente autentica al usuario y después solicita una lista de plantillas de perfil en las que el usuario autenticado puede inscribirse:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    <br/>
2.  El cliente muestra la lista resultante al usuario. El usuario selecciona una plantilla de perfil VSC (Tarjeta inteligente virtual) con el nombre "VPN de tarjeta inteligente virtual" y el UUID *97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1*.

3.  El cliente recupera la directiva de inscripción de la plantilla de perfil seleccionada mediante el UUID devuelto en el paso anterior:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```
 <br/>   
4.  El cliente analiza el campo "DataCollection" en la directiva devuelta, teniendo en cuenta que aparece un elemento de colección de datos único elemento titulado "Motivo de la solicitud". El cliente también observa que la marca "collectComments" está establecida en *false*, por lo que no solicita al usuario que escriba ningún comentario.

5.  Una vez que el usuario ha escrito el motivo por el que solicita los certificados, el cliente crea una solicitud:

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```
<br/>
6.  El servidor crea correctamente la solicitud y devuelve el identificador URI de la solicitud al cliente: /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5.

7.  El cliente recupera el objeto de solicitud llamando al URI devuelto:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```
<br/>
8.  El cliente comprueba que la propiedad "estado" de la solicitud esté establecida en "aprobada". Puede comenzar la ejecución de la solicitud.

9.  El cliente examina la solicitud para ver si ya hay una tarjeta inteligente asociada a la solicitud, para lo cual analiza el contenido del parámetro "newsmartcarduuid".

10. Puesto que solo contiene un GUID vacío, el cliente debe usar una tarjeta existente que no esté usando MIM CM o crear una si la plantilla de perfil se ha configurado para tarjetas inteligentes virtuales.

11. Puesto que lo segundo se ha indicado al cliente mediante la consulta inicial para las plantillas de perfil que se pueden inscribir (paso 1), el cliente debe crear ahora un dispositivo de tarjeta inteligente virtual.

12. El cliente recupera la directiva de tarjeta inteligente a partir de la plantilla de perfil (con el UUID de la plantilla que se seleccionó en el paso 3):

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```
13. La directiva de tarjeta inteligente contendrá la clave de administrador predeterminada de la tarjeta en la propiedad "DefaultAdminKeyHex". Al crear la tarjeta inteligente, el cliente debe establecer esta clave como la clave de administrador inicial de dicha tarjeta.  

14. Una vez que se haya creado el dispositivo de tarjeta inteligente, el cliente debe asignarlo a la solicitud:

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```
<br/>
15. El servidor responderá al cliente con un URI al objeto de tarjeta inteligente recién creado: api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644.

16. El cliente recupera el objeto de tarjeta inteligente:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```
<br/>
17. Mediante la comprobación del valor de la marca "diversifyadminkey" en la directiva de tarjeta inteligente que se obtuvo en el paso 12, el cliente sabe que tiene que diversificar la clave de administrador.

18. El cliente recupera la clave de administrador propuesta:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```
<br/>
19. El cliente debe autenticarse en la tarjeta como administrador para establecer la clave de administrador. Para hacerlo, el cliente solicita un desafío de autenticación desde la tarjeta inteligente y lo envía al servidor:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    El servidor envía la respuesta al desafío en el cuerpo de respuesta HTTP. Busque la cadena de desafío hexadecimal, conviértala a base64 y pásela como un parámetro en la dirección URL. La dirección URL devolverá otra respuesta, que se debe convertir de nuevo al formato hexadecimal.
<br/>
20. El servidor envía la respuesta al desafío en el cuerpo de respuesta HTTP y el cliente la usa para diversificar la clave de administrador.

21. El cliente observa que el usuario debe proporcionar el PIN que quiera e la inspección del campo "UserPinOption" en la directiva de tarjeta inteligente.

22. Una vez que el usuario haya escrito el PIN que quiere en un cuadro de diálogo, el cliente realiza una autenticación desafío-respuesta, tal como se describe en el paso 19, con la única diferencia de que la marca diversificada ahora se debe establecer en "true":

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```
<br/>
23. El cliente usa la respuesta recibida desde el servidor para establecer el PIN de usuario deseado.

24. El cliente ya está listo para generar solicitudes de certificados. Consulta los parámetros de generación de certificados de plantillas de perfiles para determinar cómo se deben generar las claves/solicitudes.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```
<br/>
25. El servidor responde con un único objeto JSON serializado CertificateRequestGenerationOptions que detalla la exportabilidad de la clave, el nombre descriptivo del certificado, el algoritmo hash, el algoritmo de clave, el tamaño de la clave, etc. El cliente usa estos parámetros para generar una solicitud de certificado.

26. La solicitud de certificado individual se envía al servidor. El cliente podría especificar además una contraseña PFX que se tendría que usar para descifrar los blobs PFX en caso de que la plantilla de certificado especifique el archivado de los certificados en la entidad de certificación (CA), es decir, la CA genera el par de claves y después lo envía al cliente. El cliente también puede optar por agregar algunos comentarios.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```
<br/>
27. Después de unos segundos, el servidor responde con un único objeto JSON serializado Microsoft.Clm.Shared.Certificate. Mediante la comprobación de la marca "isPkcs7", el cliente descubre que esta respuesta no es un blob PFX. El cliente extrae el blob descodificando en base 64 la cadena "pkcs7" y lo instala.

28. El cliente marca la solicitud como completa.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```
