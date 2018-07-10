---
title: Registro dinámico del servicio MIM | Microsoft Docs
description: Habilitar el registro dinámico del servicio sin tener que reiniciar el servicio de administración
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 06/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ''
ms.openlocfilehash: 35d210b06a1e58b3b8f4f08677c2a4151f540246
ms.sourcegitcommit: 88d4e41d8d57f44f4c6c4468fdbd37c2d7e91fd5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2018
ms.locfileid: "36957546"
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

De forma predeterminada, la ubicación del registro será **C:\Archivos de programa\Microsoft Forefront Identity Manager\2010\Service. La cuenta del servicio FIM necesitará permiso de escritura en esta ubicación para generar el registro dinámico.

![Ubicación de carpeta de los registros](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  En caso de que se produzcan errores inesperados (errores de sintaxis en el archivo de configuración Microsoft.ResourceManagement.Service.exe.config u otros errores) se escribirá el mensaje de error correspondiente en el archivo Microsoft.ResourceManagement.Service.exe_Emergency.log en la siguiente ruta de acceso %TMP%, %TEMP% o %USERPROFILE% (el primero que exista).  
> 1. “%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log”
> 2. “%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log”
> 3. “% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log”

Para ver el seguimiento puede utilizar la [herramienta de visor de seguimiento de servicios](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx).

 ![Captura de pantalla del visor de seguimiento de servicios](media/mim-service-dynamic-logging/screen04.png)

# <a name="updates-build-45xx-or-greater"></a>Actualizaciones: compilación 4.5.x.x o posterior

En la compilación 4.5.x.x se ha revisado la característica de registro para especificar que el nivel de registro predeterminado es **"Advertencia"**. El servicio escribe mensajes en dos archivos (antes de la extensión se agregan los índices "00" y "01"). Los archivos se guardan en el directorio “C:\Archivos de programa\Microsoft Forefront Identity Manager\2010\Service”. Cuando el archivo excede el tamaño máximo, el servicio empieza a escribir en otro archivo. Si existe otro archivo, se sobrescribirá. El tamaño máximo predeterminado del archivo es de 1 GB. Para cambiar el tamaño máximo predeterminado, es necesario agregar el parámetro **"maxOutputFileSizeKB"** con el valor de tamaño máximo de archivo en KB en el agente de escucha (consulte el ejemplo siguiente) y, después, reiniciar el servicio MIM. Cuando se inicia el servicio, anexa los registros en el archivo más reciente (si se supera el límite de espacio, se sobrescribe el archivo más antiguo). 

> [!NOTE] Como el servicio comprueba el tamaño de archivo antes de que se sobrescriba el mensaje, el tamaño de archivo puede ser mayor que el tamaño máximo establecido para un mensaje. De forma predeterminada, el tamaño de los registros puede ser aproximadamente de 6 GB (tres >agentes de escucha con dos archivos para el tamaño de 1 GB).

> [!NOTE] La cuenta de servicio debe tener permisos para escribir en el directorio <“C:\Archivos de programa\Microsoft Forefront Identity Manager\2010\Service”>. En caso de que la cuenta de servicio no tenga los suficientes derechos, los >archivos no se crearán.

Ejemplo sobre cómo establecer el tamaño máximo de archivo en 200 MB (200 * 1024 KB) para archivos svclog y 100 MB *(100 * 1024 KB) para archivos txt

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`
