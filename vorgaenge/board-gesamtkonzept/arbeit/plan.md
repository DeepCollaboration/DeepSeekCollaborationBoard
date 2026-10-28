# Plan

## Scheiben

1. **Das zusammengesetzte Board** — vier Repositories, ein Board; lesen, zeigen, weitergeben, Stand je Repository.
2. **Anzeige, Bibliothek, Vertrauen** — Boards hinzufügen und wechseln; Stationen und Workflows in der Bibliothek; das Vertrauensbild.
3. **Der Umzug** — die alte Ablage lesen; die vorhandenen Inhalte in getrennte Repositories schreiben; öffentlich und privat trennen.
4. **GitHub und die Zugänge** — Token, Metadaten, Sterne; öffentlich und privat; das Hauptboard kommt von selbst; der Katalog.
5. **Zusammenarbeit** — Beiträge als Pull Requests, Verantwortung und Teams, Forken und Fragen.
6. **Die Einladung** — das Probeprojekt, die frische Installation, die Abnahme, die alte Ablage abnehmen.

## Schritte

| Nr | Schritt | Ergebnis | Prüfung | Status |
| --- | --- | --- | --- | --- |
| 01 | Die vier Formate lesen | `repos.js`, `formate/station.js`, `workflow.js`, `vorhaben.js`, `board.js`; Prüfablage; `test/schritt18.mjs` | `node --test test/schritt18.mjs` und die alte Prüfsuite grün | bestanden |
| 02 | Das Board aus den Repositories zusammensetzen | `/board/data` liefert das gewohnte Bild aus der Prüfablage | `curl /board/data` nennt Karte und Stationen | bestanden |
| 03 | Die Fläche zeigt das zusammengesetzte Board | Seite zeigt Karte an der Station, Datei öffnet | Board-Seite, Testboard, Karte | bestanden |
| 04 | Weitergeben schreibt ins Vorhaben-Repository | `position` in `vorhaben.json`; nur dieses Repository | `git log -1 --stat` im Vorhaben-Repository | bestanden |
| 05 | Der Stand je Repository | Standzeile je Repository; offline sichtbar | Remote abschalten, weitergeben, wieder abgleichen | bestanden |
| 06 | Die Anzeige: Boards hinzufügen, wechseln, entfernen | Board-Liste; Hauptboard fest | Testboard aufnehmen, öffnen, entfernen | offen |
| 07 | Die Bibliothek: Stationen, Workflows, Vorhaben | Karten der drei Arten mit Host und Fassung | Bibliothek öffnen; drei Arten | offen |
| 08 | Eine Station in einen Workflow stellen | Workflow-Repository schreibt `stationen`/`positionen`/`kanten` | `git log -1 --stat` im Workflow-Repository | offen |
| 09 | Das Vertrauensbild einer Station | Stationsseite: Host, Fassung, Beschreibung, Beispiele, Verwendung, Verweis | Station öffnen; fehlende Angaben als fehlend | offen |
| 10 | Einen Workflow in ein Board übernehmen | `board.json` schreibt den Verweis; Fläche baut neu | `git log -1 --stat` im Board-Repository | offen |
| 11 | Die alte Ablage lesen | Fassung 1 bleibt lesbar; Fassung 2 daneben | altes Board unverändert; beide Prüfungen grün | offen |
| 12 | Das Umzugswerkzeug | `werkzeuge/umzug2.mjs`; Bericht; neue lokale Repositories | Umzug auf einer Kopie; Bericht zählt | offen |
| 13 | Die vorhandenen Inhalte umziehen | Hauptboard öffentlich, Team-Board privat; vier Vorhaben privat | zwei Boards; altes Board bleibt | offen |
| 14 | GitHub-Client und Token | `github.js`; Token 0600; Metadaten und Sterne | öffentliches Repository zeigt Sterne; Token eintragen/löschen | offen |
| 15 | Öffentlich und privat | Repositories auf GitHub; Sichtbarkeit; Lesemodus; Verweisprüfung | öffentlich lesbar; privat nur mit Zugang; Lücke bei privatem Verweis | offen |
| 16 | Das Hauptboard kommt von selbst | frisches Profil sieht das Hauptboard ohne Einrichten | frisches Profil; Hauptboard; Entfernen fehlt | offen |
| 17 | Sterne und Katalog | Katalog mit Sternen, Suche, Verweis, Zwischenspeicher | Katalog; Suche; Neuladen aus dem Zwischenspeicher | offen |
| 18 | Beiträge als Pull Requests | PR lesen/anlegen; Board zeigt Stand | Test-PR; Board; zusammenführen | offen |
| 19 | Verantwortung und Teams | Host, Verantwortliche, Teams sichtbar; Schreibprüfung | Station, Vorhaben, Team; Schreiben abgewiesen | offen |
| 20 | Forken und Fragen | Fork und Issue verlinkt; Board zeigt Stand | Fork; Issue; schließen | offen |
| 21 | Das Probeprojekt | öffentliches Vorhaben an „Idee“; übernehmen in eigenes Board | Hauptboard; übernehmen; Original bleibt | offen |
| 22 | Die frische Installation | frisches Profil: Hauptboard, Identität später, eigenes Board | frisches Profil; übernehmen; eigenes Board | offen |
| 23 | Die Abnahme | Wege gegangen; Prüfungen; Bericht | Liste der Wege mit Ergebnis | offen |
| 24 | Die alte Ablage abnehmen | Fassung 1 nur noch lesbar; altes Board Archiv | neue Prüfungen; altes Board lesbar; Schreiben abgewiesen | offen |
