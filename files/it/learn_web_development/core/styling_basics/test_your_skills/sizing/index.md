---
title: "Testa le tue abilità: Dimensionamento"
short-title: Sizing
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Sizing
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Lo scopo di questo test di abilità è valutare se comprende i diversi modi di [dimensionare gli elementi in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Sizing).

> [!NOTE]
> Clicchi su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Può anche copiare il codice (cliccando sull'icona della clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se si trova in difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito 1

In questo compito, ha due caselle. La prima deve essere dimensionata in modo che l'altezza sia di almeno 100 pixel, anche se vi è meno contenuto che farebbe espandere la casella fino a tale altezza. Tuttavia, il contenuto non deve traboccare se c'è più contenuto di quanto possa entrare in 100 pixel. Testi questa casella rimuovendo il contenuto dall'HTML per assicurarsi che ottiene ancora una casella alta 100 pixel anche senza contenuto.

La seconda casella deve essere fissata a 100 pixel di altezza, in modo che il contenuto trabocchi se ce n'è troppo.

![Due caselle una con contenuto traboccante](mdn-sizing-height-min-height.png)

Provi a aggiornare il codice qui sotto per ricreare l'esempio finale:

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
<summary>Clicchi qui per vedere la soluzione</summary>

Ci sono due caselle, la prima dovrebbe avere un'altezza minima, in tal caso si espanderà per accogliere il contenuto aggiuntivo ma se rimuove un po' di contenuto, la casella sarà alta almeno quanto il `min-height`. La seconda ha un'altezza fissa che farà traboccare il contenuto.

```css
.box1 {
  min-height: 100px;
}

.box2 {
  height: 100px;
}
```

</details>

## Compito 2

In questo compito, ha una casella che contiene un'altra casella. Il suo compito è rendere la casella interna larga il 60% rispetto alla casella esterna. Il valore della proprietà {{cssxref("box-sizing")}} è impostato su `border-box`, il che significa che la larghezza totale include qualsiasi padding e bordo. Inoltre, dovrebbe dare alla casella interna un padding del 10% utilizzando la larghezza (o dimensione in linea) come la misura da cui viene calcolato tale percentuale.

Il suo risultato finale dovrebbe apparire come l'immagine qui sotto:

![Una casella con un'altra casella annidata all'interno](mdn-sizing-percentages.png)

Provi a aggiornare il codice qui sotto per ricreare l'esempio finale:

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
<summary>Clicchi qui per vedere la soluzione</summary>

Faccia la casella al 60% del contenitore e dati un padding del 10% su tutti i lati.
Tutti gli elementi hanno già `box-sizing: border-box` per evitarle di preoccuparsi su quale larghezza sta usando:

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

## Compito 3

In questo compito, ha due immagini in caselle. Un'immagine è più piccola della casella, l'altra è più grande e esce dalla casella. Se immagina che la casella sia reattiva e quindi potrebbe crescere e ridursi, quale proprietà applicherebbe all'immagine in modo che l'immagine grande si riduca nella casella ma l'immagine piccola non si estenda.

Il suo risultato finale dovrebbe apparire come le immagini qui sotto:

![Due caselle con immagini dentro](mdn-sizing-max-width.png)

Provi a aggiornare il codice qui sotto per ricreare l'esempio finale:

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
<summary>Clicchi qui per vedere la soluzione</summary>

L'esempio ha un'immagine che esce dalla casella e una che è più piccola della casella, è necessario usare `max-width` impostato al 100% per fare in modo che l'immagine più grande cresca solo fino alla dimensione della casella. Se usa `width: 100%`, l'immagine piccola si estenderà.

```css
img {
  max-width: 100%;
}
```

</details>

## Vedi anche

- [Nozioni di base sullo styling CSS](/it/docs/Learn_web_development/Core/Styling_basics)
