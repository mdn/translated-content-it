---
title: Concetti base dello stile CSS
slug: Learn_web_development/Core/Styling_basics
l10n:
  sourceCommit: 5f37fd46fc2408e6b646fe81d4964be7168be197
---

{{NextMenu("Learn_web_development/Core/Styling_basics/What_is_CSS", "Learn_web_development/Core")}}

CSS (Cascading Style Sheets) è utilizzato per stilizzare e impaginare le pagine web — ad esempio, per modificare il carattere, il colore, la dimensione e lo spazio del tuo contenuto, suddividerlo in più colonne o aggiungere animazioni ed altri elementi decorativi. Questo modulo fornisce tutti i fondamentali del CSS necessari per iniziare, includendo sintassi, funzionalità e tecniche.

## Prerequisiti

Prima di iniziare questo modulo, dovresti avere un ambiente di lavoro di base configurato (come descritto in [Installazione dei software di base](/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software)) e comprendere come creare e gestire file (come descritto in [Gestione dei file](/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files)). Dovresti anche avere familiarità con HTML (lavora attraverso il nostro modulo [Strutturare i contenuti con HTML](/it/docs/Learn_web_development/Core/Structuring_content) se non lo hai già fatto).

> [!NOTE]
> Se stai lavorando su un computer/tablet/altro dispositivo in cui non hai la possibilità di creare i tuoi file, potresti provare (la maggior parte) degli esempi di codice in un programma di codifica online come [JSBin](https://jsbin.com/) o [Glitch](https://glitch.com/).

## Tutorial e sfide

- [Cos'è il CSS?](/it/docs/Learn_web_development/Core/Styling_basics/What_is_CSS)
  - : CSS ti permette di creare pagine web dall'ottimo aspetto, ma come funziona internamente? Questo articolo spiega cos'è il CSS, come appare la sintassi di base e come il tuo browser applica il CSS all'HTML per stilizzarlo.
- [Iniziare con il CSS](/it/docs/Learn_web_development/Core/Styling_basics/Getting_started)
  - : In questo articolo, prenderemo un semplice documento HTML e vi applicheremo il CSS, apprendendo alcuni dettagli pratici della lingua lungo il percorso. Rivedremo anche le caratteristiche della sintassi CSS che non hai ancora visto.
- [Stilizzare una pagina biografica](/it/docs/Learn_web_development/Core/Styling_basics/Styling_a_bio_page) <sup>Sfida</sup>
  - : In questa sfida stilizzerai una semplice pagina biografica, mettendoti alla prova su alcune delle abilità apprese nelle ultime lezioni, inclusa la scrittura di selettori e lo stile del testo.
- [Selettori CSS di base](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors)
  - : In questo articolo riepilogheremo alcuni fondamenti dei selettori, inclusi i selettori di tipo base, classe e ID.
- [Selettori di attributo](/it/docs/Learn_web_development/Core/Styling_basics/Attribute_selectors)
  - : Come sai dal tuo studio dell'HTML, gli elementi possono avere attributi che forniscono ulteriori dettagli sull'elemento marcato. In CSS puoi utilizzare i selettori di attributo per mirare agli elementi con certi attributi. Questa lezione ti mostrerà come usare questi utilissimi selettori.
- [Pseudo-classi e pseudo-elementi](/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements)
  - : Il prossimo set di selettori che esamineremo sono denominati **pseudo-classi** e **pseudo-elementi**. Ce ne sono un gran numero, e spesso servono a scopi abbastanza specifici. Una volta che sai come usarli, puoi dare un'occhiata ai diversi tipi per vedere se ce n'è uno che funziona per il compito che stai cercando di raggiungere.
- [Combinatori](/it/docs/Learn_web_development/Core/Styling_basics/Combinators)
  - : Gli ultimi selettori che esamineremo sono chiamati combinatori. I combinatori sono usati per combinare altri selettori in modo tale da permetterci di selezionare elementi in base alla loro posizione nel DOM rispetto ad altri elementi (ad esempio, figlio o fratello).
- [Il modello a scatola](/it/docs/Learn_web_development/Core/Styling_basics/Box_model)
  - : Tutto in CSS ha una scatola attorno, e comprendere queste scatole è fondamentale per poter creare layout più complessi con CSS o per allineare elementi con altri elementi. In questa lezione, daremo un'occhiata al _modello a scatola_ del CSS. Avrai una comprensione di come funziona e della terminologia ad esso relativa.
- [Gestire i conflitti](/it/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts)
  - : L'obiettivo di questa lezione è sviluppare la tua comprensione di alcuni dei concetti più fondamentali del CSS — la cascata, la specificità, e l'ereditarietà — che controllano come il CSS viene applicato all'HTML e come i conflitti tra dichiarazioni di stile vengono risolti.
- [Valori e unità](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units)
  - : Le regole CSS contengono [dichiarazioni](/it/docs/Web/CSS/CSS_syntax/Syntax#css_declarations), che a loro volta sono composte da proprietà e valori. Ogni proprietà utilizzata in CSS ha un **tipo di valore** che descrive quale tipo di valori è consentito avere. In questa lezione, daremo un'occhiata ad alcuni dei tipi di valori più frequentemente utilizzati, cosa sono e come funzionano.
- [Dimensionamento degli elementi in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Sizing)
  - : Capire quanto saranno grandi le diverse caratteristiche del tuo design è importante. In questa lezione riassumeremo i vari modi in cui gli elementi ottengono una dimensione tramite CSS e definiremo alcuni termini sul dimensionamento che ti aiuteranno in futuro.
- [Sfondo e bordi](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders)
  - : In questa lezione esamineremo alcune delle cose creative che puoi fare con gli sfondi e i bordi CSS. Dall'aggiunta di gradienti, immagini di sfondo, e angoli arrotondati, gli sfondi e i bordi sono la risposta a molte domande di stile nel CSS.
- [Contenuto traboccante](/it/docs/Learn_web_development/Core/Styling_basics/Overflow)
  - : Il traboccamento è ciò che accade quando c'è troppo contenuto per entrare all'interno di una scatola di un elemento. In questa lezione imparerai come gestire il traboccamento utilizzando il CSS.
- [Immagini, media e elementi del modulo](/it/docs/Learn_web_development/Core/Styling_basics/Images_media_forms)
  - : In questa lezione daremo un'occhiata a come alcuni elementi speciali sono trattati in CSS. Le immagini, altri media, e gli elementi del modulo si comportano in modo leggermente diverso dalle normali scatole in termini di capacità di stilizzarli con CSS. Capire cosa è possibile e cosa no può evitare frustrazioni, e questa lezione evidenzierà alcune delle cose principali che è necessario conoscere.
- [Stilizzare tabelle](/it/docs/Learn_web_development/Core/Styling_basics/Tables)
  - : Stilizzare una tabella HTML non è il lavoro più glamoroso del mondo, ma a volte tutti dobbiamo farlo. Questo articolo spiega come far apparire bene le tabelle HTML, con alcune tecniche specifiche di stile delle tabelle messe in evidenza.
- [Debugging CSS](/it/docs/Learn_web_development/Core/Styling_basics/Debugging_CSS)
  - : Questo articolo fornirà indicazioni su come affrontare il debugging di un problema CSS, e mostrerà come i DevTools inclusi in tutti i browser moderni possono aiutarti a capire cosa sta succedendo.
- [Sfida: Comprensione fondamentale del CSS](/it/docs/Learn_web_development/Core/Styling_basics/Fundamental_CSS_comprehension) <sup>Sfida</sup>
  - : Questa sfida fornisce un numero di esercizi correlati che devono essere completati per creare il design finale — un biglietto da visita/un biglietto per giocatori/un profilo sui social media.
- [Sfida: Creare carta intestata elegante](/it/docs/Learn_web_development/Core/Styling_basics/Fancy_letterheaded_paper) <sup>Sfida</sup>
  - : Se vuoi fare una buona impressione, scrivere una lettera su della bella carta intestata può essere un ottimo inizio. In questa sfida creerai un template online per ottenere un tale aspetto.
- [Sfida: Una scatola dall'aspetto accattivante](/it/docs/Learn_web_development/Core/Styling_basics/Cool-looking_box) <sup>Sfida</sup>
  - : In questa sfida, avrai ancora un po' di pratica nel creare scatole dall'aspetto accattivante provando a creare una scatola che attiri l'attenzione.

## Tutorial aggiuntivi

Questi tutorial non fanno parte del percorso di apprendimento, ma sono comunque interessanti — dovresti considerarli come obiettivi estesi, da studiare facoltativamente quando hai terminato con gli articoli Core principali.

- [Effetti di stile avanzati](/it/docs/Learn_web_development/Core/Styling_basics/Advanced_styling_effects)
  - : Questo articolo funge da scatola dei trucchi, fornendo un'introduzione ad alcune interessanti caratteristiche di stile avanzate come ombre delle scatole, modalità di fusione, e filtri.
- [Layer della cascata](/it/docs/Learn_web_development/Core/Styling_basics/Cascade_layers)
  - : Questa lezione mira a introdurti ai [layer della cascata](/it/docs/Web/CSS/@layer), una caratteristica più avanzata che si basa sui concetti fondamentali della [cascata CSS](/it/docs/Web/CSS/CSS_cascade/Cascade) e della [specificità CSS](/it/docs/Web/CSS/CSS_cascade/Specificity).
- [Gestire diverse direzioni di testo](/it/docs/Learn_web_development/Core/Styling_basics/Handling_different_text_directions)
  - : Negli ultimi anni, CSS si è evoluto per supportare meglio le diverse direzionalità dei contenuti, inclusa quella da destra a sinistra ma anche quella dall'alto verso il basso (come il giapponese) — queste diverse direzionalità sono chiamate modalità di scrittura. Man mano che progredisci nei tuoi studi e inizi a lavorare con il layout, una comprensione delle modalità di scrittura ti sarà molto utile, quindi le introdurremo in questo articolo.
- [Organizzare il CSS](/it/docs/Learn_web_development/Core/Styling_basics/Organizing)
  - : Quando inizi a lavorare su fogli di stile più grandi e progetti di grandi dimensioni scoprirai che mantenere un enorme file CSS può essere una sfida. In questo articolo daremo un'occhiata breve ad alcune delle migliori pratiche per scrivere il tuo CSS in modo che sia facilmente gestibile, e alcune delle soluzioni che troverai in uso da altri per aiutare a migliorare la manutenibilità.

## Vedi anche

- [Imparare HTML e CSS](https://scrimba.com/learn-html-and-css-c0p?via=mdn), Scrimba <sup>[_MDN partner dell'apprendimento_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Imparare HTML e CSS_ di [Scrimba](https://scrimba.com/?via=mdn) ti insegna HTML e CSS attraverso la costruzione e la distribuzione di cinque progetti fantastici, con lezioni interattive divertenti e sfide insegnate da insegnanti esperti.
- [Scrivi le tue prime righe di CSS!](https://scrimba.com/the-frontend-developer-career-path-c0j/~015?via=mdn), Scrimba <sup>[_MDN partner dell'apprendimento_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Questa lezione interattiva fornisce una utile introduzione alla sintassi del CSS.

{{NextMenu("Learn_web_development/Core/Styling_basics/What_is_CSS", "Learn_web_development/Core")}}
