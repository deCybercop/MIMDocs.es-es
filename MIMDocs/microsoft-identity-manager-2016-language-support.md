---
title: Idiomas admitidos de Microsoft Identity Manager 2016 SP1 | Microsoft Docs
description: Una lista de idiomas admitidos por Microsoft Identity Manager 2016 SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/23/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.openlocfilehash: 5704e978734bea13f1a362aeb203810f3864205a
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701468"
---
# <a name="supported-languages"></a>Idiomas compatibles

En este artículo se describen los idiomas admitidos y la asignación de actualizaciones de Microsoft Identity Manager 2016 SP1 versión 4.5 o superior.

## <a name="mim-service-and-portal-and-add-ins-and-extensions-language-pack"></a>Paquete de idioma del portal, servicio, complementos y extensiones de MIM 

El paquete de idioma del portal y servicio de MIM de Microsoft admite los 33 idiomas siguientes.  

> [!NOTE]
> En la compilación [4.4.1642.0](https://support.microsoft.com/en-us/help/4021562/hotfix-rollup-package-build-4-4-1642-0-is-available-for-microsoft) se agregó una clave del Registro denominada "OverrideDefaultUILocale" al paquete de idioma de extensiones y complementos de MIM que intentará asignar todos los idiomas similares al idioma que se admite. Por ejemplo, si el idioma de visualización de Windows es ES-CL (español de Chile), o cualquiera ES-\*, intentará asignar ese idioma al idioma ES-ES (español de España).

> [!IMPORTANT]
> El texto del portal y e¡l complemento de SSPR se localizarán, pero no las preguntas sin trabajo adicional. Debe crear flujos de trabajo de autenticación (y MPR y conjuntos de acompañamiento para establecerlos como destino) a las preguntas de destino en cada idioma a la ubicación de destino.

|       Lenguajes        | FIM(4.3.x.x)/MIM(4.4.xx) | MIM(4.5.x.x) |
|-----------------------|--------------------------|--------------|
|       Búlgaro       |          bg-BG           |      bg      |
| Chino (simplificado)  |          zh-CN           |   zh-hans    |
|   Chino (Taiwán)    |          zh-TW           |   zh-hant    |
|       Croata        |          hr-HR           |      hr      |
|         Checo         |          cs-CZ           |      cs      |
|        Danés         |          da-DK           |      da      |
|         Neerlandés         |          nl-NL           |      nl      |
|       Estonio        |          et-EE           |      et      |
|        Francés         |          fr-FR           |      fr      |
|        Finés        |          fi-FI           |      fi      |
|        Alemán         |          de-DE           |      de      |
|         Griego         |          el-GR           |      el      |
|         Hindi         |          hi-IN           |      hi      |
|       Húngaro       |          hu-HU           |      hu      |
|        Italiano        |          it-IT           |      it      |
|       Japonés        |          ja-JP           |      ja      |
|        Coreano         |          ko-KR           |      ko      |
|      Lituano       |          lt-LT           |      lt      |
|        Letón        |          lv-LV           |      lv      |
|       Noruego       |          nb-NO           |    nb-NO     |
|        Polaco         |          pl-PL           |      pl      |
| Portugués (Portugal) |          pt-PT           |      pt      |
|  Portugués (Brasil)  |          pt-BR           |    pt-BR     |
|        Ruso        |          ru-RU           |      ru      |
|       Rumano        |          ro-RO           |      ro      |
|        Español        |          es-ES           |      Sí      |
|        Eslovaco         |          sk-SK           |      sk      |
|        Sueco        |          sv-SE           |      sv      |
|       Esloveno       |          sl-SI           |      sl      |
|   Serbio - Serbia    |  sr-latn-CS (en desuso)  |  sr-Latn-RS  |
|         Tailandés          |          th-TH           |      th      |
|        Turco        |          tr-TR           |      tr      |
|       Ucraniano       |          uk-UA           |      uk      |

## <a name="certificate-management"></a>Administración de certificados 
Microsoft Certificate Management admite los nueve idiomas siguientes. 

|Lenguajes|FIM(4.3.x.x)/MIM(4.4.xx)|Nuevo MIM(4.5.x.x)
|-----|-----|-----|-----|
|Chino (simplificado)|zh-CN|zh-hans|
|Chino (Taiwán)|zh-TW|zh-hant|
|Neerlandés|nl-NL|nl|
|Francés|fr-FR|fr|
|Alemán|de-DE|de|
|Italiano|it-IT|it|
|Japonés|ja-JP|ja|
|Portugués (Portugal)|pt-PT|pt-PT|
|Español|es-ES|Sí|

## <a name="certificate-management-modern-application"></a>Aplicación moderna Certificate Management  
La aplicación moderna Microsoft Certificate Management admite los nueve idiomas siguientes. 

|Lenguajes | [1.0.225.104](https://www.microsoft.com/en-us/download/details.aspx?id=54954) | |
|-----|-----|-----|-----|
|Neerlandés|nl-NL|nl|
|Chino (simplificado)|zh-CN|zh-hans|
|Chino (Taiwán)|zh-TW|zh-hant|
|Checo|cs-CZ|cs|
|Danés|da-DK|da|
|Francés|fr-FR|fr|
|Finés|fi-FI|fi|
|Alemán|de-DE|de|
|Griego|el-GR|el|
|Húngaro|hu-HU|hu|
|Italiano|it-IT|it|
|Japonés|ja-JP|ja|
|Coreano|ko-KR|ko|
|Noruego|nb-NO|nb-NO|
|Polaco|pl-PL|pl|
|Portugués (Portugal)|pt-PT|pt|
|Portugués (Brasil)|pt-BR|pt-BR|
|Ruso|ru-RU|ru|
|Rumano|ro-RO|ro|
|Español|es-ES|Sí|
|Sueco|sv-SE|sv|
|Turco|tr-TR|tr|

## <a name="next-steps"></a>Pasos siguientes

- [Primera implementación](microsoft-identity-manager-deploy.md)
- [Historial de versiones](reference/version-history.md)
