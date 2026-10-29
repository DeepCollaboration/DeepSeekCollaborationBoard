# Bericht Schritt 40 — die alte Beitragsablage ist weg

## Gebaut

- **Alte Wege entfernt (`index.js`):** `/board/beitrag` (GET und POST),
  `/board/beitrag/entscheiden` und der lokale Fork
  `/board/vorhaben/forken` sind nicht mehr angemeldet. Ein Aufruf läuft ins
  Leere und antwortet 404. Die zugehörigen Helfer (`beitragDateien`,
  `beitragKennung`) und die Importe der alten Schreibfunktionen sind
  mitgegangen. `/board/github/beitrag` und `/board/github/fork` bleiben
  unverändert erreichbar.
- **Keine Schreibwege mehr (`ablage.js`):** `schreibeBeitrag` und
  `schreibeEntscheid` sind entfernt; damit gibt es auch auf Bibliotheksebene
  keinen Weg mehr, der unter `beitraege/` schreibt. `liesBeitraege` bleibt:
  alte Einträge werden weiter gelesen und über `/board/data` ausgeliefert.
- **Oberfläche (`client.js`):** Der Entscheid im Board, der Aufruf von
  `/board/beitrag/entscheiden`, das Nachladen eines Beitrags über
  `/board/beitrag` und der lokale Fork samt Knopf sind entfernt. Alte
  Beiträge lassen sich weiterhin öffnen und lesen — Notiz, Station, Name der
  vorgeschlagenen Fassung und ein früherer Entscheid stehen da; entschieden
  wird nicht mehr. Forken läuft über `/board/github/fork`, Beitragen über
  `/board/github/beitrag`.
- **README:** Der Abschnitt „Beitragen, forken, fragen“ nennt den Beitrag
  weiterhin als Pull Request und sagt jetzt ausdrücklich, dass die frühere
  Ablage unter `beitraege/` nicht mehr beschrieben wird und vorhandene
  Einträge lesbar bleiben.
- **Neue Prüfung (`test/markt40.mjs`):** Alte Ablage der Fassung 1 unter
  `/tmp/board-scheibe1/markt40`, Server in-process, ein kleiner HTTP-Server auf
  `127.0.0.1` mit 404-Vorgabe für nicht angemeldete Pfade. Geprüft wird:
  die drei alten Wege antworten 404; es entsteht kein `beitraege/`-Ordner,
  kein lokaler Fork und keine Fassung; `/board/github/beitrag` und
  `/board/github/fork` antworten nicht 404; ein alter Beitrag bleibt mit
  Notiz und Dateiname über `/board/data` lesbar; `client.js` ruft die alten
  Pfade nicht mehr auf. Kein echtes Netz.
- **Alte Beitrags-Prüfungen angepasst:** `test/schritt11.mjs`,
  `test/schritt12.mjs`, `test/schritt13.mjs` und `test/schritt15.mjs` prüfen
  statt des alten Schreibens jetzt, dass die alten Wege 404 liefern, nichts
  schreiben und alte Daten lesbar bleiben; ihre übrigen Absichten (fremdes
  Vorhaben unberührt, Personendatei, Basis/409, Zweig-Herkunft,
  Quelltextprüfungen) bleiben erhalten. `test/schritt40.mjs` erwartet für den
  alten Beitragsweg im Archiv jetzt 404 statt 403; das Schreiben über
  `/board/doc/schreiben` bleibt bei 403.

## Dateien

- `index.js` (geändert): drei alte Routen samt Helfern entfernt, GitHub-Wege
  unberührt, `/board/data` liefert `beitraege` weiterhin.
- `ablage.js` (geändert): `schreibeBeitrag` und `schreibeEntscheid` entfernt;
  `liesBeitraege` bleibt.
- `client.js` (geändert): alter Entscheid, altes Beitrags-Lesen über die
  entfernte Route und lokaler Fork entfernt; alte Beiträge nur noch lesbar;
  GitHub-Beitrag und GitHub-Fork bleiben.
- `README.md` (geändert): Hinweis auf die stillgelegte `beitraege/`-Ablage.
- `test/markt40.mjs` (neu).
- `test/schritt11.mjs`, `test/schritt12.mjs`, `test/schritt13.mjs`,
  `test/schritt15.mjs`, `test/schritt40.mjs` (angepasst).

## Befehl und Ergebnis

```
cd /workspace/projekte/board-plugin && node --test test/markt40.mjs
```

Exit-Code: **0**

Entscheidende Zeile:

```
ok Alte Wege weg, GitHub-Wege da, nichts geschrieben
```

```
cd /workspace/projekte/board-plugin && node --test
```

Exit-Code: **0** — 56 Tests, 56 bestanden, 0 Fehler (55 vorhandene plus
`test/markt40.mjs`).

```
node --check index.js
node --check client.js
```

Exit-Code: je **0**.

## Grenzen

Nur Dateien im Produktordner und `test/markt40.mjs` geändert; kein Netz,
keine eigenen Commits. Die echte Ablage unter `/workspace/projekte/board`
blieb unberührt.
