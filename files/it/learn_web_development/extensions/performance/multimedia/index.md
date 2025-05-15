---
title: "Multimedia: Immagini"
slug: Learn_web_development/Extensions/Performance/Multimedia
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/measuring_performance", "Learn_web_development/Extensions/Performance/video", "Learn_web_development/Extensions/Performance")}}

I media, ovvero immagini e video, rappresentano oltre il 70% dei byte scaricati per il sito web medio. In termini di prestazioni di download, l'eliminazione dei media e la riduzione della dimensione del file sono risultati facili da ottenere. Questo articolo esamina l'ottimizzazione delle immagini e dei video per migliorare le prestazioni web.

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
        Imparare i vari formati di immagine, il loro impatto sulle prestazioni e
        come ridurre l'impatto delle immagini sul tempo di caricamento complessivo della pagina.
      </td>
    </tr>
  </tbody>
</table>

> [!NOTE]
> Questa è un'introduzione ad alto livello all'ottimizzazione della consegna di contenuti multimediali sul web, coprendo principi e tecniche generali. Per una guida più approfondita, veda <https://web.dev/learn/images>.

## Perché ottimizzare i suoi contenuti multimediali?

Per il sito web medio, [il 51% della sua larghezza di banda proviene dalle immagini, seguito dai video al 25%](https://discuss.httparchive.org/t/state-of-the-web-top-image-optimization-strategies/1367), quindi è sicuro dire che è importante affrontare e ottimizzare i suoi contenuti multimediali.

Deve essere considerato l'uso dei dati. Molte persone hanno piani dati limitati o addirittura pagano in base all'uso effettivo pagato al megabyte. Questo non è nemmeno un problema di mercato emergente. A partire dal 2018, il 24% del Regno Unito utilizza ancora il pagamento a consumo secondo [OFCOM Nations & regions technology tracker - H1 2018 (PDF)](https://www.ofcom.org.uk/siteassets/resources/documents/research-and-data/technology-research/technology-tracker/technology-tracker-h1-2018-data-tables?v=323142).

È necessario anche considerare la memoria poiché molti dispositivi mobili hanno RAM limitata. È importante ricordare che quando le immagini sono scaricate, devono essere archiviate in memoria.

## Ottimizzazione della consegna delle immagini

Nonostante sia il maggiore consumatore di larghezza di banda, l'impatto del download delle immagini sulla [percezione delle prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Perceived_performance) è molto inferiore a quanto ci si aspetterebbe (principalmente perché il contenuto testuale della pagina viene scaricato immediatamente e gli utenti possono vedere le immagini renderizzate man mano che arrivano). Tuttavia, per una buona esperienza utente è comunque importante che un visitatore possa vederle il prima possibile.

### Strategia di caricamento

Uno dei maggiori miglioramenti per la maggior parte dei siti web è il [caricamento differito](/it/docs/Web/Performance/Guides/Lazy_loading) delle immagini sotto la piega, piuttosto che scaricarle tutte al primo caricamento della pagina indipendentemente dal fatto che il visitatore scorra per vederle o meno. I browser forniscono questo nativamente tramite l'attributo [`loading="lazy"`](/it/docs/Web/HTML/Reference/Elements/img#loading) sull'elemento `<img>`, e ci sono anche molte librerie JavaScript lato client che possono farlo.

Oltre a caricare un sottoinsieme di immagini, dovrebbe considerare il formato delle immagini stesse:

- Sta caricando i formati di file più ottimali?
- Ha compresso bene le immagini?
- Sta caricando le dimensioni corrette?

#### Il formato più ottimale

Il formato di file ottimale dipende tipicamente dal carattere dell'immagine.

> [!NOTE]
> Per informazioni generali sui tipi di immagini veda la [Guida ai tipi e formati di file immagine](/it/docs/Web/Media/Guides/Formats/Image_types)

Il formato [SVG](/it/docs/Web/Media/Guides/Formats/Image_types#svg_scalable_vector_graphics) è più appropriato per immagini con pochi colori e che non sono foto-realistiche. Questo richiede che la fonte sia disponibile in un formato grafico vettoriale. Se tale immagine esiste solo come bitmap, allora il [PNG](/it/docs/Web/Media/Guides/Formats/Image_types#png_portable_network_graphics) sarebbe il formato di riserva da scegliere. Esempi di questi tipi di motivi sono loghi, illustrazioni, grafici o icone (nota: gli SVG sono molto migliori dei font icona!). Entrambi i formati supportano la trasparenza.

I PNG possono essere salvati con tre diverse combinazioni di output:

- Colore a 24 bit + trasparenza a 8 bit — offrono precisione dei colori completa e trasparenza fluida a spese della dimensione. Probabilmente vorrebbe evitare questa combinazione a favore di WebP (si veda sotto).
- Colore a 8 bit + trasparenza a 8 bit — offrono non più di 255 colori ma mantengono trasparenze fluide. La dimensione non è troppo grande. Questi sono i PNG che probabilmente vorrà.
- Colore a 8 bit + trasparenza a 1 bit — offrono non più di 255 colori e offrono solo trasparenza totale o assente per pixel, il che rende i bordi di trasparenza apparire duri e frastagliati. La dimensione è piccola ma il prezzo è la fedeltà visiva.

Uno strumento online valido per ottimizzare gli SVG è [SVGOMG](https://jakearchibald.github.io/svgomg/). Per i PNG ci sono [ImageOptim online](https://imageoptim.com/online) o [Squoosh](https://squoosh.app/).

Con motivi fotografici che non presentano trasparenza, c'è una gamma molto più ampia di formati tra cui scegliere. Se vuole andare sul sicuro, allora sceglierebbe **JPEG Progressivi** ben compressi. I JPEG Progressivi, a differenza dei JPEG normali, vengono visualizzati progressivamente (da qui il nome), il che significa che l'utente vede una versione a bassa risoluzione che guadagna chiarezza man mano che l'immagine viene scaricata, piuttosto che caricare l'immagine a risoluzione completa dall'alto al basso o addirittura renderizzarla solo una volta completamente scaricata. Un buon compressore per questi sarebbe MozJPEG, ad esempio, disponibile per l'uso nello strumento di ottimizzazione delle immagini online [Squoosh](https://squoosh.app/). Un'impostazione di qualità del 75% dovrebbe dare buoni risultati.

Altri formati migliorano le capacità di compressione del JPEG, ma non sono disponibili su tutti i browser:

- [WebP](/it/docs/Web/Media/Guides/Formats/Image_types#webp_image) — Scelta eccellente per immagini statiche e animate. WebP offre una compressione molto migliore rispetto a PNG o JPEG con supporto per profondità di colore più elevate, fotogrammi animati, trasparenza, ecc. (ma non visualizzazione progressiva). Supportato da tutti i principali browser ad eccezione di Safari 14 su macOS desktop Big Sur o precedenti.

  > [!NOTE]
  > Nonostante Apple [abbia annunciato il supporto per WebP in Safari 14](https://developer.apple.com/videos/play/wwdc2020/10663/?time=1174), le versioni di Safari precedenti a 16.0 non visualizzano correttamente le immagini `.webp` sulle versioni desktop macOS precedenti a 11/Big Sur. Safari per iOS 14 _visualizza_ correttamente le immagini `.webp`.

- [AVIF](/it/docs/Web/Media/Guides/Formats/Image_types#avif_image) — Buona scelta per immagini statiche e animate grazie all'alta performance e al formato immagine senza diritti d'autore (ancora più efficiente di WebP, ma non così ampiamente supportato). Ora supportato su Chrome, Edge, Opera e Firefox. [Squoosh](https://squoosh.app/) è un buon strumento online per convertire i formati di immagine precedenti in AVIF.
- **JPEG2000** — una volta doveva essere il successore del JPEG ma solo supportato in Safari. Non supporta la visualizzazione progressiva.

Data la limitata compatibilità di JPEG-XR e JPEG2000, e anche considerando i costi di decodifica, l'unico serio contendente per JPEG è WebP. Per questo motivo, potrebbe anche offrire le sue immagini in quel formato. Questo può essere fatto tramite l'elemento `<picture>` con l'aiuto di un elemento `<source>` dotato di un attributo [type](/it/docs/Web/HTML/Reference/Elements/picture#the_type_attribute).

Se tutto questo sembra un po' complicato o troppo lavoro per il suo team, ci sono anche servizi online che può utilizzare come CDN di immagini che automatizzano la fornitura del formato di immagine corretto in tempo reale, in base al tipo di dispositivo o browser che richiede l'immagine. Scelte popolari includono [Cloudinary](https://cloudinary.com/blog/make_all_images_on_your_website_responsive_in_3_easy_steps), [Image Engine](https://imageengine.io/), [ImageKit](https://imagekit.io/docs/image-optimization#automatic-format-conversion) e [imgix](https://www.imgix.com/).

Infine, se vuole includere immagini animate nella sua pagina, sappia che Safari permette l'uso di file video all'interno di elementi `<img>` e `<picture>`. Questi consentono anche di aggiungere un **WebP Animato** per tutti gli altri browser moderni.

```html
<picture>
  <source type="video/mp4" src="giphy.mp4" />
  <source type="image/webp" src="giphy.webp" />
  <img src="giphy.gif" alt="A GIF animation" />
</picture>
```

#### Fornitura della dimensione ottimale

Nella consegna delle immagini, l'approccio "una taglia per tutti" non darà i migliori risultati, il che significa che per schermi più piccoli vorrebbe servire immagini con risoluzione più piccola, e viceversa per schermi più grandi. Inoltre, vorrebbe servire immagini ad alta risoluzione ai dispositivi che vantano uno schermo ad alta DPI (ad esempio, "Retina"). Quindi, oltre a creare molte varianti di immagine intermedie, è necessaria una modalità per servire il file giusto al browser giusto. Ecco dove è necessario aggiornare i suoi elementi `<picture>` e `<source>` con attributi [`media`](/it/docs/Web/HTML/Reference/Elements/source#media) e/o [`sizes`](/it/docs/Web/HTML/Reference/Elements/source#sizes). [Responsive images done right: A guide to `<picture>` and `srcset`](https://www.smashingmagazine.com/2014/05/responsive-images-done-right-guide-picture-srcset/) spiega in dettaglio come combinare tutti questi attributi.

Due effetti interessanti da tenere a mente riguardo gli schermi ad alta dpi sono:

- con uno schermo ad alta DPI, gli esseri umani noteranno gli artefatti di compressione molto più tardi, il che significa che per immagini destinate a questi schermi si può aumentare la compressione oltre il solito.
- [Solo pochissime persone sono in grado di notare un aumento di risoluzione oltre 2× DPI](https://observablehq.com/@eeeps/visual-acuity-and-device-pixel-ratio), il che significa che non è necessario servire immagini con risoluzione superiore a 2×.

#### Controllo della priorità (e dell'ordine) di download delle immagini

Mostrare le immagini più importanti ai visitatori prima di quelle meno importanti può migliorare le prestazioni percepite.

La prima cosa da verificare è che le sue immagini di contenuto utilizzino elementi `<img>` o `<picture>` e che le sue immagini di sfondo siano definite in CSS con `background-image` — le immagini riferite in elementi `<img>` ricevono una priorità di caricamento maggiore rispetto alle immagini di sfondo.

In secondo luogo, con l'adozione dei Priority Hints, può controllare ulteriormente la priorità aggiungendo un attributo `fetchPriority` ai suoi tag immagine. Un caso d'uso esemplare per i priority hints sulle immagini sono i caroselli, dove la prima immagine ha una priorità maggiore rispetto a quelle successive.

### Strategia di rendering: prevenzione delle scosse quando si caricano le immagini

Poiché le immagini sono caricate in modo asincrono e continuano a caricarsi dopo la prima verniciatura, se le loro dimensioni non sono definite prima del caricamento, possono causare riflussi al contenuto della pagina. Ad esempio, quando il testo viene spinto verso il basso nella pagina dalle immagini in caricamento. Per questo motivo, è importante impostare gli attributi `width` e `height` in modo che il browser possa riservare spazio per loro nel layout.

Quando gli attributi `width` e `height` di un'immagine sono inclusi su un elemento HTML {{htmlelement("img")}}, il [rapporto d'aspetto dell'immagine](/it/docs/Web/CSS/CSS_box_sizing/Understanding_aspect-ratio#adjusting_aspect_ratios_of_replaced_elements) può essere calcolato dal browser prima che l'immagine venga caricata. Questo {{Glossary("aspect_ratio", "rapporto d'aspetto")}} è utilizzato per riservare lo spazio necessario per visualizzare l'immagine, riducendo o addirittura prevenendo uno spostamento del layout quando l'immagine viene scaricata e rappresentata sullo schermo. Ridurre lo spostamento del layout è un elemento importante per una buona esperienza utente e per le performance web.

I browser iniziano a renderizzare i contenuti mentre l'HTML viene analizzato, spesso prima che tutte le risorse, incluse le immagini, siano scaricate. Includere le dimensioni permette ai browser di riservare un box segnaposto di dimensioni corrette per ciascuna immagine che apparirà quando le immagini saranno caricate al primo rendering della pagina.

![Due schermate, la prima senza un'immagine ma con spazio riservato, la seconda mostra l'immagine caricata nello spazio riservato.](ar-guide.jpg)

Senza gli attributi `width` e `height`, non viene creato alcuno spazio segnaposto, creando un evidente {{Glossary("jank", "jank")}}, o spostamento del layout, nella pagina quando l'immagine si carica dopo il rendering della pagina. Il recalcolo della pagina e i repaint sono problemi di prestazioni e usabilità.

La metrica {{Glossary("CLS", "CLS")}} misura il jank durante il caricamento della pagina, ovvero quanto visibile il contenuto si sposti nel viewport e di quanto. I principali colpevoli di un cattivo CLS sono gli elementi sostituiti senza dimensioni dichiarate che rifluiscono quando l'asset viene caricato, comprese immagini, annunci, embed e iframe senza dimensioni o {{cssxref("aspect-ratio")}} e font web.

Nei design responsive, quando un contenitore è più stretto di un'immagine, il seguente CSS viene generalmente utilizzato per evitare che le immagini escano dai loro contenitori:

```css
img {
  max-width: 100%;
  height: auto;
}
```

Mentre è utile per layout responsive, questo provoca jank e un cattivo CLS quando le informazioni di larghezza e altezza non sono incluse, poiché se non sono presenti informazioni di altezza quando l'elemento `<img>` viene analizzato ma prima che l'immagine sia caricata, questo CSS ha effettivamente impostato l'altezza a 0. Quando l'immagine viene caricata dopo che la pagina è stata inizialmente rappresentata sullo schermo, la pagina recalcola e effettua repaint creando uno spostamento del layout mentre crea spazio per l'altezza appena determinata.

I browser hanno un meccanismo per dimensionare le immagini prima che l'immagine effettiva venga caricata. Quando un elemento `<img>`, `<video>`, o `<input type="button">` ha gli attributi `width` e `height` impostati su di esso, il suo rapporto d'aspetto viene calcolato prima del tempo di caricamento, ed è disponibile per il browser, utilizzando le dimensioni fornite.

Il rapporto d'aspetto viene quindi utilizzato per calcolare l'altezza e, pertanto, la dimensione corretta viene applicata all'elemento `<img>`, il che significa che il jank menzionato in precedenza non si verificherà o sarà minimo se le dimensioni elencate non sono completamente accurate quando l'immagine viene caricata.

Il rapporto d'aspetto viene utilizzato per riservare spazio solo sul caricamento dell'immagine. Una volta che l'immagine è stata caricata, il rapporto d'aspetto intrinseco dell'immagine caricata o il valore della proprietà `aspect-ratio` viene utilizzato anziché il rapporto d'aspetto degli attributi. Questo assicura che venga visualizzato al rapporto d'aspetto corretto anche se le dimensioni degli attributi non sono accurate.

Mentre le pratiche migliori per gli sviluppatori dell'ultimo decennio avrebbero potuto raccomandare di omettere gli attributi `width` e `height` di un'immagine su un HTML {{htmlelement("img")}}, a causa della mappatura del rapporto d'aspetto, includere questi due attributi è considerato una buona pratica per sviluppatori.

Per tutte le immagini di sfondo, è importante impostare un valore `background-color` in modo che qualsiasi contenuto sovrapposto sia ancora leggibile prima che l'immagine sia stata scaricata.

## Conclusione

In questa sezione, abbiamo esaminato l'ottimizzazione delle immagini. Ora ha una comprensione generale di come ottimizzare la metà del totale medio della larghezza di banda di un sito web medio. Questo è solo uno dei tipi di media che consumano la larghezza di banda degli utenti e rallentano il caricamento delle pagine. Diamo un'occhiata all'ottimizzazione dei video, affrontando il successivo 20% del consumo di larghezza di banda.

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/measuring_performance", "Learn_web_development/Extensions/Performance/video", "Learn_web_development/Extensions/Performance")}}
