---
title: "Metti alla prova le tue competenze: valori e unità"
short-title: "Test: valori e unità"
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Values
l10n:
  sourceCommit: 8c253b4a3c79adb13bc6a70427cc38f438b6a809
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Values_and_units", "Learn_web_development/Core/Styling_basics/Sizing", "Learn_web_development/Core/Styling_basics")}}

L'obiettivo di questo test sulle competenze è aiutare a valutare se sono stati compresi i diversi tipi di [valori e unità utilizzati nelle proprietà CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units).

> [!NOTE]
> Per ricevere aiuto, leggere la nostra guida all'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci utilizzando uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Valori e unità 1

In questa attività, al primo elemento dell'elenco è stato assegnato un colore di sfondo usando un codice colore esadecimale. Completare il CSS utilizzando lo stesso colore in formati diversi, più un elemento finale dell'elenco in cui rendere lo sfondo semiopaco.

- Il secondo elemento dell'elenco deve utilizzare il colore RGB.
- Il terzo deve utilizzare il colore HSL.
- Il quarto deve utilizzare il colore RGB, ma con il canale alpha impostato su `0.6`.

È possibile convertire il colore esadecimale usando [convertingcolors.com](https://convertingcolors.com/hex-color-86DEFA.html). Occorre capire come utilizzare i valori in CSS.

Il punto di partenza dell'attività è simile a questo:

{{EmbedLiveSample("values1-start", "", "300px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___values1-start live-sample___values1-finish
<ul>
  <li class="hex">hex color</li>
  <li class="rgb">RGB color</li>
  <li class="hsl">HSL color</li>
  <li class="transparency">Alpha value 0.6</li>
</ul>
```

```css live-sample___values1-start live-sample___values1-finish
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

.hex {
  background-color: #86defa;
}

/* Add styles here */
```

Lo stile aggiornato dovrebbe apparire così:

{{EmbedLiveSample("values1-finish", "", "300px")}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Utilizzando [uno strumento di conversione dei colori](https://convertingcolors.com/hex-color-86DEFA.html), dovrebbero essere disponibili gli strumenti necessari per usare diverse [funzioni di colore](/it/docs/Web/CSS/Reference/Values/color_value#syntax) per definire lo stesso colore in modi diversi:

```css live-sample___values1-finish
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

## Valori e unità 2

In questa attività, impostare la dimensione del carattere di vari elementi di testo:

- L'elemento `<h1>` deve essere `50px`.
- L'elemento `<h2>` deve essere `2em`.
- Tutti gli elementi `<p>` devono essere `16px`.
- Un elemento `<p>` che si trova direttamente dopo un `<h1>` deve essere `120%`.

Il punto di partenza dell'attività è simile a questo:

{{EmbedLiveSample("values2-start", "", "420px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___values2-start live-sample___values2-finish
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

```css live-sample___values2-start live-sample___values2-finish
body {
  font: 1.2em / 1.5 sans-serif;
}

h1 {
  /* Add styles here */
}

h2 {
  /* Add styles here */
}

p {
  /* Add styles here */
}

h1 + p {
  /* Add styles here */
}
```

Lo stile aggiornato dovrebbe apparire così:

{{EmbedLiveSample("values2-finish", "", "430px")}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

È possibile utilizzare i seguenti valori di lunghezza:

```css live-sample___values2-finish
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

## Valori e unità 3

Per completare l'attività, aggiornare il CSS per spostare l'immagine di sfondo in modo che sia centrata orizzontalmente e si trovi al `20%` dalla parte superiore del riquadro.

Il punto di partenza dell'attività è simile a questo:

{{EmbedLiveSample("values3-start", "", "400px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___values3-start live-sample___values3-finish
<div class="box"></div>
```

```css live-sample___values3-start live-sample___values3-finish
.box {
  border: 5px solid black;
  height: 350px;
}

.box {
  background-image: url("https://mdn.github.io/shared-assets/images/examples/purple-star.png");
  background-repeat: no-repeat;
}
```

Lo stile aggiornato dovrebbe apparire così:

{{EmbedLiveSample("values3-finish", "", "400px")}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Usare `background-position` con la parola chiave `center` e una percentuale:

```css live-sample___values3-finish
.box {
  background-image: url("https://mdn.github.io/shared-assets/images/examples/purple-star.png");
  background-repeat: no-repeat;
  background-position: center 20%;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Values_and_units", "Learn_web_development/Core/Styling_basics/Sizing", "Learn_web_development/Core/Styling_basics")}}
