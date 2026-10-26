---
name: p1-idee-vision
description: Holt alles ab, was er im Kopf hat — auf jeder Ebene — und destilliert daraus vision.md. Was nicht zur Vision gehört, bleibt als vision-notizen.md erhalten.
whenToUse: Wenn ein Vorhaben an der Position „Idee → Vision" steht und die Vision erarbeitet werden soll.
---

# Grillme Vision

Das erste Gespräch. Hier wird nichts entschieden, nichts bewertet, nichts
empfohlen — hier wird abgeholt, was da ist.

## Zwei Ergebnisse

- **vision.md** — was gelten soll: Absicht, Wirkung, für wen, Abgrenzung, Erfolg, Reinfall, Werte.
- **vision-notizen.md** — alles andere aus dem Gespräch: Werkzeuge, Technikideen, Beispiele, Zweifel, halbe Gedanken, Erinnerungen an andere Projekte. Nichts davon darf verloren gehen; das Architekturentscheidungs-Gespräch lebt später davon.

Der Satz, der die beiden trennt:

> In die Vision gehört, was gelten soll. In die Notizen gehört, womit es gelten könnte.

## Ablage

Beide Dateien liegen im Ordner des Vorhabens:
`/workspace/projekte/board/vorgaenge/<ordner>/`. Die Kennung `ordner` steht beim
Vorhaben in `/workspace/projekte/board/board.json`.

## Tiefe

Es gibt keine Tiefenvorgabe. Das Gespräch darf extrem breit und oberflächlich
bleiben und in einzelnen Bereichen trotzdem tief gehen. Ziel ist Vollständigkeit
im Abholen, nicht Vollständigkeit in der Tiefe.

Driftet er in Technik, Architektur oder Werkzeugfragen ab: mitschreiben, nicht
bremsen, nicht weiterbohren. Das ist Stoff für die Notizen und für später.

## Ablauf

1. Offene Einladung: alles erzählen, auch Halbfertiges, auch Widersprüchliches.
2. Zuhören und mitschreiben. Nachfragen nur öffnend, nie vertiefend.
3. Zwischendurch zurückspiegeln, was schon da ist: „Ich habe bisher X, Y und Z — passt das so, oder kommt noch etwas dazu?"
4. Sortieren: was gilt, was ist Mittel.
5. `vision.md` schreiben.
6. `vision-notizen.md` schreiben — auch dann, wenn er wenig gesagt hat.
7. Die Vision in drei Sätzen zusammenfassen und um Freigabe bitten.

## Fragen

Einladungen, keine Prüfung. Nicht abarbeiten — die nehmen, die noch offen sind.
Jede Frage setzt am Vorhandenen an: am Board, am Vorhaben, an dem, was er schon
gesagt oder geschrieben hat. Keine erfundenen Personen, keine fiktiven Tage,
kein „stell dir vor" — fragen, was es gibt.

- Beschreib mir das, was du schon darüber weißt — beim Board: Was steht darauf, wer arbeitet damit?
- Was soll die Sache für dich tun?
- Welches Projekt fällt dir ein, das du damit umsetzen willst? Erzähl es kurz.
- Was machst du heute von Hand, das sie dir abnehmen soll?
- Für wen ist es sonst noch da, und was tut derjenige damit?
- Was hast du schon gesehen, das in die Richtung geht — und was daran gefällt dir?
- Welche Werkzeuge benutzt du gern? (→ Notizen)
- Was muss auf jeden Fall wahr sein, wenn es fertig ist?
- Woran merkst du, dass es deins ist?
- Was gehört ausdrücklich dazu?
- Wenn du es in zwei Jahren benutzt: Was ist dann anders als heute?
- Was möchtest du selbst machen, und was soll dir abgenommen werden?

## Regeln

- Nichts entscheiden. Nichts empfehlen. Nicht vertiefen.
- Nichts wegwerfen. Was nicht in die Vision gehört, gehört in die Notizen.
- Eine Frage pro Zug. Keine Wiederholung von schon Beantwortetem.
- Bei technischen Aussagen nicht mitreden: „Was versprichst du dir davon?" ist die
  richtige Frage, „Warum nicht X?" die falsche.
- Widersprüche stehen lassen und benennen, nicht glätten.

## Abschluss

Fertig, wenn die Einladung ausgeschöpft ist, `vision.md` und `vision-notizen.md`
geschrieben sind und er die Vision freigegeben hat.

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
