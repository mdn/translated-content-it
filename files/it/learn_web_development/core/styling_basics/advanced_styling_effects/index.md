---
title: Effetti di stile avanzati
slug: Learn_web_development/Core/Styling_basics/Advanced_styling_effects
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Questo articolo funge da scatola di trucchi, fornendo un'introduzione a alcune interessanti funzionalità di stile avanzate come le ombre del riquadro, le modalità di fusione e i filtri.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Concetti base di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Introduzione a HTML</a
        >) e un'idea di come funziona CSS (studiare
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Concetti base di CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire un'idea su come utilizzare alcuni degli effetti di stile avanzati disponibili nei browser moderni.
      </td>
    </tr>
  </tbody>
</table>

## Ombre del riquadro

{{cssxref("box-shadow")}} consente di applicare una o più ombre al riquadro di un elemento. Come le ombre del testo, le ombre del riquadro sono ben supportate dai browser, inclusi IE9+ ed Edge. Gli utenti di versioni più vecchie di IE potrebbero dover fare a meno delle ombre, quindi testate i vostri design per assicurarvi che il contenuto sia leggibile anche senza di esse.

### Un'ombra di riquadro semplice

Esaminiamo un esempio semplice per iniziare. Innanzitutto, un po' di HTML:

```html
<article class="simple">
  <p>
    <strong>Warning</strong>: The thermostat on the cosmic transcender has
    reached a critical level.
  </p>
</article>
```

Ora il CSS:

```css
p {
  margin: 0;
}

article {
  max-width: 500px;
  padding: 10px;
  background-color: red;
  background-image: linear-gradient(
    to bottom,
    rgb(0 0 0 / 0%),
    rgb(0 0 0 / 25%)
  );
}

.simple {
  box-shadow: 5px 5px 5px rgb(0 0 0 / 70%);
}
```

Questo ci dà il seguente risultato:

{{EmbedLiveSample("A_simple_box_shadow", "", "100px")}}

Osservate che abbiamo quattro elementi nel valore della proprietà `box-shadow`:

1. Il primo valore di lunghezza è l'**offset orizzontale** — la distanza a destra a cui l'ombra è spostata rispetto al box originale (o a sinistra, se il valore è negativo).
2. Il secondo valore di lunghezza è l'**offset verticale** — la distanza verso il basso a cui l'ombra è spostata rispetto al box originale (o verso l'alto, se il valore è negativo).
3. Il terzo valore di lunghezza è il **raggio di sfocatura** — la quantità di sfocatura applicata all'ombra.
4. Il valore del colore è il **colore di base** dell'ombra.

È possibile utilizzare qualsiasi unità di lunghezza e colore che possa avere senso per definire questi valori.

### Ombre multiple del riquadro

È possibile specificare anche più ombre del riquadro in una singola dichiarazione `box-shadow`, separandole con virgole:

```html hidden
<article class="multiple">
  <p>
    <strong>Warning</strong>: The thermostat on the cosmic transcender has
    reached a critical level.
  </p>
</article>
```

```css-nolint
p {
  margin: 0;
}

article {
  max-width: 500px;
  padding: 10px;
  background-color: red;
  background-image: linear-gradient(
    to bottom,
    rgb(0 0 0 / 0%),
    rgb(0 0 0 / 25%)
  );
}

.multiple {
  box-shadow: 1px 1px 1px black,
              2px 2px 1px black,
              3px 3px 1px red,
              4px 4px 1px red,
              5px 5px 1px black,
              6px 6px 1px black;
}
```

Ora otteniamo questo risultato:

{{EmbedLiveSample("Multiple_box_shadows", "", "100px")}}

Abbiamo fatto qualcosa di divertente qui creando un riquadro sollevato con strati colorati multipli, ma potrebbe essere utilizzato in qualsiasi modo si desideri, per esempio per creare un aspetto più realistico con ombre basate su più fonti di luce.

### Altre caratteristiche delle ombre del riquadro

A differenza di {{cssxref("text-shadow")}}, {{cssxref("box-shadow")}} ha una parola chiave `inset` disponibile — posizionandola all'inizio di una dichiarazione di ombra si trasforma in un'ombra interna, piuttosto che in un'ombra esterna. Vediamo cosa intendiamo.

Per prima cosa, utilizzeremo un diverso HTML per questo esempio:

```html
<button>Press me!</button>
```

```css
button {
  width: 150px;
  font-size: 1.1rem;
  line-height: 2;
  border-radius: 10px;
  border: none;
  background-image: linear-gradient(to bottom right, #777, #ddd);
  box-shadow:
    1px 1px 1px black,
    inset 2px 3px 5px rgb(0 0 0 / 30%),
    inset -2px -3px 5px rgb(255 255 255 / 50%);
}

button:focus,
button:hover {
  background-image: linear-gradient(to bottom right, #888, #eee);
}

button:active {
  box-shadow:
    inset 2px 2px 1px black,
    inset 2px 3px 5px rgb(0 0 0 / 30%),
    inset -2px -3px 5px rgb(255 255 255 / 50%);
}
```

Questo ci dà il seguente risultato:

{{EmbedLiveSample("Other_box_shadow_features", "100%", "70px")}}

Qui abbiamo impostato alcuni stili per il bottone insieme agli stati di focus/hover/active. Il bottone ha un semplice box shadow nero impostato di default, più un paio di ombre interne, una chiara e una scura, posizionate su angoli opposti del bottone per dare un bel effetto di ombreggiatura.

Quando il bottone viene premuto, lo stato attivo fa sì che la prima ombra del riquadro venga scambiata per un'ombra interna molto scura, dando l'impressione che il bottone venga premuto.

> [!NOTE]
> C'è un altro parametro che può essere impostato nel valore `box-shadow` — un altro valore di lunghezza può essere opzionalmente impostato appena prima del valore del colore, il quale è un **raggio di espansione**. Se impostato, questo fa sì che l'ombra diventi più grande del riquadro originale. Non è molto comunemente usato, ma vale la pena menzionarlo.

## Filtri

Anche se non si può cambiare la composizione di un'immagine usando CSS, ci sono alcune cose creative che si possono fare. Una proprietà molto interessante, che può aiutare a rendere i vostri design più interessanti, è la proprietà {{cssxref("filter")}}. Questa proprietà abilita filtri simili a quelli di Photoshop direttamente da CSS.

Nell'esempio sottostante abbiamo utilizzato due valori diversi per `filter`. Il `primo` è `blur()` — questa funzione può ricevere un valore per indicare quanto l'immagine debba essere sfocata.

Il secondo è `grayscale()`; usando una percentuale stiamo impostando quanto colore vogliamo che venga rimosso.

Giocate con i parametri percentuali e di pixel nell'esempio sottostante per vedere come cambiano le immagini. Potreste anche sostituire i valori con altri. Provate `contrast(200%)`, `invert(100%)` o `hue-rotate(20deg)` nell'esempio live sopra. Date un'occhiata alla pagina MDN per [`filter`](/it/docs/Web/CSS/filter) per molte altre opzioni che potete provare.

```html live-sample___filter
<div class="wrapper">
  <div class="box">
    <img
      alt="balloons"
      class="blur"
      src="https://mdn.github.io/shared-assets/images/examples/balloons.jpg" />
  </div>
  <div class="box">
    <img
      alt="balloons"
      class="grayscale"
      src="https://mdn.github.io/shared-assets/images/examples/balloons.jpg" />
  </div>
</div>
```

```css hidden live-sample___filter
.wrapper {
  display: flex;
  align-items: flex-start;
}

.wrapper > * {
  margin: 20px;
  flex: 1;
}

.box {
  border: 5px solid darkblue;
}
```

```css live-sample___filter
img {
  height: 100%;
  width: 100%;
  display: block;
  object-fit: cover;
}

.blur {
  filter: blur(10px);
}

.grayscale {
  filter: grayscale(60%);
}
```

{{EmbedLiveSample("filter", "", "260px")}}

È possibile applicare filtri a qualsiasi elemento e non solo alle immagini. Alcune delle opzioni del filtro disponibili fanno cose molto simili ad altre funzionalità CSS, per esempio `drop-shadow()` funziona in un modo molto simile e dà un effetto simile a [`box-shadow`](/it/docs/Web/CSS/box-shadow) o [`text-shadow`](/it/docs/Web/CSS/text-shadow). La cosa veramente piacevole dei filtri, però, è che funzionano sulle forme esatte del contenuto all'interno del riquadro, non solo sul riquadro stesso come un grande blocco unico, quindi vale la pena conoscere la differenza.

In questo prossimo esempio stiamo applicando il nostro filtro a un riquadro, e confrontandolo con un'ombra del riquadro. Come potete vedere, il filtro drop-shadow segue la forma esatta del testo e dei trattini di confine. L'ombra del riquadro segue semplicemente il quadrato del riquadro.

```html live-sample___filter-text
<p class="filter">Filter</p>
<p class="box-shadow">Box shadow</p>
```

```css live-sample___filter-text
body {
  font-family: sans-serif;
}
p {
  margin: 1em 2em;
  padding: 20px;
  width: 100px;
  display: inline-block;
  border: 5px dashed red;
}

.filter {
  filter: drop-shadow(5px 5px 1px rgb(0 0 0 / 70%));
}

.box-shadow {
  box-shadow: 5px 5px 1px rgb(0 0 0 / 70%);
}
```

{{EmbedLiveSample("filter-text")}}

## Modalità di fusione

Le modalità di fusione CSS ci permettono di aggiungere modalità di fusione agli elementi che specificano un effetto di fusione quando due elementi si sovrappongono — il colore finale mostrato per ogni pixel sarà il risultato di una combinazione del colore originale del pixel e di quello del pixel nel livello sottostante. Le modalità di fusione sono di nuovo molto familiari agli utenti di applicazioni grafiche come Photoshop.

Ci sono due proprietà che utilizzano le modalità di fusione in CSS:

- {{cssxref("background-blend-mode")}}, che fonde insieme più immagini di sfondo e colori impostati su un singolo elemento.
- {{cssxref("mix-blend-mode")}}, che fonde insieme l'elemento su cui è impostata con gli elementi con cui si sovrappone — sia il fondo che il contenuto.

È possibile trovare molti più esempi di quelli disponibili qui nella nostra pagina di esempio [blend-modes.html](https://mdn.github.io/learning-area/css/styling-boxes/advanced_box_effects/blend-modes.html) (vedere [codice sorgente](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/advanced_box_effects/blend-modes.html)), e sulla pagina di riferimento {{cssxref("&lt;blend-mode&gt;")}}.

> [!NOTE]
> Le modalità di fusione sono anche molto nuove e leggermente meno ben supportate dei filtri. Non c'è ancora supporto in Edge, e Safari supporta solo alcune delle opzioni di modalità di fusione.

### background-blend-mode

Di nuovo, vediamo alcuni esempi per capire meglio. Per prima cosa, {{cssxref("background-blend-mode")}} — qui mostreremo un paio di semplici {{htmlelement("div")}}, così potete confrontare l'originale con la versione fusa:

```html
<div></div>
<div class="multiply"></div>
```

Ora un po' di CSS — stiamo aggiungendo al `<div>` un'immagine di sfondo e un colore di sfondo verde:

```css
div {
  width: 250px;
  height: 130px;
  padding: 10px;
  margin: 10px;
  display: inline-block;
  background: url(colorful-heart.png) no-repeat center 20px;
  background-color: green;
}

.multiply {
  background-blend-mode: multiply;
}
```

Il risultato che otteniamo è questo — potete vedere l'originale a sinistra e la modalità di fusione mupltly a destra:

{{EmbedLiveSample("background-blend-mode", "", "220px")}}

### mix-blend-mode

Ora vediamo {{cssxref("mix-blend-mode")}}. Qui presenteremo gli stessi due `<div>`, ma ciascuno ora è posizionato sopra un semplice `<div>` con uno sfondo viola, per mostrare come gli elementi si fonderanno insieme:

```html
<article>
  No mix blend mode
  <div></div>
  <div></div>
</article>

<article>
  Multiply mix
  <div class="multiply-mix"></div>
  <div></div>
</article>
```

Ecco il CSS con cui stileremo questo:

```css
article {
  width: 280px;
  height: 180px;
  margin: 10px;
  position: relative;
  display: inline-block;
}

div {
  width: 250px;
  height: 130px;
  padding: 10px;
  margin: 10px;
}

article div:first-child {
  position: absolute;
  top: 10px;
  left: 0;
  background: url(colorful-heart.png) no-repeat center 20px;
  background-color: green;
}

article div:last-child {
  background-color: purple;
  position: absolute;
  bottom: -10px;
  right: 0;
  z-index: -1;
}

.multiply-mix {
  mix-blend-mode: multiply;
}
```

Questo ci dà i seguenti risultati:

{{EmbedLiveSample("mix-blend-mode", "", "220px")}}

Come potete vedere qui, la modalità di fusione mix multipla ha fuso insieme non solo le due immagini di sfondo, ma anche il colore del `<div>` sottostante.

> [!NOTE]
> Non preoccupatevi se non comprendete alcune delle proprietà del layout sopra, come {{cssxref("position")}}, {{cssxref("top")}}, {{cssxref("bottom")}}, {{cssxref("z-index")}}, ecc. Copriremo questi dettagliatamente nel nostro modulo [CSS Layout](/it/docs/Learn_web_development/Core/CSS_layout).

## Forme CSS

Sebbene sia vero che tutto ciò che è in CSS è un box rettangolare, e le immagini sono un box rettangolare fisico, possiamo far sembrare che il nostro contenuto fluisca intorno a cose non rettangolari usando [Forme CSS](/it/docs/Web/CSS/CSS_shapes).

La specifica delle Forme CSS consente di avvolgere il testo attorno a una forma non rettangolare. È particolarmente utile quando si lavora con un'immagine che ha degli spazi vuoti intorno ai quali si potrebbe voler far scorrere il testo.

Nell'immagine sottostante abbiamo un palloncino piacevolmente rotondo. Il file reale è rettangolare, ma facendo galleggiare l'immagine (le forme si applicano solo agli elementi flottanti) e usando la proprietà {{cssxref("shape-outside")}} con un valore di `circle(50%)`, possiamo dare l'effetto che il testo segua il contorno del palloncino.

```html live-sample___shapes
<div class="wrapper">
  <img
    alt="balloon"
    src="https://mdn.github.io/shared-assets/images/examples/round-balloon.png" />
  <p>
    One November night in the year 1782, so the story runs, two brothers sat
    over their winter fire in the little French town of Annonay, watching the
    grey smoke-wreaths from the hearth curl up the wide chimney. Their names
    were Stephen and Joseph Montgolfier, they were papermakers by trade, and
    were noted as possessing thoughtful minds and a deep interest in all
    scientific knowledge and new discovery. Before that night—a memorable night,
    as it was to prove—hundreds of millions of people had watched the rising
    smoke-wreaths of their fires without drawing any special inspiration from
    the fact.
  </p>
</div>
```

```css live-sample___shapes
body {
  font-family: sans-serif;
}
img {
  float: left;
  shape-outside: circle(50%);
}
```

{{EmbedLiveSample("shapes", "", "200px")}}

La forma in questo esempio non reagisce al contenuto del file immagine. Invece, la funzione circle prende il suo punto centrale dal centro del file immagine, come se avessimo messo un compasso nel mezzo del file e tracciato un cerchio che si adatta all'interno del file. È quel cerchio che il testo segue.

> [!NOTE]
> In Firefox è possibile utilizzare il DevTools [Shapes Inspector](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/edit_css_shapes/index.html) per ispezionare le Forme.

La funzione `circle()` è solo una delle poche forme di base definite, tuttavia ci sono diversi modi per creare forme. Per ulteriori informazioni e codice di esempio sulle Forme CSS, consultare le [Guide alle Forme CSS](/it/docs/Web/CSS/CSS_shapes/Overview_of_shapes) su MDN.

## -webkit-background-clip: text

Un'altra funzione che pensavamo di menzionare brevemente è il valore `text` per {{cssxref("background-clip")}}. Se utilizzato insieme alla proprietà proprietaria `-webkit-text-fill-color: transparent;`, questo consente di tagliare le immagini di sfondo alla forma del testo dell'elemento, creando alcuni effetti carini. Questo non è uno standard ufficiale, ma è stato implementato su più browser, in quanto è popolare e abbastanza utilizzato dagli sviluppatori. Quando utilizzato in questo contesto, entrambe le proprietà richiederebbero un prefisso `-webkit-`, anche per i browser non basati su WebKit/Chrome.
È possibile vedere questa funzione in azione nel campione live sottostante:

```html live-sample___webkit-background-clip
<h2>WOW</h2>
<h2 class="text-clip">WOW</h2>
```

```css hidden live-sample___webkit-background-clip
body {
  font-family: impact, sans-serif;
}

h2 {
  width: 250px;
  height: 250px;
  text-align: center;
  line-height: 250px;
  font-size: 50px;
}
```

```css live-sample___webkit-background-clip
h2 {
  color: white;
  display: inline-block;
  background: url(colorful-heart.png) no-repeat center;
}

.text-clip {
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
```

{{EmbedLiveSample("webkit-background-clip", "", "340px")}}

Quindi perché altri browser hanno implementato un prefisso `-webkit-`? Principalmente per la compatibilità con i browser — così tanti sviluppatori web hanno iniziato a implementare siti web con prefissi `-webkit-` che ha iniziato a sembrare che gli altri browser fossero guasti, mentre in realtà seguivano gli standard. Sono quindi stati costretti a implementare alcune di queste funzionalità. Questo evidenzia il pericolo di utilizzare funzionalità CSS non standard e/o con prefisso nel vostro lavoro — non solo causano problemi di compatibilità con i browser, ma sono anche soggette a modifiche, quindi il vostro codice potrebbe smettere di funzionare in qualsiasi momento. È molto meglio attenersi agli standard.

Se desidera utilizzare tali funzionalità nel suo lavoro di produzione, assicuratevi di testare a fondo su tutti i browser e di controllare che, dove queste funzionalità non funzionano, il sito sia comunque utilizzabile.

## Riassunto

Speriamo che questo articolo sia stato divertente — giocare con giocattoli luccicanti lo è generalmente, ed è sempre interessante vedere che tipo di strumenti avanzati di stile stanno diventando disponibili nei browser moderni.
