---
title: 'Azure Data Factory-Datenfluss: Verschieben von Knoten'
description: Verschieben von Knoten in einem Azure Data Factory-Datenflussdiagramm
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: d82b32e876144a626333f3df1481c5fce9862067
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2019
ms.locfileid: "56732176"
---
# <a name="mapping-data-flow-move-nodes"></a>Mapping Data Flow: Verschieben von Knoten

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Optionen für die Transformation zum Aggregieren](media/data-flow/agghead.png "Aggregatorheader")

Die Azure Data Factory-Datenfluss-Entwurfsoberfläche ist eine „Konstruktionsoberfläche“, mit der Sie Datenflüsse von oben nach unten und von links nach rechts erstellen können. Jeder Transformation mit einem Pluszeichen (+) ist eine Toolbox angefügt. Konzentrieren Sie sich auf Ihre Geschäftslogik, statt auf das Verbinden von Knoten über Edges in einer Freiform-DAG-Umgebung.

Ohne ein Drag & Drop-Paradigma besteht die Möglichkeit zum „Verschieben“ eines Transformationsknotens also darin, den eingehenden Datenstrom zu ändern. Stattdessen verschieben Sie Transformationen, indem Sie den „eingehenden Datenstrom“ ändern.

In Azure Data Factory-Datenfluss stellen Datenströme den Fluss der Daten dar. Im Bereich der Transformationseinstellungen wird ein Feld „Eingehender Datenstrom“ angezeigt. Daraus geht hervor, welcher eingehende Datenstrom die Transformation speist. Sie können den physischen Speicherort Ihres Transformationsknotens im Diagramm ändern, indem Sie auf den Namen des eingehenden Datenstroms klicken und einen anderen Datenstrom auswählen. Dann werden die aktuelle Transformation sowie alle nachfolgenden Transformationen in diesem Datenstrom an den neuen Speicherort verschoben.

Wenn Sie eine Transformation mit einer oder mehreren nachfolgenden Transformationen verschieben, wird der neue Speicherort im Datenfluss über eine neue Verzweigung verknüpft.

Wenn es nach dem von Ihnen ausgewählten Knoten keine nachfolgenden Transformationen gibt, wird nur diese Transformation an den neuen Speicherort verschoben.
