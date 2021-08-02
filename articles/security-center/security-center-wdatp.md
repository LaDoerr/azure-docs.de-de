---
title: Verwenden der in Azure Security Center enthaltenen Lizenz für Microsoft Defender für den Endpunkt
description: Hier erfahren Sie mehr über Microsoft Defender für den Endpunkt und dessen Bereitstellung aus Azure Security Center.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 06/10/2021
ms.author: memildin
ms.openlocfilehash: df7d3d880161895b6cc883a15f7adf2def839a53
ms.sourcegitcommit: 23040f695dd0785409ab964613fabca1645cef90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112062260"
---
# <a name="protect-your-endpoints-with-security-centers-integrated-edr-solution-microsoft-defender-for-endpoint"></a>Schützen Sie Ihre Endpunkte mit der in Security Center integrierten EDR-Lösung: Microsoft Defender für den Endpunkt

Microsoft Defender für den Endpunkt ist eine ganzheitliche, cloudbasierte Lösung für die Endpunktsicherheit. Wichtigste Features:

- Risikobasierte Verwaltung und Bewertung von Sicherheitsrisiken 
- Verringerung der Angriffsfläche
- Verhaltensbasierter und cloudgestützter Schutz
- Endpoint Detection and Response (EDR, Endpunkterkennung und Reaktion)
- Automatisierte Untersuchung und Wiederherstellung
- Verwaltete Bedrohungssuchdienste

> [!TIP]
> Endpoint Detection and Response (EDR) wurde ursprünglich als **Windows Defender ATP** veröffentlicht und 2019 in **Microsoft Defender ATP** umbenannt.
>
> Auf der Ignite 2020 wurde die [Microsoft Defender XDR-Suite](https://www.microsoft.com/security/business/threat-protection) vorgestellt, und diese EDR-Komponente wurde in **Microsoft Defender für den Endpunkt** umbenannt.


## <a name="availability"></a>Verfügbarkeit

| Aspekt                          | Details                                                                                                                                                                                                                                                                                                       |
|---------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Status des Release:                  | Allgemeine Verfügbarkeit (General Availability, GA)                                                                                                                                                                                                                                                                                      |
| Preise:                        | Erfordert [Azure Defender für Server](defender-for-servers-introduction.md)                                                                                                                                                                                                                                             |
| Unterstützte Plattformen:            |  • Azure-Computer unter Windows<br> • Azure Arc-Computer unter Windows|
| Unterstützte Versionen von Windows für die Erkennung:  |  • Windows Server 2019, 2016, 2012 R2, und 2008 R2 SP1<br> • [Windows Virtual Desktop (WVD)](../virtual-desktop/overview.md)<br> • [Windows 10 Enterprise Multi-Session](../virtual-desktop/windows-10-multisession-faq.yml) (ehemals Enterprise for Virtual Desktops (EVD))|
| Nicht unterstützte Betriebssysteme:  |  • Windows 10 (außer EVD und WVD)<br> • Linux|
| Erforderliche Rollen und Berechtigungen: | So aktivieren/deaktivieren Sie die Integration: **Sicherheitsadministrator** oder **Besitzer**<br>So zeigen Sie MDATP-Warnungen in Security Center an: **Sicherheitsleseberechtigter**, **Leser**, **Ressourcengruppenmitwirkender**, **Ressourcengruppenbesitzer**, **Sicherheitsadministrator**, **Abonnementbesitzer** oder **Abonnementmitwirkender**|
| Clouds:                         | ![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Ja](./media/icons/yes-icon.png) US Gov<br>![Nein](./media/icons/no-icon.png) China Gov/andere Gov-Clouds                                                        |
|                                 |                                                                                                                                                                                                                                                                                                               |

## <a name="microsoft-defender-for-endpoint-features-in-security-center"></a>Features von Microsoft Defender für den Endpunkt in Security Center

Microsoft Defender für den Endpunkt bietet folgende Features:

- **Erweiterte Erkennungssensoren für Sicherheitsverletzungen**. Sensoren von Defender für den Endpunkt für Windows-Computer sammeln eine Vielzahl von verhaltensbezogenen Signalen.

- **Auf Analysen basierende, cloudgestützte Erkennung von Sicherheitsverletzungen**. Defender für den Endpunkt nimmt eine schnelle Anpassung an sich ändernde Bedrohungen vor. Das Produkt verwendet fortschrittliche Analysen und Big Data. Es wird durch die Leistungsfähigkeit von Intelligent Security Graph mit Signalen aus Windows, Azure und Office verstärkt, um unbekannte Bedrohungen zu erkennen. Das Produkt stellt aussagekräftige Warnmeldungen zur Verfügung und ermöglicht eine schnelle Reaktion.

- **Informationen zu Bedrohungen:** Defender für den Endpunkt generiert Warnungen, wenn es Angriffstools, -techniken und -verfahren erkennt. Das Produkt verwendet Daten, die von Microsoft-Bedrohungsspezialisten und Sicherheitsteams generiert werden, ergänzt durch Informationen von Partnern.

Durch die Integration von Defender für den Endpunkt in Security Center können Sie von den folgenden zusätzlichen Funktionen profitieren:

- **Automatisiertes Onboarding**. Security Center aktiviert automatisch den Sensor von Microsoft Defender für Endpunkte für alle Windows-Server, die von Security Center überwacht werden.

- **Zentralisierte Benutzeroberfläche**. Die Security Center-Konsole zeigt Warnungen aus Microsoft Defender für den Endpunkt an. Zur weiteren Untersuchung verwenden Sie die speziellen Portalseiten von Microsoft Defender für den Endpunkt, auf denen Sie zusätzliche Informationen wie die Warnungsprozessstruktur und das Incidentdiagramm finden. Sie können auch eine detaillierte Computerzeitachse verwenden, die jedes Verhalten über einen historischen Zeitraum von bis zu sechs Monaten anzeigt.

    :::image type="content" source="./media/security-center-wdatp/microsoft-defender-security-center.png" alt-text="Security Center von Microsoft Defender für den Endpunkt" lightbox="./media/security-center-wdatp/microsoft-defender-security-center.png":::

## <a name="what-are-the-requirements-for-the-microsoft-defender-for-endpoint-tenant"></a>Welche Anforderungen gelten für Mandanten für den Microsoft Defender für den Endpunkt?

Wenn Sie Azure Security Center zum Überwachen Ihrer Server verwenden, wird automatisch ein Mandant für Microsoft Defender für den Endpunkt erstellt. 

- **Speicherort:** Die von Defender für den Endpunkt gesammelten Daten werden im Rahmen der Bereitstellung des Mandanten am Standort des Mandanten gespeichert. Kundendaten (in pseudonymisierter Form) können auch in den zentralen Speicher- und Verarbeitungssystemen in den USA gespeichert werden. Nachdem Sie den Speicherort konfiguriert haben, kann dieser nicht mehr geändert werden. Wenn Sie Ihre eigene Lizenz für Microsoft Defender für Endpunkt besitzen und Ihre Daten an einen anderen Speicherort verschieben müssen, bitten Sie den Microsoft-Support, den Mandanten zurückzusetzen.
- **Verschieben von Abonnements:** Wenn Sie Ihr Azure-Abonnement zwischen Azure-Mandanten verschoben haben, sind ein paar manuelle Vorbereitungsschritte erforderlich, bevor Security Center Defender für Endpoint bereitstellen kann. Ausführliche Informationen erhalten Sie vom [Microsoft-Support](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).


## <a name="enable-the-microsoft-defender-for-endpoint-integration"></a>Aktivieren von Microsoft Defender für die Integration des Endpunkts

### <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich, dass Ihr Computer die erforderlichen Anforderungen für Defender für Endpunkt erfüllt:

1. Stellen Sie sicher, dass der Computer wie erforderlich mit Azure verbunden ist:

    - Für **Windows**-Server konfigurieren Sie die Netzwerkeinstellungen, die unter [Konfigurieren von Geräteproxy- und Internetkonnektivitätseinstellungen](/windows/security/threat-protection/microsoft-defender-atp/configure-proxy-internet) beschrieben werden.
    - Für **lokale Computer** stellen Sie eine Verbindung mit Azure Arc her, wie in [Verbinden eines Hybridcomputers mit Azure Arc-fähigen Servern](../azure-arc/servers/learn/quick-enable-hybrid-vm.md) erläutert.
    - Für **Windows Server 2019** und [Windows Virtual Desktop (WVD)](../virtual-desktop/overview.md)-Computer vergewissern Sie sich, dass auf Ihren Computern die Erweiterung „MicrosoftMonitoringAgent“ verwendet wird.
    
1. Aktivieren Sie **Azure Defender für Server**. Siehe [Schnellstart: Aktivieren von Azure Defender](enable-azure-defender.md).
1. Wenn Sie Microsoft Defender für den Endpunkt bereits lizenziert und auf Ihren Servern bereitgestellt haben, entfernen Sie diesen mithilfe des unter [Ausschließen von Windows-Servern](/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints#offboard-windows-servers) beschriebenen Verfahrens.
1. Wenn Sie Ihr Abonnement zwischen Azure-Mandanten verschoben haben, sind auch einige manuelle Vorbereitungsschritte erforderlich. Ausführliche Informationen erhalten Sie vom [Microsoft-Support](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).


### <a name="enable-the-integration"></a>Die Integration aktivieren
1. Wählen Sie im Menu des „Security Center“ **Preise und Einstellungen** und dann das Abonnement aus, das Sie ändern möchten.
1. Wählen Sie **Integrationen** aus.
1. Wählen Sie **Microsoft Defender für Endpunkt den Zugriff auf meine Daten erlauben** und dann **Speichern** aus.

    :::image type="content" source="./media/security-center-wdatp/enable-integration-with-edr.png" alt-text="Aktivieren der Integration zwischen Azure Security Center und Microsoft Defender für den Endpunkt, der EDR-Lösung von Microsoft":::

    Azure Security Center integriert die Server automatisch in Microsoft Defender für den Endpunkt. Das Onboarding kann bis zu 24 Stunden dauern.


## <a name="access-the-microsoft-defender-for-endpoint-portal"></a>Zugreifen auf das Portal von Microsoft Defender für den Endpunkt

1. Vergewissern Sie sich, dass das Benutzerkonto über die notwendigen Berechtigungen verfügt. Weitere Informationen finden Sie unter [Zuweisen von Benutzerzugriff auf Microsoft Defender Security Center](/windows/security/threat-protection/microsoft-defender-atp/assign-portal-access).

1. Überprüfen Sie, ob Sie einen Proxy oder eine Firewall einsetzen, der bzw. die anonymen Datenverkehr blockiert. Der Sensor von Defender für den Endpunkt stellt eine Verbindung aus dem Systemkontext her, daher muss anonymer Datenverkehr zugelassen werden. Um den ungehinderten Zugriff auf das Portal von Defender für den Endpunkt sicherzustellen, befolgen Sie die Anweisungen unter [Aktivieren des Zugriffs auf Dienst-URLs im Proxyserver](/windows/security/threat-protection/microsoft-defender-atp/configure-proxy-internet#enable-access-to-microsoft-defender-atp-service-urls-in-the-proxy-server).

1. Öffnen Sie das [Microsoft Defender Security Center-Portal](https://securitycenter.windows.com/). Weitere Informationen zu den Features und Symbolen des Portals finden Sie in der [Übersicht über das Microsoft Defender Security Center-Portal](/windows/security/threat-protection/microsoft-defender-atp/portal-overview). 

## <a name="send-a-test-alert"></a>Senden einer Testwarnung

So generieren Sie eine unbedenkliche Testwarnung in Microsoft Defender für den Endpunkt

1. Erstellen Sie einen Ordner „C:\test-MDATP-test“.
1. Verwenden Sie Remotedesktop, um auf Ihren Computer zuzugreifen.
1. Öffnen Sie ein Befehlszeilenfenster.
1. Kopieren Sie an der Eingabeaufforderung den folgenden Befehl, und führen Sie ihn aus. Das Eingabeaufforderungsfenster wird automatisch geschlossen.

    ```powershell
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-MDATP-test\\invoice.exe'); Start-Process 'C:\\test-MDATP-test\\invoice.exe'
    ```
    :::image type="content" source="./media/security-center-wdatp/generate-edr-alert.png" alt-text="Ein Eingabeaufforderungsfenster mit dem Befehl zum Generieren einer Testwarnung":::

1. Wenn der Befehl erfolgreich ist, wird Ihnen eine neue Warnung im Azure Security Center-Dashboard und im Portal von Microsoft Defender für den Endpunkt angezeigt. Die Anzeige dieser Warnung kann einige Minuten dauern.
1. Um die Warnung in Security Center zu überprüfen, navigieren Sie zu **Sicherheitswarnungen** > **Verdächtige PowerShell-Befehlszeile**.
1. Wählen Sie im Untersuchungsfenster den Link aus, um zum Portal von Microsoft Defender für den Endpunkt zu navigieren.

    > [!TIP]
    > Die Warnung wird mit dem Schweregrad **Info** ausgelöst.

## <a name="faq---security-centers-integration-with-microsoft-defender-for-endpoint"></a>FAQ – Integration von Security Center mit Microsoft Defender für Endpunkt

- [Worum handelt es sich bei der „MDE.Windows“-Erweiterung, die auf meiner VM ausgeführt wird?](#whats-this-mdewindows-extension-running-on-my-machine)
- [Welche Lizenzierungsanforderungen gelten für Microsoft Defender für den Endpunkt?](#what-are-the-licensing-requirements-for-microsoft-defender-for-endpoint)
- [Kann ich für Azure Defender einen Rabatt erhalten, wenn ich bereits über eine Lizenz für Microsoft Defender für Endpunkt verfüge?](#if-i-already-have-a-license-for-microsoft-defender-for-endpoint-can-i-get-a-discount-for-azure-defender)
- [Wie steige ich von einem Drittanbieter-EDR-Tool um?](#how-do-i-switch-from-a-third-party-edr-tool)

### <a name="whats-this-mdewindows-extension-running-on-my-machine"></a>Worum handelt es sich bei der „MDE.Windows“-Erweiterung, die auf meiner VM ausgeführt wird?

In der Vergangenheit wurde Microsoft Defender für Endpunkt vom Log Analytics-Agent bereitgestellt. Als wir die [Unterstützung um Windows Server 2019 erweitert](release-notes.md#microsoft-defender-for-endpoint-integration-with-azure-defender-now-supports-windows-server-2019-and-windows-10-virtual-desktop-wvd-released-for-general-availability-ga) haben, haben wir auch eine Erweiterung zum Ausführen des automatischen Onboardings hinzugefügt. 

Security Center stellt automatisch die Erweiterung MDE.Windows auf VMs mit folgenden Betriebssystemen bereit:

- Windows Server 2019 
- Windows 10 Virtual Desktop (WVD)
- Andere Versionen von Windows Server, wenn Security Center die Betriebssystemversion nicht erkennt (z. B. wenn ein benutzerdefiniertes VM-Image verwendet wird). In diesem Fall wird Microsoft Defender für Endpunkt weiterhin vom Log Analytics-Agent bereitgestellt.

> [!TIP]
> Wenn Sie die MDE.Windows-Erweiterung löschen, entfernt dies nicht Microsoft Defender für Endpunkt. Informationen zum Offboarden finden Sie unter [Offboarding von Windows-Servern](/microsoft-365/security/defender-endpoint/configure-server-endpoints?view=o365-worldwide).

### <a name="what-are-the-licensing-requirements-for-microsoft-defender-for-endpoint"></a>Welche Lizenzierungsanforderungen gelten für Microsoft Defender für den Endpunkt?
Defender für den Endpunkt ist ohne zusätzliche Kosten in **Azure Defender für Server** enthalten. Alternativ kann er separat für 50 Computer oder mehr erworben werden.

### <a name="if-i-already-have-a-license-for-microsoft-defender-for-endpoint-can-i-get-a-discount-for-azure-defender"></a>Kann ich für Azure Defender einen Rabatt erhalten, wenn ich bereits über eine Lizenz für Microsoft Defender für Endpunkt verfüge?
Wenn Sie bereits eine Lizenz für Microsoft Defender für Endpunkt besitzen, müssen Sie für diesen Teil Ihrer Azure Defender-Lizenz nicht bezahlen.

Fordern Sie Ihren Rabatt beim Supportteam von Security Center an, und geben Sie die entsprechende Arbeitsbereichs-ID, die Region und die Anzahl der Microsoft Defender for Endpoint-Lizenzen an, die für Computer im angegebenen Arbeitsbereich angewendet wurden.

Der Rabatt gilt ab dem Genehmigungsdatum und wird nicht rückwirkend wirksam.

### <a name="how-do-i-switch-from-a-third-party-edr-tool"></a>Wie steige ich von einem Drittanbieter-EDR-Tool um?
Vollständige Anweisungen zum Wechseln von einer Nicht-Microsoft-Endpunktlösung finden Sie in der Dokumentation zu Microsoft Defender für den Endpunkt: [Übersicht über die Migration](/windows/security/threat-protection/microsoft-defender-atp/switch-to-microsoft-defender-migration).
  


## <a name="next-steps"></a>Nächste Schritte

- [Von Azure Security Center unterstützte Features und Plattformen](security-center-os-coverage.md)
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md): Erfahren Sie, wie Empfehlungen Ihnen beim Schutz Ihrer Azure-Ressourcen helfen.
