# Fortschritt

## Runde 5 — 2026-10-26

- **Stand:** Schritt 05 bestanden. `/board/data` liefert `workflows` als Flächen je
  Person, eigene zuerst; fremde Schritte tragen `host` und `fremd: true`; Verweise
  `{ref}` werden aufgelöst statt kopiert, ein fehlendes Ziel erscheint als fehlend;
  die Seite hat je Person eine Fläche, die fremde nur lesend; `/board/schritt/aendern`
  auf einen fremden Schritt antwortet 403. Beleg: `test/schritt05.mjs` Exit 0;
  `workflows/p1.json` byte-gleich, echte Ablage ohne zweite Person.
- **Nächster Schritt:** 06 — Jedes Vorhaben nennt seinen Verantwortlichen.
- **Blocker:** keiner.
- **Nicht geprüft:** Die fremde Fläche ist im Code belegt, nicht im laufenden
  Browser gesehen (kein Neustart möglich).
- **Hinweis:** Die Ablage ist nach „Fremde Workflows“ wieder schmutzig
  (Prüfprotokoll, Plan, Fortschritt); wird mit Schritt 06 festgehalten.

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
