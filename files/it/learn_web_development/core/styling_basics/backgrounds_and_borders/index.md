---
title: Backgrounds e bordi
slug: Learn_web_development/Core/Styling_basics/Backgrounds_and_borders
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Sizing", "Learn_web_development/Core/Styling_basics/Overflow", "Learn_web_development/Core/Styling_basics")}}

In questa lezione, esamineremo alcune delle cose creative che puoi fare con i background e i bordi in CSS. Dall'aggiunta di gradienti, immagini di sfondo e angoli arrotondati, i background e i bordi rappresentano la soluzione a molte domande di stile in CSS.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Basi di HTML (studia
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base di HTML</a
        >), <a href="/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units">Valori e unità CSS</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics/Sizing">Dimensionamento CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Stilizzazione base dei background — colori e immagini.</li>
          <li>Dimensione, ripetizione, posizione e attacco dell'immagine di sfondo.</li>
          <li>Gradienti di sfondo — concetto generale e gradienti lineari (i gradienti radiali, conici e ripetuti sono più avanzati; la conoscenza approfondita non è richiesta a questo stadio.)</li>
          <li>Considerazioni sull'accessibilità dei background — assicurare un buon contrasto.</li>
          <li>Nozioni di base sui bordi — larghezza, stile, colore e scorciatoia del bordo. Raggio del bordo per angoli arrotondati.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Stilizzazione dei background in CSS

La proprietà CSS {{cssxref("background")}} è una scorciatoia per un numero di proprietà di background dettagliate che incontreremo in questa lezione. Se scopri una proprietà di background complessa in un foglio di stile, potrebbe sembrare un po' difficile da capire poiché molti valori possono essere passati contemporaneamente:

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

Torneremo su come funziona la scorciatoia più avanti nel tutorial, ma prima diamo un'occhiata alle diverse cose che puoi fare con i background in CSS, esaminando le singole proprietà del background.

## Colori di sfondo

La proprietà {{cssxref("background-color")}} definisce il colore di sfondo su qualsiasi elemento in CSS. La proprietà accetta qualsiasi valore valido di [`<color>`](/it/docs/Web/CSS/color_value). Un `background-color` si estende sotto il contenuto e il padding box dell'elemento.

Nell'esempio seguente, abbiamo utilizzato vari valori di colore per aggiungere un colore di sfondo alla casella, a un'intestazione e a un elemento {{htmlelement("span")}}.
Prova tu stesso, utilizzando qualsiasi valore disponibile di [`<color>`](/it/docs/Web/CSS/color_value).

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

La proprietà {{cssxref("background-image")}} consente la visualizzazione di un'immagine sullo sfondo di un elemento. Nell'esempio seguente, abbiamo due caselle — una ha un'immagine di sfondo più grande della scatola ([balloons.jpg](https://mdn.github.io/shared-assets/images/examples/balloons.jpg)). L'altra ha una piccola immagine di una singola stella ([star.png](https://mdn.github.io/shared-assets/images/examples/star.png)).

Questo esempio dimostra due cose sulle immagini di sfondo. Per impostazione predefinita, l'immagine grande non viene ridimensionata per adattarsi alla casella, quindi possiamo vedere solo un piccolo angolo di essa, mentre l'immagine piccola viene replicata per riempire la casella.

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

Se specifichi un colore di sfondo oltre a un'immagine di sfondo, l'immagine viene visualizzata sopra il colore.
Prova ad aggiungere una proprietà `background-color` all'esempio sopra per vedere il risultato.

### Controllo della ripetizione del background

La proprietà {{cssxref("background-repeat")}} è utilizzata per controllare il comportamento del tiling delle immagini. I valori disponibili sono:

- `no-repeat` — impedisce alla sfondo di ripetersi del tutto.
- `repeat-x` — ripeti orizzontalmente.
- `repeat-y` — ripeti verticalmente.
- `repeat` — di default; ripeti in entrambe le direzioni.
- `space` — ripeti il più possibile, aggiungendo spazio tra le immagini se c'è spazio extra disponibile.
- `round` — simile a `space`, ma allunga le immagini per riempire qualsiasi spazio extra

Prova questi valori nell'esempio seguente. Abbiamo impostato il valore su `no-repeat` in modo che vedrai solo una stella. Prova i diversi valori — `repeat-x` e `repeat-y` — per vedere i loro effetti.

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

### Dimensionare l'immagine di sfondo

L'immagine _balloons.jpg_ utilizzata nell'esempio iniziale dell'immagine di sfondo è un'immagine grande che è stata ritagliata a causa delle sue dimensioni maggiori rispetto all'elemento su cui è impostata come sfondo. In questo caso, potremmo usare la proprietà {{cssxref("background-size")}}, che può accettare valori di {{cssxref("length")}} o {{cssxref("percentage")}}, per dimensionare l'immagine all'interno dello sfondo.

Puoi anche utilizzare parole chiave:

- `cover` — il browser renderà l'immagine abbastanza grande da coprire completamente l'area della scatola mantenendo il suo {{Glossary("aspect_ratio", "rapporto di aspetto")}}. In questo caso, parte dell'immagine potrebbe finire fuori dalla scatola.
- `contain` — il browser renderà l'immagine della dimensione giusta per adattarsi all'interno della scatola. In questo caso, potresti avere degli spazi vuoti su entrambi i lati o sulla parte superiore e inferiore dell'immagine, se il rapporto di aspetto dell'immagine è diverso da quello della scatola.

Nell'esempio seguente, l'immagine _balloons.jpg_ ha unità di lunghezza impostate per dimensionarla all'interno della scatola. Puoi vedere che ciò ha distorto l'immagine.

Prova quanto segue:

- Cambia le unità di lunghezza utilizzate per modificare la dimensione del background.
- Rimuovi le unità di lunghezza e osserva cosa succede quando usi `background-size: cover` o `background-size: contain`.
- Se l'immagine è più piccola della scatola, puoi cambiare il valore di `background-repeat` per replicare l'immagine.

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

### Posizionamento dell'immagine di sfondo

La proprietà {{cssxref("background-position")}} ti consente di scegliere la posizione in cui l'immagine di sfondo appare sulla casella su cui è applicata. Usa un sistema di coordinate in cui l'angolo in alto a sinistra della casella è `(0,0)`, e la casella è posizionata lungo gli assi orizzontale (`x`) e verticale (`y`).

> [!NOTE]
> Il valore predefinito di `background-position` è `(0,0)`.

I valori `background-position` più comuni prendono due valori individuali — un valore orizzontale seguito da un valore verticale.

Puoi usare parole chiave come `top` e `right` (consulta le altre sulla pagina {{cssxref("background-position")}}):

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

Puoi anche mescolare valori di parole chiave con lunghezze o percentuali, in tal caso il primo valore deve riferirsi alla posizione o offset orizzontale e il secondo verticale. Per esempio:

```css
.box {
  background-image: url(image.png);
  background-repeat: no-repeat;
  background-position: 20px top;
}
```

Infine, puoi anche usare una sintassi a 4 valori per indicare una distanza da certi bordi della scatola — l'unità di lunghezza, in questo caso, è un offset dal valore che lo precede. Quindi nel CSS qui sotto stiamo posizionando il background a 20px dall'alto e 10px dalla destra:

```css
.box {
  background-image: url(image.png);
  background-repeat: no-repeat;
  background-position: top 20px right 10px;
}
```

Usa l'esempio seguente per giocare con questi valori e spostare la stella all'interno della scatola:

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
> La scorciatoia `background-position` è utilizzata invece di {{cssxref("background-position-x")}} e {{cssxref("background-position-y")}}, che ti permettono di impostare i valori delle posizioni degli assi differentemente.

## Gradiente di sfondo

Un gradiente — quando usato come sfondo — si comporta proprio come un'immagine ed è impostato anche usando la proprietà {{cssxref("background-image")}}.

Puoi leggere di più sui diversi tipi di gradienti e sulle cose che puoi fare con loro sulla pagina MDN per il tipo di dati [`<gradient>`](/it/docs/Web/CSS/gradient). Un modo divertente di giocare con i gradienti è utilizzare uno dei molti generatori di gradienti CSS disponibili sul web, come [CSSGradient.io](https://cssgradient.io/). Puoi creare un gradiente e copiare e incollare il codice sorgente che lo genera.

Prova alcuni gradienti diversi nell'esempio seguente. Nelle due caselle rispettivamente, abbiamo un gradiente lineare che è allungato su tutta la casella, e un gradiente radiale con una dimensione impostata, che dunque si ripete.

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

È anche possibile avere immagini di sfondo multiple — specifichi più valori di `background-image` in un unico valore di proprietà, separando ciascuno con una virgola.

Quando fai ciò, potresti finire con immagini di sfondo che si sovrappongono tra loro. Gli sfondi sono disposti a strati con l'ultima immagine di sfondo elencata nella parte inferiore della pila, e ciascuna immagine precedente che si impila sopra quella che la segue nel codice.

> [!NOTE]
> I gradienti possono essere felicemente mescolati con immagini di sfondo regolari.

Le altre proprietà `background-*` possono anche avere valori separati da virgola nello stesso modo di `background-image`:

```css
background-image:
  url(image1.png), url(image2.png), url(image3.png), url(image4.png);
background-repeat: no-repeat, repeat-x, repeat;
background-position:
  10px 20px,
  top right;
```

Ogni valore delle diverse proprietà si abbinerà ai valori nella stessa posizione nelle altre proprietà. Sopra, ad esempio, il valore di `background-repeat` di `image1` sarà `no-repeat`. Comunque, cosa succede quando diverse proprietà hanno numeri diversi di valori? La risposta è che i numeri minori di valori cicleranno — nell'esempio sopra ci sono quattro immagini di sfondo ma solo due valori `background-position`. I primi due valori di posizione saranno applicati alle prime due immagini, poi cicleranno di nuovo — `image3` avrà assegnato il primo valore di posizione, e `image4` avrà assegnato il secondo valore di posizione.

Giochiamo. L'esempio sotto include due immagini di sfondo. Per dimostrare l'ordine della sequenza, prova a invertire quale immagine di sfondo è elencata per prima. Oppure gioca con le altre proprietà per cambiare la posizione, la dimensione o i valori di ripetizione.

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

## Attacco del background

Un'altra opzione disponibile per gli sfondi è specificare come scorrono quando il contenuto scorre. Questo è controllato usando la proprietà {{cssxref("background-attachment")}}, che può prendere i seguenti valori:

- `scroll`: fa sì che lo sfondo dell'elemento scorra quando la pagina viene scorsa. Se il contenuto dell'elemento viene scorrere, lo sfondo non si muove. In effetti, lo sfondo è fissato nella stessa posizione sulla pagina, quindi scorre mentre la pagina scorre.
- `fixed`: fa sì che lo sfondo di un elemento sia fissato al viewport, in modo che non scorra quando la pagina o il contenuto dell'elemento vengono scorsi. Rimarrà sempre nella stessa posizione sullo schermo.
- `local`: fissa il background all'elemento su cui è impostato, quindi quando scorri l'elemento, lo sfondo scorre con esso.

La proprietà {{cssxref("background-attachment")}} ha effetto solo quando c'è del contenuto da scorrere, quindi abbiamo creato una demo per dimostrare le differenze tra i tre valori — dai un'occhiata a [background-attachment.html](https://mdn.github.io/learning-area/css/styling-boxes/backgrounds/background-attachment.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/css/styling-boxes/backgrounds) qui).

## Utilizzare la proprietà shorthand `background`

Come menzionato all'inizio di questa lezione, si vedono spesso i background specificati utilizzando la proprietà {{cssxref("background")}}. Questa scorciatoia ti permette di impostare tutte le diverse proprietà contemporaneamente.

Se usi multiple immagini di sfondo, devi specificare tutte le proprietà per il primo sfondo, quindi aggiungere il tuo prossimo sfondo dopo una virgola. Nell'esempio sottostante abbiamo un gradiente con una dimensione e posizione, poi un'immagine di sfondo con `no-repeat` e una posizione, poi un colore.

Ci sono alcune regole che devono essere seguite quando si scrivono i valori shorthand dell'immagine di sfondo, per esempio:

- Un `background-color` può essere specificato solo dopo l'ultima virgola.
- Il valore di `background-size` può essere incluso solo immediatamente dopo `background-position`, separato dal carattere '/' così: `center/80%`.

Dai un'occhiata alla pagina MDN per {{cssxref("background")}} per vedere tutte le considerazioni.

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

## Considerazioni sull'accessibilità con i background

Quando si posiziona il testo sopra un'immagine o un colore di sfondo, si dovrebbe fare attenzione ad avere abbastanza [contrasto](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Perceivable/Color_contrast) affinché il testo sia leggibile per i tuoi visitatori. Se si specifica un'immagine, e se il testo sarà posizionato sopra quell'immagine, dovresti anche specificare un `background-color` che permetterà al testo di essere leggibile se l'immagine non viene caricata.

I lettori di schermo non possono interpretare le immagini di sfondo; pertanto, dovrebbero essere puramente ornamentali. Qualsiasi contenuto importante dovrebbe far parte della pagina HTML e non essere contenuto in un'immagine di sfondo.

## Bordi

Quando si impara il [modello a scatola](/it/docs/Learn_web_development/Core/Styling_basics/Box_model), abbiamo scoperto come i bordi influiscono sulla dimensione della nostra scatola. In questa lezione, vedremo come usare i bordi in modo creativo. Tipicamente, quando aggiungiamo bordi a un elemento con CSS, utilizziamo una proprietà di scorciatoia che imposta il colore, la larghezza e lo [stile](/it/docs/Web/CSS/line-style) del bordo in una riga di CSS.

Possiamo impostare un bordo per tutti e quattro i lati di una scatola con {{cssxref("border")}}:

```css
.box {
  border: 1px solid black;
}
```

Oppure possiamo mirare a un solo lato della scatola, ad esempio:

```css
.box {
  border-top: 1px solid black;
}
```

Le proprietà individuali includono le proprietà shorthand {{cssxref("border-width")}}, {{cssxref("border-style")}}, e {{cssxref("border-color")}}:

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
> Queste proprietà per i bordi superiore, destro, inferiore e sinistro hanno anche proprietà di confine [_logiche_ mappate](/it/docs/Web/CSS/CSS_logical_properties_and_values#properties) che si riferiscono al modo di scrittura del documento (ad esempio, testo da sinistra a destra o da destra a sinistra, o dall'alto in basso). Esploreremo queste nella lezione su [gestire diverse direzioni di testo](/it/docs/Learn_web_development/Core/Styling_basics/Handling_different_text_directions).

Ci sono una varietà di stili che puoi utilizzare per i bordi. Nell'esempio seguente, abbiamo usato due diversi stili di bordo per la casella e due diversi stili di bordo per l'intestazione. Gioca con lo stile del bordo, la larghezza e il colore per vedere come funzionano i bordi.

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

L'arrotondamento degli angoli su una casella viene ottenuto utilizzando la proprietà {{cssxref("border-radius")}} e le relative proprietà dettagliate associate che si riferiscono a ciascun angolo della scatola. Possono essere utilizzate due lunghezze o percentuali come valore, il primo valore definisce il raggio orizzontale, e il secondo il raggio verticale. In molti casi, passerai solo un valore, che verrà utilizzato per entrambi.

Ad esempio, per fare in modo che tutti e quattro gli angoli di una casella abbiano un raggio di 10px:

```css
.box {
  border-radius: 10px;
}
```

Oppure per fare in modo che l'angolo in alto a destra abbia un raggio orizzontale di `1em`, e un raggio verticale del 10%:

```css
.box {
  border-top-right-radius: 1em 10%;
}
```

> [!NOTE]
> Come con le proprietà di bordo sopra, queste proprietà di `border-radius` hanno anche proprietà di `border-radius` [_logiche_ mappate](/it/docs/Web/CSS/CSS_logical_properties_and_values#properties).

Abbiamo impostato tutti e quattro gli angoli nell'esempio seguente e poi cambiato i valori per l'angolo in alto a destra per renderlo diverso. Puoi giocare con i valori per cambiare gli angoli. Dai un'occhiata alla pagina della proprietà per {{cssxref("border-radius")}} per vedere le opzioni di sintassi disponibili. Il [generatore di border-radius](/it/docs/Web/CSS/CSS_backgrounds_and_borders/Border-radius_generator) può essere utilizzato per generare valori di angoli arrotondati per te.

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

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare di aver mantenuto queste informazioni prima di procedere — vedi [Metti alla prova le tue abilità: Backgrounds e borders](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Backgrounds_and_borders).

## Sommario

Puoi vedere che c'è molto nell'aggiungere un background o un bordo a una scatola. Esplora le diverse pagine delle proprietà se vuoi saperne di più su qualsiasi delle funzionalità discusse qui. Quasi ogni pagina su MDN ha esempi con cui giocare per migliorare le tue conoscenze.

Nel prossimo articolo, impareremo di più sul concetto di overflow, che governa cosa succede quando c'è troppo contenuto per adattarsi all'interno di una scatola di un elemento.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Sizing", "Learn_web_development/Core/Styling_basics/Overflow", "Learn_web_development/Core/Styling_basics")}}
