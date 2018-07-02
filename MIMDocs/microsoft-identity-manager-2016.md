---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: MIM incluye las capacidades de administración de acceso de FIM 2010 y le ayuda a administrar usuarios, credenciales, directivas y acceso dentro de su organización.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/02/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: bd483ecb0abc3e4bb4444c87715971a3fba9820b
ms.sourcegitcommit: 5405ed10fea6f50b711eca1153446c04d4faff7a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/25/2018
ms.locfileid: "36927194"
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016

Microsoft Identity Manager (MIM) 2016 se basa en las funcionalidades de administración de identidades y acceso de [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Al igual que su predecesor, MIM le ayuda a administrar usuarios, credenciales, directivas y acceso en la organización.  Además, MIM 2016 proporciona una experiencia híbrida, capacidades de administración de acceso con privilegios y compatibilidad con nuevas plataformas.

Y la funcionalidad actual de administración de identidades incluida en [FIM](https://technet.microsoft.com/library/jj133868). MIM 2016 proporciona nuevas características y mejoras, como estas:

- Privileged Identity Management
- Nueva funcionalidad de administración de certificados
  - [Referencia de API de REST de administración de certificados](./reference/certificate-management-rest-api-reference.md)
  - Compatibilidad con topologías de varios bosques
  - [Una aplicación de Windows para tarjetas inteligentes virtuales](working-with-mim-certificate-manager.md)
  - Eventos actualizados y funcionalidades de solución de problemas 
- Los [escenarios de autoservicio](working-with-self-service-password-reset.md) incluyen ahora el desbloqueo de cuentas y la puerta de Azure MFA (autenticación multifactor) para restablecer contraseñas.

## <a name="hybrid-experience"></a>Experiencia híbrida

Microsoft Identity Manager 2016 funciona con [Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) para proporcionar un control de todo el entorno. La creación de informes híbridos en Azure AD presenta los datos locales y en la nube en un solo lugar. Además, el [portal de autoservicio de restablecimiento de contraseña](working-with-self-service-password-reset.md) es compatible con Microsoft Azure Multi-Factor Authentication (MFA).

## <a name="privileged-identity-management"></a>Privileged Identity Management

Privileged Identity Management controla y administra el acceso administrativo, proporcionando un acceso a los recursos confidenciales que es temporal y se basa en tareas. Esto implica que puede dar a los usuarios solo los permisos que sean necesarios, lo que reduce las posibilidades de que un ciberatacante tenga acceso administrativo completo. Además, Privileged Identity Management extrae y aísla las cuentas administrativas de los bosques de Active Directory existentes.

MIM admite una solución de Privileged Identity Management local para la administración de Active Directory. Para empezar, vea [Usar Privileged Access Management](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## <a name="related-topics"></a>Temas relacionados

- Microsoft Identity Manager sigue estrechamente relacionado con su predecesor, Forefront Identity Manager. Si sigue usando FIM o quiere consultar documentación adicional, eche un vistazo a la [Guía básica de documentación de FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Consideraciones de topología a la hora de implementar MIM](topology-considerations.md) Este artículo presenta varias topologías de implementación que pueden utilizarse.
- [Guía de planeación de capacidad](capacity-planning-guide.md) Puede usar esta guía con los entornos de prueba para comprender el ámbito adecuado de la implementación.