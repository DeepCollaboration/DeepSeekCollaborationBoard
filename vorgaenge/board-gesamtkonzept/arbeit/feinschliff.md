---
etage: Umsetzung
status: offen
offene_fragen: 0
stand: 2026-10-30
---

# Feinschliff — was „hübsch“ heißt

Vorbilder sind n8n und ComfyUI: links eine schmale Leiste, daneben ein Panel,
in der Mitte die Fläche, rechts das, was gerade gewählt ist; unten höchstens
eine schmale Werkzeugleiste. Die Fläche bleibt die Hauptsache, alles andere
tritt zurück.

## Regeln

- **Eine Zone, eine Aufgabe.** Keine zwei Bedienelemente für dieselbe Sache.
- **Ruhe durch Fläche.** Ein Ton je Zone, wenige Rahmen; Schatten nur für
  Schwebendes (Karten, offenes Panel), nicht für Flächen.
- **Abstände** 4 / 8 / 12 / 16 / 24. Eine Skala, keine Einzelwerte.
- **Schrift** 12 / 13 / 14 / 18 / 22 px, Gewicht 400 und 600.
- **Farbe** nur aus `var(--dsw-…)`; der Akzent ist für Auswahl, aktiven Reiter
  und Fokus da, nicht für Verzierung.
- **Bewegung** 120–160 ms, ease-out; `prefers-reduced-motion` schaltet sie ab.
- **Jeder Zustand ist sichtbar:** leer, lädt, Fehler, gesperrt. Nichts
  verschwindet ungesehen.

## Was konkret

1. **Karten.** Rand und ein weicher Schatten; beim Überfahren hebt sich die
   Karte 1 px; die offene Karte trägt einen 2-px-Ring im Akzent. Kopf mit
   Name, Fassungs-Chip und einem Statuspunkt (läuft / fertig / Fehler).
2. **Kanten.** 2 px, runde Ecken, kleiner Pfeil; überfahren macht sie dicker;
   die gewählte Kante nimmt den Akzent.
3. **Schiene.** 40 × 40 px Ziele, 18-px-Symbole; der aktive Knopf trägt einen
   2-px-Balken links und eine getönte Fläche; Tooltip mit Namen.
4. **Kopfzeile.** Chips 24 px hoch, Trenner 1 px, Suche mit Lupe; die
   Statuszeile darunter bleibt leise.
5. **Docks.** Reiterleiste, stehender Kopf, 12 px Innenabstand, Abschnitte
   durch eine 1-px-Linie getrennt, nicht durch Abstände allein.
6. **Fläche.** Punktraster 24 px, Ton aus dem Token; die Ecken der Fläche
   weich, ohne Rahmen.
7. **Leerzustand.** Ein Satz und zwei Wege (Token eintragen, eigenes Board) —
   keine leere weiße Wand.
8. **Fokus.** Ein 2-px-Ring, sichtbar auf jedem Knopf, Feld und jeder Karte.
9. **Meldungen.** Ruhiger, an einem Ort; Erfolg verschwindet, Fehler bleibt.
10. **Begleiter (Spielerei).** Eine kleine Figur unten links auf der Fläche:
    blinzelt, schaut dem Zeiger nach, nickt bei „fertig“, erschrickt bei einem
    Fehler. Ohne Text, abschaltbar in den Einstellungen, ohne Wirkung auf die
    Bedienung.

## Reihenfolge

a) Karten und Kanten · b) Docks und Reiter · c) Schiene und Kopfzeile ·
d) Fläche und Leerzustand · e) Bewegung und Fokus · f) Begleiter.

## Grenzen

Ohne Browser gibt es keine Messung. Jede Änderung wird aus dem Quelltext und
mit einer Prüfung belegt; ob sie schön ist, entscheidet sein Blick in Schritt
86 und danach. Layout-Änderungen, die nur Pixel verschieben, bleiben aus.
