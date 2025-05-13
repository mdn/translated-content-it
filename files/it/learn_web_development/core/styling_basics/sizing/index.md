---
title: Dimensionare gli elementi in CSS
short-title: Sizing
slug: Learn_web_development/Core/Styling_basics/Sizing
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Values_and_units", "Learn_web_development/Core/Styling_basics/Backgrounds_and_borders", "Learn_web_development/Core/Styling_basics")}}

Nelle varie lezioni finora, ha incontrato diversi modi per dimensionare elementi su una pagina web usando CSS. Comprendere quanto grandi saranno le diverse caratteristiche nel suo design è importante. Quindi, in questa lezione riassumeremo i vari modi in cui gli elementi ottengono una dimensione tramite CSS e definiremo alcuni termini relativi alle dimensioni che le saranno utili in futuro.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Basi di HTML (studi
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax">Sintassi di base di HTML</a>), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Getting_started">Sintassi di base di CSS</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors">Selettori CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere il concetto di dimensione intrinseca.</li>
          <li>Impostare dimensioni assolute e percentuali.</li>
          <li>Impostare larghezze e altezze massime e minime.</li>
          <li>Comprendere le unità di visualizzazione e perché sono utili.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## La dimensione naturale o intrinseca delle cose

Gli elementi HTML hanno una dimensione naturale, impostata prima che siano influenzati da qualunque CSS. Un esempio semplice è un'immagine. Un file immagine contiene informazioni sulle dimensioni, descritte come **dimensione intrinseca**. Questa dimensione è determinata dall'immagine _stessa_, non da alcun formato che possiamo applicare.

Se si posiziona un'immagine su una pagina e non se ne cambia l'altezza o la larghezza, tramite attributi nel tag `<img>` o CSS, sarà visualizzata utilizzando quella dimensione intrinseca. Abbiamo dato all'immagine nell'esempio seguente un bordo in modo da poter vedere l'estensione della sua dimensione come definita nel suo file.

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

{{EmbedLiveSample("intrinsic-image")}}

Un {{htmlelement("div")}} vuoto, d'altra parte, non ha dimensione propria. Se aggiunge un {{htmlelement("div")}} al suo HTML senza contenuto e gli assegna un bordo come abbiamo fatto con l'immagine, vedrà una linea sulla pagina. Questo è il bordo collassato sull'elemento — non c'è contenuto per tenerlo aperto. Nel nostro esempio sotto, quel bordo si estende alla larghezza del contenitore, perché è un elemento di livello blocco, un comportamento che dovrebbe cominciare a diventare familiare. Non ha altezza (o dimensione nella direzione del blocco) perché non c'è contenuto.

```html live-sample___intrinsic-text
<div class="box"></div>
```

```css live-sample___intrinsic-text
.box {
  border: 5px solid darkblue;
}
```

{{EmbedLiveSample("intrinsic-text")}}

Nell'esempio sopra, provi ad aggiungere del testo all'interno dell'elemento vuoto. Il bordo ora contiene quel testo perché l'altezza dell'elemento è definita dal contenuto. Pertanto la dimensione di questo `<div>` nella dimensione del blocco deriva dalla dimensione del contenuto. Ancora una volta, questa è la dimensione intrinseca dell'elemento — la sua dimensione è definita dal suo contenuto.

## Impostare una dimensione specifica

Possiamo, naturalmente, dare agli elementi nel nostro design una dimensione specifica. Quando a un elemento viene assegnata una dimensione specifica (il cui contenuto deve quindi adattarsi a tale dimensione) ci riferiamo a essa come una **dimensione estrinseca**. Prenda il nostro `<div>` dall'esempio sopra — possiamo assegnargli valori specifici di {{cssxref("width")}} e {{cssxref("height")}}, e avrà quella dimensione qualunque sia il contenuto inserito all'interno. Un'altezza impostata può causare un traboccamento di contenuto se c'è più contenuto di quanto l'elemento abbia spazio per inserirlo (imparerà di più su [overflow](/it/docs/Learn_web_development/Core/Styling_basics/Overflow) in una lezione successiva).

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

A causa di questo problema di traboccamento, fissare l'altezza degli elementi con lunghezze o percentuali è qualcosa che dobbiamo fare con molta attenzione sul web.

### Usare le percentuali

In molti modi, le percentuali si comportano come le unità di lunghezza, e come abbiamo [discusso nella lezione sui valori e unità](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#percentages), possono spesso essere utilizzate in modo intercambiabile con le lunghezze. Quando si utilizza una percentuale deve essere consapevole di cosa sia una percentuale _di_. Nel caso di una casella all'interno di un altro contenitore, se dà alla casella figlio una larghezza percentuale sarà una percentuale della larghezza del contenitore padre.

```html live-sample___percent-width
<div class="box">I have a percentage width.</div>
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

Questo perché le percentuali si risolvono rispetto alla dimensione del blocco contenitore. Senza una percentuale applicata, il nostro `<div>` occuperebbe `100%` dello spazio disponibile, in quanto è un elemento di livello blocco. Se gli dà una larghezza percentuale, diventa una percentuale dello spazio che normalmente riempirebbe.

### Margini e padding percentuali

Se imposta `margins` e `padding` come percentuale, potrebbe notare comportamenti strani. Nell'esempio sottostante abbiamo una casella. Abbiamo dato alla casella interna un {{cssxref("margin")}} del 10% e un {{cssxref("padding")}} di `10%`. Il padding e il margine in alto e in basso della casella sono della stessa dimensione del padding e del margine a sinistra e a destra.

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

Potrebbe aspettarsi, per esempio, che i margini in alto e in basso in percentuale siano una percentuale dell'altezza dell'elemento, e i margini a sinistra e a destra in percentuale siano una percentuale della larghezza dell'elemento. Tuttavia, non è così!

Quando si utilizzano margini e padding impostati in percentuale, il valore è calcolato dalla **dimensione in linea** del blocco contenitore — quindi la larghezza quando si lavora in una lingua orizzontale. Nel nostro esempio, tutti i margini e i padding sono `10%` della larghezza. Ciò significa che può avere margini e padding di dimensioni uguali tutto intorno alla casella. Questo è un fatto da ricordare se utilizza le percentuali in questo modo.

## min- e max- sizes

Oltre a dare una dimensione fissa alle cose, possiamo chiedere a CSS di dare a un elemento una dimensione minima o massima. Se ha una casella che può contenere una quantità variabile di contenuto, e desidera sempre che sia _almeno_ di una certa altezza, potrebbe impostare la proprietà {{cssxref("min-height")}} su di essa. La casella sarà sempre almeno di questa altezza, ma crescerà più alta se c'è più contenuto di quanto la casella abbia spazio per alla sua altezza minima.

Nell'esempio sotto può vedere due caselle, entrambe con una `min-height` definita di 100 pixel. La casella a sinistra è alta 100 pixel; la casella a destra ha un contenuto che necessita di più spazio, quindi è cresciuta più alta di 100 pixel.

```html live-sample___min-height
<div class="wrapper">
  <div class="box"></div>
  <div class="box">
    These boxes both have a min-height set, this box has content in it which
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

Questo è molto utile per evitare il traboccamento quando si gestiscono quantità variabili di contenuto.

Un uso comune di {{cssxref("max-width")}} è far sì che le immagini si ridimensionino verso il basso se non c'è abbastanza spazio per mostrarle alla loro larghezza intrinseca, assicurandosi che non diventino più grandi di quella larghezza.

Ad esempio, se dovesse impostare `width: 100%` su un'immagine, e la sua larghezza intrinseca fosse più piccola del suo contenitore, l'immagine verrebbe forzata a espandersi e diventare più grande, facendola apparire pixelata.

Se invece usa `max-width: 100%`, e la sua larghezza intrinseca è più piccola del suo contenitore, l'immagine non sarà forzata ad espandersi e diventare più grande, prevenendo così la pixelizzazione.

Nell'esempio sotto, abbiamo usato la stessa immagine tre volte. La prima immagine è stata data `width: 100%` ed è in un contenitore più grande di essa, quindi si espande alla larghezza del contenitore. La seconda immagine ha `max-width: 100%` impostato su di essa e quindi non si espande per riempire il contenitore. La terza casella contiene nuovamente la stessa immagine, con `max-width: 100%` impostato; in questo caso può vedere come si è ridimensionata per adattarsi alla casella.

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
  width: 50px;
}
.width {
  width: 100%;
}
.max {
  max-width: 100%;
}
```

{{EmbedLiveSample("max-width", "", "260px")}}

Questa tecnica è utilizzata per rendere le immagini _responsive_, in modo che quando vengono visualizzate su un dispositivo più piccolo si ridimensionino correttamente. Tuttavia, non dovrebbe usare questa tecnica per caricare immagini veramente grandi e poi ridimensionarle nel browser. Le immagini devono essere dimensionate in modo appropriato per non essere più grandi di quanto sia necessario per la dimensione massima in cui vengono visualizzate nel design. Scaricare immagini esageratamente grandi rallenterà il suo sito e potrebbe costare di più agli utenti se sono su una connessione a consumo.

## Unità di viewport

Anche il viewport — che è l'area visibile della sua pagina nel browser che sta utilizzando per visualizzare un sito — ha una dimensione. In CSS abbiamo unità che si riferiscono alla dimensione del viewport — l'unità `vw` per la larghezza del viewport, e `vh` per l'altezza del viewport. Usando queste unità può dimensionare qualcosa in relazione al viewport dell'utente.

`1vh` è uguale al 1% dell'altezza del viewport, e `1vw` è uguale al 1% della larghezza del viewport. Può usare queste unità per dimensionare delle caselle, ma anche del testo. Nell'esempio sotto abbiamo una casella che è dimensionata come 20vh e 20vw. La casella contiene una lettera `A`, a cui è stata assegnata una {{cssxref("font-size")}} di 10vh.

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

Se cambia i valori `vh` e `vw` modificherà la dimensione della casella o del font; cambiare la dimensione del viewport cambierà anche le loro dimensioni perché sono dimensionate in relazione al viewport. Per vedere l'esempio cambiare quando cambia la dimensione del viewport dovrà caricare l'esempio in una nuova finestra del browser che può ridimensionare (poiché l'`<iframe>` incorporato che contiene l'esempio mostrato sopra è il suo viewport). Apra l'esempio, ridimensioni la finestra del browser e osservi cosa accade alla dimensione della casella e del testo.

Dimensionare le cose in base al viewport può essere utile nei suoi design. Ad esempio, se desidera una sezione hero a tutta pagina che venga mostrata prima del resto del contenuto, rendendo quella parte della sua pagina alta `100vh` spingerà il resto del contenuto sotto il viewport, significando che apparirà solo una volta che il documento viene scorrevole verso il basso.

## Metti alla prova le sue abilità!

Ha raggiunto la fine di questo articolo, ma può ricordare le informazioni più importanti? Può trovare alcuni ulteriori test per verificare di aver trattenuto queste informazioni prima di passare avanti — veda [Metti alla prova le sue abilità: Dimensionare](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Sizing).

## Riassunto

Questa lezione le ha fornito una panoramica di alcune problematiche chiave che potrebbe incontrare quando dimensiona elementi sul web. Quando procederà con il [Layout CSS](/it/docs/Learn_web_development/Core/CSS_layout), il dimensionamento diventerà molto importante nel padroneggiare i diversi metodi di layout, quindi vale la pena comprendere i concetti qui prima di proseguire.

Nel prossimo articolo, daremo un'occhiata a come sfondi e bordi vengono manipolati in CSS.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Values_and_units", "Learn_web_development/Core/Styling_basics/Backgrounds_and_borders", "Learn_web_development/Core/Styling_basics")}}
