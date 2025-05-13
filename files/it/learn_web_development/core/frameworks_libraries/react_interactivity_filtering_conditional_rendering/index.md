---
title: "Interattività in React: Modifica, filtro, rendering condizionale"
short-title: Modifica, filtro, UI condizionale in React
slug: Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state","Learn_web_development/Core/Frameworks_libraries/React_accessibility", "Learn_web_development/Core/Frameworks_libraries")}}

Alla fine del nostro viaggio con React (almeno per ora), aggiungeremo gli ultimi ritocchi alle principali aree di funzionalità nell'app della lista di cose da fare. Questo include la possibilità di modificare attività esistenti e filtrare l'elenco delle attività tra tutte, completate e incomplete. Durante questo percorso, esploreremo il rendering condizionale dell'interfaccia utente.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi fondamentali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e con il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/linea di comando</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        Rendering condizionale in React e implementazione del filtraggio dell'elenco e di un'interfaccia di modifica nella nostra app.
      </td>
    </tr>
  </tbody>
</table>

## Modifica del nome di un'attività

Attualmente non abbiamo un'interfaccia utente per modificare il nome di un'attività. Ci arriveremo tra poco. Per cominciare, possiamo almeno implementare una funzione `editTask()` in `App.jsx`. Sarà simile a `deleteTask()` perché riceverà un `id` per trovare l'oggetto di destinazione, ma riceverà anche una proprietà `newName` contenente il nome con cui aggiornare l'attività. Utilizzeremo [`Array.prototype.map()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/map) invece di [`Array.prototype.filter()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) perché vogliamo restituire un nuovo array con alcune modifiche, invece di eliminare qualcosa dall'array.

Aggiungi la funzione `editTask()` all'interno del tuo componente `<App />`, nello stesso posto delle altre funzioni:

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

Passare `editTask` ai componenti `<Todo />` come prop nello stesso modo in cui l'abbiamo fatto con `deleteTask`:

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

## Un'interfaccia utente per la modifica

Per consentire agli utenti di modificare un'attività, dobbiamo fornire un'interfaccia utente per farlo. Prima di tutto, importa `useState` nel componente `<Todo />` come abbiamo fatto prima con il componente `<App />`:

```jsx
import { useState } from "react";
```

Lo useremo per impostare uno stato `isEditing` con un valore predefinito di `false`. Aggiungi la seguente riga all'inizio della definizione del tuo componente `<Todo />`:

```jsx
const [isEditing, setEditing] = useState(false);
```

Successivamente, ripenseremo al componente `<Todo />`. D'ora in poi, vogliamo che mostri uno dei due possibili "template", piuttosto che il single template che ha usato finora:

- Il template "view", quando stiamo semplicemente visualizzando un todo; è ciò che abbiamo usato finora nel tutorial.
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

Abbiamo ora due diverse strutture di template — "edit" e "view" — definite all'interno di due costanti separate. Questo significa che l'istruzione `return` del `<Todo />` è ora ripetitiva — contiene anche una definizione del template "view". Possiamo semplificare questo utilizzando il **rendering condizionale** per determinare quale template il componente restituisce e quindi viene reso nell'interfaccia utente.

## Rendering condizionale

In JSX, possiamo utilizzare una condizione per cambiare cosa viene reso dal browser. Per scrivere una condizione in JSX, possiamo usare un [operatore ternario](/it/docs/Web/JavaScript/Reference/Operators/Conditional_operator).

Nel caso del nostro componente `<Todo />`, la nostra condizione è "Questa attività viene modificata?" Modifica l'istruzione `return` in `Todo()` in modo che legga come segue:

```jsx
return <li className="todo">{isEditing ? editingTemplate : viewTemplate}</li>;
```

Il browser dovrebbe rendere tutte le tue attività come prima. Per vedere il template di modifica, per ora dovrai modificare lo stato predefinito di `isEditing` da `false` a `true` nel tuo codice; vedremo come fare in modo che il pulsante modifichi questo nella sezione successiva!

## Alternanza dei template `<Todo />`

Finalmente siamo pronti per rendere interattiva la nostra funzione principale. Per iniziare, vogliamo chiamare `setEditing()` con un valore di `true` quando un utente preme il pulsante "Edit" nel nostro `viewTemplate`, in modo da poter cambiare template.

Aggiorna il pulsante "Edit" nel `viewTemplate` in questo modo:

```jsx
<button type="button" className="btn" onClick={() => setEditing(true)}>
  Edit <span className="visually-hidden">{props.name}</span>
</button>
```

Ora aggiungeremo lo stesso gestore `onClick` al pulsante "Cancel" nel `editingTemplate`, ma questa volta imposteremo `isEditing` su `false` in modo da tornare al template di visualizzazione.

Aggiorna il pulsante "Cancel" nel `editingTemplate` in questo modo:

```jsx
<button
  type="button"
  className="btn todo-cancel"
  onClick={() => setEditing(false)}>
  Cancel
  <span className="visually-hidden">renaming {props.name}</span>
</button>
```

Con questo codice in posizione, dovresti essere in grado di premere i pulsanti "Edit" e "Cancel" nei tuoi elementi todo per passare da un template all'altro.

![Il todo "eat" mostra il template di visualizzazione, con i pulsanti di modifica e eliminazione disponibili](view.png)

![Il todo "eat" mostra il template di modifica, con un campo di input per inserire un nuovo nome e i pulsanti di cancellazione e salvataggio disponibili](edit.png)

Il passo successivo è rendere effettivamente funzionante la funzionalità di modifica.

## Modifica dall'interfaccia utente

Gran parte di ciò che stiamo per fare rispecchierà il lavoro fatto in `Form.jsx`: mentre l'utente digita nel nostro nuovo campo di input, dobbiamo tenere traccia del testo che inserisce; una volta che viene inviato il modulo, bisogna usare un prop callback per aggiornare il nostro stato con il nuovo nome dell'attività.

Inizieremo creando un nuovo hook per memorizzare e impostare il nuovo nome. Ancora in `Todo.jsx`, posiziona quanto segue sotto l'hook esistente:

```jsx
const [newName, setNewName] = useState("");
```

Successivamente, crea una funzione `handleChange()` che imposterà il nuovo nome; posizionala sotto gli hook ma prima dei template:

```jsx
function handleChange(e) {
  setNewName(e.target.value);
}
```

Ora aggiorneremo il campo `<input />` del `editingTemplate`, impostando un attributo `value` di `newName` e collegando la nostra funzione `handleChange()` al suo evento `onChange`. Aggiornatelo come segue:

```jsx
<input
  id={props.id}
  className="todo-text"
  type="text"
  value={newName}
  onChange={handleChange}
/>
```

Infine, dobbiamo creare una funzione che gestisca l'evento `onSubmit` del modulo di modifica. Aggiungi il seguente just sotto `handleChange()`:

```jsx
function handleSubmit(e) {
  e.preventDefault();
  props.editTask(props.id, newName);
  setNewName("");
  setEditing(false);
}
```

Ricorda che il nostro prop callback `editTask()` necessita dell'ID dell'attività che stiamo modificando nonché del suo nuovo nome.

Collega questa funzione all'evento `submit` del modulo aggiungendo il seguente gestore `onSubmit` al `<form>` del `editingTemplate`:

```jsx
<form className="stack-small" onSubmit={handleSubmit}>
```

Dovresti ora essere in grado di modificare un'attività nel tuo browser. A questo punto, il tuo file `Todo.jsx` dovrebbe apparire come segue:

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

## Torniamo ai pulsanti di filtro

Ora che le nostre funzionalità principali sono complete, possiamo pensare ai nostri pulsanti di filtro. Attualmente, ripetono l'etichetta "All" e non hanno funzionalità! Riproveremo alcune abilità che abbiamo utilizzato nel nostro componente `<Todo />` per:

- Creare un hook per memorizzare il filtro attivo.
- Rendere un array di elementi `<FilterButton />` che consentano agli utenti di cambiare il filtro attivo tra tutte, completate e incomplete.

### Aggiungi un hook per il filtro

Aggiungi un nuovo hook alla tua funzione `App()` che legga e imposti un filtro. Vogliamo che il filtro predefinito sia `All` perché inizialmente tutte le attività dovrebbero essere mostrate:

```jsx
const [filter, setFilter] = useState("All");
```

### Definisci i nostri filtri

Il nostro obiettivo ora è duplice:

- Ogni filtro deve avere un nome univoco.
- Ogni filtro deve avere un comportamento univoco.

Un oggetto JavaScript sarebbe un ottimo modo per collegare i nomi ai comportamenti: ogni chiave è il nome di un filtro; ogni proprietà è il comportamento associato a quel nome.

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
- Il filtro `Active` mostra le attività il cui prop `completed` è `false`.
- Il filtro `Completed` mostra le attività il cui prop `completed` è `true`.

Sotto la nostra precedente aggiunta, aggiungi il seguente — qui stiamo usando il metodo [`Object.keys()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) per raccogliere un array di `FILTER_NAMES`:

```jsx
const FILTER_NAMES = Object.keys(FILTER_MAP);
```

> [!NOTE]
> Stiamo definendo queste costanti al di fuori della funzione `App()` perché se fossero definite al suo interno, verrebbero ricalcolate ogni volta che il componente `<App />` viene renderizzato, e non vogliamo questo. Queste informazioni non cambieranno mai indipendentemente da ciò che fa la nostra applicazione.

### Rendering dei filtri

Ora che abbiamo l'array `FILTER_NAMES`, possiamo usarlo per rendere tutti e tre i nostri filtri. All'interno della funzione `App()` possiamo creare una costante chiamata `filterList`, che utilizzeremo per eseguire il map sul nostro array di nomi e restituire un componente `<FilterButton />`. Ricordiamo, abbiamo bisogno delle chiavi anche qui.

Aggiungi quanto segue sotto la dichiarazione della tua costante `taskList`:

```jsx
const filterList = FILTER_NAMES.map((name) => (
  <FilterButton key={name} name={name} />
));
```

Ora sostituiremo i tre `<FilterButton />` ripetuti in `App.jsx` con questa `filterList`. Sostituisci il seguente:

```jsx
<FilterButton />
<FilterButton />
<FilterButton />
```

Con questo:

```jsx-nolint
{filterList}
```

Questo ancora non funzionerà. Abbiamo ancora un po' di lavoro da fare.

### Filtri interattivi

Per rendere interattivi i nostri pulsanti di filtro, dovremmo considerare quali props devono utilizzare.

- Sappiamo che il `<FilterButton />` dovrebbe segnalare se è attualmente premuto e dovrebbe essere premuto se il suo nome corrisponde al valore corrente del nostro stato di filtro.
- Sappiamo che il `<FilterButton />` necessita di un callback per impostare il filtro attivo. Possiamo fare uso diretto del nostro hook `setFilter`.

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

Nello stesso modo in cui abbiamo fatto in precedenza con il nostro componente `<Todo />`, ora dobbiamo aggiornare `FilterButton.jsx` per utilizzare i props che gli abbiamo dato. Fai ciascuno dei seguenti e ricorda di usare le parentesi graffe per leggere queste variabili!

- Sostituire `all` con `{props.name}`.
- Imposta il valore di `aria-pressed` su `{props.isPressed}`.
- Aggiungi un gestore `onClick` che chiama `props.setFilter()` con il nome del filtro.

Con tutto ciò fatto, il tuo file `FilterButton.jsx` dovrebbe apparire così:

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

Visita di nuovo il tuo browser. Dovresti vedere che i diversi pulsanti sono stati dotati dei rispettivi nomi. Quando premi un pulsante del filtro, dovresti vedere il suo testo prendere un nuovo contorno — questo ti dice che è stato selezionato. E se guardi l'Inspector della Pagina dei tuoi DevTool mentre clicchi sui pulsanti, vedrai i valori dell'attributo `aria-pressed` cambiare di conseguenza.

![I tre pulsanti di filtro dell'app - tutte, attive e completate - con un'evidenziazione del focus attorno al completato](filter-buttons.png)

Tuttavia, i nostri pulsanti ancora non filtrano effettivamente i todo nell'interfaccia utente! Concludiamo questo.

### Filtrare le attività nell'interfaccia utente

In questo momento, la nostra costante `taskList` in `App()` esegue una mappatura sullo stato delle attività e restituisce un nuovo componente `<Todo />` per tutte. Questo non è ciò che vogliamo! Un'attività dovrebbe essere resa solo se inclusa nei risultati dell'applicazione del filtro selezionato. Prima di eseguire la mappatura sullo stato delle attività, dovremmo filtrarlo (con [`Array.prototype.filter()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)) per eliminare gli oggetti che non vogliamo rendere.

Aggiorna il tuo `taskList` come segue:

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

Per decidere quale funzione di callback utilizzare in `Array.prototype.filter()`, accediamo al valore in `FILTER_MAP` che corrisponde alla chiave del nostro stato di filtro. Quando il filtro è `All`, ad esempio, `FILTER_MAP[filter]` valuterà `() => true`.

Scegliere un filtro nel tuo browser ora rimuoverà le attività che non soddisfano i suoi criteri. Anche il conteggio nel titolo sopra l'elenco cambierà per riflettere l'elenco!

![L'app con i pulsanti di filtro in posizione. Attivo è evidenziato, quindi vengono visualizzati solo gli elementi todo attivi.](filtered-todo-list.png)

## Sommario

Quindi è tutto — la nostra app è ora completa dal punto di vista funzionale. Tuttavia, ora che abbiamo implementato tutte le nostre funzionalità, possiamo fare alcune migliorie per garantire che una gamma più ampia di utenti possa utilizzare la nostra app. Nel nostro prossimo articolo, concluderemo i nostri tutorial React esaminando l'inclusione della gestione del focus in React, il che può migliorare l'usabilità e ridurre la confusione per utenti che utilizzano solo la tastiera e lettori di schermo.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state","Learn_web_development/Core/Frameworks_libraries/React_accessibility", "Learn_web_development/Core/Frameworks_libraries")}}
