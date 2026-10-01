# Documenti Famiglia

App PWA per fotografare ricevute, fatture e garanzie, ritagliarle, convertirle in PDF e salvarle su Dropbox. Fino al 30/09/2026 si chiamava "Spese Sanitarie": il repo e il link sono rimasti `spese-sanitarie` per non rompere il login Dropbox e le app già installate.

## Link

[https://hasoli70.github.io/spese-sanitarie/](https://hasoli70.github.io/spese-sanitarie/)

## Installazione sul telefono

**Android (Chrome):**
1. Apri il link con Chrome
2. Menu a tre puntini, "Aggiungi a schermata Home"
3. L'icona appare sulla home

**iPhone (Safari):**
1. Apri il link con Safari
2. Tocca Condividi
3. "Aggiungi a schermata Home"

## Funzionalita principali

- Fotografia diretta dall'app
- Scelta del numero di pagine per ricevuta: 1, 2, 3 o 4
- Ritaglio rettangolare regolabile per ogni foto: 4 barre sui lati permettono di impostare larghezza e altezza esatte del documento
- Filtro "Scansione B/N" attivabile: grayscale, stretch del contrasto, snap del bianco carta per un aspetto da scanner
- Generazione PDF multipagina senza librerie esterne
- Salvataggio automatico su Dropbox nella cartella `/spesefiscali_<anno>/<categoria>`
- Lista ricevute con anteprima e data
- Sezione **Garanzie**: scontrino + garanzia in un unico PDF, con titolo e data di acquisto, salvati in `/garanzie/`
- Ricerca per nome/cartella e apertura del PDF toccando la scheda
- Cancellazione da Dropbox direttamente dall'app
- Funzionamento offline (PWA con service worker)
- Token Dropbox salvato solo in locale sul dispositivo

## Categorie

Le categorie disponibili al momento della selezione sono:
- Medico (Oliver, Ilaria, Niccolo, Mattia)
- Farmacia (Oliver, Ilaria, Niccolo, Mattia)
- Veterinario
- Scuola
- Trasporto
- Assicurazioni
- CUD

La cartella di destinazione su Dropbox e' del tipo `/spesefiscali_2026/Medico/Oliver/ricevuta_<timestamp>.pdf`.

## Flusso d'uso

1. Tocca il pulsante fotocamera
2. Seleziona anno e categoria
3. Scegli quante pagine avra' la ricevuta (1-4)
4. Scatta la prima foto
5. Nell'editor di ritaglio, trascina le 4 barre sui lati per regolare il rettangolo intorno al documento. Lascia attivo il toggle "Scansione B/N" per un aspetto da scanner oppure disattivalo per un PDF a colori
6. Conferma con OK. Se hai scelto piu' di una pagina, la fotocamera si riapre per lo scatto successivo
7. Al termine l'app costruisce il PDF multipagina e lo carica su Dropbox

## Garanzie

Dalla scheda **🛡️ Garanzie** in alto:
1. Tocca il pulsante fotocamera
2. Inserisci un titolo (es. "Lavatrice Bosch") e la data di acquisto (default: oggi)
3. Scegli il numero di pagine (es. 2: prima lo scontrino, poi la garanzia) e scatta/carica le foto
4. Il PDF viene salvato come `/garanzie/<data acquisto>_<titolo>.pdf`

Nella lista ogni garanzia mostra titolo, data di acquisto e scadenza della garanzia (acquisto + 2 anni, oppure 1 anno se scelto al salvataggio: il file diventa `<data acquisto>_1anno_<titolo>.pdf`), evidenziata in rosso se scaduta.

La scheda **⏰ Scadenze** elenca tutte le garanzie ordinate per data di scadenza, divise in: in scadenza (entro 90 giorni), attive, scadute e senza data.

## Configurazione iniziale

Al primo avvio ti viene chiesto il login su Dropbox con OAuth (PKCE). Serve un'app Dropbox configurata con:
- App folder o Full Dropbox access
- Permessi: `files.content.write`, `files.content.read`, `account_info.read`
- Redirect URI corrispondente all'URL di deploy

La chiave pubblica (`APP_KEY`) e' hardcoded nel file `index.html`.

## File del progetto

| File | Descrizione |
|------|-------------|
| `index.html` | Applicazione completa (HTML, CSS, JS in un unico file) |
| `manifest.json` | Configurazione PWA (nome, icona, colori) |
| `sw.js` | Service worker per funzionamento offline |
| `README.md` | Questo documento |

## Deploy

Il sito e' pubblicato tramite GitHub Pages dal branch principale del repository.

Per aggiornare:
1. Modifica i file localmente
2. Incrementa il numero di versione in `sw.js` (`CACHE = 'spese-sanitarie-vX'`) per forzare il refresh della cache sui dispositivi gia' installati
3. Carica i file modificati su github.com (Add file, Upload files) oppure via git push
4. GitHub Pages rideploya automaticamente in circa 30 secondi

Per vedere l'aggiornamento sul telefono: ricarica la pagina in Chrome; se la PWA e' installata a schermo intero, chiudila dallo switcher app e riaprila, oppure disinstalla e reinstalla.

## Tecnologie

- HTML, CSS, JavaScript vanilla
- Dropbox API v2 (upload, list_folder, get_thumbnail, get_temporary_link, delete_v2, users/get_current_account)
- OAuth 2.0 con PKCE
- Canvas API per il ritaglio e il filtro scansione
- Generazione PDF 1.4 a mano (nessuna libreria)
- PWA con manifest e service worker
- GitHub Pages per l'hosting

## Privacy

- Il token Dropbox viene salvato solo in `localStorage` del browser
- Nessun server backend: l'app parla direttamente con le API Dropbox dal browser
- I file immagine vengono elaborati sul dispositivo e caricati cifrati in HTTPS su Dropbox

---

Sviluppato da Oliver Hasler, Val Gardena, 2026.
