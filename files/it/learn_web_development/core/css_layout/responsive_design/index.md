---
title: Progettazione responsiva
slug: Learn_web_development/Core/CSS_layout/Responsive_Design
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Grids", "Learn_web_development/Core/CSS_layout/Media_queries", "Learn_web_development/Core/CSS_layout")}}

La _progettazione web responsiva_ (RWD) è un approccio alla progettazione web per rendere le pagine web ben visualizzabili su tutte le dimensioni e risoluzioni dello schermo garantendo una buona usabilità. È il modo di progettare per il web multi-dispositivo. In questo articolo, ti aiuteremo a comprendere alcune tecniche che possono essere utilizzate per padroneggiarlo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare il contenuto con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi di styling CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Elementi fondamentali dello stile del testo e dei font</a>,
        familiarità con i <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">concetti fondamentali del layout CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Che cos'è la progettazione responsiva — progettare layout web in modo che siano flessibili e funzionino bene su diverse dimensioni di schermo, risoluzioni, ecc.</li>
          <li>La relazione tra strumenti di layout moderni come grid e flexbox e la progettazione responsiva.</li>
          <li>I concetti alla base dell'uso delle media query per la progettazione responsiva, inclusi il mobile-first e i breakpoints.</li>
          <li>Perché <code>&lt;meta viewport=""&gt;</code> è necessario per visualizzare correttamente i documenti web sui dispositivi mobili.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Precursore della progettazione responsiva: progettazione web per dispositivi mobili

Prima che la progettazione web responsiva diventasse l'approccio standard per far funzionare i siti web su diversi tipi di dispositivi, gli sviluppatori web parlavano di progettazione web per dispositivi mobili, sviluppo web per dispositivi mobili, o talvolta, progettazione mobile-friendly. Questi concetti sono sostanzialmente gli stessi della progettazione web responsiva — gli obiettivi sono di assicurarsi che i siti web funzionino bene su dispositivi con diverse caratteristiche fisiche (dimensioni dello schermo, risoluzione) in termini di layout, contenuto (testo e media), e prestazioni.

La differenza riguarda principalmente i dispositivi coinvolti e le tecnologie disponibili per creare soluzioni:

- Si parlava di desktop o mobile, ma ora ci sono molti diversi tipi di dispositivi disponibili come desktop, laptop, mobile, tablet, orologi, ecc. Invece di soddisfare poche dimensioni di schermo diverse, ora dobbiamo progettare siti in modo difensivo per soddisfare comuni dimensioni di schermo e risoluzioni, più gli sconosciuti.
- I dispositivi mobili erano poco potenti in termini di CPU/GPU e larghezza di banda disponibile. Alcuni non supportavano CSS o persino HTML, e di conseguenza, era comune eseguire lo sniffing del browser server-side per determinare il tipo di dispositivo/browser prima di servire un sito che il dispositivo sarebbe stato in grado di gestire. I dispositivi mobili ricevevano spesso esperienze davvero semplici e basilari perché era tutto ciò che potevano gestire. Al giorno d'oggi, i dispositivi mobili sono in grado di gestire le stesse tecnologie dei computer desktop, quindi tali tecniche sono meno comuni.
  - Dovresti comunque usare le tecniche discusse in questo articolo per offrire agli utenti mobili un'esperienza adeguata, poiché ci sono ancora vincoli come la durata della batteria e la larghezza di banda da preoccupare.
  - Anche l'esperienza utente è un problema. Un utente mobile di un sito di viaggi potrebbe voler semplicemente controllare gli orari dei voli e le informazioni sui ritardi, per esempio, e non avere una sfera animata in 3D che mostra le rotte dei voli e la storia della tua azienda. Questo può essere gestito usando tecniche di progettazione responsiva, tuttavia.
- Le tecnologie moderne sono molto migliori per creare esperienze responsive. Ad esempio, le [tecnologie per immagini/media responsivi](#responsive_imagesmedia) ora consentono di servire media appropriati a diversi dispositivi senza dover fare affidamento su tecniche come lo sniffing server-side.

## Introduzione alla progettazione web responsiva

HTML è fondamentalmente responsive, o _fluido_. Se crei una pagina web contenente solo HTML, senza CSS, e ridimensioni la finestra, il browser riadatterà automaticamente il testo per adattarsi alla finestra.

Sebbene il comportamento responsive predefinito possa sembrare che non sia necessaria alcuna soluzione, lunghe righe di testo visualizzate a schermo intero su un monitor largo possono essere difficili da leggere. Se la lunghezza della riga a schermo largo è ridotta con CSS, ad esempio creando colonne o aggiungendo un padding significativo, il sito potrebbe sembrare schiacciato per l'utente che restringe la finestra del browser o apre il sito su un dispositivo mobile.

![Un layout con due colonne schiacciate in un viewport di dimensioni mobili.](mdn-rwd-liquid.png)

Creare una pagina web non ridimensionabile impostando una larghezza fissa non funziona neanche; questo porta barre di scorrimento su dispositivi stretti e troppo spazio vuoto su schermi ampi.

La progettazione web responsiva, o RWD, è un approccio di design che affronta la gamma di dispositivi e dimensioni dei dispositivi, permettendo un adattamento automatico allo schermo, indipendentemente dal fatto che il contenuto venga visualizzato su un tablet, telefono, televisione o orologio.

La progettazione web responsiva non è una tecnologia separata — è un approccio. È un termine usato per descrivere un insieme di migliori pratiche utilizzate per creare un layout che può _rispondere_ a qualsiasi dispositivo utilizzato per visualizzare il contenuto.

Il termine _responsive design_, [coniato da Ethan Marcotte nel 2010](https://alistapart.com/article/responsive-web-design/), descriveva l'uso di griglie fluide, immagini fluide e media query per creare contenuti responsivi.

All'epoca, la raccomandazione era di utilizzare CSS `float` per il layout e media query per interrogare la larghezza del browser, creando layout per diversi breakpoints. Le immagini fluide sono impostate per non superare la larghezza del loro contenitore; hanno la loro proprietà `max-width` impostata su `100%`. Le immagini fluide si ridimensionano quando la loro colonna contenitore si restringe ma non crescono più grandi della loro dimensione intrinseca quando la colonna cresce. Questo permette a un'immagine di ridimensionarsi per adattarsi al suo contenuto, piuttosto che traboccarlo, ma non cresce più grande e diventare pixelata se il contenitore diventa più largo dell'immagine.

I moderni metodi di layout CSS sono intrinsecamente responsivi e, dalla pubblicazione dell'articolo di Marcotte, abbiamo una moltitudine di funzionalità integrate nella piattaforma web per rendere più facile progettare siti responsivi.

Il resto di questo articolo ti indirizzerà alle varie funzionalità della piattaforma web che potresti voler utilizzare quando crei un sito responsive.

## Media Queries

[Le media query](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries) ci permettono di eseguire una serie di test (ad esempio, se lo schermo dell'utente è maggiore di una certa larghezza o risoluzione) e applicare CSS in modo selettivo per stilizzare la pagina in modo appropriato alle esigenze dell'utente.

Ad esempio, la seguente media query testa se l'attuale pagina web viene visualizzata come media screen (quindi non un documento stampato) e se la finestra ha almeno `80rem` di larghezza. Il CSS per il selettore `.container` verrà applicato solo se queste due condizioni sono vere.

```css
@media screen and (min-width: 80rem) {
  .container {
    margin: 1em 2em;
  }
}
```

Puoi aggiungere multiple media query all'interno di un foglio di stile, modificando l'intero layout o parti di esso per adattarsi meglio alle varie dimensioni di schermo. I punti in cui viene introdotta una media query e il layout viene cambiato, sono noti come _breakpoint_.

Un approccio comune quando si utilizzano le media query è creare un layout semplice a colonna singola per dispositivi con schermo stretto (ad esempio, telefoni cellulari), quindi verificare la presenza di schermi più ampi e implementare un layout a più colonne quando sai di avere abbastanza larghezza di schermo per gestirlo. Progettare prima per i dispositivi mobili è conosciuto come design **mobile first**.

Se si usano breakpoints, le migliori pratiche incoraggiano a definire i breakpoints delle media query con [unità relative](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#relative_length_units) piuttosto che dimensioni assolute di un singolo dispositivo.

Ci sono diversi approcci agli stili definiti all'interno di un blocco di media query; che vanno dall'uso delle media query per {{htmlelement("link")}} fogli di stile basati su intervalli di dimensioni del browser a includere solo variabili di proprietà personalizzate per memorizzare i valori associati a ciascun breakpoint.

Le media query possono aiutare con il RWD, ma non sono un requisito. Griglie flessibili, unità relative e valori di unità minimi e massimi possono essere usati senza media query.

## Tecnologie di layout responsive

I siti responsivi sono costruiti su griglie flessibili, il che significa che non hai bisogno di mirare a ogni possibile dimensione di dispositivo con layout al pixel perfetto.

Utilizzando una griglia flessibile, puoi cambiare una caratteristica o aggiungere un breakpoint e cambiare il design nel punto in cui il contenuto inizia ad apparire male. Ad esempio, per garantire che la lunghezza delle righe non diventi illeggibile man mano che aumenta la dimensione dello schermo, puoi usare {{cssxref('columns')}}; se una casella diventa schiacciata con due parole su ogni riga mentre si restringe, puoi impostare un breakpoint.

Diversi metodi di layout — compresi [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox), e [CSS Grid](/it/docs/Learn_web_development/Core/CSS_layout/Grids) — sono responsivi per default. Presuppongono tutti che tu stia cercando di creare una griglia flessibile e ti offrono modi più semplici per farlo.

### Flexbox

In Flexbox, gli elementi flessibili si restringono o crescono, distribuendo lo spazio tra gli elementi in base allo spazio nel loro contenitore. Modificando i valori per `flex-grow` e `flex-shrink`, puoi indicare come vuoi che gli elementi si comportino quando incontrano più o meno spazio intorno a loro.

Nell'esempio seguente, gli elementi flessibili prenderanno ciascuno una quantità uguale di spazio nel contenitore flessibile, usando la sintassi abbreviata di `flex: 1` come discusso in precedenza (vedi [Flexbox: Dimensionamento flessibile degli elementi flex](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox#flexible_sizing_of_flex_items)).

```css
.container {
  display: flex;
}

.item {
  flex: 1;
}
```

Ecco come potremmo usare Flexbox con una media query per il design responsive.

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

Ridimensiona lo schermo. Il layout cambierà quando la dimensione dell'esempio sopra supererà la soglia di larghezza di 600px.

### Griglia CSS

Nel layout della griglia CSS, l'unità `fr` consente la distribuzione dello spazio disponibile tra i tracciati della griglia. Il prossimo esempio crea un contenitore di griglia con tre tracciati dimensionati a `1fr`. Questo creerà tre tracciati di colonne, ciascuno prendendo una parte dello spazio disponibile nel contenitore. Hai già esaminato questo approccio (vedi [Griglie flessibili con l'unità fr](/it/docs/Learn_web_development/Core/CSS_layout/Grids#flexible_grids_with_the_fr_unit) per un riepilogo).

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
```

Ecco come potremmo usare il layout della griglia con una media query per il design responsive.

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

## Immagini/media responsivi

Per garantire che i media non siano mai più grandi del loro contenitore responsive, può essere utilizzato il seguente approccio:

```css
img,
picture,
video {
  max-width: 100%;
}
```

Questo ridimensiona i media per garantire che non trabocchino mai dai loro contenitori.

> [!NOTE]
> L'utilizzo di una singola immagine grande e ridimensionarla per adattarsi a dispositivi piccoli spreca larghezza di banda scaricando immagini più grandi del necessario. Inoltre, potrebbe apparire male — un'immagine in formato orizzontale, ad esempio, potrebbe apparire bene su un monitor widescreen, ma potrebbe essere difficile da vedere su un dispositivo mobile, che richiederebbe un'immagine in formato verticale più adatta. Tali problemi possono essere risolti utilizzando l'elemento {{htmlelement("picture")}} e gli attributi {{htmlelement("img")}} `srcset` e `sizes`. Queste sono funzionalità avanzate che vanno oltre lo scopo di questo corso, ma puoi trovare una guida dettagliata su [Immagini responsivi](/it/docs/Web/HTML/Guides/Responsive_images).

Altri consigli utili:

- Assicurati sempre di utilizzare un formato di immagine appropriato per le immagini del tuo sito web (come PNG o JPG) e assicurati di ottimizzare la dimensione del file usando un editor di grafica prima di metterle sul tuo sito web.
- Puoi utilizzare funzionalità CSS come [gradienti](/it/docs/Web/CSS/CSS_images/Using_CSS_gradients) e [ombre](/it/docs/Web/CSS/box-shadow) per implementare effetti visivi senza utilizzare immagini.
- Puoi utilizzare le media query all'interno dell'attributo media sugli elementi {{htmlelement("source")}} annidati all'interno degli elementi {{htmlelement("video")}}/{{htmlelement("audio")}} per servire file video/audio come appropriato per diversi dispositivi (video/audio responsivi).

## Tipografia responsiva

La tipografia responsiva descrive la modifica delle dimensioni dei font all'interno delle media query o l'uso delle unità del viewport per riflettere quantità minori o maggiori di spazio sullo schermo.

### Utilizzare le media query per la tipografia responsiva

In questo esempio, vogliamo impostare il nostro intestazione di livello 1 per essere `4rem`, il che significa che sarà quattro volte la nostra dimensione base del font. È un'intestazione davvero grande! Vogliamo questa intestazione jumbo solo su schermi di grandi dimensioni, quindi creiamo prima un'intestazione più piccola e poi usiamo le media query per sovrascriverla con la dimensione maggiore se sappiamo che l'utente ha una dimensione dello schermo di almeno `1200px`.

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

Abbiamo modificato il nostro esempio di griglia responsive sopra per includere anche il tipo responsivo utilizzando il metodo illustrato. Puoi vedere come l'intestazione cambia dimensioni quando il layout passa alla versione a due colonne.

Su mobile l'intestazione è più piccola, ma su desktop, vediamo la dimensione dell'intestazione più grande:

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

Come mostra questo approccio alla tipografia, non è necessario limitare le media query solo alla modifica del layout della pagina. Possono essere utilizzate per regolare qualsiasi elemento per renderlo più usabile o attraente su dimensioni di schermo alternative.

### Utilizzare le unità del viewport per la tipografia responsiva

Le unità del viewport `vw` possono anche essere utilizzate per abilitare la tipografia responsiva, senza la necessità di impostare breakpoints con le media query. `1vw` è uguale a uno percento della larghezza del viewport, il che significa che se imposti la dimensione del font utilizzando `vw`, sarà sempre correlata alla dimensione del viewport.

```css
h1 {
  font-size: 6vw;
}
```

Il problema nel fare quanto sopra è che l'utente perde la possibilità di zoommare su qualsiasi testo impostato utilizzando l'unità `vw`, poiché quel testo è sempre correlato alla dimensione del viewport. **Pertanto, non dovresti mai impostare il testo usando solo unità viewport**.

Esiste una soluzione, e prevede l'uso di [`calc()`](/it/docs/Web/CSS/calc). Se aggiungi l'unità `vw` a un valore impostato utilizzando una dimensione fissa come `em` o `rem`, il testo sarà ancora zoomabile. Fondamentalmente, l'unità `vw` si aggiunge a quel valore in scala:

```css
h1 {
  font-size: calc(1.5rem + 4vw);
}
```

Questo significa che dobbiamo specificare la dimensione del font per l'intestazione solo una volta, piuttosto che impostarla per i dispositivi mobili e ridefinirla nelle media query. Il font quindi aumenta gradualmente man mano che aumenta la dimensione del viewport.

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

## Il tag meta del viewport

Se guardi il codice sorgente HTML di una pagina responsive, vedrai solitamente il seguente tag {{htmlelement("meta")}} nel `<head>` del documento.

```html
<meta name="viewport" content="width=device-width,initial-scale=1" />
```

Questo tag meta [viewport](/it/docs/Web/HTML/Guides/Viewport_meta_element) dice ai browser mobili che dovrebbero impostare la larghezza del viewport sulla larghezza del dispositivo, e scalare il documento al 100% della sua dimensione prevista, che mostra il documento nella dimensione ottimizzata per i dispositivi mobili che hai previsto.

Perché è necessario? Perché i browser mobili tendono a mentire sulla larghezza del loro viewport.

Questo tag meta esiste perché quando gli smartphone sono arrivati per la prima volta, la maggior parte dei siti non era ottimizzata per il mobile. Il browser mobile avrebbe quindi impostato la larghezza del viewport a 980 pixel, renderizzato la pagina a quella larghezza, e mostrato il risultato come una versione rimpicciolita del layout desktop. Gli utenti potevano zoommare e scorrere il sito per visualizzare le parti di loro interesse, ma appariva male.

Impostando `width=device-width` stai sovrascrivendo il default di un dispositivo mobile, come il default `width=980px` di Apple, con la larghezza effettiva del dispositivo. Senza di esso, il tuo design responsive con breakpoints e media query potrebbe non funzionare come previsto sui browser mobili. Se hai un layout a schermo stretto che si attiva a `480px` di larghezza del viewport o meno, ma il dispositivo dice di essere largo `980px`, quell'utente non vedrà il tuo layout a schermo stretto.

**Pertanto, dovresti _sempre_ includere il tag meta del viewport nell'intestazione dei tuoi documenti.**

## Sommario

Il design responsivo si riferisce a un sito o un'applicazione che risponde all'ambiente in cui viene visualizzata. Comprende una serie di funzionalità e tecniche CSS e HTML ed è ora essenzialmente solo il modo in cui costruiamo i siti web per default. Considera i siti che visiti sul tuo telefono — è probabilmente abbastanza insolito trovare un sito che è la versione desktop ridimensionata, o dove devi scorrere lateralmente per trovare le cose. Questo perché il web è passato a questo approccio di progettare in modo responsivo.

È anche diventato molto più facile ottenere design responsivi grazie ai metodi di layout che hai imparato in queste lezioni. Se sei nuovo nello sviluppo web oggi hai molti più strumenti a tua disposizione rispetto ai primi giorni del design responsivo. Pertanto, vale la pena controllare l'età di qualsiasi materiale che stai utilizzando. Mentre gli articoli storici sono ancora utili, l'uso moderno di CSS e HTML rende molto più facile creare design eleganti e utili, indipendentemente dal dispositivo con cui il tuo visitatore visualizza il sito.

Successivamente, studieremo le media query in maggiore dettaglio e mostreremo come usarle per risolvere alcuni problemi comuni.

## Vedi anche

- Lavorare con dispositivi touchscreen:
  - [Eventi touch](/it/docs/Web/API/Touch_events) forniscono la capacità di interpretare l'attività delle dita (o dello stilo) su touchscreen o trackpad, permettendo un supporto di qualità per interfacce utente complesse basate sul touch.
  - Utilizza le media query [pointer](/it/docs/Web/CSS/@media/pointer) o [any-pointer](/it/docs/Web/CSS/@media/any-pointer) per caricare diversi CSS su dispositivi touch-enabled.
- [Guida di CSS-Tricks alle media queries](https://css-tricks.com/a-complete-guide-to-css-media-queries/)

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Grids", "Learn_web_development/Core/CSS_layout/Media_queries", "Learn_web_development/Core/CSS_layout")}}
