---
title: Consigli per la creazione di componenti nella nostra app Svelte
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_components
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_variables_props","Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility", "Learn_web_development/Core/Frameworks_libraries")}}

Nell'ultimo articolo abbiamo iniziato a sviluppare la nostra app per la lista di attività. L'obiettivo centrale di questo articolo è vedere come suddividere la nostra app in componenti gestibili e condividere informazioni tra di essi. Componentizzeremo la nostra app, quindi aggiungeremo più funzionalità per permettere agli utenti di aggiornare i componenti esistenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Si consiglia di avere familiarità con i linguaggi di base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e di avere conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/linea di comando</a
          >.
        </p>
        <p>
          Avrai bisogno di un terminale con node e npm installati per compilare e costruire la tua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare come dividere la nostra app in componenti e condividere informazioni tra di essi.
      </td>
    </tr>
  </tbody>
</table>

## Codifica con noi

### Git

Clona il repository GitHub (se non lo hai già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Quindi per accedere allo stato corrente dell'app, esegui

```bash
cd mdn-svelte-tutorial/04-componentizing-our-app
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/04-componentizing-our-app
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità sviluppo.

### REPL

Per programmare insieme a noi utilizzando il REPL, inizia su

<https://svelte.dev/repl/99b9eb228b404a2f8c8959b22c0a40d3?version=3.23.2>

## Suddividere l'app in componenti

In Svelte, un'applicazione è composta da uno o più componenti. Un componente è un blocco di codice riutilizzabile e autonomo che incapsula HTML, CSS e JavaScript che appartengono insieme, scritto in un file `.svelte`. I componenti possono essere grandi o piccoli, ma di solito sono chiaramente definiti: i componenti più efficaci servono uno scopo singolo e ovvio.

I benefici di definire componenti sono simili alla pratica migliore più generale di organizzare il tuo codice in parti gestibili. Aiuterà a capire come si relazionano tra di loro, promuoverà il riutilizzo e renderà il tuo codice più facile da comprendere, mantenere ed estendere.

Ma come si sa cosa dovrebbe essere suddiviso in un proprio componente?

Non ci sono regole rigide per questo. Alcune persone preferiscono un approccio intuitivo e iniziano a osservare il markup e tracciando riquadri attorno a ogni componente e sottocomponente che sembra avere una propria logica.

Altre persone applicano le stesse tecniche utilizzate per decidere se dovresti creare una nuova funzione o oggetto. Una di queste tecniche è il principio di responsabilità singola — cioè, un componente dovrebbe idealmente fare solo una cosa. Se cresce troppo, dovrebbe essere suddiviso in sottocomponenti più piccoli.

Entrambi gli approcci dovrebbero completarsi a vicenda e aiutarti a decidere come organizzare meglio i tuoi componenti.

Alla fine, suddivideremo la nostra app nei seguenti componenti:

- `Alert.svelte`: Un box di notifica generale per comunicare le azioni effettuate.
- `NewTodo.svelte`: L'input di testo e il pulsante che ti consentono di inserire un nuovo elemento della lista di attività.
- `FilterButton.svelte`: I pulsanti _All_, _Active_ e _Completed_ che ti consentono di applicare filtri agli elementi visualizzati della lista di attività.
- `TodosStatus.svelte`: L'intestazione "x su y elementi completati".
- `Todo.svelte`: Un singolo elemento della lista di attività. Ogni elemento visibile della lista sarà visualizzato in una copia separata di questo componente.
- `MoreActions.svelte`: I pulsanti _Check All_ e _Remove Completed_ nella parte inferiore dell'interfaccia utente che ti consentono di eseguire azioni di massa sugli elementi della lista di attività.

![rappresentazione grafica dell'elenco dei componenti nella nostra app](01-todo-components.png)

In questo articolo ci concentreremo sulla creazione dei componenti `FilterButton` e `Todo`; affronteremo gli altri nei prossimi articoli.

Iniziamo.

> [!NOTE]
> Durante il processo di creazione dei nostri primi componenti, impareremo anche diverse tecniche per comunicare tra i componenti e i pro e i contro di ciascuna.

## Estrarre il nostro componente di filtro

Inizieremo creando il nostro `FilterButton.svelte`.

1. Prima di tutto, crea un nuovo file, `components/FilterButton.svelte`.
2. All'interno di questo file, dichiareremo una prop `filter`, quindi copieremo il markup pertinente da `Todos.svelte`. Aggiungi il seguente contenuto nel file:

   ```svelte
   <script>
     export let filter = 'all'
   </script>

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

3. Nel nostro componente `Todos.svelte`, vogliamo utilizzare il nostro componente `FilterButton`. Per prima cosa, dobbiamo importarlo. Aggiungi la seguente riga all'inizio della sezione `<script>` di `Todos.svelte`:

   ```js
   import FilterButton from "./FilterButton.svelte";
   ```

4. Ora sostituisci l'elemento `<div class="filters...` con una chiamata al componente `FilterButton`, che prende il filtro corrente come prop. La seguente riga di codice è tutto ciò di cui hai bisogno:

   ```svelte
   <FilterButton {filter} />
   ```

> [!NOTE]
> Ricorda che quando il nome dell'attributo HTML e la variabile coincidono, possono essere sostituiti con `{variabile}`. Ecco perché abbiamo potuto sostituire `<FilterButton filter={filter} />` con `<FilterButton {filter} />`.

Finora tutto bene! Proviamo ora l'app. Noterai che quando fai clic sui pulsanti di filtro, vengono selezionati e lo stile si aggiorna in modo appropriato. Ma abbiamo un problema: le attività non vengono filtrate. Questo perché la variabile `filter` fluisce dal componente `Todos` al componente `FilterButton` attraverso la prop, ma le modifiche che si verificano nel componente `FilterButton` non ritornano al suo genitore — il binding dei dati è unidirezionale per impostazione predefinita. Vediamo un modo per risolvere questo problema.

## Condividere dati tra componenti: passare un gestore come una prop

Un modo per consentire ai componenti figlio di notificare ai loro genitori eventuali modifiche è passare un gestore come prop. Il componente figlio eseguirà il gestore, passando le informazioni necessarie come parametro, e il gestore modificherà lo stato del genitore.

Nel nostro caso, il componente `FilterButton` riceverà un gestore `onclick` dal suo genitore. Ogni volta che l'utente fa clic su un qualsiasi pulsante di filtro, il figlio chiamerà il gestore `onclick`, passando il filtro selezionato come parametro al suo genitore.

Dichiareremo semplicemente la prop `onclick` assegnando un gestore fittizio per evitare errori, in questo modo:

```js
export let onclick = (clicked) => {};
```

E dichiareremo l'istruzione reattiva `$: onclick(filter)` per chiamare il gestore `onclick` ogni volta che la variabile `filter` viene aggiornata.

1. La sezione `<script>` del nostro componente `FilterButton` dovrebbe finire per assomigliare a questa. Aggiorna ora il tuo:

   ```js
   export let filter = "all";
   export let onclick = (clicked) => {};
   $: onclick(filter);
   ```

2. Ora, quando chiamiamo `FilterButton` all'interno di `Todos.svelte`, dovremo specificare il gestore. Aggiornalo in questo modo:

   ```svelte
   <FilterButton {filter} onclick={ (clicked) => filter = clicked }/>
   ```

Quando un qualsiasi pulsante di filtro viene cliccato, aggiorniamo semplicemente la variabile filter con il nuovo filtro. Ora il nostro componente `FilterButton` funzionerà di nuovo.

## Binding bidirezionale più semplice con la direttiva bind

Nell'esempio precedente abbiamo realizzato che il nostro componente `FilterButton` non funzionava perché lo stato della nostra applicazione scorreva dal genitore al figlio tramite la prop `filter`, ma non ritornava. Così abbiamo aggiunto una prop `onclick` per consentire al componente figlio di comunicare il nuovo valore `filter` al suo genitore.

Funziona bene, ma Svelte ci fornisce un modo più semplice e diretto per ottenere il binding bidirezionale dei dati. I dati normalmente scorrono dal genitore al figlio utilizzando le prop. Se vogliamo che scorrano anche nella direzione opposta, dal figlio al genitore, possiamo usare [la direttiva `bind:`](https://svelte.dev/docs/element-directives#bind-property).

Utilizzando `bind`, diremo a Svelte che qualsiasi modifica apportata alla prop `filter` nel componente `FilterButton` dovrebbe propagarsi al componente genitore, `Todos`. In altre parole, legheremo il valore della variabile `filter` nel genitore al suo valore nel figlio.

1. In `Todos.svelte`, aggiorna la chiamata al componente `FilterButton` come segue:

   ```svelte
   <FilterButton bind:filter={filter} />
   ```

   Come al solito, Svelte ci fornisce una comoda abbreviazione: `bind:value={value}` è equivalente a `bind:value`. Quindi, nell'esempio sopra, potresti semplicemente scrivere `<FilterButton bind:filter />`.

2. Il componente figlio può ora modificare il valore della variabile filtro del genitore, quindi non abbiamo più bisogno della prop `onclick`. Modifica l'elemento `<script>` del tuo `FilterButton` in questo modo:

   ```svelte
   <script>
     export let filter = "all";
   </script>
   ```

3. Prova nuovamente la tua app e dovresti vedere che i filtri funzionano correttamente.

## Creare il nostro componente Todo

Ora creeremo un componente `Todo` per incapsulare ciascun elemento della lista di attività individuale, incluso il checkbox e una logica di modifica per permetterti di cambiare un'attività esistente.

Il nostro componente `Todo` riceverà un singolo oggetto `todo` come prop. Dichiareremo la prop `todo` e sposteremo il codice dal componente `Todos`. Per ora, sostituiremo la chiamata a `removeTodo` con un alert. Aggiungeremo di nuovo questa funzionalità più avanti.

1. Crea un nuovo file di componenti, `components/Todo.svelte`.
2. Inserisci i seguenti contenuti all'interno di questo file:

   ```svelte
   <script>
     export let todo
   </script>

   <div class="stack-small">
     <div class="c-cb">
       <input type="checkbox" id="todo-{todo.id}"
         on:click={() => todo.completed = !todo.completed}
         checked={todo.completed}
       />
       <label for="todo-{todo.id}" class="todo-label">{todo.name}</label>
     </div>
     <div class="btn-group">
       <button type="button" class="btn">
         Edit <span class="visually-hidden">{todo.name}</span>
       </button>
       <button type="button" class="btn btn__danger" on:click={() => alert('not implemented')}>
         Delete <span class="visually-hidden">{todo.name}</span>
       </button>
     </div>
   </div>
   ```

3. Ora dobbiamo importare il nostro componente `Todo` in `Todos.svelte`. Vai a questo file ora e aggiungi la seguente istruzione `import` sotto la tua precedente:

   ```js
   import Todo from "./Todo.svelte";
   ```

4. Successivamente dobbiamo aggiornare il nostro blocco `{#each}` per includere un componente `<Todo>` per ogni attività, anziché il codice che è stato spostato in `Todo.svelte`. Stiamo anche passando l'oggetto `todo` corrente nel componente come una prop.

   Aggiorna il blocco `{#each}` all'interno di `Todos.svelte` in questo modo:

   ```svelte
   <ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
     {#each filterTodos(filter, todos) as todo (todo.id)}
     <li class="todo">
       <Todo {todo} />
     </li>
     {:else}
     <li>Nothing to do here!</li>
     {/each}
   </ul>
   ```

L'elenco delle attività è visualizzato sulla pagina e le checkbox dovrebbero funzionare (prova a selezionare/deselezionare alcune, e quindi osserva che i filtri funzionano ancora come previsto), ma la nostra intestazione di stato "x su y elementi completati" non si aggiornerà più di conseguenza. Questo perché il nostro componente `Todo` sta ricevendo l'attività tramite la prop, ma non sta inviando alcuna informazione al suo genitore. Lo risolveremo più avanti.

## Condividere dati tra componenti: schema props-down, events-up

La direttiva `bind` è abbastanza semplice e ti consente di condividere dati tra un componente genitore e figlio con il minimo sforzo. Tuttavia, quando la tua applicazione cresce e diventa più complessa, può essere difficile tenere traccia di tutti i tuoi valori legati. Un approccio diverso è lo schema di comunicazione "props-down, events-up".

In pratica, questo schema si basa sui componenti figli che ricevono dati dai loro genitori tramite le prop e i componenti genitori che aggiornano il loro stato gestendo gli eventi emessi dai componenti figli. Quindi le prop _fluiscono verso il basso_ dal genitore al figlio e gli eventi _risalgono_ dal figlio al genitore. Questo schema stabilisce un flusso bidirezionale di informazioni, che è prevedibile e più facile da ragionare.

Diamo un'occhiata a come emettere i nostri eventi personalizzati per reimplementare la funzionalità mancante del pulsante _Delete_.

Per creare eventi personalizzati, useremo l'utilità `createEventDispatcher`. Questo restituirà una funzione `dispatch()` che ci permetterà di emettere eventi personalizzati. Quando invii un evento, devi passare il nome dell'evento e, facoltativamente, un oggetto con informazioni aggiuntive che desideri passare a ogni ascoltatore. Questi dati aggiuntivi saranno disponibili sulla proprietà `detail` dell'oggetto evento.

> [!NOTE]
> Gli eventi personalizzati in Svelte condividono la stessa API degli eventi DOM regolari. Inoltre, puoi far risalire un evento al tuo componente genitore specificando `on:event` senza alcun gestore.

Modificheremo il nostro componente `Todo` per emettere un evento `remove`, passando l'attività da rimuovere come informazione aggiuntiva.

1. Prima di tutto, aggiungi le seguenti righe all'inizio della sezione `<script>` del componente `Todo`:

   ```js
   import { createEventDispatcher } from "svelte";
   const dispatch = createEventDispatcher();
   ```

2. Ora aggiorna il pulsante _Delete_ nella sezione markup dello stesso file in modo che assomigli a questo:

   ```svelte
   <button type="button" class="btn btn__danger" on:click={() => dispatch('remove', todo)}>
     Delete <span class="visually-hidden">{todo.name}</span>
   </button>
   ```

   Con `dispatch('remove', todo)` stiamo emettendo un evento `remove`, e passando come dati aggiuntivi l'attività che viene eliminata. Il gestore sarà chiamato con un oggetto evento disponibile, con i dati aggiuntivi disponibili nella proprietà `event.detail`.

3. Ora dobbiamo ascoltare quell'evento dall'interno di `Todos.svelte` e agire di conseguenza. Torna a questo file e aggiorna la tua chiamata al componente `<Todo>` in questo modo:

   ```svelte
   <Todo {todo} on:remove={(e) => removeTodo(e.detail)} />
   ```

   Il nostro gestore riceve il parametro `e` (l'oggetto evento), che come descritto in precedenza contiene l'attività da eliminare nella proprietà `detail`.

4. A questo punto, se provi di nuovo la tua app, dovresti vedere che la funzionalità _Delete_ ora funziona di nuovo. Quindi il nostro evento personalizzato ha funzionato come ci aspettavamo. Inoltre, il listener dell'evento `remove` sta inviando la modifica dei dati al genitore, quindi la nostra intestazione di stato "x su y elementi completati" ora si aggiornerà correttamente quando vengono eliminate delle attività.

Ora ci occuperemo dell'evento `update`, in modo che il nostro componente genitore possa essere notificato di qualsiasi modifica all'attività.

## Aggiornare le attività

Dobbiamo ancora implementare la funzionalità che ci permetta di modificare le attività esistenti. Dovremo includere una modalità di modifica nel componente `Todo`. Quando si entra in modalità modifica, mostreremo un campo `<input>` per consentirci di modificare il nome dell'attività corrente, con due pulsanti per confermare o annullare le modifiche.

### Gestire gli eventi

1. Avremo bisogno di una variabile per tenere traccia se siamo in modalità di modifica e un'altra per memorizzare il nome dell'attività che viene aggiornata. Aggiungi le seguenti definizioni di variabile alla fine della sezione `<script>` del componente `Todo`:

   ```js
   let editing = false; // track editing mode
   let name = todo.name; // hold the name of the to-do being edited
   ```

2. Dobbiamo decidere quali eventi il nostro componente `Todo` emetterà:

   - Potremmo emettere diversi eventi per il toggle dello stato e la modifica del nome (ad esempio, `updateTodoStatus` e `updateTodoName`).
   - Oppure potremmo adottare un approccio più generico ed emettere un singolo evento `update` per entrambe le operazioni.

   Adotteremo il secondo approccio in modo da poter dimostrare una tecnica diversa. Il vantaggio di questo approccio è che successivamente possiamo aggiungere più campi alle attività e ancora gestire tutti gli aggiornamenti con lo stesso evento.

   Creiamo una funzione `update()` che riceverà le modifiche ed emetterà un evento di aggiornamento con l'attività modificata. Aggiungi il seguente, ancora una volta, alla fine della sezione `<script>`:

   ```js
   function update(updatedTodo) {
     todo = { ...todo, ...updatedTodo }; // applies modifications to todo
     dispatch("update", todo); // emit update event
   }
   ```

   Qui stiamo utilizzando la [sintassi spread](/it/docs/Web/JavaScript/Reference/Operators/Spread_syntax) per restituire l'attività originale con le modifiche applicate.

3. Successivamente, creeremo diverse funzioni per gestire ciascuna azione dell'utente. Quando l'attività è in modalità modifica, l'utente può salvare o annullare le modifiche. Quando non è in modalità modifica, l'utente può eliminare l'attività, modificarla o attivare/disattivare il suo stato tra completata e attiva.

   Aggiungi il seguente set di funzioni sotto la tua funzione precedente per gestire queste azioni:

   ```js
   function onCancel() {
     name = todo.name; // restores name to its initial value and
     editing = false; // and exit editing mode
   }

   function onSave() {
     update({ name }); // updates todo name
     editing = false; // and exit editing mode
   }

   function onRemove() {
     dispatch("remove", todo); // emit remove event
   }

   function onEdit() {
     editing = true; // enter editing mode
   }

   function onToggle() {
     update({ completed: !todo.completed }); // updates todo status
   }
   ```

### Aggiornare il markup

Ora dobbiamo aggiornare il markup del nostro componente `Todo` per chiamare le funzioni sopra quando vengono intraprese le azioni appropriate.

Per gestire la modalità modifica, stiamo utilizzando la variabile `editing`, che è un booleano. Quando è `true`, dovrebbe visualizzare il campo `<input>` per modificare il nome dell'attività, e i pulsanti _Cancel_ e _Save_. Quando non è in modalità modifica, visualizzerà la checkbox, il nome dell'attività e i pulsanti per modificare ed eliminare l'attività.

Per ottenere questo utilizzeremo un [blocco `if`](https://svelte.dev/docs/logic-blocks#if). Il blocco `if` rende condizionale del markup. Tieni presente che non solo mostrerà o nasconderà il markup in base alla condizione — aggiungerà e rimuoverà dinamicamente gli elementi dal DOM, a seconda della condizione.

Quando `editing` è `true`, ad esempio, Svelte mostrerà il modulo di aggiornamento; quando è `false`, lo rimuoverà dal DOM e aggiungerà il checkbox. Grazie alla reattività di Svelte, assegnare il valore della variabile editing sarà sufficiente per visualizzare gli elementi HTML corretti.

Il seguente ti dà un'idea di come appare la struttura di base del blocco `if`:

```svelte
<div class="stack-small">
  {#if editing}
  <!-- markup for editing to-do: label, input text, Cancel and Save Button -->
  {:else}
  <!-- markup for displaying to-do: checkbox, label, Edit and Delete Button -->
  {/if}
</div>
```

La sezione non di modifica — cioè, la parte `{:else}` (metà inferiore) del blocco `if` — sarà molto simile a quella che avevamo nel nostro componente `Todos`. L'unica differenza è che stiamo chiamando `onToggle()`, `onEdit()` e `onRemove()`, a seconda dell'azione dell'utente.

```svelte
{:else}
  <div class="c-cb">
    <input type="checkbox" id="todo-{todo.id}"
      on:click={onToggle} checked={todo.completed}
    >
    <label for="todo-{todo.id}" class="todo-label">{todo.name}</label>
  </div>
  <div class="btn-group">
    <button type="button" class="btn" on:click={onEdit}>
      Edit<span class="visually-hidden"> {todo.name}</span>
    </button>
    <button type="button" class="btn btn__danger" on:click={onRemove}>
      Delete<span class="visually-hidden"> {todo.name}</span>
    </button>
  </div>
{/if}
</div>
```

Vale la pena notare che:

- Quando l'utente preme il pulsante _Edit_, eseguiamo `onEdit()`, che semplicemente imposta la variabile `editing` su `true`.
- Quando l'utente clicca sulla checkbox, chiamiamo la funzione `onToggle()`, che esegue `update()`, passando un oggetto con il nuovo valore `completed` come parametro.
- La funzione `update()` emette l'evento `update`, passando come informazioni aggiuntive una copia dell'attività originale con le modifiche applicate.
- Infine, la funzione `onRemove()` emette un evento `remove`, passando l'attività `todo` da eliminare come dati aggiuntivi.

L'interfaccia di modifica (la metà superiore) conterrà un campo `<input>` e due pulsanti per annullare o salvare le modifiche:

```svelte
<div class="stack-small">
{#if editing}
  <form on:submit|preventDefault={onSave} class="stack-small" on:keydown={(e) => e.key === 'Escape' && onCancel()}>
    <div class="form-group">
      <label for="todo-{todo.id}" class="todo-label">New name for '{todo.name}'</label>
      <input bind:value={name} type="text" id="todo-{todo.id}" autoComplete="off" class="todo-text" />
    </div>
    <div class="btn-group">
      <button class="btn todo-cancel" on:click={onCancel} type="button">
        Cancel<span class="visually-hidden">renaming {todo.name}</span>
        </button>
      <button class="btn btn__primary todo-edit" type="submit" disabled={!name}>
        Save<span class="visually-hidden">new name for {todo.name}</span>
      </button>
    </div>
  </form>
{:else}
[...]
```

Quando l'utente preme il pulsante _Edit_, la variabile `editing` sarà impostata su `true`, e Svelte rimuoverà il markup nella parte `{:else}` del DOM e lo sostituirà con il markup nella sezione `{#if}`.

Il attributo `value` del `<input>` sarà legato alla variabile `name`, e i pulsanti per annullare e salvare le modifiche chiamano rispettivamente `onCancel()` e `onSave()` (abbiamo aggiunto quelle funzioni prima):

- Quando viene invocato `onCancel()`, `name` viene ripristinato al suo valore originale (quando passato come prop) e usciamo dalla modalità di modifica (impostando `editing` su `false`).
- Quando viene invocato `onSave()`, eseguiamo la funzione `update()` — passando il `name` modificato — e usciamo dalla modalità di modifica.

Disabilitiamo anche il pulsante _Save_ quando l'`<input>` è vuoto, usando l'attributo `disabled={!name}`, e permettiamo all'utente di annullare la modifica usando il tasto <kbd>Escape</kbd>, così:

```plain
on:keydown={(e) => e.key === 'Escape' && onCancel()}
```

Usiamo anche `todo.id` per creare id univoci per i nuovi controlli di input e label.

1. Il markup completo aggiornato del nostro componente `Todo` appare come segue. Aggiorna il tuo ora:

   ```svelte
   <div class="stack-small">
   {#if editing}
     <!-- markup for editing todo: label, input text, Cancel and Save Button -->
     <form on:submit|preventDefault={onSave} class="stack-small" on:keydown={(e) => e.key === 'Escape' && onCancel()}>
       <div class="form-group">
         <label for="todo-{todo.id}" class="todo-label">New name for '{todo.name}'</label>
         <input bind:value={name} type="text" id="todo-{todo.id}" autoComplete="off" class="todo-text" />
       </div>
       <div class="btn-group">
         <button class="btn todo-cancel" on:click={onCancel} type="button">
           Cancel<span class="visually-hidden">renaming {todo.name}</span>
           </button>
         <button class="btn btn__primary todo-edit" type="submit" disabled={!name}>
           Save<span class="visually-hidden">new name for {todo.name}</span>
         </button>
       </div>
     </form>
   {:else}
     <!-- markup for displaying todo: checkbox, label, Edit and Delete Button -->
     <div class="c-cb">
       <input type="checkbox" id="todo-{todo.id}"
         on:click={onToggle} checked={todo.completed}
       >
       <label for="todo-{todo.id}" class="todo-label">{todo.name}</label>
     </div>
     <div class="btn-group">
       <button type="button" class="btn" on:click={onEdit}>
         Edit<span class="visually-hidden"> {todo.name}</span>
       </button>
       <button type="button" class="btn btn__danger" on:click={onRemove}>
         Delete<span class="visually-hidden"> {todo.name}</span>
       </button>
     </div>
   {/if}
   </div>
   ```

   > [!NOTE]
   > Potremmo ulteriormente suddividere questo in due diversi componenti, uno per modificare l'attività e l'altro per visualizzarla. Alla fine, dipende da quanto ti senti a tuo agio nel gestire questo livello di complessità in un singolo componente. Dovresti anche considerare se suddividerlo ulteriormente permetterebbe di riutilizzare questo componente in un contesto diverso.

2. Per far funzionare la funzionalità di aggiornamento, dobbiamo gestire l'evento `update` dal componente `Todos`. Nella sua sezione `<script>`, aggiungi questo gestore:

   ```js
   function updateTodo(todo) {
     const i = todos.findIndex((t) => t.id === todo.id);
     todos[i] = { ...todos[i], ...todo };
   }
   ```

   Troviamo l'attività `todo` tramite `id` nel nostro array `todos`, e aggiorniamo il suo contenuto utilizzando la sintassi spread. In questo caso avremmo potuto anche semplicemente usare `todos[i] = todo`, ma questa implementazione è più sicura, permettendo al componente `Todo` di restituire solo le parti aggiornate dell'attività.

3. Successivamente dobbiamo ascoltare l'evento `update` sulla chiamata al nostro componente `<Todo>`, ed eseguire la nostra funzione `updateTodo()` quando si verifica per cambiare il `name` e lo stato `completed`. Aggiorna la tua chiamata al \<Todo> in questo modo:

   ```svelte
   {#each filterTodos(filter, todos) as todo (todo.id)}
   <li class="todo">
     <Todo {todo} on:update={(e) => updateTodo(e.detail)} on:remove={(e) =>
     removeTodo(e.detail)} />
   </li>
   ```

4. Prova di nuovo l'app e dovresti vedere che puoi eliminare, aggiungere, modificare, annullare la modifica e attivare/disattivare lo stato di completamento delle attività. E la nostra intestazione di stato "x su y elementi completati" ora si aggiornerà correttamente quando le attività vengono completate.

Come puoi vedere, è facile implementare lo schema "props-down, events-up" in Svelte. Tuttavia, per componenti semplici `bind` può essere una buona scelta; Svelte ti permetterà di scegliere.

> [!NOTE]
> Svelte fornisce meccanismi più avanzati per condividere informazioni tra componenti: l'[API Context](https://svelte.dev/docs/svelte#setcontext) e [Stores](https://svelte.dev/docs/svelte-store). L'API Context fornisce un meccanismo per consentire ai componenti e ai loro discendenti di "parlare" tra di loro senza passare dati e funzioni come prop, o emettere molti eventi. I Stores ti permettono di condividere dati reattivi tra componenti che non sono gerarchicamente correlati. Esamineremo i Stores più avanti nella serie.

## Il codice finora

### Git

Per vedere lo stato del codice come dovrebbe essere alla fine di questo articolo, accedi alla tua copia del nostro repo in questo modo:

```bash
cd mdn-svelte-tutorial/05-advanced-concepts
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/05-advanced-concepts
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità sviluppo.

### REPL

Per vedere lo stato attuale del codice in un REPL, visita:

<https://svelte.dev/repl/76cc90c43a37452e8c7f70521f88b698?version=3.23.2>

## Sommario

Ora abbiamo tutte le funzionalità richieste della nostra app in atto. Possiamo visualizzare, aggiungere, modificare ed eliminare le attività, segnarle come completate e filtrare per stato.

In questo articolo, abbiamo trattato i seguenti argomenti:

- Estrarre funzionalità in un nuovo componente
- Passare informazioni dal figlio al genitore usando un gestore ricevuto come prop
- Passare informazioni dal figlio al genitore usando la direttiva `bind`
- Rendere condizionalmente blocchi di markup usando il blocco `if`
- Implementare lo schema di comunicazione "props-down, events-up"
- Creare e ascoltare eventi personalizzati

Nel prossimo articolo continueremo a componentizzare la nostra app e vedremo alcune tecniche avanzate per lavorare con il DOM.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_variables_props","Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility", "Learn_web_development/Core/Frameworks_libraries")}}
