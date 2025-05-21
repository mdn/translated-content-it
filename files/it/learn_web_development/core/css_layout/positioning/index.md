---
title: Posizionamento
slug: Learn_web_development/Core/CSS_layout/Positioning
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Floats", "Learn_web_development/Core/CSS_layout/Flexbox", "Learn_web_development/Core/CSS_layout")}}

Il posizionamento consente di rimuovere gli elementi dal normale flusso del documento e farli comportare in modo diverso, ad esempio sovrapponendoli o mantenendoli sempre nella stessa posizione all'interno della finestra del browser. Questo articolo spiega i diversi valori della proprietà {{cssxref("position")}} e come utilizzarli.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo styling CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Fondamenti di stile del testo e dei font</a>,
        familiarità con <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">i concetti fondamentali del layout CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Il posizionamento <code>static</code> è il modo predefinito in cui gli elementi sono posizionati sulla pagina.</li>
          <li>Gli elementi posizionati relativamente rimangono nel flusso normale, ma il posizionamento assoluto (e fisso/adesivo) rimuove completamente gli elementi dal flusso normale per farli sedere in un livello separato.</li>
          <li>La posizione di layout finale può essere modificata utilizzando le proprietà <code>top</code>, <code>bottom</code>, <code>left</code> e <code>right</code>, ma queste hanno effetti diversi a seconda del valore <code>position</code> impostato.</li>
          <li>Impostazione del contesto di posizionamento di un elemento posizionato posizionando un elemento antenato.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Vorremmo che eseguissi i seguenti esercizi sul tuo computer locale. Se possibile, prendi una copia di [`0_basic-flow.html`](https://mdn.github.io/learning-area/css/css-layout/positioning/0_basic-flow.html) dal nostro repo GitHub ([codice sorgente qui](https://github.com/mdn/learning-area/blob/main/css/css-layout/positioning/0_basic-flow.html)) e usalo come punto di partenza.

## Introduzione al posizionamento

Il posizionamento ci permette di ottenere risultati interessanti sovrascrivendo il normale flusso del documento. E se volessi modificare leggermente la posizione di alcuni box dalla loro posizione di flusso predefinita per dare un tocco leggermente eccentrico e scomposto? Il posizionamento è il tuo strumento. O cosa succede se vuoi creare un elemento UI che fluttua sopra altre parti della pagina e/o si trova sempre nello stesso punto all'interno della finestra del browser indipendentemente da quanto la pagina viene scorsa? Il posizionamento rende possibile questo tipo di layout.

Ci sono diversi tipi di posizionamento che puoi applicare agli elementi HTML. Per attivare un tipo specifico di posizionamento su un elemento, utilizziamo la proprietà {{cssxref("position")}}.

## Posizionamento statico

Il posizionamento statico è il predefinito che ogni elemento riceve. Significa semplicemente "posiziona l'elemento nella sua posizione predefinita nel flusso normale — niente di speciale da vedere qui."

Per vedere questo (e impostare il tuo esempio per le sezioni future) innanzitutto aggiungi una `class` di `positioned` al secondo {{htmlelement("p")}} nel tuo HTML:

```html
<p class="positioned">…</p>
```

Ora aggiungi la seguente regola alla fine del tuo CSS:

```css
.positioned {
  position: static;
  background: yellow;
}
```

Se salvi e ricarichi, non vedrai alcuna differenza, se non per il colore di sfondo aggiornato del secondo paragrafo. Questo è normale — come detto prima, il posizionamento statico è il comportamento predefinito!

> [!NOTE]
> Puoi vedere l'esempio a questo punto dal vivo su [`1_static-positioning.html`](https://mdn.github.io/learning-area/css/css-layout/positioning/1_static-positioning.html) ([vedi codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/positioning/1_static-positioning.html)).

## Posizionamento relativo

Il posizionamento relativo è il primo tipo di posizionamento che esamineremo. È molto simile al posizionamento statico, tranne per il fatto che, una volta che l'elemento posizionato ha preso il suo posto nel flusso normale, puoi quindi modificare la sua posizione finale, incluso farlo sovrapporre ad altri elementi sulla pagina. Vai avanti e aggiorna la dichiarazione `position` nel tuo codice:

```css
position: relative;
```

Se salvi e ricarichi a questo punto, non vedrai alcun cambiamento nel risultato. Quindi come puoi modificare la posizione dell'elemento? È necessario utilizzare le proprietà {{cssxref("top")}}, {{cssxref("bottom")}}, {{cssxref("left")}} e {{cssxref("right")}}, che spiegheremo nella sezione successiva.

### Introduzione a top, bottom, left, e right

{{cssxref("top")}}, {{cssxref("bottom")}}, {{cssxref("left")}} e {{cssxref("right")}} sono utilizzati insieme a {{cssxref("position")}} per specificare esattamente dove spostare l'elemento posizionato. Per provarlo, aggiungi le seguenti dichiarazioni alla regola `.positioned` nel tuo CSS:

```css
top: 30px;
left: 30px;
```

> [!NOTE]
> I valori di queste proprietà possono utilizzare qualsiasi [unità](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units) che ti aspetteresti ragionevolmente: pixel, mm, rem, %, ecc.

Se ora salvi e ricarichi, otterrai un risultato simile a questo:

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

Interessante, vero? Bene, probabilmente non era quello che ti aspettavi. Perché si è spostato verso il basso e a destra se abbiamo specificato _top_ e _left_? Questo può sembrare controintuitivo. Devi pensarlo come se ci fosse una forza invisibile che spinge il lato specificato del box posizionato, spostandolo nella direzione opposta. Quindi, ad esempio, se specifichi `top: 30px;`, sembra che una forza spinga la parte superiore del box, facendolo spostare verso il basso di 30px.

> [!NOTE]
> Puoi vedere l'esempio a questo punto dal vivo su [`2_relative-positioning.html`](https://mdn.github.io/learning-area/css/css-layout/positioning/2_relative-positioning.html) ([vedi codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/positioning/2_relative-positioning.html)).

## Posizionamento assoluto

Il posizionamento assoluto produce risultati molto diversi.

### Impostazione di position: absolute

Proviamo a cambiare la dichiarazione di posizione nel tuo codice come segue:

```css
position: absolute;
```

Se ora salvi e ricarichi, dovresti vedere qualcosa del genere:

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

Per prima cosa, nota che lo spazio dove dovrebbe trovarsi l'elemento posizionato nel flusso del documento non è più presente — il primo e il terzo elemento si sono chiusi insieme come se non esistesse più! Bene, in un certo senso, questo è vero. Un elemento posizionato assolutamente non esiste più nel normale flusso del documento. Invece, si siede su un proprio livello separato da tutto il resto. Questo è molto utile: significa che possiamo creare funzionalità UI isolate che non interferiscono con il layout di altri elementi sulla pagina. Ad esempio, box informativi a comparsa, menu di controllo, pannelli a rotazione, funzionalità UI che possono essere trascinate e rilasciate ovunque sulla pagina, e così via.

Secondo, nota che la posizione dell'elemento è cambiata. Questo perché {{cssxref("top")}}, {{cssxref("bottom")}}, {{cssxref("left")}} e {{cssxref("right")}} si comportano in modo diverso con il posizionamento assoluto. Piuttosto che posizionare l'elemento in base alla sua posizione relativa all'interno del normale flusso del documento, specificano la distanza che l'elemento dovrebbe essere da ciascun lato dell'elemento contenitore. In questo caso, stiamo dicendo che l'elemento posizionato assolutamente dovrebbe trovarsi a 30px dalla parte superiore dell'**elemento contenitore** (il **blocco contenitore iniziale**, in questo caso, vedi sotto) e 30px da sinistra.

> [!NOTE]
> Puoi usare {{cssxref("top")}}, {{cssxref("bottom")}}, {{cssxref("left")}} e {{cssxref("right")}} per ridimensionare gli elementi se necessario. Prova a impostare `top: 0; bottom: 0; left: 0; right: 0;` e `margin: 0;` sui tuoi elementi posizionati e vedere cosa succede! Rimettilo poi come era prima...

> [!NOTE]
> Sì, i margini influenzano ancora gli elementi posizionati. Il collasso dei margini, comunque, no.

> [!NOTE]
> Puoi vedere l'esempio a questo punto dal vivo su [`3_absolute-positioning.html`](https://mdn.github.io/learning-area/css/css-layout/positioning/3_absolute-positioning.html) ([vedi codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/positioning/3_absolute-positioning.html)).

### Contesti di posizionamento

Quale elemento è l'"elemento contenitore" di un elemento posizionato assolutamente? Questo dipende molto dal valore della proprietà `position` degli antenati dell'elemento posizionato.

Se nessun elemento antenato ha la proprietà position definita esplicitamente, allora per impostazione predefinita tutti gli antenati avranno una posizione statica. Il risultato è che l'elemento posizionato assolutamente sarà contenuto nel **blocco contenitore iniziale**. Il blocco contenitore iniziale ha le dimensioni della viewport ed è anche il blocco che contiene l'elemento {{htmlelement("html")}}. In altre parole, l'elemento posizionato assolutamente verrà visualizzato al di fuori dell'elemento {{htmlelement("html")}} e sarà posizionato rispetto al viewport iniziale.

L'elemento posizionato è nidificato all'interno dell'elemento {{htmlelement("body")}} nel sorgente HTML, ma nel layout finale è a 30px dai bordi superiori e sinistri della pagina. Possiamo cambiare il **contesto di posizionamento**, cioè quale elemento l'elemento posizionato assolutamente è relativo a. Questo si fa impostando il posizionamento su uno degli antenati dell'elemento: uno degli elementi in cui è nidificato (non puoi posizionarlo relativo a un elemento in cui non è nidificato). Per vedere questo, aggiungi la seguente dichiarazione alla tua regola `body`:

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

L'elemento posizionato ora si trova relativo all'elemento {{htmlelement("body")}}.

> [!NOTE]
> Puoi vedere l'esempio a questo punto dal vivo su [`4_positioning-context.html`](https://mdn.github.io/learning-area/css/css-layout/positioning/4_positioning-context.html) ([vedi codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/positioning/4_positioning-context.html)).

### Introduzione a z-index

Tutto questo posizionamento assoluto è molto divertente, ma c'è un'altra funzionalità che non abbiamo ancora considerato. Quando gli elementi iniziano a sovrapporsi, cosa determina quali elementi appaiono sopra gli altri e quali elementi appaiono sotto gli altri? Nell'esempio che abbiamo visto finora, abbiamo solo un elemento posizionato nel contesto di posizionamento, e appare in cima poiché gli elementi posizionati prevalgono sugli elementi non posizionati. Che cosa succede quando ne abbiamo più di uno?

Prova ad aggiungere il seguente codice al tuo CSS per posizionare assolutamente anche il primo paragrafo:

```css
p:nth-of-type(1) {
  position: absolute;
  background: lime;
  top: 10px;
  right: 30px;
}
```

A questo punto vedrai il primo paragrafo colorato di verde lime, spostato fuori dal flusso del documento, e posizionato un po' sopra rispetto a dove era originariamente. È anche impilato sotto il paragrafo `.positioned` originale dove si sovrappongono. Questo perché il paragrafo `.positioned` è il secondo paragrafo nell'ordine del sorgente, e gli elementi posizionati più tardi nell'ordine del sorgente prevalgono sugli elementi posizionati più presto nell'ordine del sorgente.

Puoi cambiare l'ordine di impilamento? Sì, puoi farlo usando la proprietà {{cssxref("z-index")}}. "z-index" è un riferimento all'asse z. Potresti ricordare dai punti precedenti del corso quando abbiamo discusso delle pagine web che utilizzano coordinate orizzontali (asse x) e verticali (asse y) per calcolare il posizionamento per cose come immagini di sfondo e offset di ombre. Per le lingue che scorrono da sinistra a destra, (0,0) è in alto a sinistra della pagina (o dell'elemento), e gli assi x e y si estendono a destra e in basso nella pagina.

Le pagine web hanno anche un asse z: una linea immaginaria che va dalla superficie del tuo schermo verso il tuo viso (o qualsiasi altra cosa ti piaccia avere davanti allo schermo). I valori di {{cssxref("z-index")}} influenzano dove si trovano gli elementi posizionati su quell'asse; i valori positivi li spostano più in alto nella pila, i valori negativi li spostano più in basso nella pila. Per impostazione predefinita, gli elementi posizionati hanno tutti un `z-index` di `auto`, che è effettivamente 0.

Per cambiare l'ordine di impilamento, prova ad aggiungere la seguente dichiarazione alla tua regola `p:nth-of-type(1)`:

```css
z-index: 1;
```

Dovresti ora vedere il paragrafo lime sopra:

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

Nota che `z-index` accetta solo valori di indice senza unità; non puoi specificare che vuoi un elemento a 23 pixel sull'asse Z — non funziona in questo modo. I valori più alti vanno sopra i valori più bassi e spetta a te quali valori utilizzare. Usare valori di 2 o 3 darebbe lo stesso effetto di valori di 300 o 40000.

> [!NOTE]
> Puoi vedere un esempio di questo dal vivo su [`5_z-index.html`](https://mdn.github.io/learning-area/css/css-layout/positioning/5_z-index.html) ([vedi codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/positioning/5_z-index.html)).

## Posizionamento fisso

Vediamo ora il posizionamento fisso. Questo funziona nello stesso modo del posizionamento assoluto, con una differenza chiave: mentre il posizionamento assoluto fissa un elemento in posizione rispetto al suo antenato posizionato più vicino (il blocco contenitore iniziale se non ce n'è uno), il **posizionamento fisso** fissa un elemento in posizione rispetto alla porzione visibile del viewport. Ciò significa che puoi creare elementi UI utili che sono fissi in posizione, come i menu di navigazione persistenti che sono sempre visibili indipendentemente da quanto la pagina viene scorsa.

Mettiamo insieme un semplice esempio per mostrare cosa intendiamo. Prima di tutto, elimina le regole esistenti `p:nth-of-type(1)` e `.positioned` dal tuo CSS.

Ora aggiorna la regola `body` per rimuovere la dichiarazione `position: relative;` e aggiungere un'altezza fissa, come segue:

```css
body {
  width: 500px;
  height: 1400px;
  margin: 0 auto;
}
```

Ora impostiamo l'elemento {{htmlelement("Heading_Elements", "&lt;h1>")}} con `position: fixed;` e lo facciamo sedere in cima al viewport. Aggiungi la seguente regola al tuo CSS:

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

Il `top: 0;` è necessario per farlo aderire alla parte superiore dello schermo. Diamo all'intestazione la stessa larghezza della colonna dei contenuti e poi uno sfondo bianco e qualche padding e margin in modo che i contenuti non siano visibili sotto di essa.

Se salvi e ricarichi, vedrai un simpatico effetto con l'intestazione che rimane fissa — sembra che il contenuto scorra verso l'alto scomparendo sotto di essa. Ma nota come alcuni contenuti siano inizialmente tagliati sotto l'intestazione. Questo perché l'intestazione posizionata non appare più nel flusso del documento, quindi il resto del contenuto si sposta verso l'alto fino al top. Possiamo migliorare questo spostando tutti i paragrafi un po' verso il basso. Possiamo farlo impostando un po' di margine superiore sul primo paragrafo. Aggiungi ora:

```css
p:nth-of-type(1) {
  margin-top: 60px;
}
```

Dovresti ora vedere l'esempio terminato:

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
> Puoi vedere un esempio per questo dal vivo su [`6_fixed-positioning.html`](https://mdn.github.io/learning-area/css/css-layout/positioning/6_fixed-positioning.html) ([vedi codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/positioning/6_fixed-positioning.html)).

## Posizionamento adesivo

C'è un altro valore di posizione disponibile chiamato `position: sticky`, che è relativamente più recente degli altri. Questo è fondamentalmente un ibrido tra posizionamento relativo e fisso. Permette a un elemento posizionato di agire come se fosse posizionato relativamente fino a quando non viene scorrinato fino a una certa soglia (es. 10px dalla parte superiore del viewport), dopo di che diventa fisso.

### Esempio base

Il posizionamento adesivo può essere utilizzato, ad esempio, per causare a una barra di navigazione di scorrere con la pagina fino a un certo punto e poi aderire alla parte superiore della pagina.

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

Un uso interessante e comune di `position: sticky` è la creazione di una pagina indice di scorrimento dove diverse intestazioni aderiscono alla parte superiore della pagina man mano che la raggiungono. Il markup per un esempio del genere potrebbe sembrare così:

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

Il CSS potrebbe apparire come segue. Nel flusso normale gli elementi {{htmlelement("dt")}} scorrono con il contenuto. Quando aggiungiamo `position: sticky` all'elemento {{htmlelement("dt")}}, insieme a un valore {{cssxref("top")}} di 0, i browser supportati aderiscono le intestazioni al top del viewport quando raggiungono quella posizione. Ogni intestazione successiva sostituirà la precedente mentre scorre fino a quella posizione.

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

Gli elementi adesivi sono "adesivi" rispetto all'antenato più vicino con un "meccanismo di scorrimento", che è determinato dalla proprietà [overflow](/it/docs/Web/CSS/overflow) degli antenati.

> [!NOTE]
> Puoi vedere questo esempio dal vivo su [`7_sticky-positioning.html`](https://mdn.github.io/learning-area/css/css-layout/positioning/7_sticky-positioning.html) ([vedi codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/positioning/7_sticky-positioning.html)).

## Testa le tue abilità!

Sei arrivato alla fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare di aver assimilato queste informazioni prima di continuare — vedi [Testa le tue abilità: Posizionamento](/it/docs/Learn_web_development/Core/CSS_layout/Test_your_skills/Position).

## Sommario

Sono sicuro che ti sei divertito a giocare con il posizionamento base. Anche se non è un metodo ideale da utilizzare per layout completi, ci sono molti obiettivi specifici per cui è adatto. Successivamente esamineremo Flexbox.

## Vedi anche

- La proprietà {{cssxref("position")}} di riferimento.
- [Esempi pratici di posizionamento](/it/docs/Learn_web_development/Core/CSS_layout/Practical_positioning_examples), per altre idee utili.

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Floats", "Learn_web_development/Core/CSS_layout/Flexbox", "Learn_web_development/Core/CSS_layout")}}
