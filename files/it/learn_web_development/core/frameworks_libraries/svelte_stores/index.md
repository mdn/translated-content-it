---
title: Lavorare con gli store di Svelte
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_stores
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility","Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript", "Learn_web_development/Core/Frameworks_libraries")}}

Nell'ultimo articolo abbiamo completato lo sviluppo della nostra app, finito di organizzarla in componenti, e discusso alcune tecniche avanzate per gestire la reattività, lavorare con i nodi DOM ed esporre la funzionalità del componente. In questo articolo mostreremo un altro modo per gestire la gestione dello stato in Svelte: [Store](https://learn.svelte.dev/tutorial/writable-stores). Gli store sono repository di dati globali che contengono valori. I componenti possono iscriversi agli store e ricevere notifiche quando i loro valori cambiano.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È consigliato almeno avere familiarità con le basi dei linguaggi
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/command line</a
          >.
        </p>
        <p>
          Avrà bisogno di un terminale con node e npm installati per compilare e costruire
          la sua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare a usare gli store di Svelte</td>
    </tr>
  </tbody>
</table>

Usando gli store creeremo un componente `Alert` che mostra notifiche sullo schermo, che può ricevere messaggi da qualsiasi componente. In questo caso, il componente `Alert` è indipendente dal resto — non è un genitore o un figlio di nessun altro — quindi i messaggi non si adattano alla gerarchia del componente.

Vedremo anche come sviluppare il nostro store personalizzato per memorizzare le informazioni dei todo nel [web storage](/it/docs/Web/API/Web_Storage_API), permettendo ai nostri to-do di persistere oltre i ricaricamenti della pagina.

## Codifichi con noi

### Git

Cloni il repository GitHub (se non l'ha già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi, per arrivare allo stato attuale dell'app, esegua

```bash
cd mdn-svelte-tutorial/06-stores
```

Oppure scarichi direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/06-stores
```

Ricordi di eseguire `npm install && npm run dev` per avviare la sua app in modalità sviluppo.

### REPL

Per codificare con noi usando REPL, cominci da

<https://svelte.dev/repl/d1fa84a5a4494366b179c87395940039?version=3.23.2>

## Gestire lo stato della nostra app

Abbiamo già visto come i nostri componenti possano comunicare tra di loro usando props, binding bidirezionale dei dati, ed eventi. In tutti questi casi ci stavamo occupando di comunicazione tra componenti padre e figlio.

Ma non tutto lo stato dell'applicazione appartiene alla gerarchia dei componenti dell'applicazione. Ad esempio, informazioni sull'utente autenticato, o se il tema scuro è selezionato o meno.

A volte, lo stato della sua app avrà bisogno di essere accessibile da più componenti che non sono gerarchicamente collegati, o da un modulo JavaScript regolare.

Inoltre, quando la sua app diventa complicata e la sua gerarchia di componenti diventa complessa, potrebbe diventare troppo difficile per i componenti trasmettere dati tra di loro. In quel caso, passare a uno store di dati globale potrebbe essere una buona opzione. Se ha già lavorato con [Redux](https://redux.js.org/) o [Vuex](https://vuex.vuejs.org/), allora sarà familiare con il funzionamento di questo tipo di store. Gli store di Svelte offrono funzionalità simili per la gestione dello stato.

Uno store è un oggetto con un metodo `subscribe()` che permette alle parti interessate di essere notificate ogni volta che il valore dello store cambia e un metodo opzionale `set()` che le permette di impostare nuovi valori per lo store. Questo API minima è conosciuta come il [contratto dello store](https://svelte.dev/docs/svelte-components#script-4-prefix-stores-with-$-to-access-their-values-store-contract).

Svelte fornisce funzioni per creare store [readable](https://svelte.dev/docs/svelte-store#readable), [writable](https://svelte.dev/docs/svelte-store#writable), e [derived](https://svelte.dev/docs/svelte-store#derived) nel modulo `svelte/store`.

Svelte fornisce anche un modo molto intuitivo per integrare gli store nel suo sistema di reattività usando la [sintassi reattiva `$store`](https://svelte.dev/docs/svelte-components#script-4-prefix-stores-with-$-to-access-their-values). Se crea i suoi store rispettando il contratto dello store, ottiene questo zucchero sintattico di reattività gratuitamente.

## Creare il componente Alert

Per mostrare come lavorare con gli store, creeremo un componente `Alert`. Questi tipi di widget potrebbero essere conosciuti anche come popup notifiche, toast, o bolle di notifica.

Il nostro componente `Alert` sarà visualizzato dal componente `App`, ma qualsiasi componente può inviare notifiche ad esso. Ogni volta che arriva una notifica, il componente `Alert` sarà incaricato di visualizzarla sullo schermo.

### Creare uno store

Iniziamo creando uno store scrivibile. Qualsiasi componente sarà in grado di scrivere in questo store, e il componente `Alert` si iscriverà ad esso e visualizzerà un messaggio ogni volta che lo store viene modificato.

1. Creare un nuovo file, `stores.js`, all'interno della sua directory `src`.
2. Inserisci il seguente contenuto:

   ```js
   import { writable } from "svelte/store";

   export const alert = writable("Welcome to the to-do list app!");
   ```

> [!NOTE]
> Gli store possono essere definiti e utilizzati al di fuori dei componenti Svelte, quindi può organizzarli come preferisce.

Nel codice sopra abbiamo importato la funzione `writable()` da `svelte/store` e la usiamo per creare un nuovo store chiamato `alert` con un valore iniziale di "Benvenuto nell'app lista delle cose da fare!". Poi `esportiamo` lo store.

### Creare il vero componente

Creiamo ora il nostro componente `Alert` e vediamo come possiamo leggere i valori dallo store.

1. Creare un altro nuovo file denominato `src/components/Alert.svelte`.
2. Inserisci il seguente contenuto:

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

Analizziamo nel dettaglio questo pezzo di codice.

- All'inizio importiamo lo store `alert`.
- Successivamente importiamo la funzione di ciclo di vita `onDestroy()` che ci permette di eseguire un callback dopo che il componente è stato smontato.
- Creiamo quindi una variabile locale denominata `alertContent`. Ricordi che possiamo accedere alle variabili di livello superiore dal markup e ogni volta che vengono modificate, il DOM si aggiornerà di conseguenza.
- Poi chiamiamo il metodo `alert.subscribe()`, passando come parametro una funzione di callback. Ogni volta che il valore dello store cambia, la funzione di callback verrà chiamata con il nuovo valore come parametro. Nella funzione di callback assegniamo semplicemente il valore ricevuto alla variabile locale, che attiverà l'aggiornamento del DOM del componente.
- Il metodo `subscribe()` restituisce anche una funzione di pulizia, che si occupa di rilasciare l'iscrizione. Quindi ci iscriviamo quando il componente viene inizializzato e usiamo `onDestroy` per cancellare l'iscrizione quando il componente viene smontato.
- Infine utilizziamo la variabile `alertContent` nel nostro markup, e se l'utente clicca sull'alert, lo puliamo.
- Alla fine includiamo alcune linee di CSS per stilizzare il nostro componente `Alert`.

Questa configurazione ci permette di lavorare con gli store in modo reattivo. Quando il valore dello store cambia, il callback viene eseguito. Lì assegniamo un nuovo valore a una variabile locale, e grazie alla reattività di Svelte tutto il nostro markup e le nostre dipendenze reattive vengono aggiornati di conseguenza.

### Usare il componente

Utilizziamo ora il nostro componente.

1. In `App.svelte` importeremo il componente. Aggiungi la seguente dichiarazione di importazione sotto quella esistente:

   ```js
   import Alert from "./components/Alert.svelte";
   ```

2. Quindi chiama il componente `Alert` subito sopra la chiamata `Todos`, così:

   ```svelte
   <Alert />
   <Todos {todos} />
   ```

3. Carichi ora la sua app di test e dovrebbe ora vedere il messaggio di `Alert` sullo schermo. Può cliccarci sopra per eliminarlo.

   ![Una semplice notifica nell'angolo in alto a destra di un'app che dice benvenuto nell'app lista delle cose da fare](01-alert-message.png)

## Rendere gli store reattivi con la sintassi reattiva `$store`

Questo funziona, ma dovrà copiare e incollare tutto questo codice ogni volta che vuole iscriversi a uno store:

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

È troppo codice ridondante per Svelte! Essendo un compilatore, Svelte ha più risorse per rendere la nostra vita più facile. In questo caso, Svelte fornisce la sintassi reattiva `$store`, conosciuta anche come auto-iscrizione. In termini semplici, basta anteporre il segno `$` allo store e Svelte genererà il codice per renderlo automaticamente reattivo. Quindi il nostro blocco di codice precedente può essere sostituito da questo:

```svelte
<script>
  import myStore from "./stores.js";
</script>

{$myStore}
```

E `$myStore` sarà completamente reattivo. Questo si applica anche ai suoi store personalizzati. Se implementa i metodi `subscribe()` e `set()`, come faremo dopo, la sintassi reattiva `$store` si applicherà anche ai suoi store.

1. Applichiamo questo al nostro componente `Alert`. Aggiorni le sezioni `<script>` e markup di `Alert.svelte` come segue:

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

2. Controlli di nuovo la sua app e vedrà che funziona proprio come prima. Molto meglio!

Dietro le quinte, Svelte ha generato il codice per dichiarare la variabile locale `$alert`, iscriversi allo store `alert`, aggiornare `$alert` ogni volta che il contenuto dello store viene modificato, e annullare l'iscrizione quando il componente viene smontato. Genererà anche le istruzioni `alert.set()` ogni volta che assegnamo un valore a `$alert`.

Il risultato finale di questo trucco ingegnoso è che può accedere agli store globali così facilmente come usare variabili locali reattive.

Questo è un perfetto esempio di come Svelte metta il compilatore a carico di un migliore ergonomia per lo sviluppatore, non solo salvandoci dal digitare codice ridondante, ma anche generando un codice meno incline agli errori.

## Scrivere nel nostro store

Scrivere nel nostro store è semplicemente una questione di importarlo ed eseguire `$store = 'nuovo valore'`. Usiamolo nel nostro componente `Todos`.

1. Aggiungi la seguente dichiarazione di importazione sotto quelle esistenti:

   ```js
   import { alert } from "../stores.js";
   ```

2. Aggiorni la sua funzione `addTodo()` in questo modo:

   ```js
   function addTodo(name) {
     todos = [...todos, { id: newTodoId, name, completed: false }];
     $alert = `Todo '${name}' has been added`;
   }
   ```

3. Aggiorni `removeTodo()` così:

   ```js
   function removeTodo(todo) {
     todos = todos.filter((t) => t.id !== todo.id);
     todosStatus.focus(); // give focus to status heading
     $alert = `Todo '${todo.name}' has been deleted`;
   }
   ```

4. Aggiorni la funzione `updateTodo()` a questo:

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

5. Aggiunga il seguente blocco reattivo sotto il blocco che inizia con `let filter = 'all'`:

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

6. E infine per ora, aggiorni i blocchi `const checkAllTodos` e `const removeCompletedTodos` come segue:

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

7. Quindi fondamentalmente, abbiamo importato lo store e lo abbiamo aggiornato a ogni evento, il che provoca la visualizzazione di un nuovo avviso ogni volta. Guardi di nuovo la sua app, e provi ad aggiungere/cancellare/aggiornare alcuni to-do!

Non appena eseguiamo `$alert = …`, Svelte eseguirà `alert.set()`. Il nostro componente `Alert` — come ogni iscritto allo store di avviso — sarà notificato quando riceve un nuovo valore, e grazie alla reattività di Svelte il suo markup sarà aggiornato.

Potremmo fare lo stesso all'interno di qualsiasi componente o file `.js`.

> [!NOTE]
> Al di fuori dei componenti Svelte non può usare la sintassi `$store`. Questo perché il compilatore Svelte non toccherà nulla al di fuori dei componenti Svelte. In tal caso dovrà affidarsi ai metodi `store.subscribe()` e `store.set()`.

## Migliorare il nostro componente Alert

È un po' fastidioso dover cliccare sull'alert per eliminarlo. Sarebbe meglio se la notifica scomparisse semplicemente dopo un paio di secondi.

Vediamo come farlo. Specificheremo una prop con i millisecondi da aspettare prima di eliminare la notifica, e definiremo un timeout per rimuovere l'alert. Ci prenderemo anche cura di cancellare il timeout quando il componente `Alert` viene smontato per prevenire perdite di memoria.

1. Aggiorni la sezione `<script>` del suo componente `Alert.svelte` in questo modo:

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

2. E aggiorni la sezione del markup `Alert.svelte` così:

   ```svelte
   {#if visible}
   <div on:click={() => visible = false}>
     <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M12.432 0c1.34 0 2.01.912 2.01 1.957 0 1.305-1.164 2.512-2.679 2.512-1.269 0-2.009-.75-1.974-1.99C9.789 1.436 10.67 0 12.432 0zM8.309 20c-1.058 0-1.833-.652-1.093-3.524l1.214-5.092c.211-.814.246-1.141 0-1.141-.317 0-1.689.562-2.502 1.117l-.528-.88c2.572-2.186 5.531-3.467 6.801-3.467 1.057 0 1.233 1.273.705 3.23l-1.391 5.352c-.246.945-.141 1.271.106 1.271.317 0 1.357-.392 2.379-1.207l.6.814C12.098 19.02 9.365 20 8.309 20z"/></svg>
     <p>{ $alert }</p>
   </div>
   {/if}
   ```

Qui creiamo prima la prop `ms` con un valore predefinito di 3000 (millisecondi). Poi creiamo una funzione `onMessageChange()` che si occuperà di controllare se l'Alert è visibile o meno. Con `$: onMessageChange($alert, ms)` diciamo a Svelte di eseguire questa funzione ogni volta che lo store `$alert` o la prop `ms` cambiano.

Ogni volta che lo store `$alert` cambia, cancelleremo qualsiasi timeout pendente. Se `$alert` è vuoto, impostiamo `visible` su `false` e l'Alert sarà rimosso dal DOM. Se non è vuoto, impostiamo `visible` su `true` e usiamo la funzione `setTimeout()` per eliminare l'alert dopo `ms` millisecondi.

Infine, con la funzione di ciclo di vita `onDestroy()`, ci assicuriamo di chiamare la funzione `clearTimeout()`.

Abbiamo anche aggiunto un'icona SVG sopra il paragrafo dell'alert, per renderlo un po' più carino. Lo provi di nuovo, e dovrebbe vedere i cambiamenti.

## Rendere accessibile il nostro componente Alert

Il nostro componente `Alert` funziona bene, ma non è molto amichevole per le tecnologie assistive. Il problema sono gli elementi che vengono aggiunti dinamicamente e rimossi dalla pagina. Mentre sono visivamente evidenti per gli utenti che possono vedere la pagina, potrebbero non essere così ovvi per gli utenti delle tecnologie assistive, come i lettori di schermo. Per affrontare queste situazioni, possiamo approfittare delle [regioni ARIA live](/it/docs/Web/Accessibility/ARIA/Guides/Live_regions), che forniscono un modo per esporre programmaticamente i cambiamenti di contenuto dinamici, in modo che possano essere rilevati e annunciati dalle tecnologie assistive.

Possiamo dichiarare una regione che contiene contenuti dinamici che dovrebbero essere annunciati dalle tecnologie assistive con la proprietà `aria-live` seguita dall'impostazione di cortesia, che viene utilizzata per impostare la priorità con cui i lettori di schermo dovrebbero gestire gli aggiornamenti a quelle regioni. Le impostazioni possibili sono `off`, `polite`, o `assertive`.

Per situazioni comuni, ci sono anche diversi valori di `role` specializzati predefiniti che possono essere usati, come `log`, `status` e `alert`.

Nel nostro caso, basta aggiungere un `role="alert"` al contenitore `<div>` per fare il trucco, come questo:

```svelte
<div role="alert" on:click={() => visible = false}>
```

In generale, testare le sue applicazioni utilizzando i lettori di schermo è una buona idea, non solo per scoprire problemi di accessibilità, ma anche per abituarsi a come le persone ipovedenti usano il Web. Ha diverse opzioni, come [NVDA](https://www.nvaccess.org/) per Windows, [ChromeVox](https://support.google.com/chromebook/answer/7031755) per Chrome, [Orca](https://wiki.gnome.org/Projects/Orca) su Linux, e [VoiceOver](https://www.apple.com/accessibility/vision/) per macOS e iOS, tra le altre opzioni.

Per saperne di più su come rilevare e correggere i problemi di accessibilità, controlli il nostro modulo [Accessibilità](/it/docs/Learn_web_development/Core/Accessibility).

## Usare il contratto dello store per memorizzare i nostri to-do

La nostra piccola app ci permette di gestire i nostri to-do abbastanza facilmente, ma è piuttosto inutile se otteniamo sempre la stessa lista di to-do predefiniti quando la ricarichiamo. Per renderla veramente utile, dobbiamo capire come memorizzare i nostri to-do.

Prima abbiamo bisogno di un modo per il nostro componente `Todos` di restituire i to-do aggiornati al suo genitore. Potremmo emettere un evento aggiornato con l'elenco dei to-do, ma è più facile solo collegare la variabile `todos`. Apriamo `App.svelte` e proviamoci.

1. Prima, aggiungi la seguente riga sotto il tuo array `todos`:

   ```js
   $: console.log("todos", todos);
   ```

2. Poi, aggiorna la tua chiamata al componente `Todos` come segue:

   ```svelte
   <Todos bind:todos />
   ```

   > **Nota:** `<Todos bind:todos />` è solo un'abbreviazione per `<Todos bind:todos={todos} />`.

3. Torni alla sua app, provi ad aggiungere alcuni to-do, poi vada alla console degli strumenti per sviluppatori web. Vedrà che ogni modifica che facciamo ai nostri to-do si riflette nell'array `todos` definito in `App.svelte` grazie alla direttiva `bind`.

Ora dobbiamo trovare un modo per memorizzare questi to-do. Potremmo implementare del codice nel nostro componente `App.svelte` per leggere e salvare i nostri to-do nel [web storage](/it/docs/Web/API/Web_Storage_API) o in un servizio web.
Ma non sarebbe meglio se potessimo sviluppare qualche store generica che ci permetta di persistere i suoi contenuti? Questo ci permetterebbe di usarlo proprio come qualsiasi altro store, e astrarre via il meccanismo di persistenza. Potremmo creare uno store che sincronizzi i suoi contenuti con il web storage, e in seguito sviluppare un altro che sincronizzi contro un servizio web. Passare tra di loro sarebbe banale e non dovremmo toccare affatto `App.svelte`.

### Salvare i nostri to-do

Cominciamo quindi usando un normale store scrivibile per salvare i nostri to-do.

1. Apri il file `stores.js` e aggiunga il seguente store sotto quello esistente:

   ```js
   export const todos = writable([]);
   ```

2. È stato facile. Ora dobbiamo importare lo store e usarlo in `App.svelte`. Ricordi solo che per accedere ai to-do ora dobbiamo usare la sintassi reattiva `$todos`.

   Aggiorni il suo file `App.svelte` in questo modo:

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

3. Lo provi; tutto dovrebbe funzionare. Successivamente vedremo come definire i nostri store personalizzati.

### Come implementare un contratto di store: La teoria

Può creare i suoi store senza affidarsi a `svelte/store` implementando il contratto di store. Le sue caratteristiche devono funzionare così:

1. Uno store deve contenere un metodo `subscribe()`, che deve accettare come argomento una funzione di sottoscrizione. Tutte le funzioni di sottoscrizione attive di uno store devono essere chiamate ogni volta che il valore dello store cambia.
2. Il metodo `subscribe()` deve restituire una funzione `unsubscribe()`, che quando chiamata deve interrompere la sua sottoscrizione.
3. Uno store può contenere facoltativamente un metodo `set()`, che deve accettare come argomento un nuovo valore per lo store, e che chiama in modo sincrono tutte le funzioni di sottoscrizione attive dello store. Uno store con un metodo `set()` è chiamato uno store scrivibile.

Prima di tutto, aggiunga le seguenti istruzioni `console.log()` al nostro componente `App.svelte` per vedere lo store `todos` e il suo contenuto in azione. Aggiunga queste righe sotto l'array `todos`:

```js
console.log("todos store - todos:", todos);
console.log("todos store content - $todos:", $todos);
```

Quando esegue l'app ora, vedrà qualcosa del genere nella sua console web:

![console web che mostra le funzioni e i contenuti dello store dei todo](02-svelte-store-in-action.png)

Come può vedere, il nostro store è solo un oggetto contenente i metodi `subscribe()`, `set()`, e `update()`, e `$todos` è il nostro array di to-do.

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

Qui dichiariamo `subs`, che è un array di sottoscrittori. Nel metodo `subscribe()` aggiungiamo il gestore all'array `subs` e restituiamo una funzione che, quando eseguita, rimuoverà il gestore dall'array.

Quando chiamiamo `set()`, aggiorniamo il valore dello store e chiamiamo ogni gestore, passando il nuovo valore come parametro.

Di solito non si implementano store da zero; invece si utilizza lo store scrivibile per creare [store personalizzati](https://learn.svelte.dev/tutorial/custom-stores) con logica specifica del dominio. Nel seguente esempio creiamo uno store contatore, che ci permetterà solo di aggiungere uno al contatore o di resettare il suo valore:

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

Se la nostra app della lista delle cose da fare diventa troppo complessa, potremmo lasciare che il nostro store dei to-do gestisca ogni modifica dello stato. Potremmo spostare tutti i metodi che modificano l'array `todo` (come `addTodo()`, `removeTodo()`, ecc.) dal nostro componente `Todos` allo store. Se ha un unico posto dove viene applicata ogni modifica dello stato, i componenti potrebbero semplicemente chiamare quei metodi per modificare lo stato dell'app e visualizzare reattivamente le informazioni esposte dallo store. Avere un unico posto per gestire le modifiche di stato rende più facile ragionare sul flusso di stato e individuare i problemi.

Svelte non la obbligherà a organizzare la sua gestione dello stato in un modo specifico; fornirà solo gli strumenti per scegliere come gestirlo.

### Implementare il nostro store personalizzato di to-do

La nostra app di lista delle cose da fare non è particolarmente complessa, quindi non sposteremo tutti i nostri metodi di modifica in un unico posto. Le lasceremo così come sono, e ci concentreremo invece sulla persistenza dei nostri to-do.

> [!NOTE]
> Se sta seguendo questa guida lavorando dal Svelte REPL, non sarà in grado di completare questo passaggio. Per motivi di sicurezza, il Svelte REPL funziona in un ambiente isolato che non le permetterà di accedere al web storage, e riceverà un errore "L'operazione non è sicura". Per seguire questa sezione, dovrà clonare il repository e andare alla cartella `mdn-svelte-tutorial/06-stores`, oppure può scaricare direttamente il contenuto della cartella con `npx degit opensas/mdn-svelte-tutorial/06-stores`.

Per implementare un store personalizzato che salva i suoi contenuti nel web storage, avremo bisogno di uno store scrivibile che faccia il seguente:

- Inizialmente legge il valore dal web storage, e se non è presente, lo inizializza con un valore predefinito
- Ogni volta che il valore viene modificato, aggiorna lo store stesso e anche i dati nel local storage

Inoltre, poiché il web storage supporta solo il salvataggio di valori stringa, dovremo convertire da oggetto a stringa quando salviamo, e viceversa quando carichiamo il valore dal local storage.

1. Creare un nuovo file chiamato `localStore.js`, nella sua directory `src`.
2. Inserisca il seguente contenuto:

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

   - Il nostro `localStore` sarà una funzione che quando eseguita inizialmente legge il suo contenuto dal web storage, e restituisce un oggetto con tre metodi: `subscribe()`, `set()`, e `update()`.
   - Quando creiamo un nuovo `localStore`, dovremo specificare la chiave del web storage e un valore iniziale. Poi controlliamo se il valore esiste nel web storage e, se no, lo creiamo.
   - Utilizziamo i metodi [`localStorage.getItem(key)`](/it/docs/Web/API/Storage/getItem) e [`localStorage.setItem(key, value)`](/it/docs/Web/API/Storage/setItem) per leggere e scrivere informazioni nel web storage, e le funzioni di aiuto [`toString()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Object/toString) e `toObj()` (che usa [`JSON.parse()`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)) per convertire i valori.
   - Successivamente, convertiamo il contenuto stringa ricevuto dal web storage in un oggetto, e salviamo quell'oggetto nel nostro store.
   - Infine, ogni volta che aggiorniamo i contenuti dello store, aggiorniamo anche il web storage, con il valore convertito in una stringa.

   Noti che abbiamo dovuto ridefinire solo il metodo `set()`, aggiungendo l'operazione per salvare il valore nel web storage. Il resto del codice è principalmente inizializzazione e conversione di cose.

3. Ora useremo il nostro local store da `stores.js` per creare i nostri to-do memorizzati localmente.

   Aggiorni `stores.js` in questo modo:

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

   Usando `localStore('mdn-svelte-todo', initialTodos)`, stiamo configurando lo store per salvare i dati nel web storage sotto la chiave `mdn-svelte-todo`. Abbiamo anche impostato un paio di to-do come valori iniziali.

4. Ora eliminiamo i to-do preimpostati in `App.svelte`. Aggiorni i suoi contenuti come segue. Stiamo fondamentalmente solo eliminando l'array `$todos` e le istruzioni `console.log()`:

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
   > Questo è l'unico cambiamento che dobbiamo fare per usare il nostro store personalizzato. `App.svelte` è completamente trasparente in termini di quale tipo di store stiamo usando.

5. Provi di nuovo la sua app. Crei alcuni to-do e poi chiuda il browser. Può anche arrestare il server Svelte e riavviarlo. Al rientro nell'URL, i suoi to-do saranno ancora lì.
6. Può anche ispezionarlo nella console DevTools. Nella console web, inserisca il comando `localStorage.getItem('mdn-svelte-todo')`. Faccia alcune modifiche alla sua app, come premere il pulsante _Seleziona Tutto_, e ricontrolli il contenuto del web storage. Otterrà qualcosa del genere:

   ![app di to-do con vista console web accanto, che mostra che quando un to-do viene cambiato nell'app, la voce corrispondente viene cambiata nel web storage](03-persisting-todos-to-local-storage.png)

Gli store di Svelte forniscono un modo molto semplice e leggero, ma estremamente potente, per gestire lo stato complesso dell'app da uno store di dati globale in modo reattivo. E poiché Svelte compila il nostro codice, può fornire la sintassi [`$store` auto-subscription](https://svelte.dev/docs/svelte-components#script-4-prefix-stores-with-$-to-access-their-values) che ci permette di lavorare con gli store nello stesso modo delle variabili locali. Poiché gli store hanno un API minimale, è molto semplice creare gli store personalizzati per astrarre via i meccanismi interni dello store stesso.

## Bonus track: Transizioni

Cambiamo ora argomento e facciamo qualcosa di divertente e diverso: aggiungere un'animazione ai nostri alert. Svelte fornisce un intero modulo per definire [transizioni](https://learn.svelte.dev/tutorial/transition) e [animazioni](https://learn.svelte.dev/tutorial/animate) in modo da poter rendere le nostre interfacce utente più attraenti.

Una transizione viene applicata con la direttiva [transition:fn](https://svelte.dev/docs/element-directives#transition-fn), e viene innescata da un elemento che entra o esce dal DOM a seguito di un cambiamento di stato.

Diamo alla nostra `Alert` un `transition` usando il `fly`. Apriremo il file `Alert.svelte` e importeremo la funzione `fly` dal modulo `svelte/transition`.

1. Inserisca la seguente dichiarazione di importazione sotto quelle esistenti:

   ```js
   import { fly } from "svelte/transition";
   ```

2. Per usarlo, aggiorni il tag di apertura `<div>` come segue:

   ```svelte
   <div role="alert" on:click={() => visible = false}
     transition:fly
   >
   ```

   Le transizioni possono ricevere anche parametri, così:

   ```svelte
   <div role="alert" on:click={() => visible = false}
     transition:fly="\{{delay: 250, duration: 300, x: 0, y: -100, opacity: 0.5}}"
   >
   ```

   > [!NOTE]
   > Le doppie parentesi graffe non sono una sintassi speciale di Svelte. È solo un oggetto JavaScript letterale passato come parametro alla transizione fly.

3. Provi di nuovo la sua app, e vedrà che le notifiche ora sembrano un po' più accattivanti.

> [!NOTE]
> Essere un compilatore permette a Svelte di ottimizzare la dimensione del nostro bundle escludendo funzionalità che non sono usate. In questo caso, se compiliamo la nostra app per la produzione con `npm run build`, il nostro file `public/build/bundle.js` peserà poco meno di 22 KB. Se rimuoviamo la direttiva `transitions:fly`, Svelte è abbastanza intelligente da riconoscere che la funzione fly non è utilizzata e la dimensione del file `bundle.js` scenderà a soli 18 KB.

Questo è solo la punta dell'iceberg. Svelte ha molte opzioni per gestire le animazioni e le transizioni. Svelte supporta anche la specificazione di diverse transizioni da applicare quando l'elemento viene aggiunto o rimosso dal DOM con le direttive `in:fn`/`out:fn`, e permette anche di definire le proprie [transizioni CSS personalizzate](https://learn.svelte.dev/tutorial/custom-css-transitions) e [JavaScript personalizzate](https://learn.svelte.dev/tutorial/custom-js-transitions). Ha anche diverse funzioni di easing per specificare il tasso di cambiamento nel tempo. Dia un'occhiata al [visualizzatore di easing](https://svelte.dev/examples/easing) per esplorare le varie funzioni di easing disponibili.

## Il codice finora

### Git

Per vedere lo stato del codice come dovrebbe essere alla fine di questo articolo, acceda alla sua copia del nostro repo così:

```bash
cd mdn-svelte-tutorial/07-next-steps
```

Oppure scarichi direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/07-next-steps
```

Ricordi di eseguire `npm install && npm run dev` per avviare la sua app in modalità sviluppo.

### REPL

Per vedere lo stato attuale del codice in un REPL, visiti:

<https://svelte.dev/repl/378dd79e0dfe4486a8f10823f3813190?version=3.23.2>

## Sommario

In questo articolo abbiamo aggiunto due nuove funzionalità: un componente `Alert` e la memorizzazione dei `todos` nel web storage.

- Questo ci ha permesso di mostrare alcune tecniche avanzate di Svelte. Abbiamo sviluppato il componente `Alert` per mostrare come implementare la gestione dello stato tra componenti utilizzando gli store. Abbiamo anche visto come iscriversi automaticamente agli store per integrarli senza soluzione di continuità nel sistema di reattività di Svelte.
- Poi abbiamo visto come implementare il nostro store da zero, e anche come estendere lo store scrivibile di Svelte per memorizzare i dati nel web storage.
- Alla fine abbiamo dato un'occhiata a come usare la direttiva `transition` di Svelte per implementare animazioni sugli elementi del DOM.

Nel prossimo articolo impareremo come aggiungere il supporto a TypeScript nella nostra applicazione Svelte. Per sfruttare tutte le sue funzionalità, porteremo anche tutta la nostra applicazione a TypeScript.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility","Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript", "Learn_web_development/Core/Frameworks_libraries")}}
