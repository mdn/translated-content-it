---
title: Introduzione al layout CSS
short-title: Introduction
slug: Learn_web_development/Core/CSS_layout/Introduction
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{NextMenu("Learn_web_development/Core/CSS_layout/Floats", "Learn_web_development/Core/CSS_layout")}}

Questa lezione riepiloga alcune delle funzionalità di layout CSS che abbiamo già affrontato in moduli precedenti, come i diversi valori di {{cssxref("display")}}, e introduce alcuni dei concetti che tratteremo in questo modulo. Copre anche in dettaglio il concetto di flusso normale.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturazione del contenuto con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo stile CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Nozioni fondamentali sullo stile del testo e dei caratteri</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Riconoscere i metodi utilizzati per implementare layout di pagina moderni.</li>
          <li>Comprendere che il flusso normale è il modo predefinito in cui un browser dispone il contenuto di blocchi e in linea.</li>
          <li>Sapere che proprietà come <code>display</code>, <code>float</code> e <code>position</code> sono destinate a cambiare il modo in cui il browser dispone il contenuto.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Le tecniche di layout delle pagine CSS ci permettono di prendere elementi contenuti in una pagina web e controllare dove sono posizionati in relazione ai seguenti fattori: la loro posizione predefinita nel flusso di layout normale, gli altri elementi intorno a loro, il loro contenitore padre e la finestra/viewport principale.

Le tecniche di layout della pagina che menzioneremo sotto e copriremo in dettaglio nel modulo hanno i loro usi, vantaggi e svantaggi. Nessuna tecnica è progettata per essere utilizzata in isolamento. Comprendendo per cosa è progettato ciascun metodo di layout, si sarà in una buona posizione per capire quale metodo è più appropriato per ciascun compito.

## Flusso di layout normale

Gli elementi su una pagina web vengono disposti in **flusso normale** se non hai applicato alcun CSS per cambiare il modo in cui si comportano. Puoi cambiare il comportamento degli elementi modificando la loro posizione nel flusso normale o rimuovendoli da esso del tutto. Iniziare con un documento ben strutturato e leggibile in flusso normale è il modo migliore per iniziare qualsiasi pagina web. Garantisce che il tuo contenuto sia leggibile anche se l'utente utilizza un browser molto limitato o un dispositivo come un lettore di schermo che legge il contenuto della pagina. Inoltre, dato che il flusso normale è progettato per creare un documento leggibile, iniziando in questo modo stai lavorando _con_ il documento piuttosto che lottando _contro_ di esso mentre apporti modifiche al layout.

Prima di approfondire i diversi metodi di layout, vale la pena rivisitare alcune delle cose che hai studiato nei moduli precedenti riguardanti il flusso normale del documento.

## Come sono disposti gli elementi per impostazione predefinita?

Il processo inizia quando le box dei singoli elementi vengono disposte in modo tale che qualsiasi `padding`, `border` o `margin` che possono avere viene aggiunto al loro contenuto. Questo è ciò che chiamiamo **box model**.

Per impostazione predefinita, il contenuto di un {{Glossary("Block-level_content", "elemento di livello blocco")}} riempie lo spazio in linea disponibile dell'elemento padre che lo contiene, crescendo lungo la dimensione del blocco per adattarsi al suo contenuto. La dimensione degli {{Glossary("Inline-level_content", "elementi di livello in linea")}} è solo la dimensione del loro contenuto. È possibile impostare {{cssxref("width")}} o {{cssxref("height")}} su alcuni elementi che hanno un valore predefinito della proprietà {{cssxref("display")}} di `inline`, come {{HTMLElement("img")}}, ma il valore `display` rimarrà comunque `inline`.

Se vuoi controllare la proprietà `display` di un elemento di livello in linea in questo modo, utilizza il CSS per impostarlo in modo che si comporti come un elemento di livello blocco (ad esempio, con `display: block;` o `display: inline-block;`, che mescola caratteristiche di entrambi).

Questo spiega come sono strutturati gli elementi singolarmente, ma per quanto riguarda il modo in cui sono strutturati quando interagiscono tra loro? Il flusso di layout normale (menzionato nell'articolo di introduzione al layout) è il sistema secondo cui gli elementi sono posti all'interno del viewport del browser. Per impostazione predefinita, gli elementi di livello blocco sono disposti nella _direzione del flusso del blocco_, che si basa sulla [modalità di scrittura](/it/docs/Web/CSS/writing-mode) del genitore (_iniziale_: horizontal-tb). Ogni elemento apparirà su una nuova riga sotto l'ultimo, ciascuno separato dal margine specificato. In inglese, per esempio, (o in qualsiasi altra modalità di scrittura orizzontale, dall'alto verso il basso) gli elementi di livello blocco sono disposti verticalmente.

Gli elementi in linea si comportano diversamente. Non appaiono su nuove righe; invece, siedono tutti sulla stessa riga insieme a qualsiasi contenuto testuale adiacente (o avvolto) fintanto che c'è spazio per farlo all'interno della larghezza dell'elemento di livello blocco padre. Se non c'è spazio, allora il contenuto traboccante si sposterà su una nuova riga.

Se due elementi adiacenti verticalmente hanno entrambi un margine impostato su di essi e i loro margini si toccano, il più grande dei due margini rimane e il più piccolo scompare. Questo è conosciuto come [**collasso dei margini**](/it/docs/Web/CSS/CSS_box_model/Mastering_margin_collapsing). I margini che collassano sono rilevanti solo nella **direzione verticale**.

### Esempio di flusso normale

Diamo uno sguardo a un semplice esempio che spiega tutto questo:

```html
<h1>Basic document flow</h1>

<p>
  I am a basic block level element. My adjacent block level elements sit on new
  lines below me.
</p>

<p>
  By default we span 100% of the width of our parent element, and we are as tall
  as our child content. Our total width and height is our content + padding +
  border width/height.
</p>

<p>
  We are separated by our margins. Because of margin collapsing, we are
  separated by the size of one of our margins, not both.
</p>

<p>
  Inline elements <span>like this one</span> and <span>this one</span> sit on
  the same line along with adjacent text nodes, if there is space on the same
  line. Overflowing inline elements will
  <span>wrap onto a new line if possible (like this one containing text)</span>,
  or just go on to a new line if not, much like this image will do:
  <img
    src="https://mdn.github.io/shared-assets/images/examples/long.jpg"
    alt="snippet of cloth" />
</p>
```

```css
body {
  width: 500px;
  margin: 0 auto;
}

p {
  background: rgb(255 84 104 / 30%);
  border: 2px solid rgb(255 84 104);
  padding: 10px;
  margin: 10px;
}

span {
  background: white;
  border: 1px solid black;
}
```

{{ EmbedLiveSample('How_are_elements_laid_out_by_default', '100%', 600) }}

Nota come l'HTML viene visualizzato nell'ordine esatto in cui appare nel codice sorgente, con elementi di blocco empilati uno sopra l'altro.

Per molti degli elementi della tua pagina, il flusso normale creerà esattamente il layout di cui hai bisogno. Tuttavia, per layout più complessi sarà necessario modificare questo comportamento predefinito utilizzando alcuni degli strumenti disponibili in CSS. Iniziare con un documento HTML ben strutturato è molto importante perché puoi quindi lavorare con il modo in cui le cose sono disposte per impostazione predefinita piuttosto che lottare contro di esso.

## Sovrascrivere il flusso normale

I metodi che possono sovrascrivere il flusso normale e cambiare il modo in cui gli elementi sono disposti in CSS, che tratteremo in dettaglio in questo modulo, sono:

- La proprietà {{cssxref("display")}}
  - : I valori standard come `block`, `inline` o `inline-block` possono cambiare il comportamento degli elementi nel flusso normale, per esempio, facendo comportare un elemento di livello blocco come un elemento di livello in linea (abbiamo trattato questi nella lezione sul [Box model](/it/docs/Learn_web_development/Core/Styling_basics/Box_model#block_and_inline_boxes)).
- Float
  - : Applicare un valore {{cssxref("float")}} come `left` può causare agli elementi di livello blocco di avvolgersi lungo un lato di un elemento, come il modo in cui le immagini a volte hanno testo che galleggia intorno a loro nei layout di riviste.
- Posizionamento
  - : La proprietà {{cssxref("position")}} permette di controllare con precisione il posizionamento delle scatole all'interno di altre scatole. Il posizionamento `static` è il valore predefinito nel flusso normale, ma puoi fare in modo che gli elementi siano disposti diversamente usando altri valori, per esempio fissandoli nella parte superiore del viewport del browser utilizzando `position: fixed`.
- Sistemi di layout specifici accessibili tramite `display`
  - : Abbiamo anche metodi di layout interi che vengono abilitati tramite valori `display` specifici. I più importanti di cui devi essere a conoscenza sono [CSS grid](/it/docs/Learn_web_development/Core/CSS_layout/Grids) e [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox), che entrambi alterano il modo in cui gli elementi figli sono disposti all'interno dei loro genitori.
- Design responsivo
  - : Il design responsivo si riferisce alla creazione di layout che si adattano ai diversi dispositivi su cui la pagina web è resa (per esempio, desktop e telefoni cellulari). Il design responsivo non fornisce strumenti di layout specifici di per sé; il suo componente più significativo è la direttiva {{cssxref("@media")}}, che ti permette di applicare layout diversi a seconda degli attributi del dispositivo come la larghezza o la risoluzione dello schermo.

### Altre tecniche di layout

Ci sono altre tecniche di layout che sono meno comunemente usate, che non copriremo in questo modulo:

- [Layout a tabella](/it/docs/Web/CSS/CSS_table)
  - : Le funzionalità progettate per lo stile delle parti di una tabella HTML possono essere utilizzate su elementi non tabellari usando `display: table` e proprietà associate.
- [Layout a colonne multiple](/it/docs/Web/CSS/CSS_multicol_layout)
  - : Le proprietà di layout a colonne multiple possono causare il layout del contenuto di un blocco in colonne, come si potrebbe vedere in un giornale.

## Riepilogo

Questo articolo ha fornito un breve riepilogo di tutte le tecnologie di layout che dovresti conoscere a questo punto della tua formazione! Continua a leggere per ulteriori informazioni su ciascuna tecnologia individuale. Il prossimo argomento sarà sui float.

{{NextMenu("Learn_web_development/Core/CSS_layout/Floats", "Learn_web_development/Core/CSS_layout")}}
