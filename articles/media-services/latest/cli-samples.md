---
title: 'Azure CLI-Beispiele: Azure Media Services | Microsoft-Dokumentation'
description: Azure CLI-Beispiele für den Azure Media Services-Dienst
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 03/11/2019
ms.author: juliako
ms.openlocfilehash: dee7f831562dc1f4b2478d13b204aab1d8455e1e
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57840629"
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Azure CLI-Beispiele für Azure Media Services

Die folgende Tabelle enthält Links zu Azure CLI-Beispielen für Azure Media Services.

## <a name="examples"></a>Beispiele

|  |  |
|---|---|
|**Skalieren**||
| [Skalieren reservierter Einheiten für Medien](media-reserved-units-cli-how-to.md)|Für die Audioanalyse- und Videoanalyseaufträge, die von Media Services v3 oder Video Indexer ausgelöst werden, wird dringend empfohlen, Ihr Konto mit zehn S3-MRUs bereitzustellen. <br/>Das Skript zeigt, wie Sie mit der Befehlszeilenschnittstelle reservierte Einheiten für Medien (MRUs) skalieren.|
|**Konto**||
| [Erstellen eines Media Services-Kontos](create-account-cli-how-to.md) | Das Skript erstellt ein Azure Media Services-Konto. |
| [Zurücksetzen der Kontoanmeldeinformationen](./scripts/cli-reset-account-credentials.md)|Setzt Ihre Kontoanmeldeinformationen zurück und stellt die Einstellungen von „app.config“ wieder her.|
|**Medienobjekte**||
| [Erstellen von Medienobjekten](./scripts/cli-create-asset.md)|Erstellt ein Media Services-Medienobjekt, in das Inhalt hochgeladen werden kann.|
| [Hochladen einer Datei](./scripts/cli-upload-file-asset.md)|Lädt eine lokale Datei in einen Speichercontainer hoch.|
| **Transformationen** und **Aufträge**||
| [Erstellen von Transformationen](./scripts/cli-create-transform.md)|Zeigt, wie Transformationen erstellt werden. Transformationen beschreiben einen einfachen Workflow von Aufgaben zur Verarbeitung Ihrer Video- oder Audiodateien (der häufig als „Rezept“ bezeichnet wird).<br/> Sie müssen immer überprüfen, ob eine Transformation mit dem gewünschten Namen und „Rezept“ bereits vorhanden ist. Wenn dies der Fall ist, verwenden Sie diese erneut. |
| [Codieren mit einer benutzerdefinierten Transformation](custom-preset-cli-howto.md) | Zeigt das Erstellen einer benutzerdefinierten Voreinstellung für Ihr spezielles Szenario oder Ihre Geräteanforderungen.|
| [Erstellen von Aufträgen](./scripts/cli-create-jobs.md)|Übermittelt einen Auftrag mithilfe einer HTTPS-URL an eine einfache Codierungstransformation.|
| [Erstellen von EventGrid](./scripts/cli-create-event-grid.md)|Erstellt ein Event Grid-Abonnement für Änderungen des Auftragsstatus auf Kontoebene.|
| **Bereitstellen**||
| [Veröffentlichen eines Medienobjekts](./scripts/cli-publish-asset.md)| Erstellt einen Streaminglocator und erhält Streaming-URLs zurück. |
| [Filter](filters-dynamic-manifest-cli-howto.md)| konfiguriert einen Filter für ein Video-on-Demand-Medienobjekt und zeigt, wie mithilfe der Befehlszeilenschnittstelle [Kontofilter](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) und [Medienobjektfilter](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest) erstellt werden. 

## <a name="see-also"></a>Weitere Informationen

- [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
- [Schnellstart: Streamen von Videodateien: CLI](stream-files-cli-quickstart.md)
