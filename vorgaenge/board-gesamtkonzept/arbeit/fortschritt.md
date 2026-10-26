# Fortschritt

## Runde 12 — 2026-10-26

- **Stand:** Schritt 12 bestanden. **Scheibe 3 ist vollständig: beitragen und
  annehmen.** `POST /board/beitrag/entscheiden` prüft `darf(…, "entscheiden")`
  (403), lehnt einen zweiten Entscheid ab (409) und einen unbekannten Beitrag
  (404). Annehmen schreibt die vorgeschlagenen Dateien in den Vorhabenordner und
  hält sie als eine Fassung mit eigenem Urheber fest; Teile übernehmen nur die
  genannten; Ablehnen ändert nichts. `entschieden = { von, am, art, teile }` steht
  am Beitrag; die Seite zeigt ihn und bietet die Handlungen nur dem
  Verantwortlichen. Beleg: `test/schritt12.mjs` Exit 0, Tests 02–11 grün.
- **Nächster Schritt:** 13 — Zweig und Fork tragen ihre Herkunft.
- **Blocker:** keiner.
- **Nicht geprüft:** der Entscheid im Browser.

## Runde 11 — 2026-10-26

- **Stand:** Schritt 11 bestanden, Beiträge unter `beitraege/`.
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
