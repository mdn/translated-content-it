---
title: Come evidenziare il primo paragrafo
short-title: Evidenziare il primo paragrafo
slug: Learn_web_development/Howto/Solve_CSS_problems/Highlight_first_para
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questa guida puoi scoprire come evidenziare il primo paragrafo all'interno di un contenitore.

## Stilizzare il primo paragrafo

Vorresti rendere il primo paragrafo più grande e in grassetto. Potresti aggiungere una classe al primo paragrafo e selezionarlo in quel modo, tuttavia, usare un selettore pseudo-classe è più flessibile — significa che puoi puntare il paragrafo in base alla sua posizione nel documento, e non dovrai spostare manualmente la classe se l'ordine sorgente cambia.

## Usare una pseudo-classe

Una {{cssxref("pseudo-classes","pseudo-classe")}} agisce come se avessi applicato una classe; tuttavia, anziché usare un selettore di classe, CSS seleziona in base alla struttura del documento. Ci sono diverse pseudo-classi che possono selezionare cose differenti. Nel nostro caso useremo {{cssxref(":first-child")}}. Questo selezionerà l'elemento che è il primo figlio di un genitore.

```html live-sample___highlight_first_para
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

```css live-sample___highlight_first_para
.wrapper p:first-child {
  font-weight: bold;
  font-size: 130%;
}
```

{{EmbedLiveSample("highlight_first_para")}}

Puoi provare a cambiare {{cssxref(":first-child")}} con {{cssxref(":last-child")}} nel live example sopra, e selezionerai l'ultimo paragrafo.

Ogni volta che hai bisogno di puntare qualcosa nel tuo documento, puoi verificare se una delle {{cssxref("pseudo-classes")}} disponibili può farlo per te.

## Vedi anche

- La pagina di riferimento delle {{cssxref("pseudo-classes")}}.
- [Impara CSS: Pseudo-classi e pseudo-elementi.](/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements)
