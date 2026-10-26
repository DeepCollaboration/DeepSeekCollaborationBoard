# Fortschritt

## Runde 3 — 2026-10-26

- **Stand:** Schritt 03 bestanden. Die Ablage hat Format 1: `format`,
  `personen/p1.json`, `workflows/p1.json`, fünf `vorgaenge/*/vorhaben.json`. Das
  Modul liest und schreibt nur noch diese Dateien (`produkt/ablage.js`), `board.json`
  wird nicht mehr gelesen. Belege: `schritt03.mjs` Exit 0 (5 Vorhaben, 11
  Positionen, 11 Schritte, ohne `board.json` in der Kopie), Fassung
  „Umzug auf Format 1“ von Thomas Heinrich.
- **Nächster Schritt:** 04 — Der Abgleich holt und gibt.
- **Blocker:** keiner.
- **Beobachtung:** Das laufende alte Board wird im Fenster benutzt (Layout „frei“,
  Positionen verschoben). Solche Änderungen landen in `board.json` und in den neuen
  Dateien nicht. Vor dem Umschalten wird der Umzug mit dem letzten `board.json`
  aufgefrischt (siehe `entscheidungen.md`).
- **Persönliche Seiten:** Einstellungen und Gesprächsverweise liegen jetzt unter
  `$DSH_HOME/board/` (`einstellungen.json`, `gespraeche.json`).

## Runde 2 — 2026-10-26

- **Stand:** Schritt 02 bestanden. Die Ablage `projekte/board/` ist ein eigenes
  Repository, `/workspace` führt sie nicht mehr, `produkt/board.config.json` nennt
  Person und Ort.
- **Nächster Schritt:** 03.

## Runde 1 — 2026-10-26

- **Stand:** Plan angelegt, Schritt 01 bestanden (Kopie byte-gleich, Profil zeigt
  auf `produkt/`, laufendes Board antwortet mit 200).
- **Nächster Schritt:** 02.
