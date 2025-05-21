---
title: Stilizzare le liste
slug: Learn_web_development/Core/Text_styling/Styling_lists
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Fundamentals", "Learn_web_development/Core/Text_styling/Styling_links", "Learn_web_development/Core/Text_styling")}}

Le [Liste](/it/docs/Learn_web_development/Core/Structuring_content/Lists) si comportano come qualsiasi altro testo per la maggior parte, ma ci sono alcune proprietà CSS specifiche per le liste che è necessario conoscere, nonché alcune buone pratiche da considerare. Questo articolo spiega tutto.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare il contenuto con HTML</a
        > e
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi dello stile CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Distanziamento degli elementi della lista, ad esempio con margine o altezza della linea.</li>
          <li>Utilizzo delle proprietà <code>list-style</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Un semplice esempio di lista

Per cominciare, diamo un'occhiata a un semplice esempio di lista. In tutto l'articolo esamineremo liste non ordinate, ordinate e di descrizioni — tutte hanno caratteristiche di stile simili, oltre ad alcune particolari. L'esempio non stilizzato è [disponibile su GitHub](https://mdn.github.io/learning-area/css/styling-text/styling-lists/unstyled-list.html) (consulta anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/css/styling-text/styling-lists/unstyled-list.html)).

L'HTML per il nostro esempio di lista è il seguente:

```html
<h2>Shopping (unordered) list</h2>

<p>
  Paragraph for reference, paragraph for reference, paragraph for reference,
  paragraph for reference, paragraph for reference, paragraph for reference.
</p>

<ul>
  <li>Hummus</li>
  <li>Pita</li>
  <li>Green salad</li>
  <li>Halloumi</li>
</ul>

<h2>Recipe (ordered) list</h2>

<p>
  Paragraph for reference, paragraph for reference, paragraph for reference,
  paragraph for reference, paragraph for reference, paragraph for reference.
</p>

<ol>
  <li>Toast pita, leave to cool, then slice down the edge.</li>
  <li>
    Fry the halloumi in a shallow, non-stick pan, until browned on both sides.
  </li>
  <li>Wash and chop the salad.</li>
  <li>Fill pita with salad, hummus, and fried halloumi.</li>
</ol>

<h2>Ingredient description list</h2>

<p>
  Paragraph for reference, paragraph for reference, paragraph for reference,
  paragraph for reference, paragraph for reference, paragraph for reference.
</p>

<dl>
  <dt>Hummus</dt>
  <dd>
    A thick dip/sauce generally made from chick peas blended with tahini, lemon
    juice, salt, garlic, and other ingredients.
  </dd>
  <dt>Pita</dt>
  <dd>A soft, slightly leavened flatbread.</dd>
  <dt>Halloumi</dt>
  <dd>
    A semi-hard, unripened, brined cheese with a higher-than-usual melting
    point, usually made from goat/sheep milk.
  </dd>
  <dt>Green salad</dt>
  <dd>That green healthy stuff that many of us just use to garnish kebabs.</dd>
</dl>
```

Se ora vai all'esempio dal vivo e indaghi sugli elementi della lista usando gli [strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools), noterai un paio di valori predefiniti di stile:

- Gli elementi {{htmlelement("ul")}} e {{htmlelement("ol")}} hanno un {{cssxref("margin")}} superiore e inferiore di `16px` (`1em`) e un {{cssxref("padding-left")}} di `40px` (`2.5em`). Se l'attributo di direzionalità [`dir`](/it/docs/Web/HTML/Reference/Global_attributes/dir) è impostato su destra a sinistra (`rtl`) per gli elementi `ul` e `ol`, in quel caso il {{cssxref("padding-right")}} entra in gioco e il suo valore predefinito è `40px` (`2.5em`).
- Gli elementi della lista (elementi {{htmlelement("li")}}) non hanno valori predefiniti impostati per il distanziamento.
- L'elemento {{htmlelement("dl")}} ha un {{cssxref("margin")}} superiore e inferiore di `16px` (`1em`), ma non è impostato alcun padding.
- Gli elementi {{htmlelement("dd")}} hanno un {{cssxref("margin-left")}} di `40px` (`2.5em`).
- Gli elementi {{htmlelement("p")}} che abbiamo incluso come riferimento hanno un {{cssxref("margin")}} superiore e inferiore di `16px` (`1em`) — lo stesso dei diversi tipi di lista.

## Gestione del distanziamento delle liste

Quando si stilizzano le liste, è necessario regolare i loro stili in modo che mantengano lo stesso distanziamento verticale degli elementi circostanti (come paragrafi e immagini; a volte chiamato ritmo verticale) e lo stesso distanziamento orizzontale fra di loro (puoi vedere l'[esempio stilizzato finale](https://mdn.github.io/learning-area/css/styling-text/styling-lists/) su GitHub, e [trova il codice sorgente](https://github.com/mdn/learning-area/blob/main/css/styling-text/styling-lists/index.html) anche).

Il CSS utilizzato per lo stile del testo e il distanziamento è il seguente:

```css
/* General styles */

html {
  font-family: Helvetica, Arial, sans-serif;
  font-size: 10px;
}

h2 {
  font-size: 2rem;
}

ul,
ol,
dl,
p {
  font-size: 1.5rem;
}

li,
p {
  line-height: 1.5;
}

/* Description list styles */

dd,
dt {
  line-height: 1.5;
}

dt {
  font-weight: bold;
}
```

- La prima regola imposta un font universalmente e una dimensione del font di base di 10px. Questi sono ereditati da tutto sulla pagina.
- Le regole 2 e 3 impostano dimensioni relative del font per i titoli, i diversi tipi di liste (i figli degli elementi delle liste ereditano questi), e i paragrafi. Ciò significa che ogni paragrafo e lista avrà la stessa dimensione del font e spaziatura superiore e inferiore, aiutando a mantenere il ritmo verticale coerente.
- La regola 4 imposta lo stesso {{cssxref("line-height")}} sui paragrafi e sugli elementi della lista — quindi i paragrafi e ogni singolo elemento della lista avranno lo stesso spazio tra le linee. Anche questo aiuterà a mantenere il ritmo verticale coerente.
- Le regole 5 e 6 si applicano alla lista di descrizione. Impostiamo lo stesso `line-height` nei termini e nelle descrizioni della lista di descrizione come abbiamo fatto con i paragrafi e gli elementi della lista. Ancora una volta, la coerenza è buona! Facciamo anche in modo che i termini della descrizione abbiano un font in grassetto, in modo che si distinguano visivamente più facilmente.

## Stili specifici per le liste

Ora che abbiamo esaminato le tecniche generali di distanziamento per le liste, esploriamo alcune proprietà specifiche per le liste. Ci sono tre proprietà che dovresti conoscere per iniziare, che possono essere impostate sugli elementi {{htmlelement("ul")}} o {{htmlelement("ol")}}:

- {{cssxref("list-style-type")}}: Imposta il tipo di punti elenco da utilizzare per la lista, ad esempio punti quadrati o cerchio per una lista non ordinata, o numeri, lettere o numeri romani per una lista ordinata.
- {{cssxref("list-style-position")}}: Imposta se i punti elenco, all'inizio di ogni elemento, appaiono all'interno o all'esterno delle liste.
- {{cssxref("list-style-image")}}: Permette di utilizzare un'immagine personalizzata per il punto elenco, invece di un semplice quadrato o cerchio.

### Stili dei punti elenco

Come menzionato sopra, la proprietà {{cssxref("list-style-type")}} consente di impostare il tipo di punto da utilizzare per i punti elenco. Nel nostro esempio, abbiamo impostato la lista ordinata per utilizzare numeri romani maiuscoli con:

```css
ol {
  list-style-type: upper-roman;
}
```

Questo ci dà il seguente aspetto:

![una lista ordinata con i punti elenco impostati per apparire fuori dal testo dell'elemento della lista.](outer-bullets.png)

Puoi trovare molte più opzioni consultando la pagina di riferimento {{cssxref("list-style-type")}}.

### Posizione del punto elenco

La proprietà {{cssxref("list-style-position")}} imposta se i punti elenco appaiono all'interno degli elementi della lista o all'esterno di essi prima dell'inizio di ogni elemento. Il valore predefinito è `outside`, il che fa sì che i punti elenco si trovino fuori dagli elementi della lista, come visto sopra.

Se imposti il valore su `inside`, i punti elenco si troveranno all'interno delle linee:

```css
ol {
  list-style-type: upper-roman;
  list-style-position: inside;
}
```

![una lista ordinata con i punti elenco impostati per apparire all'interno del testo dell'elemento della lista.](inner-bullets.png)

### Usare un'immagine personalizzata per il punto elenco

La proprietà {{cssxref("list-style-image")}} ti permette di usare un'immagine personalizzata per il tuo punto elenco. La sintassi è abbastanza semplice:

```css
ul {
  list-style-image: url(star.svg);
}
```

Tuttavia, questa proprietà è un po' limitata in termini di controllo della posizione, della dimensione, ecc. dei punti elenco. È meglio utilizzare la famiglia di proprietà {{cssxref("background")}}, che hai imparato nella nostra precedente lezione su [Sfondi e bordi](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders).

Nel nostro esempio finale, abbiamo stilizzato la lista non ordinata in questo modo (sopra a ciò che hai già visto sopra):

```css
ul {
  padding-left: 2rem;
  list-style-type: none;
}

ul li {
  padding-left: 2rem;
  background-image: url(star.svg);
  background-position: 0 0;
  background-size: 1.6rem 1.6rem;
  background-repeat: no-repeat;
}
```

Qui abbiamo fatto quanto segue:

- Impostato il {{cssxref("padding-left")}} del {{htmlelement("ul")}} da `40px` predefinito a `20px`, quindi impostato lo stesso quantitativo sugli elementi delle liste. Questo è in modo che, complessivamente, gli elementi delle liste siano ancora allineati con gli elementi delle liste ordinate e le descrizioni delle liste di descrizione, ma gli elementi delle liste abbiano del padding per posizionare le immagini di sfondo all'interno. Se non lo facessimo, le immagini di sfondo si sovrapporrebbero con il testo dell'elemento della lista, il che sembrerebbe disordinato.
- Impostato il {{cssxref("list-style-type")}} su `none`, in modo che nessun punto elenco appaia di default. Utilizzeremo le proprietà {{cssxref("background")}} per gestire i punti elenco invece.
- Inserito un punto elenco su ogni elemento della lista non ordinata. Le proprietà rilevanti sono le seguenti:

  - {{cssxref("background-image")}}: Questo fa riferimento al percorso del file immagine che vuoi usare come punto elenco.
  - {{cssxref("background-position")}}: Questo definisce dove nell'area di sfondo dell'elemento selezionato apparirà l'immagine — in questo caso stiamo dicendo `0 0`, il che significa che il punto elenco apparirà in alto a sinistra di ogni elemento della lista.
  - {{cssxref("background-size")}}: Questo imposta la dimensione dell'immagine di sfondo. Vogliamo idealmente che i punti elenco abbiano la stessa dimensione degli elementi della lista (o molto leggermente più piccoli o più grandi). Stiamo utilizzando una dimensione di `1.6rem` (`16px`), che si adatta molto bene al padding di `20px` che abbiamo lasciato per il punto elenco — 16px più 4px di spazio tra il punto elenco e il testo dell'elemento della lista funziona bene.
  - {{cssxref("background-repeat")}}: Per impostazione predefinita, le immagini di sfondo si ripetono fino a riempire lo spazio disponibile dello sfondo. Vogliamo solo una copia dell'immagine inserita in ciascun caso, quindi lo impostiamo su un valore di `no-repeat`.

Questo ci dà il seguente risultato:

![una lista non ordinata con i punti elenco impostati come piccole immagini di stelle](list_formatting.png)

### Abbreviazione list-style

Le tre proprietà menzionate sopra possono essere tutte impostate utilizzando una singola proprietà abbreviata, {{cssxref("list-style")}}. Ad esempio, il seguente CSS:

```css
ul {
  list-style-type: square;
  list-style-image: url(example.png);
  list-style-position: inside;
}
```

Potrebbe essere sostituito da questo:

```css
ul {
  list-style: square url(example.png) inside;
}
```

I valori possono essere elencati in qualsiasi ordine, e puoi usarne uno, due o tutti e tre (i valori predefiniti utilizzati per le proprietà che non sono incluse sono `disc`, `none`, e `outside`). Se sia un `type` che un `image` sono specificati, il tipo viene utilizzato come fallback se l'immagine non può essere caricata per qualche motivo.

## Controllare la numerazione delle liste

A volte potresti voler contare diversamente in una lista ordinata — ad esempio, a partire da un numero diverso da 1, o contare all'indietro, o contare a passi di più di 1. HTML e CSS hanno alcuni strumenti per aiutarti in questo.

### start

L'attributo [`start`](/it/docs/Web/HTML/Reference/Elements/ol#start) ti permette di iniziare la numerazione della lista da un numero diverso da 1. Il seguente esempio:

```html
<ol start="4">
  <li>Toast pita, leave to cool, then slice down the edge.</li>
  <li>
    Fry the halloumi in a shallow, non-stick pan, until browned on both sides.
  </li>
  <li>Wash and chop the salad.</li>
  <li>Fill pita with salad, hummus, and fried halloumi.</li>
</ol>
```

Ti dà questo output:

{{ EmbedLiveSample('start', '100%', 150) }}

### reversed

L'attributo [`reversed`](/it/docs/Web/HTML/Reference/Elements/ol#reversed) inizierà la numerazione della lista in ordine decrescente invece che crescente. Il seguente esempio:

```html
<ol start="4" reversed>
  <li>Toast pita, leave to cool, then slice down the edge.</li>
  <li>
    Fry the halloumi in a shallow, non-stick pan, until browned on both sides.
  </li>
  <li>Wash and chop the salad.</li>
  <li>Fill pita with salad, hummus, and fried halloumi.</li>
</ol>
```

Ti dà questo output:

{{ EmbedLiveSample('reversed', '100%', 150) }}

> [!NOTE]
> Se ci sono più elementi nella lista di quelli indicati dal valore dell'attributo `start`, la conteggio continuerà fino a zero e poi in valori negativi.

### value

L'attributo [`value`](/it/docs/Web/HTML/Reference/Elements/li#value) ti permette di impostare gli elementi della lista su valori numerici specifici. Il seguente esempio:

```html
<ol>
  <li value="2">Toast pita, leave to cool, then slice down the edge.</li>
  <li value="4">
    Fry the halloumi in a shallow, non-stick pan, until browned on both sides.
  </li>
  <li value="6">Wash and chop the salad.</li>
  <li value="8">Fill pita with salad, hummus, and fried halloumi.</li>
</ol>
```

Ti dà questo output:

{{ EmbedLiveSample('value', '100%', 150) }}

> [!NOTE]
> Anche se stai utilizzando un {{cssxref("list-style-type")}} non numerico, devi comunque utilizzare i valori numerici equivalenti nell'attributo `value`.

## Esercizio pratico: Stilizzare una lista annidata

In questa sessione di apprendimento attivo, vogliamo che tu prenda ciò che hai imparato sopra e provi a stilizzare una lista annidata. Ti abbiamo fornito l'HTML, e vogliamo che tu:

1. Assegni alla lista non ordinata punti quadrati.
2. Imposti gli elementi della lista non ordinata e quelli della lista ordinata con un'altezza di linea pari a 1,5 della loro dimensione del carattere.
3. Assegni alla lista ordinata punti elenco alfabetici in minuscolo.
4. Sentiti libero di giocare con l'esempio della lista quanto desideri, sperimentando con tipi di punti elenco, spaziatura o qualsiasi altra cosa tu riesca a trovare.

Se commetti un errore, puoi sempre reimpostarlo utilizzando il pulsante _Reset_. Se sei veramente bloccato, premi il pulsante _Show solution_ per vedere una possibile soluzione.

```html hidden
<div
  class="body-wrapper"
  style="font-family: 'Open Sans Light',Helvetica,Arial,sans-serif;">
  <h2>HTML Input</h2>
  <textarea
    id="code"
    class="html-input"
    style="width: 90%;height: 10em;padding: 10px;border: 1px solid #0095dd;">
<ul>
  <li>First, light the candle.</li>
  <li>Next, open the box.</li>
  <li>Finally, place the three magic items in the box, in this exact order, to complete the spell:
    <ol>
      <li>The book of spells</li>
      <li>The shiny rod</li>
      <li>The goblin statue</li>
    </ol>
  </li>
</ul>
  </textarea>

  <h2>CSS Input</h2>
  <textarea
    id="code"
    class="css-input"
    style="width: 90%;height: 10em;padding: 10px;border: 1px solid #0095dd;"></textarea>

  <h2>Output</h2>
  <div
    class="output"
    style="width: 90%;height: 12em;padding: 10px;border: 1px solid #0095dd;overflow: auto;"></div>
  <div class="controls">
    <input
      id="reset"
      type="button"
      value="Reset"
      style="margin: 10px 10px 0 0;" />
    <input
      id="solution"
      type="button"
      value="Show solution"
      style="margin: 10px 0 0 10px;" />
  </div>
</div>
```

```js hidden
const htmlInput = document.querySelector(".html-input");
const cssInput = document.querySelector(".css-input");
const reset = document.getElementById("reset");
const htmlCode = htmlInput.value;
const cssCode = cssInput.value;
const output = document.querySelector(".output");
const solution = document.getElementById("solution");

const styleElem = document.createElement("style");
const headElem = document.querySelector("head");
headElem.appendChild(styleElem);

function drawOutput() {
  output.innerHTML = htmlInput.value;
  styleElem.textContent = cssInput.value;
}

reset.addEventListener("click", () => {
  htmlInput.value = htmlCode;
  cssInput.value = cssCode;
  drawOutput();
});

solution.addEventListener("click", () => {
  htmlInput.value = htmlCode;
  cssInput.value = `ul {
  list-style-type: square;
}

ul li,
ol li {
  line-height: 1.5;
}

ol {
  list-style-type: lower-alpha;
}`;
  drawOutput();
});

htmlInput.addEventListener("input", drawOutput);
cssInput.addEventListener("input", drawOutput);
window.addEventListener("load", drawOutput);
```

{{ EmbedLiveSample('Active_learning_Styling_a_nested_list', 700, 800) }}

## Riassunto

Le liste sono relativamente facili da capire come stilizzare una volta che conosci alcuni principi di base associati e proprietà specifiche. Nel prossimo articolo, passeremo alle tecniche di stilizzazione dei link.

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Fundamentals", "Learn_web_development/Core/Text_styling/Styling_links", "Learn_web_development/Core/Text_styling")}}
