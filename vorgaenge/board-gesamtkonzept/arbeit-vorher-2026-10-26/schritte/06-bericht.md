# Bericht Schritt 06 — Jedes Vorhaben nennt seinen Verantwortlichen

- **Stand:** 2026-10-26
- **Gebaut:** `verantwortliche` in allen fünf Vorhaben; eine Prüfstelle
  `darf(vorhaben, aktion, person)` im Host; `verantwortet` und `darf` in
  `/board/data`; 403 auf fremde Handlungen; Weitergeben mit Eintrag an der
  Zielstation; „verantwortet von …“ und die Handlung „Weitergeben“ in der Seite;
  Prüfskript `produkt/test/schritt06.mjs`; die Fassung `Verantwortliche`.
- **Nicht neu gestartet:** Das laufende Board arbeitet weiter mit dem geladenen
  alten Modul. Alles Neue ist an einer Modulkopie in `/tmp` geprüft; die echte
  Ablage bekam nur die fünf Verantwortlichen-Einträge.

## Die Übernahme (einmalig)

In den fünf heutigen Vorhaben steht die eigene Person an der Station, an der das
Vorhaben gerade steht. Der Schlüssel ist die Station, der Wert `["p1"]`;
`regel` blieb überall `"einer"`. Die Einträge stehen in der echten Ablage, nicht
nur in der Prüfkopie.

| Vorhaben | Station | Eintrag |
| --- | --- | --- |
| `board-gesamtkonzept` | `umsetzen` | `{"umsetzen":["p1"]}` |
| `board-gesamtkonzept-idee-zu-produkt` | `idee-zu-produkt` | `{"idee-zu-produkt":["p1"]}` |
| `board-gesamtkonzept-vidionpruefen` | `vidionpruefen-2` | `{"vidionpruefen-2":["p1"]}` |
| `second-brain-gesamtkonzept` | `vision` | `{"vision":["p1"]}` |
| `test` | `idee` | `{"idee":["p1"]}` |

Das ist die Übernahme: Es gab vorher keinen Verantwortlichen, und es gab keine
Stelle, an der er hätte entstehen können. Ab jetzt entsteht der Eintrag beim
Anlegen, beim Weitergeben und beim Zurückschicken von selbst.

## Die eine Prüfstelle (`produkt/index.js`)

- `AKTIONEN` sind die vier Handlungen `aendern`, `verschieben`, `weitergeben`,
  `zurueckschicken`.
- `verantwortlicheVon(vorhaben)` liest `verantwortliche[position]` und gibt eine
  Liste von Kennungen zurück; ein fehlendes oder falsch geformtes Feld ergibt
  eine leere Liste.
- `darf(vorhaben, aktion, person)` ist die Stelle, an der alle Handlungen
  vorbeikommen. Sie ist wahr, wenn die Person an der jetzigen Station eingetragen
  ist. Bei mehreren Einträgen entscheidet `regel`: `einer` heißt, ein
  Eingetragener genügt. `alle` heißt, keiner allein — die Zustimmung aller
  bräuchte eine Stelle, an der sie zusammenkommt, und die gibt es noch nicht;
  solange gilt `alle` nur, wenn genau diese eine Person eingetragen ist.
- Was später dazukommt (annehmen, Rücksprung, Beiträge), fragt dieselbe Stelle.
  Heute rufen es die vier schreibenden Routen; das Anlegen setzt den ersten
  Eintrag, statt zu fragen.
- `darfVon(vorhaben)` beantwortet die vier Fragen für die eigene Person;
  `verantwortetVon(vorhaben, personen)` setzt die Namen zusammen. Fehlt eine
  Personendatei, steht ihre Kennung statt eines erfundenen Namens.

## `/board/data`

Jedes Vorhaben trägt jetzt zusätzlich:

```json
{
  "verantwortliche": { "umsetzen": ["p1"] },
  "regel": "einer",
  "verantwortet": "Thomas Heinrich",
  "darf": {
    "aendern": true,
    "verschieben": true,
    "weitergeben": true,
    "zurueckschicken": true
  }
}
```

`verantwortet` und `darf` werden bei jedem Lesen abgeleitet und nie in die
`vorhaben.json` geschrieben; `vorgangDatei` speichert nur die Felder aus der
Ablage. Nach dem Weitergeben an `p2` steht dort `verantwortet: "Zweite Person"`
und alle vier `darf`-Werte sind für `p1` falsch.

## Die schreibenden Routen

| Route | Aktion | Verhalten |
| --- | --- | --- |
| `/board/vorhaben/aendern` | `aendern` | 403, wenn die eigene Person nicht eingetragen ist |
| `/board/vorhaben/verschieben` | `verschieben` bzw. `weitergeben` | 403 vor jeder Änderung; mit `person` ist es ein Weitergeben |
| `/board/vorhaben/abzweigen` | `aendern` | 403; der Zweig bekommt den Anlegenden an seiner Zielstation |
| `/board/vorhaben/zurueckschicken` | `zurueckschicken` | 403 vor der Positions- und Grundprüfung; Eintrag an der Zielstation wie beim Weitergeben |
| `/board/vorhaben` | — (neu) | wer anlegt, wird an der ersten Station eingetragen |

Die Prüfung steht jeweils nach dem Finden des Vorhabens und vor jedem Schreiben.
Im Prüfskript antworten alle vier Routen auf ein fremdes Vorhaben mit 403; die
fremde `vorhaben.json` ist danach byte-gleich.

## Weitergeben

„Weitergeben“ ist eine Handlung am Vorhaben und nennt Zielstation und nächste
verantwortliche Person. Sie liegt auf `/board/vorhaben/verschieben` mit dem
zusätzlichen Feld `person`; eine fünfte Route gibt es nicht, die vier genannten
Routen bleiben die vier. Die Route prüft dann `darf.weitergeben`, trägt
`verantwortliche[<zielstation>] = [person]` ein und verschiebt das Vorhaben
dorthin. Ohne `person` bleibt es beim Abgebenden: Ist an der Zielstation niemand
eingetragen, wandert der bisherige Eintrag mit. Dasselbe gilt beim
Zurückschicken; der alte Eintrag an der verlassenen Station bleibt stehen, weil
er die Geschichte des Vorhabens erzählt und für die jetzige Station nicht zählt.

## Die Seite (`produkt/client.js`)

- An jedem Vorhaben steht unter dem Namen „verantwortet von ‹Name›“ — auch am
  nicht ausgewählten Vorhaben.
- Die Handlungsreihe erscheint nur, wenn `darf` mindestens eine Handlung erlaubt.
  „Weitergeben“ erscheint nur bei `darf.weitergeben`; ein fremdes Vorhaben bekommt
  keine Reihe und ist nicht ziehbar.
- Die Handlung öffnet ein kleines Formular: Zielstation (die nächsten Stationen)
  und nächste verantwortliche Person (alle Personen der Ablage). Vorgeschlagen
  ist der Host der Zielstation. Nach dem Weitergeben antwortet der Host mit dem
  neuen Stand; die Handlung verschwindet sofort.
- Ein weitergegebenes Vorhaben bleibt in der Spalte seiner Zielstation stehen,
  jetzt mit dem Namen der neuen verantwortlichen Person und ohne Handlungen.
- Ein fremdes Vorhaben an einer Station, die es auf meiner Fläche nicht gibt,
  steht unter der Fläche seiner Person: „Vorhaben ‹Name› an ‹Station› —
  verantwortet von ‹Name›“, ohne „Weitergeben“. Steht die Station auch auf meiner
  Fläche, erscheint es dort einmal und nicht doppelt.

## Dateien

- `produkt/index.js` — `AKTIONEN`, `darf`, `verantwortlicheVon`, `darfVon`,
  `verantwortetVon`; `datenAntwort`; die fünf Routen.
- `produkt/client.js` — Anzeige, Handlung, Formular, fremde Vorhaben an ihrer
  Station.
- `produkt/test/schritt06.mjs` — neu.
- `vorgaenge/*/vorhaben.json` — die fünf Übernahme-Einträge.
- `arbeit/schritte/06-bericht.md` — dieser Bericht.

## Prüfungen

| Nr | Befehl | Ergebnis | Exit |
| --- | --- | --- | --- |
| 1 | `node produkt/test/schritt06.mjs` | eigenes Vorhaben, fremdes Vorhaben, 403, Weitergeben, Anlegen, Seite, echte Ablage | 0 |
| 2 | `node produkt/test/schritt05.mjs` | fremde Fläche und Verweis unverändert | 0 |
| 3 | `node produkt/test/schritt04.mjs` | Abgleich, Speichern, kein Remote unverändert | 0 |
| 4 | `node produkt/test/schritt03.mjs` | 5 Vorhaben, 11 Positionen, 11 Schritte, Fassung nennt Thomas Heinrich | 0 |
| 5 | `node produkt/test/schritt02.mjs` | Ablage, Urheber und Lauf ohne Konfiguration unverändert | 0 |
| 6 | `python3 -c "…"` (unten) | 5 von 5: Eintrag an der eigenen Station, `regel` einer | 0 |
| 7 | `node --check produkt/index.js`, `ablage.js`, `client.js` | keine Syntaxfehler | 0 |
| 8 | `git -C /workspace/projekte/board status --porcelain` | leer | 0 |
| 9 | `git -C /workspace/projekte/board log -1 --format='%an %s'` | `Thomas Heinrich Verantwortliche` | 0 |

Das Python-Prüfkommando:

```bash
cd /workspace/projekte/board
python3 -c "
import glob, json, sys
dateien = sorted(glob.glob('vorgaenge/*/vorhaben.json'))
fehler = [f for f in dateien
          if json.load(open(f)).get('verantwortliche', {}).get(json.load(open(f))['position']) != ['p1']
          or json.load(open(f)).get('regel') != 'einer']
print('ok 5 von 5' if not fehler and len(dateien) == 5 else 'FEHLER: ' + ', '.join(fehler))
sys.exit(0 if not fehler and len(dateien) == 5 else 1)
"
```

Ausgabe von `schritt06.mjs`:

```
ok eigenes Vorhaben: verantwortet von „Thomas Heinrich“, darf.weitergeben true
ok fremdes Vorhaben: verantwortet von „Zweite Person“ an „pruefen“, darf.weitergeben false
ok 403: POST /board/vorhaben/verschieben auf das fremde Vorhaben
ok 403: aendern, abzweigen und zurueckschicken ebenso — dieselbe Stelle
ok Weitergeben: verantwortliche.vision = [p2], position vision, darf.weitergeben für p1 falsch
ok Anlegen: „Frisches Vorhaben“ an „idee“, verantwortet von „Thomas Heinrich“
ok Seite: „verantwortet von“ steht am Vorhaben, „Weitergeben“ nur bei darf.weitergeben
ok echte Ablage unverändert: fünf Einträge, kein fremdes Vorhaben
ok Prüfordner aufgeräumt: /tmp/board-schritt06-…
```

**Entscheidende Zeile:** `ok Weitergeben: verantwortliche.vision = [p2], position
vision, darf.weitergeben für p1 falsch` — das Vorhaben wandert mit seinem neuen
Verantwortlichen, und die Handlung ist danach bei mir weg.

## Entscheidungen

- **Weitergeben liegt auf `/board/vorhaben/verschieben`.** Der Auftrag nennt vier
  schreibende Routen und keine fünfte; das zusätzliche Feld `person` macht aus
  dem Verschieben ein Weitergeben, und dieselbe Stelle entscheidet. Folge: Eine
  Stelle, ein Weg — aber die Route heißt weiter „verschieben“, auch wenn sie
  weiterreicht.
- **`abzweigen` prüft `aendern`.** Es verändert den Bestand an Vorhaben, nicht die
  Lage des Quellvorhabens. Weil heute alle vier Aktionen dieselbe Zugehörigkeit
  prüfen, ist das nur die Etikette für später.
- **`alle` heißt: keiner allein.** Es gibt noch keine Stelle, an der mehrere
  Verantwortliche gemeinsam zustimmen könnten; lieber verweigert die Stelle den
  einzelnen Zugriff, als eine Regel vorzutäuschen. Kein heutiges Vorhaben nutzt
  `alle`.
- **Ohne Eintrag an der Zielstation wandert der Eintrag des Abgebenden mit.**
  Sonst stünde ein verschobenes Vorhaben danach ohne Verantwortlichen da. Der
  alte Eintrag bleibt stehen; er zählt nur an seiner Station.
- **Fremde Vorhaben an Stationen außerhalb meiner Fläche stehen unter der Fläche
  ihrer Person.** Eine Spalte gibt es dort nicht. Ein weitergegebenes Vorhaben
  bleibt dagegen in meiner Spalte, weil seine Station zu meinem Workflow gehört —
  mit dem neuen Namen und ohne Handlungen.
- **`verantwortet` ist ein Name, keine Kennung.** Bei mehreren Verantwortlichen
  stehen ihre Namen durch Komma getrennt; fehlt eine Personendatei, steht die
  Kennung. Es wird nichts geraten.

## Fassung

Der Stand der Ablage ist als Fassung `Verantwortliche` festgehalten (Urheber
`Thomas Heinrich`, `thomas.heinrich@board`). Sie enthält die fünf
Verantwortlichen-Einträge, diesen Bericht und die Arbeitsstände, die beim Schritt
im Arbeitsbaum lagen. Die Änderungen am Produkt (`produkt/index.js`,
`produkt/client.js`, `produkt/test/schritt06.mjs`) sind wie in den Schritten davor
nicht Teil der Board-Fassung: `vorgaenge/*/produkt/` ist im Repository ignoriert.

## Grenzen und offene Punkte

- **Nicht neu gestartet.** Das laufende Board antwortet weiter mit dem alten
  Modul; `verantwortet`, `darf`, die 403-Prüfung und die neue Seite erscheinen
  erst nach einem Neustart.
- **Im Browser nicht gesehen.** Die Seite ist syntaktisch geprüft, und das
  Prüfskript belegt, dass Anzeige und Handlung im Client stehen und an
  `darf.weitergeben` hängen. Das tatsächliche Bild ist nicht geprüft, weil dafür
  ein Neustart nötig wäre.
- **Die zweite Person lebt nur in der Prüfkopie.** Die echte Ablage hat weiter
  nur `p1`, kein fremdes Vorhaben und keinen `p2`-Workflow.
- **Die Prüfung ist eine Prüfung im Board, keine Sperre im Repository.** Wer am
  Board vorbei schreibt, kann weiterhin alles ändern; die Fassungen zeigen, wer
  es war.
