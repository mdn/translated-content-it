---
title: Come riempire una casella con un'immagine senza distorcerla
short-title: Riempire una casella con un'immagine
slug: Learn_web_development/Howto/Solve_CSS_problems/Fill_a_box_with_an_image
l10n:
  sourceCommit: 451c6b58988664128473a881871707c5ec9737f2
---

In questa guida è possibile apprendere una tecnica per fare in modo che un'immagine HTML riempia completamente una casella.

## Uso di object-fit

Quando si aggiunge un'immagine a una pagina usando l'elemento HTML {{htmlelement("img")}}, l'immagine manterrà le dimensioni e le {{Glossary("aspect_ratio", "proporzioni")}} del file immagine, oppure quelle specificate dagli attributi HTML [`width`](/it/docs/Web/HTML/Reference/Elements/img#width) o [`height`](/it/docs/Web/HTML/Reference/Elements/img#height). Talvolta si desidera che l'immagine riempia completamente la casella in cui è stata inserita. In questo caso, occorre prima decidere cosa accade se l'immagine ha proporzioni errate per il contenitore.

1. L'immagine deve riempire completamente la casella, mantenendo le proporzioni e ritagliando l'eccesso sul lato troppo grande per adattarsi.
2. L'immagine deve adattarsi all'interno della casella, con lo sfondo visibile come bande sul lato troppo piccolo.
3. L'immagine deve riempire la casella e allungarsi, il che può significare che venga visualizzata con proporzioni errate.

La proprietà {{cssxref("object-fit")}} rende possibile ciascuno di questi approcci. Nell'esempio seguente è possibile vedere come funzionano diversi valori di `object-fit` usando la stessa immagine. Selezionare l'approccio più adatto al proprio design.

```html live-sample___object-fit
<div class="wrapper">
  <div class="box box1">
    <img
      alt="a colorful hot air balloon against a clear sky"
      src="https://mdn.github.io/shared-assets/images/examples/balloon.jpg" />
  </div>
  <div class="box box2">
    <img
      alt="a colorful hot air balloon against a clear sky"
      src="https://mdn.github.io/shared-assets/images/examples/balloon.jpg" />
  </div>
  <div class="box box3">
    <img
      alt="a colorful hot air balloon against a clear sky"
      src="https://mdn.github.io/shared-assets/images/examples/balloon.jpg" />
  </div>
</div>
```

```css live-sample___object-fit
.wrapper {
  height: 200px;
  display: flex;
  gap: 20px;
}

.box {
  border: 5px solid black;
}

.box img {
  width: 100%;
  height: 100%;
}

.box1 img {
  object-fit: cover;
}

.box2 img {
  object-fit: contain;
}

.box3 img {
  object-fit: fill;
}
```

{{EmbedLiveSample("object-fit", "", "220px")}}
