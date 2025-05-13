---
title: "Metta alla prova le sue abilità: Overflow"
short-title: Overflow
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Overflow
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se comprende [overflow in CSS e come gestirlo](/it/docs/Learn_web_development/Core/Styling_basics/Overflow).

> [!NOTE]
> Clicchi su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Può anche copiare il codice (cliccando sull'icona della clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se si trova in difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito 1

In questo compito, il contenuto sta strabordando dal box perché ha un'altezza fissa. Mantenga l'altezza ma faccia in modo che il box abbia le barre di scorrimento solo se c'è abbastanza testo da causare un overflow. Provi a rimuovere un po' di testo dall'HTML, verificando che se c'è solo una piccola quantità di testo che non straborda, non appaia la barra di scorrimento.

![Un piccolo box con un bordo e una barra di scorrimento verticale.](mdn-overflow1.png)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finale:

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
<summary>Clicchi qui per mostrare la soluzione</summary>

Dovrebbe aggiungere `overflow: auto` in modo che il box acquisisca le barre di scorrimento solo quando il contenuto è troppo grande:

```css
.box {
  overflow: auto;
}
```

</details>

## Compito 2

In questo compito, c'è un'immagine nel box che è più grande delle dimensioni del box stesso, quindi straborda visibilmente. Modifichi il codice in modo che qualsiasi immagine al di fuori del box venga nascosta.

Il suo risultato finale dovrebbe assomigliare all'immagine sottostante:

![Un box con un'immagine che riempie il box ma non fuoriesce dai bordi.](mdn-overflow2.png)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finale:

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
<summary>Clicchi qui per mostrare la soluzione</summary>

Dovrebbe aggiungere `overflow: hidden` al selettore `.box`:

```css
.box {
  overflow: hidden;
}
```

</details>

## Vedere anche

- [Nozioni di base sullo stile CSS](/it/docs/Learn_web_development/Core/Styling_basics)
