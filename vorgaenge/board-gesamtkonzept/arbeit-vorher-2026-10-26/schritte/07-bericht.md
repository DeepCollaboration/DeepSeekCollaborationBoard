# Bericht Schritt 07 — Ein fremdes Vorhaben steht auf meinem Board und lässt sich lesen

- **Stand:** 2026-10-26
- **Gebaut:** fremde Vorhaben tragen in `/board/data` die Farbe ihrer
  verantwortlichen Person; `/board/doc/schreiben` und `/board/skill/lauf`
  weisen fremde Vorhaben mit 403 ab, bevor etwas entsteht; ein Speichern des
  eigenen Standes fasst fremde `vorhaben.json` nicht an; die Seite öffnet ein
  fremdes Vorhaben, zeigt seine Dateien und liest den Text ohne
  Bearbeitungsfeld; Prüfskript `produkt/test/schritt07.mjs`; die Fassung
  `Fremde Vorhaben lesen`.
- **Nicht neu gestartet:** Das laufende Board arbeitet weiter mit dem geladenen
  alten Modul. Alles Neue ist an einer Modulkopie in `/tmp` geprüft.

## Was schon da war

Zwei Teile des Auftrags waren bereits erfüllt und sind jetzt geprüft, nicht neu
gebaut:

- `liesVorgaenge` liest jeden Ordner unter `vorgaenge/`, unabhängig davon, wer
  ihn verantwortet. Es gibt keine Filterung nach der eigenen Person. Das
  fremde Vorhaben erscheint deshalb ohne Zutun in `/board/data`.
- `/board/dateien`, `/board/doc` und `/board/datei` arbeiteten schon für jedes
  Vorhaben; sie kannten keine Besitzprüfung. Der neue Prüfskript belegt, dass
  die Dateiliste den fremden Ordner samt `transkripte/` zeigt und `idee.md`
  lesbar ist.

`verantwortet` und `darf` kommen aus Schritt 06 und bleiben unverändert.

## Farbe und Verantwortlicher (`/board/data`)

`datenAntwort` leitet die Farbe bei jedem Lesen ab, wenn das Vorhaben keine
eigene gültige `farbe` hat: Die erste verantwortliche Person an der jetzigen
Station gibt ihre Farbe aus `personen/`. Die Farbe wird nicht in die
`vorhaben.json` geschrieben. Ein eigenes Vorhaben mit eigener Farbe behält sie;
eines ohne eigene Farbe bekommt die Farbe seiner Person. Gibt es keine
verantwortliche Person oder keine Personendatei, bleibt die Farbe weg — die
Seite fällt dann auf ihre Reihenfolge zurück, statt eine Person zu erfinden.
`verantwortet` stand schon vorher daneben.

Im Prüfskript:

- fremdes Vorhaben: `verantwortet: "Zweite Person"`, `farbe: "#8b5cf6"` (aus
  `personen/p2.json`), alle vier `darf`-Werte für `p1` falsch;
- eigenes Vorhaben `board-gesamtkonzept`: eigene Farbe `#60a5fa` bleibt;
- `test` ohne eigene Farbe: `#fbbf24`, die Farbe von `p1`.

## Schreiben: eine Prüfung für alle

- `/board/doc/schreiben` prüft `darf(vorhaben, "aendern", PERSON_ID)` direkt
  nach dem Finden des Vorhabens und antwortet ohne Berechtigung mit 403. Das
  steht vor der Ordnernamen-, Dateinamen- und Ordnerprüfung, vor `mkdirSync`
  und vor jedem Schreiben. Eine neue Datei entsteht damit auch nicht.
- `/board/skill/lauf` schreibt ein Transkript in den Vorhabenordner und trägt
  einen Lauf am Vorhaben ein; es prüft dieselbe Stelle und antwortet mit 403,
  bevor eine Datei oder ein Eintrag entsteht.
- Die vier Handlungen aus Schritt 06 (`aendern`, `verschieben`, `weitergeben`,
  `zurueckschicken`) prüfen weiter dieselbe Stelle.

Zusätzlich bleibt eine fremde `vorhaben.json` auch dann unangetastet, wenn ein
ganz anderes Speichern den Stand schreibt: `liesBoard` merkt jedem Vorhaben
beim Lesen, ob die eigene Person es verantwortet; `baueDateien` überspringt
fremde Vorhaben. Der Vermerk ist nicht aufzählbar, steht weder in der Datei
noch in der Antwort an die Seite. Ein eigenes Vorhaben, das gerade weitergegeben
wird, bleibt dabei schreibbar, weil der Vermerk vom Lesen vor der Änderung
stammt.

## Die Seite (`produkt/client.js`)

- Ein fremdes Vorhaben auf der Fläche seiner Person ist jetzt ein Knopf mit
  Farbpunkt, Stationsnamen und „verantwortet von …“. Er wählt das Vorhaben aus
  und öffnet links seinen Dateibaum — Öffnen, Lesen, Dateien ansehen.
- Steht ein fremdes Vorhaben an einer Station der eigenen Fläche, erscheint am
  ausgewählten Vorhaben der Knopf „Dateien“; die Handlungsreihe bleibt weg,
  weil `darf` nichts erlaubt.
- Der Leser ist für ein fremdes Vorhaben nur Lesen: `leserDarfAendern` schaltet
  den Bearbeiten-Knopf, „Speichern“, „Rechtschreibung“, „Überarbeiten“ und
  „Abzweigen“ ab; ein alter Bearbeiten-Stand wird als Lesen angezeigt. Der
  Verlauf bleibt lesbar.
- „Schreiben“ an einer Station und „Skill starten“ hängen zusätzlich an
  `darf.aendern`; ein Lauf schreibt ein Transkript in den Ordner und ist damit
  kein Lesen.
- Umbenennen, Ziehen und Weitergeben bleiben wie in Schritt 06 an `darf`
  gebunden.

## Prüfskript (`produkt/test/schritt07.mjs`)

Das Skript kopiert die Ablage nach `/tmp`, legt dort `personen/p2.json`, einen
Workflow von `p2` und das fremde Vorhaben `vorgaenge/von-p2/` mit `idee.md` und
`transkripte/` an. Die echte Ablage bekommt nichts davon.

Geprüft werden zehn Punkte:

1. fremdes Vorhaben an „Prüfen“ im Workflow von `p2`, `verantwortet: "Zweite
   Person"`, Farbe `#8b5cf6`, alle Handlungen für `p1` falsch;
2. eigene Farbe bleibt, fehlende eigene Farbe wird zur Farbe der Person;
3. Dateiliste: genau `transkripte, idee.md, vorhaben.json` — kein erfundenes
   `pruefung.md`; jede Zeile liegt wirklich im Ordner;
4. `transkripte/` ist als Ordner da, das Transkript darin über `/board/datei`
   lesbar;
5. `/board/doc` liefert `idee.md`;
6. `/board/doc/schreiben` auf `idee.md`: 403, Datei byte-gleich;
7. das gilt auch für eine neue Datei und für `/board/skill/lauf`; kein
   Transkript entsteht, die fremde `vorhaben.json` bleibt byte-gleich;
8. Gegenprobe: das eigene Vorhaben bleibt schreibbar;
9. ein Speichern des eigenen Standes (`POST /board/vorhaben`) lässt die fremden
   Dateien byte-gleich;
10. die Seite enthält Öffnen/Lesen/Dateien und die Prüfung an `darf.aendern`;
    die echte Ablage hat kein `p2` und kein `von-p2`.

Die Prüfskripte 02 und 04 legten ihr Testvorhaben mit `verantwortliche: {}` an.
Seit `/board/doc/schreiben` `darf` prüft, wäre ihr eigener Schreibversuch 403 —
zu Recht, denn das Vorhaben hätte keinen Verantwortlichen. Die beiden
Testvorhaben tragen jetzt den Eintrag an ihrer Station
(`{ s1: ["p1"] }` bzw. `{ pos1: ["p1"] }`); der Zweck der Skripte
(Ablage, Urheber, Abgleich) bleibt unverändert.

## Dateien

- `produkt/index.js` — `schreibbar`-Vermerk in `liesBoard`; Farbe in
  `datenAntwort` (Zeile 1182); 403 in `/board/doc/schreiben` (Zeile 1379) und
  `/board/skill/lauf` (Zeile 1896).
- `produkt/ablage.js` — fremde Vorhaben beim Schreiben überspringen (Zeile 439).
- `produkt/client.js` — fremdes Vorhaben öffnen (`fremdesVorhabenOeffnen`,
  Zeile 3343), Dateien-Knopf (Zeile 3102), Leser ohne Bearbeiten
  (`leserDarfAendern`, Zeile 4844), Schreiben/Starten an `darf.aendern`
  (Zeilen 1953, 3301, 3462).
- `produkt/test/schritt07.mjs` — neu.
- `produkt/test/schritt02.mjs`, `produkt/test/schritt04.mjs` — Verantwortlichen-
  Eintrag im Testvorhaben.
- `arbeit/schritte/07-bericht.md` — dieser Bericht.

## Prüfungen

| Nr | Befehl | Ergebnis | Exit |
| --- | --- | --- | --- |
| 1 | `node produkt/test/schritt07.mjs` | 10 ok-Zeilen: fremdes Vorhaben, Farbe, Dateien, 403, eigene Gegenprobe, Seite, echte Ablage | 0 |
| 2 | `node produkt/test/schritt06.mjs` | neun ok-Zeilen, Weitergeben und 403 unverändert | 0 |
| 3 | `node produkt/test/schritt05.mjs` | fremde Fläche und Verweis unverändert | 0 |
| 4 | `node produkt/test/schritt04.mjs` | Abgleich, Speichern, kein Remote unverändert | 0 |
| 5 | `node produkt/test/schritt03.mjs` | 5 Vorhaben, 11 Positionen, Fassung nennt Thomas Heinrich | 0 |
| 6 | `node produkt/test/schritt02.mjs` | Ablage, Urheber und Lauf ohne Konfiguration unverändert | 0 |
| 7 | `node --check produkt/index.js`, `ablage.js`, `client.js` | keine Syntaxfehler | 0 |
| 8 | `git -C /workspace/projekte/board status --porcelain` | leer | 0 |
| 9 | `git -C /workspace/projekte/board log -1 --format='%an %s'` | `Thomas Heinrich Fremde Vorhaben lesen` | 0 |

Ausgabe von `schritt07.mjs`:

```
ok fremdes Vorhaben: an „Prüfen“ im Workflow von p2, verantwortet von „Zweite Person“, Farbe #8b5cf6
ok Farbe: eigene bleibt (#60a5fa), ohne eigene gilt die Person (#fbbf24)
ok /board/dateien: transkripte, idee.md, vorhaben.json — nichts Erfundenes
ok /board/doc: idee.md, 42 Zeichen
ok 403: doc/schreiben und skill/lauf — idee.md bleibt byte-gleich, neu.md und ein Transkript entstehen nicht
ok eigenes Vorhaben bleibt schreibbar: notiz-schritt07.md
ok eigener Stand gespeichert, fremde Dateien byte-gleich
ok Seite: fremdes Vorhaben öffnen und Dateien lesen; Leser ohne Bearbeiten; Schreiben und Starten nur bei darf.aendern
ok echte Ablage unverändert: kein p2, kein von-p2
ok Prüfordner aufgeräumt: /tmp/board-schritt07-…
```

**Entscheidende Zeile:** `ok fremdes Vorhaben: an „Prüfen“ im Workflow von p2,
verantwortet von „Zweite Person“, Farbe #8b5cf6` — das fremde Vorhaben steht
lesbar auf dem Board. Die Zeile darunter `ok 403: …` belegt, dass Schreiben
überall abgewiesen wird, bevor etwas entsteht.

## Entscheidungen

- **Farbe wird abgeleitet, nicht gespeichert.** `datenAntwort` setzt sie bei
  jedem Lesen; `vorgangDatei` schreibt sie nicht mit. Ein eigenes Vorhaben mit
  eigener Farbe behält sie; bei mehreren Verantwortlichen gilt die erste.
- **`aendern` ist die Prüfung für jedes Schreiben in den Ordner.** Ein eigenes
  Transkript und eine neue Datei sind keine Ausnahme; beide Routen fragen
  dieselbe Stelle wie die vier Handlungen.
- **Der Vermerk `schreibbar` entsteht beim Lesen.** Er hält fest, wer das
  Vorhaben vor der Änderung verantwortet hat. So bleibt ein Weitergeben
  schreibbar, ein fremdes Vorhaben aber unangetastet — auch wenn ein anderes
  Speichern den ganzen Stand schreibt.
- **Testvorhaben brauchen seit dieser Prüfung einen Verantwortlichen.** Die
  Prüfskripte 02 und 04 tragen `p1` an ihrer Station ein; das ist eine
  Fixture-Anpassung, keine Änderung ihres Prüfziels.
- **Der Leser fremder Dateien bleibt beim Lesen.** Der Verlauf ist erlaubt,
  Bearbeiten, Speichern und Abzweigen nicht. Das Bild im Browser ist damit
  nicht geprüft; der Quelltext und die Host-Antworten sind es.

## Fassung

Der Stand der Ablage ist als Fassung `Fremde Vorhaben lesen` festgehalten
(Urheber `Thomas Heinrich`, `thomas.heinrich@board`). Sie enthält diesen
Bericht, den Schritt 07 und die Arbeitsstände, die beim Schritt im Arbeitsbaum
lagen. Die Änderungen am Produkt (`produkt/index.js`, `produkt/ablage.js`,
`produkt/client.js`, `produkt/test/*`) sind wie in den Schritten davor nicht
Teil der Board-Fassung: `vorgaenge/*/produkt/` ist im Repository ignoriert.

## Grenzen und offene Punkte

- **Nicht neu gestartet.** Das laufende Board antwortet weiter mit dem alten
  Modul; Farbe, 403 und die neue Seite erscheinen erst nach einem Neustart.
- **Im Browser nicht gesehen.** Die Seite ist syntaktisch geprüft, und der
  Prüfskript belegt die Bindungen im Quelltext. Das tatsächliche Bild ist nicht
  geprüft, weil dafür ein Neustart nötig wäre.
- **Die zweite Person lebt nur in der Prüfkopie.** Die echte Ablage hat weiter
  nur `p1`, kein fremdes Vorhaben und keinen `p2`-Workflow.
- **Die Prüfung ist eine Prüfung im Board, keine Sperre im Repository.** Wer am
  Board vorbei schreibt, kann weiterhin alles ändern; die Fassungen zeigen, wer
  es war.
