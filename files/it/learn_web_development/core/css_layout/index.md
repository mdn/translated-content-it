---
title: Layout CSS
slug: Learn_web_development/Core/CSS_layout
l10n:
  sourceCommit: 85fccefc8066bd49af4ddafc12c77f35265c7e2d
---

{{NextMenu("Learn_web_development/Core/CSS_layout/Introduction", "Learn_web_development/Core")}}

Nei moduli precedenti è stato esaminato come applicare stili e manipolare i riquadri che contengono i contenuti. Ora è il momento di vedere come disporre correttamente i riquadri gli uni rispetto agli altri e rispetto alla viewport del browser. Questo modulo tratta float, posizionamento, altri moderni strumenti di layout e la creazione di design responsive che si adattano a dispositivi, dimensioni dello schermo e risoluzioni differenti.

## Prerequisiti

Prima di iniziare questo modulo, è necessario avere familiarità con [HTML](/it/docs/Learn_web_development/Core/Structuring_content), i [fondamenti di base di CSS](/it/docs/Learn_web_development/Core/Styling_basics) e la [formattazione del testo CSS](/it/docs/Learn_web_development/Core/Text_styling).

> [!NOTE]
> Se si lavora su un computer, tablet o altro dispositivo su cui non è possibile creare file, è possibile provare a eseguire il codice in un editor online come [CodePen](https://codepen.io/) o [JSFiddle](https://jsfiddle.net/).

## Tutorial e sfide

- [Introduzione al layout CSS](/it/docs/Learn_web_development/Core/CSS_layout/Introduction)
  - : Questa lezione riepiloga alcune delle funzionalità di layout CSS già trattate nei moduli precedenti, come i diversi valori di {{cssxref("display")}}, e introduce alcuni dei concetti che verranno affrontati nel corso di questo modulo. Tratta inoltre in modo approfondito il concetto di flusso normale.
- [Float](/it/docs/Learn_web_development/Core/CSS_layout/Floats)
  - : Originariamente pensata per far fluttuare immagini all'interno di blocchi di testo, la proprietà {{cssxref("float")}} è diventata uno degli strumenti più utilizzati per creare layout a più colonne nelle pagine web. Con l'avvento di flexbox e grid, è ora tornata al suo scopo originale, come spiega questo articolo.
- [Posizionamento](/it/docs/Learn_web_development/Core/CSS_layout/Positioning)
  - : Il posizionamento consente di estrarre gli elementi dal normale flusso del documento e farli comportare diversamente, ad esempio sovrapponendoli oppure facendoli rimanere sempre nello stesso punto all'interno della viewport del browser. Questo articolo spiega i diversi valori di {{cssxref("position")}} e come usarli.
- [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox)
  - : [Flexbox](/it/docs/Web/CSS/Guides/Flexible_box_layout/Use_cases) è un metodo di layout unidimensionale per disporre gli elementi in righe o colonne. Gli elementi si espandono per riempire lo spazio aggiuntivo e si restringono per adattarsi a spazi più piccoli. Questo articolo spiega tutti i fondamenti.
- [Layout CSS grid](/it/docs/Learn_web_development/Core/CSS_layout/Grids)
  - : Il layout CSS grid è un sistema di layout bidimensionale per il web. Permette di organizzare i contenuti in righe e colonne e offre molte funzionalità per semplificare la creazione di layout complessi. Questo articolo spiega tutto ciò che serve per iniziare a usare grid layout.
- [Comprensione dei fondamenti del layout](/it/docs/Learn_web_development/Core/CSS_layout/Fundamental_Layout_Comprehension) <sup>Sfida</sup>
  - : Questa sfida metterà alla prova la conoscenza delle funzionalità di layout trattate finora nel modulo, ovvero flexbox, float, grid e posizionamento. Al termine sarà stato sviluppato il layout di una pagina web usando diverse tecniche.
- [Design responsive](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design)
  - : Con la comparsa di dimensioni dello schermo sempre più diverse sui dispositivi abilitati al web, è comparso il concetto di responsive web design (RWD): un insieme di pratiche che consente alle pagine web di modificare layout e aspetto per adattarsi a diverse larghezze dello schermo, risoluzioni e così via. È un'idea che ha cambiato il modo di progettare per un web multi-dispositivo e, in questo articolo, verranno illustrate le principali tecniche necessarie per padroneggiarla.
- [Fondamenti delle media query](/it/docs/Learn_web_development/Core/CSS_layout/Media_queries)
  - : Le **CSS Media Query** consentono di applicare CSS solo quando l'ambiente del browser e del dispositivo corrisponde alle regole specificate. Le media query sono una parte fondamentale del responsive web design perché permettono di creare layout diversi in base alle dimensioni della viewport. In questa lezione verrà illustrata la sintassi utilizzata nelle media query, quindi verranno usate in un esempio interattivo che mostra come rendere responsive un design semplice.

## Metti alla prova le tue competenze

Tra gli articoli tutorial sono presenti articoli "Metti alla prova le tue competenze" per verificare se sono state assimilate le informazioni più importanti prima di proseguire. Per esplorarli tutti insieme, sono elencati in [Metti alla prova le tue competenze: layout CSS](/it/docs/Learn_web_development/Core/CSS_layout/Test_your_skills).

## Tutorial aggiuntivi

Questi tutorial non fanno parte del percorso di apprendimento, ma sono comunque interessanti: possono essere considerati obiettivi aggiuntivi, da studiare facoltativamente una volta terminati gli articoli principali di Core.

- [Layout a più colonne](/it/docs/Learn_web_development/Core/CSS_layout/Multiple-column_Layout)
  - : La specifica del layout a più colonne fornisce un metodo per disporre i contenuti in colonne, come avviene in un giornale. Questo articolo spiega come usare questa funzionalità.
- [Esempi pratici di posizionamento](/it/docs/Learn_web_development/Core/CSS_layout/Practical_positioning_examples)
  - : Questo articolo mostra come creare alcuni esempi reali per illustrare quali tipi di risultati è possibile ottenere con il posizionamento.
- [Metodi di layout legacy](/it/docs/Learn_web_development/Core/CSS_layout/Legacy_Layout_Methods)
  - : I sistemi a griglia sono una funzionalità molto comune nei layout CSS e, prima del layout CSS grid, tendevano a essere implementati usando float o altre funzionalità di layout. Si immagina il layout come un numero stabilito di colonne, ad esempio 4, 6 o 12, e quindi si inseriscono le colonne dei contenuti all'interno di queste colonne immaginarie. In questo articolo verrà esaminato il funzionamento di questi metodi meno recenti, per comprendere come venivano utilizzati quando si lavora su un progetto meno recente.
- [Supportare browser meno recenti](/it/docs/Learn_web_development/Core/CSS_layout/Supporting_Older_Browsers)
  - : Tra i visitatori di un sito web potrebbero esserci utenti che usano browser meno recenti oppure browser che non supportano le funzionalità CSS implementate. Questo è uno scenario comune sul web, dove vengono continuamente aggiunte nuove funzionalità a CSS. I browser differiscono nel supporto di queste funzionalità perché browser diversi tendono a dare priorità all'implementazione di funzionalità diverse. Questo articolo spiega come uno sviluppatore web può utilizzare tecniche web moderne per garantire che il proprio sito web rimanga accessibile agli utenti con tecnologie meno recenti.

## Vedi anche

- [Ricettario del layout CSS](/it/docs/Web/CSS/How_to/Layout_cookbook)
  - : Il ricettario del layout CSS mira a riunire ricette per modelli di layout comuni, che potrebbero essere necessari da implementare nei siti. Oltre a fornire codice utilizzabile come punto di partenza nei progetti, queste ricette evidenziano i diversi modi in cui le specifiche di layout possono essere utilizzate e le scelte possibili per uno sviluppatore.
- [Impara Flexbox](https://scrimba.com/learn-flexbox-c0k?via=mdn) e [Impara CSS Grid](https://scrimba.com/learn-css-grid-c02k?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Questi corsi di Scrimba offrono lezioni interattive che insegnano tutto ciò che serve sapere su Flexbox e Grid.

{{NextMenu("Learn_web_development/Core/CSS_layout/Introduction", "Learn_web_development/Core")}}
