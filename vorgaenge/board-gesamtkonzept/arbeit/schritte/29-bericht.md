# Bericht Schritt 29 — Ein Vorhaben anlegen

## Gebaut

- **Zweig für Fassung 2 (`index.js`):** `POST /board/vorhaben` behandelt zuerst
  `AKTIV.format === 2`; die alte Ablage bleibt unverändert. Gelesen werden
  `name` (getrimmt; leer → 400, mehr als 120 Zeichen → 400) und `station`
  (fehlt → 400, keine Position im Workflow → 400). Ohne Board 409; ein nicht
  lesbares Board 409.
- **Recht, Eigentümer und Workflow:** Dieselbe Prüfung wie bei einer Station
  (Schritte 25/26): ohne Eigentümer 403 mit Grund und Hinweis, ohne
  `anlegen.darf` 403 mit dessen Grund und Hinweis. Der Workflow ist
  `board.workflows[0]`; fehlt er oder seine `fassung`, antwortet die Route 409.
- **Name und Kollision:** Die Repo-Kennung ist `vorhaben-<slug(name)>`. Vor dem
  Anlegen werden der lokale Zwischenspeicher und der Anlegeweg geprüft; belegt
  antwortet die Route 409 mit „Ein Vorhaben mit diesem Namen gibt es schon“.
- **Bauen (`index.js`):** `baueVorhaben` legt
  `$DSH_HOME/board/cache/<owner>/vorhaben-<slug>` an, schreibt `vorhaben.json`
  (Format 2, `id` = slug, `name`, `workflow: { repo, fassung }` aus dem Board,
  `position` = `station`, `verantwortliche: { [station]: [login] }` aus
  `liesLogin` — unbekannt: leere Liste —, `laeufe: []`, `farbe` der handelnden
  Person aus dem Board, sonst `#fbbf24`) und ein `README.md` mit Name, Station,
  Workflow und Thema. Danach `git init -b main`, `add`, `commit`; kein Remote,
  kein Token in `.git/config`.
- **Schicken (`index.js`):** Über denselben Anlegeweg wie Schritt 27
  (`selbst`/`attrappe`), Thema `dsh-board-vorhaben`. Die Fehlertexte des Weges
  nennen jetzt das richtige Ding (`Ein Vorhaben …`, `Das Vorhaben …`); der
  Stationsweg bleibt unverändert. Die Route ruft GitHub nicht selbst auf.
- **Eintragen (`index.js`):** Der Verweis `owner/repo` wird über den
  gemeinsamen `schreibeFassung2Verweis` in `board.vorhaben` aufgenommen (nicht
  doppelt), `board.json` wird geschrieben und als Stand gefasst (nur diese
  Datei). Die Antwort ist das Bild von `/board/data` plus
  `vorhaben: { owner, repo, id, name, position }`.
- **Anzeige (`client.js`):** An der Stationskarte steht „Vorhaben anlegen“,
  nur wenn `anlegen.darf` wahr ist. Darunter öffnet sich ein Formular mit Name,
  „Anlegen“, „Abbrechen“ und dem Fehler im Formular; die Antwort ersetzt den
  Stand, die Karte steht danach ohne Neuladen da. Der alte Knopf „+ Neues
  Vorhaben“ unter der Fläche entfällt in Fassung 2, weil er keinen Ort kennt;
  in der alten Ablage bleibt er.
- **Prüfung (`test/markt29.mjs`, neu):** Eigenes Temp-Heim und eigene Fläche
  unter `/tmp/board-scheibe1/markt29`: Station `station-start`, Workflow
  `workflow-standard` (`v1`) mit der Position `start`, Board mit
  `einstellungen: { eigentuemer: "testkonto", anlegeweg: "attrappe" }` und der
  Person `testkonto` in Farbe `#8b5cf6`. Geprüft wird:
  (a) Anlegen → 200, lokales Repository mit gültigem `vorhaben.json`
  (`position`, `workflow.fassung`, `farbe`, Verantwortliche), README, genau
  eine Fassung, Verweis genau einmal in `board.vorhaben`, Karte an der Station
  in `/board/data`, Board sauber gefasst;
  (b) derselbe Name → 409 mit Grund, kein zweites Repository, keine neue
  Fassung;
  (c) ohne Namen und ohne `station` → je 400, nichts angelegt;
  (d) ohne Eigentümer → 403 mit Grund und Hinweis; mit Eigentümer, aber ohne
  Anmeldung → 403 mit dem Grund des Anlegebildes, nichts angelegt;
  (e) `anlegeweg: "selbst"` mit nachgebildetem GitHub-Server:
  `GET /repos/...` (404 = frei), `GET /user`, `POST /user/repos` (201), Thema
  `dsh-board-vorhaben`, zwei Inhalte, Antwort 200; die gesendeten Dateien sind
  byte-gleich den lokalen;
  (f) kein Token in `.git/config`, kein Remote.
- **Live-Lauf im Test eingebaut, nicht ausgeführt:** `BOARD_LIVE=1` und
  `BOARD_LIVE_TOKEN` (nur im Prozess) legen unter `ThomasTestDSBOARD` ein
  Vorhaben an, prüfen Repository, Thema und Dateien über die API und löschen
  das Repository wieder. Ohne `BOARD_LIVE` wird dieser Weg nicht betreten; der
  Offline-Lauf benutzt nur Loopback (geschlossener Port bzw. nachgebildeter
  Server auf `127.0.0.1`).

## Geänderte Dateien

- `/workspace/projekte/board-plugin/index.js`
- `/workspace/projekte/board-plugin/client.js`
- `/workspace/projekte/board-plugin/test/markt29.mjs` (neu)

`formate/vorhaben.js` und `komposition.js` wurden gelesen, aber nicht geändert:
Das Format trägt die Felder bereits, und die Komposition löst `owner/repo`
seit Schritt 27 auf und zeigt die Karte an der Station.

## Befehl und Ergebnis

- `cd /workspace/projekte/board-plugin && node --test test/markt29.mjs`
  → Exit-Code 0
  - Entscheidende Zeile: `ok Vorhaben: an der Station, Karte, Kollision, ohne Recht`
- `cd /workspace/projekte/board-plugin && node --test` → Exit-Code 0;
  `tests 45`, `pass 45`, `fail 0` (die 44 vorhandenen plus `markt29`).
- `node --check index.js`, `node --check client.js`,
  `node --check test/markt29.mjs` → je Exit-Code 0.
- Der Live-Lauf wurde nicht ausgeführt; `BOARD_LIVE` war nicht gesetzt.
