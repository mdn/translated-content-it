---
title: Come aggiungere un'ombra al testo
short-title: Aggiungere un'ombra al testo
slug: Learn_web_development/Howto/Solve_CSS_problems/Add_a_text_shadow
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questa guida può scoprire come aggiungere un'ombra a qualsiasi testo della sua pagina.

## Aggiungere ombre al testo

Nella nostra [guida su come aggiungere un'ombra alle scatole](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Add_a_shadow), può scoprire come aggiungere un'ombra a qualsiasi elemento della sua pagina. Tuttavia, quella tecnica aggiunge solo ombre alla casella circostante dell'elemento. Per aggiungere un'ombra al testo all'interno della casella, ha bisogno di una proprietà CSS diversa — {{cssxref("text-shadow")}}.

La proprietà `text-shadow` accetta diversi valori:

- L'offset sull'asse x
- L'offset sull'asse y
- Un raggio di sfocatura
- Un colore

Nell'esempio sotto, abbiamo impostato l'offset sull'asse x a 2px, l'offset sull'asse y a 4px, il raggio di sfocatura a 4px e il colore a un blu semi-trasparente. Sperimenta con i diversi valori per vedere come cambiano l'ombra.

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

{{EmbedLiveSample("text-shadow")}}

> [!NOTE]
> È abbastanza facile rendere il testo difficile da leggere con le ombre. Si assicuri che le scelte fatte rendano comunque il testo leggibile e forniscano un sufficiente [contrasto di colore](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Perceivable/Color_contrast) per i visitatori che hanno difficoltà con il testo a basso contrasto.
