---
title: Fondamenti dello stile CSS
slug: Learn_web_development/Core/Styling_basics
l10n:
  sourceCommit: cdf7db9ffe08cb952243d7e20b9beee4e9c9451b
---

{{NextMenu("Learn_web_development/Core/Styling_basics/What_is_CSS", "Learn_web_development/Core")}}

CSS (Cascading Style Sheets) viene usato per applicare stili e creare il layout delle pagine web, ad esempio per modificare il font, il colore, la dimensione e la spaziatura dei contenuti, suddividerli in più colonne oppure aggiungere animazioni e altre funzionalità decorative. Questo modulo fornisce tutte le basi di CSS necessarie per il momento, incluse sintassi, funzionalità e tecniche.

## Prerequisiti

Prima di iniziare questo modulo, dovrebbe essere configurato un ambiente di lavoro di base (come descritto in [Installare il software di base](/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software)) e dovrebbe essere chiaro come creare e gestire i file (come descritto in [Gestire i file](/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files)). È inoltre necessario avere familiarità con HTML (in caso contrario, seguire il nostro modulo [Strutturare i contenuti con HTML](/it/docs/Learn_web_development/Core/Structuring_content)).

> [!NOTE]
> Se si lavora su un computer, tablet o altro dispositivo su cui non è possibile creare file, è possibile provare a eseguire il codice in un editor online come [CodePen](https://codepen.io/) o [JSFiddle](https://jsfiddle.net/).

## Tutorial e sfide

- [Che cos'è CSS?](/it/docs/Learn_web_development/Core/Styling_basics/What_is_CSS)
  - : CSS consente di creare pagine web dall'aspetto gradevole, ma come funziona internamente? Questo articolo spiega che cos'è CSS, quale aspetto ha la sintassi di base e come il browser applica CSS a HTML per applicare gli stili.
- [Iniziare con CSS](/it/docs/Learn_web_development/Core/Styling_basics/Getting_started)
  - : In questo articolo verrà preso un semplice documento HTML e gli verrà applicato CSS, apprendendo lungo il percorso alcuni dettagli pratici del linguaggio. Verranno inoltre esaminate le funzionalità della sintassi CSS non ancora affrontate.
- [Applicare stili a una pagina biografica](/it/docs/Learn_web_development/Core/Styling_basics/Styling_a_bio_page) <sup>Sfida</sup>
  - : In questa sfida verranno applicati stili a una semplice pagina biografica, mettendo alla prova alcune delle competenze apprese nelle ultime lezioni, inclusa la scrittura di selettori, la colorazione degli sfondi e lo stile del testo. Verrà anche richiesto di cercare alcune funzionalità CSS di base non ancora trattate, per mettere alla prova le capacità di ricerca.
- [Selettori CSS di base](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors)
  - : In questo articolo verranno riepilogati alcuni fondamenti dei selettori, inclusi i selettori di tipo, classe e ID di base.
- [Selettori di attributo](/it/docs/Learn_web_development/Core/Styling_basics/Attribute_selectors)
  - : Come noto dallo studio di HTML, gli elementi possono avere attributi che forniscono ulteriori dettagli sull'elemento contrassegnato. In CSS è possibile usare i selettori di attributo per selezionare elementi con determinati attributi. Questa lezione mostrerà come usare questi selettori molto utili.
- [Pseudo-classi e pseudo-elementi](/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements)
  - : Il successivo insieme di selettori che verrà esaminato è denominato **pseudo-classi** e **pseudo-elementi**. Ne esiste un gran numero e spesso hanno scopi piuttosto specifici. Una volta imparato a usarli, è possibile esaminare i diversi tipi per verificare se ne esiste uno adatto all'attività da svolgere.
- [Combinatori](/it/docs/Learn_web_development/Core/Styling_basics/Combinators)
  - : Gli ultimi selettori che verranno esaminati sono chiamati combinatori. I combinatori vengono usati per combinare altri selettori in un modo che consente di selezionare elementi in base alla loro posizione nel DOM rispetto ad altri elementi, ad esempio un figlio o un elemento fratello.
- [Il box model](/it/docs/Learn_web_development/Core/Styling_basics/Box_model)
  - : In CSS tutto è circondato da un riquadro e comprendere questi riquadri è fondamentale per poter creare layout più complessi con CSS o allineare elementi ad altri elementi. In questa lezione verrà esaminato il _Box model_ di CSS. Sarà possibile comprendere come funziona e la relativa terminologia.
- [Gestire i conflitti](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts)
  - : Lo scopo di questa lezione è sviluppare la comprensione di alcuni dei concetti più fondamentali di CSS — la cascata, la specificità e l'ereditarietà — che controllano il modo in cui CSS viene applicato a HTML e come vengono risolti i conflitti tra dichiarazioni di stile.
- [Correggere gli stili di una pagina blog](/it/docs/Learn_web_development/Core/Styling_basics/Fixing_blog_styles) <sup>Sfida</sup>
  - : In questa sfida viene fornito un esempio di pagina blog di base parzialmente stilizzata. È necessario correggere alcuni problemi nel CSS esistente e aggiungere stili per completarla. Lungo il percorso verranno messe alla prova le conoscenze sui selettori, sul box model e sui conflitti/cascata.
- [Valori e unità](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units)
  - : Le regole CSS contengono [dichiarazioni](/it/docs/Web/CSS/Guides/Syntax/Introduction#css_declarations), a loro volta composte da proprietà e valori. Ogni proprietà usata in CSS ha un **tipo di valore** che descrive quali tipi di valori può avere. In questa lezione verranno esaminati alcuni dei tipi di valore usati più frequentemente, cosa sono e come funzionano.
- [Dimensionare gli elementi in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Sizing)
  - : Comprendere quali dimensioni avranno le diverse funzionalità del progetto è importante. In questa lezione verranno riepilogati i vari modi in cui gli elementi ottengono una dimensione tramite CSS e verranno definiti alcuni termini relativi al dimensionamento che saranno utili in futuro.
- [Sfondi e bordi](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders)
  - : In questa lezione verranno esaminate alcune delle cose creative che è possibile fare con gli sfondi e i bordi CSS. Dall'aggiunta di gradienti, immagini di sfondo e angoli arrotondati, sfondi e bordi rispondono a molte esigenze di stile in CSS.
- [Sfida: dimensionare e decorare un pannello di contenuto](/it/docs/Learn_web_development/Core/Styling_basics/Size_decorate_content_panel) <sup>Sfida</sup>
  - : In questa sfida viene fornita una struttura di pagina leggermente stilizzata che visualizza un pannello di contenuto, con un'intestazione in alto e una barra di pulsanti in basso. È necessario seguire le istruzioni per dimensionarlo e decorarlo, ottenendo come risultato un layout interessante. Lungo il percorso verranno messe alla prova le conoscenze su valori e unità CSS, dimensionamento, sfondi e bordi.
- [Contenuto in overflow](/it/docs/Learn_web_development/Core/Styling_basics/Overflow)
  - : L'overflow è ciò che accade quando il contenuto è troppo grande per entrare nel riquadro di un elemento. In questa lezione verrà spiegato come gestire l'overflow usando CSS.
- [Immagini, media ed elementi del modulo](/it/docs/Learn_web_development/Core/Styling_basics/Images_media_forms)
  - : In questa lezione verrà esaminato come alcuni elementi speciali vengono trattati in CSS. Immagini, altri media ed elementi del modulo si comportano in modo leggermente diverso dai riquadri normali per quanto riguarda la possibilità di applicare loro stili con CSS. Comprendere cosa è possibile e cosa non lo è può evitare alcune frustrazioni, e questa lezione evidenzierà alcuni degli aspetti principali da conoscere.
- [Applicare stili alle tabelle](/it/docs/Learn_web_development/Core/Styling_basics/Tables)
  - : Applicare stili a una tabella HTML non è il lavoro più affascinante del mondo, ma talvolta è necessario farlo. Questo articolo spiega come rendere gradevoli le tabelle HTML, evidenziando alcune tecniche specifiche per applicare stili alle tabelle.
- [Debug di CSS](/it/docs/Learn_web_development/Core/Styling_basics/Debugging_CSS)
  - : Questo articolo fornirà indicazioni su come eseguire il debug di un problema CSS e mostrerà come i DevTools inclusi in tutti i browser moderni possono aiutare a capire cosa sta succedendo.

## Metti alla prova le tue competenze

Tra gli articoli tutorial si trovano articoli "Metti alla prova le tue competenze", per verificare di aver acquisito le informazioni più importanti prima di proseguire. Per esplorarli tutti insieme, sono elencati in [Metti alla prova le tue competenze: fondamenti dello stile CSS](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills).

## Tutorial aggiuntivi

Questi tutorial non fanno parte del percorso di apprendimento, ma sono comunque interessanti: possono essere considerati obiettivi più ambiziosi, da studiare facoltativamente dopo aver completato gli articoli principali della sezione Core.

- [Effetti di stile avanzati](/it/docs/Learn_web_development/Core/Styling_basics/Advanced_styling_effects)
  - : Questo articolo funge da raccolta di trucchi, fornendo un'introduzione ad alcune interessanti funzionalità di stile avanzate, quali ombre dei riquadri, modalità di fusione e filtri.
- [Livelli della cascata](/it/docs/Learn_web_development/Core/Styling_basics/Cascade_layers)
  - : Questa lezione mira a introdurre i [livelli della cascata](/it/docs/Web/CSS/Reference/At-rules/@layer), una funzionalità più avanzata che si basa sui concetti fondamentali della [cascata CSS](/it/docs/Web/CSS/Guides/Cascade/Introduction) e della [specificità CSS](/it/docs/Web/CSS/Guides/Cascade/Specificity).
- [Gestire diverse direzioni del testo](/it/docs/Learn_web_development/Core/Styling_basics/Handling_different_text_directions)
  - : Negli ultimi anni, CSS si è evoluto per supportare meglio le diverse direzionalità dei contenuti, inclusi i contenuti da destra a sinistra e dall'alto verso il basso, come il giapponese: queste diverse direzionalità sono chiamate modalità di scrittura. Man mano che si progredisce nello studio e si inizia a lavorare con il layout, la comprensione delle modalità di scrittura sarà molto utile; per questo verranno introdotte in questo articolo.
- [Organizzare CSS](/it/docs/Learn_web_development/Core/Styling_basics/Organizing)
  - : Quando si inizia a lavorare su fogli di stile più grandi e progetti di grandi dimensioni, si scoprirà che gestire un enorme file CSS può essere impegnativo. In questo articolo verranno esaminate brevemente alcune buone pratiche per scrivere CSS in modo che sia facilmente manutenibile e alcune delle soluzioni usate da altri per contribuire a migliorarne la manutenibilità.

## Vedi anche

- [Impara HTML e CSS](https://scrimba.com/learn-html-and-css-c0p?via=mdn), Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Learn HTML and CSS_ di [Scrimba](https://scrimba.com/?via=mdn) insegna HTML e CSS attraverso la creazione e la distribuzione di cinque fantastici progetti, con lezioni interattive e divertenti e sfide tenute da insegnanti esperti.
- [Scrivi le tue prime righe di CSS!](https://scrimba.com/the-frontend-developer-career-path-c0j/~015?via=mdn), Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Questa lezione interattiva fornisce un'utile introduzione alla sintassi CSS.

{{NextMenu("Learn_web_development/Core/Styling_basics/What_is_CSS", "Learn_web_development/Core")}}
