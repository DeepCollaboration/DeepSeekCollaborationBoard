# Bericht Schritt 22 — Die frische Installation

## Gebaut

- **`README.md`** vollständig neu geschrieben für die Fassung 2:
  - was das Board ist: Stationen, Workflows, Vorhaben und Boards als eigene
    Repositories; ein Board als Repository voller Verweise;
  - Installieren im DSH-Profil mit
    `dsh plugin --profile web add github:DeepCollaboration/DeepSeekCollaborationBoard-Plugin`;
    kein Eintragen von Hand in `dsh.profile.bundles` mehr;
  - erster Start: das Hauptboard kommt von selbst in den Zwischenspeicher, seine
    Verweise werden mitgeholt; ohne Netz „nicht geholt“ mit Wiederholen;
  - Lesen ohne Namen; der Name kommt erst beim Beitreten;
  - eigenes Board über das Probeprojekt (Katalog → „In mein Board übernehmen“),
    lokale Ablage, Kopie mit `fork_von`, Verweise statt Kopien;
  - GitHub-Token als optional: was er zusätzlich kann, wo er liegt
    (`$DSH_HOME/board/github.json`, 0600), wie er gelöscht wird, dass er keinen
    Git-Zugang ersetzt;
  - Beitrag als Pull Request, Fork und Frage als Issue: mit Token im Board, ohne
    Token über die verlinkten GitHub-Wege; entschieden wird auf GitHub;
  - Tabelle der lokalen Dateien unter `$DSH_HOME/board/` und die Form der Ablage.
- **`package.json`**: `files` enthält `github.js`, `katalog.js`, `rechte.js`,
  `erststart.js` und `werkzeuge/`; die Liste war bereits vollständig und wurde
  nicht verändert. `description` und `version` blieben unverändert.
- **`test/schritt39.mjs`** (neu): frisches Home ohne `boards.json` und ohne
  `identitaet.json`, lokale Bare-Repositories für Hauptboard, Station, Workflow
  und Probeprojekt als `DSH_BOARD_HAUPTBOARD`/`DSH_BOARD_REMOTE_BASIS`, Server
  in-process, kein echter GitHub-Aufruf. Geprüft werden:
  1. `/board/data` zeigt das Hauptboard mit Station „Idee“, dem Schritt im
     Workflow und dem Probeprojekt an „Idee“, ohne Lücken.
  2. `/board/boards` zeigt das Hauptboard als offenes Hauptboard und
     `identitaet: null`.
  3. Übernehmen ohne Namen antwortet 409 mit `fehlt: 'name'`; es entsteht keine
     Ablage.
  4. `POST /board/identitaet` setzt „Anna“ und schreibt `identitaet.json`.
  5. Übernehmen legt `annas-board` an, öffnet es, kopiert das Probeprojekt als
     eigenes Repository und trägt beide Boards ins Register ein.
  6. Die Anzeige listet „Hauptboard“ und „Annas Board“, das eigene Board ist
     offen, die Identität steht da.
  7. Das eigene Board zeigt die Kopie an „Idee“.
  8. Die README nennt „Hauptboard“, „Probeprojekt“ und den Token als optional;
     `package.json.files` trägt die geforderten Einträge.
- **`client.js`** wurde nicht verändert: der frische Weg über die vorhandenen
  Routen und die vorhandene Oberfläche trägt; der Test braucht keine Änderung.

## Geänderte Dateien

- `/workspace/projekte/board-plugin/README.md`
- `/workspace/projekte/board-plugin/test/schritt39.mjs` (neu)

`package.json` wurde geprüft, aber nicht geändert; `client.js` blieb unverändert.

## Befehl und Ergebnis

- `cd /workspace/projekte/board-plugin && node --test test/schritt39.mjs` →
  Exit-Code 0.
  - `ok Frische Installation: Hauptboard ohne Einrichten, Name erst beim
    Beitreten, eigenes Board, README aktuell`
- `node --test` → 39 von 39 bestanden, Exit-Code 0.
- `node --check index.js` → 0; `node --check client.js` → 0.

## Grenzen

- Kein echter GitHub-Aufruf und kein Netz: der Erststart wird mit lokalen
  Bare-Repositories in-process nachgebildet.
- Kein echtes frisches DSH-Profil und kein Browser: geprüft werden die Routen
  und Datenwege; die Oberfläche nur über `node --check`.
- Der Test arbeitet auf einer Kopie des Produkts ohne `board.config.json` unter
  `/tmp/board-scheibe1/schritt39` und entfernt sie am Ende. Das echte
  `$DSH_HOME` und die echten Repositories wurden nicht angefasst.
- Keine Commits, kein Push.
