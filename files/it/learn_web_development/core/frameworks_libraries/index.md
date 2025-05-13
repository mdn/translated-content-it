---
title: JavaScript frameworks and libraries
slug: Learn_web_development/Core/Frameworks_libraries
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Introduction", "Learn_web_development/Core")}}

I framework JavaScript sono una parte essenziale dello sviluppo web front-end moderno, fornendo agli sviluppatori strumenti collaudati per costruire applicazioni web scalabili e interattive. Molte aziende moderne utilizzano i framework come parte standard degli strumenti, quindi molti lavori di sviluppo front-end ora richiedono esperienza con i framework. In questo set di articoli, miriamo a fornire un punto di partenza confortevole per aiutare a iniziare ad apprendere i framework.

Come aspirante sviluppatore front-end, può essere difficile capire da dove iniziare quando si apprendono i framework — ci sono così tanti framework tra cui scegliere, ne appaiono di nuovi tutto il tempo, lavorano per lo più in modo simile ma fanno alcune cose diversamente, e ci sono alcune cose specifiche di cui essere cauti quando si utilizzano i framework.

Il nostro obiettivo non è insegnarvi esaustivamente tutto ciò che c'è da sapere su React/ReactDOM, Vue o su qualche altro framework specifico; le documentazioni dei team che sviluppano i framework (e altre risorse) già svolgono questo compito. Invece, vogliamo prima rispondere a domande più fondamentali come:

- Perché dovrei usare un framework? Quali problemi risolvono per me?
- Quali domande dovrei pormi quando cerco di scegliere un framework? Ho veramente bisogno di usare un framework?
- Quali caratteristiche hanno i framework? Come funzionano in generale, e come differiscono le implementazioni di queste caratteristiche nei diversi framework?
- Come si relazionano con JavaScript o HTML "vanilla"?

Dopo di ciò, forniremo alcuni tutorial che coprono gli elementi essenziali di React, una scelta popolare di framework, per fornirvi abbastanza contesto e familiarità per iniziare ad approfondire autonomamente. Vogliamo che andiate avanti ed apprendiate i framework in un modo pragmatico che non dimentichi le pratiche fondamentali migliori della piattaforma web come l'accessibilità.

Forniamo anche alcuni tutorial che coprono le basi di altre scelte di framework, per coloro che vogliono optare per una scelta diversa da React.

## Prerequisiti

Dovreste veramente imparare le basi delle lingue web principali prima di tentare di passare all'apprendimento dei framework lato client — [HTML](/it/docs/Learn_web_development/Core/Structuring_content), [CSS](/it/docs/Learn_web_development/Core/Styling_basics), e soprattutto [JavaScript](/it/docs/Learn_web_development/Core/Scripting).

Il vostro codice sarà più ricco e professionale come risultato, e sarete in grado di risolvere i problemi con maggiore sicurezza se comprendete le caratteristiche fondamentali della piattaforma web su cui i framework si basano.

## Tutorial introduttivi

- [Introduzione ai framework lato client](/it/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
  - : Iniziamo il nostro sguardo sui framework con una panoramica generale dell'area, esaminando una breve storia di JavaScript e dei framework, perché i framework esistono e cosa ci offrono, come iniziare a pensare alla scelta di un framework da apprendere, e quali alternative ci sono ai framework lato client.
- [Caratteristiche principali dei framework](/it/docs/Learn_web_development/Core/Frameworks_libraries/Main_features)
  - : Ogni principale framework JavaScript ha un approccio diverso per aggiornare il DOM, gestire gli eventi del browser, e fornire un'esperienza di sviluppo piacevole. Questo articolo esplorerà le principali caratteristiche dei "big 4" framework, osservando come tendono a funzionare i framework a livello elevato e le differenze tra loro.

## Tutorial su React

> [!NOTE]
> I tutorial su React sono stati testati l'ultima volta a gennaio 2023, con React/ReactDOM 18.2.0 e create-react-app 5.0.1.
>
> Se ha bisogno di confrontare il suo codice con la nostra versione, può trovare una versione finita del codice dell'app di esempio React nel nostro [repository todo-react](https://github.com/mdn/todo-react). Per una versione dal vivo in esecuzione, veda <https://mdn.github.io/todo-react/>.

- [Iniziare con React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_getting_started)
  - : In questo articolo diremo ciao a React. Scopriremo un po' di dettagli sul suo background e casi d'uso, configureremo una toolchain React di base sul nostro computer locale, e creeremo e giocheremo con una semplice app iniziale, imparando un po' su come funziona React nel processo.
- [Iniziare la nostra app ToDo in React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning)
  - : Supponiamo di essere stati incaricati di creare un proof-of-concept in React – un'app che consente agli utenti di aggiungere, modificare e cancellare compiti che vogliono affrontare, e anche di contrassegnare i compiti come completati senza cancellarli. Questo articolo vi guiderà nel mettere in atto la struttura e lo stylesheet di base del componente `App`, pronti per la definizione dei singoli componenti e l'interattività, che aggiungeremo successivamente.
- [Compartmentalizzare la nostra app React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_components)
  - : A questo punto, la nostra app è un monolito. Prima che possiamo farla funzionare, dobbiamo scomporla in componenti gestibili e descrittivi. React non ha regole rigide per cosa è e cosa non è un componente — la scelta è vostra! In questo articolo, vi mostreremo un modo ragionevole per scomporre la nostra app in componenti.
- [Interattività in React: eventi e stato](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state)
  - : Con il nostro piano dei componenti elaborato, è ora di iniziare ad aggiornare la nostra app da un'UI completamente statica a una che ci permetta effettivamente di interagire e cambiare le cose. In questo articolo faremo questo, analizzando gli eventi e lo stato lungo il percorso.
- [Interattività in React: editing, filtering, rendering condizionale](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering)
  - : Mentre ci avviciniamo alla fine del nostro percorso React (almeno per ora), aggiungeremo gli ultimi ritocchi alle principali aree di funzionalità nella nostra app Lista di Todo. Questo include la possibilità di modificare i compiti esistenti e filtrare la lista dei compiti tra tutti, completati e incompleti. Lungo il percorso, esamineremo il rendering condizionale dell'UI.
- [Accessibilità in React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_accessibility)
  - : Nel nostro ultimo articolo tutorial, ci concentreremo (gioco di parole voluto) sull'accessibilità, inclusa la gestione del focus in React, che può migliorare l'usabilità e ridurre la confusione sia per gli utenti che usano solo la tastiera sia per quelli che usano i lettori di schermo.
- [Risorse React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_resources)
  - : Il nostro articolo finale vi fornisce una lista di risorse React che potete utilizzare per proseguire nel vostro apprendimento.

## Altre scelte di framework

### Tutorial Ember

> [!NOTE]
> I tutorial Ember sono stati testati l'ultima volta a maggio 2020, con Ember/Ember CLI versione 3.18.0.
>
> Se ha bisogno di confrontare il suo codice con la nostra versione, può trovare una versione finita del codice dell'app di esempio Ember nel [repository ember-todomvc-tutorial](https://github.com/NullVoxPopuli/ember-todomvc-tutorial/tree/master/steps/00-finished-todomvc/todomvc). Per una versione dal vivo in esecuzione, veda <https://nullvoxpopuli.github.io/ember-todomvc-tutorial/> (questo include anche alcune funzionalità aggiuntive non trattate nel tutorial).

- [Iniziare con Ember](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_getting_started)
  - : Nel nostro primo articolo su Ember, esamineremo come funziona Ember e a cosa serve, installeremo la toolchain Ember localmente, creeremo un'app di esempio, e poi faremo alcune configurazioni iniziali per prepararla allo sviluppo.
- [Struttura dell'app Ember e componentizzazione](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_structure_componentization)
  - : In questo articolo passeremo subito alla pianificazione della struttura della nostra app TodoMVC Ember, aggiungendo l'HTML e poi suddividendo quella struttura HTML in componenti.
- [Interattività Ember: eventi, classi e stato](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_interactivity_events_state)
  - : A questo punto inizieremo ad aggiungere un po' di interattività alla nostra app, fornendo la possibilità di aggiungere e visualizzare nuovi elementi todo. Durante il percorso, esamineremo l'uso degli eventi in Ember, la creazione di classi di componenti per contenere il codice JavaScript per controllare le funzionalità interattive, e la configurazione di un servizio per tenere traccia dello stato dei dati della nostra app.
- [Interattività Ember: funzionalità del footer, rendering condizionale](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_conditional_footer)
  - : Ora è il momento di iniziare a affrontare le funzionalità del footer nella nostra app. Qui faremo in modo che il contatore dei todo si aggiorni per mostrare il numero corretto di todo ancora da completare, e applicheremo la formattazione corretta ai todo completati (cioè, dove il checkbox è stato selezionato). Collegheremo anche il nostro pulsante "Clear completed". Durante il percorso, impareremo a usare il rendering condizionale nei nostri template.
- [Routing in Ember](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_routing)
  - : In questo articolo impariamo a conoscere il routing o il filtraggio basato su URL come talvolta viene chiamato. Lo useremo per fornire un URL unico per ognuna delle tre viste todo — "All", "Active" e "Completed".
- [Risorse e risoluzione dei problemi Ember](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_resources)
  - : Il nostro ultimo articolo su Ember vi fornisce una lista di risorse che potete utilizzare per proseguire nel vostro apprendimento, oltre a qualche utile risoluzione dei problemi e altre informazioni.

### Tutorial Vue

> [!NOTE]
> Il tutorial Vue è stato testato l'ultima volta a gennaio 2023, con Vue 3.2.45.
>
> Se ha bisogno di confrontare il suo codice con la nostra versione, può trovare una versione finita del codice dell'app di esempio Vue nel nostro [repository todo-vue](https://github.com/mdn/todo-vue). Per una versione dal vivo in esecuzione, veda <https://mdn.github.io/todo-vue/>.

- [Iniziare con Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_getting_started)
  - : Ora introduciamo Vue, il terzo dei nostri framework. In questo articolo, esamineremo un po' del background di Vue, impareremo come installarlo e creare un nuovo progetto, studieremo la struttura a livello alto dell'intero progetto e di un singolo componente, vedremo come eseguire il progetto localmente e prepararlo per iniziare a costruire il nostro esempio.
- [Creazione del nostro primo componente Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_first_component)
  - : Ora è ora di approfondire Vue e creare il nostro componente personalizzato — inizieremo creando un componente per rappresentare ogni elemento nella lista todo. Durante il percorso, impareremo alcuni concetti importanti come chiamare componenti all'interno di altri componenti, passare dati a loro tramite props e salvare lo stato dei dati.
- [Rendering di una lista di componenti Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_rendering_lists)
  - : A questo punto abbiamo un componente completamente funzionante; ora siamo pronti per aggiungere multipli componenti `ToDoItem` alla nostra app. In questo articolo esamineremo l'aggiunta di una serie di dati di elementi todo al nostro componente `App.vue`, che poi scorreremo e visualizzeremo all'interno dei componenti `ToDoItem` usando la direttiva `v-for`.
- [Aggiungere un nuovo modulo todolist: eventi Vue, metodi e modelli](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_methods_events_models)
  - : Ora abbiamo dati di esempio in atto e un ciclo che prende ogni bit di dati e li rende all'interno di un `ToDoItem` nella nostra app. Ciò di cui abbiamo realmente bisogno è la capacità di consentire ai nostri utenti di inserire i propri elementi todo nell'app, e per questo avremo bisogno di un `<input>` di testo, un evento da attivare quando i dati vengono inviati, un metodo da attivare al momento dell'invio per aggiungere i dati e rerendere la lista, e un modello per controllare i dati. Questo è ciò che copriremo in questo articolo.
- [Stilizzare i componenti Vue con CSS](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_styling)
  - : È finalmente arrivato il momento di rendere la nostra app più carina. In questo articolo, esploreremo i diversi modi di stilizzare i componenti Vue con CSS.
- [Utilizzare le proprietà calcolate Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_computed_properties)
  - : In questo articolo aggiungeremo un contatore che mostra il numero di elementi todo completati, utilizzando una caratteristica di Vue chiamata proprietà calcolate. Queste funzionano in modo simile ai metodi ma si riavviano solo quando una delle loro dipendenze cambia.
- [Rendering condizionale Vue: modificare i todo esistenti](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_conditional_rendering)
  - : Ora è il momento di aggiungere una delle principali parti di funzionalità che ci manca ancora — la capacità di modificare gli elementi todo esistenti. Per fare questo, sfrutteremo le capacità di rendering condizionale di Vue — in particolare `v-if` e `v-else` — per permetterci di passare dalla vista degli elementi todo esistenti a una vista di modifica dove è possibile aggiornare le etichette degli elementi todo. Esamineremo anche l'aggiunta di funzionalità per eliminare gli elementi todo.
- [Vue refs e metodi di ciclo di vita per la gestione del focus](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_refs_focus_management)
  - : Siamo quasi alla fine con Vue. L'ultimo pezzo di funzionalità da esaminare riguarda la gestione del focus, o, con altre parole, come possiamo migliorare l'accessibilità della nostra app con la tastiera. Esamineremo l'uso dei refs Vue per gestire questo — una caratteristica avanzata che consente di avere accesso diretto ai nodi DOM sottostanti al virtual DOM o accesso diretto da un componente alla struttura interna del DOM di un componente figlio.
- [Risorse Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_resources)
  - : Ora concludiamo il nostro studio su Vue fornendovi una lista di risorse che potete utilizzare per proseguire nel vostro apprendimento, oltre ad alcuni altri suggerimenti utili.

### Tutorial Svelte

> [!NOTE]
> I tutorial Svelte sono stati testati l'ultima volta ad agosto 2020, con Svelte 3.24.1.
>
> Se ha bisogno di confrontare il suo codice con la nostra versione, può trovare una versione finita del codice dell'app di esempio Svelte come dovrebbe essere dopo ogni articolo, nel nostro [repository mdn-svelte-tutorial](https://github.com/opensas/mdn-svelte-tutorial). Per una versione dal vivo in esecuzione, veda il nostro Svelte REPL a <https://svelte.dev/repl/378dd79e0dfe4486a8f10823f3813190?version=3.23.2>.

- [Iniziare con Svelte](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_getting_started)
  - : In questo articolo forniremo una rapida introduzione al [framework Svelte](https://svelte.dev/). Vedremo come funziona Svelte e cosa lo distingue dagli altri framework e strumenti che abbiamo visto finora. Poi impareremo come configurare il nostro ambiente di sviluppo, creare un'app di esempio, comprendere la struttura del progetto e vedere come eseguirlo localmente e costruirlo per la produzione.
- [Iniziare la nostra app Todo list in Svelte](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_Todo_list_beginning)
  - : Ora che abbiamo una comprensione di base di come funzionano le cose in Svelte, possiamo iniziare a costruire la nostra app di esempio: una lista todo. In questo articolo esamineremo prima la funzionalità desiderata della nostra app, quindi creeremo un componente `Todos.svelte` e metteremo in atto il markup statico e gli stili, lasciando tutto pronto per iniziare a sviluppare le funzionalità della nostra app To-Do list, che affronteremo nei successivi articoli.
- [Comportamento dinamico in Svelte: lavorare con variabili e props](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_variables_props)
  - : Ora che abbiamo il nostro markup e i nostri stili pronti, possiamo iniziare a sviluppare le funzionalità richieste per la nostra app Svelte To-Do list. In questo articolo useremo variabili e props per rendere la nostra app dinamica, consentendoci di aggiungere e eliminare todo, concluderli e filtrarli per stato.
- [Componente la nostra app Svelte](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_components)
  - : L'obiettivo centrale di questo articolo è esaminare come scomporre la nostra app in componenti gestibili e condividere informazioni tra di essi. Componentizzeremo la nostra app, quindi aggiungeremo ulteriore funzionalità per consentire agli utenti di aggiornare i componenti esistenti.
- [Svelte avanzato: Reattività, ciclo di vita, accessibilità](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility)
  - : In questo articolo aggiungeremo le funzionalità finali dell'app e ulteriormente componentizzeremo la nostra app. Impareremo come affrontare i problemi di reattività relativi all'aggiornamento di oggetti e array. Per evitare errori comuni, dovremo immergerci un po' più a fondo nel sistema di reattività di Svelte. Esamineremo anche la risoluzione di alcuni problemi di focus per l'accessibilità e altro ancora.
- [Lavorare con i negozi Svelte](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_stores)
  - : In questo articolo mostreremo un altro modo di gestire la gestione dello stato in Svelte — [Negozi](https://learn.svelte.dev/tutorial/writable-stores). I negozi sono repository di dati globali che contengono valori. I componenti possono iscriversi ai negozi e ricevere notifiche quando i loro valori cambiano.
- [Supporto TypeScript in Svelte](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript)
  - : Ora impareremo come usare TypeScript nelle applicazioni Svelte. Prima impareremo cos'è TypeScript e quali benefici può portare. Quindi vedremo come configurare il nostro progetto per lavorare con file TypeScript. Infine, esamineremo la nostra app e vedremo quali modifiche dobbiamo apportare per sfruttare appieno le funzionalità di TypeScript.
- [Deployment e prossimi passi](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_deployment_next)
  - : In questo articolo finale esamineremo come distribuire la vostra applicazione e metterla online, e condivideremo anche alcune delle risorse che seguireste per continuare il vostro viaggio di apprendimento di Svelte.

### Tutorial Angular

> [!NOTE]
> I tutorial Angular sono stati testati l'ultima volta ad aprile 2021, con Angular CLI (NG) 11.2.5.

- [Iniziare con Angular](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_getting_started)
  - : In questo articolo esaminiamo cosa offre Angular, installiamo i prerequisiti e configuriamo un'app di esempio, e vediamo l'architettura di base di Angular.
- [Iniziare la nostra app lista di cose da fare in Angular](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_todo_list_beginning)
  - : A questo punto, siamo pronti per iniziare a creare la nostra applicazione di lista di cose da fare utilizzando Angular. L'applicazione finita visualizzerà un elenco di elementi di cose da fare e includerà funzionalità di modifica, eliminazione e aggiunta. In questo articolo conoscerete la struttura della vostra applicazione e lavorerete fino a visualizzare un elenco base di elementi di cose da fare.
- [Stilizzare la nostra app Angular](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_styling)
  - : Ora che abbiamo configurato la struttura base dell'applicazione e abbiamo iniziato a visualizzare qualcosa di utile, cambiamo marcia e dedichiamo un articolo a vedere come Angular gestisce la stilizzazione delle applicazioni.
- [Creare un componente per gli articoli](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_item_component)
  - : I componenti forniscono un modo per organizzare la vostra applicazione. Questo articolo vi guida attraverso la creazione di un componente per gestire gli articoli individuali nella lista, e ad aggiungere funzionalità di controllo, modifica e cancellazione. Qui viene trattato il modello di eventi Angular.
- [Filtrare gli elementi della nostra lista di cose da fare](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_filtering)
  - : Ora passiamo ad aggiungere funzionalità per consentire agli utenti di filtrare i loro elementi di cose da fare, in modo che possano visualizzare gli articoli attivi, completati o tutti.
- [Costruire applicazioni Angular e ulteriori risorse](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_building)
  - : Questo articolo finale su Angular copre come costruire un'app pronta per la produzione e fornisce ulteriori risorse per continuare il vostro viaggio di apprendimento.

## Quali framework abbiamo scelto?

Copriamo cinque framework nei nostri tutorial — Angular, Ember, React/ReactDOM, Svelte e Vue:

- Sono scelte popolari che rimarranno in circolazione per un po' — come per qualsiasi strumento software, è buona cosa attenersi a scelte attivamente sviluppate che probabilmente non verranno interrotte la prossima settimana e che saranno aggiunte desiderabili al vostro set di competenze quando si cerca un lavoro.
- Hanno comunità forti e buona documentazione. È molto importante poter ottenere aiuto quando si impara un argomento complesso, specialmente quando si è agli inizi.
- Non abbiamo le risorse per coprire _tutti_ i framework moderni. Quella lista sarebbe comunque molto difficile da mantenere aggiornata, poiché ne appaiono di nuovi continuamente.
- Come un principiante, cercare di scegliere su cosa concentrarsi tra il numero enorme di scelte disponibili è un problema molto reale. Mantenere la lista corta è quindi utile.

Vogliamo dirlo in anticipo — **non** abbiamo scelto i framework su cui ci concentriamo perché pensiamo che siano i migliori, o perché li approviamo in qualche modo. Pensiamo solo che abbiano un punteggio alto riguardo ai criteri sopra menzionati.

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Introduction", "Learn_web_development/Core")}}
