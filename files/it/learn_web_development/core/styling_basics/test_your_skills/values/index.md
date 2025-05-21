---
title: "Metti alla prova le tue abilità: Valori e unità"
short-title: Valori e unità
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Values
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test delle abilità è valutare se comprendi i diversi tipi di [valori e unità usati nelle proprietà CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units).

> [!NOTE]
> Clicca su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Puoi anche copiare il codice (clicca sull'icona degli appunti) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se incontri difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Attività 1

In questa attività, al primo elemento dell'elenco è stato assegnato un colore di sfondo usando un codice colore esadecimale. Il tuo compito è completare il CSS utilizzando lo stesso colore in formati diversi, più un elemento finale dove dovresti rendere lo sfondo semiopaco.

- Il secondo elemento dell'elenco dovrebbe utilizzare il colore RGB.
- Il terzo dovrebbe utilizzare il colore HSL.
- Il quarto dovrebbe utilizzare il colore RGB ma con il canale alpha impostato a `0.6`.

Puoi [convertire il colore esadecimale su convertingcolors.com](https://convertingcolors.com/hex-color-86DEFA.html). Devi capire come utilizzare i valori in CSS. Il tuo risultato finale dovrebbe apparire come l'immagine qui sotto:

![Quattro elementi di elenco. I primi tre con lo stesso colore di sfondo e l'ultimo con uno sfondo più chiaro.](mdn-value-color.png)

Prova a aggiornare il codice qui sotto per ricreare l'esempio finale:

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
<summary>Clicca qui per vedere la soluzione</summary>

Utilizzando [uno strumento di conversione del colore](https://convertingcolors.com/hex-color-86DEFA.html), dovresti essere in grado di utilizzare diverse [funzioni di colore](/it/docs/Web/CSS/color_value#syntax) per definire lo stesso colore in modi diversi:

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

## Attività 2

In questa attività, vogliamo che imposti la dimensione di vari testi, come descritto di seguito:

- L'elemento `<h1>` dovrebbe essere di 50 pixel.
- L'elemento `<h2>` dovrebbe essere di 2em.
- Tutti gli elementi `<p>` dovrebbero essere di 16 pixel.
- Un elemento `<p>` che segue direttamente un `<h1>` dovrebbe essere al 120%.

Il tuo risultato finale dovrebbe apparire come l'immagine qui sotto:

![Alcuni testi con dimensioni variabili.](mdn-value-length.png)

Prova a aggiornare il codice qui sotto per ricreare l'esempio finale:

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
<summary>Clicca qui per vedere la soluzione</summary>

Puoi utilizzare i seguenti valori di lunghezza:

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

## Attività 3

In questa attività, vogliamo che sposti l'immagine di sfondo in modo che sia centrata orizzontalmente e si trovi al 20% dalla parte superiore della casella.

Il tuo risultato finale dovrebbe apparire come l'immagine qui sotto:

![Una stat centrata orizzontalmente in una casella e a breve distanza dalla parte superiore della casella.](mdn-value-position.png)

Prova a aggiornare il codice qui sotto per ricreare l'esempio finale:

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
<summary>Clicca qui per vedere la soluzione</summary>

Utilizza `background-position` con la parola chiave `center` e una percentuale:

```css
.box {
  background-image: url(https://mdn.github.io/shared-assets/images/examples/purple-star.png);
  background-repeat: no-repeat;
  background-position: center 20%;
}
```

</details>

## Vedi anche

- [Nozioni di base sullo styling CSS](/it/docs/Learn_web_development/Core/Styling_basics)
