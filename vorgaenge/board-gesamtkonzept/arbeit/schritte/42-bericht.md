# Bericht Schritt 42 — der Umzug als Route

## Gebaut

- **`POST /board/umzug` (`index.js`):** Nimmt `{ zuordnung?, ziel? }`. Die
  Route liest die alte Ablage aus dem Register — das offene Board, wenn es
  selbst die Fassung 1 ist, sonst das erste Board der Fassung 1 (das
  Hauptboard zuerst). Sie prüft, dass dort `format` den Wert `1` trägt, und
  schreibt nie in die Quelle.
- **Zuordnung:** Ohne `zuordnung` gilt die vorhandene Abbildung Kennung →
  GitHub-Name: zuerst der GitHub-Name aus `identitaet.json`, wenn er zu einer
  Person der alten Ablage passt (über den Namen), dann ein GitHub-Name an der
  Person selbst, zuletzt der Name, unter dem die Boards der Fassung 2 im
  Register stehen (in diesem Haus `p1` → `DeepCollaboration`). Eine
  mitgegebene Zuordnung wird geprüft (Kennungen nicht leer, Werte gültige
  GitHub-Namen, höchstens 39 Zeichen). Die Antwort nennt die Abbildung unter
  `zuordnung` samt `host`, bevor der Lauf beginnt; fehlt für die Kennung der
  alten Ablage ein Name, antwortet der Weg 400.
- **Lauf:** Der vorhandene `werkzeuge/umzug2.mjs` läuft als eigener Prozess
  mit `--quelle`, `--ziel`, `--host` und `--bericht`
  (`GIT_TERMINAL_PROMPT=0`, 300 s Grenze). Ohne `ziel` entsteht ein neuer
  Ordner `$DSH_HOME/board/umzug/umzug-<Zeitstempel>`; ein vorhandener, leerer
  Zielordner wird benutzt. Die Quelle wird nur gelesen.
- **Bericht:** Die Antwort trägt `quelle`, `ziel`, `zuordnung`, `host`,
  `bericht`, `hinweis`, `anzahl` und je Repository
  `{ repo, pfad, art, name, github, sichtbarkeit, stand, fehler }`. `stand`
  ist `angelegt` oder `übersprungen`; `github` kommt aus der Ankerdatei
  (`host`, `autor`, `personen[].login`, `verantwortliche`), die Sichtbarkeit
  aus der Karte neben den Repositories. Zusätzlich hängt die Route an
  `<ziel>/umzug.md` die Abschnitte `## Zuordnung` und `## Repositories (n)`
  mit einer Zeile je Repository an — die Zuordnung steht damit im Bericht.
- **Stopp:** Ohne Schreibrecht des offenen Boards antwortet der Weg 403 mit
  Grund und Hinweis und legt kein Ziel an. Ohne Ziel (leerer oder falscher
  Wert) 400; ein Ziel in der alten Ablage oder eine alte Ablage im Ziel 400;
  eine ungültige Zuordnung 400. Ist das Ziel nicht leer, wird nichts
  geschrieben: 200, `hinweis` „Das Ziel ist nicht leer; es wird nichts
  überschrieben.“, alle vorhandenen Repositories als `übersprungen`.
- **Ausnahmen:** `/board/umzug` steht in `OHNE_ABLAGE_SCHREIBEN` (er schreibt
  keine Ablagedatei des offenen Boards) und in `ARCHIV_ERLAUBT` (er liest das
  Archiv und schreibt daneben). Die Routenübersicht im Kopf nennt den Weg.
- **Neue Prüfung (`test/markt42.mjs`):** Kopiert die echte alte Ablage
  vollständig nach `/tmp/board-scheibe1/markt42/quelle` und vergleicht deren
  Prüfsumme (alle Dateien samt `.git`) vorher und nachher. Geprüft wird:
  GET → 405; ohne Schreibrecht (Board mit Remote, kein Token) 403 ohne Ziel;
  leeres Ziel, Ziel in der Quelle und ungültige Zuordnung je 400; ein Aufruf
  ohne Angaben schreibt 18 Repositories (11 Stationen, 1 Workflow, 4
  Vorhaben, 2 Boards; 13 öffentlich, 5 privat) mit Namen, GitHub-Namen und
  `angelegt`, die Zuordnung `p1` → `DeepCollaboration` steht in Antwort und
  Bericht, die Kopie ist byte-gleich; ein zweiter Lauf auf dasselbe Ziel
  nennt alle 18 als `übersprungen` und lässt Ziel und Quelle byte-gleich;
  nach `POST /board/identitaet` gilt `p1` → `thomas-heinrich`; eine eigene
  Zuordnung `p1` → `UmzugBot` wird auf einer kleinen Ablage im Archiv benutzt
  (4 Repositories, Station trägt den Namen), das Archiv bleibt unverändert.
  Die Prüfablage wird danach entfernt.

## Dateien

- `index.js` (geändert): die Route `/board/umzug`, die Helfer
  `istGithubName`, `umzugQuelle`, `zuordnungAusBestand`, `pruefeZuordnung`,
  `hostAusZuordnung`, `umzugZielVorgabe`, `pruefeUmzugZiel`, `liegtInnen`,
  `githubAusAnker`, `umzugRepoBild`, `umzugRepositories`,
  `ergaenzeUmzugBericht`, die beiden Ausnahmen, der Import `fileURLToPath`
  und die Routenübersicht im Kopf.
- `test/markt42.mjs` (neu).

`werkzeuge/umzug2.mjs`, `client.js` und `README.md` blieben unverändert.

## Befehl und Ergebnis

```
cd /workspace/projekte/board-plugin && node --test test/markt42.mjs
```

Exit-Code: **0**

Entscheidende Zeile:

```
ok Umzug: Route, Zuordnung, alte Ablage unverändert
```

```
cd /workspace/projekte/board-plugin && node --test
```

Exit-Code: **0** — 58 Tests, 58 bestanden, 0 Fehler (57 vorhandene plus
`test/markt42.mjs`).

```
node --check index.js
node --check test/markt42.mjs
```

Exit-Code: je **0**.

## Grenzen

Nur Dateien im Produktordner und `test/markt42.mjs` geändert; kein Netz, keine
eigenen Commits. Die echte alte Ablage wurde nur gelesen und kopiert: ihr HEAD
steht weiter auf `ae905d81b080cbb9aae986ca2804723b24dd2d6a`, der Arbeitsbaum
wurde von dieser Arbeit nicht angefasst. Im echten `$DSH_HOME/board` entstand
kein `umzug`-Ordner; die Route lief nur in der Prüfung unter `/tmp`.
