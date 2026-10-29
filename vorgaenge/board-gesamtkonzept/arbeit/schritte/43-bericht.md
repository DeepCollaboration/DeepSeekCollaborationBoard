# Bericht Schritt 43 — Zwei Menschen, ein Board

## Gebaut

- **`test/markt43.mjs` (neu):** Der offene Weg einmal ganz, mit zwei
  Identitäten gegen einen nachgebildeten GitHub-Server auf `127.0.0.1`. Kein
  echtes Netz. A (`testkonto`) und B (`fremd`) haben je ein eigenes
  `$DSH_HOME` mit eigener Identität und eigenem Token; Board, Workflow und
  Katalog liegen in einer gemeinsamen Fläche, die beide lesen.
  1. **A legt an.** `POST /board/station` legt „Gemeinsame Station“ unter
     `testkonto/station-gemeinsame-station` an; der nachgebildete Server sieht
     das Thema `dsh-board-station`, der Verweis steht in `board.json`.
  2. **B findet.** B ruft `GET /board/katalog?frisch=1` noch ohne Token auf;
     die Station steht als `herkunft: "gefunden"` in der Antwort, die Suche
     ging anonym hinaus.
  3. **B benutzt.** B meldet sich an und ruft `POST /board/workflow/station`;
     der Workflow schreibt den Verweis mit kurzer Fassung und eine Position,
     die Karte sagt `imWorkflow`.
  4. **B forkt und beiträgt.** `POST /board/github/fork` legt
     `fremd/station-gemeinsame-station` an; `POST /board/github/beitrag`
     stellt Pull Request #1 mit Quelle `fremd:beitrag` und Ziel `main`.
     A liest ihn über `/board/github/beitraege` und
     `/board/github/beitrag?nummer=1` als offen von `fremd`.
  5. **B legt ein Vorhaben an.** `POST /board/vorhaben` legt „Baut zusammen“
     als `fremd/vorhaben-baut-zusammen` an der Station an. A sieht die Karte
     an der Position, mit `fremd` als Verantwortlichem.
  6. **Der Katalog zeigt beide Herkünfte.** Je ein Eintrag `kuratiert`
     („Kuratierte Station“) und `gefunden` (A's Station), zusammen zwei.
  7. **Ohne Token lesbar.** Nach dem Entfernen beider Token liefern Board,
     Workflow, Station, Katalog sowie der einzelne und der Listen-Pull-Request
     weiter 200; das Board sagt `darf: false` mit Grund. In keiner Adresse
     steht ein Token.
  Jede Station wird mit ihrem Ergebnis als eigene `ok`-Zeile genannt.
- **Live-Teil gebaut, nicht ausgeführt:** Mit `BOARD_LIVE=1
  BOARD_LIVE_TOKEN=<token>` legt derselbe Test mit dem Testkonto eine Station
  an, sucht sie nach Entfernen des Tokens anonym im Katalog (Wiederholung,
  weil die GitHub-Suche nachhinkt) und löscht das Repository wieder. Der
  Token steht nur im Prozess. Ohne `BOARD_LIVE` wird dieser Weg nicht
  betreten.
- **`README.md`:** Neuer Abschnitt **„Der offene Weg — zwei Menschen, ein
  Board“** mit den fünf Schritten Anlegen, Auffinden, in einen Workflow
  stellen, Forken und Beitragen sowie dem Satz, dass ohne Token alles lesbar
  bleibt.

## Dateien

- `test/markt43.mjs` (neu).
- `README.md` (geändert): der neue Abschnitt zwischen „Eine Station anlegen“
  und „Der GitHub-Token“.
- `index.js`, `client.js`, `katalog.js` und die übrigen Produktdateien blieben
  unverändert.

Keine eigenen Commits.

## Befehl und Ergebnis

```
cd /workspace/projekte/board-plugin && node --test test/markt43.mjs
```

Exit-Code: **0**

Entscheidende Zeile:

```
ok Zwei Menschen: anlegen, finden, benutzen, forken, beitragen
```

Danach die ganze Suite:

```
cd /workspace/projekte/board-plugin && node --test
```

Exit-Code: **0** — `tests 59`, `pass 59`, `fail 0` (58 vorhandene plus der
neue Test; kein vorhandener Test gebrochen).

Syntax:

```
node --check test/markt43.mjs   → 0
node --check index.js           → 0   (unverändert, gegengeprüft)
node --check client.js          → 0   (unverändert, gegengeprüft)
node --check katalog.js         → 0   (unverändert, gegengeprüft)
```

## Live-Lücke

Die B-Seite (Fork und Pull Request unter `fremd`) braucht ein zweites
GitHub-Konto. Es liegt nicht vor; sie wird deshalb nur gegen den
nachgebildeten Server geprüft und **nicht** als Live-Ergebnis ausgegeben. Der
Live-Teil deckt nur die A-Seite ab (anlegen, anonym finden, löschen) und wurde
im Auftrag dieses Schritts nicht ausgeführt.

## Hinweis zum Prüfaufbau

Die Anlege-Route lässt den lokalen Ordner der neuen Station ohne Remote (so
ist Schritt 27 gebaut). B hat im Prüfaufbau einen eigenen Klon dieser Station
mit GitHub-Remote, wie ihn ein zweiter Rechner nach dem Holen hätte; nur so
sind `boardPfadFuer`, Fork und Pull Request für B erreichbar. Das ist eine
Eigenschaft des Prüfaufbaus, keine Produktänderung.
