---
title: Responsive design
slug: Learn_web_development/Core/CSS_layout/Responsive_Design
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Grids", "Learn_web_development/Core/CSS_layout/Media_queries", "Learn_web_development/Core/CSS_layout")}}

Il _Responsive web design_ (RWD) è un approccio alla progettazione di siti web per garantire che le pagine web vengano visualizzate correttamente su tutti i formati e risoluzioni dello schermo, garantendo al contempo una buona usabilità. È il modo di progettare per un web multi-dispositivo. In questo articolo, la aiuteremo a comprendere alcune tecniche che possono essere utilizzate per padroneggiarlo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS Styling basics</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Fundamentali di stile di testo e font</a>, familiarità con i <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">concetti fondamentali del layout CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Che cos'è il responsive design — progettare layout web in modo che siano flessibili e funzionino bene su diverse dimensioni di schermo, risoluzioni, ecc.</li>
          <li>La relazione tra strumenti di layout moderni come grid e flexbox e il responsive design.</li>
          <li>I concetti dietro l'uso delle media queries per il responsive design, inclusi il mobile-first e i breakpoints.</li>
          <li>Perché il <code>&lt;meta viewport=""&gt;</code> è necessario per visualizzare correttamente i documenti web sui dispositivi mobili.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Precursore del responsive design: progettazione web mobile

Prima che il responsive web design diventasse l'approccio standard per far funzionare i siti web su diversi tipi di dispositivi, gli sviluppatori web parlavano di progettazione web mobile, sviluppo web mobile, o talvolta, progettazione mobile-friendly. Sono fondamentalmente la stessa cosa del responsive web design: l'obiettivo è garantire che i siti web funzionino bene su dispositivi con diverse caratteristiche fisiche (dimensioni dello schermo, risoluzione) in termini di layout, contenuti (testi e media) e prestazioni.

La differenza è principalmente legata ai dispositivi coinvolti e alle tecnologie disponibili per creare soluzioni:

- Si parlava di desktop o di mobile, ma ora ci sono molti tipi di dispositivi disponibili come desktop, laptop, mobile, tablet, orologi, ecc. Invece di adattarsi a poche dimensioni di schermo, ora dobbiamo progettare siti in modo difensivo per adattarsi alle dimensioni comuni e alle risoluzioni degli schermi, inoltre agli sconosciuti.
- I dispositivi mobili erano a bassa potenza in termini di CPU/GPU e larghezza di banda disponibile. Alcuni non supportavano CSS o anche HTML, e di conseguenza era comune eseguire sniffing del browser lato server per determinare il tipo di dispositivo/browser prima di fornire un sito con il quale il dispositivo sarebbe in grado di gestire. I dispositivi mobili spesso avevano esperienze molto semplici e basilari servite a loro perché era tutto ciò che potevano gestire. Oggi, i dispositivi mobili sono in grado di gestire le stesse tecnologie dei computer desktop, quindi tali tecniche sono meno comuni.
  - Dovrebbe comunque utilizzare le tecniche discusse in questo articolo per fornire agli utenti mobili un'esperienza adatta, poiché ci sono ancora vincoli come la durata della batteria e la larghezza di banda da considerare.
  - Anche l'esperienza utente è una preoccupazione. Un utente mobile di un sito di viaggi potrebbe voler solo controllare gli orari dei voli e le informazioni sui ritardi, ad esempio, e non essere presentato con un globo animato in 3D con percorsi di volo e la storia della sua azienda. Ciò può essere gestito utilizzando tecniche di responsive design.
- Le tecnologie moderne sono molto migliori per creare esperienze responsive. Ad esempio, le [tecnologie per immagini/media responsive](#responsive_imagesmedia) ora consentono di servire contenuti appropriati a diversi dispositivi senza dover fare affidamento su tecniche come lo sniffing lato server.

## Introduzione al responsive web design

L'HTML è intrinsecamente reattivo, o _fluido_. Se crea una pagina web contenente solo HTML, senza CSS, e ridimensiona la finestra, il browser adeguerà automaticamente il testo per adattarsi al viewport.

Sebbene il comportamento responsivo predefinito possa sembrare che non sia necessaria alcuna soluzione, le righe lunghe di testo visualizzate a schermo intero su un monitor largo possono essere difficili da leggere. Se la lunghezza delle righe a schermo largo viene ridotta con CSS, ad esempio creando colonne o aggiungendo un padding significativo, il sito potrebbe sembrare schiacciato per l'utente che riduce la finestra del browser o apre il sito su un dispositivo mobile.

![Un layout con due colonne schiacciato in un viewport di dimensioni mobili.](mdn-rwd-liquid.png)

Creare una pagina web non ridimensionabile impostando una larghezza fissa non funziona nemmeno; ciò porta a barre di scorrimento su dispositivi stretti e troppo spazio vuoto su schermi larghi.

Il responsive web design, o RWD, è un approccio di progettazione che affronta la gamma di dispositivi e dimensioni dei dispositivi, consentendo un adattamento automatico allo schermo, indipendentemente dal fatto che il contenuto sia visualizzato su un tablet, telefono, televisione o orologio.

Il responsive web design non è una tecnologia a sé stante — è un approccio. È un termine utilizzato per descrivere un insieme di best practice utilizzate per creare un layout che può _rispondere_ a qualsiasi dispositivo utilizzato per visualizzare il contenuto.

Il termine _responsive design_, [coniato da Ethan Marcotte nel 2010](https://alistapart.com/article/responsive-web-design/), descriveva l'uso di griglie fluide, immagini fluide e media queries per creare contenuti responsive.

All'epoca, la raccomandazione era di utilizzare il `float` CSS per il layout e le media queries per interrogare la larghezza del browser, creando layout per diversi breakpoints. Le immagini fluide sono impostate per non superare la larghezza del loro contenitore; hanno la proprietà `max-width` impostata al `100%`. Le immagini fluide si riducono quando la loro colonna contenente si restringe ma non crescono più grandi della loro dimensione intrinseca quando la colonna si espande. Questo consente a un'immagine di adattarsi al contenitore, piuttosto che traboccare, ma non crescere e diventare pixellata se il contenitore diventa più largo dell'immagine.

I metodi di layout CSS moderni sono intrinsecamente reattivi e, dalla pubblicazione dell'articolo di Marcotte, abbiamo molteplici funzionalità integrate nella piattaforma web per rendere più semplice la progettazione di siti responsive.

Il resto di questo articolo la indirizzerà alle varie funzionalità della piattaforma web che potrebbe voler utilizzare quando crea un sito responsive.

## Media Queries

[Le media queries](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries) ci permettono di eseguire una serie di test (per esempio, se lo schermo dell'utente supera una certa larghezza o risoluzione) e applicare CSS selettivamente per stilizzare la pagina in modo appropriato per le esigenze dell'utente.

Ad esempio, la seguente media query verifica se la pagina web corrente è visualizzata come supporto per schermo (quindi non un documento stampato) e se il viewport è di almeno `80rem` di larghezza. Il CSS per il selettore `.container` verrà applicato solo se queste due condizioni sono vere.

```css
@media screen and (min-width: 80rem) {
  .container {
    margin: 1em 2em;
  }
}
```

Può aggiungere più media queries all'interno di un foglio di stile, modificando l'intero layout o parti di esso per adattarsi meglio alle varie dimensioni dello schermo. I punti in cui viene introdotta una media query e il layout cambia, sono noti come _breakpoints_.

Un approccio comune quando si usano le media queries è creare un semplice layout a colonna singola per dispositivi con schermi stretti (ad esempio, telefoni cellulari), quindi controllare per schermi più larghi e implementare un layout a colonne multiple quando sa che ha abbastanza larghezza di schermo per gestirlo. Progettare per il mobile first è noto come design **mobile-first**.

Se si usano breakpoints, le best practice incoraggiano a definire i breakpoints delle media query con unità relative piuttosto che dimensioni assolute di un dispositivo individuale.

Ci sono diversi approcci agli stili definiti all'interno di un blocco di media query; che vanno dall'uso delle media queries per {{htmlelement("link")}} fogli di stile basati su intervalli di dimensioni del browser all'inclusione solo di variabili di proprietà personalizzate per memorizzare valori associati a ciascun breakpoint.

Le media queries possono essere di aiuto con il RWD, ma non sono un requisito. Griglie flessibili, unità relative e valori di unità minima e massima possono essere usate senza media queries.

## Tecnologie di layout responsive

I siti responsive sono costruiti su griglie flessibili, il che significa che non è necessario mirare a ogni possibile dimensione del dispositivo con layout perfettamente pixelati.

Utilizzando una griglia flessibile, può cambiare una funzionalità o aggiungere un breakpoint e modificare il design nel punto in cui il contenuto inizia a sembrare brutto. Ad esempio, per garantire che le lunghezze delle righe non diventino illeggibili man mano che aumenta la dimensione dello schermo, può utilizzare {{cssxref('columns')}}; se una casella diventa schiacciata con due parole su ogni riga mentre si restringe, può impostare un breakpoint.

Diversi metodi di layout, tra cui [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox), e [CSS Grid](/it/docs/Learn_web_development/Core/CSS_layout/Grids), sono di default responsive. Presuppongono tutti che stia cercando di creare una griglia flessibile e le offrono modi più semplici per farlo.

### Flexbox

Nel flexbox, gli elementi flessibili si restringono o crescono, distribuendo lo spazio tra gli elementi in base allo spazio nel loro contenitore. Modificando i valori per `flex-grow` e `flex-shrink` può indicare come vuole che gli elementi si comportino quando incontrano più o meno spazio intorno a loro.

Nell'esempio seguente, gli elementi flex prenderanno ciascuno una quantità uguale di spazio nel contenitore flex, utilizzando l'abbreviazione `flex: 1` come discusso in precedenza (vedi [Flexbox: dimensionamento flessibile degli elementi flex](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox#flexible_sizing_of_flex_items)).

```css
.container {
  display: flex;
}

.item {
  flex: 1;
}
```

Ecco come potremmo usare il flexbox con una media query per il responsive design.

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

Ridimensioni il suo schermo. Il layout cambierà quando la dimensione dell'esempio sopra supererà la soglia di larghezza di 600px.

### CSS grid

Nel layout della griglia CSS l'unità `fr` permette la distribuzione dello spazio disponibile tra le tracce della griglia. Il prossimo esempio crea un contenitore a griglia con tre tracce dimensionate a `1fr`. Ciò creerà tre tracce di colonna, ciascuna delle quali prende una parte dello spazio disponibile nel contenitore. Ha già visto questo approccio (vedi [Griglie flessibili con l'unità fr](/it/docs/Learn_web_development/Core/CSS_layout/Grids#flexible_grids_with_the_fr_unit) per un ripasso).

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
```

Ecco come potremmo usare il layout a griglia con una media query per il responsive design.

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

Per garantire che i media non siano mai più grandi del loro contenitore responsive, può essere utilizzato il seguente approccio:

```css
img,
picture,
video {
  max-width: 100%;
}
```

Questo scala i media per garantire che non superino mai i loro contenitori.

> [!NOTE]
> Utilizzare una singola immagine grande e ridurla per adattarsi ai dispositivi piccoli spreca larghezza di banda scaricando immagini più grandi del necessario. Può anche sembrare male: un'immagine paesaggistica, ad esempio, potrebbe sembrare buona su un monitor widescreen, ma potrebbe essere difficile da vedere su un dispositivo mobile, il quale potrebbe adattarsi meglio a un'immagine ritratto. Tali problemi possono essere risolti utilizzando l'elemento {{htmlelement("picture")}} e gli attributi {{htmlelement("img")}} `srcset` e `sizes`. Queste sono funzionalità avanzate che vanno oltre lo scopo di questo corso, ma può trovare una guida dettagliata su [Immagini responsive](/it/docs/Web/HTML/Guides/Responsive_images).

Altri suggerimenti utili:

- Assicurarsi sempre di utilizzare un formato di immagine appropriato per le immagini del suo sito web (come PNG o JPG), e assicurarsi di ottimizzare la dimensione del file usando un editor grafico prima di metterle sul suo sito web.
- Può fare uso di funzionalità CSS come [sfumature](/it/docs/Web/CSS/CSS_images/Using_CSS_gradients) e [ombre](/it/docs/Web/CSS/box-shadow) per implementare effetti visivi senza utilizzare immagini.
- Può usare media queries all'interno dell'attributo media sugli elementi {{htmlelement("source")}} annidati all'interno degli elementi {{htmlelement("video")}}/{{htmlelement("audio")}} per servire file video/audio appropriati per diversi dispositivi (video/audio responsive).

## Tipografia responsive

La tipografia responsive descrive il cambiamento delle dimensioni dei caratteri all'interno delle media queries o l'uso delle unità di viewport per riflettere minori o maggiori quantità di spazio sullo schermo.

### Usare le media queries per la tipografia responsive

In questo esempio, vogliamo impostare la nostra intestazione di livello 1 su `4rem`, il che significa che sarà quattro volte la nostra dimensione del carattere di base. È un'intestazione davvero grande! Vogliamo questa intestazione jumbo solo su schermi di dimensioni maggiori, quindi prima creiamo un'intestazione più piccola, poi usiamo le media queries per sovrascriverla con la dimensione maggiore se sappiamo che l'utente ha uno schermo di almeno `1200px`.

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

Abbiamo modificato il nostro esempio di griglia responsive sopra per includere anche un tipo responsive utilizzando il metodo descritto. Può vedere come l'intestazione cambia dimensioni quando il layout passa alla versione a due colonne.

Su mobile l'intestazione è più piccola, ma su desktop vediamo la dimensione maggiore dell'intestazione:

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

Come mostra questo approccio alla tipografia, non è necessario limitare le media queries solo a cambiare il layout della pagina. Possono essere utilizzate per modificare qualsiasi elemento per renderlo più utilizzabile o attraente a dimensioni di schermo alternative.

### Usare le unità di viewport per la tipografia responsive

Le unità di viewport `vw` possono essere utilizzate per abilitare la tipografia responsive, senza bisogno di impostare breakpoints con media queries. `1vw` è uguale all'uno percento della larghezza del viewport, il che significa che se imposta la dimensione del carattere usando `vw`, essa sarà sempre correlata alla dimensione del viewport.

```css
h1 {
  font-size: 6vw;
}
```

Il problema con quanto sopra è che l'utente perde la capacità di fare zoom su qualsiasi testo impostato usando l'unità `vw`, poiché quel testo è sempre correlato alla dimensione del viewport. **Perciò non dovrebbe mai impostare il testo utilizzando solo unità di viewport**.

C'è una soluzione, e coinvolge l'uso di [`calc()`](/it/docs/Web/CSS/calc). Se aggiunge l'unità `vw` a un valore impostato usando una dimensione fissa come `em` o `rem`, il testo sarà comunque zoomabile. Essenzialmente, l'unità `vw` si aggiunge a quel valore zoomato:

```css
h1 {
  font-size: calc(1.5rem + 4vw);
}
```

Questo significa che dobbiamo specificare la dimensione del carattere per l'intestazione una sola volta, piuttosto che configurarla per il mobile e ridefinirla nelle media queries. Il font quindi aumenta gradualmente man mano che aumenta la dimensione del viewport.

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

Se guarda il sorgente HTML di una pagina responsive, vedrà di solito il seguente tag {{htmlelement("meta")}} nell`<head>` del documento.

```html
<meta name="viewport" content="width=device-width,initial-scale=1" />
```

Questo [tag meta del viewport](/it/docs/Web/HTML/Guides/Viewport_meta_element) indica ai browser mobili che dovrebbero impostare la larghezza del viewport sulla larghezza del dispositivo, e ridimensionare il documento al 100% della sua dimensione prevista, il che mostra il documento alla dimensione ottimizzata per i dispositivi mobili che ha inteso.

Perché è necessario? Perché i browser mobili tendono a mentire sulla larghezza del loro viewport.

Questo meta tag esiste perché quando gli smartphone sono arrivati per la prima volta, la maggior parte dei siti non era ottimizzata per i dispositivi mobili. Il browser mobile quindi impostava la larghezza del viewport a 980 pixel, renderizzava la pagina a quella larghezza e mostrava il risultato come una versione ridotta del layout desktop. Gli utenti potevano ingrandire e navigare nel sito web per vedere le parti di loro interesse, ma sembrava brutto.

Impostando `width=device-width` si sovrascrive il default di un dispositivo mobile, come il `width=980px` predefinito di Apple, con la larghezza effettiva del dispositivo. Senza di esso, il suo responsive design con breakpoints e media queries potrebbe non funzionare come previsto sui browser mobili. Se ha un layout per schermi stretti che si attiva a larghezza di viewport di 480px o meno, ma il dispositivo dice di essere largo 980px, quell'utente non vedrà il suo layout per schermi stretti.

**Quindi dovrebbe _sempre_ includere il meta tag viewport nella testa dei suoi documenti.**

## Sommario

Il design responsivo si riferisce a un design di un sito o di un'applicazione che risponde all'ambiente in cui viene visualizzato. Comprende un numero di funzionalità e tecniche CSS e HTML ed è ora essenzialmente solo il modo in cui costruisce i siti web per impostazione predefinita. Considere i siti che visita sul suo telefono: probabilmente è abbastanza insolito imbattersi in un sito che è la versione desktop ridotta, o dove è necessario scorrere lateralmente per trovare le cose. Questo perché il web si è spostato su questo approccio di progettazione responsiva.

È diventato anche molto più facile ottenere design responsivi con l'aiuto dei metodi di layout che ha imparato in queste lezioni. Se è nuovo nel web development oggi, ha molti più strumenti a sua disposizione rispetto agli albori del design responsivo. Vale quindi la pena controllare l'età di qualsiasi materiale che sta utilizzando. Mentre gli articoli storici sono ancora utili, l'uso moderno di CSS e HTML rende molto più facile creare design eleganti e utili, indipendentemente dal dispositivo con cui il suo visitatore visualizza il sito.

Successivamente, studieremo le media queries in modo più dettagliato e mostreremo come usarle per risolvere alcuni problemi comuni.

## Vedi anche

- Lavorare con dispositivi touchscreen:
  - [Eventi touch](/it/docs/Web/API/Touch_events) forniscono la capacità di interpretare l'attività del dito (o dello stilo) sugli schermi tattili o sui trackpad, abilitando il supporto di qualità per le interfacce utente complesse basate sul tocco.
  - Usare le media queries [pointer](/it/docs/Web/CSS/@media/pointer) o [any-pointer](/it/docs/Web/CSS/@media/any-pointer) per caricare diversi CSS sui dispositivi touch-enabled.
- [Guida CSS-Tricks alle media queries](https://css-tricks.com/a-complete-guide-to-css-media-queries/)

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Grids", "Learn_web_development/Core/CSS_layout/Media_queries", "Learn_web_development/Core/CSS_layout")}}
