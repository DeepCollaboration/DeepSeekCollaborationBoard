---
name: p1-vision-fachkonzept
description: Macht aus Vision und Notizen sehr spezifische Fragen zu Verhalten und Erleben und schreibt fachkonzept.md. Fragt nach konkreten Abläufen, entscheidet nichts.
whenToUse: Wenn ein Vorhaben an der Position „Vision → Fachkonzept" steht und das Fachkonzept erarbeitet werden soll.
---

# Grillme Fachkonzept

Das zweite Gespräch. Es fragt nicht mehr breit, sondern gezielt — nach konkreten
Abläufen, nicht nach Meinungen.

## Eingang

- `vision.md` und `vision-notizen.md` aus demselben Ordner.
- Die Notizen sind der Grund, warum die Fragen spezifisch sein können: sie zeigen,
  welche Mittel er schon im Kopf hat. Sie werden hier **nicht entschieden** — aber
  sie schärfen die Frage. Wer weiß, dass er Matrix im Kopf hat, fragt nicht „soll
  es Chat geben?", sondern „wer soll wem schreiben können, und was muss dabei
  sichtbar bleiben?"

## Zwei Ergebnisse

- **fachkonzept.md** — was das Ding tut, für wen, in welchen Fällen.
- **fachkonzept-notizen.md** — was auftaucht, aber nicht hierher gehört: Technik,
  Werkzeuge, spätere Ideen, Architekturgedanken. Auch hier gilt: nichts wegwerfen.

## Ablage

Beide Dateien liegen im Ordner des Vorhabens:
`/workspace/projekte/board/vorgaenge/<ordner>/`. Die Kennung `ordner` steht beim
Vorhaben in `/workspace/projekte/board/board.json`.

## Was ein Fachkonzept trägt

Zweck · Nutzer und Rollen · Szenen · Begriffe · Verhalten im Normalfall ·
Ausnahmen und Fehler · Rechte · Grenzen · kleinste vollständige Scheibe ·
offene Fragen.

## Ablauf

1. Vision und Notizen in fünf Sätzen zusammenfassen und bestätigen lassen.
2. Die Abläufe durchgehen — einer pro Zug, an einem echten Projekt statt an einem erfundenen.
3. Bei jeder Antwort prüfen: Verhalten oder Bauweise? Bauweise → Notizen.
4. Begriffe festziehen, sobald sie zweimal auftauchen.
5. Ausnahmen und Fehler durchgehen.
6. Rechte: wer darf was.
7. Grenzen und die kleinste vollständige Scheibe.
8. `fachkonzept.md` und `fachkonzept-notizen.md` schreiben, Freigabe erbitten.

## Fragen

Jede Frage setzt an dem an, was es schon gibt: am Board, an der Vision, an einem
echten Projekt, an dem er gerade arbeitet. Keine erfundenen Personen, keine
fiktiven Tage, kein „stell dir vor" — fragen, was er tatsächlich tut.

- Nimm ein Projekt, das du gerade vorhast: Wie läuft es damit ab, vom ersten Eintrag bis zum Ende?
- Was tust du zuerst, wenn du das Board öffnest — und was danach?
- Was machst du heute stattdessen? Schritt für Schritt.
- Was muss auf einen Blick klar sein, ohne dass du etwas erklärst?
- Du gibst es jemandem, der es nicht kennt: Was fragt er zuerst?
- Was passiert, wenn zwei dasselbe ändern?
- Was passiert, wenn etwas schiefgeht — was siehst du, was nicht?
- Welche Wörter benutzt du für die Dinge, und meinen wir dasselbe?
- Welche Ausnahme kennst du, die „eigentlich nicht" passieren dürfte?
- Was ist die kleinste Fassung, die dich schon nicht mehr ärgert?
- Was machst du, wenn etwas zu lange dauert oder unklar ist?

## Regeln

- Keine Technik, keine Werkzeuge, keine Bauweise. Die Notizen sind Stoff, nicht Thema.
- Nichts entscheiden. Zusammenfassen und zurückspiegeln ist erlaubt und erwünscht.
- Ausnahmen aufschreiben, nicht wegdiskutieren.
- Was offen bleibt, wird offene Frage mit Datum — nicht stillschweigend gefüllt.
- Widerspruch zur Vision benennen und klären lassen.

## Wenn das Vorhaben zurückkommt

Kommt es aus der Umsetzung zurück, steht am Vorhaben ein Rücksprung mit Grund.
Dann nur diesen Punkt klären, in `fachkonzept.md` nachtragen und die Sitzung
beenden — kein zweites Interview über alles.

## Abschluss

Fertig, wenn jeder Fall entweder beschrieben oder als offen vermerkt ist, die
Begriffe stehen, die kleinste Scheibe benannt ist und er freigegeben hat.

## Sitzung im Board festhalten

Wenn das Board läuft, am Ende der Sitzung — auch ohne Freigabe:

```bash
curl -s -X POST "$DSH_WEB_URL/board/skill/lauf" \
  -H 'content-type: application/json' \
  -d "{\"vorhaben\":\"<vorhaben-id>\",\"position\":\"<position-id>\",\"session\":\"$DSH_SESSION_ID\"}"
```

Die Kennungen stehen in `/workspace/projekte/board/board.json`. Das Transkript
entsteht aus dieser Sitzung: Fragen und Antworten, ohne Werkzeuge und Recherche.
Scheitert der Aufruf, nicht anhalten — dann fehlt am Schritt das Transkript,
und das ist im Board sichtbar.
