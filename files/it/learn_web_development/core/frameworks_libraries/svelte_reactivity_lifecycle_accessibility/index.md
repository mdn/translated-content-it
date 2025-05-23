---
title: "Svelte Avanzato: Reattività, ciclo di vita, accessibilità"
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_components","Learn_web_development/Core/Frameworks_libraries/Svelte_stores", "Learn_web_development/Core/Frameworks_libraries")}}

Nell'ultimo articolo abbiamo aggiunto più funzionalità alla nostra lista di cose da fare e iniziato a organizzare la nostra app in componenti. In questo articolo aggiungeremo le funzionalità finali dell'app e suddivideremo ulteriormente la nostra app in componenti. Impareremo a gestire i problemi di reattività legati all'aggiornamento di oggetti e array. Per evitare errori comuni, dovremo approfondire un po' il sistema di reattività di Svelte. Guarderemo anche alla soluzione di alcuni problemi di focalizzazione relativi all'accessibilità e altro ancora.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È consigliato avere familiarità con i linguaggi principali
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/riga di comando</a>.
        </p>
        <p>
          Avrete bisogno di un terminale con node e npm installati per compilare e costruire
          la vostra app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare alcune tecniche avanzate di Svelte riguardanti la soluzione dei problemi di reattività,
        problemi di accessibilità della tastiera relativi al ciclo di vita dei componenti,
        e altro ancora.
      </td>
    </tr>
  </tbody>
</table>

Ci concentreremo su alcuni problemi di accessibilità che coinvolgono la gestione del focus. Per farlo, utilizzeremo alcune tecniche per accedere ai nodi DOM ed eseguire metodi come [`focus()`](/it/docs/Web/API/HTMLElement/focus) e [`select()`](/it/docs/Web/API/HTMLInputElement/select). Vedremo anche come dichiarare e pulire i listener degli eventi sugli elementi DOM.

Dobbiamo anche imparare un po' sul ciclo di vita dei componenti per capire quando questi nodi DOM vengono montati e smontati dal DOM e come possiamo accedervi. Impareremo anche la direttiva `action`, che ci permetterà di estendere la funzionalità degli elementi HTML in un modo riutilizzabile e dichiarativo.

Infine, impareremo un po' più sui componenti. Finora, abbiamo visto come i componenti possono condividere dati usando props, e comunicare con i loro genitori usando eventi e binding a due vie. Ora vedremo come i componenti possono anche esporre metodi e variabili.

I seguenti nuovi componenti saranno sviluppati nel corso di questo articolo:

- `MoreActions`: Visualizza i pulsanti _Check All_ e _Remove Completed_, ed emette gli eventi corrispondenti richiesti per gestire la loro funzionalità.
- `NewTodo`: Visualizza il campo `<input>` e il pulsante _Add_ per aggiungere un nuovo compito.
- `TodosStatus`: Visualizza l'intestazione dello stato "x su y elementi completati".

## Codifica insieme a noi

### Git

Clona il repository GitHub (se non lo hai già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi per arrivare allo stato attuale dell'app, esegui

```bash
cd mdn-svelte-tutorial/05-advanced-concepts
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/05-advanced-concepts
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità di sviluppo.

### REPL

Per codificare insieme a noi usando il REPL, inizia da

<https://svelte.dev/repl/76cc90c43a37452e8c7f70521f88b698?version=3.23.2>

## Lavorare sul componente MoreActions

Ora affronteremo i pulsanti _Check All_ e _Remove Completed_. Creiamo un componente che sarà incaricato di visualizzare i pulsanti ed emettere i corrispondenti eventi.

1. Crea un nuovo file, `components/MoreActions.svelte`.
2. Quando si clicca sul primo pulsante, emetteremo un evento `checkAll` per segnalare che tutti i compiti devono essere selezionati/deselezionati. Quando si clicca sul secondo pulsante, emetteremo un evento `removeCompleted` per segnalare che tutti i compiti completati devono essere rimossi. Inserisci il seguente contenuto nel tuo file `MoreActions.svelte`:

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

   Abbiamo anche incluso una variabile `completed` per alternare tra selezionare e deselezionare tutti i compiti.

3. Tornando a `Todos.svelte`, importeremo il nostro componente `MoreActions` e creeremo due funzioni per gestire gli eventi emessi dal componente `MoreActions`.

   Aggiungi la seguente dichiarazione di importazione sotto quelle esistenti:

   ```js
   import MoreActions from "./MoreActions.svelte";
   ```

4. Quindi aggiungi le funzioni descritte alla fine della sezione `<script>`:

   ```js
   const checkAllTodos = (completed) =>
     todos.forEach((t) => (t.completed = completed));

   const removeCompletedTodos = () =>
     (todos = todos.filter((t) => !t.completed));
   ```

5. Ora vai in fondo alla sezione di markup di `Todos.svelte` e sostituisci l'elemento `<div class="btn-group">` che abbiamo copiato in `MoreActions.svelte` con una chiamata al componente `MoreActions`, come segue:

   ```svelte
   <!-- MoreActions -->
   <MoreActions
     on:checkAll={(e) => checkAllTodos(e.detail)}
     on:removeCompleted={removeCompletedTodos}
   />
   ```

6. OK, torniamo nell'app e proviamo. Scoprirai che il pulsante _Remove Completed_ funziona bene, ma il pulsante _Check All_/_Uncheck All_ fallisce silenziosamente.

Per scoprire cosa sta succedendo qui, dovremo approfondire un po' la reattività di Svelte.

## Insidie della reattività: aggiornare oggetti e array

Per vedere cosa sta succedendo possiamo registrare la matrice `todos` dalla funzione `checkAllTodos()` nella console.

1. Aggiorna la tua funzione `checkAllTodos()` esistente come segue:

   ```js
   const checkAllTodos = (completed) => {
     todos.forEach((t) => (t.completed = completed));
     console.log("todos", todos);
   };
   ```

2. Torna nel tuo browser, apri la console degli strumenti di sviluppo e clicca su _Check All_/_Uncheck All_ alcune volte.

Noterai che l'array viene aggiornato con successo ogni volta che premi il pulsante (le proprietà `completed` degli oggetti `todo` vengono alternate tra `true` e `false`), ma Svelte non ne è consapevole. Questo significa anche che, in questo caso, un'istruzione reattiva come `$: console.log('todos', todos)` non sarà molto utile.

Per scoprire perché questo accade, dobbiamo capire come funziona la reattività in Svelte quando si aggiornano array e oggetti.

Molti framework web utilizzano la tecnica del DOM virtuale per aggiornare la pagina. Fondamentalmente, il DOM virtuale è una copia in memoria del contenuto della pagina web. Il framework aggiorna questa rappresentazione virtuale, che poi viene sincronizzata con il DOM "reale". Questo è molto più veloce rispetto all'aggiornamento diretto del DOM e permette al framework di applicare molte tecniche di ottimizzazione.

Questi framework, di default, rieseguono fondamentalmente tutto il nostro JavaScript ad ogni cambio contro questo DOM virtuale e applicano diversi metodi per memorizzare calcoli costosi e ottimizzare l'esecuzione. Tentano poco o nulla di comprendere cosa stia facendo il nostro codice JavaScript.

Svelte non utilizza una rappresentazione del DOM virtuale. Invece, analizza e analizza il nostro codice, crea un albero delle dipendenze, e poi genera il JavaScript necessario per aggiornare solo le parti del DOM che devono essere aggiornate. Questo approccio di solito genera codice JavaScript ottimale con il minimo overhead, ma ha anche le sue limitazioni.

A volte Svelte non può rilevare le modifiche alle variabili monitorate. Ricorda che per dire a Svelte che una variabile è cambiata, devi assegnargli un nuovo valore. Una semplice regola da tenere a mente è che **Il nome della variabile aggiornata deve apparire sul lato sinistro dell'assegnazione.**

Ad esempio, nel seguente pezzo di codice:

```js
const foo = obj.foo;
foo.bar = "baz";
```

Svelte non aggiornerà i riferimenti a `obj.foo.bar`, a meno che tu non lo segua con `obj = obj`. Questo perché Svelte non può tracciare i riferimenti degli oggetti, quindi dobbiamo esplicitamente dire che `obj` è cambiato emettendo un'assegnazione.

> [!NOTE]
> Se `foo` è una variabile di alto livello, puoi facilmente dire a Svelte di aggiornare `obj` ogni volta che `foo` cambia con la seguente istruzione reattiva: `$: foo, obj = obj`. Con questo stiamo definendo `foo` come una dipendenza, e ogni volta che cambia Svelte eseguirà `obj = obj`.

Nella nostra funzione `checkAllTodos()`, quando eseguiamo:

```js
todos.forEach((t) => (t.completed = completed));
```

Svelte non segnerà `todos` come cambiato perché non sa che quando aggiorniamo la nostra variabile `t` all'interno del metodo `forEach()`, stiamo anche modificando l'array `todos`. E ha senso, perché altrimenti Svelte sarebbe a conoscenza delle funzioni interne del metodo `forEach()`; lo stesso sarebbe quindi vero per qualsiasi metodo collegato a un qualsiasi oggetto o array.

Ci sono, tuttavia, diverse tecniche che possiamo applicare per risolvere questo problema, e tutte prevedono l'assegnazione di un nuovo valore alla variabile osservata.

Come abbiamo già visto, potremmo dire a Svelte di aggiornare la variabile con un'auto-assegnazione, come questa:

```js
const checkAllTodos = (completed) => {
  todos.forEach((t) => (t.completed = completed));
  todos = todos;
};
```

Questo risolverà il problema. Internamente Svelte segnerà `todos` come cambiato e rimuoverà l'apparente ridondanza dell'auto-assegnazione. A parte il fatto che sembra strano, è perfettamente OK usare questa tecnica, e a volte è il modo più conciso per farlo.

Potremmo anche accedere all'array `todos` per indice, come questo:

```js
const checkAllTodos = (completed) => {
  todos.forEach((t, i) => (todos[i].completed = completed));
};
```

Le assegnazioni alle proprietà di array e oggetti — ad esempio, `obj.foo += 1` o `array[i] = x` — funzionano allo stesso modo delle assegnazioni dirette ai valori. Quando Svelte analizza questo codice, può rilevare che l'array `todos` è stato modificato.

Un'altra soluzione è assegnare un nuovo array a `todos` contenente una copia di tutti i compiti con la proprietà `completed` aggiornata di conseguenza, così:

```js
const checkAllTodos = (completed) => {
  todos = todos.map((t) => ({ ...t, completed }));
};
```

In questo caso stiamo usando il metodo [`map()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/map), che restituisce un nuovo array con i risultati dell'esecuzione della funzione fornita per ciascun elemento. La funzione restituisce una copia di ciascun compito utilizzando la [sintassi spread](/it/docs/Web/JavaScript/Reference/Operators/Spread_syntax) e sovrascrive la proprietà del valore completato di conseguenza. Questa soluzione ha il beneficio aggiuntivo di restituire un nuovo array con nuovi oggetti, evitando completamente di modificare l'array `todos` originale.

> [!NOTE]
> Svelte ci permette di specificare diverse opzioni che influenzano il funzionamento del compilatore. L'opzione `<svelte:options immutable={true}/>` dice al compilatore che si promette di non modificare alcun oggetto. Questo gli consente di essere meno conservativo riguardo al controllo di eventuali cambiamenti nei valori e di generare codice più semplice e performante. Per ulteriori informazioni su `<svelte:options>`, controlla la [documentazione delle opzioni Svelte](https://svelte.dev/docs/special-elements#svelte-options).

Tutte queste soluzioni prevedono un'assegnazione in cui la variabile aggiornata è sul lato sinistro dell'equazione. Qualsiasi di queste tecniche consentirà a Svelte di notare che il nostro array `todos` è stato modificato.

**Scegline una, e aggiorna la tua funzione `checkAllTodos()` come richiesto. Ora dovresti essere in grado di selezionare tutte le tue cose da fare e deselezionarle tutte insieme. Provalo!**

## Finire il nostro componente MoreActions

Aggiungeremo un dettaglio di usabilità al nostro componente. Disabiliteremo i pulsanti quando non ci sono attività da elaborare. Per creare questo, riceveremo l'array `todos` come una prop e imposteremo di conseguenza la proprietà `disabled` di ciascun pulsante.

1. Aggiorna il tuo componente `MoreActions.svelte` come segue:

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

2. Non dimenticare di passare la prop in `MoreActions` da dentro `Todos.svelte`, dove il componente viene chiamato:

   ```svelte
   <MoreActions {todos}
       on:checkAll={(e) => checkAllTodos(e.detail)}
       on:removeCompleted={removeCompletedTodos}
     />
   ```

## Lavorare con il DOM: concentrarsi sui dettagli

Ora che abbiamo completato tutta la funzionalità richiesta dell'app, ci concentreremo su alcune funzionalità di accessibilità che miglioreranno l'usabilità della nostra app per utenti esclusivamente da tastiera e utenti con lettori di schermo.

Nello stato attuale la nostra app ha un paio di problemi di accessibilità della tastiera che coinvolgono la gestione del focus. Diamo un'occhiata a questi problemi.

## Esplorare i problemi di accessibilità della tastiera nella nostra app di cose da fare

In questo momento, gli utenti da tastiera troveranno che il flusso di focus della nostra app non è molto prevedibile o coerente.

Se fai clic sull'input nella parte superiore della nostra app, vedrai un contorno tratteggiato e spesso attorno a quell'input. Questo contorno è il tuo indicatore visivo che il browser è attualmente concentrato su questo elemento.

Se sei un utente di mouse, potresti aver saltato questo suggerimento visivo. Ma se lavori esclusivamente con la tastiera, sapere quale controllo ha il focus è di vitale importanza. Ci dice quale controllo riceverà le nostre battute.

Se premi ripetutamente il tasto <kbd>Tab</kbd>, vedrai l'indicatore di focus tratteggiato che scorre tra tutti gli elementi focalizzabili sulla pagina. Se sposti il focus sul pulsante _Edit_ e premi <kbd>Enter</kbd>, improvvisamente il focus scompare e non puoi più sapere quale controllo riceverà le nostre battute.

Inoltre, se premi il tasto <kbd>Escape</kbd> o <kbd>Enter</kbd>, non succede nulla. E se fai clic su _Cancel_ o _Save_, il focus scompare di nuovo. Per un utente che lavora con la tastiera, questo comportamento sarà al meglio confuso.

Vogliamo anche aggiungere alcune funzionalità di usabilità, come disabilitare il pulsante _Save_ quando i campi richiesti sono vuoti, dare focus a determinati elementi HTML o selezionare automaticamente i contenuti quando un input di testo riceve focus.

Per implementare tutte queste funzionalità, avremo bisogno di accesso programmatico ai nodi DOM per eseguire funzioni come [`focus()`](/it/docs/Web/API/HTMLElement/focus) e [`select()`](/it/docs/Web/API/HTMLInputElement/select). Dovremo anche usare [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) e [`removeEventListener()`](/it/docs/Web/API/EventTarget/removeEventListener) per eseguire compiti specifici quando il controllo riceve il focus.

Il problema è che tutti questi nodi DOM sono creati dinamicamente da Svelte in fase di runtime. Quindi dovremo aspettare che vengano creati e aggiunti al DOM per poterli usare. Per farlo, dovremo imparare sul [ciclo di vita del componente](https://learn.svelte.dev/tutorial/onmount) per capire quando possiamo accedervi — più su questo più tardi.

## Creare un componente NewTodo

Iniziamo estraendo il nostro modulo nuovo da fare nel proprio componente. Con quello che sappiamo finora possiamo creare un nuovo file componente e adattare il codice per emettere un evento `addTodo`, passando il nome del nuovo compito con i dettagli aggiuntivi.

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

   Qui stiamo associando l'`<input>` alla variabile `name` con `bind:value={name}` e disabilitando il pulsante _Add_ quando è vuoto (cioè, senza contenuto di testo) usando `disabled={!name}`. Gestiamo anche il tasto <kbd>Escape</kbd> con `on:keydown={(e) => e.key === 'Escape' && onCancel()}}`. Ogni volta che il tasto <kbd>Escape</kbd> viene premuto, eseguiamo `onCancel()`, che semplicemente pulisce la variabile `name`.

3. Ora dobbiamo `importare` e utilizzarlo da dentro il componente `Todos`, e aggiornare la funzione `addTodo()` per ricevere il nome del nuovo compito.

   Aggiungi la seguente dichiarazione di importazione sotto le altre in `Todos.svelte`:

   ```js
   import NewTodo from "./NewTodo.svelte";
   ```

4. Aggiorna la funzione `addTodo()` così:

   ```js
   function addTodo(name) {
     todos = [...todos, { id: newTodoId, name, completed: false }];
   }
   ```

   `addTodo()` ora riceve il nome del nuovo compito direttamente, quindi non abbiamo più bisogno della variabile `newTodoName` per dargli il suo valore. Il nostro componente `NewTodo` si occupa di quello.

   > [!NOTE]
   > La sintassi `{ name }` è solo una scorciatoia per `{ name: name }`. Questa viene da JavaScript stesso e non ha nulla a che fare con Svelte, oltre a fornire un po' di ispirazione per le scorciatoie proprie di Svelte.

5. Infine, per questa sezione, sostituisci il markup del modulo NewTodo con una chiamata al componente `NewTodo`, così:

   ```svelte
   <!-- NewTodo -->
   <NewTodo on:addTodo={(e) => addTodo(e.detail)} />
   ```

## Lavorare con i nodi DOM usando la direttiva `bind:this={dom_node}`

Ora vogliamo che l'elemento `<input>` del componente `NewTodo` riacquisti il focus ogni volta che il pulsante _Add_ viene premuto. Per questo avremo bisogno di un riferimento al nodo DOM dell'input. Svelte fornisce un modo per fare questo con la direttiva `bind:this={dom_node}`. Quando specificato, appena il componente è montato e il nodo DOM è creato, Svelte assegna un riferimento al nodo DOM alla variabile specificata.

Creeremo una variabile `nameEl` e la legheremo all'input usando `bind:this={nameEl}`. Poi all'interno di `addTodo()`, dopo aver aggiunto il nuovo compito chiameremo `nameEl.focus()` per rifocalizzare di nuovo sul `<input>`. Faremo lo stesso quando l'utente preme il tasto <kbd>Escape</kbd>, con la funzione `onCancel()`.

Aggiorna i contenuti di `NewTodo.svelte` così:

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

Prova l'app: digita il nome di un nuovo compito nel campo `<input>`, premi <kbd>tab</kbd> per dare focus al pulsante _Add_, e poi premi <kbd>Enter</kbd> o <kbd>Escape</kbd> per vedere come l'input recupera il focus.

### Mettere a fuoco automaticamente il nostro input

La prossima funzionalità che aggiungeremo al nostro componente `NewTodo` sarà una prop `autofocus`, che ci permetterà di specificare che vogliamo che il campo `<input>` sia focalizzato al caricamento della pagina.

1. Il nostro primo tentativo è il seguente: proviamo ad aggiungere la prop `autofocus` e semplicemente chiamare `nameEl.focus()` dal blocco `<script>`. Aggiorna la prima parte della sezione `<script>` di `NewTodo.svelte` (i primi quattro righe) per sembrare così:

   ```svelte
   <script>
     import { createEventDispatcher } from 'svelte';
     const dispatch = createEventDispatcher();

     export let autofocus = false;

     let name = '';
     let nameEl; // reference to the name input DOM node

     if (autofocus) nameEl.focus();
   ```

2. Ora torna al componente `Todos` e passa la prop `autofocus` alla chiamata al componente `<NewTodo>`, così:

   ```svelte
   <!-- NewTodo -->
   <NewTodo autofocus on:addTodo={(e) => addTodo(e.detail)} />
   ```

3. Se provi ora la tua app, vedrai che la pagina è ora vuota, e nella tua console di strumenti di sviluppo vedrai un errore del tipo: `TypeError: nameEl is undefined`.

Per capire cosa sta succedendo qui, parliamo un po' di più di quel [ciclo di vita del componente](https://learn.svelte.dev/tutorial/onmount) che abbiamo menzionato prima.

## Ciclo di vita del componente, e la funzione `onMount()`

Quando un componente viene istanziato, Svelte esegue il codice di inizializzazione (cioè la sezione `<script>` del componente). Ma in quel momento, tutti i nodi che compongono il componente non sono ancora attaccati al DOM, infatti, non esistono nemmeno.

Quindi, come puoi sapere quando il componente è stato creato e montato sul DOM? La risposta è che ogni componente ha un ciclo di vita che inizia quando viene creato e termina quando viene distrutto. Ci sono un certo numero di funzioni che ci permettono di eseguire codice in momenti chiave durante quel ciclo di vita.

Quella che userai più frequentemente è `onMount()`, che ci permette di eseguire una callback non appena il componente è stato montato sul DOM. Proviamo e vediamo cosa succede alla variabile `nameEl`.

1. Per iniziare, aggiungi la seguente riga all'inizio della sezione `<script>` di `NewTodo.svelte`:

   ```js
   import { onMount } from "svelte";
   ```

2. E queste righe alla fine di essa:

   ```js
   console.log("initializing:", nameEl);
   onMount(() => {
     console.log("mounted:", nameEl);
   });
   ```

3. Ora rimuovi la riga `if (autofocus) nameEl.focus()` per evitare di lanciare l'errore che stavamo vedendo prima.
4. L'app ora funzionerà di nuovo, e vedrai il seguente output nella tua console:

   ```plain
   initializing: undefined
   mounted: <input id="todo-0" class="input input__lg" type="text" autocomplete="off">
   ```

   Come puoi vedere, mentre il componente si sta inizializzando, `nameEl` è undefined, il che ha senso perché il nodo `<input>` non esiste nemmeno ancora. Dopo che il componente è stato montato, Svelte assegna un riferimento al nodo DOM `<input>` alla variabile `nameEl`, grazie alla direttiva `bind:this={nameEl}`.

5. Per far funzionare la funzionalità di autofocus, sostituisci il precedente blocco `console.log()`/`onMount()` che hai aggiunto con questo:

   ```js
   onMount(() => autofocus && nameEl.focus()); // if autofocus is true, we run nameEl.focus()
   ```

6. Vai di nuovo alla tua app, e ora vedrai che il campo `<input>` è focalizzato al caricamento della pagina.

> [!NOTE]
> Puoi dare un'occhiata alle altre [funzioni di ciclo di vita nella documentazione di Svelte](https://svelte.dev/docs/svelte), e puoi vederle in azione nel [tutorial interattivo](https://learn.svelte.dev/tutorial/onmount).

## Attendere che il DOM sia aggiornato con la funzione `tick()`

Ora ci occuperemo dei dettagli della gestione del focus del componente `Todo`. Prima di tutto, vogliamo che un `<input>` in modalità modifica del componente `Todo` riceva il focus quando entriamo nella modalità di modifica premendo il suo pulsante _Edit_. Nello stesso modo di quanto visto in precedenza, creeremo una variabile `nameEl` all'interno di `Todo.svelte` e chiameremo `nameEl.focus()` dopo aver impostato la variabile `editing` a `true`.

1. Apri il file `components/Todo.svelte` e aggiungi una dichiarazione della variabile `nameEl` subito sotto le tue dichiarazioni di editing e name:

   ```js
   let nameEl; // reference to the name input DOM node
   ```

2. Ora aggiorna la tua funzione `onEdit()` così:

   ```js
   function onEdit() {
     editing = true; // enter editing mode
     nameEl.focus(); // set focus to name input
   }
   ```

3. E infine lega `nameEl` al campo `<input>` aggiornandolo così:

   ```svelte
   <input
     bind:value={name}
     bind:this={nameEl}
     type="text"
     id="todo-{todo.id}"
     autocomplete="off"
     class="todo-text" />
   ```

4. Tuttavia, quando provi l'app aggiornata, riceverai un errore del tipo "TypeError: nameEl is undefined" nella console quando premi il pulsante _Edit_ di un compito.

Quindi, cosa sta succedendo qui? Quando aggiorni lo stato di un componente in Svelte, non aggiorna il DOM immediatamente. Invece, aspetta fino al prossimo microtask per vedere se ci sono altri cambiamenti che devono essere applicati, inclusi altri componenti. Facendo così evita lavori inutili e permette al browser di raggruppare le cose in modo più efficace.

In questo caso, quando `editing` è `false`, il `<input>` di modifica non è visibile perché non esiste nel DOM. All'interno della funzione `onEdit()` impostiamo `editing = true` e immediatamente dopo cerchiamo di accedere alla variabile `nameEl` ed eseguire `nameEl.focus()`. Il problema qui è che Svelte non ha ancora aggiornato il DOM.

Un modo per risolvere questo problema è usare [`setTimeout()`](/it/docs/Web/API/Window/setTimeout) per ritardare la chiamata a `nameEl.focus()` fino al prossimo ciclo di eventi, e dare a Svelte l'opportunità di aggiornare il DOM.

Provalo ora:

```js
function onEdit() {
  editing = true; // enter editing mode
  setTimeout(() => nameEl.focus(), 0); // asynchronous call to set focus to name input
}
```

La soluzione sopra funziona, ma è piuttosto inelegante. Svelte fornisce un modo migliore per gestire questi casi. La funzione [`tick()`](https://learn.svelte.dev/tutorial/tick) restituisce una promessa che viene risolta non appena le modifiche di stato in sospeso sono state applicate al DOM (o immediatamente, se non ci sono cambiamenti di stato in sospeso). Proviamo ora.

1. Per prima cosa, importa `tick` all'inizio della sezione `<script>` accanto al tuo import esistente:

   ```js
   import { tick } from "svelte";
   ```

2. Successivamente, chiama `tick()` con [`await`](/it/docs/Web/JavaScript/Reference/Operators/await) da una [funzione asincrona](/it/docs/Web/JavaScript/Reference/Statements/async_function); aggiorna `onEdit()` così:

   ```js
   async function onEdit() {
     editing = true; // enter editing mode
     await tick();
     nameEl.focus();
   }
   ```

3. Se lo provi ora vedrai che tutto funziona come previsto.

> [!NOTE]
> Per vedere un altro esempio usando `tick()`, visita il [tutorial di Svelte](https://learn.svelte.dev/tutorial/tick).

## Aggiungere funzionalità agli elementi HTML con la direttiva `use:action`

Il prossimo passo è che vogliamo che l`'<input>` per il nome selezioni automaticamente tutto il testo al seguente. Inoltre, vogliamo sviluppare ciò in modo che possa essere facilmente riutilizzato su qualsiasi `<input>` HTML e applicato in modo dichiarativo. Utilizzeremo questo requisito come un pretesto per mostrare una funzione molto potente che Svelte ci fornisce per aggiungere funzionalità a elementi HTML regolari: [azioni](https://svelte.dev/docs/svelte-action).

Per selezionare il testo di un nodo input DOM, dobbiamo chiamare [`select()`](/it/docs/Web/API/HTMLInputElement/select). Per far sì che questa funzione venga chiamata ogni volta che il nodo riceve il focus, abbiamo bisogno di un listener per eventi come il seguente:

```js
node.addEventListener("focus", (event) => node.select());
```

E, per evitare perdite di memoria, dovremmo anche chiamare [`removeEventListener()`](/it/docs/Web/API/EventTarget/removeEventListener) quando il nodo viene distrutto.

> [!NOTE]
> Tutto questo è solo funzionalità standard del WebAPI; niente qui è specifico per Svelte.

Potremmo ottenere tutto questo nel nostro componente `Todo` ogni volta che aggiungiamo o rimuoviamo l'`<input>` dal DOM, ma dovremmo essere molto attenti ad aggiungere il listener dell'evento dopo che il nodo è stato aggiunto al DOM, e rimuovere il listener prima che il nodo venga rimosso dal DOM. Inoltre, la nostra soluzione non sarebbe molto riutilizzabile.

Ecco dove entrano in gioco le azioni di Svelte. Fondamentalmente ci permettono di eseguire una funzione ogni volta che un elemento è stato aggiunto al DOM, e dopo la rimozione dal DOM.

Nel nostro caso immediato, definiremo una funzione chiamata `selectOnFocus()` che riceverà un nodo come parametro. La funzione aggiungerà un listener di eventi a quel nodo in modo che ogni volta che riceve il focus selezionerà il testo. Quindi restituirà un oggetto con una proprietà `destroy`. La proprietà `destroy` è ciò che Svelte eseguirà dopo aver rimosso il nodo dal DOM. Qui rimuoveremo il listener per assicurarci di non lasciare alcuna perdita di memoria.

1. Creiamo la funzione `selectOnFocus()`. Aggiungi il seguente alla fine della sezione `<script>` di `Todo.svelte`:

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

2. Ora dobbiamo dire all'`<input>` di usare quella funzione con la direttiva [`use:action`](https://svelte.dev/docs/element-directives#use-action):

   ```svelte
   <input use:selectOnFocus />
   ```

   Con questa direttiva stiamo dicendo a Svelte di eseguire questa funzione, passando il nodo DOM dell'`<input>` come parametro, non appena il componente è montato sul DOM. Sarà anche incaricato di eseguire la funzione `destroy` quando il componente viene rimosso dal DOM. Quindi, con la direttiva `use`, Svelte si occupa del ciclo di vita del componente per noi.

   Nel nostro caso, il nostro `<input>` finirebbe per sembrare così: aggiorna la prima coppia label/input (all'interno del template di modifica) del componente come segue:

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

3. Proviamolo. Vai nella tua app, premi il pulsante _Edit_ di un compito, quindi sposta il focus dal `<input>` usando <kbd>Tab</kbd>. Ora fai clic sull'`<input>`, e vedrai che l'intero testo dell'input viene selezionato.

### Rendere l'azione riutilizzabile

Ora rendiamo questa funzione veramente riutilizzabile in diversi componenti. `selectOnFocus()` è solo una funzione senza dipendenze specifiche sul componente `Todo.svelte`, quindi possiamo semplicemente estrarla in un file e usarla da lì.

1. Crea un nuovo file, `actions.js`, nella cartella `src`.
2. Dagli il seguente contenuto:

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

3. Ora importalo da dentro `Todo.svelte`; aggiungi la seguente dichiarazione di importazione subito sotto le altre:

   ```js
   import { selectOnFocus } from "../actions.js";
   ```

4. E rimuovi la definizione `selectOnFocus()` da `Todo.svelte`, dato che non ne abbiamo più bisogno lì.

### Riutilizzare la nostra azione

Per dimostrare la riutilizzabilità della nostra azione, usiamola in `NewTodo.svelte`.

1. Importa `selectOnFocus()` da `actions.js` in questo file anche qui, come sopra:

   ```js
   import { selectOnFocus } from "../actions.js";
   ```

2. Aggiungi la direttiva `use:selectOnFocus` al `<input>`, così:

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

Con poche righe di codice possiamo aggiungere funzionalità agli elementi HTML regolari in un modo molto riutilizzabile e dichiarativo. È sufficiente un `import` e una breve direttiva come `use:selectOnFocus` che descrive chiaramente il suo scopo. E possiamo ottenere tutto questo senza la necessità di creare un elemento wrapper personalizzato come `TextInput`, `MyInput`, o simili. Inoltre, puoi aggiungere tutte le direttive `use:action` che desideri a un elemento.

Inoltre, non abbiamo dovuto lottare con `onMount()`, `onDestroy()`, o `tick()` — la direttiva `use` si occupa del ciclo di vita del componente per noi.

### Altri miglioramenti alle azioni

Nella sezione precedente, lavorando con i componenti `Todo`, abbiamo dovuto gestire `bind:this`, `tick()`, e funzioni `async` solo per dare focus al nostro `<input>` non appena è stato aggiunto al DOM.

1. Ecco come possiamo implementarlo invece con le azioni:

   ```js
   const focusOnInit = (node) =>
     node && typeof node.focus === "function" && node.focus();
   ```

2. E poi nel nostro markup dobbiamo solo aggiungere un'altra direttiva `use:`:

   ```svelte
   <input bind:value={name} use:selectOnFocus use:focusOnInit />
   ```

3. La nostra funzione `onEdit()` può ora essere molto più semplice:

   ```js
   function onEdit() {
     editing = true; // enter editing mode
   }
   ```

Come ultimo esempio prima di procedere, torniamo al nostro componente `Todo.svelte` e diamo il focus al pulsante _Edit_ dopo che l'utente ha premuto _Save_ o _Cancel_.

Potremmo provare semplicemente a riutilizzare di nuovo la nostra azione `focusOnInit`, aggiungendo `use:focusOnInit` al pulsante _Edit_. Ma introdurremmo un bug sottile. Quando aggiungi un nuovo compito, il focus sarà messo sul pulsante _Edit_ del compito appena aggiunto. Questo perché l'azione `focusOnInit` viene eseguita quando il componente viene creato.

Non è quello che vogliamo — vogliamo che il pulsante _Edit_ riceva il focus solo quando l'utente ha premuto _Save_ o _Cancel_.

1. Quindi, torna al tuo file `Todo.svelte`.
2. Per prima cosa creeremo un flag chiamato `editButtonPressed` e lo inizializzeremo a `false`. Aggiungi questo subito sotto le altre dichiarazioni di variabile:

   ```js
   let editButtonPressed = false; // track if edit button has been pressed, to give focus to it after cancel or save
   ```

3. Successivamente modificheremo la funzionalità del pulsante _Edit_ per salvare questo flag, e creeremo l'azione per esso. Aggiorna la funzione `onEdit()` così:

   ```js
   function onEdit() {
     editButtonPressed = true; // user pressed the Edit button, focus will come back to the Edit button
     editing = true; // enter editing mode
   }
   ```

4. Sotto di esso, aggiungi la seguente definizione per `focusEditButton()`:

   ```js
   const focusEditButton = (node) => editButtonPressed && node.focus();
   ```

5. Infine usiamo l'azione `focusEditButton` nell

pulsante _Edit_, così:

```svelte
<button type="button" class="btn" on:click={onEdit} use:focusEditButton>
  Edit<span class="visually-hidden"> {todo.name}</span>
</button>
```

6. Torna e prova di nuovo la tua app. A questo punto, ogni volta che il pulsante _Edit_ viene aggiunto al DOM, l'azione `focusEditButton` viene eseguita, ma darà il focus al pulsante solo se il flag `editButtonPressed` è `true`.

> [!NOTE]
> Abbiamo appena graffiato la superficie delle azioni qui. Le azioni possono anche avere parametri reattivi, e Svelte ci consente di rilevare quando uno qualsiasi di questi parametri cambia. In questo modo possiamo aggiungere funzionalità che si integrano bene con il sistema reattivo di Svelte. Per un'introduzione più dettagliata alle azioni, considera di dare un'occhiata al [tutorial interattivo di Svelte](https://learn.svelte.dev/tutorial/actions) o alla [documentazione di Svelte `use:action`](https://svelte.dev/docs/element-directives#use-action).

## Binding del componente: esporre metodi e variabili del componente usando la direttiva `bind:this={component}`

C'è ancora un fastidio di accessibilità rimasto. Quando l'utente preme il pulsante _Delete_, il focus svanisce.

L'ultima funzionalità che esamineremo in questo articolo comporta il posizionamento del focus sull'intestazione di stato dopo che un compito è stato eliminato.

Perché l'intestazione di stato? In questo caso, l'elemento che aveva il focus è stato eliminato, quindi non c'è un candidato chiaro a ricevere il focus. Abbiamo scelto l'intestazione di stato perché è vicino all'elenco di cose da fare, ed è un modo per dare un feedback visivo sull'eliminazione del compito, oltre a indicare cosa è successo agli utenti con lettori di schermo.

Innanzitutto estrarremo l';intestazione di stato nel suo componente.

1. Crea un nuovo file, `components/TodosStatus.svelte`.
2. Aggiungi i seguenti contenuti:

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

4. Sostituisci l'intestazione `<h2>` stato all'interno di `Todos.svelte` con una chiamata al componente `TodosStatus`, passando `todos` ad esso come prop, così:

   ```svelte
   <TodosStatus {todos} />
   ```

5. Puoi anche fare un po' di pulizia, rimuovendo le variabili `totalTodos` e `completedTodos` da `Todos.svelte`. Rimuovi solo le righe `$: totalTodos = …` e `$: completedTodos = …`, e anche rimuovi il riferimento a `totalTodos` quando calcoliamo `newTodoId` e usa invece `todos.length`. Per fare ciò, sostituisci il blocco che inizia con `let newTodoId` con questo:

   ```js
   $: newTodoId = todos.length ? Math.max(...todos.map((t) => t.id)) + 1 : 1;
   ```

6. Tutto funziona come previsto — abbiamo semplicemente estratto l'ultimo pezzo di markup nel suo componente.

Ora dobbiamo trovare un modo per dare focus all'intestazione `<h2>` dello stato dopo che un compito è stato eliminato.

Finora abbiamo visto come inviare informazioni a un componente tramite prop, e come un componente può comunicare con il suo genitore emettendo eventi o usando binding a due vie. Il componente figlio potrebbe ottenere un riferimento al nodo `<h2>` `usando bind:this={dom_node}` ed esporlo all'esterno usando il binding a due vie. Ma facendo così si romperebbe l'incapsulamento del componente; mettere a fuoco su di esso dovrebbe essere sua responsabilità.

Quindi abbiamo bisogno che il componente `TodosStatus` esponga un metodo che il suo genitore possa chiamare per dare focus a esso. È uno scenario molto comune che un componente debba esporre un comportamento o informazioni al consumatore; vediamo come ottenerlo con Svelte.

Abbiamo già visto che Svelte usa `export let varname = …` per [dichiarare le prop](https://svelte.dev/docs/svelte-components#script-1-export-creates-a-component-prop). Ma se invece di usare `let` esporti un `const`, `class` o `function`, è di sola lettura all'esterno del componente. Le espressioni di funzione sono valide prop, tuttavia. Nell'esempio seguente, le prime tre dichiarazioni sono prop, e il resto sono valori esportati:

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

Con ciò in mente, torniamo al nostro caso d'uso. Creeremo una funzione chiamata `focus()` che dà focus all'intestazione `<h2>`. Per farlo avremo bisogno di una variabile `headingEl` per tenere il riferimento al nodo DOM, e dovremo associarla all'elemento `<h2>` usando `bind:this={headingEl}`. Il nostro metodo focus eseguirà semplicemente `headingEl.focus()`.

1. Aggiorna i contenuti di `TodosStatus.svelte` così:

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

   Nota che abbiamo aggiunto un attributo `tabindex` all`<h2>` per consentire all';elemento di ricevere programmaticamente la messa a fuoco.

   Come abbiamo visto in precedenza, usando la direttiva `bind:this={headingEl}` otteniamo un riferimento al nodo DOM nella variabile `headingEl`. Poi utilizziamo `export function focus()` per esporre una funzione che dà focus all'intestazione `<h2>`.

   Come possiamo accedere a quei valori esportati dal genitore? Così come puoi associare agli elementi DOM con la direttiva `bind:this={dom_node}`, puoi anche associare alle istanze dei componenti stessi con `bind:this={component}`. Quindi, quando usi `bind:this` su un elemento HTML, ottieni un riferimento al nodo DOM, e quando lo fai su un componente Svelte, ottieni un riferimento all'istanza di quel componente.

2. Quindi per collegarsi all'istanza di `TodosStatus`, creeremo prima una variabile `todosStatus` in `Todos.svelte`. Aggiungi la seguente riga sotto le tue dichiarazioni di importazione:

   ```js
   let todosStatus; // reference to TodosStatus instance
   ```

3. Successivamente, aggiungi una direttiva `bind:this={todosStatus}` alla chiamata, come segue:

   ```svelte
   <!-- TodosStatus -->
   <TodosStatus bind:this={todosStatus} {todos} />
   ```

4. Ora possiamo chiamare il metodo focus esportato dalla nostra funzione `removeTodo()`:

   ```js
   function removeTodo(todo) {
     todos = todos.filter((t) => t.id !== todo.id);
     todosStatus.focus(); // give focus to status heading
   }
   ```

5. Torna alla tua app. Ora se elimini un qualsiasi compito, l'intestazione di stato sarà focalizzata. Questo è utile per evidenziare il cambiamento nei numeri di cose da fare, sia per gli utenti vedenti che per quelli che usano un lettore di schermo.

> [!NOTE]
> Potresti chiederti perché dobbiamo dichiarare una nuova variabile per il binding del componente. Perché non possiamo semplicemente chiamare `TodosStatus.focus()`? Potresti avere più istanze `TodosStatus` attive, quindi hai bisogno di un modo per fare riferimento a ciascuna istanza particolare. Ecco perché devi specificare una variabile a cui legare ciascuna specifica istanza.

## Il codice finora

### Git

Per vedere lo stato del codice come dovrebbe essere alla fine di questo articolo, accedi alla tua copia del nostro repo così:

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

In questo articolo abbiamo finito di aggiungere tutte le funzionalità richieste alla nostra app, oltre a occuparci di una serie di problemi di accessibilità e usabilità. Abbiamo anche finito di suddividere la nostra app in componenti gestibili, ciascuno con una responsabilità unica.

Nel frattempo, abbiamo visto alcune tecniche avanzate di Svelte, come:

- Affrontare le insidie di reattività quando si aggiornano oggetti e array
- Lavorare con i nodi DOM usando `bind:this={dom_node}` (binding a elementi DOM)
- Usare la funzione del ciclo di vita del componente `onMount()`
- Forzare Svelte a risolvere i cambiamenti di stato in sospeso con la funzione `tick()`
- Aggiungere funzionalità agli elementi HTML in un modo riutilizzabile e dichiarativo con la direttiva `use:action`
- Accedere ai metodi dei componenti usando `bind:this={component}` (binding a componenti)

Nel prossimo articolo vedremo come usare gli store per comunicare tra i componenti e aggiungere animazioni ai nostri componenti.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_components","Learn_web_development/Core/Frameworks_libraries/Svelte_stores", "Learn_web_development/Core/Frameworks_libraries")}}
