# Prüfprotokoll

## Schritt 01 — bestanden

- **Befehl:** `cd /workspace/projekte/board-plugin && node --test test/schritt18.mjs` und `node --test`
- **Exit-Code:** 0 und 0
- **Entscheidende Zeile:** `ℹ pass 17`, `ℹ fail 0`
- **Befund:** Die vier Leser lesen die echten Formatdateien, prüfen Format 2, Pflichtfelder, Enums und Verweise; die neue Prüfung belegt die Werte der Station „Umsetzen“, der Positionen, des Vorhabens und des Boards und räumt die Ablage wieder ab. `repos.js` erkennt die vier Arten ohne Git; die alte Suite bleibt grün.
