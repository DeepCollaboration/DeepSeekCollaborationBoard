# Nachbesserung 1 — Konvergenzbefunde

Kein Bauplan-Schritt, sondern die Abarbeitung der tragenden Abweichungen aus
`arbeit/konvergenz.md`. Kein Neustart, kein Netz. Die echte Ablage bleibt
unberührt; alles wird auf Kopien geprüft. **Schreibe kein Urteil in
`arbeit/pruefung.md`.**

- **Eingang:** `arbeit/konvergenz.md` (A1, A3, A4, A8, A9, A13, A14),
  `produkt/index.js`, `produkt/ablage.js`, `produkt/client.js`,
  `produkt/test/schritt08.mjs`, `produkt/test/schritt10.mjs`,
  `produkt/test/schritt15.mjs`, `skills/p1/*.md`.
- **Tun:**
  1. **A1 — Die Anweisung bleibt für die Dauer der Sitzung.** Der Schritt-Skill
     wird vor `steuerung.create` eingesetzt und **nicht** mehr vor
     `steuerung.prompt` entfernt. Er bleibt unter
     `$DSH_HOME/skills/<host>-<schritt>`, solange die Sitzung läuft, und wird
     weggenommen, wenn die Sitzung endet: bei `/board/gespraech/stopp` und
     `/board/gespraech/verwerfen`; wird am selben Schritt eine neue Sitzung
     begonnen, wird ein alter Einsatz ersetzt. Der Einsatz wird je Sitzung
     gemerkt (etwa in `$DSH_HOME/board/gespraeche.json` am Gespräch), damit das
     Entfernen den richtigen Ordner trifft. Ein Start, der scheitert, hinterlässt
     nichts. `test/schritt08.mjs` und `test/schritt10.mjs` werden auf die neue
     Lebensdauer umgestellt: während `prompt` liegt die Datei, nach `stopp` nicht
     mehr.
  2. **A9 — Annehmen prüft die Basis.** `/board/beitrag/entscheiden` vergleicht vor
     dem Annehmen je übernommener Datei die `basis` des Beitrags mit der aktuellen
     Basis der Datei. Weicht sie ab, wird nichts geschrieben: Antwort 409 mit dem
     Hinweis, dass der Stand neuer ist, und den neuen Basen. `teile` gilt auch für
     diese Prüfung.
  3. **A8 — Speichern ohne Basis prüft.** `/board/doc/schreiben` verlangt für eine
     vorhandene, gefasste Datei eine `basis`; fehlt sie, antwortet die Route 409
     mit der aktuellen Basis, ohne zu schreiben. Eine neue, noch nicht gefasste
     Datei darf ohne Basis angelegt werden. `test/schritt15.mjs` anpassen.
  4. **A14 — Nur die eigene Person.** `baueDateien` schreibt nur
     `personen/<eigene>.json`; die Personendateien anderer bleiben unberührt.
     `/board/person/aendern` antwortet 403, wenn `id` nicht die eigene Person ist.
  5. **A3 — `/board/konfiguration`.** `GET /board/konfiguration` → `{ person, repo,
     remote, format }`.
  6. **A4 — `person` und `repo` in `/board/data`.** Beide Schlüssel ergänzen;
     `repo` ist der Ablageort aus der Konfiguration.
  7. **A13 — Die Skill-Texte nennen nicht mehr `board.json`.** In allen fünf
     Dateien unter `skills/p1/`:
     - „Kennung `ordner` beim Vorhaben in `/workspace/projekte/board/board.json`“
       → „der Ordner des Vorhabens unter
       `/workspace/projekte/board/vorgaenge/<ordner>/`“;
     - „Die Kennungen stehen in `/workspace/projekte/board/board.json`.“ → „Die
       Kennungen stehen in `vorgaenge/<ordner>/vorhaben.json` und
       `workflows/<person>.json`.“;
     - in `umsetzen.md` die Fundstelle „Sonst der Eintrag in
       `/workspace/projekte/board/board.json`, dessen `werkstatt.session` oder eine
       `gespraeche[].session` gleich `$DSH_SESSION_ID` ist“ → „Sonst das Vorhaben,
       dessen `laeufe[].sitzung` gleich `$DSH_SESSION_ID` ist — zu finden in
       `vorgaenge/<ordner>/vorhaben.json`“.
     Keine andere inhaltliche Änderung an den Anweisungen; die Fassungen bleiben
     sonst wörtlich.
  8. Die Prüfskripte 02–16 grün halten (08, 10, 15 angepasst).
  9. Am Ende den Stand der Ablage als Fassung festhalten („Konvergenz-Nachbesserung“).
- **Prüfung:**
  ```
  node .../produkt/test/schritt08.mjs   # Exit 0, neue Lebensdauer
  node .../produkt/test/schritt10.mjs   # Exit 0
  node .../produkt/test/schritt15.mjs   # Exit 0, Basis verlangt
  node .../produkt/test/schritt12.mjs   # Exit 0, Basis beim Annehmen geprüft
  node .../produkt/test/schritt02.mjs … schritt16.mjs   # Exit 0
  grep -rn "board.json" /workspace/projekte/board/skills/p1/   # kein Treffer
  git -C /workspace/projekte/board status --porcelain            # leer
  git -C /workspace/projekte/board log -1 --format='%an %s'      # Thomas Heinrich, Konvergenz-Nachbesserung
  ```
- **Grenzen:** Kein Neustart, kein Netz, keine echte Sitzung. Die echte Ablage
  bekommt keine fremde Person und keinen Beitrag. `$DSH_HOME/skills` der echten
  Installation nicht verändern. Keine fremden Dateien außerhalb von `produkt/`,
  `arbeit/` und `skills/p1/` ändern. Keine Rückfrage.

Am Ende: Bericht nach `arbeit/nachbesserung-1-bericht.md` — was geändert, welche
Dateien, welcher Befehl, Exit-Code, entscheidende Zeile.
