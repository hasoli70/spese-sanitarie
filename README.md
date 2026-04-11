# 🏥 Spese Sanitarie

App PWA per fotografare e salvare le ricevute sanitarie direttamente su Dropbox.

## 🚀 Link App

**[https://hasoli70.github.io/spese-sanitarie/](https://hasoli70.github.io/spese-sanitarie/)**

## 📱 Come installare sul telefono

**Android (Chrome):**
1. Apri il link sopra con Chrome
2. Menu ⋮ → "Aggiungi a schermata Home"
3. L'icona verde appare sulla home!

**iPhone (Safari):**
1. Apri il link sopra con Safari
2. Tocca condividi □↑
3. "Aggiungi a schermata Home"

## ✨ Funzionalità

- 📸 **Fotocamera** — scatta foto alle ricevute direttamente dall'app
- ☁️ **Dropbox** — salvataggio automatico nella cartella `/Ricevute`
- 🗑️ **Elimina** — cancella le ricevute sbagliate con un tap
- 📋 **Lista** — vedi tutte le ricevute con anteprima e data
- 🔒 **Privato** — il token Dropbox è salvato solo sul tuo dispositivo
- 📴 **Offline** — funziona anche senza connessione (PWA)

## ⚙️ Configurazione

Al primo avvio inserisci il tuo **token Dropbox**:

1. Vai su [dropbox.com/developers/apps](https://www.dropbox.com/developers/apps)
2. Seleziona la tua app `spese-sanitarie`
3. Tab **Settings** → **Generated access token** → **Generate**
4. Copia il token e incollalo nell'app

## 📁 File

| File | Descrizione |
|------|-------------|
| `index.html` | App completa (HTML + CSS + JS) |
| `manifest.json` | Configurazione PWA e icona |
| `sw.js` | Service Worker per uso offline |

## 🛠️ Tecnologie

- HTML / CSS / JavaScript vanilla
- Dropbox API v2
- PWA (Progressive Web App)
- GitHub Pages (hosting gratuito)

---

*Sviluppato da Oliver Hasler — Val Gardena, 2026*
