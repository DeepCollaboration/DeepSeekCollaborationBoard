# Schritt 81 — Bericht

## Gebaut

Die Seite hat jetzt den Rahmen aus dem Schritt: unter der Kopfzeile und dem
Hinweis „Du liest mit“ liegt der Streifen `S.koerper` (`display: flex`,
`flex: 1`, `minHeight: 0`). Darin stehen von links die Schiene (72 px), das
offene Dock (360 px) und der bisherige `raum` mit `flex: 1; minWidth: 0`.
Der `raum` behält `ref: raumRef`, seine Reihenfolge und alle Kinder; ergänzt
ist an ihm nur `minWidth: 0`, damit er neben dem festen Dock schrumpfen kann.
Die Fläche nimmt ihre Höhe aus dem Körper, nicht aus ihrem Inhalt: Ob das
Dock offen ist, ändert ihre Höhe nicht.

**Schiene.** `S.schiene` ist 72 px breit, volle Höhe, eine Spalte, oben
beginnend, mit `borderRight` und Hintergrund `var(--dsw-alias-bg-layer-1)`.
Zwei Knöpfe untereinander, je ein 20 × 20-SVG mit `stroke: currentColor` und
`fill: none` (Bücherregal für die Bibliothek, Uhr für den Verlauf) und die
Beschriftung in 12 px. Der offene Ort trägt `aria-pressed: true`, die Farbe
`var(--dsw-alias-label-primary)` für Schrift und Symbol und einen abgesetzten
Hintergrund; der geschlossene bleibt bei `var(--dsw-alias-label-secondary)`.
Jeder Knopf hat einen `title`; er nennt den Zustand mit dem vorhandenen Pfeil
`▸`/`▾`. Der Pfeil ist kein neuer Text: die beiden Zeichen stehen schon als
Schlüssel in den Wörterbüchern, und `texte/*.json` bleibt in diesem Schritt
unberührt. Der Klick toggelt `setLeiste`; ein zweiter Klick und ✕ schließen
über `setLeiste(null)`.

**Dock.** `S.dock` ist 360 px breit, volle Höhe, eine Spalte, mit
`borderRight`, Hintergrund `var(--dsw-alias-bg-layer-1)`, `minHeight: 0` und
ohne `position`, `margin` oder Radius. Darin liegt das vorhandene
`bibliothekPanel` mit seinen Abschnitten und dem verschachtelten Katalog oder
der vorhandene Verlauf. Der Verlauf ist jetzt ein eigener `Panel`-Aufruf mit
Kopf, ✕, Escape und Fokus; der Inhalt rollt in `S.bibInhalt`, der Kopf bleibt
stehen. Beide Panels holen den Fokus über `fokus: true` und geben ihn über
`onSchliessen` an den Schienenknopf zurück. Welches Dock steht, entscheidet
`const dock = leiste === 'bibliothek' ? bibliothekPanel : leiste === 'verlauf'
? verlaufPanel : null`.

**Obere Leiste.** Die beiden Reiter „Bibliothek“ und „Verlauf“ sind fort:
`const reiter`, `S.reiterOffen` und `S.reiterPfeil` sind gelöscht; `S.reiter`
bleibt für „Alles zeigen“. Suchfeld und „Alles zeigen“ stehen unverändert in
`leistenReiter` über der Fläche.

## Bestandsaufnahme und neue Zeilen

| Stelle | vorher | nachher |
|---|---:|---:|
| Stilblock `S` | 177 | 177 |
| `S.leiste` / `S.reiter` | 479 / 483 | 478 / 485 |
| `S.reiterOffen` / `S.reiterPfeil` | 489 / 494 | gelöscht |
| `S.leistenFlaeche` / `S.bibInhalt` | 525 / 532 | gelöscht / 554 |
| `S.koerper`, `S.schiene`, `S.schienenKnopf`, `S.schienenKnopfAktiv`, `S.dock` | — | 494, 497, 503, 511, 547 |
| `Panel` | 1956 | 1978 (unverändert) |
| `leiste` / `bibAnsicht` / `raumRef` | 3134 / 3147 / 3229 | 3156 / 3169 / 3251 |
| `katalogPanel` / `bibliothekPanel` | 8480 / 8490 | 8503 / 8513 |
| `schienenBild` / `schienenKnopf` / `schiene` | — | 8530 / 8545 / 8557 |
| Reiter-Funktion `reiter` | 8506 | gelöscht |
| `leistenReiter` | 8641 | 8690 |
| `verlaufFlaeche` / `verlaufPanel` / `dock` | 8653 / — / — | 8700 / 8718 / 8732 |
| Körper im Seitenbau | 10094–10170 | 10166–10241 |
| `ref: raumRef` | 10099 | 10170 |

## Tests

- **`test/nach81.mjs`** (neu, 203 Zeilen): prüft die fünf Punkte der
  Schritt-Prüfung — die Stile `S.schiene`/`S.dock` (Dock ohne `position`,
  `margin`, Radius und feste Höhe), die Reihenfolge Schiene → Dock → `raum`
  im Körper, den Bibliotheksinhalt und den Verlauf im Dock, die zwei
  Schienenknöpfe mit `aria-pressed` und abgesetztem aktivem Zustand, das
  Fehlen der alten Reiter samt Funktion und Stilen sowie den `raum` mit
  `flex: 1; minWidth: 0`, ohne `display: none` und ohne feste Höhe.
- **`test/nach61.mjs`**: In der Flächenliste der Bibliothek steht jetzt
  `seiteStil: S.dock` statt `S.leistenFlaeche`. Die Absicht bleibt: eigener
  `Panel`-Aufruf, Schließen, Fokus, verschachtelter Katalog. Der Kopf der
  Datei nennt die neue Hülle.
- **`test/nach64.mjs`**: Abschnitt 2 prüft nicht mehr `maxHeight:
  calc(100vh …)`, sondern die neue Höhenkette: `S.koerper` mit `flex: 1` und
  `minHeight: 0`, `S.dock` als Spalte mit `minHeight: 0` und ohne
  `maxHeight`/`height`, `bibInhalt` mit `flex: 1`, `minHeight: 0` und
  `overflow: auto`. Die Absicht — die Bibliothek bleibt im Bild, nur der
  Inhalt rollt — bleibt damit geprüft. Der Schlusssatz der Datei ist
  angepasst.

## Unberührt

`index.js`, `texte/*.json`, `locale/*`, Einstellungen, Chat, Leser, Dateien,
offene Station und Kopfzeile sind nicht angefasst. `Panel` selbst ist
unverändert; der `raum` behält `ref`, Reihenfolge und Kinder. Es wurde nichts
committet.

## Testergebnis

```
cd /workspace/projekte/board-plugin && node --check client.js && node --test test/*.mjs
```

Exit-Code 0. **77 von 77 Prüfungen grün** (76 vorhandene + die neue
`test/nach81.mjs`). Die Flanke um `test/schritt30.mjs` (Git-Stand von
`/workspace/projekte/board`) trat nicht auf; ein zweiter Lauf war nicht nötig.

## Offene Flanken

1. **Fokus beim direkten Wechsel.** Öffnet ein Schienenknopf das Dock, holt
   `Panel` den Fokus herein und gibt ihn beim Schließen an den Knopf zurück,
   der beim Öffnen den Fokus hatte. Wechselt man bei offenem Dock direkt zum
   anderen Schienenknopf, ist das der zuerst geöffnete Knopf; nach dem
   Schließen steht der Fokus also dort, nicht auf dem zuletzt gewählten.
   Für Öffnen und Schließen über ✕ oder Escape stimmt die Rückgabe.
2. **Sichtprüfung.** Die Prüfungen lesen `client.js` als Text. Ob Schiene,
   Dock und Kante im Browser sitzen, zeigt erst die Sichtprüfung in Schritt 85.
3. **Schmale Fenster.** 72 px Schiene plus 360 px Dock sind fest; unter etwa
   500 px Fensterbreite bleibt der Fläche kaum Platz. Das ist die Folge der
   vorgegebenen festen Breiten, nicht dieses Umbaus.
4. **Pfeil im Titel.** Der Zustandspfeil im `title` hält den Schlüssel `▸`
   in Gebrauch. Würden die Wörterbücher später neu erzeugt, kann er durch
   ein sichtbares Symbol am Knopf ersetzt oder entfernt werden.
