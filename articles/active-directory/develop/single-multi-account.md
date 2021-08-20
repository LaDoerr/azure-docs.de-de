---
title: Öffentliche Client-Apps für einzelne und mehrere Konten | Azure
description: Hier finden Sie eine Übersicht über öffentliche Client-Apps für einzelne und mehrere Konten.
services: active-directory
author: shoatman
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/26/2019
ms.author: shoatman
ms.custom: aaddev, has-adal-ref
ms.reviewer: shoatman
ms.openlocfilehash: 4d8b45746702e8dcb9da0d9816f2a0e564854cf4
ms.sourcegitcommit: 1deb51bc3de58afdd9871bc7d2558ee5916a3e89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2021
ms.locfileid: "122428972"
---
# <a name="single-and-multiple-account-public-client-apps"></a>Öffentliche Client-Apps für einzelne und mehrere Konten

Dieser Artikel hilft Ihnen dabei, die in öffentlichen Client-Apps für einzelne und mehrere Konten verwendeten Typen zu verstehen, wobei der Schwerpunkt auf öffentlichen Client-Apps für einzelne Konten liegt. 

Der Server wird von der Azure Active Directory-Authentifizierungsbibliothek (Active Directory Authentication Library, ADAL) modelliert.  Ihre Clientanwendung wird dagegen von der Microsoft Authentication Library (MSAL) modelliert.  Die meisten Android-Apps gelten als öffentliche Clients. Ein öffentlicher Client ist eine App, die einen geheimen Schlüssel nicht sicher aufbewahren kann.  

MSAL spezialisiert die API-Oberfläche von `PublicClientApplication`, um die Entwicklungsumgebung für Apps zu vereinfachen und zu verdeutlichen, die jeweils nur ein Konto verwenden können. Für `PublicClientApplication` sind die Unterklassen `SingleAccountPublicClientApplication` und `MultipleAccountPublicClientApplication` definiert.  Im folgenden Diagramm ist die Beziehung zwischen diesen Klassen dargestellt.

![UML-Klassendiagramm für SingleAccountPublicClientApplication](./media/single-multi-account/single-and-multiple-account.png)

## <a name="single-account-public-client-application"></a>Öffentliche Clientanwendung für ein einzelnes Konto

Die `SingleAccountPublicClientApplication`-Klasse ermöglicht Ihnen das Erstellen einer MSAL-basierten App, die jeweils nur die Anmeldung eines einzelnen Kontos zulässt. `SingleAccountPublicClientApplication` unterscheidet sich auf folgende Weise von `PublicClientApplication`:

- MSAL verfolgt das aktuell angemeldete Konto nach.
  - Wenn Ihre App einen Broker verwendet (Standard bei der App-Registrierung im Azure-Portal) und auf einem Gerät installiert ist, auf dem ein Broker vorhanden ist, überprüft MSAL, ob das Konto weiterhin auf dem Gerät verfügbar ist.
- Mit `signIn` können Sie ein Konto explizit und separat von der Anforderung von Bereichen anmelden.
- `acquireTokenSilent` erfordert keinen Kontoparameter.  Wenn Sie ein Konto angeben und das von Ihnen angegebene Konto nicht dem von MSAL nachverfolgten aktuellen Konto entspricht, wird eine `MsalClientException` ausgelöst.
- `acquireToken` lässt nicht zu, dass der Benutzer Konten wechselt. Wenn der Benutzer versucht, zu einem anderen Konto zu wechseln, wird eine Ausnahme ausgelöst.
- `getCurrentAccount` gibt ein Ergebnisobjekt zurück, das Folgendes angibt:
  - Einen booleschen Wert, der angibt, ob das Konto geändert wurde. Ein Konto kann beispielsweise geändert werden, wenn ein Konto vom Gerät entfernt wurde.
  - Das vorherige Konto. Dies ist hilfreich, wenn Sie eine lokale Datenbereinigung durchführen müssen, wenn das Konto vom Gerät entfernt wird oder ein neues Konto angemeldet wird.
  - Das aktuelle Konto.
- `signOut` entfernt alle Ihrem Client zugeordneten Token vom Gerät.  

Wenn ein Android-Authentifizierungsbroker wie z. B. Microsoft Authenticator oder Intune-Unternehmensportal auf dem Gerät installiert ist und Ihre App für die Verwendung des Brokers konfiguriert ist, entfernt `signOut` das Konto nicht vom Gerät.

## <a name="single-account-scenario"></a>Szenario mit einem einzelnen Konto

Im folgenden Pseudocode wird die Verwendung von `SingleAccountPublicClientApplication` veranschaulicht.

```java
// Construct Single Account Public Client Application
ISingleAccountPublicClientApplication app = PublicClientApplication.createSingleAccountPublicClientApplication(getApplicationContext(), R.raw.msal_config);

String[] scopes = {"User.Read"};
IAccount mAccount = null;

// Acquire a token interactively
// The user will get a UI prompt before getting the token.
app.signIn(getActivity(), scopes, new AuthenticationCallback()
{

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) 
        {
            mAccount = authenticationResult.getAccount();
        }

        @Override
        public void onError(MsalException exception)
        {
        }

        @Override
        public void onCancel()
        {
        }
    }
);

// Load Account Specific Data
getDataForAccount(account);

// Get Current Account
ICurrentAccountResult currentAccountResult = app.getCurrentAccount();
if (currentAccountResult.didAccountChange())
{
    // Account Changed Clear existing account data
    clearDataForAccount(currentAccountResult.getPriorAccount());
    mAccount = currentAccountResult.getCurrentAccount();
    if (account != null)
    {
        //load data for new account
        getDataForAccount(account);
    }
}

// Sign out
if (app.signOut())
{
    clearDataForAccount(mAccount);
    mAccount = null;
}
```

## <a name="multiple-account-public-client-application"></a>Öffentliche Clientanwendung für mehrere Konten

Die `MultipleAccountPublicClientApplication`-Klasse wird verwendet, um MSAL-basierte Apps zu erstellen, die ein gleichzeitiges Anmelden mehrerer Konten zulassen. Damit können Sie Konten wie folgt abrufen, hinzufügen und entfernen:

### <a name="add-an-account"></a>Konto hinzufügen

Verwenden Sie ein oder mehrere Konten in Ihrer Anwendung, indem Sie `acquireToken` einmal oder mehrmals aufrufen.  

### <a name="get-accounts"></a>Abrufen von Konten

- Rufen Sie `getAccount` auf, um ein bestimmtes Konto abzurufen.
- Rufen Sie `getAccounts` auf, um eine Liste der Konten abzurufen, die der App aktuell bekannt sind.

Ihre App kann nicht alle auf dem Gerät vorhandenen Microsoft Identity Platform-Konten auflisten, die der Broker-App bekannt sind. Es können nur die von Ihrer App verwendeten Konten aufgelistet werden.  Konten, die vom Gerät entfernt wurden, werden von diesen Funktionen nicht zurückgegeben.

### <a name="remove-an-account"></a>Entfernen eines Kontos

Entfernen Sie ein Konto, indem Sie `removeAccount` mit einem Kontobezeichner aufrufen.

Wenn Ihre App für die Verwendung eines Brokers konfiguriert ist und ein Broker auf dem Gerät installiert ist, wird das Konto nicht vom Broker entfernt, wenn Sie `removeAccount` aufrufen.  Es werden nur Ihrem Client zugeordnete Token entfernt.

## <a name="multiple-account-scenario"></a>Szenario mit mehreren Konten

Aus dem folgenden Pseudocode geht hervor, wie Sie eine App für mehrere Konten erstellen, Konten auf dem Gerät auflisten und Token abrufen.

```java
// Construct Multiple Account Public Client Application
IMultipleAccountPublicClientApplication app = PublicClientApplication.createMultipleAccountPublicClientApplication(getApplicationContext(), R.raw.msal_config);

String[] scopes = {"User.Read"};
IAccount mAccount = null;

// Acquire a token interactively
// The user will be required to interact with a UI to obtain a token
app.acquireToken(getActivity(), scopes, new AuthenticationCallback()
 {

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) 
        {
            mAccount = authenticationResult.getAccount();
        }

        @Override
        public void onError(MsalException exception)
        {
        }

        @Override
        public void onCancel()
        {
        }
 });

...

// Get the default authority
String authority = app.getConfiguration().getDefaultAuthority().getAuthorityURL().toString();

// Get a list of accounts on the device
List<IAccount> accounts = app.getAccounts();

// Pick an account to obtain a token from without prompting the user to sign in
IAccount selectedAccount = accounts.get(0);

// Get a token without prompting the user
app.acquireTokenSilentAsync(scopes, selectedAccount, authority, new SilentAuthenticationCallback()
{

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) 
        {
            mAccount = authenticationResult.getAccount();
        }

        @Override
        public void onError(MsalException exception)
        {
        }
});
```
