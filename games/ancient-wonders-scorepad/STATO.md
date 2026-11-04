# Ancient Wonders Scorepad — stato

Aggiornato al 04/11/2026. Stato: **da iniziare** (sessione dedicata in avvio).

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

## Da fare

- MVP: inserimento punteggi per categoria, totali automatici, spareggio, EN/IT, versione.
