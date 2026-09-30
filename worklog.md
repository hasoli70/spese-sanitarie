# Worklog

Registro cronologico delle modifiche all'app Documenti Famiglia (ex Spese Sanitarie). Voce più recente in alto.

## 2026-09-30

### Nuovo nome: Documenti Famiglia
- L'app archivia ormai ricevute, fatture e garanzie, non solo spese sanitarie: nome visibile "Documenti Famiglia", sotto l'icona "Documenti" (title, apple-mobile-web-app-title, header, schermata login, manifest, testo delle icone).
- Repo, URL GitHub Pages e cartelle Dropbox invariati: cambiarli romperebbe il redirect OAuth Dropbox e le PWA installate.
- `sw.js`: cache v13 -> v14.

### Visore documenti nell'app + versione visibile
- 👁 ora apre il documento (prima "nascondeva" la scheda fino al reload). Rimossa `hideReceipt`.
- Nuovo `viewerModal`: scarica il file con `files/download`, estrae i JPEG dal PDF (`extractJpegs`) e mostra le pagine come immagini, tocco per ingrandire, pinch-zoom abilitato solo nel visore; bottone "Apri ↗" per il file originale. Anche il tocco sulla scheda usa il visore (niente più `window.open`, inaffidabile nella PWA iOS).
- `dbxArg()`: header `Dropbox-API-Arg` con caratteri non ASCII escapati → upload di garanzie con titoli accentati non fallisce più.
- Numero versione visibile nell'header ("Spese Sanitarie · v13") per capire se il telefono ha l'ultima versione.
- `sw.js`: cache v12 → v13.

### Tab "⏰ Scadenze"
- Terza scheda con tutte le garanzie ordinate per scadenza (acquisto + 2 anni), raggruppate in: In scadenza (entro 90 giorni), Attive, Scadute, Senza data.
- Badge con giorni rimanenti (≤ 90 gg) o mesi; tocco → apre il PDF. Il pulsante fotocamera da questa scheda crea una nuova garanzia.
- `sw.js`: cache v11 → v12.

### Fotocamera per le pagine successive
- Dopo la pagina 1 l'app chiamava `fileInput.click()` da sola (dopo operazioni async) → su iOS bloccato, la fotocamera non si riapriva. Ora compare `nextPageModal` con il bottone "📷 Scatta pagina N": il tocco dell'utente riapre la fotocamera.
- `sw.js`: cache v10 → v11.

### Fix leggibilità PDF
- `imagesToPdf`: prima ogni foto veniva ridotta a 595×842 px (A4 a 72 dpi) → scontrini lunghi e stretti illeggibili. Ora l'immagine mantiene la sua risoluzione (lato lungo max 2400 px, JPEG 0.85) e viene scalata sulla pagina A4 dal PDF stesso. Corretto anche `/Width /Height` dell'XObject che non corrispondeva al JPEG reale.
- Filtro Scansione B/N: soglia "snap al bianco" 225 → 240 per non cancellare il testo sbiadito degli scontrini termici.
- `sw.js`: cache v9 → v10.

### Repository garanzie
- `index.html`: schede "🧾 Ricevute" / "🛡️ Garanzie" sopra la lista, con campo di ricerca (nome o percorso).
- Nella scheda Garanzie il pulsante fotocamera apre `warrantyModal`: titolo (obbligatorio) + data di acquisto, poi il solito flusso pagine → fotografa/carica → crop → PDF.
- Salvataggio in `/garanzie/<YYYY-MM-DD>_<titolo>.pdf` (titolo ripulito dai caratteri non ammessi da Dropbox).
- La scheda garanzia mostra titolo, data di acquisto e scadenza garanzia legale (+2 anni, rosso se scaduta).
- Tocco su una scheda (ricevuta o garanzia) → apre il PDF via `files/get_temporary_link`.
- `loadReceipts` legge anche `/garanzie`; nomi file ora escapati in HTML.
- `sw.js`: cache v8 → v9.

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
