# Schritt 82 — Bericht

## Gebaut

**Kopfzeile in drei Gruppen.** Statt `S.kopf` mit zwei Spalten (`S.kopfText`
links, Menschen rechts) gibt es jetzt `S.kopfZeile`: eine Zeile, 44 px hoch,
`alignItems: center`, `gap: 12px`, unten eine Trennlinie. Die drei Gruppen
stehen in dieser Reihenfolge:

- **links** `S.kopfLinks` mit der unveränderten Boardwahl (Akzentpunkt, Name,
  „Nur lesen“-Chip, `▾`), der Herkunft, dem Wiederholen-Hinweis fürs
  Hauptboard und der Board-Liste im `Panel`. Die Liste bleibt an derselben
  Stelle im Baum und öffnet sich wie bisher unter der Wahl.
- **Mitte** `S.kopfMitte` mit dem Titel in `S.kopfTitel` (18 px, eine Zeile,
  Auslassung) und dem Dateien-Knopf am Gruppenende (`marginInlineStart:
  auto`), nur wenn ein Vorhaben gewählt ist.
- **rechts** `S.kopfRechts` mit der Chip-Reihe der Menschen
  (`S.mitglieder`, ohne die Überschrift „Am Board“; der Gruppenname steht als
  `aria-label` an einem `role="group"`), danach das Suchfeld „Station finden“
  und „Alles zeigen“.

Zwischen den Gruppen stehen zwei `S.trenner` (1 px breit, 20 px hoch,
`var(--dsw-alias-border-l1)`); die Gruppen selbst haben keinen Kasten und
keinen Hintergrund mehr.

**Statuszeile.** Unter der Kopfzeile liegt `S.statusZeile` (Spalte, 2 px
Abstand, 6 px Abstand nach oben). Sie erscheint nur, wenn es etwas zu sagen
gibt, und sammelt in dieser Folge: die Meldung `status` über `Meldung`, die
Zeile „Nur lesen: {0}“ mit `darfHinweis`, `standZeile(data.stand)`, je eine
Zeile aus `data.staende`, `formatWarnung` und die `sichtbarkeitsLuecken`. Die
alte Stelle der Meldung zwischen Titel und Fläche ist damit leer; die
Warnungen stehen nicht mehr im Kopftext.

**Lange Anleitung.** Der Satz „Klick auf ein Vorhaben zeigt …“ steht jetzt in
einem eigenen bedingten Zweig `karten.length === 0` unter der Statuszeile. Bei
einer Karte fehlt er; der Text und sein Schlüssel sind unverändert.

**Nur lesen.** Die Zeile „Nur lesen: {Grund}“ und der Weg (`darfHinweis`)
stehen nur noch in der Statuszeile. Der Grund bleibt im `title` des Chips in
der Boardwahl. Der Lesehinweis „Du liest mit“ mit Token- und
Eigenes-Board-Knopf (`ersteSitzungHinweis`) ist unverändert und steht weiter
zwischen Kopf und Körper.

**Fläche und Personenflächen.** Das Brett liegt jetzt in `S.eigeneFlaeche`:
Karte mit `var(--dsw-alias-bg-layer-1)`, 12 px Radius, Rand
`var(--dsw-alias-border-l1)`, Padding 10/12/14. Darin stehen der bisherige
Kopf (`eigenerKopf`) und das Brett; das Brett selbst trägt
`background: var(--dsw-alias-bg-layer-2)`, 8 px Radius und behält sein
Punktraster. `S.fremdFlaeche` wechselt vom `color-mix`-Ton auf
`var(--dsw-alias-bg-layer-1)`; Rand, 12 px Radius und der Kopf `fremdKopf`
bleiben. `bg-layer-0` gibt es im Dashboard nicht; `bg-layer-2` ist der
nächste vorhandene Schichtton aus dem Stilblock.

**Aufgeräumt.** `S.kopf`, `S.kopfText`, `S.mitgliederTitel`,
`S.mitgliederReihe` und `S.leiste` sind fort, ebenso die Variable
`leistenReiter` und ihr Einbau über der Fläche. Neu sind `S.kopfZeile`,
`S.kopfLinks`, `S.kopfMitte`, `S.kopfRechts`, `S.kopfTitel`, `S.trenner`,
`S.statusZeile` und `S.eigeneFlaeche`; `S.mitglieder` ist jetzt die Reihe
selbst. Kein Zustand, keine Funktion und kein sichtbarer Text ist verloren:
Boardwechsel, Board-Liste, Person bearbeiten, Hauptboard wiederholen,
Warnungen, Suche und „Alles zeigen“ bleiben erreichbar.

## Bestandsaufnahme und neue Zeilen

| Stelle | vorher | nachher |
|---|---:|---:|
| Stilblock `S` | 177 | 177 |
| `S.kopfFest`, `S.kopf`, `S.kopfText`, `S.mitglieder` | 184–188 | 184, 188–191, 223 |
| `S.mitgliederTitel` / `S.mitgliederReihe` | 189 / 193 | gelöscht |
| `S.kopfLinks` / `S.kopfMitte` / `S.kopfRechts` | — | 193 / 197 / 201 |
| `S.kopfTitel` / `S.trenner` / `S.statusZeile` | — | 206 / 211 / 217 |
| `S.eigeneFlaeche` / `S.fremdFlaeche` | — / 212 | 242 / 252 |
| `S.leiste` | 478 | gelöscht |
| Brett `const flaeche = h('div', {` | 7371 | 7404 |
| `stationsFeld` / „Alles zeigen“ | 8624 / 8687 | 8660 / 8723 (Inhalt unverändert) |
| `leistenReiter` (Variable und Einbau) | 8697 / 10169 | gelöscht |
| `eigene` / `eigenerKopf` | 8769 / 8771 | 8803 / 8805 |
| Statuszeile `const statusZeile =` | — | 9840–9881 |
| Kopfzeile im Seitenbau | 9807–10117 | 9888–10173 |
| „Nur lesen: {0}“ | 10041 | 9853 |
| Eigene Karte im Seitenbau | — | 10268 |
| `personEdit` / `ersteSitzungHinweis` / `S.koerper` | 10118 / 10170 / 10174 | 10173 / 10224 / 10228 |

## Tests

- **`test/nach82.mjs`** (neu, 235 Zeilen): prüft die sechs Punkte der
  Schritt-Prüfung — die Stile `S.kopfZeile`/`S.trenner`/`S.statusZeile` samt
  Maßen und Verwendung; die drei Gruppen der Kopfzeile in ihrer Reihenfolge
  (Boardwahl, Titel mit Dateien-Knopf, Menschen, Suche, „Alles zeigen“) und
  das Fehlen der alten Kopfzeilenform; die lange Anleitung an
  `karten.length === 0`; „Nur lesen“ in der Statuszeile und den Grund im
  `title` des Chips; Brett-Hintergrund sowie Rand, Radius und Hintergrund der
  eigenen und fremden Personenflächen; schließlich 26 Ausnahmen in
  `test/nach66.mjs` und 603 Schlüssel in allen sieben Wörterbüchern.
- **`test/nach65.mjs`**: Abschnitt 2 hielt die alte `S.leiste` fest
  (`flexWrap: 'wrap'`, 8 px). Diese Leiste ist fort; die Prüfung schaut jetzt
  auf die neue Kopfzeile: die Mitte schrumpft mit Auslassung, die Chip-Reihe
  kürzt sich, die Statuszeile stapelt und bricht um, das Suchfeld bleibt
  140 px fest. Panel, Lesehinweis, Chat und die Breitenstichprobe sind
  unverändert. Die Absicht — schmale Fenster brechen die Fläche nicht — bleibt
  geprüft, reicht aber nicht mehr so weit wie vorher (siehe Flanken).
- **`test/nach81.mjs`**: Abschnitt 4 suchte die Variable `leistenReiter`, um
  „Suchfeld und ‚Alles zeigen‘ bleiben“ zu prüfen. Sie prüft jetzt denselben
  Punkt am gerenderten Bereich `S.kopfZeile` bis `S.koerper` und hält weiter
  fest, dass Bibliothek und Verlauf keine Reiter der oberen Leiste sind.
- **Unverändert grün:** `test/nach59.mjs` (keine toten Stile — die gelöschten
  Namen fallen dort auf), `test/nach66.mjs` (26 Ausnahmen, 603 Schlüssel),
  `test/markt44.mjs` (sucht die Zeichenkette `uebersetze('Nur lesen: {0}',
  [darfGrund` weiterhin), `test/nach64.mjs`/`test/nach64b.mjs` (Suche und
  „Alles zeigen“ bleiben im Baum, `allesZeigenKnopf` bleibt benutzt).

## Unberührt

`index.js`, `texte/*.json`, `locale/*`, Einstellungen, Bibliothek, Dock, Chat,
Leser, offene Station, Dateien, Personenbearbeitung, der Lesehinweis „Du
liest mit“ und der Körper aus Schritt 81 sind nicht angefasst. Es wurde nichts
committet.

## Testergebnis

```
cd /workspace/projekte/board-plugin && node --check client.js && node --test test/*.mjs
```

Exit-Code 0. **78 von 78 Prüfungen grün** (77 vorhandene + die neue
`test/nach82.mjs`). Die Flanke um `test/schritt30.mjs` trat nicht auf.

## Offene Flanken

1. **Schmale Fenster.** Die Kopfzeile ist laut Schritt eine Zeile mit 44 px.
   Bei sehr schmalem Fenster (grob unter 630 px) können Boardgruppe und
   Werkzeuge rechts deshalb seitlich überlaufen; der Titel schrumpft zuerst,
   die Chip-Reihe kürzt sich. Die alte, umbrechende Leiste war dafür robuster.
   Sichtprüfung in Schritt 86.
2. **Ablegen auf dem Brett.** Die Ablegerechnung nimmt den Scrollrahmen, nicht
   das Brett selbst. Durch das neue Kartenpolster verschiebt sich der Bezug
   um 12 px waagerecht (und etwa 10 px senkrecht zusätzlich); senkrecht war er
   schon vorher um den Kopf verschoben. Betrifft nur die Genauigkeit beim
   Ziehen einer Station, nicht die Erreichbarkeit.
3. **Hintergrund in heller Darstellung.** `bg-layer-1` und `bg-layer-2` sind
   im hellen Dashboard beide Weiß; die Fläche hebt sich dort über Rand,
   Radius und Punktraster ab, nicht über den Ton. Im dunklen Dashboard sind
   die Schichten verschieden.
4. **Sichtprüfung.** Die Prüfungen lesen `client.js` als Text. Ob die drei
   Gruppen, die 44 px und die Karten wirklich sitzen, zeigt erst der Browser
   in Schritt 86.
