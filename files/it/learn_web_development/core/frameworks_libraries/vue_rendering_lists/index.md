---
title: Rendering a list of Vue components
slug: Learn_web_development/Core/Frameworks_libraries/Vue_rendering_lists
l10n:
  sourceCommit: e6d43da6c6d28a6ac92cdd47882809ffbdf987ce
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_first_component","Learn_web_development/Core/Frameworks_libraries/Vue_methods_events_models", "Learn_web_development/Core/Frameworks_libraries")}}

A questo punto abbiamo un componente completamente funzionante; siamo ora pronti ad aggiungere più componenti `ToDoItem` alla nostra app. In questo articolo vedremo come aggiungere una serie di dati di elementi da fare al nostro componente `App.vue`, che poi eseguiremo in un ciclo e visualizzeremo all'interno dei componenti `ToDoItem` utilizzando la direttiva `v-for`.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Familiarità con i linguaggi principali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
          conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/riga di comando</a
          >.
        </p>
        <p>
          I componenti Vue sono scritti come una combinazione di oggetti JavaScript che
          gestiscono i dati dell'app e una sintassi di template basata su HTML che mappa
          la struttura del DOM sottostante. Per l'installazione, e per utilizzare alcune delle
          funzionalità più avanzate di Vue (come componenti a file singolo o funzioni di render),
          avrai bisogno di un terminale con node + npm installato.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare come eseguire un loop su un array di dati e renderizzarlo in più
        componenti.
      </td>
    </tr>
  </tbody>
</table>

## Renderizzare liste con v-for

Per essere una lista di cose da fare efficace, dobbiamo essere in grado di renderizzare più elementi da fare. Per fare ciò, Vue ha una direttiva speciale, [`v-for`](https://vuejs.org/api/built-in-directives.html#v-for). Questa è una direttiva Vue integrata che ci consente di includere un ciclo all'interno del nostro template, ripetendo il rendering di una caratteristica del template per ciascun elemento in un array. Useremo questo per iterare attraverso un array di elementi da fare e visualizzarli nella nostra app in separati componenti `ToDoItem`.

### Aggiunta di alcuni dati da renderizzare

Prima di tutto, abbiamo bisogno di ottenere un array di elementi da fare. Per fare ciò, aggiungeremo una proprietà `data` all'oggetto del componente `App.vue`, contenente un campo `ToDoItems` il cui valore è un array di elementi da fare. Anche se alla fine aggiungeremo un meccanismo per aggiungere nuovi elementi da fare, possiamo iniziare con alcuni elementi fittizi. Ogni elemento da fare sarà rappresentato da un oggetto con una proprietà `label` e una `done`.

Aggiungi alcuni elementi da fare di esempio, simili a quelli mostrati sotto. In questo modo avrai alcuni dati disponibili per il rendering usando `v-for`.

```js
export default {
  name: "app",
  components: {
    ToDoItem,
  },
  data() {
    return {
      ToDoItems: [
        { label: "Learn Vue", done: false },
        { label: "Create a Vue project with the CLI", done: true },
        { label: "Have fun", done: true },
        { label: "Create a to-do list", done: false },
      ],
    };
  },
};
```

Ora che abbiamo un elenco di elementi, possiamo usare la direttiva `v-for` per visualizzarli. Le direttive sono applicate agli elementi come altri attributi. Nel caso di `v-for`, usi una sintassi speciale simile a un ciclo [`for...in`](/it/docs/Web/JavaScript/Reference/Statements/for...in) in JavaScript — `v-for="item in items"` — dove `items` è l'array su cui vuoi iterare, e `item` è un riferimento all'elemento corrente nell'array.

`v-for` si attacca all'elemento che vuoi ripetere e renderizza quell'elemento e i suoi figli. In questo caso, vogliamo visualizzare un elemento `<li>` per ogni elemento da fare all'interno del nostro array `ToDoItems`. Quindi vogliamo passare i dati di ciascun elemento da fare a un componente `ToDoItem`.

### Attributo Key

Prima di farlo, c'è un altro elemento sintattico da conoscere che viene usato con `v-for`, l'attributo `key`. Per aiutare Vue a ottimizzare il rendering degli elementi nell'elenco, cerca di correggere gli elementi dell'elenco in modo da non doverli ricreare ogni volta che l'elenco cambia. Tuttavia, Vue ha bisogno di aiuto. Per garantire che riutilizzi in modo appropriato gli elementi dell'elenco, ha bisogno di un unico "key" sullo stesso elemento a cui si attacca `v-for`.

Per garantire che Vue possa confrontare accuratamente gli attributi `key`, devono essere valori stringa o numerici. Anche se sarebbe fantastico usare il campo name, questo campo sarà eventualmente controllato dall'input dell'utente, il che significa che non possiamo garantire che i nomi siano unici. Possiamo però utilizzare `nanoid()`, come abbiamo fatto nell'articolo precedente.

1. Importa `nanoid` nel tuo componente `App` nello stesso modo in cui hai fatto con il tuo componente `ToDoItem`, usando

   ```js
   import { nanoid } from "nanoid";
   ```

2. Successivamente, aggiungi un campo `id` a ciascun elemento nel tuo array `ToDoItems` e assegna a ciascuno di essi un valore di `"todo-" + nanoid()`.

   L'elemento `<script>` in `App.vue` dovrebbe ora avere il seguente contenuto:

   ```js
   import { nanoid } from "nanoid";
   import ToDoItem from "./components/ToDoItem.vue";

   export default {
     name: "app",
     components: {
       ToDoItem,
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
   };
   ```

3. Ora, aggiungi la direttiva `v-for` e l'attributo `key` all'elemento `<li>` nel tuo template `App.vue`, come segue:

   ```html
   <ul>
     <li v-for="item in ToDoItems" :key="item.id">
       <to-do-item label="My ToDo Item" :done="true"></to-do-item>
     </li>
   </ul>
   ```

   Quando apporti questa modifica, ogni espressione JavaScript tra i tag `<li>` avrà accesso al valore `item` oltre agli altri attributi del componente. Ciò significa che possiamo passare i campi dei nostri oggetti item al nostro componente `ToDoItem` — ricorda solo di usare la sintassi `v-bind`. Questo è molto utile, poiché vogliamo che i nostri elementi da fare visualizzino le loro proprietà `label` come loro etichetta, non un'etichetta statica di "My Todo Item". Inoltre, vogliamo che il loro stato di selezione rifletta le loro proprietà `done`, non essere sempre impostato su `done="true"`.

4. Aggiorna l'attributo `label="My ToDo Item"` a `:label="item.label"`, e l'attributo `:done="true"` a `:done="item.done"`, come si vede nel contesto qui sotto:

   ```html
   <ul>
     <li v-for="item in ToDoItems" :key="item.id">
       <to-do-item :label="item.label" :done="item.done"></to-do-item>
     </li>
   </ul>
   ```

Ora, quando guardi la tua app in esecuzione, mostrerà gli elementi da fare con i loro nomi corretti, e se ispezioni il codice sorgente vedrai che tutti gli input hanno `id` unici, presi dall'oggetto nel componente `App`.

![L'app con un elenco di elementi da fare renderizzati.](rendered-todo-items.png)

## Opportunità per un leggero refactor

C'è un piccolo refactoring che possiamo fare qui. Invece di generare l'`id` per le checkbox all'interno del tuo componente `ToDoItem`, possiamo trasformare l'`id` in un prop. Anche se questo non è strettamente necessario, rende più facile per noi gestirlo poiché già dobbiamo creare un `id` unico per ciascun elemento da fare.

1. Aggiungi una nuova prop al tuo componente `ToDoItem` — `id`.
2. Rendila obbligatoria, e imposta il suo tipo su `String`.
3. Per prevenire conflitti di nome, rimuovi il campo `id` dal tuo attributo `data`.
4. Non stai più usando `nanoid`, quindi devi rimuovere la riga `import { nanoid } from 'nanoid';`, altrimenti la tua app genererà un errore.

Il contenuto di `<script>` nel tuo componente `ToDoItem` dovrebbe ora assomigliare a questo:

```js
export default {
  props: {
    label: { required: true, type: String },
    done: { default: false, type: Boolean },
    id: { required: true, type: String },
  },
  data() {
    return {
      isDone: this.done,
    };
  },
};
```

Ora, nel tuo componente `App.vue`, passa `item.id` come prop al componente `ToDoItem`. Il tuo template `App.vue` dovrebbe ora assomigliare a questo:

```html
<template>
  <div id="app">
    <h1>My To-Do List</h1>
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

Quando guardi il tuo sito renderizzato, dovrebbe sembrare lo stesso, ma il nostro refactor ora significa che il nostro `id` viene preso dai dati all'interno di `App.vue` e passato in `ToDoItem` come prop, proprio come tutto il resto, quindi le cose ora sono più logiche e coerenti.

## Sintesi

E questo ci porta alla fine di un altro articolo. Ora abbiamo dati di esempio in posizione, e un ciclo che prende ogni bit di dati e lo renderizza all'interno di un `ToDoItem` nella nostra app.

Quello di cui abbiamo realmente bisogno ora è la possibilità di consentire ai nostri utenti di inserire i propri elementi da fare nell'app, e per questo avremo bisogno di un `<input>` di testo, un evento da attivare quando i dati vengono inviati, un metodo da attivare all'invio per aggiungere i dati e rirendere l'elenco, e un modello per controllare i dati. Tratteremo questi aspetti nel prossimo articolo.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_first_component","Learn_web_development/Core/Frameworks_libraries/Vue_methods_events_models", "Learn_web_development/Core/Frameworks_libraries")}}
