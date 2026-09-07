---
title: Sfondi e bordi
slug: Learn_web_development/Core/Styling_basics/Backgrounds_and_borders
l10n:
  sourceCommit: 1b7c3c1e03f14c3878e4d8518b0f1a89bedfdc9c
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Test_your_skills/Sizing", "Learn_web_development/Core/Styling_basics/Test_your_skills/Backgrounds_and_borders", "Learn_web_development/Core/Styling_basics")}}

In questa lezione verranno esaminate alcune delle possibilità creative offerte dagli sfondi e dai bordi CSS. Dall'aggiunta di gradienti, immagini di sfondo e angoli arrotondati, gli sfondi e i bordi rispondono a molte esigenze di styling in CSS.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units">Valori e unità CSS</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics/Sizing">Dimensionamento CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Styling di base degli sfondi — colori e immagini.</li>
          <li>Dimensione, ripetizione, posizione e fissaggio dell'immagine di sfondo.</li>
          <li>Gradienti di sfondo — concetto generale e gradienti lineari (i gradienti radiali, conici e ripetuti sono più avanzati; in questa fase non è richiesta una conoscenza approfondita).</li>
          <li>Considerazioni di accessibilità per gli sfondi — garantire un buon contrasto.</li>
          <li>Nozioni di base sui bordi — larghezza, stile, colore e abbreviazione dei bordi. Raggio del bordo per gli angoli arrotondati.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Colori di sfondo

La proprietà {{cssxref("background-color")}} definisce il colore di sfondo di qualsiasi elemento in CSS. La proprietà accetta qualsiasi valore {{cssxref("&lt;color&gt;")}} valido. Un `background-color` si estende sotto il contenuto e il riquadro di riempimento dell'elemento.

Nell'esempio seguente sono stati usati vari valori di colore per aggiungere un colore di sfondo al riquadro, a un'intestazione e a un elemento {{htmlelement("span")}}.

Provare a modificare l'esempio sostituendo i colori specificati con uno qualsiasi dei valori {{cssxref("&lt;color&gt;")}} disponibili.

```html live-sample___color
<div class="box">
  <h2>Background Colors</h2>
  <p>Try changing the background <span>colors</span>.</p>
</div>
```

```css live-sample___color
.box {
  padding: 0.3em;
  background-color: #567895;
}

h2 {
  background-color: black;
  color: white;
}
span {
  background-color: rgb(255 255 255 / 50%);
}
```

{{EmbedLiveSample("color")}}

## Immagini di sfondo

La proprietà {{cssxref("background-image")}} consente di visualizzare un'immagine sullo sfondo di un elemento. Nell'esempio seguente sono presenti due riquadri: uno ha un'immagine di sfondo più grande del riquadro ([balloons.jpg](https://mdn.github.io/shared-assets/images/examples/balloons.jpg)). L'altro ha una piccola immagine di una singola stella ([star.png](https://mdn.github.io/shared-assets/images/examples/star.png)).

Questo esempio dimostra due aspetti delle immagini di sfondo. Per impostazione predefinita, l'immagine grande non viene ridimensionata per adattarsi al riquadro, quindi se ne vede solo un piccolo angolo, mentre l'immagine piccola viene ripetuta per riempire il riquadro.

```html live-sample___background-image
<div class="wrapper">
  <div class="box a"></div>
  <div class="box b"></div>
</div>
```

```css live-sample___background-image
.wrapper {
  display: flex;
}

.box {
  width: 200px;
  height: 80px;
  padding: 0.5em;
  border: 1px solid #cccccc;
  margin: 20px;
}

.a {
  background-image: url("https://mdn.github.io/shared-assets/images/examples/balloons.jpg");
}

.b {
  background-image: url("https://mdn.github.io/shared-assets/images/examples/star.png");
}
```

{{EmbedLiveSample("background-image")}}

Se viene specificato un colore di sfondo oltre a un'immagine di sfondo, l'immagine viene visualizzata sopra il colore.
Provare ad aggiungere una proprietà `background-color` all'esempio precedente per vederlo in azione.

### Controllare `background-repeat`

La proprietà {{cssxref("background-repeat")}} viene usata per controllare il comportamento di ripetizione delle immagini. I valori disponibili sono:

- `no-repeat` — interrompe completamente la ripetizione dello sfondo.
- `repeat-x` — ripete orizzontalmente.
- `repeat-y` — ripete verticalmente.
- `repeat` — valore predefinito; ripete in entrambe le direzioni.
- `space` — ripete il maggior numero di volte possibile, aggiungendo spazio tra le immagini se è disponibile spazio aggiuntivo.
- `round` — simile a `space`, ma estende le immagini per riempire l'eventuale spazio aggiuntivo.

Provare questi valori nell'esempio seguente. Il valore è stato impostato su `no-repeat`, quindi verrà visualizzata una sola stella. Provare i diversi valori per osservare i loro effetti.

```html live-sample___repeat
<div class="box"></div>
```

```css hidden live-sample___repeat
.box {
  width: 200px;
  height: 80px;
  padding: 0.5em;
  border: 1px solid #cccccc;
  margin: 20px;
}
```

```css live-sample___repeat
.box {
  background-image: url("https://mdn.github.io/shared-assets/images/examples/star.png");
  background-repeat: no-repeat;
}
```

{{EmbedLiveSample("repeat")}}

### Dimensionare l'immagine di sfondo

L'immagine _balloons.jpg_ usata nell'esempio iniziale sull'immagine di sfondo è un'immagine grande che è stata ritagliata perché più grande dell'elemento di cui costituisce lo sfondo. In questo caso, è possibile usare la proprietà {{cssxref("background-size")}} per dimensionare l'immagine in modo che si adatti allo sfondo.

`background-size` può accettare due valori {{cssxref("length")}} o {{cssxref("percentage")}} per specificare la dimensione dell'immagine nelle direzioni orizzontale e verticale, oppure le seguenti parole chiave:

- `cover` — il browser renderà l'immagine appena abbastanza grande da coprire completamente l'area del riquadro mantenendo il suo {{Glossary("aspect_ratio", "rapporto d'aspetto")}}. In questo caso, è probabile che una parte dell'immagine finisca fuori dal riquadro.
- `contain` — il browser renderà l'immagine delle dimensioni corrette per adattarsi al riquadro. In questo caso, potrebbero rimanere spazi vuoti sui lati oppure sopra e sotto l'immagine, se il rapporto d'aspetto dell'immagine è diverso da quello del riquadro.

#### Sperimentare con `background-size`

Nell'esempio seguente, all'immagine _balloons.jpg_ sono state assegnate unità di lunghezza per adattarla al riquadro. È possibile notare che questo ha distorto l'immagine.

Provare quanto segue:

- Modificare le unità di lunghezza usate per cambiare la dimensione dello sfondo.
- Rimuovere le unità di lunghezza e osservare cosa accade usando `background-size: cover` oppure `background-size: contain`.
- Dimensionare l'immagine in modo che sia più piccola del riquadro, quindi modificare il valore di `background-repeat` per ripetere l'immagine.

```html live-sample___size
<div class="box"></div>
```

```css hidden live-sample___size
.box {
  width: 500px;
  height: 100px;
  padding: 0.5em;
  border: 1px solid #cccccc;
  margin: 10px;
}
```

```css live-sample___size
.box {
  background-image: url("https://mdn.github.io/shared-assets/images/examples/balloons.jpg");
  background-repeat: no-repeat;
  background-size: 80px 10em;
}
```

{{EmbedLiveSample("size")}}

### Posizionare l'immagine di sfondo

La proprietà {{cssxref("background-position")}} consente di scegliere la posizione in cui l'immagine di sfondo viene visualizzata nel riquadro a cui è applicata. Usa un sistema di coordinate in cui l'angolo superiore sinistro del riquadro è `(0,0)` e il riquadro è posizionato lungo gli assi orizzontale (`x`) e verticale (`y`).

> [!NOTE]
> Il valore predefinito di `background-position` è `(0,0)`.

I valori più comuni di `background-position` accettano due valori individuali: un valore orizzontale seguito da un valore verticale. È possibile usare parole chiave come `top` e `right` (consultare gli altri nella pagina {{cssxref("background-position")}}):

```css
.box {
  background-image: url("image.png");
  background-repeat: no-repeat;
  background-position: top center;
}
```

È inoltre possibile usare {{cssxref("length", "length")}} e {{cssxref("percentage", "percentuali")}}:

```css
.box {
  background-image: url("image.png");
  background-repeat: no-repeat;
  background-position: 20px 10%;
}
```

È anche possibile combinare valori di parole chiave con lunghezze o percentuali; in questo caso, il primo valore fa riferimento alla posizione orizzontale e il secondo a quella verticale. Ad esempio:

```css
.box {
  background-image: url("image.png");
  background-repeat: no-repeat;
  background-position: 20px top;
}
```

Infine, è possibile usare anche una sintassi a 4 valori per indicare una distanza da determinati bordi del riquadro. Ogni coppia di valori rappresenta il bordo del riquadro da cui applicare l'offset e l'entità dell'offset da quel bordo. Nel frammento seguente, lo sfondo viene posizionato a `20px` dal `top` e a `10px` dal `right`:

```css
.box {
  background-image: url("image.png");
  background-repeat: no-repeat;
  background-position: top 20px right 10px;
}
```

#### Sperimentare con `background-position`

Usare l'esempio seguente per sperimentare con questi valori e spostare la stella all'interno del riquadro:

```html live-sample___position
<div class="box"></div>
```

```css hidden live-sample___position
.box {
  width: 500px;
  height: 80px;
  padding: 0.5em;
  border: 1px solid #cccccc;
  margin: 20px;
}
```

```css live-sample___position
.box {
  background-image: url("https://mdn.github.io/shared-assets/images/examples/star.png");
  background-repeat: no-repeat;
  background-position: 120px 1em;
}
```

{{EmbedLiveSample("position")}}

> [!NOTE]
> L'abbreviazione `background-position` viene usata al posto di {{cssxref("background-position-x")}} e {{cssxref("background-position-y")}}, che consentono di impostare singolarmente i valori di posizione dei diversi assi.

## Sfondi con gradiente

Un gradiente, quando viene usato come sfondo, si comporta esattamente come un'immagine e viene anch'esso impostato usando la proprietà {{cssxref("background-image")}}.

Informazioni sui diversi tipi di valori di gradiente e sulle possibilità offerte sono disponibili nella pagina MDN relativa al tipo di dati {{cssxref("gradient")}}.

Provare alcuni valori di gradiente diversi nell'esempio seguente. Inizialmente, è presente un gradiente lineare esteso sull'intero primo riquadro e un gradiente radiale di dimensione impostata, ripetuto sul secondo riquadro.

```html live-sample___gradients
<div class="wrapper">
  <div class="box a"></div>
  <div class="box b"></div>
</div>
```

```css live-sample___gradients
.wrapper {
  display: flex;
}

.box {
  width: 400px;
  height: 80px;
  padding: 0.5em;
  border: 1px solid #cccccc;
  margin: 20px;
}

.a {
  background-image: linear-gradient(
    105deg,
    rgb(0 249 255 / 100%) 39%,
    rgb(51 56 57 / 100%) 96%
  );
}

.b {
  background-image: radial-gradient(
    circle,
    rgb(0 249 255 / 100%) 39%,
    rgb(51 56 57 / 100%) 96%
  );
  background-size: 100px 50px;
}
```

{{EmbedLiveSample("gradients")}}

> [!NOTE]
> Un modo divertente per sperimentare con i gradienti è usare uno dei numerosi generatori di gradienti CSS disponibili sul web, come [CSSGradient.io](https://cssgradient.io/). È possibile creare un gradiente e copiare e incollare il codice sorgente che lo genera.

## Immagini di sfondo multiple

È anche possibile specificare più immagini di sfondo in una singola dichiarazione. Per farlo, specificare più valori `background-image` separati da virgole.

In questo caso, le immagini di sfondo potrebbero sovrapporsi. Gli sfondi vengono sovrapposti con l'ultima immagine di sfondo elencata in fondo alla pila, e ogni immagine precedente viene sovrapposta a quella che la segue nel codice.

> [!NOTE]
> I gradienti possono essere combinati senza problemi con normali immagini di sfondo.

Anche le altre proprietà `background-*` possono avere valori separati da virgole, allo stesso modo di `background-image`:

```css
background-image:
  url("image1.png"), url("image2.png"), url("image3.png"), url("image4.png");
background-repeat: no-repeat, repeat-x, repeat;
background-position:
  10px 20px,
  top right;
```

Ogni valore delle diverse proprietà corrisponderà ai valori nella stessa posizione delle altre proprietà. Sopra, ad esempio, il valore `background-repeat` di `image1` sarà `no-repeat`. Tuttavia, cosa accade quando proprietà diverse hanno un numero diverso di valori? La risposta è che il numero minore di valori viene ripetuto ciclicamente: nell'esempio precedente sono presenti quattro immagini di sfondo, ma solo due valori `background-position`. I primi due valori di posizione verranno applicati alle prime due immagini, quindi il ciclo ricomincerà: a `image3` verrà assegnato il primo valore di posizione e a `image4` il secondo valore di posizione.

### Sperimentare con immagini di sfondo multiple

Sperimentiamo. L'esempio seguente include due immagini di sfondo. Provare a modificare l'esempio come segue:

- Per dimostrare l'ordine di sovrapposizione, provare a invertire l'ordine delle immagini di sfondo nell'elenco.
- Aggiungere altre proprietà `background-*` per modificare la posizione, la dimensione o il valore di ripetizione delle immagini.
- Provare ad aggiungere un gradiente come terza `background-image`.

```html live-sample___multiple-background-image
<div class="wrapper">
  <div class="box"></div>
</div>
```

```css live-sample___multiple-background-image
.wrapper {
  display: flex;
}

.box {
  width: 500px;
  height: 80px;
  padding: 0.5em;
  border: 1px solid #cccccc;
  margin: 20px;
}

.box {
  background-image:
    url("https://mdn.github.io/shared-assets/images/examples/star.png"),
    url("https://mdn.github.io/shared-assets/images/examples/big-star.png");
}
```

{{EmbedLiveSample("multiple-background-image")}}

## Fissaggio dello sfondo

Un'altra opzione disponibile per gli sfondi è specificare come scorrono quando il contenuto scorre. Questo è controllato usando la proprietà {{cssxref("background-attachment")}}, che può accettare i seguenti valori:

- `scroll`: fa scorrere lo sfondo dell'elemento quando la pagina viene fatta scorrere. Se il contenuto dell'elemento viene fatto scorrere, lo sfondo non si sposta. In effetti, lo sfondo è fissato alla stessa posizione sulla pagina, quindi scorre insieme alla pagina.
- `fixed`: fissa lo sfondo di un elemento alla viewport, in modo che non scorra quando viene fatta scorrere la pagina o il contenuto dell'elemento. Rimane sempre nella stessa posizione sullo schermo.
- `local`: fissa lo sfondo all'elemento su cui è impostato, quindi quando l'elemento viene fatto scorrere, lo sfondo scorre con esso.

La proprietà {{cssxref("background-attachment")}} ha effetto solo quando esiste contenuto da scorrere, quindi è stata creata una demo per mostrare le differenze tra i tre valori:

```html hidden live-sample___background-atachment
<section>
  <article class="scroll">
    <p>
      <code>background-attachment: scroll</code> causes the element's background
      to be fixed to the page, so that it scrolls when the page is scrolled. If
      the element content is scrolled, the background does not move.
    </p>

    <pre></pre>
  </article>

  <article class="fixed">
    <p>
      <code>background-attachment: fixed</code> causes an element's background
      to be fixed to the viewport, so that it doesn't scroll when the page or
      element content is scrolled. It will always remain in the same position on
      the screen.
    </p>

    <pre></pre>
  </article>

  <article class="local">
    <p>
      <code>background-attachment: local</code> causes an element's background
      to be fixed to the actual element itself. When the page is scrolled, the
      element's background will move along with it only if the element does so.
      When the element's content is scrolled, the background will scroll along
      with it.
    </p>

    <pre></pre>
  </article>
</section>
```

```css hidden live-sample___background-atachment
html,
body {
  margin: 0;
  padding: 0;
}

h1 {
  margin-top: 0;
}

body {
  padding: 1em;
}

html {
  background-color: yellow;
  font-family: sans-serif;
}

body {
  height: 2000px;
}

p {
  padding: 10px;
  color: white;
  background: rgba(0, 0, 0, 0.3);
}

section {
  display: flex;
  gap: 10px;
}

article {
  flex: 1;
  height: 300px;
  background-color: rgba(0, 0, 0, 0.5);
  background-image: url(https://mdn.github.io/shared-assets/images/examples/grapefruit-slice.jpg);
  background-size: 400px 400px;
  background-repeat: no-repeat;
  background-position: top center;
  padding: 1%;
  overflow: auto;
}

article pre {
  height: 800px;
}

.fixed {
  background-attachment: fixed;
}

.scroll {
  background-attachment: scroll;
}

.local {
  background-attachment: local;
}
```

{{embedlivesample("background-attachment", "100%", 350)}}

Provare a far scorrere l'intero esempio incorporato e poi i singoli contenitori, osservando le differenze nel comportamento degli sfondi dei contenitori.

## Usare la proprietà abbreviata `background`

Spesso gli sfondi vengono specificati usando la proprietà abbreviata {{cssxref("background")}}, che consente di impostare tutte le diverse proprietà contemporaneamente.

Se si usano più sfondi, è necessario specificare tutte le proprietà per il primo sfondo, quindi aggiungere lo sfondo successivo dopo una virgola. Nell'esempio seguente è presente un gradiente con dimensione e posizione, poi un'immagine di sfondo con `no-repeat` e una posizione, quindi un colore.

Esistono alcune regole da seguire quando si scrivono i valori abbreviati delle immagini di sfondo, ad esempio:

- Un `background-color` può essere specificato solo dopo la virgola finale.
- Il valore di `background-size` può essere incluso solo immediatamente dopo `background-position`, separato dal carattere `/`, in questo modo: `center/80%`.

Consultare la pagina MDN relativa a {{cssxref("background")}} per ulteriori informazioni sulla sintassi.

```html live-sample___background
<div class="box"></div>
```

```css live-sample___background
.box {
  width: 500px;
  height: 300px;
  padding: 0.5em;
  background:
    linear-gradient(
        105deg,
        rgb(255 255 255 / 20%) 39%,
        rgb(51 56 57 / 100%) 96%
      )
      center center / 400px 200px no-repeat,
    url("https://mdn.github.io/shared-assets/images/examples/big-star.png")
      center no-repeat,
    rebeccapurple;
}
```

{{EmbedLiveSample("background", "", "320px")}}

## Considerazioni di accessibilità sugli sfondi

Quando si posiziona testo sopra un'immagine o un colore di sfondo, è necessario assicurarsi che il testo abbia un [contrasto](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Perceivable/Color_contrast) sufficiente per essere leggibile dai visitatori. Quando si specifica un'immagine con contenuto testuale sovrapposto, è necessario specificare anche un `background-color` che consenta al testo di essere leggibile se l'immagine non viene caricata.

Gli screen reader non possono analizzare le immagini di sfondo; pertanto, queste devono essere puramente decorative. Qualsiasi contenuto importante deve far parte della pagina HTML e non essere contenuto in uno sfondo.

## Bordi

Studiando il [modello a riquadri](/it/docs/Learn_web_development/Core/Styling_basics/Box_model), è stato scoperto come i bordi influenzino la dimensione del riquadro. In questa lezione verrà esaminato come usare i bordi in modo creativo.

In genere, quando si aggiungono bordi a un elemento con CSS, si usa la proprietà abbreviata {{cssxref("border")}} per impostare in un'unica dichiarazione il colore, la larghezza e lo [stile](/it/docs/Web/CSS/Reference/Values/line-style) del bordo su tutti e quattro i lati di un riquadro:

```css
.box {
  border: 1px solid black;
}
```

Oppure è possibile indirizzare un bordo del riquadro, ad esempio:

```css
.box {
  border-top: 1px solid black;
}
```

Le proprietà individuali includono le proprietà abbreviate {{cssxref("border-width")}}, {{cssxref("border-style")}} e {{cssxref("border-color")}}:

```css
.box {
  border-width: 1px;
  border-style: solid;
  border-color: black;
}
```

Esistono anche proprietà estese per larghezza, stile e colore di ciascuno dei quattro lati:

```css
.box {
  border-top-width: 1px;
  border-top-style: solid;
  border-top-color: black;
}
```

> [!NOTE]
> Queste proprietà per i bordi superiore, destro, inferiore e sinistro hanno anche proprietà di bordo [_logiche_](/it/docs/Web/CSS/Guides/Logical_properties_and_values#properties) corrispondenti, relative alla modalità di scrittura del documento, ad esempio testo da sinistra a destra o da destra a sinistra, oppure dall'alto verso il basso. Per approfondire, consultare [gestire diverse direzioni del testo](/it/docs/Learn_web_development/Core/Styling_basics/Handling_different_text_directions).

### Sperimentare con i bordi

Esiste una varietà di stili utilizzabili per i bordi. Nell'esempio seguente, sono stati usati due stili di bordo diversi per il riquadro e due stili di bordo diversi per l'intestazione. Sperimentare con stile, larghezza e colore del bordo per osservare il funzionamento dei bordi.

```html live-sample___borders
<div class="box">
  <h2>Borders</h2>
  <p>Try changing the borders.</p>
</div>
```

```css live-sample___borders
* {
  padding: 0.2em;
}
.box {
  width: 500px;
  background-color: #567895;
  border: 5px solid #0b385f;
  border-bottom-style: dashed;
  color: white;
}

h2 {
  border-top: 2px dotted rebeccapurple;
  border-bottom: 1em double rgb(24 163 78);
}
```

{{EmbedLiveSample("borders", "", "200px")}}

## Angoli arrotondati

È possibile aggiungere angoli arrotondati a un riquadro usando la proprietà {{cssxref("border-radius")}} e le proprietà estese associate a ciascun angolo del riquadro. Come valore possono essere usate due lunghezze o percentuali: il primo valore definisce il raggio orizzontale e il secondo quello verticale. In molti casi viene passato un solo valore, che verrà usato per entrambi.

Ad esempio, per assegnare un raggio di `10px` a tutti e quattro gli angoli di un riquadro:

```css
.box {
  border-radius: 10px;
}
```

Oppure per fare in modo che l'angolo superiore destro abbia un raggio orizzontale di `1em` e un raggio verticale di `10%`:

```css
.box {
  border-top-right-radius: 1em 10%;
}
```

> [!NOTE]
> Come per le proprietà dei bordi precedenti, anche queste proprietà `border-radius` hanno proprietà `border-radius` [_logiche_](/it/docs/Web/CSS/Guides/Logical_properties_and_values#properties) corrispondenti.

### Sperimentare con il raggio del bordo

Nell'esempio seguente sono stati impostati tutti e quattro gli angoli, quindi sono stati modificati i valori dell'angolo superiore destro per renderlo diverso. È possibile sperimentare con i valori per modificare gli angoli. Consultare la pagina della proprietà {{cssxref("border-radius")}} per vedere le opzioni di sintassi disponibili. Il [generatore di `border-radius`](/it/docs/Web/CSS/Guides/Backgrounds_and_borders/Border-radius_generator) può essere usato per ottenere valori per gli angoli arrotondati.

```html live-sample___corners
<div class="box">
  <h2>Borders</h2>
  <p>Try changing the borders.</p>
</div>
```

```css live-sample___corners
.box {
  width: 500px;
  height: 110px;
  padding: 0.5em;
  border: 10px solid rebeccapurple;
  border-radius: 1em;
  border-top-right-radius: 10% 30%;
}
```

{{EmbedLiveSample("corners")}}

## Riepilogo

Come si può vedere, l'aggiunta di uno sfondo o di un bordo a un riquadro comporta molti aspetti. Esplorare le diverse pagine delle proprietà per approfondire una qualsiasi delle funzionalità trattate qui. Quasi ogni pagina su MDN contiene esempi con cui sperimentare per ampliare le proprie conoscenze.

Nel prossimo articolo verranno proposti alcuni test utilizzabili per verificare quanto siano state comprese e memorizzate le informazioni fornite sullo styling di sfondi e bordi.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Test_your_skills/Sizing", "Learn_web_development/Core/Styling_basics/Test_your_skills/Backgrounds_and_borders", "Learn_web_development/Core/Styling_basics")}}
