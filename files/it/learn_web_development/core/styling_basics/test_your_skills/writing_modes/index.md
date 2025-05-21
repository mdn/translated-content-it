---
title: "Metti alla prova le tue capacità: Modalità di scrittura e proprietà logiche"
short-title: Modalità di scrittura
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Writing_modes
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Lo scopo di questo test di abilità è valutare se comprendi come [gestire diverse direzioni del testo usando modalità di scrittura e proprietà logiche in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Handling_different_text_directions).

> [!NOTE]
> Clicca su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Puoi anche copiare il codice (clicca sull'icona della clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se rimani bloccato, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Attività 1

In questa attività, la scatola viene mostrata con una modalità di scrittura orizzontale. Puoi aggiungere una linea di CSS per cambiarla in modo che utilizzi una modalità di scrittura verticale con testo da destra a sinistra?

Il tuo risultato finale dovrebbe essere simile all'immagine qui sotto:

![Una scatola con una modalità di scrittura verticale](mdn-writing-modes1.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finito:

```html live-sample___writing-mode
<div class="box">Turn me on my side.</div>
```

```css hidden live-sample___writing-mode
body {
  font: 1.2em / 1.5 sans-serif;
}
```

```css live-sample___writing-mode
.box {
  border: 5px solid rebeccapurple;
  background-color: lightgray;
  padding: 40px;
  margin: 40px;
}
```

{{EmbedLiveSample("writing-mode", "", "250px")}}

<details>
<summary>Clicca qui per mostrare la soluzione</summary>

Dovresti usare la proprietà `writing-mode` con un valore di `vertical-rl` per uno script verticale da destra a sinistra:

```css
.box {
  border: 5px solid rebeccapurple;
  background-color: lightgray;
  padding: 40px;
  margin: 40px;
  writing-mode: vertical-rl;
}
```

</details>

## Attività 2

In questa attività, vogliamo che utilizzi le proprietà logiche per sostituire `width` e `height` al fine di mantenere il {{Glossary("aspect_ratio", "rapporto d'aspetto")}} della scatola mentre viene ruotata verticalmente.

Il tuo risultato finale dovrebbe essere simile all'immagine qui sotto:

![Due scatole una orizzontale l'altra verticale](mdn-writing-modes2.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finito:

```html live-sample___logical-width-height
<div class="box">Horizontal.</div>
<div class="box vertical">Vertical.</div>
```

```css hidden live-sample___logical-width-height
body {
  font: 1.2em / 1.5 sans-serif;
}
```

```css live-sample___logical-width-height
.box {
  border: 5px solid rebeccapurple;
  background-color: lightgray;
  padding: 40px;
  margin: 40px;
  width: 200px;
  height: 100px;
}
```

{{EmbedLiveSample("logical-width-height", "", "500px")}}

<details>
<summary>Clicca qui per mostrare la soluzione</summary>

Oltre a impostare `writing-mode: vertical-rl` sulla scatola `.vertical`, devi applicare le proprietà `inline-size` e `block-size` per sostituire `width` e `height`:

```css
.box {
  border: 5px solid rebeccapurple;
  background-color: lightgray;
  padding: 40px;
  margin: 40px;
  inline-size: 200px;
  block-size: 100px;
}
.vertical {
  writing-mode: vertical-rl;
}
```

</details>

## Attività 3

In questa attività, vogliamo che utilizzi versioni logiche delle proprietà di margine, bordo e padding in modo che i bordi della scatola si riferiscano al testo anziché seguire alto, sinistra, basso e destra.

Il tuo risultato finale dovrebbe essere simile all'immagine qui sotto:

![Due scatole una orizzontale e una verticale con margine, bordo e padding diversi](mdn-writing-modes3.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finito:

```html live-sample___logical-mbp
<div class="box">Horizontal.</div>
<div class="box vertical">Vertical.</div>
```

```css hidden live-sample___logical-mbp
body {
  font: 1.2em / 1.5 sans-serif;
}
```

```css hidden live-sample___logical-mbp
.vertical {
  writing-mode: vertical-rl;
}
```

```css live-sample___logical-mbp
.box {
  width: 150px;
  height: 150px;
  border-top: 5px solid rebeccapurple;
  border-right: 5px solid grey;
  border-bottom: 5px dotted red;
  border-left: 5px dotted blue;
  padding-top: 40px;
  margin-bottom: 30px;
}
```

{{EmbedLiveSample("logical-mbp", "", "500px")}}

<details>
<summary>Clicca qui per mostrare la soluzione</summary>

Per risolvere questo, è necessario comprendere le mappature logiche, relative al flusso per le proprietà fisiche di margine, bordo e padding:

```css
.box {
  width: 150px;
  height: 150px;
  border-block-start: 5px solid rebeccapurple;
  border-inline-end: 5px solid grey;
  border-block-end: 5px dotted red;
  border-inline-start: 5px dotted blue;
  padding-block-start: 40px;
  margin-block-end: 30px;
}
```

</details>

## Vedi anche

- [Basi dello styling CSS](/it/docs/Learn_web_development/Core/Styling_basics)
