# Prüfprotokoll

## Schritt 01 — bestanden

- **Befehl:** `cd /workspace/projekte/board-plugin && node --test test/schritt18.mjs` und `node --test`
- **Exit-Code:** 0 und 0
- **Entscheidende Zeile:** `ℹ pass 17`, `ℹ fail 0`
- **Befund:** Die vier Leser lesen die echten Formatdateien, prüfen Format 2, Pflichtfelder, Enums und Verweise; die neue Prüfung belegt die Werte der Station „Umsetzen“, der Positionen, des Vorhabens und des Boards und räumt die Ablage wieder ab. `repos.js` erkennt die vier Arten ohne Git; die alte Suite bleibt grün.

## Schritt 02 — bestanden

- **Befehl:** `cd /workspace/projekte/board-plugin && node --test test/schritt19.mjs` und `node --test`
- **Exit-Code:** 0 und 0
- **Entscheidende Zeile:** `ok Testboard: „Board Gesamtkonzept“ steht an „idee“; der Workflow trägt „Idee“ und „Umsetzen“`
- **Befund:** Die Prüfung ruft den in `index.js` registrierten Handler für `/board/data` auf, nicht die Komposition direkt. Die Fassung 1 bleibt unverändert; `test/schritt18.mjs` wurde nur im Ablageort und beim Aufräumen geändert, Inhalt und Prüfungen sind gleich. 18 von 18 Prüfungen bestehen.

## Schritt 03 — bestanden

- **Befehl:** `cd /workspace/projekte/board-plugin && node --test test/schritt20.mjs` und `node --test`
- **Exit-Code:** 0 und 0
- **Entscheidende Zeile:** `ok Datei: „Board Gesamtkonzept“ öffnet idee.md über /board/datei und /board/doc`
- **Befund:** `/board/datei`, `/board/doc` und `/board/dateien` lesen die Datei aus dem Vorhaben-Repository; ein fremdes Vorhaben antwortet 404, `../idee.md` 400. Die volle Suite besteht aus 19 von 19 Prüfungen; Fassung 1 bleibt unverändert, `client.js` ist unverändert. `werkzeuge/pruefflaeche.mjs` legt die fünf Repositories mit je einer Fassung an und gibt zweimal denselben Board-Pfad aus; ein geändertes `vorhaben.json` (Position `umsetzen`) und eine zusätzliche Datei blieben beim zweiten Lauf byte-gleich. Das echte Hauptboard (Fassung 1) liest weiter seine vier Vorhaben.

## Schritt 04 — bestanden

- **Befehl:** `cd /workspace/projekte/board-plugin && node --test test/schritt21.mjs` und `node --test`
- **Exit-Code:** 0 und 0
- **Entscheidende Zeile:** `ok Weitergeben: Commit 61cdd7a nennt nur vorhaben.json; station-idee, station-umsetzen, workflow und board sind unverändert`
- **Befund:** Die Route `/board/vorhaben/verschieben` schreibt für Fassung 2 nur `vorhaben.json` im Vorhaben-Repository und committet nur diese Datei; die HEADs der Stationen, des Workflows und des Boards bleiben gleich. Der Fassung-1-Zweig ist byte-gleich geblieben; die volle Suite besteht aus 20 von 20 Prüfungen. Die laufende Prüffläche blieb unberührt.

## Schritt 05 — bestanden

- **Befehl:** `cd /workspace/projekte/board-plugin && node --test test/schritt22.mjs`, `node --test` und `node --check client.js`
- **Exit-Code:** 0, 0 und 0
- **Entscheidende Zeile:** `ok Stand: Vorhaben zuerst 1 Fassung nur lokal, dann abgeglichen; station-idee, station-umsetzen, workflow und board: kein Remote`
- **Befund:** Der Abgleich arbeitet je Repository; ein Repository ohne Remote steht als „kein Remote“ da, ein Fehler bleibt an seinem Repository, das Board bleibt sichtbar. Der Client zeigt eine Zeile je Repository, die alte Zusammenfassung bleibt. Die volle Suite besteht aus 21 von 21 Prüfungen; Fassung 1 bleibt unverändert.

## Schritt 05b — bestanden

- **Befehl:** `cd /workspace/projekte/board-plugin && node --test test/schritt23.mjs`, `node --test` und `node --check index.js`
- **Exit-Code:** 0, 0 und 0
- **Entscheidende Zeile:** `ok Wechsel: testboard aktiv; darf.weitergeben: true, darf.verschieben: true, darf.aendern: true, darf.zurueckschicken: true; verantwortet von Thomas Heinrich`
- **Befund:** Der Rückwechsel auf ein Board der Fassung 2 liefert 200, weil die Route jetzt `board.json` prüft; `/board/data` liefert `darf` aus derselben Stelle wie Fassung 1, für eine fremde Person false. Die volle Suite besteht aus 22 von 22 Prüfungen; Fassung 1 bleibt unverändert.

## Schritt 06 — bestanden

- **Befehl:** `cd /workspace/projekte/board-plugin && node --test test/schritt24.mjs`, `node --test`, `node --check client.js` und `node --check index.js`
- **Exit-Code:** 0, 0, 0 und 0
- **Entscheidende Zeile:** `ok Kennzahlen: Hauptboard abgewiesen (409 „Das Hauptboard bleibt“); Testboard entfernt und über seinen Ordner wieder aufgenommen, die fünf Ordner liegen weiter da; Identität geändert, GET /board/boards zeigt „Thomas Neu“`
- **Befund:** `GET /board/boards` liefert je Board Art, Form, Besitzer, Verweise, Stand und Fehler; das Hauptboard lässt sich nicht entfernen, ein anderes Board schon, seine Dateien bleiben. Die Anzeige zeigt Öffnen, Adresse und Entfernen; die Identität ist dort änderbar. Die volle Suite besteht aus 23 von 23 Prüfungen; Fassung 1 bleibt unverändert.

## Schritt 07 — bestanden

- **Befehl:** `cd /workspace/projekte/board-plugin && node --test test/schritt25.mjs`, `node --test`, `node --check client.js` und `node --check index.js`
- **Exit-Code:** 0, 0, 0 und 0
- **Entscheidende Zeile:** `ok Bibliothek: Stationen „Idee“ und „Umsetzen“ (Host p1, Fassung v1); Workflow „Standard-Workflow“ (Autor p1, Fassung v1, 2 Stationen); Vorhaben „Board Gesamtkonzept“ (Fassung v1, verantwortet von Thomas Heinrich)`
- **Befund:** `/board/data` trägt für Fassung 2 die drei Arten mit Host und Fassung; das Vorhaben nennt seinen Workflow mit fester Fassung. Der Client zeigt drei Abschnitte und die Einzelheiten; Fassung 1 behält die bisherige Bibliothek samt „+ Neue Station“. Die volle Suite besteht aus 24 von 24 Prüfungen.

## Schritt 08 — bestanden

- **Befehl:** `cd /workspace/projekte/board-plugin && node --test test/schritt26.mjs`, `node --test`, `node --check client.js` und `node --check index.js`
- **Exit-Code:** 0, 0, 0 und 0
- **Entscheidende Zeile:** `ok Workflow gestellt: Commit fb2b1eb nennt nur workflow.json; Station „Prüfen“ als Verweis und Position „pruefen“; station-pruefen, station-idee, station-umsetzen, vorhaben und board unverändert`
- **Befund:** `/board/data` liefert alle Stationen des Boards mit `imWorkflow`; die Route stellt eine Station in den Workflow, schreibt Verweis, Position und Kante nur in `workflow.json` und committet nur diese Datei. Unbekannte Station 404 ohne Commit; Fassung 1 unverändert; 25 von 25 Prüfungen.






