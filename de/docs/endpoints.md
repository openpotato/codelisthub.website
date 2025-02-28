Die CodeListHub-API erlaubt es, einzelne Dokumente im OpenCodeList-Format oder (falls vorhanden) im CSV-Format herunterzuladen sowie das Gesamtverzeichnis aller Dokumente auf CodeListHub zu durchsuchen.

Der einfachste Weg, die API zu nutzen, ist der Weg über die Kommandozeile. Wir werden in diesem Kapitel mit der Kommandozeilenanwendung [curl](https://curl.se/) arbeiten. 

Unter Linux ist `curl` in der Regel vorinstalliert. Unter Windows ist `curl` als Alias des Cmdlet [Invoke-WebRequest](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-webrequest) definiert, kann also via Powershell genutzt werden. Die hier verwendeten Befehlsfolgen variieren leicht, daher werden sie für Powershell 7 (Windows) und Bash (Linux) getrennt angegeben.

## Dokumentenindex

Der Dokumentenindex erlaubt die Recherche über alle auf CodeListHub gespeicherten Dokumente:

!!! info ""
    
    **`api.codelisthub.org/v1/documents/index`**
    
    :    Liefert eine Liste aller verfügbaren Code-Listen und Code-Listensammlungen. Die Abfrage unterliegt einem [Paging](paging.md).
    
         Mögliche [HTTP-Request-Header](https://developer.mozilla.org/de/docs/Web/HTTP/Headers) sind: 

         + `Accept`: Der [Accept](https://developer.mozilla.org/de/docs/Web/HTTP/Headers/Accept)-Header steuert, in welchem Format die Liste zurückgeliefert werden soll. Mögliche Werte sind `application/json`, `text/json` und `text/plain`. Ist dieser Header nicht gegeben, wird `application/json` angenommen

         Optionale Parameter sind: 

         + `language (string)`: Sprache als [IETF BCP 47-Sprachtag](https://datatracker.ietf.org/doc/html/rfc5646), nach welcher gefiltert werden soll.
         + `searchTerm (string)`: Suche nach Namen und Bezeichnungen mittels [regulärer Ausdrücke](regex.md)
         + `tags (string)`: Kommaseparierte Liste mit Tags bzw. Schlüsselwörtern, nach denen gefiltert werden soll
         + `type (string)`: Dokumententyp. Mögliche Werte:
            + `CodeList` 
            + `CodeListSet`
         + `publishedFrom (string)`: Ältester Publikationszeitpunkt, nach dem gefiltert werden soll
         + `publishedUntil (string)`: Jüngster Publikationszeitpunkt, nach dem gefiltert werden soll

    **`api.codelisthub.org/v1/documents/index/{canonicalUri}`**
    
    :    Liefert eine Liste aller verfügbaren Code-Listen und Code-Listensammlungen für eine gegebene kanonische URI. Die Abfrage unterliegt einem [Paging](paging.md).
    
         Mögliche [HTTP-Request-Header](https://developer.mozilla.org/de/docs/Web/HTTP/Headers) sind: 

         + `Accept`: Der [Accept](https://developer.mozilla.org/de/docs/Web/HTTP/Headers/Accept)-Header steuert, in welchem Format die Liste zurückgeliefert werden soll. Mögliche Werte sind `application/json`, `text/json` und `text/plain`. Ist dieser Header nicht gegeben, wird `application/json` angenommen

         Optionale Parameter sind: 

         + `language (string)`: Sprache als [IETF BCP 47-Sprachtag](https://datatracker.ietf.org/doc/html/rfc5646), nach welcher gefiltert werden soll.

Pro Dokument werden folgende Attribute geliefert:

+ `type (string)`: Dokumententyp. Mögliche Werte:
    + `CodeList` 
    + `CodeListSet`
+ `language (string)`: Sprache des Dokuments als [IETF BCP 47-Sprachtag](https://datatracker.ietf.org/doc/html/rfc5646)
+ `shortName (string)`: Kurzname des Dokuments
+ `longName (string)`: Langname des Dokuments
+ `description (string)`: Kurze Beschreibung des Dokuments
+ `version (string)`: Version des Dokuments
+ `canonicalUri (string)`: URN, die alle Versionen (zusammen) des Dokuments eindeutig identifiziert 
+ `canonicalVersionUri (string)`:  URN, die diese Version des Dokuments eindeutig identifiziert
+ `tags (string-array)`: Liste mit Tags bzw. Schlüsselwörtern, die den Inhalt des Dokuments bestimmen
+ `publishedAt (string)`: Zeitpunkt der Veröffentlichung dieses Dokuments.
+ `publisher.shortName (string)`: Kurzname des Herausgebers
+ `publisher.longName (string)`: Langname des Herausgebers
+ `publisher.url (string)`: Verweis auf zusätzliche externe Informationen (z.B. eine Webseite) zum des Herausgeber
+ `publisher.identifier.Value (string)`: Schlüssel, Code oder eine ID, die mit dem Herausgeber verbunden ist
+ `publisher.identifier.source.shortName (string)`: Kurzname der Quelle
+ `publisher.identifier.source.longName (string)`: Langname der Quelle
+ `publisher.identifier.source.url (string)`: Verweis auf zusätzliche externe Informationen (z.B. eine Webseite) zur Quelle

Hier eine Beispielabfrage für eine Liste aller Dokumente des Standards [OpenT8](https://openpotato.github.io/opent8/): 

=== "Powershell 7"

    ``` powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/index?searchTerm=opent8' -H 'accept: text/json' | ConvertFrom-Json | ConvertTo-Json
    ```

=== "Bash"

    ``` bash
    curl -X GET 'https://api.codelisthub.org/v1/documents/index?searchTerm=opent8' -H 'accept: text/json' | json_pp
    ```

Hier eine Beispielabfrage für alle deutschsprachigen Code-Listen: 

=== "Powershell 7"

    ``` powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/index?type=CodeList&language=de' -H 'accept: text/json' | ConvertFrom-Json | ConvertTo-Json
    ```

=== "Bash"

    ``` bash
    curl -X GET 'https://api.codelisthub.org/v1/documents/index?type=CodeList&language=de' -H 'accept: text/json' | json_pp
    ```

## Abfrage einzelner Dokumente

Die Abfrage konkreter Code-Listen oder Code-Listensammlungen erfolgt über folgenden API-Endpunkt:

!!! info ""

    **`api.codelisthub.org/v1/documents/{canonicalUri}`**

    :    Liefert ein durch die Parameter `canonicalUri` spezifiziertes OpenCodeList-Dokument zurück.

         Mögliche [HTTP-Request-Header](https://developer.mozilla.org/de/docs/Web/HTTP/Headers) sind: 

         + `Accept`: Der [Accept](https://developer.mozilla.org/de/docs/Web/HTTP/Headers/Accept)-Header steuert, in welchem Format das OpenCodeList-Dokument zurückgeliefert werden soll. Mögliche Werte sind `application/json`, `text/json` und `text/plain`. Ist dieser Header nicht gegeben, wird `application/json` angenommen
         + `Accept-Language`: Der [Accept-Language](https://developer.mozilla.org/de/docs/Web/HTTP/Headers/Accept-Language)-Header steuert, in welcher Sprache das OpenCodeList-Dokument zurückgeliefert werden soll. Standardmäßig sollte dies ein [IETF BCP 47-Sprachtag](https://datatracker.ietf.org/doc/html/rfc5646) sein. Ist dieser Header nicht gegeben, wird `de` angenommen

         Optionale Parameter sind: 

         + `language (string)`: Sprache als [IETF BCP 47-Sprachtag](https://datatracker.ietf.org/doc/html/rfc5646), in welcher Sprache das OpenCodeList-Dokument zurückgeliefert werden soll.
         + `metaOnly (boolean)`: Steuert, ob lediglich ein Metadokument, also eine OpenCodeList-Dokument ohne Daten, zurückgeliefert werden soll. 

Bei erfolgreicher Abfrage wird ein JSON-Dokument im OpenCodeList-Format zurückgeliefert.

Hier eine Beispielabfrage für das deutschsprachige ISO-Länderverzeichnis: 

=== "Powershell 7"

    ``` powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/urn%3Acodelisthub%3Aiso%3Acountries%3Av1' -H 'accept: text/json' -H 'accept-language: de' | ConvertFrom-Json | ConvertTo-Json
    ```

=== "Bash"

    ``` bash
    curl -X GET 'https://api.codelisthub.org/v1/documents/urn%3Acodelisthub%3Aiso%3Acountries%3Av1' -H 'accept: text/json' -H 'accept-language: de' | json_pp
    ```

Das gleiche Beispiel als Metadokument: 

=== "Powershell 7"

    ``` powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/urn%3Acodelisthub%3Aiso%3Acountries%3Av1?language=de&metaOnly=true' -H 'accept: text/json' -H 'accept-language: de' | ConvertFrom-Json | ConvertTo-Json
    ```

=== "Bash"

    ``` bash
    curl -X GET 'https://api.codelisthub.org/v1/documents/urn%3Acodelisthub%3Aiso%3Acountries%3Av1?language=de&metaOnly=true' -H 'accept: text/json' -H 'accept-language: de' | json_pp
    ```

## Abfrage alternativer Formate

Die Abfrage von Code-Listen im CSV-Format erfolgt über folgenden API-Endpunkt:

!!! info ""

    **`api.codelisthub.org/v1/documents/{canonicalUri}/alternative-format`**

    :    Liefert ein durch die Parameter `canonicalUri` spezifiziertes spezifiziertes Dokument in einem alternativen Format zurück. 
	
         Mögliche [HTTP-Request-Header](https://developer.mozilla.org/de/docs/Web/HTTP/Headers) sind: 

         + `Accept`: Der [Accept](https://developer.mozilla.org/de/docs/Web/HTTP/Headers/Accept)-Header steuert, in welchem Format das Dokument zurückgeliefert werden soll. Mögliche Werte sind `text/csv`. Ist dieser Header nicht gegeben, wird `text/csv` angenommen
         + `Accept-Language`: Der [Accept-Language](https://developer.mozilla.org/de/docs/Web/HTTP/Headers/Accept-Language)-Header steuert, in welcher Sprache das Dokument zurückgeliefert werden soll. Standardmäßig sollte dies ein [IETF BCP 47-Sprachtag](https://datatracker.ietf.org/doc/html/rfc5646) sein. Ist dieser Header nicht gegeben, wird `de` angenommen

         Optionale Parameter sind: 

         + `language (string)`: Sprache als [IETF BCP 47-Sprachtag](https://datatracker.ietf.org/doc/html/rfc5646), in welcher Sprache das Dokument zurückgeliefert werden soll.

Bei erfolgreicher Abfrage wird ein Dokument im gewünschten Format zurückgeliefert.

Hier eine Beispielabfrage für das deutschsprachige ISO-Länderverzeichnis im CSV-Format: 

=== "Powershell 7"

    ``` powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/urn%3Acodelisthub%3Aiso%3Acountries%3Av1/alternative-format' -H 'accept: text/csv' -H 'accept-language: de'
    ```

=== "Bash"

    ``` bash
    curl -X GET 'https://api.codelisthub.org/v1/documents/urn%3Acodelisthub%3Aiso%3Acountries%3Av1/alternative-format' -H 'accept: text/csv' -H 'accept-language: de'
    ```

Das gleiche Beispiel für das englischsprachige ISO-Länderverzeichnis im CSV-Format: 

=== "Powershell 7"

    ``` powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/urn%3Acodelisthub%3Aiso%3Acountries%3Av1/alternative-format' -H 'accept: text/csv' -H 'accept-language: en'
    ```

=== "Bash"

    ``` bash
    curl -X GET 'https://api.codelisthub.org/v1/documents/urn%3Acodelisthub%3Aiso%3Acountries%3Av1/alternative-format' -H 'accept: text/csv' -H 'accept-language: en'
    ```

## Abfrage Tags

Eine Liste aller verfügbaren Tags bzw. Schlüsselwörtern erhält man über folgenden API-Endpunkt:

!!! info ""

    **`api.codelisthub.org/v1/tags`**

    :    Liefert eine Liste aller Tags auf CodeListHub zurück. Die Abfrage unterliegt einem [Paging](paging.md). 

         Mögliche [HTTP-Request-Header](https://developer.mozilla.org/de/docs/Web/HTTP/Headers) sind: 

         + `Accept`: Der [Accept](https://developer.mozilla.org/de/docs/Web/HTTP/Headers/Accept)-Header steuert, in welchem Format die Tags-Liste zurückgeliefert werden soll. Mögliche Werte sind `application/json`, `text/json` und `text/plain`. Ist dieser Header nicht gegeben, wird `application/json` angenommen

Bei erfolgreicher Abfrage wird ein JSON-String-Array zurückgeliefert.

Hier eine Beispielabfrage: 

=== "Powershell 7"

    ``` powershell
    curl -X GET 'https://api.codelisthub.org/v1/tags' -H 'accept: text/json'
    ```

=== "Bash"

    ``` bash
    curl -X GET 'https://api.codelisthub.org/v1/tags' -H 'accept: text/json'
    ```
