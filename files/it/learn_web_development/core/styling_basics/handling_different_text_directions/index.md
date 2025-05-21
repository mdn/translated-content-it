---
title: Gestire diverse direzioni di testo
short-title: Direzioni di testo multiple
slug: Learn_web_development/Core/Styling_basics/Handling_different_text_directions
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Molte delle proprietà e dei valori che abbiamo incontrato finora nel nostro apprendimento di CSS sono stati legati alle dimensioni fisiche del nostro schermo. Creiamo bordi sulla parte superiore, destra, inferiore e sinistra di un riquadro, ad esempio. Queste dimensioni fisiche si adattano molto bene ai contenuti visualizzati orizzontalmente, e per impostazione predefinita il web tende a supportare meglio le lingue da sinistra a destra (ad esempio, inglese o francese) rispetto a quelle da destra a sinistra (come l'arabo).

Negli ultimi anni, tuttavia, CSS si è evoluto per supportare meglio diversa direzionalità dei contenuti, non solo da destra a sinistra ma anche dall'alto verso il basso (come il giapponese) — queste diverse direzionalità sono chiamate **modi di scrittura**. Man mano che progredisci nei tuoi studi e inizi a lavorare con il layout, comprendere i modi di scrittura ti sarà molto utile, quindi li introduciamo ora.

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
        >, nozioni fondamentali di HTML (studia
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Introduzione a HTML</a
        >), e un'idea di come funziona CSS (studia
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni fondamentali di CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Comprendere l'importanza dei modi di scrittura nel CSS moderno.</td>
    </tr>
  </tbody>
</table>

## Cosa sono i modi di scrittura?

Un modo di scrittura in CSS si riferisce a se il testo scorre orizzontalmente o verticalmente. La proprietà {{cssxref("writing-mode")}} ci consente di passare da un modo di scrittura all'altro. Non è necessario lavorare in una lingua che utilizza un modo di scrittura verticale per voler fare ciò — si potrebbe anche cambiare il modo di scrittura di parti del layout per scopi creativi.

Nell'esempio qui sotto, abbiamo un intestazione visualizzata usando `writing-mode: vertical-rl`. Il testo ora scorre verticalmente. Il testo verticale è comune nel design grafico, e può essere un modo per aggiungere un aspetto più interessante al design del tuo sito web.

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

- `horizontal-tb`: Direzione del flusso dei blocchi dall'alto verso il basso. Le frasi scorrono orizzontalmente.
- `vertical-rl`: Direzione del flusso dei blocchi da destra a sinistra. Le frasi scorrono verticalmente.
- `vertical-lr`: Direzione del flusso dei blocchi da sinistra a destra. Le frasi scorrono verticalmente.

Quindi, la proprietà `writing-mode` in realtà imposta la direzione in cui gli elementi a livello di blocco vengono visualizzati sulla pagina — dall'alto verso il basso, da destra a sinistra, o da sinistra a destra. Questo poi determina la direzione in cui il testo scorre nelle frasi.

## Modi di scrittura e layout a blocchi e in-linea

Abbiamo già discusso il [layout a blocchi e in-linea](/it/docs/Web/CSS/CSS_display/Block_and_inline_layout_in_normal_flow), e il fatto che alcune cose si visualizzano come elementi a blocco mentre altre come elementi in-linea. Come abbiamo visto sopra, il blocco e l'in-linea sono legati al modo di scrittura del documento, e non allo schermo fisico. I blocchi vengono visualizzati dall'alto verso il basso della pagina solo se si utilizza un modo di scrittura che visualizza il testo orizzontalmente, come l'inglese.

Guardando un esempio questo diventerà più chiaro. In questo prossimo esempio ho due riquadri che contengono un'intestazione e un paragrafo. Il primo utilizza `writing-mode: horizontal-tb`, un modo di scrittura orizzontale che va dalla parte superiore della pagina verso il basso. Il secondo utilizza `writing-mode: vertical-rl`; questo è un modo di scrittura verticale che va da destra a sinistra.

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

Quando cambiamo il modo di scrittura, stiamo cambiando quale direzione è il blocco e quale è l'in-linea. In un modo di scrittura `horizontal-tb`, la direzione del blocco va dall'alto verso il basso; in un modo di scrittura `vertical-rl`, la direzione del blocco va da destra a sinistra orizzontalmente. Quindi la **dimensione del blocco** è sempre la direzione in cui i blocchi vengono visualizzati sulla pagina nel modo di scrittura in uso. La **dimensione in-linea** è sempre la direzione in cui scorre una frase.

Questa figura mostra le due dimensioni quando si è in un modo di scrittura orizzontale.![Mostra l'asse del blocco e dell'in-linea per un modo di scrittura orizzontale.](horizontal-tb.png)

Questa figura mostra le due dimensioni in un modo di scrittura verticale.

![Mostra l'asse del blocco e dell'in-linea per un modo di scrittura verticale.](vertical.png)

Quando inizi a guardare al layout CSS, e in particolare ai metodi di layout più recenti, questa idea di blocco e in-linea diventa molto importante. La riaffronteremo più avanti.

### Direzione

Oltre al modo di scrittura abbiamo anche la direzione del testo. Come menzionato sopra, alcune lingue come l'arabo sono scritte orizzontalmente, ma da destra a sinistra. Non è qualcosa che probabilmente utilizzerai in modo creativo — se vuoi allineare qualcosa a destra ci sono altri modi per farlo — tuttavia è importante comprendere questo come parte della natura di CSS. Il web non è solo per le lingue che vengono visualizzate da sinistra a destra!

Dato che il modo di scrittura e la direzione del testo possono cambiare, i metodi di layout CSS più recenti non si riferiscono a sinistra e destra, e in alto e in basso. Invece parlano di _inizio_ e _fine_ insieme a questa idea di in-linea e blocco. Non preoccuparti troppo di questo adesso, ma tieni queste idee in mente mentre inizi a guardare al layout; ti sarà davvero utile nella comprensione di CSS.

## Proprietà e valori logici

Il motivo per cui parliamo di modi di scrittura e direzione a questo punto del tuo apprendimento è che abbiamo già esaminato molte proprietà legate alle dimensioni fisiche dello schermo, e queste hanno più senso in un modo di scrittura orizzontale.

Diamo un'occhiata di nuovo ai nostri due riquadri — uno con un modo di scrittura `horizontal-tb` e uno con `vertical-rl`. Ho dato a entrambi questi riquadri una {{cssxref("width")}}. Puoi vedere che quando il riquadro è nel modo di scrittura verticale, ha ancora una larghezza, e questo causa l'overflow del testo.

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

Ciò che vogliamo davvero in questo scenario è essenzialmente scambiare altezza con larghezza in accordo con il modo di scrittura. Quando siamo in un modo di scrittura verticale vogliamo che il riquadro si espanda nella dimensione del blocco proprio come fa nel modo orizzontale.

Per facilitare questo, CSS ha recentemente sviluppato un set di proprietà mappate. Queste essenzialmente sostituiscono le proprietà fisiche — cose come `width` e `height` — con versioni **logiche**, o **relative al flusso**.

La proprietà mappata a `width` quando si è in un modo di scrittura orizzontale è chiamata {{cssxref("inline-size")}} — si riferisce alla dimensione nella dimensione in-linea. La proprietà per `height` è chiamata {{cssxref("block-size")}} ed è la dimensione nella dimensione del blocco. Puoi vedere come funziona nell'esempio qui sotto dove abbiamo sostituito `width` con `inline-size`.

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

Nelle ultime due lezioni abbiamo imparato a conoscere il modello di riquadro CSS e i bordi CSS. Nelle proprietà di margine, bordo e padding troverai molte istanze di proprietà fisiche, ad esempio {{cssxref("margin-top")}}, {{cssxref("padding-left")}}, e {{cssxref("border-bottom")}}. Allo stesso modo in cui abbiamo mappature per larghezza e altezza esistono mappature per queste proprietà.

La proprietà `margin-top` è mappata a {{cssxref("margin-block-start")}} — questa si riferirà sempre al margine all'inizio della dimensione del blocco.

La proprietà {{cssxref("padding-left")}} è mappata a {{cssxref("padding-inline-start")}}, il padding che viene applicato all'inizio della direzione in-linea. Questo sarà dove iniziano le frasi in quel modo di scrittura. La proprietà {{cssxref("border-bottom")}} è mappata a {{cssxref("border-block-end")}}, che è il bordo alla fine della dimensione del blocco.

Puoi vedere un confronto tra proprietà fisiche e logiche qui sotto.

Se cambi il modo di scrittura dei riquadri cambiando la proprietà `writing-mode` su `.box` in `vertical-rl`, vedrai come le proprietà fisiche restano legate alla loro direzione fisica, mentre le proprietà logiche cambiano con il modo di scrittura.

Puoi anche notare che l'elemento di intestazione {{htmlelement("Heading_Elements", "h2")}} ha un bordo nero in basso. Puoi capire come far sì che quel bordo inferiore vada sempre sotto il testo in entrambi i modi di scrittura?

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

Ci sono un numero enorme di proprietà quando consideri tutti i dettagli specifici dei bordi, e puoi vedere tutte le proprietà mappate sulla pagina MDN dedicata alle [Proprietà e Valori Logici](/it/docs/Web/CSS/CSS_logical_properties_and_values).

### Valori logici

Finora abbiamo esaminato i nomi delle proprietà logiche. Esistono anche alcune proprietà che prendono valori fisici di `top`, `right`, `bottom`, e `left`. Anche questi valori hanno mappature, a valori logici — `block-start`, `inline-end`, `block-end`, e `inline-start`.

Ad esempio, puoi far fluttuare a sinistra un'immagine per fare in modo che il testo si avvolga attorno ad essa. Potresti sostituire `left` con `inline-start` come mostrato nell'esempio qui sotto.

Cambia il modo di scrittura in questo esempio a `vertical-rl` per vedere cosa succede all'immagine. Cambia `inline-start` in `inline-end` per cambiare il float:

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

Qui stiamo anche utilizzando valori di margine logici per garantire che il margine sia nel posto corretto indipendentemente dal modo di scrittura.

### Dovresti usare proprietà fisiche o logiche?

Le proprietà e i valori logici sono più recenti dei loro equivalenti fisici, e pertanto sono stati implementati nei browser solo di recente. Puoi controllare qualsiasi pagina delle proprietà su MDN per vedere fino a quale versione del browser arriva il supporto. Se non stai utilizzando più modi di scrittura, per ora potresti preferire utilizzare le versioni fisiche. Tuttavia, in ultima analisi, ci aspettiamo che le persone passino alle versioni logiche per la maggior parte delle cose, poiché hanno molto senso una volta che inizi a utilizzare metodi di layout come flexbox e grid.

## Testa le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare di aver assimilato queste informazioni prima di procedere — vedi [Testa le tue abilità: Modi di scrittura e proprietà logiche](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Writing_modes).

## Sommario

I concetti spiegati in questa lezione stanno diventando sempre più importanti in CSS. Una comprensione della direzione del blocco e dell'in-linea — e di come il flusso del testo cambia con un cambiamento nel modo di scrittura — sarà molto utile in avanti. Ti aiuterà a comprendere CSS anche se non utilizzi mai un modo di scrittura diverso da uno orizzontale.
