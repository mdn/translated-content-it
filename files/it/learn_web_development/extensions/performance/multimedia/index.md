---
title: "Multimedia: Immagini"
slug: Learn_web_development/Extensions/Performance/Multimedia
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/measuring_performance", "Learn_web_development/Extensions/Performance/video", "Learn_web_development/Extensions/Performance")}}

I media, ossia immagini e video, rappresentano oltre il 70% dei byte scaricati per il sito web medio. In termini di prestazioni di download, eliminare i media e ridurre la dimensione dei file sono le soluzioni più immediate. Questo articolo esamina l'ottimizzazione delle immagini e dei video per migliorare le prestazioni web.

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
        Imparare a conoscere i vari formati di immagine, il loro impatto sulle prestazioni,
        e come ridurre l'impatto delle immagini sul tempo complessivo di caricamento della pagina.
      </td>
    </tr>
  </tbody>
</table>

> [!NOTE]
> Questo è un'introduzione di alto livello all'ottimizzazione della distribuzione di contenuti multimediali sul web, che copre principi e tecniche generali. Per una guida più dettagliata, vedere <https://web.dev/learn/images>.

## Perché ottimizzare il tuo contenuto multimediale?

Per il sito web medio, [il 51% della larghezza di banda proviene dalle immagini, seguite dai video al 25%](https://discuss.httparchive.org/t/state-of-the-web-top-image-optimization-strategies/1367), quindi è importante affrontare e ottimizzare i contenuti multimediali.

È necessario essere consapevoli dell'uso dei dati. Molte persone hanno piani dati limitati o addirittura a consumo dove pagano letteralmente per megabyte. Questo non è un problema solo dei mercati emergenti. Al 2018, il 24% del Regno Unito utilizzava ancora il modello a consumo secondo [OFCOM Nations & regions technology tracker - H1 2018 (PDF)](https://www.ofcom.org.uk/siteassets/resources/documents/research-and-data/technology-research/technology-tracker/technology-tracker-h1-2018-data-tables?v=323142).

Devi anche considerare la memoria poiché molti dispositivi mobili hanno RAM limitata. È importante ricordare che quando le immagini vengono scaricate, devono essere memorizzate in memoria.

## Ottimizzazione della distribuzione delle immagini

Nonostante siano i più grandi consumatori di larghezza di banda, l'impatto del download di immagini sulle [prestazioni percepite](/it/docs/Learn_web_development/Extensions/Performance/Perceived_performance) è molto inferiore a quanto molti si aspettano (principalmente perché il contenuto di testo della pagina viene scaricato immediatamente e gli utenti possono vedere le immagini mentre vengono renderizzate man mano che arrivano). Tuttavia, per una buona esperienza utente, è importante che un visitatore possa vederle il prima possibile.

### Strategia di caricamento

Uno dei miglioramenti più significativi per la maggior parte dei siti web è [il caricamento in ritardo](/it/docs/Web/Performance/Guides/Lazy_loading) delle immagini al di sotto del fold, piuttosto che scaricarle tutte al caricamento iniziale della pagina, indipendentemente dal fatto che un visitatore scorra per vederle o meno. I browser forniscono questo nativamente tramite l'attributo [`loading="lazy"`](/it/docs/Web/HTML/Reference/Elements/img#loading) sull'elemento `<img>`, e ci sono anche molte librerie JavaScript lato client che possono farlo.

Oltre a caricare un sottoinsieme di immagini, dovresti esaminare il formato stesso delle immagini:

- Stai caricando i formati di file più ottimali?
- Hai compresso bene le immagini?
- Stai caricando le dimensioni corrette?

#### Il formato più ottimale

Il formato di file ottimale dipende tipicamente dal carattere dell'immagine.

> [!NOTE]
> Per informazioni generali sui tipi di immagini, vedere la [Guida ai tipi di file immagine e ai formati](/it/docs/Web/Media/Guides/Formats/Image_types)

Il formato [SVG](/it/docs/Web/Media/Guides/Formats/Image_types#svg_scalable_vector_graphics) è più appropriato per immagini che hanno pochi colori e che non sono fotorealistiche. Questo richiede che la fonte sia disponibile in un formato grafico vettoriale. Se tale immagine esiste solo come bitmap, allora il fallback da scegliere sarebbe [PNG](/it/docs/Web/Media/Guides/Formats/Image_types#png_portable_network_graphics). Esempi di questi tipi di motivi sono loghi, illustrazioni, grafici o icone (nota: gli SVG sono di gran lunga migliori dei font icona!). Entrambi i formati supportano la trasparenza.

I PNG possono essere salvati con tre diverse combinazioni di output:

- 24-bit color + 8-bit trasparenza — offre una precisione di colore completa e una trasparenza fluida a discapito della dimensione. Probabilmente si vuole evitare questa combinazione a favore di WebP (vedi sotto).
- 8-bit color + 8-bit trasparenza — offre non più di 255 colori ma mantiene trasparenze fluide. La dimensione non è troppo grande. Questi sono i PNG che probabilmente desidererai.
- 8-bit color + 1-bit trasparenza — offre non più di 255 colori e solo trasparenza completa o nulla per pixel, il che rende i bordi di trasparenza appaiono duri e frastagliati. La dimensione è piccola ma il prezzo è la fedeltà visiva.

Un buon strumento online per ottimizzare gli SVG è [SVGOMG](https://jakearchibald.github.io/svgomg/). Per i PNG ci sono [ImageOptim online](https://imageoptim.com/online) o [Squoosh](https://squoosh.app/).

Con motivi fotografici che non presentano trasparenze, c'è una gamma molto più ampia di formati tra cui scegliere. Se vuoi giocare sul sicuro, allora opteresti per **JPEG Progressivi** ben compressi. I JPEG Progressivi, a differenza dei JPEG normali, vengono renderizzati progressivamente (da cui il nome), il che significa che l'utente vede una versione a bassa risoluzione che acquista chiarezza man mano che l'immagine viene scaricata, piuttosto che caricare l'immagine a piena risoluzione dall'alto verso il basso o addirittura renderizzarla solo una volta completamente scaricata. Un buon compressore per questi sarebbe MozJPEG, ad esempio, disponibile per l'uso nello strumento online di ottimizzazione delle immagini [Squoosh](https://squoosh.app/). Una impostazione di qualità del 75% dovrebbe fornire risultati decenti.

Altri formati migliorano le capacità del JPEG in termini di compressione, ma non sono disponibili su tutti i browser:

- [WebP](/it/docs/Web/Media/Guides/Formats/Image_types#webp_image) — Scelta eccellente sia per immagini che per immagini animate. WebP offre una compressione molto migliore rispetto a PNG o JPEG con supporto per profondità di colore superiori, fotogrammi animati, trasparenza, ecc. (ma non visualizzazione progressiva). Supportato da tutti i principali browser tranne Safari 14 su macOS desktop Big Sur o versioni precedenti.

  > [!NOTE]
  > Nonostante Apple [abbia annunciato il supporto per WebP in Safari 14](https://developer.apple.com/videos/play/wwdc2020/10663/?time=1174), le versioni di Safari precedenti alla 16.0 non visualizzano con successo le immagini `.webp` sulle versioni desktop di macOS precedenti a 11/Big Sur. Safari per iOS 14 _visualizza_ le immagini `.webp` con successo.

- [AVIF](/it/docs/Web/Media/Guides/Formats/Image_types#avif_image) — Ottima scelta per immagini e immagini animate grazie ad alte prestazioni e formato immagine senza royalties (ancora più efficiente di WebP, ma non ampiamente supportato). È ora supportato su Chrome, Edge, Opera e Firefox. [Squoosh](https://squoosh.app/) è un buon strumento online per convertire formati di immagini precedenti in AVIF.
- **JPEG2000** — una volta pensato come successore del JPEG ma supportato solo in Safari. Non supporta nemmeno la visualizzazione progressiva.

Data la ristretta compatibilità di JPEG-XR e JPEG2000, e tenendo anche in conto i costi di decodifica, l'unico serio concorrente per JPEG è WebP. Ecco perché potresti offrire le tue immagini anche in quel formato. Questo può essere fatto tramite l'elemento `<picture>` con l'aiuto di un elemento `<source>` dotato di un [attributo type](/it/docs/Web/HTML/Reference/Elements/picture#the_type_attribute).

Se tutto questo sembra un po' complicato o richiede troppo lavoro per il tuo team, ci sono anche servizi online che puoi usare come CDN di immagini che automatizzeranno la distribuzione del formato di immagine corretto al volo, in base al tipo di dispositivo o browser che richiede l'immagine. Scelte popolari includono [Cloudinary](https://cloudinary.com/blog/make_all_images_on_your_website_responsive_in_3_easy_steps), [Image Engine](https://imageengine.io/), [ImageKit](https://imagekit.io/docs/image-optimization#automatic-format-conversion), e [imgix](https://www.imgix.com/).

Infine, se desideri includere immagini animate nella tua pagina, allora sappi che Safari consente l'uso di file video all'interno degli elementi `<img>` e `<picture>`. Questi consentono anche di aggiungere un **Animated WebP** per tutti gli altri browser moderni.

```html
<picture>
  <source type="video/mp4" src="giphy.mp4" />
  <source type="image/webp" src="giphy.webp" />
  <img src="giphy.gif" alt="A GIF animation" />
</picture>
```

#### Fornire la dimensione ottimale

Nella distribuzione delle immagini, l'approccio "one size fits all" non darà i migliori risultati, il che significa che per schermi più piccoli vorresti servire immagini con risoluzione più piccola e viceversa per schermi più grandi. Inoltre, desidereresti servire immagini a risoluzione più alta a quei dispositivi che vantano uno schermo ad alta DPI (ad esempio, "Retina"). Quindi, oltre a creare molte varianti intermedie delle immagini, hai anche bisogno di un modo per servire il file giusto al browser giusto. È qui che dovresti aggiornare i tuoi elementi `<picture>` e `<source>` con attributi [`media`](/it/docs/Web/HTML/Reference/Elements/source#media) e/o [`sizes`](/it/docs/Web/HTML/Reference/Elements/source#sizes). [Responsive images done right: A guide to `<picture>` and `srcset`](https://www.smashingmagazine.com/2014/05/responsive-images-done-right-guide-picture-srcset/) spiega in dettaglio come combinare tutti questi attributi.

Due interessanti effetti da tenere a mente riguardanti gli schermi ad alta DPI sono che:

- con uno schermo ad alta DPI, gli esseri umani noteranno gli artefatti di compressione molto più tardi, il che significa che per le immagini destinate a questi schermi puoi aumentare la compressione oltre il solito.
- [Solo pochissime persone possono notare un aumento della risoluzione oltre il 2× DPI](https://observablehq.com/@eeeps/visual-acuity-and-device-pixel-ratio), il che significa che non è necessario servire immagini risolte oltre il 2×.

#### Controllare la priorità (e l'ordine) del download delle immagini

Avere le immagini più importanti davanti ai visitatori prima di quelle meno importanti può fornire un miglioramento delle prestazioni percepite.

La prima cosa da controllare è che le immagini del contenuto utilizzino elementi `<img>` o `<picture>` e che le immagini di sfondo siano definite in CSS con `background-image` — le immagini riferite negli elementi `<img>` ricevono una priorità di caricamento maggiore rispetto alle immagini di sfondo.

In secondo luogo, con l'adozione degli hints di priorità, puoi controllare ulteriormente la priorità aggiungendo un attributo `fetchPriority` ai tuoi tag immagine. Un esempio di utilizzo degli hints di priorità per le immagini sono i caroselli dove la prima immagine ha una priorità maggiore rispetto alle immagini successive.

### Strategia di rendering: prevenire "jank" nel caricamento delle immagini

Poiché le immagini vengono caricate in modo asincrono e continuano a essere caricate dopo il primo paint, se le loro dimensioni non sono definite prima del caricamento, possono causare riposizionamento dei contenuti della pagina. Ad esempio, quando il testo viene spostato verso il basso dalla pagina dalle immagini che si caricano. Per questo motivo, è importante impostare gli attributi `width` e `height` in modo che il browser possa riservare spazio per loro nel layout.

Quando gli attributi `width` e `height` di un'immagine sono inclusi su un elemento HTML {{htmlelement("img")}}, il [rapporto d'aspetto dell'immagine](/it/docs/Web/CSS/CSS_box_sizing/Understanding_aspect-ratio#adjusting_aspect_ratios_of_replaced_elements) può essere calcolato dal browser prima che l'immagine sia caricata. Questo {{Glossary("aspect_ratio", "rapporto d'aspetto")}} viene utilizzato per riservare lo spazio necessario per visualizzare l'immagine, riducendo o addirittura prevenendo una modifica del layout quando l'immagine viene scaricata e riprodotta sullo schermo. Ridurre lo spostamento del layout è un componente importante di una buona esperienza utente e delle prestazioni web.

I browser iniziano a renderizzare il contenuto mentre l'HTML viene analizzato, spesso prima che tutte le risorse, incluse le immagini, siano scaricate. Includendo le dimensioni si consente ai browser di riservare una casella segnaposto di dimensioni corrette per ogni immagine, in cui apparirà quando le immagini vengono caricate al primo rendering della pagina.

![Due schermate: la prima senza un'immagine ma con spazio riservato, la seconda mostra l'immagine caricata nello spazio riservato.](ar-guide.jpg)

Senza gli attributi `width` e `height`, non viene creato alcuno spazio segnaposto, creando una notevole {{Glossary("jank", "jank")}}, o spostamento di layout, nella pagina quando l'immagine si carica dopo il rendering della pagina. Il reflow e il ridisegno della pagina sono problemi di prestazioni e usabilità.

Il metro di misura {{Glossary("CLS", "CLS")}} misura lo jank al caricamento della pagina, ossia quanto il contenuto visibile si sposta nella finestra di visualizzazione e quanto. I principali colpevoli di una cattiva CLS sono gli elementi sostituiti senza dimensioni dichiarate che rielaborano quando il bene viene caricato, comprese immagini, annunci, embed e iframe senza dimensioni o {{cssxref("aspect-ratio")}} e web font.

Nei design responsive, quando un contenitore è più stretto di un'immagine, il seguente CSS è generalmente usato per impedire che le immagini escano dai loro contenitori:

```css
img {
  max-width: 100%;
  height: auto;
}
```

Anche se utile per i layout responsivi, questo causa jank e cattiva CLS quando le informazioni di larghezza e altezza non sono incluse, poiché se non è presente alcuna informazione sull'altezza quando l'elemento `<img>` viene analizzato ma prima che l'immagine sia stata caricata, questo CSS ha effettivamente impostato l'altezza su 0. Quando l'immagine si carica dopo che la pagina è stata inizialmente renderizzata sullo schermo, la pagina rielabora e ridisegna creando uno spostamento del layout mentre crea lo spazio per l'altezza appena determinata.

I browser hanno un meccanismo per dimensionare le immagini prima che l'immagine reale sia caricata. Quando un elemento `<img>`, `<video>`, o `<input type="button">` ha gli attributi `width` e `height` impostati, il suo rapporto d'aspetto viene calcolato prima del tempo di caricamento, ed è disponibile per il browser, utilizzando le dimensioni fornite.

Il rapporto d'aspetto viene quindi utilizzato per calcolare l'altezza, e quindi, la dimensione corretta viene applicata all'elemento `<img>`, il che significa che il jank menzionato sopra non si verificherà o sarà minimo se le dimensioni elencate non sono completamente accurate quando l'immagine si carica.

Il rapporto d'aspetto viene utilizzato per riservare spazio solo al caricamento dell'immagine. Una volta che l'immagine è stata caricata, viene utilizzato il rapporto d'aspetto intrinseco dell'immagine caricata o il valore della proprietà `aspect-ratio` piuttosto che il rapporto d'aspetto dagli attributi. Ciò garantisce che venga visualizzato al corretto rapporto d'aspetto anche se le dimensioni degli attributi non sono accurate.

Sebbene le migliori pratiche per sviluppatori dell'ultimo decennio potessero raccomandare di omettere gli attributi `width` e `height` di un'immagine su un HTML {{htmlelement("img")}}, a causa del mapping del rapporto d'aspetto, includere questi due attributi è considerato una buona pratica per gli sviluppatori.

Per qualsiasi immagine di sfondo, è importante impostare un valore `background-color` in modo che qualsiasi contenuto sovrapposto sia ancora leggibile prima che l'immagine sia stata scaricata.

## Conclusione

In questa sezione, abbiamo esaminato l'ottimizzazione delle immagini. Ora hai una comprensione generale di come ottimizzare metà del totale medio della larghezza di banda del sito web medio. Questa è solo una delle tipologie di media che consuma la larghezza di banda degli utenti e rallenta il caricamento della pagina. Diamo un'occhiata all'ottimizzazione del video, affrontando il successivo 20% del consumo di larghezza di banda.

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/measuring_performance", "Learn_web_development/Extensions/Performance/video", "Learn_web_development/Extensions/Performance")}}
