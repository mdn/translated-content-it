---
title: Framework e librerie JavaScript
slug: Learn_web_development/Core/Frameworks_libraries
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Introduction", "Learn_web_development/Core")}}

I framework JavaScript sono una parte essenziale dello sviluppo web front-end moderno, fornendo agli sviluppatori strumenti provati e testati per costruire applicazioni web interattive e scalabili. Molte aziende moderne utilizzano i framework come parte standard delle loro toolchain, quindi molti lavori di sviluppo front-end richiedono ora esperienza con i framework. In questo insieme di articoli, ci proponiamo di offrire un punto di partenza confortevole per aiutare a iniziare ad apprendere i framework.

Come sviluppatore front-end alle prime armi, può essere difficile capire da dove iniziare quando si imparano i framework — ci sono così tanti framework tra cui scegliere, ne compaiono di nuovi continuamente, lavorano per lo più in modo simile, ma fanno alcune cose in modo diverso, e ci sono alcune cose specifiche di cui avere cura quando si utilizzano i framework.

Non abbiamo l'obiettivo di insegnare in modo esaustivo tutto ciò che c'è da sapere su React/ReactDOM, o Vue, o qualche altro framework specifico; la documentazione dei team dei framework (e altre risorse) svolgono già questo compito. Invece, vogliamo fare un passo indietro e rispondere prima a domande più fondamentali come:

- Perché dovrei usare un framework? Quali problemi risolvono per me?
- Quali domande dovrei pormi quando cerco di scegliere un framework? Ho realmente bisogno di usare un framework?
- Quali caratteristiche possiedono i framework? Come funzionano in generale e in che modo le implementazioni di queste caratteristiche differiscono tra i framework?
- Come si relazionano con JavaScript o HTML "vanilla"?

Dopo di ciò, forniremo alcuni tutorial sulle basi di React, una scelta popolare di framework, per fornire contesto e familiarità sufficiente a iniziare ad approfondire autonomamente. Vogliamo che si possa andare avanti e imparare i framework in modo pragmatico, senza dimenticare le migliori pratiche fondamentali della piattaforma web come l'accessibilità.

Forniremo anche alcuni tutorial che coprono le basi di altre scelte di framework, per coloro che desiderano fare una scelta diversa rispetto a React.

## Prerequisiti

Dovresti davvero imparare prima le basi dei linguaggi web fondamentali prima di tentare di passare ad apprendere i framework lato client — [HTML](/it/docs/Learn_web_development/Core/Structuring_content), [CSS](/it/docs/Learn_web_development/Core/Styling_basics), e in particolare [JavaScript](/it/docs/Learn_web_development/Core/Scripting).

Il tuo codice sarà più ricco e professionale di conseguenza, e sarai in grado di risolvere i problemi con più sicurezza se comprenderai le caratteristiche fondamentali della piattaforma web su cui i framework si basano.

## Tutorial introduttivi

- [Introduzione ai framework lato client](/it/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
  - : Iniziamo il nostro sguardo ai framework con una panoramica generale dell'area, osservando una breve storia di JavaScript e dei framework, perché i framework esistono e cosa ci offrono, come iniziare a pensare di scegliere un framework da imparare, e quali sono le alternative ai framework lato client.
- [Caratteristiche principali del framework](/it/docs/Learn_web_development/Core/Frameworks_libraries/Main_features)
  - : Ogni principale framework JavaScript ha un approccio diverso per l'aggiornamento del DOM, la gestione degli eventi del browser e la fornitura di un'esperienza di sviluppo piacevole. Questo articolo esplorerà le caratteristiche principali dei "big 4" framework, esaminando il funzionamento dei framework a un alto livello e le differenze tra di essi.

## Tutorial su React

> [!NOTE]
> Ultimo test dei tutorial su React a gennaio 2023, con React/ReactDOM 18.2.0 e create-react-app 5.0.1.
>
> Se hai bisogno di controllare il tuo codice rispetto alla nostra versione, puoi trovare una versione finale del codice dell'app React nel nostro [repository todo-react](https://github.com/mdn/todo-react). Per una versione live funzionante, vedi <https://mdn.github.io/todo-react/>.

- [Iniziare con React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_getting_started)
  - : In questo articolo faremo conoscenza con React. Scopriremo un po' di dettagli sul suo background e i casi d'uso, imposteremo una toolchain React di base sul nostro computer locale, e creeremo e giocheremo con un'app di partenza semplice, imparando un po' su come funziona React nel processo.
- [Iniziare la nostra app React ToDo](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning)
  - : Supponiamo che ci sia stato assegnato il compito di creare una prova di concetto in React - un'app che consente agli utenti di aggiungere, modificare e cancellare le attività su cui desiderano lavorare, e di segnare anche le attività come completate senza eliminarle. Questo articolo ti guiderà nella messa a punto della struttura e dello stile del componente `App` di base, pronta per la definizione e l'interattività dei singoli componenti, che aggiungeremo successivamente.
- [Componentizzazione della nostra app React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_components)
  - : A questo punto, la nostra app è un monolite. Prima di poterle far fare cose, dobbiamo smontarla in componenti gestibili e descrittivi. React non ha regole rigide su ciò che è e non è un componente — dipende da te! In questo articolo, mostreremo un modo ragionevole di suddividere la nostra app in componenti.
- [Interattività di React: Eventi e stato](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state)
  - : Con il nostro piano dei componenti elaborato, è arrivato il momento di iniziare ad aggiornare la nostra app da un'interfaccia utente completamente statica a una che consenta effettivamente di interagire e cambiare le cose. In questo articolo lo faremo, scavando nei dettagli con eventi e stato lungo il percorso.
- [Interattività di React: Modifica, filtri, rendering condizionale](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering)
  - : Mentre ci avviciniamo alla fine del nostro percorso React (almeno per adesso), aggiungeremo i tocchi finali alle aree principali della funzionalità nella nostra app della lista delle cose da fare. Questo include consentirti di modificare gestioni e filtrare la lista di attività tra tutte, completate e incomplete. Osserveremo il rendering dell'interfaccia utente condizionale lungo il percorso.
- [Accessibilità in React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_accessibility)
  - : Nel nostro articolo tutorial finale, ci concentreremo (gioco di parole voluto) sull'accessibilità, inclusa la gestione del focus in React, che può migliorare l'usabilità e ridurre la confusione sia per gli utenti solo tastiera che per i lettori di schermo.
- [Risorse su React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_resources)
  - : Il nostro articolo finale ti fornisce un elenco di risorse su React che puoi usare per approfondire il tuo apprendimento.

## Altre scelte di framework

### Tutorial su Ember

> [!NOTE]
> Ultimo test dei tutorial su Ember a maggio 2020, con Ember/Ember CLI versione 3.18.0.
>
> Se hai bisogno di controllare il tuo codice rispetto alla nostra versione, puoi trovare una versione finale del codice dell'app Ember di esempio nel [repository ember-todomvc-tutorial](https://github.com/NullVoxPopuli/ember-todomvc-tutorial/tree/master/steps/00-finished-todomvc/todomvc). Per una versione live funzionante, vedi <https://nullvoxpopuli.github.io/ember-todomvc-tutorial/> (questo include anche alcune funzionalità aggiuntive non trattate nel tutorial).

- [Iniziare con Ember](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_getting_started)
  - : Nel nostro primo articolo su Ember vedremo come funziona Ember e per cosa è utile, installeremo la toolchain di Ember localmente, creeremo un'app di esempio, e poi faremo una configurazione iniziale per renderla pronta per lo sviluppo.
- [Struttura dell'app Ember e componentizzazione](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_structure_componentization)
  - : In questo articolo partiremo subito con la pianificazione della struttura della nostra app Ember TodoMVC, aggiungendo l'HTML per essa, e poi suddividendo quella struttura HTML in componenti.
- [Interattività di Ember: Eventi, classi e stato](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_interactivity_events_state)
  - : A questo punto inizieremo ad aggiungere un po' di interattività alla nostra app, fornendo la possibilità di aggiungere e visualizzare nuove voci delle cose da fare. Lungo il percorso, esamineremo l'utilizzo degli eventi in Ember, la creazione di classi di componenti per contenere il codice JavaScript per controllare le funzionalità interattive, e l'impostazione di un servizio per tenere traccia dello stato dei dati della nostra app.
- [Interattività di Ember: Funzionalità del footer, rendering condizionale](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_conditional_footer)
  - : Ora è il momento di iniziare a affrontare la funzionalità del footer nella nostra app. Qui faremo in modo che il contatore delle cose da fare si aggiorni per mostrare il numero corretto di cose ancora da completare, e applichiamo correttamente lo stile alle cose completate (vale a dire, dove la casella di controllo è stata selezionata). Collegheremo anche il nostro pulsante "Clear completed". Lungo il percorso, impareremo a utilizzare il rendering condizionale nei nostri template.
- [Routing in Ember](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_routing)
  - : In questo articolo impareremo il routing o il filtraggio basato su URL come a volte viene chiamato. Lo utilizzeremo per fornire un URL univoco per ciascuna delle tre visualizzazioni delle cose da fare — "Tutte", "Attive" e "Completate".
- [Risorse su Ember e risoluzione dei problemi](/it/docs/Learn_web_development/Core/Frameworks_libraries/Ember_resources)
  - : Il nostro articolo finale su Ember ti fornisce un elenco di risorse che puoi usare per approfondire il tuo apprendimento, oltre ad alcune informazioni utili sulla risoluzione dei problemi e altre.

### Tutorial su Vue

> [!NOTE]
> Ultimo test del tutorial su Vue a gennaio 2023, con Vue 3.2.45.
>
> Se hai bisogno di controllare il tuo codice rispetto alla nostra versione, puoi trovare una versione finale del codice dell'app Vue di esempio nel nostro [repository todo-vue](https://github.com/mdn/todo-vue). Per una versione live funzionante, vedi <https://mdn.github.io/todo-vue/>.

- [Iniziare con Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_getting_started)
  - : Ora introduciamo Vue, il terzo dei nostri framework. In questo articolo, vedremo un po' di background di Vue, impareremo come installarlo e creare un nuovo progetto, studieremo la struttura ad alto livello dell'intero progetto e di un componente individuale, vedremo come eseguire il progetto localmente e prepararlo per iniziare a costruire il nostro esempio.
- [Creare il primo componente Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_first_component)
  - : Ora è il momento di approfondire Vue e creare il nostro componente personalizzato — inizieremo creando un componente per rappresentare ciascun elemento nella lista delle cose da fare. Lungo il percorso, impareremo alcuni concetti importanti come invocare componenti all'interno di altri componenti, passare dati attraverso props e salvare lo stato dei dati.
- [Rendering di una lista di componenti Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_rendering_lists)
  - : A questo punto, abbiamo un componente completamente funzionante; siamo ora pronti ad aggiungere più componenti `ToDoItem` alla nostra app. In questo articolo, esamineremo come aggiungere un set di dati delle cose da fare al nostro componente `App.vue`, che poi verranno iterate e visualizzate all'interno dei componenti `ToDoItem` utilizzando la direttiva `v-for`.
- [Aggiungere un nuovo modulo delle cose da fare: Eventi, metodi e modelli Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_methods_events_models)
  - : Ora abbiamo dati di esempio in posizione e un ciclo che prende ogni dato e lo rende dentro un `ToDoItem` nella nostra app. Ciò di cui abbiamo veramente bisogno è la possibilità per i nostri utenti di inserire le proprie attività dell'elenco "da fare" nell'app, e per questo avremo bisogno di un testo `<input>`, un evento da attivare quando i dati vengono inviati, un metodo da chiamare al momento dell'invio per aggiungere i dati e rimodellare la lista, e un modello per controllare i dati. È ciò che copriremo in questo articolo.
- [Stilizzare i componenti Vue con CSS](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_styling)
  - : Il momento è finalmente giunto per rendere la nostra app un po' più bella. In questo articolo esploreremo i diversi modi di stilizzare i componenti Vue con CSS.
- [Utilizzo delle proprietà calcolate Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_computed_properties)
  - : In questo articolo aggiungeremo un contatore che visualizza il numero di attività completate, utilizzando una caratteristica di Vue chiamata proprietà calcolate. Queste funzionano in modo simile ai metodi, ma vengono eseguite nuovamente solo quando una delle loro dipendenze cambia.
- [Rendering condizionale Vue: modificare le cose da fare esistenti](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_conditional_rendering)
  - : Ora è il momento di aggiungere una delle principali caratteristiche che ci mancano — la possibilità di modificare le attività dell'elenco già esistenti. Per fare ciò, sfrutteremo le capacità del rendering condizionale di Vue — specificamente `v-if` e `v-else` — per consentire di passare dalla visualizzazione dell'elemento attività esistente a una visualizzazione di modifica dove è possibile aggiornare le etichette delle cose da fare. Guarderemo anche a come aggiungere funzionalità per eliminare le attività dell'elenco.
- [Riferimenti Vue e metodi di ciclo di vita per la gestione del focus](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_refs_focus_management)
  - : Siamo quasi alla fine con Vue. L'ultimo pezzo di funzionalità da esaminare è la gestione del focus, o in altre parole, come possiamo migliorare l'accessibilità del nostro app tramite tastiera. Guarderemo all'utilizzo dei riferimenti Vue per gestire questo — una caratteristica avanzata che consente di avere accesso diretto ai nodi DOM sottostanti sotto il virtual DOM, oppure accesso diretto dal un componente alla struttura DOM interna di un componente figlio.
- [Risorse su Vue](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_resources)
  - : Ora concluderemo il nostro studio su Vue fornendoti un elenco di risorse che puoi utilizzare per approfondire il tuo apprendimento, oltre ad alcuni altri utili suggerimenti.

### Tutorial su Svelte

> [!NOTE]
> Ultimo test dei tutorial su Svelte ad agosto 2020, con Svelte 3.24.1.
>
> Se hai bisogno di controllare il tuo codice rispetto alla nostra versione, puoi trovare una versione finale del codice dell'app Svelte di esempio, come dovrebbe essere dopo ciascun articolo, nel nostro repository [mdn-svelte-tutorial](https://github.com/opensas/mdn-svelte-tutorial). Per una versione live funzionante, vedi il nostro Svelte REPL su <https://svelte.dev/repl/378dd79e0dfe4486a8f10823f3813190?version=3.23.2>.

- [Iniziare con Svelte](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_getting_started)
  - : In questo articolo forniremo una rapida introduzione al [framework Svelte](https://svelte.dev/). Vedremo come funziona Svelte e cosa lo distingue dagli altri framework e strumenti che abbiamo visto finora. Poi impareremo a configurare il nostro ambiente di sviluppo, creare un'app di esempio, capire la struttura del progetto, e vedere come eseguirlo localmente e prepararlo per la produzione.
- [Avviare la nostra app della lista delle cose da fare in Svelte](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_Todo_list_beginning)
  - : Ora che abbiamo una comprensione di base di come funzionano le cose in Svelte, possiamo iniziare a costruire la nostra app di esempio: una lista delle cose da fare. In questo articolo prima daremo uno sguardo alla funzionalità desiderata della nostra app, poi creeremo un componente `Todos.svelte` e metteremo in atto il markup e lo stile statico, lasciando tutto pronto per iniziare a sviluppare le caratteristiche della nostra app della lista delle cose da fare, che proseguiremo nei successivi articoli.
- [Comportamento dinamico in Svelte: lavorare con variabili e props](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_variables_props)
  - : Ora che abbiamo il markup e gli stili pronti, possiamo iniziare a sviluppare le funzionalità richieste per la nostra app della lista delle cose da fare in Svelte. In questo articolo useremo variabili e props per rendere la nostra app dinamica, consentendo di aggiungere ed eliminare elementi da fare, segnarli come completi e filtrarli in base allo stato.
- [Componentizzare la nostra app Svelte](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_components)
  - : L'obiettivo centrale di questo articolo è esaminare come suddividere la nostra app in componenti gestibili e condividere le informazioni tra di essi. Componentizzeremo la nostra app, quindi aggiungeremo più funzionalità per consentire agli utenti di aggiornare i componenti esistenti.
- [Svelte avanzato: Reattività, ciclo di vita, accessibilità](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility)
  - : In questo articolo aggiungeremo le funzionalità finali dell'app e ulteriori componenti alla nostra app. Impareremo come affrontare i problemi di reattività relativi all'aggiornamento di oggetti e array. Per evitare insidie comuni, dovremo scavare un po' più a fondo nel sistema di reattività di Svelte. Osserveremo anche come risolvere alcuni problemi di messa a fuoco per l'accessibilità e altro ancora.
- [Lavorare con i negozi Svelte](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_stores)
  - : In questo articolo mostreremo un altro modo per gestire la gestione dello stato in Svelte — [Stores](https://learn.svelte.dev/tutorial/writable-stores). I negozi sono repository di dati globali che contengono valori. I componenti possono iscriversi ai negozi e ricevere notifiche quando i loro valori cambiano.
- [Supporto TypeScript in Svelte](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript)
  - : Ora impareremo come utilizzare TypeScript nelle applicazioni Svelte. Innanzitutto, impareremo cos'è TypeScript e quali benefici può portare. Quindi vedremo come configurare il nostro progetto per lavorare con i file TypeScript. Infine esamineremo la nostra app per vedere quali modifiche dobbiamo apportare per sfruttare appieno le funzionalità di TypeScript.
- [Distribuzione e prossimi passi](/it/docs/Learn_web_development/Core/Frameworks_libraries/Svelte_deployment_next)
  - : In questo articolo finale, guarderemo a come distribuire la tua applicazione e metterla online, e condivideremo alcune delle risorse a cui dovresti continuare per proseguire il tuo percorso di apprendimento con Svelte.

### Tutorial su Angular

> [!NOTE]
> Ultimo test dei tutorial su Angular ad aprile 2021, con Angular CLI (NG) 11.2.5.

- [Iniziare con Angular](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_getting_started)
  - : In questo articolo vediamo cosa offre Angular, installiamo i prerequisiti e configuriamo un'app di esempio, e osserviamo l'architettura di base di Angular.
- [Iniziare la nostra app della lista delle cose da fare in Angular](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_todo_list_beginning)
  - : A questo punto, siamo pronti per iniziare a creare la nostra applicazione della lista delle cose da fare usando Angular. L'app finale visualizzerà una lista di elementi delle cose da fare e include funzionalità di modifica, eliminazione e aggiunta. In questo articolo conoscerai la struttura della tua applicazione, e arriverai fino a visualizzare una lista di base degli elementi delle cose da fare.
- [Stilizzare la nostra app Angular](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_styling)
  - : Ora che abbiamo impostato la struttura base dell'applicazione e avviato la visualizzazione di qualcosa di utile, cambiamo marcia e dedichiamo un articolo sull'analisi di come Angular gestisce lo stile delle applicazioni.
- [Creare un componente elemento](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_item_component)
  - : I componenti forniscono un modo per organizzare la tua applicazione. Questo articolo ti guida nella creazione di un componente per gestire gli elementi individuali nella lista, e nell'aggiunta di funzionalità di check, modifica ed eliminazione. Il modello di evento di Angular viene trattato qui.
- [Filtrare i nostri elementi della lista delle cose da fare](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_filtering)
  - : Ora passiamo all'aggiunta di funzionalità che consentano agli utenti di filtrare i propri elementi della lista delle cose da fare, in modo da poter visualizzare elementi attivi, completati o tutti.
- [Creare applicazioni Angular e ulteriori risorse](/it/docs/Learn_web_development/Core/Frameworks_libraries/Angular_building)
  - : Questo ultimo articolo su Angular copre come costruire un'app pronta per la produzione, e fornisce ulteriori risorse per continuare il tuo percorso di apprendimento.

## Quali framework abbiamo scelto?

Copriamo cinque framework nei nostri tutorial — Angular, Ember, React/ReactDOM, Svelte e Vue:

- Sono scelte popolari che saranno presenti per un po' — come con qualsiasi strumento software, è bene attenersi a scelte in attivo sviluppo che verosimilmente non verranno dismesse la prossima settimana e che saranno aggiunte desiderabili al tuo set di competenze quando cerchi un lavoro.
- Hanno comunità forti e buona documentazione. È molto importante essere in grado di ricevere aiuto nell'apprendimento di un argomento complesso, specialmente quando si sta appena iniziando.
- Non abbiamo le risorse per coprire _tutti_ i framework moderni. Quell'elenco sarebbe molto difficile da mantenere aggiornato comunque, poiché ne appaiono di nuovi tutto il tempo.
- Come principiante, cercare di scegliere su cosa concentrarsi tra il gran numero di scelte disponibili è un problema molto reale. Mantenere l'elenco breve è quindi utile.

Vogliamo dirlo chiaramente subito — non abbiamo **scelto** i framework su cui ci concentriamo perché pensiamo che siano i migliori, o perché li sosteniamo in alcun modo. Semplicemente pensiamo che abbiano un punteggio alto sui criteri sopra menzionati.

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Introduction", "Learn_web_development/Core")}}
