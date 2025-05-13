---
title: Come centrare un elemento
short-title: Centrare un elemento
slug: Learn_web_development/Howto/Solve_CSS_problems/Center_an_item
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questa guida può scoprire come centrare un elemento all'interno di un altro elemento, sia orizzontalmente che verticalmente.

## Centrare un riquadro

Per centrare un riquadro all'interno di un altro utilizzando CSS, sarà necessario usare le proprietà di [allineamento del riquadro CSS](/it/docs/Web/CSS/CSS_box_alignment) sul contenitore padre. Poiché queste proprietà di allineamento non hanno ancora il supporto del browser per il layout a blocco e inline, sarà necessario rendere il contenitore un contenitore [flex](/it/docs/Web/CSS/CSS_flexible_box_layout) o [grid](/it/docs/Web/CSS/CSS_grid_layout) per poter utilizzare l'allineamento.

Nell'esempio qui sotto, abbiamo dato al contenitore padre `display: flex`; poi impostato {{cssxref("justify-content")}} su center per allinearlo orizzontalmente, e {{cssxref("align-items")}} su center per allinearlo verticalmente.

```html live-sample___center
<div class="wrapper">
  <div class="box">center me!</div>
</div>
```

```css live-sample___center
.wrapper {
  height: 200px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.box {
  background-color: rgb(69 164 181);
  border-radius: 5px;
  padding: 10px;
  color: #fff;
}
```

{{EmbedLiveSample("center", "", "220px")}}

> [!NOTE]
> Può utilizzare questa tecnica per fare qualsiasi tipo di allineamento di uno o più elementi all'interno di un altro. Nell'esempio sopra può provare a cambiare i valori con qualsiasi valore valido per {{cssxref("justify-content")}} e {{cssxref("align-items")}}.

## Vedere anche

- [Allineamento del riquadro in flexbox](/it/docs/Web/CSS/CSS_box_alignment/Box_alignment_in_flexbox)
- [Allineamento del riquadro nel layout a griglia](/it/docs/Web/CSS/CSS_box_alignment/Box_alignment_in_grid_layout)
