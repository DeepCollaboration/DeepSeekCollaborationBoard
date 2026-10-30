# Bericht Schritt 90b — Bilder in Stationen

- **Gebaut:**
  - `bild.quelle` ist jetzt entweder eine `https:`-Adresse oder ein relativer
    Bildpfad im Station-Repository (`bilder/aufbau.svg`). Geprüft werden Form
    (kein führender `/`, kein `..`, kein leerer oder mit `.` beginnender Teil,
    nur Buchstaben, Ziffern, `-`, `_`, `.`, `/`) und Endung (`png`, `jpg`,
    `jpeg`, `webp`, `gif`, `svg`). `video` bleibt bei `https:`.
  - Neue Route `GET /board/station/bild?station=<id>&pfad=<relativer Pfad>`.
    Sie löst die Station über das aktive Board auf wie `GET /board/station`,
    verbindet den Pfad mit dem Stationsordner, prüft die Lage nach `realpath`,
    die Endung und die Lesbarkeit (Symlink nach draußen und gesperrte Datei
    → 403), liefert `Content-Type`, `Content-Length`,
    `Cache-Control: private, max-age=60` und `X-Content-Type-Options: nosniff`.
    400 bei ungültigem Pfad, 403 bei verbotener Endung, 404 bei fehlender
    Datei, 413 über 8 MB; keine Verzeichnisliste, kein absoluter Pfad im Text.
  - Client: `bild` mit `https:` wie bisher; ein relativer Pfad geht über die
    Route, mit `alt`, `loading: 'lazy'` (im DOM `loading="lazy"`) und 960 px.
    Fehlt das Bild, bleibt die Bildunterschrift stehen, darunter steht klein
    „Bild nicht gefunden“; das kaputte Bild wird ausgeblendet.
  - Drei eigene SVGs in den Einführungstationen (`vorhaben.svg`,
    `aufbau.svg`, `station.svg`), je ein `bild`-Block nach dem ersten
    Textblock. `legeStartboardAn` kopiert `bilder/` mit in die Station.
  - Neue Prüfung `test/nach90b.mjs` (Format, Route unter `/tmp`, Clienttext,
    SVGs und `bild`-Blöcke).

- **Geänderte Dateien:** `formate/station.js`, `index.js`, `client.js`,
  `erststart.js`, `vorlagen/start/station-vorhaben/{station.json,bilder/vorhaben.svg}`,
  `vorlagen/start/station-kennenlernen/{station.json,bilder/aufbau.svg}`,
  `vorlagen/start/station-eigene/{station.json,bilder/station.svg}`,
  `texte/{en,es,fr,ja,ru,ar,zh}.json`, `test/nach90b.mjs` (neu),
  `test/nach87.mjs`, `test/nach89.mjs`, `test/nach90.mjs`,
  `test/grundlagen.mjs`, `test/nach82.mjs`, `test/nach83.mjs`,
  `test/nach84.mjs`, `test/nach85.mjs`, `test/nach85b.mjs`,
  `docs/datenformate.md`, `docs/routen.md`, `docs/entwicklung.md`,
  `.github/workflows/ci.yml`.

- **Befehl:**
  ```
  cd /workspace/projekte/board-plugin && npm run check && node --test test/*.mjs
  ```

- **Exit-Code:** `0` (check grün, Suite grün: 89 Tests bestanden — die
  bisherigen 88 plus `nach90b`).

- **Entscheidende Zeile:** in `bildAusStation`:
  ```js
  if (echt !== wurzel && !echt.startsWith(wurzel + sep)) {
    return { status: 403, fehler: 'Dieser Pfad liegt außerhalb der Station' };
  }
  ```
  Sie hält auch einen Symlink im Stationsordner davon ab, eine Datei außerhalb
  auszuliefern.
