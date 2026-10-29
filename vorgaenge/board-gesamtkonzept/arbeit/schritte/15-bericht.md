# Bericht Schritt 15 — Öffentlich und privat

## Gebaut

- `rechte.js` (neu): `sichtbarkeitVon(home, pfad)` liest zuerst `$DSH_HOME/board/sichtbarkeit.json` (absolute Pfade), dann `sichtbarkeit.json` im Elternordner (Schlüssel = Ordnername), zuletzt den Namen (`vorhaben-*` und `board-team` privat, sonst öffentlich). Keine Datei wird geschrieben.
- `pruefeVerweise(boardPfad, {home})`: prüft Board, Workflows, Stationen und Vorhaben; ein öffentliches Ding auf einem privaten ergibt `{art:'sichtbarkeit', von, auf, grund}`. Liefert `{luecken, sichtbarkeiten}`.
- `darfSchreiben({pfad, home, client})`: lokal ohne Remote `true`, mit Remote und ohne Token `false`, mit Token `true` bei `admin`, `maintain`, `push` oder `write` über `berechtigung`; Ergebnis 30 Sekunden je Repository, Remote und Person zwischengespeichert.
- `komposition.js`: Sichtbarkeit je Station (auch als Schritt), je Workflow und je Vorhaben; das Board selbst trägt `sichtbarkeit`; die Lücken der Verweisprüfung werden an die vorhandenen `luecken` angehängt.
- `index.js`: `/board/data` gibt `luecken`, `sichtbarkeit` und `darf` weiter; bei `darf: false` sind alle Handlungen der Vorhaben gesperrt. `POST /board/workflow/station` und `POST /board/workflow/verbinden` antworten 409 „Öffentlich verweist nur auf Öffentliches“, bevor geschrieben wird. `POST /board/github/veroeffentlichen` prüft die Verweise, antwortet ohne Token 409 mit `befehle` und `organisation` und legt mit Token je Repository an oder verbindet es, setzt den Remote (`DSH_GIT_REMOTE_BASIS` ersetzt den Anfang), pusht `main --tags` und setzt die Sichtbarkeit; Fehler nennen das Repository und halten an (`{veroeffentlicht, fehler}`). `GET /board/boards` und die Board-Liste in `/board/data` nennen je Board die Sichtbarkeit.
- `client.js`: je Board „öffentlich“/„privat“ und ein Knopf „Veröffentlichen“; ohne Token zeigt er die Befehle zum Kopieren. Lücken erscheinen als Zeile „Öffentlich verweist auf Privates: …“ an der Bibliothek und am Board. Ohne Schreibrecht fehlen „+ Neues Vorhaben“, „+ Workflow“, „+ Workflow übernehmen“, „Bearbeiten“ und die Handlungen am Vorhaben.
- `test/schritt32.mjs` (neu): fünf Repositories unter `/tmp/board-scheibe1/schritt32`, Sichtbarkeitskarte im Home und im Elternordner, Server in-process, nachgebildetes GitHub und lokale Bare-Repositories.

## Geänderte Dateien

- `/workspace/projekte/board-plugin/rechte.js` — neu, Sichtbarkeit, Verweisprüfung, Schreibrecht.
- `/workspace/projekte/board-plugin/komposition.js` — Sichtbarkeit und Sichtbarkeitslücken.
- `/workspace/projekte/board-plugin/index.js` — Datenform, prüfende Routen, Veröffentlichen, Board-Sichtbarkeit.
- `/workspace/projekte/board-plugin/client.js` — Anzeige, Befehle, Lückenzeilen, Lesemodus.
- `/workspace/projekte/board-plugin/package.json` — `rechte.js` in `files`.
- `/workspace/projekte/board-plugin/test/schritt32.mjs` — neu, Prüfung.

## Befehl und Ergebnis

- `cd /workspace/projekte/board-plugin && node --test test/schritt32.mjs` → Exit-Code 0.
- Entscheidende Zeile: `ok Sichtbarkeit: Lücke erkannt, Schreiben abgewiesen, ohne Token Befehle, mit Token gepusht, Lesemodus`
- `node --test` → 31 von 31 bestanden, Exit-Code 0.
- `node --check rechte.js`, `node --check index.js`, `node --check client.js` → je Exit-Code 0.

## Geprüft und offen

- Geprüft: Sichtbarkeit aus Home, Elternordner und Name (Home gewinnt); Lücken bei öffentlichem Board; 409 ohne Commit für private Station im öffentlichen Workflow und für privaten Workflow im öffentlichen Board; erlaubter Weg nach Umschalten; Befehle ohne Token; Anlegen bzw. Verbinden, Remote, Push samt Tags und Sichtbarkeit mit Token gegen den nachgebildeten Server und lokale Bare-Repositories; Lesemodus mit `darf: false` und 403 beim Verschieben ohne Änderung an `vorhaben.json`.
- Die echten Repositories unter `/workspace/projekte/board-repos` wurden nur gelesen: `komponiere` und `pruefeVerweise` liefern dort 11/1/0 (Hauptboard) und 11/1/4 (Team), null Lücken, Sichtbarkeit unverändert; alle 18 Arbeitsbäume sind sauber.
- Kein echter GitHub-Aufruf; die Veröffentlichung ist gegen einen nachgebildeten Server und lokale Bare-Repositories geprüft, nicht live.
- Die Oberfläche ist mit `node --check` geprüft, nicht im Browser durchgeklickt.
