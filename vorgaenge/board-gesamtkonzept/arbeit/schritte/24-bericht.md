# Bericht Schritt 24 — Die alte Ablage abnehmen

## Gebaut

- **Register (`boards.js`):** Ein Board der Fassung 1 kann `archiv: true`
  tragen. Es heißt dann in der Anzeige „Archiv“; der gespeicherte Name bleibt
  als `nameAlt` erhalten. Ein Board der Fassung 2 wird auch mit `archiv`-Feld
  nicht als Archiv geführt.
- **Host (`index.js`):** Die gemeinsame Routen-Hülle prüft vor jedem Handler,
  ob das offene Board ein Archiv ist, und antwortet auf alle schreibenden
  `/board/*`-Routen mit **403 „Die alte Ablage ist Archiv“**. Ausgenommen sind
  nur Wechsel, Entfernen und Identität. `BOARD_ARCHIV=1` erzwingt die Abweisung
  für jede Fassung-1-Ablage, ohne die echte Ablage zu markieren; Fassung 2
  bleibt davon unberührt. `GET /board/data` eines Archivs liefert weiter
  Vorhaben und Dateien, meldet aber `archiv: true` und `darf: false`; die
  Handlungen an den Vorhaben sind gesperrt. Die Identitätsroute schreibt im
  Archiv nicht mehr in die alte Personendatei — die Identität liegt ohnehin
  außerhalb der Ablage.
- **Seite (`client.js`):** Ist das offene Board ein Archiv, zeigt die Leiste
  „Archiv“ und den Hinweis **„Nur lesen“**; Bearbeiten, Forken, neue Vorhaben
  und neue Dateien fehlen. Vorhaben und Dateien bleiben lesbar.
- **Prüfung (`test/schritt40.mjs`, neu):** Zwei alte Ablagen (eine markiert,
  eine nicht) und die Prüffläche der Fassung 2. Geprüft werden: lesbares Archiv
  samt Vorhaben und Dateien; `/board/boards` mit `archiv: true`, „Archiv“ und
  `nameAlt`; 403 für `/board/doc/schreiben` und `/board/beitrag` ohne neue
  Fassung; Wechsel auf Fassung 2 samt Schreibvorgang; `BOARD_ARCHIV=1` gegen
  das nicht markierte alte Board; Wechsel zurück auf das Archiv. Die Ablage
  wird am Ende entfernt.
- **Dokumente:** `README.md` beschreibt den alten Weg als Archiv, das Lesen
  bleibt, das Schreiben nicht; `umsetzung.md` ist auf diesen Stand gezogen.
- **Die echte alte Ablage bleibt unmarkiert.** Im echten Register steht kein
  `archiv`; `/workspace/projekte/board` behält `HEAD ae905d8`. Die Markierung
  erfolgt erst nach der Abnahme durch den Auftraggeber.

## Geänderte Dateien

- `/workspace/projekte/board-plugin/boards.js`
- `/workspace/projekte/board-plugin/index.js`
- `/workspace/projekte/board-plugin/client.js`
- `/workspace/projekte/board-plugin/test/schritt40.mjs` (neu)
- `/workspace/projekte/board-plugin/README.md`
- `/workspace/projekte/board/vorgaenge/board-gesamtkonzept/umsetzung.md`

## Befehl und Ergebnis

- `cd /workspace/projekte/board-plugin && node --test test/schritt40.mjs`
  → Exit-Code 0.
  - Entscheidende Zeile: `ok Archiv: lesbar, Schreiben 403 ohne Commit, Wechsel
    erlaubt, Fassung 2 schreibbar`
- `cd /workspace/projekte/board-plugin && node --test`
  → 40 von 40 bestanden, Exit-Code 0 (`tests 40`, `pass 40`, `fail 0`).
- `node --check index.js` → Exit-Code 0; `node --check client.js` → Exit-Code 0;
  `node --check boards.js` und `node --check test/schritt40.mjs` ebenfalls 0.
- Die Prüfung arbeitet ausschließlich unter `/tmp/board-scheibe1/schritt40/`
  und räumt selbst auf.

## Grenzen

- **Die echte alte Ablage ist noch nicht als Archiv markiert.** Der Schalter
  ist gebaut und mit `BOARD_ARCHIV=1` belegt; im Normalbetrieb bleibt das echte
  alte Board bis zur Markierung beschreibbar.
- **Kein Browserdurchgang.** Die Anzeige ist über den Client-Quelltext und
  `node --check` belegt, nicht mit Maus und Augen im laufenden Harness.
- **Keine Commits im Produkt.** Der Arbeitsbaum trägt weiterhin unversionierte
  und geänderte Dateien; `origin/main` bleibt auf `52b054b`.
- Das echte `$DSH_HOME` und die echten Repositories wurden gelesen, nicht
  geschrieben; das echte Register trägt kein `archiv`.
