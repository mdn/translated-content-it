---
title: Il modello a riquadri
short-title: Modello a riquadri
slug: Learn_web_development/Core/Styling_basics/Box_model
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Combinators", "Learn_web_development/Core/Styling_basics/Handling_conflicts", "Learn_web_development/Core/Styling_basics")}}

Tutto in CSS ha un riquadro intorno, e comprendere questi riquadri è fondamentale per essere in grado di creare layout più complessi con CSS o per allineare elementi con altri elementi. In questa lezione, esamineremo il _modello a riquadri_ di CSS. Otterrà una comprensione di come funziona e della terminologia ad esso relativa.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base su HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >La sintassi di base di HTML</a
        >)
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Elementi a blocco e inline</li>
          <li>I diversi riquadri che compongono un elemento e come stilarli: contenuto, margine, bordo, padding.</li>
          <li>Il modello a riquadri alternativo (accessibile tramite <code>box-sizing: border-box</code>) e come differisce dal modello a riquadri regolare.</li>
          <li>Collasso del margine.</li>
          <li>Valori di display di base e come influenzano il comportamento del riquadro: <code>block</code>, <code>inline</code>, <code>inline-block</code>, <code>none</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Riquadri a blocco e inline

In CSS abbiamo diversi tipi di riquadri che generalmente si suddividono nelle categorie di **riquadri a blocco** e **riquadri inline**. Il tipo si riferisce a come il riquadro si comporta in termini di flusso della pagina e in relazione ad altri riquadri sulla pagina. I riquadri hanno un **display interno** e un **display esterno**.

In generale, può impostare vari valori per il tipo di display utilizzando la proprietà {{cssxref("display")}}, che può avere vari valori.

Se un riquadro ha un valore di display `block`, allora:

- Il riquadro si interromperà su una nuova linea.
- Le proprietà {{cssxref("width")}} e {{cssxref("height")}} sono rispettate.
- Padding, margine e bordo causeranno lo spostamento di altri elementi lontano dal riquadro.
- Se {{cssxref("width")}} non è specificato, il riquadro si estenderà nella direzione inline per riempire lo spazio disponibile nel suo contenitore. Nella maggior parte dei casi, il riquadro diventerà largo quanto il suo contenitore, riempiendo il 100% dello spazio disponibile.

Alcuni elementi HTML, come `<h1>` e `<p>`, usano `block` come tipo di display esterno per impostazione predefinita.

Se un riquadro ha un tipo di display `inline`, allora:

- Il riquadro non si interromperà su una nuova linea.
- Le proprietà {{cssxref("width")}} e {{cssxref("height")}} non si applicheranno.
- Il padding, i margini e i bordi superiore e inferiore si applicheranno ma non causeranno lo spostamento di altri riquadri inline lontano dal riquadro.
- Il padding, i margini e i bordi sinistro e destro si applicheranno e causeranno lo spostamento di altri riquadri inline lontano dal riquadro.

Alcuni elementi HTML, come `<a>`, `<span>`, `<em>` e `<strong>` usano `inline` come tipo di display esterno per impostazione predefinita.

Il layout a blocco e inline è il modo predefinito in cui le cose si comportano sul web. Per impostazione predefinita e senza altre istruzioni, gli elementi all'interno di un riquadro sono disposti anche nel **[flusso normale](/it/docs/Learn_web_development/Core/CSS_layout/Introduction#normal_layout_flow)** e si comportano come riquadri a blocco o inline.

## Tipi di display interno e esterno

I valori di display `block` e `inline` sono detti tipi di **display esterno** — influenzano il modo in cui il riquadro viene disposto in relazione ad altri riquadri intorno ad esso. I riquadri hanno anche un tipo di **display interno**, che detta come gli elementi all'interno di quel riquadro sono disposti.

Può cambiare il tipo di display interno impostando un valore di display interno, ad esempio `display: flex;`. L'elemento utilizzerà ancora il tipo di display esterno `block` ma questo cambia il tipo di display interno a `flex`. Tutti i figli diretti di questo riquadro diventeranno elementi flex e si comporteranno secondo la specifica [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox).

Quando inizierà a imparare di più sui layout CSS, incontrerà [`flex`](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox), e vari altri valori interni che i suoi riquadri possono avere, ad esempio [`grid`](/it/docs/Learn_web_development/Core/CSS_layout/Grids).

Non si preoccupi troppo della terminologia interna ed esterna per ora; questo è ciò che sta accadendo internamente, e lo abbiamo menzionato qui nel caso in cui lo incontri altrove. Generalmente, tratterà solo con singoli valori di `display`, e non dovrà pensarci molto.

## Esempi di diversi tipi di display

L'esempio seguente ha tre diversi elementi HTML, tutti con un tipo di display esterno `block`.

- Un paragrafo con un bordo aggiunto in CSS. Il browser lo rende come un riquadro a blocco. Il paragrafo inizia su una nuova linea ed estende tutta la larghezza disponibile.

- Un elenco, che è disposto utilizzando `display: flex`. Questo stabilisce il layout flex per i figli del contenitore, che sono elementi flex. L'elenco stesso è un riquadro a blocco e — come il paragrafo — si espande fino a occupare tutta la larghezza del contenitore e si interrompe su una nuova linea.

- Un paragrafo a livello di blocco, all'interno del quale ci sono due elementi `<span>`. Questi elementi sarebbero normalmente `inline`, tuttavia, uno degli elementi ha una classe "block" che viene impostata su `display: block`.

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

Nell'esempio successivo, possiamo vedere come si comportano gli elementi `inline`.

- Gli elementi `<span>` nel primo paragrafo sono inline per impostazione predefinita e quindi non forzano interruzioni di riga.

- L'elemento `<ul>` che è impostato su `display: inline-flex` crea un riquadro inline contenente alcuni elementi flex.

- I due paragrafi sono entrambi impostati su `display: inline`. Il contenitore flex inline e i paragrafi scorrono tutti insieme su una linea anziché interrompersi su nuove linee (come farebbero se fossero visualizzati come elementi a livello di blocco).

Per passare da una modalità di display all'altra, può cambiare `display: inline` in `display: block` o `display: inline-flex` in `display: flex`:

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

La cosa chiave da ricordare per ora è: Cambiare il valore della proprietà `display` può cambiare se il tipo di display esterno di un riquadro è a blocco o inline. Questo cambia il modo in cui si visualizza insieme ad altri elementi nel layout.

## Cos'è il modello a riquadri di CSS?

Il modello a riquadri di CSS nel complesso si applica ai riquadri a blocco e definisce come le diverse parti di un riquadro — margine, bordo, padding e contenuto — lavorano insieme per creare un riquadro visibile su una pagina. I riquadri inline utilizzano solo _alcuni_ dei comportamenti definiti nel modello a riquadri.

Per aggiungere complessità, c'è un modello a riquadri standard e uno alternativo. Per impostazione predefinita, i browser usano il modello a riquadri standard.

### Parti di un riquadro

A comporre un riquadro a blocco in CSS abbiamo:

- **Riquadro del contenuto**: L'area in cui viene visualizzato il contenuto; dimensionarla usando proprietà come {{cssxref("width")}} e {{cssxref("height")}}.
- **Riquadro del padding**: Il padding si trova intorno al contenuto come spazio bianco; dimensionarlo usando {{cssxref("padding")}} e proprietà correlate.
- **Riquadro del bordo**: Il riquadro del bordo avvolge il contenuto e qualsiasi padding; dimensionarlo usando {{cssxref("border")}} e proprietà correlate.
- **Riquadro del margine**: Il margine è lo strato più esterno, avvolgendo il contenuto, il padding e il bordo come spazio bianco tra questo riquadro e altri elementi; dimensionarlo usando {{cssxref("margin")}} e proprietà correlate.

Lo schema seguente mostra questi strati:

![Diagramma del modello a riquadri](box-model.png)

### Modello a riquadri di CSS standard

Nel modello a riquadri standard, se imposta valori per le proprietà `width` e `height` su un riquadro, questi valori definiscono la `width` e `height` del _riquadro del contenuto_. Qualsiasi padding e bordo vengono quindi aggiunti a quelle dimensioni per ottenere la dimensione totale occupata dal riquadro (vedi immagine sotto).

Se supponiamo che un riquadro abbia il seguente CSS:

```css
.box {
  width: 350px;
  height: 150px;
  margin: 10px;
  padding: 25px;
  border: 5px solid black;
}
```

Lo spazio _effettivo_ occupato dal riquadro sarà di 410px di larghezza (350 + 25 + 25 + 5 + 5) e 210px di altezza (150 + 25 + 25 + 5 + 5).

![Mostrando la dimensione del riquadro quando si usa il modello a riquadri standard.](standard-box-model.png)

> [!NOTE]
> Il margine non viene conteggiato nella dimensione effettiva del riquadro — certo, influisce sullo spazio totale che il riquadro occuperà sulla pagina, ma solo lo spazio esterno al riquadro. L'area del riquadro si ferma al bordo — non si estende nel margine.

### Modello a riquadri di CSS alternativo

Nel modello a riquadri alternativo, qualsiasi larghezza è la larghezza del riquadro visibile sulla pagina. La larghezza dell'area del contenuto è quella larghezza meno la larghezza per il padding e il bordo (vedi immagine sotto). Non c'è bisogno di sommare il bordo e il padding per ottenere la dimensione reale del riquadro.

Per attivare il modello alternativo per un elemento, impostare `box-sizing: border-box` su di esso:

```css
.box {
  box-sizing: border-box;
}
```

Se supponiamo che il riquadro abbia lo stesso CSS di sopra:

```css
.box {
  width: 350px;
  height: 150px;
  margin: 10px;
  padding: 25px;
  border: 5px solid black;
}
```

Ora, lo spazio _effettivo_ occupato dal riquadro sarà di 350px nella direzione inline e 150px nella direzione block.

![Mostrando la dimensione del riquadro quando si utilizza il modello a riquadri alternativo.](alternate-box-model.png)

Per usare il modello a riquadri alternativo per tutti i suoi elementi (che è una scelta comune tra gli sviluppatori), impostare la proprietà `box-sizing` sull'elemento `<html>` e impostare tutti gli altri elementi per ereditare quel valore:

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

Per comprendere l'idea di fondo, può leggere l'articolo di CSS Tricks sul [box-sizing](https://css-tricks.com/inheriting-box-sizing-probably-slightly-better-best-practice/).

## Giocare con i modelli a riquadri

Nell'esempio sotto, può vedere due riquadri. Entrambi hanno una classe `.box`, che gli conferisce la stessa `width`, `height`, `margin`, `border` e `padding`. L'unica differenza è che il secondo riquadro è stato impostato per usare il modello a riquadri alternativo. Può cambiare la dimensione del secondo riquadro (aggiungendo CSS alla classe `.alternate`) per farlo coincidere con il primo riquadro in larghezza e altezza?

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
> Può trovare una soluzione a questo compito [qui](https://github.com/mdn/css-examples/blob/main/learn/solutions.md#the-box-model).

### Usare gli strumenti DevTools del browser per visualizzare il modello a riquadri

I [DevTools del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) possono rendere più facile comprendere il modello a riquadri — possono mostrare la dimensione dell'elemento più il suo margine, padding e bordo. Ispezionare un elemento in questo modo è un ottimo modo per scoprire se il suo riquadro è davvero della dimensione che pensa!

![Ispezionare il modello a riquadri di un elemento usando i DevTools di Firefox](box-model-devtools.png)

## Margini, padding e bordi

Ha già visto le proprietà {{cssxref("margin")}}, {{cssxref("padding")}}, e {{cssxref("border")}} in azione nell'esempio sopra. Le proprietà usate in quell'esempio sono abbreviazioni e ci permettono di impostare tutti e quattro i lati del riquadro in una sola volta. Queste abbreviazioni hanno anche proprietà equivalenti in forma estesa, che permettono di controllare i diversi lati del riquadro individualmente.

Esploriamo queste proprietà in dettaglio.

### Margine

Il margine è uno spazio invisibile attorno al suo riquadro. Allontana altri elementi dal riquadro. I margini possono avere valori positivi o negativi. Impostare un margine negativo su un lato del suo riquadro può causare il sovrapporsi ad altre cose sulla pagina. Che stia usando il modello a riquadri standard o alternativo, il margine è sempre aggiunto dopo che la dimensione del riquadro visibile è stata calcolata.

Possiamo controllare tutti i margini di un elemento contemporaneamente usando la proprietà {{cssxref("margin")}}, o ciascun lato individualmente usando le proprietà in forma estesa equivalenti:

- {{cssxref("margin-top")}}
- {{cssxref("margin-right")}}
- {{cssxref("margin-bottom")}}
- {{cssxref("margin-left")}}

Nell'esempio sotto, provi a cambiare i valori del margine per vedere come il riquadro viene spostato a causa del margine che crea o rimuove spazio (se è un margine negativo) tra questo elemento e l'elemento contenitore.

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

- Due margini positivi si combineranno per diventare un solo margine. La sua dimensione sarà uguale al maggiore dei margini individuali.
- Due margini negativi collasseranno e il valore più piccolo (più distante da zero) sarà usato.
- Se un margine è negativo, il suo valore sarà _sottratto_ dal totale.

Nell'esempio sottostante, abbiamo due paragrafi. Il paragrafo superiore ha un `margin-bottom` di 50 pixel, l'altro ha un `margin-top` di 30 pixel. I margini si sono collassati insieme, quindi il margine effettivo tra i riquadri è di 50 pixel e non la somma dei due margini.

Può testarlo impostando il `margin-top` del secondo paragrafo a `0`. Il margine visibile tra i due paragrafi non cambierà — rimane il 50 pixel impostato nel `margin-bottom` del primo paragrafo. Se lo imposta a `-10px`, vedrà che il margine complessivo diventa `40px` — viene sottratto dai `50px`.

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

Una serie di regole determina quando i margini collassano e quando no. Per ulteriori informazioni, vedere la pagina dettagliata su [padroneggiare il collasso dei margini](/it/docs/Web/CSS/CSS_box_model/Mastering_margin_collapsing). La cosa principale da ricordare è che il collasso dei margini è una cosa che accade se si sta creando spazio con i margini e non si ottiene lo spazio che si desidera.

### Bordi

Il bordo è disegnato tra il margine e il padding di un riquadro. Se sta utilizzando il modello a riquadri standard, la dimensione del bordo viene aggiunta alla `width` e `height` del riquadro del contenuto. Se sta usando il modello a riquadri alternativo, più grande è il bordo, più piccolo è il riquadro del contenuto, poiché il bordo occupa parte di quella larghezza e altezza disponibili del riquadro dell'elemento.

Per lo stile dei bordi, ci sono un gran numero di proprietà — ci sono quattro bordi, e ciascun bordo ha uno stile, una larghezza e un colore che potremmo voler manipolare.

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

Per impostare la larghezza, lo stile o il colore di un singolo lato, usare una delle proprietà in forma estesa più granulari:

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

Nell'esempio sotto, abbiamo usato varie abbreviazioni e forme estese per creare bordi. Giocate con le diverse proprietà per verificare che comprendiate come funzionano. Le pagine di MDN per le proprietà dei bordi forniscono informazioni sui diversi stili di bordo disponibili.

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

Il padding si trova tra il bordo e l'area del contenuto ed è usato per spingere il contenuto lontano dal bordo. A differenza dei margini, non può avere un padding negativo. Qualsiasi sfondo applicato al suo elemento verrà visualizzato dietro il padding.

La proprietà {{cssxref("padding")}} controlla il padding su tutti i lati di un elemento. Per controllare ciascun lato individualmente, utilizzare queste proprietà in forma estesa:

- {{cssxref("padding-top")}}
- {{cssxref("padding-right")}}
- {{cssxref("padding-bottom")}}
- {{cssxref("padding-left")}}

Nell'esempio sotto, può cambiare i valori del padding sulla classe `.box` per vedere che questo cambia dove il testo inizia in relazione al riquadro. Può anche cambiare il padding sulla classe `.container` per creare spazio tra il contenitore e il riquadro. Può cambiare il padding su qualsiasi elemento per creare spazio tra il suo bordo e qualunque cosa ci sia all'interno dell'elemento.

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

## Il modello a riquadri e i riquadri inline

Tutto quanto sopra si applica pienamente ai riquadri a blocco. Alcune delle proprietà possono applicarsi anche ai riquadri inline, come quelli creati da un elemento `<span>`.

Nell'esempio sotto, abbiamo un `<span>` all'interno di un paragrafo. Abbiamo applicato una `width`, `height`, `margin`, `border`, e `padding` ad esso. Può vedere che la larghezza e l'altezza vengono ignorate. Il margine, il padding e il bordo superiore e inferiore sono rispettati ma non cambiano la relazione di altri contenuti con il nostro riquadro inline. Il padding e il bordo coprono altre parole nel paragrafo. Il padding, i margini e i bordi sinistro e destro spostano altri contenuti lontano dal riquadro.

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

`display: inline-block` è un valore speciale di `display`, che fornisce una via di mezzo tra `inline` e `block`. Usalo se non vuole che un elemento si interrompa su una nuova linea, ma vuole che rispetti `width` e `height` ed eviti la sovrapposizione vista sopra.

Un elemento con `display: inline-block` fa un sottoinsieme delle cose a blocco che già conosciamo:

- Le proprietà `width` e `height` sono rispettate.
- `padding`, `margin`, e `border` causeranno lo spostamento di altri elementi lontano dal riquadro.

Tuttavia, non si interrompe su una nuova linea e diventerà più grande del suo contenuto solo se esplicitamente aggiunge `width` e `height`.

In questo prossimo esempio, abbiamo aggiunto `display: inline-block` al nostro elemento `<span>`. Provate a cambiare questo in `display: block` o rimuovere completamente la linea per vedere la differenza nei modelli di visualizzazione:

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

Dove questo può essere utile è quando si vuole dare a un collegamento un'area di hit più grande aggiungendo del `padding`. `<a>` è un elemento inline come `<span>`; può usare `display: inline-block` per permettere di impostare il padding su di esso, facilitando il clic sul collegamento da parte dell'utente.

Si vede questo abbastanza frequentemente nelle barre di navigazione. La navigazione sottostante è mostrata in una riga usando flexbox e abbiamo aggiunto del padding all'elemento `<a>` poiché vogliamo essere in grado di cambiare il `background-color` quando l'elemento `<a>` è in hover. Il padding sembra sovrapporsi al bordo sull'elemento `<ul>`. Questo accade perché `<a>` è un elemento inline.

Aggiungere `display: inline-block;` alla regola con il selettore `.links-list a`, e vedrà come risolve questo problema facendo sì che il padding sia rispettato da altri elementi:

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

## Metti alla prova le sue abilità!

Ha raggiunto la fine di questo articolo, ma può ricordare le informazioni più importanti? Può trovare alcuni ulteriori test per verificare che abbia mantenuto queste informazioni prima di procedere — veda [Metti alla prova le sue abilità: Il modello a riquadri](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Box_model).

## Sommario

Questo è tutto ciò che deve comprendere sul modello a riquadri. Potrebbe voler tornare su questa lezione in futuro se si trova mai confuso su quanto sono grandi i riquadri nel suo layout.

Nell'articolo successivo, esamineremo come CSS gestisce i conflitti — quando più regole selezionano lo stesso elemento, quali stili vengono applicati?

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Combinators", "Learn_web_development/Core/Styling_basics/Handling_conflicts", "Learn_web_development/Core/Styling_basics")}}
