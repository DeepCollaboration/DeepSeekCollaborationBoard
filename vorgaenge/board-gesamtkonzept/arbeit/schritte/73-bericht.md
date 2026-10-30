# Schritt 73 — Bericht

## Angelegt

Vier Dokumente unter `docs/` im Plugin-Repository, auf Deutsch, mit relativen
Links auf die Quelldateien:

| Datei | Inhalt |
| --- | --- |
| `docs/architektur.md` | Host und Client, Module, Speicherorte, kein Dienst/keine Datenbank/keine eigene Anmeldung, GitHub als Zugang und Recht, Zusammensetzen, Slots, Übersetzung, Grenzen |
| `docs/entwicklung.md` | Voraussetzungen, Einklinken ins Profil, Prüfen (83/49/34), Aufbau und Regeln von `client.js`, eine Sprache hinzufügen, Fehlersuche |
| `docs/datenformate.md` | Fassung 1 und 2, Verweise, Felder und Beispiele für `board.json`, `station.json`, `workflow.json`, `vorhaben.json`, `katalog.json`, `sichtbarkeit.json`, „Was fehlen darf“, alte Ablage |
| `docs/routen.md` | alle 70 Routen mit Methode, Pfad, Zweck, Recht/Voraussetzung und Antwort, gemeinsame Prüfungen, Fehlercodes, Weg Token → Prüfung → Schreiben |

Kein Code, kein Test, keine andere Datei im Plugin geändert; kein Commit.
Dieser Bericht liegt außerhalb von `docs/`, wie verlangt.

## Nachbesserung nach der Prüfung

Die Prüfung in `arbeit/pruefung.md` hat zwei sachliche Fehler und eine
verschwiegene Lücke gefunden. Alle drei sind behoben; nur `docs/` wurde
geändert.

1. **Speicherorte richtiggestellt.** `docs/architektur.md` sagte, die
   Repositories lägen „entweder“ unter `$DSH_HOME/board` „oder bei GitHub“.
   Jetzt steht dort: `$DSH_HOME/board` trägt Register, Identität, Token und
   Zwischenspeicher; jedes Repository liegt am lokalen Pfad aus `repo` im
   Register, oft `$DSH_HOME/board/cache/<owner>/<repo>/`,
   `$DSH_HOME/board/ablagen/<kennung>/` oder an einem eigenen Werkstattpfad
   wie `/workspace/projekte/board-repos/board-hauptboard`; `remote` nennt
   GitHub, wenn es eines gibt. Belegt am Register
   `/dsh-home/board/boards.json` (`hauptboard-neu` und `team` mit
   `/workspace/projekte/board-repos/…`, `remote: null`).
2. **Abschnittsreihenfolge von `client.js` richtiggestellt.** Die alte Liste
   war eine thematische Zusammenfassung in falscher Ordnung. Jetzt stehen die
   20 tatsächlichen `// --- …`-Marken in ihrer Reihenfolge, dazu die 14
   Marken innerhalb von `BoardPage`. Abgeglichen mit
   `grep -nE '^ {4}// --- ' client.js`; die Liste stimmt 20 von 20.
3. **„Bekannte Lücken“ ergänzt.** `docs/architektur.md` hat jetzt ein eigenes
   Kapitel mit: Kopfkommentar nennt 55 von 70 Routen; 11 Routen ohne
   Methodenprüfung (`GET*`); Fassung 1 prüft an den Struktur-Routen kein
   Verantwortungsrecht; 34 von 83 Prüfungen brauchen die Werkstatt (49 in der
   CI); kein Browser-Test. `docs/routen.md` ist als maßgebliche Routenliste
   genannt.

## Quellen

Gelesen wurden `index.js` (Routen, `registriere`, Ablage, Rechte, Modellwege),
`formate/{board,station,workflow,vorhaben,katalog,hilfe}.js`, `ablage.js`,
`boards.js`, `repos.js`, `komposition.js`, `abgleich.js`, `rechte.js`,
`github.js`, `client.js` (Kopf, Stile, Sprache, Slots, `apply`), `README.md`,
`CONTRIBUTING.md`, `SECURITY.md`, `CHANGELOG.md`, `package.json`,
`cordis.patch.yml`, `.github/workflows/ci.yml`, `test/grundlagen.mjs`,
`test/markt48.mjs`, `test/nach66.mjs`, `test/nach63.mjs`, `test/nach85b.mjs`,
`test/schritt30.mjs`, `werkzeuge/pruefflaeche.mjs`,
`werkzeuge/texte-schluessel.mjs`, `werkzeuge/umzug2.mjs` und
`werkzeuge/abnahme.mjs`.

## Prüfungen (Stichproben)

| Prüfung | Ergebnis |
| --- | --- |
| Routentabelle gegen `grep "path: '/board"` | 70 Zeilen, 70 Codepfade, keine fehlend, keine erfunden, keine doppelt (`index.js:3641–8166`) |
| Methoden der Tabelle gegen die `req.method`-Prüfungen | 70 von 70 stimmen; `/board/station` ist als GET, POST erkannt (POST-Zweig vor der GET-Prüfung) |
| `GET*`-Liste | genau die 11 Routen ohne `req.method`-Prüfung |
| Formatfelder gegen die Leser | 45 Feldzeilen in vier Tabellen, jedes Feld im jeweiligen Leser vorhanden |
| `format: 2` | `formate/hilfe.js:37` erzwingt es für alle vier Ankerdateien |
| `vorhaben.farbe` | Pflichtfeld im Leser (`formate/vorhaben.js:60`) |
| `sichtbarkeit.json` | Reihenfolge Home → Elternordner → Name (`rechte.js:84–108`) |
| Schreibrechte | `admin/maintain/push/write` (`rechte.js:43`), 30 s (`rechte.js:46`) |
| Zahlen | 83 Testdateien, 34 Werkstatt-Ausnahmen in der CI, 49 selbstständige; 614 Schlüssel = 605 Client + 9 Host-Sätze; acht Wörterbücher, `de.json` leer |
| Sprachtests | `node --test test/markt48.mjs test/nach66.mjs test/grundlagen.mjs` grün |
| Syntax | `npm run check` grün |
| Relative Links | alle Ziele vorhanden; Anker `#sichtbarkeitjson` und `#eine-sprache-hinzufügen` passend zu den Überschriften |

## Abweichungen und Befunde

1. **Der Kopfkommentar von `index.js` ist keine vollständige Routenliste.**
   Er nennt 55 der 70 Routen und führt `/board/station` nur als GET. Das steht
   jetzt unter „Bekannte Lücken“ in `docs/architektur.md`; `docs/routen.md`
   folgt den `registriere`-Aufrufen, nicht dem Kopf.
2. **Elf Routen prüfen die Methode nicht.** `/board/data`, `/board/doc`,
   `/board/dateien`, `/board/datei`, `/board/versionen`, `/board/version`,
   `/board/verlauf`, `/board/skills`, `/board/transkript`,
   `/board/gespraech/stand`, `/board/werkstatt/stand` führen auch bei POST die
   Lesefunktion aus; ein 405 gibt es dort nicht. Die Tabelle kennzeichnet das
   mit `GET*`.
3. **Eine neunte Sprache macht `test/grundlagen.mjs` rot.** Der Test verlangt
   `texte.length === 8`; die Anleitung nennt das als eigenen Schritt. Ohne
   diese Anpassung ist die Aufgabe „eine Sprache hinzufügen“ nicht grün.
4. **Englisch und Chinesisch sind nicht in der `addLanguage`-Liste**, haben
   aber Wörterbücher. Die Liste meldet nur Sprachen an, die das Dashboard
   nicht selbst mitbringt. Das steht so in `README.md` („Das Dashboard selbst
   bleibt bei Englisch oder Chinesisch“) und passt zu `client.js`.
5. **Fassung 1 prüft nicht jede Struktur-Route.** `/board/schritt`,
   `/board/kante`, `/board/layout`, `/board/position/*` und die
   Fremd-Verweise schreiben ohne Verantwortungsprüfung; nur die Handlungen am
   Vorhaben fragen `darf`. Das Dokument sagt das ausdrücklich, statt eine
   Prüfung zu behaupten.
6. **Der Statuskopf fehlt bewusst.** Die vier Dateien sind Produktdokumente
   für ein öffentliches Repository; `README.md`, `CONTRIBUTING.md` und
   `SECURITY.md` führen ebenfalls keinen. Der Arbeitsbereichs-Statuskopf wäre
   hier eine falsche Etage.

## Offen

- **Einklinken als Pfad.** `dsh plugin --profile web add <Pfad>` ist aus der
  Hilfe von `dsh` abgeleitet (der Befehl wird an pnpm weitergegeben), nicht
  aus dem Repository belegt. Der dokumentierte Weg über Paket oder Pfad ist
  als Anleitung formuliert, nicht als geprüfte Installation.
- **Neuladen des Clients.** Ob nach einer Änderung an `client.js` ein
  Seitenneuladen genügt, steht nirgends im Repository. Das Dokument empfiehlt
  deshalb den Neustart als sicheren Weg und sagt die Unsicherheit.
- **83 Tests.** Die Zahl ist die der Dateien unter `test/`; der Testläufer
  zählt sie als 83 Tests. Eine Zählung über einen vollen `npm test`-Lauf
  wurde nicht abgewartet, weil die Werkstatt-Prüfungen die echte Ablage
  lesen.
- **Sprachanleitung als Stichprobe.** Die Schritte wurden gegen die Prüfungen
  und Zählungen geprüft, aber nicht durch ein neuntes Wörterbuch ausgeführt
  (das hätte `docs/` verlassen und `texte/` verändert).
- **`sichtbarkeit.json` wird von keiner Board-Route geschrieben.** Belegt ist
  nur der lesende Zugriff in `rechte.js` und der schreibende im Umzug; eine
  Pflegemöglichkeit in der Oberfläche gibt es nicht und wird nicht behauptet.
- **Kein Browser.** Die sichtbare Wirkung der Regeln (Farben, Schrift,
  Radien) ist nur über die Textprüfungen belegt, nicht im Browser gesichtet.
