---
etage: Umsetzung
status: fertig
offene_fragen: 0
stand: 2026-10-26
---

# Konvergenz — Board

Geprüft wurden `architektur.md`, das Produkt unter `produkt/` und die Ablage `/workspace/projekte/board/`. Nicht gelesen wurden `arbeit/schritte/`, `arbeit/plan.md`, `arbeit/fortschritt.md` und `arbeit/pruefung.md`. Die Prüfskripte unter `produkt/test/` wurden vollständig ausgeführt: 15 von 15 enden mit Exit 0.

Die Prüfskripte belegen die **Produktform**, nicht die Architekturform. Sie schreiben mehrere der unten genannten Abweichungen ausdrücklich fest (so prüft `test/schritt08.mjs`, dass der eingesetzte Skill nach `create` wieder verschwunden ist, und `test/schritt15.mjs`, dass ohne Basis geschrieben wird). Ein grüner Lauf ist deshalb kein Beleg gegen die Architektur.

Gezählt: 17 erfüllt, 15 Abweichungen, 6 nicht prüfbar, 6 offene Annahmen.

## 1. Erfüllt

1. **Technikrahmen.** JavaScript (ESM), Node 24, DSH-Plugin-API, keine zusätzlichen Abhängigkeiten, keine Datenbank, kein eigener Prozess und kein eigener Port. — Beleg: `package.json` (`"type": "module"`, keine `dependencies`); `node --version` → `v24.22.1`; kein `createServer` in `index.js`/`client.js`. Architektur Z. 26–36.
2. **Plugin-Manifest.** Exporte `.`, `./client`, `./locale/*.json`, `./icon`; `dsh.bundle.patch` auf `./cordis.patch.yml`; `dsh.client` mit `platform: "web"`, `immediately: true` und `inject: ["@deepseek-ai/dsh-client-ui-layout"]`. — Beleg: `package.json`; `cordis.patch.yml`; Client lädt über `window.__ModuleLoader__.load({ id: '@local/dsh-board', … })` und `require('react')`. Architektur Z. 127–129.
3. **Host- und Client-Hälfte.** Host: `export const inject = ['webServer']`, `apply(ctx)`, Routen über `ctx.webServer.register({ kind: 'exact', … })`, Aufräumen über `ctx.effect`; Sitzungen über `ctx.get('sessionController')`; Modell über den Dienst des Harness. Client: eigene Seite und Sidebar-Eintrag, keine eigene Anmeldung. — Beleg: `index.js:1570–1585`, `index.js:3395–3397`, `client.js` (Ende). Architektur Z. 128–129.
4. **Ablage-Grundform.** `format` = `1`; `personen/p1.json`; `workflows/p1.json` mit `{ person, workflows }`; `skills/p1/<schritt>.md` mit `name`, `description`, `whenToUse`; `vorgaenge/<ordner>/vorhaben.json`; Dateien und `transkripte/<schritt>-<zeit>.md` mit den geforderten Frontmatter-Feldern. — Beleg: Ablage; `ablage.js:103–368`; Transkriptköpfe in `vorgaenge/board-gesamtkonzept/transkripte/`. Architektur Z. 63–77.
5. **Eigene und fremde Workflows.** `/board/data` liefert die Flächen aller Personen, die eigene zuerst; fremde Schritte werden aufgelöst und als Verweis geführt, nicht als Kopie in die eigene Workflow-Datei geschrieben. — Beleg: Tests 05 und 09; `ablage.js:155–267`. Architektur Z. 16, 71.
6. **Vorhaben am Schritt.** `/board/data` liefert alle Vorhaben mit `position`, Verantwortlichen, Dateien und Transkripten; fremde Vorhaben sind sichtbar. — Beleg: Tests 06 und 07; `ablage.js:283–318`. Architektur Z. 17.
7. **Dateien öffnen, lesen, bearbeiten.** `/board/doc`, `/board/dateien`, `/board/datei`; gelesen wird der Vorhabenordner; nicht vorhandene Dateien werden als `exists: false` gemeldet, nicht erfunden. — Beleg: Tests 03 und 07. Architektur Z. 18, 157.
8. **Fassungen.** Jede Speicherung wird als Git-Commit mit Urheber, Datum und Nachricht geführt; `/board/versionen` und `/board/version` zeigen Verlauf und zwei Stände; der Leser kann einen alten Stand als neue Fassung zurückschreiben. — Beleg: `index.js:2113–2199`; `client.js:2028–2069`; Test 15. Architektur Z. 56, 102–104, 151.
9. **Beiträge.** `/board/beitrag` legt JSON und vorgeschlagene Dateien unter `beitraege/` an; die Originaldatei bleibt unberührt; `/board/beitrag/entscheiden` nimmt ganz, gar nicht oder in Teilen an. — Beleg: Tests 11 und 12; `ablage.js:320–439`. Architektur Z. 20, 76–77, 105–106.
10. **Fork, Branch, Bridge, Rücksprung.** `/board/vorhaben/abzweigen`, `/board/vorhaben/forken`, `/board/fremd/anstellen`, `/board/vorhaben/zurueckschicken`; Herkunft und Rücksprung werden als Daten geführt und in der Oberfläche gezeigt. — Beleg: Tests 09, 13 und 14. Architektur Z. 21, 159.
11. **Git-Abgleich und Stand.** `/board/sync` holt, legt die eigene Fassung darauf und gibt weiter; gelesen wird vor dem Lesen, geschrieben nach jeder Speicherung; `/board/data.stand` nennt Zeit, `nur_lokal`, Remote und Lage; `format` wird vor Schreibvorgängen geprüft. — Beleg: `index.js:421–545`; Tests 04 und 16. Architektur Z. 22, 55, 110, 119–122.
12. **Rechteprüfung an den Schreibstellen.** Fremde Vorhaben werden bei `doc/schreiben`, `skill/lauf`, `gespraech/senden`, Verschieben, Ändern, Abzweigen und Zurückschicken mit 403 abgewiesen; Beiträge sind auch auf fremden Vorhaben erlaubt; über einen Beitrag entscheidet nur der Verantwortliche. — Beleg: Tests 06, 07, 09 und 12. Architektur Z. 59, 166–170.
13. **Kein Dienst, kein Port, keine Anmeldung.** Die Routen laufen im Harness; der Client benutzt die Sitzung des Browsers; es gibt keinen eigenen HTTP-Server. — Beleg: `inject = ['webServer']`; kein `listen`/`createServer`; `client.js` ohne Login. Architektur Z. 36, 137, 164, 179.
14. **Modell nur an zwei Stellen.** Korrektur und Überarbeitung rufen das Modell im lokalen Harness; andere Board-Routen rufen kein Modell direkt auf. — Beleg: `index.js:2208–2288`; nur `modellText` an diesen beiden Routen. Architektur Z. 138.
15. **Sitzungen und Transkripte.** Sitzungen liegen unter `$DSH_HOME/sessions`; `/board/gespraech/senden` legt eine Sitzung an und sendet; `/board/gespraech/stand` liest; `/board/skill/lauf` schreibt das Transkript mit `art`, `schritt`, `skill`, `sitzung`, `stand`, `fragen`. — Beleg: Tests 08 und 10; `index.js:1081–1170`, `2734–2800`. Architektur Z. 109, 130–131.
16. **Eigene Person und Ablage in der Konfiguration.** `board.config.json` enthält `{ "person": "p1", "repo": "/workspace/projekte/board" }`; das Profil verlinkt `@local/dsh-board` auf `produkt/`. — Beleg: `produkt/board.config.json`; `/dsh-home/profiles/web/package.json`. Architektur Z. 132–133.
17. **Gezeigt wird, was da ist.** Dateilisten kommen aus dem Ordner; ein fehlender fremder Verweis erscheint als fehlend, nicht als geratener Schritt; fehlende Dateien erscheinen nicht als Inhalt. — Beleg: Tests 07 und 09; `ablage.js:155–205`; `index.js:750–773`. Architektur Z. 157.

## 2. Abweichungen

### A1 — Der Schritt-Skill ist beim ersten Auftrag der Sitzung bereits entfernt

**Ort:** `produkt/index.js:3437–3461`: `setzeSchrittSkillEin` legt den Skill an, `steuerung.create` wird aufgerufen, danach entfernt `finally` (`index.js:3454–3455`) den Ordner; erst danach folgt `steuerung.prompt` (`index.js:3461`). Die Prüfungen `test/schritt08.mjs:365–373` und `test/schritt10.mjs:424` schreiben dieses Entfernen fest.
**Grund:** Architektur Z. 19, 58 und 130: Der Host legt den Skill eines Schritts „beim Benutzen“ unter `<host>-<schritt>` in `$DSH_HOME/skills` und startet die Sitzung; der Skill des Schritts soll ausgeführt werden. Die Architektur verlangt die Anweisung für den Lauf, nicht nur für den Moment des Anlegens.
**Wirkung:** Der echte Harness baut den Skill-Katalog im `agent/pre-step` und lädt den Skill erst beim `skill`-Tool-Aufruf. Zu diesem Zeitpunkt ist `$DSH_HOME/skills/<name>` bereits gelöscht; der Katalog enthält den Schritt-Skill nicht, und `skill` antwortet „unknown or no longer available“. Der Kern „den Skill des Schritts ausführen“ läuft damit nicht; das Transkript entsteht nicht. Die Prüfskripte sind grün, weil ihr `sessionController`-Ersatz die Datei in `create` liest, bevor sie entfernt wird.

### A2 — Routen und Daten führen `position` statt `schritt`

**Ort:**
- `/board/gespraech/senden`: liest `koerper.position` (`index.js:3414`), antwortet mit `position` (`index.js:3468`).
- `/board/gespraech/stand`: liest `frage.get('position')` (`index.js:3485`).
- `/board/skill/lauf`: liest `koerper.position` (`index.js:2752–2754`).
- `vorgaenge/<ordner>/vorhaben.json`, Feld `laeufe`: Einträge tragen `position` statt `schritt` (`index.js:2782–2788`; vorhandene Ablage z. B. `vorgaenge/board-gesamtkonzept/vorhaben.json`).
- Transkriptdatei: `transkripte/<position>-<zeit>.md` (`index.js:2775`, `transkriptStamm` `index.js:1081`).
- `/board/verlauf`: Einträge tragen `positionId` statt `schritt` (`index.js:860–863`).

**Grund:** Architektur Z. 75, 84, 88, 104–109. Die Architektur trennt „Position“ (die Kennung des Schritts, an dem das Vorhaben steht) von „Schritt“ (dem Element des Workflows); die Routen und `laeufe` nennen ausdrücklich `schritt`, das Transkript `transkripte/<schritt>-<zeit>.md`.
**Wirkung:** Ein Aufrufer, der die Architekturform benutzt, sendet `{ schritt: … }`. `/board/gespraech/senden` antwortet dann 400 „Schritt fehlt“; `/board/gespraech/stand` liefert keine Sitzung (leere `position`); `/board/skill/lauf` antwortet 404 „Station nicht gefunden“. `laeufe`-Einträge und Dateinamen tragen `position`; ein Leser, der `laeufe[].schritt` oder `transkripte/<schritt>-…` erwartet, findet die Einträge nicht. Die Produkt-eigenen Skills benutzen `position` und sind mit der Architektur an dieser Stelle nicht austauschbar.

### A3 — `/board/konfiguration` fehlt

**Ort:** In `produkt/index.js` gibt es keine Route `path: '/board/konfiguration'`; der Client ruft sie nicht auf. Ein Aufruf der Adresse antwortet 404.
**Grund:** Architektur Z. 111: `GET /board/konfiguration` → `{ person, repo, remote, format }`, Zweck „wer bin ich, wo liegt was“.
**Wirkung:** Die Seite kann Person, Ablageort, Remote und Format nicht über die vorgesehene Route beziehen. Ein nach der Architektur gebauter Client bekommt 404; die Werte sind nur verstreut über `/board/data` (dort ohne `person` und `repo`), `stand.remote` und `formatAblage` erreichbar.

### A4 — `/board/data` liefert weder `person` noch `repo`

**Ort:** `datenAntwort`, `index.js:1555–1567`: zurückgegeben werden `format`, `formatAblage`, `workflows`, `vorgaenge`, `beitraege`, `personen`, `einstellungen`, `stand` — nicht `person`, nicht `repo`.
**Grund:** Architektur Z. 99: `GET /board/data` → `{ person, repo, format, workflows, vorgaenge, beitraege, stand }`.
**Wirkung:** Ein Client nach Architektur erhält die beiden Felder nicht. Die eigene Kennung lässt sich nur aus `personen` mit `ich` ableiten, der Ablageort gar nicht. Deshalb setzt der Client die Ablage in den Aufträgen fest mit `/workspace/projekte/board/…` (`client.js:2391`, `2787`, `2805`, `2820`), statt `repo` zu benutzen; auf einem anderen Arbeitsordner zeigt der Auftrag ins Leere.

### A5 — `/board/versionen` hat eine andere Form und nennt den Urheber `autor`

**Ort:** `index.js:2113–2137`: Antwort `{ datei, versionen: [ { hash, autor, datum, nachricht } ] }`.
**Grund:** Architektur Z. 102: `GET /board/versionen` → `[ { hash, datum, urheber, nachricht } ]`.
**Wirkung:** Die Antwort ist in ein Objekt gehüllt statt eine Liste; das Feld heißt `autor` statt `urheber`. Ein Client nach Architektur findet weder die Liste an der Wurzel noch den Urheber. Die Produkt-Oberfläche liest `d.versionen` und `autor` und funktioniert nur mit dieser Form.

### A6 — `/board/verlauf` ist keine Liste und nicht auf den Weg bis zur Position begrenzt

**Ort:** `index.js:2294–2301`: Antwort `{ vorhaben, schritte: [ … ] }`; `verlaufVon` (`index.js:856–875`) bildet **alle** `board.workflow.positionen` ab, nicht nur die Stationen bis zur aktuellen Position; Einträge heißen `positionId`, `name`, `aktuell`. Zusätzlich wird `board.workflow` aus der eigenen Fläche verwendet (`index.js:2298`), auch wenn das angefragte Vorhaben einer anderen Person gehört.
**Grund:** Architektur Z. 104: `GET /board/verlauf` → `[ { schritt, dateien, transkripte } ]`, Zweck „der Weg bis zur Position“; fremde Vorhaben sollen an ihrem Schritt sichtbar sein (Z. 17).
**Wirkung:** Die Antwort ist in ein Objekt gehüllt; `schritt` fehlt; der Verlauf enthält Stationen hinter der aktuellen Position und bei verzweigten Workflows auch andere Wege. Für ein fremdes Vorhaben werden die Stationen der **eigenen** Fläche verwendet; seine Position, Dateien und Transkripte erscheinen dadurch nicht. Ein Client nach Architektur kann die Antwort nicht lesen.

### A7 — `/board/gespraech/stand` liefert `eintraege` statt `bloecke`

**Ort:** `index.js:3503–3510`: Antwort `{ session, position, skill, laeuft, eintraege, fehler }`.
**Grund:** Architektur Z. 108: `GET /board/gespraech/stand` → `{ session, bloecke }`.
**Wirkung:** Das Feld `bloecke` fehlt; ein Client nach Architektur erhält den Gesprächsinhalt nicht. Die Produkt-Oberfläche liest `eintraege`. Die zusätzlichen Felder sind unschädlich, das ersetzte Feld ist es nicht.

### A8 — Speichern ohne `basis` überschreibt ohne Prüfung

**Ort:** `index.js:1793–1811`: Die Basisprüfung läuft nur, wenn der Körper die Eigenschaft `basis` hat (`hatBasis`); fehlt sie, wird ohne Vergleich geschrieben. `test/schritt15.mjs:438` prüft ausdrücklich „ohne Basis: wird wie bisher geschrieben“.
**Grund:** Architektur Z. 101 und 121: „speichern, auf Basis prüfen“; „Eine Speicherung gilt nur, wenn die Datei seither unverändert ist; sonst antwortet er mit `{ ok: false, neu }`“.
**Wirkung:** Ein Aufrufer, der `basis` weglässt, kann einen neueren Stand still überschreiben. Die Produkt-Oberfläche sendet die Basis zwar mit, aber die Zusage hängt an der Disziplin des Clients, nicht am Host. Das widerspricht „nie still überschreiben“ (Z. 191) auf der Route.

### A9 — Beim Annehmen eines Beitrags wird dessen `basis` nicht geprüft

**Ort:** `index.js:1972–2100` (`/board/beitrag/entscheiden`). Beim Annehmen werden die vorgeschlagenen Dateien gelesen und direkt in den Vorhabenordner geschrieben (`index.js:2056–2066`); geprüft wird nur, ob der Beitrag schon entschieden ist. `beitrag.basis` wird weder gegen den aktuellen Dateistand noch gegen `HEAD` geprüft.
**Grund:** Architektur Z. 84 (`basis` ist der Commit, auf dem ein Beitrag aufsitzt), Z. 121 (Basisprüfung) und Z. 191 („Nichts wird still überschrieben. Eine Änderung sitzt auf der Fassung auf, die beim Öffnen da war; ist die nicht mehr die neueste, schreibt das Board nicht“).
**Wirkung:** Wird die Originaldatei nach dem Anlegen des Beitrags geändert und der Beitrag danach angenommen, überschreibt die Annahme den neueren Stand ohne Hinweis. Die neue Fassung zeigt die Änderung, der neuere Text ist aber verloren. Die Prüfskripte decken den Fall nicht ab: Test 15 legt einen Beitrag mit Basis an, entscheidet ihn aber nicht; Test 12 entscheidet ohne zwischenzeitliche Änderung.

### A10 — `regel: "alle"` lähmt ein Vorhaben mit mehreren Verantwortlichen

**Ort:** `index.js:1325–1331`: `if (vorhaben.regel === 'alle' && liste.length > 1) return false;`. Es gibt keine Stelle, die die Zustimmung aller einsammelt; `AKTIONEN` (`index.js:139`) enthält keine eigene Aktion dafür.
**Grund:** Architektur Z. 82 (`regel` ist `einer` oder `alle`), Z. 167 (jede Rechte-Zeile wird geprüft), Annahme 10 („einer reicht oder alle müssen“). Das Fachkonzept nennt dieselbe Regel.
**Wirkung:** Stehen zwei Verantwortliche an der aktuellen Station und ist `regel: "alle"` gesetzt, dann ist `darf` für **beide** falsch: Speichern, Entscheiden, Verschieben, Weitergeben und Zurückschicken sind allen verwehrt. Das ist nicht „alle müssen“, sondern „keiner kann“. Die Oberfläche zeigt beide als verantwortlich, bietet aber keine Handlung an.

### A11 — `zweig_von` ist ein Objekt, nicht eine Kennung oder `null`

**Ort:** `ablage.js:567` schreibt `zweig_von` unverändert; `index.js:3151–3155` legt `{ von, station, datum }` an. Vorhandene Ablage: `vorgaenge/board-gesamtkonzept-idee-zu-produkt/vorhaben.json` und `vorgaenge/board-gesamtkonzept-vidionpruefen/vorhaben.json` tragen Objekte.
**Grund:** Architektur Z. 87: „`fork_von`, `zweig_von` — Kennung des Ursprungs oder `null`“; Z. 159: der Branch trägt seinen Ursprung.
**Wirkung:** Die Form ist ersetzt: Ein Leser, der `zweig_von` als Zeichenkette oder `null` behandelt, erhält ein Objekt. Die Produkt-Oberfläche baut daraus `herkunft.zweig`; das ist intern konsistent, aber nicht die genannte Ablageform. `fork_von` ist dagegen eine Zeichenkette oder `null`.

### A12 — `wie.art` verwendet andere Werte als `manuell | skill | skript`

**Ort:** `ablage.js:485–492` (`wieDatei`): erlaubt sind `skill`, `skript`, `textfeld`, `chat`, `datei`; `manuell` wird zu `textfeld`. `index.js:130` (`WIE_ARTEN`) und `index.js:290–294` (`schritteVon`) schreiben dieselbe Umwandlung. Die Ablage führt z. B. `wie: { "art": "datei" }` und `{ "art": "textfeld" }`.
**Grund:** Architektur Z. 89: `wie` ist `{ art: "manuell" | "skill" | "skript", wert }`.
**Wirkung:** Ein Repository mit `art: "manuell"` wird beim nächsten Schreiben zu `art: "textfeld"`; der genannte Wert verschwindet. Ein Leser, der die drei genannten Werte kennt, sieht `textfeld`, `datei` oder `chat` und kann sie nicht einordnen. Die Form ist ersetzt, nicht nur ergänzt.

### A13 — `board.json` liegt als alter zweiter Stand in der Ablage, und die Skills lesen ihn

**Ort:** `/workspace/projekte/board/board.json` ist versioniert (seit Commit `f085831`, unverändert) und enthält `workflow`, `schritte`, `vorgaenge`, `personen`, `einstellungen`. Die fünf Skill-Dateien unter `skills/p1/` sagen am Ende: „Die Kennungen stehen in `/workspace/projekte/board/board.json`.“ Die Datei ist gegenüber der aktuellen Ablage veraltet: `board-gesamtkonzept-idee-zu-produkt` steht dort an `idee`, aktuell an `idee-zu-produkt`; außerdem trägt sie noch einen einzelnen `workflow`, eine flache `schritte`-Liste und `einstellungen` — den alten Stand vor der Aufteilung in `workflows/` und `$DSH_HOME`.
**Grund:** Architektur Z. 65–77: Die Ablage hat die Form `format`, `personen/`, `workflows/`, `skills/`, `vorgaenge/`, `beitraege/`; Z. 157: „Gezeigt wird, was da ist.“ `board.json` gehört nicht zu dieser Form; das Produkt liest und schreibt sie nicht (`ablage.js`, Kopfkommentar).
**Wirkung:** Die Ablage enthält eine zweite, veraltete Zustandsdatei. Die Skill-Anweisungen — also der Text, den eine Sitzung ausführen soll — verweisen ausdrücklich darauf. Ein Modell, das der Anweisung folgt, kann eine alte Positionskennung wählen und das Transkript an der falschen Station eintragen oder den Aufruf scheitern lassen. Ein Klon der Ablage trägt die Datei mit; das Produkt aktualisiert sie nicht. Die Ablage und die in ihr liegenden Anweisungen widersprechen sich.

### A14 — Personendateien fremder Personen werden geschrieben und sind über eine Route änderbar

**Ort:** `ablage.js:579–585` (`baueDateien`) schreibt **alle** `board.personen` in `personen/<kennung>.json`, nicht nur die eigene; es gibt keinen `schreibbar`-Vermerk für Personen. `index.js:2533–2553` (`/board/person/aendern`) sucht die Person allein über `koerper.id` und prüft nicht, ob sie die eigene ist.
**Grund:** Architektur Z. 163–167: „Ein Konto ist ein Name und ein Zugang. Der Name steht in der Datei dieser Person im Repository“; die Rollen werden im Board geprüft. Z. 147 überträgt denselben Gedanken auf den Schritt („nur der Host schreibt ihn“).
**Wirkung:** Ein Speichern von p1 kann die Personendatei von p2 mit dem eigenen, möglicherweise veralteten Stand überschreiben; zusätzlich kann jeder Aufrufer über `/board/person/aendern` den Namen oder die Farbe einer beliebigen anderen Person ändern und festschreiben. Das ist ein stiller Schreibzugriff auf eine fremde Datei und eine fehlende Rollenprüfung an einer Stelle, die eine benannte Ablagedatei ändert.

### A15 — Die ausgelieferte Ablage hat keinen Remote

**Ort:** `/workspace/projekte/board/.git/config`; `git remote -v` ist leer. `/board/data.stand` meldet folgerichtig `remote: null`, `lage: "kein remote"`.
**Grund:** Architektur Z. 117: „Remote: `origin`; Vorgabe ist ein privates Repository in deiner GitHub-Organisation“; Z. 145 und 205: die beiden Kopien und GitHub sind die Sicherung.
**Wirkung:** Die Ablage ist in diesem Zustand nur lokal; `git fetch`/`git push` und damit der in Z. 22 verlangte Abgleich zwischen zwei Personen sind nicht möglich. Das Produkt behandelt den Fall korrekt („kein remote“), und Test 04 belegt den Abgleich mit einem lokalen Bare-Repository; die ausgelieferte Ablage selbst erfüllt die Remote-Vorgabe aber nicht.

## 3. Nicht prüfbar

1. **Die aktuelle Oberfläche im laufenden Harness.** Der Prozess (PID 23, gestartet 07:36) hat eine ältere Board-Fassung geladen: `GET http://172.19.0.2:3080/board/data` liefert die alte Form `{ workflow, schritte, … }` statt `{ format, workflows, … }`; `/board/skills` liefert alte Felder. Das Profil verlinkt zwar `@local/dsh-board` auf `produkt/`, der laufende Prozess hat die Fassung aber nicht neu geladen. Die aktuelle Client-Seite ist deshalb nicht im Browser prüfbar; dafür wäre ein Neustart des Harness nötig.
2. **Echte Sitzung und echtes Modell.** Kein Modellaufruf wurde ausgeführt; die Prüfskripte ersetzen `sessionController`, `llm` und die Sitzungsdateien. Der Gesprächsverlauf, die Skill-Ausführung und die Transkriptentstehung sind nur mit Ersatzdiensten belegt. A1 ist aus dem Code und dem DSH-Katalogverhalten belegt, nicht in einer echten Sitzung nachgestellt.
3. **Zweiter Rechner und zweite Person.** Die echte Ablage enthält nur `p1`; die Tests legen `p2` in einer Kopie an. Ein zweiter Harness, ein zweiter Arbeitsordner und die echte Fremdsicht sind nicht vorhanden.
4. **GitHub und Netz.** Die Ablage hat keinen `origin`; ein echter Abgleich über HTTPS oder SSH, ein Push und ein Pull wurden nicht ausgeführt. Test 04 benutzt ein lokales Bare-Repository.
5. **Browser-Verhalten.** Ziehen, Layout, Markdown-Darstellung, Scrollen, Sidebar-Panel und die tatsächliche Bedienung des Client sind nicht interaktiv geprüft; geprüft wurde der Client-Code und die von ihm aufgerufenen Routen.
6. **Gleichzeitige Schreiber auf zwei Rechnern.** Die Basisprüfung ist mit zwei aufeinanderfolgenden Schreibvorgängen belegt (Test 15), nicht mit zwei echt parallelen Installationen. Der Fall „zwei Änderungen im selben Moment“ bleibt offen.

## 4. Offene Annahmen

Die folgenden Punkte sind Entscheidungen, die die Architektur offenlässt oder nur als Annahme führt; sie sind keine Abweichung von einem genannten Verhalten.

1. **Farbe.** Die Architektur nennt die Farbe eine Anzeigeregel und lässt die Frage im Fachkonzept offen (Frage 5). Das Produkt führt `farbe` an `personen` und `vorgaenge` und färbt Vorhaben und Stationen danach. Das ist eine Entscheidung, keine Ablageform der Architektur.
2. **Rücksprung.** Frage 6 des Fachkonzepts lässt offen, wer den Rücksprung ausführt und annimmt. Das Produkt lässt den aktuellen Verantwortlichen zurückschicken und trägt ihn an der Zielstation ein; der Vermerk bleibt mit `erledigt` am Vorhaben. Die Architektur lässt diese Rollenwahl offen.
3. **`eingang` und `ausgabe`.** Die Architektur sagt „sind Dateinamen“ und erlaubt zugleich „mehrere Ausgaben“. Das Produkt führt Listen (`eingang: []`, `ausgabe: ["a.md", "b.md"]`, Ordner mit `/`). Das ist eine Auslegung der mehrdeutigen Stelle, keine belegte Abweichung.
4. **`format`.** Die Architektur nennt nur den Fall „eigene Fassung älter als die Ablage → nicht schreiben“. Das Produkt verlangt Gleichheit und liefert in `/board/data` `format` (Programm) und `formatAblage` (Ablage). Welche Zahl das in der Architektur genannte `format` meint, ist dort nicht ausbuchstabiert.
5. **Modell-Überarbeitung.** Das Produkt schreibt eine vom Modell überarbeitete, vom Menschen gespeicherte Fassung der Person zu und vermerkt „Fassung vom Modell“ nur in der Commit-Nachricht (`index.js:1817–1820`; `client.js:1789`). Die Architektur sagt „die Person, die gespeichert hat, oder `Modell` mit der Person, in deren Auftrag es lief“ und lässt die Zuordnung in diesem Fall offen.
6. **Arbeitsordner.** Der Client und die Skill-Anweisungen setzen `/workspace` als Wurzel voraus (`client.js:2391`, `2787`, `2805`, `2820`; `skills/p1/*.md`). Die Architektur sagt nur „im Arbeitsordner jeder Person unter `projekte/board/`“. Auf einem anderen Arbeitsordner wären die absoluten Pfade falsch; das ist eine Annahme über die Umgebung, keine belegte Architekturform.

## Offene Fragen

Keine.
