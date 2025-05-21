---
title: Ottimizzazione delle prestazioni CSS
short-title: CSS performante
slug: Learn_web_development/Extensions/Performance/CSS
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/html", "Learn_web_development/Extensions/Performance/business_case_for_performance", "Learn_web_development/Extensions/Performance")}}

Quando si sviluppa un sito web, è importante considerare come il browser gestisce il CSS del tuo sito. Per mitigare eventuali problemi di performance causati dal CSS, si dovrebbe ottimizzarlo. Ad esempio, si dovrebbe ottimizzare il CSS per mitigare la {{Glossary("Render_blocking", "render-blocking")}} e ridurre al minimo il numero di \[reflow\] richiesti. Questo articolo ti guida attraverso le principali tecniche di ottimizzazione delle prestazioni CSS.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        >, e conoscenze di base delle
        <a href="/it/docs/Learn_web_development/Getting_started/Your_first_website"
          >tecnologie web lato client</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare l'impatto del CSS sulle prestazioni del sito web
        e come ottimizzare il tuo CSS per migliorare le prestazioni.
      </td>
    </tr>
  </tbody>
</table>

## Ottimizzare o non ottimizzare

La prima domanda a cui rispondere prima di iniziare a ottimizzare il CSS è "cosa devo ottimizzare?". Alcuni dei consigli e delle tecniche discussi di seguito sono buone pratiche che beneficeranno quasi ogni progetto web, mentre altri sono necessari solo in determinate situazioni. Cercare di applicare tutte queste tecniche dappertutto è probabilmente superfluo e potrebbe essere una perdita di tempo. È necessario determinare quali ottimizzazioni delle prestazioni sono effettivamente necessarie per ciascun progetto.

Per fare questo, è necessario [misurare le prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance) del proprio sito. Come mostra il link precedente, ci sono diversi modi per misurare le prestazioni, alcuni dei quali coinvolgono sofisticate [performance API](/it/docs/Web/API/Performance_API). Il modo migliore per iniziare, tuttavia, è imparare come utilizzare strumenti come gli strumenti [di rete](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#network_monitor_tools) e [di prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#performance_monitor_tools) integrati nel browser, per vedere quali parti del caricamento della pagina richiedono più tempo e necessitano di ottimizzazione.

## Ottimizzazione del rendering

I browser seguono un percorso di rendering specifico: la verniciatura avviene solo dopo il layout, che si verifica dopo che l'albero di rendering è stato creato, il quale a sua volta richiede sia gli alberi DOM che CSSOM.

Mostrare agli utenti una pagina non stilizzata e quindi ridipingere dopo che i CSS sono stati analizzati sarebbe una pessima esperienza utente. Per questo motivo, il CSS è bloccante fino a quando il browser non determina che il CSS è necessario. Il browser può dipingere la pagina dopo aver scaricato il CSS e costruito il {{Glossary("CSSOM", "CSS object model (CSSOM)")}}.

Per ottimizzare la costruzione del CSSOM e migliorare le prestazioni della pagina, puoi fare una o più delle seguenti azioni in base allo stato attuale del tuo CSS:

- **Rimuovere stili non necessari**: Può sembrare ovvio, ma è sorprendente quanti sviluppatori dimenticano di ripulire le regole CSS inutilizzate che sono state aggiunte ai loro fogli di stile durante lo sviluppo e che alla fine non sono state utilizzate. Tutti gli stili vengono analizzati, che siano utilizzati durante il layout e la verniciatura o meno, quindi eliminare quelli inutilizzati può velocizzare il rendering della pagina. Come riassunto in [How Do You Remove Unused CSS From a Site?](https://css-tricks.com/how-do-you-remove-unused-css-from-a-site/) (csstricks.com, 2019), questo è un problema difficile da risolvere per una grande base di codice e non esiste una soluzione magica per trovare e rimuovere in modo affidabile il CSS inutilizzato. È necessario fare il duro lavoro di mantenere il CSS modulare ed essere attenti e ponderati su ciò che viene aggiunto e rimosso.

- **Dividere il CSS in moduli separati**: Mantenere il CSS modulare significa che il CSS non necessario al caricamento della pagina può essere caricato successivamente, riducendo il blocco del rendering iniziale e i tempi di caricamento del CSS. Il modo più semplice per fare ciò è dividere il tuo CSS in file separati e caricare solo ciò che è necessario:

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

  L'esempio sopra fornisce tre set di stili: stili di default che verranno sempre caricati, stili che verranno caricati solo quando il documento viene stampato e stili che verranno caricati solo da dispositivi con schermi stretti. Per default, il browser assume che ogni foglio di stile specificato sia di blocco del rendering. Puoi dire al browser quando un foglio di stile dovrebbe essere applicato aggiungendo un attributo `media` contenente una [media query](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries). Quando il browser vede un foglio di stile che deve essere applicato solo in uno scenario specifico, lo scarica comunque, ma non blocca il rendering. Separando il CSS in più file, il file principale di blocco del rendering, in questo caso `styles.css`, è molto più piccolo, riducendo il tempo per cui il rendering è bloccato.

- **Minimizzare e comprimere il tuo CSS**: La minimizzazione comporta la rimozione di tutti gli spazi bianchi nel file che sono lì solo per la leggibilità umana, una volta che il codice è messo in produzione. Puoi ridurre notevolmente i tempi di caricamento minimizzando il tuo CSS. La minimizzazione è generalmente effettuata come parte di un processo di build (ad esempio, la maggior parte dei framework JavaScript minimizzerà il codice quando costruisci un progetto pronto per il deployment). Oltre alla minimizzazione, assicurati che il server su cui è ospitato il tuo sito utilizzi la compressione come gzip sui file prima di servirli.

- **Semplificare i selettori**: Spesso le persone scrivono selettori più complessi del necessario per applicare gli stili richiesti. Ciò non solo aumenta le dimensioni del file, ma anche il tempo di analisi di quei selettori. Ad esempio:

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

  Rendere i selettori meno complessi e specifici è anche positivo per la manutenzione. È facile capire cosa stanno facendo i selettori semplici ed è facile sovrascrivere gli stili quando necessario in un secondo momento se i selettori sono meno [specifici](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts#specificity_2).

- **Non applicare stili a più elementi del necessario**: Un errore comune è applicare stili a tutti gli elementi utilizzando il [selettore universale](/it/docs/Web/CSS/Universal_selectors), o almeno, a più elementi del necessario. Questo tipo di styling può influire negativamente sulle prestazioni, specialmente su siti di grandi dimensioni.

  ```css
  /* Selects every element inside the <body> */
  body * {
    font-size: 14px;
    display: flex;
  }
  ```

  Ricorda che molte proprietà (come {{cssxref("font-size")}}) ereditano i loro valori dai genitori, quindi non è necessario applicarle ovunque. E strumenti potenti come [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox) devono essere usati con parsimonia. Usarli ovunque può causare ogni sorta di comportamento inaspettato.

- **Ridurre le richieste HTTP delle immagini con sprite CSS**: Gli [sprite CSS](https://css-tricks.com/css-sprites/) sono una tecnica che mette diverse piccole immagini (come icone) che vuoi utilizzare sul tuo sito in un singolo file immagine, e poi usa diversi valori di {{cssxref("background-position")}} per visualizzare il pezzo di immagine che desideri mostrare in ciascun posto diverso. Questo può ridurre drasticamente il numero di richieste HTTP necessarie per recuperare le immagini.

- **Precarica asset importanti**: Puoi usare [`rel="preload"`](/it/docs/Web/HTML/Reference/Attributes/rel/preload) per trasformare elementi {{htmlelement("link")}} in preloader per asset critici. Ciò include file CSS, font e immagini:

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

  Con il `preload`, il browser recupererà le risorse referenziate il più presto possibile e le metterà nella cache del browser in modo che siano pronte per l'uso non appena vengono riferite nel codice successivo. È utile precaricare risorse ad alta priorità che l'utente incontrerà presto in una pagina in modo che l'esperienza sia il più fluida possibile. Nota come puoi anche usare attributi `media` per creare preloader reattivi.

  Vedi anche [Preload critical assets to improve loading speed](https://web.dev/articles/preload-critical-assets) su web.dev (2020)

## Gestione delle animazioni

Le animazioni possono migliorare le prestazioni percepite, facendo sentire le interfacce più veloci e facendo sentire gli utenti come se il progresso venisse fatto mentre aspettano il caricamento di una pagina (ad esempio, spinner di caricamento). Tuttavia, le animazioni più grandi e un numero maggiore di animazioni richiederanno naturalmente più potenza di elaborazione per essere gestite, il che può degradare le prestazioni.

Il semplice consiglio è di ridurre tutte le animazioni non necessarie. Potresti anche fornire agli utenti un controllo/preferenza del sito per disattivare le animazioni se stanno utilizzando un dispositivo a bassa potenza o un dispositivo mobile con batteria limitata. Potresti anche usare JavaScript per controllare se l'animazione viene applicata alla pagina in primo luogo. C'è anche una media query chiamata [`prefers-reduced-motion`](/it/docs/Web/CSS/@media/prefers-reduced-motion) che può essere usata per servire selettivamente gli stili di animazione o meno in base alle preferenze a livello di sistema operativo di un utente per l'animazione.

Per le animazioni DOM essenziali, si consiglia di utilizzare [animazioni CSS](/it/docs/Web/CSS/CSS_animations/Using_CSS_animations) dove possibile, piuttosto che animazioni JavaScript (l'[API Web Animations](/it/docs/Web/API/Web_Animations_API) fornisce un modo per collegarsi direttamente alle animazioni CSS usando JavaScript).

### Scelta delle proprietà da animare

In seguito, le prestazioni delle animazioni dipendono fortemente dalle proprietà che stai animando. Alcune proprietà, quando vengono animate, attivano un {{Glossary("Reflow", "reflow")}} (e quindi anche un {{Glossary("Repaint", "repaint")}}) e dovrebbero essere evitate. Queste includono proprietà che:

- Alterano le dimensioni di un elemento, come [`width`](/it/docs/Web/CSS/width), [`height`](/it/docs/Web/CSS/height), [`border`](/it/docs/Web/CSS/border) e [`padding`](/it/docs/Web/CSS/padding).
- Riposizionano un elemento, come [`margin`](/it/docs/Web/CSS/margin), [`top`](/it/docs/Web/CSS/top), [`bottom`](/it/docs/Web/CSS/bottom), [`left`](/it/docs/Web/CSS/left) e [`right`](/it/docs/Web/CSS/right).
- Cambiano il layout di un elemento, come [`align-content`](/it/docs/Web/CSS/align-content), [`align-items`](/it/docs/Web/CSS/align-items) e [`flex`](/it/docs/Web/CSS/flex).
- Aggiungono effetti visivi che alterano la geometria dell'elemento, come [`box-shadow`](/it/docs/Web/CSS/box-shadow).

I browser moderni sono abbastanza intelligenti da ridipingere solo l'area cambiata del documento, piuttosto che l'intera pagina. Di conseguenza, le animazioni più grandi sono più costose.

Se possibile, è meglio animare proprietà che non causano reflow/repaint. Queste includono:

- [Transformazioni](/it/docs/Web/CSS/CSS_transforms)
- [`opacity`](/it/docs/Web/CSS/opacity)
- [`filter`](/it/docs/Web/CSS/filter)

### Animare sulla GPU

Per migliorare ulteriormente le prestazioni, si dovrebbe considerare di spostare il lavoro di animazione fuori dal thread principale e sulla GPU del dispositivo (anche noto come compositing). Ciò si ottiene scegliendo tipi specifici di animazioni che il browser invierà automaticamente alla GPU per gestire; queste includono:

- Animazioni di trasformazioni 3D come [`transform: translateZ()`](/it/docs/Web/CSS/transform) e [`rotate3d()`](/it/docs/Web/CSS/transform-function/rotate3d).
- Elementi con determinate altre proprietà animate come [`position: fixed`](/it/docs/Web/CSS/position).
- Elementi con [`will-change`](/it/docs/Web/CSS/will-change) applicato (vedi la sezione sottostante).
- Alcuni elementi che sono renderizzati nel proprio strato, inclusi [`<video>`](/it/docs/Web/HTML/Reference/Elements/video), [`<canvas>`](/it/docs/Web/HTML/Reference/Elements/canvas) e [`<iframe>`](/it/docs/Web/HTML/Reference/Elements/iframe).

Animazione sulla GPU può portare a un miglioramento delle prestazioni, specialmente su dispositivi mobili. Tuttavia, spostare le animazioni sulla GPU non è sempre così semplice. Leggi [CSS GPU Animation: Doing It Right](https://www.smashingmagazine.com/2016/12/gpu-animation-doing-it-right/) (smashingmagazine.com, 2016) per un'analisi molto utile e dettagliata.

## Ottimizzare le modifiche agli elementi con `will-change`

I browser possono configurare ottimizzazioni prima che un elemento sia effettivamente cambiato. Questi tipi di ottimizzazioni possono aumentare la reattività di una pagina eseguendo un lavoro potenzialmente costoso prima che sia richiesto. La proprietà CSS [`will-change`](/it/docs/Web/CSS/will-change) fornisce un indizio ai browser su come un elemento è previsto cambiare.

> **Nota:** `will-change` è inteso per essere utilizzato come ultima risorsa per cercare di affrontare problemi di prestazioni esistenti. Non dovrebbe essere usato per anticipare problemi di prestazioni.

```css
.element {
  will-change: opacity, transform;
}
```

## Ottimizzare per il blocco del rendering

Il CSS può applicare stili a particolari condizioni con le media queries. Le media queries sono importanti per un design web reattivo e ci aiutano a ottimizzare un percorso di rendering critico. Il browser blocca il rendering fino a quando non esegue il parsing di tutti questi stili, ma non bloccherà il rendering su stili che sa di non usare, come i fogli di stile di stampa. Dividendo il CSS in più file in base alle media query, puoi prevenire il blocco del rendering durante il download di CSS inutilizzato. Per creare un collegamento CSS non bloccante, sposta gli stili non immediatamente utilizzati, come gli stili di stampa, in un file separato, aggiungi un [`<link>`](/it/docs/Web/HTML/Reference/Elements/link) al markup HTML e aggiungi una media query, in questo caso specificando che è un foglio di stile di stampa.

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

Per impostazione predefinita, il browser assume che ogni foglio di stile specificato sia di blocco del rendering. Dì al browser quando il foglio di stile dovrebbe essere applicato aggiungendo un attributo `media` con la [media query](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries). Quando il browser vede un foglio di stile, sa che deve applicarlo solo per uno scenario specifico, lo scarica comunque, ma non blocca il rendering. Separando il CSS in più file, il file principale di blocco del rendering, in questo caso `styles.css`, è molto più piccolo, riducendo il tempo in cui il rendering è bloccato.

## Migliorare le prestazioni dei font

Questa sezione contiene alcuni utili consigli per migliorare le prestazioni dei web font.

In generale, pensa attentamente ai font che utilizzi sul tuo sito. Alcuni file di font possono essere molto grandi (diversi megabyte). Mentre può essere tentante utilizzare molti font per un maggiore dinamismo visivo, ciò può rallentare significativamente il caricamento della pagina e far sembrare il tuo sito disordinato. Probabilmente hai bisogno solo di due o tre font e puoi cavartela con meno se scegli di usare [font sicuri per il web](/it/docs/Learn_web_development/Core/Text_styling/Fundamentals#web_safe_fonts).

### Caricamento dei font

Ricorda che un font viene caricato solo quando è effettivamente applicato a un elemento usando la proprietà [`font-family`](/it/docs/Web/CSS/font-family), non quando è prima referenziato usando la regola `@font-face`:

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

Quindi può essere utile usare `rel="preload"` per caricare i font importanti in anticipo, in modo che siano disponibili più rapidamente quando sono effettivamente necessari:

```html
<link
  rel="preload"
  href="OpenSans-Regular-webfont.woff2"
  as="font"
  type="font/woff2"
  crossorigin />
```

Questo è più probabile che sia utile se la tua dichiarazione `font-family` è nascosta all'interno di un ampio foglio di stile esterno e non sarà raggiunta fino a molto più tardi nel processo di parsing. È comunque un compromesso: i file di font sono abbastanza grandi e se ne pre-carichi troppi, potresti ritardare altre risorse.

Puoi anche considerare:

- Usare [`rel="preconnect"`](/it/docs/Web/HTML/Reference/Attributes/rel/preconnect) per stabilire una connessione iniziale con il fornitore di font. Vedi [Preconnect to critical third-party origins](https://web.dev/articles/font-best-practices#preconnect_to_critical_third-party_origins) per i dettagli.
- Usare la [API CSS Font Loading](/it/docs/Web/API/CSS_Font_Loading_API) per personalizzare il comportamento di caricamento dei font tramite JavaScript.

### Caricare solo i glifi necessari

Quando scegli un font per il corpo del testo, è più difficile essere certi dei glifi che verranno utilizzati, specialmente se stai gestendo contenuti generati dagli utenti e/o contenuti in più lingue.

Tuttavia, se sai che utilizzerai un set specifico di glifi (ad esempio, solo glifi per i titoli o caratteri di punteggiatura specifici), potresti limitare il numero di glifi che il browser deve scaricare. Ciò può essere fatto creando un file di font che contenga solo il sottoinsieme richiesto. Un processo chiamato [subsetting](https://fonts.google.com/knowledge/glossary/subsetting). Il descrittore `unicode-range` `@font-face` può quindi essere utilizzato per specificare quando viene usato il font del subset. Se la pagina non usa alcun carattere in questo intervallo, il font non viene scaricato.

```css
@font-face {
  font-family: "Open Sans";
  src: url("OpenSans-Regular-webfont.woff2") format("woff2");
  unicode-range: U+0025-00FF;
}
```

### Definire il comportamento del display dei font con il descrittore `font-display`

Applicato alla regola `@font-face`, il descrittore [`font-display`](/it/docs/Web/CSS/@font-face/font-display) definisce come i file di font vengono caricati e mostrati dal browser, permettendo al testo di apparire con un font di riserva mentre un font si carica, o fallisce nel caricamento. Ciò migliora le prestazioni facendo apparire il testo visibile anziché avere uno schermo vuoto, con il compromesso di un flash di testo non stilizzato.

```css
@font-face {
  font-family: someFont;
  src: url(/path/to/fonts/someFont.woff) format("woff");
  font-weight: 400;
  font-style: normal;
  font-display: fallback;
}
```

## Ottimizzare il ricalcolo degli stili con il CSS containment

Utilizzando le proprietà definite nel modulo [CSS containment](/it/docs/Web/CSS/CSS_containment), è possibile istruire il browser a isolare diverse parti di una pagina e a ottimizzarne il rendering in modo indipendente l'una dall'altra. Questo consente un miglioramento delle prestazioni nel rendering di sezioni individuali. Ad esempio, puoi specificare al browser di non renderizzare certi contenitori fino a quando non sono visibili nel viewport.

La proprietà {{cssxref("contain")}} permette a un autore di specificare esattamente quali [tipi di containment](/it/docs/Web/CSS/CSS_containment/Using_CSS_containment) vuole applicare ai singoli contenitori sulla pagina. Questo permette al browser di ricalcolare layout, stile, pittura, dimensione o qualsiasi combinazione di essi per una parte limitata del DOM.

```css
article {
  contain: content;
}
```

La proprietà {{cssxref("content-visibility")}} è un utile scorciatoia, che consente agli autori di applicare un forte set di containment su un insieme di contenitori e specificare che il browser non dovrebbe eseguire il layout e il rendering di quei contenitori fino a quando necessario.

È disponibile anche una seconda proprietà, {{cssxref("contain-intrinsic-size")}}, che consente di fornire una dimensione segnaposto per i contenitori mentre sono sotto gli effetti del containment. Ciò significa che i contenitori occuperanno spazio anche se i loro contenuti non sono ancora stati renderizzati, consentendo al containment di fare la sua magia di prestazioni senza il rischio di spostare la barra di scorrimento e di creare scatti mentre gli elementi vengono renderizzati e giungono in vista. Ciò migliora la qualità dell'esperienza utente mentre il contenuto viene caricato.

```css
article {
  content-visibility: auto;
  contain-intrinsic-size: 1000px;
}
```

## Vedi anche

- [Prestazioni delle animazioni CSS](/it/docs/Web/Performance/Guides/CSS_JavaScript_animation_performance)
- [Migliori pratiche per i font](https://web.dev/articles/font-best-practices) su web.dev (2022)
- [content-visibility: the new CSS property that boosts your rendering performance](https://web.dev/articles/content-visibility) su web.dev (2022)

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/html", "Learn_web_development/Extensions/Performance/business_case_for_performance", "Learn_web_development/Extensions/Performance")}}
