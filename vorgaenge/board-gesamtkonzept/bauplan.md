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

## Zurückgeschickt

Nichts. Die Architektur beantwortet die offenen Punkte der früheren Etappen; der Umzug und die Zugänge sind Annahmen und Stopps, keine offenen Fragen.
