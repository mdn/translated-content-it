---
title: "Metti alla prova le tue competenze: Overflow"
short-title: Overflow
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Overflow
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se hai compreso [l'overflow in CSS e come gestirlo](/it/docs/Learn_web_development/Core/Styling_basics/Overflow).

> [!NOTE]
> Fai clic su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Puoi anche copiare il codice (cliccando sull'icona della clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
> Se hai difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito 1

In questo compito, il contenuto trabocca dalla scatola perché ha un'altezza fissa. Mantieni l'altezza ma fai in modo che la scatola abbia barre di scorrimento solo se c'è abbastanza testo da causare un overflow. Prova a rimuovere parte del testo dall'HTML, in modo che se c'è solo una piccola quantità di testo che non trabocca, non appaia nessuna barra di scorrimento.

![Una piccola scatola con un bordo e una barra di scorrimento verticale.](mdn-overflow1.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finale:

```html live-sample___overflow-scroll
<div class="box">
  <p>
    Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi welsh onion
    daikon amaranth tatsoi tomatillo melon azuki bean garlic.
  </p>

  <p>
    Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot courgette
    tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato.
    Dandelion cucumber earthnut pea peanut soko zucchini.
  </p>
</div>
```

```css live-sample___overflow-scroll
body {
  font: 1.2em / 1.5 sans-serif;
}

.box {
  border: 5px solid black;
  padding: 1em;
  height: 200px;
  width: 300px;
}
```

{{EmbedLiveSample("overflow-scroll", "", "450px")}}

<details>
<summary>Fai clic qui per vedere la soluzione</summary>

Dovresti aggiungere `overflow: auto` in modo che la scatola acquisisca le barre di scorrimento solo quando il contenuto è troppo grande:

```css
.box {
  overflow: auto;
}
```

</details>

## Compito 2

In questo compito, c'è un'immagine nella scatola che è più grande delle dimensioni della scatola in modo che trabocchi visibilmente. Cambia questo in modo che qualsiasi parte dell'immagine al di fuori della scatola sia nascosta.

Il tuo risultato finale dovrebbe apparire come l'immagine qui sotto:

![Una scatola con un'immagine che riempie la scatola ma non trabocca dai bordi.](mdn-overflow2.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finale:

```html live-sample___overflow-hidden
<div class="box">
  <img
    alt="flowers"
    src="https://mdn.github.io/shared-assets/images/examples/flowers.jpg" />
</div>
```

```css live-sample___overflow-hidden
body {
  font: 1.2em / 1.5 sans-serif;
}
.box {
  border: 5px solid black;
  height: 200px;
  width: 300px;
}
```

{{EmbedLiveSample("overflow-hidden", "", "300px")}}

<details>
<summary>Fai clic qui per vedere la soluzione</summary>

Dovresti aggiungere `overflow: hidden` al selettore `.box`:

```css
.box {
  overflow: hidden;
}
```

</details>

## Vedi anche

- [Concetti base di stile CSS](/it/docs/Learn_web_development/Core/Styling_basics)
