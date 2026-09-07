---
title: "Contenuti multimediali: video"
slug: Learn_web_development/Extensions/Performance/video
l10n:
  sourceCommit: d64e1ee3cdbe602324fce3f7320d026f58186715
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/Multimedia", "Learn_web_development/Extensions/Performance/JavaScript", "Learn_web_development/Extensions/Performance")}}

Come appreso nella sezione precedente, i contenuti multimediali, ovvero immagini e video, rappresentano oltre il 70% dei byte scaricati per un sito web medio. Sono già state esaminate le tecniche per ottimizzare le immagini. Questo articolo tratta l'ottimizzazione dei video per migliorare le prestazioni web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        > e conoscenza di base delle
        <a href="/it/docs/Learn_web_development/Getting_started/Your_first_website"
          >tecnologie web lato client</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare a conoscere i vari formati video, il loro impatto sulle prestazioni
        e come ridurre l'impatto dei video sul tempo complessivo di caricamento della pagina,
        offrendo al contempo il file video più piccolo in base al supporto dei tipi di file di ciascun browser.
      </td>
    </tr>
  </tbody>
</table>

## Perché ottimizzare i contenuti multimediali?

Per un sito web medio, il [25% della larghezza di banda proviene dai video](https://discuss.httparchive.org/t/state-of-the-web-top-image-optimization-strategies/1367). L'ottimizzazione dei video può consentire notevoli risparmi di larghezza di banda, che si traducono in migliori prestazioni del sito web.

## Ottimizzazione della distribuzione dei video

Le sezioni seguenti descrivono le tecniche di ottimizzazione seguenti:

- [comprimere tutti i video](#comprimere_tutti_i_video)
- [ottimizzare l'ordine di `<source>`](#optimize_source_order)
- [rimuovere l'audio dai video disattivati](#rimuovere_l'audio_dai_video_hero_disattivati)
- [ottimizzare il preload dei video](#preload_dei_video)
- [considerare lo streaming](#considerare_lo_streaming)

### Comprimere tutti i video

La maggior parte del lavoro di compressione video confronta i fotogrammi adiacenti all'interno di un video, con l'obiettivo di rimuovere i dettagli identici in entrambi i fotogrammi. Comprimere il video ed esportarlo in più formati video, inclusi WebM e MPEG-4/H.264.

Il software di editing video probabilmente dispone di una funzionalità per ridurre le dimensioni del file. In caso contrario, sono disponibili strumenti online, come [FFmpeg](https://www.ffmpeg.org/) (trattato nella sezione seguente), che codificano, decodificano, convertono ed eseguono altre funzioni di ottimizzazione.

### Ottimizzare l'ordine di `<source>`

Ordinare le sorgenti video dalla più piccola alla più grande. Ad esempio, dati video compressi nei formati da 10 MB e 12 MB, dichiarare prima la risorsa da 10 MB:

```html
<video width="400" height="300" controls="controls">
  <!-- WebM: 10 MB -->
  <source src="video.webm" type="video/webm" />
  <!-- MPEG-4/H.264: 12 MB -->
  <source src="video.mp4" type="video/mp4" />
</video>
```

Il browser scarica il primo formato che comprende. L'obiettivo è offrire versioni più piccole prima di quelle più grandi. Con la versione più piccola, assicurarsi che il video più compresso abbia comunque un buon aspetto. Alcuni algoritmi di compressione possono far apparire il video (male) come una GIF animata. Sebbene un video da 128 Kb possa sembrare in grado di offrire un'esperienza utente migliore rispetto a un download da 10 MB, un video granuloso simile a una GIF può riflettersi negativamente sul brand o sul progetto.

### Rimuovere l'audio dai video hero disattivati

Per i video hero o altri video senza audio, rimuovere l'audio è una scelta intelligente.

```html
<video autoplay="" loop="" muted playsinline="" id="hero-video">
  <source src="banner_video.webm" type='video/webm; codecs="vp8, vorbis"' />
  <source src="web_banner.mp4" type="video/mp4" />
</video>
```

Questo codice per video hero (sopra) è comune nei siti web di conferenze e nelle home page aziendali. Include un video in riproduzione automatica, in loop e disattivato. Non sono presenti controlli, quindi non è possibile ascoltare l'audio. L'audio è spesso vuoto, ma è comunque presente e utilizza comunque larghezza di banda. Non c'è motivo di fornire audio con un video che è sempre disattivato. **La rimozione dell'audio può far risparmiare il 20% della larghezza di banda.**

A seconda del software scelto, potrebbe essere possibile rimuovere l'audio durante l'esportazione e la compressione. In caso contrario, un'utilità gratuita chiamata [FFmpeg](https://www.ffmpeg.org/) può farlo. Questa è la stringa di comando FFmpeg per rimuovere l'audio:

```bash
ffmpeg -i original.mp4 -an -c:v copy audioFreeVersion.mp4
```

### Preload dei video

L'attributo `preload` dispone di tre opzioni: `auto`, `metadata` e `none`. L'impostazione predefinita è `metadata`. Queste impostazioni controllano quanto di un file video viene scaricato durante il caricamento della pagina. È possibile risparmiare dati rimandando il download dei video meno popolari.

L'impostazione `preload="none"` comporta che nessuna parte del video venga scaricata fino all'avvio della riproduzione. Ritarda l'avvio, ma offre un notevole risparmio di dati per i video con una bassa probabilità di riproduzione.

Con un risparmio di larghezza di banda più modesto, l'impostazione `preload="metadata"` può scaricare fino al 3% del video durante il caricamento della pagina. È un'opzione utile per alcuni file piccoli o di dimensioni moderate.

Modificare l'impostazione in `auto` indica al browser di scaricare automaticamente l'intero video. Farlo solo quando la riproduzione è molto probabile. In caso contrario, si spreca molta larghezza di banda.

### Considerare lo streaming

Lo [streaming video consente di fornire all'utente finale le dimensioni video e la larghezza di banda appropriate](https://www.smashingmagazine.com/2018/10/video-playback-on-the-web-part-2/) (in base alla velocità della rete). Analogamente alle immagini responsive, al browser viene fornito il video delle dimensioni corrette, garantendo un rapido avvio del video, un buffering ridotto e una riproduzione ottimizzata.

## Conclusione

L'ottimizzazione dei video può migliorare significativamente le prestazioni del sito web. I file video sono relativamente grandi rispetto agli altri file di un sito web e meritano sempre attenzione. Questo articolo spiega come ottimizzare i video di un sito web riducendo le dimensioni dei file, mediante le impostazioni di download (HTML) e tramite lo streaming.

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/Multimedia", "Learn_web_development/Extensions/Performance/JavaScript", "Learn_web_development/Extensions/Performance")}}
