# Bericht Schritt 01 — Die vier Formate lesen

## Gebaut

- `repos.js`: erkennt ein lokales Repository an `station.json`, `workflow.json`, `vorhaben.json` oder `board.json` und liefert `{ pfad, art, dateien }` mit sortierter, relativer Dateiliste; ohne Git, ohne Remote.
- `formate/station.js`, `formate/workflow.js`, `formate/vorhaben.js`, `formate/board.js`: lesen und prüfen `format: 2` und die Felder der Fassung 2; gemeinsame Helfer in `formate/hilfe.js`.
- `test/schritt18.mjs`: legt fünf Repositories unter `/tmp/board-scheibe1/` an, liest alle vier Formate, prüft die geforderten Felder, prüft eine fehlende Formatdatei als Fehler und entfernt die Ablage wieder.
- `package.json`: `repos.js` und `formate/*` stehen in `files`.

## Dateien

- `/workspace/projekte/board-plugin/repos.js`
- `/workspace/projekte/board-plugin/formate/hilfe.js`
- `/workspace/projekte/board-plugin/formate/station.js`
- `/workspace/projekte/board-plugin/formate/workflow.js`
- `/workspace/projekte/board-plugin/formate/vorhaben.js`
- `/workspace/projekte/board-plugin/formate/board.js`
- `/workspace/projekte/board-plugin/test/schritt18.mjs`
- `/workspace/projekte/board-plugin/package.json`

## Prüfung

- Befehl: `cd /workspace/projekte/board-plugin && node --test test/schritt18.mjs`
- Exit-Code: 0
- Entscheidende Zeile: `ok Station: Umsetzen mit bauplan.md, produkt/, umsetzung.md und skill gelesen`
- Zusatz: `cd /workspace/projekte/board-plugin && node --test` → 17 von 17 Prüfungen bestanden, Exit-Code 0.
- Die Prüfablage `/tmp/board-scheibe1/` ist nach dem Lauf entfernt; die alte Ablage und ihre Daten sind unverändert, nur dieser Bericht kam hinzu.
