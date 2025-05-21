---
title: Layout CSS
slug: Learn_web_development/Core/CSS_layout
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Core/CSS_layout/Introduction", "Learn_web_development/Core")}}

Nei moduli precedenti abbiamo esaminato come stilizzare e manipolare i box che contengono il tuo contenuto. Ora è il momento di vedere come disporre correttamente i tuoi box in relazione tra loro e con il viewport del browser. Questo modulo analizza float, posizionamento, altri strumenti di layout moderni e la creazione di design reattivi che si adattano a diversi dispositivi, dimensioni di schermo e risoluzioni.

## Prerequisiti

Prima di iniziare questo modulo, dovresti avere familiarità con [HTML](/it/docs/Learn_web_development/Core/Structuring_content), i [fondamenti di base del CSS](/it/docs/Learn_web_development/Core/Styling_basics) e lo [stile del testo in CSS](/it/docs/Learn_web_development/Core/Text_styling).

> [!NOTE]
> Se stai lavorando su un computer/tablet/altro dispositivo dove non hai la possibilità di creare i tuoi file, puoi provare (la maggior parte) degli esempi di codice in un programma di codifica online come [JSBin](https://jsbin.com/) o [Glitch](https://glitch.com/).

## Tutorial e sfide

- [Introduzione al layout CSS](/it/docs/Learn_web_development/Core/CSS_layout/Introduction)
  - : Questa lezione ricapitola alcune delle caratteristiche del layout CSS che abbiamo già toccato nei moduli precedenti, come diversi valori di {{cssxref("display")}}, oltre a introdurre alcuni dei concetti che tratteremo durante questo modulo. Copre anche in dettaglio il concetto di flusso normale.
- [Floats](/it/docs/Learn_web_development/Core/CSS_layout/Floats)
  - : Originariamente utilizzata per far fluttuare immagini all'interno di blocchi di testo, la proprietà {{cssxref("float")}} è diventata uno degli strumenti più comunemente utilizzati per creare layout a colonne multiple sulle pagine web. Con l'avvento di flexbox e grid, è tornata al suo scopo originale, come spiegato in questo articolo.
- [Posizionamento](/it/docs/Learn_web_development/Core/CSS_layout/Positioning)
  - : Il posizionamento ti permette di rimuovere gli elementi dal flusso normale del documento e farli comportare diversamente, ad esempio, sovrapporsi o rimanere sempre nello stesso punto all'interno del viewport del browser. Questo articolo spiega i diversi valori di {{cssxref("position")}} e come usarli.
- [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox)
  - : [Flexbox](/it/docs/Web/CSS/CSS_flexible_box_layout/Typical_use_cases_of_flexbox) è un metodo di layout unidimensionale per disporre gli elementi in righe o colonne. Gli elementi si espandono per riempire lo spazio aggiuntivo e si restringono per adattarsi agli spazi più piccoli. Questo articolo spiega tutti i fondamenti.
- [Layout a griglia CSS](/it/docs/Learn_web_development/Core/CSS_layout/Grids)
  - : Il layout a griglia CSS è un sistema di layout bidimensionale per il web. Ti consente di organizzare il contenuto in righe e colonne e offre molte funzionalità per semplificare la creazione di layout complessi. Questo articolo ti spiegherà tutto ciò che devi sapere per iniziare con il layout a griglia.
- [Design responsivo](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design)
  - : Poiché sono apparse dimensioni di schermo più diversificate sui dispositivi connessi al web, è apparso il concetto di design web responsivo (RWD): un insieme di pratiche che consente alle pagine web di modificare il loro layout e il loro aspetto per adattarsi a diverse larghezze di schermo, risoluzioni, ecc. È un'idea che ha cambiato il modo in cui progettiamo per un web multi-dispositivo, e in questo articolo ti aiuteremo a comprendere le tecniche principali che devi conoscere per padroneggiarla.
- [Fondamenti delle media query](/it/docs/Learn_web_development/Core/CSS_layout/Media_queries)
  - : La **Media Query CSS** ti offre un modo per applicare CSS solo quando l'ambiente del browser e del dispositivo corrisponde alle regole specificate. Le media query sono una parte fondamentale del design web responsivo perché ti consentono di creare layout differenti a seconda delle dimensioni del viewport. In questa lezione, imparerai la sintassi utilizzata nelle media query per poi usarle in un esempio interattivo che mostra come un semplice design potrebbe diventare responsivo.
- [Comprensione fondamentale del layout](/it/docs/Learn_web_development/Core/CSS_layout/Fundamental_Layout_Comprehension) <sup>Challenge</sup>
  - : Una sfida per testare la tua conoscenza di diversi metodi di layout disponendo una pagina web.

## Tutorial aggiuntivi

Questi tutorial non fanno parte del percorso di apprendimento, ma sono comunque interessanti — dovresti considerarli come obiettivi da raggiungere opzionalmente dopo aver completato gli articoli principali del core.

- [Layout a colonne multiple](/it/docs/Learn_web_development/Core/CSS_layout/Multiple-column_Layout)
  - : La specifica di layout a colonne multiple ti fornisce un metodo per disporre il contenuto in colonne, come potresti vedere in un giornale. Questo articolo spiega come utilizzare questa caratteristica.
- [Metodi di layout legacy](/it/docs/Learn_web_development/Core/CSS_layout/Legacy_Layout_Methods)
  - : I sistemi a griglia sono una caratteristica molto comune utilizzata nei layout CSS e, prima del layout a griglia CSS, tendevano a essere implementati usando float o altre caratteristiche di layout. Immagini il tuo layout come un numero fisso di colonne (es. 4, 6 o 12), e poi adattare le tue colonne di contenuto all'interno di queste colonne immaginarie. In questo articolo esploreremo come funzionano questi metodi più vecchi, affinché tu comprenda come venivano usati se lavori su un progetto più vecchio.
- [Supporto per browser più vecchi](/it/docs/Learn_web_development/Core/CSS_layout/Supporting_Older_Browsers)
  - : I visitatori del tuo sito potrebbero includere utenti che utilizzano browser più vecchi o browser che non supportano le funzionalità CSS che hai implementato. Questo è uno scenario comune sul web, dove vengono continuamente aggiunte nuove funzionalità al CSS. I browser differiscono nel loro supporto per queste funzionalità perché i diversi browser tendono a dare priorità all'implementazione di funzionalità diverse. Questo articolo spiega come, in qualità di sviluppatore web, puoi utilizzare tecniche moderne per garantire che il tuo sito web rimanga accessibile agli utenti con tecnologia più vecchia.

## Vedi anche

- [Esempi di posizionamento pratico](/it/docs/Learn_web_development/Core/CSS_layout/Practical_positioning_examples)
  - : Questo articolo mostra come costruire alcuni esempi del mondo reale per illustrare cosa puoi fare con il posizionamento.
- [Ricettario di layout CSS](/it/docs/Web/CSS/Layout_cookbook)
  - : Il ricettario di layout CSS mira a riunire ricette per schemi di layout comuni, cose che potresti dover implementare nei tuoi siti. Oltre a fornire codice che puoi utilizzare come punto di partenza nei tuoi progetti, queste ricette evidenziano i diversi modi in cui le specifiche di layout possono essere utilizzate e le scelte che puoi fare come sviluppatore.

{{NextMenu("Learn_web_development/Core/CSS_layout/Introduction", "Learn_web_development/Core")}}
