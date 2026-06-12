# Design system · eventi-balneari (v2, riprogettato 2026-06-12 con /ui-pro)

Source of truth per ogni schermata/componente futuro. I valori reali stanno in
`tokens.json` (letto dal gate `ui_lint.py`); qui componenti, stati e regole.
**Riferimento visivo confermato da Mauro: Luma (luma.com)**, in particolare le
pagine città (es. luma.com/milan): timeline per giorno, card morbide, neutri
derivati da un solo ink in alpha. v1 (sabbia/mare con header a gradiente) è
nella storia git (commit 59438c1).

## Carattere
Agenda quieta in stile Luma: superficie fredda quasi bianca (`bg #f7f8f9`),
UN solo colore ink (`#131517`) declinato in alpha (64% testi secondari, 40%
terziari, 8% hairline, 4% fondi controlli), card bianche radius 16 con ombra
appena percettibile. Niente maiuscole, niente gradienti. L'identità propria
sopravvive nei punti giusti: stelle dorate, emoji (🧘 🎪 ↗ ⚠️ ★), palette tipi
come pillole tinte, `sea #0e7c86` SOLO per i link.

## Componenti core (con varianti e stati)

- **Card evento** (`.card`): bianca, bordo hairline, radius 16px, padding 16px,
  ombra `shadow`. Ordine interno (come Luma): ora (`.when`, 13px terziario,
  tabular-nums) sopra; riga titolo con marker (stelle/🧘) + titolo 16px/600;
  venue 14px/500 secondario con località terziaria; descrizione 14px
  secondario; riga `.tags` con le pillole; fonte 13px link `sea`.
- **Pillole** (`.badge .rec .fest .ver`): 12px/500, radius 999px, padding
  2px 10px, schema bg tinta + testo tono scuro (mai testo bianco su colore
  pieno): cult viola, music petrolio, food terracotta, well verde, altro
  grigio, warn ambra, dead rosa spento. Un colore per tipo, mai altri usi.
- **Gruppi giorno** (`.group`): timeline alla Luma; puntino bianco con bordo
  `dot_border` + binario verticale tratteggiato hairline a sinistra;
  intestazione giorno 14px/600 ink, minuscola (niente uppercase).
- **Controlli sticky** (`.controls`): fondo pagina + hairline sotto; seg e chip
  tutti uguali: pillola `soft` (ink 4%), testo 13px/500 secondario, attivo =
  fondo `ink` testo bianco. Nessun bordo. (Il select zona è stato sostituito
  dal picker `.zonepick`.)
- **Picker zona** (`.zonepick`/`.zones`): UNA riga scorrevole sotto l'header,
  chip `zchip` con conteggio eventi in `.zn` (opacità ridotta), zone ordinate
  per numero di eventi; prime 8 + chip "+N altre" che espande a griglia
  (`.zones.open`); su desktop sempre a griglia. L'ultima zona scelta si salva
  in localStorage (`eb_zona`) e al ritorno è preselezionata e centrata.
- **Header**: leggero, sul fondo pagina: titolo 28px/600 (24px sotto i 560px),
  sottotitolo 14px secondario, legenda 13px terziario.

## Stati
- **Empty/loading/error** (`.empty`): card con bordo tratteggiato `dot_border`,
  testo terziario centrato; il messaggio riflette i filtri attivi.
- **Staleness** (`.stale`): blocco ambra (`warn_bg`/`warn`), radius 12px, solo
  se dati più vecchi di 7 giorni.

## Responsive (rilevamento automatico via viewport, niente sniffing UA)
- **Mobile (<=560px)**: header compatto (24px sopra), h1 24px, filtri in UNA
  riga scorrevole orizzontale senza scrollbar, binario timeline a 16px, card
  padding 12px.
- **Tablet (561-979px)**: layout base, colonna singola max 680px.
- **Desktop (>=980px)**: wrap a 980px, griglia `1fr + 280px` con gap 48px; la
  nota di copertura va nella colonna laterale destra, sticky a 64px (come la
  sidebar delle pagine città di Luma).

## Regole di composizione
- Niente modal né drawer: lista a una colonna (max 680px), controlli sticky.
  Nuovo filtro = nuovo chip o seg, non popup.
- Le emoji sono parte del linguaggio: non sostituirle con icone.
- Nuovo tipo di evento = coppia `X` + `X_bg` nella palette `tipo` (testo tono
  scuro leggibile sulla tinta) + chip + riga in legenda.
- `sea` resta il colore dei link e di nient'altro; l'attivo dei controlli è
  `ink` (come Luma).

## Storia
- v1 (estrazione fedele del look sabbia/mare): risolti in v2 i suoi candidati
  al consolidamento (padding 13/15/7px fuori scala, terzo grigio `#34474a`,
  `#6b7a7c`: eliminati).

## Gate (obbligatorio prima di consegnare qualsiasi modifica UI)
```bash
python3 ~/banco/.claude/skills/ui-pro/scripts/ui_lint.py \
  --tokens design/tokens.json --src index.html --strict
```
La v2 passa `--strict`: mantenerlo. Limiti noti del lint: non vede rgba() e i
colori URL-encoded nei data URI (freccia del select, `%23131517`); quelli li
copre questa pagina + review.
