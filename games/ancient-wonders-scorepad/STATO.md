# Ancient Wonders Scorepad — stato

Aggiornato al 04/11/2026. Stato: **MVP completo** (v1.0.0) — committato su `main`.

## Cosa è

Segnapunti / calcolatore di punteggio per un gioco di civiltà antiche a carte
(compatibile con **7 Wonders**). Sostituisce il foglio segnapunti cartaceo:
inserisci i punti di ogni giocatore per categoria e calcola i totali in automatico.

Nome pubblico **neutro**: "Ancient Wonders Scorepad". Il marchio "7 Wonders" è usato
solo come etichetta di compatibilità e termine di ricerca (uso nominativo), mai come
titolo, logo o grafica.

## Vincoli (come tutta la suite)

- **Clean-room**: replica solo la *funzionalità* di calcolo; niente layout del foglio
  ufficiale, niente nomi/testi delle carte, niente artwork, niente loghi/marchi.
- Singolo `index.html` autonomo (HTML + CSS + JS inline), offline, mobile-first.
- Salvataggio in `localStorage`. UI in inglese con italiano selezionabile (EN/IT).
- Versione visibile in pagina (`APP_VERSION` / `APP_DATE`), convenzione della suite.
- Commit nel monorepo `board-game-sheets` su `main`. La voce nella home (launcher) la
  aggiunge l'hub, non questa sessione.

## Categorie di punteggio (gioco base) da coprire

- Militare (conflitti, rosso): +/- token per età.
- Tesoro / monete (3 monete = 1 punto).
- Meraviglia (stadi costruiti).
- Civili (blu): punti fissi.
- Commerciali (gialli): punti (alcune carte a fine partita).
- Scienza (verdi): per ogni simbolo n → n²; + 7 per ogni set di 3 simboli diversi.
- Gilde (viola): punti variabili.
- Spareggio: chi ha più monete.
- Giocatori: 3–7.
- (Espansioni — Cities/Leaders/Armada/Edifice — eventualmente dopo il base.)

## Fatto (MVP v1.0.0, 04/11/2026)

- `index.html` singolo file, HTML+CSS+JS inline, offline, apribile con doppio clic.
- 3–7 giocatori (±), nomi modificabili, persistenza in `localStorage` (con try/catch:
  funziona anche senza storage).
- 7 categorie per giocatore con totale live e classifica:
  - Militare: totale diretto, anche negativo.
  - Tesoro: inserisci monete → punti = `floor(monete / 3)` (mostrato accanto).
  - Meraviglia / Civili / Commerciali / Gilde: punti diretti.
  - Scienza: inserisci compasso/ingranaggio/tavoletta + jolly; punteggio `n²` per tipo
    + 7 per set completo; i jolly sono assegnati per **massimizzare** (brute force).
- Spareggio: a parità di totale vince chi ha più monete; il leader è evidenziato (★).
- Nuova partita con conferma (mantiene il numero di giocatori, azzera i punteggi).
- EN/IT con default dalla lingua del browser, scelta ricordata; toggle tema chiaro/scuro
  (oltre a `prefers-color-scheme`). Versione visibile a piè di pagina `v1.0.0 · 2026-11-04`.
- Nota clean-room / disclaimer a piè di pagina (EN+IT).

## Idee future (non MVP)

- Espansioni: Cities (monete nere/debiti/diplomazia), Leaders, Armada (navale/isole),
  Edifice. Richiederebbero categorie/regole aggiuntive, da valutare separatamente.
- Esporta/condividi risultato; cronologia partite.

## Note di coordinamento

- La voce nella home `index.html` alla radice la aggiunge l'hub (questa sessione non
  tocca la home, per evitare conflitti sullo stesso file).
