---
title: "Metti alla prova le tue abilità: Dimensionamento"
short-title: Sizing
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Sizing
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se comprendi i diversi modi di [dimensionare gli elementi in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Sizing).

> [!NOTE]
> Clicca su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Puoi anche copiare il codice (clicca sull'icona della clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se incontri difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Esercizio 1

In questo esercizio, hai due contenitori. Il primo dovrebbe essere dimensionato in modo tale che l'altezza sia di almeno 100 pixel, anche se c'è meno contenuto che potrebbe farlo crescere fino a quell'altezza. Tuttavia, il contenuto non dovrebbe traboccare se c'è più contenuto di quanto entri in 100 pixel. Prova questo contenitore rimuovendo il contenuto dall'HTML per assicurarti di ottenere comunque un contenitore alto 100 pixel anche senza contenuto.

Il secondo contenitore dovrebbe essere fissato a 100 pixel di altezza, in modo che il contenuto trabocchi se ce n'è troppo.

![Due contenitori, uno con contenuto che trabocca](mdn-sizing-height-min-height.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finale:

```html live-sample___height-min-height
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

```css hidden live-sample___height-min-height
body {
  font: 1.2em / 1.5 sans-serif;
  padding: 1em;
}

.box {
  border: 5px solid #000;
  width: 400px;
  margin-bottom: 1em;
}
```

```css live-sample___height-min-height
.box1 {
  /* Add styles here */
}

.box2 {
  /* Add styles here */
}
```

{{EmbedLiveSample("height-min-height", "", "500px")}}

<details>
<summary>Clicca qui per mostrare la soluzione</summary>

Ci sono due contenitori, al primo dovrebbe essere assegnata un'altezza minima, nel qual caso si espanderà per accogliere il contenuto aggiuntivo, ma se rimuovi del contenuto, il contenitore sarà almeno alto quanto il `min-height`. Al secondo è assegnata un'altezza fissa che farà traboccare il contenuto.

```css
.box1 {
  min-height: 100px;
}

.box2 {
  height: 100px;
}
```

</details>

## Esercizio 2

In questo esercizio, hai un contenitore, che ne contiene un altro. Il tuo compito è rendere il contenitore interno il 60% della larghezza del contenitore esterno. Il valore della proprietà {{cssxref("box-sizing")}} è impostato su `border-box`, il che significa che la larghezza totale include eventuali padding e bordi. Dovresti anche dare al contenitore interno un padding del 10% utilizzando la larghezza (o dimensione inline) come misura da cui calcolare quella percentuale.

Il risultato finale dovrebbe assomigliare all'immagine qui sotto:

![Un contenitore con un altro contenitore annidato all'interno](mdn-sizing-percentages.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finale:

```html live-sample___percentages
<div class="box">
  <div class="inner">Make me 60% of my parent's width.</div>
</div>
```

```css hidden live-sample___percentages
body {
  font: 1.2em / 1.5 sans-serif;
  padding: 1em;
}
.box {
  border: 5px solid #000;
  width: 400px;
  margin-bottom: 1em;
}

.inner {
  background-color: rebeccapurple;
  color: white;
  border-radius: 5px;
}
```

```css live-sample___percentages
* {
  box-sizing: border-box;
}
.inner {
  /* Add styles here */
}
```

{{EmbedLiveSample("percentages", "", "250px")}}

<details>
<summary>Clicca qui per mostrare la soluzione</summary>

Rendi il contenitore il 60% del contenitore esterno e dagli un padding del 10% su tutti i lati.
Tutti gli elementi hanno già `box-sizing: border-box` per evitare di preoccuparti di quale larghezza stai usando:

```css
* {
  box-sizing: border-box;
}
.inner {
  width: 60%;
  padding: 10%;
}
```

</details>

## Esercizio 3

In questo esercizio, hai due immagini in contenitori. Un'immagine è più piccola del contenitore, l'altra è più grande e esce dal contenitore. Se immagini che il contenitore sia responsivo e quindi possa crescere e ridursi, quale proprietà applicheresti all'immagine affinché l'immagine grande si riduca nel contenitore ma l'immagine piccola non si estenda.

Il risultato finale dovrebbe assomigliare alle immagini qui sotto:

![Due contenitori con immagini all'interno](mdn-sizing-max-width.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finale:

```html live-sample___max-width
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

```css hidden live-sample___max-width
body {
  font: 1.2em / 1.5 sans-serif;
  padding: 1em;
}
.box {
  border: 5px solid #000;
  margin-bottom: 1em;
  width: 500px;
}
```

```css live-sample___max-width
img {
  /* Add styles here */
}
```

{{EmbedLiveSample("max-width", "", "700px")}}

<details>
<summary>Clicca qui per mostrare la soluzione</summary>

L'esempio ha un'immagine che sta uscendo dal contenitore e una che è più piccola del contenitore, devi usare `max-width` impostato al 100% per far sì che l'immagine più grande cresca solo quanto il contenitore. Se usi `width: 100%`, l'immagine piccola si estenderà.

```css
img {
  max-width: 100%;
}
```

</details>

## Vedi anche

- [Nozioni di base sullo stile CSS](/it/docs/Learn_web_development/Core/Styling_basics)
