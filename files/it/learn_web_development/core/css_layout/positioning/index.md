---
title: Posizionamento
slug: Learn_web_development/Core/CSS_layout/Positioning
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Floats", "Learn_web_development/Core/CSS_layout/Flexbox", "Learn_web_development/Core/CSS_layout")}}

Il posizionamento consente di togliere gli elementi dal normale flusso del documento e farli comportare diversamente, per esempio sovrapporsi l'uno all'altro o rimanere sempre nella stessa posizione all'interno del viewport del browser. Questo articolo spiega i diversi valori di {{cssxref("position")}} e come utilizzarli.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare il contenuto con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base su CSS Styling</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Fondamenti di stilizzazione del testo e dei font</a>,
        familiarità con i <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">concetti fondamentali del layout CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Il posizionamento <code>static</code> è il modo predefinito in cui gli elementi sono posizionati nella pagina.</li>
          <li>Gli elementi posizionati relativamente rimangono nel flusso normale, ma il posizionamento assoluto (e fisso/in adesione) toglie gli elementi completamente dal flusso normale per collocarli su un livello separato.</li>
          <li>La posizione finale del layout può essere modificata usando le proprietà <code>top</code>, <code>bottom</code>, <code>left</code> e <code>right</code>, ma queste hanno effetti diversi a seconda del valore di <code>position</code> impostato.</li>
          <li>Impostare il contesto di posizionamento di un elemento posizionato posizionando un elemento antenato.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Le consigliamo di svolgere i seguenti esercizi sul suo computer locale. Se possibile, scarichi una copia di [`0_basic-flow.html`](https://mdn.github.io/learning-area/css/css-layout/positioning/0_basic-flow.html) dal nostro repository GitHub ([codice sorgente qui](https://github.com/mdn/learning-area/blob/main/css/css-layout/positioning/0_basic-flow.html)) e lo usi come punto di partenza.

## Introduzione al posizionamento

Il posizionamento ci permette di ottenere risultati interessanti superando il normale flusso del documento. Cosa succede se vuole alterare leggermente la posizione di alcune scatole dalla loro posizione di flusso predefinita per dare un tocco leggermente eccentrico? Il posizionamento è il suo strumento. Oppure, cosa fare se desidera creare un elemento UI che fluttui sopra le altre parti della pagina e/o rimanga sempre nello stesso posto all'interno della finestra del browser indipendentemente dallo scorrimento della pagina? Il posizionamento rende possibile questo tipo di layout.

Ci sono diversi tipi di posizionamento che si possono applicare sugli elementi HTML. Per attivare un tipo specifico di posizionamento su un elemento, usiamo la proprietà {{cssxref("position")}}.

## Posizionamento statico

Il posizionamento statico è il valore predefinito che ogni elemento ottiene. Significa semplicemente "mettere l'elemento nella sua posizione predefinita nel flusso normale — niente di speciale da vedere qui."

Per vedere questo (e impostare il suo esempio per le sezioni future) aggiunga prima una `class` di `positioned` al secondo {{htmlelement("p")}} nell'HTML:

```html
<p class="positioned">…</p>
```

Ora aggiunga la seguente regola alla fine del suo CSS:

```css
.positioned {
  position: static;
  background: yellow;
}
```

Se salva e aggiorna, non vedrà alcuna differenza, tranne per il colore di sfondo aggiornato del secondo paragrafo. Questo è normale — come abbiamo detto prima, il posizionamento statico è il comportamento predefinito!

> [!NOTE]
> Può vedere l'esempio a questo punto in diretta su [`1_static-positioning.html`](https://mdn.github.io/learning-area/css/css-layout/positioning/1_static-positioning.html) ([vedere codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/positioning/1_static-positioning.html)).

## Posizionamento relativo

Il posizionamento relativo è il primo tipo di posizione che esamineremo. Questo è molto simile al posizionamento statico, eccetto che una volta che l'elemento posizionato ha preso il suo posto nel flusso normale, può poi modificare la sua posizione finale, incluso farlo sovrapporre ad altri elementi della pagina. Aggiorni la dichiarazione `position` nel suo codice:

```css
position: relative;
```

Se salva e aggiorna in questa fase, non vedrà alcun cambiamento nel risultato. Quindi come si modifica la posizione dell'elemento? Deve usare le proprietà {{cssxref("top")}}, {{cssxref("bottom")}}, {{cssxref("left")}}, e {{cssxref("right")}}, che spiegheremo nella prossima sezione.

### Introduzione a top, bottom, left, e right

{{cssxref("top")}}, {{cssxref("bottom")}}, {{cssxref("left")}}, e {{cssxref("right")}} sono usati insieme a {{cssxref("position")}} per specificare esattamente dove spostare l'elemento posizionato. Per provare questo, aggiunga le seguenti dichiarazioni alla regola `.positioned` nel suo CSS:

```css
top: 30px;
left: 30px;
```

> [!NOTE]
> I valori di queste proprietà possono prendere qualsiasi [unità](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units) che si aspetterebbe ragionevolmente: pixel, mm, rem, %, ecc.

Se ora salva e aggiorna, otterrà un risultato simile a questo:

```html hidden
<h1>Relative positioning</h1>

<p>
  I am a basic block level element. My adjacent block level elements sit on new
  lines below me.
</p>

<p class="positioned">
  By default we span 100% of the width of our parent element, and we are as tall
  as our child content. Our total width and height is our content + padding +
  border width/height.
</p>

<p>
  We are separated by our margins. Because of margin collapsing, we are
  separated by the width of one of our margins, not both.
</p>

<p>
  Inline elements <span>like this one</span> and <span>this one</span> sit on
  the same line as one another, and adjacent text nodes, if there is space on
  the same line. Overflowing inline elements
  <span>wrap onto a new line if possible — like this one containing text</span>,
  or just go on to a new line if not, much like this image will do:
  <img
    src="https://mdn.github.io/shared-assets/images/examples/long.jpg"
    alt="snippet of cloth" />
</p>
```

```css hidden
body {
  width: 500px;
  margin: 0 auto;
}

p {
  background: aqua;
  border: 3px solid blue;
  padding: 10px;
  margin: 10px;
}

span {
  background: red;
  border: 1px solid black;
}

.positioned {
  position: relative;
  background: yellow;
  top: 30px;
  left: 30px;
}
```

{{ EmbedLiveSample('Introducing_top_bottom_left_and_right', '100%', 500) }}

Fantastico, vero? Ok, quindi probabilmente non era quello che si aspettava. Perché è stato spostato verso il basso e a destra se abbiamo specificato _top_ e _left_? Questo può sembrare controintuitivo. Deve pensarla come se ci fosse una forza invisibile che spinge il lato specificato della scatola posizionata, spostandola nella direzione opposta. Quindi, ad esempio, se specifica `top: 30px;`, è come se una forza spingesse la parte superiore della scatola, facendola spostare verso il basso di 30px.

> [!NOTE]
> Può vedere l'esempio a questo punto in diretta su [`2_relative-positioning.html`](https://mdn.github.io/learning-area/css/css-layout/positioning/2_relative-positioning.html) ([vedere codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/positioning/2_relative-positioning.html)).

## Posizionamento assoluto

Il posizionamento assoluto porta a risultati molto diversi.

### Impostare position: absolute

Provi a cambiare la dichiarazione di posizione nel suo codice come segue:

```css
position: absolute;
```

Se ora salva e aggiorna, dovrebbe vedere qualcosa del genere:

```html hidden
<h1>Absolute positioning</h1>

<p>
  I am a basic block level element. My adjacent block level elements sit on new
  lines below me.
</p>

<p class="positioned">
  By default we span 100% of the width of our parent element, and we are as tall
  as our child content. Our total width and height is our content + padding +
  border width/height.
</p>

<p>
  We are separated by our margins. Because of margin collapsing, we are
  separated by the width of one of our margins, not both.
</p>

<p>
  inline elements <span>like this one</span> and <span>this one</span> sit on
  the same line as one another, and adjacent text nodes, if there is space on
  the same line. Overflowing inline elements
  <span>wrap onto a new line if possible — like this one containing text</span>,
  or just go on to a new line if not, much like this image will do:
  <img
    src="https://mdn.github.io/shared-assets/images/examples/long.jpg"
    alt="snippet of cloth" />
</p>
```

```css hidden
body {
  width: 500px;
  margin: 0 auto;
}

p {
  background: aqua;
  border: 3px solid blue;
  padding: 10px;
  margin: 10px;
}

span {
  background: red;
  border: 1px solid black;
}

.positioned {
  position: absolute;
  background: yellow;
  top: 30px;
  left: 30px;
}
```

{{ EmbedLiveSample('Setting_position_absolute', '100%', 450) }}

Prima di tutto, noti che il divario dove l'elemento posizionato dovrebbe essere nel flusso del documento non c'è più — il primo e il terzo elemento si sono avvicinati come se non esistesse più! Beh, in un certo senso questo è vero. Un elemento posizionato in modo assoluto non esiste più nel flusso normale del documento. Invece, si trova su un proprio livello separato da tutto il resto. Questo è molto utile: significa che possiamo creare funzionalità UI isolate che non interferiscono con il layout di altri elementi sulla pagina. Ad esempio, caselle di informazione a comparsa, menu di controllo, pannelli a comparsa, funzionalità UI che possono essere trascinate e rilasciate ovunque sulla pagina, e così via.

In secondo luogo, noti che la posizione dell'elemento è cambiata. Questo perché {{cssxref("top")}}, {{cssxref("bottom")}}, {{cssxref("left")}}, e {{cssxref("right")}} si comportano in modo diverso con il posizionamento assoluto. Piuttosto che posizionare l'elemento basandosi sulla sua posizione relativa nel flusso normale del documento, specificano la distanza che l'elemento dovrebbe avere dai lati dell'elemento contenitore. In questo caso, stiamo dicendo che l'elemento posizionato in modo assoluto dovrebbe trovarsi a 30px dalla parte superiore dell'**elemento contenitore** (ovvero il **blocco contenitore iniziale**, in questo caso) e a 30px dalla sinistra.

> [!NOTE]
> Può usare {{cssxref("top")}}, {{cssxref("bottom")}}, {{cssxref("left")}}, e {{cssxref("right")}} per ridimensionare gli elementi se necessario. Provi a impostare `top: 0; bottom: 0; left: 0; right: 0;` e `margin: 0;` sui suoi elementi posizionati e veda cosa succede! Ripristini il tutto successivamente...

> [!NOTE]
> Sì, i margini continuano ad influenzare gli elementi posizionati. Il collasso dei margini non lo fa, però.

> [!NOTE]
> Può vedere l'esempio a questo punto in diretta su [`3_absolute-positioning.html`](https://mdn.github.io/learning-area/css/css-layout/positioning/3_absolute-positioning.html) ([vedere codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/positioning/3_absolute-positioning.html)).

### Contesti di posizionamento

Quale elemento è "l'elemento contenitore" di un elemento posizionato in modo assoluto? Questo dipende molto dal valore della proprietà `position` degli antenati dell'elemento posizionato.

Se nessun elemento antenato ha la sua proprietà di posizione definita esplicitamente, allora per impostazione predefinita tutti gli elementi antenati avranno un posizionamento statico. Il risultato di questo è che l'elemento posizionato in modo assoluto sarà contenuto nel **blocco contenitore iniziale**. Il blocco contenitore iniziale ha le dimensioni del viewport ed è anche il blocco che contiene l'elemento {{htmlelement("html")}}. In altre parole, l'elemento posizionato in modo assoluto sarà visualizzato all'esterno dell'elemento {{htmlelement("html")}} e posizionato rispetto al viewport iniziale.

L'elemento posizionato è annidato all'interno del {{htmlelement("body")}} nel sorgente HTML, ma nel layout finale si trova a 30px dai bordi superiore e sinistro della pagina. Possiamo cambiare il **contesto di posizionamento**, cioè a quale elemento l'elemento posizionato in modo assoluto è relativo. Questo si fa impostando il posizionamento su uno degli antenati dell'elemento: a uno degli elementi in cui è annidato (non può posizionarlo in relazione a un elemento in cui non è annidato). Per vedere questo, aggiunga la seguente dichiarazione alla regola `body`:

```css
position: relative;
```

Questo dovrebbe dare il seguente risultato:

```html hidden
<h1>Positioning context</h1>

<p>
  I am a basic block level element. My adjacent block level elements sit on new
  lines below me.
</p>

<p class="positioned">
  Now I'm absolutely positioned relative to the
  <code>&lt;body&gt;</code> element, not the <code>&lt;html&gt;</code> element!
</p>

<p>
  We are separated by our margins. Because of margin collapsing, we are
  separated by the width of one of our margins, not both.
</p>

<p>
  inline elements <span>like this one</span> and <span>this one</span> sit on
  the same line as one another, and adjacent text nodes, if there is space on
  the same line. Overflowing inline elements
  <span>wrap onto a new line if possible — like this one containing text</span>,
  or just go on to a new line if not, much like this image will do:
  <img
    src="https://mdn.github.io/shared-assets/images/examples/long.jpg"
    alt="snippet of cloth" />
</p>
```

```css hidden
body {
  width: 500px;
  margin: 0 auto;
  position: relative;
}

p {
  background: aqua;
  border: 3px solid blue;
  padding: 10px;
  margin: 10px;
}

span {
  background: red;
  border: 1px solid black;
}

.positioned {
  position: absolute;
  background: yellow;
  top: 30px;
  left: 30px;
}
```

{{ EmbedLiveSample('Positioning_contexts', '100%', 420) }}

Ora l'elemento posizionato si trova relativamente all'elemento {{htmlelement("body")}}.

> [!NOTE]
> Può vedere l'esempio a questo punto in diretta su [`4_positioning-context.html`](https://mdn.github.io/learning-area/css/css-layout/positioning/4_positioning-context.html) ([vedere codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/positioning/4_positioning-context.html)).

### Introduzione a z-index

Tutto questo posizionamento assoluto è divertente, ma c'è un'altra caratteristica che non abbiamo considerato ancora. Quando gli elementi iniziano a sovrapporsi, cosa determina quali elementi appaiono sopra gli altri e quali elementi appaiono sotto? Nell'esempio che abbiamo visto finora, abbiamo un solo elemento posizionato nel contesto di posizionamento, e appare in cima poiché gli elementi posizionati prevalgono su quelli non posizionati. Che dire quando abbiamo più di uno?

Provi ad aggiungere quanto segue al suo CSS per rendere anche il primo paragrafo posizionato in modo assoluto:

```css
p:nth-of-type(1) {
  position: absolute;
  background: lime;
  top: 10px;
  right: 30px;
}
```

A questo punto vedrà il primo paragrafo color lime, spostato fuori dal flusso del documento e posizionato un po' sopra rispetto a dove era originariamente. È anche impilato sotto il paragrafo `.positioned` originale dove i due si sovrappongono. Questo perché il paragrafo `.positioned` è il secondo paragrafo nell'ordine sorgente, e gli elementi posizionati più tardi nell'ordine sorgente prevalgono su quelli posizionati prima.

Può cambiare l'ordine di impilamento? Sì, può, usando la proprietà {{cssxref("z-index")}}. "z-index" è un riferimento all'asse z. Potrebbe ricordare dai punti precedenti nel corso dove abbiamo discusso le pagine web che utilizzano coordinate orizzontali (asse x) e verticali (asse y) per calcolare il posizionamento per cose come immagini di sfondo e offset delle ombre. Per le lingue che scorrono da sinistra a destra, (0,0) si trova in alto a sinistra della pagina (o dell'elemento), e gli assi x e y corrono verso destra e verso il basso nella pagina.

Le pagine web hanno anche un asse z: una linea immaginaria che corre dalla superficie del suo schermo verso la sua faccia (o quello che preferisce avere davanti allo schermo). I valori di {{cssxref("z-index")}} influenzano dove gli elementi posizionati si trovano su quell'asse; valori positivi li muovono più in alto nella pila, valori negativi li spostano più in basso. Di default, tutti gli elementi posizionati hanno un `z-index` di `auto`, che è effettivamente 0.

Per cambiare l'ordine di impilamento, provi ad aggiungere la seguente dichiarazione alla regola `p:nth-of-type(1)`:

```css
z-index: 1;
```

Dovrebbe ora vedere il paragrafo lime sopra:

```html hidden
<h1>z-index</h1>

<p>
  I am a basic block level element. My adjacent block level elements sit on new
  lines below me.
</p>

<p class="positioned">
  Now I'm absolutely positioned relative to the
  <code>&lt;body&gt;</code> element, not the <code>&lt;html&gt;</code> element!
</p>

<p>
  We are separated by our margins. Because of margin collapsing, we are
  separated by the width of one of our margins, not both.
</p>

<p>
  inline elements <span>like this one</span> and <span>this one</span> sit on
  the same line as one another, and adjacent text nodes, if there is space on
  the same line. Overflowing inline elements
  <span>wrap onto a new line if possible — like this one containing text</span>,
  or just go on to a new line if not, much like this image will do:
  <img
    src="https://mdn.github.io/shared-assets/images/examples/long.jpg"
    alt="snippet of cloth" />
</p>
```

```css hidden
body {
  width: 500px;
  margin: 0 auto;
  position: relative;
}

p {
  background: aqua;
  border: 3px solid blue;
  padding: 10px;
  margin: 10px;
}

span {
  background: red;
  border: 1px solid black;
}

.positioned {
  position: absolute;
  background: yellow;
  top: 30px;
  left: 30px;
}

p:nth-of-type(1) {
  position: absolute;
  background: lime;
  top: 10px;
  right: 30px;
  z-index: 1;
}
```

{{ EmbedLiveSample('Introducing_z-index', '100%', 400) }}

Noti che `z-index` accetta solo valori di indice senza unità; non può specificare che desidera un elemento 23 pixel in alto sull'asse Z — non funziona in questo modo. Valori più alti andranno sopra i valori più bassi e sta a lei decidere quali valori usare. Usare valori di 2 o 3 darebbe lo stesso effetto di valori di 300 o 40000.

> [!NOTE]
> Può vedere un esempio di questo in diretta su [`5_z-index.html`](https://mdn.github.io/learning-area/css/css-layout/positioning/5_z-index.html) ([vedere codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/positioning/5_z-index.html)).

## Posizionamento fisso

Passiamo ora al posizionamento fisso. Questo funziona esattamente come il posizionamento assoluto, con una differenza chiave: mentre il posizionamento assoluto fissa un elemento in posizione rispetto al suo antenato posizionato più vicino (il blocco contenitore iniziale se non ce n'è uno), il **posizionamento fisso** fissa un elemento in posizione rispetto alla porzione visibile del viewport. Questo significa che può creare elementi UI utili che sono fissi in posizione, come menu di navigazione persistenti che sono sempre visibili indipendentemente da quanto viene scorsa la pagina.

Mettiamo insieme un semplice esempio per mostrarle cosa intendiamo. Prima di tutto, cancelli le regole esistenti `p:nth-of-type(1)` e `.positioned` dal suo CSS.

Ora aggiorni la regola `body` per rimuovere la dichiarazione `position: relative;` e aggiungere un'altezza fissa, come segue:

```css
body {
  width: 500px;
  height: 1400px;
  margin: 0 auto;
}
```

Ora daremo all'elemento {{htmlelement("Heading_Elements", "&lt;h1>")}} `position: fixed;` e lo faremo stare in alto rispetto al viewport. Aggiunga la seguente regola al suo CSS:

```css
h1 {
  position: fixed;
  top: 0;
  width: 500px;
  margin-top: 0;
  background: white;
  padding: 10px;
}
```

Il `top: 0;` è necessario per farlo aderire alla parte superiore dello schermo. Diamo all'intestazione la stessa larghezza della colonna del contenuto e quindi uno sfondo bianco con un po' di padding e margine in modo che il contenuto non sia visibile al di sotto di esso.

Se salva e aggiorna, vedrà un piccolo effetto divertente dell'intestazione che rimane fissa — il contenuto sembra scorrere verso l'alto e sparire sotto di esso. Ma noti come parte del contenuto inizialmente sia ritagliata sotto l'intestazione. Questo perché l'intestazione posizionata non appare più nel flusso del documento, così il resto del contenuto si sposta verso l'alto. Potremmo migliorarlo spostando tutti i paragrafi un po' in basso. Possiamo farlo impostando del margine superiore sul primo paragrafo. Lo aggiunga ora:

```css
p:nth-of-type(1) {
  margin-top: 60px;
}
```

Ora dovrebbe vedere l'esempio finito:

```html hidden
<h1>Fixed positioning</h1>

<p>
  I am a basic block level element. My adjacent block level elements sit on new
  lines below me.
</p>

<p class="positioned">I'm not positioned any more.</p>

<p>
  We are separated by our margins. Because of margin collapsing, we are
  separated by the width of one of our margins, not both.
</p>

<p>
  Inline elements <span>like this one</span> and <span>this one</span> sit on
  the same line as one another, and adjacent text nodes, if there is space on
  the same line. Overflowing inline elements
  <span>wrap onto a new line if possible — like this one containing text</span>,
  or just go on to a new line if not, much like this image will do:
  <img
    src="https://mdn.github.io/shared-assets/images/examples/long.jpg"
    alt="snippet of cloth" />
</p>
```

```css hidden
body {
  width: 500px;
  height: 1400px;
  margin: 0 auto;
}

p {
  background: aqua;
  border: 3px solid blue;
  padding: 10px;
  margin: 10px;
}

span {
  background: red;
  border: 1px solid black;
}

h1 {
  position: fixed;
  top: 0px;
  width: 500px;
  background: white;
  padding: 10px;
}

p:nth-of-type(1) {
  margin-top: 60px;
}
```

{{ EmbedLiveSample('Fixed_positioning', '100%', 400) }}

> [!NOTE]
> Può vedere un esempio di questo in diretta su [`6_fixed-positioning.html`](https://mdn.github.io/learning-area/css/css-layout/positioning/6_fixed-positioning.html) ([vedere codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/positioning/6_fixed-positioning.html)).

## Posizionamento in adesione

Esiste un altro valore di posizione disponibile chiamato `position: sticky`, che è relativamente più nuovo rispetto agli altri. Questo è fondamentalmente un ibrido tra posizione relativa e fissa. Consente a un elemento posizionato di comportarsi come se fosse relativamente posizionato fino a quando non viene scorretto fino a una certa soglia (per esempio, 10px dalla parte superiore del viewport), dopo di che diventa fisso.

### Esempio di base

Il posizionamento in adesione può essere usato, per esempio, per far sì che una barra di navigazione scorra con la pagina fino a un certo punto e poi si attacchi alla parte superiore della pagina.

```html hidden
<h1>Sticky positioning</h1>

<p>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus aliquam
  dolor, eu lacinia lorem placerat vulputate. Duis felis orci, pulvinar id metus
  ut, rutrum luctus orci. Cras porttitor imperdiet nunc, at ultricies tellus
  laoreet sit amet. Sed auctor cursus massa at porta. Integer ligula ipsum,
  tristique sit amet orci vel, viverra egestas ligula. Curabitur vehicula tellus
  neque, ac ornare ex malesuada et. In vitae convallis lacus. Aliquam erat
  volutpat. Suspendisse ac imperdiet turpis. Aenean finibus sollicitudin eros
  pharetra congue. Duis ornare egestas augue ut luctus. Proin blandit quam nec
  lacus varius commodo et a urna. Ut id ornare felis, eget fermentum sapien.
</p>

<div class="positioned">Sticky</div>

<p>
  Nam vulputate diam nec tempor bibendum. Donec luctus augue eget malesuada
  ultrices. Phasellus turpis est, posuere sit amet dapibus ut, facilisis sed
  est. Nam id risus quis ante semper consectetur eget aliquam lorem. Vivamus
  tristique elit dolor, sed pretium metus suscipit vel. Mauris ultricies lectus
  sed lobortis finibus. Vivamus eu urna eget velit cursus viverra quis
  vestibulum sem. Aliquam tincidunt eget purus in interdum. Cum sociis natoque
  penatibus et magnis dis parturient montes, nascetur ridiculus mus.
</p>

<p>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus aliquam
  dolor, eu lacinia lorem placerat vulputate. Duis felis orci, pulvinar id metus
  ut, rutrum luctus orci. Cras porttitor imperdiet nunc, at ultricies tellus
  laoreet sit amet. Sed auctor cursus massa at porta. Integer ligula ipsum,
  tristique sit amet orci vel, viverra egestas ligula. Curabitur vehicula tellus
  neque, ac ornare ex malesuada et. In vitae convallis lacus. Aliquam erat
  volutpat. Suspendisse ac imperdiet turpis. Aenean finibus sollicitudin eros
  pharetra congue. Duis ornare egestas augue ut luctus. Proin blandit quam nec
  lacus varius commodo et a urna. Ut id ornare felis, eget fermentum sapien.
</p>
```

```css hidden
body {
  width: 500px;
  margin: 0 auto;
}

.positioned {
  background: rgb(255 84 104 / 30%);
  border: 2px solid rgb(255 84 104);
  padding: 10px;
  margin: 10px;
  border-radius: 5px;
}
```

```css
.positioned {
  position: sticky;
  top: 30px;
  left: 30px;
}
```

{{ EmbedLiveSample('Basic_example', '100%', 200) }}

### Indice di scorrimento

Un uso interessante e comune di `position: sticky` è creare una pagina indice a scorrimento in cui diverse intestazioni si attaccano alla parte superiore della pagina man mano che la raggiungono. Il markup per un tale esempio potrebbe apparire così:

```html
<h1>Sticky positioning</h1>

<dl>
  <dt>A</dt>
  <dd>Apple</dd>
  <dd>Ant</dd>
  <dd>Altimeter</dd>
  <dd>Airplane</dd>
  <dt>B</dt>
  <dd>Bird</dd>
  <dd>Buzzard</dd>
  <dd>Bee</dd>
  <dd>Banana</dd>
  <dd>Beanstalk</dd>
  <dt>C</dt>
  <dd>Calculator</dd>
  <dd>Cane</dd>
  <dd>Camera</dd>
  <dd>Camel</dd>
  <dt>D</dt>
  <dd>Duck</dd>
  <dd>Dime</dd>
  <dd>Dipstick</dd>
  <dd>Drone</dd>
  <dt>E</dt>
  <dd>Egg</dd>
  <dd>Elephant</dd>
  <dd>Egret</dd>
</dl>
```

Il CSS potrebbe apparire come segue. Nel flusso normale gli elementi {{htmlelement("dt")}} scorreranno con il contenuto. Quando aggiungiamo `position: sticky` all'elemento {{htmlelement("dt")}}, insieme a un valore {{cssxref("top")}} di 0, i browser supportati attaccheranno le intestazioni alla parte superiore del viewport man mano che raggiungono quella posizione. Ogni intestazione successiva sostituirà quindi quella precedente mentre scorre verso l'alto fino a quella posizione.

```css
dt {
  background-color: black;
  color: white;
  padding: 10px;
  position: sticky;
  top: 0;
  left: 0;
  margin: 1em 0;
}
```

```css hidden
body {
  width: 500px;
  height: 880px;
  margin: 0 auto;
}
```

{{ EmbedLiveSample('Scrolling_index', '100%', 200) }}

Gli elementi in adesione sono "appiccicosi" relativamente all'antenato più vicino con un "meccanismo di scorrimento", che è determinato dalla proprietà [overflow](/it/docs/Web/CSS/overflow) dei suoi antenati.

> [!NOTE]
> Può vedere questo esempio in diretta su [`7_sticky-positioning.html`](https://mdn.github.io/learning-area/css/css-layout/positioning/7_sticky-positioning.html) ([vedere codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/positioning/7_sticky-positioning.html)).

## Metti alla prova le sue abilità!

Ha raggiunto la fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare ulteriori test per verificare di averle assimilate prima di procedere — vedere [Metti alla prova le tue abilità: Posizionamento](/it/docs/Learn_web_development/Core/CSS_layout/Test_your_skills/Position).

## Sommario

Sono sicuro che si è divertito a giocare con il posizionamento di base. Anche se non è un metodo ideale da utilizzare per layout interi, ci sono molti obiettivi specifici per cui è adatto. La prossima volta, esamineremo Flexbox.

## Vedere anche

- La proprietà di riferimento {{cssxref("position")}}.
- [Esempi pratici di posizionamento](/it/docs/Learn_web_development/Core/CSS_layout/Practical_positioning_examples), per alcune idee più utili.

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Floats", "Learn_web_development/Core/CSS_layout/Flexbox", "Learn_web_development/Core/CSS_layout")}}
