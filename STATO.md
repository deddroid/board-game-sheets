# Board Game HTML Suite — stato del progetto

Aggiornato al 06/11/2026. Stato: in corso.
**Progetto madre**: raccoglie piccole app HTML per il foglio di gioco di vari giochi da tavolo.
Ogni gioco è una sottocartella con il suo progetto e la sua sessione.

## Cosa fa

Suite di app HTML autonome (un singolo file, offline, apribili dal telefono, facili da passare a
terzi) che sostituiscono i fogli / segnapunti cartacei dei giochi da tavolo. Primo gioco: Cluedo
(foglio investigazioni + blocco appunti).

## Stato: in corso

## Come è organizzata

- Una **sessione "supervisore" della suite** fa da hub: tiene la visione d'insieme e avvia una
  sessione dedicata (fork) per ogni gioco. L'utente parla con questa, non con i singoli giochi.
- **Monorepo GitHub** `board-game-sheets` (un solo repo con tutti i giochi).
- Struttura: `index.html` (home/launcher che elenca i giochi) + `games/<gioco>/index.html`
  (ogni gioco è un file autonomo e condivisibile da solo).
- Aggiungere un gioco = creare `games/<gioco>/` + una voce nell'array `GAMES` della home.
- Il Supervisore globale di `~/Code` vede questa suite come **un unico progetto madre**.

## Struttura

```
board-game-sheets/
├── index.html                       # home / launcher (EN/IT, elenco inline)
├── README.md                        # bilingue
├── LICENSE                          # MIT
├── games/
│   ├── cluedo-detective-notes/      # Detective Notes (storia git importata via subtree)
│   │   └── index.html
│   └── ancient-wonders-scorepad/    # Ancient Wonders Scorepad
│       └── index.html
└── STATO.md
```

## Giochi

- **Detective Notes** (`./games/cluedo-detective-notes/`, compatibile Cluedo): MVP funzionante —
  singolo `index.html`, griglia sospetti/armi/stanze × giocatori con marcatori, deduzione automatica,
  gestione giocatori, edizioni, blocco appunti, tema, privacy, salvataggio locale, UI EN/IT. v1.0.0.
  Sessione dedicata: `cluedo-detective` (sotto-sessione dell'hub, avviata il 06/11/2026 con brief
  "test di attendibilità + ottimizzazione/semplificazione con latenze prima/dopo"). La precedente
  `cluedo-notes` è archiviata (il suo lavoro è già nel monorepo).
- **Ancient Wonders Scorepad** (`./games/ancient-wonders-scorepad/`, compatibile 7 Wonders): MVP
  completo v1.0.0 (04/11/2026) — calcolatore 3–7 giocatori, 7 categorie, scienza calcolata dai simboli
  (jolly massimizzati), monete→punti, spareggio, EN/IT, tema, salvataggio locale. Presente nella home.
  Sessione dedicata: `wonders-scorepad` (sotto-sessione dell'hub, idle).

## Pubblicazione

- **Repo GitHub** (pubblico): https://github.com/deddroid/board-game-sheets
- **GitHub Pages** attivo (branch `main`, root):
  - Home: https://deddroid.github.io/board-game-sheets/
  - Detective Notes: https://deddroid.github.io/board-game-sheets/games/cluedo-detective-notes/

## Sessioni

- **Hub (questa)**: chat principale del progetto; possiede la home `index.html`, `README.md`, `STATO.md`
  della suite. Avvia una sotto-sessione per ogni gioco e aggiorna la voce nel launcher.
- **Sotto-sessioni** (annidate sotto l'hub): `cluedo-detective` (Detective Notes), `wonders-scorepad`
  (Ancient Wonders Scorepad). Ogni sessione tocca solo la propria cartella `games/<slug>/`.
- Nota: l'app non permette di ri-annidare una sessione esistente sotto un altro genitore; per questo
  la vecchia `cluedo-notes` (avviata da un'altra sessione) è rimasta archiviata e Cluedo ha una nuova
  sotto-sessione.

## Convenzione versioning

- Ogni pagina gioco: `var APP_VERSION = "x.y.z"; var APP_DATE = "AAAA-MM-GG";` in cima allo script,
  mostrati a piè di pagina come `vx.y.z · AAAA-MM-GG`. Si aggiornano a ogni modifica pubblicata
  (PATCH fix/refactor interni, MINOR funzioni nuove, MAJOR cambi grossi).
- Home: `SUITE_VERSION`/`SUITE_UPDATED` per il launcher; ogni voce di `GAMES` ha `version`/`updated`
  che l'hub allinea quando una sessione-gioco pubblica una nuova versione.

## Home / launcher — test e ottimizzazione (06/11/2026)

Test funzionali (tutti superati, nel browser, da `file://` + viewport 375px): ricerca per titolo,
descrizione, alias e compatibilità; insensibile a maiuscole/accenti e agli spazi; stato vuoto; pulsante
×; EN/IT (testi, placeholder, `aria-pressed`, cambio lingua che conserva il filtro); badge versione e
compatibilità; link verso file esistenti; nessun overflow orizzontale; nessun errore in console; nessuna
stringa "undefined". `localStorage` indisponibile nello snapshot `data:` gestito senza errori.

Refactor (v1.0.1, comportamento e aspetto invariati): card costruite una sola volta e filtro che
alterna solo `hidden` (prima: `innerHTML` ricostruito a ogni tasto); stringa di ricerca normalizzata
precalcolata per gioco; i18n centralizzata in `applyI18n()` (`data-i18n` + `data-i18n-ph`); storage
centralizzato in `ls`; CSS `.has-value`/`display:none` sostituiti da `[hidden]`.

Latenze (µs/operazione, mediana di 7 rep × 1000 op, stesso ambiente prima/dopo):

| Operazione                       | Prima | Dopo | Δ      |
|----------------------------------|------:|-----:|-------:|
| `render()` completo              | 110   | 5,3  | −95 %  |
| evento digitazione (con match)   | 258   | 221  | −14 %  |
| digitazione senza risultati      | 162   | 146  | −10 %  |
| cambio lingua                    | 75    | 83   | ≈ (rumore) |

Il costo residuo della digitazione è il dispatch dell'evento + `normalize("NFD")` della query, non il
rendering: con decine di giochi il vantaggio del filtro a `hidden` cresce, quello della lingua no.

## Da fare

- Aggiungere altri giochi su richiesta (una sessione per gioco), ognuno sotto `games/`.
- Allineare `version`/`updated` di Detective Notes nella home quando `cluedo-detective` pubblica.
- Stesso passaggio "test + ottimizzazione" per Ancient Wonders Scorepad, se richiesto.

## Vincoli

- **Clean-room**; niente marchi nei nomi pubblici.
- GUIDA-PROGETTI (`~/Code/0_Claude/GUIDA-PROGETTI.md`): codice/commit/README/UI in inglese con
  italiano selezionabile; **chiedere prima** di pubblicare (GitHub) o scegliere la licenza.
