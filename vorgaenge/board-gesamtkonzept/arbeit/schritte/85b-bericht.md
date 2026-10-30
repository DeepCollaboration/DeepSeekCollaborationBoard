# Schritt 85b — Bericht

## Gebaut

**Nur `client.js` und ein neuer Test.** `index.js`, `rechte.js`,
`texte/*.json`, `locale/*`, `package.json`, die Einstellungen und die
Arbeitsdokumente sind unberührt. Keine neuen sichtbaren Texte: der Client
trägt weiter 605 eigene Schlüssel, jedes Wörterbuch 614, `de.json` bleibt
leer.

## Klassen

| Ort | Klasse | Stelle |
|---|---|---|
| Board-Wurzel (mit `data-board-akzent`) | `board-seite` | `client.js:10240` |
| Kartenhülle | `board-karte`, aktiv zusätzlich `board-karte-aktiv` | `client.js:7914` |
| Schienenknopf | `board-schiene-knopf` | `client.js:8811` |
| Begleiter | `board-begleiter` | `client.js:8843` |
| Augen des Begleiters | `board-begleiter-auge` | `client.js:8859`, `8863` |

Der Stil steht in `AKZENT_STIL` (`client.js:1876`; der neue Block beginnt
mit dem Kommentar in `1887`, die erste Regel in `1890`) und wird wie bisher
als ein `<style>` in den Kopf gehängt (`client.js:10768`). Alle neuen Regeln
für Elemente beginnen mit `.board-seite`; allein die Keyframe-Definition
`board-blinz` ist nicht scoped, weil CSS das nicht vorsieht. Das Dashboard
bleibt unberührt.

## Regeln

- **Karte:** `transition: filter .14s ease`; beim Überfahren
  `filter: brightness(1.04)`; bei `:focus-visible` ein 2-px-Ring in
  `var(--board-akzent)` mit 2 px Abstand. Kein `transform`, keine Position,
  keine Größe — die Verbindungen rechnen weiter mit den Kartenkanten.
- **Fokus überall:** `.board-seite :is(button, input, textarea, select,
  [tabindex]):focus-visible` bekommt denselben Ring. Es gibt keine
  `outline: none`-Regel.
- **Schiene:** `.board-schiene-knopf` ist `position: relative`; beim
  Überfahren nimmt die Beschriftung `var(--dsw-alias-label-primary)` an
  (der Span erbt). Bei `[aria-pressed="true"]` zeigt `::before` einen
  2-px-Balken links, von `top: 6px` bis `bottom: 6px`, in
  `var(--board-akzent)`. Der Balken ist absolut gesetzt und verschiebt
  nichts.
- **Bewegung:** `.board-begleiter-auge` blinzelt mit
  `board-blinz 6s ease-in-out infinite`; die Augen sind bei 0/88/100 %
  offen, bei 91/94 % fast geschlossen und bei 97 % wieder offen.
- **Reduzierte Bewegung:** `@media (prefers-reduced-motion: reduce)`
  schaltet für `.board-seite *`, `*::before` und `*::after` Übergänge und
  Animationen mit `!important` ab.
- **Farben:** ausschließlich `var(--dsw-alias-label-primary)`,
  `var(--dsw-alias-label-secondary)` und `var(--board-akzent)`; keine
  Hex-, rgb- oder hsl-Werte. Keine neuen Token.
- **Maße:** `S.page`, `S.koerper`, `S.karte` und `S.schiene` sind
  zeichengleich geblieben (Padding, Höhe, MinHeight, Radius, Breite,
  Abstände, Rahmen, Hintergrund, Schatten).

## Begleiter

Unten in der Schiene, mit `marginTop: 'auto'` an den unteren Rand gedrückt
und über `display: flex`/`justifyContent: center` zentriert. Die Figur ist
ein SVG in 18 × 18 mit `viewBox="0 0 18 18"`: Kopfkreis, zwei Augen und ein
Mund. `data-art` kommt aus `status.art`:

| `status.art` | `data-art` | Mund |
|---|---|---|
| `fertig` | `fertig` | Lächeln (Bogen) |
| `fehler` | `fehler` | kleines „!“ (Strich und Punkt) |
| alles andere, auch ohne Status | `ruhe` | ruhiger Strich |

Der Behälter ist `aria-hidden="true"`, trägt `pointerEvents: 'none'`, hat
keinen Text, keinen `title` und keinen Klick- oder Zeiger-Handler. Das SVG
ist ebenfalls `aria-hidden` und nicht fokussierbar. Die Augen blinzeln nur
über die CSS-Klasse; bei reduzierter Bewegung steht das Blinzeln still. Der
automatische Außenabstand nimmt nur freien Platz und schiebt die beiden
Schienenknöpfe nicht.

## Testausgabe

```
$ node --check client.js
(keine Ausgabe, Rückgabewert 0)

$ node --test test/*.mjs
ℹ tests 82
ℹ suites 0
ℹ pass 82
ℹ fail 0
ℹ cancelled 0
ℹ skipped 0
ℹ todo 0
ℹ duration_ms 5570.677326
```

`test/nach85b.mjs` prüft: die neuen Regeln in `AKZENT_STIL`; die drei
Klassen an Wurzel, Karte und Schienenknopf; den Begleiter samt
`aria-hidden`, `pointer-events: none`, Textfreiheit, 18 × 18 und den drei
Mündern; das Fehlen von `outline: none` und harten Farben; die unveränderten
Maße der vier Stilblöcke; und die Schlüsselzahl 614 (605 im Client).
Die Datei selbst meldet:

```
ok Feinschliff: Karten mit Hover und Fokusring, Balken im aktiven
Schienenknopf, blinzelnder Begleiter mit drei Mündern, reduzierte Bewegung
abgeschaltet, Maße unverändert, 614 Schlüssel
```

## Gegenproben

Jede Änderung wurde gegen `test/nach85b.mjs` gesetzt und danach die Datei
aus einer Sicherung wiederhergestellt (`cmp` bestätigt Gleichheit):

| Gegenprobe | Ergebnis |
|---|---|
| `prefers-reduced-motion` entfernt | rot |
| `outline: none` eingesetzt | rot |
| `className: 'board-seite'` entfernt | rot |
| `aria-hidden` am Begleiter entfernt | rot |
| `S.schiene` von 72 auf 73 px geändert | rot |

## Abweichungen

Keine. Die Vorgaben aus `85b.md` sind wörtlich umgesetzt; es gibt keine
Änderung an Abständen, Schriftgrößen, Breiten, Höhen, Positionen oder
Reihenfolge, keine neuen Texte und keine neuen Farben.

## Offene Punkte

- **Sichtprüfung:** ob `brightness(1.04)` im hellen und dunklen Modus
  sichtbar, aber ruhig wirkt, ob der 2-px-Balken neben der getönten Fläche
  des aktiven Knopfs gut sitzt und ob der Begleiter unten in der Schiene
  nicht gedrängt wirkt — das entscheidet der Blick im Browser in Schritt 86.
  Ohne Browser ist hier nur der Quelltext belegt.
- **Kartenfokus:** Die Regel `.board-karte:focus-visible` steht wie
  vorgegeben; die Kartenhülle selbst ist nicht fokussierbar, der sichtbare
  Ring erscheint am Namensknopf über die allgemeine Fokusregel. Das ist
  keine Abweichung, aber ein Punkt für die Sichtprüfung.
- **Kurze Fenster:** Bei sehr geringer Höhe könnte der Begleiter den beiden
  Schienenknöpfen Platz nehmen; die Schiene hat keine eigene Scrollfläche.
  Bei üblicher Fensterhöhe bleibt genug Platz.
- Die Abschaltung der Bewegung ist als CSS-Text geprüft, nicht im Browser.
