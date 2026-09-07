---
title: Effetti di stile avanzati
slug: Learn_web_development/Core/Styling_basics/Advanced_styling_effects
l10n:
  sourceCommit: 1b7c3c1e03f14c3878e4d8518b0f1a89bedfdc9c
---

Questo articolo funge da raccolta di trucchi, fornendo un'introduzione ad alcune interessanti funzionalità avanzate di stile, come ombre delle caselle, modalità di fusione e filtri.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Introduzione a HTML</a
        >) e un'idea di come funziona CSS (studiare
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo stile CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Farsi un'idea di come utilizzare alcuni degli effetti di stile avanzati
        disponibili nei browser moderni.
      </td>
    </tr>
  </tbody>
</table>

## Ombre delle caselle

{{cssxref("box-shadow")}} consente di applicare una o più ombre esterne alla casella di un elemento. Come le ombre del testo, le ombre delle caselle sono supportate piuttosto bene nei browser, inclusi IE9+ ed Edge. Gli utenti di versioni precedenti di IE potrebbero semplicemente dover fare a meno delle ombre, quindi occorre testare i progetti per assicurarsi che il contenuto sia leggibile anche senza di esse.

### Una semplice ombra della casella

Vediamo un semplice esempio per iniziare. Prima, un po' di HTML:

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
  background-image: linear-gradient(to bottom, transparent, rgb(0 0 0 / 25%));
}

.simple {
  box-shadow: 5px 5px 5px rgb(0 0 0 / 70%);
}
```

Questo produce il seguente risultato:

{{EmbedLiveSample("A_simple_box_shadow", "", "100px")}}

Nel valore della proprietà `box-shadow` sono presenti quattro elementi:

1. Il primo valore di lunghezza è l'**offset orizzontale** — la distanza verso destra di cui l'ombra è spostata rispetto alla casella originale (o verso sinistra, se il valore è negativo).
2. Il secondo valore di lunghezza è l'**offset verticale** — la distanza verso il basso di cui l'ombra è spostata rispetto alla casella originale (o verso l'alto, se il valore è negativo).
3. Il terzo valore di lunghezza è il **raggio di sfocatura** — la quantità di sfocatura applicata all'ombra.
4. Il valore del colore è il **colore di base** dell'ombra.

Per definire questi valori è possibile usare qualsiasi unità di lunghezza e colore appropriata.

### Ombre multiple delle caselle

È anche possibile specificare più ombre delle caselle in una singola dichiarazione `box-shadow`, separandole con virgole:

```html hidden
<article class="multiple">
  <p>
    <strong>Warning</strong>: The thermostat on the cosmic transcender has
    reached a critical level.
  </p>
</article>
```

```css
p {
  margin: 0;
}

article {
  max-width: 500px;
  padding: 10px;
  background-color: red;
  background-image: linear-gradient(to bottom, transparent, rgb(0 0 0 / 25%));
}

.multiple {
  box-shadow:
    1px 1px 1px black,
    2px 2px 1px black,
    3px 3px 1px red,
    4px 4px 1px red,
    5px 5px 1px black,
    6px 6px 1px black;
}
```

Ora si ottiene questo risultato:

{{EmbedLiveSample("Multiple_box_shadows", "", "100px")}}

Qui è stato fatto qualcosa di divertente creando una casella rialzata con più livelli colorati, ma è possibile usarlo in qualsiasi modo, ad esempio per creare un aspetto più realistico con ombre basate su più sorgenti luminose.

### Altre funzionalità delle ombre delle caselle

A differenza di {{cssxref("text-shadow")}}, {{cssxref("box-shadow")}} dispone della parola chiave `inset`: inserendola all'inizio di una dichiarazione di ombra, questa diventa un'ombra interna anziché un'ombra esterna. Vediamo cosa significa.

Per prima cosa, in questo esempio verrà usato un HTML diverso:

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
  background-image: linear-gradient(to bottom right, #777777, #dddddd);
  box-shadow:
    1px 1px 1px black,
    inset 2px 3px 5px rgb(0 0 0 / 30%),
    inset -2px -3px 5px rgb(255 255 255 / 50%);
}

button:focus,
button:hover {
  background-image: linear-gradient(to bottom right, #888888, #eeeeee);
}

button:active {
  box-shadow:
    inset 2px 2px 1px black,
    inset 2px 3px 5px rgb(0 0 0 / 30%),
    inset -2px -3px 5px rgb(255 255 255 / 50%);
}
```

Questo produce il seguente risultato:

{{EmbedLiveSample("Other_box_shadow_features", "100%", "70px")}}

Qui sono stati impostati alcuni stili per pulsanti insieme agli stati focus/hover/active. Il pulsante ha per impostazione predefinita una semplice ombra della casella nera, oltre a un paio di ombre interne, una chiara e una scura, posizionate negli angoli opposti del pulsante per conferirgli un gradevole effetto di ombreggiatura.

Quando il pulsante viene premuto, lo stato active sostituisce la prima ombra della casella con un'ombra interna molto scura, dando l'impressione che il pulsante venga premuto verso l'interno.

> [!NOTE]
> È presente un altro elemento che può essere impostato nel valore `box-shadow`: un ulteriore valore di lunghezza può essere impostato facoltativamente appena prima del valore del colore; si tratta di un **raggio di espansione**. Se impostato, fa sì che l'ombra diventi più grande della casella originale. Non viene usato molto comunemente, ma vale la pena menzionarlo.

## Filtri

Anche se non è possibile modificare la composizione di un'immagine usando CSS, è possibile fare alcune cose creative. Una proprietà molto interessante, che può contribuire a rendere più interessanti i progetti, è la proprietà {{cssxref("filter")}}. Questa proprietà abilita filtri simili a Photoshop direttamente da CSS.

Nell'esempio seguente sono stati usati due valori diversi per filter. Il `first` è `blur()`: a questa funzione può essere passato un valore per indicare quanto l'immagine debba essere sfocata.

Il secondo è `grayscale()`; usando una percentuale viene impostata la quantità di colore che si desidera rimuovere.

Modificare i parametri percentuali e in pixel nell'esempio seguente per vedere come cambiano le immagini. È anche possibile sostituire i valori con altri. Provare `contrast(200%)`, `invert(100%)` o `hue-rotate(20deg)` nell'esempio live precedente. Consultare la pagina MDN di {{cssxref("filter")}} per molte altre opzioni da provare.

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

È possibile applicare filtri a qualsiasi elemento, non solo alle immagini. Alcune delle opzioni di filtro disponibili svolgono operazioni molto simili ad altre funzionalità CSS; ad esempio, `drop-shadow()` funziona in modo molto simile e produce un effetto simile a {{cssxref("box-shadow")}} o {{cssxref("text-shadow")}}. L'aspetto davvero interessante dei filtri, tuttavia, è che funzionano sulle forme esatte del contenuto all'interno della casella, non soltanto sulla casella stessa come un unico blocco; pertanto vale la pena conoscere la differenza.

Nel prossimo esempio viene applicato il filtro a una casella e confrontato con un'ombra della casella. Come si può vedere, il filtro drop-shadow segue la forma esatta del testo e dei trattini del bordo. L'ombra della casella segue soltanto il quadrato della casella.

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

È possibile trovare molti più esempi rispetto a quelli disponibili qui nella pagina di esempio [filters.html](https://mdn.github.io/learning-area/css/styling-boxes/advanced_box_effects/filters.html) (vedere il [codice sorgente](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/advanced_box_effects/filters.html)) e nella pagina di riferimento di {{cssxref("filter")}}.

## Modalità di fusione

Le modalità di fusione CSS consentono di aggiungere modalità di fusione agli elementi che specificano un effetto di fusione quando due elementi si sovrappongono: il colore finale mostrato per ciascun pixel sarà il risultato di una combinazione del colore del pixel originale e di quello del pixel nel livello sottostante. Le modalità di fusione sono ancora una volta molto familiari agli utenti di applicazioni grafiche come Photoshop.

In CSS sono presenti due proprietà che usano modalità di fusione:

- {{cssxref("background-blend-mode")}}, che fonde più immagini e colori di sfondo impostati su un singolo elemento.
- {{cssxref("mix-blend-mode")}}, che fonde l'elemento su cui è impostata con gli elementi a cui si sovrappone, sia lo sfondo sia il contenuto.

È possibile trovare molti più esempi rispetto a quelli disponibili qui nella pagina di esempio [blend-modes.html](https://mdn.github.io/learning-area/css/styling-boxes/advanced_box_effects/blend-modes.html) (vedere il [codice sorgente](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/advanced_box_effects/blend-modes.html)) e nella pagina di riferimento di {{cssxref("&lt;blend-mode&gt;")}}.

> [!NOTE]
> Anche le modalità di fusione sono molto recenti e hanno un supporto leggermente inferiore rispetto ai filtri. Non è ancora presente alcun supporto in Edge e Safari supporta solo alcune delle opzioni delle modalità di fusione.

### background-blend-mode

Ancora una volta, vediamo alcuni esempi per comprenderlo meglio. Prima, {{cssxref("background-blend-mode")}}: qui verranno mostrati un paio di semplici {{htmlelement("div")}}, in modo da poter confrontare l'originale con la versione fusa:

```html
<div></div>
<div class="multiply"></div>
```

Ora un po' di CSS: al `<div>` vengono aggiunti un'immagine di sfondo e un colore di sfondo verde:

```css
div {
  width: 250px;
  height: 130px;
  padding: 10px;
  margin: 10px;
  display: inline-block;
  background: url("https://mdn.github.io/shared-assets/images/examples/colorful-heart.png")
    no-repeat center 20px;
  background-color: green;
}

.multiply {
  background-blend-mode: multiply;
}
```

Il risultato ottenuto è questo: si può vedere l'originale a sinistra e la modalità di fusione multiply a destra:

{{EmbedLiveSample("background-blend-mode", "", "220px")}}

### mix-blend-mode

Ora vediamo {{cssxref("mix-blend-mode")}}. Qui verranno presentati gli stessi due `<div>`, ma ciascuno ora si trova sopra un semplice `<div>` con uno sfondo viola, per mostrare come gli elementi si fonderanno:

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

Ecco il CSS con cui verranno applicati gli stili:

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
  background: url("https://mdn.github.io/shared-assets/images/examples/colorful-heart.png")
    no-repeat center 20px;
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

Questo produce i seguenti risultati:

{{EmbedLiveSample("mix-blend-mode", "", "220px")}}

Qui si può vedere che la fusione mix multiply ha fuso non solo le due immagini di sfondo, ma anche il colore del `<div>` sottostante.

> [!NOTE]
> Non preoccuparsi se alcune delle proprietà di layout precedenti, come {{cssxref("position")}}, {{cssxref("top")}}, {{cssxref("bottom")}}, {{cssxref("z-index")}} e così via, non sono ancora chiare. Verranno trattate in dettaglio nel modulo [Layout CSS](/it/docs/Learn_web_development/Core/CSS_layout).

## Forme CSS

Sebbene sia vero che tutto in CSS è una casella rettangolare e che le immagini sono caselle rettangolari fisiche, è possibile far sembrare che il contenuto fluisca attorno a elementi non rettangolari usando le [forme CSS](/it/docs/Web/CSS/Guides/Shapes).

La specifica CSS Shapes consente di disporre il testo attorno a una forma non rettangolare. È particolarmente utile quando si lavora con un'immagine che contiene spazio bianco attorno al quale si potrebbe voler far fluire il testo.

Nell'immagine seguente è presente un palloncino piacevolmente rotondo. Il file effettivo è rettangolare, ma facendo fluttuare l'immagine (le forme si applicano solo agli elementi flottanti) e usando la proprietà {{cssxref("shape-outside")}} con un valore `circle(50%)`, è possibile ottenere l'effetto del testo che segue il contorno del palloncino.

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

La forma in questo esempio non reagisce al contenuto del file immagine. Al contrario, la funzione circle prende il punto centrale dal centro del file immagine, come se fosse stato posto un compasso al centro del file e disegnato un cerchio che rientra nel file. È attorno a quel cerchio che fluisce il testo.

> [!NOTE]
> In Firefox è possibile usare [Shapes Inspector](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/how_to/edit_css_shapes/index.html) dei DevTools per ispezionare le forme.

La funzione `circle()` è solo una delle poche forme di base definite; tuttavia, esistono diversi modi per creare forme. Per ulteriori informazioni e codice di esempio per le forme CSS, vedere le [guide alle forme CSS](/it/docs/Web/CSS/Guides/Shapes/Overview) su MDN.

## -webkit-background-clip: text

Un'altra funzionalità che vale la pena menzionare brevemente è il valore `text` per {{cssxref("background-clip")}}. Quando viene usato insieme alla funzionalità proprietaria `-webkit-text-fill-color: transparent;`, consente di ritagliare le immagini di sfondo secondo la forma del testo dell'elemento, creando effetti interessanti. Non è uno standard ufficiale, ma è stato implementato in più browser poiché è popolare e usato abbastanza ampiamente dagli sviluppatori. Quando vengono usate in questo contesto, entrambe le proprietà richiedono un prefisso del fornitore `-webkit-`, anche per browser non basati su WebKit/Chrome.
È possibile vederlo in azione nell'esempio live seguente:

```html live-sample___webkit-background-clip
<h2>WOW</h2>
<h2 class="text-clip">WOW</h2>
```

```css hidden live-sample___webkit-background-clip
body {
  font-family: "impact", sans-serif;
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
  background: url("https://mdn.github.io/shared-assets/images/examples/colorful-heart.png")
    no-repeat center;
}

.text-clip {
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
```

{{EmbedLiveSample("webkit-background-clip", "", "340px")}}

Perché allora altri browser hanno implementato un prefisso `-webkit-`? Principalmente per la compatibilità del browser: così tanti sviluppatori web hanno iniziato a implementare siti web con prefissi `-webkit-` che ha cominciato a sembrare che gli altri browser fossero difettosi, mentre in realtà stavano seguendo gli standard. Sono quindi stati costretti a implementare alcune di queste funzionalità. Questo evidenzia il pericolo di usare funzionalità CSS non standard e/o con prefisso nel proprio lavoro: non solo causano problemi di compatibilità del browser, ma sono anche soggette a cambiamenti, quindi il codice potrebbe smettere di funzionare in qualsiasi momento. È molto meglio attenersi agli standard.

Se si desidera usare tali funzionalità nel lavoro di produzione, assicurarsi di effettuare test approfonditi in tutti i browser e di verificare che, laddove queste funzionalità non funzionano, il sito sia comunque utilizzabile.

## Riepilogo

Si spera che questo articolo sia stato divertente: giocare con strumenti luccicanti in genere lo è, ed è sempre interessante vedere quali tipi di strumenti di stile avanzati stanno diventando disponibili nei browser moderni.
