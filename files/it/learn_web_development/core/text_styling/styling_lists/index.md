---
title: Applicare stili alle liste
slug: Learn_web_development/Core/Text_styling/Styling_lists
l10n:
  sourceCommit: 1b7c3c1e03f14c3878e4d8518b0f1a89bedfdc9c
---

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Fundamentals", "Learn_web_development/Core/Text_styling/Styling_links", "Learn_web_development/Core/Text_styling")}}

Le [liste](/it/docs/Learn_web_development/Core/Structuring_content/Lists) si comportano per la maggior parte come qualsiasi altro testo, ma esistono alcune proprietà CSS specifiche per le liste che è necessario conoscere, oltre ad alcune buone pratiche da considerare. Questo articolo le spiega tutte.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        > e
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Fondamenti dello stile CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Spaziatura degli elementi delle liste, ad esempio con margin o line height.</li>
          <li>Utilizzo delle proprietà <code>list-style</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Un esempio di lista di base

Vediamo un esempio di lista di base. Nel corso di questo articolo verranno esaminate liste non ordinate, ordinate e di descrizione: tutte dispongono di funzionalità di stile simili, oltre ad alcune specifiche per ciascun tipo.

L'HTML per il nostro esempio di lista è il seguente:

```html live-sample___unstyled live-sample___initial-style live-sample___finished-style
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

Senza alcuno stile, il rendering è il seguente:

{{embedlivesample("unstyled", "100%", 400)}}

Esaminando questi elementi di lista con gli [strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools), si noteranno alcune impostazioni di stile predefinite:

- Gli elementi {{htmlelement("ul")}} e {{htmlelement("ol")}} hanno un {{cssxref("margin")}} superiore e inferiore di `16px` (`1em`) e un {{cssxref("padding-left")}} di `40px` (`2.5em`). Se l'attributo di direzionalità [`dir`](/it/docs/Web/HTML/Reference/Global_attributes/dir) è impostato da destra a sinistra (`rtl`) per gli elementi `ul` e `ol`, entra invece in vigore {{cssxref("padding-right")}}, il cui valore predefinito è `40px` (`2.5em`).
- Gli elementi della lista (elementi {{htmlelement("li")}}) non hanno impostazioni predefinite per la spaziatura.
- L'elemento {{htmlelement("dl")}} ha un {{cssxref("margin")}} superiore e inferiore di `16px` (`1em`), ma nessun padding impostato.
- Gli elementi {{htmlelement("dd")}} hanno un {{cssxref("margin-left")}} di `40px` (`2.5em`).
- Gli elementi {{htmlelement("p")}} inclusi come riferimento hanno un {{cssxref("margin")}} superiore e inferiore di `16px` (`1em`), uguale a quello dei diversi tipi di lista.

## Gestire la spaziatura delle liste

Quando si applicano stili alle liste, è necessario regolarli in modo che mantengano la stessa spaziatura verticale degli elementi circostanti, come paragrafi e immagini, talvolta chiamata ritmo verticale, e la stessa spaziatura orizzontale tra loro. Alcuni tipici stili CSS e impostazioni di spaziatura per il testo potrebbero essere così:

```css live-sample___initial-style live-sample___list-style-type live-sample___list-style-position live-sample___custom-bullets live-sample___finished-style
/* General styles */

html {
  font-family: "Helvetica", "Arial", sans-serif;
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

- La prima regola imposta un font per l'intero sito e una dimensione del font di base pari a 10px. Queste impostazioni vengono ereditate da tutti gli elementi della pagina.
- Le regole 2 e 3 impostano dimensioni relative del font per intestazioni, diversi tipi di liste (ereditate dai figli degli elementi lista) e paragrafi. Ciò significa che ciascun paragrafo e lista avrà la stessa dimensione del font e la stessa spaziatura superiore e inferiore, contribuendo a mantenere coerente il ritmo verticale.
- La regola 4 imposta lo stesso {{cssxref("line-height")}} per i paragrafi e gli elementi di lista, quindi i paragrafi e ogni singolo elemento della lista avranno la stessa spaziatura tra le righe. Anche questo contribuisce a mantenere coerente il ritmo verticale.
- Le regole 5 e 6 si applicano alla lista di descrizione. Viene impostato lo stesso `line-height` per i termini e le descrizioni della lista di descrizione usato per paragrafi ed elementi di lista. Anche in questo caso, la coerenza è utile. Inoltre, i termini di descrizione vengono resi in grassetto, in modo che risaltino più facilmente a livello visivo.

Quando viene applicato all'HTML mostrato in precedenza, il codice viene visualizzato così:

{{embedlivesample("initial-style", "100%", 400)}}

## Stili specifici delle liste

Dopo aver esaminato le tecniche generali di spaziatura per le liste, esploriamo alcune proprietà specifiche delle liste. Per iniziare, è necessario conoscere tre proprietà, che possono essere impostate sugli elementi {{htmlelement("ul")}} o {{htmlelement("ol")}}:

- {{cssxref("list-style-type")}}: imposta il tipo di marcatore da usare per la lista, ad esempio marcatori quadrati o circolari per una lista non ordinata, oppure numeri, lettere o numeri romani per una lista ordinata.
- {{cssxref("list-style-position")}}: imposta se i marcatori, all'inizio di ogni elemento, appaiono all'interno o all'esterno delle liste.
- {{cssxref("list-style-image")}}: consente di usare un'immagine personalizzata come marcatore, anziché un semplice quadrato o cerchio.

### Stili dei marcatori

Come già accennato, la proprietà {{cssxref("list-style-type")}} consente di impostare il tipo di marcatore da usare per i punti elenco. Nel nostro esempio, la lista ordinata è stata impostata per usare numeri romani maiuscoli con:

```html hidden live-sample___list-style-type live-sample___list-style-position
<ol>
  <li>Toast pita, leave to cool, then slice down the edge.</li>
  <li>
    Fry the halloumi in a shallow, non-stick pan, until browned on both sides.
  </li>
  <li>Wash and chop the salad.</li>
  <li>Fill pita with salad, hummus, and fried halloumi.</li>
</ol>
```

```css live-sample___list-style-type
ol {
  list-style-type: upper-roman;
}
```

Questo produce il seguente aspetto:

{{embedlivesample("list-style-type", "100%", 120)}}

Sono disponibili molte altre opzioni nella pagina di riferimento di {{cssxref("list-style-type")}}.

### Posizione dei marcatori

La proprietà {{cssxref("list-style-position")}} imposta se i marcatori appaiono all'interno degli elementi della lista oppure all'esterno, prima dell'inizio di ciascun elemento. Il valore predefinito è `outside`, che fa sì che i marcatori si trovino all'esterno degli elementi della lista, come mostrato sopra.

Se il valore viene impostato su `inside`, i marcatori si troveranno all'interno delle righe:

```css live-sample___list-style-position live-sample___finished-style
ol {
  list-style-type: upper-roman;
  list-style-position: inside;
}
```

{{embedlivesample("list-style-position", "100%", 120)}}

### Utilizzare un'immagine personalizzata per i marcatori

La proprietà {{cssxref("list-style-image")}} consente di usare un'immagine personalizzata come marcatore. La sintassi è la seguente:

```css
ul {
  list-style-image: url("https://mdn.github.io/shared-assets/images/examples/star-shape.png");
}
```

Tuttavia, questa proprietà è piuttosto limitata per quanto riguarda il controllo di posizione, dimensione e altri aspetti dei marcatori. È preferibile usare la famiglia di proprietà {{cssxref("background")}}, illustrata nella precedente lezione su [sfondi e bordi](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders).

Nell'esempio finale, la lista non ordinata è stata stilizzata nel seguente modo:

```html hidden live-sample___custom-bullets
<ul>
  <li>Hummus</li>
  <li>Pita</li>
  <li>Green salad</li>
  <li>Halloumi</li>
</ul>
```

```css live-sample___custom-bullets live-sample___finished-style
ul {
  padding-left: 2rem;
  list-style-type: none;
}

ul li {
  padding-left: 2rem;
  background-image: url("https://mdn.github.io/shared-assets/images/examples/star-shape.png");
  background-position: 0 0;
  background-size: 1.6rem 1.6rem;
  background-repeat: no-repeat;
}
```

Qui è stato fatto quanto segue:

- Il {{cssxref("padding-left")}} di {{htmlelement("ul")}} è stato ridotto dal valore predefinito di `40px` a `20px`, quindi è stata impostata la stessa quantità sugli elementi della lista. In questo modo, nel complesso, gli elementi della lista rimangono allineati con quelli della lista ordinata e con le descrizioni della lista di descrizione, ma hanno un padding in cui posizionare le immagini di sfondo. Senza questa impostazione, le immagini di sfondo si sovrapporrebbero al testo degli elementi della lista, con un risultato disordinato.
- {{cssxref("list-style-type")}} è stato impostato su `none`, in modo che non venga visualizzato alcun marcatore predefinito. Verranno invece usate le proprietà {{cssxref("background")}} per gestire i marcatori.
- È stato inserito un marcatore in ogni elemento della lista non ordinata. Le proprietà pertinenti sono le seguenti:
  - {{cssxref("background-image")}}: fa riferimento al percorso del file immagine da usare come marcatore.
  - {{cssxref("background-position")}}: definisce dove apparirà l'immagine nello sfondo dell'elemento selezionato. In questo caso viene indicato `0 0`, il che significa che il marcatore apparirà nell'angolo superiore sinistro di ciascun elemento della lista.
  - {{cssxref("background-size")}}: imposta la dimensione dell'immagine di sfondo. Idealmente, i marcatori dovrebbero avere la stessa dimensione degli elementi della lista, oppure essere leggermente più piccoli o più grandi. Viene usata una dimensione di `1.6rem` (`16px`), che si adatta molto bene al padding di `20px` previsto per il marcatore: 16px più 4px di spazio tra il marcatore e il testo dell'elemento della lista funzionano bene.
  - {{cssxref("background-repeat")}}: per impostazione predefinita, le immagini di sfondo si ripetono finché non riempiono lo spazio di sfondo disponibile. Si desidera inserire una sola copia dell'immagine in ciascun caso, quindi questa proprietà viene impostata sul valore `no-repeat`.

Il risultato è il seguente:

{{embedlivesample("custom-bullets", "100%", 120)}}

### Abbreviazione list-style

Le tre proprietà menzionate sopra possono essere tutte impostate usando un'unica proprietà abbreviata, {{cssxref("list-style")}}. Ad esempio, il seguente CSS:

```css
ul {
  list-style-type: square;
  list-style-image: url("example.png");
  list-style-position: inside;
}
```

Potrebbe essere sostituito da questo:

```css
ul {
  list-style: square url("example.png") inside;
}
```

I valori possono essere elencati in qualsiasi ordine e se ne possono usare uno, due o tutti e tre; i valori predefiniti usati per le proprietà non incluse sono `disc`, `none` e `outside`. Se vengono specificati sia un `type` sia un'`image`, il tipo viene usato come alternativa di riserva se, per qualche motivo, l'immagine non può essere caricata.

## Esempio completo

Nelle ultime sezioni sono stati mostrati gli effetti di alcune funzionalità isolate delle liste. Applicandole tutte all'elenco HTML iniziale, il risultato è il seguente:

{{embedlivesample("finished-style", "100%", 400)}}

## Controllare la numerazione delle liste

Talvolta può essere necessario numerare una lista ordinata in modo diverso, ad esempio iniziando da un numero diverso da 1, contando all'indietro oppure aumentando di più di 1 a ogni passaggio. HTML e CSS offrono alcuni strumenti utili per questo.

### start

L'attributo [`start`](/it/docs/Web/HTML/Reference/Elements/ol#start) consente di iniziare la numerazione della lista da un numero diverso da 1. Il seguente esempio:

```html live-sample___counting-control
<ol start="4">
  <li>Toast pita, leave to cool, then slice down the edge.</li>
  <li>
    Fry the halloumi in a shallow, non-stick pan, until browned on both sides.
  </li>
  <li>Wash and chop the salad.</li>
  <li>Fill pita with salad, hummus, and fried halloumi.</li>
</ol>
```

Produce il seguente output:

{{ EmbedLiveSample('counting-control', '100%', 150) }}

### reversed

L'attributo [`reversed`](/it/docs/Web/HTML/Reference/Elements/ol#reversed) farà iniziare il conteggio della lista in ordine decrescente anziché crescente. Il seguente esempio:

```html live-sample___counting-control-reversed
<ol start="4" reversed>
  <li>Toast pita, leave to cool, then slice down the edge.</li>
  <li>
    Fry the halloumi in a shallow, non-stick pan, until browned on both sides.
  </li>
  <li>Wash and chop the salad.</li>
  <li>Fill pita with salad, hummus, and fried halloumi.</li>
</ol>
```

Produce il seguente output:

{{ EmbedLiveSample('counting-control-reversed', '100%', 150) }}

> [!NOTE]
> Se in una lista invertita sono presenti più elementi di lista rispetto al valore dell'attributo `start`, il conteggio proseguirà fino a zero e poi in valori negativi.

### value

L'attributo [`value`](/it/docs/Web/HTML/Reference/Elements/li#value) consente di impostare valori numerici specifici per gli elementi della lista. Il seguente esempio:

```html counting-control-values
<ol>
  <li value="2">Toast pita, leave to cool, then slice down the edge.</li>
  <li value="4">
    Fry the halloumi in a shallow, non-stick pan, until browned on both sides.
  </li>
  <li value="6">Wash and chop the salad.</li>
  <li value="8">Fill pita with salad, hummus, and fried halloumi.</li>
</ol>
```

Produce il seguente output:

{{ EmbedLiveSample('counting-control-values', '100%', 150) }}

> [!NOTE]
> Anche quando si usa un {{cssxref("list-style-type")}} non numerico, è comunque necessario usare nell'attributo `value` i valori numerici equivalenti.

## Tocca a te: applicare stili a una lista annidata

È il momento di completare un'altra attività. Questa volta è necessario usare quanto appreso sopra per provare ad applicare stili a una lista annidata.

1. Fare clic su **"Play"** nel blocco di codice seguente per modificare l'esempio nel Playground MDN.
2. Applicare alla lista non ordinata marcatori quadrati.
3. Assegnare agli elementi della lista non ordinata e agli elementi della lista ordinata un `line-height` pari a `1.5` della loro `font-size`.
4. Impostare la lista ordinata con marcatori alfabetici minuscoli.
5. È possibile sperimentare liberamente con l'esempio di lista, provando tipi di marcatore, spaziatura o qualsiasi altra caratteristica desiderata.

In caso di errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nel Playground MDN. Se si incontrano particolari difficoltà, la soluzione è disponibile sotto l'output dell'esempio.

```html live-sample___styling_lists
<ul>
  <li>First, light the candle.</li>
  <li>Next, open the box.</li>
  <li>
    Finally, place the three magic items in the box, in this exact order, to
    complete the spell:
    <ol>
      <li>The book of spells</li>
      <li>The shiny rod</li>
      <li>The goblin statue</li>
    </ol>
  </li>
</ul>
```

```css live-sample___styling_lists

```

{{ EmbedLiveSample('styling_lists', "100%", 160) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il CSS finale dovrebbe avere un aspetto simile a questo:

```css
ul {
  list-style-type: square;
}

li {
  line-height: 1.5;
}

ol {
  list-style-type: lower-alpha;
}
```

</details>

## Riepilogo

Applicare stili alle liste è relativamente semplice una volta appresi alcuni principi di base associati e proprietà specifiche. Nel prossimo articolo verranno esaminate le tecniche per applicare stili ai link.

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Fundamentals", "Learn_web_development/Core/Text_styling/Styling_links", "Learn_web_development/Core/Text_styling")}}
