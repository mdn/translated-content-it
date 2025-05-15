---
title: Lavorare con i negozi Svelte
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_stores
l10n:
  sourceCommit: eaec5c4226ac64696a95314a7bce995165a4d124
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility","Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript", "Learn_web_development/Core/Frameworks_libraries")}}

Nell'ultimo articolo abbiamo completato lo sviluppo della nostra app, finito di organizzarla in componenti e discusso alcune tecniche avanzate per trattare la reattività, lavorare con i nodi DOM e esporre la funzionalità dei componenti. In questo articolo mostreremo un altro modo per gestire la gestione dello stato in Svelte: i [negozi](https://learn.svelte.dev/tutorial/writable-stores). I negozi sono repository di dati globali che contengono valori. I componenti possono iscriversi ai negozi e ricevere notifiche quando i loro valori cambiano.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È consigliato, come minimo, che lei sia familiare con i linguaggi di base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e che abbia conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/linea di comando</a
          >.
        </p>
        <p>
          Lei avrà bisogno di un terminale con node e npm installati per compilare e costruire la sua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare come usare i negozi Svelte</td>
    </tr>
  </tbody>
</table>

Utilizzando i negozi, creeremo un componente `Alert` che mostra notifiche sullo schermo, che può ricevere messaggi da qualsiasi componente. In questo caso, il componente `Alert` è indipendente dal resto — non è un genitore o figlio di nessun altro — quindi i messaggi non si adattano alla gerarchia dei componenti.

Vedremo anche come sviluppare il nostro negozio personalizzato per mantenere le informazioni sul todo nella [memorizzazione web](/it/docs/Web/API/Web_Storage_API), permettendo ai nostri todo di persistere oltre i ricaricamenti della pagina.

## Seguite insieme a noi con il codice

### Git

Clonare il repo GitHub (se non lo ha già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Quindi, per arrivare allo stato attuale dell'app, eseguire

```bash
cd mdn-svelte-tutorial/06-stores
```

Oppure scaricare direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/06-stores
```

Ricordi di eseguire `npm install && npm run dev` per avviare la sua app in modalità di sviluppo.

### REPL

Per seguire insieme a noi utilizzando il REPL, inizi a

<https://svelte.dev/repl/d1fa84a5a4494366b179c87395940039?version=3.23.2>

## Gestire lo stato della nostra app

Abbiamo già visto come i nostri componenti possono comunicare tra loro usando props, binding di dati bidirezionale ed eventi. In tutti questi casi stavamo trattando la comunicazione tra componenti genitori e figli.

Ma non tutto lo stato dell'applicazione appartiene alla gerarchia dei componenti della sua applicazione. Ad esempio, informazioni sull'utente connesso, o se è selezionato il tema scuro o no.

A volte, lo stato della sua app dovrà essere accessibile da più componenti che non sono gerarchicamente correlati, o da un modulo JavaScript regolare.

Inoltre, quando la sua app diventa complicata e la gerarchia dei componenti diventa complessa, potrebbe diventare troppo difficile per i componenti relazionarsi tra loro. In tal caso, passare a un negozio di dati globali potrebbe essere una buona opzione. Se ha già lavorato con [Redux](https://redux.js.org/) o [Vuex](https://vuex.vuejs.org/), allora lei conoscerà come funziona questo tipo di negozio. I negozi Svelte offrono funzionalità simili per la gestione dello stato.

Un negozio è un oggetto con un metodo `subscribe()` che permette alle parti interessate di essere notificate ogni volta che il valore del negozio cambia, e un metodo `set()` opzionale che permette di impostare nuovi valori per il negozio. Questa API minima è conosciuta come [store contract](https://svelte.dev/docs/svelte-components#script-4-prefix-stores-with-$-to-access-their-values-store-contract).

Svelte fornisce funzioni per creare negozi [readable](https://svelte.dev/docs/svelte-store#readable), [writable](https://svelte.dev/docs/svelte-store#writable), e [derived](https://svelte.dev/docs/svelte-store#derived) nel modulo `svelte/store`.

Svelte fornisce anche un modo molto intuitivo per integrare i negozi nel suo sistema di reattività utilizzando la [sintassi reattiva `$store`](https://svelte.dev/docs/svelte-components#script-4-prefix-stores-with-$-to-access-their-values). Se crea i suoi negozi onorando il contratto del negozio, ottiene questo zucchero sintattico reattivo gratuitamente.

## Creare il componente Alert

Per mostrare come lavorare con i negozi, creeremo un componente `Alert`. Questo tipo di widget potrebbe anche essere conosciuto come notifiche popup, toast o bolle di notifica.

Il nostro componente `Alert` sarà visualizzato dal componente `App`, ma qualsiasi componente può inviare notifiche ad esso. Ogni volta che arriva una notifica, il componente `Alert` sarà incaricato di visualizzarla sullo schermo.

### Creare un negozio

Iniziamo creando un negozio scrivibile. Qualsiasi componente sarà in grado di scrivere in questo negozio, e il componente `Alert` si iscriverà a esso e visualizzerà un messaggio ogni volta che il negozio viene modificato.

1. Creare un nuovo file, `stores.js`, all'interno della sua directory `src`.
2. Dargli il seguente contenuto:

   ```js
   import { writable } from "svelte/store";

   export const alert = writable("Welcome to the to-do list app!");
   ```

> [!NOTE]
> I negozi possono essere definiti e utilizzati al di fuori dei componenti Svelte, quindi lei può organizzarli nel modo che desidera.

Nel codice sopra importiamo la funzione `writable()` da `svelte/store` e la usiamo per creare un nuovo negozio chiamato `alert` con un valore iniziale di "Benvenuto nell'app lista di cose da fare!". Poi `esportiamo` il negozio.

### Creare il componente effettivo

Creiamo ora il nostro componente `Alert` e vediamo come possiamo leggere i valori dal negozio.

1. Creare un altro nuovo file chiamato `src/components/Alert.svelte`.
2. Dargli il seguente contenuto:

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

Passiamo in dettaglio questo pezzo di codice.

- All'inizio importiamo il negozio `alert`.
- Successivamente importiamo la funzione del ciclo di vita `onDestroy()`, che ci permette di eseguire un callback dopo che il componente è stato rimosso.
- Poi creiamo una variabile locale chiamata `alertContent`. Ricordate che possiamo accedere alle variabili di livello superiore dal markup, e ogni volta che vengono modificate, il DOM si aggiornerà di conseguenza.
- Poi chiamiamo il metodo `alert.subscribe()`, passandogli una funzione di callback come parametro. Ogni volta che il valore del negozio cambia, la funzione di callback verrà chiamata con il nuovo valore come suo parametro. Nella funzione di callback assegniamo semplicemente il valore che riceviamo alla variabile locale, il quale attiverà l'aggiornamento del DOM del componente.
- Il metodo `subscribe()` restituisce anche una funzione di pulizia, che si occupa di liberare l'iscrizione. Quindi ci iscriviamo quando il componente viene inizializzato, e usiamo `onDestroy` per disiscriverci quando il componente viene rimosso.
- Infine usiamo la variabile `alertContent` nel nostro markup, e se l'utente clicca sull'alert lo puliamo.
- Alla fine includiamo alcune righe di CSS per stilizzare il nostro componente `Alert`.

Questa configurazione ci permette di lavorare con i negozi in modo reattivo. Quando il valore del negozio cambia, viene eseguito il callback. Lì assegniamo un nuovo valore a una variabile locale, e grazie alla reattività di Svelte tutto il nostro markup e le dipendenze reattive vengono aggiornate di conseguenza.

### Utilizzare il componente

Utilizziamo ora il nostro componente.

1. In `App.svelte` importeremo il componente. Aggiunga la seguente istruzione di importazione sotto quella esistente:

   ```js
   import Alert from "./components/Alert.svelte";
   ```

2. Poi richiami il componente `Alert` appena sopra la chiamata `Todos`, in questo modo:

   ```svelte
   <Alert />
   <Todos {todos} />
   ```

3. Carichi ora la sua app di test, e dovrebbe vedere il messaggio `Alert` sullo schermo. Può cliccarci sopra per chiuderlo.

   ![Una semplice notifica nell'angolo in alto a destra di un'app che dice benvenuto all'app lista di cose da fare](01-alert-message.png)

## Rendere i negozi reattivi con la sintassi reattiva `$store`

Funziona, ma lei dovrà copiare e incollare tutto questo codice ogni volta che vuole iscriversi a un negozio:

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

È troppo boilerplate per Svelte! Essendo un compilatore, Svelte ha più risorse per rendere la nostra vita più facile. In questo caso, Svelte fornisce la sintassi reattiva `$store`, anche conosciuta come auto-sottoscrizione. In termini semplici, basta anteporre al negozio il segno `$` e Svelte genererà il codice per renderlo reattivo automaticamente. Quindi il nostro blocco di codice precedente può essere sostituito con questo:

```svelte
<script>
  import myStore from "./stores.js";
</script>

{$myStore}
```

E `$myStore` sarà completamente reattivo. Questo vale anche per i suoi negozi personalizzati. Se implementa i metodi `subscribe()` e `set()`, come faremo in seguito, la sintassi reattiva `$store` si applicherà anche ai suoi negozi.

1. Applichiamolo al nostro componente `Alert`. Aggiorni sezioni `<script>` e markup di `Alert.svelte` come segue:

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

2. Verifichi nuovamente la sua app e vedrà che funziona proprio come prima. Molto meglio!

Dietro le quinte, Svelte ha generato il codice per dichiarare la variabile locale `$alert`, iscriversi al negozio `alert`, aggiornare `$alert` ogni volta che il contenuto del negozio viene modificato, e disiscriversi quando il componente viene smontato. Genererà anche le istruzioni `alert.set()` ogni volta che assegniamo un valore a `$alert`.

Il risultato finale di questo astuto trucchetto è che lei può accedere ai negozi globali con la stessa facilità con cui lavora con le variabili locali reattive.

Questo è un perfetto esempio di come Svelte mette il compilatore a carico di migliori ergonomia per lo sviluppatore, non solo risparmiandoci dal digitare boilerplate, ma anche generando codice meno propenso agli errori.

## Scrivere nel nostro negozio

Scrivere nel nostro negozio è solo una questione di importare il negozio ed eseguire `$store = 'nuovo valore'`. Usiamolo nel nostro componente `Todos`.

1. Aggiungere la seguente istruzione `import` sotto quelle esistenti:

   ```js
   import { alert } from "../stores.js";
   ```

2. Aggiornare la sua funzione `addTodo()` in questo modo:

   ```js
   function addTodo(name) {
     todos = [...todos, { id: newTodoId, name, completed: false }];
     $alert = `Todo '${name}' has been added`;
   }
   ```

3. Aggiornare `removeTodo()` in questo modo:

   ```js
   function removeTodo(todo) {
     todos = todos.filter((t) => t.id !== todo.id);
     todosStatus.focus(); // give focus to status heading
     $alert = `Todo '${todo.name}' has been deleted`;
   }
   ```

4. Aggiornare la funzione `updateTodo()` in questo modo:

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

5. Aggiungere il seguente blocco reattivo sotto il blocco che inizia con `let filter = 'all'`:

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

6. E infine per ora, aggiornare i blocchi `const checkAllTodos` e `const removeCompletedTodos` come segue:

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

7. Abbiamo sostanzialmente importato il negozio e lo abbiamo aggiornato a ogni evento, il che causa una nuova notifica da mostrare ogni volta. Guardi di nuovo la sua app e provi ad aggiungere/eliminare/aggiornare alcuni todo!

Appena eseguiamo `$alert = …`, Svelte eseguirà `alert.set()`. Il nostro componente `Alert` — come ogni sottoscrittore al negozio di allerta — verrà notificato quando riceve un nuovo valore, e grazie alla reattività di Svelte il suo markup verrà aggiornato.

Potremmo fare lo stesso in qualsiasi componente o file `.js`.

> [!NOTE]
> Al di fuori dei componenti Svelte non può usare la sintassi `$store`. Questo perché il compilatore Svelte non toccherà nulla al di fuori dei componenti Svelte. In tal caso dovrà fare affidamento sui metodi `store.subscribe()` e `store.set()`.

## Migliorare il nostro componente Alert

È un po' fastidioso dover cliccare sull'allerta per eliminarla. Sarebbe meglio se la notifica sparisse da sola dopo un paio di secondi.

Vediamo come fare. Specificheremo un prop con i millisecondi da aspettare prima di cancellare la notifica, e definiremo un timeout per rimuovere l'alert. Prenderemo anche cura di pulire il timeout quando il componente `Alert` viene smontato per prevenire perdite di memoria.

1. Aggiornare la sezione `<script>` del suo componente `Alert.svelte` come segue:

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

2. E aggiornare la sezione di markup di `Alert.svelte` come segue:

   ```svelte
   {#if visible}
   <div on:click={() => visible = false}>
     <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M12.432 0c1.34 0 2.01.912 2.01 1.957 0 1.305-1.164 2.512-2.679 2.512-1.269 0-2.009-.75-1.974-1.99C9.789 1.436 10.67 0 12.432 0zM8.309 20c-1.058 0-1.833-.652-1.093-3.524l1.214-5.092c.211-.814.246-1.141 0-1.141-.317 0-1.689.562-2.502 1.117l-.528-.88c2.572-2.186 5.531-3.467 6.801-3.467 1.057 0 1.233 1.273.705 3.23l-1.391 5.352c-.246.945-.141 1.271.106 1.271.317 0 1.357-.392 2.379-1.207l.6.814C12.098 19.02 9.365 20 8.309 20z"/></svg>
     <p>{ $alert }</p>
   </div>
   {/if}
   ```

Qui creiamo innanzitutto la prop `ms` con un valore predefinito di 3000 (millisecondi). Poi creiamo una funzione `onMessageChange()` che si occuperà di controllare se l'Alert è visibile o meno. Con `$: onMessageChange($alert, ms)` diciamo a Svelte di eseguire questa funzione ogni volta che il negozio `$alert` o la prop `ms` cambia.

Ogni volta che il negozio `$alert` cambia, puliremo qualsiasi timeout in sospeso. Se `$alert` è vuoto, impostiamo `visible` su `false` e l'Alert sarà rimosso dal DOM. Se non è vuoto, impostiamo `visible` su `true` e usiamo la funzione `setTimeout()` per cancellare l'alert dopo `ms` millisecondi.

Infine, con la funzione del ciclo di vita `onDestroy()`, ci assicuriamo di chiamare la funzione `clearTimeout()`.

Abbiamo anche aggiunto un'icona SVG sopra il paragrafo dell'alert, per farlo sembrare un po' più bello. Provi nuovamente, e dovrebbe notare i cambiamenti.

## Rendere il nostro componente Alert accessibile

Il nostro componente `Alert` funziona bene, ma non è molto amichevole per le tecnologie assistive. Il problema sono gli elementi che vengono aggiunti e rimossi dinamicamente dalla pagina. Per quanto visivamente evidenti per gli utenti che possono vedere la pagina, potrebbero non essere così ovvi per gli utenti di tecnologie assistive, come i lettori di schermo. Per gestire queste situazioni, possiamo sfruttare le [regioni live ARIA](/it/docs/Web/Accessibility/ARIA/Guides/Live_regions), che forniscono un modo per esporre programmaticamente i cambiamenti di contenuto dinamico in modo che possano essere rilevati e annunciati dalle tecnologie assistive.

Possiamo dichiarare una regione che contiene contenuti dinamici che dovrebbero essere annunciati dalle tecnologie assistive con la proprietà `aria-live` seguita dall'impostazione della cortesia, che viene utilizzata per impostare la priorità con cui i lettori di schermo dovrebbero gestire gli aggiornamenti a quelle regioni. Le impostazioni possibili sono `off`, `polite`, o `assertive`.

Per situazioni comuni, ci sono anche diversi valori di ruolo specializzati predefiniti che possono essere utilizzati, come `log`, `status` e `alert`.

Nel nostro caso, basta aggiungere un `role="alert"` al contenitore `<div>` e fare il trucco, in questo modo:

```svelte
<div role="alert" on:click={() => visible = false}>
```

In generale, testare le sue applicazioni utilizzando i lettori di schermo è una buona idea, non solo per scoprire problemi di accessibilità, ma anche per abituarsi a come le persone con disabilità visive usano il Web. Ha diverse opzioni, come [NVDA](https://www.nvaccess.org/) per Windows, [ChromeVox](https://support.google.com/chromebook/answer/7031755) per Chrome, [Orca](https://wiki.gnome.org/Projects/Orca) su Linux, e [VoiceOver](https://www.apple.com/accessibility/features/?vision) per macOS e iOS, tra altre opzioni.

Per saperne di più su come rilevare e risolvere i problemi di accessibilità, consulti il nostro modulo [Accessibilità](/it/docs/Learn_web_development/Core/Accessibility).

## Usare il contratto dello store per mantenere i nostri to-dos

La nostra piccola app ci permette di gestire i nostri to-dos abbastanza facilmente, ma è piuttosto inutile se riceviamo sempre la stessa lista di to-dos predefiniti quando la ricarichiamo. Per renderla veramente utile, dobbiamo trovare un modo per mantenere i nostri to-dos.

Prima abbiamo bisogno di un modo per il nostro componente `Todos` di restituire i to-dos aggiornati al suo genitore. Potremmo emettere un evento aggiornato con l'elenco dei to-dos, ma è più facile legare la variabile `todos`. Apriamo `App.svelte` e proviamolo.

1. Prima, aggiungere la seguente riga sotto il suo array `todos`:

   ```js
   $: console.log("todos", todos);
   ```

2. Successivamente, aggiornare la chiamata del suo componente `Todos` come segue:

   ```svelte
   <Todos bind:todos />
   ```

   > **Nota:** `<Todos bind:todos />` è solo una scorciatoia per `<Todos bind:todos={todos} />`.

3. Torni alla sua app, provi ad aggiungere alcuni to-dos, poi vada alla console web degli strumenti di sviluppo. Vedrà che ogni modifica che facciamo ai nostri to-dos si riflette nell'array `todos` definito in `App.svelte` grazie alla direttiva `bind`.

Ora dobbiamo trovare un modo per mantenere questi to-dos. Potremmo implementare del codice nel nostro componente `App.svelte` per leggere e salvare i nostri to-dos nella [memorizzazione web](/it/docs/Web/API/Web_Storage_API) o in un servizio web.
Ma non sarebbe meglio se potessimo sviluppare qualche negozio generico che ci permetta di mantenere il suo contenuto? Questo ci permetterebbe di usarlo proprio come qualsiasi altro negozio, e di astrarre via il meccanismo di mantenimento. Potremmo creare un negozio che sincronizza il suo contenuto con la memorizzazione web, e poi svilupparne un altro che sincronizza contro un servizio web. Passare tra loro sarebbe banale e non dovremmo toccare `App.svelte` per niente.

### Salvare i nostri to-dos

Iniziamo quindi usando un negozio scrivibile regolare per salvare i nostri to-dos.

1. Aprire il file `stores.js` e aggiungere il seguente negozio sotto quello esistente:

   ```js
   export const todos = writable([]);
   ```

2. È stato facile. Ora dobbiamo importare il negozio e usarlo in `App.svelte`. Ricordi solo che per accedere ai to-dos ora dobbiamo usare la sintassi reattiva `$todos` del `$store`.

   Aggiornare il file `App.svelte` in questo modo:

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

3. Provi; tutto dovrebbe funzionare. Poi vedremo come definire i nostri negozi personalizzati.

### Come implementare un contratto di negozio: La teoria

Può creare i suoi negozi senza fare affidamento su `svelte/store` implementando il contratto del negozio. Le sue caratteristiche devono funzionare in questo modo:

1. Un negozio deve contenere un `metodo subscribe()`, che deve accettare come suo argomento una funzione di sottoscrizione. Tutte le funzioni di sottoscrizione attive di un negozio devono essere chiamate ogni volta che il valore del negozio cambia.
2. Il `metodo subscribe()` deve restituire una funzione `unsubscribe()`, che quando chiamata deve interrompere la sua sottoscrizione.
3. Un negozio può contenere opzionalmente un `metodo set()`, che deve accettare come suo argomento un nuovo valore per il negozio, e che sincronamente chiama tutte le funzioni di sottoscrizione attive del negozio. Un negozio con un `metodo set()` è chiamato negozio scrivibile.

Innanzitutto, aggiungere le seguenti istruzioni `console.log()` al suo componente `App.svelte` per vedere il negozio `todos` e il suo contenuto in azione. Aggiungere queste righe sotto l'array `todos`:

```js
console.log("todos store - todos:", todos);
console.log("todos store content - $todos:", $todos);
```

Ora, quando esegue l'app, vedrà qualcosa del genere nella sua console web:

![console web che mostra le funzioni e i contenuti del negozio todos](02-svelte-store-in-action.png)

Come può vedere, il nostro negozio è solo un oggetto che contiene i metodi `subscribe()`, `set()`, e `update()`, e `$todos` è il nostro array di to-dos.

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

Qui dichiariamo `subs`, che è un array di abbonati. Nel metodo `subscribe()` aggiungiamo il gestore all'array `subs` e restituiamo una funzione che, quando eseguita, rimuoverà il gestore dall'array.

Quando chiamiamo `set()`, aggiorniamo il valore del negozio e chiamiamo ogni gestore, passando il nuovo valore come parametro.

Di solito non si implementano negozi da zero; invece si usa il negozio scrivibile per creare [negozi personalizzati](https://learn.svelte.dev/tutorial/custom-stores) con logica specifica del dominio. Nell'esempio seguente creiamo un negozio di contatori, che ci permetterà solo di aggiungere uno al contatore o di resettarne il valore:

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

Se la nostra app lista di cose da fare diventa troppo complessa, potremmo lasciare che il nostro negozio di to-dos gestisca ogni modifica di stato. Potremmo spostare tutti i metodi che modificano l'array `todo` (come `addTodo()`, `removeTodo()`, ecc.) dal nostro componente `Todos` al negozio. Se ha un unico luogo in cui viene applicata tutta la modifica dello stato, i componenti possono semplicemente chiamare quei metodi per modificare lo stato dell'app e visualizzare in modo reattivo le informazioni esposte dal negozio. Avere un unico luogo per gestire le modifiche di stato rende più facile ragionare sul flusso dello stato e individuare i problemi.

Svelte non la costringerà a organizzare la gestione del suo stato in un modo specifico; fornisce semplicemente gli strumenti per scegliere come gestirlo.

### Implementare il nostro negozio personalizzato di to-dos

La nostra app lista di cose da fare non è particolarmente complessa, quindi non sposteremo tutti i nostri metodi di modifica in un luogo centrale. Li lasceremo semplicemente come sono, e ci concentreremo invece sul mantenere i nostri to-dos.

> [!NOTE]
> Se sta seguendo questa guida lavorando dal REPL di Svelte, non sarà in grado di completare questo passaggio. Per motivi di sicurezza, il REPL di Svelte funziona in un ambiente sandbox che non le permetterà di accedere alla memorizzazione web, e otterrà un errore "L'operazione non è sicura". Per seguire questa sezione, dovrà clonare il repo e andare nella cartella `mdn-svelte-tutorial/06-stores`, o può scaricare direttamente il contenuto della cartella con `npx degit opensas/mdn-svelte-tutorial/06-stores`.

Per implementare un negozio personalizzato che salva il suo contenuto nella memorizzazione web, avremo bisogno di un negozio scrivibile che faccia quanto segue:

- Legge inizialmente il valore dalla memorizzazione web, e se non è presente, lo inizializza con un valore predefinito
- Ogni volta che il valore viene modificato, aggiorna il negozio stesso e anche i dati nella memorizzazione locale

Inoltre, poiché la memorizzazione web supporta solo il salvataggio di valori stringa, dovremo convertire da oggetto a stringa quando salviamo, e viceversa quando carichiamo il valore dalla memorizzazione locale.

1. Creare un nuovo file chiamato `localStore.js`, nella sua directory `src`.
2. Dargli il seguente contenuto:

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

   - Il nostro `localStore` sarà una funzione che quando eseguita legge inizialmente il suo contenuto dalla memorizzazione web, e restituisce un oggetto con tre metodi: `subscribe()`, `set()`, e `update()`.
   - Quando creiamo un nuovo `localStore`, dovremo specificare la chiave della memorizzazione web e un valore iniziale. Poi controlliamo se il valore esiste nella memorizzazione web e, in caso contrario, lo creiamo.
   - Usiamo i metodi [`localStorage.getItem(key)`](/it/docs/Web/API/Storage/getItem) e [`localStorage.setItem(key, value)`](/it/docs/Web/API/Storage/setItem) per leggere e scrivere informazioni nella memorizzazione web, e le funzioni di aiuto [`toString()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Object/toString) e `toObj()` (che usa [`JSON.parse()`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)) per convertire i valori.
   - Successivamente, convertiamo il contenuto stringa ricevuto dalla memorizzazione web in un oggetto, e salviamo quell'oggetto nel nostro negozio.
   - Infine, ogni volta che aggiorniamo i contenuti del negozio, aggiorniamo anche la memorizzazione web, con il valore convertito in una stringa.

   Noti che abbiamo dovuto ridefinire solo il metodo `set()`, aggiungendo l'operazione per salvare il valore nella memorizzazione web. Il resto del codice è per lo più inizializzazione e conversione delle cose.

3. Ora useremo il nostro negozio locale da `stores.js` per creare i nostri to-dos mantenuti localmente.

   Aggiornare `stores.js` in questo modo:

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

   Utilizzando `localStore('mdn-svelte-todo', initialTodos)`, stiamo configurando il negozio per salvare i dati nella memorizzazione web sotto la chiave `mdn-svelte-todo`. Impostiamo anche un paio di todo come valori iniziali.

4. Ora eliminiamo i to-dos predefiniti in `App.svelte`. Aggiornare i suoi contenuti come segue. Stiamo fondamentalmente eliminando l'array `$todos` e le istruzioni `console.log()`:

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
   > Questo è l'unico cambio che dobbiamo fare per usare il nostro negozio personalizzato. `App.svelte` è completamente trasparente in termini di che tipo di negozio stiamo usando.

5. Vada avanti e provi di nuovo la sua app. Crei alcuni to-dos e poi chiuda il browser. Può persino fermare il server Svelte e riavviarlo. Al ritorno all'URL, i suoi to-dos saranno ancora lì.
6. Può anche ispezionarli nella console DevTools. Nella console web, immetta il comando `localStorage.getItem('mdn-svelte-todo')`. Apporti alcune modifiche alla sua app, come premere il pulsante _Deseleziona tutto_ e controllare ancora una volta il contenuto della memorizzazione web. Otterrà qualcosa del genere:

   ![app to-do con vista console web accanto, che mostra che quando un to-do è cambiato nell'app, la voce corrispondente è cambiata nella memorizzazione web](03-persisting-todos-to-local-storage.png)

I negozi Svelte forniscono un modo molto semplice e leggero, ma estremamente potente, per gestire lo stato complesso dell'app da un negozio di dati globali in modo reattivo. E poiché Svelte compila il nostro codice, può fornire la [sintassi di auto-sottoscrizione `$store`](https://svelte.dev/docs/svelte-components#script-4-prefix-stores-with-$-to-access-their-values) che ci permette di lavorare con i negozi nello stesso modo delle variabili locali. Poiché i negozi hanno un'API minima, è molto semplice creare i nostri negozi personalizzati per astrarre via i dettagli del funzionamento interno del negozio stesso.

## Traccia bonus: Transizioni

Cambiamo ora argomento e facciamo qualcosa di divertente e diverso: aggiungiamo un'animazione ai nostri alert. Svelte fornisce un intero modulo per definire [transizioni](https://learn.svelte.dev/tutorial/transition) e [animazioni](https://learn.svelte.dev/tutorial/animate) in modo da rendere le nostre interfacce utente più attraenti.

Una transizione viene applicata con la direttiva [transition:fn](https://svelte.dev/docs/element-directives#transition-fn), ed è attivata da un elemento che entra o esce dal DOM come risultato di un cambiamento di stato.

Diamo una transizione 'fly' al nostro componente `Alert`. Apriremo il file `Alert.svelte` e importeremo la funzione `fly` dal modulo `svelte/transition`.

1. Metta la seguente istruzione `import` sotto quelle esistenti:

   ```js
   import { fly } from "svelte/transition";
   ```

2. Per usarla, aggiornare il suo tag di apertura `<div>` in questo modo:

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
   > Le doppie parentesi elle non sono una sintassi Svelte speciale. È semplicemente un oggetto JavaScript letterale passato come parametro alla transizione fly.

3. Provare di nuovo la sua app, e vedrà che le notifiche ora sembrano un po' più attraenti.

> [!NOTE]
> Essere un compilatore permette a Svelte di ottimizzare le dimensioni del nostro pacchetto escludendo le funzionalità che non sono usate. In questo caso, se compiliamo la nostra app per la produzione con `npm run build`, il nostro file `public/build/bundle.js` peserà poco meno di 22 KB. Se rimuoviamo la direttiva `transitions:fly` Svelte è abbastanza intelligente da capire che la funzione fly non è usata e la dimensione del file `bundle.js` scenderà a solo 18 KB.

Questo è solo la punta dell'iceberg. Svelte ha molte opzioni per trattare con animazioni e transizioni. Svelte supporta anche la specifica di diverse transizioni da applicare quando l'elemento viene aggiunto o rimosso dal DOM con le direttive `in:fn`/`out:fn`, e permette anche di definire le sue [transizioni CSS personalizzate](https://learn.svelte.dev/tutorial/custom-css-transitions) e [transizioni JavaScript personalizzate](https://learn.svelte.dev/tutorial/custom-js-transitions). Ha anche diverse funzioni di easing per specificare la velocità del cambiamento nel tempo. Dia un'occhiata al [visualizzatore di ease](https://svelte.dev/examples/easing) per esplorare le diverse funzioni di ease disponibili.

## Il codice finora

### Git

Per vedere lo stato del codice come dovrebbe essere alla fine di questo articolo, acceda alla sua copia del nostro repo in questo modo:

```bash
cd mdn-svelte-tutorial/07-next-steps
```

Oppure scaricare direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/07-next-steps
```

Ricordi di eseguire `npm install && npm run dev` per avviare la sua app in modalità di sviluppo.

### REPL

Per vedere lo stato attuale del codice in un REPL, visiti:

<https://svelte.dev/repl/378dd79e0dfe4486a8f10823f3813190?version=3.23.2>

## Riepilogo

In questo articolo abbiamo aggiunto due nuove funzionalità: un componente `Alert` e mantenere i `todos` nella memorizzazione web.

- Questo ci ha permesso di mostrare alcune tecniche avanzate di Svelte. Abbiamo sviluppato il componente `Alert` per mostrare come implementare la gestione dello stato tra componenti usando i negozi. Abbiamo anche visto come auto-abbonarci ai negozi per integrarli senza soluzione di continuità con il sistema di reattività di Svelte.
- Poi abbiamo visto come implementare il nostro negozio da zero, e anche come estendere il negozio scrivibile di Svelte per mantenere i dati nella memorizzazione web.
- Alla fine abbiamo dato un'occhiata all'uso della direttiva `transition` di Svelte per implementare le animazioni sugli elementi DOM.

Nel prossimo articolo impareremo come aggiungere il supporto TypeScript alla nostra applicazione Svelte. Per sfruttare tutte le sue funzionalità, porteremo anche l'intera nostra applicazione in TypeScript.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility","Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript", "Learn_web_development/Core/Frameworks_libraries")}}
