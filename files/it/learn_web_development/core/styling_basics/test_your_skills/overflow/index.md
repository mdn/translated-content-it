---
title: "Metti alla prova le tue competenze: Overflow"
short-title: "Test: Overflow"
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Overflow
l10n:
  sourceCommit: a623d4459e2aa00d17dc0fd6b6bc44f56c589950
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Overflow", "Learn_web_development/Core/Styling_basics/Size_decorate_content_panel", "Learn_web_development/Core/Styling_basics")}}

Lo scopo di questo test delle competenze è aiutare a valutare se si comprende [l'overflow in CSS e come gestirlo](/it/docs/Learn_web_development/Core/Styling_basics/Overflow).

> [!NOTE]
> Per ottenere aiuto, leggere la nostra guida sull'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Overflow 1

In questa attività, il contenuto fuoriesce dal box perché ha un'altezza fissa.

Per completare l'attività:

1. Aggiornare il CSS in modo che l'altezza del box venga mantenuta e le barre di scorrimento appaiano solo quando è presente abbastanza testo da causare un overflow.
2. Testare la soluzione rimuovendo parte del testo dall'HTML e verificando che non appaia alcuna barra di scorrimento quando è presente solo una piccola quantità di testo.

Il punto di partenza dell'attività ha questo aspetto:

{{EmbedLiveSample("overflow1-start", "", "450px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___overflow1-start live-sample___overflow1-finish
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

```css live-sample___overflow1-start live-sample___overflow1-finish
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

Lo stile aggiornato dovrebbe avere questo aspetto:

{{EmbedLiveSample("overflow1-finish", "", "300px")}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Aggiungere `overflow: auto` affinché il box ottenga le barre di scorrimento solo quando il contenuto è troppo grande:

```css live-sample___overflow1-finish
.box {
  overflow: auto;
}
```

</details>

## Overflow 2

In questa attività, nel box è presente un'immagine più grande delle dimensioni del box, perciò fuoriesce visibilmente. Aggiornare il CSS in modo che qualsiasi parte dell'immagine esterna al box venga nascosta.

Il punto di partenza dell'attività ha questo aspetto:

{{EmbedLiveSample("overflow2-start", "", "260px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___overflow2-start live-sample___overflow2-finish
<div class="box">
  <img
    alt="flowers"
    src="https://mdn.github.io/shared-assets/images/examples/flowers.jpg" />
</div>
```

```css live-sample___overflow2-start live-sample___overflow2-finish
body {
  font: 1.2em / 1.5 sans-serif;
}
.box {
  border: 5px solid black;
  height: 200px;
  width: 300px;
}
```

Lo stile aggiornato dovrebbe avere questo aspetto:

{{EmbedLiveSample("overflow2-finish", "", "260px")}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Aggiungere `overflow: hidden` al selettore `.box`:

```css live-sample___overflow2-finish
.box {
  overflow: hidden;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Overflow", "Learn_web_development/Core/Styling_basics/Size_decorate_content_panel", "Learn_web_development/Core/Styling_basics")}}
