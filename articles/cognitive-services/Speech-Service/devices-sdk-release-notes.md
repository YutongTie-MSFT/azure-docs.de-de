---
title: Dokumentation zum Speech-Geräte-SDK
titleSuffix: Azure Cognitive Services
description: Versionshinweise – Änderungen in den neuesten Versionen
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 08/07/2018
ms.author: gracez
ms.openlocfilehash: 4d802b9f71edee1eec4b2c92881e2a8796db2865
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58005507"
---
# <a name="release-notes-of-cognitive-services-speech-devices-sdk"></a>Versionshinweise zum Cognitive Services Speech-Geräte-SDK

In den folgenden Abschnitten sind Änderungen in den neuesten Versionen aufgeführt.

## <a name="cognitive-services-speech-devices-sdk-131-2019-mar-release"></a>Cognitive Services Speech Devices SDK 1.3.1: Release März 2019 

* Die [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference)-Komponente wurde auf Version 1.3.1 aktualisiert. Weitere Informationen finden Sie in den [Versionshinweisen](https://aka.ms/csspeech/whatsnew). 
*   Aktualisierte Aktivierungswortverarbeitung, siehe „Wichtige Änderungen“.
*   Beispielanwendung fügt die Auswahl der Sprache für die Spracherkennung und Übersetzung hinzu.

**Wichtige Änderungen** 

*   [Das Installieren eines Aktivierungsworts](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg#run-a-sample-application) wurde vereinfacht und ist jetzt Teil der App. Es ist keine separate Installation auf dem Gerät erforderlich.
*   Die Erkennung des Aktivierungsworts wurde geändert, und es werden zwei Ereignisse unterstützt.
    - RecognizingKeyword: Gibt an, dass das Ergebnis der Spracherkennung (nicht überprüften) Schlüsselworttext enthält.
    - RecognizedKeyword: Gibt an, dass die Schlüsselworterkennung abgeschlossen und das angegebene Schlüsselwort erkannt wurde.


## <a name="cognitive-services-speech-devices-sdk-110-2018-nov-release"></a>Cognitive Services Speech Devices SDK 1.1.0: Version 2018-Nov 

* Die [Speech-SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference)-Komponente wurde auf Version 1.1.0 aktualisiert. Weitere Informationen finden Sie in den [Versionshinweisen](https://aka.ms/csspeech/whatsnew). 
* Die Spracherkennungsgenauigkeit im Fernfeld wird mit unserem erweiterten Audioverarbeitungsalgorithmus verbessert.
* Der Beispielanwendung wurde Unterstützung für die Erkennung von Chinesisch hinzugefügt.

## <a name="cognitive-services-speech-devices-sdk-101-2018-oct-release"></a>Cognitive Services Speech Devices SDK 1.0.1: Version 2018-Oct 

* [Speech-SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference)-Komponente wurde auf Version 1.0.1 aktualisiert. Weitere Informationen finden Sie in den [Versionshinweisen](https://aka.ms/csspeech/whatsnew). 
* Die Spracherkennungsgenauigkeit wird mit unserem verbesserten Audioverarbeitungsalgorithmus verbessert.  
* Ein Fehler bei der kontinuierlichen Erkennung von Audiositzungen wurde behoben.

**Wichtige Änderungen** 

* Mit diesem Release werden einige Breaking Changes eingeführt. Ausführliche Informationen zu den APIs finden Sie auf [dieser Seite](https://aka.ms/csspeech/breakingchanges_1_0_0). 
* Die KWS-Modelldateien sind nicht mit dem Speech-Geräte-SDK 1.0.1 kompatibel. Vorhandene Aktivierungswörter werden nach dem Schreiben der neuen Aktivierungswörter auf dem Gerät gelöscht. 

## <a name="cognitive-services-speech-devices-sdk-050-2018-aug-release"></a>Cognitive Services Speech Devices SDK 0.5.0: Version 2018-Aug

* Die Genauigkeit der Spracherkennung wurde verbessert, indem ein Fehler im Code für die Audiodatenverarbeitung behoben wurde.
* [Speech-SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference)-Komponente wurde auf Version 0.5.0 aktualisiert. Weitere Informationen finden Sie in den [Versionshinweisen](releasenotes.md#cognitive-services-speech-sdk-050-2018-july-release).

## <a name="cognitive-services-speech-devices-sdk-0212733-2018-may-release"></a>Cognitive Services Speech Devices SDK 0.2.12733: Version 2018-May

Die erste öffentliche Vorschauversion des Cognitive Services Speech-Geräte-SDK.
