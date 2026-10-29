---
etage: Umsetzung
status: in Arbeit
offene_fragen: 0
stand: 2026-10-28
---

# Umsetzung — Board

## Erste Scheibe

Ein Board aus vier getrennten Repositories — eine Station, ein Workflow, ein Vorhaben, ein Board —, das ein Vorhaben an seiner Station zeigt, seine Dateien öffnet, es eine Station weiterschiebt und die Änderung im Vorhaben-Repository festhält. Die Station und der Workflow bleiben dabei unberührt. Das bisherige Board bleibt daneben benutzbar; die neue Form läuft auf einer Prüffläche, nicht auf seinen Daten.

Danach ist zum ersten Mal sichtbar, dass das Board nicht mehr ein Repository ist, sondern viele: Die Karte kommt aus dem Vorhaben-Repository, die Station aus ihrem, die Reihenfolge aus dem Workflow, die Liste aus dem Board. Wer das sieht, kann beurteilen, ob der Weg trägt, bevor die vorhandenen Inhalte umziehen.

## Scheiben

1. **Das zusammengesetzte Board** (Schritte 1–5): vier Repositories, ein Board; lesen, zeigen, weitergeben, Stand je Repository.
2. **Anzeige, Bibliothek, Vertrauen** (Schritte 6–10): Boards hinzufügen und wechseln; Stationen und Workflows in der Bibliothek; das Vertrauensbild einer Station.
3. **Der Umzug** (Schritte 11–13): die alte Ablage lesen; die vorhandenen Inhalte in getrennte Repositories schreiben; öffentlich und privat trennen.
4. **GitHub und die Zugänge** (Schritte 14–17): Token, Metadaten, Sterne; öffentlich und privat; das Hauptboard kommt von selbst; der Katalog.
5. **Zusammenarbeit** (Schritte 18–20): Beiträge als Pull Requests, Verantwortung und Teams, Forken und Fragen.
6. **Die Einladung** (Schritte 21–24): das Probeprojekt, die frische Installation, die Abnahme, die alte Ablage abnehmen.

## Schritte

1. Die vier Formate lesen.
2. Das Board aus den Repositories zusammensetzen.
3. Die Fläche zeigt das zusammengesetzte Board.
4. Weitergeben schreibt ins Vorhaben-Repository.
5. Der Stand je Repository.
6. Die Anzeige: Boards hinzufügen, wechseln, entfernen.
7. Die Bibliothek: Stationen, Workflows, Vorhaben.
8. Eine Station in einen Workflow stellen.
9. Das Vertrauensbild einer Station.
10. Einen Workflow in ein Board übernehmen.
11. Die alte Ablage lesen.
12. Das Umzugswerkzeug.
13. Die vorhandenen Inhalte umziehen.
14. GitHub-Client und Token.
15. Öffentlich und privat.
16. Das Hauptboard kommt von selbst.
17. Sterne und Katalog.
18. Beiträge als Pull Requests.
19. Verantwortung und Teams.
20. Forken und Fragen.
21. Das Probeprojekt.
22. Die frische Installation.
23. Die Abnahme.
24. Die alte Ablage abnehmen.

## Schritt 1 — Die vier Formate lesen

- **Scheibe:** Das zusammengesetzte Board.
- **Ziel:** Eine Station, ein Workflow, ein Vorhaben und ein Board lassen sich aus vier getrennten Ordnern lesen und prüfen.
- **Eingang:** `/workspace/projekte/board-plugin` (das vorhandene Plugin); die Formate aus `architektur.md`; eine Kopie des Vorhabens „Board Gesamtkonzept“ und der Stationen „Idee“ und „Umsetzen“ als Prüfinhalt.
- **Tun:** `repos.js` für lokale Repositories (Pfad, Art, Stand); `formate/station.js`, `formate/workflow.js`, `formate/vorhaben.js`, `formate/board.js` mit Lesen, Prüfen, Schreiben; die Prüfablage unter `/tmp/board-scheibe1/` mit vier Repositories; `test/schritt18.mjs`.
- **Ergebnis:** Vier lesbare Repositories; die neue Prüfung grün; die vorhandenen Prüfungen (schritt02–17) unverändert grün, weil die alte Ablage nicht angefasst wird.
- **Prüfung:** `node --test test/schritt18.mjs` und die vorhandenen Prüfungen laufen durch; die Prüfung liest die Station „Umsetzen“, den Workflow mit den Positionen „idee“ und „umsetzen“, das Vorhaben an „idee“ und das Board mit den drei Verweisen.
- **Stopp:** nein.

## Schritt 2 — Das Board aus den Repositories zusammensetzen

- **Scheibe:** Das zusammengesetzte Board.
- **Ziel:** `/board/data` liefert für ein Board der Fassung 2 das gewohnte Bild: Workflow, Schritte, Vorhaben, Personen, Boards.
- **Eingang:** Schritt 1; die vorhandene Form der Seite (`data.workflows`, `data.vorgaenge`, `data.personen`, `data.workflow`, `data.schritte`, `data.boards`, `data.aktiv`).
- **Tun:** Die Komposition: Board lesen, Workflow und Stationen auflösen, Vorhaben und ihre Dateien einsammeln, Personen und Farben zusammensetzen, Lücken benennen; das Register um die neue Board-Art erweitern; die Route `/board/data` liest je nach `format` die alte oder die neue Form; die alte Form bleibt unverändert.
- **Ergebnis:** Ein Testboard in der Prüfablage ist über die Seite erreichbar; `/board/data` nennt das Vorhaben an der Station „idee“, die Stationen aus ihren Repositories und die Verweise des Boards.
- **Prüfung:** Das Testboard ist aktiv; `curl -s "$DSH_WEB_URL/board/data"` enthält `vorgaenge[0].name` = „Board Gesamtkonzept“, `vorgaenge[0].position` = „idee“ und `workflows[0].schritte` mit „Idee“ und „Umsetzen“; das alte Board liefert weiter seine alte Form.
- **Stopp:** nein.

## Schritt 3 — Die Fläche zeigt das zusammengesetzte Board

- **Scheibe:** Das zusammengesetzte Board.
- **Ziel:** Die Seite zeigt das Testboard mit der Karte an der Station und öffnet die Datei des Vorhabens.
- **Eingang:** Schritt 2; die vorhandene Board-Seite.
- **Tun:** Die Board-Leiste zeigt beide Boards (alt und Test); das Testboard wird gewählt; die Karte „Board Gesamtkonzept“ steht an „Idee“; ein Klick öffnet die Datei; die Station „Umsetzen“ steht im Workflow; nichts wird erfunden.
- **Ergebnis:** Die Seite zeigt das Testboard; die Karte und die Datei sind da; das alte Board bleibt erreichbar.
- **Prüfung:** Board-Seite öffnen, Testboard wählen: Die Karte steht an „Idee“, ein Klick zeigt die Datei; das alte Board zeigt weiter seine vier Vorhaben.
- **Stopp:** nein.

## Schritt 4 — Weitergeben schreibt ins Vorhaben-Repository

- **Scheibe:** Das zusammengesetzte Board.
- **Ziel:** Ein Vorhaben eine Station weiterzugeben ändert nur sein Repository.
- **Eingang:** Schritt 3; die Route `/board/vorhaben/verschieben`.
- **Tun:** Die Route ermittelt aus dem Board und dem Vorhaben das Vorhaben-Repository, prüft die Berechtigung, schreibt `position` und `verantwortliche` in `vorhaben.json`, committet und schickt; die Station und der Workflow werden nicht angefasst; die Seite lädt das Board neu.
- **Ergebnis:** Die Karte steht an „Umsetzen“; im Vorhaben-Repository steht ein Commit, der nur `vorhaben.json` ändert; die anderen Repositories haben keinen neuen Commit.
- **Prüfung:** Karte weitergeben; `git -C /tmp/board-scheibe1/vorhaben log -1 --stat` zeigt nur `vorhaben.json`; `git -C /tmp/board-scheibe1/station log -1` und `git -C /tmp/board-scheibe1/workflow log -1` zeigen den alten Stand; die Seite zeigt die Karte an „Umsetzen“.
- **Stopp:** nein.

## Schritt 5 — Der Stand je Repository

- **Scheibe:** Das zusammengesetzte Board.
- **Ziel:** Jedes Repository sagt, ob es abgeglichen ist, was nur lokal liegt und wo ein Fehler steht.
- **Eingang:** Schritt 4; die vorhandene Standzeile.
- **Tun:** Der Abgleich je Repository (holen, ziehen, schicken); die Standzeile je Board; ein Fehler bleibt an seinem Repository; der Zwischenspeicher zeigt den letzten Stand, wenn das Netz fehlt.
- **Ergebnis:** Das Testboard zeigt je Repository den Stand; ein Schreibvorgang ohne Remote bleibt „nur lokal“; das alte Board zeigt weiter seinen einen Stand.
- **Prüfung:** Die Standzeile nennt die Repositories; das Remote des Vorhabens abschalten, weitergeben: „nur lokal“; wieder einschalten, abgleichen: „abgeglichen“; das Board bleibt in beiden Fällen sichtbar.
- **Stopp:** nein.

## Schritt 6 — Die Anzeige: Boards hinzufügen, wechseln, entfernen

- **Scheibe:** Anzeige, Bibliothek, Vertrauen.
- **Ziel:** Eine Fläche listet die Boards, nimmt ein Board auf, wechselt und entfernt es; das Hauptboard ist fest.
- **Eingang:** Schritt 5; die vorhandene Board-Leiste; das Register.
- **Tun:** Die Anzeige als Startseite: Name, Art, Besitzer, Stand, Verweise; „Board verbinden“ (Adresse oder Ordner); „Öffnen“; „Entfernen“ (nicht das Hauptboard); „Adresse teilen“; die Identität (Name, GitHub-Name, Farbe) an dieser Stelle; die Liste bleibt lokal.
- **Ergebnis:** Ein Board lässt sich mit einem lokalen Ordner oder einer Adresse aufnehmen und wieder entfernen; das Hauptboard steht fest; die Seite wechselt.
- **Prüfung:** Das Testboard aufnehmen, öffnen, entfernen; das Hauptboard bietet kein Entfernen; die Identität ändern und wieder öffnen.
- **Stopp:** nein.

## Schritt 7 — Die Bibliothek: Stationen, Workflows, Vorhaben

- **Scheibe:** Anzeige, Bibliothek, Vertrauen.
- **Ziel:** Die Bibliothek zeigt die Dinge, die das Board benutzt: Stationen, Workflows, Vorhaben, jeweils mit Host und Fassung.
- **Eingang:** Schritt 6; die vorhandene Bibliothek (Stationen des Workflows).
- **Tun:** Die Bibliothek liest die Verweise des Boards und die Stationen des Workflows; Karten für Stationen, Workflows, Vorhaben; ein Klick öffnet die Einzelheit; die vorhandene Stationsbibliothek bleibt für das alte Board.
- **Ergebnis:** Die Bibliothek des Testboards zeigt „Idee“ und „Umsetzen“ mit ihrem Host, den Workflow und das Vorhaben; das alte Board zeigt seine Stationen wie bisher.
- **Prüfung:** Bibliothek öffnen: die drei Arten sind da, jede mit Host und Fassung; ein Klick öffnet die Einzelheit; das alte Board unverändert.
- **Stopp:** nein.

## Schritt 8 — Eine Station in einen Workflow stellen

- **Scheibe:** Anzeige, Bibliothek, Vertrauen.
- **Ziel:** Eine Station aus der Bibliothek kommt als Verweis in einen Workflow; das Workflow-Repository wird geschrieben.
- **Eingang:** Schritt 7; die Route für Positionen.
- **Tun:** Die Bibliothek bietet „In den Workflow stellen“; die Route ergänzt `stationen`, `positionen` und `kanten` im Workflow-Repository, prüft die Fassung und die Berechtigung, committet; die Seite lädt das Board neu.
- **Ergebnis:** Die Station steht im Workflow und auf der Fläche; im Workflow-Repository steht ein Commit; die Stationsdatei bleibt unberührt.
- **Prüfung:** Station „Umsetzen“ in den Test-Workflow stellen; `git -C /tmp/board-scheibe1/workflow log -1 --stat` zeigt `workflow.json`; die Karte der Station erscheint; das Stations-Repository hat keinen neuen Commit.
- **Stopp:** nein.

## Schritt 9 — Das Vertrauensbild einer Station

- **Scheibe:** Anzeige, Bibliothek, Vertrauen.
- **Ziel:** Eine Station zeigt, was sie tut, von wem sie kommt, welche Fassung gilt, welche Beispiele es gibt und wo sie verwendet wird.
- **Eingang:** Schritt 8; `station.json`, `README.md`, `examples/`, Tags.
- **Tun:** Die Einzelheit: Beschreibung, Host, Fassung, Reife, Lizenz, Beispiele, Verwendung in den Workflows des Boards, Verweis ins Repository; was fehlt, steht als fehlend da, nicht als Platzhalter.
- **Ergebnis:** Die Stationsseite zeigt die vorhandenen Angaben; der Verweis öffnet das Repository; die Verwendung zählt die Workflows, die auf die Station verweisen.
- **Prüfung:** „Umsetzen“ öffnen: Host, Fassung, Beschreibung, Beispiel oder „kein Beispiel“, Verwendung; der Verweis führt zum Repository; eine Station ohne README zeigt „kein README“.
- **Stopp:** nein.

## Schritt 10 — Einen Workflow in ein Board übernehmen

- **Scheibe:** Anzeige, Bibliothek, Vertrauen.
- **Ziel:** Ein Workflow lässt sich in ein Board aufnehmen; das Board setzt ihn zusammen.
- **Eingang:** Schritt 9; die Verweise des Boards.
- **Tun:** „Workflow aufnehmen“ schreibt den Verweis in `board.json`, prüft die Fassung und die Berechtigung, committet; die Seite baut das Board aus dem neuen Workflow; das alte Board bleibt.
- **Ergebnis:** Das Testboard hat den Workflow; die Fläche zeigt seine Stationen; das Board-Repository hat einen Commit.
- **Prüfung:** Workflow aufnehmen; die Fläche zeigt die Stationen; `git -C /tmp/board-scheibe1/board log -1 --stat` zeigt `board.json`.
- **Stopp:** nein.

## Schritt 11 — Die alte Ablage lesen

- **Scheibe:** Der Umzug.
- **Ziel:** Eine Ablage der Fassung 1 bleibt lesbar, während die Fassung 2 daneben läuft.
- **Eingang:** Schritt 10; die vorhandene `ablage.js`; das laufende Board.
- **Tun:** Den vorhandenen Leseweg als Fassung-1-Zweig kapseln; die Komposition erkennt am `format`, welchen Weg sie nimmt; die alten Routen bleiben für das alte Board; die alten Prüfungen bleiben.
- **Ergebnis:** Das laufende Board funktioniert unverändert; die neue Form läuft daneben; die Prüfungen beider Formen sind grün.
- **Prüfung:** Das alte Board öffnen: vier Vorhaben, die Stationen, die Dateien; die vorhandenen Prüfungen laufen; das Testboard der Fassung 2 läuft weiter.
- **Stopp:** nein.

## Schritt 12 — Das Umzugswerkzeug

- **Scheibe:** Der Umzug.
- **Ziel:** Aus einer Ablage der Fassung 1 entstehen Stationen, ein Workflow, Vorhaben und Boards der Fassung 2 in getrennten lokalen Repositories.
- **Eingang:** Schritt 11; die vorhandenen elf Stationen, vier Vorhaben, ein Workflow; der GitHub-Name des Auftraggebers.
- **Tun:** `werkzeuge/umzug2.mjs`: liest die alte Ablage, schreibt je Station ein Repository mit `station.json`, README und Aktionsplatz, je Workflow ein Repository mit Verweisen, je Vorhaben ein Repository mit Dateien und Transkripten, je Board ein Repository mit Verweisen; ordnet `p1` dem GitHub-Namen zu; schreibt einen Bericht; die alte Ablage bleibt.
- **Ergebnis:** Ein neuer Ordner mit den lokalen Repositories; der Bericht nennt jede angelegte Station, jeden Workflow, jedes Vorhaben und jedes Board; die alte Ablage ist unverändert.
- **Prüfung:** Den Umzug auf einer Kopie laufen lassen; der Bericht zählt elf Stationen, einen Workflow, vier Vorhaben; die neue Komposition zeigt das Vorhaben „Board Gesamtkonzept“ an seiner Station; die alte Ablage hat keinen neuen Commit.
- **Stopp:** ja — der GitHub-Name muss eingetragen sein; ohne ihn stoppt der Umzug, statt eine Kennung zu erfinden.

## Schritt 13 — Die vorhandenen Inhalte umziehen

- **Scheibe:** Der Umzug.
- **Ziel:** Sein wirkliches Board läuft aus den neuen Repositories; die vier Vorhaben sind privat, die Stationen und das Probeprojekt öffentlich.
- **Eingang:** Schritt 12; die Entscheidung, was öffentlich wird; das Probeprojekt.
- **Tun:** Den Umzug auf dem wirklichen Board; die vier Vorhaben in private Repositories, die elf Stationen und den Workflow in öffentliche, das Probeprojekt als öffentliches Vorhaben, das Hauptboard als öffentliches Board, das Team-Board als privates; die Sichtbarkeit als lokale Angabe, bis GitHub sie trägt; das alte Board bleibt daneben.
- **Ergebnis:** Zwei lokale Boards: das Hauptboard mit Stationen, Workflow und Probeprojekt; das Team-Board mit den vier Vorhaben; das alte Board bleibt erreichbar.
- **Prüfung:** Das Hauptboard zeigt die Stationen und das Probeprojekt, keine der vier Vorhaben; das Team-Board zeigt die vier Vorhaben an ihren Stationen; das alte Board zeigt sie weiter; die Sichtbarkeitsangabe steht an jedem Repository.
- **Stopp:** ja — die Trennung öffentlich/privat ist eine Entscheidung über Inhalte; sie wird vor dem Lauf bestätigt.

## Schritt 14 — GitHub-Client und Token

- **Scheibe:** GitHub und die Zugänge.
- **Ziel:** Das Plugin liest Metadaten und Sterne von GitHub und kann mit einem Token schreiben, was Git nicht kann.
- **Eingang:** Schritt 13; die GitHub-REST-Schnittstelle; ein öffentliches Repository zum Prüfen.
- **Tun:** `github.js`: Repository-Metadaten, Sterne, Releases, Issues, Pull Requests, Themen, Teams, Benutzer; Rate-Limits, Zwischenspeicher, Fehler; `github.json` (0600) für den Token; die Anzeige zum Eintragen; ein nachgebildeter Server für die Prüfungen.
- **Ergebnis:** Ein öffentliches Repository zeigt seine Sterne und seine Fassung; der Token lässt sich eintragen, ändern und löschen; ohne Token bleiben die Leseabfragen.
- **Prüfung:** Ein öffentliches Repository eintragen: Sterne und letzte Fassung erscheinen; den Token eintragen: die Anzeige sagt „verbunden“; den Token löschen: die Leseabfragen bleiben, die Schreibhandlungen verschwinden.
- **Stopp:** ja — der Token wird bei GitHub angelegt und hier eingetragen; ohne ihn bleiben Anlegen und PRs manuell.

## Schritt 15 — Öffentlich und privat

- **Scheibe:** GitHub und die Zugänge.
- **Ziel:** Die lokalen Repositories werden zu GitHub-Repositories; öffentlich und privat sind durchgesetzt; ein öffentliches Ding verweist nur auf öffentliche.
- **Eingang:** Schritt 14; die Organisation; die lokalen Repositories aus Schritt 13.
- **Tun:** Repositories anlegen oder verbinden, Remotes setzen, push; Sichtbarkeit je Repository; Zugang prüfen; Lesemodus, wenn kein Schreibrecht; die Komposition prüft die Verweise und zeigt eine Lücke bei einem privaten Verweis in einem öffentlichen Ding.
- **Ergebnis:** Die öffentlichen Repositories stehen unter der Organisation; das private Team-Repository ist nur mit Zugang lesbar; ein öffentlicher Workflow mit einer privaten Station wird abgelehnt und sichtbar als Lücke benannt.
- **Prüfung:** Die öffentlichen Repositories auf GitHub öffnen; das private Repository ohne Zugang öffnen: nicht sichtbar; mit Zugang: sichtbar; einen privaten Verweis in das öffentliche Board schreiben: die Seite zeigt die Lücke, der Host weist das Schreiben ab.
- **Stopp:** ja — Organisation, Rechte und Sichtbarkeit; die vorhandenen Repositories werden angelegt und geschickt.

## Schritt 16 — Das Hauptboard kommt von selbst

- **Scheibe:** GitHub und die Zugänge.
- **Ziel:** Eine frische Installation zeigt das öffentliche Hauptboard ohne Einrichten.
- **Eingang:** Schritt 15; das öffentliche Hauptboard-Repository.
- **Tun:** Beim ersten Start das Hauptboard holen und als festes Board eintragen; lesen ohne Identität; Schreiben nur mit Recht; ohne Netz „nicht geholt“ mit Wiederholen; die Anzeige zeigt das Hauptboard fest.
- **Ergebnis:** Ein frisches Profil öffnet das Board und sieht das Hauptboard mit Stationen und Probeprojekt; kein Name, kein Token, kein Verbinden nötig; das Hauptboard lässt sich nicht entfernen.
- **Prüfung:** Ein frisches Profil starten: das Hauptboard erscheint; die Stationen und das Probeprojekt sind da; das Entfernen fehlt; ohne Netz erscheint der Hinweis und der Rest bleibt nutzbar.
- **Stopp:** ja — das öffentliche Hauptboard-Repository muss existieren und erreichbar sein.

## Schritt 17 — Sterne und Katalog

- **Scheibe:** GitHub und die Zugänge.
- **Ziel:** Der Katalog zeigt die öffentlichen Dinge mit Sternen, Suche und Verweis; die Zahlen sind zwischengespeichert.
- **Eingang:** Schritt 16; die öffentlichen Repositories; die Sterne aus Schritt 14.
- **Tun:** Den Katalog aus dem Hauptboard und den Themen der Organisation; Sterne, Fassungen, Releases, offene Fragen, Verwendung; Suche und Filter; der Verweis öffnet GitHub; der Zwischenspeicher hält die Zahlen.
- **Ergebnis:** Der Katalog listet Stationen, Workflows und Vorhaben; jede Karte zeigt Sterne, Host und Fassung; die Suche findet eine Station; der Verweis führt zu GitHub.
- **Prüfung:** Katalog öffnen: die öffentlichen Dinge sind da, mit Sternen; suchen: die Station erscheint; neu laden: die Zahlen kommen aus dem Zwischenspeicher; ein Stern auf GitHub ändert die Zahl nach dem nächsten Abruf.
- **Stopp:** nein.

## Schritt 18 — Beiträge als Pull Requests

- **Scheibe:** Zusammenarbeit.
- **Ziel:** Ein Beitrag ist ein Pull Request; das Board zeigt ihn und seinen Stand.
- **Eingang:** Schritt 17; der GitHub-Client; ein Repository mit Schreibrecht.
- **Tun:** Pull Requests lesen und anlegen; ein Beitrag an einer Station, einem Workflow oder einem Vorhaben öffnet einen PR gegen dessen Repository; das Board zeigt offene und angenommene Beiträge; der Verantwortliche führt auf GitHub zusammen; das Board liest den neuen Stand.
- **Ergebnis:** Ein Beitrag erscheint als PR am Ding; das Board verlinkt ihn; nach dem Zusammenführen zeigt das Board den neuen Stand; ein abgelehnter PR bleibt sichtbar.
- **Prüfung:** Auf einem Test-Repository einen PR anlegen; das Board zeigt ihn am Vorhaben; auf GitHub zusammenführen; das Board zeigt den neuen Stand; ohne Token steht der Verweis auf GitHub statt des Knopfs.
- **Stopp:** ja — ein zweites Konto oder ein Test-Repository mit Schreibrecht; ohne das bleibt der PR-Weg ungeprüft.

## Schritt 19 — Verantwortung und Teams

- **Scheibe:** Zusammenarbeit.
- **Ziel:** Das Board zeigt, wer eine Station hostet und wer an welcher Station eines Vorhabens verantwortlich ist.
- **Eingang:** Schritt 18; `station.json` (Host), `vorhaben.json` (Verantwortliche), die Teams der Organisation.
- **Tun:** Host je Station; Verantwortliche je Station im Vorhaben; Team und Mitglieder lesen; die Anzeige an Karte, Bibliothek und Einzelheit; die Rechteprüfung nutzt dieselbe Quelle.
- **Ergebnis:** Jede Station nennt ihren Host; jedes Vorhaben nennt je Station die Verantwortlichen; die Teams sind sichtbar; ein Schreibversuch ohne Verantwortung wird abgewiesen.
- **Prüfung:** Eine Station öffnen: Host; ein Vorhaben öffnen: Verantwortliche je Station; ein Team öffnen: Mitglieder; als Nicht-Verantwortlicher schreiben: abgewiesen.
- **Stopp:** ja — die Teams und ihre Mitglieder müssen auf GitHub bestehen.

## Schritt 20 — Forken und Fragen

- **Scheibe:** Zusammenarbeit.
- **Ziel:** Ein Fork und eine Frage sind GitHub-Handlungen; das Board verlinkt sie und zeigt ihren Stand.
- **Eingang:** Schritt 19; der GitHub-Client.
- **Tun:** Fork anlegen und verlinken; Issues lesen und anlegen; das Board zeigt offene Fragen am Ding; der Fork erscheint als eigene Kopie; die Verweise öffnen GitHub.
- **Ergebnis:** Ein Fork eines Vorhabens ist verlinkt; eine Frage steht als Issue am Ding; das Board zeigt beides, ohne eine zweite Ablage.
- **Prüfung:** Ein Vorhaben forken: der Fork ist verlinkt; eine Frage anlegen: das Issue erscheint am Vorhaben; auf GitHub schließen: das Board zeigt es geschlossen.
- **Stopp:** ja — ein Konto mit Recht, ein Fork anzulegen.

## Schritt 21 — Das Probeprojekt

- **Scheibe:** Die Einladung.
- **Ziel:** Ein öffentliches Vorhaben liegt an der ersten Station und lädt zum Mitmachen ein; jeder kann es in ein eigenes Board übernehmen.
- **Eingang:** Schritt 20; das Hauptboard; das Probeprojekt-Repository.
- **Tun:** Das Probeprojekt an „Idee“; der Einladungstext; „In mein Board übernehmen“ legt ein lokales Board mit den Stationen und einer Kopie des Vorhabens an, mit dem eigenen Namen als Host; die Kopie verweist auf das Original.
- **Ergebnis:** Das Probeprojekt ist im Hauptboard sichtbar; ein Besucher kann es übernehmen und in seinem Board bearbeiten; das Original bleibt.
- **Prüfung:** Hauptboard öffnen: das Probeprojekt an „Idee“; übernehmen: ein lokales Board mit dem Vorhaben und den Stationen; die Kopie bearbeiten: das Original bleibt unverändert.
- **Stopp:** nein.

## Schritt 22 — Die frische Installation

- **Scheibe:** Die Einladung.
- **Ziel:** Eine neue Installation erlebt das Board ohne Einrichten: Hauptboard, Stationen, Probeprojekt, Lesen; dann der eigene Name und das eigene Board.
- **Eingang:** Schritt 21; ein frisches Profil.
- **Tun:** Das Plugin in ein frisches Profil; das Hauptboard automatisch; die Anzeige; die Identität erst beim Beitreten; der Token optional; das eigene Board aus dem Hauptboard; die README mit dem Weg.
- **Ergebnis:** Ein frisches Profil sieht das Hauptboard; ohne Namen; das Übernehmen fragt nach dem Namen; das eigene Board entsteht; die Anzeige listet beide.
- **Prüfung:** Frisches Profil starten; das Hauptboard erscheint; das Probeprojekt übernehmen; der Name wird gefragt; das eigene Board steht in der Anzeige; die README beschreibt den Weg.
- **Stopp:** ja — das Plugin muss in ein frisches Profil installiert werden; das ist ein Eingriff in die Umgebung.

## Schritt 23 — Die Abnahme

- **Scheibe:** Die Einladung.
- **Ziel:** Der ganze Weg ist einmal gegangen: öffentlich sehen, prüfen, sternen, übernehmen; privat arbeiten, teilen, beitragen.
- **Eingang:** Schritte 1–22.
- **Tun:** Die Wege prüfen; die Prüfungen laufen; die README und die Dokumente sind aktuell; die alten und neuen Boards sind erreichbar; die Sterne, PRs und Teams sind sichtbar.
- **Ergebnis:** Ein Bericht mit den gegangenen Wegen, den Prüfungen und den offenen Resten; das Produkt ist benutzbar.
- **Prüfung:** Die Liste der Wege: Hauptboard ohne Einrichten; Station prüfen und sternen; Probeprojekt übernehmen; privates Team-Board öffnen; Beitrag als PR; Fork; Frage. Jeder Punkt mit seinem Ergebnis.
- **Stopp:** ja — die Abnahme durch den Auftraggeber.

## Schritt 24 — Die alte Ablage abnehmen

- **Scheibe:** Die Einladung.
- **Ziel:** Die alte Fassung 1 ist nicht mehr der Weg; das alte Board bleibt als Archiv lesbar.
- **Eingang:** Schritt 23; die alten Prüfungen; die alten Routen.
- **Tun:** Den Schreibweg der Fassung 1 abschalten, den Leseweg behalten; die alten Prüfungen, die nur den Schreibweg prüfen, entfernen oder auf den Leseweg ziehen; das alte Board aus der Anzeige nehmen oder als „Archiv“ führen; die Dokumente nachziehen.
- **Ergebnis:** Die Prüfungen der neuen Form laufen; das alte Board ist lesbar, aber nicht mehr der Weg; die Anzeige nennt es „Archiv“ oder lässt es weg.
- **Prüfung:** Die neuen Prüfungen laufen; das alte Board öffnet sich lesbar; ein Schreibversuch im alten Board wird abgewiesen; die Anzeige zeigt es als Archiv.
- **Stopp:** ja — der Auftraggeber bestätigt, dass das alte Board nicht mehr gebraucht wird.

## Zweiter Bauabschnitt — Der Marktplatz

Die erste Fassung hat das Board aus getrennten Repositories gebaut; sie steht. Dieser Abschnitt macht es offen: Fremde legen Stationen und Vorhaben unter ihrem eigenen Namen an, andere finden sie über ein Thema bei GitHub, und der Anlegeweg ist so geschnitten, dass später ein Torwächter an seine Stelle treten kann, ohne die Fläche umzubauen.

### Erste Scheibe

Eine Station unter eigenem Namen anlegen, sie öffentlich bei GitHub stehen sehen und sie ohne Zutun auf einem fremden Board wiederfinden.

Danach ist zum ersten Mal sichtbar, dass das Board nicht nur viele Repositories zusammensetzt, sondern dass fremde Arbeit von selbst hereinkommt: Der eine legt an, der andere sieht es, ohne dass jemand etwas freischaltet. Wer das sieht, kann beurteilen, ob Offenheit ohne Dienst und ohne Schlüssel im Plugin trägt.

### Voraussetzungen

- Ein **Testkonto** bei GitHub (nicht sein Hauptkonto) mit einem feinen Token, das Repositories anlegen und Themen setzen darf. Ohne ihn sind die Schritte 27, 29, 38, 39, 42 und 43 nur gegen den nachgebildeten Server geprüft; die Abnahme wartet.
- Ein **zweiter Eigentümer** für die Abnahme: ein zweites Konto oder die Organisation.
- **Netz** für die Live-Prüfungen; die Suite bleibt ohne Netz.
- Die Organisation bleibt kuratiert; für das Hauptboard braucht es weiter ein Recht auf die Organisation.

### Was dieser Abschnitt nicht baut

- Den **Torwächter** (GitHub App oder Action). Die Naht hält den Platz; gebaut wird er erst, wenn ein Schlüssel bei GitHub liegen darf und die Regeln stehen.
- Die **Geräte-Anmeldung**. Der Token wird weiter eingetragen; die Anmeldung ist Komfort, kein Weg.
- Die **automatische Mitgliedschaft** in der Organisation. Sie ist ein Admin-Recht und bleibt draußen.
- Den **Übertrag** eigener Repositories in die Organisation. GitHub kann ihn; das Board bietet ihn nicht an.

### Scheiben

1. **Eine fremde Station** (Schritte 25–28): Eigentümer, Anmeldung und Recht, Anlegen mit der Naht, Auffinden.
2. **Ein eigenes Vorhaben** (Schritte 29–33): anlegen, Dateien speichern, Fassungen lesen, Verweise, Konflikt.
3. **Katalog und Vertrauen** (Schritte 34–37): zusammenführen, vollständig, sauber, Vertrauensbild.
4. **Mitarbeiten** (Schritte 38–40): Beitrag als Pull Request, Verantwortung, alte Ablage weg.
5. **Abschluss** (Schritte 41–44): fehlende Routen, Umzug als Route, Abnahme, frische Installation.

### Schritte

25. Der Eigentümer ist eine Einstellung.
26. Anmelden und dürfen.
27. Eine Station anlegen — mit austauschbarem Anlegeweg.
28. Auffinden.
29. Ein Vorhaben anlegen.
30. Dateien speichern.
31. Fassungen lesen.
32. Verweise als `owner/repo`.
33. Die Konfliktmeldung nennt die Datei.
34. Der Katalog führt zusammen.
35. Der Katalog ist vollständig.
36. Der Katalog bleibt sauber.
37. Das Vertrauensbild.
38. Beitragen als Pull Request.
39. Verantwortung und Veröffentlichen.
40. Die alte Beitragsablage entfernen.
41. Die fehlenden Routen.
42. Der Umzug als Route.
43. Zwei Menschen, ein Board.
44. Die frische Installation.

### Schritt 25 — Der Eigentümer ist eine Einstellung

- **Scheibe:** Eine fremde Station.
- **Ziel:** Ein Board weiß, unter welchem Eigentümer neue Repositories entstehen; nichts ist mehr fest auf die Organisation verdrahtet.
- **Eingang:** `bauplan.md` Schritte 1–24; `board.json` der Fassung 2 mit `einstellungen`; die vorhandene Einstellungsroute; `komposition.js`.
- **Tun:** `einstellungen.eigentuemer` lesen und schreiben; Vorgabe ist der angemeldete GitHub-Name, für das bestehende Hauptboard `DeepCollaboration`; das Feld in `/board/data` mitgeben; die Anzeige nennt den Eigentümer dort, wo neue Dinge entstehen.
- **Ergebnis:** Ein Board trägt seinen Eigentümer; das Hauptboard bleibt bei der Organisation; ein Board ohne Eintrag nimmt den angemeldeten Namen.
- **Prüfung:** In einem Testboard den Eigentümer auf ein Testkonto setzen, neu laden, der Wert steht da; das Hauptboard nennt weiter `DeepCollaboration`; ohne Anmeldung bleibt der Eintrag leer und die Seite sagt warum.
- **Stopp:** nein.

### Schritt 26 — Anmelden und dürfen

- **Scheibe:** Eine fremde Station.
- **Ziel:** Das Board sagt, wer angemeldet ist und ob diese Person hier anlegen oder schreiben darf; ohne Token sagt es den Grund, und lokales Schreiben bleibt möglich.
- **Eingang:** Schritt 25; `github.js`; `rechte.js`; `/board/github/stand`, `/board/github/token`.
- **Tun:** `stand` nennt den Login und ob ein Token liegt; das Recht wird pro Person gefragt, nicht pro Token; ohne Remote gilt lokal ja; mit Remote und ohne Token nein, mit Grund und Verweis; die Seite bietet nur an, was auch geht.
- **Ergebnis:** `/board/github/stand` nennt den Login; ein Board ohne Token zeigt „nur lesen“ mit Grund; ein lokales Repository lässt sich weiter beschreiben.
- **Prüfung:** Testtoken eintragen, `stand` nennt den Login; Token entfernen, Anlegen wird mit Grund abgewiesen; eine Datei in einem lokalen Repository speichern, es geht.
- **Stopp:** nein.

### Schritt 27 — Eine Station anlegen — mit austauschbarem Anlegeweg

- **Scheibe:** Eine fremde Station.
- **Ziel:** Aus dem Board entsteht ein öffentliches Stations-Repository unter dem Eigentümer, mit Thema, `station.json` und README; der Anlegeweg ist eine Schnittstelle mit einem zweiten Platz.
- **Eingang:** Schritte 25, 26; `formate/station.js`; `github.js`; die Route `/board/station`.
- **Tun:** Anlegen über `einstellungen.anlegeweg`; „selbst“ legt im Konto oder in der Organisation an, setzt das Thema `dsh-board-station`, schreibt den ersten Stand und schickt ihn; Namenskollision und fehlendes Recht werden klar gemeldet; in der Prüfung wird eine Attrappe als zweiter Anlegeweg eingesetzt; die Oberfläche bietet nur „selbst“ an.
- **Ergebnis:** Ein öffentliches Stations-Repository mit Thema und erstem Stand; die Karte steht im Board; die Route kennt keinen fest verdrahteten GitHub-Aufruf.
- **Prüfung:** Über die Seite eine Station anlegen: Repository öffentlich, Thema gesetzt, Karte da; derselbe Name ein zweites Mal: klare Meldung; die Attrappe legt über dieselbe Route ohne Netz an; kein „Torwächter“ in der Oberfläche.
- **Stopp:** nein — aber nur mit dem Testkonto, nie mit seinem Konto.

### Schritt 28 — Auffinden

- **Scheibe:** Eine fremde Station.
- **Ziel:** Öffentliche Stationen anderer erscheinen ohne Zutun, neben den kuratierten; doppelte werden zusammengeführt.
- **Eingang:** Schritt 27; `katalog.js`; `github.js`; `/board/katalog`.
- **Tun:** Suche nach dem Thema, Zwischenspeicher mit Haltbarkeit, Zusammenführen mit `katalog.json`, Zusammenfassen über `owner/repo`, Grenzen und Fehler; anonymes Lesen ist der Normalfall.
- **Ergebnis:** Die Katalogantwort nennt `kuratiert` und `gefunden`; eine fremde Station steht nach dem Neuladen da; ein kuratierter Eintrag erscheint nicht doppelt.
- **Prüfung:** Ein öffentliches Repository mit dem Thema unter einem anderen Eigentümer; nach dem Neuladen steht es unter „gefunden“; das Hauptboard zeigt seine kuratierten weiter; ohne Netz steht der letzte Stand mit Hinweis.
- **Stopp:** nein.

### Schritt 29 — Ein Vorhaben anlegen

- **Scheibe:** Ein eigenes Vorhaben.
- **Ziel:** Ein Vorhaben entsteht als eigenes Repository unter dem Eigentümer, an einer Station, mit festem Workflow-Verweis; es erscheint als Karte.
- **Eingang:** Schritte 25–28; `formate/vorhaben.js`; `POST /board/vorhaben`.
- **Tun:** Zweig für Fassung 2; Recht prüfen; Repository anlegen, Thema `dsh-board-vorhaben`, `vorhaben.json` mit `position` und `workflow: { repo, fassung }`, erste Datei; schicken; Karte.
- **Ergebnis:** Ein neues öffentliches Vorhaben-Repository; die Karte steht an der gewählten Station; der Workflow-Verweis trägt eine Fassung.
- **Prüfung:** Vorhaben an „Idee“ anlegen: Repository, Thema, `vorhaben.json` mit Fassung; Karte; ohne Recht wird es abgewiesen.
- **Stopp:** nein.

### Schritt 30 — Dateien speichern

- **Scheibe:** Ein eigenes Vorhaben.
- **Ziel:** Eine Änderung an einer Datei eines Vorhabens landet in dessen Repository, nicht in der alten Ablage.
- **Eingang:** Schritt 29; `/board/doc/schreiben`; `dateiBasis`.
- **Tun:** Das Vorhaben-Repository auflösen; der Pfad bleibt darin; Commit; offline lokal, sonst schicken; kein `..`.
- **Ergebnis:** Die Datei ist nach dem Neuladen geändert; der Commit steht im richtigen Repository.
- **Prüfung:** Datei bearbeiten, neu laden, Inhalt da; `git log` im Vorhaben-Repository zeigt den Commit; ein Pfad mit `..` wird abgewiesen.
- **Stopp:** nein.

### Schritt 31 — Fassungen lesen

- **Scheibe:** Ein eigenes Vorhaben.
- **Ziel:** Die Komposition liest Stationen und Workflows auf der genannten Fassung; eine neue Stationsfassung ändert bestehende Workflows nicht.
- **Eingang:** Schritte 29, 30; `komposition.js`; `workflow.json` und `board.json` mit `fassung`.
- **Tun:** Die genannte Fassung aus der Git-Geschichte lesen; fehlt sie, gilt der aktuelle Stand und wird beim nächsten Schreiben festgehalten; das Nachziehen wird eine eigene Handlung; die Fassung steht an der Karte.
- **Ergebnis:** Ein Workflow bleibt auf seiner Fassung; ein neuer Commit an der Station wirkt erst nach dem Nachziehen.
- **Prüfung:** Eine Station ändern; der bestehende Workflow zeigt den alten Stand; nachziehen; jetzt zeigt er den neuen.
- **Stopp:** nein.

### Schritt 32 — Verweise als `owner/repo`

- **Scheibe:** Ein eigenes Vorhaben.
- **Ziel:** Neue Verweise tragen keinen absoluten Pfad; sie sind auf einem anderen Rechner auflösbar.
- **Eingang:** Schritt 25; `/board/workflow/station`, `/board/workflow/verbinden`, `/board/uebernehmen`.
- **Tun:** Die drei Schreibstellen; den Verweis aus dem Remote als `owner/repo` bilden; die alte Form weiter lesen.
- **Ergebnis:** `workflow.json` und `board.json` enthalten `owner/repo`; kein neuer Verweis nennt `/workspace`.
- **Prüfung:** Eine Station in einen Workflow stellen: der Verweis ist `owner/repo`; `grep /workspace` in den neuen Dateien findet nichts; ein zweiter Rechner mit frischem Zwischenspeicher löst ihn auf.
- **Stopp:** nein.

### Schritt 33 — Die Konfliktmeldung nennt die Datei

- **Scheibe:** Ein eigenes Vorhaben.
- **Ziel:** Bei einem Konflikt steht die betroffene Datei in der Meldung.
- **Eingang:** `abgleich.js`.
- **Tun:** Auch die Ausgabe des Abgleichs lesen; die Zeile mit dem Konflikt auswerten; die Datei nennen.
- **Ergebnis:** Die Standzeile nennt die Datei; der Rest der Repositories bleibt in Ruhe.
- **Prüfung:** Einen Konflikt erzeugen; die Meldung nennt die Datei; die übrigen Repositories bleiben unberührt.
- **Stopp:** nein.

### Schritt 34 — Der Katalog führt zusammen

- **Scheibe:** Katalog und Vertrauen.
- **Ziel:** Der Katalog zeigt kuratierte und gefundene Dinge in einer Antwort, ohne Doppel.
- **Eingang:** Schritte 28, 31; `katalog.js`; `formate/katalog.js`.
- **Tun:** `kuratiert` und `gefunden` zusammensetzen; über `owner/repo` zusammenfassen; den Stand mitgeben.
- **Ergebnis:** Ein Katalog mit beiden Herkünften; die Herkunft ist am Eintrag sichtbar.
- **Prüfung:** Ein eigener und ein fremder Eintrag; keine Doppel; beide Herkünfte stehen da.
- **Stopp:** nein.

### Schritt 35 — Der Katalog ist vollständig

- **Scheibe:** Katalog und Vertrauen.
- **Ziel:** Jeder Eintrag trägt Beispiele, Prüfung, Sterne, Fragen und Beiträge; Fragen und Beiträge werden getrennt gezählt.
- **Eingang:** Schritt 34; `katalog.js`; `github.js`.
- **Tun:** Beispiele aus dem Repository sammeln; `geprüft` mit Person und Datum durchreichen; offene Fragen und offene Pull Requests getrennt zählen; Sterne lesen.
- **Ergebnis:** Ein Eintrag nennt Beispiele, Prüfung, Sterne, Fragen und Beiträge.
- **Prüfung:** Ein Eintrag mit Beispielen: sie stehen da; ein offener Pull Request erhöht die Fragen nicht; `geprüft` nennt Person und Datum.
- **Stopp:** nein.

### Schritt 36 — Der Katalog bleibt sauber

- **Scheibe:** Katalog und Vertrauen.
- **Ziel:** Ein öffentlicher Katalog nennt keine privaten Dinge; Unerwünschtes lässt sich aus den Listen nehmen, ohne es zu löschen.
- **Eingang:** Schritte 34, 35; `katalog.json`.
- **Tun:** Private Einträge filtern und ihre Namen nicht nennen; eine Sperrliste `gesperrt` im Katalog; gesperrte Dinge aus den Listen nehmen; ein direkter Verweis bleibt lesbar.
- **Ergebnis:** Kein privater Name im öffentlichen Katalog; Gesperrtes verschwindet aus den Listen.
- **Prüfung:** Ein privates Repository in `katalog.json`: es erscheint nicht; einen Eintrag sperren: nach dem Neuladen weg; der direkte Verweis öffnet weiter.
- **Stopp:** nein.

### Schritt 37 — Das Vertrauensbild

- **Scheibe:** Katalog und Vertrauen.
- **Ziel:** Stationsseite und Bibliothekskarten zeigen Sterne, Releases, Fragen, Beiträge und Beispiele.
- **Eingang:** Schritt 35; `/board/station`; `client.js`.
- **Tun:** Die Metadaten lesen und zwischenspeichern; an der Station und an der Bibliothekskarte anzeigen; den Verweis ins Repository setzen.
- **Ergebnis:** Eine Station mit Sternen zeigt sie; die Bibliothekskarte nennt sie, ohne dass die Einzelheit geöffnet wird.
- **Prüfung:** Eine Station mit Sternen öffnen: Zahl und Releases da; in der Bibliothek zeigt die Karte Sterne, Beispiele und Fragen.
- **Stopp:** nein.

### Schritt 38 — Beitragen als Pull Request

- **Scheibe:** Mitarbeiten.
- **Ziel:** „Beitragen“ öffnet einen Pull Request; es entsteht keine lokale Beitragsdatei.
- **Eingang:** Schritte 34–37; `/board/github/beitrag`; `/board/beitrag` (alt).
- **Tun:** Die Oberfläche auf den Pull-Request-Weg führen; den Stand des Beitrags zeigen; die alte Route nicht mehr aufrufen.
- **Ergebnis:** Ein Beitrag ist ein Pull Request; sein Stand ist sichtbar.
- **Prüfung:** An einer fremden Station beitragen: der Pull Request existiert; unter `beitraege/` entsteht nichts.
- **Stopp:** nein.

### Schritt 39 — Verantwortung und Veröffentlichen

- **Scheibe:** Mitarbeiten.
- **Ziel:** Verantwortliche stehen an den Karten; Veröffentlichen hängt nur eigene Repositories um.
- **Eingang:** Schritt 38; `/board/github/veroeffentlichen`.
- **Tun:** `verantwortliche` anzeigen; prüfen, welche Repositories dem Board gehören; fremde auslassen und in der Meldung nennen.
- **Ergebnis:** Ein Board mit fremden Verweisen veröffentlicht nur seine eigenen; die Meldung nennt die fremden; die Karten nennen die Verantwortlichen.
- **Prüfung:** Ein Board mit einer fremden Station veröffentlichen: die fremde bleibt unberührt; die Meldung nennt sie; die Karte zeigt den Verantwortlichen.
- **Stopp:** ja — es werden Repositories bei GitHub angelegt oder umgehängt.

### Schritt 40 — Die alte Beitragsablage entfernen

- **Scheibe:** Mitarbeiten.
- **Ziel:** Kein Weg schreibt mehr `beitraege/` oder legt lokale Fork-Kopien an.
- **Eingang:** Schritt 38.
- **Tun:** Die alten Routen entfernen; die Prüfungen umstellen; alte Daten lesbar lassen.
- **Ergebnis:** Die Routen antworten nicht mehr; die Suite ist grün.
- **Prüfung:** `POST /board/beitrag` antwortet nicht mehr; es entsteht kein Ordner; die Suite läuft durch.
- **Stopp:** nein.

### Schritt 41 — Die fehlenden Routen

- **Scheibe:** Abschluss.
- **Ziel:** `/board/board`, `/board/board/verbinden` und `/board/workflow` antworten.
- **Eingang:** Schritte 25–40.
- **Tun:** Die drei Routen ergänzen; die vorhandenen Wege dahinterlegen; Prüfungen.
- **Ergebnis:** Die drei Routen liefern JSON; die Seite bleibt bedienbar.
- **Prüfung:** Die drei Routen aufrufen; jede antwortet; das Board funktioniert weiter.
- **Stopp:** nein.

### Schritt 42 — Der Umzug als Route

- **Scheibe:** Abschluss.
- **Ziel:** Der Umzug läuft aus dem Board, mit Zuordnung der Kennungen zu GitHub-Namen.
- **Eingang:** `werkzeuge/umzug2.mjs`; `identitaet.json`.
- **Tun:** Eine Route für den Umzug; die Zuordnung vor dem Lauf abfragen und im Bericht festhalten; den vorhandenen Umzug dahinterlegen.
- **Ergebnis:** Ein Aufruf schreibt die Repositories und gibt einen Bericht; die alte Ablage bleibt unverändert.
- **Prüfung:** Auf einer Kopie umziehen; der Bericht nennt die Zuordnung; die alten Inhalte sind unverändert.
- **Stopp:** ja — er schreibt viele Repositories.

### Schritt 43 — Zwei Menschen, ein Board

- **Scheibe:** Abschluss.
- **Ziel:** Der offene Weg ist einmal gegangen: A legt an, B findet, benutzt, forkt und stellt einen Pull Request; A sieht ihn.
- **Eingang:** Schritte 25–42; ein Testkonto und ein zweiter Eigentümer.
- **Tun:** Die Wege gehen; die Prüfungen laufen; die README beschreibt sie.
- **Ergebnis:** Ein Bericht mit den gegangenen Wegen und den offenen Resten.
- **Prüfung:** A legt eine Station an; B findet sie ohne Zutun; B stellt sie in einen Workflow; B forkt und stellt einen Pull Request; A sieht ihn; B legt ein Vorhaben an; A sieht es; der Katalog zeigt beides; ohne Token ist alles lesbar.
- **Stopp:** ja — seine Abnahme.

### Schritt 44 — Die frische Installation

- **Scheibe:** Abschluss.
- **Ziel:** Ein frisches Profil sieht das Hauptboard und die gefundenen Stationen ohne Token; Anlegen verlangt eine Anmeldung.
- **Eingang:** Schritt 43; ein frisches Profil.
- **Tun:** Den Erststart ohne Token gehen; anonym suchen; beim Anlegen erklären, was fehlt; die README nachziehen.
- **Ergebnis:** Ein frisches Profil ist lesend bedienbar; Anlegen sagt, was fehlt.
- **Prüfung:** Frisches Profil starten: Hauptboard und gefundene Stationen sind da, kein Fehler; „anlegen“ verlangt eine Anmeldung und nennt den Weg.
- **Stopp:** ja — das Plugin wird in ein frisches Profil installiert.

## Zurückgeschickt

Nichts. Die Architektur beantwortet die offenen Punkte der früheren Etappen; der Umzug und die Zugänge sind Annahmen und Stopps, keine offenen Fragen. Für den Marktplatz gilt dasselbe: Der Nachtrag entscheidet Aufteilung, Naht und Identität; der Testzugang ist eine Voraussetzung, keine offene Frage. Ohne ihn bleiben die Live-Prüfungen nachgebildet.
