---
title: Introduzione al layout CSS
short-title: Introduction
slug: Learn_web_development/Core/CSS_layout/Introduction
l10n:
  sourceCommit: 85fccefc8066bd49af4ddafc12c77f35265c7e2d
---

{{NextMenu("Learn_web_development/Core/CSS_layout/Floats", "Learn_web_development/Core/CSS_layout")}}

Questa lezione ripassa alcune delle funzionalità di layout CSS già affrontate nei moduli precedenti, come i diversi valori di {{cssxref("display")}}, e introduce alcuni dei concetti che verranno trattati nel corso di questo modulo. Approfondisce inoltre il concetto di flusso normale.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Fondamenti dello stile CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Stile fondamentale di testo e font</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Riconoscere i metodi utilizzati per implementare layout di pagina moderni.</li>
          <li>Comprendere che il flusso normale è il modo predefinito con cui un browser dispone i contenuti block e inline.</li>
          <li>Sapere che proprietà come <code>display</code>, <code>float</code> e <code>position</code> sono pensate per modificare il modo in cui il browser dispone i contenuti.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Le tecniche di layout delle pagine CSS consentono di prendere gli elementi contenuti in una pagina web e controllare dove vengono posizionati rispetto ai seguenti fattori: la loro posizione predefinita nel flusso di layout normale, gli altri elementi circostanti, il loro contenitore padre e la viewport/finestra principale.

Le tecniche di layout della pagina che verranno menzionate di seguito e trattate in dettaglio nel modulo hanno ciascuna usi, vantaggi e svantaggi. Nessuna tecnica è progettata per essere utilizzata isolatamente. Comprendendo lo scopo di ciascun metodo di layout, sarà più semplice capire quale metodo sia più appropriato per ogni attività.

## Flusso di layout normale

Gli elementi di una pagina web vengono disposti nel **flusso normale** se non è stato applicato alcun CSS per modificare il loro comportamento. È possibile cambiare il comportamento degli elementi regolando la loro posizione nel flusso normale oppure rimuovendoli completamente da esso. Iniziare qualsiasi pagina web con un documento solido e ben strutturato, leggibile nel flusso normale, è il modo migliore per procedere. Ciò garantisce che il contenuto sia leggibile anche se l'utente utilizza un browser molto limitato o un dispositivo come un lettore di schermo che legge ad alta voce il contenuto della pagina. Inoltre, poiché il flusso normale è progettato per creare un documento leggibile, iniziando in questo modo si lavora _con_ il documento anziché lottare _contro_ di esso durante le modifiche al layout.

Prima di approfondire i diversi metodi di layout, vale la pena ripassare alcuni degli argomenti studiati nei moduli precedenti riguardo al flusso normale del documento.

## Come vengono disposti gli elementi per impostazione predefinita?

Il processo inizia quando le box dei singoli elementi vengono disposte in modo tale che eventuali padding, border o margin siano aggiunti al loro contenuto. Questo è ciò che viene chiamato **box model**.

Per impostazione predefinita, il contenuto di un {{Glossary("Block-level_content", "elemento a livello block")}} riempie lo spazio inline disponibile dell'elemento padre che lo contiene, espandendosi lungo la dimensione block per accogliere il proprio contenuto. La dimensione degli {{Glossary("Inline-level_content", "elementi a livello inline")}} corrisponde semplicemente alla dimensione del loro contenuto. È possibile impostare {{cssxref("width")}} o {{cssxref("height")}} su alcuni elementi che hanno un valore predefinito della proprietà {{cssxref("display")}} pari a `inline`, come {{HTMLElement("img")}}, ma il valore di `display` rimarrà comunque `inline`.

Se si desidera controllare in questo modo la proprietà `display` di un elemento a livello inline, usare CSS per impostarla affinché si comporti come un elemento a livello block, ad esempio con `display: block;` o `display: inline-block;`, che combina caratteristiche di entrambi.

Questo spiega come gli elementi siano strutturati individualmente, ma che dire di come sono strutturati quando interagiscono tra loro? Il flusso di layout normale, menzionato nell'articolo introduttivo sul layout, è il sistema con cui gli elementi vengono collocati all'interno della viewport del browser. Per impostazione predefinita, gli elementi a livello block vengono disposti nella _direzione del flusso block_, che si basa sulla [modalità di scrittura](/it/docs/Web/CSS/Reference/Properties/writing-mode) del padre (_iniziale_: horizontal-tb). Ogni elemento apparirà su una nuova riga sotto quello precedente, separato dagli altri in base al margin specificato. In italiano, per esempio, o in qualsiasi altra modalità di scrittura orizzontale dall'alto verso il basso, gli elementi a livello block sono disposti verticalmente.

Gli elementi inline si comportano diversamente. Non appaiono su nuove righe; restano invece tutti sulla stessa riga insieme a qualsiasi contenuto testuale adiacente, o mandato a capo, finché vi è spazio sufficiente all'interno della larghezza dell'elemento padre a livello block. Se non c'è spazio sufficiente, il contenuto in eccesso verrà spostato su una nuova riga.

Se due elementi adiacenti verticalmente hanno entrambi un margin impostato e i loro margin si toccano, il maggiore dei due margin rimane e quello minore scompare. Questo fenomeno è noto come [**collasso dei margin**](/it/docs/Web/CSS/Guides/Box_model/Margin_collapsing).
Il collasso dei margin è rilevante solo nella **direzione verticale**.

### Esempio di flusso normale

Vediamo un semplice esempio che illustra tutto questo:

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

Si noti come l'HTML venga visualizzato nell'esatto ordine in cui appare nel codice sorgente, con gli elementi block impilati uno sopra l'altro.

Per molti degli elementi nella pagina, il flusso normale creerà esattamente il layout necessario. Tuttavia, per layout più complessi sarà necessario modificare questo comportamento predefinito utilizzando alcuni degli strumenti disponibili in CSS. Iniziare con un documento HTML ben strutturato è molto importante, perché consente di lavorare con il modo in cui gli elementi vengono disposti per impostazione predefinita, anziché contrastarlo.

## Sovrascrivere il flusso normale

I metodi che possono sovrascrivere il flusso normale e modificare il modo in cui gli elementi vengono disposti in CSS, trattati in dettaglio in questo modulo, sono:

- La proprietà {{cssxref("display")}}
  - : I valori standard come `block`, `inline` o `inline-block` possono modificare il comportamento degli elementi nel flusso normale, per esempio facendo comportare un elemento a livello block come un elemento a livello inline. Questi valori sono stati trattati nella lezione sul [Box model](/it/docs/Learn_web_development/Core/Styling_basics/Box_model#block_and_inline_boxes).
- Float
  - : Applicare un valore {{cssxref("float")}} come `left` può far sì che gli elementi a livello block si dispongano lungo un lato di un elemento, come accade alle immagini che talvolta hanno il testo disposto attorno a esse nei layout delle riviste.
- Posizionamento
  - : La proprietà {{cssxref("position")}} consente di controllare con precisione la collocazione delle box all'interno di altre box. Il posizionamento `static` è l'impostazione predefinita nel flusso normale, ma è possibile disporre gli elementi in modo diverso usando altri valori, ad esempio fissandoli nella parte superiore della viewport del browser con `position: fixed`.
- Sistemi di layout specifici accessibili tramite `display`
  - : Esistono anche interi metodi di layout abilitati tramite valori specifici di `display`. I più importanti da conoscere sono [CSS grid](/it/docs/Learn_web_development/Core/CSS_layout/Grids) e [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox), che modificano entrambi il modo in cui gli elementi figli vengono disposti all'interno dei loro elementi padre.
- Design responsive
  - : Il design responsive consiste nella creazione di layout che si adattano ai diversi dispositivi sui quali viene renderizzata la pagina web, ad esempio computer desktop e telefoni cellulari. Il design responsive non fornisce strumenti di layout specifici propri; il suo componente più significativo è l'at-rule {{cssxref("@media")}}, che consente di applicare layout diversi in base agli attributi del dispositivo, come la larghezza o la risoluzione dello schermo.

### Altre tecniche di layout

Esistono altre tecniche di layout utilizzate meno frequentemente, che non verranno trattate in questo modulo:

- [Layout delle tabelle](/it/docs/Web/CSS/Guides/Table)
  - : Le funzionalità progettate per stilizzare parti di una tabella HTML possono essere utilizzate su elementi non tabella mediante `display: table` e le proprietà associate.
- [Layout multicolonna](/it/docs/Web/CSS/Guides/Multicol_layout)
  - : Le proprietà del layout multicolonna possono disporre il contenuto di un block in colonne, come avviene in un giornale.

## Riepilogo

Questo articolo ha fornito un breve riepilogo di tutte le tecnologie di layout che è necessario conoscere a questo punto del percorso di apprendimento. Proseguire nella lettura per ulteriori informazioni su ciascuna tecnologia. Successivamente verranno esaminati i float.

{{NextMenu("Learn_web_development/Core/CSS_layout/Floats", "Learn_web_development/Core/CSS_layout")}}
