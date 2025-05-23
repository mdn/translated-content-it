---
title: Lavorare con i negozi di Svelte
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_stores
l10n:
  sourceCommit: eaec5c4226ac64696a95314a7bce995165a4d124
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility","Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript", "Learn_web_development/Core/Frameworks_libraries")}}

Nell'ultimo articolo abbiamo completato lo sviluppo della nostra app, finito di organizzarla in componenti e discusso alcune tecniche avanzate per gestire la reattività, lavorare con i nodi DOM ed esporre la funzionalità dei componenti. In questo articolo mostreremo un altro modo per gestire la gestione dello stato in Svelte: i [negozi](https://learn.svelte.dev/tutorial/writable-stores). I negozi sono repository di dati globali che contengono valori. I componenti possono iscriversi ai negozi e ricevere notifiche quando i loro valori cambiano.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È raccomandato almeno avere familiarità con i linguaggi principali
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/riga di comando</a
          >.
        </p>
        <p>
          Avrai bisogno di un terminale con node e npm installati per compilare e costruire la tua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare ad usare i negozi di Svelte</td>
    </tr>
  </tbody>
</table>

Usando i negozi creeremo un componente `Alert` che mostra notifiche sullo schermo, che possono ricevere messaggi da qualsiasi componente. In questo caso, il componente `Alert` è indipendente dal resto — non è un genitore o figlio di nessun altro — così i messaggi non si inseriscono nella gerarchia dei componenti.

Vedremo anche come sviluppare un nostro negozio personalizzato per mantenere le informazioni delle attività nel [web storage](/it/docs/Web/API/Web_Storage_API), permettendo così alle nostre attività di persistere oltre i ricaricamenti della pagina.

## Codifica insieme a noi

### Git

Clona il repository GitHub (se non l'hai già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi, per ottenere lo stato attuale dell'app, esegui

```bash
cd mdn-svelte-tutorial/06-stores
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/06-stores
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità sviluppo.

### REPL

Per codificare insieme a noi usando il REPL, inizia da

<https://svelte.dev/repl/d1fa84a5a4494366b179c87395940039?version=3.23.2>

## Gestione dello stato della nostra app

Abbiamo già visto come i nostri componenti possano comunicare tra di loro usando props, binding dei dati bidirezionale ed eventi. In tutti questi casi ci stavamo occupando della comunicazione tra componenti genitori e figli.

Ma non tutto lo stato dell'applicazione appartiene alla gerarchia dei componenti dell'applicazione. Per esempio, informazioni sull'utente connesso, o se il tema scuro è selezionato o meno.

A volte, lo stato della tua app avrà bisogno di essere accessibile da più componenti che non sono gerarchicamente correlati, o da un modulo JavaScript regolare.

Inoltre, quando la tua app diventa complicata e la tua gerarchia di componenti diventa complessa, potrebbe diventare troppo difficile per i componenti trasmettere i dati tra loro. In tal caso, passare a un negozio di dati globale potrebbe essere una buona opzione. Se hai già lavorato con [Redux](https://redux.js.org/) o [Vuex](https://vuex.vuejs.org/), allora sarai familiare con il modo in cui funziona questo tipo di negozio. I negozi di Svelte offrono funzionalità simili per la gestione dello stato.

Un negozio è un oggetto con un metodo `subscribe()` che consente alle parti interessate di essere notificate ogni volta che il valore del negozio cambia e un metodo opzionale `set()` che consente di impostare nuovi valori per il negozio. Questa API minima è conosciuta come [store contract](https://svelte.dev/docs/svelte-components#script-4-prefix-stores-with-$-to-access-their-values-store-contract).

Svelte fornisce funzioni per creare negozi [readable](https://svelte.dev/docs/svelte-store#readable), [writable](https://svelte.dev/docs/svelte-store#writable), e [derived](https://svelte.dev/docs/svelte-store#derived) nel modulo `svelte/store`.

Svelte offre anche un modo molto intuitivo per integrare i negozi nel suo sistema di reattività usando la [sintassi reattiva `$store`](https://svelte.dev/docs/svelte-components#script-4-prefix-stores-with-$-to-access-their-values). Se crei i tuoi negozi rispettando il contratto del negozio, ottieni questo zucchero sintattico di reattività gratuitamente.

## Creare il componente Alert

Per mostrare come lavorare con i negozi, creeremo un componente `Alert`. Questi tipi di widget possono anche essere conosciuti come notifiche popup, toast o bolle di notifica.

Il nostro componente `Alert` sarà mostrato dal componente `App`, ma qualsiasi componente potrà inviare notifiche ad esso. Ogni volta che una notifica arriva, il componente `Alert` sarà incaricato di mostrarla sullo schermo.

### Creare un negozio

Iniziamo creando un negozio writable. Qualsiasi componente sarà in grado di scrivere su questo negozio, e il componente `Alert` si iscriverà ad esso e mostrerà un messaggio ogni volta che il negozio viene modificato.

1. Crea un nuovo file, `stores.js`, all'interno della tua directory `src`.
2. Dagli il seguente contenuto:

   ```js
   import { writable } from "svelte/store";

   export const alert = writable("Welcome to the to-do list app!");
   ```

> [!NOTE]
> I negozi possono essere definiti e usati al di fuori dei componenti Svelte, quindi puoi organizzarli in qualsiasi modo desideri.

Nel codice sopra importiamo la funzione `writable()` da `svelte/store` e la usiamo per creare un nuovo negozio chiamato `alert` con un valore iniziale di "Benvenuto all'app lista delle attività!". Poi `esportiamo` il negozio.

### Creare il componente effettivo

Creiamo ora il nostro componente `Alert` e vediamo come possiamo leggere i valori dal negozio.

1. Crea un altro nuovo file chiamato `src/components/Alert.svelte`.
2. Dagli il seguente contenuto:

   ```svelte
   <script>
     import { alert } from '../stores.js'
     import { onDestroy } from 'svelte'

     let alertContent = ''

     const unsubscribe = alert.subscribe((value) => alertContent = value)

     onDestroy(unsubscribe)
   </script>

   {#if alertContent}
   <div on:click={() => alertContent = ''}>
     <p>{ alertContent }</p>
   </div>
   {/if}

   <style>
   div {
     position: fixed;
     cursor: pointer;
     margin-right: 1.5rem;
     margin-left: 1.5rem;
     margin-top: 1rem;
     right: 0;
     display: flex;
     align-items: center;
     border-radius: 0.2rem;
     background-color: #565656;
     color: #fff;
     font-weight: 700;
     padding: 0.5rem 1.4rem;
     font-size: 1.5rem;
     z-index: 100;
     opacity: 95%;
   }
   div p {
     color: #fff;
   }
   div svg {
     height: 1.6rem;
     fill: currentcolor;
     width: 1.4rem;
     margin-right: 0.5rem;
   }
   </style>
   ```

Diamo un'occhiata a questo pezzo di codice nel dettaglio.

- All'inizio importiamo il negozio `alert`.
- Successivamente importiamo la funzione di ciclo di vita `onDestroy()`, che ci permette di eseguire un callback dopo che il componente è stato smontato.
- Poi creiamo una variabile locale chiamata `alertContent`. Ricorda che possiamo accedere alle variabili di livello superiore dal markup, e ogni volta che vengono modificate, il DOM si aggiornerà di conseguenza.
- Quindi chiamiamo il metodo `alert.subscribe()`, passandogli una funzione di callback come parametro. Ogni volta che il valore del negozio cambia, la funzione di callback verrà chiamata con il nuovo valore come suo parametro. Nella funzione di callback semplicemente assegniamo il valore ricevuto alla variabile locale, il che attiverà l'aggiornamento del DOM del componente.
- Il metodo `subscribe()` restituisce anche una funzione di pulizia, che si occupa di rilasciare l'iscrizione. Quindi ci iscriviamo quando il componente viene inizializzato, e usiamo `onDestroy` per desabbrirci quando il componente viene smontato.
- Infine usiamo la variabile `alertContent` nel nostro markup, e se l'utente fa clic sull'alert lo puliamo.
- Alla fine includiamo alcune righe di CSS per stilizzare il nostro componente `Alert`.

Questa configurazione ci permette di lavorare con i negozi in modo reattivo. Quando il valore del negozio cambia, il callback viene eseguito. Lì assegnamo un nuovo valore a una variabile locale e, grazie alla reattività di Svelte, tutto il nostro markup e le dipendenze reattive vengono aggiornate di conseguenza.

### Usare il componente

Usiamo ora il nostro componente.

1. In `App.svelte` importeremo il componente. Aggiungi la seguente istruzione di importazione sotto quella esistente:

   ```js
   import Alert from "./components/Alert.svelte";
   ```

2. Poi chiama il componente `Alert` appena sopra la chiamata a `Todos`, in questo modo:

   ```svelte
   <Alert />
   <Todos {todos} />
   ```

3. Carica ora la tua app di prova, e dovresti ora vedere il messaggio `Alert` sullo schermo. Puoi fare clic su di esso per chiuderlo.

   ![Una semplice notifica nell'angolo in alto a destra di un'app che dice benvenuto all'app lista delle attività](01-alert-message.png)

## Rendere i negozi reattivi con la sintassi reattiva `$store`

Questo funziona, ma dovrai copiare e incollare tutto questo codice ogni volta che vuoi iscriverti a un negozio:

```svelte
<script>
  import myStore from "./stores.js";
  import { onDestroy } from "svelte";

  let myStoreContent = "";

  const unsubscribe = myStore.subscribe((value) => (myStoreContent = value));

  onDestroy(unsubscribe);
</script>

{myStoreContent}
```

È troppo boilerplate per Svelte! Essendo un compilatore, Svelte ha più risorse per rendere la nostra vita più facile. In questo caso Svelte fornisce la sintassi reattiva `$store`, anche conosciuta come auto-abbonamento. In termini semplici, basta prefissare il negozio con il segno `$` e Svelte genererà il codice per renderlo reattivo automaticamente. Quindi il nostro blocco di codice precedente può essere sostituito con questo:

```svelte
<script>
  import myStore from "./stores.js";
</script>

{$myStore}
```

E `$myStore` sarà completamente reattivo. Questo si applica anche ai tuoi negozi personalizzati. Se implementi i metodi `subscribe()` e `set()`, come faremo più avanti, la sintassi reattiva `$store` si applicherà anche ai tuoi negozi.

1. Applichiamo questo al nostro componente `Alert`. Aggiorna le sezioni `<script>` e markup di `Alert.svelte` come segue:

   ```svelte
   <script>
     import { alert } from '../stores.js'
   </script>

   {#if $alert}
   <div on:click={() => $alert = ''}>
     <p>{ $alert }</p>
   </div>
   {/if}
   ```

2. Controlla di nuovo la tua app e vedrai che funziona esattamente come prima. È molto meglio!

Dietro le quinte Svelte ha generato il codice per dichiarare la variabile locale `$alert`, iscriversi al negozio `alert`, aggiornare `$alert` ogni volta che il contenuto del negozio viene modificato e desabbrirsi quando il componente viene smontato. Genererà anche le istruzioni `alert.set()` ogni volta che assegniamo un valore a `$alert`.

Il risultato finale di questo trucco ingegnoso è che puoi accedere ai negozi globali proprio come se fossero variabili locali reattive.

Questo è un perfetto esempio di come Svelte pone il compilatore al servizio di una migliore ergonomia dello sviluppatore, non solo risparmiando tempo sulla digitazione del boilerplate, ma anche generando codice meno incline agli errori.

## Scrivere nel nostro negozio

Scrivere nel nostro negozio è solo questione di importarlo ed eseguire `$store = 'new value'`. Usciamolo nel nostro componente `Todos`.

1. Aggiungi la seguente istruzione `import` sotto quelle esistenti:

   ```js
   import { alert } from "../stores.js";
   ```

2. Aggiorna la tua funzione `addTodo()` come così:

   ```js
   function addTodo(name) {
     todos = [...todos, { id: newTodoId, name, completed: false }];
     $alert = `Todo '${name}' has been added`;
   }
   ```

3. Aggiorna `removeTodo()` come segue:

   ```js
   function removeTodo(todo) {
     todos = todos.filter((t) => t.id !== todo.id);
     todosStatus.focus(); // give focus to status heading
     $alert = `Todo '${todo.name}' has been deleted`;
   }
   ```

4. Aggiorna la funzione `updateTodo()` a questo:

   ```js
   function updateTodo(todo) {
     const i = todos.findIndex((t) => t.id === todo.id);
     if (todos[i].name !== todo.name)
       $alert = `todo '${todos[i].name}' has been renamed to '${todo.name}'`;
     if (todos[i].completed !== todo.completed)
       $alert = `todo '${todos[i].name}' marked as ${
         todo.completed ? "completed" : "active"
       }`;
     todos[i] = { ...todos[i], ...todo };
   }
   ```

5. Aggiungi il seguente blocco reattivo sotto il blocco che inizia con `let filter = 'all'`:

   ```js
   $: {
     if (filter === "all") {
       $alert = "Browsing all to-dos";
     } else if (filter === "active") {
       $alert = "Browsing active to-dos";
     } else if (filter === "completed") {
       $alert = "Browsing completed to-dos";
     }
   }
   ```

6. E infine per ora, aggiorna i blocchi `const checkAllTodos` e `const removeCompletedTodos` come segue:

   ```js
   const checkAllTodos = (completed) => {
     todos = todos.map((t) => ({ ...t, completed }));
     $alert = `${completed ? "Checked" : "Unchecked"} ${todos.length} to-dos`;
   };
   const removeCompletedTodos = () => {
     $alert = `Removed ${todos.filter((t) => t.completed).length} to-dos`;
     todos = todos.filter((t) => !t.completed);
   };
   ```

7. Quindi, fondamentalmente, abbiamo importato il negozio e lo abbiamo aggiornato su ogni evento, il che fa sì che un nuovo avviso venga mostrato ogni volta. Guarda di nuovo la tua app e prova ad aggiungere/eliminare/aggiornare alcune attività!

Appena eseguiamo `$alert = …`, Svelte eseguirà `alert.set()`. Il nostro componente `Alert` — come ogni sottoscrittore del negozio alert — sarà notificato quando riceve un nuovo valore e, grazie alla reattività di Svelte, il suo markup verrà aggiornato.

Potremmo fare lo stesso all'interno di qualsiasi componente o file `.js`.

> [!NOTE]
> Al di fuori dei componenti Svelte non puoi usare la sintassi `$store$. Questo perché il compilatore Svelte non toccherà nulla al di fuori dei componenti Svelte. In tal caso dovrai affidarti ai metodi `store.subscribe()`e`store.set()`.

## Migliorare il nostro componente Alert

È un po' fastidioso dover cliccare sull'alert per rimuoverlo. Sarebbe meglio se la notifica scomparisse dopo un paio di secondi.

Vediamo come fare. Specificheremo una prop con i millisecondi da attendere prima di rimuovere la notifica, e definiremo un timeout per rimuovere l'alert. Ci occuperemo anche di liberare il timeout quando il componente `Alert` viene smontato per prevenire perdite di memoria.

1. Aggiorna la sezione `<script>` del tuo componente `Alert.svelte` così:

   ```js
   import { onDestroy } from "svelte";
   import { alert } from "../stores.js";

   export let ms = 3000;
   let visible;
   let timeout;

   const onMessageChange = (message, ms) => {
     clearTimeout(timeout);
     if (!message) {
       // hide Alert if message is empty
       visible = false;
     } else {
       visible = true; // show alert
       if (ms > 0) timeout = setTimeout(() => (visible = false), ms); // and hide it after ms milliseconds
     }
   };
   $: onMessageChange($alert, ms); // whenever the alert store or the ms props changes run onMessageChange

   onDestroy(() => clearTimeout(timeout)); // make sure we clean-up the timeout
   ```

2. E aggiorna la sezione di markup di `Alert.svelte` come così:

   ```svelte
   {#if visible}
   <div on:click={() => visible = false}>
     <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M12.432 0c1.34 0 2.01.912 2.01 1.957 0 1.305-1.164 2.512-2.679 2.512-1.269 0-2.009-.75-1.974-1.99C9.789 1.436 10.67 0 12.432 0zM8.309 20c-1.058 0-1.833-.652-1.093-3.524l1.214-5.092c.211-.814.246-1.141 0-1.141-.317 0-1.689.562-2.502 1.117l-.528-.88c2.572-2.186 5.531-3.467 6.801-3.467 1.057 0 1.233 1.273.705 3.23l-1.391 5.352c-.246.945-.141 1.271.106 1.271.317 0 1.357-.392 2.379-1.207l.6.814C12.098 19.02 9.365 20 8.309 20z"/></svg>
     <p>{ $alert }</p>
   </div>
   {/if}
   ```

Qui creiamo prima la prop `ms` con un valore predefinito di 3000 (millisecondi). Poi creiamo una funzione `onMessageChange()` che si occuperà di controllare se l'Alert è visibile o meno. Con `$: onMessageChange($alert, ms)` diciamo a Svelte di eseguire questa funzione ogni volta che il negozio `$alert` o la prop `ms` cambiano.

Ogni volta che il negozio `$alert` cambia, liberiamo eventuali timeout in sospeso. Se `$alert` è vuoto, impostiamo `visible` su `false` e l'Alert sarà rimosso dal DOM. Se non è vuoto, impostiamo `visible` su `true` e utilizziamo la funzione `setTimeout()` per pulire l'alert dopo `ms` millisecondi.

Infine, con la funzione di ciclo di vita `onDestroy()`, ci assicuriamo di chiamare la funzione `clearTimeout()`.

Abbiamo anche aggiunto un'icona SVG sopra il paragrafo dell'alert, per renderlo più piacevole. Provalo di nuovo e dovresti vedere i cambiamenti.

## Rendere accessibile il nostro componente Alert

Il nostro componente `Alert` funziona bene, ma non è molto amichevole per le tecnologie assistive. Il problema è gli elementi che sono aggiunti e rimossi dinamicamente dalla pagina. Sebbene siano visivamente evidenti per gli utenti che possono vedere la pagina, potrebbero non essere così ovvi per gli utenti delle tecnologie assistive, come i lettori di schermo. Per gestire queste situazioni, possiamo sfruttare le [regioni ARIA live](/it/docs/Web/Accessibility/ARIA/Guides/Live_regions), che forniscono un modo per esporre programmaticamente i cambiamenti dinamici del contenuto affinché possano essere rilevati e annunciati dalle tecnologie assistive.

Possiamo dichiarare una regione che contiene contenuti dinamici che dovrebbero essere annunciati dalle tecnologie assistive con la proprietà `aria-live` seguita dall'impostazione di cortesia, che viene utilizzata per impostare la priorità con cui i lettori di schermo devono gestire gli aggiornamenti a quelle regioni. Le impostazioni possibili sono `off`, `polite` o `assertive`.

Per situazioni comuni, hai anche diversi valori di `role` predefiniti specializzati che possono essere utilizzati, come `log`, `status` e `alert`.

Nel nostro caso, basta aggiungere un `role="alert"` al contenitore `<div>` per risolvere il problema, così:

```svelte
<div role="alert" on:click={() => visible = false}>
```

In generale, testare le tue applicazioni utilizzando i lettori di schermo è una buona idea, non solo per scoprire problemi di accessibilità ma anche per abituarsi a come le persone con disabilità visive usano il Web. Hai diverse opzioni, come [NVDA](https://www.nvaccess.org/) per Windows, [ChromeVox](https://support.google.com/chromebook/answer/7031755) per Chrome, [Orca](https://wiki.gnome.org/Projects/Orca) su Linux e [VoiceOver](https://www.apple.com/accessibility/features/?vision) per macOS e iOS, tra le altre.

Per sapere di più su come rilevare e correggere i problemi di accessibilità, dai un'occhiata al nostro modulo [Accessibility](/it/docs/Learn_web_development/Core/Accessibility).

## Utilizzare il contratto del negozio per mantenere le nostre attività

La nostra piccola app ci permette di gestire le nostre attività piuttosto facilmente, ma è piuttosto inutile se otteniamo sempre lo stesso elenco di attività predefinite quando la ricarichiamo. Per renderla veramente utile, dobbiamo trovare un modo per mantenere le nostre attività.

Innanzitutto abbiamo bisogno di un modo per il nostro componente `Todos` di restituire le attività aggiornate al suo genitore. Potremmo emettere un evento aggiornato con l'elenco delle attività, ma è più facile semplicemente associare la variabile `todos`. Apriamo `App.svelte` e proviamoci.

1. Per prima cosa, aggiungi la seguente linea sotto il tuo array `todos`:

   ```js
   $: console.log("todos", todos);
   ```

2. Successivamente, aggiorna la chiamata al tuo componente `Todos` come segue:

   ```svelte
   <Todos bind:todos />
   ```

   > **Nota:** `<Todos bind:todos />` è solo una scorciatoia per `<Todos bind:todos={todos} />`.

3. Torna alla tua app, prova ad aggiungere alcune attività, poi vai alla console web del tuo strumenti per sviluppatori. Vedrai che ogni modifica che facciamo alle nostre attività si riflette nell'array `todos` definito in `App.svelte` grazie alla direttiva `bind`.

Ora dobbiamo trovare un modo per mantenere queste attività. Potremmo implementare del codice nel nostro componente `App.svelte` per leggere e salvare le nostre attività nel [web storage](/it/docs/Web/API/Web_Storage_API) o in un servizio web.
Ma non sarebbe meglio se potessimo sviluppare un negozio generico che ci permetta di mantenere il suo contenuto? Questo ci permetterebbe di usarlo proprio come qualsiasi altro negozio, e di astrarre il meccanismo di persistenza. Potremmo creare un negozio che sincronizza il suo contenuto con il web storage e, successivamente, svilupparne un altro che si sincronizza con un servizio web. Passare da uno all'altro sarebbe banale e non dovremmo toccare `App.svelte` per niente.

### Salvare le nostre attività

Quindi iniziamo usando un negozio writable regolare per salvare le nostre attività.

1. Apri il file `stores.js` e aggiungi il seguente negozio sotto quello esistente:

   ```js
   export const todos = writable([]);
   ```

2. È stato facile. Ora dobbiamo importare il negozio e usarlo in `App.svelte`. Ricorda solo che ora per accedere alle attività dobbiamo usare la sintassi `$todos` reattiva `$store`.

   Aggiorna il tuo file `App.svelte` così:

   ```svelte
   <script>
     import Todos from "./components/Todos.svelte";
     import Alert from "./components/Alert.svelte";

     import { todos } from "./stores.js";

     $todos = [
       { id: 1, name: "Create a Svelte starter app", completed: true },
       { id: 2, name: "Create your first component", completed: true },
       { id: 3, name: "Complete the rest of the tutorial", completed: false }
     ];
   </script>

   <Alert />
   <Todos bind:todos={$todos} />
   ```

3. Provalo; tutto dovrebbe funzionare. In seguito vedremo come definire i nostri negozi personalizzati.

### Come implementare un contratto di negozio: La teoria

Puoi creare i tuoi negozi senza affidarti a `svelte/store` implementando il contratto del negozio. Le caratteristiche devono funzionare in questo modo:

1. Un negozio deve contenere un metodo `subscribe()`, che deve accettare come argomento una funzione di iscrizione. Tutte le funzioni di iscrizione attive di un negozio devono essere chiamate ogni volta che il valore del negozio cambia.
2. Il metodo `subscribe()` deve restituire una funzione `unsubscribe()`, che quando chiamata deve interrompere la sua iscrizione.
3. Un negozio può opzionalmente contenere un metodo `set()`, che deve accettare come argomento un nuovo valore per il negozio e che sincronamente chiama tutte le funzioni di iscrizione attive del negozio. Un negozio con un metodo `set()` si chiama negozio writable.

Innanzitutto, aggiungiamo le seguenti istruzioni `console.log()` al nostro componente `App.svelte` per vedere il negozio `todos` e il suo contenuto in azione. Aggiungi queste righe sotto l'array `todos`:

```js
console.log("todos store - todos:", todos);
console.log("todos store content - $todos:", $todos);
```

Quando esegui l'app ora, vedrai qualcosa di simile a questo nella tua console web:

![console web che mostra le funzioni e il contenuto del negozio todos](02-svelte-store-in-action.png)

Come puoi vedere, il nostro negozio è semplicemente un oggetto che contiene metodi `subscribe()`, `set()`, e `update()`, e `$todos` è il nostro array di attività.

Solo per riferimento, ecco un negozio di base funzionante implementato da zero:

```js
export const writable = (initial_value = 0) => {
  let value = initial_value; // content of the store
  let subs = []; // subscriber's handlers

  const subscribe = (handler) => {
    subs = [...subs, handler]; // add handler to the array of subscribers
    handler(value); // call handler with current value
    return () => (subs = subs.filter((sub) => sub !== handler)); // return unsubscribe function
  };

  const set = (new_value) => {
    if (value === new_value) return; // same value, exit
    value = new_value; // update value
    subs.forEach((sub) => sub(value)); // update subscribers
  };

  const update = (update_fn) => set(update_fn(value)); // update function

  return { subscribe, set, update }; // store contract
};
```

Qui dichiariamo `subs`, che è un array di iscritti. Nel metodo `subscribe()` aggiungiamo l'handler all'array `subs` e ritorniamo una funzione che, quando eseguita, rimuoverà l'handler dall'array.

Quando chiamiamo `set()`, aggiorniamo il valore del negozio e chiamiamo ciascun handler, passando il nuovo valore come parametro.

Di solito non implementi negozi da zero; invece useresti il negozio writable per creare [negozi personalizzati](https://learn.svelte.dev/tutorial/custom-stores) con logica specifica del dominio. Nell'esempio seguente creiamo un negozio di contatore, che ci permetterà solo di aggiungere uno al contatore o resettarne il valore:

```js
import { writable } from "svelte/store";
function myStore() {
  const { subscribe, set, update } = writable(0);

  return {
    subscribe,
    addOne: () => update((n) => n + 1),
    reset: () => set(0),
  };
}
```

Se la nostra app lista delle attività diventa troppo complessa, potremmo lasciare che il nostro negozio delle attività gestisca ogni modifica dello stato. Potremmo spostare tutti i metodi che modificano l'array `todo` (come `addTodo()`, `removeTodo()`, ecc.) dal nostro componente `Todos` al negozio. Se hai un posto unico dove tutte le modifiche allo stato vengono applicate, i componenti potrebbero semplicemente chiamare quei metodi per modificare lo stato dell'app e visualizzare in modo reattivo le informazioni esposte dal negozio. Avere un posto unico per gestire le modifiche dello stato rende più facile ragionare sul flusso di stato e individuare i problemi.

Svelte non ti costringe a organizzare la gestione dello stato in un modo specifico; fornisce solo gli strumenti per scegliere come gestirlo.

### Implementare il nostro negozio personalizzato di attività

La nostra app lista delle attività non è particolarmente complessa, quindi non sposteremo tutti i nostri metodi di modifica in un posto centrale. Li lasceremo così come sono e ci concentreremo invece a mantenere le nostre attività.

> [!NOTE]
> Se stai seguendo questa guida lavorando dal REPL di Svelte, non potrai completare questo passaggio. Per motivi di sicurezza, il REPL di Svelte funziona in un ambiente sandbox che non ti permetterà di accedere al web storage e riceverai un errore "The operation is insecure". Per seguire questa sezione, dovrai clonare il repository e andare nella cartella `mdn-svelte-tutorial/06-stores`, oppure puoi scaricare direttamente il contenuto della cartella con `npx degit opensas/mdn-svelte-tutorial/06-stores`.

Per implementare un negozio personalizzato che salva il suo contenuto nel web storage, avremo bisogno di un negozio writable che faccia quanto segue:

- Legge inizialmente il valore dal web storage e, se non è presente, lo inizializza con un valore predefinito
- Ogni volta che il valore viene modificato, aggiorna il negozio stesso e anche i dati nel web storage

Inoltre, dato che il web storage supporta solo il salvataggio di valori di stringa, dovremo convertire da oggetto a stringa quando salviamo, e viceversa quando carichiamo il valore dal web storage.

1. Crea un nuovo file chiamato `localStore.js`, nella tua directory `src`.
2. Dagli il seguente contenuto:

   ```js
   import { writable } from "svelte/store";

   export const localStore = (key, initial) => {
     // receives the key of the local storage and an initial value

     const toString = (value) => JSON.stringify(value, null, 2); // helper function
     const toObj = JSON.parse; // helper function

     if (localStorage.getItem(key) === null) {
       // item not present in local storage
       localStorage.setItem(key, toString(initial)); // initialize local storage with initial value
     }

     const saved = toObj(localStorage.getItem(key)); // convert to object

     const { subscribe, set, update } = writable(saved); // create the underlying writable store

     return {
       subscribe,
       set: (value) => {
         localStorage.setItem(key, toString(value)); // save also to local storage as a string
         return set(value);
       },
       update,
     };
   };
   ```

   - Il nostro `localStore` sarà una funzione che, quando eseguita, legge inizialmente il suo contenuto dal web storage, e restituisce un oggetto con tre metodi: `subscribe()`, `set()`, e `update()`.
   - Quando creiamo un nuovo `localStore`, dovremo specificare la chiave del web storage e un valore iniziale. Poi controlliamo se il valore esiste nel web storage e, se no, lo creiamo.
   - Utilizziamo i metodi [`localStorage.getItem(key)`](/it/docs/Web/API/Storage/getItem) e [`localStorage.setItem(key, value)`](/it/docs/Web/API/Storage/setItem) per leggere e scrivere le informazioni nel web storage, e le funzioni di supporto [`toString()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Object/toString) e `toObj()` (che utilizza [`JSON.parse()`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)) per convertire i valori.
   - Successivamente, convertiamo il contenuto della stringa ricevuto dal web storage in un oggetto e salviamo tale oggetto nel nostro negozio.
   - Infine, ogni volta che aggiorniamo i contenuti del negozio, aggiorniamo anche il web storage, con il valore convertito in una stringa.

   Nota che abbiamo dovuto solo ridefinire il metodo `set()`, aggiungendo l'operazione per salvare il valore nel web storage. Il resto del codice è principalmente inizializzazione e conversione.

3. Ora useremo il nostro negozio locale da `stores.js` per creare il nostro negozio di attività mantenute localmente.

   Aggiorna `stores.js` così:

   ```js
   import { writable } from "svelte/store";
   import { localStore } from "./localStore.js";

   export const alert = writable("Welcome to the to-do list app!");

   const initialTodos = [
     { id: 1, name: "Visit MDN web docs", completed: true },
     { id: 2, name: "Complete the Svelte Tutorial", completed: false },
   ];

   export const todos = localStore("mdn-svelte-todo", initialTodos);
   ```

   Usando `localStore('mdn-svelte-todo', initialTodos)`, stiamo configurando il negozio per salvare i dati nel web storage sotto la chiave `mdn-svelte-todo`. Impostiamo anche un paio di attività come valori iniziali.

4. Ora liberiamoci delle attività predefinite in `App.svelte`. Aggiorna il suo contenuto come segue. Stiamo fondamentalmente solo eliminando l'array `$todos` e le istruzioni `console.log()`:

   ```svelte
   <script>
     import Todos from './components/Todos.svelte'
     import Alert from './components/Alert.svelte'

     import { todos } from './stores.js'
   </script>

   <Alert />
   <Todos bind:todos={$todos} />
   ```

   > [!NOTE]
   > Questo è l'unico cambiamento che dobbiamo fare per usare il nostro negozio personalizzato. `App.svelte` è completamente trasparente riguardo al tipo di negozio che stiamo usando.

5. Vai avanti e prova di nuovo la tua app. Crea alcune attività e poi chiudi il browser. Puoi anche interrompere il server Svelte e riavviarlo. Alla successiva visita dell'URL, le tue attività saranno ancora lì.
6. Puoi anche ispezionarlo nella Console DevTools. Nella console web, inserisci il comando `localStorage.getItem('mdn-svelte-todo')`. Fai alcune modifiche alla tua app, come premere il pulsante _Uncheck All_, e controlla di nuovo il contenuto del web storage. Otterrai qualcosa del genere:

   ![app lista delle attività con vista della console web accanto, mostrando che quando un'attività viene modificata nell'app, la corrispondente voce viene modificata nel web storage](03-persisting-todos-to-local-storage.png)

I negozi di Svelte forniscono un modo molto semplice e leggero, ma estremamente potente, per gestire lo stato complesso dell'applicazione da un negozio di dati globale in modo reattivo. E poiché Svelte compila il nostro codice, può fornire la [sintassi di auto-abbonamento `$store`](https://svelte.dev/docs/svelte-components#script-4-prefix-stores-with-$-to-access-their-values) che ci consente di lavorare con i negozi nello stesso modo delle variabili locali. Poiché i negozi hanno un'API minima, è molto semplice creare i nostri negozi personalizzati per astrarre il funzionamento interno del negozio stesso.

## Traccia bonus: Transizioni

Cambiamo argomento ora e facciamo qualcosa di divertente e diverso: aggiungiamo un'animazione ai nostri avvisi. Svelte fornisce un intero modulo per definire [transition](https://learn.svelte.dev/tutorial/transition) e [aminations](https://learn.svelte.dev/tutorial/animate) in modo da poter rendere le nostre interfacce utente più attraenti.

Una transizione viene applicata con la direttiva [transition:fn](https://svelte.dev/docs/element-directives#transition-fn), e viene attivata da un elemento che entra o esce dal DOM come risultato di un cambiamento di stato.

Diamo al nostro componente `Alert` una transizione `fly`. Apriremo il file `Alert.svelte` e importeremo la funzione `fly` dal modulo `svelte/transition`.

1. Metti la seguente istruzione `import` sotto quelle esistenti:

   ```js
   import { fly } from "svelte/transition";
   ```

2. Per usarla, aggiorna il tuo tag `<div>` di apertura così:

   ```svelte
   <div role="alert" on:click={() => visible = false}
     transition:fly
   >
   ```

   Le transizioni possono anche ricevere parametri, come questo:

   ```svelte
   <div role="alert" on:click={() => visible = false}
     transition:fly="\{{delay: 250, duration: 300, x: 0, y: -100, opacity: 0.5}}"
   >
   ```

   > [!NOTE]
   > Le doppie parentesi graffe non sono una sintassi speciale di Svelte. È solo un oggetto letterale JavaScript passato come parametro alla transizione fly.

3. Prova di nuovo la tua app e vedrai che le notifiche ora sembrano un po' più attraenti.

> [!NOTE]
> Essendo un compilatore, Svelte può ottimizzare la dimensione del nostro bundle escludendo le caratteristiche che non vengono utilizzate. In questo caso, se compiliamo la nostra app per la produzione con `npm run build`, il nostro file `public/build/bundle.js` peserà poco meno di 22 KB. Se rimuoviamo la direttiva `transitions:fly`, Svelte è abbastanza intelligente da rendersi conto che la funzione fly non viene utilizzata e la dimensione del file `bundle.js` scenderà a soli 18 KB.

Questo è solo la punta dell'iceberg. Svelte ha molte opzioni per gestire animazioni e transizioni. Svelte supporta anche la specificazione di diverse transizioni da applicare quando l'elemento viene aggiunto o rimosso dal DOM con le direttive `in:fn`/`out:fn`, e permette anche di definire le proprie [transizioni CSS personalizzate](https://learn.svelte.dev/tutorial/custom-css-transitions) e [transizioni JavaScript personalizzate](https://learn.svelte.dev/tutorial/custom-js-transitions). Ha anche diverse funzioni di easing per specificare il tasso di cambiamento nel tempo. Dai un'occhiata al [visore di easing](https://svelte.dev/examples/easing) per esplorare le varie funzioni di easing disponibili.

## Il codice finora

### Git

Per vedere lo stato del codice come dovrebbe essere alla fine di questo articolo, accedi alla tua copia del nostro repo così:

```bash
cd mdn-svelte-tutorial/07-next-steps
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/07-next-steps
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità sviluppo.

### REPL

Per vedere lo stato attuale del codice in un REPL, visita:

<https://svelte.dev/repl/378dd79e0dfe4486a8f10823f3813190?version=3.23.2>

## Riepilogo

In questo articolo abbiamo aggiunto due nuove funzionalità: un componente `Alert` e la persistenza delle `todos` nel web storage.

- Questo ci ha permesso di mostrare alcune tecniche avanzate di Svelte. Abbiamo sviluppato il componente `Alert` per mostrare come implementare la gestione dello stato tra componenti usando i negozi. Abbiamo anche visto come auto-abbonarsi ai negozi per integrarli senza problemi con il sistema di reattività di Svelte.
- Poi abbiamo visto come implementare il nostro negozio da zero e anche come estendere il negozio writable di Svelte per mantenere i dati nel web storage.
- Alla fine abbiamo dato un'occhiata all'utilizzo della direttiva `transition` di Svelte per implementare le animazioni sugli elementi del DOM.

Nel prossimo articolo impareremo come aggiungere il supporto TypeScript alla nostra applicazione Svelte. Per sfruttare tutte le sue funzionalità, porteremo anche la nostra intera applicazione su TypeScript.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility","Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript", "Learn_web_development/Core/Frameworks_libraries")}}
