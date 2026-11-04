# Board Game HTML Suite — stato del progetto

Aggiornato al 01/11/2026. Stato: in corso.
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
│   └── cluedo-detective-notes/      # storia git importata via subtree
│       └── index.html
└── STATO.md
```

## Giochi

- **Detective Notes** (`./games/cluedo-detective-notes/`): MVP funzionante — singolo
  `index.html`, griglia sospetti/armi/stanze × giocatori con marcatori, deduzione automatica,
  gestione giocatori, blocco appunti, salvataggio locale, UI EN/IT. Sessione dedicata: `cluedo-notes`.
  Nome pubblico neutro "Detective Notes" (cartella storica: `cluedo-detective-notes`). Versione: v1.0.0.
- **Ancient Wonders Scorepad** (`./games/ancient-wonders-scorepad/`): **in avvio** — segnapunti /
  calcolatore di punteggio compatibile con 7 Wonders (nome pubblico neutro). Sessione dedicata appena
  avviata. Voce nella home launcher da aggiungere a MVP pronto.

## Pubblicazione

- **Repo GitHub** (pubblico): https://github.com/deddroid/board-game-sheets
- **GitHub Pages** attivo (branch `main`, root):
  - Home: https://deddroid.github.io/board-game-sheets/
  - Detective Notes: https://deddroid.github.io/board-game-sheets/games/cluedo-detective-notes/

## Da fare

- Aggiungere altri giochi su richiesta (una sessione per gioco), ognuno sotto `games/`.

## Vincoli

- **Clean-room**; niente marchi nei nomi pubblici.
- GUIDA-PROGETTI (`~/Code/0_Claude/GUIDA-PROGETTI.md`): codice/commit/README/UI in inglese con
  italiano selezionabile; **chiedere prima** di pubblicare (GitHub) o scegliere la licenza.
