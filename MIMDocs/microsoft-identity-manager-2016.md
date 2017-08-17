---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: "MIM incluye las capacidades de administración de acceso de FIM 2010 y le ayuda a administrar usuarios, credenciales, directivas y acceso dentro de su organización."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b3cdc1a71b6e9eb14a132429ea66bb4ab33fe3c4
ms.sourcegitcommit: 0a78e39976cd03225a8e24a508e9ee23585e67cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016
Microsoft Identity Manager (MIM) 2016 se basa en las funcionalidades de administración de identidades y acceso de [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Al igual que su predecesor, MIM le ayuda a administrar usuarios, credenciales, directivas y acceso en la organización.  Además, MIM 2016 proporciona una experiencia híbrida, capacidades de administración de acceso con privilegios y compatibilidad con nuevas plataformas.

Esta versión de Microsoft Identity Manager proporciona nuevas características como Privileged Identity Management y la compatibilidad de Certificate Management con el acceso de API de REST. En Certificate Management se ha agregado compatibilidad para las topologías de varios bosques, una aplicación de Windows para la administración del ciclo de vida de tarjetas inteligentes virtuales y certificados, eventos actualizados y capacidades de solución de problemas. Los escenarios de autoservicio incluyen ahora el desbloqueo de cuentas y la puerta de Azure MFA (autenticación multifactor) para restablecer contraseñas.

## <a name="hybrid-experience"></a>Experiencia híbrida
Microsoft Identity Manager 2016 funciona junto con Azure AD para proporcionarle control sobre todo el entorno. La creación de informes híbridos en Azure AD presenta los datos locales y en la nube en un solo lugar. El portal de autoservicio de restablecimiento de contraseña también es compatible con la autenticación multifactor (MFA) de Azure.

## <a name="privileged-identity-management"></a>Privileged Identity Management
Privileged Identity Management controla y administra el acceso administrativo, proporcionando un acceso a los recursos confidenciales que es temporal y se basa en tareas. Esto implica que puede dar a los usuarios solo los permisos que sean necesarios, lo que reduce las posibilidades de que un ciberatacante tenga acceso administrativo completo. Además, Privileged Identity Management extrae y aísla las cuentas administrativas de los bosques de Active Directory existentes.

MIM admite una solución de Privileged Identity Management local para la administración de Active Directory. Para empezar, vea [Usar Privileged Access Management](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## <a name="related-topics"></a>Temas relacionados

- Microsoft Identity Manager sigue estrechamente relacionado con su predecesor, Forefront Identity Manager. Si sigue usando FIM o quiere consultar documentación adicional, eche un vistazo a la [Guía básica de documentación de FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Consideraciones de topología a la hora de implementar MIM](topology-considerations.md)
- [Guía de planificación de capacidad](capacity-planning-guide.md)