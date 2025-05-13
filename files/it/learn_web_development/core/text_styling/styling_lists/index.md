---
title: Styling lists
slug: Learn_web_development/Core/Text_styling/Styling_lists
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Fundamentals", "Learn_web_development/Core/Text_styling/Styling_links", "Learn_web_development/Core/Text_styling")}}

[Liste](/it/docs/Learn_web_development/Core/Structuring_content/Lists) si comportano come qualsiasi altro testo per la maggior parte, ma ci sono alcune proprietà CSS specifiche per le liste che è necessario conoscere, oltre a alcune migliori pratiche da considerare. Questo articolo spiega tutto.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">Strutturare i contenuti con HTML</a> e
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi di CSS Styling</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Spaziare gli elementi delle liste, ad esempio con margine o interlinea.</li>
          <li>Utilizzare le proprietà <code>list-style</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Un semplice esempio di lista

Per cominciare, vediamo un semplice esempio di lista. In questo articolo esamineremo liste non ordinate, ordinate e di descrizione — tutte hanno caratteristiche di stile simili, oltre a alcune che sono particolari per ciascuna. L'esempio non stilizzato è [disponibile su GitHub](https://mdn.github.io/learning-area/css/styling-text/styling-lists/unstyled-list.html) (controlli anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/css/styling-text/styling-lists/unstyled-list.html)).

L'HTML per il nostro esempio di lista appare così:

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

Se vai all'esempio dal vivo ora e investighe gli elementi della lista usando [gli strumenti per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools), noterai alcuni predefiniti di stile:

- Gli elementi {{htmlelement("ul")}} e {{htmlelement("ol")}} hanno un {{cssxref("margin")}} superiore e inferiore di `16px` (`1em`) e un {{cssxref("padding-left")}} di `40px` (`2.5em`). Se l'attributo di direzionalità [`dir`](/it/docs/Web/HTML/Reference/Global_attributes/dir) è impostato su destra-a-sinistra (`rtl`) per gli elementi `ul` e `ol`, in quel caso {{cssxref("padding-right")}} entra in gioco e il suo valore predefinito è `40px` (`2.5em`).
- Gli elementi della lista (elementi {{htmlelement("li")}}) non hanno predefiniti impostati per lo spazio.
- L'elemento {{htmlelement("dl")}} ha un {{cssxref("margin")}} superiore e inferiore di `16px` (`1em`), ma nessun padding impostato.
- Gli elementi {{htmlelement("dd")}} hanno un {{cssxref("margin-left")}} di `40px` (`2.5em`).
- Gli elementi {{htmlelement("p")}} che abbiamo incluso come riferimento hanno un {{cssxref("margin")}} superiore e inferiore di `16px` (`1em`) — lo stesso dei diversi tipi di liste.

## Gestire la spaziatura delle liste

Quando si stilizzano le liste, è necessario aggiustare i loro stili in modo che mantengano la stessa spaziatura verticale degli elementi circostanti (come paragrafi e immagini; a volte chiamato ritmo verticale) e la stessa spaziatura orizzontale tra di loro (può vedere l'[esempio stilizzato finale](https://mdn.github.io/learning-area/css/styling-text/styling-lists/) su GitHub e [trovare il codice sorgente](https://github.com/mdn/learning-area/blob/main/css/styling-text/styling-lists/index.html)).

Il CSS usato per lo stile del testo e la spaziatura è il seguente:

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

- La prima regola imposta un carattere generale e una dimensione base del carattere di 10px. Questi sono ereditati da tutto nel pagina.
- Le regole 2 e 3 impostano dimensioni del carattere relative per i titoli, diversi tipi di liste (i figli degli elementi lista ereditano questi) e paragrafi. Ciò significa che ogni paragrafo e lista avrà la stessa dimensione del carattere e dello spazio superiore e inferiore, aiutando a mantenere il ritmo verticale coerente.
- La regola 4 imposta la stessa {{cssxref("line-height")}} sui paragrafi e sugli elementi della lista — in modo tale che i paragrafi e ciascun elemento della lista abbiano lo stesso spazio tra le righe. Anche questo aiuterà a mantenere il ritmo verticale coerente.
- Le regole 5 e 6 si applicano alla lista di descrizione. Impostiamo la stessa `line-height` sui termini e le descrizioni della lista come abbiamo fatto con i paragrafi e gli elementi della lista. Ancora, la coerenza è buona! Facciamo anche in modo che i termini della descrizione abbiano il carattere in grassetto, così da risaltare visivamente.

## Stili specifici per le liste

Ora che abbiamo esaminato le tecniche generali di spaziatura per le liste, esploriamo alcune proprietà specifiche per le liste. Ci sono tre proprietà che è bene conoscere e che possono essere impostate sugli elementi {{htmlelement("ul")}} o {{htmlelement("ol")}}:

- {{cssxref("list-style-type")}}: Imposta il tipo di punti elenco da utilizzare per la lista, ad esempio, punti quadrati o cerchi per una lista non ordinata, o numeri, lettere o numeri romani per una lista ordinata.
- {{cssxref("list-style-position")}}: Imposta se i punti, all'inizio di ciascun elemento, appaiono all'interno o all'esterno delle liste.
- {{cssxref("list-style-image")}}: Consente di utilizzare un'immagine personalizzata come punto elenco, piuttosto che un semplice quadrato o cerchio.

### Stili dei punti elenco

Come accennato sopra, la proprietà {{cssxref("list-style-type")}} consente di impostare quale tipo di punto utilizzare per i punti elenco. Nel nostro esempio, abbiamo impostato la lista ordinata per utilizzare numeri romani maiuscoli con:

```css
ol {
  list-style-type: upper-roman;
}
```

Questo ci dà l'aspetto seguente:

![una lista ordinata con i punti elenco impostati a comparire fuori dal testo dell'elemento della lista.](outer-bullets.png)

Può trovare molte più opzioni consultando la pagina di riferimento {{cssxref("list-style-type")}}.

### Posizione dei punti elenco

La proprietà {{cssxref("list-style-position")}} imposta se i punti appaiono all'interno degli elementi della lista o all'esterno di essi prima dell'inizio di ciascun elemento. Il valore predefinito è `outside`, il che fa sì che i punti siedano fuori dagli elementi della lista, come visto sopra.

Se imposta il valore su `inside`, i punti siederanno all'interno delle righe:

```css
ol {
  list-style-type: upper-roman;
  list-style-position: inside;
}
```

![una lista ordinata con i punti elenco impostati a comparire dentro il testo dell'elemento della lista.](inner-bullets.png)

### Utilizzare un'immagine di punto elenco personalizzata

La proprietà {{cssxref("list-style-image")}} consente di utilizzare un'immagine personalizzata per il suo punto elenco. La sintassi è abbastanza semplice:

```css
ul {
  list-style-image: url(star.svg);
}
```

Tuttavia, questa proprietà è un po' limitata in termini di controllo della posizione, della dimensione, ecc. dei punti elenco. È meglio utilizzare la famiglia di proprietà {{cssxref("background")}}, di cui ha appreso nella nostra precedente lezione [Sfondi e bordi](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders).

Nel nostro esempio finale, abbiamo stilizzato la lista non ordinata in questo modo (oltre a quanto ha già visto sopra):

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

- Abbiamo ridotto il {{cssxref("padding-left")}} del {{htmlelement("ul")}} dal valore predefinito di `40px` a `20px`, poi impostato la stessa quantità sugli elementi della lista. Questo è per garantire che, in generale, gli elementi della lista siano ancora allineati con gli elementi della lista ordinata e le descrizioni della lista di descrizione, ma che gli elementi della lista abbiano un po’ di padding per gli sfondi delle immagini. Se non avessimo fatto così, le immagini di sfondo si sovrapporrebbero al testo dell'elemento della lista, il che apparirebbe disordinato.
- Abbiamo impostato {{cssxref("list-style-type")}} su `none`, in modo che nessun punto elenco appaia per impostazione predefinita. Useremo le proprietà di {{cssxref("background")}} per gestire i punti elenco.
- Abbiamo inserito un punto elenco su ciascun elemento della lista non ordinata. Le proprietà pertinenti sono le seguenti:

  - {{cssxref("background-image")}}: Questo fa riferimento al percorso del file immagine che si desidera utilizzare come punto elenco.
  - {{cssxref("background-position")}}: Questo definisce dove nell'elemento selezionato apparirà l'immagine — in questo caso stiamo dicendo `0 0`, il che significa che il punto elenco apparirà in alto a sinistra di ciascun elemento della lista.
  - {{cssxref("background-size")}}: Questo imposta la dimensione dell'immagine di sfondo. Idealmente vogliamo che i punti elenco siano della stessa dimensione degli elementi della lista (o molto leggermente più piccoli o più grandi). Stiamo usando una dimensione di `1.6rem` (`16px`), che si adatta molto bene al padding di `20px` che abbiamo lasciato per il punto elenco — 16px più 4px di spazio tra il punto elenco e il testo dell'elemento della lista funziona bene.
  - {{cssxref("background-repeat")}}: Per impostazione predefinita, le immagini di sfondo si ripetono fino a riempire lo spazio disponibile dello sfondo. Vogliamo solo una copia dell'immagine inserita in ciascun caso, quindi impostiamo questo valore su `no-repeat`.

Questo ci dà il seguente risultato:

![una lista non ordinata con i punti elenco impostati come piccole immagini di stelle](list_formatting.png)

### scorciatoia list-style

Le tre proprietà menzionate sopra possono tutte essere impostate usando una singola proprietà abbreviata, {{cssxref("list-style")}}. Ad esempio, il seguente CSS:

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

I valori possono essere elencati in qualsiasi ordine, e puoi usare uno, due o tutti e tre (i valori predefiniti usati per le proprietà non incluse sono `disc`, `none` e `outside`). Se sono specificati sia un `type` che un `image`, il tipo viene utilizzato come ripiego se l'immagine non può essere caricata per qualche motivo.

## Controllare il conteggio delle liste

A volte potrebbe voler contare in modo diverso in una lista ordinata — ad esempio, iniziando da un numero diverso da 1, o contando all'indietro, o contando in passi superiori a 1. HTML e CSS hanno alcuni strumenti per aiutarti qui.

### start

L'attributo [`start`](/it/docs/Web/HTML/Reference/Elements/ol#start) consente di iniziare il conteggio della lista da un numero diverso da 1. Il seguente esempio:

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

L'attributo [`reversed`](/it/docs/Web/HTML/Reference/Elements/ol#reversed) farà iniziare il conteggio della lista a ritroso invece che in avanti. Il seguente esempio:

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
> Se ci sono più elementi nella lista inversa rispetto al valore dell'attributo `start`, il conteggio continuerà a zero e poi in valori negativi.

### value

L'attributo [`value`](/it/docs/Web/HTML/Reference/Elements/li#value) consente di impostare i tuoi elementi della lista su valori numerici specifici. Il seguente esempio:

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
> Anche se stai usando un {{cssxref("list-style-type")}} non numerico, è comunque necessario utilizzare i valori numerici equivalenti nell'attributo `value`.

## Apprendimento attivo: Stilizzare una lista annidata

In questa sessione di apprendimento attivo, vogliamo che prenda ciò che ha appreso sopra e provi a stilizzare una lista annidata. Le forniamo l'HTML, e vogliamo che lei:

1. Dia alla lista non ordinata punti elenco quadrati.
2. Dia agli elementi della lista non ordinata e agli elementi della lista ordinata un interlinea di 1.5 della loro dimensione del carattere.
3. Dia alla lista ordinata punti elenco alfabetici minuscoli.
4. Si senta libero di giocare con l'esempio di lista quanto desidera, sperimentando con i tipi di punti elenco, la spaziatura, o qualsiasi altra cosa possa trovare.

Se commette un errore, può sempre ripristinarlo utilizzando il pulsante _Ripristina_. Se rimane davvero bloccato, premi il pulsante _Mostra soluzione_ per vedere una risposta potenziale.

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

## Sommario

Le liste sono relativamente facili da comprendere in termini di stile una volta che si conoscono alcuni principi di base associati e proprietà specifiche. Nel prossimo articolo, passeremo alle tecniche di stile dei link.

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Fundamentals", "Learn_web_development/Core/Text_styling/Styling_links", "Learn_web_development/Core/Text_styling")}}
