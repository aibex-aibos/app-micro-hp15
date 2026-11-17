---
name: micro-hp15-gui
version: 0.1.0
description: >
  Zeigt die Oberfläche des RPN-Rechners micro-hp15 (Tastenfeld mit Anzeige X und Y).
  Verwenden bei "/micro-hp15-gui", "Rechner öffnen", "Tastenfeld".
---

# Oberfläche micro-hp15

Du zeigst die Oberfläche des RPN-Rechners micro-hp15: ein Tastenfeld mit Anzeige X und Y. Gerechnet wird in der Oberfläche selbst, ohne Claude.

Rufe `aibos` über den Weiterleiter auf: mit dem absoluten Pfad aus dem Block `<aibos-kernel-skills>` deines Master-Kontexts, als einzelnen Befehl ohne `cd`, `;`, `&&`, `|` oder Umleitung.

## Ablauf

1. Hast Du in dieser Sitzung ein Werkzeug, das HTML-Oberflächen im Chat zeigt (Desktop-App), rufe `aibos --gui --skill micro-hp15-gui --html` auf und zeige das Fragment mit diesem Werkzeug. Die Datei `@Data/app-micro-hp15/register.yaml` setzt AIBOS beim Öffnen selbst ein; Du liest sie nicht.
2. Hast Du kein solches Werkzeug (CLI), rufe `aibos --gui --skill micro-hp15-gui` ohne `--html` auf, folge der Ausgabe und sage: Das Tastenfeld gibt es nur in der Desktop-App; im Chat rechnest Du mit `/micro-hp15 <Tasten>`, z. B. `/micro-hp15 3 ENTER 4 +`.
3. Meldet die Oberfläche einen Registerwert zum Speichern („Run the skill /micro-hp15. Save R1 = 15."), gehört das zum Skill `micro-hp15`; die Oberfläche schreibt nie selbst.
