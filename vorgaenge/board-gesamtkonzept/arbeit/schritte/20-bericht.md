# Bericht Schritt 20 — Forken und Fragen

## Gebaut

- `github.js`: `fragen(owner, name, {state:'all'})` liest offene und geschlossene Issues und lässt Pull Requests weg; `zustand` steht als „offen“/„geschlossen“ da. `erstelleFrage(owner, name, {titel, text})` legt ein Issue an und liefert dieselbe Form. `erstelleFork(owner, name)` liefert `{vollerName, adresse, eigentuemer}`.
- `index.js`: drei Routen, in der Routenübersicht eingetragen:
  - `GET /board/github/fragen?repo=` — Fragen mit Nummer, Titel, Zustand, Autor und Adresse; `kann` sagt, ob ein Token vorliegt. Ohne Token ist ein öffentliches Repository lesbar; antwortet GitHub mit 404, kommt 200 mit `{repo, adresse, kann:false, fragen:[], hinweis}`.
  - `POST /board/github/frage` — nur mit Token, Antwort `{nummer, adresse}`; ohne Token 409 mit Adresse und Verweis. Fremde Repositories erhalten 403 über `boardPfadFuer` vor jedem Aufruf bei GitHub.
  - `POST /board/github/fork` — nur mit Token, Antwort `{fork}`; ohne Token 409 mit Adresse. Dieselbe Zugehörigkeitsprüfung steht davor.
  - Beide POSTs stehen in `OHNE_ABLAGE_SCHREIBEN`: Fork und Frage sind GitHub-Handlungen, keine Ablagedateien.
- `client.js`: die Komponente `FragenFlaeche`. Sie zeigt je Frage `#Nummer · Titel`, den Zustand und „Ansehen“; mit Token den Knopf „Frage stellen“ samt Formular für Titel und Text, ohne Token den Verweis „Auf GitHub fragen“. Darunter der Fork: mit Token „Forken“, nach dem Fork „Deine Kopie: …“ als Verweis auf den Fork, ohne Token der GitHub-Verweis. Die Fläche steht in der Einzelheit von Station, Workflow und Vorhaben neben den Beiträgen. Kein lokaler Klon, keine zweite Ablage.
- `test/schritt37.mjs`: Board der Fassung 2 mit Station, Workflow und Vorhaben sowie ein fremdes Repository; nachgebildeter GitHub-Server mit Fork, Issue anlegen und Fragenliste in allen Zuständen, in der zusätzlich ein Pull Request steht. Geprüft werden: ohne Token `kann:false` mit Adresse und 409 ohne Schreibaufruf; fremde Repositories 403 ohne Aufruf; Fork `anna/station-idee` verlinkt; Frage #1 angelegt und offen, ohne den Pull Request; nach dem Schließen geschlossen sichtbar; kein Commit und keine Änderung in den lokalen Repositories.

## Geänderte Dateien

- `/workspace/projekte/board-plugin/github.js`
- `/workspace/projekte/board-plugin/index.js`
- `/workspace/projekte/board-plugin/client.js`
- `/workspace/projekte/board-plugin/test/schritt37.mjs` (neu)

## Befehl und Ergebnis

- `cd /workspace/projekte/board-plugin && node --test test/schritt37.mjs` → Exit-Code 0.
- Entscheidende Zeile: `ok Fork und Frage: ohne Token Verweis, Fork verlinkt, Frage #1 offen, geschlossen sichtbar`
- `node --test` → 36 von 36 bestanden, Exit-Code 0.
- `node --check index.js` → 0; `node --check client.js` → 0; `node --check github.js` → 0.

## Grenzen

- Kein echter GitHub-Aufruf; der Testserver in `test/schritt37.mjs` antwortet lokal.
- Keine Commits im Produkt; die Prüfablage unter `/tmp/board-scheibe1/schritt37` entfernt die Prüfung selbst.
- Die Zugehörigkeitsprüfung aus Schritt 18 (`boardPfadFuer`) und die Verantwortungsprüfung aus Schritt 19 bleiben unverändert; die Prüfungen `schritt35` und `schritt36` sind weiter grün.
