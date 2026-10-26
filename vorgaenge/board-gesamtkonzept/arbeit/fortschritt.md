# Fortschritt

## Runde 9 — 2026-10-26

- **Stand:** Schritt 09 bestanden. Ein fremder Schritt lässt sich über
  `/board/fremd/anstellen` als `{ref, id}` samt Position in den eigenen Workflow
  stellen; die fremde Datei bleibt byte-gleich, der Schritt wird nicht kopiert.
  In der eigenen Fläche erscheint er mit „bei ‹Person›“, Ein- und Ausgang, ohne
  Einstellungsformular; Ändern und Löschen antworten 403. Ein Vorhaben kann auf der
  Verweis-Position stehen; Herausnehmen geht erst, wenn keines mehr darauf steht
  (409). Beleg: `test/schritt09.mjs` Exit 0, Tests 02–08 grün.
- **Nächster Schritt:** 10 — Der Skill der anderen Person läuft in meinem Harness.
- **Blocker:** keiner.
- **Bevorstehendes Risiko:** Schritt 10 ist im Bauplan als Stopp markiert: fremde
  Anweisungen laufen dann im eigenen Harness. Die Prüfung belegt den Weg, nicht
  den Inhalt fremder Anweisungen — das bleibt eine Vertrauensfrage.

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
