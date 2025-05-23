---
title: Introduzione a CSS
short-title: Introduzione a CSS
slug: Learn_web_development/Core/Styling_basics/Getting_started
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/What_is_CSS", "Learn_web_development/Core/Styling_basics/Styling_a_bio_page", "Learn_web_development/Core/Styling_basics")}}

In questo articolo, prenderemo un documento HTML semplice e applicheremo CSS, imparando alcuni dettagli pratici del linguaggio lungo la strada. Rivedremo anche le caratteristiche della sintassi CSS che non hai ancora esaminato.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software">Software di base installato</a>, conoscenza di base del
        <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files">lavoro con i file</a>, e basi di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">Introduzione a HTML</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Applicare CSS a un documento HTML.</li>
          <li>Esperienza pratica nella scrittura di CSS di base.</li>
          <li>Conoscenza dei tipi di selettori fondamentali e combinatori.</li>
          <li>Il concetto di stato applicato a CSS.</li>
          <li>Familiarità con altre caratteristiche della sintassi CSS come at-rule, funzioni, proprietà shorthand e spazi bianchi.</li>
        <ul>
      </td>
    </tr>
  </tbody>
</table>

## Iniziare con un po' di HTML

Il nostro punto di partenza è un documento HTML. Puoi copiare il codice qui sotto se vuoi lavorare sul tuo computer. Salva il codice sottostante come `index.html` in una cartella sul tuo computer.

```html live-sample___unstyled
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Getting started with CSS</title>
  </head>

  <body>
    <h1>I am a level one heading</h1>

    <p>
      This is a paragraph of text. In the text is a
      <span>span element</span> and also a
      <a href="https://example.com">link</a>.
    </p>

    <p>
      This is the second paragraph. It contains an <em>emphasized</em> element.
    </p>

    <ul>
      <li>Item <span>one</span></li>
      <li>Item two</li>
      <li>Item <em>three</em></li>
    </ul>
  </body>
</html>
```

Questo viene reso così:

{{EmbedLiveSample("unstyled", "", "240px")}}

> [!NOTE]
> Se stai leggendo questo su un dispositivo o un ambiente in cui non puoi facilmente creare file, non preoccuparti — clicca sul pulsante "Play" nel campione live sopra per aprirlo nel MDN Playground. Lì, puoi modificare il codice CSS e HTML come indicato più avanti e vedere i risultati combinati in tempo reale.

## Aggiungere CSS al nostro documento

La primissima cosa che dobbiamo fare è dire al documento HTML che abbiamo delle regole CSS che vogliamo esso utilizzi. Ci sono tre modi diversi per applicare CSS a un documento HTML con cui ti imbatterai comunemente — fogli di stile esterni, fogli di stile interni e stili inline. Osserviamoli ora.

### Fogli di stile esterni

Un foglio di stile esterno contiene CSS in un file separato con un'estensione `.css`. Questo è il metodo più comune e utile per portare il CSS in un documento. È possibile collegare un singolo file CSS a più pagine web, stilizzandole tutte con lo stesso foglio di stile CSS.

Crea un file nella stessa cartella del tuo documento HTML e salvalo come `styles.css`.

Per collegare `styles.css` a `index.html`, aggiungi la seguente linea da qualche parte all'interno del {{htmlelement("head")}} del documento HTML:

```html
<link rel="stylesheet" href="styles.css" />
```

Questo {{htmlelement("link")}} element dice al browser che abbiamo un foglio di stile, usando l'attributo `rel`, e la posizione di quel foglio di stile come valore dell'attributo `href`. Puoi testare che il CSS funzioni aggiungendo una regola a `styles.css`. Usando il tuo editor di codice, aggiungi il seguente al tuo file CSS (o aggiungilo alla sezione "CSS" nel MDN Playground):

```css
h1 {
  color: red;
}
```

Salva i tuoi file HTML e CSS e ricarica la pagina in un browser web. Il titolo di livello uno in cima al documento dovrebbe ora essere rosso. Se ciò accade, congratulazioni — hai applicato correttamente del CSS a un documento HTML. Se non accade, controlla attentamente di aver digitato tutto correttamente.

#### Localizzare i fogli di stile in posti diversi

Nell'esempio sopra, il file CSS è nella stessa cartella del documento HTML, ma potresti posizionarlo altrove e regolare il percorso (nello stesso modo delle [immagini HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_images)). Ecco tre esempi:

```html
<!-- In a subdirectory called styles in the current directory -->
<link rel="stylesheet" href="styles/style.css" />

<!-- In a subdirectory called general, which is in a subdirectory called styles, in the current directory -->
<link rel="stylesheet" href="styles/general/style.css" />

<!-- Go back one directory level, then in a subdirectory called styles -->
<link rel="stylesheet" href="../styles/style.css" />
```

### Fogli di stile interni

I fogli di stile interni sono contenuti all'interno di elementi {{htmlelement("style")}}, che vanno dentro il {{htmlelement("head")}} HTML. Creiamone uno adesso.

Nel tuo documento HTML, aggiungi il seguente frammento da qualche parte tra i tag `<head>` e `</head>`:

```html
<style>
  p {
    color: purple;
  }
</style>
```

Salva e aggiorna, e dovresti vedere tutti i tuoi paragrafi diventare viola.

In alcune circostanze, i fogli di stile interni possono essere utili. Ad esempio, potresti lavorare con un sistema di gestione dei contenuti in cui sei bloccato dalla modifica dei file CSS esterni.

Tuttavia, per i siti con più di una pagina, i fogli di stile interni sono meno efficienti dei fogli di stile esterni. Per applicare CSS uniforme a più pagine utilizzando fogli di stile interni, devi ripetere il foglio di stile interno su ogni pagina web. La penalità di efficienza si trasferisce anche alla manutenzione del sito. Con CSS nei fogli di stile interni, c'è il rischio che anche un semplice cambiamento di stile possa richiedere modifiche a più pagine web.

Prima di andare avanti, rimuovi l'elemento `<style>` e i suoi contenuti dal tuo esempio HTML.

### Stili inline

Gli stili inline sono dichiarazioni CSS che influenzano un singolo elemento HTML, contenuti all'interno di un attributo `style`. Proviamo a implementarli adesso.

Aggiungi un attributo `style` all'elemento {{htmlelement("span")}} nel tuo HTML, in modo che appaia come segue:

```html
<span style="color: purple; font-weight: bold">span element</span>
```

Salva e aggiorna, e dovresti vedere solo il testo all'interno dello `<span>` diventare viola e grassetto. Prova ad aggiungere altre dichiarazioni all'interno del tuo attributo `style` (separate da punti e virgola), o alcuni attributi `style` aggiuntivi ad altri elementi.

Una volta terminato l'esperimento, rimuovi tutti i tuoi attributi `style`.

**Evita di usare CSS in questo modo se possibile.** È una cattiva pratica. In primo luogo, è l'implementazione meno efficiente di CSS per la manutenzione. Un cambiamento di stile potrebbe richiedere più modifiche all'interno di una singola pagina web. In secondo luogo, il CSS inline mescola anche codice (CSS) di presentazione con HTML e contenuto, rendendo tutto più difficile da leggere e comprendere. Separare codice e contenuto rende la manutenzione più semplice per tutti coloro che lavorano sul sito web.

Potresti dover ricorrere a utilizzare stili inline se il tuo ambiente di lavoro è molto restrittivo. Ad esempio, potrebbe darsi che il tuo CMS ti consenta solo di modificare il corpo HTML. Potresti anche vedere molti stili inline nelle email HTML per ottenere compatibilità con il maggior numero possibile di client di posta elettronica. È anche abbastanza comune impostare stili inline quando si applica dinamicamente lo stile usando JavaScript.

## Uso dei selettori comuni

In questa sezione faremo un breve tour attraverso alcuni dei tipi di selettori più comuni che incontrerai.

### Selezione degli elementi HTML

Rendendo il nostro titolo rosso, abbiamo già dimostrato che possiamo mirare e stilizzare un elemento HTML. Lo facciamo mirando a un **element selector** (noto anche come **type selector**) — questo è un selettore che corrisponde direttamente a un nome di elemento HTML. Per mirare a tutti i paragrafi nel documento, useresti il selettore `p`. Per rendere verdi tutti i paragrafi, useresti:

```css
p {
  color: green;
}
```

Puoi mirare a più selettori contemporaneamente separando i selettori con una virgola. Se volessi che tutti i paragrafi e tutti gli elementi di lista siano verdi, la tua regola apparirebbe così:

```css
p,
li {
  color: green;
}
```

Prova questo nell'esempio sotto (clicca su "Play") o nella tua copia locale:

```html hidden live-sample___started-types
<h1>I am a level one heading</h1>

<p>
  This is a paragraph of text. In the text is a <span>span element</span> and
  also a <a href="http://example.com">link</a>.
</p>

<p>This is the second paragraph. It contains an <em>emphasized</em> element.</p>

<ul>
  <li>Item one</li>
  <li>Item two</li>
  <li>Item <em>three</em></li>
</ul>
```

```css live-sample___started-types
h1 {
  color: red;
}

p,
li {
}
```

{{EmbedLiveSample("started-types", "", "240px")}}

### Aggiungere una classe

Fino ad ora, abbiamo stilizzato gli elementi in base ai loro nomi di elementi HTML. Questo funziona fintanto che vuoi che tutti gli elementi di quel tipo nel tuo documento abbiano lo stesso aspetto. Per selezionare un sottoinsieme degli elementi senza modificare gli altri, puoi aggiungere una `class` al tuo elemento HTML e mirare a quella classe nel tuo CSS.

1. Nel tuo documento HTML, aggiungi un [class attribute](/it/docs/Web/HTML/Reference/Global_attributes/class) al secondo elemento della lista. La tua lista apparirà ora così:

   ```html
   <ul>
     <li>Item one</li>
     <li class="special">Item two</li>
     <li>Item <em>three</em></li>
   </ul>
   ```

2. Nel tuo CSS, puoi mirare alla classe `special` creando un selettore che inizia con un punto. Aggiungi il seguente al tuo file CSS:

   ```css
   .special {
     color: orange;
     font-weight: bold;
   }
   ```

3. Salva e aggiorna per vedere quale sia il risultato.

Ora puoi applicare la classe `special` ad altri elementi sulla tua pagina che vuoi abbiano lo stesso aspetto di questo elemento della lista. Aggiungi una classe `special` allo `<span>` all'interno del paragrafo, poi ricarica la tua pagina: Ora dovrebbe essere anch'esso arancione e in grassetto.

### Stilare elementi in base alla loro posizione in un documento

Ci sono momenti in cui vuoi che qualcosa appaia diverso in base a dove si trova nel documento. Ci sono diversi selettori che possono aiutarti qui, ma per ora ne esamineremo solo un paio. Nel nostro documento, ci sono due elementi `<em>` — uno dentro un paragrafo e l'altro dentro un elemento di lista. Per selezionare solo un `<em>` che è nidificato dentro un elemento `<li>`, puoi usare un selettore chiamato **discendant combinator**, che prende la forma di uno spazio tra due altri selettori.

Aggiungi la seguente regola al tuo foglio di stile:

```css
li em {
  color: rebeccapurple;
}
```

Questo selettore selezionerà qualsiasi elemento `<em>` che è un discendente di un `<li>`. Quindi nel tuo documento di esempio, dovresti trovare che l'`<em>` nel terzo elemento di lista è ora viola, ma quello all'interno del paragrafo è invariato.

Qualcos'altro che potresti voler provare è stilare un paragrafo quando viene direttamente dopo un titolo allo stesso livello gerarchico nell'HTML. Per farlo, posiziona un `+` (un **next-sibling combinator**) tra i selettori.

Prova ad aggiungere questa regola anche al tuo foglio di stile:

```css
h1 + p {
  font-size: 200%;
}
```

Il live example here sotto include le due regole di cui sopra. Prova ad aggiungere una regola per rendere uno span rosso se è dentro un paragrafo. Saprai se hai fatto bene perché lo span nel primo paragrafo sarà rosso, ma quello nel primo elemento di lista non cambierà colore.

```html hidden live-sample___started-combinators
<h1>I am a level one heading</h1>

<p>
  This is a paragraph of text. In the text is a <span>span element</span> and
  also a <a href="http://example.com">link</a>.
</p>

<p>This is the second paragraph. It contains an <em>emphasized</em> element.</p>

<ul>
  <li>Item <span>one</span></li>
  <li>Item two</li>
  <li>Item <em>three</em></li>
</ul>
```

```css live-sample___started-combinators
li em {
  color: rebeccapurple;
}

h1 + p {
  font-size: 200%;
}
```

{{EmbedLiveSample("started-combinators", "", "340px")}}

> [!NOTE]
> Come puoi vedere, CSS ci offre diversi modi per mirare agli elementi, e abbiamo solo graffiato la superficie finora! Analizzeremo in modo approfondito tutti questi selettori e molti altri più avanti nel corso.

### Stilare elementi in base allo stato

L'ultimo tipo di stilizzazione che esamineremo in questo tutorial è la capacità di stilare elementi in base al loro stato. Un esempio semplice di questo è quando si stilizzano i collegamenti. Quando stilizziamo un collegamento, dobbiamo mirare all'elemento [`<a>`](/it/docs/Web/HTML/Reference/Elements/a) (anchor). Questo ha stati diversi a seconda che sia non visitato, visitato, messo in evidenza, focalizzato tramite tastiera, o in procinto di essere cliccato (attivato). Puoi usare CSS per mirare a questi stati diversi — il CSS qui sotto stilizza i collegamenti non visitati in rosa e i collegamenti visitati in verde.

```css
a:link {
  color: pink;
}

a:visited {
  color: green;
}
```

Puoi cambiare l'aspetto del collegamento quando l'utente ci passa sopra col mouse, ad esempio rimuovendo la sottolineatura, cosa che si ottiene con la prossima regola:

```css
a:hover {
  text-decoration: none;
}
```

Nell'esempio qui sotto, puoi giocare con valori diversi per i vari stati di un collegamento. Abbiamo aggiunto le regole di cui sopra, e ora ci rendiamo conto che il colore rosa è abbastanza chiaro e difficile da leggere — perché non cambiarlo con un colore migliore? Puoi rendere i collegamenti in grassetto?

```html hidden live-sample___started-states
<h1>I am a level one heading</h1>

<p>
  This is a paragraph of text. In the text is a <span>span element</span> and
  also a <a href="http://example.com">link</a>.
</p>

<p>This is the second paragraph. It contains an <em>emphasized</em> element.</p>

<ul>
  <li>Item one</li>
  <li>Item two</li>
  <li>Item <em>three</em></li>
</ul>
```

```css live-sample___started-states
a:link {
  color: pink;
}

a:visited {
  color: green;
}

a:hover {
  text-decoration: none;
}
```

{{EmbedLiveSample("started-states", "", "240px")}}

Abbiamo rimosso la sottolineatura sul nostro link al passaggio del mouse. Potresti rimuovere la sottolineatura da tutti gli stati di un collegamento. Vale la pena ricordare tuttavia che in un sito reale, vuoi assicurarti che i visitatori sappiano che un collegamento è un collegamento. Lasciare la sottolineatura può essere un indizio importante per capire che del testo all'interno di un paragrafo può essere cliccato — questo è il comportamento a cui sono abituati. Come con tutto in CSS, c'è il potenziale di rendere il documento meno accessibile con le tue modifiche — cercheremo di evidenziare i possibili problemi nei posti appropriati.

> [!NOTE]
> Spesso vedrai menzionata l'[accessibilità](/it/docs/Learn_web_development/Core/Accessibility) in queste lezioni e su tutto MDN. Quando parliamo di accessibilità ci riferiamo alla necessità che le nostre pagine web siano comprensibili e utilizzabili da tutti, che stiano usando un computer con un mouse o un trackpad, un telefono con touchscreen, navigando solo usando la tastiera, o tramite un screen reader, che legge il contenuto del documento.

### Combinare selettori e combinatori

Vale la pena notare che puoi combinare più selettori e combinatori insieme. Per esempio:

```css
/* selects any <span> that is inside a <p>, which is inside an <article>  */
article p span {
}

/* selects any <p> that comes directly after a <ul>, which comes directly after an <h1>  */
h1 + ul + p {
}
```

Puoi combinare più tipi insieme, anche. Prova ad aggiungere il seguente nel tuo codice:

```css
h1 + p .special {
  color: yellow;
  background-color: black;
  padding: 5px;
}
```

Questo stilizzerà qualsiasi elemento con una classe di `special`, che è dentro un `<p>`, che viene subito dopo un `<h1>`. Phew! Questo dovrebbe mirare all'elemento `<span class="special">elemento span</span>` nel tuo codice.

Non preoccuparti se questo sembra complicato al momento — presto inizierai a prenderci la mano man mano che scriverai più CSS.

## Altre caratteristiche della sintassi CSS

Ora che abbiamo giocato con alcune caratteristiche di CSS, ti faremo un tour ad alto livello di alcune delle altre caratteristiche della sintassi CSS che incontrerai durante il corso. Se vuoi cercare ulteriori dettagli su uno di questi, puoi provare a digitare il nome della funzione nel campo di ricerca in cima a questa pagina, o sfogliare il [reference CSS](/it/docs/Web/CSS/Reference) di MDN.

Per sperimentare con i frammenti di codice in ogni caso, potresti aggiungere l'HTML e il CSS forniti all'esempio locale o all'istanza MDN Playground su cui hai lavorato sopra.

### Funzioni

Mentre la maggior parte dei valori sono parole chiave relativamente semplici o valori numerici, ci sono alcuni valori che prendono la forma di una funzione.

#### La funzione calc()

Un esempio sarebbe la funzione `calc()`, che può fare semplici calcoli all'interno di CSS:

```html
<div class="outer"><div class="box">The inner box is 90% - 30px.</div></div>
```

```css
.outer {
  border: 5px solid black;
}

.box {
  padding: 10px;
  width: calc(90% - 30px);
  background-color: rebeccapurple;
  color: white;
}
```

Questo viene reso come:

{{EmbedLiveSample('The_calc_function', '100%', 200)}}

Una funzione consiste nel nome della funzione e parentesi per racchiudere i valori per la funzione. Nel caso dell'esempio `calc()` sopra, i valori definiscono la larghezza di questo box come il 90% della larghezza del blocco contenitore, meno 30 pixel.

#### Funzioni di trasformazione

Un altro esempio sarebbe i vari valori per la proprietà {{cssxref("transform")}}, come `rotate()`.

```html
<div class="box"></div>
```

```css
.box {
  margin: 30px;
  width: 100px;
  height: 100px;
  background-color: rebeccapurple;
  transform: rotate(0.8turn);
}
```

L'output del codice di cui sopra appare così:

{{EmbedLiveSample('Transform_functions', '100%', 200)}}

Cerca i diversi valori delle proprietà elencate di seguito. Prova a scrivere regole CSS che applicano lo stile a diversi elementi HTML usando le seguenti funzioni:

- {{cssxref("transform")}}
- {{cssxref("background-image")}}, in particolare valori gradient
- {{cssxref("color")}}, in particolare valori rgb e hsl

### @rules

Le [@rules](/it/docs/Web/CSS/CSS_syntax/At-rule) CSS (pronunciate "at-rules") forniscono istruzioni su come il CSS dovrebbe comportarsi. Una @rule comune che potresti incontrare è `@media`, che viene utilizzata per creare [media queries](/it/docs/Web/CSS/CSS_media_queries). Le media queries usano logica condizionale per applicare lo stile CSS.

Nell'esempio qui sotto, il foglio di stile definisce un background predefinito rosa per l'elemento `<body>`. Tuttavia, una media query segue che imposta un background blu sull'elemento `<body>` se la viewport del browser è più larga di 30em.

```css
body {
  background-color: pink;
}

@media (min-width: 30em) {
  body {
    background-color: blue;
  }
}
```

Incontrerai altre `@rules` nel corso.

### Proprietà shorthand

Alcune proprietà come {{cssxref("font")}}, {{cssxref("background")}}, {{cssxref("padding")}}, {{cssxref("border")}}, e {{cssxref("margin")}} sono chiamate **proprietà shorthand**. Questo perché le proprietà shorthand impostano più valori in una sola riga.

Per esempio, questa riga di codice:

```css
/* In 4-value shorthands like padding and margin, the values are applied
   in the order top, right, bottom, left (clockwise from the top). There are also other
   shorthand types, for example 2-value shorthands, which set padding/margin
   for top/bottom, then left/right */
padding: 10px 15px 15px 5px;
```

è equivalente a queste quattro righe di codice:

```css
padding-top: 10px;
padding-right: 15px;
padding-bottom: 15px;
padding-left: 5px;
```

Questa riga:

```css
background: red url(bg-graphic.png) 10px 10px repeat-x fixed;
```

è equivalente a queste cinque righe:

```css
background-color: red;
background-image: url(bg-graphic.png);
background-position: 10px 10px;
background-repeat: repeat-x;
background-attachment: fixed;
```

Nel corso del corso, incontrerai molti altri esempi di proprietà shorthand. Per ora, prova a usare le dichiarazioni sopra (o altre che potresti conoscere) nel tuo codice per diventare più familiare con come funzionano.

### Commenti CSS

Come per qualsiasi lavoro di codifica, è buona pratica scrivere commenti nel tuo CSS. Questo aiuta a ricordare come funziona il codice quando torni più tardi a fare correzioni o miglioramenti. Aiuta anche gli altri a capire il codice.

I commenti CSS iniziano con `/*` e finiscono con `*/`. Nell'esempio qui sotto, i commenti segnano l'inizio di sezioni distinte di codice. Questo aiuta a navigare nel codebase man mano che diventa più grande. Con questo tipo di commenti in atto, cercare i commenti nel tuo editor di codice diventa un modo per trovare in modo efficiente una sezione di codice.

```css
/* Handle basic element styling */
/* ---------------------------- */
body {
  font:
    1em/150% Helvetica,
    Arial,
    sans-serif;
  padding: 1em;
  margin: 0 auto;
  max-width: 33em;
}

@media (min-width: 70em) {
  /* Increase the global font size on larger screens or windows
     for better readability */
  body {
    font-size: 130%;
  }
}

h1 {
  font-size: 1.5em;
}

/* Handle specific elements nested in the DOM */
div p,
#id:first-line {
  background-color: red;
  border-radius: 3px;
}

div p {
  margin: 0;
  padding: 1em;
}

div p + p {
  padding-top: 0;
}
```

"Commentare" il codice è anche utile per disabilitare temporaneamente sezioni del codice per il collaudo. Nell'esempio qui sotto, le regole per `.special` sono disabilitate "commentando" il codice.

```css
/*.special {
  color: red;
}*/

p {
  color: blue;
}
```

Prova ad aggiungere commenti nel tuo CSS.

### Spazio bianco in CSS

Lo spazio bianco significa spazi effettivi, tabulazioni e nuove linee. Proprio come i browser ignorano lo spazio bianco extra in HTML, i browser ignorano lo spazio bianco extra all'interno di CSS. Il vantaggio dello spazio bianco è che rende il codice più facile da leggere.

Nell'esempio qui sotto, ogni dichiarazione (e inizio/fine regola) ha la sua riga propria. Questo è senza dubbio un buon modo per scrivere CSS. Rende più facile mantenere e comprendere il CSS.

```css
body {
  font:
    1em/150% Helvetica,
    Arial,
    sans-serif;
  padding: 1em;
  margin: 0 auto;
  max-width: 33em;
}

@media (min-width: 70em) {
  body {
    font-size: 130%;
  }
}

h1 {
  font-size: 1.5em;
}
```

Il prossimo esempio mostra lo stesso CSS in un formato più compresso, con tutto lo spazio bianco extra rimosso. Sebbene i due esempi funzionino allo stesso modo, quello sotto è più difficile da leggere.

```css-nolint
body{font:1em/150% Helvetica,Arial,sans-serif;padding:1em;margin:0 auto;max-width:33em;}
@media(min-width:70em){body{font-size:130%;}}
h1{font-size:1.5em;}
```

Tieni presente che rimuovere alcuni spazi bianchi può causare errori. I nomi delle proprietà non contengono mai spazi, mentre i valori delle proprietà che si aspettano spazi tra valori multipli saranno invalidati se quello spazio viene rimosso. Per esempio, queste dichiarazioni sono validi CSS:

```css
margin: 0 auto;
padding-left: 10px;
```

Ma queste dichiarazioni sono invalide:

```css example-bad
margin: 0auto;
padding- left: 10px;
```

Vedi gli errori di spazio? Primo, `0auto` non è riconosciuto come un valore valido per la proprietà `margin`. L'entrata `0auto` è intesa per essere due valori separati: `0` e `auto`. Secondo, il browser non riconosce `padding-` come un nome di proprietà valido. Il nome corretto della proprietà (`padding-left`) ha uno spazio inserito in esso.

Dovresti sempre assicurarti di separare i valori distinti l'uno dall'altro con almeno uno spazio. Mantieni i nomi delle proprietà e i valori delle proprietà insieme come singole stringhe ininterrotte.

Per scoprire come lo spazio può rompere il CSS, prova a giocare con lo spazio all'interno del tuo test CSS.

## Riassunto

In questo articolo, abbiamo esaminato diversi modi in cui puoi stilizzare un documento utilizzando CSS. Svilupparemo questa conoscenza man mano che proseguiamo il resto delle lezioni. Tuttavia, ora già sai abbastanza per stilizzare il testo e applicare CSS basato su diversi modi di mirare agli elementi nel documento.

Successivamente, ti daremo una sfida per testare la tua nuova conoscenza.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/What_is_CSS", "Learn_web_development/Core/Styling_basics/Styling_a_bio_page", "Learn_web_development/Core/Styling_basics")}}
