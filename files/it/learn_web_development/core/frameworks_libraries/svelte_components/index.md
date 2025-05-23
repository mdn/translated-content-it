---
title: Componentizzazione della nostra app Svelte
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_components
l10n:
  sourceCommit: e6d43da6c6d28a6ac92cdd47882809ffbdf987ce
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_variables_props","Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility", "Learn_web_development/Core/Frameworks_libraries")}}

Nell'ultimo articolo abbiamo iniziato a sviluppare la nostra app lista di cose da fare. L'obiettivo centrale di questo articolo è vedere come suddividere la nostra app in componenti gestibili e condividere informazioni tra di essi. Componentizzeremo la nostra app, quindi aggiungeremo più funzionalità per consentire agli utenti di aggiornare i componenti esistenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È raccomandato avere familiarità con i linguaggi di base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e avere conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/riga di comando</a
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
        Imparare come suddividere la nostra app in componenti e condividere informazioni tra di essi.
      </td>
    </tr>
  </tbody>
</table>

## Codifica insieme a noi

### Git

Clona il repository GitHub (se non lo hai ancora fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi per arrivare allo stato attuale dell'app, esegui

```bash
cd mdn-svelte-tutorial/04-componentizing-our-app
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/04-componentizing-our-app
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità di sviluppo.

### REPL

Per codificare insieme a noi usando il REPL, inizia qui

<https://svelte.dev/repl/99b9eb228b404a2f8c8959b22c0a40d3?version=3.23.2>

## Suddivisione dell'app in componenti

In Svelte, un'applicazione è composta da uno o più componenti. Un componente è un blocco di codice riutilizzabile e autonomo che incapsula HTML, CSS e JavaScript che appartengono insieme, scritto in un file `.svelte`. I componenti possono essere grandi o piccoli, ma di solito sono chiaramente definiti: i componenti più efficaci servono a un unico scopo evidente.

I benefici di definire componenti sono paragonabili alla pratica generale migliore di organizzare il proprio codice in pezzi gestibili. Aiuterà a comprendere come essi si relazionano tra loro, promuoverà il riutilizzo e renderà il codice più facile da ragionare, mantenere ed estendere.

Ma come si sa cosa dovrebbe essere suddiviso in un proprio componente?

Non ci sono regole rigide per questo. Alcune persone preferiscono un approccio intuitivo e iniziano a guardare il markup e a disegnare scatole attorno a ogni componente e sottocomponente che sembra avere la propria logica.

Altre persone applicano le stesse tecniche utilizzate per decidere se creare una nuova funzione o oggetto. Una di queste tecniche è il principio di responsabilità unica — cioè, un componente dovrebbe idealmente fare solo una cosa. Se finisce per crescere, dovrebbe essere suddiviso in sottocomponenti più piccoli.

Entrambi gli approcci dovrebbero completarsi a vicenda e aiutarti a decidere come organizzare meglio i tuoi componenti.

Alla fine, suddivideremo la nostra app nei seguenti componenti:

- `Alert.svelte`: Una casella di notifica generale per comunicare le azioni che sono avvenute.
- `NewTodo.svelte`: L'input di testo e il pulsante che ti permettono di inserire un nuovo elemento da fare.
- `FilterButton.svelte`: I pulsanti _Tutti_, _Attivi_ e _Completati_ che ti permettono di applicare filtri agli elementi da fare visualizzati.
- `TodosStatus.svelte`: L'intestazione "x su y elementi completati".
- `Todo.svelte`: Un singolo elemento da fare. Ogni elemento visibile da fare sarà visualizzato in una copia separata di questo componente.
- `MoreActions.svelte`: I pulsanti _Seleziona Tutti_ e _Rimuovi Completati_ nella parte inferiore dell'interfaccia utente che ti permettono di eseguire azioni di massa sugli elementi da fare.

![rappresentazione grafica dell'elenco dei componenti nella nostra app](01-todo-components.png)

In questo articolo ci concentreremo sulla creazione dei componenti `FilterButton` e `Todo`; arriveremo agli altri in articoli futuri.

Iniziamo.

> [!NOTE]
> Nel processo di creazione dei nostri primi componenti, impareremo anche diverse tecniche per comunicare tra i componenti e i pro e i contro di ciascuna.

## Estrazione del nostro componente filtro

Inizieremo creando `FilterButton.svelte`.

1. Prima di tutto, crea un nuovo file, `components/FilterButton.svelte`.
2. All'interno di questo file dichiareremo una proprietà `filter`, quindi copieremo il markup pertinente da `Todos.svelte`. Aggiungi il seguente contenuto nel file:

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

3. Tornati nel nostro componente `Todos.svelte`, vogliamo utilizzare il nostro componente `FilterButton`. Prima di tutto, dobbiamo importarlo. Aggiungi la seguente riga all'inizio della sezione `<script>` di `Todos.svelte`:

   ```js
   import FilterButton from "./FilterButton.svelte";
   ```

4. Ora sostituisci l'elemento `<div class="filters...` con una chiamata al componente `FilterButton`, che prende il filtro corrente come proprietà. La seguente linea è tutto ciò di cui hai bisogno:

   ```svelte
   <FilterButton {filter} />
   ```

> [!NOTE]
> Ricorda che quando il nome dell'attributo HTML e la variabile coincidono, possono essere sostituiti con `{variabile}`. Ecco perché potremmo sostituire `<FilterButton filter={filter} />` con `<FilterButton {filter} />`.

Fin qui tutto bene! Proviamo l'app ora. Noterai che quando fai clic sui pulsanti filtro, vengono selezionati e lo stile si aggiorna di conseguenza. Ma abbiamo un problema: i da fare non sono filtrati. Questo perché la variabile `filter` fluisce verso il basso dal componente `Todos` al componente `FilterButton` attraverso la proprietà, ma le modifiche nel componente `FilterButton` non fluiscono verso l'alto al suo genitore — il binding dei dati è unidirezionale per impostazione predefinita. Vediamo un modo per risolvere questo problema.

## Condivisione dei dati tra componenti: passare un gestore come proprietà

Un modo per consentire ai componenti figli di notificare ai genitori eventuali modifiche è passare un gestore come proprietà. Il componente figlio eseguirà il gestore, passando le informazioni necessarie come parametro, e il gestore modificherà lo stato del genitore.

Nel nostro caso, il componente `FilterButton` riceverà un gestore `onclick` dal suo genitore. Ogni volta che l'utente fa clic su un pulsante filtro, il figlio chiamerà il gestore `onclick`, passando il filtro selezionato come un parametro verso il genitore.

Dichiareremo semplicemente la proprietà `onclick` assegnandole un gestore fittizio per prevenire errori, come questo:

```js
export let onclick = (clicked) => {};
```

E dichiareremo l'istruzione reattiva `$: onclick(filter)` per chiamare il gestore `onclick` ogni volta che la variabile `filter` viene aggiornata.

1. La sezione `<script>` del nostro componente `FilterButton` dovrebbe risultare così. Aggiorna ora:

   ```js
   export let filter = "all";
   export let onclick = (clicked) => {};
   $: onclick(filter);
   ```

2. Ora quando chiamiamo `FilterButton` all'interno di `Todos.svelte`, dobbiamo specificare il gestore. Aggiorna così:

   ```svelte
   <FilterButton {filter} onclick={ (clicked) => filter = clicked }/>
   ```

Quando si clicca su qualsiasi pulsante filtro, aggiorneremo semplicemente la variabile filtro con il nuovo filtro. Ora il nostro componente `FilterButton` funzionerà di nuovo.

## Binding dei dati bidirezionale più semplice con la direttiva bind

Nell'esempio precedente abbiamo capito che il nostro componente `FilterButton` non funzionava perché il nostro stato dell'applicazione fluiva verso il basso dal genitore al figlio tramite la proprietà `filter`, ma non tornava indietro. Quindi abbiamo aggiunto una proprietà `onclick` per permettere al componente figlio di comunicare il nuovo valore `filter` al suo genitore.

Funziona, ma Svelte ci offre un modo più semplice e diretto per ottenere il binding dei dati bidirezionale. I dati ordinariamente fluiscono verso il basso dal genitore al figlio utilizzando le proprietà. Se vogliamo che fluiscano anche nell'altro modo, dal figlio al genitore, possiamo usare [la direttiva `bind:`](https://svelte.dev/docs/element-directives#bind-property).

Usando `bind`, diremo a Svelte che qualsiasi cambiamento fatto alla proprietà `filter` nel componente `FilterButton` dovrebbe propagarsi verso l'alto al componente genitore, `Todos`. Cioè, legheremo il valore della variabile `filter` nel genitore al suo valore nel figlio.

1. In `Todos.svelte`, aggiorna la chiamata al componente `FilterButton` come segue:

   ```svelte
   <FilterButton bind:filter={filter} />
   ```

   Come al solito, Svelte ci fornisce una scorciatoia: `bind:value={value}` è equivalente a `bind:value`. Quindi nell'esempio sopra potresti semplicemente scrivere `<FilterButton bind:filter />`.

2. Il componente figlio può ora modificare il valore della variabile filtro del genitore, quindi non abbiamo più bisogno della proprietà `onclick`. Modifica l'elemento `<script>` del tuo `FilterButton` così:

   ```svelte
   <script>
     export let filter = "all";
   </script>
   ```

3. Riprova la tua app e dovresti ancora vedere i tuoi filtri funzionare correttamente.

## Creazione del nostro componente Todo

Ora creeremo un componente `Todo` per incapsulare ogni singolo elemento da fare, compreso il checkbox e un po' di logica di modifica in modo da poter cambiare un elemento da fare esistente.

Il nostro componente `Todo` riceverà un singolo oggetto `todo` come proprietà. Dichiariamo la proprietà `todo` e spostiamo il codice dal componente `Todos`. Per ora, sostituiremo la chiamata a `removeTodo` con un alert. Aggiungeremo quella funzionalità più tardi.

1. Crea un nuovo file componente, `components/Todo.svelte`.
2. Metti i seguenti contenuti all'interno di questo file:

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

3. Ora dobbiamo importare il nostro componente `Todo` in `Todos.svelte`. Vai a questo file ora e aggiungi la seguente dichiarazione `import` sotto la tua precedente:

   ```js
   import Todo from "./Todo.svelte";
   ```

4. Poi dobbiamo aggiornare il nostro blocco `{#each}` per includere un componente `<Todo>` per ogni to-do, piuttosto che il codice che è stato spostato in `Todo.svelte`. Stiamo anche passando l'oggetto `todo` corrente nel componente come proprietà.

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

L'elenco degli elementi da fare è visualizzato sulla pagina, e i checkbox dovrebbero funzionare (prova a selezionarne/deselezionarne un paio, e poi osserva che i filtri funzionano ancora come previsto), ma la nostra intestazione di stato "x su y elementi completati" non si aggiornerà più di conseguenza. Questo perché il nostro componente `Todo` sta ricevendo il to-do tramite la proprietà, ma non sta inviando alcuna informazione al suo genitore. Risolveremo questo più avanti.

## Condivisione dei dati tra componenti: pattern props-down, events-up

La direttiva `bind` è piuttosto semplice e permette di condividere dati tra un componente genitore e uno figlio con un minimo sforzo. Tuttavia, quando l'applicazione cresce e diventa più complessa, può diventare facilmente difficile tenere traccia di tutti i valori vincolati. Un approccio diverso è il pattern di comunicazione "props-down, events-up".

Fondamentalmente, questo pattern si basa sul fatto che i componenti figlio ricevono dati dai loro genitori tramite le proprietà e i componenti genitori aggiornano il loro stato gestendo eventi emessi dai componenti figli. Quindi le proprietà _fluiscono verso il basso_ dal genitore al figlio e gli eventi _salgono_ dal figlio al genitore. Questo pattern stabilisce un flusso di informazioni bidirezionale, che è prevedibile e più facile da ragionare.

Vediamo come emettere i nostri eventi personalizzati per reimplementare la funzionalità mancante del pulsante _Elimina_.

Per creare eventi personalizzati, useremo l'utilità `createEventDispatcher`. Questo restituirà una funzione `dispatch()` che ci permetterà di emettere eventi personalizzati. Quando emetti un evento, devi passare il nome dell'evento e, opzionalmente, un oggetto con informazioni aggiuntive che vuoi passare a ogni ascoltatore. Questi dati aggiuntivi saranno disponibili nella proprietà `detail` dell'oggetto evento.

> [!NOTE]
> Gli eventi personalizzati in Svelte condividono la stessa API degli eventi DOM regolari. Inoltre, puoi far propagare un evento al tuo componente genitore specificando `on:event` senza alcun gestore.

Modificheremo il nostro componente `Todo` per emettere un evento `remove`, passando come informazioni aggiuntive il to-do che viene rimosso.

1. Prima di tutto, aggiungi le seguenti righe nella parte superiore della sezione `<script>` del componente `Todo`:

   ```js
   import { createEventDispatcher } from "svelte";

   const dispatch = createEventDispatcher();
   ```

2. Ora aggiorna il pulsante _Elimina_ nella sezione del markup dello stesso file per apparire così:

   ```svelte
   <button type="button" class="btn btn__danger" on:click={() => dispatch('remove', todo)}>
     Delete <span class="visually-hidden">{todo.name}</span>
   </button>
   ```

   Con `dispatch('remove', todo)` stiamo emettendo un evento `remove`, e passando come dati aggiuntivi il `todo` che viene eliminato. Il gestore verrà chiamato con un oggetto evento disponibile, con i dati aggiuntivi disponibili nella proprietà `event.detail`.

3. Ora dobbiamo ascoltare quell'evento dall'interno di `Todos.svelte` e agire di conseguenza. Torna a questo file e aggiorna la tua chiamata al componente `<Todo>` così:

   ```svelte
   <Todo {todo} on:remove={(e) => removeTodo(e.detail)} />
   ```

   Il nostro gestore riceve il parametro `e` (l'oggetto evento), che come descritto prima contiene il to-do da eliminare nella proprietà `detail`.

4. A questo punto, se provi di nuovo la tua app, dovresti vedere che la funzionalità _Elimina_ ora funziona di nuovo. Quindi il nostro evento personalizzato ha funzionato come speravamo. Inoltre, l'ascoltatore dell'evento `remove` sta inviando la modifica dei dati al genitore, quindi la nostra intestazione di stato "x su y elementi completati" ora si aggiornerà adeguatamente quando i to-do vengono eliminati.

Ora ci occuperemo dell'evento `update`, così che il nostro componente genitore possa essere notificato di qualsiasi to-do modificato.

## Aggiornamento dei to-do

Dobbiamo ancora implementare la funzionalità per consentirci di modificare i to-do esistenti. Dobbiamo includere una modalità di modifica nel componente `Todo`. Quando si entra nella modalità di modifica, mostreremo un campo `<input>` per permetterci di modificare il nome del to-do corrente, con due pulsanti per confermare o annullare le modifiche.

### Gestire gli eventi

1. Avremo bisogno di una variabile per tenere traccia se siamo in modalità di modifica e un'altra per memorizzare il nome dell'attività in fase di aggiornamento. Aggiungi le seguenti definizioni di variabili alla fine della sezione `<script>` del componente `Todo`:

   ```js
   let editing = false; // track editing mode
   let name = todo.name; // hold the name of the to-do being edited
   ```

2. Dobbiamo decidere quali eventi il nostro componente `Todo` emetterà:

   - Potremmo emettere eventi diversi per il toggle dello stato e la modifica del nome (per esempio, `updateTodoStatus` e `updateTodoName`).
   - Oppure potremmo adottare un approccio più generico ed emettere un unico evento `update` per entrambe le operazioni.

   Adotteremo il secondo approccio per poter dimostrare una tecnica diversa. Il vantaggio di questo approccio è che in seguito possiamo aggiungere più campi ai to-do e gestire comunque tutti gli aggiornamenti con lo stesso evento.

   Creiamo una funzione `update()` che riceverà le modifiche ed emetterà un evento di aggiornamento con il to-do modificato. Aggiungi il seguente codice, ancora una volta, alla fine della sezione `<script>`:

   ```js
   function update(updatedTodo) {
     todo = { ...todo, ...updatedTodo }; // applies modifications to todo
     dispatch("update", todo); // emit update event
   }
   ```

   Qui stiamo usando la [sintassi di spread](/it/docs/Web/JavaScript/Reference/Operators/Spread_syntax) per restituire il to-do originale con le modifiche applicate.

3. Quindi creeremo diverse funzioni per gestire ogni azione dell'utente. Quando il to-do è in modalità di modifica, l'utente può salvare o annullare le modifiche. Quando non è in modalità di modifica, l'utente può eliminare il to-do, modificarlo o alternare il suo stato tra completato e attivo.

   Aggiungi la seguente serie di funzioni sotto la tua funzione precedente per gestire queste azioni:

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

Ora dobbiamo aggiornare il markup del nostro componente `Todo` per chiamare le funzioni sopra quando vengono intraprese le azioni appropriate.

Per gestire la modalità di modifica, stiamo usando la variabile `editing`, che è un booleano. Quando è `true`, dovrebbe mostrare il campo `<input>` per modificare il nome del to-do, e i pulsanti _Annulla_ e _Salva_. Quando non è in modalità di modifica, mostrerà il checkbox, il nome del to-do e i pulsanti per modificare ed eliminare il to-do.

Per ottenere ciò useremo un [blocco `if`](https://svelte.dev/docs/logic-blocks#if). Il blocco `if` rende in modo condizionale un markup. Tieni presente che non si limiterà a mostrare o nascondere il markup basato sulla condizione — aggiungerà e rimuoverà dinamicamente gli elementi dal DOM, a seconda della condizione.

Quando `editing` è `true`, ad esempio, Svelte mostrerà il modulo di aggiornamento; quando è `false`, lo rimuoverà dal DOM e aggiungerà il checkbox. Grazie alla reattività di Svelte, assegnare il valore della variabile editing sarà sufficiente per visualizzare gli elementi HTML corretti.

Ecco un'idea della struttura di blocco `if` di base:

```svelte
<div class="stack-small">
  {#if editing}
  <!-- markup for editing to-do: label, input text, Cancel and Save Button -->
  {:else}
  <!-- markup for displaying to-do: checkbox, label, Edit and Delete Button -->
  {/if}
</div>
```

La sezione non di modifica — cioè, la parte `{:else}` (parte inferiore) del blocco `if` — sarà molto simile a quella che avevamo nel nostro componente `Todos`. L'unica differenza è che stiamo chiamando `onToggle()`, `onEdit()` e `onRemove()`, a seconda dell'azione dell'utente.

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

- Quando l'utente preme il pulsante _Modifica_, eseguiamo `onEdit()`, che imposta solo la variabile `editing` su `true`.
- Quando l'utente clicca sul checkbox, chiamiamo la funzione `onToggle()`, che esegue `update()`, passando un oggetto con il nuovo valore `completed` come parametro.
- La funzione `update()` emette l'evento `update`, passando come informazioni aggiuntive una copia del to-do originale con le modifiche applicate.
- Infine, la funzione `onRemove()` emette l'evento `remove`, passando il `todo` da eliminare come dati aggiuntivi.

L'interfaccia utente di modifica (la parte superiore) conterrà un campo `<input>` e due pulsanti per annullare o salvare le modifiche:

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

Quando l'utente preme il pulsante _Modifica_, la variabile `editing` sarà impostata su `true`, e Svelte rimuoverà il markup nella parte `{:else}` del DOM e lo sostituirà con il markup nella sezione `{#if}`.

La proprietà `value` dell'`<input>` sarà legata alla variabile `name`, e i pulsanti per annullare e salvare le modifiche chiamano rispettivamente `onCancel()` e `onSave()` (abbiamo aggiunto quelle funzioni precedentement).

- Quando viene invocato `onCancel()`, `name` viene ripristinato al suo valore originale (quando viene passato come proprietà) e usciamo dalla modalità di modifica (impostando `editing` su `false`).
- Quando viene invocato `onSave()`, eseguiamo la funzione `update()` — passandole il `name` modificato — e usciamo dalla modalità di modifica.

Disabilitiamo anche il pulsante _Salva_ quando l'`<input>` è vuoto, usando l'attributo `disabled={!name}`, e permettiamo all'utente di annullare la modifica usando il tasto <kbd>Escape</kbd>, così:

```plain
on:keydown={(e) => e.key === 'Escape' && onCancel()}
```

Usiamo anche `todo.id` per creare ID unici per i nuovi controlli e etichette di input.

1. Il markup completo aggiornato del nostro componente `Todo` appare così. Aggiorna il tuo ora:

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
   > Potremmo ulteriormente suddividere questo in due componenti diversi, uno per modificare il to-do e l'altro per visualizzarlo. Alla fine, si tratta di quanto ti senti a tuo agio a gestire questo livello di complessità in un singolo componente. Dovresti anche considerare se ulteriormente suddividerlo permetterebbe di riutilizzare questo componente in un contesto diverso.

2. Per far funzionare la funzionalità di aggiornamento, dobbiamo gestire l'evento `update` dal componente `Todos`. Nella sua sezione `<script>`, aggiungi questo gestore:

   ```js
   function updateTodo(todo) {
     const i = todos.findIndex((t) => t.id === todo.id);
     todos[i] = { ...todos[i], ...todo };
   }
   ```

   Troviamo il `todo` con `id` nel nostro array `todos`, e aggiorniamo il suo contenuto usando la sintassi spread. In questo caso avremmo potuto anche solo usare `todos[i] = todo`, ma questa implementazione è più solida, permettendo al componente `Todo` di restituire solo le parti aggiornate del to-do.

3. Successivamente dobbiamo ascoltare l'evento `update` durante la nostra chiamata del componente `<Todo>`, ed eseguire la nostra funzione `updateTodo()` quando questo si verifica per cambiare il nome e lo stato di completamento. Aggiorna la tua chiamata \<Todo> così:

   ```svelte
   {#each filterTodos(filter, todos) as todo (todo.id)}
   <li class="todo">
     <Todo {todo} on:update={(e) => updateTodo(e.detail)} on:remove={(e) =>
     removeTodo(e.detail)} />
   </li>
   ```

4. Prova di nuovo la tua app, e dovresti vedere che puoi eliminare, aggiungere, modificare, annullare la modifica di e attivare/disattivare lo stato di completamento dei to-do. E la nostra intestazione di stato "x su y elementi completati" ora si aggiornerà correttamente quando i to-do sono completati.

Come puoi vedere, è facile implementare il pattern "props-down, events-up" in Svelte. Tuttavia, per componenti semplici `bind` può essere una buona scelta; Svelte ti lascerà scegliere.

> [!NOTE]
> Svelte fornisce meccanismi più avanzati per condividere informazioni tra componenti: l'[API di contesto](https://svelte.dev/docs/svelte#setcontext) e [i negozi](https://svelte.dev/docs/svelte-store). L'API di contesto fornisce un meccanismo per consentire ai componenti e ai loro discendenti di "parlare" tra di loro senza passare dati e funzioni come proprietà, o emettere molti eventi. I negozi permettono di condividere dati reattivi tra componenti che non sono gerarchicamente correlati. Ci occuperemo dei negozi più avanti nella serie.

## Il codice finora

### Git

Per vedere lo stato del codice come dovrebbe essere alla fine di questo articolo, accedi alla tua copia del nostro repository in questo modo:

```bash
cd mdn-svelte-tutorial/05-advanced-concepts
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/05-advanced-concepts
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità di sviluppo.

### REPL

Per vedere lo stato attuale del codice in un REPL, visita:

<https://svelte.dev/repl/76cc90c43a37452e8c7f70521f88b698?version=3.23.2>

## Sommario

Ora abbiamo tutte le funzionalità richieste dalla nostra app in atto. Possiamo visualizzare, aggiungere, modificare ed eliminare i to-do, segnarli come completati e filtrare per stato.

In questo articolo, abbiamo coperto i seguenti argomenti:

- Estrazione della funzionalità in un nuovo componente
- Passaggio di informazioni dal figlio al genitore utilizzando un gestore ricevuto come proprietà
- Passaggio di informazioni dal figlio al genitore utilizzando la direttiva `bind`
- Resa condizionale di blocchi di markup utilizzando il blocco `if`
- Implementazione del pattern di comunicazione "props-down, events-up"
- Creazione e ascolto di eventi personalizzati

Nel prossimo articolo continueremo a componentizzare la nostra app e approfondiremo alcune tecniche avanzate per lavorare con il DOM.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_variables_props","Learn_web_development/Core/Frameworks_libraries/Svelte_reactivity_lifecycle_accessibility", "Learn_web_development/Core/Frameworks_libraries")}}
