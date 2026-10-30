# Entscheidungen

## 2026-10-28 — Neuer Bauplan, alte Arbeit archiviert

Die alte Arbeit (16 Schritte, 2026-10-26) liegt in `arbeit-vorher-2026-10-26/`. Der neue Bauplan (24 Schritte) ist ein neuer Lauf, weil die Architektur von einem gemeinsamen Repository auf getrennte Repositories je Station, Workflow, Vorhaben und Board gewechselt ist.

**Folge:** Der alte Stand bleibt lesbar; die neue `arbeit/` beginnt bei Schritt 01. Die alte Prüfsuite bleibt Bestand und muss grün bleiben.

## 2026-10-28 — Die alte Ablage bleibt unangetastet

Während des Baus bleibt die Ablage der Fassung 1 (das laufende Board) unverändert. Die neue Form läuft auf einer Prüffläche; erst Schritt 13 zieht die vorhandenen Inhalte um.

**Folge:** Das laufende Board ist in jedem Schritt benutzbar; die ersten Schritte ändern keine seiner Daten.

## 2026-10-28 — Prüfablage unter `/tmp`

Die Prüfablage der ersten Scheibe liegt unter `/tmp/board-scheibe1/` und wird von der Prüfung angelegt und wieder entfernt.

**Folge:** Keine Testdaten im öffentlichen Board; die Ablage ist wegwerfbar.

## 2026-10-28 — Schritt 01 bleibt additiv

Schritt 01 legt `repos.js` und `formate/` neu an und ändert `ablage.js`, `index.js`, `client.js` und die alten Prüfungen nicht.

**Folge:** Die alte Prüfsuite bleibt grün; die neue Form steht daneben.

## 2026-10-28 — Das Ziel wird weitergeführt

Das vorhandene Ziel (`goal-39c38a9a…`) war nach dem alten Bau blockiert (Neustart nötig). Es wird für den neuen Bauplan weitergeführt, nicht neu angelegt; die alte Begründung bleibt als Geschichte.

**Folge:** Die Rundenzahl läuft weiter; der neue Bauplan hat 72 Runden Obergrenze.

## 2026-10-28 — Der Neustart bleibt beim Auftraggeber

Die Schritte 01–02 sind über die Prüfungen belegbar. Die Fläche (ab Schritt 03) und die Abnahme (Schritt 23) brauchen einen Neustart des Harness; den macht der Auftraggeber, nicht die Sitzung.

**Folge:** Der Bau kann bis dahin laufen; die Abnahme wird am lebenden Board nachgeholt.

## 2026-10-28 — Angehalten vor dem ersten Schreiben

Der Arbeiter für Schritt 01 wurde gestoppt, bevor er eine Datei geschrieben hat; das Produkt (`/workspace/projekte/board-plugin`) ist unverändert. Das Ziel ist pausiert.

**Folge:** Kein halber Stand im Produkt; Schritt 01 beginnt beim nächsten Start neu. Der Stand liegt in `plan.md`, `fortschritt.md` und `schritte/01.md`; eine Kompaktierung des Gesprächs verliert ihn nicht.

## 2026-10-28 — Der Produkt-Verweis im Vorhaben entfällt

Der Symlink `vorgaenge/board-gesamtkonzept/produkt` auf `/workspace/projekte/board-plugin` ist entfernt. Er war nur eine lokale Abkürzung, ist nicht Teil des Vorhabens und brach die alte Prüfsuite: `test/schritt15.mjs` und `test/schritt16.mjs` kopieren das Board und lesen dabei auch den verlinkten Ordner als Datei (EISDIR).

**Folge:** Das Produkt liegt weiter unter `/workspace/projekte/board-plugin`; die alte Suite ist wieder grün (16 von 16). Am Board-Inhalt ändert sich nichts.

## 2026-10-28 — Das DSH-Profil verweist direkt auf das Produkt

Der Plugin-Link im Web-Profil (`/dsh-home/profiles/web`) zeigte auf den entfernten `produkt`-Symlink. Nach dem Neustart fehlte das Board deshalb in der Seitenleiste. Profil, Lockdatei und `node_modules/@local/dsh-board` zeigen jetzt direkt auf `/workspace/projekte/board-plugin`.

**Folge:** Kein Symlink im Vorhaben nötig; die alte Prüfsuite bleibt grün. Ein weiterer Neustart des Web-Prozesses lädt das Plugin wieder.

## 2026-10-28 — Nachbesserung 05b: Wechsel und sichtbare Aktionen

Beim Ausprobieren der ersten Scheibe konnte der Auftraggeber nicht auf das Testboard zurückwechseln: `/board/boards/wechsel` verlangte die Datei `format`, die ein Board der Fassung 2 nicht hat, und antwortete mit 409. Die Karte zeigte außerdem kein „Weitergeben“, weil `/board/data` für Fassung 2 das Feld `darf` nicht setzte — der Client blendet die Aktionen ohne `darf` aus.

**Folge:** Schritt 05b behebt beides; die Prüfung deckt den Rückwechsel und `darf` ab. Die Schritte 03 und 04 bleiben inhaltlich bestanden, die sichtbare Abnahme der ersten Scheibe hängt an 05b.

## 2026-10-28 — Der Umzug schreibt unter die Organisation

Der persönliche GitHub-Name ist nirgends eingetragen: nicht in der Identität, nicht im Profil, nicht in den bisherigen Commits. Statt einen Namen zu raten, schreibt der Umzug die öffentlichen Stationen, den Workflow und die vier Vorhaben unter die Organisation `DeepCollaboration`; die Vorhaben bleiben privat. Die Kennung `p1` wird auf diesen Host abgebildet.

**Folge:** Die Bibliothek zeigt „DeepCollaboration“ als Host und Verantwortlichen. Sobald der persönliche Name in der Identität steht, lässt sich der Host in den Repositories ändern; der Umzug stoppt ohne `--host`, statt eine Kennung zu erfinden. Rücknahme: die alte Ablage bleibt unverändert.

## 2026-10-28 — Die neuen Repositories liegen nebeneinander

Der Umzug schreibt nach `/workspace/projekte/board-repos/`; die alte Ablage `/workspace/projekte/board` bleibt unangetastet. Das Register führt die neuen Boards neben dem alten.

**Folge:** Ein Rückweg ist das Zurückverweisen des Registers auf die alte Ablage; nichts wird überschrieben.

## 2026-10-28 — GitHub-Schreibwege werden nachgebildet geprüft

Im Container gibt es keinen GitHub-Token und keinen SSH-Schlüssel. Lesende Abfragen lassen sich unangemeldet gegen `api.github.com` prüfen; Anlegen, Push, Pull Requests, Teams, Forks und Issues nicht. Diese Wege werden gegen einen nachgebildeten HTTP-Server geprüft und im Bericht als „live ungeprüft“ geführt.

**Folge:** Das Produkt kann alles, was der Bauplan verlangt; die Veröffentlichung unter der Organisation ist ein späterer Lauf mit Token. Keine Prüfung behauptet einen Live-Erfolg.

## 2026-10-28 — Das Probeprojekt entsteht in Schritt 21

Der Bauplan erwartet es schon in Schritt 13 auf dem Hauptboard. Ein echtes Probeprojekt braucht aber den Übernehmen-Weg aus Schritt 21; bis dahin zeigt das Hauptboard Stationen und Workflow.

**Folge:** Schritt 13 prüft das Hauptboard ohne Vorhaben; Schritt 21 legt das Probeprojekt an und nimmt es auf. Die Abweichung steht im Bericht.

## 2026-10-28 — Die alte Ablage wird erst nach der Abnahme archiviert

Schritt 24 baut den Schalter, der Schreiben in Fassung 1 abweist, und prüft ihn mit gesetztem Schalter. Eingeschaltet wird er erst, wenn der Auftraggeber die Abnahme bestätigt hat.

**Folge:** Das alte Board bleibt bis dahin voll benutzbar; die Abnahme kann nichts kaputt machen.

## 2026-10-28 — Verweise liegen als Geschwister

Der Umzug schreibt nackte Repo-Namen (`station-idee`, `workflow-standard`), wie es das Format vorsieht. Die Komposition löste sie relativ zum Board-Ordner auf und fand die Geschwister nicht. `komposition.js` sucht einen nackten Namen jetzt zuerst neben dem lesenden Repository; absolute und relative Pfade bleiben, wie sie sind. `test/schritt30.mjs` prüft die zusammengesetzten Boards (Hauptboard elf Schritte, Team-Board vier Vorhaben).

**Folge:** Die migrierten Boards lesen sich ohne Pfadumbau; die alten Tests bleiben grün (29 von 29).

## 2026-10-28 — Das Register wurde von Hand korrigiert

Die Route `POST /board/boards` erkannte die alte Kennung `hauptboard` und überschrieb den alten Eintrag, statt einen neuen anzulegen. Die Datei `/dsh-home/board/boards.json` wurde aus der Sicherung neu geschrieben: altes `hauptboard`, neues `hauptboard-neu` („Hauptboard (neu)“) und `team`, aktiv `hauptboard-neu`, `testboard` entfernt. Der laufende Web-Prozess hält noch seinen alten Stand im Speicher; erst der Neustart liest die Datei.

**Folge:** Nach dem Neustart zeigt die Anzeige drei Boards. Schritt 16 benennt das neue Hauptboard um und macht es zum festen; die Doppelung „Hauptboard“/„Hauptboard (neu)“ ist bis dahin sichtbar.

## 2026-10-28 — Die Konvergenzabweichungen bleiben offen

Die Konvergenzprüfung gegen `architektur.md` fand 18 Produktabweichungen. Sie werden in diesem Lauf nicht behoben: Der Bauplan schneidet sie nicht, und ohne Token und echte GitHub-Repositories wären mehrere nicht live prüfbar. Die wichtigsten sind feste Fassungen (die Komposition liest den Arbeitsbaum), das Speichern von Dateien in Fassung 2 (die Route schreibt in die alte Ablage), das Anlegen eines Vorhabens, drei fehlende Routen, absolute Pfade in neuen Verweisen und das Veröffentlichen fremder Repositories. Sie stehen mit Ort und Grund in `umsetzung.md` und `arbeit/konvergenz.md`.

**Folge:** Der Bauplan ist umgesetzt, die Architektur nicht vollständig erfüllt. Der nächste Schnitt sind die Punkte 1–3, 6, 15 und 16; ohne ihn bleibt das Produkt auf diesem Rechner benutzbar, aber nicht in allen Teilen architekturtreu.

## 2026-10-29 — Der Marktplatz

- **Die lokale Sichtbarkeitskarte bleibt.** Für Repositories ohne GitHub-Metadaten entscheidet `sichtbarkeit.json` mit der Namensregel; GitHub entscheidet, wo ein Remote bekannt ist. Folge: Ein lokales `vorhaben-*` ohne Eintrag gilt als privat; wer es öffentlich will, trägt es ausdrücklich ein. Grund: Ohne Netz kann GitHub nicht antworten.
- **Die Naht bleibt zweipletzig.** `selbst` und `attrappe` sind gebaut; `torwart` nicht. Ein gesetzter unbekannter Wert bricht mit 409 ab, statt still auf `selbst` zurückzufallen. Folge: Die Oberfläche bietet nur „selbst“; der Torwächter kommt als eigener Schnitt.
- **Veröffentlichen bleibt unter der Organisation.** Ein eigenes Board veröffentlicht nicht unter seinem Eigentümer; die Eigentümerangabe wirkt beim Anlegen von Station und Vorhaben, nicht beim Veröffentlichen. Folge: Für eigene Boards bleibt der Weg über GitHub.
- **Recht je Repository bleibt offen.** `schreibrecht` fragt Token und Person, aber nicht SSH; Anzeige und Server rechnen mit verschiedenen Personen. Folge: Wer nur über SSH schreibt, sieht den Lesemodus. Der Torwächter-Schnitt muss das lösen.
- **Geprüft wird live nur mit dem Testkonto.** Nie mit seinem Konto. Folge: Veröffentlichen und Umzug unter der Organisation bleiben nachgebildet, bis ein Zugang mit Organisationsrecht da ist.

## 2026-10-29 — Nach der ersten Benutzung

- **Kein Neuladen mehr.** Jede Handlung, die das offene Board ändert — Wechsel, Verbinden, Entfernen, Hauptboard holen, Name speichern, erstes Board, Probeprojekt übernehmen — füllt die Fläche neu, statt die Seite zu laden. Folge: Man bleibt im Board; was zum alten Board gehörte (Auswahl, Leser, Gespräch, Entwürfe, Scrollstand), ist weg. Grund: Ein Neuladen wirft in den Chat, weil die Ansicht nicht an der Adresse hängt.
- **Das Anlegen endet mit einer Bestätigung.** Sie nennt das Repository und wo die Station zu finden ist; der Knopf öffnet den Katalog mit frischem Blick. Folge: Auch wenn die Station auf einem anderen Board nur im Katalog steht, ist der Weg dorthin benannt. Grund: Die Bibliothek zeigt das Board, der Katalog den Marktplatz — das bleibt so.
- **Gefundene Dinge bleiben im Katalog.** Die Bibliothek zeigt weiter nur, was das offene Board benutzt; der Katalog trägt dafür die Herkunft am Eintrag. Folge: Wer ein Ding auf einem anderen Board sucht, geht über den Katalog. Grund: eine Fläche, eine Zuordnung; die Bibliothek ist kein zweiter Marktplatz.
- **Die Suche findet auch den Verweis.** `sucheKatalog` prüft zusätzlich `repo`. Folge: `station-…` findet das Ding in beiden Fällen — kuratiert (lokaler Pfad) und gefunden (Repository-Name). Grund: Der Bestätigungsknopf hat nur den Repository-Namen, nicht den Anzeigenamen.
- **Ein „Nein“ von GitHub ist ein Recht, kein Serverfehler.** Der Code `verweigert` antwortet mit 403 statt 502 und die Meldung nennt Eigentümer und Anmeldung. Folge: Die Seite zeigt den Grund dort, wo er gelesen wird; der Weg heraus steht daneben. Der Anlegeknopf bleibt trotzdem stehen: Ob der Token in der Organisation anlegen darf, ließe sich nur mit einem zusätzlichen Recht (`read:org`) vorab fragen; ohne das Recht verschwände der Knopf auch für Berechtigte. Die Zeile „Entsteht unter …“ und die Meldung sind der Ersatz.

## 2026-10-29 — Oberfläche

- **Die Einstellungen stehen im Einstellungsfenster des Dashboards.** Das Board behält seine Inhaltsflächen; der Reiter „Einstellungen“ fällt weg. Folge: Eine Stelle für Einstellungen, wie in aktuellen Apps; die Board-Fläche gehört ganz dem Inhalt. Grund: Menschen suchen Einstellungen dort, wo das Programm sie hat, nicht in jedem Werkzeug.
- **Das Board übernimmt das Theme, statt eigene Werte zu setzen.** Alle Farben, Flächen, Ränder, Schatten und Schriften kommen aus `--dsw-*`-Token; das gilt auch für fremde Themes, die andere Plugins anmelden. Folge: Das Board sieht in jedem Theme richtig aus, ohne eigene Theme-Pflege.
- **Ein Board-Akzent, hell und dunkel getrennt.** Er gilt nur für Hervorhebungen im Board, liegt persönlich in `$DSH_HOME/board/darstellung.json` und wird über ein Merkmal (`data-board-akzent`) gesetzt. Folge: Das Dashboard bleibt unberührt; derselbe Akzent ist in beiden Modi lesbar. Grund: Ein Akzent genügt für Wiedererkennung; ein zweiter Farbsatz wäre eine zweite Wahrheit.
- **Kein Theme-Editor im Board.** Eigene Themes entstehen dort, wo das Dashboard sie kennt (heute hell/dunkel/system, künftig über `ctx.theme`). Folge: keine doppelten Einstellungen, kein Board, das anders aussieht als der Rest.

## 2026-10-29 — Veröffentlichung

- **Lizenz MIT.** Weitergabe und Änderung sind erlaubt, ohne dass Verbesserungen zurückfließen müssen. Folge: Wer das Plugin einbaut, kann es anpassen, ohne seine Änderungen zu zeigen; die Quelle bleibt das Repository. Grund: Das Plugin hängt an DSH, nicht an einem Dienst — die Rückgabepflicht der AGPL schützt hier nichts, sie schreckt nur ab.
- **GitHub und DSH-Plugin-Markt.** Das Repository ist der Ort, an dem das Plugin lebt; der Markt ist ein Eintrag, der dorthin zeigt. Folge: zwei Stellen, an denen Menschen es finden; der Eintrag wird bei jeder Fassung mitgezogen. Grund: Wer DSH benutzt, sucht zuerst im Markt.
- **Bilder und Push kommen später.** Beides braucht ihn: Aufnahmen aus seinem Browser und einen Weg zum entfernten Rechner. Folge: `README.md` entsteht ohne Bilder, die Stellen bleiben frei; die Commits liegen bereit und werden später geschoben. Bis dahin bleibt der öffentliche Stand der alte. Grund: Der Plan läuft weiter, statt an zwei Handgriffen zu warten.
- **Kein npm-Paket.** Die Installation über GitHub genügt; ein npm-Paket wäre eine zweite Stelle, die gepflegt werden müsste.
- **Die Nachbearbeitung zählt ab 57 weiter.** Der Bauplan hat 56 Schritte; die neuen setzen fort, statt neu zu zählen. Folge: `arbeit/schritte/` bleibt eine Reihe ohne doppelte Nummern.

## 2026-10-29 — Fläche, Schrift, Abstände

- **Zoom ist zurückgestellt.** „Alles zeigen“ und der Sprung zur Station lösen das Problem, das der Zoom lösen sollte: die Karte wiederfinden. Ein Zoom auf einer freien Fläche muss alle Zeigerkoordinaten für Ziehen und Verbinden umrechnen; ohne Browser wäre das nicht prüfbar. Folge: kein Zoom, bis er ihn braucht; dann als eigener Vorgang mit Sichtprüfung.
- **Abstände bleiben ohne Skala.** Schrift und Radien sind vereinheitlicht; die Abstände (4/8/12/16) stehen als Kommentar im Stilblock, werden aber nicht über hunderte Stellen gezogen. Folge: einzelne Abstände bleiben krumm (7 px, 11 px). Grund: Jede Änderung verschiebt Layout um ein bis drei Pixel, und ohne Browser ist keine davon zu sehen — der Gewinn wäre kleiner als das Risiko.

## 2026-10-30 — Reihenfolge

- **Erst die Technik, dann das Aussehen.** Jede Runde belegt zuerst, dass die Wege tragen — Suite, Abnahme, kalter Start —; erst danach wird verschönert. Folge: Der Feinschliff beginnt später, dafür auf einem Stand, der nachweislich läuft. Grund: Ein schönes Board, das nicht lädt oder nicht schreibt, ist keine Oberfläche, sondern eine Attrappe.

## 2026-10-30 — Veröffentlichung

- **Der Store-Eintrag zeigt auf das Repository, nicht auf npm.** Ein eigener npm-Name ist dafür nicht nötig; `@local/dsh-board` bleibt der Entwicklungsname. Folge: Der Eintrag besteht aus einer YAML-Datei, die auf GitHub zeigt; eine Umbenennung des Pakets und seines Profils entfällt.
- **Zwei Commits und ein Tag `v0.1.0`.** Code und Dokumentation getrennt, damit die Geschichte lesbar bleibt; die Fassung 0.1.0 entspricht `package.json`. Folge: GitHub zeigt Lizenz, Tag und eine vollständige Startseite; die Release-Notizen stehen im CHANGELOG.
- **Die öffentlichen Texte kommen ohne Werkstatt-Bezug aus.** Interne Schritt-Nummern und der „Auftraggeber“ sind aus READMEs und `docs/` entfernt, die Prüfzahlen auf 84/50 gebracht. Folge: Wer das Repository zum ersten Mal sieht, liest eine Anleitung statt einer Werkstattnotiz.
