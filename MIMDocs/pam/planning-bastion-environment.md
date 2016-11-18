---
title: "Planificación de un entorno bastión | Microsoft Docs"
description: 
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 09/16/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: bfc7cb64-60c7-4e35-b36a-bbe73b99444b
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: d07528fd69328647ff63e4a0f0f914af7cabfb8f


---

# <a name="planning-a-bastion-environment"></a>Planificación de un entorno bastión

Agregar un entorno bastión con un bosque administrativo dedicado a una instancia de Active Directory permite que las organizaciones administren fácilmente las cuentas administrativas, estaciones de trabajo y grupos de un entorno que tiene controles de seguridad más eficaces que los del entorno de producción existente.

Esta arquitectura permite contar con diversos controles que no serían posibles o que no podrían configurarse fácilmente en una arquitectura de un solo bosque. Esto incluye el aprovisionamiento de cuentas como usuarios estándar sin privilegios en el bosque administrativo y que cuentan con altos privilegios en el entorno de producción, lo que permite un mayor cumplimiento técnico de gobierno. Esta arquitectura también permite usar la característica de autenticación selectiva de una confianza como un medio de restringir los inicios de sesión (y la exposición de credenciales) solo a los hosts autorizados. En situaciones en las que se desea un mayor nivel de control para el bosque de producción sin el costo y la complejidad que implica una recompilación total, un bosque administrativo puede proporcionar un entorno que aumente el nivel de control del entorno de producción.

Se pueden utilizar técnicas adicionales al bosque administrativo dedicado. Estas técnicas incluyen restringir la exposición de las credenciales administrativas, limitar los privilegios de rol de los usuarios en ese bosque y garantizar que las tareas administrativas no se realicen en los hosts que se utilizan para las actividades de usuario estándar (por ejemplo, correo electrónico y exploración web).

## <a name="best-practice-considerations"></a>Consideraciones sobre los procedimientos recomendados

Un bosque administrativo dedicado es un bosque de Active Directory de dominio único estándar que se usa para la administración de Active Directory. Una ventaja de usar dominios y bosques administrativos es que pueden tener más medidas de seguridad que los bosques de producción debido a sus casos de uso limitados. Además, como este bosque es independiente y no confía en los bosques existentes de la organización, si se compromete la seguridad en otro bosque no se extenderá a este bosque dedicado.

El diseño de un bosque administrativo tiene las siguientes consideraciones:

### <a name="limited-scope"></a>Ámbito limitado

El valor de un bosque administrativo es el alto nivel de garantía de seguridad y una menor superficie expuesta a ataques. El bosque puede hospedar aplicaciones y funciones de administración adicionales, pero cada aumento en el ámbito aumentará la superficie del bosque y sus recursos expuesta a ataques. El objetivo es limitar las funciones del bosque para mantener una superficie expuesta a ataques mínima.

Según el [modelo de niveles](tier-model-for-partitioning-administrative-privileges.md) de realizar particiones en los privilegios administrativos, las cuentas de un bosque administrativo dedicado deberían estar en un único nivel, normalmente en el nivel 0 o en el 1. Si un bosque está en el nivel 1, considere la posibilidad de restringirlo a un ámbito de aplicación determinado (por ejemplo, aplicaciones de finanzas) o a una comunidad de usuarios (por ejemplo, proveedores de TI subcontratados).

### <a name="restricted-trust"></a>Confianza restringida

El bosque *CORP* de producción debería confiar en el bosque *PRIV* administrativo, pero no al revés. Puede tratarse de una confianza de dominio o una confianza de bosque. No es necesario que el dominio del bosque administrativo confíe en los bosques y dominios administrados para poder administrar Active Directory; sin embargo, algunas aplicaciones adicionales podrían requerir una relación de confianza bidireccional, validación de la seguridad y prueba.

Se debe usar la autenticación selectiva para garantizar que las cuentas en el bosque administrativo solo usan los hosts de producción adecuados. Para mantener los controladores de dominio y delegar derechos en Active Directory, normalmente se requiere otorgar el derecho "Se permite el inicio de sesión" de los controladores de dominio a las cuentas de administración designadas de nivel 0 en el bosque administrativo. Consulte [Configuring Selective Authentication Settings (Configuración de la autenticación selectiva)](http://technet.microsoft.com/library/cc816580.aspx) para obtener más información.

## <a name="maintain-logical-separation"></a>Mantener una separación lógica

Con la finalidad de asegurarse de que el entorno bastión no se vea afectado por incidentes de seguridad existentes o futuros en la instancia de Active Directory de la organización, se deben usar las siguientes guías cuando se preparen los sistemas para el entorno bastión:

- Los servidores de Windows Server no deben estar unidos a un dominio ni tampoco deben utilizar la distribución de configuración o software del entorno existente.

- El entorno bastión debe contener sus propios Servicios de dominio de Active Directory, lo que brinda Kerberos y LDAP, DNS, tiempo y servicios de tiempo al entorno bastión.

- MIM no debería usar una granja de servidores de Base de datos SQL en el entorno existente. SQL Server debería implementarse en servidores dedicados en el entorno bastión.

- El entorno bastión necesita Microsoft Identity Manager 2016, especialmente, deben estar implementados los componentes de PAM y del servicio MIM.

- El software y los medios de copia de seguridad del entorno bastión deben estar separados de los sistemas de los bosques existentes, de manera tal que un administrador del bosque existente no pueda trastocar una copia de seguridad del entorno bastión.

- Los usuarios que administrar los servidores del entorno bastión deben iniciar sesión desde estaciones de trabajo a la que los administradores del entorno existente no puedan tener acceso, de manera tal que no se filtren las credenciales correspondientes al entorno bastión.

## <a name="ensure-availability-of-administration-services"></a>Garantizar la disponibilidad de los servicios de administración

Como la administración de las aplicaciones se transferirá al entorno bastión, tenga en cuenta cómo proporcionar la disponibilidad suficiente para cumplir los requisitos de esas aplicaciones. Las técnicas incluyen:

- Implementar Servicios de dominio de Active Directory en varios equipos del entorno bastión. Se necesitan al menos dos para garantizar que la autenticación continúe, incluso si un servidor se reinicia de forma temporal para realizar un mantenimiento programado. Es posible que se requieran equipos adicionales para una mayor carga o para administrar recursos y administradores ubicados en varias regiones geográficas.

- Preparar cuentas de emergencia en el bosque existente y en el bosque administrativo dedicado, con fines de emergencia.

- Implementar SQL Server y Servicio MIM en varios equipos del entorno bastión.

- Mantener una copia de seguridad de AD y SQL para cada cambio de los usuarios o las definiciones de rol en el bosque administrativo dedicado.

## <a name="configure-appropriate-active-directory-permissions"></a>Configurar los permisos adecuados de Active Directory

El bosque administrativo debe estar configurado en el menor nivel de privilegio según los requisitos para la administración de Active Directory.

- Las cuentas existentes en el bosque administrativo que se usan para administrar el entorno de producción no deben contar con privilegios administrativos para el bosque administrativo o los dominios o estaciones de trabajo que existen en él.

- Un proceso sin conexión controlará rigurosamente los privilegios administrativos sobre el bosque administrativo mismo, con el fin de disminuir la posibilidad de que un atacante o un infiltrado malintencionado borre los registros de auditoría. Esto también ayuda a asegurarse de que el personal con cuentas de administración de producción no flexibilice las restricciones sobre sus cuentas y aumente el riesgo para la organización.

- El bosque administrativo debe respetar las configuraciones de Microsoft Security Compliance Manager (SCM) para el dominio, incluidas configuraciones seguras para los protocolos de autenticación.

Cuando cree el entorno bastión, antes de instalar Microsoft Identity Manager, identifique y cree las cuentas que se usarán para la administración dentro de este entorno. Esto incluirá:

- Las **cuentas de emergencia** solo podrán iniciar sesión en los controladores de dominio en el entorno bastión.

- Los **administradores de "tarjeta roja"** aprovisionan otras cuentas y realizan el mantenimiento sin programar. No tienen acceso a bosques o sistemas existentes fuera del entorno bastión. Las credenciales, por ejemplo, una tarjeta inteligente, deben protegerse de manera física y se debe registrar el uso de estas cuentas.

- **Cuentas de servicio** que son necesarias para Microsoft Identity Manager, SQL Server y otro tipo de software.

## <a name="harden-the-hosts"></a>Proteger los hosts

Todos los hosts, incluidos los controladores de dominio, los servidores y las estaciones de trabajo unidas al bosque administrativo, deben tener instalados y actualizados los sistemas operativos y Service Packs más recientes.

- Las aplicaciones necesarias para realizar la administración deben estar instaladas previamente en las estaciones de trabajo, para que no sea necesario que las cuentas que las usan estén en el grupo de administradores locales para instalarlas. Normalmente, el mantenimiento de los controladores de dominio se realiza con RDP y con Herramientas de administración remota del servidor.

- Las actualizaciones de seguridad se deben aplicar automáticamente a los hosts del bosque administrativo. Aunque esto puede generar un riesgo de interrumpir las operaciones de mantenimiento de los controladores de dominio, permite mitigar, en gran medida, el riesgo de seguridad de las vulnerabilidades a las que no se aplicó una revisión.

### <a name="identify-administrative-hosts"></a>Identificar hosts administrativos

El riesgo de un sistema o una estación de trabajo se mide según la actividad de mayor riesgo que se realiza en estos; por ejemplo, explorar Internet, enviar y recibir correo electrónico o usar otras aplicaciones que procesan contenido desconocido o que no es de confianza.

Los hosts administrativos incluyen los siguientes equipos:

- Un equipo de escritorio, en el que se escriben o ingresan físicamente las credenciales del administrador.

- "Servidores de salto" administrativos, donde se ejecutan las herramientas y las sesiones administrativas.

- Todos los hosts donde se realizan acciones administrativas, incluidos los que usan un escritorio de usuario estándar que ejecuta un cliente RDP para administrar servidores y aplicaciones de forma remota.

- Servidores que hospedan aplicaciones que se deben administrar, y a los que no se puede tener acceso mediante RDP con modo de administración restringida o la comunicación remota de Windows PowerShell.

### <a name="deploy-dedicated-administrative-workstations"></a>Implementar estaciones de trabajo administrativas dedicadas

Aunque sea un inconveniente, son necesarias las estaciones de trabajo protegidas e independientes dedicadas a los usuarios con credenciales administrativas de gran impacto. Es importante proporcionar un host con un nivel de seguridad que sea igual o superior al nivel de los privilegios que se han confiado a las credenciales. Considere la incorporación de las siguientes medidas para obtener una protección adicional:

- **Comprobar que todos los medios de la compilación están limpios** para mitigar el malware que está instalado en una imagen maestra o que se ha insertado en un archivo de instalación durante la descarga o el almacenamiento.

- **Líneas base de seguridad** que se deben usar como configuraciones iniciales. Microsoft Security Compliance Manager (SCM) puede ayudar a configurar las líneas base en hosts administrativos.

- **Arranque seguro** , para mitigar el riesgo de atacantes o malware que intenten cargar código sin firma en el proceso de arranque.

- **Restricción de software** , para garantizar que solo se ejecute software administrativo autorizado en los hosts administrativos. Los clientes pueden usar AppLocker para esta tarea con una lista blanca de aplicaciones autorizadas, con la finalidad de evitar que se ejecute software malintencionado y aplicaciones no compatibles.

- **Cifrado del volumen completo** para mitigar la pérdida física de equipos, como equipos portátiles administrativos que se usan de forma remota.

- **Restricciones USB** para obtener protección contra la infección física.

- **Aislamiento de la red** , para protección contra ataques a la red y acciones administrativas involuntarias. Los firewalls de host deben bloquear todas las conexiones entrantes, excepto las que se requieren de forma explícita, y bloquear todo el acceso a Internet saliente innecesario.

- **Antimalware** , para protección contra malware y amenazas conocidas.

- **Mitigaciones de vulnerabilidades** , para mitigar las amenazas y vulnerabilidades desconocidas, incluido Enhanced Mitigation Experience Toolkit (EMET).

- **Análisis de la superficie expuesta a ataques** , para evitar el ingreso de nuevos vectores de ataque a Windows durante la instalación de software nuevo. Las herramientas como el analizador de superficie expuesta a ataques (ASA) ayudan a evaluar las opciones de configuración de un host y a identificar los vectores de ataque que se han introducido mediante cambios en la configuración o por un software.

- No se deben proporcionar **privilegios administrativos** a los usuarios en su equipo local.

- **Modo RestrictedAdmin** para las sesiones RDP salientes excepto cuando lo requiere el rol. Consulte [Novedades en Servicios de Escritorio remoto en Windows Server](https://technet.microsoft.com/library/dn283323.aspx) para más información.

Algunas de estas medidas pueden parecer extremas, pero las revelaciones públicas realizadas en los últimos años mostraron las funcionalidades importantes que poseen los adversarios experimentados para poner en peligro sus objetivos.

## <a name="prepare-existing-domains-to-be-managed-by-the-bastion-environment"></a>Preparar los dominios existentes que se van a administrar mediante el entorno bastión

MIM usa cmdlets de PowerShell para establecer la confianza entre los dominios AD existentes y el bosque administrativo dedicado en el entorno bastión. Cuando se implemente el entorno bastión, y antes de que ningún usuario o grupo se convierta en JIT, los cmdlets `New-PAMTrust` y `New-PAMDomainConfiguration` actualizarán las relaciones de confianza de dominio y crearán los artefactos necesarios para AD y MIM.

Cuando la topología de Active Directory existente cambia, los cmdlets `Test-PAMTrust`, `Test-PAMDomainConfiguration`, `Remove-PAMTrust` y `Remove-PAMDomainConfiguration` se pueden usar para actualizar las relaciones de confianza.

## <a name="establish-trust-for-each-forest"></a>Establecer la confianza para cada bosque

El cmdlet `New-PAMTrust` se debe ejecutar una vez para cada bosque existente. Se invoca en el equipo del Servicio MIM en el dominio administrativo. Los parámetros para este comando son el nombre de dominio del dominio principal del bosque existente y la credencial de un administrador de ese dominio.

```
New-PAMTrust -SourceForest "contoso.local" -Credentials (get-credential)
```

Después de establecer la confianza, configure cada dominio para habilitar la administración desde el entorno bastión, tal como se describe en la siguiente sección.

## <a name="enable-management-of-each-domain"></a>Habilitar la administración de cada dominio

Existen siete requisitos para habilitar la administración para un dominio existente.

### <a name="1-a-security-group-on-the-local-domain"></a>1. Un grupo de seguridad en el dominio local

En el dominio existente, debe haber un grupo cuyo nombre sea el nombre de dominio de NetBIOS, seguido de tres signos de dólar, por ejemplo, *CONTOSO$$$*. El ámbito del grupo debe ser *local de dominio* y el tipo de grupo debe ser *Seguridad*. Esto es necesario para los grupos que se creen en el bosque administrativo dedicado con el mismo identificador de seguridad que los grupos de este dominio. Cree este grupo con el siguiente comando de PowerShell, que un administrador del dominio existente puede realizar y ejecutar en una estación de trabajo unida al dominio existente:

```
New-ADGroup -name 'CONTOSO$$$' -GroupCategory Security -GroupScope DomainLocal -SamAccountName 'CONTOSO$$$'
```

### <a name="2-success-and-failure-auditing"></a>2. Auditoría de aciertos y errores

La configuración de la directiva de grupo en el controlador de dominio para auditoría debe incluir la auditoría de operaciones realizadas correctamente y operaciones erróneas para Auditar la administración de cuentas y Auditar el acceso del servidor de directorio. Esto se puede realizar con la Consola de administración de directivas de grupo, que un administrador del dominio existente puede realizar y ejecutar en una estación de trabajo unida al dominio existente:

3. Vaya a **Inicio** > **Herramientas administrativas** > **Administración de directivas de grupo**.

4. Vaya a **Bosque: contoso.local** > **Dominios** > **contoso.local** > **Controladores de dominio** > **Directiva predeterminada de controladores de dominio**. Aparecerá un mensaje informativo.

    ![Directiva predeterminada de controladores de dominio: captura de pantalla](media/pam-group-policy-management.jpg)

5. Haga clic con el botón derecho en **Directiva predeterminada de controladores de dominio** y seleccione **Editar**. Aparecerá una ventana nueva.

6. En la ventana Editor de administración de directivas de grupo, en el árbol Directiva predeterminada de controladores de dominio, vaya a **Configuración del equipo** > **Directivas** > **Configuración de Windows** > **Configuración de seguridad** > **Directivas locales** > **Directiva de auditoría**.

    ![Editor de administración de directivas de grupo: captura de pantalla](media/pam-group-policy-management-editor.jpg)

5. En el panel de detalles , haga clic con el botón derecho en **Auditar la administración de cuentas** y seleccione **Propiedades**. Seleccione **Definir esta configuración de directiva**, active **Correcto**, active **Error**, haga clic en **Aplicar** y en **Aceptar**.

6. En el panel de detalles, haga clic con el botón derecho en **Auditar el acceso del servicio de directorio** y seleccione **Propiedades**. Seleccione **Definir esta configuración de directiva**, active **Correcto**, active **Error**, haga clic en **Aplicar** y en **Aceptar**.

    ![Configuración de directivas de aciertos y errores: captura de pantalla](media/pam-group-policy-management-editor2.jpg)

7. Cierre la ventana del Editor de administración de directivas de grupo y la de Administración de directivas de grupo. A continuación, para aplicar la configuración de auditoría, inicie una ventana de PowerShell y escriba:

    ```
    gpupdate /force /target:computere
    ```

El mensaje “La actualización de la directiva de equipo se completó correctamente.” debe aparecer después de unos minutos.

### <a name="3-allow-connections-to-the-local-security-authority"></a>3. Permitir conexiones a la autoridad de seguridad local

Los controladores de dominio deben permitir conexiones de RPC sobre TCP/IP para la 	Autoridad de seguridad local (LSA) desde el entorno bastión. En versiones anteriores de Windows Server, la compatibilidad de TCP/IP en LSA debe estar habilitada en el registro:

```
New-ItemProperty -Path HKLM:SYSTEM\\CurrentControlSet\\Control\\Lsa -Name TcpipClientSupport -PropertyType DWORD -Value 1
```

### <a name="4-create-the-pam-domain-configuration"></a>4. Crear la configuración de dominio de PAM

El cmdlet `New-PAMDomainConfiguration` se debe ejecutar en el equipo de Servicio MIM en el dominio administrativo. Los parámetros para este comando son el nombre de dominio del dominio existente y la credencial de un administrador de ese dominio.

```
 New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials (get-credential)
```

### <a name="5-give-read-permissions-to-accounts"></a>5. Conceder permisos de lectura a las cuentas

Las cuentas del bosque bastión que se usan para establecer roles (administradores que usan el cmdlet `New-PAMUser` y `New-PAMGroup` ), así como la cuenta que usa el servicio de supervisión de MIM, necesitan permisos de lectura en ese dominio.

Los pasos siguientes habilitan el acceso de lectura para el usuario *PRIV\administrador* al dominio *Contoso* dentro del controlador de dominio *CORPDC*:

1. Asegúrese de que inició sesión en CORPDC como administrador del dominio Contoso (por ejemplo, como Contoso\Administrador).

2. Inicie Usuarios y equipos de Active Directory.

3. Haga clic con el botón derecho en el dominio **contoso.local** y seleccione **Control delegado**.

4. En la pestaña Usuarios y grupos seleccionados, haga clic en **Agregar**.

5. En la ventana emergente Seleccionar usuarios, equipos o grupos, haga clic en **Ubicaciones** y cambie la ubicación a *priv.contoso.local*. Como nombre de objeto, escriba *Administradores del dominio* y haga clic en **Comprobar nombres**. Cuando aparezca una ventana emergente, escriba *PRIV\administrador* como nombre de usuario y la contraseña.

6. Después de Administradores del dominio, escriba *; MIMMonitor*. Cuando aparezcan subrayados los nombres Admins. del dominio y MIMMonitor, haga clic en **Aceptar** y luego en **Siguiente**.

7. En la lista de tareas comunes, seleccione **Leer toda la información del usuario** y, después, haga clic en **Siguiente** y en **Finalizar**.

18. Cierre Usuarios y equipos de Active Directory.

### <a name="6-a-break-glass-account"></a>6. Una cuenta de emergencia

Si el objetivo del proyecto de Privileged Access Management es disminuir la cantidad de cuentas con privilegios de administrador de dominio asignados permanentemente al dominio, debe existir una cuenta *de emergencia* en el dominio, en caso de que, más adelante, se genere un problema con la relación de confianza. En cada dominio deben existir cuentas para el acceso de emergencia al bosque de producción y solo deberían tener acceso a los controladores de dominio. En el caso de las organizaciones con varios sitios, es posible que se requieran cuentas adicionales para redundancia.

### <a name="7-update-permissions-in-the-bastion-environment"></a>7. Actualizar los permisos en el entorno bastión

Revise los permisos en el objeto *AdminSDHolder* del contenedor Sistema de ese dominio. El objeto *AdminSDHolder* tiene una lista de control de acceso (ACL) que se usa para controlar los permisos de las entidades de seguridad que son miembros de los grupos de Active Directory con privilegios integrados. Observe que si se realizaron cambios en los permisos predeterminados que pudieran afectar a los usuarios con privilegios administrativos en el dominio, debido a que esos permisos no se aplicarán a los usuarios con cuentas en el entorno bastión.

## <a name="select-users-and-groups-for-inclusion"></a>Seleccionar usuarios y grupos para su inclusión

El próximo paso es definir los roles de PAM, mediante la asociación de los usuarios y grupos a los que deberían tener acceso. Normalmente, este será un subconjunto de los usuarios y grupos del nivel identificado como administrado en el entorno bastión. Hay más información disponible en [Defining roles for Privileged Access Management (Definición de roles para Privileged Access Management)](defining-roles-for-pam.md).



<!--HONumber=Nov16_HO2-->


