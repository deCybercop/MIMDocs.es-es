---
title: Definición de roles con privilegios para PAM | Microsoft Docs
description: Decida qué roles con privilegios deben administrarse y defina la directiva de administración para cada uno.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 1a368e8e-68e1-4f40-a279-916e605581bc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cfd7c5bee0038740db0ad526072ec248ed9f221d
ms.sourcegitcommit: 210195369d2ecd610569d57d0f519d683ea6a13b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/01/2017
ms.locfileid: "21943764"
---
# <a name="define-roles-for-privileged-access-management"></a>Definir roles para Privileged Access Management

Con Privileged Access Management, puede asignar usuarios a los roles con privilegios que pueden activar según sea necesario para un acceso Just-In-Time. Estos roles se definen manualmente y se establecen en el entorno bastión. Este artículo le guiará a través del proceso para decidir qué roles administrar mediante PAM y cómo definirlos con los permisos y restricciones adecuados.

Un enfoque sencillo para definir los roles de Privileged Access Management es compilar toda la información en una hoja de cálculo. Enumere los roles y use las columnas para identificar los permisos y los requisitos de gobierno.

Los requisitos de gobierno varían dependiendo de las directivas de acceso e identidad existentes o de los requisitos de cumplimiento. Los parámetros para identificar cada rol podrían incluir:

- El propietario del rol.
- Los usuarios candidatos que pueden estar en ese rol.
- Los controles de autenticación, aprobación o notificación que deben asociarse al uso del rol.

Los permisos de rol dependen de las aplicaciones que se administran. En este artículo se usa a Active Directory como aplicación de ejemplo y divide los permisos en dos categorías:

- Las necesarias para administrar el servicio de Active Directory mismo (por ejemplo, configurar la topología de replicación).

- Las necesarias para administrar los datos almacenados en Active Directory (por ejemplo, crear usuarios y grupos).

## <a name="identify-roles"></a>Identificar roles

Comience identificando todos los roles que quiera administrar con PAM. En la hoja de cálculo, cada rol potencial tendrá su propia fila.

Para encontrar los roles apropiados, considere cada aplicación en el ámbito de administración:

- ¿La aplicación se encuentra en el [nivel 0, en el nivel 1 o en el nivel 2](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)?
- ¿Cuáles son los privilegios que afectan a la confidencialidad, a la integridad o a la disponibilidad de la aplicación?
- ¿La aplicación tiene dependencias en otros componentes del sistema? ¿Por ejemplo, tiene dependencias en bases de datos, redes, infraestructura de seguridad, virtualización o plataforma de hospedaje?

Determine cómo agrupar esas consideraciones de la aplicación. Quiere roles que tengan límites claros y que solo proporcionen los permisos suficientes para completar las tareas administrativas comunes dentro de la aplicación.

Siempre quiere diseñar los roles para la asignación de privilegios mínima. Esto puede basarse en las responsabilidades organizacionales actuales (o planificadas) para los usuarios e incluiría el privilegio que requieren las obligaciones del usuario. Además, podría incluir también los privilegios que simplifican las operaciones, sin crear ningún riesgo.

Otras consideraciones en la evaluación de los permisos para incluir un rol son:

- ¿Cuántas personas trabajan en un rol determinado? Si no hay al menos 2, puede que se haya definido de una forma demasiado estricta para que sea útil o que haya definido las obligaciones de una persona determinada.

- ¿Cuántos roles asume una persona? ¿Los usuarios podrían seleccionar el rol correcto para su tarea?

- ¿La población de usuarios y la forma en que interactúan con las aplicaciones sería compatible con Privileged Access Management?

- ¿Es posible separar la administración y auditoría, de manera que un usuario en un rol administrativo no pueda borrar los registros de auditoría de sus acciones?

## <a name="establish-role-governance-requirements"></a>Establecer los requisitos de gobierno de rol

A medida que identifique los roles de candidato, comience a rellenar la hoja de cálculo. Cree columnas para los requisitos pertinentes de su organización. Algunos de los requisitos que se deben considerar son:

- ¿Quién es el propietario de rol que sería responsable de la definición de rol, elegir los permisos y mantener la configuración de gobierno del rol?

- ¿Quiénes son los titulares (usuarios) del rol que realizarán las tareas u obligaciones del rol?

- ¿Qué método de acceso (que se analiza en la sección siguiente) sería apropiado para los titulares del rol?

- ¿Se requiere la aprobación manual por parte de un propietario de rol cuando un usuario activa su rol?

- ¿Se requiere una notificación cuando un usuario activa su rol?

- ¿Usar este rol generará una alerta o notificación en un sistema SIEM con fines de seguimiento?

- ¿Es necesario restringir a los usuarios que activan el rol para que solo puedan iniciar sesión en equipos donde se requiere acceso para realizar los deberes del rol y donde hay una comprobación suficiente del host que podrá proteger los privilegios/credenciales ante un uso incorrecto?

- ¿Es necesario proporcionar una estación de trabajo de administración dedicada a los titulares del rol?

- ¿Qué permisos de aplicación (vea la lista de ejemplo para AD a continuación) están asociados a este rol?

## <a name="select-an-access-method"></a>Seleccionar un método de acceso

Puede haber varios roles en un sistema de administración de acceso con privilegios con los mismos permisos asignados a ellos. Esto puede suceder si distintas comunidades de usuarios tienen requisitos únicos de gobierno de acceso. Por ejemplo, una organización puede aplicar diferentes directivas para sus empleados a jornada completa que para los empleados de TI externos de otra organización.

En algunos casos, se puede asignar permanentemente un usuario a un rol. En ese caso, no es necesario solicitar o activar una asignación de rol. Algunos ejemplos de escenarios de asignación permanente incluyen:

- Una cuenta de servicio administrada en un bosque existente

- Una cuenta de usuario en el bosque existente, con una credencial administrada fuera de PAM. Esto puede ser una cuenta de emergencia. La cuenta de emergencia necesita un rol como "mantenimiento de dominio/controlador de dominio" para corregir problemas, como problemas de mantenimiento de confianza y de controlador de dominio. Como cuenta de emergencia, tendría el rol asignado permanentemente con una contraseña físicamente protegida).

- Una cuenta de usuario en el bosque administrativo que se autentica con una contraseña. Podría tratarse de un usuario que tiene permisos administrativos permanentes y que inicia sesión desde un dispositivo que no es compatible con una autenticación segura.

- Una cuenta de usuario en el bosque administrativo, con una tarjeta inteligente o una tarjeta inteligente virtual (por ejemplo, una tarjeta inteligente sin conexión, necesaria para las tareas de mantenimiento poco frecuentes).

En el caso de las organizaciones preocupadas por la posibilidad de robo o uso incorrecto de credenciales, la guía [Uso de Azure MFA para la activación](use-azure-mfa-for-activation.md) incluye instrucciones sobre cómo configurar MIM para requerir una comprobación fuera de banda adicional en el momento de la activación de rol.

## <a name="delegate-active-directory-permissions"></a>Delegar permisos de Active Directory

Windows Server crea automáticamente grupos predeterminados como "Administradores del dominio" cuando se crean nuevos dominios. Estos grupos simplifican la introducción y pueden ser apropiados para organizaciones más pequeñas. Las organizaciones más grandes o las que requieren más aislamiento de privilegios administrativos deberían vaciar esos grupos y sustituirlos por otros grupos que proporcionan permisos específicos.

Otra limitación del grupo Administradores del dominio es que no puede tener miembros de un dominio externo. Otra limitación es que concede permisos para tres funciones independientes:

- Administrar el propio servicio de Active Directory
- Administrar los datos almacenados en Active Directory
- Habilitar el inicio de sesión remoto en los equipos unidos a un dominio.

En lugar de los grupos predeterminados como administradores del dominio, cree nuevos grupos de seguridad que proporcionan solo los permisos necesarios. Después, debe usar MIM para proporcionar dinámicamente las cuentas de administrador con esas pertenencias a grupos.

### <a name="service-management-permissions"></a>Permisos de administración del servicio

En la tabla siguiente, se brindan ejemplos de permisos que podría ser pertinente incluir en los roles para administrar AD.

| Rol | Descripción |
| ---- | ---- |
| Mantenimiento de dominio/controlador de dominio | La pertenencia al grupo de dominio\administradores permite solucionar problemas y modificar el sistema de operativo del controlador de dominio. Las operaciones como promocionar un nuevo controlador de dominio en un dominio existente en el bosque y en la delegación de roles de AD.
|Administración de controladores de dominio virtuales | Administre máquinas virtuales de controlador de dominio con el software de administración de virtualización. Este privilegio se podrá otorgar a través del control total de todas las máquinas virtuales en la herramienta de administración o mediante la funcionalidad Control de acceso basado en rol (RBAC). |
| Extender esquema | Administre el esquema, incluida la adición de nuevas definiciones de objeto, la alteración de permisos en los objetos de esquema y la alteración de los permisos predeterminados de esquema para los tipos de objeto. |
| Crear copia de seguridad de Base de datos de Active Directory | Realice una copia de seguridad de Base de datos de Active Directory en su totalidad, incluidos todos los secretos otorgados al controlador de dominio y al dominio. |
| Administrar confianzas y niveles funcionales | Cree y elimine confianzas con bosques y dominios externos. |
| Administrar sitios, subredes y replicación | Administre los objetos de topología de replicación de Active Directory, incluida la modificación de sitios, subredes y objetos de vínculo de sitio e inicie las operaciones de replicación. |
| Administrar los GPO | Crear, eliminar y modificar objetos de directiva de grupo en todo el dominio |
| Administrar zonas | Crear, eliminar y modificar las zonas de DNS y los objetos en Active Directory |
| Modificar UO de nivel 0 | Modifique las UO de nivel 0 y los objetos contenidos en Active Directory. |

### <a name="data-management-permissions"></a>permisos de administración de datos

En la tabla siguiente, se ofrecen ejemplos de permisos que podría ser pertinente incluir en los roles para administrar o usar los datos contenidos en AD.

| Rol | Descripción |
| ---- | ---- |
| Modificar UO de administrador de nivel 1                 | Modifique las UO que contienen objetos de administrador de nivel 1 en Active Directory. |
| Modificar UO de administrador de nivel 2                 | Modifique las UO que contienen objetos de administrador de nivel 2 en Active Directory. |
| Administración de cuentas: Crear, eliminar, mover | Modifique cuentas de usuario estándar.                                      |
| Administración de cuentas: Restablecer y desbloquear       | Restablezca las contraseñas y desbloquee las cuentas.                                  |
| Grupo de seguridad: Crear y modificar          | Cree y modifique los grupos de seguridad en Active Directory.              |
| Grupo de seguridad: Eliminar                 | Elimine los grupos de seguridad en Active Directory.                         |
| Administración de GPO                         | Administre todos los GPO en el dominio/bosque que no afecten los servidores de nivel 0.             |
| Unir admin. de PC/local                    | Derechos administrativos locales a todas las estaciones de trabajo.                               |
| Unir admin. de servidor/local                   | Derechos administrativos locales a todos los servidores.                                    |

## <a name="example-role-definitions"></a>Ejemplo de definiciones de rol

La elección de las definiciones de rol dependen del nivel de servidores que administran. También dependerá de la elección de las aplicaciones administradas. Aplicaciones como Exchange o productos empresariales de terceros, como SAP, con frecuencia traerán sus propias definiciones de rol adicionales para la administración delegada.

Las siguientes secciones dan ejemplos de escenarios empresariales típicos.

### <a name="tier-0---administrative-forest"></a>Nivel 0: Bosque administrativo

Los roles adecuados para las cuentas en el entorno bastión podrían incluir:

- Acceso de emergencia al bosque administrativo.
- Administradores de "tarjeta roja": Usuarios que son administradores del bosque administrativo.
- Usuarios que son administradores del bosque de producción.
- Usuarios en los que se delegan los derechos administrativos delegados a las aplicaciones del bosque de producción.

### <a name="tier-0---enterprise-production-forest"></a>Nivel 0: Bosque de producción empresarial

Los roles adecuados para administrar los recursos y las cuentas del bosque de producción de nivel 0 podrían incluir:

- Acceso de emergencia al bosque de producción.
- Administradores de directiva de grupo.
- Administradores de DNS.
- Administradores de PKI.
- Administradores de replicación y topología de AD.
- Administradores de virtualización para servidores de nivel 0.
- Administradores de almacenamiento.
- Administradores anti-malware para servidores de nivel 0.
- Administradores de SCCM para SCCM de nivel 0.
- Administradores de SCOM para SCOM de nivel 0.
- Administradores de copia de seguridad para nivel 0.
- Usuarios de controladores de administrador fuera de banda y de placa base (para la administración en horarios nocturnos o KVM) conectados a hosts de nivel 0.

### <a name="tier-1"></a>Nivel 1

Los roles para la administración y la copia de seguridad de los servidores en nivel 1 podrían incluir:

- Mantenimiento del servidor.
- Administradores de virtualización para servidores de nivel 1.
- Cuenta de examen de seguridad
- Administradores anti-malware para servidores de nivel 1.
- Administradores de SCCM para SCCM de nivel 1.
- Administradores de SCOM para SCOM de nivel 1.
- Administradores de copia de seguridad para servidores de nivel 1.
- Usuarios de controladores de administración fuera de banda y de placa base (para la administración en horarios nocturnos o KVM) conectados a hosts de nivel 1.

Además, los roles para administrar aplicaciones empresariales en nivel 1 podrían incluir:

- Administradores de DHCP.
- Administradores de Exchange.
- Administradores de Skype Empresarial.
- Administradores de la granja de SharePoint.
- Administradores de un servicio en la nube; por ejemplo, un sitio web de la empresa o un DNS público.
- Administradores de sistemas legales, financieros o de gestión del capital humano.

### <a name="tier-2"></a>Nivel 2

Los roles para la administración de equipos y usuarios no administrativos podrían incluir:

- Administradores de cuentas.
- Departamento de soporte técnico.
- Administradores de grupos de seguridad.
- Asistencia de escritorio de la estación de trabajo

## <a name="next-steps"></a>Pasos siguientes

- [Protección del material de referencia de acceso con privilegios](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)
- [Uso de Azure MFA para la activación](use-azure-mfa-for-activation.md)
