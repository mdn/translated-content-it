---
title: "Multimedia: video"
slug: Learn_web_development/Extensions/Performance/video
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/Multimedia", "Learn_web_development/Extensions/Performance/Javascript", "Learn_web_development/Extensions/Performance")}}

Come abbiamo appreso nella sezione precedente, i media, in particolare immagini e video, rappresentano oltre il 70% dei byte scaricati per il sito web medio. Abbiamo già esaminato l'ottimizzazione delle immagini. Questo articolo analizza l'ottimizzazione dei video per migliorare le prestazioni web.

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
        Imparare i vari formati video, il loro impatto sulle prestazioni,
        e come ridurre l'impatto del video sul tempo di caricamento complessivo della pagina servendo
        la dimensione del file video più piccola possibile in base al supporto del tipo di file di ciascun browser.
      </td>
    </tr>
  </tbody>
</table>

## Perché ottimizzare i tuoi contenuti multimediali?

Per il sito web medio, [il 25% della larghezza di banda proviene dai video](https://discuss.httparchive.org/t/state-of-the-web-top-image-optimization-strategies/1367). Ottimizzare il video offre potenziali risparmi significativi di larghezza di banda, che si traducono in migliori prestazioni del sito web.

## Ottimizzazione della consegna del video

Le sezioni seguenti descrivono le seguenti tecniche di ottimizzazione:

- [comprimere tutti i video](#comprimere_tutti_i_video)
- [ottimizzare l'ordine di `<source>`](#optimize_source_order)
- [rimuovere l'audio dai video muti](#rimuovere_l'audio_dai_video_muti)
- [ottimizzare il preload del video](#preload_del_video)
- [considerare lo streaming](#considerare_lo_streaming)

### Comprimere tutti i video

La maggior parte del lavoro di compressione video confronta i fotogrammi adiacenti all'interno di un video, con l'intento di rimuovere i dettagli che sono identici in entrambi i fotogrammi. Comprimi il video ed esportalo in più formati video, tra cui WebM e MPEG-4/H.264.

Il tuo software di editing video probabilmente ha una funzione per ridurre la dimensione del file. In caso contrario, ci sono strumenti online, come [FFmpeg](https://www.ffmpeg.org/) (discusso nella sezione successiva), che codificano, decodificano, convertono e svolgono altre funzioni di ottimizzazione.

### Ottimizzare l'ordine di `<source>`

Ordina le sorgenti video dalla più piccola alla più grande. Ad esempio, dati i video compressi nei formati di 10MB e 12MB, dichiara prima la risorsa da 10MB:

```html
<video width="400" height="300" controls="controls">
  <!-- WebM: 10 MB -->
  <source src="video.webm" type="video/webm" />
  <!-- MPEG-4/H.264: 12 MB -->
  <source src="video.mp4" type="video/mp4" />
</video>
```

Il browser scarica il primo formato che comprende. L'obiettivo è offrire versioni più piccole prima di quelle più grandi. Con la versione più piccola, assicurati che il video più compresso abbia comunque un aspetto buono. Alcuni algoritmi di compressione possono far apparire il video (male) come una GIF animata. Mentre un video da 128 Kb può sembrare che possa fornire un'esperienza utente migliore rispetto a un download da 10 MB, un video simile a una GIF granulosa potrebbe riflettere negativamente sul marchio o sul progetto.

### Rimuovere l'audio dai video muti

Per video "hero" o altri video senza audio, rimuovere l'audio è una scelta intelligente.

```html
<video autoplay="" loop="" muted playsinline="" id="hero-video">
  <source src="banner_video.webm" type='video/webm; codecs="vp8, vorbis"' />
  <source src="web_banner.mp4" type="video/mp4" />
</video>
```

Questo codice per video "hero" (sopra) è comune sui siti web di conferenze e sulle pagine iniziali aziendali. Include un video che viene riprodotto automaticamente, in loop e senza audio. Non ci sono controlli, quindi non c'è modo di sentire l'audio. L'audio è spesso vuoto, ma ancora presente e ancora utilizzando larghezza di banda. Non c'è motivo di servire audio con un video che è sempre muto. **La rimozione dell'audio può risparmiare il 20% della larghezza di banda.**

A seconda del software scelto, potresti essere in grado di rimuovere l'audio durante l'esportazione e la compressione. In caso contrario, un'utilità gratuita chiamata [FFmpeg](https://www.ffmpeg.org/) può farlo per te. Questa è la stringa di comando di FFmpeg per rimuovere l'audio:

```bash
ffmpeg -i original.mp4 -an -c:v copy audioFreeVersion.mp4
```

### Preload del video

L'attributo preload ha tre opzioni disponibili: `auto`, `metadata` e `none`. L'impostazione predefinita è `metadata`. Queste impostazioni controllano quanto di un file video viene scaricato con il caricamento della pagina. È possibile risparmiare dati differendo il download per video meno popolari.

Impostare `preload="none"` fa sì che nessun video venga scaricato fino alla riproduzione. Ritarda l'avvio, ma offre notevoli risparmi di dati per video con una bassa probabilità di riproduzione.

Offrendo risparmi di larghezza di banda più modesti, impostare `preload="metadata"` può scaricare fino al 3% del video durante il caricamento della pagina. Questa è un'opzione utile per alcuni file di piccole o medie dimensioni.

Cambiare l'impostazione su `auto` dice al browser di scaricare automaticamente l'intero video. Fai questo solo quando la riproduzione è molto probabile. Altrimenti, spreca molta larghezza di banda.

### Considerare lo streaming

[Lo streaming video consente la giusta dimensione del video e larghezza di banda](https://www.smashingmagazine.com/2018/10/video-playback-on-the-web-part-2/) (basato sulla velocità della rete) da consegnare all'utente finale. Simile alle immagini responsive, il video della dimensione corretta viene consegnato al browser, garantendo un avvio rapido del video, bassa latenza, e riproduzione ottimizzata.

## Conclusione

Ottimizzare i video ha il potenziale di migliorare significativamente le prestazioni del sito web. I file video sono relativamente grandi rispetto ad altri file del sito web, e meritano sempre attenzione. Questo articolo spiega come ottimizzare i video del sito web attraverso la riduzione delle dimensioni del file, con impostazioni di download (HTML), e con lo streaming.

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/Multimedia", "Learn_web_development/Extensions/Performance/Javascript", "Learn_web_development/Extensions/Performance")}}
