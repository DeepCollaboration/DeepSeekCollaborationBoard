# Bericht Schritt 13 — Die vorhandenen Inhalte umziehen

## Angelegt

- `/workspace/projekte/board-repos/` mit 18 Repositories:
  - 11 Stationen: `station-idee`, `station-idee-vision`, `station-vision`, `station-vision-fachkonzept`, `station-fachkonzept`, `station-fachkonzept-architektur`, `station-architekturplan`, `station-architektur-umsetzung`, `station-idee-zu-produkt`, `station-umsetzen`, `station-vidionpruefen`
  - 1 Workflow: `workflow-standard`
  - 4 Vorhaben: `vorhaben-board-gesamtkonzept`, `vorhaben-board-gesamtkonzept-idee-zu-produkt`, `vorhaben-board-gesamtkonzept-vidionpruefen`, `vorhaben-second-brain-gesamtkonzept`
  - 2 Boards: `board-hauptboard`, `board-team`
  - Jedes Repository: Branch `main`, ein Commit, sauberer Status; Stationen und Workflow tragen Tag `v1`.
- `/workspace/projekte/board-repos/sichtbarkeit.json`: 13-mal `öffentlich` (11 Stationen, Workflow, Hauptboard), 5-mal `privat` (4 Vorhaben, Team-Board).
- `/workspace/projekte/board-repos/umzug.md`.
- `/workspace/projekte/board-plugin/werkzeuge/zeige-umzug.mjs` (neu): liest ein Zielverzeichnis, prüft jedes Repository mit `liesStation`, `liesWorkflow`, `liesVorhaben`, `liesBoard`, liest `sichtbarkeit.json` und gibt JSON aus — Zahl je Art, Namen, je Board die Verweise, je Vorhaben Position und Workflow, je Station Host und Fassung. Bei einem Formatfehler Exit 1 mit Repository und Grund.
- Register `/dsh-home/board/boards.json`: alter Eintrag `hauptboard` bleibt (ergänzt um `sichtbarkeit: 'öffentlich'`), neu `hauptboard-neu` („Hauptboard (neu)“, Format 2, `art: hauptboard`, `sichtbarkeit: 'öffentlich'`, `repo: /workspace/projekte/board-repos/board-hauptboard`, `remote: null`, `person: 'DeepCollaboration'`) und `team` („Team“, Format 2, `art: team`, `sichtbarkeit: 'privat'`, `repo: /workspace/projekte/board-repos/board-team`, `remote: null`, `person: 'DeepCollaboration'`); `testboard` entfernt; `aktiv: 'hauptboard-neu'`.
- Sicherung des alten Registers: `/tmp/board-scheibe1/register-vor13.json` (vor dem Schreiben, byte-gleich mit dem alten Stand).
- Keine Commits im Produkt; keine Änderung am Inhalt der alten Ablage.

## Befehl und Ergebnis

- `node werkzeuge/umzug2.mjs --quelle /workspace/projekte/board --ziel /workspace/projekte/board-repos --host DeepCollaboration --bericht /workspace/projekte/board-repos/umzug.md`
  - Exit-Code: 0
  - Entscheidende Zeilen: `ok 11 Stationen, 1 Workflow, 4 Vorhaben, 2 Boards` / `ok Bericht: /workspace/projekte/board-repos/umzug.md`
- `node werkzeuge/zeige-umzug.mjs --ziel /workspace/projekte/board-repos`
  - Exit-Code: 0
  - Entscheidende Zeile (gekürzt): `{"stationen":11,"workflows":1,"vorhaben":4,"boards":2,"hauptboard":{"stationen":11,"workflows":1,"vorhaben":0},"team":{"stationen":11,"workflows":1,"vorhaben":4}}`
  - Alle Stationen: Host `DeepCollaboration`, Fassung `v1`. Die vier Vorhaben stehen an ihren alten Positionen: `umsetzen`, `idee-zu-produkt`, `vidionpruefen-2`, `vision`.
- Gegenprobe Formatfehler (kaputte `station.json` unter `/tmp`): Exit-Code 1, `FEHLER: station-kaputt: Station: „aktion“ fehlt oder ist kein Objekt`.
- Registerprüfung (`node -e`, liest `/dsh-home/board/boards.json`): 3 Boards, `aktiv: 'hauptboard-neu'`, kein `testboard`; alter Eintrag behalten.
- Alte Ablage: `git -C /workspace/projekte/board rev-parse HEAD` vorher und nachher `ae905d81b080cbb9aae986ca2804723b24dd2d6a`; `git status --porcelain` beim Vergleich direkt nach dem Umzug vor dem Schreiben dieses Berichts identisch (24 Einträge aus der laufenden Arbeit — der Arbeitsbaum war schon vor dem Umzug nicht leer); kein Commit.

## Zusatzprüfung Komposition

- `komponiere({ repo: '/workspace/projekte/board-repos/board-hauptboard', person: 'DeepCollaboration' })` → 11 Schritte, 0 Vorhaben, 0 Lücken.
- `komponiere({ repo: '/workspace/projekte/board-repos/board-team', person: 'DeepCollaboration' })` → 11 Schritte, 4 Vorhaben, 0 Lücken.
- Die nackten Verweise (`workflow-standard`, `station-…`) lösen sich dabei als Geschwisterordner auf.
- `node --test test/schritt30.mjs` → Exit 0, `ok Umzug: 11 Stationen, 1 Workflow, 4 Vorhaben, 2 Boards; Quelle unverändert`, `pass 1`, `fail 0`.

Hinweis: Der laufende Web-Prozess hält noch den Registerstand von vor dem Schreiben im Speicher; maßgeblich ist die Datei für den nächsten Start.
