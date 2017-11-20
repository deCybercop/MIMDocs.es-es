---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: Proceso para crear usuarios en ADDS con Microsoft Identity Manager 2016
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 171aa1a2e19ea9f78f9fadbc7368404702095d71
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2017
---
# <a name="how-do-i-provision-users-to-ad-ds"></a>Procedimiento para aprovisionar usuarios en AD DS

Se aplica a: Microsoft Identity Manager 2016 SP1 (MIM)

Un requisito básico de un sistema de administración de identidades es la capacidad de aprovisionar recursos en un sistema externo.

En esta guía se detallan los principales bloques de creación que intervienen en el proceso de aprovisionamiento de usuarios desde Microsoft® Identity Manager (MIM) 2016 en Active Directory® Domain Services (AD DS); se describe cómo comprobar si el escenario funciona como está previsto; se proporcionan sugerencias para administrar usuarios de Active Directory mediante MIM 2016 y se mencionan otras fuentes de información.

## <a name="before-you-begin"></a>Antes de empezar


En esta sección, encontrará información sobre el ámbito de este documento. En general, estas guías de procedimientos van dirigidas a lectores que ya tienen una experiencia básica en el proceso de sincronización de objetos con MIM, tal y como se explica en las [guías de inicio](http://go.microsoft.com/FWLink/p/?LinkId=190486) relacionadas.

### <a name="audience"></a>Público


Esta guía está destinada a profesionales de tecnología de la información que ya tienen un conocimiento básico de cómo funciona el proceso de sincronización de MIM y que están interesados en adquirir experiencia práctica y más información conceptual sobre escenarios concretos.

### <a name="prerequisite-knowledge"></a>Conocimiento básico previo


En este documento se da por hecho que tiene acceso a una instancia en ejecución de MIM y, además, que tiene experiencia en configurar escenarios de sincronización sencillos, como se describe en los siguientes documentos:

-   [Introduction to Inbound Synchronization](http://go.microsoft.com/FWLink/p/?LinkId=189652) (Introducción a la sincronización de entrada)

-   [Introduction to Outbound Synchronization](http://go.microsoft.com/FWLink/p/?LinkId=189653) (Introducción a la sincronización saliente)

El contenido del presente artículo está pensado para ampliar estos documentos de introducción.

### <a name="scope"></a>Ámbito


El escenario descrito en este documento se ha simplificado para satisfacer los requisitos de un entorno de laboratorio básico. El objetivo es proporcionarle una descripción de los conceptos y tecnologías tratados.

Este documento sirve para desarrollar una solución que conlleve administrar grupos en AD DS mediante MIM.

### <a name="time-requirements"></a>Requisitos de tiempo


Los procedimientos descritos en este documento tardan entre 90 y 120 minutos en completarse.

En estas estimaciones de tiempo se da por hecho que el entorno de prueba ya está configurado; por lo tanto, no incluye el tiempo necesario para configurar el entorno de prueba.

### <a name="getting-support"></a>Ayuda


Si tiene alguna pregunta sobre el contenido de este documento o quiere hacer comentarios generales sobre algo que quiera abordar, no dude en publicar un mensaje en el [foro sobre Forefront Identity Manager 2010](http://go.microsoft.com/FWLink/p/?LinkId=189654).

## <a name="scenario-description"></a>Descripción del escenario


Fabrikam, una empresa ficticia, está pensando en usar MIM para administrar las cuentas de usuario de AD DS de la empresa. Como parte de este proceso, Fabrikam debe aprovisionar usuarios en AD DS. Para comenzar con las primeras pruebas, Fabrikam ha instalado un entorno de laboratorio básico compuesto por MIM y AD DS.
En este entorno de laboratorio, Fabrikam está probando un escenario que consta de un usuario que se ha creado manualmente en el portal de MIM. El objetivo de este escenario consiste en aprovisionar el usuario en AD DS como un usuario habilitado con una contraseña predefinida.

## <a name="scenario-design"></a>Diseño del escenario


Para usar esta guía, se necesitan tres componentes de la arquitectura:

-   Controlador de dominio de Active Directory

-   Un equipo donde se ejecute el Servicio de sincronización FIM
-   Un equipo donde se ejecute el Portal de FIM

En la siguiente ilustración se plasma el entorno necesario.

![Entorno necesario](media/how-provision-users-adds/image001.png)


Todos los componentes se pueden ejecutar en un solo equipo.

>[!NOTE]
Para más información sobre cómo configurar MIM, vea la [guía de instalación de FIM](http://go.microsoft.com/FWLink/p/?LinkId=165845).

## <a name="scenario-components-list"></a>Lista de componentes del escenario


En la siguiente tabla se enumeran los componentes que conforman el escenario descrito en esta guía.

| ![Unidad organizativa](media/how-provision-users-adds/image005.jpg)   | Unidad organizativa                | Objetos de MIM: unidad organizativa (OU) que se usa como destino de los usuarios aprovisionados.                                                       |
|----------------------------------------|------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| ![Cuentas de usuario](media/how-provision-users-adds/image006.jpg)   | Cuentas de usuario                      | &#183; **ADMA:** cuenta de usuario de Active Directory con derechos suficientes para conectarse a AD DS.<br/> &#183; **FIMMA:** cuenta de usuario de Active Directory con derechos suficientes para conectarse a MIM.
                                                                 |
| ![Agentes de administración y perfiles de ejecución](media/how-provision-users-adds/image007.jpg)  | Agentes de administración y perfiles de ejecución | &#183; **ADMA de Fabrikam:** agente de administración que intercambia datos con AD DS. <br/> &#183; FIMMA de Fabrikam: agente de administración que intercambia datos con MIM.                                                                                 |
| ![Reglas de sincronización](media/how-provision-users-adds/image008.jpg)  | Reglas de sincronización              | Regla de sincronización saliente de grupo de Fabrikam: regla de sincronización saliente que aprovisiona usuarios en AD DS.                                     |
| ![Establece](media/how-provision-users-adds/image009.jpg)   | Establece                               | Todos los contratistas: conjunto con pertenencia dinámica en todos los objetos cuyo valor del atributo EmployeeType sea Contractor.                                |
| ![Flujos de trabajo](media/how-provision-users-adds/image010.jpg)  | Flujos de trabajo                          | Flujo de trabajo de aprovisionamiento de AD: flujo de trabajo para incluir el usuario de MIM en el ámbito de la regla de sincronización saliente de AD.                                |
| ![Reglas de directivas de administración](media/how-provision-users-adds/image011.jpg)   | Reglas de directivas de administración            | Regla de directiva de administración de aprovisionamiento de AD: regla de directiva de administración (MPR) que se activa cuando un recurso pasa a ser miembro del conjunto Todos los contratistas. |
| ![Usuarios de MIM](media/how-provision-users-adds/image012.jpg) | Usuarios de MIM                          | Britta Simon: usuaria de MIM que se va a aprovisionar en AD DS.                                                                                             |



## <a name="scenario-steps"></a>Pasos del escenario


El escenario descrito en esta guía consta de los bloques de creación que se muestran en la siguiente ilustración.

![Pasos del escenario](media/how-provision-users-adds/image013.png)


## <a name="configuring-the-external-systems"></a>Configuración de los sistemas externos


En esta sección encontrará instrucciones relativas a los recursos que necesita crear y que están fuera de su entorno de MIM.

### <a name="step-1-create-the-ou"></a>Paso 1: Crear la unidad organizativa


Necesita una unidad organizativa como contenedor del usuario de ejemplo aprovisionado. Para más información sobre cómo crear unidades organizativas, vea [Creación de unidades organizativas](http://go.microsoft.com/FWLink/p/?LinkId=189655).

Cree una unidad organizativa denominada MIMObjects en AD DS.

### <a name="step-2-create-the-active-directory-user-accounts"></a>Paso 2: Crear las cuentas de usuario de Active Directory

Para el escenario de esta guía se necesitan dos cuentas de usuario de Active Directory:

- **ADMA:** usada por el agente de administración de Active Directory.

- **FIMMA:** usada por el agente de administración del Servicio FIM.

En ambos casos, basta con crear cuentas de usuario normales. Más adelante en este documento encontrará más información sobre los requisitos específicos de ambas cuentas. Para más información sobre cómo crear usuarios, vea [Creación de cuentas de usuario](http://go.microsoft.com/FWLink/p/?LinkId=189656).


## <a name="configuring-the-fim-synchronization-service"></a>Configuración del Servicio de sincronización FIM


Para poder realizar los pasos de configuración descritos en esta sección, debe iniciar Synchronization Service Manager de FIM.

### <a name="creating-the-management-agents"></a>Creación de los agentes de administración

Para el escenario de esta guía, tiene que crear dos agentes de administración:

-   **ADMA de Fabrikam:** agente de administración de AD DS.

-   **FIMMA de Fabrikam:** agente de administración del Servicio FIM.

### <a name="step-3-create-the-fabrikam-adma-management-agent"></a>Paso 3: Crear el agente de administración ADMA de Fabrikam

Cuando configure un agente de administración de AD DS, debe indicar una cuenta que el agente de administración use para intercambiar datos con AD DS. Conviene usar una cuenta de usuario normal. Pero si quiere importar datos desde AD DS, la cuenta debe tener el derecho para sondear los cambios en el control DirSync. Si quiere que el agente de administración exporte datos a AD DS, debe conceder a la cuenta los derechos suficientes correspondientes en la unidad organizativa de destino. Para más información sobre este tema, vea [Configuring the ADMA Account](http://go.microsoft.com/FWLink/p/?LinkId=189657) (Configurar la cuenta de ADMA).

Para crear un usuario en AD DS, es necesario que se conozca el DN del objeto. Aparte de esto, conviene también que se conozca el nombre, el apellido y el nombre para mostrar, ya que así se garantiza que los objetos van a poder detectarse.

En AD DS, sigue siendo normal que los usuarios usen el atributo sAMAccountName para iniciar sesión en el servicio de directorio. Si no especifica un valor en este atributo, el servicio de directorio generará un valor aleatorio. El problema es que estos valores aleatorios no son descriptivos, motivo por el que es habitual que una exportación en AD DS contenga una versión descriptiva de este atributo. Para que un usuario pueda iniciar sesión en AD DS, también es preciso incluir en la lógica de exportación una contraseña creada con el atributo unicodePwd.

>[!Note]                                
Asegúrese de que el valor especificado en unicodePwd cumple las directivas de contraseña del AD DS de destino.

Al establecer la contraseña de una cuenta de AD DS, debe crear también una cuenta como cuenta habilitada. Para ello, establezca el atributo userAccountControl. Para más información sobre el atributo userAccountControl, vea [Using FIM to Enable or Disable Accounts in Active Directory](http://go.microsoft.com/FWLink/p/?LinkId=189658) (Uso de FIM para habilitar o deshabilitar cuentas en Active Directory).

En la siguiente tabla se enumeran las opciones específicas de escenario más importantes que debe configurar.

| Página Diseñador del agente de administración                          | Configuración                                                  |
|---------------------------------------------------------|----------------------------------------------------------------|
| Crear agente de administración                                 | 1. **Agente de administración de:** AD DS  <br/> 2.  **Nombre:** ADMA de Fabrikam |
| Conectarse al bosque de Active Directory                      | 1. **Seleccionar particiones de directorio:** “DC=Fabrikam,DC=com”   <br/>   2. Haga clic en **Contenedores** para abrir el cuadro de diálogo **Seleccionar contenedores** y asegúrese de que **MIMObjects** es la única unidad organizativa que está seleccionada.        |
| Seleccionar tipos de objeto                                     | Aparte de los tipos de objeto ya seleccionados, seleccione **user**. |
| Seleccionar atributos                                       | 1. Haga clic en **Mostrar todo**. <br/>   2. Seleccione los siguientes atributos: <br/> &nbsp;&nbsp;&nbsp;&#176; **displayName** <br/> &nbsp;&nbsp;&nbsp;&#176; **givenName** <br/> &nbsp;&nbsp;&nbsp;&#176; **sn** <br/> &nbsp;&nbsp;&nbsp;&#176 **SamAccountName** <br/> &nbsp;&nbsp;&nbsp;&#176; **unicodePwd** <br/> &nbsp;&nbsp;&nbsp;&#176; **userAccountControl**     

Para más información, vea los siguientes temas de la Ayuda:
- Create a Management Agent (Crear un agente de administración)
- Connect to an Active Directory Forest (Conectarse a un bosque de Active Directory)
- Using the Management Agent for Active Directory (Uso del agente de administración de Active Directory)
- Configurar particiones de directorio

>[!Note]
Asegúrese de que tiene configurada una regla de flujo de importación de atributos relativa al atributo ExpectedRulesList.

### <a name="step-4-create-the-fabrikam-fimma-management-agent"></a>Paso 4: Crear el agente de administración FIMMA de Fabrikam

Cuando configure un agente de administración del Servicio FIM, debe indicar una cuenta que el agente de administración use para intercambiar datos con el Servicio FIM.

Conviene usar una cuenta de usuario normal. La cuenta debe ser la misma que la que haya especificado durante la instalación de MIM. Si quiere obtener un script con el que averiguar el nombre de la cuenta de FIMMA que especificó durante la instalación y comprobar si esta cuenta sigue siendo válida, vea el tema [Using Windows PowerShell to Do a FIM MA Account Configuration Quick Test](http://go.microsoft.com/FWLink/p/?LinkId=189659) (Usar Windows PowerShell para realizar una prueba rápida de configuración de la cuenta FIMMA).

En la tabla siguiente se enumeran las opciones específicas de escenario más importantes que debe configurar. Cree al agente de administración según la información proporcionada en la siguiente tabla.  

| Página Diseñador del agente de administración | Configuración |
|------------|------------------------------------|
| Crear agente de administración | 1. **Agente de administración para:** agente de administración del Servicio FIM <br/> 2. **Nombre:** FIMMA de Fabrikam |
| Conectar a base de datos     | Utilice la siguiente configuración: <br/> &#183; **Servidor:** localhost <br/> &#183; **Base de datos:** FIMService <br/> &#183; **Dirección base del Servicio FIM:** http://localhost:5725 <br/> <br/> Proporcione información sobre la cuenta que ha creado en relación con este agente de administración. |
| Seleccionar tipos de objeto                                     | Aparte de los tipos de objeto ya seleccionados, seleccione **Person**.   |
| Configurar asignaciones de tipos de objeto                          | Además de las asignaciones de tipos de objeto ya existentes, agregue una asignación del **Tipo de objeto de origen de datos: Person** al Tipo de objeto del **metaverso** Person. |
| Configurar el flujo de atributos                                | Además de las asignaciones de flujo de atributos ya existentes, agregue estas otras: <br/><br/> ![Flujo de atributo](media/how-provision-users-adds/image018.jpg) |




Para más información, vea los siguientes temas de la Ayuda:
-   Create a Management Agent (Crear un agente de administración)

-   Connect to an Active Directory Database (Conectarse a una base de datos de Active Directory)

-   Using the Management Agent for Active Directory (Uso del agente de administración de Active Directory)

-   Configurar particiones de directorio

>[!NOTE]
 Asegúrese de que tiene configurada una regla de flujo de importación de atributos relativa al atributo ExpectedRulesList.

### <a name="step-5-create-the-run-profiles"></a>Paso 5: Crear los perfiles de ejecución

En la siguiente tabla se enumeran los perfiles de ejecución que debe crear para el escenario descrito en esta guía.

| Agente de administración  | Perfil de ejecución     |
|-------------------|--------------------------------------|
| ADMA de Fabrikam     | 1. Importación completa  <br/> 2. Sincronización completa <br/> 3. Importación diferencial <br/> 4. Sincronización diferencial <br/> 5. Exportar                                                                    |
| FIMMA de Fabrikam   | 1. Importación completa <br> 2. Sincronización completa <br/> 3. Importación diferencial <br/> 4. Sincronización diferencial <br/> 5. Exportar|                                                                                                                                                                                   

Cree perfiles de ejecución para cada agente de administración según la tabla anterior.


>[!Note]
Para más información, vea el tema sobre cómo crear un perfil de ejecución de agente de administración en la Ayuda de MIM.                                                                                                                  


>[!Important]
 Confirme que el aprovisionamiento está habilitado en el entorno. Para ello, puede ejecutar el script del tema sobre cómo usar Windows PowerShell para habilitar el aprovisionamiento (http://go.microsoft.com/FWLink/p/?LinkId=189660).


## <a name="configuring-the-fim-service"></a>Configuración del Servicio MIM


Para el escenario de esta guía, debe configurar una directiva de aprovisionamiento, tal como se muestra en la siguiente ilustración.

![Directiva de aprovisionamiento](media/how-provision-users-adds/image019.png)

El objetivo de esta directiva de aprovisionamiento es incluir grupos en el ámbito de la regla de sincronización saliente de usuarios de AD. Al incluir su recurso en el ámbito de la regla de sincronización, lo que se consigue es permitir que el motor de sincronización aprovisione dicho recurso en AD DS según su configuración.

Para configurar el Servicio FIM, vaya a http://localhost/identitymanagement en Windows Internet Explorer®. Para crear la directiva de aprovisionamiento, en la página del portal de MIM, vaya a las páginas relacionadas de la sección de administración. Para comprobar la configuración, debe ejecutar el script del tema [Using Windows PowerShell to document your provisioning policy configuration](http://go.microsoft.com/FWLink/p/?LinkId=189661) (Usar Windows PowerShell para documentar la configuración de la directiva de aprovisionamiento).

### <a name="step-6-create-the-synchronization-rule"></a>Paso 6: Crear la regla de sincronización

En las siguientes tablas se muestra la configuración de la regla de sincronización de aprovisionamiento de Fabrikam necesaria. Cree la regla de sincronización de acuerdo con los datos de las siguientes tablas.

| Configuración de la regla de sincronización                                                                         |                                                                             |                                                           
|------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------|
| Nombre                                                                                                       | Regla de sincronización saliente de usuario de Active Directory                         |                                                          
| Descripción                                                                                               |                                                                             |                                                           
| Precedencia                                                                                                | 2                                                                           |                                                           
| Dirección del flujo de datos   | Saliente             |       
| Dependencia       |         |                                         


| Ámbito |                                                                             |                                                           
|--------|-------|
| Tipo de recurso de metaverso | persona |                                                         
| Sistema externo                   |ADMA de Fabrikam                                                               |                                                       
| Tipo de recurso de sistema externo                                                                              | usuario      



| Relationship ||
|------------|---------|
| Crear recurso en el sistema externo                                                                         | True                                                                        |                                                           
| Habilitar anulación de aprovisionamiento                                                                                      | False                                                                       |                                                           

| Criterios de relación                                                                                      | |
|------------|----------|
| Atributo ILM     | Atributo de origen de datos                                                       |
| Atributo de origen de datos         | sAMAccountName    |

| Flujos de atributo salientes iniciales        | |                                                             |
|-------------------|---------------------- |---------------|
| Permitir valores null                 | Destination                                                                 | Origen                                                    |
| false                       | dn                                                                          | \+("CN=",displayName,",OU=MIMObjects,DC=fabrikam,DC=com") |
| false                       | userAccountControl                                                          | **Constante:** 512                                         |
| false                                                                     | unicodePwd                    | Constante: P\@\$\$W0rd                                    |

| Flujos de atributo salientes persistentes  |                                                                     |                                                           |
|--------------------------------------|---------------------------------------------------------------------|-----------------------------------------------------------|
| Permitir valores null                                                                                                | Destination                                                                 | Origen                                                    |
| false                                                                                                      | sAMAccountName                                                              | accountName                                               |
| false                                                                                                      | displayName                                                                 | displayName                                               |
| false                                                                                                      | givenName                                                                   | firstName                                                 |
| false                                                                                                      | sn                                                                          | lastName                                                  |



 >[!NOTE]
 Importante: Confirme que la opción Solo flujo inicial está seleccionada en el flujo de atributo que tiene el DN como destino.                                                                          

### <a name="step-7-create-the-workflow"></a>Paso 7: Crear el flujo de trabajo

El objetivo del flujo de trabajo de aprovisionamiento de AD es agregar la regla de sincronización de aprovisionamiento de Fabrikam a un recurso. En las siguientes tablas se muestra la configuración.  Cree un flujo de trabajo según los datos recogidos en las siguientes tablas.

| Configuración del flujo de trabajo               |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Nombre                                 | Flujo de trabajo de aprovisionamiento de usuario de Active Directory                     |
| Descripción                          |                                                                 |
| Tipo de flujo de trabajo                        | Acción                                                          |
| Ejecutar al actualizar la directiva                 | False                                                           |

| Regla de sincronización                 |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Nombre                                 | Regla de sincronización saliente de usuario de Active Directory             |
| Acción                               | Agregar                                                             |




### <a name="step-8-create-the-mpr"></a>Paso 8: Crear la regla de directiva de administración

La regla de directiva de administración necesaria es de tipo Establecer transición y se activa cuando un recurso pasa a ser miembro del conjunto Todos los contratistas. En las siguientes tablas se muestra la configuración.  Cree una regla de directiva de administración según los datos recogidos en las siguientes tablas.

| Configuración de MPR                    |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Nombre                                 | Regla de directiva de administración de aprovisionamiento de usuario de AD                 |
| Descripción                          |                                                             |
| Tipo                                 | Establecer transición                                              |
| Concede permisos                   | False                                                       |
| Deshabilitado                             | False                                                       |

| Definición de transición                |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Tipo de transición                      | Realizar transición a                                               |
| Conjunto de transiciones                       | Todos los contratistas                                             |

| Flujos de trabajo de la directiva                     |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Tipo                                 | Acción                                                      |
| Nombre para mostrar                         | Flujo de trabajo de aprovisionamiento de usuario de Active Directory                 |




## <a name="initializing-your-environment"></a>Inicialización del entorno


Los objetivos de la fase de inicialización son los siguientes:

-   Incluir la regla de sincronización en el metaverso.

-   Incluir la estructura de Active Directory en el espacio conector de Active Directory.

### <a name="step-9-run-the-run-profiles"></a>Paso 9: Ejecutar los perfiles de ejecución

En la siguiente tabla se enumeran los perfiles de ejecución que forman parte de la fase de inicialización.  Ejecute los perfiles de ejecución según la siguiente tabla.

| Ejecutar                                                                                                           | Agente de administración                                      | Perfil de ejecución          |
|---------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|----------------------|
| 1                                                                                                             | FIMMA de Fabrikam                                        | Importación completa          |
| 2                                                                                                             |                                                       | Sincronización completa |
| 3                                                                                                             |                                                       | Exportar               |
| 4                                                                                                             |                                                       | Importación diferencial         |
|                                                                                                               |                                                       |                      |
| 5                                                                                                             | ADMA de Fabrikam                                         | Importación completa          |
| 6                                                                                                             |                                                       | Sincronización completa |



>[!NOTE]
Debe comprobar que la regla de sincronización saliente está correctamente proyectada hacia el metaverso.

## <a name="testing-the-configuration"></a>Comprobación de la configuración


El objetivo de esta sección es probar la configuración real. Para probar la configuración, debe hacer lo siguiente:

1.  Cree un usuario de ejemplo en el Portal de FIM.

2.  Compruebe los requisitos de aprovisionamiento del usuario de ejemplo.

3.  Aprovisione el usuario de ejemplo en AD DS.

4.  Confirme que el usuario existe en AD DS.

### <a name="step-10-create-a-sample-user-in-mim"></a>Paso 10: Crear un usuario de ejemplo en MIM


En la siguiente tabla se enumeran las propiedades del usuario de ejemplo. Cree un usuario de ejemplo según los datos recogidos en la siguiente tabla.

| Atributo                              | Valor                                                          |
|----------------------------------------|----------------------------------------------------------------|
| Nombre                             | Britta                                                         |
| Apellido                              | Simon                                                          |
| Nombre para mostrar                           | Britta Simon                                                   |
| Nombre de cuenta                           | BSimon                                                         |
| Dominio                                 | Fabrikam                                                       |
| Tipo de empleado                          | Contratista                                                     |



### <a name="verify-the-provisioning-requisites-of-the-sample-user"></a>Comprobar los requisitos de aprovisionamiento del usuario de ejemplo


Para aprovisionar el usuario de ejemplo en AD DS, deben cumplirse dos requisitos previos:

1.  El usuario debe ser miembro del conjunto Todos los contratistas.

2.  El usuario debe estar incluido en el ámbito de la regla de sincronización saliente.

### <a name="step-11-verify-the-user-is-a-member-of-all-contractors"></a>Paso 11: Comprobar que el usuario es miembro del conjunto Todos los contratistas

Para comprobar si el usuario es miembro del conjunto Todos los contratistas, abra el conjunto y haga clic en Ver miembros.

![Comprobar que el usuario es miembro del conjunto Todos los contratistas](media/how-provision-users-adds/image022.jpg)


### <a name="step-12-verify-the-user-is-in-the-scope-of-the-outbound-synchronization-rule"></a>Paso 12: Comprobar que el usuario está incluido en el ámbito de la regla de sincronización saliente

Para comprobar si el usuario está en el ámbito de la regla de sincronización, abra la página de propiedades del usuario y revise el atributo Lista de reglas esperadas en la pestaña Aprovisionamiento. El atributo Lista de reglas esperadas debe mostrar el valor AD User

Regla de sincronización saliente. En la siguiente captura de pantalla se muestra un ejemplo del atributo Lista de reglas esperadas.

![Estado de regla de sincronización](media/how-provision-users-adds/image023.jpg)

En este punto del proceso, el estado de la regla de sincronización es Pendiente; es decir, la regla de sincronización aún no se ha aplicado al usuario.



### <a name="step-13-synchronize-the-sample-group"></a>Paso 13: Sincronizar el grupo de ejemplo


Antes de iniciar el primer ciclo de sincronización en un objeto de prueba, debe controlar el estado que se espera del objeto después de cada perfil de ejecución que se ejecute en un plan de pruebas. El plan de pruebas debe contemplar los valores de atributo que se esperan, además del estado general del objeto (Creado, Actualizado o Eliminado).
Use el plan de pruebas para confirmar que cumple sus expectativas. Si un paso no devuelve los resultados esperados, no avance al siguiente paso hasta haber resuelto la discrepancia entre el resultado previsto y el resultado real obtenido.

Para comprobar que las expectativas se cumplen, puede usar las estadísticas de sincronización como primer indicador. Por ejemplo, si espera que haya nuevos objetos almacenados provisionalmente en un espacio conector, pero las estadísticas de importación no devuelven ninguna adición a este respecto (“Adds” en la imagen), es evidente que hay algo en el entorno que no funciona del modo previsto.

![Estadísticas de sincronización](media/how-provision-users-adds/image024.jpg)

Si bien las estadísticas de sincronización pueden arrojar un primer indicio de que el escenario funciona según lo previsto, conviene usar las características de búsqueda en el espacio conector y de búsqueda en el metaverso de Synchronization Service Manager para confirmar que los valores de atributo son los esperados.

Para sincronizar el usuario en AD DS:

1.  Importe el usuario al espacio conector de FIMMA.

2.  Proyecte al usuario al metaverso.

3.  Aprovisione el usuario en el espacio conector de Active Directory.

4.  Exporte la información de estado a FIM.

5.  Exporte el usuario a AD DS.

6.  Confirme que el usuario se ha creado.

Para realizar estas tareas, ejecute los siguientes perfiles de ejecución.

| Agente de administración | Perfil de ejecución  |
|------------------|--------------|
| FIMMA de Fabrikam   | 1. Importación diferencial <br/> 2. Sincronización delta <br/> 3. Exportar <br/> 4. Importación diferencial |
| FIMMA de Fabrikam   | 1. Exportar <br/> 2. Importación diferencial       |


Después de la importación desde la base de datos del servicio FIM, Britta Simon y el objeto ExpectedRuleEntry que vincula a Britta con la regla de sincronización saliente de usuario de AD se almacenan provisionalmente en el espacio conector de FIMMA de Fabrikam. Si revisa las propiedades de Britta en el espacio conector junto a los valores de atributo configurados en el Portal de FIM, también encontrará una referencia válida al objeto ExpectedRuleEntry. En la siguiente captura de pantalla se muestra un ejemplo de esto.

![Propiedades del objeto de espacio conector](media/how-provision-users-adds/image025.jpg)

El objetivo de ejecutar la sincronización diferencial en su FIMMA de Fabrikam reside en realizar varias operaciones:

-   Proyección: el nuevo objeto de usuario y el objeto ExpectedRuleEntry relacionado se proyectan hacia el metaverso.

-   Aprovisionamiento: el objeto recién proyectado Britta Simon se aprovisiona en el espacio conector de ADMA de Fabrikam.

-   Flujos de exportación de atributos: se producen flujos de exportación de atributos en ambos agentes de administración. En ADMA de Fabrikam, el objeto Britta Simon recién aprovisionado se rellena con los nuevos valores de atributo. En FIMMA de Fabrikam, el objeto Britta Simon existente y el objeto ExpectedRuleEntry relacionado se actualizan con los valores de atributo resultado de la proyección.

![Estadísticas de sincronización](media/how-provision-users-adds/image026.jpg)

Como ya han reflejado las estadísticas de sincronización, ha tenido lugar una actividad de aprovisionamiento en el espacio conector de ADMA de Fabrikam. Cuando revise las propiedades del objeto de metaverso de Britta Simon, encontrará que esta actividad es el resultado del atributo ExpectedRulesList que se ha rellenado con una referencia válida.

![propiedades de objeto de metaverso](media/how-provision-users-adds/image027.jpg)

Durante la siguiente exportación en FIMMA de Fabrikam, el estado de la regla de sincronización de Britta Simon se actualiza de Pendiente a Aplicado, lo que indica que la regla de sincronización saliente ahora está activa en el objeto en el metaverso.

![Regla de sincronización aplicada](media/how-provision-users-adds/image028.jpg)

Como se ha aprovisionado un nuevo objeto en el espacio conector de ADMA, debería haber una exportación de adición pendiente en este agente de administración. Si usamos un script creado expresamente para este propósito, se puede ver que hay una exportación de adición pendiente (Add) relativa a ADMA de Fabrikam. Para usar el script, vea [Using Windows PowerShell to Display the Export State of a Management Agent](http://go.microsoft.com/FWLink/p/?LinkId=189664) (Uso de Windows PowerShell para ver el estado de exportación de un agente de administración).

![Exportaciones pendientes del agente de administración](media/how-provision-users-adds/image029.jpg)

En FIM, para que una operación de exportación se complete, tras cada ejecución de exportación es necesario realizar una importación diferencial. La importación diferencial que se ejecuta después de una ejecución de exportación se conoce como confirmación de la importación. Las confirmaciones de importación son necesarias para que el Servicio de sincronización FIM pueda crear los requisitos de actualización apropiados durante las ejecuciones de sincronización posteriores.


Ejecute los perfiles de ejecución según las instrucciones de esta sección.

>[!IMPORTANT]
Cada perfil de ejecución debe realizarse sin errores.

### <a name="step-14-verify-the-provisioned-user-in-ad-ds"></a>Paso 14: Comprobar el usuario aprovisionado en AD DS

Para comprobar que el usuario de ejemplo se ha aprovisionado en AD DS, abra la unidad organizativa FIMObjects. Britta Simon debería aparecer en la unidad organizativa FIMObjects.

![comprobar que el usuario está en la unidad organizativa FIMObjects](media/how-provision-users-adds/image033.jpg)

<a name="summary"></a>Resumen
=======

El objetivo de este documento consiste en darle a conocer los principales bloques de creación necesarios para sincronizar un usuario de MIM con AD DS. En las pruebas iniciales, empezamos primero con los atributos mínimos que son necesarios para completar una tarea y agregamos más atributos al escenario cuando vimos que los pasos generales se realizaban según lo esperado. Mantener la complejidad a un nivel mínimo simplifica el proceso de solución de problemas.

Al probar la configuración, es muy probable que se eliminen objetos de prueba y se vuelvan a crear otros. Esto, en el caso de los objetos con un

atributo ExpectedRulesList relleno, puede hacer que se generen objetos ExpectedRuleEntry huérfanos.
Para saber cómo quitar estos objetos del entorno de prueba, vea [A Method to Remove Orphaned ExpectedRuleEntry Objects from Your Environment](http://go.microsoft.com/FWLink/p/?LinkId=189667) (Método para quitar objetos ExpectedRuleEntry huérfanos del entorno).

En un escenario típico de sincronización que incluya AD DS como destino de la sincronización, MIM no es autoritativo en todos los atributos de un objeto. Así, por ejemplo, cuando se usa FIM para administrar los objetos de usuario en AD DS, al menos el dominio y los atributos objectSID deben provenir del agente de administración de AD DS.
Los atributos de nombre de cuenta, de dominio y objectSID son necesarios si quiere que los usuarios puedan iniciar sesión en el Portal de FIM. Para rellenar estos atributos de AD DS, es preciso que haya una regla de sincronización entrante adicional para el espacio conector de AD DS. Cuando se administran objetos cuyos valores de atributo proceden de varios orígenes, hay que configurar correctamente la precedencia del flujo de atributos. Si la precedencia del flujo de atributos no está configurada correctamente, el motor de sincronización impide que se rellenen los valores de atributo. Encontrará más información sobre la precedencia del flujo de atributos en el artículo [About Attribute Flow Precedence](http://go.microsoft.com/FWLink/p/?LinkId=189675) (Acerca de la precedencia del flujo de atributos).

<a name="see-also"></a>Véase también
=========

<a name="other-resources"></a>Otros recursos
---------------

[Using FIM to Enable or Disable Accounts in Active Directory](http://go.microsoft.com/FWLink/p/?LinkId=189670) (Uso de FIM para habilitar o deshabilitar cuentas en Active Directory)

[About Reference Attributes](http://go.microsoft.com/FWLink/p/?LinkId=189671) (Acerca de los atributos de referencia)

[How Can I Manage My FIM MA Account](http://go.microsoft.com/FWLink/p/?LinkId=189672) (¿Cómo puedo administrar mi cuenta de FIM MA?)

[Detecting Nonauthoritative Accounts – Part 1: Envisioning](http://go.microsoft.com/FWLink/p/?LinkId=189673) (Detectar cuentas no autoritativas. Parte 1: concepción)

[The Poor Man’s Version of a Connector Detection Mechanism](http://go.microsoft.com/FWLink/p/?LinkId=189674) (Versión inferior de un mecanismo de detección de conectores)

[Configuring the ADMA Account](http://go.microsoft.com/FWLink/p/?LinkId=189657) (Configurar la cuenta de ADMA)

[A Method to Remove Orphaned ExpectedRuleEntry Objects from Your Environment](http://go.microsoft.com/FWLink/p/?LinkId=189667) (Método para quitar objetos ExpectedRuleEntry huérfanos del entorno)

[About Attribute Flow Precedence](http://go.microsoft.com/FWLink/p/?LinkId=189675) (Acerca de la precedencia del flujo de atributos)

[About Exports](http://go.microsoft.com/FWLink/p/?LinkId=189676) (Acerca de las exportaciones)
