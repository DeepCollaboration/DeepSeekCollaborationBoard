# Bericht Schritt 13 — Zweig und Fork tragen ihre Herkunft

- **Stand:** 2026-10-26
- **Gebaut:** Abzweigen schreibt am neuen Vorhaben `zweig_von = { von, station,
  datum }`; der Ursprung bleibt unverändert. Eine neue Route
  `POST /board/vorhaben/forken` legt zu jedem sichtbaren Vorhaben — auch einem
  fremden — eine eigene Kopie mit `fork_von`, übernommener Station und der
  eigenen Person als Verantwortliche an; die Läufe des Ursprungs kommen nicht
  mit. `/board/data` liefert die Herkunft samt Namen und Station des Ursprungs,
  die Seite zeigt „Zweig von ‹Name› ab ‹Station›“ und „Fork von ‹Name›“ und
  macht den Ursprung anklickbar. Prüfskript `produkt/test/schritt13.mjs`; die
  Fassung `Herkunft`.
- **Nicht neu gestartet:** Das laufende Board arbeitet weiter mit dem geladenen
  alten Modul. Die neue Route, die Anzeige und das geänderte Abzweigen
  erscheinen erst nach einem Neustart. Geprüft ist alles an einer Modulkopie in
  `/tmp`.

## Der Zweig

`POST /board/vorhaben/abzweigen` prüft weiter Zuständigkeit, Station und Weg.
Neu schreibt es die Herkunft nicht mehr als `abzweig`, sondern als
`zweig_von` in die Datei des neuen Vorhabens:

```json
"zweig_von": { "von": "eigenes-13", "station": "umsetzung", "datum": "2026-10-26" }
```

`von` ist die Kennung des Ursprungs, `station` die Station, von der abgezweigt
wurde, `datum` das Tagesdatum (`sv-SE`, also `JJJJ-MM-TT`). Der Ursprung bekommt
nichts: keine Datei, kein Feld, keine neue Station. Der Zweig beginnt an seiner
Zielstation und trägt dort die eigene Person als Verantwortlichen; die Dateien
bis zur Abzweigstation kopiert er wie bisher. Die Antwort auf das Abzweigen ist
jetzt eine vollständige Datenantwort (`datenAntwort`) mit `neu` und `mit` — so
zeigt die Seite die Herkunft des neuen Zweigs sofort, ohne neu zu laden.

## Der Fork

`POST /board/vorhaben/forken` nimmt `{ id, name? }` und antwortet mit der
Datenantwort, `neu` und `dateien` (Zahl der übernommenen Dateien).

- **Jedes sichtbare Vorhaben.** Die Route prüft keine Zuständigkeit: Ein Fork
  eines fremden Vorhabens ist erlaubt; gelesen wird dessen Ordner, geschrieben
  wird nur der neue. Der Name ist optional und wird sonst zu
  `‹Name des Ursprungs› — Fork`.
- **Eigener Ordner, eigene Dateien.** `vorgaenge/<neue kennung>/` entsteht und
  übernimmt den Inhalt des Ursprungs im augenblicklichen Stand. Ausgelassen
  werden `transkripte/` und die alte `vorhaben.json` — die neue schreibt
  `schreibeBoard` mit der Herkunft. `.git` kommt wie beim Zweig nicht mit; die
  Grenzen von `kopiereOrdner` (4000 Dateien, 200 MB) gelten auch hier.
- **Station und Verantwortung.** Der Fork steht an derselben Station wie der
  Ursprung, `verantwortliche` nennt dort die eigene Person, `regel` ist
  `einer`. Die Farbe des Ursprungs wird übernommen, wenn er eine hat.
- **Keine Läufe.** `laeufe` ist leer, `transkripte/` wird nicht kopiert. Ein
  Fork beginnt ohne die Gespräche des Ursprungs.
- **Der Ursprung bleibt unangetastet.** `fork_von` steht nur am neuen Vorhaben.
  Ein fremdes `vorhaben.json` trägt beim Lesen `schreibbar: false`; das
  Speichern des Standes fasst es nicht an. Der fremde Ordner wird nur gelesen.
- **Fehler.** Ein unbekanntes Vorhaben antwortet 404, ein falscher Aufruf 405.

## `/board/data`

Jedes Vorhaben trägt weiter seine rohen Felder `fork_von` und `zweig_von`.
Zusätzlich steht `herkunft` daneben, fertig für die Anzeige:

```json
"herkunft": {
  "zweig": { "von": "eigenes-13", "name": "Eigenes Vorhaben für die Herkunft",
             "station": "umsetzung", "stationName": "Architektur zu Bauplan",
             "datum": "2026-10-26", "sichtbar": true },
  "fork":  { "von": "von-p2", "name": "Fremdes Vorhaben von p2", "sichtbar": true }
}
```

`name` ist der Name des Ursprungs, `stationName` der Name seiner Station (aus
allen Workflows abgeleitet), `sichtbar` sagt, ob sein Ordner in dieser Ablage
liegt. Fehlt der Ursprung, bleibt seine Kennung als Name stehen; anklickbar ist
er dann nicht. Die Seite muss nichts nachschlagen.

## Die Seite

- Am Vorhaben (im Chip an seiner Station) steht unter „verantwortet von“:
  „Zweig von ‹Name› ab ‹Station›“ und/oder „Fork von ‹Name›“. Der Name ist ein
  Knopf und öffnet den Ursprung, wenn `sichtbar` wahr ist; sonst steht er als
  Text.
- Beim ausgewählten Vorhaben steht der Knopf **Forken** — unabhängig von
  `darf`, also auch an einem fremden. Er ruft die Route, wählt den neuen Fork
  aus und meldet, wie viele Dateien übernommen wurden. Während der Anfrage ist
  der Knopf gesperrt.
- Dieselbe Handlung steht an einem fremden Vorhaben, das nur auf der fremden
  Fläche sichtbar ist (`ihreVorhaben`), neben „Dateien ansehen“.
- `AKTIONEN` bleibt unverändert: Forken ist keine Zuständigkeit am Vorhaben,
  sondern ein Lesen des Ursprungs und ein Anlegen eines eigenen.

## Prüfskript (`produkt/test/schritt13.mjs`)

Das Skript kopiert Ablage und Produkt nach `/tmp`, setzt `person: "p1"` in der
Kopie, entfernt den Remote und legt nur in der Kopie an:

- ein eigenes Vorhaben `eigenes-13` an `umsetzung` mit `vision.md` und
  `bauplan.md`, verantwortet von `p1`;
- eine zweite Person `p2` und deren Vorhaben `von-p2` an `idee` mit `idee.md`,
  einem `laeufe`-Eintrag und `transkripte/`.

Der fremde Ordner und seine Dateien werden auf einen alten Zeitstempel gesetzt;
die zwei vorhandenen Zweige der echten Ablage werden vorher byte-weise
festgehalten. Danach prüft es:

1. die vorhandenen Zweige tragen in `/board/data` ihre Herkunft samt Namen
   „Board Gesamtkonzept“ und Station „Idee“; die Prüfvorhaben tragen keine;
2. Abzweigen: `zweig_von` mit Ursprung `eigenes-13`, Station `umsetzung` und
   gültigem Datum; der Zweig steht an `umsetzen`, verantwortet von `p1`, und
   trägt `vision.md` und `bauplan.md` byte-gleich; die `vorhaben.json` des
   Ursprungs ist byte-gleich, seine Station bleibt `umsetzung`, keiner seiner
   Zeitstempel ändert sich;
3. `/board/data` nennt am Zweig den Namen und die Station des Ursprungs;
4. Forken des fremden Vorhabens: eigener Ordner, `fork_von: "von-p2"`,
   `zweig_von: null`, Station `idee`, `verantwortliche.idee = ["p1"]`,
   `laeufe: []`, `idee.md` byte-gleich, kein `transkripte/`; der fremde Ordner
   ist byte-gleich und keiner seiner Zeitstempel ändert sich;
5. `/board/data` nennt am Fork den Namen „Fremdes Vorhaben von p2“;
6. die Seite enthält „Zweig von “, „Fork von “, `zweigHerkunft.sichtbar`,
   `forkHerkunft.sichtbar`, `herkunftOeffnen`, `function forken`, den Weg
   `/board/vorhaben/forken` und „Forken“ (Quelltext, kein Browserbild);
7. die echte Ablage bleibt ohne `p2`, ohne `von-p2`, ohne `eigenes-13`; die
   zwei vorhandenen Zweige sind byte-gleich.

Ausgabe des letzten Laufs:

```
ok vorhandene Zweige: Herkunft samt Namen und Station in /board/data
ok Zweig: zweig_von mit Ursprung, Station, Datum; Ursprung byte-gleich, Dateien kopiert
ok /board/data (Zweig): Name und Station des Ursprungs stehen am Vorhaben
ok Fork: eigenes Vorhaben mit fork_von, eigener Person, kopierten Dateien; Läufe bleiben beim Ursprung
ok /board/data (Fork): Name des fremden Ursprungs steht am Fork
ok Seite: „Zweig von“ und „Fork von“, Herkunft anklickbar, Forken an jedem Vorhaben
ok echte Ablage unberührt: kein Fork, keine zweite Person, die zwei Zweige unverändert
ok Prüfordner aufgeräumt: /tmp/board-schritt13-…
```

**Entscheidende Zeile:** `ok Zweig: zweig_von mit Ursprung, Station, Datum;
Ursprung byte-gleich, Dateien kopiert` — der Zweig sagt, woher er kommt, und
der Ursprung bleibt, wie er war. Dazu `ok Fork: eigenes Vorhaben mit fork_von,
eigener Person, kopierten Dateien; Läufe bleiben beim Ursprung` — auch ein
fremdes Vorhaben lässt sich forken, ohne dass an ihm etwas geschrieben wird.

## Dateien

- `produkt/index.js` — Routenkopf um `/board/vorhaben/forken` ergänzt (27);
  `kopiereOrdner` nimmt `auslassen` (750); `herkunftVon` und `herkunft` in
  `datenAntwort` (1421–1442, 1457); `abzweigen` schreibt `zweig_von` (2986) und
  antwortet mit `datenAntwort` (3024); die neue Route `/board/vorhaben/forken`
  (3031–3091).
- `produkt/client.js` — Stile `herkunftZeile`/`herkunftLink` (444); Zustand
  `forkLaeuft` (1352); Herkunft am Chip und `Forken` (3227–3254, 3359–3370);
  `herkunftOeffnen` und `forken` (3620–3649); Forken an Vorhaben der fremden
  Fläche (5000–5007).
- `produkt/test/schritt13.mjs` — neu, 533 Zeilen.
- `arbeit/schritte/13-bericht.md` — dieser Bericht.

## Prüfungen

| Nr | Befehl | Ergebnis | Exit |
| --- | --- | --- | --- |
| 1 | `node …/produkt/test/schritt13.mjs` | acht ok-Zeilen: vorhandene Zweige, Zweig, `/board/data` (Zweig), Fork, `/board/data` (Fork), Seite, echte Ablage, aufgeräumt | 0 |
| 2 | `node …/produkt/test/schritt12.mjs` | zehn ok-Zeilen, Entscheid unverändert | 0 |
| 3 | `node …/produkt/test/schritt07.mjs` | fremdes Vorhaben lesen unverändert | 0 |
| 4 | `node …/produkt/test/schritt03.mjs` | Ablage und Umzug unverändert | 0 |
| 5 | `node …/produkt/test/schritt02.mjs`, `04`–`11` | alle zehn grün | 0 |
| 6 | `node --check produkt/index.js`, `ablage.js`, `client.js`, `test/schritt13.mjs` | keine Syntaxfehler | 0 |
| 7 | `git -C /workspace/projekte/board status --porcelain` | leer nach der Fassung | 0 |
| 8 | `git -C /workspace/projekte/board log -1 --format='%an %s'` | `Thomas Heinrich Herkunft` | 0 |
| 9 | `ls /workspace/projekte/board/beitraege` | kein Ordner — echte Ablage ohne Beiträge | 2 |

## Entscheidungen

- **Die Herkunft bleibt getrennt von der Anzeige.** In der Datei steht die
  Kennung (`fork_von`) bzw. das Paar (`zweig_von`); die Namen und die Station
  kommen beim Lesen aus der Ablage dazu (`herkunft`). So bleibt die Datei die
  schmale Form aus Schritt 03, und die Seite muss nichts nachschlagen.
- **`sichtbar` entscheidet über den Klick.** Ein Ursprung, dessen Ordner fehlt,
  bleibt als Kennung lesbar, ist aber kein Knopf. Die Seite muss dafür nicht
  selbst in der Vorhabenliste suchen.
- **Forken ist keine Zuständigkeit.** Es steht nicht in `AKTIONEN` und wird an
  jedem sichtbaren Vorhaben angeboten; geschrieben wird nur der eigene Ordner.
  Das ist die Stelle, an der „auch an einem fremden“ ohne neue Rechteprüfung
  gilt.
- **Der Fork nimmt die Arbeit, nicht die Gespräche.** Kopiert werden die
  Dateien des Ordners; ausgelassen werden `transkripte/` und die alte
  `vorhaben.json`. `laeufe` beginnt leer. Damit ist der Fork ein neuer Anfang
  mit dem Stand der Arbeit.
- **Die Abzweig-Antwort ist jetzt eine Datenantwort.** Vorher gab sie das rohe
  Board zurück; nach dem Anlegen fehlten der Seite `darf`, `verantwortet` und
  die neue `herkunft`. Mit `datenAntwort` steht der Zweig sofort vollständig da.
- **`abzweig` wird nicht mehr geschrieben.** Die Datei trägt `zweig_von`;
  `liesVorgaenge` setzt für ältere Leser weiter den Alias `abzweig`. Die zwei
  vorhandenen Zweige tragen bereits `zweig_von` und bleiben unverändert.
- **Kein neuer Eintrag in `AKTIONEN`.** Forken ist keine Handlung *am*
  Vorhaben, sondern ein Lesen und ein Anlegen daneben; eine Zuständigkeitsprüfung
  würde genau den fremden Fork verhindern, den der Schritt verlangt.

## Fassung

Der Stand der Ablage ist als Fassung `Herkunft` festgehalten, Urheber
`Thomas Heinrich` (`thomas.heinrich@board`). Sie enthält den Schritt 13, diesen
Bericht und die Arbeitsstände in `arbeit/`, die beim Schritt im Arbeitsbaum
lagen. Die Änderungen am Produkt (`produkt/index.js`, `produkt/client.js`,
`produkt/test/schritt13.mjs`) sind wie in den Schritten davor nicht Teil der
Board-Fassung: `vorgaenge/*/produkt/` ist im Repository ignoriert.

## Grenzen und offene Punkte

- **Nicht neu gestartet.** Das laufende Board antwortet weiter mit dem alten
  Modul; Route, Anzeige und das neue `zweig_von` erscheinen erst nach einem
  Neustart.
- **Im Browser nicht gesehen.** Die Seite ist syntaktisch geprüft und ihre
  Bedingungen am Quelltext belegt; das tatsächliche Bild ist nicht geprüft, weil
  dafür ein Neustart nötig wäre.
- **Keine echte zweite Person, kein echter Fork.** `p2`, `von-p2`, der
  Prüfzweig und der Prüffork leben nur in der Kopie in `/tmp`. Die echte Ablage
  hat weiter fünf Vorhaben, eine Person und die zwei vorhandenen Zweige.
- **Der Zweig kopiert weiter die Dateien bis zur Abzweigstation.** Das ist die
  bisherige Regel des Abzweigens (Ausgaben der Stationen von Anfang bis hierher,
  keine Transkripte); der Schritt ändert daran nichts. Ein Vorhaben, das Dateien
  außerhalb dieser Ausgaben im Ordner hat, gibt sie nicht mit.
- **Der Fork kopiert den ganzen Ordner.** Auch `arbeit/`, `produkt/` und andere
  Dateien außerhalb der Stationsausgaben kommen mit — der Fork ist eine Kopie
  des Standes, kein Zweig. Die Grenze von 4000 Dateien und 200 MB gilt.
- **Ein Fork trägt den Namen des Ursprungs plus „— Fork“.** Umbenennen geht
  danach über die vorhandene Handlung „Bearbeiten“. Zwei Forks desselben
  Vorhabens tragen denselben Anzeigenamen, aber verschiedene Kennungen und
  Ordner.
- **`zweig_von.station` ist die Abzweigstation**, nicht zwingend die Station,
  an der der Ursprung gerade steht. In der üblichen Bedienung sind beide gleich;
  wird von einer früheren Datei aus abgezweigt, nennt die Herkunft den Punkt,
  an dem der neue Weg beginnt.
