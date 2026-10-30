# Schritt 91b — Bericht

## Gebaut

- `erststart.js`, `sicherstellen()`: Ist das Register leer (keine `boards.json` oder keine Einträge), wird `legeStartboardAn({ home })` gerufen; `holeHauptboard()` und `holeVerweise()` laufen dort nicht mehr. Ein vorhandenes Register bleibt unverändert, auch der Sonderfall „genau ein Hauptboard mit `fehler`“ (weiter Klonversuch). `holeHauptboard()`, `holeVerweise()` und `wiederholen()` sind unverändert.
- Fehlerfall: Scheitert `legeStartboardAn()` (Platte voll, Rechte), fängt `sicherstellen()` den Fehler, schreibt eine Warnung (`Board: Startboard beim Start nicht angelegt: …`) und gibt ein leeres Register zurück. Kein halber Eintrag, kein `fehler`-Eintrag, kein Klonversuch; `/board/data` liefert danach `leer: true`, der Client zeigt den Willkommensbildschirm.
- `index.js`, `apply()`: nur Kommentar und Warnungstext an den neuen Erststart angepasst; die Zuweisung von `REGISTER`, `AKTIV`, `PERSON_ID`, `REPO` und `VORGANG_DIR` ist unverändert.
- `test/schritt33.mjs`: holt den Klon in `starten()` direkt über `holeHauptboard()`, dann `apply`; alle bisherigen Erwartungen (Klon, Register, `/board/data`, 409, „nicht geholt“, Wiederholen) bleiben.
- `test/schritt33b.mjs`: holt Klon und nackte Verweise direkt über `wiederholen()`, dann `apply`; alle Erwartungen an Cache, Lücken und Nachholen bleiben.
- `test/schritt39.mjs`: `wiederholen({ home: HOME })` vor `apply`; die Prüfung des frischen Homes bleibt davor.
- `test/markt44.mjs`: `wiederholen({ home: HOME })` vor `apply`; der vorhandene Zwischenspeicher wird weiter ohne GitHub-Aufruf gelesen.
- `test/schritt17.mjs`: `starten()` hat einen ausdrücklichen `hole`-Schalter; der zweite Start ruft `holeHauptboard({ home: dshHome })` direkt, bevor `apply` läuft.
- `test/nach91b.mjs` (neu, selbstständig): frisches Home und `sicherstellen()` → genau ein Eintrag `start`, `aktiv: 'start'`, `art: 'lokal'`, ohne `fehler`; `board.json` mit drei Workflows und dem Vorhaben; `/board/data` liefert Board, drei Workflows, dreizehn Stationen und „Probedurchlauf“ ohne `leer`; Startweg von `apply` mit einer ins Leere zeigenden `hauptboardAdresse` → Startboard in unter 30 s, kein Klon, kein `fehler`; zweiter Start → Registertext, 18 Repositories und alle Commits unverändert; Fehlerfall (Home mit `board` als Datei) → leeres Register, keine `boards.json`.
- Dokumentation: `README.de.md` und `README.md` beschreiben den Erststart als lokales Startboard und das Hauptboard als ausdrücklich geholten Weg; `docs/fehlersuche.md` trennt „leeres Register“ (Startboard gescheitert) von „nicht geholt“ (Hauptboard); `docs/entwicklung.md` beschreibt den Aufruf in `sicherstellen()`, den Fehlerfall und zieht die Prüfzahlen 90 → 91 und 56 → 57 mit; `docs/architektur.md` nennt die neue Aufgabe von `erststart.js`; `.github/workflows/ci.yml` führt im Kommentar 57 selbstständige Prüfungen.

## Geänderte Dateien

- `erststart.js` (`sicherstellen`, Kopf- und Funktionskommentar)
- `index.js` (Kommentar und Warnungstext in `apply`)
- `test/schritt33.mjs`, `test/schritt33b.mjs`, `test/schritt39.mjs`, `test/markt44.mjs`, `test/schritt17.mjs`
- `test/nach91b.mjs` (neu)
- `README.de.md`, `README.md`
- `docs/fehlersuche.md`, `docs/entwicklung.md`, `docs/architektur.md`
- `.github/workflows/ci.yml` (nur Zahl im Kommentar)

## Prüfung

```
cd /workspace/projekte/board-plugin && npm run check && node --test test/*.mjs
```

Exit-Code: **0** — `check` grün, 91 Tests, 91 bestanden, 0 fehlgeschlagen (die bisherigen 90 plus `nach91b`).

## Entscheidende Zeile

`erststart.js:448` — `const angelegt = legeStartboardAn({ home });`

Der leere Registerzweig ruft das Startboard statt des Hauptboard-Klons; der Fehlerzweig darunter (`erststart.js:453`) gibt bei einem gescheiterten Anlegen das leere Register zurück, ohne einen Eintrag zu schreiben.
