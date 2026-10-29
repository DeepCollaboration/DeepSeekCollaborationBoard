# Bericht Schritt 27 — Eine Station anlegen, mit austauschbarem Anlegeweg

## Gebaut

- **Route (`index.js`):** `POST /board/station` nimmt `{ name, beschreibung? }`
  an; `GET` bleibt unverändert. Nur ein Board der Fassung 2 legt an (sonst 409),
  ohne Board 409, leerer Name 400, mehr als 80 Zeichen 400. Der Name ist der
  Anzeigename, die Repo-Kennung `station-<slug>` entsteht mit dem vorhandenen
  `slug`. Ohne Eigentümer antwortet die Route 403 mit Grund und Hinweis (der
  Text aus Schritt 25), ohne `anlegen.darf` 403 mit dessen Grund und Hinweis
  (Schritt 26). Danach wird der Zwischenspeicher geprüft, der Anlegeweg gewählt,
  lokal gebaut, geschickt, der Verweis `owner/repo` in `board.stationen`
  aufgenommen (nicht doppelt) und `board.json` als Fassung festgehalten. Die
  Antwort ist das neue Bild in der Form von `/board/data` plus
  `station: { owner, repo, pfad, name }`.
- **Die Naht (`index.js`):** `anlegewegFuer(board)` liest
  `einstellungen.anlegeweg` (Vorgabe `selbst`; unbekannte Angaben gelten als
  `selbst`). Beide Wege haben `pruefen` und `anlegen`; Fehler tragen eine
  deutsche Meldung und einen Code (`belegt`, `nicht-pruefbar`, `github`).
  - `selbst`: `pruefen` fragt `client.repo` (404 = frei, 200 = belegt, anderer
    Fehler = 502 „nicht prüfbar“). `anlegen` ermittelt den angemeldeten Login
    (`client.ich`), legt öffentlich unter `/user/repos` an, wenn der Eigentümer
    dieser Login ist, sonst unter `/orgs/<owner>/repos`, setzt das Thema
    `dsh-board-station` und schreibt `station.json` und `README.md` über die
    Inhalts-API (`PUT .../contents/<pfad>`). Kein `git push`, kein Token in
    einer URL.
  - `attrappe`: `pruefen` sieht nur in den Zwischenspeicher, `anlegen` schickt
    nichts und meldet Erfolg. Die Wahl ist nur über die Einstellung erreichbar,
    nie in der Oberfläche.
- **Bauen (`index.js`):** `baueStation` legt
  `$DSH_HOME/board/cache/<owner>/<repo>` an, schreibt `station.json` (Format 2,
  `id` = slug, `name`, `beschreibung`, `host` = Eigentümer, leere Ein- und
  Ausgänge, `aktion: { art: "textfeld", wert: beschreibung }`, keine Beispiele,
  `reife: "entwurf"`, `lizenz: "nicht angegeben"`, keine Tags) und `README.md`
  (Name, Beschreibung, Thema), dann `git init -b main`, `add`, `commit`. Ein
  Remote wird nicht gesetzt; kein Token gerät in `.git/config`, Argumente oder
  Adressen. Schlägt der Weg danach fehl, wird der lokale Ordner wieder entfernt.
- **`github.js`:** `erstelleRepo` kennt jetzt `eigenes` und damit auch
  `POST /user/repos`; neu ist `setzeInhalt(owner, name, pfad, inhalt, nachricht)`
  über die Inhalts-API (base64, Token nur im Kopf).
- **Komposition (`komposition.js`):** `ortVon` löst `owner/repo` zusätzlich über
  `$DSH_HOME/board/cache/<owner>/<repo>` auf. Die alte Form — nackter Name,
  Geschwisterordner, Zwischenspeicher der Organisation — bleibt.
- **Anzeige (`client.js`):** In der Bibliothek steht unter „Stationen“ der Knopf
  „Station anlegen“, nur wenn `anlegen.darf` wahr ist. Darunter öffnet sich ein
  kleines Formular mit Name und Beschreibung, „Anlegen“ und „Abbrechen“; Fehler
  stehen im Formular. Keine Auswahl des Anlegewegs, kein „Torwächter“, kein
  „später“-Hinweis. Die Antwort ersetzt den Stand ohne Neuladen.
- **README:** ein Abschnitt „Eine Station anlegen“ in drei Sätzen.
- **Prüfung (`test/markt27.mjs`, neu):** Temp-Heim und eigene Prüffläche unter
  `/tmp/board-scheibe1/markt27`; Board mit
  `einstellungen: { eigentuemer: "testkonto", anlegeweg: "attrappe" }`. Der
  API-Port ist zu Beginn geschlossen; für (d) startet dort ein nachgebildeter
  GitHub-Server. (a) Anlegen über `attrappe` → lokales Repository mit gültiger
  `station.json` und README, Verweis in `board.json`, Karte in `/board/data`;
  (b) derselbe Name → 409, keine neue Fassung, kein zweites Repository;
  (c) ohne Eigentümer → 403 mit Grund und Hinweis, nichts angelegt;
  (d) `anlegeweg: "selbst"` → `POST /user/repos` (201), Thema, zwei Inhalte,
  Antwort 200; (e) kein Token in `.git/config`, kein Remote. Zusätzlich wird
  `attrappe` bei laufendem Server aufgerufen und der Server sieht dabei keinen
  neuen Aufruf.
- **Live-Lauf im Test eingebaut, nicht ausgeführt:** `BOARD_LIVE=1` und
  `BOARD_LIVE_TOKEN` (nur im Prozess) legen unter `ThomasTestDSBOARD` eine
  Station an, prüfen Repository, Thema und Dateien über die API und löschen das
  Repository wieder. Ohne `BOARD_LIVE` wird dieser Weg nicht betreten; der
  Token steht in keiner Datei des Produkts.

## Geänderte Dateien

- `/workspace/projekte/board-plugin/index.js`
- `/workspace/projekte/board-plugin/github.js`
- `/workspace/projekte/board-plugin/komposition.js`
- `/workspace/projekte/board-plugin/client.js`
- `/workspace/projekte/board-plugin/README.md`
- `/workspace/projekte/board-plugin/test/markt27.mjs` (neu)

## Befehl und Ergebnis

- `cd /workspace/projekte/board-plugin && node --test test/markt27.mjs`
  → Exit-Code 0.
  - Entscheidende Zeile: `ok Station: attrappe lokal, Kollision, ohne Eigentümer, selbst über GitHub`
- `cd /workspace/projekte/board-plugin && node --test` → Exit-Code 0;
  `tests 43`, `pass 43`, `fail 0` (die 42 vorhandenen plus `markt27`).
- `node --check index.js`, `node --check github.js`,
  `node --check komposition.js`, `node --check client.js`,
  `node --check test/markt27.mjs` → je Exit-Code 0.
- Kein echtes Netz: (a)–(c) laufen gegen einen geschlossenen Port, (d) gegen
  den nachgebildeten Server auf `127.0.0.1`; `BOARD_LIVE` wurde nicht gesetzt,
  der Live-Lauf also nicht ausgeführt.

## Was die Prüfung zeigt

1. `attrappe`: 200; `home/board/cache/testkonto/station-meine-station` mit
   genau einer Fassung, sauberem Stand, gültiger `station.json` (Kennung, Name,
   Beschreibung, Host, leere Ein-/Ausgänge, Textfeld-Aktion, Entwurf, Lizenz
   „nicht angegeben“, keine Tags), README mit Name, Beschreibung und Thema;
   Verweis einmal in `board.json`; Karte in `/board/data` mit Name, Host und
   Pfad.
2. Kollision: 409 mit „Eine Station mit diesem Namen gibt es schon“; keine neue
   Fassung, der Verweis bleibt einmal.
3. Ohne Eigentümer: 403; der Grund nennt den fehlenden Eigentümer, der Hinweis
   steht daneben; es entsteht kein Ordner.
4. `selbst`: `POST /user/repos` mit Name und öffentlicher Sichtbarkeit; genau
   ein Themen-Aufruf mit `dsh-board-station`; genau zwei Inhalte
   (`station.json`, `README.md`), deren base64-Inhalt mit den lokalen Dateien
   übereinstimmt; Antwort 200; kein Token in einer Adresse.
5. Kein Token in `.git/config`, kein Remote in beiden neuen Repositories.
6. `attrappe` bei laufendem Server: 200, kein einziger Aufruf.

## Grenzen

- **Live-Lauf nicht ausgeführt.** Er ist eingebaut und syntaktisch geprüft; den
  echten Lauf macht die Bauleitung mit `BOARD_LIVE=1` und `BOARD_LIVE_TOKEN`.
- **`GET /board/station` bleibt unverändert**, wie im Schritt verlangt. Die neue
  Station steht als Karte in `/board/data`; die vorhandene Detailansicht holt
  weiter nur über die Kennung und löst Verweise als Pfade auf.
- **Kein Browserdurchgang.** Der laufende Server trägt den älteren Stand und
  wurde nicht benutzt; die Anzeige ist über den Code und `node --check` belegt.
  Die Abnahme im Browser steht aus.
- **Keine eigenen Commits.** Im Produkt und an der Prüffläche wurde nichts von
  Hand committet; die Prüffläche räumt sich selbst ab.
- **Der Anlegeweg `attrappe`** ist nur über `einstellungen.anlegeweg` erreichbar;
  die Einstellung hat keinen Schreibweg in der Oberfläche, sie wird von Hand
  bzw. in der Prüfung gesetzt.
