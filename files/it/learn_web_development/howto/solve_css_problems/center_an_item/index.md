---
title: Come centrare un elemento
short-title: Centrare un elemento
slug: Learn_web_development/Howto/Solve_CSS_problems/Center_an_item
l10n:
  sourceCommit: 85fccefc8066bd49af4ddafc12c77f35265c7e2d
---

In questa guida viene illustrato come centrare un elemento all'interno di un altro elemento, sia orizzontalmente sia verticalmente.

## Centrare un riquadro

Per centrare un riquadro all'interno di un altro usando CSS, è necessario utilizzare le proprietà di [allineamento dei riquadri CSS](/it/docs/Web/CSS/Guides/Box_alignment) sul contenitore padre. Poiché queste proprietà di allineamento non dispongono ancora del supporto del browser per il layout a blocchi e inline, è necessario rendere il padre un contenitore [flex](/it/docs/Web/CSS/Guides/Flexible_box_layout) o [grid](/it/docs/Web/CSS/Guides/Grid_layout) per abilitare la possibilità di utilizzare l'allineamento.

Nell'esempio seguente, al contenitore padre è stato assegnato `display: flex`; quindi {{cssxref("justify-content")}} è stato impostato su center per allinearlo orizzontalmente e {{cssxref("align-items")}} su center per allinearlo verticalmente.

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
  color: white;
}
```

{{EmbedLiveSample("center", "", "220px")}}

> [!NOTE]
> Questa tecnica può essere utilizzata per eseguire qualsiasi tipo di allineamento di uno o più elementi all'interno di un altro. Nell'esempio precedente, è possibile provare a modificare i valori con qualsiasi valore valido per {{cssxref("justify-content")}} e {{cssxref("align-items")}}.

## Vedi anche

- [Allineamento dei riquadri in flexbox](/it/docs/Web/CSS/Guides/Box_alignment/In_flexbox)
- [Allineamento dei riquadri nel layout grid](/it/docs/Web/CSS/Guides/Box_alignment/In_grid_layout)
