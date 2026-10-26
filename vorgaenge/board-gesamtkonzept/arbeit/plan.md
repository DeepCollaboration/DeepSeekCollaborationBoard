# Plan

Übernommen aus `bauplan.md` vom 2026-10-26, Schritte unverändert. Scheiben und
Reihenfolge sind dort begründet.

## Scheiben

1. **Zwei Personen sehen einander** — Schritte 01–07. Danach stehen fremde
   Workflows, Schritte und Vorhaben mit Host und Verantwortlichem auf dem Board,
   und der Stand ist abgeglichen.
2. **Ein fremder Schritt läuft bei mir** — Schritte 08–10. Danach kann ein eigenes
   Vorhaben einen fremden Schritt benutzen, und sein Skill läuft im eigenen
   Harness.
3. **Beitragen und annehmen** — Schritte 11–12. Danach lässt sich an einem fremden
   Vorhaben ein Beitrag anlegen; der Verantwortliche nimmt an, lehnt ab oder nimmt
   Teile.
4. **Herkunft und Rückweg** — Schritte 13–14. Danach tragen Zweig und Fork ihre
   Herkunft, und ein Rücksprung steht am Vorhaben, bis es wieder daran vorbei ist.
5. **Nichts wird still überschrieben** — Schritte 15–16. Danach schreibt weder ein
   überholter Stand noch eine ältere Programmfassung.

## Schritte

| Nr | Schritt | Ergebnis | Prüfung | Status |
| --- | --- | --- | --- | --- |
| 01 | Das Board wird ein Produkt dieses Vorhabens | `produkt/` mit dem Plugin; Profil lädt von dort | Kopie identisch; Profilauflösung zeigt auf `produkt/`; Board unverändert | bestanden |
| 02 | Die Ablage ist ein eigenes Repository mit meiner Person | `projekte/board/.git`, `produkt/board.config.json` | Speichern im Board → `git log --format=%an` nennt den eigenen Namen; `/workspace` bleibt ruhig | bestanden |
| 03 | Die Ablage trägt Personen und Workflows | `format`, `personen/`, `workflows/`, `vorgaenge/*/vorhaben.json`; `board.json` weg | Board zeigt dieselben Stationen; Leser, Verlauf, Vergleich arbeiten; `board.json` fehlt | bestanden |
| 04 | Der Abgleich holt und gibt | Route `/board/sync`; Anzeige „zuletzt abgeglichen“ und „n Fassungen nur hier“ | Zweiter Klon am nackten `origin`: Änderung kommt an, eigene Fassung liegt im Remote; ohne Remote öffnet das Board und sagt es | bestanden |
| 05 | Der Workflow der anderen Person steht auf meinem Board | `/board/data` liefert `workflows`; fremde Fläche sichtbar | Fremde Person samt zwei Schritten erscheint; fremder Schritt zeigt keine Änderungsfelder; eigener Workflow unverändert | bestanden |
| 06 | Jedes Vorhaben nennt seinen Verantwortlichen | `verantwortliche`, `regel`; Anzeige; eine Prüfung für Handlungen | Eigenes Vorhaben: „verantwortet von <ich>“ und Weitergeben da; fremdes: Weitergeben fehlt | bestanden |
| 07 | Ein fremdes Vorhaben steht auf meinem Board und lässt sich lesen | Fremde Vorhaben sichtbar und lesbar, ohne Eingriff | Fremdes Vorhaben an dessen Station mit Namen; `idee.md` öffnet; Umbenennen, Ziehen, Speichern fehlen | bestanden |
| 08 | Der Skill eines Schritts liegt in der Ablage | `skills/<person>/<schritt>.md`; Lauf nimmt die Anweisung von dort | Während des Laufs liegt `$DSH_HOME/skills/<host>-<schritt>/SKILL.md`, danach nicht; Transkript am Schritt | läuft |
| 09 | Mein Workflow enthält einen fremden Schritt | Verweis in `workflows/<ich>.json`; fremde Station sichtbar | Schritt steht mit „bei <Person>“; Einstellungen nicht öffenbar; fremde Datei unverändert | offen |
| 10 | Der Skill der anderen Person läuft in meinem Harness | Lauf eines fremden Schritts; Transkript im eigenen Vorhaben | Gespräch folgt der Anweisung aus der Ablage; Transkript unter `transkripte/`; Ablage ruhig | offen |
| 11 | Am fremden Vorhaben speichert der Editor als Beitrag | `beitraege/<vorhaben>/<beitrag>.json` und vorgeschlagene Dateien; Beitrag sichtbar | Beitrag erscheint mit Namen und Notiz; Originaldatei unverändert | offen |
| 12 | Der Verantwortliche entscheidet über den Beitrag | `entschieden`; Übernahme als neue Fassung | Annehmen schreibt den Text; Teile übernehmen nur die genannten; Ablehnen lässt alles | offen |
| 13 | Zweig und Fork tragen ihre Herkunft | `zweig_von`, `fork_von`; Anzeige am Vorhaben | Zweig trägt Herkunft, Original unverändert; Fork eines fremden Vorhabens nennt dessen Namen | offen |
| 14 | Der Rücksprung steht am Vorhaben | `rueckkehr`-Vermerk; Anzeige | Zurückschicken zeigt Grund und Ziel; darüberziehen setzt `erledigt`, Vermerk verschwindet aus der Anzeige | offen |
| 15 | Speichern auf einem überholten Stand schreibt nicht | Basisprüfung vor dem Schreiben; „mein Text als Beitrag“ | Alter Stand wird nicht geschrieben; Meldung; eigener Text bleibt; Datei unverändert | offen |
| 16 | Zwei Programmfassungen schreiben nicht gegeneinander | `format`-Vergleich vor dem Schreiben | Höhere `format`-Zahl → kein Schreiben, Meldung nennt sie; zurückgestellt → Speichern geht | offen |

Stopp-Punkte laut Bauplan: 01, 02, 03, 04, 10. Sie markieren hohes Risiko, nicht
einen Halt; der Lauf geht weiter.
