---
title: "Interattività React: eventi e stato"
short-title: Eventi e stato in React
slug: Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state
l10n:
  sourceCommit: 6ba4f3b350be482ba22726f31bbcf8ad3c92a9c6
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_components","Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering", "Learn_web_development/Core/Frameworks_libraries")}}

Dopo aver definito il piano dei componenti, è ora di iniziare ad aggiornare l'app, trasformandola da un'interfaccia utente completamente statica in una che consenta effettivamente di interagire e modificare elementi. In questo articolo verrà fatto proprio questo, analizzando eventi e stato e arrivando infine a un'app in cui è possibile aggiungere ed eliminare attività e contrassegnarle come completate.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi fondamentali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, nonché con il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/riga di comando</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        Gestione di eventi e stato in React e uso di questi strumenti per
        iniziare a rendere interattiva l'app del caso di studio.
      </td>
    </tr>
  </tbody>
</table>

## Gestione degli eventi

Chi ha scritto finora soltanto JavaScript vanilla potrebbe essere abituato ad avere un file JavaScript separato in cui interrogare alcuni nodi DOM e collegarvi dei listener. Per esempio, un file HTML potrebbe contenere un pulsante, come questo:

```html
<button type="button">Say hi!</button>
```

E un file JavaScript potrebbe contenere del codice come questo:

```js
const btn = document.querySelector("button");

btn.addEventListener("click", () => {
  alert("hi!");
});
```

In JSX, il codice che descrive l'interfaccia utente si trova accanto ai listener degli eventi:

```jsx
<button type="button" onClick={() => alert("hi!")}>
  Say hi!
</button>
```

In questo esempio, viene aggiunto un attributo `onClick` all'elemento {{htmlelement("button")}}. Il valore di tale attributo è una funzione che attiva un avviso. Questo potrebbe sembrare contrario alle buone pratiche che sconsigliano di scrivere listener di eventi in HTML, ma è importante ricordare che JSX non è HTML.

L'attributo `onClick` ha qui un significato speciale: comunica a React di eseguire una determinata funzione quando l'utente fa clic sul pulsante. Ci sono un paio di altri aspetti da notare:

- La forma {{Glossary("camel_case", "camelCase")}} di `onClick` è importante: JSX non riconoscerà `onclick` (anche questo è già usato in JavaScript per uno scopo specifico, correlato ma diverso, ovvero le proprietà standard dei gestori [`onclick`](/it/docs/Web/API/Element/click_event)).
- Tutti gli eventi del browser seguono questo formato in JSX: `on`, seguito dal nome dell'evento.

Applichiamo tutto questo all'app, iniziando dal componente `Form.jsx`.

### Gestione dell'invio del modulo

All'inizio della funzione del componente `Form()` (ovvero subito sotto la riga `function Form() {`), creare una funzione chiamata `handleSubmit()`. Questa funzione deve [impedire il comportamento predefinito dell'evento `submit`](/it/docs/Learn_web_development/Core/Scripting/Events#preventing_default_behavior). Successivamente, deve attivare un `alert()`, che può contenere il messaggio desiderato. Il risultato dovrebbe essere simile al seguente:

```jsx
function handleSubmit(event) {
  event.preventDefault();
  alert("Hello, world!");
}
```

Per utilizzare questa funzione, aggiungere un attributo `onSubmit` all'elemento [`<form>`](/it/docs/Web/HTML/Reference/Elements/form) e impostarne il valore sulla funzione `handleSubmit`:

```jsx
<form onSubmit={handleSubmit}>{/* … */}</form>
```

Ora, tornando al browser e facendo clic sul pulsante "Add", il browser mostrerà una finestra di avviso con le parole "Hello, world!" oppure qualunque altro testo sia stato scelto.

## Callback props

Nelle applicazioni React, l'interattività è raramente limitata a un solo componente: gli eventi che si verificano in un componente influenzano altre parti dell'app. Quando si inizia a poter creare nuove attività, ciò che avviene nel componente `<Form />` influenzerà l'elenco visualizzato in `<App />`.

Si vuole che la funzione `handleSubmit()` contribuisca in definitiva a creare una nuova attività, quindi è necessario un modo per passare informazioni da `<Form />` a `<App />`. Non è possibile passare dati dal figlio al genitore nello stesso modo in cui si passano dati dal genitore al figlio usando le props standard. È invece possibile scrivere una funzione in `<App />` che si aspetti alcuni dati dal modulo come input, quindi passare tale funzione a `<Form />` come prop. Questa funzione passata come prop è chiamata **callback prop**. Una volta ottenuta la callback prop, è possibile chiamarla all'interno di `<Form />` per inviare i dati corretti a `<App />`.

### Gestione dell'invio del modulo tramite callback

All'interno della funzione `App()` in `App.jsx`, creare una funzione chiamata `addTask()` che abbia un singolo parametro, `name`:

```jsx
function addTask(name) {
  alert(name);
}
```

Successivamente, passare `addTask()` a `<Form />` come prop. La prop può avere qualsiasi nome, ma è preferibile scegliere un nome che sia comprensibile in seguito. Qualcosa come `addTask` funziona bene perché corrisponde sia al nome della funzione sia all'azione che la funzione eseguirà. La chiamata al componente `<Form />` dovrebbe essere aggiornata come segue:

```jsx
<Form addTask={addTask} />
```

Per usare questa prop, è necessario modificare la firma della funzione `Form()` in `Form.jsx` affinché accetti `props` come parametro:

```jsx
function Form(props) {
  // …
}
```

Infine, è possibile usare questa prop all'interno della funzione `handleSubmit()` nel componente `<Form />`! Aggiornarla nel modo seguente:

```jsx
function handleSubmit(event) {
  event.preventDefault();
  props.addTask("Say hello!");
}
```

Facendo clic sul pulsante "Add" nel browser verrà dimostrato che la funzione callback `addTask()` funziona, ma sarebbe utile fare in modo che l'avviso mostri ciò che viene digitato nel campo di input. Questo è ciò che verrà fatto successivamente.

### Approfondimento: una nota sulle convenzioni di denominazione

La funzione `addTask()` è stata passata al componente `<Form />` come prop `addTask`, in modo che la relazione tra la _funzione_ `addTask()` e la _prop_ `addTask` rimanesse il più chiara possibile. Va tuttavia tenuto presente che i nomi delle props non _devono_ necessariamente essere specifici. Si sarebbe potuto passare `addTask()` a `<Form />` con qualunque altro nome, ad esempio:

```diff
- <Form addTask={addTask} />
+ <Form onSubmit={addTask} />
```

Questo renderebbe la funzione `addTask()` disponibile al componente `<Form />` come prop `onSubmit`. Questa prop potrebbe essere usata in `Form.jsx` in questo modo:

```diff
function handleSubmit(event) {
  event.preventDefault();
- props.addTask("Say hello!");
+ props.onSubmit("Say hello!");
}
```

Qui, il prefisso `on` indica che la prop è una funzione callback; `Submit` suggerisce che un evento di invio attiverà questa funzione.

Sebbene le callback props corrispondano spesso ai nomi di gestori di eventi familiari, come `onSubmit` o `onClick`, possono essere chiamate praticamente in qualsiasi modo che renda chiaro il loro significato. Un ipotetico componente `<Menu />` potrebbe includere una funzione callback eseguita quando il menu viene aperto, oltre a una funzione callback separata eseguita quando viene chiuso:

```jsx
<Menu onOpen={() => console.log("Hi!")} onClose={() => console.log("Bye!")} />
```

Questa convenzione di denominazione `on*` è molto comune nell'ecosistema React, quindi è bene tenerla a mente durante l'apprendimento. Per chiarezza, nel resto di questo tutorial verranno mantenuti i nomi delle props `addTask` e simili. Se durante la lettura di questa sezione sono stati modificati dei nomi di props, assicurarsi di ripristinarli prima di continuare.

## Conservare e modificare dati con lo stato

Finora sono state usate le props per passare dati attraverso i componenti e questo è stato sufficiente. Ora che si sta gestendo l'interattività, tuttavia, serve la capacità di creare nuovi dati, conservarli e aggiornarli in seguito. Le props non sono lo strumento adatto per questo compito, perché sono immutabili: un componente non può modificare né creare le proprie props.

Qui entra in gioco lo **stato**. Se si pensa alle props come a un modo per comunicare tra componenti, si può pensare allo stato come a un modo per fornire ai componenti una "memoria": informazioni che possono conservare e aggiornare secondo necessità.

React fornisce una funzione speciale per introdurre lo stato in un componente, chiamata opportunamente `useState()`.

> [!NOTE]
> `useState()` fa parte di una categoria speciale di funzioni chiamate **hooks**, ciascuna delle quali può essere usata per aggiungere nuove funzionalità a un componente. Altri hook verranno analizzati in seguito.

Per usare `useState()`, è necessario importarla dal modulo React. Aggiungere la seguente riga all'inizio del file `Form.jsx`, sopra la definizione della funzione `Form()`:

```jsx
import { useState } from "react";
```

`useState()` accetta un singolo argomento che determina il valore iniziale dello stato. Questo argomento può essere una stringa, un numero, un array, un oggetto o qualsiasi altro tipo di dato JavaScript. `useState()` restituisce un array contenente due elementi. Il primo elemento è il valore corrente dello stato; il secondo elemento è una funzione che può essere usata per aggiornare lo stato.

Creiamo uno stato `name`. Scrivere quanto segue sopra la funzione `handleSubmit()`, all'interno di `Form()`:

```jsx
const [name, setName] = useState("Learn React");
```

In questa riga di codice avvengono diverse operazioni:

- Viene definita una costante `name` con il valore `"Learn React"`.
- Viene definita una funzione il cui compito è modificare `name`, chiamata `setName()`.
- `useState()` restituisce questi due elementi in un array, quindi viene usato il [destructuring degli array](/it/docs/Web/JavaScript/Reference/Operators/Destructuring) per acquisirli entrambi in variabili separate.

### Lettura dello stato

È possibile osservare subito lo stato `name` in azione. Aggiungere un attributo `value` all'input del modulo e impostarne il valore su `name`. Il browser visualizzerà "Learn React" all'interno dell'input.

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

Al termine, modificare "Learn React" in una stringa vuota; questo è il valore desiderato per lo stato iniziale:

```jsx
const [name, setName] = useState("");
```

### Lettura dell'input dell'utente

Prima di poter modificare il valore di `name`, è necessario acquisire l'input dell'utente mentre digita. A questo scopo, è possibile ascoltare l'evento `onChange`. Scriviamo una funzione `handleChange()` e associamola all'elemento `<input />`.

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

Attualmente, il valore dell'input non cambierà quando si prova a inserire testo, ma il browser registrerà la parola "Typing!" nella console JavaScript, quindi si sa che il listener dell'evento è associato all'input.

Per leggere le sequenze di tasti dell'utente, è necessario accedere alla proprietà `value` dell'input. Questo è possibile leggendo l'oggetto `event` ricevuto da `handleChange()` quando viene chiamata. `event`, a sua volta, ha [una proprietà `target`](/it/docs/Web/API/Event/target), che rappresenta l'elemento che ha generato l'evento `change`. Questo è l'input. Pertanto, `event.target.value` è il testo all'interno dell'input.

È possibile usare `console.log()` su questo valore per visualizzarlo nella console del browser. Provare ad aggiornare la funzione `handleChange()` come segue e digitare nell'input per vedere il risultato nella console:

```jsx
function handleChange(event) {
  console.log(event.target.value);
}
```

### Aggiornamento dello stato

La registrazione nella console non è sufficiente: si vuole memorizzare effettivamente ciò che l'utente digita e visualizzarlo nell'input. Modificare la chiamata a `console.log()` in `setName()`, come mostrato di seguito:

```jsx
function handleChange(event) {
  setName(event.target.value);
}
```

Ora, digitando nell'input, le sequenze di tasti riempiranno il campo come previsto.

Resta un ultimo passaggio: è necessario modificare la funzione `handleSubmit()` affinché chiami `props.addTask` con `name` come argomento. Ricordate la callback prop? Questa servirà a inviare l'attività al componente `App`, così da poterla aggiungere all'elenco delle attività in seguito. Come buona pratica, l'input dovrebbe essere svuotato dopo l'invio del modulo, quindi verrà chiamata nuovamente `setName()` con una stringa vuota:

```jsx
function handleSubmit(event) {
  event.preventDefault();
  props.addTask(name);
  setName("");
}
```

Finalmente, è possibile digitare qualcosa nel campo di input nel browser e fare clic su _Add_: qualunque testo venga digitato apparirà in una finestra di avviso.

Il file `Form.jsx` dovrebbe ora essere simile a questo:

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
> Si noterà che è possibile inviare attività vuote semplicemente premendo il pulsante `Add` senza inserire un nome per l'attività. È possibile pensare a un modo per impedirlo? Come suggerimento, probabilmente è necessario aggiungere una sorta di controllo nella funzione `handleSubmit()`.

## Mettere tutto insieme: aggiungere un'attività

Dopo aver fatto pratica con eventi, callback props e hook, è il momento di scrivere la funzionalità che permetterà all'utente di aggiungere una nuova attività dal browser.

### Attività come stato

È necessario importare `useState` in `App.jsx` per poter memorizzare le attività nello stato. Aggiungere quanto segue all'inizio del file `App.jsx`:

```jsx
import { useState } from "react";
```

Si vuole passare `props.tasks` all'hook `useState()`: questo ne conserverà lo stato iniziale. Aggiungere quanto segue proprio all'inizio della definizione della funzione `App()`:

```jsx
const [tasks, setTasks] = useState(props.tasks);
```

Ora è possibile modificare il mapping di `taskList` affinché sia il risultato del mapping di `tasks`, anziché di `props.tasks`. La dichiarazione della costante `taskList` dovrebbe ora essere simile alla seguente:

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

### Aggiungere un'attività

Ora è disponibile un hook `setTasks` utilizzabile nella funzione `addTask()` per aggiornare l'elenco delle attività. C'è però un problema: non è possibile passare semplicemente l'argomento `name` di `addTask()` a `setTasks`, perché `tasks` è un array di oggetti e `name` è una stringa. Se si provasse a farlo, l'array verrebbe sostituito dalla stringa.

Prima di tutto, è necessario inserire `name` in un oggetto che abbia la stessa struttura delle attività esistenti. All'interno della funzione `addTask()`, verrà creato un oggetto `newTask` da aggiungere all'array.

È quindi necessario creare un nuovo array con questa nuova attività aggiunta e poi aggiornare lo stato dei dati delle attività a questo nuovo stato. A tale scopo, è possibile usare la sintassi spread per [copiare l'array esistente](/it/docs/Web/JavaScript/Reference/Operators/Spread_syntax#copying_an_array) e aggiungere l'oggetto alla fine. Questo array viene quindi passato a `setTasks()` per aggiornare lo stato.

Mettendo insieme tutto questo, la funzione `addTask()` dovrebbe essere simile alla seguente:

```jsx
function addTask(name) {
  const newTask = { id: "id", name, completed: false };
  setTasks([...tasks, newTask]);
}
```

Ora è possibile usare il browser per aggiungere un'attività ai dati. Digitare qualsiasi cosa nel modulo e fare clic su "Add" (oppure premere il tasto <kbd>Enter</kbd>) e il nuovo elemento todo apparirà nell'interfaccia utente.

**Tuttavia, c'è un altro problema**: la funzione `addTask()` assegna a ogni attività lo stesso `id`. Questo è negativo per l'accessibilità e rende impossibile per React distinguere le attività future tramite la prop `key`. React mostrerà infatti un avviso nella console DevTools: "Warning: Encountered two children with the same key…"

È necessario correggere questo problema. Creare identificatori univoci è difficile, e la comunità JavaScript ha scritto alcune librerie utili per farlo. Verrà usata [nanoid](https://github.com/ai/nanoid) perché è piccola e funziona bene.

Assicurarsi di essere nella directory principale dell'applicazione ed eseguire il seguente comando del terminale:

```bash
npm install nanoid
```

> [!NOTE]
> Se si usa yarn, è necessario usare invece il seguente comando: `yarn add nanoid`.

Ora è possibile usare `nanoid` per creare ID univoci per le nuove attività. Prima di tutto, importarla includendo la seguente riga all'inizio di `App.jsx`:

```jsx
import { nanoid } from "nanoid";
```

Aggiorniamo ora `addTask()` affinché ogni ID dell'attività diventi il prefisso `todo-` più una stringa univoca generata da nanoid. Aggiornare la dichiarazione della costante `newTask` in questo modo:

```jsx
const newTask = { id: `todo-${nanoid()}`, name, completed: false };
```

Salvare tutto e provare nuovamente l'app: ora è possibile aggiungere attività senza ricevere l'avviso relativo agli ID duplicati.

## Deviazione: contare le attività

Ora che è possibile aggiungere nuove attività, si potrebbe notare un problema: l'intestazione indica "3 tasks remaining" indipendentemente dal numero di attività presenti. È possibile correggerlo contando la lunghezza di `taskList` e modificando di conseguenza il testo dell'intestazione.

Aggiungere quanto segue all'interno della definizione di `App()`, prima dell'istruzione return:

```jsx
const headingText = `${taskList.length} tasks remaining`;
```

Questo è quasi corretto, tranne per il fatto che se l'elenco contiene una sola attività, l'intestazione userà comunque la parola "tasks". Anche questo può diventare una variabile. Aggiornare il codice appena aggiunto come segue:

```jsx
const tasksNoun = taskList.length !== 1 ? "tasks" : "task";
const headingText = `${taskList.length} ${tasksNoun} remaining`;
```

Ora è possibile sostituire il contenuto testuale dell'intestazione dell'elenco con la variabile `headingText`. Aggiornare `<h2>` in questo modo:

```jsx
<h2 id="list-heading">{headingText}</h2>
```

Salvare il file, tornare al browser e provare ad aggiungere alcune attività: il conteggio dovrebbe ora aggiornarsi come previsto.

## Completare un'attività

Si potrebbe notare che, facendo clic su una casella di controllo, questa viene selezionata e deselezionata correttamente. Come funzionalità di HTML, il browser sa come ricordare quali input checkbox sono selezionati o deselezionati senza alcun intervento. Questa funzionalità nasconde però un problema: attivare o disattivare una checkbox non modifica lo stato dell'applicazione React. Ciò significa che il browser e l'app non sono più sincronizzati. È necessario scrivere codice per sincronizzare nuovamente il browser con l'app.

### Dimostrare il bug

Prima di correggere il problema, osserviamolo in azione.

Si inizierà scrivendo una funzione `toggleTaskCompleted()` nel componente `App()`. Questa funzione avrà un parametro `id`, che per ora non verrà usato. Al momento, verrà registrata nella console la prima attività dell'array: verrà esaminato cosa accade quando viene selezionata o deselezionata nel browser.

Aggiungere questo codice appena sopra la dichiarazione della costante `taskList`:

```jsx
function toggleTaskCompleted(id) {
  console.log(tasks[0]);
}
```

Successivamente, aggiungere `toggleTaskCompleted` alle props di ciascun componente `<Todo />` visualizzato all'interno di `taskList`; aggiornarlo come segue:

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

Quindi, passare al componente `Todo.jsx` e aggiungere un gestore `onChange` all'elemento `<input />`, che dovrebbe usare una funzione anonima per chiamare `props.toggleTaskCompleted()` con un parametro `props.id`. L'elemento `<input />` dovrebbe ora essere simile al seguente:

```jsx
<input
  id={props.id}
  type="checkbox"
  defaultChecked={props.completed}
  onChange={() => props.toggleTaskCompleted(props.id)}
/>
```

Salvare tutto, tornare al browser e notare che la prima attività, Eat, è selezionata. Aprire la console JavaScript, quindi fare clic sulla checkbox accanto a Eat. Questa viene deselezionata, come previsto. La console JavaScript, tuttavia, registrerà qualcosa di simile a questo:

```plain
Object { id: "task-0", name: "Eat", completed: true }
```

La checkbox viene deselezionata nel browser, ma la console indica che Eat è ancora completata. Questo verrà corretto nel passaggio successivo.

### Sincronizzare il browser con i dati

Torniamo alla funzione `toggleTaskCompleted()` in `App.jsx`. Si vuole che modifichi la proprietà `completed` soltanto dell'attività attivata o disattivata, lasciando inalterate tutte le altre. Per farlo, verrà usato `map()` sull'elenco delle attività e verrà modificata soltanto quella completata.

Aggiornare la funzione `toggleTaskCompleted()` nel modo seguente:

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

Qui viene definita una costante `updatedTasks` che applica una mappatura sull'array originale `tasks`. Se la proprietà `id` dell'attività corrisponde all'`id` fornito alla funzione, viene usata la [sintassi spread degli oggetti](/it/docs/Web/JavaScript/Reference/Operators/Spread_syntax) per creare un nuovo oggetto e attivare o disattivare la proprietà `completed` di tale oggetto prima di restituirlo. Se non corrisponde, viene restituito l'oggetto originale.

Viene quindi chiamata `setTasks()` con questo nuovo array per aggiornare lo stato.

## Eliminare un'attività

L'eliminazione di un'attività seguirà un modello simile all'attivazione o disattivazione del suo stato di completamento: è necessario definire una funzione per aggiornare lo stato, quindi passare tale funzione a `<Todo />` come prop e chiamarla quando si verifica l'evento appropriato.

### La callback prop `deleteTask`

Qui si inizierà scrivendo una funzione `deleteTask()` nel componente `App`. Come `toggleTaskCompleted()`, questa funzione accetterà un parametro `id` e, inizialmente, verrà registrato tale `id` nella console. Aggiungere quanto segue sotto `toggleTaskCompleted()`:

```jsx
function deleteTask(id) {
  console.log(id);
}
```

Successivamente, aggiungere un'altra callback prop all'array di componenti `<Todo />`:

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

In `Todo.jsx`, si vuole chiamare `props.deleteTask()` quando viene premuto il pulsante "Delete". `deleteTask()` deve conoscere l'ID dell'attività che l'ha chiamata, in modo da poter eliminare l'attività corretta dallo stato.

Aggiornare il pulsante "Delete" all'interno di `Todo.jsx` nel modo seguente:

```jsx
<button
  type="button"
  className="btn btn__danger"
  onClick={() => props.deleteTask(props.id)}>
  Delete <span className="visually-hidden">{props.name}</span>
</button>
```

Ora, facendo clic su uno qualsiasi dei pulsanti "Delete" nell'app, la console del browser dovrebbe registrare l'ID dell'attività associata.

A questo punto, il file `Todo.jsx` dovrebbe essere simile al seguente:

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

Ora che è noto che `deleteTask()` viene invocata correttamente, è possibile chiamare l'hook `setTasks()` in `deleteTask()` per eliminare effettivamente quell'attività dallo stato dell'app, oltre che visivamente dall'interfaccia utente. Poiché `setTasks()` si aspetta un array come argomento, è necessario fornire un nuovo array che copi le attività esistenti, _escludendo_ l'attività il cui ID corrisponde a quello passato a `deleteTask()`.

Questa è un'occasione perfetta per usare [`Array.prototype.filter()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/filter). È possibile verificare ogni attività ed escluderla dal nuovo array se la sua prop `id` corrisponde all'argomento `id` passato a `deleteTask()`.

Aggiornare la funzione `deleteTask()` nel file `App.jsx` come segue:

```jsx
function deleteTask(id) {
  const remainingTasks = tasks.filter((task) => id !== task.id);
  setTasks(remainingTasks);
}
```

Provare di nuovo l'app. Ora dovrebbe essere possibile eliminare un'attività dall'app.

A questo punto, il file `App.jsx` dovrebbe essere simile al seguente:

```jsx
import { useState } from "react";
import { nanoid } from "nanoid";
import Todo from "./components/Todo";
import Form from "./components/Form";
import FilterButton from "./components/FilterButton";

function App(props) {
  const [tasks, setTasks] = useState(props.tasks);

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

## Riepilogo

Questo è sufficiente per un articolo. Qui è stata fornita una panoramica su come React gestisce gli eventi e lo stato, ed è stata implementata la funzionalità per aggiungere attività, eliminare attività e contrassegnare attività come completate. Manca davvero poco. Nel prossimo articolo verrà implementata la funzionalità per modificare le attività esistenti e filtrare l'elenco delle attività tra tutte, completate e incomplete. Verrà inoltre esaminato il rendering condizionale dell'interfaccia utente.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_components","Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering", "Learn_web_development/Core/Frameworks_libraries")}}
