# Bericht Schritt 58 — vier Bausteine, zuerst in den Einstellungen

## Gebaut

Die vier Bausteine stehen in `client.js` nach dem Stilblock `E` und vor der
ersten Stelle, die sie benutzt (1814–1962):

- **`Knopf`** (1825–1838): immer `type="button"`; `stil` liegt über dem
  Grundstil, `klein` legt `S.klein` darunter; `symbol` wird zu `aria-label`
  und — ohne eigenen `title` — auch zum Titel; `art` wird aus den Props
  genommen, bleibt aber ohne Wirkung auf die Farbe (Schritt 63).
- **`Feld`** (1845–1869): `<label htmlFor={id}>` über `<input>` bzw.
  `<textarea>` mit `id`, `value` und `onChange`; `hinweis` und `fehler`
  bekommen `${id}-hinweis` und `${id}-fehler`, hängen über
  `aria-describedby` am Feld, der Fehler zusätzlich mit `role="alert"`. Die
  Kennung vergibt die aufrufende Stelle (kein `useId`).
- **`Panel`** (1877–1932): `abschnitt` rendert `<section role="region">`,
  `dialog` ein `<div role="dialog" aria-modal="true">`, beide mit
  `aria-labelledby`; Kopf als `<h2 id>` mit `margin: 0` und `<p>` für den
  Untertitel, optional rechts ein `Knopf` mit `symbol="Schließen"` und `✕`;
  Escape schließt nur Dialoge mit `onSchliessen` (Ereignis gestoppt);
  `fokus` merkt sich das fokussierte Element, setzt den Fokus auf das Panel
  (`tabIndex: -1`) und gibt ihn beim Verschwinden zurück, wenn das Element
  noch im Dokument hängt. Alle `document`-Zugriffe stehen hinter
  `typeof document !== 'undefined'`.
- **`Meldung`** (1951–1962): `laeuft`/`fertig` mit `role="status"` und
  `aria-live="polite"`, `fehler` mit `role="alert"` und
  `aria-live="assertive"`, dazu `aria-atomic="true"` und das Zeichen `…`,
  `✓` bzw. `!`; leer rendert nichts, nichts verschwindet von selbst.

Dazu der Helfer **`meldungZustand`** (1939–1945): er liest den Zustand aus
dem Text der bisherigen Meldungen („Nicht“, „Konnte“, „Bitte“ … sind Fehler),
bis Schritt 62 ihn am Aufrufer führt.

Umgestellt:

- **`BoardEinstellungen`** (1969–2189): Kopf und Rahmen über `Panel`
  (`rolle: 'abschnitt'`, `seiteStil: E.seite`, `kopfStil: E.kopf`,
  `aria-labelledby="board-einstellungen"`); Ladehinweis „Wird geladen …“
  unter dem Kopf (2104–2106), im Ladezustand ohne den Satz über das
  fehlende Schreibrecht (2180–2183); Fehlerzustand (2068–2092) mit
  `Meldung` (`fehler`) und dem Knopf „Nochmal“, der `laden()` erneut ruft;
  der Eigentümer-Eintrag ist ein `Feld` mit `label` und einem `Knopf`
  (2147–2184); die vorhandene Meldung ist eine `Meldung` (2058–2066);
  `'laeuft'` wurde dabei nicht eingeführt.
- **Stationsdetail**: die Knöpfe „Bearbeiten“ und „Klein machen“ im Kopf der
  geöffneten Karte sind `Knopf` mit denselben Stilen (`klein` +
  `stil: { flex: 'none' }` bzw. `S.thStift`) und Symbolnamen (7441–7457);
  „Name“ und „Beschreibung“ im `stationFormular` sind `Feld` mit `label` und
  `id` `station-name` / `station-beschreibung` (6634–6647), die übrigen
  Felder blieben unverändert; die Meldungszeile des Boards, in der die
  Stationshandlungen melden, ist eine `Meldung` mit `S.sub` (9406–9413).
- **Neue Prüfung `test/nach58.mjs`**: lädt den Client ohne Browser
  (Ersatz-React, nachgebildeter Modullader und `fetch`), rendert den
  Einstellungsabschnitt und prüft Rendern, `section`/`role="region"` mit der
  Kennung der `h2`, `label`↔`input`, den Ladezustand („Wird geladen …“ vor
  den Einstellungen, kein Schreibrecht-Satz) sowie je eine Definition von
  `Knopf`, `Feld`, `Panel`, `Meldung` und die Aufrufe von `Knopf`, `Feld`
  und `Meldung`.

## Dateien

- `client.js` (geändert): Bausteine 1814–1962; Einstellungen 1969–2189;
  Stationsformular 6634–6647; Kartenknöpfe 7441–7457; Meldungszeile
  9406–9413. Sonst nichts.
- `test/nach58.mjs` (neu).

`index.js`, `package.json`, `texte/*.json` und die übrigen Prüfungen blieben
unberührt; die Schlüsselmenge aus `client.js` bleibt bei 555 — kein neuer
sichtbarer Text ohne Wörterbuch.

## Befehl und Ergebnis

```
cd /workspace/projekte/board-plugin && node --check client.js && node --test test/*.mjs
```

Exit-Code: **0** — 66 Tests, 66 bestanden, 0 Fehler (65 vorhandene plus
`test/nach58.mjs`). `test/markt48.mjs` ist grün.

## Entscheidende Zeile

```
client.js:2106  ? h('p', { key: 'laedt', style: E.hinweis }, uebersetze('Wird geladen …'))
```

zusammen mit `client.js:2180–2183`, wo derselbe Zustand den Satz „Für dieses
Board fehlt das Schreibrecht.“ zurückhält.
