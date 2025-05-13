---
title: Introduzione al layout CSS
short-title: Introduction
slug: Learn_web_development/Core/CSS_layout/Introduction
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{NextMenu("Learn_web_development/Core/CSS_layout/Floats", "Learn_web_development/Core/CSS_layout")}}

Questa lezione riassume alcune delle funzionalità di layout CSS che abbiamo già toccato nei moduli precedenti, come i diversi valori di {{cssxref("display")}}, introducendo anche alcuni dei concetti che tratteremo in questo modulo. Copre inoltre in dettaglio il concetto di flusso normale.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare il contenuto con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo stile CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Stile fondamentale di testo e font</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Riconoscere i metodi utilizzati per implementare layout di pagine moderne.</li>
          <li>Comprendere che il flusso normale è il modo predefinito in cui un browser dispone i contenuti a blocchi e inlinea.</li>
          <li>Sapere che proprietà come <code>display</code>, <code>float</code> e <code>position</code> sono destinate a cambiare il modo in cui il browser dispone i contenuti.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Le tecniche di layout delle pagine CSS ci permettono di prendere elementi contenuti in una pagina web e controllare dove vengono posizionati rispetto ai seguenti fattori: la loro posizione predefinita nel flusso di layout normale, gli altri elementi intorno a loro, il loro contenitore genitore e la viewport/finestra principale.

Le tecniche di layout delle pagine che menzioneremo di seguito e che copriremo in dettaglio attraverso il modulo hanno i loro usi, vantaggi e svantaggi. Nessuna tecnica è progettata per essere usata in isolamento. Comprendendo per cosa è progettato ciascun metodo di layout, sarà in una buona posizione per capire quale metodo è più appropriato per ciascun compito.

## Flusso di layout normale

Gli elementi su una pagina web si dispongono in **flusso normale** se non ha applicato alcun CSS per cambiare il modo in cui si comportano. Può cambiare il comportamento degli elementi regolando la loro posizione nel flusso normale o rimuovendoli del tutto da esso. Iniziare con un documento solido e ben strutturato, leggibile nel flusso normale, è il modo migliore per iniziare qualsiasi pagina web. Garantisce che il suo contenuto sia leggibile anche se l'utente sta utilizzando un browser molto limitato o un dispositivo come un lettore di schermo che legge il contenuto della pagina. Inoltre, poiché il flusso normale è progettato per creare un documento leggibile, iniziando in questo modo si lavora _con_ il documento invece di lottare _contro_ di esso mentre si apportano modifiche al layout.

Prima di approfondire i diversi metodi di layout, vale la pena rivedere alcune delle cose studiate nei moduli precedenti riguardo al flusso normale del documento.

## Come sono disposti gli elementi per impostazione predefinita?

Il processo inizia poiché le caselle degli elementi individuali si dispongono in modo tale che qualsiasi padding, bordo o margine che possono avere sia aggiunto al loro contenuto. Questo è ciò che chiamiamo **modello di scatola**.

Per impostazione predefinita, il contenuto di un {{Glossary("Block-level_content", "elemento a livello di blocco")}} riempie lo spazio inline disponibile dell'elemento genitore che lo contiene, crescendo lungo la dimensione blocco per ospitare il suo contenuto. La dimensione degli {{Glossary("Inline-level_content", "elementi a livello di linea")}} è solo la dimensione del loro contenuto. Può impostare {{cssxref("width")}} o {{cssxref("height")}} su alcuni elementi che hanno un valore predefinito della proprietà {{cssxref("display")}} `inline`, come {{HTMLElement("img")}}, ma il valore `display` rimarrà comunque `inline`.

Se vuole controllare la proprietà `display` di un elemento a livello di linea in questo modo, utilizzi CSS per impostarlo affinché si comporti come un elemento a livello di blocco (ad esempio, con `display: block;` o `display: inline-block;`, che mescola caratteristiche di entrambi).

Questo spiega come sono strutturati gli elementi individualmente, ma per quanto riguarda il modo in cui sono strutturati quando interagiscono l'uno con l'altro? Il flusso di layout normale (menzionato nell'articolo di introduzione al layout) è il sistema attraverso cui gli elementi sono posizionati all'interno della viewport del browser. Per impostazione predefinita, gli elementi a livello di blocco sono disposti nella _direzione del flusso a blocchi_, che si basa sulla [modalità di scrittura](/it/docs/Web/CSS/writing-mode) del genitore (_iniziale_: horizontal-tb). Ogni elemento apparirà su una nuova linea sotto l'ultimo, con ciascuno separato da qualsiasi margine specificato. In inglese, per esempio, (o in qualsiasi altra modalità di scrittura orizzontale, da cima a fondo) gli elementi a livello di blocco sono disposti verticalmente.

Gli elementi inline si comportano in modo diverso. Non appaiono su nuove linee; invece, si trovano tutti sulla stessa linea insieme a qualsiasi contenuto di testo adiacente (o ancorato) finché c'è spazio per farlo all'interno della larghezza dell'elemento genitore a livello di blocco. Se non c'è spazio, il contenuto che trabocca scenderà a una nuova linea.

Se due elementi verticalmente adiacenti hanno entrambi un margine impostato su di essi e i loro margini si toccano, il più grande dei due margini rimane e il più piccolo scompare. Questo è noto come [**collasso dei margini**](/it/docs/Web/CSS/CSS_box_model/Mastering_margin_collapsing). Il collasso dei margini è rilevante solo nella **direzione verticale**.

### Esempio di flusso normale

Diamo un'occhiata a un semplice esempio che spiega tutto questo:

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

Noti come l'HTML sia visualizzato nell'esatto ordine in cui appare nel codice sorgente, con gli elementi a blocco impilati uno sopra l'altro.

Per molti degli elementi sulla sua pagina, il flusso normale creerà esattamente il layout di cui ha bisogno. Tuttavia, per layout più complessi, sarà necessario modificare questo comportamento predefinito utilizzando alcuni degli strumenti disponibili in CSS. Iniziare con un documento HTML ben strutturato è molto importante perché si può quindi lavorare con il modo in cui le cose sono disposte per impostazione predefinita piuttosto che lottare contro di esso.

## Sovrascrivere il flusso normale

I metodi che possono sovrascrivere il flusso normale e cambiare il modo in cui gli elementi sono disposti in CSS, che copriremo in dettaglio in questo modulo, sono:

- La proprietà {{cssxref("display")}}
  - : I valori standard come `block`, `inline` o `inline-block` possono cambiare il modo in cui gli elementi si comportano nel flusso normale, ad esempio facendo sì che un elemento a livello di blocco si comporti come un elemento a livello di linea (abbiamo coperto questi nel corso [Box model](/it/docs/Learn_web_development/Core/Styling_basics/Box_model#block_and_inline_boxes)).
- Float
  - : L'applicazione di un valore di {{cssxref("float")}} come `left` può far sì che gli elementi a livello di blocco si avvicinino a un lato di un elemento, come nel caso delle immagini che a volte hanno un testo fluttuante attorno a loro nei layout di riviste.
- Posizionamento
  - : La proprietà {{cssxref("position")}} le consente di controllare con precisione la disposizione delle scatole all'interno di altre scatole. Il posizionamento `static` è l'impostazione predefinita nel flusso normale, ma si può far sì che gli elementi siano disposti diversamente usando altri valori, ad esempio fissandoli in cima alla viewport del browser usando `position: fixed`.
- Sistemi di layout specifici accessibili tramite `display`
  - : Abbiamo anche metodi di layout interi che sono abilitati tramite valori di `display` specifici. I più importanti che dovrebbe conoscere sono [griglia CSS](/it/docs/Learn_web_development/Core/CSS_layout/Grids) e [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox), che alterano entrambi il modo in cui gli elementi figlio sono disposti all'interno dei genitori.
- Design responsivo
  - : Il design responsivo si riferisce alla creazione di layout che si adattano ai diversi dispositivi su cui viene visualizzata la pagina web (ad esempio, desktop e telefoni cellulari). Il design responsivo non fornisce strumenti di layout specifici; il suo componente più significativo è la regola {{cssxref("@media")}}, che consente di applicare layout diversi in base agli attributi del dispositivo come larghezza dello schermo o risoluzione.

### Altre tecniche di layout

Ci sono altre tecniche di layout che sono meno comunemente utilizzate, che non copriremo in questo modulo:

- [Layout della tabella](/it/docs/Web/CSS/CSS_table)
  - : Le funzionalità progettate per lo styling di parti di una tabella HTML possono essere utilizzate su elementi non tabelle usando `display: table` e proprietà associate.
- [Layout multi-colonna](/it/docs/Web/CSS/CSS_multicol_layout)
  - : Le proprietà di layout multi-colonna possono causare la disposizione del contenuto di un blocco in colonne, come potrebbe vedere in un giornale.

## Riepilogo

Questo articolo ha fornito un breve riassunto di tutte le tecnologie di layout che dovrebbe conoscere a questo punto del suo apprendimento! Continui a leggere per ulteriori informazioni su ciascuna tecnologia individuale. Nel prossimo articolo, esamineremo i float.

{{NextMenu("Learn_web_development/Core/CSS_layout/Floats", "Learn_web_development/Core/CSS_layout")}}
