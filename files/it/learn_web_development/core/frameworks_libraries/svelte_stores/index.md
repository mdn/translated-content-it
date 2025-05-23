---
title: Lavorare con gli store di Svelte
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_stores
l10n:
  sourceCommit: e6d43da6c6d28a6ac92cdd47882809ffbdf987ce
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility","Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript", "Learn_web_development/Core/Frameworks_libraries")}}

Nell'ultimo articolo abbiamo completato lo sviluppo della nostra app, finito di organizzarla in componenti e discusso alcune tecniche avanzate per affrontare la reattività, lavorare con i nodi DOM ed esporre la funzionalità dei componenti. In questo articolo mostreremo un altro modo per gestire la gestione dello stato in Svelte: [Stores](https://learn.svelte.dev/tutorial/writable-stores). Gli store sono repository di dati globali che contengono valori. I componenti possono sottoscriversi agli store e ricevere notifiche quando i loro valori cambiano.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È consigliato avere familiarità con i linguaggi di base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenze del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/riga di comando</a
          >.
        </p>
        <p>
          Sarà necessario un terminale con node e npm installati per compilare e costruire
          la tua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare a usare gli store di Svelte</td>
    </tr>
  </tbody>
</table>

Usando gli store, creeremo un componente `Alert` che mostra notifiche su schermo, che può ricevere messaggi da qualsiasi componente. In questo caso, il componente `Alert` è indipendente dal resto — non è un genitore o un figlio di nessun altro — quindi i messaggi non rientrano nella gerarchia dei componenti.

Vedremo anche come sviluppare il nostro store personalizzato per mantenere le informazioni del todo nella [memoria web](/it/docs/Web/API/Web_Storage_API), permettendo ai nostri to-do di persistere dopo i ricaricamenti della pagina.

## Codice insieme a noi

### Git

Clona il repo di GitHub (se non lo hai già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Quindi, per raggiungere lo stato attuale dell'app, esegui

```bash
cd mdn-svelte-tutorial/06-stores
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/06-stores
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità di sviluppo.

### REPL

Per scrivere il codice insieme a noi usando REPL, inizia da

<https://svelte.dev/repl/d1fa84a5a4494366b179c87395940039?version=3.23.2>

## Gestire lo stato della nostra app

Abbiamo già visto come i nostri componenti possano comunicare tra loro usando props, data binding bidirezionale ed eventi. In tutti questi casi ci stavamo occupando della comunicazione tra componenti parent e child.

Ma non tutto lo stato applicativo appartiene alla gerarchia dei componenti della tua applicazione. Ad esempio, informazioni sull'utente loggato o se il tema scuro è selezionato o meno.

A volte, lo stato della tua app dovrà essere accessibile da più componenti non gerarchicamente correlati, o da un modulo JavaScript regolare.

Inoltre, quando la tua app diventa complicata e la gerarchia dei componenti diventa complessa, potrebbe diventare troppo difficile per i componenti trasmettere dati tra loro. In tal caso, passare a un data store globale potrebbe essere una buona opzione. Se hai già lavorato con [Redux](https://redux.js.org/) o [Vuex](https://vuex.vuejs.org/), allora sei già familiare con il funzionamento di questo tipo di store. Gli store di Svelte offrono funzionalità simili per la gestione dello stato.

Uno store è un oggetto con un metodo `subscribe()` che permette alle parti interessate di essere notificate ogni volta che il valore dello store cambia e un metodo `set()` opzionale che ti permette di impostare nuovi valori per lo store. Questa API minimale è conosciuta come il [contratto dello store](https://svelte.dev/docs/svelte-components#script-4-prefix-stores-with-$-to-access-their-values-store-contract).

Svelte fornisce funzioni per creare store [readable](https://svelte.dev/docs/svelte-store#readable), [writable](https://svelte.dev/docs/svelte-store#writable) e [derived](https://svelte.dev/docs/svelte-store#derived) nel modulo `svelte/store`.

Svelte fornisce anche un modo molto intuitivo per integrare gli store nel suo sistema reattivo utilizzando la [sintassi reattiva `$store`](https://svelte.dev/docs/svelte-components#script-4-prefix-stores-with-$-to-access-their-values). Se crei i tuoi store rispettando il contratto dello store, ottieni questa sorta di zucchero sintattico per la reattività gratuitamente.

## Creare il componente Alert

Per mostrare come lavorare con gli store, creeremo un componente `Alert`. Questo tipo di widget potrebbe essere anche conosciuto come popup notifications, toast, o notification bubbles.

Il nostro componente `Alert` sarà visualizzato dal componente `App`, ma qualsiasi componente può inviare notifiche ad esso. Ogni volta che arriva una notifica, il componente `Alert` avrà il compito di mostrarla sullo schermo.

### Creare uno store

Iniziamo creando uno store writable. Qualsiasi componente sarà in grado di scrivere su questo store, e il componente `Alert` si sottoscriverà a esso e visualizzerà un messaggio ogni volta che lo store verrà modificato.

1. Crea un nuovo file, `stores.js`, all'interno della tua directory `src`.
2. Dagli il seguente contenuto:

   ```js
   import { writable } from "svelte/store";

   export const alert = writable("Welcome to the to-do list app!");
   ```

> [!NOTE]
> Gli store possono essere definiti e utilizzati al di fuori dei componenti Svelte, quindi puoi organizzarli nel modo che preferisci.

Nel codice sopra importiamo la funzione `writable()` da `svelte/store` e la utilizziamo per creare un nuovo store chiamato `alert` con un valore iniziale di "Benvenuto nell'app della lista delle cose da fare!". Successivamente `export` lo store.

### Creare il componente vero e proprio

Ora creiamo il nostro componente `Alert` e vediamo come possiamo leggere i valori dallo store.

1. Crea un altro nuovo file chiamato `src/components/Alert.svelte`.
2. Dagli il seguente contenuto:

   ```svelte
   <script>
     import { alert } from "../stores.js";
     import { onDestroy } from "svelte";

     let alertContent = "";

     const unsubscribe = alert.subscribe((value) => (alertContent = value));

     onDestroy(unsubscribe);
   </script>

   {#if alertContent}
   <div on:click={() => (alertContent = "")}>
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

Analizziamo in dettaglio questo pezzo di codice.

- All'inizio importiamo lo store `alert`.
- Successivamente importiamo la funzione del ciclo di vita `onDestroy()`, che ci permette di eseguire un callback dopo che il componente è stato rimosso.
- Creiamo poi una variabile locale chiamata `alertContent`. Ricorda che possiamo accedere alle variabili di livello superiore dal markup e, ogni volta che vengono modificate, il DOM verrà aggiornato di conseguenza.
- Quindi chiamiamo il metodo `alert.subscribe()`, passandogli una funzione di callback come parametro. Ogni volta che il valore dello store cambia, la funzione di callback verrà chiamata con il nuovo valore come parametro. Nella funzione di callback assegniamo semplicemente il valore ricevuto alla variabile locale, che attiverà l'aggiornamento del DOM del componente.
- Il metodo `subscribe()` restituisce anche una funzione di pulizia, che si occupa di rilasciare l'iscrizione. Quindi ci iscriviamo quando il componente viene inizializzato e utilizziamo `onDestroy` per disiscriverci quando il componente viene rimosso.
- Infine, utilizziamo la variabile `alertContent` nel nostro markup e, se l'utente clicca sull'alert, lo puliamo.
- Alla fine includiamo alcune linee CSS per stilare il nostro componente `Alert`.

Questa configurazione ci permette di lavorare con gli store in modo reattivo. Quando il valore dello store cambia, il callback viene eseguito. Lì assegniamo un nuovo valore a una variabile locale e, grazie alla reattività di Svelte, tutto il nostro markup e le dipendenze reattive vengono aggiornati di conseguenza.

### Utilizzare il componente

Ora utilizziamo il nostro componente.

1. In `App.svelte` importeremo il componente. Aggiungi la seguente dichiarazione d'importazione sotto quella esistente:

   ```js
   import Alert from "./components/Alert.svelte";
   ```

2. Quindi chiama il componente `Alert` appena sopra la chiamata a `Todos`, in questo modo:

   ```svelte
   <Alert />
   <Todos {todos} />
   ```

3. Carica ora la tua app di test e dovresti vedere il messaggio di `Alert` sullo schermo. Puoi cliccarci sopra per eliminarlo.

   ![Una semplice notifica nell'angolo in alto a destra di un'app che dice benvenuto nell'app della lista delle cose da fare](01-alert-message.png)

## Rendere gli store reattivi con la sintassi `$store` reattiva

Questo funziona, ma dovrai copiare e incollare tutto questo codice ogni volta che vuoi sottoscriverti a uno store:

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

Troppo boilerplate per Svelte! Essendo un compilatore, Svelte ha più risorse per semplificarci la vita. In questo caso, Svelte fornisce la sintassi `$store` reattiva, nota anche come auto-subscription. In termini semplici, basta prefissare lo store con il segno `$` e Svelte genererà il codice per renderlo reattivo automaticamente. Quindi, il nostro blocco di codice precedente può essere sostituito con questo:

```svelte
<script>
  import myStore from "./stores.js";
</script>

{$myStore}
```

E `$myStore` sarà completamente reattivo. Questo si applica anche ai tuoi store personalizzati. Se implementi i metodi `subscribe()` e `set()`, come faremo più avanti, la sintassi `$store` reattiva si applicherà anche ai tuoi store.

1. Applichiamolo al nostro componente `Alert`. Aggiorna le sezioni `<script>` e markup di `Alert.svelte` come segue:

   ```svelte
   <script>
     import { alert } from "../stores.js";
   </script>

   {#if $alert}
   <div on:click={() => $alert = ""}>
     <p>{ $alert }</p>
   </div>
   {/if}
   ```

2. Controlla di nuovo la tua app e vedrai che funziona come prima. È molto meglio!

Dietro le quinte, Svelte ha generato il codice per dichiarare la variabile locale `$alert`, sottoscriversi allo store `alert`, aggiornare `$alert` ogni volta che il contenuto dello store viene modificato e disiscriversi quando il componente viene rimosso. Genererà anche le dichiarazioni `alert.set()` ogni volta che assegniamo un valore a `$alert`.

Il risultato finale di questo trucco è che puoi accedere agli store globali facilmente come utilizzare variabili locali reattive.

Questo è un perfetto esempio di come Svelte mette il compilatore al comando di una migliore ergonomia per gli sviluppatori, non solo risparmiandoci dalla scrittura di codice boilerplate, ma anche generando codice meno soggetto a errori.

## Scrivere nel nostro store

Scrivere nel nostro store è semplicemente una questione di importarlo ed eseguire `$store = 'nuovo valore'`. Usiamolo nel nostro componente `Todos`.

1. Aggiungi la seguente dichiarazione `import` sotto quelle esistenti:

   ```js
   import { alert } from "../stores.js";
   ```

2. Aggiorna la tua funzione `addTodo()` in questo modo:

   ```js
   function addTodo(name) {
     todos = [...todos, { id: newTodoId, name, completed: false }];
     $alert = `Todo '${name}' has been added`;
   }
   ```

3. Aggiorna la funzione `removeTodo()` in questo modo:

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

6. Infine, per ora, aggiorna i blocchi `const checkAllTodos` e `const removeCompletedTodos` come segue:

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

7. Quindi, fondamentalmente, abbiamo importato lo store e lo abbiamo aggiornato su ogni evento, il che provoca la visualizzazione di un nuovo alert ogni volta. Dai un'occhiata di nuovo alla tua app e prova ad aggiungere/eliminare/aggiornare alcuni to-do!

Appena eseguiamo `$alert = …`, Svelte eseguirà `alert.set()`. Il nostro componente `Alert` — come ogni abbonato allo store `alert` — verrà notificato quando riceve un nuovo valore, e grazie alla reattività di Svelte il suo markup verrà aggiornato.

Potremmo fare lo stesso all'interno di qualsiasi componente o file `.js`.

> [!NOTE]
> Al di fuori dei componenti Svelte non puoi usare la sintassi `$store`. Questo perché il compilatore Svelte non toccherà nulla al di fuori dei componenti Svelte. In tal caso dovrai affidarti ai metodi `store.subscribe()` e `store.set()`.

## Migliorare il nostro componente Alert

È un po' fastidioso dover cliccare sull'alert per eliminarlo. Sarebbe meglio se la notifica scomparisse da sola dopo un paio di secondi.

Vediamo come fare. Specificheremo una prop con i millisecondi da attendere prima di cancellare la notifica, e definiremo un timeout per rimuovere l'alert. Ci occuperemo anche di cancellare il timeout quando il componente `Alert` viene rimosso per prevenire perdite di memoria.

1. Aggiorna la sezione `<script>` del tuo componente `Alert.svelte` in questo modo:

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

2. E aggiorna la sezione del markup di `Alert.svelte` in questo modo:

   ```svelte
   {#if visible}
   <div on:click={() => visible = false}>
     <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M12.432 0c1.34 0 2.01.912 2.01 1.957 0 1.305-1.164 2.512-2.679 2.512-1.269 0-2.009-.75-1.974-1.99C9.789 1.436 10.67 0 12.432 0zM8.309 20c-1.058 0-1.833-.652-1.093-3.524l1.214-5.092c.211-.814.246-1.141 0-1.141-.317 0-1.689.562-2.502 1.117l-.528-.88c2.572-2.186 5.531-3.467 6.801-3.467 1.057 0 1.233 1.273.705 3.23l-1.391 5.352c-.246.945-.141 1.271.106 1.271.317 0 1.357-.392 2.379-1.207l.6.814C12.098 19.02 9.365 20 8.309 20z"/></svg>
     <p>{ $alert }</p>
   </div>
   {/if}
   ```

Qui creiamo prima la prop `ms` con un valore predefinito di 3000 (millisecondi). Poi creiamo una funzione `onMessageChange()` che si occuperà di controllare se l'Alert è visibile o meno. Con `$: onMessageChange($alert, ms)` diciamo a Svelte di eseguire questa funzione ogni volta che lo store `$alert` o la prop `ms` cambia.

Ogni volta che lo store `$alert` cambia, ripuliamo eventuali timeout in sospeso. Se `$alert` è vuoto, impostiamo `visible` su `false` e l`'Alert` verrà rimosso dal DOM. Se non è vuoto, impostiamo `visible` su `true` e utilizziamo la funzione `setTimeout()` per cancellare l'alert dopo `ms` millisecondi.

Infine, con la funzione del ciclo di vita `onDestroy()`, ci assicuriamo di chiamare la funzione `clearTimeout()`.

Abbiamo anche aggiunto un'icona SVG sopra il paragrafo dell'alert, per renderlo un po' più gradevole. Provalo di nuovo, e dovresti vedere i cambiamenti.

## Rendere accessibile il nostro componente Alert

Il nostro componente `Alert` funziona bene, ma non è molto amichevole per le tecnologie assistive. Il problema sono gli elementi che vengono aggiunti e rimossi dinamicamente dalla pagina. Sebbene visivamente evidenti per gli utenti che possono vedere la pagina, potrebbero non essere così evidenti per gli utenti di tecnologie assistive, come i lettori di schermo. Per gestire queste situazioni, possiamo sfruttare le [regioni live ARIA](/it/docs/Web/Accessibility/ARIA/Guides/Live_regions), che forniscono un modo per esporre programmaticamente le modifiche dinamiche al contenuto affinché possano essere rilevate e annunciate dalle tecnologie assistive.

Possiamo dichiarare una regione che contiene contenuti dinamici che devono essere annunciati dalle tecnologie assistive con la proprietà `aria-live` seguita dall'impostazione di cortesia, che viene utilizzata per impostare la priorità con cui i lettori di schermo dovrebbero gestire gli aggiornamenti a quella regione. Le impostazioni possibili sono `off`, `polite` o `assertive`.

Per situazioni comuni, hai anche diversi valori di `role` specializzati predefiniti che possono essere utilizzati, come `log`, `status` e `alert`.

Nel nostro caso, basta aggiungere un `role="alert"` al contenitore `<div>` così:

```svelte
<div role="alert" on:click={() => visible = false}>
```

In generale, testare le tue applicazioni usando i lettori di schermo è una buona idea, non solo per scoprire problemi di accessibilità ma anche per abituarsi a come le persone non vedenti utilizzano il Web. Hai diverse opzioni, come [NVDA](https://www.nvaccess.org/) per Windows, [ChromeVox](https://support.google.com/chromebook/answer/7031755) per Chrome, [Orca](https://wiki.gnome.org/Projects/Orca) su Linux, e [VoiceOver](https://www.apple.com/accessibility/features/?vision) su macOS e iOS, tra le altre opzioni.

Per saperne di più su come rilevare e risolvere problemi di accessibilità, consulta il nostro modulo di [Accessibilità](/it/docs/Learn_web_development/Core/Accessibility).

## Usare il contratto dello store per salvare i nostri to-do

La nostra piccola app ci consente di gestire facilmente i nostri to-do, ma è piuttosto inutile se ogni volta otteniamo la stessa lista di to-do predefiniti quando la ricarichiamo. Per renderla veramente utile, dobbiamo trovare un modo per salvare i nostri to-do.

Prima abbiamo bisogno di un modo per consentire al nostro componente `Todos` di restituire i to-do aggiornati al suo genitore. Potremmo emettere un evento aggiornato con l'elenco dei to-do, ma è più semplice vincolare la variabile `todos`. Apriamo `App.svelte` e proviamolo.

1. Prima, aggiungi la seguente stringa sotto il tuo array `todos`:

   ```js
   $: console.log("todos", todos);
   ```

2. Successivamente, aggiorna la chiamata del tuo componente `Todos` come segue:

   ```svelte
   <Todos bind:todos />
   ```

   > **Nota:** `<Todos bind:todos />` è solo un' abbreviazione per `<Todos bind:todos={todos} />`.

3. Torna alla tua app, prova ad aggiungere alcuni to-do, quindi vai alla web console degli strumenti per sviluppatori. Vedrai che ogni modifica che facciamo ai nostri to-do viene riflessa nell'array `todos` definito in `App.svelte` grazie alla direttiva `bind`.

Ora dobbiamo trovare un modo per salvare questi to-do. Potremmo implementare del codice nel nostro componente `App.svelte` per leggere e salvare i nostri to-do nella [memoria web](/it/docs/Web/API/Web_Storage_API) o in un servizio web.
Ma non sarebbe meglio se potessimo sviluppare uno store generico che ci permetta di salvare il suo contenuto? Questo ci permetterebbe di usarlo proprio come qualsiasi altro store, e di astrarre il meccanismo di persistenza. Potremmo creare uno store che sincronizza il suo contenuto con la memoria web e, successivamente, svilupparne un altro che si sincronizza con un servizio web. Passare da uno all'altro sarebbe banale e non dovremmo toccare affatto `App.svelte`.

### Salvare i nostri to-do

Quindi iniziamo usando uno store writable regolare per salvare i nostri to-do.

1. Apriamo il file `stores.js` e aggiungiamo il seguente store sotto quello esistente:

   ```js
   export const todos = writable([]);
   ```

2. È stato facile. Ora dobbiamo importare lo store e usarlo in `App.svelte`. Ricorda solo che per accedere ai to-do ora dobbiamo utilizzare la sintassi `$todos` reattivo `$store`.

   Aggiorna il tuo file `App.svelte` in questo modo:

   ```svelte
   <script>
     import Todos from "./components/Todos.svelte";
     import Alert from "./components/Alert.svelte";

     import { todos } from "./stores.js";

     $todos = [
       { id: 1, name: "Create a Svelte starter app", completed: true },
       { id: 2, name: "Create your first component", completed: true },
       { id: 3, name: "Complete the rest of the tutorial", completed: false },
     ];
   </script>

   <Alert />
   <Todos bind:todos={$todos} />
   ```

3. Provalo; tutto dovrebbe funzionare. Successivamente vedremo come definire i nostri store personalizzati.

### Come implementare un contratto di store: La teoria

Puoi creare i tuoi store senza fare affidamento su `svelte/store` implementando il contratto di store. Le sue funzionalità devono funzionare in questo modo:

1. Uno store deve contenere un metodo `subscribe()`, che deve accettare come argomento una funzione di sottoscrizione. Tutte le funzioni di sottoscrizione attive di uno store devono essere chiamate ogni volta che il valore dello store cambia.
2. Il metodo `subscribe()` deve restituire una funzione `unsubscribe()`, che quando chiamata deve interrompere la sua sottoscrizione.
3. Uno store può facoltativamente contenere un metodo `set()`, che deve accettare come argomento un nuovo valore per lo store, e che chiama sincronicamente tutte le funzioni di sottoscrizione attive dello store. Uno store con un metodo `set()` è chiamato uno store writable.

Per prima cosa, aggiungiamo le seguenti dichiarazioni `console.log()` al nostro componente `App.svelte` per vedere lo store `todos` e il suo contenuto in azione. Aggiungi queste righe sotto l'array `todos`:

```js
console.log("todos store - todos:", todos);
console.log("todos store content - $todos:", $todos);
```

Quando esegui l'app ora, vedrai qualcosa di simile nella tua console web:

![console web che mostra le funzioni e i contenuti dello store todos](02-svelte-store-in-action.png)

Come puoi vedere, il nostro store è semplicemente un oggetto contenente i metodi `subscribe()`, `set()`, e `update()`, e `$todos` è il nostro array di to-do.

Solo per riferimento, ecco uno store di base funzionante implementato da zero:

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

Qui dichiariamo `subs`, che è un array di subscriber. Nel metodo `subscribe()` aggiungiamo l'handler all'array `subs` e ritorniamo una funzione che, quando eseguita, rimuoverà l'handler dall'array.

Quando chiamiamo `set()`, aggiorniamo il valore dello store e chiamiamo ciascun handler, passando il nuovo valore come parametro.

Di solito non implementi store da zero; useresti invece lo store writable per creare [store personalizzati](https://learn.svelte.dev/tutorial/custom-stores) con logica specifica per il dominio. Nell'esempio seguente creiamo uno store di contatori, che ci permetterà solo di aggiungere uno al contatore o di reimpostarne il valore:

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

Se la nostra app lista delle cose da fare diventa troppo complessa, potremmo lasciare che il nostro store to-do gestisca ogni modifica di stato. Potremmo spostare tutti i metodi che modificano l'array `todo` (come `addTodo()`, `removeTodo()`, ecc.) dal nostro componente `Todos` allo store. Se hai un luogo centrale dove tutte le modifiche di stato vengono applicate, i componenti potrebbero semplicemente chiamare quei metodi per modificare lo stato dell'app e visualizzare reattivamente le informazioni esposte dallo store. Avere un luogo unico per gestire le modifiche di stato rende più facile ragionare sul flusso dello stato e individuare i problemi.

Svelte non ti obbliga ad organizzare la gestione del tuo stato in un modo specifico; ti fornisce solo gli strumenti per scegliere come gestirlo.

### Implementare il nostro store di to-do personalizzato

La nostra app lista delle cose da fare non è particolarmente complessa, quindi non sposteremo tutti i nostri metodi di modifica in un luogo centrale. Li lasceremo semplicemente come sono, e ci concentreremo invece sulla persistenza dei nostri to-do.

> [!NOTE]
> Se stai seguendo questa guida lavorando da Svelte REPL, non sarai in grado di completare questo passaggio. Per motivi di sicurezza, Svelte REPL funziona in un ambiente sandbox che non ti consentirà di accedere alla memoria web, e otterrai un errore del tipo "The operation is insecure". Per seguire questa sezione, dovrai clonare il repo e andare nella cartella `mdn-svelte-tutorial/06-stores`, oppure puoi scaricare direttamente il contenuto della cartella con `npx degit opensas/mdn-svelte-tutorial/06-stores`.

Per implementare uno store personalizzato che salva il suo contenuto nella memoria web, avremo bisogno di uno store writable che faccia quanto segue:

- Legge inizialmente il valore dalla memoria web e, se non è presente, lo inizializza con un valore predefinito
- Ogni volta che il valore viene modificato, aggiorna lo store stesso e anche i dati nella memoria locale

Inoltre, poiché la memoria web supporta solo il salvataggio di valori stringa, dovremo convertire da oggetto a stringa durante il salvataggio, e viceversa quando carichiamo il valore dalla memoria locale.

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

   - Il nostro `localStore` sarà una funzione che quando eseguita inizialmente legge il suo contenuto dalla memoria web e restituisce un oggetto con tre metodi: `subscribe()`, `set()`, ed `update()`.
   - Quando creiamo un nuovo `localStore`, dovremo specificare la chiave della memoria web e un valore iniziale. Successivamente controlliamo se il valore esiste nella memoria web e, in caso contrario, lo creiamo.
   - Utilizziamo i metodi [`localStorage.getItem(key)`](/it/docs/Web/API/Storage/getItem) e [`localStorage.setItem(key, value)`](/it/docs/Web/API/Storage/setItem) per leggere e scrivere informazioni nella memoria web, e le funzioni helper [`toString()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Object/toString) e `toObj()` (che utilizza [`JSON.parse()`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)) per convertire i valori.
   - Successivamente, convertiamo il contenuto in stringa ricevuto dalla memoria web in un oggetto e lo salviamo nel nostro store.
   - Infine, ogni volta che aggiorniamo i contenuti dello store, aggiorniamo anche la memoria web, con il valore convertito in stringa.

   Notare che abbiamo dovuto ridefinire solo il metodo `set()`, aggiungendo l'operazione per salvare il valore nella memoria web. Il resto del codice riguarda principalmente inizializzazioni e conversioni.

3. Ora useremo il nostro local store da `stores.js` per creare il nostro store to-do persistito localmente.

   Aggiorna `stores.js` come segue:

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

   Utilizzando `localStore('mdn-svelte-todo', initialTodos)`, stiamo configurando lo store per salvare i dati nella memoria web sotto la chiave `mdn-svelte-todo`. Impostiamo anche un paio di to-do come valori iniziali.

4. Ora eliminiamo i to-do predefiniti in `App.svelte`. Aggiorna i contenuti come segue. Stiamo fondamentalmente cancellando l'array `$todos` e le dichiarazioni `console.log()`:

   ```svelte
   <script>
     import Todos from "./components/Todos.svelte";
     import Alert from "./components/Alert.svelte";

     import { todos } from "./stores.js";
   </script>

   <Alert />
   <Todos bind:todos={$todos} />
   ```

   > [!NOTE]
   > Questo è l'unico cambiamento che dobbiamo fare per utilizzare il nostro store personalizzato. `App.svelte` è completamente trasparente in termini di quale tipo di store stiamo usando.

5. Vai avanti e prova la tua app di nuovo. Crea alcuni to-do e poi chiudi il browser. Puoi anche fermare il server Svelte e riavviarlo. Ritornando alla URL, i tuoi to-do saranno ancora lì.
6. Puoi anche controllarlo nei DevTools. Nella console web, immetti il comando `localStorage.getItem('mdn-svelte-todo')`. Apporta alcune modifiche alla tua app, come premere il pulsante _Deseleziona tutto_, e controlla di nuovo il contenuto della memoria web. Otterrai qualcosa come questo:

   ![app to-do con vista console web accanto, che mostra che quando un to-do viene cambiato nell'app, la voce corrispondente viene cambiata nella memoria web](03-persisting-todos-to-local-storage.png)

Gli store di Svelte offrono un modo molto semplice e leggero, ma estremamente potente, per gestire lo stato complesso delle app da un data store globale in modo reattivo. E poiché Svelte compila il nostro codice, può fornire la [sintassi di auto-sottoscrizione `$store`](https://svelte.dev/docs/svelte-components#script-4-prefix-stores-with-$-to-access-their-values) che ci consente di lavorare con gli store nello stesso modo in cui lavoriamo con le variabili locali. Poiché gli store hanno un'API minima, è molto semplice creare i nostri store personalizzati per astrarre il funzionamento interno dello store stesso.

## Traccia bonus: Transizioni

Cambiamo ora argomento e facciamo qualcosa di divertente e diverso: aggiungiamo un'animazione ai nostri alert. Svelte fornisce un intero modulo per definire [transizioni](https://learn.svelte.dev/tutorial/transition) e [animazioni](https://learn.svelte.dev/tutorial/animate) in modo da rendere le nostre interfacce utente più accattivanti.

Una transizione viene applicata con la direttiva [transition:fn](https://svelte.dev/docs/element-directives#transition-fn) e viene attivata da un elemento che entra o esce dal DOM a seguito di un cambio di stato.

Diamo al nostro componente `Alert` una transizione `fly`. Apriremo il file `Alert.svelte` e importeremo la funzione `fly` dal modulo `svelte/transition`.

1. Metti la seguente dichiarazione `import` sotto quelle esistenti:

   ```js
   import { fly } from "svelte/transition";
   ```

2. Per usarla, aggiorna il tuo tag `<div>` di apertura in questo modo:

   ```svelte
   <div role="alert" on:click={() => visible = false}
     transition:fly
   >
   ```

   Le transizioni possono anche ricevere parametri, in questo modo:

   ```svelte
   <div role="alert" on:click={() => visible = false}
     transition:fly="\{{delay: 250, duration: 300, x: 0, y: -100, opacity: 0.5}}"
   >
   ```

   > [!NOTE]
   > Le doppie parentesi graffe non sono sintassi speciale di Svelte. È solo un oggetto JavaScript letterale che viene passato come parametro alla transizione fly.

3. Prova di nuovo la tua app e vedrai che le notifiche ora sono un po' più accattivanti.

> [!NOTE]
> Essere un compilatore consente a Svelte di ottimizzare la dimensione del nostro bundle escludendo le funzionalità non utilizzate. In questo caso, se compiliamo la nostra app per la produzione con `npm run build`, il nostro file `public/build/bundle.js` peserà poco meno di 22 KB. Se rimuoviamo la direttiva `transitions:fly` Svelte è abbastanza intelligente da capire che la funzione fly non viene utilizzata e la dimensione del file `bundle.js` scenderà a soli 18 KB.

Questo è solo un assaggio. Svelte ha molte opzioni per gestire animazioni e transizioni. Svelte supporta anche la specificazione di transizioni diverse da applicare quando l'elemento viene aggiunto o rimosso dal DOM con le direttive `in:fn`/`out:fn`, e ti consente anche di definire le tue [transizioni CSS personalizzate](https://learn.svelte.dev/tutorial/custom-css-transitions) e le [transizioni JavaScript personalizzate](https://learn.svelte.dev/tutorial/custom-js-transitions). Ha anche diverse funzioni di easing per specificare il ritmo del cambiamento nel tempo. Dai un'occhiata al [visualizzatore di easing](https://svelte.dev/examples/easing) per esplorare le varie funzioni di easing disponibili.

## Il codice finora

### Git

Per vedere lo stato del codice come dovrebbe essere alla fine di questo articolo, accedi alla tua copia del nostro repo in questo modo:

```bash
cd mdn-svelte-tutorial/07-next-steps
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/07-next-steps
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità di sviluppo.

### REPL

Per vedere lo stato attuale del codice in un REPL, visita:

<https://svelte.dev/repl/378dd79e0dfe4486a8f10823f3813190?version=3.23.2>

## Sommario

In questo articolo abbiamo aggiunto due nuove funzionalità: un componente `Alert` e la persistenza dei `todos` nella memoria web.

- Questo ci ha permesso di mostrare alcune tecniche avanzate di Svelte. Abbiamo sviluppato il componente `Alert` per mostrare come implementare la gestione dello stato inter-componente usando gli store. Abbiamo anche visto come sottoscriverci automaticamente agli store per integrarli senza problemi con il sistema di reattività di Svelte.
- Poi abbiamo visto come implementare il nostro store da zero, e anche come estendere lo store writable di Svelte per salvare i dati nella memoria web.
- Alla fine abbiamo dato un'occhiata all'utilizzo della direttiva di `transition` di Svelte per implementare animazioni sugli elementi DOM.

Nel prossimo articolo impareremo come aggiungere il supporto TypeScript alla nostra applicazione Svelte. Per sfruttare tutte le sue funzionalità, porteremo anche la nostra intera applicazione su TypeScript.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility","Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript", "Learn_web_development/Core/Frameworks_libraries")}}
