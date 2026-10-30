# Schritt 91a — Bericht

## Gebaut

- `vorlagen/start/vorhaben-probedurchlauf/` (neu): `vorhaben.json` in Format 2 mit `id: probedurchlauf`, `name: Probedurchlauf`, `workflow: { repo: workflow-einfuehrung, fassung: v1 }`, `position: vorhaben` (erste Position der Einführung, „Vorhaben und Ideen“), `verantwortliche: { vorhaben: ["p1"] }`, `laeufe: []` und `farbe: #fbbf24`. Dazu eine `README.md` in drei Sätzen; sie sagt, dass das Vorhaben nur auf diesem Rechner liegt.
- `vorlagen/start/board-start/board.json`: `vorhaben: [{ repo: "vorhaben-probedurchlauf" }]` als Geschwistername, wie die Workflow-Verweise.
- `erststart.js`, `kopiereVorlagen`: `vorhaben.json` steht in der Liste der zu kopierenden Ankerdateien und in der Prüfung auf genau eine Ankerdatei. Kein Sonderfall je Ordnername; der Kommentar von `legeStartboardAn` nennt das Vorhaben.
- `rechte.js`, `sichtbarkeitAusName`: `board-start` gilt nach der Namensregel als privat, wie `board-team` und `vorhaben-*`. Ohne diese Zeile meldete `komponiere` auf dem frischen Startboard eine Sichtbarkeitslücke („Mein Board ist öffentlich und verweist auf ein privates Vorhaben“), obwohl das Startboard im Register als privat steht; `test/nach89.mjs` prüft weiterhin `luecken.length === 0` und wurde nicht abgeschwächt.
- `test/nach91a.mjs` (neu): prüft die Vorlage mit `liesVorhaben`, die erste Position und die Station „Vorhaben und Ideen“ mit `liesWorkflow`/`liesStation`, den Geschwisterverweis in der Board-Vorlage mit `liesBoard`, dann `legeStartboardAn()` unter `/tmp/board-scheibe1/nach91a`: achtzehn Repositories, das Vorhaben als eigenes Repository mit einem Commit, Tag `v1`, sauberem Stand und denselben Dateien wie die Vorlage, und `komponiere` (der Weg von `/board/data`) mit dem Vorhaben an seiner Station, ohne `fehlt`, mit Namen „Probedurchlauf“ und ohne Lücke.
- `test/nach89.mjs`: 17 → 18 Vorlagen und Repositories, Vorhaben-Verweis statt leerem `vorhaben`-Feld, Ankerdatei `vorhaben.json`, das Vorhaben in der Komposition. Keine Prüfung entfernt oder gelockert.
- `test/nach90.mjs`: 17 → 18 Repositories, Vorhaben in der Beschreibung, Ankerart je Ordner über `liesRepo` statt einer toten Variablen.
- `docs/entwicklung.md`: die Vorhaben-Vorlage in der Aufzählung, `vorhaben.json` in der Ankerliste, ein Absatz zum Probedurchlauf; die Zahl der Prüfdateien 89 → 90 und der selbstständigen Prüfungen 55 → 56 mitgezogen. `docs/datenformate.md` unverändert; `client.js`, `index.js` und `komposition.js` unverändert.

## Geänderte Dateien

- `vorlagen/start/vorhaben-probedurchlauf/vorhaben.json` (neu)
- `vorlagen/start/vorhaben-probedurchlauf/README.md` (neu)
- `vorlagen/start/board-start/board.json`
- `erststart.js` (Kopierliste und Ankerprüfung, Kommentar)
- `rechte.js` (Namensregel `board-start` privat)
- `test/nach91a.mjs` (neu)
- `test/nach89.mjs`, `test/nach90.mjs`
- `docs/entwicklung.md`, `.github/workflows/ci.yml` (nur die Zahl 55 → 56)

## Prüfung

```
cd /workspace/projekte/board-plugin && npm run check && node --test test/*.mjs
```

Exit-Code: **0** — `check` grün, 90 Tests, 90 bestanden, 0 fehlgeschlagen (die bisherigen 89 plus `nach91a`).

## Entscheidende Zeile

`vorlagen/start/vorhaben-probedurchlauf/vorhaben.json:9` — `"position": "vorhaben",`

Das ist die Kennung der ersten Position in `workflow-einfuehrung`, nicht die Stations-Id `station-vorhaben`; daran hängt die Karte an der Stelle „Vorhaben und Ideen“. Ergänzend `rechte.js:79` — `if (text === 'board-team' || text === 'board-start'`: die eine Zeile, die die Sichtbarkeitslücke des lokalen Startboards schließt.
