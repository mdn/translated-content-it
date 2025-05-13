---
title: Layout CSS
slug: Learn_web_development/Core/CSS_layout
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Core/CSS_layout/Introduction", "Learn_web_development/Core")}}

Nei moduli precedenti abbiamo visto come stilizzare e manipolare i box dove il suo contenuto si colloca. Ora è il momento di esaminare come disporre correttamente i suoi box in relazione l'uno all'altro e alla finestra del browser. Questo modulo tratta i float, il posizionamento, altri strumenti moderni di layout, e la costruzione di design responsivi che si adattano a diversi dispositivi, dimensioni dello schermo e risoluzioni.

## Prerequisiti

Prima di iniziare questo modulo, dovrebbe essere familiare con [HTML](/it/docs/Learn_web_development/Core/Structuring_content), i [fondamenti di base del CSS](/it/docs/Learn_web_development/Core/Styling_basics), e la [stilizzazione del testo con CSS](/it/docs/Learn_web_development/Core/Text_styling).

> [!NOTE]
> Se sta lavorando su un computer/tablet/altro dispositivo dove non ha la possibilità di creare i propri file, potrebbe provare (la maggior parte) degli esempi di codice in un programma di coding online come [JSBin](https://jsbin.com/) o [Glitch](https://glitch.com/).

## Tutorial e sfide

- [Introduzione al layout CSS](/it/docs/Learn_web_development/Core/CSS_layout/Introduction)
  - : Questa lezione ricapitola alcune delle caratteristiche del layout CSS che abbiamo già trattato nei moduli precedenti, come diversi valori di {{cssxref("display")}}, oltre a introdurre alcuni dei concetti che copriremo durante questo modulo. Copre anche il concetto di flusso normale in profondità.
- [Float](/it/docs/Learn_web_development/Core/CSS_layout/Floats)
  - : Originariamente utilizzata per fluttuare immagini all'interno di blocchi di testo, la proprietà {{cssxref("float")}} è diventata uno degli strumenti più comunemente usati per creare layout a più colonne nelle pagine web. Con l'avvento di flexbox e grid, è tornata al suo scopo originale, come spiega questo articolo.
- [Posizionamento](/it/docs/Learn_web_development/Core/CSS_layout/Positioning)
  - : Il posizionamento consente di rimuovere gli elementi dal flusso normale del documento e farli comportare diversamente, ad esempio, posizionandoli uno sopra l'altro o facendoli rimanere sempre nello stesso posto nella finestra del browser. Questo articolo spiega i diversi valori di {{cssxref("position")}} e come usarli.
- [Flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox)
  - : [Flexbox](/it/docs/Web/CSS/CSS_flexible_box_layout/Typical_use_cases_of_flexbox) è un metodo di layout monodimensionale per disporre gli elementi in righe o colonne. Gli elementi flessibili si espandono per riempire lo spazio aggiuntivo e si riducono per adattarsi a spazi più piccoli. Questo articolo spiega tutti i fondamenti.
- [Layout a griglia CSS](/it/docs/Learn_web_development/Core/CSS_layout/Grids)
  - : Il layout a griglia CSS è un sistema di layout bidimensionale per il web. Permette di organizzare il contenuto in righe e colonne e offre molte funzionalità per semplificare la creazione di layout complessi. Questo articolo spiegherà tutto ciò che deve sapere per iniziare con il layout a griglia.
- [Design responsivo](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design)
  - : Con l'apparire di dimensioni dello schermo più diversificate sui dispositivi abilitati per il web, è nato il concetto di design web responsivo (RWD): un insieme di pratiche che consente alle pagine web di modificare il loro layout e l'aspetto per adattarsi a diverse larghezze dello schermo, risoluzioni, ecc. È un'idea che ha cambiato il modo in cui progettiamo per un web multi-dispositivo, e in questo articolo la aiuteremo a comprendere le principali tecniche che deve conoscere per padroneggiarlo.
- [Fondamenti delle media query](/it/docs/Learn_web_development/Core/CSS_layout/Media_queries)
  - : Le **media query CSS** le danno un modo per applicare CSS solo quando l'ambiente del browser e del dispositivo corrisponde alle regole che specifica. Le media query sono una parte fondamentale del design web responsivo perché consentono di creare layout diversi a seconda delle dimensioni della finestra del browser. In questa lezione, apprenderà la sintassi usata nelle media query per poi usarle in un esempio interattivo che mostra come un design semplice possa diventare responsivo.
- [Comprensione fondamentale del layout](/it/docs/Learn_web_development/Core/CSS_layout/Fundamental_Layout_Comprehension) <sup>Sfida</sup>
  - : Una sfida per testare la sua conoscenza dei diversi metodi di layout disponendo una pagina web.

## Tutorial aggiuntivi

Questi tutorial non fanno parte del percorso di apprendimento, ma sono comunque interessanti — li consideri come obiettivi ulteriori, da studiare facoltativamente una volta completati gli articoli Core principali.

- [Layout a più colonne](/it/docs/Learn_web_development/Core/CSS_layout/Multiple-column_Layout)
  - : La specifica per il layout a più colonne le fornisce un metodo per disporre il contenuto in colonne, come potrebbe vedere in un giornale. Questo articolo spiega come usare questa funzionalità.
- [Metodi di layout legacy](/it/docs/Learn_web_development/Core/CSS_layout/Legacy_Layout_Methods)
  - : I sistemi a griglia sono una funzione molto comune utilizzata nei layout CSS e, prima del layout a griglia CSS, tendevano ad essere implementati usando float o altre caratteristiche di layout. Immagini il suo layout come un numero impostato di colonne (ad esempio, 4, 6 o 12), e poi inserisca le sue colonne di contenuti all'interno di queste colonne immaginarie. In questo articolo esploreremo come funzionano questi metodi più vecchi, in modo che lei possa capire come venivano usati se lavora su un progetto più datato.
- [Supportare browser più vecchi](/it/docs/Learn_web_development/Core/CSS_layout/Supporting_Older_Browsers)
  - : I visitatori del suo sito web possono includere utenti che usano browser più vecchi o che usano browser che non supportano le funzionalità CSS che ha implementato. Questo è uno scenario comune sul web, dove nuove funzionalità vengono continuamente aggiunte al CSS. I browser differiscono nel loro supporto per queste funzionalità perché i diversi browser tendono a dare priorità all'implementazione di diverse funzionalità. Questo articolo spiega come lei, in quanto sviluppatore web, può usare tecniche web moderne per assicurarsi che il suo sito rimanga accessibile agli utenti con tecnologia più vecchia.

## Veda anche

- [Esempi pratici di posizionamento](/it/docs/Learn_web_development/Core/CSS_layout/Practical_positioning_examples)
  - : Questo articolo mostra come costruire alcuni esempi del mondo reale per illustrare che tipi di cose può fare con il posizionamento.
- [Cookbook per layout CSS](/it/docs/Web/CSS/Layout_cookbook)
  - : Il cookbook per il layout CSS mira a raccogliere ricette per modelli di layout comuni, cose che potrebbe aver bisogno di implementare nei suoi siti. Oltre a fornire codice che può usare come punto di partenza nei suoi progetti, queste ricette evidenziano i diversi modi in cui le specifiche di layout possono essere usate e le scelte che può fare come sviluppatore.

{{NextMenu("Learn_web_development/Core/CSS_layout/Introduction", "Learn_web_development/Core")}}
