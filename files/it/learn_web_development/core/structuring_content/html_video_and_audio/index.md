---
title: HTML video e audio
short-title: Video e audio
slug: Learn_web_development/Core/Structuring_content/HTML_video_and_audio
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_images", "Learn_web_development/Core/Structuring_content/Mozilla_splash_page", "Learn_web_development/Core/Structuring_content")}}

Ora che siamo a nostro agio con l'aggiunta di immagini semplici a una pagina web, il passo successivo è iniziare ad aggiungere lettori video e audio ai Suoi documenti HTML! In questo articolo esamineremo proprio questo con gli elementi {{htmlelement("video")}} e {{htmlelement("audio")}}; termineremo poi esaminando come aggiungere didascalie/sottotitoli ai Suoi video.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come descritto in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >. Semantica a livello di testo come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >intestazioni e paragrafi</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >elenchi</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Sintassi base dei tag <code>&lt;video&gt;</code> e <code>&lt;audio&gt;</code></li>
          <li>Attributi specifici per video e audio come controls e muted.</li>
          <li>Uso di elementi <code>&lt;source&gt;</code> per fornire diverse sorgenti video o audio.</li>
          <li>Le basi dell'uso di tracce di testo come sottotitoli e didascalie.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Video e audio sul web

La prima ondata di video e audio online è stata resa possibile da tecnologie proprietarie basate su plugin come [Flash](https://en.wikipedia.org/wiki/Adobe_Flash) e [Silverlight](https://en.wikipedia.org/wiki/Microsoft_Silverlight). Entrambi hanno avuto problemi di sicurezza e accessibilità, e ora sono obsoleti, a favore delle soluzioni HTML native come gli elementi {{htmlelement("video")}} e {{htmlelement("audio")}} e la disponibilità di {{Glossary("JavaScript", "JavaScript")}} {{Glossary("API", "API")}} per controllarli. Qui non ci concentreremo su JavaScript — solo sulle basi che si possono raggiungere con l'HTML.

Non Le insegneremo come produrre file audio e video. Questo richiede un set di competenze completamente diverso. Le abbiamo fornito [file audio e video di esempio e codice di esempio](https://github.com/mdn/learning-area/tree/main/html/multimedia-and-embedding/video-and-audio-content) per il Suo esperimento, nel caso in cui non riesca a procurarsene di Suoi.

> [!NOTE]
> Prima di iniziare qui, dovrebbe sapere che ci sono parecchi fornitori di video online (OVP) come [YouTube](https://www.youtube.com/), [Dailymotion](https://www.dailymotion.com/) e [Vimeo](https://vimeo.com/), e fornitori di audio online come [Soundcloud](https://soundcloud.com/). Tali aziende offrono un modo conveniente e semplice per ospitare e consumare video, così non deve preoccuparsi del consumo di banda enorme. Gli OVP offrono spesso anche codice già pronto per l'incorporamento di video/audio nelle Sue pagine web; se utilizza quella via, può evitare alcune delle difficoltà di cui discutiamo in questo articolo. Discuteremo questo tipo di servizio un po' di più nel prossimo articolo.

## L'elemento \<video>

L'elemento {{htmlelement("video")}} Le consente di incorporare un video molto facilmente. Un esempio veramente semplice è questo:

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
  - : Allo stesso modo dell'elemento {{htmlelement("img")}}, l'attributo `src` (sorgente) contiene un percorso verso il video che desidera incorporare. Funziona esattamente allo stesso modo.
- [`controls`](/it/docs/Web/HTML/Reference/Elements/video#controls)
  - : Gli utenti devono poter controllare la riproduzione di video e audio (è particolarmente critico per le persone che hanno [epilessia](https://en.wikipedia.org/wiki/Epilepsy#Epidemiology).) Deve usare l'attributo `controls` per includere l'interfaccia di controllo del browser oppure costruire la Sua interfaccia usando l'[API JavaScript](/it/docs/Web/API/HTMLMediaElement) appropriata. Al minimo, l'interfaccia deve includere un modo per avviare e fermare il media, e per regolare il volume.
- Il paragrafo all'interno dei tag `<video>`
  - : Questo è chiamato **contenuto di fallback** — verrà visualizzato se il browser che accede alla pagina non supporta l'elemento `<video>`, permettendoci di fornire un fallback per i browser più vecchi. Può essere qualsiasi cosa voglia; in questo caso, abbiamo fornito un link diretto al file video, così l'utente può almeno accedervi in qualche modo indipendentemente dal browser che sta usando.

Il video incorporato sembrerà qualcosa del genere:

![Un semplice lettore video che mostra un video di un piccolo coniglio bianco](simple-video.png)

Può [provare l'esempio in diretta](https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/simple-video.html) qui (veda anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/video-and-audio-content/simple-video.html).)

## Utilizzo di formati di sorgente multipli per migliorare la compatibilità

C'è un problema con l'esempio sopra. È possibile che il video non si riproduca per Lei, poiché diversi browser supportano diversi formati di video (e audio). Fortunatamente, ci sono cose che può fare per aiutare a prevenire che questo diventi un problema.

### Contenuti di un file multimediale

Prima di tutto, esaminiamo velocemente la terminologia. I formati come OGG, WAV, MP4 e WebM sono chiamati **[formati contenitore](/it/docs/Web/Media/Guides/Formats/Containers)**. Definiscono una struttura in cui vengono memorizzate le tracce audio e/o video che compongono il media, insieme ai metadati che descrivono il media, quali codec sono stati utilizzati per codificare i suoi canali, e così via.

Un file WebM contenente un film che ha una traccia video principale e una traccia d'angolo alternativa, oltre ad audio per l'inglese e lo spagnolo, oltre a audio per una traccia di commento in inglese, può essere concettualizzato come mostrato nel diagramma sotto. Sono incluse anche tracce di testo che contengono sottotitoli chiusi per il film principale, sottotitoli spagnoli per il film e sottotitoli inglesi per il commento.

![Diagramma che concettualizza il contenuto di un file multimediale a livello di traccia.](containersandtracks.png)

Le tracce audio e video all'interno del contenitore contengono dati nel formato appropriato per il codec utilizzato per codificare quel media. Formati diversi vengono utilizzati per le tracce audio rispetto alle tracce video. Ogni traccia audio viene codificata utilizzando un [codec audio](/it/docs/Web/Media/Guides/Formats/Audio_codecs), mentre le tracce video vengono codificate utilizzando (come avrà probabilmente intuito) [un codec video](/it/docs/Web/Media/Guides/Formats/Video_codecs). Come accennato prima, diversi browser supportano diversi formati video e audio, e diversi formati contenitore (come OGG, MP4 e WebM, che a loro volta possono contenere diversi tipi di video e audio).

Ad esempio:

- Un contenitore WebM generalmente incorpora audio Vorbis o Opus con video VP8/VP9. Questo è supportato in tutti i browser moderni, anche se le versioni più vecchie possono non funzionare.
- Un contenitore MP4 spesso incorpora audio AAC o MP3 con video H.264. Questo è supportato in tutti i browser moderni.
- Il contenitore Ogg tende a utilizzare audio Vorbis e video Theora. Questo è supportato al meglio in Firefox e Chrome, ma è stato in pratica superato dal formato WebM di migliore qualità.

Esistono alcuni casi speciali. Ad esempio, per alcuni tipi di audio, i dati di un codec vengono spesso archiviati senza un contenitore, o con un contenitore semplificato. Uno di questi casi è il codec FLAC, che è archiviato più comunemente in file FLAC, che sono semplicemente tracce FLAC raw.

Un altro esempio è il sempre popolare "file MP3". Un "file MP3" è un file audio codificato utilizzando la compressione MPEG-1 Audio Layer III. Anche se può includere metadati, non è incapsulato all'interno di un contenitore MPEG o MPEG-2 separato. Il suo supporto diffuso negli elementi {{htmlelement("audio")}} e {{htmlelement("video")}} è largamente una testimonianza della sua popolarità duratura.

Un lettore audio tenderà a riprodurre una traccia audio direttamente, ad esempio, un file MP3 o Ogg. Non hanno bisogno di contenitori.

### Supporto dei file multimediali nei browser

> [!NOTE]
> Diversi formati popolari, come MP3 e MP4/H.264, sono eccellenti ma sono coperti da brevetti; cioè, ci sono brevetti che coprono parte o tutta la tecnologia su cui si basano. Negli Stati Uniti, i brevetti hanno coperto l'MP3 fino al 2017, e l'H.264 è coperto da brevetti almeno fino al 2027.
>
> A causa di quei brevetti, i browser che desiderano implementare il supporto per quei codec devono pagare tipicamente enormi commissioni per le licenze. Inoltre, alcune persone preferiscono evitare software con restrizioni e preferiscono utilizzare solo formati aperti. A causa di queste ragioni legali e preferenziali, gli sviluppatori web si trovano a dover supportare più formati per catturare l'intero pubblico.

I codec descritti nella sezione precedente esistono per comprimere video e audio in file gestibili, poiché audio e video raw sono entrambi estremamente grandi. Ogni browser web supporta un assortimento di **{{Glossary("Codec", "codec")}}**, come Vorbis o H.264, che vengono utilizzati per convertire l'audio e il video compressi in dati binari e viceversa. Ogni codec offre i suoi vantaggi e svantaggi, e ogni contenitore può anche offrire le sue caratteristiche positive e negative che influenzano le vostre decisioni su quale usare.

Le cose diventano leggermente più complicate perché non solo ogni browser supporta un insieme diverso di formati di file contenitore, ma supportano anche una selezione diversa di codec. Per massimizzare la probabilità che il Suo sito web o app funzionerà sul browser di un utente, potrebbe aver bisogno di fornire ogni file multimediale che usa in più formati. Se il Suo sito e il browser dell'utente non condividono un formato multimediale in comune, il Suo media non sarà riprodotto.

A causa delle complessità di garantire che i media dell'app siano visibili su ogni combinazione di browser, piattaforme e dispositivi che desidera raggiungere, scegliere la migliore combinazione di codec e contenitore può essere un compito complicato. Veda [Scegliere il contenitore giusto](/it/docs/Web/Media/Guides/Formats/Containers#choosing_the_right_container) per aiuto nella selezione del formato di file contenitore meglio adatto alle Sue esigenze; analogamente, veda [Scegliere un codec video](/it/docs/Web/Media/Guides/Formats/Video_codecs#choosing_a_video_codec) e [Scegliere un codec audio](/it/docs/Web/Media/Guides/Formats/Audio_codecs#choosing_an_audio_codec) per aiuto nella selezione dei primi codec multimediali da usare per i Suoi contenuti e il Suo pubblico di destinazione.

Un'altra cosa da tenere a mente: i browser mobili possono supportare formati aggiuntivi non supportati dalle loro controparti desktop, proprio come potrebbero non supportare tutti

 gli stessi formati della versione desktop. Inoltre, sia i browser desktop che quelli mobili _possono_ essere progettati per delegare la gestione della riproduzione dei media (sia per tutti i media sia solo per tipi specifici che non può gestire internamente). Ciò significa che il supporto dei media dipende in parte dal software che l'utente ha installato.

Quindi, come facciamo? Dia un'occhiata al seguente [esempio aggiornato](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/video-and-audio-content/multiple-video-formats.html) ([provare in diretta qui](https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/multiple-video-formats.html), anche):

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

Qui abbiamo tolto l'attributo `src` dal tag {{HTMLElement("video")}} effettivo, e invece abbiamo incluso elementi {{htmlelement("source")}} separati che puntano alle proprie sorgenti. In questo caso, il browser passerà attraverso gli elementi {{HTMLElement("source")}} e riprodurrà il primo che ha il codec per supportare. Includere sorgenti WebM e MP4 dovrebbe essere sufficiente a riprodurre il Suo video su la maggior parte delle piattaforme e browser al giorno d'oggi.

Ogni elemento `<source>` ha anche un attributo [`type`](/it/docs/Web/HTML/Reference/Elements/source#type). Questo è opzionale, ma si consiglia di includerlo. L'attributo `type` contiene il {{Glossary("MIME_type", "tipo MIME")}} del file specificato dal `<source>`, e i browser possono utilizzare il `type` per ignorare immediatamente i video che non comprendono. Se `type` non è incluso, i browser caricheranno e tenteranno di riprodurre ogni file fino a che non ne trovano uno che funziona, il che naturalmente richiede tempo ed è un uso inutile delle risorse.

Si riferisca alla nostra [guida ai tipi e formati multimediali](/it/docs/Web/Media/Guides/Formats) per aiuto nella selezione dei migliori contenitori e codec per le Sue esigenze, oltre che per ricercare i tipi MIME corretti da specificare per ciascuno.

## Altre caratteristiche di \<video>

Ci sono una serie di altre caratteristiche che può includere quando visualizza un video HTML. Dia un'occhiata al nostro prossimo esempio:

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

L'interfaccia risultante apparirà qualcosa del genere:

![Un lettore video mostra un'immagine del poster prima di essere riprodotto. L'immagine del poster dice esempio di video HTML, OMG che bello!](poster_screenshot_updated.png)

Le caratteristiche includono:

- [`width`](/it/docs/Web/HTML/Reference/Elements/video#width) e [`height`](/it/docs/Web/HTML/Reference/Elements/video#height)
  - : Può controllare la dimensione del video sia con questi attributi sia con {{Glossary("CSS", "CSS")}}. In entrambi i casi, i video mantengono il loro rapporto di larghezza-altezza nativo, noto come **rapporto d'aspetto**. Se il rapporto d'aspetto non è mantenuto dalle dimensioni che imposta, il video si espanderà per riempire lo spazio orizzontalmente, e lo spazio non riempito riceverà semplicemente un colore di sfondo solido di default.
- [`autoplay`](/it/docs/Web/HTML/Reference/Elements/video#autoplay)
  - : Fa sì che l'audio o il video inizino a riprodursi subito, mentre il resto della pagina si sta caricando. Si consiglia di non usare video (o audio) in autoplay sui Suoi siti, perché gli utenti possono trovarli davvero fastidiosi.
- [`loop`](/it/docs/Web/HTML/Reference/Elements/video#loop)
  - : Fa sì che il video (o l'audio) inizi a essere riprodotto di nuovo ogni volta che finisce. Questo può essere altrettanto fastidioso, quindi usi solo se veramente necessario.
- [`muted`](/it/docs/Web/HTML/Reference/Elements/video#muted)
  - : Fa sì che il media venga riprodotto con il suono disattivato di default.
- [`poster`](/it/docs/Web/HTML/Reference/Elements/video#poster)
  - : L'URL di un'immagine che verrà visualizzata prima che il video venga riprodotto. È inteso per essere utilizzato come schermata promozionale o di pubblicità.
- [`preload`](/it/docs/Web/HTML/Reference/Elements/video#preload)

  - : Utilizzato per il buffering di file di grandi dimensioni; può assumere uno dei tre valori:

    - `"none"` non bufferizza il file
    - `"auto"` bufferizza il file multimediale
    - `"metadata"` bufferizza solo i metadati del file

Può trovare l'esempio sopra disponibile per [la riproduzione dal vivo su GitHub](https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/extra-video-features.html) (veda anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/video-and-audio-content/extra-video-features.html).) Si noti che non abbiamo incluso l'attributo `autoplay` nella versione dal vivo — se il video inizia a riprodursi non appena la pagina viene caricata, non si vede il poster!

## L'elemento \<audio>

L'elemento {{htmlelement("audio")}} funziona proprio come l'elemento {{htmlelement("video")}}, con alcune piccole differenze come descritto di seguito. Un tipico esempio potrebbe essere simile a questo:

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

Questo produce in un browser qualcosa del genere:

![Un semplice lettore audio con un pulsante di riproduzione, timer, controllo del volume e barra di progresso](audio-player.png)

> [!NOTE]
> Può [eseguire il demo audio dal vivo](https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/multiple-audio-formats.html) su GitHub (veda anche il [codice sorgente del lettore audio](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/video-and-audio-content/multiple-audio-formats.html).)

Richiede meno spazio rispetto a un lettore video, poiché non c'è componente visiva — deve solo mostrare i controlli per riprodurre l'audio. Altre differenze rispetto all'HTML video sono le seguenti:

- L'elemento {{htmlelement("audio")}} non supporta gli attributi `width`/`height` — ancora una volta, non c'è componente visiva, quindi non c'è nulla a cui assegnare una larghezza o altezza.
- Non supporta nemmeno l'attributo `poster` — ancora una volta, nessuna componente visiva.

A parte questo, `<audio>` supporta tutte le stesse caratteristiche di `<video>` — riveda le sezioni sopra per ulteriori informazioni su di esse.

## Visualizzare le tracce di testo video

Ora discuteremo un concetto leggermente più avanzato che è davvero utile da conoscere. Molte persone non possono o non vogliono ascoltare il contenuto audio/video che trovano sul Web, almeno in certi momenti. Ad esempio:

- Molte persone hanno disabilità uditive (come essere ipoudenti o sorde) e quindi non riescono a sentire l'audio chiaramente se non del tutto.
- Altri potrebbero non riuscire a sentire l'audio perché si trovano in ambienti rumorosi (come un bar affollato quando viene mostrato un gioco sportivo).
- Allo stesso modo, in ambienti in cui avere l'audio riprodotto sarebbe una distrazione o una interruzione (come in una biblioteca o quando un partner sta cercando di dormire), avere delle didascalie può essere molto utile.
- Persone che non parlano la lingua del video potrebbero desiderare una trascrizione testuale o persino una traduzione per aiutarle a capire il contenuto multimediale.

Non sarebbe bello poter fornire a queste persone una trascrizione delle parole pronunciate nell'audio/video? Bene, grazie all'HTML video, si può fare. Per farlo utilizziamo il formato file [WebVTT](/it/docs/Web/API/WebVTT_API) e l'elemento {{htmlelement("track")}}.

> [!NOTE]
> "Trascrivere" significa "scrivere le parole pronunciate come testo." Il testo risultante è una "trascrizione."

WebVTT è un formato per scrivere file di testo contenenti più stringhe di testo insieme a metadati come il tempo nel video in cui ciascuna stringa di testo dovrebbe essere visualizzata, e persino limitate informazioni di stile/posizionamento. Queste stringhe di testo sono chiamate **cue**, e ci sono diversi tipi di cue che vengono utilizzati per scopi diversi. I cue più comuni sono:

- sottotitoli
  - : Traduzioni di materiale straniero, per persone che non comprendono le parole pronunciate nell'audio.
- didascalie
  - : Trascrizioni sincronizzate di dialoghi o descrizioni di suoni significativi, per consentire alle persone che non riescono a sentire l'audio di capire cosa sta succedendo.
- descrizioni temporizzate
  - : Testo che dovrebbe essere pronunciato dal lettore multimediale per descrivere importanti elementi visivi a utenti non vedenti o ipovedenti.

Un tipico file WebVTT avrà un aspetto simile a questo:

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

Per farlo visualizzare insieme alla riproduzione dei media HTML, è necessario:

1. Salvarlo come file `.vtt` in un punto in cui il server possa servire (vedi sotto), come nella stessa directory del file HTML.
2. Collegare il file `.vtt` con l'elemento {{htmlelement("track")}}. `<track>` dovrebbe essere posizionato all'interno di `<audio>` o `<video>`, ma dopo tutti gli elementi `<source>`. Usare l'attributo [`kind`](/it/docs/Web/HTML/Reference/Elements/track#kind) per specificare se i cue sono `subtitles`, `captions`, o `descriptions`. Inoltre, usare [`srclang`](/it/docs/Web/HTML/Reference/Elements/track#srclang) per dire al browser in quale lingua sono stati scritti i sottotitoli. Infine, aggiungere [`label`](/it/docs/Web/HTML/Reference/Elements/track#label) per aiutare i lettori a identificare la lingua che stanno cercando.

Ecco un esempio:

```html
<video controls>
  <source src="example.mp4" type="video/mp4" />
  <source src="example.webm" type="video/webm" />
  <track kind="subtitles" src="subtitles_es.vtt" srclang="es" label="Spanish" />
</video>
```

Per provare questo, è necessario ospitare i file su un [server HTTP locale](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server). Nell'output in browser, vedrà un video che ha sottotitoli visualizzati, un po' come questo:

![Lettore video con controlli standard come play, stop, volume, e sottotitoli on e off. Il video riprodotto mostra una scena di un uomo che tiene in mano un'arma simile a una lancia e una didascalia dice "Esta hoja tiene pasado oscuro."](video-player-with-captions.png)

Per ulteriori dettagli, incluso come aggiungere etichette La preghiamo di leggere [Aggiungere didascalie e sottotitoli ai video HTML](/it/docs/Web/Media/Guides/Audio_and_video_delivery/Adding_captions_and_subtitles_to_HTML5_video). Può [trovare l'esempio](https://iandevlin.github.io/mdn/video-player-with-captions/) che accompagna questo articolo su GitHub, scritto da Ian Devlin (Veda il [codice sorgente](https://github.com/iandevlin/iandevlin.github.io/tree/master/mdn/video-player-with-captions) anche.) Questo esempio utilizza un po' di JavaScript per permettere agli utenti di scegliere tra diversi sottotitoli. Si noti che per attivare i sottotitoli, è necessario premere il pulsante "CC" e selezionare un'opzione — Inglese, Deutsch, o Español.

> [!NOTE]
> Le tracce di testo aiutano anche con la {{Glossary("SEO", "SEO")}}, poiché i motori di ricerca prosperano particolarmente con il testo. Le tracce di testo consentono anche ai motori di ricerca di collegarsi direttamente a un punto a metà del video.

## Apprendimento attivo: Incorporare il Suo audio e video

Per questo apprendimento attivo, ci piacerebbe (idealmente) che Lei uscisse nel mondo e registrasse alcuni Suoi video e audio — la maggior parte dei telefoni al giorno d'oggi Le consente di registrare audio e video molto facilmente e, a condizione che possa trasferirli sul Suo computer, può usarli. Potrebbe dover fare alcune conversioni per ottenere un WebM e un MP4 nel caso del video, e un MP3 e un Ogg nel caso dell'audio, ma ci sono abbastanza programmi là fuori per permetterLe di fare questo senza troppa difficoltà, come [Miro Video Converter](http://www.mirovideoconverter.com/) e [Audacity](https://sourceforge.net/projects/audacity/). Ci piacerebbe che provasse!

Se non è in grado di trovare alcun video o audio, allora può sentirsi libero di usare i nostri [file audio e video di esempio](https://github.com/mdn/learning-area/tree/main/html/multimedia-and-embedding/video-and-audio-content) per svolgere questo esercizio. Può anche usare il nostro codice di esempio per riferimento.

Vorremmo che:

1. Salvasse i Suoi file audio e video in una nuova directory sul Suo computer.
2. Creasse un nuovo file HTML nella stessa directory, chiamato `index.html`.
3. Aggiungesse elementi {{HTMLElement("audio")}} e {{HTMLElement("video")}} alla pagina; faccia in modo che mostrino i controlli del browser predefiniti.
4. Desse a entrambi elementi {{HTMLElement("source")}} in modo che i browser troveranno il formato audio che supportano meglio e lo caricheranno. Questi dovrebbero includere attributi [`type`](/it/docs/Web/HTML/Reference/Elements/source#type).
5. Desse all'elemento `<video>` un poster che verrà visualizzato prima che il video inizi a essere riprodotto. Si diverta a creare la Sua grafica del poster.

Per un bonus aggiunto, potrebbe provare a ricercare le tracce di testo e capire come aggiungere sottotitoli al Suo video.

## Testi le Sue abilità!

Ha raggiunto la fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare ulteriori test per verificare che abbia trattenuto queste informazioni prima di procedere — veda [Testi le Sue abilità: Multimedia e incorporamento](/it/docs/Learn_web_development/Core/Structuring_content/Test_your_skills/Multimedia_and_embedding).

## Sommario

E questo è tutto — speriamo si sia divertito a giocare con video e audio nelle pagine web! Il prossimo passo, Le presenteremo una sfida per testare le Sue abilità con i media HTML.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_images", "Learn_web_development/Core/Structuring_content/Mozilla_splash_page", "Learn_web_development/Core/Structuring_content")}}
