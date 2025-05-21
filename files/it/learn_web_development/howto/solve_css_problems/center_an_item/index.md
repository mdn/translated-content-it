---
title: Come centrare un elemento
short-title: Centrare un elemento
slug: Learn_web_development/Howto/Solve_CSS_problems/Center_an_item
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questa guida puoi scoprire come centrare un elemento all'interno di un altro elemento, sia orizzontalmente che verticalmente.

## Centrare una scatola

Per centrare una scatola all'interno di un'altra utilizzando CSS, è necessario utilizzare le proprietà di [allineamento del box CSS](/it/docs/Web/CSS/CSS_box_alignment) nel contenitore genitore. Poiché queste proprietà di allineamento non hanno ancora supporto nei browser per layout a blocchi e in linea, dovrai trasformare il genitore in un contenitore [flex](/it/docs/Web/CSS/CSS_flexible_box_layout) o [grid](/it/docs/Web/CSS/CSS_grid_layout) per attivare la capacità di utilizzare l'allineamento.

Nell'esempio seguente abbiamo dato al contenitore genitore `display: flex`; poi abbiamo impostato {{cssxref("justify-content")}} su center per allinearlo orizzontalmente, e {{cssxref("align-items")}} su center per allinearlo verticalmente.

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
> Puoi utilizzare questa tecnica per qualsiasi tipo di allineamento di uno o più elementi all'interno di un altro. Nell'esempio sopra puoi provare a cambiare i valori con qualsiasi valore valido per {{cssxref("justify-content")}} e {{cssxref("align-items")}}.

## Vedi anche

- [Allineamento del box in flexbox](/it/docs/Web/CSS/CSS_box_alignment/Box_alignment_in_flexbox)
- [Allineamento del box nei layout a griglia](/it/docs/Web/CSS/CSS_box_alignment/Box_alignment_in_grid_layout)
