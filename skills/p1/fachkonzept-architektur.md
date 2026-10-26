---
name: p1-fachkonzept-architektur
description: Entscheidet aus Fachkonzept, Vision und Notizen die technische Architektur und schreibt architektur.md — Techstack, Bausteine, Schnittstellen, Betrieb. Kein Interview, Rückfrage nur bei einer wesentlichen Weggabelung.
whenToUse: Wenn ein Vorhaben an der Position „Fachkonzept → Architektur" steht und die technische Bauweise festgelegt werden soll.
---

# Architekturentscheidungen

Hier wird nicht mehr gefragt, hier wird entschieden. Das Gespräch über das
Produkt ist vorbei. Was jetzt kommt, ist Recherche, Abwägung und eine begründete
Wahl — mit ihm als Auftraggeber, nicht als Auskunftsperson.

## Was die Architektur ist

Das **finale Produkt, technisch beschrieben**. Ein Zustand, kein Weg: So sieht es
aus, wenn es fertig ist — aus welchen Teilen, auf welcher Technologie, über welche
Schnittstellen, mit welchem Betrieb.

Die Architektur ist die Zeichnung. Der Bauablauf ist der Weg dorthin und steht
nicht hier, sondern im Umsetzungsplan.

## Was hier nicht hingehört

- **Keine Reihenfolge des Baus.** Keine „erste Scheibe", keine Bauabschnitte, keine
  Schritte. Das ist der Umsetzungsplan.
- **Keine vertikale Zerlegung.** Wie das Produkt in Scheiben wächst, entscheidet
  der Plan, nicht die Architektur.
- **Kein Zeitplan.** Keine Dauer, keine Reihenfolge, keine Meilensteine.
- **Keine Arbeitsanweisung.** Kein „zuerst dies, dann das".

Ein Abschnitt „Reihenfolge des Baus" in einer Architektur ist ein Fehler: Er
veraltet, sobald der erste Schritt anders läuft, und er macht die Architektur zu
einem halben Plan.

## Warum kein Interview

Er kann zu Architekturentscheidungen keine Auskunft geben, ohne dass ich ihn
ausfrage, was er nicht weiß. Wer nach der Bauweise fragt, bekommt Vermutungen.
Also entscheide ich die Bauweise und lege die Folgen dar.

Seine Rolle hier: die Folgen beurteilen. Meine: die Mittel wählen.

## Eingang

- `fachkonzept.md` — was das Ding können muss.
- `vision.md` — was gelten soll, inklusive Werte.
- `fachkonzept-notizen.md` und `vision-notizen.md` — **Pflichtlektüre.**

Die Notizen sind der eigentliche Grund für diesen Schritt: dort stehen seine
Aussagen zu Werkzeugen und Bauweise, die nie ins Fachkonzept gehörten. „Muss open
source und dezentral sein", „Matrix fand ich gut", „läuft auf meinem Homeserver".
Jede davon, die die Architektur berührt, wird hier aufgenommen, geprüft und
entschieden. Nichts davon fällt stillschweigend unter den Tisch.

## Was am Ende da ist

`architektur.md` im Ordner des Vorhabens — `/workspace/projekte/board/vorgaenge/<ordner>/`,
der Ordner des Vorhabens unter `/workspace/projekte/board/vorgaenge/<ordner>/`:

```markdown
---
etage: Architektur
status: in Arbeit
offene_fragen: 0
stand: JJJJ-MM-TT
---

# Architektur

## Das Produkt
## Techstack
## Bausteine
## Schnittstellen
## Daten und Besitz
## Rechte und Sicherheit
## Ausfälle und Betrieb
## Verworfene Wege
## Annahmen
```

## Der Techstack

Der wichtigste Abschnitt. Er muss so konkret sein, dass ein Fremder ihn umsetzen
könnte, ohne eine Technologie zu wählen. Als Tabelle:

| Teil | Wahl | Grund | Verworfen |
| --- | --- | --- | --- |
| Sprache | z. B. Python 3.12 | ... | ... |
| Laufzeit | z. B. Node 22 | ... | ... |
| Framework | ... | ... | ... |
| Bibliotheken | ... | ... | ... |
| Datenhaltung | z. B. SQLite-Datei | ... | ... |
| Protokolle | z. B. HTTP, Git | ... | ... |
| Betrieb | z. B. ein Prozess, kein Port | ... | ... |

Regeln dafür:

- **Eine Wahl, nicht ein Menü.** „SQLite oder Postgres" ist keine Entscheidung.
- **Konkret, nicht allgemein.** „SQLite 3, eine Datei im Vorhabenordner" statt
  „eine Datenbank".
- **Jede Wahl mit Grund und verworfener Alternative.** Ohne verworfene
  Alternative ist es keine Entscheidung, sondern ein Einfall.
- **Nur, was das Fachkonzept verlangt.** Keine Bibliothek auf Vorrat.
- **Was er nicht kennt, wird erklärt** — in einem Satz, ohne Vorlesung.

## Bausteine und Schnittstellen

Die Bausteine sind die Teile des fertigen Produkts, nicht die Schritte zu ihm.
Jeder Baustein: was er tut, wovon er abhängt, was er nach außen gibt.

Die Schnittstellen sind die Übergänge zwischen Bausteinen und zur Außenwelt:
Dateien, Protokolle, Adressen. Namen und Formen nennen, nicht nur „API".

## Wie entschieden wird

1. Anforderungen aus dem Fachkonzept sammeln, die die Bauweise berühren.
2. Seine Aussagen aus den Notizen dazustellen — wörtlich, nicht umgedeutet.
3. **Den Techstack entscheiden:** Sprache, Laufzeit, Framework, Bibliotheken,
   Datenhaltung, Protokolle, Betrieb. Je Punkt zwei oder drei übliche Wege
   recherchieren, wählen, begründen.
4. Die Bausteine und Schnittstellen daraus ableiten.
5. Bewerten an dem, was ihm wichtig ist: offen, selbst gehostet, nicht kommerziell,
   Datenhoheit, wenig Betriebsaufwand, keine Abhängigkeit von einem Anbieter.
6. Wählen, begründen, und die Kosten benennen, die die Wahl hat.
7. Nicht gewählte Wege mit dem Grund der Ablehnung notieren.

## Wann gefragt werden darf

Nur bei einer Weggabelung, die aus Fachkonzept und Vision nicht zu entscheiden
ist **und** die später teuer zu ändern wäre.

Das gilt besonders für den Techstack: Wenn zwei Wege gleich gut passen und die
Wahl seine Umgebung betrifft — etwa eine Sprache, die er selbst pflegen will —,
dann ist das eine Frage, keine Entscheidung.

Dann so — Wege mit Folge, Empfehlung, eine Frage:

> Du hast Matrix erwähnt; es gibt auch XMPP. Matrix heißt: …, kostet: …. XMPP
> heißt: …, kostet: …. Ich würde … nehmen, weil …. Spricht etwas dagegen?

Kein Fragenkatalog, keine Themenrunde, kein Nachbohren. Zwei, drei solcher
Rückfragen sind viel. Möglich — und völlig normal — ist auch, dass gar keine nötig
ist; dann läuft der Skill ohne eine einzige Rückfrage durch.

## Wenn das Vorhaben zurückkommt

Es kann aus der Umsetzung zurückgeschickt worden sein — etwa weil der Techstack
fehlt oder eine Schnittstelle nicht bestimmt ist. Dann steht am Vorhaben ein
Rücksprung mit Grund. In diesem Fall:

- Nur diesen einen Punkt behandeln. Der Rest der Architektur steht schon.
- Den Punkt in `architektur.md` nachtragen, an der Stelle, an die er gehört.
- Nicht die ganze Datei neu schreiben und nicht von vorn anfangen.

## Grenzen

- Keine Technikschule. Folgen, nicht Vorlesung.
- Nichts bauen, was das Fachkonzept nicht verlangt.
- Keine Wahl ohne verworfene Alternative.
- Kein Bauablauf. Keine Reihenfolge, keine Scheiben, keine Schritte.
- Was offen bleibt: als Annahme eingetragen, mit Datum und dem Satz, was passiert,
  wenn sie falsch ist.

## Abschluss

Fertig, wenn `architektur.md` geschrieben ist, der Techstack vollständig und
konkret ist, jeder Baustein seine Schnittstellen hat, jede Entscheidung eine
Begründung und eine verworfene Alternative hat und er die Folgen freigegeben hat.

## Sitzung im Board festhalten

Wenn das Board läuft, am Ende der Sitzung — auch ohne Freigabe:

```bash
curl -s -X POST "$DSH_WEB_URL/board/skill/lauf" \
  -H 'content-type: application/json' \
  -d "{\"vorhaben\":\"<vorhaben-id>\",\"position\":\"<position-id>\",\"session\":\"$DSH_SESSION_ID\"}"
```

Die Kennungen stehen in `vorgaenge/<ordner>/vorhaben.json` und
`workflows/<person>.json`. Das Transkript
entsteht aus dieser Sitzung: Fragen und Antworten, ohne Werkzeuge und Recherche.
Scheitert der Aufruf, nicht anhalten — dann fehlt am Schritt das Transkript,
und das ist im Board sichtbar.
