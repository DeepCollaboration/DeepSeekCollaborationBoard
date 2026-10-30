# Schritt 84 — Bericht

## Gebaut

**Ein rechtes Dock.** `rechtsDock` (`client.js:9878–9937`) ist das eine Panel
rechts. Es erscheint, wenn eine Station offen ist **oder** ein Gespräch läuft
(`rechtsAktiv`, `client.js:5633–5643`), sonst nicht. Die alte Chatspalte
`gespraechPanel` und ihr Breitengriff sind fort. Das Dock steht als Geschwister
nach dem `raum` im Körper (`10538–10567`), nicht mehr darin. `S.dockRechts`
bleibt die Form aus 83 (`min(380px, calc(100vw - 124px))`, `flex: none`,
`minHeight: 0`, Rand links, `bg-layer-1`, kein `position`/`margin`/Radius,
`client.js:591`); die Breite im Aufbau setzt `spaltenBreite`.

**Zwei Reiter, nur wenn beide da sind.** `stationImDock && gespraechImDock`
entscheidet über die Reiterleiste (`9878–9908`): zwei Knöpfe mit den
vorhandenen Wörtern „Station“ und „Gespräch“, `role: 'tab'`,
`aria-selected`, aktiver Reiter über `S.dockReiterAktiv` hervorgehoben. Ist nur
eines offen, ist `reiterLeiste` `null`; der Kopf zeigt direkt den einen
Inhalt. Gebaut wird nur der aktive Inhalt:
`const teile = rechtsAktiv === 'station' ? stationDockTeile() : gespraechDockTeile();`
(`9879`). `rechtsReiter` ist der Zustand des vorne stehenden Reiters
(`client.js:3243`), gemerkt wie die Auswahl (`3583`).

**Kopf und Schließen gehören dem aktiven Reiter.** `titel`, `untertitel`,
`titelStil`, `kopfRechts`, `kinder` und `fuss` kommen aus `teile`
(`9930–9935`). Die Schließen-Taste und Escape rufen `rechtsSchliessen`
(`5704–5707`): bei „Station“ `stationSchliessen`, bei „Gespräch“
`gespraechSchliessen`. `stationSchliessen` (`5672–5688`) leert nur die
Auswahl; bleibt das Gespräch, wechselt der Reiter auf „Gespräch“ und der Fokus
geht in dessen `section`; sonst kehrt er wie bisher auf
`[data-karten-name]` zurück. `gespraechSchliessen` (`5693–5701`) setzt
`gespraechZu` und `setGespraech(null)`; bleibt die Station, steht das Dock
weiter und der Fokus geht auf ihren Reiter. Die `Panel`-Hülle hat dafür einen
Platz über dem Kopf bekommen (`oben`, `client.js:2059`, `2132–2134`).

**Fokus und Tastatur.** `Panel` holt den Fokus beim Öffnen (`fokus: true`) und
behandelt Escape über `onSchliessen`. Zusätzlich öffnen die Auslöser ihren
Reiter und setzen den Fokus nach dem Rendern: `schrittWaehlen` (`5794–5807`),
`gespraechBeginnen` (`4383`, `4404`) und `werkstattStarten` (`5387`, `5402`).
`fokusRechts` (`5646–5655`) geht von `#board-station` beziehungsweise
`#board-chat` über `closest('section')` auf die Dock-fläche. Ein Reiterwechsel
fokussiert nichts neu; der Fokus bleibt auf dem geklickten Knopf. Der Sprung
aus der Suche (`springeZuStation`, `7496`) fokussiert weiter die
Dock-`section` über `#board-station` und `closest('section')`.

**Das Gespräch umgezogen, nicht nachgebaut.** `gespraechDockTeile`
(`9730–9872`) enthält einmal den bisherigen Chat-Inhalt: Einträge, Status,
Fehler, neue Skills, Eingabefeld mit `feldHoehe`, Griff, Höhenzustand und die
Handler `gespraechSenden`, `gespraechStoppen`, `gespraechVerwerfen`,
`skillEintragen`, `chatOeffnen`. Die Kennung `board-chat` sitzt jetzt am
Inhalt (`9762`), nicht mehr am Panel. `S.gespraech` (`999`) ist nur noch der
Körper: Spalte, `minHeight: 0`, keine eigene Breite, kein `flex`, kein Rand.
Der Chat-Breitengriff ist durch `S.dockBreiteGriff`/`S.dockBreiteStrich`
(`1005`, `1010`) für das ganze Dock ersetzt.

**Breite und Grenzen.** `spaltenBreite` startet bei 380 (`gemerkt`,
`client.js:1736`; Zustand `3238`, Untergrenze 300 beim Aufbau). Das Dock
übernimmt sie als `width: spaltenBreite + 'px'` und begrenzt sich mit
`maxWidth: 'calc(100vw - 124px)'` (`9922–9923`). Der Griff (`10538–10566`)
zieht zwischen 300 und
`Math.max(300, Math.min(window.innerWidth - 520, window.innerWidth - 124))`.
Ein zweiter Breitengriff existiert nicht.

**Ein Seitenpanel.** `schrittWaehlen` schließt die linke Leiste
(`setLeiste(null)`, `5798`). `gespraechBeginnen` und `werkstattStarten` tun
dasselbe (`4365`, `5366`). Umgekehrt schließt `leisteOeffnen`
(`5712–5719`) die Station und nimmt das Gespräch aus der Ansicht
(`setGespraech(null)`, `gespraechZu`); benutzt wird es im Schienenknopf
(`8765`) und in `katalogZeigen` (`5002`). Ein gewähltes Vorhaben bleibt dabei
gewählt. `gespraechVerwerfen` setzt den Fokus auf die Station, wenn sie offen
bleibt (`4477–4478`).

## Bestandsaufnahme und Zeilen

| Stelle | Zeilen |
|---|---:|
| `S.gespraech` (Körper ohne Breite/Rand) | 999 |
| `S.dockBreiteGriff` / `S.dockBreiteStrich` | 1005 / 1010 |
| `S.dockReiter` / `-Knopf` / `-Aktiv` | 1017 / 1021 / 1028 |
| `S.gespraechKopf` (Kopf beider Reiter) | 1036 |
| `Panel`-Platz `oben` | 2059, 2132–2134 |
| `gemerkt.rechtsReiter`, `spaltenBreite: 380` | 1721, 1736 |
| Zustand `spaltenBreite` / `rechtsReiter` | 3238 / 3243 |
| `gemerkt.rechtsReiter`-Wirkung | 3583 |
| `gespraechBeginnen` (Leiste, Reiter, Fokus) | 4359, 4365, 4383, 4404 |
| `gespraechVerwerfen` (Fokus auf Station) | 4477–4478 |
| `werkstattStarten` (Leiste, Reiter, Fokus) | 5361, 5366, 5387, 5402 |
| `stationImDock` / `gespraechImDock` / `rechtsAktiv` | 5633–5643 |
| `fokusRechts` / `nachRender` | 5646 / 5657 |
| `stationSchliessen` | 5672–5688 |
| `gespraechSchliessen` | 5693–5701 |
| `rechtsSchliessen` | 5704–5707 |
| `leisteOeffnen` | 5712–5719 |
| `schrittWaehlen` | 5794–5807 |
| `stationDockTeile` | 8954–8998 |
| `gespraechDockTeile` | 9730–9872 |
| `rechtsDock` | 9878–9937 |
| Griff / Dock im Körper | 10538 / 10567 |

## Tests

- **`test/nach84.mjs`** (neu): prüft die sieben Punkte der Schritt-Prüfung —
  ein rechtes Dock nach dem `raum` (mit Klammerzählung, nicht nur Textreihenfolge),
  das Fehlen von `gespraechPanel` und des Chat-Griffs, die Reiterleiste nur bei
  `beide`, zwei Knöpfe mit `aria-selected` und aktivem Stil, nur den aktiven
  Inhalt, Kopf/Schließen/Fokus über den aktiven Reiter, dieselben
  Chat-Zustände und -Handler samt einmaliger Fundstelle von `board-chat`,
  Escape/Reiterwechsel, die Dockbreite mit einem Griff sowie die
  Ein-Seitenpanel-Regel; am Ende 605 Schlüssel.
- **`test/nach61.mjs`** (angepasst): Die fünfte Fläche ist jetzt das rechte
  Dock (`id: 'board-gespraech'`); der Chat-Inhalt darunter trägt
  `board-chat`. Fokus, Schließer und `S.gespraechKopf` bleiben geprüft.
- **`test/nach65.mjs`** (angepasst): Die Breitenprüfung folgt `rechtsDock`:
  `spaltenBreite` als `width`, `maxWidth: 'calc(100vw - 124px)'`, kein
  Chat-Breitengriff.
- **`test/nach81.mjs`** (angepasst): Der Schienenknopf öffnet über
  `leisteOeffnen()`; geprüft wird, dass die Leiste nur eine Station schließt
  und das Gespräch aus der Ansicht nimmt.
- **`test/nach83.mjs`** (angepasst): `stationDockTeile` und `rechtsDock`
  ersetzen `stationDock`; das eine Panel schließt über `rechtsSchliessen`,
  die Position wird mit Klammerzählung gegen den `raum` geprüft, Leiste und
  Katalog öffnen über `leisteOeffnen`.
- **Unverändert grün:** die übrigen 75 Dateien, darunter `nach63`
  (Radien/Schriften), `nach66` (Ausnahmen), `nach82` (Kopfzeile) und
  `test/schritt30.mjs`.

## Testergebnis

`cd /workspace/projekte/board-plugin && node --check client.js` → Exit 0,
keine Ausgabe.

`node --test test/*.mjs` →

```
ℹ tests 80
ℹ suites 0
ℹ pass 80
ℹ fail 0
ℹ cancelled 0
ℹ skipped 0
ℹ todo 0
ℹ duration_ms 6380.141383
```

80 Dateien: die 79 vorhandenen plus `test/nach84.mjs`. `test/schritt30.mjs`
war im ersten Lauf grün; keine Wiederholung nötig.

## Gegenproben

Alle Proben liefen in Kopien unter `/tmp/pruef84-*`; das Produkt blieb
unberührt. Die Kopien sind wieder entfernt.

| Probe | Änderung in der Kopie | Ergebnis |
|---|---|---|
| a | `const reiterLeiste = beide ?` → `true ?` | `nach84` rot: „Die Reiterleiste erscheint auch bei nur einem offenen Reiter“ |
| b | `setGespraech(null)` in `stationSchliessen` | `nach84` rot: „Das Schließen der Station schließt auch das Gespräch“ |
| c | zweites `rechtsDock` in den Körper | `nach84` rot: „Im Körper stehen mehr als ein rechtes Dock (Griff und Dock erwartet)“ |
| d | `S.dockBreiteGriff` wieder `S.gespraechBreiteGriff` | `nach84` rot: „Der Breitengriff der alten Chatspalte steht noch im Client“ |

## Abweichungen von der Aufgabe

1. **Fokus beim Schließen eines von zwei Reitern.** Punkt 6 nennt den
   „Gesprächsreiter“ beziehungsweise „Stationsreiter“. Da die Reiterleiste
   bei nur einem offenen Reiter laut Punkt 2 verschwindet, gibt es diesen
   Knopf dann nicht mehr. Der Fokus geht deshalb in die `section` des
   verbleibenden Inhalts (`fokusRechts`), also in dieselbe Fläche, die der
   Reiter zeigte. Der Test hält diese Fassung fest.
2. **Der Tooltip des Breitengriffs bleibt der vorhandene Schlüssel**
   „Ziehen: Gesprächsspalte breiter oder schmaler“. Ein neuer Text wäre ein
   neuer Schlüssel; die Zahl muss bei 605 bleiben. Der Griff zieht jetzt das
   ganze Dock, der alte Wortlaut ist an dieser Stelle ungenau.
3. **`S.gespraech` hat gar keinen Rand mehr**, nicht nur keinen linken: Der
   Körper sitzt im gepolsterten Dock, dessen linke Kante die Trennung
   zeichnet; ein zweiter Rahmen wäre eine Doppelung.
4. **Ein Kopf für beide Reiter.** Die Station benutzte bisher `S.bibKopf`,
   das Gespräch `S.gespraechKopf`. Das gemeinsame Dock benutzt
   `S.gespraechKopf` für beide, damit der Kopf beim Reiterwechsel nicht
   springt.
5. **`rechtsReiter` wird gemerkt** wie die Auswahl und die Sitzung. Die
   Aufgabe verlangt nur den Zustand; ohne Merker stünde nach einem
   Chat-Wechsel wieder die Station vorne, obwohl das Gespräch das letzte war,
   das er ansah.

## Offene Punkte

1. **360 px.** Das Dock ist bei 360 px Fensterbreite über
   `calc(100vw - 124px)` auf 236 px begrenzt; die geforderte Untergrenze von
   300 px kann dort nicht gelten, weil sonst wieder etwas seitlich
   überliefe. Das Dock läuft nicht mehr über, aber die Fläche daneben wird
   sehr schmal. Ob das bei 360 px benutzbar ist, ist die Sichtprüfung in
   Schritt 86.
2. **Sichtprüfung Kopf und Reiter.** Ob der gemeinsame Kopf, die schmale
   Reiterleiste und der aktive Reiter bei 100 % und 200 % ruhig wirken, ist
   eine Sichtfrage (Schritt 86). Die Tests lesen nur den Quelltext.
3. **Reiter-Semantik für Vorleser.** Die Knöpfe tragen `role: 'tab'` und
   `aria-selected`; `aria-controls`/`role="tabpanel"` sind nicht gesetzt.
   Wenn die Vorleseprüfung in 86 das verlangt, ist das eine kleine Ergänzung
   an der Reiterleiste.
4. **Tooltip-Wortlaut.** Siehe Abweichung 2; eine Korrektur bräuchte einen
   neuen Schlüssel oder eine Änderung an `texte/*.json`, beides außerhalb
   dieses Schritts.
5. **Keine Commits, keine Installation.** Geschrieben wurden `client.js`,
   `test/nach84.mjs` (neu) und die vier angepassten Tests `nach61`,
   `nach65`, `nach81`, `nach83`; `index.js`, `rechte.js`, `texte/*.json`,
   `locale/*`, `package.json`, die Einstellungen und die übrigen
   Arbeitsdokumente sind unberührt.
