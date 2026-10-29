# Bericht Schritt 41 — die fehlenden Routen

## Gebaut

- **`GET /board/board` (`index.js`):** Antwortet mit dem offenen Board —
  `board` (der Registereintrag mit `id`, `name`, `art`, `format`, `repo`,
  `remote`, `person`, `sichtbarkeit`, `aktiv`), dazu `format`, `art`,
  `sichtbarkeit`, `darf` und `darfGrund`. `darfGrund` ist ohne Hindernis der
  leere Text und sonst ein deutscher Satz; bei fehlendem Schreibrecht kommt
  `darfHinweis` dazu. Für die Fassung 2 stehen die Verweise des `board.json`
  unter `verweise` (`workflows`, `vorhaben`, `stationen`). Die Fläche selbst —
  `vorgaenge`, `workflows`, `personen`, `schritte` — wird nicht gesendet. Ein
  Board der Fassung 1 antwortet ohne `verweise`; ein Archiv antwortet 200 mit
  `darf: false` und dem Grund „Die alte Ablage ist Archiv“. Ist das Board
  offen, aber sein Ordner fehlt, steht der Registergrund (etwa „nicht
  geholt“) in `fehler` und `darfGrund`.
- **`POST /board/board/verbinden` (`index.js`):** Nimmt `{ repo }` oder
  `{ adresse }` (optional `name`) und geht denselben Weg wie `/board/boards`
  über `verbindeBoard` — ein Board der Fassung 2 wird über `nimmBoardAuf`
  aufgenommen, eine alte Ablage über den Personen- und Formatweg. Danach wird
  es geöffnet (`setzeAktiv`) und die Antwort ist das Bild des offenen Boards in
  der Form von `/board/data` (`fassung2Antwort` bzw. `datenAntwort`). Fehler:
  fehlende Angabe oder fehlender Name → 400, unbekannter Ordner oder
  unerreichbare Adresse → 404, ein vorhandener Ordner, der kein Board ist, ein
  Formatkonflikt oder ein unlesbares `board.json` → 409. Ein nicht vorhandener
  Ordner wird nicht angelegt; ein vorhandener, leerer Ordner darf wie bei
  `/board/boards` ein neues Board bekommen.
- **`GET /board/workflow` (`index.js`):** Antwortet mit dem Workflow des
  offenen Boards: `verweis` und `fassung` aus dem `board.json`, dazu `ort`,
  `id`, `titel`, `beschreibung`, `autor`, `stationen`, `positionen`, `kanten`,
  `layout` und `regeln`. Für die Fassung 2 wird der erste Workflow genommen —
  derselbe, den `/board/data` als `workflow` zeigt. Jede Station wird in ihrer
  genannten Fassung gelesen und trägt `ort`, `name`, `host`, `fehlt` und
  `fassungFehlt`; fehlt eine Station lokal, bleibt ihr Verweis stehen und
  `grund` nennt den Grund. Fassung 1 antwortet mit dem eigenen Workflow aus
  `workflows/<person>.json` (`verweis`, `stationen` = seine Schritte,
  `fassung: ""`). Ohne Workflow, mit unlesbarem `board.json` oder mit nicht
  lesbarem Workflow-Repository antwortet der Weg 409 mit `error` und Grund.
- **Schreibprüfung (`index.js`):** `/board/board/verbinden` steht in
  `OHNE_ABLAGE_SCHREIBEN` und `ARCHIV_ERLAUBT`: der Weg schreibt keine Datei
  des offenen Boards und bleibt auch dann erreichbar, wenn das offene Board
  ein Archiv ist.
- **Neue Prüfung (`test/markt41.mjs`):** Prüffläche
  `/tmp/board-scheibe1/markt41`; zwei Boards der Fassung 2 mit Workflow und
  Station als Geschwisterordner, ein drittes Board ohne Workflow; Register und
  Identität vor dem Start gesetzt; Server in-process, ohne Netz. Geprüft wird:
  `GET /board/board` nennt Registereintrag, Format, Art, Sichtbarkeit, `darf`
  und `darfGrund` und sendet keine Fläche; `GET /board/workflow` nennt
  Verweis, Fassung, Station (in `v1` gelesen) und Position; `POST
  /board/board/verbinden` antwortet ohne Angabe 400 und mit unbekanntem Board
  404; ein verbundenes Board steht offen im Register, die Antwort ist das Bild,
  mit Remote und ohne Token ist `darf: false` samt Token-Grund; ein Board ohne
  Workflow → 409; danach ist `/board/data` weiter bedienbar, der Wechsel zurück
  funktioniert und der Workflow ist wieder da. Am Ende sind alle sieben
  Repositories auf ihrem HEAD und ohne Änderung im Arbeitsbaum.

## Dateien

- `index.js` (geändert): drei Routen, die Helfer `rechtDesOffenenBoards`,
  `offenesBoardBild`, `bildDesOffenenBoards` und `workflowDesOffenenBoards`,
  Import von `liesWorkflows`, die zwei Ausnahmen für den Verbindungsweg und die
  Routenübersicht im Kopf.
- `test/markt41.mjs` (neu).

`client.js`, `README.md` und die übrigen Dateien blieben unverändert.

## Befehl und Ergebnis

```
cd /workspace/projekte/board-plugin && node --test test/markt41.mjs
```

Exit-Code: **0**

Entscheidende Zeile:

```
ok Routen: Board, Verbinden, Workflow antworten
```

```
cd /workspace/projekte/board-plugin && node --test
```

Exit-Code: **0** — 57 Tests, 57 bestanden, 0 Fehler (56 vorhandene plus
`test/markt41.mjs`).

```
node --check index.js
node --check client.js
```

Exit-Code: je **0**.

## Grenzen

Nur Dateien im Produktordner und `test/markt41.mjs` geändert; kein Netz, keine
eigenen Commits. Die echten Ablagen und Register unter `/dsh-home` und
`/workspace/projekte/board-repos` wurden nur gelesen.
