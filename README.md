# Galleria Fotografica Dinamica

Una galleria fotografica web **completamente client-side**, senza dipendenze esterne da installare e senza alcun server necessario. Basta aprire `index.html` nel browser, selezionare una cartella con le proprie foto e la galleria si costruisce da sola.

---

## Funzionalità

### Galleria
- **Caricamento dinamico** — clicca "Apri cartella" e seleziona la cartella con le foto; la galleria si popola automaticamente senza toccare il codice
- **Griglia adattiva** — CSS Grid con colonne flessibili (`auto-fill`, min 220px), aspect-ratio 3:4, immagini ritagliate con `object-fit: cover`
- **Skeleton loading con shimmer** — animazione di caricamento su ogni card, scompare con fade quando l'immagine è pronta
- **Ricerca testuale** — filtra in tempo reale su nome file, modello dispositivo, software, descrizione, data
- **Ordinamento** — ordine originale, data crescente/decrescente, nome A→Z / Z→A
- **Contatore foto** — mostra il totale e il risultato del filtro attivo

### Vista dettaglio
- **Apertura a schermo intero** — pannello immagine a sinistra, info a destra
- **Zoom click** — click sull'immagine zooma 2.8× nel punto cliccato; secondo click (o `Esc`) resetta
- **Navigazione** — frecce nel nav bar, tasti `←` `→` della tastiera, swipe orizzontale su touch
- **Transizione fade** — cambio immagine con dissolvenza (200ms), lock anti-race-condition
- **Striscia miniature** — thumbnail strip scrollabile orizzontalmente, miniatura attiva evidenziata e centrata automaticamente
- **Download** — pulsante "Scarica" che forza il salvataggio del file originale
- **URL con hash** — `#<indice>` nell'URL per condividere/riaprire una foto specifica; gestione del tasto Back del browser

### Metadati EXIF
- Lettura EXIF dinamica lato client tramite **[exifr](https://github.com/MikeKovarik/exifr)** (caricato da CDN)
- Dati mostrati: data e ora scatto, produttore, modello dispositivo, software, ISO, apertura, tempo di esposizione, lunghezza focale, flash, orientamento
- **Coordinate GPS** con link diretto a OpenStreetMap (se disponibili nell'EXIF)
- Cache locale per evitare letture ripetute della stessa foto
- Fallback elegante se i dati EXIF non sono disponibili

### Design
- Tema scuro (`#0f0f0f`) con accent oro/sabbia (`#c8a96e`)
- Font di sistema (`system-ui`)
- Header sticky durante lo scroll
- Responsive: sotto 680px il pannello info scende sotto l'immagine; nav bar compatta su mobile

---

## Come si usa

1. **Apri** `index.html` nel browser (doppio click — nessun server necessario)
2. **Clicca** il pulsante **"Apri cartella"** nell'header
3. **Seleziona** la cartella che contiene le tue foto
4. La galleria si costruisce automaticamente con tutti i file immagine trovati

Per aggiornare con nuove foto, basta cliccare di nuovo "Apri cartella".

### Formati supportati
`jpg` · `jpeg` · `png` · `gif` · `webp` · `heic` · `heif` · `bmp` · `tiff` · `avif`

---

## Struttura del progetto

```
galleria-foto-dinamica/
├── index.html        # Tutta l'app: HTML + CSS + JavaScript in un unico file
├── img/              # Cartella di esempio (vuota — aggiungi le tue foto qui)
│   └── .gitkeep
├── info.md           # Log dettagliato delle modifiche e dei metadati
├── .gitignore
└── README.md
```

---

## Stack tecnico

| Componente | Tecnologia |
|---|---|
| Struttura | HTML5 |
| Stile | CSS3 (Grid, custom properties, animazioni) |
| Logica | JavaScript vanilla (ES2020+) |
| EXIF | [exifr@7.1.3](https://github.com/MikeKovarik/exifr) via CDN |
| Mappe | OpenStreetMap (link esterno) |
| Server | Nessuno — funziona da `file://` |

Nessun build step, nessun `npm install`, nessun framework.

---

## Compatibilità browser

| Browser | Supporto |
|---|---|
| Chrome / Edge | Completo (incluso `webkitdirectory`) |
| Safari (macOS) | Completo |
| Firefox | Completo |
| Mobile (iOS/Android) | Parziale — selezione singoli file invece della cartella |

> Su mobile, `webkitdirectory` non è supportato: il dialogo permette di selezionare file singoli invece dell'intera cartella.

---

## Note sulla privacy

Le immagini **non lasciano mai il dispositivo**. Tutto il processing (lettura file, parsing EXIF, rendering) avviene nel browser localmente. L'unica richiesta di rete è il caricamento della libreria `exifr` da CDN (jsdelivr) al primo avvio.
