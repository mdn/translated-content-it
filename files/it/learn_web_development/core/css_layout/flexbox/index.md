---
title: Flexbox
slug: Learn_web_development/Core/CSS_layout/Flexbox
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Positioning", "Learn_web_development/Core/CSS_layout/Grids", "Learn_web_development/Core/CSS_layout")}}

[Flexbox](/it/docs/Web/CSS/CSS_flexible_box_layout) è un metodo di layout unidimensionale per disporre elementi in righe o colonne. Gli elementi si espandono o si contraggono per occupare lo spazio disponibile o per adattarsi a spazi più piccoli. Questo articolo spiega tutti i fondamenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare il contenuto con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi di stile CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Fondamenti di stile del testo e dei font</a>,
        familiarità con i <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">concetti fondamentali del layout CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo di flexbox — disporre in maniera flessibile un insieme di elementi block o inline in una dimensione.</li>
          <li>Terminologia flex — contenitore flex, elemento flex, asse principale, e asse trasversale.</li>
          <li>Comprendere cosa offre <code>display: flex</code> per impostazione predefinita.</li>
          <li>Come avvolgere il contenuto su nuove righe e colonne.</li>
          <li>Dimensionamento e ordinamento flessibile degli elementi flex.</li>
          <li>Giustificare e allineare il contenuto.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Perché flexbox?

Il layout a scatola flessibile CSS ti consente di:

- Centrare verticalmente un blocco di contenuto all'interno del suo genitore.
- Fare in modo che tutti i figli di un contenitore occupino una quantità uguale di larghezza/altezza disponibile, indipendentemente da quanta larghezza/altezza sia disponibile.
- Fare in modo che tutte le colonne in un layout a più colonne adottino la stessa altezza anche se contengono una quantità diversa di contenuto.

Le caratteristiche di Flexbox possono essere la soluzione perfetta per le tue esigenze di layout unidimensionale. Esploriamo insieme per scoprire di più!

> [!NOTE]
> Lo [scrim su Flexbox di Scrimba](https://scrimba.com/learn-html-and-css-c0p/~017?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> offre una guida interattiva che copre quanto il flexbox sia comune sul web e quindi perché sia così importante da imparare, e ti guida attraverso un caso d'uso tipico che dimostra la potenza del flexbox.

## Introduzione a un semplice esempio

In questo articolo, lavorerai attraverso una serie di esercizi per aiutarti a comprendere come funziona il flexbox. Per iniziare, dovresti fare una copia locale dell'HTML e del CSS. Caricalo in un browser moderno (come Firefox o Chrome) e dai un'occhiata al codice nel tuo editor di codice. In alternativa, apri l'esempio in {{LiveSampleLink("flexbox_0", "open the playground")}}.

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

Vedrai che abbiamo un elemento {{htmlelement("header")}} con un'intestazione di primo livello all'interno e un elemento {{htmlelement("section")}} contenente tre {{htmlelement("article")}}. Useremo questi per creare un layout a tre colonne piuttosto standard.

## Specificare quali elementi disporre come box flessibili

Per iniziare, dobbiamo selezionare quali elementi devono essere disposti come box flessibili. Per fare ciò, impostiamo un valore speciale di {{cssxref("display")}} sull'elemento padre degli elementi che vuoi influenzare. In questo caso vogliamo disporre gli elementi {{htmlelement("article")}}, quindi impostiamo questo sul {{htmlelement("section")}}:

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

Ciò fa sì che l'elemento `<section>` diventi un **contenitore flex** e i suoi figli diventino **elementi flex**. Questo è come appare:

{{EmbedLiveSample("flexbox_1", "100", "210")}}

Questa singola dichiarazione ci dà tutto ciò di cui abbiamo bisogno. Incredibile, vero? Abbiamo un layout a colonne multiple con colonne di dimensioni uguali, e tutte le colonne hanno la stessa altezza. Questo perché i valori predefiniti assegnati agli elementi flex (i figli del contenitore flex) sono configurati per risolvere problemi comuni come questo.

Riepiloghiamo cosa sta succedendo qui. Aggiungere un valore di {{cssxref("display")}} di `flex` a un elemento lo rende un contenitore flex. Il contenitore viene visualizzato come {{Glossary("Block-level_content", "contenuto di livello blocco")}} in termini di interazione con il resto della pagina. Quando l'elemento viene convertito in un contenitore flex, i suoi figli vengono convertiti e disposti come elementi flex.

Puoi rendere il contenitore inline usando un [valore `display` esterno](/it/docs/Web/CSS/display#outside) (es., `display: inline flex`), che influisce su come il contenitore stesso è disposto nella pagina.
Il valore di display legacy `inline-flex` visualizza il contenitore anche come inline.
In questo tutorial ci concentreremo su come si comportano i contenuti del contenitore, ma se vuoi vedere l'effetto del layout inline rispetto a quello block, puoi dare un'occhiata alla [confronto dei valori](/it/docs/Web/CSS/display#display_value_comparison) sulla pagina della proprietà `display`.

Le sezioni successive spiegano in dettaglio cosa sono gli elementi flex e cosa accade all'interno di un elemento quando lo rendi un contenitore flex.

## Il modello flex

Quando gli elementi sono disposti come elementi flex, sono disposti lungo due assi:

![Tre elementi flex in una lingua da sinistra a destra sono disposti uno accanto all'altro in un contenitore flex. L'asse principale — l'asse del contenitore flex nella direzione in cui gli elementi flex sono disposti — è orizzontale. Gli estremi dell'asse sono l'inizio principale e la fine principale e sono rispettivamente a sinistra e a destra. L'asse trasversale è verticale; perpendicolare all'asse principale. L'inizio e la fine trasversale sono in alto e in basso rispettivamente. La lunghezza dell'elemento flex lungo l'asse principale, in questo caso, la larghezza, è chiamata dimensione principale, e la lunghezza dell'elemento flex lungo l'asse trasversale, in questo caso, l'altezza, è chiamata dimensione trasversale.](flex_terms.png)

- L'**asse principale** è l'asse che corre nella direzione in cui gli elementi flex sono disposti (ad esempio, come una riga attraverso la pagina, o una colonna giù per la pagina). L'inizio e la fine di questo asse sono chiamati **inizio principale** e **fine principale**. La lunghezza dal bordo di inizio principale al bordo di fine principale è la **dimensione principale**.
- L'**asse trasversale** è l'asse che corre perpendicolarmente alla direzione in cui gli elementi flex sono disposti. L'inizio e la fine di questo asse sono chiamati **inizio trasversale** e **fine trasversale**. La lunghezza dal bordo di inizio trasversale al bordo di fine trasversale è la **dimensione trasversale**.
- L'elemento genitore che ha impostato `display: flex` su di esso (il {{htmlelement("section")}} nel nostro esempio) è chiamato il **contenitore flex**.
- Gli elementi disposti come box flessibili all'interno del contenitore flex sono chiamati **elementi flex** (gli elementi {{htmlelement("article")}} nel nostro esempio).

Tieni a mente questa terminologia mentre prosegui nelle sezioni successive. Puoi sempre fare riferimento a essa se sei confuso su uno qualsiasi dei termini utilizzati.

## Colonne o righe?

Flexbox fornisce una proprietà chiamata {{cssxref("flex-direction")}} che specifica in quale direzione corre l'asse principale (in quale direzione sono disposti i figli del flexbox). Per impostazione predefinita, è impostato su `row`, il che li fa disporre in una riga nella direzione in cui lavora la lingua predefinita del tuo browser (da sinistra a destra, nel caso di un browser in inglese).

Prova ad aggiungere la seguente dichiarazione alla tua regola {{htmlelement("section")}}:

```css
flex-direction: column;
```

Vedrai che questo mette gli elementi di nuovo in un layout a colonne, molto simile a come erano prima che aggiungessimo qualsiasi CSS. Prima di andare avanti, elimina questa dichiarazione dal tuo esempio.

> [!NOTE]
> Puoi anche disporre gli elementi flex in una direzione inversa usando i valori `row-reverse` e `column-reverse`. Sperimenta anche con questi valori!

## Avvolgimento

Un problema che si presenta quando hai un'altezza o una larghezza fissa nel tuo layout è che alla fine i tuoi figli del flexbox usciranno dal loro contenitore, rompendo il layout. Nell'esempio seguente abbiamo 5 {{htmlelement("article")}}, che non si adattano, perché hanno una `min-width` di `400px`, quindi c'è uno scorrimento orizzontale.

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

Qui vediamo che i figli stanno effettivamente uscendo dal loro contenitore. Per impostazione predefinita, il browser prova a posizionare tutti gli elementi flex in una singola riga se la `flex-direction` è impostata su `row` o in una singola colonna se la `flex-direction` è impostata su `column`.

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

Un modo per risolvere questo è aggiungere la seguente dichiarazione alla tua regola {{htmlelement("section")}}:

```css live-sample___flex-wrap_1
section {
  flex-wrap: wrap;
}
```

Vedrai che il layout appare molto meglio con questo incluso:

{{EmbedLiveSample("flex-wrap_1", "100", "430")}}

Ora abbiamo più righe. Ogni riga ha tanti figli flexbox inseriti in essa quanto sia sensato. Qualsiasi trabocco viene spostato verso il basso alla riga successiva.

Ma c'è di più che possiamo fare qui. Prima di tutto, prova a cambiare il valore della tua proprietà {{cssxref("flex-direction")}} in `row-reverse`. Ora vedrai che hai ancora il tuo layout a più righe, ma parte dall'angolo opposto della finestra del browser e fluisce al contrario.

## Abbreviazione flex-flow

A questo punto vale la pena notare che esiste un'abbreviazione per {{cssxref("flex-direction")}} e {{cssxref("flex-wrap")}}: {{cssxref("flex-flow")}}. Quindi, ad esempio, puoi sostituire

```css
flex-direction: row;
flex-wrap: wrap;
```

con

```css
flex-flow: row wrap;
```

## Dimensionamento flessibile degli elementi flex

Torniamo ora al nostro primo esempio e vediamo come possiamo controllare quale proporzione di spazio occupano gli elementi flex rispetto agli altri elementi flex.

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

Nella tua copia locale, aggiungi la seguente regola alla fine del tuo CSS:

```css live-sample___flexbox_2
article {
  flex: 1;
}
```

{{EmbedLiveSample("flexbox_2", "100", "210")}}

Questo è un valore di proporzione senza unità che regola quanto spazio disponibile lungo l'asse principale ogni elemento flex occuperà rispetto ad altri elementi flex. In questo caso, stiamo dando a ciascun elemento {{htmlelement("article")}} lo stesso valore (un valore di "1"), il che significa che tutti occuperanno una quantità uguale dello spazio rimanente dopo che proprietà come padding e margin sono state impostate. Questo valore è condiviso proporzionalmente tra gli elementi flex: dare a ciascun elemento flex un valore di `400000` avrebbe esattamente lo stesso effetto.

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

Ora aggiungi la seguente regola sotto la precedente:

```css live-sample___flexbox_3
article:nth-of-type(3) {
  flex: 2;
}
```

{{EmbedLiveSample("flexbox_3", "100", "210")}}

Ora, dopo aver aggiornato, vedrai che il terzo {{htmlelement("article")}} occupa il doppio dello spazio disponibile rispetto agli altri due. Ora ci sono quattro unità di proporzione disponibili in totale (poiché 1 + 1 + 2 = 4). I primi due elementi flex hanno una unità ciascuno, quindi ciascuno occupa 1/4 dello spazio disponibile. Il terzo ha due unità, quindi occupa 2/4 dello spazio disponibile (o metà).

Puoi anche specificare un valore di dimensione minima all'interno del valore flex. Prova ad aggiornare le tue regole esistenti per gli articoli come segue:

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

Questo essenzialmente dice: "A ciascun elemento flex verranno dati prima '100px' di spazio disponibile. Dopo di ciò, il restante spazio disponibile verrà condiviso secondo le unità proporzionali." Vedrai una differenza in come viene condiviso lo spazio.

{{EmbedLiveSample("flexbox_4", "100", "210")}}

Tutti gli elementi flex hanno una larghezza minima di 100 pixel — impostata usando 'flex'. Il valore di flex per i primi due elementi flex è 1 e per il terzo elemento è 2. Questo divide il restante spazio nel contenitore flex in 4 unità proporzionali. Una unità viene assegnata ai primi due elementi flex e 2 unità vengono assegnate al terzo elemento flex, rendendo il terzo elemento flex più largo rispetto agli altri due, che sono della stessa larghezza.

Il vero valore del flexbox può essere visto nella sua flessibilità/reattività. Se ridimensioni la finestra del browser o aggiungi un altro elemento {{htmlelement("article")}}, il layout continua a funzionare perfettamente.

## flex: abbreviazione vs lunga

{{cssxref("flex")}} è una proprietà abbreviazione che può specificare fino a tre valori diversi:

- Il valore di proporzione senza unità di cui abbiamo discusso sopra. Questo può essere specificato separatamente usando la proprietà lunga {{cssxref("flex-grow")}}.
- Un secondo valore di proporzione senza unità, {{cssxref("flex-shrink")}}, che entra in gioco quando gli elementi flex traboccano dal loro contenitore. Questo valore specifica di quanto un elemento si ridurrà per evitare il trabocco. Questa è una caratteristica piuttosto avanzata del flexbox e non la tratteremo ulteriormente in questo articolo.
- Il valore di dimensione minima di cui abbiamo discusso sopra. Questo può essere specificato separatamente usando il valore lungo {{cssxref("flex-basis")}}.

Consigliamo di non utilizzare le proprietà lunghe del flex a meno che non sia strettamente necessario (ad esempio, per sovrascrivere qualcosa impostato in precedenza). Portano a molto codice extra da scrivere e possono risultare piuttosto confusi.

## Allineamento orizzontale e verticale

Puoi anche utilizzare le caratteristiche del flexbox per allineare gli elementi flex lungo l'asse principale o trasversale. Esploriamo questo concetto guardando un nuovo esempio:

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

Stiamo per trasformare questo in un elegante e flessibile pulsante/barra degli strumenti. Al momento vedrai una barra di menu orizzontale con alcuni pulsanti ammucchiati nell'angolo in alto a sinistra.

{{EmbedLiveSample("flex-align_0", "100", "125")}}

Prima di tutto, prendi una copia locale di questo esempio.

Ora, aggiungi quanto segue alla fine del CSS dell'esempio:

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

Aggiorna la pagina e vedrai che i pulsanti sono ora ben centrati orizzontalmente e verticalmente. Lo abbiamo fatto tramite due nuove proprietà. Gli elementi flex sono posizionati al centro dell'asse trasversale impostando la proprietà `align-items` su `center`. Gli elementi flex sono spaziati uniformemente lungo l'asse principale impostando la proprietà `justify-content` su `space-around`.

La proprietà {{cssxref("align-items")}} controlla dove si posizionano gli elementi flex sull'asse trasversale.

- Di default, il valore `normal` che si comporta come `stretch` nel flexbox. Questo estende tutti gli elementi flex per riempire il genitore nella direzione dell'asse trasversale. Se il genitore non ha una dimensione fissa nella direzione dell'asse trasversale, allora tutti gli elementi flex diventeranno alti (o larghi) quanto l'elemento flex più alto (o più largo). È così che il nostro primo esempio ha avuto colonne di altezza uguale per impostazione predefinita.
- Il valore `center` che abbiamo utilizzato nel nostro codice sopra fa sì che gli elementi mantengano le loro dimensioni intrinseche, ma siano centrati lungo l'asse trasversale. Questo è il motivo per cui i pulsanti dell'esempio attuale sono centrati verticalmente.
- Puoi avere anche valori come `flex-start`, `self-start` o `start` e `flex-end`, `self-end` o `end`, che allineeranno tutti gli elementi all'inizio e alla fine dell'asse trasversale rispettivamente. I valori `baseline` allineeranno gli elementi flex secondo la loro baseline; essenzialmente il fondo di ciascuna riga di testo del primo elemento flex sarà allineato con il fondo della riga dell'elemento con la maggiore distanza tra l'inizio trasversale e quella baseline. Vedi {{cssxref("align-items")}} per i dettagli completi.

Puoi sovrascrivere il comportamento di {{cssxref("align-items")}} per singoli elementi flex applicando la proprietà {{cssxref("align-self")}} a essi. Ad esempio, prova ad aggiungere quanto segue al tuo CSS:

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

Dai un'occhiata all'effetto e rimuovi nuovamente le modifiche quando hai terminato.

{{cssxref("justify-content")}} controlla dove si posizionano gli elementi flex sull'asse principale.

- Il valore predefinito è `normal`, che si comporta come `start`, il che fa sì che tutti gli elementi si posizionino all'inizio dell'asse principale.
- Puoi usare `end` o `flex-end` per farli posizionare alla fine.
- I valori `left` e `right` si comportano come `start` o `end` a seconda della direzione del writing mode.
- `center` è anche un valore di `justify-content`. Farà in modo che gli elementi flex si posizionino al centro dell'asse principale.
- Il valore che abbiamo usato sopra, `space-around`, è utile — distribuisce tutti gli elementi uniformemente lungo l'asse principale con un po' di spazio lasciato a ciascuna estremità.
- C'è un altro valore, `space-between`, che è molto simile a `space-around` tranne per il fatto che non lascia spazio a ciascuna estremità.

La proprietà [`justify-items`](/it/docs/Web/CSS/justify-items) viene ignorata nei layout flexbox.

Vorremmo incoraggiarti a giocare con questi valori per vedere come funzionano prima di continuare.

## Ordinare gli elementi flex

Flexbox ha anche una funzione per cambiare l'ordine di layout degli elementi flex senza influenzare l'ordine sorgente. Questo è un altro aspetto impossibile da fare con i metodi di layout tradizionali.

Prova ad aggiungere il seguente CSS al codice del tuo esempio della barra dei pulsanti:

```css
button:first-child {
  order: 1;
}
```

Aggiorna e vedrai che il pulsante "Smile" si è spostato alla fine dell'asse principale. Parliamo di come funziona questo nei dettagli:

- Di default, tutti gli elementi flex hanno un valore di {{cssxref("order")}} di `0`.
- Gli elementi flex con valori di ordine specificati più alti appariranno più tardi nell'ordine di visualizzazione rispetto agli elementi con valori di ordine più bassi.
- Gli elementi flex con lo stesso valore di ordine appariranno nel loro ordine sorgente. Quindi, se hai quattro elementi i cui valori di ordine sono stati impostati come `2`, `1`, `1` e `0` rispettivamente, il loro ordine di visualizzazione sarebbe 4°, 2°, 3°, quindi 1°.
- Il 3° elemento appare dopo il 2° perché ha lo stesso valore di ordine ed è dopo di esso nell'ordine sorgente.

Puoi impostare valori di ordine negativi per fare apparire gli elementi prima di quelli il cui valore è `0`. Ad esempio, potresti far apparire il pulsante "Blush" all'inizio dell'asse principale utilizzando la seguente regola:

```css
button:last-child {
  order: -1;
}
```

Mentre puoi cambiare l'ordine usando `order`, l'ordine di tabulazione rimane lo stesso dell'ordine del codice. Cambiare l'ordine degli elementi focalizzabili può avere un impatto negativo sull'usabilità per gli utenti che usano la tastiera!

## Flex boxes annidati

È possibile creare alcuni layout piuttosto complessi con il flexbox. È perfettamente lecito impostare un elemento flex come un contenitore flex, in modo che i suoi figli siano anch'essi disposti come box flessibili.

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

Questo layout complesso ha alcuni elementi flex che sono anche contenitori flex. L'HTML per questo è abbastanza lineare. Abbiamo un elemento {{htmlelement("section")}} contenente tre {{htmlelement("article")}}. Il terzo {{htmlelement("article")}} contiene tre {{htmlelement("div")}}, e il primo {{htmlelement("div")}} contiene cinque {{htmlelement("button")}}:

```plain
section - article
          article
          article - div - button
                    div   button
                    div   button
                          button
                          button
```

Diamo uno sguardo al codice che abbiamo utilizzato per il layout.

Innanzitutto, impostiamo i figli del {{htmlelement("section")}} per essere disposti come box flessibili.

```css
section {
  display: flex;
}
```

Poi, impostiamo alcuni valori flex sugli {{htmlelement("article")}} stessi. Nota in particolare la seconda regola qui: stiamo impostando il terzo {{htmlelement("article")}} in modo che i suoi figli siano disposti come elementi flex anche loro, ma questa volta li stiamo disponendo come una colonna.

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

Successivamente, selezioniamo il primo {{htmlelement("div")}}. Usiamo prima `flex: 1 100px;` per dargli effettivamente un'altezza minima di `100px`, poi impostiamo i suoi figli (gli elementi {{htmlelement("button")}}) per essere anch'essi disposti come elementi flex. Qui li disponiamo in una riga avvolgente e li allineiamo al centro dello spazio disponibile come abbiamo fatto con l'esempio del singolo pulsante che abbiamo visto prima.

```css
article:nth-of-type(3) div:first-child {
  flex: 1 100px;
  display: flex;
  flex-flow: row wrap;
  align-items: center;
  justify-content: space-around;
}
```

Infine, impostiamo alcune dimensioni sui pulsanti. Questa volta dando un valore di flex di `1 auto`. Questo ha un effetto molto interessante, che vedrai se provi a ridimensionare la larghezza della finestra del browser. I pulsanti occuperanno il maggior spazio possibile. Molti si adatteranno su una linea quanto sia confortevole; oltre ciò, cadranno su una nuova riga.

```css
button {
  flex: 1 auto;
  margin: 5px;
  font-size: 18px;
  line-height: 1.5;
}
```

## Metti alla prova le tue competenze!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare se hai conservato queste informazioni prima di procedere — vedi [Metti alla prova le tue competenze: Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Test_your_skills/Flexbox).

## Sommario

Questo conclude il nostro tour dei fondamenti del flexbox. Speriamo che ti sia divertito e che giocherai un po' con esso mentre prosegui con il tuo apprendimento. Successivamente, esamineremo un altro aspetto importante dei layout CSS: [griglie CSS](/it/docs/Learn_web_development/Core/CSS_layout/Grids).

## Vedi anche

- [Concetti base del flexbox](/it/docs/Web/CSS/CSS_flexible_box_layout/Basic_concepts_of_flexbox)
- [Allineare gli elementi in un contenitore flex](/it/docs/Web/CSS/CSS_flexible_box_layout/Aligning_items_in_a_flex_container)
- [Ordinare gli elementi flex](/it/docs/Web/CSS/CSS_flexible_box_layout/Ordering_flex_items)
- [Controllare le proporzioni degli elementi flex lungo l'asse principale](/it/docs/Web/CSS/CSS_flexible_box_layout/Controlling_ratios_of_flex_items_along_the_main_axis)
- Modulo [Layout a scatola flessibile CSS](/it/docs/Web/CSS/CSS_flexible_box_layout)
- [Guida al flexbox di CSS-Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) — un articolo che spiega tutto sul flexbox in modo visivamente accattivante
- [Flexbox Froggy](https://flexboxfroggy.com/) — un gioco educativo per imparare e comprendere meglio i fondamenti del flexbox

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Positioning", "Learn_web_development/Core/CSS_layout/Grids", "Learn_web_development/Core/CSS_layout")}}
