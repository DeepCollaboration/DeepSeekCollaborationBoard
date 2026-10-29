# Bericht Schritt 09 — Das Vertrauensbild einer Station

## Gebaut

- `index.js`: neue Route `GET /board/station?station=<kennung>` für Fassung 2. Sie sucht die Station in `board.json.stationen` und liest ihr Repository: `station.json` (Name, Beschreibung, Host, Aktion, Reife, Lizenz, Tags, Beispiel-Namen), `README.md` (Text, sonst `null`), `examples/` (Dateinamen). Die Fassung kommt aus dem Verweis des ersten Workflows, der die Station nennt, sonst aus dem letzten Tag oder `HEAD`. `verwendung` führt jeden Workflow des Boards mit Titel, Fassung und Repository-Pfad. Unbekannte Station, fehlendes Repository und fehlende Stationsdatei antworten 404 mit dem Grund; fehlende Einzelfelder stehen als `null` oder leer in der Antwort. Fassung 1 antwortet 409 und bleibt sonst unverändert.
- `client.js`: Die Stationsanzeige der Fassung 2 holt das Vertrauensbild einmal je Station über die neue Route und merkt es sich unter der Kennung. Im Stationsfeld stehen danach Name, Fassung, Reife, Lizenz, Beschreibung, Beispiele als Liste, Verwendung mit Titel und Fassung je Workflow, der Repository-Pfad mit „kopieren“ und eine README-Vorschau. Fehlt etwas, steht „keine Angabe“, „kein Beispiel“ oder „kein README“ da; der Host steht weiter mit Personenname in der Kopfzeile. Die Fassung-1-Anzeige bleibt unverändert, ebenso das Feld und der Ablauf.
- `test/schritt27.mjs`: eigene Prüffläche unter `/tmp/board-scheibe1/schritt27`; in `station-idee` zusätzlich `examples/beispiel.md`; eine eigene Kopie `station-ohne-readme` im Board. Geprüft werden `GET /board/station?station=idee` (200 mit Name, Host `p1`, Fassung `v1`, Beschreibung, Reife, Lizenz, Beispiel, Verwendung im Standard-Workflow, Repository und README), `station-umsetzen` mit leeren `beispiele`, eine unbekannte Station mit 404, die Kopie ohne README mit `readme: null` und leerer Verwendung samt Fassung aus `HEAD`, und dass die Lese-Route keinen Commit anlegt. Danach werden Server, Ablage und Home entfernt.
- `package.json` blieb unverändert; es entstand kein neues Modul.

## Dateien

- `/workspace/projekte/board-plugin/index.js`
- `/workspace/projekte/board-plugin/client.js`
- `/workspace/projekte/board-plugin/test/schritt27.mjs`

## Prüfung

- Befehl: `cd /workspace/projekte/board-plugin && node --test test/schritt27.mjs`
- Exit-Code: 0
- Entscheidende Zeile: `ok Station „Idee“: Host p1, Fassung v1, Beispiel beispiel.md, Verwendung Standard-Workflow (v1); Station ohne README: readme null`
- Zusatz: `cd /workspace/projekte/board-plugin && node --test` → `tests 26`, `pass 26`, `fail 0`, Exit-Code 0; die alte Suite und Schritt 18–27 bestanden.
- Zusatz: `node --check client.js`, `node --check index.js` und `node --check test/schritt27.mjs` → Exit-Code 0.
- Die laufende Prüffläche `/tmp/board-scheibe1` blieb unverändert, das Vorhaben steht weiter auf „umsetzen“. Keine Commits im echten Board, kein Push, keine GitHub-Aufrufe.
