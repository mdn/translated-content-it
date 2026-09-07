---
title: Framework e librerie JavaScript
slug: Learn_web_development/Core/Frameworks_libraries
l10n:
  sourceCommit: 238b07dfeb8c347c590bd02a63140867525d511c
---

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Introduction", "Learn_web_development/Core")}}

I framework JavaScript sono una parte essenziale dello sviluppo web front-end moderno e forniscono agli sviluppatori strumenti collaudati per creare applicazioni web scalabili e interattive. Molte aziende moderne utilizzano i framework come parte standard dei propri strumenti, quindi molti lavori nello sviluppo front-end richiedono ormai esperienza con i framework. Questa serie di articoli offre un punto di partenza agevole per iniziare a imparare i framework.

Per un aspirante sviluppatore front-end, può essere difficile capire da dove iniziare nell'apprendimento dei framework: ce ne sono moltissimi tra cui scegliere, ne compaiono continuamente di nuovi, perlopiù funzionano in modo simile ma svolgono alcune attività in modo diverso e ci sono aspetti specifici a cui prestare attenzione durante l'uso dei framework.

Non intendiamo insegnare in modo esaustivo tutto ciò che serve sapere su React/ReactDOM o su qualsiasi altro framework specifico; la documentazione dei team che sviluppano i framework (e altre risorse) svolge già questo compito. Vogliamo invece fare un passo indietro e rispondere prima a domande più fondamentali, quali:

- Perché usare un framework? Quali problemi risolve?
- Quali domande porre quando si cerca di scegliere un framework? È davvero necessario usare un framework?
- Quali funzionalità hanno i framework? Come funzionano in generale e in cosa differiscono le implementazioni di queste funzionalità nei vari framework?
- Come si rapportano a JavaScript o HTML "vanilla"?

Successivamente, verranno forniti alcuni tutorial che trattano gli aspetti essenziali di React, una scelta di framework popolare, per offrire contesto e familiarità sufficienti a proseguire autonomamente con maggiore approfondimento. L'obiettivo è consentire di apprendere i framework in modo pragmatico, senza dimenticare le best practice fondamentali della piattaforma web, come l'accessibilità.

> [!NOTE]
> Il tutorial interattivo di Scrimba [Libraries/Frameworks](https://scrimba.com/learn-react-c0e/~033a?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> offre un utile riepilogo delle differenze tra framework e librerie, una breve storia delle librerie e dei framework sul web e alcune informazioni di contesto specifiche su React.

## Prerequisiti

Prima di passare all'apprendimento dei framework lato client, è opportuno imparare le basi dei linguaggi web principali: [HTML](/it/docs/Learn_web_development/Core/Structuring_content), [CSS](/it/docs/Learn_web_development/Core/Styling_basics) e soprattutto [JavaScript](/it/docs/Learn_web_development/Core/Scripting).

Il codice risulterà più ricco e professionale e sarà possibile risolvere i problemi con maggiore sicurezza se si comprendono le funzionalità fondamentali della piattaforma web su cui si basano i framework.

## Tutorial introduttivi

- [Introduzione ai framework lato client](/it/docs/Learn_web_development/Core/Frameworks_libraries/Introduction)
  - : Iniziamo l'analisi dei framework con una panoramica generale dell'argomento, esaminando una breve storia di JavaScript e dei framework, il motivo dell'esistenza dei framework e ciò che offrono, come iniziare a valutare quale framework imparare e quali alternative esistono ai framework lato client.
- [Funzionalità principali dei framework](/it/docs/Learn_web_development/Core/Frameworks_libraries/Main_features)
  - : Ogni principale framework JavaScript adotta un approccio diverso per l'aggiornamento del DOM, la gestione degli eventi del browser e l'offerta di un'esperienza piacevole per gli sviluppatori. Questo articolo esplorerà le funzionalità principali dei "quattro grandi" framework, esaminando in generale il funzionamento tipico dei framework e le differenze tra essi.

## Tutorial su React

> [!NOTE]
> I tutorial su React sono stati testati l'ultima volta a gennaio 2023, con React/ReactDOM 18.2.0 e create-react-app 5.0.1.
>
> Per verificare il codice rispetto alla nostra versione, è possibile trovare una versione completata del codice dell'app React di esempio nel nostro [repository todo-react](https://github.com/mdn/todo-react). Per una versione live in esecuzione, vedere <https://mdn.github.io/todo-react/>.

- [Introduzione a React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_getting_started)
  - : In questo articolo verrà presentato React. Verranno illustrati alcuni dettagli relativi al suo contesto e ai casi d'uso, verrà configurata una toolchain React di base sul computer locale e verrà creata e sperimentata una semplice app iniziale, imparando nel frattempo qualcosa sul funzionamento di React.
- [Iniziare l'app React ToDo](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning)
  - : Supponiamo di aver ricevuto l'incarico di creare una proof of concept in React: un'app che consenta agli utenti di aggiungere, modificare ed eliminare le attività su cui desiderano lavorare, nonché di contrassegnare le attività come completate senza eliminarle. Questo articolo guiderà nella realizzazione della struttura e dello stile di base del componente `App`, pronti per la definizione dei singoli componenti e dell'interattività, che verranno aggiunte in seguito.
- [Suddividere in componenti l'app React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_components)
  - : A questo punto, l'app è un monolite. Prima di poterle far svolgere delle attività, è necessario suddividerla in componenti gestibili e descrittivi. React non impone regole rigide su cosa sia o non sia un componente: la scelta spetta allo sviluppatore. In questo articolo verrà mostrato un modo ragionevole per suddividere l'app in componenti.
- [Interattività in React: eventi e stato](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state)
  - : Definito il piano dei componenti, è il momento di aggiornare l'app, trasformandola da un'interfaccia utente completamente statica in una che consenta effettivamente di interagire e modificare elementi. In questo articolo verrà fatto proprio questo, approfondendo nel frattempo eventi e stato.
- [Interattività in React: modifica, filtraggio, rendering condizionale](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering)
  - : Avvicinandosi alla fine del percorso su React, almeno per ora, verranno aggiunti gli ultimi ritocchi alle principali aree funzionali dell'app per elenchi di attività. Ciò include la possibilità di modificare le attività esistenti e filtrare l'elenco delle attività tra tutte, completate e incomplete. Verrà inoltre esaminato il rendering condizionale dell'interfaccia utente.
- [Accessibilità in React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_accessibility)
  - : Nell'ultimo articolo tutorial, l'attenzione sarà rivolta, letteralmente, all'accessibilità, inclusa la gestione del focus in React, che può migliorare l'usabilità e ridurre la confusione sia per gli utenti che usano solo la tastiera sia per quelli che usano screen reader.
- [Risorse su React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_resources)
  - : L'ultimo articolo fornisce un elenco di risorse su React che consentono di proseguire nell'apprendimento.

## Altre scelte di framework

Se non si desidera iniziare a imparare i framework usando React, è possibile scegliere un'altra opzione.

Come alternative, consigliamo di esaminare le seguenti:

- [Angular](https://angular.dev/): iniziare con il [tutorial su Angular](https://angular.dev/tutorials/learn-angular).
- [Ember](https://emberjs.com/): iniziare con la [documentazione Learning Ember.js](https://emberjs.com/learn/).
- [Svelte](https://svelte.dev/): iniziare con il [tutorial su Svelte](https://svelte.dev/tutorial/svelte/welcome-to-svelte).
- [Vue](https://vuejs.org/): iniziare con la [Guida rapida a Vue](https://vuejs.org/guide/quick-start.html).

È importante chiarirlo fin da subito: i framework menzionati in precedenza **non** sono stati scelti perché ritenuti i migliori o perché vengono in qualche modo sostenuti. Si ritiene semplicemente che ottengano ottimi risultati secondo i criteri seguenti, che dovrebbero essere considerati quando si inizia a investire tempo nell'apprendimento di nuovo software:

- Sono ben supportati e rimarranno disponibili a lungo: come per qualsiasi strumento software, è consigliabile scegliere opzioni sviluppate attivamente, che probabilmente non verranno interrotte la settimana successiva e che costituiranno aggiunte desiderabili alle proprie competenze durante la ricerca di lavoro.
- Hanno community solide e buona documentazione: poter ricevere aiuto nell'apprendimento di un argomento complesso è molto importante, soprattutto agli inizi.

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Introduction", "Learn_web_development/Core")}}
