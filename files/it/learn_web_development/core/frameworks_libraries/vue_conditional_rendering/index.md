---
title: "Rendering condizionale in Vue: modifica delle attività esistenti"
slug: Learn_web_development/Core/Frameworks_libraries/Vue_conditional_rendering
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_computed_properties","Learn_web_development/Core/Frameworks_libraries/Vue_refs_focus_management", "Learn_web_development/Core/Frameworks_libraries")}}

È giunto il momento di aggiungere una delle principali funzionalità che ci manca ancora: la possibilità di modificare le attività esistenti. Per fare ciò, sfrutteremo le capacità di rendering condizionale di Vue, in particolare `v-if` e `v-else`, per consentirci di alternare tra la visualizzazione dell'attività esistente e una vista di modifica dove si possono aggiornare le etichette delle attività. Vedremo anche come aggiungere funzionalità per eliminare le attività.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Familiarità con i linguaggi core
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
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
          la struttura del DOM sottostante. Per l'installazione, e per utilizzare alcune delle
          caratteristiche avanzate di Vue (come Componenti a File Singolo o funzioni di rendering),
          serve un terminale con node + npm installato.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare a fare il rendering condizionale in Vue.</td>
    </tr>
  </tbody>
</table>

## Creare un componente di modifica

Possiamo iniziare creando un componente separato per gestire la funzionalità di modifica. Nella sua directory `components`, crei un nuovo file chiamato `ToDoItemEditForm.vue`. Copi il seguente codice in quel file:

```vue
<template>
  <form class="stack-small" @submit.prevent="onSubmit">
    <div>
      <label class="edit-label">Edit Name for &quot;\{{ label }}&quot;</label>
      <input
        :id="id"
        type="text"
        autocomplete="off"
        v-model.lazy.trim="newLabel" />
    </div>
    <div class="btn-group">
      <button type="button" class="btn" @click="onCancel">
        Cancel
        <span class="visually-hidden">editing \{{ label }}</span>
      </button>
      <button type="submit" class="btn btn__primary">
        Save
        <span class="visually-hidden">edit for \{{ label }}</span>
      </button>
    </div>
  </form>
</template>
<script>
export default {
  props: {
    label: {
      type: String,
      required: true,
    },
    id: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      newLabel: this.label,
    };
  },
  methods: {
    onSubmit() {
      if (this.newLabel && this.newLabel !== this.label) {
        this.$emit("item-edited", this.newLabel);
      }
    },
    onCancel() {
      this.$emit("edit-cancelled");
    },
  },
};
</script>
<style scoped>
.edit-label {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #0b0c0c;
  display: block;
  margin-bottom: 5px;
}
input {
  display: inline-block;
  margin-top: 0.4rem;
  width: 100%;
  min-height: 4.4rem;
  padding: 0.4rem 0.8rem;
  border: 2px solid #565656;
}
form {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
form > * {
  flex: 0 0 100%;
}
</style>
```

> [!NOTE]
> Analizzi il codice sopra e legga la descrizione sottostante per assicurarsi di comprendere tutto ciò che il componente sta facendo prima di procedere. Questo è un modo utile per rafforzare tutto ciò che ha imparato finora.

Questo codice imposta il nucleo della funzionalità di modifica. Creiamo un modulo con un campo `<input>` per modificare il nome della nostra attività da svolgere.

C'è un pulsante "Salva" e un pulsante "Annulla":

- Quando si clicca sul pulsante "Salva", il componente emette la nuova etichetta tramite un evento `item-edited`.
- Quando si clicca sul pulsante "Annulla", il componente lo segnala emettendo un evento `edit-cancelled`.

## Modifica del nostro componente ToDoItem

Prima di poter aggiungere `ToDoItemEditForm` alla nostra app, dobbiamo fare alcune modifiche al nostro componente `ToDoItem`. In particolare, dobbiamo aggiungere una variabile per tenere traccia se l'elemento è in fase di modifica e un pulsante per alternare quella variabile. Aggiungeremo anche un pulsante `Elimina` poiché l'eliminazione è strettamente correlata.

Aggiorni il template del suo `ToDoItem` come mostrato di seguito.

```vue
<template>
  <div class="stack-small">
    <div class="custom-checkbox">
      <input
        type="checkbox"
        class="checkbox"
        :id="id"
        :checked="isDone"
        @change="$emit('checkbox-changed')" />
      <label :for="id" class="checkbox-label">\{{ label }}</label>
    </div>
    <div class="btn-group">
      <button type="button" class="btn" @click="toggleToItemEditForm">
        Edit <span class="visually-hidden">\{{ label }}</span>
      </button>
      <button type="button" class="btn btn__danger" @click="deleteToDo">
        Delete <span class="visually-hidden">\{{ label }}</span>
      </button>
    </div>
  </div>
</template>
```

Abbiamo aggiunto un wrapper `<div>` attorno all'intero template per scopi di layout.

Abbiamo anche aggiunto pulsanti "Modifica" e "Elimina":

- Il pulsante "Modifica", quando cliccato, attiverà la visualizzazione del componente `ToDoItemEditForm` in modo da poterlo utilizzare per modificare il nostro elemento della lista tramite una funzione gestore di eventi chiamata `toggleToItemEditForm()`. Questo gestore imposterà un flag `isEditing` su true. Per fare ciò, dovremo prima definirlo all'interno della nostra proprietà `data()`.
- Il pulsante "Elimina", quando cliccato, eliminerà l'attività tramite una funzione gestore di eventi chiamata `deleteToDo()`. In questo gestore, emetteremo un evento `item-deleted` al nostro componente padre in modo che la lista possa essere aggiornata.

Definiamo i nostri gestori di clic e il necessario flag `isEditing`.

Aggiunga `isEditing` sotto il suo punto dati esistente `isDone`:

```js
export default {
  // …
  data() {
    return {
      isDone: this.done,
      isEditing: false,
    };
  },
  // …
};
```

Ora aggiunga i suoi metodi all'interno di una proprietà methods, subito sotto la sua proprietà `data()`:

```js
export default {
  // …
  methods: {
    deleteToDo() {
      this.$emit("item-deleted");
    },
    toggleToItemEditForm() {
      this.isEditing = true;
    },
  },
  // …
};
```

## Visualizzazione condizionale dei componenti tramite v-if e v-else

Ora abbiamo un flag `isEditing` che possiamo utilizzare per significare che l'elemento è in fase di modifica (o meno). Se `isEditing` è true, vogliamo utilizzare tale flag per visualizzare il nostro `ToDoItemEditForm` invece della casella di controllo. Per fare ciò, utilizzeremo un'altra direttiva di Vue: [`v-if`](https://vuejs.org/api/built-in-directives.html#v-if).

La direttiva `v-if` renderizzerà un blocco solo se il valore passato è veritiero. Questo è simile a come funziona un'istruzione `if` in JavaScript. `v-if` ha anche corrispondenti direttive [`v-else-if`](https://vuejs.org/api/built-in-directives.html#v-else-if) e [`v-else`](https://vuejs.org/api/built-in-directives.html#v-else) per fornire l'equivalente della logica `else if` e `else` di JavaScript all'interno dei template di Vue.

È importante notare che i blocchi `v-else` e `v-else-if` devono essere il primo fratello di un blocco `v-if`/`v-else-if`, altrimenti Vue non li riconoscerà. Si può anche attaccare `v-if` a un tag `<template>` se si ha bisogno di rendere condizionalmente un intero template.

Infine, si può usare un `v-if` + `v-else` alla radice del proprio componente per visualizzare un solo blocco o un altro, poiché Vue renderizzerà solo uno di questi blocchi alla volta. Faremo questo nella nostra app, poiché ci permetterà di sostituire il codice che visualizza il nostro elemento di attività con il modulo di modifica.

Prima di tutto aggiunga `v-if="!isEditing"` al `<div>` radice nel suo componente `ToDoItem`,

```vue
<div class="stack-small" v-if="!isEditing"></div>
```

Successivamente, sotto il tag di chiusura di quel `<div>`, aggiunga la seguente riga:

```vue
<to-do-item-edit-form v-else :id="id" :label="label"></to-do-item-edit-form>
```

Dobbiamo anche importare e registrare il componente `ToDoItemEditForm`, in modo da poterlo usare all'interno di questo template. Aggiunga questa riga nella parte superiore del suo elemento `<script>`:

```js
import ToDoItemEditForm from "./ToDoItemEditForm";
```

E aggiunga una proprietà `components` sopra la proprietà `props` all'interno dell'oggetto componente:

```js
export default {
  // …
  components: {
    ToDoItemEditForm,
  },
  // …
};
```

Ora, se va alla sua app e clicca sul pulsante "Modifica" di un elemento, dovrebbe vedere che la casella di controllo viene sostituita con il modulo di modifica.

![L'app della lista delle cose da fare, con i pulsanti Modifica ed Elimina mostrati, e uno degli elementi in modalità di modifica, con un input di modifica e pulsanti salva e annulla visualizzati](todo-edit-delete.png)

Tuttavia, al momento non c'è modo di tornare indietro. Per risolvere questo problema, abbiamo bisogno di aggiungere altri gestori di eventi al nostro componente.

## Uscire dalla modalità di modifica

Innanzitutto, dobbiamo aggiungere un metodo `itemEdited()` ai metodi del nostro componente `ToDoItem`. Questo metodo dovrebbe prendere la nuova etichetta dell'elemento come argomento, emettere un evento `itemEdited` al componente padre e impostare `isEditing` su `false`.

Lo aggiunga ora, sotto i metodi esistenti:

```js
export default {
  // …
  methods: {
    // …
    itemEdited(newLabel) {
      this.$emit("item-edited", newLabel);
      this.isEditing = false;
    },
    // …
  },
  // …
};
```

Successivamente, avremo bisogno di un metodo `editCancelled()`. Questo metodo non prenderà argomenti e servirà solo a impostare `isEditing` di nuovo su `false`. Aggiunga questo metodo sotto il precedente:

```js
export default {
  // …
  methods: {
    // …
    editCancelled() {
      this.isEditing = false;
    },
    // …
  },
  // …
};
```

Infine, per questa sezione, aggiungeremo gestori di eventi per gli eventi emessi dal componente `ToDoItemEditForm` e collegheremo i metodi appropriati a ciascun evento.

Aggiorni la chiamata `<to-do-item-edit-form></to-do-item-edit-form>` per apparire nel seguente modo:

```vue
<to-do-item-edit-form
  v-else
  :id="id"
  :label="label"
  @item-edited="itemEdited"
  @edit-cancelled="editCancelled">
</to-do-item-edit-form>
```

## Aggiornare ed eliminare elementi da fare

Ora possiamo alternare tra il modulo di modifica e la casella di controllo. Tuttavia, non abbiamo ancora gestito l'aggiornamento dell'array `ToDoItems` in `App.vue`. Per risolvere questo, dobbiamo ascoltare l'evento `item-edited` e aggiornare di conseguenza la lista. Vorremmo anche gestire l'evento di eliminazione in modo da poter eliminare le attività.

Aggiunga i seguenti nuovi metodi all'oggetto del componente in `App.vue`, sotto i metodi esistenti all'interno della proprietà `methods`:

```js
export default {
  // …
  methods: {
    // …
    deleteToDo(toDoId) {
      const itemIndex = this.ToDoItems.findIndex((item) => item.id === toDoId);
      this.ToDoItems.splice(itemIndex, 1);
    },
    editToDo(toDoId, newLabel) {
      const toDoToEdit = this.ToDoItems.find((item) => item.id === toDoId);
      toDoToEdit.label = newLabel;
    },
    // …
  },
  // …
};
```

Successivamente, aggiungeremo gli ascoltatori di eventi per gli eventi `item-deleted` e `item-edited`:

- Per `item-deleted`, deve passare `item.id` al metodo.
- Per `item-edited`, dovrà passare `item.id` e la variabile speciale `$event`. Questa è una variabile speciale di Vue usata per passare i dati degli eventi ai metodi. Quando si utilizzano eventi HTML nativi (come `click`), questo passerà l'oggetto evento nativo al suo metodo.

Aggiorni la chiamata `<to-do-item></to-do-item>` all'interno del template `App.vue` per apparire così:

```vue
<to-do-item
  :label="item.label"
  :done="item.done"
  :id="item.id"
  @checkbox-changed="updateDoneStatus(item.id)"
  @item-deleted="deleteToDo(item.id)"
  @item-edited="editToDo(item.id, $event)">
</to-do-item>
```

Ed eccoci qui: ora dovrebbe essere in grado di modificare ed eliminare elementi dalla lista!

## Correzione di un piccolo bug con lo stato di isDone

Finora è tutto fantastico, ma in effetti abbiamo introdotto un bug aggiungendo la funzionalità di modifica. Provi a fare questo:

1. Spunti (o deseleziona) una delle caselle di controllo.
2. Premi il pulsante "Modifica" per quell'elemento della lista.
3. Annulla la modifica premendo il pulsante "Annulla".

Noti lo stato della casella di controllo dopo aver annullato: non solo l'app ha dimenticato lo stato della casella, ma lo stato di completamento di quell'attività è ora sfasato. Se prova a spuntarla (o deselezionarla) di nuovo, il conteggio dei completi cambierà in modo opposto a quanto ci si aspetterebbe. Questo perché `isDone` all'interno di `data` riceve solo il valore `this.done` al caricamento del componente.

Fortunatamente, correggere questo è abbastanza semplice: possiamo farlo convertendo il nostro elemento di dati `isDone` in una [proprietà computata](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_computed_properties) — un altro vantaggio delle proprietà computate è che preservano la [reattività](https://vuejs.org/guide/essentials/reactivity-fundamentals.html), il che significa (tra le altre cose) che il loro stato è salvato quando il template cambia come sta facendo ora.

Quindi, implementiamo la correzione in `ToDoItem.vue`:

1. Rimuova la seguente riga all'interno della nostra proprietà `data()`:

   ```js
   export default {
     // …
     isDone: this.done,
     // …
   };
   ```

2. Aggiunga il seguente blocco sotto il blocco `data() {}`:

   ```js
   export default {
     // …
     computed: {
       isDone() {
         return this.done;
       },
     },
     // …
   };
   ```

Ora, quando salva e ricarica, troverà che il problema è risolto: lo stato della casella di controllo è ora preservato quando si passa da un template di attività a un altro.

## Comprendere l'intreccio degli eventi

Una delle parti potenzialmente più confuse è l'intreccio di eventi standard e personalizzati che abbiamo utilizzato per attivare tutta l'interattività nella nostra app. Per comprendere meglio questo aspetto, è una buona idea scrivere un diagramma di flusso, una descrizione o un diagramma di quali eventi vengono emessi dove, dove vengono ascoltati e cosa succede come risultato della loro attivazione.

### App.vue

`<to-do-form>` ascolta:

- L'evento `todo-added` emesso dal metodo `onSubmit()` all'interno del componente `ToDoForm` quando il modulo viene inviato.
  **Risultato**: metodo `addToDo()` invocato per aggiungere un nuovo elemento alla lista `ToDoItems`.

`<to-do-item>` ascolta:

- L'evento `checkbox-changed` emesso dal `<input>` checkbox all'interno del componente `ToDoItem` quando viene spuntato o deselezionato.
  **Risultato**: metodo `updateDoneStatus()` invocato per aggiornare lo stato di completamento dell'attività associata.
- L'evento `item-deleted` emesso dal metodo `deleteToDo()` all'interno del componente `ToDoItem` quando viene premuto il pulsante "Elimina".
  **Risultato**: metodo `deleteToDo()` invocato per eliminare l'attività associata.
- L'evento `item-edited` emesso dal metodo `itemEdited()` all'interno del componente `ToDoItem` quando l'evento `item-edited` emesso dal metodo `onSubmit()` all'interno di `ToDoItemEditForm` è stato ascoltato con successo. Sì, questa è una catena di due diversi eventi `item-edited`!
  **Risultato**: metodo `editToDo()` invocato per aggiornare l'etichetta dell'attività associata.

### ToDoForm.vue

`<form>` ascolta l'evento `submit`.
**Risultato**: il metodo `onSubmit()` è invocato, che controlla che la nuova etichetta non sia vuota, quindi emette l'evento `todo-added` (che viene poi ascoltato dentro `App.vue`, vedi sopra) e infine cancella il `<input>` della nuova etichetta.

### ToDoItem.vue

La `<input>` di `type="checkbox"` ascolta gli eventi `change`.
**Risultato**: evento `checkbox-changed` emesso quando la casella di controllo viene selezionata/deselezionata (che viene poi ascoltato dentro `App.vue`; vedi sopra).

Il pulsante "Modifica" ascolta l'evento `click`.
**Risultato**: metodo `toggleToItemEditForm()` è invocato, che alterna `this.isEditing` a `true`, mostrando così il modulo di modifica all'elemento della lista nel rendering successivo.

Il pulsante "Elimina" ascolta l'evento `click`.
**Risultato**: metodo `deleteToDo()` è invocato, che emette l'evento `item-deleted` (che viene poi ascoltato dentro `App.vue`; vedi sopra).

`<to-do-item-edit-form>` ascolta:

- L'evento `item-edited` emesso dal metodo `onSubmit()` all'interno di `ToDoItemEditForm` quando il modulo viene inviato con successo.
  **Risultato**: metodo `itemEdited()` è invocato, che emette l'evento `item-edited` (che viene poi ascoltato dentro `App.vue`, vedi sopra), e imposta `this.isEditing` nuovamente a `false`, in modo che il modulo di modifica non venga più mostrato nel rendering successivo.
- L'evento `edit-cancelled` emesso dal metodo `onCancel()` all'interno di `ToDoItemEditForm` quando si clicca sul pulsante "Annulla".
  **Risultato**: metodo `editCancelled()` è invocato, che imposta `this.isEditing` nuovamente a `false`, in modo che il modulo di modifica non venga più mostrato nel rendering successivo.

### ToDoItemEditForm.vue

`<form>` ascolta l'evento `submit`.
**Risultato**: il metodo `onSubmit()` è invocato, che controlla se il valore della nuova etichetta non è vuoto e non è lo stesso di quello vecchio, e se sì, emette l'evento `item-edited` (che è poi ascoltato dentro `ToDoItem.vue`, vedi sopra).

Il pulsante "Annulla" ascolta l'evento `click`.
**Risultato**: il metodo `onCancel()` è invocato, che emette l'evento `edit-cancelled` (che è poi ascoltato dentro `ToDoItem.vue`, vedi sopra).

## Sommario

Questo articolo è stato piuttosto intenso e abbiamo coperto molti argomenti qui. Ora abbiamo funzionalità di modifica e eliminazione nella nostra app, il che è piuttosto entusiasmante. Siamo ormai alla fine della nostra serie su Vue. L'ultima funzionalità che esamineremo è la gestione del focus, o in altre parole, come possiamo migliorare l'accessibilità della nostra app tramite tastiera.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_computed_properties","Learn_web_development/Core/Frameworks_libraries/Vue_refs_focus_management", "Learn_web_development/Core/Frameworks_libraries")}}
