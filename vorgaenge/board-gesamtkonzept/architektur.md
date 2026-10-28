---
etage: Architektur
status: in Arbeit
offene_fragen: 0
stand: 2026-10-28
---

# Architektur — Board

## Das Produkt

Das Board ist ein öffentlicher Marktplatz für Arbeitsschritte und ein privater Arbeitsplatz für Vorhaben. Jeder Arbeitsschritt — eine **Station** — ist ein eigenes Repository unter der Organisation; jedes **Vorhaben**, jeder **Workflow** und jedes **Board** ebenso. Das Plugin setzt aus diesen Repositories die Fläche zusammen: Es zeigt die Stationen, die Vorhaben an ihrem Schritt, die Dateien, die Bibliothek, den Katalog und das Vertrauensbild einer Station. GitHub trägt die Inhalte, die Rechte und die Zusammenarbeit; das Plugin ist die Ansicht und der Git-Client. Es gibt keinen Dienst dazwischen, keine Datenbank und keine eigene Anmeldung.

Eine Station ist ein Repository pro Arbeitsschritt — wie ein Repository pro Skill. Wer eine Station pflegt, pflegt eine angewandte Fertigkeit: die Anweisung, die Beispiele, die Grenzen, die Fassungen. Ein Workflow ist ein Repository pro Zusammensetzung; ein Vorhaben ein Repository pro Arbeit; ein Board ein Repository voller Verweise. Menschen begegnen sich über die öffentlichen Dinge: Sie sehen eine Station, prüfen sie, geben ihr einen Stern, verbessern sie per Pull Request, bauen sie in einen Workflow und arbeiten an einem öffentlichen Probeprojekt. Private Teams tun dasselbe hinter einer Tür, die GitHub öffnet.

Fertig ist das Produkt, wenn es das kann:

- Ein Board aus getrennten Repositories zusammensetzen: Stationen, Workflow, Vorhaben, Board; jede Datei aus ihrem Repository.
- Ein Vorhaben an seiner Station zeigen, öffnen und weitergeben; die Änderung landet im Vorhaben-Repository, die Station und der Workflow bleiben unberührt.
- Die Bibliothek zeigen: Stationen, Workflows, Vorhaben; mit Host, Fassung, Sternen, Beispielen, offenen Fragen und dem Verweis ins Repository.
- Eine Station in einen Workflow stellen, einen Workflow in ein Board übernehmen, ein Vorhaben forken.
- Öffentlich und privat trennen: öffentliche Dinge lesen alle; private nur, wer auf GitHub Zugang hat; ein öffentliches Ding verweist nur auf öffentliche Dinge.
- Beiträge als Pull Requests führen, Fragen als Issues, Forks als Forks; die Verantwortlichen entscheiden auf GitHub, das Board zeigt den Stand.
- Sterne lesen und zeigen; ein Stern wird auf GitHub gegeben.
- Ohne Dienst, Port, Datenbank und eigene Anmeldung laufen; der Harness ist der Prozess, Git die Ablage, GitHub der Ort.

### Die Teile

**Station** — ein konkreter Arbeitsschritt: was hineingeht, was herauskommt, wie er läuft (Skill, Skript, von Hand), Beispiele, Grenzen, Fassung. Ein Repository unter der Organisation, öffentlich oder privat. Der **Host** ist ihr Eigentümer; seine Änderung ist eine neue Fassung. Stationen sind die kleinste teilbare Einheit. Sterne, Issues, Pull Requests, Releases und die Beispiele im Repository sind ihr Vertrauensbild.

**Workflow** — eine Zusammensetzung aus Stationen: Reihenfolge, Abzweigungen, Positionen, Regeln. Ein Repository; er verweist auf Stationen mit Fassung (Commit oder Tag), kopiert sie nicht. Der **Verantwortliche des Workflows** pflegt die Zusammensetzung. Ein Workflow ist teilbar, sternbar, forkbar; er erscheint in der Bibliothek.

**Vorhaben** — die konkrete Arbeit: ein Name, ein Workflow mit Fassung, eine Station, an der es steht, Verantwortliche je Station, Dateien, Transkripte, Läufe. Ein Repository; öffentlich (Schaufenster) oder privat (Team). Wer es weiterführt, committet in sein Repository; wer mitarbeitet, öffnet einen Pull Request; wer es anders will, forkt es.

**Board (Workspace)** — ein Repository voller Verweise: welche Workflows, Vorhaben und Stationen ein Mensch oder ein Team benutzt, wer dazugehört, welche Einstellungen gelten. Das **öffentliche Hauptboard** ist das kuratierte Board der Organisation — die Startseite; **Team-Boards** sind private Boards, geteilt über GitHub-Teams; **lokale Boards** haben kein Remote. Die Anzeige listet die Boards, die jemand hinzugefügt hat; sie ist lokal und wird nicht veröffentlicht.

**Bibliothek und Katalog** — die Bibliothek zeigt die Dinge, die ein Board benutzt oder abonniert: Stationen, Workflows, Vorhaben. Der Katalog ist die öffentliche Sammlung der Organisation; das Hauptboard verweist auf das, was im Schaufenster steht. Host, Fassung, Sterne, Beispiele, offene Fragen und der Verweis ins Repository stehen an jedem Eintrag.

**Plugin** — die Ansicht und der Client: Es klont die Repositories in einen lokalen Zwischenspeicher, liest die Formate, setzt das Board zusammen, zeigt Bibliothek, Katalog und Vertrauensbild, führt Schreibvorgänge ins richtige Repository und gleicht ab. Es besitzt keine Inhalte.

### Wie Zusammenarbeit aussieht

Wer eine Station verbessern will, forkt ihr Repository oder öffnet einen Pull Request; der Host entscheidet. Wer einen Workflow anders zusammensetzen will, forkt ihn. Wer an einem Vorhaben mitarbeitet, öffnet einen Pull Request gegen dessen Repository; der Verantwortliche des Abschnitts nimmt an oder nicht. Wer ein Vorhaben in eine andere Richtung führen will, forkt es. Wer eine Station in einem eigenen Workflow verwenden will, verweist auf ihre Fassung. Verantwortung kann über den Weg wechseln: der Host einer Station, der Verantwortliche eines Abschnitts, der Verantwortliche des Vorhabens. Das Board zeigt sie; GitHub trägt sie.

## Techstack

| Teil | Wahl | Grund | Verworfen |
| --- | --- | --- | --- |
| Sprache | JavaScript (ESM) | Der Harness lädt das Plugin ohne Übersetzung; der vorhandene Code ist so geschrieben; ein Fremder kann ihn lesen. | TypeScript (Übersetzung und Werkzeug), Python (nicht die Sprache des Harness). |
| Laufzeit | Node.js 24 im Harness-Prozess; die Seite im Browser | Beides ist da; kein zweiter Prozess. | Eigener Node-Prozess, Deno, Bun. |
| Framework | DSH-Plugin-API: Host mit `ctx.webServer.register`, Client als DSH-Client-Plugin mit React aus dem Harness | Routen, Oberfläche und Anmeldung kommen vom Harness; das Board baut keine eigene Web-Schicht. | Express oder Fastify, ein eigenes Frontend mit Build. |
| Bibliotheken | keine zusätzlichen; Node `fs`, `path`, `child_process`, `fetch`, das `git`-Kommando, der vorhandene Markdown-Leser | Wenig Abhängigkeiten; `fetch` ist in Node 24 eingebaut; der Leser ist schon da. | Octokit (Abhängigkeit für wenige Aufrufe), simple-git (das `git`-Kommando reicht), SQLite-Treiber. |
| Datenhaltung | Git-Repositories als Wahrheit; lokaler Zwischenspeicher als JSON unter `$DSH_HOME/board/`; keine Datenbank | Jedes Ding hat sein Repository, seine Geschichte und seine Rechte; der Zwischenspeicher ist wegwerfbar. | SQLite oder Postgres (zweite Wahrheit), ein zentraler Server. |
| Protokolle | Git über SSH oder HTTPS; GitHub REST v3 über HTTPS; HTTP/JSON zwischen Seite und Host | Jede Schicht spricht, was schon da ist; kein neues Protokoll, kein offener Port. | GraphQL (mehr Apparat für wenige Abfragen), WebSocket (braucht einen Dienst), ein eigenes Sync-Protokoll. |
| Anmeldung | SSH-Schlüssel für Git; ein optionaler feiner GitHub-Token (0600) für API-Schreiben (Repository anlegen, Pull Request, private Metadaten) | Git und GitHub sind das Tor; der Token ist nur für Handlungen nötig, die Git nicht kann. | OAuth-App oder GitHub App (eigener Dienst, Rücksprung-Adresse), ein eigenes Konto. |
| Betrieb | Ein Prozess: der Harness; kein eigener Port, kein Dienst; die Repositories bei GitHub, der Zwischenspeicher lokal | Nichts muss dauerhaft laufen oder offenstehen; GitHub trägt die Zusammenarbeit. | Eigener Dienst, Docker-Sidecar, VPS. |
| Prüfungen | `node:test`; lokale Bare-Repositories; ein nachgebildeter GitHub-HTTP-Server | Kein Netz in den Prüfungen; Formate und Komposition sind prüfbar. | Prüfungen gegen das echte GitHub (langsam, von Zugängen abhängig). |

Zum Verständnis, in je einem Satz:

- **Repository** — ein Ordner mit Geschichte; jede Speicherung ist eine Fassung (Commit) mit Urheber und Zeit.
- **GitHub REST** — die Schnittstelle, über die das Plugin Sterne, Sichtbarkeit, Releases, Issues und Pull Requests liest.
- **Token** — ein Schlüssel, den man bei GitHub für ein Programm anlegt; er erlaubt genau die Handlungen, die man ihm gibt.
- **SSH** — der Schlüssel, mit dem Git schreibt, ohne ein Passwort zu fragen.

## Bausteine

| Baustein | Was er tut | Hängt ab von | Gibt nach außen |
| --- | --- | --- | --- |
| **Repo-Verwaltung** | Führt die bekannten Repositories (Board, Station, Workflow, Vorhaben), ihren lokalen Pfad, ihren Remote, ihre Sichtbarkeit und ihren Stand; klont, holt, zieht, schickt. | Git, dem Dateisystem, dem Token für private Metadaten. | Lokale Klone, Stand je Repository, Fehler je Repository. |
| **Formate** | Liest, prüft und schreibt `station.json`, `workflow.json`, `vorhaben.json`, `board.json`; kennt Fassung 2 und den Leseweg für Fassung 1. | Den Repositories. | Geprüfte Objekte; Dateien; Formatfehler. |
| **Komposition** | Setzt aus einem Board und seinen Verweisen das Bild zusammen: Stationen aus ihren Repositories, Positionen und Kanten aus dem Workflow, Karten und Dateien aus den Vorhaben, Menschen, Bibliothek, Lücken bei fehlendem Zugang. | Repo-Verwaltung, Formaten, Rechten. | Das Board-Bild für die Seite; die Zuordnung jeder Schreibhandlung zu ihrem Repository. |
| **Katalog** | Liest die öffentlichen Dinge der Organisation und die Verweise des Hauptboards; holt Sterne, Fassungen, Releases, offene Fragen und die Verwendung in Workflows; speichert sie zwischen. | GitHub-Client, Repo-Verwaltung, Zwischenspeicher. | Katalogseinträge, Sterne, Vertrauensbild, Suche. |
| **GitHub-Client** | Spricht GitHub REST: Repository-Metadaten, Sterne, Releases, Issues, Pull Requests, Teams, Organisation, Benutzer, Repository anlegen; hält Grenzen und Fehler ein. | Netz, Token (optional), Zwischenspeicher. | Metadaten und Handlungen; klare Fehler. |
| **Rechte** | Stellt fest, ob jemand in ein Repository schreiben darf (Git und GitHub); hält das Board im Lesemodus, wenn nicht; prüft, dass ein öffentliches Ding nur auf öffentliche Dinge verweist. | Repo-Verwaltung, GitHub-Client. | Erlaubte Handlungen; Lesemodus; sichtbare Lücken. |
| **Abgleich** | Holt je Repository den fremden Stand, legt die eigene Fassung darauf, schickt sie weiter; zeigt je Repository, was nur lokal liegt; hält Konflikte an ihrem Repository an. | Git, Repo-Verwaltung. | Frischer Stand je Repository; Standzeilen; Konflikte. |
| **Oberfläche** | Zeigt Board, Anzeige, Bibliothek, Katalog, Station, Workflow, Vorhaben, Leser und Gespräch; nimmt Klicks, Ziehen und Eingaben. | Den Routen und dem Board-Bild. | Die Seite; keine eigene Ablage. |
| **Umzug** | Liest eine Ablage der Fassung 1 und schreibt daraus Stationen, Workflow, Vorhaben und Board der Fassung 2; ordnet Kennungen den GitHub-Namen zu; trennt öffentlich und privat. | Formaten, Git, der alten Ablage. | Neue Repositories und ein Bericht; die alte Ablage bleibt. |
| **Plugin-Rumpf** | Meldet die Seite und die Routen beim Harness an; hält Identität, Einstellungen, Sprachdateien, Symbol. | Dem Harness. | Die Seite in der Seitenleiste, die Routen. |

## Schnittstellen

### Git

Jedes Repository hat einen Remote (`git@github.com:<owner>/<repo>.git` oder HTTPS), den Branch `main` und Tags für Fassungen (`v1`, `v1.2.0`). Der Zwischenspeicher liegt unter `$DSH_HOME/board/cache/<owner>/<repo>`. Gelesen wird der Arbeitsbaum; geschrieben wird ein Commit; geschickt wird nach `main`.

### GitHub REST

Benutzt werden: `GET /repos/{owner}/{repo}` (Metadaten, Sterne, Sichtbarkeit), `GET /repos/{owner}/{repo}/releases`, `/issues`, `/pulls`, `/topics`, `GET /orgs/{org}/repos`, `GET /orgs/{org}/teams`, `GET /user`, `GET /repos/{owner}/{repo}/collaborators/{user}/permission`, `POST /orgs/{org}/repos`, `POST /repos/{owner}/{repo}/pulls`. Der Token geht als `Authorization: Bearer`; ohne Token nur öffentliche Leseabfragen. Antworten werden zwischengespeichert; Grenzen (Rate-Limit) werden beachtet.

### Formate

**`station.json`** — `{ format: 2, id, name, beschreibung, host, eingang: [], ausgabe: [], aktion: { art: 'skill'|'skript'|'textfeld'|'datei', datei?, wert? }, beispiele: [], reife: 'entwurf'|'erprobt'|'geprüft', lizenz, tags: [] }`; daneben `README.md`, die Aktionsdatei (`SKILL.md`, Skript), `examples/`, `CHANGELOG.md`.

**`workflow.json`** — `{ format: 2, id, titel, beschreibung, autor, stationen: [{ repo, fassung, id }], positionen: [{ id, station, x, y, bedingung: [] }], kanten: [{ von, nach }], layout: 'frei'|'auto', regeln: {} }`; daneben `README.md`.

**`vorhaben.json`** — `{ format: 2, id, name, workflow: { repo, fassung }, position, verantwortliche: { <position>: [<login>] }, laeufe: [], fork_von, zweig_von, farbe }`; daneben die Dateien und `transkripte/`.

**`board.json`** — `{ format: 2, id, name, art: 'hauptboard'|'team'|'lokal', personen: [{ login, name, farbe }], workflows: [{ repo, fassung }], vorhaben: [{ repo }], stationen: [{ repo }], einstellungen: {}, layout: {} }`; daneben `README.md`.

**`katalog.json`** (im Hauptboard) — `{ format: 2, stationen: [{ repo, besonderheit }], workflows: [{ repo }], vorhaben: [{ repo }], geprüft: [{ repo, von, am }] }`.

### Board-Routen

Die vorhandenen Routen unter `/board/*` bleiben für die Fläche; dazu kommen `/board/board` (das aktuelle Board und seine Verweise), `/board/board/verbinden`, `/board/katalog`, `/board/station`, `/board/workflow`, `/board/vorhaben`, `/board/github/stand`, `/board/github/token`, `/board/umzug`. Jede Route gibt JSON; Fehler nennen das Repository.

### Lokale Dateien

`$DSH_HOME/board/boards.json` — die Boards, die dieser Mensch benutzt; `$DSH_HOME/board/identitaet.json` — Name, GitHub-Name, Farbe; `$DSH_HOME/board/github.json` — Token (0600); `$DSH_HOME/board/cache/` — die Klone; `$DSH_HOME/board/sterne.json` — der Stern-Zwischenspeicher; `$DSH_HOME/board/ablagen/` — lokale Boards ohne Remote.

### Organisation

Die öffentlichen Dinge liegen unter `DeepCollaboration`. Die Repository-Themen heißen `dsh-board-station`, `dsh-board-workflow`, `dsh-board-vorhaben`, `dsh-board-board`, `dsh-board-katalog`. Die Teams der Organisation tragen die Verantwortung: `stations`, `workflows`, `vorhaben`; die Team-Mitglieder sind die Verantwortlichen. Das Hauptboard ist ein Repository der Organisation; sein `board.json` verweist auf den Katalog.

## Daten und Besitz

- **Station** — gehört ihrem Host (Person oder Team); öffentlich oder privat; ihre Fassung ist ein Tag; ein Workflow verweist auf eine Fassung. Eine neue Fassung ändert bestehende Workflows nicht; der Workflow-Verantwortliche zieht sie bewusst nach. Das ersetzt die frühere Regel „eine neue Fassung gilt sofort“: Vertrauen und Nachvollziehbarkeit brauchen eine feste Fassung.
- **Workflow** — gehört seinem Verantwortlichen oder Team; verweist auf Stationen mit Fassung; öffentlich oder privat; ein öffentlicher Workflow verweist nur auf öffentliche Stationen. Seine Fassung ist ein Tag.
- **Vorhaben** — gehört seinem Verantwortlichen oder Team; verweist auf einen Workflow mit Fassung; öffentlich oder privat; seine Dateien, Transkripte und Läufe liegen bei ihm. Verantwortliche je Station stehen in `vorhaben.json`.
- **Board** — gehört einer Person oder einem Team; verweist auf Workflows, Vorhaben und Stationen; das öffentliche Hauptboard gehört der Organisation und ist kuratiert; Team-Boards sind privat; lokale Boards haben kein Remote.
- **Katalog** — gehört der Organisation; liegt im Hauptboard; nennt die öffentlichen Dinge und die geprüften.
- **Zwischenspeicher** — gehört niemandem; ist eine Kopie; löschbar und wiederherstellbar.
- **Sterne** — gehören GitHub; das Board liest sie und zeigt sie; es vergibt sie nicht selbst.
- **Identität** — der GitHub-Name ist die Kennung; Name und Farbe sind Anzeige und liegen lokal beziehungsweise im Board.
- **Alte Ablage** — die Fassung 1 bleibt als Repository erhalten; der Umzug schreibt neue Repositories; nichts wird überschrieben.

## Rechte und Sicherheit

- **GitHub ist das Tor.** Sichtbarkeit und Mitarbeiter oder Teams entscheiden, wer liest und wer schreibt. Das Plugin vergibt keine Rechte und umgeht keine.
- **Öffentlich heißt:** jedes Mitglied der Welt kann lesen; schreiben nur, wer auf GitHub Rechte hat. **Privat heißt:** nur wer Zugang hat, sieht das Repository; das Plugin kann es ohne Zugang nicht lesen und listet es nicht.
- **Öffentliche Dinge verweisen nur auf öffentliche Dinge.** Ein öffentliches Board, ein öffentlicher Workflow und ein öffentliches Vorhaben dürfen keine privaten Stationen, Workflows oder Vorhaben nennen; die Komposition prüft das und zeigt sonst eine Lücke statt einer stillen Kopie.
- **Lesemodus:** Wer in einem Repository nicht schreiben darf, sieht es im Lesemodus; die Seite bietet keine Handlung an, die scheitern würde; der Host weist Schreibversuche ab.
- **Beiträge sind Pull Requests.** Ein Vorschlag ist ein Branch oder ein PR gegen das Repository; der Verantwortliche prüft und führt zusammen. Fragen sind Issues; ein eigenes Weiterführen ist ein Fork. Das Board verweist und zeigt den Stand; es führt keine zweite Beitragsablage.
- **Der Token** ist fein geschnitten, liegt unter `$DSH_HOME/board/github.json` mit Rechten 0600, steht in keinem Repository und in keiner Adresse. Ohne Token bleiben Lesen, Klonen und Schreiben über Git möglich; Anlegen und PRs gehen dann über den Browser.
- **Sterne** werden auf GitHub gegeben; das Board zeigt die Zahl und verlinkt. Es gibt keine eigene Bewertung, die Sterne ersetzt.
- **Kuratierung:** Das Hauptboard und der Katalog werden von der Organisation gepflegt; „geprüft“ ist ein Eintrag im Katalog mit Person und Datum, keine stille Behauptung. Jede Änderung daran ist ein Commit oder PR.
- **Private Namen** stehen nur in der lokalen Board-Liste und im privaten Board; der öffentliche Katalog nennt sie nicht. Protokolle und Fehlermeldungen enthalten keine Token.
- **Grenzen:** Der GitHub-Client hält Rate-Limits ein, zwischenspeichert und versucht erneut; ein Fehler nennt das Repository und die Ursache.

## Ausfälle und Betrieb

- **Kein Dienst.** Das Plugin läuft im Harness; es gibt keinen eigenen Port, keine Datenbank, keine Anmeldung.
- **Ein Repository fällt aus:** Das Board zeigt die übrigen; das eine Ding steht mit seinem Fehler und, wenn vorhanden, seinem letzten Stand da. Ein kaputtes Repository verdeckt nicht das Board.
- **Offline:** Der Zwischenspeicher zeigt den letzten Stand; Schreibvorgänge werden lokal committet und beim nächsten Abgleich geschickt; die Standzeile sagt „nur lokal“.
- **GitHub nicht erreichbar oder Rate-Limit:** Sterne und Metadaten sind veraltet; Git und die vorhandenen Klone funktionieren weiter; der Katalog zeigt den letzten Stand und einen Hinweis.
- **Zugang fehlt:** Ein privates Ding erscheint als Lücke („nicht zugänglich“) oder gar nicht; das Board bleibt bedienbar.
- **Konflikt:** Der Abgleich hält am betroffenen Repository an, nennt die Datei und lässt den Rest in Ruhe; aufgelöst wird im Repository, nicht durch stilles Überschreiben.
- **Token abgelaufen:** API-Handlungen scheitern mit klarer Meldung; Lesen und Git-Schreiben bleiben, solange die Schlüssel gehen; der Token lässt sich neu eintragen.
- **Format neuer:** Das Plugin schreibt nicht, wenn die Ablage eine neuere Fassung hat; es sagt es auf der Seite.
- **Datenverlust:** Jedes Ding hat seine Git-Geschichte; der Zwischenspeicher ist ersetzbar; das Board ist eine Liste von Verweisen, die sich wieder aufbauen lässt.
- **Umzug:** Die alte Ablage bleibt unverändert; die neuen Repositories entstehen daneben; ein Rückweg ist das Zurückverweisen des Boards auf die alte Ablage.
- **Erster Start:** Das Hauptboard wird geholt; scheitert es, steht „nicht geholt“ mit Wiederholen da; der Rest des Plugins bleibt nutzbar.

## Verworfene Wege

- **Ein Repository für alles** (die bisherige Fassung): keine Sterne, keine Rechte und keine Geschichte je Ding, ein Fehler trifft alles. Durch die Entscheidung für den Katalog verworfen.
- **Ein zentraler Dienst mit Datenbank:** Betrieb, Anmeldung, Datenhoheit; widerspricht GitHub als Ablage. Verworfen.
- **Git-Submodule:** technisch, brüchig, vom Menschen zu pflegen. Verworfen; das Plugin setzt Verweise selbst zusammen.
- **Ein Katalog-Repository mit Kopien:** doppelte Inhalte, keine Sterne oder PRs je Ding. Verworfen.
- **Persönliche Repositories plus zentraler Katalog (föderiert):** offener, aber die Organisation kuratiert nicht; für später denkbar, jetzt nicht.
- **Eigene Beitrags- und Fork-Ablage** (die bisherigen `beitraege/`): GitHub hat PR, Issue und Fork; eine zweite Mechanik wäre eine zweite Wahrheit. Verworfen.
- **Octokit, simple-git, SQLite:** Abhängigkeiten für Aufgaben, die `fetch`, `git` und Dateien lösen. Verworfen.
- **GraphQL:** mehr Apparat für wenige Abfragen. Verworfen.
- **Eigene Desktop-App:** Der Harness ist die Oberfläche. Verworfen.
- **Neue Stationsfassung gilt sofort:** bricht feste Fassungen und Vertrauen; der Workflow zieht bewusst nach. Verworfen.
- **Rechte im Plugin:** GitHub ist das Tor; ein zweites Rechtemodell wäre nicht durchsetzbar. Verworfen.
- **Datenbank oder Index lokal:** Die Repositories sind die Wahrheit; ein JSON-Zwischenspeicher reicht. Verworfen.

## Annahmen

- **2026-10-28:** Die Organisation `DeepCollaboration` besteht, und der Auftraggeber hat dort Eigentümer- oder Adminrechte. Wenn nicht: Die öffentlichen Repositories können nicht angelegt werden; der Umzug stoppt.
- **2026-10-28:** Der GitHub-Name des Auftraggebers ist bekannt und wird in der Identität eingetragen. Wenn nicht: Der Umzug kann die Kennung `p1` nicht zuordnen; er stoppt an dieser Stelle.
- **2026-10-28:** Ein feiner GitHub-Token mit Recht auf die Organisation kann angelegt werden; ohne ihn bleiben Anlegen und PRs manuell. Wenn nicht: Die Oberfläche bietet die Handlungen nicht an und verweist auf GitHub.
- **2026-10-28:** Der Tarif der Organisation erlaubt die privaten Repositories und Teams. Wenn nicht: Private Werkstatt und Team-Freigabe sind nicht möglich; das Board bleibt öffentlich.
- **2026-10-28:** GitHub ist über das Netz erreichbar; Rate-Limits reichen mit Zwischenspeicher. Wenn nicht: Sterne und Metadaten sind veraltet, die Inhalte bleiben über Git.
- **2026-10-28:** Sterne, Issues, Pull Requests, Releases und das README sind das Vertrauensbild; eine eigene Bewertung ist nicht nötig. Wenn nicht: Der Katalog braucht ein zusätzliches Feld.
- **2026-10-28:** Die Aktion einer Station lässt sich als Datei (Skill, Skript, Anweisung) im Repository darstellen; das Board führt sie nicht automatisch aus. Wenn nicht: Die Station braucht eine andere Form.
- **2026-10-28:** Die vorhandenen Inhalte (elf Stationen, vier Vorhaben) lassen sich umziehen; die alte Ablage bleibt. Wenn nicht: Der Umzug wird je Ding nachgeholt.
- **2026-10-28:** Die vorhandene Oberfläche lässt sich aus der zusammengesetzten Form weiterbetreiben; die neuen Flächen (Anzeige, Bibliothek, Katalog) kommen dazu. Wenn nicht: Die Oberfläche wird geteilt.
- **2026-10-28:** Die Vertrauten haben GitHub-Konten und lassen sich einem Team hinzufügen. Wenn nicht: Die private Werkstatt bleibt beim Auftraggeber.
- **2026-10-28:** Der öffentliche Katalog wird von der Organisation kuratiert; Besucher schlagen per PR vor. Wenn nicht: Jeder müsste eigene Repositories anlegen; der Katalog wäre offener, aber unkuratiert.
- **2026-10-28:** Der Zwischenspeicher ist wegwerfbar. Wenn nicht: Er braucht eine Sicherung.
