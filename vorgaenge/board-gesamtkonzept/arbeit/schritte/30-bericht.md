# Bericht Schritt 30 — Dateien speichern

## Gebaut

- **Zweig für Fassung 2 (`index.js`):** `POST /board/doc/schreiben` prüft nach
  dem Lesen des Körpers und **vor** `liesBoard()`, ob das offene Board Fassung 2
  ist; dann läuft `speichereImVorhaben(koerper, res)`. Der bisherige Weg für die
  alte Ablage steht unverändert darunter und wird nicht betreten.
- **Vorhaben und Ordner:** Das Vorhaben wird über `koerper.vorhaben` (id) im
  Bild aus `komponiere` gesucht; nicht gefunden → 404 „Vorhaben nicht
  gefunden“. Sein Ordner kommt aus `vorgang.ordner` (so heißt das Feld im Bild,
  nicht `pfad`); fehlt er oder ist er kein Ordner → 409 mit Grund. Ein
  unlesbares `board.json` antwortet 409, bevor etwas entschieden wird.
- **Recht:** Das Bild der Fläche entsteht mit
  `fassung2Bild(..., await schreibrechtFuerBoard())`. Ist das Board gesperrt →
  403 mit `darfGrund` und ggf. `darfHinweis`. Ist die handelnde Person nicht
  verantwortlich (`vorgang.darf.aendern` falsch) → 403 „Dieses Vorhaben
  verantwortet jemand anderes.“. Zuletzt prüft `schreibrecht({ pfad: ordner,
  home: HOME })` das Repository des Vorhabens; ein Remote ohne Token → 403 mit
  `GRUND_OHNE_TOKEN` und Hinweis. `darfSchreiben` ist dieselbe Prüfung, liefert
  aber keinen Grund; genommen wird deshalb `schreibrecht`. Alle Prüfungen stehen
  vor dem Schreiben.
- **Pfad:** `dateiImVorhaben` weist absolute Pfade (auch `C:` und `\\`), `..` in
  jedem Teil, leere Namen und einen Ordner statt Datei mit 400 ab. Danach macht
  `dateiName` den sicheren Namen, `pfadImOrdner` prüft, dass der aufgelöste Pfad
  mit dem Vorhaben-Ordner beginnt. Ein Schrägstrich am Ende bleibt ein Ordner
  und wird abgewiesen.
- **Basis:** `dateiBasis` hat einen optionalen dritten Parameter `repo`
  (Standard: die Ablage). Die Prüfung ist die vorhandene: fehlende Basis bei
  einer gefassten Datei, ungültige oder abweichende Basis → 409 mit
  `neu: { text, basis }`; geschrieben wird nichts. Zusätzlich liefert
  `GET /board/doc` die Basis jetzt aus dem Repository des Vorhabens
  (`repoFuerVorgang`); ohne diese Stelle bekäme der Editor für jede gefasste
  Fassung-2-Datei `null` und könnte sie nie speichern.
- **Schreiben und fassen:** atomar `tmp` + `rename`; danach im
  **Vorhaben-Repository** `git add -- <datei>` und `git commit` mit
  `autorFassung2()` (die handelnde Person) und der Meldung „<datei>
  gespeichert“. Ist der Text unverändert, entsteht kein leerer Commit. Kein
  Commit im Board-Repository, kein Push, kein `festhalten` (das würde die
  Ablage fassen und weitergeben).
- **Antwort:** wie bisher `{ ok, name, laenge, basis, fassung }`; `basis` ist
  die Fassung der Datei nach dem Commit.
- **Prüfung (`test/markt30.mjs`, neu):** Eigenes Temp-Heim und eigene Fläche
  unter `/tmp/board-scheibe1/markt30`. Das Board der Fassung 2 verweist auf eine
  Station, einen Workflow (`v1`) und zwei Vorhaben-Repositories im
  Zwischenspeicher: `vorhaben-markt30` (verantwortet von `testkonto`) und
  `vorhaben-fremd` (verantwortet von `jemand-anderes`). Geprüft wird:
  (a) `/board/doc` liefert die Basis aus dem Vorhaben-Repository; Speichern mit
  dieser Basis → 200, neue Basis, neue Fassung mit „Testperson“ als Autor und
  „gespeichert“, Datei nach erneutem Lesen geändert, Vorhaben-Repository genau
  eine Fassung mehr, Board-Repository unverändert und sauber;
  (b) `..`, `unter/../../ausbruch.md`, absolute Pfade (`/etc/passwd`, ein Pfad
  in die Fläche) und ein Ordner statt Datei → je 400, nichts geschrieben, keine
  Fassung, kein Ausbruch;
  (c) unbekannte id → 404; fremdes Vorhaben → 403 mit Grund, nichts
  geschrieben;
  (d) Vorhaben mit Remote ohne Token → 403 mit Grund („Token“/„Anmeldung“),
  nichts geschrieben oder gefasst, danach Remote wieder entfernt;
  (e) von außen gefasster neuer Stand → 409 mit `neu.text` und `neu.basis`,
  Datei byte-gleich, keine Fassung; mit der neuen Basis geht dasselbe Speichern
  danach durch. Kein echtes Netz: `DSH_GITHUB_API` zeigt auf einen
  geschlossenen Port.

## Geänderte Dateien

- `/workspace/projekte/board-plugin/index.js`
- `/workspace/projekte/board-plugin/test/markt30.mjs` (neu)

`client.js`, `komposition.js`, `rechte.js` und die Formate wurden gelesen, aber
nicht geändert.

## Befehl und Ergebnis

- `cd /workspace/projekte/board-plugin && node --test test/markt30.mjs`
  → Exit-Code 0
  - Entscheidende Zeile:
    `ok Datei: im Vorhaben, Commit, Pfad abgewiesen, Basis`
- `cd /workspace/projekte/board-plugin && node --test` → Exit-Code 0;
  `tests 46`, `pass 46`, `fail 0` (die 45 vorhandenen plus `markt30`).
- `node --check index.js` → Exit-Code 0;
  `node --check test/markt30.mjs` → Exit-Code 0.

## Grenzen

- **Kein echtes Netz.** Der Remote-Fall ändert nur `.git/config`; es wird kein
  GitHub gefragt. Der Testserver ist ein geschlossener Port auf `127.0.0.1`.
- **Kein Push.** Das Festschreiben bleibt lokal; der Push gehört zu Schritt 39.
- **Der laufende Server** trägt den älteren Stand und wurde nicht benutzt;
  geprüft wurde mit `node --test`.
- **Keine eigenen Commits.** Die Testrepositories liegen unter
  `/tmp/board-scheibe1/markt30` und werden am Ende entfernt.
