---
title: Progettazione Responsive
slug: Learn_web_development/Core/CSS_layout/Responsive_Design
l10n:
  sourceCommit: 0915a5e602d475bd1a1a57d905f0bac1b7ed57b8
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Grids", "Learn_web_development/Core/CSS_layout/Media_queries", "Learn_web_development/Core/CSS_layout")}}

La _web design responsive_ (RWD) è un approccio alla progettazione web per rendere le pagine web ben visualizzabili su tutte le dimensioni e risoluzioni degli schermi garantendo una buona usabilità. È il modo di progettare per un web multi-dispositivo. In questo articolo, aiuteremo a capire alcune tecniche che possono essere utilizzate per padroneggiarlo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS Styling basics</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Stile del testo e dei font fondamentali</a>,
        familiarità con i <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">concetti fondamentali del layout CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Cos'è il design responsive: progettare layout web in modo che siano flessibili e funzionino bene su diverse dimensioni di schermo, risoluzioni, ecc.</li>
          <li>La relazione tra strumenti di layout moderni come griglia e flexbox e il design responsive.</li>
          <li>I concetti alla base dell'uso delle media query per il design responsive, inclusi il principio mobile-first e i breakpoints.</li>
          <li>Perché <code>&lt;meta viewport=""&gt;</code> è necessario per visualizzare correttamente i documenti web su dispositivi mobili.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Precursore della progettazione responsive: progettazione web per dispositivi mobili

Prima che il web design responsive diventasse l'approccio standard per rendere i siti web funzionanti su diversi tipi di dispositivi, gli sviluppatori web parlavano di design web mobile, sviluppo web mobile o, talvolta, design mobile-friendly. Questi sono sostanzialmente equivalenti al web design responsive: gli obiettivi sono quelli di assicurarsi che i siti web funzionino bene sui dispositivi con attributi fisici diversi (dimensione dello schermo, risoluzione) in termini di layout, contenuto (testo e media) e prestazioni.

La differenza riguarda principalmente i dispositivi coinvolti e le tecnologie disponibili per creare soluzioni:

- Si parlava di desktop o mobile, ma ora ci sono molti tipi di dispositivi disponibili come desktop, laptop, mobili, tablet, orologi, ecc. Invece di adattarsi per alcune dimensioni di schermo diverse, ora dobbiamo progettare siti in modo da adattarsi a dimensioni di schermo e risoluzioni comuni, più incognite.
- I dispositivi mobili erano poco potenti in termini di CPU/GPU e larghezza di banda disponibile. Alcuni non supportavano CSS o addirittura HTML, e di conseguenza, era comune effettuare sniffing del browser lato server per determinare il tipo di dispositivo/browser prima di servire un sito che il dispositivo sarebbe stato in grado di gestire. I dispositivi mobili avevano spesso esperienze molto semplici e basilari servite loro perché era tutto ciò che potevano gestire. Al giorno d'oggi, i dispositivi mobili sono in grado di gestire le stesse tecnologie dei computer desktop, quindi tali tecniche sono meno comuni.
  - È comunque opportuno utilizzare le tecniche discusse in questo articolo per offrire agli utenti mobili un'esperienza adeguata, poiché ci sono ancora vincoli come la durata della batteria e la larghezza di banda di cui preoccuparsi.
  - Anche l'esperienza utente è una preoccupazione. Un utente mobile di un sito di viaggi potrebbe voler semplicemente controllare gli orari dei voli e le informazioni sui ritardi, ad esempio, e non vedersi presentato un globo animato in 3D che mostra le rotte di volo e la storia della tua azienda. Questo può essere gestito usando le tecniche di design responsive, tuttavia.
- Le tecnologie moderne sono molto migliori per creare esperienze responsive. Ad esempio, le [tecnologie per immagini/media responsive](#responsive_imagesmedia) ora permettono di servire media adeguati a diversi dispositivi senza dover fare affidamento su tecniche come lo sniffing lato server.

## Introduzione al web design responsive

HTML è fondamentalmente responsive, o _fluido_. Se si crea una pagina web contenente solo HTML, senza CSS, e si ridimensiona la finestra, il browser riallineerà automaticamente il testo per adattarsi al viewport.

Sebbene il comportamento responsive predefinito possa sembrare che non sia necessaria alcuna soluzione, lunghe righe di testo visualizzate a schermo intero su un monitor largo possono risultare difficili da leggere. Se la lunghezza delle linee dello schermo largo viene ridotta con CSS, ad esempio creando colonne o aggiungendo margini consistenti, il sito può apparire schiacciato per l'utente che restringe la finestra del browser o apre il sito su un dispositivo mobile.

![Un layout con due colonne compresse in un viewport di dimensioni mobili.](mdn-rwd-liquid.png)

Creare una pagina web non ridimensionabile impostando una larghezza fissa non funziona neanche; ciò porta a barre di scorrimento su dispositivi stretti e troppo spazio vuoto su schermi ampi.

Il web design responsive, o RWD, è un approccio di progettazione che affronta la gamma di dispositivi e dimensioni dei dispositivi, consentendo l'adattamento automatico allo schermo, sia che il contenuto venga visualizzato su un tablet, telefono, televisione o orologio.

Il web design responsive non è una tecnologia separata: è un approccio. È un termine usato per descrivere un insieme di migliori pratiche utilizzate per creare un layout che possa _rispondere_ a qualsiasi dispositivo utilizzato per visualizzare il contenuto.

Il termine _responsive design_, [coniato da Ethan Marcotte nel 2010](https://alistapart.com/article/responsive-web-design/), descriveva l'uso di griglie fluide, immagini fluide e media query per creare contenuti responsive.

All'epoca, la raccomandazione era di usare il `float` di CSS per il layout e le media query per interrogare la larghezza del browser, creando layout per diversi breakpoints. Le immagini fluide sono impostate in modo da non superare la larghezza del loro contenitore; hanno la proprietà `max-width` impostata su `100%`. Le immagini fluide si ridimensionano verso il basso quando la colonna contenente si restringe ma non crescono più del loro intrinseco quando la colonna si allarga. Ciò consente a un'immagine di ridimensionarsi per adattarsi al suo contenuto, piuttosto che traboccarlo, ma di non diventare più grande e pixellarsi se il contenitore diventa più largo dell'immagine.

I metodi di layout CSS moderni sono intrinsecamente responsive e, dalla pubblicazione dell'articolo di Marcotte, abbiamo una moltitudine di funzionalità integrate nella piattaforma web per facilitare la progettazione di siti responsive.

Il resto di questo articolo vi guiderà alle varie funzionalità della piattaforma web che potreste voler utilizzare quando create un sito responsive.

## Media Queries

Le [media query](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries) ci permettono di eseguire una serie di test (ad esempio, se lo schermo dell'utente è maggiore di una certa larghezza o risoluzione) e applicare CSS selettivamente per stilizzare la pagina in modo appropriato per le esigenze dell'utente.

Ad esempio, la seguente media query verifica se la pagina web corrente è visualizzata come media a schermo (quindi non un documento stampato) e il viewport è largo almeno `80rem`. Il CSS per il selettore `.container` verrà applicato solo se queste due condizioni sono vere.

```css
@media screen and (min-width: 80rem) {
  .container {
    margin: 1em 2em;
  }
}
```

Puoi aggiungere più media query all'interno di un foglio di stile, modificando l'intero layout o parti di esso per adattarlo al meglio alle diverse dimensioni dello schermo. I punti in cui viene introdotta una media query e il layout cambia sono noti come _breakpoints_.

Un approccio comune quando si usano le media query è creare un layout semplice a colonna singola per i dispositivi a schermo stretto (ad esempio, telefoni mobili), quindi verificare la presenza di schermi più larghi e implementare un layout a colonne multiple quando si sa di avere larghezza sufficiente dello schermo per gestirlo. Progettare per primo il mobile è noto come design **mobile first**.

Se si utilizzano breakpoints, le migliori pratiche incoraggiano la definizione di breakpoints di media query con [unità relative](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#relative_length_units) piuttosto che dimensioni assolute di un singolo dispositivo.

Ci sono diversi approcci per gli stili definiti all'interno di un blocco di media query; che vanno dall'uso delle media query per {{htmlelement("link")}} fogli di stile basati su intervalli di dimensioni del browser alla semplice inclusione di variabili di proprietà personalizzate per memorizzare i valori associati a ciascun breakpoint.

Le media query possono aiutare con RWD, ma non sono un requisito. Griglie flessibili, unità relative e valori di unità minimi e massimi possono essere utilizzati senza media query.

> [!NOTE]
> Scrimba ha un tutorial chiamato [Aside: Media queries](https://scrimba.com/frontend-path-c0j/~0j3?via=mdn) <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>, che fornisce un'introduzione interattiva alle media query oltre a una sfida per testare che si comprende i fondamenti.

## Tecnologie di layout responsive

I siti responsive sono costruiti su griglie flessibili, il che significa che non è necessario mirare a ogni possibile dimensione del dispositivo con layout perfettamente pixelati.

Usando una griglia flessibile, puoi modificare una caratteristica o aggiungere un breakpoint e cambiare il design nel punto in cui il contenuto inizia a sembrare brutto. Ad esempio, per garantire che le lunghezze delle linee non diventino illeggibili all'aumentare della dimensione dello schermo, puoi usare {{cssxref('columns')}}; se un riquadro diventa schiacciato con due parole su ogni riga mentre si restringe, puoi impostare un breakpoint.

Diversi metodi di layout — tra cui [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox) e [CSS Grid](/it/docs/Learn_web_development/Core/CSS_layout/Grids) — sono per impostazione predefinita responsive. Presumono tutti che tu stia cercando di creare una griglia flessibile e ti offrono modi più semplici per farlo.

### Flexbox

Nel flexbox, gli elementi flex si riducono o crescono, distribuendo lo spazio tra gli elementi in base allo spazio nel loro contenitore. Cambiando i valori di `flex-grow` e `flex-shrink` puoi indicare come desideri che gli elementi si comportino quando incontrano più o meno spazio intorno a loro.

Nell'esempio seguente, gli elementi flex prenderanno ciascuno una quantità uguale di spazio nel container flex, usando l'abbreviazione di `flex: 1` come discusso in precedenza (vedi [Flexbox: Flexible sizing of flex items](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox#flexible_sizing_of_flex_items)).

```css
.container {
  display: flex;
}

.item {
  flex: 1;
}
```

Ecco come potremmo utilizzare flexbox con una media query per il design responsive.

```html live-sample___flex-based-rwd
<div class="wrapper">
  <div class="col1">
    <p>
      This layout is responsive. See what happens if you make the browser window
      wider or narrow.
    </p>
  </div>
  <div class="col2">
    <p>
      One November night in the year 1782, so the story runs, two brothers sat
      over their winter fire in the little French town of Annonay, watching the
      grey smoke-wreaths from the hearth curl up the wide chimney. Their names
      were Stephen and Joseph Montgolfier, they were papermakers by trade, and
      were noted as possessing thoughtful minds and a deep interest in all
      scientific knowledge and new discovery.
    </p>
    <p>
      Before that night—a memorable night, as it was to prove—hundreds of
      millions of people had watched the rising smoke-wreaths of their fires
      without drawing any special inspiration from the fact.
    </p>
  </div>
</div>
```

```css hidden live-sample___flex-based-rwd
body {
  font: 1.2em / 1.5 sans-serif;
  margin: 20px;
  padding: 0;
  background-color: #eee;
}
.wrapper {
  max-width: 960px;
  margin: 2em auto;
}

.col1,
.col2 {
  background-color: #fff;
}
```

```css live-sample___flex-based-rwd
@media screen and (min-width: 600px) {
  .wrapper {
    display: flex;
  }

  .col1 {
    flex: 1;
    margin-right: 5%;
  }

  .col2 {
    flex: 2;
  }
}
```

{{EmbedLiveSample("flex-based-rwd", "", "550px")}}

Ridimensiona il tuo schermo. Il layout cambierà quando la dimensione dell'esempio sopra attraversa la soglia di larghezza di 600px.

### CSS grid

Nel layout CSS grid l'unità `fr` consente la distribuzione dello spazio disponibile tra le tracce di griglia. Il prossimo esempio crea un contenitore di griglia con tre tracce dimensionate a `1fr`. Questo creerà tre colonne di tracce, ciascuna prendendo una parte dello spazio disponibile nel contenitore. Hai già visto questo approccio (vedi [Flexible grids with the fr unit](/it/docs/Learn_web_development/Core/CSS_layout/Grids#flexible_grids_with_the_fr_unit) per un riepilogo).

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
```

Ecco come potremmo utilizzare il layout a griglia con una media query per il design responsive.

```html live-sample___grid-based-rwd
<div class="wrapper">
  <div class="col1">
    <p>
      This layout is responsive. See what happens if you make the browser window
      wider or narrow.
    </p>
  </div>
  <div class="col2">
    <p>
      One November night in the year 1782, so the story runs, two brothers sat
      over their winter fire in the little French town of Annonay, watching the
      grey smoke-wreaths from the hearth curl up the wide chimney. Their names
      were Stephen and Joseph Montgolfier, they were papermakers by trade, and
      were noted as possessing thoughtful minds and a deep interest in all
      scientific knowledge and new discovery.
    </p>
    <p>
      Before that night—a memorable night, as it was to prove—hundreds of
      millions of people had watched the rising smoke-wreaths of their fires
      without drawing any special inspiration from the fact.
    </p>
  </div>
</div>
```

```css hidden live-sample___grid-based-rwd
body {
  font: 1.2em / 1.5 sans-serif;
  margin: 20px;
  padding: 0;
  background-color: #eee;
}
.wrapper {
  max-width: 960px;
  margin: 2em auto;
}

.col1,
.col2 {
  background-color: #fff;
}
```

```css live-sample___grid-based-rwd
@media screen and (min-width: 600px) {
  .wrapper {
    display: grid;
    grid-template-columns: 1fr 2fr;
    column-gap: 5%;
  }
}
```

{{EmbedLiveSample("grid-based-rwd", "", "550px")}}

## Immagini/media responsive

Per garantire che i media non siano mai più grandi del loro contenitore responsive, si può usare il seguente approccio:

```css
img,
picture,
video {
  max-width: 100%;
}
```

Questo ridimensiona i media per garantire che non trabocchino mai dai loro contenitori.

> [!NOTE]
> Usare un'unica immagine grande e ridimensionarla per adattarla a dispositivi piccoli spreca larghezza di banda scaricando immagini più grandi del necessario. Può anche apparire male: un'immagine in formato paesaggio, ad esempio, potrebbe apparire bene su un monitor widescreen, ma potrebbe essere difficile da vedere su un dispositivo mobile, che si adatterebbe meglio a un'immagine verticale. Tali problemi possono essere risolti usando l'elemento {{htmlelement("picture")}} e gli attributi `srcset` e `sizes` di {{htmlelement("img")}}. Queste sono funzionalità avanzate che esulano dallo scopo di questo corso, ma è possibile trovare una guida dettagliata in [Immagini responsive](/it/docs/Web/HTML/Guides/Responsive_images).

Altri suggerimenti utili:

- Assicurarsi sempre di utilizzare un formato immagine appropriato per le immagini del tuo sito web (come PNG o JPG) e ottimizzare le dimensioni del file utilizzando un editor grafico prima di metterle sul tuo sito web.
- Puoi utilizzare caratteristiche CSS come [gradients](/it/docs/Web/CSS/CSS_images/Using_CSS_gradients) e [shadows](/it/docs/Web/CSS/box-shadow) per implementare effetti visivi senza usare immagini.
- Puoi usare le media query all'interno dell'attributo media su elementi {{htmlelement("source")}} annidati all'interno di elementi {{htmlelement("video")}}/{{htmlelement("audio")}} per servire file video/audio appropriati per diversi dispositivi (video/audio responsive).

## Tipografia responsive

La tipografia responsive descrive la modifica delle dimensioni dei caratteri all'interno delle media query o l'uso delle unità del viewport per riflettere quantità maggiori o minori di spazio sullo schermo.

### Utilizzo delle media query per la tipografia responsive

In questo esempio, vogliamo impostare il nostro titolo di livello 1 a `4rem`, il che significa che sarà quattro volte la nostra dimensione base del carattere. Questo è un titolo davvero grande! Vogliamo solo questo titolo jumbo su schermi di dimensioni maggiori, quindi creiamo prima un titolo più piccolo quindi utilizziamo le media query per sovrascriverlo con la dimensione più grande se sappiamo che l'utente ha una dimensione dello schermo di almeno `1200px`.

```css
html {
  font-size: 1em;
}

h1 {
  font-size: 2rem;
}

@media (min-width: 1200px) {
  h1 {
    font-size: 4rem;
  }
}
```

Abbiamo modificato il nostro esempio di griglia responsive sopra per includere anche un tipo responsive utilizzando il metodo sopra descritto. Puoi vedere come il titolo cambia dimensioni mentre il layout passa alla versione a due colonne.

Su mobile il titolo è più piccolo, ma su desktop vediamo la dimensione maggiore del titolo:

```html live-sample___type-rwd
<div class="wrapper">
  <div class="col1">
    <h1>Watch my size!</h1>
    <p>
      This layout is responsive. See what happens if you make the browser window
      wider or narrow.
    </p>
  </div>
  <div class="col2">
    <p>
      One November night in the year 1782, so the story runs, two brothers sat
      over their winter fire in the little French town of Annonay, watching the
      grey smoke-wreaths from the hearth curl up the wide chimney. Their names
      were Stephen and Joseph Montgolfier, they were papermakers by trade, and
      were noted as possessing thoughtful minds and a deep interest in all
      scientific knowledge and new discovery.
    </p>
    <p>
      Before that night—a memorable night, as it was to prove—hundreds of
      millions of people had watched the rising smoke-wreaths of their fires
      without drawing any special inspiration from the fact.
    </p>
  </div>
</div>
```

```css live-sample___type-rwd
html {
  font-size: 1em;
}

body {
  font:
    1.2em Helvetica,
    Arial,
    sans-serif;
  margin: 20px;
  padding: 0;
  background-color: #eee;
}
.wrapper {
  max-width: 960px;
  margin: 2em auto;
}

h1 {
  font-size: 2rem;
  margin: 0;
}

.col1,
.col2 {
  background-color: #fff;
}

@media screen and (min-width: 600px) {
  .wrapper {
    display: grid;
    grid-template-columns: 1fr 2fr;
    column-gap: 5%;
  }

  h1 {
    font-size: 4rem;
  }
}
```

{{EmbedLiveSample("type-rwd", "", "550px")}}

Come mostra questo approccio alla tipografia, non è necessario limitare le media query solo al cambiamento del layout della pagina. Possono essere utilizzate per modificare qualsiasi elemento per renderlo più utilizzabile o attraente a dimensioni di schermo alternative.

### Utilizzo delle unità del viewport per la tipografia responsive

Le unità del viewport `vw` possono essere utilizzate anche per abilitare la tipografia responsive, senza la necessità di impostare breakpoint con le media query. `1vw` è uguale a un percento della larghezza del viewport, il che significa che se imposti la dimensione del carattere utilizzando `vw`, sarà sempre correlata alla dimensione del viewport.

```css
h1 {
  font-size: 6vw;
}
```

Il problema con quanto sopra è che l'utente perde la capacità di ingrandire qualsiasi testo impostato utilizzando l'unità `vw`, poiché quel testo è sempre legato alla dimensione del viewport. **Pertanto, non si dovrebbe mai impostare il testo utilizzando solo le unità del viewport**.

C'è una soluzione, e comporta l'uso di [`calc()`](/it/docs/Web/CSS/calc). Se aggiungi l'unità `vw` a un valore impostato utilizzando una dimensione fissa come `em` o `rem`, il testo sarà ancora ingrandibile. In sostanza, l'unità `vw` si aggiunge a tale valore zoomato:

```css
h1 {
  font-size: calc(1.5rem + 4vw);
}
```

Questo significa che dobbiamo specificare la dimensione del carattere per il titolo solo una volta, piuttosto che impostarlo per il mobile e ridefinirlo nelle media query. Il carattere poi aumenta gradualmente man mano che si aumenta la dimensione del viewport.

```html live-sample___type-vw
<div class="wrapper">
  <div class="col1">
    <h1>Watch my size!</h1>
    <p>
      This layout is responsive. See what happens if you make the browser window
      wider or narrow.
    </p>
  </div>
  <div class="col2">
    <p>
      One November night in the year 1782, so the story runs, two brothers sat
      over their winter fire in the little French town of Annonay, watching the
      grey smoke-wreaths from the hearth curl up the wide chimney. Their names
      were Stephen and Joseph Montgolfier, they were papermakers by trade, and
      were noted as possessing thoughtful minds and a deep interest in all
      scientific knowledge and new discovery.
    </p>
  </div>
</div>
```

```css live-sample___type-vw
body {
  font: 1.2em / 1.5 sans-serif;
  margin: 20px;
  padding: 0;
  background-color: #eee;
}

.wrapper {
  max-width: 960px;
  margin: 2em auto;
}

h1 {
  font-size: calc(1.5rem + 4vw);
  margin: 0;
}

.col1,
.col2 {
  background-color: #fff;
}

@media screen and (min-width: 600px) {
  .wrapper {
    display: grid;
    grid-template-columns: 1fr 2fr;
    column-gap: 5%;
  }
}
```

{{EmbedLiveSample("type-vw", "", "550px")}}

## Il meta tag viewport

Se guardi il codice sorgente HTML di una pagina responsive, vedrai di solito il seguente tag {{htmlelement("meta")}} nel `<head>` del documento.

```html
<meta name="viewport" content="width=device-width,initial-scale=1" />
```

Questo [viewport](/it/docs/Web/HTML/Guides/Viewport_meta_element) meta tag comunica ai browser mobili che dovrebbero impostare la larghezza del viewport alla larghezza del dispositivo, e scalare il documento al 100% della dimensione prevista, che mostra il documento nella dimensione ottimizzata per i dispositivi mobili che intendevi.

Perché è necessario? Perché i browser mobili tendono a mentire sulla larghezza del loro viewport.

Questo meta tag esiste perché quando sono arrivati i primi smartphone, la maggior parte dei siti non era ottimizzata per dispositivi mobili. Il browser mobile avrebbe quindi impostato la larghezza del viewport a 980 pixel, visualizzando la pagina a tale larghezza, e mostrato il risultato come una versione ridotta del layout desktop. Gli utenti potevano ingrandire e navigare all'interno del sito per visualizzare le sezioni di loro interesse, ma appariva male.

Impostando `width=device-width` stai sovrascrivendo un'impostazione predefinita del dispositivo mobile, come il `width=980px` predefinito di Apple, con la larghezza effettiva del dispositivo. Senza di esso, il tuo design responsive con breakpoints e media query potrebbe non funzionare come previsto sui browser mobili. Se hai un layout a schermo stretto che si attiva con un viewport di larghezza inferiore a 480px, ma il dispositivo dice che è largo 980px, quell'utente non vedrà il tuo layout a schermo stretto.

**Quindi dovresti _sempre_ includere il meta tag viewport nell'head dei tuoi documenti.**

## Sommario

Il design responsive si riferisce a un design di sito o applicazione che risponde all'ambiente in cui viene visualizzato. Comprende una serie di caratteristiche e tecniche CSS e HTML ed è essenzialmente ora il modo in cui costruiamo i siti web da default. Considera i siti che visiti sul tuo telefono: è probabilmente piuttosto insolito imbattersi in un sito che è la versione desktop ridotta, o dove devi scorrere lateralmente per trovare le cose. Questo è perché il web si è spostato su questo approccio di progettazione in modo responsive.

È anche diventato molto più facile ottenere design responsive con l'aiuto dei metodi di layout che hai imparato in queste lezioni. Se sei nuovo alla sviluppo web oggi hai molti più strumenti a disposizione rispetto ai primi giorni del design responsive. Vale quindi la pena controllare l'età di qualsiasi materiale stai utilizzando. Mentre gli articoli storici sono ancora utili, l'uso moderno di CSS e HTML rende molto più facile creare design eleganti e utili, a prescindere dal dispositivo con cui il tuo visitatore visualizza il sito.

Successivamente, studieremo le media query in modo più dettagliato e mostreremo come utilizzarle per risolvere alcuni problemi comuni.

## Vedi anche

- Lavorare con dispositivi touchscreen:
  - [Touch events](/it/docs/Web/API/Touch_events) forniscono la possibilità di interpretare l'attività del dito (o stilo) su schermi touch o trackpad, abilitando un supporto di qualità per interfacce utente complesse basate sul tocco.
  - Usa le [pointer](/it/docs/Web/CSS/@media/pointer) o [any-pointer](/it/docs/Web/CSS/@media/any-pointer) media query per caricare diversi CSS su dispositivi abilitati al tocco.
- [Guida di CSS-Tricks alle media query](https://css-tricks.com/a-complete-guide-to-css-media-queries/)
- [The Frontend Developer Career Path](https://scrimba.com/the-frontend-developer-career-path-c0j?via=mdn) <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> di Scrimba insegna tutto ciò che serve sapere per essere un sviluppatore web front-end competente, con lezioni e sfide interattive divertenti, insegnanti competenti e una comunità di supporto. Passa da zero a ottenere il tuo primo lavoro front-end! Molti dei componenti del corso sono disponibili come versioni gratuite autonome. Questo include un modulo sul design responsive.

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Grids", "Learn_web_development/Core/CSS_layout/Media_queries", "Learn_web_development/Core/CSS_layout")}}
