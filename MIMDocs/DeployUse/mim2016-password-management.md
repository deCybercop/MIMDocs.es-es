---
title: "Administración de contraseñas de Microsoft Identity Manager 2016 | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 05/11/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 0a5a3f28af58dd59ab805f2836ffeb88f3508ae0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="microsoft-identity-manager-2016-password-management"></a>Administración de contraseñas de Microsoft Identity Manager 2016

Administrar contraseñas para varias cuentas de usuario es una de las complejidades de administrar un entorno empresarial con varios orígenes de datos. Microsoft Identity Manager 2016 (MIM) proporciona dos soluciones de administración de contraseñas:

-   Sincronización de contraseñas: usa el servicio de notificación de cambio de contraseña (PCNS) para capturar cambios de contraseña desde Active Directory y propagarlos a otros orígenes de datos conectados.

-   Administración de cambios de contraseña basados en el usuario: usa el Instrumental de administración de Windows (WMI) a través del Servicio de asistencia basado en web y las aplicaciones de restablecimiento de contraseña de autoservicio.

Con la sincronización de contraseñas y la administración de cambios de contraseña basados en el usuario, puede:

-   Reducir el número de contraseñas diferentes que los usuarios tienen que recordar.

-   Establecer o cambiar contraseñas simultáneamente en varias cuentas del usuario a la misma contraseña.

-   Permitir que los usuarios cambien sus propias contraseñas en Active Directory y realizar el cambio de contraseña en los demás sistemas.

-   Eliminar el riesgo de crear una contraseña o un almacén de credenciales adicional.

-   Sincronizar contraseñas en varios orígenes de datos con Active Directory como el origen de autoridad.

-   Realizar operaciones de administración de contraseñas en tiempo real, independientemente de las operaciones de MIM.

## <a name="password-extensions"></a>Extensiones de contraseña

Los agentes de administración de los servidores de directorio admiten el cambio de contraseña y las operaciones de configuración de manera predeterminada. Para los agentes de administración de conectividad extensible, base de datos y basados en archivos, que no admiten el cambio de contraseña y las operaciones de configuración de manera predeterminada, puede crear una biblioteca de vínculos dinámicos (DLL) de extensión de contraseña .NET.
La DLL de extensión de contraseña .NET se llama siempre que se invoque una llamada de configuración o cambio de contraseña para cualquiera de estos agentes de administración. Las opciones de extensión de contraseña están configuradas para estos agentes de administración en Synchronization Service Manager. Para obtener más información sobre la configuración de extensiones de contraseña, vea la Referencia del desarrollador de FIM.

| La administración de contraseñas se admite de manera predeterminada en los agentes de administración para: | Con el uso de una extensión de contraseña, la administración de contraseñas también se admite en los agentes de administración para: |
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Active Directory                                                          | Archivos de texto de par atributo-valor                                                                    |
| Active Directory Lightweight Directory Services (ADLDS)                   | Archivos de texto delimitado                                                                               |
| IBM Directory Server                                                      | Lenguaje de marcado de servicios de directorio (DSML)                                                          |
| Lotus Notes                                                               | Conectividad extensible                                                                            |
| Novell eDirectory                                                         | Archivos de texto de ancho fijo                                                                             |
| Servidores de directorio Sun y Netscape                                        | IBM DB2 Universal Database                                                                         |
|                                                                           | Formato de intercambio de datos LDAP (LDIF)                                                                |
|                                                                           | Microsoft SQL Server                                                                               |
|                                                                           | Base de datos de Oracle                                                                                    |

## <a name="password-synchronization"></a>Sincronización de contraseñas


La sincronización de contraseñas funciona con el servicio de notificación de cambio de contraseña (PCNS) en un dominio de Active Directory, y permite los cambios de contraseña que se originan desde Active Directory para que se propaguen automáticamente en otros orígenes de datos conectados. MIM consigue esto ejecutándose como un servidor de llamada a procedimiento remoto (RPC) que capta una notificación de cambio de contraseña desde un controlador de dominio de Active Directory. Cuando la solicitud de cambio de contraseña se recibe y autentica, se procesa mediante MIM y se propaga a los agentes de administración adecuados.

>[!IMPORTANT]
MIM no admite la sincronización de contraseñas bidireccional. Configurar la sincronización de contraseñas bidireccional puede crear un bucle, que consumirá los recursos del servidor y tendrá un posible efecto negativo tanto en Active Directory como en MIM.

El PCNS se ejecuta en cada controlador de dominio de Active Directory. Los sistemas que reciben las notificaciones de contraseña se conocen como destinos. Su servidor de MIM debe configurarse como un destino de PCNS en Active Directory antes de que se envíen las notificaciones de contraseña. La configuración PCNS debe definir un grupo de inclusión y, opcionalmente, un grupo de exclusión. Estos grupos se usan para restringir el flujo de contraseñas confidenciales desde el dominio. Por ejemplo, para enviar contraseñas a todos los usuarios, pero no enviar contraseñas administrativas, puede decidir usar Usuarios del dominio como el grupo de inclusión y Admins. del dominio como el grupo de exclusión. Para obtener más información sobre la configuración del servicio de notificación de cambio de contraseña, vea [Using Password Synchronization](https://technet.microsoft.com/library/jj590288(v=ws.10).aspx) (Usar la sincronización de contraseñas).

Los componentes implicados en el proceso de sincronización de contraseñas son:

-   **Servicio de notificación de cambio de contraseña (Pcnssvc.exe)**: el servicio de notificación de cambio de contraseña se ejecuta en un controlador de dominio y es responsable de recibir las notificaciones de cambio de contraseña del filtro de contraseña local, colocarlos en el servidor de destino que ejecuta MIM y usar RPC para entregar las notificaciones. El servicio cifra la contraseña y garantiza que esta permanezca segura hasta que se entregue correctamente en el servidor de destino que ejecuta MIM.

-   **Nombre de entidad de seguridad de servicio (SPN)**: el SPN es una propiedad del objeto de cuenta en Active Directory que se usa mediante el protocolo Kerberos para autenticar mutuamente el PCNS y el destino. El SPN garantiza que el PCNS se autentique en el servidor correcto que ejecuta MIM, y que ningún otro servicio pueda recibir las notificaciones de cambio de contraseña. El SPN se crea y se asigna mediante la herramienta setspn.exe. Para obtener más información sobre la configuración del SPN, vea Using Password Synchronization (Usar la sincronización de contraseñas).

-   **Filtro de notificación de cambio de contraseña (Pcnsflt.dll)**: el filtro de contraseña se usa para obtener contraseñas de texto simple desde Active Directory. Este filtro se carga mediante la autoridad de seguridad local (LSA) en cada controlador de dominio de Windows Server que participa en la distribución de contraseñas en un servidor de destino que ejecuta MIM. Una vez que el filtro se ha instalado y el controlador de dominio se ha reiniciado, el filtro comienza a recibir notificaciones de cambio de contraseña para los cambios de contraseña que se originan en el controlador de dominio. El filtro de notificación de contraseñas se ejecuta de manera simultánea con otros filtros que se están ejecutando en el controlador de dominio.

-   **Utilidad de la configuración del servicio de notificación de cambio de contraseña (Pcnscfg.exe)**: la utilidad pcnscfg.exe se usa para administrar y mantener los parámetros de configuración del servicio de notificación de cambio de contraseña almacenados en Active Directory. Estos parámetros de configuración, como la definición de los servidores de destino, el intervalo de recuperación de la cola de contraseñas y habilitar o deshabilitar un servidor de destino, se usan al autenticar y enviar las notificaciones de contraseña al servidor de destino que ejecuta MIM.
    La configuración del servicio se almacena en Active Directory, de manera que solo es necesario actualizar la configuración en un controlador de dominio. Active Directory replica el cambio en todos los demás controladores de dominio.

-   **Servidor de llamada a procedimiento remoto (RPC) en el servidor que ejecuta MIM**: cuando la sincronización de contraseñas está habilitada, el servidor de RPC en el servidor que ejecuta MIM se inicia, permitiendo que reciba notificaciones del servicio de notificación de cambio de contraseña. RPC selecciona dinámicamente un intervalo de puertos que se va a usar. Si necesita que MIM se comunique con el bosque de Active Directory mediante un firewall, debe abrir un intervalo de puertos.

-   **DLL de extensión de contraseña**: la DLL de extensión de contraseña proporciona una manera de implementar operaciones de cambio o configuración de contraseña mediante una extensión de reglas para cualquier agente de administración basado en archivos, conectividad extensible o base de datos.
    Esto se consigue creando un atributo cifrado solo para exportación denominado "export_password" que no existe realmente en el directorio conectado pero al que se puede acceder y establecer en extensiones de reglas de aprovisionamiento o puede usarse durante el flujo de atributos de exportación. Para obtener más información sobre la configuración de extensiones de contraseña, vea la [Referencia del desarrollador de FIM](https://msdn.microsoft.com/library/windows/desktop/ee652263(v=vs.100).aspx).

## <a name="preparing-for-password-synchronization"></a>Preparar la sincronización de contraseñas

Antes de que configure la sincronización de contraseñas para su entorno de MIM y Active Directory, compruebe lo siguiente:

-   MIM está instalado según las instrucciones de instalación.

-   Los agentes de administración para los orígenes de datos conectados que se van a administrar para la sincronización de contraseñas ya están creados y los objetos se están uniendo y sincronizando correctamente.

Para configurar la sincronización de contraseñas:

-   Extienda el esquema de Active Directory para agregar las clases y los atributos necesarios para instalar y ejecutar el servicio de notificación de cambio de contraseña (PCNS).

-   Instale el PCNS en cada controlador de dominio.

-   Configure el nombre de entidad de seguridad de servicio (SPN) en Active Directory para la cuenta de servicio de MIM.

-   Configure el PCNS para que se comunique con el servicio MIM de destino.

-   Configure los agentes de administración para los orígenes de datos conectados que se van a administrar para la sincronización de contraseña.

-   Habilite la sincronización de contraseña en MIM.

Para obtener más información sobre la configuración de la sincronización de contraseña, vea Using Password Synchronization (Usar la sincronización de contraseñas).

## <a name="password-synchronization-process"></a>Proceso de sincronización de contraseñas

El proceso de sincronizar una solicitud de cambio de contraseña desde un controlador de dominio de Active Directory en otros orígenes de datos conectados se muestra en el diagrama siguiente:

1.  El usuario inicia la solicitud de cambio de contraseña presionando Ctrl+Alt+Supr. La solicitud de cambio de contraseña, incluida la contraseña nueva, se envía al controlador de dominio más cercano.

2.  El controlador de dominio registra la solicitud de cambio de contraseña y se lo notifica al filtro de notificación de cambio de contraseña (Pcnsflt.dll).

3.  El filtro de notificación de cambio de contraseña pasa la solicitud al servicio de notificación de cambio de contraseña (PCNS).

4.  El PCNS comprueba la solicitud de cambio de contraseña, después autentica el nombre de entidad de seguridad de servicio (SPN) con Kerberos y reenvía la solicitud de cambio de contraseña en una RPC cifrada al servidor de destino de MIM.

5.  MIM valida el controlador de dominio de origen, después usa el nombre de dominio para buscar el agente de administración que sirve a ese dominio y usa la información de cuenta de usuario de la solicitud de cambio de contraseña para buscar el objeto correspondiente en el espacio conector.

6.  Mediante la información de la tabla combinada, MIM determina los agentes de administración que reciben el cambio de contraseña, y la inserta en estos.

## <a name="password-synchronization-security"></a>Seguridad de sincronización de contraseñas

Se han tratado los siguientes problemas de seguridad de sincronización de contraseñas:

-   Autenticación desde el origen de contraseña: cuando la notificación de cambio de contraseña se recibe, MIM y el controlador de dominio de origen realizan la autenticación Kerberos para garantizar que tanto el destinatario como el remitente sean válidos. Tras recibir una notificación de cambio de contraseña, MIM garantiza que el autor de la llamada tiene una cuenta en el contenedor Controladores de dominio del dominio al que pertenece.

-   Error en la sincronización de contraseña en un origen de datos de destino debido a una conexión no segura: si el agente de administración se ha configurado para necesitar una conexión segura pero no se detecta ninguna, se produce un error en la sincronización.
    La sincronización se sigue produciendo si el agente de administración se ha configurado para permitir conexiones no seguras. Permitir conexiones no seguras debe habilitarse solo después de examinar y entender los riesgos que conlleva.

-   Almacenamiento seguro de contraseñas: MIM solo almacena contraseñas cifradas temporalmente. Todas las contraseñas recibidas por MIM durante una operación de notificación de cambio de contraseña se cifran tan pronto como entran en el proceso de MIM.
    El momento en el que se envían correctamente al origen de datos conectado de destino, se descifran y la memoria que almacena la contraseña se borra inmediatamente. Si se produce un error en la operación al escribirse en el origen de datos conectado de destino, la contraseña cifrada se almacena hasta que se han intentado todos los intentos de reintento y, después, se borra de la memoria.

-   Proteger las colas de contraseña: las contraseñas almacenadas en colas de contraseñas del PCNS se cifran hasta que se entregan.

## <a name="password-synchronization-error-recovery-scenarios"></a>Escenarios de recuperación de errores de la sincronización de contraseñas

De manera ideal, cuando un usuario cambia una contraseña, el cambio se sincroniza sin errores. Los escenarios siguientes describen cómo MIM se recupera de los errores de sincronización comunes:

-   **Error en la notificación de contraseña de Active Directory a MIM**: esto puede producirse si la red está inactiva o si el servidor que ejecuta MIM no está disponible. La notificación de cambio de contraseña permanece en cola localmente en el controlador de dominio mediante el PCNS. El PCNS vuelve a intentar la notificación según su configuración del intervalo de reintento.

-   **Error en la sincronización de contraseñas en un origen de datos de destino**: esto también puede ocurrir si la red está inactiva o si el origen de datos de destino no está disponible.
    La notificación de cambio de contraseña permanece en cola y se reintenta según la configuración del agente de administración para el intento y el intervalo de reintento. Todas las contraseñas están cifradas mientras están almacenadas para el reintento, y se eliminan cuando la operación se realiza correctamente o se alcanzan los límites de reintento.

-   **Activar un servidor en espera semiactiva que ejecuta MIM después de un error**: en el caso de que se produzca un error en el servidor principal que ejecuta MIM, puede configurar un servidor en espera semiactiva para la sincronización de contraseña, y activarlo sin perder los cambios de la contraseña. Para obtener más información, vea [MIISactivate: Server Activation Tool](https://technet.microsoft.com/library/jj590194(v=ws.10).aspx) (MIISactivate: Herramienta de activación del servidor)

Algunos errores son lo suficientemente graves como para que ningún número de reintentos produzca probablemente una operación correcta. En estos casos, se registra un evento de error y se detiene el proceso. Los siguientes eventos no se reintentan:

| Evento | Gravedad    | Descripción                                                                                                                                                            |
|-------|-------------|-----------|
| 6919  | Información de | No se ha realizado una operación de establecimiento de sincronización de contraseña porque la marca de tiempo no estaba actualizada.                                                                      |
| 6921  | Error       | No se ha procesado la operación de establecimiento de sincronización de contraseña porque la administración de contraseñas no está habilitada en el agente de administración de destino.                                |
| 6922  | Error       | No se ha procesado la operación de establecimiento de sincronización de contraseña porque la administración de contraseñas no está configurada en el agente de administración de destino.                             |
| 6923  | Advertencia     | No se ha procesado la operación de establecimiento de sincronización de contraseña porque el objeto de espacio conector de destino no se ha encontrado en el directorio conectado.                  |
| 6927  | Error       | Se ha producido un error en la operación de establecimiento de sincronización de contraseña porque la contraseña no cumple la directiva de contraseñas del sistema de destino.                                      |
| 6928  | Error       | Se ha producido un error en la operación de establecimiento de sincronización de contraseña porque la extensión de la contraseña para el agente de administración de destino no está configurada para admitir operaciones de establecimiento de contraseña. |

## <a name="user-based-password-change-management"></a>Administración del cambio de contraseña basado en el usuario

MIM proporciona dos aplicaciones web que usan el Instrumental de administración de Windows (WMI) para restablecer contraseñas. Como con la sincronización de contraseñas, activa la administración de contraseñas cuando configura el agente de administración en el Diseñador del agente de administración. Para obtener información sobre la administración de contraseñas y WMI, vea la Referencia del desarrollador de MIM.

MIM crea dos grupos de seguridad durante la instalación que admiten específicamente las operaciones de administración de contraseñas:

-   FIMSyncBrowse: los miembros de este grupo tienen permiso para recopilar información sobre las cuentas del usuario cuando realizan operaciones de búsqueda con consultas de WMI.

-   FIMSyncPasswordSet: los miembros de este grupo tienen permiso para realizar operaciones de cambio de contraseña, establecimiento de contraseña y búsqueda en cuentas con las interfaces de administración de contraseñas de WMI.
