# Fortschritt

## Runde 15 — 2026-10-26

- **Stand:** Schritt 15 bestanden. **Scheibe 5 ist fast vollständig.** `GET
  /board/doc` liefert `basis` dateigenau (letzter Commit der Datei; `null` bei
  ungefassten Dateien). `POST /board/doc/schreiben` weist einen überholten Stand
  mit 409 und `{ ok: false, neu: { text, basis } }` ab, ohne zu schreiben; die
  Seite behält den eigenen Text, zeigt den neuen Stand und bietet „Eigenen Text
  als Beitrag anlegen“ (über `/board/beitrag` mit der neuen Basis). Nichts wird
  zusammengeführt. Beleg: `test/schritt15.mjs` Exit 0 (12 ok-Zeilen), Tests 02–14
  grün, echte Ablage unberührt.
- **Nächster Schritt:** 16 — Zwei Programmfassungen schreiben nicht gegeneinander.
- **Blocker:** keiner.
- **Nicht geprüft:** der Konfliktweg im Browser.

## Runde 14 — 2026-10-26

- **Stand:** Schritt 14 bestanden; Scheibe 4 vollständig (Herkunft und Rückweg).
  Rücksprung als Vermerk mit Ziel und Grund, erledigt beim Vorbeiziehen.

## Runde 13 — 2026-10-26

- **Stand:** Schritt 13 bestanden, `zweig_von` und `fork_von` samt Anzeige.

## Runde 12 — 2026-10-26

- **Stand:** Schritt 12 bestanden; Scheibe 3 vollständig (beitragen und annehmen).

## Runde 11 — 2026-10-26

- **Stand:** Schritt 11 bestanden, Beiträge unter `beitraege/`.

## Früher

- **Runde 10:** Schritt 10, fremder Skill läuft im eigenen Harness; Scheibe 2
  vollständig.
- **Runde 9:** Schritt 09, fremder Schritt als Verweis im eigenen Workflow.
- **Runde 8:** Schritt 08, Schritt-Anweisungen in der Ablage.
- **Runde 7:** Schritt 07, fremde Vorhaben lesbar; Scheibe 1 vollständig.
- **Runde 6:** Schritt 06, Verantwortliche und eine Prüfstelle `darf`.
- **Runde 5:** Schritt 05, eine Fläche je Person.
- **Runde 4:** Schritt 04, Abgleich mit `stand`; die Ablage hat keinen `origin`.
- **Runde 3:** Schritt 03, Ablage auf Format 1.
- **Runde 2:** Schritt 02, eigenes Repository mit eigener Person.
- **Runde 1:** Schritt 01, das Plugin liegt in `produkt/`.
