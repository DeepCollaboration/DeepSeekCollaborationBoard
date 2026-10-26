# Fortschritt

## Runde 14 — 2026-10-26

- **Stand:** Schritt 14 bestanden. Ein Rücksprung steht als
  `{ von, nach, grund, seit, erledigt }` am Vorhaben; das Zurückschicken geht
  durch `darf(…, "zurueckschicken")` und verlangt Grund und vorhandene
  Zielstation. `/board/data` liefert die Einträge samt Zielnamen und Grund, die
  Seite zeigt nur die offenen. Das Weiterziehen über das Ziel setzt
  `erledigt: true`; der Eintrag bleibt in der Datei. Beleg:
  `test/schritt14.mjs` Exit 0, Tests 02–13 grün, echte Ablage ohne Rücksprung
  und ohne zweite Person.
- **Nächster Schritt:** 15 — Speichern auf einem überholten Stand schreibt nicht.
- **Blocker:** keiner.
- **Nicht geprüft:** die Rücksprung-Anzeige im Browser.

## Runde 13 — 2026-10-26

- **Stand:** Schritt 13 bestanden. Abzweigen schreibt `zweig_von = { von, station,
  datum }`; die neue Route `POST /board/vorhaben/forken` legt ein eigenes Vorhaben
  mit `fork_von`, kopierten Dateien und eigener Verantwortung an — auch aus einem
  fremden Vorhaben, ohne dessen Ordner anzufassen. `/board/data` liefert die
  Herkunft samt Namen; die Seite zeigt „Zweig von … ab …“ und „Fork von …“.
  Beleg: `test/schritt13.mjs` Exit 0, Tests 02–12 grün, echte Ablage und die zwei
  vorhandenen Zweige unverändert.
- **Nächster Schritt:** 14 — Der Rücksprung steht am Vorhaben.
- **Blocker:** keiner.
- **Nicht geprüft:** die Herkunftsanzeige und der Forken-Knopf im Browser.

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
