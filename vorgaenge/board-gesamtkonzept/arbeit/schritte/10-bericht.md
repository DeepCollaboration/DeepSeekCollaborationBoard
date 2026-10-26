# Bericht Schritt 10 — Der Skill der anderen Person läuft in meinem Harness

- **Stand:** 2026-10-26
- **Gebaut:** Ein Vorhaben, das auf der Verweis-Position eines fremden Schritts
  steht, lässt sich dort starten. Der Host liest die Anweisung aus
  `skills/<host>/<schritt>.md`, setzt sie als `<host>-<schritt>` in
  `$DSH_HOME/skills` ein, startet die Sitzung im eigenen Arbeitsbereich und nimmt
  den Einsatz danach wieder weg. Das Transkript entsteht wie bei einem eigenen
  Schritt unter `vorgaenge/<eigenes>/transkripte/`, erscheint in `laeufe` und am
  Schritt; die Fassung trägt den eigenen Urheber. Die fremde Workflow- und
  Skill-Datei werden nur gelesen. Ein Start auf einem fremden Vorhaben antwortet
  jetzt 403. Prüfskript `produkt/test/schritt10.mjs`; die Fassung
  `Fremder Skill-Lauf`.
- **Nicht neu gestartet:** Das laufende Board arbeitet weiter mit dem geladenen
  alten Modul. Alles Neue ist an einer Modulkopie in `/tmp` geprüft. Die echte
  Ablage bleibt ohne zweite Person und ohne fremden Schritt; `/dsh-home/skills`
  der echten Installation ist unverändert.

## Der Lauf an einem Verweis-Schritt

Der Verweis aus Schritt 09 löst beim Lesen zu einem Schritt mit `ref`, `host`,
`wie` und `skill` auf. Für den Lauf zählt das Ziel, nicht die Stelle in der
eigenen Fläche:

1. `schrittKennung` (`produkt/index.js` 828) nimmt bei einem Verweis Host und
   Schritt aus dem Verweis — nicht die lokale Kennung, die bei einem
   Namensstreit `ref:<person>/<workflow>/<schritt>` lautet.
2. `schrittSkillPfad` (848) liest die Anweisung aus `skills/<host>/<schritt>.md`;
   ein gültiger Pfad im Feld `skill` des Ziels gilt weiter.
3. `laufNameVon` (856) bildet `<host>-<schritt>`, hier `p2-pruefen`.
4. `/board/gespraech/senden` (2787) prüft zuerst, ob die eigene Person das
   Vorhaben verantwortet (403), nimmt dann die Anweisung aus der Ablage,
   `setzeSchrittSkillEin` (970) schreibt sie für die Dauer des Starts nach
   `$DSH_HOME/skills/p2-pruefen/SKILL.md` mit `name: p2-pruefen`, und
   `nehmeSchrittSkillWeg` (989) räumt sie im `finally` wieder weg — auch bei
   einem gescheiterten Start. Die Sitzung entsteht über `arbeitsplatz(ctx)` im
   eigenen Arbeitsbereich.
5. Der Startauftrag nennt den Laufnamen, auch wenn der Aufrufer ihn nicht
   mitschickt. Die Anweisung selbst steht im eingesetzten Skill; der Chat ist
   nicht ihre Quelle.
6. `/board/skill/lauf` (2198) schreibt das Transkript unter
   `vorgaenge/<eigenes>/transkripte/` und trägt es in `laeufe` ein. Der
   Dateiname wird über `transkriptStamm` (1019) flach gehalten: die Kennung
   `ref:<person>/<workflow>/<schritt>` enthält Doppelpunkt und Schrägstriche und
   taugt nicht als Dateiname. Die Fassung entsteht mit dem eigenen Urheber, hier
   `Modell (Thomas Heinrich)`.

**Die Seite** bietet den Start am Verweis-Schritt an, sobald das ausgewählte
Vorhaben dort steht und die eigene Person es verantwortet; `fremd` schließt den
Start nicht aus (`produkt/client.js` 3336). Das Bild im Browser ist nicht
geprüft, weil dafür ein Neustart nötig wäre.

## Prüfskript (`produkt/test/schritt10.mjs`)

Das Skript kopiert Ablage und Produkt nach `/tmp`, legt ein eigenes `$DSH_HOME`
an und kopiert den Harness-Skillordner dorthin. `p2`, ihr Workflow und
`skills/p2/pruefen.md` entstehen nur in der Kopie und werden dort als
„Prüfung: p2 in der Kopie“ festgehalten. Der Sitzungsdienst ist ersetzt:
`create` liest während des Starts die eingesetzte Anweisung (`p2-*`) und
notiert Namen, Text und Arbeitsort; `prompt` notiert den Startauftrag. Eine
echte Sitzung und eine echte Modellanfrage gibt es nicht.

Geprüft werden acht Punkte:

1. `p2` mit Person, Workflow und Anweisung lebt nur in der Kopie; die echte
   Ablage hat keinen Verweis, der echte Harness-Skillordner kein `p1-*`/`p2-*`;
2. der Verweis `p2/standard/pruefen` steht auf der eigenen Fläche, und das
   eigene Vorhaben lässt sich dort hinstellen (`verantwortet: "Thomas Heinrich"`,
   `darf.aendern: true`);
3. die Seite bietet den Start am Verweis-Schritt an — geprüft am Quelltext:
   Stelle des Vorhabens, `darf.aendern`, `wie.art: "skill"`, kein Ausschluss
   über `p.fremd`;
4. der Start setzt während des Aufrufs `$DSH_HOME/skills/p2-pruefen/SKILL.md`
   ein: Ordner, `name: p2-pruefen`, Text byte-gleich `skills/p2/pruefen.md`;
   der Arbeitsort ist die eigene Ablage-Kopie; der Startauftrag nennt
   `p2-pruefen`; nach dem Aufruf ist der Ordner weg;
5. das Transkript liegt flach unter `vorgaenge/<eigenes>/transkripte/`, steht in
   `laeufe` und am Schritt; die Fassung nennt „Thomas Heinrich“ und heißt
   „Skill p2-pruefen an der Station „Prüfen““; der Arbeitsbaum ist danach ruhig;
6. `workflows/p2.json` und `skills/p2/pruefen.md` sind byte-gleich, ein fremdes
   Vorhaben ist nicht entstanden, der Verweis steht weiter in der eigenen Datei;
7. ein Start auf einem fremden Vorhaben an der Verweis-Position antwortet 403:
   keine Sitzung, kein Einsatz, die fremde `vorhaben.json` bleibt byte-gleich;
8. `/dsh-home/skills` ist unverändert (gleiche Einträge, kein `p2-*`), die Kopie
   von `grillme-vision/SKILL.md` byte-gleich, die echte Ablage ohne zweite
   Person.

Ausgabe:

```
ok p2 lebt nur in der Kopie: Person, Workflow und skills/p2/pruefen.md
ok eigenes Vorhaben auf der Verweis-Position zu p2/standard/pruefen
ok Seite: Start am Verweis-Schritt, wenn das eigene Vorhaben dort steht
ok Start: während des Aufrufs p2-pruefen/SKILL.md aus der Ablage, danach weg
ok Transkript: transkripte/pruefen-2026-10-26-185701.md unter transkripte/, in laeufe, Fassung „Modell (Thomas Heinrich)“
ok fremde Dateien: workflows/p2.json und skills/p2/pruefen.md byte-gleich, kein fremdes Vorhaben
ok fremdes Vorhaben am fremden Schritt: 403, keine Sitzung, kein Einsatz
ok echte Installation: /dsh-home/skills unverändert, kein p2-* in der Ablage
ok Prüfordner aufgeräumt: /tmp/board-schritt10-…
```

**Entscheidende Zeile:** `ok Start: während des Aufrufs p2-pruefen/SKILL.md aus
der Ablage, danach weg` — die Anweisung der anderen Person kommt mit der Ablage,
liegt nur für den Start im eigenen Harness und ist danach wieder verschwunden.

## Dateien

- `produkt/index.js` — `schrittKennung` (828), `schrittSkillPfad` (848),
  `laufNameVon` (856), `transkriptStamm` (1019); Verwendungen in
  `/board/skill/lauf` (2239) und `/board/gespraech/senden` (2807).
- `produkt/client.js` — Start am Verweis-Schritt (3336).
- `produkt/test/schritt10.mjs` — neu, 545 Zeilen.
- `arbeit/schritte/10-bericht.md` — dieser Bericht.

## Prüfungen

| Nr | Befehl | Ergebnis | Exit |
| --- | --- | --- | --- |
| 1 | `node …/produkt/test/schritt10.mjs` | neun ok-Zeilen: Kopie, Vorhaben, Seite, Start, Transkript, fremde Dateien, 403, echte Installation | 0 |
| 2 | `node …/produkt/test/schritt09.mjs` | Verweis, Anstellen, Herausnehmen unverändert | 0 |
| 3 | `node …/produkt/test/schritt08.mjs` | Skills, Lauf, gescheiterter Start, Werkstatt unverändert | 0 |
| 4 | `node …/produkt/test/schritt07.mjs` | fremdes Vorhaben, 403, Seite unverändert | 0 |
| 5 | `node …/produkt/test/schritt03.mjs` | 5 Vorhaben, 11 Positionen, 11 Schritte | 0 |
| 6 | `node …/produkt/test/schritt02.mjs`, `04`, `05`, `06` | Ablage, Abgleich, Verweis, Verantwortliche unverändert | 0 |
| 7 | `node --check produkt/index.js`, `ablage.js`, `client.js`, `test/schritt10.mjs` | keine Syntaxfehler | 0 |
| 8 | `ls /dsh-home/skills` | sieben Ordner wie vorher, kein `p2-*` | 0 |
| 9 | `git -C /workspace/projekte/board status --porcelain` | leer | 0 |
| 10 | `git -C /workspace/projekte/board log -1 --format='%an %s'` | `Thomas Heinrich Fremder Skill-Lauf` | 0 |

## Entscheidungen

- **Der Laufname kommt aus dem Verweis, nicht aus der eigenen Fläche.** Bei
  einem Namensstreit heißt der Verweis lokal `ref:p2/standard/pruefen`; der
  Lauf heißt trotzdem `p2-pruefen`, und das Transkript bekommt einen flachen,
  sicheren Dateinamen. Der Host und der Schritt des Ziels sind die Wahrheit.
- **Ein fremdes Vorhaben beginnt kein Gespräch.** Bisher prüfte nur
  `/board/skill/lauf` die Verantwortung. Da jetzt die Anweisung einer anderen
  Person im eigenen Harness läuft, steht die Prüfung auch vor dem Start; sie
  antwortet 403, bevor eine Sitzung entsteht oder ein Einsatz geschrieben wird.
- **Die Fassung des Transkripts trägt den eigenen Urheber.** Geschrieben wird
  sie vom eigenen Host nach dem Lauf, nicht von der Person, deren Anweisung
  gelaufen ist: `Modell (Thomas Heinrich)`, nicht „Zweite Person“.
- **Die fremde Datei wird nicht kopiert.** Gelesen wird `skills/p2/pruefen.md`
  direkt aus der Ablage; die eingesetzte Kopie liegt nur in `$DSH_HOME` und nur
  für die Dauer des Starts. `workflows/p2.json` und die fremde Anweisung bleiben
  byte-gleich.
- **Der Start bleibt ohne Rückfrage.** Die Seite bietet ihn an, wenn das eigene
  Vorhaben dort steht und die eigene Person es verantwortet. Die Warnung, dass
  fremde Anweisungen laufen, steht im Bauplan als Stopp und im Bericht — ein
  zusätzlicher Dialog war nicht Teil des Auftrags.

## Fassung

Der Stand der Ablage ist als Fassung `Fremder Skill-Lauf` festgehalten, Urheber
`Thomas Heinrich` (`thomas.heinrich@board`). Sie enthält den Schritt 10, diesen
Bericht und die Arbeitsstände in `arbeit/`, die beim Schritt im Arbeitsbaum
lagen. Die Änderungen am Produkt (`produkt/index.js`, `produkt/client.js`,
`produkt/test/schritt10.mjs`) sind wie in den Schritten davor nicht Teil der
Board-Fassung: `vorgaenge/*/produkt/` ist im Repository ignoriert.

## Grenzen und offene Punkte

- **Nicht neu gestartet.** Das laufende Board antwortet weiter mit dem alten
  Modul. Der Start am Verweis-Schritt, der 403 und der neue Transkript-Name
  erscheinen erst nach einem Neustart.
- **Im Browser nicht gesehen.** Die Seite ist syntaktisch geprüft und ihre
  Bedingungen am Quelltext belegt; das tatsächliche Bild ist nicht geprüft, weil
  dafür ein Neustart nötig wäre.
- **Keine echte Sitzung, keine echte Modellanfrage.** Der Sitzungsdienst war im
  Prüfskript ersetzt. Ob der Harness die Anweisung nach dem Wegnehmen noch
  lädt, bleibt die offene Frage aus Schritt 08: nach dem Wortlaut des Auftrags
  liegt sie nur während des Starts. Wenn der Harness sie erst beim Skill-Aufruf
  liest, müsste das Wegnehmen später erfolgen.
- **Der Inhalt fremder Anweisungen ist Vertrauen.** Die Prüfung belegt den Weg —
  eingesetzt, gestartet, weggenommen —, nicht was die Anweisung im Lauf tut. Das
  ist der Stopp aus dem Bauplan und keine technische Sperre.
- **Parallele Starts am selben Verweis** teilen sich den Einsatzordner
  `$DSH_HOME/skills/p2-pruefen`. Zwei gleichzeitige Starts würden den Ordner des
  einen wegnehmen; das ist nicht gebaut und nicht geprüft.
- **Ein Start an einer Position, an der das Vorhaben nicht steht,** ist
  serverseitig weiter möglich, solange die eigene Person es verantwortet. Die
  Seite bietet ihn nicht an; der Auftrag verlangt die Prüfung nur für das
  fremde Vorhaben.
