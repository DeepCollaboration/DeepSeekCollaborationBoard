# Bericht Schritt 14 — Der Rücksprung steht am Vorhaben

- **Stand:** 2026-10-26
- **Gebaut:** Ein offener Rücksprung steht sichtbar am Vorhaben. `/board/data`
  liefert je Vorhaben `rueckkehr` samt Ziel- und Herkunftsnamen und Grund; die
  Seite zeigt nur die offenen Einträge (`erledigt: false`) mit Ziel und Grund und
  blendet erledigte aus, ohne sie aus der Datei zu nehmen. Das Zurückschicken
  selbst, der Eintrag `{ von, nach, grund, seit, erledigt: false }`, die
  Verantwortung an der Zielstation und das Erledigen beim Weiterziehen standen
  seit Schritt 06 und sind unverändert übernommen und geprüft. Neu sind die
  Anreicherung in `/board/data`, die Anzeige, das Prüfskript
  `produkt/test/schritt14.mjs` und die Fassung `Rücksprung`.
- **Nicht neu gestartet:** Das laufende Board arbeitet weiter mit dem geladenen
  alten Modul. Die angereicherte Antwort und die Anzeige erscheinen erst nach
  einem Neustart; geprüft ist alles an einer Modulkopie in `/tmp`.

## Der Vermerk am Vorhaben

Die Route `POST /board/vorhaben/zurueckschicken` bleibt, wie sie in Schritt 06
entstanden ist. Sie nimmt `{ id, nach, grund }` und prüft in dieser Reihenfolge:

1. das Vorhaben existiert (sonst 404),
2. `darf(vorgang, "zurueckschicken", PERSON_ID)` — dieselbe Prüfstelle wie
   `aendern`, `verschieben`, `weitergeben`, `entscheiden` (sonst 403),
3. die Zielstation steht im eigenen Workflow (sonst 400),
4. der Grund ist nicht leer (sonst 400).

Dann trägt sie am Vorhaben ein:

```json
{
  "von": "umsetzung",
  "nach": "vision",
  "grund": "Der Punkt aus der Vision ist noch offen.",
  "seit": "2026-10-26",
  "erledigt": false
}
```

und setzt `position` auf `nach`. `von` ist die Station, an der das Vorhaben
vorher stand, `seit` das Tagesdatum (`sv-SE`, also `JJJJ-MM-TT`), `erledigt`
zunächst `false`. Ist an der Zielstation noch niemand eingetragen, übernimmt sie
den bisherigen Verantwortlichen — dieselbe Regel wie beim Weitergeben; der alte
Eintrag an der verlassenen Station bleibt als Geschichte stehen.

Ein fremdes Vorhaben wird vor jedem Schreiben mit 403 abgewiesen. Die Prüfung
steht vor der Ziel- und Grundprüfung, damit ein Fremder nichts über die Ablage
erfährt.

## Erledigt beim Vorbeiziehen

Die Route `/board/vorhaben/verschieben` (sie trägt auch das Weitergeben) setzt
nach dem neuen Stand alle offenen Rücksprünge auf `erledigt: true`, deren
Zielstation **vor** der neuen Station liegt:

```js
for (const r of (Array.isArray(vorgang.rueckkehr) ? vorgang.rueckkehr : [])) {
  if (r.erledigt) continue;
  const nachIndex = liste.findIndex((p) => p.id === r.nach);
  if (nachIndex >= 0 && zielIndex > nachIndex) r.erledigt = true;
}
```

Der Eintrag wird nicht gelöscht: `rueckkehr` behält ihn mit allen Feldern, nur
`erledigt` wechselt auf `true`. Steht das Vorhaben genau an der Zielstation,
bleibt er offen; erst eine Station dahinter erledigt ihn. Wird die Zielstation
nicht mehr gefunden, bleibt der Eintrag offen, statt zu raten.

## `/board/data`

Jedes Vorhaben trägt weiter das rohe Feld `rueckkehr` aus der Datei. Beim Lesen
bekommt jeder gültige Eintrag die Namen seiner beiden Stationen; die Datei bleibt
die schmale Form aus Schritt 03:

```json
"rueckkehr": [
  {
    "von": "umsetzung",
    "nach": "vision",
    "grund": "Der Punkt aus der Vision ist noch offen.",
    "seit": "2026-10-26",
    "erledigt": false,
    "vonName": "Architektur zu Bauplan",
    "nachName": "Idee → Vision"
  }
]
```

`vonName` und `nachName` kommen aus `stationenVon` über alle Workflows, also auch
für eine Station der anderen Person; fehlt eine Station, bleibt ihre Kennung
stehen. `erledigt` wird zu einem echten Wahrheitswert normalisiert. Ungültige
Einträge (kein Objekt) werden beim Lesen übergangen, nicht in die Anzeige
gegeben. Geschrieben wird nichts: die Namen entstehen bei jeder Antwort neu.

## Die Seite

Am Vorhaben (im Chip an seiner Station) steht der offene Rücksprung jetzt
unabhängig davon, ob das Vorhaben ausgewählt ist:

```
↩ zurück nach „Idee → Vision“: „Der Punkt aus der Vision ist noch offen.“
   — von „Architektur zu Bauplan“, 2026-10-26
```

- `offen` wird aus `v.rueckkehr` mit `filter((r) => !r.erledigt)` gebildet; ein
  erledigter Eintrag erscheint nicht mehr, bleibt aber in der Datei.
- Ziel und Grund kommen fertig aus `/board/data` (`r.nachName`, `r.grund`);
  `nameVon` bleibt nur als Rückfall für ältere Antworten.
- Der Name des Vorhabens trägt weiter den Marker `↩`, solange mindestens ein
  Rücksprung offen ist.
- Am ausgewählten Vorhaben bleibt die Handlung „↩“ mit dem Formular (Zielstation,
  Grund) unverändert; die Meldung nach dem Zurückschicken nennt den Zielnamen.

Das Bild im Browser ist nicht geprüft; dafür wäre ein Neustart nötig. Belegt sind
die Bedingungen am Quelltext.

## Prüfskript (`produkt/test/schritt14.mjs`)

Das Skript kopiert Ablage und Produkt nach `/tmp`, setzt `person: "p1"` in der
Kopie, entfernt den Remote und legt nur in der Kopie an:

- ein eigenes Vorhaben `eigenes-14` an `umsetzung` mit `bauplan.md`,
  verantwortet von `p1`;
- eine zweite Person `p2`, ihren Workflow (`pruefen` → `freigeben`) und ein
  fremdes Vorhaben `von-p2` an `pruefen` mit `idee.md`.

Die Prüfvorhaben werden als eigene Fassung festgehalten; der fremde Ordner und
seine Zeitstempel werden vorher festgehalten. Danach prüft es:

1. **Zurückschicken mit Grund:** das Vorhaben steht an `vision`, die Datei trägt
   genau einen Eintrag `{ von: "umsetzung", nach: "vision", grund, seit,
   erledigt: false }`; `verantwortliche.vision` ist `["p1"]`, der Eintrag an
   `umsetzung` bleibt; die Fassung nennt `Thomas Heinrich` und
   „zurückgeschickt“;
2. **`/board/data`:** der Eintrag trägt `nachName: "Idee → Vision"`,
   `vonName: "Architektur zu Bauplan"`, den Grund und `erledigt: false`; die
   offene Liste enthält genau einen Eintrag;
3. **Fehler:** ohne Grund, mit leerem Grund und mit unbekannter Zielstation
   antwortet die Route 400; Bytes und Zeitstempel der Datei bleiben gleich;
4. **fremdes Vorhaben:** 403; die fremde `vorhaben.json`, der fremde Ordner, die
   Personendatei und der Git-Status des fremden Pfads bleiben unverändert;
5. **Weiterziehen über das Ziel:** nach `umsetzen` steht in der Datei
   `erledigt: true`, der Eintrag bleibt mit `von`, `nach`, `grund`; `/board/data`
   trägt ihn weiter, die offene Liste ist leer;
6. **Seite:** Quelltext enthält `!r.erledigt`, `r.nachName`, `r.grund` und
   „↩ zurück nach …“;
7. **echte Ablage:** kein `p2`, kein `von-p2`, kein `eigenes-14`, das echte
   Vorhaben steht weiter an `umsetzen` mit leerem `rueckkehr`.

Ausgabe des letzten Laufs:

```
ok Rücksprung: Vorhaben an „Idee → Vision“, Vermerk mit von, nach, Grund, seit, erledigt false; Verantwortung bleibt bei p1
ok 400: ohne Grund, mit leerem Grund und ohne vorhandene Zielstation — die Datei bleibt unverändert
ok 403: fremdes Vorhaben — keine Änderung an Datei, Ordner oder Person
ok erledigt: Weiterziehen über „Idee → Vision“ setzt erledigt true; der Eintrag bleibt in der Datei, die offene Anzeige ist leer
ok Seite: nur offene Rücksprünge, Ziel und Grund am Vorhaben
ok echte Ablage unberührt: kein Rücksprung, keine zweite Person, kein Prüfvorhaben
ok Prüfordner aufgeräumt: /tmp/board-schritt14-…
```

**Entscheidende Zeile:** `ok Rücksprung: Vorhaben an „Idee → Vision“, Vermerk mit
von, nach, Grund, seit, erledigt false; Verantwortung bleibt bei p1` — der Grund
steht am Vorhaben. Dazu `ok erledigt: Weiterziehen über „Idee → Vision“ setzt
erledigt true; der Eintrag bleibt in der Datei, die offene Anzeige ist leer` —
der Vermerk verschwindet nur aus der Anzeige, nicht aus der Ablage.

## Dateien

- `produkt/index.js` — `rueckkehrVon` in `datenAntwort` (1443–1465), gesetzt in
  1481; die Namen kommen aus `stationenVon` (1310–1325). Unverändert übernommen:
  `zurueckschicken` (3180–3233; `darf` 3194, Ziel 3198, Grund 3201, Eintrag
  3216–3224) und das Erledigen in der Verschiebe-Route (3160–3165).
- `produkt/client.js` — offene Liste (3147); Anzeige mit Ziel und Grund
  (3256–3270).
- `produkt/test/schritt14.mjs` — neu, 505 Zeilen.
- `arbeit/fortschritt.md`, `arbeit/plan.md`, `arbeit/pruefung.md` — Runde 14,
  Status, Prüfvermerk.
- `arbeit/schritte/14-bericht.md` — dieser Bericht.

## Prüfungen

| Nr | Befehl | Ergebnis | Exit |
| --- | --- | --- | --- |
| 1 | `node …/produkt/test/schritt14.mjs` | sieben ok-Zeilen: Rücksprung, 400er, 403, erledigt, Seite, echte Ablage, aufgeräumt | 0 |
| 2 | `node …/produkt/test/schritt13.mjs` | Herkunft unverändert | 0 |
| 3 | `node …/produkt/test/schritt06.mjs` | Verantwortliche und 403 unverändert | 0 |
| 4 | `node …/produkt/test/schritt03.mjs` | Ablage und Umzug unverändert | 0 |
| 5 | `node …/produkt/test/schritt02.mjs`, `04`–`12` | alle neun grün | 0 |
| 6 | `node --check produkt/index.js`, `client.js`, `test/schritt14.mjs` | keine Syntaxfehler | 0 |
| 7 | `git -C /workspace/projekte/board status --porcelain` | leer nach der Fassung | 0 |
| 8 | `git -C /workspace/projekte/board log -1 --format='%an %s'` | `Thomas Heinrich Rücksprung` | 0 |

## Entscheidungen

- **Die Namen entstehen beim Lesen, nicht in der Datei.** `rueckkehr` bleibt die
  Form `{ von, nach, grund, seit, erledigt }` aus Schritt 03. `vonName` und
  `nachName` stehen nur in der Antwort an die Seite; so muss sie nichts
  nachschlagen, und die Ablage bleibt zwischen zwei Installationen schmal.
- **Nur `erledigt` entscheidet über die Anzeige.** Es gibt kein zusätzliches Feld
  `offen`; Seite und Prüfskript filtern auf `!r.erledigt`. Damit ist die Datei die
  einzige Wahrheit, und ein erledigter Eintrag kann nicht durch eine zweite
  Ableitung wieder auftauchen.
- **Der Eintrag bleibt stehen.** Erledigen heißt markieren, nicht löschen. Die
  Geschichte des Vorhabens bleibt lesbar, auch wenn die Anzeige den Vermerk
  ausblendet.
- **Die Route bleibt eine Route.** Zurückschicken ist ein Weitergeben an eine
  frühere Station; es gibt keine zweite Route und keine neue Aktion. Die
  bestehende Prüfstelle `darf(…, "zurueckschicken")` genügt.
- **Kein Urteil über „früher“ in der Route.** Sie verlangt, wie der Schritt es
  sagt, einen Grund und eine vorhandene Zielstation. Die Zielstation muss nicht
  kleiner indiziert sein als die jetzige; das Erledigen hängt allein an der
  neuen Station hinter dem Ziel.
- **Die Anzeige steht am Vorhaben, nicht erst im Detail.** Der offene Grund ist
  der Grund, warum das Vorhaben zurückkam; er soll sichtbar sein, ohne dass man
  das Vorhaben erst auswählt.

## Fassung

Der Stand der Ablage ist als Fassung `Rücksprung` festgehalten, Urheber
`Thomas Heinrich` (`thomas.heinrich@board`). Sie enthält den Schritt 14, diesen
Bericht und die Arbeitsstände in `arbeit/`, die beim Schritt im Arbeitsbaum
lagen. Die Änderungen am Produkt (`produkt/index.js`, `produkt/client.js`,
`produkt/test/schritt14.mjs`) sind wie in den Schritten davor nicht Teil der
Board-Fassung: `vorgaenge/*/produkt/` ist im Repository ignoriert.

## Grenzen und offene Punkte

- **Nicht neu gestartet.** Das laufende Board antwortet weiter mit dem alten
  Modul; die Anzeige und `nachName`/`vonName` in `/board/data` erscheinen erst
  nach einem Neustart.
- **Im Browser nicht gesehen.** Die Seite ist syntaktisch geprüft und ihre
  Bedingungen am Quelltext belegt; das tatsächliche Bild ist nicht geprüft, weil
  dafür ein Neustart nötig wäre.
- **Kein echter Rücksprung in der Ablage.** `eigenes-14` und der Rücksprung leben
  nur in der Kopie in `/tmp`. Die echte Ablage hat weiter fünf Vorhaben, eine
  Person und leere `rueckkehr`-Listen.
- **Die Anzeige ist eine Zeile, kein eigener Bereich.** Bei mehreren offenen
  Rücksprüngen stehen sie untereinander; ein erledigter verschwindet sofort.
  Eine eigene Historie der Rücksprünge gibt es nicht — dafür ist die Datei da.
- **Das Formular bietet alle Stationen an.** Die Handlung „↩“ listet die
  Stationen des eigenen Workflows, nicht nur die früheren. Die Route prüft die
  Zielstation, nicht ihre Lage; ein Vorhaben lässt sich damit auch vorwärts
  „zurückschicken“, was als Rücksprung offen bliebe. Das ist die bestehende
  Bedienung aus Schritt 06 und nicht Teil dieses Schritts.
