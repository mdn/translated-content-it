---
title: Dimensionamento degli elementi in CSS
short-title: Sizing
slug: Learn_web_development/Core/Styling_basics/Sizing
l10n:
  sourceCommit: 38397b7418708bd0a7c5ee8e69b16e985c85de33
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Test_your_skills/Values", "Learn_web_development/Core/Styling_basics/Test_your_skills/Sizing", "Learn_web_development/Core/Styling_basics")}}

Nelle varie lezioni affrontate finora, sono stati illustrati diversi modi per dimensionare gli elementi di una pagina web usando CSS. Comprendere quanto saranno grandi le diverse caratteristiche di un design è importante. In questa lezione verranno quindi riepilogati i vari modi in cui gli elementi ottengono una dimensione tramite CSS e definiti alcuni termini relativi al dimensionamento che saranno utili in futuro.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Fondamenti di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Getting_started">Sintassi CSS di base</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors">Selettori CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere il concetto di dimensione intrinseca.</li>
          <li>Impostare dimensioni assolute e percentuali.</li>
          <li>Impostare larghezza e altezza massime e minime.</li>
          <li>Comprendere le unità viewport e perché sono utili.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## La dimensione naturale o intrinseca degli elementi

Gli elementi HTML hanno una dimensione naturale, impostata prima che siano influenzati da qualsiasi CSS. Un esempio semplice è un'immagine. Un file immagine contiene informazioni sulle dimensioni, descritte come **dimensione intrinseca**. Questa dimensione è determinata dall'immagine _stessa_, non da alcuna formattazione applicata.

Se si inserisce un'immagine in una pagina e non se ne modifica l'altezza o la larghezza, né tramite attributi `<img>` né tramite CSS, verrà visualizzata usando quella dimensione intrinseca. All'immagine nell'esempio seguente è stato assegnato un bordo, così da poter vedere l'estensione della sua dimensione come definita nel file.

```html live-sample___intrinsic-image
<img
  alt="star"
  src="https://mdn.github.io/shared-assets/images/examples/big-star.png" />
```

```css live-sample___intrinsic-image
img {
  border: 5px solid darkblue;
}
```

{{EmbedLiveSample("intrinsic-image","100%", "80")}}

Un {{htmlelement("div")}} vuoto, invece, non ha alcuna dimensione propria. Se si aggiunge un {{htmlelement("div")}} all'HTML senza contenuto e poi gli si assegna un bordo, come fatto con l'immagine, nella pagina verrà visualizzata una linea. Questo è il bordo collassato del `<div>`: non esiste alcun contenuto che lo mantenga aperto.

Nell'esempio seguente, quel bordo copre l'intera larghezza del contenitore poiché è un elemento a livello di blocco, un comportamento che dovrebbe iniziare a essere familiare. Non ha altezza (o dimensione nella direzione del blocco) perché non ha contenuto.

```html live-sample___intrinsic-text
<div class="box"></div>
```

```css live-sample___intrinsic-text
.box {
  border: 5px solid darkblue;
}
```

{{EmbedLiveSample("intrinsic-text","100%", "60")}}

Nell'esempio precedente, provare ad aggiungere del testo all'interno dell'elemento vuoto. Il bordo si aprirà perché l'altezza dell'elemento è definita dal contenuto. Anche in questo caso, si tratta della dimensione intrinseca dell'elemento: la sua dimensione è definita dal contenuto.

## Impostare una dimensione specifica

Naturalmente, è possibile assegnare agli elementi di un design una dimensione specifica. Quando a un elemento viene assegnata una dimensione, nella quale deve poi adattarsi il suo contenuto, si parla di **dimensione estrinseca**.

Nell'esempio successivo, a due `<div>` vengono assegnati valori specifici per {{cssxref("width")}} e {{cssxref("height")}}, e avranno quindi tale dimensione indipendentemente dal contenuto inserito al loro interno. Come dimostra il `<div>` a destra, un'altezza impostata può causare l'overflow del contenuto se è presente più contenuto di quanto possa entrare nell'elemento contenitore (si approfondirà [l'overflow](/it/docs/Learn_web_development/Core/Styling_basics/Overflow) in una lezione successiva).

```html live-sample___height
<div class="wrapper">
  <div class="box"></div>
  <div class="box">
    These boxes both have a height set, this box has content in it which will
    need more space than the assigned height, and so we get overflow.
  </div>
</div>
```

```css live-sample___height
body {
  font: 1.2em sans-serif;
}
.wrapper {
  display: flex;
}

.wrapper > * {
  margin: 20px;
}

.box {
  border: 5px solid darkblue;
  height: 100px;
  width: 200px;
}
```

{{EmbedLiveSample("height", "", "200px")}}

A causa di questo problema di overflow, impostare l'altezza degli elementi con lunghezze o percentuali è un'operazione da eseguire con molta attenzione sul web.

### Usare le percentuali

Per molti aspetti, le percentuali si comportano come unità di lunghezza e, come [illustrato nella lezione sui valori e sulle unità](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#percentages), spesso possono essere usate in modo intercambiabile con le lunghezze. Quando si usa una percentuale, occorre sapere rispetto a cosa rappresenta una percentuale. Nel caso di un riquadro all'interno di un altro contenitore, assegnando al riquadro figlio una larghezza percentuale, questa sarà una percentuale della larghezza del contenitore padre.

```html live-sample___percent-width
<div class="container">
  <div class="box">I have a percentage width.</div>
</div>
```

```css live-sample___percent-width
body {
  font: 1.2em sans-serif;
}

.box {
  border: 5px solid darkblue;
  width: 50%;
}
```

{{EmbedLiveSample("percent-width")}}

Questo avviene perché le percentuali vengono risolte rispetto alla dimensione del blocco contenitore. Senza alcuna percentuale applicata, il `<div>` `box` occupa il `100%` dello spazio disponibile, poiché è un elemento a livello di blocco. Se gli viene assegnata una larghezza percentuale, questa diventa una percentuale dello spazio che normalmente riempirebbe.

Provare a modificare l'esempio precedente:

1. Rimuovere la dichiarazione `width` del `<div>` `box` per verificare che, per impostazione predefinita, occupi il `100%` della `width` disponibile.
2. Ripristinare la modifica precedente: assegnare di nuovo al `<div>` `box` una `width` di `50%`.
3. Ora assegnare al `<div>` `container` una `width` di `50%`. La `width` del `<div>` `box` diventerà più piccola, perché è relativa alla `width` del suo contenitore.

### Margini e padding percentuali

Se si impostano `margins` e `padding` come percentuali, potrebbe essere osservato un comportamento insolito.

Nell'esempio seguente è presente un riquadro, al quale sono stati assegnati un {{cssxref("margin")}} del 10% e un {{cssxref("padding")}} del `10%`. Il padding e il margine nella parte superiore e inferiore del riquadro hanno la stessa dimensione del padding e del margine a sinistra e a destra.

```html live-sample___percent-mp
<div class="box">I have margin and padding set to 10% on all sides.</div>
```

```css live-sample___percent-mp
body {
  font: 1.2em sans-serif;
}
.box {
  border: 5px solid darkblue;
  width: 200px;
  margin: 10%;
  padding: 10%;
}
```

{{EmbedLiveSample("percent-mp", "", "380px")}}

Ci si potrebbe aspettare che i margini percentuali superiore e inferiore siano una percentuale dell'altezza dell'elemento e che i margini percentuali sinistro e destro siano una percentuale della larghezza dell'elemento. Tuttavia, non è così.

Quando si usano margin e padding impostati in percentuale, il valore viene calcolato a partire dalla **dimensione inline** del blocco contenitore, quindi dalla larghezza quando si lavora in una lingua orizzontale. Nell'esempio, tutti i margini e i padding sono il `10%` della larghezza. Questo significa che è possibile avere margini e padding della stessa dimensione su tutti i lati del riquadro. È un fatto da ricordare se si usano le percentuali in questo modo.

## Dimensioni minime e massime

Oltre a fornire agli elementi una dimensione fissa, è possibile chiedere a CSS di assegnare a un elemento una dimensione minima o massima. Se è presente un riquadro che potrebbe contenere una quantità variabile di contenuto e si desidera che abbia sempre _almeno_ una certa altezza, è possibile impostare su di esso la proprietà {{cssxref("min-height")}}. Il riquadro avrà sempre almeno questa altezza, ma aumenterà poi in altezza se è presente più contenuto di quanto spazio disponibile alla sua altezza minima.

Nell'esempio successivo sono visibili due riquadri, entrambi con un `min-height` definito di 100 pixel. Il riquadro a sinistra è alto 100 pixel; il riquadro a destra contiene contenuto che richiede più spazio e, pertanto, è diventato più alto di 100 pixel.

```html live-sample___min-height
<div class="wrapper">
  <div class="box"></div>
  <div class="box">
    These boxes both have a min-height set. This box has content in it, which
    will need more space than the assigned height, and so it grows from the
    minimum.
  </div>
</div>
```

```css live-sample___min-height
body {
  font: 1.2em sans-serif;
}
.wrapper {
  display: flex;
  align-items: flex-start;
}

.wrapper > * {
  margin: 20px;
}

.box {
  border: 5px solid darkblue;
  min-height: 100px;
  width: 200px;
}
```

{{EmbedLiveSample("min-height", "", "220px")}}

Questo è molto utile per evitare l'overflow quando si gestiscono quantità variabili di contenuto.

### `max-width` sulle immagini

Un uso comune di {{cssxref("max-width")}} consiste nel fare in modo che le immagini si ridimensionino verso il basso quando non c'è spazio sufficiente per visualizzarle alla loro larghezza intrinseca, assicurandosi al tempo stesso che non diventino più grandi di tale larghezza.

Ad esempio, se si impostasse `width: 100%` su un'immagine la cui larghezza intrinseca è minore del suo contenitore, l'immagine verrebbe forzata ad allungarsi e a diventare più grande, assumendo un aspetto pixelato.

Se invece si usa `max-width: 100%` e la larghezza intrinseca dell'immagine è minore del suo contenitore, l'immagine non verrà forzata ad allungarsi e a diventare più grande, evitando così la pixelazione.

Nell'esempio seguente, la stessa immagine è stata incorporata tre volte:

- Alla prima immagine è stato assegnato `width: 100%` e si trova in un contenitore più grande di essa; pertanto, si allunga fino alla larghezza del contenitore.
- Alla seconda immagine è impostato `max-width: 100%` e quindi non si allunga per riempire il contenitore.
- Il terzo riquadro contiene nuovamente la stessa immagine, anch'essa con `max-width: 100%` impostato; in questo caso è possibile vedere come si sia ridimensionata verso il basso per adattarsi al riquadro.

```html live-sample___max-width
<div class="wrapper">
  <div class="box">
    <img
      alt="star"
      class="width"
      src="https://mdn.github.io/shared-assets/images/examples/big-star.png" />
  </div>
  <div class="box">
    <img
      alt="star"
      class="max"
      src="https://mdn.github.io/shared-assets/images/examples/big-star.png" />
  </div>
  <div class="mini-box">
    <img
      alt="star"
      class="max"
      src="https://mdn.github.io/shared-assets/images/examples/big-star.png" />
  </div>
</div>
```

```css hidden live-sample___max-width
.wrapper {
  display: flex;
  align-items: flex-start;
}

.wrapper > * {
  margin: 20px;
}

.box,
.mini-box {
  border: 5px solid darkblue;
}
```

```css live-sample___max-width
.box {
  width: 200px;
}
.mini-box {
  width: 30px;
}
.width {
  width: 100%;
}
.max {
  max-width: 100%;
}
```

{{EmbedLiveSample("max-width", "", "260px")}}

Questa tecnica viene usata per rendere le immagini _responsive_, affinché vengano ridimensionate adeguatamente verso il basso quando visualizzate su un dispositivo più piccolo. Tuttavia, questa tecnica non dovrebbe essere usata per caricare immagini molto grandi e poi ridimensionarle nel browser. Le immagini dovrebbero avere dimensioni appropriate, senza essere più grandi del necessario per la dimensione massima alla quale vengono visualizzate nel design. Scaricare immagini eccessivamente grandi renderà il sito lento e può costare di più agli utenti che pagano i dati a megabyte.

## Unità viewport

La viewport, ovvero l'area visibile della pagina nel browser usato per visualizzare un sito, ha anch'essa una dimensione. In CSS sono disponibili unità relative alla dimensione della viewport: l'unità `vw` per la larghezza della viewport e `vh` per l'altezza della viewport. Usando queste unità, è possibile dimensionare qualcosa in relazione alla viewport dell'utente.

`1vh` equivale all'`1%` dell'altezza della viewport e `1vw` equivale all'`1%` della larghezza della viewport. Queste unità possono essere usate per dimensionare i riquadri, ma anche il testo. Nell'esempio seguente è presente un riquadro dimensionato con `20vh` e `20vw`. Il riquadro contiene una lettera `A`, alla quale è stato assegnato un {{cssxref("font-size")}} di `10vh`.

```html live-sample___vw-vh
<div class="box">A</div>
```

```css live-sample___vw-vh
body {
  font-family: sans-serif;
}

.box {
  border: 5px solid darkblue;
  width: 20vw;
  height: 20vh;
  font-size: 10vh;
}
```

{{EmbedLiveSample("vw-vh")}}

La modifica dei valori `vh` e `vw` cambierà rispettivamente la dimensione del riquadro e del carattere; anche la modifica della dimensione della viewport cambierà le dimensioni del riquadro e del carattere, poiché sono dimensionati in relazione alla viewport. Per osservare il cambiamento della dimensione del riquadro e del testo regolando la dimensione della viewport, {{LiveSampleLink("vw-vh", "caricare l'esempio in una nuova scheda")}} e ridimensionare la finestra del browser.

Dimensionare gli elementi in base alla viewport può essere utile nei design. Ad esempio, se si desidera mostrare un banner a pagina intera prima del resto del contenuto, rendere quella parte della pagina alta `100vh` spingerà il resto del contenuto sotto la viewport, facendo sì che venga visualizzato solo dopo lo scorrimento del documento.

## Riepilogo

Questa lezione ha fornito una panoramica di alcuni aspetti chiave che possono presentarsi durante il dimensionamento degli elementi sul web. Passando a [CSS Layout](/it/docs/Learn_web_development/Core/CSS_layout), il dimensionamento diventerà molto importante per padroneggiare i diversi metodi di layout; vale quindi la pena comprendere questi concetti prima di proseguire.

Nel prossimo articolo verranno proposti alcuni test che possono essere usati per verificare quanto bene siano state comprese e memorizzate le informazioni fornite sul dimensionamento in CSS.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Test_your_skills/Values", "Learn_web_development/Core/Styling_basics/Test_your_skills/Sizing", "Learn_web_development/Core/Styling_basics")}}
