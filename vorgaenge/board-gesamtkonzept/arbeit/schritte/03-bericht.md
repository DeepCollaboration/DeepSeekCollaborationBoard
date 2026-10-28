# Bericht Schritt 03 — Die Fläche zeigt das zusammengesetzte Board

## Gebaut

- `werkzeuge/pruefflaeche.mjs`: legt unter `/tmp/board-scheibe1/` die fünf Repositories `station-idee`, `station-umsetzen`, `workflow`, `vorhaben` und `board` an, mit den Inhalten aus `test/schritt19.mjs`; jedes ist ein Git-Repository mit einer Fassung. Der Text von `idee.md` kommt aus dem echten Vorhaben. Das Werkzeug ist idempotent: vorhandene Repositories und `vorhaben.json` bleiben unangetastet. Es gibt `{ board, vorhaben }` als JSON aus.
- `komposition.js`: jedes Vorhaben trägt jetzt `ordner` mit dem absoluten Pfad seines Repositories; fehlt der Verweis, bleibt das Feld leer.
- `index.js`: `/board/datei`, `/board/doc` und `/board/dateien` beziehen das Board bei Fassung 2 aus der Komposition und den Vorhaben-Ordner aus dem Verweis. Jeder Pfad wird mit `pfadImOrdner` innerhalb des Vorhaben-Repositories gehalten. Für Fassung 1 bleibt der bisherige Weg unverändert.
- `test/schritt20.mjs`: eigene Ablage unter `/tmp/board-scheibe1/schritt20` mit eigenem `DSH_HOME`; verbindet das Testboard über `POST /board/boards` mit dem lokalen Ordner und prüft `/board/data`, `/board/datei`, `/board/doc`, `/board/dateien`, ein fremdes Vorhaben (404) und `../idee.md` (400); räumt Ablage und Home wieder ab.
- `package.json`: `werkzeuge/` steht in `files`.

## Dateien

- `/workspace/projekte/board-plugin/werkzeuge/pruefflaeche.mjs`
- `/workspace/projekte/board-plugin/test/schritt20.mjs`
- `/workspace/projekte/board-plugin/komposition.js`
- `/workspace/projekte/board-plugin/index.js`
- `/workspace/projekte/board-plugin/package.json`

## Prüfung

- Befehl: `cd /workspace/projekte/board-plugin && node --test test/schritt20.mjs`
- Exit-Code: 0
- Entscheidende Zeile: `ok Datei: „Board Gesamtkonzept“ öffnet idee.md über /board/datei und /board/doc`
- Zusatz: `cd /workspace/projekte/board-plugin && node --test` → `pass 19`, `fail 0`, Exit-Code 0; die alte Suite, Schritt 18, 19 und 20 bestanden.
- Prüffläche: `node werkzeuge/pruefflaeche.mjs` zweimal → beide Exit-Code 0, derselbe Board-Pfad `/tmp/board-scheibe1/board`, das Vorhaben steht weiter an `idee`; eine geänderte Position in einer Kopie blieb nach dem zweiten Lauf erhalten.
