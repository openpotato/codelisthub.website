# Änderungslog - Web-Service  

Der CodeListHub-API Web-Service ist [Open Source](https://github.com/openpotato/codelisthub) unter GitHub. Dort kann die detailierte [Commit-Historie](https://github.com/openpotato/codelisthub/commits/develop/) eingesehen werden. Dieses Änderungslog dient als zusammenfassender Überblick der Änderungen.

Wir halten uns dabei weitestgehend an die Empfehlungen aus dem Community-Projekt [Keep a Changelog](https://keepachangelog.com/de).

## 0.0.2 <small>_ 04. März 2025</small>

**Geändert:**

+ API-Endpunkt `api.codelisthub.org/v1/documents/index`: Der Parameter `searchTerm` berücksichtigt jetzt auch die Eigenschaften `identification.canonicalUri` und `identification.canonicalVersionUri` der Code-Listen.

## 0.0.1 <small>_ 21. Februar 2025</small>

Erste Veröffentlichung.