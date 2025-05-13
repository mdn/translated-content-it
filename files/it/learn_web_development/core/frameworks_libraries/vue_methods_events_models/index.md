---
title: "Aggiungere un nuovo modulo todo: eventi, metodi e modelli in Vue"
slug: Learn_web_development/Core/Frameworks_libraries/Vue_methods_events_models
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_rendering_lists","Learn_web_development/Core/Frameworks_libraries/Vue_styling", "Learn_web_development/Core/Frameworks_libraries")}}

Ora abbiamo dei dati di esempio in posto, e un loop che prende ciascun dato e lo rende all'interno di un `ToDoItem` nella nostra app. Quello di cui abbiamo veramente bisogno ora è la capacità di permettere ai nostri utenti di inserire i propri elementi todo nell'app, e per questo avremo bisogno di un `<input>` di testo, di un evento da lanciare quando i dati vengono inviati, di un metodo da eseguire all'invio per aggiungere i dati e rirendere la lista, e di un modello per controllare i dati. Questo è ciò che copriremo in questo articolo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Familiarità con i linguaggi core <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, 
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e 
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, conoscenza del 
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/linea di comando</a
          >.
        </p>
        <p>
          I componenti Vue sono scritti come una combinazione di oggetti JavaScript che gestiscono i dati dell'app e una sintassi template basata su HTML che mappa la struttura DOM sottostante. Per l'installazione e per utilizzare alcune delle funzionalità più avanzate di Vue (come i componenti a file singolo o le funzioni di rendering), avrà bisogno di un terminale con node + npm installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare a gestire i moduli in Vue, e per associazione, eventi, modelli e metodi.
      </td>
    </tr>
  </tbody>
</table>

## Creare un nuovo modulo To-Do

Ora abbiamo un'app che visualizza un elenco di elementi to-do. Tuttavia, non possiamo aggiornare la nostra lista di elementi senza modificare manualmente il nostro codice! Sistemiamo questo problema. Creiamo un nuovo componente che ci permetterà di aggiungere un nuovo elemento to-do.

1. Nella sua cartella dei componenti, crei un nuovo file chiamato `ToDoForm.vue`.
2. Aggiunga un `<template>` vuoto e un tag `<script>` come prima:

   ```vue
   <template></template>

   <script>
   export default {};
   </script>
   ```

3. Aggiungiamo un modulo HTML che le permetta di inserire un nuovo elemento todo e inviargli nell'app. Abbiamo bisogno di un [`<form>`](/it/docs/Web/HTML/Reference/Elements/form) con un [`<label>`](/it/docs/Web/HTML/Reference/Elements/label), un [`<input>`](/it/docs/Web/HTML/Reference/Elements/input) e un [`<button>`](/it/docs/Web/HTML/Reference/Elements/button). Aggiorna il suo template come segue:

   ```vue
   <template>
     <form>
       <label for="new-todo-input"> What needs to be done? </label>
       <input
         type="text"
         id="new-todo-input"
         name="new-todo"
         autocomplete="off" />
       <button type="submit">Add</button>
     </form>
   </template>
   ```

   Quindi ora abbiamo un componente form nel quale possiamo inserire il titolo di un nuovo elemento todo (che diventerà un'etichetta per il `ToDoItem` corrispondente quando sarà eventualmente renderizzato).

4. Carichiamo questo componente nella nostra app. Torni in `App.vue` e aggiunga la seguente istruzione `import` appena sotto quella precedente, all'interno del suo elemento `<script>`:

   ```js
   import ToDoForm from "./components/ToDoForm.vue";
   ```

5. Deve anche registrare il nuovo componente nel suo componente `App` — aggiorni la proprietà `components` dell'oggetto componente in modo che appaia così:

   ```js
   components: {
     ToDoItem, ToDoForm,
   }
   ```

6. Infine per questa sezione, renderizzi il suo componente `ToDoForm` all'interno della sua app aggiungendo l'elemento `<to-do-form />` all'interno del `<template>` della sua `App`, in questo modo:

   ```vue
   <template>
     <div id="app">
       <h1>My To-Do List</h1>
       <to-do-form></to-do-form>
       <ul>
         <li v-for="item in ToDoItems" :key="item.id">
           <to-do-item
             :label="item.label"
             :done="item.done"
             :id="item.id"></to-do-item>
         </li>
       </ul>
     </div>
   </template>
   ```

Ora quando visualizzerà il suo sito in esecuzione, dovrebbe vedere il nuovo form visualizzato.

![L'app della lista todo renderizzata con un input di testo per inserire nuovi todo](rendered-form-with-text-input.png)

Se lo compila e fa clic sul pulsante "Add", la pagina invierà il modulo al server, ma questo non è veramente ciò che vogliamo. Quello che vogliamo realmente fare è eseguire un metodo sull'[evento `submit`](/it/docs/Web/API/HTMLFormElement/submit_event) che aggiungerà il nuovo todo alla lista di dati `ToDoItem` definita all'interno di `App`. Per fare questo, dovremo aggiungere un metodo all'istanza del componente.

## Creare un metodo e collegarlo a un evento con v-on

Per rendere un metodo disponibile per il componente `ToDoForm`, dobbiamo aggiungerlo all'oggetto del componente, e questo è fatto all'interno di una proprietà `methods` nel nostro componente, che va nello stesso posto di `data()`, `props`, ecc. La proprietà `methods` contiene qualunque metodo potremmo dover chiamare nel nostro componente. Quando sono referenziati, i metodi vengono completamente eseguiti, quindi non è una buona idea usarli per visualizzare informazioni all'interno del template. Per visualizzare dati provenienti da calcoli, dovrà usare una proprietà `computed`, di cui ci occuperemo più avanti.

1. In questo componente, dobbiamo aggiungere un metodo `onSubmit()` a una proprietà `methods` all'interno dell'oggetto del componente `ToDoForm`. Lo useremo per gestire l'azione di invio.

   Aggiunga questo come segue:

   ```js
   export default {
     methods: {
       onSubmit() {
         console.log("form submitted");
       },
     },
   };
   ```

2. Successivamente dobbiamo collegare il metodo al gestore dell'evento `submit` del nostro elemento `<form>`. Così come Vue utilizza la sintassi [`v-bind`](https://vuejs.org/api/built-in-directives.html#v-bind) per collegare gli attributi, Vue ha una direttiva speciale per la gestione degli eventi: [`v-on`](https://vuejs.org/api/built-in-directives.html#v-on). La direttiva `v-on` funziona attraverso la sintassi `v-on:event="method"`. E proprio come `v-bind`, c'è anche una sintassi abbreviata: `@event="method"`.

   Utilizzeremo la sintassi abbreviata qui per coerenza. Aggiungi il gestore `submit` al suo elemento `<form>` come segue:

   ```vue
   <form @submit="onSubmit">…</form>
   ```

3. Quando esegue questo, l'app invia comunque i dati al server, causando un aggiornamento. Poiché stiamo svolgendo tutto il nostro processo sul client, non c'è alcun server per gestire il postback. Inoltre, perdiamo tutto lo stato locale all'aggiornamento della pagina. Per impedire al browser di inviare i dati al server, dobbiamo interrompere l'azione predefinita dell'evento mentre propagandosi attraverso la pagina ([`Event.preventDefault()`](/it/docs/Web/API/Event/preventDefault), in JavaScript puro). Vue ha una sintassi speciale chiamata **modificatori di evento** che può gestire questo direttamente nel nostro template.

   I modificatori vengono aggiunti alla fine di un evento con un punto così: `@event.modifier`. Ecco un elenco di modificatori di evento:

   - `.stop`: Interrompe la propagazione dell'evento. Equivalente a [`Event.stopPropagation()`](/it/docs/Web/API/Event/stopPropagation) nei normali eventi JavaScript.
   - `.prevent`: Impedisce il comportamento predefinito dell'evento. Equivalente a [`Event.preventDefault()`](/it/docs/Web/API/Event/preventDefault).
   - `.self`: Esegue il gestore solo se l'evento è stato inviato da questo esatto elemento.
   - `{.key}`: Esegue il gestore dell'evento solo tramite il tasto specificato. [MDN ha un elenco di valori di tasti validi](/it/docs/Web/API/UI_Events/Keyboard_event_key_values); i tasti con più parole devono solo essere convertiti in {{Glossary("kebab_case", "kebab-case")}} (es. `page-down`).
   - `.native`: Ascolta un evento nativo sull'elemento radice (più esterno) avvolgente nel suo componente.
   - `.once`: Ascolta l'evento finché non è stato attivato una volta e poi non più.
   - `.left`: Esegue il gestore solo tramite l'evento del pulsante sinistro del mouse.
   - `.right`: Esegue il gestore solo tramite l'evento del pulsante destro del mouse.
   - `.middle`: Esegue il gestore solo tramite l'evento del pulsante centrale del mouse.
   - `.passive`: Equivalente a usare il parametro `{ passive: true }` quando si crea un listener di evento in JavaScript puro usando [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener).

   In questo caso, dobbiamo usare il modificatore `.prevent` per impedire l'azione di invio predefinita del browser. Aggiungi `.prevent` al gestore `@submit` nel suo template come segue:

   ```vue
   <form @submit.prevent="onSubmit">…</form>
   ```

Se prova a inviare il modulo ora, noterà che la pagina non si ricarica. Se apre la console, può vedere i risultati del [`console.log()`](/it/docs/Web/API/console/log_static) che abbiamo aggiunto all'interno del nostro metodo `onSubmit()`.

## Collegare dati agli input con v-model

Successivamente, abbiamo bisogno di un modo per ottenere il valore dall'`<input>` del modulo in modo da poter aggiungere il nuovo elemento to-do alla nostra lista di dati `ToDoItems`.

La prima cosa di cui abbiamo bisogno è una proprietà `data` nel nostro form per tenere traccia del valore del to-do.

1. Aggiunga un metodo `data()` al nostro oggetto componente `ToDoForm` che restituisce un campo `label`. Possiamo impostare il valore iniziale di `label` come una stringa vuota.

   L'oggetto componente dovrebbe ora apparire qualcosa del genere:

   ```js
   export default {
     methods: {
       onSubmit() {
         console.log("form submitted");
       },
     },
     data() {
       return {
         label: "",
       };
     },
   };
   ```

2. Ora abbiamo bisogno di un modo per collegare il valore del campo `new-todo-input` al campo `label`. Vue ha una direttiva speciale per questo: [`v-model`](https://vuejs.org/api/built-in-directives.html#v-model). `v-model` si collega alla proprietà del dato che imposta su di esso e la mantiene sincronizzata con l'`<input>`. `v-model` funziona su tutti i vari tipi di input, incluse le checkboxes, i radio e gli input select. Per utilizzare `v-model`, aggiunga un attributo con la struttura `v-model="variable"` all'`<input>`.

   Quindi nel nostro caso, lo aggiungeremmo al nostro campo `new-todo-input` come visto sotto. Fai questo ora:

   ```vue
   <input
     type="text"
     id="new-todo-input"
     name="new-todo"
     autocomplete="off"
     v-model="label" />
   ```

   > [!NOTE]
   > È anche possibile sincronizzare i dati con i valori dell'`<input>` attraverso una combinazione di eventi e attributi `v-bind`. In realtà, questo è ciò che `v-model` fa dietro le quinte. Tuttavia, l'esatta combinazione di eventi e attributi varia a seconda dei tipi di input e richiederà più codice rispetto all'utilizzo del comodo `v-model`.

3. Proviamo l'uso di `v-model` registrando il valore dei dati inviati nel nostro metodo `onSubmit()`. Nei componenti, gli attributi dei dati sono accessibili utilizzando la parola chiave `this`. Quindi accediamo al nostro campo `label` usando `this.label`.

   Aggiorna il suo metodo `onSubmit()` per apparire così:

   ```js
   methods: {
     onSubmit() {
       console.log('Label value: ', this.label);
     }
   },
   ```

4. Ora torni alla sua app in esecuzione, aggiunga un po' di testo nel campo dell'`<input>`, e faccia clic sul pulsante "Add". Dovrebbe vedere il valore che ha inserito registrato nella sua console, per esempio:

   ```plain
   Label value: My value
   ```

## Modificare il comportamento di v-model con modificatori

In modo simile ai modificatori di evento, possiamo anche aggiungere modificatori per modificare il comportamento di `v-model`. Nel nostro caso, ce ne sono due che vale la pena considerare. Il primo, `.trim`, rimuoverà gli spazi bianchi dall'inizio o dalla fine dell'input. Possiamo aggiungere il modificatore alla dichiarazione `v-model` così: `v-model.trim="label"`.

Il secondo modificatore che dovremmo considerare si chiama `.lazy`. Questo modificatore cambia quando `v-model` sincronizza il valore per gli input di testo. Come menzionato in precedenza, la sincronizzazione di `v-model` funziona aggiornando la variabile utilizzando gli eventi. Per gli input di testo, questa sincronizzazione avviene utilizzando l'[evento `input`](/it/docs/Web/API/Element/input_event). Spesso, questo significa che Vue sta sincronizzando i dati dopo ogni battuta di tasto. Il modificatore `.lazy` fa sì che `v-model` usi l'[evento `change`](/it/docs/Web/API/HTMLElement/change_event) invece. Questo significa che Vue sincronizzerà i dati solo quando l'input perde il focus o il modulo viene inviato. Per i nostri scopi, questo è molto più ragionevole poiché ci serve solo il dato finale.

Per usare insieme il modificatore `.lazy` e il modificatore `.trim`, possiamo concatenarli, ad esempio, `v-model.lazy.trim="label"`.

Aggiorna il suo attributo `v-model` concatenando `lazy` e `trim` come mostrato sopra, e poi testi la sua app di nuovo. Provi, per esempio, a inviare un valore con spazi bianchi a ciascuna estremità.

## Passare dati ai genitori con eventi personalizzati

Ora siamo molto vicini all'essere in grado di aggiungere nuovi elementi to-do alla nostra lista. La prossima cosa che dobbiamo essere in grado di fare è passare il nuovo elemento to-do creato al nostro componente `App`. Per fare ciò, possiamo far emettere al nostro `ToDoForm` un evento personalizzato che passa i dati, e far sì che `App` lo ascolti. Questo funziona in modo molto simile agli eventi nativi sugli elementi HTML: un componente figlio può emettere un evento che può essere ascoltato tramite `v-on`.

Nel gestore dell'evento `onSubmit` del nostro `ToDoForm`, aggiungiamo un evento `todo-added`. Gli eventi personalizzati sono emessi così: `this.$emit("event-name")`. È importante sapere che i gestori degli eventi sono sensibili al maiuscole-minuscole e non possono includere spazi. Anche i template di Vue vengono convertiti in minuscolo, il che significa che i template di Vue non possono ascoltare eventi chiamati con lettere maiuscole.

1. Sostituisci il `console.log()` nel metodo `onSubmit()` con quanto segue:

   ```js
   this.$emit("todo-added");
   ```

2. Successivamente, torni a `App.vue` e aggiunga una proprietà `methods` al suo oggetto componente contenente un metodo `addToDo()`, come mostrato di seguito. Per ora, questo metodo può solo registrare `To-do added` sulla console.

   ```js
   export default {
     name: "app",
     components: {
       ToDoItem,
       ToDoForm,
     },
     data() {
       return {
         ToDoItems: [
           { id: "todo-" + nanoid(), label: "Learn Vue", done: false },
           {
             id: "todo-" + nanoid(),
             label: "Create a Vue project with the CLI",
             done: true,
           },
           { id: "todo-" + nanoid(), label: "Have fun", done: true },
           {
             id: "todo-" + nanoid(),
             label: "Create a to-do list",
             done: false,
           },
         ],
       };
     },
     methods: {
       addToDo() {
         console.log("To-do added");
       },
     },
   };
   ```

3. Successivamente, aggiungi un listener per l'evento `todo-added` al `<to-do-form></to-do-form>`, che chiama il metodo `addToDo()` quando l'evento viene attivato. Utilizzando l'abbreviazione `@`, il listener apparirebbe così: `@todo-added="addToDo"`:

   ```vue
   <to-do-form @todo-added="addToDo"></to-do-form>
   ```

4. Quando invia il suo `ToDoForm`, dovrebbe vedere il log della console dal metodo `addToDo()`. Questo è buono, ma non stiamo ancora passando alcun dato al componente `App.vue`. Possiamo fare ciò passando ulteriori argomenti alla funzione `this.$emit()` nel componente `ToDoForm`.

   In questo caso, quando attiviamo l'evento vogliamo passare i dati `label` insieme ad esso. Questo è fatto includendo i dati che vogliamo passare come un altro parametro nel metodo `$emit()`: `this.$emit("todo-added", this.label)`. Questo è simile a come gli eventi JavaScript nativi includono dati, tranne che gli eventi personalizzati di Vue non includono un oggetto evento di default. Questo significa che l'evento emesso corrisponderà direttamente a qualsiasi oggetto si invii. Quindi nel nostro caso, il nostro oggetto evento sarà solo una stringa.

   Aggiorna il suo metodo `onSubmit()` in questo modo:

   ```js
   export default {
     // …
     methods: {
       // …
       onSubmit() {
         this.$emit("todo-added", this.label);
       },
       // …
     },
     // …
   };
   ```

5. Per effettivamente raccogliere questi dati all'interno di `App.vue`, abbiamo bisogno di aggiungere un parametro al nostro metodo `addToDo()` che includa il `label` del nuovo elemento to-do.

   Torni a `App.vue` e lo aggiorni ora:

   ```js
   export default {
     // …
     methods: {
       // …
       addToDo(toDoLabel) {
         console.log("To-do added:", toDoLabel);
       },
       // …
     },
     // …
   };
   ```

Se testi il suo modulo di nuovo, vedrà qualsiasi testo immesso registrato nella sua console all'invio. Vue passa automaticamente gli argomenti dopo il nome dell'evento in `this.$emit()` al suo gestore dell'evento.

## Aggiungere il nuovo todo nei nostri dati

Ora che abbiamo i dati dal `ToDoForm` disponibili in `App.vue`, dobbiamo aggiungere un elemento che lo rappresenta all'array `ToDoItems`. Questo può essere fatto inserendo un nuovo oggetto articolo to-do nell'array contenente i nostri nuovi dati.

1. Aggiorna il suo metodo `addToDo()` in questo modo:

   ```js
   export default {
     // …
     methods: {
       // …
       addToDo(toDoLabel) {
         this.ToDoItems.push({
           id: "todo-" + nanoid(),
           label: toDoLabel,
           done: false,
         });
       },
       // …
     },
     // …
   };
   ```

2. Provi a testare il suo modulo di nuovo, e dovrebbe vedere i nuovi articoli to-do appenθersi alla fine della lista.
3. Facciamo un ulteriore miglioramento prima di proseguire. Se invia il modulo mentre l'input è vuoto, gli elementi to-do senza testo vengono ancora aggiunti all'elenco. Per risolvere questo problema, possiamo evitare che l'evento todo-added venga attivato quando il nome è vuoto. Poiché il nome viene già eliminato dal modificatore `.trim`, dobbiamo solo testare per la stringa vuota.

   Torni al suo componente `ToDoForm`, e aggiorni il metodo `onSubmit()` così. Se il valore del label è vuoto, non emettiamo l'evento `todo-added`.

   ```js
   export default {
     // …
     methods: {
       // …
       onSubmit() {
         if (this.label === "") {
           return;
         }
         this.$emit("todo-added", this.label);
       },
       // …
     },
     // …
   };
   ```

4. Provi di nuovo il suo modulo. Ora non sarà possibile aggiungere elementi vuoti alla lista to-do.

![L'app della lista todo renderizzata con un input di testo per inserire nuovi todo](rendered-form-with-new-items.png)

## Usare v-model per aggiornare un valore di input

C'è un'altra cosa da sistemare nel nostro componente `ToDoForm` — dopo l'invio, l'`<input>` contiene ancora il vecchio valore. Ma questo è facile da risolvere — poiché stiamo usando `v-model` per legare i dati all'`<input>` in `ToDoForm`, se impostiamo il parametro name per essere uguale a una stringa vuota, l'input si aggiornerà anche.

Aggiorna il metodo `onSubmit()` del suo componente `ToDoForm` a questo:

```js
export default {
  // …
  methods: {
    // …
    onSubmit() {
      if (this.label === "") {
        return;
      }
      this.$emit("todo-added", this.label);
      this.label = "";
    },
    // …
  },
  // …
};
```

Ora quando clicca il pulsante "Add", l'input "new-todo-input" si svuoterà.

## Riepilogo

Eccellente. Possiamo ora aggiungere elementi todo al nostro modulo! La nostra app ora inizia a sembrare interattiva, ma un problema è che abbiamo completamente ignorato il suo aspetto e la sua sensazione fino ad ora. Nel prossimo articolo, ci concentreremo a sistemare questo aspetto, esaminando i diversi modi in cui Vue fornisce per stilizzare i componenti.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_rendering_lists","Learn_web_development/Core/Frameworks_libraries/Vue_styling", "Learn_web_development/Core/Frameworks_libraries")}}
