# Statistik Stadt Zürich: RDF Data Cube Pipeline
This program obtains the data files in HDB format from Statistik Stadt Zürich and feeds them into the RDF data cube. 
The HDB format consists of two CSV and one Excel. The Excel contains all basic data and meta data. The others contain all the statistical observations and relations.

## Struktur

* `input` - Enthält die CSV on the Web JSON-Konfiguration. Die Daten selber werden über WebDAV bezogen und werden nicht im Github Repository gepflegt.
* `config` - Enthält Templates von config-Dateien für die benötigten Tools
* `scripts` - Enthält diverse Shell-Scripte, um die Generierung möglichst vollständig zu automatisieren. Bedingt ein Unix-System, geschrieben und getestet auf macOS.
* `target` - Wird erstellt durch die Scripte und enthält die zwischen- und Endresultate in [N-Triples](https://en.wikipedia.org/wiki/N-Triples) Serialisierung. Die finale Datei heisst `everything.nt.gz` und ist mit gzip komprimiert.
* `sparql` - Enthält SPARQL Abfragen, welche in der Pipeline ausgeführt werden. Diese werden benötigt, um den vollständigen Graphen automatisiert zu bauen.
* `package.json` - Stellt die eigentliche Pipeline zur Verfügung.


## CSV on the Web

Die Transformation basiert auf dem neuen CSV on the Web Standard vom W3C. Folgende Dokumente dienten als Grundlage für die Konfiguration:

* [Model for Tabular Data and Metadata on the Web](https://www.w3.org/TR/tabular-data-model/)
* [Metadata Vocabulary for Tabular Data](https://www.w3.org/TR/tabular-metadata/)
* [CSV on the Web: A Primer](https://www.w3.org/TR/tabular-data-primer/)

Für die Transformation wird eine von Zazuko in JavaScript (Node.js) geschriebene Implementation von CSV on the Web verwendet: [A CSV on the Web parser with RDFJS Stream interface](https://github.com/rdf-ext/rdf-parser-csvw). Der Parser implementiert aktuell nicht den vollständigen Standard, ist aber massiv performanter als die anderen uns bekannten Implementationen.

## Daten Pipeline

Die Daten werden durch verschiedene Scripte generiert. Die produktive Pipeline wird automatisiert in einer Gitlab-Umgebung ausgeführt. Die Daten werden dabei in das [RDF Data Cube](https://www.w3.org/TR/vocab-data-cube/) Vokabular überführt.

### Anforderungen

* [Apache Jena](https://jena.apache.org/download/index.cgi) (bedingt Java Umgebung). Die Kommanozeilen-Werkzeuge müssen im `PATH` sein
* [Serd](https://drobilla.net/software/serd), ebenfalls im `PATH`
* [Node.js](https://nodejs.org/)
* Unix Umgebung wie MacOS, FreeBSD oder Linux
  * curl
  * sh
  * sed

Alternativ kann das folgende Docker-Image verwendet werden: [zazukoians/node-java-jena](https://hub.docker.com/r/zazukoians/node-java-jena/). Dieses Docker-Image wird von der Pipeline selber verwendet und über Gitlab automatisiert ausgeführt. Details können der [Gitlab YAML Datei](.gitlab-ci.yml) entnommen werden.

### Ausführung

Die Konvertierung kann von Hand gestartet werden. Dazu muss initial folgendes ausgeführt werden: `npm install`

Danach kann mit `npm run` angezeigt werden, was ausgeführt werden soll. Bitte zum Testen ausschliesslich `npm run build` ausführen!


# License
This program is licensed under [3-Clause BSD License](https://opensource.org/licenses/BSD-3-Clause):
Copyright 2018 Statistik Stadt Zürich

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.

2. Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

3. Neither the name of the copyright holder nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
