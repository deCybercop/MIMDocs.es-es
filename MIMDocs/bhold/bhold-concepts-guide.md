---
title: Guía de los conceptos de Microsoft BHOLD Suite | Microsoft Docs
description: Comience a utilizar los componentes de MIM 2016 instalando y configurando el servicio de sincronización.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/14/2017
ms.topic: conceptual
ms.assetid: ''
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 3749b74fd867601ee05f8e45d273ad2de9144b5b
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701419"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Guía de los conceptos de Microsoft BHOLD Suite

Microsoft Identity Manager 2016 (MIM) permite a las organizaciones administrar todo el ciclo de vida de identidades de usuario y sus credenciales asociadas. Se puede configurar para sincronizar identidades, administrar centralmente certificados y contraseñas, y aprovisionar usuarios en sistemas heterogéneos. Con MIM, las organizaciones de TI pueden definir y automatizar los procesos que se usan para administrar identidades, desde la creación a la cancelación.

Microsoft BHOLD Suite amplía estas funcionalidades de MIM mediante la adición de control de acceso basado en roles. BHOLD permite a las organizaciones definir los roles de usuario y controlar el acceso a datos y aplicaciones confidenciales. El acceso se basa en lo que es adecuado para esos roles. El conjunto de aplicaciones de BHOLD incluye servicios y herramientas que simplifican el modelado de las relaciones entre roles dentro de la organización. BHOLD asigna esos roles a derechos y comprueba que las definiciones de rol y los derechos asociados se aplican correctamente a los usuarios. Estas funcionalidades están totalmente integradas con MIM, lo que proporciona una experiencia sin problemas para los usuarios finales y el personal de TI.

Esta guía le ayudará a comprender cómo funciona el conjunto de aplicaciones de BHOLD con MIM y trata los temas siguientes:

- Control de acceso basado en roles
- Atestación
- Análisis
- Generación de informes
- Access Management Connector
- Integración de MIM

## <a name="role-based-access-control"></a>Control de acceso basado en roles

El método más común para controlar el acceso de los usuarios a los datos y aplicaciones es a través del control de acceso discrecional (DAC). En las implementaciones más comunes, todos los objetos importantes tienen un propietario identificado. El propietario tiene la capacidad de conceder o denegar el acceso al objeto a otros usuarios en función de la identidad individual o la pertenencia a grupos. En la práctica, DAC normalmente da como resultado una gran cantidad de grupos de seguridad, algunos que reflejan la estructura organizativa, otros que representan grupos funcionales (por ejemplo, tipos de trabajo o asignaciones de proyectos) y otros que se componen de colecciones provisionales de usuarios y dispositivos que están vinculados con fines más temporales. Con el crecimiento de las organizaciones, es cada vez más difícil administrar la pertenencia en estos grupos. Por ejemplo, si un empleado se transfiere de un proyecto a otro, los grupos que se usan para controlar el acceso a los recursos de proyectos se deben actualizar manualmente. En esos casos, no es infrecuente que se produzcan errores que pueden impedir la seguridad o la productividad del proyecto.

MIM incluye características que ayudan a mitigar este problema proporcionando un control automático de pertenencia a grupos y listas de distribución. Pero esto no resuelve la complejidad intrínseca de la proliferación de grupos que no están necesariamente relacionados entre ellos de forma estructurada.

Una manera de reducir significativamente esta proliferación es mediante la implementación del control de acceso basado en roles (RBAC). RBAC no desplaza a DAC.  RBAC se basa en DAC proporcionando un marco de trabajo para clasificar a los usuarios y recursos de TI. Esto le permite hacer explícita su relación y los derechos de acceso adecuados de acuerdo a esa clasificación. Por ejemplo, mediante la asignación a un usuario de atributos que especifican su puesto y las asignaciones de proyectos, se puede conceder acceso al usuario a las herramientas necesarias para el trabajo y los datos que necesita para participar en un proyecto determinado. Cuando el usuario asume un trabajo diferente y asignaciones de proyecto diferentes, cambiar los atributos que especifican el puesto y los proyectos del usuario bloquea automáticamente el acceso a los recursos que solo son necesarios para el puesto anterior del usuario.

Dado que los roles pueden estar incluidos en otros roles de forma jerárquica, (por ejemplo, los roles de administrador de ventas y representante de ventas se pueden incluir en el rol de ventas más general), es fácil asignar los derechos adecuados a roles específicos y seguir proporcionando derechos adecuados a todos los usuarios que también comparten el rol más inclusivo. Por ejemplo, en un hospital, se podría asignar a todo el personal el derecho de ver las historias de los pacientes, pero asignar solo a los médicos (un subrol del rol medicina) el derecho de escribir las recetas del paciente. De manera similar, se podría denegar el acceso a las historias de los pacientes a los usuarios que pertenecen al rol administrativo, excepto a los jefes de facturación (un subrol del rol administrativo), a los que se podría conceder acceso a esas partes de las historias de los pacientes que son necesarias para facturarles los servicios que proporciona el hospital.

Un beneficio adicional de RBAC es la capacidad para definir y aplicar la separación de tareas (SoD). Esto permite a una organización definir combinaciones de roles que conceden permisos que un mismo usuario no debe tener, para que no se puedan asignar roles a un usuario determinado que le permitan iniciar un pago y autorizarlo, por ejemplo. RBAC proporciona la capacidad de aplicar automáticamente una directiva de este tipo, en lugar de tener que evaluar la implementación eficaz de la directiva por cada usuario.

### <a name="bhold-role-model-objects"></a>Objetos de modelo de roles de BHOLD

Con el conjunto de aplicaciones de BHOLD, puede especificar y organizar los roles dentro de su organización, asignar usuarios a roles y asignar los permisos adecuados a los roles. Esta estructura se denomina modelo de roles, y contiene y conecta cinco tipos de objetos: 

- Unidades organizativas
- Usuarios
- Roles
- Permisos
- Aplicaciones

#### <a name="organizational-units"></a>Unidades organizativas

Las unidades organizativas (OrgUnits) son el medio principal para organizar a los usuarios en el modelo de roles de BHOLD. Cada usuario debe pertenecer al menos a una OrgUnit. (De hecho, cuando se quita un usuario de la última unidad organizativa de BHOLD, el registro de datos del usuario se elimina de la base de datos de BHOLD).

> [!Important]
> Las unidades organizativas del modelo de roles de BHOLD no deben confundirse con las unidades organizativas de Active Directory Domain Services (AD DS). Por lo general, la estructura de unidades organizativas en BHOLD se basa en la organización y las directivas de la empresa, no en los requisitos de la infraestructura de red.

Aunque no es necesario, en la mayoría de los casos las unidades organizativas se estructuran en BHOLD para representar la estructura jerárquica de la organización, similar a la siguiente:

![](media/bhold-concepts-guide/org-chart.png)

En este ejemplo, el modelo de roles contendría una unidad organizativa para la totalidad de la empresa (representada por el presidente, porque el presidente no forma parte de una unidad organizativa más específica), o bien se podría usar la unidad organizativa raíz de BHOLD (que siempre existe) para ese fin. Las unidades organizativas que representan las divisiones corporativas dirigidas por los vicepresidentes se colocarían en la unidad organizativa corporativa. Después, las unidades organizativas correspondientes a los directores de ventas y marketing se agregarían a las unidades organizativas de ventas y marketing, y las unidades organizativas que representan a los directores de ventas regionales se colocarían en la unidad organizativa para el director de ventas de la región Este. Los socios comerciales, que no administran a otros usuarios, podrían convertirse en miembros de la unidad organizativa del director de ventas de la región Este. Tenga en cuenta que los usuarios pueden ser miembros de una unidad organizativa en cualquier nivel. Por ejemplo, un auxiliar administrativo, que no es un administrador y que depende directamente de un vicepresidente, sería un miembro de la unidad organizativa del vicepresidente.

Además de representar la estructura de la organización, las unidades organizativas también se pueden usar para agrupar usuarios y otras unidades organizativas según criterios funcionales (por ejemplo para proyectos o especialización). En el diagrama siguiente se muestra cómo se podrían usar unidades organizativas para agrupar a los socios comerciales según el tipo de cliente:

![](media/bhold-concepts-guide/org-chart-02.png)

En este ejemplo, cada socio comercial podría pertenecer a dos unidades organizativas: una que representa el lugar del socio en la estructura administrativa de la organización y otra que representa la base de clientes del socio (comerciales o corporativos). A cada unidad organizativa se le pueden asignar distintos roles a los que, a su vez, se pueden asignar permisos diferentes para tener acceso a los recursos de TI de la organización. Además, los roles se pueden heredar de unidades organizativas primarias, simplificando el proceso de propagación de roles a los usuarios. Por otro lado, se puede impedir que se hereden roles específicos, para garantizar que un rol concreto se asocia únicamente a las unidades organizativas adecuadas.

En el conjunto de aplicaciones de BHOLD se pueden crear unidades organizativas mediante el portal web de BHOLD Core o el generador de modelos de BHOLD.

#### <a name="users"></a>Usuarios

Como se mencionó anteriormente, cada usuario debe pertenecer al menos a una unidad organizativa (OrgUnit). Dado que las unidades organizativas son el mecanismo principal para asociar usuarios a roles, en la mayoría de las organizaciones un usuario determinado pertenece a varias unidades organizativas para que resulte más fácil asociar roles a ese usuario. Pero en algunos casos, puede ser necesario asociar un rol a un usuario aparte de las unidades organizativas a las que pertenezca. Por tanto, se puede asignar un usuario directamente a un rol, además de obtener roles de las unidades organizativas a las que pertenece el usuario.

Cuando un usuario no está activo dentro de la organización (mientras está fuera por un permiso por enfermedad, por ejemplo), se le puede suspender, lo que revoca todos los permisos del usuario sin quitarlo del modelo de roles. Cuando se reincorpore, se puede reactivar el usuario, lo que restaura todos los permisos concedidos por los roles del usuario.

Los objetos para los usuarios se pueden crear individualmente en BHOLD a través del portal web de BHOLD Core. También se pueden importar de forma masiva mediante el generador de modelos de BHOLD o el conector de administración de acceso con el servicio de sincronización de FIM para importar información de usuario de orígenes como Active Directory Domain Services o aplicaciones de recursos humanos.

Los usuarios se pueden crear directamente en BHOLD sin usar el servicio de sincronización de FIM. Esto puede ser útil para realizar el modelado de roles en un entorno de preproducción o pruebas. También puede permitir que a usuarios externos (por ejemplo, empleados de un subcontratista) se les asignen roles y, por tanto, tengan acceso a los recursos de TI sin que se agreguen a la base de datos de empleados; pero estos usuarios no podrán usar las características de autoservicio de BHOLD.

#### <a name="roles"></a>Roles

Como se indicó anteriormente, en el modelo de control de acceso basado en roles (RBAC), los permisos están asociados a roles en lugar de a usuarios individuales. Esto permite conceder a cada usuario los permisos necesarios para realizar sus tareas cambiando los roles del usuario en lugar de conceder o denegar los permisos por separado. En consecuencia, la asignación de permisos ya no requiere la participación del departamento de TI, y en su lugar se puede realizar como parte de la administración de la empresa. Un rol puede agregar permisos para tener acceso a diferentes sistemas, ya sea directamente o mediante el uso de subroles, reduciendo así la necesidad de intervención de TI en la administración de permisos de usuario.

Es importante tener en cuenta que los roles son una característica del propio modelo de RBAC; normalmente no se aprovisionan roles a las aplicaciones de destino. Esto permite usar RBAC junto con las aplicaciones existentes que no están diseñadas para usar roles o cambiar las definiciones de rol a fin de satisfacer las necesidades de los modelos de negocio cambiantes sin tener que modificar las propias aplicaciones. Si una aplicación de destino está diseñada para usar roles, puede asociar los del modelo de roles de BHOLD a los correspondientes roles de aplicación si trata los roles específicos de la aplicación como permisos.

En BHOLD, puede asignar un rol a un usuario principalmente a través de dos mecanismos:

- Mediante la asignación de un rol a una unidad organizativa de la que el usuario es miembro.
- Mediante la asignación de un rol directamente a un usuario

El rol asignado a una unidad organizativa primaria puede ser heredado opcionalmente por sus unidades organizativas miembro. Cuando se asigna un rol a una unidad organizativa o se hereda por ella, se puede designar como un rol efectivo o propuesto. Si es un rol efectivo, se asigna a todos los usuarios de la unidad organizativa. Si es un rol propuesto, se debe activar para cada usuario o unidad organizativa miembro para que sea efectivo para ese usuario o los miembros de la unidad organizativa. Esto permite asignar a los usuarios un subconjunto de los roles asociados a una unidad organizativa, en lugar de asignar automáticamente todos los roles de la unidad organizativa a todos los miembros. Además, los roles pueden tener fechas de inicio y finalización asignadas, y se pueden colocar límites en el porcentaje de usuarios dentro de una unidad organizativa para los que un rol puede ser efectivo.

En el diagrama siguiente se muestra cómo se puede asignar un rol a un usuario individual en BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

En este diagrama, un rol A se asigna a una unidad organizativa como un rol heredable y, por tanto, es heredado por sus unidades organizativas miembro y todos los usuarios dentro de esas unidades organizativas. El rol B se asigna como un rol propuesto para una unidad organizativa. Se debe activar antes de poder autorizar a un usuario en la unidad organizativa con los permisos del rol. El rol C es un rol efectivo, por lo que sus permisos se aplican inmediatamente a todos los usuarios en la unidad organizativa. El rol D está vinculado directamente al usuario y por tanto sus permisos se aplican inmediatamente a ese usuario.

Además, un rol se puede activar para un usuario según los atributos del usuario. Para más información, vea Autorización basada en atributos.

#### <a name="permissions"></a>Permisos

En BHOLD, un permiso se corresponde a una autorización importada desde una aplicación de destino. Es decir, cuando se configura BHOLD para trabajar con una aplicación, recibe una lista de autorizaciones que puede vincular a roles. Por ejemplo, cuando se agrega Active Directory Domain Services (AD DS) como una aplicación a BHOLD, recibe una lista de grupos de seguridad que, como los permisos de BHOLD, se pueden vincular a roles en BHOLD.

Los permisos son específicos de las aplicaciones. BHOLD proporciona una sola vista unificada de permisos, para que se pueda asociar con roles sin necesidad de que los administradores de rol conozcan los detalles de implementación de los permisos. En la práctica, es posible que los distintos sistemas apliquen un permiso de manera diferente. El conector específico de la aplicación desde el servicio de sincronización de FIM a la aplicación determina cómo se proporcionan los cambios en los permisos para un usuario a esa aplicación. 

#### <a name="applications"></a>Aplicaciones

BHOLD implementa un método para aplicar control de acceso basado en roles (RBAC) a las aplicaciones externas. Es decir, cuando BHOLD se aprovisiona con usuarios y permisos de una aplicación, puede asociar esos permisos a los usuarios mediante la asignación de los roles a los usuarios y, después, vinculando los permisos a los roles. Después, el proceso en segundo plano de la aplicación puede asignar los permisos correctos a sus usuarios en función de la asignación de roles y permisos en BHOLD.

### <a name="developing-the-bhold-suite-role-model"></a>Desarrollo del modelo de roles del conjunto de aplicaciones de BHOLD

Para ayudarle a desarrollar el modelo de roles, el conjunto de aplicaciones de BHOLD proporciona el generador de modelos (Model Generator), una herramienta completa y fácil de usar.

Antes de usar el generador de modelos, debe crear una serie de archivos que definan los objetos que usa para crear el modelo de roles. Para obtener información sobre cómo crear estos archivos, vea la referencia técnica de Microsoft BHOLD Suite.

El primer paso para usar el generador de modelos de BHOLD consiste en importar estos archivos para cargar los bloques de creación básicos en el generador de modelos. Cuando los archivos se han cargado correctamente, puede especificar los criterios que el generador de modelos usa para crear varias clases de roles:

- Roles de pertenencia que se asignan a un usuario en función de las unidades organizativas (OrgUnits) a las que pertenece.
- Roles de atributo que se asignan a un usuario en función de los atributos del usuario en la base de datos de BHOLD.
- Roles propuestos que están vinculados a una unidad organizativa pero que deben activarse para usuarios específicos.
- Roles de propiedad que conceden a un usuario el control de unidades organizativas y roles para los que no se especificó un propietario en los archivos importados.

> [!Important]
> Al cargar los archivos, active la casilla **Retain Existing Model** (Conservar modelo existente) únicamente en entornos de prueba. En entornos de producción, debe usar el generador de modelos para crear el modelo de roles inicial. No se puede usar para modificar un modelo de roles existente en la base de datos de BHOLD.

Después de que el generador de modelos crea estos roles en el modelo de roles, puede exportarlo a la base de datos de BHOLD en forma de archivo XML.

### <a name="advanced-bhold-features"></a>Características avanzadas de BHOLD

En las secciones anteriores se describieron las características básicas del control de acceso basado en roles (RBAC) en BHOLD. En esta sección se describen características adicionales de BHOLD que pueden proporcionar una seguridad mejorada y la flexibilidad necesaria para la implementación de RBAC de su organización. En esta sección se proporciona información general sobre las siguientes características de BHOLD:

- Cardinalidad
- Separación de tareas
- Permisos adaptables al contexto
- Autorización basada en atributos
- Tipos de atributo flexibles


#### <a name="cardinality"></a>Cardinalidad

La *cardinalidad* hace referencia a la implementación de reglas de negocios diseñadas para limitar el número de veces que dos entidades se pueden relacionar entre sí. En el caso de BHOLD, se pueden establecer reglas de cardinalidad para roles, permisos y usuarios.

Puede configurar un rol para limitar lo siguiente:

- El número máximo de usuarios para los que se puede activar un rol propuesto.
- El número máximo de subroles que se pueden vincular al rol.
- El número máximo de permisos que se pueden vincular al rol.

Puede configurar un permiso para limitar lo siguiente:

- El número máximo de roles que se pueden vincular al permiso.
- El número máximo de usuarios a los que se puede conceder el permiso.

Puede configurar un usuario para limitar lo siguiente:

- El número máximo de roles que se pueden vincular al usuario.
- El número máximo de permisos que se pueden asignar al usuario a través de asignaciones de roles.

#### <a name="separation-of-duties"></a>Separación de tareas

La Separación de tareas (SoD) es un principio empresarial que intenta impedir que los usuarios obtengan la capacidad de realizar acciones que no deben estar disponibles para una misma persona. Por ejemplo, un empleado no debería poder solicitar un pago y autorizarlo. El principio de SoD permite a las organizaciones implementar un sistema de comprobaciones y equilibrios para minimizar su exposición a posibles errores o conductas impropias de los empleados.

BHOLD implementa SoD al permitirle definir permisos incompatibles. Cuando se definen estos permisos, BHOLD aplica SoD evitando la creación de roles que están vinculados a permisos incompatibles, independientemente de que estén vinculados directamente o a través de la herencia, y evitando que se asigne a los usuarios varios roles que, cuando se combinan, les concedan permisos incompatibles, de nuevo bien por asignación directa o a través de la herencia. Opcionalmente, los conflictos se pueden invalidar.

#### <a name="context-adaptable-permissions"></a>Permisos adaptables al contexto

Mediante la creación de permisos que pueden modificarse automáticamente en función de un atributo de objeto, puede reducir el número total de permisos que tiene que administrar. Los permisos adaptables al contexto (CAP) le permiten definir una fórmula como un atributo de permiso que modifica cómo aplica el permiso la aplicación asociada con el permiso. Por ejemplo, puede crear una fórmula que cambia el permiso de acceso a una carpeta de archivos (a través de un grupo de seguridad asociado a la lista de control de acceso de la carpeta) en función de si un usuario pertenece a una organización unidad (unidad organizativa) que contiene empleados a jornada completa o subcontratados. Si el usuario se cambia de una unidad organizativa a otra, el nuevo permiso se aplica automáticamente y el permiso anterior se desactiva. 

La fórmula de CAP puede consultar los valores de los atributos que se han aplicado a las aplicaciones, permisos, unidades organizativas y usuarios.

#### <a name="attribute-based-authorization"></a>Autorización basada en atributos

Una manera de controlar si un rol que está vinculado a una unidad organizativa (unidad organizativa) se activa para un usuario determinado en la unidad organizativa consiste en usar la autorización basada en atributos (ABA). Mediante el uso de ABA, puede activar automáticamente un rol solo cuando se cumplan ciertas reglas basadas en los atributos de un usuario. Por ejemplo, puede vincular un rol a una unidad organizativa que se active para un usuario solo si el cargo del usuario coincide con el de la regla de ABA. Esto elimina la necesidad de activar manualmente un rol propuesto para un usuario. En su lugar, se puede activar un rol para todos los usuarios en una unidad organizativa que tienen un valor de atributo que cumple la regla de ABA del rol. Las reglas se pueden combinar para que un rol solo se active cuando los atributos de un usuario satisfacen todas las reglas de ABA especificadas para el rol.

Es importante tener en cuenta que los resultados de pruebas de reglas de ABA están limitados por la configuración de cardinalidad. Por ejemplo, si la configuración de cardinalidad de una regla especifica que no se puede asignar un rol a más de dos usuarios, y si en otros casos una regla de ABA activaría un rol para cuatro usuarios, el rol se activará únicamente para los dos primeros usuarios que pasan la prueba de ABA.

#### <a name="flexible-attribute-types"></a>Tipos de atributo flexibles

El sistema de atributos de BHOLD es muy extensible. Puede definir nuevos tipos de atributos para objetos como usuarios, unidades organizativas (unidades organizativas) y roles. Los atributos se pueden definir con valores que sean números enteros, valores booleanos (sí/no), alfanuméricos, de fecha, hora y direcciones de correo electrónico. Los atributos se pueden especificar como valores únicos o como lista de valores.

## <a name="attestation"></a>Atestación

El conjunto de aplicaciones de BHOLD proporciona herramientas que se pueden usar para comprobar que se han asignado los permisos adecuados a usuarios concretos para realizar sus tareas empresariales. El administrador puede usar el portal proporcionado por el módulo de atestación de BHOLD para diseñar y administrar el proceso de atestación.

El proceso de atestación se lleva a cabo por medio de campañas, en las que se proporciona a administradores de campaña la oportunidad y los medios de comprobar que los usuarios de los que son responsables tienen el acceso adecuado a las aplicaciones administradas por BHOLD y los permisos correctos en esas aplicaciones. Se designa un propietario de la campaña para supervisarla y para asegurarse de que se lleva a cabo correctamente. Puede crearse una campaña que se produzca una sola vez o de forma periódica.

Normalmente, el administrador de una campaña será el administrador que dará fe de los derechos de acceso de los usuarios que pertenecen a una o más unidades organizativas de las que el administrador es responsable. Los administradores se pueden seleccionar automáticamente para los usuarios que se van a atestiguar en una campaña en función de los atributos de usuario, o bien se pueden definir administradores para una campaña incluyéndolos en un archivo que asigna a un administrador todos los usuarios que se van a atestiguar en la campaña.

Cuando se inicia una campaña, BHOLD envía un mensaje de notificación de correo electrónico a los administradores y al propietario de la campaña, y después envía avisos periódicos para ayudarles a mantener el progreso de la campaña. Los administradores se dirigen a un portal de campaña donde pueden ver una lista de los usuarios de los que son responsables y los roles asignados a esos usuarios. Después, los administradores pueden confirmar si son responsables de cada uno de los usuarios de la lista y aprobar o denegar los derechos de acceso de cada uno de los usuarios de la lista.

Los propietarios de la campaña también usan el portal para supervisar el progreso de la campaña y se registran las actividades de la campaña para que los propietarios puedan analizar las acciones que se realizaron en el transcurso de esta.

## <a name="analytics"></a>Análisis

Una de las consideraciones importantes al implementar un sistema completo de control de acceso basado en derechos (RBAC) es el equilibrio entre mantener el control de acceso estricto y evitar obstáculos innecesarios al acceso (o, lo que es peor, inesperados). El esfuerzo necesario para mantener este equilibrio a menudo da como resultado una estructura de control de acceso que es tan compleja que las interacciones inesperadas entre las directivas son casi inevitables.

Por esta razón, es importante ser capaz de analizar los efectos de las directivas de control de acceso antes de que se definan. El módulo de análisis del conjunto de aplicaciones de BHOLD ofrece también la capacidad de realizar este análisis, permitiendo el desarrollo de reglas que representan las directivas y, después, mostrando los usuarios cuyos permisos cumplen o incumplen la regla. En función de este análisis, puede ajustar la directiva o modificar los roles y permisos para eliminar cualquier conflicto con la directiva.

El portal de análisis de BHOLD ofrece la posibilidad de crear conjuntos de reglas que se componen de una o varias reglas que se crean para probar una directiva concreta o un grupo de directivas. Una regla consta de los siguientes elementos principales:

- Un título y una descripción que permiten identificar y describir la regla.
- Un estado que indica si la regla está preparada para su revisión, se está revisando o está aprobada.
- Un conjunto de elementos (por ejemplo, usuarios o permisos) que la regla está diseñada para probar.
- Filtros de subconjunto opcionales que son expresiones que se pueden usar para seleccionar un subgrupo correspondiente del elemento que se va a examinar.
- Uno o más filtros de regla que son expresiones que representan la directiva que se está probando.

Una regla puede probar cualquiera de los siguientes conjuntos de elementos:

- Usuarios
- Unidades organizativas
- Roles
- Permisos
- Aplicaciones
- Cuentas

En el siguiente diagrama se muestra una regla sencilla que consta de dos reglas de subconjunto y dos reglas de filtro:

![](media/bhold-concepts-guide/rules.png)

Tenga en cuenta la diferencia en el efecto de un filtro de subconjunto con errores y un filtro de regla con errores: un filtro de subconjunto con errores quita un objeto de elemento de la prueba en los filtros siguientes, mientras que un filtro de regla con errores hace que el objeto se notifique como que no cumple la regla. Solo los objetos que pasan todos los filtros de subconjunto y todos los filtros de regla se notifican como que cumplen la regla.

Cada filtro se compone de un tipo, un operador (que es un tipo dependiente), una clave (uno de los elementos) y un valor en el que el operador prueba la clave. Por ejemplo, el filtro siguiente podría probar si el número de usuarios de un subconjunto de elementos es superior a 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Tipo:**   | Número de   |
| **Clave:**  | Usuarios  |
| **Operador**  | >  |
| **Valor:** | 10 |

Los filtros de reglas pueden ser de tres tipos y usar operadores específicos de su tipo, como se indica a continuación:

- Atributo
  - < y >
  - = y !=
  - **Contiene**
  - **No contiene**
- Número de
  - < y >
  - = y !=
- Restrictivos
  - **Debe tener alguno y Debe tener todos**
  - **No puede tener ninguno y No puede todos**
  - **Solo puede tener alguno y Solo puede tener todos**
  - **Exclusivamente tiene alguno y Exclusivamente tiene todos**

> [!Note]
> Los filtros restrictivos pueden usar los operadores indicados para probar una clave con un conjunto de varios valores.

Por ejemplo, si quisiera probar la implementación de una directiva de segregación de tareas (SoD) que indica que ningún usuario que tenga el permiso Solicitar pago también pueda tener el permiso Aprobar pago, podría crear una regla similar a la siguiente:

|   |  |
|---|--|
|Nombre:| Prueba de SoD de pago|
|Elemento:| Usuarios|
|Filtro de subconjunto:| Tener los permisos Solicitar pago|
|Filtro de regla: | No se puede tener ningún permiso Aprobar pago|

Cuando se ejecuta esta regla, el módulo de análisis de BHOLD muestra el número de usuarios en el subconjunto seleccionado (el número de usuarios con el permiso Solicitar pago), el número de usuarios que cumplen la regla y el número de usuarios que no la cumplen. Después, puede mostrar los usuarios que no cumplen la regla para poder corregir el problema de incoherencia.

Además de mostrar los resultados, también puede guardar el informe como un archivo de valores separados por comas (CSV) o XML para poder analizar los resultados más adelante. También puede personalizar el informe resultante para mostrar información adicional que puede ayudarle a entender mejor el impacto. Por ejemplo, si va a probar usuarios, puede mostrar (o notificar) las cuentas de los usuarios que cumplen o no cumplen la norma para poder ver las aplicaciones que están implicadas.

Dado que una regla puede contener varios filtros, puede conectar filtros para comprobar si existe una combinación determinada de condiciones. De forma predeterminada, el resultado es el producto de una prueba booleana Y de todos los filtros, pero puede especificar que se realice una prueba O de la combinación de filtros.

Por ejemplo, si su directiva empresarial requiere que los administradores tengan el permiso Modificar pago o el permiso Aprobar pago, podría probar el cumplimiento de la directiva mediante la creación de una regla similar a la siguiente:


|  |  |
|--|--|
|Nombre: | Prueba de SoD Modificar pago|
|Elemento: | Usuarios |
|Filtro de subconjunto: | Con cualquier rol de administrador|
| Filtros de regla: |Debe tener cualquier permiso Modificar pago </br> Debe tener cualquier permiso Aprobar pago|

De forma predeterminada, cualquier usuario que sea un administrador que tenga los permisos Modificar pago y Solicitar pago se notificará como que cumple la norma. Pero la directiva requiere que un administrador tenga uno de los permisos, no necesariamente los dos. Para probar el cumplimiento compatibilidad real de la directiva, se debe usar el operador booleano O con la regla para determinar si existen administradores que no tienen el permiso Modificar pago o el permiso Aprobar pago.

A diferencia de otros operadores, **Exclusivamente tiene alguno** y **Exclusivamente tiene todos** indican el cumplimiento de los objetos que en otros casos serían excluidos por un filtro de subconjunto. Por ejemplo, para probar una directiva de que todos los administradores (y solo los administradores) tengan el permiso Aprobar revisiones, podría crear una regla como la siguiente:

|  |  |
|--|--|
|Nombre: | Prueba Aprobar revisiones|
|Elemento: | Usuarios|
| Filtro de subconjunto: | Con cualquier rol de administrador
|Filtro de regla: | Tener exclusivamente cualquier permiso Aprobar revisiones|

Esta regla notificará como usuarios que cumplen la norma a los que son administradores y tienen el permiso Aprobar revisiones y a los usuarios que no son administradores y que no tiene el permiso Aprobar revisiones. Por el contrario, los administradores que no tiene ese permiso y los usuarios que no son administradores pero que tienen ese permiso se notifican como que no cumplen la regla.

Como se indicó anteriormente, puede combinar reglas en un conjunto de reglas, lo que facilita clasificar y administrar las reglas para satisfacer sus requisitos empresariales.

También puede definir un conjunto de filtros globales que, cuando se habilitan, se aplican a cualquier regla que se pruebe. Si tiene que excluir un subconjunto específico de registros al probar reglas en conjuntos de reglas diferentes con frecuencia, puede especificar filtros globales que se puedan habilitar o deshabilitar según sea necesario.

## <a name="reporting"></a>Generación de informes

El módulo de informes de BHOLD ofrece la capacidad de ver la información en el modelo de roles a través de una serie de informes. El módulo de informes de BHOLD proporciona un amplio conjunto de informes integrados, además de incluir un asistente que se puede usar para crear informes personalizados básicos y avanzados. Al ejecutar un informe, puede mostrar los resultados inmediatamente o guardarlo en un archivo de Microsoft Excel (.xlsx). Para ver este archivo con Microsoft Excel 2000, Microsoft Excel 2002 o Microsoft Excel 2003, puede descargar e instalar el Paquete de compatibilidad de Microsoft Office para formatos de archivo de Word, Excel y PowerPoint.


El módulo de informes de BHOLD se usa principalmente para generar informes que se basan en información de rol actual. Para generar informes de cambios de auditoría de la información de identidad, Forefront Identity Manager 2010 R2 tiene una funcionalidad de creación de informes para el servicio FIM que se implementa en el almacén de datos de System Center Service Manager. Para más información sobre la creación de informes de FIM, vea la documentación de Forefront Identity Manager 2010 y Forefront Identity Manager 2010 R2 en la biblioteca técnica de Forefront Identity Management.

Las categorías que abarcan los informes integrados incluyen las siguientes:

- Administration
- Atestación
- Controles
- Control de acceso de entrada
- Registro
- Modelo
- Estadísticas
- Flujo de trabajo

Puede crear informes y agregarlos a estas categorías, o bien puede definir sus propias categorías en las que colocar informes personalizados e integrados.

Al crear un informe, el asistente le ayudará a proporcionar los parámetros siguientes:

- Información de identificación, incluidos el nombre, descripción, categoría, uso y destinatarios.
- Los campos que se van a mostrar en el informe.
- Las consultas que especifican los elementos que se van a analizar.
- El orden en el que se van a ordenar las filas.
- Los campos usados para dividir el informe en secciones.
- Los filtros para restringir los elementos que se devuelven en el informe.

En cada fase del asistente, puede obtener una vista previa del informe tal y como se ha definido hasta el momento y guardarlo si no tiene que especificar parámetros adicionales. También puede retroceder a pasos anteriores para cambiar los parámetros que especificó anteriormente en el asistente.

## <a name="access-management-connector"></a>Access Management Connector

El módulo Conector de administración de acceso del conjunto de aplicaciones de BHOLD admite la sincronización inicial y continua de datos en BHOLD. El conector de administración de acceso funciona con el servicio de sincronización de FIM para mover datos entre la base de datos principal de BHOLD, el metaverso de MIM y aplicaciones de destino y almacenes de identidades.

Las versiones anteriores de BHOLD requerían varios agentes de administración para controlar el flujo de datos entre MIM y las tablas de base de datos de BHOLD intermedias. En BHOLD Suite SP1, el conector de administración de acceso permite definir agentes de administración (MA) en MIM que proporcionan la transferencia de datos directamente entre BHOLD y MIM.

## <a name="mim-integration"></a>Integración de MIM

Una característica importante y eficaz de Forefront Identity Manager 2010 y Forefront Identity Manager 2010 R2 es el portal de autoservicio que permite a los usuarios finales ver y administrar su información de identidad y pertenencia. Integración de MIM amplía el Portal de MIM con administración de roles de autoservicio. Por ejemplo, mediante las características de BHOLD del Portal de MIM, un usuario puede solicitar la asignación de roles y ver los roles activos y las solicitudes pendientes. Se pueden conceder funcionalidades adicionales a administradores delegados, como la capacidad de solicitar asignaciones de roles para otros usuarios.

Es importante recordar que las extensiones de BHOLD para el Portal de MIM admiten la administración de autoservicio de roles y flujos de trabajo, así como la creación de informes. Otras funciones de administración de BHOLD, así como la atestación, se proporcionan con los portales web de los módulos BHOLD, que se hospedan por separado desde el Portal de MIM.

## <a name="next-steps"></a>Pasos siguientes

- [Guía de instalación de BHOLD](bhold-installation-guide.md)
- [Referencia para desarrolladores de BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historial de versiones de BHOLD](../reference/version-bhold-history.md)
