# Schritt 89 — Bericht

## Gebaut

- `vorlagen/start/` mit zwölf Ordnern und je Ankerdatei plus README:
  - `board-start/board.json`: Format 2, `id: start`, Name „Mein Board“, `art: lokal`, eine lokale Person `p1` („Ich“, damit die Komposition ohne Personenlücke bleibt), leere `vorhaben`, Verweise auf beide Workflows und alle neun Stationen.
  - `workflow-oeffentlich/workflow.json`: „Öffentliches Board“, fünf Stationen (Idee, Ausarbeitung, Umsetzung, Prüfung, Veröffentlichung), `layout: auto`, Positionen `x = 0…1600`, `y = 0`, vier Kanten, `regeln: {}`.
  - `workflow-kanban/workflow.json`: „Kanban“, vier Stationen (Idee, In Arbeit, Prüfung, Fertig), gleiche Form mit `x = 0…1200` und drei Kanten.
  - Neun `station-<slug>/station.json`: Name, Beschreibung, sprechender Eingang/Ausgang, `aktion.art: textfeld`, `reife: entwurf`, `lizenz: MIT`, leere `tags`. Kein Schlagwort `kanban`.
- Alle Verweise sind Geschwisternamen; jede Fassung ist `v1`.
- `legeStartboardAn()` in `erststart.js`: legt Board, zwei Workflows und neun Stationen unter `$DSH_HOME/board/start/` als echte Repositories an (Anker + README kopiert, `git init -b main`, ein Commit mit der festen Identität `Board <board@local>`, Tag `v1`), schreibt erst danach den Registereintrag (`format: 1`, `id: start`, `format: 2`, `art: lokal`, absoluter Repo-Pfad, `remote/person: null`, `sichtbarkeit: privat`). Ein vorhandenes aktives Board bleibt aktiv; ein vorhandener Eintrag `start` führt zu `{ angelegt: false }`. Bei Abbruch wird der halbe Ordner entfernt, das Register bleibt unangetastet.
- `package.json` führt `vorlagen/start/` unter `files`; `npm pack --dry-run` enthält alle 24 Vorlagendateien.
- `docs/entwicklung.md`: Abschnitt „Startboard und Vorlagen“ (Ort, Geschwisternamen, `v1`, Station ergänzen, einmaliges Anlegen); Testzahlen auf 87/53 nachgezogen.
- `test/nach89.mjs`: zwölf gültige Vorlagen, frisches Home, genau ein Eintrag und offen, je ein Commit mit Tag `v1` und leerem `git status`, Komposition mit zwei Workflows und neun Stationen ohne `fehlt`/`fassungFehlt`/Lücken, zweiter Aufruf unverändert, vorhandenes `hauptboard` bleibt aktiv, keine Stationsplatzhalter.

## Geänderte Dateien

- `erststart.js`
- `package.json`
- `docs/entwicklung.md`
- `.github/workflows/ci.yml` (nur die Zahl der selbstständigen Prüfungen: 53)
- `test/nach89.mjs` (neu)
- `vorlagen/start/` (neu, 24 Dateien)

In diesem Schritt nicht angefasst: `index.js`, `client.js`, `komposition.js`, `formate/`, `locale/`, `texte/`. `legeStartboardAn()` wird noch von niemandem aufgerufen.

## Prüfung

```
cd /workspace/projekte/board-plugin && npm run check && node --test test/*.mjs
```

Exit-Code: **0** — `check` grün, 87 Tests, 87 bestanden, 0 fehlgeschlagen (die bisherigen 86 plus `nach89`).

## Entscheidende Zeile

`erststart.js:172` — `if (hatStartEintrag(register)) return { angelegt: false };`

Ergänzend `erststart.js:205` — `schreibeRegister(zuhause, geschrieben);` steht erst nach allen Repositories.
