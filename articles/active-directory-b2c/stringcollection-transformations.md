---
title: Beispiele für die Transformation von StringCollection-Ansprüchen für das Schema des Frameworks für die Identitätsfunktion von Azure Active Directory B2C | Microsoft-Dokumentation
description: Hier finden Sie Beispiele für die Transformation von StringCollection-Ansprüchen für das Schema des Frameworks für die Identitätsfunktion von Azure Active Directory B2C
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: c312433832f7402eaff8b40c4e0a2a61397f6f87
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58123503"
---
# <a name="stringcollection-claims-transformations"></a>Transformationen von StringCollection-Ansprüchen

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel werden Beispiele für die Verwendung von Transformationen von StringCollection-Ansprüchen für das Schema des Frameworks für die Identitätsfunktion in Azure Active Directory B2C veranschaulicht. Weitere Informationen finden Sie unter [ClaimsTransformations](claimstransformations.md).

## <a name="additemtostringcollection"></a>AddItemToStringCollection

Fügt einen String-Anspruch zu einem neuen StringCollection-Anspruch hinzu. 

| Item | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | item | Zeichenfolge | Der Anspruchstyp, der dem Ausgabeanspruch hinzugefügt werden soll. |
| InputClaim | collection | StringCollection | [Optional] Wenn dieses Element angegeben wird, werden die Elemente aus dieser Sammlung von der Anspruchstransformation kopiert, und das Element wird am Ende des Ausgabensammlungsanspruchs hinzugefügt. |
| OutputClaim | collection | StringCollection | Die Anspruchstypen, die erstellt werden, nachdem die Anspruchstransformation aufgerufen wurde. |

Verwenden Sie diese Anspruchstransformation, um eine Zeichenfolge zu einer neuen oder einer vorhandenen Zeichenfolgensammlung hinzuzufügen. Sie wird häufig in einem technischen **AAD-UserWriteUsingAlternativeSecurityId**-Profil verwendet. Bevor ein Social Media-Konto erstellt wird, liest die **CreateOtherMailsFromEmail**-Anspruchstransformation den Anspruchstyp und fügt den Wert zum Anspruchstyp **otherMails** hinzu. 

Die folgende Anspruchstransformation fügt den Anspruchstyp **email** zu **otherMails** hinzu.

```XML
<ClaimsTransformation Id="CreateOtherMailsFromEmail" TransformationMethod="AddItemToStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="item" />
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
  - **collection**: [„someone@outlook.com“]
  - **item**: „admin@contoso.com“
- Ausgabeansprüche: 
  - **collection**: [„someone@outlook.com“, „admin@contoso.com“]

## <a name="addparametertostringcollection"></a>AddParameterToStringCollection

Fügt einen Zeichenfolgenparameter zu einem neuen StringCollection-Anspruch hinzu. 

| Item | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | collection | StringCollection | [Optional] Wenn dieses Element angegeben wird, werden die Elemente aus dieser Sammlung von der Anspruchstransformation kopiert, und das Element wird am Ende des Ausgabensammlungsanspruchs hinzugefügt. |
| InputParameter | item | Zeichenfolge | Der Wert, der dem Ausgabeanspruch hinzugefügt werden soll. |
| OutputClaim | collection | StringCollection | Die Anspruchstypen, die erstellt werden, nachdem die Anspruchstransformation aufgerufen wurde. |

Verwenden Sie diese Anspruchstransformation, um einen Zeichenfolgenwert zu einer neuen oder einer vorhandenen Zeichenfolgensammlung hinzuzufügen. Im folgenden Beispiel wird eine konstante E-Mail-Adresse (admin@contoso.com) zum Anspruch **otherMails** hinzugefügt. 

```XML
<ClaimsTransformation Id="SetCompanyEmail" TransformationMethod="AddParameterToStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="item" DataType="string" Value="admin@contoso.com" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
  - **collection**: [„someone@outlook.com“]
- Eingabeparameter 
  - **item**: „admin@contoso.com“
- Ausgabeansprüche:
  - **collection**: [„someone@outlook.com“, „admin@contoso.com“]

## <a name="getsingleitemfromstringcollection"></a>GetSingleItemFromStringCollection

Ruft das erste Element aus der angegebenen Zeichenfolgensammlung ab. 

| Item | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | collection | StringCollection | Die Anspruchstypen, die von der Anspruchstransformation verwendet werden, um das Element abzurufen. |
| OutputClaim | extractedItem | Zeichenfolge | Die Anspruchstypen, die erstellt werden, nachdem diese Anspruchstransformation aufgerufen wurde. Das erste Element in der Sammlung. |

Im folgenden Beispiel wird der Anspruch **otherMails** gelesen, und das erste Element wird im Anspruch **email** zurückgegeben. 

```XML
<ClaimsTransformation Id="CreateEmailFromOtherMails" TransformationMethod="GetSingleItemFromStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="extractedItem" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
  - **collection**: [„someone@outlook.com“, „someone@contoso.com“]
- Ausgabeansprüche: 
  - **extractedItem**: „someone@outlook.com“

