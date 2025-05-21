---
title: "Interattività in React: Modifica, filtro e rendering condizionale"
short-title: Modifica, filtro e UI condizionale in React
slug: Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering
l10n:
  sourceCommit: 611edf6335e4a833a6f394d0d98b117e7b0a36bf
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state","Learn_web_development/Core/Frameworks_libraries/React_accessibility", "Learn_web_development/Core/Frameworks_libraries")}}

Man mano che ci avviciniamo alla fine del nostro percorso React (almeno per ora), aggiungeremo i tocchi finali alle aree principali di funzionalità nella nostra app To-do list. Ciò include la possibilità di modificare le attività esistenti e filtrare l'elenco delle attività tra tutte, completate e incomplete. Esamineremo anche il rendering condizionale della UI.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/linea di comando</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        Rendering condizionale in React, e implementazione del filtro delle liste e una UI di modifica nella nostra app.
      </td>
    </tr>
  </tbody>
</table>

## Modificare il nome di un'attività

Non abbiamo ancora un'interfaccia utente per modificare il nome di un'attività. Ci arriveremo tra un momento. Per iniziare, possiamo almeno implementare una funzione `editTask()` in `App.jsx`. Sarà simile a `deleteTask()` perché richiederà un `id` per trovare il suo oggetto di destinazione, ma richiederà anche una proprietà `newName` che contiene il nome con cui aggiornare l'attività. Useremo [`Array.prototype.map()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/map) invece di [`Array.prototype.filter()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) perché vogliamo restituire un nuovo array con alcune modifiche, invece di eliminare qualcosa dall'array.

Aggiungi la funzione `editTask()` all'interno del tuo componente `<App />`, nello stesso punto degli altri funzioni:

```jsx
function editTask(id, newName) {
  const editedTaskList = tasks.map((task) => {
    // if this task has the same ID as the edited task
    if (id === task.id) {
      // Copy the task and update its name
      return { ...task, name: newName };
    }
    // Return the original task if it's not the edited task
    return task;
  });
  setTasks(editedTaskList);
}
```

Passa `editTask` nei nostri componenti `<Todo />` come prop nello stesso modo in cui abbiamo fatto con `deleteTask`:

```jsx
const taskList = tasks.map((task) => (
  <Todo
    id={task.id}
    name={task.name}
    completed={task.completed}
    key={task.id}
    toggleTaskCompleted={toggleTaskCompleted}
    deleteTask={deleteTask}
    editTask={editTask}
  />
));
```

Ora apri `Todo.jsx`. Faremo un po' di refactoring.

## Una UI per modificare

Per consentire agli utenti di modificare un'attività, dobbiamo fornire loro un'interfaccia utente. Innanzitutto, importa `useState` nel componente `<Todo />` come avevamo fatto prima con il componente `<App />`:

```jsx
import { useState } from "react";
```

Useremo questo per impostare uno stato `isEditing` con un valore predefinito di `false`. Aggiungi la seguente linea appena all'inizio della definizione del tuo componente `<Todo />`:

```jsx
const [isEditing, setEditing] = useState(false);
```

Successivamente, ripenseremo al componente `<Todo />`. D'ora in poi, vogliamo che visualizzi uno dei due possibili "template", piuttosto che il singolo template usato finora:

- Il template "view", quando stiamo semplicemente visualizzando un todo; questo è ciò che abbiamo usato nel tutorial finora.
- Il template "editing", quando stiamo modificando un todo. Stiamo per creare questo.

Copia questo blocco di codice nella funzione `Todo()`, sotto il tuo hook `useState()` ma sopra l'istruzione `return`:

```jsx
const editingTemplate = (
  <form className="stack-small">
    <div className="form-group">
      <label className="todo-label" htmlFor={props.id}>
        New name for {props.name}
      </label>
      <input id={props.id} className="todo-text" type="text" />
    </div>
    <div className="btn-group">
      <button type="button" className="btn todo-cancel">
        Cancel
        <span className="visually-hidden">renaming {props.name}</span>
      </button>
      <button type="submit" className="btn btn__primary todo-edit">
        Save
        <span className="visually-hidden">new name for {props.name}</span>
      </button>
    </div>
  </form>
);
const viewTemplate = (
  <div className="stack-small">
    <div className="c-cb">
      <input
        id={props.id}
        type="checkbox"
        defaultChecked={props.completed}
        onChange={() => props.toggleTaskCompleted(props.id)}
      />
      <label className="todo-label" htmlFor={props.id}>
        {props.name}
      </label>
    </div>
    <div className="btn-group">
      <button type="button" className="btn">
        Edit <span className="visually-hidden">{props.name}</span>
      </button>
      <button
        type="button"
        className="btn btn__danger"
        onClick={() => props.deleteTask(props.id)}>
        Delete <span className="visually-hidden">{props.name}</span>
      </button>
    </div>
  </div>
);
```

Abbiamo ora i due diversi strutture di template — "edit" e "view" — definiti all'interno di due costanti separate. Ciò significa che l'istruzione `return` di `<Todo />` è ora ripetitiva — contiene anche una definizione del template "view". Possiamo pulire questo usando il **rendering condizionale** per determinare quale template il componente restituisce, e quindi viene reso nell'UI.

## Rendering condizionale

In JSX, possiamo usare una condizione per cambiare ciò che viene rappresentato dal browser. Per scrivere una condizione in JSX, possiamo utilizzare un [operatore ternario](/it/docs/Web/JavaScript/Reference/Operators/Conditional_operator).

Nel caso del nostro componente `<Todo />`, la nostra condizione è "Questa attività è in fase di modifica?" Cambia l'istruzione `return` all'interno di `Todo()` in modo che legga così:

```jsx
return <li className="todo">{isEditing ? editingTemplate : viewTemplate}</li>;
```

Il tuo browser dovrebbe tornare a visualizzare tutte le tue attività proprio come prima. Per vedere il template di modifica, dovrai cambiare lo stato predefinito di `isEditing` da `false` a `true` nel tuo codice per ora; ci occuperemo di far sì che il pulsante di modifica possa alternarlo nella sezione successiva!

## Alternare i template di `<Todo />`

Finalmente siamo pronti a rendere la nostra ultima funzionalità principale interattiva. Per iniziare, vogliamo chiamare `setEditing()` con un valore di `true` quando un utente preme il pulsante "Edit" nel nostro `viewTemplate`, in modo da poter cambiare i template.

Aggiorna il pulsante "Edit" nel `viewTemplate` come segue:

```jsx
<button type="button" className="btn" onClick={() => setEditing(true)}>
  Edit <span className="visually-hidden">{props.name}</span>
</button>
```

Ora aggiungeremo lo stesso gestore `onClick` al pulsante "Cancel" nel `editingTemplate`, ma questa volta imposteremo `isEditing` su `false` in modo da tornare al template view.

Aggiorna il pulsante "Cancel" nel `editingTemplate` come segue:

```jsx
<button
  type="button"
  className="btn todo-cancel"
  onClick={() => setEditing(false)}>
  Cancel
  <span className="visually-hidden">renaming {props.name}</span>
</button>
```

Con questo codice in atto, dovresti essere in grado di premere i pulsanti "Edit" e "Cancel" nei tuoi elementi todo per passare da un template all'altro.

![L'elemento eat todo mostrando il template view, con i pulsanti di modifica e eliminazione disponibili](view.png)

![L'elemento eat todo mostrando il template edit, con un campo di input per inserire un nuovo nome e i pulsanti di annullamento e salvataggio disponibili](edit.png)

Il passaggio successivo è rendere effettivamente funzionante la funzionalità di modifica.

## Modificare dall'UI

Molto di quello che stiamo per fare rifletterà il lavoro che abbiamo fatto in `Form.jsx`: mentre l'utente digita nel nostro nuovo campo di input, dobbiamo tracciare il testo che inserisce; una volta inviato il modulo, dobbiamo usare un prop di callback per aggiornare il nostro stato con il nuovo nome dell'attività.

Inizieremo creando un nuovo hook per memorizzare e impostare il nuovo nome. Ancora in `Todo.jsx`, metti il seguente sotto l'hook esistente:

```jsx
const [newName, setNewName] = useState("");
```

Successivamente, crea una funzione `handleChange()` che imposterà il nuovo nome; inseriscila sotto gli hook ma prima di i template:

```jsx
function handleChange(e) {
  setNewName(e.target.value);
}
```

Ora aggiorneremo il campo `<input />` del nostro `editingTemplate`, impostando un attributo `value` di `newName` e collegando la nostra funzione `handleChange()` al suo evento `onChange`. Aggiornalo come segue:

```jsx
<input
  id={props.id}
  className="todo-text"
  type="text"
  value={newName}
  onChange={handleChange}
/>
```

Infine, dobbiamo creare una funzione per gestire l'evento `onSubmit` del modulo di modifica. Aggiungi il seguente poco sotto `handleChange()`:

```jsx
function handleSubmit(e) {
  e.preventDefault();
  props.editTask(props.id, newName);
  setNewName("");
  setEditing(false);
}
```

Ricorda che il nostro prop di callback `editTask()` ha bisogno dell'ID dell'attività che stiamo modificando oltre al suo nuovo nome.

Collega questa funzione all'evento `submit` del modulo aggiungendo il seguente gestore `onSubmit` al `<form>` del `editingTemplate`:

```jsx
<form className="stack-small" onSubmit={handleSubmit}>
  {/* … */}
</form>
```

Ora dovresti essere in grado di modificare un'attività nel tuo browser. A questo punto, il tuo file `Todo.jsx` dovrebbe apparire così:

```jsx
function Todo(props) {
  const [isEditing, setEditing] = useState(false);
  const [newName, setNewName] = useState("");

  function handleChange(e) {
    setNewName(e.target.value);
  }

  function handleSubmit(e) {
    e.preventDefault();
    props.editTask(props.id, newName);
    setNewName("");
    setEditing(false);
  }

  const editingTemplate = (
    <form className="stack-small" onSubmit={handleSubmit}>
      <div className="form-group">
        <label className="todo-label" htmlFor={props.id}>
          New name for {props.name}
        </label>
        <input
          id={props.id}
          className="todo-text"
          type="text"
          value={newName}
          onChange={handleChange}
        />
      </div>
      <div className="btn-group">
        <button
          type="button"
          className="btn todo-cancel"
          onClick={() => setEditing(false)}>
          Cancel
          <span className="visually-hidden">renaming {props.name}</span>
        </button>
        <button type="submit" className="btn btn__primary todo-edit">
          Save
          <span className="visually-hidden">new name for {props.name}</span>
        </button>
      </div>
    </form>
  );

  const viewTemplate = (
    <div className="stack-small">
      <div className="c-cb">
        <input
          id={props.id}
          type="checkbox"
          defaultChecked={props.completed}
          onChange={() => props.toggleTaskCompleted(props.id)}
        />
        <label className="todo-label" htmlFor={props.id}>
          {props.name}
        </label>
      </div>
      <div className="btn-group">
        <button
          type="button"
          className="btn"
          onClick={() => {
            setEditing(true);
          }}>
          Edit <span className="visually-hidden">{props.name}</span>
        </button>
        <button
          type="button"
          className="btn btn__danger"
          onClick={() => props.deleteTask(props.id)}>
          Delete <span className="visually-hidden">{props.name}</span>
        </button>
      </div>
    </div>
  );

  return <li className="todo">{isEditing ? editingTemplate : viewTemplate}</li>;
}

export default Todo;
```

## Tornando ai pulsanti di filtro

Ora che le nostre funzionalità principali sono complete, possiamo pensare ai nostri pulsanti di filtro. Attualmente, ripetono l'etichetta "All" e non hanno alcuna funzionalità! Riappliqueremo alcune delle abilità che abbiamo usato nel nostro componente `<Todo />` per:

- Creare un hook per memorizzare il filtro attivo.
- Visualizzare un array di elementi `<FilterButton />` che consentono agli utenti di cambiare il filtro attivo tra tutti, completati e incompleti.

### Aggiungere un hook di filtro

Aggiungi un nuovo hook alla tua funzione `App()` che legge e imposta un filtro. Vogliamo che il filtro predefinito sia `All` perché tutte le nostre attività dovrebbero essere mostrate inizialmente:

```jsx
const [filter, setFilter] = useState("All");
```

### Definire i nostri filtri

Il nostro obiettivo attuale è duplice:

- Ogni filtro dovrebbe avere un nome univoco.
- Ogni filtro dovrebbe avere un comportamento univoco.

Un oggetto JavaScript sarebbe un ottimo modo per relazionare i nomi ai comportamenti: ogni chiave è il nome di un filtro; ogni proprietà è il comportamento associato a quel nome.

In cima a `App.jsx`, sotto le nostre importazioni ma sopra la nostra funzione `App()`, aggiungiamo un oggetto chiamato `FILTER_MAP`:

```jsx
const FILTER_MAP = {
  All: () => true,
  Active: (task) => !task.completed,
  Completed: (task) => task.completed,
};
```

I valori di `FILTER_MAP` sono funzioni che utilizzeremo per filtrare l'array di dati `tasks`:

- Il filtro `All` mostra tutte le attività, quindi restituiamo `true` per tutte le attività.
- Il filtro `Active` mostra attività il cui prop `completed` è `false`.
- Il filtro `Completed` mostra attività il cui prop `completed` è `true`.

Sotto alla nostra aggiunta precedente, aggiungi il seguente — qui stiamo usando il metodo [`Object.keys()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) per raccogliere un array di `FILTER_NAMES`:

```jsx
const FILTER_NAMES = Object.keys(FILTER_MAP);
```

> [!NOTE]
> Stiamo definendo queste costanti al di fuori della nostra funzione `App()` perché se fossero definite al suo interno, verrebbero ricalcolate ogni volta che il componente `<App />` viene renderizzato di nuovo, e non vogliamo questo. Queste informazioni non cambieranno mai qualunque cosa faccia la nostra applicazione.

### Rendering dei filtri

Ora che abbiamo l'array `FILTER_NAMES`, possiamo usarlo per visualizzare tutti e tre i nostri filtri. All'interno della funzione `App()` possiamo creare una costante chiamata `filterList`, che utilizzeremo per mappare il nostro array di nomi e restituire un componente `<FilterButton />`. Ricorda, abbiamo bisogno di chiavi anche qui.

Aggiungi il seguente sotto la dichiarazione della costante `taskList`:

```jsx
const filterList = FILTER_NAMES.map((name) => (
  <FilterButton key={name} name={name} />
));
```

Ora sostituiremo i tre `<FilterButton />` ripetuti in `App.jsx` con questo `filterList`. Sostituisci il seguente:

```jsx
<div className="filters btn-group stack-exception">
  <FilterButton />
  <FilterButton />
  <FilterButton />
</div>
```

Con questo:

```jsx
<div className="filters btn-group stack-exception">{filterList}</div>
```

Questo ancora non funzionerà. Abbiamo ancora un po' di lavoro da fare prima.

### Filtri interattivi

Per rendere i nostri pulsanti di filtro interattivi, dobbiamo considerare quali props devono utilizzare.

- Sappiamo che il `<FilterButton />` dovrebbe indicare se è attualmente premuto e dovrebbe essere premuto se il suo nome corrisponde al valore corrente del nostro stato filtro.
- Sappiamo che il `<FilterButton />` ha bisogno di una callback per impostare il filtro attivo. Possiamo fare uso diretto del nostro hook `setFilter`.

Aggiorna la tua costante `filterList` come segue:

```jsx
const filterList = FILTER_NAMES.map((name) => (
  <FilterButton
    key={name}
    name={name}
    isPressed={name === filter}
    setFilter={setFilter}
  />
));
```

Allo stesso modo di quanto fatto prima con il nostro componente `<Todo />`, ora dobbiamo aggiornare `FilterButton.jsx` per utilizzare i props che gli abbiamo dato. Fai ciascuno dei seguenti, e ricorda di usare le parentesi graffe per leggere queste variabili!

- Sostituisci `all` con `{props.name}`.
- Imposta il valore di `aria-pressed` su `{props.isPressed}`.
- Aggiungi un gestore `onClick` che chiama `props.setFilter()` con il nome del filtro.

Con tutto quanto fatto, il tuo file `FilterButton.jsx` dovrebbe risultare così:

```jsx
function FilterButton(props) {
  return (
    <button
      type="button"
      className="btn toggle-btn"
      aria-pressed={props.isPressed}
      onClick={() => props.setFilter(props.name)}>
      <span className="visually-hidden">Show </span>
      <span>{props.name}</span>
      <span className="visually-hidden"> tasks</span>
    </button>
  );
}

export default FilterButton;
```

Visita di nuovo il tuo browser. Dovresti vedere che i diversi pulsanti sono stati assegnati i rispettivi nomi. Quando premi un pulsante filtro, dovresti vedere il suo testo prendere un nuovo contorno — questo ti dice che è stato selezionato. E se guardi l'Ispettore Pagina del tuo DevTool mentre clicchi i pulsanti, vedrai i valori dell'attributo `aria-pressed` cambiare di conseguenza.

![I tre pulsanti di filtro dell'app - tutto, attivi e completati - con un'attenzione evidenziata su completati](filter-buttons.png)

Tuttavia, i nostri pulsanti ancora non filtrano effettivamente i todo nell'interfaccia utente! Facciamola finita.

### Filtrare le attività nell'UI

Attualmente, la nostra costante `taskList` in `App()` mappa sullo stato delle attività e restituisce un nuovo componente `<Todo />` per tutte esse. Questo non è ciò che vogliamo! Un'attività dovrebbe essere visualizzata solo se è inclusa nei risultati dell'applicazione del filtro selezionato. Prima di mappare sullo stato delle attività, dovremmo filtrarle (con [`Array.prototype.filter()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)) per eliminare gli oggetti che non vogliamo mostrare.

Aggiorna il tuo `taskList` in questo modo:

```jsx
const taskList = tasks
  .filter(FILTER_MAP[filter])
  .map((task) => (
    <Todo
      id={task.id}
      name={task.name}
      completed={task.completed}
      key={task.id}
      toggleTaskCompleted={toggleTaskCompleted}
      deleteTask={deleteTask}
      editTask={editTask}
    />
  ));
```

Per decidere quale funzione di callback utilizzare in `Array.prototype.filter()`, accediamo al valore in `FILTER_MAP` che corrisponde alla chiave del nostro stato filtro. Quando il filtro è `All`, ad esempio, `FILTER_MAP[filter]` si valuterà in `() => true`.

Scegliendo un filtro nel tuo browser adesso rimuoverà le attività che non soddisfano i suoi criteri. Anche il conteggio nell'intestazione sopra l'elenco cambierà per riflettere la lista!

![L'app con i pulsanti di filtro in posizione. Attivi è evidenziato, quindi solo gli elementi todo attivi sono mostrati.](filtered-todo-list.png)

## Sommario

Ecco fatto — la nostra app è ora funzionalmente completa. Tuttavia, ora che abbiamo implementato tutte le nostre funzionalità, possiamo apportare alcuni miglioramenti per garantire che una gamma più ampia di utenti possa utilizzare la nostra app. Il nostro prossimo articolo completa le nostre guide su React esaminando l'inclusione della gestione del focus in React, che può migliorare l'usabilità e ridurre la confusione sia per gli utenti che usano solo la tastiera sia per quelli che usano un lettore di schermo.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state","Learn_web_development/Core/Frameworks_libraries/React_accessibility", "Learn_web_development/Core/Frameworks_libraries")}}
