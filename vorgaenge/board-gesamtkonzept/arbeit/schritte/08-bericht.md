# Bericht Schritt 08 — Der Skill eines Schritts liegt in der Ablage

- **Stand:** 2026-10-26
- **Gebaut:** Die fünf Anweisungen der Schritte liegen unter `skills/p1/`;
  Anzeige, Lesen und Ändern arbeiten auf diesen Dateien; beim Start eines Laufs
  wird die Anweisung als `<host>-<schritt>` in `$DSH_HOME/skills` eingesetzt und
  danach wieder weggenommen; die Werkstatt schreibt in die Ablage, und der Host
  hält den Stand als Fassung fest; Prüfskript `produkt/test/schritt08.mjs`; die
  Fassung `Schritt-Skills`.
- **Nicht neu gestartet:** Das laufende Board arbeitet weiter mit dem geladenen
  alten Modul. Alles Neue ist an einer Modulkopie in `/tmp` geprüft. Die echte
  Installation unter `/dsh-home/skills` blieb unverändert.

## Die fünf Anweisungen

Die einmalige Übernahme in `skills/p1/` (Schritt → heutige Anweisung):

| Datei | aus `$DSH_HOME/skills/` | Laufname |
| --- | --- | --- |
| `skills/p1/idee-vision.md` | `grillme-vision` | `p1-idee-vision` |
| `skills/p1/vision-fachkonzept.md` | `grillme-fachkonzept` | `p1-vision-fachkonzept` |
| `skills/p1/fachkonzept-architektur.md` | `architekturentscheidungen` | `p1-fachkonzept-architektur` |
| `skills/p1/architektur-umsetzung.md` | `bauplan-erstellen` | `p1-architektur-umsetzung` |
| `skills/p1/umsetzen.md` | `umsetzen` | `p1-umsetzen` |

Jede Datei ist der Text der heutigen Anweisung, wörtlich übernommen bis auf die
`name:`-Zeile der Kopfzeile: Sie trägt den Laufnamen `<host>-<schritt>`, weil
der Harness die Datei beim Einsatz unter genau diesem Namen findet. `description`
und `whenToUse` bleiben, wie sie waren. `$DSH_HOME/skills` wurde nicht
verschoben und nicht verändert; die fünf Originale liegen dort unverändert.

In `workflows/p1.json` tragen die fünf Schritte jetzt:

- `wie: { art: "skill", wert: "<host>-<schritt>" }` — der Name des Laufs;
- `skill: "skills/p1/<schritt>.md"` — der Pfad der Anweisung in der Ablage.

`workflows/p1.json` ist nach dem Laden und Schreiben byte-gleich: `liesAblage`
und `baueDateien` erzeugen denselben Text (geprüft, 7900 Bytes, `gleich: true`).

## Die Ablage ist die Quelle

- **Anzeigen** — `GET /board/skills` liest `leseSchrittSkills`
  (`produkt/index.js` 846): Es listet die Schritte des Workflows, deren Datei
  unter `skills/<host>/<schritt>.md` liegt, mit `name` (Laufname), `schritt`,
  `titel`, `beschreibung`, `whenToUse` und `datei`. Die Ordner unter
  `$DSH_HOME/skills` werden dafür nicht mehr gelesen.
- **Lesen** — `GET /board/skill/dateien?name=<laufname>` sucht den Eintrag in
  der Ablage und liest die Datei von dort (1786).
- **Eintragen** — `POST /board/skill/eintragen` nimmt einen Laufnamen, findet
  die Anweisung in der Ablage und setzt `wie` und `skill` der Station (1973).
  Ein fremder Schritt antwortet 403.
- **Ändern** — Wird an einer Station `wie` gespeichert, setzt
  `/board/schritt/aendern` den Pfad in `skill` passend nach oder leert ihn
  (1895).
- `produkt/ablage.js` führt die beiden Felder auseinander: `schrittAnzeige`
  gibt `skill` als Pfad zurück, `schrittDatei` schreibt ihn so zurück (111,
  353). Ältere Stände, in denen beide Felder denselben Namen tragen, bleiben
  lesbar.
- `produkt/client.js` zeigt in der Skill-Auswahl den Schrittnamen und den
  Laufnamen (`Titel — p1-…`, 3656). Der Werkstatt-Auftrag nennt statt
  `/dsh-home/skills/...` jetzt `/workspace/projekte/board/skills/<host>/<schritt>.md`
  und den Laufnamen in der Kopfzeile (2422–2454).

## Einsatz beim Start

`POST /board/gespraech/senden` setzt beim ersten Senden eines Schritts die
Anweisung ein:

1. Die Station wird aus `mitAbleitung` geholt; ist `wie.art` `skill`, muss die
   Datei in der Ablage liegen, sonst antwortet der Host 409.
2. `setzeSchrittSkillEin` schreibt `$DSH_HOME/skills/<host>-<schritt>/SKILL.md`
   (915). Der Text kommt aus der Ablage; die `name:`-Zeile wird auf den
   Laufnamen gesetzt.
3. Der Startauftrag nennt den Laufnamen; fehlt er im Text, stellt der Host
   „Führe den Skill „<laufname>“ aus.“ voran (2637). Die Sitzung beginnt damit
   unter diesem Namen.
4. Nach `create` — auch wenn es scheitert — nimmt `nehmeSchrittSkillWeg` den
   Ordner wieder weg (`finally`, 934). `$DSH_HOME/skills` selbst bleibt.

Das Transkript entsteht unverändert: `/board/skill/lauf` schreibt es mit dem
Laufnamen in der Kopfzeile unter `vorgaenge/<ordner>/transkripte/` und trägt den
Lauf am Vorhaben ein; `/board/verlauf` zeigt es am Schritt.

## Werkstatt

Der Auftrag der Werkstatt schreibt die Anweisung nicht mehr in den Harness,
sondern nach `skills/<host>/<schritt>.md` in der Ablage. Nach dem Senden hält
`/board/werkstatt/senden` den geänderten Stand als Fassung fest
(`festhalten(..., autorModell())`, 2832); der Urheber ist „Modell (<Person>)“.
Die Anzeige folgt der geänderten Datei, weil `/board/skills` sie bei jedem
Aufruf neu liest. Für eine Station ohne Anweisung erscheint die neue Datei als
neuer Eintrag; „eintragen“ setzt sie an die Station.

## Prüfskript (`produkt/test/schritt08.mjs`)

Das Skript kopiert die Ablage und das Produkt nach `/tmp`, legt ein eigenes
`$DSH_HOME` an und **kopiert** den Harness-Skillordner dorthin. Der
Sitzungsdienst ist ersetzt: `create` liest während des Starts die eingesetzte
`SKILL.md` und notiert ihren Namen; auf Wunsch scheitert es. `prompt` schreibt —
wie das Modell in der Werkstatt — die geänderte Anweisung in die Ablage.

Geprüft werden neun Punkte:

1. `skills/p1/` trägt fünf Anweisungen, jede mit `name`
   (`<host>-<schritt>`), `description` und `whenToUse`;
2. `/board/skills` listet genau diese fünf Schritte aus der Ablage, keinen
   installierten Fremdskill;
3. `/board/skill/dateien` liest die Anweisung aus der Ablage (`vision.md`);
4. `/board/data` zeigt an den fünf Schritten `wie.wert` und `skill` (Pfad);
5. ein Lauf an `idee-vision` (Position `vision`) legt während des Starts
   `$DSH_HOME/skills/p1-idee-vision/SKILL.md` an; der Ersatz notiert den Namen;
   nach dem Aufruf ist der Ordner weg;
6. das Transkript entsteht unter `transkripte/` und erscheint an der Station;
7. ein gescheiterter Start hinterlässt keinen Ordner und kein Gespräch;
8. die Werkstatt-Route schreibt `skills/p1/idee-vision.md` und erzeugt eine
   Fassung mit Urheber „Modell (Thomas Heinrich)“; danach ist der Arbeitsbaum
   ruhig;
9. `grillme-vision/SKILL.md` bleibt in der Kopie byte-gleich; die echte Ablage
   trägt die fünf Dateien, der echte Harness-Ordner kein `p1-*`.

Ausgabe:

```
ok skills/p1: 5 Anweisungen, je Name, description, whenToUse
ok /board/skills: idee-vision, vision-fachkonzept, fachkonzept-architektur, architektur-umsetzung, umsetzen
ok /board/skill/dateien liest die Anweisung aus der Ablage
ok /board/data: fünf Schritte mit Laufname und Ablagepfad
ok Lauf: während des Starts p1-idee-vision/SKILL.md, danach weg
ok Transkript: transkripte/vision-2026-10-26-181439.md unter transkripte/, am Schritt sichtbar
ok gescheiterter Start: Ordner wieder weg, kein Gespräch eingetragen
ok Werkstatt: skills/p1/idee-vision.md geändert, Fassung „Modell (Thomas Heinrich)“
ok Harness-Skills: Kopie unverändert, echter Ordner ohne p1-*
ok Prüfordner aufgeräumt: /tmp/board-schritt08-…
```

**Entscheidende Zeile:** `ok Lauf: während des Starts p1-idee-vision/SKILL.md,
danach weg` — die Anweisung kommt aus der Ablage, liegt nur für den Start im
Harness und verschwindet wieder.

## Dateien

- `skills/p1/idee-vision.md`, `vision-fachkonzept.md`,
  `fachkonzept-architektur.md`, `architektur-umsetzung.md`, `umsetzen.md` — neu,
  die fünf Anweisungen.
- `workflows/p1.json` — `wie.wert` und `skill` der fünf Schritte.
- `produkt/index.js` — Helfer `schrittSkillPfad` (825), `laufNameVon` (835),
  `leseSchrittSkills` (846), `schrittAnweisung` (880), `mitSkillNamen` (895),
  `setzeSchrittSkillEin` (915), `nehmeSchrittSkillWeg` (934); Routen
  `/board/skills` (1958), `/board/skill/dateien` (1786),
  `/board/skill/eintragen` (1973); `/board/schritt/aendern` (1895);
  `/board/gespraech/senden` (2591, Einsatz 2618–2644); `/board/werkstatt/senden`
  (2832).
- `produkt/ablage.js` — `schrittAnzeige` (111), `schrittDatei` (353).
- `produkt/client.js` — Werkstatt-Auftrag (2411–2468), Optionsbeschriftung
  (3656).
- `produkt/test/schritt08.mjs` — neu.
- `arbeit/schritte/08-bericht.md` — dieser Bericht.

## Prüfungen

| Nr | Befehl | Ergebnis | Exit |
| --- | --- | --- | --- |
| 1 | `node produkt/test/schritt08.mjs` | neun ok-Zeilen: Ablage, Anzeige, Einsatz, Transkript, gescheiterter Start, Werkstatt, Harness-Kopie | 0 |
| 2 | `node produkt/test/schritt07.mjs` | fremdes Vorhaben, 403, Seite unverändert | 0 |
| 3 | `node produkt/test/schritt06.mjs` | Verantwortliche und Weitergeben unverändert | 0 |
| 4 | `node produkt/test/schritt05.mjs` | fremde Fläche und Verweis unverändert | 0 |
| 5 | `node produkt/test/schritt04.mjs` | Abgleich, Speichern, kein Remote unverändert | 0 |
| 6 | `node produkt/test/schritt03.mjs` | 5 Vorhaben, 11 Positionen, Fassung nennt Thomas Heinrich | 0 |
| 7 | `node produkt/test/schritt02.mjs` | Ablage, Urheber und Lauf ohne Konfiguration unverändert | 0 |
| 8 | `node --check produkt/index.js`, `ablage.js`, `client.js`, `test/schritt08.mjs` | keine Syntaxfehler | 0 |
| 9 | `ls /workspace/projekte/board/skills/p1` | 5 Dateien | 0 |
| 10 | `ls /dsh-home/skills` | 7 Ordner wie vorher, kein `p1-*` | 0 |
| 11 | `git -C /workspace/projekte/board status --porcelain` | leer | 0 |
| 12 | `git -C /workspace/projekte/board log -1 --format='%an %s'` | `Thomas Heinrich Schritt-Skills` | 0 |

## Entscheidungen

- **Der Laufname steht in `wie.wert`, der Pfad in `skill`.** Die bestehende
  Anzeige und der Startauftrag lesen `wie.wert`; die Architektur verlangt in
  `skill` den Pfad zur Datei. Beides getrennt zu führen, lässt Auswahl und Start
  an derselben Stelle und macht die Ablage zur Quelle. Ältere Stände mit
  demselben Wert in beiden Feldern bleiben lesbar.
- **Die Kopie trägt den Laufnamen in der Kopfzeile.** Der Text ist wörtlich
  übernommen; nur `name:` ist `<host>-<schritt>`. So ist die Datei in der Ablage
  schon die, die der Harness erwartet, und der Einsatz muss nichts erfinden.
- **Der Host setzt ein und nimmt weg, nicht die Sitzung.** Der Einsatz liegt
  nur während des Starts im Harness. Das ist die im Auftrag verlangte
  Reihenfolge; das Wegnehmen steht in `finally`, damit auch ein gescheiterter
  Start keinen Ordner hinterlässt.
- **Die Werkstatt schreibt in die Ablage.** Ihr Auftrag nennt den Pfad dort;
  der Host hält den Stand als Fassung mit dem Modell als Urheber fest.
- **Die Anzeige folgt der Ablage, nicht dem Harness.** `/board/skills` ist die
  einzige Liste; sie zeigt Schritte, keine installierten Ordner.

## Fassung

Der Stand der Ablage ist als Fassung `Schritt-Skills` festgehalten, Urheber
`Thomas Heinrich` (`thomas.heinrich@board`). Sie enthält die fünf Anweisungen,
die geänderten `workflows/p1.json`, den Schritt 08 und diesen Bericht. Die
Änderungen am Produkt (`produkt/index.js`, `produkt/ablage.js`,
`produkt/client.js`, `produkt/test/schritt08.mjs`) sind wie in den Schritten
davor nicht Teil der Board-Fassung: `vorgaenge/*/produkt/` ist im Repository
ignoriert.

## Grenzen und offene Punkte

- **Nicht neu gestartet.** Das laufende Board antwortet weiter mit dem alten
  Modul. `/board/skills` in der neuen Form, die neue Auswahl und der Einsatz
  erscheinen erst nach einem Neustart.
- **Im Browser nicht gesehen.** Die Seite ist syntaktisch geprüft; das
  tatsächliche Bild ist nicht geprüft, weil dafür ein Neustart nötig wäre.
- **Keine echte Sitzung.** Der Sitzungsdienst war im Prüfskript ersetzt. Ob
  eine echte Sitzung die Anweisung nach dem Wegnehmen noch laden kann, ist
  damit nicht geprüft; nach dem Wortlaut des Auftrags liegt sie nur während
  des Starts. Wenn der Harness die Anweisung erst beim Skill-Aufruf liest,
  müsste das Wegnehmen später erfolgen — das ist eine Frage an den nächsten
  Schritt, nicht an diesen.
- **Die Werkstatt-Fassung entsteht nach der Rückkehr von `prompt`,** also nach
  der Annahme des Auftrags, nicht nach dem Ende des Modellzugs. Im Prüfskript
  schreibt der Ersatz synchron, deshalb greift sie dort. Schreibt das Modell
  später, hält der vorhandene Zeitgeber (eine Minute) den Stand als Fassung
  mit dem Modell als Urheber fest — dieselbe Wirkung, ein anderer Auslöser.
- **Parallele Läufe am selben Schritt** teilen sich den Einsatzordner. Der
  Auftrag verlangt keinen Gleichlauf; zwei gleichzeitige Starts würden den
  Ordner des einen wegnehmen. Das ist nicht gebaut und nicht geprüft.
- **Die Prüfung ist eine Prüfung im Board, keine Sperre im Repository.** Wer
  am Board vorbei schreibt, kann die Dateien weiterhin ändern; die Fassungen
  zeigen, wer es war.
