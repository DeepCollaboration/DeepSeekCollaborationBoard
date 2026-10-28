# Bericht Schritt 05b — Die sichtbaren Aktionen der ersten Scheibe

## Gebaut

- `index.js`, Route `/board/boards/wechsel`: Bei einem Board der Fassung 2 wird jetzt `board.json` im Ordner geprüft, bei Fassung 1 wie bisher `format`. Fehlt die Datei, bleibt es bei Status 409 und der Meldung „Der Ordner dieses Boards fehlt: …“; `setzeAktiv` folgt unverändert erst danach. Damit lässt sich ein Board der Fassung 2 anwählen, obwohl es keine `format`-Datei hat.
- `index.js`, Route `/board/data` für Fassung 2: Die Vorhaben der Komposition gehen durch dieselbe Stelle `datenAntwort` wie bei Fassung 1 und tragen dadurch `verantwortliche` als Objekt, `regel`, `verantwortet` und `darf`. `darf` entsteht in `darfVon` aus `darf`/`verantwortlicheVon`; eine zweite Rechnung wurde nicht eingebaut. Die Stelle ist im Fassung-2-Zweig als die eine Quelle gekennzeichnet. Fassung 1 bleibt unverändert.
- `test/schritt23.mjs`: eigene Prüffläche unter `/tmp/board-scheibe1/schritt23` und eigenes `DSH_HOME`; verbindet das Testboard und ein gewöhnliches Altboard, wechselt auf `testboard` zurück (vorher 409) und prüft an `/board/data`: `aktiv: testboard`, Fassung 2, eine Karte an `idee`, `verantwortliche.idee` mit `p1`, `regel: einer`, `verantwortet` und `darf.verschieben`, `.weitergeben`, `.aendern`, `.zurueckschicken` jeweils `true`. Danach werden die Verantwortlichen in `vorhaben.json` ohne Commit auf `p2` gesetzt; erneut gelesen ist `darf.verschieben === false`, die Datei wird auf ihren Stand zurückgesetzt. Zwei Registereinträge ohne `board.json` (Fassung 2) beziehungsweise ohne `format` (Fassung 1) antworten 409 mit der Meldung vom fehlenden Ordner; das offene Board bleibt `testboard`. Server, Ablage und Home werden entfernt.
- `client.js`: unverändert. Die Karte liest `v.darf` bereits; kein neuer Schreibweg.

## Dateien

- `/workspace/projekte/board-plugin/index.js`
- `/workspace/projekte/board-plugin/test/schritt23.mjs`

## Prüfung

- Befehl: `cd /workspace/projekte/board-plugin && node --test test/schritt23.mjs`
- Exit-Code: 0
- Entscheidende Zeile: `ok Wechsel: testboard aktiv; darf.weitergeben: true, darf.verschieben: true, darf.aendern: true, darf.zurueckschicken: true; verantwortet von Thomas Heinrich`
- Zusatz: `cd /workspace/projekte/board-plugin && node --test` → `pass 22`, `fail 0`, Exit-Code 0; Schritt 18–23 bestanden.
- Zusatz: `cd /workspace/projekte/board-plugin && node --check index.js` → Exit-Code 0.
- Die laufende Prüffläche `/tmp/board-scheibe1` mit ihren fünf Ordnern und dem Testboard blieb unverändert; die Prüfung arbeitet nur im Unterordner `schritt23` und entfernt ihn wieder. Kein Commit, keine laufende Board-Liste angefasst.
