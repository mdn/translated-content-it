---
title: "Comportamento dinamico in Svelte: lavorare con variabili e props"
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_variables_props
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_Todo_list_beginning","Learn_web_development/Core/Frameworks_libraries/Svelte_components", "Learn_web_development/Core/Frameworks_libraries")}}

Ora che abbiamo il nostro markup e gli stili pronti, possiamo iniziare a sviluppare le funzionalità necessarie per la nostra app Svelte per la gestione di una lista di cose da fare. In questo articolo useremo variabili e props per rendere la nostra app dinamica, permettendo di aggiungere e eliminare attività, segnarle come completate e filtrarle per stato.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Come minimo, è consigliato avere familiarità con i linguaggi base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
          e avere conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/command line</a>.
        </p>
        <p>
          Avrai bisogno di un terminale con node e npm installati per compilare e costruire la tua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare e mettere in pratica alcuni concetti base di Svelte, come creare componenti, passare dati usando props, rendere espressioni JavaScript nel nostro markup, modificare lo stato dei componenti e iterare su liste.
      </td>
    </tr>
  </tbody>
</table>

## Segui il codice con noi

### Git

Clona il repository di GitHub (se non l'hai già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi per raggiungere lo stato attuale dell'app, esegui

```bash
cd mdn-svelte-tutorial/03-adding-dynamic-behavior
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/03-adding-dynamic-behavior
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità sviluppo.

### REPL

Per seguire il codice con noi usando il REPL, inizia da

<https://svelte.dev/repl/c862d964d48d473ca63ab91709a0a5a0?version=3.23.2>

## Lavorare con le liste di compiti

Il nostro componente `Todos.svelte` attualmente visualizza solo markup statico; cominciamo a renderlo un po' più dinamico. Prenderemo le informazioni delle attività dal markup e le memorizzeremo in un array `todos`. Creeremo anche due variabili per tenere traccia del numero totale di attività e delle attività completate.

Lo stato del nostro componente sarà rappresentato da queste tre variabili di alto livello.

1. Crea una sezione `<script>` nella parte superiore di `src/components/Todos.svelte` e inserisci del contenuto, come segue:

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

   Ora facciamo qualcosa con queste informazioni.

2. Iniziamo mostrando un messaggio di stato. Trova l'intestazione `<h2>` con un `id` di `list-heading` e sostituisci il numero fisso di attività attive e completate con espressioni dinamiche:

   ```svelte
   <h2 id="list-heading">{completedTodos} out of {totalTodos} items completed</h2>
   ```

3. Vai all'app e dovresti vedere il messaggio "2 su 3 elementi completati" come prima, ma questa volta le informazioni provengono dall'array `todos`.
4. Per provarlo, vai a quell'array e prova a cambiare alcuni dei valori delle proprietà completate degli oggetti "da fare", e anche ad aggiungere un nuovo oggetto "da fare". Osserva come i numeri nel messaggio si aggiornano di conseguenza.

## Generazione dinamica delle attività dall'array

Al momento, gli elementi "da fare" visualizzati sono tutti statici. Vogliamo iterare su ciascun elemento nel nostro array `todos` e rendere il markup per ciascuna attività, quindi facciamolo ora.

HTML non ha un modo di esprimere la logica — come condizionali e loop. Svelte lo fa. In questo caso utilizziamo la direttiva [`{#each}`](https://svelte.dev/docs/logic-blocks#each) per iterare sull'array `todos`. Il secondo parametro, se fornito, conterrà l'indice dell'elemento corrente. Inoltre, può essere fornita un'espressione chiave, che identificherà univocamente ciascun elemento. Svelte la utilizzerà per trovare la differenza nella lista quando i dati cambiano, piuttosto che aggiungere o rimuovere elementi alla fine, e specificarne sempre uno è una buona pratica. Infine, può essere fornito un blocco `:else`, che verrà reso quando la lista è vuota.

Proviamoci.

1. Sostituisci l'elemento `<ul>` esistente con la seguente versione semplificata per avere un'idea di come funziona:

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

2. Torna all'app e vedrai qualcosa di simile:

   ![output di una lista di cose da fare molto semplice creata usando un blocco each](01-each-block.png)

3. Ora che abbiamo visto che funziona, generiamo un elemento di attività completo con ciascun ciclo della direttiva `{#each}` e all'interno incorporiamo le informazioni dall'array `todos`: `id`, `name` e `completed`. Sostituisci il tuo blocco `<ul>` esistente con il seguente:

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

   Nota come stiamo usando parentesi graffe per incorporare espressioni JavaScript negli attributi HTML, come abbiamo fatto con gli attributi `checked` e `id` della casella di controllo.

Abbiamo trasformato il nostro markup statico in un template dinamico pronto a mostrare le attività dallo stato del nostro componente. Ottimo! Ci stiamo arrivando.

## Lavorare con i props

Con una lista di attività codificata, il nostro componente `Todos` non è molto utile. Per trasformare il nostro componente in un editor di attività generico, dovremmo permettere al genitore di questo componente di passare la lista delle attività da modificare. Questo ci permetterebbe di salvarle in un servizio web o nello storage locale e recuperarle successivamente per l'aggiornamento. Quindi trasformiamo l'array in un `prop`.

1. In `Todos.svelte`, sostituisci il blocco esistente `let todos = …` con `export let todos = []`.

   ```js
   export let todos = [];
   ```

   Questo potrebbe sembrare un po' strano all'inizio. Non è così che `export` funziona normalmente nei moduli JavaScript! Questo è il modo in cui Svelte 'estende' il JavaScript prendendo una sintassi valida e attribuendole un nuovo scopo. In questo caso Svelte utilizza la parola chiave `export` per contrassegnare una dichiarazione di variabile come proprietà o prop, il che significa che diventa accessibile ai consumatori del componente.

   Puoi anche specificare un valore iniziale predefinito per un prop. Questo verrà utilizzato se il consumatore del componente non specifica il prop sul componente — o se il suo valore iniziale è indefinito — quando si istanzia il componente.

   Quindi con `export let todos = []`, stiamo dicendo a Svelte che il nostro componente `Todos.svelte` accetterà un attributo `todos`, che se omesso sarà inizializzato a un array vuoto.

2. Dai un'occhiata all'app e vedrai il messaggio "Nothing to do here!" (Niente da fare qui). Questo perché attualmente non stiamo passando alcun valore da `App.svelte`, quindi sta usando il valore predefinito.
3. Ora spostiamo le nostre attività su `App.svelte` e passiamole al componente `Todos.svelte` come prop. Aggiorna `src/App.svelte` come segue:

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

4. Quando l'attributo e la variabile hanno lo stesso nome, Svelte permette di specificare solo la variabile come scorciatoia comoda, quindi possiamo riscrivere la nostra ultima riga in questo modo. Prova ora.

   ```svelte
   <Todos {todos} />
   ```

A questo punto le tue attività dovrebbero essere visualizzate proprio come prima, tranne per il fatto che ora le stiamo passando dal componente `App.svelte`.

## Alternare e rimuovere le attività

Aggiungiamo alcune funzionalità per alternare lo stato delle attività. Svelte ha la direttiva `on:eventname` per ascoltare gli eventi DOM. Aggiungiamo un gestore all'evento `on:click` dell'input checkbox per alternare il valore completato.

1. Aggiorna l'elemento `<input type="checkbox">` dentro `src/components/Todos.svelte` come segue:

   ```svelte
   <input type="checkbox" id="todo-{todo.id}"
     on:click={() => todo.completed = !todo.completed}
     checked={todo.completed}
   />
   ```

2. Successivamente aggiungeremo una funzione per rimuovere un'attività dal nostro array `todos`. In fondo alla sezione `<script>` di `Todos.svelte`, aggiungi la funzione `removeTodo()` in questo modo:

   ```js
   function removeTodo(todo) {
     todos = todos.filter((t) => t.id !== todo.id);
   }
   ```

3. Lo chiameremo tramite il pulsante _Delete_. Aggiornalo con un evento `click`, come segue:

   ```svelte
   <button type="button" class="btn btn__danger"
     on:click={() => removeTodo(todo)}
   >
     Delete <span class="visually-hidden">{todo.name}</span>
   </button>
   ```

   Un errore molto comune con i gestori in Svelte è passare il risultato dell'esecuzione di una funzione come gestore, invece di passare la funzione. Ad esempio, se specifichi `on:click={removeTodo(todo)}}`, eseguirà `removeTodo(todo)` e il risultato verrà passato come gestore, il che non è ciò che avevamo in mente.

   In questo caso devi specificare `on:click={() => removeTodo(todo)}}` come gestore. Se `removeTodo()` non ricevesse parametri, potresti usare `on:event={removeTodo}`, ma non `on:event={removeTodo()}}`. Questa non è una sintassi speciale di Svelte — qui stiamo solo usando le [funzioni arrow](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Functions/Arrow_functions) di JavaScript normale.

Ottimo progresso — a questo punto possiamo ora eliminare le attività. Quando viene premuto il pulsante _Delete_ di un elemento attività, l'attività rilevante viene rimossa dall'array `todos`, e l'interfaccia utente viene aggiornata per non mostrarla più. Inoltre, ora possiamo controllare le caselle e lo stato completato delle attività rilevanti verrà aggiornato nell'array `todos`.

Tuttavia, l'intestazione "x su y elementi completati" non viene aggiornata. Continua a leggere per scoprire perché accade e come possiamo risolverlo.

## Attività reattive

Come abbiamo già visto, ogni volta che il valore di una variabile di componente di alto livello viene modificato, Svelte sa come aggiornare l'interfaccia utente. Nella nostra app, il valore dell'array `todos` viene aggiornato direttamente ogni volta che un'attività viene alternata o eliminata, e quindi Svelte aggiornerà automaticamente il DOM.

Non è lo stesso per `totalTodos` e `completedTodos`, tuttavia. Nel seguente codice vengono assegnati un valore quando il componente viene istanziato e lo script viene eseguito, ma successivamente i loro valori non vengono modificati:

```js
let totalTodos = todos.length;
let completedTodos = todos.filter((todo) => todo.completed).length;
```

Potremmo ricalcolarli dopo aver alternato e rimosso le attività, ma c'è un modo più semplice per farlo.

Possiamo dire a Svelte che vogliamo che le nostre variabili `totalTodos` e `completedTodos` siano reattive anteponendo loro `$:`. Svelte genererà il codice per aggiornarle automaticamente ogni volta che i dati da cui dipendono vengono modificati.

> [!NOTE]
> Svelte utilizza la sintassi [JavaScript label statement](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Statements/label) `$:` per contrassegnare le istruzioni reattive. Proprio come la parola chiave `export` utilizzata per dichiarare i props, questo potrebbe sembrare un po' alieno. Questo è un altro esempio in cui Svelte approfitta di una sintassi JavaScript valida e le attribuisce un nuovo scopo — in questo caso significa "ri-esegui questo codice ogni volta che uno dei valori referenziati cambia". Una volta abituati, non si torna più indietro.

Aggiorna le tue definizioni di variabili `totalTodos` e `completedTodos` all'interno di `src/components/Todos.svelte` per apparire così:

```js
$: totalTodos = todos.length;
$: completedTodos = todos.filter((todo) => todo.completed).length;
```

Se controlli la tua app ora, vedrai che i numeri dell'intestazione vengono aggiornati quando le attività vengono completate o eliminate. Bello!

Dietro le quinte, il compilatore Svelte analizzerà il nostro codice per creare un albero delle dipendenze e quindi genererà il codice JavaScript per valutare di nuovo ciascuna dichiarazione reattiva ogni volta che una delle loro dipendenze viene aggiornata. La reattività in Svelte è implementata in modo molto leggero e performante, senza utilizzare listener, setter, getter o altri meccanismi complessi.

## Aggiungere nuove attività

Passiamo ora al prossimo compito principale per questo articolo — aggiungiamo un po' di funzionalità per aggiungere nuove attività.

1. Innanzitutto creeremo una variabile per contenere il testo della nuova attività. Aggiungi questa dichiarazione alla sezione `<script>` del file `Todos.svelte`:

   ```js
   let newTodoName = "";
   ```

2. Ora utilizzeremo questo valore nell'`<input>` per aggiungere nuove attività. Per farlo dobbiamo collegare la nostra variabile `newTodoName` alla proprietà `value` dell'input `todo-0`, in modo che il valore della variabile `newTodoName` rimanga sincronizzato con la proprietà `value` dell'input. Potremmo fare qualcosa del genere:

   ```svelte
   <input value={newTodoName} on:keydown={(e) => newTodoName = e.target.value} />
   ```

   Ogni volta che il valore della variabile `newTodoName` cambia, sarà riflesso nell'attributo `value` dell'input, e ogni volta che un tasto viene premuto nell'input, aggiorneremo il contenuto della variabile `newTodoName`.

   Questa è un'implementazione manuale del binding bidirezionale dei dati per una casella di input. Ma non dobbiamo farlo — Svelte fornisce un modo più semplice per collegare qualsiasi proprietà a una variabile, usando la direttiva [`bind:property`](https://svelte.dev/docs/element-directives#bind-property):

   ```svelte
   <input bind:value={newTodoName} />
   ```

   Facciamolo. Aggiorna l'input `todo-0` così:

   ```svelte
   <input
     bind:value={newTodoName}
     type="text"
     id="todo-0"
     autocomplete="off"
     class="input input__lg" />
   ```

3. Un modo semplice per testare che questo funzioni è aggiungere un'istruzione reattiva per registrare il contenuto di `newTodoName`. Aggiungi questo snippet alla fine della sezione `<script>`:

   ```js
   $: console.log("newTodoName: ", newTodoName);
   ```

   > [!NOTE]
   > Come avrai notato, le istruzioni reattive non sono limitate alla dichiarazione di variabili. Puoi inserire _qualsiasi_ dichiarazione JavaScript dopo il segno `$:`.

4. Ora prova a tornare a `localhost:5042`, premendo <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>K</kbd> per aprire la console del tuo browser e digitando qualcosa nel campo di input. Dovresti vedere le tue voci registrate. A questo punto, puoi eliminare il `console.log()` reattivo se lo desideri.
5. Successivamente creeremo una funzione per aggiungere la nuova attività — `addTodo()` — che aggiungerà un nuovo oggetto `todo` all'array `todos`. Aggiungilo alla fine del tuo blocco `<script>` dentro `src/components/Todos.svelte`:

   ```js
   function addTodo() {
     todos.push({ id: 999, name: newTodoName, completed: false });
     newTodoName = "";
   }
   ```

   > [!NOTE]
   > Per il momento stiamo solo assegnando lo stesso `id` a ogni attività, ma non preoccuparti, lo sistemeremo presto.

6. Ora vogliamo aggiornare il nostro HTML in modo da chiamare `addTodo()` ogni volta che il form viene inviato. Aggiorna il tag di apertura del form `NewTodo` così:

   ```svelte
   <form on:submit|preventDefault={addTodo}>
   ```

   La direttiva [`on:eventname`](https://svelte.dev/docs/element-directives#on-eventname) supporta l'aggiunta di modificatori all'evento DOM con il carattere `|`. In questo caso, il modificatore `preventDefault` dice a Svelte di generare il codice per chiamare `event.preventDefault()` prima di eseguire il gestore. Esplora il link precedente per vedere quali altri modificatori sono disponibili.

7. Se provi ad aggiungere nuove attività a questo punto, le nuove attività vengono aggiunte all'array delle attività, ma la nostra interfaccia utente non viene aggiornata. Ricorda che in Svelte [la reattività viene attivata con assegnazioni](https://svelte.dev/docs/svelte-components#script-2-assignments-are-reactive). Ciò significa che la funzione `addTodo()` viene eseguita, l'elemento viene aggiunto all'array `todos`, ma Svelte non rileverà che il metodo push ha modificato l'array, quindi non aggiornerà l'`<ul>` delle attività.

   Basta aggiungere `todos = todos` alla fine della funzione `addTodo()` per risolvere il problema, ma sembra strano doverlo includere alla fine della funzione. Invece, toglieremo il metodo `push()` e utilizzeremo la [spread syntax](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Operators/Spread_syntax) per ottenere lo stesso risultato: assegneremo un valore all'array `todos` uguale all'array `todos` più il nuovo oggetto.

   > **Nota:** `Array` ha diverse operazioni mutabili: [`push()`](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Global_Objects/Array/push), [`pop()`](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Global_Objects/Array/pop), [`splice()`](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Global_Objects/Array/splice), [`shift()`](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Global_Objects/Array/shift), [`unshift()`](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift), [`reverse()`](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse), e [`sort()`](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Global_Objects/Array/sort). Usarle spesso causa effetti collaterali e bug difficili da tracciare. Utilizzando la spread syntax invece di `push()` evitiamo di mutare l'array, il che è considerato una buona pratica.

   Aggiorna la tua funzione `addTodo()` così:

   ```js
   function addTodo() {
     todos = [...todos, { id: 999, name: newTodoName, completed: false }];
     newTodoName = "";
   }
   ```

## Dare un ID univoco a ciascuna attività

Se provi ad aggiungere nuove attività alla tua app ora, sarai in grado di aggiungere una nuova attività e farla apparire nell'interfaccia utente — una volta. Se provi una seconda volta, non funzionerà e riceverai un messaggio di console che dice "Error: Cannot have duplicate keys in a keyed each". Abbiamo bisogno di ID univoci per le nostre attività.

1. Dichiariamo una variabile `newTodoId` calcolandola dal numero di attività più 1, e rendiamola reattiva. Aggiungi il seguente snippet alla sezione `<script>`:

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
   > Come puoi vedere, le dichiarazioni reattive non sono limitate a una sola riga. Anche la seguente funzionerebbe, ma è un po' meno leggibile: `$: newTodoId = totalTodos ? Math.max(...todos.map((t) => t.id)) + 1 : 1`

2. Come fa Svelte ad ottenere ciò? Il compilatore analizza l'intera dichiarazione reattiva e rileva che dipende dalla variabile `totalTodos` e dall'array `todos`. Quindi ogni volta che uno di loro viene modificato, questo codice viene valutato di nuovo, aggiornando `newTodoId` di conseguenza.

   Usiamolo nella nostra funzione `addTodo()`. Aggiornala come segue:

   ```js
   function addTodo() {
     todos = [...todos, { id: newTodoId, name: newTodoName, completed: false }];
     newTodoName = "";
   }
   ```

## Filtrare le attività per stato

Infine, per questo articolo, implementiamo la possibilità di filtrare le nostre attività per stato. Creeremo una variabile per conservare il filtro corrente e una funzione di supporto che restituirà le attività filtrate.

1. In fondo alla nostra sezione `<script>`, aggiungi quanto segue:

   ```js
   let filter = "all";
   const filterTodos = (filter, todos) =>
     filter === "active"
       ? todos.filter((t) => !t.completed)
       : filter === "completed"
         ? todos.filter((t) => t.completed)
         : todos;
   ```

   Usiamo la variabile `filter` per controllare il filtro attivo: _all_, _active_, o _completed_. Assegnare uno di questi valori alla variabile filter attiverà il filtro e aggiornerà l'elenco delle attività. Vediamo come ottenere questo risultato.

   La funzione `filterTodos()` riceverà il filtro corrente e l'elenco delle attività, e restituirà un nuovo array di attività filtrate di conseguenza.

2. Aggiorniamo il markup del pulsante di filtro per renderlo dinamico e aggiornare il filtro corrente quando l'utente preme uno dei pulsanti di filtro. Aggiornalo così:

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

   Mostreremo il filtro corrente applicando la classe `btn__primary` al pulsante del filtro attivo. Per applicare condizionalmente le classi di stile a un elemento utilizziamo la direttiva `class:name={value}`. Se l'espressione del valore valuta a vero, il nome della classe verrà applicato. Puoi aggiungere molte di queste direttive, con diverse condizioni, allo stesso elemento. Quindi quando scriviamo `class:btn__primary={filter === 'all'}`, Svelte applicherà la classe `btn__primary` se filter è uguale a all.

   > [!NOTE]
   > Svelte fornisce una scorciatoia che ci permette di abbreviare `<div class:active={active}>` in `<div class:active>` quando la classe coincide con il nome della variabile.

   Qualcosa di simile succede con `aria-pressed={filter === 'all'}`: quando l'espressione JavaScript passata tra parentesi graffe valuta a un valore vero, l'attributo `aria-pressed` verrà aggiunto al pulsante.

   Ogni volta che clicchiamo su un pulsante, aggiorniamo la variabile filter emettendo `on:click={() => filter = 'all'}`. Continua a leggere per scoprire come Svelte reattività si occuperà del resto.

3. Ora ci basta usare la funzione di supporto nel ciclo `{#each}`; aggiornalo così:

   ```svelte
   …
     <ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
     {#each filterTodos(filter, todos) as todo (todo.id)}
   …
   ```

   Dopo aver analizzato il nostro codice, Svelte rileva che la nostra funzione `filterTodos()` dipende dalle variabili `filter` e `todos`. E, proprio come con qualsiasi altra espressione dinamica incorporata nel markup, ogni volta che una di queste dipendenze cambia, il DOM sarà aggiornato di conseguenza. Quindi ogni volta che `filter` o `todos` cambiano, la funzione `filterTodos()` verrà valutata di nuovo e gli elementi all'interno del ciclo verranno aggiornati.

> [!NOTE]
> La reattività può essere a volte complicata. Svelte riconosce `filter` come dipendenza perché la stiamo riferendo nell'espressione `filterTodos(filter, todo)`. `filter` è una variabile di alto livello, quindi potremmo essere tentati di rimuoverla dai parametri della funzione di supporto, e di chiamarla semplicemente così: `filterTodos(todo)`. Questo funzionerebbe, ma ora Svelte non ha modo di scoprire che `{#each filterTodos(todos) }` dipende da `filter`, e l'elenco delle attività filtrate non verrà aggiornato quando il filtro cambia. Ricorda sempre che Svelte analizza il nostro codice per trovare le dipendenze, quindi è meglio essere espliciti a riguardo e non fare affidamento sulla visibilità delle variabili di alto livello. Inoltre, è una buona pratica rendere il nostro codice chiaro ed esplicito su quali informazioni sta usando.

## Il codice finora

### Git

Per vedere lo stato del codice com'è alla fine di questo articolo, accedi alla tua copia del nostro repository in questo modo:

```bash
cd mdn-svelte-tutorial/04-componentizing-our-app
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/04-componentizing-our-app
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità sviluppo.

### REPL

Per vedere lo stato attuale del codice in un REPL, visita:

<https://svelte.dev/repl/99b9eb228b404a2f8c8959b22c0a40d3?version=3.23.2>

## Sommario

Per ora è tutto! In questo articolo abbiamo già implementato la maggior parte delle funzionalità desiderate. La nostra app può visualizzare, aggiungere ed eliminare attività, alternare il loro stato completato, mostrare quante di esse sono completate e applicare filtri.

Per ricapitolare, abbiamo trattato i seguenti argomenti:

- Creare e utilizzare componenti
- Trasformare il markup statico in un template live
- Incorporare espressioni JavaScript nel nostro markup
- Iterare su liste usando la direttiva `{#each}`
- Passare informazioni tra componenti con i props
- Ascoltare eventi DOM
- Dichiarare istruzioni reattive
- Debugging di base con `console.log()` e istruzioni reattive
- Collegare proprietà HTML con la direttiva `bind:property`
- Attivare la reattività con le assegnazioni
- Utilizzare espressioni reattive per filtrare i dati
- Definire esplicitamente le nostre dipendenze reattive

Nel prossimo articolo aggiungeremo ulteriori funzionalità, che permetteranno agli utenti di modificare le attività.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_Todo_list_beginning","Learn_web_development/Core/Frameworks_libraries/Svelte_components", "Learn_web_development/Core/Frameworks_libraries")}}
