# Bericht Schritt 23 — Die Abnahme

## Gebaut

- **`werkzeuge/abnahme.mjs`** (neu): ein Lauf, der den ganzen Weg belegt.
  - Die Prüfungen der neuen Form von `test/schritt29.mjs` bis
    `test/schritt40.mjs`, soweit vorhanden — mit `test/schritt33b.mjs` sind es
    zwölf. Je Lauf werden Exit-Code, Dauer und die erste `ok`-Zeile festgehalten;
    `test/schritt40.mjs` gibt es nicht, das steht im Bericht.
  - Die ganze Suite `node --test` mit Zahl bestanden/gesamt.
  - Eine lesende Live-Probe über `github.js` gegen
    `DeepCollaboration/DeepSeekCollaborationBoard`: Sterne, Sichtbarkeit,
    Standardzweig. Sie läuft ohne Token in einem eigenen temporären Home, damit
    der Stern-Zwischenspeicher nicht in die echten Dateien schreibt. Bei Netz-
    oder Grenzfehler steht „live nicht geprüft“ und der Lauf bleibt bestehen.
  - `komponiere` für die echten Boards: Hauptboard 11 Schritte, 0 Lücken;
    Team-Board 11 Schritte, 4 Vorhaben, 0 Lücken.
  - Das Register `$DSH_HOME/board/boards.json`: drei Boards, aktiv
    `hauptboard-neu`.
  - Die alte Ablage: `HEAD ae905d8`, 42 Fassungen, Board-Datenpfade ohne
    Änderung; dazu ein Vorher/Nachher-Vergleich der Köpfe und offenen
    Änderungen aller 19 echten Repositories.
  - Die sieben Wege mit ihrer Prüfung: Hauptboard ohne Einrichten (33), Station
    prüfen und sternen (27/31), Probeprojekt übernehmen (38), privates
    Team-Board (30/31/32), Beitrag als Pull Request (35), Fork (37), Frage (37).
  - Ausgabe als Markdown-Tabellen und JSON; `--bericht <datei>` schreibt den
    Bericht. Exit 1, wenn eine Prüfung der neuen Form, die Suite, ein echtes
    Board, das Register oder die alte Ablage fehlschlägt — nicht bei „live nicht
    geprüft“.
- **`arbeit/abnahme.md`** (vom Werkzeug geschrieben): Datum, Befehl, Exit-Code,
  entscheidende Zeile, die sieben Wege mit Ergebnis und Beleg, die Prüfungen
  der neuen Form, die Suite, die Live-Probe, die echten Boards, Register und
  alte Ablage, Grenzen.
- **`umsetzung.md`** neu gefasst für den ganzen Stand: Architektur, was
  entstanden ist, Start, was es kann, was es nicht kann, ungeprüft, wichtigste
  Entscheidungen, wie geprüft wurde, Abweichungen von der Architektur, Dateien.
- **Eine Abweichung ist offengelegt:** Der Auftrag nennt für das Hauptboard
  „0 Vorhaben“. Seit Schritt 21 liegt dort das öffentliche Probeprojekt; die
  Abnahme prüft deshalb „0 Vorhaben außer dem Probeprojekt“ und sagt in Tabelle
  und Bericht, dass die Vorgabe von vor Schritt 21 stammt.

## Geänderte Dateien

- `/workspace/projekte/board-plugin/werkzeuge/abnahme.mjs` (neu)
- `/workspace/projekte/board/vorgaenge/board-gesamtkonzept/arbeit/abnahme.md`
  (vom Lauf geschrieben)
- `/workspace/projekte/board/vorgaenge/board-gesamtkonzept/umsetzung.md`
  (neu gefasst)

## Befehl und Ergebnis

- `cd /workspace/projekte/board-plugin && node werkzeuge/abnahme.mjs --bericht /workspace/projekte/board/vorgaenge/board-gesamtkonzept/arbeit/abnahme.md`
  → Exit-Code 0.
  - Entscheidende Zeile: `Abnahme bestanden: 12 von 12 Prüfungen der neuen Form
    bestanden; Suite 39 von 39; sieben Wege belegt; live geprüft (0 Sterne,
    öffentlich, main); Hauptboard 11 Schritte/1 Vorhaben/0 Lücken (Vorgabe 0
    Vorhaben durch Schritt 21 überholt), Team 11 Schritte/4 Vorhaben/0 Lücken;
    Register 3 Boards, aktiv hauptboard-neu; alte Ablage HEAD ae905d8
    unverändert.`
- `node --test` → 39 von 39 bestanden, Exit-Code 0.
- `node --check` über alle 61 JavaScript-Dateien des Produkts (Module, Formate,
  Werkzeuge, Tests) → Exit-Code 0.
- Die echten Repositories blieben unberührt: `board-hauptboard` `752c72c`,
  `board-team` `7b84627`, `vorhaben-probeprojekt` `db28ec1`,
  `workflow-standard` `f8b809e`, `station-idee` `72a6332`; alte Ablage
  `ae905d8`, ihre Board-Datenpfade ohne Änderung. Der Abnahmelauf selbst
  vergleicht alle 19 Repositories vorher und nachher.

## Grenzen

- **Kein echter GitHub-Schreibaufruf.** Pull Request, Frage, Fork, Teams,
  Veröffentlichen und Push sind gegen einen nachgebildeten Server und lokale
  Bare-Repositories geprüft; live geprüft ist nur das Lesen.
- **Kein Token.** Die Live-Probe liest öffentlich; private Metadaten und die
  Schreibwege bleiben ungeprüft.
- **Die neuen Repositories sind lokal, ohne Remote und nicht veröffentlicht.**
  `DeepCollaboration/board-hauptboard` gibt es auf GitHub nicht; der Erststart
  ist mit lokalen Bare-Repositories belegt. Probeprojekt und Katalog sind
  ebenfalls lokal.
- **Die Abnahme durch den Auftraggeber steht aus.** Der laufende Web-Prozess
  trägt bis zum Neustart den beim Start geladenen Stand; die Seite wurde nicht
  im Browser durchgeklickt.
- **Keine Commits im Produkt.** Der Arbeitsbaum trägt 46 unversionierte oder
  geänderte Dateien; `origin/main` steht lokal auf `52b054b`.
- Die Prüfungen arbeiteten auf `/tmp/board-scheibe1/` und räumten selbst auf;
  das echte `$DSH_HOME` und die echten Repositories wurden gelesen, nicht
  geschrieben.
