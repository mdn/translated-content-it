---
title: "Multimedia: Immagini"
slug: Learn_web_development/Extensions/Performance/Multimedia
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/measuring_performance", "Learn_web_development/Extensions/Performance/video", "Learn_web_development/Extensions/Performance")}}

I media, vale a dire immagini e video, rappresentano oltre il 70% dei byte scaricati per il sito web medio. In termini di prestazioni di download, eliminare i media e ridurre la dimensione dei file è un intervento a bassa difficoltà. Questo articolo esamina l'ottimizzazione delle immagini e dei video per migliorare le prestazioni del web.

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
        Imparare i vari formati di immagini, il loro impatto sulle prestazioni,
        e come ridurre l'impatto delle immagini sul tempo complessivo di caricamento della pagina.
      </td>
    </tr>
  </tbody>
</table>

> [!NOTE]
> Questa è un'introduzione di alto livello all'ottimizzazione della distribuzione multimediale sul web, che copre principi e tecniche generali. Per una guida più approfondita, vedere <https://web.dev/learn/images>.

## Perché ottimizzare i suoi contenuti multimediali?

Per il sito web medio, [il 51% della sua larghezza di banda proviene da immagini, seguito dai video al 25%](https://discuss.httparchive.org/t/state-of-the-web-top-image-optimization-strategies/1367), quindi è giusto dire che è importante affrontare e ottimizzare i suoi contenuti multimediali.

Lei deve considerare l'uso dei dati. Molte persone hanno piani dati a consumo o anche pay-as-you-go dove letteralmente pagano per megabyte. Questo non è neanche un problema dei mercati emergenti. Nel 2018, il 24% del Regno Unito utilizzava ancora il pay-as-you-go secondo [OFCOM Nations & regions technology tracker - H1 2018 (PDF)](https://www.ofcom.org.uk/siteassets/resources/documents/research-and-data/technology-research/technology-tracker/technology-tracker-h1-2018-data-tables?v=323142).

Deve inoltre tenere conto della memoria poiché molti dispositivi mobili hanno memoria RAM limitata. È importante ricordare che quando le immagini vengono scaricate, devono essere memorizzate in memoria.

## Ottimizzazione della distribuzione delle immagini

Nonostante sia il maggiore consumatore di larghezza di banda, l'impatto del download delle immagini sulla [prestazione percepita](/it/docs/Learn_web_development/Extensions/Performance/Perceived_performance) è di gran lunga inferiore a quanto molti si aspettano (principalmente perché il contenuto testuale della pagina viene scaricato immediatamente e gli utenti possono vedere le immagini essere rese man mano che arrivano). Tuttavia, per una buona esperienza utente, è importante che un visitatore possa vederle il prima possibile.

### Strategia di caricamento

Uno dei maggiori miglioramenti alla maggior parte dei siti web è [il caricamento ritardato](/it/docs/Web/Performance/Guides/Lazy_loading) delle immagini sotto la piega, piuttosto che scaricarle tutte al primo caricamento della pagina indipendentemente dal fatto che un visitatore scorra per vederle o meno. I browser forniscono questo nativamente tramite l'attributo [`loading="lazy"`](/it/docs/Web/HTML/Reference/Elements/img#loading) sull'elemento `<img>`, e ci sono anche molte librerie JavaScript lato client che possono farlo.

Oltre a caricare un sottoinsieme di immagini, dovrebbe esaminare il formato delle immagini stesse:

- Sta caricando i formati di file più ottimali?
- Ha compresso bene le immagini?
- Sta caricando le dimensioni corrette?

#### Il formato più ottimale

Il formato di file ottimale dipende tipicamente dal carattere dell'immagine.

> [!NOTE]
> Per informazioni generali sui tipi di immagini vedere la [guida ai tipi e formati di file immagine](/it/docs/Web/Media/Guides/Formats/Image_types)

Il formato [SVG](/it/docs/Web/Media/Guides/Formats/Image_types#svg_scalable_vector_graphics) è più appropriato per le immagini che hanno pochi colori e che non sono fotorealistiche. Questo richiede che la sorgente sia disponibile in un formato di grafica vettoriale. Se un'immagine del genere esiste solo come bitmap, allora [PNG](/it/docs/Web/Media/Guides/Formats/Image_types#png_portable_network_graphics) sarebbe il formato di ripiego da scegliere. Esempi di questi tipi di motivi sono loghi, illustrazioni, grafici o icone (nota: gli SVG sono di gran lunga migliori dei font di icone!). Entrambi i formati supportano la trasparenza.

I PNG possono essere salvati con tre diverse combinazioni di output:

- Colore a 24 bit + trasparenza a 8 bit — offrono precisione del colore completa e trasparenza fluida a discapito delle dimensioni. Probabilmente vuole evitare questa combinazione a favore di WebP (vedere sotto).
- Colore a 8 bit + trasparenza a 8 bit — offrono non più di 255 colori ma mantengono trasparenze fluide. La dimensione non è troppo grande. Questi sono i PNG che probabilmente vorrà.
- Colore a 8 bit + trasparenza a 1 bit — offrono non più di 255 colori e offrono solo nessuna o piena trasparenza per pixel il che rende i bordi della trasparenza duri e frastagliati. La dimensione è piccola ma il prezzo è la fedeltà visiva.

Un buon strumento online per ottimizzare gli SVG è [SVGOMG](https://jakearchibald.github.io/svgomg/). Per i PNG ci sono [ImageOptim online](https://imageoptim.com/online) o [Squoosh](https://squoosh.app/).

Con motivi fotografici che non presentano trasparenza, esiste una gamma molto più ampia di formati tra cui scegliere. Se vuole andare sul sicuro, sceglierà **JPEG progressivi** ben compressi. I JPEG progressivi, a differenza dei JPEG normali, si rendono progressivamente (da qui il nome), il che significa che l'utente vede una versione a bassa risoluzione che acquisisce chiarezza man mano che l'immagine viene scaricata, piuttosto che l'immagine che si carica a piena risoluzione dall'alto verso il basso o persino solo rappresentata una volta completamente scaricata. Un buon compressore per questi sarebbe MozJPEG, disponibile ad esempio per l'utilizzo nello strumento online di ottimizzazione delle immagini [Squoosh](https://squoosh.app/). Un'impostazione di qualità del 75% dovrebbe produrre risultati decenti.

Altri formati migliorano le capacità di compressione del JPEG, ma non sono disponibili su tutti i browser:

- [WebP](/it/docs/Web/Media/Guides/Formats/Image_types#webp_image) — Scelta eccellente sia per immagini che per immagini animate. WebP offre una compressione molto migliore del PNG o del JPEG con supporto per profondità di colore più elevate, frame animati, trasparenza, ecc. (ma non visualizzazione progressiva). Supportato da tutti i principali browser, eccetto Safari 14 su macOS desktop Big Sur o precedenti.

  > [!NOTE]
  > Nonostante Apple [abbia annunciato il supporto per WebP in Safari 14](https://developer.apple.com/videos/play/wwdc2020/10663/?time=1174), le versioni di Safari precedenti alla 16.0 non visualizzano correttamente le immagini `.webp` su macOS desktop versione 11/Big Sur o precedente. Safari per iOS 14 _visualizza_ correttamente le immagini `.webp`.

- [AVIF](/it/docs/Web/Media/Guides/Formats/Image_types#avif_image) — Buona scelta sia per immagini che per immagini animate grazie all'elevata prestazione ed è un formato di immagine privo di royalty (ancora più efficiente del WebP, ma non ampiamente supportato). Ora è supportato su Chrome, Edge, Opera e Firefox. [Squoosh](https://squoosh.app/) è un buon strumento online per convertire formati di immagine precedenti in AVIF.
- **JPEG2000** — pensato per essere il successore del JPEG ma è supportato solo in Safari. Non supporta nemmeno la visualizzazione progressiva.

Dato il supporto limitato per JPEG-XR e JPEG2000, e considerando anche i costi di decodifica, l'unico contendere serio per JPEG è WebP. Questo è il motivo per cui potrebbe offrire le sue immagini anche in quel formato. Questo può essere fatto tramite l'elemento `<picture>` con l'aiuto di un elemento `<source>` dotato di un [attributo type](/it/docs/Web/HTML/Reference/Elements/picture#the_type_attribute).

Se tutto ciò sembra complicato o richiede troppo lavoro per il suo team, esistono anche servizi online che può utilizzare come CDN di immagini che automerŕ il servizio del formato di immagine corretto al volo, in base al tipo di dispositivo o browser richiedente l'immagine. Scelte popolari includono [Cloudinary](https://cloudinary.com/blog/make_all_images_on_your_website_responsive_in_3_easy_steps), [Image Engine](https://imageengine.io/), [ImageKit](https://imagekit.io/docs/image-optimization#automatic-format-conversion) e [imgix](https://www.imgix.com/).

Infine, se desidera includere immagini animate sulla sua pagina, sappia che Safari consente l'uso di file video all'interno di `<img>` e `<picture>`. Questi le consentono anche di aggiungere un **Animated WebP** per tutti gli altri browser moderni.

```html
<picture>
  <source type="video/mp4" src="giphy.mp4" />
  <source type="image/webp" src="giphy.webp" />
  <img src="giphy.gif" alt="A GIF animation" />
</picture>
```

#### Fornire la dimensione ottimale

Nella distribuzione delle immagini, l'approccio "una dimensione va bene per tutti" non darà i migliori risultati, il che significa che per schermi più piccoli vorrà fornire immagini con risoluzione più piccola e viceversa per schermi più grandi. Inoltre, vorrà anche fornire immagini ad alta risoluzione a quei dispositivi dotati di uno schermo ad alta DPI (ad esempio, "Retina"). Quindi, oltre a creare molte varianti intermedie di immagini, avrà bisogno di un modo per fornire il file corretto al browser corretto. È qui che dovrà aggiornare i suoi elementi `<picture>` e `<source>` con gli attributi [media](/it/docs/Web/HTML/Reference/Elements/source#media) e/o [sizes](/it/docs/Web/HTML/Reference/Elements/source#sizes). Un articolo dettagliato su come combinare tutti questi attributi può essere trovato [qui](https://www.smashingmagazine.com/2014/05/responsive-images-done-right-guide-picture-srcset/).

Due effetti interessanti da tenere a mente riguardo agli schermi ad alta dpi sono che:

- con uno schermo ad alta DPI, gli esseri umani noteranno artefatti di compressione molto più tardi, il che significa che per le immagini destinate a questi schermi può aumentare la compressione oltre il solito.
- [solo pochissime persone possono rilevare un aumento di risoluzione oltre il 2× DPI](https://observablehq.com/@eeeps/visual-acuity-and-device-pixel-ratio), il che significa che non ha bisogno di fornire immagini con una risoluzione superiore a 2×.

#### Controllare la priorità (e l'ordine) del download delle immagini

Portare le immagini più importanti davanti ai visitatori prima di quelle meno importanti può offrire una migliore prestazione percepita.

La prima cosa da controllare è che le immagini di contenuto utilizzino elementi `<img>` o `<picture>` e che le immagini di sfondo siano definite in CSS con `background-image` — le immagini referenziate negli elementi `<img>` ricevono una priorità di caricamento più alta rispetto alle immagini di sfondo.

In secondo luogo, con l'adozione degli Hint di priorità, può controllare ulteriormente la priorità aggiungendo un attributo `fetchPriority` ai tag delle immagini. Un caso d'uso per gli hint di priorità sulle immagini sono i caroselli in cui la prima immagine ha una priorità più alta rispetto alle immagini successive.

### Strategia di rendering: prevenire il jank durante il caricamento delle immagini

Poiché le immagini vengono caricate in modo asincrono e continuano a caricare dopo il primo rendering, se le loro dimensioni non sono definite prima del caricamento, possono causare ritardi nel contenuto della pagina. Ad esempio, quando il testo viene spinto verso il basso nella pagina dalle immagini che si caricano. Per questo motivo, è importante impostare gli attributi `width` e `height` in modo che il browser possa riservare spazio per loro nel layout.

Quando gli attributi `width` e `height` di un'immagine sono inclusi in un elemento HTML {{htmlelement("img")}}, il [rapporto d'aspetto dell'immagine](/it/docs/Web/CSS/CSS_box_sizing/Understanding_aspect-ratio#adjusting_aspect_ratios_of_replaced_elements) può essere calcolato dal browser prima che l'immagine sia caricata. Questo {{Glossary("aspect_ratio", "rapporto d'aspetto")}} viene utilizzato per riservare lo spazio necessario per visualizzare l'immagine, riducendo o addirittura prevenendo uno spostamento del layout quando l'immagine viene scaricata e dipinta sullo schermo. Ridurre lo spostamento del layout è un componente importante di una buona esperienza utente e delle prestazioni del web.

I browser iniziano a renderizzare il contenuto man mano che si analizza l'HTML, spesso prima che tutte le risorse, incluse le immagini, siano scaricate. Includere le dimensioni consente ai browser di riservare un box placeholder correttamente dimensionato per ciascuna immagine da visualizzare quando le immagini vengono caricate durante il primo rendering della pagina.

![Due schermate, la prima senza un'immagine ma con uno spazio riservato, la seconda mostra l'immagine caricata nello spazio riservato.](ar-guide.jpg)

Senza gli attributi `width` e `height`, non viene creato alcuno spazio placeholder, creando un evidente {{Glossary("jank", "jank")}}, o spostamento del layout, nella pagina quando l'immagine viene caricata dopo che la pagina è stata renderizzata. Il reflow della pagina e i reprint sono problemi di prestazioni e usabilità.

Il {{Glossary("CLS", "CLS")}} misura il jank durante il caricamento della pagina, ovvero quanto e quanto il contenuto visibile si sposta nella viewport. I principali responsabili del cattivo CLS sono gli elementi sostituiti senza dimensioni dichiarate che ridimensionano quando l'asset viene caricato, incluse immagini, annunci, incorporamenti e iframe senza una dimensione o {{cssxref("aspect-ratio")}} e font web.

Nei design responsivi, quando un contenitore è più stretto di un'immagine, si usa generalmente il seguente CSS per impedire che le immagini fuoriescano dai loro contenitori:

```css
img {
  max-width: 100%;
  height: auto;
}
```

Sebbene utile per i layout responsivi, questo causa jank e scarso CLS quando non sono incluse informazioni su larghezza e altezza, poiché se non sono presenti informazioni sull'altezza quando l'elemento `<img>` viene analizzato ma prima che l'immagine sia stata caricata, questo CSS ha effettivamente impostato l'altezza a 0. Quando l'immagine viene caricata dopo che la pagina è stata inizialmente renderizzata sullo schermo, la pagina ridimensiona e rieffettua il reprint creando uno spostamento nel layout mentre crea spazio per l'altezza appena determinata.

I browser hanno un meccanismo per dimensionare le immagini prima che l'immagine effettivamente sia caricata. Quando un elemento `<img>`, `<video>` o `<input type="button">` ha attributi `width` e `height` impostati, il suo rapporto di aspetto viene calcolato prima del tempo di caricamento ed è disponibile per il browser, utilizzando le dimensioni fornite.

Il rapporto d'aspetto viene poi utilizzato per calcolare l'altezza e, quindi, la dimensione corretta viene applicata all'elemento `<img>`, il che significa che il jank menzionato non si verificherà o sarà minimo se le dimensioni elencate non sono completamente accurate quando l'immagine viene caricata.

Il rapporto d'aspetto viene utilizzato per riservare spazio solo al caricamento dell'immagine. Una volta che l'immagine è stata caricata, il rapporto d'aspetto intrinseco dell'immagine caricata o il valore della proprietà `aspect-ratio` viene utilizzato al posto del rapporto d'aspetto dagli attributi. Ciò garantisce che venga visualizzato con il rapporto d'aspetto corretto anche se le dimensioni degli attributi non sono accurate.

Mentre le migliori pratiche degli sviluppatori dell'ultimo decennio avrebbero potuto raccomandare di omettere gli attributi `width` e `height` di un'immagine su un HTML {{htmlelement("img")}}, grazie alla mappatura del rapporto d'aspetto, l'inclusione di questi due attributi è considerata una buona pratica per gli sviluppatori.

Per eventuali immagini di sfondo, è importante impostare un valore `background-color` in modo che qualsiasi contenuto sovrapposto sia ancora leggibile prima che l'immagine venga scaricata.

## Conclusione

In questa sezione, abbiamo esaminato l'ottimizzazione delle immagini. Ha ora una comprensione generale di come ottimizzare la metà del totale medio della larghezza di banda del sito web medio. Questo è solo uno dei tipi di media che consumano la larghezza di banda degli utenti e rallentano il caricamento della pagina. Diamo un'occhiata all'ottimizzazione video, affrontando il prossimo 20% di consumo della larghezza di banda.

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/measuring_performance", "Learn_web_development/Extensions/Performance/video", "Learn_web_development/Extensions/Performance")}}
