---
title: "Aggiungere un nuovo modulo todo: eventi, metodi e modelli di Vue"
slug: Learn_web_development/Core/Frameworks_libraries/Vue_methods_events_models
l10n:
  sourceCommit: 611edf6335e4a833a6f394d0d98b117e7b0a36bf
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_rendering_lists","Learn_web_development/Core/Frameworks_libraries/Vue_styling", "Learn_web_development/Core/Frameworks_libraries")}}

Ora abbiamo dei dati di esempio in posizione e un ciclo che prende ogni bit di dati e lo rende all'interno di un `ToDoItem` nella nostra applicazione. Quello di cui abbiamo realmente bisogno ora è la capacità di permettere ai nostri utenti di inserire i propri elementi todo nell'app, e per questo avremo bisogno di un `<input>` di testo, un evento che si attiva quando i dati vengono inviati, un metodo che viene eseguito al momento dell'invio per aggiungere i dati e ri-renderizzare la lista, e un modello per controllare i dati. Questo è ciò che tratteremo in questo articolo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Familiarità con i linguaggi di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> principale,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
          conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/riga di comando</a
          >.
        </p>
        <p>
          I componenti Vue sono scritti come una combinazione di oggetti JavaScript che
          gestiscono i dati dell'app e una sintassi di template basata su HTML che si
          mappa sulla struttura del DOM sottostante. Per l'installazione, e per utilizzare alcune delle
          caratteristiche avanzate di Vue (come Single File Components o funzioni di rendering),
          avrai bisogno di un terminale con node + npm installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare a gestire i moduli in Vue, e per associazione, eventi,
        modelli, e metodi.
      </td>
    </tr>
  </tbody>
</table>

## Creare un nuovo modulo To-Do

Ora abbiamo un'app che visualizza una lista di elementi da fare. Tuttavia, non possiamo aggiornare la nostra lista di elementi senza cambiare manualmente il nostro codice! Correggiamo questo. Creiamo un nuovo componente che ci permetterà di aggiungere un nuovo elemento da fare.

1. Nella tua cartella di componenti, crea un nuovo file chiamato `ToDoForm.vue`.
2. Aggiungi un `<template>` vuoto e un tag `<script>` come prima:

   ```vue
   <template></template>

   <script>
   export default {};
   </script>
   ```

3. Aggiungiamo un modulo HTML che ti permette di inserire un nuovo elemento todo e inviarlo nell'app. Abbiamo bisogno di un [`<form>`](/it/docs/Web/HTML/Reference/Elements/form) con un [`<label>`](/it/docs/Web/HTML/Reference/Elements/label), un [`<input>`](/it/docs/Web/HTML/Reference/Elements/input), e un [`<button>`](/it/docs/Web/HTML/Reference/Elements/button). Aggiorna il tuo template come segue:

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

   Quindi ora abbiamo un componente modulo in cui possiamo inserire il titolo di un nuovo elemento todo (che diventerà un'etichetta per la corrispondente `ToDoItem` quando sarà poi reso).

4. Carichiamo questo componente nella nostra app. Torna a `App.vue` e aggiungi la seguente istruzione `import` subito sotto quella precedente, all'interno del tuo elemento `<script>`:

   ```js
   import ToDoForm from "./components/ToDoForm.vue";
   ```

5. Devi anche registrare il nuovo componente nel tuo componente `App` — aggiorna la proprietà `components` dell'oggetto del componente in modo che sembri questo:

   ```js
   export default {
     // …
     components: {
       ToDoItem,
       ToDoForm,
     },
     // …
   };
   ```

6. Infine per questa sezione, rendi il tuo componente `ToDoForm` all'interno della tua app aggiungendo l'elemento `<to-do-form />` all'interno del `<template>` della tua `App`, così:

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

Ora quando visualizzi il tuo sito in esecuzione, dovresti vedere il nuovo modulo visualizzato.

![La nostra app todo list resa con un input di testo per inserire nuovi todo](rendered-form-with-text-input.png)

Se lo compili e fai clic sul pulsante "Aggiungi", la pagina restituirà il modulo al server, ma questo non è realmente ciò che vogliamo. In realtà vogliamo eseguire un metodo sull'[evento `submit`](/it/docs/Web/API/HTMLFormElement/submit_event) che aggiungerà il nuovo todo alla lista dati `ToDoItem` definita all'interno di `App`. Per fare ciò, dovremo aggiungere un metodo all'istanza del componente.

## Creare un metodo e associarlo a un evento con v-on

Per rendere disponibile un metodo al componente `ToDoForm`, dobbiamo aggiungerlo all'oggetto del componente, e questo viene fatto all'interno di una proprietà `methods` del nostro componente, che va nello stesso posto di `data()`, `props`, ecc. La proprietà `methods` tiene qualsiasi metodo che potremmo dover chiamare nel nostro componente. Quando viene referenziata, i metodi vengono eseguiti completamente, quindi non è una buona idea usarli per visualizzare informazioni all'interno del template. Per visualizzare i dati che provengono da calcoli, dovresti usare una proprietà `computed`, che tratteremo più avanti.

1. In questo componente, dobbiamo aggiungere un metodo `onSubmit()` a una proprietà `methods` all'interno dell'oggetto componente `ToDoForm`. Useremo questo per gestire l'azione di invio.

   Aggiungi questo come segue:

   ```js
   export default {
     methods: {
       onSubmit() {
         console.log("form submitted");
       },
     },
   };
   ```

2. Successivamente, dobbiamo associare il metodo al gestore eventi `submit` del nostro elemento `<form>`. Proprio come Vue usa la sintassi [`v-bind`](https://vuejs.org/api/built-in-directives.html#v-bind) per associare gli attributi, Vue ha una direttiva speciale per la gestione degli eventi: [`v-on`](https://vuejs.org/api/built-in-directives.html#v-on). La direttiva `v-on` funziona tramite la sintassi `v-on:event="method"`. E proprio come `v-bind`, c'è anche una sintassi abbreviata: `@event="method"`.

   Useremo la sintassi abbreviata qui per coerenza. Aggiungi il gestore `submit` al tuo elemento `<form>` come segue:

   ```vue
   <form @submit="onSubmit">…</form>
   ```

3. Quando lo esegui, l'app invia ancora i dati al server, causando un aggiornamento. Poiché stiamo facendo tutta la nostra elaborazione sul client, non c'è un server che gestisca il postback. Inoltre, perdiamo tutto lo stato locale al momento dell'aggiornamento della pagina. Per impedire al browser di inviare al server, dobbiamo interrompere l'azione predefinita dell'evento mentre si propaga attraverso la pagina ([`Event.preventDefault()`](/it/docs/Web/API/Event/preventDefault), in JavaScript puro). Vue ha una sintassi speciale chiamata **modificatori di eventi** che può gestirlo direttamente nel nostro template.

   I modificatori vengono aggiunti alla fine di un evento con un punto così: `@event.modifier`. Ecco un elenco di modificatori di eventi:

   - `.stop`: Ferma la propagazione dell'evento. Equivalente a [`Event.stopPropagation()`](/it/docs/Web/API/Event/stopPropagation) negli eventi JavaScript regolari.
   - `.prevent`: Impedisce il comportamento predefinito dell'evento. Equivalente a [`Event.preventDefault()`](/it/docs/Web/API/Event/preventDefault).
   - `.self`: Attiva il gestore solo se l'evento è stato inviato da questo elemento esatto.
   - `{.key}`: Attiva il gestore evento solo tramite il tasto specificato. [MDN ha un elenco di valori di tasto validi](/it/docs/Web/API/UI_Events/Keyboard_event_key_values); i tasti a più parole devono essere convertiti in {{Glossary("kebab_case", "kebab-case")}} (es. `page-down`).
   - `.native`: Ascolta un evento nativo sull'elemento root (più esterno avvolgente) nel tuo componente.
   - `.once`: Ascolta l'evento finché non è stato attivato una volta, e poi non più.
   - `.left`: Attiva il gestore solo tramite l'evento del pulsante sinistro del mouse.
   - `.right`: Attiva il gestore solo tramite l'evento del pulsante destro del mouse.
   - `.middle`: Attiva il gestore solo tramite l'evento del pulsante centrale del mouse.
   - `.passive`: Equivalente all'utilizzo del parametro `{ passive: true }` quando si crea un ascoltatore di eventi in JavaScript puro tramite [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener).

   In questo caso, dobbiamo usare il modificatore `.prevent` per fermare l'azione di invio predefinita del browser. Aggiungi `.prevent` al gestore `@submit` nel tuo template così:

   ```vue
   <form @submit.prevent="onSubmit">…</form>
   ```

Se provi a inviare il modulo ora, noterai che la pagina non si ricarica. Se apri la console, puoi vedere i risultati del [`console.log()`](/it/docs/Web/API/console/log_static) che abbiamo aggiunto nel nostro metodo `onSubmit()`.

## Associare i dati agli input con v-model

Il prossimo passo, abbiamo bisogno di un modo per ottenere il valore dell'`<input>` del modulo, così possiamo aggiungere il nuovo elemento todo alla nostra lista dati `ToDoItems`.

La prima cosa di cui abbiamo bisogno è una proprietà `data` nel nostro modulo per tracciare il valore del todo.

1. Aggiungi un metodo `data()` al nostro oggetto componente `ToDoForm` che restituisce un campo `label`. Possiamo impostare il valore iniziale del `label` su una stringa vuota.

   Il tuo oggetto componente dovrebbe ora sembrare qualcosa di simile a questo:

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

2. Ora abbiamo bisogno di un modo per collegare il valore del campo `new-todo-input` all'elemento `label`. Vue ha una direttiva speciale per questo: [`v-model`](https://vuejs.org/api/built-in-directives.html#v-model). `v-model` si associa alla proprietà dei dati impostata su di esso e la mantiene in sincronia con l'`<input>`. `v-model` funziona su tutti i vari tipi di input, inclusi caselle di controllo, radio e select input. Per utilizzare `v-model`, aggiungi un attributo con la struttura `v-model="variabile"` all'`<input>`.

   Quindi nel nostro caso, lo aggiungeremmo al nostro campo `new-todo-input` come mostrato di seguito. Fallo ora:

   ```vue
   <input
     type="text"
     id="new-todo-input"
     name="new-todo"
     autocomplete="off"
     v-model="label" />
   ```

   > [!NOTE]
   > Puoi anche sincronizzare i dati con i valori dell'`<input>` attraverso una combinazione di eventi e attributi `v-bind`. In effetti, questo è ciò che `v-model` fa dietro le quinte. Tuttavia, l'esatta combinazione di eventi e attributi varia a seconda del tipo di input e richiederà più codice rispetto all'utilizzo del collegamento di scorciatoia `v-model`.

3. Testiamo il nostro uso di `v-model` registrando il valore dei dati inviati nel nostro metodo `onSubmit()`. Nei componenti, gli attributi di dati sono accessibili utilizzando la parola chiave `this`. Quindi accediamo al nostro campo `label` usando `this.label`.

   Aggiorna il tuo metodo `onSubmit()` per assomigliare a questo:

   ```js
   export default {
     methods: {
       onSubmit() {
         console.log("Label value: ", this.label);
       },
     },
   };
   ```

4. Ora torna alla tua app in esecuzione, aggiungi del testo al campo `<input>`, e fai clic sul pulsante "Aggiungi". Dovresti vedere il valore che hai inserito registrato nella tua console, per esempio:

   ```plain
   Label value: My value
   ```

## Cambiare il comportamento di v-model con i modificatori

In modo simile ai modificatori di eventi, possiamo anche aggiungere modificatori per cambiare il comportamento di `v-model`. Nel nostro caso, ce ne sono due da considerare. Il primo, `.trim`, rimuoverà gli spazi bianchi prima o dopo l'input. Possiamo aggiungere il modificatore alla nostra dichiarazione `v-model` in questo modo: `v-model.trim="label"`.

Il secondo modificatore che dovremmo considerare si chiama `.lazy`. Questo modificatore cambia quando `v-model` sincronizza il valore per gli input di testo. Come accennato in precedenza, la sincronizzazione di `v-model` funziona aggiornando la variabile utilizzando gli eventi. Per gli input di testo, questa sincronizzazione avviene tramite l'evento [`input`](/it/docs/Web/API/Element/input_event). Spesso, ciò significa che Vue sta sincronizzando i dati dopo ogni battitura. Il modificatore `.lazy` fa sì che `v-model` usi l'evento [`change`](/it/docs/Web/API/HTMLElement/change_event) invece. Questo significa che Vue sincronizzerà i dati solo quando l'input perde focus o il modulo viene inviato. Per i nostri scopi, questo è molto più ragionevole poiché abbiamo bisogno solo del dato finale.

Per utilizzare sia il modificatore `.lazy` che il modificatore `.trim` insieme, possiamo concatenarli, ad esempio, `v-model.lazy.trim="label"`.

Aggiorna il tuo attributo `v-model` per concatenare `lazy` e `trim` come mostrato sopra, e poi testa di nuovo la tua app. Prova, ad esempio, a inviare un valore con spazi bianchi ad ogni estremità.

## Passare dati ai genitori con eventi personalizzati

Ora siamo molto vicini a essere in grado di aggiungere nuovi elementi todo alla nostra lista. La prossima cosa che dobbiamo essere in grado di fare è passare il nuovo elemento todo creato al nostro componente `App`. Per fare questo, possiamo far emettere al nostro `ToDoForm` un evento personalizzato che passa i dati, e far ascoltare `App` su di esso. Questo funziona in modo molto simile agli eventi nativi sugli elementi HTML: un componente figlio può emettere un evento che può essere ascoltato tramite `v-on`.

Nel gestore eventi `onSubmit` del nostro `ToDoForm`, aggiungiamo un evento `todo-added`. Gli eventi personalizzati vengono emessi così: `this.$emit("event-name")`. È importante sapere che i gestori eventi sono sensibili al case e non possono includere spazi. Anche i template Vue vengono convertiti in minuscolo, il che significa che i template Vue non possono ascoltare eventi denominati con lettere maiuscole.

1. Sostituisci il `console.log()` nel metodo `onSubmit()` con il seguente:

   ```js
   this.$emit("todo-added");
   ```

2. Successivamente, torna a `App.vue` e aggiungi una proprietà `methods` al tuo oggetto componente contenente un metodo `addToDo()`, come mostrato di seguito. Per ora, questo metodo può semplicemente registrare `To-do aggiunto` nella console.

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

3. Successivamente, aggiungi un ascoltatore di eventi per l'evento `todo-added` sul `<to-do-form></to-do-form>`, che richiama il metodo `addToDo()` quando l'evento si attiva. Usando la scorciatoia `@`, l'ascoltatore sarebbe così: `@todo-added="addToDo"`:

   ```vue
   <to-do-form @todo-added="addToDo"></to-do-form>
   ```

4. Quando invii il tuo `ToDoForm`, dovresti vedere la registrazione console del metodo `addToDo()`. Questo è buono, ma non stiamo ancora passando nessun dato al componente `App.vue`. Possiamo farlo passando argomenti aggiuntivi alla funzione `this.$emit()` nel componente `ToDoForm`.

   Nel nostro caso, quando attiviamo l'evento vogliamo passare il dato `label` insieme. Questo si fa includendo i dati che vuoi passare come un altro parametro nel metodo `$emit()`: `this.$emit("todo-added", this.label)`. Questo è simile a come gli eventi JavaScript nativi includono i dati, eccetto che gli eventi personalizzati Vue non includono un oggetto evento di default. Questo significa che l'evento emesso corrisponderà direttamente a qualsiasi oggetto inviato. Quindi nel nostro caso, il nostro oggetto evento sarà solo una stringa.

   Aggiorna il tuo metodo `onSubmit()` così:

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

5. Per raccogliere effettivamente questi dati dentro `App.vue`, dobbiamo aggiungere un parametro al nostro metodo `addToDo()` che include il `label` del nuovo elemento todo.

   Torna a `App.vue` e aggiornalo ora:

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

Se testi di nuovo il tuo modulo, vedrai qualsiasi testo inserito registrato nella tua console all'invio. Vue passa automaticamente gli argomenti dopo il nome dell'evento in `this.$emit()` al tuo gestore eventi.

## Aggiungere il nuovo todo ai nostri dati

Ora che abbiamo i dati del `ToDoForm` disponibili in `App.vue`, dobbiamo aggiungere un elemento che lo rappresenta all'array `ToDoItems`. Questo può essere fatto aggiungendo un nuovo oggetto elemento todo all'array contenente i nostri nuovi dati.

1. Aggiorna il tuo metodo `addToDo()` in questo modo:

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

2. Prova di nuovo il tuo modulo, e dovresti vedere nuovi elementi todo aggiunti alla fine della lista.
3. Facciamo un ulteriore miglioramento prima di andare avanti. Se invii il modulo mentre l'input è vuoto, gli elementi todo senza testo vengono ancora aggiunti alla lista. Per risolvere questo problema, possiamo impedire all'evento todo-added di attivarsi quando il nome è vuoto. Poiché il nome viene già troncato dal modificatore `.trim`, dobbiamo solo testare la stringa vuota.

   Torna al tuo componente `ToDoForm`, e aggiorna il metodo `onSubmit()` in modo tale da evitare di emettere l'evento `todo-added` se il valore label è vuoto.

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

4. Prova di nuovo il tuo modulo. Ora non sarai in grado di aggiungere elementi vuoti alla lista todo.

![La nostra app todo list resa con un input di testo per inserire nuovi todo](rendered-form-with-new-items.png)

## Usare v-model per aggiornare un valore di input

C'è un'altra cosa da correggere nel nostro componente `ToDoForm` — dopo l'invio, l'`<input>` contiene ancora il vecchio valore. Ma questo è facile da correggere — dato che stiamo usando `v-model` per associare i dati all'`<input>` in `ToDoForm`, se impostiamo il parametro name per essere uguale a una stringa vuota, anche l'input si aggiornerà.

Aggiorna il metodo `onSubmit()` del tuo componente `ToDoForm` a questo:

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

Ora quando fai clic sul pulsante "Aggiungi", l'"new-todo-input" si svuoterà da solo.

## Sommario

Eccellente. Ora possiamo aggiungere elementi todo al nostro modulo! La nostra app inizia ora a sembrare interattiva, ma un problema è che abbiamo completamente ignorato il suo aspetto fino ad ora. Nell'articolo successivo, ci concentreremo sulla risoluzione di questo, esaminando i diversi modi in cui Vue offre per stilizzare i componenti.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_rendering_lists","Learn_web_development/Core/Frameworks_libraries/Vue_styling", "Learn_web_development/Core/Frameworks_libraries")}}
