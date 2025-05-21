---
title: "Rendering condizionale in Vue: modifica di todo esistenti"
slug: Learn_web_development/Core/Frameworks_libraries/Vue_conditional_rendering
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_computed_properties","Learn_web_development/Core/Frameworks_libraries/Vue_refs_focus_management", "Learn_web_development/Core/Frameworks_libraries")}}

Ora è il momento di aggiungere una delle principali funzionalità che ci manca ancora: la possibilità di modificare gli elementi todo esistenti. Per fare ciò, ci avvarremo delle capacità di rendering condizionale di Vue — in particolare `v-if` e `v-else` — per consentirci di passare dalla vista dell'elemento todo esistente a una vista di modifica dove è possibile aggiornare le etichette degli elementi todo. Esamineremo anche l'aggiunta della funzionalità per eliminare gli elementi todo.

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
          gestiscono i dati dell'app e una sintassi template basata su HTML che mappa
          la struttura DOM sottostante. Per l'installazione e per utilizzare alcune delle
          caratteristiche più avanzate di Vue (come i componenti a file singolo o le funzioni di render),
          avrai bisogno di un terminale con node + npm installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare come fare il rendering condizionale in Vue.</td>
    </tr>
  </tbody>
</table>

## Creazione di un componente per la modifica

Possiamo iniziare creando un componente separato per gestire la funzionalità di modifica. Nella vostra directory `components`, create un nuovo file chiamato `ToDoItemEditForm.vue`. Copia il seguente codice in quel file:

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
> Analizza il codice sopra e poi leggi la descrizione qui sotto per assicurarti di comprendere tutto ciò che il componente sta facendo prima di andare avanti. Questo è un modo utile per rafforzare tutto ciò che hai imparato finora.

Questo codice imposta il nucleo della funzionalità di modifica. Creiamo un modulo con un campo `<input>` per modificare il nome delle nostre attività.

Ci sono un pulsante "Salva" e un pulsante "Annulla":

- Quando si clicca sul pulsante "Salva", il componente emette la nuova etichetta tramite un evento `item-edited`.
- Quando si clicca sul pulsante "Annulla", il componente segnala ciò emettendo un evento `edit-cancelled`.

## Modifica del nostro componente ToDoItem

Prima di poter aggiungere `ToDoItemEditForm` alla nostra app, dobbiamo apportare alcune modifiche al nostro componente `ToDoItem`. In particolare, dobbiamo aggiungere una variabile per tracciare se l'elemento è in fase di modifica e un pulsante per attivare quella variabile. Aggiungeremo anche un pulsante `Elimina` poiché l'eliminazione è strettamente correlata.

Aggiorna il tuo template `ToDoItem` come mostrato di seguito.

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

Abbiamo aggiunto un wrapper `<div>` intorno all'intero template per scopi di layout.

Abbiamo anche aggiunto i pulsanti "Modifica" ed "Elimina":

- Il pulsante "Modifica", quando viene cliccato, attiverà la visualizzazione del componente `ToDoItemEditForm` in modo da poterlo utilizzare per modificare il nostro elemento todo, tramite una funzione gestore di eventi chiamata `toggleToItemEditForm()`. Questo gestore imposterà un flag `isEditing` a true. Per fare ciò, dobbiamo prima definirlo all'interno della nostra proprietà `data()`.
- Il pulsante "Elimina", quando viene cliccato, eliminerà l'elemento todo tramite una funzione gestore di eventi chiamata `deleteToDo()`. In questo gestore emetteremo un evento `item-deleted` al nostro componente genitore in modo che l'elenco possa essere aggiornato.

Definiamo i nostri gestori di clic e il necessario flag `isEditing`.

Aggiungi `isEditing` sotto al tuo esistente punto dati `isDone`:

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

Ora aggiungi i tuoi metodi all'interno di una proprietà methods, subito sotto la tua proprietà `data()`:

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

## Visualizzazione condizionale di componenti tramite v-if e v-else

Ora abbiamo un flag `isEditing` che possiamo usare per indicare che l'elemento è in fase di modifica (o no). Se `isEditing` è true, desideriamo utilizzare quel flag per visualizzare il nostro `ToDoItemEditForm` invece della casella di controllo. Per fare ciò, utilizzeremo un'altra direttiva Vue: [`v-if`](https://vuejs.org/api/built-in-directives.html#v-if).

La direttiva `v-if` renderà un blocco solo se il valore passato ad essa è veritiero. Questo è simile a come funziona un'istruzione `if` in JavaScript. `v-if` ha anche direttive [`v-else-if`](https://vuejs.org/api/built-in-directives.html#v-else-if) e [`v-else`](https://vuejs.org/api/built-in-directives.html#v-else) corrispondenti per fornire l'equivalente della logica `else if` e `else` di JavaScript all'interno dei template Vue.

È importante notare che i blocchi `v-else` e `v-else-if` devono essere il primo fratello di un blocco `v-if`/`v-else-if`, altrimenti Vue non li riconoscerà. Puoi anche allegare `v-if` a un tag `<template>` se hai bisogno di rendere condizionalmente un intero template.

Infine, puoi usare un `v-if` + `v-else` alla radice del tuo componente per visualizzare solo un blocco o un altro, poiché Vue renderà solo uno di questi blocchi alla volta. Lo faremo nella nostra app, poiché ci permetterà di sostituire il codice che visualizza il nostro elemento todo con il modulo di modifica.

Innanzitutto aggiungi `v-if="!isEditing"` al `<div>` radice nel tuo componente `ToDoItem`,

```vue
<div class="stack-small" v-if="!isEditing"></div>
```

Successivamente, sotto il tag di chiusura di quel `<div>` aggiungi la seguente riga:

```vue
<to-do-item-edit-form v-else :id="id" :label="label"></to-do-item-edit-form>
```

Dobbiamo anche importare e registrare il componente `ToDoItemEditForm`, in modo da poterlo usare all'interno di questo template. Aggiungi questa riga all'inizio del tuo elemento `<script>`:

```js
import ToDoItemEditForm from "./ToDoItemEditForm";
```

E aggiungi una proprietà `components` sopra la proprietà `props` all'interno dell'oggetto componente:

```js
export default {
  // …
  components: {
    ToDoItemEditForm,
  },
  // …
};
```

Ora, se vai nella tua app e clicchi sul pulsante "Modifica" di un elemento todo, dovresti vedere la casella di controllo sostituita dal modulo di modifica.

![L'app della lista di cose da fare, con i pulsanti Modifica ed Elimina mostrati, e uno dei todo in modalità modifica, con un input di modifica e pulsanti di salvataggio e annullamento mostrati](todo-edit-delete.png)

Tuttavia, al momento non c'è modo di tornare indietro. Per risolvere questo problema, dobbiamo aggiungere alcuni altri gestori di eventi al nostro componente.

## Uscire dalla modalità di modifica

Per prima cosa, dobbiamo aggiungere un metodo `itemEdited()` ai nostri `methods` del componente `ToDoItem`. Questo metodo dovrebbe prendere la nuova etichetta dell'elemento come argomento, emettere un evento `itemEdited` al componente genitore e impostare `isEditing` su `false`.

Aggiungilo ora, sotto i metodi esistenti:

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

Successivamente, avremo bisogno di un metodo `editCancelled()`. Questo metodo non prenderà argomenti e servirà solo per impostare di nuovo `isEditing` su `false`. Aggiungi questo metodo sotto il precedente:

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

Infine, aggiungeremo i gestori di eventi per gli eventi emessi dal componente `ToDoItemEditForm`, e allegheremo i metodi appropriati a ciascun evento.

Aggiorna la tua chiamata `<to-do-item-edit-form></to-do-item-edit-form>` per sembrare così:

```vue
<to-do-item-edit-form
  v-else
  :id="id"
  :label="label"
  @item-edited="itemEdited"
  @edit-cancelled="editCancelled">
</to-do-item-edit-form>
```

## Aggiornare ed eliminare elementi todo

Ora possiamo alternare tra il modulo di modifica e la casella di controllo. Tuttavia, non abbiamo ancora gestito l'aggiornamento dell'array `ToDoItems` in `App.vue`. Per risolvere questo problema, dobbiamo ascoltare l'evento `item-edited` e aggiornare l'elenco di conseguenza. Vogliamo anche gestire l'evento di eliminazione in modo da poter eliminare elementi todo.

Aggiungi i seguenti nuovi metodi all'oggetto componente di `App.vue`, sotto i metodi esistenti all'interno della proprietà `methods`:

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

Successivamente, aggiungeremo i listener per gli eventi `item-deleted` e `item-edited`:

- Per `item-deleted`, dovrai passare `item.id` al metodo.
- Per `item-edited`, dovrai passare `item.id` e la variabile speciale `$event`. Questa è una variabile speciale di Vue usata per passare i dati dell'evento ai metodi. Quando si usano eventi HTML nativi (come `click`), questo passerà l'oggetto evento nativo al tuo metodo.

Aggiorna la chiamata `<to-do-item></to-do-item>` all'interno del template di `App.vue` per sembrare così:

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

Ed ecco fatto: ora dovresti essere in grado di modificare ed eliminare gli elementi dalla lista!

## Risolvere un piccolo bug con lo stato di isDone

Fino ad ora è andato tutto bene, ma in realtà abbiamo introdotto un bug aggiungendo la funzionalità di modifica. Prova a fare quanto segue:

1. Seleziona (o deseleziona) una delle caselle di controllo todo.
2. Premi il pulsante "Modifica" per quell'elemento todo.
3. Annulla la modifica premendo il pulsante "Annulla".

Nota lo stato della casella di controllo dopo aver annullato, non solo l'app ha dimenticato lo stato della casella di controllo, ma lo stato di completamento di quell'elemento todo è ora sballato. Se provi a selezionare (o deselezionare) di nuovo, il conteggio dei completati cambierà in modo opposto a quello che ti aspetteresti. Questo perché `isDone` all'interno di `data` riceve solo il valore `this.done` al caricamento del componente.

Fortunatamente, risolvere questo problema è abbastanza facile: possiamo farlo convertendo il nostro elemento dati `isDone` in una [proprietà computata](/it/docs/Learn_web_development/Core/Frameworks_libraries/Vue_computed_properties) — un altro vantaggio delle proprietà computate è che preservano la [reattività](https://vuejs.org/guide/essentials/reactivity-fundamentals.html), il che significa (tra le altre cose) che il loro stato viene salvato quando il template cambia come sta facendo ora il nostro.

Quindi, implementiamo la correzione in `ToDoItem.vue`:

1. Rimuovi la seguente riga dall'interno della nostra proprietà `data()`:

   ```js
   export default {
     // …
     isDone: this.done,
     // …
   };
   ```

2. Aggiungi il seguente blocco sotto il blocco `data() {}`:

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

Ora, quando salvi e ricarichi, troverai che il problema è risolto: lo stato della casella di controllo è ora preservato quando passi tra i template degli elementi todo.

## Comprendere il groviglio di eventi

Uno degli aspetti potenzialmente più confusi è il groviglio di eventi standard e personalizzati che abbiamo usato per attivare tutta l'interattività nella nostra app. Per comprendere meglio questo aspetto, è una buona idea scrivere un diagramma a flusso, una descrizione o un diagramma di quali eventi vengono emessi dove, dove vengono ascoltati e cosa accade come risultato del loro attivarsi.

### App.vue

`<to-do-form>` ascolta per:

- Evento `todo-added` emesso dal metodo `onSubmit()` all'interno del componente `ToDoForm` quando il modulo viene inviato.
  **Risultato**: Metodo `addToDo()` invocato per aggiungere un nuovo elemento todo all'array `ToDoItems`.

`<to-do-item>` ascolta per:

- Evento `checkbox-changed` emesso dall'elemento `<input>` di tipo checkbox all'interno del componente `ToDoItem` quando la casella viene selezionata o deselezionata.
  **Risultato**: Metodo `updateDoneStatus()` invocato per aggiornare lo stato di completamento dell'elemento todo associato.
- Evento `item-deleted` emesso dal metodo `deleteToDo()` all'interno del componente `ToDoItem` quando viene premuto il pulsante "Elimina".
  **Risultato**: Metodo `deleteToDo()` invocato per eliminare l'elemento todo associato.
- Evento `item-edited` emesso dal metodo `itemEdited()` all'interno del componente `ToDoItem` quando l'evento `item-edited` emesso dal metodo `onSubmit()` all'interno di `ToDoItemEditForm` è stato ascoltato con successo. Sì, questa è una catena di due eventi `item-edited` diversi!
  **Risultato**: Metodo `editToDo()` invocato per aggiornare l'etichetta dell'elemento todo associato.

### ToDoForm.vue

Il `<form>` ascolta l'evento `submit`.
**Risultato**: Il metodo `onSubmit()` viene invocato, che verifica che la nuova etichetta non sia vuota, quindi emette l'evento `todo-added` (che viene poi ascoltato all'interno di `App.vue`, vedi sopra), e infine cancella l'`<input>` della nuova etichetta.

### ToDoItem.vue

L'`<input>` di `type="checkbox"` ascolta gli eventi `change`.
**Risultato**: Evento `checkbox-changed` emesso quando la casella è selezionata/deselezionata (che viene poi ascoltato all'interno di `App.vue`; vedi sopra).

Il `<button>` "Modifica" ascolta l'evento `click`.
**Risultato**: Il metodo `toggleToItemEditForm()` viene invocato, che alterna `this.isEditing` a `true`, il che consente di visualizzare il modulo di modifica dell'elemento todo al nuovo render.

Il `<button>` "Elimina" ascolta l'evento `click`.
**Risultato**: Il metodo `deleteToDo()` viene invocato, che emette l'evento `item-deleted` (che viene poi ascoltato all'interno di `App.vue`; vedi sopra).

`<to-do-item-edit-form>` ascolta per:

- Evento `item-edited` emesso dal metodo `onSubmit()` all'interno del componente `ToDoItemEditForm` quando il modulo viene inviato con successo.
  **Risultato**: Il metodo `itemEdited()` viene invocato, che emette l'evento `item-edited` (che viene poi ascoltato all'interno di `App.vue`, vedi sopra), e imposta `this.isEditing` su `false`, in modo che il modulo di modifica non venga più mostrato sul nuovo render.
- Evento `edit-cancelled` emesso dal metodo `onCancel()` all'interno del componente `ToDoItemEditForm` quando viene cliccato il pulsante "Annulla".
  **Risultato**: Il metodo `editCancelled()` viene invocato, che imposta `this.isEditing` su `false`, in modo che il modulo di modifica non venga più mostrato sul nuovo render.

### ToDoItemEditForm.vue

Il `<form>` ascolta l'evento `submit`.
**Risultato**: Il metodo `onSubmit()` viene invocato, che controlla se il valore della nuova etichetta non è vuoto e non è lo stesso di quello vecchio, e se così è emette l'evento `item-edited` (che viene poi ascoltato all'interno di `ToDoItem.vue`, vedi sopra).

Il `<button>` "Annulla" ascolta l'evento `click`.
**Risultato**: Il metodo `onCancel()` viene invocato, che emette l'evento `edit-cancelled` (che viene poi ascoltato all'interno di `ToDoItem.vue`, vedi sopra).

## Sommario

Questo articolo è stato abbastanza intenso e abbiamo coperto molto qui. Ora abbiamo le funzionalità di modifica ed eliminazione nella nostra app, il che è piuttosto emozionante. Siamo quasi alla fine della nostra serie su Vue. L'ultimo aspetto da guardare è la gestione del focus, o in altre parole, come possiamo migliorare l'accessibilità tramite tastiera della nostra app.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_computed_properties","Learn_web_development/Core/Frameworks_libraries/Vue_refs_focus_management", "Learn_web_development/Core/Frameworks_libraries")}}
