# Bericht Schritt 04 — Weitergeben schreibt ins Vorhaben-Repository

## Gebaut

- `formate/vorhaben.js`: `schreibeVorhaben(repo, { position, verantwortliche })` liest `vorhaben.json` roh und prüft Format 2; es schreibt dieselben Angaben in derselben Reihenfolge zurück und ersetzt nur `position` und `verantwortliche`. Geschrieben wird über eine Zwischendatei, die anschließend umbenannt wird.
- `index.js`: `/board/vorhaben/verschieben` unterscheidet nach `AKTIV.format`. Fassung 1 geht unverändert den bisherigen Weg. Fassung 2 findet das Vorhaben über die Komposition und seinen `ordner`, prüft die Zielposition gegen die Positionen des Workflows, setzt `position`, trägt an der Zielstation die bisherige Person ein, wenn dort noch niemand steht, schreibt `vorhaben.json` und committet nur diese Datei im Vorhaben-Repository — Autor aus der Identität, deutsche Nachricht, kein Push. Antwort ist das neu zusammengesetzte Board.
- `index.js`: `git()` bekommt einen optionalen Ordner, damit der Commit im Vorhaben-Repository statt in der Ablage läuft. `pruefeFormatVorSchreiben()` überspringt für Fassung 2 nur die Prüfung der Fassung-1-`format`-Datei, die es dort nicht gibt; die Fassung wird in jeder Formatdatei geprüft. Die Rechteprüfung `darf` bleibt die bisherige.
- `test/schritt21.mjs`: eigene Prüffläche unter `/tmp/board-scheibe1/schritt21` und eigenes `DSH_HOME`; verbindet das Testboard, merkt die HEAD-Fassungen von `vorhaben`, `station-idee`, `station-umsetzen`, `workflow` und `board`, gibt weiter und prüft Antwort, `/board/data`, die Datei samt unveränderten übrigen Angaben, `git log -1 --name-only` (nur `vorhaben.json`), die unveränderten Fassungen der anderen Repositories sowie 400 bei unbekannter Position und 404 bei unbekanntem Vorhaben ohne neuen Commit. Danach Server, Ablage und Home entfernt.
- `package.json`: unverändert; `formate/*` und `werkzeuge/` stehen bereits in `files`.

## Dateien

- `/workspace/projekte/board-plugin/formate/vorhaben.js`
- `/workspace/projekte/board-plugin/index.js`
- `/workspace/projekte/board-plugin/test/schritt21.mjs`

## Prüfung

- Befehl: `cd /workspace/projekte/board-plugin && node --test test/schritt21.mjs`
- Exit-Code: 0
- Entscheidende Zeile: `ok Weitergeben: Commit 7200b15 nennt nur vorhaben.json; station-idee, station-umsetzen, workflow und board sind unverändert`
- Zusatz: `cd /workspace/projekte/board-plugin && node --test` → `pass 20`, `fail 0`, Exit-Code 0.
- Von Hand geprüft: Der Commit trägt `Thomas Heinrich <thomas.heinrich@board>` und die Nachricht `Vorhaben „Board Gesamtkonzept“ verschoben`.
- Die laufende Prüffläche `/tmp/board-scheibe1` (fünf Ordner) blieb unverändert; die Prüfung arbeitet nur im Unterordner `schritt21` und entfernt ihn wieder.
