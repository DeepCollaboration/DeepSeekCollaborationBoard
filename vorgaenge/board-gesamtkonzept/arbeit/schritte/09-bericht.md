# Bericht Schritt 09 — Mein Workflow enthält einen fremden Schritt

- **Stand:** 2026-10-26
- **Gebaut:** Ein fremder Schritt lässt sich aus seiner Fläche heraus als
  Verweis in den eigenen Workflow stellen. In `workflows/p1.json` steht danach
  ein Eintrag `{ ref, id }` samt Position; die Datei der anderen Person wird
  gelesen, nicht geschrieben, und der Schritt nicht kopiert. Der Verweis lässt
  sich wieder herausnehmen; der fremde Schritt selbst antwortet auf Ändern und
  Löschen mit 403. Ein Vorhaben kann auf der Verweis-Position stehen und bleibt
  bei der eigenen Person. Prüfskript `produkt/test/schritt09.mjs`; die Fassung
  `Fremder Schritt`.
- **Nicht neu gestartet:** Das laufende Board arbeitet weiter mit dem geladenen
  alten Modul. Alles Neue ist an einer Modulkopie in `/tmp` geprüft. Die echte
  Ablage hat weiterhin nur `p1` und keinen Verweis.

## Das Anstellen

`POST /board/fremd/anstellen` nimmt `{ ref, x?, y? }`. Der Verweis hat die Form
`<person>/<workflow>/<schritt>`. Der Host prüft:

1. genau drei nichtleere Teile;
2. die Person ist nicht die eigene;
3. Workflow und Schritt stehen in der Datei dieser Person;
4. das Ziel ist selbst kein Verweis;
5. der Host des Ziels ist nicht die eigene Person;
6. derselbe Verweis steht noch nicht im eigenen Workflow.

Dann schreibt er in die **eigene** Datei:

```json
{ "ref": "p2/standard/pruefen", "id": "pruefen" }
```

und legt eine Position mit dieser Kennung an. `x` und `y` sind freiwillig; ohne
sie steht die neue Position rechts neben der breitesten vorhandenen. Die
fremde Datei wird nur über `liesAblage` gelesen. Danach liest der Host den
Stand neu ein und antwortet mit derselben Form wie `/board/data`.

Beispiel: Ausgangsdatei mit 11 Schritten und 11 Positionen, nach dem Anstellen
12 Schritte und 12 Positionen; `workflows/p2.json` ist byte-gleich.

**Warum die Kennung mit in die Datei geht.** Die Position zeigt auf die
Kennung des Verweises. Würde sie nur beim Lesen entstehen, verlöre die Position
ihren Schritt, sobald das Ziel aus der Ablage verschwindet. Mit `id` in der
Datei bleibt die Position stehen, und der Verweis erscheint als fehlend. Die
Datei bleibt ein Verweis; `name`, `eingang`, `ausgang` und `wie` des Ziels
werden nicht hineingeschrieben.

## Das Herausnehmen

`POST /board/fremd/entfernen` nimmt `{ ref }`. Der Host sucht den Verweis in
der eigenen Fläche, entfernt seine Positionen und die Kanten daran und nimmt
den Verweis aus der Schrittliste. Stehen noch Vorhaben an einer dieser
Positionen, antwortet er mit 409 und nennt sie — sonst verlöre ein Vorhaben
seinen Ort. Die fremde Datei wird nicht angefasst.

Zusätzlich räumt `POST /board/position/loeschen` einen Verweis weg, wenn seine
letzte Position aus dem Workflow genommen wird. `POST /board/schritt/loeschen`
prüft jetzt vor allem anderen `fremd` und antwortet mit 403; `POST
/board/schritt/aendern` tat das schon. Damit ist der fremde Schritt selbst von
hier aus weder zu ändern noch zu löschen.

## Die Seite

- **Fremde Fläche:** Jede Karte eines fremden Schritts trägt den Knopf „In
  meinen Workflow stellen“. Steht der Verweis schon in der eigenen Fläche,
  heißt der Knopf „steht in deinem Workflow“ und ist nicht mehr drückbar. Ein
  fehlendes Ziel und ein Verweis in der fremden Datei bieten den Knopf nicht an.
- **Eigene Fläche:** Der Verweis steht als Karte mit dem Namen seines Ziels da,
  darunter „bei ‹Person› · Verweis“ samt Eingang und Ausgang. Geöffnet zeigt er
  oben „bei ‹Person›“, darunter dieselben Werte; „Bearbeiten“ und das Formular
  fehlen, weil der Schritt seinem Host gehört.
- **Herausnehmen:** Die geöffnete Verweis-Karte trägt unten „Verweis
  herausnehmen“ mit einer Rückfrage. Der fremde Schritt bleibt dabei stehen.
- **Fehlendes Ziel:** Die Karte heißt „Verweis fehlt“; die Position bleibt an
  ihrem Platz. Nach dem Zurücklegen der fremden Datei löst sie sich wieder auf.

## Prüfskript (`produkt/test/schritt09.mjs`)

Das Skript kopiert Ablage und Produkt nach `/tmp`, legt ein eigenes
`$DSH_HOME` an und schreibt `p2` samt Workflow erst in die Kopie. Geprüft
werden neun Punkte:

1. die Kopie beginnt ohne `p2` und ohne Verweis in `workflows/p1.json`;
2. `/board/fremd/anstellen` schreibt genau einen `{ref, id}`-Eintrag und eine
   Position; `workflows/p2.json` ist byte-gleich;
3. `/board/data` löst den Verweis in der eigenen Fläche auf: Name „Prüfen“,
   `host: "p2"`, `fremd: true`, Eingang `idee.md`, Ausgang `pruefung.md`;
4. ein Vorhaben wird angelegt und auf die Verweis-Position verschoben; dort
   erscheint es mit `verantwortet: "Thomas Heinrich"` und darf verschoben
   werden;
5. `/board/schritt/aendern` und `/board/schritt/loeschen` antworten mit 403;
   beide Workflow-Dateien bleiben byte-gleich;
6. verschwindet `workflows/p2.json`, erscheint der Verweis als `fehlt` mit
   leerem Namen, sein Host bleibt `p2`, die Position bleibt stehen; danach ist
   die p2-Datei wieder byte-gleich;
7. Herausnehmen mit einem Vorhaben darauf antwortet 409;
8. nach dem Wegstellen des Vorhabens nimmt `/board/fremd/entfernen` den Verweis
   heraus: `workflows/p1.json` ist wieder byte-gleich der Ausgangsstand,
   `workflows/p2.json` unverändert;
9. die Seite ruft beide Routen auf, bietet das Anstellen und das Herausnehmen
   an und schreibt „bei ‹Person›“.

Ausgabe:

```
ok p1 ohne Verweis, p2 in der Kopie angelegt
ok angestellt: p1 trägt {ref, id} und eine Position, p2 byte-gleich
ok /board/data: Verweis als Prüfen (host p2, fremd) mit Ein- und Ausgang
ok Vorhaben auf der Verweis-Position: verantwortet Thomas Heinrich
ok Ändern und Löschen am fremden Schritt: 403, beide Dateien unverändert
ok Ziel weg: Verweis erscheint als fehlend, Position bleibt
ok Herausnehmen mit Vorhaben: 409; danach weg: p1 wieder der Ausgangsstand, p2 byte-gleich
ok Seite: anstellen, herausnehmen, „bei ‹Person›“
ok Prüfordner aufgeräumt: /tmp/board-schritt09-…
```

**Entscheidende Zeile:** `ok angestellt: p1 trägt {ref, id} und eine Position,
p2 byte-gleich` — der fremde Schritt steht im eigenen Workflow, ohne dass
seine Datei oder sein Inhalt angefasst wird.

## Dateien

- `produkt/index.js` — `zielVonRef` (902), `refKennung` (924); Routen
  `/board/fremd/anstellen` (1799) und `/board/fremd/entfernen` (1865); 403 in
  `/board/schritt/loeschen` (2085); Aufräumen des Verweises in
  `/board/position/loeschen` (2435).
- `produkt/ablage.js` — ein Verweis wird beim Schreiben mit seiner Kennung
  abgelegt (`workflowDatei`, 383–389).
- `produkt/client.js` — Zustand `bestaetigenVerweis` (1371), `fremdAnstellen`
  (2289), `verweisEntfernen` (2301), „bei ‹Person›“ in der Anzeige (3595), am
  Kartenkörper (4140–4156), Herausnehmen am Verweis (4225), Anstellen-Knopf in
  der fremden Fläche (4655).
- `produkt/test/schritt09.mjs` — neu, 375 Zeilen.
- `arbeit/schritte/09-bericht.md` — dieser Bericht.

## Prüfungen

| Nr | Befehl | Ergebnis | Exit |
| --- | --- | --- | --- |
| 1 | `node …/produkt/test/schritt09.mjs` | neun ok-Zeilen: anstellen, auflösen, Vorhaben, 403, fehlendes Ziel, herausnehmen | 0 |
| 2 | `node …/produkt/test/schritt08.mjs` | Skills, Lauf, Werkstatt unverändert | 0 |
| 3 | `node …/produkt/test/schritt07.mjs` | fremdes Vorhaben lesen, 403 unverändert | 0 |
| 4 | `node …/produkt/test/schritt05.mjs` | fremde Flächen und Verweis unverändert | 0 |
| 5 | `node …/produkt/test/schritt03.mjs` | 5 Vorhaben, 11 Positionen, 11 Schritte | 0 |
| 6 | `node …/produkt/test/schritt02.mjs`, `04`, `06` | Ablage, Abgleich, Verantwortliche unverändert | 0 |
| 7 | `python3 -c "…"` | `workflows/p1.json` ohne `ref`, Personen nur `p1` | 0 |
| 8 | `node --check produkt/index.js`, `ablage.js`, `client.js`, `test/schritt09.mjs` | keine Syntaxfehler | 0 |
| 9 | `git -C /workspace/projekte/board status --porcelain` | leer | 0 |
| 10 | `git -C /workspace/projekte/board log -1 --format='%an %s'` | `Thomas Heinrich Fremder Schritt` | 0 |

## Entscheidungen

- **Der Verweis trägt seine Kennung in der eigenen Datei.** Sie ist der Anker
  der Position. Verschwindet das Ziel, bleibt die Stelle sichtbar und zeigt
  „Verweis fehlt“, statt einen Schritt zu raten. Geschrieben wird weiter nur
  `ref` und `id`, nie der Inhalt des fremden Schritts.
- **Anstellen und Herausnehmen sind eigene Routen.** Sie ändern den eigenen
  Workflow, nicht den fremden Schritt. `/board/schritt/loeschen` bleibt für
  eigene Schritte; ein Verweis wird nicht über ein Löschen des Schritts
  beseitigt, sondern über seinen eigenen Weg.
- **Herausnehmen mit Vorhaben wird abgelehnt.** Ein stilles Verschieben oder
  Verlieren des Vorhabens wäre schlimmer als eine Meldung. Der Host nennt die
  betroffenen Vorhaben; der Mensch stellt sie vorher weg.
- **Nur der erste eigene Workflow.** Die eigene Fläche ist der erste Workflow
  der eigenen Person; dorthin kommt der Verweis. Weitere Workflows der eigenen
  Person bleiben, wie sie sind.

## Fassung

Der Stand der Ablage ist als Fassung `Fremder Schritt` festgehalten, Urheber
`Thomas Heinrich` (`thomas.heinrich@board`). Sie enthält den Schritt 09, diesen
Bericht und die Arbeitsstände in `arbeit/`, die beim Schritt im Arbeitsbaum
lagen. Die Änderungen am Produkt (`produkt/index.js`, `produkt/ablage.js`,
`produkt/client.js`, `produkt/test/schritt09.mjs`) sind wie in den Schritten
davor nicht Teil der Board-Fassung: `vorgaenge/*/produkt/` ist im Repository
ignoriert.

## Grenzen und offene Punkte

- **Nicht neu gestartet.** Das laufende Board antwortet weiter mit dem alten
  Modul. Der Anstellen-Knopf, der Verweis auf der eigenen Fläche und das
  Herausnehmen erscheinen erst nach einem Neustart.
- **Im Browser nicht gesehen.** Die Seite ist syntaktisch geprüft und ihre
  Aufrufe sind im Prüfskript belegt; das tatsächliche Bild ist nicht geprüft,
  weil dafür ein Neustart nötig wäre.
- **Kein Lauf des fremden Schritts.** Der Schritt lässt sich anstellen und ein
  Vorhaben darauf setzen; ob sein Skill in einer Sitzung läuft, gehört zum
  Schritt selbst und ist hier nicht geprüft.
- **Alte Verweise ohne Kennung.** Ein von Hand geschriebener Verweis ohne `id`
  bleibt lesbar; verschwindet sein Ziel, kann seine Position den Bezug
  verlieren. Neue Verweise tragen die Kennung, und jedes Speichern schreibt sie
  mit.
- **Gleichlauf.** Zwei gleichzeitige Anstellen-Aufrufe können denselben
  Verweis doppelt prüfen; der letzte Schreibvorgang gewinnt. Für zwei Menschen,
  die selten gleichzeitig klicken, ist das nicht gebaut.
- **Keine technische Sperre im Repository.** Wer am Board vorbei schreibt,
  kann die Dateien weiterhin ändern; die Fassungen zeigen, wer es war.
