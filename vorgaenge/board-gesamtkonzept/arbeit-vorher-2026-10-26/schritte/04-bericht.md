# Bericht Schritt 04 — Der Abgleich holt und gibt

- **Stand:** 2026-10-26
- **Gebaut:** `POST /board/sync`; `stand` in `/board/data`; Abgleich vor dem
  Lesen (höchstens einmal je Minute); Weitergeben nach jeder Speicherung;
  Kopfzeile mit Zeit und Zahl; Prüfskript `produkt/test/schritt04.mjs`; die
  Ablage ist als Fassung „Abgleich“ festgehalten.
- **Nicht neu gestartet:** Das laufende Board arbeitet weiter mit dem geladenen
  alten Modul. Die Prüfungen laufen am Modul in `/tmp`; die echte Ablage hat
  weiterhin **keinen** Remote.

## Was gebaut ist

### Der Abgleich in `produkt/index.js`

- **`POST /board/sync`** (`{}` → `{ geholt, gegeben, lage }`): `git fetch
  --prune origin`, dann `git pull --rebase origin <zweig>`, dann `git push
  origin <zweig>`. Der Zweig kommt aus `HEAD` (heute `main`); gibt es den
  fremden Zweig noch nicht, entfällt das Holen und der eigene Stand wird
  geschrieben.
- **Ohne `origin`:** Es wird kein Netz versucht. Die Route antwortet
  `{ geholt: false, gegeben: false, lage: "kein remote" }`.
- **Scheitert etwas:** Der lokale Stand bleibt stehen, ein angefangener Rebase
  wird abgebrochen, die Antwort kommt trotzdem. `zeit` bleibt der letzte
  erfolgreiche Abgleich; `nur_lokal` zählt die lokal wartenden Fassungen über
  `git rev-list --count origin/<zweig>..HEAD`.
- **`git`-Aufrufe** haben jetzt eine Zeitgrenze (Vorgabe 20 s, Netz 10 s) und
  `GIT_TERMINAL_PROMPT=0`: Der Host kann nicht antworten, also scheitert ein
  Aufruf lieber, als auf eine unsichtbare Eingabe zu warten.
- **Nach dem Schreiben:** `festhalten` legt wie bisher den Commit an und
  versucht danach `git push origin <zweig>`. Scheitert das, bleibt der Commit
  lokal; `nur_lokal` zählt ihn. Die Antwort der schreibenden Route hängt nicht
  davon ab.
- **Vor dem Lesen:** `/board/data` ruft `abgleichVorDemLesen()`. Der Versuch
  läuft höchstens einmal je 60 Sekunden; ein Fehler wird nicht geworfen.

### `stand` in `/board/data`

`/board/data` liefert jetzt zusätzlich:

```json
"stand": { "zeit": "…ISO…", "nur_lokal": 0, "remote": "…", "lage": "abgeglichen" }
```

- `zeit` — Zeitpunkt des letzten erfolgreichen Abgleichs, im Speicher; nach
  einem Neustart `null`.
- `nur_lokal` — Zahl der Fassungen, die in `origin/<zweig>` fehlen; ohne Remote
  `0`.
- `remote` — Adresse von `origin` oder `null`.
- `lage` — `abgeglichen`, `nicht abgeglichen` oder `kein remote`. Bei
  `abgeglichen` ist zusätzlich `nur_lokal` 0.

Die Form von `/board/data` ist damit `{ workflow, schritte, vorgaenge,
personen, einstellungen, stand }`. Die Prüfung in `schritt03.mjs` erwartet
diesen zusätzlichen Schlüssel jetzt mit; alles andere dort ist unverändert.

### Die Kopfzeile in `produkt/client.js`

Unter dem Hinweistext steht eine Zeile aus `stand`:

- `zuletzt abgeglichen 26.10., 16:30` (Ortszeit, aus `stand.zeit`),
- sonst `nicht abgeglichen`, ohne Remote `nicht abgeglichen — kein Remote`,
- bei Bedarf dahinter `· n Fassungen nur hier` bzw. `· 1 Fassung nur hier`.

Fehlt `stand` (etwa beim noch laufenden alten Modul), bleibt die Zeile weg.

### Die Prüfung `produkt/test/schritt04.mjs`

Das Skript legt in `/tmp` ein nacktes Repository als `origin` an, füllt es mit
einer kleinen Ablage (`format`, Person, Workflow, ein Vorhaben), klont es
zweimal und lädt das Produkt als Kopie mit eigener `board.config.json` auf den
ersten Klon. Geprüft wird:

1. Im zweiten Klon eine Datei ändern, festschreiben, schieben; im Modul
   `POST /board/sync` → die Datei liegt im ersten Klon, `stand.zeit` ist
   gesetzt, `nur_lokal` ist 0, `remote` nennt das nackte Repository.
2. Über `/board/doc/schreiben` speichern und `POST /board/sync` → die Fassung
   liegt im nackten `origin`, samt Fassungsnachricht „notiz.md gespeichert“.
3. Ein frischer Modullauf holt eine weitere Änderung schon beim ersten
   `/board/data` (ohne eigenen `POST /board/sync`).
4. `git remote remove origin` im ersten Klon; `/board/data` antwortet weiter,
   `stand.lage` ist `kein remote`, `stand.remote` ist `null`; auch
   `POST /board/sync` meldet `kein remote`.

Der Prüfordner wird am Ende gelöscht; die echte Ablage bleibt unberührt.

## Prüfungen

| Nr | Befehl | Ergebnis | Exit |
| --- | --- | --- | --- |
| 1 | `node …/produkt/test/schritt04.mjs` | holt, gibt, liest vorab, kein remote | 0 |
| 2 | `node …/produkt/test/schritt03.mjs` | 5 Vorhaben, 11 Positionen, 11 Schritte, 1 Person; Fassung nennt Thomas Heinrich | 0 |
| 3 | `node produkt/index.js` (Leseprobe am echten Repository) | `stand` = `{zeit:null, nur_lokal:0, remote:null, lage:"kein remote"}` | 0 |
| 4 | `git -C /workspace/projekte/board status --porcelain` | leer | 0 |
| 5 | `git -C /workspace/projekte/board log -1 --format='%an %s'` | `Thomas Heinrich Abgleich` | 0 |

Ausgabe von `schritt04.mjs`:

```
ok POST /board/sync holt: von-anderer.md, lage abgeglichen
ok stand: zeit gesetzt, nur_lokal 0, remote /tmp/board-schritt04-…/origin.git
ok Speichern + POST /board/sync: notiz.md liegt im nackten origin
ok /board/data holt vor dem Lesen: vor-dem-lesen.md ist da
ok ohne remote: /board/data antwortet, lage kein remote, remote null
ok Prüfordner aufgeräumt: /tmp/board-schritt04-…
```

**Entscheidende Zeile:** `ok ohne remote: /board/data antwortet, lage kein
remote, remote null` — der Fall ohne Remote ist der heutige Zustand der echten
Ablage.

Leseprobe am echten Repository (`/board/data` über das neue Modul):

```
keys einstellungen,personen,schritte,stand,vorgaenge,workflow
stand {"zeit":null,"nur_lokal":0,"remote":null,"lage":"kein remote"}
vorhaben 5 schritte 11 personen 1
```

## Entscheidungen

- **Zweig statt fest „main“.** Geholt und gegeben wird der Zweig, auf dem die
  Ablage steht (`git symbolic-ref --short HEAD`), heute `main`. `nur_lokal`
  zählt entsprechend `origin/<zweig>..HEAD`. Das erfüllt den Auftrag und
  verträgt ein Repository, dessen Zweig anders heißt.
- **`zeit` nur nach einem ganzen Abgleich.** Ein erfolgreicher Push nach dem
  Speichern setzt den Zeitpunkt nicht; erst `fetch` + `pull` + `push` gelten
  als Abgleich. So bleibt „zuletzt abgeglichen“ ehrlich, auch wenn nur
  weitergegeben wurde.
- **Weitergeben nach jeder Speicherung, aber ohne Wurf.** Der Push nach dem
  Commit ist ein Versuch mit 10 s Grenze. Ohne Netz bleibt die Fassung lokal;
  die Route antwortet trotzdem, und `nur_lokal` zählt sie. Das ist die
  Umsetzung von „gibt nach dem Schreiben“.
- **Bremse nur beim Lesen.** `/board/data` versucht höchstens einmal je Minute
  einen Abgleich. `POST /board/sync` ist der ausdrückliche Anstoß und wartet
  nicht auf die Bremse.
- **Ohne Remote ist `nur_lokal` 0.** `git rev-list origin/<zweig>..HEAD` gibt
  es dann nicht; die Lage `kein remote` sagt bereits alles. Die Zahl 0 ist
  keine Behauptung, dass nichts lokal liegt, sondern dass es nichts zu zählen
  gibt.

## Fassung

Der Stand der Ablage ist als Fassung `Abgleich` festgehalten (Urheber
`Thomas Heinrich`, `thomas.heinrich@board`). Die Fassung enthält auch die
Änderungen, die vor diesem Schritt im Arbeitsbaum lagen (`board.json` aus dem
noch laufenden alten Board sowie die Arbeitsstände aus den Schritten davor), weil
der Auftrag `git status --porcelain` leer verlangt. An `board.json` hat dieser
Schritt nichts geändert; die neuen Ablagedateien (`format`, `personen/`,
`workflows/`, `vorgaenge/*/vorhaben.json`) hat er nicht angefasst.
Die Änderungen am Produkt (`produkt/index.js`, `produkt/client.js`, die beiden
Prüfskripte) sind wie in den Schritten davor nicht Teil der Board-Fassung:
`vorgaenge/*/produkt/` ist im Repository ignoriert.

## Grenzen und offene Punkte

- **Kein Remote in der echten Ablage.** Es wurde keiner angelegt. Bis Adresse
  und Zugang vorliegen, sagt das Board `kein remote` und zeigt „nicht
  abgeglichen — kein Remote“. Das ist der im Auftrag genannte offene Punkt.
- **Nicht neu gestartet.** Die Kopfzeile ist am laufenden alten Modul nicht
  sichtbar; geprüft sind der Serverpfad, die Syntax von `client.js` und die
  Datenform, nicht die gezeichnete Seite.
- **Der Live-Abgleich gegen GitHub ist ungeprüft.** Geprüft ist der Abgleich
  gegen lokale nackte Repositories in `/tmp`; ein Netz nach außen wurde nicht
  benutzt.
- **Ein Konflikt beim Rebase** wird abgebrochen; die lokale Fassung bleibt
  stehen und `lage` sagt `nicht abgeglichen`. Wie der Text dann zusammengeführt
  wird, ist nicht Teil dieses Schritts.
