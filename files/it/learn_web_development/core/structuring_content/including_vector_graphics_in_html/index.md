---
title: Inclusione di grafica vettoriale in HTML
short-title: Grafica vettoriale
slug: Learn_web_development/Core/Structuring_content/Including_vector_graphics_in_HTML
l10n:
  sourceCommit: 2066cc916dfdcbb782340bf0ce562b230e947cba
---

La grafica vettoriale è molto utile in molte circostanze: ha file di piccole dimensioni ed è altamente scalabile, quindi non si pixella quando viene ingrandita tramite zoom o portata a grandi dimensioni. In questo articolo verrà mostrato come includerla in una pagina web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        È necessario conoscere le
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">basi di HTML</a>
        e sapere come
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/HTML_images"
          >inserire un'immagine nel documento</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare come incorporare un'immagine SVG (vettoriale) in una pagina web.</td>
    </tr>
  </tbody>
</table>

> [!NOTE]
> Questo articolo non intende insegnare SVG, ma soltanto che cos'è e come aggiungerlo alle pagine web.

## Cosa sono le immagini vettoriali?

Sul Web si lavorerà con due tipi di immagini: **immagini raster** e **immagini vettoriali**:

- Le **immagini raster** sono definite mediante una griglia di pixel: un file di immagine raster contiene informazioni che indicano esattamente dove collocare ciascun pixel e di quale colore deve essere. Tra i formati raster Web più diffusi vi sono Bitmap (`.bmp`), PNG (`.png`), JPEG (`.jpg`) e GIF (`.gif`).
- Le **immagini vettoriali** sono definite mediante algoritmi: un file di immagine vettoriale contiene definizioni di forme e percorsi che il computer può utilizzare per determinare l'aspetto dell'immagine quando viene renderizzata sullo schermo. Il formato {{Glossary("SVG", "SVG")}} consente di creare potenti immagini vettoriali da utilizzare sul Web.

Per farsi un'idea della differenza tra i due tipi, osserviamo un esempio:

```html live-sample___raster-vector live-sample___raster-vector-zoomed
<img src="star.png" alt="A raster star" />
<img src="star.svg" role="img" alt="A vector star" />
```

Questo mostra due stelle rosse apparentemente identiche con ombre esterne nere, affiancate. La differenza è che quella a sinistra è un'immagine raster (PNG), mentre quella a destra è un'immagine vettoriale (SVG).

{{embedlivesample("raster-vector", "100%", 120)}}

La differenza diventa evidente quando si esegue lo zoom sulla pagina o si aumenta la dimensione delle immagini. Quanto segue mostra come vengono renderizzate le due stelle a una larghezza di `300px`:

```css hidden live-sample___raster-vector-zoomed
img {
  width: 300px;
}
```

{{embedlivesample("raster-vector-zoomed", "100%", 350)}}

L'immagine PNG diventa pixelata perché contiene informazioni sulla posizione di ciascun pixel e sul suo colore. Quando viene ingrandita, ogni pixel aumenta di dimensione per riempire più pixel sullo schermo, quindi l'immagine inizia ad apparire a blocchi. L'immagine SVG, invece, continua ad apparire nitida, perché indipendentemente dalle sue dimensioni gli algoritmi vengono utilizzati per determinare le forme nell'immagine, scalando i valori man mano che aumenta di dimensione.

Inoltre, i file di immagini vettoriali sono molto più leggeri dei loro equivalenti raster, perché devono contenere solo un numero limitato di algoritmi, anziché informazioni su ogni singolo pixel dell'immagine.

## Cos'è SVG?

[SVG](/it/docs/Web/SVG) è un linguaggio basato su {{Glossary("XML", "XML")}} per descrivere immagini vettoriali. È fondamentalmente markup, come HTML, tranne per il fatto che sono disponibili molti elementi diversi per definire le forme che devono apparire nell'immagine e gli effetti da applicare a tali forme. SVG serve per il markup della grafica, non del contenuto. SVG definisce elementi per creare forme di base, come {{svgelement("circle")}} e {{svgelement("rect")}}, nonché elementi per creare forme più complesse, come {{svgelement("path")}} e {{svgelement("polygon")}}. Le funzionalità SVG più avanzate includono {{svgelement("feColorMatrix")}} (trasformare i colori utilizzando una matrice di trasformazione), {{svgelement("animate")}} (animare parti dell'immagine vettoriale) e {{svgelement("mask")}} (applicare una maschera sopra l'immagine).

Come esempio di base, il codice seguente crea un cerchio e un rettangolo:

```html
<svg
  version="1.1"
  baseProfile="full"
  width="300"
  height="200"
  xmlns="http://www.w3.org/2000/svg">
  <rect width="100%" height="100%" fill="black" />
  <circle cx="150" cy="100" r="90" fill="blue" />
</svg>
```

Questo crea il seguente output:

{{ EmbedLiveSample('What_is_SVG', 300, 240, "", "") }}

Dall'esempio precedente si potrebbe avere l'impressione che SVG sia facile da scrivere manualmente. È possibile scrivere manualmente SVG semplici in un editor di testo, ma per un'immagine complessa questa operazione diventa rapidamente molto difficile. Per creare immagini SVG, la maggior parte delle persone usa un editor di grafica vettoriale come [Inkscape](https://inkscape.org/) o [Illustrator](https://en.wikipedia.org/wiki/Adobe_Illustrator). Questi pacchetti consentono di creare una varietà di illustrazioni utilizzando vari strumenti grafici e di realizzare approssimazioni di fotografie, ad esempio con la funzionalità Trace Bitmap di Inkscape.

SVG presenta alcuni vantaggi aggiuntivi oltre a quelli descritti finora:

- Il testo nelle immagini vettoriali rimane accessibile, a beneficio anche della {{Glossary("SEO", "SEO")}}.
- Gli SVG si prestano bene allo styling e allo scripting, perché ogni componente dell'immagine è un elemento che può essere stilizzato tramite CSS o gestito tramite script JavaScript.

Perché, dunque, utilizzare immagini raster anziché SVG? SVG presenta alcuni svantaggi:

- SVG può diventare complesso molto rapidamente, il che significa che le dimensioni dei file possono crescere; gli SVG complessi possono inoltre richiedere tempi di elaborazione significativi nel browser.
- SVG può essere più difficile da creare rispetto alle immagini raster, a seconda del tipo di immagine che si sta tentando di realizzare.

Per le ragioni descritte sopra, le immagini raster sono probabilmente migliori per immagini complesse e precise, come le fotografie.

Le immagini SVG esportate da editor quali Inkscape offrono ampio margine di ottimizzazione delle dimensioni. Prima di pubblicarle sul Web, è probabilmente opportuno elaborarle con un ottimizzatore SVG come [SVGO](https://www.npmjs.com/package/svgo).

## Aggiungere SVG alle pagine

In questa sezione verranno esaminati i diversi modi per aggiungere immagini vettoriali SVG alle pagine web.

### Il modo rapido: elemento `img`

Per incorporare un SVG tramite un elemento {{htmlelement("img")}}, è sufficiente farvi riferimento nell'attributo src, come previsto. Sarà necessario un attributo `height` o `width` (oppure entrambi, se l'SVG non dispone di un {{Glossary("aspect_ratio", "rapporto d'aspetto")}} intrinseco). Se non è già stato fatto, leggere [Immagini HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_images).

```html
<img
  src="equilateral.svg"
  alt="triangle with all three sides equal"
  height="87"
  width="100" />
```

#### Vantaggi

- Sintassi delle immagini rapida e familiare, con un equivalente testuale integrato disponibile nell'attributo `alt`.
- È possibile trasformare facilmente l'immagine in un collegamento ipertestuale annidando `<img>` all'interno di un elemento {{htmlelement("a")}}.
- Il file SVG può essere memorizzato nella cache dal browser, con tempi di caricamento più rapidi per qualsiasi pagina futura che utilizzi l'immagine.

#### Svantaggi

- Non è possibile manipolare l'immagine con JavaScript.
- Se si desidera controllare il contenuto SVG con CSS, è necessario includere stili CSS inline nel codice SVG. I fogli di stile esterni richiamati dal file SVG non hanno effetto.
- Non è possibile riassegnare lo stile all'immagine con pseudoclassi CSS, come `:focus`.

### Risoluzione dei problemi e supporto cross-browser

Per i browser che non supportano SVG (IE 8 e versioni precedenti, Android 2.3 e versioni precedenti), è possibile fare riferimento a un PNG o JPG dall'attributo `src` e utilizzare un attributo [`srcset`](/it/docs/Web/HTML/Reference/Elements/img#srcset), riconosciuto solo dai browser recenti, per fare riferimento all'SVG. In questo caso, solo i browser supportati caricheranno l'SVG, mentre i browser più vecchi caricheranno invece il PNG:

```html
<img
  src="equilateral.png"
  alt="triangle with equal sides"
  srcset="equilateral.svg" />
```

È inoltre possibile utilizzare SVG come immagini di sfondo CSS, come mostrato di seguito. Nel codice seguente, i browser più vecchi utilizzeranno il PNG che sono in grado di interpretare, mentre i browser più recenti caricheranno l'SVG:

```css
background: url("fallback.png") no-repeat center;
background-image: url("image.svg");
background-size: contain;
```

Come per il metodo `<img>` descritto sopra, inserire SVG utilizzando immagini di sfondo CSS significa che l'SVG non può essere manipolato con JavaScript ed è soggetto alle stesse limitazioni CSS.

Se gli SVG non vengono visualizzati affatto, potrebbe essere perché il server non è configurato correttamente. Se questo è il problema, [questo articolo indicherà la direzione giusta](/it/docs/Web/SVG/Tutorials/SVG_from_scratch/Getting_started#a_word_on_web_servers_for_.svgz_files).

### Come includere codice SVG all'interno dell'HTML

È anche possibile aprire il file SVG in un editor di testo, copiare il codice SVG e incollarlo nel documento HTML. Questa operazione viene talvolta chiamata inserire **SVG inline** o **inlining SVG**. Assicurarsi che lo snippet di codice SVG inizi con un tag di apertura `<svg>` e termini con un tag di chiusura `</svg>`. Ecco un esempio molto semplice di ciò che potrebbe essere incollato nel documento:

```html
<svg width="300" height="200">
  <rect width="100%" height="100%" fill="green" />
</svg>
```

#### Vantaggi

- L'inserimento inline di SVG evita una richiesta HTTP e può quindi ridurre leggermente il tempo di caricamento.
- È possibile assegnare `class` e `id` agli elementi SVG e applicare loro stili con CSS, sia all'interno dell'SVG sia ovunque siano inserite le regole di stile CSS per il documento HTML. È infatti possibile utilizzare qualsiasi [attributo di presentazione SVG](/it/docs/Web/SVG/Reference/Attribute#presentation_attributes) come proprietà CSS.
- L'inlining SVG è l'unico approccio che consente di utilizzare interazioni CSS, come `:focus`, e animazioni CSS sull'immagine SVG, anche nel normale foglio di stile.
- È possibile trasformare il markup SVG in un collegamento ipertestuale racchiudendolo in un elemento {{htmlelement("a")}}.

#### Svantaggi

- Questo metodo è adatto solo se l'SVG viene utilizzato in un unico punto. La duplicazione rende la manutenzione onerosa in termini di risorse.
- Il codice SVG aggiuntivo aumenta la dimensione del file HTML.
- Il browser non può memorizzare nella cache SVG inline come farebbe con le normali risorse immagine, quindi le pagine che includono l'immagine non verranno caricate più velocemente dopo il primo caricamento di una pagina contenente l'immagine.
- È possibile includere un fallback in un elemento {{svgelement("foreignObject")}}, ma i browser che supportano SVG scaricano comunque tutte le immagini di fallback. Occorre valutare se il sovraccarico aggiuntivo valga davvero la pena solo per supportare browser obsoleti.

### Come incorporare un SVG con un `iframe`

È possibile aprire immagini SVG nel browser proprio come pagine web. Pertanto, incorporare un documento SVG con un `<iframe>` avviene nello stesso modo studiato in [Da `<object>` a `<iframe>` — tecnologie di incorporamento generali](/it/docs/Learn_web_development/Core/Structuring_content/General_embedding_technologies).

Ecco un breve ripasso:

```html
<iframe src="triangle.svg" width="500" height="500" sandbox></iframe>
```

Questo non è certamente il metodo migliore da scegliere:

#### Svantaggi

- Gli elementi `<iframe>` possono includere contenuto di fallback tra i tag di apertura e chiusura, ma questo viene visualizzato solo nei browser che non supportano gli `<iframe>`, non quando il caricamento dell'immagine non riesce.
- Inoltre, a meno che l'SVG e la pagina web corrente non abbiano la stessa {{Glossary("origin", "origine")}}, non è possibile utilizzare JavaScript nella pagina web principale per manipolare l'SVG.

## Sperimentare con SVG

In questo esercizio è possibile provare a sperimentare con SVG. Premere il pulsante **Play** per aprire l'esempio successivo nell'MDN Playground e modificarlo lì.

Consultare il [riferimento agli elementi SVG](/it/docs/Web/SVG/Reference/Element) per vedere quali altri elementi è possibile utilizzare e che offrono molte funzionalità integrate.
Ci sono altre forme da provare, come le ellissi, oppure è possibile sperimentare con i [motivi](/it/docs/Web/SVG/Reference/Element/pattern) o persino con gli [effetti filtro](/it/docs/Web/SVG/Reference/Element/filter).
Questa sezione riguarda le capacità di ricerca, il provare qualcosa di nuovo e il divertirsi.

Se si resta bloccati e non si riesce a far funzionare il codice, è sempre possibile ripristinarlo usando il pulsante _Reset_ nel Playground.

```html live-sample___playing-with-svg
<svg width="100%" height="100%">
  <rect width="100%" height="100%" fill="red" />
  <circle cx="100%" cy="100%" r="150" fill="blue" stroke="black" />
  <polygon points="120,0 240,225 0,225" fill="green" />
  <text
    x="50"
    y="100"
    font-family="Verdana"
    font-size="55"
    fill="white"
    stroke="black"
    stroke-width="2">
    Hello!
  </text>
</svg>
```

{{ EmbedLiveSample('playing-with-SVG', 700, 300) }}

## Riepilogo

Questo articolo ha fornito una rapida panoramica di cosa sono la grafica vettoriale e SVG, perché è utile conoscerli e come includere SVG nelle pagine web. Non è mai stato concepito come una guida completa per imparare SVG, ma soltanto come un'indicazione per sapere cos'è SVG quando lo si incontra navigando sul Web. Non preoccuparti, quindi, se non ti senti ancora un esperto di SVG. Di seguito sono inclusi alcuni collegamenti che potrebbero essere utili per approfondire il funzionamento di SVG.

## Vedere anche

- [Tutorial SVG](/it/docs/Web/SVG/Tutorials/SVG_from_scratch/Getting_started) su MDN
- [Tutorial di Sara Soueidan sulle immagini SVG responsive](https://tympanus.net/codrops/2014/08/19/making-svgs-responsive-with-css/)
- [Proprietà SVG e CSS](https://css-tricks.com/svg-properties-and-css/)
- [Come scalare gli SVG](https://css-tricks.com/scale-svg/) (non è semplice come per la grafica raster!)
