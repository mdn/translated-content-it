---
title: Dimensionare gli elementi in CSS
short-title: Sizing
slug: Learn_web_development/Core/Styling_basics/Sizing
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Values_and_units", "Learn_web_development/Core/Styling_basics/Backgrounds_and_borders", "Learn_web_development/Core/Styling_basics")}}

Nelle varie lezioni finora, hai incontrato diversi modi per dimensionare gli elementi su una pagina web utilizzando CSS. Comprendere le dimensioni delle varie caratteristiche del tuo design è importante. Quindi, in questa lezione riassumeremo i vari modi in cui gli elementi ottengono una dimensione tramite CSS e definirà alcuni termini relativi alle dimensioni che ti aiuteranno in futuro.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base sull'HTML (studia
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base HTML</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Getting_started">Sintassi di base CSS</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors">Selettori CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere il concetto di dimensione intrinseca.</li>
          <li>Impostare dimensioni assolute e percentuali.</li>
          <li>Impostare larghezza e altezza massime e minime.</li>
          <li>Comprendere le unità del viewport e perché sono utili.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## La dimensione naturale o intrinseca degli elementi

Gli elementi HTML hanno una dimensione naturale, impostata prima che vengano influenzati da qualsiasi CSS. Un esempio semplice è un'immagine. Un file immagine contiene informazioni sulle dimensioni, descritte come la sua **dimensione intrinseca**. Questa dimensione è determinata dall'immagine _stessa_, non da qualsiasi formattazione che potremmo applicare.

Se inserisci un'immagine su una pagina e non ne modifichi l'altezza o la larghezza, né utilizzando attributi sul tag `<img>` né tramite CSS, verrà visualizzata utilizzando quella dimensione intrinseca. Abbiamo fornito all'immagine nell'esempio sottostante un bordo in modo che tu possa vedere l'estensione della sua dimensione come definita nel suo file.

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

Una {{htmlelement("div")}} vuota, invece, non ha dimensioni proprie. Se aggiungi un {{htmlelement("div")}} al tuo HTML senza contenuto, quindi gli dai un bordo come abbiamo fatto con l'immagine, vedrai una linea sulla pagina. Questo è il bordo collassato sull'elemento — non c'è contenuto per tenerlo aperto. Nel nostro esempio sottostante, quel bordo si estende fino alla larghezza del contenitore, perché è un elemento di livello blocco, un comportamento che dovrebbe iniziare a diventare familiare. Non ha altezza (o dimensione nella dimensione del blocco) perché non c'è nessun contenuto.

```html live-sample___intrinsic-text
<div class="box"></div>
```

```css live-sample___intrinsic-text
.box {
  border: 5px solid darkblue;
}
```

{{EmbedLiveSample("intrinsic-text")}}

Nell'esempio sopra, prova ad aggiungere del testo all'interno dell'elemento vuoto. Il bordo ora contiene quel testo perché l'altezza dell'elemento è definita dal contenuto. Pertanto la dimensione di questo `<div>` nella dimensione del blocco deriva dalla dimensione del contenuto. Ancora una volta, questa è la dimensione intrinseca dell'elemento — la sua dimensione è definita dal suo contenuto.

## Impostare una dimensione specifica

Naturalmente, possiamo dare agli elementi nel nostro design una dimensione specifica. Quando una dimensione viene data a un elemento (il cui contenuto deve quindi adattarsi a quella dimensione) la si riferisce come **dimensione estrinseca**. Prendiamo il nostro `<div>` dall'esempio sopra — possiamo dargli valori specifici di {{cssxref("width")}} e {{cssxref("height")}}, e avrà ora quella dimensione indipendentemente dal contenuto inserito al suo interno. Un'altezza impostata può causare traboccamento del contenuto se c'è più contenuto di quanto l'elemento abbia spazio per contenerlo (imparerai di più sull'[overflow](/it/docs/Learn_web_development/Core/Styling_basics/Overflow) in una lezione successiva).

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

### Utilizzo delle percentuali

In molti modi, le percentuali agiscono come unità di lunghezza e come abbiamo [discusso nella lezione sui valori e unità](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#percentages), possono spesso essere utilizzate in modo intercambiabile con le lunghezze. Quando si utilizza una percentuale è necessario essere consapevoli di cosa si tratta una percentuale _di_. Nel caso di una casella all'interno di un altro contenitore, se si assegna alla casella figlia una larghezza in percentuale, sarà una percentuale della larghezza del contenitore padre.

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

Questo perché le percentuali si risolvono rispetto alla dimensione del blocco contenitore. Senza una percentuale applicata, il nostro `<div>` occuperebbe il `100%` dello spazio disponibile, in quanto è un elemento di livello blocco. Se gli assegniamo una larghezza in percentuale, questa diventa una percentuale dello spazio che normalmente riempirebbe.

### Margini e padding in percentuale

Se imposti `margins` e `padding` come percentuale, potresti notare un comportamento strano. Nell'esempio sottostante abbiamo una casella. Abbiamo dato alla casella interna un {{cssxref("margin")}} del 10% e un {{cssxref("padding")}} di `10%`. Il padding e il margine sulla parte superiore e inferiore della casella sono della stessa dimensione del padding e margine sulla sinistra e destra.

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

Potresti aspettarti, ad esempio, che i margini superiori e inferiori in percentuale siano una percentuale dell'altezza dell'elemento, mentre i margini sinistra e destra in percentuale siano una percentuale della larghezza dell'elemento. Tuttavia, non è così!

Quando si utilizzano margine e padding impostati in percentuale, il valore viene calcolato dalla **dimensione in linea** del blocco contenitore — quindi la larghezza quando si lavora in una lingua orizzontale. Nel nostro esempio, tutti i margini e padding sono `10%` della larghezza. Questo significa che puoi avere margini e padding di dimensioni uguali attorno alla casella. Questo è un fatto che vale la pena ricordare se usi le percentuali in questo modo.

## Dimensioni minime e massime

Oltre a dare agli elementi una dimensione fissa, possiamo chiedere al CSS di dare a un elemento una dimensione minima o massima. Se hai una casella che potrebbe contenere una quantità variabile di contenuto e vuoi sempre che sia _almeno_ di una certa altezza, puoi impostare la proprietà {{cssxref("min-height")}}. La casella sarà sempre almeno di questa altezza, ma crescerà in altezza se c'è più contenuto di quanto la casella abbia spazio per contenere alla sua altezza minima.

Nell'esempio sotto puoi vedere due caselle, entrambe con un `min-height` definito di 100 pixel. La casella a sinistra è alta 100 pixel; la casella a destra ha un contenuto che richiede più spazio e quindi è cresciuta oltre i 100 pixel.

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

Questo è molto utile per evitare trabocchi quando si lavora con quantità variabili di contenuto.

Un uso comune di {{cssxref("max-width")}} è quello di far ridimensionare le immagini se non c'è abbastanza spazio per visualizzarle alla loro larghezza intrinseca, assicurandosi allo stesso tempo che non diventino più grandi di quella larghezza.

Ad esempio, se imposti `width: 100%` su un'immagine e la sua larghezza intrinseca è minore del contenitore, l'immagine sarà costretta ad allungarsi e diventare più grande, causando un aspetto pixelato.

Se invece usi `max-width: 100%`, e la sua larghezza intrinseca è minore del contenitore, l'immagine non sarà costretta ad allungarsi e diventare più grande, evitando così la pixelazione.

Nell'esempio sottostante, abbiamo usato la stessa immagine tre volte. La prima immagine ha un `width: 100%` ed è in un contenitore che è più grande di essa, quindi si allunga fino alla larghezza del contenitore. La seconda immagine ha `max-width: 100%` impostato su di essa e quindi non si allunga per riempire il contenitore. La terza casella contiene la stessa immagine di nuovo, anch'essa con `max-width: 100%` impostato; in questo caso puoi vedere come si è ridimensionata per adattarsi alla casella.

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

Questa tecnica viene utilizzata per rendere le immagini _responsive_, in modo che quando vengono visualizzate su un dispositivo più piccolo si ridimensionino in modo appropriato. Tuttavia, non dovresti usare questa tecnica per caricare immagini molto grandi e poi ridimensionarle nel browser. Le immagini dovrebbero essere dimensionate in modo appropriato per non essere più grandi di quanto necessario per la dimensione più grande in cui vengono visualizzate nel design. Scaricare immagini eccessivamente grandi farà rallentare il tuo sito e potrebbe costare di più agli utenti se sono su una connessione a consumo.

## Unità del viewport

Il viewport — che è l'area visibile della tua pagina nel browser che stai usando per visualizzare un sito — ha anch'esso una dimensione. In CSS abbiamo unità che si riferiscono alla dimensione del viewport — l'unità `vw` per la larghezza del viewport e `vh` per l'altezza del viewport. Usando queste unità puoi dimensionare qualcosa in relazione al viewport dell'utente.

`1vh` è uguale al 1% dell'altezza del viewport e `1vw` è uguale al 1% della larghezza del viewport. Puoi usare queste unità per dimensionare le caselle, ma anche il testo. Nell'esempio sottostante abbiamo una casella che è dimensionata come 20vh e 20vw. La casella contiene una lettera `A`, a cui è stato dato un {{cssxref("font-size")}} di 10vh.

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

Se cambi i valori `vh` e `vw` cambierà la dimensione della casella o del font; cambiare la dimensione del viewport modificherà anche le loro dimensioni perché sono dimensionati in relazione al viewport. Per vedere l'esempio cambiare quando modifichi la dimensione del viewport dovrai caricare l'esempio in una nuova finestra del browser che puoi ridimensionare (poiché l'`<iframe>` incorporato che contiene l'esempio mostrato sopra è il suo viewport). Apri l'esempio, ridimensiona la finestra del browser e osserva cosa succede alla dimensione della casella e del testo.

Dimensionare le cose secondo il viewport può essere utile nei tuoi design. Ad esempio, se vuoi una sezione hero a tutta pagina da mostrare prima del resto del tuo contenuto, rendere quella parte della tua pagina alta `100vh` spingerà il resto del contenuto sotto il viewport, significando che apparirà solo quando il documento viene scorrimento.

## Metti alla prova le tue capacità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare di aver conservato queste informazioni prima di procedere — vedi [Metti alla prova le tue capacità: Dimensionamento](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Sizing).

## Riepilogo

Questa lezione ti ha fornito un riassunto di alcune questioni chiave che potresti incontrare nel dimensionamento degli elementi sul web. Quando passerai a [CSS Layout](/it/docs/Learn_web_development/Core/CSS_layout), il dimensionamento diventerà molto importante per padroneggiare i diversi metodi di layout, quindi vale la pena comprendere i concetti qui prima di procedere.

Nel prossimo articolo, daremo un'occhiata a come gli sfondi e i bordi vengono manipolati in CSS.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Values_and_units", "Learn_web_development/Core/Styling_basics/Backgrounds_and_borders", "Learn_web_development/Core/Styling_basics")}}
