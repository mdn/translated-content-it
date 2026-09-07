---
title: Posizionamento
slug: Learn_web_development/Core/CSS_layout/Positioning
l10n:
  sourceCommit: 1b7c3c1e03f14c3878e4d8518b0f1a89bedfdc9c
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Test_your_skills/Floats", "Learn_web_development/Core/CSS_layout/Test_your_skills/Position", "Learn_web_development/Core/CSS_layout")}}

Il posizionamento consente di estrarre gli elementi dal normale flusso del documento e farli comportare in modo diverso, ad esempio sovrapponendoli oppure facendoli rimanere sempre nello stesso punto all'interno della viewport del browser. Questo articolo spiega i diversi valori di {{cssxref("position")}} e come usarli.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Fondamenti dello stile CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Stile fondamentale del testo e dei caratteri</a>,
        familiarità con i <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">concetti fondamentali del layout CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Il posizionamento <code>static</code> è il modo predefinito in cui gli elementi vengono posizionati nella pagina.</li>
          <li>Gli elementi posizionati relativamente rimangono nel normale flusso, mentre il posizionamento assoluto (e fisso/sticky) estrae completamente gli elementi dal normale flusso per collocarli in un livello separato.</li>
          <li>La posizione finale del layout può essere modificata usando le proprietà <code>top</code>, <code>bottom</code>, <code>left</code> e <code>right</code>, ma queste hanno effetti diversi in base al valore <code>position</code> impostato.</li>
          <li>Impostare il contesto di posizionamento di un elemento posizionato posizionando un elemento antenato.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Svolgere gli esercizi

È consigliabile svolgere i seguenti esercizi sul computer locale. Per iniziare, creare un nuovo file HTML nel sistema locale e aggiungervi il seguente contenuto:

```html
<!DOCTYPE html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>Positioning example</title>

    <style>
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
    </style>
  </head>
  <body>
    <h1>Basic document flow</h1>

    <p>
      I am a basic block level element. My adjacent block level elements sit on
      new lines below me.
    </p>

    <p>
      By default we span 100% of the width of our parent element, and our height
      is as tall as our child content. Our total width and height is our content
      + padding + border width/height.
    </p>

    <p>
      We are separated by our margins. Because of margin collapsing, we are
      separated by the width of one of our margins, not both.
    </p>

    <p>
      inline elements <span>like this one</span> and <span>this one</span> sit
      on the same line as one another, and adjacent text nodes, if there is
      space on the same line. Overflowing inline elements
      <span
        >wrap onto a new line if possible — like this one containing text</span
      >, or just go on to a new line if not, much like this image will do:
      <img
        src="https://mdn.github.io/shared-assets/images/examples/long.jpg"
        alt="a wide but short section of a photo of several fabrics" />
    </p>
  </body>
</html>
```

## Introduzione al posizionamento

Il posizionamento consente di ottenere risultati interessanti sovrascrivendo il normale flusso del documento. Cosa succede se si desidera modificare leggermente la posizione di alcune caselle rispetto alla loro posizione predefinita nel flusso, per dare un aspetto un po' insolito e irregolare? Il posizionamento è lo strumento adatto. Oppure, cosa succede se si desidera creare un elemento dell'interfaccia utente che fluttua sopra altre parti della pagina e/o rimane sempre nello stesso punto della finestra del browser, indipendentemente da quanto viene fatta scorrere la pagina? Il posizionamento rende possibile questo tipo di layout.

Esistono diversi tipi di posizionamento che si possono applicare agli elementi HTML. Per attivare un tipo specifico di posizionamento su un elemento, si usa la proprietà {{cssxref("position")}}.

## Posizionamento statico

Il posizionamento statico è l'impostazione predefinita per ogni elemento. Significa semplicemente "inserisci l'elemento nella sua posizione predefinita nel normale flusso — niente di particolare."

Per osservare questo comportamento (e preparare l'esempio per le sezioni successive), aggiungere prima una `class` di `positioned` al secondo elemento {{htmlelement("p")}} nell'HTML:

```html
<p class="positioned">…</p>
```

Ora aggiungere la seguente regola alla fine del CSS:

```html hidden live-sample___static
<h1>Static positioning</h1>

<p>
  I am a basic block level element. My adjacent block level elements sit on new
  lines below me.
</p>

<p class="positioned">
  By default we span 100% of the width of our parent element, and our are as
  tall as our child content. Our total width and height is our content + padding
  + border width/height.
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
    alt="a wide but short section of a photo of several fabrics" />
</p>
```

```css hidden live-sample___static live-sample___relative live-sample___absolute
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
```

```css live-sample___static
.positioned {
  position: static;
  background: yellow;
}
```

Salvando e aggiornando la pagina, non si noterà alcuna differenza, eccetto il colore di sfondo aggiornato del secondo paragrafo. L'esempio dovrebbe essere simile al seguente:

{{embedlivesample("static", "100%", 500)}}

Va bene così: come già detto, il posizionamento statico è il comportamento predefinito.

## Posizionamento relativo

Il posizionamento relativo è il primo tipo di posizione che verrà esaminato. È molto simile al posizionamento statico, tranne per il fatto che, una volta che l'elemento posizionato ha occupato il proprio posto nel normale flusso, è possibile modificarne la posizione finale, anche sovrapponendolo ad altri elementi della pagina. Aggiornare la dichiarazione `position` nel codice:

```css
.positioned {
  position: relative;
  background: yellow;
}
```

Salvando e aggiornando a questo punto, non si noterà alcuna modifica nel risultato. Come si modifica quindi la posizione dell'elemento? È necessario usare le proprietà {{cssxref("top")}}, {{cssxref("bottom")}}, {{cssxref("left")}} e {{cssxref("right")}}, che verranno spiegate nella sezione successiva.

### Introduzione a top, bottom, left e right

{{cssxref("top")}}, {{cssxref("bottom")}}, {{cssxref("left")}} e {{cssxref("right")}} vengono usate insieme a {{cssxref("position")}} per specificare esattamente dove spostare l'elemento posizionato. Per provarlo, aggiungere le seguenti dichiarazioni alla regola `.positioned` nel CSS:

```css live-sample___relative
.positioned {
  position: relative;
  background: yellow;
  top: 30px;
  left: 30px;
}
```

> [!NOTE]
> I valori di queste proprietà possono usare qualsiasi [unità](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units) ragionevolmente prevedibile: pixel, mm, rem, %, ecc.

Salvando e aggiornando la pagina, si otterrà un risultato simile a questo:

```html hidden live-sample___relative
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

{{embedlivesample("relative", "100%", 500)}}

Interessante, vero? Probabilmente non era ciò che ci si aspettava. Perché l'elemento si è spostato in basso e a destra se sono stati specificati _top_ e _left_? Questo può sembrare controintuitivo. Bisogna immaginare che esista una forza invisibile che spinge il lato specificato della casella posizionata, spostandola nella direzione opposta. Ad esempio, specificando `top: 30px;`, è come se una forza spingesse la parte superiore della casella, facendola spostare verso il basso di `30px`.

## Posizionamento assoluto

Il posizionamento assoluto produce risultati molto diversi.

### Impostare position: absolute

Provare a modificare la dichiarazione di posizione nel codice come segue:

```css live-sample___absolute
.positioned {
  position: absolute;
  background: yellow;
  top: 30px;
  left: 30px;
}
```

Salvando e aggiornando, si dovrebbe vedere qualcosa di simile:

```html hidden live-sample___absolute
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

{{embedlivesample("absolute", "100%", 420)}}

Prima di tutto, notare che lo spazio in cui l'elemento posizionato dovrebbe trovarsi nel flusso del documento non è più presente: il primo e il terzo elemento si sono avvicinati come se l'elemento non esistesse più. In un certo senso, è proprio così. Un elemento posizionato in modo assoluto non esiste più nel normale flusso del documento. Si trova invece in un proprio livello separato da tutto il resto. Questo è molto utile: significa che è possibile creare funzionalità UI isolate che non interferiscono con il layout degli altri elementi della pagina. Ad esempio, riquadri informativi popup, menu di controllo, pannelli al passaggio del mouse, funzionalità UI trascinabili e rilasciabili in qualsiasi punto della pagina e così via.

In secondo luogo, notare che la posizione dell'elemento è cambiata. Questo accade perché {{cssxref("top")}}, {{cssxref("bottom")}}, {{cssxref("left")}} e {{cssxref("right")}} si comportano in modo diverso con il posizionamento assoluto. Invece di posizionare l'elemento in base alla sua posizione relativa all'interno del normale flusso del documento, specificano la distanza dell'elemento da ciascun lato dell'elemento contenitore. In questo caso, si indica che l'elemento posizionato in modo assoluto deve trovarsi a 30px dalla parte superiore dell'**elemento contenitore** (l'**initial containing block**, in questo caso; vedere sotto) e a 30px dalla sinistra.

> [!NOTE]
> Se necessario, è possibile usare {{cssxref("top")}}, {{cssxref("bottom")}}, {{cssxref("left")}} e {{cssxref("right")}} per ridimensionare gli elementi. Provare a impostare `top: 0; bottom: 0; left: 0; right: 0;` e `margin: 0;` sugli elementi posizionati e osservare cosa accade. Ripristinare poi i valori precedenti.

> [!NOTE]
> Sì, i margini influiscono ancora sugli elementi posizionati. Tuttavia, il collasso dei margini non influisce su di essi.

### Contesti di posizionamento

Quale elemento è l'"elemento contenitore" di un elemento posizionato in modo assoluto? Dipende in larga misura dal valore della proprietà `position` degli antenati dell'elemento posizionato.

Se nessun elemento antenato ha la propria proprietà position definita esplicitamente, per impostazione predefinita tutti gli elementi antenati avranno una posizione statica. Di conseguenza, l'elemento posizionato in modo assoluto sarà contenuto nell'**initial containing block**. L'initial containing block ha le dimensioni della viewport ed è anche il blocco che contiene l'elemento {{htmlelement("html")}}. In altre parole, l'elemento posizionato in modo assoluto verrà visualizzato all'esterno dell'elemento {{htmlelement("html")}} e posizionato rispetto alla viewport iniziale.

L'elemento posizionato è annidato all'interno di {{htmlelement("body")}} nel sorgente HTML, ma nel layout finale si trova a 30px dai bordi superiore e sinistro della pagina.

È possibile modificare il **contesto di posizionamento**, ovvero l'elemento rispetto al quale viene posizionato l'elemento posizionato in modo assoluto. Questo avviene impostando il posizionamento su uno degli antenati dell'elemento (gli elementi al cui interno è annidato; non è possibile posizionarlo rispetto a un elemento al cui interno non è annidato). Per osservarlo, aggiornare la regola `body` per impostare `position: relative` su di essa:

```css
body {
  width: 500px;
  margin: 0 auto;
  position: relative;
}
```

Questo dovrebbe produrre il seguente risultato:

```html hidden live-sample___contexts
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

```css hidden live-sample___contexts live-sample___z-index
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

{{embedlivesample("contexts", "100%", 420)}}

L'elemento posizionato ora si trova relativamente all'elemento {{htmlelement("body")}}.

### Introduzione a z-index

Tutto questo posizionamento assoluto è divertente, ma c'è un'altra funzionalità che non è stata ancora considerata. Quando gli elementi iniziano a sovrapporsi, cosa determina quali elementi appaiono sopra gli altri e quali sotto? Nell'esempio visto finora, c'è un solo elemento posizionato nel contesto di posizionamento e appare in alto poiché gli elementi posizionati hanno la precedenza su quelli non posizionati. Ma cosa accade quando ce n'è più di uno?

Provare ad aggiungere quanto segue al CSS per posizionare in modo assoluto anche il primo paragrafo:

```css
p:nth-of-type(1) {
  position: absolute;
  background: lime;
  top: 10px;
  right: 30px;
}
```

A questo punto il primo paragrafo apparirà color lime, estratto dal flusso del documento e posizionato leggermente più in alto rispetto alla sua posizione originale. Inoltre, nei punti in cui i due elementi si sovrappongono, sarà impilato sotto il paragrafo `.positioned` originale. Questo accade perché il paragrafo `.positioned` è il secondo paragrafo nell'ordine sorgente e gli elementi posizionati che compaiono più tardi nell'ordine sorgente hanno la precedenza sugli elementi posizionati che compaiono prima.

È possibile modificare l'ordine di impilamento? Sì, usando la proprietà {{cssxref("z-index")}}. "z-index" fa riferimento all'asse z. Si potrebbe ricordare che in precedenza nel corso sono state esaminate pagine web che usano coordinate orizzontali (asse x) e verticali (asse y) per calcolare il posizionamento di elementi quali immagini di sfondo e offset delle ombre esterne. Per le lingue scritte da sinistra a destra, (0,0) si trova nell'angolo superiore sinistro della pagina (o dell'elemento), mentre gli assi x e y si estendono verso destra e verso il basso nella pagina.

Le pagine web hanno anche un asse z: una linea immaginaria che va dalla superficie dello schermo verso il viso dell'utente (o verso qualsiasi altra cosa si trovi davanti allo schermo). I valori di {{cssxref("z-index")}} influiscono sul punto in cui gli elementi posizionati si trovano su quell'asse; i valori positivi li spostano più in alto nella pila, mentre i valori negativi li spostano più in basso. Per impostazione predefinita, tutti gli elementi posizionati hanno un `z-index` di `auto`, che equivale di fatto a 0.

Per modificare l'ordine di impilamento, provare ad aggiungere la dichiarazione `z-index: 1` alla regola `p:nth-of-type(1)`:

```css live-sample___z-index
p:nth-of-type(1) {
  position: absolute;
  background: lime;
  top: 10px;
  right: 30px;
  z-index: 1;
}
```

Ora il paragrafo lime dovrebbe apparire sopra:

```html hidden live-sample___z-index
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

{{embedlivesample("z-index", "100%", 350)}}

Notare che `z-index` accetta solo valori di indice senza unità; non è possibile specificare che un elemento deve trovarsi 23 pixel più in alto sull'asse Z: non funziona in questo modo. I valori più alti vengono visualizzati sopra quelli più bassi e spetta allo sviluppatore scegliere quali valori usare. L'uso dei valori 2 e 3 produce lo stesso effetto dei valori 300 e 40000.

## Posizionamento fisso

Esaminiamo ora il posizionamento fisso. Funziona esattamente come il posizionamento assoluto, con una differenza fondamentale: mentre il posizionamento assoluto fissa un elemento rispetto al suo antenato posizionato più vicino (l'initial containing block se non ce n'è uno), il **posizionamento fisso** fissa un elemento rispetto alla porzione visibile della viewport. Questo significa che è possibile creare elementi UI utili che rimangono fissi, come menu di navigazione persistenti sempre visibili indipendentemente da quanto viene fatta scorrere la pagina.

Creiamo un semplice esempio per mostrare cosa si intende. Prima di tutto, eliminare le regole `p:nth-of-type(1)` e `.positioned` esistenti dal CSS.

Ora aggiornare la regola `body` per rimuovere la dichiarazione `position: relative;` e aggiungere un'altezza fissa, come segue:

```css
body {
  width: 500px;
  height: 1400px;
  margin: 0 auto;
}
```

Ora verrà aggiunta una dichiarazione `position: fixed;` all'elemento {{htmlelement("Heading_Elements", "&lt;h1>")}} e questo verrà posizionato nella parte superiore della viewport. Aggiungere la seguente regola al CSS:

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

`top: 0;` è necessario per fissarlo nella parte superiore dello schermo. Al titolo viene assegnata la stessa larghezza della colonna dei contenuti, uno sfondo bianco e un po' di padding e margine affinché il contenuto non sia visibile sotto di esso.

Salvando e aggiornando, si noterà un piccolo effetto: il titolo rimane fisso mentre il contenuto sembra scorrere verso l'alto e scomparire sotto di esso. Notare però che parte del contenuto viene inizialmente ritagliata sotto il titolo. Questo avviene perché il titolo posizionato non compare più nel flusso del documento, quindi il resto del contenuto si sposta verso l'alto.

È possibile migliorare questo comportamento spostando tutti i paragrafi un po' più in basso. Impostare un margine superiore sul primo paragrafo, come segue:

```css
p:nth-of-type(1) {
  margin-top: 60px;
}
```

Ora dovrebbe essere visualizzato il seguente esempio:

```html hidden live-sample___fixed
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

```css hidden live-sample___fixed
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
  margin-top: 0;
  background: white;
  padding: 10px;
}

p:nth-of-type(1) {
  margin-top: 60px;
}
```

{{ EmbedLiveSample('fixed', '100%', 400) }}

## Posizionamento sticky

È disponibile un altro valore di posizione chiamato `position: sticky`, in qualche modo più recente degli altri. Si tratta sostanzialmente di un ibrido tra il posizionamento relativo e quello fisso. Consente a un elemento posizionato di comportarsi come se fosse posizionato relativamente fino a quando non raggiunge una determinata soglia durante lo scorrimento (ad esempio, 10px dalla parte superiore della viewport), dopodiché diventa fisso.

### Esempio di base

Il posizionamento sticky può essere usato, ad esempio, per fare in modo che una barra di navigazione scorra insieme alla pagina fino a un certo punto e poi resti fissata nella parte superiore della pagina.

```html hidden live-sample___basic-sticky
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

```css hidden live-sample___basic-sticky
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

```css live-sample___basic-sticky
.positioned {
  position: sticky;
  top: 30px;
  left: 30px;
}
```

{{ EmbedLiveSample('basic-sticky', '100%', 200) }}

### Indice scorrevole

Un uso interessante e comune di `position: sticky` consiste nel creare una pagina indice scorrevole, in cui titoli diversi rimangono fissati nella parte superiore della pagina quando la raggiungono. Il markup per un esempio di questo tipo potrebbe essere simile al seguente:

```html live-sample___sticky-scrolling-index
<h1>Sticky scrolling index</h1>

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

Il CSS sarebbe il seguente. Nel flusso normale, gli elementi {{htmlelement("dt")}} scorrono insieme al contenuto. Quando si aggiunge `position: sticky` all'elemento {{htmlelement("dt")}}, insieme a un valore {{cssxref("top")}} di `0`, i titoli restano fissati nella parte superiore della viewport quando la raggiungono. Ogni intestazione successiva sostituisce quindi quella precedente quando scorre fino a quella posizione.

```css live-sample___sticky-scrolling-index
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

```css hidden live-sample___sticky-scrolling-index
body {
  width: 500px;
  height: 880px;
  margin: 0 auto;
}
```

{{ EmbedLiveSample('sticky-scrolling-index', '100%', 200) }}

Gli elementi sticky sono "aderenti" rispetto all'antenato più vicino dotato di un "meccanismo di scorrimento", determinato dalla proprietà [overflow](/it/docs/Web/CSS/Reference/Properties/overflow) dei suoi antenati.

## Riepilogo

Probabilmente è stato divertente sperimentare con il posizionamento di base. Anche se non è un metodo ideale da usare per interi layout, è adatto a molti obiettivi specifici.

Nel prossimo articolo saranno proposti alcuni test per verificare quanto bene siano state comprese e memorizzate tutte queste informazioni.

## Vedi anche

- Il riferimento della proprietà {{cssxref("position")}}.
- [Esempi pratici di posizionamento](/it/docs/Learn_web_development/Core/CSS_layout/Practical_positioning_examples), per altre idee utili.

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Test_your_skills/Floats", "Learn_web_development/Core/CSS_layout/Test_your_skills/Position", "Learn_web_development/Core/CSS_layout")}}
