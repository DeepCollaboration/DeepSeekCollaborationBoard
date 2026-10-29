# Schritt 83 — Bericht

## Gebaut

**Rechtes Dock.** `S.dockRechts` (`client.js:589–595`) ist 380 px breit,
eine Spalte, volle Höhe über den Flex-Körper, mit `minHeight: 0`, Rand links
`var(--dsw-alias-border-l1)` und Hintergrund `var(--dsw-alias-bg-layer-1)`.
Es hat kein `position`, kein `margin` und keinen Radius. Der Inhalt rollt in
`S.bibInhalt`; der Kopf bleibt stehen. `S.dockTitel` (`597–601`) und
`S.fassung` (`603–607`) sind neu: 14 px für den Namen, 12 px
Schreibmaschinenschrift für die Fassung. Die Fassung benutzt dieselbe Stelle
auf der Karte und im Dock.

**Dock im Körper.** `stationDock` (`8802–8855`) ist ein `Panel` mit
`seiteStil: S.dockRechts`, `fokus: true` und `onSchliessen: stationSchliessen`.
Der Kopf zeigt den Namen und, wenn vorhanden, die Fassung; rechts steht
„Bearbeiten“ nur bei Schreibrecht und ohne fremden Schritt, danach das ✕ aus
`Panel`. Ein fremder Schritt trägt den Satz „… hier nur zu lesen“ als
Untertitel (`stationFremdHinweis`, `6863–6870`). Das Panel steht im Körper
nach dem `raum` (`10293–10373`) und erscheint nur bei
`auswahl.art === 'schritt'` und vorhandenem `gewaehlt`.

**Ein Inhalt, zwei Orte.** `stationsInhalt(p)` (`7077–7292`) enthält den
gesamten Stationsinhalt: Verweisblock, Vorhaben-Chips, Anlegen-Formular,
Beiträge, Start/Schreiben/Skill, Dateien und Übergabewarnungen. Ist die
Station offen, kommen `stationAnzeige()` und `fuss` dazu, beim Ändern
`stationFormular(false)`. Die Karte ruft die Funktion an `7877`, das Dock an
`8848`. Der alte zweite Bau (`const inhalt = [` im `karten.map`) ist entfernt;
der Inhalt entsteht nur noch an einer Stelle.

**Karte bleibt klein.** `KARTE_GROSS` und `KARTE_GROSS_HOEHE` sind fort.
`breiteFuer` (`7069`) gibt für jede Karte `KARTEN_BREITE` zurück;
`flaechenHoehe` (`7320–7321`) rechnet für alle Karten mit `KARTEN_HOEHE + 60`;
`allesZeigen` (`7333–7351`) benutzt dieselben kleinen Maße ohne Sonderfall
für eine Einzelkarte. Die offene Karte bekommt `height: KARTEN_HOEHE`
(`7734`) und einen rollenden Körper (`7850–7877`) statt der bisherigen
760 px.

**Station ohne Karte.** Eine Station mit `stellen === 0` öffnet sich im
selben Dock. Über dem Formular beziehungsweise der Nur-Lese-Anzeige steht der
vorhandene Hinweis „Noch nicht auf der Fläche — zieh sie dorthin, dann öffnet
sie sich dort.“ (`8850–8852`). Das alte `schrittPanel` und die Stile
`S.detail`, `S.detailKopf`, `S.detailTitle` sind gelöscht; `S.thStift` ist
mit dem alten Karten-✕ ebenfalls fort.

**Fokus und Tastatur.** `stationSchliessen` (`5576–5587`) leert die Auswahl
und setzt den Fokus auf den Namen der zugehörigen Karte
(`[data-karten-name]`). `Panel` holt den Fokus beim Öffnen herein und
behandelt Escape. Die Karte behält ihren eigenen Escape-Handler und ihre
Tastatur (Enter/Leertaste öffnen, der Name toggelt). Der Sprung aus der Suche
(`springeZuStation`, `7356–7373`) setzt den Fokus nicht mehr selbst auf den
Kartennamen; das Dock übernimmt ihn, beim Schließen kehrt er auf den Namen
zurück.

## Bestandsaufnahme und neue Zeilen

| Stelle | vorher | nachher |
|---|---:|---:|
| `S.detail`, `S.detailKopf`, `S.detailTitle` | 817–826 | gelöscht |
| `S.thStift` | 499–504 | gelöscht (unbenutzt) |
| `S.dockRechts` / `S.dockTitel` / `S.fassung` | — | 589 / 597 / 603 |
| `gewaehlt` / `stationSchliessen` | 5560 / — | 5566 / 5576 |
| `schrittWaehlen` | 5641 | 5664 |
| `stationFremdHinweis` | — | 6863 |
| `KARTE_GROSS` / `KARTE_GROSS_HOEHE` | 7037 / 7039 | gelöscht |
| `breiteFuer` / `flaechenHoehe` / `allesZeigen` | 7045 / 7073 / 7087 | 7069 / 7320 / 7333 |
| `stationsInhalt(p)` | — | 7077–7292 |
| `karten.map` / Kartenkörper | 7464 / 7849 | 7704 / 7850 |
| `schrittPanel` | 8796 | gelöscht |
| `stationDock` | — | 8802–8855 |
| Stationsdock im Körper | — | 10373 |

## Tests

- **`test/nach83.mjs`** (neu, 186 Zeilen): prüft die sechs Punkte der
  Schritt-Prüfung — `S.dockRechts` mit Maßen und ohne `position`/`margin`/
  Radius; das Dock nur bei `auswahl.art === 'schritt'` und nach dem `raum`;
  `stationsInhalt` als einzigen Bau für Karte und Dock (genau eine Definition,
  genau zwei Aufrufe, ein `const inhalt`); das Fehlen von `KARTE_GROSS` und
  die kleinen Maße in Fläche und „Alles zeigen“; das Fehlen von
  `schrittPanel`/`S.detail` und den Hinweis über dem Formular im Dock;
  schließlich Escape/Fokus über `Panel`, `stationSchliessen` und
  `[data-karten-name]`. Am Ende: 605 Schlüssel, also kein neuer sichtbarer
  Text.
- **`test/nach64b.mjs`** (angepasst): Abschnitt 3 verlangt nicht mehr, dass
  `springeZuStation` den Kartennamen fokussiert — das Dock holt den Fokus
  jetzt (neue Absicht aus Schritt 83); die Dimensionsprüfung in „Alles
  zeigen“ fragt `KARTEN_HOEHE` statt `KARTE_GROSS` und hält fest, dass die
  große Einzelkarte fort ist. Kommentar und Schlusszeile sind mitgezogen.
- **`test/nach65.mjs`** bleibt unverändert grün: `S.dockRechts` trägt ein
  eigenes `maxWidth` (`calc(100vw - 16px)`), damit die Breitenstichprobe
  „keine feste Breite über 360 px ohne Maximum“ weiter gilt.
- **Unverändert grün:** `test/nach61.mjs` (Panel-Muster, Karte mit
  `aria-label` und Escape), `test/nach81.mjs` (Rahmen), `test/nach82.mjs`
  (Kopfzeile und 605 Schlüssel), `test/nach66.mjs` (26 Ausnahmen),
  `test/nach64.mjs` („Alles zeigen“) und die übrigen 69 Dateien.

## Testergebnis

`cd /workspace/projekte/board-plugin && node --check client.js && node --test test/*.mjs` → Exit 0, `ℹ tests 79`, `ℹ pass 79`, `ℹ fail 0`, `ℹ duration_ms 5736.707677` (79 Dateien: 78 vorhandene + `nach83`; `test/schritt30.mjs` war im ersten Lauf grün, keine Wiederholung nötig).

## Abweichungen von der Aufgabe

1. **Der Kopf (Name/Fassung) liegt nicht in `stationsInhalt(p)`.** Die
   Funktion baut den Inhalt unter dem Kopf; der Kartenkopf bleibt Teil der
   Hülle (dort sitzen Ports und Ziehgriff), der Dockkopf kommt aus `Panel`.
   Beide benutzen dieselbe Fassungsdarstellung `S.fassung` und dieselbe
   Namensquelle. Ein gemeinsamer Kopf hätte entweder `Panel` (Fokus, ✕,
   Escape) oder den Ziehgriff der Karte ein zweites Mal gebaut.
2. **`S.dockRechts` hat zusätzlich `maxWidth: 'calc(100vw - 16px)'`.** Das
   war nicht verlangt, hält aber die Prüfung aus Schritt 65 (keine feste
   Breite über 360 px ohne eigenes Maximum) und schmale Fenster im Blick.
3. **Eine Station aus der Bibliothek zeigt den Hinweis „Noch nicht auf der
   Fläche …“ jetzt auch.** Vorher zeigte das alte `schrittPanel` bei
   `fremd` nur die Nur-Lese-Anzeige. Schritt 5 verlangt den Hinweis für eine
   Station, die noch nicht auf der Fläche steht; er steht jetzt über der
   Anzeige. Das Formular bleibt fremden Stationen verwehrt.
4. **Der Kartenkörper ist nicht leer, wenn die Station offen ist.** Karte und
   Dock teilen denselben Inhalt; die Karte hält ihn in 240 px mit Rollen,
   statt ihn auszublenden. Das folgt dem Satz „Karte und Dock teilen denselben
   Inhalt“; „zeigt nur noch, dass sie offen ist“ ist damit als „wächst nicht
   mehr“ umgesetzt, nicht als leerer Kartenkörper.

## Offene Punkte

1. **Sichtprüfung Schritt 86.** Ob der gemeinsame Inhalt in der kleinen Karte
   neben dem Dock doppelt wirkt, ist eine Geschmacksfrage. Wenn er den
   Kartenkörper offen leer haben will, ist das eine kleine Änderung an
   `7877`; `stationsInhalt` bliebe für den Dock und die geschlossene Karte.
2. **Schmale Fenster.** Mit offener Bibliothek (360 px) und offenem
   Stationsdock (380 px) bleiben neben Schiene (72 px) und Fläche nur noch
   wenig Pixel. Die Docks haben eigene Maximalbreiten, aber die Summe kann
   bei sehr schmalem Fenster seitlich überlaufen. Sichtprüfung in Schritt 86.
3. **Fokus nach der Suche.** Der Sprung aus der Suche setzt den Fokus jetzt
   ins Dock statt auf den Kartennamen. Das ist die neue Absicht; wenn er den
   alten Landepunkt behalten will, muss `test/nach64b.mjs` zurückgedreht
   werden.
4. **Keine Commits, keine Installation.** `index.js`, `texte/*.json`,
   `locale/*`, `package.json`, die Einstellungen und die Arbeitsdokumente
   sind unberührt; geschrieben wurden nur `client.js`, `test/nach64b.mjs` und
   `test/nach83.mjs`.
