---
etage: Vision
status: in Arbeit
offene_fragen: 1
stand: 2026-10-25
---

# Vision — Notizen

Alles, was nicht in die Vision gehört: Mittel, Ideen, Wünsche, Zweifel, Erinnerungen. Nichts davon ist entschieden. Das Architekturentscheidungs-Gespräch lebt später davon.

## Was heute schon da ist

Stand 2026-10-25: Die erste Fassung des Boards läuft als eigenes Plugin (`plugins/dsh-board`) im Harness, erreichbar über „Board" in der Seitenleiste.

Vorhanden: Spalten als Positionen, Vorhaben an ihrer Position, Klick auf ein Vorhaben zeigt die Dateien, die es bis dahin wirklich gibt · der Skill des Schritts mit Transkript · Leser für Markdown mit Bearbeiten · Gesprächsspalte rechts, von dort startet der Skill · Ziehen mit Rückfrage, Pfeil weiter wird grün, sobald der Ausgang vorliegt · Positionen einfügen und löschen · Vorhaben anlegen und umbenennen · Idee von Hand schreiben · Eingang, Ausgang und Skill aus Listen wählen · ein Workflow (`board.json`) mit acht Positionen.

Noch nicht gebaut, laut Minikonzept vom selben Tag: Fragen und Antworten je Position · der Schalter · Varianten und fremde Positionen in der Oberfläche · Skripte und Modellprüfungen · Verzweigungen · mehrere Workflows nebeneinander · der Rücksprung.

**Das Entscheidende daran:** Die laufende Fassung ist Ein-Personen-Betrieb. Der Kern, den er vermisst, ist genau dieser: mehrere Menschen am selben Board.

## Der Kern als Bauauftrag

- Mehrere **Accounts**, mehrere Menschen.
- Gegenseitig die Sachen und den Status sehen.
- An den jeweiligen Projekten teilnehmen: kommentieren, weiterarbeiten — je nach Freigabe.
- Der erste Beweis: ein fremder Teilbereich auf der Übersicht, mit Namen, anklickbar, ein Beitrag möglich.

## Zwei Workflows, teils gleich, teils eigen

Die Erweiterung, die ihn am meisten reizt:

- Alex kann **eigene Positionen** und **eigene Skills** in den Workflow bringen.
- Dieselbe Stelle im Ablauf kann bei ihm ein Grillme-Skill sein und bei Alex ein anderer, den er besser findet — und er sieht, welchen Alex benutzt.
- Auch ein **komplett anderer Workflow** von Alex bleibt sichtbar.
- Das Board wird damit eine Übersicht über **mehrere Workflows auf derselben Ebene**, deren Punkte sich überschneiden können oder nicht: einzelne Punkte identisch verwendet, andere individuell.
- Das ist für ihn der spannende Teil, nicht der Gleichschritt.

Verwandt, aber älter: Im Interview vom selben Tag stand bei fremden Beiträgen der Gedanke eines **Pull Requests** — beitragen ja, annehmen entscheidet der andere. Das war ein Widerspruch zur „Freigabe", die ohne Bestätigung durchläuft. Aufgelöst wurde er nicht durch eine Entscheidung, sondern durch einen Schalter: beides möglich, je Punkt einstellbar.

## Treiber und Rechte

- Sein Wort „Treiber" heißt: verantwortlich, entscheidet was eingebaut wird und wann der Punkt fertig ist.
- Der Treiber wird **je Schritt** gesetzt und kann über den Workflow hinweg wechseln.
- Der Grund ist Kompetenz, nicht Rang — daraus folgt ein Muster: Vision bleibt bei ihm, Architektur bei Alex.
- Die Einstellung sitzt nicht nur am Projekt oder Teilbereich, sondern auch an den einzelnen Punkten. (Er hat das ausdrücklich korrigiert.)

## Offene Punkte, die hier liegen bleiben

- **Eigentümer der Dateien.** Er konnte es nicht beantworten, weil es am dezentralen Modell hängt: Wo liegen die Dateien, auf die mehrere zugreifen? Gibt es überhaupt einen Eigentümer, oder ist es ein Server? Grundsätzlich zu klären, nicht hier.
- **Farbe.** Er will farbliche Unterscheidung der Bereiche/Projekte auch dann, wenn nichts ausgewählt ist — je nach Person. Heute färbt die Auswahl ein Vorhaben ein. Offen: Farbe nach dem, der gerade verantwortlich ist, oder nach dem, der angelegt hat. Beides stand in seinem Satz.
- **Reichweite.** Zivilgesellschaftliche Projekte und andere Vorhaben, über Organisationen hinweg — die Vision nennt es, das Fachkonzept muss es tragen.
- **Menschenzahl.** Keine technische Obergrenze. Vorläufig zwei.

## Ausbau, nicht Kern

- **Grillme-Sitzung in der Seitenleiste**, statt für die Sitzung in die Übersicht zum Chat zu müssen. Ausdrücklich als „schönermachen" eingeordnet, nicht als Kern.
- Oberfläche: grafisch schön und übersichtlich. Rückfragen und Hinweise, damit klar ist, was man gerade einrichtet — besonders beim Erteilen einer Erlaubnis an jemanden.

## Nicht wieder aufbringen

- **„Geschäftsmodell"** war so nicht gemeint: Es geht um ein Organisationsmodell, nicht ums Geldverdienen. Er hat gesagt, dass das nicht wichtig ist und nicht weiter verfolgt werden soll. Es war „ein bisschen ein Scherz". Steht nur hier, damit es nicht wieder auftaucht.
- **Die Frage „Woran würdest du merken, dass es nicht deins ist?"** konnte er nicht einordnen. Die erste Lesart — das Board ist nicht so geworden, wie er es wollte — gefiel ihm nicht und ist nicht beantwortbar. Die zweite Lesart führte zur Farbe. Die Frage wird nicht wiederholt.

## Form und Ton, die er sich wünscht

- Ein Satz, der oben steht, muss sitzen: knapp, ohne Fachjargon.
- Keine Wiederholung von schon Gesagtem.
- Widersprüche sollen stehen bleiben, nicht geglättet werden.

## Offene Fragen

1. **2026-10-25, an ihn: Farbe nach Verantwortlichem oder nach Anlegendem?** Blockiert nur die Farbregel, nicht das Fachkonzept.
2. **2026-10-25, an das Architekturentscheidungs-Gespräch: Wo liegen die Dateien, und gibt es einen Eigentümer?** Beantwortet am 2026-10-25 in `architektur.md`, Abschnitt „Daten und Besitz“ — ein gemeinsames Repository, das beide vollständig haben.
