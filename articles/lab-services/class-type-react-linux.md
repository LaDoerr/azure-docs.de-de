---
title: Einrichten eines Labs mit React unter Linux mithilfe von Azure Lab Services
description: Erfahren Sie, wie Sie Labs für einen React-Entwicklungskurs einrichten.
author: emaher
ms.topic: article
ms.date: 05/16/2021
ms.author: enewman
ms.openlocfilehash: 60ac7c3a95564fad5c271c543beac875334b05a1
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110482776"
---
# <a name="set-up-lab-for-react-on-linux"></a>Einrichten eines Labs für React unter Linux

[React](https://reactjs.org/) ist eine beliebte JavaScript-Bibliothek zum Erstellen von Benutzeroberflächen (UI). React ist eine deklarative Möglichkeit, wiederverwendbare Komponenten für Ihre Website zu erstellen.  Es gibt viele weitere beliebte Bibliotheken für JavaScript-basierte Front-End-Entwicklung.  Wir verwenden einige dieser Bibliotheken beim Erstellen des Labs.  [Redux](https://redux.js.org/) ist eine Bibliothek, die einen vorhersagbaren Zustandscontainer für JavaScript-Apps bereitstellt und häufig als Ergänzung zu React verwendet wird. [JSX](https://reactjs.org/docs/introducing-jsx.html) ist eine Bibliothekssyntaxerweiterung für JavaScript, die häufig mit React verwendet wird, um zu beschreiben, wie die Benutzeroberfläche aussehen sollte.  [NodeJS](https://nodejs.org/) ist eine bequeme Möglichkeit, einen Webserver für Ihre React-Anwendung auszuführen.

In diesem Artikel erfahren Sie, wie Sie [Visual Studio Code](https://code.visualstudio.com/) für Ihre Entwicklungsumgebung, die Tools und die Bibliotheken installieren, die für einen React-Webentwicklungskurs erforderlich sind.

## <a name="lab-configuration"></a>Labkonfiguration

Zum Einrichten dieses Labs benötigen Sie zunächst ein Azure-Abonnement und ein Lab-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. Sobald Sie ein Azure-Abonnement erhalten, können Sie ein neues Lab-Konto in Azure Lab Services erstellen. Weitere Informationen zum Erstellen eines neuen Lab-Kontos finden Sie im Tutorial zum [Einrichten eines Lab-Kontos](./tutorial-setup-lab-account.md). Sie können auch ein vorhandenes Lab-Konto verwenden.

### <a name="lab-account-settings"></a>Lab-Kontoeinstellungen

Aktivieren Sie die Einstellungen Ihres Lab-Kontos wie in der folgenden Tabelle beschrieben. Weitere Informationen zum Aktivieren von Azure Marketplace-Images finden Sie unter [Angeben der für Lab-Ersteller verfügbaren Azure Marketplace-Images](./specify-marketplace-images.md).

| Lab-Kontoeinstellungen | Instructions |
| ----------- | ------------ |  
| Marketplace-Images | Aktivieren Sie das Image „Ubuntu Server 18.04 LTS“ für die Verwendung in Ihrem Lab-Konto. |

### <a name="lab-settings"></a>Lab-Einstellungen

Die empfohlene VM-Größe hängt von den Arten der Workloads ab, die Ihre Kursteilnehmer verwenden sollen.  

| Lab-Einrichtung | Wert und Beschreibung |
| ------------ | ------------------ |
| VM-Größe | **Klein**.|

Es wird empfohlen, Ihre Workloads zu testen, um festzustellen, ob eine größere Dimensionierung erforderlich ist.  Weitere Informationen zu den einzelnen Größen finden Sie unter [VM-Größen](administrator-guide.md#vm-sizing).

## <a name="template-machine-configuration"></a>Konfiguration des Vorlagencomputers

Die Schritte in diesem Abschnitt zeigen, wie Sie Folgendes ausführen, um die Vorlagen-VM einzurichten:

1. Installieren von Entwicklungstools.
1. Installieren von Debuggererweiterungen für Ihren Webbrowser.
1. Aktualisieren der Firewalleinstellungen.

### <a name="install-development-tools"></a>Installieren von Entwicklungstools

1. Installieren Sie Ihren bevorzugten Webbrowser.  
1. Installieren Sie [Node.js](https://nodejs.org).

    ```bash
    sudo apt install nodejs
    ```

1. Installieren Sie den [Node-Paket-Manager](https://www.npmjs.com/), der zum Installieren von React, Redux und JSX verwendet wird.

    ```bash
    sudo apt install npm
    ```

1. Installieren Sie [Visual Studio Code](https://code.visualstudio.com/docs/setup/linux).
1. Installieren Sie die [Erweiterung „Reactive Native Tools“ für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=msjsdiag.vscode-react-native).
1. Installieren Sie optional Erweiterungen für die Entwicklung mit [Redux](https://marketplace.visualstudio.com/search?term=Redux&target=VSCode&category=All%20categories&sortBy=Relevance) und [JSX](https://marketplace.visualstudio.com/search?term=JSX&target=VSCode&category=All%20categories&sortBy=Relevance).

[Create React App](https://create-react-app.dev/) (Erstellen einer React-App) ist eine offiziell unterstützte Methode zum Erstellen einer ReactApp und erfordert keine weitere Konfiguration, wenn npm 5.2 oder höher verwendet wird.  Anweisungen zum Verwenden von „Create React App“ (Erstellen einer React-App) finden Sie in der Dokumentation zu den [ersten Schritten](https://create-react-app.dev/docs/getting-started).

Andere Komponenten, die für eine React-basierte Website benötigt werden, werden mit npm in einer bestimmten Anwendung installiert. Geben Sie beispielsweise die folgenden Befehle ein, um die Redux- und JSX-Bibliotheken zu installieren:

```bash
npm install react-redux
npm install react-jsx
```

### <a name="install-debugger-extensions"></a>Installieren von Debuggererweiterungen

Installieren Sie die Erweiterungen für React-Entwicklertools für Ihren Browser, damit Sie React-Komponenten untersuchen und Leistungsinformationen aufzeichnen können.  

- [Add-On „React-Entwicklertools Edge“](https://microsoftedge.microsoft.com/addons/detail/react-developer-tools/gpphkfbcpidddadnkolkpfckpihlkkil)
- [Add-On „React-Entwicklertools Chrome“](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)
- [Add-On „React-Entwicklertools FireFox“](https://addons.mozilla.org/firefox/addon/react-devtools/)

Verwenden Sie den integrierten Befehl `npm start`, um die App im Entwicklungsmodus auszuführen.  Die lokalen und Netzwerk-URLs werden in der Befehlsausgabe aufgeführt.  Informationen zur Verwendung von HTTPS anstelle von HTTP finden Sie unter [Erstellen einer React-App mit HTTPS in der Entwicklung](https://create-react-app.dev/docs/using-https-in-development).

>[!IMPORTANT]
>Dozenten müssen die Vorlagen-VM oder eine andere Lab-VM verwenden, um auf die Website eines Kursteilnehmers zuzugreifen.

## <a name="cost"></a>Cost

Sehen wir uns eine Beispielkostenschätzung für diesen Kurs an.  Angenommen, Sie haben einen Kurs mit 25 Kursteilnehmern. Jeder Kursteilnehmer verfügt über eine geplante Kurszeit von 20 Stunden.  Zudem erhält jeder Kursteilnehmer ein Kontingent von 10 zusätzlichen Stunden für Hausaufgaben und Arbeitsaufträge außerhalb der geplanten Kurszeit.  Als Größe des virtuellen Computers wurde **Klein** ausgewählt, was 20 Lab-Einheiten entspricht.

- 25 Kursteilnehmer &times; (20 geplante Stunden + 10 Kontingentstunden) &times; 20 Lab-Einheiten &times; 0,01 USD pro Stunde = 150,00 USD

> [!IMPORTANT]
> Diese Kostenschätzung dient ausschließlich zu Beispielzwecken.  Aktuelle Informationen zu Preisen finden Sie unter [Azure Lab Services – Preise](https://azure.microsoft.com/pricing/details/lab-services/).

## <a name="next-steps"></a>Nächste Schritte

Das Vorlagenimage kann jetzt im Lab veröffentlicht werden. Weitere Anweisungen finden Sie unter [Veröffentlichen der Vorlagen-VM](how-to-create-manage-template.md#publish-the-template-vm).

Lesen Sie folgende Artikel zum Einrichten des Labs:

- [Hinzufügen von Benutzern](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Festlegen von Kontingenten](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zeitplan festlegen](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Registrierungslinks per E-Mail an Kursteilnehmer senden](how-to-configure-student-usage.md#send-invitations-to-users)
