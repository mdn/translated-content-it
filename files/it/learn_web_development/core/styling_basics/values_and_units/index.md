---
title: Valori e unità CSS
short-title: Valori e unità
slug: Learn_web_development/Core/Styling_basics/Values_and_units
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Handling_conflicts", "Learn_web_development/Core/Styling_basics/Sizing", "Learn_web_development/Core/Styling_basics")}}

Le regole CSS contengono [dichiarazioni](/it/docs/Web/CSS/CSS_syntax/Syntax#css_declarations), che a loro volta sono composte da proprietà e valori. Ogni proprietà utilizzata in CSS ha un **tipo di valore** che descrive quali tipi di valori può avere. In questa lezione, esamineremo alcuni dei tipi di valori più frequentemente usati, cosa sono e come funzionano.

> [!NOTE]
> Ogni [pagina delle proprietà CSS](/it/docs/Web/CSS/Reference#index) ha una sezione sintassi che elenca i tipi di valori che puoi usare con quella proprietà.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base HTML</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Getting_started">Sintassi di base CSS</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors">Selettori CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere che i valori delle proprietà possono assumere molti tipi diversi e quali tipi rappresentano.</li>
          <li>Familiarizzarsi con l'uso dei tipi fondamentali: numeri, lunghezze, percentuali, colori, immagini, posizioni, stringhe e identificatori e funzioni.</li>
          <li>Comprendere cosa sono le unità assolute e relative e la differenza tra di esse.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è un valore CSS?

Nelle specifiche CSS e nelle pagine delle proprietà qui su MDN sarai in grado di individuare i tipi di valori poiché saranno circondati da parentesi angolari (`<`, `>`), come [`<color>`](/it/docs/Web/CSS/color_value) o {{cssxref("length")}}. Quando vedi il tipo di valore `<color>` come valido per una determinata proprietà, significa che puoi usare qualsiasi colore valido come valore per quella proprietà, come indicato nella pagina di riferimento [`<color>`](/it/docs/Web/CSS/color_value).

A volte i tipi di valori e le proprietà possono avere nomi uguali o simili — Ad esempio, esiste una proprietà {{cssxref("color")}} e un tipo di dato [`<color>`](/it/docs/Web/CSS/color_value). Puoi usare le parentesi angolari per determinare quale stai studiando in ciascun caso. Anche gli elementi HTML utilizzano parentesi angolari, ma dovrebbe essere chiaro dal contesto quale stai guardando. Se non sei sicuro, prova a cercarlo su MDN.

> [!NOTE]
> Vedrai i tipi di valori CSS indicati come _data types_. I termini sono praticamente intercambiabili — quando vedi qualcosa in CSS indicato come tipo di dato, è davvero solo un modo elegante di dire tipo di valore. Il termine _value_ si riferisce a qualsiasi espressione particolare supportata da un tipo di valore che scegli di usare.

Nell'esempio seguente, abbiamo impostato il colore del nostro titolo utilizzando una parola chiave e lo sfondo utilizzando la funzione `rgb()`:

```css
h1 {
  color: black;
  background-color: rgb(197 93 161);
}
```

Un tipo di valore in CSS è un modo per definire una raccolta di valori consentiti. Ciò significa che se vedi `<color>` come valido non hai bisogno di chiederti quale dei diversi tipi di valore colore può essere usato — parole chiave, valori esadecimali, funzioni `rgb()`, ecc. Puoi utilizzare _qualsiasi_ valore `<color>` disponibile, supponendo che siano supportati dal tuo browser. La pagina su MDN per ciascun valore ti darà informazioni sul supporto del browser. Ad esempio, se guardi la pagina per [`<color>`](/it/docs/Web/CSS/color_value) vedrai che la sezione di compatibilità del browser elenca diversi tipi di valori colore e il supporto per essi.

Diamo un'occhiata ad alcuni dei tipi di valori e unità che potresti incontrare frequentemente, con esempi in modo che tu possa provare diversi valori possibili.

## Numeri, lunghezze e percentuali

Ci sono vari tipi di valori numerici che potresti utilizzare in CSS. I seguenti sono tutti classificati come numerici:

<table class="standard-table no-markdown">
  <thead>
    <tr>
      <th scope="col">Tipo di dato</th>
      <th scope="col">Descrizione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <code><a href="/it/docs/Web/CSS/integer">&#x3C;integer></a></code>
      </td>
      <td>
        Un <code>&#x3C;integer></code> è un numero intero come
        <code>1024</code> o <code>-55</code>.
      </td>
    </tr>
    <tr>
      <td>
        <code><a href="/it/docs/Web/CSS/number">&#x3C;number></a></code>
      </td>
      <td>
        Un <code>&#x3C;number></code> rappresenta un numero decimale — può avere o meno un punto decimale con una componente frazionaria. Per
        esempio, <code>0.255</code>, <code>128</code>, o <code>-1.2</code>.
      </td>
    </tr>
    <tr>
      <td>
        <code
          ><a href="/it/docs/Web/CSS/dimension">&#x3C;dimension></a></code
        >
      </td>
      <td>
        Una <code>&#x3C;dimension></code> è un <code>&#x3C;number></code> con un'unità allegata. Per esempio, <code>45deg</code>, <code>5s</code>,
        o <code>10px</code>. <code>&#x3C;dimension></code> è una categoria 
        che include la {{cssxref("length")}}, <code><a href="/it/docs/Web/CSS/angle">&#x3C;angle></a></code
        >, <code><a href="/it/docs/Web/CSS/time">&#x3C;time></a></code
        >, e
        <code
          ><a href="/it/docs/Web/CSS/resolution">&#x3C;resolution></a></code
        >
        tipi.
      </td>
    </tr>
    <tr>
      <td>{{cssxref("percentage")}}</td>
      <td>
        Un <code>&#x3C;percentage></code> rappresenta una frazione di qualche altro
        valore. Ad esempio, <code>50%</code>. I valori percentuali sono sempre
        relativi a un'altra quantità. Ad esempio, la lunghezza di un elemento è
        relativa alla lunghezza dell'elemento genitore.
      </td>
    </tr>
  </tbody>
</table>

### Lunghezze

Il tipo numerico che incontrerai più frequentemente è {{cssxref("length")}}. Ad esempio, `10px` (pixel) o `30em`. Ci sono due tipi di lunghezze usati in CSS — relative e assolute. È importante conoscere la differenza per capire quanto grandi diventeranno le cose.

#### Unità di lunghezza assolute

Le seguenti sono tutte unità di lunghezza **assolute** — non sono relative a nient'altro e sono generalmente considerate sempre della stessa dimensione.

| Unità | Nome                | Equivalente a            |
| ---- | ------------------- | ------------------------ |
| `cm` | Centimetri          | 1cm = 37.8px = 25.2/64in |
| `mm` | Millimetri          | 1mm = 1/10th di 1cm      |
| `Q`  | Quarti di millimetro| 1Q = 1/40th di 1cm       |
| `in` | Pollici             | 1in = 2.54cm = 96px      |
| `pc` | Punti tipografici   | 1pc = 1/6th di 1in       |
| `pt` | Punti              | 1pt = 1/72nd di 1in      |
| `px` | Pixel              | 1px = 1/96th di 1in      |

La maggior parte di queste unità è più utile quando viene utilizzata per la stampa, piuttosto che per l'output dello schermo. Ad esempio, non usiamo tipicamente `cm` (centimetri) sullo schermo. Il solo valore che userai comunemente è `px` (pixel).

#### Unità di lunghezza relative

Le unità di lunghezza relative sono relative a qualcos'altro. Ad esempio:

- `em` è relativa alla dimensione del carattere di questo elemento, o alla dimensione del carattere dell'elemento genitore quando usata per {{cssxref("font-size")}}. `rem` è relativa alla dimensione del carattere dell'elemento radice.
- `vh` e `vw` sono relative all'altezza e alla larghezza della finestra del viewport, rispettivamente.

Il vantaggio dell'uso delle unità relative è che con una pianificazione attenta puoi fare in modo che la dimensione del testo o di altri elementi si adatti proporzionalmente a tutto il resto sulla pagina. Per un elenco completo delle unità relative disponibili, vedi la pagina di riferimento per il tipo {{cssxref("length")}}.

In questa sezione esploreremo alcune delle unità relative più comuni.

#### Esplorare un esempio

Nell'esempio di seguito, puoi vedere come si comportano alcune unità di lunghezza relative e assolute. La prima casella ha una {{cssxref("width")}} impostata in pixel. Essendo un'unità assoluta, questa larghezza rimarrà la stessa indipendentemente da cos'altro cambi.

La seconda casella ha una larghezza impostata in unità `vw` (larghezza viewport). Questo valore è relativo alla larghezza del viewport, quindi 10vw è il 10 percento della larghezza del viewport. Se modifichi la larghezza della finestra del tuo browser, la dimensione della casella dovrebbe cambiare. Tuttavia questo esempio è incorporato nella pagina usando un [`<iframe>`](/it/docs/Web/HTML/Reference/Elements/iframe), quindi non funzionerà. Per vedere questo in azione dovrai [provare l'esempio dopo averlo aperto in una propria scheda del browser](https://mdn.github.io/css-examples/learn/values-units/length.html).

La terza casella usa unità `em`. Queste sono relative alla dimensione del carattere dell'elemento. Ho impostato una dimensione del carattere di `1em` sul contenitore {{htmlelement("div")}}, che ha una classe `.wrapper`. Cambia questo valore in `1.5em` e vedrai che la dimensione del carattere di tutti gli elementi aumenta, ma solo l'ultimo elemento si allargherà, poiché la sua larghezza è relativa a quella dimensione del carattere.

Dopo aver seguito le istruzioni sopra, prova a giocare ulteriormente con i valori per vedere cosa ottieni.

```html live-sample___length
<div class="wrapper">
  <div class="box px">I am 200px wide</div>
  <div class="box vw">I am 10vw wide</div>
  <div class="box em">I am 10em wide</div>
</div>
```

```css live-sample___length
.box {
  background-color: lightblue;
  border: 5px solid darkblue;
  padding: 10px;
  margin: 1em 0;
}

.wrapper {
  font-size: 1em;
}

.px {
  width: 200px;
}

.vw {
  width: 10vw;
}

.em {
  width: 10em;
}
```

{{EmbedLiveSample("length", "", "250px")}}

#### em e rem

`em` e `rem` sono le due lunghezze relative che probabilmente incontrerai più frequentemente quando dimensionerai qualsiasi cosa, dagli elementi ai testi. Vale la pena comprendere come funzionano e le differenze tra di esse, specialmente quando inizi ad affrontare argomenti più complessi come [stile del testo](/it/docs/Learn_web_development/Core/Text_styling) o [layout CSS](/it/docs/Learn_web_development/Core/CSS_layout). L'esempio qui sotto fornisce una dimostrazione.

L'HTML illustrato di seguito è un insieme di liste nidificate — abbiamo due liste in totale e entrambi gli esempi hanno lo stesso HTML. La sola differenza è che la prima ha una classe di _ems_ e la seconda una classe di _rems_.

Per iniziare, abbiamo impostato 16px come dimensione del carattere sull'elemento `<html>`.

**Per riassumere, l'unità `em` significa "la dimensione del carattere del mio elemento genitore"** se usato per `font-size` (e "la mia dimensione del carattere" quando usato per qualsiasi altra cosa). Gli elementi {{htmlelement("li")}} all'interno dell'{{htmlelement("ul")}} con una `class` di `ems` prendono le loro dimensioni dal loro genitore. Quindi ogni livello di nidificazione successivo diventa progressivamente più grande, poiché ognuno ha la sua dimensione del carattere impostata su `1.3em` — 1.3 volte la dimensione del carattere del suo elemento genitore.

**Per riassumere, l'unità `rem` significa "la dimensione del carattere dell'elemento radice"** (rem sta per "root em"). Gli elementi {{htmlelement("li")}} all'interno dell'{{htmlelement("ul")}} con una `class` di `rems` prendono le loro dimensioni dall'elemento radice (`<html>`). Ciò significa che ogni livello di nidificazione successivo non continua a diventare più grande.

Tuttavia, se cambi la `font-size` dell'elemento `<html>` nel CSS vedrai che tutto il resto cambia in relazione ad esso — sia il testo dimensionato in `rem` che il testo dimensionato in `em`.

```html live-sample___em-rem
<ul class="ems">
  <li>One</li>
  <li>Two</li>
  <li>
    Three
    <ul>
      <li>Three A</li>
      <li>
        Three B
        <ul>
          <li>Three B 2</li>
        </ul>
      </li>
    </ul>
  </li>
</ul>

<ul class="rems">
  <li>One</li>
  <li>Two</li>
  <li>
    Three
    <ul>
      <li>Three A</li>
      <li>
        Three B
        <ul>
          <li>Three B 2</li>
        </ul>
      </li>
    </ul>
  </li>
</ul>
```

```css live-sample___em-rem
html {
  font-size: 16px;
}

.ems li {
  font-size: 1.3em;
}

.rems li {
  font-size: 1.3rem;
}
```

{{EmbedLiveSample("em-rem", "", "400px")}}

#### Unità di altezza linea

`lh` e `rlh` sono unità di lunghezza relative simili a `em` e `rem`. La differenza tra `lh` e `rlh` è che la prima è relativa all'altezza linea dell'elemento stesso, mentre la seconda è relativa all'altezza linea dell'elemento radice, di solito `<html>`.

Utilizzando queste unità, possiamo allineare in modo preciso la decorazione della casella al testo. In questo esempio, usiamo l'unità `lh` per creare linee simili a un blocco note usando [`repeating-linear-gradient()`](/it/docs/Web/CSS/gradient/repeating-linear-gradient). Non importa qual è l'altezza della linea del testo, le linee inizieranno sempre nel posto giusto.

```css hidden
body {
  margin: 0;
  display: grid;
  grid-template-columns: 1fr 1fr;
  padding: 24px;
  gap: 24px;
  background-color: floralwhite;
  font-family: sans-serif;
}

@supports not (height: 1lh) {
  body::before {
    grid-column: 1 / -1;
    padding: 8px;
    border-radius: 4px;
    background-color: tomato;
    color: white;
    content: "You browser doesn't support lh unit just yet";
  }
}
```

```css
p {
  margin: 0;
  background-image: repeating-linear-gradient(
    to top,
    lightskyblue 0 2px,
    transparent 2px 1lh
  );
}
```

```html
<p style="line-height: 2em">
  Summer is a time for adventure, and this year was no exception. I had many
  exciting experiences, but two of my favorites were my trip to the beach and my
  week at summer camp.
</p>

<p style="line-height: 4em">
  At the beach, I spent my days swimming, collecting shells, and building
  sandcastles. I also went on a boat ride and saw dolphins swimming alongside
  us.
</p>
```

{{EmbedLiveSample("line_height_units", "100%", "370")}}

### Percentuali

In molti casi, una percentuale viene trattata allo stesso modo di una lunghezza. Il fatto con le percentuali è che sono sempre impostate in relazione a qualche altro valore. Ad esempio, se imposti la `font-size` di un elemento come percentuale, sarà una percentuale della `font-size` dell'elemento genitore. Se usi una percentuale per un valore `width`, sarà una percentuale della `width` del genitore.

Nell'esempio seguente, le due caselle con dimensioni percentuali e le due caselle con dimensioni in pixel hanno gli stessi nomi di classe. I set sono larghi rispettivamente 40% e 200px.

La differenza è che il secondo set di due caselle è all'interno di un wrapper largo 400 pixel. La seconda casella larga 200px è della stessa larghezza della prima, ma la seconda casella al 40% è ora il 40% di 400px — molto più stretta della prima!

Prova a cambiare la larghezza del contenitore o il valore percentuale per vedere come funziona:

```html live-sample___percentage
<div class="box px">I am 200px wide</div>
<div class="box percent">I am 40% wide</div>
<div class="wrapper">
  <div class="box px">I am 200px wide</div>
  <div class="box percent">I am 40% wide</div>
</div>
```

```css live-sample___percentage
.box {
  background-color: lightblue;
  border: 5px solid darkblue;
  padding: 10px;
  margin: 1em 0;
}
.wrapper {
  width: 400px;
  border: 5px solid rebeccapurple;
}

.px {
  width: 200px;
}

.percent {
  width: 40%;
}
```

{{EmbedLiveSample("percentage", "", "350px")}}

Il prossimo esempio ha dimensioni del carattere impostate in percentuali. Ogni `<li>` ha una `font-size` dell'80%; pertanto, gli elementi delle liste nidificate diventano progressivamente più piccoli poiché ereditano la loro dimensione dal loro genitore.

```html live-sample___percentage-fonts
<ul>
  <li>One</li>
  <li>Two</li>
  <li>
    Three
    <ul>
      <li>Three A</li>
      <li>
        Three B
        <ul>
          <li>Three B 2</li>
        </ul>
      </li>
    </ul>
  </li>
</ul>
```

```css live-sample___percentage-fonts
li {
  font-size: 80%;
}
```

{{EmbedLiveSample("percentage-fonts")}}

Nota che, mentre molti tipi di valori accettano una lunghezza o una percentuale, ci sono alcuni che accettano solo la lunghezza. Puoi vedere quali valori sono accettati sulle pagine di riferimento delle proprietà MDN. Se il valore permesso include {{cssxref("length-percentage")}} allora puoi usare una lunghezza o una percentuale. Se il valore permesso include solo `<length>`, non è possibile usare una percentuale.

### Numeri

Alcuni tipi di valore accettano numeri, senza alcuna unità aggiunta a questi. Un esempio di proprietà che accetta un numero senza unità è la proprietà `opacity`, che controlla l'opacità di un elemento (quanto è trasparente). Questa proprietà accetta un numero tra `0` (completamente trasparente) e `1` (completamente opaco).

Nell'esempio seguente, prova a cambiare il valore di `opacity` con vari valori decimali tra `0` e `1` e osserva come la casella e il suo contenuto diventino più o meno opachi:

```html live-sample___opacity
<div class="wrapper">
  <div class="box">I am a box with opacity</div>
</div>
```

```css live-sample___opacity
.wrapper {
  background-image: url(https://mdn.github.io/shared-assets/images/examples/balloons.jpg);
  background-repeat: no-repeat;
  background-position: bottom left;
  padding: 20px;
}

.box {
  margin: 40px auto;
  width: 200px;
  background-color: lightblue;
  border: 5px solid darkblue;
  padding: 10px;
  opacity: 0.6;
}
```

{{EmbedLiveSample("opacity", "", "210px")}}

> [!NOTE]
> Quando usi un numero in CSS come valore non dovrebbe essere avvolto tra virgolette.

## Colore

I valori del colore possono essere utilizzati in molti posti in CSS, sia che stiate specificando il colore del testo, degli sfondi, dei bordi e molto altro.
Ci sono molti modi per impostare il colore in CSS, permettendoti di controllare molte proprietà eccitanti.

Il sistema standard di colori disponibile nei moderni computer supporta colori a 24 bit, consentendo di visualizzare circa 16,7 milioni di colori distinti tramite una combinazione di diversi canali rosso, verde e blu con 256 valori diversi per canale (256 x 256 x 256 = 16.777.216).

In questa sezione, esamineremo prima i modi più comunemente visti di specificare i colori: usando parole chiave, esadecimale e valori `rgb()`.
Esploreremo anche rapidamente le funzioni aggiuntive del colore, permettendoti di riconoscerle quando le vedi o di sperimentare diversi modi di applicare il colore.

Probabilmente deciderai una palette di colori e poi userai quei colori — e il tuo modo preferito di specificare il colore — per tutto il tuo progetto.
Puoi mescolare e abbinare i modelli di colore, ma di solito è meglio se l'intero progetto utilizza lo stesso metodo di dichiarazione dei colori per la coerenza!

### Parole chiave del colore

Vedrai le parole chiave del colore (o "colori denominati") utilizzate in molti esempi di codice su MDN. Poiché il tipo di dati [`<named-color>`](/it/docs/Web/CSS/named-color) contiene un numero molto limitato di valori di colore, non sono comunemente utilizzati su siti web di produzione con un linguaggio di design sofisticato. D'altra parte, i colori denominati sono usati negli esempi di codice per indicare chiaramente all'utente quale colore è previsto in modo che l'apprendista possa concentrarsi sul contenuto insegnato.

Prova a giocare con valori di colore diversi negli esempi dal vivo qui sotto, per avere un'idea di come funzionano:

```html live-sample___color-keywords
<div class="wrapper">
  <div class="box one">antiquewhite</div>
  <div class="box two">blueviolet</div>
  <div class="box three">greenyellow</div>
</div>
```

```css live-sample___color-keywords
.box {
  padding: 10px;
  margin: 0.5em 0;
  border-radius: 0.5em;
}
.one {
  background-color: antiquewhite;
}

.two {
  background-color: blueviolet;
}

.three {
  background-color: greenyellow;
}
```

{{EmbedLiveSample("color-keywords")}}

### Valori esadecimali RGB

Il prossimo tipo di valore di colore che probabilmente incontrerai sono i codici esadecimali.
L'esadecimale utilizza 16 caratteri da `0-9` e `a-f`, quindi l'intera gamma è `0123456789abcdef`.
Ogni valore di colore esadecimale consiste in un simbolo hash/pound (`#`) seguito da tre o sei caratteri esadecimali (`#fcc` o `#ffc0cb`, per esempio), con uno o due caratteri esadecimali opzionali che rappresentano la trasparenza alfa dei valori di colore precedenti a tre o sei caratteri.

Quando si utilizza l'esadecimale per descrivere i valori RGB, ogni **coppia** di caratteri esadecimali è un numero decimale che rappresenta uno dei canali — rosso, verde e blu — e ci consente di specificare ognuno dei 256 valori disponibili per ciascuno (16 x 16 = 256).
Questi valori sono meno intuitivi rispetto alle parole chiave per definire i colori, ma sono molto più versatili perché puoi rappresentare qualsiasi colore RGB con essi.

Prova a cambiare i valori per vedere come variano i colori:

```html live-sample___color-hex
<div class="wrapper">
  <div class="box one">#02798b</div>
  <div class="box two">#c55da1</div>
  <div class="box three">#128a7d</div>
</div>
```

```css live-sample___color-hex
.box {
  padding: 10px;
  margin: 0.5em 0;
  border-radius: 0.5em;
}

.one {
  background-color: #02798b;
}

.two {
  background-color: #c55da1;
}

.three {
  background-color: #128a7d;
}
```

{{EmbedLiveSample("color-hex")}}

### Valori RGB

Per creare valori RGB direttamente, la funzione [`rgb()`](/it/docs/Web/CSS/color_value/rgb) prende tre parametri che rappresentano i valori dei canali **rosso**, **verde** e **blu** dei colori, con un quarto valore opzionale separato da una barra ('/') che rappresenta l'opacità, in modo simile ai valori esadecimali. La differenza con RGB è che ogni canale è rappresentato non da due cifre esadecimali, ma da un numero decimale tra 0 e 255 o una percentuale tra 0% e 100% inclusi (ma non una miscela dei due).

Riscriviamo il nostro ultimo esempio per utilizzare colori RGB:

```html live-sample___color-rgb
<div class="wrapper">
  <div class="box one">rgb(2 121 139)</div>
  <div class="box two">rgb(197 93 161)</div>
  <div class="box three">rgb(18 138 125)</div>
</div>
```

```css live-sample___color-rgb
.box {
  padding: 10px;
  margin: 0.5em 0;
  border-radius: 0.5em;
}
.one {
  background-color: rgb(2 121 139);
}

.two {
  background-color: rgb(197 93 161);
}

.three {
  background-color: rgb(18 138 125);
}
```

{{EmbedLiveSample("color-rgb")}}

Puoi passare un quarto parametro a `rgb()`, che rappresenta il canale alfa del colore, che controlla l'opacità. Se imposti questo valore su `0` renderà il colore completamente trasparente, mentre `1` lo renderà completamente opaco. I valori intermedi ti danno diversi livelli di trasparenza.

> [!NOTE]
> Impostare un canale alfa su un colore ha una differenza fondamentale rispetto all'utilizzo della proprietà {{cssxref("opacity")}} che abbiamo visto in precedenza. Quando usi l'opacità rendi opaco l'elemento e tutto ciò che si trova all'interno, mentre utilizzando RGB con un parametro alfa rendi opaco solo il colore che stai specificando.

Nell'esempio seguente, abbiamo aggiunto un'immagine di sfondo al blocco contenitore delle nostre caselle colorate. Abbiamo quindi impostato le caselle su diversi valori di opacità — nota come lo sfondo si mostra più quando il valore del canale alfa è più piccolo.
In questo esempio, prova a cambiare i valori del canale alfa per vedere come influisce sull'output colore.

```html live-sample___color-rgba
<div class="wrapper">
  <div class="box one">rgb(2 121 139 / .3)</div>
  <div class="box two">rgb(197 93 161 / .7)</div>
  <div class="box three">rgb(18 138 125 / .9)</div>
</div>
```

```css live-sample___color-rgba
.wrapper {
  background-image: url(https://mdn.github.io/shared-assets/images/examples/balloons.jpg);
  padding: 40px 20px;
}

.box {
  padding: 10px;
  margin: 0.5em 0;
  border-radius: 0.5em;
}

.one {
  background-color: rgb(2 121 139 / 0.3);
}

.two {
  background-color: rgb(197 93 161 / 0.7);
}

.three {
  background-color: rgb(18 138 125 / 0.9);
}
```

{{EmbedLiveSample("color-rgba", "", "250px")}}

### Valori SRGB

Lo spazio colore `sRGB` definisce i colori nei canali **rosso** (r), **verde** (g), e **blu** (b) dello spazio colore.

### Utilizzo delle tonalità per specificare un colore

Se vuoi andare oltre le parole chiave, l'esadecimale e `rgb()` per i colori, potresti voler provare a utilizzare [`<hue>`](/it/docs/Web/CSS/hue).
La tonalità è la proprietà che ci consente di distinguere o vedere la somiglianza tra colori come rosso, arancione, giallo, verde, blu, ecc.
Il concetto chiave è che puoi specificare una tonalità in un [`<angle>`](/it/docs/Web/CSS/angle) perché la maggior parte dei modelli di colore descrive le tonalità utilizzando una {{Glossary("color_wheel", "ruota dei colori")}}.

Ci sono diverse funzioni di colore che includono un componente [`<hue>`](/it/docs/Web/CSS/hue), tra cui `hsl()`, `hwb()`, e [`lch()`](/it/docs/Web/CSS/color_value/lch). Altre funzioni di colore, come [`lab()`](/it/docs/Web/CSS/color_value/lab), definiscono i colori in base a ciò che gli esseri umani possono vedere.

Se vuoi saperne di più su queste funzioni e spazi colore, vedi la [Guida: Applicare i colori agli elementi HTML usando CSS](/it/docs/Web/CSS/CSS_colors/Applying_color), il riferimento [`<color>`](/it/docs/Web/CSS/color_value) che elenca tutti i modi diversi in cui puoi usare i colori in CSS, e il [modulo di colore CSS](/it/docs/Web/CSS/CSS_colors) che fornisce una panoramica di tutti i tipi di colore in CSS e delle proprietà che utilizzano valori di colore.

### HWB

Un ottimo punto di partenza per usare le tonalità in CSS è la funzione [`hwb()`](/it/docs/Web/CSS/color_value/hwb) che specifica un colore `srgb()`.
Le tre parti sono:

- **Hue**: La tonalità base del colore. Questo prende un valore [`<hue>`](/it/docs/Web/CSS/hue) tra 0 e 360, rappresentando gli angoli attorno a una ruota dei colori.
- **Whiteness**: Quanto è bianco il colore? Questo prende un valore dal `0%` (nessuna bianchezza) al `100%` (bianchezza completa).
- **Blackness**: Quanto è nero il colore? Questo prende un valore dal 0% (nessuna nero) al 100% (nero completo).

### HSL

Simile alla funzione `hwb()`, è la funzione [`hsl()`](/it/docs/Web/CSS/color_value/hsl) che specifica anche un colore `srgb()`.
HSL usa `Hue`, oltre a `Saturation` e `Lightness`:

- **Hue**
- **Saturation**: Quanto è saturo il colore? Questo prende un valore dal 0 al 100%, dove 0 rappresenta nessun colore (apparirà come una tonalità di grigio), e 100% rappresenta la saturazione completa del colore.
- **Lightness**: Quanto è chiaro o luminoso il colore? Questo prende un valore dal 0 al 100%, dove 0 rappresenta nessuna luce (apparirà completamente nero) e 100% rappresenta luce completa (apparirà completamente bianco).

Il valore di colore `hsl()` ha anche un quarto valore opzionale, separato dal colore con una barra (`/`), che rappresenta la trasparenza alfa.

Aggiorniamo l'esempio RGB per utilizzare invece colori HSL:

```html live-sample___color-hsl
<div class="wrapper">
  <div class="box one">hsl(188 97% 28%)</div>
  <div class="box two">hsl(321 47% 57%)</div>
  <div class="box three">hsl(174 77% 31%)</div>
</div>
```

```css live-sample___color-hsl
.box {
  padding: 10px;
  margin: 0.5em 0;
  border-radius: 0.5em;
}

.one {
  background-color: hsl(188 97% 28%);
}

.two {
  background-color: hsl(321 47% 57%);
}

.three {
  background-color: hsl(174 77% 31%);
}
```

{{EmbedLiveSample("color-hsl")}}

Proprio come con `rgb()`, puoi passare un parametro alfa a `hsl()` per specificare l'opacità:

```html live-sample___color-hsla
<div class="wrapper">
  <div class="box one">hsl(188 97% 28% / .3)</div>
  <div class="box two">hsl(321 47% 57% / .7)</div>
  <div class="box three">hsl(174 77% 31% / .9)</div>
</div>
```

```css live-sample___color-hsla
.wrapper {
  background-image: url(https://mdn.github.io/shared-assets/images/examples/balloons.jpg);
  padding: 40px 20px;
}

.box {
  padding: 10px;
  margin: 0.5em 0;
  border-radius: 0.5em;
}

.one {
  background-color: hsl(188 97% 28% / 0.3);
}

.two {
  background-color: hsl(321 47% 57% / 0.7);
}

.three {
  background-color: hsl(174 77% 31% / 0.9);
}
```

{{EmbedLiveSample("color-hsla", "", "250px")}}

## Immagini

Il tipo di valore [`<image>`](/it/docs/Web/CSS/image) viene utilizzato ovunque un'immagine sia un valore valido. Questo può essere un vero e proprio file immagine a cui si punta tramite una funzione `url()`, oppure un gradiente.

Nell'esempio seguente, abbiamo dimostrato un'immagine e un gradiente in uso come valore per la proprietà CSS `background-image`.

```html live-sample___image
<div class="box image"></div>
<div class="box gradient"></div>
```

```css live-sample___image
.box {
  height: 150px;
  width: 300px;
  margin: 20px auto;
  border-radius: 0.5em;
}
.image {
  background-image: url(https://mdn.github.io/shared-assets/images/examples/big-star.png);
}

.gradient {
  background-image: linear-gradient(
    90deg,
    rgb(119 0 255 / 39%),
    rgb(0 212 255 / 100%)
  );
}
```

{{EmbedLiveSample("image", "", "380px")}}

> [!NOTE]
> Ci sono altri valori possibili per `<image>`, tuttavia questi sono più recenti e attualmente hanno scarso supporto nei browser. Dai un'occhiata alla pagina su MDN per il tipo di dato [`<image>`](/it/docs/Web/CSS/image) se vuoi saperne di più.

## Posizione

Il tipo di valore [`<position>`](/it/docs/Web/CSS/position_value) rappresenta un insieme di coordinate 2D, utilizzato per posizionare un elemento come un'immagine di sfondo (tramite [`background-position`](/it/docs/Web/CSS/background-position)). Può assumere parole chiave come `top`, `left`, `bottom`, `right` e `center` per allineare gli elementi con limiti specifici di una casella 2D, insieme a lunghezze, che rappresentano offset dai bordi superiore e sinistro della casella.

Un tipico valore di posizione è costituito da due valori — il primo imposta la posizione orizzontalmente, il secondo verticalmente. Se specifichi valori solo per un asse, l'altro predefinirà a `center`.

Nell'esempio seguente abbiamo posizionato un'immagine di sfondo 40px dall'alto e a destra del contenitore usando una parola chiave.
Gioca con questi valori per vedere come puoi spostare l'immagine.

```html live-sample___position
<div class="box"></div>
```

```css live-sample___position
.box {
  height: 100px;
  width: 400px;
  background-image: url(https://mdn.github.io/shared-assets/images/examples/big-star.png);
  background-repeat: no-repeat;
  background-position: right 40px;
  margin: 20px auto;
  border-radius: 0.5em;
  border: 5px solid rebeccapurple;
}
```

{{EmbedLiveSample("position")}}

## Stringhe e identificatori

In tutti gli esempi sopra, abbiamo visto luoghi in cui le parole chiave vengono utilizzate come valore (ad esempio parole chiave `<color>` come `red`, `black`, `rebeccapurple` e `goldenrod`). Queste parole chiave sono più precisamente descritte come _identificatori_, un valore speciale che CSS comprende. In quanto tali non sono racchiuse tra virgolette — non sono trattate come stringhe.

Ci sono luoghi in cui si usano le stringhe in CSS. Ad esempio, [quando si specifica il contenuto generato](/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements#generating_content_with_before_and_after). In questo caso, il valore è quotato per dimostrare che è una stringa. Nell'esempio qui sotto, usiamo parole chiave di colore non quotate insieme a una stringa generata quotata.

```html live-sample___strings-idents
<div class="box"></div>
```

```css live-sample___strings-idents
.box {
  width: 400px;
  padding: 1em;
  border-radius: 0.5em;
  border: 5px solid rebeccapurple;
  background-color: lightblue;
}

.box::after {
  content: "This is a string. I know because it is quoted in the CSS.";
}
```

{{EmbedLiveSample("strings-idents")}}

## Funzioni

In programmazione, una funzione è un pezzo di codice che svolge un'attività specifica.
Le funzioni sono utili perché puoi scrivere codice una volta, quindi riutilizzarlo molte volte invece di scrivere la stessa logica ripetutamente.
La maggior parte dei linguaggi di programmazione non solo supporta le funzioni, ma fornisce anche funzioni integrate convenienti per attività comuni in modo da non doverle scrivere da zero.

CSS ha [funzioni](/it/docs/Web/CSS/CSS_Values_and_Units/CSS_Value_Functions), che funzionano in modo simile alle funzioni in altri linguaggi.
In realtà, abbiamo già visto le funzioni CSS nella sezione [Colore](#colore) sopra con le funzioni [`rgb()`](/it/docs/Web/CSS/color_value/rgb) e [`hsl()`](/it/docs/Web/CSS/color_value/hsl).

Oltre ad applicare colori, puoi usare le funzioni in CSS per fare molte altre cose.
Ad esempio, le [funzioni di trasformazione](/it/docs/Web/CSS/CSS_Values_and_Units/CSS_Value_Functions#transform_functions) sono un modo comune per spostare, ruotare e ridimensionare elementi su una pagina.
Potresti vedere [`translate()`](/it/docs/Web/CSS/transform-function/translate) per spostare qualcosa orizzontalmente o verticalmente, [`rotate()`](/it/docs/Web/CSS/transform-function/rotate) per ruotare qualcosa, o [`scale()`](/it/docs/Web/CSS/transform-function/scale) per rendere qualcosa più grande o più piccolo.

### Funzioni matematiche

Quando stai creando stili per un progetto, probabilmente inizierai con numeri come `300px` per le lunghezze o `200ms` per le durate.
Se vuoi che questi valori cambino in base ad altri valori, dovrai fare qualche calcolo.
Potresti calcolare la percentuale di un valore o aggiungere un numero a un altro numero, poi aggiornare il tuo CSS con il risultato.

CSS ha il supporto per le [funzioni matematiche](/it/docs/Web/CSS/CSS_Values_and_Units/CSS_Value_Functions#math_functions), che ci consentono di eseguire calcoli invece di fare affidamento su valori statici o fare il calcolo in JavaScript.
Una delle funzioni matematiche più comuni è [`calc()`](/it/docs/Web/CSS/calc), che ti consente di eseguire operazioni come addizione, sottrazione, moltiplicazione e divisione.

Ad esempio, supponiamo di voler impostare la larghezza di un elemento a essere il 20% del suo contenitore genitore più 100px.
Non possiamo specificare questa larghezza con un valore statico — se il genitore utilizza una larghezza in percentuale (o un'unità relativa come `em` o `rem`) allora varierà a seconda del contesto in cui è utilizzato, e altri fattori come il dispositivo dell'utente o la larghezza della finestra del browser.
Tuttavia, possiamo usare `calc()` per impostare la larghezza dell'elemento a essere il 20% del suo contenitore genitore più 100px.
Il 20% è basato sulla larghezza del contenitore genitore (`.wrapper`) e se quella larghezza cambia, la calcolazione cambierà anche:

```html live-sample___calc
<div class="wrapper">
  <div class="box">My width is calculated.</div>
</div>
```

```css live-sample___calc
.wrapper {
  width: 400px;
}
.box {
  padding: 1em;
  border-radius: 0.5em;
  border: 5px solid rebeccapurple;
  background-color: lightblue;
  width: calc(20% + 100px);
}
```

{{EmbedLiveSample("calc")}}

Ci sono molte altre funzioni matematiche che puoi usare in CSS, come [`min()`](/it/docs/Web/CSS/min), [`max()`](/it/docs/Web/CSS/max), e [`clamp()`](/it/docs/Web/CSS/clamp); rispettivamente queste ti permettono di scegliere il valore più piccolo, più grande, o intermedio da un set di valori.
Puoi anche usare [funzioni trigonometriche](/it/docs/Web/CSS/CSS_Values_and_Units/CSS_Value_Functions#trigonometric_functions) come [`sin()`](/it/docs/Web/CSS/sin), [`cos()`](/it/docs/Web/CSS/cos), e [`tan()`](/it/docs/Web/CSS/tan) per calcolare angoli per ruotare elementi attorno a un punto, o scegliere colori che prendono un [angolo di tonalità](/it/docs/Web/CSS/hue) come parametro.
Le [funzioni esponenziali](/it/docs/Web/CSS/CSS_Values_and_Units/CSS_Value_Functions#exponential_functions) potrebbero anche essere utilizzate per animazioni e transizioni, quando si richiede un controllo molto specifico su come qualcosa si muove e appare.

Conoscere le funzioni CSS è utile per riconoscerle quando le vedi. Dovresti iniziare a sperimentare con loro nei tuoi progetti — ti aiuteranno ad evitare di scrivere codice personalizzato o ripetitivo per ottenere risultati che puoi ottenere con CSS regolare.

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma puoi ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare di aver memorizzato queste informazioni prima di procedere — vedi [Metti alla prova le tue abilità: Valori e unità](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Values).

## Riepilogo

Questa è stata una rapida panoramica dei tipi di valori e unità più comuni che potresti incontrare. Puoi dare un'occhiata a tutti i diversi tipi alla pagina del modulo [Valori e unità CSS](/it/docs/Web/CSS/CSS_Values_and_Units) — incontrerai molti di questi in uso mentre lavori attraverso queste lezioni.

La cosa più importante da ricordare è che ogni proprietà ha un elenco definito di tipi di valore consentiti e ogni tipo di valore ha una definizione che spiega cosa sono i valori. Poi puoi verificare i dettagli qui su MDN. Per esempio, capire che [`<image>`](/it/docs/Web/CSS/image) ti permette anche di creare un gradiente di colore è utile, ma forse una conoscenza non ovvia da avere!

Nel prossimo articolo, daremo un'occhiata a come gli elementi vengono dimensionati in CSS.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Handling_conflicts", "Learn_web_development/Core/Styling_basics/Sizing", "Learn_web_development/Core/Styling_basics")}}
