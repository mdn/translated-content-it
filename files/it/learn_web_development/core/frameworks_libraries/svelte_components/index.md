---
title: Componentizzazione della nostra app Svelte
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_components
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_variables_props","Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility", "Learn_web_development/Core/Frameworks_libraries")}}

Nell'articolo precedente abbiamo iniziato a sviluppare la nostra app per la lista di cose da fare. L'obiettivo centrale di questo articolo è vedere come suddividere la nostra app in componenti gestibili e condividere informazioni tra di essi. Componentizzeremo la nostra app, quindi aggiungeremo più funzionalità per permettere agli utenti di aggiornare i componenti esistenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È consigliato, come minimo, che Lei abbia familiarità con i linguaggi base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          abbia conoscenze di
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminal/command line</a
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
        Imparare come suddividere la nostra app in componenti e condividere informazioni
        tra di essi.
      </td>
    </tr>
  </tbody>
</table>

## Codifichi insieme a noi

### Git

Clonare il repository GitHub (se non l'ha già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi, per arrivare allo stato corrente dell'app, eseguire

```bash
cd mdn-svelte-tutorial/04-componentizing-our-app
```

Oppure scaricare direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/04-componentizing-our-app
```

Ricordarsi di eseguire `npm install && npm run dev` per avviare la sua app in modalità di sviluppo.

### REPL

Per codificare insieme a noi utilizzando il REPL, inizi da

<https://svelte.dev/repl/99b9eb228b404a2f8c8959b22c0a40d3?version=3.23.2>

## Suddividere l'app in componenti

In Svelte, un'applicazione è composta da uno o più componenti. Un componente è un blocco di codice riutilizzabile e autonomo che incapsula HTML, CSS e JavaScript che sono logicamente correlati, scritto in un file `.svelte`. I componenti possono essere grandi o piccoli, ma di solito sono chiaramente definiti: i componenti più efficaci servono a uno scopo singolo e ovvio.

I benefici della definizione di componenti sono paragonabili alle migliori pratiche generali di organizzazione del codice in pezzi gestibili. Aiuterà a capire come si relazionano tra di loro, promuoverà il riutilizzo, e renderà il codice più facile da ragionare, mantenere, ed estendere.

Ma come si fa a sapere cosa dovrebbe essere diviso in un proprio componente?

Non ci sono regole rigide per questo. Alcune persone preferiscono un approccio intuitivo e iniziano a guardare al markup e disegnano riquadri attorno a ogni componente e sottocomponente che sembra avere la propria logica.

Altri applicano le stesse tecniche utilizzate per decidere se creare una nuova funzione o oggetto. Una di queste tecniche è il principio di singola responsabilità — cioè, un componente dovrebbe idealmente fare una sola cosa. Se finisce per crescere, andrebbe diviso in sottocomponenti più piccoli.

Entrambi gli approcci dovrebbero complementarsi a vicenda e aiutarla a decidere come organizzare meglio i suoi componenti.

Alla fine, smonteremo la nostra app nei seguenti componenti:

- `Alert.svelte`: Una casella di notifica generale per comunicare le azioni che si sono verificate.
- `NewTodo.svelte`: L'input di testo e il pulsante che le permettono di inserire un nuovo elemento da fare.
- `FilterButton.svelte`: I pulsanti _All_, _Active_, e _Completed_ che le permettono di applicare filtri agli elementi da fare visualizzati.
- `TodosStatus.svelte`: L'intestazione "x su y elementi completati".
- `Todo.svelte`: Un singolo elemento da fare. Ogni elemento da fare visibile sarà visualizzato in una copia separata di questo componente.
- `MoreActions.svelte`: I pulsanti _Check All_ e _Remove Completed_ in fondo all'interfaccia utente che permettono di eseguire azioni di massa sugli elementi da fare.

![Rappresentazione grafica dell'elenco dei componenti nella nostra app](01-todo-components.png)

In questo articolo ci concentreremo sulla creazione dei componenti `FilterButton` e `Todo`; affronteremo gli altri in articoli futuri.

Cominciamo.

> [!NOTE]
> Nel processo di creazione dei nostri primi componenti, impareremo anche diverse tecniche per comunicare tra componenti e i pro e i contro di ciascuna.

## Estrazione del nostro componente filtro

Cominceremo creando il nostro `FilterButton.svelte`.

1. Prima di tutto, crei un nuovo file, `components/FilterButton.svelte`.
2. All'interno di questo file dichiareremo una proprietà `filter`, e quindi copieremo il markup pertinente da `Todos.svelte`. Aggiunga il seguente contenuto nel file:

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

3. Ora, nel nostro componente `Todos.svelte`, vogliamo utilizzare il nostro componente `FilterButton`. Prima di tutto, dobbiamo importarlo. Aggiunga la seguente riga all'inizio della sezione `<script>` di `Todos.svelte`:

   ```js
   import FilterButton from "./FilterButton.svelte";
   ```

4. Ora sostituisca l'elemento `<div class="filters...` con una chiamata al componente `FilterButton`, che prende il filtro corrente come proprietà. La riga seguente è tutto ciò di cui ha bisogno:

   ```svelte
   <FilterButton {filter} />
   ```

> [!NOTE]
> Ricordi che quando il nome dell'attributo HTML e la variabile corrispondono, possono essere sostituiti con `{variabile}`. Ecco perché potremmo sostituire `<FilterButton filter={filter} />` con `<FilterButton {filter} />`.

Fin qui tutto bene! Proviamo ora l'app. Noterà che quando clicca sui pulsanti del filtro, vengono selezionati e lo stile si aggiorna correttamente. Ma abbiamo un problema: le cose da fare non vengono filtrate. Questo perché la variabile `filter` scorre verso il basso dal componente `Todos` al componente `FilterButton` attraverso la proprietà, ma le modifiche che avvengono nel componente `FilterButton` non fluiscono verso l'alto nel suo genitore — il data binding è unidirezionale per impostazione predefinita. Vediamo un modo per risolvere questo problema.

## Condivisione dei dati tra componenti: passare un gestore come una proprietà

Un modo per permettere ai componenti figlio di notificare ai genitori eventuali modifiche è passare un gestore come una proprietà. Il componente figlio eseguirà il gestore, passando le informazioni necessarie come un parametro, e il gestore modificherà lo stato del genitore.

Nel nostro caso, il componente `FilterButton` riceverà un gestore `onclick` dal suo genitore. Ogni volta che l'utente clicca su un qualsiasi pulsante del filtro, il figlio chiamerà il gestore `onclick`, passando il filtro selezionato come un parametro verso l'alto al suo genitore.

Dichiareremo semplicemente la proprietà `onclick` assegnando un gestore fittizio per prevenire errori, come questo:

```js
export let onclick = (clicked) => {};
```

E dichiareremo l'istruzione reattiva `$: onclick(filter)` per chiamare il gestore `onclick` ogni volta che la variabile `filter` viene aggiornata.

1. La sezione `<script>` del nostro componente `FilterButton` dovrebbe finire per assomigliare a questa. La aggiorni ora:

   ```js
   export let filter = "all";
   export let onclick = (clicked) => {};
   $: onclick(filter);
   ```

2. Ora, quando chiamiamo `FilterButton` all'interno di `Todos.svelte`, dovremo specificare il gestore. La aggiorni come segue:

   ```svelte
   <FilterButton {filter} onclick={ (clicked) => filter = clicked }/>
   ```

Quando un qualsiasi pulsante di filtro è cliccato, aggiorniamo semplicemente la variabile filter con il nuovo filtro. Ora il nostro componente `FilterButton` funzionerà di nuovo.

## Binding dati bidirezionale più semplice con il direttiva bind

Nell'esempio precedente abbiamo realizzato che il nostro componente `FilterButton` non stava funzionando perché lo stato della nostra applicazione stava fluendo verso il basso dal genitore al figlio attraverso la proprietà `filter`, ma non tornava verso l'alto. Quindi abbiamo aggiunto una proprietà `onclick` per permettere al componente figlio di comunicare il nuovo valore `filter` al suo genitore.

Funziona bene, ma Svelte ci fornisce un modo più semplice e diretto per ottenere un binding dati bidirezionale. I dati generalmente fluiscono verso il basso dal genitore al figlio usando le proprietà. Se vogliamo che fluiscano anche nell'altro modo, dal figlio al genitore, possiamo usare [la direttiva `bind:`](https://svelte.dev/docs/element-directives#bind-property).

Usando `bind`, diremo a Svelte che qualsiasi modifica apportata alla proprietà `filter` nel componente `FilterButton` dovrebbe propagarsi verso l'alto al componente genitore, `Todos`. Cioè, collegheremo il valore della variabile `filter` nel genitore al suo valore nel figlio.

1. In `Todos.svelte`, aggiorni la chiamata al componente `FilterButton` come segue:

   ```svelte
   <FilterButton bind:filter={filter} />
   ```

   Come di consueto, Svelte ci fornisce una bella scorciatoia: `bind:value={value}` è equivalente a `bind:value`. Quindi, nell'esempio sopra, potrebbe semplicemente scrivere `<FilterButton bind:filter />`.

2. Il componente figlio ora può modificare il valore della variabile filter del genitore, quindi non abbiamo più bisogno della proprietà `onclick`. Modifichi l'elemento `<script>` del suo `FilterButton` così:

   ```svelte
   <script>
     export let filter = "all";
   </script>
   ```

3. Provi di nuovo la sua app, e dovrebbe vedere i filtri funzionare correttamente.

## Creazione del nostro componente Todo

Ora creeremo un componente `Todo` per incapsulare ogni singola attività da fare, incluso la casella di controllo e una logica di modifica in modo da poter cambiare un'attività da fare esistente.

Il nostro componente `Todo` riceverà un singolo oggetto `todo` come proprietà. Dichiariamo la proprietà `todo` e spostiamo il codice dal componente `Todos`. Per ora, sostituiremo la chiamata a `removeTodo` con un avviso. Aggiungeremo nuovamente quella funzionalità più avanti.

1. Crei un nuovo file di componente, `components/Todo.svelte`.
2. Inserisca il seguente contenuto all'interno di questo file:

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

3. Ora dobbiamo importare il nostro componente `Todo` in `Todos.svelte`. Vada ora a questo file e aggiunga la seguente istruzione `import` sotto quella precedente:

   ```js
   import Todo from "./Todo.svelte";
   ```

4. Ora dobbiamo aggiornare il nostro blocco `{#each}` per includere un componente `<Todo>` per ogni attività da fare, anziché il codice che è stato spostato a `Todo.svelte`. Stiamo anche passando l'oggetto `todo` corrente nel componente come proprietà.

   Aggiorni il blocco `{#each}` all'interno di `Todos.svelte` così:

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

L'elenco delle attività è visualizzato sulla pagina e le caselle di controllo dovrebbero funzionare (provi a selezionarne alcune, e poi verifichi che i filtri funzionino ancora correttamente), ma la nostra intestazione di stato "x su y elementi completati" non si aggiornerà più di conseguenza. Questo perché il nostro componente `Todo` sta ricevendo l'attività da fare attraverso la proprietà, ma non sta inviando alcuna informazione al suo genitore. Risolveremo questo più avanti.

## Condivisione dei dati tra componenti: pattern props-down, events-up

La direttiva `bind` è piuttosto semplice e le consente di condividere dati tra un componente genitore e figlio con il minimo sforzo. Tuttavia, quando la sua applicazione diventa più grande e complessa, può facilmente diventare difficile tenere traccia di tutti i suoi valori collegati. Un approccio diverso è il pattern di comunicazione "props-down, events-up".

Fondamentalmente, questo pattern si basa sul fatto che i componenti figli ricevono dati dai loro genitori tramite le proprietà e i componenti genitori aggiornano il loro stato gestendo gli eventi emessi dai componenti figli. Quindi le proprietà _fluiscono verso il basso_ dal genitore al figlio e gli eventi _risalgono_ dal figlio al genitore. Questo pattern stabilisce un flusso di informazioni bidirezionale, che è prevedibile e più facile da razionalizzare.

Vediamo come emettere i nostri eventi per re-implementare la funzionalità mancante del pulsante _Delete_.

Per creare eventi personalizzati, useremo l'utilità `createEventDispatcher`. Questo restituirà una funzione `dispatch()` che ci permetterà di emettere eventi personalizzati. Quando si invia un evento, è necessario passare il nome dell'evento e, facoltativamente, un oggetto con informazioni aggiuntive che si desidera passare a ogni ascoltatore. Questi dati aggiuntivi saranno disponibili sulla proprietà `detail` dell'oggetto evento.

> [!NOTE]
> Gli eventi personalizzati in Svelte condividono la stessa API degli eventi DOM regolari. Inoltre, può far risalire un evento al suo componente genitore specificando `on:event` senza alcun gestore.

Modificheremo il nostro componente `Todo` per emettere un evento `remove`, passando l'attività da rimuovere come informazione aggiuntiva.

1. Prima di tutto, aggiunga le seguenti righe nella sezione `<script>` del componente `Todo`:

   ```js
   import { createEventDispatcher } from "svelte";
   const dispatch = createEventDispatcher();
   ```

2. Ora aggiorni il pulsante _Delete_ nella sezione markup dello stesso file per assomigliare a questo:

   ```svelte
   <button type="button" class="btn btn__danger" on:click={() => dispatch('remove', todo)}>
     Delete <span class="visually-hidden">{todo.name}</span>
   </button>
   ```

   Con `dispatch('remove', todo)` stiamo emettendo un evento `remove` e passando come dati aggiuntivi l'attività che viene eliminata. Il gestore sarà chiamato con un oggetto evento disponibile, con i dati aggiuntivi disponibili nella proprietà `event.detail` dell'oggetto evento.

3. Ora dobbiamo ascoltare quell'evento dall'interno di `Todos.svelte` e agire di conseguenza. Torni a questo file e aggiorni la sua chiamata al componente `<Todo>` come segue:

   ```svelte
   <Todo {todo} on:remove={(e) => removeTodo(e.detail)} />
   ```

   Il nostro gestore riceve il parametro `e` (l'oggetto evento), che come descritto prima contiene l'attività da eliminare nella proprietà `detail`.

4. A questo punto, se prova di nuovo la sua app, dovrebbe vedere che la funzionalità _Delete_ ora funziona nuovamente. Quindi, il nostro evento personalizzato ha funzionato come speravamo. Inoltre, il listener dell'evento `remove` sta inviando la modifica dei dati verso l'alto al genitore, quindi la nostra intestazione di stato "x su y elementi completati" ora si aggiornerà correttamente quando le attività vengono eliminate.

Ora ci occuperemo dell'evento `update`, in modo che il nostro componente genitore possa essere notificato di qualsiasi modifica all'attività.

## Aggiornamento delle attività

Dobbiamo ancora implementare la funzionalità per consentirci di modificare le attività esistenti. Dovremo includere una modalità di modifica nel componente `Todo`. Quando si entra in modalità di modifica, mostreremo un campo `<input>` che ci permetterà di modificare il nome dell'attività corrente, con due pulsanti per confermare o annullare le nostre modifiche.

### Gestire gli eventi

1. Avremo bisogno di una variabile per tenere traccia del fatto che siamo in modalità di modifica e un'altra per memorizzare il nome dell'attività in fase di aggiornamento. Aggiunga le seguenti definizioni di variabili alla fine della sezione `<script>` del componente `Todo`:

   ```js
   let editing = false; // track editing mode
   let name = todo.name; // hold the name of the to-do being edited
   ```

2. Dobbiamo decidere quali eventi il nostro componente `Todo` emetterà:

   - Potremmo emettere eventi diversi per il toggle dello stato e la modifica del nome (per esempio, `updateTodoStatus` e `updateTodoName`).
   - Oppure potremmo adottare un approccio più generico ed emettere un solo evento `update` per entrambe le operazioni.

   Prenderemo il secondo approccio in modo da poter dimostrare una tecnica diversa. Il vantaggio di questo approccio è che in seguito possiamo aggiungere più campi alle attività e gestire comunque tutti gli aggiornamenti con lo stesso evento.

   Creiamo una funzione `update()` che riceverà le modifiche ed emetterà un evento update con l'attività modificata. Aggiunga quanto segue, sempre alla fine della sezione `<script>`:

   ```js
   function update(updatedTodo) {
     todo = { ...todo, ...updatedTodo }; // applies modifications to todo
     dispatch("update", todo); // emit update event
   }
   ```

   Qui stiamo usando la [sintassi di spread](/it/docs/Web/JavaScript/Reference/Operators/Spread_syntax) per restituire l'attività originale con le modifiche applicate ad essa.

3. Poi creeremo diverse funzioni per gestire ciascuna azione dell'utente. Quando l'attività è in modalità di modifica, l'utente può salvare o annullare le modifiche. Quando non è in modalità di modifica, l'utente può eliminare l'attività, modificarla o cambiare il suo stato tra completata e attiva.

   Aggiunga il seguente set di funzioni sotto la sua funzione precedente per gestire queste azioni:

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

### Aggiornamento del markup

Ora dobbiamo aggiornare il markup del nostro componente `Todo` per chiamare le funzioni sopra riportate quando vengono intraprese le azioni appropriate.

Per gestire la modalità di modifica, stiamo usando la variabile `editing`, che è booleana. Quando è `true`, dovrebbe visualizzare il campo `<input>` per modificare il nome dell'attività, e i pulsanti _Cancel_ e _Save_. Quando non è in modalità di modifica, visualizzerà la casella di controllo, il nome dell'attività e i pulsanti per modificare ed eliminare l'attività.

Per ottenere ciò utilizzeremo un [blocco `if`](https://svelte.dev/docs/logic-blocks#if). Il blocco `if` rappresenta condizionatamente un markup. Tenga presente che non mostrerà o nasconderà solo il markup in base alla condizione — aggiungerà dinamicamente e rimuoverà gli elementi dal DOM, a seconda della condizione.

Quando `editing` è `true`, per esempio, Svelte mostrerà il modulo di aggiornamento; quando è `false`, lo rimuoverà dal DOM e aggiungerà la casella di controllo. Grazie alla reattività di Svelte, assegnare il valore della variabile editing sarà sufficiente a visualizzare gli elementi HTML corretti.

Il seguente le dà un'idea di come appare la struttura base del blocco `if`:

```svelte
<div class="stack-small">
  {#if editing}
  <!-- markup for editing to-do: label, input text, Cancel and Save Button -->
  {:else}
  <!-- markup for displaying to-do: checkbox, label, Edit and Delete Button -->
  {/if}
</div>
```

La sezione non di modifica — cioè, la parte del `{:else}` (parte inferiore) del blocco `if` — sarà molto simile a quella che avevamo nel nostro componente `Todos`. La sola differenza è che stiamo chiamando `onToggle()`, `onEdit()`, e `onRemove()`, a seconda dell'azione dell'utente.

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

- Quando l'utente preme il pulsante _Edit_, eseguiamo `onEdit()`, che imposta semplicemente la variabile `editing` su `true`.
- Quando l'utente clicca sulla casella di controllo, chiamiamo la funzione `onToggle()`, che esegue `update()`, passando un oggetto con il nuovo valore `completed` come parametro.
- La funzione `update()` emette l'evento `update`, passando come informazione aggiuntiva una copia dell'attività originale con le modifiche applicate.
- Infine, la funzione `onRemove()` emette l'evento `remove`, passando l'attività `todo` da eliminare come dati aggiuntivi.

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

Quando l'utente preme il pulsante _Edit_, la variabile `editing` sarà impostata su `true`, e Svelte rimuoverà il markup nella parte `{:else}` del DOM e lo sostituirà con il markup nella parte `{#if}`.

La proprietà `value` dell'`<input>` sarà collegata alla variabile `name`, e i pulsanti per annullare e salvare le modifiche chiamano rispettivamente `onCancel()` e `onSave()` (abbiamo aggiunto queste funzioni precedentemente):

- Quando `onCancel()` viene invocata, `name` viene ripristinato al suo valore originale (quando passato come proprietà) e usciamo dalla modalità di modifica (impostando `editing` su `false`).
- Quando `onSave()` viene invocata, eseguiamo la funzione `update()` — passandole il nome modificato — e usciamo dalla modalità di modifica.

Disabilitiamo anche il pulsante _Save_ quando l'`<input>` è vuoto, usando l'attributo `disabled={!name}`, e permettiamo all'utente di annullare l'editing usando il tasto <kbd>Escape</kbd>, come questo:

```plain
on:keydown={(e) => e.key === 'Escape' && onCancel()}
```

Usiamo anche `todo.id` per creare ID univoci per i nuovi controlli e etichette di input.

1. Il markup aggiornato completo del nostro componente `Todo` appare come segue. Lo aggiorni ora:

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
   > Potremmo ulteriormente dividere questo in due componenti diversi, uno per modificare l'attività e l'altro per visualizzarla. Alla fine, si riduce a quanto si sente a suo agio a gestire questo livello di complessità in un singolo componente. Dovrebbe anche considerare se dividerlo ulteriormente consentirebbe di riutilizzare questo componente in un contesto diverso.

2. Per far funzionare la funzionalità di aggiornamento, dobbiamo gestire l'evento `update` dal componente `Todos`. Nella sua sezione `<script>`, aggiunga questo gestore:

   ```js
   function updateTodo(todo) {
     const i = todos.findIndex((t) => t.id === todo.id);
     todos[i] = { ...todos[i], ...todo };
   }
   ```

   Troviamo l'attività tramite `id` nel nostro array `todos`, e aggiorniamo il suo contenuto usando la sintassi di spread. In questo caso potremmo aver anche solo usato `todos[i] = todo`, ma questa implementazione è più a prova di errori, consentendo al componente `Todo` di restituire solo le parti aggiornate dell'attività.

3. Poi dobbiamo ascoltare l'evento `update` nella nostra chiamata componente `<Todo>`, ed eseguire la nostra funzione `updateTodo()` quando questo avviene per cambiare il nome e lo stato `completed`. Aggiorni la sua chiamata \<Todo> così:

   ```svelte
   {#each filterTodos(filter, todos) as todo (todo.id)}
   <li class="todo">
     <Todo {todo} on:update={(e) => updateTodo(e.detail)} on:remove={(e) =>
     removeTodo(e.detail)} />
   </li>
   ```

4. Provi di nuovo la sua app, e dovrebbe vedere che può eliminare, aggiungere, modificare, annullare la modifica e cambiare lo stato di completamento delle attività. E la nostra intestazione di stato "x su y elementi completati" ora si aggiornerà correttamente quando le attività vengono completate.

Come può vedere, è facile implementare il pattern "props-down, events-up" in Svelte. Tuttavia, per componenti semplici il `bind` può essere una buona scelta; Svelte le permetterà di scegliere.

> [!NOTE]
> Svelte fornisce meccanismi più avanzati per condividere informazioni tra componenti: l'[API di Contesto](https://svelte.dev/docs/svelte#setcontext) e i [Negozio](https://svelte.dev/docs/svelte-store). L'API di Contesto fornisce un meccanismo affinché i componenti e i loro discendenti possano "parlare" tra loro senza dover passare dati e funzioni come proprietà, o inviare molti eventi. I Negozio permettono di condividere dati reattivi tra componenti che non sono gerarchicamente correlati. Esamineremo i Negozio più avanti nella serie.

## Il codice fino ad ora

### Git

Per vedere lo stato del codice com'è alla fine di questo articolo, acceda alla sua copia del nostro repository in questo modo:

```bash
cd mdn-svelte-tutorial/05-advanced-concepts
```

Oppure scarichi direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/05-advanced-concepts
```

Ricordarsi di eseguire `npm install && npm run dev` per avviare la sua app in modalità di sviluppo.

### REPL

Per vedere lo stato corrente del codice in un REPL, visiti:

<https://svelte.dev/repl/76cc90c43a37452e8c7f70521f88b698?version=3.23.2>

## Sommario

Ora abbiamo in atto tutte le funzionalità richieste della nostra app. Possiamo visualizzare, aggiungere, modificare ed eliminare le attività, contrassegnarle come completate, e filtrare per stato.

In questo articolo, abbiamo trattato i seguenti argomenti:

- Estrarre funzionalità in un nuovo componente
- Passare informazione da figlio a genitore usando un gestore ricevuto come proprietà
- Passare informazione da figlio a genitore usando la direttiva `bind`
- Rappresentazione condizionale dei blocchi di markup usando il blocco `if`
- Implementare il pattern di comunicazione "props-down, events-up"
- Creare e ascoltare eventi personalizzati

Nell'articolo successivo continueremo a componentizzare la nostra app e vedremo alcune tecniche avanzate per lavorare con il DOM.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_variables_props","Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility", "Learn_web_development/Core/Frameworks_libraries")}}
