---
title: Risolvere problemi comuni di CSS
short-title: Problemi comuni di CSS
slug: Learn_web_development/Howto/Solve_CSS_problems
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Questa pagina raccoglie domande e risposte, oltre ad altro materiale sul sito web di MDN, che possono aiutare a risolvere problemi comuni di CSS.

## Stile delle box

- [Come posso aggiungere un'ombra a un elemento?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Add_a_shadow)
  - : Le ombre possono essere aggiunte alle box con la proprietà {{cssxref("box-shadow")}}. Questo tutorial spiega come funziona e mostra un esempio.
- [Come posso riempire una box con un'immagine senza distorcere l'immagine?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Fill_a_box_with_an_image)
  - : La proprietà {{cssxref("object-fit")}} offre diversi modi per adattare un'immagine a una box con un diverso {{Glossary("aspect_ratio", "rapporto d'aspetto")}}, e in questo tutorial puoi scoprire come usarli.
- [Quali metodi possono essere usati per stilizzare le box?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Create_fancy_boxes)
  - : Una panoramica delle diverse proprietà che potrebbero essere utili quando si stilizzano le box utilizzando CSS.
- [Come posso rendere gli elementi semitrasparenti?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Make_box_transparent)
  - : La proprietà {{cssxref("opacity")}} e i valori di colore con un canale alpha possono essere utilizzati per questo; scopri quale usare e quando.

### Lezioni e guide sullo stile delle box

- [Il box model](/it/docs/Learn_web_development/Core/Styling_basics/Box_model)
- [Stilizzare sfondi e bordi](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders)

## CSS e testo

- [Come posso aggiungere un'ombra al testo?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Add_a_text_shadow)
  - : Le ombre possono essere aggiunte al testo con la proprietà {{cssxref("text-shadow")}}. Questo tutorial spiega come funziona e mostra un esempio.
- [Come posso evidenziare la prima riga di un paragrafo?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Highlight_first_line)
  - : Scopri come targetizzare la prima riga di testo in un paragrafo con il pseudo-elemento {{cssxref("::first-line")}}.
- [Come posso evidenziare il primo paragrafo in un articolo?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Highlight_first_para)
  - : Scopri come targetizzare il primo paragrafo con la pseudo-classe {{cssxref(":first-child")}}.
- [Come posso evidenziare un paragrafo solo se viene immediatamente dopo un'intestazione?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Highlight_para_after_h1)
  - : I combinatori possono aiutare a targetizzare con precisione gli elementi in base alla loro posizione nel documento; questo tutorial spiega come usarli per applicare CSS a un paragrafo solo se segue immediatamente un'intestazione.

### Lezioni e guide sullo stile del testo

- [Come stilizzare il testo](/it/docs/Learn_web_development/Core/Text_styling/Fundamentals)
- [Come personalizzare un elenco di elementi](/it/docs/Learn_web_development/Core/Text_styling/Styling_lists)
- [Come stilizzare i link](/it/docs/Learn_web_development/Core/Text_styling/Styling_links)
- [Selettori CSS](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors)

## Layout CSS

- [Come posso centrare un elemento?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Center_an_item)
  - : Centrare un elemento all'interno di un'altra box orizzontalmente e verticalmente era solitamente complicato, tuttavia flexbox lo rende ora semplice.

### Guide sul layout

- [Usare CSS flexbox](/it/docs/Web/CSS/CSS_flexible_box_layout/Basic_concepts_of_flexbox)
- [Usare layout multicolonna CSS](/it/docs/Web/CSS/CSS_multicol_layout/Using_multicol_layouts)
- [Usare CSS grid layout](/it/docs/Web/CSS/CSS_grid_layout/Basic_concepts_of_grid_layout)
- [Usare contenuto generato da CSS](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Generated_content)

> [!NOTE]
> Abbiamo un libro di cucina dedicato alle [soluzioni di layout CSS](/it/docs/Web/CSS/Layout_cookbook), con esempi completamente funzionanti e spiegazioni di compiti comuni di layout. Dai un'occhiata anche a [Esempi pratici di posizionamento](/it/docs/Learn_web_development/Core/CSS_layout/Practical_positioning_examples), che mostra come utilizzare il posizionamento per creare una scheda informativa a tab e un pannello nascosto scorrevole.
