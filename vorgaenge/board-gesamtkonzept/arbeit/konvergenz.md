---
etage: Umsetzung
status: fertig
offene_fragen: 0
stand: 2026-10-28
---

# Konvergenz — Board-Plugin gegen Architektur

## Prüfgrundlage

- Datum: 2026-10-28.
- Architektur: `/workspace/projekte/board/vorgaenge/board-gesamtkonzept/architektur.md` (Stand 2026-10-28, vollständig gelesen).
- README des Produkts: `/workspace/projekte/board-plugin/README.md` (vollständig gelesen).
- Produkt: `/workspace/projekte/board-plugin`, HEAD `52b054b99a2236e197bfb2090957e459c6d35dcf` (Kurzform `52b054b`), Paketfassung `0.1.0`. Geprüft wurde der Arbeitsbaum, nicht nur der Commit: `git status --porcelain` nennt 42 uncommittete Änderungen und neue Dateien.
- Nicht gelesen: `arbeit/plan.md`, `arbeit/fortschritt.md`, `arbeit/pruefung.md`, `arbeit/schritte/`. Geprüft wurde gegen die Architektur, nicht gegen die Schrittliste.

## Suite-Ergebnis

`node --test` in `/workspace/projekte/board-plugin` am 2026-10-28:

- 40 Tests, 40 bestanden, 0 fehlgeschlagen, 0 übersprungen, 0 offen.
- Laufzeit: 5,1 s.

Die Suite ist grün. Sie prüft die folgenden Abweichungen nicht; ein grüner Lauf ist deshalb kein Nachweis der Architekturtreue.

## Produktabweichungen (18)

1. **Fassungen werden nicht gelesen, nur angezeigt.**
   Ort: `komposition.js`, `stationenVon` und `workflowZusammensetzen` (ca. 117–273); `index.js`, `board.json`/`workflow.json`-Schreibwege.
   Grund: `workflow.json` und `board.json` tragen `fassung`, aber die Komposition liest den aktuellen Arbeitsbaum der Repositories. Es gibt kein `git show`/`git checkout` auf die genannte Fassung. Eine neue Stationsfassung wirkt damit sofort auf bestehende Workflows; die Architektur verlangt, dass sie bestehende Workflows nicht ändert und der Workflow-Verantwortliche sie bewusst nachzieht (Architektur 117, verworfener Weg 165). Eine Route zum bewussten Nachziehen einer Fassung fehlt.

2. **Dateien lassen sich in Fassung 2 nicht speichern.**
   Ort: `POST /board/doc/schreiben` (`index.js:3607–3624`); `dateiBasis` (`index.js:1156–1162`).
   Grund: Die Route liest mit `liesBoard()` und schreibt über `ordnerVon()` in die alte Ablage (`REPO/vorgaenge/...`). Ein Vorhaben der Fassung 2 liegt aber in seinem eigenen Repository; die alte Ablage kennt es nicht, die Route antwortet mit 404. `dateiBasis` bildet den Pfad relativ zu `REPO`, liegt der Vorhaben-Ordner außerhalb, entsteht `../` und keine Basis. Die Architektur verlangt, dass die Änderung im Vorhaben-Repository landet (Architektur 19, 33); die README verspricht dasselbe (README 124–126).

3. **`POST /board/vorhaben` legt kein eigenes Vorhaben-Repository an.**
   Ort: `index.js:5328–5363`.
   Grund: Die Route hat keinen Zweig für Fassung 2. Sie liest die alte Ablage; dort gibt es in einem Fassung-2-Board keinen Workflow, die Route antwortet mit 400. Ein eigenes Vorhaben-Repository entsteht nicht; lägen zufällig alte Dateien im Board-Repository, würde sie nach `REPO/vorgaenge/...` schreiben. Eine Schreibrechtsprüfung fehlt. Die Architektur bestimmt jedes Vorhaben als eigenes Repository (Architektur 12, 33) und verlangt, dass das Plugin Schreibvorgänge ins richtige Repository führt (Architektur 39).

4. **Sichtbarkeit kommt aus einer lokalen Karte und dem Ordnernamen, nicht von GitHub.**
   Ort: `rechte.js:79–103` (`sichtbarkeitVon`); `README.md:140`; gelesen wird `$DSH_HOME/board/sichtbarkeit.json` und zusätzlich eine `sichtbarkeit.json` im Elternordner.
   Grund: Zuerst entscheidet die lokale Karte; ohne Eintrag gelten `vorhaben-*` und `board-team` pauschal als privat, alles andere als öffentlich. Die GitHub-Metadaten (`private`) und die GitHub-Berechtigungen werden dafür nicht befragt. Ein öffentliches Vorhaben namens `vorhaben-probeprojekt` wäre ohne die Karte privat; die öffentlich-privat-Prüfung und der Katalog hängen an dieser lokalen Einordnung. Die Architektur macht GitHub zum Tor für Sichtbarkeit und Rechte (Architektur 129–130); `sichtbarkeit.json` steht nicht in der Liste der lokalen Dateien (Architektur 109).

5. **Schreibrecht hängt am Token und an erreichbarem GitHub.**
   Ort: `rechte.js:462–472` (`darfSchreiben`); `index.js:2212–2216` (`darfSchnell`) und `index.js:2193–2204` (`darfFuerBoard`).
   Grund: `darfSchreiben` liefert für jedes Repository mit Remote ohne Token `false`; mit Token fragt es GitHub und liefert bei einem Fehler ebenfalls `false`. `/board/data` setzt dann `darf: false` und sperrt die Handlungen. Die Architektur sagt, der Token sei optional; ohne Token blieben Lesen, Klonen und Schreiben über Git möglich (Architektur 134), und offline würden Schreibvorgänge lokal committet und beim nächsten Abgleich geschickt (Architektur 144). Ein vorhandener SSH-Zugang und lokales Schreiben werden so nicht genutzt.

6. **Drei geforderte Board-Routen fehlen.**
   Ort: Routentabelle `index.js` (ab ca. 2455).
   Grund: `/board/board`, `/board/board/verbinden` und `/board/workflow` sind nicht registriert. Vorhanden sind unter anderem `/board/data`, `/board/boards`, `/board/boards/wechsel`, `/board/workflow/station` und `/board/workflow/verbinden`; die verlangten Pfade heißen aber anders. Die Architektur führt diese drei Routen ausdrücklich als neue Schnittstelle (Architektur 105).

7. **Der Umzug liegt nur als CLI vor; die Personen-/Host-Zuordnung ist pauschal.**
   Ort: `werkzeuge/umzug2.mjs` (Aufruf über `node`, keine Route); `stationPlan` 246–266, `personenEintrag` 599–603, `vorhabenPlan` 344–370; `/board/umzug` fehlt.
   Grund: Die Architektur verlangt die Route `/board/umzug` (Architektur 105). Das Werkzeug nimmt genau einen `--host` und setzt jeden Stations-Host auf diesen Namen; nur die Kennung `p1` wird ersetzt, weitere Personen der alten Ablage werden nicht zugeordnet. Die Architektur verlangt, dass der Umzug Kennungen den GitHub-Namen zuordnet (Architektur 78).

8. **Repository-Themen `dsh-board-*` werden nicht gesetzt.**
   Ort: `POST /board/github/veroeffentlichen` (`index.js:3389–3405`) ruft `erstelleRepo` ohne `themen`; `github.js:480–502` könnte Themen setzen. Auch der Umzug setzt keine.
   Grund: Die Architektur nennt die Themen `dsh-board-station`, `dsh-board-workflow`, `dsh-board-vorhaben`, `dsh-board-board` und `dsh-board-katalog` (Architektur 113). Kein Weg im Produkt setzt sie; angelegte Repositories tragen sie nicht.

9. **Die Katalogantwort enthält keine Beispiele und gibt `geprüft` nicht weiter.**
   Ort: `katalog.js:142–185` (`eintragLesen` sammelt kein `beispiele`); `katalog.js:276–383` (`sammleKatalog` gibt `eintraege`, `hinweis`, `stand`, `luecken` zurück, nicht `geprueft`); `index.js:2552–2563`; `client.js:6773–6919`.
   Grund: Die Architektur verlangt Beispiele an jedem Katalogeintrag (Architektur 37) und `geprüft` als Eintrag mit Person und Datum (Architektur 136). `katalog.json` wird zwar mit `geprueft` gelesen, das Feld wird aber nicht in die Antwort übernommen; Beispiele werden weder gesammelt noch angezeigt.

10. **Die Bibliothekseinträge tragen nicht die geforderten Angaben.**
    Ort: `client.js:6482–6679` (`bibliothekFassung2`).
    Grund: Stationskarten zeigen Host und Fassung; Workflow-Karten Autor, Fassung und Stationszahl; Vorhaben-Karten Station, Verantwortliche und Workflow-Fassung. Sterne, Beispiele, offene Fragen und der Verweis ins Repository stehen nicht am Eintrag; Repository, Beispiele, Beiträge und Fragen erscheinen erst in der geöffneten Einzelheit. Die Architektur verlangt diese Angaben an jedem Bibliothekseintrag (Architektur 20, 37).

11. **Das Vertrauensbild einer Station zeigt keine Sterne und keine Releases.**
    Ort: `GET /board/station` (`index.js:1051–1125`, `stationBild`); `client.js:5406–5511`.
    Grund: `stationBild` liefert Name, Beschreibung, Aktion, Reife, Lizenz, Tags, Fassung, Beispiele, Verwendung, Repository und README. Sterne und Releases kommen nicht vor; Beiträge und Fragen werden separat geladen. Die Architektur zählt Sterne, Issues, Pull Requests, Releases und Beispiele zum Vertrauensbild einer Station (Architektur 29).

12. **Der Katalog zählt offene Pull Requests als offene Fragen.**
    Ort: `github.js:136` (`offeneFragen: open_issues_count`); `katalog.js:349`.
    Grund: GitHub zählt in `open_issues_count` Issues und Pull Requests zusammen. Die Architektur unterscheidet Fragen (Issues) und Beiträge (Pull Requests) (Architektur 20, 23); der Katalogwert ist bei offenen Pull Requests zu hoch.

13. **Der Katalog filtert private Einträge nicht.**
    Ort: `katalog.js:308–331` liest jeden Verweis aus `katalog.json`; `rechte.js:160` (`pruefeVerweise`) liest `katalog.json` nicht.
    Grund: Ein öffentliches Hauptboard kann in `katalog.json` ein privates Repository nennen; die Katalogroute gibt den Eintrag samt lokalem Pfad und, wenn lesbar, Namen und Metadaten zurück. Die Architektur verlangt, dass ein öffentliches Ding nur auf öffentliche Dinge verweist (Architektur 131) und der öffentliche Katalog keine privaten Namen nennt (Architektur 137).

14. **Die zweite Beitrags- und Fork-Ablage ist weiter vorhanden.**
    Ort: `POST /board/beitrag`, `GET /board/beitrag`, `POST /board/beitrag/entscheiden` (`index.js:3682–3820`); `POST /board/vorhaben/forken` (`index.js:5508–5564`); `ablage.js:327–440`.
    Grund: Das Produkt führt weiterhin `beitraege/`-Dateien mit eigenen Entscheiden und legt lokale Vorhaben-Kopien als Fork an. Die Architektur verwirft die eigene Beitrags- und Fork-Ablage ausdrücklich (Architektur 161) und bestimmt Beiträge als Pull Requests, Fragen als Issues und Forks als GitHub-Forks (Architektur 133). Die Oberfläche ruft die Beitragsroute weiterhin auf, ohne Fassungsgrenze.

15. **Verweise werden als absolute lokale Pfade geschrieben.**
    Ort: `POST /board/workflow/station` (`index.js:4363`, `repo: treffer.ort`); `POST /board/workflow/verbinden` (`index.js:4538`, `repo: ort`); `POST /board/uebernehmen` (`index.js:2682–2689`).
    Grund: `workflow.json` und `board.json` enthalten danach Pfade wie `/workspace/...`. Ein anderer Leser eines öffentlichen Boards kann sie nicht auflösen; die Verweise funktionieren nur auf dem Rechner, auf dem sie entstanden sind. Die Architektur setzt das Board aus getrennten Repositories zusammen und macht öffentliche Dinge für alle lesbar (Architektur 18, 130).

16. **Veröffentlichen behandelt alle verwiesenen Repositories als eigene.**
    Ort: `POST /board/github/veroeffentlichen` (`index.js:3332–3346` sammelt alle `board.staende`; `3389–3436` legt sie unter `DeepCollaboration` an, setzt ihren Remote und schickt sie).
    Grund: Ein lokales Board verweist auch auf öffentliche Stationen und Workflows des Hauptboards. Die Route legt für alle diese Repositories ein Organisation-Repository an oder hängt das vorhandene auf die Organisation um und schickt den lokalen Stand. Die Architektur sagt: Die Station gehört ihrem Host, der Workflow seinem Verantwortlichen, das Board verweist nur (Architektur 117–120).

17. **Eine Konfliktmeldung nennt die Datei nicht.**
    Ort: `abgleich.js:165–173`; `kurz` in `abgleich.js:59–64`.
    Grund: Bei `git pull --rebase` steht `CONFLICT (content): Merge conflict in <datei>` auf stdout; gelesen wird nur stderr. `kurz` nimmt die erste Zeile, im Versuch `From <remote>`, und gibt sie als `pull --rebase: ...` zurück. Die Architektur verlangt, dass der Konflikt am betroffenen Repository angehalten wird und die Datei nennt (Architektur 147).

18. **`GET /orgs/{org}/repos` fehlt.**
    Ort: `github.js` nutzt `POST /orgs/{org}/repos` (484), aber kein `GET /orgs/{org}/repos`; `katalog.js` liest nur `katalog.json`.
    Grund: Die Architektur führt `GET /orgs/{org}/repos` unter den benutzten GitHub-Schnittstellen (Architektur 89) und beschreibt den Katalog als Leser der öffentlichen Dinge der Organisation (Architektur 73). Ohne diesen Weg findet der Katalog nur, was im kuratierten `katalog.json` steht.

## Betriebsstand (4)

Diese Punkte sind Grenzen der Prüfumgebung, nicht als Produktfehler gewertet.

1. **Kein GitHub-Token.** `$DSH_HOME/board/github.json` fehlt. Damit sind API-Schreiben (Repository anlegen, Pull Request, Issue, Fork, Teams, Veröffentlichen) und ihre Fehlerpfade nicht live prüfbar; die Suite nutzt einen nachgebildeten Server.
2. **Keine echten GitHub-Repositories und kein Zugang zur Organisation.** Hauptboard und Team-Board liegen im Prüfstand als lokale Ordner unter `/workspace/projekte/board-repos` ohne Remote; `$DSH_HOME/board/cache` und `$DSH_HOME/board/ablagen` fehlen. Die öffentliche Veröffentlichung und die echten Remotes sind nicht geprüft.
3. **Kein Browser.** Die Oberfläche wurde nur im Code gelesen; Klicks, Layout und die tatsächlich gerenderte Anzeige sind nicht geprüft.
4. **Kein Netz in der Suite.** Die Tests laufen mit lokalen Bare-Repositories und einem nachgebildeten GitHub-HTTP-Server; Rate-Limit, Offline und echte Git-Remotes sind nur nachgebildet.

## Ergebnis

Die Architektur ist nicht erfüllt: 18 Produktabweichungen, darunter feste Fassungen, Schreiben ins Vorhaben-Repository, Rechte und Sichtbarkeit sowie mehrere geforderte Schnittstellen, stehen 4 Betriebspunkten gegenüber.

## Zweiter Bauabschnitt — Konvergenz gegen architektur.md (2026-10-29)

Ein frischer Prüfer hielt das Produkt gegen die Architektur (nicht gegen die Schrittliste), einschließlich des Nachtrags „Der Marktplatz“. Neun Befunde:

1. **Sichtbarkeit aus der lokalen Karte.** Lokale Repositories ohne GitHub-Metadaten entscheiden über `sichtbarkeit.json` und die Namensregel; GitHub entscheidet nur mit Remote. — offen, begründet: ohne Netz ist die Karte die einzige Quelle; sie wird beim Umzug gefüllt und ausdrücklich gepflegt.
2. **Verweisprüfung erreichte `owner/repo` nicht.** — behoben: `pruefeVerweise` löst zusätzlich über `$DSH_HOME/board/cache/<owner>/<repo>` auf; Prüfung in `test/markt32.mjs`.
3. **Unbekannter Anlegeweg fiel still auf `selbst` zurück.** — behoben: 409 mit Meldung; Prüfung in `test/markt27.mjs`.
4. **Kein Torwächter.** — vereinbarter Schnitt; `torwart` ist nicht gebaut und nicht in der Oberfläche.
5. **Recht je Repository nicht durchgezogen.** `schreibrecht` kennt Token und Person, aber nicht SSH; Anzeige und Server rechnen mit verschiedenen Personen; Workflow-Routen prüfen nur das Board-Repository. — offen, größte Lücke, fällig im Torwächter-Schnitt.
6. **Veröffentlichen nur unter der Organisation.** `eigentuemer` bleibt dort wirkungslos; ein eigenes Board hat keinen Veröffentlichungsweg. — offen, aus der ersten Fassung.
7. **Auffinden nur für Stationen.** Workflows, Vorhaben und Boards tragen ihre Themen noch nicht. — offen, nächster Schnitt.
8. **Vertrauensbild an gefundenen Einträgen unvollständig.** Host und Fassung fehlen; die Bibliothekskarten zeigen die Zahlen nur teilweise. — offen.
9. **Formate schreiben nicht alle Formate.** `station.json`, Board-Verweise und Einstellungen entstehen in `index.js`. — offen, technische Schuld.
10. **Urheberschaft ist der Anzeigename.** Commits tragen nicht den GitHub-Login. — offen.
11. **Die Katalogsuche ist eine Seite.** Sie nimmt die 50 sternenreichsten Repositories mit dem Thema; ein frisches 0-Stern-Repository kann fehlen, und „Im Katalog zeigen“ findet dann nichts. — offen; der Leerhinweis nennt den Grund, die gezielte Suche nach `owner/repo` fehlt.

Behoben sind die Punkte 2 und 3; die übrigen stehen mit Grund in `umsetzung.md` und `arbeit/entscheidungen.md`.

## Dritte Runde — Oberfläche (2026-10-29)

Geprüft wurde die Oberfläche gegen die Erwartung „Einstellungen wie in aktuellen Apps, Theme übernehmen, zugänglicher". Sechs Befunde:

1. **Einstellungen lagen im Board.** — behoben: Abschnitt „Board“ im Einstellungsfenster (Darstellung, Akzent, Anordnung, Mausrad, Eigentümer, Identität, GitHub); der Reiter ist weg, die Board-Leiste schaltet nur noch Boards.
2. **Eigene Farben neben dem Theme.** — behoben: nur noch `--dsw-*`-Token; Personenfarben und sechs Akzentwerte bleiben Daten.
3. **Kein Ort für einen eigenen Ton.** — behoben: ein Akzent, hell/dunkel getrennt, persönlich gespeichert, nur im Board wirksam.
4. **Fokus war an Feldern und Editoren abgeschaltet.** — behoben: `outline: none` entfernt; der globale Fokusring greift.
5. **Karten waren nur mit der Maus zu öffnen; Symbolknöpfe hatten keinen Namen für die Vorlesehilfe.** — behoben: die klickbare leere Stationsfläche ist ein Knopf mit Enter/Leertaste, die übrigen Karten sind ohnehin Knöpfe; 16 Symbolknöpfe tragen `aria-label`, Meldungen `role="status"`.
6. **Die Sprache ist deutsch.** — offen; der Umbau auf das Sprachsystem des Dashboards ist die nächste Scheibe (Deutsch, Englisch, Chinesisch, dann die übrigen fünf; Arabisch mit `dir="rtl"`).

## Vierte Runde — Sprachen (2026-10-29)

1. **Die Texte waren fest deutsch.** — behoben: 548 Texte laufen durch `uebersetze(...)`; Wörterbücher liegen als `texte/<sprache>.json` und werden vom Host geliefert.
2. **Es gab keinen Sprachumschalter fürs Board.** — behoben: das Board folgt der Sprache des Dashboards; Deutsch ist dort angemeldet, Englisch und Chinesisch sind eingebaut.
3. **Eine halb übersetzte Sprache hätte leere Stellen.** — vermieden: fehlt ein Eintrag, bleibt der deutsche Ausgangstext; die Prüfung erzwingt denselben Schlüsselsatz und dieselben Platzhalter je Sprache.
4. **Fünf Sprachen fehlen noch.** — offen: Spanisch, Französisch, Russisch, Japanisch, Arabisch; Arabisch braucht zusätzlich `dir="rtl"` und gespiegelte Anordnung.
5. **Ein Rest technischer Texte bleibt deutsch.** — teils Absicht: HTTP-Status und die Aufträge an das Modell bleiben deutsch, weil sie das Verhalten steuern; sichtbare Reste werden in der nächsten Runde eingesammelt.
