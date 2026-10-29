# Bericht Schritt 11 — Die alte Ablage lesen

## Gebaut

- `test/schritt29.mjs`: Die Prüfung legt unter `/tmp/board-scheibe1/schritt29` eine alte Ablage (`alt`, `format` = 1) und daneben über `legePruefflaeche({ wurzel })` die Prüffläche der Fassung 2 an. Die alte Ablage ist ein eigenes Git-Repository mit einer Person, einem Workflow „Alte Kette“ mit den Schritten „Anfang“ und „Prüfen“ sowie zwei Vorhaben; das erste trägt `idee.md` und ein Transkript unter `transkripte/`, das zweite nur seine `vorhaben.json`. Ein eigenes `DSH_HOME` und ein Register führen beide Boards, offen ist zuerst `alt`; der Server läuft in-process.
- Geprüft wird: `GET /board/data` für das alte Board mit beiden Vorhaben, dem Workflow samt abgeleiteter Datei `idee.md`, der Person und dem Transkript in `laeufe`; `GET /board/dateien` mit `transkripte`, `idee.md` und `vorhaben.json`; `GET /board/datei` mit dem Inhalt von `idee.md` und des Transkripts; `GET /board/doc` mit `idee.md` und ihrer Basis; nach `POST /board/boards/wechsel` auf das Testboard `GET /board/data` mit dem Vorhaben an der Station `idee`, dem Workflow aus den Stationen „Idee“ und „Umsetzen“ und den Dateien des Vorhabens; alter `HEAD`, Zahl der Fassungen und `git status --porcelain` vor und nach allen Lesezugriffen unverändert.
- `index.js` blieb unverändert: Der Fassung-1-Zweig ist vorhanden und richtig. `/board/data` prüft `AKTIV.format === 2` und liest sonst über `liesBoard()`; `/board/doc`, `/board/dateien` und `/board/datei` gehen über `boardFuerDateien()` und `ordnerFuerVorgang()`. Es fehlte nichts, also gab es keine Produktkorrektur.

## Dateien

- `/workspace/projekte/board-plugin/test/schritt29.mjs` (neu)
- Keine Produktdatei geändert.

## Prüfung

- Befehl: `cd /workspace/projekte/board-plugin && node --test test/schritt29.mjs`
- Exit-Code: 0
- Entscheidende Zeile: `ok Alte Ablage: 2 Vorhaben (altes-vorhaben mit idee.md und Transkript, zweites-vorhaben), Workflow „Alte Kette“ mit den Schritten Anfang und Prüfen, Person „Alte Hüterin“; an „start“ liegt die Datei idee.md`
- Zusatz: `cd /workspace/projekte/board-plugin && node --test` → `tests 28`, `pass 28`, `fail 0`, Exit-Code 0.
- Zusatz: `node --check index.js` → Exit-Code 0.
