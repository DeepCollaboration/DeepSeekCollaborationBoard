---
etage: Umsetzung
status: fertig
offene_fragen: 3
stand: 2026-10-29
---

# Umsetzung — Board

## Offene Fragen

1. **2026-10-29, an ihn: Der Web-Prozess muss neu starten.** Der laufende
   Prozess liefert weiter den Stand von vor dieser Sitzung; die Schritte 25–44
   liegen in den Dateien. Nach dem Neustart ist die Browser-Abnahme dran.
2. **2026-10-29, an ihn: Ein zweites Konto für den Live-Teil des offenen
   Wegs.** A legt an, B findet, benutzt, forkt und stellt einen Pull Request —
   das ist mit zwei Identitäten gegen einen nachgebildeten Server geprüft; der
   Live-Teil mit einem zweiten Menschen fehlt, weil es kein zweites Konto gibt.
3. **2026-10-29, offen: Ein Zugang mit Recht auf die Organisation
   `DeepCollaboration`.** Veröffentlichen unter der Organisation ist gebaut,
   aber nur gegen einen nachgebildeten Server geprüft; der Testtoken ist kein
   Mitglied der Organisation.

## Die Architektur

Das Board ist eine öffentliche Werkstatt und ein privater Arbeitsplatz. Jede
Station, jeder Workflow, jedes Vorhaben und jedes Board ist ein eigenes
Git-Repository; das Plugin setzt aus diesen Repositories die Fläche zusammen.
Es besitzt keine Inhalte: Was auf der Seite steht, steht in den Repositories;
was jemand ändert, wird dort als neue Fassung festgehalten. Es gibt keinen
Dienst dazwischen, keine Datenbank und keine eigene Anmeldung. Der Harness ist
der Prozess, Git die Ablage, GitHub der Ort für Rechte und Zusammenarbeit.

Das Plugin hat zwei Hälften: die Host-Hälfte mit den Routen unter `/board/*`
und die Seite im Browser. Die Host-Hälfte liest die Formate der Fassung 2,
setzt das Board zusammen, führt Schreibvorgänge ins richtige Repository und
gleicht ab; die Seite zeigt Board, Anzeige, Bibliothek, Katalog, die Einzelheit
einer Station, eines Workflows oder Vorhabens und die Beiträge. Persönliche
Daten liegen unter `$DSH_HOME/board/`: das Register der Boards, die Identität,
der optionale GitHub-Token, der Zwischenspeicher der Klone und der Sterne.

Öffentlich und privat entscheidet GitHub. Ein öffentliches Ding darf nur auf
öffentliche Dinge verweisen; die Komposition prüft das und zeigt sonst eine
Lücke statt einer stillen Kopie. Wer in einem Repository nicht schreiben darf,
sieht es im Lesemodus; geschrieben wird erst nach der Rechteprüfung. Ein
Beitrag ist ein Pull Request, eine Frage ein Issue, ein eigenes Weiterführen
ein Fork. Das Board führt keine zweite Beitragsablage; es zeigt den Stand von
GitHub.

Die alte Ablage der Fassung 1 bleibt lesbar. Der Umzug hat ihre Inhalte in
getrennte Repositories geschrieben; die alte Ablage selbst ist unverändert.
Trägt sie im Register `archiv: true`, ist sie Archiv: Die Leiste zeigt
„Archiv“ und „Nur lesen“, ihre Vorhaben und Dateien bleiben lesbar, und jeder
schreibende Weg antwortet mit 403, bevor eine Datei angefasst wird. Ein
frisches Profil bekommt das öffentliche Hauptboard von selbst und braucht
weder Namen noch Token, um zu lesen. Ein eigener Name wird erst gebraucht, um
beizutreten, etwas beizutragen oder ein eigenes Board zu übernehmen.

## Was entstanden ist

**Das Produkt** liegt unter `/workspace/projekte/board-plugin`:

- `index.js` — die Host-Hälfte: 66 Routen unter `/board/*`, Format- und
  Archivprüfung vor jedem Schreiben, Rechte- und Verantwortungsprüfung,
  Lesemodus, Board-Register, Katalog, GitHub-Handlungen, Gespräch und Skill.
- `client.js` — die Seite: Board-Leiste mit „Archiv · Nur lesen“, Anzeige,
  Bibliothek, Katalog, Einzelheit von Station, Workflow und Vorhaben, Beiträge,
  Fragen und Fork, Einstellungen und Identität.
- `boards.js` — Register und Identität, Board-Liste, Hauptboard, Kennzahlen;
  ein Board der Fassung 1 kann als Archiv geführt werden.
- `ablage.js` — die alte Ablage der Fassung 1 lesen und schreiben, je Datei nur
  bei Änderung; sie bleibt für das alte Board im Register.
- `repos.js` — ein Repository der Fassung 2 erkennen und lesen; der lokale
  Zwischenspeicher unter `$DSH_HOME/board/cache/`.
- `formate/` — `station.js`, `workflow.js`, `vorhaben.js`, `board.js`,
  `katalog.js`, `hilfe.js`: prüfen, lesen und schreiben die fünf Ankerdateien.
- `komposition.js` — aus einem Board und seinen Verweisen das Bild der Seite;
  Lücken statt erfundener Namen.
- `katalog.js` — der Katalog der öffentlichen Dinge mit Sternen, Fassung, Host,
  Suche und Zwischenspeicher; ohne Netz der letzte Stand.
- `github.js` — GitHub REST lesen und mit Token schreiben; Token 0600,
  Zwischenspeicher, Grenzen, klare Fehler.
- `rechte.js` — Sichtbarkeit, Verweisprüfung, Schreibrecht, Verantwortung.
- `abgleich.js` — der Abgleich je Repository über Git; Stand ohne Netz.
- `erststart.js` — das Hauptboard beim ersten Start holen, Verweise mitholen,
  „nicht geholt“ mit Wiederholen.
- `werkzeuge/abnahme.mjs` — die Abnahme: Prüfungen, Suite, Live-Probe, echte
  Boards, Register, alte Ablage, sieben Wege.
- `werkzeuge/umzug2.mjs` — der Umzug von Fassung 1 auf getrennte Repositories;
  `werkzeuge/zeige-umzug.mjs` — der geschriebene Stand als Bericht;
  `werkzeuge/pruefflaeche.mjs` — die Prüffläche unter `/tmp/board-scheibe1/`.
- `test/schritt02.mjs` … `test/schritt40.mjs` samt `schritt33b.mjs` — 40
  Prüfungen; 18–40 sind in diesem Lauf neu, 02–17 bleiben Bestand.
- `README.md`, `package.json`, `cordis.patch.yml`, `icon.svg`, `locale/`.

**Die migrierten Repositories** liegen unter
`/workspace/projekte/board-repos/`: elf Stationen, `workflow-standard`, vier
Vorhaben, `board-hauptboard` und `board-team` — 18 Repositories; dazu
`vorhaben-probeprojekt` als öffentliches Probeprojekt, zusammen 19.
`sichtbarkeit.json` und `umzug.md` liegen daneben. Jedes Repository hat eine
eigene Geschichte; die Stationen und der Workflow tragen Tag `v1`.

**Die alte Ablage** `/workspace/projekte/board` bleibt unverändert: Fassung 1,
`HEAD ae905d8`, 42 Fassungen; ihre Board-Daten sind nicht angefasst.

**Lokal unter `$DSH_HOME/board/`**: `boards.json` mit drei Boards, aktiv
`hauptboard-neu`; `identitaet.json`; `einstellungen.json`; `gespraeche.json`.
Der Zwischenspeicher der Klone, `sterne.json`, `katalog-cache.json`,
`github.json` und `ablagen/` entstehen beim Benutzen.

## Start

1. Den Harness neu starten. Das Profil `web` lädt das Plugin direkt aus
   `/workspace/projekte/board-plugin`; einen eigenen Dienst, Port oder
   Einrichtungsschritt gibt es nicht.
2. Das Board in der Seitenleiste öffnen. Das Register führt das Hauptboard
   (neu), das Team-Board und die alte Ablage; offen ist `hauptboard-neu`.
   Ohne Neustart zeigt der laufende Prozess noch seinen geladenen alten Stand.
3. Prüfen: `curl -s "$DSH_WEB_URL/board/data"` antwortet mit 200 und trägt
   `format: 2`, `person`, `repo`, `boards`, `workflows`, `stationen`,
   `vorgaenge`, `luecken` und `stand`. `curl -s "$DSH_WEB_URL/board/katalog"`
   liefert die Einträge des Hauptboards.
4. Die Prüfungen: `cd /workspace/projekte/board-plugin && node --test` →
   40 von 40. Die Abnahme:
   `node werkzeuge/abnahme.mjs --bericht <pfad>` → Exit 0.

Für eine wirklich frische Installation ist der Weg in der `README.md`
beschrieben: Plugin installieren, neu starten, das Hauptboard kommt von selbst.
Der Arbeitsbaum des Produkts ist nicht committet; die README-Installation ist
durch diesen Stand nicht gedeckt.

## Was es kann

- **Ein Board aus getrennten Repositories zeigen**: Stationen, Workflow,
  Vorhaben und ihre Dateien; jede Karte nennt Host, Fassung und Sichtbarkeit;
  fehlt ein Repository, steht die Lücke an ihrer Stelle.
- **Die Bibliothek**: Stationen, Workflows und Vorhaben mit Host und Fassung;
  Suche und Katalog der öffentlichen Dinge. Sterne, Beispiele, offene Fragen und
  der Repository-Verweis stehen erst in der geöffneten Einzelheit, nicht auf
  jeder Karte.
- **Eine Station prüfen**: das Vertrauensbild aus Beschreibung, Beispielen,
  Verwendung und Fassungen; Sterne und Releases fehlen dort noch. Sterne werden
  auf GitHub gegeben, nicht im Board.
- **Ein Board hinzufügen, wechseln und entfernen**: das Hauptboard ist fest;
  lokale Boards und das Team-Board stehen daneben; das Register liegt lokal.
- **Öffentlich und privat trennen**: Sichtbarkeit aus Home, Elternordner und
  Name; ein öffentliches Ding verweist nur auf öffentliche; ein privates Ding
  ohne Zugang erscheint als Lücke, nicht als leerer Name.
- **Im Lesemodus arbeiten**: ohne Schreibrecht fehlen die Handlungen; ein
  Schreibversuch wird abgewiesen, bevor etwas entsteht.
- **Dateien öffnen und lesen**: Fassungen mit Urheber vergleichen. Bearbeiten
  und Speichern ist für Fassung 2 noch nicht umgestellt; die Route schreibt
  weiter in die alte Ablage.
- **Beiträge als Pull Requests**: lesen und anlegen; der Stand auf GitHub wird
  gezeigt, entschieden und zusammengeführt wird dort.
- **Fragen und Forken**: Issues lesen, anlegen, offen und geschlossen zeigen;
  einen Fork anlegen und verlinken; Pull Requests bleiben aus der Fragenliste.
- **Verantwortung und Teams**: Verantwortliche je Station, Host einer Station,
  Teams der Organisation; Schreiben nur für Verantwortliche.
- **Das Hauptboard kommt von selbst**: beim ersten Start geholt, sobald die
  öffentliche Adresse steht, seine Verweise mitgeholt; ohne Netz „nicht geholt“
  mit Wiederholen, der Rest bleibt bedienbar.
- **Das Probeprojekt übernehmen**: aus dem Katalog in ein eigenes lokales Board;
  die Kopie verweist mit `fork_von` auf das Original, Workflows und Stationen
  werden nur verwiesen; der Name wird erst dabei verlangt.
- **Eigene Boards lokal führen**: unter `$DSH_HOME/board/ablagen/`, ohne Remote,
  mit eigener Geschichte; später über den Token veröffentlichen.
- **Mit Token nach GitHub schreiben**: Repositories anlegen, veröffentlichen,
  Pull Requests, Fragen und Forks; Token prüfen, speichern (0600) und löschen.
- **Die alte Ablage als Archiv lesen**: Fassung 1 bleibt mit ihren Vorhaben
  und Dateien lesbar; ist sie im Register als Archiv markiert, zeigt die Leiste
  „Archiv“ und „Nur lesen“, und jeder Schreibversuch wird mit 403 abgewiesen,
  bevor die Ablage angefasst wird. `BOARD_ARCHIV=1` erzwingt dasselbe für eine
  Prüfung, ohne die echte Ablage zu markieren.
- **Den Umzug belegen**: die migrierten Boards lesen sich ohne Pfadumbau, mit
  elf Schritten und ohne Lücke.

## Was es nicht kann

- **Ohne Token nichts auf GitHub schreiben.** Pull Requests, Fragen, Forks,
  Teams, private Metadaten und das Veröffentlichen brauchen den Token oder den
  Weg über den Browser; das Board verweist dann nur.
- **Die neuen Repositories sind nicht veröffentlicht.** Sie liegen unter
  `/workspace/projekte/board-repos/` und haben keinen Remote;
  `DeepCollaboration/board-hauptboard` gibt es auf GitHub nicht. Ein frisches
  Profil bleibt deshalb bei „Hauptboard nicht geholt“, bis veröffentlicht ist.
- **Das Probeprojekt und der Katalog sind lokal.** Sie sind gebaut und geprüft,
  aber nicht gepusht; der öffentliche Katalog zeigt sie erst nach dem
  Veröffentlichen.
- **Der Plugin-Stand ist nicht veröffentlicht.** Das Produkt ist ein
  Git-Repository mit Remote, aber 42 Einträge im Arbeitsbaum sind unversioniert
  oder geändert (36 unversioniert, 6 geändert); es gab keinen Commit und keinen
  Push. `origin/main` steht lokal auf `52b054b`; die README-Installation ist
  durch diesen Stand nicht gedeckt.
- **Die echte alte Ablage ist noch nicht als Archiv markiert.** Der Schalter
  ist gebaut und mit `BOARD_ARCHIV=1` belegt; die Markierung im echten Register
  erfolgt erst nach der Abnahme durch den Auftraggeber. Bis dahin bleibt das
  echte alte Board im Normalbetrieb beschreibbar.
- **Ohne Remote kein Abgleich zwischen zwei Rechnern.** Die neuen Repositories
  haben keinen `origin`; das Board sagt „kein remote“ und arbeitet lokal weiter.
- **`regel: "alle"` ist nicht benutzbar.** Es gibt keinen Weg, die Zustimmung
  mehrerer einzusammeln; das Board bietet dann niemandem eine Handlung an.
- **Kein zweiter Rechner, keine echte zweite Person.** `p2`, `anna` und `thomas`
  leben nur in Prüfkopien; ein echter zweiter Rechner ist nicht vorhanden.
- **Der Browser ist nicht durchgeklickt.** Die Seite ist über `node --check` und
  die von ihr gerufenen Routen geprüft, nicht mit Maus und Augen.

## Ungeprüft

1. **GitHub-Schreibwege live**: Pull Request, Frage, Fork, Teams, privates
   Lesen, Veröffentlichen und Push sind gegen einen nachgebildeten Server und
   lokale Bare-Repositories geprüft, nicht gegen GitHub.
2. **Die Oberfläche im laufenden Harness nach dem Neustart**: Layout, Ziehen,
   Markdown, Scrollen und die neuen Flächen; der laufende Prozess trägt noch
   einen älteren Stand.
3. **Eine frische Installation gegen das echte GitHub**: Der Erststart ist mit
   lokalen Bare-Repositories belegt; die öffentliche Adresse ist noch nicht da.
4. **Ein zweiter Rechner und eine echte zweite Person**: Der Abgleich ist
   zwischen Klonen auf einer Platte belegt, nicht über zwei Rechner.
5. **Ein Abgleich über HTTPS oder SSH**: geprüft mit einem lokalen
   Bare-Repository; echte Schlüssel und ein echter Remote fehlen.
6. **Eine echte Rate-Limit-Grenze und ein abgelaufener Token gegen GitHub**:
   als Verhalten geprüft, nicht live erlebt.
7. **Private GitHub-Repositories ohne Zugang**: als Lücke geprüft, nicht mit
   einem echten privaten Repository der Organisation.
8. **Das Veröffentlichen der neuen Boards**: Der Weg ist da und geprüft, aber
   kein Repository wurde unter der Organisation angelegt.

## Wichtigste Entscheidungen

- **Getrennte Repositories statt einer Ablage.** Station, Workflow, Vorhaben
  und Board haben je ihre eigene Geschichte, ihre Rechte und ihre Fassung. Das
  ist der Kern des neuen Bauplans; die alte Ablage bleibt lesbar.
- **GitHub ist das Tor, der Token ist optional.** Lesen, Klonen und Schreiben
  über Git brauchen keinen Token; nur die GitHub-API-Handlungen brauchen ihn.
  Er liegt unter `$DSH_HOME/board/github.json`, 0600, und erscheint nirgends.
- **Die Live-Probe liest nur.** Sie prüft Sterne, Sichtbarkeit und Standardzweig
  des öffentlichen Repositories; bei Netz- oder Grenzfehler heißt es „live
  nicht geprüft“ und der Lauf bleibt bestehen.
- **Die Prüfungen laufen gegen Nachbildungen.** Ein eigener HTTP-Server ersetzt
  GitHub, lokale Bare-Repositories ersetzen Remotes; die echten Repositories
  werden nur gelesen. Kein Prüflauf schreibt in sie.
- **Das Probeprojekt entsteht in Schritt 21.** Ein echtes Probeprojekt braucht
  den Übernehmen-Weg; bis dahin zeigt das Hauptboard Stationen und Workflow.
  Deshalb trägt das Hauptboard heute ein öffentliches Vorhaben.
- **Nackte Verweise liegen als Geschwister.** Die Komposition sucht einen
  nackten Repo-Namen zuerst neben dem lesenden Repository; absolute und
  relative Pfade bleiben, wie sie sind.
- **Die migrierten Repositories liegen nebeneinander.** `/workspace/projekte/board-repos/`
  ist der Ort; die alte Ablage bleibt unangetastet. Ein Rückweg ist das
  Zurückverweisen des Registers.
- **Der Umzug schreibt unter die Organisation.** Der persönliche GitHub-Name
  ist nicht eingetragen; die öffentlichen Dinge und die vier privaten Vorhaben
  laufen unter `DeepCollaboration`.
- **Das Register wurde von Hand geradegerückt.** Drei Boards, aktiv
  `hauptboard-neu`; die alte Kennung `hauptboard` bleibt für die alte Ablage.
- **Die alte Ablage wird erst nach der Abnahme abgenommen.** Der Archiv-Schalter
  ist gebaut und geprüft; die Markierung des echten Registers erfolgt erst nach
  der Bestätigung durch den Auftraggeber.

## Wie geprüft wurde

- Jeder der 24 Schritte — dazu die Nachbesserungen 05b und 16b — hatte einen
  Arbeiter und einen frischen Prüfer; jedes Urteil steht mit Befehl, Exit-Code
  und entscheidender Zeile in `arbeit/pruefung.md`. Die Prüfungen 18 bis 40
  enden mit Exit 0; die alten Prüfungen 02 bis 17 bleiben grün.
- `node --test` → 40 von 40 bestanden, Exit-Code 0.
- Die Abnahme `node werkzeuge/abnahme.mjs --bericht ...` → Exit 0: 13 von 13
  Prüfungen der neuen Form bestanden, Suite 40 von 40, sieben Wege belegt,
  Live-Probe gelesen (0 Sterne, öffentlich, `main`), Hauptboard 11 Schritte/
  1 Vorhaben/0 Lücken, Team-Board 11 Schritte/4 Vorhaben/0 Lücken, Register drei
  Boards mit `hauptboard-neu` aktiv, alte Ablage `HEAD ae905d8` unverändert. Der
  vollständige Befund steht in `arbeit/abnahme.md`.
- Die Abnahme vergleicht die Köpfe und offenen Änderungen der 19 echten
  Repositories und der alten Ablage vor und nach dem Lauf; sie sind gleich.
- Die Prüfungen arbeiten auf `/tmp/board-scheibe1/` und räumen selbst auf; das
  echte `$DSH_HOME` und die echten Repositories werden nicht geschrieben. Nur
  der Live-Leser bekommt ein eigenes temporäres Home für den Stern-Cache.

## Abweichungen von der Architektur

- **Das Probeprojekt kommt später als geplant.** Die Architektur und der
  Bauplan erwarten es schon auf dem Hauptboard; ein echtes Probeprojekt braucht
  aber den Übernehmen-Weg aus Schritt 21. Folge: Das Hauptboard trägt heute ein
  öffentliches Vorhaben, und die Abnahme-Vorgabe „0 Vorhaben“ ist überholt.
  Ohne das Probeprojekt sind es 0.
- **Nackte Verweise werden als Geschwister aufgelöst.** Die Architektur nennt
  Verweise, aber nicht ihre Auflösung. `komposition.js` sucht einen nackten
  Namen zuerst neben dem lesenden Repository; sonst blieben die migrierten
  Boards bei zwei Lücken.
- **`sichtbarkeit.json` liegt auch neben den Repositories.** Der Umzug schreibt
  seine Sichtbarkeitsangabe in den Elternordner; `rechte.js` liest sie nach der
  Datei im Home als zweite Quelle, bevor der Name entscheidet. Die Architektur
  nennt nur `$DSH_HOME/board/sichtbarkeit.json`.
- **Prüfungen überschreiben Adressen über Umgebungsvariablen.**
  `DSH_BOARD_HAUPTBOARD`, `DSH_BOARD_REMOTE_BASIS`, `DSH_GITHUB_API` und
  `DSH_BOARD_CACHE_TTL` gibt es nur für Prüfungen; im Betrieb gilt die
  öffentliche Adresse.
- **Der Betrieb ist noch lokal, nicht bei GitHub.** Die Architektur setzt
  GitHub als Ort der Repositories; die neuen Repositories haben keinen Remote,
  das Hauptboard ist nicht veröffentlicht, und der Plugin-Stand ist nicht
  gepusht. Das ist ein offener Betriebsstand, keine andere Form.
- **Die echte alte Ablage ist noch nicht als Archiv markiert.** Der Schalter ist
  gebaut und geprüft, die Markierung steht aus. Die Architektur sieht die alte
  Ablage als lesbar und archiviert vor; bis zur Markierung bleibt sie im
  Normalbetrieb beschreibbar.


## Konvergenz

Die Konvergenzprüfung gegen `architektur.md` ist gelaufen (2026-10-28, Produkt
`52b054b` plus Arbeitsbaum, Suite 40 von 40). Sie fand **18
Produktabweichungen** und **4 Betriebspunkte**; der vollständige Befund steht in
`arbeit/konvergenz.md`. Die Abweichungen werden in diesem Lauf **nicht behoben**
— der Bauplan schneidet sie nicht, und ein Teil ist ohne Token und echte
GitHub-Repositories nicht live prüfbar. Sie sind hier mit Grund genannt:

1. **Feste Fassungen werden angezeigt, aber nicht gelesen.** Die Komposition
   liest den Arbeitsbaum; eine neue Stationsfassung wirkt sofort, statt vom
   Workflow bewusst nachgezogen zu werden. Ein Nachziehen fehlt.
2. **Dateien in Fassung 2 lassen sich nicht speichern.** `POST
   /board/doc/schreiben` schreibt weiter in die alte Ablage und antwortet für
   ein Vorhaben-Repository mit 404. Die Route ist ein Erbe der Fassung 1.
3. **`POST /board/vorhaben` legt kein eigenes Vorhaben-Repository an.** Die
   Route hat keinen Fassung-2-Zweig; ein neues Vorhaben entsteht so nicht.
4. **Sichtbarkeit kommt aus `sichtbarkeit.json` und dem Ordnernamen, nicht von
   GitHub.** Das ist die in Schritt 13 vereinbarte lokale Angabe, bis GitHub
   sie trägt; ohne Token und Remote bleibt sie lokal.
5. **Schreibrecht hängt am Token.** Mit Remote und ohne Token ist das Board
   lesend, auch wenn ein SSH-Schlüssel schreiben könnte. Die Architektur erlaubt
   Git-Schreiben ohne Token; das Produkt wählt den sicheren Lesemodus.
6. **`/board/board`, `/board/board/verbinden` und `/board/workflow` fehlen.**
   Vorhanden sind `/board/data`, `/board/boards`, `/board/boards/wechsel`,
   `/board/workflow/station` und `/board/workflow/verbinden`; die verlangten
   Pfade heißen anders.
7. **Der Umzug ist ein CLI, keine Route.** `/board/umzug` fehlt; das Werkzeug
   schreibt außerhalb des Profils und wird bewusst von Hand gestartet. Die
   Personen-Zuordnung ist pauschal auf einen Host.
8. **Die Themen `dsh-board-*` werden nicht gesetzt.** `veroeffentlichen` legt
   Repositories ohne Themen an.
9. **Der Katalog zeigt keine Beispiele und gibt `geprüft` nicht weiter.**
10. **Bibliothekskarten tragen keine Sterne, Beispiele, offenen Fragen oder
    Repository-Verweise**; diese stehen erst in der Einzelheit.
11. **Das Vertrauensbild einer Station zeigt keine Sterne und Releases.**
12. **Offene Pull Requests zählen im Katalog als offene Fragen**, weil GitHub
    `open_issues_count` beide zählt.
13. **Der Katalog filtert private Einträge nicht.** Ein öffentliches Hauptboard
    könnte in `katalog.json` ein privates Repository nennen.
14. **Die alte Beitrags- und Fork-Ablage bleibt für Fassung 1 bestehen.** Sie
    ist der Leseweg der alten Ablage, nicht der Weg der Fassung 2.
15. **Neue Verweise werden als absolute lokale Pfade geschrieben** (Workflow,
    Board, Übernehmen). Das funktioniert auf diesem Rechner, nicht bei einem
    anderen Leser eines öffentlichen Boards.
16. **Veröffentlichen behandelt alle verwiesenen Repositories als eigene.** Ein
    lokales Board würde auch die öffentlichen Stationen des Hauptboards
    umhängen.
17. **Eine Konfliktmeldung nennt die Datei nicht**, weil der Abgleich nur
    stderr liest.
18. **`GET /orgs/{org}/repos` fehlt**; der Katalog liest nur `katalog.json`.

Die vier Betriebspunkte sind kein Produktfehler: kein Token, keine echten
GitHub-Repositories und kein Zugang zur Organisation, kein Browser, keine
Netzprüfung der Suite. **Damit ist der Bauplan umgesetzt; die Architektur ist
nicht vollständig erfüllt.** Die Punkte 1–3, 6, 15 und 16 sind der nächste
Schnitt, wenn die Architektur ganz gelten soll.

## Ausblick

Der Marktplatz und die verbliebenen Konvergenzpunkte sind als zweiter Bauabschnitt geplant: `bauplan.md`, Schritte 25–44. Die Architektur trägt ihn als Nachtrag vom 2026-10-28.

## Dateien

- Produkt: `/workspace/projekte/board-plugin/index.js`, `client.js`,
  `boards.js`, `ablage.js`, `repos.js`, `komposition.js`, `katalog.js`,
  `github.js`, `rechte.js`, `abgleich.js`, `erststart.js`, `package.json`,
  `cordis.patch.yml`, `icon.svg`, `README.md`, `locale/de.json`,
  `locale/en.json`, `formate/*.js`, `werkzeuge/abnahme.mjs`,
  `werkzeuge/umzug.mjs`, `werkzeuge/umzug2.mjs`, `werkzeuge/zeige-umzug.mjs`,
  `werkzeuge/pruefflaeche.mjs`, `test/schritt02.mjs` … `test/schritt40.mjs`.
- Echte Repositories: `/workspace/projekte/board-repos/` mit elf Stationen,
  `workflow-standard`, vier Vorhaben, `board-hauptboard`, `board-team`,
  `vorhaben-probeprojekt`, `sichtbarkeit.json`, `umzug.md`.
- Alte Ablage: `/workspace/projekte/board/` (Fassung 1, unverändert).
- Lokal: `$DSH_HOME/board/boards.json`, `identitaet.json`,
  `einstellungen.json`, `gespraeche.json`; dazu nach Benutzung
  `github.json`, `sterne.json`, `katalog-cache.json`, `cache/`, `ablagen/`.
- Arbeit: `arbeit/plan.md`, `arbeit/fortschritt.md`, `arbeit/entscheidungen.md`,
  `arbeit/pruefung.md`, `arbeit/abnahme.md`, `arbeit/schritte/01.md` …
  `arbeit/schritte/24.md` samt Berichten.

## Zweiter Bauabschnitt — Der Marktplatz

Stand 2026-10-29: Die Schritte 25–44 aus `bauplan.md` sind gebaut und geprüft; die Suite steht bei 60 von 60. Der Nachtrag „Der Marktplatz“ in `architektur.md` ist umgesetzt, mit den unten genannten Abweichungen.

### Die Architektur

Das Board ist offen: Jeder legt Stationen und Vorhaben unter seinem eigenen GitHub-Namen an. Ein Thema am Repository (`dsh-board-station`, `dsh-board-vorhaben`) macht sie auffindbar; das Board setzt sie zusammen, ohne sie zu besitzen. Die Organisation `DeepCollaboration` bleibt das kuratierte Schaufenster; die eigene Arbeit liegt beim Eigentümer. Der Anlegeweg ist eine Schnittstelle mit zwei Plätzen — `selbst` (heute) und `attrappe` (Prüfung) —, damit später ein Torwächter an seine Stelle treten kann, ohne die Fläche umzubauen. Rechte werden pro Person gefragt; ohne Anmeldung bleibt Lesen.

### Was entstanden ist

- **Eigentümer.** `einstellungen.eigentuemer` im `board.json`; Auflösung: gesetzt → angemeldeter Login → leer mit Hinweis. Das Hauptboard trägt `DeepCollaboration`. Das Feld steht in den Einstellungen der Anzeige.
- **Anmelden und dürfen.** `schreibrecht` fragt pro Person und Repository; `darfGrund`/`darfHinweis` und `anlegen` stehen in `/board/data`; ohne Recht zeigt die Leiste „Nur lesen“ und keine Schreibknöpfe; lokales Schreiben bleibt möglich.
- **Station anlegen.** `POST /board/station` über den Anlegeweg; öffentliches Repository, Thema, `station.json`, README, Commit; Namenskollision 409; die Karte steht im Board.
- **Auffinden.** Suche nach `topic:dsh-board-station`; der Katalog nennt `kuratiert` und `gefunden`, führt über `owner/repo` zusammen und hält den letzten Stand.
- **Vorhaben anlegen.** `POST /board/vorhaben` für Fassung 2; `vorhaben.json` mit Position, Workflow-Fassung und Farbe; die Karte steht an der Station.
- **Dateien.** `/board/doc/schreiben` schreibt in das Vorhaben-Repository, mit Basisprüfung, Pfadschutz und Commit als handelnde Person.
- **Fassungen.** Die Komposition liest `station.json` über `git show <fassung>:station.json`; „Fassung nachziehen“ ist eine eigene Route; die Karte nennt die Fassung.
- **Verweise.** Drei Schreibstellen bilden `owner/repo` aus dem Remote; ohne Remote steht der Ordnername; alte Formen bleiben lesbar.
- **Konflikt.** Der Abgleich nennt die betroffene Datei und bricht sauber ab.
- **Katalog.** Jeder Eintrag trägt Herkunft, Beispiele, Prüfung, Sterne, offene Fragen und Beiträge (getrennt); private Einträge und gesperrte verschwinden aus den Listen, der direkte Verweis bleibt.
- **Vertrauensbild.** Stationsseite und Bibliothekskarte zeigen die Angaben mit Verweisen.
- **Mitarbeiten.** Beitragen öffnet einen Pull Request; es entsteht keine lokale Beitragsdatei; die alten Beitrags- und lokalen Fork-Wege sind entfernt. Verantwortliche stehen an den Karten; Veröffentlichen lässt fremde Repositories aus und nennt sie.
- **Abschluss.** Die drei fehlenden Routen antworten; der Umzug läuft als Route mit Zuordnung; der offene Weg ist mit zwei Identitäten nachgebildet; die frische Installation ist lesend bedienbar.

### Start

1. DSH neu starten — der laufende Server trägt den Stand von vor dieser Sitzung.
2. Board öffnen; Einstellungen → Eigentümer prüfen (Hauptboard: `DeepCollaboration`).
3. GitHub-Abschnitt: „Verbinden“; der Token liegt bereits.
4. Bibliothek → „Station anlegen“; an einer Station → „Vorhaben anlegen“.
5. Ohne Token: lesen; anlegen erst mit Token.

### Was es kann

- Anonym lesen: Board, Katalog, Station, Workflow, Pull Requests.
- Mit Token: Stationen und Vorhaben unter eigenem Namen anlegen, auffinden, in Workflows stellen, beitragen, veröffentlichen (nur eigene Repositories).
- Private Dinge bleiben aus dem öffentlichen Katalog; die Sperrliste nimmt Einträge heraus, ohne sie zu löschen.
- Zwei Menschen: A legt an, B findet, benutzt, forkt und stellt einen Pull Request — nachgebildet; live ist die A-Seite gegangen.

### Was es nicht kann

- Kein Torwächter (GitHub App oder Action), keine Geräte-Anmeldung, keine automatische Mitgliedschaft in der Organisation, kein Übertrag eigener Repositories in die Organisation.
- Kein Server, keine eigene Anmeldung, keine Datenbank — GitHub bleibt das Tor.
- Veröffentlichen legt unter der Organisation an; ein eigenes Board veröffentlicht nicht unter seinem Eigentümer.
- Gefunden werden nur Stationen; Workflows, Vorhaben und Boards tragen ihre Themen noch nicht.

### Ungeprüft

- Der Browserdurchgang nach dem Neustart. Die Tests prüfen die Wege im Prozess, nicht mit Maus und Augen.
- Live mit einem zweiten Konto: Fork und Pull Request eines fremden Menschen.
- Live gegen echtes GitHub: Veröffentlichen und Umzug (nur nachgebildet).
- Die Organisation als Eigentümer: Der Testtoken ist kein Mitglied von `DeepCollaboration`.

### Wichtigste Entscheidungen

- Marktplatz statt Organisationswerkstatt: Die Organisation ist das Schaufenster, die eigene Arbeit bleibt beim Eigentümer.
- Die Naht ist klein: `einstellungen.eigentuemer`, `einstellungen.anlegeweg` und die Frage „darf diese Person hier anlegen?“. Kein Torwächter gebaut, keiner in der Oberfläche angeboten.
- Recht pro Person, nicht pro Token; ohne Anmeldung lesen.
- Kein Schreibrecht im Plugin, keine automatische Mitgliedschaft — beides wäre nicht durchsetzbar.
- Private Namensregel gilt auch im Katalog; öffentliche Ausnahmen stehen ausdrücklich in `sichtbarkeit.json`.
- Live nur mit dem Testkonto, nie mit seinem Konto.

### Wie geprüft wurde

- `node --test`: 60 Tests, 60 bestanden, 0 Fehler. Je Schritt ein Arbeiter und ein frischer Prüfer; zwei Nachbesserungen (kurzer HEAD bei Schritt 31; Namensregel und Datenschutz bei Schritt 36).
- Live mit dem Testkonto `ThomasTestDSBOARD`: Token-Rechte (Repository anlegen, Datei schreiben, Thema setzen, löschen) geprüft; eine Station und ein Vorhaben angelegt, Thema und Dateien geprüft, wieder gelöscht; die A-Seite des offenen Wegs (anlegen, anonym im Katalog finden) gegangen.
- Konvergenz gegen `architektur.md` mit frischem Prüfer; die Befunde stehen unten und in `arbeit/konvergenz.md`.

### Abweichungen von der Architektur

Die Konvergenzprüfung fand neun Punkte. Zwei sind behoben, die übrigen stehen hier mit Grund. Sie ändern nichts daran, dass die gebauten Wege funktionieren; sie sind der nächste Schnitt.

**Behoben**

- **Verweisprüfung erreichte `owner/repo` nicht.** `pruefeVerweise` löst die neue Form jetzt zusätzlich über `$DSH_HOME/board/cache/<owner>/<repo>` auf; ein öffentliches Board mit einer privaten `owner/…`-Station nennt die Lücke.
- **Unbekannter Anlegeweg fiel still auf `selbst` zurück.** Ein gesetzter, unbekannter Wert bricht jetzt mit 409 und deutscher Meldung ab, ohne Repository und ohne GitHub-Aufruf.

**Offen**

1. **Sichtbarkeit entscheidet die lokale Karte.** Für lokale Repositories ohne GitHub-Metadaten entscheiden `sichtbarkeit.json` und die Namensregel; GitHub entscheidet, wo ein Remote bekannt ist. Die Karte wird beim Umzug gefüllt und ausdrücklich gepflegt; sie ist die einzige Quelle, die ohne Netz antworten kann. Folge: Ein lokales `vorhaben-*` ohne Eintrag gilt als privat; das öffentliche Probeprojekt trägt seinen Eintrag.
2. **Recht je Repository ist nicht durchgezogen.** `schreibrecht` fragt Token und Person, aber nicht SSH; die Anzeige rechnet mit der Board-Person, der Server mit `IDENTITAET.github`. Die Workflow-Routen prüfen das Board-, nicht das Workflow-Repository. Das ist die größte offene Lücke; sie stammt aus der ersten Fassung und wird im Torwächter-Schnitt fällig.
3. **Kein Torwächter.** Die Naht hat zwei Plätze; `torwart` ist nicht gebaut, nicht in der Oberfläche, und ein gesetzter unbekannter Wert bricht ab. Anlegen braucht heute einen Token; ohne ihn bleibt es beim Lesen. Das ist der vereinbarte Schnitt („Was dieser Abschnitt nicht baut“).
4. **Veröffentlichen unter der Organisation.** Die Route legt unter `ORGANISATION` an und ignoriert `eigentuemer`; ein eigenes Board hat keinen Veröffentlichungsweg. Die Route stammt aus der ersten Fassung und wurde in Schritt 39 nur um die Eigentumsprüfung ergänzt.
5. **Auffinden nur für Stationen.** Workflows, Vorhaben und Boards tragen ihre Themen noch nicht; die Suche fragt nur `dsh-board-station`. Der Marktplatz beginnt bei den Stationen; die übrigen Arten folgen.
6. **Vertrauensbild an gefundenen Einträgen.** Gefundene Einträge tragen Name, Beschreibung, Sterne und Web, aber nicht Host und Fassung; die Bibliothekskarten zeigen die Zahlen nur teilweise. Die Stationsseite ist vollständig.
7. **Formate schreiben.** `station.json`, Board-Verweise und Einstellungen werden in `index.js` gebaut und geschrieben, nicht im Formate-Baustein. Die Dateien sind gültig, die Zuständigkeit ist es nicht.
8. **Urheberschaft ist der Anzeigename.** Commits tragen den Anzeigenamen, nicht den GitHub-Login. Die Architektur verlangt den Login; die Änderung berührt alle Schreibwege und die Prüfungen und steht noch aus.

### Dateien

- Produkt: `index.js`, `client.js`, `github.js`, `katalog.js`, `komposition.js`, `rechte.js`, `abgleich.js`, `formate/katalog.js`, `formate/station.js`, `formate/workflow.js`, `README.md`, `test/markt25.mjs` bis `test/markt45.mjs` (neu), einzelne Alt-Tests angepasst.
- Daten: `board-hauptboard/board.json` (`einstellungen.eigentuemer`).
- Arbeit: `arbeit/plan.md`, `arbeit/pruefung.md`, `arbeit/fortschritt.md`, `arbeit/schritte/25.md` bis `44.md`.

## Nachtrag — nach der ersten Benutzung (2026-10-29)

Drei Rückmeldungen aus dem Betrieb (gesammelt in `arbeit/feedback-2026-10-29.md`) sind umgesetzt:

1. **Der Board-Wechsel bleibt im Board.** Der Client lädt die Seite nicht mehr neu, sondern holt Bild und Board-Liste und räumt die Fläche ab (Auswahl, Leser, Gespräch, Entwürfe, Karten, Scrollstand). Dieselbe Behandlung haben Verbinden, Entfernen, Hauptboard holen, Name speichern, das erste Board und das Übernehmen des Probeprojekts; `window.location.reload()` kommt im Client nicht mehr vor.
2. **Anlegen endet mit einer Bestätigung.** Sie nennt das Repository und wo die Station zu finden ist; **Im Katalog zeigen** öffnet den Katalog mit dem Repository-Namen in der Suche und einem frischen Blick zu GitHub. Der Katalog trägt je Eintrag die Herkunft und bei gefundenen das Repository; die Suche prüft auch den Verweis.
3. **Ein „Nein“ von GitHub wird erklärt.** Die Meldung nennt Eigentümer und Anmeldung des Tokens und den Weg heraus; das Formular zeigt vorher „Entsteht unter <Eigentümer>.“. Antwort 403 statt 502. Scheitert nur das Schreiben der Dateien, sagt die Meldung das und nennt das entstandene Repository. Prüfung: `test/markt45.mjs` (beide Fälle).

Dazu aus der unabhängigen Prüfung: Zeitgrenzen und ein Generationszähler für Antworten von vor einem Wechsel; der Fehlerpfad nach einem Wechsel führt in die Board-Leiste statt zum Neuladen.

Suite 61/61. Offen bleibt aus der Rückmeldung Punkt 4 (gefundene Dinge in der Bibliothek) und die Abnahme im Browser, die einen Neustart des Prozesses braucht.

## Nachtrag — Oberfläche (2026-10-29, zweite Runde)

- **Einstellungen** sind jetzt ein Abschnitt **Board** im Einstellungsfenster des Dashboards: Akzentfarbe, Anordnung, Mausrad, Eigentümer. Der Board-Reiter „Einstellungen“ ist weg. `client.js` meldet den Abschnitt über `settings.section` an; gespeichert wird über die bestehenden Routen.
- **Theme:** Das Board setzt keine eigenen Farben mehr (außer Personenfarben und den sechs Akzentwerten); Statusfarben, Flächen, Ränder, Schatten und Schriftfamilie kommen aus `--dsw-*`. Der Akzent (`marke`, `violett`, `tuerkis`, `bernstein`, `rose`, `indigo`) wird hell und dunkel getrennt über `data-board-akzent` gesetzt; die Wahl liegt in `$DSH_HOME/board/darstellung.json` (Route `GET/POST /board/darstellung`).
- **Lesbarkeit:** kleinste Schriften 10/10,5 → 11/11,5 px, kleinere Knöpfe mindestens 28 px hoch, Fokusringe bleiben an Feldern und Editoren, die Einstellungsseite benennt ihre Felder.
- **Prüfungen:** `test/markt46.mjs` (Akzent-Route), `test/markt47.mjs` (Client ohne Browser: Registrierung, Stil, Einstellungsseite). Suite 63/63.
- **Offen:** Board-Verwaltung (Boards, GitHub, Identität) aus der Kopfzeile in den Abschnitt holen; Sprache (Deutsch, Englisch, Chinesisch, dann die übrigen fünf); Tastaturbedienung der Karten und Namen für alle Symbolknöpfe.

## Nachtrag — Sprachen (2026-10-29, dritte Runde)

- **Die Texte sind überführt.** 548 Texte im Client laufen durch `uebersetze(...)`; der erste und zweite Durchgang haben Kinder, `title`/`placeholder`/`aria-label`, Beschriftungen von `feld`/`abschnitt`/`reiter` und Meldungen erfasst, danach von Hand Statuszeilen, Formatwarnungen, Farbnamen und Veröffentlichungsmeldungen.
- **Wörterbücher:** `texte/en.json` und `texte/zh.json` mit je 548 Einträgen; `GET /board/sprache?id=…` liefert sie, unbekannte Sprache leer, ungültige Kennung 400. Deutsch braucht keine Datei — der Schlüssel ist der deutsche Text.
- **Umschalter:** Deutsch ist im Sprachumschalter des Dashboards angemeldet; Englisch und Chinesisch sind eingebaut. Die aktive Sprache kommt vom Dashboard, `locale/change` lädt das Wörterbuch neu, die Seite zeichnet neu. Fehlt ein Eintrag, bleibt der deutsche Ausgangstext.
- **Prüfungen:** `test/markt48.mjs` (Route, Schlüsselsatz, Platzhalter), `test/markt49.mjs` (Client lädt Englisch, Rückfall, Werte). Suite 65/65.
- **Offen:** Spanisch, Französisch, Russisch, Japanisch, Arabisch (Arabisch mit `dir="rtl"`); rund 100 technische Bausteine bleiben deutsch.

## Nachtrag — acht Sprachen (2026-10-29, vierte Runde)

- **Fünf weitere Wörterbücher:** `texte/es.json`, `fr.json`, `ru.json`, `ja.json`, `ar.json` mit je 548 Einträgen; zusammen mit Englisch und Chinesisch sind acht Sprachen vollständig. Deutsch braucht keine Datei.
- **Umschalter:** sechs Sprachen meldet das Board im Sprachumschalter des Dashboards an (Deutsch, Spanisch, Französisch, Russisch, Japanisch, Arabisch); Englisch und Chinesisch sind eingebaut.
- **RTL:** Arabisch setzt `dir="rtl"` an der Fläche und am Einstellungsabschnitt; logische Ränder (`marginInlineStart`) spiegeln die Kopfzeile. Die Karten behalten ihre Lagen — die Fläche ist ein Ort, keine Textspalte.
- **Datum und Zahlen:** folgen der Sprache (`sprachTag()`), nicht mehr fest `de-DE`.
- **Prüfungen:** `test/markt48.mjs` (Schlüsselsatz, Platzhalter, Route), `test/markt49.mjs` (sechs Sprachen angemeldet, Arabisch rechtsläufig, Rückfall, Werte). Suite 65/65.
- **Offen:** Tastaturbedienung der Karten und Namen für alle Symbolknöpfe; Board-Verwaltung aus der Kopfzeile in den Abschnitt „Board“.

## Nachtrag — Identität und GitHub (2026-10-29, fünfte Runde)

- **Verschoben:** Identität (Name, GitHub-Name, Farbe) und GitHub (Status, Token verbinden/löschen, Repository prüfen) sind eigene Karten im Abschnitt **Board**; sie holen ihren Stand selbst und schreiben über die bestehenden Routen. Die Board-Leiste schaltet nur noch Boards, öffnet und legt neue an.
- **Entfernt:** die vier Handler (`boardIdentitaetSpeichern`, `githubVerbinden`, `githubLoeschen`, `githubPruefen`), ihre sieben Zustände und zwei Effekte aus der Board-Seite; kein Rest im Quelltext.
- **Texte:** fünf neue Schlüssel (u. a. „Gilt für alle Boards.“, „GitHub verbunden: {0}.“) in allen acht Wörterbüchern, ein nicht mehr benutzter entfernt.
- **Prüfung:** `test/markt47.mjs` rendert die Einstellungsseite und findet die Karten; Suite 65/65.
