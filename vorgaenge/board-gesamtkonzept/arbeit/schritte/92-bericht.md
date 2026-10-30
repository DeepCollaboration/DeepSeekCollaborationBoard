# Bericht Schritt 92 — Bibliothek mit Schlagworten und Herkunft

## Was gebaut wurde

- `workflow.json` trägt das freiwillige Feld `schlagworte` (Liste nicht leerer Texte, höchstens 20 Einträge, je höchstens 40 Zeichen). `pruefeWorkflow` ist als eigene Prüfung herausgezogen, `liesWorkflow` reicht das Feld durch, `schreibeWorkflow` verliert es beim Fortschreiben nicht. Fehlt das Feld, gilt die leere Liste; alte Workflows bleiben gültig.
- Neue reine Regel `herkunftVon(eintrag, ich)` in `formate/herkunft.js`: eigener Host/Autor → `eigen`; sonst `privat`, wenn `sichtbarkeit` nicht `öffentlich` ist; sonst `oeffentlich`. Ohne eigenen Login wird nichts `eigen`.
- `komposition.js` nimmt `tags` der Stationen und `schlagworte` der Workflows in `data.stationen[]` und `data.workflows[].workflows[]` auf und setzt dort die Herkunft. `index.js` setzt sie beim Zusammenstellen von `/board/data` noch einmal aus derselben Regel und reicht `schlagworte` auch über `GET /board/workflow` durch.
- Bibliothek: Filterzeile über den drei Abschnitten mit vier Herkunftsknöpfen (`Alle`, `Eigen`, `Öffentlich`, `Privat`, aktiver Knopf mit `aria-pressed`), Schlagwortknöpfen (ohne Doppel, alphabetisch, dazu `Alle`; ohne Schlagworte entfällt die Gruppe), Einfachauswahl je Dimension, sichtbaren Karten und aus den sichtbaren Karten gezählten Abschnittszahlen. Passt nichts, stehen `Nichts passt zum Filter` und `Filter zurücksetzen`. Herkunft und Schlagworte stehen als kleine Zeile unter dem Kartennamen, durch `·` getrennt. Der Filter lebt nur im Bibliothekszustand und wird nicht gespeichert.
- Vorlagen: Kanban trägt `kanban`, Einführung `einfuehrung`, das öffentliche Board `grundlage`; die dreizehn Stationen tragen je ein bis zwei Tags (`planung`, `bau`, `pruefung`, `einfuehrung`, `kanban`).
- Sieben neue Texte in allen sieben Wörterbüchern (`Herkunft`, `Schlagworte`, `Eigen`, `Privat`, `Nichts passt zum Filter`, `Filter zurücksetzen`, `Alle`); `Öffentlich` wird wiederverwendet.
- Neue Prüfung `test/nach92.mjs`; `test/nach89.mjs` prüft statt „keine Schlagworte“ jetzt genau die drei Schlagwortlisten. Schlüsselzahlen in den bestehenden Prüfungen auf 635 gezogen.

## Geänderte Dateien

- Neu: `formate/herkunft.js`, `test/nach92.mjs`
- Geändert: `formate/workflow.js`, `komposition.js`, `index.js`, `client.js`
- Dokumentation: `docs/datenformate.md` (Tabelle, Beispiel, „Was fehlen darf“), `docs/entwicklung.md` (Schlüsselzahl, 93 Prüfdateien, 59 selbstständige Prüfungen)
- CI: `.github/workflows/ci.yml` (Kommentar auf 59 selbstständige Prüfungen)
- Vorlagen: `vorlagen/start/workflow-kanban/workflow.json`, `workflow-einfuehrung/workflow.json`, `workflow-oeffentlich/workflow.json` sowie alle dreizehn `vorlagen/start/station-*/station.json`
- Wörterbücher: `texte/ar.json`, `texte/en.json`, `texte/es.json`, `texte/fr.json`, `texte/ja.json`, `texte/ru.json`, `texte/zh.json`
- Prüfungen: `test/nach89.mjs`, `test/grundlagen.mjs`, `test/nach82.mjs`, `test/nach83.mjs`, `test/nach84.mjs`, `test/nach85.mjs`, `test/nach85b.mjs`

## Befehl und Exit-Code

```
cd /workspace/projekte/board-plugin && npm run check && node --test test/*.mjs
node --test $(ls test/*.mjs | grep -vE '/(markt(25|26|27|42|45|46|48)|schritt(03|0[5-9]|1[0-9]|2[0-9]|30))\.mjs$')
```

Exit-Code: **0** — `check` grün, Suite 93 Prüfungen, 93 bestanden, 0 fehlgeschlagen. CI-Filter: 59 Prüfungen, 59 bestanden, 0 fehlgeschlagen, Exit-Code **0**. `test/` hat 93 Dateien; `.github/workflows/ci.yml` und `docs/entwicklung.md` nennen jetzt 59 selbstständige Prüfungen und 93 Prüfdateien.

## Entscheidende Zeile

`formate/herkunft.js:36` — `return sichtbarkeit === 'öffentlich' ? 'oeffentlich' : 'privat';`
