---
name: p1-umsetzen
description: Setzt den Bauplan um — ein Schritt je Runde, ein Arbeiter je Schritt, ein frischer Prüfer je Schritt. Ohne Rückfrage.
whenToUse: Wenn ein Vorhaben an der Station „Umsetzen" steht und der Bauplan ausgeführt werden soll.
---

# Umsetzen

Ein Bauauftrag, kein Gespräch. Die Architektur steht, die Fragen sind gestellt.
Jetzt wird gebaut — in Schritten, die man prüfen kann, ohne den Überblick zu
verlieren.

## Warum so

Ein Agent verliert bei langen Läufen nicht den Text, sondern den Faden: welche
Entscheidung getroffen wurde, was schon läuft, was als Nächstes kommt. Dagegen
hilft nicht mehr Kontext, sondern weniger: ein Schritt je Runde, ein Arbeiter mit
frischem Blick, ein Prüfer ohne Baugeschichte, ein Zustand in Dateien.

Die drei Rollen sind Kontexttrennung, keine Hierarchie:

- **Bauleitung** (du): liest Architektur und Berichte, schneidet, entscheidet,
  schreibt den Zustand. Liest keinen Produktcode.
- **Arbeiter**: ein Subagent je Schritt. Frischer Kontext, nur sein Auftrag und
  seine Dateien.
- **Prüfer**: ein Subagent je Schritt. Frischer Kontext, sieht Anforderung und
  Ergebnis, nicht den Weg dorthin.

**Ein Schreiber zur Zeit.** Parallele Arbeiter treffen widersprüchliche
Entscheidungen; das ist der häufigste Grund, warum große Läufe unbrauchbar werden.
Parallel erlaubt sind nur Leser: Prüfer, Recherchen, Suchen.

## Eingang

Im Ordner des Vorhabens, `/workspace/projekte/board/vorgaenge/<ordner>/`:

- `architektur.md` — der Auftrag. Er wird nicht verändert.
- `fachkonzept.md`, `vision.md` — für das Verständnis, nicht für den Plan.
- `fachkonzept-notizen.md`, `vision-notizen.md` — oft steht dort der Grund für
  eine Reihenfolge.
- `bauplan.md` — wenn vorhanden, ist sie der Plan. Ihre Scheiben und Schritte
  werden übernommen, nicht neu erfunden. Fehlt sie, schneidest du selbst.

Vorhaben finden: Nennt der Auftrag einen Ordner, nimm ihn. Sonst das Vorhaben,
dessen `laeufe[].sitzung` gleich `$DSH_SESSION_ID` ist — zu finden in
`vorgaenge/<ordner>/vorhaben.json`; sonst der Vorgang an der
Position `architektur-umsetzung`. Findest du keinen, arbeite im aktuellen Ordner
und frage nicht.

Fehlt `architektur.md` oder ist sie leer: nicht raten. Bericht schreiben, Station
ohne Produkt beenden.

Prüfe, ob schon Arbeit existiert: `produkt/` oder `arbeit/`. Wenn ja, wird
fortgesetzt, nicht neu begonnen.

## Was am Ende da ist

```text
produkt/                        das Produkt selbst
arbeit/plan.md                  Scheiben, Schritte, Status, Prüfkriterium
arbeit/schritte/NN.md           der Auftrag je Schritt
arbeit/schritte/NN-bericht.md   was der Arbeiter getan hat
arbeit/fortschritt.md           Runde, Stand, nächster Schritt, Blocker
arbeit/entscheidungen.md        Entscheidungen mit Grund und Folge
arbeit/pruefung.md              Prüfprotokoll: Urteil, Befehl, Exit-Code, Zeile
umsetzung.md       der Bericht am Ende
```

## Wer was liest

Das ist die wichtigste Tabelle des Skills. Wer mehr liest, verliert den Faden.

| Datei | Bauleitung | Arbeiter | Prüfer |
| --- | --- | --- | --- |
| `architektur.md` | einmal am Anfang | nein | bei der Konvergenz |
| `arbeit/plan.md` | ja, offene Schritte | nein | nein |
| `arbeit/schritte/NN.md` | schreibt | ja, nur seinen | ja, als Anforderung |
| `arbeit/schritte/NN-bericht.md` | ja | schreibt | **nein** |
| `produkt/` | nein | ja, seinen Teil | ja |
| `arbeit/fortschritt.md` | ja | nein | nein |

Der Prüfer bekommt den Bericht des Arbeiters **nicht**. Sonst prüft er die
Erzählung statt das Ergebnis.

## Der Schnitt

Ein Schritt ist die kleinste Einheit, die

- ein sichtbares Ergebnis hat,
- ein Arbeiter allein mit Dateien erledigen kann,
- ein Prüfer ohne Vorwissen beurteilen kann.

Nicht: die in ein Kontextfenster passt. Die Fenstergröße ist nicht der Maßstab;
der frische Blick ist es. Ein Schritt, der in ein Fenster passt und trotzdem
unprüfbar ist, ist zu groß.

- Zwei Ergebnisse sind zwei Schritte.
- Kein Umbau nebenbei. Aufräumen ist ein eigener Schritt oder keiner.
- Risiko früh: Was am ehesten schiefgeht, kommt nach vorn, solange es billig ist.
- Nichts auf Vorrat: kein Baustein, der erst drei Schritte später gebraucht wird.
- Die erste Scheibe ist der kleinste vollständige Weg, den ein Mensch benutzen
  kann — kein Grundgerüst, keine Datenbank zuerst.
- Nach jedem Schritt bleibt das Vorhandene benutzbar.

## Ablauf

### 1. Lesen und schneiden

Architektur, Fachkonzept und Notizen lesen. Existiert `bauplan.md`, übernimm
ihre Scheiben und Schritte; sie sind der Plan. Sonst schneidest du selbst.
Widersprüche und Lücken notieren — nicht fragen, sondern als Annahme in
`arbeit/entscheidungen.md` festhalten.

Dann Scheiben schneiden (vertikal, jede benutzbar) und je Scheibe die Schritte.
`arbeit/plan.md` anlegen:

```markdown
# Plan

## Scheiben
1. <Name> — <was danach benutzbar ist>

## Schritte

| Nr | Schritt | Ergebnis | Prüfung | Status |
| --- | --- | --- | --- | --- |
| 01 | ... | ... | ... | offen |
```

Status: `offen`, `läuft`, `bestanden`, `zurückgestellt`.

### 2. Ziel anlegen

`create_goal` mit der Zielsetzung und einer Rundenzahl, die aus der Zahl der
Schritte folgt:

```text
max_goal_rounds = min(240, max(20, 3 × Anzahl Schritte))
```

Drei Runden je Schritt decken Bauer, Prüfer und eine Nachbesserung. Die Zahl ist
keine Schätzung der Dauer, sondern eine Obergrenze für den Aufwand.

Das Ziel wird **einmal** angelegt, im ersten Zug nach dem menschlichen Auftrag.
In einer Goal-Runde lässt es sich nicht neu anlegen — dort ist nur noch
`update_goal` mit `complete` oder `blocked` erlaubt. `blocked` erst nach drei
erfolglosen Runden.

### 3. Runde für Runde

Jede Runde beginnt gleich. In der ersten Runde fehlen die Dateien noch; dann
werden sie angelegt.

1. `pwd`, `cat arbeit/fortschritt.md`, `cat arbeit/plan.md` (nur offene Zeilen).
2. Prüfen, ob das Produkt noch startet — sobald es einen Startbefehl gibt.
   Ist etwas gebrochen, wird zuerst repariert.
3. Den Schritt wählen, der die meisten anderen blockiert.
4. Den Auftrag schreiben: `arbeit/schritte/NN.md` (Vorlage unten).
5. Arbeiter starten: `subagent` mit `description` und `prompt`. Nicht
   `subagent_fork` — der erbt deinen Kontext und damit deine blinden Flecken.
6. Auf den Bericht warten. Dann Prüfer starten, ebenfalls `subagent`.
7. Urteil: **bestanden** → Schritt in `plan.md` abhaken, `fortschritt.md`
   fortschreiben. **nicht bestanden** → Befund in den Auftrag, zweiter Versuch.
   Nach zwei Fehlversuchen wird der Schritt kleiner geschnitten, nicht wiederholt.
8. Runde in `fortschritt.md` fortschreiben.

Eine Runde endet immer mit einem aktualisierten Zustand. Auch eine gescheiterte.

### 4. Konvergieren

Nach dem letzten Schritt: ein frischer Prüfer, der das Ganze gegen die Architektur
hält, nicht gegen die Schrittliste. Sein Auftrag:

> Hier ist die Architektur, hier ist das Produkt. Prüfe, ob das Produkt die
> Architektur erfüllt. Nenne jede Abweichung mit Ort und Grund. Kein Urteil über
> Qualität, die nicht in der Architektur steht.

Abweichungen werden behoben oder mit Grund in `arbeit/entscheidungen.md`
festgehalten und im Bericht genannt.

### 5. Bericht

`umsetzung.md` im Vorhabenordner, Statuskopf nach
`/workspace/wissen/vorlagen/statuskopf.md`:

```markdown
# Umsetzung

## Die Architektur
## Was entstanden ist
## Start
## Was es kann
## Was es nicht kann
## Ungeprüft
## Wichtigste Entscheidungen
## Wie geprüft wurde
## Abweichungen von der Architektur
## Dateien
```

Der Bericht wird ab der ersten lauffähigen Fassung mitgeschrieben, nicht erst am
Ende. Wird der Lauf vorher gestoppt, ist er trotzdem aktuell.

## Der Auftrag an den Arbeiter

`arbeit/schritte/NN.md` ist die einzige Quelle. Der Arbeiter liest sie und die
dort genannten Dateien. Sonst nichts.

```markdown
# Schritt NN — <Name>

- **Ziel:** ein Satz.
- **Eingang:** welche Dateien er liest, mit Pfad.
- **Tun:** was geschieht, in Stichworten.
- **Ergebnis:** was danach existiert, mit Pfad.
- **Prüfung:** der Befehl, mit dem man es prüft, und was dabei herauskommen muss.
- **Grenzen:** nur im Produktordner; keine anderen Dateien ändern; keine Rückfrage.

Am Ende: kurzer Bericht nach `arbeit/schritte/NN-bericht.md` — was gebaut wurde,
welche Dateien geändert wurden, welcher Befehl lief, welcher Exit-Code, welche
entscheidende Zeile. Keine Zusammenfassung des Wegs.
```

Der Arbeiter sieht dein Gespräch nicht. Jeder Pfad, den er braucht, steht im
Auftrag. Der `prompt` an den Subagenten ist kurz und verweist auf die Datei:

> Du baust genau einen Schritt. Lies `arbeit/schritte/NN.md` und die dort genannten
> Dateien. Folge dem Auftrag wörtlich. Ändere nichts außerhalb des Produktordners.
> Keine Rückfrage. Schreibe am Ende den Bericht nach
> `arbeit/schritte/NN-bericht.md` und antworte mit drei Zeilen: was gebaut, welcher
> Befehl, welcher Exit-Code.

## Der Auftrag an den Prüfer

Der Prüfer sieht die Anforderung und das Ergebnis. Nicht den Bericht des Arbeiters.

> Du hast diesen Schritt nicht gebaut. Lies `arbeit/schritte/NN.md` (die
> Anforderung) und prüfe das Ergebnis in `<Pfad>` gegen genau diese Anforderung.
> Führe den dort genannten Prüfbefehl aus. Urteil: **bestanden** oder **nicht
> bestanden**. Bei nicht bestanden: der Reproduktionsbefehl und die entscheidende
> Zeile. Keine Note, keine Reparatur, keine Vorschläge.

Das Urteil kommt in `arbeit/pruefung.md`:

```markdown
## Schritt NN — bestanden|nicht bestanden
- Befehl:
- Exit-Code:
- Entscheidende Zeile:
- Befund:
```

## Wenn es klemmt

- **Zwei Versuche am selben Schritt:** kleiner schneiden. Nicht denselben Auftrag
  wiederholen.
- **Derselbe Werkzeugaufruf dreimal identisch:** Ansatz wechseln, nicht wiederholen.
- **Arbeiter hängt oder dreht sich:** `interrupt_agent`, Auftrag halbieren, neu
  starten.
- **Der Arbeiter behauptet Erfolg ohne Beleg:** Prüfer entscheidet, nicht der
  Arbeiter. Fehlt der Beleg, ist der Schritt nicht bestanden.
- **Dein eigener Kontext wächst:** du liest keine Produktdateien. Nur Berichte,
  Urteile, `plan.md` und `fortschritt.md`. Bei Bedarf `grep` statt `read`.
- **Die Architektur reicht nicht:** als Annahme dokumentieren und im Bericht als
  Abweichung führen. Die Architektur wird hier nicht umgebaut.

## Grenzen

- Nur im Vorhabenordner arbeiten; `/tmp` ist erlaubt. Nichts außerhalb ändern.
- Nichts löschen, was nicht in diesem Lauf entstanden ist. `architektur.md`,
  `fachkonzept.md` und die Dateien anderer Vorhaben sind unantastbar.
- Nichts veröffentlichen, hochladen, versenden; keine Konten, keine Käufe.
- Keine Rückfrage, kein `ask_user_question`, kein Warten auf Freigabe.
- Keine eigenen Commits; der Arbeitsbereich wird ohnehin gesichert.
- Das Board nicht ändern.
- Ein Schreiber zur Zeit.

## Fertig

Fertig ist es, wenn alle Schritte `bestanden` sind **und**:

1. Der Startbefehl aus einem sauberen Zustand funktioniert.
2. Die Kernwege als Nutzer durchgespielt sind, mit Belegen.
3. Die Konvergenzprüfung gelaufen ist und jede Abweichung erklärt ist.
4. Ein frischer Prüfer keine offenen Blocker gefunden hat.
5. Der Bericht geschrieben ist und nichts beschönigt.

Dann `get_goal`, `id` und `revision` kopieren, `update_goal complete` setzen, ihm
das Ergebnis nennen und den Bericht mit `present` zeigen.

Wird es nicht fertig: Bericht schreiben, den Stand ehrlich benennen, `get_goal`
lesen, dann `update_goal blocked` mit dem konkreten Grund. Kein „fertig", das nicht belegt ist.

## Wiederaufnahme

Wird die Sitzung fortgesetzt oder neu geöffnet:

- `arbeit/fortschritt.md` und `arbeit/plan.md` lesen; dort steht der Stand.
- Existiert ein Ziel und ist es entwaffnet, läuft nichts von allein; der Lauf wird
  erst nach einem menschlichen Zuruf fortgesetzt.
- Ist der Plan da, aber kein Ziel: auf den Auftrag warten, dann `create_goal` und
  weiterbauen.
- Nichts neu erfinden, was schon in `produkt/` steht. Erst prüfen, was da ist.

## Sitzung im Board festhalten

Wenn das Board läuft, am Ende — auch ohne Freigabe:

```bash
curl -s -X POST "$DSH_WEB_URL/board/skill/lauf" \
  -H 'content-type: application/json' \
  -d "{\"vorhaben\":\"<vorhaben-id>\",\"position\":\"umsetzen\",\"session\":\"$DSH_SESSION_ID\"}"
```

Die Kennungen stehen in `vorgaenge/<ordner>/vorhaben.json` und
`workflows/<person>.json`. Scheitert der
Aufruf, nicht anhalten — dann fehlt am Schritt das Transkript, und das ist im
Board sichtbar.

## Woher die Regeln kommen

Die Begründungen stehen ausführlich in `/workspace/skill-lange-bauten.md`.
Kurz:

- **Faden statt Text:** Die effektive Kontextlänge hängt an der Aufgabe, nicht am
  Fenster. Deshalb Dateien als Gedächtnis und ein frischer Blick je Schritt.
- **Ein Schreiber:** Parallele Schreiber treffen widersprüchliche Entscheidungen.
  Zusätzliche Agenten liefern Intelligenz, keine Aktionen.
- **Frischer Prüfer:** Ein Prüfer ohne Baugeschichte findet mehr als einer, der
  den Weg kennt.
- **Kleiner schneiden statt wiederholen:** Zerlegung nach Bedarf schlägt starre
  Wiederholung.
- **Konvergieren:** Nach dem Bauen wird gegen die Architektur geprüft, nicht gegen
  die eigene Schrittliste.
