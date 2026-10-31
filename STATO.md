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

- **Griglia** sospetti / armi / stanze × giocatori, intestazioni e prima colonna fisse
  (scroll orizzontale), ottimizzata per il telefono (tap target grandi, dark mode automatica).
- **Marcatori per cella**: tap che cicla vuoto → ✗ (non ce l'ha) → ✓ (ce l'ha) → ? (forse) → vuoto.
- **Deduzione automatica** (attivabile/disattivabile): quando segno ✓ a un giocatore, gli altri
  ricevono ✗ in automatico; una carta esclusa a *tutti* viene evidenziata come **SOLUZIONE**.
- **Gestione giocatori**: aggiungi/rimuovi/rinomina, scelta di quale sei "tu" (colonna evidenziata).
- **Le mie carte**: pannello con checklist per segnare la propria mano (✓ a me, ✗ agli altri).
- **Carte personalizzabili**: rinomina/aggiungi/togli carte per edizione diversa; reset ai default.
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
