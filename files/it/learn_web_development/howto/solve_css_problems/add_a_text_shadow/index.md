---
title: Come aggiungere un'ombra al testo
short-title: Aggiungere un'ombra al testo
slug: Learn_web_development/Howto/Solve_CSS_problems/Add_a_text_shadow
l10n:
  sourceCommit: a862f7714755b140b150bd2c724589f6f95a19a2
---

In questa guida viene illustrato come aggiungere un'ombra a qualsiasi testo nella pagina.

## Aggiungere ombre al testo

La nostra [guida per aggiungere un'ombra ai riquadri](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Add_a_shadow) spiega come aggiungere un'ombra a qualsiasi elemento nella pagina. Tuttavia, questa tecnica aggiunge ombre soltanto al riquadro circostante dell'elemento. Per aggiungere un'ombra esterna al testo stesso, è necessaria un'altra proprietà CSS: {{cssxref("text-shadow")}}.

La proprietà `text-shadow` accetta diversi valori:

- L'offset sull'asse x
- L'offset sull'asse y
- Un raggio di sfocatura
- Un colore

Nell'esempio seguente, l'offset sull'asse x è impostato su `2px`, l'offset sull'asse y su `4px`, il raggio di sfocatura su `4px` e il colore su un blu semitrasparente. È possibile modificare i diversi valori per osservare come cambiano l'effetto dell'ombra.

```html live-sample___text-shadow
<div class="wrapper">
  <h1>Adding a shadow to text</h1>
</div>
```

```css live-sample___text-shadow
h1 {
  color: royalblue;
  text-shadow: 2px 4px 4px rgb(46 91 173 / 0.6);
}
```

{{EmbedLiveSample("Text_shadow")}}

> [!NOTE]
> Quando si aggiungono ombre al testo, il testo potrebbe diventare involontariamente difficile da leggere. Assicurarsi che le scelte offrano un [contrasto di colore](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Perceivable/Color_contrast) sufficiente per mantenere il testo leggibile.
