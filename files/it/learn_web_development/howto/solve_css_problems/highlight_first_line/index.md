---
title: Come evidenziare la prima riga di un paragrafo
short-title: Evidenziare la prima riga di un paragrafo
slug: Learn_web_development/Howto/Solve_CSS_problems/Highlight_first_line
l10n:
  sourceCommit: 85fccefc8066bd49af4ddafc12c77f35265c7e2d
---

In questa guida viene illustrato come evidenziare la prima riga di testo in un paragrafo, anche se non si conosce la lunghezza della riga.

## Applicare stili alla prima riga di testo

Si desidera rendere la prima riga di un paragrafo più grande e in grassetto. Racchiudere la prima riga in uno `<span>` consente di applicarle qualsiasi stile; tuttavia, se la prima riga diventa più corta a causa di una dimensione della viewport ridotta, il testo stilizzato andrà a capo sulla riga successiva.

## Utilizzare uno pseudo-elemento

Uno {{cssxref("pseudo-elements", "pseudo-element")}} può sostituire lo `<span>`; tuttavia, è più flessibile — il contenuto esatto selezionato da uno pseudo-elemento viene calcolato dopo che il browser ha renderizzato il contenuto, quindi funzionerà anche se la dimensione della viewport cambia.

In questo caso è necessario utilizzare lo pseudo-elemento {{cssxref("::first-line")}}. Seleziona la prima riga formattata di ciascun paragrafo, consentendo di applicarle gli stili necessari.

```html live-sample___highlight_first_line
<div class="wrapper">
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

```css live-sample___highlight_first_line
.wrapper p::first-line {
  font-weight: bold;
  font-size: 130%;
}
```

{{EmbedLiveSample("highlight_first_line")}}

> [!NOTE]
> Tutti gli pseudo-elementi agiscono in questo modo. Si comportano come se fosse stato inserito un elemento nel documento, ma lo fanno dinamicamente in base a come il contenuto viene visualizzato in fase di esecuzione.

## Combinare pseudo-elementi con altri selettori

Nell'esempio precedente, lo pseudo-elemento seleziona la prima riga di ogni paragrafo. Per selezionare solo la prima riga del primo paragrafo, è possibile combinarlo con un altro selettore. In questo caso viene utilizzata la {{cssxref(":first-child")}} {{cssxref("pseudo-classes", "pseudo-class")}}. Questo consente di selezionare la prima riga del primo elemento figlio di `.wrapper`, se tale primo elemento figlio è un paragrafo.

```html live-sample___highlight_first_line2
<div class="wrapper">
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

```css live-sample___highlight_first_line2
.wrapper p:first-child::first-line {
  font-weight: bold;
  font-size: 130%;
}
```

{{EmbedLiveSample("highlight_first_line2")}}

> [!NOTE]
> Quando si combinano pseudo-elementi con altri selettori in un selettore [complesso](/it/docs/Web/CSS/Guides/Selectors/Selector_structure#complex_selector) o [composto](/it/docs/Web/CSS/Guides/Selectors/Selector_structure#compound_selector), gli pseudo-elementi devono comparire dopo tutti gli altri componenti nel selettore in cui sono presenti.

## Vedere anche

- La pagina di riferimento sugli {{cssxref("pseudo-elements", "pseudo-elements")}}.
- [Imparare CSS: pseudo-classi e pseudo-elementi](/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements).
