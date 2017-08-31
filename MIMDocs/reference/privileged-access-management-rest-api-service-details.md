---
title: Detalles de servicio de la API de REST de PAM | Microsoft Docs
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
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6e248ba4a65e75b1e914c61de70072c7e094123a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="pam-rest-api-service-details"></a>Detalles de servicio de la API de REST de PAM
En las siguientes secciones se describen los detalles de la API de REST de Privileged Access Management (PAM) de Microsoft Identity Manager (MIM).

## <a name="http-request-and-response-headers"></a>Encabezados de solicitud y respuesta HTTP

Las solicitudes HTTP enviadas a la API deben incluir los siguientes encabezados (esta lista no es exhaustiva):

Encabezado | Descripción
-------|------------
Autorización | Obligatorio. El contenido dependerá del método de autenticación, que es configurable y puede basarse en WIA (Autenticación integrada de Windows) o ADFS.
Content-Type | Obligatorio si la solicitud tiene un cuerpo. Debe ser "application/json".
Content-Length | Obligatorio si la solicitud tiene un cuerpo. 
Cookie | Cookie de la sesión. Puede ser obligatorio en función del método de autenticación.
<br/>
Las respuestas HTTP incluirán los siguientes encabezados (esta lista no es exhaustiva):

Header | Descripción
-------|------------
Content-Type | La API siempre devuelve "application/json".
Content-Length | Longitud del cuerpo de la solicitud en bytes si es que está presente.

## <a name="versioning"></a>Control de versiones 
La versión actual de la API es 1. La versión de API puede especificarse mediante un parámetro de consulta en la dirección URL de solicitud, como en el ejemplo siguiente: `http://localhost:8086/api/pamresources/pamrequests?v=1` Si no se especifica la versión de la solicitud, la solicitud se ejecuta con la versión más reciente publicada de la API. 

## <a name="security"></a>Seguridad 
El acceso a la API requiere Autenticación integrada de Windows (IWA). Esta debe configurarse manualmente en IIS antes de instalar Microsoft Identity Manager (MIM).

Se ofrece compatibilidad con HTTPS (TLS), pero debe configurarse manualmente en IIS. Para más información, consulte: **Implement Secure Sockets Layer (SSL) for the FIM Portal** (Implementación de Capa de sockets seguros [SSL] para el portal de FIM) [Step 9: Perform FIM 2010 R2 Post-Installation Tasks]\(Paso 9: Realización de tareas posteriores a la instalación de FIM 2010 R2) (https://technet.microsoft.com/library/hh322875(v=ws.10%29.aspx) en el documento Installing FIM 2010 R2 Test Lab Guide (Guía de laboratorio de prueba de instalación de FIM 2010 R2). 

Puede generar un nuevo certificado de servidor SSL ejecutando el siguiente comando en el símbolo del sistema de Visual Studio:
```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
Este comando crea un certificado autofirmado que se puede usar para probar una aplicación web que use SSL en un servidor web cuya dirección URL sea test.cwap.com. El OID definido mediante la opción -eku identifica el certificado como certificado SSL de servidor. El certificado se almacena en el almacén y está disponible en el nivel de equipo, por lo que puede exportarlo desde el complemento Certificados en mmc.exe.

## <a name="cross-domain-access-cors"></a>Acceso entre dominios (CORS) 
Se ofrece compatibilidad con CORS, pero debe configurarse manualmente en IIS. Agregue los siguientes elementos al archivo web.config de la API implementada para configurar la API para que permita las llamadas entre dominios: 

```
<system.webServer>       
    <httpProtocol> 
        <customHeaders> 
            <add name="Access-Control-Allow-Credentials" value="true"  /> 
            <add name="Access-Control-Allow-Headers" value="content-type" /> 
            <add name="Access-Control-Allow-Origin" value="http://<hostname>:8090" /> 
        </customHeaders> 
    </httpProtocol> 
</system.webServer> 
```
<br/>

## <a name="error-handling"></a>Control de errores 
La API devuelve respuestas de error HTTP para indicar condiciones de error. Los errores son compatibles con OData. La siguiente tabla muestra los códigos de error que se pueden devolver a un cliente.

Código de estado HTTP | Descripción
-----------------|------------
401 | Sin autorización 
403 | prohibido 
408 | Tiempo de espera de solicitud   
500 | error interno del servidor 
503 | servicio no disponible 
<br/>

## <a name="filtering"></a>Filtrado 
Las solicitudes de la API de REST de PAM pueden incluir filtros para especificar las propiedades que deben incluirse en la respuesta. La sintaxis de filtro se basa en expresiones de OData.

Los filtros pueden especificar cualquiera de las propiedades de las solicitudes de PAM, de los roles de PAM o de las solicitudes pendientes de PAM. Por ejemplo: *ExpirationTime*, *DisplayName*o cualquier otra propiedad válida de una solicitud de PAM, rol de PAM o solicitud pendiente de PAM.

La API es compatible con los siguientes operadores en las expresiones de filtros: *And*, *Equal*, *NotEqual*, *GreaterThan*, *LessThan*, *GreaterThenOrEqueal*y *LessThanOrEqual*. 

Las siguientes solicitudes de ejemplo incluyen filtros:

- Esta solicitud devuelve todas las solicitudes de PAM entre fechas específicas: `http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime"2015-01-09T08:26:49.721Z" and ExpirationTime lt datetime"2015-02-10T08:26:49.722Z" `
 
- Esta solicitud devuelve el rol de PAM con el nombre para mostrar "Acceso a archivo SQL": `http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq "SQL File Access" `
