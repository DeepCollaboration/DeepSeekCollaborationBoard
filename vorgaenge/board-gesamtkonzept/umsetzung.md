---
etage: Umsetzung
status: fertig
offene_fragen: 4
stand: 2026-10-30
---

# Umsetzung — CollaborationBoard

## Offene Fragen

1. **2026-10-30, an ihn: Sichtprüfung der Oberfläche (Schritte 86 und 97).**
   Reiter, Einführung, Filter, Kartengröße, Einrasten, Kanban, Ziehen zwischen
   Spalten und WIP-Warnung sind ohne Browser geprüft. Erst sein Durchgang sagt,
   ob es so bleibt; die Wege stehen in `arbeit/abnahmeliste.md`.
2. **2026-10-30, an ihn: Bilder für README und Handbuch.** Die Stellen sind
   frei; Aufnahmen aus seinem Browser fehlen.
3. **2026-10-30, an ihn: Store-PR.** Der Eintrag liegt als Pull Request bei
   `awesome-dsh-plugin` (https://github.com/awesome-dsh-plugin/awesome-dsh-plugin/pull/10571);
   ob er angenommen wird, entscheidet der Markt, nicht dieses Board.
4. **2026-10-30, an ihn: Push und Tag.** Die fünfte Runde (Schritte 87–97)
   liegt nur im Arbeitsbaum; `origin/main` und `v0.1.0` tragen den Stand davor.
   Ohne Push und eine neue Fassung ist sie von GitHub nicht installierbar.

## Die Architektur

Das Board ist eine gemeinsame Fläche aus getrennten Git-Repositories: Jede
Station, jeder Workflow, jedes Vorhaben und jedes Board hat eine eigene
Ankerdatei und eine eigene Geschichte; das Plugin setzt sie zusammen, ohne
Inhalte zu besitzen. Der Harness ist der Prozess, Git die Ablage, GitHub der Ort
für Rechte und Zusammenarbeit — es gibt keinen Dienst, keine Datenbank und keine
eigene Anmeldung. Öffentlich und privat entscheidet GitHub, geschrieben wird
erst nach der Rechteprüfung, und die alte Ablage der Fassung 1 bleibt lesbar.

## Was entstanden ist

**Das Produkt** liegt unter `/workspace/projekte/board-plugin`:

- `index.js` — die Host-Hälfte: 71 Routen unter `/board/*`, Format- und
  Archivprüfung vor jedem Schreiben, Rechte- und Verantwortungsprüfung,
  Lesemodus, Board-Register, Katalog, GitHub-Handlungen, Gespräch und Skill.
- `client.js` — die Seite: Kopfzeile, Schiene, Docks, Fläche mit Reitern,
  Kanban, Modulansicht einer Station, Bibliothek mit Filter, Katalog,
  Einzelheit von Station, Workflow und Vorhaben, Beiträge, Fragen, Fork,
  Einrichtung und Identität.
- `boards.js`, `ablage.js`, `repos.js`, `komposition.js`, `katalog.js`,
  `github.js`, `rechte.js`, `abgleich.js`, `erststart.js` — Register und
  Identität, alte Ablage, Repository-Erkennung, Zusammensetzen, Katalog,
  GitHub, Sichtbarkeit und Recht, Abgleich und Erststart.
- `formate/` — `board.js`, `workflow.js`, `station.js`, `vorhaben.js`,
  `katalog.js`, `hilfe.js` und `herkunft.js`: prüfen, lesen und schreiben die
  Ankerdateien; `herkunft.js` ist die reine Regel für „eigen, öffentlich,
  privat“ in der Bibliothek.
- `vorlagen/start/` — achtzehn Ordner: das Startboard `board-start/`, drei
  Workflows, dreizehn Stationen und das Vorhaben „Probedurchlauf“.
- `werkzeuge/` — Abnahme, Prüffläche, Schlüsselzählung und der Umzug von
  Fassung 1 nach 2.
- `test/` — 98 Prüfdateien; 64 davon laufen ohne Werkstatt und bilden den
  CI-Filter. 640 Schlüssel je Wörterbuch in sieben Sprachen, Deutsch ist die
  Ausgangssprache ohne eigenes Wörterbuch.
- `README.md`, `README.de.md` und sechs Kurzfassungen, `docs/` mit
  Architektur, Entwicklung, Routen, Datenformaten und Fehlersuche, dazu
  `CHANGELOG.md`, `CONTRIBUTING.md`, `SECURITY.md`, Lizenz und CI.

**Der Arbeitsbaum** trägt die fünfte Runde: 38 geänderte und 16 unversionierte
Dateien über `HEAD bbcd746` (Tag `v0.1.0`). Sie ist gebaut und geprüft, aber
nicht committet und nicht gepusht.

## Start

1. **Im Profil.** Das DSH-Profil `web` verweist direkt auf
   `/workspace/projekte/board-plugin`; nach einem Neustart des Harness lädt es
   den aktuellen Stand. Einen eigenen Dienst, Port oder Einrichtungsschritt
   gibt es nicht.
2. **Von GitHub.** `dsh plugin --profile web add github:DeepCollaboration/DeepSeekCollaborationBoard-Plugin`
   installiert den Stand von `v0.1.0` — ohne die fünfte Runde, solange sie
   nicht gepusht ist.
3. **Erster Start.** Ein leeres Register legt unter `$DSH_HOME/board/start/`
   das Startboard an: achtzehn lokale Repositories mit je einem Commit und Tag
   `v1`, ohne Netz. Das Board ist offen, die Einführung öffnet sich einmal je
   Sitzung, ihre erste Station steht im Dock. Ein vorhandenes Register bleibt
   unangetastet.
4. **Prüfen.** `curl -s "$DSH_WEB_URL/board/data"` antwortet mit 200 und trägt
   `format: 2`, `workflows`, `stationen`, `vorgaenge`, `luecken` und `stand`;
   `GET /board/sprache?id=en` liefert 640 Schlüssel.
5. **Prüfungen.** `cd /workspace/projekte/board-plugin && npm run check &&
   node --test test/*.mjs && node werkzeuge/abnahme.mjs`.

## Was es kann

- **Ein Board aus getrennten Repositories zeigen:** Stationen, Workflows,
  Vorhaben und Boards; jede Karte nennt Host, Fassung und Sichtbarkeit; fehlt
  ein Repository, steht die Lücke an seiner Stelle.
- **Reiter über der Fläche:** „Öffentlich“ versammelt alle Stationen des
  Boards, jeder Workflow hat einen eigenen Reiter mit eigenen Positionen und
  Kanten; Pfeile, Home und End wechseln, der Fokus folgt, eine offene Station
  wird beim Wechsel geschlossen.
- **Stationen als Module:** Eine Station mit `modul` öffnet eine eigene Ansicht
  über dem Board — Text, Bild, Video, Gespräch und Knopf, „Zum Board“, Escape
  und ein „weiter“ zur nächsten Station. Ohne `modul` ändert sich nichts.
- **Bilder aus dem Repository:** `bild.quelle` ist eine `https:`-Adresse oder
  ein relativer Pfad; der Host liefert die Datei über
  `GET /board/station/bild` mit derselben Leseberechtigung wie die Station,
  `nosniff` und 8-MB-Grenze; Pfade außerhalb des Repositories und fremde
  Endungen werden abgewiesen.
- **Ein Startboard mit drei Workflows:** öffentliches Board, Kanban und
  Einführung, dreizehn Stationen, ein Vorhaben; aus Vorlagen angelegt,
  idempotent, ohne Netz, mit Tag `v1`.
- **Eine Einführung:** vier Stationen mit Text, Bild, Gespräch und Knopf; sie
  öffnet sich beim ersten Start, „Überspringen“ merkt den Stand, ein „?“
  öffnet sie wieder; an „Eigene Station“ steht der Token-Weg samt Token-Feld.
- **Bibliothek mit Filter:** Herkunft (eigen, öffentlich, privat) und
  Schlagwort, Zahlen an den Abschnitten, Leerfall mit „Filter zurücksetzen“.
- **Karten in drei Stufen:** 1×1, 2×1, 2×2; der Griff unten rechts schaltet
  per Maus oder Enter/Leertaste; Ziehen rastet am 10-px-Raster und an
  Nachbarkanten unter 8 px ein; die Größe wird in Fassung 2 gespeichert.
- **Kanban:** Ein Workflow mit Schlagwort `kanban` zeigt seine Stationen als
  Spalten in der Reihenfolge des Workflows; Vorhaben lassen sich an einer
  Kanban-Station anlegen, stehen in der Spalte ihrer Station, lassen sich
  dorthin ziehen, nach Person filtern, die zuständige Person setzen und im
  Detail öffnen. Die WIP-Grenze aus `regeln.wip` warnt, blockiert aber nicht.
- **Die bisherigen Wege:** Bibliothek, Katalog, Verlauf, Leser und Gespräch;
  Stationen und Vorhaben anlegen, ändern, löschen, verbinden, weitergeben,
  abgleichen und als Archiv lesen; acht Oberflächensprachen über die Sprache
  des Dashboards, Arabisch rechtsläufig.
- **GitHub als Zugang:** Token prüfen, speichern (0600) und löschen; Rechte je
  Repository; Pull Requests, Fragen, Forks und Sterne lesen und anlegen.

## Was es nicht kann

- **Keinen Code aus einer Station ausführen.** Module zeigen Inhalte; Skripte
  und Code-Module sind bewusst zurückgestellt, bis es eine abgeschottete
  Ausführung mit Erlaubnis je Station und Fassung gibt.
- **Kein Kanban-Zubehör.** Fristen, Labels, Checklisten und Swimlanes fehlen;
  die Grundfassung soll erst im Gebrauch stehen.
- **Kein Kanban im öffentlichen Reiter.** Dort liegen Stationen aus mehreren
  Workflows; die Spalte hätte keine eindeutige Ordnung, also bleibt es bei der
  freien Fläche.
- **Keine übersetzten Inhalte.** Die Oberfläche spricht acht Sprachen; die
  Texte der Einführung und der Vorlagen sind deutsche Inhalte.
- **Kein Zoom.** „Alles zeigen“ und der Sprung zur Station lösen das
  Wiederfinden; ein Zoom müsste alle Zeigerkoordinaten umrechnen und ist ohne
  Browser nicht prüfbar.
- **Keine echte zweite Person und keinen zweiten Rechner.** Der Abgleich ist
  zwischen Klonen auf einer Platte belegt.
- **Ohne Token nichts auf GitHub schreiben.** Mit Remote bleibt das Board im
  Lesemodus, auch wenn ein SSH-Schlüssel schreiben könnte.
- **Die fünfte Runde ist nicht veröffentlicht.** Sie liegt nur im Arbeitsbaum;
  eine Installation von GitHub liefert `v0.1.0` ohne sie.

## Ungeprüft

1. **Die Browser-Sicht (Schritte 86 und 97).** Reiter, Einführung, Filter,
   Kartengröße, Einrasten, Kanban, Ziehen und WIP sind im Quelltext und gegen
   den Server geprüft, nicht mit Maus und Augen.
2. **Die echten GitHub-Schreibwege.** Anlegen, Veröffentlichen, Pull Request,
   Frage, Fork, Teams und Push sind gegen einen nachgebildeten Server und
   lokale Bare-Repositories geprüft, nicht live; der Token der Werkstatt darf
   nur lesen.
3. **Eine Installation des aktuellen Standes von GitHub.** Der Weg ist mit
   `link:` aus einer Kopie gegangen; die `github:`-Zeile trägt bis zum Push
   den alten Stand.
4. **Ein zweiter Rechner und eine echte zweite Person.** Zwei Klone auf einer
   Platte ersetzen das nicht.
5. **Ein automatisierter Browser-Test.** Er fehlt; die Prüfungen lesen
   `client.js` als Text und rufen die Routen.

## Wichtigste Entscheidungen

- **Getrennte Repositories statt einer Ablage.** Jedes Ding hat seine eigene
  Geschichte, seine Rechte und seine Fassung; die alte Ablage bleibt lesbar.
- **GitHub ist das Tor, der Token ist optional.** Lesen, Klonen und lokales
  Schreiben brauchen keinen Token; nur die GitHub-API-Handlungen brauchen ihn.
- **Verweise statt Kopien.** Ein Board nennt, was es benutzt; fehlt ein
  Repository, steht eine Lücke mit Grund statt eines erfundenen Namens.
- **Das Startboard kommt aus Vorlagen, nicht aus dem Netz.** Der erste Start
  legt achtzehn lokale Repositories an, idempotent und ohne Anmeldung; das
  Hauptboard wird nur noch ausdrücklich geholt.
- **Die Einführung ist ein Workflow mit einem Vorhaben, kein Sonderfall.** Sie
  benutzt dieselben Formate und dieselbe Fläche wie jeder andere Workflow.
- **Stationen sind Inhalte, kein Code.** Ein Modul zeigt Text, Bild, Video,
  Gespräch und Knopf; ausführbarer Code kommt erst mit einer abgeschotteten
  Ausführung und Erlaubnis je Station und Fassung.
- **Kanban ist ein Workflow mit Schlagwort.** Keine zweite Fläche und keine
  zweite Ablage: Spalten sind die Positionen, Chips die Vorhaben an ihrer
  Station.
- **Feste Kartengrößen und ein reines Einrasten.** Drei Stufen statt freier
  Größen; `rasteEin` ist eine reine Funktion und ohne Browser prüfbar.
- **Die WIP-Grenze ist weich.** Sie warnt und färbt den Zähler, blockiert aber
  nicht; ein Zug geht nie verloren.
- **Acht Sprachen über das Dashboard.** Die Oberfläche folgt der
  Dashboard-Sprache; Inhalte bleiben deutsch. Arabisch ist rechtsläufig.
- **Geprüft wird ohne Browser und gegen Nachbildungen.** Ein HTTP-Server
  ersetzt GitHub, lokale Bare-Repositories ersetzen Remotes; die echten
  Repositories werden nur gelesen.

## Wie geprüft wurde

```
cd /workspace/projekte/board-plugin
npm run check                                         # Exit 0
node --test test/*.mjs                                # 98 von 98, Exit 0
node --test $(ls test/*.mjs | grep -vE '/(markt(25|26|27|42|45|46|48)|schritt(03|0[5-9]|1[0-9]|2[0-9]|30))\.mjs$')
                                                      # 64 von 64, Exit 0
node werkzeuge/abnahme.mjs --bericht …/arbeit/abnahme.md
                                                      # Exit 0, 13/13, Suite 98/98
```

Die Abnahme belegt außerdem: Live-Probe gelesen (0 Sterne, öffentlich, `main`),
Hauptboard 11 Schritte/1 Vorhaben/0 Lücken, Team-Board 11 Schritte/4
Vorhaben/0 Lücken, Register mit drei Boards und aktiv `hauptboard-neu`, alte
Ablage in den Daten unverändert seit `ae905d8` (HEAD `6123b58`). Jeder Schritt
87–96 hatte einen Arbeiter und einen frischen Prüfer; die Urteile stehen mit
Befehl, Exit-Code und entscheidender Zeile in `arbeit/pruefung.md`.

In diesem Schritt sind die Zahlen in `README.md`, `README.de.md`,
`docs/architektur.md` und `docs/fehlersuche.md` auf 98 Prüfdateien und 64
selbstständige Prüfungen nachgezogen; `docs/entwicklung.md` trug sie schon.
`docs/routen.md` ist gegen die `registriere`-Aufrufe abgeglichen: 71 von 71
Routen stehen in der Tabelle. `docs/datenformate.md` deckt `modul`,
`schlagworte`, `groesse`, `regeln.wip` und `knopf.ziel` (auch `token`) so, wie
`formate/*.js` sie prüft.

**Kanban im Startboard, mit Token geprüft:** In einem temporären Zuhause, mit
gesetztem Token und `anlegeweg: attrappe`, gegen den echten Host: Anlegen an
`kanban-idee` antwortet 200 und schreibt `workflow-kanban`; Anlegen an `idee`
antwortet 200 und schreibt `workflow-oeffentlich`; ein Zug von `kanban-idee`
nach `kanban-in-arbeit` antwortet 200 und setzt eine neue Fassung in
`vorhaben.json`; mit `regeln.wip: 1` kommt `ok: true` und `warnung: wip`.
Dafür entsteht ein Vorhaben jetzt im Workflow der gewählten Station, und ein
Zug bleibt im Workflow des Vorhabens.

**Eine Messung wurde geradegezogen:** `werkzeuge/abnahme.mjs` startete die
Suite ohne Dateiliste. `node --test` zählt dabei auch die Helferdatei
`test/rechte-texte.js` als Prüfung (99 statt 98). Das Werkzeug führt jetzt wie
`npm test` genau `test/*.mjs` aus; die Abnahme meldet 98 von 98.

## Abweichungen von der Architektur

- **Der erste Start holt nicht mehr das Hauptboard.** `architektur.md` sagt:
  „Das Hauptboard wird geholt.“ Seit Schritt 91b legt der Erststart das lokale
  Startboard an; das Hauptboard bleibt über `POST /board/hauptboard/holen`
  erreichbar. Grund: Ein frisches Profil soll ohne Netz und ohne Anmeldung
  etwas zeigen. Die Produktdokumentation beschreibt den neuen Weg.
- **Die fünfte Runde steht nicht in der Architektur.** Module, Reiter,
  Startboard, Einführung, Filter, Kartengrößen und Kanban kommen aus dem
  Leitsatz der Runde in `arbeit/plan.md`, nicht aus `architektur.md`. Sie
  erweitern die Fläche, ohne eine zweite Mechanik zu bauen; die Architektur
  wurde nicht nachgezogen.
- **Schreiben mit Remote hängt am Token.** Die Architektur lässt Git-Schreiben
  ohne Token zu, wenn SSH-Schlüssel gehen. `rechte.js` wählt ohne Token den
  Lesemodus; das ist die sichere, lokal prüfbare Variante.
- **Sichtbarkeit ohne GitHub ist lokal.** Für Repositories ohne Remote
  entscheidet `sichtbarkeit.json` und die Namensregel, nicht GitHub. Ohne Netz
  kann GitHub nicht antworten.
- **Veröffentlichen bleibt unter der Organisation.** Ein eigenes Board
  veröffentlicht nicht unter seinem Eigentümer; die Eigentümerangabe wirkt
  beim Anlegen von Station und Vorhaben. Das ist die Entscheidung vom
  2026-10-29, keine stille Abweichung.

Die Konvergenzprüfung vom 2026-10-28 fand 18 Abweichungen; mehrere sind mit dem
Marktplatz behoben (feste Fassungen, Dateien in Fassung 2 speichern, Vorhaben
anlegen, fehlende Routen, Verweise als `owner/repo`, Themen und Suche,
Konfliktdatei). Der alte Befund steht in `arbeit/konvergenz.md`, die
Entscheidungen in `arbeit/entscheidungen.md`.

## Dateien

- **Produkt:** `/workspace/projekte/board-plugin/` mit `index.js`, `client.js`,
  `boards.js`, `ablage.js`, `repos.js`, `komposition.js`, `katalog.js`,
  `github.js`, `rechte.js`, `abgleich.js`, `erststart.js`, `formate/*.js`,
  `vorlagen/start/`, `werkzeuge/`, `test/`, `docs/`, `README*.md`,
  `CHANGELOG.md`, `package.json`, `cordis.patch.yml`, `icon.svg`.
- **In diesem Schritt geändert:** `README.md`, `README.de.md`, `CHANGELOG.md`,
  `docs/architektur.md`, `docs/datenformate.md`, `docs/fehlersuche.md`,
  `docs/routen.md`, `werkzeuge/abnahme.mjs`, `index.js`, `komposition.js`,
  `vorlagen/start/workflow-oeffentlich/workflow.json`, `test/nach89.mjs`.
- **Arbeit:** `arbeit/plan.md`, `arbeit/fortschritt.md`,
  `arbeit/entscheidungen.md`, `arbeit/pruefung.md`, `arbeit/abnahme.md`,
  `arbeit/abnahmeliste.md`, `arbeit/schritte/87.md` … `97.md` samt Berichten.
- **Alte Ablage:** `/workspace/projekte/board/` (Fassung 1, Daten unverändert).
- **Lokal:** `$DSH_HOME/board/boards.json`, `identitaet.json`,
  `darstellung.json`, `github.json`, `start/`, `cache/`, `ablagen/`.

## Verlauf

Runde 1–3: Grundgerüst, Fläche und Oberfläche (Schritte 01–56, 57–66).
Runde 4: Name und Oberfläche (80–86). Runde 5: Einstieg, Module, Kanban
(87–97). Die Einzelheiten stehen in `arbeit/fortschritt.md` und
`arbeit/pruefung.md`; der frühere Bericht ist dort aufgegangen.
