---
title: "Metti alla prova le tue abilità: Valori e unità"
short-title: Valori e unità
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Values
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test delle abilità è valutare se ha compreso i diversi tipi di [valori e unità utilizzati nelle proprietà CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units).

> [!NOTE]
> Clicchi su **"Play"** nei blocchi di codice sottostanti per modificare gli esempi nel MDN Playground.
> Può anche copiare il codice (clicchi sull'icona degli appunti) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se si trova in difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito 1

In questo compito, al primo elemento dell'elenco è stato assegnato un colore di sfondo utilizzando un codice colore esadecimale. Il suo compito è completare il CSS utilizzando lo stesso colore in diversi formati, oltre a un elemento finale dell'elenco in cui dovrebbe rendere lo sfondo semi-trasparente.

- Il secondo elemento dell'elenco dovrebbe usare il colore RGB.
- Il terzo dovrebbe usare il colore HSL.
- Il quarto dovrebbe usare il colore RGB ma con il canale alpha impostato a `0.6`.

[Può convertire il colore esadecimale su convertingcolors.com](https://convertingcolors.com/hex-color-86DEFA.html). Deve capire come utilizzare i valori in CSS. Il risultato finale dovrebbe apparire come l'immagine sottostante:

![Quattro elementi dell'elenco. I primi tre con lo stesso colore di sfondo e l'ultimo con uno sfondo più chiaro.](mdn-value-color.png)

Provi ad aggiornare il codice sottostante per ricreare l'esempio finito:

```html live-sample___color
<ul>
  <li class="hex">hex color</li>
  <li class="rgb">RGB color</li>
  <li class="hsl">HSL color</li>
  <li class="transparency">Alpha value 0.6</li>
</ul>
```

```css hidden live-sample___color
body {
  font: 1.2em / 1.5 sans-serif;
}
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

li {
  margin: 1em;
  padding: 0.5em;
}
```

```css live-sample___color
.hex {
  background-color: #86defa;
}

/* Add styles here */
```

{{EmbedLiveSample("color", "", "300px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Utilizzando [uno strumento di conversione dei colori](https://convertingcolors.com/hex-color-86DEFA.html), dovrebbe essere in grado di utilizzare diverse [funzioni di colore](/it/docs/Web/CSS/color_value#syntax) per definire lo stesso colore in modi diversi:

```css
.hex {
  background-color: #86defa;
}

.rgb {
  background-color: rgb(134 222 250);
}

.hsl {
  background-color: hsl(194 92% 75%);
}

.transparency {
  background-color: rgb(134 222 250 / 60%);
}
```

</details>

## Compito 2

In questo compito, vogliamo che imposti la dimensione di vari elementi di testo, come descritto di seguito:

- L'elemento `<h1>` dovrebbe essere 50 pixel.
- L'elemento `<h2>` dovrebbe essere 2em.
- Tutti gli elementi `<p>` dovrebbero essere 16 pixel.
- Un elemento `<p>` direttamente dopo un `<h1>` dovrebbe essere 120%.

Il risultato finale dovrebbe apparire come l'immagine sottostante:

![Testo di varie dimensioni.](mdn-value-length.png)

Provi ad aggiornare il codice sottostante per ricreare l'esempio finito:

```html live-sample___length
<h1>Level 1 heading</h1>
<p>
  Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi welsh onion
  daikon amaranth tatsoi tomatillo melon azuki bean garlic.
</p>
<h2>Level 2 heading</h2>
<p>
  Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot courgette
  tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato.
  Dandelion cucumber earthnut pea peanut soko zucchini.
</p>
```

```css hidden live-sample___length
body {
  font: 1.2em / 1.5 sans-serif;
}
```

```css live-sample___length
h1 {
}

h2 {
}

p {
}

h1 + p {
}
```

{{EmbedLiveSample("length", "", "420px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Può utilizzare i seguenti valori di lunghezza:

```css
h1 {
  font-size: 50px;
}

h2 {
  font-size: 2em;
}

p {
  font-size: 16px;
}

h1 + p {
  font-size: 120%;
}
```

</details>

## Compito 3

In questo compito, vogliamo che sposti l'immagine di sfondo in modo che sia centrata orizzontalmente e sia al 20% dalla parte superiore della casella.

Il risultato finale dovrebbe apparire come l'immagine sottostante:

![Un dato centrato orizzontalmente in una casella e a una breve distanza dalla parte superiore della casella.](mdn-value-position.png)

Provi ad aggiornare il codice sottostante per ricreare l'esempio finito:

```html live-sample___position
<div class="box"></div>
```

```css hidden live-sample___position
.box {
  border: 5px solid #000;
  height: 350px;
}
```

```css live-sample___position
.box {
  background-image: url(https://mdn.github.io/shared-assets/images/examples/purple-star.png);
  background-repeat: no-repeat;
}
```

{{EmbedLiveSample("position", "", "400px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Utilizzi `background-position` con la parola chiave `center` e una percentuale:

```css
.box {
  background-image: url(https://mdn.github.io/shared-assets/images/examples/purple-star.png);
  background-repeat: no-repeat;
  background-position: center 20%;
}
```

</details>

## Vedere anche

- [Elementi di base dello styling CSS](/it/docs/Learn_web_development/Core/Styling_basics)
