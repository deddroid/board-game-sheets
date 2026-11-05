# Cluedo - Detective Notes — stato del progetto

Aggiornato al 06/11/2026. Stato: v1.0.1 — MVP funzionante, testato e ottimizzato (`APP_VERSION` 1.0.1, `APP_DATE` 2026-11-06).
I numeri qui riportati vanno misurati, non stimati.

## Cosa fa

App **open source** che sostituisce il foglio investigazioni cartaceo del Cluedo: la griglia
sospetti / armi / stanze × giocatori dove si segna cosa è escluso o confermato, così non si spreca
carta. Replica le funzionalità dell'app di riferimento **"Get a Clue"** (clean-room) e in più
aggiunge un **blocco appunti** libero — l'unica cosa che a Get a Clue manca.

## Fatto (MVP)

Un unico file `index.html` autonomo, nessuna dipendenza, funziona offline. Apribile dal telefono
aprendo il file nel browser.

- **Griglia** sospetti / armi / stanze × giocatori, ottimizzata per il telefono (dark mode
  automatica). **Scorrimento unico di pagina**: la **riga dei nomi (intestazioni)** resta fissa in
  alto appena sotto l'header dell'app (sticky `top: var(--header-h)`), quindi è sempre visibile a
  qualunque altezza di scroll — niente più risalire tutta la pagina per rivederla. La **prima colonna**
  resta fissa a sinistra.
- **Ridimensionamento della griglia (zoom)**: controllo in Impostazioni (− / % / + e **Adatta**) che
  scala tutta la tabella via `--ui-scale`. All'avvio e al ridimensionamento si **adatta da sola alla
  larghezza** (riduce solo quanto serve) così entra tutto senza scorrere a sinistra/destra. Nomi
  giocatori di default brevi (Io, G2…G6) e colonne strette per lasciare celle più grandi.
- **Marcatori per cella**: tap che cicla vuoto → ✗ (non ce l'ha) → ✓ (ce l'ha) → ? (forse) → vuoto.
- **Deduzione automatica** (attivabile/disattivabile): quando segno ✓ a un giocatore, gli altri
  ricevono ✗ in automatico; una carta esclusa a *tutti* viene evidenziata come **SOLUZIONE**.
  Le ✗ messe in automatico sono tracciate (`state.auto`): togliendo la ✓ (✓→?) spariscono, mentre
  le ✗ messe a mano restano. Così è sempre possibile azzerare una riga.
- **Istruzioni con le regole di Cluedo**: la modale ❓ include ora un riassunto delle regole del
  gioco (obiettivo, preparazione, turno/ipotesi, confutazione, accusa), scritto con parole proprie
  (clean-room), oltre alla guida all'uso dell'app, EN/IT.
- **Gestione giocatori**: parte da **6 giocatori** (il massimo del Cluedo: togliere è più veloce che
  aggiungere); aggiungi/rimuovi/rinomina, scelta di quale sei "tu" (colonna evidenziata).
- **Le mie carte**: pannello con checklist per segnare la propria mano (✓ a me, ✗ agli altri).
- **Carte personalizzabili**: rinomina/aggiungi/togli carte per edizione diversa; reset ai default.
- **Edizioni (preset)**: icona 🃏 + chip con l'edizione attiva. Edizione predefinita "Classico" (EN/IT);
  si possono salvare edizioni personalizzate dalle carte correnti, rinominarle, eliminarle e passare
  dall'una all'altra (il cambio chiede conferma e azzera i segni, il blocco appunti resta).
- **Istruzioni**: icona ❓ con una guida all'uso dell'app + consigli rapidi di gioco (EN/IT).
- **UI**: header compatto a una riga (logo + titolo + chip edizione + **menu ☰**); le opzioni usate
  di rado — lingua, giocatori, edizioni, le mie carte, impostazioni, istruzioni, nuova partita — sono
  raccolte nel menu, così resta più spazio alla griglia sul telefono. Colori per categoria
  (sospetti/armi/stanze), chip con l'edizione attiva.
- **Tema**: selettore in Impostazioni **Auto / Chiaro / Scuro**. "Auto" segue il sistema
  (`prefers-color-scheme`); "Chiaro"/"Scuro" forzano il tema via `data-theme` su `<html>` e vincono
  sul sistema (salvato in `state.theme`).
- **Privacy**: tasto 🙈 nell'header che copre tutto lo schermo (foglio + appunti) con una schermata
  "Contenuto nascosto" e un pulsante **Mostra** per tornare. Serve a non far copiare gli appunti agli
  altri. In memoria (un refresh lo azzera).
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
- Header compatto + menu, 6 giocatori di default, intestazioni sticky su entrambi gli assi: OK.
- **Flicker intestazioni risalendo**: la griglia ora scorre in un elemento interno senza angoli
  arrotondati (il bordo tondo sta sul riquadro esterno con `overflow:hidden`); tolto anche
  `-webkit-overflow-scrolling`. Riduce il ridisegno della riga sticky scorrendo verso l'alto su
  Chrome. Essendo un artefatto di compositing di 1 frame, difficile da catturare in automatico: da
  controllare sul telefono reale.
- **Test funzionali** (eseguiti nel contesto della pagina, 17/17 PASS): default 6 giocatori e un solo
  "io"; carte classiche 6/6/9; ciclo dei marcatori; cascata ✗ auto che preserva le ✗ a mano;
  un-cascade che toglie solo le auto; rilevamento SOLUZIONE; cambio edizione che azzera segni/auto e
  scambia le carte; `activePreset`; differenza i18n IT/EN.
- **Scroll unico + zoom/fit** (nuova suite, 15/15 PASS, viewport 375px): 6 giocatori con nomi brevi;
  adattamento alla larghezza senza scroll orizzontale (griglia ≤ larghezza disponibile); intestazioni
  colonne fissate appena sotto l'header dell'app anche in fondo alla pagina; `setScale` con clamp
  [0.5, 1.3]; `fitGrid` che adatta; variabile `--ui-scale` applicata; regressioni OK su
  cascata/un-cascade, tema e privacy. Header forzato a una riga (titolo troncato) a 375px.

## Test di attendibilità (06/11/2026)

Eseguito nel pannello browser dell'app desktop, con una suite in-page che pilota la **UI reale**
(click sulle celle, pulsanti dei modali, eventi `input`/`change`, `prompt`/`confirm` sostituiti),
più controlli visivi con screenshot. Servito da `http://127.0.0.1:8765/…` (localStorage attivo);
il `file://` nel pannello diventa un `data:` URL senza storage ed è servito da prova "senza
localStorage". Viewport: telefono 375×812 (preset mobile), tablet 768×1024, desktop.

**Cosa è stato testato (tutto OK salvo le anomalie sotto):**
- Griglia: ciclo vuoto → ✗ → ✓ → ? → vuoto su **tutte le 126 celle** (3 sezioni × 6 giocatori),
  classe e simbolo coerenti a ogni passo; colonna "Io" evidenziata (21 celle `col-me`, ★ in testa).
- Deduzione automatica: ✓ → ✗ automatiche solo nelle celle vuote, tracciate in `state.auto`;
  ✗ manuali mai toccate; ✓ → ? ritira solo le ✗ automatiche; con deduzione spenta nessuna
  cascata; SOLUZIONE evidenziata quando una carta è ✗ per tutti e tolta appena cambia.
- "Le mie carte": spunta → ✓ a me + ✗ auto agli altri; togli spunta → tutto pulito; riapertura
  coerente; messaggio se nessun giocatore è "io".
- Giocatori: default 6 con un solo "io"; aggiunta (7ª colonna), rinomina (intestazione aggiornata
  al "Fatto"), scelta "io" (colonna evidenziata si sposta), rimozione (segni del giocatore
  eliminati), fallback di "io" sul primo se rimosso, minimo 2 giocatori.
- Edizioni: lista (Classico predefinita + personalizzate), "Salva le carte attuali come edizione"
  (i segni restano), rinomina, elimina, cambio con conferma (azzera segni/auto, **tiene** gli
  appunti, aggiorna il chip), tap sull'edizione attiva che chiude e basta.
- Nuova partita (annulla / conferma), blocco appunti (salvato a ogni tasto), istruzioni (7 sezioni,
  lingua corretta), menu (6 voci + lingua), tema Auto/Chiaro/Scuro (`data-theme` e colori di sfondo
  verificati), zoom −/+/Adatta, copertura privacy (nascondi/mostra).
- Persistenza: ricarica con 6 giocatori e 63 celle piene → segni, auto, giocatori, appunti, tema,
  lingua, edizione tutti ripristinati; storage corrotto → stato nuovo senza errori; **senza
  localStorage** (iframe `sandbox="allow-scripts"`) l'app parte, 126 celle, i tap funzionano.
- Layout: a 375px nessuno scorrimento orizzontale (griglia 349px, scala auto 0,86–0,88),
  intestazione colonne fissa esattamente sotto l'header dell'app (59px) anche a scroll 545px,
  prima colonna fissa; tablet e desktop OK; chiaro/scuro OK.
- i18n: nessuna chiave mancante tra EN e IT (anche `help.*`); dopo il cambio lingua cambiano
  etichette statiche, placeholder, titolo, sezioni, modali e tag SOLUZIONE.

**Anomalie trovate e corrette (commit "fix auto-deduction edge cases…"):**
1. **Due ✓ sulla stessa carta**: togliendone una sparivano anche le ✗ automatiche che l'altra ✓
   giustificava ancora. 2. **Rimozione di un giocatore con ✓**: le ✗ automatiche derivate restavano
   orfane (con 6 giocatori la carta finiva segnalata come SOLUZIONE sbagliata). → Entrambe risolte
   sostituendo cascade/uncascade con `reconcileAuto(cardId)`: ritira tutte le ✗ automatiche della
   riga e le rideriva dai ✓ presenti; i segni manuali non vengono mai toccati; con deduzione spenta
   non fa nulla (comportamento invariato).
3. Eliminando una carta dall'editor restava la sua voce in `state.auto` → pulita.
4. "Ripristina le carte predefinite" lasciava attiva nel chip un'edizione personalizzata sopra
   carte Classico → ora riattiva "Classico".
5. "Annulla" nella conferma di cambio edizione chiudeva anche la lista → torna alla lista.
6. Cambiando lingua le carte dell'edizione Classico restavano nella lingua precedente → ora le
   carte che hanno ancora il nome predefinito seguono la lingua (stessi id: i segni restano);
   carte rinominate ed edizioni personalizzate non vengono toccate. *Scelta di UX segnalata
   all'utente: prudente ma è una decisione.*
7. Tooltip/aria-label dei tre pulsanti dell'header ("Editions", "Privacy", "Menu") e il titolo del
   pulsante ✕ dei giocatori ("remove") non erano tradotti → chiave `privacy` + `data-i18n-title`.

**Non modificato, da sapere:** la scala ridotta per adattarsi al telefono resta salvata
(`uiScale`) anche se poi si apre su uno schermo largo: è la scelta "solo riduzione" documentata
(si torna a 100% con "Adatta"/+). Il titolo è troncato con "…" a 375px (voluto, header a una riga).

## Ottimizzazione (06/11/2026)

Protocollo: `performance.now()` nel browser, caso realistico (6 giocatori, 63 celle su 126 piene,
appunti, IT), 20–21 ripetizioni per misura, **mediana**; caricamento da
`performance.getEntriesByType("navigation")` su 11 caricamenti **alternati vecchio/nuovo** (stessa
macchina, stesso momento: il rumore tra un caricamento e l'altro è maggiore delle differenze).

| Misura | Prima (v1.0.0) | Dopo (v1.0.1) |
|---|---:|---:|
| Caricamento: `domContentLoaded` | ≈ 109 ms | ≈ 115 ms (invariato entro il rumore, 100–230) |
| Caricamento: parse+script (`responseEnd → DCL`) | ≈ 93 ms | ≈ 97 ms (idem) |
| `render()` a forma invariata (es. chiusura di un modale) | 12,4 ms | **1,7 ms** |
| `render()` con ricostruzione della tabella | 12,4 ms | 7,6 ms |
| Tap su una cella (click reale, ciclo marcatore) | 11,3 ms | **0,25 ms** |
| Cambio lingua + render | 13,3 ms | 9,5 ms |
| Cambio numero giocatori + render | 12,2 ms | 8,5 ms |
| `save()` (JSON + localStorage) | 0,2 ms | 0,1 ms |

Cosa è cambiato (stesso comportamento, stesso aspetto, stesso file unico):
- **Griglia incrementale**: la tabella viene ricostruita solo se cambia la sua *forma* (giocatori,
  carte, lingua: `gridKey()`); altrimenti `paintCard(cardId)` aggiorna solo classe/simbolo delle
  celle e il tag SOLUZIONE della riga toccata. Un tap aggiorna una riga invece di rifare 126 celle.
- **Event delegation**: un solo listener sul `<table>` (celle con `data-c`/`data-p`) al posto di
  126 listener ricreati a ogni render.
- **Deduzione centralizzata**: `setCell()` (tap in griglia e spunta in "Le mie carte") →
  `reconcileAuto()`; sparite le due funzioni cascade/uncascade duplicate.
- **Helper DOM** (`el`, `btn`, `byId`, `each`), **modali** (`modalHead`, `actionBar` con il "Fatto"
  predefinito, `confirmModal` per Nuova partita e Cambio edizione, `segmented` + `labelRow` per i
  selettori lingua e tema), **carte** (`cardsFromNames`, `replaceCards` per edizione/reset, prima
  tre copie dello stesso ciclo), i18n in `applyI18n()`.
- **Codice morto rimosso**: query su `#lang-toggle` (elemento inesistente), chiavi i18n `meFull`,
  `save`, `loadEdition`, CSS `.toolbar`/`header .spacer`, doppia regola `td.card-cell` (unita),
  `updateHeaderOffset()` ripetuta all'avvio.
- Scartato: debounce del salvataggio appunti (0,1 ms, non vale la complessità) e unificazione delle
  due copie della palette scura in CSS (servirebbe `light-dark()`: cambio di tecnica, non solo di
  forma).

Regressione dopo il refactor: la stessa suite in-page (griglia, deduzione, giocatori, edizioni,
mie carte, nuova partita, appunti, istruzioni, tema, zoom, privacy, i18n, sandbox senza storage,
layout mobile) tutta PASS; larghezza naturale della griglia identica (414,09 px a 6 giocatori).

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
- **Set di carte predefiniti:** solo EN/IT (deciso dall'utente, 01/11/2026). Altre lingue/edizioni si
  creano a mano come edizioni personalizzate (già supportate). Niente set predefiniti in altre lingue.

## Vincoli

- **Clean-room:** si replicano le *funzionalità*, non si copiano codice, grafica, asset o marchio di
  "Get a Clue".
- GUIDA-PROGETTI: codice/commit/README/UI in inglese con italiano selezionabile; **chiedere prima**
  di pubblicare (GitHub) o scegliere la licenza. Repo non ancora pubblicato.

## Strumenti

- REA (se l'app di riferimento è un APK): `~/Code/ZTool/re/` — `jadx`, `apktool`, `platform-tools/adb`.
  Non ancora usato: serve il nome esatto dell'app.
