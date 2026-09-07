---
title: Il modello a scatola
short-title: Modello a scatola
slug: Learn_web_development/Core/Styling_basics/Box_model
l10n:
  sourceCommit: 85fccefc8066bd49af4ddafc12c77f35265c7e2d
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Test_your_skills/Selectors", "Learn_web_development/Core/Styling_basics/Test_your_skills/Box_model", "Learn_web_development/Core/Styling_basics")}}

Ogni elemento in CSS ha una scatola attorno a sé e comprendere queste scatole è fondamentale per poter creare layout più complessi con CSS o allineare elementi con altri elementi. In questa lezione verrà esaminato il _modello a scatola_ CSS. Sarà possibile comprendere come funziona e la terminologia a esso correlata.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base di HTML (studiare la
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >sintassi HTML di base</a
        >)
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Elementi block e inline</li>
          <li>Le diverse scatole che costituiscono un elemento e come applicare loro stili — contenuto, margine, bordo, padding.</li>
          <li>Il modello a scatola alternativo (accessibile tramite <code>box-sizing: border-box</code>) e in cosa differisce dal modello a scatola normale.</li>
          <li>Collasso dei margini.</li>
          <li>Valori di display di base e come influenzano il comportamento della scatola — <code>block</code>, <code>inline</code>, <code>inline-block</code>, <code>none</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Scatole block e inline

In CSS esistono diversi tipi di scatole che rientrano generalmente nelle categorie delle **scatole block** e delle **scatole inline**. Il tipo si riferisce al comportamento della scatola in termini di flusso della pagina e in relazione alle altre scatole nella pagina. Le scatole hanno un **tipo di display interno** e un **tipo di display esterno**.

In generale, è possibile impostare vari valori per il tipo di display utilizzando la proprietà {{cssxref("display")}}.

Se una scatola ha un valore di display pari a `block`, allora:

- La scatola va a capo su una nuova riga.
- Le proprietà {{cssxref("width")}} e {{cssxref("height")}} vengono rispettate.
- Padding, margine e bordo fanno sì che gli altri elementi vengano allontanati dalla scatola.
- Se {{cssxref("width")}} non è specificata, la scatola si estende nella direzione inline per riempire lo spazio disponibile nel suo contenitore. Nella maggior parte dei casi, la scatola diventa larga quanto il suo contenitore, occupando il 100% dello spazio disponibile.

Alcuni elementi HTML, come `<h1>` e `<p>`, utilizzano `block` come tipo di display esterno predefinito.

Se una scatola ha un tipo di display pari a `inline`, allora:

- La scatola non va a capo su una nuova riga.
- {{cssxref("width")}}, {{cssxref("height")}} e i margini superiore e inferiore non hanno effetto.
- Il padding e i bordi **superiore e inferiore** modificano la dimensione della scatola senza influire sulla posizione del contenuto circostante, il che può causare sovrapposizioni.
- Il padding, i margini e i bordi **sinistro e destro** influenzano la posizione del contenuto inline circostante.

Alcuni elementi HTML, come `<a>`, `<span>`, `<em>` e `<strong>`, utilizzano `inline` come tipo di display esterno predefinito.

Il layout block e inline è il modo predefinito in cui gli elementi si comportano sul Web. Per impostazione predefinita e senza altre istruzioni, gli elementi all'interno di una scatola sono disposti anch'essi nel **[flusso normale](/it/docs/Learn_web_development/Core/CSS_layout/Introduction#normal_layout_flow)** e si comportano come scatole block o inline.

## Tipi di display interno ed esterno

I valori di display `block` e `inline` sono detti tipi di **display esterno**: influenzano il modo in cui la scatola viene disposta rispetto alle altre scatole circostanti. Le scatole hanno anche un tipo di **display interno**, che stabilisce come vengono disposti gli elementi al loro interno.

È possibile modificare il tipo di display interno impostando un valore di display interno, ad esempio `display: flex;`. L'elemento utilizzerà comunque il tipo di display esterno `block`, ma il tipo di display interno diventerà `flex`. Tutti i figli diretti di questa scatola diventeranno elementi flex e si comporteranno secondo la specifica [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox).

Proseguendo nello studio più dettagliato di CSS Layout, verranno incontrati [`flex`](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox) e vari altri valori interni che le scatole possono avere, ad esempio [`grid`](/it/docs/Learn_web_development/Core/CSS_layout/Grids).

Per il momento non bisogna preoccuparsi troppo della terminologia relativa a interno ed esterno; questo è ciò che avviene internamente ed è stato menzionato qui nel caso in cui si incontri altrove. In genere, si avranno a che fare solo con singoli valori di `display` e non sarà necessario rifletterci molto.

## Esempi di diversi tipi di display

L'esempio seguente contiene tre diversi elementi HTML, tutti con un tipo di display esterno `block`.

- Un paragrafo con un bordo aggiunto in CSS. Il browser lo renderizza come una scatola block. Il paragrafo inizia su una nuova riga e si estende orizzontalmente per riempire tutta la larghezza disponibile.

- Un elenco disposto usando `display: flex`. Questo stabilisce un layout flex per i figli del contenitore, che sono elementi flex disposti per impostazione predefinita in una riga. L'elenco stesso è una scatola block e, come il paragrafo, si espande fino all'intera larghezza del contenitore e va a capo su una nuova riga.

- Un paragrafo a livello block, al cui interno sono presenti due elementi `<span>`. Questi elementi sarebbero normalmente `inline`; tuttavia, uno degli elementi ha una classe `block` e viene impostato su `display: block`. Di conseguenza, quella singola parola inizia su una nuova riga che si estende per tutta la larghezza del suo elemento padre.

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

Nell'esempio successivo è possibile osservare come si comportano gli elementi `inline`.

- Gli elementi `<span>` nel primo paragrafo sono inline per impostazione predefinita e quindi non forzano interruzioni di riga.

- L'elemento `<ul>` impostato su `display: inline-flex` crea una scatola inline contenente alcuni elementi flex.

- Entrambi i paragrafi sono impostati su `display: inline`. Il contenitore flex inline e i paragrafi si trovano tutti sulla stessa riga anziché andare a capo su nuove righe, come farebbero se fossero visualizzati come elementi a livello block.

Per passare da una modalità di display all'altra, è possibile cambiare `display: inline` in `display: block` oppure `display: inline-flex` in `display: flex`:

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

L'aspetto principale da ricordare per ora è il seguente: modificare il valore della proprietà `display` può cambiare il tipo di display esterno di una scatola da block a inline o viceversa. Questo modifica il modo in cui viene visualizzata accanto agli altri elementi nel layout.

## Cos'è il modello a scatola CSS?

Il modello a scatola CSS nel suo complesso si applica alle scatole block e definisce come le diverse parti di una scatola — margine, bordo, padding e contenuto — collaborano per creare una scatola visibile in una pagina. Le scatole inline utilizzano solo _alcuni_ dei comportamenti definiti nel modello a scatola.

Per aggiungere complessità, esistono un modello a scatola standard e uno alternativo. Per impostazione predefinita, i browser utilizzano il modello a scatola standard.

### Parti di una scatola

Una scatola block in CSS è composta da:

- **Scatola del contenuto**: l'area in cui viene visualizzato il contenuto; se ne imposta la dimensione usando proprietà come {{cssxref("width")}} e {{cssxref("height")}}.
- **Scatola del padding**: il padding si trova attorno al contenuto come spazio bianco; se ne imposta la dimensione usando {{cssxref("padding")}} e le proprietà correlate.
- **Scatola del bordo**: la scatola del bordo racchiude il contenuto e qualsiasi padding; se ne imposta la dimensione usando {{cssxref("border")}} e le proprietà correlate.
- **Scatola del margine**: il margine è il livello più esterno e racchiude contenuto, padding e bordo come spazio bianco tra questa scatola e gli altri elementi; se ne imposta la dimensione usando {{cssxref("margin")}} e le proprietà correlate.

Il diagramma seguente mostra questi livelli:

![Diagramma del modello a scatola](box-model.png)

### Il modello a scatola CSS standard

Nel modello a scatola standard, se si impostano valori per le proprietà `width` e `height` di una scatola, questi valori definiscono `width` e `height` della _scatola del contenuto_. A queste dimensioni vengono quindi aggiunti padding e bordi per ottenere la dimensione totale occupata dalla scatola, come mostrato nell'immagine seguente.

Supponendo che una scatola abbia il seguente CSS:

```css
.box {
  width: 350px;
  height: 150px;
  margin: 10px;
  padding: 25px;
  border: 5px solid black;
}
```

Lo spazio _effettivo_ occupato dalla scatola sarà largo `410px` (350 + 25 + 25 + 5 + 5) e alto `210px` (150 + 25 + 25 + 5 + 5).

![Dimensione della scatola quando viene utilizzato il modello a scatola standard.](standard-box-model.png)

> [!NOTE]
> Il margine non viene conteggiato nella dimensione effettiva della scatola: certamente influisce sullo spazio totale che la scatola occupa nella pagina, ma solo sullo spazio esterno alla scatola. L'area della scatola termina al bordo e non si estende nel margine.

### Il modello a scatola CSS alternativo

Nel modello a scatola alternativo, qualsiasi larghezza è la larghezza della scatola visibile nella pagina. La larghezza dell'area del contenuto corrisponde a tale larghezza meno la larghezza di padding e bordo, come mostrato nell'immagine seguente. Questo è conveniente perché non è necessario sommare bordo e padding per ottenere la dimensione reale della scatola.

Per attivare il modello alternativo per un elemento, impostare `box-sizing: border-box` su di esso:

```css
.box {
  box-sizing: border-box;
}
```

Supponendo che la scatola abbia lo stesso CSS di prima:

```css
.box {
  width: 350px;
  height: 150px;
  margin: 10px;
  padding: 25px;
  border: 5px solid black;
}
```

Lo spazio _effettivo_ occupato dalla scatola sarà ora `350px` nella direzione inline e `150px` nella direzione block.

![Dimensione della scatola quando viene utilizzato il modello a scatola alternativo.](alternate-box-model.png)

Per utilizzare il modello a scatola alternativo per tutti gli elementi, scelta comune tra gli sviluppatori, impostare la proprietà `box-sizing` sull'elemento `<html>` e fare in modo che tutti gli altri elementi ereditino quel valore:

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

Per comprendere l'idea alla base, è possibile leggere [l'articolo di CSS Tricks su box-sizing](https://css-tricks.com/inheriting-box-sizing-probably-slightly-better-best-practice/).

## Sperimentare con i modelli a scatola

Nell'esempio seguente sono visibili due scatole. Entrambe hanno una classe `.box`, che assegna loro gli stessi `width`, `height`, `margin`, `border` e `padding`. L'unica differenza è che la seconda scatola è impostata per utilizzare il modello a scatola alternativo.
È possibile modificare la dimensione della seconda scatola, aggiungendo CSS alla classe `.alternate`, affinché corrisponda alla prima in larghezza e altezza?

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
> È possibile trovare una soluzione per questa attività [nel nostro repository css-examples](https://github.com/mdn/css-examples/blob/main/learn/solutions.md#the-box-model).

### Uso dei DevTools del browser per visualizzare il modello a scatola

Gli [strumenti per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) possono rendere molto più semplice la comprensione del modello a scatola: possono mostrare la dimensione dell'elemento insieme a margine, padding e bordo. Ispezionare un elemento in questo modo è un ottimo modo per verificare se la scatola ha davvero la dimensione prevista.

![Ispezione del modello a scatola di un elemento usando Firefox DevTools](box-model-devtools.png)

## Margini, padding e bordi

Nell'esempio precedente sono già state viste in azione le proprietà {{cssxref("margin")}}, {{cssxref("padding")}} e {{cssxref("border")}}. Le proprietà utilizzate in quell'esempio sono **abbreviazioni** e consentono di impostare tutti e quattro i lati della scatola contemporaneamente. Queste abbreviazioni hanno anche proprietà estese equivalenti, che consentono di controllare singolarmente i diversi lati della scatola.

Esaminiamo queste proprietà più in dettaglio.

### Margine

Il margine è uno spazio invisibile attorno alla scatola. Allontana gli altri elementi dalla scatola. I margini possono avere valori positivi o negativi. Impostare un margine negativo su un lato della scatola può farla sovrapporre ad altri elementi della pagina. Indipendentemente dall'uso del modello a scatola standard o alternativo, il margine viene sempre aggiunto dopo il calcolo della dimensione della scatola visibile.

È possibile controllare tutti i margini di un elemento contemporaneamente usando la proprietà {{cssxref("margin")}}, oppure ciascun lato singolarmente usando le proprietà estese equivalenti:

- {{cssxref("margin-top")}}
- {{cssxref("margin-right")}}
- {{cssxref("margin-bottom")}}
- {{cssxref("margin-left")}}

#### Sperimentare con i margini

Modificare l'esempio seguente. Provare a cambiare i valori del margine per osservare come la scatola viene spostata dal margine, che crea o rimuove spazio, nel caso di un margine negativo, tra questo elemento e l'elemento contenitore.

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

A seconda che due elementi con margini a contatto abbiano margini positivi o negativi, i risultati saranno diversi:

- Due margini positivi si combinano in un unico margine. La sua dimensione è uguale al margine individuale più grande.
- Due margini negativi collassano e viene usato il valore più piccolo, cioè quello più lontano da zero.
- Se un margine è negativo, il suo valore viene _sottratto_ dal totale.

Nell'esempio seguente sono presenti due paragrafi. Il paragrafo superiore ha un `margin-bottom` di 50 pixel, mentre l'altro ha un `margin-top` di 30 pixel. I margini sono collassati insieme, quindi il margine effettivo tra le scatole è di 50 pixel e non la somma dei due margini.

È possibile verificarlo impostando `margin-top` del secondo paragrafo su `0`. Il margine visibile tra i due paragrafi non cambierà: manterrà i 50 pixel impostati in `margin-bottom` del primo paragrafo. Se lo si imposta su `-10px`, il margine complessivo diventerà `40px`, poiché viene sottratto dai `50px`.

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

Diverse regole stabiliscono quando i margini collassano e quando non collassano. Per ulteriori informazioni, consultare la pagina dettagliata su [come padroneggiare il collasso dei margini](/it/docs/Web/CSS/Guides/Box_model/Margin_collapsing). L'aspetto principale da ricordare è che il collasso dei margini può verificarsi quando si crea spazio con i margini e non si ottiene lo spazio previsto.

> [!NOTE]
> [Imparare i margini tramite flag](https://scrimba.com/frontend-path-c0j/~01e?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> di Scrimba è una lezione interattiva che offre esercizi utili sui margini.

### Bordi

Il bordo viene disegnato tra il margine e il padding di una scatola. Se si utilizza il modello a scatola standard, la dimensione del bordo viene aggiunta a `width` e `height` della scatola del contenuto. Se si utilizza il modello a scatola alternativo, maggiore è lo spessore del bordo, minore è la scatola del contenuto, poiché il bordo occupa parte di `width` e `height` disponibili della scatola dell'elemento.

Per applicare stili ai bordi esiste un gran numero di proprietà: ci sono quattro bordi e ogni bordo ha uno stile, una larghezza e un colore che potrebbero dover essere modificati.

È possibile impostare larghezza, stile o colore di tutti e quattro i bordi contemporaneamente usando la proprietà {{cssxref("border")}}.

Per impostare singolarmente le proprietà di ciascun lato, usare:

- {{cssxref("border-top")}}
- {{cssxref("border-right")}}
- {{cssxref("border-bottom")}}
- {{cssxref("border-left")}}

Per impostare larghezza, stile o colore di tutti i lati, usare:

- {{cssxref("border-width")}}
- {{cssxref("border-style")}}
- {{cssxref("border-color")}}

Per impostare larghezza, stile o colore di un singolo lato, usare una delle proprietà estese più specifiche:

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

#### Sperimentare con i bordi

Nell'esempio seguente sono state utilizzate varie abbreviazioni e proprietà estese per creare bordi. Modificare le diverse proprietà per verificare di comprendere come funzionano. Le pagine MDN relative alle proprietà dei bordi forniscono informazioni sui diversi stili di bordo disponibili.

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

Il padding si trova tra il bordo e l'area del contenuto e viene utilizzato per allontanare il contenuto dal bordo. A differenza dei margini, non è possibile avere padding negativo. Qualsiasi sfondo applicato all'elemento viene visualizzato dietro il padding.

La proprietà {{cssxref("padding")}} controlla il padding su tutti i lati di un elemento. Per controllare ogni lato singolarmente, usare queste proprietà estese:

- {{cssxref("padding-top")}}
- {{cssxref("padding-right")}}
- {{cssxref("padding-bottom")}}
- {{cssxref("padding-left")}}

#### Sperimentare con il padding

Nell'esempio seguente, modificare i valori del padding nella classe `.box` e osservare come cambia il punto di inizio del testo rispetto alla scatola. È inoltre possibile modificare il padding nella classe `.container` per creare spazio tra il contenitore e la scatola. Il padding può essere modificato su qualsiasi elemento per creare spazio tra il suo bordo e ciò che contiene.

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

Quanto descritto sopra si applica completamente alle scatole block. Alcune proprietà possono essere applicate anche alle scatole inline, come quelle create da un elemento `<span>`.

Nell'esempio seguente è presente un `<span>` all'interno di un paragrafo. Sono stati applicati `width`, `height`, `margin`, `border` e `padding`. È possibile osservare che larghezza, altezza e margini superiore e inferiore non influenzano lo `<span>`. Il padding e i bordi superiore e inferiore modificano la dimensione della scatola inline, ma non influenzano la posizione del contenuto circostante. Al contrario, il padding e i bordi superiore e inferiore si sovrappongono alle altre parole nel paragrafo. Solo il padding, i margini e i bordi sinistro e destro influenzano la posizione del testo attorno allo `<span>`.

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
  margin: 20px 30px;
  padding: 10px 20px;
  width: 80px;
  height: 150px;
  background-color: lightblue;
  border: solid blue;
  border-width: 7px 1px;
}
```

{{EmbedLiveSample("inline-box-model")}}

## Uso di display: inline-block

`display: inline-block` è un valore speciale di `display` che offre una via di mezzo tra `inline` e `block`. Usarlo quando non si desidera che un elemento vada a capo su una nuova riga, ma si desidera che rispetti `width` e `height` ed eviti le sovrapposizioni illustrate in precedenza.

Un elemento con `display: inline-block` esegue un sottoinsieme dei comportamenti block già descritti:

- Le proprietà `width` e `height` vengono rispettate.
- `padding`, `margin` e `border` fanno sì che gli altri elementi vengano allontanati dalla scatola.

Tuttavia, non va a capo su una nuova riga e diventa più grande del proprio contenuto solo se vengono aggiunte esplicitamente le proprietà `width` e `height`.

### Sperimentare con inline-block

In questo esempio successivo, è stato aggiunto `display: inline-block` all'elemento `<span>`. Provare a cambiarlo in `display: block` oppure a rimuovere completamente la riga per osservare la differenza tra i modelli di display:

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

Questo può essere utile quando si desidera assegnare a un collegamento un'area cliccabile più ampia aggiungendo `padding`. `<a>` è un elemento inline come `<span>`; è possibile utilizzare `display: inline-block` per consentire l'impostazione del padding, rendendo più semplice per un utente fare clic sul collegamento.

Questo si osserva piuttosto frequentemente nelle barre di navigazione. La navigazione seguente viene visualizzata in una riga usando flexbox ed è stato aggiunto padding all'elemento `<a>` perché si desidera poter modificare `background-color` quando il puntatore passa sopra `<a>`. Il padding sembra sovrapporsi al bordo dell'elemento `<ul>`. Questo accade perché `<a>` è un elemento inline.

Aggiungere `display: inline-block;` alla regola con il selettore `.links-list a` e sarà possibile vedere come risolve questo problema facendo sì che il padding venga rispettato dagli altri elementi:

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
  border: 1px solid black;
}

li {
  margin: 5px;
}

.links-list a {
  background-color: rgb(179 57 81);
  color: white;
  text-decoration: none;
  padding: 1em 2em;
}

.links-list a:hover {
  background-color: rgb(66 28 40);
  color: white;
}
```

{{EmbedLiveSample("inline-block-nav")}}

## Riepilogo

Questo è quasi tutto ciò che serve comprendere sul modello a scatola. Potrebbe essere utile tornare a questa lezione in futuro nel caso sorgano dubbi sulle dimensioni delle scatole nel layout.

Nel prossimo articolo verranno proposti alcuni test da usare per verificare quanto bene sono state comprese e memorizzate le informazioni fornite sul modello a scatola CSS.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Test_your_skills/Selectors", "Learn_web_development/Core/Styling_basics/Test_your_skills/Box_model", "Learn_web_development/Core/Styling_basics")}}
