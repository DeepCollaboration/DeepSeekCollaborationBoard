# Entscheidungen

## 2026-10-28 — Neuer Bauplan, alte Arbeit archiviert

Die alte Arbeit (16 Schritte, 2026-10-26) liegt in `arbeit-vorher-2026-10-26/`. Der neue Bauplan (24 Schritte) ist ein neuer Lauf, weil die Architektur von einem gemeinsamen Repository auf getrennte Repositories je Station, Workflow, Vorhaben und Board gewechselt ist.

**Folge:** Der alte Stand bleibt lesbar; die neue `arbeit/` beginnt bei Schritt 01. Die alte Prüfsuite bleibt Bestand und muss grün bleiben.

## 2026-10-28 — Die alte Ablage bleibt unangetastet

Während des Baus bleibt die Ablage der Fassung 1 (das laufende Board) unverändert. Die neue Form läuft auf einer Prüffläche; erst Schritt 13 zieht die vorhandenen Inhalte um.

**Folge:** Das laufende Board ist in jedem Schritt benutzbar; die ersten Schritte ändern keine seiner Daten.

## 2026-10-28 — Prüfablage unter `/tmp`

Die Prüfablage der ersten Scheibe liegt unter `/tmp/board-scheibe1/` und wird von der Prüfung angelegt und wieder entfernt.

**Folge:** Keine Testdaten im öffentlichen Board; die Ablage ist wegwerfbar.

## 2026-10-28 — Schritt 01 bleibt additiv

Schritt 01 legt `repos.js` und `formate/` neu an und ändert `ablage.js`, `index.js`, `client.js` und die alten Prüfungen nicht.

**Folge:** Die alte Prüfsuite bleibt grün; die neue Form steht daneben.

## 2026-10-28 — Das Ziel wird weitergeführt

Das vorhandene Ziel (`goal-39c38a9a…`) war nach dem alten Bau blockiert (Neustart nötig). Es wird für den neuen Bauplan weitergeführt, nicht neu angelegt; die alte Begründung bleibt als Geschichte.

**Folge:** Die Rundenzahl läuft weiter; der neue Bauplan hat 72 Runden Obergrenze.

## 2026-10-28 — Der Neustart bleibt beim Auftraggeber

Die Schritte 01–02 sind über die Prüfungen belegbar. Die Fläche (ab Schritt 03) und die Abnahme (Schritt 23) brauchen einen Neustart des Harness; den macht der Auftraggeber, nicht die Sitzung.

**Folge:** Der Bau kann bis dahin laufen; die Abnahme wird am lebenden Board nachgeholt.

## 2026-10-28 — Angehalten vor dem ersten Schreiben

Der Arbeiter für Schritt 01 wurde gestoppt, bevor er eine Datei geschrieben hat; das Produkt (`/workspace/projekte/board-plugin`) ist unverändert. Das Ziel ist pausiert.

**Folge:** Kein halber Stand im Produkt; Schritt 01 beginnt beim nächsten Start neu. Der Stand liegt in `plan.md`, `fortschritt.md` und `schritte/01.md`; eine Kompaktierung des Gesprächs verliert ihn nicht.
