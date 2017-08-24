---
title: "Registro dinámico del servicio MIM | Microsoft Docs"
description: "Habilitar el registro dinámico del servicio sin tener que reiniciar el servicio de administración"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
ms.openlocfilehash: 96dcd03616a0b63fc7cc9806446ebb36b3c81fa0
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2017
---
# <a name="mim-sp1-4414360--service-dynamic-logging"></a>Registro dinámico del servicio MIM SP1 (4.4.1436.0)
Hemos introducido una nueva funcionalidad de registro en 4.4.1436.0. Esto permite que los administradores y los ingenieros de soporte técnico activen el registro sin tener que reiniciar el servicio de administración.

Una vez instalado, verá la siguiente línea nueva en Microsoft.resourcemanagement.Service.exe.config denominada

*   Línea 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*   Línea 8: ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*   Línea 266: ``</system.diagnostics> ``

![Secciones resaltadas en las que se muestran las nuevas entradas del registro dinámico](media/mim-service-dynamic-logging/screen01.png)

Podrá encontrar los niveles de registro dinámico [aquí](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3)

- Critical = el servicio de nivel predeterminado solo escribirá en eventos críticos
- Actualizar la línea 8 (dynamicLogging mode=“true” loggingLevel=“Critical”) con el valor de registro preferido

Configuración del registro dinámico en la línea 266: Microsoft.ResourceManagement.Service.exe.config

![Secciones resaltadas en las que se muestran líneas con las distintas áreas de registro disponibles](media/mim-service-dynamic-logging/screen02.png)

De forma predeterminada, la ubicación del registro será **C:\Archivos de programa\Microsoft Forefront Identity Manager\2010\Service**. La cuenta del servicio FIM necesitará permiso de escritura en esta ubicación para generar el registro dinámico.

![Ubicación de carpeta de los registros](media/mim-service-dynamic-logging/screen03.png)

 >[!NOTE]
 En caso de que se produzcan errores inesperados (errores de sintaxis en el archivo de configuración Microsoft.ResourceManagement.Service.exe.config u otros errores) se escribirá el mensaje de error correspondiente en el archivo Microsoft.ResourceManagement.Service.exe_Emergency.log en la siguiente ruta de acceso %TMP%, %TEMP% o %USERPROFILE% (el primero que exista).  
1. “%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log”
2. “%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log”
3. “% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log”

Para ver el seguimiento puede utilizar la [herramienta de visor de seguimiento de servicios](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx).

 ![Captura de pantalla del visor de seguimiento de servicios](media/mim-service-dynamic-logging/screen04.png)
