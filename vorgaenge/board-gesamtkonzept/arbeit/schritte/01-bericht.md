# Bericht Schritt 01 — Das Board wird ein Produkt dieses Vorhabens

- **Kopiert:** `/workspace/plugins/dsh-board/` → `/workspace/projekte/board/vorgaenge/board-gesamtkonzept/produkt/`
  (gesamter Inhalt inklusive `locale/`, mit `cp -a`; Exit 0).
- **Dateien im Ziel (7):**

  | Datei | Bytes |
  | --- | ---: |
  | `package.json` | 517 |
  | `index.js` | 94164 |
  | `client.js` | 227249 |
  | `cordis.patch.yml` | 63 |
  | `icon.svg` | 540 |
  | `locale/de.json` | 142 |
  | `locale/en.json` | 142 |

- **Original unverändert:** `git status --porcelain -- plugins/dsh-board` ohne
  Ausgabe (Exit 0); nichts geschrieben, nichts gelöscht.

## Prüfungen

1. `diff -r /workspace/plugins/dsh-board /workspace/projekte/board/vorgaenge/board-gesamtkonzept/produkt`
   → keine Ausgabe, **Exit 0**. Zusätzlich SHA-256 und Dateimodi je Datei
   verglichen: identisch (Exit 0).
2. `find .../produkt -type f` → **7** Dateien.
3. Export-Prüfung mit `node -e` gegen `produkt/package.json` → **Exit 0**:

   ```
   ok ./index.js
   ok ./client.js
   ok ./locale/*.json (Muster, locale/ vorhanden)
   ok ./locale/de.json
   ok ./locale/en.json
   ok ./icon.svg
   ```

   Der Einzeiler aus dem Auftrag lief bei `./locale/*.json` in `ENOENT`
   (`statSync` auf den wörtlichen Stern). Gemäß Auftrag wurde dieser Eintrag
   übersprungen und stattdessen `locale/` sowie `locale/de.json` und
   `locale/en.json` geprüft.

**Entscheidende Zeile:** `diff -r` ohne Ausgabe und Exit 0 — die Kopie ist
byte-gleich mit dem Original.

**Grenzen eingehalten:** nur `produkt/` angelegt und gefüllt; `plugins/dsh-board/`
nicht verändert; `/dsh-home` nicht angefasst; keine Commits; keine Rückfrage.
