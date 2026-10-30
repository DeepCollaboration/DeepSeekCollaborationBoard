# Schritt 91c — Bericht

## Gebaut

- **Merker im Board.** `vorlagen/start/board-start/board.json` trägt `"einstellungen": { "einfuehrung": "offen" }`. `POST /board/einstellung` nimmt `einfuehrung` an, erlaubt nur `offen` und `uebersprungen`, schreibt über `schreibeFassung2Einstellung` und fasst nur `board.json` (Status, `add`, `commit` mit „Einführung geöffnet“ bzw. „Einführung übersprungen“). Jeder andere Wert antwortet 400 ohne Datei und ohne Fassung; ein zweiter Aufruf überschreibt sauber. `GET /board/data` reichte `einstellungen` bereits durch (`komponiere` → `datenAntwort`) — unverändert, durch die Prüfung belegt.
- **Beim Start.** `client.js` öffnet einmal je Sitzung den Reiter des Workflows `einfuehrung` und dessen erste Position („Vorhaben und Ideen“) im Dock, wenn `data.einstellungen.einfuehrung === 'offen'`; der Merker `gemerkt.einfuehrungGeoeffnet` überlebt einen Wechsel zum Chat. Fehlt der Workflow, bleibt alles wie bisher.
- **Überspringen.** Solange der Einführungsreiter vorne steht, liegt über der Fläche eine schmale Zeile: „Einführung“ links, „Überspringen“ rechts. Der Knopf schreibt zuerst `uebersprungen`, dann wechselt `wechsleReiter(null)` auf den öffentlichen Reiter; scheitert das Schreiben, bleibt der Reiter stehen und die Statusmeldung nennt den Grund.
- **Wieder öffnen.** Rechts neben „Alles zeigen“ steht ein kleiner Knopf mit Fragezeichen und `aria-label` „Einführung“. Er öffnet Reiter und erste Station jederzeit und schreibt `offen` ohne auf die Antwort zu warten.
- **Token an der Stelle.** `formate/station.js` lässt für `knopf.ziel` neben `weiter` auch `token` zu. Die Vorlage `station-eigene` trägt „Token eintragen“ (`ziel: 'token'`) vor dem Doku-Knopf. Der Client öffnet dafür den vorhandenen Zustand `tokenWeg` (`{ offen: true, token: '', laeuft: false, fehler: '' }`, derselbe Speicherweg `tokenSpeichern`); die aktive Ansicht bleibt offen, das Feld steht über ihrem Inhalt, Escape schließt nur das Feld. Ist ein Token gesetzt (`data.anlegen.darf`), steht statt des Knopfs `uebersetze('Token ist gesetzt')`.
- **Texte.** Vier neue Schlüssel über `uebersetze` in `client.js` und in allen sieben Wörterbüchern; Schlüsselzahl 624 → 628, Client-Anteil 615 → 619.
- **Prüfung.** `test/nach91c.mjs` (neu, selbstständig, echter HTTP-Server auf 127.0.0.1, Board unter `/tmp/board-scheibe1/nach91c`): Vorlage und `liesBoard`, frisches Startboard über `komponiere` (Einführung offen, erste Station „Vorhaben und Ideen“, Token-Knopf am Schritt „Eigene Station“), Route mit `offen`/`uebersprungen`/`quatsch` samt Commit-Zählung und Überschreiben, `pruefeStation` mit `token` und `javascript:`, `client.js` als Text (Start, Überspringen samt Schreibweg, `aria-label`, `ziel === 'token'`, „Token ist gesetzt“), `/board/data` mit `einstellungen`, Knopfreihenfolge und die vier Texte in sieben Wörterbüchern.

## Geänderte Dateien

- `vorlagen/start/board-start/board.json`, `vorlagen/start/station-eigene/station.json`
- `formate/station.js` (`pruefeModul`: `ziel: 'token'`)
- `index.js` (`POST /board/einstellung`: Zweig `einfuehrung`)
- `client.js` (Reiter-Helfer, Start-Effekt, `oeffneEinfuehrung`, `einfuehrungUeberspringen`, Einführungszeile, Fragezeichen-Knopf, Token-Zweig im Modul, Token-Feld über der aktiven Ansicht)
- `texte/en.json`, `texte/es.json`, `texte/fr.json`, `texte/ru.json`, `texte/ja.json`, `texte/ar.json`, `texte/zh.json`
- `test/nach91c.mjs` (neu)
- `test/nach90.mjs` (fünf Blöcke der „Eigenen Station“, Doku-Knopf nicht mehr der erste Nicht-`weiter`-Knopf)
- `test/grundlagen.mjs`, `test/nach82.mjs`, `test/nach83.mjs`, `test/nach84.mjs`, `test/nach85.mjs`, `test/nach85b.mjs` (Schlüsselzahl 628)
- `docs/entwicklung.md` (Prüfzahlen 92/58, Schlüssel 628/619), `docs/routen.md`, `docs/datenformate.md`
- `.github/workflows/ci.yml` (Zahl im Kommentar: 58)

## Prüfung

```
cd /workspace/projekte/board-plugin && npm run check && node --test test/*.mjs
```

Exit-Code: **0** — `check` grün, 92 Tests, 92 bestanden, 0 fehlgeschlagen (die bisherigen 91 plus `nach91c`).

## Entscheidende Zeile

`client.js:3723–3724` — `if (einstellungen.einfuehrung !== 'offen') return;` / `oeffneEinfuehrung(false);`

Der Start liest den Merker aus `data.einstellungen`, öffnet genau einmal den Einführungsreiter samt erster Station und lässt danach jede Wahl des Menschen stehen; geschrieben wird derselbe Merker in `index.js:7245` mit `schreibeFassung2Einstellung('einfuehrung', wert);`.
