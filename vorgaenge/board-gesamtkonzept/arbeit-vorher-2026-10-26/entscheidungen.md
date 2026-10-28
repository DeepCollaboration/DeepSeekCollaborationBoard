# Entscheidungen

## 2026-10-26 — Bauplan übernommen, nicht neu geschnitten

`bauplan.md` enthält 16 Schritte in 5 Scheiben mit Prüfungen. Sie sind der Plan.
Kein eigener Schnitt, keine Umordnung.

## 2026-10-26 — Produkt ist der Plugin-Quellcode, nicht ein Dienst

`produkt/` nimmt die Dateien des Plugins auf. Das Profil `web` verweist danach
dorthin (`link:` in `/dsh-home/profiles/web/package.json`). Das ist ein Eingriff
außerhalb des Arbeitsbereichs; er wird als solcher ausgeführt und im Bericht
genannt. Der bisherige Ordner `plugins/dsh-board` bleibt als Rückweg liegen und
wird nicht mehr bearbeitet.

## 2026-10-26 — Profilverweis umgestellt (einmaliger Eingriff außerhalb des Arbeitsbereichs)

`/dsh-home/profiles/web/package.json` und `pnpm-lock.yaml` verweisen jetzt mit
`link:` auf `produkt/`; der Symlink `node_modules/@local/dsh-board` zeigt dorthin.
Die Auflösung im Profil wurde geprüft und nennt
`produkt/index.js`. Folge: Beim nächsten Neustart des Harness läuft das Board aus
dem Produktordner. Bis dahin läuft das bereits geladene Modul unverändert weiter
und schreibt weiter an den alten Ort. `plugins/dsh-board` bleibt unangetastet
liegen; der Rückweg ist ein Zurückstellen der drei Stellen.

## 2026-10-26 — Prüfung am Modul statt am lebenden Board

Der Harness lädt das Host-Plugin beim Start und hält es im Speicher; die Sitzung
kann den Harness nicht neu starten, ohne sich selbst zu beenden. Prüfungen, die
„Board neu laden“ verlangen, werden deshalb in dieser Sitzung ersetzt durch
Prüfungen am Modul: Routen-Handler direkt aufrufen, Dateien und Git-Zustände
nachsehen. Was nur im laufenden Board sichtbar wäre, steht am Ende unter
„Ungeprüft“. Das ist eine Abweichung von den Prüfungen des Bauplans, keine von
der Architektur.

## 2026-10-26 — Auffrischung vor dem Umschalten

Das alte Board wird im Fenster weiter benutzt; seine Klicks (Layout, Positionen,
neue Vorhaben) landen in `board.json` und nicht in den neuen Dateien. Damit beim
Umschalten nichts davon fehlt, wird der Umzug vor dem letzten Schritt noch einmal
mit dem dann aktuellen `board.json` aufgefrischt: Personen, Workflow mit Schritten
und Positionen sowie Name, Position, Farbe und Läufe der Vorhaben werden
übernommen; die Felder, die erst das neue Modell führt (`verantwortliche`,
`regel`, `rueckkehr`, `fork_von`, Beiträge), bleiben unangetastet. Was danach noch
im alten Board geändert wird, geht verloren — deshalb endet der Lauf mit dem
Hinweis, den Harness neu zu starten.

## 2026-10-26 — Persönliche Seiten unter `$DSH_HOME/board/`

Einstellungen und die Verweise auf laufende Gespräche sind persönlich und stehen
nicht in der Ablage: `$DSH_HOME/board/einstellungen.json` und
`$DSH_HOME/board/gespraeche.json`. Die Dateien wurden aus dem Umzug dorthin
übernommen (Eingriff außerhalb des Arbeitsbereichs, einmalig). Ohne sie fehlt
„Mausrad: seitwärts“, und die Gesprächsspalte kennt die alten Sitzungen nicht.

## 2026-10-26 — Übergang: das alte Modul bleibt bis zum Neustart lesbar

Der Harness hält das alte Board-Modul im Speicher. Es liest weiter
`projekte/board/board.json`. Würde Schritt 03 diese Datei löschen, wäre das Board
im Fenster ab dem nächsten Aufruf leer — die Daten wären zwar in den neuen
Dateien, aber das laufende Modul kennt sie nicht. Deshalb bleibt `board.json`
zunächst als Ausgabe liegen, die das neue Modul beim Schreiben mitführt; die
neuen Dateien sind die Wahrheit. Die Prüfung „ohne board.json" läuft auf einer
Kopie. Entfernt wird die Datei erst, wenn das neue Modul läuft (Neustart), und
zwar im letzten Schritt des Laufs. Folge: Bis zum Neustart ist die Board-Oberfläche
der alte Stand; Änderungen an Metadaten, die dort gemacht werden, kommen in den
neuen Dateien nicht an. Die Dokumente selbst liegen an derselben Stelle und
bleiben gültig.

## 2026-10-26 — `produkt/` liegt außerhalb der Board-Geschichte

`.gitignore` des Arbeitsbereichs schließt `vorgaenge/*/produkt/` aus. Der
Rückweg für Schritt 01 ist deshalb die Geschichte des Arbeitsbereichs, in der
`plugins/dsh-board` gefasst ist. Der Bauplan sieht für die Ablage ein eigenes
Repository vor (Schritt 02); das Produkt gehört nicht hinein.

## 2026-10-26 — Konvergenzprüfung: sieben Befunde behoben, acht festgehalten

Die Prüfung gegen die Architektur (`arbeit/konvergenz.md`, 15 Abweichungen) ist
gelaufen. Sieben sind behoben (`arbeit/nachbesserung-1.md`, Fassung
„Konvergenz-Nachbesserung“):

- **A1** — Der Schritt-Skill bleibt für die Dauer der Sitzung in
  `$DSH_HOME/skills` und wird erst bei „Stopp“ oder „Verwerfen“ weggenommen. Vorher
  war er schon vor dem ersten Auftrag gelöscht; ein Modelllauf hätte ihn nicht
  gesehen. Damit ist der Kern von Scheibe 2 erst wirklich belegt.
- **A8** — Speichern einer vorhandenen, gefassten Datei verlangt eine `basis`;
  ohne sie antwortet der Host 409 mit dem neuen Stand, statt ungeprüft zu
  überschreiben.
- **A9** — Annehmen eines Beitrags prüft dessen `basis` je übernommener Datei;
  ist der Stand neuer, wird nichts geschrieben (409).
- **A14** — Nur die eigene Personendatei wird geschrieben; `/board/person/aendern`
  auf eine fremde Kennung antwortet 403.
- **A3, A4, A13** — `/board/konfiguration` ergänzt; `/board/data` trägt `person`
  und `repo`; die fünf Schritt-Anweisungen verweisen nicht mehr auf `board.json`,
  sondern auf `vorgaenge/<ordner>/vorhaben.json` und `workflows/<person>.json`.

Acht bleiben bewusst stehen, mit Grund:

- **A2 (Position statt Schritt in Routen und Läufen).** Das Produkt trennt die
  Station (eine Stelle, an der ein Schritt stehen kann) von der Art des Schritts;
  die Route nimmt darum die Kennung der Station. Ein fremder Client nach der
  Architekturtabelle müsste `position` statt `schritt` senden. Die Bedeutung ist
  dieselbe, der Name nicht.
- **A5, A6, A7 (Form von `/board/versionen`, `/board/verlauf`,
  `/board/gespraech/stand`).** Die Antworten sind in Objekte gefasst und tragen
  teils andere Feldnamen (`autor`, `positionId`, `eintraege`). Die mitgelieferte
  Seite ist darauf gebaut; ein fremder Client müsste die produkt-eigene Form
  lesen. Das Verhalten stimmt, die Form weicht ab.
- **A10 (`regel: "alle"`).** Es gibt keinen Weg, die Zustimmung mehrerer
  einzusammeln. Das Board bietet dann niemandem eine Handlung an, statt eine
  Zustimmung zu behaupten. Die Frage, wie „alle müssen“ funktioniert, bleibt im
  Fachkonzept offen (Frage 2).
- **A11 (`zweig_von` als Objekt).** Der Bauplan verlangt Ursprung, Station und
  Datum; die Architektur nennt nur die Kennung. Das Objekt trägt mehr, nicht
  weniger.
- **A12 (`wie.art`).** Das Produkt kennt `datei`, `textfeld`, `chat` neben `skill`
  und `skript`; die drei Werte der Architektur sind eine engere Fassung. Ein
  Abbilden auf `manuell` würde das Verhalten der Felder verlieren.
- **A15 (kein `origin`).** Die Adresse und der Zugang sind seine Einrichtung; das
  Board sagt bis dahin „kein remote“ und arbeitet lokal weiter.
