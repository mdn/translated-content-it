---
title: Framework e librerie JavaScript
slug: Learn_web_development/Core/Frameworks_libraries
l10n:
  sourceCommit: 0915a5e602d475bd1a1a57d905f0bac1b7ed57b8
---

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Introduction", "Learn_web_development/Core")}}

I framework JavaScript sono una parte essenziale dello sviluppo moderno del web front-end, fornendo agli sviluppatori strumenti collaudati per costruire applicazioni web scalabili e interattive. Molte aziende moderne utilizzano i framework come parte standard dei loro strumenti, quindi molti lavori di sviluppo front-end ora richiedono esperienza con i framework. In questo insieme di articoli, miriamo a fornire un punto di partenza confortevole per aiutarti a iniziare a imparare i framework.

Come aspirante sviluppatore front-end, può essere difficile capire da dove iniziare quando si impara a utilizzare i framework — ci sono così tanti framework tra cui scegliere, ne appaiono di nuovi continuamente, funzionano per lo più in modo simile ma fanno alcune cose in modo diverso, e ci sono alcune cose specifiche di cui bisogna essere attenti quando si usano i framework.

Non miriamo a insegnarti in modo esaustivo tutto ciò che devi sapere su React/ReactDOM, Vue o su qualche altro specifico framework; la documentazione dei team dei framework (e altre risorse) svolge già questo compito. Invece, vogliamo ritornare e rispondere prima a domande più fondamentali come:

- Perché dovrei usare un framework? Quali problemi risolvono per me?
- Quali domande dovrei pormi quando cerco di scegliere un framework? Ho davvero bisogno di usare un framework?
- Quali caratteristiche hanno i framework? Come funzionano in generale e come differiscono le implementazioni di queste caratteristiche nei vari framework?
- Come si relazionano al JavaScript "vanilla" o HTML?

Dopo ciò, forniremo alcuni tutorial che coprono gli elementi essenziali di React, una scelta popolare di framework, per darti abbastanza contesto e familiarità per iniziare a esplorare più in profondità da te. Vogliamo che prosegui e impari sui framework in un modo pragmatico che non dimentichi le migliori pratiche fondamentali della piattaforma web come l'accessibilità.

Forniremo anche alcuni tutorial che coprono le basi di altre scelte di framework, per coloro che vogliono optare per una scelta diversa rispetto a React.

> [!NOTE]
> Il tutorial interattivo di Scrimba su [Libraries/Frameworks](https://scrimba.com/learn-react-c0e/~033a?via=mdn) <sup>[_Partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> fornisce un utile riassunto dei framework rispetto alle librerie, una breve storia delle librerie e dei framework sul web, e alcune informazioni di contesto specificamente su React.

## Prerequisiti

Dovresti davvero imparare prima le basi delle lingue core del web prima di tentare di passare all'apprendimento dei framework lato client — [HTML](/it/docs/Learn_web_development/Core/Structuring_content), [CSS](/it/docs/Learn_web_development/Core/Styling_basics), e soprattutto [JavaScript](/it/docs/Learn_web_development/Core/Scripting).

Il tuo codice sarà più ricco e professionale di conseguenza, e sarai in grado di risolvere i problemi con più sicurezza se comprendi le caratteristiche fondamentali della piattaforma web su cui i framework si basano.

## Tutorial introduttivi

- [Introduzione ai framework lato client](/it/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
  - : Cominciamo il nostro sguardo ai framework con una panoramica generale dell'area, esaminando una breve storia del JavaScript e dei framework, perché i framework esistono e cosa ci offrono, come iniziare a pensare alla scelta di un framework da apprendere, e quali alternative ci sono ai framework lato client.
- [Caratteristiche principali dei framework](/it/docs/Learn_web_development/Core/Frameworks_libraries/Main_features)
  - : Ogni framework JavaScript principale ha un approccio diverso all'aggiornamento del DOM, alla gestione degli eventi del browser e al fornire un'esperienza di sviluppo piacevole. Questo articolo esplorerà le caratteristiche principali dei "big 4" framework, esaminando come tendono a funzionare a un livello alto e le differenze tra di loro.

## Tutorial React

> [!NOTE]
> I tutorial di React sono stati testati l'ultima volta a gennaio 2023, con React/ReactDOM 18.2.0 e create-react-app 5.0.1.
>
> Se hai bisogno di controllare il tuo codice rispetto alla nostra versione, puoi trovare una versione finita del codice dell'app React di esempio nel nostro repository [todo-react](https://github.com/mdn/todo-react). Per una versione live in esecuzione, vedi <https://mdn.github.io/todo-react/>.

- [Iniziare con React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_getting_started)
  - : In questo articolo saluteremo React. Scopriremo un po' di dettagli sul suo background e sui casi d'uso, imposteremo una toolchain di base di React sul nostro computer locale, e creeremo e giocheremo con una semplice app iniziale, imparando un po' su come funziona React nel processo.
- [Iniziare la nostra app React ToDo](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning)
  - : Diciamo che siamo stati incaricati di creare un proof-of-concept in React – un'app che consente agli utenti di aggiungere, modificare e eliminare le attività su cui vogliono lavorare, e anche di contrassegnare le attività come completate senza eliminarle. Questo articolo ti guiderà nel mettere in atto la struttura e lo stile di base del componente `App`, pronto per la definizione e l'interattività del singolo componente, che aggiungeremo in seguito.
- [Componentizzare la nostra app React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_components)
  - : A questo punto, la nostra app è un monolite. Prima di poterla far eseguire funzioni, dobbiamo scomporla in componenti gestibili e descrittivi. React non ha regole rigide su cosa sia o non sia un componente – dipende da te! In questo articolo, ti mostreremo un modo sensato per suddividere la nostra app in componenti.
- [Interattività in React: Eventi e stato](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state)
  - : Con il nostro piano dei componenti delineato, è ora di iniziare ad aggiornare la nostra app da un'interfaccia utente completamente statica a una che ci consenta effettivamente di interagire e cambiare le cose. In questo articolo lo faremo, esaminando eventi e stato lungo il percorso.
- [Interattività in React: Modifica, filtraggio, rendering condizionale](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering)
  - : Mentre ci avviciniamo alla fine del nostro viaggio con React (almeno per ora), aggiungeremo i tocchi finali alle principali aree di funzionalità della nostra app della lista delle cose da fare. Questo include permetterti di modificare le attività esistenti e filtrare la lista delle attività tra tutte, completate e incompiute. Guarderemo anche al rendering condizionale dell'interfaccia utente lungo il percorso.
- [Accessibilità in React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_accessibility)
  - : Nel nostro articolo finale del tutorial, ci concentreremo (gioco di parole voluto) sull'accessibilità, inclusa la gestione del focus in React, che può migliorare l'usabilità e ridurre la confusione sia per gli utenti che utilizzano solo la tastiera sia per quelli che utilizzano lettori di schermo.
- [Risorse React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_resources)
  - : Il nostro articolo finale ti fornisce un elenco di risorse React che puoi usare per approfondire il tuo apprendimento.

## Altre scelte di framework

### Tutorial Ember

> [!NOTE]
> I tutorial di Ember sono stati testati l'ultima volta a maggio 2020, con la versione 3.18.0 di Ember/Ember CLI.
>
> Se hai bisogno di controllare il tuo codice rispetto alla nostra versione, puoi trovare una versione finita del codice dell'app di esempio Ember nel [repository ember-todomvc-tutorial](https://github.com/NullVoxPopuli/ember-todomvc-tutorial/tree/master/steps/00-finished-todomvc/todomvc). Per una versione live in esecuzione, vedi <https://nullvoxpopuli.github.io/ember-todomvc-tutorial/> (questo include anche alcune funzionalità aggiuntive non coperte nel tutorial).

- [Iniziare con Ember](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_getting_started)
  - : Nel nostro primo articolo su Ember esamineremo come funziona Ember e per cosa è utile, installeremo la toolchain di Ember localmente, creeremo un'app di esempio e poi faremo alcune configurazioni iniziali per prepararla allo sviluppo.
- [Struttura dell'app Ember e componentizzazione](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_structure_componentization)
  - : In questo articolo procederemo direttamente alla pianificazione della struttura della nostra app TodoMVC Ember, aggiungendo l'HTML per essa e poi suddividendo quella struttura HTML in componenti.
- [Interattività in Ember: Eventi, classi e stato](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_interactivity_events_state)
  - : A questo punto inizieremo ad aggiungere un po' di interattività alla nostra app, fornendo la possibilità di aggiungere e mostrare nuovi elementi da fare. Lungo il percorso, esamineremo l'uso degli eventi in Ember, la creazione di classi di componenti per contenere il codice JavaScript per controllare le caratteristiche interattive, e l'impostazione di un servizio per tenere traccia dello stato dei dati della nostra app.
- [Interattività in Ember: Funzionalità del footer, rendering condizionale](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_conditional_footer)
  - : Ora è il momento di iniziare a occuparsi delle funzionalità del footer nella nostra app. Qui faremo in modo che il conteggio delle attività da fare si aggiorni per mostrare il numero corretto di attività ancora da completare e si applichi correttamente lo stile alle attività completate (cioè, dove la casella di controllo è stata spuntata). Collegheremo anche il nostro pulsante "Clear completed". Lungo il percorso, impareremo a utilizzare il rendering condizionale nei nostri template.
- [Routing in Ember](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_routing)
  - : In questo articolo impariamo a conoscere il routing o il filtraggio basato su URL, come talvolta viene chiamato. Lo useremo per fornire un URL univoco per ciascuna delle tre visualizzazioni della lista delle cose da fare: "Tutte", "Attive" e "Completate".
- [Risorse e risoluzione dei problemi di Ember](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_resources)
  - : Il nostro articolo finale su Ember ti fornisce un elenco di risorse che puoi utilizzare per approfondire il tuo apprendimento, oltre a qualche informazione utile per risolvere problemi e altro ancora.

### Tutorial Vue

> [!NOTE]
> Il tutorial di Vue è stato testato l'ultima volta a gennaio 2023, con Vue 3.2.45.
>
> Se hai bisogno di controllare il tuo codice rispetto alla nostra versione, puoi trovare una versione finita del codice dell'app Vue di esempio nel nostro repository [todo-vue](https://github.com/mdn/todo-vue). Per una versione live in esecuzione, vedi <https://mdn.github.io/todo-vue/>.

- [Iniziare con Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_getting_started)
  - : Ora introduciamo Vue, il terzo dei nostri framework. In questo articolo, guarderemo un po' al background di Vue, impareremo come installarlo e creare un nuovo progetto, esamineremo la struttura di alto livello dell'intero progetto e di un singolo componente, vedremo come eseguire il progetto localmente e prepararlo per iniziare a costruire il nostro esempio.
- [Creare il nostro primo componente Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_first_component)
  - : Ora è il momento di approfondire Vue e creare il nostro componente personalizzato — inizieremo creando un componente per rappresentare ciascun elemento nella lista delle cose da fare. Lungo il percorso, impareremo alcuni concetti importanti come la chiamata di componenti all'interno di altri componenti, il passaggio di dati a loro tramite `props` e il salvataggio dello stato dei dati.
- [Rendering di una lista di componenti Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_rendering_lists)
  - : A questo punto abbiamo un componente completamente funzionante; siamo ora pronti ad aggiungere più componenti `ToDoItem` alla nostra app. In questo articolo esamineremo l'aggiunta di un set di dati degli elementi da fare al nostro componente `App.vue`, che poi scorreremo e visualizzeremo all'interno di `ToDoItem` usando la direttiva `v-for`.
- [Aggiungere un nuovo modulo di attività: eventi, metodi e modelli di Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_methods_events_models)
  - : Ora abbiamo dati di esempio in posizione e un ciclo che prende ogni pezzetto di dati e lo rende all'interno di un `ToDoItem` nella nostra app. Ciò di cui abbiamo veramente bisogno ora è la capacità di permettere ai nostri utenti di inserire nella app i propri elementi da fare, e per questo avremo bisogno di un `<input>` di testo, un evento che si attiva quando i dati vengono inviati, un metodo che si attiva al momento dell'invio per aggiungere i dati e rielaborare l'elenco, e un modello per controllare i dati. Questo è ciò che copriremo in questo articolo.
- [Stilizzare i componenti Vue con CSS](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_styling)
  - : È finalmente giunto il momento di rendere la nostra app un po' più piacevole da vedere. In questo articolo esploreremo i diversi modi di stilizzare i componenti Vue con CSS.
- [Usare le proprietà calcolate di Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_computed_properties)
  - : In questo articolo aggiungeremo un contatore che mostri il numero di elementi dell'elenco delle cose da fare completati, usando una funzionalità di Vue chiamata proprietà calcolate. Queste funzionano in modo simile ai metodi, ma vengono rieseguite solo quando cambia una delle loro dipendenze.
- [Rendering condizionale in Vue: modifica degli elenchi delle cose da fare esistenti](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_conditional_rendering)
  - : Ora è il momento di aggiungere una delle principali funzionalità che ci mancano ancora — la capacità di modificare gli elementi esistenti nell'elenco delle cose da fare. Per farlo, sfrutteremo le capacità di rendering condizionale di Vue — vale a dire `v-if` e `v-else` — per permetterci di alternare tra la visualizzazione dell'elemento dell'elenco delle cose da fare esistente e una visualizzazione di modifica in cui è possibile aggiornare le etichette degli elementi da fare. Guarderemo anche all'aggiunta della funzionalità per eliminare gli elementi dell'elenco delle cose da fare.
- [Riferimenti Vue e metodi di ciclo di vita per la gestione del focus](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_refs_focus_management)
  - : Siamo quasi alla fine con Vue. L'ultimo pezzo di funzionalità da esaminare è la gestione del focus, o in altre parole, come possiamo migliorare l'accessibilità della nostra app via tastiera. Guarderemo all'uso dei riferimenti di Vue per gestire questa funzionalità — una funzionalità avanzata che ti permette di avere accesso diretto ai nodi DOM sottostanti al DOM virtuale, o accesso diretto da un componente alla struttura DOM interna di un componente figlio.
- [Risorse Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_resources)
  - : Ora concluderemo il nostro studio su Vue fornendo un elenco di risorse che puoi usare per approfondire il tuo apprendimento, oltre a qualche altro utile consiglio.

### Tutorial Svelte

> [!NOTE]
> I tutorial di Svelte sono stati testati l'ultima volta ad agosto 2020, con Svelte 3.24.1.
>
> Se hai bisogno di controllare il tuo codice rispetto alla nostra versione, puoi trovare una versione finita del codice dell'app Svelte di esempio come dovrebbe essere dopo ciascun articolo nel nostro repository [mdn-svelte-tutorial](https://github.com/opensas/mdn-svelte-tutorial). Per una versione live in esecuzione, vedi la nostra Svelte REPL a <https://svelte.dev/repl/378dd79e0dfe4486a8f10823f3813190?version=3.23.2>.

- [Iniziare con Svelte](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_getting_started)
  - : In questo articolo forniamo una rapida introduzione al [framework Svelte](https://svelte.dev/). Vedremo come funziona Svelte e cosa lo distingue dal resto dei framework e degli strumenti che abbiamo visto finora. Poi impareremo come impostare il nostro ambiente di sviluppo, creare un'app di esempio, capire la struttura del progetto e vedere come eseguirlo localmente e costruirlo per la produzione.
- [Iniziare la nostra app della lista delle cose da fare con Svelte](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_Todo_list_beginning)
  - : Ora che abbiamo una comprensione di base di come funzionano le cose in Svelte, possiamo iniziare a costruire la nostra app di esempio: una lista delle cose da fare. In questo articolo esamineremo prima la funzionalità desiderata della nostra app, poi creeremo un componente `Todos.svelte` e metteremo in atto il markup statico e gli stili, lasciando tutto pronto per iniziare a sviluppare le funzionalità dell'app della lista delle cose da fare, che affronteremo negli articoli successivi.
- [Comportamento dinamico in Svelte: lavorare con variabili e props](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_variables_props)
  - : Ora che abbiamo pronti il nostro markup e gli stili possiamo iniziare a sviluppare le funzionalità richieste per la nostra app Svelte della lista delle cose da fare. In questo articolo utilizzeremo variabili e props per rendere la nostra app dinamica, permettendoci di aggiungere ed eliminare attività da fare, contrassegnarle come completate e filtrarle per stato.
- [Componentizzare la nostra app Svelte](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_components)
  - : L'obiettivo centrale di questo articolo è guardare come suddividere la nostra app in componenti gestibili e condividere informazioni tra di essi. Componentizzeremo la nostra app, quindi aggiungeremo più funzionalità per consentire agli utenti di aggiornare i componenti esistenti.
- [Svelte avanzato: reattività, ciclo di vita, accessibilità](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility)
  - : In questo articolo aggiungeremo le funzionalità finali dell'app e componentizzeremo ulteriormente la nostra app. Impareremo come affrontare i problemi di reattività legati all'aggiornamento di oggetti e array. Per evitare insidie comuni, dovremo approfondire un po' il sistema di reattività di Svelte. Guarderemo anche alla risoluzione di alcuni problemi di focus di accessibilità e altro ancora.
- [Lavorare con i negozi Svelte](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_stores)
  - : In questo articolo mostreremo un altro modo di gestire lo stato in Svelte — [Stores](https://learn.svelte.dev/tutorial/writable-stores). I negozi sono repository di dati globali che contengono valori. I componenti possono iscriversi ai negozi e ricevere notifiche quando i loro valori cambiano.
- [Supporto TypeScript in Svelte](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript)
  - : Ora impareremo a usare TypeScript nelle applicazioni Svelte. Prima vedremo cos'è TypeScript e quali vantaggi può portare. Poi vedremo come configurare il nostro progetto per lavorare con file TypeScript. Infine, esamineremo la nostra app e vedremo quali modifiche dobbiamo apportare per sfruttare appieno le funzionalità di TypeScript.
- [Distribuzione e passi successivi](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_deployment_next)
  - : In questo articolo finale vedremo come distribuire la tua applicazione e metterla online, e condivideremo anche alcune delle risorse che dovresti consultare per continuare il tuo viaggio di apprendimento di Svelte.

### Tutorial Angular

> [!NOTE]
> I tutorial di Angular sono stati testati l'ultima volta ad aprile 2021, con Angular CLI (NG) 11.2.5.

- [Iniziare con Angular](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_getting_started)
  - : In questo articolo esaminiamo cosa ha da offrire Angular, installiamo i prerequisiti e configuriamo un'app di esempio, e ci occupiamo dell'architettura di base di Angular.
- [Iniziare la nostra app della lista delle cose da fare di Angular](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_todo_list_beginning)
  - : A questo punto, siamo pronti a iniziare a creare la nostra applicazione della lista delle cose da fare utilizzando Angular. L'applicazione finita mostrerà un elenco di elementi da fare e includerà funzionalità di modifica, eliminazione e aggiunta. In questo articolo conoscerai la struttura della tua applicazione e arriverai a mostrare un elenco di base degli elementi da fare.
- [Stilizzare la nostra app Angular](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_styling)
  - : Ora che abbiamo preparato la nostra struttura di applicazione di base e abbiamo iniziato a mostrare qualcosa di utile, cambiamo marcia e dedichiamo un articolo a esaminare come Angular gestisce la stilizzazione delle applicazioni.
- [Creare un componente di elemento](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_item_component)
  - : I componenti forniscono un modo per organizzare la tua applicazione. Questo articolo ti guida nella creazione di un componente per gestire gli elementi individuali nell'elenco e aggiungere funzionalità di controllo, modifica ed eliminazione. Il modello di eventi di Angular è trattato qui.
- [Filtrare i nostri elementi da fare](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_filtering)
  - : Ora passiamo ad aggiungere funzionalità per consentire agli utenti di filtrare i propri elementi da fare, in modo che possano visualizzare elementi attivi, completati o tutti.
- [Costruire applicazioni Angular e altre risorse](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_building)
  - : Questo ultimo articolo su Angular tratta di come costruire un'app pronta per la produzione e fornisce ulteriori risorse per continuare il tuo viaggio di apprendimento.

## Quali framework abbiamo scelto?

Copriamo cinque framework nei nostri tutorial — Angular, Ember, React/ReactDOM, Svelte e Vue:

- Sono scelte popolari che saranno in circolazione per un po' — come con qualsiasi strumento software, è bene attenersi a scelte attivamente sviluppate che probabilmente non verranno interrotte la prossima settimana e che saranno aggiunte desiderabili al tuo set di competenze quando cerchi un lavoro.
- Hanno comunità forti e documentazione adeguata. È molto importante poter ottenere aiuto nell'apprendere un argomento complesso, soprattutto quando si è all'inizio.
- Non abbiamo le risorse per coprire _tutti_ i framework moderni. Quel elenco sarebbe comunque molto difficile da tenere aggiornato, poiché ne compaiono di nuovi continuamente.
- Come principiante, cercare di scegliere su cosa concentrarsi tra l'enorme numero di scelte disponibili è un problema molto reale. Mantenere l'elenco breve è quindi utile.

Vogliamo dirlo subito — non abbiamo **scelto** i framework su cui ci concentriamo perché pensiamo che siano i migliori, o perché li approviamo in alcun modo. Pensiamo solo che ottengano punteggi elevati sui criteri sopra indicati.

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Introduction", "Learn_web_development/Core")}}
