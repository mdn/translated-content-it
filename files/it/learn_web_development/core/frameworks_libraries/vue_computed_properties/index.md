---
title: Utilizzo delle proprietà calcolate di Vue
slug: Learn_web_development/Core/Frameworks_libraries/Vue_computed_properties
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_styling","Learn_web_development/Core/Frameworks_libraries/Vue_conditional_rendering", "Learn_web_development/Core/Frameworks_libraries")}}

In questo articolo aggiungeremo un contatore che visualizza il numero di elementi della lista delle cose da fare completati, usando una funzionalità di Vue chiamata proprietà calcolate. Queste funzionano in modo simile ai metodi, ma vengono rieseguite solo quando cambia una delle loro dipendenze.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Familiarità con le lingue di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
          conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/linea di comando</a
          >.
        </p>
        <p>
          I componenti Vue sono scritti come una combinazione di oggetti JavaScript che
          gestiscono i dati dell'app e una sintassi di template basata su HTML che mappa
          la struttura del DOM sottostante. Per l'installazione e per utilizzare alcune delle
          funzionalità più avanzate di Vue (come Single File Components o funzioni di rendering),
          avrà bisogno di un terminale con node + npm installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare come utilizzare le proprietà calcolate di Vue.</td>
    </tr>
  </tbody>
</table>

## Uso delle proprietà calcolate

L'obiettivo qui è di aggiungere un conteggio sommario alla nostra lista delle cose da fare. Questo può essere utile per gli utenti e serve anche ad etichettare la lista per le tecnologie assistive. Se abbiamo completato 2 su 5 elementi nella nostra lista delle cose da fare, il nostro sommario potrebbe leggere "2 elementi completati su 5". Anche se potrebbe essere tentante fare qualcosa del genere:

```vue
<h2>
  \{{ToDoItems.filter(item =&gt; item.done).length}} out of
  \{{ToDoItems.length}} items completed
</h2>
```

Sarebbe ricalcolato ad ogni render. Per una piccola app come questa, probabilmente non importa troppo. Per app più grandi, o quando l'espressione è più complicata, potrebbe causare un serio problema di prestazioni.

Una soluzione migliore è usare le [proprietà calcolate](https://vuejs.org/guide/essentials/computed.html) di Vue. Le proprietà calcolate funzionano in modo simile ai metodi, ma vengono rieseguite solo quando cambia una delle loro dipendenze. Nel nostro caso, questa proprietà verrebbe rieseguita solo quando cambia l'array `ToDoItems`.

Per creare una proprietà calcolata, dobbiamo aggiungere una proprietà `computed` al nostro oggetto componente, molto simile alla proprietà `methods` che abbiamo usato in precedenza.

## Aggiungere un contatore sommario

Aggiunga il seguente codice all'oggetto componente `App`, sotto la proprietà `methods`. Il metodo del sommario della lista otterrà il numero di `ToDoItems` completati e restituirà una stringa che lo riporta.

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

Ora possiamo aggiungere `\{{listSummary}}` direttamente al nostro template; lo inseriremo all'interno di un elemento `<h2>`, appena sopra il nostro `<ul>`. Inoltre, aggiungeremo un attributo `id` e un attributo `aria-labelledby` per assegnare il contenuto di `<h2>` come etichetta per l'elemento `<ul>`.

Aggiunga il `<h2>` descritto e aggiorni il `<ul>` all'interno del template del suo `App` come segue:

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

Dovrebbe ora vedere il sommario della lista nella sua app e il numero totale degli elementi si aggiornerà man mano che aggiunge più elementi da fare! Tuttavia, se prova a selezionare e deselezionare alcuni elementi, rivelerà un bug. Attualmente, non stiamo effettivamente tracciando i dati di "fatto" in alcun modo, quindi il numero degli elementi completati non cambia.

## Tracciare i cambiamenti del "fatto"

Possiamo utilizzare gli eventi per catturare l'aggiornamento della casella di controllo e gestire di conseguenza la nostra lista.

Dato che non ci basiamo su una pressione di un pulsante per innescare il cambio, possiamo allegare un gestore di eventi `@change` a ciascuna casella di controllo invece di usare `v-model`.

Aggiorni l'elemento `<input>` in `ToDoItem.vue` in modo che assomigli a questo.

```vue
<input
  type="checkbox"
  class="checkbox"
  :id="id"
  :checked="isDone"
  @change="$emit('checkbox-changed')" />
```

Poiché tutto ciò che bisogna fare è emettere che la casella di controllo è stata selezionata, possiamo includere `$emit()` in linea.

In `App.vue`, aggiunga un nuovo metodo chiamato `updateDoneStatus()`, sotto il suo metodo `addToDo()`. Questo metodo dovrebbe prendere un parametro: l'_id_ dell'elemento da fare. Vogliamo trovare l'elemento con l'`id` corrispondente e aggiornare il suo stato `done` per essere l'opposto del suo stato attuale:

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

Vogliamo eseguire questo metodo ogni volta che un `ToDoItem` emette un evento `checkbox-changed`, e passare il suo `item.id` come parametro. Aggiorni la sua chiamata a `<to-do-item></to-do-item>` come segue:

```vue
<to-do-item
  :label="item.label"
  :done="item.done"
  :id="item.id"
  @checkbox-changed="updateDoneStatus(item.id)">
</to-do-item>
```

Ora se seleziona un `ToDoItem`, dovrebbe vedere il sommario aggiornarsi correttamente!

![La nostra app, con un contatore delle cose da fare completate aggiunto. Attualmente legge 3 su 5 elementi completati](todo-counter.png)

## Sommario

In questo articolo abbiamo usato una proprietà calcolata per aggiungere una piccola funzionalità alla nostra app. Tuttavia, abbiamo altro da affrontare — nell'articolo successivo esamineremo il rendering condizionale e come possiamo usarlo per mostrare un modulo di modifica quando vogliamo modificare elementi da fare esistenti.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_styling","Learn_web_development/Core/Frameworks_libraries/Vue_conditional_rendering", "Learn_web_development/Core/Frameworks_libraries")}}
