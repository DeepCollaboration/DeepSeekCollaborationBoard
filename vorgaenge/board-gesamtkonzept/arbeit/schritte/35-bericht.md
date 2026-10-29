# Bericht Schritt 35 — Der Katalog ist vollständig

## Gebaut

- **Beispiele (`katalog.js`, `beispieleLesen`, Zeile 173–190):** Jede Station
  trägt `beispiele` mit den Namen aus `station.json` (`beispiele`). Ist die
  Liste dort leer, gelten die Dateien im Ordner `beispiele/` des Repositories,
  nach Namen sortiert. Fehlt beides, bleibt die Liste leer. Workflows und
  Vorhaben bekommen `beispiele: []`; ihr eigener `beispiele/`-Ordner wird
  nicht gelesen. Gefundene Einträge tragen ebenfalls `beispiele: []` und
  erfinden keine.
- **Prüfung (`katalog.js`, `pruefungsSchluessel`, `pruefungsKarte`,
  `eintragsSchluessel`, `pruefungFuer`, Zeile 195–242):** `geprueft` aus
  `katalog.json` (`repo`, `von`, `am`) wird jedem Eintrag zugeordnet. Der
  Vergleich läuft über `owner/repo` (klein geschrieben, aus `github` oder dem
  Remote) oder über den aufgelösten lokalen Pfad. Die Prüfung wird auch beim
  schnellen Weg aus dem Zwischenspeicher frisch aus `katalog.json` gelesen
  (Zeile 545–563). Ohne passenden Eintrag steht `geprueft: null`; es wird
  keine Prüfung erfunden. Das gilt auch für Workflows, Vorhaben und
  gefundene Einträge.
- **Zwei getrennte Zahlen (`katalog.js`, Zeile 640–661):** Nach den
  Repository-Metadaten zählt der Katalog offene Fragen über
  `github.fragen(..., { state: 'open' })` — Issues ohne Pull Request — und
  offene Beiträge über `github.beitraege(..., { state: 'open' })`. Der Pull
  Request in der Issues-Liste erhöht die Fragen nicht. `open_issues_count`
  aus den Metadaten wird dafür nicht mehr verwendet. Schlägt ein Abruf fehl,
  bleiben die Zahlen `null` oder der letzte Stand aus dem
  Katalog-Zwischenspeicher; 0 wird nur gesetzt, wenn der Client eine leere
  Liste liefert. Die Zahlen stehen mit den übrigen Feldern in
  `katalog-cache.json` und gelten ohne Netz weiter (`uebernimmZahlen`,
  Zeile 500–510).
- **Neue Prüfung (`test/markt35.mjs`):** Eigene Fläche unter
  `/tmp/board-scheibe1/markt35`, eigenes `DSH_HOME`, daneben ein
  nachgebildeter GitHub-Server auf `127.0.0.1`. Fünf kuratierte Einträge
  (`station-a`, `station-b`, `station-c`, `workflow-w`, `vorhaben-v`) und ein
  Suchtreffer `fremd/station-d`. Geprüft wird:
  1. `station-a`: `sterne: 7`, `fragen: 2`, `beitraege: 1` (die Issues-Liste
     enthält zusätzlich einen Pull Request), Fassung `v9` aus der Freigabe,
     zwei Beispiele aus `beispiele/` und eine Prüfung mit Person und Datum
     (`eigen/station-a`, Thomas Heinrich, 2026-10-20).
  2. `station-b`: Namen aus `station.json` verdrängen die Datei im Ordner;
     `fragen: 0` und `beitraege: 0` bleiben ermittelbare Nullen; die Prüfung
     wird über den lokalen Pfad zugeordnet (Anna Beispiel, 2026-10-21).
  3. `station-c` ohne Remote: `sterne`, `fragen`, `beitraege` und `geprueft`
     sind `null`, nicht 0; `beispiele` ist leer. Workflow und Vorhaben tragen
     trotz eigenem `beispiele/`-Ordner keine Beispiele. Der gefundene Eintrag
     hat 5 Sterne, aber `fragen`/`beitraege`/`geprueft` `null`.
  4. `katalog-cache.json` hält die zwei Zahlen, Beispiele und Prüfung; nach
     dem Schließen des Servers liefert `?frisch=1` die letzten Zahlen (2/1),
     Sterne und Prüfung weiter, mit Hinweis; `katalog.json` bleibt
     unverändert. Kein echtes Netz.
- **Hinweis:** Der Ordner heißt im Auftrag `beispiele/`, nicht `examples/`.
  Die Stationsseite (`index.js`) liest weiterhin `examples/`; das ist von
  diesem Schritt nicht berührt.

## Dateien

- `katalog.js` (geändert): Modulkopf; `beispieleLesen` (173–190);
  Prüfungszuordnung (195–242); `beispiele`/`beitraege`/`geprueft` in
  `eintragLesen` (319–367) und `gefundenerEintrag` (248–265);
  `ergaenzeFelder` für ältere Zwischenspeicher (402–412); `uebernimmZahlen`
  und `uebernimmAlt` (500–521); frische Prüfung im schnellen Weg (545–563);
  Zählung über `fragen` und `beitraege` (640–661); Zuordnung der Prüfung in
  der Ausgabe (712–720).
- `test/markt35.mjs` (neu).
- `github.js` unverändert; `formate/katalog.js` unverändert.
- Keine eigenen Commits.

## Prüfung

- `cd /workspace/projekte/board-plugin && node --test test/markt35.mjs`
  → **Exit-Code 0**; entscheidende Zeile:
  `ok Katalog: Beispiele, Prüfung, Sterne, Fragen und Beiträge getrennt`
- `cd /workspace/projekte/board-plugin && node --test` → **Exit-Code 0**;
  `ℹ tests 51`, `ℹ pass 51`, `ℹ fail 0` (50 vorhandene plus der neue Test).
- `node --check katalog.js` → **0**; `node --check github.js` → **0**.

## Folge

Jeder Katalogeintrag trägt jetzt Beispiele, eine Prüfung, Sterne sowie
getrennte Zahlen für offene Fragen und offene Beiträge. Die Anzeige in
`client.js` zeigt weiterhin nur Sterne, Fassung und Adresse; die neuen Felder
stehen in der Antwort und können in einem folgenden Schritt sichtbar gemacht
werden.

## Grenzen

- **Kein echtes Netz.** Der nachgebildete Server deckt Suche, Metadaten,
  Issues, Pull Requests und Freigaben ab.
- **Kein Browserdurchgang.** Der laufende Server trägt den älteren Stand und
  wurde nicht benutzt; geprüft wurde mit `node --test`.
- **Keine eigenen Commits.**
