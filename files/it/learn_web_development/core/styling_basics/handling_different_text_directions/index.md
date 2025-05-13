---
title: Gestione delle diverse direzioni del testo
short-title: Molteplici direzioni del testo
slug: Learn_web_development/Core/Styling_basics/Handling_different_text_directions
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Molte delle proprietà e dei valori che abbiamo incontrato finora nel nostro apprendimento del CSS sono legati alle dimensioni fisiche dello schermo. Ad esempio, creiamo bordi in alto, a destra, in basso e a sinistra di una scatola. Queste dimensioni fisiche si adattano perfettamente al contenuto visualizzato orizzontalmente, e per impostazione predefinita il web tende a supportare meglio le lingue da sinistra a destra (ad esempio, inglese o francese) rispetto alle lingue da destra a sinistra (come l'arabo).

Negli ultimi anni, tuttavia, il CSS si è evoluto per supportare meglio le diverse direzioni del contenuto, comprese quelle da destra a sinistra ma anche dall'alto verso il basso (come il giapponese) — queste diverse direzionalità sono chiamate **modalità di scrittura**. Man mano che progredisce nei suoi studi e inizia a lavorare con il layout, una comprensione delle modalità di scrittura le sarà molto utile, quindi le introdurremo ora.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        >, conoscenza di base di
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files"
          >lavorare con i file</a
        >, nozioni di base di HTML (studia
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Introduzione a HTML</a
        >), e un'idea di come funziona il CSS (studia
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo stile CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Comprendere l'importanza delle modalità di scrittura nel CSS moderno.</td>
    </tr>
  </tbody>
</table>

## Cosa sono le modalità di scrittura?

Una modalità di scrittura in CSS si riferisce al fatto che il testo venga scritto orizzontalmente o verticalmente. La proprietà {{cssxref("writing-mode")}} ci consente di passare da una modalità di scrittura all'altra. Non è necessario lavorare in una lingua che utilizza una modalità di scrittura verticale per volerlo fare — potrebbe anche cambiare la modalità di scrittura di parti del suo layout per scopi creativi.

Nell'esempio sottostante abbiamo un'intestazione visualizzata utilizzando `writing-mode: vertical-rl`. Il testo ora scorre verticalmente. Il testo verticale è comune nel design grafico e può essere un modo per aggiungere un aspetto e una sensazione più interessanti al suo design web.

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

I tre valori possibili per la proprietà [`writing-mode`](/it/docs/Web/CSS/writing-mode) sono:

- `horizontal-tb`: Direzione del flusso di blocco dall'alto verso il basso. Le frasi scorrono orizzontalmente.
- `vertical-rl`: Direzione del flusso di blocco da destra a sinistra. Le frasi scorrono verticalmente.
- `vertical-lr`: Direzione del flusso di blocco da sinistra a destra. Le frasi scorrono verticalmente.

Quindi la proprietà `writing-mode` sta in realtà impostando la direzione in cui gli elementi di blocco sono visualizzati sulla pagina — o dall'alto verso il basso, da destra a sinistra, o da sinistra a destra. Questo poi determina la direzione in cui il testo scorre nelle frasi.

## Modalità di scrittura e layout a blocchi e inline

Abbiamo già discusso il [layout a blocchi e inline](/it/docs/Web/CSS/CSS_display/Block_and_inline_layout_in_normal_flow), e il fatto che alcune cose siano visualizzate come elementi di blocco e altre come elementi inline. Come abbiamo visto descritto sopra, blocco e inline sono legati alla modalità di scrittura del documento, e non allo schermo fisico. I blocchi sono visualizzati dall'alto verso il basso della pagina solo se si sta utilizzando una modalità di scrittura che visualizza il testo orizzontalmente, come l'inglese.

Se guardiamo un esempio, questo sarà più chiaro. In questo esempio successivo ho due scatole che contengono un'intestazione e un paragrafo. La prima utilizza `writing-mode: horizontal-tb`, una modalità di scrittura che viene scritta orizzontalmente e dall'alto della pagina al basso. La seconda utilizza `writing-mode: vertical-rl`; questa è una modalità di scrittura che viene scritta verticalmente e da destra a sinistra.

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
  border: 1px solid #ccc;
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

Quando cambiamo la modalità di scrittura, stiamo cambiando quale direzione è di blocco e quale è inline. In una modalità di scrittura `horizontal-tb` la direzione di blocco va dall'alto verso il basso; in una modalità di scrittura `vertical-rl` la direzione di blocco va da destra a sinistra orizzontalmente. Quindi la **dimensione di blocco** è sempre la direzione in cui i blocchi sono visualizzati sulla pagina nella modalità di scrittura in uso. La **dimensione inline** è sempre la direzione in cui una frase scorre.

Questa figura mostra le due dimensioni in una modalità di scrittura orizzontale.![Mostra l'asse a blocchi e inline per una modalità di scrittura orizzontale.](horizontal-tb.png)

Questa figura mostra le due dimensioni in una modalità di scrittura verticale.

![Mostra l'asse a blocchi e inline per una modalità di scrittura verticale.](vertical.png)

Una volta che inizia a guardare il layout CSS, e in particolare i metodi di layout più recenti, questa idea di blocco e inline diventa molto importante. Ci torneremo più avanti.

### Direzione

Oltre alla modalità di scrittura abbiamo anche la direzione del testo. Come menzionato sopra, alcune lingue come l'arabo sono scritte orizzontalmente, ma da destra a sinistra. Questo non è qualcosa che è probabile che si utilizzi per scopi creativi — se si desidera allineare qualcosa a destra ci sono altri modi per farlo — tuttavia è importante capire questo come parte della natura del CSS. Il web non è solo per le lingue visualizzate da sinistra a destra!

Poiché la modalità di scrittura e la direzione del testo possono cambiare, i metodi di layout CSS più recenti non si riferiscono a sinistra e destra, sopra e sotto. Invece parlano di _inizio_ e _fine_ insieme a questa idea di inline e blocco. Non si preoccupi troppo di questo adesso, ma tenga a mente queste idee quando inizia a guardare il layout; le troverà molto utili nella sua comprensione del CSS.

## Proprietà e valori logici

Il motivo per parlare delle modalità di scrittura e della direzione in questo punto del suo apprendimento è che abbiamo già esaminato molte proprietà legate alle dimensioni fisiche dello schermo, e queste hanno più senso quando in una modalità di scrittura orizzontale.

Diamo un'occhiata di nuovo alle nostre due scatole — una con una modalità di scrittura `horizontal-tb` e una con `vertical-rl`. Ho dato a entrambe queste scatole una {{cssxref("width")}}. Si può vedere che quando la scatola è in modalità di scrittura verticale, ha ancora una larghezza, e questo sta causando il traboccamento del testo.

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
  border: 1px solid #ccc;
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

Quello che vogliamo realmente in questo scenario è essenzialmente scambiare altezza con larghezza in base alla modalità di scrittura. Quando siamo in una modalità di scrittura verticale vogliamo che la scatola si espanda nella dimensione di blocco proprio come fa nella modalità orizzontale.

Per rendere questo più facile, il CSS ha recentemente sviluppato un insieme di proprietà mappate. Queste essenzialmente sostituiscono le proprietà fisiche — cose come `width` e `height` — con versioni **logiche**, o **relative al flusso**.

La proprietà mappata a `width` quando in una modalità di scrittura orizzontale è chiamata {{cssxref("inline-size")}} — si riferisce alla dimensione nella dimensione inline. La proprietà per `height` è chiamata {{cssxref("block-size")}} ed è la dimensione nella dimensione di blocco. Può vedere come funziona nell'esempio sottostante dove abbiamo sostituito `width` con `inline-size`.

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
  border: 1px solid #ccc;
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

### Proprietà logiche di margine, bordo e padding

Nelle ultime due lezioni abbiamo appreso riguardo al modello box CSS e ai bordi CSS. Nelle proprietà di margine, bordo e padding troverà molti esempi di proprietà fisiche, ad esempio {{cssxref("margin-top")}}, {{cssxref("padding-left")}}, e {{cssxref("border-bottom")}}. Nello stesso modo in cui abbiamo mappature per larghezza e altezza, ci sono mappature per queste proprietà.

La proprietà `margin-top` è mappata a {{cssxref("margin-block-start")}} — questo si riferirà sempre al margine all'inizio della dimensione di blocco.

La proprietà {{cssxref("padding-left")}} si mappa a {{cssxref("padding-inline-start")}}, il padding che è applicato all'inizio della direzione inline. Questo sarà dove iniziano le frasi in quella modalità di scrittura. La proprietà {{cssxref("border-bottom")}} si mappa a {{cssxref("border-block-end")}}, che è il bordo alla fine della dimensione di blocco.

Può vedere un confronto tra proprietà fisiche e logiche sotto.

Se cambia la modalità di scrittura delle caselle modificando la proprietà `writing-mode` su `.box` a `vertical-rl`, vedrà come le proprietà fisiche rimangono legate alla loro direzione fisica, mentre le proprietà logiche cambiano con la modalità di scrittura.

Può anche vedere che l'{{htmlelement("Heading_Elements", "h2")}} ha un `border-bottom` nero. Può capire come fare in modo che quel bordo inferiore vada sempre sotto il testo in entrambe le modalità di scrittura?

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
  border: 5px solid #ccc;
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

Ci sono un numero enorme di proprietà quando si considerano tutti i singoli longhand del bordo, e può vedere tutte le proprietà mappate sulla pagina MDN per [Proprietà e Valori Logici](/it/docs/Web/CSS/CSS_logical_properties_and_values).

### Valori logici

Finora abbiamo esaminato i nomi delle proprietà logiche. Ci sono anche alcune proprietà che prendono valori fisici di `top`, `right`, `bottom`, e `left`. Questi valori hanno anche mappature, a valori logici — `block-start`, `inline-end`, `block-end`, e `inline-start`.

Ad esempio, può far fluttuare un'immagine a sinistra per far sì che il testo si avvolga attorno all'immagine. Potrebbe sostituire `left` con `inline-start` come mostrato nell'esempio sotto.

Cambi la modalità di scrittura in questo esempio a `vertical-rl` per vedere cosa succede all'immagine. Cambi `inline-start` a `inline-end` per cambiare il float:

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
  border: 1px solid #ccc;
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

Qui stiamo anche usando valori di margine logici per garantire che il margine sia nel posto corretto indipendentemente dalla modalità di scrittura.

### Dovrebbe utilizzare proprietà fisiche o logiche?

Le proprietà e i valori logici sono più recenti rispetto ai loro equivalenti fisici, e quindi sono stati implementati nei browser solo di recente. Può controllare qualsiasi pagina di proprietà su MDN per vedere quanto indietro va il supporto del browser. Se non sta usando più modalità di scrittura, allora per ora potrebbe preferire utilizzare le versioni fisiche. Tuttavia, alla fine ci aspettiamo che le persone passino alle versioni logiche per la maggior parte delle cose, poiché hanno molto senso una volta che si iniziano a gestire anche metodi di layout come flexbox e grid.

## Metta alla prova le sue abilità!

Ha raggiunto la fine di questo articolo, ma può ricordare le informazioni più importanti? Può trovare alcuni test ulteriori per verificare di aver trattenuto queste informazioni prima di procedere — veda [Metta alla prova le sue abilità: Modalità di scrittura e proprietà logiche](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Writing_modes).

## Riepilogo

I concetti spiegati in questa lezione stanno diventando sempre più importanti nel CSS. Una comprensione della direzione di blocco e inline — e di come il flusso del testo cambia con un cambiamento nella modalità di scrittura — sarà molto utile in futuro. Le aiuterà a comprendere il CSS anche se non utilizza mai una modalità di scrittura diversa da quella orizzontale.
