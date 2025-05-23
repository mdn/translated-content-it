---
title: "Svelte Avanzato: Reattività, ciclo di vita, accessibilità"
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility
l10n:
  sourceCommit: e6d43da6c6d28a6ac92cdd47882809ffbdf987ce
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_components","Learn_web_development/Core/Frameworks_libraries/Svelte_stores", "Learn_web_development/Core/Frameworks_libraries")}}

Nell'ultimo articolo abbiamo aggiunto più funzionalità alla nostra lista di cose da fare e abbiamo iniziato a organizzare la nostra app in componenti. In questo articolo aggiungeremo le funzionalità finali dell'app e suddivideremo ulteriormente la nostra app in componenti. Impareremo a gestire i problemi di reattività relativi all'aggiornamento di oggetti e array. Per evitare errori comuni, dovremo approfondire il sistema di reattività di Svelte. Osserveremo anche come risolvere alcuni problemi di focus per l'accessibilità e altro ancora.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Al minimo, è consigliato che tu abbia familiarità con i linguaggi
          principali come
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
          e che tu abbia conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminal/riga di comando</a
          >.
        </p>
        <p>
          Avrai bisogno di un terminale con node e npm installati per compilare
          e costruire la tua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare alcune tecniche avanzate di Svelte coinvolgendo la
        risoluzione di problemi di reattività, problemi di accessibilità della
        tastiera relativi al ciclo di vita dei componenti, e altro.
      </td>
    </tr>
  </tbody>
</table>

Ci concentreremo su alcuni problemi di accessibilità che coinvolgono la gestione dei focus. Per farlo, utilizzeremo alcune tecniche per accedere ai nodi DOM ed eseguire metodi come [`focus()`](/it/docs/Web/API/HTMLElement/focus) e [`select()`](/it/docs/Web/API/HTMLInputElement/select). Vedremo anche come dichiarare e ripulire i listener degli eventi sugli elementi DOM.

Dovremo inoltre imparare qualcosina sul ciclo di vita dei componenti per capire quando questi nodi DOM vengono montati e smontati dal DOM e come possiamo accedervi. Impareremo anche la direttiva `action`, che ci permetterà di estendere la funzionalità degli elementi HTML in modo riutilizzabile e dichiarativo.

Infine, impareremo qualcosa in più sui componenti. Finora, abbiamo visto come i componenti possono condividere dati utilizzando le prop e comunicare con i loro genitori utilizzando eventi e il binding bidirezionale dei dati. Ora vedremo come i componenti possono anche esporre metodi e variabili.

I seguenti nuovi componenti saranno sviluppati nel corso di questo articolo:

- `MoreActions`: Visualizza i pulsanti _Check All_ e _Remove Completed_ ed emette gli eventi corrispondenti necessari per gestire la loro funzionalità.
- `NewTodo`: Visualizza il campo `<input>` e il pulsante _Add_ per aggiungere un nuovo to-do.
- `TodosStatus`: Visualizza l'intestazione dello stato "x out of y items completed".

## Programma con noi

### Git

Clona l'archivio GitHub (se non lo hai già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi, per ottenere lo stato attuale dell'app, esegui

```bash
cd mdn-svelte-tutorial/05-advanced-concepts
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/05-advanced-concepts
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità di sviluppo.

### REPL

Per programmare con noi usando il REPL, inizia a

<https://svelte.dev/repl/76cc90c43a37452e8c7f70521f88b698?version=3.23.2>

## Lavorare sul componente MoreActions

Ora affronteremo i pulsanti _Check All_ e _Remove Completed_. Creiamo un componente che sarà responsabile di visualizzare i pulsanti ed emettere gli eventi corrispondenti.

1. Crea un nuovo file, `components/MoreActions.svelte`.
2. Quando viene cliccato il primo pulsante, emetteremo un evento `checkAll` per segnalare che tutti i to-do devono essere controllati/non controllati. Quando viene cliccato il secondo pulsante, emetteremo un evento `removeCompleted` per segnalare che tutti i to-do completati devono essere rimossi. Inserisci il seguente contenuto nel tuo file `MoreActions.svelte`:

   ```svelte
   <script>
     import { createEventDispatcher } from "svelte";

     const dispatch = createEventDispatcher();

     let completed = true;

     const checkAll = () => {
       dispatch("checkAll", completed);
       completed = !completed;
     };

     const removeCompleted = () => dispatch("removeCompleted");
   </script>

   <div class="btn-group">
     <button type="button" class="btn btn__primary" on:click={checkAll}>{completed ? 'Check' : 'Uncheck'} all</button>
     <button type="button" class="btn btn__primary" on:click={removeCompleted}>Remove completed</button>
   </div>
   ```

   Abbiamo anche incluso una variabile `completed` per alternare tra controllare e non controllare tutte le attività.

3. Torniamo in `Todos.svelte`, importeremo il nostro componente `MoreActions` e creeremo due funzioni per gestire gli eventi emessi dal componente `MoreActions`.

   Aggiungi la seguente dichiarazione di importazione sotto quelle esistenti:

   ```js
   import MoreActions from "./MoreActions.svelte";
   ```

4. Poi aggiungi le funzioni descritte alla fine della sezione `<script>`:

   ```js
   const checkAllTodos = (completed) =>
     todos.forEach((t) => (t.completed = completed));

   const removeCompletedTodos = () =>
     (todos = todos.filter((t) => !t.completed));
   ```

5. Ora vai alla fine della sezione di markup di `Todos.svelte` e sostituisci l'elemento `<div class="btn-group">` che abbiamo copiato in `MoreActions.svelte` con una chiamata al componente `MoreActions`, così:

   ```svelte
   <!-- MoreActions -->
   <MoreActions
     on:checkAll={(e) => checkAllTodos(e.detail)}
     on:removeCompleted={removeCompletedTodos}
   />
   ```

6. Bene, torniamo nell'app e proviamo. Troverai che il pulsante _Remove Completed_ funziona bene, ma il pulsante _Check All_/_Uncheck All_ fallisce silenziosamente.

Per scoprire cosa sta succedendo qui, dovremo scavare un po' più a fondo nella reattività di Svelte.

## Problemi di reattività: aggiornamento di oggetti e array

Per vedere cosa sta succedendo possiamo registrare l'array `todos` dalla funzione `checkAllTodos()` nella console.

1. Aggiorna la tua funzione `checkAllTodos()` esistente alla seguente:

   ```js
   const checkAllTodos = (completed) => {
     todos.forEach((t) => (t.completed = completed));
     console.log("todos", todos);
   };
   ```

2. Torna al tuo browser, apri la console degli strumenti per sviluppatori, e fai clic su _Check All_/_Uncheck All_ alcune volte.

Noterai che l'array viene aggiornato correttamente ogni volta che premi il pulsante (le proprietà `completed` degli oggetti `todo` vengono alternate tra `true` e `false`), ma Svelte non ne è a conoscenza. Questo significa anche che, in questo caso, un'istruzione reattiva come `$: console.log('todos', todos)` non sarà molto utile.

Per capire perché ciò accade, dobbiamo comprendere come funziona la reattività in Svelte quando si aggiornano array e oggetti.

Molti framework web utilizzano la tecnica del DOM virtuale per aggiornare la pagina. Fondamentalmente, il DOM virtuale è una copia in memoria del contenuto della pagina web. Il framework aggiorna questa rappresentazione virtuale, che viene quindi sincronizzata con il DOM "reale". Questo è molto più veloce che aggiornare direttamente il DOM e consente al framework di applicare molte tecniche di ottimizzazione.

Questi framework, per impostazione predefinita, eseguono principalmente tutto il nostro JavaScript a ogni cambiamento contro questo DOM virtuale, e applicano metodi diversi per memorizzare in cache calcoli costosi e ottimizzare l'esecuzione. Fanno poco o nessun tentativo di capire cosa sta facendo il nostro codice JavaScript.

Svelte non utilizza una rappresentazione del DOM virtuale. Invece, analizza e analizza il nostro codice, crea un albero delle dipendenze e quindi genera il codice JavaScript richiesto per aggiornare solo le parti del DOM che devono essere aggiornate. Questo approccio di solito genera codice JavaScript ottimale con minimali overhead, ma ha anche le sue limitazioni.

A volte Svelte non riesce a rilevare le modifiche alle variabili monitorate. Ricorda che per dire a Svelte che una variabile è cambiata, devi assegnarle un nuovo valore. Una semplice regola da tenere a mente è che **il nome della variabile aggiornata deve apparire a sinistra dell'assegnazione**.

Ad esempio, nel seguente pezzo di codice:

```js
const foo = obj.foo;
foo.bar = "baz";
```

Svelte non aggiornerà i riferimenti a `obj.foo.bar`, a meno che tu non lo segua con `obj = obj`. Questo perché Svelte non può tracciare i riferimenti agli oggetti, quindi dobbiamo dirle esplicitamente che `obj` è cambiato emettendo un'assegnazione.

> [!NOTE]
> Se `foo` è una variabile di livello superiore, puoi facilmente dire a Svelte di aggiornare `obj` ogni volta che `foo` cambia con la seguente istruzione reattiva: `$: foo, obj = obj`. Con questo stiamo definendo `foo` come dipendenza, e ogni volta che cambia, Svelte eseguirà `obj = obj`.

Nella nostra funzione `checkAllTodos()`, quando eseguiamo:

```js
todos.forEach((t) => (t.completed = completed));
```

Svelte non segnerà `todos` come modificati perché non sa che quando aggiorniamo la nostra variabile `t` all'interno del metodo `forEach()`, stiamo anche modificando l'array `todos`. E ha senso, perché altrimenti Svelte sarebbe a conoscenza del funzionamento interno del metodo `forEach()`, lo stesso sarebbe quindi vero per qualsiasi metodo associato a qualsiasi oggetto o array.

Tuttavia, ci sono diverse tecniche che possiamo applicare per risolvere questo problema, e tutte implicano l'assegnazione di un nuovo valore alla variabile monitorata.

Come abbiamo già visto, potremmo semplicemente dire a Svelte di aggiornare la variabile con un auto-assegnazione, come questa:

```js
const checkAllTodos = (completed) => {
  todos.forEach((t) => (t.completed = completed));
  todos = todos;
};
```

Questo risolverà il problema. Internamente Svelte segnerà `todos` come modificato e rimuoverà l'apparentemente ridondante auto-assegnazione. A parte il fatto che sembra strano, è perfettamente accettabile utilizzare questa tecnica, e a volte è il modo più conciso di farlo.

Potremmo anche accedere all'array `todos` per indice, come questo:

```js
const checkAllTodos = (completed) => {
  todos.forEach((t, i) => (todos[i].completed = completed));
};
```

Le assegnazioni a proprietà di array e oggetti — ad esempio, `obj.foo += 1` o `array[i] = x` — funzionano allo stesso modo delle assegnazioni ai valori stessi. Quando Svelte analizza questo codice, può rilevare che l'array `todos` viene modificato.

Un'altra soluzione è assegnare un nuovo array a `todos` contenente una copia di tutti i to-do con la proprietà `completed` aggiornata di conseguenza, come questo:

```js
const checkAllTodos = (completed) => {
  todos = todos.map((t) => ({ ...t, completed }));
};
```

In questo caso stiamo usando il metodo [`map()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/map), che restituisce un nuovo array con i risultati dell'esecuzione della funzione fornita per ciascun elemento. La funzione restituisce una copia di ogni to-do utilizzando la [spread syntax](/it/docs/Web/JavaScript/Reference/Operators/Spread_syntax) e sovrascrive la proprietà del valore completed di conseguenza. Questa soluzione ha il vantaggio aggiuntivo di restituire un nuovo array con nuovi oggetti, evitando completamente di modificare l'array originale `todos`.

> [!NOTE]
> Svelte ci consente di specificare diverse opzioni che influiscono sul funzionamento del compilatore. L'opzione `<svelte:options immutable={true}/>` dice al compilatore che prometti di non mutare alcun oggetto. Ciò gli consente di essere meno conservativo nel controllare se i valori sono cambiati e di generare codice più semplice e performante. Per maggiori informazioni su `<svelte:options>`, controlla la [documentazione delle opzioni di Svelte](https://svelte.dev/docs/special-elements#svelte-options).

Tutte queste soluzioni implicano un'assegnazione in cui la variabile aggiornata si trova sul lato sinistro dell'equazione. Qualsiasi di queste tecniche consentirà a Svelte di notare che il nostro array `todos` è stato modificato.

**Scegline una e aggiorna la tua funzione `checkAllTodos()` come richiesto. Ora dovresti essere in grado di controllare e deselezionare tutti i tuoi to-do in una volta. Provalo!**

## Completare il nostro componente MoreActions

Aggiungeremo un dettaglio di usabilità al nostro componente. Disabiliteremo i pulsanti quando non ci sono attività da elaborare. Per crearlo, riceveremo l'array `todos` come una prop e imposteremo la proprietà `disabled` di ciascun pulsante di conseguenza.

1. Aggiorna il tuo componente `MoreActions.svelte` in questo modo:

   ```svelte
   <script>
     import { createEventDispatcher } from 'svelte';
     const dispatch = createEventDispatcher();

     export let todos;

     let completed = true;

     const checkAll = () => {
       dispatch('checkAll', completed);
       completed = !completed;
     }

     const removeCompleted = () => dispatch('removeCompleted');

     $: completedTodos = todos.filter((t) => t.completed).length;
   </script>

   <div class="btn-group">
     <button type="button" class="btn btn__primary"
       disabled={todos.length === 0} on:click={checkAll}>{completed ? 'Check' : 'Uncheck'} all</button>
     <button type="button" class="btn btn__primary"
       disabled={completedTodos === 0} on:click={removeCompleted}>Remove completed</button>
   </div>
   ```

   Abbiamo anche dichiarato una variabile reattiva `completedTodos` per abilitare o disabilitare il pulsante _Remove Completed_.

2. Non dimenticare di passare la prop in `MoreActions` dall'interno di `Todos.svelte`, dove il componente è chiamato:

   ```svelte
   <MoreActions {todos}
       on:checkAll={(e) => checkAllTodos(e.detail)}
       on:removeCompleted={removeCompletedTodos}
     />
   ```

## Lavorare con il DOM: concentrarsi sui dettagli

Ora che abbiamo completato tutte le funzionalità richieste dell'app, ci concentreremo su alcune funzionalità di accessibilità che miglioreranno l'usabilità della nostra app per gli utenti che utilizzano solo la tastiera e per gli utenti di screen reader.

Nello stato attuale, la nostra app presenta un paio di problemi di accessibilità della tastiera che coinvolgono la gestione del focus. Diamo un'occhiata a questi problemi.

## Esplorare i problemi di accessibilità della tastiera nella nostra app di to-do

In questo momento, gli utenti che utilizzano la tastiera scopriranno che il flusso di messa a fuoco della nostra app non è molto prevedibile o coerente.

Se fai clic sull'input in alto alla nostra app, vedrai un bordo tratteggiato spesso attorno a quell'input. Questo contorno è il tuo indicatore visivo che il browser è attualmente focalizzato su questo elemento.

Se sei un utente del mouse, potresti aver saltato questo suggerimento visivo. Ma se lavori esclusivamente con la tastiera, sapere quale controllo ha il focus è di vitale importanza. Ci dice quale controllo riceverà le nostre sequenze di tasti.

Se premi ripetutamente il tasto <kbd>Tab</kbd>, vedrai l'indicatore di messa a fuoco tratteggiato che cicla tra tutti gli elementi focalizzabili della pagina. Se sposti il focus sul pulsante _Edit_ e premi <kbd>Enter</kbd>, improvvisamente il focus scompare e non puoi più sapere quale controllo riceverà le nostre sequenze di tasti.

Inoltre, se premi il tasto <kbd>Escape</kbd> o <kbd>Enter</kbd>, non succede nulla. E se fai clic su _Cancel_ o _Save_, il focus scompare di nuovo. Per un utente che lavora con la tastiera, questo comportamento sarà al massimo confuso.

Vorremmo anche aggiungere alcune funzionalità di usabilità, come disabilitare il pulsante _Save_ quando i campi richiesti sono vuoti, dare focus a determinati elementi HTML o selezionare automaticamente i contenuti quando un input di testo riceve il focus.

Per implementare tutte queste funzionalità, avremo bisogno di accesso programmatico ai nodi DOM per eseguire funzioni come [`focus()`](/it/docs/Web/API/HTMLElement/focus) e [`select()`](/it/docs/Web/API/HTMLInputElement/select). Dovremo anche utilizzare [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) e [`removeEventListener()`](/it/docs/Web/API/EventTarget/removeEventListener) per eseguire attività specifiche quando il controllo riceve il focus.

Il problema è che tutti questi nodi DOM vengono creati dinamicamente da Svelte in fase di esecuzione. Quindi dovremo aspettare che vengano creati e aggiunti al DOM per poterli usare. Per farlo, dovremo imparare sul [ciclo di vita del componente](https://learn.svelte.dev/tutorial/onmount) per capire quando possiamo accedervi — di più su questo più tardi.

## Creare un componente NewTodo

Iniziamo estraendo il nostro modulo di nuovo to-do in un proprio componente. Con quello che sappiamo finora possiamo creare un nuovo file componente e aggiustare il codice per emettere un evento `addTodo`, passando il nome del nuovo to-do con i dettagli aggiuntivi.

1. Crea un nuovo file, `components/NewTodo.svelte`.
2. Inserisci il seguente contenuto all'interno:

   ```svelte
   <script>
     import { createEventDispatcher } from 'svelte';
     const dispatch = createEventDispatcher();

     let name = '';

     const addTodo = () => {
       dispatch('addTodo', name);
       name = '';
     }

     const onCancel = () => name = '';

   </script>

   <form on:submit|preventDefault={addTodo} on:keydown={(e) => e.key === 'Escape' && onCancel()}>
     <h2 class="label-wrapper">
       <label for="todo-0" class="label__lg">What needs to be done?</label>
     </h2>
     <input bind:value={name} type="text" id="todo-0" autoComplete="off" class="input input__lg" />
     <button type="submit" disabled={!name} class="btn btn__primary btn__lg">Add</button>
   </form>
   ```

   Qui stiamo collegando il `<input>` alla variabile `name` con `bind:value={name}` e disabilitando il pulsante _Add_ quando è vuoto (ossia, senza contenuto di testo) usando `disabled={!name}`. Stiamo anche gestendo il tasto <kbd>Escape</kbd> con `on:keydown={(e) => e.key === 'Escape' && onCancel()}}`. Ogni volta che viene premuto il tasto <kbd>Escape</kbd> eseguiamo `onCancel()`, che cancella semplicemente la variabile `name`.

3. Ora dobbiamo `importare` e usare dall'interno del componente `Todos`, e aggiornare la funzione `addTodo()` per ricevere il nome del nuovo todo.

   Aggiungi la seguente dichiarazione di importazione sotto le altre all'interno di `Todos.svelte`:

   ```js
   import NewTodo from "./NewTodo.svelte";
   ```

4. E aggiorna la funzione `addTodo()` in questo modo:

   ```js
   function addTodo(name) {
     todos = [...todos, { id: newTodoId, name, completed: false }];
   }
   ```

   `addTodo()` ora riceve il nome del nuovo to-do direttamente, quindi non abbiamo più bisogno della variabile `newTodoName` per fornirgli il suo valore. Il nostro componente `NewTodo` si prende cura di questo.

   > [!NOTE]
   > La sintassi `{ name }` è solo una scorciatoia per `{ name: name }`. Questa viene da JavaScript stesso e non ha nulla a che fare con Svelte, oltre a fornire qualche ispirazione per le scorciatoie proprie di Svelte.

5. Infine per questa sezione, sostituisci il markup del modulo NewTodo con una chiamata al componente `NewTodo`, così:

   ```svelte
   <!-- NewTodo -->
   <NewTodo on:addTodo={(e) => addTodo(e.detail)} />
   ```

## Lavorare con i nodi del DOM usando la direttiva `bind:this={dom_node}`

Ora vogliamo che l'elemento `<input>` del componente `NewTodo` recuperi il focus ogni volta che viene premuto il pulsante _Add_. Per questo avremo bisogno di un riferimento al nodo DOM dell'input. Svelte fornisce un modo per farlo con la direttiva `bind:this={dom_node}`. Quando specificato, non appena il componente è montato e il nodo DOM viene creato, Svelte assegna un riferimento al nodo DOM alla variabile specificata.

Creeremo una variabile `nameEl` e la collegheremo all'input usando `bind:this={nameEl}`. Poi all'interno di `addTodo()`, dopo aver aggiunto il nuovo to-do chiameremo `nameEl.focus()` per rifocalizzare di nuovo l'`<input>`. Faremo lo stesso quando l'utente preme il tasto <kbd>Escape</kbd>, con la funzione `onCancel()`.

Aggiorna i contenuti di `NewTodo.svelte` in questo modo:

```svelte
<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  let name = '';
  let nameEl; // reference to the name input DOM node

  const addTodo = () => {
    dispatch('addTodo', name);
    name = '';
    nameEl.focus(); // give focus to the name input
  }

  const onCancel = () => {
    name = '';
    nameEl.focus(); // give focus to the name input
  }
</script>

<form on:submit|preventDefault={addTodo} on:keydown={(e) => e.key === 'Escape' && onCancel()}>
  <h2 class="label-wrapper">
    <label for="todo-0" class="label__lg">What needs to be done?</label>
  </h2>
  <input bind:value={name} bind:this={nameEl} type="text" id="todo-0" autoComplete="off" class="input input__lg" />
  <button type="submit" disabled={!name} class="btn btn__primary btn__lg">Add</button>
</form>
```

Prova l'app: digita un nuovo nome di to-do nel campo `<input>`, premi <kbd>Tab</kbd> per concentrare il focus sul pulsante _Add_ e poi premi <kbd>Enter</kbd> o <kbd>Escape</kbd> per vedere come l'input recupera il focus.

### Auto-focalizzare il nostro input

La successiva funzionalità che aggiungeremo al nostro componente `NewTodo` sarà una prop `autofocus`, che ci permetterà di specificare che vogliamo che il campo `<input>` sia focalizzato al caricamento della pagina.

1. Il nostro primo tentativo è il seguente: proviamo ad aggiungere la prop `autofocus` e semplicemente chiamiamo `nameEl.focus()` dal blocco `<script>`. Aggiorna la prima parte della sezione `<script>` di `NewTodo.svelte` (le prime quattro righe) per apparire così:

   ```svelte
   <script>
     import { createEventDispatcher } from 'svelte';
     const dispatch = createEventDispatcher();

     export let autofocus = false;

     let name = '';
     let nameEl; // reference to the name input DOM node

     if (autofocus) nameEl.focus();
   ```

2. Ora torna al componente `Todos` e passa la prop `autofocus` nella chiamata al componente `<NewTodo>`, così:

   ```svelte
   <!-- NewTodo -->
   <NewTodo autofocus on:addTodo={(e) => addTodo(e.detail)} />
   ```

3. Se provi la tua app ora, vedrai che la pagina è ora vuota, e nella console web dei tuoi DevTools vedrai un errore simile a: `TypeError: nameEl is undefined`.

Per capire cosa sta succedendo, parliamo un po' di più di quel [ciclo di vita del componente](https://learn.svelte.dev/tutorial/onmount) che abbiamo menzionato prima.

## Ciclo di vita del componente, e la funzione `onMount()`

Quando un componente viene istanziato, Svelte esegue il codice di inizializzazione (cioè la sezione `<script>` del componente). Ma in quel momento, tutti i nodi che compongono il componente non sono ancora attaccati al DOM, infatti, non esistono nemmeno.

Quindi come puoi sapere quando il componente è stato già creato e montato nel DOM? La risposta è che ogni componente ha un ciclo di vita che inizia quando viene creato e termina quando viene distrutto. Ci sono un numero limitato di funzioni che ti consentono di eseguire il codice in momenti chiave durante quel ciclo di vita.

Quella che userai più frequentemente è `onMount()`, che ci permette di eseguire un callback non appena il componente è stato montato nel DOM. Proviamola e vediamo cosa succede alla variabile `nameEl`.

1. Per iniziare, aggiungi la seguente riga all'inizio della sezione `<script>` di `NewTodo.svelte`:

   ```js
   import { onMount } from "svelte";
   ```

2. E queste righe alla fine di esso:

   ```js
   console.log("initializing:", nameEl);
   onMount(() => {
     console.log("mounted:", nameEl);
   });
   ```

3. Ora rimuovi la riga `if (autofocus) nameEl.focus()` per evitare di lanciare l'errore che stavamo vedendo prima.
4. L'app ora funzionerà di nuovo e vedrai il seguente nella tua console:

   ```plain
   initializing: undefined
   mounted: <input id="todo-0" class="input input__lg" type="text" autocomplete="off">
   ```

   Come puoi vedere, mentre il componente è in fase di inizializzazione, `nameEl` è undefined, il che ha senso perché il nodo `<input>` non esiste nemmeno ancora. Dopo che il componente è stato montato, Svelte assegna un riferimento al nodo DOM `<input>` alla variabile `nameEl`, grazie alla direttiva `bind:this={nameEl}`.

5. Per far funzionare la funzionalità di autofocus, sostituisci il precedente blocco `console.log()`/`onMount()` che hai aggiunto con questo:

   ```js
   onMount(() => autofocus && nameEl.focus()); // if autofocus is true, we run nameEl.focus()
   ```

6. Vai di nuovo alla tua app, e ora vedrai che il campo `<input>` è focalizzato al caricamento della pagina.

> [!NOTE]
> Puoi dare un'occhiata alle altre [funzioni del ciclo di vita nei documenti Svelte](https://svelte.dev/docs/svelte) e puoi vederle in azione nel [tutorial interattivo](https://learn.svelte.dev/tutorial/onmount).

## Aspettare che il DOM venga aggiornato con la funzione `tick()`

Ora ci occuperemo dei dettagli di gestione del focus del componente `Todo`. Innanzitutto, vogliamo che l'input di modifica del `Todo` riceva il focus quando entriamo in modalità di modifica premendo il suo pulsante _Edit_. Nello stesso modo che abbiamo visto prima, creeremo una variabile `nameEl` all'interno di `Todo.svelte` e chiameremo `nameEl.focus()` dopo aver impostato la variabile `editing` su `true`.

1. Apri il file `components/Todo.svelte` e aggiungi una dichiarazione di variabile `nameEl` appena sotto le tue dichiarazioni editing e name:

   ```js
   let nameEl; // reference to the name input DOM node
   ```

2. Ora aggiorna la tua funzione `onEdit()` in questo modo:

   ```js
   function onEdit() {
     editing = true; // enter editing mode
     nameEl.focus(); // set focus to name input
   }
   ```

3. E infine, collega `nameEl` al campo `<input>` aggiornandolo in questo modo:

   ```svelte
   <input
     bind:value={name}
     bind:this={nameEl}
     type="text"
     id="todo-{todo.id}"
     autocomplete="off"
     class="todo-text" />
   ```

4. Tuttavia, quando provi l'app aggiornata, otterrai un errore simile a "TypeError: nameEl is undefined" nella console quando premi il pulsante _Edit_ di un to-do.

Quindi, cosa sta succedendo qui? Quando aggiorni lo stato di un componente in Svelte, esso non aggiorna immediatamente il DOM. Invece, aspetta fino al successivo microtask per vedere se ci sono altri cambiamenti che devono essere applicati, inclusi in altri componenti. Facendolo evita lavori non necessari e consente al browser di raggruppare più efficacemente le operazioni.

In questo caso, quando `editing` è `false`, l'input di modifica `<input>` non è visibile perché non esiste nel DOM. All'interno della funzione `onEdit()` impostiamo `editing = true` e immediatamente dopo proviamo ad accedere alla variabile `nameEl` ed eseguire `nameEl.focus()`. Il problema qui è che Svelte non ha ancora aggiornato il DOM.

Un modo per risolvere questo problema è usare [`setTimeout()`](/it/docs/Web/API/Window/setTimeout) per ritardare la chiamata a `nameEl.focus()` fino al prossimo ciclo di eventi e dare a Svelte l'opportunità di aggiornare il DOM.

Prova questo ora:

```js
function onEdit() {
  editing = true; // enter editing mode
  setTimeout(() => nameEl.focus(), 0); // asynchronous call to set focus to name input
}
```

La soluzione sopra funziona, ma è piuttosto inelegante. Svelte fornisce un modo migliore per gestire questi casi. La [funzione `tick()`](https://learn.svelte.dev/tutorial/tick) restituisce una promessa che si risolve non appena le eventuali modifiche allo stato pendenti sono state applicate al DOM (o immediatamente, se non ci sono modifiche allo stato in sospeso). Proviamola ora.

1. Prima di tutto, importa `tick` all'inizio della sezione `<script>` insieme alla tua importazione esistente:

   ```js
   import { tick } from "svelte";
   ```

2. Successivamente, chiama `tick()` con [`await`](/it/docs/Web/JavaScript/Reference/Operators/await) da una [funzione asincrona](/it/docs/Web/JavaScript/Reference/Statements/async_function); aggiorna `onEdit()` in questo modo:

   ```js
   async function onEdit() {
     editing = true; // enter editing mode
     await tick();
     nameEl.focus();
   }
   ```

3. Se lo provi ora vedrai che tutto funziona come previsto.

> [!NOTE]
> Per vedere un altro esempio utilizzando `tick()`, visita il [tutorial Svelte](https://learn.svelte.dev/tutorial/tick).

## Aggiungere funzionalità agli elementi HTML con la direttiva `use:action`

Successivamente, vogliamo che l'input `<input>` selezioni automaticamente tutto il testo al focus. Inoltre, vogliamo sviluppare questo in modo che possa essere facilmente riutilizzato su qualsiasi `<input>` HTML e applicato in modo dichiarativo. Utilizzeremo questa esigenza come scusa per mostrare una funzionalità molto potente che Svelte ci fornisce per aggiungere funzionalità agli elementi HTML regolari: [azioni](https://svelte.dev/docs/svelte-action).

Per selezionare il testo di un nodo di input DOM, dobbiamo chiamare [`select()`](/it/docs/Web/API/HTMLInputElement/select). Per ottenere che questa funzione venga chiamata ogni volta che il nodo viene messo a fuoco, abbiamo bisogno di un listener di eventi simile a questo:

```js
node.addEventListener("focus", (event) => node.select());
```

E, per evitare perdite di memoria, dovremmo anche chiamare la funzione [`removeEventListener()`](/it/docs/Web/API/EventTarget/removeEventListener) quando il nodo viene distrutto.

> [!NOTE]
> Tutto questo è solo funzionalità standard WebAPI; qui non c'è nulla di specifico per Svelte.

Potremmo ottenere tutto questo nel nostro componente `Todo` ogni volta che aggiungiamo o rimuoviamo l'`<input>` dal DOM, ma dovremmo essere molto accurati per aggiungere il listener agli eventi dopo che il nodo è stato aggiunto al DOM, e rimuovere il listener prima che il nodo venga rimosso dal DOM. Inoltre, la nostra soluzione non sarebbe molto riutilizzabile.

Ecco dove entrano in gioco le azioni di Svelte. Fondamentalmente ci permettono di eseguire una funzione ogni volta che un elemento è stato aggiunto al DOM, e dopo la rimozione dal DOM.

Nel nostro caso d'uso immediato, definiremo una funzione chiamata `selectOnFocus()` che riceverà un nodo come parametro. La funzione aggiungerà un listener di eventi a quel nodo in modo che ogni volta che riceve il focus, selezioni il testo. Quindi restituirà un oggetto con una proprietà `destroy`. La proprietà `destroy` è ciò che Svelte eseguirà dopo aver rimosso il nodo dal DOM. Qui rimuoveremo il listener per assicurarci di non lasciare dietro di noi nessuna perdita di memoria.

1. Creiamo la funzione `selectOnFocus()`. Aggiungi il seguente codice alla fine della sezione `<script>` di `Todo.svelte`:

   ```js
   function selectOnFocus(node) {
     if (node && typeof node.select === "function") {
       // make sure node is defined and has a select() method
       const onFocus = (event) => node.select(); // event handler
       node.addEventListener("focus", onFocus); // when node gets focus call onFocus()
       return {
         destroy: () => node.removeEventListener("focus", onFocus), // this will be executed when the node is removed from the DOM
       };
     }
   }
   ```

2. Ora dobbiamo dire all'elemento `<input>` di usare quella funzione con la direttiva [`use:action`](https://svelte.dev/docs/element-directives#use-action):

   ```svelte
   <input use:selectOnFocus />
   ```

   Con questa direttiva stiamo dicendo a Svelte di eseguire questa funzione, passando il nodo DOM del `<input>` come parametro, non appena il componente è montato nel DOM. Sarà anche incaricata di eseguire la funzione `destroy` quando il componente viene rimosso dal DOM. Quindi con la direttiva `use`, Svelte si occupa del ciclo di vita del componente per noi.

   Nel nostro caso, il nostro `<input>` finirebbe per essere così: aggiorna la prima coppia label/input del componente (all'interno del template di modifica) come segue:

   ```svelte
   <label for="todo-{todo.id}" class="todo-label">New name for '{todo.name}'</label>
   <input
     bind:value={name}
     bind:this={nameEl}
     use:selectOnFocus
     type="text"
     id="todo-{todo.id}"
     autocomplete="off"
     class="todo-text" />
   ```

3. Proviamolo. Vai alla tua app, premi il pulsante _Edit_ di un to-do e poi <kbd>Tab</kbd> per spostare il focus via dall'`<input>`. Ora fai clic sull' `<input>`, e vedrai che l'intero testo di input è selezionato.

### Rendere l'azione riutilizzabile

Ora rendiamo questa funzione veramente riutilizzabile tra i componenti. `selectOnFocus()` è solo una funzione senza alcuna dipendenza dal componente `Todo.svelte`, quindi possiamo semplicemente estrarla in un file e utilizzarla da lì.

1. Crea un nuovo file, `actions.js`, nella cartella `src`.
2. Dagli i seguenti contenuti:

   ```js
   export function selectOnFocus(node) {
     if (node && typeof node.select === "function") {
       // make sure node is defined and has a select() method
       const onFocus = (event) => node.select(); // event handler
       node.addEventListener("focus", onFocus); // when node gets focus call onFocus()
       return {
         destroy: () => node.removeEventListener("focus", onFocus), // this will be executed when the node is removed from the DOM
       };
     }
   }
   ```

3. Ora importalo dall'interno di `Todo.svelte`; aggiungi la seguente dichiarazione di importazione appena sotto le altre:

   ```js
   import { selectOnFocus } from "../actions.js";
   ```

4. E rimuovi la definizione di `selectOnFocus()` da `Todo.svelte`, poiché non ne abbiamo più bisogno lì.

### Riutilizzare la nostra azione

Per dimostrare la riusabilità della nostra azione, utilizziamola in `NewTodo.svelte`.

1. Importa `selectOnFocus()` da `actions.js` in questo file, come prima:

   ```js
   import { selectOnFocus } from "../actions.js";
   ```

2. Aggiungi la direttiva `use:selectOnFocus` all'`<input>`, in questo modo:

   ```svelte
   <input
     bind:value={name}
     bind:this={nameEl}
     use:selectOnFocus
     type="text"
     id="todo-0"
     autocomplete="off"
     class="input input__lg" />
   ```

Con poche righe di codice possiamo aggiungere funzionalità agli elementi HTML regolari in un modo molto riutilizzabile e dichiarativo. Basta un `import` e una breve direttiva come `use:selectOnFocus` che ne descrive chiaramente lo scopo. E possiamo ottenere questo senza il bisogno di creare un elemento wrapper personalizzato come `TextInput`, `MyInput`, o simile. Inoltre, puoi aggiungere tutte le direttive `use:action` che vuoi ad un elemento.

Inoltre, non abbiamo dovuto lottare con `onMount()`, `onDestroy()`, o `tick()` — la direttiva `use` si occupa del ciclo di vita del componente per noi.

### Altri miglioramenti delle azioni

Nella sezione precedente, mentre lavoravamo con i componenti `Todo`, abbiamo dovuto gestire `bind:this`, `tick()`, e funzioni `async` solo per dare focus al nostro `<input>` appena aggiunto al DOM.

1. Ecco come possiamo implementarlo con le azioni invece:

   ```js
   const focusOnInit = (node) =>
     node && typeof node.focus === "function" && node.focus();
   ```

2. E poi nel nostro markup, abbiamo solo bisogno di aggiungere un'altra direttiva `use:`:

   ```svelte
   <input bind:value={name} use:selectOnFocus use:focusOnInit />
   ```

3. La nostra funzione `onEdit()` può ora essere molto più semplice:

   ```js
   function onEdit() {
     editing = true; // enter editing mode
   }
   ```

Come ultimo esempio prima di andare avanti, torniamo al nostro componente `Todo.svelte` e diamo focus al pulsante _Edit_ dopo che l'utente ha premuto _Save_ o _Cancel_.

Potremmo provare a riutilizzare semplicemente la nostra azione `focusOnInit` di nuovo, aggiungendo `use:focusOnInit` al pulsante _Edit_. Ma staremmo introducendo un bug sottile. Quando aggiungi un nuovo to-do, il focus sarà puntato sul pulsante _Edit_ del to-do appena aggiunto. Questo perché l'azione `focusOnInit` è in esecuzione quando il componente è creato.

Quello non è ciò che vogliamo — vogliamo che il pulsante _Edit_ riceva il focus solo quando l'utente ha premuto _Save_ o _Cancel_.

1. Quindi, torna al tuo file `Todo.svelte`.
2. Per prima cosa, creeremo un flag chiamato `editButtonPressed` e lo inizializzeremo su `false`. Aggiungi questo subito sotto le tue altre definizioni di variabili:

   ```js
   let editButtonPressed = false; // track if edit button has been pressed, to give focus to it after cancel or save
   ```

3. Successivamente, modificheremo la funzionalità del pulsante _Edit_ per salvare questo flag e creare l'azione per esso. Aggiorna la funzione `onEdit()` come segue:

   ```js
   function onEdit() {
     editButtonPressed = true; // user pressed the Edit button, focus will come back to the Edit button
     editing = true; // enter editing mode
   }
   ```

4. Sotto di essa, aggiungi la seguente definizione per `focusEditButton()`:

   ```js
   const focusEditButton = (node) => editButtonPressed && node.focus();
   ```

5. Infine, usiamo l'azione `focusEditButton` sul pulsante _Edit_, così:

   ```svelte
   <button type="button" class="btn" on:click={onEdit} use:focusEditButton>
     Edit<span class="visually-hidden"> {todo.name}</span>
   </button>
   ```

6. Torna e prova la tua app di nuovo. A questo punto, ogni volta che il pulsante _Edit_ viene aggiunto al DOM, l'azione `focusEditButton` viene eseguita, ma darà focus al pulsante solo se il flag `editButtonPressed` è `true`.

> [!NOTE]
> Abbiamo appena graffiato la superficie delle azioni qui. Le azioni possono anche avere parametri reattivi, e Svelte ci consente di rilevare quando uno di questi cambia. Quindi possiamo aggiungere funzionalità che si integrano bene con il sistema reattivo di Svelte. Per un'introduzione più dettagliata alle azioni, considera di controllare il [tutorial interattivo di Svelte](https://learn.svelte.dev/tutorial/actions) o la [documentazione Svelte `use:action`](https://svelte.dev/docs/element-directives#use-action).

## Binding del componente: esporre i metodi e le variabili del componente utilizzando la direttiva `bind:this={component}`

C'è ancora una fastidiosità di accessibilità rimasta. Quando l'utente preme il pulsante _Delete_, il focus scompare.

L'ultima funzione che esamineremo in questo articolo riguarda l'impostazione del focus sull'intestazione dello stato dopo che un to-do è stato eliminato.

Perché l'intestazione dello stato? In questo caso, l'elemento che aveva il focus è stato eliminato, quindi non c'è un chiaro candidato a ricevere il focus. Abbiamo scelto l'intestazione dello stato perché è vicino alla lista dei to-do, ed è un modo per dare un feedback visivo sulla rimozione del task, oltre a indicare cosa è successo agli utenti di screen reader.

Per prima cosa estrarremo l'intestazione dello stato in un proprio componente.

1. Crea un nuovo file, `components/TodosStatus.svelte`.
2. Aggiungi i seguenti contenuti ad esso:

   ```svelte
   <script>
     export let todos;

     $: totalTodos = todos.length;
     $: completedTodos = todos.filter((todo) => todo.completed).length;
   </script>

   <h2 id="list-heading">
     {completedTodos} out of {totalTodos} items completed
   </h2>
   ```

3. Importa il file all'inizio di `Todos.svelte`, aggiungendo la seguente dichiarazione di importazione sotto le altre:

   ```js
   import TodosStatus from "./TodosStatus.svelte";
   ```

4. Sostituisci l'intestazione `<h2>` dello stato all'interno di `Todos.svelte` con una chiamata al componente `TodosStatus`, passando `todos` come prop, così:

   ```svelte
   <TodosStatus {todos} />
   ```

5. Puoi anche fare un po' di pulizia, rimuovendo le variabili `totalTodos` e `completedTodos` da `Todos.svelte`. Rimuovi semplicemente le righe `$: totalTodos = …` e `$: completedTodos = …`, e rimuovi anche il riferimento a `totalTodos` quando calcoliamo `newTodoId` e usa invece `todos.length`. Per farlo, sostituisci il blocco che inizia con `let newTodoId` con questo:

   ```js
   $: newTodoId = todos.length ? Math.max(...todos.map((t) => t.id)) + 1 : 1;
   ```

6. Tutto funziona come previsto — abbiamo appena estratto l'ultimo pezzo di markup in un proprio componente.

Ora dobbiamo trovare un modo per dare focus all'etichetta `<h2>` dello stato dopo che un to-do è stato rimosso.

Finora abbiamo visto come inviare informazioni a un componente tramite le prop e come un componente può comunicare con il suo genitore emettendo eventi o utilizzando il binding bidirezionale dei dati. Il componente figlio potrebbe ottenere un riferimento al nodo `<h2>` utilizzando `bind:this={dom_node}` ed esporlo all'esterno utilizzando il binding bidirezionale dei dati. Ma farlo romperebbe l'incapsulamento del componente; impostare il focus su di esso dovrebbe essere una sua responsabilità.

Quindi abbiamo bisogno che il componente `TodosStatus` esponga un metodo che il suo genitore può chiamare per dare focus su di esso. È uno scenario molto comune che un componente debba esporre qualche comportamento o informazione al consumatore; vediamo come realizzarlo con Svelte.

Abbiamo già visto che Svelte utilizza `export let varname = …` per [dichiarare le prop](https://svelte.dev/docs/svelte-components#script-1-export-creates-a-component-prop). Ma se invece di utilizzare `let` si esporta un `const`, `class`, o `function`, esso è di sola lettura al di fuori del componente. Le espressioni di funzione sono prop valide, comunque. Nel seguente esempio, le prime tre dichiarazioni sono prop, e il resto sono valori esportati:

```svelte
<script>
  export let bar = "optional default initial value"; // prop
  export let baz = undefined; // prop
  export let format = (n) => n.toFixed(2); // prop

  // these are readonly
  export const thisIs = "readonly"; // read-only export

  export function greet(name) {
    // read-only export
    alert(`Hello, ${name}!`);
  }

  export const greet = (name) => alert(`Hello, ${name}!`); // read-only export
</script>
```

Con questo in mente, torniamo al nostro caso d'uso. Creeremo una funzione chiamata `focus()` che dà focus all'intestazione `<h2>`. Per farlo avremo bisogno di una variabile `headingEl` per contenere il riferimento al nodo DOM, e dovremo collegarla all'elemento `<h2>` usando `bind:this={headingEl}`. Il nostro metodo focus eseguirà semplicemente `headingEl.focus()`.

1. Aggiorna i contenuti di `TodosStatus.svelte` in questo modo:

   ```svelte
   <script>
     export let todos;

     $: totalTodos = todos.length;
     $: completedTodos = todos.filter((todo) => todo.completed).length;

     let headingEl;

     export function focus() {
       // shorter version: export const focus = () => headingEl.focus()
       headingEl.focus();
     }
   </script>

   <h2 id="list-heading" bind:this={headingEl} tabindex="-1">
     {completedTodos} out of {totalTodos} items completed
   </h2>
   ```

   Nota che abbiamo aggiunto un attributo `tabindex` sull'`<h2>` per consentire all'elemento di ricevere focus programmaticamente.

   Come abbiamo visto prima, usare la direttiva `bind:this={headingEl}` ci dà un riferimento al nodo DOM nella variabile `headingEl`. Poi usiamo `export function focus()` per esporre una funzione che dà focus all'intestazione `<h2>`.

   Come possiamo accedere a quei valori esportati dal genitore? Proprio come puoi legare agli elementi DOM con la direttiva `bind:this={dom_node}`, puoi anche legare alle istanze dei componenti con `bind:this={component}`. Quindi, quando usi `bind:this` su un elemento HTML, ottieni un riferimento al nodo DOM, e quando lo fai su un componente Svelte, ottieni un riferimento all'istanza di quel componente.

2. Quindi, per legare all'istanza di `TodosStatus`, creeremo prima una variabile `todosStatus` in `Todos.svelte`. Aggiungi la seguente riga sotto le tue dichiarazioni di importazione:

   ```js
   let todosStatus; // reference to TodosStatus instance
   ```

3. Successivamente, aggiungi una direttiva `bind:this={todosStatus}` alla chiamata, come segue:

   ```svelte
   <!-- TodosStatus -->
   <TodosStatus bind:this={todosStatus} {todos} />
   ```

4. Ora possiamo chiamare il metodo `exported focus()` dalla nostra funzione `removeTodo()`:

   ```js
   function removeTodo(todo) {
     todos = todos.filter((t) => t.id !== todo.id);
     todosStatus.focus(); // give focus to status heading
   }
   ```

5. Torna alla tua app. Ora, se elimini un to-do, l'intestazione dello stato sarà focalizzata. Questo è utile per evidenziare il cambiamento nei numeri di to-do, sia per gli utenti vedenti che per gli utenti di screen reader.

> [!NOTE]
> Ti starai chiedendo perché abbiamo bisogno di dichiarare una nuova variabile per il binding del componente. Perché non possiamo semplicemente chiamare `TodosStatus.focus()`? Potresti avere più istanze di `TodosStatus` attive, quindi hai bisogno di un modo per riferirti a ciascuna istanza particolare. Ecco perché devi specificare una variabile per associare ogni istanza specifica.

## Il codice finora

### Git

Per vedere lo stato del codice come dovrebbe essere alla fine di questo articolo, accedi alla tua copia del nostro repo in questo modo:

```bash
cd mdn-svelte-tutorial/06-stores
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/06-stores
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità di sviluppo.

### REPL

Per vedere lo stato attuale del codice in un REPL, visita:

<https://svelte.dev/repl/d1fa84a5a4494366b179c87395940039?version=3.23.2>

## Sommario

In questo articolo abbiamo terminato di aggiungere tutte le funzionalità richieste alla nostra app, oltre a prenderci cura di una serie di problemi di accessibilità e usabilità. Abbiamo anche completato la suddivisione della nostra app in componenti gestibili, ciascuno con una responsabilità unica.

Nel frattempo, abbiamo visto alcune tecniche avanzate di Svelte, come:

- Affrontare i problemi di reattività quando si aggiornano oggetti e array
- Lavorare con nodi DOM utilizzando `bind:this={dom_node}` (binding degli elementi DOM)
- Utilizzare la funzione `onMount()` del ciclo di vita del componente
- Forzare Svelte a risolvere le modifiche di stato in sospeso con la funzione `tick()`
- Aggiungere funzionalità agli elementi HTML in un modo riutilizzabile e dichiarativo con la direttiva `use:action`
- Accedere ai metodi del componente utilizzando `bind:this={component}` (binding dei componenti)

Nel prossimo articolo vedremo come usare gli store per comunicare tra componenti e aggiungere animazioni ai nostri componenti.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_components","Learn_web_development/Core/Frameworks_libraries/Svelte_stores", "Learn_web_development/Core/Frameworks_libraries")}}
