---
title: Includedatei
description: Includedatei
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 04/09/2019
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: eae4b1e919270413d4ca6627964fad9c7f5bd9bf
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59804122"
---
1. Verwenden Sie das `dps-keygen`-Befehlszeilen-Hilfsprogramm, um eine Verbindungszeichenfolge zu generieren:

    Führen Sie zum Installieren des [Schlüsselgenerator-Hilfsprogramms](https://github.com/Azure/dps-keygen) den folgenden Befehl aus:

    ```cmd/sh
    npm i -g dps-keygen
    ```

1. Um eine Verbindungszeichenfolge zu generieren, führen Sie den folgenden Befehl mit den Details der Verbindung aus, die Sie zuvor notiert haben:

    ```cmd/sh
    dps-keygen -di:<Device ID> -dk:<Primary or Secondary Key> -si:<Scope ID>
    ```

1. Kopieren Sie die Verbindungszeichenfolge aus der `dps-keygen`-Ausgabe, um sie in Ihrem Gerätecode zu verwenden.