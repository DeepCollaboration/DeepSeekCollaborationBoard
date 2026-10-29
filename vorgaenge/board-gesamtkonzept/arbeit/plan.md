# Plan

## Scheiben

1. **Das zusammengesetzte Board** — vier Repositories, ein Board; lesen, zeigen, weitergeben, Stand je Repository.
2. **Anzeige, Bibliothek, Vertrauen** — Boards hinzufügen und wechseln; Stationen und Workflows in der Bibliothek; das Vertrauensbild.
3. **Der Umzug** — die alte Ablage lesen; die vorhandenen Inhalte in getrennte Repositories schreiben; öffentlich und privat trennen.
4. **GitHub und die Zugänge** — Token, Metadaten, Sterne; öffentlich und privat; das Hauptboard kommt von selbst; der Katalog.
5. **Zusammenarbeit** — Beiträge als Pull Requests, Verantwortung und Teams, Forken und Fragen.
6. **Die Einladung** — das Probeprojekt, die frische Installation, die Abnahme, die alte Ablage abnehmen.
7. **Eine fremde Station** — Eigentümer, Anmeldung und Recht, Anlegen mit der Naht, Auffinden.
8. **Ein eigenes Vorhaben** — anlegen, Dateien speichern, Fassungen lesen, Verweise, Konflikt.
9. **Katalog und Vertrauen** — zusammenführen, vollständig, sauber, Vertrauensbild.
10. **Mitarbeiten** — Beitrag als Pull Request, Verantwortung, alte Ablage weg.
11. **Abschluss** — fehlende Routen, Umzug als Route, Abnahme, frische Installation.
12. **Grundlage der Oberfläche** — vier Bausteine, ein gegliederter Client; sichtbar ändert sich nichts.
13. **Oberfläche** — erste Sitzung, Panels, Status, Schrift, Fläche, schmale Fenster, alle Texte übersetzt.
14. **Benutzung** — 16 Wege abgenommen, Befunde behoben, Handbuch auf Deutsch und Englisch.
15. **GitHub und Mitentwicklung** — MIT, Startseite in acht Sprachen, Entwicklerdoku, CI, Commits.
16. **Freigabe** — frischer Klon, Fassung 1.0.0, Endabnahme.

## Schritte

| Nr | Schritt | Ergebnis | Prüfung | Status |
| --- | --- | --- | --- | --- |
| 01 | Die vier Formate lesen | `repos.js`, `formate/station.js`, `workflow.js`, `vorhaben.js`, `board.js`; Prüfablage; `test/schritt18.mjs` | `node --test test/schritt18.mjs` und die alte Prüfsuite grün | bestanden |
| 02 | Das Board aus den Repositories zusammensetzen | `/board/data` liefert das gewohnte Bild aus der Prüfablage | `curl /board/data` nennt Karte und Stationen | bestanden |
| 03 | Die Fläche zeigt das zusammengesetzte Board | Seite zeigt Karte an der Station, Datei öffnet | Board-Seite, Testboard, Karte | bestanden |
| 04 | Weitergeben schreibt ins Vorhaben-Repository | `position` in `vorhaben.json`; nur dieses Repository | `git log -1 --stat` im Vorhaben-Repository | bestanden |
| 05 | Der Stand je Repository | Standzeile je Repository; offline sichtbar | Remote abschalten, weitergeben, wieder abgleichen | bestanden |
| 05b | Die sichtbaren Aktionen der ersten Scheibe | Wechsel auf ein Board der Fassung 2; Karte zeigt „Weitergeben“ | Board-Leiste, Karte, `/board/data` mit `darf` | bestanden |
| 06 | Die Anzeige: Boards hinzufügen, wechseln, entfernen | Board-Liste; Hauptboard fest | Testboard aufnehmen, öffnen, entfernen | bestanden |
| 07 | Die Bibliothek: Stationen, Workflows, Vorhaben | Karten der drei Arten mit Host und Fassung | Bibliothek öffnen; drei Arten | bestanden |
| 08 | Eine Station in einen Workflow stellen | Workflow-Repository schreibt `stationen`/`positionen`/`kanten` | `git log -1 --stat` im Workflow-Repository | bestanden |
| 09 | Das Vertrauensbild einer Station | Stationsseite: Host, Fassung, Beschreibung, Beispiele, Verwendung, Verweis | Station öffnen; fehlende Angaben als fehlend | bestanden |
| 10 | Einen Workflow in ein Board übernehmen | `board.json` schreibt den Verweis; Fläche baut neu | `git log -1 --stat` im Board-Repository | bestanden |
| 11 | Die alte Ablage lesen | Fassung 1 bleibt lesbar; Fassung 2 daneben | altes Board unverändert; beide Prüfungen grün | bestanden |
| 12 | Das Umzugswerkzeug | `werkzeuge/umzug2.mjs`; Bericht; neue lokale Repositories | Umzug auf einer Kopie; Bericht zählt | bestanden |
| 13 | Die vorhandenen Inhalte umziehen | Hauptboard öffentlich, Team-Board privat; vier Vorhaben privat | zwei Boards; altes Board bleibt | bestanden |
| 14 | GitHub-Client und Token | `github.js`; Token 0600; Metadaten und Sterne | öffentliches Repository zeigt Sterne; Token eintragen/löschen | bestanden |
| 15 | Öffentlich und privat | Repositories auf GitHub; Sichtbarkeit; Lesemodus; Verweisprüfung | öffentlich lesbar; privat nur mit Zugang; Lücke bei privatem Verweis | bestanden |
| 16 | Das Hauptboard kommt von selbst | frisches Profil sieht das Hauptboard ohne Einrichten | frisches Profil; Hauptboard; Entfernen fehlt | bestanden |
| 16b | Die Verweise des Hauptboards holen | Stationen, Workflow und Vorhaben aus der Organisation in den Zwischenspeicher | frisches Profil; Zwischenspeicher; Lücke bei fehlendem Remote | bestanden |
| 17 | Sterne und Katalog | Katalog mit Sternen, Suche, Verweis, Zwischenspeicher | Katalog; Suche; Neuladen aus dem Zwischenspeicher | bestanden |
| 18 | Beiträge als Pull Requests | PR lesen/anlegen; Board zeigt Stand | Test-PR; Board; zusammenführen | bestanden |
| 19 | Verantwortung und Teams | Host, Verantwortliche, Teams sichtbar; Schreibprüfung | Station, Vorhaben, Team; Schreiben abgewiesen | bestanden |
| 20 | Forken und Fragen | Fork und Issue verlinkt; Board zeigt Stand | Fork; Issue; schließen | bestanden |
| 21 | Das Probeprojekt | öffentliches Vorhaben an „Idee“; übernehmen in eigenes Board | Hauptboard; übernehmen; Original bleibt | bestanden |
| 22 | Die frische Installation | frisches Profil: Hauptboard, Identität später, eigenes Board | frisches Profil; übernehmen; eigenes Board | bestanden |
| 23 | Die Abnahme | Wege gegangen; Prüfungen; Bericht | Liste der Wege mit Ergebnis | bestanden |
| 24 | Die alte Ablage abnehmen | Fassung 1 nur noch lesbar; altes Board Archiv | neue Prüfungen; altes Board lesbar; Schreiben abgewiesen | bestanden |
| 25 | Der Eigentümer ist eine Einstellung | `einstellungen.eigentuemer` lesen/schreiben; Vorgabe angemeldeter Name, Hauptboard `DeepCollaboration` | Testboard setzen und neu laden; Hauptboard unverändert; ohne Anmeldung leer mit Grund | bestanden |
| 26 | Anmelden und dürfen | `stand` nennt Login und Token; Recht pro Person; ohne Token „nur lesen“ mit Grund | Token eintragen/entfernen; lokales Schreiben geht weiter | bestanden |
| 27 | Eine Station anlegen — mit Naht | Route `/board/station` über `anlegeweg`; öffentliches Repository, Thema, erster Stand | Anlegen über die Seite; Namenskollision; Attrappe als zweiter Weg | bestanden |
| 28 | Auffinden | Katalog nennt `kuratiert` und `gefunden`; Zusammenfassen über `owner/repo` | fremdes Repository mit Thema; nach Neuladen da; keine Doppel | bestanden |
| 29 | Ein Vorhaben anlegen | Vorhaben-Repository mit Thema, `vorhaben.json`, Workflow-Verweis mit Fassung | an „Idee“ anlegen; Karte; ohne Recht abgewiesen | bestanden |
| 30 | Dateien speichern | Änderung landet im Vorhaben-Repository statt in der alten Ablage | Datei ändern, neu laden; `git log`; `..` abgewiesen | bestanden |
| 31 | Fassungen lesen | Genannte Fassung aus der Geschichte; Nachziehen als eigene Handlung | Station ändern; alter Stand bleibt; nachziehen | bestanden |
| 32 | Verweise als `owner/repo` | Drei Schreibstellen bilden den Verweis aus dem Remote; alte Form lesbar | `owner/repo`; kein `/workspace`; zweiter Rechner löst auf | bestanden |
| 33 | Die Konfliktmeldung nennt die Datei | Abgleich nennt die betroffene Datei; übrige Repositories unberührt | Konflikt erzeugen; Meldung nennt Datei | bestanden |
| 34 | Der Katalog führt zusammen | Kuratiert und gefunden in einer Antwort; Herkunft am Eintrag | eigener und fremder Eintrag; keine Doppel | bestanden |
| 35 | Der Katalog ist vollständig | Beispiele, Prüfung, Sterne, Fragen und Beiträge getrennt gezählt | Beispiele da; PR erhöht Fragen nicht; Prüfung mit Person und Datum | bestanden |
| 36 | Der Katalog bleibt sauber | Private Namen weg; Sperrliste; direkter Verweis bleibt lesbar | privates Repository erscheint nicht; sperren; Verweis öffnet | bestanden |
| 37 | Das Vertrauensbild | Stationsseite und Karte zeigen Sterne, Releases, Fragen, Beiträge, Beispiele | Station öffnen; Karte in der Bibliothek | bestanden |
| 38 | Beitragen als Pull Request | Beitrag öffnet PR; keine lokale Beitragsdatei | PR existiert; `beitraege/` entsteht nicht | bestanden |
| 39 | Verantwortung und Veröffentlichen | Verantwortliche an Karten; Veröffentlichen nur eigene Repositories | fremde Station bleibt unberührt; Meldung nennt sie | bestanden |
| 40 | Die alte Beitragsablage entfernen | Kein Weg schreibt `beitraege/`; alte Daten lesbar | Route antwortet nicht mehr; Suite grün | bestanden |
| 41 | Die fehlenden Routen | `/board/board`, `/board/board/verbinden`, `/board/workflow` antworten | drei Aufrufe; Board bedienbar | bestanden |
| 42 | Der Umzug als Route | Umzug aus dem Board mit Zuordnung der Kennungen | auf Kopie umziehen; Bericht nennt Zuordnung | bestanden |
| 43 | Zwei Menschen, ein Board | A legt an, B findet, benutzt, forkt, stellt PR; A sieht ihn | ganzer Weg mit Testkonto und zweitem Eigentümer | bestanden |
| 44 | Die frische Installation | Frisches Profil liest Hauptboard und Gefundenes; Anlegen verlangt Anmeldung | frisches Profil; anonym suchen; Anlegen erklärt | bestanden |
| 45 | Der Board-Wechsel bleibt im Board | Wechsel füllt die Fläche neu, ohne Neuladen; Auswahl, Leser, Gespräch und Scrollstand des alten Boards fallen weg; „Board gewechselt: …“ | Wechsel zwischen zwei Boards; kein Chat; kein zweiter Klick | bestanden |
| 46 | Anlegen bestätigt und zeigt den Weg | Bestätigung mit Repository; „zu finden im Katalog“ mit Knopf; Katalog nennt Herkunft und Verweis | Station anlegen; Karte lesen; Katalog öffnen | bestanden |
| 47 | Das „Nein“ von GitHub wird erklärt | 403 nennt Eigentümer und Anmeldung; Formular zeigt „Entsteht unter …“; eigener Prüfschritt | `test/markt45.mjs`; 403 unter fremdem Eigentümer | bestanden |
| 48 | Die Einstellungen ziehen ins Dashboard | Abschnitt „Board“ im Einstellungsfenster: Akzent, Anordnung, Mausrad, Eigentümer; Board-Reiter „Einstellungen“ fällt weg | Einstellungen öffnen; Werte ändern; Fläche folgt | bestanden |
| 49 | Theme und Akzent | Board nutzt nur `--dsw-*`-Token; Akzent hell/dunkel als Merkmal, persönlich gespeichert; keine harten Farben mehr | `test/markt46.mjs`; `test/markt47.mjs`; Farbzählung im Client | bestanden |
| 50 | Lesbarkeit und Fokus | Kleinste Schriften größer, Trefferflächen, Fokus sichtbar, Felder benannt | Sichtprüfung; Client-Prüfung | bestanden |
| 51 | Die Sprache des Dashboards gilt fürs Board | Texte im Client über eine Übersetzung geführt; Wörterbuch je Sprache vom Host; Deutsch im Umschalter; Rückfall auf den Ausgangstext | `test/markt48.mjs`; `test/markt49.mjs` | bestanden |
| 52 | Englisch und Chinesisch vollständig | 472 Texte überführt, englische und chinesische Wörterbücher mit 549 Einträgen; Platzhalter und Schlüsselsatz geprüft; deutsche Sprachdatei im Umschalter | `test/markt48.mjs`; Stichproben; Suite 65/65 | bestanden |
| 53 | Acht Sprachen und RTL | Spanisch, Französisch, Russisch, Japanisch, Arabisch ergänzt; sechs Sprachen im Umschalter angemeldet; Arabisch rechtsläufig; Datum und Zahlen folgen der Sprache | `test/markt48.mjs`; `test/markt49.mjs`; Suite 65/65 | bestanden |
| 54 | Tastatur und Statusmeldungen | Die klickbare leere Stationsfläche ist ein Knopf mit Enter/Leertaste; Meldungen werden als Status angekündigt; Hinweistext übersetzt | Client-Prüfung; Suite 65/65 | bestanden |
| 55 | Namen für Symbolknöpfe | 14 Symbolknöpfe tragen `aria-label` (aus dem Titel), zwei ohne Titel benannt; keine doppelten Labels | Client-Prüfung; Suite 65/65 | bestanden |
| 56 | Identität und GitHub im Einstellungsfenster | Eigene Karten im Abschnitt „Board“; Board-Leiste schaltet nur noch Boards und legt neue an; Panel-Handler und Zustände entfernt | `test/markt47.mjs` (Karten), Suite 65/65 | bestanden |
| 57 | Oberfläche aufnehmen und Befunde festhalten | `arbeit/ui-analyse.md` mit Zeilenverweisen | jede Aussage mit Zeilenverweis | bestanden |
| 58 | Vier Bausteine herausziehen: Knopf, Feld, Panel, Meldung | Panel mit Kopf, Schließen, Escape, Fokus; Meldung mit Zustand; zuerst in Einstellungen und Stationsdetail; Einstellungen zeigen beim Laden „wird geladen“ | Suite grün; im Browser unverändert | bestanden |
| 59 | Client gliedern und kommentieren | Kopfindex, Abschnittsköpfe, ein Satz je Funktion; tote Reste weg (18 Stile, `spalten`, `tdSpalte`); Kopfkommentar auf die freie Fläche | `node --check`; Suite 65/65; eine Stelle in unter einer Minute gefunden | bestanden |
| 60 | Erste Sitzung führt zum Schreiben | Karte „Du liest mit“ mit zwei Wegen (Token, eigenes Board); verschwindet mit Schreibrecht | Hauptboard ohne Token; Karte da; Weg gehen; Karte weg | bestanden |
| 61 | Alle Panels auf ein Muster | Station, Vorhaben, Katalog, Beiträge, Fragen, Board-Liste, Leser und Chat teilen Hülle, Schließen und Tastatur; der Fokus kehrt zurück | mit Tab öffnen, Escape schließen; Fokus zurück | bestanden |
| 62 | Rückmeldungen als Status | Statusbaustein mit `aria-live`, getrennt vom Hilfetext; der Zustand steht im Zustand, nicht in der Zeichensetzung; Fehler bleibt stehen | Aktion auslösen: „fertig“ oder Fehler; nichts verschwindet ungesehen | bestanden |
| 62b | Fortschritt bei den Wegen, die Netz brauchen | die acht Netzwege setzen „läuft“ vor dem Aufruf und enden mit „fertig“ oder Fehler | `laeuft(` mindestens achtmal, jede Stelle endet; keine bleibt stehen | bestanden |
| 63 | Schrift und Maße auf eine Skala | Schrift 12/13/14/18 px, Radien 6/8/12; fester Takt für Abstände nur dokumentiert | keine Fließtextgröße unter 12 px; Browser | bestanden |
| 64 | Die Fläche erklärt sich | Satz auf der leeren Fläche, Bibliothek höhenbegrenzt, „Alles zeigen“ rollt zu den Karten | leeres Board zeigt einen Satz; Bibliothek rollt innen; Knopf findet die Karten | bestanden |
| 64b | Sprung zur Station | ein Feld in der Leiste findet Stationen und rollt zu ihrer Karte | zwölf Stationen, eine suchen, Karte im Blick | bestanden |
| 64c | Übersicht und Zoom | Knöpfe für näher, weiter und alles; die Fläche skaliert, Karten bleiben bedienbar | zoomen, alles zeigen, Station öffnen | zurückgestellt |
| 65 | Schmale Fenster und 200 % | Panels passen, Menüs kippen nicht heraus, Fläche scrollt | Fenster auf 360 px, Zoom 200 % | bestanden |
| 66 | Fehlende Übersetzungen schließen | die 17 Texte gehen durch `uebersetze`; eine Prüfung findet sichtbare Texte ohne `uebersetze` | neue Prüfung grün; acht Wörterbücher ergänzt | bestanden (zweiter Lauf) |
| 66b | Der Grund im Lesemodus spricht die Sprache | die fünf Sätze des Hosts werden über eine Tabelle übersetzt, mit Rückfall | Suite grün; englischer Grund bei englischem Dashboard | offen || 67 | Abnahmeliste schreiben | `arbeit/abnahme.md`: 16 Wege mit Erwartung und Ergebnis | Liste vollständig; von dir bestätigt | offen |
| 68 | Durchgang im Browser | Protokoll: jeder Weg bestanden oder Befund | du gehst mit; ich schreibe mit | offen |
| 69 | Befunde beheben | je Befund ein Schritt oder eine Zeile im Protokoll | Suite grün; Nachgang im Browser | offen |
| 70 | Handbuch der Benutzung | `docs/benutzung.de.md` und `docs/benutzung.en.md` mit Bildern und Fehlersuche | ein Fremder kommt ohne dich bis zur ersten eigenen Station | offen |
| 71 | Repo-Grundlagen | LICENSE (MIT), `package.json` (Zweck, Lizenz, Repository, Fehleradresse, Node 24, `npm test`), CHANGELOG, `.editorconfig`, `.gitattributes` | `npm test` grün; Metadaten vollständig | offen |
| 72 | README Englisch, Sprachen, Bilder | `README.md` englisch; `README.de.md`; kurze Fassungen zh, es, fr, ru, ja, ar; Sprachwahl oben; Bilder folgen | jede Fassung: Installation, erste Schritte, Links | offen |
| 73 | Entwicklerdoku im Repo | `docs/architektur.md`, `docs/entwicklung.md`, `docs/datenformate.md`, `docs/routen.md` | ein Fremder findet Route, Format, Platz und Test | offen |
| 74 | Code dokumentieren | Modulköpfe, Verträge, Abschnittsköpfe; Anleitung „eine Sprache hinzufügen“ | Sprache in unter einer Stunde ergänzt | offen |
| 75 | Zusammenarbeit und CI | CONTRIBUTING, SECURITY, Issue- und PR-Vorlage, CI auf Node 24 | CI grün auf GitHub; Vorlagen erscheinen | offen |
| 76 | Fassungen und Push | Commits in Scheiben, Branch, PR-Beschreibung, Repo-Beschreibung und Themen | `git log` lesbar; GitHub zeigt den aktuellen Stand | offen |
| 77 | Frischer Klon, frisches Profil | Installation und erster Start protokolliert | Hauptboard da; Lesen ohne Token; keine Fehlerseite | offen |
| 78 | Release 1.0.0 | Version, Tag, Release Notes, Lizenzangabe | Release auf GitHub; Installation mit Tag | offen |
| 79 | Endabnahme | dein Durchgang, Protokoll, Freigabe | du sagst „veröffentlichen“ | offen |

Die Schritte 57–79 stehen ausführlich in `arbeit/nachbearbeitung.md`.

## Neue Runde: Name, Oberfläche, Übersetzungen (2026-10-29, nach dem Neustart)

Diese Schritte kommen **vor** 67/68 (Durchgang durch die Oberfläche) und vor
71–79 (Veröffentlichung): Name und Aussehen ändern sich noch, ein Durchgang
durch die alte Oberfläche wäre Arbeit für die Tonne. 66b (Gründe des Servers)
geht im Übersetzungsdurchgang 86 auf.

| Nr. | Schritt | Ergebnis | Prüfung | Stand |
| --- | --- | --- | --- | --- |
| 80 | Name | sichtbar „CollaborationBoard“ statt „Board“: Titel der Seite, Abschnitt und Symbol im Dashboard, README, `description`; „Board“ bleibt der Gegenstand (ein Board), Pfade und Kennungen bleiben `board` | Suite grün; die zwei Tests, die den alten Namen festhielten, angepasst | erledigt |
| 81 | Rahmen | linke Leiste mit „Bibliothek“ und „Verlauf“, daneben das gewählte Panel fest angedockt; die Bibliothek liegt nicht mehr über der Fläche und schiebt sie nicht nach unten | Dock nicht `position: absolute`; Fläche behält ihre Höhe; Escape schließt, Fokus geht zurück; Suite grün | bestanden (zweiter Lauf) |
| 82 | Kopfzeile und Fläche | eine Kopfzeile mit drei Gruppen (Board, Titel und Zustand, Menschen und Suche), Trennern und einer Statuszeile darunter; die lange Bedienanleitung nur, solange die Fläche leer ist; die Fläche bekommt eine eigene Farbe, die Flächen der Personen einen Kopf und Rand | jede Funktion erreichbar (Board-Liste, Person bearbeiten, Hauptboard wiederholen, Warnungen), kein Text außerhalb der Übersetzung; Suite grün | offen |
| 83 | Offene Station | Stationsdetail als rechtes Panel; die Karte bleibt klein statt auf 760 px zu wachsen | Karte öffnen/schließen, Ziehen, Anlegen, Löschen unverändert; Suite grün | offen |
| 84 | Gespräch | das Gespräch steht als Reiter im rechten Stationspanel, statt als eigene Spalte mit Griff daneben | Chat öffnen/schreiben/verlaufen unverändert; Suite grün | offen |
| 85 | Übersetzungen | jede sichtbare Zeichenkette in sechs Sprachen; Servergründe (`rechte.js`) als Schlüssel statt roher deutscher Text; 66b hierin | Schlüsselmengen gleich, Platzhalter gleich, keine deutsche Zeile in en/es/fr/ru/ja/ar | offen |
| 86 | Sichtprüfung | dein Durchgang durch die neue Oberfläche; Befunde als 87 | du sagst „so bleibt es“ | offen |
