---
title: "Svelte avanzato: Reattività, ciclo di vita, accessibilità"
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_components","Learn_web_development/Core/Frameworks_libraries/Svelte_stores", "Learn_web_development/Core/Frameworks_libraries")}}

Nell'ultimo articolo abbiamo aggiunto più funzionalità alla nostra lista di cose da fare e abbiamo iniziato a organizzare la nostra app in componenti. In questo articolo aggiungeremo le funzionalità finali dell'app e ulteriormente componentizzeremo la nostra app. Impareremo come affrontare i problemi di reattività relativi all'aggiornamento di oggetti e array. Per evitare errori comuni, dovremo approfondire un po' di più il sistema di reattività di Svelte. Inoltre, affronteremo alcuni problemi di accessibilità relativi alla gestione del focus e altro ancora.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Al minimo, si raccomanda di essere familiari con i linguaggi di base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenza del
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
        Imparare alcune tecniche avanzate di Svelte per risolvere problemi di reattività,
        problemi di accessibilità tramite tastiera legati al ciclo di vita dei componenti
        e altro ancora.
      </td>
    </tr>
  </tbody>
</table>

Ci concentreremo su alcuni problemi di accessibilità relativi alla gestione del focus. A tal fine, utilizzeremo alcune tecniche per accedere ai nodi DOM ed eseguire metodi come [`focus()`](/it/docs/Web/API/HTMLElement/focus) e [`select()`](/it/docs/Web/API/HTMLInputElement/select). Vedremo anche come dichiarare e ripulire listener di eventi sugli elementi DOM.

Dobbiamo anche imparare un po' sul ciclo di vita dei componenti per capire quando questi nodi DOM vengono montati e smontati dal DOM e come possiamo accedervi. Impareremo anche la direttiva `action`, che ci permetterà di estendere la funzionalità degli elementi HTML in modo riutilizzabile e dichiarativo.

Infine, impareremo qualcosa in più sui componenti. Finora abbiamo visto come i componenti possono condividere dati utilizzando props e comunicare con i loro genitori utilizzando eventi e data binding bidirezionale. Ora vedremo come i componenti possono anche esporre metodi e variabili.

I seguenti nuovi componenti verranno sviluppati nel corso di questo articolo:

- `MoreActions`: Visualizza i pulsanti _Check All_ e _Remove Completed_, ed emette gli eventi corrispondenti necessari per gestire la loro funzionalità.
- `NewTodo`: Visualizza il campo `<input>` e il pulsante _Add_ per aggiungere una nuova voce.
- `TodosStatus`: Visualizza l'intestazione di stato "x out of y items completed".

## Codificare insieme a noi

### Git

Clonare il repository GitHub (se non lo ha già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi, per arrivare allo stato attuale dell'app, eseguire

```bash
cd mdn-svelte-tutorial/05-advanced-concepts
```

Oppure scaricare direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/05-advanced-concepts
```

Ricordi di eseguire `npm install && npm run dev` per avviare la sua app in modalità di sviluppo.

### REPL

Per codificare insieme a noi usando il REPL, inizi a

<https://svelte.dev/repl/76cc90c43a37452e8c7f70521f88b698?version=3.23.2>

## Lavorare sul componente MoreActions

Ora affronteremo i pulsanti _Check All_ e _Remove Completed_. Creiamo un componente che si occuperà di visualizzare i pulsanti ed emettere gli eventi corrispondenti.

1. Creare un nuovo file, `components/MoreActions.svelte`.
2. Quando viene cliccato il primo pulsante, emetteremo un evento `checkAll` per indicare che tutte le voci devono essere selezionate/deselezionate. Quando viene cliccato il secondo pulsante, emetteremo un evento `removeCompleted` per indicare che tutte le voci completate devono essere rimosse. Inserire il seguente contenuto nel file `MoreActions.svelte`:

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

   Abbiamo anche incluso una variabile `completed` per alternare tra selezionare e deselezionare tutte le attività.

3. Torniamo in `Todos.svelte`, importeremo il nostro componente `MoreActions` e creeremo due funzioni per gestire gli eventi emessi dal componente `MoreActions`.

   Aggiungere la seguente dichiarazione di importazione sotto quelle esistenti:

   ```js
   import MoreActions from "./MoreActions.svelte";
   ```

4. Quindi aggiungere le funzioni descritte alla fine della sezione `<script>`:

   ```js
   const checkAllTodos = (completed) =>
     todos.forEach((t) => (t.completed = completed));

   const removeCompletedTodos = () =>
     (todos = todos.filter((t) => !t.completed));
   ```

5. Ora andare in fondo alla sezione di markup di `Todos.svelte` e sostituire l'elemento `<div class="btn-group">` che abbiamo copiato in `MoreActions.svelte` con una chiamata al componente `MoreActions`, in questo modo:

   ```svelte
   <!-- MoreActions -->
   <MoreActions
     on:checkAll={(e) => checkAllTodos(e.detail)}
     on:removeCompleted={removeCompletedTodos}
   />
   ```

6. OK, torniamo nell'app e proviamolo. Scoprirà che il pulsante _Remove Completed_ funziona bene, ma il pulsante _Check All_/_Uncheck All_ semplicemente fallisce silenziosamente.

Per scoprire cosa sta succedendo, dovremo approfondire un po' di più la reattività di Svelte.

## Problemi di reattività: aggiornare oggetti e array

Per vedere cosa sta succedendo possiamo registrare l'array `todos` dalla funzione `checkAllTodos()` nella console.

1. Aggiorni la sua già esistente `checkAllTodos()` alla seguente:

   ```js
   const checkAllTodos = (completed) => {
     todos.forEach((t) => (t.completed = completed));
     console.log("todos", todos);
   };
   ```

2. Torni sul suo browser, apra la console DevTools e clicchi su _Check All_/_Uncheck All_ alcune volte.

Noterà che l'array viene aggiornato con successo ogni volta che preme il pulsante (le proprietà `completed` degli oggetti `todo` vengono alternate tra `true` e `false`), ma Svelte non ne è consapevole. Ciò significa anche che in questo caso una dichiarazione reattiva come `$: console.log('todos', todos)` non sarà molto utile.

Per scoprire perché questo stia succedendo, dobbiamo capire come funziona la reattività in Svelte durante l'aggiornamento di array e oggetti.

Molti framework web utilizzano la tecnica del virtual DOM per aggiornare la pagina. Fondamentalmente, il virtual DOM è una copia in memoria del contenuto della pagina web. Il framework aggiorna questa rappresentazione virtuale, che viene poi sincronizzata con il DOM "reale". Questo è molto più veloce di aggiornare direttamente il DOM e permette al framework di applicare molte tecniche di ottimizzazione.

Questi framework, di default, rieseguono sostanzialmente tutto il nostro JavaScript ad ogni cambiamento contro questo virtual DOM, e applicano diversi metodi per memorizzare in cache calcoli costosi e ottimizzare l'esecuzione. Fanno poco o nessun tentativo di capire cosa stia facendo il nostro codice JavaScript.

Svelte non usa una rappresentazione del virtual DOM. Invece, analizza e analizza il nostro codice, crea un albero di dipendenze, e poi genera il JavaScript necessario per aggiornare solo le parti del DOM che devono essere aggiornate. Questo approccio di solito genera codice JavaScript ottimale con un minimo di sovraccarico, ma ha anche i suoi limiti.

A volte Svelte non può rilevare modifiche nelle variabili che sta osservando. Ricorda che per dire a Svelte che una variabile è cambiata, devi assegnarle un nuovo valore. Una regola semplice da tenere a mente è che **il nome della variabile aggiornata deve apparire sul lato sinistro dell'assegnazione.**

Per esempio, nel seguente pezzo di codice:

```js
const foo = obj.foo;
foo.bar = "baz";
```

Svelte non aggiornerà i riferimenti a `obj.foo.bar`, a meno che non lo segua con `obj = obj`. Questo perché Svelte non può tracciare i riferimenti agli oggetti, quindi dobbiamo dirgli esplicitamente che `obj` è cambiato emettendo un'assegnazione.

> [!NOTE]
> Se `foo` è una variabile di primo livello, può facilmente dire a Svelte di aggiornare `obj` ogni volta che `foo` viene cambiato con la seguente dichiarazione reattiva: `$: foo, obj = obj`. Con questo stiamo definendo `foo` come una dipendenza, e ogni volta che cambia Svelte eseguirà `obj = obj`.

Nella nostra funzione `checkAllTodos()`, quando eseguiamo:

```js
todos.forEach((t) => (t.completed = completed));
```

Svelte non segnerà `todos` come cambiato perché non sa che quando aggiorniamo la nostra variabile `t` all'interno del metodo `forEach()`, stiamo anche modificando l'array `todos`. E questo ha senso, perché altrimenti Svelte sarebbe consapevole del funzionamento interno del metodo `forEach()`; lo stesso sarebbe quindi vero per qualsiasi metodo allegato a qualsiasi oggetto o array.

Tuttavia, ci sono diverse tecniche che possiamo applicare per risolvere questo problema, e tutte coinvolgono l'assegnazione di un nuovo valore alla variabile guardata.

Come abbiamo già visto, possiamo semplicemente dire a Svelte di aggiornare la variabile con un'auto-assegnazione, come questa:

```js
const checkAllTodos = (completed) => {
  todos.forEach((t) => (t.completed = completed));
  todos = todos;
};
```

Questo risolverà il problema. Internamente Svelte segnalerà `todos` come cambiato e rimuoverà l'auto-assegnazione apparentemente ridondante. A parte il fatto che sembra strano, è perfettamente accettabile usare questa tecnica, e a volte è il modo più conciso per farlo.

Potremmo anche accedere all'array `todos` tramite indice, come questo:

```js
const checkAllTodos = (completed) => {
  todos.forEach((t, i) => (todos[i].completed = completed));
};
```

Le assegnazioni alle proprietà di array e oggetti - ad esempio, `obj.foo += 1` o `array[i] = x` - funzionano allo stesso modo delle assegnazioni ai valori stessi. Quando Svelte analizza questo codice, può rilevare che l'array `todos` viene modificato.

Un'altra soluzione è assegnare un nuovo array a `todos` contenente una copia di tutti i to-do con la proprietà `completed` aggiornata di conseguenza, come questo:

```js
const checkAllTodos = (completed) => {
  todos = todos.map((t) => ({ ...t, completed }));
};
```

In questo caso stiamo utilizzando il metodo [`map()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/map), che restituisce un nuovo array con i risultati dell'esecuzione della funzione fornita per ogni elemento. La funzione restituisce una copia di ogni to-do usando la [sintassi di spread](/it/docs/Web/JavaScript/Reference/Operators/Spread_syntax) e sovrascrive di conseguenza la proprietà del valore completato. Questa soluzione ha il vantaggio aggiunto di restituire un nuovo array con nuovi oggetti, evitando completamente di mutare l'array `todos` originale.

> [!NOTE]
> Svelte ci permette di specificare diverse opzioni che influenzano il modo in cui funziona il compilatore. L'opzione `<svelte:options immutable={true}/>` dice al compilatore che promettiamo di non mutare alcun oggetto. Questo gli permette di essere meno conservativo nel verificare se i valori sono cambiati e di generare codice più semplice e performante. Per ulteriori informazioni su `<svelte:options>`, controlli la [documentazione sulle opzioni di Svelte](https://svelte.dev/docs/special-elements#svelte-options).

Tutte queste soluzioni comportano un'assegnazione in cui la variabile aggiornata si trova sul lato sinistro dell'equazione. Qualsiasi di queste tecniche permetterà a Svelte di notare che il nostro array `todos` è stato modificato.

**Scegli uno e aggiorna la tua funzione `checkAllTodos()` come richiesto. Ora dovrebbe essere in grado di selezionare e deselezionare tutte le sue cose da fare in una volta. Provalo!**

## Finire il nostro componente MoreActions

Aggiungeremo un dettaglio di usabilità al nostro componente. Disabiliteremo i pulsanti quando non ci sono attività da processare. Per crearli, riceveremo l'array `todos` come prop e imposteremo la proprietà `disabled` di ciascun pulsante di conseguenza.

1. Aggiorni il suo componente `MoreActions.svelte` in questo modo:

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

2. Non dimentichi di passare il prop in `MoreActions` dall'interno di `Todos.svelte`, dove il componente viene chiamato:

   ```svelte
   <MoreActions {todos}
       on:checkAll={(e) => checkAllTodos(e.detail)}
       on:removeCompleted={removeCompletedTodos}
     />
   ```

## Lavorare con il DOM: concentrarsi sui dettagli

Ora che abbiamo completato tutte le funzionalità richieste dall'app, ci concentreremo su alcune funzionalità di accessibilità che miglioreranno l'usabilità della nostra app sia per gli utenti che usano solo la tastiera, sia per gli utenti che utilizzano un lettore di schermo.

Nello stato attuale, la nostra app presenta un paio di problemi di accessibilità tramite tastiera relativi alla gestione del focus. Consideriamo questi problemi.

## Esplorare i problemi di accessibilità tramite tastiera nella nostra app delle cose da fare

Attualmente, agli utenti che utilizzano la tastiera risulterà che il flusso di focus della nostra app non è molto prevedibile o coerente.

Se si clicca sull'input in cima alla nostra app, si vedrà un contorno spesso e tratteggiato attorno a quell'input. Questo contorno è il suo indicatore visivo che il browser è attualmente focalizzato su questo elemento.

Se è un utente che utilizza il mouse, potrebbe aver saltato questo suggerimento visivo. Ma se sta lavorando esclusivamente con la tastiera, sapere quale controllo ha il focus è di vitale importanza. Ci dice quale controllo riceverà le nostre pressioni di tasti.

Se preme ripetutamente il tasto <kbd>Tab</kbd>, vedrà l'indicatore di focus tratteggiato che cicla tra tutti gli elementi focalizzabili della pagina. Se sposta il focus sul pulsante _Edit_ e preme <kbd>Enter</kbd>, improvvisamente il focus scompare e non può più distinguere quale controllo riceverà le nostre pressioni di tasti.

Inoltre, se preme il tasto <kbd>Escape</kbd> o il tasto <kbd>Enter</kbd>, non succede nulla. E se clicca su _Cancel_ o _Save_, il focus scompare di nuovo. Per un utente che lavora con la tastiera, questo comportamento sarà confuso nel migliore dei casi.

Ci piacerebbe anche aggiungere alcune funzionalità di usabilità, come disabilitare il pulsante _Save_ quando i campi richiesti sono vuoti, dare focus a determinati elementi HTML o selezionare automaticamente i contenuti quando un input di testo riceve il focus.

Per implementare tutte queste funzionalità, avremo bisogno di accesso programmatico ai nodi DOM per eseguire funzioni come [`focus()`](/it/docs/Web/API/HTMLElement/focus) e [`select()`](/it/docs/Web/API/HTMLInputElement/select). Dovremo anche utilizzare [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) e [`removeEventListener()`](/it/docs/Web/API/EventTarget/removeEventListener) per eseguire attività specifiche quando il controllo riceve il focus.

Il problema è che tutti questi nodi DOM vengono creati dinamicamente da Svelte a runtime. Quindi dovremo aspettare che siano creati e aggiunti al DOM per poterli usare. A tal fine, dovremo imparare qualcosa sul [ciclo di vita del componente](https://learn.svelte.dev/tutorial/onmount) per capire quando possiamo accedervi — ne parleremo più avanti.

## Creare un componente NewTodo

Cominciamo estraendo il nostro modulo di nuovo to-do in un proprio componente. Con ciò che sappiamo finora possiamo creare un nuovo file di componenti e adattare il codice per emettere un evento `addTodo`, passando il nome del nuovo to-do con i dettagli addizionali.

1. Crei un nuovo file, `components/NewTodo.svelte`.
2. Inserisca i seguenti contenuti all'interno:

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

   Qui stiamo associando il `<input>` alla variabile `name` con `bind:value={name}` e disabilitando il pulsante _Add_ quando è vuoto (cioè, senza contenuto di testo) usando `disabled={!name}`. Ci stiamo anche occupando del tasto <kbd>Escape</kbd> con `on:keydown={(e) => e.key === 'Escape' && onCancel()}}`. Ogni volta che il tasto <kbd>Escape</kbd> viene premuto, eseguiremo `onCancel()`, che semplicemente ripulisce la variabile `name`.

3. Ora dobbiamo `importare` e usarlo all'interno del componente `Todos`, e aggiornare la funzione `addTodo()` per ricevere il nome del nuovo todo.

   Aggiunga la seguente dichiarazione di importazione sotto le altre all'interno di `Todos.svelte`:

   ```js
   import NewTodo from "./NewTodo.svelte";
   ```

4. E aggiorni la funzione `addTodo()` così:

   ```js
   function addTodo(name) {
     todos = [...todos, { id: newTodoId, name, completed: false }];
   }
   ```

   `addTodo()` ora riceve il nome del nuovo todo direttamente, quindi non abbiamo più bisogno della variabile `newTodoName` per darle il suo valore. Il nostro componente `NewTodo` si occupa di questo.

   > [!NOTE]
   > La sintassi `{ name }` è solo una scorciatoia per `{ name: name }`. Questa proviene dal JavaScript stesso e non ha nulla a che fare con Svelte, oltre a fornire un po' di ispirazione per le scorciatoie di Svelte.

5. Infine per questa sezione, sostituisce il markup del modulo NewTodo con una chiamata al componente `NewTodo`, come segue:

   ```svelte
   <!-- NewTodo -->
   <NewTodo on:addTodo={(e) => addTodo(e.detail)} />
   ```

## Lavorare con i nodi DOM usando la direttiva `bind:this={dom_node}`

Ora vogliamo che l'elemento `<input>` del componente `NewTodo` riacquisti la messa a fuoco ogni volta che il pulsante _Add_ viene premuto. Per questo avremo bisogno di un riferimento al nodo DOM dell'input. Svelte fornisce un modo per farlo con la direttiva `bind:this={dom_node}`. Quando specificato, non appena il componente viene montato e il nodo DOM viene creato, Svelte assegna un riferimento al nodo DOM alla variabile specificata.

Creeremo una variabile `nameEl` e la assoceremo all'input usando `bind:this={nameEl}`. Poi, dentro `addTodo()`, dopo aver aggiunto il nuovo to-do, chiameremo `nameEl.focus()` per rifocalizzare l'`<input>` di nuovo. Faremo lo stesso quando l'utente premerà il tasto <kbd>Escape</kbd>, con la funzione `onCancel()`.

Aggiorni i contenuti di `NewTodo.svelte` così:

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

Provi l'app: inserisci un nuovo nome di to-do nel campo `<input>`, premi <kbd>tab</kbd> per dare il focus al pulsante _Add_ e poi premi <kbd>Enter</kbd> o <kbd>Escape</kbd> per vedere come l'input recupera il focus.

### Autofocalizzare il nostro input

La prossima funzionalità che aggiungeremo al nostro componente `NewTodo` sarà un prop `autofocus`, che ci permetterà di specificare che vogliamo che il campo `<input>` riceva il focus al caricamento della pagina.

1. Il nostro primo tentativo è il seguente: proviamo ad aggiungere il prop `autofocus` e semplicemente a chiamare `nameEl.focus()` dal blocco `<script>`. Aggiorni la prima parte della sezione `<script>` di `NewTodo.svelte` (le prime quattro righe) per farla assomigliare a questo:

   ```svelte
   <script>
     import { createEventDispatcher } from 'svelte';
     const dispatch = createEventDispatcher();

     export let autofocus = false;

     let name = '';
     let nameEl; // reference to the name input DOM node

     if (autofocus) nameEl.focus();
   ```

2. Ora torni nel componente `Todos` e passi il prop `autofocus` nella chiamata al componente `<NewTodo>`, come segue:

   ```svelte
   <!-- NewTodo -->
   <NewTodo autofocus on:addTodo={(e) => addTodo(e.detail)} />
   ```

3. Se prova la sua app ora, vedrà che la pagina è ora bianca, e nella console degli strumenti di sviluppo vedrà un errore del tipo: `TypeError: nameEl is undefined`.

Per capire cosa sta succedendo qui, parliamo un po' di più di quel [ciclo di vita dei componenti](https://learn.svelte.dev/tutorial/onmount) di cui abbiamo parlato prima.

## Ciclo di vita del componente, e la funzione `onMount()`

Quando un componente viene istanziato, Svelte esegue il codice di inizializzazione (cioè, la sezione `<script>` del componente). Ma in quel momento, tutti i nodi che compongono il componente non sono ancora collegati al DOM, anzi, non esistono ancora.

Quindi, come si può sapere quando il componente è già stato creato e montato nel DOM? La risposta è che ogni componente ha un ciclo di vita che inizia quando viene creato e termina quando viene distrutto. Esistono alcune funzioni che permettono di eseguire il codice in momenti chiave durante quel ciclo di vita.

Quella che utilizzerà più frequentemente è `onMount()`, che ci permette di eseguire un callback non appena il componente è stato montato nel DOM. Proviamo a vedere cosa succede alla variabile `nameEl`.

1. Per cominciare, aggiunga la seguente riga all'inizio della sezione `<script>` di `NewTodo.svelte`:

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

3. Ora rimuova la riga `if (autofocus) nameEl.focus()` per evitare di lanciare l'errore che vedevamo prima.
4. L'app ora funzionerà di nuovo, e vedrà quanto segue nella sua console:

   ```plain
   initializing: undefined
   mounted: <input id="todo-0" class="input input__lg" type="text" autocomplete="off">
   ```

   Come può vedere, mentre il componente si sta inizializzando, `nameEl` è indefinito, il che ha senso perché il nodo `<input>` non esiste ancora. Dopo che il componente è stato montato, Svelte assegna un riferimento al nodo DOM `<input>` alla variabile `nameEl`, grazie alla direttiva `bind:this={nameEl}`.

5. Per far funzionare la funzionalità di autofocus, sostituire il blocco precedente `console.log()`/`onMount()` che ha aggiunto con questo:

   ```js
   onMount(() => autofocus && nameEl.focus()); // if autofocus is true, we run nameEl.focus()
   ```

6. Torni di nuovo alla sua app, e ora vedrà che il campo `<input>` è focalizzato al caricamento della pagina.

> [!NOTE]
> Può dare un'occhiata alle altre [funzioni del ciclo di vita nella documentazione di Svelte](https://svelte.dev/docs/svelte), e può vederle in azione nel [tutorial interattivo](https://learn.svelte.dev/tutorial/onmount).

## Attendere che il DOM venga aggiornato con la funzione `tick()`

Ora prenderemo in carico i dettagli della gestione del focus del componente `Todo`. Prima di tutto, vogliamo che l'`<input>` del componente `Todo` riceva focus quando entriamo in modalità di modifica premendo il suo pulsante _Edit_. Allo stesso modo di quanto visto in precedenza, creeremo una variabile `nameEl` all'interno di `Todo.svelte` e chiameremo `nameEl.focus()` dopo aver impostato la variabile `editing` su `true`.

1. Aperti il file `components/Todo.svelte` e aggiunga una dichiarazione di variabile `nameEl` proprio sotto le sue dichiarazioni di editing e name:

   ```js
   let nameEl; // reference to the name input DOM node
   ```

2. Ora aggiorni la sua funzione `onEdit()` in questo modo:

   ```js
   function onEdit() {
     editing = true; // enter editing mode
     nameEl.focus(); // set focus to name input
   }
   ```

3. E infine, assegni `nameEl` al campo `<input>` aggiornandolo in questo modo:

   ```svelte
   <input
     bind:value={name}
     bind:this={nameEl}
     type="text"
     id="todo-{todo.id}"
     autocomplete="off"
     class="todo-text" />
   ```

4. Tuttavia, quando prova l'app aggiornata, otterrà un errore del tipo "TypeError: nameEl è indefinito" nella console quando preme il pulsante _Edit_ di un to-do.

Quindi, cosa sta succedendo qui? Quando aggiorna lo stato di un componente in Svelte, non aggiorna immediatamente il DOM. Invece, attende fino al prossimo microtask per vedere se ci sono altri cambiamenti che devono essere applicati, inclusi quelli in altri componenti. Facendo ciò si evitano lavori non necessari e si permette al browser di raggruppare le cose più efficacemente.

In questo caso, quando `editing` è `false`, l'`<input>` di modifica non è visibile perché non esiste nel DOM. All'interno della funzione `onEdit()` impostiamo `editing = true` e immediatamente dopo tentiamo di accedere alla variabile `nameEl` ed eseguire `nameEl.focus()`. Il problema qui è che Svelte non ha ancora aggiornato il DOM.

Un modo per risolvere questo problema è utilizzare [`setTimeout()`](/it/docs/Web/API/Window/setTimeout) per ritardare la chiamata a `nameEl.focus()` fino al prossimo ciclo di eventi e dare a Svelte l'opportunità di aggiornare il DOM.

Provi ora:

```js
function onEdit() {
  editing = true; // enter editing mode
  setTimeout(() => nameEl.focus(), 0); // asynchronous call to set focus to name input
}
```

La soluzione sopra funziona, ma è piuttosto inelegante. Svelte fornisce un modo migliore per gestire questi casi. La funzione [`tick()`](https://learn.svelte.dev/tutorial/tick) restituisce una promessa che si risolve non appena le eventuali modifiche di stato in sospeso sono state applicate al DOM (o immediatamente, se non ci sono modifiche di stato in sospeso). Proviamola ora.

1. Per prima cosa, importi `tick` in cima alla sezione `<script>` insieme al suo import esistente:

   ```js
   import { tick } from "svelte";
   ```

2. Successivamente, chiami `tick()` con [`await`](/it/docs/Web/JavaScript/Reference/Operators/await) da una [funzione asincrona](/it/docs/Web/JavaScript/Reference/Statements/async_function); aggiorni `onEdit()` in questo modo:

   ```js
   async function onEdit() {
     editing = true; // enter editing mode
     await tick();
     nameEl.focus();
   }
   ```

3. Se prova ora vedrà che tutto funziona come previsto.

> [!NOTE]
> Per vedere un altro esempio che utilizza `tick()`, visiti il [tutorial di Svelte](https://learn.svelte.dev/tutorial/tick).

## Aggiungere funzionalità agli elementi HTML con la direttiva `use:action`

Successivamente, vogliamo che l'input del nome `<input>` selezioni automaticamente tutto il testo durante il focus. Inoltre, vogliamo sviluppare questo in modo tale che possa essere facilmente riutilizzato su qualsiasi `<input>` HTML e applicato in modo dichiarativo. Useremo questo requisito come scusa per mostrare una funzionalità molto potente che Svelte ci fornisce per aggiungere funzionalità agli elementi HTML regolari: [azioni](https://svelte.dev/docs/svelte-action).

Per selezionare il testo di un nodo input DOM, dobbiamo chiamare [`select()`](/it/docs/Web/API/HTMLInputElement/select). Per fare in modo che questa funzione venga chiamata ogni volta che il nodo viene focalizzato, abbiamo bisogno di un event listener del genere:

```js
node.addEventListener("focus", (event) => node.select());
```

E, per evitare perdite di memoria, dovremmo anche chiamare la funzione [`removeEventListener()`](/it/docs/Web/API/EventTarget/removeEventListener) quando il nodo viene distrutto.

> [!NOTE]
> Tutto questo è solo funzionalità standard del WebAPI; nulla qui è specifico di Svelte.

Potremmo ottenere tutto ciò nel nostro componente `Todo` ogni volta che aggiungiamo o rimuoviamo l'`<input>` dal DOM, ma dovremmo essere molto attenti per aggiungere il listener di eventi dopo che il nodo è stato aggiunto al DOM, e rimuovere il listener prima che il nodo venga rimosso dal DOM. Inoltre, la nostra soluzione non sarebbe molto riutilizzabile.

È qui che entrano in gioco le azioni di Svelte. In sostanza, ci permettono di eseguire una funzione ogni volta che un elemento è stato aggiunto al DOM e dopo che è stato rimosso dal DOM.

Nel nostro caso d'uso immediato, definiremo una funzione chiamata `selectOnFocus()` che riceverà un nodo come parametro. La funzione aggiungerà un listener di eventi a quel nodo in modo che ogni volta che viene messo a fuoco seleziona il testo. Poi restituirà un oggetto con una proprietà `destroy`. La proprietà `destroy` è ciò che Svelte eseguirà dopo aver rimosso il nodo dal DOM. Qui rimuoveremo il listener per assicurarci di non lasciare alcuna perdita di memoria dietro.

1. Creiamo la funzione `selectOnFocus()`. Aggiunga quanto segue alla fine della sezione `<script>` di `Todo.svelte`:

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

2. Ora abbiamo bisogno di dire al `<input>` di usare quella funzione con la direttiva [`use:action`](https://svelte.dev/docs/element-directives#use-action):

   ```svelte
   <input use:selectOnFocus />
   ```

   Con questa direttiva stiamo dicendo a Svelte di eseguire questa funzione, passando il nodo DOM dell'`<input>` come parametro, non appena il componente è montato sul DOM. Sarà inoltre incaricata di eseguire la funzione `destroy` quando il componente viene rimosso dal DOM. Quindi, con la direttiva `use`, Svelte si prende cura del ciclo di vita del componente per noi.

   Nel nostro caso, il nostro `<input>` finirebbe così: aggiorni la coppia di etichette/elementi di input del componente (all'interno del template di modifica) come segue:

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

3. Proviamo. Vada alla sua app, prema il pulsante _Edit_ di un to-do, poi <kbd>Tab</kbd> per spostare il focus fuori dal campo `<input>`. Ora faccia clic sul campo `<input>`, e vedrà che l'intero testo dell'input viene selezionato.

### Rendere l'azione riutilizzabile

Ora rendiamo questa funzione veramente riutilizzabile tra i componenti. `selectOnFocus()` è solo una funzione senza alcuna dipendenza dal componente `Todo.svelte`, quindi possiamo semplicemente estrarla in un file e usarla da lì.

1. Crei un nuovo file, `actions.js`, all'interno della cartella `src`.
2. Gli dia il seguente contenuto:

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

3. Ora la importi all'interno di `Todo.svelte`; aggiunga la seguente dichiarazione di importazione proprio sotto le altre:

   ```js
   import { selectOnFocus } from "../actions.js";
   ```

4. E rimuova la definizione di `selectOnFocus()` da `Todo.svelte`, poiché non ne abbiamo più bisogno lì.

### Riutilizzare la nostra azione

Per dimostrare la riusabilità della nostra azione, usiamola in `NewTodo.svelte`.

1. Importi `selectOnFocus()` da `actions.js` anche in questo file, come prima:

   ```js
   import { selectOnFocus } from "../actions.js";
   ```

2. Aggiunga la direttiva `use:selectOnFocus` al `<input>`, in questo modo:

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

Con poche righe di codice possiamo aggiungere funzionalità agli elementi HTML regolari in un modo molto riutilizzabile e dichiarativo. Basta un `import` e una breve direttiva come `use:selectOnFocus` che descrive chiaramente il suo scopo. E possiamo ottenere questo senza la necessità di creare un elemento wrapper personalizzato come `TextInput`, `MyInput` o simili. Inoltre, può aggiungere quante più direttive `use:action` desidera a un elemento.

Inoltre, non abbiamo dovuto lottare con `onMount()`, `onDestroy()`, o `tick()` — la direttiva `use` si prende cura del ciclo di vita del componente per noi.

### Altri miglioramenti delle azioni

Nella sezione precedente, mentre lavoravamo con i componenti `Todo`, abbiamo dovuto occuparci di `bind:this`, `tick()`, e funzioni `async` solo per dare focus al nostro `<input>` non appena veniva aggiunto al DOM.

1. Ecco come possiamo implementarlo invece con le azioni:

   ```js
   const focusOnInit = (node) =>
     node && typeof node.focus === "function" && node.focus();
   ```

2. E poi nel nostro markup dobbiamo solo aggiungere un'altra direttiva `use:`:

   ```svelte
   <input bind:value={name} use:selectOnFocus use:focusOnInit />
   ```

3. Ora la nostra funzione `onEdit()` può essere molto più semplice:

   ```js
   function onEdit() {
     editing = true; // enter editing mode
   }
   ```

Come ultimo esempio prima di passare oltre, torniamo al nostro componente `Todo.svelte` e diamo focus al pulsante _Edit_ dopo che l'utente ha premuto _Save_ o _Cancel_.

Potremmo provare a riutilizzare di nuovo la nostra azione `focusOnInit`, aggiungendo `use:focusOnInit` al pulsante _Edit_. Ma introdurremmo un bug sottile. Quando aggiunge un nuovo to-do, il focus sarà posto sul pulsante _Edit_ del to-do recentemente aggiunto. Questo perché l'azione `focusOnInit` viene eseguita quando il componente viene creato.

Non è quello che vogliamo — vogliamo che il pulsante _Edit_ riceva il focus solo quando l'utente ha premuto _Save_ o _Cancel_.

1. Quindi, torni a file `Todo.svelte`.
2. Iniziamo creando un flag chiamato `editButtonPressed` e lo inizializziamo a `false`. Aggiunga questo proprio sotto le sue altre definizioni di variabile:

   ```js
   let editButtonPressed = false; // track if edit button has been pressed, to give focus to it after cancel or save
   ```

3. Poi modificheremo la funzionalità del pulsante _Edit_ per memorizzare questo flag, e creiamo l'azione per esso. Aggiorni la funzione `onEdit()` così:

   ```js
   function onEdit() {
     editButtonPressed = true; // user pressed the Edit button, focus will come back to the Edit button
     editing = true; // enter editing mode
   }
   ```

4. Sotto di esso, aggiunga la seguente definizione per `focusEditButton()`:

   ```js
   const focusEditButton = (node) => editButtonPressed && node.focus();
   ```

5. Infine usiamo l'azione `focusEditButton` sul pulsante _Edit_, in questo modo:

   ```svelte
   <button type="button" class="btn" on:click={onEdit} use:focusEditButton>
     Edit<span class="visually-hidden"> {todo.name}</span>
   </button>
   ```

6. Torni indietro e provi di nuovo la sua app. A questo punto, ogni volta che il pulsante _Edit_ viene aggiunto al DOM, viene eseguita l'azione `focusEditButton`, ma darà focus al pulsante solo se il flag `editButtonPressed` è `true`.

> [!NOTE]
> Abbiamo appena grattato la superficie delle azioni qui. Le azioni possono anche avere parametri reattivi e Svelte ci permette di rilevare quando uno di questi parametri cambia. Così possiamo aggiungere funzionalità che si integrano bene con il sistema reattivo di Svelte. Per un'introduzione più dettagliata alle azioni, consideri di dare un'occhiata al [tutorial interattivo di Svelte](https://learn.svelte.dev/tutorial/actions) o alla [documentazione di Svelte `use:action`](https://svelte.dev/docs/element-directives#use-action).

## Binding dei componenti: esposizione di metodi e variabili di componenti utilizzando la direttiva `bind:this={component}`

C'è ancora un fastidio di accessibilità lasciato. Quando l'utente preme il pulsante _Delete_, il focus svanisce.

L'ultima funzionalità che esamineremo in questo articolo riguarda l'impostazione del focus sull'intestazione dello stato dopo che un to-do è stato eliminato.

Perché l'intestazione dello stato? In questo caso, l'elemento che aveva il focus è stato eliminato, quindi non c'è un candidato chiaro per ricevere il focus. Abbiamo scelto l'intestazione dello stato perché è vicino alla lista dei to-do, ed è un modo per dare un feedback visivo sulla rimozione della task, oltre a indicare cosa è successo agli utenti di screen reader.

Eccezionalmente estrarremo l'intestazione dello stato in un suo componente.

1. Crei un nuovo file, `components/TodosStatus.svelte`.
2. Aggiunga i seguenti contenuti al suo interno:

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

3. Importi il file all'inizio di `Todos.svelte`, aggiungendo la seguente dichiarazione di importazione sotto le altre:

   ```js
   import TodosStatus from "./TodosStatus.svelte";
   ```

4. Sostituisca l'intestazione `<h2>` dello stato all'interno di `Todos.svelte` con una chiamata al componente `TodosStatus`, passando `todos` come prop, in questo modo:

   ```svelte
   <TodosStatus {todos} />
   ```

5. Può anche fare un po' di pulizia, rimuovendo le variabili `totalTodos` e `completedTodos` da `Todos.svelte`. Basta rimuovere le righe `$: totalTodos = …` e `$: completedTodos = …`, e rimuovere anche il riferimento a `totalTodos` quando calcoliamo `newTodoId` e utilizzare `todos.length` invece. Per far ciò, sostituire il blocco che inizia con `let newTodoId` con questo:

   ```js
   $: newTodoId = todos.length ? Math.max(...todos.map((t) => t.id)) + 1 : 1;
   ```

6. Tutto funziona come previsto — abbiamo appena estratto il pezzo finale di markup in un proprio componente.

Ora dobbiamo trovare un modo per dare focus all'intestazione `<h2>` dello stato dopo che un to-do è stato rimosso.

Finora abbiamo visto come inviare informazioni a un componente tramite props e come un componente può comunicare con il suo genitore emettendo eventi o utilizzando il data binding bidirezionale. Il componente figlio potrebbe ottenere un riferimento al nodo `<h2>` `utilizzando bind:this={dom_node}` ed esporlo all'esterno utilizzando il data binding bidirezionale. Ma facendo ciò si romperebbe l'incapsulamento del componente; impostare il focus su di esso dovrebbe essere di sua responsabilità.

Quindi abbiamo bisogno che il componente `TodosStatus` esponga un metodo che il suo genitore possa chiamare per dare focus ad esso. È uno scenario molto comune che un componente debba esporre qualche comportamento o informazione al consumer; vediamo come ottenerlo con Svelte.

Abbiamo già visto che Svelte usa `export let varname = …` per [dichiarare props](https://svelte.dev/docs/svelte-components#script-1-export-creates-a-component-prop). Ma se invece di usare `let` esporta una `const`, `class`, o `function`, è read-only all'esterno del componente. Tuttavia, le espressioni di funzione sono validi props. Nel seguente esempio, le prime tre dichiarazioni sono props, e il resto sono valori esportati:

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

Con questo in mente, torniamo al nostro caso d'uso. Creeremo una funzione chiamata `focus()` che dà focus all'intestazione `<h2>`. Per questo avremo bisogno di una variabile `headingEl` per tenere il riferimento al nodo DOM, e dovremo associarla all'elemento `<h2>` utilizzando `bind:this={headingEl}`. Il nostro metodo focus eseguirà semplicemente `headingEl.focus()`.

1. Aggiorni i contenuti di `TodosStatus.svelte` come segue:

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

   Si noti che abbiamo aggiunto un attributo `tabindex` all'`<h2>` per permettere all'elemento di ricevere focus programmaticamente.

   Come visto in precedenza, usando la direttiva `bind:this={headingEl}` otteniamo un riferimento al nodo DOM nella variabile `headingEl`. Poi utilizziamo `export function focus()` per esporre una funzione che dà focus all'intestazione `<h2>`.

   Come possiamo accedere a quei valori esportati dal genitore? Proprio come può fare il bind agli elementi DOM con la direttiva `bind:this={dom_node}`, può anche fare il bind alle istanze dei componenti stessi con `bind:this={component}`. Quindi, quando utilizza `bind:this` su un elemento HTML, ottiene un riferimento al nodo DOM, e quando lo fa su un componente Svelte, ottiene un riferimento all'istanza di quel componente.

2. Quindi per fare il bind all'istanza di `TodosStatus`, prima creiamo una variabile `todosStatus` in `Todos.svelte`. Aggiunga la seguente riga sotto le sue dichiarazioni di importazione:

   ```js
   let todosStatus; // reference to TodosStatus instance
   ```

3. Successivamente, aggiunga una direttiva `bind:this={todosStatus}` alla chiamata, come segue:

   ```svelte
   <!-- TodosStatus -->
   <TodosStatus bind:this={todosStatus} {todos} />
   ```

4. Ora possiamo chiamare il metodo `focus()` esportato dalla nostra funzione `removeTodo()`:

   ```js
   function removeTodo(todo) {
     todos = todos.filter((t) => t.id !== todo.id);
     todosStatus.focus(); // give focus to status heading
   }
   ```

5. Torni alla sua app. Ora se elimina un qualsiasi to-do, l'intestazione dello stato sarà messa a fuoco. Questo è utile per evidenziare il cambiamento nel numero di to-do sia agli utenti vedenti che a quelli che utilizzano lettori di schermo.

> [!NOTE]
> Potrebbe chiedersi perché abbiamo bisogno di dichiarare una nuova variabile per il binding del componente. Perché non possiamo semplicemente chiamare `TodosStatus.focus()`? Potrebbe avere più istanze `TodosStatus` attive, quindi ha bisogno di un modo per fare riferimento a ciascuna istanza particolare. Ecco perché deve specificare una variabile per fare il bind a ciascuna istanza specifica.

## Il codice finora

### Git

Per vedere lo stato del codice come dovrebbe essere alla fine di questo articolo, acceda alla sua copia del nostro repository in questo modo:

```bash
cd mdn-svelte-tutorial/06-stores
```

Oppure scarichi direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/06-stores
```

Ricordi di eseguire `npm install && npm run dev` per avviare la sua app in modalità di sviluppo.

### REPL

Per vedere lo stato attuale del codice in un REPL, visiti:

<https://svelte.dev/repl/d1fa84a5a4494366b179c87395940039?version=3.23.2>

## Sommario

In questo articolo abbiamo terminato di aggiungere tutte le funzionalità richieste alla nostra app, e ci siamo occupati di una serie di problemi di accessibilità e usabilità. Inoltre, abbiamo finito di suddividere la nostra app in componenti gestibili, ognuno con una responsabilità unica.

Nel frattempo, abbiamo visto alcune tecniche avanzate di Svelte, come:

- Gestire i problemi di reattività quando si aggiornano oggetti e array
- Lavorare con i nodi DOM usando `bind:this={dom_node}` (binding degli elementi DOM)
- Utilizzare la funzione del ciclo di vita del componente `onMount()`
- Costringere Svelte a risolvere le modifiche di stato in sospeso con la funzione `tick()`
- Aggiungere funzionalità agli elementi HTML in un modo riutilizzabile e dichiarativo con la direttiva `use:action`
- Accedere ai metodi dei componenti usando `bind:this={component}` (binding dei componenti)

Nel prossimo articolo vedremo come usare i negozi per comunicare tra i componenti e aggiungere animazioni ai nostri componenti.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_components","Learn_web_development/Core/Frameworks_libraries/Svelte_stores", "Learn_web_development/Core/Frameworks_libraries")}}
