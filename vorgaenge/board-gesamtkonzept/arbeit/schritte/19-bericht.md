# Bericht Schritt 19 — Verantwortung und Teams

## Gebaut

- `rechte.js`: `verantwortlicheVon({vorhaben, position})` liest die eingetragenen Logins einer Stelle. `istVerantwortlich({vorhaben, position, identitaet, teams, org, lokal, art, team})` prüft den GitHub-Namen der Identität gegen die Eintragung, sonst die Mitgliedschaft im Team `vorhaben` (für Stationen `stations`, für Workflows `workflows`). Ohne GitHub-Namen entscheidet nur ein lokales Repository für den lokalen Besitzer; ein vorhandener, nicht eingetragener Name wird auch lokal abgewiesen, ein Remote ohne Identität ebenso.
- `index.js`: `/board/vorhaben/verschieben` prüft vor jedem Schreiben `istVerantwortlich` an der aktuellen Station und antwortet sonst 403 mit „Nicht verantwortlich für diese Station“. Die Git-Rechteprüfung `darfFuerBoard` bleibt davor. `/board/workflow/station` prüft Host der Station, Autor des Workflows oder Team `workflows` und antwortet sonst 403.
- `index.js`: `GET /board/github/teams?org=DeepCollaboration&frisch=1` liefert Teams mit `name`, `schluessel`/`key`, Beschreibung, Sichtbarkeit und `mitglieder`. Ohne Token kommt `{kann:false, hinweis:'Teams nur mit Token'}`. Der Zwischenspeicher gilt zehn Minuten je Organisation, `DSH_BOARD_CACHE_TTL` überschreibt die Zeit, `frisch` umgeht ihn; bei Netzfehler kommt der letzte Stand mit Hinweis.
- `github.js`: `teams()` und `teamMitglieder()` nehmen ein optionales `frisch` und geben es an den GitHub-Aufruf weiter.
- `komposition.js`: je Vorhaben stehen `verantwortliche` (wie in `vorhaben.json`) und `verantwortlicheJeStation` (Positions-id → Logins) im Bild; je Station bleiben `host` und `sichtbarkeit` erhalten.
- `client.js`: in der Vorhaben-Einzelheit je Station eine Zeile „Station ‚…‘ — Verantwortlich: …“; ein Abschnitt „Teams“ in der Bibliothek mit Teamname, Schlüssel und Mitgliedern; ohne Token der Hinweis „Teams nur mit Token“. Schreibknöpfe am Vorhaben erscheinen nur, wenn `darf` gilt und die Identität an der Station verantwortlich ist (ohne GitHub-Namen: lokaler Besitzer). Die Stations-Einzelheit zeigt weiter „Host: …“.
- `test/schritt36.mjs`: Board der Fassung 2 mit Vorhaben an „umsetzen“, `verantwortliche: {umsetzen:['thomas']}`, nachgebildeter GitHub-Server mit Team `vorhaben` und Mitglied `anna`. Geprüft werden thomas → 200 mit Commit nur `vorhaben.json`, fremd → 403 ohne Commit, anna über das Team → 200, Host thomas stellt die Station, fremd → 403 ohne Commit, Teams-Route mit und ohne Token sowie `verantwortlicheJeStation`, Host und Sichtbarkeit in `/board/data`.

## Geänderte Dateien

- `/workspace/projekte/board-plugin/rechte.js`
- `/workspace/projekte/board-plugin/index.js`
- `/workspace/projekte/board-plugin/github.js`
- `/workspace/projekte/board-plugin/komposition.js`
- `/workspace/projekte/board-plugin/client.js`
- `/workspace/projekte/board-plugin/test/schritt36.mjs` (neu)

## Befehl und Ergebnis

- `cd /workspace/projekte/board-plugin && node --test test/schritt36.mjs` → Exit-Code 0.
- Entscheidende Zeile: `ok Verantwortung: thomas darf, fremd 403, anna über Team, Host prüft, Teams sichtbar`
- `node --test` → 35 von 35 bestanden, Exit-Code 0.
- `node --check rechte.js` → 0; `node --check index.js` → 0; `node --check client.js` → 0.

## Grenzen

- Kein echter GitHub-Aufruf; der Testserver in `test/schritt36.mjs` antwortet lokal.
- Keine Commits im Produkt; die Prüfablage unter `/tmp/board-scheibe1/schritt36` wird von der Prüfung selbst entfernt.
- Die Zugehörigkeitsprüfung der Beitrags-Route aus Schritt 18 (`boardPfadFuer`) bleibt unverändert.
