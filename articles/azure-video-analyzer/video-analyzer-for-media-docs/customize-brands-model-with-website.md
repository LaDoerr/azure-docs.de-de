---
title: Anpassen eines Markenmodells mit der Website von Azure Video Analyzer for Media (früher Video Indexer)
titleSuffix: Azure Media Services
description: Erfahren Sie, wie Sie ein Markenmodell mit der Website von Azure Video Analyzer for Media (früher Video Indexer) anpassen.
services: media-services
author: anikaz
manager: johndeu
ms.topic: article
ms.date: 12/15/2019
ms.author: kumud
ms.openlocfilehash: 7efb7d434771f7c4f1e8c40c37fdc3fa9b7a45e6
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385737"
---
# <a name="customize-a-brands-model-with-the-video-analyzer-for-media-website"></a>Anpassen eines Markenmodells mit der Video Analyzer for Media-Website

Azure Video Analyzer for Media (früher Video Indexer) unterstützt die Markenerkennung aus Sprache und visuellem Text während der Indizierung und Neuindizierung von Video- und Audioinhalten. Das Feature zur Erkennung von Marken identifiziert Erwähnungen von Produkten, Diensten und Unternehmen, die von der Markendatenbank von Bing vorgeschlagen werden. Wenn Microsoft beispielsweise in Video- oder Audioinhalten erwähnt wird oder im visuellen Text in einem Video angegeben wird, erkennt Video Analyzer for Media dies als Marke im Inhalt.

Ein benutzerdefiniertes Markenmodell ermöglicht Ihnen Folgendes:

- Wählen Sie diese Option aus, wenn Video Analyzer for Media Marken aus der Markendatenbank von Bing erkennen soll.
- Wählen Sie diese Option aus, wenn Video Analyzer for Media bestimmte Marken von der Erkennung ausschließen soll (im Wesentlichen Erstellen einer Markenablehnungsliste).
- Wählen Sie diese Option aus, wenn Video Analyzer for Media bestimmte, möglicherweise nicht in der Markendatenbank von Bing enthaltene Marken einschließen soll, die Teil Ihres Modells sein sollten (im Wesentlichen Erstellen einer Markenakzeptanzliste).

Eine ausführliche Übersicht finden Sie unter [Übersicht](customize-brands-model-overview.md).

Sie können die Video Analyzer for Media-Website zum Erstellen, Verwenden und Bearbeiten von benutzerdefinierten Markenmodellen verwenden, die in einem Video erkannt wurden, wie in diesem Thema beschrieben. Sie können auch die API verwenden. Dies wird unter [Anpassen von Markenmodellen mithilfe von APIs](customize-brands-model-with-api.md) beschrieben.

> [!NOTE]
> Wenn Ihr Video vor dem Hinzufügen einer Marke indiziert wurde, müssen Sie es neu indizieren. Sie finden das **Neu indizieren**-Element im Dropdownmenü, das dem Video zugeordnet ist. Wählen Sie **Erweiterte Optionen** -> **Markenkategorien** aus, und aktivieren Sie **Alle Marken**.

## <a name="edit-brands-model-settings"></a>Bearbeiten der Einstellungen für das Markenmodell

Sie haben die Möglichkeit festzulegen, ob Marken aus der Bing-Markendatenbank erkannt werden sollen oder nicht. Um diese Option festzulegen, müssen Sie die Einstellungen des Markenmodells bearbeiten. Folgen Sie diesen Schritten:

1. Navigieren Sie zur [Video Analyzer for Media](https://www.videoindexer.ai/)-Website und melden Sie sich an.
1. Um ein Modell in Ihrem Konto anzupassen, wählen Sie links auf der Seite die Schaltfläche **Anpassung des Inhaltsmodells** aus.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/content-model-customization/content-model-customization.png" alt-text="Anpassen eines Inhaltsmodells in Video Analyzer for Media":::
1. Um Marken zu bearbeiten, wählen Sie die Registerkarte **Marken** aus.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/customize-brand-model/customize-brand-model.png" alt-text="Der Screenshot zeigt die Registerkarte „Marken“ des Dialogfelds „Anpassung des Inhaltsmodells“":::.
1. Aktivieren Sie die Option **Von Bing vorgeschlagene Marken anzeigen**, wenn Sie möchten, dass Video Analyzer for Media von Bing vorgeschlagenen Marken erkennt – lassen Sie die Option andernfalls deaktiviert.

## <a name="include-brands-in-the-model"></a>Einbeziehen von Marken in das Modell

Der Abschnitt **Marken einschließen** stellt benutzerdefinierte Marken dar, die Video Analyzer for Media auch dann erkennen soll, wenn sie nicht von Bing vorgeschlagen werden.  

### <a name="add-a-brand-to-include-list"></a>Hinzufügen einer Marke zur Einschlussliste

1. Wählen Sie **+ Neue Marke erstellen** aus.

    Geben Sie einen Namen (erforderlich), eine Kategorie (optional), eine Beschreibung (optional) und eine Verweis-URL (optional) an.
    Das Feld „Kategorie“ soll Sie beim Kennzeichnen Ihrer Marken unterstützen. Dieses Feld wird als *Tags* der Marke angezeigt, wenn die Video Analyzer for Media-APIs verwendet werden. Beispielsweise kann die Marke „Azure“ als „Cloud“ gekennzeichnet oder eingestuft werden.

    Das Feld für die Verweis-URL kann eine beliebige Referenzwebsite für die Marke sein (z. B. ein Link zur entsprechenden Wikipedia-Seite).

2. Wählen Sie **Speichern** aus. Sie sehen dann, dass die Marke der Liste **Marken einschließen** hinzugefügt wurde.

### <a name="edit-a-brand-on-the-include-list"></a>Bearbeiten einer Marke in der Einschlussliste

1. Wählen Sie das Stiftsymbol neben der Marke aus, die Sie bearbeiten möchten.

    Sie können die Kategorie, die Beschreibung oder die Verweis-URL einer Marke aktualisieren. Sie können den Namen einer Marke nicht ändern, weil Namen von Marken eindeutig sind. Wenn Sie den Markennamen ändern müssen, löschen Sie die gesamte Marke (siehe nächster Abschnitt,) und erstellen Sie dann eine neue Marke mit dem neuen Namen.

2. Wählen Sie die Schaltfläche **Aktualisieren** aus, um die Marke mit den neuen Informationen zu aktualisieren.

### <a name="delete-a-brand-on-the-include-list"></a>Löschen einer Marke in der Einschlussliste

1. Wählen Sie das Papierkorbsymbol neben der Marke aus, die Sie löschen möchten.
2. Wählen Sie **Löschen** aus. Die Marke wird nicht mehr in der Liste *Marken einschließen* angezeigt.

## <a name="exclude-brands-from-the-model"></a>Ausschließen von Marken aus dem Modell

Der Abschnitt **Marken ausschließen** stellt die Marken dar, die Video Analyzer for Media nicht erkennen soll.

### <a name="add-a-brand-to-exclude-list"></a>Hinzufügen einer Marke zur Ausschlussliste

1. Wählen Sie **+ Neue Marke erstellen** aus.

    Geben Sie einen Namen (erforderlich) und eine Kategorie (optional) an.

2. Wählen Sie **Speichern** aus. Sie sehen dann, dass die Marke der Liste *Marken ausschließen* hinzugefügt wurde.

### <a name="edit-a-brand-on-the-exclude-list"></a>Bearbeiten einer Marke in der Ausschlussliste

1. Wählen Sie das Stiftsymbol neben der Marke aus, die Sie bearbeiten möchten.

    Sie können nur die Kategorie eine Marke aktualisieren. Sie können den Namen einer Marke nicht ändern, weil Namen von Marken eindeutig sind. Wenn Sie den Markennamen ändern müssen, löschen Sie die gesamte Marke (siehe nächster Abschnitt,) und erstellen Sie dann eine neue Marke mit dem neuen Namen.

2. Wählen Sie die Schaltfläche **Aktualisieren** aus, um die Marke mit den neuen Informationen zu aktualisieren.

### <a name="delete-a-brand-on-the-exclude-list"></a>Löschen einer Marke in der Ausschlussliste

1. Wählen Sie das Papierkorbsymbol neben der Marke aus, die Sie löschen möchten.
2. Wählen Sie **Löschen** aus. Die Marke wird nicht mehr in der Liste *Marken ausschließen* angezeigt.

## <a name="next-steps"></a>Nächste Schritte

[Anpassen des Markenmodells mit APIs](customize-brands-model-with-api.md)
