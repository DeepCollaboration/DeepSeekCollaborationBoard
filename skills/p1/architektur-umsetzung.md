---
name: p1-architektur-umsetzung
description: Macht aus Architektur und Fachkonzept einen Bauplan — vertikale Scheiben, prüfbare Schritte, erste Scheibe. Entscheidet keine Technik, die steht in der Architektur. Ohne Rückfrage.
whenToUse: Wenn ein Vorhaben an der Station „Architektur zu Bauplan" steht und der Bau geplant werden soll.
---

# Bauplan erstellen

Ein Arbeitsauftrag, kein Gespräch. Lesen, schneiden, schreiben — ohne eine einzige
Rückfrage.

## Was der Plan ist

Der **Weg zum finalen Produkt**. Die Architektur beschreibt den Zustand; der Plan
beschreibt, in welcher Reihenfolge daraus etwas Benutzbares wird und woran man
erkennt, dass ein Schritt fertig ist.

Die Architektur ist die Zeichnung. Der Plan ist die Baustelle.

## Warum ohne Rückfrage

Alle Fragen sind bis hierher gestellt: in der Vision, im Fachkonzept, in den
Architekturentscheidungen. Was jetzt noch unklar ist, wird nicht nachgefragt,
sondern entschieden und als Annahme vermerkt.

Eine Rückfrage an dieser Stelle wäre ein Zeichen, dass früher etwas versäumt
wurde. Dann steht das so in der Datei — und der Plan wird trotzdem geschrieben.

## Eingang

`architektur.md`, `fachkonzept.md`, `vision.md` sowie `fachkonzept-notizen.md`
und `vision-notizen.md`.

Alle fünf werden gelesen. Die Notizen enthalten oft den Grund für eine Reihenfolge
oder eine Sorge, die sonst verloren geht.

## Was der Plan entscheidet — und was nicht

**Er entscheidet:**

- die Scheiben,
- die Reihenfolge,
- die Schnittgröße,
- die Prüfkriterien.

**Er entscheidet nicht:**

- **den Techstack.** Sprache, Laufzeit, Framework, Bibliotheken, Datenhaltung,
  Protokolle stehen in `architektur.md`. Der Plan übernimmt sie.
- **die technischen Details.** Dateistruktur, Namen, konkrete Aufrufe entscheidet
  der Ausführende. Der Plan sagt, was danach da ist, nicht wie es innen aussieht.
- **die Zeit.** Keine Dauer, keine Schätzung.

Fehlt der Techstack oder ist er unklar — keine Sprache, keine Datenhaltung, kein
Betrieb —, wird nicht geraten. Das Vorhaben geht zurück (siehe Rücksprung).

## Was am Ende da ist

`bauplan.md` im Ordner des Vorhabens — `/workspace/projekte/board/vorgaenge/<ordner>/`,
der Ordner des Vorhabens unter `/workspace/projekte/board/vorgaenge/<ordner>/`:

```markdown
---
etage: Umsetzung
status: in Arbeit
offene_fragen: 0
stand: JJJJ-MM-TT
---

# Umsetzung

## Erste Scheibe
## Scheiben
## Schritte
## Schritt 1 — Name
## Zurückgeschickt
```

## Vertikal bauen

Eine **Scheibe** ist ein Weg durch das ganze Produkt, nicht eine Ebene davon. Die
erste Scheibe ist der kleinste vollständige Weg, den ein Mensch gehen kann: eine
Eingabe, eine Verarbeitung, eine Ausgabe, eine Ablage — jede Ebene einmal, in
klein.

Das ist kein Grundgerüst. Es ist ein benutzbares Produkt im Kleinen. Danach wird
jede weitere Scheibe eine Ebene tiefer oder breiter, ohne die anderen zu brechen.

**Horizontal ist der Fehler:** erst die Datenablage, dann die Schnittstellen, dann
die Oberfläche. Danach ist lange nichts benutzbar, und niemand kann sagen, ob die
Architektur trägt.

Beispiel: „Ein Vorhaben anlegen, speichern, wiederfinden und anzeigen" ist eine
Scheibe. „Die Repository-Schicht", „das Plugin-Gerüst" und „die Oberfläche" sind
Ebenen, keine Scheiben.

## Der Schnitt

Ein Schritt ist die kleinste Einheit, die ein Fremder prüfen kann.

- **Ein Schritt, ein Ergebnis.** Zwei Ergebnisse sind zwei Schritte.
- **Ein prüfbares Ergebnis.** Jeder Schritt endet mit etwas, das man benutzen,
  ansehen oder mit einem Befehl prüfen kann. Kein Schritt endet mit „jetzt ist die
  Grundlage da".
- **Ein Arbeiter kann es allein.** Der Schritt braucht nur die genannten Dateien
  und den genannten Auftrag. Wenn er den halben Plan braucht, ist er zu groß.
- **Kein Umbau nebenbei.** Aufräumen ist ein eigener Schritt oder gar keiner.
- **Risiko früh.** Was am ehesten schiefgeht, kommt nach vorn, solange die
  Entscheidung noch billig ist.
- **Nichts auf Vorrat.** Kein Baustein, der erst drei Schritte später gebraucht wird.

Teilen, wenn: zwei Ergebnisse, zwei unabhängige Teile, „und dann noch", oder man
den Schritt nicht in einem Satz prüfen kann.

Zusammenfassen, wenn: der erste Teil allein nichts Sichtbares bringt.

**Grob bleiben.** Zehn bis fünfundzwanzig Schritte für ein Produkt dieser Größe.
Keine Datei-für-Datei-Planung: Was in einem Schritt gebaut wird, entscheidet der
Ausführende. Der Plan legt fest, was danach da ist und wie man es prüft.

## Form eines Schritts

```markdown
### Schritt 3 — Name

- **Scheibe:** zu welcher Scheibe er gehört.
- **Ziel:** ein Satz.
- **Eingang:** was vorher da sein muss, mit Pfad.
- **Tun:** was geschieht, in Stichworten.
- **Ergebnis:** was danach existiert, mit Pfad.
- **Prüfung:** der Befehl oder Handgriff, mit dem man es prüft, und was dabei
  herauskommen muss — am Verhalten, nicht an der Innenseite.
- **Stopp:** ja, wenn hier eine Entscheidung fällt, die teuer zurückzunehmen ist.
```

## Reihenfolge

1. Die erste Scheibe: der kleinste vollständige Weg, danach prüfen.
2. Danach das, was er wirklich benutzt.
3. Was nichts riskiert und nichts sichtbar macht: ans Ende oder streichen.
4. Nach jedem Schritt bleibt das Vorhandene benutzbar.

## Nichts bleibt offen

Der Plan wird nicht mit Annahmen gefüllt. Ist etwas unklar, wird nicht geraten und
nicht entschieden — das Vorhaben geht zurück.

Offen heißt: die Antwort ändert, was gebaut wird. Alles andere ist Formulierung und
wird entschieden. Die Grenze ist eng gezogen; im Zweifel gilt: zurück.

## Der Rücksprung

Findet er einen offenen Punkt, dann in dieser Reihenfolge:

1. `bauplan.md` schreiben — aber nur mit dem Abschnitt „Zurückgeschickt": der
   offene Punkt, die Spalte, zu der er gehört (Vision → Fachkonzept, Fachkonzept →
   Architektur oder Architektur → Umsetzung), und was dadurch blockiert ist.
2. Das Vorhaben auf dem Board zurückschicken: `↩` beim ausgewählten Vorhaben,
   Zielspalte wählen, den offenen Punkt als Grund eintragen.
3. Dort läuft der Skill dieser Spalte noch einmal — nur für den offenen Punkt.
4. Danach geht es wieder vor bis hierher, und der Plan wird geschrieben.

Der Rücksprung bleibt am Vorhaben sichtbar, bis es wieder an seiner Spalte vorbei
ist. Beim zweiten Anlauf ist der Rest schon da: nichts wird neu erfunden, nur der
offene Punkt kommt dazu.

## Grenzen

- Keine Rückfrage. Keine.
- Was offen ist, wird nicht entschieden, sondern zurückgeschickt.
- Keine Dauer, keine Schätzung in Stunden oder Tagen. Reihenfolge statt Zeitplan.
- Keine Techstack-Entscheidung. Die steht in der Architektur.
- Keine Schritte, die er nicht selbst prüfen kann.
- Kein Technikunterricht; er prüft am Verhalten.

## Während der Arbeit

Der Plan bleibt in Gebrauch, nicht im Archiv: erledigte Schritte werden markiert
statt gelöscht, Änderungen an der Reihenfolge bekommen ihren Grund, Abweichungen
werden nachgezogen.

## Abschluss

Fertig, wenn die Scheiben stehen, die erste Scheibe benannt ist, jeder Schritt ein
Ergebnis, eine Prüfung und einen Stopp-Punkt hat und **nichts offen geblieben ist**.
Danach beginnt die Ausführung mit Schritt 1 — nicht mit einer neuen Planung.

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
