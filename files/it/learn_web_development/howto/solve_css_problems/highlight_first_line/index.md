---
title: Come evidenziare la prima riga di un paragrafo
short-title: Evidenziare la prima riga di un paragrafo
slug: Learn_web_development/Howto/Solve_CSS_problems/Highlight_first_line
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questa guida scoprirai come evidenziare la prima riga di testo in un paragrafo, anche se non sai quanto sarà lunga quella riga.

## Stilizzare la prima riga di testo

Potresti voler rendere la prima riga di un paragrafo più grande e in grassetto. Inserire un `<span>` attorno alla prima riga significa che puoi stilizzarla; tuttavia, se la prima riga diventa più corta a causa di una dimensione del viewport inferiore, il testo stilizzato si sposterà sulla riga successiva.

## Utilizzare un pseudo-elemento

Un {{cssxref("pseudo-elements", "pseudo-elemento")}} può sostituire il `<span>`; tuttavia, è più flessibile — il contenuto esatto selezionato da un pseudo-elemento viene calcolato una volta che il browser ha eseguito il rendering del contenuto, quindi funzionerà anche se la dimensione del viewport cambia.

In questo caso, dobbiamo utilizzare il pseudo-elemento {{cssxref("::first-line")}}. Seleziona la prima riga formattata di ciascun paragrafo, il che significa che puoi stilizzarla come necessario.

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
> Tutti i pseudo-elementi agiscono in questo modo. Si comportano come se avessi inserito un elemento nel documento, ma lo fanno in modo dinamico in base al contenuto visualizzato a runtime.

## Combinare pseudo-elementi con altri selettori

Nell'esempio sopra, il pseudo-elemento seleziona la prima riga di ogni paragrafo. Per selezionare solo la prima riga del primo paragrafo, puoi combinarlo con un altro selettore. In questo caso, utilizziamo la {{cssxref(":first-child")}} {{cssxref("pseudo-classes", "pseudo-classe")}}. Questo ci permette di selezionare la prima riga del primo figlio di `.wrapper` se quel primo figlio è un paragrafo.

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
> Quando si combinano pseudo-elementi con altri selettori in un [complesso](/it/docs/Web/CSS/CSS_selectors/Selector_structure#complex_selector) o [composto](/it/docs/Web/CSS/CSS_selectors/Selector_structure#compound_selector) selettore, i pseudo-elementi devono apparire dopo tutti gli altri componenti nel selettore in cui appaiono.

## Vedi anche

- La pagina di riferimento sui {{cssxref("pseudo-elements", "pseudo-elementi")}}.
- [Learn CSS: Pseudo-classi e pseudo-elementi](/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements).
