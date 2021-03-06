---
title: 'Tutorial: Moderieren von Facebook-Inhalten: Content Moderator'
titlesuffix: Azure Cognitive Services
description: In diesem Tutorial wird beschrieben, wie Sie Content Moderator auf Machine Learning-Basis verwenden, um Facebook-Posts und -Kommentare zu moderieren.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: tutorial
ms.date: 01/18/2019
ms.author: pafarley
ms.openlocfilehash: 662eca2a727f3112f169ab8d669bf18c81700275
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57871027"
---
# <a name="tutorial-moderate-facebook-posts-and-commands-with-azure-content-moderator"></a>Tutorial: Moderieren von Facebook-Posts und -Kommentaren mit Azure Content Moderator

In diesem Tutorial wird beschrieben, wie Sie Azure Content Moderator verwenden, um Posts und Kommentare auf einer Facebook-Seite zu moderieren. Facebook sendet die von Besuchern geposteten Inhalte an den Content Moderator-Dienst. Über Ihre Content Moderator-Workflows werden entweder die Inhalte veröffentlicht, oder es werden mit dem entsprechenden Tool Bewertungen erstellt. Dies richtet sich nach den Inhaltspunktzahlen und Schwellenwerten. Das [Demovideo von der Build 2017](https://channel9.msdn.com/Events/Build/2017/T6033) enthält ein funktionsfähiges Beispiel für dieses Szenario.

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Erstellen eines Content Moderator-Teams
> * Erstellen von Azure-Funktionen, die auf HTTP-Ereignisse von Content Moderator und Facebook lauschen
> * Verknüpfen einer Facebook-Seite mit Content Moderator über eine Facebook-Anwendung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

In diesem Diagramm sind die einzelnen Komponenten dieses Szenarios dargestellt:

![Diagramm: Content Moderator empfängt Informationen von Facebook über „FBListener“ und sendet Informationen über „CMListener“](images/tutorial-facebook-moderation.png)

## <a name="prerequisites"></a>Voraussetzungen

- Ein Content Moderator-Abonnementschlüssel. Gehen Sie wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um den Content Moderator-Dienst zu abonnieren und Ihren Schlüssel zu erhalten.
- Ein [Facebook-Konto](https://www.facebook.com/).

## <a name="create-a-review-team"></a>Erstellen eines Prüfungsteams

Informationen zum Registrieren für das [Content Moderator-Prüfungstool](https://contentmoderator.cognitive.microsoft.com/) sowie zum Erstellen eines Prüfungsteams finden Sie unter [Schnellstart: Testen von Content Moderator im Web](quick-start.md). Notieren Sie sich auf der Seite **Anmeldeinformationen** die **Team-ID**.

## <a name="configure-image-moderation-workflow"></a>Konfigurieren des Workflows für die Bildmoderation

Informationen zur Erstellung eines benutzerdefinierten Bildworkflows finden Sie unter [Definieren, Testen und Verwenden von Workflows](review-tool-user-guide/workflows.md). So wird es für Content Moderator ermöglicht, Bilder automatisch auf Facebook zu überprüfen und einige davon an das Prüfungstool zu senden. Notieren Sie sich den **Namen** des Workflows.

## <a name="configure-text-moderation-workflow"></a>Konfigurieren des Workflows für die Textmoderation

Verwenden Sie wieder den Leitfaden [Definieren, Testen und Verwenden von Workflows](review-tool-user-guide/workflows.md), und erstellen Sie nun einen benutzerdefinierten Textworkflow. Content Moderator kann dann automatisch den Textinhalt überprüfen. Notieren Sie sich den **Namen** des Workflows.

![Konfigurieren des Textworkflows](images/text-workflow-configure.PNG)

Testen Sie Ihren Workflow mit der Schaltfläche **Workflow ausführen**.

![Testen des Textworkflows](images/text-workflow-test.PNG)

## <a name="create-azure-functions"></a>Erstellen von Azure-Funktionen

Melden Sie sich am [Azure-Portal](https://portal.azure.com/) an, und führen Sie diese Schritte aus:

1. Erstellen Sie eine Azure-Funktionen-App gemäß der Anleitung auf der Seite [Erstellen einer Funktionen-App im Azure-Portal](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal).
2. Navigieren Sie zur neu erstellten Funktions-App.
3. Navigieren Sie in der App zur Registerkarte **Plattformfeatures**, und wählen Sie **Anwendungseinstellungen**. Scrollen Sie im Abschnitt **Anwendungseinstellungen** der nächsten Seite an das Ende der Liste, und klicken Sie auf **Neue Einstellung hinzufügen**. Fügen Sie die folgenden Schlüssel-Wert-Paare hinzu.
    
    | Name der App-Einstellung | value   | 
    | -------------------- |-------------|
    | cm:TeamId   | Ihre Content Moderator-Team-ID  | 
    | cm:SubscriptionKey | Ihr Content Moderator-Abonnementschlüssel – Siehe [Anmeldeinformationen](review-tool-user-guide/credentials.md) | 
    | cm:Region | Ihr Content Moderator-Regionsname, ohne Leerzeichen. Siehe vorherigen Hinweis. |
    | cm:ImageWorkflow | Name des Workflows für Bilder |
    | cm:TextWorkflow | Name des Workflows für Text |
    | cm:CallbackEndpoint | URL für die CMListener-Funktionen-App, die Sie später in diesem Leitfaden erstellen |
    | fb:VerificationToken | Das geheime Token, das auch zum Abonnieren der Facebook-Feedereignisse verwendet wird |
    | fb:PageAccessToken | Das Zugriffstoken für die Facebook-Graph-API läuft nicht ab und lässt die Funktion zum Ausblenden/Löschen von Posts in Ihrem Namen zu |

    Klicken Sie oben auf der Seite auf die Schaltfläche **Speichern**.

1. Verwenden Sie die Schaltfläche **+** im linken Bereich, um den Bereich „Neue Funktion“ anzuzeigen.

    ![Azure Functions-Bereich mit hervorgehobener Schaltfläche „Funktion hinzufügen“](images/new-function.png)

    Klicken Sie anschließend oben auf der Seite auf **+ Neue Funktion**. Diese Funktion empfängt Ereignisse von Facebook. Erstellen Sie diese Funktion, indem Sie folgenden Schritte ausführen:

    1. Klicken Sie auf die Kachel **HTTP-Trigger**.
    1. Geben Sie den Namen **FBListener** ein. Das Feld **Autorisierungsstufe** sollte auf **Funktion** festgelegt sein.
    1. Klicken Sie auf **Create**.
    1. Ersetzen Sie den Inhalt von **run.csx** durch den Inhalt von **FbListener/run.csx**.

    [!code-csharp[FBListener: csx file](~/samples-fbPageModeration/FbListener/run.csx?range=1-160)]

1. Erstellen Sie eine neue Funktion vom Typ **HTTP-Trigger** mit dem Namen **CMListener**. Diese Funktion empfängt Ereignisse von Content Moderator. Ersetzen Sie den Inhalt von **run.csx** durch den Inhalt von **CMListener/run.csx**.

    [!code-csharp[FBListener: csx file](~/samples-fbPageModeration/CmListener/run.csx?range=1-106)]

---

## <a name="configure-the-facebook-page-and-app"></a>Konfigurieren der Facebook-Seite und der App
1. Erstellen Sie eine Facebook-App.

    ![Seite für Facebook-Entwickler](images/facebook-developer-app.png)

    1. Navigieren Sie zur [Facebook-Entwicklerwebsite](https://developers.facebook.com/).
    2. Klicken Sie auf **My Apps** (Meine Apps).
    3. Fügen Sie eine neue App hinzu.
    1. Geben Sie ihr einen Namen.
    1. Wählen Sie **Webhooks > Set Up** (Webhooks > Einrichten).
    1. Wählen Sie im Dropdownmenü die Option **Page** (Seite), und wählen Sie **Subscribe to this object** (Objekt abonnieren).
    1. Geben Sie die **FBListener Url** als Rückruf-URL und das von Ihnen in den **Funktionen-App-Einstellungen** konfigurierte **Überprüfungstoken** an.
    1. Scrollen Sie nach dem Abonnieren nach unten zum Feed, und wählen Sie **Subscribe** (Abonnieren).

2. Erstellen Sie eine Facebook-Seite.

    1. Navigieren Sie zu [Facebook](https://www.facebook.com/bookmarks/pages), und erstellen Sie eine **neue Facebook-Seite**.
    2. Lassen Sie für die Facebook-App den Zugriff auf diese Seite zu, indem Sie die folgenden Schritte ausführen:
        1. Navigieren Sie zum [Graph-API-Explorer](https://developers.facebook.com/tools/explorer/).
        2. Wählen Sie die Option **Anwendung**.
        3. Wählen Sie die Option **Page Access Token** (Seitenzugriffstoken), und senden Sie eine **Get**-Anforderung.
        4. Klicken Sie in der Antwort auf die **Page ID** (Seiten-ID).
        5. Fügen Sie jetzt **/subscribed_apps** an die URL an, und senden Sie eine **Get**-Anforderung (leere Antwort).
        6. Übermitteln Sie eine **POST**-Anforderung. Sie erhalten eine Antwort der Form **success: true**.

3. Erstellen Sie ein nicht ablaufendes Graph-API-Zugriffstoken.

    1. Navigieren Sie zum [Graph-API-Explorer](https://developers.facebook.com/tools/explorer/).
    2. Wählen Sie die Option **Application** (Anwendung).
    3. Wählen Sie die Option **Get User Access Token** (Benutzerzugriffstoken abrufen).
    4. Wählen Sie unter **Select Permissions** (Berechtigungen auswählen) die Optionen **manage_pages** und **publish_pages**.
    5. Wir verwenden das **Zugriffstoken** (kurzlebiges Token) im nächsten Schritt.

4. Für die nächsten Schritte verwenden wir Postman.

    1. Öffnen Sie **Postman** (oder rufen Sie die Anwendung [hier](https://www.getpostman.com/) ab).
    2. Importieren Sie diese beiden Dateien:
        1. [Postman-Sammlung](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/Facebook%20Permanant%20Page%20Access%20Token.postman_collection.json)
        2. [Postman-Umgebung](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FB%20Page%20Access%20Token%20Environment.postman_environment.json)       
    3. Aktualisieren Sie diese Umgebungsvariablen:
    
        | Schlüssel | Wert   | 
        | -------------------- |-------------|
        | appId   | Fügen Sie hier Ihre Facebook-App-ID ein.  | 
        | appSecret | Fügen Sie hier Ihr Facebook-App-Geheimnis ein. | 
        | short_lived_token | Fügen Sie das kurzlebige Benutzerzugriffstoken ein, das Sie im vorherigen Schritt generiert haben. |
    4. Führen Sie nun die drei APIs aus, die in der Sammlung aufgeführt sind: 
        1. Wählen Sie **Generate Long-Lived Access Token** (Langlebiges Zugriffstoken generieren), und klicken Sie auf **Send** (Senden).
        2. Wählen Sie **Get User ID** (Benutzer-ID abrufen), und klicken Sie auf **Send** (Senden).
        3. Wählen Sie **Get Permanent Page Access Token** (Dauerhaftes Seitenzugriffstoken abrufen), und klicken Sie auf **Send** (Senden).
    5. Kopieren Sie den Wert für **access_token** aus der Antwort, und weisen Sie ihn der App-Einstellung **fb:PageAccessToken** zu.

Die Lösung sendet alle Bilder und Texte, die auf Ihrer Facebook-Seite gepostet werden, an Content Moderator. Jetzt werden die Workflows aufgerufen, die Sie zuvor konfiguriert haben. Die Inhalte, die die in den Workflows definierten Kriterien nicht erfüllen, werden an Überprüfungsfunktionen im Prüfungstool übergeben. Der Rest des Inhalts wird automatisch veröffentlicht.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein Programm für die Produktbildanalyse eingerichtet, um sie mit produkttypspezifischen Tags zu versehen und ein Prüfungsteam beim Treffen fundierter Entscheidungen für die Inhaltsmoderation zu unterstützen. Machen Sie sich als Nächstes mit den Details der Bildmoderation vertraut.

> [!div class="nextstepaction"]
> [Bildmoderation](./image-moderation-api.md)
