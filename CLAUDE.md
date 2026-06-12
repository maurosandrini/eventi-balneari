# eventi-balneari

App a file singolo (index.html + events.json) pubblicata su GitHub Pages:
https://maurosandrini.github.io/eventi-balneari/. Il lavoro per versione vive
sul banco (`~/banco/lavoro/app/eventi-balneari-*`); qui solo codice e dati.

## Regola UI (standing rule /ui-pro, non rimuovere)
Per ogni nuova schermata o componente: riferisci PRIMA il design system
(`design/tokens.json` + `design/SYSTEM.md`). Prima di scrivere un colore,
spacing o type, verifica se il sistema lo definisce già; usa SOLO valori del
sistema, non introdurre nuovi colori, radius o font. Se il sistema non copre un
valore core: fermati e chiedi. Gap minore: valore più vicino del sistema + FLAG
in coda alla risposta. Prima di consegnare qualsiasi modifica UI:

```bash
python3 ~/banco/.claude/skills/ui-pro/scripts/ui_lint.py \
  --tokens design/tokens.json --src index.html
```

FAIL = non consegnare (riparare il codice o estendere il sistema in design/,
mai inventare inline). Accenti tipografici corretti in ogni stringa italiana.
