---
title: Konfigurieren einer Keychain
titleSuffix: Microsoft identity platform
description: Hier erfahren Sie, wie Sie eine Keychain so konfigurieren, dass Ihre App Token darin zwischenspeichert.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: how-to
ms.date: 08/28/2019
ms.author: marsma
ms.reviewer: oldalton
ms.custom: aaddev, has-adal-ref
ms.openlocfilehash: dd8ad7f8c07a164d8c8bdac56d7ea3b57ef3aecc
ms.sourcegitcommit: 34aa13ead8299439af8b3fe4d1f0c89bde61a6db
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2021
ms.locfileid: "122419089"
---
# <a name="configure-keychain"></a>Konfigurieren einer Keychain

Wenn bei der [Microsoft Authentication Library für iOS und macOS](msal-overview.md) (MSAL) ein Benutzer angemeldet oder ein Token aktualisiert wird, versucht MSAL, Token in der Keychain zwischenzuspeichern. Dadurch kann MSAL im Hintergrund das einmalige Anmelden (SSO, Single Sign-On) zwischen mehreren Apps bereitstellen, die vom selben Apple-Entwickler herausgegeben wurden. Das einmalige Anmelden erfolgt über die Funktion der Keychain-Zugriffsgruppen. Weitere Informationen finden Sie in der [Apple-Dokumentation zu Keychain-Elementen](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc).

In diesem Artikel wird beschrieben, wie Sie App-Berechtigungen so konfigurieren, dass MSAL Token in die iOS- und macOS-Keychain schreiben kann.

## <a name="default-keychain-access-group"></a>Standard-Keychain-Zugriffsgruppe

### <a name="ios"></a>iOS

Unter iOS verwendet MSAL standardmäßig die Zugriffsgruppe `com.microsoft.adalcache`. Dabei handelt es sich um eine freigegebene Zugriffsgruppe, die von den SDKs von sowohl MSAL als auch ADAL (Azure AD Authentication Library) für das einmalige Anmelden zwischen mehreren Apps vom selben Herausgeber verwendet wird.

Fügen Sie unter iOS die Keychaingruppe `com.microsoft.adalcache` der Berechtigung Ihrer App in XCode unter **Projekteinstellungen** > **Funktionen** > **Keychainfreigabe** hinzu.

### <a name="macos"></a>macOS

Unter macOS verwendet MSAL standardmäßig die Zugriffsgruppe `com.microsoft.identity.universalstorage`.

Aufgrund von Einschränkungen der macOS-Keychain wird die MSAL-Gruppe `access group` unter macOS 10.14 und früher nicht direkt in das Attribut der Keychain-Zugriffsgruppe (siehe [kSecAttrAccessGroup](https://developer.apple.com/documentation/security/ksecattraccessgroup?language=objc)) übersetzt. Aus SSO-Perspektive verhält sich die Gruppe jedoch ähnlich, indem sie sicherstellt, dass das einmalige Anmelden für mehrere Anwendungen vom selben Apple-Herausgeber im Hintergrund ausgeführt wird.

Unter macOS 10.15 und höher (macOS Catalina) verwendet MSAL ähnlich wie unter iOS das Attribut der Keychain-Zugriffsgruppen für das einmalige Anmelden im Hintergrund.

## <a name="custom-keychain-access-group"></a>Benutzerdefinierte Keychain-Zugriffsgruppe

Wenn Sie eine andere Keychain-Zugriffsgruppe verwenden möchten, können Sie beim Erstellen von `MSALPublicClientApplicationConfig` eine benutzerdefinierte Gruppe übergeben, bevor Sie `MSALPublicClientApplication` erstellen:

# <a name="objective-c"></a>[Objective-C](#tab/objc)

```objc
MSALPublicClientApplicationConfig *config = [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"your-client-id"
                                                                                            redirectUri:@"your-redirect-uri"
                                                                                              authority:nil];
    
config.cacheConfig.keychainSharingGroup = @"custom-group";
    
MSALPublicClientApplication *application = [[MSALPublicClientApplication alloc] initWithConfiguration:config error:nil];
    
// Now call acquiretoken. 
// Tokens will be saved into the "custom-group" access group
// and only shared with other applications declaring the same access group
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
let config = MSALPublicClientApplicationConfig(clientId: "your-client-id",
                                            redirectUri: "your-redirect-uri",
                                              authority: nil)
config.cacheConfig.keychainSharingGroup = "custom-group"
        
do {
  let application = try MSALPublicClientApplication(configuration: config)
  // continue on with application          
} catch let error as NSError {
  // handle error here
}       
```

---

## <a name="disable-keychain-sharing"></a>Deaktivieren der Keychainfreigabe

Wenn Sie den SSO-Status nicht zwischen mehreren Apps freigeben oder wenn Sie keine Keychain-Zugriffsgruppen verwenden möchten, können Sie die Keychainfreigabe deaktivieren. Übergeben Sie hierzu die Anwendungspaket-ID als Wert für „keychainGroup“:

# <a name="objective-c"></a>[Objective-C](#tab/objc)

```objc
config.cacheConfig.keychainSharingGroup = [[NSBundle mainBundle] bundleIdentifier];
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
if let bundleIdentifier = Bundle.main.bundleIdentifier {
    config.cacheConfig.keychainSharingGroup = bundleIdentifier
}
```

---

## <a name="handle--34018-error-failed-to-set-item-into-keychain"></a>Fehler -34018 (Fehler beim Festlegen des Elements in der Keychain)

Der Fehler-34018 deutet in der Regel darauf hin, dass die Keychain nicht ordnungsgemäß konfiguriert wurde. Stellen Sie sicher, dass die in MSAL konfigurierte und die in den Berechtigungen konfigurierte Keychain-Zugriffsgruppe übereinstimmen.

## <a name="ensure-your-application-is-properly-signed"></a>Stellen Sie sicher, dass Ihre Anwendung ordnungsgemäß signiert ist.

Unter macOS können Anwendungen auch dann ausgeführt werden, wenn sie nicht von Entwicklern signiert wurden. Während die meisten MSAL-Funktionen trotzdem ausgeführt werden, ist für das einmalige Anmelden über den Keychainzugriff erforderlich, dass die Anwendung signiert ist. Wenn Sie mehrere Eingabeaufforderungen für die Keychain erhalten, stellen Sie sicher, dass die Signatur Ihrer Anwendung gültig ist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Keychain-Zugriffsgruppen finden Sie im Apple-Artikel [Freigeben des Zugriffs auf Keychainelemente innerhalb einer Sammlung von Apps](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc).
