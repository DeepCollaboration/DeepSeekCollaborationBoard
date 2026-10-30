# Schritt 97 — Bericht

## Was geprüft wurde

- `npm run check` (Syntax von `client.js` und `index.js`) und
  `node --check komposition.js`.
- Die ganze Suite `node --test test/*.mjs` — 98 Prüfdateien.
- Der CI-Filter aus `.github/workflows/ci.yml` — dieselben 98 Dateien ohne die
  34 Werkstatt-Prüfungen.
- `node werkzeuge/abnahme.mjs` — Prüfungen der neuen Form, Suite, Live-Probe,
  beide echten Boards, Register und alte Ablage.
- `docs/routen.md` gegen die `registriere`-Aufrufe in `index.js`: 71 von 71
  Pfaden in der Tabelle, keiner fehlt, keiner zu viel.
- `docs/datenformate.md` gegen `formate/*.js`: `modul` (Arten, Bildpfad,
  1–50 Blöcke), `schlagworte` (20/40), `groesse` (`1x1`, `2x1`, `2x2`),
  `regeln.wip` (positive ganze Zahl), `knopf.ziel` (auch `token`),
  `einstellungen.einfuehrung` — gedeckt.
- Die Zahlen in `README.md`, `README.de.md`, `docs/architektur.md`,
  `docs/fehlersuche.md` gegen die Messung: 98 Prüfdateien, 64 selbstständige
  Prüfungen, 34 Werkstatt-Prüfungen, 640 Schlüssel.
- Live gegen den laufenden Harness: `GET /board/data` 200 (Format 2),
  `GET /board/sprache?id=en` 200 mit 640 Schlüsseln.
- Die Startboard-Vorlagen: Die öffentliche steht jetzt auf `layout: "frei"`,
  die Positionen bleiben in einer Reihe; Kanban und Einführung bleiben `auto`.
  `test/nach89.mjs` hält das ausdrücklich fest (statt die Prüfung zu streichen).
- Kanban im Startboard gegen den echten Host, in einem temporären Zuhause mit
  Token und `anlegeweg: attrappe`:
  - Anlegen an `kanban-idee` → 200, `vorhaben.json` nennt `workflow-kanban`;
  - Anlegen an `idee` → 200, `vorhaben.json` nennt `workflow-oeffentlich`;
  - Zug `kanban-idee` → `kanban-in-arbeit` → 200, neue Fassung im Vorhaben-
    Repository;
  - `regeln.wip: 1`, Zug in eine volle Spalte → 200 mit `warnung: wip`.
  Dafür entsteht ein Vorhaben im Workflow der gewählten Station, und ein Zug
  bleibt im Workflow des Vorhabens (vorher nahm die Route immer den ersten
  Workflow des Boards — damit waren Kanban-Wege nicht begehbar).
- `arbeit/abnahmeliste.md`: Wege 17–25 mit „was du tust / was zu sehen sein
  soll“, Kurzfassung auf 17, 18, 21, 24; die Kanban-Wege nennen die
  Voraussetzung „Token eingetragen, zwei Vorhaben an einer Kanban-Station
  angelegt“; Weg 26 (WIP) ist gestrichen und als Hinweis erklärt, weil die
  Kanban-Vorlage bewusst `regeln: {}` trägt.
- `CHANGELOG.md`: Abschnitt `## [Unveröffentlicht]` mit den Änderungen der
  Runde; `0.1.0` bleibt die gesetzte Fassung.
- `umsetzung.md`: Statuskopf, Architektur in drei Sätzen, Stand, Start,
  Fähigkeiten, Grenzen, Ungeprüftes, Entscheidungen, Prüfungen,
  Architektur-Abweichungen, Dateien, offene Punkte.
- `werkzeuge/abnahme.mjs`: Die Suite lief ohne Dateiliste und zählte dadurch
  `test/rechte-texte.js` als 99. Prüfung; sie läuft jetzt wie `npm test` über
  `test/*.mjs` (98).

## Befehle und Exit-Codes

```
cd /workspace/projekte/board-plugin
npm run check                                            Exit 0
node --check komposition.js                              Exit 0
node --check werkzeuge/abnahme.mjs                       Exit 0
node --test test/nach89.mjs test/nach90.mjs test/nach95.mjs
                                                         Exit 0  (3/3)
node --test test/*.mjs                                   Exit 0  (tests 98, pass 98, fail 0)
node --test $(ls test/*.mjs | grep -vE '/(markt(25|26|27|42|45|46|48)|schritt(03|0[5-9]|1[0-9]|2[0-9]|30))\.mjs$')
                                                         Exit 0  (tests 64, pass 64, fail 0)
node werkzeuge/abnahme.mjs                               Exit 0
```

Entscheidende Zeile der Abnahme:

```
Abnahme bestanden: 13 von 13 Prüfungen der neuen Form bestanden; Suite 98 von 98;
sieben Wege belegt; live geprüft (0 Sterne, öffentlich, main); Hauptboard 11
Schritte/1 Vorhaben/0 Lücken (Vorgabe 0 Vorhaben durch Schritt 21 überholt),
Team 11 Schritte/4 Vorhaben/0 Lücken; Register 3 Boards, aktiv hauptboard-neu;
alte Ablage Daten unverändert seit ae905d8 (HEAD 6123b58).
```

Der Abnahmebefund liegt in `arbeit/abnahme.md` (Lauf mit `--bericht`).

## Was sich geändert hat

- `vorlagen/start/workflow-oeffentlich/workflow.json` — `layout: "frei"`,
  Positionen unverändert in einer Reihe. Damit sind Größe, Ziehen und Einrasten
  im Startboard begehbar; Kanban und Einführung bleiben `auto`.
- `test/nach89.mjs` — erwartet `frei` für die öffentliche Vorlage, `auto` für
  die beiden anderen (Prüfung umgestellt, nicht gestrichen).
- `index.js`, `komposition.js` — Anlegen nimmt den Workflow, in dem die
  gewählte Station steht; Verschieben bleibt im Workflow des Vorhabens;
  `ortVon` ist dafür exportiert. `docs/routen.md` beschreibt den neuen Weg.
- `docs/datenformate.md` — `knopf.ziel` nennt jetzt auch `token`.
- `README.md`, `README.de.md` — 98/64; Erststart und Oberfläche beschreiben
  Einführung, Reiter, Kartengröße und Kanban.
- `docs/architektur.md`, `docs/fehlersuche.md` — 98/64.
- `CHANGELOG.md` — `## [Unveröffentlicht]`.
- `werkzeuge/abnahme.mjs` — Suite über `test/*.mjs`.
- `arbeit/abnahmeliste.md` — Wege 17–25, Kurzfassung, Belege.
- `arbeit/abnahme.md` — neu erzeugt (2026-10-30).
- `umsetzung.md` — neu geschrieben, Stand 2026-10-30.

## Abweichungen, die bleiben

- **Sichtprüfung im Browser (86/97) fehlt.** Reiter, Einführung, Filter,
  Kartengröße, Einrasten, Kanban, Ziehen und WIP sind nur im Quelltext und
  gegen den Server geprüft. Das ist seine Abnahme, nicht meine.
- **Echte GitHub-Schreibwege bleiben ungeprüft.** Anlegen, Veröffentlichen,
  Pull Request, Frage, Fork, Teams und Push laufen gegen Nachbildungen; der
  Token der Werkstatt darf nur lesen.
- **Die WIP-Warnung ist im Startboard nicht begehbar.** Die Kanban-Vorlage
  trägt bewusst `regeln: {}`; zwei Vorhaben in einer Spalte brauchen Token und
  Anlegen. Sie ist in `test/nach95.mjs` mit `regeln.wip` geprüft und in
  `arbeit/abnahmeliste.md` als Hinweis erklärt, nicht als Weg geführt.
- **Die fünfte Runde ist nicht veröffentlicht.** 38 geänderte und 16
  unversionierte Dateien liegen über `HEAD bbcd746` (Tag `v0.1.0`); Push und
  Tag stehen bei ihm. Bis dahin liefert die `github:`-Installation den alten
  Stand.
- **Bilder für README und Handbuch fehlen**, der Store-PR ist offen.
- **Architektur-Abweichungen:** Der Erststart legt das Startboard an statt das
  Hauptboard zu holen (Architektur sagt Letzteres); die fünfte Runde (Module,
  Reiter, Startboard, Einführung, Filter, Größen, Kanban) steht nicht in
  `architektur.md`; Schreiben mit Remote hängt am Token statt an Git/SSH;
  Sichtbarkeit ohne Remote kommt lokal aus `sichtbarkeit.json`; Veröffentlichen
  bleibt unter der Organisation. Alle mit Grund in `umsetzung.md`.
- **Kein Werkzeugfehler mehr offen:** Die frühere 99. „Prüfung“ war die
  Helferdatei `test/rechte-texte.js`; die Abnahme zählt jetzt 98 wie `npm test`.

## Zahlen

| Größe | Wert |
| --- | --- |
| Routen unter `/board/*` | 71 |
| Prüfdateien | 98 |
| selbstständige Prüfungen (CI) | 64 |
| Werkstatt-Prüfungen | 34 |
| Wörterbuchschlüssel je Sprache | 640 (sieben Wörterbücher, `de.json` leer) |
| Startboard-Repositories | 18 (1 Board, 3 Workflows, 13 Stationen, 1 Vorhaben) |
| HEAD / Tag | `bbcd746` / `v0.1.0` |
| alte Ablage | Daten unverändert seit `ae905d8`, HEAD `6123b58` |
| Abnahme | 13/13, Suite 98/98, Exit 0 |
