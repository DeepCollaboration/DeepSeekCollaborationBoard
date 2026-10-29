---
etage: Umsetzung
status: fertig
offene_fragen: 0
stand: 2026-10-29
---

# Oberfläche — Bestandsaufnahme

Grundlage für die Nachbearbeitung. Alle Zahlen und Zeilen stammen aus
`/workspace/projekte/board-plugin/client.js` (9518 Zeilen) und `index.js`
(8177 Zeilen), Stand 2026-10-29. Zeilenangaben ohne Dateinamen meinen `client.js`.

## Wo die Oberfläche sitzt

Das Plugin belegt drei Plätze im Dashboard. Alles, was Menschen sehen, spielt
sich im ersten Platz ab; die beiden anderen sind schmal.

| Platz | Inhalt | Zeile |
| --- | --- | --- |
| `main` | die ganze Seite: Fläche, Panels, Chat, Leser | 9501 |
| `settings.section` | Abschnitt „Board“ im Einstellungsfenster | 9507 |
| `sidebar.panellist` | Symbol in der Seitenleiste | 9512 |

Es gibt kein eigenes Fenster, keine Route und keinen Dialogdienst. Panels sind
absolut positionierte Kästen im selben Baum; die Seite ist ein einziger
React-Baum mit einem Wurzelelement.

## Die Teile der Seite

| Teil | Zeilen | Aufgabe |
| --- | --- | --- |
| Kopfzeile und Board-Leiste | 8976–9270 | Board wechseln, Zustand („Nur lesen“, „Hauptboard nicht geholt“), Abgleichstand, Menschen am Board |
| Fläche (frei, scrollbar) | 6869–7295 | Karten liegen absolut auf einer 2D-Fläche, Verbindungen als SVG; Ziehen und Verbinden |
| Reiter und Panels | 7985, 8012, 8434, 8456, 8752, 9322 | Bibliothek/Verlauf, Stationspanel, Dateien, Leser, Chat |
| Stationsdetail | 6376–6925 | geöffnete Station: Inhalt, Wie, Dateien, Fuß mit Aktionen |
| Bibliothek | 7697–7960 | Stationen, Workflows, Vorhaben; Suche; Stern; Verweis |
| Katalog | 7791–8100 | öffentliche Dinge, Herkunft, Übernehmen |
| Beiträge und Fragen | 2260–2448 | Pull Requests und Issues als Flächen |
| Leser | 8457–8730 | Datei, Verlauf, Fassungen, Transkript |
| Chat-Spalte | 8740–8930 | Gespräch an einer Station, Eingabe, Verlauf |
| Startseite | 4913–5010 | Name, Farbe, erstes Board — nur wenn **kein** Board im Register steht |
| Einstellungen | 1819–2100 | Akzent, Anordnung, Mausrad, Eigentümer, Identität, GitHub |

## Zahlen

| Maß | Wert | Was daraus folgt |
| --- | --- | --- |
| `BoardPage` | 6778 Zeilen (2742–9519) | ein Bauteil trägt fast die ganze Seite |
| Zustände | 96 × `useState`, 47 × `useEffect`; davon 72 und 39 in `BoardPage` | Zustand ist über eine Funktion verteilt, nicht über Haken |
| Stilobjekt `S` | 868 Zeilen (146–1014), 402 Verweise `style: S.` | Gestaltung liegt als ein Block in der Datei, nicht in Bausteinen |
| Meldungen | 133 × `setMeldung`, davon 42 × `setMeldung(null)` | eine Zeile Text für Erfolg, Fehler und „läuft“ |
| Anfragen | 34 × `fetch`, 25 × `sende` | zwei Wege mit verschiedenen Grenzen und Fehlertexten |
| Zugänglichkeit | 26 `aria-*`, 4 × `role`, 1 × `tabIndex`, 0 × `.focus()` | Panels und Karten sind für Tastatur und Vorlesehilfe kaum beschriftet |
| Tastatur | 8 Stellen mit `Escape` | Schließen per Taste nur an wenigen Panels |
| Fenstergröße | 0 × `resize`/`ResizeObserver`, keine Media-Abfrage | die Seite rechnet nicht mit schmalen Fenstern |
| Schrift | 11 px an 60 Stellen, 12 px an 46, 9 px an 2 | Fließtext ist kleiner als im Dashboard (Basis 14 px) |
| Maße | 10 Schriftgrößen, 10 Radien (2–14 px plus 999) | keine Skala; jede Änderung streut |
| Stileinträge | 213 Einträge im Stilblock, 18 nie benutzt | tote Reste im Stilblock |
| Knöpfe | 146 × `type: 'button'`, 79 × `S.klein` | viel Handarbeit statt eines Bausteins |
| Formulare | 0 × `<label>`/`htmlFor` | Beschriftung nur über `span` und `aria-label` |

## Befunde

**1. Ein Bauteil trägt fast alles.** `BoardPage` vereint Daten laden, Auswahl,
Leser, Chat, Bibliothek, Katalog, Ziehen, Meldungen und die ganze Darstellung.
Ein Mitentwickler muss 6778 Zeilen lesen, bevor er eine Karte ändern kann; jede
Änderung kann an einer entfernten Stelle wirken.

**2. Panels sind Einzelbauten.** Board-Liste (291), Leser (8457), Katalog (7791),
Stationsdetail (6376), Chat (8740) und die beiden Flächen teilen sich keine
Hülle. Es gibt kein gemeinsames Schließen, keine gemeinsame Tastatur, keinen
gemeinsamen Fokus. Sieben Stellen bauen ihren Kopf selbst.

**3. Eine Meldung für alles.** Alle Rückmeldungen laufen über einen String
(`setMeldung`). „Läuft“, „fertig“ und „Fehler“ sehen gleich aus; 42 Stellen
löschen ihn beim nächsten Klick. Ein Fehler kann verschwinden, bevor er gelesen
ist. Eine `aria-live`-Region gibt es nicht; nur vier Stellen tragen `role`.

**4. Tastatur und Fokus fehlen.** Panels öffnen, ohne den Fokus zu holen, und
schließen, ohne ihn zurückzugeben. Es gibt genau eine `tabIndex`-Stelle (die
leere Stationsfläche) und keine einzige Fokusvergabe. Das ✕ entfernt den
fokussierten Knoten; danach steht der Fokus auf `body` (7265–7270, 8018, 8439,
8607). Wer mit Tastatur arbeitet, bleibt hinter dem Panel stehen; wer mit
Vorlesehilfe arbeitet, hört nicht, dass sich etwas geöffnet hat. Eine geöffnete
Karte verliert außerdem `role` und `tabIndex`, die sie geschlossen hatte
(7277–7278).

**5. Die Schrift ist klein.** Der Fließtext liegt überwiegend bei 11 bis 12 px;
das Dashboard setzt seine Basis bei 14 px. Die Farben sind geprüft: alle sechs
Akzente halten den Kontrast (5,0 bis 18,9 : 1, hell und dunkel). Das Problem ist
die Größe, nicht der Kontrast.

**6. Die Fenstergröße ist nicht bedacht.** Es gibt keine Reaktion auf `resize`,
keine Media-Abfrage. Feste Breiten (Board-Liste 460 px) und drei Stellen mit
`calc(100vw …)` tragen bis zu einem gewissen Grad; darunter und bei 200 %
Vergrößerung schiebt sich Inhalt aus dem Bild.

**7. Siebzehn sichtbare Texte stehen außerhalb der Übersetzung.** Der
Wörterbuch-Test prüft nur, was durch `uebersetze(...)` geht. Diese Titel und
Sätze gehen nicht durch und bleiben deshalb in jeder Sprache deutsch:

- 5629, 5630: „Weitergeben: …“, „Keine Station danach“
- 7024, 7025: „Ordner öffnen — …“, „Dokument öffnen“
- 7210: „Klein machen“, „Station öffnen“
- 7226: „… fehlt; gelesen wurde der aktuelle Stand“
- 7236: „Ziehen verschiebt die Station“
- 7409: „Kein lesbarer Verweis“
- 7969, 7970: „Die öffentlichen Dinge des Hauptboards“, „Was dieses Board benutzt“
- 8062: „Ein- und Ausgang zuklappen“, „Ein- und Ausgang zeigen“
- 8084–8086: „Steht schon als Verweis …“, „Als Verweis … stellen …“
- 9012: „Für dieses Board fehlt das Schreibrecht“
- 9259, 9260: „Dein Name und deine Farbe“, „Name und Farbe von …“

**8. Die erste Sitzung führt nicht zum Schreiben.** Nach dem ersten Start ist
das Hauptboard offen und lesbar, aber schreibgeschützt. Der Zustand steht als
Etikett „Nur lesen“ in der Leiste; der Weg zum Token steht nur im
Einstellungsfenster. Die Willkommensseite (4913) erscheint nur, wenn gar kein
Board im Register steht — im Normalfall also nie. Wer neu ist, sieht eine
fremde, gesperrte Fläche ohne nächsten Schritt.

**9. Zwei Anfragewege.** `sende` (25 Aufrufe) hat eine Zeitgrenze und einen
Fehlertext; die 18 direkten `fetch`-Aufrufe haben beides nicht. Je nach Panel
verhält sich das Board bei Netzfehlern anders.

**10. Maße ohne Skala.** Zehn Schriftgrößen und zehn Radien, Abstände ad hoc.
Das wirkt unruhig und macht jede Anpassung teuer.

**11. Der Kopfkommentar beschreibt ein Board, das es nicht mehr gibt.** Die
Zeilen 2–15 sprechen von Spalten („In einer Spalte stehen die Vorhaben“); die
Fläche ist eine freie 2D-Fläche mit absolut gesetzten Karten (7124). `spalten`
(5001) wird berechnet und nie gelesen, `S.tdSpalte` (655) ist tot, 18
Stileinträge werden nirgends benutzt. Wer den Code über den Kommentar liest,
sucht Bauteile, die es nicht gibt.

**12. Die Einstellungen zeigen beim Laden einen falschen Zustand.** Solange
`/board/data` lädt, stehen in `BoardEinstellungen` leere Werte und der Satz
„Für dieses Board fehlt das Schreibrecht.“ (1823, 1844, 1989–1993) — als wäre
etwas verboten, statt „wird geladen“. Ein Ladefehler hat dort kein „Nochmal“.

## Reibung im Alltag

Zehn Stellen, an denen Menschen hängen bleiben; aus dem Code abgeleitet, nicht
aus einem Test.

1. **Leere Fläche ohne Satz.** Ein Board ohne Stationen zeigt nichts (6903);
   der Weg zur Bibliothek steckt im zugeklappten Reiter (7985).
2. **Erster Fehlschlag ohne Ausweg.** Der Ladefehler der Seite (4902–4905)
   bietet kein „Nochmal“, die Absturzfalle (9424) schon.
3. **Kopfzeile wächst unbegrenzt.** Dauerhinweis, Abgleichstände und Warnungen
   schieben die Fläche zusammen, statt zu verschwinden (9218–9245).
4. **Meldung im Hilfetext.** Meldungen stehen dort, wo die Hilfe steht, und
   verdrängen sie (9218); 42 Stellen löschen sie beim nächsten Klick.
5. **Bibliothek ohne Höhengrenze.** Sie kann über die Fläche hinauswachsen
   (469–473, 9323); die Fläche darunter hat `flex: 1` und `minHeight: 0`.
6. **Öffnen ohne Fokus.** Kein Panel holt den Fokus; `Escape` wirkt nur in acht
   Feldern (5177, 5703, 5742, 6961, 7517, 7603, 9282, 9297).
7. **Ziehen nur mit Maus.** Stationen, Vorhaben, Kanten und Ports sind nicht per
   Tastatur erreichbar; ein Fehlwurf bleibt stumm (7142–7186).
8. **„Nur lesen“ ohne Weg.** Das Etikett sagt nicht, was zu tun ist; die Sperren
   liegen an sechs Stellen verstreut (3015, 7252, 7321, 7423, 7584, 8579).
9. **Leser mit neun gleich gewichteten Knöpfen.** „Speichern“ (8542) sieht aus
   wie „nach unten“ (8605).
10. **RTL spiegelt Text, nicht Lagen.** Arabisch dreht die Schrift, die Karte
    bleibt an ihrer physischen Stelle (8974, 7124) — bewusst, aber im Handbuch
    zu erklären.

## Was schon gut ist

- **Farben kommen aus dem Dashboard.** Keine eigenen Farbwerte außer den
  Personenfarben und den sechs Akzenten; hell und dunkel getrennt.
- **Acht Sprachen mit Rückfall.** Fehlt ein Eintrag, steht der deutsche
  Ausgangstext; Arabisch dreht die Fläche.
- **Zustände sind sichtbar.** „Nur lesen“ mit Grund, „Hauptboard nicht geholt“
  mit Wiederholen, Ladefehler mit Satz — die Seite bricht nicht ab.
- **Bestätigungen bleiben stehen.** Eine angelegte Station nennt Repository und
  Weg in den Katalog, statt zu verschwinden.
- **Die Fläche ist ruhig.** Wenige Farben, freie Anordnung, ein persönlicher
  Akzent; das Board sieht nach Arbeitsfläche aus, nicht nach Formular.
- **Einstellungen an einem Ort.** Darstellung, Identität und GitHub stehen im
  Einstellungsfenster des Dashboards, nicht im Board verstreut.

## Was das für die Nachbearbeitung heißt

Die Reihenfolge folgt aus den Befunden: erst die Bausteine (Panel, Meldung,
Feld, Knopf) und die Gliederung, dann die sichtbaren Verbesserungen, dann
Benutzung, Dokumentation und Veröffentlichung. Der Plan steht in
`arbeit/nachbearbeitung.md`.
