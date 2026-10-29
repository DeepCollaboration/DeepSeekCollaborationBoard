# Bericht Schritt 21 — Das Probeprojekt

## Gebaut

- **Probeprojekt-Repository** `/workspace/projekte/board-repos/vorhaben-probeprojekt`:
  - `vorhaben.json` mit `id: 'probeprojekt'`, Name „Probeprojekt“, `workflow: {repo:'workflow-standard', fassung:'v1'}`, `position:'idee'`, `verantwortliche: {idee:['DeepCollaboration']}`, `laeufe: []`, `fork_von: null`, `zweig_von: null`, `farbe:'#38bdf8'`.
  - `idee.md` mit der Einladung in drei Abschnitten: was das Board ist, was hier passiert, was der nächste Schritt ist — sachlich, ohne Werbetext.
  - `README.md` mit dem Weg „Übernehmen → eigenes Board → an der Idee arbeiten“ und den drei Schritten.
  - Ein Commit, Tag `v1`; `git log -1 --stat` nennt genau diese drei Dateien.
- **Verweise im Hauptboard**: `board.json` führt `vorhaben: [{repo:'vorhaben-probeprojekt'}]`; ein Commit ändert nur `board.json`. `katalog.json` führt denselben Eintrag unter `vorhaben`; ein Commit ändert nur `katalog.json`. Kein Push.
- **Route `POST /board/uebernehmen`** in `index.js`:
  - Quelle ist der übergebene Ordner (`repo`) oder das Probeprojekt des Hauptboards; das Hauptboard wird wie im Katalog über `art:'hauptboard'` und `format:2` aus dem Register bestimmt, das Probeprojekt über den Vorhaben-Verweis seiner `board.json`. Fehlt der Name, antwortet die Route 409 `{fehlt:'name'}`, bevor irgendetwas entsteht.
  - Ziel ist `$DSH_HOME/board/ablagen/<kennung>/`. Die Kennung entsteht aus dem Boardnamen und weicht vorhandenen Boards und Ordnern aus. Die `board.json` trägt eigene Kennung, Name (`boardName` oder „<Name>s Board“), `art:'lokal'`, die eigene Person (Login oder Kennung, Name, Farbe), Workflows und Stationen als absolute Verweise auf die Repositories des Hauptboards und `vorhaben` auf die Kopie.
  - Die Kopie liegt unter `<board>/vorhaben-<id>-kopie`, ihre Kennung ist `<id>-kopie`. Alle Dateien außer `.git` und der alten `vorhaben.json` werden mitkopiert; die neue `vorhaben.json` behält den Namen, setzt `workflow.repo` auf den absoluten Pfad des Original-Workflows, `fork_von: {repo:'vorhaben-probeprojekt', fassung:'v1'}` (Fassung aus dem Tag des Originals, sonst ohne erfundene Fassung) und an jeder vorhandenen Position den eigenen Login, sonst die Kennung. Die übrigen Angaben bleiben stehen.
  - Board und Kopie werden als eigene Git-Repositories angelegt und committet. Ein `.gitignore` mit `vorhaben-*/` hält die Kopie aus dem Board-Repository heraus; ihre Geschichte gehört in ihr eigenes Repository.
  - Danach trägt `nimmBoardAuf` das Board ins Register ein und öffnet es (`setzeAktiv`). Antwort: `{board, vorhaben}`. Das Original wird nur gelesen; die Route steht in `OHNE_ABLAGE_SCHREIBEN`, weil sie keine Datei der offenen Ablage schreibt.
- **Oberfläche** in `client.js`: Die Karte des Probeprojekts im Katalog trägt „In mein Board übernehmen“. Beim Klick wird die Identität gelesen; fehlt der Name, erscheint das kleine Formular (Name, GitHub-Name, Farbe) und darunter der Knopf. Der Knopf trägt die Identität nach, ruft die Route auf und lädt die Seite neu; danach ist das eigene Board offen und zeigt die Kopie an „Idee“. Fehler stehen unter dem Knopf.
- **`test/schritt38.mjs`**: Hauptboard mit Station, Workflow und Probeprojekt, Server in-process, eigenes Home. Geprüft werden: ohne Namen 409 `fehlt:name` und kein Board; mit Namen ein lokales Board im Register, das geöffnet ist; `board.json` mit Person, absoluten Workflow- und Stationsverweisen und Verweis auf die Kopie; Kopie vorhanden, neue Kennung, gleicher Name, Workflow auf das Original, `fork_von` gesetzt, eigener Login verantwortlich, Dateien mitkopiert; beide Repositories mit genau einer Fassung und ohne offene Änderungen; Original-`HEAD` und Stand unverändert, Hauptboard unverändert; `/board/data` des neuen Boards zeigt die Kopie an „Idee“.

## Geänderte Dateien

- `/workspace/projekte/board-plugin/index.js`
- `/workspace/projekte/board-plugin/client.js`
- `/workspace/projekte/board-plugin/test/schritt38.mjs` (neu)
- `/workspace/projekte/board-repos/vorhaben-probeprojekt/vorhaben.json` (neu)
- `/workspace/projekte/board-repos/vorhaben-probeprojekt/idee.md` (neu)
- `/workspace/projekte/board-repos/vorhaben-probeprojekt/README.md` (neu)
- `/workspace/projekte/board-repos/board-hauptboard/board.json`
- `/workspace/projekte/board-repos/board-hauptboard/katalog.json`

## Befehl und Ergebnis

- `cd /workspace/projekte/board-plugin && node --test test/schritt38.mjs` → Exit-Code 0.
  - `ok Probeprojekt: ohne Name 409, übernommen, Kopie verweist aufs Original, Original unverändert`
- `node --test` → 38 von 38 bestanden, Exit-Code 0.
- `node --check index.js` → 0; `node --check client.js` → 0.
- `git -C /workspace/projekte/board-repos/vorhaben-probeprojekt log -1 --stat` → Exit-Code 0; nennt `README.md`, `idee.md`, `vorhaben.json`; Tag `v1` liegt auf dem Commit.
- `git -C /workspace/projekte/board-repos/board-hauptboard show --stat HEAD~1` → nur `board.json`; `git show --stat HEAD` → nur `katalog.json`.
- Eine Probe mit dem echten Hauptboard (temporäres Home unter `/tmp/board-scheibe1`, danach entfernt) zeigte das Probeprojekt an „Idee“, den Katalogeintrag und eine Übernahme, die das eigene Board mit der Kopie an „Idee“ öffnet.

## Grenzen

- Keine Commits im Produktordner, kein Push. Die vier vorhandenen Vorhaben wurden nur gelesen.
- Das Hauptboard-Repository wurde nur um die beiden genannten Verweise ergänzt. Seine `README.md` sagt weiterhin „Vorhaben: —“, weil nur die genannte Datei geändert werden durfte.
- Die gemeinsame `sichtbarkeit.json` im Ordner `board-repos` wurde nicht ergänzt (sie lag außerhalb des Auftrags). Nach der Namensregel gilt `vorhaben-*` dort als privat; `/board/data` des Hauptboards meldet deshalb eine Sichtbarkeitslücke „Hauptboard ist öffentlich und verweist auf Probeprojekt, das privat ist“. Der Katalog führt das Probeprojekt als öffentlichen Eintrag; die Übernahme ist von der Lücke nicht betroffen. Eine spätere Runde kann die Karte ergänzen.
- Der Test bildet GitHub nicht nach und braucht kein Netz; er arbeitet auf einer Prüffläche unter `/tmp/board-scheibe1/schritt38` und entfernt sie selbst.
- Die Oberfläche wurde über `node --check` und die Datenwege geprüft, nicht in einem echten Browser durchgeklickt.
- Das neue Board-Repository ignoriert `vorhaben-*/`; die Kopie ist damit ausschließlich im eigenen Repository versioniert. Wer das Board-Repository allein kopiert, bekommt den Verweis, aber nicht die Kopie — das entspricht der Trennung von Board und Vorhaben.
