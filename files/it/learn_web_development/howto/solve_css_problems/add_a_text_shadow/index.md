---
title: Come aggiungere un'ombra al testo
short-title: Aggiungere un'ombra al testo
slug: Learn_web_development/Howto/Solve_CSS_problems/Add_a_text_shadow
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questa guida scoprirai come aggiungere un'ombra a qualsiasi testo sulla tua pagina.

## Aggiunta di ombre al testo

Nella nostra [guida su come aggiungere un'ombra ai box](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Add_a_shadow), puoi scoprire come aggiungere un'ombra a qualsiasi elemento sulla tua pagina. Tuttavia, quella tecnica aggiunge solo ombre al contorno del box dell'elemento. Per aggiungere un'ombra all'interno del box, al testo, hai bisogno di una proprietà CSS diversa — {{cssxref("text-shadow")}}.

La proprietà `text-shadow` accetta un numero di valori:

- Lo scostamento sull'asse x
- Lo scostamento sull'asse y
- Un raggio di sfocatura
- Un colore

Nell'esempio qui sotto, abbiamo impostato lo scostamento sull'asse x a 2px, lo scostamento sull'asse y a 4px, il raggio di sfocatura a 4px e il colore a un blu semi-trasparente. Gioca con i diversi valori per vedere come modificano l'ombra.

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
> Può essere abbastanza facile rendere il testo difficile da leggere con le ombre. Assicurati che le scelte che fai rendano ancora il tuo testo leggibile e forniscano abbastanza [contrasto di colore](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Perceivable/Color_contrast) per i visitatori che hanno difficoltà con il testo a basso contrasto.
