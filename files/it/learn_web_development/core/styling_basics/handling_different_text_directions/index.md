---
title: Gestione di diverse direzioni del testo
short-title: Direzioni multiple del testo
slug: Learn_web_development/Core/Styling_basics/Handling_different_text_directions
l10n:
  sourceCommit: 2b4a2ad5d9ba084a9eaa2f9204102655e7b575c4
---

Molte delle proprietà e dei valori che abbiamo incontrato finora nel nostro apprendimento di CSS sono stati associati alle dimensioni fisiche dello schermo. Creiamo, ad esempio, bordi nella parte superiore, destra, inferiore e sinistra di un riquadro. Queste dimensioni fisiche si adattano molto bene ai contenuti visualizzati orizzontalmente e, per impostazione predefinita, il web tende a supportare meglio le lingue da sinistra a destra (ad esempio, inglese o francese) rispetto alle lingue da destra a sinistra (come l'arabo).

Negli ultimi anni, tuttavia, CSS si è evoluto per supportare meglio le diverse direzionalità dei contenuti, inclusi i contenuti da destra a sinistra ma anche quelli dall'alto verso il basso (come il giapponese): queste diverse direzionalità sono chiamate **modalità di scrittura**. Man mano che si procede nello studio e si inizia a lavorare con il layout, una comprensione delle modalità di scrittura sarà molto utile; per questo motivo le introdurremo ora.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        >, conoscenza di base del
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files"
          >lavoro con i file</a
        >, basi di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Introduzione a HTML</a
        >) e un'idea di come funziona CSS (studiare
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi dello stile CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Comprendere l'importanza delle modalità di scrittura per il CSS moderno.</td>
    </tr>
  </tbody>
</table>

## Cosa sono le modalità di scrittura?

Una modalità di scrittura in CSS indica se il testo scorre orizzontalmente o verticalmente. La proprietà {{cssxref("writing-mode")}} consente di passare da una modalità di scrittura a un'altra. Non è necessario lavorare in una lingua che usa una modalità di scrittura verticale per volerlo fare: è anche possibile modificare la modalità di scrittura di parti del layout per scopi creativi.

Nell'esempio seguente è presente un'intestazione visualizzata con `writing-mode: vertical-rl`. Il testo ora scorre verticalmente. Il testo verticale è comune nel graphic design e può essere un modo per dare un aspetto più interessante al web design.

```html live-sample___simple-vertical
<h1>Play with writing modes</h1>
```

```css live-sample___simple-vertical
body {
  font-family: sans-serif;
  height: 300px;
}
h1 {
  writing-mode: vertical-rl;
  color: white;
  background-color: black;
  padding: 10px;
}
```

{{EmbedLiveSample("simple-vertical", "", "350px")}}

I tre possibili valori della proprietà {{cssxref("writing-mode")}} sono:

- `horizontal-tb`: Direzione del flusso dei blocchi dall'alto verso il basso. Le frasi scorrono orizzontalmente.
- `vertical-rl`: Direzione del flusso dei blocchi da destra a sinistra. Le frasi scorrono verticalmente.
- `vertical-lr`: Direzione del flusso dei blocchi da sinistra a destra. Le frasi scorrono verticalmente.

La proprietà `writing-mode` imposta quindi, in realtà, la direzione in cui gli elementi a livello di blocco vengono visualizzati nella pagina: dall'alto verso il basso, da destra a sinistra oppure da sinistra a destra. Questo determina poi la direzione in cui il testo scorre nelle frasi.

## Modalità di scrittura e layout a blocchi e inline

Abbiamo già parlato del [layout a blocchi e inline](/it/docs/Web/CSS/Guides/Display/Block_and_inline_layout) e del fatto che alcuni elementi vengono visualizzati come elementi a blocco e altri come elementi inline. Come descritto sopra, blocco e inline sono legati alla modalità di scrittura del documento, non allo schermo fisico. I blocchi vengono visualizzati dall'alto verso il basso nella pagina solo se si utilizza una modalità di scrittura che visualizza il testo orizzontalmente, come l'inglese.

Osservando un esempio, questo sarà più chiaro. Nell'esempio seguente sono presenti due riquadri contenenti un'intestazione e un paragrafo. Il primo usa `writing-mode: horizontal-tb`, una modalità di scrittura orizzontale e dall'alto verso il basso. Il secondo usa `writing-mode: vertical-rl`; questa è una modalità di scrittura verticale e da destra a sinistra.

```html live-sample___block-inline
<div class="wrapper">
  <div class="box horizontal">
    <h2>Heading</h2>
    <p>A paragraph demonstrating writing modes in CSS.</p>
  </div>
  <div class="box vertical">
    <h2>Heading</h2>
    <p>A paragraph demonstrating writing modes in CSS.</p>
  </div>
</div>
```

```css live-sample___block-inline
body {
  font-family: sans-serif;
  height: 300px;
}
.wrapper {
  display: flex;
}

.box {
  border: 1px solid #cccccc;
  padding: 0.5em;
  margin: 10px;
}

.horizontal {
  writing-mode: horizontal-tb;
}

.vertical {
  writing-mode: vertical-rl;
}
```

{{EmbedLiveSample("block-inline", "", "350px")}}

Quando si cambia la modalità di scrittura, si cambia quale direzione è block e quale è inline. In una modalità di scrittura `horizontal-tb`, la direzione block va dall'alto verso il basso; in una modalità di scrittura `vertical-rl`, la direzione block va orizzontalmente da destra a sinistra. Quindi la **dimensione block** è sempre la direzione in cui i blocchi vengono visualizzati nella pagina nella modalità di scrittura in uso. La **dimensione inline** è sempre la direzione in cui scorre una frase.

Questa figura mostra le due dimensioni in una modalità di scrittura orizzontale.![Mostra gli assi block e inline per una modalità di scrittura orizzontale.](horizontal-tb.png)

Questa figura mostra le due dimensioni in una modalità di scrittura verticale.

![Mostra gli assi block e inline per una modalità di scrittura verticale.](vertical.png)

Quando si inizia a esaminare il layout CSS, e in particolare i metodi di layout più recenti, questa idea di block e inline diventa molto importante. La riprenderemo più avanti.

### Direzione

Oltre alla modalità di scrittura, esiste anche la direzione del testo. Come menzionato sopra, alcune lingue, come l'arabo, vengono scritte orizzontalmente, ma da destra a sinistra. Non è probabile che questo venga usato in senso creativo — se si vuole allineare qualcosa a destra esistono altri modi per farlo — tuttavia è importante comprenderlo come parte della natura di CSS. Il web non è solo per le lingue visualizzate da sinistra a destra!

Poiché la modalità di scrittura e la direzione del testo possono cambiare, i metodi di layout CSS più recenti non fanno riferimento a sinistra e destra, né ad alto e basso. Parlano invece di _inizio_ e _fine_, insieme a questa idea di inline e block. Non è necessario preoccuparsene troppo per ora, ma è bene tenere a mente queste idee quando si inizia a esaminare il layout: saranno davvero utili per comprendere CSS.

## Proprietà e valori logici

Il motivo per cui parliamo di modalità di scrittura e direzione a questo punto dell'apprendimento è che abbiamo già esaminato molte proprietà legate alle dimensioni fisiche dello schermo, che risultano più intuitive in una modalità di scrittura orizzontale.

Osserviamo di nuovo i due riquadri: uno con modalità di scrittura `horizontal-tb` e uno con `vertical-rl`. A entrambi i riquadri è stata assegnata una {{cssxref("width")}}. È possibile vedere che, quando il riquadro è in modalità di scrittura verticale, mantiene comunque una larghezza, e questo causa l'overflow del testo.

```html live-sample___width
<div class="wrapper">
  <div class="box horizontal">
    <h2>Heading</h2>
    <p>A paragraph demonstrating writing modes in CSS.</p>
    <p>These boxes have a width.</p>
  </div>
  <div class="box vertical">
    <h2>Heading</h2>
    <p>A paragraph demonstrating writing modes in CSS.</p>
    <p>These boxes have a width.</p>
  </div>
</div>
```

```css live-sample___width
body {
  font-family: sans-serif;
  height: 300px;
}
.wrapper {
  display: flex;
}

.box {
  border: 1px solid #cccccc;
  padding: 0.5em;
  margin: 10px;
  width: 100px;
}

.horizontal {
  writing-mode: horizontal-tb;
}

.vertical {
  writing-mode: vertical-rl;
}
```

{{EmbedLiveSample("width", "", "350px")}}

Ciò che serve realmente in questo scenario è essenzialmente scambiare height e width in base alla modalità di scrittura. In una modalità di scrittura verticale, il riquadro dovrebbe espandersi nella dimensione block proprio come accade nella modalità orizzontale.

Per semplificare questo processo, CSS ha sviluppato recentemente un insieme di proprietà mappate. Queste sostituiscono essenzialmente le proprietà fisiche — elementi come `width` e `height` — con versioni **logiche**, o **relative al flusso**.

La proprietà mappata a `width` in una modalità di scrittura orizzontale si chiama {{cssxref("inline-size")}}: fa riferimento alla dimensione inline. La proprietà per `height` si chiama {{cssxref("block-size")}} e rappresenta la dimensione block. È possibile vedere come funziona nell'esempio seguente, in cui `width` è stata sostituita con `inline-size`.

```html live-sample___inline-size
<div class="wrapper">
  <div class="box horizontal">
    <h2>Heading</h2>
    <p>A paragraph demonstrating writing modes in CSS.</p>
    <p>These boxes have inline-size.</p>
  </div>
  <div class="box vertical">
    <h2>Heading</h2>
    <p>A paragraph demonstrating writing modes in CSS.</p>
    <p>These boxes have inline-size.</p>
  </div>
</div>
```

```css live-sample___inline-size
.wrapper {
  display: flex;
}

.box {
  border: 1px solid #cccccc;
  padding: 0.5em;
  margin: 10px;
  inline-size: 100px;
}

.horizontal {
  writing-mode: horizontal-tb;
}

.vertical {
  writing-mode: vertical-rl;
}
```

{{EmbedLiveSample("inline-size", "", "300px")}}

### Proprietà logiche per margin, border e padding

Nelle ultime due lezioni abbiamo imparato a conoscere il box model CSS e i bordi CSS. Nelle proprietà margin, border e padding si trovano molti esempi di proprietà fisiche, ad esempio {{cssxref("margin-top")}}, {{cssxref("padding-left")}} e {{cssxref("border-bottom")}}. Allo stesso modo in cui esistono mappature per width e height, esistono mappature anche per queste proprietà.

La proprietà `margin-top` è mappata a {{cssxref("margin-block-start")}}: questa fa sempre riferimento al margin all'inizio della dimensione block.

La proprietà {{cssxref("padding-left")}} è mappata a {{cssxref("padding-inline-start")}}, il padding applicato all'inizio della direzione inline. Questo corrisponde al punto in cui iniziano le frasi in quella modalità di scrittura. La proprietà {{cssxref("border-bottom")}} è mappata a {{cssxref("border-block-end")}}, che rappresenta il border alla fine della dimensione block.

Di seguito è possibile vedere un confronto tra proprietà fisiche e logiche.

Se si modifica la modalità di scrittura dei riquadri impostando la proprietà `writing-mode` su `.box` a `vertical-rl`, sarà possibile vedere come le proprietà fisiche rimangano associate alla loro direzione fisica, mentre le proprietà logiche cambino con la modalità di scrittura.

È inoltre possibile vedere che {{htmlelement("Heading_Elements", "h2")}} ha un `border-bottom` nero. È possibile capire come fare in modo che il bordo inferiore si trovi sempre sotto il testo in entrambe le modalità di scrittura?

```html live-sample___logical-mbp
<div class="wrapper">
  <div class="box physical">
    <h2>Physical Properties</h2>
    <p>A paragraph demonstrating logical properties in CSS.</p>
  </div>
  <div class="box logical">
    <h2>Logical Properties</h2>
    <p>A paragraph demonstrating logical properties in CSS.</p>
  </div>
</div>
```

```css live-sample___logical-mbp
.wrapper {
  display: flex;
  border: 5px solid #cccccc;
}

.box {
  margin-right: 30px;
  inline-size: 200px;
  writing-mode: horizontal-tb;
}

.logical {
  margin-block-start: 20px;
  padding-inline-end: 2em;
  padding-block-start: 2px;
  border-block-start: 5px solid pink;
  border-inline-end: 10px dotted rebeccapurple;
  border-block-end: 1em double orange;
  border-inline-start: 1px solid black;
}

.physical {
  margin-top: 20px;
  padding-right: 2em;
  padding-top: 2px;
  border-top: 5px solid pink;
  border-right: 10px dotted rebeccapurple;
  border-bottom: 1em double orange;
  border-left: 1px solid black;
}

h2 {
  border-bottom: 5px solid black;
}
```

{{EmbedLiveSample("logical-mbp", "", "200px")}}

Esiste un numero enorme di proprietà considerando tutte le proprietà longhand individuali per i border; è possibile vedere tutte le proprietà mappate nella pagina MDN dedicata alle [proprietà e ai valori logici](/it/docs/Web/CSS/Guides/Logical_properties_and_values).

### Valori logici

Finora abbiamo esaminato i nomi delle proprietà logiche. Esistono anche alcune proprietà che accettano i valori fisici `top`, `right`, `bottom` e `left`. Anche questi valori hanno delle mappature verso valori logici: `block-start`, `inline-end`, `block-end` e `inline-start`.

Ad esempio, è possibile applicare il float a un'immagine verso sinistra per far sì che il testo fluisca attorno all'immagine. È possibile sostituire `left` con `inline-start`, come mostrato nell'esempio seguente.

Modificare la modalità di scrittura di questo esempio in `vertical-rl` per vedere cosa accade all'immagine. Modificare `inline-start` in `inline-end` per cambiare il float:

```html live-sample___float
<div class="wrapper">
  <div class="box logical">
    <img
      alt="star"
      src="https://mdn.github.io/shared-assets/images/examples/big-star.png" />
    <p>
      This box uses logical properties. The star image has been floated
      inline-start, it also has a margin on the inline-end and block-end.
    </p>
  </div>
</div>
```

```css live-sample___float
.wrapper {
  display: flex;
}

.box {
  margin: 10px;
  padding: 0.5em;
  border: 1px solid #cccccc;
  inline-size: 200px;
  writing-mode: horizontal-tb;
}

img {
  float: inline-start;
  margin-inline-end: 10px;
  margin-block-end: 10px;
}
```

{{EmbedLiveSample("float", "", "200px")}}

Qui vengono inoltre usati valori logici di margin per assicurare che il margin si trovi nella posizione corretta, indipendentemente dalla modalità di scrittura.

### È necessario usare proprietà fisiche o logiche?

Le proprietà e i valori logici sono più recenti delle loro controparti fisiche e, pertanto, sono stati implementati nei browser solo di recente. È possibile controllare ogni pagina delle proprietà su MDN per vedere fino a quale versione precedente arriva il supporto dei browser. Se non si utilizzano più modalità di scrittura, per ora potrebbe essere preferibile usare le versioni fisiche. Tuttavia, in definitiva, ci si aspetta che le persone passino alle versioni logiche per la maggior parte dei casi, poiché sono molto sensate quando si iniziano a gestire anche metodi di layout come flexbox e grid.

## Riepilogo

I concetti illustrati in questa lezione stanno diventando sempre più importanti in CSS. Comprendere la direzione block e inline — e come il flusso del testo cambi quando cambia la modalità di scrittura — sarà molto utile in futuro. Aiuterà a comprendere CSS anche se non verrà mai utilizzata una modalità di scrittura diversa da quella orizzontale.
