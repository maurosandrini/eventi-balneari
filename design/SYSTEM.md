# Design system · eventi-balneari (v1, estratto 2026-06-12 con /ui-pro)

Source of truth per ogni schermata/componente futuro. I valori reali stanno in
`tokens.json` (letto dal gate `ui_lint.py`); qui componenti, stati e regole.

## Carattere
Utility balneare-romagnola: superficie sabbia (`sand`), accent mare (`sea`),
card bianche con bordo `line` e ombra morbida. Densità alta (è un'agenda), font
di sistema, niente webfont. Tono: informativo, zero decorazione gratuita.

## Componenti core (con varianti e stati)

- **Card evento** (`.card`): bianca, bordo `line`, radius 14px, ombra `shadow`.
  Riga top: marker (stelle `star` o 🧘) + titolo (650) + ora (`muted`,
  tabular-nums) + badge + pillole. Poi venue (`sea_dark`, 600) con località
  (`muted`), descrizione (`desc_text`), fonte (`sea`, link).
- **Badge tipo** (`.badge`): uppercase 0.68rem, radius 6px, testo bianco, fondo
  dalla palette `tipo` (cult/music/food/well/altro). Un colore per tipo, mai
  altri usi di quei colori.
- **Pillole stato**: `rec` (ricorrente: verde tenue), `fest` (festival: viola
  tenue), `ver` (da confermare: ambra `pill_warn`), `ver.dead` (fonte
  irraggiungibile: `pill_dead`). Schema: bg tenue + bordo tono + testo scuro
  dello stesso tono. Radius 6px, 0.7rem.
- **Controlli sticky** (`.controls`): segmented (`.seg`, radius 999px, attivo =
  fondo `sea` testo bianco), select pillola, chip filtro (attivo = fondo `ink`).
  Fondo `sand`, bordo basso `line`.
- **Intestazioni giorno** (`.day h2`): uppercase 0.82rem, `sea_dark`,
  border-bottom 2px `line`.
- **Header**: gradiente 135° `sea` → `sea_dark`, testo bianco.

## Stati
- **Empty** (`.empty`): card bianca con bordo TRATTEGGIATO `line`, testo
  `muted`, centrato, messaggio che riflette i filtri attivi.
- **Loading**: stesso `.empty` con "Carico gli eventi…".
- **Staleness** (`.stale`): banner ambra (`pill_warn`), radius 12px, solo se
  dati più vecchi di 7 giorni.
- **Error** (fetch fallito): `.empty` con istruzione concreta.

## Regole di composizione
- Niente modal né drawer: l'app è una lista a una colonna (max 860px), i
  controlli sono sticky in alto. Nuove feature di filtro = nuovo chip o seg,
  non popup.
- Le emoji sono parte del linguaggio (🧘 🎪 ↗ ⚠️ e ★): non sostituirle con icone.
- Nuovo tipo di evento = nuovo colore nella palette `tipo` (scelto scuro
  abbastanza da reggere testo bianco) + chip + riga in legenda.

## Candidati a consolidamento (rilevati in Fase A, decidere alla prossima UI)
- `.card` padding `13px 15px` e pillole `2px 7px`: fuori scala (13/15/7 non
  sono nella scala spacing); alla prossima passata UI portare a 12/16 e 2/6 o
  promuovere 13/15/7 a scala ufficiale.
- `desc_text #34474a` vive a metà tra `ink` e `muted`: valutare se serve davvero
  un terzo grigio.
- `tipo.altro #6b7a7c` è quasi `muted #5d6f71`: ok finché regge il badge bianco.

## Riferimenti visivi (da confermare con Mauro, max 2-3)
Da fissare al prossimo giro Premium: candidati naturali sono le agende eventi
pulite tipo Luma (lu.ma) e le liste dense tipo Hacker News con card. <!-- segnaposto: scelta dell'utente -->

## Gate (obbligatorio prima di consegnare qualsiasi modifica UI)
```bash
python3 ~/banco/.claude/skills/ui-pro/scripts/ui_lint.py \
  --tokens design/tokens.json --src index.html
```
Limiti noti del lint: non vede rgba() e i colori URL-encoded nei data URI
(la freccia del select); quelli li copre questa pagina + review.
