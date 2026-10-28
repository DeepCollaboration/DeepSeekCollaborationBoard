# Bericht Nachbesserung 1 — Konvergenzbefunde

- **Stand:** 2026-10-26
- **Geändert:** A1, A3, A4, A8, A9, A13, A14; die Prüfskripte 08, 10, 12,
  15 und 16 folgen der neuen Form, 02–16 sind grün.
- **Nicht getan:** kein Neustart, kein Netz, keine echte Sitzung. Die echte
  Ablage bekam weder eine fremde Person noch einen Beitrag; `$DSH_HOME/skills`
  der echten Installation blieb unverändert. In `arbeit/pruefung.md` steht kein
  neues Urteil.
- **Fassung:** „Konvergenz-Nachbesserung“ am Ende des Laufs.

## Was geändert wurde

### A1 — Die Anweisung bleibt für die Dauer der Sitzung

- `produkt/index.js:1047` setzt die Anweisung vor `steuerung.create` ein und
  gibt den Laufnamen zurück; ein vorhandener Ordner am selben Schritt wird
  ersetzt.
- `produkt/index.js:1070` nimmt nur einen geprüften Namen unter
  `$DSH_HOME/skills` wieder weg.
- Beim Start (`produkt/index.js:3505–3545`) bleibt der Einsatz nach dem
  Anlegen der Sitzung liegen und wird als `einsatz` am Gespräch in
  `$DSH_HOME/board/gespraeche.json` gemerkt. Scheitert der Start, wird der
  Einsatz entfernt, die angelegte Sitzung abgebrochen und kein Gespräch
  eingetragen.
- `/board/gespraech/stopp` (`produkt/index.js:3639`) und
  `/board/gespraech/verwerfen` (`produkt/index.js:3668`) nehmen den gemerkten
  Einsatz weg und löschen den Vermerk; Verwerfen entfernt auch das Gespräch.
- `produkt/test/schritt08.mjs` prüft: ein alter Rest wird ersetzt, die Datei
  liegt nach dem Start, der Einsatz steht an `gespraeche.json`, nach `stopp`
  und nach `verwerfen` ist sie weg. `produkt/test/schritt10.mjs` prüft dieselbe
  Lebensdauer am fremden Schritt.

### A3 — `/board/konfiguration`

- Neue Route `GET /board/konfiguration` (`produkt/index.js:1651`) mit genau
  `{ person, repo, remote, format }`; `remote` ist die Adresse von `origin`
  oder `null`.

### A4 — `person` und `repo` in `/board/data`

- `datenAntwort` (`produkt/index.js:1577`) ergänzt `person` (eigene Kennung)
  und `repo` (aufgelöster Ablageort aus `board.config.json`).
- `produkt/test/schritt03.mjs` prüft beide Schlüssel und die neue Route;
  `produkt/test/schritt05.mjs` und `-11.mjs` führen die erweiterte Schlüsselliste.

### A8 — Speichern ohne Basis prüft

- `/board/doc/schreiben` (`produkt/index.js:1843`) verlangt für eine
  vorhandene, gefasste Datei eine `basis`. Fehlt sie, antwortet die Route 409
  mit `{ ok: false, neu: { text, basis } }` und schreibt nicht. Eine neue,
  noch nicht gefasste Datei darf ohne Basis angelegt werden; `basis: null`
  bleibt dafür gültig.
- `produkt/test/schritt15.mjs` erwartet bei `notiz.md` ohne Basis den 409 mit
  byte-gleicher Datei und ohne Fassung und legt `frisch.md` ohne Basis neu an.
- `produkt/test/schritt16.mjs` speichert die gefasste Prüfdatei mit ihrer
  Basis, weil sie inzwischen gefasst ist.

### A9 — Annehmen prüft die Basis

- `/board/beitrag/entscheiden` (`produkt/index.js:2104`) vergleicht vor dem
  Annehmen je übernommener Datei, ob sie seit der Basis des Beitrags verändert
  wurde (`dateiUnveraendertSeit`, `produkt/index.js:789`). Ist das der Fall,
  wird nichts geschrieben und nichts entschieden: 409 mit dem Hinweis
  „Der Stand ist neuer“ und den aktuellen Basen unter `neu`.
- `teile` grenzt die Prüfung auf die tatsächlich übernommenen Dateien ein.
- `produkt/test/schritt12.mjs` prüft den 409 mit der neuen Basis, die
  unveränderte Datei, den offenen Beitrag und den Teile-Fall (nur `a.md`
  geprüft, `vision.md` bleibt).

### A13 — Die Skill-Texte nennen nicht mehr `board.json`

- In allen fünf Dateien unter `skills/p1/` sind die drei genannten Stellen
  ersetzt: der Ordner des Vorhabens, `vorgaenge/<ordner>/vorhaben.json` und
  `workflows/<person>.json`; in `umsetzen.md` führt `laeufe[].sitzung` zum
  Vorhaben.
- `grep -rn "board.json" skills/p1/` findet nichts mehr.

### A14 — Nur die eigene Person

- `baueDateien` (`produkt/ablage.js:577`) schreibt nur noch
  `personen/<eigene>.json`; die Dateien anderer Personen bleiben unberührt.
- `/board/person/aendern` (`produkt/index.js:2618`) antwortet 403, wenn `id`
  nicht die eigene Person ist.
- `produkt/test/schritt12.mjs` prüft beides: `personen/p2.json` bleibt
  byte-gleich (auch mit einem eigenen Feld) und die fremde Kennung bekommt 403.

### Angepasste Prüfskripte

`produkt/test/schritt03.mjs`, `-05.mjs`, `-08.mjs`, `-10.mjs`, `-11.mjs`,
`-12.mjs`, `-15.mjs`, `-16.mjs`. Die inhaltlichen Prüfungen der übrigen
Skripte blieben unverändert.

## Prüfung

Im Produktordner
`/workspace/projekte/board/vorgaenge/board-gesamtkonzept/produkt`:

```bash
for n in 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16; do
  node "test/schritt$n.mjs" || exit 1
done
```

**Exit-Code: 0** für alle fünfzehn Skripte (02–16).

Einzeln:

| Prüfung | Exit | Entscheidende Zeile |
| --- | --- | --- |
| `node test/schritt08.mjs` | 0 | `ok Lauf: p1-idee-vision/SKILL.md liegt nach dem Start, Einsatz am Gespräch gemerkt`; `ok Stopp: p1-idee-vision/SKILL.md weg, Einsatz am Gespräch gelöscht`; `ok Verwerfen: p1-vision-fachkonzept/SKILL.md weg, Gespräch entfernt` |
| `node test/schritt10.mjs` | 0 | `ok Start: p2-pruefen/SKILL.md liegt nach dem Start, aus der Ablage`; `ok Stopp: p2-pruefen/SKILL.md ist weg` |
| `node test/schritt15.mjs` | 0 | `ok ohne Basis: 409 mit dem neuen Text und der Basis …, Datei byte-gleich`; `ok neue Datei: ohne Basis angelegt, danach gefasst` |
| `node test/schritt12.mjs` | 0 | `ok Basis beim Annehmen: 409 mit neuer Basis, nichts geschrieben`; `ok teile: nur a.md geprüft und übernommen, vision.md bleibt` |
| `node test/schritt03.mjs` | 0 | `/board/data` trägt `person` und `repo`; `/board/konfiguration` liefert `format,person,remote,repo` |
| `node test/schritt02.mjs` … `schritt16.mjs` | 0 | alle fünfzehn Läufe grün |

Weitere Prüfungen:

```bash
grep -rn "board.json" /workspace/projekte/board/skills/p1/   # kein Treffer (Exit 1)
git -C /workspace/projekte/board status --porcelain            # leer nach der Fassung
git -C /workspace/projekte/board log -1 --format='%an %s'      # Thomas Heinrich Konvergenz-Nachbesserung
```

## Grenzen

- Kein Harness-Neustart; die laufende Seite wurde nicht geprüft.
- Kein Modellaufruf und keine echte Sitzung; die Prüfskripte ersetzen
  `sessionController`, `llm` und die Sitzungsdateien.
- Die echte Ablage hat weiterhin nur `p1`, keinen Beitrag und keinen Remote.
- `$DSH_HOME/skills` der echten Installation ist unverändert; alle Einsätze
  fanden in den `$DSH_HOME`-Kopien der Prüfskripte statt.
