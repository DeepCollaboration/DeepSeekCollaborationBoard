# Bericht Schritt 05 — Der Workflow der anderen Person steht auf meinem Board

- **Stand:** 2026-10-26
- **Gebaut:** `/board/data` liefert `workflows` als eine Fläche je Person, die
  eigene zuerst. Fremde Schritte tragen ihren Host und sind als `fremd`
  markiert; ein Verweis `{ ref: "<person>/<workflow>/<schritt>" }` wird beim
  Lesen aufgelöst, nicht kopiert. Die Seite zeigt die eigene Fläche mit ihrer
  Bedienung und darunter je eine fremde Fläche zum Lesen: Schritte anklickbar,
  Ein- und Ausgang sichtbar, keine Felder zum Ändern, Löschen, Anlegen oder
  Ziehen. Die zweite Person lebt nur in der Prüfkopie.
- **Nicht neu gestartet:** Das laufende Board arbeitet weiter mit dem geladenen
  alten Modul. Die neue Antwort und die fremden Flächen sind an Modulkopien in
  `/tmp` geprüft; die echte Ablage hat weiterhin nur `p1`.

## Was gebaut ist

### Die Ablage liest alle Personen und Workflows (`produkt/ablage.js`)

- `liesWorkflows(repo, person)` liest `personen/*.json` und `workflows/*.json`.
  Eine Workflow-Datei ohne Personendatei zählt mit ihrer Kennung aus dem
  Dateinamen; die Personendatei liefert Name und Farbe, wenn es sie gibt.
- Jeder Workflow wird mit seinen Schritten, Positionen, Kanten und seinem Layout
  zurückgegeben. Ein Schritt bekommt `host` (aus der Datei oder vom Host der
  Datei), `wie` und `skill`.
- **Fremd heißt nicht bearbeitbar:** Jeder Schritt trägt `fremd`. Wahr ist es,
  wenn sein Host nicht die eigene Person ist — bei den Schritten der zweiten
  Person also immer, bei den eigenen nie.
- **Verweise werden aufgelöst, nicht kopiert:**
  `{ ref: "p2/standard/pruefen" }` wird über die Dateien aller Personen
  nachgeschlagen und als Schritt mit Name, Host, Eingang und Ausgang gezeigt;
  das Feld `ref` bleibt am Eintrag stehen. Fehlt das Ziel, steht der Verweis mit
  `fehlt: true` und leerem Namen da — geraten wird nichts.
- **Schreiben bleibt ehrlich:** `workflowDatei` schreibt einen Verweis weiter
  als `{ ref }` und lässt fremde Schritte nicht in die eigene Datei wandern.
  Die Felder `name`, `abgeleitet` und `fehlt` an Positionen sind abgeleitet und
  werden nicht mitgespeichert.
- Die eigene Person steht in der Liste zuerst; `ich` steht nur bei ihr.

### Die Antwort (`produkt/index.js`)

`GET /board/data` liefert jetzt:

```json
{
  "workflows": [
    { "id": "p1", "name": "…", "farbe": "#…", "ich": true,
      "workflows": [ { "id": "standard", "titel": "…", "schritte": [], "positionen": [], "kanten": [], "layout": "…" } ] },
    { "id": "p2", "name": "…", "farbe": "#…", "ich": false, "workflows": [] }
  ],
  "vorgaenge": [], "personen": [], "einstellungen": {}, "stand": {}
}
```

- `workflow` und `schritte` der alten Antwort entfallen. `personen`,
  `vorgaenge`, `einstellungen` und `stand` bleiben unverändert.
- Die eigene Fläche trägt ihren ersten Workflow mit den abgeleiteten
  Positionen (Eingang, Ausgang, Vorgänger, Nachfolger) — die Seite braucht sie
  für Karten, Dateien und Übergaben. Fremde Flächen tragen ihre Schritte, wie
  sie in der Datei stehen.
- Jede Fläche hat Name und Farbe; fehlt eine Farbe, kommt der nächste Ton aus
  der Palette. Die Reihenfolge beginnt mit der eigenen Person.
- Die schreibenden Routen, die bisher das Board zurückgaben, antworten in
  derselben Form. `POST /board/schritt/aendern` lehnt einen fremden Schritt mit
  Status 403 ab: er gehört seinem Host.

### Die Seite (`produkt/client.js`)

- Alle Leser der alten Schlüssel (`data.workflow`, `data.schritte`) lesen jetzt
  die eigene Fläche aus `data.workflows`; für Antworten ohne `workflows` bleibt
  ein Rückfall auf die alte Form.
- Über der eigenen Fläche steht die eigene Person mit Farbe. Darunter folgt je
  eine fremde Fläche: Personenname, Farbe, der Hinweis „hier nur zu lesen“ und
  die Schritte ihres Workflows als Karten. Ein Klick auf eine Karte klappt Ein-
  und Ausgang auf; es gibt dort nur diesen Klick, keine Eingabefelder, keinen
  Lösch- oder Anlegeknopf und kein Ziehen.
- Ein fremder Schritt, der als Verweis im eigenen Workflow steht, erscheint auf
  der eigenen Fläche und in der Bibliothek mit „bei ‹Person› — nur lesen“.
  „Bearbeiten“, „Löschen“ und „Aus dem Workflow nehmen“ fehlen dort.
- Fehlt das Ziel eines Verweises, steht „Verweis fehlt“ an der Karte; die
  Bibliothek zeigt es ebenso.

### Prüfungen (`produkt/test/`)

- **`schritt05.mjs` (neu):** kopiert Ablage und Produkt nach `/tmp`, lädt das
  Modul mit der Kopie und prüft zuerst ohne `p2`, dann mit `p2` und einem
  Verweis im eigenen Workflow: zwei Flächen mit eigener zuerst, `ich` nur
  einmal, zwei fremde Schritte mit `host: "p2"` und `fremd: true`, den Verweis
  aufgelöst als „Prüfen“ mit Host p2, Eingang `idee.md` und Ausgang
  `pruefung.md`, die p1- und p2-Dateien vor und nach dem Lesen byte-gleich, die
  p1-Datei mit dem Verweis statt einer Kopie; eine Speicherung über
  `/board/position/platzieren` lässt den Verweis Verweis und p2 unverändert.
- **`schritt03.mjs` (angepasst):** erwartet die neue Form
  `{ workflows, vorgaenge, personen, einstellungen, stand }`, nimmt Positionen
  und Schritte aus `workflows[0].workflows[0]` und prüft zusätzlich, dass die
  eigene Person zuerst steht und `ich` genau einmal vorkommt.
- **`schritt04.mjs` (angepasst):** prüft zusätzlich, dass `/board/data`
  `workflows` liefert und die alten Schlüssel `workflow`/`schritte` nicht mehr
  trägt. Abgleich, Speichern und der Fall ohne Remote bleiben unverändert.

## Dateien

- `produkt/ablage.js` — `liesWorkflows`, `schritteAnzeigen`, Verweise,
  `fremd`, Schreiben von Verweisen.
- `produkt/index.js` — `datenAntwort`, neue Form von `/board/data` und der
  schreibenden Antworten, Schutz fremder Schritte, Host eines Verweises bleibt.
- `produkt/client.js` — eigene Fläche aus `workflows`, fremde Leseflächen,
  Nur-lesen an fremden Schritten.
- `produkt/test/schritt05.mjs` — neu.
- `produkt/test/schritt03.mjs`, `produkt/test/schritt04.mjs` — an die neue
  Antwort angepasst.
- `arbeit/schritte/05-bericht.md` — dieser Bericht.

## Prüfungen

| Nr | Befehl | Ergebnis | Exit |
| --- | --- | --- | --- |
| 1 | `node produkt/test/schritt05.mjs` | ohne p2 eine Fläche; mit p2 zwei Flächen, Verweis aufgelöst, Dateien byte-gleich | 0 |
| 2 | `node produkt/test/schritt04.mjs` | holt, gibt, liest vorab, kein remote; neue Form geprüft | 0 |
| 3 | `node produkt/test/schritt03.mjs` | 5 Vorhaben, 11 Positionen, 11 Schritte, 1 Person; eigene Fläche zuerst | 0 |
| 4 | `node produkt/test/schritt02.mjs` | Ablage, Urheber und Lauf ohne Konfiguration unverändert | 0 |
| 5 | `node --check produkt/index.js`, `ablage.js`, `client.js` | keine Syntaxfehler | 0 |
| 6 | `grep -n 'workflows' produkt/index.js \| head` | `datenAntwort` baut `workflows`; eigene zuerst | 0 |
| 7 | `grep -n 'fremd' produkt/client.js \| head` | fremde Fläche und Nur-lesen im Client | 0 |
| 8 | `git -C /workspace/projekte/board status --porcelain` | leer | 0 |
| 9 | `git -C /workspace/projekte/board log -1 --format='%an %s'` | `Thomas Heinrich Fremde Workflows` | 0 |

Ausgabe von `schritt05.mjs`:

```
ok ohne p2: workflows hat 1 Eintrag, die eigene Person zuerst
ok mit p2: 2 Flächen, eigene zuerst; 2 fremde Schritte mit host p2 und fremd
ok Verweis: aufgelöst als Prüfen (host p2), p2-Datei unverändert
ok Speichern: Verweis bleibt Verweis, p2 unverändert
ok p1.json und p2.json: vor und nach dem Lesen byte-gleich
ok Prüfordner aufgeräumt: /tmp/board-schritt05-…
```

Ausgabe von `schritt03.mjs`:

```
ok /board/data: 5 Vorhaben, 11 Positionen, 11 Schritte, 1 Person
ok workflows: 1 Fläche(n), eigene zuerst: p1*
```

**Entscheidende Zeile:** `ok Verweis: aufgelöst als Prüfen (host p2),
p2-Datei unverändert` — der fremde Schritt steht mit Name und Host auf meinem
Board, ohne dass seine Datei angefasst wird.

## Entscheidungen

- **Die eigene Fläche steht als erster Workflow der eigenen Person.** Die alten
  Schlüssel `workflow` und `schritte` gibt es nicht mehr; die Seite liest die
  eigene Fläche über `ich === true`. Damit gibt es eine Form, nicht zwei.
- **`fremd` steht an jedem Schritt, nicht nur an fremden.** `false` bei eigenen
  Schritten ist keine Markierung, sondern die Antwort auf dieselbe Frage; der
  Client muss nicht zwischen „fehlt“ und „ist eigen“ unterscheiden.
- **Verweis-Kennung.** Ein aufgelöster Verweis behält die Kennung des Ziels,
  solange sie im Workflow frei ist; sonst bekommt er `ref:<ref>`. So bleiben
  Positionen, die auf das Ziel zeigen, gültig, und ein gleichnamiger eigener
  Schritt wird nicht überschrieben.
- **Fremde Flächen zeigen Schritte, keine Positionen.** Der Auftrag verlangt
  die Schritte sichtbar und anklickbar; für das Lesen reicht die Reihenfolge der
  Schritte. Eine zweite, nur teilweise bedienbare Board-Fläche mit Ziehen und
  Verbindungen hätte mehr versprochen, als dort erlaubt ist.
- **Ändern fremder Schritte ist auch am Host gesperrt.** Der Client zeigt keine
  Felder; zusätzlich antwortet `/board/schritt/aendern` mit 403. Die Datei des
  Hosts wird in keinem Fall geschrieben.

## Fassung

Der Stand der Ablage ist als Fassung `Fremde Workflows` festgehalten (Urheber
`Thomas Heinrich`, `thomas.heinrich@board`). Sie enthält diesen Bericht; die
Arbeitsstände aus `arbeit/`, die beim Schritt im Arbeitsbaum lagen, stehen schon
in der Fassung davor. Die Änderungen am Produkt
(`produkt/index.js`, `produkt/ablage.js`, `produkt/client.js`, die drei
Prüfskripte) sind wie in den Schritten davor nicht Teil der Board-Fassung:
`vorgaenge/*/produkt/` ist im Repository ignoriert.

## Grenzen und offene Punkte

- **Nicht neu gestartet.** Die neue Antwort und die fremden Flächen sind an
  Modulkopien geprüft, nicht am laufenden Board. Nach einem Neustart liest die
  Seite die neue Form.
- **Im Browser nicht gesehen.** Die Seite ist syntaktisch geprüft und einmal mit
  einem React-Ersatz aufgebaut (beide Flächen erscheinen, kein Absturz); das
  tatsächliche Bild im Browser ist nicht geprüft, weil dafür ein Neustart nötig
  wäre.
- **Eine automatische Fassung dazwischen.** Beim Laden des Produkts in einem
  Prüflauf hat dessen Zeitgeber („Änderung außerhalb des Boards“, alle 60
  Sekunden) den damaligen Arbeitsstand als Fassung `Modell (Thomas Heinrich) —
  Änderung außerhalb des Boards` festgehalten. Sie enthält nur Dateien aus
  `arbeit/`, keine fremde Ablage; die Fassung `Fremde Workflows` ist die
  jüngste und nennt Thomas Heinrich.
- **Die echte Ablage hat weiter nur `p1`.** Die zweite Person wurde
  ausschließlich in `/tmp` angelegt, wie im Auftrag verlangt.
