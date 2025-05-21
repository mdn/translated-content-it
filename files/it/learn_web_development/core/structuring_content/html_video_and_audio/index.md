---
title: HTML video e audio
short-title: Video e audio
slug: Learn_web_development/Core/Structuring_content/HTML_video_and_audio
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_images", "Learn_web_development/Core/Structuring_content/Mozilla_splash_page", "Learn_web_development/Core/Structuring_content")}}

Ora che ci siamo abituati ad aggiungere immagini semplici a una pagina web, il passo successivo è iniziare ad aggiungere lettori video e audio ai documenti HTML! In questo articolo vedremo come fare proprio questo con gli elementi {{htmlelement("video")}} e {{htmlelement("audio")}}; poi, concluderemo esaminando come aggiungere didascalie/sottotitoli ai tuoi video.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come trattato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >. Semantica a livello di testo come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >titoli e paragrafi</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >elenchi</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Sintassi di base del tag <code>&lt;video&gt;</code> e <code>&lt;audio&gt;</code></li>
          <li>Attributi specifici per video e audio come controls e muted.</li>
          <li>Utilizzo degli elementi <code>&lt;source&gt;</code> per fornire diverse fonti video o audio.</li>
          <li>Nozioni di base sull'uso delle tracce di testo come didascalie e sottotitoli.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Video e audio sul web

I primi video e audio online sono stati resi possibili da tecnologie proprietarie basate su plug-in come [Flash](https://en.wikipedia.org/wiki/Adobe_Flash) e [Silverlight](https://en.wikipedia.org/wiki/Microsoft_Silverlight). Entrambi presentavano problemi di sicurezza e accessibilità, e ora sono obsoleti, a favore delle soluzioni native HTML tramite gli elementi {{htmlelement("video")}} e {{htmlelement("audio")}} e della disponibilità di {{Glossary("API", "APIs JavaScript")}} per controllarli. Qui non ci occuperemo di JavaScript — solo delle basi che possono essere realizzate con HTML.

Non ti insegneremo come produrre file audio e video — ciò richiede un set di competenze completamente diverso. Ti abbiamo fornito dei [file audio e video di esempio e codice d'esempio](https://github.com/mdn/learning-area/tree/main/html/multimedia-and-embedding/video-and-audio-content) per consentire la tua sperimentazione, nel caso non riuscissi a procurarli autonomamente.

> [!NOTE]
> Prima di iniziare qui, dovresti anche sapere che ci sono parecchi fornitori di video online (OVP) come [YouTube](https://www.youtube.com/), [Dailymotion](https://www.dailymotion.com/) e [Vimeo](https://vimeo.com/), e fornitori di audio online come [Soundcloud](https://soundcloud.com/). Tali aziende offrono un modo conveniente e semplice per ospitare e consumare video, così non devi preoccuparti del consumo di banda enorme. Gli OVP spesso offrono anche codice pronto per incorporare video/audio nelle tue pagine web; se usi quella strada, puoi evitare alcune delle difficoltà di cui discutiamo in questo articolo. Parleremo di questo tipo di servizio un po' di più nel prossimo articolo.

## L'elemento \<video>

L'elemento {{htmlelement("video")}} ti permette di incorporare un video molto facilmente. Un esempio davvero semplice appare così:

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
  - : Allo stesso modo dell'elemento {{htmlelement("img")}}, l'attributo `src` (source) contiene un percorso al video che vuoi incorporare. Funziona esattamente allo stesso modo.
- [`controls`](/it/docs/Web/HTML/Reference/Elements/video#controls)
  - : Gli utenti devono essere in grado di controllare la riproduzione video e audio (è particolarmente critico per le persone che hanno [epilessia](https://en.wikipedia.org/wiki/Epilepsy#Epidemiology)). Devi utilizzare l'attributo `controls` per includere l'interfaccia di controllo del browser, oppure costruire la tua interfaccia utilizzando l'[API JavaScript](/it/docs/Web/API/HTMLMediaElement) appropriata. Come minimo, l'interfaccia deve includere un modo per avviare e interrompere il media, e per regolare il volume.
- Il paragrafo all'interno dei tag `<video>`
  - : Questo è chiamato **contenuto di fallback** — verrà visualizzato se il browser che accede alla pagina non supporta l'elemento `<video>`, permettendoci di fornire una soluzione alternativa per i browser più vecchi. Può essere qualsiasi cosa tu voglia; in questo caso, abbiamo fornito un link diretto al file video, in modo che l'utente possa almeno accedervi in qualche modo indipendentemente dal browser che sta usando.

Il video incorporato apparirà all'incirca così:

![Un semplice lettore video che mostra un video di un piccolo coniglio bianco](simple-video.png)

Puoi [provare l'esempio dal vivo](https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/simple-video.html) qui (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/video-and-audio-content/simple-video.html).)

## Utilizzare più formati di origine per migliorare la compatibilità

C'è un problema con l'esempio sopra. È possibile che il video non venga riprodotto, poiché diversi browser supportano diversi formati video (e audio). Fortunatamente, ci sono cose che puoi fare per evitare che questo diventi un problema.

### Contenuto di un file multimediale

Innanzitutto, esaminiamo rapidamente la terminologia. I formati come OGG, WAV, MP4 e WebM sono chiamati **[formati di contenitore](/it/docs/Web/Media/Guides/Formats/Containers)**. Definiscono una struttura nella quale le tracce audio e/o video che compongono il media vengono memorizzate, insieme ai metadati che descrivono il media, quali codec vengono utilizzati per codificare i suoi canali, e così via.

Un file WebM contenente un film che ha una traccia video principale e una traccia di angolo alternativo, più audio sia per l'inglese che per lo spagnolo, oltre all'audio per una traccia di commento in inglese può essere concettualizzato come mostrato nel diagramma qui sotto. Sono incluse anche tracce di testo contenenti sottotitoli per il film di lungometraggio, sottotitoli in spagnolo per il film e sottotitoli in inglese per i commenti.

![Diagramma che concettualizza i contenuti di un file multimediale a livello di traccia.](containersandtracks.png)

Le tracce audio e video all'interno del contenitore contengono dati nel formato appropriato per il codec utilizzato per codificare quel media. Vengono utilizzati formati diversi per le tracce audio rispetto alle tracce video. Ogni traccia audio è codificata usando un [codec audio](/it/docs/Web/Media/Guides/Formats/Audio_codecs), mentre le tracce video sono codificate usando (come probabilmente avrai intuito) [un codec video](/it/docs/Web/Media/Guides/Formats/Video_codecs). Come abbiamo detto prima, diversi browser supportano diversi formati video e audio, e diversi formati di contenitore (come OGG, MP4, e WebM, che a loro volta possono contenere diversi tipi di video e audio).

Per esempio:

- Un contenitore WebM solitamente confeziona audio Vorbis o Opus con video VP8/VP9. Questo è supportato in tutti i browser moderni, anche se le versioni più vecchie potrebbero non funzionare.
- Un contenitore MP4 spesso confeziona audio AAC o MP3 con video H.264. Anche questo è supportato in tutti i browser moderni.
- Il contenitore Ogg tende a usare audio Vorbis e video Theora. È meglio supportato in Firefox e Chrome, ma è stato sostanzialmente superato dal formato WebM di qualità migliore.

Ci sono alcuni casi speciali. Ad esempio, per alcuni tipi di audio, i dati di un codec vengono spesso memorizzati senza un contenitore, o con un contenitore semplificato. Un caso di questo tipo è il codec FLAC, che viene memorizzato più comunemente in file FLAC, che sono solo tracce raw FLAC.

Un altro esempio è il popolarissimo "file MP3". Un "file MP3" è un file audio codificato utilizzando la compressione MPEG-1 Audio Layer III. Pur potendo includere metadati, non è incapsulato in un contenitore MPEG o MPEG-2 separato. Il suo ampio supporto negli elementi {{htmlelement("audio")}} e {{htmlelement("video")}} è in gran parte una testimonianza della sua duratura popolarità.

Un lettore audio tende a riprodurre direttamente una traccia audio, ad esempio, un file MP3 o Ogg. Questi non hanno bisogno di contenitori.

### Supporto dei file multimediali nei browser

> [!NOTE]
> Diversi formati popolari, come MP3 e MP4/H.264, sono eccellenti ma sono gravati da brevetti; cioè, ci sono brevetti che coprono alcune o tutte le tecnologie su cui si basano. Negli Stati Uniti, i brevetti coprivano l'MP3 fino al 2017, e l'H.264 è gravato da brevetti fino a almeno il 2027.
>
> A causa di questi brevetti, i browser che desiderano implementare il supporto per questi codec devono pagare tariffari di licenza tipicamente enormi. Inoltre, alcune persone preferiscono evitare software con restrizioni e scegliere di utilizzare solo formati open. Per queste ragioni legali e preferenziali, gli sviluppatori web si trovano a dover supportare più formati per raggiungere tutto il loro pubblico.

I codec descritti nella sezione precedente esistono per comprimere video e audio in file gestibili, poiché sia l'audio che il video raw sono entrambi estremamente grandi. Ogni browser web supporta un assortimento di **{{Glossary("Codec", "codec")}}**, come Vorbis o H.264, che vengono utilizzati per convertire l'audio e il video compressi in dati binari e viceversa. Ogni codec offre i propri vantaggi e svantaggi, e ogni contenitore può anche offrire le proprie caratteristiche positive e negative influenzando le tue decisioni su quali utilizzare.

Le cose diventano leggermente più complicate poiché non solo ogni browser supporta un diverso set di formati di file contenitore, ma ciascuno supporta anche una diversa selezione di codec. Per massimizzare le probabilità che il tuo sito web o app funzioni su un browser dell'utente, potresti dover fornire ogni file multimediale che usi in più formati. Se il tuo sito e il browser dell'utente non condividono un formato multimediale comune, i tuoi media non verranno riprodotti.

A causa delle complessità dell'assicurarsi che i media della tua app siano visibili attraverso ogni combinazione di browser, piattaforme e dispositivi che desideri raggiungere, scegliere la migliore combinazione di codec e contenitore può essere un compito complicato. Vedi [Scegliere il contenitore giusto](/it/docs/Web/Media/Guides/Formats/Containers#choosing_the_right_container) per aiuto nella scelta del formato file contenitore più adatto alle tue esigenze; similmente, vedi [Scegliere un codec video](/it/docs/Web/Media/Guides/Formats/Video_codecs#choosing_a_video_codec) e [Scegliere un codec audio](/it/docs/Web/Media/Guides/Formats/Audio_codecs#choosing_an_audio_codec) per aiuto nella scelta dei primi codec multimediali da usare per i tuoi contenuti e il tuo pubblico target.

Un'altra cosa da tenere a mente: i browser mobili possono supportare formati aggiuntivi non supportati dalle loro controparti desktop, proprio come potrebbero non supportare tutti gli stessi formati che la versione desktop supporta. Inoltre, sia i browser desktop che quelli mobili _possono_ essere progettati per trasferire la gestione della riproduzione multimediale (sia per tutti i media che solo per tipi specifici che non possono gestire internamente). Ciò significa che il supporto multimediale dipende in parte dal software che l'utente ha installato.

Quindi come facciamo? Dai un'occhiata al seguente [esempio aggiornato](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/video-and-audio-content/multiple-video-formats.html) ([prova dal vivo qui](https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/multiple-video-formats.html), anche):

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

Qui abbiamo rimosso l'attributo `src` dal tag {{HTMLElement("video")}} effettivo, e invece incluso elementi {{htmlelement("source")}} separati che puntano alle loro fonti. In questo caso il browser esaminerà gli elementi {{HTMLElement("source")}} e riprodurrà il primo per cui ha il codec per supportarlo. Includere fonti WebM e MP4 dovrebbe essere sufficiente per riprodurre il tuo video sulla maggior parte delle piattaforme e browser al giorno d'oggi.

Ogni elemento `<source>` ha anche un attributo [`type`](/it/docs/Web/HTML/Reference/Elements/source#type). Questo è opzionale, ma è consigliato includerlo. L'attributo `type` contiene il {{Glossary("MIME_type", "tipo MIME")}} del file specificato dal `<source>`, e i browser possono usare `type` per saltare immediatamente i video che non comprendono. Se `type` non è incluso, i browser caricheranno e proveranno a riprodurre ogni file finché non ne trovano uno che funziona, il che ovviamente richiede tempo ed è un uso non necessario delle risorse.

Consulta la nostra [guida ai tipi e formati multimediali](/it/docs/Web/Media/Guides/Formats) per aiuto nella scelta dei migliori contenitori e codec per le tue esigenze, nonché per cercare i giusti tipi MIME da specificare per ciascuno.

## Altre caratteristiche \<video>

Ci sono una serie di altre funzionalità che puoi includere quando visualizzi un video HTML. Dai un'occhiata al nostro prossimo esempio:

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

L'interfaccia risultante appare qualcosa di simile a questo:

![Un lettore video che mostra un'immagine di poster prima che venga riprodotto. L'immagine del poster dice esempio di video HTML, OMG hell yeah!](poster_screenshot_updated.png)

Le caratteristiche includono:

- [`width`](/it/docs/Web/HTML/Reference/Elements/video#width) e [`height`](/it/docs/Web/HTML/Reference/Elements/video#height)
  - : Puoi controllare la dimensione del video sia con questi attributi che con {{Glossary("CSS", "CSS")}}. In entrambi i casi, i video mantengono il loro rapporto larghezza-altezza nativo — conosciuto come **rapporto d'aspetto**. Se il rapporto d'aspetto non viene mantenuto dalle dimensioni che imposti, il video si allargherà per riempire lo spazio orizzontalmente, e lo spazio non riempito avrà semplicemente un colore di sfondo solido per impostazione predefinita.
- [`autoplay`](/it/docs/Web/HTML/Reference/Elements/video#autoplay)
  - : Fa cominciare a riprodurre immediatamente l'audio o il video, mentre il resto della pagina si carica. Si consiglia di non utilizzare video (o audio) in autoplay sui tuoi siti, perché gli utenti possono trovarlo molto fastidioso.
- [`loop`](/it/docs/Web/HTML/Reference/Elements/video#loop)
  - : Fa ricominciare il video (o l'audio) ogni volta che termina. Anche questo può essere fastidioso, quindi usalo solo se realmente necessario.
- [`muted`](/it/docs/Web/HTML/Reference/Elements/video#muted)
  - : Fa sì che i media vengano riprodotti con il suono spento di default.
- [`poster`](/it/docs/Web/HTML/Reference/Elements/video#poster)
  - : L'URL di un'immagine che verrà visualizzata prima che il video sia riprodotto. È destinata ad essere utilizzata come schermata di benvenuto o schermo pubblicitario.
- [`preload`](/it/docs/Web/HTML/Reference/Elements/video#preload)
  - : Utilizzato per il buffering di file di grandi dimensioni; può accettare uno di tre valori:
    - `"none"` non bufferizza il file
    - `"auto"` bufferizza il file multimediale
    - `"metadata"` bufferizza solo i metadati per il file

Puoi trovare l'esempio sopra disponibile per [essere riprodotto dal vivo su GitHub](https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/extra-video-features.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/video-and-audio-content/extra-video-features.html).) Nota che non abbiamo incluso l'attributo `autoplay` nella versione dal vivo — se il video comincia a riprodursi appena la pagina si carica, non riesci a vedere il poster!

## L'elemento \<audio>

L'elemento {{htmlelement("audio")}} funziona proprio come l'elemento {{htmlelement("video")}}, con alcune piccole differenze evidenziate di seguito. Un esempio tipico potrebbe apparire così:

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

Questo produce qualcosa di simile a quanto segue in un browser:

![Un semplice lettore audio con un pulsante play, un timer, un controllo del volume e una barra di progresso](audio-player.png)

> [!NOTE]
> Puoi [eseguire la demo audio dal vivo](https://mdn.github.io/learning-area/html/multimedia-and-embedding/video-and-audio-content/multiple-audio-formats.html) su GitHub (vedi anche il [codice sorgente dell'audio player](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/video-and-audio-content/multiple-audio-formats.html).)

Questo occupa meno spazio di un lettore video, poiché non c'è componente visiva — devi solo mostrare i controlli per riprodurre l'audio. Altre differenze dal video HTML sono le seguenti:

- L'elemento {{htmlelement("audio")}} non supporta gli attributi `width`/`height` — di nuovo, non c'è componente visiva, quindi non c'è nulla a cui assegnare una larghezza o altezza.
- Inoltre non supporta l'attributo `poster` — di nuovo, nessun componente visivo.

A parte questo, `<audio>` supporta tutte le stesse funzionalità di `<video>` — consulta le sezioni precedenti per maggiori informazioni su di esse.

## Visualizzare tracce di testo video

Ora discuteremo un concetto leggermente più avanzato che è davvero utile da conoscere. Molte persone non possono o non vogliono ascoltare il contenuto audio/video che trovano sul Web, almeno in determinati momenti. Per esempio:

- Molte persone hanno disabilità uditive (come essere parzialmente sordi o non udenti) quindi non possono ascoltare l'audio chiaramente se non del tutto.
- Altri potrebbero non essere in grado di ascoltare l'audio perché si trovano in ambienti rumorosi (come un bar affollato durante la visione di una partita sportiva).
- Analogamente, in ambienti in cui l'audio sarebbe una distrazione o un'interferenza (come in una biblioteca o quando un partner cerca di dormire), avere i sottotitoli può essere molto utile.
- Persone che non parlano la lingua del video potrebbero voler ottenere una trascrizione testuale o anche una traduzione per comprenderne il contenuto media.

Non sarebbe bello poter fornire a queste persone una trascrizione delle parole pronunciate nell'audio/video? Bene, grazie al video HTML, puoi farlo. Per farlo utilizziamo il formato di file [WebVTT](/it/docs/Web/API/WebVTT_API) e l'elemento {{htmlelement("track")}}.

> [!NOTE]
> "Trascrivere" significa "scrivere come testo le parole pronunciate." Il testo risultante è una "trascrizione."

WebVTT è un formato per scrivere file di testo contenenti più stringhe di testo insieme a metadati come il tempo nel video in cui ciascuna stringa di testo dovrebbe essere visualizzata, e persino informazioni di stile/posizionamento limitate. Queste stringhe di testo sono chiamate **cue**, e ci sono diversi tipi di cue che vengono usati per scopi diversi. I cue più comuni sono:

- sottotitoli
  - : Traduzioni di materiale straniero, per le persone che non comprendono le parole pronunciate nell'audio.
- didascalie
  - : Trascrizioni sincronizzate del dialogo o descrizioni di suoni significativi, per permettere alle persone che non possono sentire l'audio di capire cosa sta succedendo.
- descrizioni temporizzate
  - : Testo che dovrebbe essere pronunciato dal lettore multimediale al fine di descrivere le importanti visuali agli utenti ciechi o con disabilità visive.

Un tipico file WebVTT apparirà qualcosa di simile a questo:

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

Per far sì che venga visualizzato insieme alla riproduzione multimediale HTML, devi:

1. Salvarlo come un file `.vtt` da qualche parte in cui il server può servire (vedi sotto), come nella stessa directory del file HTML.
2. Collegarlo al file `.vtt` con l'elemento {{htmlelement("track")}}. `<track>` deve essere posto all'interno di `<audio>` o `<video>`, ma dopo tutti gli elementi `<source>`. Usa l'attributo [`kind`](/it/docs/Web/HTML/Reference/Elements/track#kind) per specificare se i cue sono `subtitles`, `captions` o `descriptions`. Inoltre, usa [`srclang`](/it/docs/Web/HTML/Reference/Elements/track#srclang) per dire al browser in quale lingua hai scritto i sottotitoli. Infine, aggiungi [`label`](/it/docs/Web/HTML/Reference/Elements/track#label) per aiutare i lettori a identificare la lingua che cercano.

Ecco un esempio:

```html
<video controls>
  <source src="example.mp4" type="video/mp4" />
  <source src="example.webm" type="video/webm" />
  <track kind="subtitles" src="subtitles_es.vtt" srclang="es" label="Spanish" />
</video>
```

Per provare ciò devi ospitare i file su un [server HTTP locale](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server). Nell'output nel browser, vedrai un video che mostra sottotitoli visualizzati, in una forma vicina a questa:

![Lettore video con controlli standard come play, stop, volume e attivazione e disattivazione dei sottotitoli. Il video in riproduzione mostra una scena di un uomo che tiene un'arma simile a una lancia, e un sottotitolo che recita "Esta hoja tiene passato oscuro."](video-player-with-captions.png)

Per ulteriori dettagli, inclusi su come aggiungere etichette, leggi [Aggiungere sottotitoli e didascalie al video HTML](/it/docs/Web/Media/Guides/Audio_and_video_delivery/Adding_captions_and_subtitles_to_HTML5_video). Puoi [trovare l'esempio](https://iandevlin.github.io/mdn/video-player-with-captions/) che accompagna questo articolo su GitHub, scritto da Ian Devlin (vedi anche il [codice sorgente](https://github.com/iandevlin/iandevlin.github.io/tree/master/mdn/video-player-with-captions) anch'esso.) Questo esempio utilizza un po' di JavaScript per permettere agli utenti di scegliere tra diversi sottotitoli. Nota che per attivare i sottotitoli, devi premere il pulsante "CC" e selezionare un'opzione — Inglese, Deutsch, o Español.

> [!NOTE]
> Le tracce di testo ti aiutano anche con la {{Glossary("SEO", "SEO")}}, poiché i motori di ricerca prosperano particolarmente sul testo. Le tracce di testo permettono anche ai motori di ricerca di collegarsi direttamente a un punto a metà del video.

## Apprendimento attivo: Incorpora il tuo audio e video

Per questo apprendimento attivo, vorremmo (idealmente) che tu esca nel mondo e registri qualche tuo video e audio — la maggior parte dei telefoni attualmente permette di registrare facilmente audio e video e, purché tu possa trasferirlo sul tuo computer, potrai utilizzarlo. Potrebbe essere necessario fare qualche conversione per ottenere un WebM e MP4 nel caso del video, e un MP3 e Ogg nel caso dell'audio, ma ci sono abbastanza programmi là fuori per permetterti di fare questo senza troppi problemi, come [Miro Video Converter](http://www.mirovideoconverter.com/) e [Audacity](https://sourceforge.net/projects/audacity/). Vorremmo che ci provassi!

Se non riesci a trovare nessun video o audio, allora puoi sentiti libero di usare i nostri [file audio e video di esempio](https://github.com/mdn/learning-area/tree/main/html/multimedia-and-embedding/video-and-audio-content) per svolgere questo esercizio. Puoi anche usare il nostro codice di esempio per riferimento.

Vorremmo che tu:

1. Salvassi i tuoi file audio e video in una nuova directory sul tuo computer.
2. Creassi un nuovo file HTML nella stessa directory, chiamato `index.html`.
3. Aggiungessi elementi {{HTMLElement("audio")}} e {{HTMLElement("video")}} alla pagina; facendoli visualizzare con i controlli predefiniti del browser.
4. Fornissi a entrambi gli elementi {{HTMLElement("source")}} in modo che i browser trovino il formato audio che supportano meglio e lo carichino. Questi dovrebbero includere attributi [`type`](/it/docs/Web/HTML/Reference/Elements/source#type).
5. Dessi all'elemento `<video>` un poster che sarà visualizzato prima che il video inizi a essere riprodotto. Divertiti a creare la tua immagine di poster.

Per un bonus aggiunto, potresti provare a ricercare le tracce di testo, e lavorare su come aggiungere delle didascalie al tuo video.

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare l'informazione più importante? Puoi trovare ulteriori test per verificare di aver trattenuto queste informazioni prima di proseguire — vedi [Metti alla prova le tue abilità: Multimedialità e incorporamenti](/it/docs/Learn_web_development/Core/Structuring_content/Test_your_skills/Multimedia_and_embedding).

## Riassunto

E questo è tutto — speriamo che ti sia divertito a giocare con video e audio nelle pagine web! Prossimamente, ti presenteremo una sfida per testare le tue abilità con i media HTML.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_images", "Learn_web_development/Core/Structuring_content/Mozilla_splash_page", "Learn_web_development/Core/Structuring_content")}}
