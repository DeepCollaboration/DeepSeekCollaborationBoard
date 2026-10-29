# Übersetzungsbefunde

Gesammelt für Schritt 66. Jeder Eintrag nennt Ort, Befund und was richtig ist.

## Arabisch

- **`Dieses Board liegt auf GitHub. Ohne Token bleibt es beim Lesen; dein Token bleibt auf diesem Rechner.`**
  (Schritt 60, client.js) — im Arabischen steht `هذا الـ Board على GitHub` mit
  Artikel-Zugstrich (Tatwil) vor dem lateinischen Wort. Die übrigen 55 Stellen
  im Wörterbuch schreiben `Board` ohne Artikel oder `لوح`. Richtig:
  `هذا Board على GitHub. بدون رمز تبقى القراءة فقط؛ يبقى رمزك على هذا الجهاز.`
  **erledigt (Schritt 66):** `texte/ar.json` trägt den Satz jetzt genau so, ohne
  Artikel-Zugstrich vor `Board`.

## Sichtbare Texte

- Die 17 sichtbaren Texte außerhalb von `uebersetze` (Liste mit Zeilenverweisen
  in `ui-analyse.md`, Abschnitt „Siebzehn Texte stehen außerhalb der
  Übersetzung“). Die Zeilen haben sich seit Schritt 58 verschoben; Schritt 66
  sucht sie neu.
  **erledigt (Schritt 66):** alle 17 Texte der Liste stehen jetzt in
  `uebersetze(…)`; die neuen Schlüssel liegen in allen sieben Wörterbüchern.
  `test/nach66.mjs` findet keinen sichtbaren Text mehr außerhalb der
  Übersetzung — die verbliebenen Fundstellen stehen mit Grund in der
  Ausnahmeliste (Modellaufträge, Auswahltexte, Knopf- und Feldbeschriftungen,
  Konsole). Die Farbtabellen sind Schlüssel der Wörterbücher und werden in
  `farbName()` bzw. an den Akzent-Beschriftungen übersetzt.
  **Nachbesserung (Schritt 66):** die Begründung der Modellaufträge sagt jetzt,
  dass sie im Chat als Nachricht sichtbar sind und deutsch bleiben, weil sie
  das Verhalten steuern; die sechs Akzentnamen stehen in `uebersetze`; drei
  schwache Übersetzungen (ja/ru/zh „Issues“, ru „Pull Requests“, ru
  „Korrektur“) sind nachgezogen.
