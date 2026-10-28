# Bericht Schritt 07 — Die Bibliothek: Stationen, Workflows, Vorhaben

## Gebaut

- `komposition.js`: Jedes Vorhaben trägt `workflow: { repo, fassung }` aus `vorhaben.json`; fehlt der Verweis, bleiben beide Felder leer. Jeder Workflow trägt im Bild für die Seite den Pfad seines Repositories (`repo`), damit die Einzelheit ihn nennen kann. Die übrigen Felder bleiben.
- `client.js`: Bei `data.format === 2` zeigt die Bibliothek drei Abschnitte. **Stationen** aus `data.schritte` mit Name, Host (Name der Person, sonst Kennung; „ohne Host“) und Fassung („ohne Fassung“); ein Klick öffnet die vorhandene Stationsansicht und schließt die Bibliothek. **Workflows** aus den Workflows der Personen in `data.workflows` mit Titel, Autor, Fassung und Zahl der Stationen; ein Klick öffnet darunter Titel, Autor, Fassung, Beschreibung, Stationen und Repository-Pfad. **Vorhaben** aus `data.vorgaenge` mit Name, Station, Verantwortliche und Workflow-Fassung; ein Klick öffnet darunter Name, Station, Verantwortliche, Fassung, Repository-Pfad und Dateien; „Auf dem Board zeigen“ wählt das Vorhaben und schließt die Bibliothek. Bei Fassung 2 entfällt der Anlege-Knopf. Fassung 1 zeigt unverändert die bisherige Stationsbibliothek samt „+ Neue Station“.
- `test/schritt25.mjs`: eigenes `DSH_HOME` und Testboard aus `legePruefflaeche` unter `/tmp/board-scheibe1/schritt25`; prüft an `GET /board/data` `host` und `fassung` der Stationen, `titel`, `autor`, `fassung` und Repository-Pfad des Workflows, `workflow.fassung` und `verantwortet` des Vorhabens sowie die drei aus den Daten gebildeten Arten. Danach Server, Ablage und Home entfernt.
- `package.json`: unverändert; es entsteht kein neues Modul.

## Dateien

- `/workspace/projekte/board-plugin/komposition.js`
- `/workspace/projekte/board-plugin/client.js`
- `/workspace/projekte/board-plugin/test/schritt25.mjs`

## Prüfung

- Befehl: `cd /workspace/projekte/board-plugin && node --test test/schritt25.mjs`
- Exit-Code: 0
- Entscheidende Zeile: `ok Bibliothek: Stationen „Idee“ und „Umsetzen“ (Host p1, Fassung v1); Workflow „Standard-Workflow“ (Autor p1, Fassung v1, 2 Stationen); Vorhaben „Board Gesamtkonzept“ (Fassung v1, verantwortet von Thomas Heinrich)`
- Zusatz: `cd /workspace/projekte/board-plugin && node --test` → `tests 24`, `pass 24`, `fail 0`, Exit-Code 0; die alte Suite und Schritt 18–25 bestanden.
- Zusatz: `node --check client.js` und `node --check index.js` → Exit-Code 0.
- Die laufende Prüffläche `/tmp/board-scheibe1` blieb unverändert; `git status --porcelain` ist in ihren fünf Repositories leer. Der Prüfordner `schritt25` wurde wieder entfernt. Keine Commits.
