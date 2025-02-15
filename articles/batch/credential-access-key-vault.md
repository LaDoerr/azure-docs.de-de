---
title: Verwenden von Zertifikaten und sicheres Zugreifen auf Azure Key Vault mit Batch
description: Erfahren Sie, wie Sie mit Azure Batch programmgesteuert auf Ihre Anmeldeinformationen aus Key Vault zugreifen können.
ms.topic: how-to
ms.date: 08/25/2021
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 20af753813ead55a3107b952a56d92b16135b523
ms.sourcegitcommit: dcf1defb393104f8afc6b707fc748e0ff4c81830
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2021
ms.locfileid: "123098963"
---
# <a name="use-certificates-and-securely-access-azure-key-vault-with-batch"></a>Verwenden von Zertifikaten und sicheres Zugreifen auf Azure Key Vault mit Batch

In diesem Artikel erfahren Sie, wie Sie Batch-Knoten einrichten, um sicher auf die in [Azure Key Vault](../key-vault/general/overview.md) gespeicherten Anmeldeinformationen zuzugreifen.

Sie benötigen Folgendes, um Azure Key Vault über einen Batch-Knoten zu authentifizieren:

- Eine Azure Active Directory-Anmeldeinformation (Azure AD)
- Ein Zertifikat
- Ein Batch-Konto
- Einen Batch-Pool mit mindestens einem Knoten

> [!IMPORTANT]
> Batch bietet ab sofort eine verbesserte Option für den Zugriff auf Anmeldeinformationen, die in Azure Key Vault gespeichert sind. Wenn Sie Ihren Pool mit einer benutzerseitig zugewiesenen verwalteten Identität erstellen, die auf das Zertifikat in Azure Key Vault zugreifen kann, müssen Sie den Zertifikatsinhalt nicht an den Batch-Dienst senden. Dies erhöht die Sicherheit. Es wird empfohlen, anstelle der in diesem Thema beschriebenen Methode die automatische Zertifikatrotation zu verwenden. Weitere Informationen erhalten Sie unter [Aktivieren der automatischen Zertifikatrotation in einem Batch-Pool](automatic-certificate-rotation.md).

## <a name="obtain-a-certificate"></a>Beschaffung eines Zertifikats

Wenn Sie noch kein Zertifikat besitzen, ist der einfachste Weg, eines zu erhalten, ein selbstsigniertes Zertifikat mit dem Befehlszeilentool `makecert` zu generieren.

Sie finden `makecert` normalerweise in diesem Pfad: `C:\Program Files (x86)\Windows Kits\10\bin\<arch>`. Öffnen Sie eine Eingabeaufforderung als Administrator, und navigieren Sie anhand des folgenden Beispiels zu `makecert`.

```console
cd C:\Program Files (x86)\Windows Kits\10\bin\x64
```

Als nächstes verwenden Sie das Tool `makecert`, um selbstsignierte Zertifikatsdateien mit den Namen `batchcertificate.cer` und `batchcertificate.pvk` zu erstellen. Der verwendete allgemeine Name (CN) ist für diese Anwendung nicht wichtig, aber es ist hilfreich, ihn so zu wählen, dass ersichtlich ist, wofür das Zertifikat verwendet wird.

```console
makecert -sv batchcertificate.pvk -n "cn=batch.cert.mydomain.org" batchcertificate.cer -b 09/23/2019 -e 09/23/2019 -r -pe -a sha256 -len 2048
```

Batch erfordert eine `.pfx`-Datei. Verwenden Sie das Tool [pvk2pfx](/windows-hardware/drivers/devtest/pvk2pfx), um die von `makecert` erstellten `.cer`- und `.pvk`-Dateien in eine einzelne `.pfx`-Datei zu konvertieren.

```console
pvk2pfx -pvk batchcertificate.pvk -spc batchcertificate.cer -pfx batchcertificate.pfx -po
```

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

Der Zugriff auf Key Vault wird entweder einem **Benutzer** oder einem **Dienstprinzipal** gewährt. Um programmgesteuert auf Key Vault zuzugreifen, verwenden Sie einen [Dienstprinzipal](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) mit dem Zertifikat, das Sie im vorherigen Schritt erstellt haben. Der Dienstprinzipal muss sich in demselben Azure AD-Mandanten befinden wie Key Vault.

```powershell
$now = [System.DateTime]::Parse("2020-02-10")
# Set this to the expiration date of the certificate
$expirationDate = [System.DateTime]::Parse("2021-02-10")
# Point the script at the cer file you created $cerCertificateFilePath = 'c:\temp\batchcertificate.cer'
$cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$cer.Import($cerCertificateFilePath)
# Load the certificate into memory
$credValue = [System.Convert]::ToBase64String($cer.GetRawCertData())
# Create a new AAD application that uses this certificate
$newADApplication = New-AzureRmADApplication -DisplayName "Batch Key Vault Access" -HomePage "https://batch.mydomain.com" -IdentifierUris "https://batch.mydomain.com" -certValue $credValue -StartDate $now -EndDate $expirationDate
# Create new AAD service principal that uses this application
$newAzureAdPrincipal = New-AzureRmADServicePrincipal -ApplicationId $newADApplication.ApplicationId
```

Die URLs für die Anwendung sind nicht wichtig, da wir sie nur für den Zugriff auf Key Vault verwenden.

## <a name="grant-rights-to-key-vault"></a>Gewähren von Rechten für Key Vault

Der im vorherigen Schritt erstellte Dienstprinzipal erfordert die Berechtigung, die Geheimnisse aus Key Vault abzurufen. Die Berechtigung kann entweder über das [Azure-Portal](../key-vault/general/assign-access-policy-portal.md) oder mit dem nachfolgenden PowerShell-Befehl erteilt werden.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'BatchVault' -ServicePrincipalName '"https://batch.mydomain.com' -PermissionsToSecrets 'Get'
```

## <a name="assign-a-certificate-to-a-batch-account"></a>Zuweisen eines Zertifikats zu einem Batch-Konto

Erstellen Sie einen Batch-Pool, wechseln Sie dann zur Registerkarte „Zertifikat“ im Pool, und weisen Sie das erstellte Zertifikat zu. Das Zertifikat befindet sich jetzt auf allen Batch-Knoten.

Als Nächstes müssen wir das Zertifikat dem Batch-Konto zuordnen. Die Zuweisung des Zertifikats zum Konto ermöglicht Azure Batch, es den Pools und dann den Knoten zuzuweisen. Am einfachsten geht das, indem Sie im Portal zu Ihrem Batch-Konto wechseln, zu **Zertifikate** navigieren und **Hinzufügen** auswählen. Laden Sie die `.pfx`-Datei hoch, die Sie zuvor generiert haben, und geben Sie das Kennwort an. Nachdem der Vorgang abgeschlossen ist, wird das Zertifikat der Liste hinzugefügt, und Sie können den Fingerabdruck überprüfen.

Wenn Sie nun einen Batch-Pool erstellen, können Sie innerhalb des Pools zu **Zertifikate** navigieren und das von Ihnen erstellte Zertifikat diesem Pool zuweisen. Stellen Sie in diesem Fall sicher, dass Sie **LocalMachine** für den Speicherort auswählen. Das Zertifikat wird auf alle Batch-Knoten im Pool geladen.

## <a name="install-azure-powershell"></a>Installieren von Azure Powershell

Wenn Sie planen, mit PowerShell-Skripts auf Ihren Knoten auf Key Vault zuzugreifen, dann müssen Sie die Azure PowerShell-Bibliothek installieren. Wenn auf Ihren Knoten das Windows Management Framework 5 (WMF) installiert ist, können Sie es mit dem Befehl „install-module“ herunterladen. Wenn Sie Knoten verwenden, die nicht über WMF 5 verfügen, ist es am einfachsten, die `.msi`-Datei von Azure PowerShell mit Ihren Batch-Dateien zu bündeln und dann das Installationsprogramm als ersten Teil Ihres Batch-Startskripts aufzurufen. Weitere Informationen finden Sie in diesem Beispiel:

```powershell
$psModuleCheck=Get-Module -ListAvailable -Name Azure -Refresh
if($psModuleCheck.count -eq 0) {
    $psInstallerPath = Join-Path $downloadPath "azure-powershell.3.4.0.msi" Start-Process msiexec.exe -ArgumentList /i, $psInstallerPath, /quiet -wait
}
```

## <a name="access-key-vault"></a>Zugreifen auf Key Vault

Jetzt sind Sie bereit, in Skripts, die auf Ihren Batch-Knoten ausgeführt werden, auf Key Vault zuzugreifen. Um von einem Skript aus auf Key Vault zuzugreifen, brauchen Sie nur Ihr Skript mithilfe des Zertifikats für Azure AD zu authentifizieren. Verwenden Sie dazu in PowerShell die folgenden Beispielbefehle. Geben Sie die entsprechende GUID für **Fingerabdruck**, **Anwendungs-ID** (die ID Ihres Dienstprinzipals) und **Mandanten-ID** (der Mandant, in dem Ihr Dienstprinzipal vorhanden ist) an.

```powershell
Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint -ApplicationId
```

Nach der Authentifizierung können Sie wie gewohnt auf Key Vault zugreifen.

```powershell
$adminPassword=Get-AzureKeyVaultSecret -VaultName BatchVault -Name batchAdminPass
```

Dies sind die Anmeldeinformationen, die in Ihrem Skript verwendet werden sollen.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Azure Key Vault](../key-vault/general/overview.md)
- Informieren Sie sich über die [Azure-Sicherheitsbaseline für Batch](security-baseline.md).
- Erfahren Sie mehr über Batch-Features, z. B. [Konfigurieren des Zugriffs auf Computeknoten](pool-endpoint-configuration.md) [mithilfe von Linux-Computeknoten](batch-linux-nodes.md) und [privater Endpunkte](private-connectivity.md).
