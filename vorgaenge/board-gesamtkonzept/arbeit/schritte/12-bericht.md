# Bericht Schritt 12 — Das Umzugswerkzeug

## Gebaut

- `werkzeuge/umzug2.mjs`: liest eine Ablage der Fassung 1 und schreibt je Ding ein eigenes lokales Git-Repository unter das Ziel — 11 Stationen (`station.json`, `README.md`, die kopierte Anweisung als `SKILL.md`, `CHANGELOG.md`, Branch `main`, Commit, Tag `v1`), 1 Workflow (Verweise mit `v1`, alte Positionen und Kanten, Layout, Autor = Host, Commit, Tag `v1`), 4 Vorhaben (`vorhaben.json`, alle Dateien und `transkripte/`, Verantwortliche mit dem Host statt `p1`, `zweig_von` als `vorhaben-<von>`, Commit) sowie `board-hauptboard` (öffentlich, 11 Stationen, 1 Workflow, keine Vorhaben) und `board-team` (privat, dazu alle 4 Vorhaben). Dazu `sichtbarkeit.json` (13 öffentlich, 5 privat) und ein Bericht, standardmäßig `<ziel>/umzug.md`.
- Ohne `--quelle`, `--ziel` oder `--host`, bei einem nicht leeren Ziel und bei fehlender oder falscher Fassung bricht das Werkzeug mit Exit 1 und einer Meldung ab, bevor es schreibt. Die Quelle wird nur gelesen; `git` läuft ausschließlich in den neuen Repositories.
- Die Stationen tragen 5-mal die Aktionsart `skill` mit byte-gleicher `SKILL.md`, 4-mal `datei` und 2-mal `textfeld`. Der im Auftrag genannte Pfad `skills/<person>/<wert>.md` trifft die alte Ablage nicht: `wie.wert` trägt dort den Präfix `p1-`, die Dateien heißen nach dem Schritt. Gesucht wird deshalb in dieser Reihenfolge: genannter Pfad, der im Schritt eingetragene Pfad `skill`, dann `skills/<person>/<schritt-id>.md`.
- `test/schritt30.mjs`: kopiert die echte alte Ablage nach `/tmp/board-scheibe1/schritt30/quelle`, fasst die Kopie einmal sauber und lässt den Umzug nur dort laufen. Geprüft werden 18 Repositories mit den vier Formatlesern, Branch `main`, Tag `v1` bei Stationen und Workflow, die kopierten Anweisungen und Vorhaben-Dateien, die Boards, `sichtbarkeit.json`, der Bericht, gleicher `HEAD` und leerer Status der Kopie, Unverändertheit der echten Ablage sowie fünf Fehlerfälle mit einer winzigen eigenen Ablage (ohne `--host`/`--quelle`/`--ziel`, volles Ziel, Format 2, fehlendes `format`).

## Dateien

- `/workspace/projekte/board-plugin/werkzeuge/umzug2.mjs` (neu)
- `/workspace/projekte/board-plugin/test/schritt30.mjs` (neu)
- Keine weitere Produktdatei geändert; keine Commits.

## Prüfung

- Befehl: `cd /workspace/projekte/board-plugin && node --test test/schritt30.mjs`
- Exit-Code: 0
- Entscheidende Zeile: `ok Umzug: 11 Stationen, 1 Workflow, 4 Vorhaben, 2 Boards; Quelle unverändert`
- Zusatz: `node --test` → `tests 29`, `pass 29`, `fail 0`, Exit-Code 0.
- Zusatz: `node --check werkzeuge/umzug2.mjs` → Exit-Code 0.
- Die echte Ablage `/workspace/projekte/board` behält `HEAD ae905d8` und ihren Stand von 23 Einträgen in `git status --porcelain`; die Prüfung arbeitet auf einer bereinigten Kopie, weil die echte Ablage durch die laufende Arbeit bereits uncommittete Dateien trägt.
