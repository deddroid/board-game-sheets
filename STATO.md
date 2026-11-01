# Cluedo - Detective Notes — stato del progetto

Aggiornato al 31/10/2026. Stato: MVP funzionante.
I numeri qui riportati vanno misurati, non stimati.

## Cosa fa

App **open source** che sostituisce il foglio investigazioni cartaceo del Cluedo: la griglia
sospetti / armi / stanze × giocatori dove si segna cosa è escluso o confermato, così non si spreca
carta. Replica le funzionalità dell'app di riferimento **"Get a Clue"** (clean-room) e in più
aggiunge un **blocco appunti** libero — l'unica cosa che a Get a Clue manca.

## Fatto (MVP)

Un unico file `index.html` autonomo, nessuna dipendenza, funziona offline. Apribile dal telefono
aprendo il file nel browser.

- **Griglia** sospetti / armi / stanze × giocatori, ottimizzata per il telefono (tap target grandi,
  dark mode automatica). Scorre dentro il proprio riquadro: la **riga dei nomi (intestazioni)** resta
  fissa in alto scorrendo in verticale e la **prima colonna** resta fissa scorrendo in orizzontale.
- **Marcatori per cella**: tap che cicla vuoto → ✗ (non ce l'ha) → ✓ (ce l'ha) → ? (forse) → vuoto.
- **Deduzione automatica** (attivabile/disattivabile): quando segno ✓ a un giocatore, gli altri
  ricevono ✗ in automatico; una carta esclusa a *tutti* viene evidenziata come **SOLUZIONE**.
  Le ✗ messe in automatico sono tracciate (`state.auto`): togliendo la ✓ (✓→?) spariscono, mentre
  le ✗ messe a mano restano. Così è sempre possibile azzerare una riga.
- **Istruzioni con le regole di Cluedo**: la modale ❓ include ora un riassunto delle regole del
  gioco (obiettivo, preparazione, turno/ipotesi, confutazione, accusa), scritto con parole proprie
  (clean-room), oltre alla guida all'uso dell'app, EN/IT.
- **Gestione giocatori**: aggiungi/rimuovi/rinomina, scelta di quale sei "tu" (colonna evidenziata).
- **Le mie carte**: pannello con checklist per segnare la propria mano (✓ a me, ✗ agli altri).
- **Carte personalizzabili**: rinomina/aggiungi/togli carte per edizione diversa; reset ai default.
- **Edizioni (preset)**: icona 🃏 + chip con l'edizione attiva. Edizione predefinita "Classico" (EN/IT);
  si possono salvare edizioni personalizzate dalle carte correnti, rinominarle, eliminarle e passare
  dall'una all'altra (il cambio chiede conferma e azzera i segni, il blocco appunti resta).
- **Istruzioni**: icona ❓ con una guida all'uso dell'app + consigli rapidi di gioco (EN/IT).
- **UI**: header con logo e toolbar, colori per categoria (sospetti/armi/stanze), chip edizione.
- **Blocco appunti** libero, sotto la griglia.
- **Salvataggio locale** su `localStorage` (best-effort, con fallback in memoria dove lo storage è
  disabilitato — es. apertura come `data:` URL). Tutto resta solo sul dispositivo.
- **Lingua**: inglese / italiano selezionabile dall'header; default dall'`navigator.language`.
  Set di carte classico tradotto nelle due lingue.

### Verificato (nella preview, viewport telefono)

- Render griglia, cicli dei marcatori, cascata ✗ al segnare un ✓, rilevamento SOLUZIONE: OK.
- i18n IT/EN, colonna "io" evidenziata, layout mobile con header/colonna sticky: OK.
- Correzione fatta: la lettura di `localStorage` a livello globale bloccava lo script nei contesti
  dove lo storage è disabilitato → ora tutte le letture/scritture passano da wrapper protetti.

## Da fare / possibili migliorie

- **Ricognizione "Get a Clue"**: l'MVP è stato costruito dalla conoscenza del foglio cartaceo
  classico, NON ispezionando l'app. Da identificare con l'utente (nome esatto/store) e catalogare le
  sue funzioni per colmare eventuali lacune (es. annotazioni per cella, registro delle ipotesi,
  deduzioni più avanzate, più partite salvate).
- **Edizioni predefinite**: per ora c'è solo "Classico" (verificato). Non ho incluso edizioni
  ufficiali espanse (es. Master Detective/Super Cluedo) per non spedire liste potenzialmente
  inesatte e per prudenza sui marchi; l'utente può crearle da sé, oppure aggiungerle su richiesta
  indicando le carte esatte. Da rivedere i nomi delle edizioni prima di un'eventuale pubblicazione.
- Eventuale registro strutturato dei turni (chi ha chiesto cosa, chi ha mostrato) oltre al blocco
  appunti libero.
- Note per singola cella / segni liberi dentro la cella.
- Export/condivisione dello stato; più partite salvate con nome.
- PWA (manifest + service worker) per "installazione" e vero uso offline da URL.

## Scelte

- **Stack:** web app statica, singolo `index.html`, nessun framework. Da confermare con l'utente
  (scelta reversibile; MVP già pronto).
- **Nome pubblico:** usato "Detective Notes" (neutro). "Cluedo" / "Clue" / "Get a Clue" sono marchi.
  Da confermare.
- **Lingua UI:** inglese con italiano selezionabile (da GUIDA-PROGETTI). Fatto.

## Vincoli

- **Clean-room:** si replicano le *funzionalità*, non si copiano codice, grafica, asset o marchio di
  "Get a Clue".
- GUIDA-PROGETTI: codice/commit/README/UI in inglese con italiano selezionabile; **chiedere prima**
  di pubblicare (GitHub) o scegliere la licenza. Repo non ancora pubblicato.

## Strumenti

- REA (se l'app di riferimento è un APK): `~/Code/ZTool/re/` — `jadx`, `apktool`, `platform-tools/adb`.
  Non ancora usato: serve il nome esatto dell'app.
