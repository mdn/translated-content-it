---
title: Effetti di stile avanzati
slug: Learn_web_development/Core/Styling_basics/Advanced_styling_effects
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Questo articolo funge da raccoglitore di trucchi, fornendo un'introduzione ad alcune caratteristiche avanzate di stile interessanti come le ombre delle scatole, le modalità di fusione e i filtri.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Basi di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Introduzione a HTML</a
        >) e una conoscenza di base su come funziona CSS (studiare
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi di stile CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Ottenere un'idea di come utilizzare alcuni degli effetti di stile avanzati
        disponibili nei browser moderni.
      </td>
    </tr>
  </tbody>
</table>

## Ombre delle scatole

{{cssxref("box-shadow")}} ti permette di applicare una o più ombre a un elemento. Come le ombre sul testo, le ombre delle scatole sono supportate abbastanza bene su tutti i browser, inclusi IE9+ ed Edge. Gli utenti di versioni precedenti di IE potrebbero semplicemente dover fare a meno delle ombre, quindi è bene testare i propri progetti per assicurarsi che il contenuto sia leggibile anche senza di esse.

### Un'ombra semplice

Vediamo un esempio semplice per iniziare. Prima, un po' di HTML:

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

Vedrai che abbiamo quattro elementi nel valore della proprietà `box-shadow`:

1. Il primo valore di lunghezza è lo **scostamento orizzontale** — la distanza verso destra dello scostamento dell'ombra rispetto al riquadro originale (o a sinistra, se il valore è negativo).
2. Il secondo valore di lunghezza è lo **scostamento verticale** — la distanza verso il basso dello scostamento dell'ombra rispetto al riquadro originale (o verso l'alto, se il valore è negativo).
3. Il terzo valore di lunghezza è il **raggio di sfocatura** — la quantità di sfocatura applicata all'ombra.
4. Il valore di colore è il **colore base** dell'ombra.

Puoi utilizzare qualsiasi unità di lunghezza e colore che abbia senso per definire questi valori.

### Ombre multiple

Puoi anche specificare ombre multiple in una singola dichiarazione `box-shadow`, separandole con delle virgole:

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

Abbiamo fatto qualcosa di interessante qui creando una scatola sollevata con più strati colorati, ma puoi usarlo in qualsiasi modo desideri, ad esempio per creare un aspetto più realistico con ombre basate su fonti di luce multiple.

### Altre caratteristiche delle ombre delle scatole

A differenza di {{cssxref("text-shadow")}}, {{cssxref("box-shadow")}} ha una keyword `inset` disponibile — mettendo questa all'inizio di una dichiarazione di ombra, l'ombra diventa interna piuttosto che esterna. Vediamo di cosa si tratta.

Prima utilizziamo un HTML diverso per questo esempio:

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

Qui abbiamo impostato uno stile per i pulsanti insieme a stati di focus/hover/attivo. Il pulsante ha un'ombra semplice nera impostata di default, più un paio di ombre interne, una chiara e una scura, posizionate su angoli opposti del pulsante per dare un bel effetto di ombreggiatura.

Quando il pulsante viene premuto, lo stato attivo causa la sostituzione della prima ombra con un'ombra interna molto scura, dando l'apparenza che il pulsante sia stato premuto.

> [!NOTE]
> C'è un altro elemento che può essere impostato nel valore `box-shadow` — un altro valore di lunghezza può essere impostato facoltativamente appena prima del valore del colore, che è un **raggio di diffusione**. Se impostato, causa l'ingrandimento dell'ombra rispetto al riquadro originale. Non è molto comunemente utilizzato, ma vale la pena menzionarlo.

## Filtri

Sebbene non sia possibile cambiare la composizione di un'immagine usando CSS, ci sono alcune cose creative che puoi fare. Una proprietà molto interessante, che può aiutare a rendere più interessanti i tuoi progetti, è la proprietà {{cssxref("filter")}}. Questa proprietà abilita filtri simili a quelli di Photoshop direttamente da CSS.

Nell'esempio seguente abbiamo utilizzato due valori diversi per il filtro. Il `primo` è `blur()` — questa funzione può ricevere un valore per indicare quanto l'immagine debba essere sfocata.

Il secondo è `grayscale()`; utilizzando una percentuale impostiamo quanto colore vogliamo rimuovere.

Sperimenta con i parametri di percentuale e pixel nell'esempio seguente per vedere come le immagini cambiano. Puoi anche scambiare i valori con altri. Prova `contrast(200%)`, `invert(100%)` o `hue-rotate(20deg)` sull'esempio live sopra. Dai un'occhiata alla pagina di MDN per [`filter`](/it/docs/Web/CSS/filter) per molte altre opzioni che potresti provare.

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

Puoi applicare filtri a qualsiasi elemento, e non solo alle immagini. Alcune delle opzioni di filtro disponibili fanno cose molto simili ad altre funzionalità CSS, ad esempio `drop-shadow()` funziona in modo molto simile e produce un effetto simile a [`box-shadow`](/it/docs/Web/CSS/box-shadow) o [`text-shadow`](/it/docs/Web/CSS/text-shadow). Tuttavia, la cosa veramente interessante dei filtri è che lavorano sulle forme esatte del contenuto dentro il riquadro, non solo sul riquadro stesso come un unico grande blocco, quindi vale la pena conoscere la differenza.

In questo prossimo esempio stiamo applicando il nostro filtro a una scatola, e lo confrontiamo con un'ombra scatola. Come puoi vedere, il filtro drop-shadow segue la forma esatta del testo e dei tratti del bordo. L'ombra scatola segue solo il quadrato del riquadro.

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

Le modalità di fusione CSS ci permettono di aggiungere modalità di fusione agli elementi che specificano un effetto di fusione quando due elementi si sovrappongono — il colore finale mostrato per ogni pixel sarà il risultato di una combinazione del colore del pixel originale e quello del pixel nel livello sottostante. Le modalità di fusione sono di nuovo molto familiari agli utenti di applicazioni grafiche come Photoshop.

Ci sono due proprietà che utilizzano le modalità di fusione in CSS:

- {{cssxref("background-blend-mode")}}, che mescola insieme più immagini di sfondo e colori impostati su un singolo elemento.
- {{cssxref("mix-blend-mode")}}, che mescola insieme l'elemento su cui è impostato con gli elementi su cui si sovrappone — sia sfondo che contenuto.

Puoi trovare molti più esempi di quelli disponibili qui nella nostra pagina di esempio [blend-modes.html](https://mdn.github.io/learning-area/css/styling-boxes/advanced_box_effects/blend-modes.html) (vedi [codice sorgente](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/advanced_box_effects/blend-modes.html)), e sulla pagina di riferimento {{cssxref("&lt;blend-mode&gt;")}}.

> [!NOTE]
> Le modalità di fusione sono anche molto nuove, e leggermente meno supportate rispetto ai filtri. Non ci sono supporti attualmente in Edge, e Safari supporta solo alcune delle opzioni delle modalità di fusione.

### background-blend-mode

Ancora una volta, vediamo alcuni esempi per capire meglio questo concetto. Prima, {{cssxref("background-blend-mode")}} — qui mostreremo un paio di semplici {{htmlelement("div")}}, così da poter confrontare l'originale con la versione fusa:

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

Il risultato che otteniamo è questo — puoi vedere l'originale a sinistra e la modalità di fusione multiplies a destra:

{{EmbedLiveSample("background-blend-mode", "", "220px")}}

### mix-blend-mode

Ora vediamo {{cssxref("mix-blend-mode")}}. Qui presenteremo gli stessi due `<div>`, ma ciascuno è ora posizionato sopra un semplice `<div>` con uno sfondo viola, per mostrare come gli elementi si fondono insieme:

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

Qui puoi vedere che la modalità di fusione multiply ha fuso insieme non solo le due immagini di sfondo, ma anche il colore del `<div>` sotto di esso.

> [!NOTE]
> Non preoccuparti se non comprendi alcune delle proprietà di layout sopra, come {{cssxref("position")}}, {{cssxref("top")}}, {{cssxref("bottom")}}, {{cssxref("z-index")}}, ecc. Ne parleremo in dettaglio nel nostro modulo [CSS Layout](/it/docs/Learn_web_development/Core/CSS_layout).

## Forme CSS

Anche se è vero che tutto in CSS è una scatola rettangolare, e le immagini sono una scatola rettangolare fisica, possiamo far sembrare che il nostro contenuto si adatti intorno a oggetti non rettangolari usando le [Forme CSS](/it/docs/Web/CSS/CSS_shapes).

La specifica delle Forme CSS consente di avvolgere il testo intorno a una forma non rettangolare. È particolarmente utile quando si lavora con un'immagine che ha dello spazio bianco intorno di cui si potrebbe voler far fluttuare il testo.

Nell'immagine qui sotto abbiamo un palloncino rotondo esteticamente piacevole. Il file effettivo è rettangolare, ma flottando l'immagine (le forme si applicano solo agli elementi flottanti) e utilizzando la proprietà {{cssxref("shape-outside")}} con un valore di `circle(50%)`, possiamo dare l'effetto che il testo segua il perimetro del palloncino.

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

La forma in questo esempio non reagisce al contenuto del file dell'immagine. Invece, la funzione cerchio prende il suo punto centrale dal centro del file dell'immagine, come se avessimo messo un compasso al centro del file e disegnato un cerchio che si adatta al file. È quel cerchio che il testo segue.

> [!NOTE]
> In Firefox puoi utilizzare i DevTools [Shapes Inspector](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/edit_css_shapes/index.html) per ispezionare le Forme.

La funzione `circle()` è solo una delle poche forme di base che sono definite, tuttavia ci sono diversi modi per creare forme. Per ulteriori informazioni e codice di esempio per le Forme CSS, vedi le [Guide alle Forme CSS](/it/docs/Web/CSS/CSS_shapes/Overview_of_shapes) su MDN.

## -webkit-background-clip: text

Un'altra caratteristica che pensavamo di menzionare brevemente è il valore `text` per {{cssxref("background-clip")}}. Quando viene usato insieme alla funzione proprietaria `-webkit-text-fill-color: transparent;`, permette di ritagliare le immagini di sfondo alla forma del testo dell'elemento, creando effetti molto piacevoli. Questo non è uno standard ufficiale, ma è stato implementato in più browser, in quanto è popolare ed è usato ampiamente dagli sviluppatori. Quando viene usato in questo contesto, entrambe le proprietà richiederebbero un prefisso `-webkit-`, anche per i browser non basati su WebKit/Chrome.

Puoi vedere questo in azione nel seguente esempio dal vivo:

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

Quindi, perché altri browser hanno implementato un prefisso `-webkit-`? Principalmente per la compatibilità del browser — molti sviluppatori web hanno iniziato a implementare siti web con prefissi `-webkit-` tanto che è sembrato che gli altri browser fossero "rotti", mentre in realtà stavano seguendo gli standard. Sono stati quindi costretti a implementare alcune di queste funzionalità. Questo evidenzia il pericolo di utilizzare funzionalità CSS non standard e/o con prefissi nel tuo lavoro — non solo causano problemi di compatibilità tra browser, ma sono anche soggette a cambiamenti, quindi il tuo codice potrebbe rompersi in qualsiasi momento. È molto meglio attenersi agli standard.

Se vuoi utilizzare tali funzionalità nel lavoro di produzione, assicurati di testare a fondo su diversi browser e controlla che, dove queste funzionalità non funzionano, il sito sia comunque utilizzabile.

## Riassunto

Speriamo che questo articolo sia stato divertente — giocare con oggetti luccicanti lo è generalmente, ed è sempre interessante vedere quali tipi di strumenti di stile avanzati stanno diventando disponibili nei browser moderni.
