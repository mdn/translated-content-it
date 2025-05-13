---
title: Ottimizzazione delle prestazioni HTML
short-title: HTML performante
slug: Learn_web_development/Extensions/Performance/HTML
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/Javascript", "Learn_web_development/Extensions/Performance/CSS", "Learn_web_development/Extensions/Performance")}}

L'HTML è per sua natura rapido e accessibile. È nostro compito, come sviluppatori, assicurarci di preservare queste due caratteristiche quando creiamo o modifichiamo il codice HTML. Le complicazioni possono verificarsi quando, ad esempio, la dimensione di un file di incorporamento di un {{htmlelement("video")}} è troppo grande o quando il parsing di JavaScript blocca il rendering di elementi critici della pagina. Questo articolo vi guida attraverso le principali funzionalità di prestazione HTML che possono migliorare notevolmente la qualità della vostra pagina web.

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
        Imparare l'impatto dell'HTML sulle prestazioni del sito web
        e come ottimizzare il proprio HTML per migliorare le prestazioni.
      </td>
    </tr>
  </tbody>
</table>

## Ottimizzare o non ottimizzare

La prima domanda a cui deve rispondere prima di iniziare a ottimizzare il suo HTML è "cosa devo ottimizzare?". Alcuni dei suggerimenti e delle tecniche discussi di seguito sono buone pratiche che beneficeranno quasi ogni progetto web, mentre alcuni sono richiesti solo in determinate situazioni. Provare ad applicare tutte queste tecniche ovunque probabilmente è inutile e potrebbe essere una perdita di tempo. Dovrebbe capire quali ottimizzazioni delle prestazioni sono effettivamente necessarie in ogni progetto.

Per fare ciò, deve [misurare le prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance) del suo sito. Come mostra questo link, ci sono diversi modi per misurare le prestazioni, alcuni dei quali coinvolgono sofisticate [API di prestazione](/it/docs/Web/API/Performance_API). Tuttavia, il modo migliore per iniziare è imparare come utilizzare strumenti come quelli integrati del browser per [la rete](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#network_monitor_tools) e [le prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#performance_monitor_tools), per esaminare le parti della pagina che richiedono tempo per essere caricate e che necessitano di ottimizzazione.

## Principali problemi di prestazione HTML

L'HTML è semplice in termini di prestazioni — è principalmente testo, che è piccolo in dimensioni, e quindi il download e il rendering sono per lo più rapidi. I principali problemi che possono influenzare le prestazioni di una pagina web includono:

- Dimensione dei file immagine e video: È importante considerare come gestire il contenuto degli elementi sostituiti come {{htmlelement("img")}} e {{htmlelement("video")}}. I file immagine e video sono grandi e possono aggiungere significativamente al peso della pagina. Pertanto, è importante minimizzare la quantità di byte scaricati sul dispositivo di un utente (ad esempio, servire immagini più piccole per dispositivi mobili). Deve anche considerare di migliorare le prestazioni percepite caricando immagini e video su una pagina solo quando necessario.
- Consegna dei contenuti incorporati: Si tratta solitamente del contenuto incorporato negli elementi {{htmlelement("iframe")}}. Il caricamento di contenuti dentro `<iframe>` può influenzare significativamente le prestazioni, quindi deve essere considerato attentamente.
- Ordine di caricamento delle risorse: Per massimizzare le prestazioni percepite e reali, l'HTML dovrebbe essere caricato per primo, nell'ordine in cui appare sulla pagina. Successivamente può usare varie funzionalità per influenzare l'ordine di caricamento delle risorse per migliorare le prestazioni. Ad esempio, può caricare anticipatamente i CSS critici e i font, ma posticipare il JavaScript non critico fino a un momento successivo.

> [!NOTE]
> C'è un argomento da fare per semplificare la struttura del proprio HTML e [minificare](<https://en.wikipedia.org/wiki/Minification_(programming)>) il proprio codice sorgente, in modo che il rendering e i download siano più rapidi. Tuttavia, la dimensione del file HTML è trascurabile rispetto a immagini e video, e il rendering del browser è molto veloce oggigiorno. Se il suo sorgente HTML è così grande e complesso da causare problemi di rendering e download delle prestazioni, probabilmente ha problemi più grandi e dovrebbe mirare a semplificarlo e suddividere il contenuto.

## Gestione reattiva degli elementi sostituiti

Il [design reattivo](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design) ha rivoluzionato il modo in cui il layout del contenuto web è gestito su diversi dispositivi. Un vantaggio chiave che abilita è il passaggio dinamico di layout ottimizzati per diverse dimensioni dello schermo, ad esempio un layout a schermo ampio rispetto a un layout a schermo stretto (mobile). Può anche gestire il passaggio dinamico di contenuti basati su altri attributi del dispositivo, come la risoluzione o la preferenza per lo schema di colori chiaro o scuro.

La cosiddetta tecnica "mobile first" può garantire che il layout predefinito sia per dispositivi con schermo piccolo, quindi i dispositivi mobili possono semplicemente scaricare immagini adatte ai loro schermi e non devono subire il costo prestazionale di scaricare immagini di grandi dimensioni per desktop. Tuttavia, poiché ciò è controllato utilizzando [media query](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries) nel suo CSS, può influenzare positivamente le prestazioni delle immagini caricate in CSS.

Nelle sezioni seguenti, riassumeremo come implementare elementi sostituiti reattivi. Può trovare molti più dettagli su queste implementazioni nella [Guida ai video e audio in HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio) e nella [Guida alle immagini reattive](/it/docs/Web/HTML/Guides/Responsive_images).

### Fornire diverse risoluzioni delle immagini tramite srcset

Per fornire diverse versioni a risoluzione dell'immagine in base alla risoluzione del dispositivo e alla dimensione della viewport, può usare gli attributi [`srcset`](/it/docs/Web/HTML/Reference/Elements/img#srcset) e [`sizes`](/it/docs/Web/HTML/Reference/Elements/img#sizes).

Questo esempio fornisce immagini di diverse dimensioni per diverse larghezze di schermo:

```html
<img
  srcset="480w.jpg 480w, 800w.jpg 800w"
  sizes="(max-width: 600px) 480px,
         800px"
  src="800w.jpg"
  alt="Family portrait" />
```

`srcset` fornisce la dimensione intrinseca delle immagini sorgente insieme ai loro nomi di file, e `sizes` fornisce le media query insieme alle larghezze degli slot immagine che devono essere riempiti in ogni caso. Il browser decide quindi quali immagini hanno senso caricare per ciascun slot. Come esempio, se la larghezza dello schermo è `600px` o meno, allora `max-width: 600px` è vero, e quindi, lo slot da riempire si dice essere `480px`. In questo caso, il browser probabilmente sceglierà di caricare il file 480w.jpg (immagine larga 480px). Questo aiuta con le prestazioni perché i browser non caricano immagini più grandi di quanto necessario.

Questo esempio fornisce immagini a risoluzioni diverse per diverse risoluzioni di schermo:

```html
<img
  srcset="320w.jpg, 480w.jpg 1.5x, 640w.jpg 2x"
  src="640w.jpg"
  alt="Family portrait" />
```

`1.5x`, `2x`, ecc. sono indicatori di risoluzione relativa. Se l'immagine è stilizzata per essere larga 320px (ad esempio, con `width: 320px` in CSS), il browser caricherà `320w.jpg` se il dispositivo è a bassa risoluzione (un {{Glossary("device_pixel", "device pixel")}} per ogni pixel CSS), o `640x.jpg` se il dispositivo è ad alta risoluzione (due device pixel per ogni pixel CSS o più).

In entrambi i casi, l'attributo `src` fornisce un'immagine predefinita da caricare se il browser non supporta `src`/`srcset`.

### Fornire diverse sorgenti per immagini e video

L'elemento {{htmlelement("picture")}} si basa sull'elemento {{htmlelement("img")}} tradizionale, consentendo di fornire più sorgenti diverse per situazioni diverse. Ad esempio, se il layout è ampio, vorrà probabilmente un'immagine ampia, e se è stretto, vorrà un'immagine più stretta che funzioni comunque in quel contesto.

Naturalmente, questo funziona anche per fornire un download più piccolo di informazioni su dispositivi mobili, aiutando con le prestazioni.

Un esempio è il seguente:

```html
<picture>
  <source media="(max-width: 799px)" srcset="narrow-banner-480w.jpg" />
  <source media="(min-width: 800px)" srcset="wide-banner-800w.jpg" />
  <img src="large-banner-800w.jpg" alt="Dense forest scene" />
</picture>
```

Gli elementi {{htmlelement("source")}} contengono media query all'interno degli attributi `media`. Se una media query restituisce vero, l'immagine referenziata nell'attributo `srcset` del suo elemento `<source>` viene caricata. Nell'esempio sopra, se la larghezza della viewport è `799px` o meno, viene caricata l'immagine `narrow-banner-480w.jpg`. Noti anche come l'elemento `<picture>` includa un elemento `<img>`, che fornisce un'immagine predefinita da caricare nel caso di browser che non supportano `<picture>`.

Noti l'uso dell'attributo `srcset` in questo esempio. Come mostrato nella sezione precedente, può fornire diverse risoluzioni per ciascuna sorgente di immagine.

Gli elementi `<video>` funzionano in modo simile, in termini di fornitura di sorgenti diverse:

```html
<video controls>
  <source src="video/smaller.mp4" type="video/mp4" />
  <source src="video/smaller.webm" type="video/webm" />
  <source src="video/larger.mp4" type="video/mp4" media="(min-width: 800px)" />
  <source
    src="video/larger.webm"
    type="video/webm"
    media="(min-width: 800px)" />

  <!-- fallback for browsers that don't support video element -->
  <a href="video/larger.mp4">download video</a>
</video>
```

Tuttavia, ci sono alcuni differenze chiave tra la fornitura di sorgenti per immagini e video:

- Nell'esempio sopra, stiamo usando `src` piuttosto che `srcset`; non può specificare diverse risoluzioni per video tramite `srcset`.
- Invece, specifica diverse risoluzioni all'interno dei diversi elementi `<source>`.
- Noti come stiamo anche specificando diversi formati video all'interno dei diversi elementi `<source>`, con ciascun formato identificato tramite il suo tipo MIME all'interno dell'attributo `type`. I browser caricheranno il primo che incontrano che supportano, dove il test della media query restituisce vero.

### Caricamento differito delle immagini

Una tecnica molto utile per migliorare le prestazioni è il **caricamento differito**. Questo si riferisce alla pratica di non caricare tutte le immagini immediatamente quando l'HTML è renderizzato, ma invece di caricarle solo quando sono effettivamente visibili all'utente nella viewport (o prossimamente visibili). Questo significa che il contenuto immediatamente visibile/utilizzabile è pronto per essere usato più rapidamente, mentre il contenuto successivo ha le sue immagini renderizzate solo quando scorre a quel punto, e il browser non spreca banda di rete caricando immagini che l'utente non vedrà mai.

Il caricamento differito è stato storicamente gestito usando JavaScript, ma i browser ora hanno un attributo `loading` disponibile che può istruire il browser a caricare le immagini su richiesta automaticamente:

```html
<img src="800w.jpg" alt="Family portrait" loading="lazy" />
```

Vedere [Browser-level image lazy loading for the web](https://web.dev/articles/browser-level-image-lazy-loading) su web.dev per informazioni dettagliate.

Può anche caricare contenuti video su richiesta utilizzando l'attributo `preload`. Ad esempio:

```html
<video controls preload="none" poster="poster.jpg">
  <source src="video.webm" type="video/webm" />
  <source src="video.mp4" type="video/mp4" />
</video>
```

Dare a `preload` un valore di `none` dice al browser di non precaricare nessun dato video prima che l'utente decida di riprodurlo, il che è ovviamente buono per le prestazioni. Invece, mostrerà solo l'immagine indicata dall'attributo `poster`. Diversi browser hanno comportamenti di caricamento video predefiniti differenti, quindi è bene essere espliciti.

Vedere [Fast playback with audio and video preload](https://web.dev/articles/fast-playback-with-preload) su web.dev per informazioni dettagliate.

## Gestione dei contenuti incorporati

È molto comune che i contenuti vengano incorporati nelle pagine web da altre fonti. Questo è più comunemente fatto quando si visualizzano annunci pubblicitari su un sito per generare entrate — gli annunci sono solitamente generati da una società di terze parti di qualche tipo e incorporati nella sua pagina. Altri usi potrebbero includere:

- Visualizzare contenuti condivisi di cui un utente potrebbe aver bisogno su più pagine, come un carrello della spesa o informazioni del profilo.
- Visualizzare contenuti di terze parti correlati al sito principale dell'organizzazione, come un feed di post sui social media.

L'incorporamento dei contenuti è più comunemente fatto utilizzando elementi {{htmlelement("iframe")}}, sebbene esistano altri elementi di incorporamento meno comunemente usati, come {{htmlelement("object")}} e {{htmlelement("embed")}}. Ci concentreremo sugli `<iframe>` in questa sezione.

Il pezzo di consiglio più importante e fondamentale per l'uso degli `<iframe>` è: "Non usi `<iframe>` incorporati a meno che non ne abbia assolutamente bisogno". Se sta creando una pagina con più pannelli di informazione diversi, potrebbe sembrare sensato suddividerli in pagine separate e caricarli in diversi `<iframe>`. Tuttavia, ciò comporta una serie di problemi in termini di prestazioni e altro:

- Caricare il contenuto in un `<iframe>` è molto più costoso di caricarlo come parte della stessa pagina diretta — non solo richiede richieste HTTP extra per caricare il contenuto, ma il browser deve anche creare un'istanza di pagina separata per ciascuno. Ciascuno è effettivamente un'istanza di pagina web separata incorporata nella pagina web di livello superiore.
- Seguendo dal punto precedente, dovrà anche gestire qualsiasi stile CSS o manipolazione JavaScript separatamente per ciascun `<iframe>` (a meno che le pagine incorporate siano dalla stessa origine), il che diventa molto più complesso. Non può indirizzare il contenuto incorporato con CSS e JavaScript applicati alla pagina di livello superiore, o viceversa. Questo è un sensato misura di sicurezza che è fondamentale per il web. Immagini tutti i problemi in cui potrebbe imbattersi se contenuti incorporati di terze parti potessero eseguire arbitrariamente script contro qualsiasi pagina in cui sono incorporati!
- Ciascun <iframe> dovrà anche caricare qualsiasi dato condiviso e file multimediali separatamente — non può condividere risorse memorizzate nella cache tra diversi incorporamenti di pagina (ancora una volta, a meno che le pagine incorporate siano dalla stessa origine). Questo può portare a un uso di larghezza di banda della pagina molto più grande di quanto si potrebbe aspettare.

È consigliabile mettere il contenuto in una singola pagina. Se vuole aggiungere nuovo contenuto dinamicamente mentre la pagina cambia, è comunque meglio per le prestazioni caricarlo nella stessa pagina piuttosto che metterlo in un <iframe>. Potrebbe recuperare i nuovi dati utilizzando il metodo [`fetch()`](/it/docs/Web/API/Window/fetch), ad esempio, e quindi iniettarli nella pagina usando qualche scripting DOM. Veda [Making network requests with JavaScript](/it/docs/Learn_web_development/Core/Scripting/Network_requests) e [Introduzione allo scripting DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting) per ulteriori informazioni.

> [!NOTE]
> Se controlla il contenuto ed è relativamente semplice, potrebbe considerare di usare contenuto codificato in base-64 nell'attributo `src` per popolare il `<iframe>`, o addirittura inserire HTML grezzo nell'attributo `srcdoc` (Veda [Iframe Performance Part 2: The Good News](https://medium.com/slices-of-bread/iframe-performance-part-2-the-good-news-26eb53cea429) per ulteriori informazioni).

Se deve usare `<iframe>`, lo usi con parsimonia.

### Caricamento differito degli iframes

Così come per gli elementi `<img>`, può anche usare l'attributo `loading` per istruire il browser a caricare su richiesta il contenuto dei `<iframe>` che inizialmente non sono visibili sullo schermo, migliorando quindi le prestazioni:

```html
<iframe src="https://example.com" loading="lazy" width="600" height="400">
</iframe>
```

Veda [It's time to lazy-load offscreen iframes!](https://web.dev/articles/iframe-lazy-loading) per ulteriori informazioni.

## Gestione dell'ordine di caricamento delle risorse

L'ordine di caricamento delle risorse è importante per massimizzare le prestazioni percepite e reali. Quando una pagina web viene caricata:

1. L'HTML viene generalmente analizzato per primo, nell'ordine in cui appare sulla pagina.
2. Eventuali CSS trovati vengono analizzati per comprendere gli stili che devono essere applicati alla pagina. Durante questo tempo, vengono iniziati a recuperare risorse collegate come immagini e font web.
3. Eventuali JavaScript trovati vengono analizzati, valutati ed eseguiti contro la pagina. Per impostazione predefinita, questo blocca l'analisi dell'HTML che appare dopo gli elementi {{htmlelement("script")}} dove viene incontrato il JavaScript.
4. Poco dopo, il browser calcola come ciascun elemento HTML dovrebbe essere stilizzato, dato il CSS applicato ad esso.
5. Il risultato stilizzato viene quindi dipinto sullo schermo.

> [!NOTE]
> Questa è una descrizione molto semplificata di ciò che accade, ma le dà un'idea.

Diverse funzionalità HTML le permettono di modificare come avviene il caricamento delle risorse per migliorare le prestazioni. Esploreremo alcune di queste ora.

### Gestione del caricamento di JavaScript

L'analisi e l'esecuzione di JavaScript blocca l'analisi del contenuto DOM successivo. Questo aumenta il tempo fino a quando quel contenuto è reso e usabile dagli utenti della pagina. Uno script piccolo non farà molta differenza, ma considerare che le applicazioni web moderne tendono ad essere molto dipendenti da JavaScript.

Un altro effetto collaterale del comportamento predefinito del parsing di JavaScript è che, se lo script in corso di rendering si basa sul contenuto DOM che appare più avanti sulla pagina, si verificheranno errori.

Ad esempio, immaginiamo un semplice paragrafo su una pagina:

```html
<p>My paragraph</p>
```

Ora immaginiamo un file JavaScript che contiene il seguente codice:

```js
const pElem = document.querySelector("p");

pElem.addEventListener("click", () => {
  alert("You clicked the paragraph");
});
```

Possiamo applicare questo script alla pagina riferendosi ad essa in un elemento `<script>` in questo modo:

```html
<script src="index.js"></script>
```

Se mettiamo questo elemento `<script>` prima dell'elemento `<p>` nell'ordine delle sorgenti (ad esempio, nell'elemento {{htmlelement("head")}}), la pagina lancerà un errore (Chrome, ad esempio, rsporta "Uncaught TypeError: Cannot read properties of null (reading 'addEventListener')" nella console). Questo avviene perché lo script si basa sull'elemento `<p>` per funzionare, ma al punto in cui lo script viene analizzato, l'elemento `<p>` non esiste sulla pagina. Non è ancora stato reso.

Può risolvere il problema sopra mettendo l'elemento `<script>` dopo l'elemento `<p>` (ad esempio, alla fine del corpo del documento), o eseguendo il codice all'interno di un gestore di eventi idoneo (ad esempio, eseguirlo sul [`DOMContentLoaded`](/it/docs/Web/API/Document/DOMContentLoaded_event) che viene attivato quando il DOM è stato completamente analizzato).

Tuttavia, questo non risolve il problema dell'attesa per il caricamento dello script. Migliori prestazioni possono essere raggiunte aggiungendo l'attributo `async` all'elemento `<script>`:

```html
<script async src="index.js"></script>
```

Questo causa il recupero dello script in parallelo con l'analisi del DOM, quindi è pronto nello stesso momento e non bloccherà il rendering, migliorando così le prestazioni.

> [!NOTE]
> C'è un altro attributo, `defer`, che causa l'esecuzione dello script dopo che il documento è stato analizzato, ma prima di avviare `DOMContentLoaded`. Questo ha un effetto simile a `async`.

Un altro suggerimento per la gestione del caricamento di JavaScript è dividere il proprio script in moduli di codice e caricare ciascuna parte quando richiesto, invece di concentrare tutto il proprio codice in un unico grande script e caricarlo tutto all'inizio. Questo viene fatto utilizzando [moduli JavaScript](/it/docs/Web/JavaScript/Guide/Modules). Legga l'articolo collegato per una guida dettagliata.

### Caricamento anticipato del contenuto con rel="preload"

Il recupero di altre risorse (come immagini, video o file di font) collegati dal suo HTML, CSS e JavaScript può anche causare problemi di prestazioni, bloccando l'esecuzione del suo codice e rallentando l'esperienza. Un modo per mitigare tali problemi è utilizzare `rel="preload"` per trasformare gli elementi {{htmlelement("link")}} in caricamenti anticipati. Ad esempio:

```html
<link rel="preload" href="sintel-short.mp4" as="video" type="video/mp4" />
```

Quando si imbatte in un link con `rel="preload"`, il browser recupera la risorsa referenziata il prima possibile e la rende disponibile nella cache del browser in modo che sarà pronta per l'uso presto quando viene referenziata nel codice successivo. È utile precaricare risorse di alta priorità che l'utente incontrerà presto in una pagina affinché l'esperienza sia il più possibile fluida.

Veda i seguenti articoli per informazioni dettagliate sull'uso di `rel="preload"`:

- [`rel="preload"`](/it/docs/Web/HTML/Reference/Attributes/rel/preload)
- [Preload critical assets to improve loading speed](https://web.dev/articles/preload-critical-assets) su web.dev (2020)

> [!NOTE]
> Può usare `rel="preload"` per precaricare anche file CSS e JavaScript.

> [!NOTE]
> Ci sono altri valori di [`rel`](/it/docs/Web/HTML/Reference/Attributes/rel) progettati per velocizzare vari aspetti del caricamento delle pagine: `dns-prefetch`, `preconnect`, `modulepreload`, `prefetch` e `prerender`. Visiti la pagina collegata e scopra cosa fanno.

## Vedi anche

- [Making network requests with JavaScript](/it/docs/Learn_web_development/Core/Scripting/Network_requests)
- [Introduzione allo scripting DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting)

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/Javascript", "Learn_web_development/Extensions/Performance/CSS", "Learn_web_development/Extensions/Performance")}}
