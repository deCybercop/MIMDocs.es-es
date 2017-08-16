---
title: Obtener la directiva de flujo de trabajo | Microsoft Docs
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
ms.assetid: be636205-c1f0-457c-982e-e17478cf0889
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6bc88fee88bdfeec3ce4ead60bc17fe2ea169402
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="get-workflow-policy"></a>Obtener la directiva de flujo de trabajo
Obtiene la directiva de plantilla de perfil para el flujo de trabajo especificado. Estos datos se usan durante la creación de la solicitud. La directiva de flujo de trabajo especifica los datos que necesita el cliente para crear una solicitud. Estos datos pueden incluir: diversos elementos de recopilación de datos, comentarios de la solicitud y directiva de contraseña de un solo uso.

**Nota**: Las direcciones URL que se muestran en este tema son relativas con respecto al nombre de host elegido durante la implementación de la API, como, por ejemplo: `https://api.contoso.com`.
##<a name="request"></a>Solicitud


Método  |URL de solicitud  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{tipo}

###<a name="url-parameters"></a>Parámetros de URL
Parámetro| Descripción
--------|-------------
id| Obligatorio. GUID correspondiente a la plantilla de perfil de la que se va a extraer la directiva.
tipo| Obligatorio. Tipo de directiva que se solicita. Los valores posibles son: *Enroll*, *Duplicate*, *OfflineUnblock*, *OnlineUpdate*, *Renew*, *Recover*, *RecoverOnBehalf*, *Reinstate*, *Retire*, *Revoke*, *TemporaryEnroll*y *Unblock*.

###<a name="request-headers"></a>Encabezados de solicitud
Para información sobre los encabezados de solicitud habituales, consulte [Encabezados de solicitud y respuesta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) en *Detalles de servicio de la API de REST de CM*.
###<a name="request-body"></a>Cuerpo de la solicitud
ninguno

##<a name="response"></a>Respuesta
###<a name="response-codes"></a>Códigos de respuesta
Código  |Descripción  
---------|---------
200     | Aceptar
403 | prohibido
204 | Sin contenido
500 | Error interno

###<a name="response-headers"></a>Encabezados de respuesta
Para información sobre los encabezados de respuesta habituales, consulte [Encabezados de solicitud y respuesta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) en *Detalles de servicio de la API de REST de CM*.
###<a name="response-body"></a>Cuerpo de respuesta
Si se ejecuta correctamente, devuelve un objeto de directiva basado en un objeto [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx). Como mínimo, el objeto de directiva contiene las propiedades de la tabla siguiente, pero puede contener propiedades adicionales según la directiva solicitada. Por ejemplo, una solicitud en una directiva de inscripción devolverá un objeto [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy.aspx). Para obtener más información, consulte la documentación para el objeto de directiva asociada al parámetro de {tipo} en la solicitud. La documentación de los distintos tipos de objetos de directiva puede encontrarse en la documentación del [espacio de nombres Microsoft.Clm.Shared.ProfileTemplates](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx).

Propiedad | Descripción
---------|------------
ApprovalsNeeded | Número de aprobaciones necesarias para las solicitudes de FIM CM para la directiva.
AuthorizedApprover | Descriptor de seguridad de los usuarios que tienen autorización para aprobar las solicitudes de FIM CM para la directiva.
AuthorizedEnrollmentAgent | Descriptor de seguridad para los usuarios que pueden actuar como agentes de inscripción para la directiva.
AuthorizedInitiator | Descriptor de seguridad para los usuarios que pueden iniciar solicitudes de FIM CM para la directiva.
CollectComments | Valor booleano que indica si está habilitada la recopilación de comentarios para las solicitudes de FIM CM para la directiva.
CollectSolicitudPriority | Valor booleano que indica si está habilitada la recopilación de prioridad para las solicitudes de FIM CM para la directiva.
DefaultSolicitudPriority | Prioridad predeterminada para las solicitudes de FIM CM para la directiva.
Documentos | Documentos de directivas que están configurados para la directiva.
Habilitada | Valor booleano que indica si la directiva está habilitada.
EnrollAgentRequired | Valor booleano que indica si se necesitan agentes de inscripción para las solicitudes de FIM CM para la directiva.
OneTimePasswordPolicy | Obtiene la forma en que se distribuyen las contraseñas de un solo uso para las solicitudes de FIM CM para la directiva.
Personalization | Opciones de personalización de tarjeta inteligente para la directiva.
PolicyDataCollection | Elementos de recopilación de datos que están asociados a la directiva.
SelfServiceHabilitada | Valor booleano que indica si está habilitada la iniciación de autoservicio para las solicitudes de FIM CM para la directiva.

##<a name="example"></a>Ejemplo

###<a name="request-1"></a>Solicitud 1
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```
###<a name="response-1"></a>Respuesta 1
```
HTTP/1.1 200 OK

... body coming soon
```       
###<a name="request-2"></a>Solicitud 2
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```
###<a name="response-2"></a>Respuesta 2
```
HTTP/1.1 200 OK

... body coming soon
```       
##<a name="see-also"></a>Véase también

- [Clase Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Espacio de nombres Microsoft.Clm.Shared.ProfileTemplates](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
