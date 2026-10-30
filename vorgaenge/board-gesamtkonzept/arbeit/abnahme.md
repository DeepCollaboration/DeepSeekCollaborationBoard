# Abnahme — Board

- **Datum:** 2026-10-30
- **Befehl:** `cd /workspace/projekte/board-plugin && node werkzeuge/abnahme.mjs --bericht /workspace/projekte/board/vorgaenge/board-gesamtkonzept/arbeit/abnahme.md`
- **Exit-Code:** 0
- **Entscheidende Zeile:** Abnahme bestanden: 13 von 13 Prüfungen der neuen Form bestanden; Suite 98 von 98; sieben Wege belegt; live geprüft (0 Sterne, öffentlich, main); Hauptboard 11 Schritte/1 Vorhaben/0 Lücken (Vorgabe 0 Vorhaben durch Schritt 21 überholt), Team 11 Schritte/4 Vorhaben/0 Lücken; Register 3 Boards, aktiv hauptboard-neu; alte Ablage Daten unverändert seit ae905d8 (HEAD 6123b58).

Der Lauf führt die Prüfungen der neuen Form, die Suite, die Live-Probe, die beiden echten Boards, das Register und die alte Ablage nacheinander aus und ordnet die sieben Wege ihren Prüfungen zu.

## Die sieben Wege

| # | Weg | Prüfung | Ergebnis | Beleg |
| --- | --- | --- | --- | --- |
| 1 | Hauptboard ohne Einrichten | schritt33 | belegt | ok Hauptboard: ausdrücklich geholt, fest, ohne Identität; nicht geholt mit Wiederholen; danach da |
| 2 | Station prüfen und sternen | schritt27, schritt31 | belegt | ok GitHub: Sterne 7, Fassung v1.2.0; Token 0600, verbunden, gelöscht; 404/401/Grenze klar; Token nirgends im Text · schritt27 in der Suite 98/98 |
| 3 | Probeprojekt übernehmen | schritt38 | belegt | ok Probeprojekt: ohne Name 409, übernommen, Kopie verweist aufs Original, Original unverändert |
| 4 | Privates Team-Board | schritt30, schritt31, schritt32 | belegt | ok Umzug: 11 Stationen, 1 Workflow, 4 Vorhaben, 2 Boards; Quelle unverändert · (+ 2 weitere Prüfungen) |
| 5 | Beitrag als Pull Request | schritt35 | belegt | ok Beiträge: ohne Token Verweis, mit Token PR #1 offen, zusammengeführt, geschlossen sichtbar |
| 6 | Fork | schritt37 | belegt | ok Fork und Frage: ohne Token Verweis, Fork verlinkt, Frage #1 offen, geschlossen sichtbar |
| 7 | Frage | schritt37 | belegt | ok Fork und Frage: ohne Token Verweis, Fork verlinkt, Frage #1 offen, geschlossen sichtbar |

## Prüfungen der neuen Form (29 bis 40)

| Prüfung | Exit | Dauer | Erste ok-Zeile |
| --- | --- | --- | --- |
| test/schritt29.mjs | 0 | 248 ms | ok Alte Ablage: 2 Vorhaben (altes-vorhaben mit idee.md und Transkript, zweites-vorhaben), Workflow „Alte Kette“ mit den Schritten Anfang und Prüfen, Person „Alte Hüterin“; an „start“ liegt die Datei idee.md |
| test/schritt30.mjs | 0 | 1161 ms | ok Umzug: 11 Stationen, 1 Workflow, 4 Vorhaben, 2 Boards; Quelle unverändert |
| test/schritt31.mjs | 0 | 467 ms | ok GitHub: Sterne 7, Fassung v1.2.0; Token 0600, verbunden, gelöscht; 404/401/Grenze klar; Token nirgends im Text |
| test/schritt32.mjs | 0 | 801 ms | ok Sichtbarkeit: Lücke erkannt, Schreiben abgewiesen, ohne Token Befehle, mit Token gepusht, Lesemodus |
| test/schritt33.mjs | 0 | 345 ms | ok Hauptboard: ausdrücklich geholt, fest, ohne Identität; nicht geholt mit Wiederholen; danach da |
| test/schritt33b.mjs | 0 | 786 ms | ok Hauptboard: geholt und allein im Register |
| test/schritt34.mjs | 0 | 363 ms | ok Katalog: 4 Einträge, Sterne 7/2, Suche „Idee“, frisch 8, offline letzter Stand |
| test/schritt35.mjs | 0 | 615 ms | ok Beiträge: ohne Token Verweis, mit Token PR #1 offen, zusammengeführt, geschlossen sichtbar |
| test/schritt36.mjs | 0 | 763 ms | ok Verantwortung: thomas darf, fremd 403, anna über Team, Host prüft, Teams sichtbar |
| test/schritt37.mjs | 0 | 555 ms | ok Fork und Frage: ohne Token Verweis, Fork verlinkt, Frage #1 offen, geschlossen sichtbar |
| test/schritt38.mjs | 0 | 246 ms | ok Probeprojekt: ohne Name 409, übernommen, Kopie verweist aufs Original, Original unverändert |
| test/schritt39.mjs | 0 | 595 ms | ok Frische Installation: Hauptboard ausdrücklich geholt, Name erst beim Beitreten, eigenes Board, deutsche README aktuell |
| test/schritt40.mjs | 0 | 395 ms | ok Archiv: lesbar, Schreiben 403 ohne Commit, Wechsel erlaubt, Fassung 2 schreibbar |

Bestanden: 13 von 13.

## Die Suite

| Lauf | Exit | Bestanden | Gesamt | Dauer |
| --- | --- | --- | --- | --- |
| node --test | 0 | 98 | 98 | 7081 ms |

## Live-Probe (nur lesend, ohne Token)

| Repository | Status | Sterne | Sichtbarkeit | Standardzweig | Hinweis |
| --- | --- | --- | --- | --- | --- |
| DeepCollaboration/DeepSeekCollaborationBoard | geprüft | 0 | öffentlich | main | https://github.com/DeepCollaboration/DeepSeekCollaborationBoard |

## Die echten migrierten Boards

| Board | Schritte | Vorhaben | Lücken | Ergebnis | Hinweis |
| --- | --- | --- | --- | --- | --- |
| Hauptboard | 11 | 1 (Probeprojekt, öffentlich) | 0 | bestanden | Vorgabe „0 Vorhaben“ durch Schritt 21 überholt: das Probeprojekt liegt öffentlich auf dem Hauptboard. |
| Team | 11 | 4 (Board Gesamtkonzept, privat; Board Gesamtkonzept — Idee zu Produkt, privat; Board Gesamtkonzept — VidionPrüfen, privat; Second Brain Gesamtkonzept, privat) | 0 | bestanden | vier private Vorhaben, elf Schritte, keine Lücke |

Das Hauptboard trägt seit Schritt 21 das öffentliche Probeprojekt; ohne dieses Vorhaben sind es 0 Vorhaben. Die Vorgabe „0 Vorhaben“ in Schritt 23 stammt von vor Schritt 21. Das Team-Board führt seine vier Vorhaben privat und ohne Lücke.

## Register und alte Ablage

| Prüfung | Ergebnis | Beleg |
| --- | --- | --- |
| Register | bestanden | 3 Boards: hauptboard, hauptboard-neu, team; aktiv hauptboard-neu |
| Alte Ablage | bestanden | HEAD 6123b58, 99 Fassungen, Board-Daten unverändert seit ae905d8 |
| Unverändert durch den Lauf | bestanden | 19 echte Repositories und die alte Ablage vor und nach dem Lauf gleich |

## Grenzen

- **Live geprüft ist nur das Lesen.** Sterne, Sichtbarkeit und Standardzweig kommen von `api.github.com`; Anlegen, Push, Pull Requests, Forks und Fragen laufen gegen einen nachgebildeten Server (schritt35, schritt37) und sind live ungeprüft.
- **Die Abnahme durch den Auftraggeber steht aus.** Dieser Lauf belegt die Wege im Modul und an den echten Repositories; er ersetzt nicht den Blick auf das laufende Board im Browser. Der laufende Web-Prozess trägt bis zum Neustart den beim Start geladenen Stand.
- **Die neuen Repositories sind lokal, ohne Remote und nicht veröffentlicht.** Die Live-Probe liest das alte öffentliche Repository `DeepSeekCollaborationBoard`; der Erststart gegen das neue Hauptboard ist mit lokalen Bare-Repositories geprüft, nicht gegen GitHub.
- **Probeprojekt und Katalog sind lokal, nicht veröffentlicht.** Sie liegen unter `/workspace/projekte/board-repos/`; es gab keinen Push.
- **Ohne Token.** Die Live-Probe liest öffentlich und ohne Anmeldung; private Metadaten und Schreibwege bleiben ungeprüft.
- **Die Prüfungen der neuen Form und die Suite schreiben nur unter `/tmp/board-scheibe1/`** und räumen dort selbst auf; die echten Repositories und `$DSH_HOME/board` werden nur gelesen.

Der vollständige Befund steht als JSON in der Ausgabe des Laufs.
