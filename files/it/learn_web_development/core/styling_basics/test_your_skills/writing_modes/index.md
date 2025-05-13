---
title: "Metti alla prova le tue abilità: Modalità di scrittura e proprietà logiche"
short-title: Modalità di scrittura
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Writing_modes
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se ha compreso come [gestire diverse direzioni del testo utilizzando modalità di scrittura e proprietà logiche in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Handling_different_text_directions).

> [!NOTE]
> Clicchi su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Può anche copiare il codice (clicchi sull'icona degli appunti) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se riscontra difficoltà, può contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito 1

In questo compito, la casella è visualizzata con una modalità di scrittura orizzontale. Può aggiungere una riga di CSS per cambiarla in modo che utilizzi una modalità di scrittura verticale con testo da destra a sinistra?

Il suo risultato finale dovrebbe assomigliare all'immagine qui sotto:

![Una casella con una modalità di scrittura verticale](mdn-writing-modes1.png)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finale:

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
<summary>Clicchi qui per mostrare la soluzione</summary>

Dovrebbe usare la proprietà `writing-mode` con un valore di `vertical-rl` per uno script verticale da destra a sinistra:

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

## Compito 2

In questo compito, desideriamo che usi proprietà logiche per sostituire `width` e `height` al fine di mantenere il {{Glossary("aspect_ratio", "rapporto d'aspetto")}} della casella quando viene ruotata verticalmente.

Il suo risultato finale dovrebbe assomigliare all'immagine qui sotto:

![Due caselle, una orizzontale e l'altra verticale](mdn-writing-modes2.png)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finale:

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
<summary>Clicchi qui per mostrare la soluzione</summary>

Oltre a impostare `writing-mode: vertical-rl` sulla casella `.vertical`, deve applicare le proprietà `inline-size` e `block-size` per sostituire `width` e `height`:

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

## Compito 3

In questo compito, desideriamo che utilizzi versioni logiche delle proprietà di margine, bordo e imbottitura in modo che i bordi della casella siano relativi al testo piuttosto che seguire alto, sinistra, basso e destra.

Il suo risultato finale dovrebbe assomigliare all'immagine qui sotto:

![Due caselle, una orizzontale e una verticale con differenti margini, bordi e imbottiture](mdn-writing-modes3.png)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finale:

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
<summary>Clicchi qui per mostrare la soluzione</summary>

Per risolvere questo, è necessario comprendere le mappature relative al flusso logico per le proprietà fisiche di margine, bordo e imbottitura:

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

## Vedere anche

- [Basi dello styling CSS](/it/docs/Learn_web_development/Core/Styling_basics)
