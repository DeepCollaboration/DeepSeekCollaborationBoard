# Fortschritt

## Runde 6 — 2026-10-26

- **Stand:** Schritt 06 bestanden. Alle fünf Vorhaben tragen `verantwortliche`
  mit `p1` an ihrer Station. Eine Prüfstelle `darf(vorhaben, aktion, person)`
  entscheidet über `aendern`, `verschieben`, `weitergeben`, `zurueckschicken`;
  alle vier Routen antworten fremd mit 403. `/board/data` liefert
  `verantwortet` und `darf`; „Weitergeben“ trägt die nächste verantwortliche
  Person ein und verschiebt. Beleg: `test/schritt06.mjs` Exit 0 (9 ok-Zeilen),
  Tests 02–05 weiter grün.
- **Nächster Schritt:** 07 — Ein fremdes Vorhaben steht auf meinem Board und
  lässt sich lesen.
- **Blocker:** keiner.
- **Nicht geprüft:** die Anzeige „verantwortet von“ und das Weitergeben-Formular
  im Browser.

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
