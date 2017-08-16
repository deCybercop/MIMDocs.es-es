---
title: "Referencia de XML de configuración de la visualización de control de recursos | Microsoft Docs"
description: 
keywords: 
author: fimguy
ms.author: fimguy
manager: mbaldwin
ms.date: 05/1/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: c6ed843fb15b150fc934062945ab76ba32d72d33
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2017
---
# <a name="resource-control-display-configuration-xml-reference"></a>Referencia de XML de configuración de la visualización de control de recursos

Los recursos de configuración de la visualización de control de recursos (RCDC) son recursos definidos por el usuario que puede usar para controlar cómo aparecen los demás recursos en el almacén de datos de Microsoft Identity Manager 2016 SP1 (MIM) de la interfaz de usuario (UI) para el usuario final. Cada recurso RCDC contiene un archivo de configuración XML que puede cambiar para agregar, modificar o quitar controles y texto de la interfaz de usuario. Si bien MIM 2016 SP1 proporciona varios recursos RCDC predeterminados, también puede crear otros personalizados. Para más información sobre el uso de la interfaz de usuario de RCDC en el portal de FIM, consulte [Introduction to Configuring and Customizing the FIM Portal](http://go.microsoft.com/fwlink/?LinkID=165848) (Introducción a la configuración y personalización del portal de FIM) en la documentación de FIM.


## <a name="known-issues"></a>Problemas conocidos

En muchos controles de configuración de la visualización de control de recursos no se admite el valor predeterminado.

En esta versión, los valores predeterminados de configuración de los controles en un control de recursos no se admiten, excepto para el control de botón de opción. Para solucionar este problema en un cuadro desplegable, especifique un valor predeterminado que no esté asociado con ningún valor para obligar a que el usuario cambie la selección. Para solucionar este problema con otros controles, debe usar un flujo de trabajo de autorización para proporcionar un valor predeterminado durante el envío de la solicitud.

## <a name="basic-structure"></a>Estructura básica

Los datos XML en un recurso RCDC constan del elemento único XML **ObjectControlConfiguration.**

>[!NOTE]
Para ver el esquema XSD completo, consulte el Apéndice A: Esquema XSD predeterminado más adelante en este documento.

Este es el esquema XSD para el elemento ObjectControlConfiguration:

```XML
<xsd:element name="ObjectControlConfiguration"\>
  <xsd:complexType\>
    <xsd:sequence\>
      <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:Panel"/>
      <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
    </xsd:sequence>
    <xsd:attribute ref="my:TypeName"/>
    <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
  </xsd:complexType>
</xsd:element>
```



El elemento **ObjectControlConfiguration** contiene lo siguiente:

1.  **ObjectDataSource**: este elemento especifica el valor TypeName de una clase de origen de datos que usa el control de recursos (RC). Para ver una descripción y la definición de esquema, consulte la siguiente sección Orígenes de datos de este documento. Un elemento **ObjectControlConfiguration** puede contener hasta 32 nodos del elemento **ObjectDataSource**.

2.  **XmlDataSource**: se trata de un origen de datos de ejemplo que se usa normalmente para especificar el diseño de una página de resumen. Para ver una descripción y la definición de esquema, consulte la siguiente sección Orígenes de datos de este documento. Un elemento **ObjectControlConfiguration** puede contener hasta 32 nodos del elemento **XmlDataSource**.

3.  **Panel**: el administrador puede personalizar el diseño de la página de RCDC mediante la modificación de los elementos del panel. Para más información, consulte la sección Panel más adelante en este documento. Un elemento **ObjectControlConfiguration** solo debe tener un elemento Panel.

4.  **Eventos**: los administradores no pueden proporcionar código personalizado subyacente; esta característica está limitada. Este es el evento que puede emitir un panel o control, según el cambio de estado. Para más información, consulte la sección Eventos más adelante en este documento. Un elemento **ObjectControlConfiguration** puede contener opcionalmente un elemento **Event**. En general, el uso de **eventos** personalizados no se admite a menos que se desarrolle específicamente en posteriores mejoras.

## <a name="data-sources"></a>Orígenes de datos

Microsoft Identity Manager usa orígenes de datos como una forma de enlazar datos a componentes de la interfaz de usuario. Esto ayuda a facilitar la separación de los datos de la capa de presentación. Existen dos tipos de orígenes de datos en los datos de configuración de recursos RCDC: **ObjectDataSource** y **XmlDataSource**.

-   **ObjectDataSource** especifica una clase de .NET de Microsoft que proporciona los datos al RC. Hay un conjunto fijo de tipos disponibles de ObjectDataSources que se proporcionan que el administrador puede elegir consumir al crear recursos RCDC.

-   **XMLDataSource** ofrece una manera sencilla de estructurar datos basados en XML, y los administradores pueden usarlos para proporcionar datos personalizados. Los datos XML deben especificarse directamente en el RCDC, a menos que use la estructura XML predefinida integrada. La estructura XML integrada se usa para generar páginas de resumen en el RC.

En el RCDC, puede enlazar estos orígenes de datos a atributos de los controles de interfaz de usuario que se especifican en el RCDC para generar la interfaz de usuario.

### <a name="objectdatasource"></a>ObjectDataSource

Microsoft Identity Manager proporciona los tipos de orígenes de datos comunes de la tabla siguiente que están disponibles para todos los tipos de recursos (excepto donde se indica).

| TypeName                        | Descripción     | Admite enlace bidireccional | Admite sintaxis de enlace        |
|----------------|-----------|-------------------------|--------------|
| PrimaryResourceObjectDataSource | Representa el recurso de FIM 2010 que se crea, edita o visualiza. La ruta de acceso en la cadena de enlace es el nombre del atributo. Tenga en cuenta que el tipo de recurso se especifica mediante el atributo TargetObjectType del RCDC en lugar del atributo RCDC.ConfigurationData. | Sí                     | [AttributeName] Valor del atributo de objeto proporcionado por su nombre.    |
| PrimaryResourceDeltaDataSource  | Este origen de datos genera el XML diferencial que compara el estado original y el estado actual del recurso de FIM 2010. El control de resumen de RC consume el XML diferencial generado para representar la interfaz de usuario de la solicitud que envía el usuario.                                    | No                      | DeltaXml: </br> Se usa con el control de resumen para mostrar las diferencias.                                                 |
| PrimaryResourceRightsDataSource | Este origen de datos proporciona los derechos en línea para cada atributo del recurso de FIM 2010. Permite al RC determinar antes del envío qué permisos tiene el usuario sobre ese atributo y luego representar la interfaz de usuario de ese atributo de forma adecuada.                     | No                      | [AttributeName]                                                                                         |
| SchemaDataSource                | Este origen de datos puede usarse para acceder a información relacionada con el esquema, como el nombre para mostrar, la descripción, si el atributo es o no obligatorio, e información del tipo de recurso.                                                                                             | No                      | [AttributeName].**Obligatorio**: valor booleano que indica si el atributo debe tener un valor para ser válido. <br/> [AttributeName].**DisplayNameString**: valor que indica el nombre para mostrar del enlace. <br/>[AttributeName].**DescriptionString**: valor que indica la descripción del enlace. <br/>[AttributeName].StringRegexString: valor que indica la expresión regular de cadena de un enlace. <br/>[AttributeName].**DisplayName** <br/> [AttributeName].**Description** <br/> [AttributeName]. [AttributeName].**IntergerValueMinimum** <br/>[AttributeName].**IntergerValueMaximum** <br/>[AttributeName].**LocalizedAllowedValues**|
| DomainDataSource                | Este origen de datos proporciona una enumeración de dominios, en función de los recursos de configuración de dominio. Tenga en cuenta que este origen de datos solo se puede usar en los recursos RCDC que sean para recursos de grupo y recursos de usuario.                                                                           | Sí                     | Dominio           |

El siguiente es un fragmento de código de ejemplo de RCDC que enlaza tres orígenes de datos al control UocTextBox para editar el atributo Description de un grupo:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceObjectDataSource" my:Name="object" my:Parameters=""/>
<my:ObjectDataSource my:TypeName="SchemaDataSource" my:Name="schema"/>
<my:ObjectDataSource my:TypeName="PrimaryResourceRightsDataSource" my:Name="rights"/>

     <my:Control my:Name="Description" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Description.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Description}">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
               <my:Property my:Name="Rows" my:Value="3"/>
               <my:Property my:Name="Columns" my:Value="60"/>
               <my:Property my:Name="MaxLength" my:Value="450"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=Description, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
```

### <a name="xmldatasource"></a>XMLDataSource

Con un elemento **XMLDataSource**, puede especificar datos personalizados que el RCDC puede consumir para un recurso dado. En este caso, los datos XML deben especificarse en el RCDC. Como alternativa, este origen de datos se puede usar para hacer referencia a una estructura de datos XML integrada para representar la interfaz de usuario de las páginas de resumen. Puede controlar qué tipo de **XMLDataSource** se usará al definirlo en el RCDC.


| TypeName                 | Descripción   | | |
|--------------------------|------------|
| **XMLDataSource**            | El origen de datos representa los datos XML. Pueden tener el formato XSL o XSL incrustado: <br/>**Formato XSL:** <br/> Microsoft.IdentityManagement.WebUI.Controls.dll<my:XmlDataSource my:Name=" <br/>summaryTransformXsl"my:Parameters=”Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl”> </my:XmlDataSource><br/> **Formato XSL incrustado:** <br/> <my:XmlDataSource my:Name="RequestStatusTransformXsl"> <br/> <xsl:stylesheet version="1.0" xmlns:xsl=http://www.w3.org/1999/XSL/Transform <br/> xmlns:msxsl="urn:schemas-microsoft-com:xslt"><br/></xsl:stylesheet></my:XmlDataSource>                       |No | ```Xpath[;namespaces]``` <br/> Donde Xpath es una expresión Xpath XML válida para seleccionar la nota necesaria, con frecuencia "/" (root) <br/>Namespaces es una lista opcional de cadenas prefix=URI, delimitadas por punto y coma, que se requiere para que la expresión Xpath funcione en el XML con espacio de nombres. |
| **ReferenceDeltaDataSource** | El origen de datos representa diferencias de atributos de referencia multivalor. Se usa únicamente en RCDC para Group y Set. <br/> Aunque el origen de datos no está limitado Group y Set, se requieren cambios de código en el host de RCDC para enviar tales diferencias. Actualmente, Group y Set son los únicos hosts que reconocen este origen de datos.  | Sí                      | [AttributeName].Add donde [AttributeName] representa un atributo de referencia y los datos devueltos son las adiciones diferenciales. <br/> Ejemplo: [ReferenceAttribute].Add <br/>Ejemplo: ```<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}" />``` <br/>[AttributeName].Remove donde [AttributeName] representa un atributo de referencia y los datos devueltos son las eliminaciones diferenciales. <br/> DeltaXml |
|**RequestDetailsDataSource**| El origen de datos representa el atributo RequestParameter de objetos Request. El parámetro establece el número máximo de valores de atributo que se mostrarán por atributo multivalor. Solo se usa en RCDC para Request. ```<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />```| No | DeltaXml |
|**RequestStatusDataSource**| El origen de datos representa el atributo **RequestStatusDetails** de objetos Request. <br/>Se usa únicamente en RCDC para Request.  | No | DeltaXml |

-   Para definir un origen de datos XML personalizado:

 ```XML
   <my:XmlDataSource my:Name="MyCustomData" >
   %Insert custom, properly formatted XML data here%
   </my:XmlDataSource>
   ```

-   Para usar el XSL de control de resumen integrado, defina el origen de datos de la manera siguiente:

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

 Si va a crear un RCDC para un tipo de recurso personalizado, puede usar este método para representar automáticamente una página de resumen para ese recurso personalizado.

El siguiente es un ejemplo de cómo crear una pestaña de resumen en el RCDC, usando PrimaryResourceDeltaDataSource con XMLDataSource mediante el XSL integrado:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceDeltaDataSource" my:Name="delta" />
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />

<my:Grouping my:Name="summaryGroup" my:Caption="Summary” my:IsSummary="true">
     <my:Control my:Name="summaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}" />
              <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}" />
          </my:Properties>
     </my:Control>
</my:Grouping>
```

Como alternativa, el usuario puede reemplazar el elemento XmlDataSource especificado anteriormente por el formato siguiente para definir un diseño personalizado de una página de resumen. Como referencia, el XSL de resumen predeterminado de FIM 2010 se incluye en el Apéndice B: XSL de resumen predeterminado, más adelante en este documento.

```XML
<my:XmlDataSource my:Name="summaryTransformXsl">
     Insert valid XSL code here
</my:XmlDataSource>
```
### <a name="schema-for-data-sources"></a>Esquema para orígenes de datos
Este es el esquema XSD para los dos tipos de orígenes de datos:

```XML
<xsd:element name="ObjectDataSource">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
     </xsd:complexType>
</xsd:element>
<xsd:element name="XmlDataSource">
     <xsd:complexType  mixed="true">
          <xsd:sequence>
              <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
    </xsd:complexType>
</xsd:element>
```

## <a name="events"></a>Eventos
Un evento define el estado de cambio de un control. La extensibilidad de esta característica está limitada porque no se puede escribir una función personalizada (Handler) para definir cuál es el comportamiento una vez que se desencadena un evento. Puede usarse el mismo elemento Event en el elemento Panel. Para más información, consulte la sección Panel más adelante en este documento. Este es el esquema XSD para el elemento Event:

```XML
<xsd:element name="Events">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Event">
     xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
     </xsd:element>
```


Un evento es un elemento vacío y tiene dos atributos.

**Atributos:**

1.  **Name**: este es el nombre único de un evento. El único evento admitido en **ObjectControlConfiguration** es el evento Load. Este evento se desencadena la primera vez que se carga la página.

2.  **Handler**: es el nombre único de un controlador. Cuando se desencadena el evento, se llama normalmente a un método Program para controlar el cambio del estado del control. No se admiten los siguientes casos: quitar un controlador existente de un control existente y crear un nuevo controlador y asociarlo a un control nuevo o existente.

Ejemplo:

Este es un ejemplo de un elemento Events.
```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

**Panel**


El elemento Panel es el elemento principal en un diseño de RCDC. Este es el esquema XSD para el elemento Panel:

```XML
<xsd:element name="Panel">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:DisplayAsWizard"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:AutoValidate"/>
     </xsd:complexType>
</xsd:element>
```

Este elemento contiene un elemento recurrente, Grouping. Para más información, consulte la sección Agrupación de este documento.

El elemento Panel contiene cuatro atributos:

1.  **Name**: el nombre del panel. Este es un atributo de tipo cadena obligatorio.

2.  **DisplayAsWizard**: este atributo está actualmente en desuso. El atributo VerbContext correspondiente en el RCDC controla si el diseño de recursos está en modo Wizard o en modo Tab. Si se establece en 0 (modo Create), también está en modo Wizard. En caso contrario, está en modo Tab. Para más información, consulte Introduction to Configuring and Customizing the FIM Portal (Introducción a la configuración y personalización del portal de FIM) en la documentación.

3.  **Caption**: este atributo está actualmente en desuso. El usuario puede especificar los títulos de una página mediante un elemento Group que solo contiene información de encabezado. Para más información, consulte la sección Agrupación de este documento.

4.  **AutoValidate**: es un atributo booleano opcional. Cuando se establece en true, se desencadena la validación en cada control de la pestaña actual. De forma predeterminada, si falta este atributo, se establece en true. Se puede usar en combinación con la propiedad RegularExpression. Para más información, consulte "RegularExpression" en una sección posterior de este documento.

## <a name="grouping"></a>Grouping

El elemento Grouping define el diseño general de un panel. Funciona como un contenedor que agrupa los controles individuales en distintas secciones y pestañas. Este es el esquema XSD del elemento Grouping:

```XML
<xsd:element name="Grouping">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:IsHeader"/>
          <xsd:attribute ref="my:IsSummary"/>
     </xsd:complexType>
</xsd:element>
```


Existen tres tipos de **agrupaciones**:

1.  **Agrupación de encabezado**: la agrupación de encabezado es opcional. Solo puede haber una agrupación de encabezado en un **panel**. Una agrupación de encabezado aparece encima de un panel como título.
    Solo se permite el uso de un control UocCaptionControl en esta agrupación. Para ver un ejemplo de una agrupación de encabezado, consulte la sección Ejemplo.

2.  **Agrupaciones de contenido**: se requiere al menos una agrupación de contenido. Puede haber varias agrupaciones de contenido en un panel. Una agrupación de contenido aparece como el contenido principal de una página de RCDC. Cada agrupación de contenido aparece como una pestaña en el mismo panel y puede contener entre 1 y 256 controles. Consulte la sección Ejemplo a continuación para ver un ejemplo de una **agrupación de contenido**.

3.  **Agrupaciones de resumen**: la agrupación de resumen es opcional. Solo puede haber una agrupación de resumen en un panel. Una agrupación de resumen aparece como la última pestaña de un panel. Solo se puede usar un control **UocHtmlSummary** en una agrupación de resumen para mostrar los cambios que ha realizado el usuario antes de enviar la solicitud. Consulte la sección Ejemplo a continuación para ver un ejemplo de una agrupación de resumen.

Cada tipo de agrupación contiene los siguientes elementos:

1.  **Help**: este elemento proporciona texto de ayuda en una pestaña. También puede usarlo para agregar un vínculo a un archivo de Ayuda para la pestaña.

2.  **Controls**: para obtener información sobre este elemento, consulte la sección Control de este documento. Cada agrupación debe tener entre 1 y 256 controles, ambos inclusive, según el tipo de la agrupación.

3.  **Events**: para obtener información sobre este elemento, consulte la sección Eventos de este documento. Cada agrupación puede tener, opcionalmente, un evento. Los eventos que se admiten en un elemento Grouping son los siguientes:

    - **BeforeLeave**: este evento se desencadena cuando el usuario está preparado para abandonar una pestaña en una agrupación de contenido.
    - **AfterEnter**: este evento se desencadena cuando el usuario está preparado para entrar en una pestaña en una agrupación de contenido.

Atributos:

1.  **Name**: es el nombre obligatorio de la agrupación. El **nombre** debe ser único dentro del **panel**.

2.  **Caption**: el **título** aparece como el título de encabezado en una agrupación de encabezado. Aparece como el título de pestaña de una agrupación de contenido o de resumen.

3.  **Description**: un atributo de cadena opcional; **Description** solo es funcional cuando se usa en una agrupación de contenido. Use este elemento para proporcionar al usuario final algunos detalles sobre la información dentro de la misma pestaña.

  >[!NOTE]
  Si este atributo se usa en una agrupación de resumen, el XML se considera no válido. Si este atributo se usa en una agrupación de encabezado, el XML se considera válido, pero se omite.

4.  **Enabled**: un atributo booleano opcional; se establece en true cuando está ausente. Si Enabled se establece en false, el usuario final ve una pestaña Deshabilitado. Este atributo solo funciona en una agrupación de contenido.

  >[!NOTE]
  Si este atributo se usa en una agrupación de resumen, el XML se considera no válido. Si este atributo se usa en una agrupación de encabezado, el XML se considera válido, pero se omite.

5.  **Visible**: puede ocultar una pestaña de página de RCDC o su encabezado si establece este atributo en false. De forma predeterminada, este atributo de tipo booleano opcional se establece en true. Este atributo solo funciona en una agrupación de contenido.

  >[!NOTE]
  Cuando solo hay una agrupación de contenido en un panel, esta característica no funciona. Cuando hay más de una agrupación de contenido en un panel, se comporta como se ha descrito anteriormente.

6.  **IsHeader**: es un atributo booleano opcional que define si la agrupación es una agrupación de encabezado. Si no se especifica este atributo, se establece en false.

7.  **IsSummary**: es un atributo booleano opcional que define si la agrupación es una agrupación de resumen. Si no se especifica este atributo, se establece en false.

![Configuración de XML de RCD](media/rcd-configuration-xml-reference/image005.jpg)

El siguiente código de ejemplo XML genera la agrupación de encabezado anterior. La agrupación de encabezado es el área con la agrupación de encabezado de ejemplo de texto.

```XML
<!--Sample for a Header Grouping-->
<my:Grouping my:Name="HeaderGroupingSample" my:IsHeader="true">
     <my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Header Grouping">
          <my:Properties>
               <my:Property my:Name="MaxHeight" my:Value="32"/>
               <my:Property my:Name="MaxWidth" my:Value="32"/>
          </my:Properties>
      </my:Control>
</my:Grouping>
<!--End of Header Grouping Sample-->
```

![Configuración de XML de RCD](media\rcd-configuration-xml-reference/image007.jpg)

El siguiente código de ejemplo XML genera la agrupación de contenido anterior. La agrupación de contenido es la pestaña del extremo izquierdo con el texto **Sample Content Grouping**.

```XML
<!--Sample for a Content Grouping-->
<my:Grouping my:Name="ContentGroupingSample" my:Caption="Sample Content Grouping" my:Description="Some description for content grouping">
     <my:Control my:Name="DisplayName" my:TypeName="UocTextBox" my:Caption="Display name" my:Description="This is the display name of the set.">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="True"/>
               <my:Property my:Name="MaxLength" my:Value="128"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Content Grouping Sample-->
```

![Configuración de XML de RCD](media/rcd-configuration-xml-reference/image010.jpg)

El siguiente código de ejemplo XML genera la agrupación de resumen anterior. La agrupación de resumen es la pestaña del extremo derecho con el texto **Summary**.

```XML
<!--Sample for a Summary Grouping-->
<my:Grouping my:Name="Summary" my:Caption="Sample Summary Grouping" my:IsSummary="true">
     <my:Control my:Name="SummaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}"/>
               <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Summary Grouping Sample-->
```
### <a name="help"></a>Ayuda

El elemento Help, como opción, se puede incluir en una agrupación o un elemento Control. Si se usa en una agrupación, debe ser el primer elemento utilizado. Ofrece Ayuda textual a los usuarios finales para ayudarles a proporcionar información precisa. Este es el esquema XSD para el elemento Help:

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```
A continuación se muestra un ejemplo del elemento Help:
```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control"></a>Control

Un elemento Grouping contiene uno o más elementos Control. Los controles son los elementos principales de un RCDC. Puede personalizar el elemento Grouping mediante la definición de los distintos elementos Control que contiene. Este es el esquema XSD para el elemento Control:

```XML
<xsd:element name="Control">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:ExpandArea"/>
          <xsd:attribute ref="my:Hint"/>
          <xsd:attribute ref="my:AutoPostback"/>
          <xsd:attribute ref="my:RightsLevel"/>
     </xsd:complexType>
</xsd:element>
```

Un control contiene los siguientes elementos:

1.  **Help**: este elemento se omite. Solo funciona en Grouping.

2.  **CustomProperties**: este elemento no se admite.

3.  **Options**: este elemento solo se usa en combinación con los controles **UocDropDownList** o **UocRadioButtonList**. No funciona con ningún otro control. Consulte la sección Options de este documento para ver la estructura de este elemento. Consulte el control individual para ver cómo se usa en el contexto de un control.

4.  **Buttons**: este elemento se usa únicamente en combinación con el control **UocListView**. No funciona con ningún otro control. Para más información, consulte la sección UocListView de este documento.

5.  Properties: este elemento se usa en todos los controles para especificar comportamientos adicionales de un control. Para más información sobre este elemento, consulte la sección Propiedades de este documento.

6.  **Events**: para ver la estructura de este elemento, consulte la sección Eventos anteriormente en este documento. Consulte la definición de control individual para determinar qué eventos se usan en ese control.

El elemento Control contiene los siguientes atributos:

1.  **Name**: es el nombre del control. El nombre de un control debe ser único dentro de cada panel. Este es un atributo de tipo cadena obligatorio.

2.  **TypeName**: este atributo especifica qué tipo de control es. Este es un atributo de tipo cadena obligatorio. Consulte la sección Controles individuales de este documento con cada nombre de control.

3.  **Caption**: puede usar este atributo para incluir un título con el control.
    El título suele ser el nombre para mostrar de los datos que muestra o escribe el control. Puede especificar un valor para el título explícitamente o enlazarlo con la información de nombre para mostrar del atributo de esquema. El título aparece en el extremo izquierdo de un control de tamaño normal. Si un control abarca la pantalla completa, el título aparece sobre el control. Este es un atributo de tipo de cadena opcional. Para más información sobre cómo enlazar un origen de datos a un atributo o un valor de propiedad, consulte la sección Propiedades.

   En el ejemplo siguiente se muestra cómo se puede usar un título explícitamente:

   ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
   ```
     En el ejemplo siguiente se muestra cómo se puede usar un título con un origen de datos. Si ha usado la plantilla para un origen de datos mostrado anteriormente en este documento, el origen de datos es schema. Se recomienda enlazar el elemento DisplayName del atributo a un atributo Caption.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```
4.  Enabled: es un atributo de tipo booleano opcional. Al establecer este valor de atributo en false, el usuario puede deshabilitar un control. El valor predeterminado se establece en true.

5.  Visible: es un atributo de tipo booleano opcional. Este atributo se puede usar para ocultar la totalidad del control. El valor predeterminado se establece en true.

6.  Description: use este atributo de tipo cadena opcional para incluir una descripción que ayude al usuario final a comprender lo que debe poner en el control o lo que hace el control. Puede especificar explícitamente un valor para la descripción o enlazarla a la información de la descripción del atributo de esquema. <br/>La descripción aparece en el extremo izquierdo de un control de tamaño normal debajo del título. Si un control abarca la pantalla completa, la descripción aparece en la parte superior del control debajo del título. Para más información sobre cómo enlazar un origen de datos a un atributo o un valor de propiedad, consulte la sección Propiedades de este documento.

7.  En el ejemplo siguiente se muestra cómo se puede usar una descripción explícitamente:
  ```XML
  <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
  ```
  En este ejemplo se muestra cómo se puede usar una descripción con un origen de datos. Si ha usado la plantilla para un origen de datos mostrado anteriormente en este documento, el origen de datos es **schema**. Se recomienda enlazar el elemento **Description** del atributo a un atributo Description.
  ```XML
  <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
  ````
8. ExpandArea: este atributo indica si el control abarca la pantalla completa. Este es un atributo de tipo booleano opcional. El valor predeterminado se establece en false.

    >[!NOTE]
    Los atributos Caption y Description están deshabilitados cuando este atributo está establecido en true. Debe usar el control UocLabel para proporcionar un título para el control expandido.
9. **Hint**: este es un atributo de tipo cadena opcional. El texto del atributo Hint ayuda al usuario final a decidir lo que es una entrada válida para el control. La sugerencia aparece debajo del control.

10.  **AutoPostback**: este es un atributo de tipo booleano opcional. El valor predeterminado es falso. Si se establece en false, puede que al actualizar la página no se actualice el control. Para más información sobre AutoPostback, busque la propiedad del control de la interfaz de usuario de Microsoft ASP.NET del mismo nombre.

11. **RightsLevel**: este es un atributo de tipo de cadena opcional. Solo puede enlazar este atributo con derechos en línea a un origen de datos. El control se activa o deshabilita dinámicamente, en función de los derechos del usuario. Para más información sobre cómo enlazar orígenes de datos a un atributo o un valor de propiedad, consulte la sección Propiedades de este documento.

    En este ejemplo se muestra cómo un atributo **RightsLevel** se puede usar con un origen de datos. Si ha usado la plantilla para un origen de datos mostrado anteriormente en este documento, el origen de datos es **rights**. Use el nombre de atributo como ruta de acceso.

### <a name="properties"></a>Propiedades

Puede usar una propiedad para personalizar aún más el comportamiento de cada control. Una propiedad es un elemento vacío. Este es el esquema XSD para el elemento Property:
```XML
<xsd:element name="Properties">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Property">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```



Cada propiedad tiene dos atributos obligatorios:

1.  **Name**: este atributo de tipo cadena es el nombre único de la propiedad.
    Diferentes controles tienen propiedades diferentes. Hay algunas propiedades comunes que pueden usar todos los controles. Para más información sobre qué nombres están disponibles para un control dado, consulte la sección Propiedades comunes y Controles individuales.

2.  **Value**: este es el valor de la propiedad. El tipo de datos del valor depende de la propiedad que tenga asignada. Consulte la sección siguiente para conocer el formato de valor permitido para propiedades específicas.

Algunas propiedades se pueden enlazar a información de un origen de datos. Para ello, debe usar el siguiente formato de cadena. Vea las propiedades individuales en la sección Controles individuales para averiguar cómo enlazarlas a un origen de datos.

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}”

SourceExpression:= “Source=” + [ObjectDataSourceName]

PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

ModeExpression:= “Mode=” + [ModeChoice]

ModeChoice:= “OneWay”|”TwoWay”

ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

AttributeName:= valid schema attribute name from the data source.

AttributePropertyName:= valid property name of a schema attribute from the data source.

````
**EJEMPLO:**

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>

```



<a name="common-properties"></a>Propiedades comunes
-----------------

Todos los controles de RCDC que se especifican en este documento pueden tener las siguientes propiedades comunes. Puede usar estas propiedades junto con otras propiedades que son específicas de diferentes controles.

1.  Required: esta propiedad indica que el campo es obligatorio u opcional. Un campo obligatorio debe rellenarse con un valor. No se admite un valor vacío en el caso de entrada de cadena. Un campo opcional puede dejarse vacío. Si este campo es un campo obligatorio y no tiene ningún valor rellenado, aparece un mensaje de error en la parte superior del control de entrada. Puede especificar explícitamente si un campo es obligatorio u opcional. También puede enlazar el campo a la información de esquema de un enlace dado entre un atributo y un tipo de recurso. De forma predeterminada, si esta propiedad está ausente, significa que el control es un control de entrada opcional.

   Este es un ejemplo que usa un valor explícito para esta propiedad:

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```
   Este es un ejemplo que usa un origen de datos dinámico para esta propiedad. Si ha usado la plantilla para un origen de datos mostrado en la sección anterior de este documento, el origen de datos es schema. Use \<nombre de atributo\>.Required como la ruta de acceso.

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```
2. **ReadOnly**: al establecer esta propiedad en true, el usuario final experimenta el control en modo de solo lectura. Este es un atributo de tipo booleano opcional.
    El valor predeterminado se establece en false. Sin embargo, en ocasiones, el comportamiento de esta propiedad se invalida por el tipo de derechos que tiene una persona sobre el enlace de datos al control. Por ejemplo, si un usuario no tiene derechos para actualizar un campo y el campo está enlazado a derechos en línea, el usuario ve los datos en modo de solo lectura incluso si esta propiedad está establecida en false.

3.  **RegularExpression**: esta propiedad especifica las restricciones que se imponen sobre el valor del control. Los formatos de este valor de propiedad son los que se admiten en el estándar StringRegex de .NET. Para más información, consulte [.NET Framework Regular Expressions](http://go.microsoft.com/fwlink/?LinkId=165361) (Expresiones regulares de .NET Framework). Si el control se usa para insertar un valor, el valor se compara con la restricción especificada en esta propiedad cuando el usuario intenta abandonar la página actual.
    El mensaje de error aparece en la parte superior del control que tiene una entrada no válida. El usuario puede especificar explícitamente una expresión regular de cadena. El usuario también puede enlazarla a la información de esquema de un atributo dado. De forma predeterminada, si esta propiedad está ausente, significa que el control no comprueba las restricciones en las cadenas de entrada.
    Este es un ejemplo que usa un valor explícito para esta propiedad:

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```
    Este es un ejemplo que usa un origen de datos dinámico para esta propiedad. Si ha usado la plantilla para un origen de datos mostrado anteriormente en este documento, el origen de datos es schema. Use <attribute name>.StringRegex como la ruta de acceso.
    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```
4.  Visible: es un atributo de tipo booleano opcional. Este atributo se puede usar para ocultar la totalidad del control. El valor predeterminado se establece en true.

### <a name="options"></a>Options

El elemento Options incluye uno o varios subnodos de opción. El elemento Options solo se usa con los controles UocRadioButtonList y UocDropDownList. Para más información sobre cómo usarlas, consulte la sección Controles individuales. Este es el esquema XSD para el elemento Options:

```XML
<xsd:element name="Options">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Option">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```


Atributos:

1.  Value: este es un atributo obligatorio de tipo cadena. El atributo de valor debe ser único dentro del mismo control. Solo se pueden usar caracteres de la A a la Z, que no distinguen mayúsculas de minúsculas.

2.  Caption: este atributo obligatorio es el nombre para mostrar de cada opción.

3.  Hint: este es un atributo opcional. Use este atributo para proporcionar más información y sugerencias al usuario final.

### <a name="environment-variables"></a>Variables de entorno

Las variables de entorno en la tabla siguiente se puedan usar en cualquier configuración de RCDC.

| Variable | description |
|--------|--------|
| `<LoginID>`       | Muestra el identificador del usuario que ha iniciado sesión en ese momento.           |
| `<LoginDomain>`   | Muestra el dominio del usuario que ha iniciado sesión en ese momento.       |
| `<Today>  `       | Muestra la fecha y hora actuales.                                |
| `<FromToday_nnn>` | Muestra la fecha actual, más nnn y la hora. nnn es un entero.  |
| `<ObjectID> `     | El identificador de recurso principal de RCDC.                                     |
| `<Attribute_xxx>` | Devuelve un atributo especificado, xxx, del recurso principal de RCDC. |

### <a name="debugging-xml-configuration-files"></a>Depuración de archivos de configuración XML


Si va a desarrollar o modificar archivos de configuración XML para un RCDC, puede servir de ayuda validar el XML con los archivos XSD, mediante un editor como Microsoft Visual Studio®. Para más información, consulte [An Introduction to the XML Tools in Visual Studio 2005](http://go.microsoft.com/fwlink/?LinkID=74512) (Introducción a las herramientas de XML en Visual Studio 2005).

### <a name="customizing-a-help-file"></a>Personalización de un archivo de Ayuda

Si crea nuevos atributos y recursos, puede que quiera actualizar los archivos de Ayuda existentes en el portal de FIM con contenido de sus recursos personalizados. Los archivos de Ayuda del portal de FIM tienen el formato .htm y se pueden editar manualmente.

>[!IMPORTANT]
Para más información sobre la creación de atributos personalizados, consulte Introduction to Custom Resource and Attribute Management (Introducción a la administración de recursos y atributos personalizados) en la documentación de FIM 2010.

>[!IMPORTANT]
En esta sección no se proporciona información sobre los conceptos básicos de formato o edición de HTML. Para modificar los archivos de Ayuda, debe estar familiarizado con la edición de HTML.


**Ubicación de los archivos de Ayuda**: todos los archivos de Ayuda del portal de Microsoft Identity Management 2016 SP1 se encuentran en la siguiente carpeta en el servidor de servicio de MIM:

  `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html`

**Cómo encontrar el archivo de Ayuda adecuado**: todos los archivos de Ayuda del portal de FIM se denominan con un identificador único global (GUID). Para localizar el archivo correcto para el recurso personalizado:

1.  En el portal de FIM, abra el archivo de Ayuda en la página del portal que quiera personalizar.

2.  Haga clic con el botón derecho en el archivo de Ayuda y, a continuación, haga clic en **Propiedades**.

3.  Resalte y copie el archivo `<GUID\>.htm` en el campo "Dirección URL".

4.  Vaya a la carpeta donde se almacenan los archivos de Ayuda y busque el archivo.

**Adición de contenido para un nuevo atributo**: para agregar contenido descriptivo para un nuevo atributo dentro de un elemento Grouping existente (pestaña):

1.  Identifique y busque el archivo de Ayuda adecuado.

2.  Abra el archivo con un editor de HTML.

3.  Busque dónde desea agregar el contenido. Normalmente, esto está un párrafo adicional, por ejemplo:

`<p xmlns="">A new paragraph with customized information.</p>`

También puede ser un elemento que se inserta en una lista existente, por ejemplo:
```
<li class="unordered"><b>First Name</b> – The first name of the User.<br>

<li class="unordered"><b>Last Name</b> - The last name of the User.<br>

<li class="unordered"><b>Added a new line</b><br>
```

**Adición de contenido para un nuevo elemento Grouping**: la mayoría de las páginas del portal de FIM tiene varios elementos Grouping (o pestañas), y los archivos de Ayuda complementarios tienen secciones marcadas relacionadas con cada uno de estos elementos. Los marcadores en el código HTML se especifican en las secciones. Por ejemplo, este es el código HTML de la pestaña Información laboral en el archivo de Ayuda para la página Crear usuario del portal de FIM:

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

El elemento Grouping **WorkInfo** hace referencia a él en el archivo XML de datos de configuración para el RCDC **Configuración para crear un usuario**. Tenga en cuenta que el nombre de archivo `\<GUID\>.htm` y el marcador se especifican en el parámetro my:Link:

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

**Ejemplos de controles simples**

En la siguiente figura se muestra un ejemplo de diferentes controles sencillos de cuadro de texto en distintos modos.

Ejemplo:

![](media/rcd-configuration-xml-reference/image016.gif)

El segmento de código siguiente crea el primer control de cuadro de texto, que usa texto explícito en todos los atributos y propiedades:

```
<!-- Sample for a simple control to use explicit information. (with hints)-->
<my:Control my:Name="ExplicitControl" my:TypeName="UocTextBox" my:Caption="Explicit Control" my:Description="This is explicit description." my:Hint="This is a Hint (enter any text).">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
          <my:Property my:Name="Text" my:Value="Enter Information Here"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use explicit information.-->

```


El segmento de código siguiente crea el segundo control de cuadro de texto, que usa la técnica de enlace dinámico para vincular el control con un origen de datos diferente:

```
<!-- Sample for a simple control to use stored data information.-->
<my:Control my:Name="DynamicControl" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=DisplayName, Mode=OneWay}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required, Mode=OneWay}"/>
          <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=DisplayName.StringRegex, Mode=OneWay}"/>
          <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use stored data information.-->
```


El segmento de código siguiente crea el tercer control de cuadro de texto y etiqueta expandida:

```
<!-- Sample for a simple expanded control with caption control.-->
<my:Control my:Name="SampleExpandLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is an expanded control."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ExpandedControl" my:TypeName="UocTextBox"
          my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="Columns" my:Value="40"/>
          <my:Property my:Name="Text" my:Value="Expanded control (enter text)"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple expanded control.-->
```

El segmento de código siguiente crea el cuarto control de cuadro de texto deshabilitado.
Aunque este control no muestra una diferencia visible entre el estado deshabilitado y el estado habilitado, el usuario ya no puede escribir datos en el cuadro de texto.

```
<!-- Sample for a simple disabled control.-->
<my:Control my:Name="DisabledControl" my:TypeName="UocTextBox" my:Caption="Disabled Control" my:Description="This is disabled simple control." my:Enabled="false">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="MaxLength" my:Value="128"/>
          <my:Property my:Name="Text" my:Value="Disabled control"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple disabled control.-->
```
## <a name="individual-controls"></a>Controles individuales

### <a name="uocbutton"></a>UocButton

**Nombre**: UocButton

**Descripción**: se trata de un control de botón simple que puede usar para desencadenar determinadas acciones. Sin embargo, dado que no puede especificar su propio controlador, el uso de este control es limitado.

**Propiedades**:

1.  **Todas las propiedades comunes**: para más información, consulte la sección Propiedades comunes de este documento.

2.  **Text**: esta propiedad especifica el texto que aparece en el botón. Este es un atributo de tipo de cadena opcional. El texto toma un valor de cadena explícito.

Evento:

   • **OnButtonClicked**: el evento se emite cuando se hace clic en el botón.

Ejemplo:

![](media/rcd-configuration-xml-reference/image017.png)


El siguiente segmento XML produce el botón simple:

```
<!--Sample enabled simple button control-->
<my:Control my:Name="ButtonControl" my:TypeName="UocButton" my:Caption="SampleButton" my:Description="This is a simple button."
my:Hint="Click the button">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="Text" my:Value="Click Me"/>
     </my:Properties>
</my:Control>
<!--End of sample enabled simple button control -->
```

### <a name="uoccaptioncontrol"></a>UocCaptionControl

**Nombre**: UocCaptionControl

**Descripción**: este control se usa para mostrar el título de una página de RCDC. Este control está diseñado para usarse únicamente como un control individual en una agrupación de encabezado.
Su uso en otro contexto, puede provocar problemas de representación o errores del portal.

**Modo**: solo lectura (unidireccional)

**Propiedades:**

1.  **Todas las propiedades comunes:** para más información, consulte la sección Propiedades comunes de este documento.

2.  **MaxHeight:** esta propiedad especifica el alto máximo del icono en la sección de título. Esta propiedad es opcional. Esta propiedad toma un valor entero en píxeles. El valor predeterminado es "32" píxeles.

Ejemplo:

![](media/rcd-configuration-xml-reference/image020.jpg)

El segmento de código siguiente genera el **título de encabezado**:
```
<!--Sample header caption control-->
<my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Header Caption" my:Description="Description Starts here.">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="32"/>
          <my:Property my:Name="MaxWidth" my:Value="32"/>
     </my:Properties>
</my:Control>
<!--End of sample header caption control-->

```


Este segmento de código genera el **título de contenido explícito:**

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

El segmento de código siguiente genera el título dinámico del **nombre para mostrar**:
```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```

### <a name="uoccheckbox"></a>UocCheckBox

**Nombre**: UocCheckBox

Descripción: se trata de un control de casilla de verificación simple. Se recomienda que el usuario enlace este control a datos de tipo booleano. Este control se puede usar como control de solo lectura o como control actualizable, según los datos a los que está enlazado.

>[!NOTE]
En esta versión, al usar el control de casilla en modo de edición para mostrar un atributo booleano, si el atributo no tiene un valor previamente asignado, el control de recursos agrega un valor de **false** al atributo cuando se hace clic en **Aceptar** en modo de edición. La solución es crear siempre un atributo booleano que considere que la no existencia es lo mismo que **false**, o usar otros controles como botones de radio para atributos booleanos.

**Propiedades**:

1.  **Todas las propiedades comunes:** para más información, consulte la sección Propiedades comunes de este documento.

2.  **DefaultValue**: esta es una de tipo booleano opcional. El valor predeterminado se establece en false. Este campo especifica el comportamiento predeterminado de una casilla.
    Esta especificación puede ser explícita.

3.  **Checked**: esta es una propiedad de tipo boolenano opcional. El valor predeterminado se establece en false. Este valor sobrescribe la propiedad DefaultValue cuando está presente junto con DefaultValue. Este campo especifica el comportamiento de una casilla. Al igual que DefaultValue, se puede especificar explícitamente o enlazarse a datos desde el servidor.

4.  **Text**: este es un atributo de tipo cadena opcional. El texto se muestra a la derecha de la casilla. Puede usar esta propiedad para especificar texto que proporcione más información al usuario final.

**Eventos**:

   • CheckedChanged: cuando la casilla cambia su estado, se emite este evento.

En el ejemplo siguiente, se crea un enlace personalizado entre el tipo de recurso personalizado y el atributo **IsConfigurationType**. El código XML se usa en la RCDC de un tipo de recurso personalizado.

Ejemplo:

![](media/rcd-configuration-xml-reference/image022.png)

El siguiente código de segmento genera una **casilla dinámica**, tal y como se muestra en la figura anterior. Este tipo de enlace suele ser más versátil y útil que una casilla explícita. El atributo debe pertenecer al tipo de recurso actual.

```
<!--Sample dynamic check box-->
<my:Control my:Name="SampleDynamicCheckBox" my:TypeName="UocCheckBox" my:Caption="Dynamic Check Box" my:Description="This is a dynamic check box. It saves to data source." my:RightsLevel="{Binding Source=rights, Path=IsConfigurationType}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="{Binding Source=schema, Path=IsConfigurationType.DisplayName, Mode=OneWay}"/>
          <my:Property my:Name="Checked" my:Value="{Binding Source=object, Path=IsConfigurationType, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample dynamic check box -->
```


### <a name="uoccommonmultivaluecontrol"></a>UocCommonMultiValueControl

**Descripción**: se trata de un control de cuadro de texto multilínea que admite formato especial de cadena. Cada valor entre las entradas multivalor está separado de los demás por un punto y coma; o un salto de línea en el cuadro de texto. Se recomienda enlazar este control a datos de tipo entero y cadena cortos multivalor. Este control es compatible con los modos de solo lectura y actualizable.

**Propiedades**:

1.  **Todas las propiedades comunes:** para más información, consulte la sección Propiedades comunes de este documento.

2.  **DataType**: este es un atributo de tipo cadena obligatorio. Se puede especificar como **String, Integer** o **DateTime** explícitamente. También puede enlazar el atributo a la propiedad **DataType** del atributo de esquema. Un tipo de referencia multivalor debe controlarse mediante **UOCListView** o **UOCIdentityPicker**. Los valores booleanos multivalor no son un tipo de dato admitido.

3.  **Rows**: este es un atributo de tipo entero opcional. Puede definir el alto del cuadro en número de caracteres. El valor predeterminado se establece en 1.

4.  **Columns**: este es un atributo de tipo entero opcional. Puede definir el ancho del cuadro en número de caracteres. El valor predeterminado se establece en 1.
    20.

5.  **Value**: este es un atributo de tipo cadena opcional. Solo puede enlazar este atributo a un origen de datos.

Eventos:

   • **ValueListChanged**: este evento se desencadena cuando cambia el valor actual del control.

En el ejemplo siguiente, se crea un atributo de cadena multivalor llamado **AMultiValueString** y se enlaza al tipo de recurso personalizado. Este ejemplo funciona solo después de que se crea este enlace.

**Ejemplo:**

![](media/rcd-configuration-xml-reference/image024.jpg)

El segmento de código siguiente genera la ilustración anterior:

```
<!--Sample multivalue control-->
<my:Control my:Name="SampleDynamicMultiValueControl" my:TypeName="UocCommonMultiValueControl" my:Caption="{Binding Source=schema, Path=AMultiValueString.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=AMultiValueString.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=AMultiValueString}">
     <my:Properties>
          <my:Property my:Name="Rows" my:Value="6"/>
          <my:Property my:Name="Columns" my:Value="60"/>
          <my:Property my:Name="DataType" my:Value="String"/>
          <!--not supported for above property my:Value={Binding Source=schema, Path=AMultiValueString.DataType, Mode=OneWay}"/>-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AMultiValueString, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample multivalue control -->
```



### <a name="uocdatetimecontrol"></a>UocDateTimeControl


**Nombre**: UocDateTimeControl

**Descripción**: es similar a un control de cuadro de texto, pero **Description** solo acepta un formato determinado. En el modo de solo lectura, aparece como una etiqueta. Para conocer el formato de la cadena de entrada que se admite, consulte la propiedad **DateTimeFormat** en esta sección.

**Propiedades**:

1.  **Todas las propiedades comunes:** para más información, consulte la sección Propiedades comunes de este documento.

2.  **DateTimeFormat**: este es un atributo de tipo de cadena opcional. Los formatos admitidos son DateTime o DateOnly. El valor predeterminado se establece en el formato DateTime.
    a. Formato DateTime: el formato de este atributo es mm/dd/aaaa hh:mm:ss a.m.

      <[!NOTE]
      Se admite tanto el formato **DateTime** como **DateOnly**, con independencia del usuario que especifica la diferencia.
3.  **Value**: este es un atributo de tipo cadena opcional. Este atributo se enlaza a un origen de datos de recurso. El valor de este atributo tiene que ajustarse al formato de fecha y hora correcto.

Eventos:

   • **DateTimeChanged**: cuando el valor de fecha y hora cambia, se produce el evento.

Ejemplo:

![](media/rcd-configuration-xml-reference/image027.jpg)

El segmento de código siguiente genera el primer control **DateTime**.

```
<!--Sample explicit DateTime control-->
<my:Control my:Name="SampleExplicitDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="Explicit Date Time Control" my:Description="The data shown here is explicit and in date time format.">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateTime"/>
          <my:Property my:Name="Value" my:Value="11/11/2008 00:00:00"/>
     </my:Properties>
</my:Control>
<!--End of sample explicit DateTime control -->
```


El segmento de código siguiente genera el segundo control **DateTime**. Si ha usado el código de ejemplo de la sección Orígenes de datos, el atributo **ExpirationTime** se enlaza a todos los tipos de recursos. Por lo tanto, puede usarlo con el código siguiente:

```
<!--Sample dynamic DateTime control-->
<my:Control my:Name="SampleDynamicDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="{Binding Source=schema, Path=ExpirationTime.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ExpirationTime.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ExpirationTime}">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateOnly"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExpirationTime, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic explicit DateTime control -->
```



### <a name="uocdropdownlist"></a>UocDropDownList

**Nombre**: UocDropDownList

Descripción: este es un control simple de cuadro desplegable. Este control se usa normalmente cuando desea seleccionar opciones de un conjunto definido de opciones. Los tipos de datos de cadena, entero, fecha y hora y booleano son buenos candidatos para este control.

**Propiedades**:

1.  **Todas las propiedades comunes:** para más información, consulte la sección Propiedades comunes de este documento.

2.  **ValuePath**: la propiedad para obtener el atributo Value de ItemSource. Cuando se especifica ItemSource como personalizado, la ruta de acceso al valor se establece en Value. Se enlaza al campo Value del elemento Option que se define más adelante en este documento.

3.  **CaptionPath**: la propiedad para obtener el atributo Value de ItemSource. Cuando se especifica ItemSource como personalizado, la ruta de acceso al valor se establece en Caption. Se enlaza al campo Caption del elemento Option que se define más adelante en este documento.

4.  **HintPath**: la propiedad para obtener el atributo Value de ItemSource. Cuando se especifica ItemSource como personalizado, la ruta de acceso al valor se establece en Hint. Se enlaza al campo Hint del elemento Option que se define más adelante en este documento.

5.  **ItemSource**: una colección de ListControlItems que define las opciones de la lista. El usuario puede establecerlo explícitamente en Custom y usar el elemento Option para especificar el valor de cadena.

6.  **SelectedValue**: el valor que está seleccionado actualmente. Esta es una propiedad de tipo cadena obligatoria. Esta propiedad se enlaza a datos de cadena desde el origen de datos.

Eventos:

  • SelectedIndexChanged: el evento se produce cuando cambia la selección en el cuadro desplegable.

Opciones

Para conocer la estructura de un elemento Option, consulte la sección "Opción" en este documento.

1.  **Value**: el valor de un único elemento Option se puede establecer en cualquier cadena que sea la entrada válida del origen de datos al que está enlazado el control.

2.  **Caption**: el título puede ser cualquier valor de cadena.

3.  **Hint**: la sugerencia puede ser cualquier valor de cadena.

Ejemplo:

![](media/rcd-configuration-xml-reference/image030.jpg)


![](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
Para que el ejemplo funcione, debe enlazar un atributo de tipo de cadena existente, **Scope** con el tipo de recurso personalizado al que se aplica el RCDC.


Este segmento de código genera la lista desplegable:

```
<!--Sample for drop-down list control-->
<my:Control my:Name="Scope" my:TypeName="UocDropDownList" my:Caption="{Binding Source=schema, Path=Scope.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Scope}">
     <my:Options>
          <my:Option my:Value="DomainLocal" my:Caption="Domain Local" my:Hint="to secure a local resource (i.e. a file share on your computer)" />
          <my:Option my:Value="Global" my:Caption="Global" my:Hint="to secure resources across your team or division" />
          <my:Option my:Value="Universal" my:Caption="Universal" my:Hint="to use this group across your organization" />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Scope.Required" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="ItemSource" my:Value="Custom" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=Scope, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for drop-down list control-->
```


### <a name="uocfiledownload"></a>UocFileDownload

Nombre: UocFileDownload

Descripción: este control contiene un hipervínculo. Cuando se hace clic en el hipervínculo, aparece una página Guardar archivo de Windows. El usuario puede guardar el archivo en su unidad local.
También se admite la opción Abrir si Internet Explorer puede representar el formato de archivo. Los tipos de datos recomendados con los que se puede usar este control son cadena con formato (XML) y binarios.

>[!NOTE]
En esta versión de Microsoft Identity Manager 2016 SP1, el usuario debe cerrar la ventana de Internet Explorer en la que abrió el archivo y luego actualizar la página. Después de actualizar la ventana de Internet Explorer, el usuario puede comenzar la descarga para guardar o abrir de nuevo el mismo archivo en la ventana original.


Propiedades:

1.  **Todas las propiedades comunes:** para más información, consulte la sección Propiedades comunes de este documento.

2.  **Text**: este es un atributo de tipo cadena opcional que define el texto de hipervínculo. El usuario puede especificar una cadena explícita para esta propiedad.

3.  **Value**: este es un atributo necesario. Especifica el enlace de atributo en el servidor cuyo contenido se va a descargar.

4.  **PromptedFileName**: este es un atributo de tipo cadena opcional. Este es el nombre de archivo que se sugiere al usuario al guardar el archivo descargado.

5.  **ContentType**: este es un atributo de tipo cadena obligatorio. Este es el tipo de archivo en el que se guardan los datos. Las dos opciones de cadena admitidas son texto o binario. Si es texto, el valor devuelto se considera una cadena larga.
    Si es binario, el valor devuelto se considera un byte[]. Si se selecciona texto, el usuario puede agregar opcionalmente un sufijo para especificar el tipo de formato en el que se encuentra el texto. Por ejemplo, texto/xml es válido.


>[!NOTE]
Cuando el valor que está enlazado a este control está vacío, el control carece del hipervínculo que se usará para desencadenar la acción de descarga. Esto se debe a que no hay nada que descargar.


**Eventos**:

No hay ningún evento para este control.

**Ejemplo**:

![](media/rcd-configuration-xml-reference/image035.png)


>[!NOTE]
Antes de cargar este archivo de ejemplo, el usuario debe crear un enlace entre un tipo de recurso personalizado y el atributo ConfigurationData existente.


El segmento de código siguiente genera el control de descarga de archivo de la ilustración anterior:

```
<!--Sample dynamic download control-->
<my:Control my:Name="SampleDynamicFileDownloadControl" my:TypeName="UocFileDownload" my:Caption="{Binding Source=schema, Path=ConfigurationData.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ConfigurationData.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ConfigurationData}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="Download Dummy xml"/>
          <my:Property my:Name="PromptedFileName" my:Value="DummyXML.xml"/>
          <my:Property my:Name="ContentType" my:Value="text/xml"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ConfigurationData}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic download control -->
```


### <a name="uocfileupload"></a>UocFileUpload

**Nombre**: UocFileUpload

**Descripción**: este control contiene un cuadro de texto que muestra la ubicación del archivo local que se va a cargar, un botón para examinar el archivo y un botón de carga. Cuando el usuario final hace clic en un botón Examinar, aparece una ventana Abrir archivo de Windows. El usuario final puede seleccionar un archivo en su unidad local para cargar. Cuando se selecciona el archivo, la ubicación de este se muestra en el cuadro de texto. Cuando se hace clic en el botón Cargar, el archivo se carga en el origen de datos local del cliente. El contenido del archivo no se envía aún al servidor. Los tipos de datos recomendados con los que se puede usar este control son cadena con formato (XML) o binarios.

>[!NOTE]
No hay ninguna indicación del estado o progreso de la carga. Cuando se carga el archivo en el origen de datos local, se borra el cuadro de texto.


Propiedades:

1.  Todas las propiedades comunes: para más información, consulte la sección Propiedades comunes de este documento.

2.  Value: este es un atributo necesario. Especifica el enlace de atributo de esquema en el servidor en el que se cargan los datos.

3.  ContentType: este es un atributo de tipo de cadena opcional. Este es el tipo de datos con el que el archivo se guarda en el servidor. Se puede establecer en texto o binario. Cuando la propiedad está ausente, el valor predeterminado es binario.

4.  MaxFileSize: este es un atributo de tipo de cadena opcional. MaxFileSize define lo grande que puede ser el tamaño del archivo cargado. De forma predeterminada, si la propiedad está ausente, el tamaño máximo es 1 megabyte (MB).

5.  PromptedForNoValue: este es un atributo de tipo de cadena opcional. Define el texto que le aparece al usuario cuando no se está cargando un archivo.

Eventos:

   • FileUploaded: este evento se emite cuando el archivo se ha cargado correctamente.

Ejemplo:

![](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
Para que el siguiente código de ejemplo funcione, debe crear un nuevo atributo de tipo binario denominado ABinaryAttribute y luego crear un nuevo enlace entre un tipo de recurso personalizado y este atributo.


El segmento de código siguiente genera el control de carga de la ilustración anterior:
```
<!--Sample dynamic upload control-->
<my:Control my:Name="SampleDynamicFileUploadControl" my:TypeName="UocFileUpload" my:Caption="{Binding Source=schema, Path=ABinaryAttribute.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ABinaryAttribute.Description, Mode=OneWay}” my:RightsLevel="{Binding Source=rights, Path=ABinaryAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABinaryAttribute.Required}"/>
          <my:Property my:Name="ContentType" my:Value="Binary"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ABinaryAttribute, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic upload control -->
```

### <a name="uocfilterbuilder"></a>UocFilterBuilder

Nombre: UocFilterBuilder

Descripción: este es un control complejo que permite al usuario representar una expresión XPath de MIM 2016. Algunas expresiones XPath no se admiten. Para más información sobre cómo usar el generador de filtros, consulte la Ayuda correspondiente.

Propiedades:

1.  Todas las propiedades comunes: para más información, consulte la sección Propiedades comunes de este documento.

2.  PermittedObjectTypes: define una lista de tipos de recursos que se mostrarán en la instrucción Select de un generador de filtros. Para más información sobre cómo usar el generador de filtros, consulte la Ayuda correspondiente. La cadena tiene el formato de ResourceTypeA, ResourceTypeB, donde cada tipo de recurso está separado por una coma (,).

3.  Valor: este es el valor con el que se representa el generador de filtros.
    Solo se admite un enlace con datos de tipo cadena que contenga una expresión XPath. El atributo Filter es un atributo recomendado para enlazar este control.

4.  PreviewButtonVisible: esta es una propiedad de tipo booleano opcional. Cuando esta propiedad está establecida en false, el usuario no puede ver un botón de vista previa. El valor predeterminado se establece en true. Este botón se puede usar en combinación con un control de vista de lista para obtener una vista previa de los resultados de una expresión XPath.

5.  ExcludeGroupMembership: esta es una propiedad booleana. Cuando esta propiedad se establece en true, no se puede crear un filtro que use un \<atributo de referencia\> (por ejemplo, ResourceID) que sea miembro del \<objeto Group\>. En otras palabras, cuando esta propiedad se establece en true, no se puede crear un filtro que use el directorio de pertenencia al grupo.

6.  PreviewButtonCaption: esta es una cadena opcional. Cuando PreviewButtonVisible está establecido en true, puede usar esta propiedad para proporcionar al botón un texto personalizado. El texto aparece en el botón de vista previa.

Eventos:

   • OnFilterChanged: este evento se desencadena cuando cambia el contenido del generador de filtros.

Ejemplo:

![](media/rcd-configuration-xml-reference/image044.png)

>[!NOTE]
Antes de usar este código de ejemplo, cree un nuevo enlace entre un atributo Filter existente y un tipo de recurso personalizado.


El siguiente código de ejemplo incluye un control UOCLabel, un generador de filtros simple con PermittedObjectTypes y una vista de lista de vista previa. La propiedad ListFilter de vista de lista y la propiedad Value del generador de filtros deben apuntar al mismo atributo de origen de datos para vincular las dos.

```
<!--Sample filter builder with preview list-->
<my:Control my:Name="ComplexFilterBuilderLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is a Filter Builder with preview."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ComplexFilterBuilder" my:TypeName="UocFilterBuilder" my:RightsLevel="{Binding Source=rights, Path=Filter}" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="PermittedObjectTypes" my:Value="Person,Group" />
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Filter, Mode=TwoWay}" />
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Filter.Required, Mode=OneWay}" />
     </my:Properties>
</my:Control>
<my:Control my:Name="FilterBuilderwithpreview" my:TypeName="UocListView" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName,ObjectType,AccountName" />
          <my:Property my:Name="EmptyResultText" my:Value="There is no members according to the filter definition." />
       <my:Property my:Name="PageSize" my:Value="10" />
       <my:Property my:Name="ShowTitleBar" my:Value="false" />
       <my:Property my:Name="ShowActionBar" my:Value="false" />
       <my:Property my:Name="ShowPreview" my:Value="false" />
       <my:Property my:Name="ShowSearchControl" my:Value="false" />
       <my:Property my:Name="EnableSelection" my:Value="false" />
       <my:Property my:Name="SingleSelection" my:Value="false" />
       <my:Property my:Name="ItemClickBehavior" my:Value=" ModelessDialog "/>
       <my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}" />
     </my:Properties>
</my:Control>
<!--end of sample filter builder with preview-->
```


<a name="uochtmlsummary"></a>UocHtmlSummary
--------------

Nombre: UocHtmlSummary

Descripción: puede usar este control para definir una página de resumen en una página de RCDC.
Esta página de resumen aparece después de que el usuario final envía una solicitud. Este control solo puede usarse en una agrupación de resumen y debe ser el único control. Se recomienda encarecidamente que use el código de ejemplo que se proporciona.

>[!NOTE]
Este control no ha sido probado exhaustivamente.


Propiedades:

1.  Todas las propiedades comunes: para más información sobre esta propiedad, consulte la sección Propiedades comunes de este documento.

2.  ModificationsXml: esta propiedad debe tener el formato {origen de enlace=delta, ruta de acceso=DeltaXml}, donde delta se define en el encabezado de configuración ObjectDataSource.

3.  TransformXsl: esta propiedad suele tener el formato {Origen de enlace=summaryTransformXsl, ruta de acceso=/}, donde summaryTransformXsl se define en el encabezado de configuración XmlDataSource.

Para ver un ejemplo de este control, consulte "Agrupación de resumen" en la sección Agrupación anterior de este documento.

### <a name="uochyperlink"></a>UocHyperLink

Nombre: UocHyperLink

Descripción: este es un control simple de hipervínculo. Puede usar este control para mostrar la información como un hipervínculo.

Propiedades:

1.  Todas las propiedades comunes: para más información, consulte la sección Propiedades comunes de este documento.

2.  ObjectReference: esta es una propiedad de tipo referencia opcional. Si el GUID que se define en esta propiedad hace referencia a un recurso válido, el hipervínculo proporciona al usuario final una forma para acceder al recurso. Es mutuamente excluyente con la propiedad NavigateUrl (a continuación).

3.  Text: esta es un propiedad de tipo cadena opcional. Se usa para definir el texto que aparece como hipervínculo.

4.  NavigateUrl: esta es una propiedad de tipo cadena opcional. Se usa para definir la dirección URL de ruta de acceso completa a la que vincula el hipervínculo. Es mutuamente excluyente con la propiedad ObjectReference (anterior).

Ejemplo:

![](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
Necesita un GUID válido de un recurso al que vincularlo. En este caso, el segundo hipervínculo se genera con un GUID válido. El primero puede ser cualquier sitio web.


El segmento de código siguiente genera un hipervínculo de redirección:

```
<!--Sample for a hyperlink that redirects page.-->
<my:Control my:Name="RedirectHyperlink" my:TypeName="UocHyperLink" my:Caption="Redirect Hyperlink" my:Description="This is a hyperlink that takes you to other pages.">
     <my:Properties>
          <my:Property my:Name="NavigateUrl" my:Value="http://www.microsoft.com"/>
          <my:Property my:Name="Text" my:Value="Microsoft Home Page"/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that redirect page-->

```

El segmento de código siguiente genera un hipervínculo que hace referencia a un recurso. La referencia explícita puede reemplazarse por la expresión {Origen de enlace=object, ruta de acceso=Creator} para vincularlo a un origen de datos. Puede que este enfoque solo sea válido cuando existe el administrador del recurso y tiene un valor de tipo referencia.

```
<!--Sample for a hyperlink that reference object-->
<my:Control my:Name="ReferenceHyperlink" my:TypeName="UocHyperLink" my:Caption="Reference Hyperlink" my:Description="This is a hyperlink gives you an object view of the reference object">
     <my:Properties>
          <my:Property my:Name="ObjectReference" my:Value="e4e048b1-9e43-415e-806c-cf44c429c34c"/>
          <my:Property my:Name="Text" my:Value="View a group in FIM 2010."/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that reference object-->
```

### <a name="uocidentitypicker"></a>UocIdentityPicker

Nombre: UocIdentityPicker

Descripción: este control consta de un cuadro Resolve opcional y una ventana de exploración. El cuadro Resolve opcional consta de un cuadro de texto opcional para escribir la identidad, un botón Resolve para resolver la identidad y un botón Browse para mostrar una ventana emergente de exploración. La ventana de exploración permite al usuario seleccionar identidades mediante un control de vista de lista. La identidad seleccionada en la ventana de exploración se refleja en el cuadro Resolve.

Propiedades:

1.  Todas las propiedades comunes: para más información sobre esta propiedad, consulte la sección Propiedades comunes de este documento.

2.  UsageKeywords: esta es una propiedad de cadena opcional. Puede definir una lista de ámbitos de búsqueda que se usará en el selector de recursos; para ello, proporcione una lista de las palabras clave de uso que se admiten en la estructura SearchScopeConfiguration, donde cada palabra clave esté separada por un (').

3.  Filter: esta es una propiedad de cadena opcional. El usuario proporciona una expresión XPath para establecer el ámbito del selector de recursos a fin de mostrar solo los elementos que caben dentro de un ámbito definido. Esta propiedad es mutuamente excluyente con la propiedad UsageKeywords (anteriormente). Cuando se aplica el ámbito de búsqueda, esta propiedad no tiene ningún efecto.

4.  ResultObjectType: esta es una propiedad de cadena opcional. El tipo de recurso se usa para representar recursos en la lista del cuadro de diálogo emergente. Se usa con el filtro para ayudar al selector de identidad a identificar qué tipo de recurso devuelve el filtro y representar los datos en consecuencia. Esta propiedad es mutuamente excluyente con la propiedad UsageKeywords (consulte anteriormente). Cuando se aplica el ámbito de búsqueda, esto no tiene ningún efecto. La cadena que se acepta para esta propiedad es cualquier nombre de tipo recurso válido y único, por ejemplo, Person.
    Cuando se espera que el filtro devuelva varios tipos de recursos, se usa Resource.

5.  PreviewTitle: este es el título de vista previa que se usa en una vista de lista. Para más información sobre esta propiedad, consulte la sección UocListView.

6.  ListViewTitle: esta es una propiedad de cadena opcional. Puede usar esta propiedad para definir el texto que aparece encima de la vista de lista como título.

7.  Value: esta es una propiedad de cadena opcional. Se recomienda enlazarla a un atributo de esquema para conectar el valor con un origen de datos.

8.  Mode: esta es una propiedad de cadena opcional. Úsela para definir si el selector de identidad puede seleccionar un valor o si se pueden seleccionar varias identidades. Los valores permitidos son SingleResult y MultipleResult. De forma predeterminada, se establece en SingleResult.

9.  ObjectTypes: esta es una propiedad de tipo de cadena opcional. Puede definir una lista de tipos de recursos con los que el usuario final puede resolver las entradas en el cuadro Resolve del selector de identidad. La lista está formada por una lista de nombres de tipos de recursos separados por una coma (,).

10. AttributesToSearch: esta es propiedad de tipo cadena opcional. Puede definir una lista de atributos para resolver el elemento en el selector de identidad, donde la lista sea una lista de atributos de esquema, separados por una coma (,). Por ejemplo, si AttributesToSearch está establecido en DisplayName, Alias, significa que el usuario puede buscar en los elementos con DisplayName = \<valor de búsqueda\> o Alias=\<valor de búsqueda\>. Se recomienda que los nombres de atributo que se especifican aquí sean atributos válidos en los tipos de recursos de destino del origen de datos que está situado en Value. Los tipos de recursos de destino pueden encontrarse en el campo ObjectTypes. Todos los atributos deben ser válidos en cualquier tipo de recurso dado que se cite en el campo ObjectTypes.

11. ColumnsToDisplay: esta es una propiedad de tipo de cadena opcional. El usuario proporciona una lista de nombres de atributo de esquema, separados por una coma (,). Los atributos que se definen aquí constituyen la columna de la vista de lista en el selector de identidad.

12. Rows: esta es una propiedad de número entero opcional. Funciona únicamente cuando el modo está establecido en MultipleResult. Use esta propiedad para establecer el alto del cuadro de texto Resolve en un tamaño determinado en unidades de caracteres.

13. MainSearchScreenText: esta es una propiedad de tipo de cadena opcional. Es el texto personalizado que aparece mientras se ejecuta la búsqueda en la ventana de exploración.

Eventos:

 • SelectedObjectChanged: este evento se emite cuando el usuario cambia los recursos seleccionados.

Ejemplo:

![](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
Para que este ejemplo funcione, debe crear un nuevo enlace entre el atributo Manager y cualquier tipo de recurso personalizado al que se aplica este XML.


El segmento de código siguiente genera un selector de identidad en modo único mediante las propiedades Filter y ResultObjectType como parte del RCDC:

```
<!--Sample for a single-selection identity picker using Filter and Result Object Type-->
<my:Control my:Name="SingleSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A Single Selection Identity Picker" my:Description="The user is allowed to select only one entry here." my:RightsLevel="{Binding Source=rights, Path=Manager}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Manager.Required}"/>
          <my:Property my:Name="Mode" my:Value="SingleResult" />
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--single valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Manager , Mode=TwoWay}" />
          <!--Scoping the list explicitly to All Persons name contains letter "e"-->
          <my:Property my:Name="Filter" my:Value="/Person[contains(JobTitle, 'Manager')]"/>
          <!--Result object type specify the type is Person-->
          <my:Property my:Name="ResultObjectType" my:Value="Person"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select only one entry" />
          <my:Property my:Name="PreviewTitle" my:Value="Entry selected:" />
     </my:Properties>
</my:Control>
<!--End of sample for a single-selection identity picker.-->
```



Ejemplo:

![](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
Para que este código de ejemplo funcione, debe enlazar el atributo ExplicitMember (un atributo de referencia multivalor) al tipo de recurso personalizado. También debe crear ámbitos de búsqueda con la propiedad UsageKeyword establecida en Person y Group.


El segmento de código siguiente crea el control de la ilustración anterior:

```
<!--Sample for a multiselection Identity Picker uses Search Scope-->
<my:Control my:Name="multiSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A multi Selection Identity Picker" my:Description="The user is allowed to select more than one entry here" my:RightsLevel="{Binding Source=rights, Path=ExplicitMember}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ExplicitMember.Required}"/>
          <my:Property my:Name="Mode" my:Value="MultipleResult" />
          <my:Property my:Name="Rows" my:Value="10" />
          <!--There are existing search scopes that has key word "Person" and "Group" use both sets of search scopes here.-->
          <my:Property my:Name="UsageKeywords" my:Value="Person,Group"/>
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--multi valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExplicitMember , Mode=TwoWay}" />
          <my:Property my:Name="ResultObjectType" my:Value="Resource"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select multiple entries" />
          <my:Property my:Name="PreviewTitle" my:Value="Entries selected" />
     </my:Properties>
</my:Control>
<!--End of sample for a multiselection Identity Picker.-->
```


### <a name="uoclabel"></a>UocLabel

Nombre: UocLabel

Descripción: este es un control simple de etiqueta de texto de solo lectura. Se recomienda que este control se use para mostrar datos de solo lectura.

Propiedades:

1.  Todas las propiedades comunes: para más información sobre esta propiedad, consulte la sección Propiedades comunes de este documento.

2.  Text: este es un atributo de tipo cadena. Para definir esta propiedad, proporcione un valor de cadena explícito o enlácela a un origen de datos. Un enlace de ejemplo que asigna el valor de esta propiedad es {Enlace de datos=object, ruta de acceso=\<nombre de atributo válido\>.

Para ver un ejemplo del control UocLabel, consulte información sobre los controles simples en la sección de ejemplos de controles simples.

### <a name="uoclistview"></a>UocListView

Nombre: UocListView

Descripción: este es un control avanzado de vista de lista. Consta de una vista de lista simple, una búsqueda simple opcional, un control de búsqueda avanzada opcional, un cuadro de vista previa de selección opcional y una barra de botones de acción. La búsqueda simple opcional consta de un ámbito de búsqueda y un cuadro de texto de búsqueda simple. El control de búsqueda avanzada es un generador de filtros. La vista de lista muestra una lista de recursos representada previamente. También puede mostrar resultados de búsqueda procedentes de los controles de búsqueda de este control. La barra de botones de acción define qué acción se puede realizar de acuerdo con la selección en la vista de lista. El cuadro de vista previa de selección muestra los elementos seleccionados en la vista de lista.

>[!IMPORTANT]
UocListView no funciona con atributos de referencia de un único valor. Solo se puede usar con atributos de referencia multivalor. Para atributos de referencia de valor único, consulte UocIdentityPicker en este documento.


Propiedades:

1.  Todas las propiedades comunes: para más información sobre esta propiedad, consulte la sección Propiedades comunes de este documento.

2.  SelectedValue: esta es una propiedad de tipo cadena opcional que está enlazada normalmente a un atributo de referencia multivalor que acepta una lista de cadenas con formato GUID.

3.  PageSize: esta es una propiedad de número entero opcional. El usuario puede especificar cuántas entradas cabrán en una página en un control de vista de lista. El valor predeterminado es 10 entradas. Cualquier entero positivo es válido.

4.  UsageKeyword: esta es una propiedad de tipo cadena opcional. El usuario puede especificar una lista de palabras clave que definen qué ámbito de búsqueda se usa en el control de búsqueda de vista de lista. Hay recursos del ámbito de búsqueda en el servidor de FIM 2010. El atributo en una estructura SearchScopeConfiguration, denominada UsageKeyword, se usa para agrupar un conjunto de ámbitos de búsqueda. La vista de lista consume esa lista de palabras clave. Cada palabra clave está separada por una coma (,).
    Este es el atributo UsageKeyword usado en el ámbito de búsqueda correspondiente que quiere mostrar en esta vista de lista. Solo tiene efecto cuando la propiedad ShowSearchControl está establecida en true.

5.  SearchControlAutoPostback: esta es una propiedad booleana opcional. Establezca el valor de esta propiedad en true para realizar autopostback cuando se desencadena una búsqueda. De forma predeterminada, SearchControlAutoPostback se establece en false.

6.  EmptyResultText: esta es una propiedad de tipo cadena opcional. De forma predeterminada, está establecida en Ningún elemento, pero se puede establecer en cualquier valor de cadena. Este texto aparece cuando un resultado de búsqueda está vacío.

7.  ButtonHeight: esta es una propiedad de tipo entero opcional. Establezca el valor de esta propiedad en un valor entero positivo. Esta propiedad define el alto de los botones en la barra de acciones, en píxeles. El valor predeterminado es "32" píxeles.

8.  ButtonWidth: esta es una propiedad de tipo entero opcional. Establezca el valor de esta propiedad en un valor entero positivo. Esta propiedad define el ancho de los botones en la barra de acciones, en píxeles. El valor predeterminado es "32" píxeles.

9.  CaptionImageMaxHeight: esta es una propiedad de tipo entero opcional. Establezca el valor de esta propiedad en un número entero positivo. Esta propiedad define un alto de icono máximo opcional del título. El valor predeterminado es "32" píxeles.

10. CaptionImageMaxWidth: esta es una propiedad de tipo entero opcional. Establezca el valor de esta propiedad en un número entero positivo. Esta propiedad define un ancho de icono máximo opcional de un título. El valor predeterminado es "32" píxeles.

11. CaptionImageUrl: esta es una propiedad de tipo cadena opcional. Esta propiedad define una dirección URL que vincula a una imagen que aparece como imagen de título.

12. PreviewTitle: esta es una propiedad de tipo cadena opcional. Use esta propiedad para definir el texto que aparece en la parte superior del cuadro de vista previa de selección.

13. EnableSelection: esta es una propiedad de tipo booleano opcional. Use esta propiedad para definir si una vista de lista está en modo de selección. Si una vista de lista está en modo de selección, aparece una columna de casillas en la columna izquierda de la vista de lista y un cuadro de vista previa de selección aparece en la parte inferior de la vista de lista. El valor predeterminado de esta propiedad se establece en true.

14. SingleSelection: esta es una propiedad de tipo booleano opcional. Si el modo de selección está activado para la vista de lista, al establecer este valor en true, el usuario final está limitado a seleccionar únicamente un elemento de la lista. De forma predeterminada, el valor de esta propiedad se establece en false. Esto significa que, de forma predeterminada, el usuario final puede seleccionar varios elementos de la lista.

15. RedirectUrl: esta es una propiedad de tipo cadena opcional. Use esta propiedad para especificar una página de redireccionamiento cuando se haga clic en un elemento hipervinculado de la lista. Esta dirección URL puede contener marcadores de posición que se reemplazan por el valor real en tiempo de ejecución. Los marcadores de posición son los siguientes:

     ◦ {0} - objectType

     ◦ {1} - objectID

     ◦ {2} - displayName

16.  ShowTitleBar: esta es una propiedad de tipo booleano opcional. Use esta propiedad para especificar si la barra de título debe estar visible. El valor predeterminado de esta propiedad es false.

17.  ShowActionBar: esta es una propiedad de tipo booleano opcional. Use esta propiedad para especificar si el área de la barra de acción debe estar visible. El valor predeterminado de esta propiedad es true.

18.  ShowPreview: esta es una propiedad de tipo booleano opcional. Use esta propiedad para especificar si el área de vista previa debe estar visible. El valor predeterminado de esta propiedad es true.

19.  ShowSearchControl: esta es una propiedad de tipo booleano opcional. Use esta propiedad para especificar si el control de búsqueda debe estar visible. El valor predeterminado de esta propiedad es true.

20.  ResultObjectType: esta es una propiedad de tipo de cadena opcional. Use esta propiedad para especificar el tipo de objeto esperado de los resultados de búsqueda. El valor predeterminado de esta propiedad es Resource. Si el resultado de búsqueda contiene varios tipos de recursos, este valor debe especificarse como Resource.

21.  ColumnsToDisplay: esta es una propiedad opcional. Use esta propiedad para especificar los atributos que desea que la vista de lista muestre como columnas. El valor predeterminado de esta propiedad es DisplayName, ResourceType. Cada columna se representa mediante el nombre del sistema de un atributo. Cada columna está separada por una coma (,). No es necesario especificar un valor para esta propiedad cuando se usa la vista de lista en modo de selección. En el modo de selección, el valor de columna proviene del atributo SearchScopeColumn del ámbito de búsqueda que está seleccionado actualmente.

22.  ListFilter: esta es una propiedad de tipo cadena opcional. Esta es la expresión Xpath que se usa para representar la vista de lista y solo tiene efecto cuando la propiedad ShowSearchControl está establecida en false. Cuando se especifica este valor, la vista de lista usa este valor de propiedad para las consultas y la vista de lista no está en modo de selección. El filtro se puede enlazar a un atributo de cadena del recurso, de la manera siguiente:<br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>` <br/> <br/>
     o bien, ser una cadena que contiene alguna variable de entorno predefinida, como se indica a continuación: <br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

23.  TargetAttribute: esta es una propiedad obsoleta. Su valor debe ser el nombre del sistema de un atributo multivalor al que se hace referencia. Se recomienda dejar de usar esta propiedad. Por ejemplo, en administración de grupos, en lugar de:

  `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>` <br/>

  Debe ser: `<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>` <br/><br/>
24.  ItemClickBehavior: esta es una propiedad de tipo cadena opcional. Use esta propiedad para especificar si quiere hacer clic en un elemento de la vista de lista para desencadenar un postback del servidor o para mostrar una vista detallada del elemento. Se admiten dos valores de opción: ModelessDialog y Server. El valor predeterminado es ModelessDialog.

25.  SearchOnLoad: esta es una propiedad de tipo booleano opcional que especifica si el control de vista de lista debe consultar en la carga. Esta propiedad solo es aplicable cuando la vista de lista está en modo de selección. El valor predeterminado de esta propiedad es true. Puede desactivarla si espera que el usuario escriba normalmente texto en el cuadro de búsqueda para obtener resultados significativos. En este caso, la vista de lista muestra inicialmente un mensaje para indicar al usuario cómo realizar una búsqueda. El texto se puede personalizar mediante las siguientes propiedades:

26.  MainSearchScreenText: esta propiedad de tipo cadena opcional solo es aplicable cuando SearchOnload se establece en true. Esta propiedad puede usarse para personalizar el texto que aparece en el medio de la vista de lista cuando la vista de lista no busca automáticamente. El valor predeterminado de esta propiedad es encontrar los recursos deseados mediante la búsqueda anterior. Puede especificar un valor para que el texto sea más significativo para su escenario.

27.  SubSearchScreenText: esta propiedad de tipo cadena opcional se usa para personalizar el texto que aparece debajo de MainSearchScreenText. Por lo general, no es necesario especificar un valor para esta propiedad a menos que desee agregar algunas instrucciones adicionales sobre cómo usar la vista de lista.

Para ver ejemplos de cómo usar la vista de lista, junto con el control UocFilterBuilder como lista de vista previa, consulte los ejemplos de UocFilterBuilder anteriormente en este documento. UocListView puede usarse también sin el generador de filtros.

### <a name="uocnumericbox"></a>UocNumericBox

Nombre: UocNumericBox

Descripción: este es un cuadro de texto simple que toma únicamente valores enteros. Este control es compatible con los modos de solo lectura y actualizable.

Propiedades:

1.  Todas las propiedades comunes: para más información sobre esta propiedad, consulte la sección Propiedades comunes de este documento.

2.  MaxValue: esta es una propiedad de tipo entero opcional. Use esta propiedad para definir una validación del lado cliente para el control. El valor que escribe el usuario no puede superar este valor. Puede especificar un número entero explícito o enlazarlo con datos enteros de un origen de datos mediante {Origen de enlace=schema, ruta de acceso=IntegerMaximum}.

3.  MinValue: esta es una propiedad de tipo entero opcional. Use esta propiedad para definir una validación del lado cliente para el control. El valor que especifica el usuario final no puede ser inferior a este valor. Puede especificar un número entero explícito o enlazarlo con datos enteros de un origen de datos mediante {Origen de enlace=schema, ruta de acceso=IntegerMinimum}.

4.  DefaultValue: esta es una propiedad de tipo entero opcional. Use esta propiedad para definir un valor predeterminado para el control si este se usa para crear nuevos datos. Este valor solo puede establecerse explícitamente en un entero estático.

5.  Value: esta es una propiedad de tipo entero opcional. Cuando se enlaza con un tipo de datos entero desde un origen de datos, el valor de ese atributo aparece cuando se carga la página y, a continuación, se guarda en el origen de datos después del envío.

Controlador:

   • TextChanged: este evento se emite cuando cambia el valor actual dentro del control.

Ejemplo:

![](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
El código de ejemplo siguiente genera el primer cuadro numérico. El cuadro numérico no está conectado con un origen de datos o alguna información de esquema.

```
<!--Sample for an explicit Numeric Box-->
<my:Control my:Name="SampleExplicitNumericBox" my:TypeName="UocNumericBox" my:Caption="An Explicit NumericBox" my:Description="This is a dummy numeric box that is not linked with data source.">
     <my:Properties>
          <my:Property my:Name="MinValue" my:Value="1"/>
          <my:Property my:Name="MaxValue" my:Value="100"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
     </my:Properties>
</my:Control>
<!--End of sample for an explicit Numeric Box.-->

```

El código de ejemplo siguiente genera el segundo cuadro numérico.

>[!NOTE]
Para que este ejemplo funcione, primero debe crear un nuevo atributo de tipo entero llamado AnIntegerAttribute y enlazarlo con el tipo de recurso personalizado.

```
<!--Sample for a dynamically rendered numeric box-->
<my:Control my:Name="SampleDynamicNumericBox" my:TypeName="UocNumericBox" my:Caption="{Binding Source=schema, Path=AnIntegerAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=AnIntegerAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=AnIntegerAttribute}">
     <my:Properties>
          <my:Property my:Name="MaxValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMaximum}"/>
          <my:Property my:Name="MinValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMinimum}"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AnIntegerAttribute, Mode=TwoWay}"/>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.Required}"/>
     </my:Properties>
</my:Control>
<!--End of sample for a dynamically numeric box.-->
```


### <a name="uocpicturebox"></a>UocPictureBox

Nombre: UocPictureBox

Descripción: este control se usa para representar datos de imagen de tipo binario. Se recomienda usar este control con datos de tipo binario. La imagen se puede representar mediante una dirección URL de la imagen proporcionada, datos de tipo binario o el origen de atributo que contiene los datos de tipo imagen.

Propiedades:

1.  Todas las propiedades comunes: para más información sobre esta propiedad, consulte la sección Propiedades comunes de este documento.

2.  ImageUrl: esta es una propiedad de tipo cadena opcional. Escriba la dirección URL de la imagen de destino.

3.  MaxHeight: esta es una propiedad de tipo cadena opcional. Define el alto máximo de la imagen que se representará, en píxeles.

4.  MaxWidth: esta es una propiedad de tipo de cadena opcional. Define el ancho máximo de la imagen que se representará, en píxeles.

5.  ImageData: esta es una propiedad de tipo binario. Use esta propiedad para enlazar un origen de datos a la imagen mostrada. El origen de datos enlazado tiene que ser binario.
    También puede usar este campo para establecer explícitamente una imagen proporcionando datos en formato de byte[].

6.  ImageResource: esta es una propiedad de tipo binario opcional.

7.  AlternativeText: esta es una propiedad de tipo cadena opcional. Esta propiedad aparece como texto alternativo cuando no se puede mostrar la imagen.

Ejemplo:

>[!NOTE]
Para usar este ejemplo, debe tener un enlace de datos de imagen existentes al control.


El segmento de código siguiente genera un control de cuadro de imagen que enlaza un origen de datos al control:

```
<!--Sample for a Picture Box control binding with a data source-->
<my:Control my:Name="SamplePictureBoxImageData" my:TypeName="UocPictureBox" my:RightsLevel="{Binding Source=rights, Path=Photo}">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageData" my:Value="{Binding Source=object, Path=Photo}" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

El segmento de código siguiente genera un control de cuadro de imagen que enlaza una imagen de dirección URL al control:

```
<!--Sample for a Picture Box control bind with explicit URL-->
<my:Control my:Name="SamplePictureBoxImageUrl" my:TypeName="UocPictureBox">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageUrl" my:Value="http://www.microsoft.com/dummypicture.jpg" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

### <a name="uocradiobuttonlist"></a>UocRadioButtonList

Nombre: UocRadioButtonList

Descripción: esta es una lista simple de botones de opción. Las opciones son mutuamente excluyentes en esta lista. Este control se recomienda cuando los usuarios tienen cinco o menos opciones para elegir. En caso contrario, se recomienda UOCListView.

Propiedades:

1.  Todas las propiedades comunes: para más información sobre esta propiedad, consulte la sección Propiedades comunes de este documento.

2.  ValuePath: la ruta de acceso al valor se establece en Value. Crea un enlace al campo Value del elemento Option que se define en este documento.

3.  CaptionPath: la ruta de acceso al valor se establece en Caption. Crea un enlace al campo Caption del elemento Option que se define en este documento.

4.  HintPath: la ruta de acceso al valor se establece en Hint. Crea un enlace al campo Hint del elemento Option que se define en este documento.

5.  SelectedValue: el valor que está seleccionado actualmente. Esta es una propiedad de tipo cadena obligatoria. Esta propiedad enlaza a datos de cadena desde el origen de datos.

Eventos:

1.  SelectedIndexChanged

2.  CheckedChanged

Opciones

Solo puede haber dos elementos Option en Options para este control.

1.  Value: el campo Value de un único elemento Option se debe establecer en True o False.

2.  Caption: puede ser cualquier valor de cadena.

3.  Hint: puede ser cualquier valor de cadena.

Ejemplo:

![](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
Para que este ejemplo funcione, debe crear un nuevo atributo booleano, ABooleanAttribute, y enlazarlo al tipo de recurso personalizado.

El segmento de código siguiente crea la lista de botones de opción de la ilustración anterior:

```
<!--Sample for option button list control-->
<my:Control my:Name="SampleRadioButtonList" my:TypeName="UocRadioButtonList" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Options>
          <my:Option my:Value="False" my:Caption="Set Value To False" my:Hint="By selecting this option, you are setting the value of the attribute to false." />
          <my:Option my:Value="True" my:Caption="Set Value To True" my:Hint="By selecting this option, you are setting the value of the attribute to true." />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for option button list control-->
```


### <a name="uocsimpleradiobutton"></a>UocSimpleRadioButton


Nombre: UocSimpleRadioButton

Descripción: este es un control simple de botón de opción. El uso de este control es similar a una casilla simple. Hay dos botones de opción, que se muestran uno al lado de otro con la etiqueta del texto. Se recomienda enlazar el control a los datos de tipo booleano.

Propiedades:

1.  Todas las propiedades comunes: para más información sobre esta propiedad, consulte la sección Propiedades comunes de este documento.

2.  TrueText: esta es una propiedad de tipo cadena opcional. Este es el texto que aparece cuando se selecciona el botón de opción.

3.  FalseText: esta es una propiedad de tipo cadena opcional. Este es el texto que aparece cuando no se selecciona el botón de opción.

4.  SelectedItem: esta es una propiedad de tipo booleano opcional. Este valor indica que está seleccionado el botón de opción. Puede enlazarlo a datos de tipo booleano desde un origen de datos. El valor predeterminado se establece en false.

Eventos:

   • CheckedChanged: cuando el estado del botón de opción cambia de seleccionado a no seleccionado, o al contrario, se emite esta señal.

Ejemplo:

![](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
Para que el ejemplo funcione, debe crear un nuevo atributo booleano, ABooleanAttribute, y enlazarlo a su tipo de recurso personalizado. Los datos de RCDC se aplican al mismo tipo de recurso personalizado.

El segmento de código siguiente genera el botón de opción de la ilustración anterior:

```
<!--Sample for simple option button control-->
<my:Control my:Name="SampleSimpleRadioButton" my:TypeName="UocSimpleRadioButton" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="FalseText" my:Value="False"/>
          <my:Property my:Name="TrueText" my:Value="True"/>
          <my:Property my:Name="SelectedItem" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for simple option button control-->
```


### <a name="uoctextbox"></a>UocTextBox

Nombre: UocTextBox

Descripción: este es un cuadro de texto simple que admite la entrada de tipo cadena. Se recomienda usar este control para crear un enlace con datos de tipo cadena.

Propiedades:

1.  Todas las propiedades comunes: para más información sobre esta propiedad, consulte la sección Propiedades comunes de este documento.

2.  MaxLength: este es un atributo de tipo entero opcional. Esta propiedad especifica la longitud máxima de una entrada de cadena. El valor predeterminado de esta propiedad es 128 caracteres.

3.  Text: esta es una propiedad de tipo cadena opcional. Este es el texto que aparece en el cuadro de texto. Puede definir una cadena explícita que aparezca en el cuadro de texto durante la carga inicial del control o enlazarla a un atributo de esquema de un tipo de cadena.

4.  Rows: esta es una propiedad de tipo entero opcional. Esta propiedad define el alto del cuadro de texto en unidades de caracteres. El valor predeterminado es 1 carácter.

5.  Columns: esta es una propiedad de tipo entero opcional. Esta propiedad define el ancho del cuadro de texto en unidades de caracteres. El valor predeterminado es 20 caracteres.

6.  Wrap: esta es una propiedad de tipo booleano opcional. Al establecer el valor de esta propiedad en true, el usuario habilita la característica de ajuste de línea en el cuadro de texto. El valor predeterminado de esta propiedad se establece en true.

7.  UniquenessValidationXPath: esta es una propiedad de tipo cadena opcional. Toma una expresión de filtro XPath de FIM y garantiza que el valor insertado por el usuario sea único dentro de los recursos que están en el ámbito del filtro.
    Por ejemplo, para asegurarse de que el nombre para mostrar solicitado por el usuario sea único dentro de todos los grupos de seguridad habilitados para correo de la base de datos de servicios de FIM, usaría la expresión XPath `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’`. La acción de validación se realiza cuando el usuario abandona la página. Esta propiedad solo se admite en el RCDC para la creación de un recurso.

8.  UniquenessErrorMessage: esta es una propiedad de tipo cadena opcional. Esta cadena se usa para mostrar un mensaje de error si se produce un error de validación de UniquenessValidationXPath, y puede ser texto explícito o una variable de recurso de cadena. Si no se especifica esta propiedad, el mensaje de error predeterminado para un error de validación es "% VALUE % ya existe. Pruebe con otro".

Eventos:

   • TextChanged: este evento se emite cuando cambia el texto dentro del cuadro de texto.

Consulte la sección Ejemplos de controles simples para ver un ejemplo completo de este control.

## <a name="appendix-a-default-xsd-schema"></a>Apéndice A: Esquema XSD predeterminado

Este es el esquema XSD completo para todos los RCDC predeterminados que se proporcionan con Microsoft Identity Manager 2016 SP1:

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<xsd:schema targetNamespace="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:my="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xd="http://schemas.microsoft.com/office/infopath/2003" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <xsd:attribute name="TypeName" type="my:requiredString"/>
    <xsd:attribute name="Name" type="my:requiredAlphanumericString"/>
    <xsd:attribute name="Parameters" type="xsd:string"/>
    <xsd:attribute name="DisplayAsWizard" type="xsd:boolean"/>
    <xsd:attribute name="Caption" type="xsd:string"/>
    <xsd:attribute name="AutoValidate" type="xsd:boolean"/>
    <xsd:attribute name="Enabled" type="xsd:string"/>
    <xsd:attribute name="Visible" type="xsd:string"/>
    <xsd:attribute name="IsSummary" type="xsd:boolean"/>
    <xsd:attribute name="IsHeader" type="xsd:boolean"/>
    <xsd:attribute name="HelpText" type="xsd:string"/>
    <xsd:attribute name="Link" type="xsd:string"/>
    <xsd:attribute name="Description" type="xsd:string"/>
    <xsd:attribute name="ExpandArea" type="xsd:boolean"/>
    <xsd:attribute name="Hint" type="xsd:string"/>
    <xsd:attribute name="AutoPostback" type="xsd:string"/>
    <xsd:attribute name="RightsLevel" type="my:requiredString"/>
    <xsd:attribute name="Value" type="xsd:string"/>
    <xsd:attribute name="Handler" type="my:requiredString"/>
    <xsd:attribute name="ImageUrl" type="xsd:string"/>
    <xsd:attribute name="RedirectUrl" type="xsd:string"/>
    <xsd:attribute name="ClickBehavior" type="xsd:string"/>
    <xsd:attribute name="EnableMode" type="xsd:string"/>
    <xsd:attribute name="ValueType" type="xsd:string"/>
    <xsd:attribute name="Condition" type="xsd:string"/>
    <xsd:element name="ObjectControlConfiguration">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:Panel"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="ObjectDataSource">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="XmlDataSource">
        <xsd:complexType  mixed="true">
      <xsd:sequence>
        <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
      </xsd:sequence>
      <xsd:attribute ref="my:Name"/>
      <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Panel">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:DisplayAsWizard"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:AutoValidate"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Grouping">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:IsHeader"/>
            <xsd:attribute ref="my:IsSummary"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Help">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:HelpText"/>
            <xsd:attribute ref="my:Link"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Control">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:ExpandArea"/>
            <xsd:attribute ref="my:Hint"/>
            <xsd:attribute ref="my:AutoPostback"/>
            <xsd:attribute ref="my:RightsLevel"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="CustomProperties">
        <xsd:complexType mixed="true">
            <xsd:sequence>
                <xsd:any minOccurs="0" maxOccurs="unbounded" namespace="##targetNamespace" processContents="lax"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Options">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
            <xsd:attribute ref="my:ValueType"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Option">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Buttons">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Button" minOccurs="1" maxOccurs="8"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Button">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                    <xsd:attribute ref="my:ImageUrl"/>
                    <xsd:attribute ref="my:ClickBehavior"/>
                    <xsd:attribute ref="my:RedirectUrl"/>
                    <xsd:attribute ref="my:Enabled"/>
                    <xsd:attribute ref="my:Visible"/>
                    <xsd:attribute ref="my:EnableMode"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Properties">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Property">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Condition"/>                 
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Events">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Event">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
          <xsd:attribute ref="my:Parameters"/>
        </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:simpleType name="requiredString">
        <xsd:restriction base="xsd:string">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
  <xsd:simpleType name="requiredAlphanumericString">
    <xsd:restriction base="xsd:string">
      <xsd:pattern value="[A-Za-z0-9_]{1,128}"/>
    </xsd:restriction>
  </xsd:simpleType>
    <xsd:simpleType name="requiredAnyURI">
        <xsd:restriction base="xsd:anyURI">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="requiredBase64Binary">
        <xsd:restriction base="xsd:base64Binary">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema>
```



## <a name="appendix-b-default-summary-xsl"></a>Apéndice B: XSL de resumen predeterminado

Este es el XSL de resumen completo que se proporciona con Microsoft Identity Manager 2016 SP1:
```XML
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt">
  <xsl:template name="output-attribute-value">
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <xsl:choose>
      <xsl:when test="$type='Binary'">
        <xsl:value-of select="$attribute" disable-output-escaping="yes"/>
      </xsl:when>
      <xsl:when test="$type='Text'">
        <xsl:text xml:space="preserve" disable-output-escaping="yes">(text data)</xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:value-of select="translate($attribute,' ','&#160;')" disable-output-escaping="yes"/>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:template>

  <xsl:template name="output-modified-value">
    <xsl:param name="name"/>
    <xsl:param name="attribute1"/>
    <xsl:param name="text1"/>
    <xsl:param name="attribute2"/>
    <xsl:param name="text2"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template name="output-localized-attribute-value">
    <xsl:param name="locale"/>
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template match="/">
    <xsl:choose>
      <xsl:when test="ModifiedAttributes[@ActionType='Create']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Create">
          <Attribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the value]"/>
          other <Attribute> elements
        </ModifiedAttributes>
        -->
        <table cellspacing="0" cellpadding="3" class="commonSummaryListViewGridBorder">
          <tr align="left" class="listViewHeader" style="height:22px;">
            <th class="commonSummaryListViewHeaderCellBR">Attribute</th>
            <th class="commonSummaryListViewHeaderCellBR">Value</th>
          </tr>
          <xsl:for-each select="ModifiedAttributes/Attribute">
            <xsl:sort select="@DisplayName" order="ascending"/>
            <tr class="listViewRow" style="height:22px;">
              <xsl:if test="position() mod 2 != 0">
                <td class="commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
              <xsl:if test="position() mod 2 != 1">
                <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
            </tr>
          </xsl:for-each>
        </table>
      </xsl:when>
      <xsl:when test="ModifiedAttributes[@ActionType='Modify']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Modify">
          <SingleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the old value]" SetValue="[the new value]"/>
          other <SingleAttribute> elements
          <MultipleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InsertedItem="[inserted items separated by ';']" RemovedItem="[removed items separated by ';']"/>
          other <MultipleAttribute> elements
        </ModifiedAttributes>
        -->
        <table class="commonSummaryListViewGridBorder" cellspacing="0" cellpadding="3">
          <xsl:if test="ModifiedAttributes[count(SingleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Single-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
              <th class="commonSummaryListViewHeaderCellBR">New Value</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/SingleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="listViewRow">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" cellpadding="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@InitializedValue"/>
                  <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@SetValue"/>
                  <xsl:with-param name="text2">(value removed)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
          <xsl:if test="ModifiedAttributes[count(MultipleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Multiple-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
              <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/MultipleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="uocSummaryTitleTR">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@RemovedItem"/>
                  <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@InsertedItem"/>
                  <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
        </table>
      </xsl:when>
    </xsl:choose>
  </xsl:template>
</xsl:stylesheet>
```
