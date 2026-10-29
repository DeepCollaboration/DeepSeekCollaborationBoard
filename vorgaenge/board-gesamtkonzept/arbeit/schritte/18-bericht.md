# Bericht Schritt 18 — Beiträge als Pull Requests

## Gebaut

- `github.js`: `normalisierePull` mit den Feldern `{nummer, titel, zustand, autor, adresse, erstellt, aktualisiert, zusammenGefuehrt, quelle, ziel}`. `zustand` ist das Wort, das die Seite zeigt: „zusammengeführt“ bei `merged_at`, sonst „geschlossen“ bei `state: closed`, sonst „offen“. `quelle` und `ziel` sind die Zweige aus `head`/`base`.
- `github.js`: `beitraege(owner, name, {state:'all', frisch})` liest offene, geschlossene und zusammengeführte Pull Requests; `pull(owner, name, nummer)` liest einen einzelnen, immer frisch. `erstellePull` leert zusätzlich den Zwischenspeicher für `state=all`.
- `index.js`: drei Routen, in der Routenübersicht eingetragen:
  - `GET /board/github/beitraege?repo=` — nimmt `owner/name`, eine GitHub-Adresse oder einen lokalen Ordner mit GitHub-Remote. Antwort `{repo, adresse, kann, beitraege, hinweis}`; `kann` ist `true`, wenn ein Token vorliegt. Ein Repository, das GitHub ohne Zugang mit 404 beantwortet, ergibt 200 mit `{repo, adresse, kann:false, beitraege:[], hinweis}`.
  - `GET /board/github/beitrag?repo=&nummer=` — ein einzelner Beitrag mit Zustand, `{repo, adresse, kann, beitrag}`; fehlt er, 404 mit Repository im Text.
  - `POST /board/github/beitrag` — beschrieben werden nur Repositories, die das offene Board nennt (in `board.json`, einem Workflow oder einem Vorhaben). Alles andere erhält 403 vor jedem Aufruf bei GitHub; ein lokaler Ordner mit passendem Remote zählt nicht, wenn er nicht der Ordner des Boards ist. Ohne Token 409 mit `{kann:false, repo, adresse, hinweis:'Pull Request auf GitHub öffnen'}`. Mit Token sind `titel`, `head` und `base` Pflicht; das Schreibrecht wird über `darfSchreiben` am Ordner im Board geprüft. Antwort `{nummer, adresse}`. GitHub-Fehler nennen das Repository. Kein `festhalten`, kein Commit, kein Push.
  - Der POST steht in `OHNE_ABLAGE_SCHREIBEN`: ein Pull Request schreibt nicht in die Ablage; ein Formatkonflikt soll ihn nicht mit einem irreführenden 409 abweisen.
- `index.js`: Helfer `githubZielAus` (Verweis auflösen, Remote lesen), `githubAdresse`, `boardRepos`/`boardPfadFuer` (die Repositories des offenen Boards und ihr Ordner), `kurzZeile` (erste Fehlerzeile).
- `client.js`: die Komponente `BeitraegeFlaeche`. Sie liest beim Öffnen frisch, zeigt je Beitrag `#Nummer · Titel`, den Zustand („offen“, „zusammengeführt“, „geschlossen“) und „Ansehen“ zur GitHub-Adresse; ein zusammengeführter bleibt stehen. Mit Token: Knopf „Pull Request öffnen“ und ein Formular mit Titel, Text, Quellzweig, Zielzweig; nach dem Anlegen wird die Liste neu gelesen. Ohne Token: der Verweis „Auf GitHub beitragen“. Der Abschnitt „Beiträge“ steht in der Einzelheit des Vorhabens (Bibliothek), des Workflows (Bibliothek) und der geöffneten Station (im Block „Aus dem Repository“).
- `test/schritt35.mjs`: Board der Fassung 2 mit Station, Workflow und einem Vorhaben an der Station, alle als Repositories mit GitHub-Remote; nachgebildeter GitHub-Server mit Anlegen, Lesen, Zusammenführen und Schreibrecht. Geprüft werden der 409 ohne Token samt Adresse und `kann:false`, der Verweis bei nicht lesbarem Repository, das Anlegen mit Token (Nummer 1, Adresse), „offen“ in der Liste, „zusammengeführt“ nach dem Zusammenführen, ein abgelehnter Beitrag als „geschlossen“, 403 ohne Schreibrecht, die Auflösung eines lokalen Ordners zur selben Adresse, ein fremder lokaler Ordner und ein fremdes `owner/name` (je 403, kein Aufruf bei GitHub) und ein unverändertes lokales Repository (HEAD, Arbeitsbaum).

## Geänderte Dateien

- `/workspace/projekte/board-plugin/github.js` — `normalisierePull`, `beitraege`, `pull`, Zwischenspeicher.
- `/workspace/projekte/board-plugin/index.js` — Helfer und drei Routen, Routenübersicht, Ausnahme vom Formatvorbehalt.
- `/workspace/projekte/board-plugin/client.js` — `BeitraegeFlaeche`, eingebunden bei Vorhaben, Workflow und Station.
- `/workspace/projekte/board-plugin/test/schritt35.mjs` — neu, Prüfung.

## Befehl und Ergebnis

- `cd /workspace/projekte/board-plugin && node --test test/schritt35.mjs` → Exit-Code 0.
- Entscheidende Zeile: `ok Beiträge: ohne Token Verweis, mit Token PR #1 offen, zusammengeführt, geschlossen sichtbar`
- `node --test` → 34 von 34 bestanden, Exit-Code 0.
- `node --check index.js` → Exit-Code 0; `node --check client.js` → Exit-Code 0; `node --check github.js` → Exit-Code 0.

## Geprüft und offen

- Geprüft: ohne Token lesbar (öffentlich), 409 ohne Token mit Adresse und `kann:false`, kein Schreibaufruf ohne Token; mit Token Anlegen über `darfSchreiben` (Mock liefert `push`), Nummer und Adresse; Liste und Einzelbeitrag mit Zustand; Zusammenführen ändert den Zustand, der Beitrag bleibt; ein abgelehnter bleibt „geschlossen“; ein Token mit nur Leserecht erhält 403 und legt nichts an; ein fremder lokaler Ordner und ein fremdes `owner/name` erhalten 403, ohne dass GitHub gefragt wird; HEAD und Arbeitsbaum der lokalen Repositories bleiben unverändert. Kein echter GitHub-Aufruf.
- `zustand` ist bewusst das deutsche Wort, damit Liste und Oberfläche dieselbe Vokabel benutzen; `zusammenGefuehrt` steht zusätzlich als Wahrheitswert daneben.
- Zugehörigkeit und Schreibrecht sind zwei Prüfungen: Die Zugehörigkeit entscheidet der Ordner im offenen Board (nicht der Remote), das Schreibrecht `darfSchreiben`. Ein Repository außerhalb des Boards wird nicht beschrieben, auch nicht mit Token.
- Die Oberfläche ist mit `node --check` geprüft, nicht im Browser durchgeklickt.
- Keine Commits; außerhalb des Produktordners nur die Prüfablage unter `/tmp/board-scheibe1/schritt35`, die die Prüfung selbst entfernt, und dieser Bericht.
