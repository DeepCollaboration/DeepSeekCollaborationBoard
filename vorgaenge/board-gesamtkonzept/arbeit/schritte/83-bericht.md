# Schritt 83 — Bericht

## Nachbesserung nach der Prüfung

Die fünf Punkte aus `arbeit/pruefung.md` sind eingearbeitet:

1. **Karte und Dock getrennt.** `stationsInhalt(p, imDock)` baut immer den
   Basisinhalt und gibt bei `if (imDock !== true) return inhalt;` früh zurück
   (`client.js:7250`). Nur das Dock ruft `stationsInhalt(offeneStelle, true)`
   (`8865`); die Karte ruft `stationsInhalt(p)` (`7886`) und bekommt den
   Basisinhalt. `stationAnzeige()`, `fuss` und `stationFormular(false)` stehen
   nur noch hinter dieser Schwelle (`7298`). Die Karte hat keinen festen
   Dock-Inhalt mehr: ihr Körper ist wieder `S.karteKoerper` mit
   `cursor: 'default'` (`7856–7860`), der Auswahlring bleibt
   (`boxShadow: var(--dsw-elevation-prominent)`, `7735`). Beim Bearbeiten
   entsteht `stationFormular(false)` genau einmal (`test/nach83` prüft die
   eine Fundstelle); die Feld-IDs `station-name` und `station-beschreibung`
   liegen damit nur im Dock.
2. **Dock als Geschwister, ein Seitenpanel.** `stationDock` steht im Körper
   nach dem `raum` (`10390`), nicht darin. `S.dock` und `S.dockRechts` sind
   auf `min(360px, calc(100vw - 124px))` beziehungsweise
   `min(380px, calc(100vw - 124px))` begrenzt (`580–586`, `591–597`);
   124 = 2 × 26 Seitenrand + 72 Schiene. `schrittWaehlen` schließt die linke
   Leiste (`5669–5677`); der Schienenknopf schließt die Station beim Öffnen
   (`8626–8631`), `katalogZeigen` ebenso (`4950–4960`). Damit ist immer nur
   ein Seitenpanel offen. Der Chat bleibt unberührt.
3. **Fokus beim Sprung.** `springeZuStation` fokussiert nach dem Rendern
   `#board-station` (`7366–7387`); beim ersten Aufbau erledigt das `Panel`
   denselben Schritt. `test/nach64b.mjs` prüft das wieder positiv.
4. **`Bearbeiten` nur mit Karte.** Die Bedingung lautet jetzt
   `offeneStelle && !stationEdit && !gewaehlt.fremd && schreibbar`
   (`8824`). Eine Bibliotheksstation ohne Fläche bekommt keinen wirkungslosen
   Knopf.
5. **Tests nachgezogen.** `test/nach83.mjs` prüft die Trennung über
   `imDock`, den frühen Rückfall, die beiden Dockbreiten, die
   Ein-Seitenpanel-Regel, die `Bearbeiten`-Bedingung und den Fokus auf
   `#board-station`. `test/nach64b.mjs`, `test/nach81.mjs` und
   `test/nach64.mjs` sind an die neuen Breiten beziehungsweise den positiven
   Fokus angepasst.

## Gebaut

**Rechtes Dock.** `S.dockRechts` (`client.js:591–597`) ist 380 px breit
(bei schmalem Fenster `100vw - 124px`), eine Spalte, volle Höhe über den
Flex-Körper, mit `minHeight: 0`, Rand links `var(--dsw-alias-border-l1)` und
Hintergrund `var(--dsw-alias-bg-layer-1)`. Es hat kein `position`, kein
`margin` und keinen Radius. Der Inhalt rollt in `S.bibInhalt`; der Kopf
bleibt stehen. `S.dockTitel` (`599–603`) und `S.fassung` (`605–609`) sind
neu: 14 px für den Namen, 12 px Schreibmaschinenschrift für die Fassung. Die
Fassung benutzt dieselbe Stelle auf der Karte und im Dock.

**Dock im Körper.** `stationDock` (`8819–8870`) ist ein `Panel` mit
`seiteStil: S.dockRechts`, `fokus: true` und
`onSchliessen: stationSchliessen`. Der Kopf zeigt den Namen und, wenn
vorhanden, die Fassung; rechts steht „Bearbeiten“ nur bei vorhandener Karte,
Schreibrecht und ohne fremden Schritt, danach das ✕ aus `Panel`. Ein fremder
Schritt trägt den Satz „… hier nur zu lesen“ als Untertitel
(`stationFremdHinweis`, `6871–6878`). Das Panel steht im Körper nach dem
`raum` (`10310–10390`) und erscheint nur bei `auswahl.art === 'schritt'` und
vorhandenem `gewaehlt`.

**Ein Inhalt, zwei Orte.** `stationsInhalt(p, imDock)`
(`7085–7301`) enthält als Basis den gesamten Stationsinhalt: Verweisblock,
Vorhaben-Chips, Anlegen-Formular, Beiträge, Start/Schreiben/Skill, Dateien
und Übergabewarnungen. Die Karte ruft die Funktion an `7886` ohne `true`, das
Dock an `8865` mit `true`. Nur dann kommen `stationAnzeige()`, `fuss` und
beim Ändern `stationFormular(false)` dazu (`7250`, `7298`). Der alte zweite
Bau (`const inhalt = [` im `karten.map`) bleibt entfernt; der Inhalt entsteht
nur an einer Stelle.

**Karte bleibt klein.** `KARTE_GROSS` und `KARTE_GROSS_HOEHE` sind fort.
`breiteFuer` (`7069`) gibt für jede Karte `KARTEN_BREITE` zurück;
`flaechenHoehe` (`7329–7331`) rechnet für alle Karten mit
`KARTEN_HOEHE + 60`; `allesZeigen` (`7342–7360`) benutzt dieselben kleinen
Maße ohne Sonderfall für eine Einzelkarte. Die offene Karte behält
`minHeight: KARTEN_HOEHE` (`7736`), bekommt aber kein Dock-Maß und keinen
Rollkörper mehr.

**Station ohne Karte.** Eine Station mit `stellen === 0` öffnet sich im
selben Dock. Über dem Formular beziehungsweise der Nur-Lese-Anzeige steht der
vorhandene Hinweis „Noch nicht auf der Fläche — zieh sie dorthin, dann öffnet
sie sich dort.“ (`8867–8869`). Das alte `schrittPanel` und die Stile
`S.detail`, `S.detailKopf`, `S.detailTitle` sind gelöscht; `S.thStift` ist
mit dem alten Karten-✕ ebenfalls fort.

**Fokus und Tastatur.** `stationSchliessen` (`5581–5592`) leert die Auswahl
und setzt den Fokus auf den Namen der zugehörigen Karte
(`[data-karten-name]`). `Panel` holt den Fokus beim Öffnen herein und
behandelt Escape. Die Karte behält ihren eigenen Escape-Handler und ihre
Tastatur (Enter/Leertaste öffnen, der Name toggelt). Der Sprung aus der Suche
(`springeZuStation`, `7366–7387`) fokussiert `#board-station`; beim
Schließen kehrt der Fokus auf den Namen der Karte zurück.

## Bestandsaufnahme und neue Zeilen

| Stelle | vorher | nachher |
|---|---:|---:|
| `S.detail`, `S.detailKopf`, `S.detailTitle` | 817–826 | gelöscht |
| `S.thStift` | 499–504 | gelöscht (unbenutzt) |
| `S.dock` / `S.dockRechts` | 576 / — | 580 / 591 |
| `S.dockTitel` / `S.fassung` | — | 599 / 605 |
| `gewaehlt` / `stationSchliessen` | 5560 / — | 5566 / 5581 |
| `schrittWaehlen` | 5641 | 5669 |
| `katalogZeigen` | 4948 | 4950 |
| `stationFremdHinweis` | — | 6871 |
| `KARTE_GROSS` / `KARTE_GROSS_HOEHE` | 7037 / 7039 | gelöscht |
| `breiteFuer` / `flaechenHoehe` / `allesZeigen` | 7045 / 7073 / 7087 | 7069 / 7329 / 7342 |
| `stationsInhalt(p, imDock)` | — | 7085–7301 |
| `karten.map` / Kartenkörper | 7464 / 7849 | 7718 / 7856 |
| `schrittPanel` | 8796 | gelöscht |
| `stationDock` | — | 8819–8870 |
| Schienenknopf schließt Station | — | 8626–8631 |
| Stationsdock im Körper | — | 10390 |

## Tests

- **`test/nach83.mjs`** (neu, mit Nachbesserung): prüft die sechs Punkte der
  Schritt-Prüfung und die fünf Nachbesserungen — `S.dock`/`S.dockRechts` mit
  `min(…, calc(100vw - 124px))` und ohne `position`/`margin`/Radius; das Dock
  nur bei `auswahl.art === 'schritt'` und nach dem `raum`;
  `stationsInhalt(p, imDock)` als einzigen Bau für Karte und Dock, die Karte
  ohne `true`, das Dock mit `true`, den frühen Rückfall vor der Anzeige und
  genau eine `stationFormular(false)`-Stelle; die Ein-Seitenpanel-Regel über
  `schrittWaehlen`, Schienenknopf und `katalogZeigen`; die
  `Bearbeiten`-Bedingung mit `offeneStelle`; das Fehlen von `KARTE_GROSS` und
  die kleinen Maße in Fläche und „Alles zeigen“; den Hinweis über dem
  Formular; Escape/Fokus über `Panel`, `stationSchliessen`,
  `[data-karten-name]` und `#board-station`. Am Ende: 605 Schlüssel, also
  kein neuer sichtbarer Text.
- **`test/nach64b.mjs`** (angepasst): Abschnitt 3 verlangt wieder positiv,
  dass `springeZuStation` `#board-station` fokussiert; die Dimensionsprüfung
  in „Alles zeigen“ fragt `KARTEN_HOEHE` statt `KARTE_GROSS` und hält fest,
  dass die große Einzelkarte fort ist.
- **`test/nach81.mjs`** (angepasst): `S.dock` trägt die neue
  `min(360px, calc(100vw - 124px))`-Breite; der Schienenknopf prüft den
  Toggle über `oeffnen` und das Schließen der Station.
- **`test/nach64.mjs`** (angepasst): `S.dock` trägt dieselbe begrenzte
  Breite.
- **`test/nach65.mjs`** bleibt unverändert grün: die Breitenstichprobe sieht
  keine feste Breite über 360 px ohne Maximum mehr, weil die Docks jetzt
  `min(…)` tragen.
- **Unverändert grün:** `test/nach61.mjs` (Panel-Muster, Karte mit
  `aria-label` und Escape), `test/nach82.mjs` (Kopfzeile und 605 Schlüssel),
  `test/nach66.mjs` (26 Ausnahmen) und die übrigen Dateien.

## Testergebnis

`cd /workspace/projekte/board-plugin && node --check client.js && node --test test/*.mjs` → Exit 0, `ℹ tests 79`, `ℹ pass 79`, `ℹ fail 0`, `ℹ duration_ms 6384.752258` (79 Dateien: 78 vorhandene + `nach83`; `test/schritt30.mjs` war im ersten Lauf grün, keine Wiederholung nötig).

## Gegenproben

Alle Proben liefen in Kopien unter `/tmp/pruef83-fix-*`; das Produkt blieb
unberührt. Basis war grün.

| Probe | Änderung in der Kopie | Ergebnis |
|---|---|---|
| a | Karte ruft `stationsInhalt(p, true)` | `nach83` rot: „Die Karte auf der Fläche baut ihren Inhalt nicht über `stationsInhalt`“ |
| b | `if (imDock !== true) return inhalt;` entfernt | `nach83` rot: „`stationsInhalt` baut den Dock-Inhalt auch ohne `imDock === true`“ |
| c | `setLeiste(null)` in `schrittWaehlen` entfernt | `nach83` rot: „Das Öffnen einer Station schließt die linke Leiste nicht“ |
| d | `setAuswahl(null)` im Schienenknopf entfernt | `nach83` rot: „Das Öffnen der linken Leiste schließt die offene Station nicht“ |
| e | Fokus auf `#board-station` im Sprung entfernt | `nach83` und `nach64b` rot: „Der Sprung setzt den Fokus nicht in das Stationsdock“ |
| f | `S.dockRechts` wieder `width: '380px'` | `nach83` rot: „Das rechte Dock ist nicht 380 px breit und bei schmalem Fenster begrenzt“ |
| g | `offeneStelle &&` aus der `Bearbeiten`-Bedingung entfernt | `nach83` rot: „‚Bearbeiten‘ erscheint auch ohne Station auf der Fläche oder beim Bearbeiten“ |

## Abweichungen von der Aufgabe

1. **Der Kopf (Name/Fassung) liegt nicht in `stationsInhalt(p, imDock)`.**
   Die Funktion baut den Inhalt unter dem Kopf; der Kartenkopf bleibt Teil
   der Hülle (dort sitzen Ports und Ziehgriff), der Dockkopf kommt aus
   `Panel`. Beide benutzen dieselbe Fassungsdarstellung `S.fassung` und
   dieselbe Namensquelle. Ein gemeinsamer Kopf hätte entweder `Panel` (Fokus,
   ✕, Escape) oder den Ziehgriff der Karte ein zweites Mal gebaut.
2. **Eine Station aus der Bibliothek zeigt den Hinweis „Noch nicht auf der
   Fläche …“ jetzt auch.** Vorher zeigte das alte `schrittPanel` bei
   `fremd` nur die Nur-Lese-Anzeige. Schritt 5 verlangt den Hinweis für eine
   Station, die noch nicht auf der Fläche steht; er steht jetzt über der
   Anzeige. Das Formular bleibt fremden Stationen verwehrt.
3. **Die Karte zeigt den Basisinhalt, nicht nur den Auswahlring.** Der
   Prüfer hat die Trennung `stationsInhalt(p)`/`stationsInhalt(p, true)`
   verlangt; der Basisinhalt ist derselbe wie an einer geschlossenen Karte,
   der Auswahlring bleibt. Die Karte ist damit nicht leer, aber sie doppelt
   den Dock-Inhalt nicht mehr.

## Offene Punkte

1. **Sichtprüfung Schritt 86.** Ob der Basisinhalt in der kleinen Karte neben
   dem Dock ruhig wirkt und ob der Auswahlring deutlich genug ist, ist eine
   Sichtfrage.
2. **Schmale Fenster mit Chat.** Ein Seitenpanel ist jetzt begrenzt und nie
   zwei gleichzeitig; der Chat behält vorerst seine Spalte. Bei 360 px und
   offenem Chat kann die Summe weiterhin eng werden; Schritt 84 zieht den
   Chat um. Sichtprüfung in Schritt 86.
3. **Fokus nach der Suche.** Der Sprung fokussiert jetzt `#board-station`,
   auch wenn die Station schon offen war. Wenn er dort den Kartennamen
   bevorzugt, ist das eine kleine Änderung an `springeZuStation`; der Test
   hält die aktuelle Absicht positiv fest.
4. **Keine Commits, keine Installation.** `index.js`, `texte/*.json`,
   `locale/*`, `package.json`, die Einstellungen und die übrigen
   Arbeitsdokumente sind unberührt; geschrieben wurden `client.js`, die vier
   Tests `nach83`, `nach64b`, `nach81`, `nach64` und dieser Bericht.
