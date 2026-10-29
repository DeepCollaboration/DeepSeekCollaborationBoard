# Schritt 64 — Bericht

## Gebaut

`client.js` erklärt jetzt die leere Fläche, hält die Bibliothek im Bild und
zeigt mit einem Knopf alle Karten. Dazu `test/nach64.mjs` und zwei neue
Schlüssel in den sieben Wörterbüchern.

## Der Satz für die leere Fläche

Steht genau einmal, als `uebersetze('…')`, in der Fläche:

> Noch keine Station auf dieser Fläche: Stationen entstehen in der Bibliothek
> — öffne sie über den Reiter „Bibliothek“ und stelle sie von dort auf die
> Fläche.

Er hängt an `!karten.length`, steht mittig in einem absolut gesetzten,
klickdurchlässigen Feld (`S.leerFlaeche`, `S.leerSatz`) und nennt den
vorhandenen Weg. Kein Knopf führt ins Leere.

Der Satz steht in `ar, en, es, fr, ja, ru, zh`; `texte/de.json` bleibt leer,
weil der deutsche Text selbst der Schlüssel ist. Der Knopf trägt den zweiten
neuen Schlüssel `Alles zeigen`.

## Die Bibliothek

- `S.leistenFlaeche` ist jetzt `boxSizing: border-box`, eine Spalte
  (`display: flex`, `flexDirection: column`) und trägt
  `maxHeight: 'calc(100vh - 220px)'`.
- Der Inhalt liegt in `S.bibInhalt` (`flex: 1, minHeight: 0,
  overflow: 'auto'`). Der Panel-Kopf steht außerhalb dieses Feldes und bleibt
  stehen; nur der Inhalt rollt. Das Bibliothek-Panel gibt weiter
  `seiteStil: S.leistenFlaeche` hinein.
- Die Verlaufsfläche benutzt dieselbe Leistenfläche und bekommt die Grenze
  damit ebenfalls; sie war vorher schon innen begrenzt (`S.fassungen`).

## „Alles zeigen“

Ein Knopf in `S.leiste`, rechts in der Reiterzeile, nur wenn `karten.length`
wahr ist. Sonst fehlt er. Er rechnet:

1. äußere Kanten aller Karten: `links = min(x)`, `oben = min(y)`,
   `rechts = max(x + breiteFuer(p))`, `unten = max(y + Höhe)`, wobei eine
   geöffnete Station 760 px hoch ist, eine geschlossene `KARTEN_HOEHE`;
2. Mittelpunkt dieser Kanten;
3. `scrollLeft = clamp(mitteX - clientWidth / 2, 0, scrollWidth - clientWidth)`
   und `scrollTop` ebenso mit `clientHeight`/`scrollHeight`;
4. danach wird der Stand in `gemerkt.scroll` nachgeführt.

Kein Zoom, keine Übersichtskarte, kein neuer React-Zustand.

## Prüfdatei

`test/nach64.mjs` liest Client und Wörterbücher als Text und prüft:

1. der leere Satz steht als `uebersetze('…')` im Client, hängt an
   `!karten.length`, liegt in `S.leerFlaeche`/`S.leerSatz` und steht
   nicht leer in allen sieben Wörterbüchern;
2. das Bibliothek-Panel trägt über `S.leistenFlaeche` ein `maxHeight` mit
   `calc(100vh - …)`, ist eine Spalte, und der Inhalt rollt in `S.bibInhalt`;
3. `allesZeigen` rechnet aus `karten.map` und begrenzt `scrollLeft` und
   `scrollTop` auf den scrollbaren Bereich; der Knopf hängt an
   `karten.length`, ruft die Funktion und steht in der Leiste;
4. die Suite selbst ist der vierte Punkt und läuft über
   `node --test test/*.mjs`.

## Geänderte Dateien

- `client.js` — zwei Stile für die leere Fläche, `bibInhalt`, Höchsthöhe der
  Leistenfläche, `allesZeigen`, leerer Satz in der Fläche, Knopf in der
  Leiste, Inhaltsrolle der Bibliothek.
- `texte/ar.json`, `texte/en.json`, `texte/es.json`, `texte/fr.json`,
  `texte/ja.json`, `texte/ru.json`, `texte/zh.json` — je zwei Schlüssel.
- `test/nach64.mjs` — neu.

`index.js`, `texte/de.json` und alle übrigen Dateien sind unberührt.

## Prüfung

```
cd /workspace/projekte/board-plugin && node --check client.js && node --test test/*.mjs
```

Ergebnis: **Exit-Code 0**, 73 von 73 Prüfungen grün (72 vorhandene + die neue
`test/nach64.mjs`).

Die Sichtprüfung im Browser bleibt bei Schritt 68.
