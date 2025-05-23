---
title: Rendering a list of Vue components
slug: Learn_web_development/Core/Frameworks_libraries/Vue_rendering_lists
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_first_component","Learn_web_development/Core/Frameworks_libraries/Vue_methods_events_models", "Learn_web_development/Core/Frameworks_libraries")}}

A questo punto abbiamo un componente completamente funzionante; siamo ora pronti per aggiungere più componenti `ToDoItem` alla nostra app. In questo articolo esamineremo come aggiungere un insieme di dati di elementi da fare al nostro componente `App.vue`, che poi cicleremo e visualizzeremo all'interno di componenti `ToDoItem` utilizzando la direttiva `v-for`.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
          conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/riga di comando</a
          >.
        </p>
        <p>
          I componenti Vue sono scritti come una combinazione di oggetti JavaScript che gestiscono i dati dell'app e una sintassi template basata su HTML che mappa la struttura DOM sottostante. Per l'installazione e per utilizzare alcune delle funzioni più avanzate di Vue (come Componenti a File Singolo o funzioni di render), è necessario un terminale con node + npm installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare come ciclare attraverso un array di dati e renderizzarlo in più componenti.
      </td>
    </tr>
  </tbody>
</table>

## Rendering di elenchi con v-for

Per essere un efficace elenco di cose da fare, dobbiamo essere in grado di renderizzare più elementi da fare. Per farlo, Vue ha una direttiva speciale, [`v-for`](https://vuejs.org/api/built-in-directives.html#v-for). Questa è una direttiva Vue incorporata che ci permette di includere un ciclo all'interno del nostro template, ripetendo il rendering di una caratteristica del template per ogni elemento di un array. Useremo questo per iterare attraverso un array di elementi da fare e mostrarli nella nostra app in componenti `ToDoItem` separati.

### Aggiungere alcuni dati da renderizzare

Per prima cosa dobbiamo ottenere un array di elementi da fare. Per farlo, aggiungeremo una proprietà `data` all'oggetto del componente `App.vue`, contenente un campo `ToDoItems` il cui valore è un array di elementi da fare. Anche se alla fine aggiungeremo un meccanismo per aggiungere nuovi elementi da fare, possiamo iniziare con alcuni mock di elementi da fare. Ogni elemento da fare sarà rappresentato da un oggetto con una proprietà `label` e una `done`.

Aggiungi alcuni esempi di elementi da fare, simili a quelli visti di seguito. In questo modo hai alcuni dati disponibili per il rendering utilizzando `v-for`.

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

Ora che abbiamo un elenco di elementi, possiamo usare la direttiva `v-for` per visualizzarli. Le direttive si applicano agli elementi come altri attributi. Nel caso di `v-for`, si utilizza una sintassi speciale simile a un ciclo [`for...in`](/it/docs/Web/JavaScript/Reference/Statements/for...in) in JavaScript — `v-for="item in items"` — dove `items` è l'array su cui si desidera iterare, e `item` è un riferimento all'elemento corrente nell'array.

`v-for` si attacca all'elemento che si desidera ripetere e rende quell'elemento e i suoi figli. In questo caso, vogliamo visualizzare un elemento `<li>` per ogni elemento da fare all'interno del nostro array `ToDoItems`. Poi vogliamo passare i dati di ogni elemento da fare a un componente `ToDoItem`.

### Attributo Key

Prima di farlo, c'è un altro elemento di sintassi da conoscere che è usato con `v-for`, l'attributo `key`. Per aiutare Vue a ottimizzare il rendering degli elementi nell'elenco, cerca di aggiungere patch agli elementi dell'elenco in modo da non ricrearli ogni volta che l'elenco cambia. Tuttavia, Vue ha bisogno di aiuto. Per assicurarsi che stia riutilizzando correttamente gli elementi dell'elenco, ha bisogno di una "chiave" unica sullo stesso elemento a cui si attacca `v-for`.

Per assicurarsi che Vue possa confrontare accuratamente gli attributi `key`, devono essere valori stringa o numerici. Anche se sarebbe fantastico usare il campo nome, questo campo sarà eventualmente controllato dall'input dell'utente, il che significa che non possiamo garantire che i nomi siano unici. Tuttavia, possiamo utilizzare `nanoid()`, come abbiamo fatto nell'articolo precedente.

1. Importa `nanoid` nel tuo componente `App` nello stesso modo in cui lo hai fatto con il tuo componente `ToDoItem`, usando

   ```js
   import { nanoid } from "nanoid";
   ```

2. Successivamente, aggiungi un campo `id` a ciascun elemento nel tuo array `ToDoItems` e assegna a ciascuno di essi un valore di `"todo-" + nanoid()`.

   L'elemento `<script>` in `App.vue` dovrebbe ora avere il seguente contenuto:

   ```js
   import ToDoItem from "./components/ToDoItem.vue";
   import { nanoid } from "nanoid";

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

3. Ora, aggiungi la direttiva `v-for` e l'attributo `key` all'elemento `<li>` nel tuo template `App.vue`, in questo modo:

   ```html
   <ul>
     <li v-for="item in ToDoItems" :key="item.id">
       <to-do-item label="My ToDo Item" :done="true"></to-do-item>
     </li>
   </ul>
   ```

   Quando apporti questa modifica, ogni espressione JavaScript tra i tag `<li>` avrà accesso al valore `item` oltre agli altri attributi del componente. Questo significa che possiamo passare i campi dei nostri oggetti item al nostro componente `ToDoItem` — basta ricordare di usare la sintassi `v-bind`. Questo è molto utile, poiché vogliamo che i nostri elementi da fare visualizzino le loro proprietà `label` come etichetta, non un'etichetta statica di "My Todo Item". Inoltre, vogliamo che il loro stato spuntato rifletta le loro proprietà `done`, non sia sempre impostato su `done="true"`.

4. Aggiorna l'attributo `label="My ToDo Item"` a `:label="item.label"`, e l'attributo `:done="true"` a `:done="item.done"`, come visto nel contesto sotto:

   ```html
   <ul>
     <li v-for="item in ToDoItems" :key="item.id">
       <to-do-item :label="item.label" :done="item.done"></to-do-item>
     </li>
   </ul>
   ```

Ora, quando guardi la tua app in esecuzione, mostrerà gli elementi da fare con i loro nomi corretti, e se ispezioni il codice sorgente, vedrai che gli input hanno tutti `id` unici, presi dall'oggetto nel componente `App`.

![L'app con un elenco di elementi da fare renderizzati.](rendered-todo-items.png)

## Opportunità per una leggera rifattorizzazione

C'è un piccolo refactoring che possiamo fare qui. Invece di generare l'`id` per i checkbox all'interno del tuo componente `ToDoItem`, possiamo trasformare l'`id` in una prop. Anche se non è strettamente necessario, rende più facile per noi gestirlo poiché dobbiamo già creare un `id` univoco per ogni elemento da fare comunque.

1. Aggiungi una nuova prop al tuo componente `ToDoItem` — `id`.
2. Rendila obbligatoria e rendi il suo tipo una `String`.
3. Per evitare collisioni di nomi, rimuovi il campo `id` dal tuo attributo `data`.
4. Non stai più usando `nanoid`, quindi devi rimuovere la linea `import { nanoid } from 'nanoid';`, altrimenti la tua app genererà un errore.

Il contenuto `<script>` nel tuo componente `ToDoItem` dovrebbe ora apparire come segue:

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

Ora, nel tuo componente `App.vue`, passa `item.id` come prop al componente `ToDoItem`. Il tuo template `App.vue` dovrebbe ora apparire come segue:

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

Quando guardi il tuo sito renderizzato, dovrebbe apparire lo stesso, ma il nostro refactoring ora significa che il nostro `id` viene preso dai dati all'interno di `App.vue` e passato in `ToDoItem` come prop, proprio come tutto il resto, quindi le cose ora sono più logiche e coerenti.

## Sommario

E questo ci porta alla fine di un altro articolo. Ora abbiamo dati di esempio in atto, e un ciclo che prende ciascun bit di dati e lo rende all'interno di un `ToDoItem` nella nostra app.

Quello di cui abbiamo veramente bisogno è la possibilità per i nostri utenti di inserire i propri elementi da fare nell'app, e per questo avremo bisogno di un `<input>` di testo, un evento che si attiva quando i dati sono inviati, un metodo da attivare all'invio per aggiungere i dati e ri-renderizzare l'elenco, e un modello per controllare i dati. Affronteremo questi aspetti nel prossimo articolo.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_first_component","Learn_web_development/Core/Frameworks_libraries/Vue_methods_events_models", "Learn_web_development/Core/Frameworks_libraries")}}
