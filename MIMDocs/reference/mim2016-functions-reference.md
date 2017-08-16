---
title: "Referencia de función para Microsoft Identity Manager 2016 | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 8f36cf981971db0d6c55fc17cce874a8faf0ecaf
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2017
---
# <a name="functions-reference-for-microsoft-identity-manager-2016"></a>Referencia de función para Microsoft Identity Manager 2016


En Microsoft Identity Manager (MIM) 2016, las funciones le permiten modificar los valores de atributo antes de que fluyan a un destino en una actividad de función o un aprovisionamiento declarativo. El objetivo de este documento es ofrecerle una visión general de las funciones disponibles y una descripción de cómo se pueden usar.

La configuración de asignaciones de flujo de atributos es una tarea elemental al configurar reglas de sincronización. La forma más sencilla de una asignación de flujo de atributos es una asignación directa. Tal y como indica su nombre, una asignación directa toma el valor de un atributo de origen y lo aplica al atributo de destino configurado. Hay casos en los que necesitará que se modifiquen los valores de atributo existentes o que se calculen nuevos valores de atributo antes de que el sistema los aplique a un destino.

Las funciones son un método integrado que se usa para definir el tipo de modificación que necesita que aplique el motor de sincronización al generar un valor de atributo para un destino.

En MIM, puede agrupar las funciones existentes en las siguientes categorías:

-   **Funciones de manipulación de datos**. Funciones para llevar a cabo diversas operaciones de manipulación sobre cadenas.

-   **Funciones de recuperación de datos**. Funciones para extraer datos de valores de atributo.

-   **Funciones de generación de datos**. Funciones para generar valores.

-   **Funciones de lógica**. Funciones para realizar operaciones basadas en condiciones.

En las secciones siguientes se proporcionan más detalles sobre las funciones de cada categoría.

## <a name="data-manipulation-functions"></a>Funciones de manipulación de datos

Las funciones de manipulación de datos se usan para realizar diversas operaciones de manipulación sobre cadenas.

| Concatenate        |   |
|--------------------|-------------------------|
| Descripción        | La función Concatenate se usa para concatenar dos o más cadenas.                                                                                                       |
| Firma de función | string1 + string2...                                                                                                                                                     |
| Entradas             | Dos o más cadenas                                                                                                                                                        |
| Operaciones         | Todos los parámetros de la cadena de entrada se concatenan entre sí.                                                                                                              |
| Salida             | Una cadena        |


| UpperCase         |         |
|-------------------|---------|
| Descripción        | La función UpperCase convierte en mayúsculas todos los caracteres de una cadena.         |
| Firma de función | String UpperCase(string)                                                                                                                                                   |
| Entradas             | Una cadena                                                                                                                                                                 |
| Operaciones         | Todos los caracteres en minúsculas del parámetro de entrada se convierten en caracteres en mayúsculas. Ejemplo: UpperCase("test") da como resultado "TEST".                                     |
| Salida             | Una cadena                                                              |


| LowerCase          |                                 |
|--------------------|---------------------------------|
| Descripción        | La función LowerCase convierte en minúsculas todos los caracteres de una cadena.                                                                                                  |
| Firma de función | String LowerCase(string)                                                                                                                                                   |
| Entradas             | Una cadena                                                                                                                                                                 |
| Operaciones         | Todos los caracteres en mayúsculas del parámetro de entrada se convierten en caracteres en minúsculas. Ejemplo: LowerCase("TeSt") da como resultado "test".                                     |
| Salida             | Una cadena               |


| ProperCase        |                                                          |
|-------------------|----------------------------------------------------------|
| Descripción        | La función ProperCase convierte en mayúsculas el primer carácter de cada palabra delimitada por espacios de una cadena, y todos los demás caracteres se convierten en minúsculas.           |
| Firma de función | String ProperCase(string)                                                                                                                                                  |
| Entradas             | Una cadena                                                                                                                                                                 |
| Operaciones         | El primer carácter de cada palabra delimitada por espacios en el parámetro de entrada se convierte en mayúsculas y todos los caracteres en mayúsculas se convierten en caracteres en minúsculas. Si una palabra en el parámetro de entrada comienza con un carácter no alfabético, el primer carácter de la palabra no se convierte en mayúsculas. <br/> Ejemplos: <br/> - ProperCase("TEsT") da como resultado "Test". <br/> -   ProperCase("britta simon") da como resultado "Britta Simon". <br/>-   ProperCase(" TEsT") da como resultado " Test". <br/> -   ProperCase("\$TEsT") da como resultado "\$Test".|
| Salida             | Una cadena      |


| LTrim              |      |
|--------------------|------|
| Descripción        | La función LTrim quita los espacios en blanco iniciales de una cadena.                                                                                                             |
| Firma de función | String LTrim(string)                                                                                                                                                       |
| Entradas             | Una cadena                                                                                                                                                                 |
| Operaciones         | Se quitan los caracteres de espacio en blanco iniciales contenidos en el parámetro de entrada. <br/><br/>Ejemplo: LTrim(" Test") da como resultado "Test ".                                              |
| Salida             | Una cadena      |



| RTrim              |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Descripción        | La función RTrim quita los espacios en blanco finales de una cadena.                                                                 |
| Firma de función | String RTrim(string)                                                                                                            |
| Entradas             | Una cadena                                                                                                                      |
| Operaciones         | Se quitan los caracteres de espacio en blanco finales contenidos en el parámetro de entrada. Ejemplo: RTrim(" Test ") da como resultado " Test".  |
| Salida             | Una cadena                                                                                                                      |


| Trim               |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Descripción        | La función Trim quita los espacios en blanco iniciales y finales de una cadena.                                                      |
| Firma de función | String Trim(string)                                                                                                             |
| Entradas             | Una cadena                                                                                                                      |
| Operaciones         | Se quitan los caracteres de espacio en blanco iniciales y finales contenidos en la cadena. Ejemplo: Trim(" Test ") da como resultado "Test". |
| Salida             | Una cadena                                                                                                                      |




| RightPad           |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Descripción        | La función RightPad rellena hacia la derecha una cadena hasta la longitud especificada mediante el carácter de relleno proporcionado.                          |
| Firma de función | String RightPad(string, lenght, padCharacter)                                                                                   |
| Operaciones         | Si la longitud de la cadena es menor que el valor de lenght, padCharacter se anexa repetidamente al final de la cadena hasta que tenga una longitud igual a lenght. <br/> Ejemplos: <br/> - RightPad("User", 10, "0") daría como resultado "User000000". <br/> - RightPad(RandomNum(1,10), 5, "0") podría dar como resultado "9000".   |
| Salida                                                                                                                                                          | Si la cadena tiene una longitud mayor o igual que la longitud, se devuelve una cadena idéntica a la cadena. Si la longitud de la cadena es menor que la longitud, se devuelve una cadena nueva con la longitud deseada que contiene una cadena rellenada con un padCharacter. Si la cadena es null, la función devuelve una cadena vacía. |   |   |
>[!NOTE]
**padCharacter** puede ser un carácter de espacio, pero no puede ser un valor null. Si la longitud de **string** es igual o superior a **length**, **string** se devuelve sin cambios.


| LeftPad      |     |
|----|-------|
| Descripción  | La función LeftPad rellena hacia la izquierda una cadena hasta la longitud especificada mediante el carácter de relleno proporcionado.    |
| Firma de función      | String LeftPad(string, length, padCharacter)     |
| Entradas |  - **String.** Cadena que se va a rellenar. <br/> - **length.** entero que representa la longitud de cadena deseada. <br/> - **padCharacter.** Una cadena que consta de un solo carácter que se usará como carácter de relleno. |
| Operaciones  | Si la longitud de la cadena es menor que el valor de lenght, padCharacter se anexa repetidamente al comienzo de la cadena hasta que tenga una longitud igual a lenght. <br/> Ejemplos: <br/> - LeftPad("User", 10, "0") daría como resultado "000000User". <br/> LeftPad(RandomNum(1,10), 5, "0") podría dar como resultado "0009". |  
|Salida | Si la cadena tiene una longitud mayor o igual que la longitud, se devuelve una cadena idéntica a la cadena. <br/> Si la longitud de la cadena es menor que la longitud, se devuelve una cadena nueva con la longitud deseada que contiene una cadena rellenada con un padCharacter. <br/>  Si **string** es null, la función devuelve una cadena vacía.                                                   |

<[!NOTE]
**padCharacter** puede ser un carácter de espacio, pero no puede ser un valor null. Si la longitud de **string** es igual o superior a **length**, **string** se devuelve sin cambios.

| BitOr    |  |
|----- |------|
| Descripción  | La función BitOr establece un bit especificado en una marca en 1.     |
| Firma de función  | Int BitOr(mask, flag)       |  
| Entradas     | 1. **mask.** Valor hexadecimal que especifica el bit para establecer en la marca. <br/> 2. **flag.** Valor hexadecimal que va a tener un bit específico modificado.    |   
| Operaciones         | Esta función convierte ambos parámetros en una representación binaria y los compara: <br/> -Establece un bit en 1 si uno o ambos bits correspondientes de máscara y marca son 1 y en 0 si los dos bits correspondientes son 0. <br/> -Devuelve 1 en todos los casos excepto cuando los bits correspondientes de ambos parámetros son 0. <br/> -El patrón de bits resultante son los bits "establecidos" (1 o true) de cualquiera de los dos operandos. Se pueden establecer varios bits de marca si varios bits tienen el valor 1 en la máscara.  |
| Salida             | Una nueva versión de **flag**, con los bits especificados en **mask** establecidos en 1.                   |


| BitAnd             |                                                                                    |
|--------------------|------------------------------------------------------------------------------------|
| Descripción        | La función BitAnd establece un bit especificado en una marca en 0.                           |
| Firma de función | Int BitOr(mask, flag)                                                              |
| Entradas             | 1. **mask.** Valor hexadecimal que especifica el bit que hay que modificar en la marca. <br/> 2. **flag.** Valor hexadecimal que va a tener un bit específico modificado.   |
| Operaciones         | Esta función convierte ambos parámetros en una representación binaria y los compara: <br/> -Establece un bit en 0 si uno o ambos bits correspondientes de **máscara** y **marca** son 0 y en 1 si los dos bits correspondientes son 1. <br/> -Devuelve 0 en todos los casos excepto cuando los bits correspondientes de ambos parámetros son 1. Se pueden establecer varios bits de marca en 0 si varios bits tienen el valor 0 en **mask**. |
| Salida             | Una nueva versión de **flag** con los bits especificados en **mask** establecidos en 0.                    |

| DateTimeFormat                                 |    |
|---------------------------------------|------------|
| Descripción       | La función DateTimeFormat se usa para dar formato a un elemento DateTime en formato de cadena y transformarlo en un formato especificado.     |
| Firma de función   | String DateTimeFormat(dateTime, format)      |
| Entradas   | 1. dateTime. Cadena que representa el elemento DateTime al que se dará formato.  <br/> 2. **format.** Cadena que representa el formato al que se convertirá.  |   
| Operaciones           | La cadena de formato especificada en format se aplica al elemento dateTime de la cadena dateTime. <br/> La cadena especificada en format debe ser un formato de fecha y hora válido. Si no es así, se devuelve un error que indica que el formato no es un formato correcto de DateTime. <br/> Ejemplo: DateTime("12/25/2007", "aaaa-MM-dd") da como resultado "2007-12-25".|   
| Salida     | Una cadena que resulta de aplicar **format** a **dateTime.**   |

>[!Note]                                                                                                                                                                             
Para conocer los caracteres aceptados para crear formatos definidos por el usuario, consulte [Formatos de fecha y hora definidos por el usuario](http://go.microsoft.com/fwlink/?LinkId=195182)


| ConvertSidToString       |    |   
|--------------------------|----|
| Descripción       | La función ConvertSidToString convierte una matriz de bytes que contiene un identificador de seguridad en una cadena.         |
| Firma de función      | String ConvertSidToString(ObjectSID)    |
| Entradas  | **ObjectSID.** Matriz de bytes que contiene un identificador de seguridad (SID).   |
| Operaciones    | El SID binario especificado se convierte en una cadena.    |
| Salida              | Representación de cadena del SID.   |  

| ConvertStringToGuid |        |
|---------------------|--------|
| Descripción         | La función  **ConvertStringToGuid** convierte la representación de cadena de un GUID en una representación binaria del GUID.      |
| Función            | Byte[] ConvertStringToGuid(stringGuid)  |  
| Entradas              | **Guid.** Cadena con formato con este patrón: **xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx**, donde el valor del GUID se representa como una serie de dígitos hexadecimales en grupos de 8, 4, 4, 4 y 12 dígitos separados por guiones. Un ejemplo de un valor devuelto es "382c74c3-721d-4f34-80e557657b6cbc27".  |
| Operaciones          | La cadena **Guid** especificada en el parámetro 1 se convierte en su representación binaria. <br/> Si la cadena no es una representación de un **Guid** válido, la función rechaza el argumento con el siguiente error: <br/> **El parámetro de la función ConvertStringToGuid debe ser una cadena que representa un Guid válido.**  |
| Salida              | Una representación binaria del Guid.           |                                                                           


| ReplaceString       |     |
|--------------------|-------|
| Descripción         | La función ReplaceString reemplaza todas las apariciones de una cadena con otra cadena.  |   
| Función            | String ReplaceString(string, OldValue, NewValue)    |                                                                          
| Entradas              | 1. **String.** Cadena en la que se va a reemplazar valores. <br/> 2. **OldValue.** cadena que se va a buscar y reemplazar. <br/> 3. NewValue. cadena con la que se va a reemplazar. |
| Operaciones          | Todas las apariciones de OldValue en la cadena se reemplazan por NewValue. La función debe poder gestionar los caracteres especiales siguientes: <br/> - **\n.** Nueva línea. <br/> - **\r.** Retorno de carro. <br/> - **\t.** Tabulación. <br/> Ejemplo: ReplaceString(“One\n\rMicrosoft\n\r\Way”,”\n\r”,” “) devuelve "One Microsoft Way". |   
| Salida              | Cadena con todas las apariciones de **OldValue** en la cadena que se reemplazan por **NewValue.**      |

## <a name="data-retrieval-functions"></a>Funciones de recuperación de datos

Las funciones de recuperación de datos se usan para realizar operaciones que recuperan los caracteres deseados de una cadena.

| Word       |        |
|--------------------|---------------|
| Descripción        | La función Word devuelve una palabra contenida en una cadena, en función de los parámetros que describen los delimitadores que se van a utilizar y el número de palabras que se va a devolver.                                                                |
| Firma de función | String Word(string, number, delimiters)                                                                                                                                                                        |
| Entradas             | 1. **string.** La cadena de la que se va a devolver una palabra. <br/> 2. **number.** Número que identifica el número de palabras que se debe devolver. <br/> 3. **delimeters.** Cadena que representa los delimitadores que deben usarse para identificar las palabras. |
| Operaciones         | Cada cadena de caracteres de una cadena que está separada por uno de los caracteres de los delimitadores se identifica como una palabra. Se devuelve la palabra que se encontró en la posición especificada en el parámetro 3 (número¡): <br/> • Si el número < 1, se devuelve una cadena vacía. <br/> • Si la cadena es null, se devuelve una cadena vacía. <br/><br/> Ejemplos: <br/> 1. Word("Test;of%function;", 3, 2;$&%") devuelve "function". <br/> 2. Word(2Test;;Function" , 2 , ";") devuelve “” (una cadena vacía). 3. Word("Test;of%function;", 0, 2;$&%") devuelve "" (una cadena vacía).
| Salida             | Cadena que contiene la palabra en la posición que solicita el usuario. Si **string** contiene menos del número de palabras, o **string** no contiene ninguna palabra identificada por **delimitadores,** se devuelve una cadena vacía. |  


| Izquierda               |   |
|-------|-------|
| Descripción        | La función Left devuelve un número especificado de caracteres desde la izquierda de una cadena.       |
| Firma de función | String Left(string, numChars)     |
| Entradas             | 1. **string.** Cadena desde la que se devuelven los caracteres. 2. **numChars.** Número que identifica el número de caracteres que se va a devolver desde el principio de una cadena.         |
| Operaciones         | Se devuelven caracteres **numChars** desde la primera posición de la cadena. <br/> Ejemplo: Left("Britta Simon", 3) devuelve "Bri".   |
| Salida             | Cadena que contiene los primeros caracteres numChars de la cadena.  <br/> • Si numChars = 0, se devuelve una cadena vacía. <br/> • Si numChars < 0, se devuelve una cadena de entrada. <br/> • Si la cadena es null, se devuelve una cadena vacía. |




| Right       |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Descripción | La función Right devuelve el número especificado de caracteres desde la derecha (el final) de una cadena.                                 |
| Firma de función   | String Right(string, numChars)   |
| Entradas      | 1. **String.** Cadena desde la que se devuelven los caracteres. <br/> 2. **numChars.** Número que identifica el número de caracteres que se va a devolver desde el final de la cadena.  |
| Operaciones  | **numChars.** Se devuelven los caracteres del final de una cadena. <br/> Ejemplo: Right("Britta Simon", 3) devuelve "mon".                  |
| Salida      | Cadena que contiene los últimos caracteres numChars de una cadena. Si numChars = 0, se devuelve una cadena vacía. <br/> - Si **numChars** < 0, se devuelve una cadena de entrada. <br/> - Si la cadena es null, se devuelve una cadena vacía. <br/> -Si la cadena contiene menos caracteres que el número especificado en numChars, se devuelve una cadena idéntica a la cadena. |




| Mid         |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Descripción | La función Mid devuelve un número especificado de caracteres a partir de una posición especificada en una cadena.                              |
| Firma de función    | String Mid(string, pos, numChars)                                                                                             |
| Entradas      | 1. **string.** Cadena desde la que se devuelven los caracteres.   <br/> 2. **pos.** Número que identifica la posición inicial en una cadena para devolver caracteres. <br/> 3. **numChars.** Número que identifica el número de caracteres que se devolverá desde una posición en la cadena.  |
| Operaciones  | Devuelve caracteres **numChars** a partir de la posición **pos** de la cadena. <br/>Ejemplo: Mid("Britta Simon", 3, 5) devolvería "itta ". |
| Salida      | Cadena que contiene caracteres **numChars** a partir de la posición **pos** de una cadena: <br/> - Si **numChars** = 0, se devuelve una cadena vacía. <br/> - Si **numChars** < 0, se devuelve una cadena vacía. <br/> - Si **pos** > la longitud de cadena, se devuelve una cadena de entrada. <br/> - Si **pos** ≤ 0, se devuelve una cadena de entrada. <br/> - Si **string** es null, se devuelve una cadena vacía. <br/> Si no hay caracteres **numChar** restantes en la **cadena** a partir de la posición **pos**, se devuelven todos los caracteres que se puedan devolver.

## <a name="data-generation-functions"></a>Funciones de generación de datos

Las funciones de generación de datos se usan para generar valores para tipos de datos específicos.

| CRLF               |                                                                                          |
|--------------------|------------------------------------------------------------------------------------------|
| Descripción        | El CRLF genera un retorno de carro/avance de línea. Esta función se usa para agregar una nueva línea. |
| Firma de función | CRLF de cadena                                                                              |
| Entradas             | Sin parámetros                                                                            |
| Operaciones         | Se devuelve un CRLF.                                                                      |
|                    | Ejemplo: AddressLine1 + CRLF() + AddressLine2 da como resultado: <br/> - AddressLine1 <br/> - AddressLine2 |
| Salida             | Un CRLF es la salida.                                                                                             |

| RandomNum          |                                                                                                                   |  
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Descripción        | La función RandomNum devuelve un número aleatorio dentro de un intervalo especificado.                                       |   
| Firma de función | Int RandomNum(start, end)                                                                                         |   
| Entradas             | - **start**. Número que identifica el límite inferior del valor aleatorio que se va a generar.   <br/> - **end**. Número que identifica el límite superior del valor aleatorio que se va a generar.  |
| Operaciones         | Se genera un número aleatorio mayor o igual que **start** y menor o igual que **end**. <br/>  Ejemplo: Random(0,999) podría devolver 100.                      |
| Salida             | Un número aleatorio dentro del intervalo especificado por **start** y **end**.                                                      |  

| EscapeDNComponent  |                                                                                                                   |   
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Descripción        | El método *EscapeDNComponent* procesa la cadena de entrada en función del tipo de agente de administración que se usa. |
| Firma de función | String EscapeDNComponent(string)                                                                                  |
| Entradas     | **string**. Cadena que se usa para procesar un nombre distintivo. La cadena no debe contener caracteres de escape. |
| Operaciones | El método EscapeDNComponent de MIISUtils se usa para realizar esta operación. Este método procesa la cadena de entrada en función del tipo de agente de administración que se usa. <br/> Dado que agentes de administración diferentes requieren diferentes formatos de nombre distintivo, este método procesa las cadenas de entrada en función del tipo de agente de administración. Los tipos son el nombre distintivo del protocolo ligero de acceso a directorios (LDAP), como Active Directory® Domain Services, Sun Directory Server (anteriormente iPlanet Directory Server), Microsoft Exchange Server; no LDAP jerárquico, como Microsoft Lotus Notes; y extrínseco, como base de datos y XML sin nombres distintivos LDAP. <br/> **Nombre distintivo LDAP: ** <br/> - Cualquier carácter XML no válido en la parte del valor de una parte dada tiene codificación hexadecimal. <br/>-Los caracteres no válidos (incluidos los caracteres XML no válidos) en la parte del nombre de una parte dada generan un error. <br/> - No se permiten los siguientes caracteres de escape: <br/> &nbsp;&nbsp;&nbsp; - Coma (",") <br/> &nbsp;&nbsp;&nbsp; - Signo igual ("=") <br/> &nbsp;&nbsp;&nbsp; - Signo más ("+") <br/> &nbsp;&nbsp;&nbsp; - Signo menor que ("<") <br/> &nbsp;&nbsp;&nbsp; - Signo mayor que (">") <br/> &nbsp;&nbsp;&nbsp; - Signo de almohadilla ("#") <br/> &nbsp;&nbsp;&nbsp; - Punto y coma (";") <br/> &nbsp;&nbsp;&nbsp; - Barra diagonal inversa ('\') <br/> &nbsp;&nbsp;&nbsp; - Comillas (""") <br/> - Si el último carácter de la cadena es un espacio, a continuación, ese espacio usa la secuencia de escape. <br/> - Cualquier espacio extraño, inicial o final, alrededor del nombre de una parte se elimina. <br/> - Para el agente de administración de XML, si hay varias partes, las partes se alfabetizan. <br/> - Si se especifican varias partes, la cadena de nombre distintivo compuesto es la concatenación de las cadenas individuales separadas por signos de más (+). <br/> - Se genera un error si la cadena de entrada no es una cadena de nombre distintivo de estilo LDAP con un formato correcto. <br/><br/> **No LDAP jerárquico** <br/> - Estos agentes de administración no son compatibles con componentes de varias partes. Si varias cadenas se pasan a EscapeDNComponent, se produce una excepción ArgumentException. <br/> - Si cualquiera de los caracteres de la cadena de entrada son caracteres XML no válidos, se produce una excepción ArgumentException. <br/> - Todas las comas y barras diagonales inversas en la cadena de entrada usan secuencias de escape. <br/> - Si el último carácter de la cadena es un espacio, a continuación, ese espacio usa la secuencia de escape. <br/><br/> **Extrínseco:** <br/> 1. Si alguna parte es binaria o contiene un carácter XML no válido, esa parte se almacena como una versión con codificación hexadecimal de los datos sin formato con una carácter "#" delante de la cadena. Por ejemplo, si una parte era "AxC" (donde x representa un carácter XML no válido, como "0x10"), esa parte se codifica como "#410010004300". <br/> 2. De lo contrario, todas las instancias de los siguientes caracteres usan secuencias de escape: <br/> &nbsp;&nbsp;&nbsp; - Barra diagonal inversa ('\') <br/> &nbsp;&nbsp;&nbsp; - Coma (",") <br/> &nbsp;&nbsp;&nbsp; - Signo más ("+") <br/> &nbsp;&nbsp;&nbsp; - Signo de almohadilla ("#") <br/> 3. Si el último carácter de la cadena de una parte dada es un espacio, ese espacio usa una secuencia de escape. <br/> 4. Si se especifican varias partes, la cadena de nombre distintivo compuesto es la concatenación de todas las cadenas individuales separadas por signos de más (+).
| Salida      | Cadena que contiene un nombre de dominio válido.                                                                                                                  |   

>[!NOTE]
La validación de nombres distintivos es menos estricta que la sintaxis definida en las especificaciones de LDAP. EscapeDNComponent(String[]) permite que el nombre de una parte contenga cualquier combinación de uno o varios de estos caracteres: "a"-"z", "A"-"Z", "0"-"9", "-", y ".". <br/>
No es posible especificar una parte binaria con este método. Sin embargo, es posible tener una parte binaria en **CommitNewConnector** si el nombre distintivo está formado por atributos delimitadores y uno de estos atributos es un tipo binario.


| Null        |                                                                                                                                                           |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Descripción | La función Null se usa para definir que este MA (agente de administración) no tiene un atributo con el que contribuir y que la prioridad de los atributos debe continuar con el siguiente MA. |   
| Firma de función    | String Null    |
| Entradas      | Sin parámetros                                                                                                                                             |   
| Operaciones  | Devuelve un valor Null. <br/> Ejemplo: IIF(Eq(domain), "unknown", Null())                                                                                           |   
| Salida      | Se genera un valor Null.                                                                                                                                         |   |   |


## <a name="logic-functions"></a>Funciones de lógica
Las funciones de lógica se usan para realizar una operación basada en las condiciones evaluadas por el sistema.

| IIF        |  |
|-------------|---|
| Descripción | La función IIF devuelve un conjunto de valores posibles basado en una condición especificada.    |
| Firma de función   | Object IIF(condition, valueIfTrue, valueIfFalse)   |                                                 |
| Entradas      | 1. **Condition**. Cualquier valor o expresión que pueda evaluarse como true o false. 2. **valueIfTrue**: valor que se devuelve si la condición se evalúa como true. <br/> 3. **valueIfFalse**: valor que se devuelve si la condición se evalúa como false. <br/><br/> Los siguientes funciones están disponibles para su uso como expresiones en la función IIF como **condición:** <br/> **Eq.** Esta función compara si dos argumentos son iguales. <br/> **NotEquals.** Esta función compara si dos argumentos no son iguales y devuelve true si no son iguales y false si son iguales.<br/> Ejemplo: NotEquals(EmployeeType, "Contractor")<br/> **LessThan.** Esta función compara dos números y devuelve true si el primero es menor que el segundo y false en caso contrario.<br/>Ejemplo: LessThan(Salary, 100000) <br/>**GreaterThan.** Esta función compara dos números y devuelve true si el primero es mayor que el segundo y false en caso contrario.<br/> Ejemplo: GreaterThan(Salary, 100000) <br/> **LessThanOrEquals.** Esta función compara dos números y devuelve true si el primero es menor o igual que el segundo y false en caso contrario.<br/>Ejemplo: LessThanOrEquals(Salary, 100000) <br/> **GreaterThanOrEquals.* Esta función compara dos números y devuelve true si el primero es mayor o igual que el segunda y false en caso contrario. <br/>Ejemplo: GreaterThanOrEquals(Salary, 100000)<br/> IsPresent Esta función toma como entrada un atributo en el esquema ILM y devuelve true si el atributo no es null y false si el atributo es null.|
| Operaciones  | Si **condition** se evalúa como true, se devuelve **valueIfTrue.**. En caso contrario, se devuelve **valorIfFalse.**. <br/>Ejemplo: IIF(Eq(EmployeeType,"Intern"),"t-" + Alias, Alias) devuelve el alias de un usuario con una "t-" agregada al comienzo si el usuario es un interno. En caso contrario, devuelve el alias del usuario tal y como está. |
| Salida      | La salida es **valueIfTrue** si la condición es true o **valueIfFalse** si la condición es false. |      
