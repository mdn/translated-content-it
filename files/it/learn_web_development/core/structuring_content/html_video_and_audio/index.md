---
title: Video e audio HTML
short-title: Video e audio
slug: Learn_web_development/Core/Structuring_content/HTML_video_and_audio
l10n:
  sourceCommit: daad50a992d56b23573fdd50517c75df176747cf
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Test_your_skills/Images", "Learn_web_development/Core/Structuring_content/Test_your_skills/Audio_and_video", "Learn_web_development/Core/Structuring_content")}}

Ora che è chiaro come aggiungere immagini semplici a una pagina web, il passaggio successivo consiste nell'iniziare ad aggiungere lettori video e audio ai documenti HTML. In questo articolo verrà illustrato come farlo con gli elementi {{htmlelement("video")}} e {{htmlelement("audio")}}; infine, verrà mostrato come aggiungere didascalie/sottotitoli ai video.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenze di base di HTML, come trattato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >. Semantica a livello di testo, come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >titoli e paragrafi</a
        > ed <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >elenchi</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Sintassi di base dei tag <code>&lt;video&gt;</code> e <code>&lt;audio&gt;</code></li>
          <li>Attributi specifici per video e audio, come controls e muted.</li>
          <li>Uso degli elementi <code>&lt;source&gt;</code> per fornire diverse sorgenti video o audio.</li>
          <li>Fondamenti dell'uso delle tracce di testo, come didascalie e sottotitoli.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Video e audio sul Web

Il primo afflusso di video e audio online è stato reso possibile da tecnologie proprietarie basate su plugin come [Flash](https://en.wikipedia.org/wiki/Adobe_Flash) e [Silverlight](https://en.wikipedia.org/wiki/Microsoft_Silverlight). Entrambe presentavano problemi di sicurezza e accessibilità e ora sono obsolete, a favore delle soluzioni HTML native, gli elementi {{htmlelement("video")}} e {{htmlelement("audio")}}, e della disponibilità di {{Glossary("API", "API")}} {{Glossary("JavaScript", "JavaScript")}} per controllarli. Qui non verrà trattato JavaScript, ma solo le basi fondamentali realizzabili con HTML.

Non verrà insegnato come produrre file audio e video: ciò richiede un insieme di competenze completamente diverso. Sono stati forniti [file audio e video di esempio e codice di esempio](https://github.com/mdn/learning-area/tree/main/html/multimedia-and-embedding/video-and-audio-content) per sperimentare, nel caso non sia possibile procurarsi file propri.

> [!NOTE]
> Prima di iniziare, è utile sapere che esistono diversi OVP (online video provider), come [YouTube](https://www.youtube.com/), [Dailymotion](https://www.dailymotion.com/) e [Vimeo](https://vimeo.com/), oltre a provider audio online come [Soundcloud](https://soundcloud.com/). Queste aziende offrono un modo pratico e semplice per ospitare e fruire video, senza doversi preoccupare dell'enorme consumo di larghezza di banda. Gli OVP di solito offrono anche codice già pronto per incorporare video/audio nelle pagine web; utilizzando questa strada, è possibile evitare alcune delle difficoltà discusse in questo articolo. Questo tipo di servizio verrà trattato più dettagliatamente nel prossimo articolo.

## L'elemento `<video>`

L'elemento {{htmlelement("video")}} consente di incorporare un video molto facilmente. Un esempio davvero semplice è il seguente:

```html
<video src="rabbit320.webm" controls>
  <p>
    Your browser doesn't support HTML video. Here is a
    <a href="rabbit320.webm">link to the video</a> instead.
  </p>
</video>
```

Le caratteristiche da notare sono:

- [`src`](/it/docs/Web/HTML/Reference/Elements/video#src)
  - : Come per l'elemento {{htmlelement("img")}}, l'attributo `src` (source) contiene un percorso al video da incorporare. Funziona esattamente nello stesso modo.
- [`controls`](/it/docs/Web/HTML/Reference/Elements/video#controls)
  - : Gli utenti devono poter controllare la riproduzione di video e audio, aspetto particolarmente importante per le persone con [epilessia](https://en.wikipedia.org/wiki/Epilepsy#Epidemiology). È necessario usare l'attributo `controls` per includere l'interfaccia di controllo del browser oppure creare un'interfaccia usando l'[API JavaScript](/it/docs/Web/API/HTMLMediaElement) appropriata. Come minimo, l'interfaccia deve includere un modo per avviare e interrompere il media e per regolare il volume.
- Il paragrafo all'interno dei tag `<video>`
  - : Questo è chiamato **contenuto di fallback** e verrà visualizzato se il browser che accede alla pagina non supporta l'elemento `<video>`, consentendo di fornire un'alternativa per i browser meno recenti. Può contenere qualsiasi contenuto; in questo caso è stato fornito un collegamento diretto al file video, così l'utente può almeno accedervi in qualche modo indipendentemente dal browser utilizzato.

Il video incorporato avrà un aspetto simile al seguente:

![Un semplice lettore video che mostra un video di un piccolo coniglio bianco](simple-video.png)

È possibile [provare l'esempio dal vivo](https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/simple-video.html) qui (vedere anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/video-and-audio-content/simple-video.html).)

## Uso di più formati sorgente per migliorare la compatibilità

L'esempio precedente presenta un problema. È possibile che il video non venga riprodotto, perché browser diversi supportano formati video (e audio) diversi. Fortunatamente, è possibile fare qualcosa per evitare che questo diventi un problema.

### Contenuto di un file multimediale

Per prima cosa, vediamo rapidamente la terminologia. Formati come OGG, WAV, MP4 e WebM sono chiamati **[formati contenitore](/it/docs/Web/Media/Guides/Formats/Containers)**. Definiscono una struttura in cui vengono archiviate le tracce audio e/o video che compongono il media, insieme a metadati che descrivono il media, quali codec vengono utilizzati per codificare i relativi canali e così via.

Un file WebM contenente un film con una traccia video principale e una traccia con un'angolazione alternativa, oltre ad audio in inglese e spagnolo e all'audio di una traccia di commento in inglese, può essere concettualizzato come mostrato nel diagramma seguente. Sono incluse anche tracce di testo contenenti sottotitoli per non udenti per il film, sottotitoli in spagnolo per il film e didascalie in inglese per il commento.

![Diagramma che concettualizza il contenuto di un file multimediale a livello di tracce.](containersandtracks.png)

Le tracce audio e video all'interno del contenitore conservano i dati nel formato appropriato al codec utilizzato per codificare quel media. Per le tracce audio e video vengono utilizzati formati diversi. Ogni traccia audio viene codificata mediante un [codec audio](/it/docs/Web/Media/Guides/Formats/Audio_codecs), mentre le tracce video vengono codificate usando, come probabilmente già intuito, [un codec video](/it/docs/Web/Media/Guides/Formats/Video_codecs). Come illustrato in precedenza, browser diversi supportano formati video e audio diversi e formati contenitore diversi, come OGG, MP4 e WebM, che a loro volta possono contenere tipi diversi di video e audio.

Ad esempio:

- Un contenitore WebM in genere combina audio Vorbis o Opus con video VP8/VP9. È supportato da tutti i browser moderni, anche se le versioni meno recenti potrebbero non funzionare.
- Un contenitore MP4 spesso combina audio AAC o MP3 con video H.264. Anche questo è supportato da tutti i browser moderni.
- Il contenitore Ogg tende a usare audio Vorbis e video Theora. È supportato al meglio da Firefox e Chrome, ma è stato sostanzialmente sostituito dal formato WebM, di qualità migliore.

Esistono alcuni casi speciali. Ad esempio, per alcuni tipi di audio, i dati di un codec vengono spesso archiviati senza un contenitore o con un contenitore semplificato. Un caso di questo tipo è il codec FLAC, che viene archiviato più comunemente nei file FLAC, ovvero semplici tracce FLAC non elaborate.

Un altro esempio è il sempre popolare "file MP3". Un "file MP3" è un file audio codificato utilizzando la compressione MPEG-1 Audio Layer III. Sebbene possa includere metadati, non è incapsulato all'interno di un contenitore MPEG o MPEG-2 separato. Il suo ampio supporto negli elementi {{htmlelement("audio")}} e {{htmlelement("video")}} è in gran parte una testimonianza della sua duratura popolarità.

Un lettore audio tende a riprodurre direttamente una traccia audio, ad esempio un file MP3 o Ogg. Questi non necessitano di contenitori.

### Supporto dei file multimediali nei browser

> [!NOTE]
> Diversi formati popolari, come MP3 e MP4/H.264, sono eccellenti ma sono gravati da brevetti; ovvero, esistono brevetti che coprono una parte o tutta la tecnologia su cui si basano. Negli Stati Uniti, i brevetti hanno coperto MP3 fino al 2017 e H.264 è gravato da brevetti almeno fino al 2027.
>
> A causa di tali brevetti, i browser che desiderano implementare il supporto per questi codec devono generalmente pagare enormi costi di licenza. Inoltre, alcune persone preferiscono evitare software con restrizioni e usare solo formati aperti. Per queste ragioni legali e di preferenza, gli sviluppatori web si trovano a dover supportare più formati per offrire un'esperienza video a tutto il pubblico.

I codec descritti nella sezione precedente esistono per comprimere video e audio in file gestibili, poiché audio e video non elaborati sono entrambi estremamente grandi. Ogni browser web supporta un insieme di **{{Glossary("Codec", "codec")}}**, come Vorbis o H.264, utilizzati per convertire audio e video compressi in dati binari e viceversa. Ogni codec offre vantaggi e svantaggi propri e ogni contenitore può offrire caratteristiche positive e negative che influenzano le decisioni su quale utilizzare.

La situazione diventa leggermente più complicata perché ogni browser non solo supporta un insieme diverso di formati di file contenitore, ma supporta anche una diversa selezione di codec. Per massimizzare la probabilità che il sito web o l'applicazione funzioni nel browser di un utente, potrebbe essere necessario fornire ogni file multimediale utilizzato in più formati. Se il sito e il browser dell'utente non hanno un formato multimediale in comune, il media non verrà riprodotto.

A causa delle complessità necessarie per assicurarsi che i media dell'applicazione siano visualizzabili su ogni combinazione di browser, piattaforme e dispositivi che si desidera raggiungere, scegliere la migliore combinazione di codec e contenitore può essere un compito complicato. Consultare [Scelta del contenitore corretto](/it/docs/Web/Media/Guides/Formats/Containers#choosing_the_right_container) per assistenza nella selezione del formato di file contenitore più adatto alle proprie necessità; analogamente, consultare [Scelta di un codec video](/it/docs/Web/Media/Guides/Formats/Video_codecs#choosing_a_video_codec) e [Scelta di un codec audio](/it/docs/Web/Media/Guides/Formats/Audio_codecs#choosing_an_audio_codec) per assistenza nella selezione dei primi codec multimediali da usare per i contenuti e il pubblico di destinazione.

Un ulteriore aspetto da tenere presente: i browser mobili possono supportare formati aggiuntivi non supportati dalle rispettive versioni desktop, così come potrebbero non supportare tutti gli stessi formati della versione desktop. Inoltre, sia i browser desktop sia quelli mobili _potrebbero_ essere progettati per delegare la gestione della riproduzione dei media, per tutti i media o solo per tipi specifici che non possono gestire internamente. Questo significa che il supporto dei media dipende in parte dal software installato dall'utente.

Come si fa, quindi? Osservare il seguente [esempio aggiornato](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/video-and-audio-content/multiple-video-formats.html) ([provarlo dal vivo qui](https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/multiple-video-formats.html)):

```html
<video controls>
  <source src="rabbit320.mp4" type="video/mp4" />
  <source src="rabbit320.webm" type="video/webm" />
  <p>
    Your browser doesn't support this video. Here is a
    <a href="rabbit320.mp4">link to the video</a> instead.
  </p>
</video>
```

In questo caso, l'attributo `src` è stato rimosso dal tag {{HTMLElement("video")}} vero e proprio e sono stati invece inclusi elementi {{htmlelement("source")}} separati che puntano alle rispettive sorgenti. In questo caso, il browser passerà in rassegna gli elementi {{HTMLElement("source")}} e riprodurrà il primo per cui dispone del codec supportato. L'inclusione di sorgenti WebM e MP4 dovrebbe essere sufficiente per riprodurre il video sulla maggior parte delle piattaforme e dei browser attuali.

Ogni elemento `<source>` ha anche un attributo [`type`](/it/docs/Web/HTML/Reference/Elements/source#type). Questo è facoltativo, ma è consigliabile includerlo. L'attributo `type` contiene il {{Glossary("MIME_type", "tipo MIME")}} del file specificato da `<source>` e i browser possono usare `type` per saltare immediatamente i video che non comprendono. Se `type` non viene incluso, i browser caricheranno e proveranno a riprodurre ogni file finché non ne troveranno uno che funziona, operazione che ovviamente richiede tempo e costituisce un uso non necessario di risorse.

Consultare la nostra [guida ai tipi e formati multimediali](/it/docs/Web/Media/Guides/Formats) per assistenza nella selezione dei contenitori e dei codec migliori per le proprie necessità, nonché per individuare i tipi MIME corretti da specificare per ciascuno.

## Altre funzionalità di `<video>`

Esistono diverse altre funzionalità che è possibile includere quando si visualizza un video HTML. Osservare il prossimo esempio:

```html
<video
  controls
  width="400"
  height="400"
  autoplay
  loop
  muted
  preload="auto"
  poster="poster.png">
  <source src="rabbit320.mp4" type="video/mp4" />
  <source src="rabbit320.webm" type="video/webm" />
  <p>
    Your browser doesn't support this video. Here is a
    <a href="rabbit320.mp4">link to the video</a> instead.
  </p>
</video>
```

L'interfaccia utente risultante ha un aspetto simile al seguente:

![Un lettore video che mostra un'immagine poster prima della riproduzione. L'immagine poster riporta HTML video example, OMG hell yeah!](poster_screenshot_updated.png)

Le funzionalità includono:

- [`width`](/it/docs/Web/HTML/Reference/Elements/video#width) e [`height`](/it/docs/Web/HTML/Reference/Elements/video#height)
  - : È possibile controllare le dimensioni del video mediante questi attributi oppure con {{Glossary("CSS", "CSS")}}. In entrambi i casi, i video mantengono il rapporto nativo tra larghezza e altezza, noto come **aspect ratio**. Se l'aspect ratio non viene mantenuto dalle dimensioni impostate, il video si espanderà per riempire lo spazio orizzontalmente e allo spazio non riempito verrà assegnato per impostazione predefinita un colore di sfondo uniforme.
- [`autoplay`](/it/docs/Web/HTML/Reference/Elements/video#autoplay)
  - : Fa sì che l'audio o il video inizi a essere riprodotto immediatamente durante il caricamento del resto della pagina. Si consiglia di non usare video o audio con riproduzione automatica nei siti, perché gli utenti potrebbero trovarli davvero fastidiosi.
- [`loop`](/it/docs/Web/HTML/Reference/Elements/video#loop)
  - : Fa sì che il video, o l'audio, ricominci a essere riprodotto ogni volta che termina. Anche questo può essere fastidioso, quindi va usato solo se realmente necessario.
- [`muted`](/it/docs/Web/HTML/Reference/Elements/video#muted)
  - : Fa sì che il media venga riprodotto con l'audio disattivato per impostazione predefinita.
- [`poster`](/it/docs/Web/HTML/Reference/Elements/video#poster)
  - : L'URL di un'immagine che verrà visualizzata prima della riproduzione del video. È pensata per essere utilizzata come schermata iniziale o pubblicitaria.
- [`preload`](/it/docs/Web/HTML/Reference/Elements/video#preload)
  - : Utilizzato per il buffering di file di grandi dimensioni; può assumere uno di tre valori:
    - `"none"` non esegue il buffering del file
    - `"auto"` esegue il buffering del file multimediale
    - `"metadata"` esegue il buffering solo dei metadati del file

L'esempio precedente è disponibile per essere [provato dal vivo su GitHub](https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/extra-video-features.html) (vedere anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/video-and-audio-content/extra-video-features.html).) Si noti che nella versione dal vivo non è stato incluso l'attributo `autoplay`: se il video inizia a essere riprodotto non appena la pagina viene caricata, non è possibile vedere il poster.

## L'elemento `<audio>`

L'elemento {{htmlelement("audio")}} funziona esattamente come l'elemento {{htmlelement("video")}}, con alcune piccole differenze illustrate di seguito. Un esempio tipico potrebbe essere il seguente:

```html
<audio controls>
  <source src="viper.mp3" type="audio/mp3" />
  <source src="viper.ogg" type="audio/ogg" />
  <p>
    Your browser doesn't support this audio file. Here is a
    <a href="viper.mp3">link to the audio</a> instead.
  </p>
</audio>
```

In un browser viene prodotto qualcosa di simile a quanto segue:

![Un semplice lettore audio con pulsante di riproduzione, timer, controllo del volume e barra di avanzamento](audio-player.png)

> [!NOTE]
> È possibile [eseguire la demo audio dal vivo](https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/multiple-audio-formats.html) su GitHub (vedere anche il [codice sorgente del lettore audio](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/video-and-audio-content/multiple-audio-formats.html).)

Questo occupa meno spazio di un lettore video, poiché non esiste un componente visivo: è sufficiente visualizzare i controlli per riprodurre l'audio. Le altre differenze rispetto al video HTML sono le seguenti:

- L'elemento {{htmlelement("audio")}} non supporta gli attributi `width`/`height`: non esiste un componente visivo, quindi non c'è nulla a cui assegnare larghezza o altezza.
- Non supporta nemmeno l'attributo `poster`: anche in questo caso, non esiste un componente visivo.

A parte questo, `<audio>` supporta tutte le stesse funzionalità di `<video>`; rivedere le sezioni precedenti per ulteriori informazioni.

## Visualizzazione delle tracce di testo del video

Ora verrà discusso un concetto leggermente più avanzato che è davvero utile conoscere. Molte persone non possono o non desiderano ascoltare i contenuti audio/video presenti sul Web, almeno in determinate situazioni. Ad esempio:

- Molte persone hanno disabilità uditive, come ipoacusia o sordità, e quindi non riescono a sentire l'audio chiaramente, se non del tutto.
- Altre persone potrebbero non riuscire a sentire l'audio perché si trovano in ambienti rumorosi, come un bar affollato durante la trasmissione di una partita sportiva.
- Analogamente, in ambienti in cui la riproduzione dell'audio costituirebbe una distrazione o un disturbo, come in una biblioteca o quando il partner sta cercando di dormire, avere didascalie può essere molto utile.
- Le persone che non parlano la lingua del video potrebbero desiderare una trascrizione testuale o persino una traduzione per aiutarle a comprendere il contenuto multimediale.

Non sarebbe utile poter fornire a queste persone una trascrizione delle parole pronunciate nell'audio/video? Grazie ai video HTML, è possibile farlo. A tale scopo si utilizzano il formato di file [WebVTT](/it/docs/Web/API/WebVTT_API) e l'elemento {{htmlelement("track")}}.

> [!NOTE]
> "Trascrivere" significa "scrivere le parole pronunciate come testo". Il testo risultante è una "trascrizione".

WebVTT è un formato per la scrittura di file di testo contenenti più stringhe di testo insieme a metadati, come il momento del video in cui ogni stringa di testo deve essere visualizzata, e persino informazioni limitate sullo stile/posizionamento. Queste stringhe di testo sono chiamate **cue** ed esistono diversi tipi di cue utilizzati per scopi differenti. I cue più comuni sono:

- subtitles
  - : Traduzioni di materiale in lingua straniera, per persone che non comprendono le parole pronunciate nell'audio.
- captions
  - : Trascrizioni sincronizzate di dialoghi o descrizioni di suoni significativi, per consentire alle persone che non possono sentire l'audio di comprendere ciò che accade.
- timed descriptions
  - : Testo che dovrebbe essere pronunciato dal lettore multimediale per descrivere elementi visivi importanti a utenti ciechi o con altre disabilità visive.

Un tipico file WebVTT avrà un aspetto simile al seguente:

```plain
WEBVTT

1
00:00:22.230 --> 00:00:24.606
This is the first subtitle.

2
00:00:30.739 --> 00:00:34.074
This is the second.

…
```

Per visualizzarlo insieme alla riproduzione del media HTML, è necessario:

1. Salvarlo come file `.vtt` in un punto che il server possa fornire, come nella stessa directory del file HTML.
2. Collegare il file `.vtt` con l'elemento {{htmlelement("track")}}. `<track>` deve essere collocato all'interno di `<audio>` o `<video>`, ma dopo tutti gli elementi `<source>`. Usare l'attributo [`kind`](/it/docs/Web/HTML/Reference/Elements/track#kind) per specificare se i cue sono `subtitles`, `captions` o `descriptions`. Inoltre, usare [`srclang`](/it/docs/Web/HTML/Reference/Elements/track#srclang) per indicare al browser in quale lingua sono stati scritti i sottotitoli. Infine, aggiungere [`label`](/it/docs/Web/HTML/Reference/Elements/track#label) per aiutare i lettori a identificare la lingua cercata.

Ecco un esempio:

```html
<video controls>
  <source src="example.mp4" type="video/mp4" />
  <source src="example.webm" type="video/webm" />
  <track kind="subtitles" src="subtitles_es.vtt" srclang="es" label="Spanish" />
</video>
```

Per provare questo esempio è necessario ospitare i file su un [server HTTP locale](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server). Nell'output nel browser verrà visualizzato un video con i sottotitoli. Per un'applicazione completa e il relativo codice sorgente, vedere [Aggiunta di didascalie e sottotitoli a video HTML](/it/docs/Web/Media/Guides/Audio_and_video_delivery/Adding_captions_and_subtitles_to_HTML5_video). Questo esempio utilizza JavaScript per consentire agli utenti di scegliere tra diversi sottotitoli. Si noti che, per attivare i sottotitoli, è necessario premere il pulsante "CC" e selezionare un'opzione: English, Deutsch o Español.

> [!NOTE]
> Le tracce di testo aiutano anche con la {{Glossary("SEO", "SEO")}}, poiché i motori di ricerca traggono particolare vantaggio dal testo. Le tracce di testo consentono persino ai motori di ricerca di creare collegamenti diretti a un punto intermedio del video.

## Incorporare audio e video propri

Per questa attività, perché non uscire nel mondo e registrare alcuni video e audio? Se si dispone di un telefono, è possibile usarlo per registrare audio e video, trasferirli al computer e provarli. Potrebbe essere necessario effettuare alcune conversioni per ottenere un WebM e un MP4 nel caso del video e un MP3 e Ogg nel caso dell'audio, ma esistono abbastanza programmi e strumenti per farlo senza troppe difficoltà, come [CloudConvert](https://cloudconvert.com/mp4-converter) (online) e [Audacity](https://sourceforge.net/projects/audacity/) (applicazione desktop). Vale la pena provarci.

> [!NOTE]
> Se non è possibile reperire video o audio, è possibile usare i nostri [file audio e video di esempio](https://github.com/mdn/learning-area/tree/main/html/multimedia-and-embedding/video-and-audio-content) per svolgere questo esercizio.

Occorre:

1. Salvare i file audio e video in una nuova directory sul computer.
2. Creare un nuovo file HTML nella stessa directory, denominato `index.html`, basato sul nostro [modello introduttivo](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html).
3. Aggiungere alla pagina elementi {{HTMLElement("audio")}} e {{HTMLElement("video")}}; devono visualizzare i controlli predefiniti del browser.
4. Assegnare a entrambi elementi {{HTMLElement("source")}} affinché i browser trovino e carichino il formato audio che supportano meglio. Questi devono includere attributi [`type`](/it/docs/Web/HTML/Reference/Elements/source#type).
5. Assegnare a entrambi un elemento `<p>` di fallback all'interno dei tag, che fornisca un collegamento diretto al media per i browser che non lo supportano.
6. Assegnare all'elemento `<video>` un poster da visualizzare prima dell'avvio della riproduzione del video. È possibile divertirsi creando una grafica poster personale.

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il codice HTML completato dovrebbe apparire simile al seguente:

```html
<video controls poster="poster.png">
  <source src="rabbit320.mp4" type="video/mp4" />
  <source src="rabbit320.webm" type="video/webm" />
  <p>
    Your browser doesn't support HTML video. Here is a
    <a href="rabbit320.mp4">link to the video</a> instead.
  </p>
</video>

<audio controls>
  <source src="viper.mp3" type="audio/mp3" />
  <source src="viper.ogg" type="audio/ogg" />
  <p>
    Your browser doesn't support HTML audio. Here is a
    <a href="viper.mp3">link to the audio</a> instead.
  </p>
</audio>
```

</details>

## Riepilogo

E questo è tutto: si spera che sperimentare con video e audio nelle pagine web sia stato divertente. Successivamente, verranno proposti alcuni test per verificare quanto bene sono state comprese e memorizzate le informazioni fornite sui video e gli audio HTML.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Test_your_skills/Images", "Learn_web_development/Core/Structuring_content/Test_your_skills/Audio_and_video", "Learn_web_development/Core/Structuring_content")}}
