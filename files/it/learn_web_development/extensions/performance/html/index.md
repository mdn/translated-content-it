---
title: Ottimizzazione delle prestazioni HTML
short-title: HTML performante
slug: Learn_web_development/Extensions/Performance/HTML
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/Javascript", "Learn_web_development/Extensions/Performance/CSS", "Learn_web_development/Extensions/Performance")}}

HTML è per impostazione predefinita veloce e accessibile. È compito nostro, come sviluppatori, assicurarci di preservare queste due proprietà quando creiamo o modifichiamo codice HTML. Le complicazioni possono insorgere quando, ad esempio, la dimensione del file di un incorporamento di un {{htmlelement("video")}} è troppo grande o quando il parsing di JavaScript blocca la visualizzazione degli elementi critici della pagina. Questo articolo ti guida attraverso le principali funzionalità di prestazioni HTML che possono migliorare drasticamente la qualità della tua pagina web.

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
        e come ottimizzare il tuo HTML per migliorare le prestazioni.
      </td>
    </tr>
  </tbody>
</table>

## Ottimizzare o non ottimizzare

La prima domanda che dovresti porti prima di iniziare a ottimizzare il tuo HTML è "cosa devo ottimizzare?". Alcuni dei suggerimenti e delle tecniche discussi di seguito sono buone pratiche che porteranno beneficio a quasi tutti i progetti web, mentre altri sono necessari solo in determinate situazioni. Tentare di applicare tutte queste tecniche ovunque potrebbe non essere necessario e potrebbe rappresentare una perdita di tempo. Dovresti capire quali ottimizzazioni delle prestazioni sono effettivamente necessarie in ogni progetto.

Per fare ciò, devi [misurare le prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance) del tuo sito. Come mostrato da questo link, ci sono diversi modi per misurare le prestazioni, alcuni dei quali coinvolgono sofisticate [API di prestazioni](/it/docs/Web/API/Performance_API). Tuttavia, il modo migliore per iniziare è imparare a usare strumenti come i tool di [rete](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#network_monitor_tools) integrati nel browser e quelli di [performance](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#performance_monitor_tools), per esaminare le parti della pagina che impiegano molto tempo a caricarsi e necessitano di ottimizzazione.

## Problemi chiave delle prestazioni HTML

HTML è semplice in termini di prestazioni — è principalmente testo, che è di piccole dimensioni, e, quindi, generalmente veloce da scaricare e renderizzare. I problemi chiave che possono influenzare le prestazioni di una pagina web includono:

- Dimensione dei file di immagini e video: È importante considerare come gestire il contenuto degli elementi sostitutivi come {{htmlelement("img")}} e {{htmlelement("video")}}. I file di immagini e video sono grandi e possono aggiungere significativamente peso alla pagina. Pertanto, è importante minimizzare il numero di byte che vengono scaricati su un dispositivo dell'utente (ad esempio, servire immagini più piccole per i dispositivi mobili). È inoltre necessario considerare il miglioramento delle prestazioni percepite caricando immagini e video su una pagina solo quando sono necessari.
- Distribuzione dei contenuti incorporati: Solitamente si tratta di contenuti integrati in elementi {{htmlelement("iframe")}}. Caricare contenuti in `<iframe>` può influenzare significativamente le prestazioni, quindi dovrebbe essere considerato con attenzione.
- Ordine di caricamento delle risorse: Per massimizzare le prestazioni percepite e reali, l'HTML dovrebbe essere caricato per primo, nell'ordine in cui appare sulla pagina. Puoi quindi utilizzare varie funzionalità per influenzare l'ordine di caricamento delle risorse per prestazioni migliori. Ad esempio, puoi pre-caricare CSS critico e font in anticipo, ma rimandare JavaScript non critico a un momento successivo.

> [!NOTE]
> C'è un argomento da fare per semplificare la tua struttura HTML e [minificare](<https://en.wikipedia.org/wiki/Minification_(programming)>) il tuo codice sorgente, in modo che rendering e download siano più veloci. Tuttavia, la dimensione del file HTML è trascurabile rispetto a immagini e video, e il rendering del browser è molto veloce al giorno d'oggi. Se il tuo sorgente HTML è così grande e complesso da creare problemi di performance nel rendering e nel download, probabilmente hai problemi più grandi e dovresti mirare a semplificarlo e suddividere il contenuto.

## Gestione responsiva degli elementi sostitutivi

[Il design responsivo](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design) ha rivoluzionato il modo in cui il layout dei contenuti web viene gestito su diversi dispositivi. Un vantaggio chiave che abilita è il passaggio dinamico di layout ottimizzati per diverse dimensioni di schermo, ad esempio un layout a schermo largo rispetto a un layout a schermo stretto (mobile). Può anche gestire il passaggio dinamico di contenuti in base ad altri attributi del dispositivo, come la risoluzione o la preferenza per uno schema cromatico chiaro o scuro.

La cosiddetta tecnica "mobile first" può garantire che il layout predefinito sia per dispositivi con schermi piccoli, quindi i dispositivi mobili possono semplicemente scaricare le immagini adatte ai loro schermi, e non devono subire l'impatto delle prestazioni delle immagini più grandi per desktop. Tuttavia, poiché ciò è controllato utilizzando [media query](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries) nel tuo CSS, può influenzare positivamente le prestazioni solo delle immagini caricate nel CSS.

Nelle sezioni sottostanti, riassumeremo come implementare elementi sostitutivi responsivi. Puoi trovare molti più dettagli su queste implementazioni nelle guide [HTML video e audio](/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio) e [Immagini responsive](/it/docs/Web/HTML/Guides/Responsive_images).

### Fornire diverse risoluzioni di immagini tramite srcset

Per fornire diverse versioni di risoluzione della stessa immagine a seconda della risoluzione del dispositivo e della dimensione della finestra di visualizzazione, puoi utilizzare gli attributi [`srcset`](/it/docs/Web/HTML/Reference/Elements/img#srcset) e [`sizes`](/it/docs/Web/HTML/Reference/Elements/img#sizes).

Questo esempio fornisce immagini di diverse dimensioni per diverse larghezze di schermo:

```html
<img
  srcset="480w.jpg 480w, 800w.jpg 800w"
  sizes="(max-width: 600px) 480px,
         800px"
  src="800w.jpg"
  alt="Family portrait" />
```

`srcset` fornisce la dimensione intrinseca delle immagini sorgente insieme ai loro nomi di file, e `sizes` fornisce media query insieme alle larghezze degli slot di immagini che devono essere riempiti in ciascun caso. Il browser decide quindi quali immagini ha senso caricare per ciascun slot. Ad esempio, se la larghezza dello schermo è `600px` o meno, allora `max-width: 600px` è vero, e quindi si dice che lo slot da riempire sia `480px`. In questo caso, il browser probabilmente sceglierà di caricare il file 480w.jpg (immagine larga 480px). Questo aiuta con le prestazioni perché i browser non caricano immagini più grandi di quelle necessarie.

Questo esempio fornisce immagini di risoluzione diversa per risoluzioni di schermo diverse:

```html
<img
  srcset="320w.jpg, 480w.jpg 1.5x, 640w.jpg 2x"
  src="640w.jpg"
  alt="Family portrait" />
```

`1.5x`, `2x`, ecc. sono indicatori di risoluzione relativa. Se l'immagine è stilizzata per essere larga 320px (ad esempio con `width: 320px` in CSS), il browser caricherà `320w.jpg` se il dispositivo ha una bassa risoluzione (un {{Glossary("device_pixel", "pixel di dispositivo")}} per pixel CSS), o `640x.jpg` se il dispositivo è ad alta risoluzione (due pixel di dispositivo per pixel CSS o più).

In entrambi i casi, l'attributo `src` fornisce un'immagine predefinita da caricare se il browser non supporta `src`/`srcset`.

### Fornire diverse sorgenti per immagini e video

L'elemento {{htmlelement("picture")}} si basa sull'elemento tradizionale {{htmlelement("img")}}, permettendoti di fornire più sorgenti diverse per diverse situazioni. Ad esempio, se il layout è ampio, probabilmente vorrai un'immagine larga, e se è stretto, vorrai un'immagine più stretta che funzioni comunque in quel contesto.

Ovviamente, questo funziona anche per fornire un download di informazioni più piccolo sui dispositivi mobili, aiutando con le prestazioni.

Un esempio è il seguente:

```html
<picture>
  <source media="(max-width: 799px)" srcset="narrow-banner-480w.jpg" />
  <source media="(min-width: 800px)" srcset="wide-banner-800w.jpg" />
  <img src="large-banner-800w.jpg" alt="Dense forest scene" />
</picture>
```

Gli elementi {{htmlelement("source")}} contengono media query dentro attributi `media`. Se una media query restituisce vero, l'immagine referenziata nell'attributo `srcset` dell'elemento `<source>` viene caricata. Nell'esempio sopra, se la larghezza della finestra è `799px` o meno, l'immagine `narrow-banner-480w.jpg` viene caricata. Nota anche come l'elemento `<picture>` includa un elemento `<img>`, che fornisce un'immagine predefinita da caricare nel caso di browser che non supportano `<picture>`.

Nota l'uso dell'attributo `srcset` in questo esempio. Come mostrato nella sezione precedente, puoi fornire diverse risoluzioni per ciascuna sorgente di immagine.

Gli elementi `<video>` funzionano in modo simile, in termini di fornitura di diverse sorgenti:

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

Ci sono, tuttavia, alcune differenze chiave tra la fornitura di sorgenti per immagini e video:

- Nell'esempio sopra, stiamo usando `src` piuttosto che `srcset`; non puoi specificare risoluzioni diverse per i video tramite `srcset`.
- Invece, specifichi diverse risoluzioni all'interno dei diversi elementi `<source>`.
- Nota come stiamo anche specificando diversi formati video all'interno dei diversi elementi `<source>`, con ogni formato identificato tramite il suo tipo MIME all'interno dell'attributo `type`. I browser caricheranno il primo che trovano che supportano, dove il test della media query restituisce vero.

### Caricamento pigro delle immagini

Una tecnica molto utile per migliorare le prestazioni è il **caricamento pigro**. Questo si riferisce alla pratica di non caricare tutte le immagini immediatamente quando l'HTML viene renderizzato, ma di caricarle solo quando sono effettivamente visibili all'utente nella finestra di visualizzazione (o imminentemente visibili). Ciò significa che il contenuto immediatamente visibile/usabile è pronto per l'uso più rapidamente, mentre il contenuto successivo ha le sue immagini renderizzate solo quando viene scrollato, e il browser non sprecherà banda caricando immagini che l'utente non vedrà mai.

Il caricamento pigro è stato storicamente gestito utilizzando JavaScript, ma i browser ora hanno un attributo `loading` disponibile che può istruire il browser a caricare automaticamente le immagini in modalità pigra:

```html
<img src="800w.jpg" alt="Family portrait" loading="lazy" />
```

Vedi [Browser-level image lazy loading for the web](https://web.dev/articles/browser-level-image-lazy-loading) su web.dev per informazioni dettagliate.

Puoi anche caricare in modalità pigra il contenuto video utilizzando l'attributo `preload`. Ad esempio:

```html
<video controls preload="none" poster="poster.jpg">
  <source src="video.webm" type="video/webm" />
  <source src="video.mp4" type="video/mp4" />
</video>
```

Dare a `preload` un valore di `none` dice al browser di non pre-caricare nessuno dei dati video prima che l'utente decida di riprodurlo, il che è ovviamente positivo per le prestazioni. Invece, mostrerà solo l'immagine indicata dall'attributo `poster`. I diversi browser hanno comportamenti predefiniti diversi per il caricamento dei video, quindi è bene essere espliciti.

Vedi [Fast playback with audio and video preload](https://web.dev/articles/fast-playback-with-preload) su web.dev per informazioni dettagliate.

## Gestione dei contenuti incorporati

È molto comune che i contenuti vengano incorporati nelle pagine web da altre fonti. Questo avviene più comunemente quando si mostrano annunci pubblicitari su un sito per generare entrate — gli annunci sono solitamente generati da un'azienda di terze parti di qualche tipo e incorporati nella tua pagina. Altri usi potrebbero includere:

- Mostrare contenuti condivisi che un utente potrebbe aver bisogno su più pagine, come un carrello della spesa o informazioni sul profilo.
- Mostrare contenuti di terze parti relativi al sito principale dell'organizzazione, come un feed di post di social media.

L'incorporamento di contenuti viene fatto più comunemente utilizzando elementi {{htmlelement("iframe")}}, anche se esistono altri elementi di incorporamento meno comunemente usati, come {{htmlelement("object")}} e {{htmlelement("embed")}}. Ci concentreremo sugli `<iframe>` in questa sezione.

Il pezzo di consiglio più importante e chiave per l'uso degli `<iframe>` è: "Non usare `<iframe>` incorporati a meno che non sia assolutamente necessario". Se stai creando una pagina con più diversi riquadri di informazioni su di essa, potrebbe sembrare sensato dal punto di vista organizzativo suddividerli in pagine separate e caricarli in diversi `<iframe>`. Tuttavia, questo ha una serie di problemi associati in termini di prestazioni e altrimenti:

- Caricare il contenuto in un `<iframe>` è molto più costoso che caricare il contenuto come parte della stessa pagina diretta — non solo richiede richieste HTTP extra per caricare il contenuto, ma il browser deve anche creare un'istanza di pagina separata per ciascuno di essi. Ciascuno di essi è effettivamente un'istanza separata di pagina web incorporata nella pagina web principale.
- Seguendo dal punto precedente, dovrai gestire qualsiasi styling CSS o manipolazione JavaScript separatamente per ciascun diverso `<iframe>` (a meno che le pagine incorporate siano dallo stesso dominio), il che diventa molto più complesso. Non puoi targetizzare contenuti incorporati con CSS e JavaScript applicati alla pagina principale, o viceversa. Questa è una misura di sicurezza sensata che è fondamentale per il web. Immagina tutti i problemi in cui potresti incorrere se contenuti incorporati di terze parti potessero arbitrariamente eseguire script contro qualsiasi pagina in cui erano incorporati!
- Ciascun `<iframe>` dovrebbe anche caricare separatamente qualsiasi dato condiviso e file multimediali — non puoi condividere asset cache tra diversi incorporamenti di pagina (ancora una volta, a meno che le pagine incorporate siano dallo stesso dominio). Ciò può portare a una pagina che utilizza molta più banda di quanto potresti aspettarti.

È consigliabile mettere il contenuto in un'unica pagina. Se vuoi caricare dinamicamente nuovi contenuti man mano che la pagina cambia, è ancora meglio per le prestazioni caricarlo nella stessa pagina piuttosto che metterlo in un `<iframe>`. Potresti ottenere i nuovi dati usando il metodo [`fetch()`](/it/docs/Web/API/Window/fetch), per esempio, e quindi iniettarli nella pagina usando un po' di scripting DOM. Vedi [Richieste di rete con JavaScript](/it/docs/Learn_web_development/Core/Scripting/Network_requests) e [Introduzione al scripting DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting) per maggiori informazioni.

> [!NOTE]
> Se controlli il contenuto ed è relativamente semplice, potresti considerare di usare contenuti codificati in base-64 nell'attributo `src` per popolare l'`<iframe>`, o persino inserire HTML grezzo nell'attributo `srcdoc` (Vedi [Iframe Performance Part 2: The Good News](https://medium.com/slices-of-bread/iframe-performance-part-2-the-good-news-26eb53cea429) per maggiori informazioni).

Se devi usare `<iframe>`, allora usali con moderazione.

### Caricamento pigro degli iframe

Nello stesso modo degli elementi `<img>`, puoi anche usare l'attributo `loading` per istruire il browser a caricare in modalità pigra contenuto `<iframe>` che è inizialmente fuori dallo schermo, migliorando così le prestazioni:

```html
<iframe src="https://example.com" loading="lazy" width="600" height="400">
</iframe>
```

Vedi [It's time to lazy-load offscreen iframes!](https://web.dev/articles/iframe-lazy-loading) per ulteriori informazioni.

## Gestione dell'ordine di caricamento delle risorse

L'ordinamento del caricamento delle risorse è importante per massimizzare le prestazioni percepite e reali. Quando una pagina web viene caricata:

1. L'HTML viene generalmente analizzato per primo, nell'ordine in cui appare sulla pagina.
2. Qualsiasi CSS trovato viene analizzato per comprendere gli stili che devono essere applicati alla pagina. Durante questo tempo, le risorse collegate come immagini e font web iniziano a essere recuperate.
3. Qualsiasi JavaScript trovato viene analizzato, valutato ed eseguito contro la pagina. Per impostazione predefinita, questo blocca l'analisi dell'HTML che appare dopo gli elementi {{htmlelement("script")}} dove si incontra il JavaScript.
4. Poco più tardi, il browser determina come ciascun elemento HTML debba essere stilizzato, data l'applicazione del CSS.
5. Il risultato stilizzato viene quindi dipinto sullo schermo.

> [!NOTE]
> Questa è una descrizione molto semplificata di ciò che accade, ma ti offre un'idea.

Diverse funzionalità HTML ti permettono di modificare come avviene il caricamento delle risorse per migliorare le prestazioni. Esploreremo alcune di queste ora.

### Gestione del caricamento di JavaScript

L'analisi e l'esecuzione di JavaScript blocca l'analisi del contenuto DOM successivo. Questo aumenta il tempo fino a quando quel contenuto è reso e utilizzabile dagli utenti della pagina. Uno script di piccole dimensioni non farà molta differenza, ma considera che le applicazioni web moderne tendono a essere molto pesanti di JavaScript.

Un altro effetto collaterale del comportamento predefinito dell'analisi di JavaScript è che, se lo script in fase di rendering si basa su contenuti DOM che appaiono più avanti nella pagina, otterrai errori.

Ad esempio, immagina un semplice paragrafo su una pagina:

```html
<p>My paragraph</p>
```

Ora immagina un file JavaScript che contiene il seguente codice:

```js
const pElem = document.querySelector("p");

pElem.addEventListener("click", () => {
  alert("You clicked the paragraph");
});
```

Possiamo applicare questo script alla pagina riferendolo in un elemento `<script>` in questo modo:

```html
<script src="index.js"></script>
```

Se mettiamo questo elemento `<script>` prima dell'elemento `<p>` nell'ordine sorgente (ad esempio, nell'elemento {{htmlelement("head")}}), la pagina lancerà un errore (Chrome ad esempio riporta "Uncaught TypeError: Cannot read properties of null (reading 'addEventListener')" nella console). Questo avviene perché lo script si basa sull'elemento `<p>` per funzionare, ma al momento in cui lo script viene analizzato, l'elemento `<p>` non esiste ancora sulla pagina. Non è stato ancora renderizzato.

Puoi risolvere il problema precedente mettendo l'elemento `<script>` dopo l'elemento `<p>` (ad esempio alla fine del corpo del documento), o eseguendo il codice all'interno di un gestore di eventi adatto (ad esempio eseguendolo su [`DOMContentLoaded`](/it/docs/Web/API/Document/DOMContentLoaded_event), che viene attivato quando il DOM è stato completamente analizzato).

Tuttavia, questo non risolve il problema dell'attesa per il caricamento dello script. Le prestazioni possono essere migliorate aggiungendo l'attributo `async` all'elemento `<script>`:

```html
<script async src="index.js"></script>
```

Questo provoca il fetch dello script in parallelo con l'analisi del DOM, quindi è pronto allo stesso tempo, e non blocca il rendering, migliorando così le prestazioni.

> [!NOTE]
> Esiste un altro attributo, `defer`, che fa sì che lo script venga eseguito dopo che il documento è stato analizzato, ma prima del trigger di `DOMContentLoaded`. Questo ha un effetto simile a `async`.

Un altro suggerimento per la gestione del caricamento di JavaScript è dividere il tuo script in moduli di codice e caricare ciascuna parte quando richiesto, invece di mettere tutto il tuo codice in un unico grande script e caricarlo tutto all'inizio. Questo viene fatto usando [moduli JavaScript](/it/docs/Web/JavaScript/Guide/Modules). Leggi l'articolo collegato per una guida dettagliata.

### Pre-caricare contenuti con rel="preload"

Il recupero di altre risorse (come immagini, video o file font) collegati dal tuo HTML, CSS e JavaScript può anche causare problemi di prestazioni, bloccando l'esecuzione del tuo codice e rallentando l'esperienza. Un modo per mitigare tali problemi è usare `rel="preload"` per trasformare gli elementi {{htmlelement("link")}} in pre-caricatori. Ad esempio:

```html
<link rel="preload" href="sintel-short.mp4" as="video" type="video/mp4" />
```

Quando incontra un link `rel="preload"`, il browser recupererà la risorsa referenziata il prima possibile e la renderà disponibile nella cache del browser in modo che sia pronta per l'uso al più presto quando viene referenziata nel codice successivo. È utile pre-caricare risorse di alta priorità che l'utente incontrerà all'inizio di una pagina in modo che l'esperienza sia il più fluida possibile.

Consulta i seguenti articoli per informazioni dettagliate sull'uso di `rel="preload"`:

- [`rel="preload"`](/it/docs/Web/HTML/Reference/Attributes/rel/preload)
- [Preload critical assets to improve loading speed](https://web.dev/articles/preload-critical-assets) su web.dev (2020)

> [!NOTE]
> Puoi usare `rel="preload"` per pre-caricare file CSS e JavaScript anche.

> [!NOTE]
> Ci sono altri valori [`rel`](/it/docs/Web/HTML/Reference/Attributes/rel) che sono anche progettati per velocizzare vari aspetti del caricamento delle pagine: `dns-prefetch`, `preconnect`, `modulepreload`, `prefetch` e `prerender`. Vai alla pagina collegata e scopri cosa fanno.

## Vedi anche

- [Richieste di rete con JavaScript](/it/docs/Learn_web_development/Core/Scripting/Network_requests)
- [Introduzione al scripting DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting)

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/Javascript", "Learn_web_development/Extensions/Performance/CSS", "Learn_web_development/Extensions/Performance")}}
