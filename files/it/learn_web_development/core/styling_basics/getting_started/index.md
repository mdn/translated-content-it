---
title: Introduzione ai CSS
short-title: Introduzione ai CSS
slug: Learn_web_development/Core/Styling_basics/Getting_started
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/What_is_CSS", "Learn_web_development/Core/Styling_basics/Styling_a_bio_page", "Learn_web_development/Core/Styling_basics")}}

In questo articolo, prenderemo un semplice documento HTML e applicheremo CSS, imparando alcuni dettagli pratici della lingua lungo il percorso. Esamineremo anche le caratteristiche sintattiche del CSS che non avete ancora visto.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software">Software di base installato</a>, conoscenza di base di
        <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files">lavorare con i file</a>, e nozioni di base di HTML (studio
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">Introduzione a HTML</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Applicare CSS a un documento HTML.</li>
          <li>Esperienza pratica nella scrittura di CSS di base.</li>
          <li>Conoscenza pratica dei tipi di selettori fondamentali e combinatori.</li>
          <li>Il concetto di stato così come si applica al CSS.</li>
          <li>Familiarità con altre caratteristiche sintattiche di CSS come at-rules, funzioni, proprietà shorthand e spazi bianchi.</li>
        <ul>
      </td>
    </tr>
  </tbody>
</table>

## Iniziare con un po' di HTML

Il nostro punto di partenza è un documento HTML. Può copiare il codice qui sotto se vuole lavorare sul proprio computer. Salvi il codice qui sotto come `index.html` in una cartella sul suo computer.

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

Questo rende in questo modo:

{{EmbedLiveSample("unstyled", "", "240px")}}

> [!NOTE]
> Se sta leggendo questo su un dispositivo o un ambiente in cui non può facilmente creare file, non si preoccupi — clicchi sul pulsante "Play" nel campione dal vivo sopra per aprirlo nel MDN Playground. Lì, può modificare il codice CSS e HTML come istruito più avanti e vedere i risultati combinati in tempo reale.

## Aggiungere CSS al nostro documento

La primissima cosa che dobbiamo fare è comunicare al documento HTML che abbiamo alcune regole CSS che vogliamo usare. Esistono tre modi diversi per applicare CSS a un documento HTML che incontrerà comunemente — fogli di stile esterni, fogli di stile interni e stili inline. Esaminiamoli ora.

### Fogli di stile esterni

Un foglio di stile esterno contiene CSS in un file separato con estensione `.css`. Questo è il metodo più comune e utile per portare CSS a un documento. Può collegare un singolo file CSS a più pagine web, stilizzandole tutte con lo stesso foglio di stile CSS.

Crei un file nella stessa cartella del suo documento HTML e lo salvi come `styles.css`.

Per collegare `styles.css` a `index.html`, aggiunga la seguente riga da qualche parte all'interno dell'{{htmlelement("head")}} del documento HTML:

```html
<link rel="stylesheet" href="styles.css" />
```

Questo elemento {{htmlelement("link")}} comunica al browser che abbiamo un foglio di stile, usando l'attributo `rel`, e la posizione di quel foglio di stile come valore dell'attributo `href`. Può testare che il CSS funzioni aggiungendo una regola a `styles.css`. Usando il suo editor di codice, aggiunga il seguente al suo file CSS (o lo aggiunga alla casella "CSS" nel MDN Playground):

```css
h1 {
  color: red;
}
```

Salvi i suoi file HTML e CSS e ricarichi la pagina in un browser web. L'intestazione di livello uno nella parte superiore del documento dovrebbe ora essere rossa. Se ciò accade, congratulazioni — ha applicato con successo un po' di CSS a un documento HTML. Se ciò non accade, controlli attentamente che abbia digitato tutto correttamente.

#### Localizzare i fogli di stile in posizioni diverse

Nell'esempio sopra, il file CSS si trova nella stessa cartella del documento HTML, ma potrebbe collocarlo altrove e regolare il percorso (allo stesso modo delle [immagini HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_images)). Ecco tre esempi:

```html
<!-- In a subdirectory called styles in the current directory -->
<link rel="stylesheet" href="styles/style.css" />

<!-- In a subdirectory called general, which is in a subdirectory called styles, in the current directory -->
<link rel="stylesheet" href="styles/general/style.css" />

<!-- Go back one directory level, then in a subdirectory called styles -->
<link rel="stylesheet" href="../styles/style.css" />
```

### Fogli di stile interni

I fogli di stile interni sono contenuti all'interno di elementi {{htmlelement("style")}}, che vanno all'interno del {{htmlelement("head")}} HTML. Creiamone uno ora.

Nel suo documento HTML, aggiunga il seguente frammento da qualche parte tra i tag `<head>` e `</head>`:

```html
<style>
  p {
    color: purple;
  }
</style>
```

Salvi e aggiorni, e dovrebbe vedere tutti i suoi paragrafi diventare viola.

In alcune circostanze, i fogli di stile interni possono essere utili. Ad esempio, forse sta lavorando con un sistema di gestione dei contenuti in cui è bloccato dalla modifica dei file CSS esterni.

Tuttavia, per i siti con più di una pagina, i fogli di stile interni sono meno efficienti dei fogli di stile esterni. Per applicare uno stile CSS uniforme a più pagine usando i fogli di stile interni, deve ripetere il foglio di stile interno su ogni pagina web. La penalità di efficienza si trasferisce anche alla manutenzione del sito. Con CSS nei fogli di stile interni, c'è il rischio che anche un solo semplice cambiamento stilistico possa richiedere modifiche a più pagine web.

Prima di andare avanti, rimuova l'elemento `<style>` e i suoi contenuti dal suo esempio HTML.

### Stili inline

Gli stili inline sono dichiarazioni CSS che riguardano un singolo elemento HTML, contenute all'interno di un attributo `style`. Proviamo a implementarne uno ora.

Aggiunga un attributo `style` all'elemento {{htmlelement("span")}} nel suo HTML, in modo che appaia come segue:

```html
<span style="color: purple; font-weight: bold">span element</span>
```

Salvi e aggiorni, e dovrebbe vedere solo il testo all'interno del `<span>` diventare viola e grassetto. Provi ad aggiungere altre dichiarazioni all'interno del suo attributo `style` (separate da punti e virgola), o alcuni attributi `style` aggiuntivi ad altri elementi.

Una volta che ha finito di sperimentare, rimuova tutti i suoi attributi `style`.

**Eviti di usare CSS in questo modo se possibile.** È una cattiva pratica. Innanzitutto, è l'implementazione meno efficiente di CSS per la manutenzione. Un cambiamento stilistico potrebbe richiedere più modifiche all'interno di una singola pagina web. In secondo luogo, il CSS inline mescola anche il codice presentazionale (CSS) con HTML e contenuti, rendendo tutto più difficile da leggere e capire. Separare il codice e il contenuto rende la manutenzione più facile per chiunque lavori sul sito web.

Potrebbe dover ricorrere all'uso di stili inline se il suo ambiente di lavoro è molto restrittivo. Ad esempio, forse il suo CMS le permette di modificare solo il corpo HTML. Potrebbe anche vedere molti stili inline nelle email HTML per ottenere compatibilità con il maggior numero possibile di client email. È anche abbastanza comune impostare stili inline quando si applica dinamicamente lo stile con JavaScript.

## Utilizzo dei selettori comuni

In questa sezione faremo un breve tour attraverso alcuni dei tipi di selettori più comuni che incontrerà.

### Selezionare gli elementi HTML

Facendo diventare rossa la nostra intestazione, abbiamo già dimostrato di poter mirare e stilizzare un elemento HTML. Facciamo questo mirando a un **selettore di elementi** (noto anche come **selettore di tipo**) — questo è un selettore che corrisponde direttamente al nome di un elemento HTML. Per mirare a tutti i paragrafi nel documento, userebbe il selettore `p`. Per fare in modo che tutti i paragrafi diventino verdi, userebbe:

```css
p {
  color: green;
}
```

Può mirare a più selettori contemporaneamente separando i selettori con una virgola. Se volesse che tutti i paragrafi e tutti gli elementi di lista fossero verdi, la sua regola sarebbe simile a questa:

```css
p,
li {
  color: green;
}
```

Provate questo nell'esempio qui sotto (clicchi su "Play") o nella sua copia locale:

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

Finora abbiamo stilizzato gli elementi in base ai loro nomi di elementi HTML. Questo funziona finché vuole che tutti gli elementi di quel tipo nel suo documento abbiano lo stesso aspetto. Per selezionare un sottoinsieme degli elementi senza cambiare gli altri, può aggiungere una `class` al suo elemento HTML e mirare a quella classe nel suo CSS.

1. Nel suo documento HTML, aggiunga un [attributo class](/it/docs/Web/HTML/Reference/Global_attributes/class) al secondo elemento della lista. La sua lista ora apparirà così:

   ```html
   <ul>
     <li>Item one</li>
     <li class="special">Item two</li>
     <li>Item <em>three</em></li>
   </ul>
   ```

2. Nel suo CSS, può mirare alla classe `special` creando un selettore che inizia con un punto. Aggiunga quanto segue al suo file CSS:

   ```css
   .special {
     color: orange;
     font-weight: bold;
   }
   ```

3. Salvi e aggiorni per vedere qual è il risultato.

Ora può applicare la classe `special` ad altri elementi sulla sua pagina che desidera abbiano lo stesso aspetto di questo elemento della lista. Aggiunga una classe `special` all'`<span>` all'interno del paragrafo, quindi ricarichi la sua pagina: ora dovrebbe essere arancione e in grassetto.

### Stilizzare le cose in base alla loro posizione in un documento

Ci sono momenti in cui desidererà che qualcosa appaia differentemente in base a dove si trova nel documento. Esistono numerosi selettori che possono aiutarla qui, ma per ora ne esamineremo solo un paio. Nel nostro documento, ci sono due elementi `<em>` — uno all'interno di un paragrafo e l'altro all'interno di un elemento della lista. Per selezionare solo un `<em>` che è annidato all'interno di un `<li>`, può usare un selettore chiamato **discendente combinator**, che assume la forma di uno spazio tra due altri selettori.

Aggiunga la seguente regola al suo foglio di stile:

```css
li em {
  color: rebeccapurple;
}
```

Questo selettore selezionerà qualsiasi elemento `<em>` che è un discendente di un `<li>`. Quindi nel suo documento di esempio, dovrebbe trovare che l'`<em>` nel terzo elemento della lista è ora viola, ma quello all'interno del paragrafo è invariato.

Qualcos'altro che potrebbe voler provare è stilizzare un paragrafo quando viene direttamente dopo un'intestazione allo stesso livello gerarchico nell'HTML. Per fare ciò, collochi un `+` (un **combinatore del prossimo fratello**) tra i selettori.

Provi ad aggiungere anche questa regola al suo foglio di stile:

```css
h1 + p {
  font-size: 200%;
}
```

L'esempio dal vivo qui sotto include le due regole sopra. Provi ad aggiungere una regola per fare in modo che uno span diventi rosso se si trova all'interno di un paragrafo. Saprà se ha fatto bene perché lo span nel primo paragrafo sarà rosso, ma quello nel primo elemento della lista non cambierà colore.

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
> Come può vedere, CSS ci offre diversi modi per mirare agli elementi, e abbiamo solo graffiato la superficie finora! Daremo un'occhiata adeguata a tutti questi selettori e molti altri più avanti nel corso.

### Stilizzare le cose in base allo stato

L'ultimo tipo di stilizzazione che esamineremo in questo tutorial è la possibilità di stilizzare le cose in base al loro stato. Un esempio semplice di questo è quando si stilizzano i link. Quando si stila un link, bisogna mirare all'elemento [`<a>`](/it/docs/Web/HTML/Reference/Elements/a) (ancora). Questo ha diversi stati a seconda che sia non visitato, visitato, passato sopra col mouse, messo a fuoco tramite tastiera, o nel processo di essere cliccato (attivato). Può usare CSS per mirare a questi diversi stati — il CSS qui sotto stila i link non visitati in rosa e i link visitati in verde.

```css
a:link {
  color: pink;
}

a:visited {
  color: green;
}
```

Può cambiare l'aspetto del link quando l'utente ci passa sopra col mouse, per esempio rimuovendo la sottolineatura, che si ottiene con la regola successiva:

```css
a:hover {
  text-decoration: none;
}
```

Nell'esempio qui sotto, può giocare con diversi valori per i vari stati di un link. Abbiamo aggiunto le regole sopra, e ora ci rendiamo conto che il colore rosa è piuttosto leggero e difficile da leggere — perché non cambiarlo in un colore migliore? Può rendere i link in grassetto?

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

Abbiamo rimosso la sottolineatura sul nostro link al passaggio del mouse. Potrebbe rimuovere la sottolineatura da tutti gli stati di un link. Vale la pena ricordare che in un sito reale, desidera assicurarsi che i visitatori sappiano che un link è un link. Lasciare la sottolineatura può essere un indizio importante per le persone per rendersi conto che un testo all'interno di un paragrafo può essere cliccato — questo è il comportamento a cui sono abituati. Come con tutto in CSS, c'è il potenziale per rendere il documento meno accessibile con le sue modifiche — cercheremo di evidenziare i potenziali rischi nei punti appropriati.

> [!NOTE]
> Spesso vedrà menzionare [l'accessibilità](/it/docs/Learn_web_development/Core/Accessibility) in queste lezioni e su MDN. Quando parliamo di accessibilità ci riferiamo alla necessità che le nostre pagine web siano comprensibili e utilizzabili da tutti, che utilizzino un computer con un mouse o un trackpad, un telefono con un touchscreen, navigando solo usando la tastiera o tramite un lettore schermo, che legge ad alta voce il contenuto del documento.

### Combinare selettori e combinatori

Vale la pena notare che può combinare insieme più selettori e combinatori. Ad esempio:

```css
/* selects any <span> that is inside a <p>, which is inside an <article>  */
article p span {
}

/* selects any <p> that comes directly after a <ul>, which comes directly after an <h1>  */
h1 + ul + p {
}
```

Può combinare anche più tipi insieme. Provi ad aggiungere quanto segue nel suo codice:

```css
h1 + p .special {
  color: yellow;
  background-color: black;
  padding: 5px;
}
```

Questo stilerà qualsiasi elemento con una classe `special`, che è all'interno di un `<p>`, che viene subito dopo un `<h1>`. Uff! Questo dovrebbe mirare all'elemento `<span class="special">elemento span</span>` nel suo codice.

Non si preoccupi se al momento sembra complicato — presto comincerà a padroneggiarlo mentre scrive più CSS.

## Altre caratteristiche sintattiche del CSS

Ora che abbiamo giocato con alcune caratteristiche del CSS, faremo un tour panoramico di alcune delle altre caratteristiche sintattiche del CSS che incontrerà durante il corso. Se vuole cercare maggiori dettagli su qualcuno di questi, può provare a digitare il nome della funzionalità nel campo di ricerca in alto a questa pagina, o sfogliare il riferimento MDN [CSS](/it/docs/Web/CSS/Reference).

Per sperimentare con i snippet di codice in ogni caso, potrebbe aggiungere l'HTML e il CSS forniti all'esempio locale o all'istanza del Playground MDN su cui ha lavorato sopra.

### Funzioni

Mentre la maggior parte dei valori sono parole chiave o valori numerici relativamente semplici, ci sono alcuni valori che assumono la forma di una funzione.

#### La funzione calc()

Un esempio sarebbe la funzione `calc()`, che può fare calcoli semplici all'interno di CSS:

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

Questo produce:

{{EmbedLiveSample('The_calc_function', '100%', 200)}}

Una funzione è composta dal nome della funzione e parentesi per racchiudere i valori per la funzione. Nel caso dell'esempio `calc()` sopra, i valori definiscono la larghezza di questa casella per essere il 90% della larghezza del blocco contenitore meno 30 pixel.

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

L'output del codice sopra appare così:

{{EmbedLiveSample('Transform_functions', '100%', 200)}}

Cerca valori diversi di proprietà elencati qui sotto. Provi a scrivere regole CSS che applicano lo stile a diversi elementi HTML utilizzando le seguenti funzioni:

- {{cssxref("transform")}}
- {{cssxref("background-image")}}, in particolare valori di gradient
- {{cssxref("color")}}, in particolare valori rgb e hsl

### @rules

Le [@regole](/it/docs/Web/CSS/CSS_syntax/At-rule) CSS (pronunciato "at-rules") forniscono istruzioni su come dovrebbe comportarsi il CSS. Una comune @regola che probabilmente incontrerà è `@media`, che è utilizzata per creare [media queries](/it/docs/Web/CSS/CSS_media_queries). Le media queries usano la logica condizionale per applicare lo stile CSS.

Nell'esempio qui sotto, il foglio di stile definisce uno sfondo rosa predefinito per l'elemento `<body>`. Tuttavia, segue una media query che imposta uno sfondo blu sull'elemento `<body>` se la finestra del browser è più larga di 30em.

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

Incontrerà altre `@regole` lungo il corso.

### Proprietà shorthand

Alcune proprietà come {{cssxref("font")}}, {{cssxref("background")}}, {{cssxref("padding")}}, {{cssxref("border")}}, e {{cssxref("margin")}} sono chiamate **proprietà shorthand**. Questo perché le proprietà shorthand impostano diversi valori in una sola riga.

Ad esempio, questa riga di codice:

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

Più avanti nel corso, incontrerà molti altri esempi di proprietà shorthand. Per ora, provi a usare le dichiarazioni sopra (o altre che potrebbe conoscere) nel suo codice per diventare più familiare con il loro funzionamento.

### Commenti CSS

Come con qualsiasi lavoro di codifica, è buona pratica scrivere commenti nel suo CSS. Questo la aiuta a ricordare come funziona il codice quando torna in seguito a fare correzioni o miglioramenti. Aiuta anche gli altri a capire il codice.

I commenti CSS iniziano con `/*` e terminano con `*/`. Nell'esempio qui sotto, i commenti segnano l'inizio di sezioni distinte di codice. Questo aiuta a navigare nel codice man mano che cresce. Con questo tipo di commento in atto, cercare commenti nel suo editor di codice diventa un modo per trovare efficacemente una sezione di codice.

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

"Commentare" il codice è anche utile per disabilitare temporaneamente sezioni di codice per i test. Nell'esempio qui sotto, le regole per `.special` sono disabilitate "commentando" il codice.

```css
/*.special {
  color: red;
}*/

p {
  color: blue;
}
```

Provi ad aggiungere commenti nel suo CSS.

### Spazi bianchi nel CSS

Gli spazi bianchi significano spazi reali, tabulazioni e nuove righe. Proprio come i browser ignorano gli spazi bianchi extra nell'HTML, i browser ignorano gli spazi bianchi extra nel CSS. Il vantaggio degli spazi bianchi è che rende più facile leggere il codice.

Nell'esempio qui sotto, ogni dichiarazione (e inizio/fine regola) ha la sua riga. Questo è senza dubbio un buon modo di scrivere CSS. Rende più facile mantenere e capire CSS.

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

Il prossimo esempio mostra lo stesso CSS in un formato più compresso, con tutti gli spazi bianchi extra rimossi. Anche se i due esempi funzionano allo stesso modo, quello qui sotto è più difficile da leggere.

```css-nolint
body{font:1em/150% Helvetica,Arial,sans-serif;padding:1em;margin:0 auto;max-width:33em;}
@media(min-width:70em){body{font-size:130%;}}
h1{font-size:1.5em;}
```

Tenga a mente che rimuovere alcuni spazi bianchi può causare errori. I nomi delle proprietà non contengono mai spazi bianchi, mentre i valori delle proprietà che si aspettano spazi bianchi tra valori multipli saranno invalidi se quello spazio è rimosso. Ad esempio, queste dichiarazioni sono CSS valido:

```css
margin: 0 auto;
padding-left: 10px;
```

Ma queste dichiarazioni sono non valide:

```css example-bad
margin: 0auto;
padding- left: 10px;
```

Vede gli errori di spaziatura? Prima, `0auto` non è riconosciuto come un valore valido per la proprietà `margin`. L'entrata `0auto` è intesa come due valori separati: `0` e `auto`. In secondo luogo, il browser non riconosce `padding-` come una proprietà valida. Il nome corretto della proprietà (`padding-left`) ha uno spazio inserito al suo interno.

Dovrebbe sempre assicurarsi di separare i valori distinti l'uno dall'altro con almeno uno spazio. Tenga i nomi delle proprietà e i valori delle proprietà insieme come stringhe singole e ininterrotte.

Per scoprire come lo spacing può rompere il CSS, provi a giocare con lo spacing all'interno del suo CSS di prova.

## Riepilogo

In questo articolo, abbiamo esaminato una serie di modi in cui può stilizzare un documento utilizzando CSS. Svilupperemo questa conoscenza man mano che avanziamo nel resto delle lezioni. Tuttavia, ora sa già abbastanza per stilizzare il testo e applicare CSS basato su diverse modalità di targeting degli elementi nel documento.

Il prossimo argomento le darà una sfida per testare le sue nuove conoscenze.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/What_is_CSS", "Learn_web_development/Core/Styling_basics/Styling_a_bio_page", "Learn_web_development/Core/Styling_basics")}}
