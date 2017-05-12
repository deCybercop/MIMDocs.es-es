---
title: "Deploy MIM Privileged Access Management with Windows Server 2016 (Implementación de Privileged Access Management de MIM con Windows Server 2016) | Microsoft Docs"
description: "Obtenga información sobre cómo implementar Privileged Access Management con Server 2016"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 05/08/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
ms.translationtype: Human Translation
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: fbdebd59249667a0e60d3a248f183bcb6a75085a
ms.contentlocale: es-es
ms.lasthandoff: 05/09/2017


---



# <a name="deploy-mim-pam-with-windows-server-2016"></a>Deploy MIM PAM with Windows Server 2016 (Implementación de MIM PAM con Windows Server 2016)


Este escenario permite a MIM 2016 SP1 aprovechar las características de Windows Server 2016 como controlador de dominio del bosque “PRIV”.  Cuando se configura este escenario, el vale de Kerberos de un usuario estará limitado en el tiempo al tiempo restante de las activaciones del rol. 

>[!Note]
Las primeras vistas previas técnicas de Windows Server 2016 anteriores a Technical Preview 5 no se pueden usar con esta versión de MIM.

## <a name="preparation"></a>Preparación

Se requieren un mínimo de dos máquinas virtuales para el entorno de laboratorio:

-   Una que aloje el controlador de dominio PRIV, con Windows Server 2016

-   Otra que aloje el servicio MIM, con Windows Server 2016 (recomendado) o Windows Server 2012 R2

>[!NOTE]
Si aún no tiene un dominio “CORP” en el entorno de laboratorio, se requiere un controlador de dominio adicional para ese dominio. El controlador de dominio “CORP” puede ejecutar Windows Server 2016 o Windows Server 2012 R2.


Realice la instalación como se describe en la [guía de introducción](privileged-identity-management-for-active-directory-domain-services.md), **excepto tal como se indica a continuación**:

-   Si crea un nuevo dominio CORP, al seguir las instrucciones de [Paso 1: Preparar el controlador del dominio CORP](step-1-prepare-corp-domain.md), puede decidir configurar el dominio CORP para estar en el nivel funcional de Windows Server 2016 de forma opcional. **Si elige esta opción, realice los siguientes ajustes**:

    -   Si usa medios de Windows Server 2016, la opción de instalación se llamará Windows Server 2016 (servidor con Experiencia de escritorio).

    -   Puede especificar el nivel funcional de Windows Server 2016 para el bosque y dominio CORP proporcionando 7 como número de versión del bosque y dominio en el argumento para el comando Install-ADDSForest, como se indica a continuación:
     ```
        Install-ADDSForest –DomainMode 7 –ForestMode 7 –DomainName contoso.local –DomainNetbiosName contoso –Force –NoDnsOnNetwork
        ```
    -   En "Create new users and groups" (Crear nuevos usuarios y grupos), el comando final (New-ADGroup -name 'CONTOSO\$\$\$' …) no **se requerirá si ambos controladores de dominio CORP y PRIV están en el nivel funcional del dominio de Windows Server 2016**.

    -   Los cambios descritos en "Configuración de la auditoría"(elemento n.º 8) y "Configuración del registro" (elemento n.º 10) se **recomiendan, pero no se requieren** si ambos contenedores de dominio CORP y PRIV están en el nivel funcional del dominio de Windows Server 2016.

-   Si decide usar Windows Server 2012 R2 como sistema operativo para CORPDC, debe instalar las revisiones 2919442 y 2919355, [así como la actualización 3155495](http://support.microsoft.com/kb/3156418) en CORPDC.

-   Siga las instrucciones de [Paso 2: Preparar el controlador del dominio PRIV](step-2-prepare-priv-domain-controller.md), excepto para estos ajustes:

    -   Realice la instalación usando medios de Windows Server 2016. La opción de instalación se llamará Windows Server 2016 (servidor con Experiencia de escritorio).

    -   En las instrucciones "Incorporación de roles" (elemento n.º 4), **debe especificar los números de versión del dominio y bosque en la cuarta línea de los comandos de PowerShell que van a ser 7**, para permitir la habilitación de las características de Windows Server AD descritas más tarde.

        ```
        Install-ADDSForest -DomainMode 7 -ForestMode 7 -DomainName priv.contoso.local  -DomainNetbiosName priv -Force -CreateDNSDelegation -DNSDelegationCredential $ca
        ```  

    -   Al configurar los derechos de auditoría e inicio de sesión, tenga en cuenta que el programa Administración de directivas de grupo se ubicará en la carpeta Herramientas administrativas de Windows.

    -   La configuración del registro necesaria para la migración del historial de Id. de seguridad (elemento n.º 8) no **se requiere si el dominio PRIV está en el nivel funcional del dominio de Windows Server 2016**.

    -   Después de configurar la delegación y antes de reiniciar el servidor, habilite las características de Privileged Access Management en Active Directory de Windows Server 2016, inicie una ventana de PowerShell como administrador y escriba los comandos siguientes.

    ```
    $of = get-ADOptionalFeature -filter "name -eq 'privileged access management feature'"
    Enable-ADOptionalFeature $of -scope ForestOrConfigurationSet -target "priv.contoso.local"
    ```

  -   Después de configurar la delegación y antes de reiniciar el servidor, autorice a los administradores MIM y la cuenta de servicio MIM a crear y actualizar las entidades de seguridad de instantáneas.

     a. Inicie una ventana de PowerShell y escriba ADSIEdit.

     b. Abra el menú Acciones y haga clic en “Conectar a”. En la configuración Punto de conexión, cambie el contexto de nomenclatura de “Contexto de nomenclatura predeterminado” a “Configuración” y haga clic en Aceptar.

     c. Después de conectarse, en el lado izquierdo de la ventana, debajo de "Editor ADSI", expanda el nodo de Configuración para ver "CN=Configuration,DC=priv,....". Expanda CN=Configuration y, a continuación, expanda CN=Services.

     d. Haga clic con el botón derecho “CN=Shadow Principal Configuration” y haga clic en Propiedades. Cuando aparezca el cuadro de diálogo Propiedades, cambie a la pestaña Seguridad.

     e. Haga clic en Add (Agregar). Especifique las cuentas "MIMService", así como cualquier otro administrador de MIM que posteriormente realice New-PAMGroup para crear grupos PAM adicionales. Para cada usuario, en la lista de permisos permitidos, agregue "Escritura", "Crear todos los objetos secundarios" y "Eliminar todos los objetos secundarios". Agregue los permisos.

     f. Cambie a la configuración de seguridad avanzada. En la línea que permite el acceso de MIMService, haga clic en Editar. Cambie la configuración "Se aplica a" a"Este objeto y todos los descendientes". Actualice esta configuración de permiso y cierre el cuadro de diálogo Seguridad.

     g. Cierre Editor ADSI.

 -   Después de configurar la delegación y antes de reiniciar el servidor, autorice a los administradores MIM a crear y actualizar la directiva de autenticación.

     a.  Inicie una ventana con privilegios elevados de un **símbolo del sistema** y escriba los siguientes comandos, reemplazando el nombre de la cuenta del administrador MIM por “mimadmin” en cada una de las cuatro líneas:
    ```
       dsacls "CN=AuthN Policies,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:RPWPRCWD;;msDS-AuthNPolicy /i:s

       dsacls "CN=AuthN Policies,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:CCDC;msDS-AuthNPolicy

       dsacls "CN=AuthN Silos,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:RPWPRCWD;;msDS-AuthNPolicySilo /i:s

       dsacls "CN=AuthN Silos,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:CCDC;msDS-AuthNPolicySilo
    ```


-   Siga las instrucciones de [Paso 3: Preparar un servidor de PAM](step-3-prepare-pam-server.md), con estos ajustes.

    -   Si la instalación se realiza en Windows Server 2016, tenga en cuenta que el rol "ApplicationServer" no está disponible.

    -   Si se instala MIM en Windows Server 2016, **no es posible instalar SharePoint 2013**.

-   Siga las instrucciones de [Paso 4: Instalar componentes de MIM en un servidor y estación de trabajo de PAM](step-4-install-mim-components-on-pam-server.md), con estos ajustes.

    -   El usuario que instala el servicio MIM y los componentes de PAM **debe tener acceso de escritura al dominio PRIV en AD**, ya que la instalación de MIM crea una nueva unidad organizativa de AD, "Objetos de PAM".

    -   Si SharePoint no está instalado, no instale el portal de MIM.

-   Siga las instrucciones de [Paso 5: Establecer la confianza](step-5-establish-trust-between-priv-corp-forests.md) con estos ajustes:

    -   Al establecer la confianza unidireccional, realice solo los dos primeros comandos de PowerShell (get-credential y New-PAMTrust), **no realice el comando New-PAMDomainConfiguration**.

    -   Después de establecer la confianza, inicie sesión en PRIVDC como PRIV\\administrador, inicie PowerShell y escriba los siguientes comandos:
  ```
    netdom trust contoso.local /domain:priv.contoso.local /enablesidhistory:yes
     /usero:contoso\\administrator /passwordo:Pass\@word1

     netdom trust contoso.local /domain:priv.contoso.local /quarantine:no
     /usero:contoso\\administrator /passwordo:Pass\@word1  

     netdom trust contoso.local /domain:priv.contoso.local /enablepimtrust:yes
     /usero:contoso\\administrator /passwordo:Pass\@word1
  ```

-   El elemento n.º 5 (comprobación de confianza) no **se requiere si ambos dominios CORP y PRIV están en el nivel funcional del dominio de Windows Server 2016**.

## <a name="more-information"></a>Más información

- [Privileged Access Management para Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md)
- [Configurar el entorno de MIM para Privileged Access Management](configuring-mim-environment-for-pam.md)
- [Configurar PAM mediante scripts](sp1-pam-configure-using-scripts.md)

