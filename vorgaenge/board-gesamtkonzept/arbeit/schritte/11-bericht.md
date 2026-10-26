# Bericht Schritt 11 — Am fremden Vorhaben speichert der Editor als Beitrag

- **Stand:** 2026-10-26
- **Gebaut:** Wer ein fremdes Vorhaben bearbeitet, schreibt nicht in dessen
  Originaldatei. Der Leser öffnet den Text, „Als Beitrag speichern“ legt Notiz
  und vorgeschlagene Fassungen unter `beitraege/<vorhaben>/<beitrag>/` ab; jeder
  Beitrag ist eine eigene Fassung mit dem Beitragenden als Urheber. `/board/data`
  liefert `beitraege`, die Seite zeigt sie an ihrer Station und im fremden
  Leseblatt, und der Leser kann Notiz und Vorschläge öffnen. Prüfskript
  `produkt/test/schritt11.mjs`; die Fassung `Beiträge`.
- **Nicht neu gestartet:** Das laufende Board arbeitet weiter mit dem geladenen
  alten Modul. Alles Neue ist an einer Modulkopie in `/tmp` geprüft. Die echte
  Ablage bekommt keinen Beitrag und keine zweite Person; `beitraege/` fehlt dort
  weiterhin.

## Der Beitrag

`POST /board/beitrag` nimmt `{ vorhaben, schritt, notiz, dateien }` und schreibt
`beitraege/<vorhaben>/<beitrag>.json` mit genau den Feldern `id, vorhaben,
schritt, von, seit, basis, notiz, dateien, entschieden` (`entschieden` ist
`null`). Je vorgeschlagener Datei liegt der Text unter
`beitraege/<vorhaben>/<beitrag>/<datei>.md`; `dateien` im JSON nennt `{ name,
datei }` mit dem Pfad dieser Fassung. Fehlt die Datei, ist der Beitrag nur eine
Notiz.

- `von` ist die eigene Person aus `personen/<kennung>.json` („Thomas Heinrich“).
- `seit` ist der Zeitpunkt als ISO-Datum.
- `basis` ist die Fassung, auf der der Vorschlag aufsitzt: der Commit von `HEAD`
  beim Anlegen. Das ist für alle vorgeschlagenen Dateien dieselbe Fassung.
- Die Route hängt nicht an `darf.aendern`: Wer das Board sieht, darf beitragen.
  Sie schreibt weder die Originaldatei noch die fremde `vorhaben.json`.
- Mindestens eine Notiz oder eine Datei muss da sein; ein leerer Beitrag antwortet
  400. Dateinamen werden wie überall über `dateiName` bereinigt; Vorschläge ohne
  lesbaren Namen oder ohne Text werden abgewiesen, statt halb zu entstehen.
- Nach dem Schreiben hält `festhalten` den Beitrag als eigene Fassung fest. Der
  Urheber ist der Beitragende (`autorMensch`), nicht der Verantwortliche des
  Vorhabens.
- `GET /board/beitrag?vorhaben=&beitrag=` liest Notiz und vorgeschlagene
  Fassungen wieder. Die Route ist dieselbe `/board/beitrag`; sie hatte im
  Bauplan nur POST, das Öffnen braucht aber einen Leseweg.

`/board/data` ergänzt `beitraege`: je Beitrag `id, vorhaben, vorhabenName,
schritt, station, von, seit, notiz, dateien` (Namen der vorgeschlagenen Dateien)
und `entschieden`. Die Station wird über alle Flächen gesucht, damit auch ein
Beitrag an einem fremden Schritt seinen Namen trägt. Die Liste steht neueste
zuerst.

## Die Seite

- An einer Station erscheinen Beiträge als Knopf mit „Beitrag von ‹Name›“, der
  Notizvorschau und den Namen der vorgeschlagenen Dateien. In der fremden Fläche
  erscheint ein Beitrag, dessen Station auf der eigenen Fläche keinen Ort hat.
- Ein Klick öffnet den Beitrag im Leser: Notiz und jede vorgeschlagene Fassung
  sind zu lesen; angenommen wird nichts.
- Ein fremdes Vorhaben zeigt im Dateileser jetzt „Bearbeiten“. Der Text steht im
  Editor, der Speichern-Knopf heißt dort „Als Beitrag speichern“, und über dem
  Text steht „Notiz zum Beitrag“ samt Hinweis, dass das Original unberührt
  bleibt. Nach dem Speichern zeigt der Leser wieder das Original, die Meldung
  sagt, dass der Beitrag liegt.
- Der Bearbeiten-Knopf an einer fremden **Station** bleibt, wie in Schritt 09
  gebaut, verborgen; geändert wird nur der Dateiinhalt als Beitrag.

## Prüfskript (`produkt/test/schritt11.mjs`)

Das Skript kopiert Ablage und Produkt nach `/tmp`, legt `p2`, ihren Workflow und
ein fremdes Vorhaben `von-p2` mit `vision.md` nur in der Kopie an und entfernt
den Remote, damit kein Abgleich ins Netz greift. Danach prüft es:

1. das fremde Vorhaben steht in `/board/data`, verantwortet von „Zweite Person“,
   und `darf.aendern` ist für die eigene Person falsch;
2. der Beitrag mit geänderter `vision.md` und Notiz entsteht: Antwort und JSON
   tragen die neun Felder, `von` ist „Thomas Heinrich“, `basis` die Fassung vor
   dem Beitrag, `dateien` den Pfad der vorgeschlagenen Fassung;
3. die vorgeschlagene Fassung liegt unter `beitraege/von-p2/<beitrag>/vision.md`
   und nennt „Thomas Heinrich“; das Original und die fremde `vorhaben.json` sind
   byte-gleich;
4. die Fassung nennt als Urheber „Thomas Heinrich“ und enthält nur den Beitrag,
   nicht das fremde Vorhaben; der Arbeitsbaum ist danach ruhig;
5. `/board/data` zeigt den Beitrag an Vorhaben und Station („Prüfen“) mit Name,
   Zeitpunkt, Notiz und `vision.md`;
6. `GET /board/beitrag` liefert Notiz und vorgeschlagene Fassung zum Lesen;
7. ein Beitrag nur mit Notiz wird angenommen (kein Dateiordner); ein leerer
   antwortet 400 und hinterlässt weder Datei noch Fassung;
8. die Seite trägt „Bearbeiten“ am fremden Leser, „Als Beitrag speichern“,
   „Notiz zum Beitrag“ und den Weg zu `/board/beitrag` (Quelltext, kein
   Browserbild);
9. die echte Ablage bleibt ohne `p2`, ohne fremdes Vorhaben und ohne Beitrag.

Ausgabe des letzten Laufs:

```
ok fremdes Vorhaben: verantwortet von „Zweite Person“, darf.aendern falsch
ok Beitrag: JSON mit allen Feldern, Fassung unter beitraege/von-p2/2026-10-26-191732/vision.md, Urheber „Thomas Heinrich“
ok /board/data: Beitrag an Vorhaben und Station, von Thomas Heinrich, Notiz und vision.md
ok Öffnen: Notiz und vorgeschlagene vision.md lesbar
ok Notiz ohne Datei angenommen, leerer Beitrag mit 400 abgewiesen
ok Seite: Bearbeiten am fremden Vorhaben, Speichern als Beitrag mit Notiz
ok Prüfordner aufgeräumt: /tmp/board-schritt11-…
```

**Entscheidende Zeile:** `ok Beitrag: JSON mit allen Feldern, Fassung unter
beitraege/von-p2/<beitrag>/vision.md, Urheber „Thomas Heinrich“` — der Vorschlag
liegt als eigene Datei in der Ablage, das Original bleibt unberührt. Dazu
`ok Notiz ohne Datei angenommen, leerer Beitrag mit 400 abgewiesen` — die
kleinste Form des Beitrags trägt, die leere nicht.

## Dateien

- `produkt/ablage.js` — `liesBeitraege` (296), `beitragDateiname` (342),
  `schreibeBeitrag` (352), `beitraege` in `liesAblage` (422).
- `produkt/index.js` — `stationenVon` (1302), `beitragDateien` (1324),
  `beitragKennung` (1342), `beitraege` in `datenAntwort` (1427), Route
  `/board/beitrag` (1652).
- `produkt/client.js` — Beitrag an der Station (4235), Beiträge in der fremden
  Fläche (4814), `beitragOeffnen` (1956), `beitragSpeichern` (1757),
  `beitragInhalt` (4905), „Als Beitrag speichern“ (5211), „Notiz zum Beitrag“
  (5329).
- `produkt/test/schritt11.mjs` — neu, 438 Zeilen.
- `produkt/test/schritt03.mjs`, `produkt/test/schritt05.mjs` — die erwartete
  Form von `/board/data` trägt jetzt zusätzlich `beitraege`; sonst unverändert.
- `arbeit/schritte/11-bericht.md` — dieser Bericht.

## Prüfungen

| Nr | Befehl | Ergebnis | Exit |
| --- | --- | --- | --- |
| 1 | `node …/produkt/test/schritt11.mjs` | sieben ok-Zeilen: fremdes Vorhaben, Beitrag, `/board/data`, Öffnen, Notiz/leer, Seite, aufgeräumt | 0 |
| 2 | `node …/produkt/test/schritt10.mjs` | fremder Skill-Lauf unverändert | 0 |
| 3 | `node …/produkt/test/schritt07.mjs` | fremdes Vorhaben, 403, Seite unverändert | 0 |
| 4 | `node …/produkt/test/schritt03.mjs` | 5 Vorhaben, 11 Positionen, 11 Schritte | 0 |
| 5 | `node …/produkt/test/schritt02.mjs`, `04`, `05`, `06`, `08`, `09` | alle sieben grün | 0 |
| 6 | `node --check produkt/index.js`, `ablage.js`, `client.js`, `test/schritt11.mjs` | keine Syntaxfehler | 0 |
| 7 | `ls /workspace/projekte/board/beitraege` | fehlt — echte Ablage ohne Beiträge | 2 |
| 8 | `git -C /workspace/projekte/board status --porcelain` | leer | 0 |
| 9 | `git -C /workspace/projekte/board log -1 --format='%an %s'` | `Thomas Heinrich Beiträge` | 0 |

## Entscheidungen

- **Ein Beitrag ist eine eigene Datei, keine Änderung.** Die Route prüft kein
  `darf.aendern`; sie schreibt nur unter `beitraege/`. Damit kann jeder am Board
  Beiträge anlegen, ohne Schreibrecht am fremden Vorhaben zu bekommen.
- **Die Basis ist der Stand des Repositories.** `basis` ist der Commit von `HEAD`
  beim Anlegen, nicht der letzte Commit der einzelnen Datei. Der Vorschlag sitzt
  damit auf dem Stand auf, den die eigene Ablage beim Öffnen hatte; für mehrere
  vorgeschlagene Dateien gibt es eine gemeinsame Basis.
- **Lesen über dieselbe Route.** Der Bauplan nennt nur `POST /board/beitrag`; zum
  Öffnen von Notiz und Fassungen kommt in derselben Route ein GET dazu. Eine
  zweite Route hätte denselben Pfad doppelt belegt.
- **Der Urheber ist der Beitragende.** Der Commit trägt „Thomas Heinrich“, auch
  wenn das Vorhaben der zweiten Person gehört; die fremde `vorhaben.json` und
  `vision.md` bleiben byte-gleich.
- **Die Seite zeigt Beiträge an ihrem Ort.** Auf der eigenen Fläche an der
  Station, in der fremden Fläche dann, wenn die Station dort keinen eigenen Ort
  hat. Geöffnet wird nur gelesen; entschieden wird in Schritt 12.
- **Die Prüfskripte 03 und 05 wurden an die neue Form angepasst.** `/board/data`
  hat jetzt sechs statt fünf Schlüssel; die beiden Skripte prüfen die Form
  ausdrücklich und erwarten `beitraege` mit. Das ist die einzige Änderung an
  älteren Prüfskripten.

## Fassung

Der Stand der Ablage ist als Fassung `Beiträge` festgehalten, Urheber
`Thomas Heinrich` (`thomas.heinrich@board`). Sie enthält den Schritt 11, diesen
Bericht und die Arbeitsstände in `arbeit/`, die beim Schritt im Arbeitsbaum
lagen. Die Änderungen am Produkt (`produkt/ablage.js`, `produkt/index.js`,
`produkt/client.js`, `produkt/test/schritt11.mjs`, `03`/`05`) sind wie in den
Schritten davor nicht Teil der Board-Fassung: `vorgaenge/*/produkt/` ist im
Repository ignoriert.

## Grenzen und offene Punkte

- **Nicht neu gestartet.** Das laufende Board antwortet weiter mit dem alten
  Modul; „Bearbeiten“ am fremden Leser und der Beitrag erscheinen erst nach einem
  Neustart.
- **Im Browser nicht gesehen.** Die Seite ist syntaktisch geprüft und ihre
  Bedingungen am Quelltext belegt; das tatsächliche Bild ist nicht geprüft, weil
  dafür ein Neustart nötig wäre.
- **Keine echte zweite Person in der echten Ablage.** `p2` und `von-p2` leben nur
  in der Prüfkopie; der Schritt hat in der echten Ablage keinen Beitrag erzeugt.
- **Die Station ist eine Positionskennung.** Sie ist innerhalb eines Workflows
  eindeutig, über zwei Workflows hinweg kann dieselbe Kennung zweimal vorkommen.
  `/board/data` und der Leser zeigen dann den ersten gefundenen Namen. Für die
  Prüfkopie ist die Kennung `pruefen` eindeutig; ob das für alle künftigen
  Flächen reicht, ist offen.
- **`basis` ist eine gemeinsame Fassung.** Ein Beitrag mit mehreren Dateien trägt
  nur eine Basis; die Basisprüfung je Datei kommt mit Schritt 15.
- **Annehmbare Teile und Ablehnung fehlen.** `entschieden` bleibt `null`; das
  entscheidet Schritt 12.
