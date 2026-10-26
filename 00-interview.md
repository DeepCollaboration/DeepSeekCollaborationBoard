---
etage: Vision
status: in Arbeit
offene_fragen: 2
stand: 2026-10-25
---

# Board — Interview und Anforderungen

## Ziel

Eine Oberfläche, die zeigt, wo ein Projekt steht: auf welcher Etage, in welchem Zustand, mit welchen offenen Fragen — und die Beantwortung dieser Fragen direkt an Ort und Stelle erlaubt.

## Was bereits feststeht

Aus dem bisherigen Gespräch, in meinen Worten:

- **Etagen statt Spalten.** Sichtbar sein sollen: Vision → konkretes Ziel → Plan → Architekturentscheidungen → Umsetzungsplan, einschließlich dessen, was davon schon umgesetzt ist.
- **Gleichzeitigkeit ist normal.** Während auf der Umsetzungsebene gebaut wird, kann die Vision noch offen sein. Das muss die Anzeige aushalten.
- **Offene Fragen je Schritt**, direkt im Board beantwortbar. Beim Absenden einer Antwort soll der Kontext der Frage eindeutig sein: worum es geht und welche konkreten Fragen beantwortet werden.
- **Bewegung entsteht aus Kommunikation.** Ist eine Frage beantwortet, arbeitet der Schritt weiter und geht in den nächsten Zustand über — nicht durch Ziehen, sondern durch den Vorgang selbst.
- **Wiederaufnahme.** Nach Tagen soll der Stand sofort erfassbar sein, ohne „gib mir einen Überblick" zu prompten.
- **Zwei Menschen.** Der Kumpel soll mindestens sehen, was er sieht. Beiträge sollen möglich sein, aber nicht als direktes Überschreiben: er kann etwas beitragen, die Annahme entscheidet der andere. Vergleichbar mit einem Pull Request.
- **Zwei Einstiege.** Entweder etwas Neues beginnen — neuer Chat, gewohnter Weg mit Interview und Konzept. Oder an etwas Bestehendem weiterarbeiten; dafür ist das Board der Standard-Einstieg.
- **Das Board ist Wegweiser, nicht nur Anzeige.** Es zeigt offene Projekte, ihren Stand, offene Fragen je Etage, fremde Beiträge und ob jemand weitergemacht hat. Von dort führt ein Klick entweder zu einer kurzen Antwort im Board oder in ein Gespräch.
- **Kontext folgt dem Klick.** Wird aus dem Board ein Gespräch geöffnet, lädt der zuständige Kontext automatisch — die Dokumente, um die es geht. Kein Prompt wie „lies diese Dateien". Was geladen wird, ergibt sich daraus, worauf geklickt wurde.
- **Gespräch fortsetzen oder neu beginnen.** Existiert eine passende Unterhaltung, wird sie fortgesetzt; sonst startet automatisch ein neues Gespräch mit dem relevanten Kontext.
- **Kein Kanban-Wissen vorausgesetzt.** Die Bedienung muss ohne Brett-Erfahrung verständlich sein.
- **Erweiterbarkeit.** Bevorzugt als Plugin, das hier entwickelt wird — sofern es nichts Vorhandenes gibt, das passt.

## Ablaufsteuerung

Das Board ist nicht nur eine Anzeige, sondern steuert auch den Ablauf:

- **Schalter zwischen den Schritten.** Pro Schritt einstellbar: automatisch weiterlaufen, sobald er fertig ist — oder auf manuelle Freigabe warten.
- **Bedingungen zwischen den Schritten.** Was muss erfüllt sein, damit es weitergeht — zum Beispiel, dass eine bestimmte Datei existiert oder dass alle offenen Fragen beantwortet sind.
- **Schritte als Module.** Der bisher nur im Prompt beschriebene Ablauf (Interview → Fachkonzept → Architekturplan → Schrittliste) wird als Kette sichtbar und in der Oberfläche zusammengesteckt, ähnlich einer Automatisierungssoftware.
- **Sichtbarkeit der Regel.** Es muss erkennbar sein, welcher Schritt warum startet.

Zwischen zwei Etagen liegen zwei verschiedene Dinge, die nicht verwechselt werden dürfen: der **Zustand** (was existiert, was offen ist) und die **Regel** (wann etwas laufen darf). Die Anzeige zeigt den Zustand; die Regel entscheidet über den Start.

### Erste Fassung

- **Ein Schalter pro Übergang, zwei Werte.** Entweder fragt der Übergang nach, ob der Schritt abgenommen ist und weitergeht — oder er geht automatisch weiter. Mehr nicht.
- **Übergänge sind eigene Objekte.** Auch wenn die erste Definition nur zwei Werte kennt: Der Übergang zwischen zwei Positionen ist ein eigenes Ding mit eigener Definition und kann später weitere Bedingungen aufnehmen. Die Architektur muss das von Anfang an mitdenken — nicht als Sonderfall im Schritt, sondern als eigenes Element dazwischen.

## Was das Modell leisten muss

- **Status als Eigenschaft, nicht als Position.** Ein Knoten *hat* einen Zustand; er steht nicht in einer Spalte. Erst dadurch sind mehrere Etagen gleichzeitig in Arbeit, ohne Widerspruch.
- **Fragen als eigene Objekte** mit Blockade, Antwort und Datum — nicht als Kommentar unter einer Karte. Einmal beantwortet, geschlossen, nie erneut gestellt.
- **Vorschläge statt Überschreiben** bei mehreren Personen.
- **Stille Übersicht** über Liegengebliebenes, ohne Benachrichtigungsdruck.

## Offene Fragen

1. **Was sind die Positionen?** Etagen (wenige, grobe Schritte) oder Schritte im Umsetzungsplan (acht bis über hundert)? Der Schalter sitzt dazwischen — die Antwort bestimmt das Datenmodell.
2. **Wer führt einen Schritt aus?** Eigenes, frisches Gespräch oder das laufende? Und sieht man ihn arbeiten oder nur das Ergebnis?
3. **Was passiert bei einem Fehlschlag?** Anhalten, wiederholen, oder weiter mit Vermerk?
4. **Wie findet das Board die passende Unterhaltung** zu einem Schritt? (technisch, später)

## Nächste Schritte

1. Interview klären (offene Fragen oben).
2. Prüfen, ob ein vorhandenes Plugin den Anforderungen genügt — erst nach der Klärung, nicht vorher.
3. Fachkonzept ohne Technik.
4. Architektur und Schrittliste.
