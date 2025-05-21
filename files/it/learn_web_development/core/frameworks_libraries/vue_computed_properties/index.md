---
title: Utilizzare le proprietà computate di Vue
slug: Learn_web_development/Core/Frameworks_libraries/Vue_computed_properties
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_styling","Learn_web_development/Core/Frameworks_libraries/Vue_conditional_rendering", "Learn_web_development/Core/Frameworks_libraries")}}

In questo articolo aggiungeremo un contatore che visualizza il numero di elementi todo completati, utilizzando una funzionalità di Vue chiamata proprietà computate. Queste funzionano in modo simile ai metodi, ma vengono rieseguite solo quando una delle loro dipendenze cambia.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Familiarità con i linguaggi core <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
          conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/command line</a
          >.
        </p>
        <p>
          I componenti Vue sono scritti come una combinazione di oggetti JavaScript che
          gestiscono i dati dell'app e una sintassi basata su HTML che mappa la
          struttura DOM sottostante. Per l'installazione, e per utilizzare alcune delle
          funzionalità avanzate di Vue (come Componenti a File Singolo o funzioni di rendere),
          avrai bisogno di un terminale con node + npm installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare come utilizzare le proprietà computate di Vue.</td>
    </tr>
  </tbody>
</table>

## Utilizzo delle proprietà computate

L'obiettivo qui è aggiungere un conteggio riassuntivo della nostra lista di cose da fare. Questo può essere utile per gli utenti e servire anche come etichetta per la lista per la tecnologia assistiva. Se abbiamo 2 di 5 elementi completati nella nostra lista, il nostro riassunto potrebbe leggere "2 elementi completati su 5". Anche se potrebbe essere allettante fare qualcosa del genere:

```vue
<h2>
  \{{ToDoItems.filter(item =&gt; item.done).length}} out of
  \{{ToDoItems.length}} items completed
</h2>
```

Verrebbe ricalcolato a ogni render. Per una piccola app come questa, probabilmente non importa troppo. Per app più grandi, o quando l'espressione è più complicata, potrebbe causare un serio problema di performance.

Una soluzione migliore è usare le [proprietà computate](https://vuejs.org/guide/essentials/computed.html) di Vue. Le proprietà computate funzionano in modo simile ai metodi, ma vengono rieseguite solo quando una delle loro dipendenze cambia. Nel nostro caso, si rieseguirà solo quando l'array `ToDoItems` cambia.

Per creare una proprietà computata, dobbiamo aggiungere una proprietà `computed` al nostro oggetto componente, molto simile alla proprietà `methods` che abbiamo usato in precedenza.

## Aggiungere un contatore riassuntivo

Aggiungi il seguente codice al tuo oggetto componente `App`, sotto la proprietà `methods`. Il metodo di riepilogo della lista otterrà il numero di `ToDoItems` completati e restituirà una stringa che lo segnala.

```js
export default {
  // …
  computed: {
    listSummary() {
      const numberFinishedItems = this.ToDoItems.filter(
        (item) => item.done,
      ).length;
      return `${numberFinishedItems} out of ${this.ToDoItems.length} items completed`;
    },
  },
  // …
};
```

Ora possiamo aggiungere `\{{listSummary}}` direttamente al nostro template; lo aggiungeremo all'interno di un elemento `<h2>`, appena sopra il nostro `<ul>`. Aggiungeremo anche un `id` e un attributo `aria-labelledby` per assegnare il contenuto di `<h2>` come etichetta per l'elemento `<ul>`.

Aggiungi l'`<h2>` descritto e aggiorna il `<ul>` all'interno del template del tuo `App` come segue:

```vue
<h2 id="list-summary">\{{listSummary}}</h2>
<ul aria-labelledby="list-summary" class="stack-large">
  <li v-for="item in ToDoItems" :key="item.id">
    <to-do-item
      :label="item.label"
      :done="item.done"
      :id="item.id"></to-do-item>
  </li>
</ul>
```

Ora dovresti vedere nel tuo app il riepilogo della lista, e il numero totale di elementi aggiornarsi mentre aggiungi nuovi elementi todo! Tuttavia, se provi a selezionare o deselezionare alcuni elementi, scoprirai un bug. Attualmente, non stiamo tracciando effettivamente i dati "done" in nessun modo, quindi il numero di elementi completati non cambia.

## Tracciare i cambiamenti dello stato "done"

Possiamo utilizzare eventi per catturare l'aggiornamento della casella di controllo e gestire di conseguenza la nostra lista.

Poiché non stiamo facendo affidamento su un pulsante per attivare il cambiamento, possiamo allegare un gestore eventi `@change` a ogni casella di controllo invece di usare `v-model`.

Aggiorna l'elemento `<input>` in `ToDoItem.vue` in questo modo.

```vue
<input
  type="checkbox"
  class="checkbox"
  :id="id"
  :checked="isDone"
  @change="$emit('checkbox-changed')" />
```

Dal momento che tutto ciò che dobbiamo fare è emettere il fatto che la casella di controllo è stata selezionata, possiamo includere `$emit()` in linea.

In `App.vue`, aggiungi un nuovo metodo chiamato `updateDoneStatus()`, sotto il tuo metodo `addToDo()`. Questo metodo dovrebbe prendere un parametro: l'_id_ dell'elemento todo. Vogliamo trovare l'elemento con l'`id` corrispondente e aggiornare il suo stato `done` in modo che sia l'opposto del suo stato attuale:

```js
export default {
  // …
  methods: {
    // …
    updateDoneStatus(toDoId) {
      const toDoToUpdate = this.ToDoItems.find((item) => item.id === toDoId);
      toDoToUpdate.done = !toDoToUpdate.done;
    },
    // …
  },
  // …
};
```

Vogliamo eseguire questo metodo ogni volta che un `ToDoItem` emette un evento `checkbox-changed`, e passare il suo `item.id` come parametro. Aggiorna la tua chiamata `<to-do-item></to-do-item>` come segue:

```vue
<to-do-item
  :label="item.label"
  :done="item.done"
  :id="item.id"
  @checkbox-changed="updateDoneStatus(item.id)">
</to-do-item>
```

Ora, se selezioni un `ToDoItem`, dovresti vedere il riepilogo aggiornarsi correttamente!

![La nostra app, con un contatore todo completato aggiunto. Attualmente legge 3 su 5 elementi completati](todo-counter.png)

## Riepilogo

In questo articolo abbiamo utilizzato una proprietà computata per aggiungere una piccola funzionalità alla nostra app. Tuttavia, abbiamo questioni più importanti in sospeso — nel prossimo articolo daremo un'occhiata al rendering condizionale e a come possiamo usarlo per mostrare un modulo di modifica quando vogliamo modificare elementi todo esistenti.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_styling","Learn_web_development/Core/Frameworks_libraries/Vue_conditional_rendering", "Learn_web_development/Core/Frameworks_libraries")}}
