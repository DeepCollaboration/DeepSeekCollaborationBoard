---
etage: Architektur
status: in Arbeit
offene_fragen: 0
stand: 2026-10-26
---

# Architektur — Board

## Das Produkt

Das Board ist ein Plugin für den DeepSeek Harness. Auf einer Seite in der Seitenleiste zeigt es die Workflows und Vorhaben aller beteiligten Personen: die Schritte mit ihrem Host, die Vorhaben an ihrem Schritt, die Dateien, die dort entstanden sind, ihre Fassungen und die Gespräche. Die gemeinsame Ablage ist ein Git-Repository, das jede Person vollständig in ihrem Arbeitsbereich hat; das Plugin gleicht es vor dem Lesen und nach jedem Schreiben ab. Es gibt keinen Dienst dazwischen, keine Datenbank und keine Anmeldung. Jede Person arbeitet in ihrem eigenen Harness mit ihrem Modell; das Board startet die Sitzungen und die Skills des Schritts lokal. Rechte prüft das Board; die Dateien und Fassungen sind die Wahrheit.

Fertig ist das Produkt, wenn es das kann:

- Eigene und fremde Workflows zeigen: Schritte mit Host, Eingang, Ausgang, Art und Skill; fremde Schritte als Verweis, nicht als Kopie.
- Vorhaben an ihrem Schritt zeigen, mit Verantwortlichen, Dateien und Transkripten; fremde Vorhaben sind sichtbar.
- Dateien öffnen, lesen, bearbeiten; Fassungen mit Urheber vergleichen; einen alten Stand als neue Fassung zurückholen; nur zeigen, was da ist.
- Am Schritt ein Gespräch führen, das im eigenen Harness läuft; den Skill des Schritts ausführen; das Transkript als Datei ablegen.
- Beiträge anderer als eigene Datei sehen; als Verantwortlicher ganz, gar nicht oder in Teilen annehmen; nie still überschreiben.
- Fork, Branch, Bridge und Rücksprung als Daten führen und sichtbar machen.
- Den Stand über Git abgleichen; sagen, wie frisch er ist; ohne Dienst, Port oder Anmeldung.

## Techstack

Der Stack ist der des Harness und des vorhandenen Plugins. Er bleibt klein: eine Sprache, eine Laufzeit, ein Prozess, Dateien statt Datenbank.

| Teil | Wahl | Grund | Verworfen |
| --- | --- | --- | --- |
| Sprache | JavaScript (ESM) | Der Harness lädt das Plugin ohne Übersetzung; der vorhandene Code ist so geschrieben; ein Fremder kann ihn lesen. | TypeScript (Übersetzungsschritt und mehr Werkzeug), Python (nicht die Sprache des Harness). |
| Laufzeit | Node.js 24 — die Host-Hälfte im Harness-Prozess, die Client-Hälfte im Browser des Harness | Beides ist da; kein zweiter Prozess. | Eigener Node-Prozess (Dienst und Port), Deno oder Bun (nicht installiert). |
| Framework | DSH-Plugin-API: Host mit `ctx.webServer.register`, Client als DSH-Client-Plugin mit React aus dem Harness | Routen, Oberfläche und Anmeldung kommen vom Harness; das Board baut keine eigene Web-Schicht. | Express oder Fastify (eigener Server), ein eigenes Frontend (Build und eigene Anmeldung). |
| Bibliotheken | keine zusätzlichen; Node `fs`, `path`, `child_process`, das `git`-Kommando, ein eigener kleiner Markdown-Leser | Wenig Abhängigkeiten, nichts zu aktualisieren; der Leser ist klein und schon da. | markdown-it, simple-git und ein SQLite-Treiber — drei Abhängigkeiten für Aufgaben, die der Harness oder Git schon lösen. |
| Datenhaltung | Dateien in einem Git-Repository: Zustand als JSON, Inhalte und Transkripte als Markdown, Fassungen als Commits | Lesbar, mitnehmbar, jede Speicherung mit Urheber; kein Schema, keine Datenbank. | SQLite (zweite Wahrheit, keine Diffs), JSON ohne Git (keine Geschichte), CRDT (binär). |
| Protokolle | Git über HTTPS oder SSH; HTTP/JSON zwischen Seite und Host; die Sitzungs-API des Harness; das Dateisystem für Ablage und Skills | Jede Schicht spricht, was schon da ist; kein neues Protokoll, kein offener Port. | WebSocket für einen Live-Stand (braucht einen Dienst), ein REST-Dienst auf eigenem Port, WebRTC. |
| Betrieb | Ein Prozess: der Harness; kein eigener Port, kein Dienst; Ablage auf beiden Rechnern, Remote in der GitHub-Organisation | Nichts muss dauerhaft laufen oder offenstehen; Sicherung sind die Kopien und das Remote. | Eigener Dienst (Prozess, Port, Konten), Docker-Sidecar (kein Docker-Zugang), VPS (Anbieter). |

Zum Verständnis, in je einem Satz:

- **Git** — ein Ordner, dessen Geschichte mitgespeichert wird; jede Speicherung ist eine Fassung (Commit) mit Urheber und Zeit.
- **JSON** — ein Textformat für Daten, das ein Editor lesen kann; **Markdown** — Text mit einfachen Auszeichnungen.
- **Plugin** — ein Modul, das der Harness lädt; die Host-Hälfte läuft im Harness, die Client-Hälfte im Browser.
- **React** — die Oberflächenbibliothek, die der Harness mitbringt; das Board baut keine eigene.
- **HTTP/JSON** — die Seite fragt den Host über Adressen wie `/board/data` und bekommt JSON zurück.

## Bausteine

Die Bausteine sind die Teile des fertigen Produkts. Jeder hängt von dem ab, was unter ihm steht; was er nach außen gibt, ist seine Schnittstelle.

| Baustein | Was er tut | Hängt ab von | Gibt nach außen |
| --- | --- | --- | --- |
| **Board-Seite** (Client-Plugin) | Zeigt Workflows, Schritte, Vorhaben, Dateien, Leser, Gespräch und Beiträge; nimmt Klicks, Ziehen und Eingaben. | Den Routen des Board-Hosts; React und der Oberfläche des Harness. | Die Seite im Browser; keine eigene Ablage, keine eigene Anmeldung. |
| **Board-Host** (Host-Plugin) | Liest und schreibt die lokale Ablage; prüft Rechte; führt den Abgleich aus; startet Sitzungen; holt Transkripte; ruft das Modell für Korrektur und Überarbeitung. | Node, Git, den Diensten des Harness (`webServer`, `sessionController`, Modell), der Ablage. | HTTP/JSON-Routen unter `/board/*`; Sitzungen; Commits. |
| **Ablage** (Repository) | Hält Personen, Workflows, Schritte, Vorhaben, Beiträge, Dateien, Transkripte und ihre Fassungen. | Git; dem Arbeitsordner jeder Person. | Dateien und Commits; ein Klon ist eine vollständige Kopie. |
| **Abgleich** | Holt den fremden Stand, legt die eigene Fassung darauf und gibt sie weiter; zeigt, was nur lokal liegt. | Git, dem Remote, der Ablage. | Einen frischen Stand; Commits im Remote. |
| **Fassungen** | Führt jede Speicherung als Commit mit Urheber; vergleicht zwei Stände; stellt einen Stand als neue Fassung wieder her. | Der Ablage und Git. | Verlauf, Vergleich, Zurücksetzen. |
| **Beiträge** | Nimmt Vorschläge als eigene Dateien an; zeigt sie am Schritt; lässt den Verantwortlichen ganz, gar nicht oder in Teilen annehmen; schreibt die Übernahme als neue Fassung. | Der Ablage, der Rechteprüfung, den Fassungen und der Oberfläche. | Beitragsdateien, einen Entscheidungsvermerk, eine neue Fassung. |
| **Schritt-Skills** | Hält die Skill-Datei eines Schritts in der Ablage; legt sie beim Benutzen unter ihrem Namen in `$DSH_HOME/skills`; räumt sie auf. | Der Ablage, `$DSH_HOME/skills`, dem Board-Host. | Eine installierte Skill-Datei, die eine Sitzung ausführen kann. |
| **Rechteprüfung** | Prüft vor jeder Aktion die Rolle (Host, Verantwortlicher, Beitragender) und bietet nur Erlaubtes an. | Der Ablage (Personen, Workflows, Vorhaben) und der eigenen Person. | Erlaubte Aktionen; keine technische Sperre. |

## Schnittstellen

### Ablage im Repository

Das Repository liegt im Arbeitsordner jeder Person unter `projekte/board/`; sein Inhalt ist die gemeinsame Ablage. Seine Form ist die Schnittstelle zwischen den Installationen.

| Pfad | Form | Inhalt |
| --- | --- | --- |
| `format` | Textdatei mit einer ganzen Zahl | Fassung der Ablage. Der Host vergleicht sie mit seiner eigenen und schreibt nicht, wenn er älter ist. |
| `personen/<kennung>.json` | JSON `{ id, name }` | Wer mitspielt; die Kennung ist der Dateiname. |
| `workflows/<kennung>.json` | JSON `{ person, workflows: [...] }` | Die Workflows einer Person. Ein Schritt trägt `id`, `name`, `host`, `eingang`, `ausgabe`, `wie`, `skill`; ein fremder Schritt steht als `{ ref: "<person>/<workflow>/<schritt>" }`. |
| `skills/<person>/<schritt>.md` | Markdown mit Frontmatter `name`, `description`, `whenToUse` | Die Anweisung des Schritts. |
| `vorgaenge/<ordner>/vorhaben.json` | JSON `{ id, name, position, verantwortliche, regel, rueckkehr, laeufe, fork_von, zweig_von }` | Der Stand des Vorhabens. |
| `vorgaenge/<ordner>/<datei>.md` | Markdown | Die Inhalte. Der Dateiname folgt dem Schritt; der Schritt kann ihn überschreiben. |
| `vorgaenge/<ordner>/transkripte/<schritt>-<zeit>.md` | Markdown mit Frontmatter `art`, `schritt`, `skill`, `sitzung`, `stand`, `fragen` | Das Gespräch eines Laufs. |
| `beitraege/<vorhaben>/<beitrag>.json` | JSON `{ id, vorhaben, schritt, von, seit, basis, notiz, dateien, entschieden }` | Der Vorschlag. |
| `beitraege/<vorhaben>/<beitrag>/<datei>.md` | Markdown | Die vorgeschlagene Fassung; fehlt sie, ist der Beitrag nur eine Notiz. |

Die Felder im Einzelnen:

- `position` — die Kennung des Schritts, an dem das Vorhaben steht.
- `verantwortliche` — Schritt → Liste von Personenkennungen; `regel` ist `einer` oder `alle`.
- `rueckkehr` — Liste `{ von, nach, grund, seit, erledigt }`.
- `basis` — der Commit, auf dem ein Beitrag oder eine Änderung aufsitzt.
- `dateien` — Liste `{ name, datei }`; die vorgeschlagene Fassung eines Beitrags.
- `entschieden` — `null` oder `{ von, am, art: "angenommen" | "abgelehnt", teile }`; `teile` nennt, was übernommen wurde — ohne Angabe alles.
- `fork_von`, `zweig_von` — Kennung des Ursprungs oder `null`.
- `laeufe` — Liste `{ schritt, skill, datei, sitzung, datum }`.
- Schritt: `eingang` und `ausgabe` sind Dateinamen; `wie` ist `{ art: "manuell" | "skill" | "skript", wert }`; `skill` ist der Pfad zur Skill-Datei oder `null`.

Ein Schritt erzeugt `‹name›.md`; bei `A → B` zählt `B`; der Schritt kann eigene Namen und mehrere Ausgaben eintragen.

### HTTP-Routen

Die Seite spricht mit dem Host über feste Adressen. Der Host antwortet mit JSON; Fehler tragen `{ error }` und einen Status.

| Route | Methode | Form | Zweck |
| --- | --- | --- | --- |
| `/board/data` | GET | → `{ person, repo, format, workflows, vorgaenge, beitraege, stand }` | alles, was die Seite braucht |
| `/board/doc` | GET | `?vorhaben=&datei=` → `{ name, exists, text, basis }` | eine Datei lesen |
| `/board/doc/schreiben` | POST | `{ vorhaben, datei, text, basis }` → `{ ok, fassung }` oder `{ ok: false, neu }` | speichern, auf Basis prüfen |
| `/board/versionen` | GET | `?vorhaben=&datei=` → `[ { hash, datum, urheber, nachricht } ]` | Fassungen einer Datei |
| `/board/version` | GET | `?vorhaben=&datei=&hash=` → `{ alt, neu }` | zwei Stände vergleichen |
| `/board/verlauf` | GET | `?vorhaben=` → `[ { schritt, dateien, transkripte } ]` | der Weg bis zur Position |
| `/board/beitrag` | POST | `{ vorhaben, schritt, notiz, dateien }` → `{ ok, beitrag }` | Beitrag anlegen |
| `/board/beitrag/entscheiden` | POST | `{ beitrag, art: "annehmen" oder "ablehnen", teile? }` → `{ ok }` | annehmen, ablehnen, Teile |
| `/board/gespraech/senden` | POST | `{ vorhaben, schritt, text }` → `{ session }` | Sitzung anlegen und senden |
| `/board/gespraech/stand` | GET | `?vorhaben=&schritt=` → `{ session, bloecke }` | Gespräch lesen |
| `/board/skill/lauf` | POST | `{ vorhaben, schritt, session }` → `{ datei }` | Transkript festhalten |
| `/board/sync` | POST | `{}` → `{ geholt, gegeben, lage }` | Abgleich auslösen |
| `/board/konfiguration` | GET | → `{ person, repo, remote, format }` | wer bin ich, wo liegt was |

`stand` in `/board/data` nennt, wann zuletzt abgeglichen wurde und wie viele Fassungen nur lokal liegen. Die Routen der Oberfläche für Schritte, Positionen, Vorhaben, Layout und Einstellungen gehören zu derselben Schnittstelle: JSON im Körper, JSON zurück, Fehler als `{ error }` mit Status.

### Git

- **Remote**: `origin`; Vorgabe ist ein privates Repository in deiner GitHub-Organisation. Ein eigener Git-Ort, etwa Forgejo, oder ein Git-Ordner hinter einem privaten Netz tritt an dieselbe Adresse; der Umzug ist eine Zeile in der Git-Konfiguration.
- **Zweig**: `main`. Jede Person hat einen Klon unter `projekte/board/`; jede Speicherung ist ein Commit, der weitergegeben wird.
- **Befehle**: `git fetch`, `git pull --rebase`, `git add -A`, `git commit -m "<was>"`, `git push`; für den Leser `git log` und `git diff`.
- **Urheber**: `-c user.name=<person>` und `-c user.email=<person>@board`; schreibt das Modell, lautet der Name `Modell (<person>)`. Jeder Commit trägt genau einen Urheber.
- **Basisprüfung**: Der Host gibt beim Öffnen den Commit der Datei mit (`basis`). Eine Speicherung gilt nur, wenn die Datei seither unverändert ist; sonst antwortet er mit `{ ok: false, neu }` und dem neuen Stand.
- **Fassungsnummer**: Der Host liest `format`; ist seine eigene Fassung älter als die in der Ablage, schreibt er nicht und sagt es.
- **Forks und Branches sind Daten, keine Git-Zweige.** `fork_von` und `zweig_von` stehen im Vorhaben; geschrieben wird immer auf `main`. Die Begriffe aus dem Fachkonzept werden also nicht auf Git-Zweige abgebildet — sonst müsste jede Person Git bedienen.

### Harness

- **Plugin-Manifest**: `package.json` mit `"type": "module"` und den Exporten `.` (Host), `./client`, `./locale/*`, `./icon`; im Feld `dsh` der Bundle-Patch `./cordis.patch.yml` und für den Client `{ platform: "web", immediately: true, inject: ["@deepseek-ai/dsh-client-ui-layout"] }`.
- **Host-Hälfte**: `export const inject = ['webServer']; export function apply(ctx) { ... }`; Routen über `ctx.webServer.register({ kind: 'exact', path, handler })`; Sitzungen über `ctx.get('sessionController')` mit `create` und `prompt`; das Modell über den Dienst des Harness; `ctx.effect` räumt auf.
- **Client-Hälfte**: `client.js` mit `window.__ModuleLoader__.load({ id: '@local/dsh-board', factory(require) { ... } })`; React kommt über `require('react')`; die Seite holt ihre Daten von den Host-Routen und benutzt die Sitzung des Browsers. Keine eigene Anmeldung.
- **Skills**: `$DSH_HOME/skills/<name>/SKILL.md` mit Frontmatter `name`, `description`, `whenToUse`. Der Host legt den Skill eines Schritts unter `<host>-<schritt>` dorthin und startet die Sitzung im Arbeitsbereich der Person.
- **Sitzungen**: `$DSH_HOME/sessions`; der Host liest die Sitzungsdatei und schreibt daraus das Transkript in die Ablage.
- **Eigene Person und Ablage**: Beides steht in der Konfiguration des Plugins (`plugins/dsh-board/board.config.json`), nicht im Repository: `{ person, repo }`. Der Remote steht in `.git/config`.
- **Installation**: je Person dieselbe Plugin-Fassung, geladen über das Profil (`dsh plugin` oder `cordis.patch.yml`). Zwei verschiedene Fassungen erkennen sich an `format` und schreiben nicht gegeneinander.

### Nach außen

- **Git-Remote**: GitHub über HTTPS oder SSH; nur ausgehend. Kein Port, kein Zertifikat, kein Tunnel.
- **Modell**: Der Harness ruft es mit dem Schlüssel der Person. Das Board selbst ruft kein Modell außer der Korrektur und der Überarbeitung im Leser — beides läuft im lokalen Harness.
- **Sonst nichts**: keine Datenbank, kein Nachrichtendienst, keine Föderation.

## Daten und Besitz

Die Frage aus dem Fachkonzept — wo liegen die Dateien, und gibt es einen Eigentümer — ist damit beantwortet: Die Dateien liegen in einem Repository, das beide vollständig haben; kein Server hält sie allein. Einen Eigentümer im Sinne von „dem gehört die Datei“ gibt es nicht; es gibt drei Zuständigkeiten.

**Wo die Dateien liegen.** In einem Git-Repository im Arbeitsordner jeder Person. Der Remote ist die Vorgabe: ein privates Repository in deiner GitHub-Organisation. Er ist eine Adresse — ein eigener Git-Ort oder ein Git-Ordner hinter einem privaten Netz tritt an dieselbe Stelle, ohne dass sich am Board etwas ändert. Das ist der Punkt, an dem die Architektur dir einen Anbieter zumutet: die Dateien liegen bei GitHub. Die beiden Kopien und die freie Adresse sind die Rückversicherung.

**Der Schritt gehört seinem Host.** Er steht in dessen Workflow-Datei, und nur der Host schreibt ihn. Wer ihn verwendet, verweist darauf; die Fassung steht in der Geschichte.

**Das Vorhaben gehört seinem Weg.** Sein Ordner hält den Stand, die Dateien und die Transkripte. Die Vollmacht über die Originaldatei liegt beim letzten Verantwortlichen und wechselt mit dem Schritt; das steht im Vorhaben, nicht als feste Rolle am Schritt. Ein Abschnitt kann mehrere Verantwortliche tragen; dann steht ihre Regel dabei.

**Die Fassung gehört ihrem Urheber.** Jede Speicherung trägt einen Namen: die Person, die gespeichert hat, oder „Modell“ mit der Person, in deren Auftrag es lief. Der Leser zeigt, wer geändert hat; der Vergleich zweier Fassungen zeigt, was.

**Ein Vorhaben mitnehmen heißt: das Repository klonen.** Sein Ordner ist vollständig, die Fassungen kommen mit. Es gibt keinen Export und kein Format, das nur das Board liest.

**Alles im Board ist geteilt.** Auch Transkripte und Notizen eines Vorhabens. Jedes Vorhaben auf diesem Board sieht die andere Person; wer eines nicht teilen will, kann es nicht auf dieses Board legen — dafür bräuchte es ein eigenes Board. Die Grenze ist der Ordner: Was nicht geteilt gehört, bleibt außerhalb, `wissen/` zum Beispiel.

**Gezeigt wird, was da ist.** Eine Datei, wenn sie im Ordner liegt; ein Schritt, wenn er in der Workflow-Datei steht; ein Beitrag, wenn seine Datei da ist. Die Ablage macht das leicht — es gibt nichts zu erfinden.

**Fork, Branch, Bridge und Rücksprung.** Ein Fork trägt seine Herkunft (`fork_von`), ein Branch seinen Ursprung (`zweig_von`), eine Bridge ist ein Schritt mehr im eigenen Workflow, ein Rücksprung ein Vermerk am Vorhaben mit dem Schritt, zu dem er zurückgeht. Sie sind Teil des Modells, nicht der Git-Zweige.

## Rechte und Sicherheit

**Ein Konto ist ein Name und ein Zugang.**
Der Name steht in der Datei dieser Person im Repository; der Zugang ist ein Schlüssel oder ein Token zum Repository und gehört zu einem Konto. Angemeldet ist, wer an seinem Rechner sitzt; im Board gibt es keine Anmeldung.

**Die Rollen gelten, geprüft im Board.**
Jede Zeile aus den Rechten des Fachkonzepts ist im Board eine Prüfung vor der Aktion: sehen, beitragen, annehmen, ausführen, abschließen, weitergeben, gestalten, forken, abgeben. Was nicht erlaubt ist, wird nicht angeboten.

**Technisch erzwungen wird es nicht.**
Beide haben Schreibrecht am Repository; wer am Board vorbei schreibt, kann es. Der Verlauf zeigt, wer es war.

Grund: Für zwei Menschen, die einander den Schlüssel geben, wäre eine zweite Gewalt — Prüfungen im Repository, ein Server, ein Rechteverwalter — viel Maschine für wenig Schutz.

Kostet: Vertrauen ist die Grundlage. Wer sich nicht daran hält, kann am Board vorbei ändern; sichtbar wird es, verhindert nicht.

**Fremde Schritte sind fremde Anweisungen.**
Wer einen fremden Schritt verwendet, führt aus, was der Host geschrieben hat. Das ist die Absicht — und der Grund, nur Schritte von Menschen zu nehmen, denen man vertraut.

**Kein offener Port.**
Nichts hört zu. Die einzige Tür ist das Repository, verschlüsselt über HTTPS oder SSH. Kein Zertifikat, kein Tunnel, keine Firewall-Regel — und in dieser Kiste wäre dafür auch nichts vorgesehen: kein Zugriff auf Docker, kein SSH-Server.

**Was das Board nicht schützt.**
Den Menschen am Rechner, einen gestohlenen Schlüssel, ein gekapertes Konto. Die Kopien sind Sicherung, nicht Zugangsschutz.

## Ausfälle und Betrieb

**Kein Netz, GitHub nicht erreichbar.** Die Arbeit geht an der Kopie weiter; die Fassungen liegen lokal und warten; das Board zeigt, dass sie noch nicht gegeben sind. Der nächste Abgleich holt nach.

**Der andere Rechner ist aus.** Ohne Wirkung. Nur das Repository muss erreichbar sein — anders als bei einem Dienst, der auf einem der beiden Rechner läuft.

**Beide ändern dasselbe.** Nichts wird still überschrieben. Eine Änderung sitzt auf der Fassung auf, die beim Öffnen da war; ist die nicht mehr die neueste, schreibt das Board nicht, sondern zeigt den neuen Stand, behält den eigenen Text und bietet ihn als Beitrag an. Zeilen, die beide geändert haben, werden nicht automatisch zusammengeführt; beide Texte stehen da.

Grund: Ein stiller letzter Schreiber verliert Arbeit, und der Verlust wäre unsichtbar — genau das, was die Fassungen verhindern sollen.

Kostet: In dem seltenen Fall muss ein Mensch entscheiden: übernehmen, als Beitrag wiederholen oder verwerfen.

**Der Abgleich geht schief.** Die lokale Fassung bleibt stehen; das Board versucht es erneut und sagt es, wenn es nicht geht. Jede Speicherung ist ganz oder gar nicht — es bleibt kein halber Stand.

**Ein Host ändert oder entfernt einen Schritt.** Ein fremdes Vorhaben, das auf dem Schritt steht, merkt es beim nächsten Abgleich. Die Fassungen bleiben; das Board zeigt, dass der Schritt fehlt, statt einen zu raten.

**Zwei Programmfassungen.** Der Host liest `format`; ist seine Fassung älter als die Ablage, schreibt er nicht und sagt es. Kein stiller Unterschied zwischen zwei Rechnern.

**Ein Fehler im Programm.** Die Daten sind Dateien und Fassungen; sie bleiben lesbar und mit Git wiederherstellbar. Ein falscher Stand wird durch eine neue Fassung zurückgenommen, nicht durch Löschen.

**Betriebsaufwand.** Die Einrichtung — Repository, zwei Konten, Schlüssel, Programm, Kopie — ist einmalig; im Betrieb läuft nichts: kein Prozess, kein Port, kein Zertifikat, kein Abgleichsdienst. Gesichert ist der Stand an drei Stellen: die beiden Kopien und GitHub; ein Wiederaufbau ist ein Klonen.

**Kosten.** Jede Person zahlt ihre Modellaufrufe — auch die Korrektur und die Überarbeitung im Leser, die im lokalen Harness laufen. Ein fremder Schritt läuft mit dem Modell dessen, der ihn benutzt.

**Was wegfällt.** Die Live-Sicht — der fremde Stand erscheint beim Abgleich, nicht im selben Augenblick —, die eine Stelle, an der der Stand allein liegt, und eine Instanz, die Rechte erzwingt. Das ist der Preis dafür, dass nichts dauerhaft laufen muss.

## Verworfene Wege

**Ein gemeinsamer Dienst mit Konten.** Er würde Konten, Anmeldung, Datenbank, Dateispeicher, einen offenen Zugang, Sicherung und Updates bedeuten. Was er zusätzlich bringt — Live-Sicht und erzwungene Rechte —, wiegt die Last für zwei Menschen mit eigenem Harness nicht auf; und in dieser Kiste ist für einen Dienst nach außen nichts vorgesehen. Wieder aufnehmen, wenn jemand ohne Harness mitmachen soll oder Rechte erzwungen werden müssen.

**Ein VPN im Plugin.** Ein Plugin kann einen vorhandenen VPN-Client starten und seine Konfiguration schreiben; den Tunnel bauen kann es nicht. Im Container fehlen Netzrechte und TUN-Gerät, und ein Plugin kann sie sich nicht geben. Ein Userspace-Client wie Tailscale käme ohne TUN aus, braucht aber einen Kontrollserver und einen Schlüssel — das ist Betrieb, nicht ein Plugin. Ein echtes privates Netz gehört auf den Wirt oder auf einen eigenen Server; es ändert den Ort, nicht das Board.

**Live-Zusammenarbeit mit CRDT.** Automerge oder Yjs — CRDTs, Verfahren, die Änderungen ohne Server zusammenführen — halten Änderungen als eigene, oft binäre Daten; Dateien und Fassungen wären nicht mehr das, was das Board zeigt, und das Zusammenführen wäre ein Mechanismus, den niemand von euch prüfen kann. Das Fachkonzept verlangt sichtbare Dateien und Fassungen mit Urheber.

**Fremde Schritte nur anzeigen, nicht benutzen.** Dann bliebe das Produkt halb: sichtbar, aber nicht verwendbar. Ohne den Skill beim Schritt könnte ihn nur sein Host ausführen — das hieße, an dessen Rechner zu arbeiten, und dafür bräuchte es den Dienst.

**Der letzte Schreiber gewinnt — oder einer sperrt die Datei.** Ein stiller letzter Stand verliert Arbeit, ohne dass es jemand merkt; eine Sperre hält den anderen auf, obwohl die beiden selten gleichzeitig arbeiten. Beides ist schlechter als der kurze Moment, in dem einer entscheiden muss.

**Ein geteilter Ordner (Syncthing, Nextcloud).** Er gleicht Dateien ab, aber nicht Geschichte: zwei Fassungen derselben Datei werden zu Konfliktkopien, der Urheber fehlt, Rechte gibt es nicht. Das Board hätte keine verlässliche Grundlage.

**Das Board auf einem Solid Pod.** Die Pod-Entscheidung ist ausdrücklich offen und gehört in eine eigene Sitzung; das Board würde von einer ungeklärten Grundlage abhängen. Ist die Entscheidung gefallen, lässt sich prüfen, ob der Pod die Ablage übernimmt; die Dateien wären dann woanders, das Board bliebe.

**Das Harness des einen für beide öffnen.** Der Gast säße an fremden Dateien, fremden Schlüsseln und fremdem Kontingent; eine Trennung gäbe es nicht. Es widerspricht dem, dass jeder in seiner Installation arbeitet.

**Föderation zwischen Boards.** Im Fachkonzept nicht verlangt; die Entscheidung schließt Föderation zwischen Harnessen aus. Zwei Menschen brauchen kein Netz aus Boards.

**Weiter eine einzelne Datei, von Hand kopiert.** Kein gemeinsamer Ort, keine Fassungen mit Urheber, stilles Überschreiben. Das ist der Zustand ohne gemeinsamen Ort.

**Git von Hand bedienen.** Die Ablage ist Git, die Menschen bedienen es nicht: Wer Konflikte auflösen muss, arbeitet an der Mechanik statt am Vorhaben. Das Board übernimmt holen, festhalten, geben.

## Annahmen

Jede mit Datum und dem Satz, was passiert, wenn sie falsch ist.

- **2026-10-25 — Annahme 1: Die zweite Person arbeitet in ihrem eigenen Harness auf ihrem Rechner.** Die Bedingung ist bestätigt: Mitmachen heißt eigener Harness. Offen ist, ob die zweite Person ihn aufsetzt; das Treffen steht aus. *Wenn falsch:* Ohne Harness gibt es keine Board-Seite und keinen Skill-Lauf; dann braucht es den Dienst mit Konten und Browser — eine andere Grundform.
- **2026-10-25 — Annahme 2: Beide halten sich an die Rollen, auch wenn das Board sie nicht erzwingt.** *Wenn falsch:* Rechte müssen erzwungen werden — Dienst mit Konten, nicht dieses Repository.
- **2026-10-25 — Annahme 3: Zwei Menschen ändern dieselbe Datei nur selten im selben Moment.** *Wenn falsch:* Jede Kollision kostet eine Entscheidung; bei häufigen Kollisionen ist das Repository die falsche Grundlage.
- **2026-10-25 — Annahme 4: Ein privates Repository bei GitHub ist für die Board-Dateien in Ordnung, und beide können dort einen Zugang einrichten.** *Wenn falsch:* Ein anderer Git-Ort tritt an dieselbe Adresse, oder du richtest den Zugang für die andere Person ein; das Board bleibt.
- **2026-10-25 — Annahme 5: Transkripte und Notizen eines Vorhabens dürfen im Board liegen und sind für die andere Person sichtbar.** *Wenn falsch:* Sie müssen aus dem Board heraus oder verschlüsselt werden — eine eigene Ablage und eine eigene Entscheidung.
- **2026-10-25 — Annahme 6: Ein fremder Schritt darf seinen Skill mitbringen, und seine Anweisungen laufen im eigenen Harness.** *Wenn falsch:* Fremde Schritte sind nur sichtbar, nicht benutzbar; das Produkt verliert einen Kernpunkt.
- **2026-10-25 — Annahme 7: Ein Beitrag ist eine eigene Datei mit Notiz und, wenn nötig, einer geänderten Fassung.** Die Form ist im Fachkonzept offen (Frage 1). *Wenn falsch:* Sind es nur Kommentare, trägt die Ablage es ohne Änderung; müssen Zeilen einzeln angenommen werden, wird der Vergleich zur Arbeit — mehr Aufwand, dieselbe Ablage.
- **2026-10-25 — Annahme 8: Der Rücksprung bleibt ein Vermerk am Vorhaben und ein Zurücksetzen auf den früheren Schritt.** Wer ihn ausführt und annimmt, ist im Fachkonzept offen (Frage 6). *Wenn falsch:* Ein Feld mehr am Vorhaben; die Ablage trägt es.
- **2026-10-25 — Annahme 9: „Gilt sofort“ heißt beim nächsten Abgleich.** *Wenn falsch:* Wird eine Änderung im selben Augenblick verlangt, braucht es einen Dienst — andere Grundform.
- **2026-10-26 — Annahme 10: Ein Abschnitt kann mehrere Verantwortliche tragen; die Regel steht am Vorhaben (einer reicht oder alle müssen).** Welche Regel gilt, ist im Fachkonzept offen (Frage 2). *Wenn falsch:* Die Regel ändert sich; ein Feld, keine Bauweise.
- **2026-10-26 — Annahme 11: Ein unbeantworteter Beitrag bleibt sichtbar, bis der Verantwortliche entscheidet.** Das Verhalten für Liegengebliebenes ist im Fachkonzept offen (Frage 3). *Wenn falsch:* Es braucht eine Wartestelle — Oberfläche und ein Feld, nicht die Grundform.
- **2026-10-26 — Annahme 12: Bei gleichzeitiger Änderung geht nichts still verloren; welche Fassung gilt, ist im Fachkonzept offen (Frage 4).** *Wenn falsch:* Eine Regel mehr; die Ablage trägt beide Fassungen.

## Offene Fragen

Keine für die Bauweise. Die Architektur beantwortet die an sie gerichtete Frage nach Ort und Eigentum. Die übrigen Fragen des Fachkonzepts — Beitragsform, mehrere Verantwortliche, Liegengebliebenes, gleichzeitige Änderung, Farbe, Rücksprung — bleiben dort. Soweit sie die Bauweise berühren, stehen sie als Annahmen; die Farbe ist eine Anzeigeregel und berührt sie nicht.
