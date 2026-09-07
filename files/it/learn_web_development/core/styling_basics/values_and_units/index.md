---
title: Valori e unità CSS
short-title: Valori e unità
slug: Learn_web_development/Core/Styling_basics/Values_and_units
l10n:
  sourceCommit: 28f5f3b9b463fa842fa686ccc73c9e1d9b06282b
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Fixing_blog_styles", "Learn_web_development/Core/Styling_basics/Test_your_skills/Values", "Learn_web_development/Core/Styling_basics")}}

Le regole CSS contengono [dichiarazioni](/it/docs/Web/CSS/Guides/Syntax/Introduction#css_declarations), che a loro volta sono composte da proprietà e valori.
Ogni proprietà usata in CSS ha un **tipo di valore** che descrive quali tipi di valori può accettare.
In questa lezione verranno esaminati alcuni dei tipi di valore usati più frequentemente, cosa sono e come funzionano.

> [!NOTE]
> Ogni [pagina delle proprietà CSS](/it/docs/Web/CSS/Reference#index) ha una sezione sulla sintassi che elenca i tipi di valore utilizzabili con tale proprietà.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Getting_started">Sintassi CSS di base</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors">Selettori CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere che i valori delle proprietà possono assumere molti tipi diversi e cosa rappresentano questi tipi.</li>
          <li>Familiarità con l'uso dei tipi fondamentali: numeri, lunghezze, percentuali, colori, immagini, posizioni, stringhe e identificatori, e funzioni.</li>
          <li>Comprendere cosa sono le unità assolute e relative e la differenza tra esse.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è un valore CSS?

I valori CSS definiscono quali tipi di valore sono validi per ciascuna proprietà CSS. Per esempio, è possibile specificare colori per i valori di {{cssxref("color")}} o {{cssxref("border-color")}}, ma non lunghezze o percentuali.

Nelle specifiche CSS e nelle pagine delle proprietà qui su MDN, è possibile riconoscere i tipi di valore perché sono racchiusi tra parentesi angolari (`<`, `>`), come {{cssxref("&lt;color&gt;")}} o {{cssxref("length")}}. Quando il tipo di valore `<color>` è indicato come valido per una determinata proprietà, significa che è possibile usare qualsiasi colore valido come valore per quella proprietà, come elencato nella pagina di riferimento {{cssxref("&lt;color&gt;")}}.

A volte i tipi di valore e le proprietà possono avere nomi uguali o simili. Per esempio, esistono una proprietà {{cssxref("color")}} e un tipo di dati {{cssxref("&lt;color&gt;")}}. È possibile usare le parentesi angolari per determinare quale dei due si sta esaminando in ciascun caso. Anche gli elementi HTML usano parentesi angolari, ma dal contesto dovrebbe essere chiaro quale si sta osservando. In caso di dubbi, provare a cercarlo su MDN.

> [!NOTE]
> I tipi di valore CSS vengono talvolta indicati come _tipi di dati_. I termini sono sostanzialmente intercambiabili: quando qualcosa in CSS è indicato come tipo di dati, è semplicemente un modo più elaborato di dire tipo di valore. Il termine _valore_ si riferisce a qualsiasi particolare espressione supportata da un tipo di valore che si sceglie di utilizzare.

Nell'esempio seguente, il colore del testo dell'intestazione è stato impostato usando una parola chiave di colore e lo sfondo usando un diverso tipo di valore di colore, la funzione `rgb()`:

```css
h1 {
  color: black;
  background-color: rgb(197 93 161);
}
```

Un tipo di valore in CSS definisce una raccolta di valori consentiti. Ciò significa che, se `<color>` è indicato come valido, non è necessario chiedersi quale dei diversi tipi di valore di colore possa essere usato — parole chiave, valori esadecimali, funzioni `rgb()` e così via. È possibile usare _qualsiasi_ valore `<color>` disponibile, purché sia supportato dal browser. La pagina MDN di ciascun valore fornisce informazioni sul supporto dei browser. Per esempio, consultando la pagina relativa a {{cssxref("&lt;color&gt;")}}, la sezione sulla compatibilità del browser elenca diversi tipi di valori di colore e il relativo supporto.

Esaminiamo alcuni dei tipi di valori e delle unità che potrebbero essere incontrati frequentemente, con esempi per provare diversi valori possibili.

## Numeri, lunghezze e percentuali

Esistono vari tipi di valori numerici che possono essere usati in CSS. I seguenti sono tutti classificati come numerici:

<table class="standard-table no-markdown">
  <thead>
    <tr>
      <th scope="col">Tipo di dati</th>
      <th scope="col">Descrizione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <code><a href="/it/docs/Web/CSS/Reference/Values/integer">&#x3C;integer></a></code>
      </td>
      <td>
        Un <code>&#x3C;integer></code> è un numero intero, come
        <code>1024</code> o <code>-55</code>.
      </td>
    </tr>
    <tr>
      <td>
        <code><a href="/it/docs/Web/CSS/Reference/Values/number">&#x3C;number></a></code>
      </td>
      <td>
        Un <code>&#x3C;number></code> rappresenta un numero decimale: può avere
        o non avere un punto decimale con una componente frazionaria. Per
        esempio, <code>0.255</code>, <code>128</code> o <code>-1.2</code>.
      </td>
    </tr>
    <tr>
      <td>
        <code
          ><a href="/it/docs/Web/CSS/Reference/Values/dimension">&#x3C;dimension></a></code
        >
      </td>
      <td>
        Una <code>&#x3C;dimension></code> è un <code>&#x3C;number></code> con un'unità
        associata. Per esempio, <code>45deg</code>, <code>5s</code>
        o <code>10px</code>. <code>&#x3C;dimension></code> è una categoria
        generale che include i tipi {{cssxref("length")}}, <code><a href="/it/docs/Web/CSS/Reference/Values/angle">&#x3C;angle></a></code
        >, <code><a href="/it/docs/Web/CSS/Reference/Values/time">&#x3C;time></a></code
        > e
        <code
          ><a href="/it/docs/Web/CSS/Reference/Values/resolution">&#x3C;resolution></a></code
        >.
      </td>
    </tr>
    <tr>
      <td>{{cssxref("percentage")}}</td>
      <td>
        Una <code>&#x3C;percentage></code> rappresenta una frazione di un altro
        valore. Per esempio, <code>50%</code>. I valori percentuali sono sempre
        relativi a un'altra quantità. Per esempio, la lunghezza di un elemento è
        relativa alla lunghezza del suo elemento padre.
      </td>
    </tr>
  </tbody>
</table>

### Lunghezze

Il tipo numerico che verrà incontrato più frequentemente è {{cssxref("length")}}. Per esempio, `10px` (pixel) o `30em`. In CSS sono usati due tipi di lunghezze: relative e assolute. È importante conoscerne la differenza per comprendere quali dimensioni assumeranno gli elementi.

#### Unità di lunghezza assolute

Le seguenti sono tutte unità di lunghezza **assolute**: non sono relative a nient'altro e in genere sono considerate sempre della stessa dimensione.

| Unità | Nome                 | Equivalente a            |
| ----- | -------------------- | ------------------------ |
| `cm`  | Centimetri           | 1cm = 37.8px = 25.2/64in |
| `mm`  | Millimetri           | 1mm = 1/10 di 1cm        |
| `Q`   | Quarti di millimetro | 1Q = 1/40 di 1cm         |
| `in`  | Pollici              | 1in = 2.54cm = 96px      |
| `pc`  | Pica                 | 1pc = 1/6 di 1in         |
| `pt`  | Punti                | 1pt = 1/72 di 1in        |
| `px`  | Pixel                | 1px = 1/96 di 1in        |

La maggior parte di queste unità è più utile per la stampa piuttosto che per l'output su schermo. Per esempio, normalmente non si usa `cm` (centimetri) sullo schermo. L'unico valore usato comunemente è `px` (pixel).

Si noti che `1px` non corrisponde necessariamente a un pixel fisico del dispositivo. Sugli schermi HD può coprire più pixel fisici.
Allo stesso modo, `1cm` in CSS spesso non corrisponde a un centesimo di metro [SI](https://en.wikipedia.org/wiki/International_System_of_Units). Su uno schermo TV di grandi dimensioni, in genere è più lungo.
Le lunghezze sono percettive: `16px` appaiono approssimativamente uguali sullo schermo di un telefono, di un portatile o di un televisore alla normale distanza di visione.

#### Unità di lunghezza relative

Le unità di lunghezza relative sono relative a qualcos'altro. Per esempio:

- `em` è relativo alla dimensione del carattere di questo elemento, oppure alla dimensione del carattere dell'elemento padre quando usato per {{cssxref("font-size")}}. `rem` è relativo alla dimensione del carattere dell'elemento radice.
- `vh` e `vw` sono relativi rispettivamente all'altezza e alla larghezza della viewport.

Il vantaggio dell'uso delle unità relative è che, con un'attenta pianificazione, è possibile fare in modo che le dimensioni del testo o di altri elementi si adattino in relazione a tutto il resto della pagina. Per un elenco completo delle unità relative disponibili, consultare la pagina di riferimento del tipo {{cssxref("length")}}.

In questa sezione verranno esplorate alcune delle unità relative più comuni.

#### Esplorare un esempio

Nell'esempio seguente è possibile vedere il comportamento di alcune unità di lunghezza relative e assolute. Il primo riquadro ha una {{cssxref("width")}} impostata in pixel. Essendo un'unità assoluta, questa larghezza rimarrà uguale indipendentemente da qualsiasi altra modifica.

Il secondo riquadro ha una larghezza impostata in unità `vw` (larghezza della viewport). Questo valore è relativo alla larghezza della viewport, quindi `10vw` corrisponde al 10% della larghezza della viewport. Se si modifica la larghezza della finestra del browser, le dimensioni del riquadro dovrebbero cambiare. Tuttavia, questo esempio è incorporato nella pagina usando un [`<iframe>`](/it/docs/Web/HTML/Reference/Elements/iframe), quindi non funzionerà. Per vederlo in azione, è necessario [provare l'esempio dopo averlo aperto nella propria scheda del browser](https://mdn.github.io/css-examples/learn/values-units/length.html).

Il terzo riquadro usa unità `em`. Queste sono relative alla dimensione del carattere dell'elemento. È stata impostata una dimensione del carattere di `1em` sul {{htmlelement("div")}} contenitore, che ha una classe `.wrapper`. Modificando questo valore in `1.5em`, si vedrà aumentare la dimensione del carattere di tutti gli elementi, ma solo l'ultimo elemento diventerà più largo, poiché la sua larghezza è relativa a quella dimensione del carattere.

Dopo aver seguito le istruzioni precedenti, provare a modificare i valori in altri modi per vedere cosa si ottiene.

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

`em` e `rem` sono le due lunghezze relative che probabilmente verranno incontrate più spesso nel dimensionamento di elementi, dai riquadri al testo. Vale la pena comprendere come funzionano e le differenze tra esse, soprattutto quando si iniziano ad affrontare argomenti più complessi come la [formattazione del testo](/it/docs/Learn_web_development/Core/Text_styling) o il [layout CSS](/it/docs/Learn_web_development/Core/CSS_layout). L'esempio seguente ne fornisce una dimostrazione.

L'esempio successivo è un insieme di elenchi annidati: sono presenti due elenchi in totale e i due esempi hanno lo stesso HTML. L'unica differenza è che il primo ha una classe _ems_ e il secondo una classe _rems_.

Per iniziare, viene impostato `16px` come dimensione del carattere sull'elemento `<html>`.

In sintesi, l'unità `em` significa **"la dimensione del carattere del mio elemento padre"** quando è usata per `font-size`, e **"la mia dimensione del carattere"** quando è usata per qualsiasi altra cosa. Gli elementi {{htmlelement("li")}} all'interno dell'elemento {{htmlelement("ul")}} con `class` pari a `ems` assumono le dimensioni dal proprio elemento padre. Ogni livello successivo di annidamento diventa quindi progressivamente più grande, poiché ciascuno ha la dimensione del carattere impostata a `1.3em`, ovvero 1,3 volte la dimensione del carattere dell'elemento padre.

In sintesi, l'unità `rem` significa **"la dimensione del carattere dell'elemento radice"** (`rem` significa "root em"). Gli elementi {{htmlelement("li")}} all'interno dell'elemento {{htmlelement("ul")}} con `class` pari a `rems` assumono le dimensioni dall'elemento radice (`<html>`). Ciò significa che ogni livello successivo di annidamento non continua a diventare più grande.

Tuttavia, modificando `font-size` dell'elemento `<html>` nel CSS, tutto il resto cambierà in relazione a esso, sia il testo dimensionato con `rem` sia quello dimensionato con `em`. Provare ora in MDN Playground.

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

### Percentuali

In molti casi, una percentuale viene trattata allo stesso modo di una lunghezza. La particolarità delle percentuali è che sono sempre impostate in relazione a un altro valore. Per esempio, se si imposta `font-size` di un elemento come percentuale, sarà una percentuale di `font-size` dell'elemento padre. Se si usa una percentuale per un valore `width`, sarà una percentuale della `width` del padre.

Nell'esempio successivo, le due coppie di riquadri dimensionati in percentuale e in pixel hanno gli stessi nomi di classe. I riquadri all'interno di ciascuna coppia sono larghi rispettivamente `40%` e `200px`.

La differenza è che il secondo insieme di due riquadri si trova all'interno di un contenitore largo `400px`. Il secondo riquadro largo `200px` ha la stessa larghezza del primo, ma il secondo riquadro al `40%` ora è il `40%` di `400px`, quindi è molto più stretto del primo.

Provare a modificare la larghezza del contenitore o il valore percentuale per vedere come funziona:

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

L'esempio successivo presenta dimensioni dei caratteri impostate in percentuale. Ogni `<li>` ha un `font-size` pari a `80%`; pertanto, gli elementi degli elenchi annidati diventano progressivamente più piccoli poiché ereditano le dimensioni dal proprio elemento padre.

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

Sebbene molte proprietà accettino una lunghezza o una percentuale come valore, alcune accettano solo una lunghezza, per esempio {{cssxref("border-width")}}. Le pagine di riferimento delle proprietà di MDN indicano in dettaglio quali tipi di valore accettano. Se il valore consentito include {{cssxref("length-percentage")}}, allora è possibile usare una lunghezza o una percentuale. Se il valore consentito include solo `<length>`, non è possibile usare una percentuale.

### Numeri

Alcuni tipi di valore accettano numeri senza unità; un esempio è la proprietà `opacity`, che controlla l'opacità di un elemento, ovvero quanto è trasparente. Questa proprietà accetta un numero compreso tra `0` (completamente trasparente) e `1` (completamente opaco).

Nell'esempio seguente, provare a modificare il valore di `opacity` con vari valori decimali tra `0` e `1` e osservare come il riquadro e il suo contenuto diventino più o meno opachi:

```html live-sample___opacity
<div class="wrapper">
  <div class="box">I am a box with opacity</div>
</div>
```

```css live-sample___opacity
.wrapper {
  background-image: url("https://mdn.github.io/shared-assets/images/examples/balloons.jpg");
  background-repeat: no-repeat;
  background-position: bottom left;
  padding: 20px;
}

.box {
  margin: 40px auto;
  width: 200px;
  background-color: lightblue;
  border: 5px solid darkblue;
  padding: 30px;
  opacity: 0.6;
}
```

{{EmbedLiveSample("opacity", "", "210px")}}

> [!NOTE]
> Quando si usa un numero come valore in CSS, non deve essere racchiuso tra virgolette.

## Colore

I valori di colore possono essere usati in molti punti in CSS, per specificare il colore del testo, degli sfondi, dei bordi e molto altro.
Esistono molti modi per impostare il colore in CSS, consentendo di controllare numerose proprietà interessanti.

Il sistema di colori standard disponibile nei computer moderni supporta colori a 24 bit, permettendo di visualizzare circa 16,7 milioni di colori distinti tramite una combinazione di diversi canali rosso, verde e blu, con 256 valori diversi per canale (256 x 256 x 256 = 16.777.216).

In questa sezione verranno innanzitutto esaminati i modi più comuni per specificare i colori: parole chiave, valori esadecimali e valori `rgb()`.
Verranno inoltre esaminate brevemente ulteriori funzioni di colore, così da poterle riconoscere quando vengono incontrate o sperimentare diversi modi di applicare il colore.

Probabilmente verrà scelta una tavolozza di colori e poi verranno usati tali colori — e il metodo preferito per specificarli — in tutto il progetto.
È possibile combinare diversi modelli di colore, ma per coerenza in genere è preferibile che l'intero progetto usi lo stesso metodo per dichiarare i colori.

### Parole chiave di colore

Le parole chiave di colore, o "colori denominati", vengono usate in molti esempi di codice su MDN. Poiché il tipo di dati {{cssxref("named-color")}} contiene un numero molto limitato di valori di colore, non sono comunemente usate nei siti web di produzione con un linguaggio di design sofisticato. D'altro canto, i colori denominati sono usati negli esempi di codice per indicare chiaramente quale colore è previsto, in modo che chi apprende possa concentrarsi sul contenuto insegnato.

Nell'esempio successivo, provare a usare diverse parole chiave di colore per capire meglio come funzionano. È possibile cercarle usando la pagina di riferimento {{cssxref("named-color")}}.

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

### Valori RGB esadecimali

Il tipo successivo di valore di colore che probabilmente verrà incontrato sono i codici esadecimali, o hex.

I numeri esadecimali usano 16 caratteri da `0-9` e `a-f`, quindi l'intero intervallo è `0123456789abcdef`. Ogni valore di colore esadecimale è costituito da un simbolo cancelletto (`#`) seguito da sei caratteri esadecimali (per esempio `#ffc0cb`). Ogni **coppia** di caratteri esadecimali rappresenta uno dei canali di un colore RGB — rosso, verde e blu — e consente di specificare uno qualsiasi dei 256 valori disponibili per ciascuno (16 x 16 = 256).

Questi valori sono meno intuitivi delle parole chiave per definire i colori, ma sono molto più versatili perché con essi è possibile _rappresentare_ qualsiasi colore RGB.

Nell'esempio successivo, provare a modificare i valori per vedere come variano i colori:

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

> [!NOTE]
> I valori di colore esadecimali possono essere scritti con tre caratteri anziché sei. Questa è una forma abbreviata utilizzabile quando i caratteri di ciascuna coppia sono uguali. Per esempio, `#ff00ff` e `#f0f` sono equivalenti. I valori di colore esadecimali possono anche essere scritti usando otto caratteri, oppure quattro, dove il quarto valore rappresenta la trasparenza alfa dei tre valori precedenti, per esempio `#ff00ff66`.

### Valori RGB

Per creare direttamente valori RGB, la funzione {{cssxref("color_value/rgb")}} accetta tre parametri che rappresentano i valori dei canali **rosso**, **verde** e **blu** dei colori, con un quarto valore facoltativo separato da una barra (`/`) che rappresenta l'opacità, in modo molto simile ai valori esadecimali. La differenza rispetto a RGB è che ciascun canale è rappresentato non da due cifre esadecimali, ma da un numero decimale compreso tra `0` e `255` o da una percentuale compresa tra `0%` e `100%` (ma non da una combinazione dei due).

Riscriviamo l'ultimo esempio per usare colori RGB:

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

#### Un esempio RGB con opacità

Nell'esempio successivo, è stata aggiunta un'immagine di sfondo al blocco contenitore dei riquadri colorati. Ai riquadri sono quindi stati assegnati diversi valori di opacità: si noti come lo sfondo risulti più visibile quando il valore del canale alfa è minore. Impostando questo valore a `0`, il colore diventa completamente trasparente, mentre `1` lo rende completamente opaco. I valori intermedi forniscono diversi livelli di trasparenza.

Provare a modificare i valori del canale alfa per vedere come influiscono sulla resa del colore.

```html live-sample___color-rgba
<div class="wrapper">
  <div class="box one">rgb(2 121 139 / .3)</div>
  <div class="box two">rgb(197 93 161 / .7)</div>
  <div class="box three">rgb(18 138 125 / .9)</div>
</div>
```

```css live-sample___color-rgba
.wrapper {
  background-image: url("https://mdn.github.io/shared-assets/images/examples/balloons.jpg");
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

> [!NOTE]
> L'impostazione di un canale alfa su un colore presenta una differenza fondamentale rispetto all'uso della proprietà {{cssxref("opacity")}} menzionata in precedenza. Usando `opacity`, si rendono trasparenti l'elemento e tutto ciò che contiene, mentre usando RGB con un parametro alfa si rende trasparente solo il colore specificato.

### Usare le tonalità per specificare un colore

Per andare oltre parole chiave, valori esadecimali e {{cssxref("color_value/rgb")}} per i colori, può essere utile provare a usare {{cssxref("hue")}}.
La tonalità è il tipo di valore che consente di distinguere o stabilire la somiglianza tra colori come rosso, arancione, giallo, verde, blu e così via.
Il concetto fondamentale è che è possibile specificare una tonalità in un {{cssxref("angle")}}, perché la maggior parte dei modelli di colore descrive le tonalità usando una {{Glossary("color_wheel", "ruota dei colori")}}.

Esistono diverse funzioni di colore che includono una componente {{cssxref("hue")}}, tra cui {{cssxref("color_value/hsl")}}, {{cssxref("color_value/hwb")}} e {{cssxref("color_value/lch")}}. Altre funzioni di colore, come {{cssxref("color_value/lab")}}, definiscono i colori in base a ciò che gli esseri umani possono vedere.

Per ulteriori informazioni su queste funzioni e sugli spazi colore, consultare la guida [Applicare il colore agli elementi HTML usando CSS](/it/docs/Web/CSS/Guides/Colors/Applying_color), il riferimento {{cssxref("&lt;color&gt;")}} che elenca tutti i diversi modi di usare i colori in CSS e il [modulo colori CSS](/it/docs/Web/CSS/Guides/Colors), che fornisce una panoramica di tutti i tipi di colore in CSS e delle proprietà che usano valori di colore.

### HWB

Un ottimo punto di partenza per usare le tonalità in CSS è la funzione {{cssxref("color_value/hwb")}}, che specifica un colore `srgb()`.
Le tre parti sono:

- **Tonalità**: La sfumatura di base del colore. Accetta un valore {{cssxref("hue")}} compreso tra `0` e `360`, che rappresenta gli angoli attorno a una ruota dei colori.
- **Bianchezza**: Quanto è bianco il colore? Accetta un valore da `0%` (nessuna bianchezza) a `100%` (bianchezza completa).
- **Nero**: Quanto è nero il colore? Accetta un valore da `0%` (nessun nero) a `100%` (nero completo).

### HSL

Simile alla funzione {{cssxref("color_value/hwb")}} è la funzione {{cssxref("color_value/hsl")}}, che specifica anch'essa un colore `srgb()`.
HSL usa `Hue`, oltre a `Saturation` e `Lightness`:

- **Tonalità**: Anche in questo caso, rappresenta la sfumatura di base del colore.
- **Saturazione**: Quanto è saturo il colore? Accetta un valore da `0` a `100%`, dove `0` significa assenza di colore, che apparirà come una sfumatura di grigio, e `100%` significa saturazione cromatica completa.
- **Luminosità**: Quanto è chiaro o brillante il colore? Accetta un valore da `0` a `100%`, dove `0` significa assenza di luce, e apparirà completamente nero, mentre `100%` significa luce completa, e apparirà completamente bianco.

Il valore di colore {{cssxref("color_value/hsl")}} dispone anche di un quarto valore facoltativo, separato dal colore con una barra (`/`), che rappresenta la trasparenza alfa.

Aggiorniamo l'esempio RGB per usare invece colori HSL:

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

Proprio come con `rgb()`, è possibile passare un parametro alfa a `hsl()` per specificare l'opacità:

```html live-sample___color-hsla
<div class="wrapper">
  <div class="box one">hsl(188 97% 28% / .3)</div>
  <div class="box two">hsl(321 47% 57% / .7)</div>
  <div class="box three">hsl(174 77% 31% / .9)</div>
</div>
```

```css live-sample___color-hsla
.wrapper {
  background-image: url("https://mdn.github.io/shared-assets/images/examples/balloons.jpg");
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

Prima di proseguire, provare a modificare i due esempi precedenti per usare alcuni valori di colore basati sulla tonalità. Provare a variare in ciascun caso il valore della tonalità per vedere come influisce sul colore di base, quindi variare anche gli altri parametri.

## Immagini

Il tipo di valore {{cssxref("image")}} viene usato ovunque un'immagine sia un valore valido. Può trattarsi di un file immagine effettivo indicato tramite una funzione `url()` oppure di un gradiente.

Nell'esempio seguente vengono usati un'immagine e un gradiente come valori per la proprietà CSS `background-image`.

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
  background-image: url("https://mdn.github.io/shared-assets/images/examples/big-star.png");
}

.gradient {
  background-image: linear-gradient(
    90deg,
    rgb(119 0 255 / 39%),
    rgb(0 212 255 / 25%)
  );
}
```

{{EmbedLiveSample("image", "", "380px")}}

> [!NOTE]
> Esistono altri possibili valori per `<image>`, tuttavia sono più recenti e attualmente hanno un supporto dei browser limitato. Consultare la pagina MDN relativa al tipo di dati {{cssxref("image")}} per ulteriori informazioni.

I valori delle immagini verranno approfonditi nell'articolo [Sfondi e bordi](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders), più avanti.

## Posizione

Il tipo di valore {{cssxref("&lt;position&gt;")}} rappresenta un insieme di coordinate 2D, usato per posizionare un elemento come un'immagine di sfondo, tramite {{cssxref("background-position")}}. Può accettare parole chiave come `top`, `left`, `bottom`, `right` e `center` per allineare gli elementi a limiti specifici di un riquadro 2D, e lunghezze, che rappresentano scostamenti dai bordi superiore e sinistro del riquadro.

Un valore di posizione tipico è costituito da due valori: il primo imposta la posizione orizzontale, il secondo quella verticale. Se si specificano valori per un solo asse, l'altro avrà come valore predefinito `center`.

Nell'esempio seguente, un'immagine di sfondo è stata posizionata a `60px` dall'alto e a `right` del contenitore usando una parola chiave.

Provare a modificare questi valori per vedere come è possibile spostare l'immagine.

```html live-sample___position
<div class="box"></div>
```

```css live-sample___position
.box {
  height: 200px;
  width: 400px;
  background-image: url("https://mdn.github.io/shared-assets/images/examples/big-star.png");
  background-repeat: no-repeat;
  background-position: right 60px;
  margin: 20px auto;
  border-radius: 0.5em;
  border: 5px solid rebeccapurple;
}
```

{{EmbedLiveSample("position", "100%", "260px")}}

## Stringhe e identificatori

Negli esempi precedenti sono stati osservati casi in cui le parole chiave vengono usate come valore, per esempio parole chiave `<color>` come `red`, `black`, `rebeccapurple` e `goldenrod`. Queste parole chiave sono più precisamente descritte come _identificatori_, un valore speciale che CSS comprende. Pertanto non sono racchiuse tra virgolette: non vengono trattate come stringhe.

Esistono situazioni in cui si usano stringhe in CSS. Per esempio, [quando si specifica contenuto generato](/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements#generating_content_with_before_and_after). In questo caso, il valore è tra virgolette per indicare che è una stringa. Nell'esempio seguente vengono usate parole chiave di colore senza virgolette insieme a una stringa tra virgolette per il contenuto generato.

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

{{EmbedLiveSample("strings-idents", "100%", "80")}}

## Funzioni

Nella programmazione, una funzione è una porzione di codice che svolge un'attività specifica.
Le funzioni sono utili perché consentono di scrivere il codice una volta e riutilizzarlo molte volte, invece di scrivere ripetutamente la stessa logica.
La maggior parte dei linguaggi di programmazione non solo supporta le funzioni, ma include anche pratiche funzioni integrate per attività comuni, così non è necessario scriverle da zero.

CSS dispone anch'esso di [funzioni](/it/docs/Web/CSS/Reference/Values/Functions), che funzionano in modo simile alle funzioni di altri linguaggi.
Infatti, nella sezione [Colore](#colore) precedente sono già state viste funzioni CSS come {{cssxref("color_value/rgb")}} e {{cssxref("color_value/hsl")}}.

Oltre ad applicare colori, le funzioni in CSS possono essere usate per molte altre operazioni.
Per esempio, le [funzioni di trasformazione](/it/docs/Web/CSS/Reference/Values/Functions#transform_functions) sono un modo comune per spostare, ruotare e ridimensionare gli elementi di una pagina.
Si potrebbero incontrare {{cssxref("transform-function/translate")}} per spostare qualcosa orizzontalmente o verticalmente, {{cssxref("transform-function/rotate")}} per ruotare qualcosa, o {{cssxref("transform-function/scale")}} per rendere qualcosa più grande o più piccolo.

### Funzioni matematiche

Durante la creazione degli stili per un progetto, probabilmente si inizierà con numeri come `300px` per le lunghezze o `200ms` per le durate.
Se si desidera che questi valori cambino in base ad altri valori, sarà necessario eseguire alcuni calcoli.
Si potrebbe calcolare la percentuale di un valore o aggiungere un numero a un altro, quindi aggiornare il CSS con il risultato.

CSS supporta le [funzioni matematiche](/it/docs/Web/CSS/Reference/Values/Functions#math_functions), che consentono di eseguire calcoli in CSS anziché fare affidamento su valori statici o eseguire i calcoli in JavaScript.
Una delle funzioni matematiche più comuni è {{cssxref("calc()")}}, che consente di effettuare operazioni come addizione, sottrazione, moltiplicazione e divisione.

Per esempio, supponiamo di voler impostare la larghezza di un elemento affinché sia il `20%` del suo contenitore padre più `100px`.
Non è possibile specificare questa larghezza con un valore statico: se l'elemento padre usa una larghezza percentuale, o un'unità relativa come `em` o `rem`, varierà a seconda del contesto in cui viene usato e di altri fattori, come il dispositivo dell'utente o la larghezza della finestra del browser.
Tuttavia, è possibile usare `calc()` per impostare la larghezza dell'elemento al `20%` del contenitore padre più `100px`.
Il `20%` si basa sulla larghezza del contenitore padre (`.wrapper`) e, se tale larghezza cambia, cambierà anche il calcolo:

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

Esistono molte altre funzioni matematiche utilizzabili in CSS, come {{cssxref("min()")}}, {{cssxref("max()")}} e {{cssxref("clamp()")}}; rispettivamente, consentono di scegliere il valore più piccolo, più grande o intermedio da un insieme di valori. Esplorare la pagina di riferimento sulle [funzioni dei valori CSS](/it/docs/Web/CSS/Reference/Values/Functions) per consultare tutte le funzioni CSS disponibili.

Conoscere le funzioni CSS è utile per poterle riconoscere quando vengono incontrate. È opportuno iniziare a sperimentarle nei propri progetti: aiutano a evitare di scrivere codice personalizzato o ripetitivo per ottenere risultati raggiungibili con CSS standard.

## Riepilogo

Questa è stata una rapida panoramica dei tipi di valori e delle unità più comuni che potrebbero essere incontrati. È possibile consultare tutti i diversi tipi nella pagina del modulo [Valori e unità CSS](/it/docs/Web/CSS/Guides/Values_and_units): molti di essi verranno incontrati durante lo svolgimento di queste lezioni.

L'aspetto fondamentale da ricordare è che ogni proprietà ha un elenco definito di tipi di valore consentiti e ogni tipo di valore ha una definizione che spiega quali sono i valori. È quindi possibile cercare i dettagli qui su MDN. Per esempio, comprendere che {{cssxref("image")}} consente anche di creare un gradiente di colore è una conoscenza utile, ma forse non immediatamente evidente.

Nel prossimo articolo verranno proposti alcuni test che possono essere usati per verificare quanto bene sono state comprese e memorizzate le informazioni fornite sui valori e sulle unità.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Fixing_blog_styles", "Learn_web_development/Core/Styling_basics/Test_your_skills/Values", "Learn_web_development/Core/Styling_basics")}}
