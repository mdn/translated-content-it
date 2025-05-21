---
title: Come riempire un riquadro con un'immagine senza distorcerla
short-title: Riempire un riquadro con un'immagine
slug: Learn_web_development/Howto/Solve_CSS_problems/Fill_a_box_with_an_image
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questa guida puoi apprendere una tecnica per far sì che un'immagine HTML riempia completamente un riquadro.

## Utilizzare `object-fit`

Quando aggiungi un'immagine a una pagina utilizzando l'elemento HTML {{htmlelement("img")}}, l'immagine manterrà la dimensione e il {{Glossary("aspect_ratio", "rapporto d'aspetto")}} del file immagine, o quello di eventuali attributi HTML [`width`](/it/docs/Web/HTML/Reference/Elements/img#width) o [`height`](/it/docs/Web/HTML/Reference/Elements/img#height). A volte può essere necessario che l'immagine riempia completamente il riquadro in cui è stata posizionata. In tal caso, devi prima decidere cosa accade se l'immagine ha un rapporto d'aspetto non corrispondente al contenitore.

1. L'immagine dovrebbe riempire completamente il riquadro, mantenendo il rapporto d'aspetto, e ritagliando qualsiasi eccesso nel lato che è troppo grande per adattarsi.
2. L'immagine dovrebbe adattarsi all'interno del riquadro, con lo sfondo visibile come barre sul lato troppo piccolo.
3. L'immagine dovrebbe riempire il riquadro e allungarsi, il che potrebbe far sì che venga visualizzata con un rapporto d'aspetto errato.

La proprietà {{cssxref("object-fit")}} rende possibile ciascuno di questi approcci. Nell'esempio qui sotto puoi vedere come funzionano i diversi valori di `object-fit` quando usi la stessa immagine. Seleziona l'approccio che funziona meglio per il tuo design.

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
