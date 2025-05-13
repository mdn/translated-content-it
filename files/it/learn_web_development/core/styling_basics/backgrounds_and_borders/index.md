---
title: Sfondi e bordi
slug: Learn_web_development/Core/Styling_basics/Backgrounds_and_borders
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Sizing", "Learn_web_development/Core/Styling_basics/Overflow", "Learn_web_development/Core/Styling_basics")}}

In questa lezione, esamineremo alcune delle cose creative che si possono fare con CSS sfondi e bordi. Dall'aggiunta di gradienti, immagini di sfondo e angoli arrotondati, gli sfondi e i bordi sono la risposta a molte domande di stile in CSS.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units">Valori e unità CSS</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics/Sizing">Dimensionamento CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Stilizzazione di base dello sfondo — colori e immagini.</li>
          <li>Dimensione dell'immagine di sfondo, ripetizione, posizione e allegato.</li>
          <li>Gradienti di sfondo — concetto generale e gradienti lineari (i gradienti radiali, conici e ripetuti sono più avanzati; una conoscenza approfondita non è richiesta a questo stadio).</li>
          <li>Considerazioni sull'accessibilità degli sfondi — assicurarsi di avere un buon contrasto.</li>
          <li>Nozioni di base sui bordi — larghezza, stile, colore e scorciatoia del bordo. Raggio del bordo per angoli arrotondati.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Stilizzazione degli sfondi in CSS

La proprietà CSS {{cssxref("background")}} è una scorciatoia per una serie di proprietà di sfondo dettagliate che incontreremo in questa lezione. Se si scopre una complessa proprietà di sfondo in un foglio di stile, potrebbe sembrare un po' difficile da comprendere poiché molti valori possono essere passati insieme:

```css
.box {
  background:
    linear-gradient(
        105deg,
        rgb(255 255 255 / 20%) 39%,
        rgb(51 56 57 / 100%) 96%
      )
      center center / 400px 200px no-repeat,
    url(image.png) center no-repeat,
    rebeccapurple;
}
```

Torneremo a come funziona la scorciatoia più avanti nel tutorial, ma prima diamo un'occhiata alle diverse cose che si possono fare con gli sfondi in CSS, esaminando le singole proprietà di sfondo.

## Colori di sfondo

La proprietà {{cssxref("background-color")}} definisce il colore di sfondo su qualsiasi elemento in CSS. La proprietà accetta qualsiasi valore valido [`<color>`](/it/docs/Web/CSS/color_value). Un `background-color` si estende sotto il contenuto e il box di padding dell'elemento.

Nell'esempio qui sotto, abbiamo utilizzato vari valori di colore per aggiungere un colore di sfondo al box, un'intestazione e un elemento {{htmlelement("span")}}.
Provi a farlo anche lei, utilizzando qualsiasi valore disponibile [`<color>`](/it/docs/Web/CSS/color_value).

```html live-sample___color
<div class="box">
  <h2>Background Colors</h2>
  <p>Try changing the background <span>colors</span>.</p>
</div>
```

```css live-sample___color
.box {
  padding: 0.3em;
  background-color: #567895;
}

h2 {
  background-color: black;
  color: white;
}
span {
  background-color: rgb(255 255 255 / 50%);
}
```

{{EmbedLiveSample("color")}}

## Immagini di sfondo

La proprietà {{cssxref("background-image")}} permette la visualizzazione di un'immagine sullo sfondo di un elemento. Nell'esempio qui sotto, abbiamo due box — uno ha un'immagine di sfondo più grande del box ([balloons.jpg](https://mdn.github.io/shared-assets/images/examples/balloons.jpg)). L'altro ha una piccola immagine di una singola stella ([star.png](https://mdn.github.io/shared-assets/images/examples/star.png)).

Questo esempio dimostra due aspetti delle immagini di sfondo. Per impostazione predefinita, l'immagine grande non viene ridotta per adattarsi al box, quindi vediamo solo un piccolo angolo di essa, mentre la piccola immagine viene tileata per riempire il box.

```html live-sample___background-image
<div class="wrapper">
  <div class="box a"></div>
  <div class="box b"></div>
</div>
```

```css live-sample___background-image
.wrapper {
  display: flex;
}

.box {
  width: 200px;
  height: 80px;
  padding: 0.5em;
  border: 1px solid #ccc;
  margin: 20px;
}

.a {
  background-image: url(https://mdn.github.io/shared-assets/images/examples/balloons.jpg);
}

.b {
  background-image: url(https://mdn.github.io/shared-assets/images/examples/star.png);
}
```

{{EmbedLiveSample("background-image")}}

Se specifica un colore di sfondo oltre a un'immagine di sfondo, l'immagine viene visualizzata sopra il colore.
Provi ad aggiungere una proprietà `background-color` all'esempio sopra per vederlo in azione.

### Controllare la ripetizione dello sfondo

La proprietà {{cssxref("background-repeat")}} viene utilizzata per controllare il comportamento di tiling delle immagini. I valori disponibili sono:

- `no-repeat` — interrompe la ripetizione dello sfondo del tutto.
- `repeat-x` — ripete orizzontalmente.
- `repeat-y` — ripete verticalmente.
- `repeat` — valore predefinito; ripete in entrambe le direzioni.
- `space` — ripete quante volte possibile, aggiungendo spazio tra le immagini se è disponibile spazio extra.
- `round` — simile a `space`, ma estende le immagini per riempire qualsiasi spazio extra

Provi questi valori nell'esempio qui sotto. Abbiamo impostato il valore su `no-repeat` quindi vedrà solo una stella. Provi i diversi valori — `repeat-x` e `repeat-y` — per vedere quali sono i loro effetti.

```html live-sample___repeat
<div class="box"></div>
```

```css hidden live-sample___repeat
.box {
  width: 200px;
  height: 80px;
  padding: 0.5em;
  border: 1px solid #ccc;
  margin: 20px;
}
```

```css live-sample___repeat
.box {
  background-image: url(https://mdn.github.io/shared-assets/images/examples/star.png);
  background-repeat: no-repeat;
}
```

{{EmbedLiveSample("repeat")}}

### Ridimensionare l'immagine di sfondo

L'immagine _balloons.jpg_ utilizzata nell'esempio iniziale dell'immagine di sfondo è un'immagine grande che è stata ritagliata poiché è più grande dell'elemento di cui è sfondo. In questo caso, potremmo usare la proprietà {{cssxref("background-size")}}, che può accettare valori di {{cssxref("length")}} o {{cssxref("percentage")}}, per dimensionare l'immagine in modo che si adatti all'interno dello sfondo.

Può anche usare parole chiave:

- `cover` — il browser renderà l'immagine abbastanza grande da coprire completamente l'area del box pur mantenendo il suo {{Glossary("aspect_ratio", "aspect ratio")}}. In questo caso, è probabile che una parte dell'immagine finisca al di fuori del box.
- `contain` — il browser adatterà l'immagine alla giusta dimensione per adattarsi all'interno del box. In questo caso, potrebbe finire con spazi vuoti ai lati o in alto e in basso dell'immagine, se il rapporto d'aspetto dell'immagine è diverso da quello del box.

Nell'esempio qui sotto, l'immagine _balloons.jpg_ ha unità di lunghezza impostate per adattarla all'interno del box. Si può vedere che ciò ha distorto l'immagine.

Provi i seguenti:

- Cambiare le unità di lunghezza usate per modificare la dimensione dello sfondo.
- Rimuovere le unità di lunghezza e vedere cosa succede quando usa `background-size: cover` o `background-size: contain`.
- Se la sua immagine è più piccola del box, può cambiare il valore di `background-repeat` per ripetere l'immagine.

```html live-sample___size
<div class="box"></div>
```

```css hidden live-sample___size
.box {
  width: 500px;
  height: 100px;
  padding: 0.5em;
  border: 1px solid #ccc;
  margin: 10px;
}
```

```css live-sample___size
.box {
  background-image: url(https://mdn.github.io/shared-assets/images/examples/balloons.jpg);
  background-repeat: no-repeat;
  background-size: 80px 10em;
}
```

{{EmbedLiveSample("size")}}

### Posizionare l'immagine di sfondo

La proprietà {{cssxref("background-position")}} consente di scegliere la posizione in cui l'immagine di sfondo appare nel box a cui è applicata. Questo utilizza un sistema di coordinate in cui l'angolo in alto a sinistra del box è `(0,0)`, e il box è posizionato lungo l'asse orizzontale (`x`) e verticale (`y`).

> [!NOTE]
> Il valore predefinito di `background-position` è `(0,0)`.

I valori più comuni di `background-position` prendono due valori individuali — un valore orizzontale seguito da un valore verticale.

Può usare parole chiave come `top` e `right` (controlli le altre sulla pagina {{cssxref("background-position")}}):

```css
.box {
  background-image: url(image.png);
  background-repeat: no-repeat;
  background-position: top center;
}
```

E {{cssxref("length", "lunghezze")}}, e {{cssxref("percentage", "percentuali")}}:

```css
.box {
  background-image: url(image.png);
  background-repeat: no-repeat;
  background-position: 20px 10%;
}
```

Può anche mescolare valori di parole chiave con lunghezze o percentuali, nel qual caso il primo valore deve riferirsi alla posizione o offset orizzontale e il secondo verticale. Ad esempio:

```css
.box {
  background-image: url(image.png);
  background-repeat: no-repeat;
  background-position: 20px top;
}
```

Infine, può anche usare una sintassi a 4 valori per indicare una distanza da certe estremità del box — l'unità di lunghezza, in questo caso, è un offset dal valore che la precede. Quindi nel CSS sottostante stiamo posizionando lo sfondo a 20px dall'alto e 10px dalla destra:

```css
.box {
  background-image: url(image.png);
  background-repeat: no-repeat;
  background-position: top 20px right 10px;
}
```

Usi l'esempio qui sotto per giocare con questi valori e spostare la stella all'interno del box:

```html live-sample___position
<div class="box"></div>
```

```css hidden live-sample___position
.box {
  width: 500px;
  height: 80px;
  padding: 0.5em;
  border: 1px solid #ccc;
  margin: 20px;
}
```

```css live-sample___position
.box {
  background-image: url(https://mdn.github.io/shared-assets/images/examples/star.png);
  background-repeat: no-repeat;
  background-position: 120px 1em;
}
```

{{EmbedLiveSample("position")}}

> [!NOTE]
> La scorciatoia `background-position` è usata al posto di {{cssxref("background-position-x")}} e {{cssxref("background-position-y")}}, che le permettono di impostare i valori di posizione degli assi individualmente.

## Sfondi a gradiente

Un gradiente, quando usato per uno sfondo, si comporta proprio come un'immagine ed è impostato usando anche la proprietà {{cssxref("background-image")}}.

Può leggere di più sui diversi tipi di gradienti e cose che può fare con essi sulla pagina MDN per il tipo di dati [`<gradient>`](/it/docs/Web/CSS/gradient). Un modo divertente per giocare con i gradienti è usare uno dei molti generatori di gradienti CSS disponibili sul web, come [CSSGradient.io](https://cssgradient.io/). Può creare un gradiente e copiare e incollare il codice sorgente che lo genera.

Provi alcuni gradienti diversi nell'esempio qui sotto. Nei due box rispettivamente, abbiamo un gradiente lineare che si estende sull'intero box, e un gradiente radiale con una dimensione impostata, che quindi si ripete.

```html live-sample___gradients
<div class="wrapper">
  <div class="box a"></div>
  <div class="box b"></div>
</div>
```

```css live-sample___gradients
.wrapper {
  display: flex;
}

.box {
  width: 400px;
  height: 80px;
  padding: 0.5em;
  border: 1px solid #ccc;
  margin: 20px;
}

.a {
  background-image: linear-gradient(
    105deg,
    rgb(0 249 255 / 100%) 39%,
    rgb(51 56 57 / 100%) 96%
  );
}

.b {
  background-image: radial-gradient(
    circle,
    rgb(0 249 255 / 100%) 39%,
    rgb(51 56 57 / 100%) 96%
  );
  background-size: 100px 50px;
}
```

{{EmbedLiveSample("gradients")}}

## Immagini di sfondo multiple

È anche possibile avere immagini di sfondo multiple — specifica più valori `background-image` in un singolo valore di proprietà, separando ciascuno con una virgola.

Quando fa questo, potrebbe finire con immagini di sfondo che si sovrappongono l'una con l'altra. Gli sfondi si stratificano con l'ultima immagine di sfondo elencata in fondo alla pila, e ogni immagine precedente che si stratifica sopra quella che la segue nel codice.

> [!NOTE]
> I gradienti possono essere felicemente mescolati con immagini di sfondo regolari.

Le altre proprietà `background-*` possono anche avere valori separati da virgole nello stesso modo di `background-image`:

```css
background-image:
  url(image1.png), url(image2.png), url(image3.png), url(image4.png);
background-repeat: no-repeat, repeat-x, repeat;
background-position:
  10px 20px,
  top right;
```

Ogni valore delle diverse proprietà corrisponderà ai valori nella stessa posizione nelle altre proprietà. Sopra, ad esempio, il valore di `background-repeat` di `image1` sarà `no-repeat`. Tuttavia, cosa succede quando le proprietà diverse hanno numeri diversi di valori? La risposta è che i numeri più piccoli di valori cicleranno — nell'esempio sopra ci sono quattro immagini di sfondo ma solo due valori `background-position`. I primi due valori di posizione verranno applicati alle prime due immagini, poi ricominceranno da capo — `image3` riceverà il primo valore di posizione, e `image4` riceverà il secondo valore di posizione.

Giochiamo. L'esempio qui sotto include due immagini di sfondo. Per dimostrare l'ordine di stratificazione, provi a scambiare quale immagine di sfondo viene prima nella lista. Oppure giochi con le altre proprietà per cambiare la posizione, la dimensione o i valori di ripetizione.

```html live-sample___multiple-background-image
<div class="wrapper">
  <div class="box"></div>
</div>
```

```css live-sample___multiple-background-image
.wrapper {
  display: flex;
}

.box {
  width: 500px;
  height: 80px;
  padding: 0.5em;
  border: 1px solid #ccc;
  margin: 20px;
}

.box {
  background-image:
    url(https://mdn.github.io/shared-assets/images/examples/star.png),
    url(https://mdn.github.io/shared-assets/images/examples/big-star.png);
}
```

{{EmbedLiveSample("multiple-background-image")}}

## Allegato dello sfondo

Un'altra opzione che abbiamo disponibile per gli sfondi è specificare come scorrono quando il contenuto scorre. Questo è controllato usando la proprietà {{cssxref("background-attachment")}}, che può prendere i seguenti valori:

- `scroll`: fa scorrere lo sfondo dell'elemento quando la pagina viene scorsa. Se il contenuto dell'elemento viene scorso, lo sfondo non si muove. In effetti, lo sfondo è fissato alla stessa posizione sulla pagina, quindi scorre mentre la pagina scorre.
- `fixed`: fa in modo che lo sfondo di un elemento sia fisso nel viewport in modo che non scorra quando la pagina o il contenuto dell'elemento vengono scorsi. Rimarrà sempre nella stessa posizione sullo schermo.
- `local`: fissa lo sfondo all'elemento in cui è impostato, quindi quando scorre l'elemento, lo sfondo scorre con esso.

La proprietà {{cssxref("background-attachment")}} ha effetto solo quando c'è contenuto da scorrere, quindi abbiamo creato una demo per dimostrare le differenze tra i tre valori — dia un'occhiata a [background-attachment.html](https://mdn.github.io/learning-area/css/styling-boxes/backgrounds/background-attachment.html) (vedi anche [il codice sorgente](https://github.com/mdn/learning-area/tree/main/css/styling-boxes/backgrounds) qui).

## Usare la proprietà scorciatoia dello sfondo

Come menzionato all'inizio di questa lezione, vedrà spesso sfondi specificati usando la proprietà {{cssxref("background")}}. Questa scorciatoia le permette di impostare tutte le diverse proprietà insieme.

Se si utilizzano più sfondi, è necessario specificare tutte le proprietà per il primo sfondo, quindi aggiungere il successivo sfondo dopo una virgola. Nell'esempio sottostante abbiamo un gradiente con una dimensione e una posizione, poi un'immagine di sfondo con `no-repeat` e una posizione, infine un colore.

Ci sono alcune regole che devono essere seguite quando si scrivono valori abbreviati dell'immagine di sfondo, ad esempio:

- Un `background-color` può essere specificato solo dopo l'ultima virgola.
- Il valore di `background-size` può essere incluso solo immediatamente dopo `background-position`, separato con il carattere '/' come questo: `center/80%`.

Dia un'occhiata alla pagina MDN per {{cssxref("background")}} per vedere tutte le considerazioni.

```html live-sample___background
<div class="box"></div>
```

```css live-sample___background
.box {
  width: 500px;
  height: 300px;
  padding: 0.5em;
  background:
    linear-gradient(
        105deg,
        rgb(255 255 255 / 20%) 39%,
        rgb(51 56 57 / 100%) 96%
      )
      center center / 400px 200px no-repeat,
    url(https://mdn.github.io/shared-assets/images/examples/big-star.png) center
      no-repeat,
    rebeccapurple;
}
```

{{EmbedLiveSample("background", "", "320px")}}

## Considerazioni sull'accessibilità con gli sfondi

Quando posiziona il testo sopra un'immagine di sfondo o un colore, dovrebbe fare attenzione che ci sia abbastanza [contrasto](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Perceivable/Color_contrast) perché il testo sia leggibile dai suoi visitatori. Se specifica un'immagine, e se il testo sarà posizionato sopra quell'immagine, dovrebbe anche specificare un `background-color` che permetta al testo di essere leggibile se l'immagine non viene caricata.

Gli screen reader non possono interpretare le immagini di sfondo; quindi dovrebbero essere puramente decorativi. Ogni contenuto importante dovrebbe far parte della pagina HTML e non essere contenuto in uno sfondo.

## Bordi

Quando si impara sul [modello di box](/it/docs/Learn_web_development/Core/Styling_basics/Box_model), abbiamo scoperto come i bordi influenzano la dimensione del nostro box. In questa lezione, vedremo come usare i bordi in modo creativo. Tipicamente quando aggiungiamo bordi a un elemento con CSS usiamo una proprietà abbreviata che imposta il colore, la larghezza e lo [stile](/it/docs/Web/CSS/line-style) del bordo in una riga di CSS.

Possiamo impostare un bordo per tutti e quattro i lati di un box con {{cssxref("border")}}:

```css
.box {
  border: 1px solid black;
}
```

Oppure possiamo mirare a un bordo del box, ad esempio:

```css
.box {
  border-top: 1px solid black;
}
```

Le proprietà individuali includono le proprietà abbreviate {{cssxref("border-width")}}, {{cssxref("border-style")}}, e {{cssxref("border-color")}}:

```css
.box {
  border-width: 1px;
  border-style: solid;
  border-color: black;
}
```

Ci sono proprietà dettagliate per larghezza, stile e colore per ciascuno dei quattro lati:

```css
.box {
  border-top-width: 1px;
  border-top-style: solid;
  border-top-color: black;
}
```

> [!NOTE]
> Queste proprietà di bordo superiori, destra, inferiore e sinistra hanno anche proprietà di bordo [_logiche_ mappate](/it/docs/Web/CSS/CSS_logical_properties_and_values#properties) che si riferiscono alla modalità di scrittura del documento (ad esempio, testo da sinistra a destra o da destra a sinistra, o dall'alto verso il basso). Esploreremo queste nella lezione su [gestire diverse direzioni di testo](/it/docs/Learn_web_development/Core/Styling_basics/Handling_different_text_directions).

Ci sono una varietà di stili che può usare per i bordi. Nell'esempio qui sotto, abbiamo usato due stili di bordo diversi per il box e due stili di bordo diversi per l'intestazione. Giochi con lo stile del bordo, la larghezza e il colore per vedere come funzionano i bordi.

```html live-sample___borders
<div class="box">
  <h2>Borders</h2>
  <p>Try changing the borders.</p>
</div>
```

```css live-sample___borders
* {
  padding: 0.2em;
}
.box {
  width: 500px;
  background-color: #567895;
  border: 5px solid #0b385f;
  border-bottom-style: dashed;
  color: #fff;
}

h2 {
  border-top: 2px dotted rebeccapurple;
  border-bottom: 1em double rgb(24 163 78);
}
```

{{EmbedLiveSample("borders", "", "200px")}}

## Angoli arrotondati

Arrotondare gli angoli di un box si ottiene usando la proprietà {{cssxref("border-radius")}} e le scorciatoie associate che si riferiscono a ciascun angolo del box. Due lunghezze o percentuali possono essere utilizzate come valore, il primo valore definisce il raggio orizzontale, e il secondo il raggio verticale. In molti casi, passerà solo un valore, che sarà usato per entrambi.

Per esempio, per rendere tutti e quattro gli angoli di un box con un raggio di 10px:

```css
.box {
  border-radius: 10px;
}
```

Oppure per rendere l'angolo superiore destro con un raggio orizzontale di `1em`, e un raggio verticale del 10%:

```css
.box {
  border-top-right-radius: 1em 10%;
}
```

> [!NOTE]
> Come con le proprietà di bordo sopra, anche queste proprietà di border-radius hanno proprietà di border-radius [_logiche_ mappate](/it/docs/Web/CSS/CSS_logical_properties_and_values#properties).

Abbiamo impostato tutti e quattro gli angoli nell'esempio qui sotto e poi modificato i valori per l'angolo superiore destro per renderlo diverso. Può giocare con i valori per modificare gli angoli. Dia un'occhiata alla pagina della proprietà per {{cssxref("border-radius")}} per vedere le opzioni di sintassi disponibili. Il [generatore di border-radius](/it/docs/Web/CSS/CSS_backgrounds_and_borders/Border-radius_generator) può essere utilizzato per generare i valori degli angoli arrotondati per lei.

```html live-sample___corners
<div class="box">
  <h2>Borders</h2>
  <p>Try changing the borders.</p>
</div>
```

```css live-sample___corners
.box {
  width: 500px;
  height: 110px;
  padding: 0.5em;
  border: 10px solid rebeccapurple;
  border-radius: 1em;
  border-top-right-radius: 10% 30%;
}
```

{{EmbedLiveSample("corners")}}

## Testi le sue competenze!

Ha raggiunto la fine di questo articolo, ma ricorda le informazioni più importanti? Può trovare alcuni ulteriori test per verificare che abbia acquisito queste informazioni prima di procedere — vedere [Testi le sue competenze: Sfondi e bordi](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Backgrounds_and_borders).

## Riassunto

Può vedere che c'è molto da aggiungere a uno sfondo o un bordo a un box. Esplori le diverse pagine delle proprietà se desidera saperne di più su qualsiasi delle caratteristiche discusse qui. Quasi ogni pagina su MDN ha esempi per lei da provare per migliorare le sue conoscenze.

Nel prossimo articolo, impareremo di più sul concetto di overflow, che governa cosa succede quando c'è troppo contenuto da adattare all'interno di un box dell'elemento.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Sizing", "Learn_web_development/Core/Styling_basics/Overflow", "Learn_web_development/Core/Styling_basics")}}
