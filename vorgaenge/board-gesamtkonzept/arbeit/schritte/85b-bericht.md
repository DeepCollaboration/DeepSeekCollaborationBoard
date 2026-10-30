# Schritt 85b — Bericht

## Nachbesserung nach der Prüfung

Die Prüfung in `arbeit/pruefung.md` hat einen Kaskadenfehler gefunden:
`S.schienenKnopf` setzte `color: 'var(--dsw-alias-label-secondary)'` als
Inline-Stil; die Autorenregel `.board-schiene-knopf:hover { color: … }`
verlor dagegen. Der Hover-Farbwechsel war wirkungslos, der Test prüfte nur
die Zeichenkette.

**1. Die Farbe steht jetzt im CSS.** Aus `S.schienenKnopf`
(`client.js:535`) und `S.schienenKnopfAktiv` (`543`) ist `color` entfernt.
Stattdessen stehen in `AKZENT_STIL` drei eigene Regeln:

```css
.board-seite .board-schiene-knopf {
  position: relative; color: var(--dsw-alias-label-secondary); }
.board-seite .board-schiene-knopf[aria-pressed="true"] {
  color: var(--dsw-alias-label-primary); }
.board-seite .board-schiene-knopf:hover {
  color: var(--dsw-alias-label-primary); }
```

Die Grundregel (`client.js:1895`), die Regel für den aktiven Knopf (`1897`)
und die Hover-Regel (`1899`) sind getrennt; der Span erbt die Farbe, der
2-px-Balken (`1901`) und der getönte Hintergrund des aktiven Knopfs bleiben
unverändert. Ohne Inline-`color` gewinnt die Hover-Regel (0,3,0 gegen 0,2,0
der Grundregel); beim aktiven Knopf ist die Farbe ohnehin primary.

**2. Der Test ist geschärft.** `test/nach85b.mjs` prüft jetzt zusätzlich:

- `S.schienenKnopf` und `S.schienenKnopfAktiv` enthalten kein `color:`
  (`nach85b.mjs:221–224`). Damit ist der Inline-Stil als Ursache
  ausgeschlossen, nicht nur die CSS-Zeichenkette vorhanden.
- Die Grundfarbe steht in der Grundregel am Klassenknoten; aktiver Knopf
  und Hover sind eigene Regeln mit eigener Farbe (`225–243`).
- Der statische Teil von `AKZENT_STIL` wird als CSS gelesen und in Regeln
  zerlegt: jede Regel steht unter `.board-seite`, Ausnahmen sind nur die
  bestehende `.board-flaeche` und die Keyframes (`193–211`).
- `board-begleiter-auge` steht im CSS und genau zweimal im JSX
  (`174`, `289–290`); fehlt die Klasse an den Augen, blinzelt nichts.
- Der ganze Stilblock `S` (auch `S.page`) wird auf `#`-, `rgb`- und
  `hsl`-Werte geprüft, nicht nur der neue Feinschliff-Abschnitt
  (`337–344`).
- `S.dock` und `S.dockRechts` sind mit ihren Maßen, Abständen, Rahmen und
  Hintergründen mitgepinnt (`383–404`).

**3. Gegenproben.** Neun Änderungen wurden gegen `nach85b` gesetzt und die
Produktdatei danach aus einer Sicherung wiederhergestellt (`cmp` bestätigt
Gleichheit). Alle wurden rot:

| Gegenprobe | Ergebnis |
|---|---|
| `color` wieder inline in `S.schienenKnopf` | rot |
| `className: 'board-begleiter-auge'` an beiden Augen entfernt | rot |
| globale Regel `body { color: var(--dsw-…) }` am Stilende | rot |
| `#ff0000` in `S.page` | rot |
| `prefers-reduced-motion` entfernt | rot |
| `outline: none` eingesetzt | rot |
| `className: 'board-seite'` entfernt | rot |
| `aria-hidden` am Begleiter entfernt | rot |
| `S.schiene` von 72 auf 73 px geändert | rot |

## Gebaut

**Nur `client.js` und ein neuer Test.** `index.js`, `rechte.js`,
`texte/*.json`, `locale/*`, `package.json`, die Einstellungen und die
Arbeitsdokumente sind unberührt. Keine neuen sichtbaren Texte: der Client
trägt weiter 605 eigene Schlüssel, die sieben Wörterbücher je 614,
`de.json` bleibt leer.

## Klassen

| Ort | Klasse | Stelle |
|---|---|---|
| Board-Wurzel (mit `data-board-akzent`) | `board-seite` | `client.js:10242` |
| Kartenhülle | `board-karte`, aktiv zusätzlich `board-karte-aktiv` | `client.js:7916` |
| Schienenknopf | `board-schiene-knopf` | `client.js:8813` |
| Begleiter | `board-begleiter` | `client.js:8845` |
| Augen des Begleiters | `board-begleiter-auge` | `client.js:8861`, `8865` |

Der Stil steht in `AKZENT_STIL` (`client.js:1875`; der statische Block
beginnt mit dem Kommentar in `1886`, die erste Regel in `1889`) und wird
wie bisher als ein `<style>` in den Kopf gehängt (`client.js:10770`). Alle
neuen Regeln für Elemente beginnen mit `.board-seite`; allein die
Keyframe-Definition `board-blinz` ist nicht scoped, weil CSS das nicht
vorsieht. Das Dashboard bleibt unberührt.

## Regeln

- **Karte:** `transition: filter .14s ease`; beim Überfahren
  `filter: brightness(1.04)`; bei `:focus-visible` ein 2-px-Ring in
  `var(--board-akzent)` mit 2 px Abstand. Kein `transform`, keine Position,
  keine Größe — die Verbindungen rechnen weiter mit den Kartenkanten.
- **Fokus überall:** `.board-seite :is(button, input, textarea, select,
  [tabindex]):focus-visible` bekommt denselben Ring. Es gibt keine
  `outline: none`-Regel.
- **Schiene:** `.board-schiene-knopf` ist `position: relative` und trägt die
  Grundfarbe `var(--dsw-alias-label-secondary)` im CSS; der aktive Knopf
  (`[aria-pressed="true"]`) und der Hover-Zustand setzen
  `var(--dsw-alias-label-primary)` in eigenen Regeln. Bei
  `[aria-pressed="true"]` zeigt `::before` einen 2-px-Balken links, von
  `top: 6px` bis `bottom: 6px`, in `var(--board-akzent)`. Der Balken ist
  absolut gesetzt und verschiebt nichts.
- **Bewegung:** `.board-begleiter-auge` blinzelt mit
  `board-blinz 6s ease-in-out infinite`; die Augen sind bei 0/88/97/100 %
  offen und bei 91/94 % fast geschlossen.
- **Reduzierte Bewegung:** `@media (prefers-reduced-motion: reduce)`
  schaltet für `.board-seite *`, `*::before` und `*::after` Übergänge und
  Animationen mit `!important` ab.
- **Farben:** ausschließlich `var(--dsw-alias-label-primary)`,
  `var(--dsw-alias-label-secondary)` und `var(--board-akzent)`; keine
  Hex-, rgb- oder hsl-Werte im neuen Block und im ganzen Stilblock `S`.
  Keine neuen Token.
- **Maße:** `S.page`, `S.koerper`, `S.karte`, `S.schiene`, `S.dock` und
  `S.dockRechts` sind zeichengleich geblieben (Padding, Höhe, MinHeight,
  Radius, Breite, Abstände, Rahmen, Hintergrund, Schatten).

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
ist ebenfalls `aria-hidden` und nicht fokussierbar. Die Augen tragen ihre
Klasse im JSX und werden über die CSS-Regel bewegt; bei reduzierter
Bewegung steht das Blinzeln still. Der automatische Außenabstand nimmt nur
freien Platz und schiebt die beiden Schienenknöpfe nicht.

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
ℹ duration_ms 5694.44222
```

`test/nach85b.mjs` prüft: die neuen Regeln in `AKZENT_STIL`; den Scope
jeder statischen Regel; die drei CSS-Farbregeln des Schienenknopfs und das
Fehlen von `color` in seinen beiden Inline-Stilen; die drei Klassen an
Wurzel, Karte und Schienenknopf; den Begleiter samt `aria-hidden`,
`pointer-events: none`, Textfreiheit, 18 × 18, beiden Augenklassen und den
drei Mündern; das Fehlen von `outline: none` und harten Farben im neuen
Block und in `S`; die unveränderten Maße von sechs Stilblöcken; und die
Schlüsselzahl 614 (605 im Client). Die Datei selbst meldet:

```
ok Feinschliff: Karten mit Hover und Fokusring, Schienenfarbe im CSS mit
eigenem Hover und aktivem Balken, blinzelnder Begleiter mit drei Mündern,
alle Regeln unter .board-seite, reduzierte Bewegung abgeschaltet, Maße
inklusive Docks unverändert, 614 Schlüssel
```

## Abweichungen

Keine. Die Vorgaben aus `85b.md` und die Nachbesserung aus der Prüfung sind
umgesetzt; es gibt keine Änderung an Abständen, Schriftgrößen, Breiten,
Höhen, Positionen oder Reihenfolge, keine neuen Texte und keine neuen
Farben.

## Offene Punkte

- **Sichtprüfung:** ob `brightness(1.04)` im hellen und dunklen Modus
  sichtbar, aber ruhig wirkt, ob der 2-px-Balken neben der getönten Fläche
  des aktiven Knopfs gut sitzt, ob der Hover-Farbwechsel der inaktiven
  Knöpfe deutlich genug ist und ob der Begleiter unten in der Schiene nicht
  gedrängt wirkt — das entscheidet der Blick im Browser in Schritt 86.
  Ohne Browser ist hier nur der Quelltext belegt.
- **Kartenfokus:** Die Regel `.board-karte:focus-visible` steht wie
  vorgegeben; die Kartenhülle selbst ist nicht fokussierbar, der sichtbare
  Ring erscheint am Namensknopf über die allgemeine Fokusregel. Das ist
  keine Abweichung, aber ein Punkt für die Sichtprüfung.
- **Kurze Fenster:** Bei sehr geringer Höhe könnte der Begleiter den beiden
  Schienenknöpfen Platz nehmen; die Schiene hat keine eigene Scrollfläche.
  Bei üblicher Fensterhöhe bleibt genug Platz.
- Die Abschaltung der Bewegung ist als CSS-Text geprüft, nicht im Browser.

## Dateien

- `client.js` — `sha256 3492161ef840907766bc8c025eb601fe1cca063f7a53a5d5a8eae08a242df7ab`
- `test/nach85b.mjs` — `sha256 691e61ceb0cab182baef24e4643e2887639103371e7872bb40b3f34df2274e0e`
