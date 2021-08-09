---
title: Konfiguration von ADDS LDAP mit erweiterten Gruppen für Azure NetApp-Dateien für NFS-Volumes | Microsoft-Dokumentation
description: Beschreibt die Überlegungen und Schritte zum Aktivieren von LDAP mit erweiterten Gruppen, wenn Sie mit Azure NetApp-Dateien ein NFS-Volume erstellen.
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
ms.topic: how-to
ms.date: 05/20/2021
ms.author: b-juche
ms.openlocfilehash: 680563e0688d11e02f79f3c184b38d5a80bafeb0
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110480355"
---
# <a name="configure-adds-ldap-with-extended-groups-for-nfs-volume-access"></a>Konfigurieren von ADDS LDAP mit erweiterten Gruppen für Zugriff auf NFS-Volumes

Wenn Sie [ein NFS-Volume erstellen](azure-netapp-files-create-volumes.md), haben Sie die Möglichkeit, die Funktion LDAP mit erweiterten Gruppen (**LDAP**-Option) für das Volume zu aktivieren. Mit dieser Funktion können Active Directory LDAP-Benutzer und erweiterte Gruppen (bis zu 1024 Gruppen) auf das Volume zugreifen. Sie können LDAP mit dem Feature für erweiterte Gruppen sowohl mit den NFSv4.1- als auch mit den NFSv3-Volumes verwenden. 

Dieser Artikel erklärt die Überlegungen und Schritte zum Aktivieren von LDAP mit erweiterten Gruppen, wenn Sie ein NFS-Volume erstellen.  

## <a name="considerations"></a>Überlegungen

* LDAP mit erweiterten Gruppen wird nur mit Active Directory Domain Services (AD DS) oder Azure Active Directory Domain Services (AADDS) unterstützt. OpenLDAP oder andere LDAP-Verzeichnisdienste von Drittanbietern werden nicht unterstützt. 

* LDAP über TLS darf *nicht* aktiviert werden, wenn Sie Azure Active Directory Domain Services (AADDS) verwenden.  

* Wenn Sie die LDAP mit erweiterten Gruppenfeature aktivieren, werden LDAP-aktivierte [Kerberos-Volumes](configure-kerberos-encryption.md) den Dateibesitz für LDAP-Benutzer nicht ordnungsgemäß anzeigen. Eine Datei oder ein Verzeichnis, die/das von einem LDAP-Benutzer erstellt wurde, wird standardmäßig als Besitzer `root` und nicht als der tatsächliche LDAP-Benutzer verwendet. Allerdings kann `root` das Konto den Dateibesitz mithilfe des Befehls manuell ändern `chown <username> <filename>`. 

* Nachdem Sie das Volume erstellt haben, können Sie die Einstellung der LDAP-Option (aktiviert oder deaktiviert) nicht mehr ändern.  

* In der folgenden Tabelle wird die Gültigkeitsdauer (Time to Live, TTL) für den LDAP-Cache beschrieben. Sie müssen warten, bis der Cache aktualisiert wird, bevor Sie versuchen, über einen Client auf eine Datei oder ein Verzeichnis zuzugreifen. Andernfalls wird auf dem Client eine Meldung vom Typ „Zugriff verweigert“ angezeigt. 

    |     Fehlerzustand    |     Lösung    |
    |-|-|
    | Cache |  Standardzeitlimit |
    | Gruppenmitgliedschaftsliste  | 24 Stunden Gültigkeitsdauer  |
    | Unix-Gruppen  | 24 Stunden Gültigkeitsdauer, 1 Minute negative Gültigkeitsdauer  |
    | UNIX-Benutzer  | 24 Stunden Gültigkeitsdauer, 1 Minute negative Gültigkeitsdauer  |

    Caches haben einen bestimmten Timeoutzeitraum namens *Gültigkeitsdauer*. Nach Ablauf des Timeouts werden Einträge als veraltet eingestuft, damit veraltete Einträge nicht gesperrt werden. Der *negative TTL* -Wert ist der Ort, an dem die Suche fehlgeschlagen ist, um Leistungsprobleme aufgrund von LDAP-Abfragen für Objekte zu vermeiden, die möglicherweise nicht vorhanden sind.“        

## <a name="steps"></a>Schritte

1. Das Feature LDAP mit erweiterten Gruppen befindet sich derzeit in der Vorschauphase. Wenn Sie diese Funktion zum ersten Mal verwenden, müssen Sie sie zuerst registrieren:  

    1. Registrieren Sie die Funktion:   

        ```azurepowershell-interactive
        Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFLdapExtendedGroups
        ```

    2. Überprüfen Sie den Status der Funktionsregistrierung: 

        > [!NOTE]
        > Der **RegistrationState** kann für bis zu 60 Minuten den Status `Registering` aufweisen, bevor der Wechsel in `Registered` erfolgt. Warten Sie, bis der Status `Registered` lautet, bevor Sie fortfahren.

        ```azurepowershell-interactive
        Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFLdapExtendedGroups
        ```
        
    Sie können auch die [Azure CLI-Befehle](/cli/azure/feature) `az feature register` und `az feature show` verwenden, um das Feature zu registrieren und den Registrierungsstatus anzuzeigen. 

2. LDAP-Volumes erfordern eine Active Directory Konfiguration für LDAP-Servereinstellungen. Befolgen Sie die Anweisungen unter [Anforderungen für Active Directory-Verbindungen](create-active-directory-connections.md#requirements-for-active-directory-connections) und [erstellen Sie eine Active Directory Verbindung](create-active-directory-connections.md#create-an-active-directory-connection), um Active Directory Verbindungen auf dem Azure-Portal zu konfigurieren.  

    > [!NOTE]
    > Stellen Sie sicher, dass Sie die Active Directory-Verbindungseinstellungen konfiguriert haben. Ein Computerkonto wird in der Organisationseinheit (OE) erstellt, die in den Active Directory-Verbindungseinstellungen angegeben ist. Die Einstellungen werden vom LDAP-Client für die Authentifizierung bei Active Directory verwendet.

3. Stellen Sie sicher, dass der Active Directory LDAP-Server auf dem Active Directory ausgeführt wird. 

4. LDAP-NFS-Benutzer müssen bestimmte POSIX-Attribute auf dem LDAP-Server haben. Legen Sie die Attribute für LDAP-Benutzer und LDAP-Gruppen wie folgt fest: 

    * Erforderliche Attribute für LDAP-Benutzer:   
        `uid: Alice`, `uidNumber: 139`, `gidNumber: 555`, `objectClass: user`
    * Erforderliche Attribute für LDAP-Gruppen:   
        `objectClass: group`, `gidNumber: 555`

    Sie können POSIX-Attribute verwalten, indem Sie das MMC-Snap-In „Active Directory-Benutzer und -Computer“ verwenden Im folgenden Beispiel ist der Active Directory-Attribut-Editor dargestellt:  

    ![Active Directory-Attribut-Editor](../media/azure-netapp-files/active-directory-attribute-editor.png) 

    Ausführliche Informationen finden Sie unter [Zugreifen auf den Active Directory-Attribut-Editor](create-volumes-dual-protocol.md#access-active-directory-attribute-editor).  

5. Wenn Sie einen mit LDAP integrierten NFSv4.1-Client von Linux konfigurieren möchten, finden Sie weitere Informationen unter [Konfigurieren eines NFS-Clients für Azure NetApp Files](configure-nfs-clients.md).

6.  Führen Sie die unter [Erstellen eines NFS-Volumes für Azure NetApp Files](azure-netapp-files-create-volumes.md) angegebenen Schritte aus, um das NFS-Volume zu erstellen. Aktivieren Sie während der Volumeerstellung auf der Registerkarte **Protokoll** die **LDAP**-Option.   

    ![Screenshot, der das Erstellen einer Volumeseite mit LDAP-Option zeigt.](../media/azure-netapp-files/create-nfs-ldap.png)  

7. Optional: Sie können lokale NFS-Client Benutzer, die auf dem Windows LDAP-Server nicht vorhanden sind, für den Zugriff auf ein NFS-Volume aktivieren, das über aktivierte LDAP mit erweiterten Gruppen verfügt. Aktivieren Sie hierzu die Option **lokale NFS-Benutzer mit LDAP zulassen** wie folgt:
    1. Klicken Sie auf **Active Directory-Verbindungen**.  Klicken Sie in einer vorhandenen Active Directory-Verbindung auf das Kontextmenü (die drei Punkte `…`), und wählen Sie **Bearbeiten** aus.  
    2. Wählen Sie im angezeigten Fenster **Active Directory-Einstellungen bearbeiten** die Option **lokale NFS-Benutzer mit LDAP zulassen** aus.  

    ![Screenshot, der die Option lokale NFS-Benutzer mit LDAP zulassen anzeigt](../media/azure-netapp-files/allow-local-nfs-users-with-ldap.png)  

## <a name="next-steps"></a>Nächste Schritte  

* [Erstellen eines NFS-Volumes für Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [Erstellen und Verwalten von Active Directory-Verbindungen](create-active-directory-connections.md)
* [Behandeln von Problemen mit LDAP-Volumes](troubleshoot-ldap-volumes.md)
