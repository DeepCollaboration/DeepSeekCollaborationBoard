# Bericht 93b — Größe und Einrasten

## Gebaut

- Drei feste Kartenstufen mit einer geordneten Liste an einer Stelle: 1×1 = 240×240, 2×1 = 500×240, 2×2 = 500×500; `breiteFuer(p)` und `hoeheFuer(p)` lesen `p.groesse`, fehlt sie, gilt 1×1.
- Feste Kartenhöhe, der Kartenkörper rollt innen (`overflow: auto`, `minHeight: 0`).
- Griff unten rechts (14×14, Diagonale, `role="button"`, `tabIndex 0`, `aria-label` „Größe ändern“): sichtbar bei Hover/Fokus, auf `pointer: coarse` immer. Ziehen ändert die Größe live und wählt beim Loslassen 2×1, 2×2 oder zurück 1×1; Enter/Leertaste schalten 1×1 → 2×1 → 2×2 → 1×1.
- Reines `rasteEin(neu, andere, masse)`: 10-px-Raster, Nachbarkanten unter 8 px, Untergrenze 0, nächster Nachbar gewinnt; im Ziehen benutzt.
- `platzieren` sendet in Fassung 2 `workflow` und bei Änderung `groesse`; Position und Größe kommen aus der Antwort der Route. Der öffentliche Reiter trägt `p.workflow` an jeder Karte; ohne Herkunft gibt es keinen Griff und kein Ziehen.
- „Größe ändern“ in allen sieben Wörterbüchern; Schlüsselzahl 636.

## Geänderte Dateien

- `client.js`
- `texte/ar.json`, `texte/en.json`, `texte/es.json`, `texte/fr.json`, `texte/ja.json`, `texte/ru.json`, `texte/zh.json`
- `test/nach93b.mjs` (neu), `test/nach83.mjs`, `test/nach64b.mjs`, `test/grundlagen.mjs`, `test/nach82.mjs`, `test/nach84.mjs`, `test/nach85.mjs`, `test/nach85b.mjs`
- `docs/entwicklung.md`
- `.github/workflows/ci.yml` (Nachtrag: Zahl der selbstständigen Prüfungen 60 → 61)

## Befehl und Exit-Code

```
cd /workspace/projekte/board-plugin && npm run check && node --test test/*.mjs
```

Exit-Code: **0** — `check` grün, 95 Prüfungen, 95 bestanden.

## Entscheidende Zeile

`client.js:1714` — `const KARTEN_STUFEN = ['1x1', '2x1', '2x2'];`

## Nachtrag nach dem ersten Prüflauf

- `.github/workflows/ci.yml:21`: „die CI führt die 60“ auf „61“ gesetzt.
- Befehl (Filter aus der CI-Datei):

```
node --test $(ls test/*.mjs | grep -vE '/(markt(25|26|27|42|45|46|48)|schritt(03|0[5-9]|1[0-9]|2[0-9]|30))\.mjs$')
```

- Exit-Code: **0** — 61 Prüfungen, 61 bestanden (61/61).
