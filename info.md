# Progetto Galleria Fotografica

## Struttura
```
Gallerie foto dinamica/
├── index.html        # Pagina web principale
├── info.md           # Questo file
└── img/              # Cartella immagini
    ├── 20141114_114135_074.jpg
    ├── 20150223_140221_403.jpg
    ├── IMG_0050.JPG
    ├── IMG_1091.JPG
    ├── IMG_3678.JPG
    ├── IMG_3700.JPG
    └── IMG_4488.JPG
```

## Immagini — Metadati

| File | Data scatto | Dispositivo | Software | Dimensioni | DPI | Descrizione |
|------|-------------|-------------|----------|------------|-----|-------------|
| 20141114_114135_074.jpg | — | — | — | 960×1280 | 72 | — |
| 20150223_140221_403.jpg | 2015-02-23 14:02:18 | Apple iPhone 6 Plus (A1524) | Retrica 2.3.2 | 960×1280 | 72 | — |
| IMG_0050.JPG | 2015-10-30 17:09:58 | Apple iPhone 6 Plus | Retrica 2.7 | 960×1280 | 72 | — |
| IMG_1091.JPG | 2016-02-20 11:38:41 | Apple iPhone 6 Plus | Retrica | 960×1280 | 72 | "Pew Pew 100%" |
| IMG_3678.JPG | 2015-07-12 23:06:49 | Apple iPhone 6 Plus | Retrica 2.6 | 960×1280 | 72 | — |
| IMG_3700.JPG | 2015-07-14 15:48:03 | Apple iPhone 6 Plus | Retrica 2.6 | 960×1280 | 72 | — |
| IMG_4488.JPG | 2015-08-17 09:30:26 | Apple iPhone 6 Plus | Retrica 2.7 | 960×1280 | 72 | — |

**Note:**
- Tutte le immagini sono JPEG, spazio colore sRGB, 3 canali RGB, 8 bit per campione
- `20141114_114135_074.jpg` è l'unica senza dati EXIF (no data, no dispositivo)
- GPS non disponibile in nessuna immagine
- Tutte scattate con iPhone 6 Plus tramite app Retrica (filtri fotografici)

---

## Cronologia modifiche

### [1] — Creazione galleria web
**File creato:** `index.html`

**Cosa è stato fatto:**
1. Esplorata la cartella di lavoro `/Prova 123/` — trovata sottocartella `img/` con 7 immagini JPEG
2. Estratti i metadati di ogni immagine tramite il comando `sips` (tool nativo macOS), ricavando: dimensioni in pixel, DPI, spazio colore, formato, profilo colore, data scatto, produttore, modello dispositivo, software, descrizione
3. Creato `index.html` — pagina web completa, senza dipendenze esterne, apribile direttamente nel browser

**Dettaglio tecnico di index.html:**
- **Header**: titolo "Galleria" + contatore foto
- **Griglia galleria**: CSS Grid adattivo (`auto-fill`, colonne min 220px, gap 2px). Ogni cella ha aspect-ratio 3:4, immagini con `object-fit: cover`. Hover: zoom + overlay con nome file
- **Vista dettaglio** (attivata al click): pannello fullscreen con:
  - Immagine a sinistra (`object-fit: contain`, sfondo nero, box-shadow)
  - Pannello info a destra (320px fissi, scrollabile) con sezioni: File, Fotocamera, Metadati, GPS
  - I metadati mancanti vengono omessi automaticamente (nessuna riga vuota)
- **Navigazione dettaglio**: frecce ‹ › nel header + tasti freccia della tastiera (`←` `→`) + `Esc` per chiudere
- **Design**: tema scuro (`#0f0f0f`), accent oro/sabbia (`#c8a96e`), font system-ui
- **Responsive**: sotto 680px il pannello info scende sotto l'immagine
- **Dati**: metadati hardcodati nell'array JS `images[]`, estratti in fase di sviluppo

---

### [2] — Creazione info.md
**File creato:** `info.md`

**Cosa è stato fatto:**
- Creato il file `info.md` per tenere traccia della struttura del progetto, dei metadati delle immagini e delle caratteristiche tecniche della pagina web
- Su richiesta dell'utente, il file è stato riscritto per includere anche la cronologia dettagliata di tutte le modifiche (questa sezione)
- Da questo momento ogni modifica richiesta viene registrata qui

---

### [3] — Implementazione 11 miglioramenti
**File modificato:** `index.html` (riscrittura completa)

**Miglioramenti implementati:**

1. **Zoom sull'immagine** — nel dettaglio, click sull'immagine zooma 2.8× nel punto cliccato (transform-origin dinamico). Secondo click resetta. `Esc` resetta anche lo zoom. Durante lo zoom, swipe e frecce sono disabilitati per evitare conflitti.

2. **Transizione animata tra foto** — navigando tra le immagini (frecce, thumbnail, swipe), l'immagine corrente fa un fade-out (opacity 0, 200ms), poi viene caricata la nuova e si fa fade-in. Lock `navigating` evita click multipli durante l'animazione.

3. **Skeleton loading con shimmer** — ogni card in galleria mostra un'animazione shimmer (gradiente che scorre, `background-size: 200%`) finché l'immagine non è caricata. All'evento `load` o `error` la card riceve la classe `.loaded` che nasconde lo skeleton con transizione opacity.

4. **Striscia miniature (thumbnail strip)** — nella vista dettaglio, sotto l'immagine: riga scrollabile orizzontale con tutte le foto visibili. La foto corrente ha bordo oro e opacity 1. Al click, fade + apertura foto. La thumbnail attiva viene scrollata al centro automaticamente.

5. **Filtro e ordinamento** — select nel header con 5 opzioni: ordine originale, data ↑, data ↓, nome A→Z, nome Z→A. Il sort agisce sull'array `visibleIndices` che guida galleria, counter e navigazione nel dettaglio.

6. **Ricerca testuale** — input nel header, filtra in tempo reale su: nome file, modello dispositivo, software, descrizione, data. Mostra "N / 7 foto" quando filtrata. La ricerca agisce insieme al sort.

7. **Download dell'immagine** — pulsante nel nav bar del dettaglio, usa tag `<a download>` con `href` e `download` aggiornati ad ogni foto. L'attributo `download` forza il salvataggio invece dell'apertura nel browser.

8. **Contatore posizione** — nel nav bar del dettaglio: "3 / 7" che mostra la posizione corrente all'interno delle foto visibili (rispetta filtri attivi).

9. **Lettura EXIF dinamica (exifr.js)** — libreria `exifr@7.1.3` caricata da CDN (jsdelivr). Parsing asincrono con `exifr.parse()` per ogni foto aperta nel dettaglio. I dati aggiuntivi (ISO, apertura, esposizione, focale, flash, GPS, orientamento) vengono mostrati nel pannello info se disponibili. Cache locale in `exifCache{}` per non rileggere. Fallback ai dati hardcodati se exifr non disponibile (protocollo file://) o se il parsing fallisce.

10. **URL con hash** — apertura del dettaglio aggiorna l'URL con `#<indice>` via `history.replaceState`. La chiusura rimuove il hash. Listener `popstate` gestisce il pulsante back del browser. Al caricamento iniziale, se c'è un hash valido, apre direttamente la foto corrispondente.

11. **Supporto touch/swipe** — nel dettaglio, `touchstart`/`touchend` rilevano swipe orizzontale. Soglia 48px. Swipe sinistra → foto successiva, destra → precedente. Disabilitato quando è attivo lo zoom. Listener `passive: true` per performance.

**Altre migliorie:**
- Header sticky (rimane visibile durante lo scroll della galleria)
- Nav bar dettaglio adattiva su mobile (titolo nascosto, label "Scarica" nascosta)
- `Esc` nel dettaglio: primo Esc resetta lo zoom, secondo Esc chiude il dettaglio
- Navigazione frecce tastiera disabilitata durante lo zoom
- Separatore `navigating` lock per evitare race condition durante le transizioni

---

### [4] — Rinomina cartella
**Operazione:** rinomina cartella di progetto

- Cartella rinominata da `Prova 123/` a `Gallerie foto dinamica/`
- Aggiornato il percorso nella sezione Struttura di questo file

---

### [5] — Galleria dinamica con file picker
**File modificato:** `index.html` (riscrittura completa)

**Cosa è stato fatto:**
- Rimosso l'array `images[]` hardcodato con i 7 file fissi
- Aggiunto pulsante **"Apri cartella"** nell'header (color accent, stile coerente)
- Aggiunto `<input type="file" webkitdirectory>` nascosto collegato al pulsante
- La galleria parte vuota con un messaggio "Nessuna cartella selezionata"

**Flusso dinamico:**
1. L'utente clicca "Apri cartella" → si apre il dialogo di selezione cartella
2. Il JS filtra i file per estensione immagine (jpg, jpeg, png, gif, webp, heic, heif, bmp, tiff, avif)
3. Per ogni file viene creato un `URL.createObjectURL()` (blob URL) e un record minimo `{file, filename, width:null, ...}`
4. Il File object viene salvato in `fileMap[filename]` per l'accesso EXIF
5. La galleria viene costruita e i metadati vengono letti in modo asincrono tramite `exifr.js` direttamente dai `File` object (affidabile anche da `file://`)
6. Le dimensioni reali vengono rilevate al momento del load dell'immagine tramite `naturalWidth`/`naturalHeight`
7. `img.date` viene aggiornato dall'EXIF per consentire l'ordinamento per data
8. I blob URL precedenti vengono revocati (`URL.revokeObjectURL`) ad ogni nuovo caricamento cartella

**Variabili JS aggiunte:**
- `fileMap = {}` — mappa `filename → File` object
- `objectURLs = []` — lista blob URL per cleanup
- `handleFolderSelect(input)` — gestisce la selezione cartella

**Note tecniche:**
- Funziona direttamente da `file://` senza alcun server locale
- Compatibile con tutti i browser moderni (Chrome, Safari, Firefox, Edge)
- `webkitdirectory` su mobile mostra una selezione file standard (l'attributo cartella è solo desktop)
- Tutte le funzionalità precedenti sono mantenute: zoom, fade, swipe, thumbnail strip, ricerca, ordinamento, download, URL hash, tastiera
