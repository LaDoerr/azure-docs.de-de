---
title: Konvertieren von Zertifikatinhalten in Base64 – Azure HDInsight
description: Konvertieren der Inhalte eines Dienstprinzipalzertifikats in ein Base64-codiertes Zeichenfolgenformat in Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/31/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: 1af6136e5d09c512ad515871cd7cbfceaf1877a2
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112299907"
---
# <a name="converting-service-principal-certificate-contents-to-base-64-encoded-string-format-in-hdinsight"></a>Konvertieren der Inhalte eines Dienstprinzipalzertifikats in ein Base64-codiertes Zeichenfolgenformat in HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Sie erhalten eine Fehlermeldung, dass die Eingabe keine gültige Basis64-Zeichenfolge ist, da sie ein Zeichen, das nicht Base64-codiert ist, mehr als zwei Füllzeichen oder ein Nicht-Leerzeichen zwischen den Füllzeichen enthält.

## <a name="cause"></a>Ursache

Wenn Sie PowerShell oder die Azure-Vorlagenbereitstellung verwenden, um Cluster mit Data Lake als primären oder zusätzlichen Speicher zu erstellen, weist der Inhalt des Dienstprinzipalzertifikats für den Zugriff auf das Data Lake Storage-Konto das Base64-Format auf. Eine fehlerhafte Konvertierung des Inhalts des PFX-Zertifikats in eine Base64-codierte Zeichenfolge kann zu diesem Fehler führen.

## <a name="resolution"></a>Lösung

Wenn das Dienstprinzipalzertifikat im PFX-Format vorliegt (Beispielschritte zum Erstellen von Dienstprinzipalen finden Sie [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.hdinsight/hdinsight-datalake-store-azure-storage)), können Sie den folgenden PowerShell-Befehl oder C#-Codeausschnitt verwenden, um den Inhalt des Zertifikats in das Base64-Format zu konvertieren.

```powershell
$servicePrincipalCertificateBase64 = [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes(path-to-servicePrincipalCertificatePfxFile))
```

```csharp
using System;
using System.IO;

namespace ConsoleApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            var certContents = File.ReadAllBytes(@"<path to pfx file>");
            string certificateData = Convert.ToBase64String(certContents);
            System.Diagnostics.Debug.WriteLine(certificateData);
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [troubleshooting next steps](../includes/hdinsight-troubleshooting-next-steps.md)]