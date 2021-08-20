---
title: Migrieren von Xamarin-Apps zu MSAL.NET
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Xamarin iOS-Apps, die Microsoft Authenticator verwenden, von ADAL.NET zu MSAL.NET migriert werden.
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/08/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: devx-track-csharp, aaddev, has-adal-ref
ms.openlocfilehash: e77a0c3ccfbd6c1b11948724083c896a5d192a65
ms.sourcegitcommit: 34aa13ead8299439af8b3fe4d1f0c89bde61a6db
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2021
ms.locfileid: "122419011"
---
# <a name="migrate-ios-applications-that-use-microsoft-authenticator-from-adalnet-to-msalnet"></a>Migrieren von iOS-Anwendungen mit Microsoft Authenticator von ADAL.NET zu MSAL.NET

Bisher haben Sie ADAL.NET (Azure Active Directory Authentication Library for .NET) und den iOS-Broker verwendet. Jetzt es ist an der Zeit, zu [MSAL.NET](msal-overview.md) (Microsoft Authentication Library for .NET) zu migrieren, da diese Bibliothek den Broker unter iOS ab Version 4.3 unterstützt.

Wo sollten Sie beginnen? Dieser Artikel unterstützt Sie beim Migrieren Ihrer Xamarin iOS-App von ADAL zu MSAL.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie bereits über eine Xamarin iOS-App verfügen, die in den iOS-Broker integriert ist. Wenn dies nicht der Fall ist, wechseln Sie direkt zu MSAL.NET und beginnen dort mit der Brokerimplementierung. Informationen zum Aufrufen des iOS-Brokers in MSAL.NET mit einer neuen Anwendung finden Sie in [dieser Dokumentation](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS#why-use-brokers-on-xamarinios-and-xamarinandroid-applications).

## <a name="background"></a>Hintergrund

### <a name="what-are-brokers"></a>Was sind Broker?

Broker sind Anwendungen, die von Microsoft unter Android und iOS bereitgestellt werden. (Siehe hierzu die [Microsoft Authenticator](https://www.microsoft.com/p/microsoft-authenticator/9nblgggzmcj6)-App unter iOS und Android und die Unternehmensportal-App von Microsoft Intune unter Android.)

Sie ermöglichen Folgendes:

- Einmaliges Anmelden.
- Geräteidentifizierung, die von einigen [Richtlinien für bedingten Zugriff](../conditional-access/overview.md) gefordert wird. Weitere Informationen finden Sie unter [Geräteverwaltung](../conditional-access/concept-conditional-access-conditions.md#device-platforms).
- Überprüfung der Anwendungsidentifizierung, die auch bei einigen Unternehmensszenarien erforderlich ist. Weitere Informationen finden Sie unter [Verwaltung mobiler Anwendungen in Intune](/intune/mam-faq).

## <a name="migrate-from-adal-to-msal"></a>Migrieren von ADAL zu MSAL

### <a name="step-1-enable-the-broker"></a>Schritt 1: Aktivieren des Brokers

<table>
<tr><td>Aktueller ADAL-Code:</td><td>MSAL-Entsprechung:</td></tr>
<tr><td>
In ADAL.NET wurde die Brokerunterstützung pro Authentifizierung aktiviert. Standardmäßig ist es deaktiviert. Sie mussten

im `PlatformParameters`-Konstruktor ein `useBroker`-Flag auf „true“ festlegen, um den Broker aufzurufen:

```csharp
public PlatformParameters(
        UIViewController callerViewController,
        bool useBroker)
```
Legen Sie auch im plattformspezifischen Code in diesem Beispiel im Seitenrenderer für iOS das `useBroker`-Flag
auf „true“ fest:
```csharp
page.BrokerParameters = new PlatformParameters(
          this,
          true,
          PromptBehavior.SelectAccount);
```

Fügen Sie dann die Parameter in den Aufruf zum Abrufen eines Tokens (AcquireToken-Aufruf) ein:
```csharp
 AuthenticationResult result =
                    await
                        AuthContext.AcquireTokenAsync(
                              Resource,
                              ClientId,
                              new Uri(RedirectURI),
                              platformParameters)
                              .ConfigureAwait(false);
```

</td><td>
In MSAL.NET wird die Brokerunterstützung wird pro PublicClientApplication aktiviert. Standardmäßig ist es deaktiviert. Um sie zu aktivieren, verwenden Sie den

`WithBroker()`-Parameter (standardmäßig auf „true“ festgelegt) zum Aufrufen des Brokers:

```csharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos)
                .Build();
```
Im AcquireToken-Aufruf:
```csharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow(App.RootViewController)
             .ExecuteAsync();
```
</table>

### <a name="step-2-set-a-uiviewcontroller"></a>Schritt 2: Festlegen von „UIViewController()“
In ADAL.NET haben Sie einen UIViewController als Teil von `PlatformParameters` übermittelt. (Siehe Beispiel in Schritt 1.) Um dem Entwickler mehr Flexibilität zu bieten, wird in MSAL.NET ein Objektfenster verwendet, das aber bei normaler iOS-Verwendung nicht erforderlich ist. Damit Sie den Broker verwenden und den Broker aufrufen und Antworten empfangen können, müssen Sie das Objektfenster festlegen.
<table>
<tr><td>Aktueller ADAL-Code:</td><td>MSAL-Entsprechung:</td></tr>
<tr><td>
Ein UIViewController wird an den

`PlatformParameters` in der iOS-spezifischen Plattform übergeben.

```csharp
page.BrokerParameters = new PlatformParameters(
          this,
          true,
          PromptBehavior.SelectAccount);
```
</td><td>
In MSAL.NET führen Sie zwei Schritte aus, um das Objektfenster für iOS festzulegen:

1. Legen Sie in `AppDelegate.cs` den `App.RootViewController`-Parameter auf einen neuen `UIViewController()` fest.
Durch diese Zuweisung wird sichergestellt, dass ein UIViewController mit dem Aufruf des Brokers vorhanden ist. Wenn der Controller nicht ordnungsgemäß festgelegt wird, erhalten Sie möglicherweise folgende Fehlermeldung: `"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
1. Verwenden Sie im AcquireTokenInteractive-Aufruf den `.WithParentActivityOrWindow(App.RootViewController)`-Parameter, und übergeben Sie den Verweis an das verwendete Objektfenster.

**Beispiel:**

In `App.cs`:
```csharp
   public static object RootViewController { get; set; }
```
In `AppDelegate.cs`:
```csharp
   LoadApplication(new App());
   App.RootViewController = new UIViewController();
```
Im AcquireToken-Aufruf:
```csharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow(App.RootViewController)
             .ExecuteAsync();
```

</table>

### <a name="step-3-update-appdelegate-to-handle-the-callback"></a>Schritt 3: Aktualisieren von AppDelegate zum Verarbeiten des Rückrufs
ADAL und MSAL rufen beide den Broker auf, und der Broker wiederum führt über die `OpenUrl`-Methode der `AppDelegate`-Klasse einen Rückruf zu Ihrer Anwendung aus. Weitere Informationen finden Sie in [dieser Dokumentation](msal-net-use-brokers-with-xamarin-apps.md#step-3-update-appdelegate-to-handle-the-callback).

Hier gibt es keine Änderungen bei MSAL.NET gegenüber ADAL.NET.

### <a name="step-4-register-a-url-scheme"></a>Schritt 4: Registrieren eines URL-Schemas
ADAL.NET und MSAL.NET verwenden URLs, um den Broker aufzurufen und die Brokerantwort an die App zurückzugeben. Registrieren Sie in der Datei `Info.plist` das URL-Schema für Ihre App wie folgt:

<table>
<tr><td>Aktueller ADAL-Code:</td><td>MSAL-Entsprechung:</td></tr>
<tr><td>
Das URL-Schema ist für Ihre App eindeutig.
</td><td>
Das

`CFBundleURLSchemes`-Name muss das Präfix

`msauth.`

gefolgt von Ihrem `CFBundleURLName` enthalten

Beispiel: `$"msauth.(BundleId")`

```csharp
 <key>CFBundleURLTypes</key>
    <array>
      <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.yourcompany.xforms</string>
        <key>CFBundleURLSchemes</key>
        <array>
          <string>msauth.com.yourcompany.xforms</string>
        </array>
      </dict>
    </array>
```

> [!NOTE]
> Dieses URL-Schema wird Teil des Umleitungs-URI, der zur eindeutigen Identifizierung der App verwendet wird, wenn die Antwort vom Broker empfangen wird.

</table>

### <a name="step-5-add-the-broker-identifier-to-the-lsapplicationqueriesschemes-section"></a>Schritt 5: Hinzufügen der Broker-ID im Abschnitt LSApplicationQueriesSchemes

ADAL.NET und MSAL.NET verwenden `-canOpenURL:`, um zu überprüfen, ob der Broker auf dem Gerät installiert ist. Fügen Sie in der Datei „Info.plist“ dem Abschnitt „LSApplicationQueriesSchemes“ den richtigen Bezeichner für den iOS-Broker wie folgt hinzu:

<table>
<tr><td>Aktueller ADAL-Code:</td><td>MSAL-Entsprechung:</td></tr>
<tr><td>
Verwendung

`msauth`


```csharp
<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauth</string>
</array>
```
</td><td>
Verwendung

`msauthv2`


```csharp
<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauthv2</string>
     <string>msauthv3</string>
</array>
```
</table>

### <a name="step-6-register-your-redirect-uri-in-the-azure-portal"></a>Schritt 6: Registrieren des Umleitungs-URI im Azure-Portal

Bei ADAL.NET und MSAL.NET bringt die Verwendung des Brokers als Ziel eine zusätzliche Anforderung an den Umleitungs-URI mit sich. Registrieren Sie den Umleitungs-URI bei Ihrer Anwendung im Azure-Portal.
<table>
<tr><td>Aktueller ADAL-Code:</td><td>MSAL-Entsprechung:</td></tr>
<tr><td>

`"<app-scheme>://<your.bundle.id>"`

Beispiel:

`mytestiosapp://com.mycompany.myapp`
</td><td>

`$"msauth.{BundleId}://auth"`

Beispiel:

`public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; `

</table>

Weitere Informationen zum Registrieren des Umleitungs-URI im Azure-Portal finden Sie unter [Schritt 7: Hinzufügen eines Umleitungs-URI zur App-Registrierung](msal-net-use-brokers-with-xamarin-apps.md#step-7-add-a-redirect-uri-to-your-app-registration).

### <a name="step-7-set-the-entitlementsplist"></a>**Schritt 7: Festlegen von „Entitlements.plist“**

Aktivieren Sie den Keychainzugriff in der Datei *Entitlements.plist*:

```xml
 <key>keychain-access-groups</key>
    <array>
      <string>$(AppIdentifierPrefix)com.microsoft.adalcache</string>
    </array>
```

Weitere Informationen zum Aktivieren des Keychainzugriffs finden Sie unter [Aktivieren des Keychainzugriffs](msal-net-xamarin-ios-considerations.md#enable-keychain-access).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie [Spezielle Überlegungen für Xamarin iOS mit MSAL.NET](msal-net-xamarin-ios-considerations.md).