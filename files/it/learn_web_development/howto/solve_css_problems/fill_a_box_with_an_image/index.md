---
title: Come riempire un box con un'immagine senza distorcerla
short-title: Riempire un box con un'immagine
slug: Learn_web_development/Howto/Solve_CSS_problems/Fill_a_box_with_an_image
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questa guida può apprendere una tecnica per fare in modo che un'immagine HTML riempia completamente un box.

## Utilizzo di object-fit

Quando si aggiunge un'immagine a una pagina utilizzando l'elemento HTML {{htmlelement("img")}}, l'immagine manterrà la dimensione e il {{Glossary("aspect_ratio", "rapporto d'aspetto")}} del file immagine, o quello di eventuali attributi HTML [`width`](/it/docs/Web/HTML/Reference/Elements/img#width) o [`height`](/it/docs/Web/HTML/Reference/Elements/img#height). A volte si desidera che l'immagine riempia completamente il box in cui è stata inserita. In tal caso, è necessario innanzitutto decidere cosa succede se l'immagine ha un rapporto d'aspetto sbagliato per il contenitore.

1. L'immagine dovrebbe riempire completamente il box, mantenendo il rapporto d'aspetto e ritagliando l'eccesso sul lato troppo grande per adattarsi.
2. L'immagine dovrebbe adattarsi all'interno del box, con lo sfondo visibile come barre sul lato troppo piccolo.
3. L'immagine dovrebbe riempire il box e allungarsi, il che potrebbe significare che viene visualizzata con un rapporto d'aspetto errato.

La proprietà {{cssxref("object-fit")}} rende possibile ciascuno di questi approcci. Nell'esempio qui sotto può vedere come diversi valori di `object-fit` funzionano quando si utilizza la stessa immagine. Selezioni l'approccio che funziona meglio per il suo design.

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
  border: 5px solid #000;
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
