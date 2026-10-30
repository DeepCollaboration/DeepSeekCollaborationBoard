# Schritt 90 — Bericht

## Gebaut

- Vier Einführungsstationen unter `vorlagen/start/`: `station-vorhaben` („Vorhaben und Ideen“), `station-kennenlernen` („Kennenlernen“, mit `gespraech` „Erzähl, was du vorhast“), `station-eigene` („Eigene Station“, mit Verweis auf die Entwicklungshilfe) und `station-erste-idee` („Erste Idee“). Jede trägt eine `station.json` in Format 2 mit `host: p1`, `reife: entwurf`, `lizenz: MIT`, leeren `beispielen`, echtem Eingang und Ausgabe, einem `modul` und einer kurzen `README.md`.
- Die Module haben je mindestens zwei Blöcke, davon einen `text`; jeder `knopf` trägt `ziel: "weiter"` (die eigene Station zusätzlich den https-Verweis). Die Kette der `weiter`-Namen läuft Vorhaben und Ideen → Kennenlernen → Eigene Station → Erste Idee → Vorhaben und Ideen, sodass der Kreis zu ist.
- `workflow-einfuehrung/workflow.json`: `id: einfuehrung`, Titel „Das Board kennen lernen“, eine Beschreibung, vier Stationen mit Fassung `v1`, Positionen `x = 0/400/800/1200` und `y = 0`, drei Kanten als Kette, `layout: auto`, `regeln: {}`.
- `board-start/board.json`: dritter Workflow-Verweis in der Reihenfolge öffentlich, kanban, einfuehrung; die vier Einführungsstationen stehen zusätzlich in `stationen`, damit die Bibliothek und die Komposition sie führen. Die README der Board-Vorlage nennt die Einführung.
- `test/nach90.mjs`: selbstständig unter `/tmp/board-scheibe1/nach90`; prüft Vorlage, Module, Blockarten, `weiter`-Namen, das Anlegen von drei Workflows und dreizehn Stationen (je ein Commit, Tag `v1`, sauber), die Reihenfolge und die Komposition ohne `fehlt`, `fassungFehlt` oder Lücke.
- `test/nach89.mjs`: Zahlen auf 17 Vorlagen, drei Workflows, dreizehn Stationen und dreizehn Bibliotheksstationen nachgezogen; die Prüfung bleibt inhaltlich gleich streng.
- `docs/entwicklung.md`: Einführung als dritte Workflow-Vorlage genannt, der Satz, dass ihre Texte Inhalt sind und beim Ändern der Vorlage mitgepflegt werden, sowie 88 Prüfdateien und 54 selbstständige Prüfungen. `erststart.js` und der CI-Kommentar tragen dieselben Zahlen.

## Geänderte Dateien

- `vorlagen/start/station-vorhaben/` (neu: `station.json`, `README.md`)
- `vorlagen/start/station-kennenlernen/` (neu: `station.json`, `README.md`)
- `vorlagen/start/station-eigene/` (neu: `station.json`, `README.md`)
- `vorlagen/start/station-erste-idee/` (neu: `station.json`, `README.md`)
- `vorlagen/start/workflow-einfuehrung/` (neu: `workflow.json`, `README.md`)
- `vorlagen/start/board-start/board.json`, `vorlagen/start/board-start/README.md`
- `test/nach90.mjs` (neu)
- `test/nach89.mjs`
- `docs/entwicklung.md`
- `erststart.js` (nur der Kommentar zur Zahl der Vorlagen)
- `.github/workflows/ci.yml` (nur die Zahl der selbstständigen Prüfungen: 54)

Nicht angefasst: `client.js`, `index.js`, `komposition.js`, `formate/`, `locale/`, `texte/`, `package.json`.

## Prüfung

```
cd /workspace/projekte/board-plugin && npm run check && node --test test/*.mjs
```

Exit-Code: **0** — `check` grün, 88 Tests, 88 bestanden, 0 fehlgeschlagen (die bisherigen 87 plus `nach90`).

## Entscheidende Zeile

`vorlagen/start/board-start/board.json:23` — `"repo": "workflow-einfuehrung",`

Der Verweis steht als dritter nach dem Kanban und legt damit die Reihenfolge öffentlich, kanban, einfuehrung fest. Ergänzend `vorlagen/start/station-vorhaben/station.json:21` — `"weiter": "Kennenlernen",`: die Kette nennt Stationsnamen, keine Kennungen.
