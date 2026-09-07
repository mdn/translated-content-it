---
title: Flexbox
slug: Learn_web_development/Core/CSS_layout/Flexbox
l10n:
  sourceCommit: c55a38c71a356502e6d3df5e246b6e89f9e83498
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Test_your_skills/Position", "Learn_web_development/Core/CSS_layout/Test_your_skills/Flexbox", "Learn_web_development/Core/CSS_layout")}}

[Flexbox](/it/docs/Web/CSS/Guides/Flexible_box_layout) è un metodo di layout unidimensionale per disporre gli elementi in righe o colonne. Gli elementi _flex_ (si espandono) per riempire lo spazio aggiuntivo oppure si restringono per adattarsi a spazi più piccoli. Questo articolo spiega tutti i concetti fondamentali.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo styling CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Nozioni fondamentali sullo styling di testo e font</a>,
        familiarità con i <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">concetti fondamentali del layout CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo di flexbox — disporre in modo flessibile un insieme di elementi block o inline in una dimensione.</li>
          <li>Terminologia flex — flex container, flex item, asse principale e asse trasversale.</li>
          <li>Comprendere cosa offre per impostazione predefinita <code>display: flex</code>.</li>
          <li>Come mandare a capo il contenuto in nuove righe e colonne.</li>
          <li>Dimensionamento flessibile e ordinamento dei flex item.</li>
          <li>Giustificare e allineare il contenuto.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Perché flexbox?

Il layout CSS flexible box consente di:

- Centrare verticalmente un blocco di contenuto all'interno del suo elemento padre.
- Fare in modo che tutti gli elementi figli di un contenitore occupino una quantità uguale della larghezza/altezza disponibile, indipendentemente da quanta larghezza/altezza è disponibile.
- Fare in modo che tutte le colonne in un layout a più colonne abbiano la stessa altezza, anche se contengono quantità diverse di contenuto.

Le funzionalità di flexbox potrebbero essere la soluzione perfetta per le esigenze di layout unidimensionale. Vediamo più nel dettaglio!

> [!NOTE]
> Lo scrim introduttivo di Scrimba su [Flexbox](https://scrimba.com/learn-html-and-css-c0p/~017?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> fornisce una guida interattiva che mostra quanto flexbox sia comune sul web e quindi perché sia così importante impararlo, e illustra un caso d'uso tipico che dimostra la potenza di flexbox.

## Introduzione a un semplice esempio

In questo articolo verrà affrontata una serie di esercizi per comprendere come funziona flexbox. Per iniziare, creare una copia locale dell'HTML e del CSS. Caricarla in un browser moderno (come Firefox o Chrome) e osservare il codice nell'editor di codice. In alternativa, fare clic sul pulsante "Play" per aprirlo nel playground.

```html live-sample___flexbox_0
<header>
  <h1>Sample flexbox example</h1>
</header>
<section>
  <article>
    <h2>First article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Second article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Third article</h2>
    <p>Content…</p>
  </article>
</section>
```

```css live-sample___flexbox_0
body {
  font-family: sans-serif;
  margin: 0;
}
header {
  background: purple;
  height: 100px;
}
h1 {
  text-align: center;
  color: white;
  line-height: 100px;
  margin: 0;
}
section {
  zoom: 0.8;
}
article {
  padding: 10px;
  margin: 10px;
  background: aqua;
}
/* Add your flexbox CSS below here */
```

{{EmbedLiveSample("flexbox_0", "100", "415")}}

Si può vedere che è presente un elemento {{htmlelement("header")}} con al suo interno un'intestazione di primo livello e un elemento {{htmlelement("section")}} contenente tre {{htmlelement("article")}}. Questi verranno usati per creare un layout abbastanza standard a tre colonne.

## Specificare quali elementi disporre come flexible box

Per iniziare, occorre selezionare gli elementi che devono essere disposti come flexible box. Per farlo, si imposta un valore speciale di {{cssxref("display")}} sull'elemento padre degli elementi che si desidera influenzare. In questo caso si desidera disporre gli elementi {{htmlelement("article")}}, quindi questa proprietà viene impostata su {{htmlelement("section")}}:

```html hidden live-sample___flexbox_1
<header>
  <h1>Sample flexbox example</h1>
</header>
<section>
  <article>
    <h2>First article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Second article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Third article</h2>
    <p>Content…</p>
  </article>
</section>
```

```css hidden live-sample___flexbox_1
body {
  font-family: sans-serif;
  margin: 0;
}
header {
  background: purple;
  height: 100px;
}
h1 {
  text-align: center;
  color: white;
  line-height: 100px;
  margin: 0;
}
section {
  zoom: 0.8;
}
article {
  padding: 10px;
  margin: 10px;
  background: aqua;
}
/* Add your flexbox CSS below here */
```

```css live-sample___flexbox_1
section {
  display: flex;
}
```

Questo fa sì che l'elemento `<section>` diventi un **flex container** e che i suoi figli diventino **flex item**. Ecco come appare:

{{EmbedLiveSample("flexbox_1", "100", "210")}}

Questa singola dichiarazione fornisce tutto ciò che serve. Incredibile, vero? È presente un layout a più colonne con colonne di uguali dimensioni, e tutte le colonne hanno la stessa altezza. Questo perché i valori predefiniti assegnati ai flex item (i figli del flex container) sono configurati per risolvere problemi comuni come questo.

Ricapitoliamo cosa sta accadendo. L'aggiunta di un valore {{cssxref("display")}} di `flex` a un elemento lo rende un flex container. Il contenitore viene visualizzato come {{Glossary("Block-level_content", "contenuto block-level")}} per quanto riguarda la sua interazione con il resto della pagina. Quando l'elemento viene convertito in un flex container, i suoi figli vengono convertiti in flex item (e disposti come tali).

È possibile rendere il contenitore inline usando un [valore `display` esterno](/it/docs/Web/CSS/Reference/Properties/display#outside) (ad esempio, `display: inline flex`), che influenza il layout del contenitore stesso nella pagina.
Anche il valore `inline-flex` di `display`, ormai legacy, visualizza il contenitore come inline.
Questo tutorial si concentra sul comportamento dei contenuti del contenitore, ma per osservare l'effetto del layout inline rispetto a quello block, consultare il [confronto dei valori](/it/docs/Web/CSS/Reference/Properties/display#display_value_comparison) nella pagina della proprietà `display`.

Le sezioni successive spiegano più nel dettaglio cosa sono i flex item e cosa accade all'interno di un elemento quando viene reso un flex container.

## Il modello flex

Quando gli elementi vengono disposti come flex item, vengono organizzati lungo due assi:

![Tre flex item in una lingua da sinistra a destra sono disposti affiancati in un flex container. L'asse principale — l'asse del flex container nella direzione in cui sono disposti i flex item — è orizzontale. Le estremità dell'asse sono main-start e main-end e si trovano rispettivamente a sinistra e a destra. L'asse trasversale è verticale; perpendicolare all'asse principale. Cross-start e cross-end si trovano rispettivamente in alto e in basso. La lunghezza del flex item lungo l'asse principale, in questo caso la larghezza, viene chiamata main size, mentre la lunghezza del flex item lungo l'asse trasversale, in questo caso l'altezza, viene chiamata cross size.](flex_terms.png)

- L'**asse principale** è l'asse che segue la direzione in cui sono disposti i flex item (ad esempio, come una riga attraverso la pagina o una colonna lungo la pagina). I punti iniziale e finale di questo asse vengono chiamati rispettivamente **main start** e **main end**. La lunghezza di un flex item lungo l'asse principale è la **main size**.
- L'**asse trasversale** è l'asse perpendicolare alla direzione in cui sono disposti i flex item. I punti iniziale e finale di questo asse vengono chiamati rispettivamente **cross start** e **cross end**. La lunghezza di un flex item lungo l'asse trasversale è la **cross size**.
- L'elemento padre su cui è impostato `display: flex` (il {{htmlelement("section")}} nel nostro esempio) viene chiamato **flex container**.
- Gli elementi disposti come flexible box all'interno del flex container vengono chiamati **flex item** (gli elementi {{htmlelement("article")}} nel nostro esempio).

Tenere presente questa terminologia durante la lettura delle sezioni successive. È sempre possibile tornare a questa sezione in caso di confusione riguardo a uno dei termini utilizzati.

## Colonne o righe?

Flexbox fornisce una proprietà chiamata {{cssxref("flex-direction")}} che specifica in quale direzione si estende l'asse principale (la direzione in cui vengono disposti gli elementi figli di flexbox). Per impostazione predefinita è impostata su `row`, che li dispone in una riga nella direzione predefinita della lingua del browser (da sinistra a destra nel caso di un browser in inglese).

Provare ad aggiungere la seguente dichiarazione alla regola per {{htmlelement("section")}}:

```css
flex-direction: column;
```

Si vedrà che questo riporta gli elementi a un layout a colonne, molto simile a quello che avevano prima di aggiungere CSS. Prima di proseguire, eliminare questa dichiarazione dall'esempio.

> [!NOTE]
> È inoltre possibile disporre i flex item in direzione inversa usando i valori `row-reverse` e `column-reverse`. Provare anche questi valori.

## A capo

Un problema che si presenta quando il layout ha una larghezza o un'altezza fissa è che, prima o poi, gli elementi figli di flexbox traboccheranno dal contenitore, compromettendo il layout. Nell'esempio seguente sono presenti 5 {{htmlelement("article")}}, che non entrano perché hanno un `min-width` di `400px`, quindi è presente uno scorrimento orizzontale.

```html hidden live-sample___flex-wrap_0
<header>
  <h1>Sample flexbox example</h1>
</header>
<section>
  <article>
    <h2>First article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Second article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Third article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Fourth article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Fifth article</h2>
    <p>Content…</p>
  </article>
</section>
```

```css hidden live-sample___flex-wrap_0
body {
  font-family: sans-serif;
  margin: 0;
}
header {
  background: purple;
  height: 100px;
}
h1 {
  text-align: center;
  color: white;
  line-height: 100px;
  margin: 0;
}
article {
  min-width: 400px;
  padding: 10px;
  margin: 10px;
  background: aqua;
}
section {
  display: flex;
  flex-direction: row;
  zoom: 0.8;
}
```

{{EmbedLiveSample("flex-wrap_0", "100", "230")}}

Qui si vede che gli elementi figli fuoriescono effettivamente dal loro contenitore. Per impostazione predefinita, il browser prova a collocare tutti i flex item in una singola riga se `flex-direction` è impostata su `row`, oppure in una singola colonna se `flex-direction` è impostata su `column`.

```html hidden live-sample___flex-wrap_1
<header>
  <h1>Sample flexbox example</h1>
</header>
<section>
  <article>
    <h2>First article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Second article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Third article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Fourth article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Fifth article</h2>
    <p>Content…</p>
  </article>
</section>
```

```css hidden live-sample___flex-wrap_1
body {
  font-family: sans-serif;
  margin: 0;
}
header {
  background: purple;
  height: 100px;
}
h1 {
  text-align: center;
  color: white;
  line-height: 100px;
  margin: 0;
}
article {
  min-width: 400px;
  padding: 10px;
  margin: 10px;
  background: aqua;
}
section {
  display: flex;
  flex-direction: row;
  zoom: 0.8;
}
```

Un modo per risolvere il problema consiste nell'aggiungere la seguente dichiarazione alla regola per {{htmlelement("section")}}:

```css live-sample___flex-wrap_1
section {
  flex-wrap: wrap;
}
```

Con questa inclusa, il layout appare molto migliore:

{{EmbedLiveSample("flex-wrap_1", "100", "430")}}

Ora sono presenti più righe. Ogni riga contiene il maggior numero possibile di elementi figli flexbox che vi può entrare in modo ragionevole. Qualsiasi contenuto in eccesso viene spostato alla riga successiva.

Ma è possibile fare di più. Innanzitutto, provare a modificare il valore della proprietà {{cssxref("flex-direction")}} in `row-reverse`. Si vedrà che il layout rimane su più righe, ma inizia dall'angolo opposto della finestra del browser e procede in direzione inversa.

## Abbreviazione flex-flow

A questo punto vale la pena notare che esiste una proprietà abbreviata per {{cssxref("flex-direction")}} e {{cssxref("flex-wrap")}}: {{cssxref("flex-flow")}}. Quindi, ad esempio, è possibile sostituire

```css
flex-direction: row;
flex-wrap: wrap;
```

con

```css
flex-flow: row wrap;
```

## Dimensionamento flessibile dei flex item

Torniamo ora al primo esempio e vediamo come controllare quale proporzione di spazio occupano i flex item rispetto agli altri flex item.

```html hidden live-sample___flexbox_2
<header>
  <h1>Sample flexbox example</h1>
</header>
<section>
  <article>
    <h2>First article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Second article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Third article</h2>
    <p>Content…</p>
  </article>
</section>
```

```css hidden live-sample___flexbox_2
body {
  font-family: sans-serif;
  margin: 0;
}
header {
  background: purple;
  height: 100px;
}
h1 {
  text-align: center;
  color: white;
  line-height: 100px;
  margin: 0;
}
article {
  padding: 10px;
  margin: 10px;
  background: aqua;
}
section {
  zoom: 0.8;
  display: flex;
}
```

Nella copia locale, aggiungere la seguente regola alla fine del CSS:

```css live-sample___flexbox_2
article {
  flex: 1;
}
```

{{EmbedLiveSample("flexbox_2", "100", "210")}}

Questo è un valore proporzionale senza unità che determina quanto spazio disponibile lungo l'asse principale occuperà ciascun flex item rispetto agli altri flex item. In questo caso, viene assegnato lo stesso valore a ciascun elemento {{htmlelement("article")}} (un valore di `1`), il che significa che tutti occuperanno una quantità uguale dello spazio rimanente dopo l'impostazione di proprietà come padding e margin. Questo valore viene condiviso proporzionalmente tra i flex item: assegnare a ogni flex item un valore di `400000` avrebbe esattamente lo stesso effetto.

```html hidden live-sample___flexbox_3
<header>
  <h1>Sample flexbox example</h1>
</header>
<section>
  <article>
    <h2>First article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Second article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Third article</h2>
    <p>Content…</p>
  </article>
</section>
```

```css hidden live-sample___flexbox_3
body {
  font-family: sans-serif;
  margin: 0;
}
header {
  background: purple;
  height: 100px;
}
h1 {
  text-align: center;
  color: white;
  line-height: 100px;
  margin: 0;
}
article {
  padding: 10px;
  margin: 10px;
  background: aqua;
}
section {
  zoom: 0.8;
  display: flex;
}
article {
  flex: 1;
}
```

Ora aggiungere la seguente regola sotto quella precedente:

```css live-sample___flexbox_3
article:nth-of-type(3) {
  flex: 2;
}
```

{{EmbedLiveSample("flexbox_3", "100", "210")}}

Dopo l'aggiornamento, si vedrà che il terzo {{htmlelement("article")}} occupa il doppio della larghezza disponibile rispetto agli altri due. Ora sono disponibili in totale quattro unità proporzionali (poiché 1 + 1 + 2 = 4). I primi due flex item hanno un'unità ciascuno, quindi ognuno occupa 1/4 dello spazio disponibile. Il terzo ne ha due, quindi occupa 2/4 dello spazio disponibile (ovvero la metà).

È inoltre possibile specificare un valore di dimensione minima nel valore flex. Provare ad aggiornare le regole article esistenti come segue:

```html hidden live-sample___flexbox_4
<header>
  <h1>Sample flexbox example</h1>
</header>
<section>
  <article>
    <h2>First article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Second article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Third article</h2>
    <p>Content…</p>
  </article>
</section>
```

```css hidden live-sample___flexbox_4
body {
  font-family: sans-serif;
  margin: 0;
}
header {
  background: purple;
  height: 100px;
}
h1 {
  text-align: center;
  color: white;
  line-height: 100px;
  margin: 0;
}
article {
  padding: 10px;
  margin: 10px;
  background: aqua;
}
section {
  zoom: 0.8;
  display: flex;
}
```

```css live-sample___flexbox_4
article {
  flex: 1 100px;
}

article:nth-of-type(3) {
  flex: 2 100px;
}
```

Questo afferma essenzialmente: "A ciascun flex item verranno prima assegnati `100px` dello spazio disponibile. Successivamente, il resto dello spazio disponibile verrà condiviso in base alle unità proporzionali." Si noterà una differenza nel modo in cui lo spazio viene condiviso.

{{EmbedLiveSample("flexbox_4", "100", "210")}}

Tutti i flex item hanno una larghezza minima di 100 pixel, impostata usando 'flex'. Il valore flex per i primi due flex item è 1 e per il terzo elemento è 2. Questo divide lo spazio rimanente nel flex container in 4 unità proporzionali. Una unità viene assegnata a ciascuno dei primi due flex item e 2 unità vengono assegnate al terzo flex item, rendendo il terzo flex item più largo degli altri due, che hanno la stessa larghezza.

Il vero valore di flexbox si osserva nella sua flessibilità/adattabilità. Se si ridimensiona la finestra del browser o si aggiunge un altro elemento {{htmlelement("article")}}, il layout continua a funzionare correttamente.

## flex: abbreviazione rispetto alla forma estesa

{{cssxref("flex")}} è una proprietà abbreviata che può specificare fino a tre valori diversi:

- Il valore proporzionale senza unità discusso in precedenza. Può essere specificato separatamente usando la proprietà in forma estesa {{cssxref("flex-grow")}}.
- Un secondo valore proporzionale senza unità, {{cssxref("flex-shrink")}}, che entra in gioco quando i flex item traboccano dal loro contenitore. Questo valore specifica di quanto un elemento si ridurrà per prevenire il traboccamento. Si tratta di una funzionalità flexbox piuttosto avanzata e non verrà approfondita ulteriormente in questo articolo.
- Il valore di dimensione minima discusso in precedenza. Può essere specificato separatamente usando il valore in forma estesa {{cssxref("flex-basis")}}.

Si consiglia di non usare le proprietà flex in forma estesa, a meno che non sia davvero necessario (ad esempio, per sovrascrivere qualcosa impostato in precedenza). Comportano la scrittura di molto codice aggiuntivo e possono risultare in qualche modo confuse.

## Allineamento orizzontale e verticale

È inoltre possibile usare le funzionalità di flexbox per allineare i flex item lungo l'asse principale o quello trasversale. Esploriamo questo aspetto osservando un nuovo esempio:

```html live-sample___flex-align_0
<div>
  <button>Smile</button>
  <button>Laugh</button>
  <button>Wink</button>
  <button>Shrug</button>
  <button>Blush</button>
</div>
```

```css live-sample___flex-align_0
body {
  font-family: sans-serif;
  width: 90%;
  max-width: 960px;
  margin: 10px auto;
}
div {
  height: 100px;
  border: 1px solid black;
}
button {
  font-size: 18px;
  line-height: 1.5;
  width: 15%;
}
/* Add your flexbox CSS below here */
```

Questo verrà trasformato in una barra di pulsanti/strumenti ordinata e flessibile. Al momento è visibile una barra dei menu orizzontale con alcuni pulsanti ammassati nell'angolo superiore sinistro.

{{EmbedLiveSample("flex-align_0", "100", "125")}}

Per prima cosa, creare una copia locale di questo esempio.

Ora aggiungere quanto segue alla fine del CSS dell'esempio:

```html hidden live-sample___flex-align_1
<div>
  <button>Smile</button>
  <button>Laugh</button>
  <button>Wink</button>
  <button>Shrug</button>
  <button>Blush</button>
</div>
```

```css hidden live-sample___flex-align_1
body {
  font-family: sans-serif;
  width: 90%;
  max-width: 960px;
  margin: 10px auto;
}
div {
  height: 100px;
  border: 1px solid black;
}
button {
  font-size: 18px;
  line-height: 1.5;
  width: 15%;
}
/* Add your flexbox CSS below here */
```

```css live-sample___flex-align_1
div {
  display: flex;
  align-items: center;
  justify-content: space-around;
}
```

{{EmbedLiveSample("flex-align_1", "100", "125")}}

Aggiornare la pagina e si vedrà che i pulsanti sono ora ben centrati orizzontalmente e verticalmente. Questo è stato ottenuto tramite due nuove proprietà. I flex item sono posizionati al centro dell'asse trasversale impostando la proprietà `align-items` su `center`. I flex item sono distribuiti uniformemente lungo l'asse principale impostando la proprietà `justify-content` su `space-around`.

La proprietà {{cssxref("align-items")}} controlla la posizione dei flex item sull'asse trasversale.

- Per impostazione predefinita, il valore è `normal`, che in flexbox si comporta come `stretch`. Questo allunga tutti i flex item per riempire l'elemento padre nella direzione dell'asse trasversale. Se l'elemento padre non ha una dimensione fissa nella direzione dell'asse trasversale, tutti i flex item avranno la stessa altezza (o larghezza) del flex item più alto (o più largo). In questo modo il primo esempio ha avuto per impostazione predefinita colonne di uguale altezza.
- Il valore `center` usato nel codice precedente fa sì che gli elementi mantengano le loro dimensioni intrinseche, ma vengano centrati lungo l'asse trasversale. Per questo i pulsanti dell'esempio corrente sono centrati verticalmente.
- È inoltre possibile usare valori come `flex-start`, `self-start` o `start` e `flex-end`, `self-end` o `end`, che allineeranno tutti gli elementi rispettivamente all'inizio e alla fine dell'asse trasversale. I valori `baseline` allineeranno i flex item in base alla loro linea di base; essenzialmente, la parte inferiore della prima riga di testo di ciascun flex item verrà allineata con la parte inferiore della prima riga dell'elemento con la maggiore distanza tra il cross start e quella linea di base. Consultare {{cssxref("align-items")}} per tutti i dettagli.

È possibile sovrascrivere il comportamento di {{cssxref("align-items")}} per singoli flex item applicando loro la proprietà {{cssxref("align-self")}}. Ad esempio, provare ad aggiungere quanto segue al CSS:

```html hidden live-sample___flex-align_2
<div>
  <button>Smile</button>
  <button>Laugh</button>
  <button>Wink</button>
  <button>Shrug</button>
  <button>Blush</button>
</div>
```

```css hidden live-sample___flex-align_2
body {
  font-family: sans-serif;
  width: 90%;
  max-width: 960px;
  margin: 10px auto;
}
div {
  height: 100px;
  border: 1px solid black;
}
button {
  font-size: 18px;
  line-height: 1.5;
  width: 15%;
}
div {
  display: flex;
  align-items: center;
  justify-content: space-around;
}
/* Add your flexbox CSS below here */
```

```css live-sample___flex-align_2
button:first-child {
  align-self: flex-end;
}
```

{{EmbedLiveSample("flex-align_2", "100", "125")}}

Osservare l'effetto prodotto e rimuoverlo nuovamente al termine.

{{cssxref("justify-content")}} controlla la posizione dei flex item sull'asse principale.

- Il valore predefinito è `normal`, che si comporta come `start`, facendo sì che tutti gli elementi si trovino all'inizio dell'asse principale.
- È possibile usare `end` o `flex-end` per disporli alla fine.
- I valori `left` e `right` si comportano come `start` o `end` a seconda della direzione della modalità di scrittura.
- Anche `center` è un valore per `justify-content`. Farà sì che i flex item si trovino al centro dell'asse principale.
- Il valore usato in precedenza, `space-around`, è utile: distribuisce tutti gli elementi uniformemente lungo l'asse principale lasciando un po' di spazio a entrambe le estremità.
- Esiste un altro valore, `space-between`, molto simile a `space-around`, con la differenza che non lascia spazio a nessuna delle estremità.

La proprietà {{cssxref("justify-items")}} viene ignorata nei layout flexbox.

Si incoraggia a sperimentare con questi valori per vedere come funzionano prima di proseguire.

## Ordinare i flex item

Flexbox dispone inoltre di una funzionalità per modificare l'ordine di layout dei flex item senza influenzare l'ordine sorgente. Anche questa è un'operazione impossibile da eseguire con i metodi di layout tradizionali.

Provare ad aggiungere il seguente CSS al codice dell'esempio della barra di pulsanti:

```css
button:first-child {
  order: 1;
}
```

Aggiornando la pagina, si vedrà che il pulsante "Smile" si è spostato alla fine dell'asse principale. Vediamo più nel dettaglio come funziona:

- Per impostazione predefinita, tutti i flex item hanno un valore {{cssxref("order")}} pari a `0`.
- I flex item con valori order specificati più elevati appariranno più tardi nell'ordine di visualizzazione rispetto agli elementi con valori order inferiori.
- I flex item con lo stesso valore order appariranno nel loro ordine sorgente. Quindi, se sono presenti quattro elementi i cui valori order sono stati impostati rispettivamente su `2`, `1`, `1` e `0`, il loro ordine di visualizzazione sarà il quarto, il secondo, il terzo e infine il primo.
- Il terzo elemento appare dopo il secondo perché ha lo stesso valore order e lo segue nell'ordine sorgente.

È possibile impostare valori order negativi per fare apparire gli elementi prima degli elementi con valore `0`. Ad esempio, è possibile far apparire il pulsante "Blush" all'inizio dell'asse principale usando la seguente regola:

```css
button:last-child {
  order: -1;
}
```

Sebbene sia possibile modificare l'ordine usando `order`, l'ordine di tabulazione rimane uguale all'ordine del codice. Modificare l'ordine degli elementi che possono ricevere il focus può influire negativamente sull'usabilità per chi usa la tastiera.

## Flex box annidati

Con flexbox è possibile creare layout piuttosto complessi. È perfettamente accettabile impostare un flex item anche come flex container, in modo che i suoi figli vengano a loro volta disposti come flexible box.

```html hidden live-sample___flex-nesting
<header>
  <h1>Complex flexbox example</h1>
</header>
<section>
  <article>
    <h2>First article</h2>
    <p>Content…</p>
  </article>
  <article>
    <h2>Second article</h2>
    <p>Content…</p>
  </article>
  <article>
    <div>
      <button>Smile</button>
      <button>Laugh</button>
      <button>Wink</button>
      <button>Shrug</button>
      <button>Blush</button>
    </div>
    <div>
      <p>Paragraph one content…</p>
    </div>
    <div>
      <p>Paragraph two content…</p>
    </div>
  </article>
</section>
```

```css hidden live-sample___flex-nesting
body {
  font-family: sans-serif;
  margin: 0;
}
header {
  background: purple;
  height: 100px;
}
h1 {
  text-align: center;
  color: white;
  line-height: 100px;
  margin: 0;
}
article {
  padding: 10px;
  margin: 10px;
  background: aqua;
}
section {
  display: flex;
  zoom: 0.8;
}
article {
  flex: 1 170px;
}
article:nth-of-type(3) {
  flex: 3 170px;
  display: flex;
  flex-flow: column;
}
article:nth-of-type(3) div:first-child {
  flex: 1 100px;
  display: flex;
  flex-flow: row wrap;
  align-items: center;
  justify-content: space-around;
}
button {
  flex: 1 auto;
  margin: 5px;
  font-size: 18px;
  line-height: 1.5;
}
```

{{EmbedLiveSample("flex-nesting", "100", "290")}}

Questo layout complesso contiene alcuni flex item che sono anche flex container. L'HTML è abbastanza semplice. È presente un elemento {{htmlelement("section")}} contenente tre {{htmlelement("article")}}. Il terzo {{htmlelement("article")}} contiene tre {{htmlelement("div")}}, e il primo {{htmlelement("div")}} contiene cinque {{htmlelement("button")}}:

```plain
section - article
          article
          article - div - button
                    div   button
                    div   button
                          button
                          button
```

Vediamo il codice usato per il layout.

Per prima cosa, gli elementi figli di {{htmlelement("section")}} vengono disposti come flexible box.

```css
section {
  display: flex;
}
```

Successivamente, vengono impostati alcuni valori flex sugli stessi {{htmlelement("article")}}. Prestare particolare attenzione alla seconda regola: il terzo {{htmlelement("article")}} viene impostato affinché anche i suoi figli siano disposti come flex item, ma questa volta vengono disposti come una colonna.

```css
article {
  flex: 1 100px;
}

article:nth-of-type(3) {
  flex: 3 100px;
  display: flex;
  flex-flow: column;
}
```

Successivamente, viene selezionato il primo {{htmlelement("div")}}. Prima viene usato `flex: 1 100px;` per assegnargli effettivamente un'altezza minima di `100px`, quindi i suoi figli (gli elementi {{htmlelement("button")}}) vengono impostati affinché siano anch'essi disposti come flex item. Qui vengono disposti in una riga con a capo e allineati al centro dello spazio disponibile, come nell'esempio dei singoli pulsanti visto in precedenza.

```css
article:nth-of-type(3) div:first-child {
  flex: 1 100px;
  display: flex;
  flex-flow: row wrap;
  align-items: center;
  justify-content: space-around;
}
```

Infine, viene impostato un dimensionamento sui pulsanti. Questa volta assegnando loro un valore flex di `1 auto`. Ciò produce un effetto molto interessante, visibile provando a ridimensionare la larghezza della finestra del browser. I pulsanti occuperanno quanto più spazio possibile. Ne entreranno su una riga quanti possono starci comodamente; oltre quel punto, andranno a capo su una nuova riga.

```css
button {
  flex: 1 auto;
  margin: 5px;
  font-size: 18px;
  line-height: 1.5;
}
```

## Riepilogo

Questo conclude il percorso attraverso le basi di flexbox. Si spera che sia stato divertente e che venga sperimentato ampiamente durante il proseguimento dell'apprendimento. Nel prossimo articolo verranno proposti alcuni test da usare per verificare quanto bene siano state comprese e ricordate tutte queste informazioni.

## Vedi anche

- [Concetti di base di flexbox](/it/docs/Web/CSS/Guides/Flexible_box_layout/Basic_concepts)
- [Allineare elementi in un flex container](/it/docs/Web/CSS/Guides/Flexible_box_layout/Aligning_items)
- [Ordinare i flex item](/it/docs/Web/CSS/Guides/Flexible_box_layout/Ordering_items)
- [Controllare le proporzioni dei flex item lungo l'asse principale](/it/docs/Web/CSS/Guides/Flexible_box_layout/Controlling_flex_item_ratios)
- Modulo [CSS flexible box layout](/it/docs/Web/CSS/Guides/Flexible_box_layout)
- [Guida a flexbox di CSS-Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) — un articolo che spiega tutto su flexbox in modo visivamente accattivante
- [Flexbox Froggy](https://flexboxfroggy.com/) — un gioco educativo per imparare e comprendere meglio le basi di flexbox

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Test_your_skills/Position", "Learn_web_development/Core/CSS_layout/Test_your_skills/Flexbox", "Learn_web_development/Core/CSS_layout")}}
