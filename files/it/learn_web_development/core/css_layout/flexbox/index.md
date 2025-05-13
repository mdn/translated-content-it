---
title: Flexbox
slug: Learn_web_development/Core/CSS_layout/Flexbox
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Positioning", "Learn_web_development/Core/CSS_layout/Grids", "Learn_web_development/Core/CSS_layout")}}

[Flexbox](/it/docs/Web/CSS/CSS_flexible_box_layout) è un metodo di layout unidimensionale per disporre gli elementi in righe o colonne. Gli elementi si _flettono_ (espandono) per riempire spazio aggiuntivo o si riducono per adattarsi a spazi più piccoli. Questo articolo spiega tutti i fondamenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare contenuti con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi di CSS Styling</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Fondamenti di stile del testo e dei caratteri</a>,
        familiarità con i <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">concetti fondamentali dei layout CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo di flexbox — disporre in modo flessibile un insieme di elementi a blocco o inline in una dimensione.</li>
          <li>Terminologia del flex — contenitore flessibile, elemento flessibile, asse principale e asse trasversale.</li>
          <li>Comprendere cosa fornisce di default il <code>display: flex</code>.</li>
          <li>Come avvolgere i contenuti su nuove righe e colonne.</li>
          <li>Dimensionamento e ordinamento flessibile degli elementi flex.</li>
          <li>Giustificazione e allineamento dei contenuti.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Perché flexbox?

Il layout a box flessibile CSS le consente di:

- Centrare verticalmente un blocco di contenuto all'interno del suo elemento padre.
- Fare in modo che tutti i figli di un contenitore occupino una quantità uguale della larghezza/altezza disponibile, indipendentemente da quanta larghezza/altezza sia disponibile.
- Fare in modo che tutte le colonne in un layout a più colonne adottino la stessa altezza anche se contengono una quantità diversa di contenuto.

Le caratteristiche del flexbox possono essere la soluzione perfetta per le vostre esigenze di layout unidimensionale. Approfondiamo per scoprirlo!

> [!NOTE]
> Il corso introduttivo di [Flexbox](https://scrimba.com/learn-html-and-css-c0p/~017?via=mdn) <sup>[_Partner educativo di MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> di Scrimba offre una guida interattiva che spiega quanto sia comune il flexbox sul web e perché è così importante impararlo, seguito da un caso d'uso tipico che dimostra la potenza del flexbox.

## Introduzione a un semplice esempio

In questo articolo, lavorerà attraverso una serie di esercizi per aiutarla a comprendere come funziona il flexbox. Per iniziare, dovrebbe fare una copia locale di HTML e CSS. Carichi il file in un browser moderno (come Firefox o Chrome) e dia un'occhiata al codice nel suo editor di codice. In alternativa, apra l'esempio in {{LiveSampleLink("flexbox_0", "apri il playground")}}.

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

Vedrà che abbiamo un elemento {{htmlelement("header")}} con un'intestazione di livello superiore all'interno e un elemento {{htmlelement("section")}} che contiene tre {{htmlelement("article")}}. Li useremo per creare un layout a tre colonne abbastanza standard.

## Specificare quali elementi disporre come box flessibili

Per cominciare, dobbiamo selezionare quali elementi devono essere disposti come box flessibili. Per farlo, impostiamo un valore speciale di {{cssxref("display")}} sull'elemento padre degli elementi che desidera influenzare. In questo caso vogliamo disporre gli elementi {{htmlelement("article")}}, quindi lo impostiamo sul {{htmlelement("section")}}:

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

Questo fa sì che l'elemento `<section>` diventi un **contenitore flessibile** e i suoi figli diventino **elementi flessibili**. Ecco come appare:

{{EmbedLiveSample("flexbox_1", "100", "210")}}

Questa singola dichiarazione ci dà tutto ciò di cui abbiamo bisogno. Incredibile, vero? Abbiamo un layout a più colonne con colonne di dimensioni uguali, e le colonne hanno tutte la stessa altezza. Questo accade perché i valori predefiniti dati agli elementi flessibili (i figli del contenitore flessibile) sono impostati per risolvere problemi comuni come questo.

Rivediamo cosa sta succedendo qui. Aggiungendo un valore di {{cssxref("display")}} `flex` a un elemento, lo si rende un contenitore flessibile. Il contenitore viene visualizzato come {{Glossary("Block-level_content", "contenuto a livello di blocco")}} in termini di interazione con il resto della pagina. Quando l'elemento viene convertito in un contenitore flessibile, i suoi figli vengono convertiti e disposti come elementi flessibili.

Può rendere il contenitore inline usando un [valore di display esterno](/it/docs/Web/CSS/display#outside) (ad esempio, `display: inline flex`), il che influenza come il contenitore stesso è disposto nella pagina. Il valore legacy `inline-flex` visualizza anche il contenitore come inline. Ci concentreremo su come si comportano i contenuti del contenitore in questo tutorial, ma se desidera vedere l'effetto del layout inline rispetto al blocco, può dare un'occhiata al [confronto dei valori](/it/docs/Web/CSS/display#display_value_comparison) sulla pagina della proprietà `display`.

Le sezioni successive spiegano più in dettaglio cosa sono gli elementi flessibili e cosa accade all'interno di un elemento quando lo si rende un contenitore flessibile.

## Il modello flex

Quando gli elementi sono disposti come elementi flessibili, sono disposti lungo due assi:

- L'**asse principale** è l'asse che corre nella direzione in cui sono disposti gli elementi flessibili (ad esempio, come una riga attraverso la pagina, o come una colonna giù per la pagina). L'inizio e la fine di questo asse sono chiamati **main start** e **main end**. La lunghezza dal bordo di main-start al bordo di main-end è la **main size**.
- L'**asse trasversale** è l'asse che corre perpendicolarmente alla direzione in cui sono disposti gli elementi flessibili. L'inizio e la fine di questo asse sono chiamati **cross start** e **cross end**. La lunghezza dal bordo di cross-start al bordo di cross-end è la **cross size**.
- L'elemento padre a cui è impostato `display: flex` (il {{htmlelement("section")}} nel nostro esempio) è chiamato **contenitore flessibile**.
- Gli elementi disposti come box flessibili all'interno del contenitore flessibile sono chiamati **elementi flessibili** (gli elementi {{htmlelement("article")}} nel nostro esempio).

Tenga a mente questa terminologia mentre affronta le sezioni successive. Può sempre tornare a questi concetti se si confonde con i termini utilizzati.

## Colonne o righe?

Flexbox fornisce una proprietà chiamata {{cssxref("flex-direction")}} che specifica in quale direzione corre l'asse principale (in quale direzione sono disposti i figli del flexbox). Per impostazione predefinita, è impostato su `row`, il che li fa disporre in una riga nella direzione in cui funziona la lingua predefinita del suo browser (da sinistra a destra, nel caso di un browser in inglese).

Provi ad aggiungere la seguente dichiarazione alla sua regola {{htmlelement("section")}}:

```css
flex-direction: column;
```

Vedrà che questo mette gli elementi di nuovo in un layout a colonna, molto simile a quello che erano prima di aggiungere qualsiasi CSS. Prima di procedere, elimini questa dichiarazione dal suo esempio.

> [!NOTE]
> Può anche disporre gli elementi flessibili in una direzione inversa utilizzando i valori `row-reverse` e `column-reverse`. Sperimenta con questi valori anche!

## Avvolgere

Un problema che si presenta quando si ha una larghezza o altezza fissa nel layout è che alla fine i suoi figli del flexbox traboccheranno dal loro contenitore, interrompendo il layout. Nel seguente esempio abbiamo 5 {{htmlelement("article")}}, che non si adattano perché hanno un `min-width` di `400px`, quindi c'è uno scroll orizzontale.

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

Qui vediamo che i figli stanno effettivamente fuoriuscendo dal loro contenitore. Per impostazione predefinita, il browser tenta di posizionare tutti gli elementi flessibili in una singola riga se il `flex-direction` è impostato su `row` o in una singola colonna se il `flex-direction` è impostato su `column`.

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

Un modo in cui può risolvere questo problema è aggiungere la seguente dichiarazione alla sua regola {{htmlelement("section")}}:

```css live-sample___flex-wrap_1
section {
  flex-wrap: wrap;
}
```

Vedrà che il layout appare molto meglio con questo incluso:

{{EmbedLiveSample("flex-wrap_1", "100", "430")}}

Ora abbiamo più righe. Ogni riga ha quanti più figli del flexbox possibile. Qualsiasi eccedenza viene spostata alla riga successiva.

Ma possiamo fare di più qui. Prima di tutto, provi a cambiare il suo valore della proprietà {{cssxref("flex-direction")}} in `row-reverse`. Ora vedrà che ha ancora il suo layout a più righe, ma inizia dall'angolo opposto della finestra del browser e scorre al contrario.

## Sintassi abbreviata flex-flow

A questo punto vale la pena notare che esiste una sintassi abbreviata per {{cssxref("flex-direction")}} e {{cssxref("flex-wrap")}}: {{cssxref("flex-flow")}}. Quindi, ad esempio, può sostituire

```css
flex-direction: row;
flex-wrap: wrap;
```

con

```css
flex-flow: row wrap;
```

## Dimensionamento flessibile degli elementi flessibili

Ora torniamo al nostro primo esempio e vediamo come possiamo controllare quale proporzione dello spazio gli elementi flessibili occupano rispetto agli altri elementi flessibili.

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

Nel suo esempio locale, aggiunga la seguente regola alla fine del suo CSS:

```css live-sample___flexbox_2
article {
  flex: 1;
}
```

{{EmbedLiveSample("flexbox_2", "100", "210")}}

Questo è un valore di proporzione senza unità che determina quanto spazio disponibile lungo l'asse principale ogni elemento flessibile occuperà rispetto agli altri elementi flessibili. In questo caso, daremo a ciascun elemento {{htmlelement("article")}} lo stesso valore (un valore di `1`), il che significa che tutti occuperanno una quantità uguale dello spazio residuo rimasto dopo che le proprietà come il padding e il margine sono state impostate. Questo valore è proporzionalmente condiviso tra gli elementi flessibili: dare a ciascun elemento flessibile un valore di `400000` avrebbe esattamente lo stesso effetto.

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

Ora aggiunga la seguente regola sotto la precedente:

```css live-sample___flexbox_3
article:nth-of-type(3) {
  flex: 2;
}
```

{{EmbedLiveSample("flexbox_3", "100", "210")}}

Ora quando aggiorna, vedrà che il terzo {{htmlelement("article")}} occupa il doppio dello spazio disponibile rispetto agli altri due. Ci sono ora quattro unità di proporzione disponibili in totale (dato che 1 + 1 + 2 = 4). I primi due elementi flessibili hanno una unità ciascuno, quindi ciascuno occupa 1/4 dello spazio disponibile. Il terzo ha due unità, quindi occupa 2/4 dello spazio disponibile (o un mezzo).

Può anche specificare un valore di dimensione minima all'interno del valore flex. Provi ad aggiornare le sue regole di articolo esistenti in questo modo:

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

Questo fondamentalmente afferma: "A ciascun elemento flessibile verranno dati prima `100px` dello spazio disponibile. Dopo di ciò, il resto dello spazio disponibile verrà condiviso secondo le unità di proporzione." Vedrà una differenza nel modo in cui lo spazio viene condiviso.

{{EmbedLiveSample("flexbox_4", "100", "210")}}

Tutti gli elementi flessibili hanno una larghezza minima di 100 pixel—impostata usando 'flex'. Il valore di flex per i primi due elementi flessibili è 1 e per il terzo elemento è 2. Questo divide il restante spazio nel contenitore flessibile in 4 unità di proporzione. Una unità viene assegnata a ciascuno dei primi due elementi flessibili e 2 unità vengono assegnate al terzo elemento flessibile, rendendo il terzo elemento flessibile più ampio degli altri due, che hanno la stessa larghezza.

Il vero valore del flexbox può essere visto nella sua flessibilità/responsività. Se ridimensiona la finestra del browser o aggiunge un altro elemento {{htmlelement("article")}}, il layout continua a funzionare correttamente.

## flex: sintassi abbreviata rispetto alla sintassi completa

{{cssxref("flex")}} è una proprietà abbreviata che può specificare fino a tre valori differenti:

- Il valore di proporzione senza unità di cui abbiamo parlato sopra. Questo può essere specificato separatamente utilizzando la proprietà completa {{cssxref("flex-grow")}}.
- Un secondo valore di proporzione senza unità, {{cssxref("flex-shrink")}}, che entra in gioco quando gli elementi flessibili superano il loro contenitore. Questo valore specifica quanto un elemento si restringerà per prevenire il traboccamento. Questo è un aspetto piuttosto avanzato del flexbox e non lo tratteremo ulteriormente in questo articolo.
- Il valore di dimensione minima di cui abbiamo parlato sopra. Questo può essere specificato separatamente utilizzando il valore completo {{cssxref("flex-basis")}}.

Consigliamo di non usare le proprietà complete del flex a meno che non sia strettamente necessario (ad esempio, per sovrascrivere qualcosa impostato in precedenza). Portano a un sacco di codice extra e possono essere alquanto confusi.

## Allineamento orizzontale e verticale

Può anche usare le caratteristiche del flexbox per allineare gli elementi flessibili lungo l'asse principale o trasversale. Esploriamo questo aspetto osservando un nuovo esempio:

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

Trasformeremo questo in un pulsante/toolbar ordinato e flessibile. Al momento vedrà una barra di menu orizzontale con alcuni pulsanti ammassati nell'angolo in alto a sinistra.

{{EmbedLiveSample("flex-align_0", "100", "125")}}

Per prima cosa, faccia una copia locale di questo esempio.

Ora, aggiunga il seguente codice alla fine del CSS dell'esempio:

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

Aggiorni la pagina e vedrà che i pulsanti ora sono centrati orizzontalmente e verticalmente. Abbiamo fatto questo tramite due nuove proprietà. Gli elementi flessibili sono posizionati al centro dell'asse trasversale impostando la proprietà `align-items` su `center`. Gli elementi flessibili sono distribuiti uniformemente lungo l'asse principale impostando la proprietà `justify-content` su `space-around`.

La proprietà {{cssxref("align-items")}} controlla dove gli elementi flessibili si trovano sull'asse trasversale.

- Per impostazione predefinita, il valore è `normal` che si comporta come `stretch` nel flexbox. Questo allunga tutti gli elementi flessibili per riempire il padre nella direzione dell'asse trasversale. Se il padre non ha una dimensione fissa nella direzione dell'asse trasversale, allora tutti gli elementi flessibili diventeranno alti (o larghi) quanto l'elemento flessibile più alto (o largo). Questo è il motivo per cui il nostro primo esempio aveva colonne di altezza uguale per impostazione predefinita.
- Il valore `center` che abbiamo usato nel nostro codice sopra fa sì che gli elementi mantengano le loro dimensioni intrinseche, ma siano centrati lungo l'asse trasversale. Questo è il motivo per cui i pulsanti dell'esempio attuale sono centrati verticalmente.
- Può anche avere valori come `flex-start`, `self-start` o `start` e `flex-end`, `self-end` o `end`, che allineano tutti gli elementi all'inizio e alla fine dell'asse trasversale rispettivamente. I valori `baseline` allineeranno gli elementi flessibili in base alla loro linea di base; fondamentalmente, la parte inferiore di ciascun elemento flessibile prima linea di testo sarà allineata con la parte inferiore della prima linea dell'elemento con la maggiore distanza tra il cross start e quella linea di base. Veda {{cssxref("align-items")}} per i dettagli completi.

Può sovrascrivere il comportamento di {{cssxref("align-items")}} per singoli elementi flessibili applicando la proprietà {{cssxref("align-self")}} a essi. Ad esempio, provi ad aggiungere quanto segue al suo CSS:

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

Dia un'occhiata all'effetto che ha e lo rimuova di nuovo quando ha finito.

{{cssxref("justify-content")}} controlla dove gli elementi flessibili si trovano sull'asse principale.

- Il valore predefinito è `normal`, che si comporta come `start`, facendoli posizionare all'inizio dell'asse principale.
- Può utilizzare `end` o `flex-end` per farli posizionare alla fine.
- I valori `left` e `right` si comportano come `start` o `end` a seconda della direzione della modalità di scrittura.
- `center` è anche un valore per `justify-content`. Metterà gli elementi flessibili al centro dell'asse principale.
- Il valore che abbiamo utilizzato sopra, `space-around`, è utile: distribuisce tutti gli elementi uniformemente lungo l'asse principale con un po' di spazio lasciato a ciascuna estremità.
- C'è un altro valore, `space-between`, che è molto simile a `space-around` tranne per il fatto che non lascia alcuno spazio a ciascuna estremità.

La proprietà [`justify-items`](/it/docs/Web/CSS/justify-items) viene ignorata nei layout flexbox.

Vorremmo incoraggiare a giocare con questi valori per vedere come funzionano prima di procedere.

## Ordinare gli elementi flessibili

Flexbox ha anche una funzione per cambiare l'ordine di visualizzazione degli elementi flessibili senza influenzare l'ordine del sorgente. Questa è un'altra cosa che è impossibile fare con i metodi di layout tradizionali.

Provi ad aggiungere il seguente CSS al suo codice esempio della barra dei pulsanti:

```css
button:first-child {
  order: 1;
}
```

Aggiorni e vedrà che il pulsante "Smile" si è spostato alla fine dell'asse principale. Vediamo più nel dettaglio come funziona:

- Per impostazione predefinita, tutti gli elementi flessibili hanno un valore di {{cssxref("order")}} pari a `0`.
- Gli elementi flessibili con valori di ordine specificati più alti appariranno più tardi nell'ordine di visualizzazione rispetto agli elementi con valori di ordine inferiore.
- Gli elementi flessibili con lo stesso valore di ordine appariranno nel loro ordine sorgente. Quindi se ha quattro elementi la cui order è impostata rispettivamente su `2`, `1`, `1` e `0`, il loro ordine di visualizzazione sarà 4°, 2°, 3° e poi 1°.
- Il 3° elemento appare dopo il 2° perché ha lo stesso valore di ordine ed è dopo di esso nell'ordine del sorgente.

Può impostare valori di ordine negativi per fare in modo che gli elementi appaiano prima di quelli il cui valore è `0`. Ad esempio, potrebbe fare in modo che il pulsante "Blush" appaia all'inizio dell'asse principale utilizzando la seguente regola:

```css
button:last-child {
  order: -1;
}
```

Sebbene possa cambiare l'ordine usando `order`, l'ordine di tabulazione rimane lo stesso dell'ordine del codice. Cambiare l'ordine degli elementi focalizzabili può avere un impatto negativo sull'usabilità per gli utenti che usano la tastiera!

## Box flessibili annidati

È possibile creare layout piuttosto complessi con il flexbox. È perfettamente lecito impostare un elemento flessibile in modo che sia anche un contenitore flessibile, in modo che i suoi figli siano disposti anche come box flessibili.

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

Questo layout complesso ha alcuni elementi flessibili che sono anche contenitori flessibili. L'HTML per questo è piuttosto semplice. Abbiamo un elemento {{htmlelement("section")}} che contiene tre {{htmlelement("article")}}. Il terzo {{htmlelement("article")}} contiene tre {{htmlelement("div")}}, e il primo {{htmlelement("div")}} contiene cinque {{htmlelement("button")}}:

```plain
section - article
          article
          article - div - button
                    div   button
                    div   button
                          button
                          button
```

Vediamo il codice che abbiamo usato per il layout.

Per prima cosa, impostiamo i figli del {{htmlelement("section")}} per essere disposti come box flessibili.

```css
section {
  display: flex;
}
```

Successivamente, impostiamo alcuni valori flex sui {{htmlelement("article")}} stessi. Nota speciale per la seconda regola qui: stiamo impostando il terzo {{htmlelement("article")}} in modo che anche i suoi figli siano disposti come elementi flessibili, ma questa volta li disponiamo come una colonna.

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

Successivamente, selezioniamo il primo {{htmlelement("div")}}. Usiamo prima `flex: 1 100px;` per dare effettivamente un'altezza minima di `100px`, poi impostiamo che i suoi figli (gli elementi {{htmlelement("button")}}) siano disposti anche come elementi flessibili. Qui li disponiamo in una riga che si avvolge e li allineiamo al centro dello spazio disponibile come abbiamo fatto con l'esempio dei pulsanti individuali visto in precedenza.

```css
article:nth-of-type(3) div:first-child {
  flex: 1 100px;
  display: flex;
  flex-flow: row wrap;
  align-items: center;
  justify-content: space-around;
}
```

Infine, impostiamo alcune dimensioni sui pulsanti. Questa volta dando loro un valore flex di `1 auto`. Questo ha un effetto molto interessante, che si vedrà se si prova a ridimensionare la larghezza della finestra del browser. I pulsanti occuperanno quante più spazio possono. Il numero che ne entrerà in una riga verrà determinato dalle dimensioni; oltre quel numero, verranno abbassati a una nuova riga.

```css
button {
  flex: 1 auto;
  margin: 5px;
  font-size: 18px;
  line-height: 1.5;
}
```

## Verifica le sue competenze!

Ha raggiunto la fine di questo articolo, ma ricorda le informazioni più importanti? Può trovare ulteriori test per verificare di aver ricordato queste informazioni prima di procedere — veda [Verifica le sue competenze: Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Test_your_skills/Flexbox).

## Sommario

Questo conclude il nostro tour dei fondamenti del flexbox. Speriamo che si sia divertita e che avrà modo di sperimentarlo ulteriormente mentre prosegue con il suo apprendimento. Prossimamente, esamineremo un altro aspetto importante dei layout CSS: [Griglie CSS](/it/docs/Learn_web_development/Core/CSS_layout/Grids).

## Veda anche

- [Concetti di base del flexbox](/it/docs/Web/CSS/CSS_flexible_box_layout/Basic_concepts_of_flexbox)
- [Allineare gli elementi in un contenitore flessibile](/it/docs/Web/CSS/CSS_flexible_box_layout/Aligning_items_in_a_flex_container)
- [Ordinare gli elementi flessibili](/it/docs/Web/CSS/CSS_flexible_box_layout/Ordering_flex_items)
- [Controllare i rapporti degli elementi flessibili lungo l'asse principale](/it/docs/Web/CSS/CSS_flexible_box_layout/Controlling_ratios_of_flex_items_along_the_main_axis)
- Modulo [CSS flexible box layout](/it/docs/Web/CSS/CSS_flexible_box_layout)
- [Guida CSS-Tricks al flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) — un articolo che spiega tutto sul flexbox in modo visivamente accattivante
- [Flexbox Froggy](https://flexboxfroggy.com/) — un gioco educativo per imparare e comprendere meglio i fondamenti del flexbox

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Positioning", "Learn_web_development/Core/CSS_layout/Grids", "Learn_web_development/Core/CSS_layout")}}
