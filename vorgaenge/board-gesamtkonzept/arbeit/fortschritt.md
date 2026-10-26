# Fortschritt

## Runde 11 — 2026-10-26

- **Stand:** Schritt 11 bestanden. `POST /board/beitrag` legt
  `beitraege/<vorhaben>/<beitrag>.json` mit allen Feldern an
  (`id`, `vorhaben`, `schritt`, `von`, `seit`, `basis`, `notiz`, `dateien`,
  `entschieden: null`) und die vorgeschlagene Fassung daneben; das Original bleibt
  byte-gleich. Der Beitrag hängt nicht an `darf.aendern` — jeder darf beitragen.
  `/board/data` liefert `beitraege` mit Namen, Notiz und Dateien; die Seite zeigt
  ihn an der Station, im fremden Leser führt „Bearbeiten“ zu „Als Beitrag
  speichern“. Beleg: `test/schritt11.mjs` Exit 0, Tests 02–10 grün.
- **Nächster Schritt:** 12 — Der Verantwortliche entscheidet über den Beitrag.
- **Blocker:** keiner.
- **Nicht geprüft:** der Beitragsweg im Browser.

## Runde 10 — 2026-10-26

- **Stand:** Schritt 10 bestanden; Scheibe 2 vollständig (fremder Schritt läuft bei
  mir).
- **Nächster Schritt:** 11.
- **Nicht geprüft:** echte Sitzung und echte Modellanfrage.

## Runde 9 — 2026-10-26

- **Stand:** Schritt 09 bestanden, fremder Schritt als Verweis im eigenen Workflow.
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
