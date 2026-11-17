---
name: micro-hp15
version: 0.1.0
description: >
  Rechnet Tastenfolgen in umgekehrter polnischer Notation (RPN) nach dem HP-15C mit
  Stack X, Y, Z, T und den Registern R0-R9 (STO, RCL) und speichert die Register in
  @Data/app-micro-hp15/. Verwenden bei "/micro-hp15", "RPN rechnen", "3 ENTER 4 +",
  "STO", "RCL".
---

# RPN-Rechner micro-hp15

Du rechnest Tastenfolgen in umgekehrter polnischer Notation (RPN) nach dem HP-15C. Rechne genau nach den Regeln unten, nie nach Gefühl. Die Regeln des Master-Kontexts (Anrede, Präfix) gelten für Deine Antwort; dieser Skill legt keinen eigenen fest.

## Aufruf

- `/micro-hp15 <Tasten>`: Tasten durch Leerzeichen, z. B. `3 ENTER 4 +`.
- Ohne Tasten: zeige den Stack und die Tastenliste.
- Aus der Oberfläche: „Run the skill /micro-hp15. Save R1 = 15." (Abschnitt *Speichern*).

## Zustand

- **Stack** X, Y, Z, T und **LASTx**, am Anfang eines Chats alle 0. Der Stack lebt im Chat: Ein weiterer Aufruf macht mit dem zuletzt gezeigten Stack weiter. Ein neuer Chat beginnt leer. Der Stack wird nie gespeichert.
- **Register** R0–R9 aus `@Data/app-micro-hp15/register.yaml`. Lies die Datei einmal pro Aufruf; fehlt sie oder ein Eintrag, ist der Wert 0.
- **Lift** (Stack-Lift): ein Merker „an/aus", am Anfang an.

## Tasten

| Taste | Wirkung |
|---|---|
| Zahl (`12`, `3.5`, `.5`) | Lift an: T←Z, Z←Y, Y←X, X←Zahl; Lift aus: X←Zahl. Danach Lift an |
| `ENTER` | T←Z, Z←Y, Y←X (X bleibt); danach Lift aus |
| `+` `−` `×` `÷` | LASTx←X; X←Y op X; Y←Z, Z←T (T bleibt); Lift an |
| `CHS` | X←−X (LASTx und Lift unverändert) |
| `1/x` | LASTx←X; X←1/X; Lift an |
| `x↔y` | X und Y tauschen; Lift an |
| `R↓` | X←Y, Y←Z, Z←T, T←alter X; Lift an |
| `CLx` | X←0; Lift aus |
| `LASTx` | wie eine Zahl: LASTx wird eingegeben |
| `STO n` | Rn←X (n = 0–9); Stack und Lift unverändert |
| `RCL n` | wie eine Zahl: Rn wird eingegeben |

Die Schreibweisen `-`, `*`, `/` für `−`, `×`, `÷` und `x<>y` für `x↔y` nimmst Du an.

## Rechnen

- Rechne dezimal mit **10 signifikanten Stellen** (Rundung auf die nächste Stelle, bei .5 weg von 0). Zwischenwerte im Stack behalten alle 10 Stellen.
- **Fehler:** Division durch 0 und `1/x` von 0 melden `Error 0`; die Operation entfällt, Stack, LASTx und Lift bleiben wie vor dieser Taste, die übrigen Tasten des Aufrufs werden nicht mehr ausgeführt. Eine unbekannte Taste meldet `Error: unbekannte Taste <taste>` mit demselben Verhalten.
- Gehe die Tasten **eine nach der anderen** durch und führe den Stack mit.

## Anzeige

Zeige nach dem Aufruf den Stack in einem Codeblock, vier Zeilen, mit 4 Nachkommastellen (FIX 4) und Dezimalpunkt:

```text
T: 0.0000
Z: 0.0000
Y: 3.0000
X: 7.0000
```

## Speichern

Nur Registerwerte werden gespeichert, nie Stack, LASTx oder Lift.

1. Hat der Aufruf ein `STO` ausgeführt, oder lautet er „Save Rn = x", zeige, **was gespeichert wird** (Datei, geänderte Register mit altem und neuem Wert).
2. Frage mit der Auswahl „Speichern (Empfohlen)" / „Nicht speichern". Schreibe nie ohne Ja.
3. Nach dem Ja lege `@Data/app-micro-hp15/` an, falls nötig, und schreibe `register.yaml` mit allen zehn Registern:

```yaml
R0: 0
R1: 15
R2: 0
R3: 12
R4: 0
R5: 0
R6: 0
R7: 0
R8: 0
R9: 0
```

Nur dieser Skill schreibt diesen Ordner. Bei „Save Rn = x" setze nur Rn.

## Beispiele

| Aufruf | Ergebnis |
|---|---|
| `3 ENTER 4 +` | X = 7.0000 |
| `5 ENTER 2 ÷` | X = 2.5000 |
| `1 ENTER 2 ENTER 3 ENTER 4 R↓` | X = 3, Y = 2, Z = 1, T = 4 |
| `1 ENTER 0 ÷` | `Error 0`; Y = 1, X = 0 |
| `12 STO 3`, dann `CLx`, dann `RCL 3` | X = 12; R3 = 12 wird nach Rückfrage gespeichert |
| `RCL 7` (nie gesetzt) | X = 0 |
