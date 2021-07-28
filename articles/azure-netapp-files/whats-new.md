---
title: Neues in Azure NetApp Files | Microsoft-Dokumentation
description: Bietet eine Übersicht über die neuesten Features und Verbesserungen in Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 06/14/2021
ms.author: b-juche
ms.openlocfilehash: 768b2f4a9b993e4824fbcebe7a96d7f62a90c934
ms.sourcegitcommit: 3bb9f8cee51e3b9c711679b460ab7b7363a62e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112078310"
---
# <a name="whats-new-in-azure-netapp-files"></a>Neues in Azure NetApp Files

Azure NetApp Files wird regelmäßig aktualisiert. Dieser Artikel bietet eine Übersicht über die neuesten Features und Verbesserungen. 

## <a name="june-2021"></a>Juni 2021

* [Azure NetApp Files-Speicherdienst-Add-Ons](storage-service-add-ons.md)

    Die neue Menüoption **Speicherdienst-Add-Ons** von Azure NetApp Files stellt eine „Startplattform“ im Azure-Portal für unterstützte Ökosystem-Add-Ons von Drittanbietern für den Azure NetApp Files-Speicherdienst bereit. Mit dieser neuen Menüoption im Portal können Sie eine Landing Page eingeben, indem Sie für den schnellen Zugriff auf ein Add-On auf die entsprechende Add-On-Kachel klicken.  

    **NetApp-Add-Ons** ist die erste Kategorie von Add-Ons, die unter **Speicherdienst-Add-Ons** angezeigt wird. Sie bietet Zugriff auf **NetApp Cloud Compliance**. Wenn Sie auf die Kachel **NetApp Cloud Compliance** klicken, wird ein neuer Browser geöffnet, und Sie werden zur Add-On-Installationsseite weitergeleitet. 

* Ab sofort allgemein verfügbare Features   

    Die folgenden Azure NetApp Files-Features sind ab sofort allgemein verfügbar. Sie müssen die Features nicht mehr registrieren, bevor Sie sie verwenden:
    * [Momentaufnahmerichtlinie](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies)
    * [Manueller QoS-Kapazitätspool](manual-qos-capacity-pool-introduction.md)

* [Unterstützung von „Freigegebenes AD“ für mehrere Konten in einem Active Directory pro Region und Abonnement](create-active-directory-connections.md#shared_ad) (Vorschau)   

    Bisher wurde von Azure NetApp Files nur ein einzelnes Active Directory (AD) pro Region unterstützt, wobei nur ein einzelnes NetApp-Konto für den Zugriff auf das AD konfiguriert werden konnte. Dank des neuen Features **Freigegebenes AD** kann von allen NetApp-Konten, die über eines der zu einem Abonnement und einer Region gehörenden NetApp-Konten erstellt wurden, eine AD-Verbindung gemeinsam genutzt werden. Bei Verwendung dieses Features kann beispielsweise von allen zu einem Abonnement und einer Region gehörenden NetApp-Konten die allgemeine AD-Konfiguration verwendet werden, um ein SMB-Volume, ein NFSv4.1-Kerberos-Volume oder ein Dualprotokollvolume zu erstellen. Wenn Sie dieses Feature verwenden, ist die AD-Verbindung in allen zu einem Abonnement und einer Region gehörenden NetApp-Konten sichtbar.

## <a name="may-2021"></a>Mai 2021 

* Das Azure NetApp Files-Tool für konsistente Momentaufnahmen in Anwendungen [(AzAcSnap)](azacsnap-introduction.md) ist ab sofort allgemein verfügbar. 

    AzAcSnap ist ein Befehlszeilentool, mit dem Sie den Datenschutz für Datenbanken von Drittanbietern (SAP HANA) in Linux-Umgebungen (z. B SUSE und RHEL) vereinfachen können. Die neuesten Änderungen am Tool finden Sie in den [Versionshinweisen zu AzAcSnap](azacsnap-release-notes.md).   

* [Unterstützung von Abrechnungstags für Kapazitätspools](manage-billing-tags.md)   

    Für Azure NetApp Files werden jetzt Abrechnungstags unterstützt, damit Sie die Kosten für Geschäftseinheiten oder andere interne Consumer vergleichen können. Abrechnungstags werden auf der Ebene des Kapazitätspools und nicht auf Volumeebene zugewiesen, und sie werden auf der Kundenrechnung angezeigt.

* [ADDS LDAP über TLS](configure-ldap-over-tls.md) (Vorschau) 

    Die LDAP-Kommunikation zwischen Client- und Serveranwendungen wird standardmäßig nicht verschlüsselt. Dies bedeutet, dass mit einem Netzwerküberwachungsgerät oder einer entsprechenden Software die Kommunikation zwischen einem LDAP-Client und den Servercomputern angezeigt werden kann. Dieses Szenario kann in nicht isolierten oder freigegebenen VNETs problematisch sein, wenn eine einfache LDAP-Bindung verwendet wird. Der Grund ist, dass die Anmeldeinformationen (Benutzername und Kennwort), die zum Binden des LDAP-Clients an den LDAP-Server verwendet werden, in unverschlüsselter Form über das Netzwerk übergeben werden. LDAP über TLS (auch als LDAPS bezeichnet) ist ein Protokoll, bei dem TLS zum Schützen der Kommunikation zwischen LDAP-Clients und -Servern verwendet wird. Für Azure NetApp Files wird jetzt die sichere Kommunikation für AD DS-Instanzen (Active Directory Domain Services) per LDAP über TLS unterstützt. Bei Azure NetApp Files kann jetzt LDAP über TLS genutzt werden, um authentifizierte Sitzungen zwischen den LDAP-Servern mit Active Directory-Integration einzurichten. Sie können das Feature „LDAP über TLS“ für Volumes mit NFS, SMB und dualem Protokoll aktivieren. Standardmäßig ist LDAP über TLS für Azure NetApp Files deaktiviert.  

* Unterstützung für [Durchsatzmetriken](azure-netapp-files-metrics.md)    

    Mit Azure NetApp Files wird die Unterstützung für die folgenden Metriken hinzugefügt:   
    * Durchsatzmetriken für Kapazitätspools
        * *Zugeordneter Pool für Volumedurchsatz*
        * *Verbrauchter Durchsatz eines Pools*
        * *Prozentsatz des zugeordneten Pools für Volumedurchsatz*
        * *Prozentsatz des verbrauchten Durchsatzes eines Pools*
    * Durchsatzmetriken für Volumes
        * *Dem Volume zugeteilter Durchsatz*
        * *Verbrauchter Durchsatz eines Volumes*
        * *Prozentsatz des verbrauchten Durchsatzes eines Volumes*

* Unterstützung der [dynamischen Änderung des Servicelevels](dynamic-change-volume-service-level.md) von Replikationsvolumes   

    Azure NetApp Files unterstützt jetzt dynamische Änderungen des Servicelevels von Quell- und Zielvolumes einer Replikation.

## <a name="april-2021"></a>April 2021

* [Manuelle Volume- und Kapazitätspoolverwaltung](volume-quota-introduction.md) (hartes Kontingent) 

    Das Verhalten der Volume- und Kapazitätspoolbereitstellung von Azure NetApp Files wurde in einen manuellen und kontrollierbaren Mechanismus geändert. Die Speicherkapazität eines Volumes ist auf die festgelegte Größe (Kontingent) des Volumes beschränkt. Wenn der Volumeverbrauch ein Maximum erreicht, wächst weder das Volume noch der zugrunde liegende Kapazitätspool automatisch. Stattdessen erhält das Volume den Zustand „Kein Speicherplatz mehr“. Sie können jedoch [die Größe des Kapazitätspools oder eines Volumes nach Bedarf ändern](azure-netapp-files-resize-capacity-pools-or-volumes.md). Sie sollten die [Kapazität eines Volumes](monitor-volume-capacity.md) und den zugrunde liegenden Kapazitätspool aktiv überwachen.

    Diese Verhaltensänderung ist ein Ergebnis der folgenden Hauptanforderungen, die von vielen Benutzern gestellt wurden:

    * Vorher wurde VM-Clients nur die schlank zugewiesene Kapazität (100 TiB) eines bestimmten Volumes angezeigt, wenn der Betriebssystemspeicherplatz oder Kapazitätsüberwachungstools verwendet wurden.  Diese Situation konnte zu einer ungenauen Anzeige der Kapazität auf Client- oder Anwendungsseite führen. Dieses Verhalten wurde nun korrigiert.  
    * Das bisherige automatische Vergrößerungsverhalten von Kapazitätspools hat Anwendungsbesitzern keine Kontrolle über den bereitgestellten Kapazitätspoolspeicher (und die damit verbundenen Kosten) gegeben. Diese Situation war besonders in Umgebungen umständlich, in denen „Endlosprozesse“ ausgeführt wurden, die die bereitgestellte Kapazität schnell auslasten und so die bereitgestellte Kapazität erhöhen konnten. Dieses Verhalten wurde korrigiert.  
    * Benutzer möchten eine direkte Korrelation zwischen Volumegröße (Kontingent) und Leistung sehen. Das vorherige Verhalten ließ (implizite) Überabonnements eines Volumes (Kapazität) und die automatische Vergrößerung von Kapazitätspools zu. Daher konnten Benutzer erst dann eine direkte Korrelation herstellen, wenn das Volumekontingent aktiv festgelegt oder zurückgesetzt wurde. Dieses Verhalten wurde nun korrigiert.

    Benutzer haben die direkte Kontrolle über die bereitgestellte Kapazität gefordert. Benutzer möchten die Speicherkapazität und -auslastung kontrollieren und ausgleichen. Zudem möchten Sie die Kosten, die anwendungs- und clientseitige Sichtbarkeit der verfügbaren, genutzten und bereitgestellten Kapazität sowie die Leistung ihrer Anwendungsvolumes kontrollieren. Mit diesem neuen Verhalten wurden nun alle diese Funktionen aktiviert.

* [Unterstützung von SMB-Freigaben für fortlaufende Verfügbarkeit (Continuous Availability, CA) für FSLogix-Benutzerprofilcontainer](azure-netapp-files-create-volumes-smb.md#add-an-smb-volume) (Vorschau)  

    [FSLogix](/fslogix/overview) ist eine Zusammenstellung von Lösungen, mit denen sich nicht persistente Windows-Datenverarbeitungsumgebungen verbessern, aktivieren und vereinfachen lassen. FSLogix-Lösungen eignen sich für virtuelle Umgebungen in öffentlichen und privaten Clouds. Mit FSLogix-Lösungen können auch portierbarere Sitzungen erstellt werden, wenn physische Geräte zum Einsatz kommen. Mit FSLogix kann dynamischer Zugriff auf persistente Benutzerprofilcontainer bereitgestellt werden, die im freigegebenen SMB-Netzwerkspeicher gespeichert sind, einschließlich Azure NetApp Files. Um die FSLogix-Resilienz bei Wartungsereignissen des Speicherdiensts weiter zu verbessern, bietet Azure NetApp Files erweiterte Unterstützung für SMB Transparent Failover über [SMB-CA-Freigaben in Azure NetApp Files](azure-netapp-files-create-volumes-smb.md#add-an-smb-volume) für Benutzerprofilcontainer. Weitere Informationen finden Sie unter [Windows Virtual Desktop](azure-netapp-files-solution-architectures.md#windows-virtual-desktop).  

* [SMB3-Protokollverschlüsselung](azure-netapp-files-create-volumes-smb.md#add-an-smb-volume) (Vorschau) 

    Sie können jetzt für SMB-Volumes und Volumes mit dualem Protokoll von Azure NetApp Files die SMB3-Protokollverschlüsselung aktivieren. Mit diesem Feature wird die Verschlüsselung für In-Flight-SMB3-Daten mithilfe des [AES-CCM-Algorithmus für SMB 3.0-Verbindungen und des AES-GCM-Algorithmus für SMB 3.1.1-Verbindungen](/windows-server/storage/file-server/file-server-smb-overview#features-added-in-smb-311-with-windows-server-2016-and-windows-10-version-1607) aktiviert. SMB-Clients ohne Verwendung der SMB3-Verschlüsselung können nicht auf dieses Volume zugreifen. Ruhende Daten werden unabhängig von dieser Einstellung verschlüsselt. Die SMB-Verschlüsselung trägt zur weiteren Erhöhung der Sicherheit bei. Dies kann sich jedoch auf den Client auswirken (CPU-Auslastung für das Verschlüsseln und Entschlüsseln von Nachrichten). Es kann sich auch auf die Speicherressourcenverwendung auswirken (Reduzierung des Durchsatzes). Sie sollten die Auswirkung auf die Verschlüsselungsleistung Ihrer Anwendungen testen, bevor Sie Workloads in der Produktion bereitstellen.

* [Active Directory Domain Services (Adds) LDAP-Benutzerzuordnung mit erweiterten NFS-Gruppen](configure-ldap-extended-groups.md) (Vorschau)   

    Standardmäßig werden von Azure NetApp Files bis zu 16 Gruppen-IDs unterstützt, wenn NFS-Benutzeranmeldeinformationen so behandelt werden, wie in [RFC 5531](https://tools.ietf.org/html/rfc5531) definiert. Mit dieser neuen Funktion können Sie jetzt den maximalen Wert auf bis zu 1.024 erhöhen, wenn Sie über Benutzer verfügen, die Mitglieder in mehr als der Standardanzahl von Gruppen sind. Um diese Funktion zu unterstützen, können NFS-Volumes nun auch zu ADDS LDAP hinzugefügt werden, wodurch Active Directory LDAP-Benutzer mit erweiterten Gruppeneinträgen (mit bis zu 1024 Gruppen) auf das Volume zugreifen können. 

## <a name="march-2021"></a>März 2021
 
* [SMB-Freigaben für fortlaufende Verfügbarkeit (Continuous Availability, CA)](azure-netapp-files-create-volumes-smb.md#add-an-smb-volume) (Vorschau)  

    SMB Transparent Failover ermöglicht Wartungsvorgänge für den Azure NetApp Files-Dienst ohne Unterbrechung der Verbindung mit Serveranwendungen, die Daten auf SMB-Volumes speichern und auf diese Daten zugreifen. Zur Unterstützung von SMB Transparent Failover unterstützt Azure NetApp Files jetzt die Option für SMB-Freigaben für fortlaufende Verfügbarkeit zur Verwendung mit SQL Server-Anwendungen über SMB, die auf virtuellen Azure-Computern ausgeführt werden. Dieses Feature wird derzeit für SQL Server unter Windows unterstützt. SQL Server unter Linux wird zurzeit nicht unterstützt. Die Aktivierung dieses Features steigert die SQL Server-Leistung erheblich und bietet Skalierungs- und Kostenvorteile für [Bereitstellungen von einzelnen Instanzen, Always On-Failoverclusterinstanzen und Always On-Verfügbarkeitsgruppen](azure-netapp-files-solution-architectures.md#sql-server). Weitere Informationen finden Sie unter [Vorteile der Verwendung von Azure NetApp Files für die SQL Server-Bereitstellung](solutions-benefits-azure-netapp-files-sql-server.md).

* [Automatische Größenänderung eines regionsübergreifenden Replikationszielvolumes](azure-netapp-files-resize-capacity-pools-or-volumes.md#resize-a-cross-region-replication-destination-volume)

    In einer regionsübergreifenden Replikationsbeziehung wird die Größe eines Zielvolumes basierend auf der Größe des Quellvolumes automatisch angepasst. Daher müssen Sie die Größe des Zielvolumes nicht separat ändern. Dieses Verhalten zur automatischen Größenänderung gilt, wenn sich die Volumes in einer aktiven Replikationsbeziehung befinden oder wenn das Replikationspeering mit dem Vorgang zur Neusynchronisierung unterbrochen wird. Damit dieses Feature funktioniert, müssen Sie einen ausreichenden Toleranzbereich in den Kapazitätspools für die Quell- und Zielvolumes sicherstellen.

## <a name="december-2020"></a>Dezember 2020

* [Tool für konsistente Momentaufnahmen in Azure-Anwendungen](azacsnap-introduction.md) (Vorschau)    

    Das Tool für konsistente Momentaufnahmen in Azure-Anwendungen (Azure Application Consistent Snapshot Tool, AzAcSnap) ist ein Befehlszeilentool, mit dem Sie den Schutz von Daten für Drittanbieterdatenbanken (SAP HANA) in Linux-Umgebungen (z. B. SUSE und RHEL) vereinfachen können.   

    Für AzAcSnap werden die Funktionen für Volumemomentaufnahmen und die Replikation von Azure NetApp Files und Azure (große Instanz) genutzt. Ihnen bieten sich folgende Vorteile:

    * Anwendungskonsistenter Schutz von Daten 
    * Datenbankkatalog-Verwaltung 
    * *Ad-hoc*-Schutz von Volumes 
    * Klonen von Speichervolumes 
    * Unterstützung der Notfallwiederherstellung 

## <a name="november-2020"></a>November 2020

* [Momentaufnahme wiederherstellen](azure-netapp-files-manage-snapshots.md#revert-a-volume-using-snapshot-revert)

    Mit der Funktion zum Wiederherstellen von Momentaufnahmen können Sie schnell ein Volume wieder in den Zustand versetzen, in dem es sich befand, als eine bestimmte Momentaufnahme erstellt wurde. In den meisten Fällen ist das Wiederherstellen eines Volumes wesentlich schneller als das Wiederherstellen einzelner Dateien aus einer Momentaufnahme in das aktive Dateisystem. Im Vergleich zur Wiederherstellung einer Momentaufnahme auf einem neuen Volume ist es auch wesentliche effizienter hinsichtlich des Speicherplatzes.

## <a name="september-2020"></a>September 2020

* [Regionsübergreifende Replikation in Azure NetApp Files](cross-region-replication-introduction.md) (Vorschau)

  Azure NetApp Files unterstützt jetzt regionsübergreifende Replikation. Mit dieser neuen Notfallwiederherstellungsfunktion können Sie Ihre Azure NetApp Files-Volumes schnell und kostengünstig von einer Azure-Region in eine andere replizieren und so Ihre Daten vor unvorhersehbaren regionalen Ausfällen schützen. Die regionsübergreifende Replikation in Azure NetApp Files nutzt die SnapMirror®-Technologie, bei der nur geänderte Blöcke in einem komprimierten, effizienten Format über das Netzwerk gesendet werden. Diese proprietäre Technologie minimiert die Menge an Daten, die regionsübergreifend repliziert werden muss, wodurch Datenübertragungskosten eingespart werden. Auch die Replikationszeit verkürzt sich dadurch, sodass Sie eine kürzere Recovery Point Objective (RPO) erreichen können.

* [Manueller QoS-Kapazitätspool](manual-qos-capacity-pool-introduction.md) (Vorschau)  

    Bei einem manuellen QoS-Kapazitätspool können Sie die Kapazität und den Durchsatz für ein Volume unabhängig voneinander zuweisen. Der Gesamtdurchsatz aller Volumes, die mit einem Kapazitätspool mit dem QoS-Typ „Manuell“ erstellt werden, ist durch den Gesamtdurchsatz des Pools begrenzt. Dieser wird anhand der Kombination von Poolgröße und Serviceleveldurchsatz ermittelt. Alternativ kann der [QoS-Typ](azure-netapp-files-understand-storage-hierarchy.md#qos_types) eines Kapazitätspools „auto“ (automatisch) sein, wobei es sich um die Standardeinstellung handelt. In einem automatischen QoS-Kapazitätspool wird der Durchsatz den Volumes im Pool automatisch zugewiesen. Dies erfolgt proportional zum Größenkontingent, das den Volumes zugewiesen ist.

* [LDAP-Signatur](azure-netapp-files-create-volumes-smb.md) (Vorschau)   

    Azure NetApp Files unterstützt jetzt LDAP-Signatur für sichere LDAP-Lookups zwischen dem Azure NetApp Files-Dienst und den benutzerseitig angegebenen Active Directory Domain Services-Domänencontrollern. Diese Funktion steht derzeit als Vorschau zur Verfügung.

* [AES-Verschlüsselung für AD-Authentifizierung](create-active-directory-connections.md#create-an-active-directory-connection) (Vorschau)

    Azure NetApp Files unterstützt jetzt AES-Verschlüsselung bei LDAP-Verbindungen mit dem Domänencontroller, um die AES-Verschlüsselung für ein SMB-Volume zu ermöglichen. Diese Funktion steht derzeit als Vorschau zur Verfügung. 

* Neue [Metriken](azure-netapp-files-metrics.md):   

    * Neue Volumemetriken: 
        * *Zugeordnete Größe des Volumes*: Die bereitgestellte Größe eines Volumes
    * Neue Poolmetriken: 
        * *Zugeordnete Poolgröße*: Die bereitgestellte Größe des Pools. 
        * *Gesamtgröße der Momentaufnahme für den Pool*: Die Summe der Momentaufnahmegrößen aller Volumes in dem Pool.

## <a name="july-2020"></a>Juli 2020

* [Doppelprotokollvolume (NFSv3 und SMB)](create-volumes-dual-protocol.md)

    Sie können jetzt ein Azure NetApp Files-Volume erstellen, das den gleichzeitigen Doppelprotokollzugriff (NFS v3 und SMB) mit Unterstützung für LDAP-Benutzerzuordnung ermöglicht. Diese Funktion ermöglicht Anwendungsfälle, bei denen Sie eine Linux-basierte Workload haben können, die Daten in einem Azure NetApp Files-Volume generiert und speichert. Gleichzeitig müssen Ihre Mitarbeiter Windows-basierte Clients und Software verwenden, um die neu generierten Daten von demselben Azure NetApp Files-Volume zu analysieren. Durch die Funktion für den gleichzeitigen Doppelprotokollzugriff entfällt die Notwendigkeit, die von der Workload generierten Daten zur Nachanalyse mit einem anderen Protokoll auf ein separates Volume zu kopieren, was Speicherkosten und Betriebszeit spart. Diese Funktion ist kostenlos (die normalen [Azure NetApp Files-Speicherkosten](https://azure.microsoft.com/pricing/details/netapp/) fallen weiterhin an) und allgemein verfügbar. Weitere Informationen finden Sie in der [Dokumentation zum gleichzeitigen Doppelprotokollzugriff](create-volumes-dual-protocol.MD).

* [NFS v4.1 Kerberos-Verschlüsselung während der Übertragung](configure-kerberos-encryption.MD)

    Azure NetApp Files unterstützt jetzt die NFS-Clientverschlüsselung in Kerberos-Modi (krb5, krb5i und krb5p) mit AES-256-Verschlüsselung. Dies bedeutet für Sie eine Erhöhung der Datensicherheit. Diese Funktion ist kostenlos (die normalen [Azure NetApp Files-Speicherkosten](https://azure.microsoft.com/pricing/details/netapp/) fallen weiterhin an) und allgemein verfügbar. Weitere Informationen finden Sie in der [Dokumentation zur NFS v4.1 Kerberos-Verschlüsselung](configure-kerberos-encryption.MD).

* [Dynamische Änderung des Servicelevels von Volumes](dynamic-change-volume-service-level.MD) (Vorschauversion) 

    Die Cloud verspricht Flexibilität bei den IT-Ausgaben. Sie können den Servicelevel eines vorhandenen Azure NetApp Files-Volumes jetzt ändern, indem Sie das Volume in einen anderen Kapazitätspool verschieben, der den von Ihnen für das Volume gewünschten Servicelevel verwendet. Diese direkte Änderung des Servicelevels für das Volume erfordert keine Migration von Daten. Auch der Zugriff auf das Volume auf Datenebene wird hiervon nicht beeinträchtigt. Sie können ein vorhandenes Volume so ändern, dass es einen höheren Servicelevel verwendet, um die Leistung zu verbessern, oder so dass es einen niedrigeren Servicelevel verwendet, um die Kosten zu optimieren. Diese Funktion ist kostenlos (die normalen [Azure NetApp Files-Speicherkosten](https://azure.microsoft.com/pricing/details/netapp/) fallen weiterhin an). Sie befindet sich derzeit in der Vorschauphase. Sie können sich für die Featurevorschau registrieren, indem Sie der [Dokumentation zur dynamischen Änderung des Servicelevels](dynamic-change-volume-service-level.md) folgen.

* [Volumemomentaufnahme-Richtlinie](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies) (Vorschau) 

    Mit Azure NetApp Files können Sie Zeitpunktmomentaufnahmen Ihrer Volumes erstellen. Sie können jetzt eine Momentaufnahmerichtlinie erstellen, damit Azure NetApp Files automatisch Volumemomentaufnahmen mit einer Häufigkeit Ihrer Wahl erstellt. Sie können die Erstellung der Momentaufnahmen in stündlichen, täglichen, wöchentlichen oder monatlichen Zyklen planen. Außerdem können Sie als Bestandteil der Momentaufnahmerichtlinie die maximale Anzahl aufzubewahrender Momentaufnahmen angeben. Diese Funktion ist kostenlos (die normalen [Azure NetApp Files-Speicherkosten](https://azure.microsoft.com/pricing/details/netapp/) fallen weiterhin an) und befindet sich derzeit in der Vorschauphase. Sie können sich für die Featurevorschau registrieren, indem Sie der [Dokumentation für Volumemomentaufnahme-Richtlinien](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies) folgen.

* [Exportrichtlinie mit NFS-Stammzugriff](azure-netapp-files-configure-export-policy.md)

    Sie können in Azure NetApp Files jetzt angeben, ob das Stammkonto auf das Volume zugreifen kann. 

* [Momentaufnahmepfad ausblenden](azure-netapp-files-manage-snapshots.md#restore-a-file-from-a-snapshot-using-a-client)

    Sie können in Azure NetApp Files jetzt angeben, ob ein Benutzer das Verzeichnis `.snapshot` (NFS-Clients) oder den Ordner `~snapshot` (SMB-Clients) auf einem eingebundenen Volume anzeigen und darauf zugreifen kann.

## <a name="may-2020"></a>Mai 2020

* [Microsoft Azure Backup-Richtlinienbenutzer](create-active-directory-connections.md) (Vorschau)

    Sie können in Azure NetApp Files jetzt weitere Konten einschließen, die erhöhte Rechte für das für Azure NetApp Files erstellte Computerkonto erfordern. Die angegebenen Konten dürfen die NTFS-Berechtigungen auf Datei- oder Ordnerebene ändern. Beispielsweise können Sie ein nicht privilegiertes Dienstkonto angeben, das zum Migrieren von Daten zu einer SMB-Dateifreigabe in Azure NetApp Files verwendet wird. Das Feature Sicherungsrichtlinienbenutzer steht derzeit als Vorschau zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte
* [Was ist Azure NetApp Files?](azure-netapp-files-introduction.md)
* [Grundlegendes zur Speicherhierarchie von Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md) 
