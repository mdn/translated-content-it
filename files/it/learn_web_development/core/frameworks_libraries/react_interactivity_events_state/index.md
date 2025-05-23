---
title: "Interattività in React: Eventi e stato"
short-title: Eventi e stato in React
slug: Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state
l10n:
  sourceCommit: 611edf6335e4a833a6f394d0d98b117e7b0a36bf
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_components","Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering", "Learn_web_development/Core/Frameworks_libraries")}}

Con il piano del nostro componente definito, è ora di iniziare ad aggiornare la nostra app da un'interfaccia utente completamente statica a una che consente effettivamente di interagire e modificare le cose. In questo articolo lo faremo, addentrandoci negli eventi e nello stato lungo il percorso, e finendo con un'app nella quale possiamo aggiungere e eliminare con successo attività e contrassegnarle come completate.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi core <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/riga di comando</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        Gestione degli eventi e dello stato in React, e utilizzo di questi per
        iniziare a rendere interattiva l'app di studio del caso.
      </td>
    </tr>
  </tbody>
</table>

## Gestione degli eventi

Se finora hai scritto solo JavaScript puro, potresti essere abituato ad avere un file JavaScript separato in cui cerchi alcuni nodi DOM e vi colleghi dei listener. Ad esempio, un file HTML potrebbe contenere un pulsante, come questo:

```html
<button type="button">Say hi!</button>
```

E un file JavaScript potrebbe contenere un codice simile a questo:

```js
const btn = document.querySelector("button");

btn.addEventListener("click", () => {
  alert("hi!");
});
```

In JSX, il codice che descrive l'interfaccia utente vive proprio accanto ai nostri listener di eventi:

```jsx
<button type="button" onClick={() => alert("hi!")}>
  Say hi!
</button>
```

In questo esempio, stiamo aggiungendo un attributo `onClick` all'elemento {{htmlelement("button")}}. Il valore di quell'attributo è una funzione che attiva un alert. Questo potrebbe sembrare contrario alle migliori pratiche secondo cui non bisogna scrivere listener di eventi in HTML, ma ricorda: JSX non è HTML.

L'attributo `onClick` ha un significato speciale qui: dice a React di eseguire una funzione data quando l'utente fa clic sul pulsante. Ci sono un paio di altre cose da notare:

- La natura {{Glossary("camel_case", "camel-case")}} di `onClick` è importante — JSX non riconoscerà `onclick` (ancora una volta, è già utilizzato in JavaScript per uno scopo specifico, che è correlato ma diverso — proprietà handler standard [`onclick`](/it/docs/Web/API/Element/click_event)).
- Tutti gli eventi del browser seguono questo formato in JSX – `on`, seguito dal nome dell'evento.

Applichiamo questo alla nostra app, iniziando nel componente `Form.jsx`.

### Gestione dell'invio del modulo

All'inizio della funzione del componente `Form()` (cioè, appena sotto la riga `function Form() {`), crea una funzione chiamata `handleSubmit()`. Questa funzione dovrebbe [impedire il comportamento predefinito dell'evento `submit`](/it/docs/Learn_web_development/Core/Scripting/Events#preventing_default_behavior). Dopo di che, dovrebbe attivare un `alert()`, che può dire quello che vuoi. Dovrebbe infine apparire qualcosa di simile a questo:

```jsx
function handleSubmit(event) {
  event.preventDefault();
  alert("Hello, world!");
}
```

Per utilizzare questa funzione, aggiungi un attributo `onSubmit` all'elemento [`<form>`](/it/docs/Web/HTML/Reference/Elements/form) e imposta il suo valore sulla funzione `handleSubmit`:

```jsx
<form onSubmit={handleSubmit}>{/* … */}</form>
```

Ora, se torni al tuo browser e fai clic sul pulsante "Aggiungi", il tuo browser ti mostrerà una finestra di dialogo di alert con le parole "Hello, world!" - o qualunque cosa tu abbia scelto di scrivere lì.

## Props di callback

Nelle applicazioni React, l'interattività è raramente confinata a un solo componente: gli eventi che accadono in un componente influenzeranno altre parti dell'app. Quando iniziamo a darci il potere di creare nuove attività, le cose che accadono nel componente `<Form />` influenzeranno l'elenco visualizzato in `<App />`.

Vogliamo che la nostra funzione `handleSubmit()` ci aiuti a creare una nuova attività, quindi abbiamo bisogno di un modo per passare le informazioni da `<Form />` a `<App />`. Non possiamo trasferire dati dal figlio al genitore allo stesso modo in cui passiamo i dati dal genitore al figlio usando le props normali. Invece, possiamo scrivere una funzione in `<App />` che si aspetterà alcuni dati dal nostro modulo come input, quindi passare quella funzione a `<Form />` come prop. Questa funzione-come-prop si chiama **prop di callback**. Una volta che abbiamo la nostra prop di callback, possiamo chiamarla all'interno di `<Form />` per inviare i dati giusti a `<App />`.

### Gestione dell'invio del modulo tramite callback

All'interno della funzione `App()` in `App.jsx`, crea una funzione chiamata `addTask()` che ha un singolo parametro di `name`:

```jsx
function addTask(name) {
  alert(name);
}
```

Successivamente, passa `addTask()` in `<Form />` come prop. La prop può avere qualsiasi nome tu voglia, ma scegli un nome che comprenderai in seguito. Qualcosa come `addTask` funziona, perché corrisponde al nome della funzione così come a ciò che la funzione farà. La tua chiamata al componente `<Form />` dovrebbe essere aggiornata come segue:

```jsx
<Form addTask={addTask} />
```

Per utilizzare questa prop, dobbiamo cambiare la firma della funzione `Form()` in `Form.jsx`, in modo che accetti `props` come parametro:

```jsx
function Form(props) {
  // …
}
```

Infine, possiamo usare questa prop all'interno della funzione `handleSubmit()` nel tuo componente `<Form />`! Aggiorna come segue:

```jsx
function handleSubmit(event) {
  event.preventDefault();
  props.addTask("Say hello!");
}
```

Cliccando sul pulsante "Aggiungi" nel tuo browser dimostrerà che la funzione di callback `addTask()` funziona, ma sarebbe bello se potessimo fare in modo che l'alert mostrasse ciò che stiamo digitando nel nostro campo di input! Questo è ciò che faremo ora.

### A parte: una nota sulle convenzioni di denominazione

Abbiamo passato la funzione `addTask()` al componente `<Form />` come prop `addTask` affinché la relazione tra la _funzione_ `addTask()` e la _prop_ `addTask` fosse il più chiara possibile. Tieni presente, però, che i nomi delle props non _devono_ essere in particolare nulla. Avremmo potuto passare `addTask()` in `<Form />` sotto un altro nome, come questo:

```diff
- <Form addTask={addTask} />
+ <Form onSubmit={addTask} />
```

Questo renderebbe la funzione `addTask()` disponibile per il componente `<Form />` come prop `onSubmit`. Quella prop potrebbe essere usata in `Form.jsx` come questo:

```diff
function handleSubmit(event) {
  event.preventDefault();
- props.addTask("Say hello!");
+ props.onSubmit("Say hello!");
}
```

Qui, il prefisso `on` ci dice che la prop è una funzione di callback; `Submit` è il nostro indizio che un evento di submit attiverà questa funzione.

Mentre le props di callback corrispondono spesso ai nomi degli handler di eventi familiari, come `onSubmit` o `onClick`, possono essere chiamate praticamente con qualsiasi nome che aiuti a renderne chiaro il significato. Un ipotetico componente `<Menu />` potrebbe includere una funzione di callback che viene eseguita quando il menu viene aperto, oltre a una funzione di callback separata che viene eseguita quando viene chiuso:

```jsx
<Menu onOpen={() => console.log("Hi!")} onClose={() => console.log("Bye!")} />
```

Questa convenzione di denominazione `on*` è molto comune nell'ecosistema React, quindi tienila a mente mentre continui il tuo apprendimento. Per ragioni di chiarezza, continueremo a utilizzare `addTask` e nomi di prop simili per il resto di questo tutorial. Se hai cambiato i nomi delle props mentre leggi questa sezione, assicurati di cambiarli di nuovo prima di continuare!

## Persistenza e modifica dei dati con lo stato

Finora abbiamo usato le props per trasferire i dati attraverso i nostri componenti e ciò ci è servito bene. Tuttavia, ora che stiamo gestendo l'interattività, abbiamo bisogno della capacità di creare nuovi dati, conservarli e aggiornarli successivamente. Le props non sono lo strumento giusto per questo lavoro perché sono immutabili: un componente non può cambiare o creare le proprie props.

Qui entra in gioco lo **stato**. Se pensiamo alle props come a un modo per comunicare tra i componenti, possiamo pensare allo stato come a un modo per dare ai componenti una "memoria" – informazioni che possono conservare e aggiornare secondo necessità.

React fornisce una funzione speciale per introdurre lo stato in un componente, chiamata appropriatamente `useState()`.

> **Nota:** `useState()` fa parte di una categoria speciale di funzioni chiamate **hook**, ognuna delle quali può essere utilizzata per aggiungere nuove funzionalità a un componente. Impareremo altri hook più avanti.

Per usare `useState()`, dobbiamo importarlo dal modulo React. Aggiungi la seguente riga all'inizio del tuo file `Form.jsx`, sopra la definizione della funzione `Form()`:

```jsx
import { useState } from "react";
```

`useState()` accetta un singolo argomento che determina il valore iniziale dello stato. Questo argomento può essere una stringa, un numero, un array, un oggetto o qualsiasi altro tipo di dato JavaScript. `useState()` restituisce un array contenente due elementi. Il primo elemento è il valore corrente dello stato; il secondo elemento è una funzione che può essere usata per aggiornare lo stato.

Creiamo uno stato `name`. Scrivi quanto segue sopra la tua funzione `handleSubmit()`, all'interno di `Form()`:

```jsx
const [name, setName] = useState("Learn React");
```

Diverse cose stanno accadendo in questa riga di codice:

- Stiamo definendo una costante `name` con il valore `"Learn React"`.
- Stiamo definendo una funzione il cui compito è modificare `name`, chiamata `setName()`.
- `useState()` restituisce queste due cose in un array, quindi stiamo usando [la destrutturazione degli array](/it/docs/Web/JavaScript/Reference/Operators/Destructuring) per catturarle entrambe in variabili separate.

### Lettura dello stato

Puoi vedere lo stato `name` in azione immediatamente. Aggiungi un attributo `value` all'input del modulo e imposta il suo valore su `name`. Il tuo browser visualizzerà "Learn React" all'interno dell'input.

```jsx
<input
  type="text"
  id="new-todo-input"
  className="input input__lg"
  name="text"
  autoComplete="off"
  value={name}
/>
```

Cambia "Learn React" in una stringa vuota una volta che hai finito; questo è quello che vogliamo per il nostro stato iniziale:

```jsx
const [name, setName] = useState("");
```

### Lettura dell'input dell'utente

Prima di poter cambiare il valore di `name`, abbiamo bisogno di catturare l'input di un utente mentre digita. Per questo possiamo ascoltare l'evento `onChange`. Scriviamo una funzione `handleChange()` e ascoltiamo per essa sull'elemento `<input />`.

```jsx
// near the top of the `Form` component
function handleChange() {
  console.log("Typing!");
}

// …

// Down in the return statement
<input
  type="text"
  id="new-todo-input"
  className="input input__lg"
  name="text"
  autoComplete="off"
  value={name}
  onChange={handleChange}
/>;
```

Attualmente, il valore del nostro input non cambierà quando provi a inserire del testo, ma il tuo browser registrerà la parola "Typing!" nella console di JavaScript, quindi sappiamo che il nostro listener di eventi è collegato all'input.

Per leggere i tasti dell'utente, dobbiamo accedere alla proprietà `value` dell'input. Possiamo farlo leggendo l'oggetto `event` che `handleChange()` riceve quando viene chiamata. `event`, a sua volta, ha [una proprietà `target`](/it/docs/Web/API/Event/target), che rappresenta l'elemento che ha generato l'evento di `change`. Questo è il nostro input. Quindi, `event.target.value` è il testo all'interno dell'input.

Puoi `console.log()` questo valore per vederlo nella console del tuo browser. Prova ad aggiornare la funzione `handleChange()` come segue e a digitare nell'input per vedere il risultato nella tua console:

```jsx
function handleChange(event) {
  console.log(event.target.value);
}
```

### Aggiornamento dello stato

Loggare non basta: vogliamo effettivamente memorizzare ciò che l'utente digita e visualizzarlo nell'input! Cambia la tua chiamata a `console.log()` in `setName()`, come mostrato di seguito:

```jsx
function handleChange(event) {
  setName(event.target.value);
}
```

Ora quando digiti nell'input, i tuoi tasti riempiranno l'input, come ti aspetti.

Abbiamo un altro passo: dobbiamo cambiare la nostra funzione `handleSubmit()` in modo che chiami `props.addTask` con `name` come argomento. Ricordi la nostra prop di callback? Questo servirà a inviare l'attività al componente `App`, in modo da poterla aggiungere al nostro elenco di attività in un secondo momento. Come buona pratica, dovresti cancellare l'input dopo che il tuo modulo è stato inviato, quindi chiameremo `setName()` di nuovo con una stringa vuota per farlo:

```jsx
function handleSubmit(event) {
  event.preventDefault();
  props.addTask(name);
  setName("");
}
```

Finalmente, puoi digitare qualcosa nel campo di input nel tuo browser e fare clic su _Aggiungi_ — qualunque cosa tu abbia digitato apparirà in una finestra di dialogo di alert.

Il tuo file `Form.jsx` ora dovrebbe apparire così:

```jsx
import { useState } from "react";

function Form(props) {
  const [name, setName] = useState("");

  function handleChange(event) {
    setName(event.target.value);
  }

  function handleSubmit(event) {
    event.preventDefault();
    props.addTask(name);
    setName("");
  }

  return (
    <form onSubmit={handleSubmit}>
      <h2 className="label-wrapper">
        <label htmlFor="new-todo-input" className="label__lg">
          What needs to be done?
        </label>
      </h2>
      <input
        type="text"
        id="new-todo-input"
        className="input input__lg"
        name="text"
        autoComplete="off"
        value={name}
        onChange={handleChange}
      />
      <button type="submit" className="btn btn__primary btn__lg">
        Add
      </button>
    </form>
  );
}

export default Form;
```

> [!NOTE]
> Noterai che puoi inviare attività vuote semplicemente premendo il pulsante `Aggiungi` senza inserire un nome dell'attività. Puoi pensare a un modo per impedirlo? Come suggerimento, probabilmente devi aggiungere un tipo di controllo nella funzione `handleSubmit()`.

## Mettere tutto insieme: Aggiunta di un'attività

Ora che abbiamo praticato con eventi, props di callback e hook, siamo pronti a scrivere la funzionalità che consentirà a un utente di aggiungere una nuova attività dal proprio browser.

### Attività come stato

Dobbiamo importare `useState` in `App.jsx` in modo da poter memorizzare le nostre attività nello stato. Aggiungi quanto segue all'inizio del tuo file `App.jsx`:

```jsx
import { useState } from "react";
```

Vogliamo passare `props.tasks` nella funzione `useState()` — questo conserverà il suo stato iniziale. Aggiungi quanto segue proprio all'inizio della tua definizione di funzione `App()`:

```jsx
const [tasks, setTasks] = useState(props.tasks);
```

Ora possiamo cambiare il nostro mapping `taskList` in modo che sia il risultato del mapping `tasks`, invece di `props.tasks`. La tua dichiarazione della costante `taskList` dovrebbe ora apparire così:

```jsx
const taskList = tasks?.map((task) => (
  <Todo
    id={task.id}
    name={task.name}
    completed={task.completed}
    key={task.id}
  />
));
```

### Aggiunta di un'attività

Ora abbiamo un hook `setTasks` che possiamo usare nella nostra funzione `addTask()` per aggiornare il nostro elenco di attività. C'è un problema tuttavia: non possiamo semplicemente passare l'argomento `name` di `addTask()` in `setTasks`, perché `tasks` è un array di oggetti e `name` è una stringa. Se provassimo a farlo, l'array sarebbe sostituito con la stringa.

Prima di tutto, dobbiamo mettere `name` in un oggetto che abbia la stessa struttura delle nostre attività esistenti. All'interno della funzione `addTask()`, creeremo un oggetto `newTask` da aggiungere all'array.

Dobbiamo quindi creare un nuovo array con questa nuova attività aggiunta e aggiornare lo stato dei dati delle attività a questo nuovo stato. Per farlo, possiamo usare la sintassi spread per [copiare l'array esistente](/it/docs/Web/JavaScript/Reference/Operators/Spread_syntax#copying_an_array) e aggiungere il nostro oggetto alla fine. Passiamo quindi questo array in `setTasks()` per aggiornare lo stato.

Mettendo tutto insieme, la tua funzione `addTask()` dovrebbe apparire così:

```jsx
function addTask(name) {
  const newTask = { id: "id", name, completed: false };
  setTasks([...tasks, newTask]);
}
```

Ora puoi usare il browser per aggiungere un'attività ai nostri dati! Digita qualsiasi cosa nel modulo e clicca su "Aggiungi" (oppure premi il tasto <kbd>Invio</kbd>) e vedrai apparire il tuo nuovo elemento todo nell'interfaccia utente!

**Tuttavia, abbiamo un altro problema**: la nostra funzione `addTask()` sta dando lo stesso `id` a ogni attività. Questo è negativo per l'accessibilità e rende impossibile per React distinguere i compiti futuri con la prop `key`. Infatti, React ti darà un avviso nella tua console DevTools — "Avvertimento: rinvenuti due figli con la stessa chiave…"

Dobbiamo risolverlo. Creare identificatori univoci è un problema difficile – uno per cui la comunità JavaScript ha scritto alcune librerie utili. Useremo [nanoid](https://github.com/ai/nanoid) perché è piccola e funziona.

Assicurati di essere nella directory principale della tua applicazione ed esegui il seguente comando nel terminale:

```bash
npm install nanoid
```

> [!NOTE]
> Se stai usando yarn, avrai bisogno invece della seguente: `yarn add nanoid`.

Ora possiamo usare `nanoid` per creare ID unici per le nostre nuove attività. Per prima cosa, importalo includendo la seguente riga all'inizio di `App.jsx`:

```jsx
import { nanoid } from "nanoid";
```

Ora aggiorniamo `addTask()` in modo che ogni ID attività diventi un prefisso `todo-` più una stringa univoca generata da nanoid. Aggiorna la tua dichiarazione della costante `newTask` con questo:

```jsx
const newTask = { id: `todo-${nanoid()}`, name, completed: false };
```

Salva tutto e prova di nuovo la tua app — ora puoi aggiungere attività senza ricevere quell'avviso sugli ID duplicati.

## Deviazione: conteggio delle attività

Ora che possiamo aggiungere nuove attività, potresti notare un problema: il nostro titolo dice "3 attività rimanenti" indipendentemente da quante attività abbiamo! Possiamo risolvere questo problema contando la lunghezza di `taskList` e cambiando il testo del nostro titolo di conseguenza.

Aggiungi questo all'interno della definizione della tua funzione `App()`, prima della dichiarazione di ritorno:

```jsx
const headingText = `${taskList.length} tasks remaining`;
```

Questo è quasi giusto, tranne per il fatto che se la nostra lista dovesse mai contenere una sola attività, il titolo continuerebbe a usare la parola "attività". Anche questa può diventare una variabile. Aggiorna il codice che hai appena aggiunto come segue:

```jsx
const tasksNoun = taskList.length !== 1 ? "tasks" : "task";
const headingText = `${taskList.length} ${tasksNoun} remaining`;
```

Ora puoi sostituire il contenuto di testo dell'intestazione della lista con la variabile `headingText`. Aggiorna il tuo `<h2>` in questo modo:

```jsx
<h2 id="list-heading">{headingText}</h2>
```

Salva il file, torna al tuo browser e prova ad aggiungere alcune attività: il conteggio dovrebbe ora aggiornarsi come previsto.

## Completamento di un'attività

Potresti notare che, quando fai clic su una casella di controllo, si seleziona e si deseleziona appropriatamente. Come caratteristica dell'HTML, il browser sa come ricordare quali input checkbox sono selezionati o deselezionati senza il nostro aiuto. Questa caratteristica nasconde tuttavia un problema: il fatto di selezionare una casella di controllo non cambia lo stato nella nostra applicazione React. Ciò significa che il browser e la nostra app sono ora fuori sincrono. Dobbiamo scrivere il nostro codice per riportare il browser in sincronia con la nostra app.

### Dimostrare il bug

Prima di risolvere il problema, osserviamolo accadere.

Inizieremo scrivendo una funzione `toggleTaskCompleted()` nel nostro componente `App()`. Questa funzione avrà un parametro `id`, ma non lo useremo ancora. Per ora, registreremo il primo task nell'array sulla console – osserveremo cosa succede quando lo controlliamo o lo deselezioniamo nel nostro browser:

Aggiungi questo proprio sopra la dichiarazione della tua costante `taskList`:

```jsx
function toggleTaskCompleted(id) {
  console.log(tasks[0]);
}
```

Successivamente, aggiungeremo `toggleTaskCompleted` alle props di ogni componente `<Todo />` renderizzato all'interno del nostro `taskList`; aggiornalo in questo modo:

```jsx
const taskList = tasks.map((task) => (
  <Todo
    id={task.id}
    name={task.name}
    completed={task.completed}
    key={task.id}
    toggleTaskCompleted={toggleTaskCompleted}
  />
));
```

Successivamente, vai al tuo componente `Todo.jsx` e aggiungi un gestore `onChange` al tuo elemento `<input />`, che dovrebbe utilizzare una funzione anonima per chiamare `props.toggleTaskCompleted()` con un parametro di `props.id`. L'elemento `<input />` dovrebbe ora apparire così:

```jsx
<input
  id={props.id}
  type="checkbox"
  defaultChecked={props.completed}
  onChange={() => props.toggleTaskCompleted(props.id)}
/>
```

Salva tutto e torna al tuo browser e noti che il nostro primo task, Mangiare, è selezionato. Apri la tua console di JavaScript, quindi fai clic sulla casella di controllo accanto a Mangiare. Si deseleziona, come ci aspettiamo. La tua console di JavaScript, tuttavia, registrerà qualcosa di simile a questo:

```plain
Object { id: "task-0", name: "Eat", completed: true }
```

La casella di controllo si deseleziona nel browser, ma la nostra console ci dice che Mangiare è ancora completato. Lo sistemeremo subito!

### Sincronizzazione del browser con i nostri dati

Rivediamo la nostra funzione `toggleTaskCompleted()` in `App.jsx`. Vogliamo che cambi la proprietà `completed` solo dell'attività che è stata alternata e lasci tutte le altre invariate. Per fare ciò, faremo il `map()` dell'elenco delle attività e cambieremo solo quella che abbiamo completato.

Aggiorna la tua funzione `toggleTaskCompleted()` come segue:

```jsx
function toggleTaskCompleted(id) {
  const updatedTasks = tasks.map((task) => {
    // if this task has the same ID as the edited task
    if (id === task.id) {
      // use object spread to make a new object
      // whose `completed` prop has been inverted
      return { ...task, completed: !task.completed };
    }
    return task;
  });
  setTasks(updatedTasks);
}
```

Qui, definiamo una costante `updatedTasks` che esegue il mapping sull'array `tasks` originale. Se la proprietà `id` dell'attività corrisponde all'`id` fornito alla funzione, utilizziamo [la sintassi spread degli oggetti](/it/docs/Web/JavaScript/Reference/Operators/Spread_syntax) per creare un nuovo oggetto e alterniamo la proprietà `completed` di quell'oggetto prima di restituirlo. Se non corrisponde, restituiamo l'oggetto originale.

Quindi chiamiamo `setTasks()` con questo nuovo array per aggiornare il nostro stato.

## Eliminare un'attività

L'eliminazione di un'attività seguirà uno schema simile all'alternanza del suo stato completato: dobbiamo definire una funzione per aggiornare il nostro stato, quindi passare quella funzione in `<Todo />` come prop e chiamarla quando si verifica l'evento giusto.

### Il callback prop `deleteTask`

Qui inizieremo scrivendo una funzione `deleteTask()` nel tuo componente `App`. Come `toggleTaskCompleted()`, questa funzione accetterà un parametro `id`, e per iniziare registreremo quell'`id` nella console. Aggiungi il seguente sotto `toggleTaskCompleted()`:

```jsx
function deleteTask(id) {
  console.log(id);
}
```

Successivamente, aggiungi un'altra prop di callback al nostro array di componenti `<Todo />`:

```jsx
const taskList = tasks.map((task) => (
  <Todo
    id={task.id}
    name={task.name}
    completed={task.completed}
    key={task.id}
    toggleTaskCompleted={toggleTaskCompleted}
    deleteTask={deleteTask}
  />
));
```

In `Todo.jsx`, vogliamo chiamare `props.deleteTask()` quando viene premuto il pulsante "Elimina". `deleteTask()` deve conoscere l'ID del task che l'ha chiamato, in modo da poter eliminare il task corretto dallo stato.

Aggiorna il pulsante "Elimina" all'interno di `Todo.jsx` come segue:

```jsx
<button
  type="button"
  className="btn btn__danger"
  onClick={() => props.deleteTask(props.id)}>
  Delete <span className="visually-hidden">{props.name}</span>
</button>
```

Ora, quando fai clic su uno dei pulsanti "Elimina" nell'app, la console del tuo browser dovrebbe registrare l'ID dell'attività correlata.

A questo punto, il tuo file `Todo.jsx` dovrebbe apparire così:

```jsx
function Todo(props) {
  return (
    <li className="todo stack-small">
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
    </li>
  );
}

export default Todo;
```

## Eliminare attività dallo stato e dall'interfaccia utente

Ora che sappiamo che `deleteTask()` è invocata correttamente, possiamo chiamare il nostro hook `setTasks()` in `deleteTask()` per eliminare effettivamente quel task dallo stato dell'app così come visivamente nell'interfaccia utente dell'app. Dato che `setTasks()` si aspetta un array come argomento, dovremmo fornirgli un nuovo array che copia i task esistenti, _escludendo_ il task il cui ID corrisponde a quello passato in `deleteTask()`.

Questa è l'opportunità perfetta per usare [`Array.prototype.filter()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/filter). Possiamo testare ciascun task ed escludere un task dal nuovo array se la sua prop `id` corrisponde all'argomento `id` passato in `deleteTask()`.

Aggiorna la funzione `deleteTask()` all'interno del tuo file `App.jsx` come segue:

```jsx
function deleteTask(id) {
  const remainingTasks = tasks.filter((task) => id !== task.id);
  setTasks(remainingTasks);
}
```

Prova di nuovo la tua app. Ora dovresti essere in grado di eliminare un'attività dalla tua app!

A questo punto, il tuo file `App.jsx` dovrebbe apparire così:

```jsx
import { useState } from "react";
import { nanoid } from "nanoid";
import Todo from "./components/Todo";
import Form from "./components/Form";
import FilterButton from "./components/FilterButton";

function App(props) {
  function addTask(name) {
    const newTask = { id: `todo-${nanoid()}`, name, completed: false };
    setTasks([...tasks, newTask]);
  }

  function toggleTaskCompleted(id) {
    const updatedTasks = tasks.map((task) => {
      // if this task has the same ID as the edited task
      if (id === task.id) {
        // use object spread to make a new object
        // whose `completed` prop has been inverted
        return { ...task, completed: !task.completed };
      }
      return task;
    });
    setTasks(updatedTasks);
  }

  function deleteTask(id) {
    const remainingTasks = tasks.filter((task) => id !== task.id);
    setTasks(remainingTasks);
  }

  const [tasks, setTasks] = useState(props.tasks);
  const taskList = tasks?.map((task) => (
    <Todo
      id={task.id}
      name={task.name}
      completed={task.completed}
      key={task.id}
      toggleTaskCompleted={toggleTaskCompleted}
      deleteTask={deleteTask}
    />
  ));

  const tasksNoun = taskList.length !== 1 ? "tasks" : "task";
  const headingText = `${taskList.length} ${tasksNoun} remaining`;

  return (
    <div className="todoapp stack-large">
      <h1>TodoMatic</h1>
      <Form addTask={addTask} />
      <div className="filters btn-group stack-exception">
        <FilterButton />
        <FilterButton />
        <FilterButton />
      </div>
      <h2 id="list-heading">{headingText}</h2>
      <ul
        role="list"
        className="todo-list stack-large stack-exception"
        aria-labelledby="list-heading">
        {taskList}
      </ul>
    </div>
  );
}

export default App;
```

## Sommario

Ce n'è abbastanza per un solo articolo. Qui ti abbiamo fornito un'analisi su come React gestisce gli eventi e lo stato, e implementato funzionalità per aggiungere attività, eliminare attività e contrassegnare le attività come completate. Ci siamo quasi. Nel prossimo articolo implementeremo funzionalità per modificare le attività esistenti e filtrare l'elenco delle attività tra tutte, completate e incomplete. Guarderemo anche il rendering condizionale dell'interfaccia utente lungo il percorso.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_components","Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering", "Learn_web_development/Core/Frameworks_libraries")}}
