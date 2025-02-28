**CodeListHub** ist ein [Open Data-Projekt](https://opendatahandbook.org/guide/de/what-is-open-data/), das eine zentrale Ablage für Code-Listen bzw. Schlüsselverzeichnisse im [OpenCodeList](https://openpotato.github.io/opencodelist/de/)-Format bereitstellt. Über eine [freie API](api.md) können diese Code-Listen abgefragt und heruntergeladen werden. Mit Hilfe des [CodeListHub Explorer](https:///explorer.codelisthub.org/de) kann CodeListHub auch bequem mit einem Web-Browser durchsucht werden.

## Was sind Code-Listen?

Code-Listen sind strukturierte Sammlungen von Codes bzw. Schlüsseln, die zur Identifikation und Klassifikation von Daten verwendet werden. Diese Verzeichnisse sind essenziell in zahlreichen Bereichen wie Datenbanken, Verwaltungssystemen, wissenschaftlichen Forschungen und industriellen Anwendungen. Sie dienen dazu, Daten konsistent und effizient zu organisieren, zu speichern und abzurufen.

Code-Listen spielen eine zentrale Rolle bei der Standardisierung und Harmonisierung von Daten. Durch die Verwendung standardisierter Codes können unterschiedliche Systeme und Organisationen Daten einheitlich interpretieren und austauschen.

Beispiele für Code-Listen sind:

+ Internationale Codes für Staaten, Sprachen und Währungen der International Organization for Standardization (ISO).

+ Nationale Gebietsschlüssel (z.B. Gemeindeschlüssel der Bundesrepublik Deutschland)

+ Nationale und subnationale Schlüssel im öffentlichen Bereich (z.B. Statistikschlüssel der Statistikbehörden)

## Was ist der OpenCodeList-Standard?

[OpenCodeList](https://openpotato.github.io/opencodelist/de/) definiert ein generisches Standard-Datenformat zur Repräsentation von Code-Listen im JSON-Format. Dieses Format kann mit nahezu jeder Programmiersprache leicht erzeugt und gelesen werden. Mit Hilfe des [OpenCodeList Document Schema](https://github.com/openpotato/opencodelist/tree/main/schemas/v0.3/schema.json) können Dokumente im OpenCodeList-Format auf ihre syntaktische Korrektheit hin validiert werden.

OpenCodeList kann zum Austausch von Code-Listen zwischen Diensten oder Anwendungen genutzt werden, als Repräsentationsformat für offizielle Code-Listen oder als Antwortformat für API-Anfragen (z.B. für RESTful Web-Services).

Ein OpenCodeList-Dokument gibt es in zwei Ausprägungen:

+ **Code-Liste:** Eine Code-Liste (Englisch: code list) ist eine klassische relationale Tabelle mit Spalten und Datenzeilen, wobei mindestens eine Spalte als Schlüssel (Code) dienen sollte. 

    OpenCodeList erlaubt es, generische Code-Listen zu definieren. Eine Code-Liste ohne Daten ist eine ist eine Meta-Code-Liste.

+ **Code-Listensammlungen:** Eine Code-Listensammlung (Englisch: code list set) ist eine Liste von Verweisen auf externe Code-Listen oder externe Code-Listensammlung. 

    Mit einer Code-Listensammlung kann Folgendes abgebildet werden:

    + Zusammenfassen unterschiedlicher Versionen einer Code-Liste in einer Sammlung.

    + Erstellung einer Hierarchie von Code-Listensammlungen.

    + Erstellung eines Indexes aller nutzbaren Code-Listen.

    Eine Code-Listensammlung ohne Daten ist eine ist eine Meta-Code-Listensammlung.

## Warum CodeListHub?

Die Repräsentation von Code-Listen zu standardisieren ist aus unserer Sicht nur die halbe Miete. Code-Listen müssen auch irgendwo gespeichert und zur Verfügung gestellt werden. Idealerweise an einem offenen, zentralen Ort. CodeListHub ist dieser Ort (bzw. macht es sich zur Aufgabe, ein solcher zu werden). 

CodeListHub stellt Code-Listen sowohl über eine [API](api.md) als auch über einen einfachen [Online-Explorer](https://explorer.codelisthub.org/de) zur Verfügung, sowohl im OpenCodeList-Format als auch im CSV-Format. Das Management der Code-Listen erfolgt über ein zentrales [GitHub-Repository](https://github.com/openpotato/codelisthub.data). Das hat den Vorteil, dass Änderungen via Git nachvollziehbar sind und im Fall der Fälle auch rückgängig gemacht werden können.
