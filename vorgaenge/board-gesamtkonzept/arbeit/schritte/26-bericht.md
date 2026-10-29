# Bericht Schritt 26 — Anmelden und dürfen

## Gebaut

- **Schreibrecht als Frage nach der Person (`rechte.js`):** Die neue Funktion
  `schreibrecht({ pfad, home, client })` antwortet mit `{ darf: true }` oder
  `{ darf: false, grund, hinweis }`. Lokal ohne Remote ja; mit Remote und ohne
  Token nein (Grund „Keine Anmeldung: es fehlt ein Token.“); mit Token
  entscheidet die Antwort von GitHub für den aus `ich()` gelesenen Login
  (`berechtigung(owner, name, login)`), nicht der Token. `darfSchreiben` bleibt
  als kurze Ja/Nein-Antwort erhalten und ruft dieselbe Stelle auf — die
  vorhandenen Prüfungen laufen unverändert. Die 30-Sekunden-Ablage hält jetzt
  das Ergebnis samt Grund und bleibt nach Repository, Remote und Login
  geschlüsselt; ein Fehler ohne Netz wird nicht gemerkt.
- **Gründe:** kein Token, Token ohne Schreibrecht, Recht nicht prüfbar (kein
  Netz); dazu die abgewiesene Anmeldung (ungültiger Token). Die Texte stehen
  als deutsche Sätze in `rechte.js`.
- **`/board/data` der Fassung 2 (`index.js`):** trägt zusätzlich `darfGrund`
  und `darfHinweis` — nur wenn `darf` falsch ist — sowie
  `anlegen: { darf, grund, hinweis }`. Ohne Token ist `anlegen.darf` falsch und
  der Hinweis nennt den Weg (Einstellungen → GitHub → Token); mit Token wahr.
  `darf` bleibt ein Wahrheitswert; ohne Recht werden die Vorhaben-Handlungen
  weiter gesperrt. Der schnelle Weg ohne Netz (lokal ja, Remote ohne Token
  nein) antwortet sofort mit demselben Grund.
- **`/board/github/stand`:** unverändert. Ohne Token `login: null` ohne Fehler;
  mit Token und ohne Netz bleibt der gemerkte Login stehen, `fehler` nennt den
  Grund. Kein neues Feld.
- **Anzeige (`client.js`):** Ist `darf` falsch, steht in der Board-Leiste ein
  „Nur lesen“-Zeichen (Titel = `darfGrund`), darunter „Nur lesen: <grund>“ und
  darunter der `darfHinweis`. Ist `darf` wahr oder fehlt es (Fassung 1), steht
  nichts davon. Der Eigentümer wird ohne Schreibrecht nur genannt; Eingabefeld
  und Speichern-Knopf entfallen. „+ Workflow“ in der Bibliothek erscheint ohne
  Schreibrecht nicht mehr (vorher nur ausgegraut). Die übrigen Schreibknöpfe
  waren schon über `schreibbar` bzw. `vorgang.darf` verborgen: Vorhaben-
  Handlungen (Weitergeben, Bearbeiten, Verschieben, Zurückschicken,
  Entscheiden), Bearbeiten der Station und der Datei, Forken, Neues Vorhaben,
  Workflow übernehmen. Der GitHub-Abschnitt zeigt weiter „verbunden als
  <login>“ und ohne Token seinen Hinweis.

## Geänderte Dateien

- `/workspace/projekte/board-plugin/rechte.js`
- `/workspace/projekte/board-plugin/index.js`
- `/workspace/projekte/board-plugin/client.js`
- `/workspace/projekte/board-plugin/test/markt26.mjs` (neu)

## Befehl und Ergebnis

- `cd /workspace/projekte/board-plugin && node --test test/markt26.mjs`
  → Exit-Code 0.
  - Entscheidende Zeile: `ok Recht: lokal ja, Remote ohne Token nein mit Grund, Token write ja, read nein, anlegen`
- `cd /workspace/projekte/board-plugin && node --test` → Exit-Code 0;
  `tests 42`, `pass 42`, `fail 0` (die 41 vorhandenen plus `markt26`).
- `node --check index.js`, `node --check rechte.js`, `node --check client.js`,
  `node --check test/markt26.mjs` → je Exit-Code 0.
- Prüffläche: eigenes Heim und eigene Repositories unter
  `/tmp/board-scheibe1/markt26`, danach entfernt. Kein echtes Netz:
  `DSH_GITHUB_API` zeigt auf einen nachgebildeten Server auf 127.0.0.1
  (`/user`, `/rate_limit`, `/repos/<owner>/<name>/collaborators/<login>/permission`).
  Ein versehentlicher echter Aufruf ist damit ausgeschlossen.

## Was die Prüfung zeigt

1. Lokal ohne Remote: `darf: true`, kein `darfGrund`/`darfHinweis`; das
   Verschieben eines Vorhabens schreibt und fasst `vorhaben.json`.
2. Remote ohne Token: `darf: false`, Grund nennt Anmeldung und Token;
   `anlegen.darf: false` mit Hinweis; `POST /board/einstellung` antwortet 403
   ohne Fassung; `stand.login: null` ohne Fehler.
3. Nachgebildeter Server + Token mit `write`: `stand.login: "schreiber"`,
   `darf: true`, `anlegen.darf: true`, kein Grund; die Berechtigung wurde für
   den Login abgefragt.
4. Token mit `read`: `darf: false`, Grund „Kein Schreibrecht …“, die
   Vorhaben-Handlungen sind gesperrt.
5. Server aus: gemerkter Login bleibt stehen, `fehler` nennt „GitHub nicht
   erreichbar“, `darf: false` mit „nicht prüfbar“.
6. Token löschen: `darf: false` mit dem Grund „keine Anmeldung“.

## Grenzen

- **Kein Browserdurchgang.** Der laufende Server trägt den alten Stand und
  wurde nicht benutzt; die Anzeige ist über den Code und `node --check`
  belegt. Die Abnahme im Browser steht aus.
- **Keine eigenen Commits.** Im Produkt und an den Prüfflächen wurde nichts
  von Hand committet.
- **„Datei speichern“ in der Prüfung über `/board/vorhaben/verschieben`.**
  Der Schreibweg `/board/doc/schreiben` liest weiter die Fassung-1-Ablage
  (`liesBoard`); für ein Fassung-2-Board schreibt er deshalb nicht. Der Test
  speichert stattdessen `vorhaben.json` über den Fassung-2-Weg und prüft die
  Datei und den sauberen Stand danach. Diesen Weg umzubauen gehört nicht zu
  Schritt 26.
- **Kein neuer Schalter, kein Torwächter, keine Anlege-Route.** `anlegen` ist
  nur die Antwort; die tatsächliche Anlage prüft Schritt 27.
- **Fassung 1 bleibt außen vor.** Sie bekommt weder `darfGrund` noch
  `anlegen`; ihre `/board/data`-Form ist unverändert.
