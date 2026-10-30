# Bericht 94 — Kanban-Layout

## Gebaut

- Reine Funktionen in `client.js`: `istKanban(workflow)` prüft `workflow.schlagworte` ohne Groß-/Kleinschreibung auf `kanban`; `kanbanSpalten(karten, vorhaben)` sortiert die Spalten nach `x`, dann `y` und ordnet jedem Vorhaben genau die Spalte seiner `position` zu (`v.position === p.id`).
- Kanban-Fläche: waagerechte Reihe, Spalten 320 px breit mit 16 px Abstand, `overflow-x: auto`, Höhe der Fläche, oben ausgerichtet.
- Spaltenkopf: Stationsname über höchstens zwei Zeilen, darunter klein der Host, rechts der Zähler der Vorhaben; Klick auf den Namen öffnet die Station über `schrittWaehlen(p.schritt, true)` im rechten Dock.
- Spaltenkörper: Chips über `chipFuer(v, true)`, 8 px Abstand, senkrecht rollbar; leerer Körper sagt `uebersetze('Nichts in dieser Spalte')`. Dateien, Beiträge und Start/Schreiben erscheinen dort nicht.
- Chips im Kanban nicht ziehbar: `draggable: imKanban !== true && darf.verschieben === true`, `cursor: 'default'`, Ziehen schon im `onDragStart` abgewiesen; Klick und Handlungen bleiben.
- Der öffentliche Reiter ist nie Kanban: die Entscheidung hängt an `gewaehlterReiter.workflow`, nicht an der eigenen Fläche.
- „Alles zeigen“ hat einen Kanban-Zweig: Spaltenreihe `scrollLeft/scrollTop = 0`, äußere Fläche ebenfalls nach oben links; Beschriftung unverändert.
- „Nichts in dieser Spalte“ in allen sieben Wörterbüchern; die Schlüsselzahlen 636 → 637 und die Client-Zahl 627 → 628 in den Prüfungen mitgezogen.
- `test/nach94.mjs` (neu, selbstständig): `istKanban`, Spaltenreihenfolge für `frei` und `auto`, Zuordnung von drei Vorhaben auf zwei Positionen, Textprüfungen des Clients, Kanban-Vorlage mit vier Positionen, sieben Wörterbücher.
- `docs/entwicklung.md`: Abschnitt „Kanban“ mit Spaltenbreite, Reihenfolge und Ausnahme des öffentlichen Reiters; Abschnittsliste ergänzt.

## Geänderte Dateien

- `client.js`
- `texte/ar.json`, `texte/en.json`, `texte/es.json`, `texte/fr.json`, `texte/ja.json`, `texte/ru.json`, `texte/zh.json`
- `test/nach94.mjs` (neu), `test/grundlagen.mjs`, `test/nach82.mjs`, `test/nach83.mjs`, `test/nach84.mjs`, `test/nach85.mjs`, `test/nach85b.mjs`, `test/markt39.mjs` (Suche auf die neue Signatur `chipFuer(v, …)`)
- `docs/entwicklung.md`
- `.github/workflows/ci.yml` (Zahl der selbstständigen Prüfungen 61 → 62)

## Zahlen nach dem Lauf

Gemessen mit dem Filter aus `.github/workflows/ci.yml`:

- Prüfdateien: **96**
- selbstständige Prüfungen: **62**
- Werkstatt-Prüfungen: **34** (unverändert)

`.github/workflows/ci.yml` und `docs/entwicklung.md` tragen 96 / 62 / 34; die Wörterbuchzahl ist 637.

## Befehl und Exit-Code

```
cd /workspace/projekte/board-plugin && npm run check && node --test test/*.mjs
```

Exit-Code: **0** — `check` grün, 96 Prüfungen, 96 bestanden.

Zusätzlich der CI-Filter:

```
node --test $(ls test/*.mjs | grep -vE '/(markt(25|26|27|42|45|46|48)|schritt(03|0[5-9]|1[0-9]|2[0-9]|30))\.mjs$')
```

Exit-Code: **0** — 62 Prüfungen, 62 bestanden.

## Entscheidende Zeile

`client.js:9237` — `const kanbanFlaeche = !kanban ? null : h('div', {`

Die Entscheidung davor: `client.js:8148` — `const kanban = !!gewaehlterReiter && istKanban(gewaehlterReiter.workflow);`
