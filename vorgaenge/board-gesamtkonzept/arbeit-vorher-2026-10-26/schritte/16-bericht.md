# Bericht Schritt 16 — zwei Programmfassungen schreiben nicht gegeneinander

- **Stand:** 2026-10-26
- **Gebaut:** Dieses Programm kennt seine eigene Fassung (`FORMAT = 1`) und liest
  die Fassung der Ablage. `/board/data` liefert beide Zahlen. Weicht die Ablage
  ab, wird gelesen, aber nicht geschrieben: Jede schreibende Route antwortet mit
  409 und nennt die Zahl der Ablage; ist die Ablage älter, sagt der Text, dass
  sie den Umzug braucht. Die Seite zeigt bei ungleichen Fassungen einen Hinweis
  mit beiden Zahlen. Neu sind die Prüfung vor jeder Route, der Hinweis in der
  Seite, das Prüfskript `produkt/test/schritt16.mjs` und die Fassung
  `Fassungen`.
- **Nicht neu gestartet:** Das laufende Board arbeitet weiter mit dem geladenen
  alten Modul. Geprüft ist alles an Kopien in `/tmp`; die echte Ablage blieb auf
  `format` 1.

## Zwei Fassungen in `/board/data`

`/board/data` trägt jetzt zwei Zahlen:

```json
{
  "format": 1,
  "formatAblage": 2,
  "workflows": [],
  "vorgaenge": [],
  "beitraege": [],
  "personen": [],
  "einstellungen": {},
  "stand": {}
}
```

- `format` ist `FORMAT` aus `produkt/ablage.js`, die Fassung, die dieses Programm
  schreibt und versteht.
- `formatAblage` ist die Zahl aus der Datei `format` der Ablage. Lässt sie sich
  nicht lesen, steht dort `null`; die Seite bleibt benutzbar, und ein Schreiben
  scheitert ohnehin an derselben Prüfung.
- Die Form ändert sich für bestehende Leser nur um diese zwei Schlüssel. Drei
  Prüfskripte, die die Schlüssel von `/board/data` genau vergleichen (03, 05,
  11), erwarten sie jetzt mit.
- `liesFormat` nimmt seit diesem Schritt auch die `0` an. Vorher galt jede Zahl
  unter 1 als ungültig; die ältere Ablage muss aber lesbar sein, damit das Board
  den Umzug nennen kann.

## Die Prüfung vor jedem Schreiben

Alle 46 Routen werden über eine gemeinsame Hülle registriert:

```js
function registriere(route) {
  const bearbeite = route.handler;
  return ctx.webServer.register({
    ...route,
    handler(req, res) {
      if (req.method !== 'GET' && !OHNE_ABLAGE_SCHREIBEN.has(route.path)) {
        try { pruefeFormatVorSchreiben(); }
        catch (fehler) {
          sende(res, (fehler && fehler.status) || 500, {
            error: String((fehler && fehler.message) || fehler),
          });
          return undefined;
        }
      }
      return bearbeite(req, res);
    },
  });
}
```

- **Die Prüfung steht vor dem Handler.** Sie läuft, bevor eine Route eine Datei
  anlegt, kopiert oder ein Vorhaben ändert. Ein 409 aus der Hülle kann deshalb
  nicht als 400 aus dem `catch` einer Route enden — die Routen antworten auf
  Fehler sonst pauschal mit 400.
- **Ist die Ablage neuer:** 409 mit
  `Die Ablage ist neuer (Format 2). Dieses Board kennt Format 1 und schreibt
  nicht, bis es aktualisiert ist.`
- **Ist die Ablage älter:** 409 mit
  `Die Ablage ist älter (Format 0). Dieses Board kennt Format 1; die Ablage
  braucht den Umzug.`
- **Ausgenommen** sind nur Wege, die keine Ablagedateien schreiben: der Abgleich
  (`/board/sync`), die beiden Modellwege (`/board/korrigieren`,
  `/board/ueberarbeiten`) und das Abbrechen eines laufenden Zuges
  (`/board/gespraech/stopp`, `/board/werkstatt/stopp`).
- **Zusätzlich in der Tiefe:** `schreibeAblage`, `schreibeBeitrag` und
  `schreibeEntscheid` prüfen selbst noch einmal, bevor sie schreiben. So bleibt
  die Regel auch dann, wenn eine neue Route die Hülle vergisst.
- **Auch Fassungen:** `festhalten` schreibt nichts fest, solange die Fassungen
  nicht übereinstimmen. Damit läuft der Zeitgeber für Änderungen außerhalb des
  Boards leer, statt gegen eine neuere Ablage zu committen.

## Die Seite

`formatHinweis(format, formatAblage)` steht neben `standZeile` in
`produkt/client.js`:

- Ablage neuer: `Die Ablage ist neuer: Format 2, dieses Board kennt Format 1.
  Dieses Board schreibt nicht, bis es aktualisiert ist.`
- Ablage älter: `Die Ablage ist älter: Format 0, dieses Board schreibt Format 1.
  Die Ablage braucht den Umzug.`
- Gleich: kein Hinweis.

Der Text steht als eigene Zeile unter dem Stand des Abgleichs, in einem
zurückhaltenden Warnfeld. Fehlt `formatAblage` (etwa beim noch laufenden alten
Modul), bleibt die Zeile weg.

## Prüfskript (`produkt/test/schritt16.mjs`)

Das Skript kopiert Ablage, Produkt und ein eigenes `DSH_HOME` nach `/tmp`,
entfernt den Remote und legt nur in der Kopie ein Vorhaben `eigenes-16` mit
einer gefassten `notiz.md` an. Danach prüft es:

1. der Ausgangsstand: `/board/data` nennt `format: 1` und `formatAblage: 1`;
2. `format` auf `2`: `/board/data` antwortet weiter, nennt `format: 1` und
   `formatAblage: 2`; Speichern und Verschieben antworten 409, der Text nennt
   `Format 2` und `Format 1` bzw. `Format 2`; Datei und `vorhaben.json` bleiben
   byte-gleich; keine neue Fassung, `git status` ruhig;
3. `format` zurück auf `1`: dieselben Schreibvorgänge gehen durch, die Datei
   ändert sich, das Vorhaben steht danach auf `vision`, je eine Fassung;
4. `format` auf `0`: Speichern und Verschieben antworten 409, der Text nennt
   `Format 0` und `Umzug`; nichts wird geschrieben, keine Fassung;
5. die Seite enthält den Hinweis mit beiden Zahlen, den Satz zum Nichtschreiben
   und den Umzug;
6. die echte Ablage steht weiter auf 1, hat kein Prüfvorhaben und keine neue
   Fassung.

Ausgabe des letzten Laufs:

```
ok Ausgang: /board/data nennt format 1 und formatAblage 1
ok Ablage 2: /board/data antwortet weiter, format 1, formatAblage 2
ok Ablage 2, Speichern: 409 nennt Format 2 und Format 1, Datei byte-gleich
ok Ablage 2, Verschieben: 409 nennt Format 2, vorhaben.json byte-gleich
ok Ablage 2: keine neue Fassung, git status ruhig
ok Ablage 1: Speichern und Verschieben gehen durch, je eine Fassung
ok Ablage 0: Schreiben abgewiesen, Umzug genannt, nichts geschrieben
ok Seite: Hinweis mit beiden Zahlen, Nichtschreiben, Umzug
ok echte Ablage unberührt: format 1, kein Prüfvorhaben, keine Fassung
ok Prüfordner aufgeräumt: /tmp/board-schritt16-…
```

**Entscheidende Zeile:** `ok Ablage 2, Speichern: 409 nennt Format 2 und
Format 1, Datei byte-gleich` — die neuere Ablage wird nicht beschrieben, und der
Fehler nennt die verlangte Fassung. Dazu `ok Ablage 1: Speichern und
Verschieben gehen durch, je eine Fassung` — dieselben Wege schreiben wieder,
sobald die Zahlen übereinstimmen.

## Dateien

- `produkt/ablage.js` — `FORMAT` (18); `liesFormat` nimmt 0 an (63);
  `pruefeFormat` mit 409 und beiden Texten (84); Prüfung in `schreibeBeitrag`
  (383), `schreibeEntscheid` (422) und `schreibeAblage` (633).
- `produkt/index.js` — `formatDerAblage` (304), `pruefeFormatVorSchreiben`
  (318), `schreibeBoard` mit dem Status der Formatprüfung (327–335), die
  Prüfung in `festhalten` (387–392), beide Zahlen in `datenAntwort` (1559), die
  gemeinsame Hülle `registriere` (1586) und die Ausnahmen (1578); alle 46
  Routen werden darüber registriert.
- `produkt/client.js` — `formatHinweis` (57), das Warnfeld (225), der Hinweis
  in der Kopfzeile (5949, 5975).
- `produkt/test/schritt16.mjs` — neu.
- `produkt/test/schritt03.mjs`, `05.mjs`, `11.mjs` — erwarten die zwei neuen
  Schlüssel in `/board/data`.
- `arbeit/schritte/16-bericht.md` — dieser Bericht.

## Prüfungen

| Nr | Befehl | Ergebnis | Exit |
| --- | --- | --- | --- |
| 1 | `node …/produkt/test/schritt16.mjs` | zehn ok-Zeilen: Ausgang, Ablage 2 (Lesen, Speichern, Verschieben, keine Fassung), Ablage 1, Ablage 0, Seite, echte Ablage, aufgeräumt | 0 |
| 2 | `node …/produkt/test/schritt15.mjs` | Überholter Stand unverändert, zwölf ok-Zeilen | 0 |
| 3 | `node …/produkt/test/schritt04.mjs` | Abgleich unverändert, sechs ok-Zeilen | 0 |
| 4 | `node …/produkt/test/schritt03.mjs` | Ablage, Umzug und Fassung unverändert | 0 |
| 5 | `node …/produkt/test/schritt02.mjs` | ohne Konfiguration und mit Fassung unverändert | 0 |
| 6 | `node …/produkt/test/schritt05.mjs` bis `14.mjs` | alle zehn grün | 0 |
| 7 | `node --check produkt/index.js`, `ablage.js`, `client.js`, `test/schritt16.mjs` | keine Syntaxfehler | 0 |
| 8 | `test "$(cat /workspace/projekte/board/format)" = 1` | echte Ablage unverändert | 0 |
| 9 | `git -C /workspace/projekte/board status --porcelain` | leer nach der Fassung | 0 |
| 10 | `git -C /workspace/projekte/board log -1 --format='%an %s'` | `Thomas Heinrich Fassungen` | 0 |

## Entscheidungen

- **Eine Hülle statt einer Prüfung je Route.** 46 Routen einzeln zu prüfen,
  hieße: eine vergessen. Die Hülle um `ctx.webServer.register` erwischt jede
  schreibende Route, auch die, die erst später dazukommt, und die Reihenfolge
  stimmt: Die Prüfung läuft vor dem Schreiben, nicht danach.
- **`liesFormat` nimmt 0 an.** Eine 0 ist keine kaputte Datei, sondern eine
  ältere Ablage. Ohne diese Änderung könnte das Board den Umzug nicht nennen;
  es würde „kein gültiges Format“ sagen.
- **409 für beide Richtungen.** Der Auftrag nennt 409 für die neuere Ablage. Die
  ältere ist derselbe Konflikt — zwei Fassungen passen nicht zusammen; sie
  bekommt denselben Status mit dem Wort „Umzug“.
- **`format` fehlt in `/board/data` nicht.** Die Seite braucht beide Zahlen an
  einer Stelle; ein eigener Aufruf für die Zahl der Ablage wäre eine zweite
  Wahrheit und ein zweiter Fehlerfall.
- **Lesen ohne Prüfung.** GET-Routen und `/board/data` prüfen nicht. Der Auftrag
  verlangt die Benutzbarkeit beim Lesen; nur das Schreiben wird abgewiesen.
- **Ausnahmen sind Wege ohne Ablagedatei.** Abgleich, Korrektur, Überarbeitung
  und das Abbrechen eines Zuges schreiben keine Datei in der Ablage. Sie zu
  sperren, würde Arbeit verhindern, ohne das Formatproblem zu lösen.
- **`festhalten` prüft mit.** Der Zeitgeber für Änderungen außerhalb des Boards
  würde sonst als einziger Weg weiter committen. Jetzt bleibt er stehen, solange
  die Fassungen verschieden sind.

## Fassung

Der Stand der Ablage ist als Fassung `Fassungen` festgehalten, Urheber
`Thomas Heinrich` (`thomas.heinrich@board`). Sie enthält diesen Schritt, diesen
Bericht und die Arbeitsstände in `arbeit/`, die beim Schritt im Arbeitsbaum
lagen. Die Änderungen am Produkt (`produkt/ablage.js`, `index.js`, `client.js`,
`test/schritt16.mjs` und die drei angepassten Prüfskripte) sind wie in den
Schritten davor nicht Teil der Board-Fassung: `vorgaenge/*/produkt/` ist im
Repository ignoriert.

## Grenzen und offene Punkte

- **Nicht neu gestartet.** Das laufende Board antwortet weiter mit dem alten
  Modul; `format`, `formatAblage` und der Hinweis erscheinen erst nach einem
  Neustart. Die echte Ablage bleibt auf 1.
- **Im Browser nicht gesehen.** Die Seite ist syntaktisch geprüft und ihr
  Hinweistext am Quelltext belegt; das tatsächliche Bild ist nicht geprüft, weil
  dafür ein Neustart nötig wäre.
- **Nur der Server entscheidet.** Die Seite zeigt den Hinweis, sperrt aber keine
  Knöpfe. Wer trotzdem speichert, bekommt den 409 und den Text der Route; die
  Ablage bleibt unverändert.
- **`formatAblage: null` bei unlesbarer Datei.** Dann gibt es keinen Hinweis;
  ein Schreiben antwortet 500 mit „Die Ablage nennt kein Format“. Ein eigener
  Hinweis für diesen Fall war nicht Teil des Auftrags.
- **Die Ausnahmen sind eine Liste.** Eine neue Route, die nur liest, aber POST
  benutzt, muss in `OHNE_ABLAGE_SCHREIBEN` eingetragen werden; sonst wird sie
  bei ungleichen Fassungen mit 409 abgewiesen. Das ist die sichere Richtung:
  lieber einmal zu viel abweisen als einmal zu viel schreiben.
