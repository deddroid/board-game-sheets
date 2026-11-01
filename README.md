# Board Game Sheets

A small suite of **single-file, offline companion sheets** for board games — the
kind of thing that replaces the paper score pad or detective sheet that comes in
the box. Each sheet is one self-contained `index.html` (HTML + CSS + JS inline,
no build step, no dependencies): open it by double-clicking the file, or host it
and open it on your phone.

Every sheet has an in-app **English / Italian** language switch.

## Games

| Game | Folder | What it does |
|------|--------|--------------|
| Detective Notes | [`games/cluedo-detective-notes/`](games/cluedo-detective-notes/) | Deduction sheet for the classic whodunit: suspects / weapons / rooms grid, markers, automatic deduction, notepad. |

The home page [`index.html`](index.html) is a simple launcher that lists every
game. It is a convenience, not a requirement: each game's `index.html` works on
its own.

## Three ways to use a sheet

1. **As a file** — download a game's `index.html` and open it with a double-click.
   Works fully offline; share the file with anyone.
2. **As a link** — if the repo is served (e.g. GitHub Pages), open
   `…/games/<game>/` in a browser. "Add to Home Screen" on mobile for an app feel.
3. **The whole suite** — clone or download the repo and open the home page.

## Structure

```
board-game-sheets/
├── index.html                       # home / launcher (lists all games)
├── games/
│   └── cluedo-detective-notes/
│       └── index.html               # a complete, standalone sheet
└── ...
```

Adding a game = create a folder under `games/` and add one entry to the `GAMES`
array in the home `index.html`.

## License

[MIT](LICENSE).

---

# Fogli per giochi da tavolo

Una piccola suite di **fogli di supporto offline, a file singolo**, per i giochi
da tavolo — sostituiscono il segnapunti o il foglio investigativo di carta che
trovi nella scatola. Ogni foglio è un unico `index.html` autonomo (HTML + CSS +
JS inline, niente build, niente dipendenze): aprilo con un doppio clic, oppure
mettilo online e aprilo dal telefono.

Ogni foglio ha un interruttore lingua **inglese / italiano** interno.

## Giochi

| Gioco | Cartella | Cosa fa |
|-------|----------|---------|
| Note dell'investigatore | [`games/cluedo-detective-notes/`](games/cluedo-detective-notes/) | Foglio di deduzione per il giallo classico: griglia sospetti / armi / stanze, marcatori, deduzione automatica, blocco appunti. |

La home [`index.html`](index.html) è un menù che elenca tutti i giochi: è una
comodità, non un obbligo. Ogni `index.html` di gioco funziona da solo.

## Tre modi di usare un foglio

1. **Come file** — scarica l'`index.html` di un gioco e aprilo col doppio clic.
   Funziona del tutto offline; passa il file a chiunque.
2. **Come link** — se il repo è pubblicato (es. GitHub Pages), apri
   `…/games/<gioco>/` nel browser. Su mobile "Aggiungi a schermata Home".
3. **Tutta la suite** — clona o scarica il repo e apri la home.

## Licenza

[MIT](LICENSE).
