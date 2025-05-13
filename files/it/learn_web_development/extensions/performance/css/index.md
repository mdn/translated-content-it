---
title: Ottimizzazione delle prestazioni CSS
short-title: CSS performante
slug: Learn_web_development/Extensions/Performance/CSS
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/html", "Learn_web_development/Extensions/Performance/business_case_for_performance", "Learn_web_development/Extensions/Performance")}}

Quando si sviluppa un sito web, è necessario considerare come il browser sta gestendo il CSS sul suo sito. Per mitigare eventuali problemi di prestazioni che il CSS potrebbe causare, dovresti ottimizzarlo. Ad esempio, dovresti ottimizzare il CSS per mitigare il {{Glossary("Render_blocking", "render-blocking")}} e minimizzare il numero di reflow richiesti. Questo articolo ti guida attraverso tecniche chiave di ottimizzazione delle prestazioni CSS.

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
        Imparare l'impatto del CSS sulle prestazioni del sito web
        e come ottimizzare il proprio CSS per migliorare le prestazioni.
      </td>
    </tr>
  </tbody>
</table>

## Ottimizzare o non ottimizzare

La prima domanda a cui dovresti rispondere prima di iniziare a ottimizzare il tuo CSS è "cosa devo ottimizzare?". Alcuni dei consigli e delle tecniche discussi di seguito sono buone pratiche che beneficeranno quasi tutti i progetti web, mentre alcuni sono necessari solo in determinate situazioni. Cercare di applicare tutte queste tecniche ovunque è probabilmente inutile e potrebbe essere una perdita di tempo. Dovresti capire quali ottimizzazioni delle prestazioni sono effettivamente necessarie in ogni progetto.

Per fare questo, è necessario [misurare le prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance) del tuo sito. Come mostra il link precedente, ci sono diversi modi per misurare le prestazioni, alcuni dei quali coinvolgono sofisticate [performance APIs](/it/docs/Web/API/Performance_API). Tuttavia, il modo migliore per iniziare è imparare a usare strumenti come gli strumenti integrati del browser per il [network](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#network_monitor_tools) e le [prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#performance_monitor_tools), per vedere quali parti del caricamento della pagina stanno richiedendo tempo e necessitano di ottimizzazione.

## Ottimizzare il rendering

I browser seguono un percorso di rendering specifico — il dipingimento avviene solo dopo il layout, che avviene dopo che l'albero di rendering è stato creato, il che richiede sia il DOM che gli alberi CSSOM.

Mostrare agli utenti una pagina senza stile e poi ridipingere dopo che i CSS sono stati analizzati sarebbe una cattiva esperienza utente. Per questa ragione, il CSS è di blocco per il rendering fino a quando il browser determina che il CSS è necessario. Il browser può dipingere la pagina dopo aver scaricato il CSS e costruito il {{Glossary("CSSOM", "modello oggetto CSS (CSSOM)")}}.

Per ottimizzare la costruzione del CSSOM e migliorare le prestazioni della pagina, puoi fare una o più delle seguenti operazioni in base allo stato attuale del tuo CSS:

- **Rimuovere stili non necessari**: Questo può sembrare ovvio, ma è sorprendente quante volte gli sviluppatori dimentichino di ripulire le regole CSS inutilizzate che erano state aggiunte nei fogli di stile durante lo sviluppo e che non sono finite per essere utilizzate. Tutti gli stili vengono analizzati, che siano utilizzati durante il layout e la pittura o meno, quindi eliminare quelli inutilizzati può velocizzare il rendering della pagina. Come riassume [How Do You Remove Unused CSS From a Site?](https://css-tricks.com/how-do-you-remove-unused-css-from-a-site/) (csstricks.com, 2019), questo è un problema difficile da risolvere per una grande base di codice e non c'è una bacchetta magica per trovare e rimuovere in modo affidabile il CSS inutilizzato. È necessario fare il duro lavoro di mantenere il CSS modulare ed essere attenti e deliberati su ciò che viene aggiunto e rimosso.

- **Dividere il CSS in moduli separati**: Mantenere il CSS modulare significa che il CSS non richiesto al caricamento della pagina può essere caricato successivamente, riducendo il blocco di rendering iniziale del CSS e i tempi di caricamento. Il modo più semplice per farlo è dividere il tuo CSS in file separati e caricare solo ciò che è necessario:

  ```html
  <!-- Loading and parsing styles.css is render-blocking -->
  <link rel="stylesheet" href="styles.css" />

  <!-- Loading and parsing print.css is not render-blocking -->
  <link rel="stylesheet" href="print.css" media="print" />

  <!-- Loading and parsing mobile.css is not render-blocking on large screens -->
  <link
    rel="stylesheet"
    href="mobile.css"
    media="screen and (max-width: 480px)" />
  ```

  L'esempio sopra fornisce tre set di stili — stili di base che verranno sempre caricati, stili che saranno caricati solo quando il documento viene stampato, e stili che verranno caricati solo dai dispositivi con schermi stretti. Di default, il browser presume che ogni foglio di stile specificato sia di blocco per il rendering. Puoi dire al browser quando applicare un foglio di stile aggiungendo un attributo `media` contenente una [media query](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries). Quando il browser vede un foglio di stile che deve applicare solo in uno scenario specifico, lo scarica comunque, ma non blocca il rendering. Separando il CSS in più file, il file principale di blocco per il rendering, in questo caso `styles.css`, è molto più piccolo, riducendo il tempo per cui il rendering è bloccato.

- **Minificare e comprimere il tuo CSS**: La minificazione consiste nel rimuovere tutti gli spazi bianchi nel file che sono lì solo per la leggibilità umana, una volta che il codice viene messo in produzione. Puoi ridurre i tempi di caricamento considerevolmente minificando il tuo CSS. La minificazione viene generalmente eseguita come parte di un processo di build (ad esempio, la maggior parte dei framework JavaScript minificano il codice quando costruisci un progetto pronto per il deployment). Oltre alla minificazione, assicurati che il server su cui è ospitato il tuo sito utilizzi la compressione come gzip sui file prima di servirli.

- **Semplificare i selettori**: Le persone spesso scrivono selettori più complessi del necessario per applicare gli stili richiesti. Questo non solo aumenta le dimensioni del file, ma anche il tempo di analisi per quei selettori. Ad esempio:

  ```css
  /* Very specific selector */
  body div#main-content article.post h2.headline {
    font-size: 24px;
  }

  /* You probably only need this */
  .headline {
    font-size: 24px;
  }
  ```

  Rendere i tuoi selettori meno complessi e specifici è anche un bene per la manutenzione. È facile capire cosa stanno facendo i selettori semplici, ed è facile sovrascrivere gli stili quando necessario più tardi se i selettori sono meno [specifici](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts#specificity_2).

- **Non applicare stili a più elementi del necessario**: Un errore comune è applicare stili a tutti gli elementi usando il [selettore universale](/it/docs/Web/CSS/Universal_selectors), o almeno a più elementi del necessario. Questo tipo di styling può influenzare negativamente le prestazioni, specialmente su siti più grandi.

  ```css
  /* Selects every element inside the <body> */
  body * {
    font-size: 14px;
    display: flex;
  }
  ```

  Ricorda che molte proprietà (come {{cssxref("font-size")}}) ereditano i loro valori dai loro genitori, quindi non è necessario applicarli ovunque. E strumenti potenti come [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox) devono essere utilizzati con parsimonia. Usarli ovunque può causare tutti i tipi di comportamenti inaspettati.

- **Ridurre le richieste HTTP di immagini con gli sprite CSS**: [Gli sprite CSS](https://css-tricks.com/css-sprites/) sono una tecnica che posiziona diverse piccole immagini (come le icone) che si desidera utilizzare nel proprio sito in un unico file immagine, e poi utilizza diversi valori di {{cssxref("background-position")}} per visualizzare il pezzo di immagine che si desidera mostrare in ciascun luogo diverso. Questo può ridurre drasticamente il numero di richieste HTTP necessarie per recuperare le immagini.

- **Precaricare risorse importanti**: È possibile utilizzare [`rel="preload"`](/it/docs/Web/HTML/Reference/Attributes/rel/preload) per trasformare gli elementi {{htmlelement("link")}} in precaricamenti per le risorse critiche. Questo include file CSS, font e immagini:

  ```html
  <link rel="preload" href="style.css" as="style" />

  <link
    rel="preload"
    href="ComicSans.woff2"
    as="font"
    type="font/woff2"
    crossorigin />

  <link
    rel="preload"
    href="bg-image-wide.png"
    as="image"
    media="(min-width: 601px)" />
  ```

  Con `preload`, il browser recupererà le risorse referenziate il più presto possibile e le renderà disponibili nella cache del browser in modo che siano pronte per l'uso più rapidamente quando vengono referenziate nel codice successivo. È utile precaricare risorse di alta priorità che l'utente incontrerà presto in una pagina in modo che l'esperienza sia il più fluida possibile. Si noti come è anche possibile utilizzare attributi `media` per creare precaricamenti reattivi.

  Vedi anche [Preload critical assets to improve loading speed](https://web.dev/articles/preload-critical-assets) su web.dev (2020)

## Gestione delle animazioni

Le animazioni possono migliorare le prestazioni percepite, rendendo le interfacce più reattive e facendo sentire agli utenti come se il progresso stesse venendo fatto quando stanno aspettando che una pagina si carichi (ad esempio, le animazioni di caricamento). Tuttavia, animazioni più grandi e un numero maggiore di animazioni richiederanno naturalmente più potenza di elaborazione per essere gestiti, il che può degradare le prestazioni.

Il consiglio più semplice è quello di ridurre tutte le animazioni non necessarie. Si potrebbe anche fornire agli utenti un controllo/preferenza del sito per disattivare le animazioni se stanno utilizzando un dispositivo a bassa potenza o un dispositivo mobile con batteria limitata. È possibile anche utilizzare JavaScript per controllare se l'animazione viene applicata alla pagina in primo luogo. Esiste anche una media query chiamata [`prefers-reduced-motion`](/it/docs/Web/CSS/@media/prefers-reduced-motion) che può essere utilizzata per servire selettivamente stili di animazione o meno in base alle preferenze del sistema operativo dell'utente per l'animazione.

Per le animazioni essenziali del DOM, si consiglia di utilizzare le [animazioni CSS](/it/docs/Web/CSS/CSS_animations/Using_CSS_animations) dove possibile, piuttosto che animazioni JavaScript (le [Web Animations API](/it/docs/Web/API/Web_Animations_API) forniscono un modo per agganciarsi direttamente alle animazioni CSS utilizzando JavaScript).

### Scegliere le proprietà da animare

Successivamente, le prestazioni delle animazioni dipendono fortemente dalle proprietà che si stanno animando. Alcune proprietà, quando animate, innescano un {{Glossary("Reflow", "reflow")}} (e quindi anche un {{Glossary("Repaint", "repaint")}}) e dovrebbero essere evitate. Queste includono proprietà che:

- Alterano le dimensioni di un elemento, come [`width`](/it/docs/Web/CSS/width), [`height`](/it/docs/Web/CSS/height), [`border`](/it/docs/Web/CSS/border), e [`padding`](/it/docs/Web/CSS/padding).
- Riposizionano un elemento, come [`margin`](/it/docs/Web/CSS/margin), [`top`](/it/docs/Web/CSS/top), [`bottom`](/it/docs/Web/CSS/bottom), [`left`](/it/docs/Web/CSS/left), e [`right`](/it/docs/Web/CSS/right).
- Cambiano il layout di un elemento, come [`align-content`](/it/docs/Web/CSS/align-content), [`align-items`](/it/docs/Web/CSS/align-items), e [`flex`](/it/docs/Web/CSS/flex).
- Aggiungono effetti visivi che cambiano la geometria dell'elemento, come [`box-shadow`](/it/docs/Web/CSS/box-shadow).

I browser moderni sono abbastanza intelligenti da ridipingere solo l'area modificata del documento, piuttosto che l'intera pagina. Di conseguenza, animazioni più grandi sono più costose.

Se possibile, è meglio animare proprietà che non causano reflow/repaint. Questo include:

- [Transforms](/it/docs/Web/CSS/CSS_transforms)
- [`opacity`](/it/docs/Web/CSS/opacity)
- [`filter`](/it/docs/Web/CSS/filter)

### Animare sulla GPU

Per migliorare ulteriormente le prestazioni, si dovrebbe considerare di spostare il lavoro di animazione fuori dal thread principale e sulla GPU del dispositivo (anche noto come compositing). Questo viene fatto scegliendo tipi specifici di animazioni che il browser invierà automaticamente alla GPU per gestirle; questi includono:

- Animazioni di trasformazione 3D come [`transform: translateZ()`](/it/docs/Web/CSS/transform) e [`rotate3d()`](/it/docs/Web/CSS/transform-function/rotate3d).
- Elementi con alcune altre proprietà animate come [`position: fixed`](/it/docs/Web/CSS/position).
- Elementi a cui è stato applicato [`will-change`](/it/docs/Web/CSS/will-change) (vedi la sezione seguente).
- Alcuni elementi che vengono resi nel proprio layer, inclusi [`<video>`](/it/docs/Web/HTML/Reference/Elements/video), [`<canvas>`](/it/docs/Web/HTML/Reference/Elements/canvas), e [`<iframe>`](/it/docs/Web/HTML/Reference/Elements/iframe).

L'animazione sulla GPU può portare a miglioramenti delle prestazioni, specialmente su dispositivi mobili. Tuttavia, spostare le animazioni sulla GPU non è sempre così semplice. Leggi [CSS GPU Animation: Doing It Right](https://www.smashingmagazine.com/2016/12/gpu-animation-doing-it-right/) (smashingmagazine.com, 2016) per un'analisi dettagliata e molto utile.

## Ottimizzazione delle modifiche agli elementi con `will-change`

I browser possono impostare ottimizzazioni prima che un elemento venga effettivamente cambiato. Questi tipi di ottimizzazioni possono aumentare la reattività di una pagina facendo un lavoro potenzialmente costoso prima che sia richiesto. La proprietà CSS [`will-change`](/it/docs/Web/CSS/will-change) fornisce suggerimenti ai browser su come si aspetta che un elemento cambi.

> **Note:** `will-change` è pensato per essere utilizzato come ultima risorsa per cercare di affrontare problemi di prestazioni esistenti. Non dovrebbe essere utilizzato per anticipare problemi di prestazioni.

```css
.element {
  will-change: opacity, transform;
}
```

## Ottimizzazione per il blocco di rendering

Il CSS può delimitare stili a particolari condizioni con le media query. Le media query sono importanti per un design web reattivo e ci aiutano a ottimizzare un percorso di rendering critico. Il browser blocca il rendering fino a quando non analizza tutti questi stili, ma non bloccherà il rendering sugli stili che sa che non utilizzerà, come i fogli di stile per la stampa. Dividendo il CSS in più file in base alle media query, è possibile prevenire il blocco del rendering durante il download del CSS non utilizzato. Per creare un collegamento CSS non bloccante, sposta gli stili non immediatamente utilizzati, come gli stili di stampa, in file separati, aggiungi un [`<link>`](/it/docs/Web/HTML/Reference/Elements/link) al markup HTML e aggiungi una media query, in questo caso dichiarando che è un foglio di stile per la stampa.

```html
<!-- Loading and parsing styles.css is render-blocking -->
<link rel="stylesheet" href="styles.css" />

<!-- Loading and parsing print.css is not render-blocking -->
<link rel="stylesheet" href="print.css" media="print" />

<!-- Loading and parsing mobile.css is not render-blocking on large screens -->
<link
  rel="stylesheet"
  href="mobile.css"
  media="screen and (max-width: 480px)" />
```

Per impostazione predefinita, il browser presuppone che ogni foglio di stile specificato sia di blocco per il rendering. Indica al browser quando il foglio di stile dovrebbe essere applicato aggiungendo un attributo `media` con la [media query](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries). Quando il browser vede un foglio di stile che sa di dover applicare solo in un caso specifico, lo scarica comunque, ma non blocca il rendering. Separando il CSS in più file, il file principale di blocco per il rendering, in questo caso `styles.css`, è molto più piccolo, riducendo il tempo in cui il rendering è bloccato.

## Migliorare le prestazioni dei font

Questa sezione contiene alcuni suggerimenti utili per migliorare le prestazioni dei font web.

In generale, pensa attentamente ai font che utilizzi sul tuo sito. Alcuni file di font possono essere molto grandi (multipli megabyte). Sebbene possa essere allettante usare molti font per un certo impatto visivo, questo può rallentare significativamente il caricamento della pagina e causare che il tuo sito appaia confuso. Probabilmente hai bisogno solo di due o tre font, e puoi cavartela anche con meno se scegli di usare [font sicuri sul web](/it/docs/Learn_web_development/Core/Text_styling/Fundamentals#web_safe_fonts).

### Caricamento dei font

Tieni presente che un font viene caricato solo quando viene effettivamente applicato a un elemento utilizzando la proprietà [`font-family`](/it/docs/Web/CSS/font-family), non quando viene prima referenziato utilizzando l'at-rule [`@font-face`](/it/docs/Web/CSS/@font-face):

```css
/* Font not loaded here */
@font-face {
  font-family: "Open Sans";
  src: url("OpenSans-Regular-webfont.woff2") format("woff2");
}

h1,
h2,
h3 {
  /* It is actually loaded here */
  font-family: "Open Sans";
}
```

Può quindi essere utile utilizzare `rel="preload"` per caricare i font importanti in anticipo, in modo che siano disponibili più rapidamente quando saranno effettivamente necessari:

```html
<link
  rel="preload"
  href="OpenSans-Regular-webfont.woff2"
  as="font"
  type="font/woff2"
  crossorigin />
```

Questo è più probabile che sia utile se la tua dichiarazione `font-family` è nascosta all'interno di un grande foglio di stile esterno e non verrà raggiunta fino a molto più tardi nel processo di analisi. Tuttavia, è un compromesso — i file di font sono abbastanza grandi, e se ne pre-carichi troppi, potresti ritardare altre risorse.

Puoi anche considerare di:

- Usare [`rel="preconnect"`](/it/docs/Web/HTML/Reference/Attributes/rel/preconnect) per stabilire una connessione anticipata con il fornitore del font. Vedi [Preconnect to critical third-party origins](https://web.dev/articles/font-best-practices#preconnect_to_critical_third-party_origins) per maggiori dettagli.
- Usare la [CSS Font Loading API](/it/docs/Web/API/CSS_Font_Loading_API) per personalizzare il comportamento del caricamento dei font tramite JavaScript.

### Caricare solo i glifi di cui si ha bisogno

Quando si sceglie un font per il testo del corpo, è più difficile essere sicuri dei glifi che verranno usati in esso, specialmente se si ha a che fare con contenuti generati dagli utenti e/o contenuti in più lingue.

Tuttavia, se sai che utilizzerai un set specifico di glifi (ad esempio, i glifi per le intestazioni o solo i caratteri di punteggiatura specifici), potresti limitare il numero di glifi che il browser deve scaricare. Questo può essere fatto creando un file di font che contiene solo il sottoinsieme richiesto. Un processo chiamato [subsetting](https://fonts.google.com/knowledge/glossary/subsetting). Il descrittore `@font-face` [`unicode-range`](/it/docs/Web/CSS/@font-face/unicode-range) può quindi essere usato per specificare quando usare il font del sottoinsieme. Se la pagina non usa nessun carattere in questo range, il font non viene scaricato.

```css
@font-face {
  font-family: "Open Sans";
  src: url("OpenSans-Regular-webfont.woff2") format("woff2");
  unicode-range: U+0025-00FF;
}
```

### Definire il comportamento di visualizzazione dei font con il descrittore `font-display`

Applicato all'at-rule `@font-face`, il descrittore [`font-display`](/it/docs/Web/CSS/@font-face/font-display) definisce come i file dei font vengono caricati e visualizzati dal browser, permettendo al testo di apparire con un font di fallback mentre un font si carica, o non riesce a caricare. Questo migliora le prestazioni rendendo il testo visibile invece di avere una schermata vuota, con un compromesso che è un lampeggiamento di testo non stilizzato.

```css
@font-face {
  font-family: someFont;
  src: url(/path/to/fonts/someFont.woff) format("woff");
  font-weight: 400;
  font-style: normal;
  font-display: fallback;
}
```

## Ottimizzazione del ricalcolo dei stili con CSS containment

Utilizzando le proprietà definite nel modulo [CSS containment](/it/docs/Web/CSS/CSS_containment), puoi istruire il browser a isolare diverse parti di una pagina e ottimizzare il loro rendering in modo indipendente l'una dall'altra. Questo consente un miglioramento delle prestazioni nel rendering delle singole sezioni. Ad esempio, puoi specificare al browser di non rendere alcuni contenitori fino a quando non sono visibili nella viewport.

La proprietà {{cssxref("contain")}} consente a un autore di specificare esattamente quali [tipi di contenimento](/it/docs/Web/CSS/CSS_containment/Using_CSS_containment) vogliono applicare ai singoli contenitori sulla pagina. Ciò consente al browser di ricalcolare il layout, lo stile, la pittura, le dimensioni, o qualsiasi combinazione di essi per una parte limitata del DOM.

```css
article {
  contain: content;
}
```

La proprietà {{cssxref("content-visibility")}} è una scorciatoia utile, che consente agli autori di applicare un set forte di contenimenti su un insieme di contenitori e specificare che il browser non dovrebbe dare layout e rendere quei contenitori fino a quando non sono necessari.

È disponibile anche una seconda proprietà, {{cssxref("contain-intrinsic-size")}}, che consente di fornire una dimensione del placeholder per i contenitori mentre sono sotto gli effetti del containment. Ciò significa che i contenitori occuperanno spazio anche se il loro contenuto non è ancora stato reso, consentendo al containment di fare la sua magia di prestazioni senza il rischio di spostamento delle barre di scorrimento e jank man mano che gli elementi vengono resi e appaiono in vista. Questo migliora la qualità dell'esperienza utente man mano che il contenuto viene caricato.

```css
article {
  content-visibility: auto;
  contain-intrinsic-size: 1000px;
}
```

## Vedi anche

- [Prestazioni delle animazioni CSS](/it/docs/Web/Performance/Guides/CSS_JavaScript_animation_performance)
- [Best practices for fonts](https://web.dev/articles/font-best-practices) su web.dev (2022)
- [content-visibility: la nuova proprietà CSS che migliora le tue prestazioni di rendering](https://web.dev/articles/content-visibility) su web.dev (2022)

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/html", "Learn_web_development/Extensions/Performance/business_case_for_performance", "Learn_web_development/Extensions/Performance")}}
