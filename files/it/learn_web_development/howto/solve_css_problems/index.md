---
title: Risolvere problemi comuni di CSS
short-title: Problemi comuni di CSS
slug: Learn_web_development/Howto/Solve_CSS_problems
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Questa pagina raccoglie domande e risposte, e altro materiale sul sito MDN che può aiutarla a risolvere problemi comuni di CSS.

## Stilizzazione delle caselle

- [Come posso aggiungere un'ombra a un elemento?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Add_a_shadow)
  - : Le ombre possono essere aggiunte alle caselle con la proprietà {{cssxref("box-shadow")}}. Questo tutorial spiega come funziona e mostra un esempio.
- [Come posso riempire una casella con un'immagine senza distorcerla?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Fill_a_box_with_an_image)
  - : La proprietà {{cssxref("object-fit")}} offre diversi modi per adattare un'immagine in una casella che ha un {{Glossary("aspect_ratio", "rapporto d'aspetto")}} diverso, e nel tutorial può scoprire come utilizzarli.
- [Quali metodi possono essere usati per stilizzare le caselle?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Create_fancy_boxes)
  - : Una panoramica delle diverse proprietà che potrebbero essere utili quando si stilizzano le caselle usando CSS.
- [Come posso rendere gli elementi semi-trasparenti?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Make_box_transparent)
  - : La proprietà {{cssxref("opacity")}} e i valori di colore con un canale alpha possono essere usati per questo; scopra quale utilizzare a seconda del caso.

### Lezioni e guide per la stilizzazione delle caselle

- [Il Modello delle Caselle (Box Model)](/it/docs/Learn_web_development/Core/Styling_basics/Box_model)
- [Stilizzazione di sfondi e bordi](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders)

## CSS e testo

- [Come posso aggiungere un'ombra al testo?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Add_a_text_shadow)
  - : Le ombre possono essere aggiunte al testo con la proprietà {{cssxref("text-shadow")}}. Questo tutorial spiega come funziona e mostra un esempio.
- [Come posso evidenziare la prima riga di un paragrafo?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Highlight_first_line)
  - : Scopra come mirare alla prima riga di testo in un paragrafo con il pseudo-elemento {{cssxref("::first-line")}}.
- [Come posso evidenziare il primo paragrafo in un articolo?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Highlight_first_para)
  - : Scopra come mirare al primo paragrafo con la pseudo-classe {{cssxref(":first-child")}}.
- [Come posso evidenziare un paragrafo solo se segue immediatamente un'intestazione?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Highlight_para_after_h1)
  - : I combinatori possono aiutare a mirare con precisione agli elementi basandosi sulla loro posizione nel documento; questo tutorial spiega come usarli per applicare CSS a un paragrafo solo se segue immediatamente un'intestazione.

### Lezioni e guide per la stilizzazione del testo

- [Come stilizzare il testo](/it/docs/Learn_web_development/Core/Text_styling/Fundamentals)
- [Come personalizzare un elenco di elementi](/it/docs/Learn_web_development/Core/Text_styling/Styling_lists)
- [Come stilizzare i link](/it/docs/Learn_web_development/Core/Text_styling/Styling_links)
- [Selettori CSS](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors)

## Layout CSS

- [Come posso centrare un elemento?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Center_an_item)
  - : Centrare un elemento all'interno di un'altra casella orizzontalmente e verticalmente era complicato, tuttavia flexbox ora lo rende semplice.

### Guide al Layout

- [Utilizzo di CSS flexbox](/it/docs/Web/CSS/CSS_flexible_box_layout/Basic_concepts_of_flexbox)
- [Utilizzo di layout multi-colonna CSS](/it/docs/Web/CSS/CSS_multicol_layout/Using_multicol_layouts)
- [Utilizzo del layout grid CSS](/it/docs/Web/CSS/CSS_grid_layout/Basic_concepts_of_grid_layout)
- [Utilizzo di contenuto generato CSS](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Generated_content)

> [!NOTE]
> Abbiamo un ricettario dedicato alle [soluzioni di layout CSS](/it/docs/Web/CSS/Layout_cookbook), con esempi completamente funzionali e spiegazioni di compiti comuni di layout. Controlli anche [Esempi Pratici di Posizionamento](/it/docs/Learn_web_development/Core/CSS_layout/Practical_positioning_examples), che mostra come si possa utilizzare il posizionamento per creare una casella di informazioni con tab e un pannello nascosto scorrevole.
