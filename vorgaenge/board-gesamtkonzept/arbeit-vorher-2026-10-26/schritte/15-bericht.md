# Bericht Schritt 15 — Speichern auf einem überholten Stand schreibt nicht

- **Stand:** 2026-10-26
- **Gebaut:** Jede Datei trägt beim Öffnen ihre `basis` — den Commit, der sie
  zuletzt geändert hat. Das Speichern prüft sie: weicht sie ab, wird nichts
  geschrieben, die Antwort ist 409 mit dem neuen Stand. Der eigene Text bleibt
  im Editor und lässt sich mit der neuen `basis` als Beitrag anlegen. Nichts
  wird zusammengeführt. Neu sind die Basis im Lesezugriff, die Prüfung vor dem
  Schreiben, der Weg „mein Text als Beitrag“ in der Seite, das Prüfskript
  `produkt/test/schritt15.mjs` und die Fassung `Überholter Stand`.
- **Nicht neu gestartet:** Das laufende Board arbeitet weiter mit dem geladenen
  alten Modul. Geprüft ist alles an einer Modulkopie in `/tmp`.

## Die Basis je Datei

`GET /board/doc` liefert zusätzlich zu `name`, `exists` und `text` das Feld
`basis`:

```json
{
  "name": "notiz.md",
  "exists": true,
  "text": "…",
  "basis": "1171162a6d…"
}
```

- `basis` ist der Commit, der diese Datei zuletzt geändert hat. Ermittelt wird
  er mit `git log -1 --format=%H -- <pfad>`; der Pfad ist relativ zum
  Repository, also `vorgaenge/<ordner>/<datei>`.
- Ein Commit anderswo im Repository ändert die Basis nicht. Nur ein Commit, der
  diese Datei berührt, tut es.
- Eine Datei, die es noch nicht gibt, und eine unversionierte Datei tragen
  `null`. Ein Ordner trägt ebenfalls `null`.
- Der Editor holt die Basis beim Öffnen über dieselbe Route. Dafür gibt es in
  der Seite `oeffneDateiMitBasis`; ein Klick auf eine Datei an der Station oder
  auf einen Link im Leser geht jetzt darüber.

## Die Prüfung vor dem Schreiben

`POST /board/doc/schreiben` liest die aktuelle Basis der Datei, bevor es
schreibt. Ist im Körper ein Feld `basis` enthalten und weicht sein Wert ab, wird
nicht geschrieben:

```json
{
  "ok": false,
  "neu": { "text": "… der neue Stand …", "basis": "77ddd2c…" }
}
```

- Status **409**. `neu.text` ist der aktuelle Text der Datei, `neu.basis` ihre
  aktuelle Basis. Die Datei bleibt byte-gleich, es entsteht keine Fassung.
- Ein nicht vorhandenes, leeres oder ungültiges `basis`-Feld zählt als
  Abweichung, außer `null` passt zur noch nicht gefassten Datei.
- **Ohne** `basis` im Körper wird wie bisher geschrieben — die Prüfung greift
  nur, wenn eine Basis mitgegeben ist.
- Eine richtige Basis schreibt die Datei und hält sie wie bisher als Fassung
  fest. Die Antwort trägt die neue Basis: `{ ok: true, name, laenge, basis,
  fassung }`. `basis` und `fassung` sind derselbe Commit — der, der die Datei
  jetzt zuletzt geändert hat.
- Nichts wird zusammengeführt. Der neue Stand kommt nur zurück; der eigene Text
  bleibt beim Aufrufer.

## Der Weg „mein Text als Beitrag“

`POST /board/beitrag` nimmt jetzt optional eine `basis` an. Die Seite schickt
bei einem überholten Stand `neu.basis` mit; der Beitrag speichert diesen Wert
statt des bisherigen `HEAD`. Ohne Angabe bleibt es beim bisherigen Verhalten
(`HEAD`), damit Schritt 11 unverändert gilt.

Der Beitrag liegt wie jeder andere unter
`beitraege/<vorhaben>/<beitrag>.json` mit den vorgeschlagenen Fassungen daneben.
Die Datei im Vorhaben wird nicht angefasst.

## Die Seite

- Beim Öffnen einer Datei im Wurzelordner wird ihre `basis` mitgelesen.
- Beim Speichern geht `leser.basis` mit. Nach Erfolg übernimmt der Editor die
  neue Basis aus der Antwort — sonst würde das nächste Speichern gegen den
  eigenen vorigen Stand laufen.
- Bei **409** bleibt der eigene Text im Editor stehen, das Speichern wird nicht
  wiederholt. Über dem Editor steht: „Der Stand ist neuer — gespeichert wurde
  nichts. Dein Text steht weiter im Editor; die Datei im Vorhaben bleibt
  unverändert.“ Der neue Stand lässt sich aufklappen.
- Zwei Wege stehen dort: **„Eigenen Text als Beitrag anlegen“** schickt den
  eigenen Text mit `neu.basis` an `/board/beitrag`; danach zeigt der Leser den
  neuen Stand. **„Neuen Stand übernehmen“** holt den neuen Text in den Leser,
  ohne etwas zu schreiben.
- Auch das Zurücksetzen auf eine frühere Fassung prüft jetzt die Basis und
  zeigt bei 409 den neuen Stand, statt ihn zu überschreiben.

## Prüfskript (`produkt/test/schritt15.mjs`)

Das Skript kopiert Ablage, Produkt und ein eigenes `DSH_HOME` nach `/tmp`,
entfernt den Remote und legt nur in der Kopie ein eigenes Vorhaben
`eigenes-15` mit einer gefassten `notiz.md` an. Danach prüft es:

1. `/board/doc` liefert die Basis der Datei (der Setup-Commit); eine fehlende
   Datei trägt `null`;
2. ein Commit an einer anderen Datei ändert diese Basis nicht;
3. Speichern mit richtiger Basis: 200, Datei geändert, neue Fassung, neue Basis
   verschieden und gleich der Fassung der Datei;
4. die Datei von außen ändern und unter „Zweite Person“ festschreiben; mit der
   alten Basis speichern: 409, `neu.text` ist der äußere Stand, `neu.basis` die
   äußere Fassung, die Datei byte-gleich, keine neue Fassung;
5. den eigenen Text über `/board/beitrag` mit der neuen Basis anlegen: der
   Beitrag liegt unter `beitraege/eigenes-15/…/notiz.md`, trägt die neue Basis,
   die Datei im Vorhaben bleibt byte-gleich;
6. mit der neuen Basis speichern: geht durch, obwohl inzwischen der Beitrag als
   Fassung dazukam — die Datei selbst hat sich nicht geändert;
7. zwei Schreibvorgänge auf denselben Stand: der zweite mit der alten Basis
   bekommt 409, die Datei bleibt beim ersten;
8. ohne Basis wird wie bisher geschrieben;
9. eine unversionierte Datei trägt `null`; Speichern mit `null` geht durch und
   setzt danach eine Basis;
10. die Seite enthält Basis beim Öffnen und Speichern, erkennt 409 samt
    `neu`, behält den eigenen Text (`eigener: text`), sagt „Der Stand ist
    neuer“ und bietet „Eigenen Text als Beitrag anlegen“ mit `konflikt.basis`;
11. die echte Ablage bleibt byte-gleich, ohne Prüfvorhaben, Beitrag und zweite
    Person.

Ausgabe des letzten Laufs:

```
ok Basis beim Öffnen: 0909c0e, fehlende Datei null
ok Basis bleibt: ein Commit an anderer Stelle ändert sie nicht
ok Speichern mit Basis: Datei geändert, neue Fassung f987002, neue Basis verschieden
ok 409: neuer Text „Von außen geändert und festgeschrieben.“, neue Basis e46b673, Datei byte-gleich
ok Beitrag: eigene Fassung unter beitraege/eigenes-15/2026-10-26-203347/notiz.md mit der neuen Basis, die Datei unverändert
ok neue Basis: Speichern geht durch, neue Basis a59c4bd
ok zwei Schreibvorgänge: der zweite mit alter Basis bekommt 409; die Datei bleibt beim ersten
ok ohne Basis: wird wie bisher geschrieben
ok neue Datei: Basis null beim Öffnen, danach gefasst
ok Seite: eigener Text bleibt, neuer Stand, Weg als Beitrag
ok echte Ablage unberührt: kein Prüfvorhaben, kein Beitrag, keine Fassung
ok Prüfordner aufgeräumt: /tmp/board-schritt15-…
```

**Entscheidende Zeile:** `ok 409: neuer Text „Von außen geändert und
festgeschrieben.“, neue Basis …, Datei byte-gleich` — der überholte Stand wird
nicht geschrieben, der neue kommt zurück. Dazu `ok Beitrag: eigene Fassung unter
beitraege/eigenes-15/…/notiz.md mit der neuen Basis, die Datei unverändert` —
der eigene Text geht als Beitrag in die Ablage, das Original bleibt.

## Dateien

- `produkt/index.js` — `dateiBasis` (738–748); Basis in `/board/doc`
  (1578–1583); Basisprüfung und 409 in `/board/doc/schreiben` (1715–1738); neue
  Basis in der Antwort (1750–1757); `basis` im Beitrag (1843–1862).
- `produkt/client.js` — `poste` behält Status und Körper am Fehler (1720–1737);
  `dateiSpeichern` mit Basis und 409 (1745–1804); `konfliktAlsBeitrag` (1807),
  `konfliktNeuenStand` (1840); Basis beim Beitrag (1873); Basis beim
  Zurücksetzen (2012–2030); `oeffneDateiMitBasis` (2169); Station und Link
  öffnen darüber (2187, 4585); 409-Banner und zwei Wege (5686–5726).
- `produkt/test/schritt15.mjs` — neu, 494 Zeilen.
- `arbeit/plan.md`, `arbeit/fortschritt.md`, `arbeit/pruefung.md` — Runde 15,
  Status, Prüfvermerk.
- `arbeit/schritte/15-bericht.md` — dieser Bericht.

## Prüfungen

| Nr | Befehl | Ergebnis | Exit |
| --- | --- | --- | --- |
| 1 | `node …/produkt/test/schritt15.mjs` | zwölf ok-Zeilen: Basis, unverändert bei fremdem Commit, Speichern, 409, Beitrag, neue Basis, zwei Schreibvorgänge, ohne Basis, neue Datei, Seite, echte Ablage, aufgeräumt | 0 |
| 2 | `node …/produkt/test/schritt14.mjs` | Rücksprung unverändert | 0 |
| 3 | `node …/produkt/test/schritt11.mjs` | Beitrag unverändert, `basis` ohne Angabe weiter `HEAD` | 0 |
| 4 | `node …/produkt/test/schritt03.mjs` | Ablage und Umzug unverändert | 0 |
| 5 | `node …/produkt/test/schritt02.mjs`, `04`–`13` | alle elf grün | 0 |
| 6 | `node --check produkt/index.js`, `client.js`, `test/schritt15.mjs` | keine Syntaxfehler | 0 |
| 7 | `git -C /workspace/projekte/board status --porcelain` | leer nach der Fassung | 0 |
| 8 | `git -C /workspace/projekte/board log -1 --format='%an %s'` | `Thomas Heinrich Überholter Stand` | 0 |

## Entscheidungen

- **Die Basis ist die Datei, nicht das Repository.** `git log -1 -- <pfad>`
  liefert den Commit, der diese Datei zuletzt geändert hat. Damit bleibt die
  Basis stehen, wenn anderswo gespeichert wird — der Kern des Schritts.
- **Nur eine mitgegebene Basis prüft.** Fehlt das Feld, schreibt der Host wie
  bisher. So bleiben ältere Aufrufer und die Prüfskripte 02–14 gültig; die Seite
  gibt die Basis immer mit.
- **`null` ist eine Basis.** Eine noch nicht gefasste Datei trägt `null`; wird
  sie inzwischen anderswo angelegt, ist das eine Abweichung und gibt 409. Nur
  `null` auf `null` schreibt.
- **Kein Zusammenführen.** Der Server gibt den neuen Stand zurück und schreibt
  nichts. Die Seite hält den eigenen Text und bietet ihn als Beitrag an; damit
  bleibt die Entscheidung beim Menschen.
- **Der Beitrag trägt die neue Basis.** Die Route nimmt `basis` optional an;
  ohne Angabe bleibt `HEAD` das bisherige Verhalten. Die Seite schickt bei 409
  `neu.basis` — so sitzt der Vorschlag auf dem Stand auf, den er gesehen hat.
- **Die neue Basis kommt in der Antwort zurück.** Sonst liefe der Editor nach
  dem ersten Speichern gegen seinen eigenen alten Stand. `basis` und `fassung`
  sind derselbe Commit.
- **Auch das Zurücksetzen prüft.** Ein alter Verlaufseintrag darf keinen
  inzwischen geänderten Stand überschreiben; bei 409 zeigt der Leser den neuen
  Stand.

## Fassung

Der Stand der Ablage ist als Fassung `Überholter Stand` festgehalten, Urheber
`Thomas Heinrich` (`thomas.heinrich@board`). Sie enthält den Schritt 15, diesen
Bericht und die Arbeitsstände in `arbeit/`, die beim Schritt im Arbeitsbaum
lagen. Die Änderungen am Produkt (`produkt/index.js`, `produkt/client.js`,
`produkt/test/schritt15.mjs`) sind wie in den Schritten davor nicht Teil der
Board-Fassung: `vorgaenge/*/produkt/` ist im Repository ignoriert.

## Grenzen und offene Punkte

- **Nicht neu gestartet.** Das laufende Board antwortet weiter mit dem alten
  Modul; Basis, 409 und der Beitragsweg erscheinen erst nach einem Neustart.
- **Im Browser nicht gesehen.** Die Seite ist syntaktisch geprüft und ihre
  Bedingungen am Quelltext belegt; das tatsächliche Bild ist nicht geprüft, weil
  dafür ein Neustart nötig wäre.
- **Nur der Wurzelordner hat eine Basis.** `/board/doc` liest Dateien im
  Wurzelordner des Vorhabens; tiefere Dateien öffnen über `/board/datei` und
  sind im Leser nicht bearbeitbar. Für sie gibt es keine Basisprüfung.
- **Unversionierte Änderungen zählen nicht als Fassung.** Wer eine Datei am
  Board vorbei ändert, ohne festzuschreiben, ändert die Basis nicht; erst der
  Commit tut es. Das entspricht der Definition „Fassung = Commit“.
- **Der Beitrag ist eine eigene Fassung.** Er ändert die Basis der Datei nicht;
  das ist im Prüfskript belegt (Speichern mit der neuen Basis geht nach dem
  Beitrag durch).
- **Kein echter überholter Stand in der Ablage.** `eigenes-15` und der Beitrag
  leben nur in der Kopie in `/tmp`; die echte Ablage hat weiter fünf Vorhaben,
  eine Person und keinen Beitrag.
