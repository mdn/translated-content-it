---
title: Il modello a scatola
short-title: Modello a scatola
slug: Learn_web_development/Core/Styling_basics/Box_model
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Combinators", "Learn_web_development/Core/Styling_basics/Handling_conflicts", "Learn_web_development/Core/Styling_basics")}}

Tutto in CSS ha una scatola intorno, e comprendere queste scatole è fondamentale per poter creare layout più complessi con CSS o per allineare elementi con altri elementi. In questa lezione, esamineremo il _modello a scatola_ di CSS. Otterrà una comprensione di come funziona e della terminologia correlata.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >)
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Elementi blocco e inline</li>
          <li>Le diverse scatole che compongono un elemento e come stilizzarle — contenuto, margine, bordo, padding.</li>
          <li>Il modello a scatola alternativo (accessibile tramite <code>box-sizing: border-box</code>) e come differisce dal modello a scatola regolare.</li>
          <li>Collasso del margine.</li>
          <li>Valori di display di base e come influenzano il comportamento della scatola — <code>block</code>, <code>inline</code>, <code>inline-block</code>, <code>none</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Scatole blocco e inline

In CSS abbiamo diversi tipi di scatole che generalmente rientrano nelle categorie delle **scatole blocco** e delle **scatole inline**. Il tipo si riferisce a come la scatola si comporta in termini di flusso della pagina e in relazione ad altre scatole nella pagina. Le scatole hanno un **tipo di display interno** e un **tipo di display esterno**.

In generale, può impostare vari valori per il tipo di display utilizzando la proprietà {{cssxref("display")}}, che può avere vari valori.

Se una scatola ha un valore di display `block`, allora:

- La scatola andrà su una nuova linea.
- Le proprietà {{cssxref("width")}} e {{cssxref("height")}} sono rispettate.
- Il padding, il margine e il bordo faranno sì che altri elementi si allontanino dalla scatola.
- Se la {{cssxref("width")}} non è specificata, la scatola si estenderà nella direzione inline per riempire lo spazio disponibile nel suo contenitore. Nella maggior parte dei casi, la scatola diventerà larga quanto il suo contenitore, riempiendo il 100% dello spazio disponibile.

Alcuni elementi HTML, come `<h1>` e `<p>`, usano `block` come loro tipo di display esterno di default.

Se una scatola ha un tipo di display `inline`, allora:

- La scatola non andrà su una nuova linea.
- Le proprietà {{cssxref("width")}} e {{cssxref("height")}} non si applicheranno.
- Il padding, i margini e i bordi superiori e inferiori si applicheranno ma non fanno sì che altre scatole inline si allontanino dalla scatola.
- Il padding, i margini e i bordi sinistro e destro si applicheranno e faranno sì che altre scatole inline si allontanino dalla scatola.

Alcuni elementi HTML, come `<a>`, `<span>`, `<em>` e `<strong>`, usano `inline` come loro tipo di display esterno di default.

Il layout blocco e inline è il modo predefinito in cui le cose si comportano sul web. Di default e senza nessun'altra istruzione, gli elementi all'interno di una scatola sono posizionati anche in **[flusso normale](/it/docs/Learn_web_development/Core/CSS_layout/Introduction#normal_layout_flow)** e si comportano come scatole blocco o inline.

## Tipi di display interno ed esterno

I valori di display `block` e `inline` sono detti tipi di **display esterno** — essi influenzano come la scatola è posizionata in relazione ad altre scatole intorno ad essa. Le scatole hanno anche un tipo di **display interno**, che detta come sono disposti gli elementi all'interno di quella scatola.

Può cambiare il tipo di display interno impostando un valore di display interno, per esempio `display: flex;`. L'elemento userà ancora il tipo di display esterno `block` ma questo cambia il tipo di display interno in `flex`. Qualsiasi figlio diretto di questa scatola diventerà elementi flex e si comporterà secondo le specifiche [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox).

Quando proseguirà nella comprensione del CSS Layout in modo più dettagliato, incontrerà [`flex`](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox) e vari altri valori interni che le sue scatole possono avere, per esempio [`grid`](/it/docs/Learn_web_development/Core/CSS_layout/Grids).

Non si preoccupi troppo della terminologia interna ed esterna per ora; questo è ciò che sta accadendo internamente, e lo abbiamo menzionato qui nel caso lo incontri altrove. In generale, si occuperà solo di valori `display` singoli e non avrà bisogno di pensarci molto.

## Esempi di diversi tipi di display

L'esempio seguente ha tre diversi elementi HTML, tutti con un tipo di display esterno `block`.

- Un paragrafo con un bordo aggiunto in CSS. Il browser lo renderizza come una scatola blocco. Il paragrafo inizia su una nuova linea e si estende per l'intera larghezza disponibile.

- Un elenco, disposto utilizzando `display: flex`. Questo stabilisce un layout flex per i figli del contenitore, che sono elementi flex. L'elenco stesso è una scatola blocco e — come il paragrafo — si espande fino all'intera larghezza del contenitore e va su una nuova linea.

- Un paragrafo a livello di blocco, all'interno del quale ci sono due elementi `<span>`. Questi elementi normalmente sarebbero `inline`, tuttavia, uno degli elementi ha una classe "block" che viene impostata su `display: block`.

```html live-sample___block
<p>I am a paragraph. A short one.</p>
<ul>
  <li>Item One</li>
  <li>Item Two</li>
  <li>Item Three</li>
</ul>
<p>
  I am another paragraph. Some of the <span class="block">words</span> have been
  wrapped in a <span>span element</span>.
</p>
```

```css live-sample___block
body {
  font-family: sans-serif;
}
p,
ul {
  border: 2px solid rebeccapurple;
  padding: 0.2em;
}

.block,
li {
  border: 2px solid blue;
  padding: 0.2em;
}

ul {
  display: flex;
  list-style: none;
}

.block {
  display: block;
}
```

{{EmbedLiveSample("block", "", "220px")}}

Nel prossimo esempio, possiamo vedere come si comportano gli elementi `inline`.

- Gli elementi `<span>` nel primo paragrafo sono inline di default e quindi non forzano interruzioni di linea.

- L'elemento `<ul>` che è impostato su `display: inline-flex` crea una scatola inline contenente alcuni elementi flex.

- I due paragrafi sono entrambi impostati su `display: inline`. Il contenitore inline flex e i paragrafi si trovano tutti insieme su una linea piuttosto che andare su nuove linee (come farebbero se venissero mostrati come elementi a livello di blocco).

Per passare tra le modalità di display, può cambiare `display: inline` in `display: block` o `display: inline-flex` in `display: flex`:

```html live-sample___inline
<p>
  I am a paragraph. Some of the
  <span>words</span> have been wrapped in a <span>span element</span>.
</p>
<ul>
  <li>Item One</li>
  <li>Item Two</li>
  <li>Item Three</li>
</ul>
<p class="inline">I am a paragraph. A short one.</p>
<p class="inline">I am another paragraph. Also a short one.</p>
```

```css live-sample___inline
body {
  font-family: sans-serif;
}
p,
ul {
  border: 2px solid rebeccapurple;
}

span,
li {
  border: 2px solid blue;
}

ul {
  display: inline-flex;
  list-style: none;
  padding: 0;
}

.inline {
  display: inline;
}
```

{{EmbedLiveSample("inline")}}

La cosa importante da ricordare per ora è: Cambiare il valore della proprietà `display` può cambiare se il tipo di display esterno di una scatola è blocco o inline. Questo cambia il modo in cui viene visualizzato insieme ad altri elementi nel layout.

## Cos'è il modello a scatola CSS?

Il modello a scatola CSS nel suo complesso si applica alle scatole blocco e definisce come le diverse parti di una scatola — margine, bordo, padding e contenuto — lavorano insieme per creare una scatola che può vedere su una pagina. Le scatole inline usano solo _alcuni_ dei comportamenti definiti nel modello a scatola.

Per aggiungere complessità, esiste un modello a scatola standard e uno alternativo. Di default, i browser usano il modello a scatola standard.

### Parti di una scatola

Costituendo una scatola blocco in CSS abbiamo:

- **Contenuto**: L'area dove viene mostrato il suo contenuto; dimensionarlo usando proprietà come {{cssxref("width")}} e {{cssxref("height")}}.
- **Padding**: Il padding si trova intorno al contenuto come spazio bianco; dimensionarlo usando {{cssxref("padding")}} e proprietà correlate.
- **Bordo**: Il bordo avvolge il contenuto e qualsiasi padding; dimensionarlo usando {{cssxref("border")}} e proprietà correlate.
- **Margine**: Il margine è lo strato più esterno, avvolgendo il contenuto, il padding e il bordo come spazio bianco tra questa scatola e altri elementi; dimensionarlo usando {{cssxref("margin")}} e proprietà correlate.

Il diagramma sotto mostra questi strati:

![Diagramma del modello a scatola](box-model.png)

### Il modello a scatola CSS standard

Nel modello a scatola standard, se imposta valori di `width` e `height` su una scatola, questi valori definiscono la `width` e `height` della _scatola del contenuto_. Qualsiasi padding e bordo sono poi aggiunti a quelle dimensioni per ottenere la dimensione complessiva occupata dalla scatola (vedi l'immagine sotto).

Se presumiamo che una scatola abbia il seguente CSS:

```css
.box {
  width: 350px;
  height: 150px;
  margin: 10px;
  padding: 25px;
  border: 5px solid black;
}
```

Lo spazio _effettivo_ occupato dalla scatola sarà largo 410px (350 + 25 + 25 + 5 + 5) e alto 210px (150 + 25 + 25 + 5 + 5).

![Mostra la dimensione della scatola quando il modello a scatola standard è usato.](standard-box-model.png)

> [!NOTE]
> Il margine non è conteggiato nelle dimensioni reali della scatola — certamente, influenza lo spazio totale che la scatola occuperà sulla pagina, ma solo lo spazio al di fuori della scatola. L'area della scatola si ferma al bordo — non si estende nel margine.

### Il modello a scatola CSS alternativo

Nel modello a scatola alternativo, qualsiasi larghezza è la larghezza della scatola visibile sulla pagina. La larghezza dell'area di contenuto è quella larghezza meno la larghezza per il padding e il bordo (vedi l'immagine sotto). Non è necessario sommare il bordo e il padding per ottenere la dimensione reale della scatola.

Per attivare il modello alternativo su un elemento, impostare `box-sizing: border-box` su di esso:

```css
.box {
  box-sizing: border-box;
}
```

Se presumiamo che la scatola abbia lo stesso CSS sopra:

```css
.box {
  width: 350px;
  height: 150px;
  margin: 10px;
  padding: 25px;
  border: 5px solid black;
}
```

Ora, lo spazio _effettivo_ occupato dalla scatola sarà 350px nella direzione inline e 150px nella direzione blocco.

![Mostra la dimensione della scatola quando il modello a scatola alternativo è usato.](alternate-box-model.png)

Per usare il modello a scatola alternativo per tutti i suoi elementi (che è una scelta comune tra gli sviluppatori), impostare la proprietà `box-sizing` sull'elemento `<html>` e impostare tutti gli altri elementi per ereditare quel valore:

```css
html {
  box-sizing: border-box;
}

*,
*::before,
*::after {
  box-sizing: inherit;
}
```

Per comprendere l'idea sottostante, può leggere [l'articolo su CSS Tricks su box-sizing](https://css-tricks.com/inheriting-box-sizing-probably-slightly-better-best-practice/).

## Giocare con i modelli a scatola

Nell'esempio sotto, può vedere due scatole. Entrambe hanno una classe `.box`, che dà loro la stessa `width`, `height`, `margin`, `border` e `padding`. L'unica differenza è che la seconda scatola è stata impostata per utilizzare il modello a scatola alternativo. Può cambiare la dimensione della seconda scatola (aggiungendo CSS alla classe `.alternate`) per farla combaciare alla prima scatola in larghezza e altezza?

```html live-sample___box-models
<div class="box">I use the standard box model.</div>
<div class="box alternate">I use the alternate box model.</div>
```

```css live-sample___box-models
.box {
  border: 5px solid rebeccapurple;
  background-color: lightgray;
  padding: 40px;
  margin: 40px;
  width: 300px;
  height: 150px;
}

.alternate {
  box-sizing: border-box;
}
```

{{EmbedLiveSample("box-models", "", "400px")}}

> [!NOTE]
> Può trovare una soluzione per questo compito [nel nostro repository css-examples](https://github.com/mdn/css-examples/blob/main/learn/solutions.md#the-box-model).

### Usare gli strumenti di sviluppo del browser per visualizzare il modello a scatola

I suoi [strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) possono rendere molto più facile la comprensione del modello a scatola — possono mostrarle la dimensione dell'elemento più il suo margine, padding e bordo. Ispezionare un elemento in questo modo è un ottimo modo per scoprire se la sua scatola è davvero della dimensione che pensa!

![Ispezionando il modello a scatola di un elemento utilizzando Firefox DevTools](box-model-devtools.png)

## Margini, padding e bordi

Ha già visto le proprietà {{cssxref("margin")}}, {{cssxref("padding")}} e {{cssxref("border")}} al lavoro nell'esempio sopra. Le proprietà utilizzate in quell'esempio sono **abbreviate** e ci permettono di impostare tutti e quattro i lati della scatola contemporaneamente. Queste abbreviazioni hanno anche proprietà in forma estesa equivalenti, che permettono di controllare i diversi lati della scatola individualmente.

Esploriamo queste proprietà in modo più dettagliato.

### Margine

Il margine è uno spazio invisibile intorno alla sua scatola. Allontana altri elementi dalla scatola. I margini possono avere valori positivi o negativi. Impostare un margine negativo su un lato della sua scatola può causare la sovrapposizione ad altre cose sulla pagina. Che lei stia usando il modello a scatola standard o alternativo, il margine viene sempre aggiunto dopo che la dimensione della scatola visibile è stata calcolata.

Possiamo controllare tutti i margini di un elemento contemporaneamente utilizzando la proprietà {{cssxref("margin")}}, o ciascun lato individualmente utilizzando le proprietà in forma estesa equivalenti:

- {{cssxref("margin-top")}}
- {{cssxref("margin-right")}}
- {{cssxref("margin-bottom")}}
- {{cssxref("margin-left")}}

Nell'esempio sotto, provi a cambiare i valori dei margini per vedere come la scatola viene spostata a causa del margine che crea o rimuove spazio (se è un margine negativo) tra questo elemento e l'elemento contenitore.

```html live-sample___margin
<div class="container">
  <div class="box">Change my margin.</div>
</div>
```

```css live-sample___margin
.container {
  border: 5px solid blue;
  margin: 40px;
}

.box {
  border: 5px solid rebeccapurple;
  background-color: lightgray;
  padding: 10px;
  height: 100px;
  /* try changing the margin properties: */
  margin-top: -40px;
  margin-right: 30px;
  margin-bottom: 40px;
  margin-left: 4em;
}
```

{{EmbedLiveSample("margin", "", "220px")}}

#### Collasso del margine

A seconda che due elementi i cui margini si toccano abbiano margini positivi o negativi, i risultati saranno diversi:

- Due margini positivi si combineranno per diventare un margine. La sua dimensione sarà uguale al più grande margine individuale.
- Due margini negativi si collasseranno e verrà usato il valore più piccolo (più lontano da zero).
- Se un margine è negativo, il suo valore sarà _sottratto_ dal totale.

Nell'esempio sotto, abbiamo due paragrafi. Il paragrafo superiore ha un `margin-bottom` di 50 pixel, l'altro ha un `margin-top` di 30 pixel. I margini si sono uniti insieme, quindi il margine effettivo tra le scatole è di 50 pixel e non il totale dei due margini.

Può testarlo impostando il `margin-top` del secondo paragrafo a `0`. Il margine visibile tra i due paragrafi non cambierà — mantiene i 50 pixel impostati nel `margin-bottom` del primo paragrafo. Se lo imposta a `-10px`, vedrà che il margine complessivo diventa `40px` — si sottrae da `50px`.

```html live-sample___margin-collapse
<div class="container">
  <p class="one">I am paragraph one.</p>
  <p class="two">I am paragraph two.</p>
</div>
```

```css live-sample___margin-collapse
.container {
  border: 5px solid blue;
  margin: 40px;
}

p {
  border: 5px solid rebeccapurple;
  background-color: lightgray;
  padding: 10px;
}
.one {
  margin-bottom: 50px;
}

.two {
  margin-top: 30px;
}
```

{{EmbedLiveSample("margin-collapse", "", "280px")}}

Un numero di regole stabilisce quando i margini crollano e quando no. Per ulteriori informazioni, vedere la pagina dettagliata su [padroneggiare il collasso dei margini](/it/docs/Web/CSS/CSS_box_model/Mastering_margin_collapsing). La cosa principale da ricordare è che il collasso del margine è un evento che accade se sta creando spazio con i margini e non ottiene lo spazio che si aspetta.

### Bordi

Il bordo viene disegnato tra il margine e il padding di una scatola. Se sta usando il modello a scatola standard, la dimensione del bordo viene aggiunta alla `width` e `height` della scatola del contenuto. Se sta usando il modello a scatola alternativo, allora più grande è il bordo, più piccola è la scatola del contenuto, poiché il bordo occupa una parte di quella `width` e `height` disponibili della scatola dell'elemento.

Per stilizzare i bordi, ci sono un gran numero di proprietà — ci sono quattro bordi, e ogni bordo ha uno stile, una larghezza, e un colore che potremmo voler manipolare.

Può impostare la larghezza, lo stile o il colore di tutti e quattro i bordi contemporaneamente usando la proprietà {{cssxref("border")}}.

Per impostare le proprietà di ciascun lato individualmente, usare:

- {{cssxref("border-top")}}
- {{cssxref("border-right")}}
- {{cssxref("border-bottom")}}
- {{cssxref("border-left")}}

Per impostare la larghezza, lo stile o il colore di tutti i lati, usare:

- {{cssxref("border-width")}}
- {{cssxref("border-style")}}
- {{cssxref("border-color")}}

Per impostare la larghezza, lo stile o il colore di un solo lato, usare una delle proprietà più granulari in forma estesa:

- {{cssxref("border-top-width")}}
- {{cssxref("border-top-style")}}
- {{cssxref("border-top-color")}}
- {{cssxref("border-right-width")}}
- {{cssxref("border-right-style")}}
- {{cssxref("border-right-color")}}
- {{cssxref("border-bottom-width")}}
- {{cssxref("border-bottom-style")}}
- {{cssxref("border-bottom-color")}}
- {{cssxref("border-left-width")}}
- {{cssxref("border-left-style")}}
- {{cssxref("border-left-color")}}

Nell'esempio sotto, abbiamo utilizzato vari abbreviations e longhands per creare bordi. Provare a giocare con le diverse proprietà per verificare che comprenda come funzionano. Le pagine di MDN per le proprietà di border le forniscono informazioni sui diversi stili di border disponibili.

```html live-sample___border
<div class="container">
  <div class="box">Change my borders.</div>
</div>
```

```css live-sample___border
body {
  font-family: sans-serif;
}
.container {
  margin: 40px;
  padding: 20px;
  border-top: 5px dotted green;
  border-right: 1px solid black;
  border-bottom: 20px double rgb(23 45 145);
}

.box {
  padding: 20px;
  background-color: lightgray;
  border: 1px solid #333333;
  border-top-style: dotted;
  border-right-width: 20px;
  border-bottom-color: hotpink;
}
```

{{EmbedLiveSample("border", "", "220px")}}

### Padding

Il padding si trova tra il bordo e l'area del contenuto e viene utilizzato per spingere il contenuto lontano dal bordo. A differenza dei margini, non può avere un padding negativo. Qualsiasi sfondo applicato al suo elemento verrà visualizzato dietro il padding.

La proprietà {{cssxref("padding")}} controlla il padding su tutti i lati di un elemento. Per controllare ciascun lato individualmente, utilizzare queste proprietà in forma estesa:

- {{cssxref("padding-top")}}
- {{cssxref("padding-right")}}
- {{cssxref("padding-bottom")}}
- {{cssxref("padding-left")}}

Nell'esempio sotto, può cambiare i valori per il padding sulla classe `.box` per vedere che questo cambia dove il testo inizia in relazione alla scatola. Può anche cambiare il padding sulla classe `.container` per creare spazio tra il contenitore e la scatola. Può cambiare il padding su qualsiasi elemento per creare spazio tra il suo bordo e ciò che si trova all'interno dell'elemento.

```html live-sample___padding
<div class="container">
  <div class="box">Change my padding.</div>
</div>
```

```css live-sample___padding
body {
  font-family: sans-serif;
}
.box {
  border: 5px solid rebeccapurple;
  background-color: lightgray;
  padding-top: 0;
  padding-right: 30px;
  padding-bottom: 40px;
  padding-left: 4em;
}

.container {
  border: 5px solid blue;
  margin: 40px;
  padding: 20px;
}
```

{{EmbedLiveSample("padding", "", "220px")}}

## Il modello a scatola e le scatole inline

Tutto quanto sopra si applica completamente alle scatole blocco. Alcune delle proprietà possono applicarsi anche alle scatole inline, come quelle create da un elemento `<span>`.

Nell'esempio sotto, abbiamo un `<span>` all'interno di un paragrafo. Abbiamo applicato una `width`, `height`, `margin`, `border` e `padding` ad esso. Può vedere che la larghezza e l'altezza vengono ignorate. Il margine, padding e bordo superiori e inferiori sono rispettati ma non cambiano la relazione di altri contenuti con la nostra scatola inline. Il padding e il bordo si sovrappongono ad altre parole nel paragrafo. I padding, margini e bordi sinistro e destro spostano altri contenuti lontano dalla scatola.

```html live-sample___inline-box-model
<p>
  I am a paragraph and this is a <span>span</span> inside that paragraph. A span
  is an inline element and so does not respect width and height.
</p>
```

```css live-sample___inline-box-model
body {
  font-family: sans-serif;
}
p {
  border: 2px solid rebeccapurple;
  width: 200px;
}
span {
  margin: 20px;
  padding: 20px;
  width: 80px;
  height: 150px;
  background-color: lightblue;
  border: 2px solid blue;
}
```

{{EmbedLiveSample("inline-box-model")}}

## Uso di display: inline-block

`display: inline-block` è un valore speciale di `display`, che fornisce una via di mezzo tra `inline` e `block`. Usarlo se non desidera che un elemento vada su una nuova linea, ma desidera che rispetti `width` e `height` ed eviti la sovrapposizione vista sopra.

Un elemento con `display: inline-block` fa un sottoinsieme delle cose del blocco che già conosciamo:

- Le proprietà `width` e `height` sono rispettate.
- `padding`, `margin` e `border` faranno sì che altri elementi si allontanino dalla scatola.

Tuttavia, non va su una nuova linea e diventerà più grande del suo contenuto solo se aggiunge esplicitamente proprietà `width` e `height`.

In questo prossimo esempio, abbiamo aggiunto `display: inline-block` al nostro elemento `<span>`. Provare a cambiarlo in `display: block` o rimuovere completamente la linea per vedere la differenza nei modelli di visualizzazione:

```html live-sample___inline-block
<p>
  I am a paragraph and this is a <span>span</span> inside that paragraph. A span
  is an inline element and so does not respect width and height.
</p>
```

```css live-sample___inline-block
body {
  font-family: sans-serif;
}
p {
  border: 2px solid rebeccapurple;
  width: 300px;
}

span {
  margin: 20px;
  padding: 20px;
  width: 80px;
  height: 50px;
  background-color: lightblue;
  border: 2px solid blue;
  display: inline-block;
}
```

{{EmbedLiveSample("inline-block", "", "240px")}}

Dove questo può essere utile è quando desidera dare a un link un'area di clic più grande aggiungendo `padding`. `<a>` è un elemento inline come `<span>`; può usare `display: inline-block` per permettere il settaggio di `padding` su di esso, rendendo più facile per un utente fare clic sul link.

Questo lo si vede abbastanza frequentemente nelle barre di navigazione. La navigazione sotto è visualizzata in una riga usando flexbox e abbiamo aggiunto padding all'elemento `<a>` poiché vogliamo essere in grado di cambiare il `background-color` quando l'`<a>` è in hover. Il padding sembra sovrapporsi al bordo sull'elemento `<ul>`. Questo perché l'`<a>` è un elemento inline.

Aggiunga `display: inline-block;` alla regola con il selettore `.links-list a`, e vedrà come risolve questo problema facendo sì che il padding venga rispettato da altri elementi:

```html live-sample___inline-block-nav
<nav>
  <ul class="links-list">
    <li><a href="">Link one</a></li>
    <li><a href="">Link two</a></li>
    <li><a href="">Link three</a></li>
  </ul>
</nav>
```

```css live-sample___inline-block-nav
ul {
  font-family: sans-serif;
  display: flex;
  list-style: none;
  border: 1px solid #000;
}

li {
  margin: 5px;
}

.links-list a {
  background-color: rgb(179 57 81);
  color: #fff;
  text-decoration: none;
  padding: 1em 2em;
}

.links-list a:hover {
  background-color: rgb(66 28 40);
  color: #fff;
}
```

{{EmbedLiveSample("inline-block-nav")}}

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma ricorda le informazioni più importanti? Può trovare alcuni ulteriori test per verificare che abbia memorizzato queste informazioni prima di procedere — veda [Metti alla prova le tue abilità: Il modello a scatola](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Box_model).

## Riassunto

Questo è la maggior parte di ciò che deve comprendere sul modello a scatola. Potrebbe voler tornare a questa lezione in futuro se mai si trova confuso su quanto grandi sono le scatole nel suo layout.

Nel prossimo articolo, esamineremo come CSS gestisce i conflitti — quando più regole selezionano lo stesso elemento, quali stili vengono applicati?

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Combinators", "Learn_web_development/Core/Styling_basics/Handling_conflicts", "Learn_web_development/Core/Styling_basics")}}
