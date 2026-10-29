---
etage: Plan
status: läuft
offene_fragen: 0
stand: 2026-10-29
---

# Nachbearbeitung — Oberfläche, Benutzung, Veröffentlichung

Der Bau ist fertig: 65 Prüfungen grün, acht Sprachen, Theme und Einstellungen
an ihrem Ort. Was fehlt, ist der Weg von „läuft bei mir“ zu „andere können es
benutzen, verstehen und mitentwickeln“. Dieser Plan ordnet diesen Weg.

Grundlage ist die Bestandsaufnahme in [ui-analyse.md](ui-analyse.md). Dort
stehen die Befunde mit Zeilenverweisen; hier stehen die Schritte. Die Nummern
setzen bei 57 fort, weil der Bauplan bei 56 endet.

## Entschieden (2026-10-29)

1. **Lizenz: MIT.** Weitergabe und Änderung erlaubt, keine Rückgabepflicht.
   Trägt Schritt 71.
2. **Veröffentlichung: GitHub und DSH-Plugin-Markt.** Das Repository ist der
   Ort, der Markt der Eintrag, der dorthin zeigt. Der Eintrag kommt nach der
   Freigabe; er braucht die Fassung, die auf GitHub liegt.
3. **Bilder und Push kommen später.** Beides braucht ihn: Aufnahmen aus seinem
   Browser, einen Weg zum entfernten Rechner. Der Plan läuft weiter, statt an
   zwei Handgriffen zu warten — die Stellen bleiben frei, nichts wird
   vorgetäuscht. Offen bleiben damit: die Bilder in 70 und 72 und der Push in 76.

## Was heute anders ist als auf GitHub

Der öffentliche Stand ist vom 2026-10-28: die Fassung vor der
Oberflächen-Überarbeitung, ohne die acht Sprachen und ohne den
Einstellungsabschnitt. Lokal liegen 16 geänderte Dateien mit 9645 Zeilen Zuwachs
uncommittet — der aktuelle Stand ist also nur auf diesem Rechner. Wer heute vom
Repository installiert, bekommt das alte Board; es startet, sieht aber anders
aus und spricht nur Deutsch.

Am Repository selbst fehlen: Beschreibung, Themen, Lizenz, README auf Englisch,
Bilder, Entwicklerdokumentation, Beitragsregeln, Sicherheitshinweis, CI,
Release. `package.json` nennt weder Zweck noch Lizenz noch Repository und hat
kein `test`-Skript.

## Entscheidungen, die ich selbst treffe

- **README:** Englisch wird die Hauptfassung (das erwartet GitHub), Deutsch
  bleibt als vollständige zweite Fassung. Die übrigen sechs Sprachen bekommen
  eine kurze Einstiegsseite (Was es ist, Installieren, erste Schritte, Links)
  statt einer vollen Übersetzung — sonst veralten sechs lange Texte still.
- **Version:** 1.0.0 zum ersten öffentlichen Stand; vorher 0.1.0.
- **Kein npm:** Installation über GitHub genügt; ein npm-Paket bringt nur eine
  zweite Veröffentlichungsstelle.

## Scheiben und Schritte

### Scheibe 12 — Grundlage

| Nr | Schritt | Ergebnis | Prüfung | Status |
| --- | --- | --- | --- | --- |
| 57 | Oberfläche aufnehmen und Befunde festhalten | `arbeit/ui-analyse.md` | jede Aussage mit Zeilenverweis | bestanden |
| 58 | Vier Bausteine herausziehen: Knopf, Feld, Panel, Meldung | ein Panel mit Kopf, Schließen, Escape, Fokus; eine Meldung mit Zustand (läuft, fertig, Fehler); zuerst in Einstellungen und Stationsdetail benutzt; dabei zeigt die Einstellungsseite beim Laden „wird geladen“ statt „fehlt das Schreibrecht“ | Suite grün; im Browser unverändert | bestanden |
| 59 | Client gliedern und kommentieren | Kopfindex, Abschnittsköpfe, je Funktion ein Satz Zweck; tote Reste entfernt (18 Stile, `spalten`, `tdSpalte`), Kopfkommentar an die freie Fläche angepasst | `node --check`; Suite 65/65; du findest eine Stelle in unter einer Minute | bestanden |

### Scheibe 13 — Oberfläche

| Nr | Schritt | Ergebnis | Prüfung | Status |
| --- | --- | --- | --- | --- |
| 60 | Erste Sitzung führt zum Schreiben | Karte „Du liest mit“ mit zwei Wegen (Token eintragen, eigenes Board); verschwindet, sobald Schreibrecht da ist | Hauptboard ohne Token öffnen; Karte da; Weg gehen; Karte weg | bestanden |
| 61 | Alle Panels auf ein Muster | Station, Vorhaben, Katalog, Beiträge, Fragen, Board-Liste, Leser und Chat teilen Hülle, Schließen und Tastatur; der Fokus kehrt zurück, eine geöffnete Karte behält ihre Tastaturbedienung | mit Tab öffnen, Escape schließen; Fokus kehrt zurück | bestanden |
| 62 | Rückmeldungen als Status | ein Statusbaustein mit `aria-live`, getrennt vom Hilfetext; der Zustand steht im Zustand, nicht in der Zeichensetzung; Fehler bleibt stehen | Aktion auslösen: „fertig“ oder Fehler; nichts verschwindet ungesehen | bestanden |
| 62b | Fortschritt bei den Wegen, die Netz brauchen | die acht Netzwege setzen „läuft“ vor dem Aufruf und enden mit „fertig“ oder Fehler | jede `laeuft`-Stelle endet; keine bleibt stehen | bestanden |
| 63 | Schrift und Maße auf eine Skala | Schrift 12/13/14/18 px, Radien 6/8/12; fester Takt für Abstände nur dokumentiert | keine Fließtextgröße unter 12 px; Browser | bestanden |
| 64 | Die Fläche erklärt sich | Satz auf der leeren Fläche, Bibliothek höhenbegrenzt, „Alles zeigen“ rollt zu den Karten | leeres Board zeigt einen Satz; Bibliothek rollt innen; Knopf findet die Karten | bestanden |
| 64b | Sprung zur Station | ein Feld in der Leiste findet Stationen und rollt zu ihrer Karte | zwölf Stationen, eine suchen, Karte im Blick | bestanden |
| 64c | Übersicht und Zoom | Knöpfe für näher, weiter und alles; die Fläche skaliert, Karten bleiben bedienbar | zoomen, alles zeigen, Station öffnen | zurückgestellt |
| 65 | Schmale Fenster und 200 % | Panels passen, Menüs kippen nicht heraus, Fläche scrollt | Fenster auf 360 px, Zoom 200 % | bestanden |
| 66 | Fehlende Übersetzungen schließen | die 17 Texte gehen durch `uebersetze`; eine Prüfung findet sichtbare Texte ohne `uebersetze` | neue Prüfung grün; acht Wörterbücher ergänzt | läuft |
| 66b | Der Grund im Lesemodus spricht die Sprache | die fünf Sätze des Hosts werden über eine Tabelle übersetzt, mit Rückfall | Suite grün; englischer Grund bei englischem Dashboard | offen |

### Scheibe 14 — Benutzung prüfen und dokumentieren

| Nr | Schritt | Ergebnis | Prüfung | Status |
| --- | --- | --- | --- | --- |
| 67 | Abnahmeliste schreiben | `arbeit/abnahme.md`: 16 Wege mit Erwartung und Ergebnis | Liste vollständig; von dir bestätigt | offen |
| 68 | Durchgang im Browser | Protokoll: jeder Weg bestanden oder Befund | du gehst mit; ich schreibe mit | wartet auf dich |
| 69 | Befunde beheben | je Befund ein Schritt oder eine Zeile im Protokoll | Suite grün; Nachgang im Browser | offen |
| 70 | Handbuch der Benutzung | `docs/benutzung.de.md` und `docs/benutzung.en.md` mit Bildern und Fehlersuche | ein Fremder kommt ohne dich bis zur ersten eigenen Station | offen |

### Scheibe 15 — GitHub und Mitentwicklung

| Nr | Schritt | Ergebnis | Prüfung | Status |
| --- | --- | --- | --- | --- |
| 71 | Repo-Grundlagen | LICENSE (MIT), `package.json` (Zweck, Lizenz, Repository, Fehleradresse, Node 24, `npm test`), CHANGELOG, `.editorconfig`, `.gitattributes` | `npm test` grün; Metadaten vollständig | offen |
| 72 | README Englisch, Sprachen, Bilder | `README.md` englisch; `README.de.md`; kurze Fassungen zh, es, fr, ru, ja, ar; Sprachwahl oben; Bilder folgen | jede Fassung: Installation, erste Schritte, Links | offen |
| 73 | Entwicklerdoku im Repo | `docs/architektur.md`, `docs/entwicklung.md`, `docs/datenformate.md`, `docs/routen.md` | ein Fremder findet Route, Format, Platz und Test | offen |
| 74 | Code dokumentieren | Modulköpfe, Verträge, Abschnittsköpfe; Anleitung „eine Sprache hinzufügen“ | Sprache in unter einer Stunde ergänzt | offen |
| 75 | Zusammenarbeit und CI | CONTRIBUTING, SECURITY, Issue- und PR-Vorlage, CI auf Node 24 | CI grün auf GitHub; Vorlagen erscheinen | offen |
| 76 | Fassungen und Push | Commits in Scheiben, Branch, PR-Beschreibung, Repo-Beschreibung und Themen | `git log` lesbar; GitHub zeigt den aktuellen Stand | wartet auf dich |

### Scheibe 16 — Freigabe

| Nr | Schritt | Ergebnis | Prüfung | Status |
| --- | --- | --- | --- | --- |
| 77 | Frischer Klon, frisches Profil | Installation und erster Start protokolliert | Hauptboard da; Lesen ohne Token; keine Fehlerseite | offen |
| 78 | Release 1.0.0 | Version, Tag, Release Notes, Lizenzangabe | Release auf GitHub; Installation mit Tag | offen |
| 79 | Endabnahme | dein Durchgang, Protokoll, Freigabe | du sagst „veröffentlichen“ | offen |

## Fertig heißt

Veröffentlicht ist es, wenn alles davon zutrifft:

1. Ein Fremder versteht in fünf Minuten, was das Board ist — englische
   Startseite mit einem Bild und einem Installationsbefehl.
2. Eine frische Installation in einem leeren Profil zeigt das Hauptboard, ohne
   Token, ohne Fehlerseite.
3. Alle Prüfungen laufen auf GitHub bei jedem Push (Node 24) und sind grün.
4. Lizenz, Beschreibung, Themen, Version und Release stehen im Repository.
5. Das Handbuch (DE und EN) deckt jeden sichtbaren Weg ab; die Fehlersuche nennt
   die drei echten Fälle (Startfehler, 403, kein Netz).
6. Ein Mitentwickler findet Aufbau, Routen, Datenformate, Tests und die
   Anleitung für eine neue Sprache.
7. Alle Panels lassen sich mit Tastatur öffnen und schließen; der Fokus kehrt
   zurück; Meldungen werden vorgelesen.
8. Keine Geheimnisse im Repository; der Token ist in SECURITY beschrieben.
9. Das Plugin steht im DSH-Plugin-Markt und zeigt dorthin.
10. Du hast es im Browser abgenommen und sagst ja.

## Was ich nicht anfasse

- Kein eigener Server, keine Datenbank, keine eigene Anmeldung — das bleibt
  GitHub.
- Keine eigenen Themes: das Board folgt weiter dem Dashboard, dazu der eine
  Akzent.
- Keine Übersetzung der Modellaufträge; sie steuern das Verhalten.

## Reihenfolge

Schritt 58 ist die Voraussetzung für 61, 62 und 63. Schritt 59 kann parallel zu
60 laufen, ist aber selbst ein Umbau am ganzen Client und läuft allein. Die
Scheiben 14 und 15 hängen nicht voneinander ab; 67 muss vor 68 stehen, 71 vor
72. Jede Scheibe endet mit einer Prüfung, die du sehen kannst.
