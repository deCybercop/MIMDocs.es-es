---
title: Instalación de BHOLD Access Management Connector | Microsoft Docs
description: El módulo BHOLD Access Management Connector admite la sincronización inicial y continua de datos
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 60886a84c6105e94a2cd3d42f17b86b2d69c8c0a
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358608"
---
# <a name="access-management-connector-installation"></a>Instalación de Access Management Connector

El módulo BHOLD Suite Access Management Connector admite la sincronización inicial y continua de los datos en BHOLD. Access Management Connector funciona con el servicio de sincronización de Microsoft Identity Manager (MIM) para mover datos entre la base de datos de BHOLD Core, el metaverso de FIM 2010, las aplicaciones de destino y los almacenes de identidades. Después de instalar el módulo Access Management Connector, podrá crear agentes de administración de FIM que controlen el flujo de datos entre BHOLD y MIM.

## <a name="access-management-connector-software-requirements"></a>Requisitos de software de Access Management Connector

Para poder instalar el módulo Access Management Connector, debe instalar Microsoft .NET Framework 4. Para más información sobre .NET Framework 4 y obtener instrucciones de instalación, vea la [página principal de Microsoft .NET](http://www.microsoft.com/net).
Debe instalar Access Management Connector en un equipo que ejecute el servicio de sincronización de FIM de MIM.

## <a name="access-management-connector-setup"></a>Configuración de Access Management Connector

Para instalar el módulo Access Management Connector, inicie sesión como miembro del grupo Administradores del dominio, descargue el archivo siguiente y ejecútelo como administrador en el servidor en el que quiere instalar el módulo BHOLD FIM Integration:

- AccessManagementConnector.msi

Para ejecutar el archivo de programa como administrador, haga clic con el botón derecho en el archivo y luego haga clic en **Ejecutar como administrador**.

## <a name="next-steps"></a>Pasos siguientes

- [Instalación de BHOLD FIM Integration](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx). Para habilitar el autoservicio de roles del usuario final, puede instalar el módulo BHOLD FIM Integration.
- [Guía de instalación de BHOLD](bhold-installation-guide.md)
- [Referencia para desarrolladores de BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historial de versiones de BHOLD](../reference/version-bhold-history.md)
