# Fortschritt

## Runde 15 — 2026-10-26

- **Stand:** Schritt 15 bestanden. **Scheibe 5 ist halb: Nichts wird still
  überschrieben.** `/board/doc` liefert zu jeder Datei ihre `basis` — den Commit,
  der sie zuletzt geändert hat, `null` ohne Fassung. `/board/doc/schreiben`
  prüft sie vor dem Schreiben: weicht sie ab, kommt 409 mit `neu: { text, basis }`,
  die Datei bleibt byte-gleich. Der eigene Text bleibt im Editor und lässt sich
  über `/board/beitrag` mit der neuen `basis` als Beitrag anlegen; mit der neuen
  `basis` geht das Speichern durch. Nichts wird zusammengeführt. Beleg:
  `test/schritt15.mjs` Exit 0, Tests 02–14 grün, echte Ablage ohne Prüfvorhaben,
  Beitrag und Fassung.
- **Nächster Schritt:** 16 — Zwei Programmfassungen schreiben nicht gegeneinander.
- **Blocker:** keiner.
- **Nicht geprüft:** der 409-Ablauf im Browser; das laufende Board arbeitet noch
  mit dem alten Modul.

## Runde 14 — 2026-10-26

- **Stand:** Schritt 14 bestanden. **Scheibe 4 ist vollständig: Herkunft und
  Rückweg.** Zurückschicken trägt `{ von, nach, grund, seit, erledigt: false }`
  ein und setzt das Vorhaben an die Zielstation; die Verantwortung erbt vom
  Abgebenden. `/board/data` liefert die Rücksprünge mit Ziel- und Herkunftsnamen;
  die Seite zeigt die offenen mit „↩ zurück nach …“. Weiterziehen hinter das Ziel
  setzt `erledigt: true` — der Eintrag bleibt in der Datei, die Anzeige ist leer.
  Ohne Grund oder Ziel: 400; fremdes Vorhaben: 403. Beleg: `test/schritt14.mjs`
  Exit 0, Tests 02–13 grün, echte Ablage ohne Rücksprung und ohne zweite Person.
- **Nächster Schritt:** 15 — Speichern auf einem überholten Stand schreibt nicht.
- **Blocker:** keiner.
- **Nicht geprüft:** die Rücksprung-Anzeige im Browser.

## Runde 13 — 2026-10-26

- **Stand:** Schritt 13 bestanden, Herkunft von Zweig und Fork.
- **Nächster Schritt:** 14.

## Runde 12 — 2026-10-26

- **Stand:** Schritt 12 bestanden; Scheibe 3 vollständig.
- **Nächster Schritt:** 13.

## Runde 11 — 2026-10-26

- **Stand:** Schritt 11 bestanden, Beiträge.
- **Nächster Schritt:** 12.

## Runde 10 — 2026-10-26

- **Stand:** Schritt 10 bestanden; Scheibe 2 vollständig.
- **Nächster Schritt:** 11.

## Runde 9 — 2026-10-26

- **Stand:** Schritt 09 bestanden, fremder Schritt als Verweis.
- **Nächster Schritt:** 10.

## Runde 8 — 2026-10-26

- **Stand:** Schritt 08 bestanden, Schritt-Anweisungen in der Ablage.
- **Nächster Schritt:** 09.
- **Nicht geprüft:** ob eine echte Sitzung die Anweisung nach dem Wegnehmen noch
  laden kann.

## Runde 7 — 2026-10-26

- **Stand:** Schritt 07 bestanden; Scheibe 1 vollständig.
- **Nächster Schritt:** 08.

## Runde 6 — 2026-10-26

- **Stand:** Schritt 06 bestanden, Verantwortliche und eine Prüfstelle `darf`.
- **Nächster Schritt:** 07.

## Runde 5 — 2026-10-26

- **Stand:** Schritt 05 bestanden, Flächen je Person.
- **Nächster Schritt:** 06.

## Runde 4 — 2026-10-26

- **Stand:** Schritt 04 bestanden, Abgleich mit `stand`.
- **Nächster Schritt:** 05.
- **Offen für den Betrieb:** kein `origin` in der Ablage.

## Runde 3 — 2026-10-26

- **Stand:** Schritt 03 bestanden, Ablage auf Format 1.
- **Nächster Schritt:** 04.
- **Beobachtung:** Das alte Board wird weiter benutzt; vor dem Umschalten wird
  der Umzug mit dem letzten `board.json` aufgefrischt.

## Runde 2 — 2026-10-26

- **Stand:** Schritt 02 bestanden, eigene Ablage und Person.
- **Nächster Schritt:** 03.

## Runde 1 — 2026-10-26

- **Stand:** Plan angelegt, Schritt 01 bestanden.
- **Nächster Schritt:** 02.
