# Prüfprotokoll

## Schritt 01 — bestanden

- **Befehl:** `cd /workspace/projekte/board-plugin && node --test test/schritt18.mjs` und `node --test`
- **Exit-Code:** 0 und 0
- **Entscheidende Zeile:** `ℹ pass 17`, `ℹ fail 0`
- **Befund:** Die vier Leser lesen die echten Formatdateien, prüfen Format 2, Pflichtfelder, Enums und Verweise; die neue Prüfung belegt die Werte der Station „Umsetzen“, der Positionen, des Vorhabens und des Boards und räumt die Ablage wieder ab. `repos.js` erkennt die vier Arten ohne Git; die alte Suite bleibt grün.

## Schritt 02 — bestanden

- **Befehl:** `cd /workspace/projekte/board-plugin && node --test test/schritt19.mjs` und `node --test`
- **Exit-Code:** 0 und 0
- **Entscheidende Zeile:** `ok Testboard: „Board Gesamtkonzept“ steht an „idee“; der Workflow trägt „Idee“ und „Umsetzen“`
- **Befund:** Die Prüfung ruft den in `index.js` registrierten Handler für `/board/data` auf, nicht die Komposition direkt. Die Fassung 1 bleibt unverändert; `test/schritt18.mjs` wurde nur im Ablageort und beim Aufräumen geändert, Inhalt und Prüfungen sind gleich. 18 von 18 Prüfungen bestehen.

