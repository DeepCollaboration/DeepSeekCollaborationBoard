---
etage: Umsetzung
status: in Arbeit
offene_fragen: 0
stand: 2026-10-26
---

# Umsetzung — Bauplan

## Erste Scheibe

**Scheibe 1 — Zwei Personen sehen einander.** Danach öffnet sich das Board aus einem
gemeinsamen Repository; der Workflow der anderen Person, ihre Schritte und ihre
Vorhaben stehen auf dem eigenen Board, mit ihrem Namen und dem, der gerade
verantwortet; das Board sagt, wann zuletzt abgeglichen wurde.

Warum sie zuerst kommt: Die Vision macht den Kern daran fest, dass ein fremder
Bereich mit Namen dasteht, anklickbar ist. Und das Riskanteste steckt darin — ein
eigenes Repository, zwei Kopien, ein Abgleich, eine Ablage, die zwischen zwei
Installationen gilt. Solange nur eine Person schreibt, ist ein Fehler billig.

Was sie nicht enthält: einen fremden Schritt ausführen (Scheibe 2), beitragen
(Scheibe 3), Herkunft und Rückweg (Scheibe 4), die Prüfungen gegen Verlust
(Scheibe 5). Die „kleinste vollständige Scheibe“ des Fachkonzepts ist erst mit
Scheibe 3 erreicht: Sie nennt vier Dinge, und das Sehen ist nur das erste davon.

## Scheiben

1. **Zwei Personen sehen einander** — Schritte 1–7. Danach stehen fremde
   Workflows, Schritte und Vorhaben mit Host und Verantwortlichem auf dem Board,
   und der Stand ist abgeglichen.
2. **Ein fremder Schritt läuft bei mir** — Schritte 8–10. Danach kann ein eigenes
   Vorhaben einen fremden Schritt benutzen, und sein Skill läuft im eigenen
   Harness.
3. **Beitragen und annehmen** — Schritte 11–12. Danach lässt sich an einem fremden
   Vorhaben ein Beitrag anlegen; der Verantwortliche nimmt an, lehnt ab oder nimmt
   Teile.
4. **Herkunft und Rückweg** — Schritte 13–14. Danach tragen Zweig und Fork ihre
   Herkunft, und ein Rücksprung steht am Vorhaben, bis es wieder daran vorbei ist.
5. **Nichts wird still überschrieben** — Schritte 15–16. Danach schreibt weder ein
   überholter Stand noch eine ältere Programmfassung.

Die Scheiben 1 bis 3 sind zusammen die „kleinste vollständige Scheibe“ des
Fachkonzepts. Die Scheiben 4 und 5 machen sichtbar und sicher, was bis dahin
gebaut ist.

## Schritte

Der Stack steht in `architektur.md` und wird hier nicht wiederholt: JavaScript
(ESM), Node, das DSH-Plugin, Dateien und Git, kein eigener Dienst. Die Schritte
sagen, was danach da ist und wie man es prüft; wie es innen aussieht, entscheidet
der Ausführende. Nach jedem Schritt bleibt benutzbar, was vorher da war.

Pfade ohne Präfix liegen im Ordner des Vorhabens (`produkt/`, `transkripte/`);
`personen/`, `workflows/`, `skills/`, `vorgaenge/`, `beitraege/` und `format` liegen
in der Ablage `projekte/board/`; `$DSH_HOME` ist das Verzeichnis des Harness.

## Schritt 1 — Das Board wird ein Produkt dieses Vorhabens

- **Scheibe:** 1 — Zwei Personen sehen einander.
- **Ziel:** Das laufende Board-Plugin liegt im Ordner dieses Vorhabens, und der
  Harness lädt es von dort.
- **Eingang:** `plugins/dsh-board/` (das heute geladene Plugin); `architektur.md`,
  Abschnitt „Harness“ (Manifest, Profil, Installation).
- **Tun:** die Plugin-Dateien nach `produkt/` übernehmen; das Profil lädt
  `@local/dsh-board` von dort; der alte Ordner bleibt als Rückweg liegen und wird
  nicht mehr bearbeitet.
- **Ergebnis:** `produkt/` mit `package.json`, `index.js`, `client.js`,
  `cordis.patch.yml`, `locale/`, `icon.svg`; das Board in der Seitenleiste kommt
  von dort.
- **Prüfung:** Board öffnen → dieselben Stationen, dieselben Vorhaben, Leser,
  Verlauf und Gesprächsspalte wie vorher. Der Verweis im Profil löst nach
  `produkt/` auf. Den alten Ordner kurz umbenennen → das Board bleibt nach dem
  Neuladen; zurückbenennen.
- **Stopp:** ja — von hier an ist das Board, mit dem gearbeitet wird, der Gegenstand
  des Vorhabens; ein Fehler in einem Schritt nimmt es weg.

## Schritt 2 — Die Ablage ist ein eigenes Repository mit meiner Person

- **Scheibe:** 1 — Zwei Personen sehen einander.
- **Ziel:** Das Board weiß, wer es benutzt und wo seine Ablage liegt: in einem
  eigenen Git-Repository, dessen Fassungen den eigenen Namen tragen.
- **Eingang:** `produkt/` (Schritt 1); `projekte/board/` mit `board.json` und
  `vorgaenge/`.
- **Tun:** `produkt/board.config.json` mit `{ person, repo }`; `projekte/board/`
  wird ein eigenes Repository und nimmt den heutigen Inhalt auf; alle
  Schreibvorgänge des Boards laufen darin; Urheber einer Fassung ist die Person,
  bei Modellläufen „Modell (<Person>)“; der Arbeitsbereich trägt `projekte/board/`
  nicht mehr in seiner eigenen Geschichte mit; das Produkt gehört nicht in die
  Ablage.
- **Ergebnis:** `projekte/board/.git`, `produkt/board.config.json`; jede
  Speicherung im Board ist eine Fassung mit eigenem Namen.
- **Prüfung:** Im Board eine Datei speichern → `git -C projekte/board log -1
  --format=%an` nennt den eigenen Namen; `git -C /workspace status` bleibt ruhig.
  Board neu laden → es steht unverändert da.
- **Stopp:** ja — die Board-Daten verlassen die Geschichte des Arbeitsbereichs;
  der Rückweg ist ein Verschieben, kein Zurücknehmen.

## Schritt 3 — Die Ablage trägt Personen und Workflows

- **Scheibe:** 1 — Zwei Personen sehen einander.
- **Ziel:** Die Ablage hat die Form, die zwischen zwei Installationen gilt:
  Personen, Workflows, Vorhaben und Fassungen als Dateien; `board.json` gibt es
  nicht mehr.
- **Eingang:** `projekte/board/` als Repository mit `board.json` und `vorgaenge/`
  (Schritt 2); `architektur.md`, Abschnitt „Ablage im Repository“.
- **Tun:** `format` schreiben; `personen/<kennung>.json` und
  `workflows/<kennung>.json` aus dem heutigen Board ableiten — ein Schritt trägt
  `id`, `name`, `host`, `eingang`, `ausgabe`, `wie`, `skill`, ein fremder Schritt
  steht als Verweis; jedes Vorhaben bekommt
  `vorgaenge/<ordner>/vorhaben.json` mit `verantwortliche`, `regel`, `rueckkehr`,
  `laeufe`, `fork_von`, `zweig_von`; Leser, Verlauf, Vergleich und Transkript
  arbeiten auf denselben Dateien weiter; Einstellungen bleiben persönlich und damit
  außerhalb der Ablage.
- **Ergebnis:** `projekte/board/format`, `personen/`, `workflows/`,
  `vorgaenge/<ordner>/vorhaben.json`; `board.json` ist gelöscht; eine Fassung hält
  den Umzug fest.
- **Prüfung:** Board öffnen → dieselben Stationen und Vorhaben; der Leser öffnet
  `architektur.md`; der Verlauf zeigt die Fassungen mit Urheber; der Vergleich
  zweier Fassungen zeigt den Unterschied; das letzte Transkript liegt unverändert
  am Vorhaben. `projekte/board/board.json` gibt es nicht mehr.
- **Stopp:** ja — die Form ist die Schnittstelle zwischen beiden Installationen;
  eine Änderung daran verlangt später einen Umzug auf beiden Rechnern.

## Schritt 4 — Der Abgleich holt und gibt

- **Scheibe:** 1 — Zwei Personen sehen einander.
- **Ziel:** Das Board hält seinen Stand mit dem Remote auseinander: es holt vor dem
  Lesen, gibt nach dem Schreiben und sagt, wie frisch der Stand ist.
- **Eingang:** `projekte/board/` als Repository (Schritt 2); `format`, `personen/`,
  `workflows/`, `vorgaenge/` (Schritt 3).
- **Tun:** `git fetch`, `git pull --rebase` und `git push` über den Abgleich;
  `stand` in `/board/data` mit Zeitpunkt und der Zahl der Fassungen, die nur lokal
  liegen; ohne Netz oder ohne Remote öffnet das Board trotzdem und sagt, dass es
  nicht abgeglichen ist; ein gescheiterter Abgleich lässt den lokalen Stand stehen.
  Der Remote ist eine Adresse; für die Prüfung genügt eines auf der Platte.
- **Ergebnis:** die Route `/board/sync`; in der Kopfzeile „zuletzt abgeglichen …“
  und „n Fassungen nur hier“.
- **Prüfung:** mit einem nackten Repository — einem ohne Arbeitsdateien — als
  `origin` und einem zweiten Klon daneben: dort eine Datei ändern, festschreiben,
  schieben; im Board abgleichen → die Änderung steht da und die Anzeige nennt den
  Zeitpunkt. Im Board speichern, abgleichen → die Fassung liegt im Remote. `origin`
  wegnehmen, Board neu laden → das Board öffnet sich, die Anzeige sagt „nicht
  abgeglichen“.
- **Stopp:** ja — der erste Weg nach draußen; von hier an verlassen Board-Daten den
  Rechner.

## Schritt 5 — Der Workflow der anderen Person steht auf meinem Board

- **Scheibe:** 1 — Zwei Personen sehen einander.
- **Ziel:** Das Board zeigt die Workflows aller Personen; fremde Schritte tragen den
  Namen ihres Hosts und werden nicht kopiert.
- **Eingang:** `workflows/<ich>.json` (Schritt 3); Abgleich (Schritt 4).
- **Tun:** alle Workflow-Dateien lesen; jede Person bekommt ihre Fläche, die eigene
  zuerst; ein Schritt gehört seinem Host — ändern lässt er sich nur bei ihm;
  fremde Schritte sind sichtbar, aber nicht bearbeitbar; ein Verweis auf einen
  fremden Schritt ist erlaubt, aber nur als Verweis.
- **Ergebnis:** `/board/data` liefert `workflows` statt eines Workflows; die Seite
  zeigt die Fläche der anderen Person mit ihren Schritten.
- **Prüfung:** im zweiten Klon `personen/<zweite>.json` und
  `workflows/<zweite>.json` mit zwei Schritten anlegen (der Name der anderen Person
  als Host), schieben, im Board abgleichen → die Schritte stehen mit diesem Namen
  da, die fremde Fläche ist sichtbar; ein Klick auf einen fremden Schritt zeigt
  Ein- und Ausgang, aber kein Feld zum Ändern. Der eigene Workflow ist unverändert.
- **Stopp:** nein.

## Schritt 6 — Jedes Vorhaben nennt seinen Verantwortlichen

- **Scheibe:** 1 — Zwei Personen sehen einander.
- **Ziel:** An jedem Vorhaben steht, wer gerade verantwortet; nur dieser Person
  werden die Handlungen angeboten.
- **Eingang:** `vorgaenge/<ordner>/vorhaben.json` (Schritt 3); `personen/`
  (Schritt 3).
- **Tun:** `verantwortliche` als Zuordnung Schritt → Personen, `regel` als Feld
  daneben; die Seite zeigt „verantwortet von …“; die Handlungen am Vorhaben werden
  an einer Stelle geprüft — was später dazukommt (annehmen, zurückschicken), geht
  durch dieselbe Prüfung; beim Anlegen ist der Anlegende verantwortlich; beim
  Weitergeben trägt der Abgebende ein, wer den nächsten Abschnitt übernimmt.
- **Ergebnis:** Verantwortliche in `vorhaben.json`; die Anzeige am Vorhaben; die
  Prüfung, die Handlungen einschränkt.
- **Prüfung:** eigenes Vorhaben → „verantwortet von <ich>“, das Weitergeben ist da.
  Im zweiten Klon ein Vorhaben mit der anderen Person als verantwortliche Person
  anlegen, schieben, abgleichen → es erscheint, das Weitergeben fehlt.
- **Stopp:** nein.

## Schritt 7 — Ein fremdes Vorhaben steht auf meinem Board und lässt sich lesen

- **Scheibe:** 1 — Zwei Personen sehen einander.
- **Ziel:** Fremde Vorhaben sind sichtbar, anklickbar und lesbar; sie gehören nicht
  mir, also ändert sich an ihnen nichts.
- **Eingang:** fremde Workflows (Schritt 5); Verantwortliche (Schritt 6).
- **Tun:** alle `vorgaenge/<ordner>/vorhaben.json` lesen, nicht nur die eigenen; das
  Vorhaben steht an seiner Station, in der Farbe seiner Person, mit dem
  Verantwortlichen; der Leser zeigt seine Dateien; umbenennen und ziehen wird nicht
  angeboten; in die Dateien eines fremden Vorhabens lässt sich nicht schreiben;
  gezeigt wird nur, was im Ordner liegt.
- **Ergebnis:** fremde Vorhaben auf der Fläche; ihr Ordner ist lesbar; keine
  Eingriffe.
- **Prüfung:** im zweiten Klon ein Vorhaben der anderen Person anlegen (Ordner mit
  `idee.md`), schieben, abgleichen → es steht an der Station der anderen Person,
  mit ihrem Namen; ein Klick öffnet `idee.md`; „Umbenennen“ und das Ziehen
  fehlen, ein Speichern in die Datei der anderen Person wird nicht angeboten.
- **Stopp:** nein.

## Schritt 8 — Der Skill eines Schritts liegt in der Ablage

- **Scheibe:** 2 — Ein fremder Schritt läuft bei mir.
- **Ziel:** Ein Schritt trägt seine Anweisung als Datei in der Ablage; wer ihn
  benutzt, legt sie für den Lauf unter dem Namen des Schritts im Harness ab und
  nimmt sie danach wieder weg.
- **Eingang:** `skills/` in der Ablage (Schritt 3); `$DSH_HOME/skills`; der heutige
  Skill-Lauf und die Werkstatt.
- **Tun:** `skills/<person>/<schritt>.md` als Ort der Anweisung; die Werkstatt
  schreibt dorthin; beim Start landet die Datei als `<host>-<schritt>` in
  `$DSH_HOME/skills`, nach dem Lauf wird sie entfernt; das Transkript entsteht wie
  bisher im Ordner des Vorhabens.
- **Ergebnis:** die eigene Skill-Datei in der Ablage; ein Lauf, der seine Anweisung
  von dort nimmt.
- **Prüfung:** an einem eigenen Schritt den Skill starten → während des Laufs liegt
  `$DSH_HOME/skills/<host>-<schritt>/SKILL.md`, danach nicht mehr; das Transkript
  liegt unter `transkripte/` und erscheint am Schritt.
- **Stopp:** nein.

## Schritt 9 — Mein Workflow enthält einen fremden Schritt

- **Scheibe:** 2 — Ein fremder Schritt läuft bei mir.
- **Ziel:** Ein fremder Schritt lässt sich in den eigenen Workflow stellen; er
  bleibt der Schritt seines Hosts.
- **Eingang:** fremde Workflows (Schritt 5); eigene Workflow-Datei (Schritt 3).
- **Tun:** im eigenen Workflow ein Verweis auf den fremden Schritt; die Seite zeigt
  ihn mit Namen, Host und Ein- und Ausgang; kein Ändern, kein Löschen; ein
  Vorhaben kann auf ihm stehen.
- **Ergebnis:** der Verweis in `workflows/<ich>.json`; die fremde Station auf der
  eigenen Fläche.
- **Prüfung:** einen fremden Schritt in den eigenen Workflow stellen, Seite neu
  laden → er steht da, mit „bei <Person>“; seine Einstellungen lassen sich nicht
  öffnen; die Datei der anderen Person ist unverändert (`git -C projekte/board
  status` zeigt an deren Workflow nichts).
- **Stopp:** nein.

## Schritt 10 — Der Skill der anderen Person läuft in meinem Harness

- **Scheibe:** 2 — Ein fremder Schritt läuft bei mir.
- **Ziel:** Ein Vorhaben an einem fremden Schritt kann dessen Skill im eigenen
  Harness ausführen; die Anweisung kommt mit der Ablage, das Transkript bleibt beim
  eigenen Vorhaben.
- **Eingang:** Verweis (Schritt 9); `skills/<fremd>/<schritt>.md` (Schritt 8).
- **Tun:** beim Start den Skill des fremden Schritts aus der Ablage nehmen und als
  `<host>-<schritt>` einsetzen; die Sitzung im eigenen Arbeitsbereich starten; das
  Transkript im eigenen Vorhaben ablegen; den Einsatz danach wieder wegnehmen.
- **Ergebnis:** ein Lauf eines fremden Schritts; ein Transkript im eigenen
  Vorhaben; die Ablage unverändert.
- **Prüfung:** ein Vorhaben auf den fremden Schritt ziehen, den Skill starten → das
  Gespräch folgt der Anweisung aus der Ablage, nicht dem Chat; nach dem Lauf liegt
  das Transkript unter `transkripte/` des eigenen Vorhabens; `git -C projekte/board
  status` ist ruhig.
- **Stopp:** ja — hier laufen fremde Anweisungen im eigenen Harness; was sie tun,
  lässt sich nicht zurücknehmen.

## Schritt 11 — Am fremden Vorhaben speichert der Editor als Beitrag

- **Scheibe:** 3 — Beitragen und annehmen.
- **Ziel:** Wer nicht verantwortet, schreibt nicht in das fremde Vorhaben, sondern
  legt einen Beitrag an: eine Notiz und, wenn nötig, eine geänderte Fassung der
  Datei.
- **Eingang:** fremdes Vorhaben (Schritt 7); `beitraege/` in der Ablage (Schritt 3);
  Verantwortliche (Schritt 6).
- **Tun:** im Leser eines fremden Vorhabens öffnet „Bearbeiten“ den Text; „Als
  Beitrag speichern“ legt `beitraege/<vorhaben>/<beitrag>.json` mit `id`, `schritt`,
  `von`, `seit`, `basis`, `notiz`, `dateien` an und die geänderte Fassung unter
  `beitraege/<vorhaben>/<beitrag>/<datei>.md`; die Originaldatei bleibt unberührt;
  der Beitrag erscheint an der Station mit dem Namen des Beitragenden.
- **Ergebnis:** Beitragsdateien; der fremde Ordner unverändert; der Beitrag
  sichtbar.
- **Prüfung:** an einem fremden Vorhaben `vision.md` bearbeiten, als Beitrag
  speichern → ein Beitrag erscheint mit Namen und Notiz; `git -C projekte/board
  status` zeigt an `vorgaenge/<ordner>/vision.md` nichts; die Beitragsdatei liegt
  unter `beitraege/`.
- **Stopp:** nein.

## Schritt 12 — Der Verantwortliche entscheidet über den Beitrag

- **Scheibe:** 3 — Beitragen und annehmen.
- **Ziel:** Der Verantwortliche nimmt einen Beitrag an, lehnt ihn ab oder nimmt
  Teile; erst dann ändert sich sein Stand.
- **Eingang:** ein Beitrag (Schritt 11); Verantwortung (Schritt 6).
- **Tun:** `entschieden` mit `{ von, am, art, teile }`; annehmen schreibt die
  vorgeschlagenen Dateien in den Ordner des Vorhabens und hält das als neue Fassung
  fest; ablehnen lässt alles, wie es ist; Teile übernehmen nur die genannten
  Dateien; der Beitrag bleibt mit dem Entscheid sichtbar.
- **Ergebnis:** die übernommene Fassung im Vorhaben, `entschieden` am Beitrag, eine
  Fassung mit Urheber.
- **Prüfung:** einen Beitrag annehmen → die Datei im Vorhaben trägt den
  vorgeschlagenen Text, `entschieden.von` nennt den Verantwortlichen, der Verlauf
  zeigt eine neue Fassung. Ein zweiter Beitrag mit zwei Dateien, nur eine annehmen
  → nur sie ändert sich. Einen dritten ablehnen → sein Stand bleibt, der Beitrag
  steht als abgelehnt da.
- **Stopp:** nein.

## Schritt 13 — Zweig und Fork tragen ihre Herkunft

- **Scheibe:** 4 — Herkunft und Rückweg.
- **Ziel:** Ein Vorhaben, das aus einem anderen entstanden ist, sagt es; das
  Original bleibt, wie es war.
- **Eingang:** `fork_von`, `zweig_von` in `vorhaben.json` (Schritt 3); das heutige
  Abzweigen.
- **Tun:** beim Abzweigen `zweig_von` mit Ursprung und Station, beim Fork `fork_von`
  eintragen; die Seite zeigt „Zweig von … ab …“ und „Fork von …“ am Vorhaben; das
  Original bleibt unverändert.
- **Ergebnis:** die Herkunft im Vorhaben und an der Karte.
- **Prüfung:** von einem Vorhaben einen Zweig anlegen → der Zweig trägt die
  Herkunft, das Original ist in Dateien und Station unverändert. Ein Fork eines
  fremden Vorhabens zeigt dessen Namen.
- **Stopp:** nein.

## Schritt 14 — Der Rücksprung steht am Vorhaben

- **Scheibe:** 4 — Herkunft und Rückweg.
- **Ziel:** Ein offener Punkt schickt ein Vorhaben an einen früheren Schritt zurück;
  der Grund steht am Vorhaben, bis es wieder daran vorbei ist.
- **Eingang:** die heutige Rückkehr (`rueckkehr` in `vorhaben.json`, Schritt 3); das
  Weitergeben (Schritt 6).
- **Tun:** `{ von, nach, grund, seit, erledigt }` am Vorhaben; die Seite zeigt den
  offenen Rücksprung mit Ziel und Grund; ein Weiterziehen über das Ziel setzt
  `erledigt`, und der Vermerk verschwindet aus der Anzeige, bleibt aber in der
  Datei.
- **Ergebnis:** der Vermerk am Vorhaben, sichtbar in der Ansicht.
- **Prüfung:** ein Vorhaben zurückschicken → es steht am früheren Schritt, der Grund
  steht daran, der Weg dorthin ist als offen markiert. Es wieder über den Schritt
  ziehen → der Vermerk ist erledigt und nicht mehr zu sehen; `vorhaben.json` trägt
  ihn mit `erledigt: true`.
- **Stopp:** nein.

## Schritt 15 — Speichern auf einem überholten Stand schreibt nicht

- **Scheibe:** 5 — Nichts wird still überschrieben.
- **Ziel:** Wer eine Datei offen hat, während ein anderer sie ändert, überschreibt
  nichts; sein Text bleibt und wird als Beitrag angeboten.
- **Eingang:** `/board/doc` mit `basis` (Schritt 3); Beiträge (Schritt 11).
- **Tun:** beim Öffnen die Fassung der Datei mitgeben; beim Speichern prüfen, ob die
  Datei seither unverändert ist; wenn nicht, nicht schreiben, sondern den neuen
  Stand zurückgeben; der eigene Text bleibt im Editor und lässt sich als Beitrag
  anlegen; nichts wird zusammengeführt.
- **Ergebnis:** die Prüfung vor dem Schreiben; der Weg „mein Text als Beitrag“.
- **Prüfung:** über den zweiten Klon eine neue Fassung von `vision.md` schieben; im
  Board die alte Fassung öffnen, ändern, speichern → die Meldung sagt, dass der
  Stand neuer ist, der eigene Text steht noch da, ein Knopf macht daraus einen
  Beitrag; die Datei im Vorhaben ist unverändert.
- **Stopp:** nein.

## Schritt 16 — Zwei Programmfassungen schreiben nicht gegeneinander

- **Scheibe:** 5 — Nichts wird still überschrieben.
- **Ziel:** Hat die Ablage eine neuere Form als das eigene Programm, schreibt es
  nicht, sondern sagt es.
- **Eingang:** `format` (Schritt 3); Abgleich (Schritt 4).
- **Tun:** `format` beim Lesen und vor jedem Schreiben vergleichen; ist die eigene
  Fassung älter, wird nicht geschrieben und der Grund genannt; die Anzeige sagt,
  welche Fassung die Ablage verlangt.
- **Ergebnis:** die Prüfung vor jedem Schreiben; eine Meldung statt eines stillen
  Unterschieds zwischen zwei Rechnern.
- **Prüfung:** `format` in der Ablage auf eine höhere Zahl setzen und schieben; im
  Board etwas speichern → es wird nicht geschrieben, die Meldung nennt die Fassung
  der Ablage; `git -C projekte/board status` bleibt ruhig. `format` zurückstellen →
  Speichern geht wieder.
- **Stopp:** nein.

## Zurückgeschickt

Nichts. Kein Schritt wartet auf eine Antwort.

Die sechs offenen Fragen des Fachkonzepts halten die Reihenfolge nicht auf:
Beitragsform und gleichzeitige Änderung sind in `architektur.md` entschieden und
stehen als Schritte 11, 12 und 15; die Regel für mehrere Verantwortliche ist ein
Feld am Vorhaben, das kein Schritt benutzt; ein unbeantworteter Beitrag bleibt
sichtbar, und das ist der gebaute Zustand (Schritte 11 und 12); die Farbe berührt
keinen Schritt; der Rücksprung ist ein Vermerk (Schritt 14), und das
Zurückschicken ist ein Weitergeben an einen früheren Schritt, also dieselbe Prüfung
wie in Schritt 6.

Fällt eine dieser Antworten später anders aus, ändert sich ein Feld oder ein
Schritt, nicht die Reihenfolge.
