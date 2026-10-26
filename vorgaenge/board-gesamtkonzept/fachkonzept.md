---
etage: Fachkonzept
status: fertig
offene_fragen: 6
stand: 2026-10-25
---

# Fachkonzept — Board

## Zweck

Das Board ist der Ort, an dem mehrere Menschen dasselbe Vorhaben voranbringen. Es bildet den Ablauf ab, den man schon kennt — Dateien anlegen und bearbeiten, Skills und Interviews ausführen —, hält alles an einem Ort und zeigt auf einen Blick, wo jedes Vorhaben steht. Der Stand steht im Board, nicht in einem Kopf.

Es ist keine neue Methode. Es macht eine vorhandene teilbar: derselbe Ablauf, den eine Person heute allein geht, soll von mehreren gegangen werden können, ohne dass jemand den Überblick allein trägt.

## Für wen und welche Rollen

Gebaut für mehrere Menschen, zuerst für zwei, ohne Obergrenze. Nicht auf Software beschränkt: auch zivilgesellschaftliche Vorhaben und Projekte über Organisationen hinweg.

**Host** — hängt am Schritt. Er gestaltet den Schritt: wie er läuft, mit welchem Skill, in welcher Version. Seine Änderung gilt sofort für alle, die diesen Schritt verwenden. Es steht niemand über ihm.

**Verantwortlicher** — hängt am Vorhaben, genauer an einem Abschnitt davon. Er führt die Schritte seines Abschnitts aus, entscheidet über Beiträge, sagt wann ein Schritt fertig ist und gibt an den nächsten weiter. Nicht eine Person für das ganze Vorhaben; der Abschnitt kann wechseln.

**Beitragende** — jeder, der das Board sieht. Er liest, schlägt vor und kann forken. Er ändert nichts direkt.

Host und Verantwortlicher können dieselbe Person sein. Ein Schritt kann mehrere Verantwortliche haben; dann gilt eine von zwei Regeln: einer reicht, oder alle müssen. Weitere Formen — etwa eine Frist, in der man widersprechen kann — sind denkbar, aber nicht jetzt.

## Szenen

**Ein Vorhaben läuft durch.** Idee → Vision → Fachkonzept → Architektur → Umsetzung. An jedem Schritt entsteht etwas: eine Datei, ein Interview, ein Skill-Lauf. Der Verantwortliche führt aus; ist der Schritt getan, gibt er an den nächsten weiter. Das Board zeigt den Stand, die Dateien und wer verantwortet.

**Eine zweite Person macht mit.** Sie sieht das Vorhaben an einem Schritt, öffnet es, liest den Stand und beantwortet eine offene Frage — oder schlägt eine Änderung vor. Der Verantwortliche sieht den Vorschlag und nimmt ihn an, nimmt ihn nicht an oder nimmt Teile davon. Erst dann ändert sich der Stand.

**Ein Schritt fehlt.** Der Verantwortliche fügt ihn in seinen Abschnitt ein. Den Schritt selbst gestaltet, wer ihn hostet; fehlt der Skill, entsteht er am Schritt.

**Jemand will es anders machen.** Wer nicht Verantwortlicher ist und den Stand weiterführt, nimmt — und das ist ein Fork. Das Original läuft weiter, der Fork steht als `fork: …` da.

**Der Host ändert seinen Schritt.** Das ist eine neue Version desselben Schritts; der Name bleibt. Sie gilt sofort für alle Vorhaben, die den Schritt verwenden. Wer den neuen Ausgang nicht will, spricht sich ab oder hängt eine Bridge dazwischen.

**Ein Schritt geht kaputt.** Er bleibt stehen; der Ausführende versucht es erneut. Ist der Host nicht erreichbar und der Schritt bleibt kaputt, forkt der letzte Verantwortliche den Schritt, repariert ihn und setzt ihn an die Stelle des kaputten. Die Datei läuft weiter.

**Ein Stand geht an zwei Stellen.** Derselbe Verantwortliche gibt ihn an mehrere Schritte gleichzeitig ab. Es entstehen mehrere Verläufe aus einem Stand — Branches.

## Begriffe

- **Vorhaben** — ein benannter Arbeitsgegenstand, der an einem Schritt steht und einen eigenen Ordner mit seinen Dateien hat.
- **Workflow** — eine Zusammensetzung aus Schritten. Es gibt kein Original des ganzen Workflows; original ist die Datei, die weiterläuft. Mehrere Workflows stehen nebeneinander.
- **Schritt** — ein Element im Workflow mit Eingang, Ausgang, Host und Version. Was er tut, gehört seinem Host.
- **Abschnitt** — der Teil des Wegs, den ein Verantwortlicher führt: von seinem Schritt bis zur Übergabe an den nächsten Verantwortlichen.
- **Host** — wer den Schritt gestaltet.
- **Verantwortlicher** — wer den Abschnitt führt: ausführen, annehmen, abschließen, weitergeben.
- **Abgeben und Nehmen** — Abgeben führt das Original weiter; Nehmen ist ein Fork.
- **Fork** — eine eigene Weiterführung, die jemand nimmt, der nicht Verantwortlicher ist; sichtbar als `fork: …`.
- **Branch** — mehrere Verläufe aus einem Stand, wenn derselbe Verantwortliche an mehrere Schritte abgibt.
- **Version** — eine Fassung eines Schritts oder einer Datei. Der Name bleibt; die Version steht in der Historie und in den Infos. Eine neue Version gilt sofort.
- **Bridge** — ein Zwischenschritt, der einen Ausgang in den Eingang umwandelt, den der nächste Schritt erwartet.
- **Beitrag** — ein Vorschlag an einem Schritt. Er überschreibt nichts; der Verantwortliche nimmt an, nimmt nicht an oder nimmt Teile.
- **Originaldatei** — der Stand, der weiterläuft; die Vollmacht über sie liegt beim letzten Verantwortlichen.

## Verhalten im Normalfall

- Der Weg ist eine Kette von Schritten. An jedem entsteht etwas; wo ein Vorhaben steht, sagt der Schritt.
- Der Verantwortliche führt aus und entscheidet, wann der Schritt fertig ist — bei mehreren nach der Regel des Schritts.
- Weitergabe: Der letzte Verantwortliche gibt an den nächsten Schritt. Abgeben ist immer original.
- Der Host gestaltet den Schritt. Seine Änderung gilt sofort für alle, die den Schritt verwenden; der Name bleibt, die Fassung steht in der Historie.
- Das Board zeigt nur, was es gibt: eine Datei, wenn sie da ist; einen Schritt, einen Fork, einen Branch, eine Version, sobald sie existieren. Kein Platzhalter, kein leerer Leser.
- Fremde Schritte und fremde Vorhaben sind sichtbar, mit Herkunft und dem, der gerade verantwortet. Ein Klick zeigt den Stand; ein Beitrag ist möglich.
- Ein eigenes Vorhaben kann einen fremden Schritt verwenden. Der Schritt bleibt der Schritt seines Hosts; er wird nicht kopiert.

## Ausnahmen und Fehler

- **Der Schritt gelingt nicht.** Er bleibt stehen, das Vorhaben geht nicht weiter; der Ausführende führt erneut aus.
- **Der Host ist nicht erreichbar und der Schritt bleibt kaputt.** Der letzte Verantwortliche forkt den Schritt, repariert ihn und setzt ihn an die Stelle des kaputten. Die Originaldatei läuft weiter; nur dieser Schritt wird umschifft. Ein Fork des ganzen Workflows gibt es nicht.
- **Der Empfänger tut nichts.** Der letzte Verantwortliche hat die Datei in der Hand und kann sie woanders hingeben — als Branch.
- **Ein Beitrag wird nicht angenommen.** Der Beitragende kann selbst forken und weiterführen.

## Rechte

- **Sehen:** Jeder, der das Board sieht, sieht fremde Schritte und Vorhaben, mit Herkunft und dem, der gerade verantwortet.
- **Beitragen:** Jeder; ein Beitrag überschreibt nichts.
- **Annehmen:** Der Verantwortliche — ganz, gar nicht oder in Teilen.
- **Ausführen:** Der Verantwortliche des Abschnitts.
- **Abschließen:** Der Verantwortliche, der ausgeführt hat — nicht der Host. Bei mehreren nach der Regel des Schritts.
- **Weitergeben:** Wer den Stand hat, also der letzte Verantwortliche.
- **Schritt gestalten:** Der Host. Seine Änderung gilt für alle, die den Schritt verwenden.
- **Forken:** Jeder, der nimmt.
- **Verantwortung abgeben:** Nur wer sie hat.

## Grenzen

Eine Abgrenzung nach außen hat sich nicht gezeigt. Das Board muss einhalten, was es anbietet — nicht mehr und nicht weniger. Es wird keine Grenze erfunden; taucht später eine auf, gehört sie hierher.

## Kleinste vollständige Scheibe

Die erste Fassung ist vollständig, wenn vier Dinge gehen:

1. Ein Schritt einer anderen Person ist auf meinem Board sichtbar.
2. Ein Vorhaben einer anderen Person ist sichtbar.
3. Mein eigenes Vorhaben kann diesen fremden Schritt verwenden.
4. Ich kann an ihrem Vorhaben etwas beitragen, und der Verantwortliche nimmt es an oder nicht.

Nicht in der ersten Scheibe: Forks und Branches in der Oberfläche, Versionen, Bridges, die Regeln für mehrere Verantwortliche. Sie bleiben im Modell vorgesehen, werden aber später gebaut.

## Offene Fragen

1. **2026-10-25, an ihn: Wie sieht ein Beitrag aus?** Ein geänderter Dokumentstand mit Angabe der Änderungen, oder nur angehängte Kommentare? Blockiert die Form des Beitrags.
2. **2026-10-25, an ihn: Wenn ein Schritt mehrere Verantwortliche hat — entscheidet einer über einen Beitrag, oder müssen alle zustimmen?** Blockiert die Annahme-Regel bei mehreren.
3. **2026-10-25, an ihn: Was passiert mit einem Beitrag, den der Verantwortliche nicht anschaut?** Bleibt er liegen, oder gibt es eine Stelle, an der er sichtbar wartet? Blockiert das Verhalten für Liegengebliebenes.
4. **2026-10-25, an ihn: Zwei Verantwortliche ändern dasselbe Dokument — was gilt?** Blockiert das Verhalten bei gleichzeitiger Änderung.
5. **2026-10-25, an ihn: Farbe — nach dem Verantwortlichen oder nach der Person, die angelegt hat?** Blockiert nur die Farbregel.
6. **2026-10-25, an ihn: Wie sieht der Rücksprung im Rollenmodell aus?** Wenn ein offener Punkt ein Vorhaben an einen früheren Schritt zurückschickt: Wer führt ihn aus, und wer nimmt das Ergebnis an? Blockiert die Ausnahme „Fehler fällt später auf". Der Umsetzungsplan sieht den Rücksprung vor.
7. **2026-10-25, an die Architektur: Wo liegen die Dateien, und gibt es einen Eigentümer?** Beantwortet am 2026-10-25 in `architektur.md`, Abschnitt „Daten und Besitz“: ein gemeinsames Repository, das beide vollständig haben; kein Server hält es allein; Eigentum teilt sich in drei Zuständigkeiten.
