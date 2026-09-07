---
title: Ottimizzazione delle prestazioni HTML
short-title: HTML performante
slug: Learn_web_development/Extensions/Performance/HTML
l10n:
  sourceCommit: f542ed344953b3312fc92150bba11536667e288a
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/JavaScript", "Learn_web_development/Extensions/Performance/CSS", "Learn_web_development/Extensions/Performance")}}

Per impostazione predefinita, HTML è veloce e accessibile. È compito degli sviluppatori assicurarsi di preservare queste due proprietà durante la creazione o la modifica del codice HTML. Possono sorgere complicazioni quando, ad esempio, la dimensione del file di un embed {{htmlelement("video")}} è troppo grande oppure quando il parsing di JavaScript blocca il rendering degli elementi critici della pagina. Questo articolo illustra le principali funzionalità HTML relative alle prestazioni che possono migliorare drasticamente la qualità della pagina web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        > e conoscenze di base delle
        <a href="/it/docs/Learn_web_development/Getting_started/Your_first_website"
          >tecnologie web lato client</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Apprendere l'impatto di HTML sulle prestazioni dei siti web
        e come ottimizzare HTML per migliorare le prestazioni.
      </td>
    </tr>
  </tbody>
</table>

## Ottimizzare o non ottimizzare

La prima domanda a cui rispondere prima di iniziare a ottimizzare HTML è: "che cosa occorre ottimizzare?". Alcuni dei suggerimenti e delle tecniche illustrati di seguito sono buone pratiche che apporteranno vantaggi praticamente a qualsiasi progetto web, mentre altri sono necessari solo in determinate situazioni. Cercare di applicare tutte queste tecniche ovunque probabilmente non è necessario e potrebbe essere una perdita di tempo. Occorre capire quali ottimizzazioni delle prestazioni sono effettivamente necessarie in ciascun progetto.

Per farlo, è necessario [misurare le prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance) del sito. Come mostra questo collegamento, esistono diversi modi per misurare le prestazioni, alcuni dei quali coinvolgono sofisticate [API per le prestazioni](/it/docs/Web/API/Performance_API). Il modo migliore per iniziare, tuttavia, è imparare a usare strumenti quali gli strumenti integrati del browser per la [rete](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#network_monitor_tools) e le [prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#performance_monitor_tools), per esaminare le parti della pagina che richiedono molto tempo per il caricamento e che necessitano di ottimizzazione.

## Principali problemi relativi alle prestazioni HTML

HTML è semplice dal punto di vista delle prestazioni: è costituito principalmente da testo, che ha dimensioni ridotte e pertanto è generalmente rapido da scaricare e renderizzare. I principali problemi che possono influire sulle prestazioni di una pagina web includono:

- Dimensioni dei file di immagini e video: è importante considerare come gestire il contenuto degli elementi sostituiti quali {{htmlelement("img")}} e {{htmlelement("video")}}. I file di immagini e video sono grandi e possono aumentare significativamente il peso della pagina. È quindi importante ridurre al minimo il numero di byte scaricati sul dispositivo dell'utente, ad esempio fornendo immagini più piccole per dispositivi mobili. Occorre inoltre considerare come migliorare le prestazioni percepite caricando immagini e video in una pagina solo quando sono necessari.
- Distribuzione di contenuti incorporati: in genere si tratta di contenuti incorporati negli elementi {{htmlelement("iframe")}}. Il caricamento di contenuti in `<iframe>` può influire significativamente sulle prestazioni, quindi deve essere valutato attentamente.
- Ordine di caricamento delle risorse: per massimizzare le prestazioni percepite ed effettive, HTML dovrebbe essere caricato per primo, nell'ordine in cui appare nella pagina. È quindi possibile usare varie funzionalità per influenzare l'ordine di caricamento delle risorse e ottenere prestazioni migliori. Ad esempio, è possibile precaricare anticipatamente CSS e font critici, ma rimandare JavaScript non critico a un momento successivo.

> [!NOTE]
> Si può sostenere l'opportunità di semplificare la struttura HTML e di [minimizzare](<https://en.wikipedia.org/wiki/Minification_(programming)>) il codice sorgente, affinché rendering e download siano più rapidi. Tuttavia, la dimensione dei file HTML è trascurabile rispetto a immagini e video, e il rendering dei browser è oggi molto veloce. Se il sorgente HTML è così grande e complesso da causare problemi di prestazioni nel rendering e nel download, probabilmente esistono problemi più importanti ed è opportuno semplificarlo e suddividere il contenuto.

## Gestione responsiva degli elementi sostituiti

Il [design responsivo](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design) ha rivoluzionato il modo in cui viene gestito il layout dei contenuti web su dispositivi diversi. Uno dei principali vantaggi che rende possibile è il passaggio dinamico tra layout ottimizzati per dimensioni dello schermo diverse, ad esempio un layout per schermi larghi rispetto a un layout per schermi stretti, cioè mobili. Può inoltre gestire il passaggio dinamico dei contenuti in base ad altri attributi del dispositivo, quali la risoluzione o la preferenza per una combinazione di colori chiara o scura.

La tecnica denominata "mobile first" può garantire che il layout predefinito sia destinato ai dispositivi con schermo piccolo, così i dispositivi mobili possono scaricare solo immagini adatte ai loro schermi e non devono subire l'impatto sulle prestazioni dovuto al download di immagini desktop più grandi. Tuttavia, poiché questa operazione è controllata tramite [media query](/it/docs/Web/CSS/Guides/Media_queries/Using) nel CSS, può influire positivamente solo sulle prestazioni delle immagini caricate in CSS.

Nelle sezioni seguenti verrà riepilogato come implementare elementi sostituiti responsivi. Maggiori dettagli su queste implementazioni sono disponibili nelle guide [Video e audio HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio) e [Immagini responsive](/it/docs/Web/HTML/Guides/Responsive_images).

### Fornire risoluzioni delle immagini diverse tramite srcset

Per fornire versioni della stessa immagine con risoluzioni diverse in base alla risoluzione del dispositivo e alla dimensione della viewport, è possibile usare gli attributi [`srcset`](/it/docs/Web/HTML/Reference/Elements/img#srcset) e [`sizes`](/it/docs/Web/HTML/Reference/Elements/img#sizes).

Questo esempio fornisce immagini di dimensioni diverse per larghezze dello schermo differenti:

```html
<img
  srcset="480w.jpg 480w, 800w.jpg 800w"
  sizes="(width <= 600px) 480px,
         800px"
  src="800w.jpg"
  alt="Family portrait" />
```

`srcset` fornisce la dimensione intrinseca delle immagini sorgente insieme ai relativi nomi di file, mentre `sizes` fornisce media query insieme alle larghezze degli slot dell'immagine che devono essere riempiti in ogni caso. Il browser decide quindi quali immagini ha senso caricare per ogni slot. Ad esempio, se la larghezza dello schermo è `600px` o inferiore, allora `width <= 600px` è vero e, pertanto, lo slot da riempire è `480px`. In questo caso, il browser probabilmente sceglierà di caricare il file 480w.jpg, un'immagine larga 480px. Questo favorisce le prestazioni perché i browser non caricano immagini più grandi di quelle necessarie.

Questo esempio fornisce immagini con risoluzioni diverse per diverse risoluzioni dello schermo:

```html
<img
  srcset="320w.jpg, 480w.jpg 1.5x, 640w.jpg 2x"
  src="640w.jpg"
  alt="Family portrait" />
```

`1.5x`, `2x` e così via sono indicatori di risoluzione relativa. Se l'immagine viene stilizzata con una larghezza di 320px, ad esempio tramite `width: 320px` in CSS, il browser caricherà `320w.jpg` se il dispositivo ha una bassa risoluzione, ovvero un {{Glossary("device_pixel", "pixel del dispositivo")}} per pixel CSS, oppure `640x.jpg` se il dispositivo ha un'alta risoluzione, ovvero due o più pixel del dispositivo per pixel CSS.

In entrambi i casi, l'attributo `src` fornisce un'immagine predefinita da caricare se il browser non supporta `src`/`srcset`.

### Fornire sorgenti diverse per immagini e video

L'elemento {{htmlelement("picture")}} si basa sul tradizionale elemento {{htmlelement("img")}} e consente di fornire più sorgenti diverse per situazioni differenti. Ad esempio, se il layout è largo, probabilmente sarà necessaria un'immagine larga; se invece è stretto, sarà necessaria un'immagine più stretta che funzioni comunque in quel contesto.

Naturalmente, questo permette anche di fornire un download di informazioni più piccolo sui dispositivi mobili, favorendo le prestazioni.

Un esempio è il seguente:

```html
<picture>
  <source media="(width < 800px)" srcset="narrow-banner-480w.jpg" />
  <source media="(width >= 800px)" srcset="wide-banner-800w.jpg" />
  <img src="large-banner-800w.jpg" alt="Dense forest scene" />
</picture>
```

Gli elementi {{htmlelement("source")}} contengono media query negli attributi `media`. Se una media query restituisce true, viene caricata l'immagine a cui fa riferimento l'attributo `srcset` dell'elemento `<source>`. Nell'esempio precedente, se la larghezza della viewport è inferiore a `800px`, viene caricata l'immagine `narrow-banner-480w.jpg`. Si noti inoltre che l'elemento `<picture>` include un elemento `<img>`, che fornisce un'immagine predefinita da caricare nel caso di browser che non supportano `<picture>`.

Notare l'uso dell'attributo `srcset` in questo esempio. Come mostrato nella sezione precedente, è possibile fornire risoluzioni diverse per ogni sorgente dell'immagine.

Gli elementi `<video>` funzionano in modo simile per quanto riguarda la fornitura di sorgenti diverse:

```html
<video controls>
  <source src="video/smaller.mp4" type="video/mp4" />
  <source src="video/smaller.webm" type="video/webm" />
  <source src="video/larger.mp4" type="video/mp4" media="(width >= 800px)" />
  <source src="video/larger.webm" type="video/webm" media="(width >= 800px)" />

  <!-- fallback for browsers that don't support video element -->
  <a href="video/larger.mp4">download video</a>
</video>
```

Esistono tuttavia alcune differenze fondamentali tra la fornitura di sorgenti per immagini e video:

- Nell'esempio precedente viene usato `src` anziché `srcset`; non è possibile specificare risoluzioni diverse per i video tramite `srcset`.
- Le risoluzioni diverse vengono invece specificate all'interno dei diversi elementi `<source>`.
- Notare come vengano specificati anche formati video diversi all'interno di diversi elementi `<source>`, con ciascun formato identificato tramite il relativo tipo MIME nell'attributo `type`. I browser caricheranno il primo formato supportato che incontrano, per il quale il test della media query restituisce true.

### Caricamento differito delle immagini

Una tecnica molto utile per migliorare le prestazioni è il **caricamento differito**. Si riferisce alla pratica di non caricare immediatamente tutte le immagini quando viene renderizzato HTML, ma di caricarle solo quando sono effettivamente visibili all'utente nella viewport, o stanno per diventarlo. Ciò significa che il contenuto immediatamente visibile e utilizzabile è pronto più rapidamente, mentre il contenuto successivo vede renderizzate le proprie immagini solo quando vi si scorre fino a raggiungerlo, e il browser non sprecherà larghezza di banda caricando immagini che l'utente non vedrà mai.

Storicamente, il caricamento differito è stato gestito tramite JavaScript, ma i browser ora dispongono di un attributo `loading` che può indicare al browser di eseguire automaticamente il caricamento differito delle immagini:

```html
<img src="800w.jpg" alt="Family portrait" loading="lazy" />
```

Per informazioni dettagliate, consultare [Browser-level image lazy loading for the web](https://web.dev/articles/browser-level-image-lazy-loading) su web.dev.

### Caricamento differito di video e audio

È inoltre possibile caricare in modo differito il contenuto video fino alla riproduzione del video, usando l'attributo `preload`. Ad esempio:

```html
<video controls preload="none" poster="poster.jpg">
  <source src="video.webm" type="video/webm" />
  <source src="video.mp4" type="video/mp4" />
</video>
```

Assegnare a `preload` il valore `none` indica al browser di non precaricare alcun dato video prima che l'utente decida di riprodurlo, il che è chiaramente positivo per le prestazioni. Verrà invece mostrata soltanto l'immagine indicata dall'attributo `poster`. Browser diversi hanno comportamenti predefiniti diversi per il caricamento dei video, pertanto è opportuno essere espliciti.

Assegnare a `preload` il valore `metadata` richiede al browser di scaricare i dati minimi necessari per visualizzare il video prima della riproduzione, ad esempio durata, dimensioni e forse il fotogramma iniziale.

L'attributo `loading` può migliorare ulteriormente il caricamento differito dei video rimandando il caricamento di tutti i dati video, indipendentemente dal valore di `preload`, nonché il caricamento dell'immagine `poster`, finché il video non è vicino alla viewport; a quel punto il valore di `preload` viene usato normalmente.

```html
<video controls preload="none" poster="poster.jpg" loading="lazy">
  <source src="video.webm" type="video/webm" />
  <source src="video.mp4" type="video/mp4" />
</video>
```

Questo può essere usato anche con contenuti audio:

```html
<audio
  controls
  src="/shared-assets/audio/t-rex-roar.mp3"
  loading="lazy"></audio>
```

Per informazioni dettagliate, consultare [Fast playback with audio and video preload](https://web.dev/articles/fast-playback-with-preload) su web.dev.

## Gestione dei contenuti incorporati

È molto comune incorporare nelle pagine web contenuti provenienti da altre fonti. Questo avviene più comunemente quando si visualizza pubblicità su un sito per generare entrate: gli annunci sono solitamente generati da un'azienda terza e incorporati nella pagina. Altri usi possono includere:

- Visualizzare contenuti condivisi di cui un utente può aver bisogno in più pagine, come un carrello della spesa o informazioni del profilo.
- Visualizzare contenuti di terze parti relativi al sito principale dell'organizzazione, come un feed di post sui social media.

L'incorporamento dei contenuti viene eseguito più comunemente usando elementi {{htmlelement("iframe")}}, anche se esistono altri elementi di incorporamento meno usati, quali {{htmlelement("object")}} e {{htmlelement("embed")}}. Questa sezione si concentrerà sugli elementi `<iframe>`.

Il consiglio più importante e fondamentale per l'uso degli elementi `<iframe>` è: "Non usare `<iframe>` incorporati se non è assolutamente necessario". Se si crea una pagina con più riquadri di informazioni differenti, potrebbe sembrare sensato dal punto di vista organizzativo suddividerli in pagine separate e caricarli in diversi `<iframe>`. Tuttavia, questo comporta una serie di problemi relativi alle prestazioni e non solo:

- Caricare il contenuto in un `<iframe>` è molto più costoso che caricare il contenuto come parte della stessa pagina diretta: non solo richiede richieste HTTP aggiuntive per caricare il contenuto, ma il browser deve anche creare un'istanza di pagina separata per ciascuno. Ciascuno è in effetti un'istanza di pagina web separata incorporata nella pagina web di primo livello.
- In seguito al punto precedente, sarà inoltre necessario gestire separatamente qualsiasi stilizzazione CSS o manipolazione JavaScript per ogni diverso `<iframe>`, a meno che le pagine incorporate non provengano dalla stessa origine, il che diventa molto più complesso. Non è possibile selezionare contenuti incorporati con CSS e JavaScript applicati alla pagina di primo livello, o viceversa. Si tratta di una misura di sicurezza ragionevole e fondamentale per il web. Si immagini tutti i problemi che potrebbero sorgere se contenuti incorporati di terze parti potessero eseguire arbitrariamente script su qualsiasi pagina in cui sono incorporati.
- Ogni `<iframe>` dovrebbe inoltre caricare separatamente eventuali dati condivisi e file multimediali: non è possibile condividere risorse memorizzate nella cache tra incorporamenti di pagine differenti, a meno che, ancora una volta, le pagine incorporate non provengano dalla stessa origine. Questo può portare una pagina a usare molta più larghezza di banda di quanto previsto.

È consigliabile inserire il contenuto in un'unica pagina. Se si desidera recuperare dinamicamente nuovi contenuti mentre la pagina cambia, è comunque preferibile per le prestazioni caricarli nella stessa pagina anziché inserirli in un `<iframe>`. È possibile recuperare i nuovi dati, ad esempio, usando il metodo [`fetch()`](/it/docs/Web/API/Window/fetch), quindi iniettarli nella pagina tramite script DOM. Per maggiori informazioni, consultare [Effettuare richieste di rete con JavaScript](/it/docs/Learn_web_development/Core/Scripting/Network_requests) e [Introduzione agli script DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting).

> [!NOTE]
> Se il contenuto è sotto controllo ed è relativamente semplice, si potrebbe considerare di usare contenuto codificato in base 64 nell'attributo `src` per popolare l'`<iframe>`, oppure persino inserire HTML non elaborato nell'attributo `srcdoc` (consultare [Iframe Performance Part 2: The Good News](https://medium.com/slices-of-bread/iframe-performance-part-2-the-good-news-26eb53cea429) per maggiori informazioni).

Se è necessario usare `<iframe>`, usarli con parsimonia.

### Caricamento differito degli iframe

Come per gli elementi `<img>`, è possibile usare l'attributo `loading` per indicare al browser di caricare in modo differito contenuti `<iframe>` inizialmente fuori dallo schermo, migliorando così le prestazioni:

```html
<iframe src="https://example.com" loading="lazy" width="600" height="400">
</iframe>
```

Per maggiori informazioni, consultare [It's time to lazy-load offscreen iframes!](https://web.dev/articles/iframe-lazy-loading).

## Gestione dell'ordine di caricamento delle risorse

L'ordine di caricamento delle risorse è importante per massimizzare le prestazioni percepite ed effettive. Quando viene caricata una pagina web:

1. HTML viene generalmente analizzato per primo, nell'ordine in cui appare nella pagina.
2. Qualsiasi CSS trovato viene analizzato per comprendere gli stili da applicare alla pagina. Durante questo periodo, le risorse collegate come immagini e web font iniziano a essere recuperate.
3. Qualsiasi JavaScript trovato viene analizzato, valutato ed eseguito sulla pagina. Per impostazione predefinita, questo blocca l'analisi dell'HTML che appare dopo gli elementi {{htmlelement("script")}} in cui viene incontrato JavaScript.
4. Poco dopo, il browser determina come deve essere stilizzato ogni elemento HTML, in base al CSS applicato.
5. Il risultato stilizzato viene quindi disegnato sullo schermo.

> [!NOTE]
> Questa è una descrizione molto semplificata di ciò che accade, ma fornisce un'idea generale.

Diverse funzionalità HTML consentono di modificare il modo in cui avviene il caricamento delle risorse per migliorare le prestazioni. Ora ne verranno esaminate alcune.

### Gestione del caricamento JavaScript

L'analisi e l'esecuzione di JavaScript bloccano l'analisi del contenuto DOM successivo. Questo aumenta il tempo prima che tale contenuto sia renderizzato e utilizzabile dagli utenti della pagina. Uno script piccolo non farà molta differenza, ma occorre considerare che le moderne applicazioni web tendono a usare molto JavaScript.

Un altro effetto collaterale del comportamento predefinito di analisi di JavaScript è che, se lo script renderizzato dipende dal contenuto DOM che appare più avanti nella pagina, si otterranno errori.

Ad esempio, si immagini un semplice paragrafo in una pagina:

```html
<p>My paragraph</p>
```

Ora si immagini un file JavaScript contenente il codice seguente:

```js
const pElem = document.querySelector("p");

pElem.addEventListener("click", () => {
  alert("You clicked the paragraph");
});
```

Questo script può essere applicato alla pagina facendo riferimento a esso in un elemento `<script>` in questo modo:

```html
<script src="index.js"></script>
```

Se questo elemento `<script>` viene inserito prima dell'elemento `<p>` nell'ordine del sorgente, ad esempio nell'elemento {{htmlelement("head")}}, la pagina genererà un errore. Chrome, ad esempio, segnala "Uncaught TypeError: Cannot read properties of null (reading 'addEventListener')" nella console. Ciò accade perché lo script dipende dall'elemento `<p>` per funzionare, ma nel momento in cui lo script viene analizzato, l'elemento `<p>` non esiste ancora nella pagina. Non è ancora stato renderizzato.

Il problema precedente può essere risolto inserendo l'elemento `<script>` dopo l'elemento `<p>`, ad esempio alla fine del corpo del documento, oppure eseguendo il codice all'interno di un gestore di eventi appropriato, ad esempio eseguendolo su [`DOMContentLoaded`](/it/docs/Web/API/Document/DOMContentLoaded_event), che si attiva quando il DOM è stato completamente analizzato.

Tuttavia, questo non risolve il problema dell'attesa del caricamento dello script. È possibile ottenere prestazioni migliori aggiungendo l'attributo `async` all'elemento `<script>`:

```html
<script async src="index.js"></script>
```

Questo fa sì che lo script venga recuperato in parallelo con l'analisi del DOM, in modo che sia pronto nello stesso momento e non blocchi il rendering, migliorando così le prestazioni.

> [!NOTE]
> Esiste un altro attributo, `defer`, che fa eseguire lo script dopo che il documento è stato analizzato, ma prima dell'attivazione di `DOMContentLoaded`. Ha un effetto simile a `async`.

Un altro suggerimento per la gestione del caricamento JavaScript consiste nel suddividere lo script in moduli di codice e caricare ogni parte quando necessario, anziché inserire tutto il codice in un unico script enorme e caricarlo interamente all'inizio. Questa operazione viene eseguita usando i [moduli JavaScript](/it/docs/Web/JavaScript/Guide/Modules). Per una guida dettagliata, leggere l'articolo collegato.

### Precaricare contenuti con rel="preload"

Anche il recupero di altre risorse, quali immagini, video o file di font, collegate da HTML, CSS e JavaScript può causare problemi di prestazioni, bloccando l'esecuzione del codice e rallentando l'esperienza. Un modo per attenuare questi problemi è usare `rel="preload"` per trasformare gli elementi {{htmlelement("link")}} in precaricatori. Ad esempio:

```html
<link rel="preload" href="sintel-short.mp4" as="video" type="video/mp4" />
```

Quando incontra un collegamento `rel="preload"`, il browser recupera la risorsa di riferimento non appena possibile e la rende disponibile nella cache del browser, affinché sia pronta prima per l'uso quando viene referenziata nel codice successivo. È utile precaricare risorse ad alta priorità che l'utente incontrerà all'inizio di una pagina, affinché l'esperienza sia il più fluida possibile.

Per informazioni dettagliate sull'uso di `rel="preload"`, consultare i seguenti articoli:

- [`rel="preload"`](/it/docs/Web/HTML/Reference/Attributes/rel/preload)
- [Preload critical assets to improve loading speed](https://web.dev/articles/preload-critical-assets) su web.dev (2020)

> [!NOTE]
> È possibile usare `rel="preload"` anche per precaricare file CSS e JavaScript.

> [!NOTE]
> Esistono altri valori [`rel`](/it/docs/Web/HTML/Reference/Attributes/rel) progettati anch'essi per velocizzare vari aspetti del caricamento della pagina: `dns-prefetch`, `preconnect`, `modulepreload` e `prefetch`. Consultare la pagina collegata per scoprire cosa fanno.

## Vedere anche

- [Effettuare richieste di rete con JavaScript](/it/docs/Learn_web_development/Core/Scripting/Network_requests)
- [Introduzione agli script DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting)

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/JavaScript", "Learn_web_development/Extensions/Performance/CSS", "Learn_web_development/Extensions/Performance")}}
