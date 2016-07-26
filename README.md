AdV INSPIRE Alignments
======================

Detaillierte Informationen zu den Alignments gibt es in der generierten Dokumentation bzw. in den ensprechenden hale-Projekten selbst.
Bei mehreren Projekt-Varianten zu einem Thema befindet sich eine kurze README im entsprechenden Ordner.

Die Alignments sind nach INSPIRE Annex und INSPIRE-Themen organisiert:

- Annex I: Ordner `annex-1/mappings`

Für das Öffnen der Alignment-Projekte oder das Generieren der Dokumentation wird halestudio in einer aktuellen Version benötigt.
Aktuelle Entwicklungsversionen können [hier](https://builds.wetransform.to/job/hale/job/hale~publish(master)/) heruntergeladen werden.


Ausführung der Transformation
-----------------------------

Zum einfachen Ausführen von Transformationen sind Konfigurationen für die verschiedenen Projekte hinterlegt.
Für die Ausführung selbst wird das Werkzeug [Gradle](https://gradle.org/) verwendet, um möglichst wenig Anforderungen an das ausführende System zu stellen.

Für das Ausführen der Transformation müssen folgende Voraussetzungen geschaffen werden:

- Internetverbindung ohne Proxy (für Verwendung eines Proxy ist weitere Konfiguration nötig)
- **Java 8** muss auf dem System installiert sein (erreichbar über `PATH` Umgebungsvariable)
- **hale** muss auf dem System in einer passenden Version verfügbar sein, der Pfad zur HALE-Executable muss in der Datei `gradle.properties` angegeben werden (siehe `gradle.properties.sample` für ein Beispiel)
- Die Quelldaten als XML/GML müssen im Verzeichnis `quelldaten` abgelegt werden (Endungen `.xml`, `.gml` oder `.gz`)

Gradle selbst wird beim ersten Aufruf von `gradlew.bat` (Windows) bzw. `./gradlew` (Linux / Mac OS X) automatisch heruntergeladen. Folgend wird `gradlew` stellvertretend für den Aufruf im jeweiligen Betriebssystem verwendet.

### Auflisten der Transformationen

Die konfigurierten Transformationen können über das Kommando `gradlew tasks` aufgelistet werden.

Gradle listet dabei alle *Tasks* auf die es kennt, interessant für uns ist die Kategorie *Transformation tasks*.
Die Auflistung der *Transformation tasks* sollte ähnlich wie hier aussehen:

```
Transformation tasks
--------------------
transform-all - Runs all transformation tasks.
transform-cp - Runs a transformation based on the project aaa-cp-01.halex.
...
```

Der *Task* `transform-all` ist speziell in der Hinsicht, dass er keine bestimmte, sondern alle Transformations-Tasks ausführt. Die Ausführung stoppt jedoch falls eine der Transformationen fehlschlägt.

Alle weiteren *Tasks* stehen für die Transformation eines bestimmten Projekts, ggf. in einer Variante (z.B. nach Modellart). Der Teil das Task-Namens nach `transform-` ist der Identifier der Transformation.

### Ausführen einer Transformation

Zum Ausführen einer Transformation wird einfach der entsprechende Task ausgeführt, z.B.:

```
gradlew transform-cp
```

Die transformierten Daten, die Report-Datei und die Konsolen-Ausgabe werden in einen Unterordner des Ordners `transformiert` abgelegt.
Der Name des Unterordners ist der Identifier der Transformation (z.B. `transformiert/cp`).

Vorhandene Dateien werden bei diesem Vorgang überschrieben.
Falls die Transformation frühzeitig abbricht kann es jedoch sein dass einzelne Dateien noch aus einer vorherigen Ausführung stammen.

### Prüfung der Transformation

Aktuell werden standardmäßig zwei Arten von Validierung auf den transformierten Daten durchgeführt:

1. XML Schema Validierung auf der geschriebenen GML-Datei (die Transformation schlägt fehl falls diese nicht erfolgreich ist)
2. Interne Validierung in hale vor dem Encoding der Daten (Warnungen im Report, die aktuell manuell geprüft werden müssen)

Speziell für letzteres, aber auch Allgemein bzgl. des gesamten Ablaufs der Transformation, empfiehlt es sich einen Blick in die Report-Datei der Transformation (`reports.log`) zu werfen.

Am einfachsten geht das indem man sie in halestudio importiert.
Dazu einfach in der **Report List**-Ansicht per Rechtsklick das Kontext-Menü öffnen, *Import Log* wählen und die Datei auswählen.

**Tip:** Falls schon andere Reports angezeigt werden, ist es einfacher die Reports der Transformation zu identifizieren, wenn man vor dem Import die Option **Clear Report List** oder **Delete Log** wählt.
