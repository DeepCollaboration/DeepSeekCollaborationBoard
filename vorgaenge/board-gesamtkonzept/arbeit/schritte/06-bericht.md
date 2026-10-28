# Bericht Schritt 06 — Die Anzeige: Boards hinzufügen, wechseln, entfernen

## Gebaut

- `boards.js`: `kennzahlenVon(board, home)` liefert je Board `hauptboard`, `besitzer`, `verweise`, `stand` und `fehler`. Gelesen wird einmal je Board: bei Fassung 2 `board.json` (Personen und Verweise) samt den Repository-Pfaden für den Stand, bei Fassung 1 die Ablage über `liesAblage` (Format, Personen, Workflows, Vorhaben); der Stand entsteht aus `staendeFuer` und `zusammenfassen` in `abgleich.js`, je Repository ohne Netz. Ein nicht lesbares Board bleibt in der Liste und trägt den Grund in `fehler`. `istHauptboard` erkennt das Hauptboard an `art === 'hauptboard'` oder `id === 'hauptboard'`.
- `boards.js`: `entferneBoard` weist das Hauptboard mit 409 und „Das Hauptboard bleibt“ ab; ein anderes Board wird nur aus dem Register genommen, seine Dateien bleiben liegen. War es offen, ist danach das Hauptboard offen (sonst das erste verbliebene).
- `index.js`: `GET /board/boards` reichert jeden Eintrag um `repo`, `hauptboard`, `besitzer`, `verweise`, `stand` und `fehler` an; die vorhandenen Felder und `identitaet` bleiben. Die Schreibwege (`POST /board/boards`, `/wechsel`, `/entfernen`) antworten weiter mit der schlanken Liste aus `registerFuerSeite`. `POST /board/boards/entfernen` gibt den Status des Fehlers weiter (409 statt 400).
- `client.js`: Die Board-Leiste öffnet eine Anzeige. Je Board eine Zeile mit Name, Art (Hauptboard, Team, lokal), Form (1/2), Besitzer, Stand (abgeglichen, nur lokal: n, kein Remote, Fehler) und Verweisen (W · V · S); Knöpfe „Öffnen“, „Adresse“ (kopiert Remote oder Ordner, mit Rückfall ohne Zwischenablage) und „×“ nur für ein Board, das weder Hauptboard noch offen ist, mit der Rückfrage „wirklich?“. Darunter das Formular „Board hinzufügen“ (Name, Adresse oder Ordner) und die Identität (Name, GitHub-Name, Farbe) mit Speichern. Die Anzeige holt ihre Kennzahlen beim Aufklappen über `GET /board/boards`; die Identitätsfläche auf der Startseite bleibt unverändert.
- `test/schritt24.mjs`: eigene Prüffläche unter `/tmp/board-scheibe1/schritt24` mit eigenem `DSH_HOME`, einem Hauptboard der Fassung 1 und dem Testboard der Fassung 2 aus `legePruefflaeche`. Geprüft werden die Kennzahlen in `GET /board/boards`, die Abweisung des Hauptboards (409), das Entfernen des offenen Testboards (Hauptboard danach offen, fünf Ordner bleiben, Schreibantwort schlank), das Wiederaufnehmen über den Ordner und die geänderte Identität in `GET /board/boards`. Danach Server, Ablage und Home entfernt.
- `package.json`: unverändert; es entsteht kein neues Modul.

## Dateien

- `/workspace/projekte/board-plugin/boards.js`
- `/workspace/projekte/board-plugin/index.js`
- `/workspace/projekte/board-plugin/client.js`
- `/workspace/projekte/board-plugin/test/schritt24.mjs`

## Prüfung

- Befehl: `cd /workspace/projekte/board-plugin && node --test test/schritt24.mjs`
- Exit-Code: 0
- Entscheidende Zeile: `ok Kennzahlen: Hauptboard abgewiesen (409 „Das Hauptboard bleibt“); Testboard entfernt und über seinen Ordner wieder aufgenommen, die fünf Ordner liegen weiter da; Identität geändert, GET /board/boards zeigt „Thomas Neu“`
- Zusatz: `cd /workspace/projekte/board-plugin && node --test` → `tests 23`, `pass 23`, `fail 0`, Exit-Code 0; die alte Suite und Schritt 24 bestanden.
- Zusatz: `node --check client.js`, `node --check index.js` und `node --check boards.js` → Exit-Code 0.
- Die laufende Prüffläche `/tmp/board-scheibe1` blieb unverändert; `git status --porcelain` ist in den fünf Repositories leer. Der Prüfordner `schritt24` wurde wieder entfernt. Keine Commits.
