# Bericht Schritt 44 — die frische Installation

## Gebaut

- **`test/markt44.mjs` (neu):** Ein leeres `$DSH_HOME` mit nichts als dem
  Zwischenspeicher — Hauptboard, Workflow und eine gefundene Station unter
  `board/cache/` — startet ohne Register, ohne Identität und ohne Token. Ein
  nachgebildeter GitHub-Server auf `127.0.0.1` beantwortet die anonyme Suche
  und die Zahlen der Station; ein echtes Netz wird nicht benutzt. Geprüft
  wird:
  1. **Start.** `apply` legt das Register an, öffnet das Hauptboard und
     hinterlässt keinen Fehler. Der Start fragt GitHub nicht; die drei
     Repositories liegen schon im Zwischenspeicher.
  2. **Board.** `/board/data` zeigt das Hauptboard und die gefundene Station
     „Gefundene Station“ an ihrer Workflow-Position. `darf` ist `false`,
     `darfGrund` nennt den fehlenden Token, `darfHinweis` den Weg über die
     Einstellungen. `eigentuemer` ist leer (`eigentuemerQuelle: "leer"`) mit
     Hinweis, `anlegen.darf` ist `false`. Lücken und Repository-Fehler: keine.
  3. **Anlegen.** `POST /board/station` ohne Token antwortet **403**. Die
     Absage nennt den Weg über die Anmeldung; `board.json` bleibt
     byte-gleich, kein Stationsordner entsteht, GitHub wird nicht gefragt.
     Wortlaut des Hinweises: „Noch kein Eigentümer bekannt. Melde dich mit
     einem GitHub-Token an; die Anmeldung trägt deinen Login ein. …“
  4. **Lesen.** Katalog (mit `gefunden` und `herkunft: "gefunden"`), Station,
     Workflow und Board antworten ohne Token mit 200. Alle Aufrufe an den
     nachgebildeten Server tragen keinen `Authorization`-Kopf.
  5. **Seite.** Codeprüfung in `client.js`: `nurLesen` hängt an
     `data.darf === false`, die Leiste zeigt „Nur lesen“ mit `darfGrund` und
     `darfHinweis`, und der Knopf „Station anlegen“ entsteht nur, wenn
     `data.anlegen.darf === true` ist.
  6. **README.** Die README nennt den Erststart in der Reihenfolge
     installieren, öffnen, lesen; anlegen erst mit Token samt
     „Token eintragen“ unter „Einstellungen → GitHub“.
- **`README.md`:** Neuer Abschnitt **„Frisch installiert: lesen ja, anlegen
  erst mit Token“** unter „Der erste Start“ mit den vier Schritten
  Installieren, Öffnen, Lesen, Anlegen mit Token. Er hält fest, dass die
  Board-Leiste ohne Schreibrecht „Nur lesen“ mit Grund zeigt und die Bibliothek
  trotzdem bedienbar bleibt, und dass der Katalog das Thema
  `dsh-board-station` anonym sucht.

Keine Produktdatei geändert: `index.js`, `client.js`, `erststart.js`,
`repos.js` und die übrigen Dateien blieben unverändert. Die vorhandenen 59
Tests laufen weiter.

## Dateien

- `test/markt44.mjs` (neu).
- `README.md` (geändert): der neue Abschnitt zwischen „Lesen braucht keinen
  Namen“ und „Das eigene Board“; keine bestehende Aussage entfernt.

Keine eigenen Commits.

## Befehl und Ergebnis

```
cd /workspace/projekte/board-plugin && node --test test/markt44.mjs
```

Exit-Code: **0**

```
ok Start: Hauptboard aus dem Zwischenspeicher, kein Fehler
ok Board: Hauptboard und gefundene Station, darf false, Eigentümer leer mit Hinweis
ok Anlegen: 403, Weg über den Token genannt, nichts geschrieben
ok Lesen: Katalog, Station, Workflow und Board ohne Token
ok Seite: „Nur lesen“ mit Grund, Knopf erst mit Anlege-Recht
ok README: installieren, öffnen, lesen, anlegen mit Token
ok Frisch: Hauptboard und Gefundenes lesbar, Anlegen verlangt Anmeldung
```

Entscheidende Zeile:

```
ok Frisch: Hauptboard und Gefundenes lesbar, Anlegen verlangt Anmeldung
```

Danach die ganze Suite:

```
cd /workspace/projekte/board-plugin && node --test
```

Exit-Code: **0** — `tests 60`, `pass 60`, `fail 0` (59 vorhandene plus der
neue Test; kein vorhandener Test gebrochen).

Syntax:

```
node --check test/markt44.mjs   → 0
```

`README.md` ist Markdown und wird nicht mit `node --check` geprüft; der Test
liest die geforderten Stellen aus der Datei und prüft sie.

## Hinweis zum Prüfaufbau

Das Zuhause ist wirklich leer: kein `boards.json`, keine `identitaet.json`,
kein `github.json`. Es liegen nur die drei Repositories im Zwischenspeicher,
wie sie ein früherer Start hinterlassen hätte; das Hauptboard trägt einen
GitHub-Remote, damit es ohne Token als „nur lesen“ gilt. Der Start holt
nichts nach und spricht nicht mit GitHub. Erst der Katalog und die
Stationszahlen fragen den nachgebildeten Server — anonym. Die Produktkopie
für den Lauf entsteht unter `/tmp/board-scheibe1/markt44/produkt` und wird mit
der Ablage am Ende entfernt.
