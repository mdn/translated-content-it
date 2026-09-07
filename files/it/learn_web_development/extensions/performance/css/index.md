---
title: Ottimizzazione delle prestazioni CSS
short-title: CSS performante
slug: Learn_web_development/Extensions/Performance/CSS
l10n:
  sourceCommit: 2b4a2ad5d9ba084a9eaa2f9204102655e7b575c4
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/HTML", "Learn_web_development/Extensions/Performance/business_case_for_performance", "Learn_web_development/Extensions/Performance")}}

Durante lo sviluppo di un sito web, è necessario considerare il modo in cui il browser gestisce il CSS del sito. Per mitigare eventuali problemi di prestazioni causati dal CSS, è opportuno ottimizzarlo. Per esempio, è consigliabile ottimizzare il CSS per mitigare il {{Glossary("Render_blocking", "render-blocking")}} e ridurre al minimo il numero di reflow necessari. Questo articolo illustra le principali tecniche di ottimizzazione delle prestazioni CSS.

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
        Apprendere l'impatto del CSS sulle prestazioni di un sito web
        e come ottimizzare il CSS per migliorare le prestazioni.
      </td>
    </tr>
  </tbody>
</table>

## Ottimizzare o non ottimizzare

La prima domanda a cui rispondere prima di iniziare a ottimizzare il CSS è: "cosa è necessario ottimizzare?". Alcuni dei suggerimenti e delle tecniche descritti di seguito sono buone pratiche che andranno a vantaggio di quasi ogni progetto web, mentre altri sono necessari solo in determinate situazioni. Cercare di applicare tutte queste tecniche ovunque è probabilmente superfluo e può essere una perdita di tempo. È opportuno stabilire quali ottimizzazioni delle prestazioni siano effettivamente necessarie in ciascun progetto.

Per farlo, è necessario [misurare le prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance) del sito. Come mostra il collegamento precedente, esistono diversi modi per misurare le prestazioni, alcuni dei quali coinvolgono sofisticate [API delle prestazioni](/it/docs/Web/API/Performance_API). Tuttavia, il modo migliore per iniziare è imparare a usare strumenti come gli strumenti integrati del browser per la [rete](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#network_monitor_tools) e le [prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#performance_monitor_tools), per individuare quali parti del caricamento della pagina richiedono molto tempo e necessitano di ottimizzazione.

## Ottimizzazione del rendering

I browser seguono uno specifico percorso di rendering: il painting avviene solo dopo il layout, che avviene dopo la creazione del render tree, il quale a sua volta richiede sia l'albero DOM sia l'albero CSSOM.

Mostrare agli utenti una pagina senza stili e poi ridipingerla dopo l'analisi degli stili CSS sarebbe una pessima esperienza utente. Per questo motivo, il CSS blocca il rendering finché il browser stabilisce che il CSS è necessario. Il browser può dipingere la pagina dopo aver scaricato il CSS e creato il {{Glossary("CSSOM", "modello a oggetti CSS (CSSOM)")}}.

Per ottimizzare la costruzione del CSSOM e migliorare le prestazioni della pagina, è possibile eseguire una o più delle seguenti operazioni, in base allo stato attuale del CSS:

- **Rimuovere gli stili non necessari**: può sembrare ovvio, ma è sorprendente quanti sviluppatori dimentichino di eliminare le regole CSS inutilizzate aggiunte ai fogli di stile durante lo sviluppo e che alla fine non vengono usate. Tutti gli stili vengono analizzati, indipendentemente dal fatto che siano usati durante il layout e il painting, quindi eliminare quelli inutilizzati può velocizzare il rendering della pagina. Come riassume [How Do You Remove Unused CSS From a Site?](https://css-tricks.com/how-do-you-remove-unused-css-from-a-site/) (csstricks.com, 2019), si tratta di un problema difficile da risolvere per una codebase di grandi dimensioni e non esiste una soluzione magica per trovare e rimuovere in modo affidabile il CSS inutilizzato. È necessario svolgere il lavoro impegnativo di mantenere il CSS modulare ed essere accurati e intenzionali riguardo a ciò che viene aggiunto e rimosso.

- **Suddividere il CSS in moduli separati**: mantenere il CSS modulare significa che il CSS non richiesto al caricamento della pagina può essere caricato in seguito, riducendo il render-blocking iniziale del CSS e i tempi di caricamento. Il modo più semplice per farlo consiste nel suddividere il CSS in file separati e caricare solo ciò che è necessario:

  ```html
  <!-- Loading and parsing styles.css is render-blocking -->
  <link rel="stylesheet" href="styles.css" />

  <!-- Loading and parsing print.css is not render-blocking -->
  <link rel="stylesheet" href="print.css" media="print" />

  <!-- Loading and parsing mobile.css is not render-blocking on large screens -->
  <link
    rel="stylesheet"
    href="mobile.css"
    media="screen and (width <= 480px)" />
  ```

  L'esempio precedente fornisce tre insiemi di stili: stili predefiniti che verranno sempre caricati, stili che verranno caricati solo durante la stampa del documento e stili che verranno caricati solo dai dispositivi con schermi stretti. Per impostazione predefinita, il browser presume che ogni foglio di stile specificato blocchi il rendering. È possibile indicare al browser quando un foglio di stile deve essere applicato aggiungendo un attributo `media` contenente una [media query](/it/docs/Web/CSS/Guides/Media_queries/Using). Quando il browser incontra un foglio di stile che deve applicare solo in uno scenario specifico, scarica comunque il foglio di stile, ma non blocca il rendering. Separando il CSS in più file, il file principale che blocca il rendering, in questo caso `styles.css`, è molto più piccolo, riducendo il tempo durante il quale il rendering è bloccato.

- **Minificare e comprimere il CSS**: la minificazione consiste nel rimuovere dal file tutti gli spazi bianchi presenti solo per la leggibilità umana, una volta che il codice viene messo in produzione. È possibile ridurre considerevolmente i tempi di caricamento minificando il CSS. La minificazione viene generalmente eseguita come parte di un processo di build; per esempio, la maggior parte dei framework JavaScript minifica il codice quando viene creata una versione del progetto pronta per il deployment. Oltre alla minificazione, assicurarsi che il server su cui è ospitato il sito utilizzi la compressione, ad esempio gzip, sui file prima di servirli.

- **Semplificare i selettori**: spesso vengono scritti selettori più complessi di quanto sia necessario per applicare gli stili richiesti. Questo non solo aumenta le dimensioni dei file, ma anche il tempo di analisi di tali selettori. Per esempio:

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

  Rendere i selettori meno complessi e specifici è utile anche per la manutenzione. È facile capire cosa fanno i selettori semplici ed è facile sovrascrivere gli stili quando necessario in seguito, se i selettori sono meno [specifici](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts#specificity_2).

- **Non applicare stili a più elementi del necessario**: un errore comune consiste nell'applicare stili a tutti gli elementi usando il [selettore universale](/it/docs/Web/CSS/Reference/Selectors/Universal_selectors), o almeno a più elementi del necessario. Questo tipo di stilizzazione può influire negativamente sulle prestazioni, specialmente nei siti più grandi.

  ```css
  /* Selects every element inside the <body> */
  body * {
    font-size: 14px;
    display: flex;
  }
  ```

  Ricordare che molte proprietà, come {{cssxref("font-size")}}, ereditano i loro valori dai rispettivi elementi genitori, quindi non è necessario applicarle ovunque. Inoltre, strumenti potenti come [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox) devono essere usati con moderazione. Usarli ovunque può causare ogni tipo di comportamento imprevisto.

- **Ridurre le richieste HTTP per le immagini con gli sprite CSS**: gli [sprite CSS](https://css-tricks.com/css-sprites/) sono una tecnica che inserisce diverse piccole immagini, come icone, da usare sul sito in un singolo file immagine, quindi utilizza diversi valori di {{cssxref("background-position")}} per mostrare la porzione dell'immagine desiderata in ciascuna posizione. Questo può ridurre drasticamente il numero di richieste HTTP necessarie per recuperare le immagini.

- **Precaricare le risorse importanti**: è possibile usare [`rel="preload"`](/it/docs/Web/HTML/Reference/Attributes/rel/preload) per trasformare gli elementi {{htmlelement("link")}} in precaricatori di risorse critiche. Sono inclusi file CSS, font e immagini:

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
    media="(width > 600px)" />
  ```

  Con `preload`, il browser recupera le risorse a cui si fa riferimento il prima possibile e le rende disponibili nella cache del browser, affinché siano pronte prima per l'uso quando vengono referenziate nel codice successivo. È utile precaricare risorse ad alta priorità che l'utente incontrerà nelle prime fasi della pagina, affinché l'esperienza sia il più fluida possibile. Si noti che è possibile usare anche attributi `media` per creare precaricatori responsive.

  Vedere anche [Preload critical assets to improve loading speed](https://web.dev/articles/preload-critical-assets) su web.dev (2020).

## Gestione delle animazioni

Le animazioni possono migliorare le prestazioni percepite, rendendo le interfacce più reattive e dando agli utenti la sensazione che ci siano progressi mentre attendono il caricamento di una pagina, come nel caso degli spinner di caricamento. Tuttavia, animazioni più grandi e un numero maggiore di animazioni richiederanno naturalmente maggiore potenza di elaborazione, il che può peggiorare le prestazioni.

Il consiglio più semplice è ridurre tutte le animazioni non necessarie. Si potrebbe anche offrire agli utenti un controllo o una preferenza del sito per disattivare le animazioni se utilizzano un dispositivo poco potente o un dispositivo mobile con batteria limitata. È inoltre possibile usare JavaScript per controllare se l'animazione viene applicata o meno alla pagina fin dall'inizio. Esiste anche una media query chiamata {{cssxref("@media/prefers-reduced-motion")}}, che può essere usata per fornire selettivamente stili di animazione o meno in base alle preferenze dell'utente relative alle animazioni a livello di sistema operativo.

Per le animazioni DOM essenziali, è consigliabile usare le [animazioni CSS](/it/docs/Web/CSS/Guides/Animations/Using) quando possibile, anziché le animazioni JavaScript; la [Web Animations API](/it/docs/Web/API/Web_Animations_API) fornisce un modo per collegarsi direttamente alle animazioni CSS usando JavaScript.

### Scelta delle proprietà da animare

Le prestazioni delle animazioni dipendono in larga misura dalle proprietà che vengono animate. Alcune proprietà, quando animate, attivano un {{Glossary("Reflow", "reflow")}}, e quindi anche un {{Glossary("Repaint", "repaint")}}, e dovrebbero essere evitate. Tra queste rientrano le proprietà che:

- Modificano le dimensioni di un elemento, come {{cssxref("width")}}, {{cssxref("height")}}, {{cssxref("border")}} e {{cssxref("padding")}}.
- Riposizionano un elemento, come {{cssxref("margin")}}, {{cssxref("top")}}, {{cssxref("bottom")}}, {{cssxref("left")}} e {{cssxref("right")}}.
- Modificano il layout di un elemento, come {{cssxref("align-content")}}, {{cssxref("align-items")}} e {{cssxref("flex")}}.
- Aggiungono effetti visivi che modificano la geometria dell'elemento, come {{cssxref("box-shadow")}}.

I browser moderni sono abbastanza intelligenti da ridipingere solo l'area modificata del documento, anziché l'intera pagina. Di conseguenza, le animazioni più grandi sono più costose.

Se possibile, è meglio animare proprietà che non causano reflow/repaint. Tra queste:

- [Trasformazioni](/it/docs/Web/CSS/Guides/Transforms)
- {{cssxref("opacity")}}
- {{cssxref("filter")}}

### Animazione sulla GPU

Per migliorare ulteriormente le prestazioni, è opportuno valutare lo spostamento del lavoro di animazione dal thread principale alla GPU del dispositivo, chiamato anche compositing. Ciò avviene scegliendo tipi specifici di animazioni che il browser invierà automaticamente alla GPU per la gestione; tra queste sono incluse:

- Animazioni di trasformazione 3D, come [`transform: translateZ()`](/it/docs/Web/CSS/Reference/Properties/transform) e {{cssxref("transform-function/rotate3d")}}.
- Elementi con determinate altre proprietà animate, come [`position: fixed`](/it/docs/Web/CSS/Reference/Properties/position).
- Elementi a cui viene applicato {{cssxref("will-change")}} (vedere la sezione seguente).
- Alcuni elementi che vengono renderizzati nel proprio layer, inclusi [`<video>`](/it/docs/Web/HTML/Reference/Elements/video), [`<canvas>`](/it/docs/Web/HTML/Reference/Elements/canvas) e [`<iframe>`](/it/docs/Web/HTML/Reference/Elements/iframe).

L'animazione sulla GPU può comportare prestazioni migliori, specialmente sui dispositivi mobili. Tuttavia, spostare le animazioni sulla GPU non è sempre così semplice. Leggere [CSS GPU Animation: Doing It Right](https://www.smashingmagazine.com/2016/12/gpu-animation-doing-it-right/) (smashingmagazine.com, 2016) per un'analisi molto utile e dettagliata.

## Ottimizzazione delle modifiche agli elementi con `will-change`

I browser possono predisporre ottimizzazioni prima che un elemento venga effettivamente modificato. Questi tipi di ottimizzazioni possono aumentare la reattività di una pagina eseguendo operazioni potenzialmente costose prima che siano necessarie. La proprietà CSS {{cssxref("will-change")}} suggerisce ai browser come si prevede che un elemento cambierà.

> [!NOTE]
> `will-change` è pensata per essere usata come ultima risorsa nel tentativo di gestire problemi di prestazioni esistenti. Non dovrebbe essere usata per anticipare problemi di prestazioni.

```css
.element {
  will-change: opacity, transform;
}
```

## Ottimizzazione per il render-blocking

Il CSS può limitare gli stili a condizioni specifiche mediante le media query. Le media query sono importanti per un web design responsive e aiutano a ottimizzare un critical rendering path. Il browser blocca il rendering finché non ha analizzato tutti questi stili, ma non blocca il rendering per gli stili che sa di non utilizzare, come i fogli di stile per la stampa. Suddividendo il CSS in più file in base alle media query, è possibile evitare il render-blocking durante il download di CSS inutilizzato. Per creare un collegamento CSS non bloccante, spostare gli stili non usati immediatamente, come gli stili di stampa, in un file separato, aggiungere un [`<link>`](/it/docs/Web/HTML/Reference/Elements/link) al markup HTML e aggiungere una media query, in questo caso indicando che si tratta di un foglio di stile per la stampa.

```html
<!-- Loading and parsing styles.css is render-blocking -->
<link rel="stylesheet" href="styles.css" />

<!-- Loading and parsing print.css is not render-blocking -->
<link rel="stylesheet" href="print.css" media="print" />

<!-- Loading and parsing mobile.css is not render-blocking on large screens -->
<link rel="stylesheet" href="mobile.css" media="screen and (width <= 480px)" />
```

Per impostazione predefinita, il browser presume che ogni foglio di stile specificato blocchi il rendering. Indicare al browser quando il foglio di stile deve essere applicato aggiungendo un attributo `media` con la [media query](/it/docs/Web/CSS/Guides/Media_queries/Using). Quando il browser incontra un foglio di stile che sa di dover applicare solo in uno scenario specifico, scarica comunque il foglio di stile, ma non blocca il rendering. Separando il CSS in più file, il file principale che blocca il rendering, in questo caso `styles.css`, è molto più piccolo, riducendo il tempo durante il quale il rendering è bloccato.

## Migliorare le prestazioni dei font

Questa sezione contiene alcuni suggerimenti utili per migliorare le prestazioni dei font web.

In generale, occorre riflettere attentamente sui font usati nel sito. Alcuni file di font possono essere molto grandi, nell'ordine di più megabyte. Sebbene possa essere allettante usare molti font per creare interesse visivo, questo può rallentare significativamente il caricamento della pagina e rendere il sito disordinato. Probabilmente sono necessari solo due o tre font, e se ne possono usare ancora meno scegliendo [font web sicuri](/it/docs/Learn_web_development/Core/Text_styling/Fundamentals#web_safe_fonts).

### Caricamento dei font

Tenere presente che un font viene caricato solo quando viene effettivamente applicato a un elemento usando la proprietà {{cssxref("font-family")}}, non quando viene referenziato per la prima volta usando l'at-rule {{cssxref("@font-face")}}:

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
  font-family: "Open Sans", sans-serif;
}
```

Può quindi essere vantaggioso usare `rel="preload"` per caricare anticipatamente i font importanti, così saranno disponibili più rapidamente quando saranno effettivamente necessari:

```html
<link
  rel="preload"
  href="OpenSans-Regular-webfont.woff2"
  as="font"
  type="font/woff2"
  crossorigin />
```

Questo è più probabile che sia vantaggioso se la dichiarazione `font-family` è nascosta all'interno di un grande foglio di stile esterno e non viene raggiunta fino a un momento significativamente successivo del processo di analisi. Si tratta tuttavia di un compromesso: i file di font sono piuttosto grandi e, precaricandone troppi, si potrebbero ritardare altre risorse.

Si può anche valutare di:

- Usare [`rel="preconnect"`](/it/docs/Web/HTML/Reference/Attributes/rel/preconnect) per stabilire una connessione anticipata con il provider dei font. Per i dettagli, vedere [Preconnect to critical third-party origins](https://web.dev/articles/font-best-practices#preconnect_to_critical_third-party_origins).
- Usare la [CSS Font Loading API](/it/docs/Web/API/CSS_Font_Loading_API) per personalizzare il comportamento di caricamento dei font tramite JavaScript.

### Caricare solo i glifi necessari

Quando si sceglie un font per il testo del corpo, è più difficile sapere con certezza quali glifi verranno usati, specialmente quando si tratta di contenuti generati dagli utenti e/o contenuti in più lingue.

Tuttavia, se è noto che verrà usato un insieme specifico di glifi, per esempio solo glifi per titoli o specifici caratteri di punteggiatura, è possibile limitare il numero di glifi che il browser deve scaricare. Questo può essere ottenuto creando un file di font che contiene solo il sottoinsieme richiesto, in un processo chiamato [subsetting](https://fonts.google.com/knowledge/glossary/subsetting). Il descrittore `@font-face` [`unicode-range`](/it/docs/Web/CSS/Reference/At-rules/@font-face/unicode-range) può quindi essere usato per specificare quando viene usato il font sottoinsieme. Se la pagina non usa alcun carattere in questo intervallo, il font non viene scaricato.

```css
@font-face {
  font-family: "Open Sans";
  src: url("OpenSans-Regular-webfont.woff2") format("woff2");
  unicode-range: U+0025-00FF;
}
```

### Definire il comportamento di visualizzazione dei font con il descrittore `font-display`

Applicato all'at-rule `@font-face`, il descrittore [`font-display`](/it/docs/Web/CSS/Reference/At-rules/@font-face/font-display) definisce il modo in cui i file di font vengono caricati e visualizzati dal browser, consentendo al testo di apparire con un font di fallback mentre un font viene caricato o non riesce a caricarsi. Questo migliora le prestazioni rendendo il testo visibile invece di mostrare una schermata vuota, con il compromesso di un flash di testo senza stile.

```css
@font-face {
  font-family: "someFont";
  src: url("/path/to/fonts/someFont.woff") format("woff");
  font-weight: normal;
  font-style: normal;
  font-display: fallback;
}
```

## Ottimizzare il ricalcolo degli stili con il containment CSS

Usando le proprietà definite nel modulo [CSS containment](/it/docs/Web/CSS/Guides/Containment), è possibile indicare al browser di isolare diverse parti di una pagina e ottimizzarne il rendering indipendentemente l'una dall'altra. Questo consente di migliorare le prestazioni nel rendering delle singole sezioni. Per esempio, è possibile specificare al browser di non renderizzare determinati contenitori finché non sono visibili nel viewport.

La proprietà {{cssxref("contain")}} consente a un autore di specificare esattamente quali [tipi di containment](/it/docs/Web/CSS/Guides/Containment/Using) desidera applicare ai singoli contenitori della pagina. Ciò consente al browser di ricalcolare layout, stile, painting, dimensione o qualsiasi combinazione di questi per una parte limitata del DOM.

```css
article {
  contain: content;
}
```

La proprietà {{cssxref("content-visibility")}} è una scorciatoia utile che consente agli autori di applicare un insieme forte di containment a un insieme di contenitori e di specificare che il browser non deve eseguire layout e rendering di tali contenitori finché non necessario.

È disponibile anche una seconda proprietà, {{cssxref("contain-intrinsic-size")}}, che consente di fornire una dimensione segnaposto per i contenitori mentre sono soggetti agli effetti del containment. Ciò significa che i contenitori occuperanno spazio anche se i relativi contenuti non sono ancora stati renderizzati, consentendo al containment di offrire i suoi vantaggi in termini di prestazioni senza il rischio di spostamenti della barra di scorrimento e jank quando gli elementi vengono renderizzati e diventano visibili. Questo migliora la qualità dell'esperienza utente durante il caricamento dei contenuti.

```css
article {
  content-visibility: auto;
  contain-intrinsic-size: 1000px;
}
```

## Ottimizzazione dei selettori `:has()`

La pseudo-classe {{cssxref(":has", ":has()")}} abilita potenti capacità di selezione, ma richiede un uso attento per evitare colli di bottiglia nelle prestazioni. Per indicazioni dettagliate sulla scrittura di selettori `:has()` efficienti, vedere [Considerazioni sulle prestazioni nella documentazione di riferimento di `:has()`](/it/docs/Web/CSS/Reference/Selectors/:has#performance_considerations).

## Vedere anche

- [Prestazioni delle animazioni CSS](/it/docs/Web/Performance/Guides/CSS_JavaScript_animation_performance)
- [Best practices for fonts](https://web.dev/articles/font-best-practices) su web.dev (2022)
- [content-visibility: the new CSS property that boosts your rendering performance](https://web.dev/articles/content-visibility) su web.dev (2022)

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/HTML", "Learn_web_development/Extensions/Performance/business_case_for_performance", "Learn_web_development/Extensions/Performance")}}
