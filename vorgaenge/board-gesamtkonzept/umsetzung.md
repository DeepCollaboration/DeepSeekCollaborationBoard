---
etage: Umsetzung
status: in Arbeit
offene_fragen: 2
stand: 2026-10-26
---

# Umsetzung — Board

## Offene Fragen

1. **2026-10-26, an ihn: Der Harness muss neu gestartet werden**, damit das neue
   Board läuft. Blockiert die Prüfung am lebenden Board und im Browser; bis dahin
   läuft das alte, geladene Modul weiter.
2. **2026-10-26, an ihn: Welche Adresse bekommt `origin`?** Die Ablage hat keinen
   Remote. Blockiert den Abgleich zwischen zwei Rechnern; bis dahin sagt das Board
   „kein remote“.

## Die Architektur

Das Board ist ein Plugin für den DeepSeek Harness. Seine Seite zeigt die
Workflows und Vorhaben aller beteiligten Personen; die gemeinsame Ablage ist ein
Git-Repository, das jede Person vollständig in ihrem Arbeitsbereich hat und das
das Plugin vor dem Lesen und nach jedem Schreiben abgleicht. Es gibt keinen
Dienst, keine Datenbank, keine Anmeldung. Jede Person arbeitet in ihrem eigenen
Harness; das Board startet die Sitzungen und die Skills eines Schritts lokal.
Rechte prüft das Board; die Dateien und Fassungen sind die Wahrheit.

## Was entstanden ist

**Das Produkt** liegt unter `vorgaenge/board-gesamtkonzept/produkt/`:

- `index.js` — die Host-Hälfte: 46 Routen unter `/board/*`, die Prüfstelle `darf`,
  Abgleich, Fassungen, Sitzungen, Skill-Einsatz, Formatprüfung vor jedem Schreiben.
- `ablage.js` — liest und schreibt die Ablage (Format 1), je Datei nur bei
  Änderung; kennt `format`, Personen, Workflows, Vorhaben, Beiträge.
- `client.js` — die Seite: Flächen je Person, fremde Vorhaben lesend, Beiträge und
  Entscheid, Herkunft, Rücksprung, Konfliktbehandlung, Kopfzeile mit dem Stand.
- `board.config.json` — `{ "person": "p1", "repo": "/workspace/projekte/board" }`.
- `test/schritt02.mjs` … `test/schritt16.mjs` — 15 Prüfskripte, die das Produkt
  gegen eine Kopie in `/tmp` fahren; `werkzeuge/umzug.mjs` — der einmalige Umzug.

**Die Ablage** `projekte/board/` ist ein eigenes Git-Repository:

- `format` = 1; `personen/p1.json`; `workflows/p1.json` mit elf Schritten und fünf
  Anweisungen; `vorgaenge/<ordner>/vorhaben.json` für fünf Vorhaben;
  `skills/p1/` mit fünf Schritt-Anweisungen; `beitraege/` entsteht beim ersten
  Beitrag.
- `board.json` liegt noch daneben: der Stand, den das alte, geladene Modul liest.
  Das neue Modul liest und schreibt sie nicht.

**Der Harness** lädt das Plugin über das Profil `web` aus `produkt/`. Persönliche
Seiten (Einstellungen, Gesprächsverweise) liegen unter `$DSH_HOME/board/`.

## Start

1. Den Harness neu starten (wie sonst; das Profil `web` lädt
   `@local/dsh-board` aus `produkt/`). Das ist der einzige Startschritt — kein
   eigener Dienst, kein Port.
2. Prüfen: `curl -s "$DSH_WEB_URL/board/data"` antwortet mit 200 und trägt
   `format`, `formatAblage`, `person`, `repo`, `workflows`, `vorgaenge`,
   `beitraege`, `stand`.
3. Das Board öffnen: dieselben fünf Vorhaben an ihren Stationen, deine Fläche,
   die fünf Schritt-Anweisungen.
4. Die Prüfskripte: `cd produkt && for n in 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16; do node test/schritt$n.mjs || break; done`.

## Was es kann

- Eigene und fremde Workflows zeigen; fremde Schritte als Verweis, mit Host;
  keine Kopie, keine Bearbeitung von hier.
- Fremde Vorhaben an ihrer Station mit Namen, Farbe und Verantwortlichem zeigen
  und lesen — ohne Umbenennen, Ziehen oder Schreiben.
- Dateien öffnen, lesen, bearbeiten; Fassungen mit Urheber vergleichen; einen
  überholten Stand nicht überschreiben, sondern als Beitrag anbieten.
- Am Schritt ein Gespräch führen; den Skill des Schritts — auch eines fremden —
  aus der Ablage einsetzen, starten und nach dem Ende wieder wegnehmen; das
  Transkript beim eigenen Vorhaben ablegen.
- Beiträge anlegen (Notiz und geänderte Fassung), sie ganz, gar nicht oder in
  Teilen annehmen; die Übernahme als neue Fassung.
- Fork, Zweig und Rücksprung als Daten führen und sichtbar machen.
- Den Stand über Git abgleichen, sagen wie frisch er ist und ohne Remote
  weiterarbeiten.

## Was es nicht kann

- **Ohne Remote kein Abgleich zwischen zwei Rechnern.** Die Ablage ist ein
  eigenes Repository ohne `origin`; das Board sagt „kein remote“.
- **Bis zum Neustart ist die Oberfläche der alte Stand.** Das geladene Modul
  schreibt weiter `board.json`; Änderungen an Metadaten, die dort gemacht werden,
  kommen in den neuen Dateien nicht an.
- **`regel: "alle"` ist nicht benutzbar.** Es gibt keinen Weg, die Zustimmung
  mehrerer einzusammeln; das Board bietet dann niemandem eine Handlung an.
- **Ein fremder Client nach der Architekturtabelle** müsste teils andere Feldnamen
  lesen (Position statt Schritt, `autor` statt `urheber`, `eintraege` statt
  `bloecke`). Die mitgelieferte Seite ist auf die produkt-eigene Form gebaut.
- **Das Produkt liegt in keinem Repository.** Der Arbeitsbereich und die Ablage
  schließen `produkt/` aus; der Rückweg für den Anfang ist die alte Kopie unter
  `plugins/dsh-board` in der Geschichte des Arbeitsbereichs.
- **Kein zweiter Rechner, kein echter GitHub-Abgleich.** Beides ist nicht
  vorhanden; belegt ist der Abgleich zwischen zwei Klonen auf einer Platte.

## Ungeprüft

1. Die Oberfläche im laufenden Harness — der Prozess lädt eine ältere Fassung;
   erst ein Neustart zeigt die neue Seite.
2. Eine echte Sitzung mit echtem Modell — die Prüfskripte ersetzen den
   Sitzungsdienst, das Modell und die Sitzungsdateien.
3. Ein zweiter Rechner und eine echte zweite Person — `p2` lebt nur in
   Prüfkopien.
4. Ein Abgleich über HTTPS oder SSH — geprüft mit einem lokalen Bare-Repository.
5. Die Bedienung im Browser — Ziehen, Layout, Markdown, Scrollen; geprüft wurde
   der Client-Code und die von ihm gerufenen Routen.
6. Zwei gleichzeitige Schreiber auf zwei Rechnern — die Basisprüfung ist mit
   aufeinanderfolgenden Vorgängen belegt, nicht mit echter Gleichzeitigkeit.

## Wichtigste Entscheidungen

- **Der Profilverweis zeigt auf `produkt/`** — ein einmaliger Eingriff außerhalb
  des Arbeitsbereichs. Folge: Beim nächsten Neustart läuft das Board von dort.
- **`board.json` bleibt bis zum Neustart liegen.** Es zu löschen hätte das
  laufende alte Modul leer gemacht. Das neue Modul fasst sie nicht an; ein späterer
  Schritt kann sie entfernen.
- **Persönliche Seiten unter `$DSH_HOME/board/`** (Einstellungen,
  Gesprächsverweise) — persönlich, nicht in der geteilten Ablage.
- **Der Schritt-Skill bleibt für die Dauer der Sitzung** und wird erst bei Stopp
  oder Verwerfen weggenommen. Vorher war er zu früh weg; der Modelllauf hätte ihn
  nicht gesehen.
- **Ein Beitrag und seine Annahme prüfen die Basis.** Ein überholter Stand wird
  nicht geschrieben; das gilt auch für das Speichern ohne Basis.
- **Ein Verweis trägt `{ref, id}`**: `ref` nennt die Herkunft, `id` gibt der
  Verweis-Station eine lokale Kennung.
- **Ein Fork kopiert die Dateien, aber nicht die Läufe und nicht die Transkripte**
  des Ursprungs.
- **Geprüft wird am Modul, nicht am lebenden Board**, solange der Harness nicht
  neu gestartet ist: ein Ersatz-`ctx`, ein Ersatz-Sitzungsdienst, Kopien der Ablage
  unter `/tmp`.

## Wie geprüft wurde

- Jeder der 16 Schritte hatte einen Arbeiter und einen frischen Prüfer; jedes
  Urteil steht mit Befehl, Exit-Code und entscheidender Zeile in
  `arbeit/pruefung.md`. Die Prüfskripte 02–16 enden mit Exit 0.
- Die Konvergenzprüfung gegen die Architektur (`arbeit/konvergenz.md`) fand 15
  Abweichungen, 6 nicht prüfbare Punkte und 6 offene Annahmen. Sieben Abweichungen
  sind behoben (`arbeit/nachbesserung-1.md`, Fassung „Konvergenz-Nachbesserung“),
  acht mit Grund festgehalten (`arbeit/entscheidungen.md`).
- Die echte Ablage blieb von allen Prüfungen unberührt: die Skripte arbeiten auf
  Kopien unter `/tmp`; `git status` ist nach jeder Fassung leer, die letzte Fassung
  ist „Thomas Heinrich Konvergenz-Nachbesserung“.

## Abweichungen von der Architektur

**Behoben:** A1 (Skill zu früh entfernt), A8 (Speichern ohne Basis), A9 (Annehmen
ohne Basisprüfung), A14 (fremde Personendatei schreibbar), A3
(`/board/konfiguration` fehlte), A4 (`person`/`repo` in `/board/data`), A13
(Skill-Texte verwiesen auf `board.json`).

**Festgehalten, mit Grund:** A2 (Position statt Schritt in Routen und Läufen), A5,
A6, A7 (Form von `/board/versionen`, `/board/verlauf`, `/board/gespraech/stand`),
A10 (`regel: "alle"`), A11 (`zweig_von` als Objekt), A12 (`wie.art`), A15 (kein
`origin`). Die Gründe stehen in `arbeit/entscheidungen.md`.

## Dateien

- `produkt/index.js`, `produkt/ablage.js`, `produkt/client.js`,
  `produkt/package.json`, `produkt/cordis.patch.yml`, `produkt/icon.svg`,
  `produkt/locale/de.json`, `produkt/locale/en.json`,
  `produkt/board.config.json`, `produkt/test/schritt02.mjs` …
  `produkt/test/schritt16.mjs`, `produkt/werkzeuge/umzug.mjs`.
- Ablage: `format`, `personen/p1.json`, `workflows/p1.json`,
  `vorgaenge/<ordner>/vorhaben.json` (fünf), `skills/p1/*.md` (fünf),
  `board.json` (Übergang).
- Arbeit: `arbeit/plan.md`, `arbeit/fortschritt.md`, `arbeit/entscheidungen.md`,
  `arbeit/pruefung.md`, `arbeit/konvergenz.md`, `arbeit/schritte/01.md` …
  `arbeit/schritte/16.md` samt Berichten, `arbeit/nachbesserung-1.md` samt
  Bericht.
