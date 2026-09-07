---
title: Come evidenziare un paragrafo che segue un'intestazione
short-title: Evidenziare un paragrafo dopo un'intestazione
slug: Learn_web_development/Howto/Solve_CSS_problems/Highlight_para_after_h1
l10n:
  sourceCommit: 85fccefc8066bd49af4ddafc12c77f35265c7e2d
---

In questa guida viene illustrato come evidenziare un paragrafo che si trova direttamente dopo un'intestazione.

## Applicare stili al primo paragrafo dopo un'intestazione

Un pattern comune consiste nell'applicare uno stile diverso al primo paragrafo di un articolo rispetto a quelli successivi. Solitamente questo primo paragrafo si trova subito dopo un'intestazione e, se questo è il caso nel design, è possibile usare quella combinazione di elementi per selezionare il paragrafo.

## Il combinatore del fratello successivo

CSS dispone di un gruppo di [selettori CSS](/it/docs/Web/CSS/Guides/Selectors) definiti **combinatori**, poiché selezionano elementi in base a una combinazione di selettori. In questo caso verrà usato il [combinatore del fratello successivo](/it/docs/Web/CSS/Reference/Selectors/Next-sibling_combinator). Questo combinatore seleziona un elemento in base al fatto che sia adiacente a un altro elemento. Nell'HTML è presente un {{htmlelement("Heading_Elements", "h1")}} seguito da un {{htmlelement("p")}}. Il `<p>` è il fratello successivo del `<h1>`, quindi è possibile selezionarlo con `h1 + p`.

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

## Vedi anche

- [Imparare CSS: Selettori di base](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors)
- [Imparare CSS: Combinatori](/it/docs/Learn_web_development/Core/Styling_basics/Combinators)
