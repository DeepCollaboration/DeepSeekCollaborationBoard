# Bericht 95 — Vorhaben verschieben

## Gebaut

- **Recht in Fassung 2:** `darfVon` prüft die eigene Kennung und den GitHub-Namen (`eigeneNamen`); der Client vergleicht `verantwortlicheJeStation[position]` mit Login **oder** `data.person`/`ich.id`. Die Route gibt `PERSON_ID` als `kennung` an `istVerantwortlich` weiter, das Eintragungen in beiden Formen annimmt. Fassung 1 bleibt bei der Kennung; `darf` und die F1-Wege sind unberührt.
- **Ziehen im Kanban:** Der Chip ist ziehbar, wenn `darf.verschieben` gilt (`grab`, sonst wie bisher). Der Spaltenkörper ist Ziel: `onDragOver` setzt `ziel` und hebt die Spalte hervor, `onDragLeave` nimmt die Hervorhebung zurück, `onDrop` ruft `verschiebenNach` ohne Bestätigungspanel und löscht `ziel`. Ein Fehlschlag meldet seinen Grund; die Karte bleibt, weil die Antwort die Daten nicht ersetzt.
- **Pfeile:** Im Kanban kommen ◀/▶ aus `kanbanSpalten(karten, [])` — eine Spalte zurück/weiter in der Spaltenreihenfolge; am Rand sind sie aus (`disabled` wie bisher). Ohne Kanban bleiben die Kanten die Nachbarn.
- **WIP-Grenze:** `pruefeRegeln` in `formate/workflow.js` prüft `regeln.wip` als positive ganze Zahl, wenn sie da ist; `komposition.js` reicht `regeln` ins Workflow-Bild. Der F2-Verschiebe-Weg zählt nach dem Zug die Vorhaben der Zielspalte und antwortet `ok: true` plus `warnung: 'wip'`, wenn die Grenze überschritten ist. Der Client zeigt den Hinweis über `hinweis(…)` (⚠, nicht `fehler`), färbt den Spaltenzähler über der Grenze und nennt die Grenze im `title` (`Vorhaben ≤ n`).
- **Vorlage:** `workflow-kanban/workflow.json` bleibt bei `regeln: {}`.
- **Texte:** „Zu viele Vorhaben in dieser Spalte“ in `client.js` und den sieben Wörterbüchern; die Schlüsselzahlen der Prüfungen sind 638/629.
- **Prüfung:** `test/nach95.mjs` (neu, selbstständig): Format (`wip` 1/3 gültig, 0/-1/'3'/2.5/null Fehler, ohne `wip` unverändert), Zug mit genau einer Fassung nur für `vorhaben.json`, zweimal = zwei Fassungen, `wip: 1` → `ok: true` + `warnung: 'wip'`, `wip: 5` → keine Warnung, 403 ohne Schreibrecht, 403 ohne Verantwortung, 200 über Login und über Kennung, Client-Text (Spaltenziel, direkter Weg, Pfeile spaltenweise, Hinweis, Zähler), sieben Wörterbücher, Datensatz-Doku.
- **`test/nach94.mjs`** auf den Stand von 95 gebracht (Chip mit Griff statt ohne) und bleibt grün.

## Geänderte Dateien

- `client.js`, `index.js`, `komposition.js`, `rechte.js`, `formate/workflow.js`
- `texte/ar.json`, `texte/en.json`, `texte/es.json`, `texte/fr.json`, `texte/ja.json`, `texte/ru.json`, `texte/zh.json`
- `docs/datenformate.md`, `docs/entwicklung.md`, `docs/architektur.md`, `docs/fehlersuche.md`
- `README.md`, `README.de.md`, `.github/workflows/ci.yml`
- `test/nach95.mjs` (neu), `test/nach94.mjs`, `test/grundlagen.mjs`, `test/nach82.mjs`, `test/nach83.mjs`, `test/nach84.mjs`, `test/nach85.mjs`, `test/nach85b.mjs`

## Befehl und Exit-Code

```
cd /workspace/projekte/board-plugin && npm run check && node --test test/*.mjs
```

Exit-Code: **0** — `check` grün, 97 Prüfungen, 97 bestanden.

Zusätzlich der CI-Filter:

```
node --test $(ls test/*.mjs | grep -vE '/(markt(25|26|27|42|45|46|48)|schritt(03|0[5-9]|1[0-9]|2[0-9]|30))\.mjs$')
```

Exit-Code: **0** — 63 Prüfungen, 63 bestanden.

## Zahlen nach dem Lauf

Gemessen mit dem Filter aus `.github/workflows/ci.yml`:

- Prüfdateien: **97**
- selbstständige Prüfungen: **63**
- Werkstatt-Prüfungen: **34** (unverändert)
- Wörterbuchschlüssel: **638** (629 Texte aus `client.js` plus neun Sätze aus `rechte.js`)

`.github/workflows/ci.yml` trägt 63; `docs/entwicklung.md` 97 / 63 / 34 / 638 / 629; die READMEs 97 / 63; `docs/architektur.md` 97 / 63; `docs/fehlersuche.md` 63.

## Entscheidende Zeile

`client.js:9382` — `verschiebenNach(id, p.id);` (der Kanban-Weg ohne Bestätigung).

Der WIP-Teil entscheidet sich in `index.js:8034` — `...(grenze > 0 && anzahl > grenze ? { warnung: 'wip' } : null),`.
