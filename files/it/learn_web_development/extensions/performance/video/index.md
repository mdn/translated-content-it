---
title: "Multimedia: video"
slug: Learn_web_development/Extensions/Performance/video
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/Multimedia", "Learn_web_development/Extensions/Performance/Javascript", "Learn_web_development/Extensions/Performance")}}

Come abbiamo appreso nella sezione precedente, i media, cioè immagini e video, rappresentano oltre il 70% dei byte scaricati per il sito web medio. Abbiamo già esaminato l'ottimizzazione delle immagini. Questo articolo si occupa di ottimizzare i video per migliorare le prestazioni web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        >, e conoscenza di base delle
        <a href="/it/docs/Learn_web_development/Getting_started/Your_first_website"
          >tecnologie web lato client</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare i vari formati video, il loro impatto sulle prestazioni e come ridurre l'impatto del video sul tempo di caricamento complessivo della pagina servendo il file video più piccolo possibile in base al supporto del formato di file di ciascun browser.
      </td>
    </tr>
  </tbody>
</table>

## Perché ottimizzare il proprio multimedia?

Per il sito web medio, [il 25% della larghezza di banda proviene dai video](https://discuss.httparchive.org/t/state-of-the-web-top-image-optimization-strategies/1367). L'ottimizzazione dei video ha il potenziale per notevoli risparmi in termini di larghezza di banda che si traducono in un miglioramento delle prestazioni del sito web.

## Ottimizzare la distribuzione video

Le sezioni seguenti descrivono le seguenti tecniche di ottimizzazione:

- [compress all video](#compress_all_videos)
- [ottimizzare l'ordine di `<source>`](#optimize_source_order)
- [rimuovere l'audio dai video muti](#rimuovere_l'audio_dai_video_muti)
- [ottimizzare il preload dei video](#video_preload)
- [considerare lo streaming](#considerare_lo_streaming)

### Compress all videos

La maggior parte del lavoro di compressione video confronta i fotogrammi adiacenti all'interno di un video, con l'intento di rimuovere i dettagli che sono identici in entrambi i fotogrammi. Comprimere il video ed esportarlo in più formati video, inclusi WebM e MPEG-4/H.264.

Il suo software di editing video probabilmente dispone di una funzione per ridurre la dimensione dei file. Se no, ci sono strumenti online, come [FFmpeg](https://www.ffmpeg.org/) (discusso nella sezione seguente), che codificano, decodificano, convertono e svolgono altre funzioni di ottimizzazione.

### Ottimizzare l'ordine di `<source>`

Ordinare le fonti video dalla più piccola alla più grande. Per esempio, dati i video compressi nei formati di 10MB e 12MB, dichiarare prima la risorsa di 10MB:

```html
<video width="400" height="300" controls="controls">
  <!-- WebM: 10 MB -->
  <source src="video.webm" type="video/webm" />
  <!-- MPEG-4/H.264: 12 MB -->
  <source src="video.mp4" type="video/mp4" />
</video>
```

Il browser scarica il primo formato che comprende. L'obiettivo è offrire versioni più piccole prima di quelle più grandi. Con la versione più piccola, assicurarsi che il video più compresso sia comunque di buona qualità. Ci sono alcuni algoritmi di compressione che possono far sembrare il video (cattivo) come una GIF animata. Anche se un video di 128 Kb può sembrare offrire una migliore esperienza utente rispetto a un download di 10 MB, un video granuloso simile a una GIF può riflettersi negativamente sul marchio o sul progetto.

### Rimuovere l'audio dai video muti

Per i video di tipo "hero" o altri video senza audio, rimuovere l'audio è intelligente.

```html
<video autoplay="" loop="" muted playsinline="" id="hero-video">
  <source src="banner_video.webm" type='video/webm; codecs="vp8, vorbis"' />
  <source src="web_banner.mp4" type="video/mp4" />
</video>
```

Questo codice di hero-video (sopra) è comune nei siti web di conferenze e home page aziendali. Include un video che è in autoplay, in loop e muto. Non ci sono controlli, quindi non c'è modo di ascoltare l'audio. L'audio è spesso vuoto, ma comunque presente, e continua a utilizzare larghezza di banda. Non c'è motivo di servire audio con video che è sempre muto. **Rimuovere l'audio può risparmiare il 20% della larghezza di banda.**

A seconda della scelta del software, potrebbe essere possibile rimuovere l'audio durante l'esportazione e la compressione. Se no, un programma gratuito chiamato [FFmpeg](https://www.ffmpeg.org/) può farlo per lei. Questa è la stringa di comando FFmpeg per rimuovere l'audio:

```bash
ffmpeg -i original.mp4 -an -c:v copy audioFreeVersion.mp4
```

### Video preload

L'attributo preload ha tre opzioni disponibili: `auto`, `metadata`, e `none`. L'impostazione predefinita è `metadata`. Queste impostazioni controllano la quantità di un file video scaricata durante il caricamento della pagina. È possibile risparmiare dati rimandando il download per i video meno popolari.

Impostare `preload="none"` risulta nel non scaricare nessun video fino alla riproduzione. Ciò ritarda l'avvio, ma offre significativi risparmi di dati per video con una bassa probabilità di riproduzione.

Offrendo risparmi di larghezza di banda più modesti, impostare `preload="metadata"` può scaricare fino al 3% del video durante il caricamento della pagina. Questa è un'opzione utile per alcuni file piccoli o di dimensioni moderate.

Cambiare l'impostazione su `auto` indica al browser di scaricare automaticamente l'intero video. Faccia ciò solo quando la riproduzione è molto probabile. Altrimenti, spreca molta larghezza di banda.

### Considerare lo streaming

[Lo streaming video consente di fornire la giusta dimensione e larghezza di banda del video](https://www.smashingmagazine.com/2018/10/video-playback-on-the-web-part-2/) (basata sulla velocità della rete) all'utente finale. Simile alle immagini responsive, è il video della giusta dimensione a essere fornito al browser, assicurando un avvio veloce del video, un buffering ridotto e una riproduzione ottimizzata.

## Conclusione

Ottimizzare i video può migliorare significativamente le prestazioni del sito web. I file video sono relativamente grandi rispetto ad altri file del sito web, e degni sempre di attenzione. Questo articolo spiega come ottimizzare i video del sito web riducendo la dimensione dei file, con impostazioni di download (HTML), e con lo streaming.

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/Multimedia", "Learn_web_development/Extensions/Performance/Javascript", "Learn_web_development/Extensions/Performance")}}
