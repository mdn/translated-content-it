---
title: Basi di stile CSS
slug: Learn_web_development/Core/Styling_basics
l10n:
  sourceCommit: 5f37fd46fc2408e6b646fe81d4964be7168be197
---

{{NextMenu("Learn_web_development/Core/Styling_basics/What_is_CSS", "Learn_web_development/Core")}}

CSS (Cascading Style Sheets) è utilizzato per stilizzare e impaginare le pagine web, per esempio, per modificare il carattere, colore, dimensione e spaziatura del tuo contenuto, dividerlo in più colonne, o aggiungere animazioni e altre caratteristiche decorative. Questo modulo fornisce tutti i fondamenti di CSS di cui avrai bisogno per ora, inclusi sintassi, funzionalità e tecniche.

## Prerequisiti

Prima di iniziare questo modulo, dovresti aver configurato un ambiente di lavoro base (come descritto in [Installare il software di base](/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software)) e comprendere come creare e gestire i file (come descritto in [Gestire i file](/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files)). Dovresti anche essere familiare con HTML (lavora attraverso il nostro modulo [Strutturare il contenuto con HTML](/it/docs/Learn_web_development/Core/Structuring_content) se non lo sei).

> [!NOTE]
> Se sta lavorando su un computer/tablet/altro dispositivo dove non ha la possibilità di creare i propri file, potrebbe provare (la maggior parte) degli esempi di codice in un programma di codifica online come [JSBin](https://jsbin.com/) o [Glitch](https://glitch.com/).

## Tutorial e sfide

- [Cos'è CSS?](/it/docs/Learn_web_development/Core/Styling_basics/What_is_CSS)
  - : CSS permette di creare pagine web dall'aspetto fantastico, ma come funziona sotto il cofano? Questo articolo spiega cos'è CSS, com'è la sintassi di base, e come il browser applica CSS a HTML per stilizzarlo.
- [Iniziare con CSS](/it/docs/Learn_web_development/Core/Styling_basics/Getting_started)
  - : In questo articolo, prenderemo un semplice documento HTML e applicheremo CSS, imparando alcuni dettagli pratici del linguaggio lungo il percorso. Inoltre, rivedremo le funzionalità della sintassi CSS che non hai ancora esaminato.
- [Stilizzare una pagina biografica](/it/docs/Learn_web_development/Core/Styling_basics/Styling_a_bio_page) <sup>Sfida</sup>
  - : In questa sfida stilerai una semplice pagina biografica, mettendoti alla prova su alcune delle abilità che hai appreso nelle ultime lezioni, inclusa la scrittura di selettori e la stilizzazione del testo.
- [Selettori CSS di base](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors)
  - : In questo articolo ripasseremo alcuni fondamenti dei selettori, inclusi i selettori di tipo base, di classe e ID.
- [Selettori di attributo](/it/docs/Learn_web_development/Core/Styling_basics/Attribute_selectors)
  - : Come sai dallo studio di HTML, gli elementi possono avere attributi che forniscono informazioni aggiuntive sull'elemento marcato. In CSS puoi usare i selettori di attributo per selezionare elementi con certi attributi. Questa lezione ti mostrerà come utilizzare questi selettori molto utili.
- [Pseudo-classi e pseudo-elementi](/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements)
  - : Il prossimo set di selettori che esamineremo sono chiamati **pseudo-classi** e **pseudo-elementi**. Ce ne sono un gran numero, e spesso servono a scopi abbastanza specifici. Una volta che saprai come usarli, potrai esplorare i diversi tipi per vedere se c'è qualcosa che funziona per il compito che stai cercando di realizzare.
- [Combinatori](/it/docs/Learn_web_development/Core/Styling_basics/Combinators)
  - : Gli ultimi selettori che esamineremo sono chiamati combinatori. I combinatori sono usati per combinare altri selettori in un modo che ci permette di selezionare elementi in base alla loro posizione nel DOM rispetto ad altri elementi (per esempio, figlio o fratello).
- [Il modello a scatola](/it/docs/Learn_web_development/Core/Styling_basics/Box_model)
  - : Tutto in CSS ha una scatola intorno ad esso, e comprendere queste scatole è fondamentale per poter creare layout più complessi con CSS, o allineare elementi con altri elementi. In questa lezione, esamineremo il _modello a scatola_ di CSS. Capirai come funziona e la terminologia ad esso relativa.
- [Gestire i conflitti](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts)
  - : L'obiettivo di questa lezione è sviluppare la tua comprensione di alcuni dei concetti più fondamentali del CSS — la cascata, la specificità e l'ereditarietà — che controllano come CSS è applicato a HTML e come vengono risolti i conflitti tra dichiarazioni di stile.
- [Valori ed unità](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units)
  - : Le regole CSS contengono [dichiarazioni](/it/docs/Web/CSS/CSS_syntax/Syntax#css_declarations), che a loro volta sono composte da proprietà e valori. Ogni proprietà utilizzata in CSS ha un **tipo di valore** che descrive che tipo di valori è permesso. In questa lezione, esamineremo alcuni dei tipi di valore più utilizzati, cosa sono e come funzionano.
- [Dimensionamento degli elementi in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Sizing)
  - : Comprendere quanto grandi saranno le diverse caratteristiche del tuo design è importante. In questa lezione riassumeremo i vari modi in cui gli elementi ottengono una dimensione tramite CSS e definiremo alcuni termini relativi al dimensionamento che ti aiuteranno in futuro.
- [Sfondo e bordi](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders)
  - : In questa lezione, esamineremo alcune delle cose creative che puoi fare con sfondi e bordi CSS. Dall'aggiungere gradienti, immagini di sfondo, e angoli arrotondati, sfondi e bordi sono la risposta a molte domande di stile in CSS.
- [Contenuto traboccante](/it/docs/Learn_web_development/Core/Styling_basics/Overflow)
  - : Il trabocco è ciò che accade quando c'è troppo contenuto per adattarsi all'interno di una scatola di elemento. In questa lezione, imparerai come gestire il trabocco usando CSS.
- [Immagini, media ed elementi di modulo](/it/docs/Learn_web_development/Core/Styling_basics/Images_media_forms)
  - : In questa lezione esamineremo come certi elementi speciali sono trattati in CSS. Le immagini, altri media ed elementi di modulo si comportano un po' diversamente dalle scatole regolari in termini di capacità di stilizzarli con CSS. Comprendere cosa è possibile e cosa non lo è può evitare qualche frustrazione, e questa lezione metterà in luce alcune delle principali cose che è necessario sapere.
- [Stilizzare le tabelle](/it/docs/Learn_web_development/Core/Styling_basics/Tables)
  - : Stilizzare una tabella HTML non è il lavoro più affascinante del mondo, ma a volte tutti dobbiamo farlo. Questo articolo spiega come far apparire bene le tabelle HTML, con alcune tecniche di styling delle tabelle specifiche evidenziate.
- [Debugging CSS](/it/docs/Learn_web_development/Core/Styling_basics/Debugging_CSS)
  - : Questo articolo ti darà indicazioni su come affrontare il debugging di un problema CSS, e ti mostrerà come i DevTools inclusi in tutti i browser moderni possono aiutarti a capire cosa sta succedendo.
- [Sfida: Comprensione fondamentale di CSS](/it/docs/Learn_web_development/Core/Styling_basics/Fundamental_CSS_comprehension) <sup>Sfida</sup>
  - : Questa sfida fornisce una serie di esercizi correlati che devono essere completati per creare il design finale — un biglietto da visita/carta del giocatore/profilo sui social media.
- [Sfida: Creare carta intestata elegante](/it/docs/Learn_web_development/Core/Styling_basics/Fancy_letterheaded_paper) <sup>Sfida</sup>
  - : Se vuole fare la giusta impressione, scrivere una lettera su una bella carta intestata può essere un ottimo inizio. In questa sfida creerà un modello online per ottenere tale aspetto.
- [Sfida: Una scatola dall'aspetto accattivante](/it/docs/Learn_web_development/Core/Styling_basics/Cool-looking_box) <sup>Sfida</sup>
  - : In questa sfida, avrà un po' più di pratica nel creare scatole dall'aspetto accattivante cercando di creare una scatola d'effetto.

## Tutorial aggiuntivi

Questi tutorial non fanno parte del percorso di apprendimento, ma sono comunque interessanti — dovrebbe considerarli come obiettivi aggiuntivi, da studiare facoltativamente quando ha terminato gli articoli principali del Core.

- [Effetti di stile avanzati](/it/docs/Learn_web_development/Core/Styling_basics/Advanced_styling_effects)
  - : Questo articolo funge da scatola di trucchi, offrendo un'introduzione ad alcune interessanti funzionalità di stile avanzate come ombre delle scatole, modalità di fusione e filtri.
- [Livelli di cascata](/it/docs/Learn_web_development/Core/Styling_basics/Cascade_layers)
  - : Questa lezione si propone di introdurla ai [livelli di cascata](/it/docs/Web/CSS/@layer), una funzionalità più avanzata che si basa sui concetti fondamentali della [cascata CSS](/it/docs/Web/CSS/CSS_cascade/Cascade) e [specificità CSS](/it/docs/Web/CSS/CSS_cascade/Specificity).
- [Gestire diverse direzioni del testo](/it/docs/Learn_web_development/Core/Styling_basics/Handling_different_text_directions)
  - : Negli ultimi anni, CSS si è evoluto per supportare meglio la diversa direzionalità del contenuto, inclusi i contenuti da destra a sinistra ma anche dall'alto verso il basso (come il giapponese) — queste diverse direzioni sono chiamate modalità di scrittura. Man mano che progredirai nei tuoi studi e comincerai a lavorare con i layout, una comprensione delle modalità di scrittura sarà molto utile, quindi le introdurremo in questo articolo.
- [Organizzare CSS](/it/docs/Learn_web_development/Core/Styling_basics/Organizing)
  - : Quando inizierà a lavorare su fogli di stile più grandi e progetti di grandi dimensioni, scoprirà che mantenere un grande file CSS può essere una sfida. In questo articolo esamineremo brevemente alcune delle migliori pratiche per scrivere il suo CSS in modo che sia facilmente gestibile, e alcune delle soluzioni che troverà in uso da altri per aiutare a migliorarne la gestibilità.

## Vedere anche

- [Impara HTML e CSS](https://scrimba.com/learn-html-and-css-c0p?via=mdn), Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Impara HTML e CSS_ di [Scrimba](https://scrimba.com/?via=mdn) le insegna HTML e CSS costruendo e distribuendo cinque fantastici progetti, con lezioni interattive divertenti e sfide insegnate da insegnanti esperti.
- [Scriva le sue prime righe di CSS!](https://scrimba.com/the-frontend-developer-career-path-c0j/~015?via=mdn), Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Questa lezione interattiva fornisce una utile introduzione alla sintassi CSS.

{{NextMenu("Learn_web_development/Core/Styling_basics/What_is_CSS", "Learn_web_development/Core")}}
