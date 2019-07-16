---
title: Usar otro proveedor de Multi-Factor Authentication a través de una API para activar PAM o en SSPR | Microsoft Docs
description: Configure la API de MFA personalizada como una segunda capa de seguridad cuando los usuarios activen roles en Privileged Access Management y use el Autoservicio de restablecimiento de contraseña.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 09/04/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 7fb111520f94541672fc56d0fd2ee95bfcd3a49e
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690744"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>Usar un proveedor personalizado de Multi-Factor Authentication a través de una API durante la activación de roles de PAM o en SSPR

Los clientes de Azure AD Premium o Azure MFA pueden integrar Azure MFA en dos escenarios de MIM: la activación de roles de Privileged Access Management (PAM) y el Autoservicio de restablecimiento de contraseña (SSPR).

Los clientes de MIM tienen dos opciones adicionales:

 - Usar un proveedor de entrega de contraseña de un solo uso, que es aplicable únicamente en el escenario de SSPR de MIM y se documenta en la guía [Configure Self-Service Password Reset with OTP SMS Gate](https://docs.microsoft.com/en-us/previous-versions/mim/hh824692(v=ws.10)) (Configurar Autoservicio de restablecimiento de contraseña con Puerta de SMS de OTP).
 - Usar un proveedor de telefonía de autenticación multifactor personalizada. Esto es aplicable en los escenarios de PAM y SSPR de MIM que se describen en este artículo.

En este artículo se describe cómo usar MIM con un proveedor personalizado de Multi-Factor Authentication, a través de una API y un SDK de integración desarrollado por el cliente.  

## <a name="prerequisites"></a>Requisitos previos

Para usar una API del proveedor personalizado de Multi-Factor Authentication con MIM, necesita:

- Números de teléfono de todos los usuarios candidatos.
- Revisión [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) o posterior de MIM; consulte el [historial de versiones](reference/version-history.md) para ver los anuncios.
- Servicio MIM configurado para SSPR o PAM.

## <a name="approach-using-custom-multi-factor-authentication-code"></a>Método mediante el código de autenticación multifactor personalizada

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>Paso 1: Asegurarse de que la versión del servicio MIM es la 4.5.202.0 o una posterior

Descargue e instale la revisión [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) de MIM (o una versión posterior).

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>Paso 2: Crear un archivo DLL que implemente la interfaz IPhoneServiceProvider

El archivo DLL debe incluir una clase que implemente tres métodos:

- `InitiateCall`: servicio MIM invocará este método. El servicio pasa el identificador de solicitud y el número de teléfono como parámetros.  El método debe devolver un valor `PhoneCallStatus` de `Pending`, `Success` o `Failed`.
- `GetCallStatus`: si una llamada anterior a `initiateCall` devolvió `Pending`, el servicio MIM invocará este método. Este método también devuelve el valor `PhoneCallStatus` de `Pending`, `Success` o `Failed`.
- `GetFailureMessage`: si una llamada anterior a `InitiateCall` o `GetCallStatus` devolvió `Failed`, el servicio MIM invocará este método. Este método devuelve un mensaje de diagnóstico.

Las implementaciones de estos métodos deben ser seguras para subprocesos y, además, la implementación de `GetCallStatus` y `GetFailureMessage` no debe asumir que serán llamadas por el mismo subproceso, como en una llamada anterior a `InitiateCall`.

Guarde el archivo DLL en el directorio `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\`.

Código de ejemplo que puede ser compilado mediante Visual Studio 2010 o posterior.

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using Microsoft.IdentityManagement.PhoneServiceProvider;

namespace CustomPhoneGate
{
    public class CustomPhoneGate: IPhoneServiceProvider
    {
        string path = @"c:\Test\phone.txt";
        public PhoneCallStatus GetCallStatus(string callId)
        {
            int res = 2;
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        bool b = Int32.TryParse(info[2], out res);
                        if (!b)
                        {
                            res = 2;
                        }
                    }
                    break;
                }
            }
            switch(res)
            {
                case 0:
                    return PhoneCallStatus.Pending;
                case 1:
                    return PhoneCallStatus.Success;
                case 2:
                    return PhoneCallStatus.Failed;
                default:
                    return PhoneCallStatus.Failed;
            }       
        }
        public string GetFailureMessage(string callId)
        {
            string res = "Call ID is not found";
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        res = info[3];
                    }
                    else
                    {
                        res = "Description is not found";
                    }
                    break;
                }
            }
            return res;            
        }
        
        public PhoneCallStatus InitiateCall(string phoneNumber, Guid requestId, Dictionary<string,object> deliveryAttributes)
        {
            // Here should be some logic for performing voice call
            // For testing purposes we just write details in file             
            string info = string.Format("{0};{1};{2};{3}", requestId, phoneNumber, 0, string.Empty);
            using (StreamWriter sw = File.AppendText(path))
            {
                sw.WriteLine(info);                
            }
            return PhoneCallStatus.Pending;    
        }
    }
}
```
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Paso 3: Realizar una copia de seguridad de MfaSettings.xml, ubicado en "C:\Archivos de programa\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-4-edit-the-mfasettingsxml-file"></a>Paso 4: Editar el archivo MfaSettings.xml

Actualice o desactive las líneas siguientes:

- Quite o borre todas las líneas de entradas de configuración 

- Actualice o agregue las líneas siguientes al MfaSettings.xml siguiente con su proveedor de teléfono personalizado <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>Paso 5: Reiniciar el servicio MIM

Una vez reiniciado el servicio, use SSPR y/o PAM para validar las funciones con el proveedor de identidades personalizado.

> [!NOTE] 
> Para revertir la configuración, reemplace MfaSettings.xml con el archivo de copia de seguridad en el paso 3.


## <a name="next-steps"></a>Pasos siguientes

- [Introducción al Servidor Azure Multi-Factor Authentication](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [¿Qué es Azure Multi-Factor Authentication?](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [MIM version release history](./reference/version-history.md) (Historial de lanzamiento de versiones de MIM)
