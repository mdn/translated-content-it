---
title: Risolvere problemi CSS comuni
short-title: Problemi CSS comuni
slug: Learn_web_development/Howto/Solve_CSS_problems
l10n:
  sourceCommit: 85fccefc8066bd49af4ddafc12c77f35265c7e2d
---

Questa pagina raccoglie domande e risposte, oltre ad altro materiale sul sito MDN, che può aiutare a risolvere problemi CSS comuni.

## Applicare stili ai riquadri

- [Come aggiungere un'ombra esterna a un elemento?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Add_a_shadow)
  - : È possibile aggiungere ombre ai riquadri con la proprietà {{cssxref("box-shadow")}}. Questo tutorial ne spiega il funzionamento e mostra un esempio.
- [Come riempire un riquadro con un'immagine senza distorcere l'immagine?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Fill_a_box_with_an_image)
  - : La proprietà {{cssxref("object-fit")}} offre diversi modi per adattare un'immagine a un riquadro con un diverso {{Glossary("aspect_ratio", "rapporto d'aspetto")}}; questo tutorial spiega come usarli.
- [Quali metodi possono essere usati per applicare stili ai riquadri?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Create_fancy_boxes)
  - : Una panoramica delle diverse proprietà che possono essere utili per applicare stili ai riquadri usando CSS.
- [Come rendere gli elementi semitrasparenti?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Make_box_transparent)
  - : A questo scopo è possibile usare la proprietà {{cssxref("opacity")}} e i valori di colore con un canale alfa; scopri quando usare ciascuno di essi.

### Lezioni e guide sullo stile dei riquadri

- [Il modello a riquadri](/it/docs/Learn_web_development/Core/Styling_basics/Box_model)
- [Applicare stili a sfondi e bordi](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders)

## CSS e testo

- [Come aggiungere un'ombra esterna al testo?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Add_a_text_shadow)
  - : È possibile aggiungere ombre al testo con la proprietà {{cssxref("text-shadow")}}. Questo tutorial ne spiega il funzionamento e mostra un esempio.
- [Come evidenziare la prima riga di un paragrafo?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Highlight_first_line)
  - : Scopri come selezionare la prima riga di testo in un paragrafo con lo pseudo-elemento {{cssxref("::first-line")}}.
- [Come evidenziare il primo paragrafo in un articolo?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Highlight_first_para)
  - : Scopri come selezionare il primo paragrafo con la pseudo-classe {{cssxref(":first-child")}}.
- [Come evidenziare un paragrafo solo se si trova subito dopo un'intestazione?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Highlight_para_after_h1)
  - : I combinatori permettono di selezionare con precisione gli elementi in base alla loro posizione nel documento; questo tutorial spiega come usarli per applicare CSS a un paragrafo solo se segue immediatamente un'intestazione.

### Lezioni e guide sullo stile del testo

- [Come applicare stili al testo](/it/docs/Learn_web_development/Core/Text_styling/Fundamentals)
- [Come personalizzare un elenco di elementi](/it/docs/Learn_web_development/Core/Text_styling/Styling_lists)
- [Come applicare stili ai link](/it/docs/Learn_web_development/Core/Text_styling/Styling_links)
- [Selettori CSS](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors)

## Layout CSS

- [Come centrare un elemento?](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Center_an_item)
  - : Centrare un elemento all'interno di un altro riquadro, sia orizzontalmente sia verticalmente, era un'operazione complessa; tuttavia, flexbox ora la rende semplice.

### Guide sul layout

- [Usare CSS flexbox](/it/docs/Web/CSS/Guides/Flexible_box_layout/Basic_concepts)
- [Usare layout CSS a più colonne](/it/docs/Web/CSS/Guides/Multicol_layout/Using)
- [Usare il layout CSS grid](/it/docs/Web/CSS/Guides/Grid_layout/Basic_concepts)
- [Usare contenuto CSS generato](/it/docs/Learn_web_development/Howto/Solve_CSS_problems/Generated_content)

> [!NOTE]
> È disponibile un ricettario dedicato alle [soluzioni per il layout CSS](/it/docs/Web/CSS/How_to/Layout_cookbook), con esempi completamente funzionanti e spiegazioni delle attività di layout più comuni. È possibile consultare anche [Esempi pratici di posizionamento](/it/docs/Learn_web_development/Core/CSS_layout/Practical_positioning_examples), che mostra come usare il posizionamento per creare un riquadro informativo a schede e un pannello nascosto scorrevole.
