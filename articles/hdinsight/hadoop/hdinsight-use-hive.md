---
title: Was sind Apache Hive und HiveQL? – Azure HDInsight
description: Apache Hive ist ein Data Warehouse-System für Apache Hadoop. Sie können in Hive gespeicherte Daten mithilfe von HiveQL abrufen, die Transact-SQL ähnelt. Erfahren Sie in diesem Dokument, wie Sie Hive und HiveQL mit Azure HDInsight verwenden.
keywords: HiveQL, was ist Hive, Hadoop HiveQL, Verwenden von Hive, Hive lernen, was ist Hive
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 03/26/2019
ms.openlocfilehash: 1f0746436fa980b6becfa7a88560734aa07a54e2
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58801928"
---
# <a name="what-is-apache-hive-and-hiveql-on-azure-hdinsight"></a>Was sind Apache Hive und HiveQL in Azure HDInsight?

[Apache Hive](https://hive.apache.org/) ist ein Data Warehouse-System für Apache Hadoop. Hive ermöglicht das Zusammenfassen, Abfragen und Analysen von Daten. Hive-Abfragen werden in HiveQL geschrieben, einer SQL-ähnlichen Abfragesprache.

Hive ermöglicht Ihnen die Strukturierung größtenteils unstrukturierter Daten. Nachdem Sie die Struktur definiert haben, können Sie mit HiveQL Daten abfragen, ohne Java- oder MapReduce-Kenntnisse zu besitzen.

HDInsight bietet verschiedene Clustertypen, die für bestimmte Workloads optimiert sind. Die folgenden Clustertypen werden am häufigsten für Hive-Abfragen verwendet:

* __Interactive Query:__ Hadoop-Cluster mit [LLAP-Funktionalität (Low Latency Analytical Processing)](https://cwiki.apache.org/confluence/display/Hive/LLAP) zur Verbesserung der Antwortzeiten für interaktive Abfragen. Weitere Informationen finden Sie unter [Start with Interactive Query in HDInsight (Einstieg in Interactive Query in HDInsight)](../interactive-query/apache-interactive-query-get-started.md).

* __Hadoop:__ Hadoop-Cluster, der für die Batchverarbeitung von Workloads optimiert ist. Weitere Informationen finden Sie unter [Einstieg in Apache Hadoop in HDInsight](../hadoop/apache-hadoop-linux-tutorial-get-started.md).

* __Spark__: Apache Spark verfügt über integrierte Funktionen für die Arbeit mit Hive. Weitere Informationen finden Sie unter [Einstieg in Apache Spark in HDInsight](../spark/apache-spark-jupyter-spark-sql.md).

* __HBase__: HiveQL kann zum Abfragen von Daten verwendet werden, die in Apache HBase gespeichert sind. Weitere Informationen finden Sie unter [Einstieg in Apache HBase in HDInsight](../hbase/apache-hbase-tutorial-get-started-linux.md).

## <a name="how-to-use-hive"></a>Verwenden von Hive

In der folgenden Tabelle finden Sie Informationen zu unterschiedlichen Methoden der Verwendung von Hive mit HDInsight:

| **Verwenden Sie diese Methode** für ... | ...**interaktive** Abfragen | ...**Batchverarbeitung** | ...von diesem **Clusterbetriebssystem** |
|:--- |:---:|:---:|:--- |:--- |
| [HDInsight Tools for Visual Studio Code](../hdinsight-for-vscode.md) |✔ |✔ | Linux, Unix, Mac OS X oder Windows |
| [HDInsight-Tools für Visual Studio](../hadoop/apache-hadoop-use-hive-visual-studio.md) |✔ |✔ |Windows |
| [Struktur anzeigen](../hadoop/apache-hadoop-use-hive-ambari-view.md) |✔ |✔ |Alle (browserbasiert) |
| [Beeline-Client](../hadoop/apache-hadoop-use-hive-beeline.md) |✔ |✔ |Linux, Unix, Mac OS X oder Windows |
| [REST-API](../hadoop/apache-hadoop-use-hive-curl.md) |&nbsp; |✔ |Linux, Unix, Mac OS X oder Windows |
| [Windows PowerShell](../hadoop/apache-hadoop-use-hive-powershell.md) |&nbsp; |✔ |Windows |


## <a name="hiveql-language-reference"></a>Referenz zu HiveQL

Die Referenz zu HiveQL ist im [Sprachhandbuch (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) verfügbar.

## <a name="hive-and-data-structure"></a>Hive und Datenstruktur

Hive kann mit strukturierten und halbstrukturierten Daten arbeiten. Dazu gehören z.B. Textdateien, in denen die Felder durch bestimmte Zeichen getrennt sind. Die folgende HiveQL-Anweisung erstellt eine Tabelle aus Daten, die durch Leerzeichen getrennt sind:

```hiveql
CREATE EXTERNAL TABLE log4jLogs (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
```

Hive unterstützt auch benutzerdefinierte **Serialisierer/Deserialisierer (SerDe)** für komplexe oder unregelmäßig strukturierte Daten. Weitere Informationen finden Sie im Artikel [Verwenden eines benutzerdefinierten JSON-SerDe mit HDInsight](https://web.archive.org/web/20190217104719/ https://blogs.msdn.microsoft.com/bigdatasupport/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight/).

Weitere Informationen zu von Hive unterstützten Dateiformaten finden Sie im [Sprachhandbuch (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).

### <a name="hive-internal-tables-vs-external-tables"></a>Interne und externe Hive-Tabellen

Sie können mit Hive zwei Arten von Tabellen erstellen:

* __Intern:__ Die Daten werden im Hive-Data Warehouse gespeichert. Dieses Data Warehouse befindet sich unter `/hive/warehouse/` im Standardspeicher für den Cluster.

    Verwenden Sie interne Tabellen, wenn eine der folgenden Bedingungen zutrifft:

    * Die Daten sind temporär.
    * Hive soll den Lebenszyklus der Tabelle und der Daten verwalten.

* __Extern:__ Die Daten werden außerhalb des Data Warehouse gespeichert. Die Daten können auf einem beliebigen Speicher gespeichert werden, der vom Cluster aus erreichbar ist.

    Verwenden Sie externe Tabellen, wenn eine der folgenden Bedingungen zutrifft:

    * Die Daten werden auch außerhalb von Hive verwendet. Beispielsweise werden die Datendateien von einem anderen Prozess aktualisiert (der die Dateien nicht sperrt).
    * Die Daten müssen am zugrunde liegenden Speicherort verbleiben – auch nach dem Löschen der Tabelle.
    * Sie benötigen einen benutzerdefinierten Speicherort, z.B. ein anderes Speicherkonto als das Standardkonto.
    * Ein anderes Programm als Hive verwaltet das Datenformat, den Speicherort usw.

Weitere Informationen finden Sie im Blogbeitrag [Einführung in interne und externe Tabellen in Hive][cindygross-hive-tables].

## <a name="user-defined-functions-udf"></a>Benutzerdefinierte Funktionen (User Defined Functions, UDFs)

Hive kann auch über **benutzerdefinierte Funktionen (UDF)** erweitert werden. Mit einer UDF-Datei können Sie Funktionen oder Logik implementieren, die sich nicht einfach in HiveQL modellieren lässt. Ein Beispiel für benutzerdefinierte Funktionen mit Hive finden Sie in den folgenden Artikeln:

* [Verwenden einer benutzerdefinierten Java-Funktion mit Apache Hive](../hadoop/apache-hadoop-hive-java-udf.md)

* [Verwenden einer benutzerdefinierten Python-Funktion mit Apache Hive](../hadoop/python-udf-hdinsight.md)

* [Verwenden einer benutzerdefinierten C#-Funktion mit Apache Hive](../hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Hinzufügen einer benutzerdefinierten Apache Hive-Funktion zu HDInsight](https://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Beispiel einer benutzerdefinierten Apache Hive-Funktion für das Umwandeln von Datums-/Uhrzeitformaten in Hive-Zeitstempel](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a id="data"></a>Beispieldaten

Hive für HDInsight bietet eine vorab geladene, interne Tabelle mit dem Namen `hivesampletable`. HDInsight stellt auch Beispieldatasets bereit, die mit Hive verwendet werden können. Diese Datasets sind in den Verzeichnissen `/example/data` und `/HdiSamples` gespeichert. Diese Verzeichnisse befinden sich im Standardspeicher für den Cluster.

## <a id="job"></a>Hive-Beispielabfrage

Die folgenden HiveQL-Anweisungen projizieren Spalten in die Datei `/example/data/sample.log`:

```hiveql
DROP TABLE log4jLogs;
CREATE EXTERNAL TABLE log4jLogs (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs 
    WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' 
    GROUP BY t4;
```

Im vorherigen Beispiel führen die HiveQL-Anweisungen die folgenden Aktionen aus:


* `DROP TABLE`: Wenn die Tabelle bereits vorhanden ist, wird sie gelöscht.

* `CREATE EXTERNAL TABLE`: Erstellt eine neue **externe** Tabelle in Hive. Externe Tabellen speichern nur die Tabellendefinition in Hive. Die Daten verbleiben an ihrem ursprünglichen Speicherort und behalten das Originalformat bei.

* `ROW FORMAT`: Teilt Hive mit, wie die Daten formatiert werden. In diesem Fall werden die Felder in den einzelnen Protokollen durch Leerzeichen getrennt.

* `STORED AS TEXTFILE LOCATION`: Teilt Hive den Speicherort der Daten (das Verzeichnis `example/data`) mit und dass die Speicherung als Text erfolgt. Die Daten können sich in einer Datei befinden oder auf mehrere Dateien im Verzeichnis verteilt sein.

* `SELECT`: Wählt die Anzahl aller Zeilen aus, bei denen die Spalte **t4** den Wert **[ERROR]** enthält. Mit dieser Anweisung wird der Wert **3** zurückgegeben, da dieser Wert in drei Zeilen enthalten ist.

* `INPUT__FILE__NAME LIKE '%.log'`: Hive versucht, das Schema auf alle Dateien im Verzeichnis anzuwenden. In diesem Fall enthält das Verzeichnis Dateien, die dem Schema nicht entsprechen. Um überflüssige Daten in den Ergebnissen zu vermeiden, weist diese Anweisung Hive an, nur Daten aus Dateien zurückzugeben, die auf „.log“ enden.

> [!NOTE]  
> Externe Tabellen sollten Sie verwenden, wenn Sie erwarten, dass die zugrunde liegenden Daten aus einer externen Quelle aktualisiert werden. Das könnte z.B. ein automatisierter Datenupload oder ein MapReduce-Vorgang sein.
>
> Durch das Löschen einer externen Tabelle werden **nicht** die Daten, sondern nur die Tabellendefinitionen gelöscht.

Zum Erstellen einer **internen** anstelle einer externen Tabelle, verwenden Sie den folgenden HiveQL-Code:

```hiveql
CREATE TABLE IF NOT EXISTS errorLogs (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
STORED AS ORC;
INSERT OVERWRITE TABLE errorLogs
SELECT t1, t2, t3, t4, t5, t6, t7 
    FROM log4jLogs WHERE t4 = '[ERROR]';
```

Diese Anweisungen führen die folgenden Aktionen aus:

* `CREATE TABLE IF NOT EXISTS`: Wenn die Tabelle nicht vorhanden ist, wird sie erstellt. Da das Schlüsselwort **EXTERNAL** nicht verwendet wird, erstellt diese Anweisung eine interne Tabelle. Die Tabelle wird im Hive-Data Warehouse gespeichert und vollständig von Hive verwaltet.

* `STORED AS ORC`: Speichert die Daten im ORC-Format (Optimized Row Columnar). ORC ist ein stark optimiertes und effizientes Format zum Speichern von Hive-Daten.

* `INSERT OVERWRITE ... SELECT`: Wählt Zeilen in der Tabelle **log4jLogs** aus, die **[ERROR]** enthalten, und fügt die Daten in die Tabelle **errorLogs** ein.

> [!NOTE]  
> Anders als bei externen Tabellen werden beim Löschen von internen Tabellen auch die zugrunde liegenden Daten gelöscht.

## <a name="improve-hive-query-performance"></a>Verbessern der Hive-Abfrageleistung

### <a id="usetez"></a>Apache Tez

[Apache Tez](https://tez.apache.org) ist ein Framework, mit dem datenintensive Anwendungen wie Hive wesentlich effizienter ausgeführt und skaliert werden können. „Tez“ ist standardmäßig aktiviert.  Die [Apache Hive mit Tez-Design-Dokumente](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) enthalten Details zu Implementierungsoptionen und Konfigurationseinstellungen.

### <a name="low-latency-analytical-processing-llap"></a>Low Latency Analytical Processing (LLAP)

[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (auch als „Live Long and Process“ bezeichnet) ist ein neues Feature in Hive 2.0, das die Zwischenspeicherung von Abfragen im Arbeitsspeicher ermöglicht. LLAP macht Hive-Abfragen deutlich schneller – bis zu [in einigen Fällen 26-mal schneller als Hive 1.x](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).

HDInsight stellt LLAP im Clustertyp „Interactive Query“ bereit. Weitere Informationen finden Sie unter [Start with Interactive Query (Einstieg in Interactive Query](../interactive-query/apache-interactive-query-get-started.md).

## <a name="scheduling-hive-queries"></a>Planen von Hive-Abfragen

Es gibt mehrere Dienste, mit denen Hive-Abfragen als Teil eines geplanten oder bedarfsgesteuerten Workflows ausgeführt werden können.

### <a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory ermöglicht Ihnen, HDInsight als Teil einer Data Factory-Pipeline zu verwenden. Weitere Informationen zum Verwenden von Hive aus einer Pipeline finden Sie im Dokument [Transformieren von Daten mit Hive-Aktivitäten in Azure Data Factory](../../data-factory/transform-data-using-hadoop-hive.md).

### <a name="hive-jobs-and-sql-server-integration-services"></a>Hive-Aufträge und SQL Server Integration Services

Sie können mit SQL Server Integration Services (SSIS) einen Hive-Auftrag ausführen. Das Azure Feature Pack für SSIS bietet die folgenden Komponenten, die mit Hive-Aufträgen in HDInsight funktionieren.

* [Hive-Aufgabe in Azure HDInsight][hivetask]

* [Verbindungs-Manager für Azure-Abonnements][connectionmanager]

Weitere Informationen finden Sie in der Dokumentation des [Azure Feature Pack][ssispack].

### <a name="apache-oozie"></a>Apache Oozie

Apache Oozie ist ein Workflow- und Koordinationssystem zur Verwaltung von Hadoop-Aufträgen. Weitere Informationen zur Verwendung von Oozie mit Hive finden Sie im Dokument [Definieren und Ausführen eines Workflows mit Apache Oozie](../hdinsight-use-oozie-linux-mac.md).

## <a id="nextsteps"></a>Nächste Schritte

Nachdem Sie erfahren haben, was Hive ist und wie Sie es mit Hadoop in HDInsight verwenden, können Sie mithilfe der nachfolgenden Links andere Möglichkeiten für die Arbeit mit Azure HDInsight untersuchen.

* [Hochladen von Daten in HDInsight][hdinsight-upload-data]
* [Verwenden von Apache Pig mit HDInsight][hdinsight-use-pig]
* [Verwenden von MapReduce-Aufträgen mit HDInsight][hdinsight-use-mapreduce]

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[apache-tez]: https://tez.apache.org
[apache-hive]: https://hive.apache.org/
[apache-log4j]: https://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: https://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: https://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: https://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: https://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: https://technet.microsoft.com/library/ee692792.aspx


[cindygross-hive-tables]: https://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
