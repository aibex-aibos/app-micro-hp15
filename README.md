# app-micro-hp15

RPN-Taschenrechner nach dem HP-15C für AIbex AIBOS: Postfix-Eingabe, Stack X, Y, Z, T, die Grundrechenarten und die Register R0–R9 mit STO und RCL. Bedienbar im Chat und über eine Oberfläche mit Tastenfeld.

## Voraussetzung

AIBOS 0.7 (`requires.aibos: ">=0.7,<0.8"`).

## Holen

```bash
aibos --fetch app-micro-hp15
```

Neue Skills sind ab dem nächsten Chat im Projekt verfügbar.

## Aufruf

- `/micro-hp15 3 ENTER 4 +` rechnet die Tastenfolge im Chat und zeigt den Stack.
- `/micro-hp15-gui` zeigt die Oberfläche mit dem Tastenfeld (Desktop-App; in der CLI erscheint ein Hinweis).

Die Register R0–R9 bleiben in `@Data/app-micro-hp15/register.yaml` erhalten, der Stack nicht.

## Lizenz

MIT, siehe `LICENSE`.
