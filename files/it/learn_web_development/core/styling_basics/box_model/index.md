---
title: Il modello di box
short-title: Modello di box
slug: Learn_web_development/Core/Styling_basics/Box_model
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Combinators", "Learn_web_development/Core/Styling_basics/Handling_conflicts", "Learn_web_development/Core/Styling_basics")}}

Tutto in CSS ha un box attorno e comprendere questi box è la chiave per essere in grado di creare layout più complessi con CSS o per allineare oggetti con altri oggetti. In questa lezione, daremo uno sguardo al _Box model_ di CSS. Otterrai una comprensione di come funziona e della terminologia ad esso relativa.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base su HTML (studia
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >)
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Elementi block e inline</li>
          <li>I diversi box che compongono un elemento e come stilizzarli — contenuto, margine, bordo, padding.</li>
          <li>Il modello di box alternativo (accessibile tramite <code>box-sizing: border-box</code>) e come differisce dal modello di box regolare.</li>
          <li>Collasso dei margini.</li>
          <li>Valori di visualizzazione di base e come influenzano il comportamento del box — <code>block</code>, <code>inline</code>, <code>inline-block</code>, <code>none</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Box block e inline

In CSS abbiamo diversi tipi di box che generalmente rientrano nelle categorie di **box block** e **box inline**. Il tipo si riferisce a come il box si comporta in termini di flusso di pagina e in relazione ad altri box nella pagina. I box hanno un **tipo di visualizzazione interno** e un **tipo di visualizzazione esterno**.

In generale, è possibile impostare vari valori per il tipo di visualizzazione utilizzando la proprietà {{cssxref("display")}}, che può avere diversi valori.

Se un box ha un valore di visualizzazione `block`, allora:

- Il box inizierà su una nuova riga.
- Le proprietà {{cssxref("width")}} e {{cssxref("height")}} sono rispettate.
- Padding, margine e bordo causeranno lo spostamento di altri elementi lontano dal box.
- Se {{cssxref("width")}} non è specificato, il box si estenderà nella direzione inline per riempire lo spazio disponibile nel suo contenitore. Nella maggior parte dei casi, il box diventerà ampio quanto il suo contenitore, riempiendo il 100% dello spazio disponibile.

Alcuni elementi HTML, come `<h1>` e `<p>`, utilizzano `block` come loro tipo di visualizzazione esterno per impostazione predefinita.

Se un box ha un tipo di visualizzazione `inline`, allora:

- Il box non inizierà su una nuova riga.
- Le proprietà {{cssxref("width")}} e {{cssxref("height")}} non si applicano.
- Padding, margini e bordi superiori e inferiori si applicheranno ma non causeranno lo spostamento di altri box inline lontano dal box.
- Padding, margini e bordi a sinistra e a destra si applicheranno e causeranno lo spostamento di altri box inline lontano dal box.

Alcuni elementi HTML, come `<a>`, `<span>`, `<em>` e `<strong>` utilizzano `inline` come loro tipo di visualizzazione esterno per impostazione predefinita.

Il layout block e inline è il modo predefinito in cui le cose si comportano sul web. Per impostazione predefinita e senza alcuna altra istruzione, gli elementi all'interno di un box sono anche disposti in **[flusso normale](/it/docs/Learn_web_development/Core/CSS_layout/Introduction#normal_layout_flow)** e si comportano come box block o inline.

## Tipi di visualizzazione interni ed esterni

I valori di visualizzazione `block` e `inline` sono detti tipi di **visualizzazione esterna** — influenzano il modo in cui il box è disposto in relazione agli altri box intorno ad esso. I box hanno anche un tipo di **visualizzazione interna**, che determina come gli elementi all'interno di quel box sono disposti.

Puoi cambiare il tipo di visualizzazione interna impostando un valore di visualizzazione interno, ad esempio `display: flex;`. L'elemento utilizzerà ancora il tipo di visualizzazione esterno `block` ma questo cambia il tipo di visualizzazione interno in `flex`. Qualsiasi figlio diretto di questo box diventerà un elemento flex e si comporterà secondo la specifica [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox).

Quando passerai a studiare CSS Layout in modo più dettagliato, incontrerai [`flex`](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox) e vari altri valori interni che i tuoi box possono avere, ad esempio [`grid`](/it/docs/Learn_web_development/Core/CSS_layout/Grids).

Non preoccuparti troppo della terminologia interna ed esterna per ora; questo è ciò che sta accadendo internamente, e lo abbiamo menzionato qui nel caso lo incontrassi altrove. Generalmente, ti occuperai solo di valori singoli di `display` e non dovrai pensarci molto.

## Esempi di diversi tipi di visualizzazione

L'esempio seguente ha tre diversi elementi HTML, tutti con un tipo di visualizzazione esterno di `block`.

- Un paragrafo con un bordo aggiunto in CSS. Il browser rende questo come un box block. Il paragrafo inizia su una nuova riga ed estende l'intera larghezza disponibile.

- Una lista, che è disposta usando `display: flex`. Questo stabilisce un layout flex per i figli del contenitore, che sono elementi flex. La lista stessa è un box block e — come il paragrafo — si espande per tutta la larghezza del contenitore e si interrompe su una nuova riga.

- Un paragrafo a livello block, all'interno del quale ci sono due elementi `<span>`. Questi elementi sarebbero normalmente `inline`, tuttavia, uno degli elementi ha una classe di "block" che viene impostata su `display: block`.

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

- Gli elementi `<span>` nel primo paragrafo sono inline per impostazione predefinita e quindi non forzano interruzioni di linea.

- L'elemento `<ul>` impostato su `display: inline-flex` crea un box inline contenente alcuni elementi flex.

- I due paragrafi sono entrambi impostati su `display: inline`. Il contenitore inline flex e i paragrafi si trovano tutti insieme su una linea piuttosto che interrompersi su nuove righe (come farebbero se fossero visualizzati come elementi a livello di blocco).

Per passare tra le modalità di visualizzazione, puoi cambiare `display: inline` in `display: block` o `display: inline-flex` in `display: flex`:

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

La cosa principale da ricordare per ora è: Cambiare il valore della proprietà `display` può cambiare se il tipo di visualizzazione esterno di un box è block o inline. Questo cambia il modo in cui viene visualizzato insieme ad altri elementi nel layout.

## Cos'è il modello di box CSS?

Il modello di box CSS nel suo insieme si applica ai box block e definisce come le diverse parti di un box — margine, bordo, padding e contenuto — lavorano insieme per creare un box che puoi vedere su una pagina. I box inline utilizzano solo _alcuni_ dei comportamenti definiti nel modello di box.

Per aggiungere complessità, esiste un modello di box standard e uno alternativo. Per impostazione predefinita, i browser utilizzano il modello di box standard.

### Parti di un box

Componendo un box block in CSS abbiamo:

- **Box del contenuto**: L'area in cui viene visualizzato il tuo contenuto; definisci le sue dimensioni utilizzando proprietà come {{cssxref("width")}} e {{cssxref("height")}}.
- **Box del padding**: Il padding si trova attorno al contenuto come spazio bianco; definiscilo utilizzando {{cssxref("padding")}} e proprietà correlate.
- **Box del bordo**: Il box del bordo avvolge il contenuto e qualsiasi padding; definiscilo utilizzando {{cssxref("border")}} e proprietà correlate.
- **Box del margine**: Il margine è lo strato più esterno, che avvolge il contenuto, il padding e il bordo come spazio bianco tra questo box e altri elementi; definiscilo utilizzando {{cssxref("margin")}} e proprietà correlate.

Il diagramma qui sotto mostra questi strati:

![Diagramma del modello di box](box-model.png)

### Il modello di box CSS standard

Nel modello di box standard, se imposti i valori delle proprietà `width` e `height` su un box, questi valori definiscono la `width` e la `height` del _box del contenuto_. Qualsiasi padding e bordi vengono quindi aggiunti a tali dimensioni per ottenere la dimensione totale occupata dal box (vedi l'immagine sotto).

Se assumiamo che un box abbia il seguente CSS:

```css
.box {
  width: 350px;
  height: 150px;
  margin: 10px;
  padding: 25px;
  border: 5px solid black;
}
```

Lo spazio _effettivo_ occupato dal box sarà largo 410px (350 + 25 + 25 + 5 + 5) e alto 210px (150 + 25 + 25 + 5 + 5).

![Mostrare la dimensione del box quando si utilizza il modello di box standard.](standard-box-model.png)

> [!NOTE]
> Il margine non viene conteggiato nella dimensione effettiva del box — certo, influisce sullo spazio totale che il box occupa sulla pagina, ma solo lo spazio al di fuori del box. L'area del box si ferma al bordo — non si estende nel margine.

### Il modello di box CSS alternativo

Nel modello di box alternativo, qualsiasi larghezza è la larghezza del box visibile sulla pagina. La larghezza dell'area del contenuto è quella larghezza meno la larghezza per il padding e il bordo (vedi immagine sotto). Non è necessario sommare il bordo e il padding per ottenere la dimensione reale del box.

Per attivare il modello alternativo per un elemento, imposta `box-sizing: border-box` su di esso:

```css
.box {
  box-sizing: border-box;
}
```

Se assumiamo che il box abbia lo stesso CSS di cui sopra:

```css
.box {
  width: 350px;
  height: 150px;
  margin: 10px;
  padding: 25px;
  border: 5px solid black;
}
```

Ora, lo spazio _effettivo_ occupato dal box sarà 350px nella direzione inline e 150px nella direzione block.

![Mostrare la dimensione del box quando si utilizza il modello di box alternativo.](alternate-box-model.png)

Per utilizzare il modello di box alternativo per tutti i tuoi elementi (che è una scelta comune tra gli sviluppatori), imposta la proprietà `box-sizing` sull'elemento `<html>` e imposta tutti gli altri elementi per ereditare quel valore:

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

Per comprendere l'idea sottostante, puoi leggere [l'articolo su CSS Tricks riguardante box-sizing](https://css-tricks.com/inheriting-box-sizing-probably-slightly-better-best-practice/).

## Giocare con i modelli di box

Nell'esempio sotto, puoi vedere due box. Entrambi hanno una classe `.box`, che dà loro gli stessi `width`, `height`, `margin`, `border` e `padding`. L'unica differenza è che il secondo box è stato impostato per utilizzare il modello di box alternativo.
Puoi cambiare la dimensione del secondo box (aggiungendo CSS alla classe `.alternate`) per farlo coincidere con il primo box in larghezza e altezza?

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
> Puoi trovare una soluzione per questo compito [nel nostro repo css-examples](https://github.com/mdn/css-examples/blob/main/learn/solutions.md#the-box-model).

### Utilizza DevTools del browser per visualizzare il modello di box

I tuoi [strumenti per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) possono rendere la comprensione del modello di box molto più facile — possono mostrarti la dimensione dell'elemento più il suo margine, padding e bordo. Ispezionare un elemento in questo modo è un ottimo modo per scoprire se il tuo box ha davvero la dimensione che pensi abbia!

![Ispezionare il modello di box di un elemento utilizzando Firefox DevTools](box-model-devtools.png)

## Margini, padding e bordi

Hai già visto le proprietà {{cssxref("margin")}}, {{cssxref("padding")}} e {{cssxref("border")}} in azione nell'esempio sopra. Le proprietà utilizzate in quell'esempio sono abbreviazioni e ci permettono di impostare contemporaneamente tutti e quattro i lati del box. Queste abbreviazioni hanno anche proprietà equivalenti estese, che consentono il controllo dei diversi lati del box individualmente.

Esploriamo queste proprietà in dettaglio.

### Margine

Il margine è uno spazio invisibile attorno al tuo box. Spinge lontano gli altri elementi dal box. I margini possono avere valori positivi o negativi. Impostare un margine negativo su un lato del tuo box può provocare una sovrapposizione con altri elementi sulla pagina. Che tu stia usando il modello di box standard o alternativo, il margine viene sempre aggiunto dopo che la dimensione del box visibile è stata calcolata.

Possiamo controllare tutti i margini di un elemento contemporaneamente usando la proprietà {{cssxref("margin")}}, o ciascun lato individualmente usando le proprietà estese equivalenti:

- {{cssxref("margin-top")}}
- {{cssxref("margin-right")}}
- {{cssxref("margin-bottom")}}
- {{cssxref("margin-left")}}

Nell'esempio sotto, prova a cambiare i valori del margine per vedere come il box viene spinto in giro a causa del margine che crea o rimuove spazio (se è un margine negativo) tra questo elemento e l'elemento contenitore.

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

#### Collasso dei margini

A seconda del fatto che due elementi i cui margini si toccano abbiano margini positivi o negativi, i risultati saranno diversi:

- Due margini positivi si combineranno per diventare un unico margine. La sua dimensione sarà pari al margine individuale più grande.
- Due margini negativi collasseranno e verrà utilizzato il valore più piccolo (più lontano da zero).
- Se un margine è negativo, il suo valore verrà _sottratto_ dal totale.

Nell'esempio sotto, abbiamo due paragrafi. Il paragrafo superiore ha un `margin-bottom` di 50 pixel, l'altro ha un `margin-top` di 30 pixel. I margini sono collassati insieme in modo che il margine effettivo tra i box sia di 50 pixel e non il totale dei due margini.

Puoi testarlo impostando il `margin-top` del paragrafo due a `0`. Il margine visibile tra i due paragrafi non cambierà — mantiene i 50 pixel impostati nel `margin-bottom` del paragrafo uno. Se lo imposti a `-10px`, vedrai che il margine complessivo diventa `40px` — viene sottratto dai `50px`.

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

Una serie di regole sanciscono quando i margini collassano e quando no. Per ulteriori informazioni vedi la pagina dettagliata su [padroneggiare il collasso dei margini](/it/docs/Web/CSS/CSS_box_model/Mastering_margin_collapsing). La cosa principale da ricordare è che il collasso dei margini è qualcosa che accade se stai creando spazio con i margini e non ottieni lo spazio che ti aspetti.

### Bordi

Il bordo è disegnato tra il margine e il padding di un box. Se stai utilizzando il modello di box standard, la dimensione del bordo viene aggiunta alla `width` e `height` del box del contenuto. Se stai utilizzando il modello di box alternativo, allora più grande è il bordo, più piccolo sarà il box del contenuto, poiché il bordo occupa parte di quella `width` e `height` disponibili dell'elemento box.

Per stilizzare i bordi, ci sono un vasto numero di proprietà — ci sono quattro bordi, e ciascun bordo ha uno stile, una larghezza e un colore che potremmo voler manipolare.

Puoi impostare la larghezza, lo stile o il colore di tutti e quattro i bordi contemporaneamente usando la proprietà {{cssxref("border")}}.

Per impostare le proprietà di ogni lato individualmente, usa:

- {{cssxref("border-top")}}
- {{cssxref("border-right")}}
- {{cssxref("border-bottom")}}
- {{cssxref("border-left")}}

Per impostare la larghezza, lo stile o il colore di tutti i lati, usa:

- {{cssxref("border-width")}}
- {{cssxref("border-style")}}
- {{cssxref("border-color")}}

Per impostare la larghezza, lo stile o il colore di un singolo lato, usa una delle proprietà estese più granulari:

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

Nell'esempio sotto, abbiamo utilizzato varie abbreviazioni e estensioni per creare bordi. Gioca con le diverse proprietà per verificare che tu abbia capito come funzionano. Le pagine MDN per le proprietà dei bordi forniscono informazioni sui diversi stili di bordo disponibili.

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

Il padding si trova tra il bordo e l'area del contenuto ed è utilizzato per spingere il contenuto lontano dal bordo. A differenza dei margini, non puoi avere un padding negativo. Qualsiasi sfondo applicato al tuo elemento verrà visualizzato dietro il padding.

La proprietà {{cssxref("padding")}} controlla il padding su tutti i lati di un elemento. Per controllare ciascun lato individualmente, utilizza queste proprietà estese:

- {{cssxref("padding-top")}}
- {{cssxref("padding-right")}}
- {{cssxref("padding-bottom")}}
- {{cssxref("padding-left")}}

Nell'esempio sotto, puoi cambiare i valori per il padding sulla classe `.box` per vedere che questo cambia dove inizia il testo in relazione al box. Puoi anche cambiare il padding sulla classe `.container` per creare spazio tra il contenitore e il box. Puoi cambiare il padding su qualsiasi elemento per creare spazio tra il suo bordo e qualsiasi cosa vi sia dentro l'elemento.

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

## Il modello di box e i box inline

Tutto quanto sopra si applica completamente ai box block. Alcune delle proprietà possono applicarsi anche ai box inline, come quelli creati da un elemento `<span>`.

Nell'esempio sotto, abbiamo uno `<span>` all'interno di un paragrafo. Abbiamo applicato una `width`, `height`, `margin`, `border` e `padding` a esso. Puoi vedere che la larghezza e l'altezza vengono ignorate. Il margine, padding e bordo superiori e inferiori vengono rispettati ma non cambiano la relazione di altri contenuti con il nostro box inline. Il padding e il bordo si sovrappongono ad altre parole nel paragrafo. Il padding, margini e bordi a sinistra e a destra spostano altri contenuti lontano dal box.

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

## Usare display: inline-block

`display: inline-block` è un valore speciale di `display`, che fornisce un compromesso tra `inline` e `block`. Usalo se non vuoi che un elemento inizi su una nuova riga, ma vuoi che rispetti `width` e `height` ed eviti la sovrapposizione vista sopra.

Un elemento con `display: inline-block` fa un sottoinsieme delle cose block che già conosciamo:

- Le proprietà `width` e `height` sono rispettate.
- `padding`, `margin` e `border` causeranno lo spostamento di altri elementi lontano dal box.

Tuttavia, non inizia una nuova riga e diventerà più grande del suo contenuto solo se aggiungi esplicitamente le proprietà `width` e `height`.

In questo prossimo esempio, abbiamo aggiunto `display: inline-block` al nostro elemento `<span>`. Prova a cambiare questo in `display: block` o a rimuovere completamente la riga per vedere la differenza nei modelli di visualizzazione:

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

Dove questo può essere utile è quando vuoi dare a un link un'area di clic più grande aggiungendo `padding`. `<a>` è un elemento inline come `<span>`; puoi usare `display: inline-block` per permettere che il padding sia impostato su di esso, rendendo più facile per un utente cliccare sul link.

Vedrai questo abbastanza frequentemente nelle barre di navigazione. La navigazione qui sotto è visualizzata in una riga usando il flexbox e abbiamo aggiunto padding all'elemento `<a>` perché vogliamo essere in grado di cambiare il `background-color` quando si passa con il mouse sopra `<a>`. Il padding sembra sovrapporsi al bordo sull'elemento `<ul>`. Questo perché `<a>` è un elemento inline.

Aggiungi `display: inline-block;` alla regola con il selettore `.links-list a`, e vedrai come risolve questo problema facendo sì che il padding sia rispettato da altri elementi:

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

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare che tu abbia conservato queste informazioni prima di andare avanti — vedi [Metti alla prova le tue abilità: Il modello di box](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Box_model).

## Riepilogo

Questo è la maggior parte di quello che devi capire riguardo al modello di box. Potresti voler tornare su questa lezione in futuro se ti trovi mai confuso su quanto grandi siano i box nel tuo layout.

Nel prossimo articolo, daremo uno sguardo a come CSS gestisce i conflitti — quando più regole selezionano lo stesso elemento, quali stili vengono applicati?

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Combinators", "Learn_web_development/Core/Styling_basics/Handling_conflicts", "Learn_web_development/Core/Styling_basics")}}
