# Bericht Schritt 12 — Der Verantwortliche entscheidet über den Beitrag

- **Stand:** 2026-10-26
- **Gebaut:** `POST /board/beitrag/entscheiden` entscheidet einen Beitrag ganz,
  in Teilen oder ablehnend. Annehmen schreibt die übernommenen Dateien aus
  `beitraege/<vorhaben>/<beitrag>/` in den Ordner des Vorhabens und hält sie mit
  dem eigenen Urheber als neue Fassung fest; Ablehnen lässt den Stand des
  Vorhabens unverändert. In beiden Fällen trägt die Beitragsdatei
  `entschieden = { von, am, art, teile }`. `/board/data` liefert den Entscheid
  an jedem Beitrag, die Seite zeigt ihn und bietet Annehmen, Teile und Ablehnen
  nur dem Verantwortlichen an. Prüfskript `produkt/test/schritt12.mjs`; die
  Fassung `Beitrag entschieden`.
- **Nicht neu gestartet:** Das laufende Board arbeitet weiter mit dem geladenen
  alten Modul. Die neue Route und die Entscheid-Handlungen erscheinen erst nach
  einem Neustart. Geprüft ist alles an einer Modulkopie in `/tmp`.

## Die Route

`POST /board/beitrag/entscheiden` nimmt `{ beitrag, art: "annehmen" | "ablehnen",
teile? }` und antwortet `{ ok, beitrag }`.

- **Zuerst die Zuständigkeit.** Die Route sucht den Beitrag (sonst 404), dann
  sein Vorhaben und fragt `darf(vorgang, "entscheiden", PERSON_ID)`. Fehlt die
  Zuständigkeit, antwortet sie 403. Die Prüfstelle aus Schritt 06 hat dafür die
  fünfte Aktion `entscheiden` bekommen.
- **Einmal entschieden ist entschieden.** Ein Beitrag mit gesetztem
  `entschieden` antwortet 409 und wird nicht angefasst.
- **Annehmen.** Ohne `teile` werden alle vorgeschlagenen Dateien übernommen;
  mit `teile` nur die genannten. Jede Quelle wird erst gelesen und geprüft,
  dann als `vorgaenge/<ordner>/<datei>` geschrieben. Fehlt eine vorgeschlagene
  Datei oder ist ein Name unsauber, antwortet die Route 400 und schreibt
  nichts. Die übernommenen Dateien und der Entscheid in der Beitragsdatei
  werden in einer Fassung mit `autorMensch()` festgehalten — „Thomas Heinrich“.
- **Ablehnen.** Es wird keine Vorhabendatei geschrieben; nur die Beitragsdatei
  bekommt `{ von, am, art: "abgelehnt", teile: [] }`. Der Stand des Vorhabens
  bleibt byte-gleich, die Ablehnung selbst wird als Fassung festgehalten.
- **`von` ist die Kennung der eigenen Person** (`p1`), nicht ihr Name. Die Seite
  löst sie über `personen/` zum Namen auf. Das folgt der Vorgabe des Schritts
  und ist dieselbe Kennung, mit der `verantwortliche` arbeitet.
- **`teile: []` beim Annehmen** ist erlaubt: Der Beitrag gilt als angenommen,
  ohne dass eine Datei übernommen wird. Das ist die Form für einen Beitrag, der
  nur aus einer Notiz besteht.

Die Prüfstelle selbst bleibt eine Stelle: `darf` prüft weiter nur die
Zugehörigkeit zur Station; die neue Aktion ist keine zweite Rechteprüfung.

## Die Seite

- Der Leser eines Beitrags zeigt unten den **Entscheid**: „Angenommen“ oder
  „Abgelehnt“, den Namen der entscheidenden Person, den Zeitpunkt und die
  übernommenen Teile („Übernommen: a.md“ oder „keine Datei“).
- Ist noch nicht entschieden und darf die eigene Person entscheiden, stehen
  dort Kästchen für jede vorgeschlagene Datei (alle zuerst angehakt) und die
  Knöpfe „Annehmen“ und „Ablehnen“. „Annehmen“ schickt die angehakten Namen als
  `teile`; ohne Änderung sind das alle. Sonst zeigt die Seite nur „Noch nicht
  entschieden — das entscheidet die verantwortliche Person.“
- An der Station und in der fremden Fläche trägt der Beitragsknopf den kurzen
  Vermerk „angenommen/abgelehnt von ‹Name›“. Der Leser bleibt die Stelle mit
  allen Einzelheiten.
- Die Handlungen hängen an `darf.entscheiden` des Vorhabens, zu dem der Beitrag
  gehört. Ein Beitrag an einem fremden Vorhaben zeigt sie auch dann nicht, wenn
  die eigene Person ihn geschrieben hat.

## Prüfskript (`produkt/test/schritt12.mjs`)

Das Skript kopiert Ablage und Produkt nach `/tmp`, setzt `person: "p1"` in der
Kopie, entfernt den Remote und legt nur in der Kopie an:

- ein eigenes Vorhaben `eigenes-12` mit `vision.md`, `a.md` und `b.md`,
  verantwortet von `p1`;
- ein fremdes Vorhaben `fremd-12`, verantwortet von `p2`;
- drei Beiträge am eigenen Vorhaben (A: `vision.md`, B: `a.md` und `b.md`,
  C: `vision.md`) und einen Beitrag am fremden Vorhaben.

Danach prüft es:

1. `darf.entscheiden` ist am eigenen Vorhaben wahr, am fremden falsch;
2. A annehmen: `vision.md` trägt den vorgeschlagenen Text, `entschieden.von` ist
   „p1“, `am` ein gültiges Datum, `art` „angenommen“, `teile` `["vision.md"]`;
   Beitragsdatei und vorgeschlagene Fassung bleiben liegen; die Fassung nennt
   „Thomas Heinrich“;
3. B nur mit `teile: ["a.md"]`: `a.md` geändert, `b.md` byte-gleich,
   `entschieden.teile` genau `["a.md"]`;
4. C ablehnen: keine Vorhabendatei geändert, `art` „abgelehnt“, `teile` leer;
5. zweiter Entscheid auf A → 409 ohne neue Fassung und ohne Änderung; Entscheid
   auf das fremde Vorhaben → 403, Beitrag und fremde Datei unverändert;
6. der Verlauf von `vision.md` hat nach dem Annehmen eine Fassung mehr, Urheber
   „Thomas Heinrich“, Nachricht mit „angenommen“;
7. `/board/data` trägt `entschieden` an jedem Beitrag, mit den erwarteten
   Werten für A, B, C und `null` beim fremden;
8. die Seite enthält `beitragEntscheiden`, den Weg zu
   `/board/beitrag/entscheiden`, die Bedingung `darf.entscheiden`, „Annehmen“,
   „Ablehnen“, die Anzeige von `entschieden.teile` und „Angenommen“/„Abgelehnt“
   (Quelltext, kein Browserbild);
9. die echte Ablage bleibt ohne `p2`, ohne die beiden Prüfvorhaben und ohne
   `beitraege/`.

Ausgabe des letzten Laufs:

```
ok Zuständigkeit: darf.entscheiden beim eigenen Vorhaben wahr, beim fremden falsch
ok A angenommen: vision.md übernommen, Beitrag liegt, Fassung von „Thomas Heinrich“
ok B in Teilen: a.md übernommen, b.md unverändert, teile ["a.md"]
ok C abgelehnt: keine Vorhabendatei geändert, art „abgelehnt“
ok 409 auf den zweiten Entscheid, 403 auf das fremde Vorhaben, nichts geändert
ok Verlauf: neue Fassung von vision.md, Urheber Thomas Heinrich
ok /board/data: entschieden an jedem Beitrag — A, B, C, fremd
ok Seite: Entscheid sichtbar, Annehmen/Ablehnen nur bei darf.entscheiden
ok echte Ablage unberührt: kein Beitrag, keine zweite Person
ok Prüfordner aufgeräumt: /tmp/board-schritt12-…
```

**Entscheidende Zeile:** `ok A angenommen: vision.md übernommen, Beitrag liegt,
Fassung von „Thomas Heinrich“` — der Vorschlag wird zur Datei des Vorhabens,
der Beitrag bleibt mit seinem Entscheid sichtbar, und die Fassung trägt den
Urheber. Dazu `ok 409 auf den zweiten Entscheid, 403 auf das fremde Vorhaben,
nichts geändert` — der Entscheid ist einmalig und gehört dem Verantwortlichen.

## Dateien

- `produkt/index.js` — die fünfte Aktion in `AKTIONEN` (137); die Route
  `/board/beitrag/entscheiden` (1774–1909); `schreibeEntscheid` wird dort
  aufgerufen (1889).
- `produkt/ablage.js` — `schreibeEntscheid` (390): prüft Vorhaben- und
  Beitrags-Kennung, liest die Beitragsdatei und schreibt nur `entschieden`.
- `produkt/client.js` — Zustand `entscheid` (1344), `beitragEntscheiden` (1797),
  `entscheidTeilUmschalten` (1833), Vorbelegung beim Öffnen (2013), kurzer
  Vermerk an der Station (4340), in der fremden Fläche (4922), Entscheid und
  Handlungen im Beitragsleser (`beitragInhalt`, 4973).
- `produkt/test/schritt12.mjs` — neu, 505 Zeilen.
- `produkt/test/schritt06.mjs` — die erwartete Form von `darf` trägt jetzt fünf
  statt vier Aktionen; sonst unverändert (211–213).
- `arbeit/schritte/12-bericht.md` — dieser Bericht.

## Prüfungen

| Nr | Befehl | Ergebnis | Exit |
| --- | --- | --- | --- |
| 1 | `node …/produkt/test/schritt12.mjs` | zehn ok-Zeilen: Zuständigkeit, A, B, C, 409/403, Verlauf, `/board/data`, Seite, echte Ablage, aufgeräumt | 0 |
| 2 | `node …/produkt/test/schritt11.mjs` | Beitragsweg unverändert | 0 |
| 3 | `node …/produkt/test/schritt06.mjs` | fünf Aktionen in `darf`, fremde Handlungen 403 | 0 |
| 4 | `node …/produkt/test/schritt03.mjs` | 5 Vorhaben, 11 Positionen, 11 Schritte | 0 |
| 5 | `node …/produkt/test/schritt02.mjs`, `04`, `05`, `07`, `08`, `09`, `10` | alle sieben grün | 0 |
| 6 | `node --check produkt/index.js`, `ablage.js`, `client.js`, `test/schritt12.mjs`, `test/schritt06.mjs` | keine Syntaxfehler | 0 |
| 7 | `ls /workspace/projekte/board/beitraege` | kein Ordner — echte Ablage ohne Beiträge | 2 |
| 8 | `git -C /workspace/projekte/board status --porcelain` | leer | 0 |
| 9 | `git -C /workspace/projekte/board log -1 --format='%an %s'` | `Thomas Heinrich Beitrag entschieden` | 0 |

## Entscheidungen

- **Der Entscheid schreibt in die Beitragsdatei, nicht in eine neue.** Ein
  Beitrag bleibt nach dem Entscheid lesbar; `entschieden` ist sein einziger
  neuer Zustand. Annehmen und der Vermerk liegen in einer Fassung, damit Datei
  und Entscheid nie auseinanderfallen.
- **`von` ist die Personenkennung.** Die Vorgabe des Schritts nennt „p1“; die
  Seite löst den Namen über `personen/` auf. So bleibt der Vermerk an dieselbe
  Kennung gebunden wie `verantwortliche`.
- **Ablehnen wird ebenso festgehalten wie Annehmen.** Auch eine Ablehnung ist
  eine Entscheidung, die im Verlauf stehen muss; geschrieben wird dabei nur die
  Beitragsdatei, keine Vorhabendatei.
- **`teile` wird streng geprüft.** Nur Namen, die im Beitrag vorgeschlagen sind,
  werden angenommen; Unbekanntes antwortet 400. Doppelte Namen werden still
  zusammengefasst.
- **Erst lesen, dann schreiben.** Fehlt eine vorgeschlagene Fassung, wird gar
  nichts übernommen. Ein halber Stand entsteht nicht; ein Schreibfehler nimmt
  die bereits geschriebenen Dateien bestmöglich zurück.
- **Die Prüfstelle bleibt eine Stelle.** `entscheiden` ist nur eine weitere
  Aktion in `darf`; die Bedingung ist dieselbe Zugehörigkeit wie bei den vier
  bisherigen Handlungen.
- **Das Prüfskript 06 wurde angepasst.** `darf` hat jetzt fünf statt vier
  Aktionen; die Formprüfung in `schritt06.mjs` erwartet sie mit. Das ist die
  einzige Änderung an einem älteren Prüfskript.

## Fassung

Der Stand der Ablage ist als Fassung `Beitrag entschieden` festgehalten, Urheber
`Thomas Heinrich` (`thomas.heinrich@board`). Sie enthält den Schritt 12, diesen
Bericht und die Arbeitsstände in `arbeit/`, die beim Schritt im Arbeitsbaum
lagen. Die Änderungen am Produkt (`produkt/index.js`, `produkt/ablage.js`,
`produkt/client.js`, `produkt/test/schritt12.mjs`, `test/schritt06.mjs`) sind wie
in den Schritten davor nicht Teil der Board-Fassung: `vorgaenge/*/produkt/` ist
im Repository ignoriert.

## Grenzen und offene Punkte

- **Nicht neu gestartet.** Das laufende Board antwortet weiter mit dem alten
  Modul; die Route und die Entscheid-Handlungen erscheinen erst nach einem
  Neustart.
- **Im Browser nicht gesehen.** Die Seite ist syntaktisch geprüft und ihre
  Bedingungen am Quelltext belegt; das tatsächliche Bild ist nicht geprüft, weil
  dafür ein Neustart nötig wäre.
- **Keine echte zweite Person in der echten Ablage.** `p2`, die beiden
  Prüfvorhaben und alle Beiträge leben nur in der Prüfkopie. Die echte Ablage
  hat kein `beitraege/`.
- **Ein Entscheid ist eine Fassung für alle Teile.** Wer mehrere Dateien
  übernimmt, bekommt einen Commit mit allen; die Fassung ist nicht je Datei
  getrennt. Für den Verlauf der einzelnen Datei ist das ausreichend, weil Git
  sie einzeln zeigt; eine getrennte Fassung je Datei war nicht verlangt.
- **`teile: []` beim Annehmen** ist die Form „Notiz angenommen, keine Datei“.
  Das Prüfskript deckt diesen Fall nicht ab; die Route lässt ihn zu.
- **Keine Basisprüfung beim Entscheid.** Der Beitrag sitzt auf seiner `basis`
  auf; ob sich die Zieldatei seither geändert hat, prüft der Entscheid nicht.
  Diese Prüfung gehört laut Bauplan zu Schritt 15.
- **Ein fremder Beitrag bleibt lesbar.** Wer nicht verantwortet, sieht den
  Entscheid, aber keine Handlungen; die Route antwortet ihm mit 403.
