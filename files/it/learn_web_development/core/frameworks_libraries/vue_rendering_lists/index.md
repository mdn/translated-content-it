---
title: Rendering a list of Vue components
slug: Learn_web_development/Core/Frameworks_libraries/Vue_rendering_lists
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_first_component","Learn_web_development/Core/Frameworks_libraries/Vue_methods_events_models", "Learn_web_development/Core/Frameworks_libraries")}}

A questo punto abbiamo un componente completamente funzionante; siamo ora pronti per aggiungere più componenti `ToDoItem` alla nostra app. In questo articolo vedremo come aggiungere un set di dati di elementi da fare al nostro componente `App.vue`, che poi scorreremo e mostreremo all'interno dei componenti `ToDoItem` usando la direttiva `v-for`.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Familiarità con i linguaggi core <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
          conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminal/linea di comando</a
          >.
        </p>
        <p>
          I componenti Vue sono scritti come una combinazione di oggetti JavaScript che
          gestiscono i dati dell'app e una sintassi di template basata su HTML che mappa
          la struttura DOM sottostante. Per l'installazione, e per utilizzare alcune delle
          funzioni avanzate di Vue (come Componenti a File Singolo o funzioni di rendering),
          avrete bisogno di un terminal con node + npm installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare come scorrere un array di dati e renderizzarlo in più
        componenti.
      </td>
    </tr>
  </tbody>
</table>

## Rendering di liste con v-for

Per essere una lista di cose da fare efficace, abbiamo bisogno di essere in grado di renderizzare più elementi da fare. Per farlo, Vue dispone di una direttiva speciale, [`v-for`](https://vuejs.org/api/built-in-directives.html#v-for). Questa è una direttiva integrata di Vue che ci permette di includere un ciclo all'interno del nostro template, ripetendo il rendering di una funzione di template per ogni elemento in un array. La utilizzeremo per iterare attraverso un array di elementi da fare e mostrarli nella nostra app in componenti `ToDoItem` separati.

### Aggiungere alcuni dati da renderizzare

Prima di tutto, abbiamo bisogno di ottenere un array di elementi da fare. Per farlo, aggiungeremo una proprietà `data` all'oggetto del componente `App.vue`, contenente un campo `ToDoItems` il cui valore è un array di elementi da fare. Anche se alla fine aggiungeremo un meccanismo per inserire nuovi elementi da fare, possiamo iniziare con alcuni elementi finti. Ogni elemento da fare sarà rappresentato da un oggetto con una proprietà `label` e una proprietà `done`.

Aggiungete alcuni esempi di elementi da fare, come quelli visti qui sotto. In questo modo avrete a disposizione alcuni dati per il rendering usando `v-for`.

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

Ora che abbiamo una lista di elementi, possiamo usare la direttiva `v-for` per mostrarli. Le direttive sono applicate agli elementi come altri attributi. Nel caso di `v-for`, si utilizza una sintassi speciale simile a un ciclo [`for...in`](/it/docs/Web/JavaScript/Reference/Statements/for...in) in JavaScript — `v-for="item in items"` — dove `items` è l'array su cui si vuole iterare, e `item` è un riferimento all'elemento corrente nell'array.

`v-for` si allega all'elemento che si desidera ripetere, e renderizza quell'elemento e i suoi figli. In questo caso, vogliamo mostrare un elemento `<li>` per ogni elemento da fare all'interno del nostro array `ToDoItems`. Successivamente vogliamo passare i dati da ciascun elemento da fare a un componente `ToDoItem`.

### Attributo chiave

Prima di farlo, c'è un altro pezzo di sintassi da conoscere che è usato con `v-for`, l'attributo `key`. Per aiutare Vue a ottimizzare il rendering degli elementi nella lista, cerca di modificare gli elementi della lista in modo che non li stia ricreando ogni volta che la lista cambia. Tuttavia, Vue ha bisogno di aiuto. Per assicurarsi che stia riutilizzando adeguatamente gli elementi della lista, ha bisogno di un "key" unico sullo stesso elemento a cui si collega `v-for`.

Per assicurarsi che Vue possa confrontare accuratamente gli attributi `key`, essi devono essere valori stringa o numerici. Anche se sarebbe ideale utilizzare il campo nome, questo campo sarà eventualmente controllato dall'input dell'utente, il che significa che non possiamo garantire che i nomi siano unici. Tuttavia, possiamo utilizzare `nanoid()`, come abbiamo fatto nell'articolo precedente.

1. Importi `nanoid` nel suo componente `App` nello stesso modo in cui ha fatto con il suo componente `ToDoItem`, usando

   ```js
   import { nanoid } from "nanoid";
   ```

2. Successivamente, aggiunga un campo `id` a ciascun elemento nel suo array `ToDoItems`, e assegni a ciascuno di essi un valore di `"todo-" + nanoid()`.

   L'elemento `<script>` in `App.vue` dovrebbe ora contenere il seguente contenuto:

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

3. Ora, aggiunga la direttiva `v-for` e l'attributo `key` all'elemento `<li>` nel suo template `App.vue`, come segue:

   ```html
   <ul>
     <li v-for="item in ToDoItems" :key="item.id">
       <to-do-item label="My ToDo Item" :done="true"></to-do-item>
     </li>
   </ul>
   ```

   Quando apporta questa modifica, ogni espressione JavaScript tra i tag `<li>` avrà accesso al valore `item` oltre ad altri attributi del componente. Ciò significa che possiamo passare i campi dei nostri oggetti elemento al nostro componente `ToDoItem` — basta ricordarsi di usare la sintassi `v-bind`. Questo risulta molto utile, poiché vogliamo che i nostri elementi da fare visualizzino le loro proprietà `label` come etichetta, e non un'etichetta statica di "My Todo Item". Inoltre, vogliamo che il loro stato di controllo rifletta le loro proprietà `done`, e non sia sempre impostato su `done="true"`.

4. Aggiorni l'attributo `label="My ToDo Item"` a `:label="item.label"`, e l'attributo `:done="true"` a `:done="item.done"`, come visto nel contesto qui sotto:

   ```html
   <ul>
     <li v-for="item in ToDoItems" :key="item.id">
       <to-do-item :label="item.label" :done="item.done"></to-do-item>
     </li>
   </ul>
   ```

Ora, quando guarda la sua app in esecuzione, vedrà gli elementi da fare con i loro nomi corretti, e se ispeziona il codice sorgente vedrà che gli inputs hanno tutti `id` unici, presi dall'oggetto nel componente `App`.

![L'app con una lista di elementi da fare renderizzati.](rendered-todo-items.png)

## Opportunità per un lieve refactoring

C'è un piccolo refactoring che possiamo fare qui. Invece di generare l'`id` per le checkbox all'interno del suo componente `ToDoItem`, possiamo trasformare l'`id` in una prop. Anche se questo non è strettamente necessario, lo rende più facile da gestire poiché dobbiamo comunque già creare un `id` univoco per ciascun elemento da fare.

1. Aggiunga una nuova prop al suo componente `ToDoItem` — `id`.
2. Lo renda obbligatorio e lo imposti come un tipo `String`.
3. Per prevenire collisioni di nome, rimuova il campo `id` dal suo attributo `data`.
4. Non sta più usando `nanoid`, quindi deve rimuovere la riga `import { nanoid } from 'nanoid';`, altrimenti la sua app genererà un errore.

Il contenuto del `<script>` nel suo componente `ToDoItem` dovrebbe ora assomigliare a questo:

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

Ora, nel suo componente `App.vue`, passi `item.id` come prop al componente `ToDoItem`. Il suo template `App.vue` dovrebbe ora apparire così:

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

Quando guarda il suo sito renderizzato, dovrebbe apparire lo stesso, ma il nostro refactoring significa ora che il nostro `id` viene preso dai dati all'interno di `App.vue` e passato a `ToDoItem` come prop, proprio come tutto il resto, quindi le cose ora sono più logiche e coerenti.

## Sommario

E ciò ci porta alla fine di un altro articolo. Abbiamo ora dati di esempio in atto, e un ciclo che prende ogni bit di dati e lo rende all'interno di un `ToDoItem` nella nostra app.

Ciò di cui abbiamo veramente bisogno ora è la possibilità di permettere ai nostri utenti di inserire i propri elementi da fare nell'app, e per farlo avremo bisogno di un `<input>` di testo, un evento da attivare quando i dati vengono inviati, un metodo da attivare al momento dell'invio per aggiungere i dati e ri-renderizzare la lista, e un modello per controllare i dati. Ci arriveremo nel prossimo articolo.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_first_component","Learn_web_development/Core/Frameworks_libraries/Vue_methods_events_models", "Learn_web_development/Core/Frameworks_libraries")}}
