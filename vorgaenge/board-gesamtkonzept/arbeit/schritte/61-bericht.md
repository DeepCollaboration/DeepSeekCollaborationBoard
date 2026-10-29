# Schritt 61 — Bericht

## Gebaut

Fünf Flächen teilen jetzt Hülle, Kopf, Schließen und Tastatur aus `Panel`.
Ihre Inhalte sind unverändert; die vorhandenen Stile gehen als Eigenschaften
hinein:

- **Bibliothek** — `id: 'board-bibliothek'`, `seiteStil: S.leistenFlaeche`,
  `kopfStil: S.bibKopf`, `titelStil: S.bibAbschnittTitel`,
  `schliessenStil: S.schliessen`, `fokus: true`, Schließer
  `() => setLeiste(null)`. Der Umschalter Bibliothek/Katalog steht als erste
  Zeile im Inhalt.
- **Katalog** — `id: 'board-katalog'`, `kopfStil: S.bibKopf`,
  `titelStil: S.bibAbschnittTitel`, `schliessenStil: S.schliessen`,
  `fokus: true`, Schließer `() => setBibAnsicht('bibliothek')`. Er liegt als
  eigenes Panel **im** Bibliothek-Panel; damit schließt Escape nur ihn, und der
  Fokus kehrt zu dem Umschalter zurück, der ihn geöffnet hat.
- **Leser** — `id: 'board-leser'`, `seiteStil` aus `S.leser` plus
  `S.leserRechts`/`S.leserUnten`, `kopfStil` aus `S.leserKopf` mit `cursor` und
  `touchAction`, `titelStil: S.leserTitel`, `schliessenStil: S.klein`,
  `fokus: true`, Schließer `() => setLeser(null)`. Die vorhandenen
  Kopfknöpfe stehen als `kopfRechts`, das Ziehen am Kopf als
  `kopfEigenschaften`; der Inhalt bleibt `S.leserInhalt`.
- **Chat** — `id: 'board-chat'`, `seiteStil` aus `S.gespraech` mit Breite und
  `maxWidth`, `kopfStil: S.gespraechKopf`, `schliessenStil: S.klein`,
  `fokus: true`, Schließer `setGespraechZu(…)` + `setGespraech(null)`. Titel
  und Ort stehen als Knoten im Kopf (mit `S.gespraechTitel` und `S.meta`), die
  Eingabe als `fuss` (`S.gespraechFuss`).
- **Board-Liste** — `id: 'board-liste'`, `seiteStil: S.boardPanel`,
  `kopfStil: S.boardKopf`, `titelStil: S.boardAnzeigeTitel`,
  `schliessenStil: S.schliessen`, `fokus: true`, Schließer
  `() => setBoardOffen(false)`.

`Panel` kennt zusätzlich `titelStil`, `schliessenStil` und `fuss`. Für die
vorhandenen Köpfe kamen `kopfRechts` (die Handlungen von Leser und Chat),
`kopfEigenschaften` (das Ziehen des Lesers) und `schliessenTitel` (der lange
Titel des Chat-Schließens) hinzu. Escape schließt jetzt jedes Panel mit
`onSchliessen`, nicht nur `rolle: 'dialog'`; `stopPropagation` bleibt, damit
ein inneres Panel nicht das äußere schließt. Ein Feld, das Escape selbst
behandelt, bleibt zuerst am Zug (`defaultPrevented`) — so schließt Escape in
einem Bibliotheksformular nicht die ganze Bibliothek.

## Fokus

`Panel` merkt sich beim Erscheinen `document.activeElement`, setzt den Fokus
auf die Fläche selbst (`tabIndex: -1`) und gibt ihn beim Verschwinden zurück,
wenn das Element noch im Dokument hängt. Alle fünf Flächen bekommen deshalb
`fokus: true` und ihren bisherigen Schließer. Verschachtelt geprüft ist der
Katalog: Er öffnet sich aus dem Bibliothek-Panel, schließt per Escape oder ✕
nur sich selbst und gibt den Fokus an den Umschalter zurück; danach schließt
Escape die Bibliothek.

## Geöffnete Karte

Die Karte ist keine Dialogfläche. Sie trägt `aria-label` mit dem Stationsnamen
und `role="group"`; der Name ist mit `data-karten-name` gezeichnet. Ist die
Karte offen, macht Escape sie klein: Der Handler auf der Karte prüft
`ev.defaultPrevented`, stoppt das Ereignis, setzt den Fokus auf den Namen und
ruft `setAuswahl(null)`. Enter und Leertaste öffnen weiterhin über den
Namensknopf und die bisherige `role="button"`-Fläche.

## Schließen

`uebersetze('Schließen')` steht genau einmal im Client — in `Panel`. Alle
anderen Schließen-Knöpfe sind `Knopf` mit `symbol: 'Schließen'`; die beiden
Textknöpfe (Stationsformular, Veröffentlichungsbefehle) bekommen den sichtbaren
Text aus demselben `symbol`, weil `Knopf` ohne Kinder das Symbol anzeigt. Damit
baut keine Fläche ihren Schließen-Knopf mehr von Hand, und die Wörterbücher
bleiben unberührt.

## Aussehen

Die vorhandenen Stile gehen als `seiteStil`, `kopfStil`, `titelStil` und
`schliessenStil` in `Panel`; für den Chat kommen `S.gespraechTitel` und
`S.meta` als Titel-Knoten hinzu. Zwei vorhandene Stile bekamen ein neutrales
Gewicht, damit das `<h2>` nicht fett macht, was vorher in einem `<div>` normal
war: `S.boardAnzeigeTitel` und `S.meta` je `fontWeight: 400`. Es gibt keine
neuen Stil-Schlüssel, keine neuen Texte, keinen neuen Zustand, keine Route und
keinen Merker.

## Geänderte Dateien

- `client.js` — `Panel` erweitert; Bibliothek, Katalog, Leser, Chat und
  Board-Liste auf `Panel`; Karte mit `aria-label` und Escape-Fokus; die
  übrigen Schließen-Knöpfe als `Knopf`.
- `test/nach61.mjs` — neu.

`index.js`, `texte/*.json` und die übrigen Prüfungen sind unberührt.

## Prüfdatei

`test/nach61.mjs` liest `client.js` als Text und prüft:

1. mindestens fünf `Panel`-Aufrufe mit `fokus` und `onSchliessen`;
2. die Escape-Behandlung in `Panel` hängt nicht mehr an `dialog`, schließt und
   stoppt das Ereignis;
3. `uebersetze('Schließen')` steht genau einmal, im `Panel`; keine der fünf
   Flächen baut ihren Schließen-Knopf von Hand;
4. Bibliothek, Katalog, Leser, Chat und Board-Liste kommen je einmal als
   `Panel`-Aufruf vor, mit den vorhandenen Stilen; der Katalog liegt im
   Bibliothek-Panel;
5. die geöffnete Karte trägt `aria-label`, schließt per Escape und setzt den
   Fokus auf ihren Namen.

## Prüfung

```
cd /workspace/projekte/board-plugin && node --check client.js && node --test test/*.mjs
```

Ergebnis: **Exit-Code 0**, 69 von 69 Prüfungen grün (68 vorhandene + die neue
`test/nach61.mjs`).

## Die entscheidende Zeile

```js
...(onSchliessen
  ? { onKeyDown: (ev) => {
    if (ev.key !== 'Escape' || ev.defaultPrevented) return;
    ev.preventDefault();
    ev.stopPropagation();
    onSchliessen();
  } }
  : null),
```

und der eine Name des Schließens:

```js
const schliessenName = schliessenTitel || uebersetze('Schließen');
```
