## Schritt 01 — bestanden

- Befehl: `diff -r /workspace/plugins/dsh-board /workspace/projekte/board/vorgaenge/board-gesamtkonzept/produkt`
- Exit-Code: 0
- Entscheidende Zeile: keine Ausgabe — Original und Produkt sind byte-gleich; `import.meta.resolve('@local/dsh-board')` im Profil `/dsh-home/profiles/web` → `file:///workspace/projekte/board/vorgaenge/board-gesamtkonzept/produkt/index.js`; `GET $DSH_WEB_URL/board/data` → `200`.
- Befund: `produkt/` enthält die 7 Dateien `package.json`, `index.js`, `client.js`, `cordis.patch.yml`, `icon.svg`, `locale/de.json`, `locale/en.json`; `find` zählt 7, alle sieben SHA-256-Werte sind paarweise gleich. Der vorgegebene Export-Einzeiler endet beim Eintrag `./locale/*.json` mit `ENOENT` (Exit 1), weil `statSync` den Glob nicht auflöst; das ist der im Schritt ausdrücklich vorgesehene Fall (Eintrag überspringen, stattdessen `locale/de.json` und `locale/en.json` prüfen). Beide Locale-Dateien sind vorhanden und gültiges JSON, `./icon` ist vorhanden; die Ersatzprüfung endet mit Exit 0. Im Profil ist `@local/dsh-board` als `link:` auf den Produktordner eingetragen, `node_modules/@local/dsh-board` dorthin verlinkt. `/workspace/plugins/dsh-board/` ist mit 7 Dateien unverändert vorhanden (byte-gleich, Arbeitsbaum sauber, letzte Änderung vor dem Schritt). Das laufende Board antwortet mit HTTP 200 und 9047 Bytes JSON.

## Schritt 02 — bestanden

- Befehl: `node /workspace/projekte/board/vorgaenge/board-gesamtkonzept/produkt/test/schritt02.mjs`
- Exit-Code: 0
- Entscheidende Zeile: `ok Urheber (beide): Thomas Heinrich, Thomas Heinrich`
- Befund: `produkt/board.config.json` ist vorhanden und nennt `{ "person": "p1", "repo": "/workspace/projekte/board" }`; `p1` ist in `board.json` die Person mit `ich: true` („Thomas Heinrich“). Die Ablage ist ein eigenes Repository auf `main` mit genau einer Fassung: `Thomas Heinrich Erste Fassung der Ablage`. Der exakte Produktpfad im Ablage-Index (`^vorgaenge/[^/]+/produkt/`) ergibt 0 Dateien; das einzige Vorkommen von `produkt/` ist `vorgaenge/board-gesamtkonzept-idee-zu-produkt/idee.md`, also ein Vorhabenordner, kein Produktordner. `/workspace` führt 0 Dateien unter `projekte/board`, `git status --porcelain | grep 'projekte/board'` ist leer; die Dateien liegen weiter auf der Platte. Das Prüfskript arbeitet ausschließlich auf einer Kopie in `/tmp`, belegt zwei Fassungen mit Urheber „Thomas Heinrich“ und den Lauf ohne Konfiguration; die echte Ablage bleibt bei einer Fassung. `GET $DSH_WEB_URL/board/data` antwortet mit 200.
