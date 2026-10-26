---
etage: Fachkonzept
status: Entwurf
offene_fragen: 2
stand: 2026-10-25
---

# Board — Minikonzept

## Der Kern

Drei Dinge, mehr nicht:

- **Workflow** — eine benannte Kette von Positionen. Eine Vorlage, die man teilen kann.
- **Position** — ein Schritt in dieser Kette, mit Eingang, Ausgang und Bedingung.
- **Vorhaben** — ein benannter Arbeitsgegenstand, der an genau einer Position steht.

Ein Vorhaben trägt seinen Namen vollständig, zum Beispiel „Second Brain Gesamtkonzept": das Projekt und das, was daran gerade entsteht. Es gibt keine Projektzeile daneben — der Name ist der Ort, an dem beides zusammenkommt.

Auch das, was früher „Übergang" hieß, ist einfach eine Position. Es gibt nur eine Form.

## Sprache

Ein Viereck auf der Fläche heißt **Station** (im Code: Schritt) — ein Ort, an dem das Vorhaben Halt macht. Die **Schnur** verbindet zwei Stationen, das **Vorhaben** läuft durch sie hindurch, die **Bibliothek** hält die Stationen, die gerade nicht auf der Fläche stehen, und die **Fläche** ist das Ganze. Jeder Mensch hat einen Namen und eine Farbe; seine Stationen tragen diese Farbe, und jede Station merkt sich ihren Host.

## Die Ansicht

Eine Seite, darauf pro Workflow eine Fläche mit Kisten: **eine Kiste je Position, frei ziehbar und auf zehn Pixel gerastet, Eingang links, Ausgang rechts.** Eine Kiste ist zweigeteilt: oben die Titelzeile mit dem Namen, darunter die Fläche für das, was gerade darin liegt — die Trennlinie macht beides auch dann unterscheidbar, wenn unten nichts liegt. Eine Kiste ist doppelt so hoch wie breit genug für ihre Dateien; ein Klick auf den Namen bläht sie auf und zeigt ihre Einstellungen (Host, Beschreibung, Eingang, Ausgang, Aktion, Weiterreichung). Der Name öffnet die Station, die freie Fläche daneben ist der Griff zum Verschieben; unten zeigt ein Klick auf ein Vorhaben dessen Stand, ein Klick daneben den Schritt. Jeder Schritt trägt eine Farbe: als Streifen auf der Titelzeile, an den Ports und an den Schnüren, die aus ihm kommen. Verbindungen werden von Ausgang zu Eingang gezogen; sie sind dicke, gespannte Linien, die mitwandern. Sie **bestimmen die Reihenfolge** — der Eingang eines Schritts ist, was seine Vorgänger erzeugen. Mehrere Verbindungen sind erlaubt; beim Weiterschicken wählt er dann. In einer Kiste stehen die Vorhaben, die gerade dort sind. Über der Fläche liegt die **Bibliothek**: alle Schritte, auch die, die gerade nicht im Workflow stehen, jeder als dieselbe Kiste in klein. Ein Schritt wird von dort auf die Fläche gezogen und mit Verbindungen in den Fluss gelegt. Ein Schritt ist ein Ding — steht er mehrfach im Workflow, ist er überall derselbe, und eine Änderung gilt an allen Stellen. Herausnehmen lässt ihn in der Bibliothek.

Es gibt keine Zustandsanzeige. Wo ein Vorhaben steht, sagt die Spalte — mehr braucht es nicht. Eine leere Spalte ist leer.

Ein Klick auf ein Vorhaben öffnet nichts. Es färbt sich, und seine Dateien erscheinen — aber nicht dort, wo der Prozess läuft, sondern dort, wo das Ergebnis hingehört: `vision.md` steht in der Spalte „Vision", nicht in „Idee → Vision".

Eine Spalte mit Pfeil ist ein **Prozess**. Eine Spalte ohne Pfeil ist ein **Dokument**; sie zeigt die Dateien, die der Prozess davor erzeugt hat. Ausnahme: der letzte Prozess, denn ihm folgt kein Dokument — dort stehen die Dateien bei ihm.

Datei und Vorhaben tragen dieselbe Farbe, die anderen Vorhaben bleiben sichtbar.

**Gezogen wird mit Rückfrage.** Vorhaben greifen, auf eine Spalte fallen lassen, dann „von … nach … verschieben?“ bestätigen. Die Knöpfe `◀ ▶` bleiben für einen Schritt; `▶` wird grün, sobald die Ausgabedatei des Schritts im Ordner liegt — grün heißt also „hier ist die Arbeit getan“. Beim Verschieben geht nichts verloren: Dateien und Transkripte hängen am Schritt, nicht an der Position. Ein Zurückziehen blendet die späteren Schritte nur aus — ihre Dateien bleiben im Ordner und sind beim Vorziehen wieder da.

Der Skill eines Prozesses steht nicht als Beschriftung über der Spalte, sondern als Element in der Zeile — mit `◆` davor, in der Farbe des Vorhabens. Er erscheint nur am ausgewählten Vorhaben, nur an Schritten, die es erreicht hat, und nur, wenn ein Transkript dazu da ist. Der Weg liest sich dann von selbst: `idee.md`, `◆ grillme-vision`, `vision.md`.

**Nichts wird gezeigt, was es nicht gibt.** Eine Datei erscheint erst, wenn sie im Ordner des Vorhabens liegt; ein Skill erst, wenn ein Transkript dazu da ist. Kein leerer Platzhalter, kein Vorgriff auf einen Namen. Angelegt wird beim Schritt — durch den Skill, das Skript oder von Hand — nicht durch einen Klick auf dem Board.

Auf dem Board steht von einer Datei nur der Name. Ein Klick darauf öffnet den **Leser**: unten über die volle Breite, mit gesetztem Markdown — Überschriften, Listen, Tabellen, Zitate, Code. Sein Kopf ist der Griff: ziehen zeigt zwei Flächen, „Unten“ und „Rechts“, loslassen legt ihn dort ab; ein Knopf tut dasselbe, ein zweiter schaltet zwischen Lesen und Bearbeiten. Beim Bearbeiten steht der rohe Text da; Speichern schreibt in den Ordner des Vorhabens.

**Das Gespräch läuft im Board.** Steht das Vorhaben an einem Prozess-Schritt, steht dort „▶ grillme-vision starten“. Rechts öffnet sich eine Spalte, und der Auftrag geht mit dem Klick sofort hinaus — der Klick ist die Freigabe; die Sitzung entsteht dabei, das Modell beginnt zu fragen. Die Spalte zeigt Fragen und Antworten, wie das Transkript, nur während es entsteht: „arbeitet … 12 s“, solange das Modell rechnet, „Stopp“ bricht ab, „im Chat“ öffnet dieselbe Sitzung dort mit Werkzeugen und Modellwahl. Jedes Gespräch gehört zu seinem Schritt: ein neuer Schritt beginnt eine neue Sitzung, ein Zurückziehen bringt das Gespräch dieses Schritts zurück. Die Spalte lässt sich am Griff links breiter und schmaler ziehen; sie folgt neuen Zeilen, solange er unten ist, und hält still, wenn er selbst nach oben scrollt. Das Eingabefeld lässt sich am Griff darüber größer und kleiner ziehen; „verwerfen“ legt die Sitzung ins Archiv und schließt die Spalte. Ist der Schritt vorbei, steht an seiner Stelle das Transkript.

Die rechte Spalte des Harness selbst ist dafür nicht zu haben: sie gehört dem Gespräch, das in der Mitte läuft. Nimmt das Board die Mitte ein, hat sie nichts, woran sie hängen könnte — deshalb bringt das Board seine eigene Spalte mit.

**Links öffnen im Leser.** Steht in einer Antwort oder in einem Dokument ein Link wie `vision.md`, öffnet er die Datei im Leser — dort, wo auch ein Klick auf dem Board sie öffnet. Fehlt die Datei, steht das da. Externe Adressen öffnen weiter im Browser.

**Die Ansicht bleibt.** Wechselt er zum Chat und zurück, steht das Board wieder so da, wie er es verlassen hat: dasselbe Vorhaben gewählt, dasselbe Dokument offen, die Gesprächsspalte an derselben Stelle, der Leser unten oder rechts, das Eingabefeld so hoch wie gezogen, der Scrollstand. Ist das Board schmal — etwa weil Dokument und Spalte offen sind —, lässt es sich an der Leiste unter der Fläche waagerecht schieben. Gemerkt wird das im geladenen Plugin; ein Neuladen der Seite setzt es zurück.

**Die Idee schreibt er selbst.** Am ersten Schritt, solange die Datei fehlt, steht „✎ Idee schreiben“: ein leeres Blatt im Leser. Speichern legt `idee.md` mit Statuskopf an. Der Knopf „Rechtschreibung“ korrigiert nur Rechtschreibung und Zeichensetzung; das Original bleibt daneben, bis gespeichert ist.

**Überarbeiten mit dem Modell.** Im Bearbeiten steht über dem Text „Was soll anders werden?“ mit einem Feld und dem Knopf „Überarbeiten“. Das Modell schreibt eine neue Fassung — in den Editor, nicht in die Datei: gespeichert wird erst mit „Speichern“, „Zurücknehmen“ stellt den Text von davor wieder her. Der Statuskopf am Anfang geht unverändert zurück, gearbeitet wird nur am Text darunter. Ein zweiter Skill ist dafür nicht nötig; das Gespräch bleibt der Ort für Inhalt und Fragen.

**Jede Fassung bleibt, mit Urheber.** Im Leser steht „Verlauf“: die Fassungen der Datei mit Datum und Namen, der Vergleich zweier Stände — was dazukam, was wegfiel — und „Auf diesen Stand zurück“. Gespeichert wird jede Speicherung, das Ende eines Skill-Laufs und, nach spätestens einer Minute, was außerhalb des Boards geschrieben wurde. Zurücknehmen löscht nichts: es entsteht eine neue Fassung. Die Geschichte liegt in einem lokalen Git-Repository im Arbeitsbereich; nichts verlässt den Rechner.

**Ein Klick auf den Skill zeigt das Transkript.** Jede Sitzung, in der ein Skill gelaufen ist, bleibt als Transkript beim Schritt: Fragen und Antworten, ohne Werkzeuge und Recherche. Es liegt als Datei unter `transkripte/` im Ordner des Vorhabens und wird nur gelesen. Welche Sitzung dazugehört, trägt der Skill am Ende selbst ein. Fehlt der Eintrag, erscheint der Skill gar nicht — lieber nichts als ein leerer Leser.

Ein Klick auf einen Spaltennamen zeigt dagegen den Schritt selbst: Inhalt, Ein- und Ausgang, wie er läuft. Das ist die Definition des Workflows, keine Arbeit daran.

Das ist der Grund, warum die Tabelle nur *innerhalb* eines Workflows funktioniert: Über zwei verschiedene Workflows hinweg würden die Spalten nicht zusammenpassen. Mehrere Workflows heißt also mehrere Tabellen auf derselben Seite — nicht eine Tabelle über alles.

## Was eine Position trägt

Name · Inhalt (was hier geschieht) · Eingang (Dateien) · Ausgang (Dateien) · Wie (von Hand, Skill, Skript) · Bedingung („weiter, wenn …") · Fragen · Verantwortlicher.

Ein Vorhaben trägt: Name · Position · Ordner mit seinen Dateien.

**Die Listen pflegt das Board selbst.** Die Auswahl der Ein- und Ausgabedateien zeigt alle Dateinamen, die im Workflow vorkommen — abgeleitet aus den Schritten, nicht gepflegt in einer zweiten Tabelle. Die Skill-Auswahl zeigt, was im Harness liegt. Von Hand tippen geht weiterhin.

**Ein Skill entsteht als Entwurf.** Der Knopf legt für den Schritt eine Datei an, die weiß, wo sie steht — Position davor, Eingang, Ausgang, Position danach — und die Fragen mitbringt, die sie beim ersten Aufruf stellt. Sie ist sofort am Schritt eingetragen. Fertig ist sie erst, wenn das Gespräch geführt und der Entwurf ersetzt ist.

Jedes Vorhaben hat einen **eigenen Ordner**. Ohne das würden zwei Vorhaben im selben Workflow dieselbe `vision.md` überschreiben. Der Ordnername wird beim Anlegen festgelegt und ändert sich beim Umbenennen nicht — sonst wären die Dateien nach einer Umbenennung weg.

## Dateien folgen dem Namen

Jeder Schritt liest Dateien und erzeugt Dateien. Beides folgt einer Vorgabe und beides ist überschreibbar.

- **Vorgabe.** Der Ausgang heißt wie der Schritt: `‹name›.md`. Trägt der Name einen Pfeil, zählt nur die rechte Seite — „Idee → Vision" erzeugt `vision.md`, „Fachkonzept → Architektur" erzeugt `architektur.md`. Der Eingang ist immer das, was der Schritt davor erzeugt. Ohne Eintrag gilt die Vorgabe — der Normalfall braucht keine Einrichtung.
- **Überschreiben.** Je Schritt lassen sich eigene Dateien eintragen: ein anderer Name, oder mehrere, durch Komma getrennt. Leeren stellt die Vorgabe wieder her.
- **Abgeleitet, nicht gespeichert.** Die geltenden Namen entstehen beim Lesen neu. Ein umbenannter Schritt sucht sofort andere Dateien; ausdrücklich eingetragene Namen bleiben davon unberührt. Eine Umbenennung verschiebt keine vorhandene Datei — sie zeigt danach auf einen neuen Namen und findet nichts, bis der Eintrag angepasst ist.
- **Warum es die Überschreibung trotzdem braucht.** Nicht jeder Schritt heißt wie seine Datei, und mancher erzeugt zwei. Beides ist ein Eintrag, kein Umbau.

## Wie ein Schritt läuft

Drei Arten: von Hand · durch einen Skill · durch ein Skript.

Zwei Arten von Skills, die nicht dasselbe tun:

**Grillme — fragt, entscheidet nichts.** `grillme-vision` holt breit alles ab, auch Technikgedanken; `grillme-fachkonzept` fragt gezielt nach Szenen und verlangt Vorstellungskraft. Beide schreiben zwei Dateien: das Dokument und ein Notizbuch mit allem, was nicht dorthin gehört — `vision-notizen.md`, `fachkonzept-notizen.md`. Nichts geht verloren.

**Kein Grillme — entscheidet oder arbeitet.** `architekturentscheidungen` recherchiert Wege, wählt, begründet, verwirft; Rückfrage nur bei einer wesentlichen Weggabelung, oft gar keine. `umsetzungsplan` zerlegt ohne jede Rückfrage in Arbeitsschritte.

Der Grund: Bei der Architektur fehlt ihm das Wissen, um die Entscheidungen zu treffen — dort ist er Auftraggeber, nicht Auskunftsperson. Bei der Umsetzung sind alle Fragen schon gestellt.

Die Spalten ohne Pfeil — Vision, Fachkonzept, Architekturplan — tragen keinen Skill: dort steht das Ergebnis, nicht die Arbeit. „Idee" läuft von Hand.

Im Board steht er am Schritt, den er ausführt. Ausgeführt wird er nicht von selbst — ausführen heißt, den Skill zu benutzen. Das ist ein eigener Schritt, weil er eigene Entscheidungen braucht.

## Der Rücksprung

Der Umsetzungsplan fragt nichts und rät nichts. Findet er einen offenen Punkt, geht das Vorhaben zurück in die Spalte, zu der der Punkt gehört — Fachkonzept oder Architektur. Dort läuft der Skill dieser Spalte noch einmal, nur für diesen Punkt. Danach geht es wieder vor bis zur Umsetzung.

Am Vorhaben steht der Rücksprung mit seinem Grund, bis es wieder an der Spalte vorbei ist. Ein `↩` im Namen zeigt ihn an, ausgeschrieben erscheint er beim Anklicken.

Das ist der einzige Weg, wie ein offener Punkt in die Umsetzung kommt: gar nicht. Er wird zurückgebracht, nicht mitgeschleppt.

## Bedingungen

Erste Sprache, bewusst klein: alle Fragen beantwortet · Freigabe erteilt (durch wen) · Datei existiert.

Später denkbar: Skript, Modellprüfung, Zeitpunkt. Entscheidend ist, dass die Bedingung ein eigenes Feld ist und nicht im Text versteckt liegt.

## Der Schalter

Pro Position zwei Werte: „automatisch weiter, wenn der Ausgang erfüllt ist" oder „vorher nachfragen".

## Zwei Menschen

- Jede Position und jedes Vorhaben hat einen Verantwortlichen.
- **Alles ist sichtbar.** Fremde Workflows, fremde Positionen, fremde Vorhaben.
- Wer eine Position vermisst, legt eine **eigene Variante** des Workflows an — ein Abzweig oder ein Zwischenschritt wie B1 zwischen B und C.
- Diese Variante ist für alle sichtbar. Vorhaben, die sie nicht verwenden, **überspringen** die Position: Die Spalte bleibt stehen und vermerkt es, sie verschwindet nicht.
- Ob eine fremde Antwort als Erfüllung zählt, entscheidet die Position: „fremde Antwort zählt" oder „braucht Bestätigung".

Damit ist Teilen und Eigenes gleichzeitig möglich, ohne dass jemand die Kette eines anderen umbaut.

## Erste Fassung — die kleine Scheibe

**Enthalten:** ein Workflow mit acht Positionen · Spalten mit Vorhaben · Klick auf ein Vorhaben zeigt den Verlauf mit allen Dateien bis hierher, nur vorhandene Dateien · der Skill als Element am Schritt, mit Transkript aus der Sitzung · Klick auf den Kartennamen zeigt den Schritt zum Bearbeiten · Schritte in der Bibliothek anlegen, auf die Fläche ziehen und verbinden · Vorhaben anlegen, umbenennen, ziehen (mit Rückfrage) und mit Grund zurückschicken · Idee von Hand schreiben, Rechtschreibung korrigieren · Skill aus dem Board starten · Position löschen mit Rückfrage · Eingang, Ausgang und Skill aus Listen wählen · Skill als Entwurf für einen Schritt anlegen · Einklappen über das Kreuz.

**Nicht enthalten:** Fragen und Antworten je Position · der Schalter · Varianten und fremde Positionen in der Oberfläche (im Datenformat aber vorgesehen) · Skripte und Modellprüfungen · Verzweigungen · mehrere Workflows nebeneinander.

## Wie wir es bauen

**Daten:** eine Datei mit Workflow und Vorhaben (`board.json`), je Vorhaben ein Ordner mit seinen Markdown-Dateien. Ohne Oberfläche lesbar, git-fähig. YAML war geplant; geblieben ist JSON, weil das Werkzeug es ohne Zusatz liest.

**Oberfläche:** eine eigene Seite im DSH-Fenster, als eigenes kleines Plugin.

**Warum kein vorhandenes Kanban-Plugin:** Sie denken in Spalten, nicht in Ketten mit Bedingungen und Varianten. Anpassen wäre mehr Arbeit als bauen — und fremder Code im Profil ist eine Vertrauensfrage.

**Reihenfolge:**
1. Vorhandene Plugins kurz ansehen, damit die Entscheidung belegt ist. ✓
2. Gerüst: Workflow anzeigen, Vorhaben in Spalten, Verlauf öffnen und schließen. ✓
3. Schritt bearbeiten: Inhalt, Ein- und Ausgang, Wie. ✓
4. Vorhaben weiterschieben: Auswahlfeld „Position“ im Vorhaben. ✓
5. Fragen und der Schalter. Die Kette selbst steht schon: Idee → Vision → Fachkonzept → Architektur → Umsetzung.

## Entscheidungen

- **Eigene Seite**, kein Teil des Chats.
- **Eine Tabelle pro Workflow**, nicht eine über alles.
- **Aufbau der Kette gehört dem Verantwortlichen**; andere legen Varianten an, die sichtbar bleiben.
- **Standardisierte Workflows sind das Ziel**, weil Zusammenarbeit Einigung braucht.

## Offene Fragen

1. Welche Positionen gehören in den Standard-Workflow? Die eigene Kette ist der Ausgangspunkt; ein Abgleich mit bekannten Vorgehensweisen kommt später.
2. Wie wird eine Variante wieder zusammengeführt, wenn beide sie übernehmen wollen? (später)
