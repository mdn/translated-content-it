---
title: Come evidenziare un paragrafo che segue un'intestazione
short-title: Evidenziare un paragrafo dopo un'intestazione
slug: Learn_web_development/Howto/Solve_CSS_problems/Highlight_para_after_h1
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questa guida può scoprire come evidenziare un paragrafo che viene direttamente dopo un'intestazione.

## Stilizzare il primo paragrafo dopo un'intestazione

Un modello comune è stilizzare il primo paragrafo in un articolo in modo diverso rispetto a quelli che lo seguono. Solitamente, questo primo paragrafo viene subito dopo un'intestazione e, se questo è il caso nel suo design, può usare quella combinazione di elementi per mirare al paragrafo.

## Il combinatore di adiacenza

CSS ha un gruppo di [selettori CSS](/it/docs/Web/CSS/CSS_selectors) che sono detti **combinatori**, perché selezionano elementi basandosi su una combinazione di selettori. Nel nostro caso, utilizzeremo il [combinatore di adiacenza](/it/docs/Web/CSS/Next-sibling_combinator). Questo combinatore seleziona un elemento basato sulla sua prossimità ad un altro elemento. Nel nostro HTML abbiamo un {{htmlelement("Heading_Elements", "h1")}} seguito da un {{htmlelement("p")}}. Il `<p>` è l'elemento fratello successivo di `<h1>`, quindi possiamo selezionarlo con `h1 + p`.

```html live-sample___highlight_h1_plus_para
<div class="wrapper">
  <h1>A heading</h1>
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

```css live-sample___highlight_h1_plus_para
.wrapper h1 + p {
  font-weight: bold;
  font-size: 130%;
  color: rebeccapurple;
}
```

{{EmbedLiveSample("highlight_h1_plus_para", "", "220px")}}

## Vedere anche

- [Imparare CSS: Selettori di base](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors)
- [Imparare CSS: Combinatori](/it/docs/Learn_web_development/Core/Styling_basics/Combinators)
