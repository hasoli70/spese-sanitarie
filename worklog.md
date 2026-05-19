# Worklog

Registro cronologico delle modifiche all'app Spese Sanitarie. Voce più recente in alto.

## 2026-05-19

### Bump cache service worker → v8
- `sw.js`: `CACHE = 'spese-sanitarie-v8'` per forzare il refresh della PWA su iOS dopo le modifiche precedenti che non venivano viste.
- Commit: `6c69a28`

### Scelta "Fotografa o Carica" dopo il numero di pagine
- `index.html`: aggiunto modal `sourceModal` con due bottoni — 📷 Fotografa / 📁 Carica.
- Il flusso ora è: categoria → numero pagine → fotografa/carica → fotocamera o selettore file di sistema.
- "Fotografa" imposta `capture="environment"` e `accept="image/*"` sul file input.
- "Carica" rimuove `capture` e imposta `accept="image/*,application/pdf"` (apre galleria/file, accetta anche PDF).
- Se il file caricato è un PDF, va dritto su Dropbox saltando il crop (`handleFile` controlla `file.type==='application/pdf'`).
- Se è un'immagine (sia da camera che da galleria) passa per il crop + filtro scansione + PDF multipagina come prima.
- `selectedSource` come variabile globale; rimossa `selectedFormat` (intermedia, mai usata in produzione).
- Fix layout `pagesModal` per schermi stretti: un solo emoji 📄 per bottone (prima i 4 emoji del bottone "4 pagine" rompevano il flex), `font-size:13px`, `padding:18px 2px`, `gap:6px`, `min-width:0` su ogni bottone.
- `sw.js`: cache v6 → v7.
- Commit: `ddf39a2`

## Note operative

- Repo: <https://github.com/hasoli70/spese-sanitarie>
- Hosting: GitHub Pages → <https://hasoli70.github.io/spese-sanitarie/>
- Cartella locale non è un repo git: per pushare si clona in temp, si copiano i file modificati, si committa e si pusha.
- Ogni modifica a `index.html` deve essere accompagnata da bump di `CACHE` in `sw.js`, altrimenti la PWA installata su iOS continua a servire la versione vecchia.
