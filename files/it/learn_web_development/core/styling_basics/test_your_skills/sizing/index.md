---
title: "Metti alla prova le tue competenze: dimensionamento"
short-title: "Test: dimensionamento"
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Sizing
l10n:
  sourceCommit: a623d4459e2aa00d17dc0fd6b6bc44f56c589950
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Sizing", "Learn_web_development/Core/Styling_basics/Backgrounds_and_borders", "Learn_web_development/Core/Styling_basics")}}

L'obiettivo di questo test sulle competenze è aiutare a valutare se si comprendono i diversi modi di [dimensionare gli elementi in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Sizing).

> [!NOTE]
> Per ricevere assistenza, leggere la nostra guida all'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È inoltre possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Dimensionamento 1

In questa attività sono presenti due riquadri.

Per completare l'attività:

1. Dimensionare il primo riquadro in modo che l'altezza sia almeno `100px`, anche se il contenuto è insufficiente per farlo crescere fino a tale altezza. Il contenuto non deve fuoriuscire se non entra nel riquadro.
2. Per verificarlo, rimuovere il contenuto dall'HTML per assicurarsi di ottenere comunque un riquadro alto `100px` anche senza contenuto.
3. Dimensionare il secondo riquadro in modo che abbia un'altezza fissa di `100px`. In questo caso, il contenuto deve fuoriuscire.

Il punto di partenza dell'attività è simile a questo:

{{EmbedLiveSample("sizing1-start", "", "480px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___sizing1-start live-sample___sizing1-finish
<div class="box box1">
  <p>
    Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi welsh onion
    daikon amaranth tatsoi tomatillo melon azuki bean garlic. Gumbo beet greens
    corn soko endive gumbo gourd.
  </p>
</div>

<div class="box box2">
  <p>
    Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi welsh onion
    daikon amaranth tatsoi tomatillo melon azuki bean garlic. Gumbo beet greens
    corn soko endive gumbo gourd.
  </p>
</div>
```

```css live-sample___sizing1-start live-sample___sizing1-finish
body {
  font: 1.2em / 1.5 sans-serif;
  padding: 1em;
}

.box {
  border: 5px solid black;
  width: 400px;
  margin-bottom: 1em;
}

.box1 {
  /* Add styles here */
}

.box2 {
  /* Add styles here */
}
```

Lo stile aggiornato dovrebbe essere simile a questo:

{{EmbedLiveSample("sizing1-finish", "", "460px")}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Sono presenti due riquadri. Al primo deve essere assegnato un valore `min-height` affinché si espanda per contenere il contenuto aggiuntivo, ma non si riduca al di sotto di `100px` di altezza se il contenuto viene rimosso. Al secondo riquadro viene assegnata un'altezza fissa, che causerà la fuoriuscita del contenuto.

```css live-sample___sizing1-finish
.box1 {
  min-height: 100px;
}

.box2 {
  height: 100px;
}
```

</details>

## Dimensionamento 2

In questa attività è presente un riquadro che ne contiene un altro.

Per completare l'attività:

1. Impostare la larghezza del riquadro interno al `60%` della larghezza del riquadro esterno. La proprietà {{cssxref("box-sizing")}} è impostata su `border-box`, il che significa che la larghezza totale include eventuali `padding` e `border`.
2. Assegnare al riquadro interno un `padding` del `10%` su tutti i lati.

Il punto di partenza dell'attività è simile a questo:

{{EmbedLiveSample("sizing2-start", "", "100px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___sizing2-start live-sample___sizing2-finish
<div class="box">
  <div class="inner">Make me 60% of my parent's width.</div>
</div>
```

```css live-sample___sizing2-start live-sample___sizing2-finish
body {
  font: 1.2em / 1.5 sans-serif;
  padding: 1em;
}

.box {
  border: 5px solid black;
  width: 400px;
  margin-bottom: 1em;
}

.inner {
  background-color: rebeccapurple;
  color: white;
  border-radius: 5px;
}

* {
  box-sizing: border-box;
}
.inner {
  /* Add styles here */
}
```

Lo stile aggiornato dovrebbe essere simile a questo:

{{EmbedLiveSample("sizing2-finish", "", "220px")}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Impostare `width` del riquadro su `60%` e assegnargli un valore `padding` di `10%`.
Tutti gli elementi hanno già `box-sizing: border-box` impostato, così non è necessario preoccuparsi di calcolare il valore della larghezza al `60%`:

```css live-sample___sizing2-finish
.inner {
  width: 60%;
  padding: 10%;
}
```

</details>

## Dimensionamento 3

In questa attività sono presenti due immagini in riquadri. Un'immagine è più piccola del riquadro, mentre l'altra è più grande e quindi fuoriesce dal riquadro.

Per completare l'attività, immaginare che il riquadro sia responsive e che quindi possa crescere e ridursi. Applicare una dichiarazione alle immagini in modo che l'immagine grande si riduca per entrare nel riquadro, mentre l'immagine piccola non venga allungata.

Il punto di partenza dell'attività è simile a questo:

{{EmbedLiveSample("sizing3-start", "", "700px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___sizing3-start live-sample___sizing3-finish
<div class="box">
  <img
    alt="A pink star"
    src="https://mdn.github.io/shared-assets/images/examples/star-pink_256x256.png" />
</div>

<div class="box">
  <img
    alt="Hot air balloons flying in clear sky, and a crowd of people in the foreground"
    src="https://mdn.github.io/shared-assets/images/examples/balloons.jpg" />
</div>
```

```css live-sample___sizing3-start live-sample___sizing3-finish
body {
  font: 1.2em / 1.5 sans-serif;
  padding: 1em;
}
.box {
  border: 5px solid black;
  margin-bottom: 1em;
  width: 500px;
}

img {
  /* Add styles here */
}
```

Lo stile aggiornato dovrebbe essere simile a questo:

{{EmbedLiveSample("sizing3-finish", "", "720px")}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Impostare la proprietà `max-width` delle immagini su `100%` per contenere l'immagine grande all'interno del relativo riquadro. Se si usa `width: 100%`, l'immagine piccola verrà allungata.

```css live-sample___sizing3-finish
img {
  max-width: 100%;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Sizing", "Learn_web_development/Core/Styling_basics/Backgrounds_and_borders", "Learn_web_development/Core/Styling_basics")}}
