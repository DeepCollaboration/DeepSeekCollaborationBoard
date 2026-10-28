# Bericht Schritt 03 — Die Ablage trägt Personen und Workflows

- **Stand:** 2026-10-26
- **Gebaut:** Die Ablage liegt in der Form, die zwischen zwei Installationen
  gilt: `format`, eine Personendatei, eine Workflow-Datei und fünf
  `vorhaben.json`. Das Modul liest und schreibt nur noch diese Dateien;
  `board.json` bleibt unverändert liegen und wird nicht mehr angefasst.
- **Nicht neu gestartet:** Das laufende Board arbeitet weiter mit dem geladenen
  alten Modul. Die Prüfung „ohne board.json“ lief auf einer Kopie in `/tmp`.

## Die Ablage

| Pfad | Inhalt |
| --- | --- |
| `format` | `1` und Zeilenumbruch |
| `personen/p1.json` | `{ id: "p1", name: "Thomas Heinrich", farbe: "#fbbf24" }` |
| `workflows/p1.json` | `{ person: "p1", workflows: [ { id, titel, schritte, positionen, kanten, layout } ] }` |
| `vorgaenge/<ordner>/vorhaben.json` | je Vorhaben `id`, `name`, `position`, `verantwortliche`, `regel`, `rueckkehr`, `laeufe`, `fork_von`, `zweig_von`; `farbe` bleibt als Anzeigefeld |

Der Umzug ergab fünf Vorhaben-Dateien; `find vorgaenge -name vorhaben.json`
zählt 5. Jeder Schritt trägt `id`, `name`, `host`, `eingang`, `ausgabe`, `wie`
und `skill` (`skill` aus `wie.art: "skill"`, sonst `null`); `inhalt` und
`werkstatt` sind erhalten. Jede Position trägt `x`, `y` und ihren `schritt`.
`abzweig` wurde zu `zweig_von`; `gespraeche` und `ordner` stehen nicht mehr in
der Datei, `ordner` folgt aus dem Verzeichnisnamen.

`board.json` ist inhaltlich unverändert (`git diff -- board.json` ohne Ausgabe).

## Das Modul

- **`produkt/ablage.js`** ist neu: Es liest `format`, `personen/`, den ersten
  Workflow der eigenen Person, alle Vorhaben und die persönlichen Seiten; es
  schreibt aus demselben Board-Objekt wieder Dateien — je Datei nur, wenn sich
  ihr Inhalt ändert. Vor dem Schreiben prüft es `format`; ist die Ablage neuer
  als das Modul, schreibt es nicht.
- **`produkt/index.js`** liest und schreibt nur noch über `ablage.js`.
  `DATA_PATH`, `TMP_PATH` und der alte `board.json`-Pfad sind entfernt.
  `/board/data` behält die Form `{ workflow, schritte, vorgaenge, personen,
  einstellungen }`. `abzweig` wird aus `zweig_von` abgeleitet, `laeufe` und
  `farbe` stehen an den Vorhaben, `ich` kommt aus `board.config.json`.
- **Einstellungen** liegen unter `$DSH_HOME/board/einstellungen.json`. Das
  Verzeichnis legt das Modul beim Schreiben an.
- **Gespräche** stehen nicht mehr am Vorhaben (`gespraeche` entfällt). Damit die
  Seite ihr Gespräch am Schritt wiederfindet, liegen die Sitzungsverweise
  persönlich unter `$DSH_HOME/board/gespraeche.json` — außerhalb der geteilten
  Ablage, wie die Einstellungen. `/board/data` liefert sie weiter mit; die
  Sitzungen selbst bleiben in `$DSH_HOME/sessions`.
- **Fassungen:** Jede schreibende Route ruft weiter `festhalten` mit dem
  Urheber aus der Personendatei. Die Werkstatt-Routen tun das jetzt auch, weil
  `werkstatt` in der Workflow-Datei liegt.

## Umzug

```bash
cd /workspace/projekte/board/vorgaenge/board-gesamtkonzept
node produkt/werkzeuge/umzug.mjs \
  --repo=/workspace/projekte/board --home=/dsh-home
```

**Exit 0.** Ausgabe: `ok format: 1`, sieben neue Dateien, `0 waren schon da`,
`ok 5 Vorhaben mit vorhaben.json`. `board.json` blieb liegen. Ein zweiter Lauf
auf einer Kopie legte nichts neu an (Exit 0) — das Skript ist wiederholbar und
überschreibt vorhandene neue Dateien nicht.

Die persönlichen Seiten konnten in dieser Sitzung nicht nach `/dsh-home/board`
geschrieben werden: Die Schreibgrenze der Sitzung lässt nur `/workspace` zu
(`EACCES: permission denied, mkdir '/dsh-home/board'`). Der Umzug legt
`einstellungen.json` und `gespraeche.json` deshalb als Ersatz unter
`/tmp/board-umzug-seiten/` ab (Inhalt: `mausrad: seitwaerts` und die bisherigen
Sitzungsverweise). Das neue Modul legt `$DSH_HOME/board/` beim ersten Schreiben
selbst an. Bis die Ersatzdateien dorthin kopiert sind, fehlt die Einstellung
„Mausrad: seitwärts“ und die Gesprächsliste ist beim Start leer; die Sitzungen
selbst bleiben erhalten.

## Prüfungen

| Nr | Befehl | Ergebnis | Exit |
| --- | --- | --- | --- |
| 1 | `cat format` | `1` | 0 |
| 2 | `ls personen workflows` | `personen/p1.json`, `workflows/p1.json` | 0 |
| 3 | `find vorgaenge -name vorhaben.json \| wc -l` | `5` | 0 |
| 4 | `node produkt/test/schritt02.mjs` | alle Zeilen `ok`, Urheber Thomas Heinrich | 0 |
| 5 | `node produkt/test/schritt03.mjs` | gleiche Vorhaben, Schritte, Personen, Positionen; Route schreibt; Fassung nennt Thomas Heinrich | 0 |
| 6 | `curl -s -o /dev/null -w '%{http_code}' "$DSH_WEB_URL/board/data"` | `200` (noch altes Modul) | 0 |
| 7 | `git -C /workspace/projekte/board log -1 --format='%an %s'` | `Thomas Heinrich Umzug auf Format 1` | 0 |
| 8 | `git -C /workspace/projekte/board status --porcelain` | leer | 0 |

Ausgabe von `schritt03.mjs`:

```
ok Kopie: /tmp/board-schritt03-…/ablage (ohne board.json)
ok /board/data: 5 Vorhaben, 11 Positionen, 11 Schritte, 1 Person
ok Vorhaben: board-gesamtkonzept, board-gesamtkonzept-idee-zu-produkt, board-gesamtkonzept-vidionpruefen, second-brain-gesamtkonzept, test
ok Positionen: … (11 Lagen, dieselben wie im Abzug)
ok Personen: p1|Thomas Heinrich|#fbbf24|true
ok /board/doc: architektur.md, 28306 Zeichen
ok Route schreibt: vorgaenge/board-gesamtkonzept/notiz-umzug.md
ok Fassung: Thomas Heinrich — „notiz-umzug.md“ gespeichert
```

## Entscheidungen

- **`gespraeche` nicht in `vorhaben.json`.** Der Auftrag sagt das ausdrücklich.
  Damit das Gespräch am Schritt nicht verloren geht, liegen die Verweise
  persönlich unter `$DSH_HOME/board/gespraeche.json`, nicht in der Ablage. Folge:
  Die Ablage ist zwischen zwei Installationen frei von Sitzungskennungen; die
  Seite liest sie weiter über `/board/data`.
- **`weiter` wird mitgeschrieben**, wenn es gesetzt ist. Es steht nicht in der
  Feldliste, die Route `/board/schritt/aendern` kennt es aber; in den heutigen
  Daten kommt es nicht vor. So geht eine gesetzte Weiterreichung nicht verloren.
- **`format` steht nicht in `/board/data`.** Die Form der Seite bleibt wie
  bisher; die Formatnummer prüft nur der Schreiber.
- **Die Fassung des Boards** (`/board/fassungen`) zeigt jetzt den ganzen
  Repository-Verlauf statt nur `board.json` — die eine Datei gibt es nicht mehr.

## Grenzen

- Kein Neustart; das laufende Board antwortet weiter mit `200`.
- `board.json` wurde nicht gelöscht und nicht verändert.
- Keine Vorhaben umgeschrieben; in `vorgaenge/` wurden nur die neuen
  `vorhaben.json` angelegt, keine anderen Dateien gelöscht.
- Die persönlichen Seiten liegen ersatzweise in `/tmp` (siehe „Umzug“); die
  Schreibgrenze dieser Sitzung ließ `/dsh-home/board` nicht zu.
- Das Produkt bleibt wie in Schritt 02 aus der Ablage heraus (`vorgaenge/*/produkt/`
  ist ignoriert); die Änderungen an `produkt/` sind deshalb nicht Teil der Fassung.
