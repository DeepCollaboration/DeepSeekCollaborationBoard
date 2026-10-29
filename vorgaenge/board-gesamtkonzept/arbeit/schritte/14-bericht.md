# Bericht Schritt 14 — GitHub-Client und Token

## Gebaut

- `github.js` (neu): Client ohne zusätzliche Abhängigkeit über `fetch` aus Node. Gelesen werden Repository-Metadaten, Releases, Fragen (ohne Pull Requests), Pull Requests, Teams, Teammitglieder, Benutzer, Berechtigungen und die Rate-Grenze; geschrieben werden Repository-Anlage, Sichtbarkeit, Pull Request, Issue und Fork.
- Normalisierte Antworten: `repo` mit Name, vollem Namen, Sichtbarkeit, Sternen, Forks, offenen Fragen, Beschreibung, Themen, Standardzweig, Adresse, letztem Push, Lizenz, Archiv- und Eigentümerangabe; `releases` mit Tag, Name, Datum und Vorab-Kennzeichen; Fragen und Pull Requests mit Nummer, Titel, Zustand, Autor, Adresse, Zeitpunkten und `zusammenGefuehrt`.
- `GithubFehler` trägt `status`, `repo`, `meldung` und `grenze` (Rest, Reset, Reset als ISO). 401 → „Token ungültig oder abgelaufen“; 403/429 mit `x-ratelimit-remaining: 0` → „GitHub-Grenze erreicht; wieder ab <ISO>“; 404 → „Repository … nicht gefunden oder nicht zugänglich“; Netzfehler → „GitHub nicht erreichbar“; 5xx wird einmal wiederholt.
- Zwischenspeicher im Speicher je Client (TTL, standardmäßig 60 s). Sternzahlen liegen zusätzlich in `$DSH_HOME/board/sterne.json` (`{ "<owner>/<repo>": { "sterne": n, "am": iso } }`). Ein fehlender oder abgelaufener Zwischenspeicher verhindert keine Leseabfrage.
- Token-Datei `$DSH_HOME/board/github.json`: Inhalt `{ "token": "…" }`, Datei `0600`, Verzeichnis `0700`, Schreiben über `.tmp` + `rename`; Löschen entfernt auch eine liegen gebliebene `.tmp`. Der Token steht in keiner Antwort, keiner Fehlermeldung und keinem Protokoll.
- Routen in `index.js`: `GET /board/github/stand` (ohne `repo`: `{verbunden, login, grenze}`, Token-Prüfung über `GET /user`, Fehler weich; mit `repo`: `{verbunden, repo, sterne, fassung, freigaben, fehler}`) und `POST /board/github/token` (`{token}` wird erst nach erfolgreichem `GET /user` gespeichert, ungültiger Token → 401 und nichts gespeichert; `{loeschen: true}`). `GET /board/github/stand?repo=…` liest auch ohne Token.
- Oberfläche in `client.js`: Im Popover „Anzeige“ ein Abschnitt „GitHub“ mit Status („nicht verbunden“ / „verbunden als <login>“), Passwortfeld, „Verbinden“, „Token löschen“, dem Feld „Repository (owner/name)“ und „Prüfen“, das Sterne und letzte Fassung anzeigt. Ohne Token bleiben Lesen und Prüfen möglich; der Hinweis auf Schreibknöpfe, die erst mit Token erscheinen, steht dort.
- `test/schritt31.mjs` (neu): lokaler HTTP-Server auf Port 0 als GitHub-Ersatz, `DSH_GITHUB_API` darauf gesetzt. Geprüft werden Metadaten und Sterne, Releases, der Pull-Request-Filter der Fragen, 404, 401, die Grenze, der Netzfehler, die einmalige 5xx-Wiederholung, der Zwischenspeicher (zweite Abfrage ohne zweiten Treffer), `sterne.json`, die Token-Datei (0600, Inhalt, Löschen), beide Routen und dass der Token in keinem Antworttext und keiner Fehlermeldung steht. Server und Home werden im `finally` entfernt.

## Dateien

- `/workspace/projekte/board-plugin/github.js` (neu)
- `/workspace/projekte/board-plugin/index.js` (Import, zwei Routen, Routenübersicht)
- `/workspace/projekte/board-plugin/client.js` (GitHub-Abschnitt im Popover „Anzeige“)
- `/workspace/projekte/board-plugin/package.json` (`github.js` in `files`)
- `/workspace/projekte/board-plugin/test/schritt31.mjs` (neu)
- Keine Commits; außer diesem Bericht keine Änderung außerhalb des Produktordners und `/tmp/board-scheibe1/`.

## Prüfung

- Befehl: `cd /workspace/projekte/board-plugin && node --test test/schritt31.mjs`
- Exit-Code: 0
- Entscheidende Zeile: `ok GitHub: Sterne 7, Fassung v1.2.0; Token 0600, verbunden, gelöscht; 404/401/Grenze klar; Token nirgends im Text`
- Danach: `node --test` → `tests 30`, `pass 30`, `fail 0`, Exit-Code 0.
- `node --check github.js`, `node --check index.js`, `node --check client.js` → je Exit-Code 0.
