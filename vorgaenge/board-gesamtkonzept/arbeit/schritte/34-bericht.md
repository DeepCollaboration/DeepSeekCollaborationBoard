# Bericht Schritt 34 — Der Katalog führt zusammen

## Gebaut

- **Herkunft an jedem Eintrag (`katalog.js`):** `eintragLesen` setzt für
  Einträge aus `katalog.json` `herkunft: "kuratiert"`; Suchtreffer tragen
  bereits `herkunft: "gefunden"`. `zusammenfuehren` setzt die Herkunft beim
  Zusammenführen erneut, sodass auch ein älterer Zwischenspeicher ohne das Feld
  in der Antwort eine eindeutige Herkunft hat. `kuratiert` und `gefunden`
  bleiben getrennte Listen; jede führt nur ihre eigene Herkunft.
- **Zusammenführen (`katalog.js`, `eintragSchluessel` und
  `zusammenfuehren`, Zeile 125–165):** Der Schlüssel ist `owner/repo`, klein
  geschrieben. Er kommt aus `eintrag.github` oder — nach dem Entfernen von
  `github` aus der Antwort — aus dem GitHub-Remote. `eintraege` enthält
  kuratiert zuerst, dann gefunden, je `owner/repo` einmal. Ein kuratierter
  Eintrag gewinnt und übernimmt keine Felder der gefundenen Fassung; das wird
  im Test mit abweichenden Sternen und einer abweichenden Beschreibung des
  Doppels belegt. Der schnelle Weg über `katalog-cache.json` nutzt dieselbe
  Zusammenführung.
- **Zwei Stände (`katalog.js` und `index.js`):** `sammleKatalog` gibt jetzt
  `stand` (letzte Sammlung) und `standGefunden` (letzte Suche) zurück. Aus dem
  Zwischenspeicher kommen `cache.am` und `cache.gefunden.am`; ohne Netz bleiben
  beide letzte Werte stehen, der Hinweis nennt den Suchstand. `GET
  /board/katalog` reicht beide Felder durch, auch bei „Kein Hauptboard“.
  `katalog.json` wird nur gelesen; gefundene Einträge werden nicht
  hineingeschrieben.
- **Neue Prüfung (`test/markt34.mjs`):** Temp-Heim und eigene Fläche unter
  `/tmp/board-scheibe1/markt34`; Hauptboard mit der kuratierten Station
  `station-y` (GitHub-Remote `kuratiert/station-y`, Fassung `v1`), daneben ein
  nachgebildeter GitHub-Server auf `127.0.0.1`. Er liefert `fremd/station-x`
  und das Doppel `kuratiert/station-y` mit absichtlich anderen Sternen (77)
  und anderer Beschreibung. Geprüft wird:
  1. `herkunft` an jedem Eintrag in `eintraege`, `kuratiert` und `gefunden`;
     die beiden Listen sind getrennt, `eintraege` ist kuratiert zuerst und je
     `owner/repo` einmal; der kuratierte Eintrag behält Sterne (3) und
     Beschreibung und trägt kein `web` der gefundenen Fassung.
  2. `stand` und `standGefunden` sind gesetzt und als Zeitpunkt lesbar; der
     Zwischenspeicher trägt `am` und `gefunden.am` getrennt.
  3. `?suche=` filtert `eintraege`, `kuratiert` und `gefunden` gleich — für
     „station“ (beide), „station-x“ (nur gefunden), „kuratiert“ und
     „KURATIERT“ (nur kuratiert) sowie „ohne-treffer“ (keine). Die
     Katalogsuche fragt den Server nicht erneut.
  4. Nach dem Schließen des Servers gelten der letzte Sammlungs- und der letzte
     Suchstand mit dem Hinweis „Ohne Verbindung: letzter Stand vom …“;
     `katalog.json` bleibt Byte-gleich und enthält kein `fremd/station-x`.
  Kein echtes Netz.

## Dateien

- `katalog.js` (geändert): `eintragSchluessel` (125–139),
  `zusammenfuehren` (141–165), `herkunft: 'kuratiert'` in `eintragLesen`
  (246), `katalogAusCache` (311–326), Zusammenführung und Rückgabe in
  `sammleKatalog` (557–583).
- `index.js` (geändert): `/board/katalog` trägt `standGefunden` und im Fall
  „Kein Hauptboard“ beide Stände als `null` (3332–3351).
- `test/markt34.mjs` (neu).
- Keine eigenen Commits.

## Prüfung

- `cd /workspace/projekte/board-plugin && node --test test/markt34.mjs`
  → **Exit-Code 0**; entscheidende Zeile:
  `ok Katalog: Herkunft, eine Liste, zwei Stände, keine Doppel`
- `cd /workspace/projekte/board-plugin && node --test` → **Exit-Code 0**;
  `ℹ tests 50`, `ℹ pass 50`, `ℹ fail 0` (49 vorhandene plus der neue Test).
- `node --check katalog.js` → **0**; `node --check index.js` → **0**;
  `node --check test/markt34.mjs` → **0**.

## Folge

Der Katalog antwortet mit einer zusammengeführten Liste und zwei getrennten
Ständen. Die Anzeige bleibt unverändert; `client.js` wurde nicht angefasst.
Welche Herkunft ein Eintrag hat, steht jetzt in der Antwort und kann in einem
folgenden Schritt sichtbar gemacht werden.

## Grenzen

- **Kein echtes Netz.** Der nachgebildete Server deckt Suche, Repository und
  Freigaben ab.
- **Kein Browserdurchgang.** Der laufende Server trägt den älteren Stand und
  wurde nicht benutzt; geprüft wurde mit `node --test`.
- **Keine eigenen Commits.**
