---
title: Introduzione a CSS
short-title: Introduzione a CSS
slug: Learn_web_development/Core/Styling_basics/Getting_started
l10n:
  sourceCommit: 85fccefc8066bd49af4ddafc12c77f35265c7e2d
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/What_is_CSS", "Learn_web_development/Core/Styling_basics/Styling_a_bio_page", "Learn_web_development/Core/Styling_basics")}}

In questo articolo verrà preso un semplice documento HTML e gli verrà applicato CSS, apprendendo nel frattempo alcuni dettagli pratici del linguaggio. Verranno inoltre esaminate alcune funzionalità aggiuntive della sintassi CSS non ancora viste.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software">Software di base installato</a>, conoscenza di base del
        <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files">lavoro con i file</a> e nozioni fondamentali di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">Introduzione a HTML</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Applicare CSS a un documento HTML.</li>
          <li>Esperienza pratica nella scrittura di CSS di base.</li>
          <li>Conoscenza operativa dei tipi fondamentali di selettori e combinatori.</li>
          <li>Il concetto di stato applicato a CSS.</li>
          <li>Familiarità con altre funzionalità della sintassi CSS, quali at-rule, funzioni, proprietà shorthand e spazi bianchi.</li>
        <ul>
      </td>
    </tr>
  </tbody>
</table>

## Partire da un po' di HTML

Il punto di partenza è un documento HTML. È possibile copiare il codice seguente per lavorare sul proprio computer. Salvare il codice seguente come `index.html` in una cartella del computer.

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

Il rendering è il seguente:

{{EmbedLiveSample("unstyled", "", "240px")}}

> [!NOTE]
> Se si sta leggendo questa pagina su un dispositivo o in un ambiente in cui non è facile creare file, non preoccuparti: fare clic sul pulsante "Play" nell'esempio live precedente per aprirlo nel MDN Playground. Qui è possibile modificare il codice CSS e HTML come indicato più avanti e vedere i risultati combinati in tempo reale.

## Aggiungere CSS al documento

La prima cosa da fare è indicare al documento HTML che sono presenti alcune regole CSS da utilizzare. Esistono tre modi diversi e comuni per applicare CSS a un documento HTML: fogli di stile esterni, fogli di stile interni e stili inline. Vediamoli ora.

Se si sta seguendo questo articolo usando MDN Playground, non sarà possibile seguire i passaggi descritti in questa sezione nello stesso modo di chi scrive il codice sul proprio computer locale. Questo perché MDN Playground gestisce implicitamente in background l'aggiunta di CSS all'HTML. È comunque opportuno leggere la sezione per conoscere questi contenuti.

### Fogli di stile esterni

Un foglio di stile esterno contiene CSS in un file separato con estensione `.css`. Questo è il metodo più comune e utile per aggiungere CSS a un documento. È possibile collegare un singolo file CSS a più pagine web, applicando lo stile a tutte con lo stesso foglio di stile CSS.

Creare un file nella stessa cartella del documento HTML e salvarlo come `styles.css`.

Per collegare `styles.css` a `index.html`, aggiungere la riga seguente in un punto qualsiasi all'interno di {{htmlelement("head")}} del documento HTML:

```html
<link rel="stylesheet" href="styles.css" />
```

Questo elemento {{htmlelement("link")}} indica al browser che è presente un foglio di stile, usando l'attributo `rel`, e la posizione di quel foglio di stile come valore dell'attributo `href`. È possibile verificare che il CSS funzioni aggiungendo una regola a `styles.css`. Usando l'editor di codice, aggiungere quanto segue al file CSS:

```css
h1 {
  color: red;
}
```

Salvare i file HTML e CSS e ricaricare la pagina in un browser web. Il titolo di livello uno nella parte superiore del documento dovrebbe ora essere rosso. Se accade, congratulazioni: CSS è stato applicato correttamente a un documento HTML. Se non accade, controllare attentamente di aver digitato tutto correttamente.

#### Collocare i fogli di stile in posizioni diverse

Nell'esempio precedente, il file CSS si trova nella stessa cartella del documento HTML, ma potrebbe essere collocato altrove e il percorso potrebbe essere modificato (allo stesso modo delle [immagini HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_images)). Ecco tre esempi:

```html
<!-- In a subdirectory called styles in the current directory -->
<link rel="stylesheet" href="styles/style.css" />

<!-- In a subdirectory called general, which is in a subdirectory called styles, in the current directory -->
<link rel="stylesheet" href="styles/general/style.css" />

<!-- Go back one directory level, then in a subdirectory called styles -->
<link rel="stylesheet" href="../styles/style.css" />
```

### Fogli di stile interni

I fogli di stile interni sono contenuti negli elementi {{htmlelement("style")}}, che si trovano all'interno di {{htmlelement("head")}} HTML. Creiamone uno ora.

Nel documento HTML, aggiungere il seguente frammento in un punto qualsiasi tra i tag `<head>` e `</head>`:

```html
<style>
  p {
    color: purple;
  }
</style>
```

Salvare e aggiornare la pagina: tutti i paragrafi dovrebbero diventare viola.

In alcune circostanze, i fogli di stile interni possono essere utili. Ad esempio, si potrebbe lavorare con un sistema di gestione dei contenuti che impedisce la modifica dei file CSS esterni.

Tuttavia, per siti con più di una pagina, i fogli di stile interni sono meno efficienti dei fogli di stile esterni. Per applicare uno stile CSS uniforme a più pagine usando fogli di stile interni, occorre ripetere il foglio di stile interno in ogni pagina web. La penalizzazione in termini di efficienza si applica anche alla manutenzione del sito. Con CSS nei fogli di stile interni, persino una semplice modifica allo stile potrebbe richiedere modifiche a più pagine web.

Prima di proseguire, rimuovere l'elemento `<style>` e il suo contenuto dall'esempio HTML.

### Stili inline

Gli stili inline sono dichiarazioni CSS che interessano un singolo elemento HTML e sono contenute in un attributo `style`. Proviamo ora a implementarne uno.

Aggiungere un attributo `style` all'elemento {{htmlelement("span")}} nell'HTML, in modo che appaia come segue:

```html
<span style="color: purple; font-weight: bold">span element</span>
```

Salvare e aggiornare la pagina: dovrebbe diventare viola e in grassetto solo il testo all'interno di `<span>`. Provare ad aggiungere altre dichiarazioni nell'attributo `style` (separate da punti e virgola), oppure ulteriori attributi `style` ad altri elementi.

Una volta terminati gli esperimenti, rimuovere tutti gli attributi `style`.

**Evitare di usare CSS in questo modo, se possibile.** È una cattiva pratica. Innanzitutto, è l'implementazione meno efficiente di CSS per quanto riguarda la manutenzione. Una modifica allo stile potrebbe richiedere più interventi all'interno di una singola pagina web. In secondo luogo, il CSS inline mescola anche il codice di presentazione (CSS) con HTML e contenuto, rendendo tutto più difficile da leggere e comprendere. Separare codice e contenuto rende la manutenzione più semplice per chiunque lavori sul sito web.

Potrebbe essere necessario ricorrere agli stili inline se l'ambiente di lavoro è molto restrittivo. Ad esempio, il CMS potrebbe consentire di modificare solo il corpo HTML. Si possono anche vedere molti stili inline nelle email HTML per ottenere compatibilità con il maggior numero possibile di client email. È inoltre abbastanza comune impostare stili inline quando si applica dinamicamente lo stile usando JavaScript.

## Usare selettori comuni

In questa sezione verrà presentata una breve panoramica di alcuni dei tipi di selettore più comuni che si incontreranno.

### Selezionare elementi HTML

Rendendo rosso il titolo, è già stato dimostrato che è possibile individuare e applicare uno stile a un elemento HTML. Questo avviene individuando un **selettore di elemento** (noto anche come **selettore di tipo**): un selettore che corrisponde direttamente al nome di un elemento HTML. Per individuare tutti i paragrafi del documento, si userebbe il selettore `p`. Per rendere verdi tutti i paragrafi, si userebbe:

```css
p {
  color: green;
}
```

È possibile individuare più selettori contemporaneamente separandoli con una virgola. Se si volessero rendere verdi tutti i paragrafi e tutti gli elementi delle liste, la regola sarebbe simile a questa:

```css
p,
li {
  color: green;
}
```

Provarlo nell'esempio seguente (fare clic su "Play") o nella copia locale:

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

La lezione interattiva seguente insegna i concetti CSS di base e offre un po' di pratica.

<mdn-scrim-inline url="https://scrimba.com/frontend-path-c0j/~015" scrimtitle="Scrivi le prime righe di CSS!"></scrim-inline>

### Aggiungere una classe

Finora, gli elementi sono stati stilizzati in base ai nomi degli elementi HTML. Questo funziona finché si desidera che tutti gli elementi di quel tipo nel documento abbiano lo stesso aspetto. Per selezionare un sottoinsieme di elementi senza modificare gli altri, è possibile aggiungere una `class` all'elemento HTML e individuare quella classe nel CSS.

1. Nel documento HTML, aggiungere un [attributo class](/it/docs/Web/HTML/Reference/Global_attributes/class) al secondo elemento della lista. La lista sarà ora simile a questa:

   ```html
   <ul>
     <li>Item one</li>
     <li class="special">Item two</li>
     <li>Item <em>three</em></li>
   </ul>
   ```

2. Nel CSS, è possibile individuare la classe `special` creando un selettore che inizia con un punto. Aggiungere quanto segue al file CSS:

   ```css
   .special {
     color: orange;
     font-weight: bold;
   }
   ```

3. Salvare e aggiornare la pagina per vedere il risultato.

Ora è possibile applicare la classe `special` ad altri elementi della pagina a cui si desidera assegnare lo stesso aspetto di questo elemento della lista. Aggiungere una classe `special` a `<span>` all'interno del paragrafo, quindi ricaricare la pagina: ora dovrebbe anch'esso essere arancione e in grassetto.

### Stilizzare elementi in base alla loro posizione in un documento

A volte si desidera che qualcosa abbia un aspetto diverso in base alla sua posizione nel documento. Esistono numerosi selettori che possono essere utili a questo scopo, ma per ora ne verranno esaminati solo un paio. Nel documento sono presenti due elementi `<em>`: uno all'interno di un paragrafo e l'altro all'interno di un elemento della lista. Per selezionare solo un `<em>` annidato all'interno di un elemento `<li>`, è possibile usare un selettore chiamato **combinatore discendente**, che ha la forma di uno spazio tra altri due selettori.

Aggiungere la seguente regola al foglio di stile:

```css
li em {
  color: rebeccapurple;
}
```

Questo selettore selezionerà qualsiasi elemento `<em>` che sia un discendente di un `<li>`. Quindi, nel documento di esempio, l'elemento `<em>` nel terzo elemento della lista dovrebbe ora essere viola, mentre quello all'interno del paragrafo rimane invariato.

Un'altra cosa da provare è stilizzare un paragrafo quando viene immediatamente dopo un titolo allo stesso livello gerarchico nell'HTML. Per farlo, inserire un `+` (un **combinatore del fratello successivo**) tra i selettori.

Provare ad aggiungere anche questa regola al foglio di stile:

```css
h1 + p {
  font-size: 200%;
}
```

L'esempio live seguente include le due regole precedenti. Provare ad aggiungere una regola per rendere rosso uno span se si trova all'interno di un paragrafo. Sarà chiaro che la regola è corretta perché lo span nel primo paragrafo sarà rosso, mentre quello nel primo elemento della lista non cambierà colore.

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
> Come si può vedere, CSS offre diversi modi per individuare gli elementi e finora è stata solo scalfita la superficie. Più avanti nel corso verranno esaminati approfonditamente tutti questi selettori e molti altri.

### Stilizzare elementi in base allo stato

L'ultimo tipo di stile che verrà esaminato in questo tutorial è la capacità di stilizzare elementi in base al loro stato. Un esempio immediato è lo stile dei link. Quando si applica lo stile a un link, è necessario individuare l'elemento [`<a>`](/it/docs/Web/HTML/Reference/Elements/a) (anchor). Questo presenta stati diversi a seconda che non sia stato visitato, sia stato visitato, vi si passi sopra con il puntatore, riceva il focus tramite tastiera oppure sia in fase di clic (attivazione). È possibile usare CSS per individuare questi diversi stati: il CSS seguente rende rosa i link non visitati e verdi quelli visitati.

```css
a:link {
  color: pink;
}

a:visited {
  color: green;
}
```

È possibile modificare l'aspetto del link quando l'utente vi passa sopra con il puntatore, ad esempio rimuovendo la sottolineatura, operazione ottenuta con la regola successiva:

```css
a:hover {
  text-decoration: none;
}
```

Nell'esempio seguente è possibile sperimentare diversi valori per i vari stati di un link. Sono state aggiunte le regole precedenti e ora il colore rosa appare piuttosto chiaro e difficile da leggere: perché non cambiarlo con un colore migliore? È possibile rendere i link in grassetto?

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

La sottolineatura del link è stata rimossa al passaggio del puntatore. Potrebbe essere rimossa la sottolineatura da tutti gli stati di un link. Tuttavia, vale la pena ricordare che in un sito reale occorre assicurarsi che i visitatori riconoscano un link come tale. Lasciare la sottolineatura può essere un indizio importante affinché le persone capiscano che un testo all'interno di un paragrafo può essere selezionato: è il comportamento a cui sono abituate. Come per ogni altra cosa in CSS, le modifiche possono rendere il documento meno accessibile: verranno evidenziate le potenziali insidie nei punti appropriati.

> [!NOTE]
> In queste lezioni e in tutto MDN viene spesso menzionata l'[accessibilità](/it/docs/Learn_web_development/Core/Accessibility). Quando si parla di accessibilità, ci si riferisce al requisito secondo cui le pagine web devono essere comprensibili e utilizzabili da tutti, indipendentemente dal fatto che si utilizzi un computer con mouse o trackpad, un telefono con touchscreen, soltanto la tastiera per navigare oppure un lettore di schermo che legge ad alta voce il contenuto del documento.

### Combinare selettori e combinatori

Vale la pena notare che è possibile combinare più selettori e combinatori. Ad esempio:

```css
/* selects any <span> that is inside a <p>, which is inside an <article>  */
article p span {
}

/* selects any <p> that comes directly after a <ul>, which comes directly after an <h1>  */
h1 + ul + p {
}
```

È anche possibile combinare più tipi. Provare ad aggiungere quanto segue al codice:

```css
h1 + p .special {
  color: yellow;
  background-color: black;
  padding: 5px;
}
```

Questo applicherà lo stile a qualsiasi elemento con classe `special` che si trovi all'interno di un `<p>`, il quale viene immediatamente dopo un `<h1>`. Uff! Questo dovrebbe individuare l'elemento `<span class="special">span element</span>` nel codice.

Non preoccuparti se al momento sembra complicato: scrivendo altro CSS, si inizierà presto a prenderci la mano.

## Altre funzionalità della sintassi CSS

Ora che sono state provate alcune funzionalità CSS, verrà offerta una panoramica generale di alcune delle altre funzionalità della sintassi CSS che si incontreranno durante il corso. Per cercare maggiori dettagli su una qualsiasi di esse, è possibile digitare il nome della funzionalità nel campo di ricerca in cima a questa pagina oppure consultare il [riferimento CSS](/it/docs/Web/CSS/Reference) di MDN.

Per sperimentare i frammenti di codice in ciascun caso, è possibile aggiungere l'HTML e il CSS forniti all'esempio locale o all'istanza di MDN Playground usata in precedenza.

### Funzioni

Sebbene la maggior parte dei valori siano parole chiave o valori numerici relativamente semplici, alcuni valori assumono la forma di una funzione.

#### La funzione calc()

Un esempio è la funzione `calc()`, che può eseguire semplici calcoli all'interno di CSS:

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

Il rendering è il seguente:

{{EmbedLiveSample('The_calc_function', '100%', 200)}}

Una funzione è composta dal nome della funzione e da parentesi che racchiudono i valori della funzione. Nel caso dell'esempio `calc()` precedente, i valori definiscono la larghezza di questo riquadro come il 90% della larghezza del blocco contenitore, meno 30 pixel.

#### Funzioni di trasformazione

Un altro esempio sono i vari valori della proprietà {{cssxref("transform")}}, come `rotate()`.

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

L'output del codice precedente è simile al seguente:

{{EmbedLiveSample('Transform_functions', '100%', 200)}}

Cercare i diversi valori delle proprietà elencate di seguito. Provare a scrivere regole CSS che applichino stili a diversi elementi HTML usando le seguenti funzioni:

- {{cssxref("transform")}}
- {{cssxref("background-image")}}, in particolare i valori gradiente
- {{cssxref("color")}}, in particolare i valori rgb e hsl

### @rule

Le [@rule](/it/docs/Web/CSS/Guides/Syntax/At-rules) CSS (pronunciate "at-rules") forniscono istruzioni su come CSS deve comportarsi. Una @rule comune che probabilmente si incontrerà è `@media`, usata per creare [media query](/it/docs/Web/CSS/Guides/Media_queries). Le media query usano logica condizionale per applicare lo stile CSS.

Nell'esempio seguente, il foglio di stile definisce uno sfondo rosa predefinito per l'elemento `<body>`. Tuttavia, segue una media query che imposta uno sfondo blu sull'elemento `<body>` se la viewport del browser è più ampia di `30em`.

```css
body {
  background-color: pink;
}

@media (width >= 30em) {
  body {
    background-color: blue;
  }
}
```

### Proprietà shorthand

Alcune proprietà, come {{cssxref("font")}}, {{cssxref("background")}}, {{cssxref("padding")}}, {{cssxref("border")}} e {{cssxref("margin")}}, sono chiamate **proprietà shorthand**. Questo perché le proprietà shorthand impostano più valori in una singola riga.

Ad esempio, questa singola riga di codice:

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

Questa singola riga:

```css
background: red url("bg-graphic.png") 10px 10px repeat-x fixed;
```

è equivalente a queste cinque righe:

```css
background-color: red;
background-image: url("bg-graphic.png");
background-position: 10px 10px;
background-repeat: repeat-x;
background-attachment: fixed;
```

Più avanti nel corso si incontreranno molti altri esempi di proprietà shorthand. Per ora, provare a usare le dichiarazioni precedenti, o altre già note, nel proprio codice per acquisire maggiore familiarità con il loro funzionamento.

### Commenti CSS

Come per qualsiasi lavoro di programmazione, è buona pratica scrivere commenti nel CSS. Questo aiuta a ricordare come funziona il codice quando vi si torna in seguito per apportare correzioni o miglioramenti. Aiuta anche altre persone a comprendere il codice.

I commenti CSS iniziano con `/*` e terminano con `*/`. Nell'esempio seguente, i commenti contrassegnano l'inizio di sezioni distinte del codice. Ciò aiuta a navigare nella codebase man mano che diventa più grande. Con questo tipo di commenti, cercare i commenti nell'editor di codice diventa un modo efficiente per trovare una sezione di codice.

```css
/* Handle basic element styling */
/* ---------------------------- */
body {
  font:
    1em/150% "Helvetica",
    "Arial",
    sans-serif;
  padding: 1em;
  margin: 0 auto;
  max-width: 33em;
}

@media (width >= 70em) {
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
#id::first-line {
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

Anche "commentare" il codice è utile per disabilitare temporaneamente sezioni di codice a scopo di test. Nell'esempio seguente, le regole per `.special` sono disabilitate commentando il codice.

```css
/* .special {
  color: red;
} */

p {
  color: blue;
}
```

Provare ad aggiungere commenti al CSS.

### Spazi bianchi in CSS

Per spazi bianchi si intendono spazi effettivi, tabulazioni e nuove righe. Proprio come i browser ignorano gli spazi bianchi aggiuntivi nell'HTML, ignorano anche quelli aggiuntivi all'interno di CSS. Il vantaggio degli spazi bianchi è che rendono il codice più facile da leggere.

Nell'esempio seguente, ogni dichiarazione, nonché l'inizio e la fine di ogni regola, si trova sulla propria riga. Questo è probabilmente un buon modo di scrivere CSS: rende CSS più semplice da mantenere e comprendere.

```css
body {
  font:
    1em/150% "Helvetica",
    "Arial",
    sans-serif;
  padding: 1em;
  margin: 0 auto;
  max-width: 33em;
}

@media (width >= 70em) {
  body {
    font-size: 130%;
  }
}

h1 {
  font-size: 1.5em;
}
```

L'esempio successivo mostra lo stesso CSS in un formato più compresso, con tutti gli spazi bianchi aggiuntivi rimossi. Sebbene i due esempi funzionino allo stesso modo, quello seguente è più difficile da leggere.

```css-nolint
body{font:1em/150% "Helvetica","Arial",sans-serif;padding:1em;margin:0 auto;max-width:33em;}
@media(width>=70em){body{font-size:130%;}}
h1{font-size:1.5em;}
```

Tenere presente che alcune modifiche agli spazi bianchi possono causare errori. I nomi delle proprietà non contengono mai spazi bianchi, mentre i valori delle proprietà che richiedono spazi bianchi tra più valori diventano non validi se quello spazio viene rimosso. Ad esempio, queste dichiarazioni sono CSS valido:

```css
margin: 0 auto;
padding-left: 10px;
```

Ma queste dichiarazioni non sono valide:

```css example-bad
margin: 0auto;
padding- left: 10px;
```

Si notano gli errori di spaziatura? Innanzitutto, `0auto` non viene riconosciuto come valore valido per la proprietà `margin`. La voce `0auto` dovrebbe essere costituita da due valori distinti: `0` e `auto`. In secondo luogo, il browser non riconosce `padding-` come proprietà valida. Il nome corretto della proprietà (`padding-left`) non contiene spazi.

Bisogna sempre assicurarsi di separare valori distinti tra loro con almeno uno spazio. Mantenere nomi e valori delle proprietà uniti come singole stringhe ininterrotte.

Per scoprire come la spaziatura può compromettere CSS, provare a modificare gli spazi nel CSS di prova.

## Riepilogo

In questo articolo sono stati esaminati diversi modi per applicare stile a un documento usando CSS. Queste conoscenze verranno sviluppate nel resto delle lezioni. Tuttavia, ora si sa già abbastanza per stilizzare il testo e applicare CSS in base a diversi modi di individuare gli elementi nel documento.

Il prossimo passo sarà una sfida per mettere alla prova le nuove conoscenze.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/What_is_CSS", "Learn_web_development/Core/Styling_basics/Styling_a_bio_page", "Learn_web_development/Core/Styling_basics")}}
