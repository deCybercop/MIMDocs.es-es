---
title: "Uso de la sincronización de Microsoft Identity Manager con AD | Microsoft Docs"
description: "Use los agentes de administración y el servicio de sincronización de MIM para sincronizar las bases de datos de Active Directory y de MIM."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 5e532b67-64a6-4af6-a806-980a6c11a82d
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 54d03fbd03f6c44298139324ea2dc7d945f008bc
ms.openlocfilehash: f84fbbdc8de5cfffc8570c52f8298cc69273c3ee
ms.lasthandoff: 01/24/2017


---

# <a name="install-mim-2016-synchronize-active-directory-and-mim-service"></a>Instalación de MIM 2016: sincronizar Active Directory y el servicio MIM

>[!div class="step-by-step"]
[« Servicio y portal de MIM](install-mim-service-portal.md)

> [!NOTE]
> Este tutorial usa los valores y nombres de ejemplo de una empresa llamada Contoso. Reemplácelos con sus propios valores. Por ejemplo:
> - Nombre del controlador de dominio: **mimservername**
> - Nombre de dominio: **contoso**
> - Contraseña - **Pass@word1**

De manera predeterminada, el servicio de sincronización de MIM no tiene ningún conector configurado.  El primer paso suele consistir en usar el servicio de sincronización de MIM para rellenar la base de datos del servicio MIM con cuentas de Active Directory existentes. Para ello, usará la aplicación del Servicio de sincronización de MIM.

## <a name="create-the-mim-management-agent"></a>Creación del agente de administración de MIM
El agente de administración (MA) de MIM es un conector entre la sincronización de MIM y el servicio MIM. Para crear este conector, use el Asistente para crear el agente de administración.

Al configurar un agente de administración de MIM, debe especificar una cuenta de usuario. Este documento usa **MIMMA** como nombre para esta cuenta.

> [!NOTE]
> La cuenta que use para el agente de administración de MIM debe ser la misma que la que haya especificado durante la instalación del Servicio MIM.

###<a name="to-create-the-mim-ma"></a>Para crear el MA de MIM

1.  Abra el Synchronization Service Manager.

2.  Para abrir el Asistente para creación de agentes de administración, cambie a la página **Acciones de administración** y haga clic en **Crear** en el menú **Acciones**.

3.  En la página **Create Management Agent** (Crear agente de administración), proporcione la siguiente configuración y después haga clic en **Siguiente**.

    -   Agente de administración para: Agente de administración del Servicio FIM

    -   Nombre: MIMMA

4.  En la página **Conectar a base de datos**, proporcione la siguiente configuración y después haga clic en **Siguiente**.

    -   Servidor: localhost

    -   Base de datos: FIMService

    -   Dirección base del servicio MIM: http://localhost:5725

    -   Modo de autenticación: Autenticación integrada de Windows

    -   Nombre de usuario: mimma

    -   Contraseña: Pass@word

    -   Dominio: contoso

5.  En la página **Selected Object Types** (Tipos de objetos seleccionados), compruebe que estén seleccionados los tipos de objeto de esta lista y después haga clic en **Siguiente**.

    -   DetectedRuleEntry

    -   ExpectedRuleEntry

    -   Group

    -   Person

    -   SynchronizationRule

6.  En la página **Atributos seleccionados**, compruebe que estén seleccionados **Mostrar todo** todos los atributos de la lista y después haga clic en **Siguiente**.

7.  En la página **Configure Connector Filter** (Configurar filtro de conector), haga clic en **Siguiente**.

8.  En la página **Configure Object Type Mappings** (Configurar asignaciones de tipo de objeto), agregue la siguiente asignación y después haga clic en **Siguiente**.

    - Seleccione **Persona** en la lista **Tipo de objeto de origen de datos**.
    - Haga clic en **Agregar asignación** para abrir el cuadro de diálogo Asignación.
    - Seleccione **persona** en la lista **Tipo de objeto de metaverso**.
    - Haga clic en **Aceptar** para cerrar el cuadro de diálogo Asignación.
    - Seleccione **Grupo** en la lista **Tipo de objeto de origen de datos**.
    - Haga clic en **Agregar asignación** para abrir el cuadro de diálogo Asignación.
    - Seleccione **grupo** en la lista **Tipo de objeto de metaverso**.
    - Haga clic en **Aceptar** para cerrar el cuadro de diálogo Asignación.

9.  En la página **Configurar el flujo de atributos**, cree las asignaciones del flujo de atributos tal y como se muestra a continuación y luego haga clic en **Siguiente**.

    -   Seleccione **Persona** como tipo de objeto de origen de datos y de metaverso.

    -   Seleccione **Directa** como tipo de asignación.

    -   Por cada fila de la siguiente tabla, realice estos pasos:

        -   Seleccione la **dirección del flujo** que se muestra para esa fila en la tabla.

        -   Seleccione el **atributo Origen de datos** que se muestra para esa fila en la tabla.

        -   Seleccione el **atributo Metaverso** que se muestra para esa fila en la tabla.

        -   Para aplicar la asignación de flujo, haga clic en **Nuevo**.

    | **Atributo de origen de datos** | **Dirección del flujo** | **Atributo de metaverso** |
    |-|-|-|
    | AccountName | Exportar | accountName |
    | DisplayName | Exportar | displayName |
    | Dominio | Exportar | dominio |
    | Correo electrónico | Exportar | mail |
    | EmployeeID | Exportar | employeeID |
    | EmployeeTipo | Exportar | employeeTipo |
    | Nombre | Exportar | firstName |
    | Apellidos | Exportar | lastName |
    | ObjectSID | Exportar | objectSid |

    -   Seleccione **Grupo** como el tipo de origen de datos y de metaverso.

    -   Seleccione **Directa** como tipo de asignación.

    -   Por cada fila de la siguiente tabla, realice estos pasos:

        -   Seleccione la **dirección del flujo** que se muestra para esa fila en la tabla.

        -   Seleccione el **atributo Origen de datos** que se muestra para esa fila en la tabla.

        -   Seleccione el **atributo Metaverso** que se muestra para esa fila en la tabla.

        -   Para aplicar la asignación de flujo, haga clic en **Nuevo**.

    | **Atributo de origen de datos** | **Dirección del flujo** | **Atributo de metaverso** |
    |-|-|-|
    | AccountName | Exportar | accountName |
    | DisplayName | Exportar | displayName |
    | Dominio | Exportar | dominio |
    | Correo electrónico | Exportar | mail |
    | MailNickName | Exportar | mailNickName |
    | Miembro | Exportar | miembro |
    | ObjectSID | Exportar | objectSid |
    | Ámbito | Exportar | ámbito |
    | Tipo | Exportar | tipo |
    | MiembroshipAddWorkflow | Exportar | membershipAddWorkflow |
    | MiembroshipLocked | Exportar | membershipLocked |
    | AccountName | Importe | accountName |
    | DisplayedOwner | Importe | displayedOwner |
    | DisplayName | Importe | displayName |
    | MailNickName | Importe | mailNickName |
    | Miembro | Importe | miembro |
    | Ámbito | Importe | ámbito |
    | Tipo | Importe | tipo |

10.  En la página **Configure Deprovisioning** (Configurar el desaprovisionamiento), haga clic en **Siguiente**.

11.  Para crear al agente de administración, haga clic en **Finalizar** en la página **Configure Extensions** (Configurar extensiones).

## <a name="create-the-ad-management-agent"></a>Creación del agente de administración de Active Directory
El agente de administración de Active Directory es un conector de servicios de dominio de Active Directory. Para crear este conector, use el Asistente para crear el agente de administración.

1. Para abrir el Asistente para creación de agentes de administración, haga clic en **Crear** en el menú **Acciones**.

2. En la página **Create Management Agent** (Crear agente de administración), proporcione la siguiente configuración y después haga clic en **Siguiente**:

    - Agente de administración para: Servicios de dominio de Active Directory
    - Nombre: ADMA

3. En la página **Connect to Active Directory Forest** (Conectar con el bosque de Active Directory), proporcione la siguiente configuración y después haga clic en **Siguiente**:

    - Nombre de bosque: contoso.local
    - Nombre de usuario: administrador
    - Contraseña: &lt;la contraseña de la cuenta&gt;
    - Dominio: contoso

4. En la página **Configure Directory Partitions** (Configurar particiones de directorio), proporcione la siguiente configuración y después haga clic en **Siguiente**:

    - En la lista **Select directory partitions** (Seleccionar particiones de directorio), seleccione **DC=CONTOSO, DC=local**.

    - Para abrir el cuadro de diálogo de selección de contenedores, haga clic en **Containers** (Contenedores).

    - Si desea cambiar el contenedor para que solo MIM administre objetos en un determinado contenedor, haga clic en el nodo **DC=CONTOSO, DC=local** y, a continuación, en el nodo del contenedor correspondiente.

    - Para cerrar el cuadro de diálogo de selección de contenedores, haga clic en **Aceptar**.

5. En la página **Configure Provisioning Hierarchy** (Configurar jerarquía de aprovisionamiento), haga clic en **Siguiente**.

6. En la página **Select Object Types** (Seleccionar tipos de objeto), proporcione la siguiente configuración y después haga clic en **Siguiente**.

    - En la lista **Tipos de objeto**, seleccione el **usuario** y el **grupo**.

7. En la página **Seleccionar atributos**, marque **Mostrar todo**, elija los siguientes atributos y haga clic en **Siguiente**:

    -   company
    -   displayName
    -   employeeID
    -   employeeTipo
    -   givenName
    -   groupTipo
    -   managedBy
    -   manager
    -   miembro
    -   objectSid
    -   sAMAccountName
    -   sAMAccountTipo
    -   sn
    -   unicodePwd
    -   userAccountControl

8. En la página **Configure Connector Filter** (Configurar filtro de conector), haga clic en **Siguiente**.

9. En la página **Configure Join and Projection Rules** (Configurar reglas de unión y proyección), haga clic en **Siguiente**.

10. En la página **Configure Attribute Flow** (Configurar el flujo de atributos), haga clic en **Siguiente**.

11. En la página **Configure Deprovisioning** (Configurar el desaprovisionamiento), haga clic en **Siguiente**.

12. En la página **Configure Extensions** (Configurar extensiones), haga clic en **Finalizar**.


## <a name="create-run-profiles"></a>Crear perfiles de ejecución

Cree perfiles de ejecución para los conectores ADMA y MIMMA.

### <a name="create-run-profiles-for-the-adma-connector"></a>Crear perfiles de ejecución para el conector ADMA

Esta tabla muestra los cinco perfiles de ejecución que va a crear para el conector ADMA:

| Nombre | Tipo |
| ---- | ---- |
| Perfil1 | Importación completa (solo copia intermedia) |
| Perfil2 | Sincronización completa |
| Perfil3 | Importación diferencial (solo copia intermedia) |
| Perfil4 | Sincronización delta |
| Perfil5 | Exportar |

Para crear perfiles de ejecución para el conector ADMA:

1. Abra Synchronization Service Manager y haga clic en **Management Agents** (Agentes de administración), en el menú **Herramientas**.

2. En la lista **Management Agents** (Agentes de administración), seleccione **ADMA**.

3. Para abrir el cuadro de diálogo de Configure Run Profiles for (Configurar perfiles de ejecución para), haga clic en **Configure Run Profiles** (Configurar perfiles de ejecución), en el menú **Acciones**.

4. Siga, en cada perfil de ejecución de la tabla, los pasos que se indican a continuación:

    - Para abrir el Asistente para Configure Run Profile (Configurar perfiles de ejecución), haga clic en **Nuevo perfil**.

    - En el cuadro **Nombre**, escriba el nombre de perfil que se muestra en la tabla y haga clic en **Siguiente**.

    - En la lista **Tipo**, seleccione el tipo de paso que se muestra en la tabla y después haga clic en **Siguiente**.

    - Haga clic en **Finalizar** para crear el perfil de ejecución.

5. Para cerrar el cuadro de diálogo Configure Run Profiles (Configurar perfiles de ejecución), haga clic en **Aceptar**.

### <a name="create-run-profiles-for-the-mimma-connector"></a>Crear perfiles de ejecución para el conector MIMMA

En esta tabla se muestran los cinco perfiles de ejecución del conector MIMMA que coinciden:

| Nombre | Tipo |
| -------- | -------- |
| Perfil1 | Importación completa (solo copia intermedia) |
| Perfil2 | Sincronización completa |
| Perfil3 | Importación diferencial (solo copia intermedia) |
| Perfil4 | Sincronización delta |
| Perfil5 | Exportar |

Para crear perfiles de ejecución para el conector MIMMA:

1. Abra Synchronization Service Manager y haga clic en **Management Agents** (Agentes de administración), en el menú **Herramientas**.

2. En la lista **Management Agents** (Agentes de administración), seleccione **MIMMA**.

3. Para abrir el cuadro de diálogo de Configure Run Profiles for (Configurar perfiles de ejecución para), haga clic en **Configure Run Profiles** (Configurar perfiles de ejecución), en el menú **Acciones**.

4. Siga, en cada perfil de ejecución de la tabla, los pasos que se indican a continuación:

    - Para abrir el Asistente para Configure Run Profile (Configurar perfiles de ejecución), haga clic en **Nuevo perfil**.

    - En el cuadro **Nombre**, escriba el nombre de perfil que se muestra en la tabla y haga clic en **Siguiente**.

    - En la lista **Tipo**, seleccione el tipo de paso que se muestra en la tabla y después haga clic en **Siguiente**.

    - Haga clic en **Finalizar** para crear el perfil de ejecución.

5. Para cerrar el cuadro de diálogo Configure Run Profiles (Configurar perfiles de ejecución), haga clic en **Aceptar**.

## <a name="configure-the-mim-service"></a>Configurar el Servicio MIM

Mediante el portal de MIM, creará la regla de sincronización de entrada de usuario de AD para el servicio MIM.

Para crear la regla de sincronización de entrada de usuario de AD:

1. En la barra de navegación de la página principal del portal de MIM, haga clic en **Administración**.

2. Para abrir la página Reglas de sincronización, haga clic en **Reglas de sincronización**.

3. Para abrir el Asistente para creación de reglas de sincronización, haga clic en **Nueva** en la barra de herramientas.

4. En la pestaña **General**, proporcione la siguiente información y después haga clic en **Siguiente**:

    -   Nombre para mostrar: Regla de sincronización de entrada de usuario de AD
    -   Dirección del flujo de datos: Entrante

5. En la pestaña **Ámbito**, proporcione la siguiente información y después haga clic en **Siguiente**:

    -   Tipo de recurso de metaverso: person
    -   Sistema externo: ADMA
    -   Tipo de recurso de sistema externo: user

6. En la pestaña **Relación**, proporcione la siguiente información y después haga clic en **Siguiente**:

    -   Para configurar Criterios de relación, seleccione **ObjectSID** en las listas MetaverseObject:person(Attribute) y ConnectedSystemObject:person (Attribute).

    -   Seleccione **Crear recurso en FIM**.

7. En la página **Inbound Attribute Flow** (Flujo del atributo entrante), proporcione la siguiente información y después haga clic en **Siguiente**:

    | Regla de flujo | Origen | Destination |
    |-|-|-|
    |Regla 1|samAccountName|accountName|
    |Regla 2|displayName|displayName|
    |Regla 3|EmployeeTipo|employeeTipo|
    |Regla 4|givenName|firstName|
    |Regla 5|sn|lastName|
    |Regla 6|Manager|manager|
    |Regla 7|objectSID|ObjectSID|
    |Regla 8|"Contoso"|dominio|

    Por cada fila de esta tabla, realice los pasos siguientes:

    - Para abrir el cuadro de diálogo de Definición de flujo, haga clic en **Nuevo flujo de atributo**.

    - En la pestaña **Origen**, seleccione el atributo que se muestra para esa fila en la tabla.

    - En la pestaña **Destino**, seleccione el atributo que se muestra para esa fila en la tabla.

    - Para aplicar la configuración de flujo de atributo, haga clic en **Aceptar**.

8. En la pestaña **Resumen**, haga clic en **Enviar**.

## <a name="initialize-the-testing-environment"></a>Inicializar el entorno de pruebas
Debe realizar cuatro pasos antes de probar la configuración de MIM con datos de AD:

### <a name="enable-provisioning"></a>Habilitación del aprovisionamiento

1. Abra el Synchronization Service Manager.

2. Para abrir el cuadro de diálogo Opciones, haga clic en **Opciones** en el menú **Herramientas**.

3. Seleccione **Enable Synchronization Rule Provisioning** (Habilitar aprovisionamiento de reglas de sincronización).

4. Para cerrar el cuadro de diálogo Opciones, haga clic en **Aceptar**.

### <a name="initialize-the-mimma"></a>Inicialización del MIMMA

Ejecute un ciclo de sincronización completo en este conector. El ciclo completo consta de los siguientes perfiles de ejecución:

- Importación completa
- Sincronización completa
- Exportar
- Importación diferencial

Siga estos pasos para ejecutar cada uno de los cuatro perfiles de ejecución.

1. Abra Synchronization Service Manager y haga clic en **Management Agents** (Agentes de administración), en el menú **Herramientas**.

2. En la lista **Management Agents** (Agentes de administración), seleccione **MIMMA**.

3. Para abrir el cuadro de diálogo Run Management Agent (Ejecutar agente de administración), haga clic en **Ejecutar** en el menú **Acciones**.

4. Siga, en cada perfil de ejecución de la tabla anterior, los pasos que se indican a continuación:

    - Para abrir el cuadro de diálogo Run Management Agent (Ejecutar agente de administración), haga clic en **Ejecutar** en el menú **Acciones**.

    - En la lista **Run profiles** (Perfiles de ejecución), seleccione aquel que quiere configurar.

    - Para iniciar el perfil de ejecución, haga clic en **Aceptar**.

#### <a name="configure-attribute-flow-precedence"></a>Configurar la prioridad del flujo de atributos

Durante la inicialización del conector de MIM, las reglas de sincronización configuradas se han introducido en el metaverso.

Ajuste la prioridad del flujo de atributos para los atributos aportados por este conector con el fin de asegurarse de que los atributos que ya estén en AD puedan introducirse en el metaverso y más adelante en la base de datos del Servicio MIM.

### <a name="initialize-the-adma"></a>Inicialización del ADMA

Para inicializar el conector de Active Directory, debe ejecutar una importación completa y una sincronización completa en él. La importación completa trae los objetos existentes desde AD hasta el espacio del conector. La sincronización completa actualiza las reglas de sincronización para que coincidan con las del conector de MIM.

1. Abra Synchronization Service Manager y haga clic en **Management Agents** (Agentes de administración), en el menú **Herramientas**.

2. En la lista **Management Agents** (Agentes de administración), seleccione **ADMA**.

3. Para abrir el cuadro de diálogo Run Management Agent (Ejecutar agente de administración), haga clic en **Ejecutar** en el menú **Acciones**.

4. Siga, en cada perfil de ejecución de la tabla anterior, los pasos que se indican a continuación:

    - Para abrir el cuadro de diálogo Run Management Agent (Ejecutar agente de administración), haga clic en **Ejecutar** en el menú **Acciones**.

    - En la lista **Run profiles** (Perfiles de ejecución), seleccione aquel que quiere configurar.

    - Para iniciar el perfil de ejecución, haga clic en **Aceptar**.

### <a name="populate-the-mim-service-database"></a>Rellenar la base de datos del Servicio MIM

Para rellenar la base de datos del Servicio MIM con los objetos, debe ejecutar un ciclo de sincronización en el conector MIMMA. El ciclo consiste en:

- Exportar
- Importación completa
- Sincronización completa

Siga estos pasos para ejecutar cada uno de los tres perfiles de ejecución.

1. Abra Synchronization Service Manager y haga clic en **Management Agents** (Agentes de administración) en el menú **Herramientas**.

2. Seleccione **MIMMA** en la lista **Management Agents** (Agentes de administración).

3. Haga clic en **Ejecutar** en el menú **Acciones** para abrir el cuadro de diálogo Run Management Agent (Ejecutar agente de administración).

4. Siga, en cada perfil de ejecución de la tabla anterior, los pasos que se indican a continuación:

    - Haga clic en **Ejecutar** en el menú **Acciones** para abrir el cuadro de diálogo Run Management Agent (Ejecutar agente de administración).
    - Seleccione el perfil de ejecución que quiera ejecutar en la lista **Run profiles** (Perfiles de ejecución).
    - Haga clic en **Aceptar** para iniciar el perfil de ejecución.

>[!div class="step-by-step"]
[« Servicio y portal de MIM](install-mim-service-portal.md)

