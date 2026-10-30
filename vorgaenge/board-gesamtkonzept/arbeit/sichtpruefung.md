---
etage: Umsetzung
status: offen
offene_fragen: 0
stand: 2026-10-30
---

# Sichtprüfung — was zu sehen sein soll

Vorher: die Seite **neu laden**, kein Neustart. Der Name in der
Plugin-Verwaltung des Dashboards wird erst beim nächsten DSH-Start
„CollaborationBoard“; die Board-Oberfläche selbst ist sofort neu.

## Die Zonen

1. **Schiene links** (72 px): „Bibliothek“ und „Verlauf“. Der offene Ort trägt
   einen Akzentbalken links und eine getönte Fläche; beim Überfahren wird die
   Schrift heller. Unten sitzt der Begleiter (18 px), die Augen blinzeln.
2. **Dock daneben** (360 px): die Liste, scrollt innen, der Kopf steht.
   Schließen mit ✕ oder Escape; der Fokus kehrt zum Schienenknopf zurück.
3. **Kopfzeile**: links die Board-Wahl, in der Mitte Titel und Zustand, rechts
   Menschen, Suche und „Alles zeigen“; darunter eine leise Statuszeile.
4. **Fläche**: Karten mit Rand und Schatten; beim Überfahren werden sie einen
   Hauch heller; die offene Karte trägt einen Akzentring. Ziehen verschiebt,
   die Punkte an den Kanten verbinden.
5. **Rechtes Dock** (380 px): öffnet sich beim Klick auf eine Karte. Oben Name
   und Fassung, „Bearbeiten“ nur bei eigenem Schritt, ✕ schließt. Die Karte
   bleibt klein und zeigt nur ihren Grundinhalt.
6. **Gespräch**: öffnet sich als zweiter Reiter im rechten Dock, wenn ein
   Vorhaben-Gespräch läuft. Schließen der Station lässt das Gespräch stehen
   und umgekehrt; die ✕-Taste schließt den aktiven Reiter.
7. **Ein Seitenpanel zur Zeit**: Station öffnen schließt Bibliothek/Verlauf;
   Bibliothek öffnen schließt das rechte Dock. Ein gewähltes Vorhaben bleibt
   dabei gewählt.

## Was ich nicht sehen kann

- Ob 1,04 Helligkeit beim Überfahren zu viel oder zu wenig ist.
- Ob der Akzentbalken an der Schiene sitzt oder zu weit innen.
- Ob der Begleiter bei kurzen Fenstern stört.
- Ob die Abstände in der Kopfzeile ruhig wirken.
- Ob das rechte Dock bei 992 px breit genug ist.

## Bekannte Kanten

- Bei 360 px nimmt ein offenes Dock die ganze Breite; der Ziehgriff ist dort
  wirkungslos. Die Fläche ist dann nicht sichtbar, bis das Dock schließt.
- Server-Fehlertexte außerhalb der neun Rechte-Sätze bleiben deutsch.
- Die echten GitHub-Schreibwege sind nicht live geprüft.

## Was ich von dir brauche

Ein Satz je Zone, die auffällt — „zu eng“, „zu laut“, „Begleiter weg“, „Dock
zu breit“ reicht. Ein Screenshot hilft mehr als eine Beschreibung.
