---
title: "Multimedia: immagini"
slug: Learn_web_development/Extensions/Performance/Multimedia
l10n:
  sourceCommit: 8db892b3e7ca294621898441e7db2481e0e6d939
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/Measuring_performance", "Learn_web_development/Extensions/Performance/video", "Learn_web_development/Extensions/Performance")}}

I media, ossia immagini e video, rappresentano oltre il 70% dei byte scaricati dal sito web medio. In termini di prestazioni di download, eliminare i media e ridurre le dimensioni dei file è l'intervento più semplice. Questo articolo esamina l'ottimizzazione di immagini e video per migliorare le prestazioni web.

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
        Apprendere i vari formati di immagine, il loro impatto sulle prestazioni
        e come ridurre l'impatto delle immagini sul tempo complessivo di caricamento della pagina.
      </td>
    </tr>
  </tbody>
</table>

> [!NOTE]
> Questa è un'introduzione generale all'ottimizzazione della distribuzione di contenuti multimediali sul web, che copre principi e tecniche generali. Per una guida più approfondita, vedere <https://web.dev/learn/images>.

## Perché ottimizzare i contenuti multimediali?

Per il sito web medio, [il 51% della larghezza di banda deriva dalle immagini, seguito dai video con il 25%](https://discuss.httparchive.org/t/state-of-the-web-top-image-optimization-strategies/1367), quindi è chiaramente importante affrontare e ottimizzare i contenuti multimediali.

È necessario considerare l'utilizzo dei dati. Molte persone dispongono di piani dati con limite o persino di piani a consumo, nei quali si paga letteralmente per megabyte. Questo non è un problema limitato ai mercati emergenti. Nel 2018, il 24% del Regno Unito utilizzava ancora piani a consumo secondo [OFCOM Nations & regions technology tracker - H1 2018 (PDF)](https://www.ofcom.org.uk/siteassets/resources/documents/research-and-data/technology-research/technology-tracker/technology-tracker-h1-2018-data-tables?v=323142).

È inoltre necessario considerare la memoria, poiché molti dispositivi mobili dispongono di RAM limitata. È importante ricordare che, quando le immagini vengono scaricate, devono essere archiviate in memoria.

## Ottimizzare la distribuzione delle immagini

Nonostante siano il principale consumatore di larghezza di banda, l'impatto del download delle immagini sulle [prestazioni percepite](/it/docs/Learn_web_development/Extensions/Performance/Perceived_performance) è molto inferiore a quanto molti si aspettino, principalmente perché il contenuto testuale della pagina viene scaricato immediatamente e gli utenti possono vedere le immagini mentre vengono renderizzate. Tuttavia, per una buona esperienza utente è comunque importante che un visitatore possa visualizzarle il prima possibile.

### Strategia di caricamento

Uno dei maggiori miglioramenti applicabili alla maggior parte dei siti web consiste nel [caricamento differito](/it/docs/Web/Performance/Guides/Lazy_loading) delle immagini sotto la piega, invece di scaricarle tutte al caricamento iniziale della pagina indipendentemente dal fatto che un visitatore scorra o meno fino a visualizzarle. I browser forniscono questa funzionalità nativamente tramite l'attributo [`loading="lazy"`](/it/docs/Web/HTML/Reference/Elements/img#loading) sugli elementi `<img>`, `<iframe>`, `<video>` e `<audio>`; esistono inoltre molte librerie JavaScript lato client che possono svolgere questa funzione.

Oltre a caricare solo un sottoinsieme di immagini, è opportuno considerare il formato delle immagini stesse:

- Vengono caricati i formati di file più ottimali?
- Le immagini sono state compresse correttamente?
- Vengono caricate le dimensioni corrette?

#### Il formato più ottimale

Il formato di file ottimale dipende in genere dalle caratteristiche dell'immagine.

> [!NOTE]
> Per informazioni generali sui tipi di immagini, vedere la [Guida ai tipi e ai formati di file immagine](/it/docs/Web/Media/Guides/Formats/Image_types).

Il formato [SVG](/it/docs/Web/Media/Guides/Formats/Image_types#svg_scalable_vector_graphics) è più adatto per immagini con pochi colori e non fotorealistiche. Ciò richiede che la sorgente sia disponibile in un formato di grafica vettoriale. Se un'immagine di questo tipo esiste solo come bitmap, allora [PNG](/it/docs/Web/Media/Guides/Formats/Image_types#png_portable_network_graphics) sarebbe il formato di fallback da scegliere. Esempi di questi tipi di motivi sono loghi, illustrazioni, grafici o icone (nota: gli SVG sono di gran lunga migliori dei font di icone). Entrambi i formati supportano la trasparenza.

I PNG possono essere salvati con tre diverse combinazioni di output:

- Colore a 24 bit + trasparenza a 8 bit: offrono accuratezza cromatica completa e trasparenza uniforme a scapito delle dimensioni. Probabilmente è preferibile evitare questa combinazione a favore di WebP (vedere sotto).
- Colore a 8 bit + trasparenza a 8 bit: offrono non più di 255 colori, ma mantengono trasparenze uniformi. Le dimensioni non sono eccessive. Sono probabilmente i PNG da preferire.
- Colore a 8 bit + trasparenza a 1 bit: offrono non più di 255 colori e solo assenza o trasparenza completa per pixel, facendo apparire i bordi della trasparenza netti e frastagliati. Le dimensioni sono ridotte, ma a scapito della fedeltà visiva.

Un buon strumento online per ottimizzare gli SVG è [SVGOMG](https://jakearchibald.github.io/svgomg/). Per i PNG sono disponibili [ImageOptim online](https://imageoptim.com/online) o [Squoosh](https://squoosh.app/).

Per soggetti fotografici privi di trasparenza, esiste una gamma molto più ampia di formati tra cui scegliere. Per una scelta sicura, si possono usare **JPEG progressivi** ben compressi. I JPEG progressivi, a differenza dei JPEG normali, vengono renderizzati progressivamente, da cui il nome: l'utente vede una versione a bassa risoluzione che diventa più nitida man mano che l'immagine viene scaricata, invece dell'immagine che si carica a piena risoluzione dall'alto verso il basso o che viene renderizzata solo una volta completato il download. Un buon compressore è MozJPEG, disponibile ad esempio nello strumento online di ottimizzazione delle immagini [Squoosh](https://squoosh.app/). Un'impostazione della qualità del 75% dovrebbe produrre risultati soddisfacenti.

Altri formati migliorano le capacità di compressione di JPEG, ma non sono disponibili in tutti i browser:

- [WebP](/it/docs/Web/Media/Guides/Formats/Image_types#webp_image): scelta eccellente sia per immagini sia per immagini animate. WebP offre una compressione molto migliore rispetto a PNG o JPEG, con supporto per profondità di colore maggiori, frame animati, trasparenza e altro ancora, ma non per la visualizzazione progressiva. È supportato da tutti i principali browser, tranne Safari 14 su macOS desktop Big Sur o versioni precedenti.

  > [!NOTE]
  > Nonostante Apple [abbia annunciato il supporto per WebP in Safari 14](https://developer.apple.com/videos/play/wwdc2020/10663/?time=1174), le versioni di Safari precedenti alla 16.0 non visualizzano correttamente le immagini `.webp` nelle versioni desktop di macOS precedenti alla 11/Big Sur. Safari per iOS 14 visualizza invece correttamente le immagini `.webp`.

- [AVIF](/it/docs/Web/Media/Guides/Formats/Image_types#avif_image): buona scelta sia per immagini sia per immagini animate grazie alle alte prestazioni e al formato di immagine privo di royalty; è persino più efficiente di WebP, ma meno ampiamente supportato. Ora è supportato da Chrome, Edge, Opera e Firefox. [Squoosh](https://squoosh.app/) è un buon strumento online per convertire formati di immagine precedenti in AVIF.
- **JPEG2000**: un tempo candidato a successore di JPEG, ma supportato solo in Safari. Non supporta neppure la visualizzazione progressiva.

Dato il supporto limitato per JPEG-XR e JPEG2000, e considerando anche i costi di decodifica, l'unico serio concorrente di JPEG è WebP. Per questo motivo, è possibile offrire le immagini anche in questo formato. Questo può essere fatto tramite l'elemento `<picture>`, con l'aiuto di un elemento `<source>` dotato di un [attributo `type`](/it/docs/Web/HTML/Reference/Elements/picture#the_type_attribute).

Se tutto ciò sembra un po' complicato o richiede troppo lavoro per il team, sono disponibili anche servizi online utilizzabili come CDN per immagini, che automatizzano al volo la distribuzione del formato di immagine corretto in base al tipo di dispositivo o browser che richiede l'immagine. Tra le scelte più diffuse figurano [Cloudinary](https://cloudinary.com/blog/make_all_images_on_your_website_responsive_in_3_easy_steps), [Image Engine](https://imageengine.io/), [ImageKit](https://imagekit.io/docs/image-optimization#automatic-format-conversion) e [imgix](https://www.imgix.com/).

Infine, se si desidera includere immagini animate nella pagina, è utile sapere che Safari consente l'uso di file video all'interno degli elementi `<img>` e `<picture>`. Questi consentono inoltre di aggiungere un **WebP animato** per tutti gli altri browser moderni.

```html
<picture>
  <source type="video/mp4" src="giphy.mp4" />
  <source type="image/webp" src="giphy.webp" />
  <img src="giphy.gif" alt="A GIF animation" />
</picture>
```

#### Distribuire le dimensioni ottimali

Nella distribuzione delle immagini, l'approccio "una dimensione per tutti" non produce i risultati migliori: per schermi più piccoli è preferibile distribuire immagini a risoluzione inferiore e viceversa per schermi più grandi. Inoltre, è opportuno distribuire immagini a risoluzione più elevata ai dispositivi dotati di schermo ad alto DPI, ad esempio "Retina". Oltre a creare molte varianti intermedie delle immagini, è necessario anche un modo per distribuire il file corretto al browser corretto. A questo scopo, è necessario integrare gli elementi `<picture>` e `<source>` con gli attributi [`media`](/it/docs/Web/HTML/Reference/Elements/source#media) e/o [`sizes`](/it/docs/Web/HTML/Reference/Elements/source#sizes). [Responsive images done right: A guide to `<picture>` and `srcset`](https://www.smashingmagazine.com/2014/05/responsive-images-done-right-guide-picture-srcset/) spiega dettagliatamente come combinare tutti questi attributi.

Due effetti interessanti da tenere presenti riguardo agli schermi ad alto DPI sono:

- Con uno schermo ad alto DPI, gli esseri umani noteranno gli artefatti di compressione molto più tardi, il che significa che per immagini destinate a questi schermi è possibile aumentare la compressione oltre il livello abituale.
- [Solo pochissime persone riescono a notare un aumento della risoluzione oltre 2× DPI](https://observablehq.com/@eeeps/visual-acuity-and-device-pixel-ratio), quindi non è necessario distribuire immagini con risoluzione superiore a 2×.

#### Controllare la priorità e l'ordine di download delle immagini

Far arrivare ai visitatori le immagini più importanti prima di quelle meno importanti può migliorare le prestazioni percepite.

La prima cosa da verificare è che le immagini dei contenuti usino elementi `<img>` o `<picture>` e che le immagini di sfondo siano definite in CSS con `background-image`: alle immagini referenziate negli elementi `<img>` viene assegnata una priorità di caricamento maggiore rispetto alle immagini di sfondo.

In secondo luogo, con l'adozione di Priority Hints, è possibile controllare ulteriormente la priorità aggiungendo un attributo `fetchPriority` ai tag immagine. Un caso d'uso di Priority Hints per le immagini sono i caroselli, nei quali la prima immagine ha una priorità maggiore rispetto alle immagini successive.

### Strategia di rendering: evitare jank durante il caricamento delle immagini

Poiché le immagini vengono caricate in modo asincrono e continuano a caricarsi dopo il primo paint, se le loro dimensioni non sono definite prima del caricamento possono causare reflow nel contenuto della pagina. Ad esempio, quando il testo viene spinto più in basso nella pagina dalle immagini in fase di caricamento. Per questo motivo, è importante impostare gli attributi `width` e `height` affinché il browser possa riservare loro spazio nel layout.

Quando gli attributi `width` e `height` di un'immagine sono inclusi in un elemento HTML {{htmlelement("img")}}, il [rapporto d'aspetto dell'immagine](/it/docs/Web/CSS/Guides/Box_sizing/Aspect_ratios#adjusting_aspect_ratios_of_replaced_elements) può essere calcolato dal browser prima che l'immagine venga caricata. Questo {{Glossary("aspect_ratio", "rapporto d'aspetto")}} viene usato per riservare lo spazio necessario a visualizzare l'immagine, riducendo o persino evitando uno spostamento del layout quando l'immagine viene scaricata e dipinta sullo schermo. La riduzione degli spostamenti del layout è una componente principale di una buona esperienza utente e delle prestazioni web.

I browser iniziano a renderizzare il contenuto durante il parsing dell'HTML, spesso prima che tutte le risorse, incluse le immagini, siano state scaricate. L'inclusione delle dimensioni consente ai browser di riservare un riquadro segnaposto di dimensioni corrette per ogni immagine, nel quale apparirà l'immagine durante il rendering iniziale della pagina.

![Due screenshot: il primo senza un'immagine ma con lo spazio riservato, il secondo mostra l'immagine caricata nello spazio riservato.](ar-guide.jpg)

Senza gli attributi `width` e `height`, non viene creato spazio segnaposto, causando un evidente {{Glossary("jank", "jank")}}, o spostamento del layout, nella pagina quando l'immagine si carica dopo il rendering della pagina. Il reflow e il repaint della pagina sono problemi di prestazioni e usabilità.

La metrica {{Glossary("CLS", "CLS")}} misura il jank al caricamento della pagina, ovvero quanto contenuto visibile si sposta nella viewport e di quanto. I principali responsabili di un CLS negativo sono gli elementi sostituiti senza dimensioni dichiarate, che subiscono reflow quando la risorsa viene caricata, incluse immagini, annunci, embed e iframe senza dimensioni o {{cssxref("aspect-ratio")}}, nonché font web.

Nei design responsive, quando un contenitore è più stretto di un'immagine, generalmente si usa il seguente CSS per impedire alle immagini di fuoriuscire dai loro contenitori:

```css
img {
  max-width: 100%;
  height: auto;
}
```

Pur essendo utile per i layout responsive, questo provoca jank e CLS negativo quando le informazioni su larghezza e altezza non sono incluse. Infatti, se non sono presenti informazioni sull'altezza quando viene eseguito il parsing dell'elemento `<img>`, ma prima che l'immagine sia stata caricata, questo CSS imposta di fatto l'altezza a 0. Quando l'immagine viene caricata dopo il rendering iniziale della pagina sullo schermo, la pagina subisce reflow e repaint, creando uno spostamento del layout mentre viene creato lo spazio per la nuova altezza determinata.

I browser dispongono di un meccanismo per dimensionare le immagini prima che l'immagine effettiva venga caricata. Quando un elemento `<img>`, `<video>` o `<input type="button">` ha gli attributi `width` e `height` impostati, il relativo rapporto d'aspetto viene calcolato prima del caricamento ed è disponibile al browser usando le dimensioni fornite.

Il rapporto d'aspetto viene quindi utilizzato per calcolare l'altezza e, di conseguenza, viene applicata la dimensione corretta all'elemento `<img>`. Ciò significa che il jank precedentemente menzionato non si verificherà o sarà minimo se le dimensioni elencate non sono completamente accurate al caricamento dell'immagine.

Il rapporto d'aspetto viene usato per riservare spazio solo durante il caricamento dell'immagine. Una volta caricata l'immagine, viene usato il rapporto d'aspetto intrinseco dell'immagine caricata o il valore della proprietà `aspect-ratio`, anziché il rapporto d'aspetto derivato dagli attributi. Ciò garantisce la visualizzazione con il rapporto d'aspetto corretto anche se le dimensioni degli attributi non sono accurate.

Sebbene le migliori pratiche per gli sviluppatori dell'ultimo decennio possano aver raccomandato di omettere gli attributi `width` e `height` di un'immagine in un elemento HTML {{htmlelement("img")}}, a causa della mappatura del rapporto d'aspetto, includere questi due attributi è considerata una buona pratica per gli sviluppatori.

Per qualsiasi immagine di sfondo, è importante impostare un valore `background-color`, affinché qualsiasi contenuto sovrapposto rimanga leggibile prima che l'immagine sia stata scaricata.

## Conclusione

In questa sezione è stata esaminata l'ottimizzazione delle immagini. Ora è disponibile una comprensione generale di come ottimizzare metà della larghezza di banda totale media del sito web medio. Questo è solo uno dei tipi di media che consumano la larghezza di banda degli utenti e rallentano il caricamento della pagina. Esaminiamo ora l'ottimizzazione dei video, affrontando il successivo 20% del consumo di larghezza di banda.

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/Measuring_performance", "Learn_web_development/Extensions/Performance/video", "Learn_web_development/Extensions/Performance")}}
