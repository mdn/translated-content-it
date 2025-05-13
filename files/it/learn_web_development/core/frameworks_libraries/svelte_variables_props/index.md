---
title: "Comportamento dinamico in Svelte: lavorare con variabili e props"
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_variables_props
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_Todo_list_beginning","Learn_web_development/Core/Frameworks_libraries/Svelte_components", "Learn_web_development/Core/Frameworks_libraries")}}

Ora che abbiamo il nostro markup e gli stili pronti, possiamo iniziare a sviluppare le funzionalità necessarie per la nostra app di lista di cose da fare con Svelte. In questo articolo useremo variabili e props per rendere la nostra app dinamica, consentendoci di aggiungere ed eliminare compiti, segnarli come completati e filtrarli per stato.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Si raccomanda di avere familiarità con le basi dei linguaggi
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          di avere conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/linea di comando</a
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
      <td>
        Apprenda e metta in pratica alcuni concetti base di Svelte, come creare
        componenti, passare dati utilizzando props, rendere le espressioni JavaScript nel
        nostro markup, modificare lo stato dei componenti e iterare su liste.
      </td>
    </tr>
  </tbody>
</table>

## Codice passo a passo con noi

### Git

Cloni il repo di GitHub (se non lo ha già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi, per raggiungere lo stato attuale dell'app, esegua

```bash
cd mdn-svelte-tutorial/03-adding-dynamic-behavior
```

Oppure scarichi direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/03-adding-dynamic-behavior
```

Ricordi di eseguire `npm install && npm run dev` per avviare la sua app in modalità sviluppo.

### REPL

Per programmare passo a passo con noi utilizzando il REPL, inizi su

<https://svelte.dev/repl/c862d964d48d473ca63ab91709a0a5a0?version=3.23.2>

## Lavorare con i to-do

Il nostro componente `Todos.svelte` sta attualmente solo visualizzando il markup statico; iniziamo a renderlo un po' più dinamico. Prenderemo le informazioni sui compiti dal markup e le memorizzeremo in un array `todos`. Creeremo anche due variabili per tenere traccia del numero totale di compiti e dei compiti completati.

Lo stato del nostro componente sarà rappresentato da queste tre variabili di alto livello.

1. Crei una sezione `<script>` in cima a `src/components/Todos.svelte` e le assegni del contenuto, come segue:

   ```svelte
   <script>
     let todos = [
       { id: 1, name: "Create a Svelte starter app", completed: true },
       { id: 2, name: "Create your first component", completed: true },
       { id: 3, name: "Complete the rest of the tutorial", completed: false }
     ];
     let totalTodos = todos.length;
     let completedTodos = todos.filter((todo) => todo.completed).length;
   </script>
   ```

   Ora facciamo qualcosa con quelle informazioni.

2. Iniziamo mostrando un messaggio di stato. Trovi la intestazione di `<h2>` con un `id` di `list-heading` e sostituisca il numero hardcoded di compiti attivi e completati con espressioni dinamiche:

   ```svelte
   <h2 id="list-heading">{completedTodos} out of {totalTodos} items completed</h2>
   ```

3. Acceda all'app e dovrebbe vedere il messaggio "2 su 3 elementi completati" come prima, ma questa volta le informazioni provengono dall'array `todos`.
4. Per provarlo, vada a quell'array e provi a cambiare alcune delle proprietà completed degli oggetti to-do, e persino ad aggiungere un nuovo oggetto to-do. Osservi come i numeri nel messaggio vengano aggiornati in modo appropriato.

## Generare dinamicamente i to-do dai dati

Al momento, i nostri elementi to-do visualizzati sono tutti statici. Vogliamo iterare su ogni elemento nel nostro array `todos` e rendere il markup per ogni compito, quindi facciamolo ora.

L'HTML non ha un modo per esprimere logica — come condizionali e cicli. Svelte sì. In questo caso usiamo la direttiva [`{#each}`](https://svelte.dev/docs/logic-blocks#each) per iterare sull'array `todos`. Il secondo parametro, se fornito, conterrà l'indice dell'elemento corrente. Inoltre, può essere fornita un'espressione chiave, che identificherà in modo univoco ciascun elemento. Svelte la userà per differenziare gli elementi nella lista quando i dati cambiano, piuttosto che aggiungere o rimuovere elementi alla fine, ed è una buona pratica specificarne sempre una. Infine, può essere fornito un blocco `:else`, che verrà renderizzato quando la lista è vuota.

Proviamolo.

1. Sostituisca l'elemento `<ul>` esistente con la seguente versione semplificata per capire come funziona:

   ```svelte
   <ul>
   {#each todos as todo, index (todo.id)}
     <li>
       <input type="checkbox" checked={todo.completed}/> {index}. {todo.name} (id: {todo.id})
     </li>
   {:else}
     Nothing to do here!
   {/each}
   </ul>
   ```

2. Torni all'app; vedrà qualcosa del genere:

   ![output di lista di cose da fare molto semplice creato usando un blocco each](01-each-block.png)

3. Ora che abbiamo visto che funziona, generiamo un elemento completo da fare con ogni ciclo della direttiva `{#each}`, e all'interno incorporiamo le informazioni dall'array `todos`: `id`, `name` e `completed`. Sostituisca il blocco `<ul>` esistente con il seguente:

   ```svelte
   <!-- To-dos -->
   <ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
     {#each todos as todo (todo.id)}
     <li class="todo">
       <div class="stack-small">
         <div class="c-cb">
           <input
             type="checkbox"
             id="todo-{todo.id}"
             checked={todo.completed} />
           <label for="todo-{todo.id}" class="todo-label"> {todo.name} </label>
         </div>
         <div class="btn-group">
           <button type="button" class="btn">
             Edit <span class="visually-hidden">{todo.name}</span>
           </button>
           <button type="button" class="btn btn__danger">
             Delete <span class="visually-hidden">{todo.name}</span>
           </button>
         </div>
       </div>
     </li>
     {:else}
     <li>Nothing to do here!</li>
     {/each}
   </ul>
   ```

   Noti come stiamo usando le parentesi graffe per incorporare espressioni JavaScript negli attributi HTML, come abbiamo fatto con gli attributi `checked` e `id` della casella di controllo.

Abbiamo trasformato il nostro markup statico in un modello dinamico pronto a visualizzare le attività dallo stato del nostro componente. Ottimo! Ci stiamo arrivando.

## Lavorare con i props

Con un elenco hardcoded di to-dos, il nostro componente `Todos` non è molto utile. Per trasformare il nostro componente in un editor to-do generico, dovremmo permettere al padre di questo componente di passare la lista dei to-do da modificare. Ciò ci permetterebbe di salvarli su un servizio web o nello storage locale e, successivamente, recuperarli per l'aggiornamento. Quindi trasformiamo l'array in un `prop`.

1. In `Todos.svelte`, sostituisca il blocco esistente `let todos = …` con `export let todos = []`.

   ```js
   export let todos = [];
   ```

   Questo potrebbe sembrare un po' strano all'inizio. Non è così che `export` normalmente funziona nei moduli JavaScript! Questo è il modo in cui Svelte 'estende' JavaScript prendendo una sintassi valida e dando un nuovo scopo. In questo caso Svelte utilizza la parola chiave `export` per contrassegnare una dichiarazione di variabile come una proprietà o prop, il che significa che diventa accessibile ai consumatori del componente.

   Può anche specificare un valore iniziale predefinito per un prop. Questo sarà utilizzato se il consumatore del componente non specifica il prop sul componente — o se il suo valore iniziale è undefined — quando istanzia il componente.

   Quindi con `export let todos = []`, stiamo dicendo a Svelte che il nostro componente `Todos.svelte` accetterà un attributo `todos`, che se omesso verrà inizializzato come un array vuoto.

2. Dia un'occhiata all'app, e vedrà il messaggio "Nulla da fare qui!" Questo perché attualmente non stiamo passando alcun valore da `App.svelte`, quindi sta usando il valore di default.
3. Ora spostiamo i nostri to-dos in `App.svelte` e li passiamo al componente `Todos.svelte` come un prop. Aggiorni `src/App.svelte` come segue:

   ```svelte
   <script>
     import Todos from "./components/Todos.svelte";

     let todos = [
       { id: 1, name: "Create a Svelte starter app", completed: true },
       { id: 2, name: "Create your first component", completed: true },
       { id: 3, name: "Complete the rest of the tutorial", completed: false }
     ];
   </script>

   <Todos todos={todos} />
   ```

4. Quando l'attributo e la variabile hanno lo stesso nome, Svelte le permette di specificare solo la variabile come una scorciatoia handy, quindi possiamo riscrivere la nostra ultima riga in questo modo. Provi questo ora.

   ```svelte
   <Todos {todos} />
   ```

A questo punto i suoi to-dos dovrebbero essere resi proprio come prima, tranne che ora li stiamo passando dal componente `App.svelte`.

## Alternare e rimuovere i to-do

Aggiungiamo un po' di funzionalità per alternare lo stato del compito. Svelte ha la direttiva `on:eventname` per ascoltare gli eventi DOM. Aggiungiamo un gestore all'evento `on:click` dell'input checkbox per alternare il valore completed.

1. Aggiorni l'elemento `<input type="checkbox">` all'interno di `src/components/Todos.svelte` come segue:

   ```svelte
   <input type="checkbox" id="todo-{todo.id}"
     on:click={() => todo.completed = !todo.completed}
     checked={todo.completed}
   />
   ```

2. Dopo aggiungeremo una funzione per rimuovere un to-do dal nostro array `todos`. In fondo alla sezione `<script>` di `Todos.svelte`, aggiunga la funzione `removeTodo()` come segue:

   ```js
   function removeTodo(todo) {
     todos = todos.filter((t) => t.id !== todo.id);
   }
   ```

3. La chiameremo tramite il pulsante _Elimina_. Lo aggiorni con un evento `click`, come segue:

   ```svelte
   <button type="button" class="btn btn__danger"
     on:click={() => removeTodo(todo)}
   >
     Delete <span class="visually-hidden">{todo.name}</span>
   </button>
   ```

   Un errore molto comune con i gestori in Svelte è passare il risultato dell'esecuzione di una funzione come gestore, invece di passare la funzione. Ad esempio, se specifica `on:click={removeTodo(todo)}}`, eseguirà `removeTodo(todo)` e il risultato verrà passato come gestore, cosa che non era nelle nostre intenzioni.

   In questo caso deve specificare `on:click={() => removeTodo(todo)}}` come gestore. Se `removeTodo()` non ricevesse parametri, potrebbe usare `on:event={removeTodo}`, ma non `on:event={removeTodo()}}`. Questa non è una sintassi speciale di Svelte — qui stiamo solo usando le [funzioni freccia](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Functions/Arrow_functions) di JavaScript.

Di nuovo, questo è un buon progresso — a questo punto, possiamo ora eliminare i compiti. Quando viene premuto il pulsante _Elimina_ di un elemento da fare, il to-do rilevante viene rimosso dall'array `todos`, e l'interfaccia utente si aggiorna per non mostrarlo più. Inoltre, ora possiamo spuntare le caselle di controllo, e lo stato di completamento dei relativi to-do verrà ora aggiornato nell'array `todos`.

Tuttavia, l'intestazione "x su y elementi completati" non si sta aggiornando. Continui a leggere per scoprire perché sta accadendo e come possiamo risolverlo.

## To-do reattivi

Come abbiamo già visto, ogni volta che il valore di una variabile di primo livello di un componente viene modificato, Svelte sa come aggiornare l'interfaccia utente. Nella nostra app, il valore dell'array `todos` viene aggiornato direttamente ogni volta che un to-do viene alternato o eliminato, e così Svelte aggiornerà automaticamente il DOM.

Lo stesso non è vero per `totalTodos` e `completedTodos`, tuttavia. Nel codice seguente viene assegnato loro un valore quando il componente viene istanziato e lo script viene eseguito, ma dopo che i loro valori non vengono modificati:

```js
let totalTodos = todos.length;
let completedTodos = todos.filter((todo) => todo.completed).length;
```

Potremmo ricalcolarli dopo aver alternato e rimosso i compiti, ma c'è un modo più semplice per farlo.

Possiamo dire a Svelte che vogliamo che le nostre variabili `totalTodos` e `completedTodos` siano reattive anteponendo loro `$:`. Svelte genererà il codice per aggiornarle automaticamente ogni volta che i dati da cui dipendono vengono modificati.

> [!NOTE]
> Svelte utilizza la sintassi di dichiarazione [JavaScript label statement](/it/docs/Web/JavaScript/Reference/Statements/label) `$:` per contrassegnare affermazioni reattive. Proprio come la parola chiave `export` utilizzata per dichiarare i prop, questo potrebbe sembrare un po' alieno. Questo è un altro esempio in cui Svelte sfrutta una sintassi JavaScript valida e le attribuisce un nuovo scopo — in questo caso significa "ri-eseguire questo codice ogni volta che uno dei valori di riferimento cambia". Una volta abituato, non potrà tornare indietro.

Aggiorni le definizioni delle variabili `totalTodos` e `completedTodos` all'interno di `src/components/Todos.svelte` come segue:

```js
$: totalTodos = todos.length;
$: completedTodos = todos.filter((todo) => todo.completed).length;
```

Se ora controlla la sua app, vedrà che i numeri dell'intestazione vengono aggiornati quando i compiti vengono completati o eliminati. Fantastico!

Dietro le quinte il compilatore Svelte analizzerà e analizzerà il nostro codice per creare un albero delle dipendenze, quindi genererà il codice JavaScript per ri-valutare ogni dichiarazione reattiva ogni volta che una delle loro dipendenze viene aggiornata. La reattività in Svelte è implementata in un modo molto leggero e performante, senza utilizzare listener, setter, getter o alcun altro meccanismo complesso.

## Aggiungere nuovi to-do

Ora passiamo al prossimo compito principale di questo articolo — aggiungiamo alcune funzionalità per aggiungere nuovi to-dos.

1. Per prima cosa creeremo una variabile per contenere il testo del nuovo to-do. Aggiunga questa dichiarazione alla sezione `<script>` del file `Todos.svelte`:

   ```js
   let newTodoName = "";
   ```

2. Ora utilizzeremo questo valore nell'`<input>` per aggiungere nuovi compiti. Per fare ciò dobbiamo collegare la nostra variabile `newTodoName` alla `todo-0` input, in modo che il valore della variabile `newTodoName` rimanga in sincronia con la proprietà `value` dell'input. Potremmo fare qualcosa di simile:

   ```svelte
   <input value={newTodoName} on:keydown={(e) => newTodoName = e.target.value} />
   ```

   Ogni volta che il valore della variabile `newTodoName` cambia, verrà riflesso nell'attributo `value` dell'input, e ogni volta che un tasto viene premuto nell'input, aggiorneremo il contenuto della variabile `newTodoName`.

   Questa è un'implementazione manuale del binding bidirezionale dei dati per una casella di input. Ma non abbiamo bisogno di farlo — Svelte fornisce un modo più semplice per collegare qualsiasi proprietà a una variabile, utilizzando la direttiva [`bind:property`](https://svelte.dev/docs/element-directives#bind-property):

   ```svelte
   <input bind:value={newTodoName} />
   ```

   Quindi, implementiamolo. Aggiorni l'input `todo-0` in questo modo:

   ```svelte
   <input
     bind:value={newTodoName}
     type="text"
     id="todo-0"
     autocomplete="off"
     class="input input__lg" />
   ```

3. Un modo semplice per testare che questo funzioni è aggiungere una dichiarazione reattiva per registrare i contenuti di `newTodoName`. Aggiunga questo snippet alla fine della sezione `<script>`:

   ```js
   $: console.log("newTodoName: ", newTodoName);
   ```

   > [!NOTE]
   > Come avrà notato, le dichiarazioni reattive non sono limitate alle dichiarazioni di variabili. Può inserire _qualsiasi_ dichiarazione JavaScript dopo il segno `$:`.

4. Ora provi a tornare a `localhost:5042`, premere <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>K</kbd> per aprire la console del browser e digitare qualcosa nel campo di input. Dovrebbe vedere i suoi ingressi registrati. A questo punto, può eliminare la dichiarazione `console.log()` reattiva se lo desidera.
5. Successivamente creeremo una funzione per aggiungere il nuovo to-do — `addTodo()` — che aggiungerà un nuovo oggetto `todo` all'array `todos`. Aggiunga questo alla fine del blocco `<script>` all'interno di `src/components/Todos.svelte`:

   ```js
   function addTodo() {
     todos.push({ id: 999, name: newTodoName, completed: false });
     newTodoName = "";
   }
   ```

   > [!NOTE]
   > Per il momento stiamo solo assegnando lo stesso `id` a ogni to-do, ma non si preoccupi, lo risolveremo presto.

6. Ora vogliamo aggiornare il nostro HTML in modo da chiamare `addTodo()` ogni volta che il modulo viene inviato. Aggiorni il tag di apertura di NewTodo form in questo modo:

   ```svelte
   <form on:submit|preventDefault={addTodo}>
   ```

   La direttiva [`on:eventname`](https://svelte.dev/docs/element-directives#on-eventname) supporta l'aggiunta di modificatori all'evento DOM con il carattere `|`. In questo caso, il modificatore `preventDefault` dice a Svelte di generare il codice per chiamare `event.preventDefault()` prima di eseguire il gestore. Esplori il collegamento precedente per vedere quali altri modificatori sono disponibili.

7. Se prova ad aggiungere nuovi to-dos a questo punto, i nuovi to-dos vengono aggiunti all'array di to-dos, ma la nostra interfaccia utente non viene aggiornata. Ricordi che in Svelte [la reattività è attivata con le assegnazioni](https://svelte.dev/docs/svelte-components#script-2-assignments-are-reactive). Ciò significa che la funzione `addTodo()` viene eseguita, l'elemento viene aggiunto all'array `todos`, ma Svelte non rileverà che il metodo push ha modificato l'array, quindi non aggiornerà gli elementi `<ul>`.

   Basta aggiungere `todos = todos` alla fine della funzione `addTodo()` risolverebbe il problema, ma sembra strano doverlo includere alla fine della funzione. Invece, toglieremo il metodo `push()` e useremo la [sintassi spread](/it/docs/Web/JavaScript/Reference/Operators/Spread_syntax) per ottenere lo stesso risultato: assegneremo un valore all'array `todos` pari all'array `todos` più il nuovo oggetto.

   > **Nota:** `Array` ha diverse operazioni mutabili: [`push()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/push), [`pop()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/pop), [`splice()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/splice), [`shift()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/shift), [`unshift()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift), [`reverse()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse) e [`sort()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/sort). Il loro utilizzo spesso causa effetti collaterali e bug difficili da rilevare. Utilizzando la sintassi spread anziché `push()` evitiamo di modificare l'array, il che è considerata una buona pratica.

   Aggiorni la sua funzione `addTodo()` in questo modo:

   ```js
   function addTodo() {
     todos = [...todos, { id: 999, name: newTodoName, completed: false }];
     newTodoName = "";
   }
   ```

## Dare a ogni to-do un ID univoco

Se prova ad aggiungere nuovi to-dos nella sua app ora, sarà in grado di aggiungere un nuovo to-do e farlo apparire nell'interfaccia utente — una volta. Se prova una seconda volta, non funzionerà, e riceverà un messaggio in console che dice "Error: Cannot have duplicate keys in a keyed each". Abbiamo bisogno di ID univoci per i nostri to-dos.

1. Dichiar

i una variabile `newTodoId` calcolata a partire dal numero di to-dos più 1, e la rende reattiva. Aggiunga il seguente snippet alla sezione `<script>`:

   ```js
   let newTodoId;
   $: {
     if (totalTodos === 0) {
       newTodoId = 1;
     } else {
       newTodoId = Math.max(...todos.map((t) => t.id)) + 1;
     }
   }
   ```

   > [!NOTE]
   > Come può vedere, le dichiarazioni reattive non sono limitate a singole righe. Anche il seguente funzionerebbe, ma è un po' meno leggibile: `$: newTodoId = totalTodos ? Math.max(...todos.map((t) => t.id)) + 1 : 1`

2. Come fa Svelte a farlo? Il compilatore analizza l'intera dichiarazione reattiva e rileva che dipende dalla variabile `totalTodos` e dall'array `todos`. Quindi, ogni volta che una di esse viene modificata, questo codice viene ri-valutato, aggiornando `newTodoId` di conseguenza.

   Usiamolo nella nostra funzione `addTodo()`. La aggiorni così:

   ```js
   function addTodo() {
     todos = [...todos, { id: newTodoId, name: newTodoName, completed: false }];
     newTodoName = "";
   }
   ```

## Filtrare i to-do per stato

Infine per questo articolo, implementiamo la capacità di filtrare i nostri to-dos per stato. Creeremo una variabile per contenere il filtro corrente, e una funzione helper che restituirà i to-do filtrati.

1. In fondo alla nostra sezione `<script>` aggiunga quanto segue:

   ```js
   let filter = "all";
   const filterTodos = (filter, todos) =>
     filter === "active"
       ? todos.filter((t) => !t.completed)
       : filter === "completed"
         ? todos.filter((t) => t.completed)
         : todos;
   ```

   Utilizziamo la variabile `filter` per controllare il filtro attivo: _all_, _active_, o _completed_. Assegnare semplicemente uno di questi valori alla variabile filtro attiverà il filtro e aggiornerà l'elenco dei to-do. Vediamo come raggiungere questo obiettivo.

   La funzione `filterTodos()` riceverà il filtro corrente e l'elenco dei to-do e restituirà un nuovo array di to-do filtrati di conseguenza.

2. Aggiorni il markup del pulsante di filtro per renderlo dinamico e aggiornare il filtro corrente quando l'utente preme uno dei pulsanti di filtro. Lo aggiorni così:

   ```svelte
   <div class="filters btn-group stack-exception">
     <button class="btn toggle-btn" class:btn__primary={filter === 'all'} aria-pressed={filter === 'all'} on:click={() => filter = 'all'} >
       <span class="visually-hidden">Show</span>
       <span>All</span>
       <span class="visually-hidden">tasks</span>
     </button>
     <button class="btn toggle-btn" class:btn__primary={filter === 'active'} aria-pressed={filter === 'active'} on:click={() => filter = 'active'} >
       <span class="visually-hidden">Show</span>
       <span>Active</span>
       <span class="visually-hidden">tasks</span>
     </button>
     <button class="btn toggle-btn" class:btn__primary={filter === 'completed'} aria-pressed={filter === 'completed'} on:click={() => filter = 'completed'} >
       <span class="visually-hidden">Show</span>
       <span>Completed</span>
       <span class="visually-hidden">tasks</span>
     </button>
   </div>
   ```

   Ci sono un paio di cose che accadono in questo markup.

   Mostreremo il filtro corrente applicando la classe `btn__primary` al pulsante del filtro attivo. Per applicare condizionalmente classi di stile a un elemento utilizziamo la direttiva `class:name={value}`. Se l'espressione del valore è valutata come vera, il nome della classe verrà applicato. Può aggiungere molte di queste direttive, con condizioni diverse, allo stesso elemento. Quindi, quando emettiamo `class:btn__primary={filter === 'all'}`, Svelte applicherà la classe `btn__primary` se il filtro è uguale a all.

   > [!NOTE]
   > Svelte fornisce una scorciatoia che permette di abbreviare `<div class:active={active}>` a `<div class:active>` quando la classe corrisponde al nome della variabile.

   Qualcosa di simile accade con `aria-pressed={filter === 'all'}`: quando l'espressione JavaScript passata tra parentesi graffe è valutata come vera, l'attributo `aria-pressed` verrà aggiunto al pulsante.

   Ogni volta che clicchiamo su un pulsante, aggiorniamo la variabile filtro emettendo `on:click={() => filter = 'all'}`. Continui a leggere per scoprire come Svelte reattività si prenderà cura del resto.

3. Ora dobbiamo solo utilizzare la funzione helper nel ciclo `{#each}`; la aggiorni così:

   ```svelte
   …
     <ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
     {#each filterTodos(filter, todos) as todo (todo.id)}
   …
   ```

   Dopo aver analizzato il nostro codice, Svelte rileva che la nostra funzione `filterTodos()` dipende dalle variabili `filter` e `todos`. E, proprio come con qualsiasi altra espressione dinamica incorporata nel markup, ogni volta che una di queste dipendenze cambia, il DOM verrà aggiornato di conseguenza. Quindi ogni volta che `filter` o `todos` cambia, la funzione `filterTodos()` verrà ri-valutata e gli elementi all'interno del ciclo verranno aggiornati.

> [!NOTE]
> La reattività può essere complicata a volte. Svelte riconosce `filter` come una dipendenza perché la stiamo citando nell'espressione `filterTodos(filter, todo)`. `filter` è una variabile di primo livello, quindi potremmo essere tentati di rimuoverla dai parametri della funzione helper e chiamarla semplicemente in questo modo: `filterTodos(todo)`. Questo funzionerebbe, ma ora Svelte non ha modo di sapere che `{#each filterTodos(todos) }` dipende da `filter`, e l'elenco dei to-do filtrati non verrà aggiornato quando il filtro cambia. Ricordi sempre che Svelte analizza il nostro codice per trovare le dipendenze, quindi è meglio essere espliciti riguardo ad esse e non fare affidamento sulla visibilità delle variabili di primo livello. Inoltre, è una buona pratica rendere il nostro codice chiaro ed esplicito su quali informazioni stia utilizzando.

## Il codice finora

### Git

Per vedere lo stato del codice come dovrebbe essere alla fine di questo articolo, acceda alla sua copia del nostro repo in questo modo:

```bash
cd mdn-svelte-tutorial/04-componentizing-our-app
```

Oppure scarichi direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/04-componentizing-our-app
```

Ricordi di eseguire `npm install && npm run dev` per avviare la sua app in modalità sviluppo.

### REPL

Per vedere lo stato attuale del codice in un REPL, visiti:

<https://svelte.dev/repl/99b9eb228b404a2f8c8959b22c0a40d3?version=3.23.2>

## Riepilogo

Per ora è tutto! In questo articolo abbiamo già implementato la maggior parte delle funzionalità desiderate. La nostra app può visualizzare, aggiungere ed eliminare to-dos, alternare il loro stato di completamento, mostrare quanti di essi sono completati e applicare filtri.

Per ricapitolare, abbiamo coperto i seguenti argomenti:

- Creare e utilizzare componenti
- Trasformare il markup statico in un modello dinamico
- Incorporare espressioni JavaScript nel nostro markup
- Iterare su elenchi utilizzando la direttiva `{#each}`
- Passare informazioni tra componenti con props
- Ascoltare eventi DOM
- Dichiarare dichiarazioni reattive
- Debugging di base con `console.log()` e dichiarazioni reattive
- Associare proprietà HTML con la direttiva `bind:property`
- Attivare la reattività con le assegnazioni
- Utilizzare espressioni reattive per filtrare i dati
- Definire esplicitamente le nostre dipendenze reattive

Nel prossimo articolo aggiungeremo ulteriori funzionalità, che permetteranno agli utenti di modificare i to-dos.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_Todo_list_beginning","Learn_web_development/Core/Frameworks_libraries/Svelte_components", "Learn_web_development/Core/Frameworks_libraries")}}
