---
title: Web design responsive
slug: Learn_web_development/Core/CSS_layout/Responsive_Design
l10n:
  sourceCommit: 4c58f4735f986a91bee1b77e336143630df727a2
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Fundamental_Layout_Comprehension", "Learn_web_development/Core/CSS_layout/Media_queries", "Learn_web_development/Core/CSS_layout")}}

Il _web design responsive_ (RWD) è un approccio al web design che consente alle pagine web di essere visualizzate correttamente su tutte le dimensioni e risoluzioni dello schermo, garantendo al contempo una buona usabilità. È il modo di progettare per un web multi-dispositivo. In questo articolo verranno illustrate alcune tecniche utili per padroneggiarlo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Fondamenti dello stile CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Fondamenti dello stile di testo e font</a>,
        familiarità con i <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">concetti fondamentali del layout CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Che cos'è il design responsive: progettare layout web flessibili che funzionino correttamente su schermi di dispositivi con dimensioni, risoluzioni e così via differenti.</li>
          <li>La relazione tra strumenti di layout moderni come grid e flexbox e il design responsive.</li>
          <li>I concetti alla base dell'uso delle media query per il design responsive, inclusi mobile-first e breakpoint.</li>
          <li>Perché <code>&lt;meta viewport=""&gt;</code> è necessario affinché i documenti web vengano visualizzati correttamente sui dispositivi mobili.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Precursore del design responsive: web design per dispositivi mobili

Prima che il web design responsive diventasse l'approccio standard per far funzionare i siti web su diversi tipi di dispositivi, gli sviluppatori web parlavano di web design per dispositivi mobili, sviluppo web mobile o, talvolta, design mobile-friendly. Si tratta sostanzialmente della stessa cosa del web design responsive: l'obiettivo è garantire che i siti web funzionino bene su dispositivi con caratteristiche fisiche differenti, come dimensioni e risoluzione dello schermo, in termini di layout, contenuti (testo e media) e prestazioni.

La differenza riguarda principalmente i dispositivi coinvolti e le tecnologie disponibili per creare soluzioni:

- In passato si parlava di desktop o mobile, ma ora sono disponibili molti tipi diversi di dispositivi, come desktop, laptop, telefoni mobili, tablet, orologi e così via. Invece di adattarsi a poche dimensioni dello schermo, ora è necessario progettare siti in modo difensivo, per adattarsi alle dimensioni e risoluzioni comuni dello schermo, oltre a quelle sconosciute.
- I dispositivi mobili avevano prestazioni limitate in termini di CPU/GPU e larghezza di banda disponibile. Alcuni non supportavano CSS né HTML e, di conseguenza, era comune effettuare il browser sniffing lato server per determinare il tipo di dispositivo/browser prima di servire un sito che il dispositivo fosse in grado di gestire. Ai dispositivi mobili venivano spesso fornite esperienze molto semplici e basilari perché erano le uniche che potevano gestire. Oggi i dispositivi mobili sono in grado di gestire le stesse tecnologie dei computer desktop, quindi queste tecniche sono meno comuni.
  - È comunque opportuno usare le tecniche descritte in questo articolo per offrire agli utenti mobili un'esperienza adeguata, poiché esistono ancora vincoli quali la durata della batteria e la larghezza di banda.
  - L'esperienza utente è ancora importante. Per esempio, un utente mobile di un sito di viaggi potrebbe voler controllare soltanto gli orari dei voli e le informazioni sui ritardi, senza vedersi presentare un globo animato 3D che mostra le rotte aeree e la storia dell'azienda.
- Le tecnologie moderne sono molto migliori per creare esperienze responsive. Per esempio, le [tecnologie per immagini/media responsive](#responsive_imagesmedia) consentono ora di servire media appropriati a dispositivi diversi senza dover ricorrere a tecniche come lo sniffing lato server.

## Introduzione al web design responsive

HTML è intrinsecamente responsive, o _fluido_. Se si crea una pagina web contenente soltanto HTML, senza CSS, e si ridimensiona la finestra, il browser ridisporrà automaticamente il testo affinché si adatti al viewport.

Sebbene il comportamento responsive predefinito possa far pensare che non sia necessaria alcuna soluzione, lunghe righe di testo visualizzate a schermo intero su un monitor ampio possono essere difficili da leggere. Questo problema può essere risolto con CSS, per esempio creando colonne strette per limitare la lunghezza delle righe. Tuttavia, ciò può creare nuovi problemi per gli utenti che restringono la finestra del browser o visualizzano il sito su un dispositivo mobile: le colonne appariranno compresse e diventeranno più difficili da leggere.

![Un layout con due colonne compresse in un viewport di dimensioni mobili.](mdn-rwd-liquid.png)

Neanche creare una pagina web non ridimensionabile impostando una larghezza fissa funziona: ciò porta a barre di scorrimento sui dispositivi stretti e a troppo spazio vuoto sugli schermi ampi.

Il web design responsive, o RWD, è un approccio progettuale che affronta l'intera gamma di dispositivi e dimensioni di dispositivi disponibili, consentendo l'adattamento automatico allo schermo, indipendentemente dal fatto che il contenuto venga visualizzato su un tablet, telefono, televisore o orologio.

Il web design responsive non è una tecnologia separata: è un approccio. È un termine usato per descrivere un insieme di buone pratiche impiegate per creare un layout in grado di _rispondere_ a qualsiasi dispositivo utilizzato per visualizzare il contenuto.

Il termine _responsive design_, [coniato da Ethan Marcotte nel 2010](https://alistapart.com/article/responsive-web-design/), descriveva l'uso di griglie fluide, immagini fluide e media query per creare contenuti responsive.

All'epoca, la raccomandazione era di usare CSS `float` per il layout e le media query per interrogare la larghezza del browser, creando layout per diversi breakpoint. Le immagini fluide vengono impostate per non superare la larghezza del proprio contenitore; la loro proprietà `max-width` è impostata su `100%`. Le immagini fluide si ridimensionano verso il basso quando la colonna che le contiene si restringe, ma non diventano più grandi della loro dimensione intrinseca quando la colonna si allarga. Ciò consente a un'immagine di ridursi per adattarsi al contenuto invece di fuoriuscire da esso, senza però ingrandirsi e diventare pixelata se il contenitore diventa più ampio dell'immagine.

I moderni metodi di layout CSS sono intrinsecamente responsive e, dalla pubblicazione dell'articolo di Marcotte, disponiamo di numerose funzionalità integrate nella piattaforma web che semplificano la progettazione di siti responsive.

Il resto dell'articolo spiegherà le varie funzionalità della piattaforma web che possono risultare utili durante la creazione di un sito responsive.

## Media query

Le [media query](/it/docs/Web/CSS/Guides/Media_queries/Using) consentono di eseguire una serie di test, per esempio verificare se lo schermo dell'utente supera una certa larghezza o risoluzione, e applicare CSS in modo selettivo per stilizzare la pagina in modo appropriato alle esigenze dell'utente.

Per esempio, la seguente media query verifica se la pagina web corrente viene visualizzata come media screen, quindi non come documento stampato, e se il viewport è largo almeno `80rem`. La regola `.container` verrà applicata soltanto se entrambe le condizioni sono vere.

```css
@media screen and (width >= 80rem) {
  .container {
    margin: 1em 2em;
  }
}
```

È possibile aggiungere più media query all'interno di un foglio di stile, modificando l'intero layout o alcune sue parti per adattarli meglio alle varie dimensioni dello schermo. I punti in cui viene introdotta una media query e il layout cambia sono noti come _breakpoint_.

Un approccio comune nell'uso delle media query consiste nel creare un semplice layout a colonna singola per dispositivi con schermi stretti, come i telefoni mobili, quindi verificare la presenza di schermi più ampi e implementare un layout a più colonne quando si sa di disporre di larghezza sufficiente. Progettare prima per dispositivi mobili è noto come design **mobile first**.

Quando si usano breakpoint, le buone pratiche incoraggiano a definire i breakpoint delle media query con [unità relative](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#relative_length_units), anziché con dimensioni assolute di un singolo dispositivo.

Esistono diversi approcci agli stili definiti all'interno di un blocco media query: si va dall'uso delle media query per {{htmlelement("link")}} fogli di stile basati su intervalli di dimensioni del browser, all'inclusione esclusiva di variabili di proprietà personalizzate per memorizzare i valori associati a ciascun breakpoint.

Le media query possono aiutare con RWD, ma non sono un requisito. Griglie flessibili, unità relative e valori unitari minimi e massimi possono essere utilizzati senza media query.

> [!NOTE]
> Scrimba offre un tutorial intitolato [A parte: Media query](https://scrimba.com/frontend-path-c0j/~0j3?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>, che fornisce un'introduzione interattiva alle media query e una sfida per verificare la comprensione dei concetti di base.

## Tecnologie di layout responsive

I siti responsive sono basati su griglie flessibili, il che significa che non è necessario indirizzare ogni possibile dimensione di dispositivo con layout pixel-perfect.

Usando una griglia flessibile, è possibile modificare una caratteristica o aggiungere un breakpoint e cambiare il design nel punto in cui il contenuto inizia ad apparire male. Per esempio, per garantire che la lunghezza delle righe non diventi illeggibile man mano che aumenta la dimensione dello schermo è possibile usare {{cssxref('columns')}}; se una casella diventa compressa con due parole per riga man mano che si restringe, è possibile impostare un breakpoint.

Diversi metodi di layout, inclusi [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox) e [CSS Grid](/it/docs/Learn_web_development/Core/CSS_layout/Grids), sono responsive per impostazione predefinita. Tutti presuppongono che si stia cercando di creare una griglia flessibile e forniscono modi più semplici per farlo.

### Flexbox

In flexbox, gli elementi flex si restringono o si espandono, distribuendo lo spazio tra gli elementi in base allo spazio presente nel loro contenitore. Modificando i valori di `flex-grow` e `flex-shrink` è possibile indicare come gli elementi devono comportarsi quando incontrano più o meno spazio attorno a essi.

Nell'esempio seguente, gli elementi flex occuperanno ciascuno una quantità uguale di spazio nel contenitore flex, usando la forma abbreviata `flex: 1` discussa in precedenza; vedere [Flexbox: Dimensionamento flessibile degli elementi flex](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox#flexible_sizing_of_flex_items).

```css
.container {
  display: flex;
}

.item {
  flex: 1;
}
```

Ecco come usare flexbox con una media query per il design responsive.

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
  background-color: #eeeeee;
}
.wrapper {
  max-width: 960px;
  margin: 2em auto;
}

.col1,
.col2 {
  background-color: white;
}
```

```css live-sample___flex-based-rwd
@media screen and (width >= 600px) {
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

Ridimensionare la finestra del browser. Il layout passerà da una colonna singola a due colonne quando la dimensione dell'esempio precedente supera la soglia di larghezza di `600px`.

### CSS grid

Nel layout CSS grid, l'unità `fr` consente la distribuzione dello spazio disponibile tra le tracce della griglia. L'esempio successivo crea un contenitore grid con tre tracce di dimensione `1fr`. Questo creerà tre tracce di colonna, ciascuna delle quali occupa una parte dello spazio disponibile nel contenitore. Questo approccio è già stato esaminato; per un ripasso, vedere [Griglie flessibili con l'unità fr](/it/docs/Learn_web_development/Core/CSS_layout/Grids#flexible_grids_with_the_fr_unit).

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
```

Ecco come usare il layout grid con una media query per il design responsive.

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
  background-color: #eeeeee;
}
.wrapper {
  max-width: 960px;
  margin: 2em auto;
}

.col1,
.col2 {
  background-color: white;
}
```

```css live-sample___grid-based-rwd
@media screen and (width >= 600px) {
  .wrapper {
    display: grid;
    grid-template-columns: 1fr 2fr;
    column-gap: 5%;
  }
}
```

{{EmbedLiveSample("grid-based-rwd", "", "550px")}}

Provare nuovamente a ridimensionare la finestra del browser: il layout dell'esempio dovrebbe cambiare alla soglia di larghezza di `600px`, nello stesso modo dell'esempio precedente.

## Immagini/media responsive

Per garantire che i media non siano mai più grandi del loro contenitore responsive, è possibile usare il seguente approccio:

```css
img,
picture,
video {
  max-width: 100%;
}
```

Questo ridimensiona gli elementi media per garantire che non fuoriescano mai dai loro contenitori.

> [!NOTE]
> Usare una singola immagine grande e ridimensionarla per adattarla a dispositivi piccoli spreca larghezza di banda scaricando immagini più grandi del necessario. Può anche avere un brutto aspetto: un'immagine orizzontale, per esempio, può apparire bene su un monitor widescreen, ma potrebbe essere difficile da vedere su un dispositivo mobile, per il quale sarebbe più adatta un'immagine verticale. Questi problemi possono essere risolti usando l'elemento {{htmlelement("picture")}} e gli attributi `srcset` e `sizes` di {{htmlelement("img")}}. Si tratta di funzionalità avanzate che vanno oltre lo scopo di questo corso, ma è disponibile una guida dettagliata in [Immagini responsive](/it/docs/Web/HTML/Guides/Responsive_images).

Altri suggerimenti utili:

- Assicurarsi sempre di usare un formato immagine appropriato per le immagini del sito web, come PNG o JPG, e di ottimizzare la dimensione del file con un editor grafico prima di inserirle nel sito.
- È possibile sfruttare funzionalità CSS come [gradienti](/it/docs/Web/CSS/Guides/Images/Using_gradients) e [ombre](/it/docs/Web/CSS/Reference/Properties/box-shadow) per implementare effetti visivi senza usare immagini.
- È possibile usare media query all'interno dell'attributo media degli elementi {{htmlelement("source")}} annidati negli elementi {{htmlelement("video")}}/{{htmlelement("audio")}} per servire file video/audio appropriati a dispositivi diversi, ovvero video/audio responsive.

## Tipografia responsive

La tipografia responsive descrive la modifica delle dimensioni dei font all'interno delle media query oppure l'uso delle unità viewport per riflettere quantità minori o maggiori di spazio disponibile sullo schermo.

### Uso delle media query per la tipografia responsive

In questo esempio, si vuole impostare il titolo di livello 1 a `4rem`, il che significa che sarà quattro volte la dimensione del font di base. È un titolo davvero grande. Questo titolo gigante è desiderato soltanto su schermi più grandi; pertanto, al titolo viene dapprima assegnata una dimensione più piccola di `2rem`, quindi vengono usate le media query per sovrascriverla con la dimensione maggiore se lo schermo dell'utente ha una larghezza di almeno `1200px`.

```css
html {
  font-size: 1em;
}

h1 {
  font-size: 2rem;
}

@media (width >= 1200px) {
  h1 {
    font-size: 4rem;
  }
}
```

L'esempio successivo è una versione modificata del precedente esempio di griglia responsive, che include un titolo responsive usando il metodo illustrato. Su dispositivi mobili il titolo è più piccolo, mentre su desktop viene visualizzato con una dimensione maggiore:

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
    1.2em "Helvetica",
    "Arial",
    sans-serif;
  margin: 20px;
  padding: 0;
  background-color: #eeeeee;
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
  background-color: white;
}

@media screen and (width >= 600px) {
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

Come negli esempi precedenti, provare a modificare la larghezza della finestra del browser e notare come non solo il layout cambia alla soglia di larghezza di `600px`, ma anche la dimensione del titolo.

Come mostra questo approccio alla tipografia, non è necessario limitare le media query alla sola modifica del layout della pagina. Possono essere utilizzate per adattare qualsiasi elemento, rendendolo più utilizzabile o attraente a dimensioni dello schermo alternative.

### Uso delle unità viewport per la tipografia responsive

Le unità viewport `vw` possono essere usate anche per abilitare la tipografia responsive, senza dover impostare breakpoint con le media query. `1vw` equivale all'uno percento della larghezza del viewport; quindi, impostando la dimensione del font con `vw`, essa sarà sempre correlata alla dimensione del viewport.

```css
h1 {
  font-size: 6vw;
}
```

Il problema di questa soluzione è che l'utente perde la possibilità di ingrandire qualsiasi testo impostato con l'unità `vw`, poiché quel testo è sempre correlato alla dimensione del viewport. **Pertanto, non impostare mai il testo usando soltanto unità viewport**.

Esiste una soluzione che prevede l'uso di {{cssxref("calc()")}}. Se si aggiunge l'unità `vw` a un valore impostato con una dimensione fissa come `em` o `rem`, il testo resterà ingrandibile. In sostanza, l'unità `vw` si aggiunge a quel valore ingrandito:

```css
h1 {
  font-size: calc(1.5rem + 4vw);
}
```

Ciò significa che è necessario specificare la dimensione del font per il titolo soltanto una volta, invece di impostarla per dispositivi mobili e ridefinirla nelle media query. Il font aumenta quindi gradualmente quando aumenta la dimensione del viewport.

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
  background-color: #eeeeee;
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
  background-color: white;
}

@media screen and (width >= 600px) {
  .wrapper {
    display: grid;
    grid-template-columns: 1fr 2fr;
    column-gap: 5%;
  }
}
```

{{EmbedLiveSample("type-vw", "", "550px")}}

Provare a ridimensionare la finestra del browser come prima e notare come, questa volta, la dimensione del titolo aumenti _gradualmente_ al cambiare della larghezza.

## Il meta tag viewport

Osservando il sorgente HTML di una pagina responsive, solitamente viene visualizzato il seguente tag {{htmlelement("meta")}} nel `<head>` del documento.

```html
<meta name="viewport" content="width=device-width" />
```

Questo meta tag [`viewport`](/it/docs/Web/HTML/Reference/Elements/meta/name/viewport) indica ai browser mobili che devono impostare la larghezza del viewport sulla larghezza del dispositivo, mostrando il documento nella dimensione ottimizzata per dispositivi mobili prevista.

Perché è necessario? Perché i browser mobili tendono a dichiarare una larghezza del viewport non corrispondente a quella effettiva.

Questo meta tag esiste perché, quando arrivarono i primi smartphone, la maggior parte dei siti non era ottimizzata per dispositivi mobili. Il browser mobile impostava quindi la larghezza del viewport a 980 pixel, eseguiva il rendering della pagina a tale larghezza e mostrava il risultato come una versione rimpicciolita del layout desktop. Gli utenti potevano ingrandire e spostarsi nel sito web per visualizzare le parti di loro interesse, ma l'aspetto era pessimo.

Impostando `width=device-width` si sovrascrive il valore predefinito di un dispositivo mobile, come il valore predefinito `width=980px` di iPhone, con la larghezza effettiva del dispositivo. Senza di esso, il design responsive con breakpoint e media query potrebbe non funzionare come previsto nei browser mobili. Se è presente un layout per schermi stretti che si attiva a una larghezza del viewport di `480px` o inferiore, ma il dispositivo dichiara una larghezza di `980px`, l'utente non vedrà il layout per schermi stretti.

**È quindi necessario includere _sempre_ il meta tag viewport nell'head dei documenti.**

Esistono diverse altre opzioni che è possibile inserire nell'attributo `content` del meta tag viewport; per maggiori dettagli, vedere il riferimento [`<meta name="viewport">`](/it/docs/Web/HTML/Reference/Elements/meta/name/viewport).

## Riepilogo

Il design responsive si riferisce a un design di sito o applicazione che risponde all'ambiente in cui viene visualizzato. Comprende una serie di funzionalità e tecniche CSS e HTML ed è essenzialmente il modo in cui vengono creati i siti web per impostazione predefinita. Considerare i siti visitati sul telefono: probabilmente è piuttosto insolito incontrare un sito che sia la versione desktop ridotta in scala o nel quale sia necessario scorrere orizzontalmente per trovare gli elementi. Questo perché il web ha adottato questo approccio alla progettazione responsive.

È inoltre diventato molto più semplice realizzare design responsive grazie ai metodi di layout trattati in questo articolo. Chi oggi si avvicina allo sviluppo web ha a disposizione molti più strumenti rispetto ai primi tempi del design responsive. Vale quindi la pena controllare l'età dei materiali utilizzati. Sebbene gli articoli storici siano ancora utili, l'uso moderno di CSS e HTML semplifica notevolmente la creazione di design eleganti e utili, indipendentemente dal dispositivo con cui il visitatore visualizza il sito.

Successivamente verranno studiate più in dettaglio le media query e verrà mostrato come usarle per risolvere alcuni problemi comuni.

## Vedi anche

- Lavorare con dispositivi touchscreen:
  - Gli [eventi touch](/it/docs/Web/API/Touch_events) consentono di interpretare l'attività delle dita, o dello stilo, su schermi touch o trackpad, fornendo un supporto di qualità per interfacce utente complesse basate sul tocco.
  - Usare le media query [pointer](/it/docs/Web/CSS/Reference/At-rules/@media/pointer) o [any-pointer](/it/docs/Web/CSS/Reference/At-rules/@media/any-pointer) per caricare CSS differenti sui dispositivi abilitati al tocco.
- [Guida di CSS-Tricks alle media query](https://css-tricks.com/a-complete-guide-to-css-media-queries/)
- [Il percorso professionale per sviluppatori frontend](https://scrimba.com/the-frontend-developer-career-path-c0j?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> di Scrimba insegna tutto ciò che serve per diventare uno sviluppatore web front-end competente, con lezioni e sfide interattive divertenti, insegnanti esperti e una comunità di supporto. Si parte da zero fino a ottenere il primo lavoro nel front-end. Molti componenti del corso sono disponibili anche come versioni gratuite autonome. Questo include un modulo sul design responsive.

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Fundamental_Layout_Comprehension", "Learn_web_development/Core/CSS_layout/Media_queries", "Learn_web_development/Core/CSS_layout")}}
