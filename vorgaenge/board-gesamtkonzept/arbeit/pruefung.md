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

