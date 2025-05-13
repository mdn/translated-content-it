---
title: "Interattività in React: Eventi e stato"
short-title: Eventi e stato in React
slug: Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state
l10n:
  sourceCommit: 759102220c07fb140b3e06971cd5981d8f0f134f
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_components","Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering", "Learn_web_development/Core/Frameworks_libraries")}}

Con il nostro piano per i componenti pronto, è ora il momento di iniziare ad aggiornare la nostra app da un'interfaccia utente completamente statica a una che ci permetta effettivamente di interagire e cambiare le cose. In questo articolo lo faremo, approfondendo eventi e stato lungo il percorso, e concludendo con un'app in cui possiamo aggiungere e eliminare compiti con successo, e alternare i compiti come completati.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e con il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/linea di comando</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        Gestire eventi e stato in React e utilizzare questi per iniziare a rendere interattiva l'app del caso di studio.
      </td>
    </tr>
  </tbody>
</table>

## Gestione degli eventi

Se finora hai scritto solo JavaScript normale, potresti essere abituato ad avere un file JavaScript separato in cui si fa una query per alcuni nodi DOM e si allegano listener ad essi. Ad esempio, un file HTML potrebbe avere un pulsante al suo interno, come questo:

```html
<button type="button">Say hi!</button>
```

E un file JavaScript potrebbe avere un codice come questo:

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

In questo esempio, stiamo aggiungendo un attributo `onClick` all'elemento {{htmlelement("button")}}. Il valore di quell'attributo è una funzione che attiva un alert. Questo potrebbe sembrare contrario ai consigli sulle buone pratiche di non scrivere listener di eventi in HTML, ma ricorda: JSX non è HTML.

L'attributo `onClick` ha un significato speciale qui: dice a React di eseguire una determinata funzione quando l'utente fa clic sul pulsante. Ci sono un paio di altre cose da notare:

- La natura {{Glossary("camel_case", "camel-cased")}} di `onClick` è importante — JSX non riconoscerà `onclick` (ancora una volta, è già utilizzato in JavaScript per uno scopo specifico, che è correlato ma diverso — proprietà gestore standard [`onclick`](/it/docs/Web/API/Element/click_event)).
- Tutti gli eventi del browser seguono questo formato in JSX – `on`, seguito dal nome dell'evento.

Applichiamo questo alla nostra app, iniziando nel componente `Form.jsx`.

### Gestire la sottomissione del modulo

All'inizio della funzione componente `Form()` (cioè, subito sotto la linea `function Form() {`), crea una funzione chiamata `handleSubmit()`. Questa funzione dovrebbe [prevenire il comportamento predefinito dell'evento `submit`](/it/docs/Learn_web_development/Core/Scripting/Events#preventing_default_behavior). Dopo di ciò, dovrebbe attivare un `alert()`, che può dire ciò che preferisce. Dovrebbe finire per assomigliare a questo:

```jsx
function handleSubmit(event) {
  event.preventDefault();
  alert("Hello, world!");
}
```

Per usare questa funzione, aggiungi un attributo `onSubmit` all'elemento [`<form>`](/it/docs/Web/HTML/Reference/Elements/form), e imposta il suo valore sulla funzione `handleSubmit`:

```jsx
<form onSubmit={handleSubmit}>
```

Ora, se torna al suo browser e fa clic sul pulsante "Add", il suo browser mostrerà un dialogo di avviso con le parole "Hello, world!" — o qualsiasi cosa abbia scelto di scrivere lì.

## Props di callback

Nelle applicazioni React, l'interattività è raramente confinata a un solo componente: gli eventi che accadono in un componente influenzeranno altre parti dell'app. Quando iniziamo a darci il potere di creare nuovi compiti, le cose che accadono nel componente `<Form />` influenzeranno l'elenco renderizzato in `<App />`.

Vogliamo che la nostra funzione `handleSubmit()` ci aiuti a creare un nuovo compito, quindi abbiamo bisogno di un modo per passare le informazioni da `<Form />` a `<App />`. Non possiamo passare dati dal figlio al genitore nello stesso modo in cui passiamo dati dal genitore al figlio usando props standard. Invece, possiamo scrivere una funzione in `<App />` che si aspetterà alcuni dati dal nostro modulo come input, quindi passare quella funzione a `<Form />` come prop. Questa funzione-come-prop è chiamata **prop di callback**. Una volta ottenuto il nostro prop di callback, possiamo chiamarlo all'interno di `<Form />` per inviare i dati giusti a `<App />`.

### Gestire la sottomissione del modulo tramite callback

All'interno della funzione `App()` in `App.jsx`, crea una funzione chiamata `addTask()` che ha un singolo parametro `name`:

```jsx
function addTask(name) {
  alert(name);
}
```

Successivamente, passa `addTask()` in `<Form />` come prop. Il prop può avere qualsiasi nome desidera, ma scelga un nome che comprenderà più tardi. Qualcosa come `addTask` funziona, perché corrisponde al nome della funzione così come a ciò che la funzione farà. Il suo richiamo del componente `<Form />` dovrebbe essere aggiornato come segue:

```jsx
<Form addTask={addTask} />
```

Per usare questo prop, dobbiamo cambiare la firma della funzione `Form()` in `Form.jsx` in modo che accetti `props` come parametro:

```jsx
function Form(props) {
  // …
}
```

Infine, possiamo usare questo prop all'interno della funzione `handleSubmit()` nel suo componente `<Form />`! Aggiorniamolo come segue:

```jsx
function handleSubmit(event) {
  event.preventDefault();
  props.addTask("Say hello!");
}
```

Fare clic sul pulsante "Add" nel suo browser dimostrerà che la funzione di callback `addTask()` funziona, ma sarebbe bello se potessimo fare in modo che l'allerta ci mostrasse cosa stiamo digitando nel nostro campo di input! Questo è ciò che faremo dopo.

### Una nota a margine: una nota sulle convenzioni di denominazione

Abbiamo passato la funzione `addTask()` nel componente `<Form />` come prop `addTask` in modo che la relazione tra la funzione `addTask()` _e_ il prop `addTask` rimanga il più chiara possibile. Tenga presente, tuttavia, che i nomi dei prop non _devono_ essere particolari. Avremmo potuto passare `addTask()` a `<Form />` con qualsiasi altro nome, come questo:

```diff
- <Form addTask={addTask} />
+ <Form onSubmit={addTask} />
```

Questo renderebbe la funzione `addTask()` disponibile per il componente `<Form />` come il prop `onSubmit`. Quel prop potrebbe essere usato in `Form.jsx` in questo modo:

```diff
function handleSubmit(event) {
  event.preventDefault();
- props.addTask("Say hello!");
+ props.onSubmit("Say hello!");
}
```

Qui, il prefisso `on` ci dice che il prop è una funzione di callback; `Submit` è il nostro indizio che un evento di sottomissione attiverà questa funzione.

Mentre i prop di callback spesso corrispondono ai nomi dei gestori di eventi familiari, come `onSubmit` o `onClick`, possono essere chiamati praticamente in qualsiasi modo che aiuti a rendere chiaro il loro significato. Un ipotetico componente `<Menu />` potrebbe includere una funzione di callback che si esegue quando il menu è aperto, così come una funzione di callback separata che si esegue quando è chiuso:

```jsx
<Menu onOpen={() => console.log("Hi!")} onClose={() => console.log("Bye!")} />
```

Questa convenzione di denominazione `on*` è molto comune nell'ecosistema React, quindi tenga a mente man mano che prosegue nel suo apprendimento. Per il bene della chiarezza, continueremo a usare `addTask` e nomi di prop simili per il resto di questo tutorial. Se ha cambiato nomi di prop mentre leggeva questa sezione, assicuriamosi di riportarli indietro prima di continuare!

## Persistere e cambiare dati con lo stato

Fino ad ora, abbiamo usato i prop per passare dati attraverso i nostri componenti e questo ci è servito bene. Ora che ci occupiamo di interattività, tuttavia, abbiamo bisogno della capacità di creare nuovi dati, di mantenerli e di aggiornarli in seguito. I prop non sono lo strumento giusto per questo lavoro perché sono immutabili — un componente non può cambiare o creare i propri prop.

Questo è dove entra in gioco **lo stato**. Se pensiamo ai prop come a un modo per comunicare tra componenti, possiamo pensare allo stato come a un modo per dare ai componenti "memoria" – informazioni su cui possono contare e aggiornare secondo necessità.

React fornisce una funzione speciale per introdurre stato in un componente, opportunamente chiamata `useState()`.

> **Nota:** `useState()` fa parte di una categoria speciale di funzioni chiamate **hook**, ognuna delle quali può essere utilizzata per aggiungere nuove funzionalità a un componente. Impareremo altri hook in seguito.

Per usare `useState()`, dobbiamo importarlo dal modulo React. Aggiunga la seguente riga in cima al file `Form.jsx`, sopra la definizione della funzione `Form()`:

```jsx
import { useState } from "react";
```

`useState()` prende un singolo argomento che determina il valore iniziale dello stato. Questo argomento può essere una stringa, un numero, un array, un oggetto, o qualsiasi altro tipo di dato JavaScript. `useState()` restituisce un array contenente due elementi. Il primo è il valore corrente dello stato; il secondo è una funzione che può essere usata per aggiornare lo stato.

Creiamo uno stato `name`. Scriva il seguente codice sopra la sua funzione `handleSubmit()`, all'interno di `Form()`:

```jsx
const [name, setName] = useState("Learn React");
```

Diversi eventi stanno accadendo in questa riga di codice:

- Stiamo definendo una costante `name` con il valore `"Learn React"`.
- Stiamo definendo una funzione il cui compito è modificare `name`, chiamata `setName()`.
- `useState()` restituisce queste due cose in un array, quindi stiamo usando la [distrutturazione degli array](/it/docs/Web/JavaScript/Reference/Operators/Destructuring) per catturarle entrambe in variabili separate.

### Lettura dello stato

Può vedere lo stato `name` in azione subito. Aggiunga un attributo `value` all'input del modulo e imposti il suo valore a `name`. Il suo browser renderizzerà "Learn React" dentro l'input.

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

Cambi "Learn React" in una stringa vuota una volta fatto; questo è ciò che vogliamo per il nostro stato iniziale:

```jsx
const [name, setName] = useState("");
```

### Lettura dell'input dell'utente

Prima che possiamo cambiare il valore di `name`, dobbiamo catturare l'input di un utente mentre digita. Per questo, possiamo ascoltare l'evento `onChange`. Scriviamo una funzione `handleChange()`, e ascoltiamo per essa sull'elemento `<input />`.

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

Attualmente, il valore del nostro input non cambierà quando si tenta di inserire del testo, ma il suo browser registrerà la parola "Typing!" nella console JavaScript, quindi sappiamo che il nostro listener dell'evento è agganciato all'input.

Per leggere i tasti dell'utente, dobbiamo accedere alla proprietà `value` dell'input. Possiamo farlo leggendo l'oggetto `event` che `handleChange()` riceve quando è chiamato. `event`, a sua volta, ha [una proprietà `target`](/it/docs/Web/API/Event/target), che rappresenta l'elemento che ha scatenato l'evento `change`. Quello è il nostro input. Quindi, `event.target.value` è il testo all'interno dell'input.

Può `console.log()` questo valore per vederlo nella console del suo browser. Provi a aggiornare la funzione `handleChange()` come segue, e a digitare nell'input per vedere il risultato nella sua console:

```jsx
function handleChange(event) {
  console.log(event.target.value);
}
```

### Aggiornare lo stato

Registrare non basta — vogliamo effettivamente memorizzare ciò che l'utente digita e renderizzarlo nell'input! Cambi la sua chiamata `console.log()` in `setName()`, come mostrato di seguito:

```jsx
function handleChange(event) {
  setName(event.target.value);
}
```

Ora quando digita nell'input, i suoi tasti riempiranno l'input, come potrebbe aspettarsi.

Abbiamo un altro passaggio: dobbiamo cambiare la nostra funzione `handleSubmit()` in modo che chiami `props.addTask` con `name` come argomento. Ricorda il nostro prop di callback? Questo servirà per inviare il compito al componente `App`, in modo che possiamo aggiungerlo alla nostra lista dei compiti in un secondo momento. Per una questione di buona pratica, dovrebbe cancellare l'input dopo che il modulo è stato inviato, quindi chiameremo nuovamente `setName()` con una stringa vuota per farlo:

```jsx
function handleSubmit(event) {
  event.preventDefault();
  props.addTask(name);
  setName("");
}
```

Infine, può digitare qualcosa nel campo di input del suo browser e fare clic su _Add_ — qualunque cosa abbia digitato apparirà in un dialogo di avviso.

Il suo file `Form.jsx` ora dovrebbe leggere così:

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
> Noterà che è in grado di inviare compiti vuoti semplicemente premendo il pulsante `Add` senza inserire un nome del compito. Può pensare a un modo per impedirlo? Come suggerimento, probabilmente ha bisogno di aggiungere un qualche tipo di controllo nella funzione `handleSubmit()`.

## Mettere tutto insieme: Aggiungere un compito

Ora che abbiamo praticato con eventi, prop di callback e hook, siamo pronti per scrivere funzionalità che permetteranno a un utente di aggiungere un nuovo compito dal proprio browser.

### Compiti come stato

Abbiamo bisogno di importare `useState` in `App.jsx` in modo da poter memorizzare i nostri compiti nello stato. Aggiungi il seguente codice in cima al tuo file `App.jsx`:

```jsx
import { useState } from "react";
```

Vogliamo passare `props.tasks` all'interno dell'hook `useState()` – questo conserverà il suo stato iniziale. Aggiungi il seguente codice proprio in cima alla definizione della tua funzione `App()`:

```jsx
const [tasks, setTasks] = useState(props.tasks);
```

Ora, possiamo cambiare il nostro mapping `taskList` in modo che sia il risultato del mapping di `tasks`, invece di `props.tasks`. La tua dichiarazione della costante `taskList` dovrebbe ora apparire così:

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

### Aggiungere un compito

Abbiamo ora un hook `setTasks` che possiamo usare nella nostra funzione `addTask()` per aggiornare la nostra lista dei compiti. C'è un problema tuttavia: non possiamo semplicemente passare l'argomento `name` di `addTask()` a `setTasks`, perché `tasks` è un array di oggetti e `name` è una stringa. Se provassimo a farlo, l'array verrebbe sostituito con la stringa.

Prima di tutto, dobbiamo mettere `name` in un oggetto che abbia la stessa struttura dei nostri compiti esistenti. All'interno della funzione `addTask()`, faremo un oggetto `newTask` da aggiungere all'array.

Poi, dobbiamo fare un nuovo array con questo nuovo compito aggiunto e poi aggiornare lo stato dei dati dei compiti a questo nuovo stato. Per farlo, possiamo usare la sintassi dello spread per [copiare l'array esistente](/it/docs/Web/JavaScript/Reference/Operators/Spread_syntax#copying_an_array), e aggiungere il nostro oggetto alla fine. Poi passiamo questo array in `setTasks()` per aggiornare lo stato.

Mettendo tutto insieme, la tua funzione `addTask()` dovrebbe apparire così:

```jsx
function addTask(name) {
  const newTask = { id: "id", name, completed: false };
  setTasks([...tasks, newTask]);
}
```

Ora puoi utilizzare il browser per aggiungere un compito ai nostri dati! Digita qualsiasi cosa nel modulo e fai clic su "Add" (o premi il tasto <kbd>Enter</kbd>) e vedrai il tuo nuovo elemento todo apparire nell'interfaccia utente!

**Tuttavia, abbiamo un altro problema**: la nostra funzione `addTask()` sta dando a ogni compito lo stesso `id`. Questo è negativo per l'accessibilità e rende impossibile per React distinguere i futuri compiti con la prop `key`. Infatti, React ti darà un avviso nella console degli strumenti di sviluppo — "Warning: Encountered two children with the same key…"

Dobbiamo risolvere questo problema. Creare identificatori unici è un problema difficile – per cui la comunità JavaScript ha scritto alcune utili librerie. Utilizzeremo [nanoid](https://github.com/ai/nanoid) perché è molto piccolo e funziona.

Assicurati di essere nella directory principale della tua applicazione e esegui il seguente comando nel terminale:

```bash
npm install nanoid
```

> [!NOTE]
> Se stai usando yarn, avrai bisogno del seguente comando: `yarn add nanoid`.

Ora possiamo usare `nanoid` per creare ID unici per i nostri nuovi compiti. Per prima cosa, importiamolo includendo la seguente riga in cima a `App.jsx`:

```jsx
import { nanoid } from "nanoid";
```

Ora possiamo aggiornare `addTask()` in modo che ogni ID del compito diventi un prefisso `todo-` più una stringa unica generata da nanoid. Aggiorna la dichiarazione della tua costante `newTask` a questo:

```jsx
const newTask = { id: `todo-${nanoid()}`, name, completed: false };
```

Salva tutto e prova nuovamente la tua app — ora puoi aggiungere compiti senza ricevere quell'avviso sugli ID duplicati.

## Deviazione: conteggio dei compiti

Ora che possiamo aggiungere nuovi compiti, potresti notare un problema: il nostro intestazione dice "3 tasks remaining" indipendentemente da quanti compiti abbiamo! Possiamo risolvere questo problema contando la lunghezza di `taskList` e cambiando di conseguenza il testo del nostro intestazione.

Aggiungi questo all'interno della tua definizione `App()`, prima della dichiarazione di ritorno:

```jsx
const headingText = `${taskList.length} tasks remaining`;
```

Questo è quasi giusto, tranne che se la nostra lista contiene mai un solo compito, l'intestazione continuerà a usare la parola "tasks". Possiamo fare di questo una variabile, anche. Aggiorna il codice che hai appena aggiunto come segue:

```jsx
const tasksNoun = taskList.length !== 1 ? "tasks" : "task";
const headingText = `${taskList.length} ${tasksNoun} remaining`;
```

Ora puoi sostituire il contenuto del testo dell'intestazione della lista con la variabile `headingText`. Aggiorna il tuo `<h2>` come segue:

```jsx
<h2 id="list-heading">{headingText}</h2>
```

Salva il file, torna al tuo browser e prova ad aggiungere qualche compito: il conteggio dovrebbe ora aggiornarsi come previsto.

## Completare un compito

Potrebbe notare che, quando si fa clic su una casella di controllo, essa si spunta e si deseleziona in modo appropriato. Come caratteristica dell'HTML, il browser sa come ricordare quali input di casella di controllo sono selezionati o deselezionati senza il nostro aiuto. Questa caratteristica però nasconde un problema: alternare una casella di controllo non cambia lo stato nella nostra applicazione React. Questo significa che il browser e la nostra app non sono più sincronizzati. Dobbiamo scrivere il nostro proprio codice per rimettere il browser in sincronia con la nostra app.

### Dimostrare il bug

Prima di correggere il problema, osserviamo il suo verificarsi.

Inizieremo scrivendo una funzione `toggleTaskCompleted()` nel nostro componente `App()`. Questa funzione avrà un parametro `id`, ma per ora non lo useremo. Per ora, registreremo nel console il primo compito nell'array – stiamo per ispezionare ciò che accade quando lo selezioniamo o deselezioniamo nel nostro browser:

Aggiungi questo proprio sopra la tua dichiarazione di costante `taskList`:

```jsx
function toggleTaskCompleted(id) {
  console.log(tasks[0]);
}
```

Successivamente, aggiungiamo `toggleTaskCompleted` ai props di ciascun componente `<Todo />` reso all'interno del nostro `taskList`; aggiornalo come segue:

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

Successivamente, vai al tuo componente `Todo.jsx` e aggiungi un gestore `onChange` al tuo elemento `<input />`, che dovrebbe usare una funzione anonima per chiamare `props.toggleTaskCompleted()` con un parametro di `props.id`. Il `<input />` dovrebbe ora sembrare così:

```jsx
<input
  id={props.id}
  type="checkbox"
  defaultChecked={props.completed}
  onChange={() => props.toggleTaskCompleted(props.id)}
/>
```

Salva tutto e torna al tuo browser e nota che il nostro primo compito, Eat, è selezionato. Apri la tua console JavaScript, quindi fai clic sulla casella di controllo accanto a Eat. Si deseleziona, come ci aspettiamo. La tua console JavaScript però registrerà qualcosa di simile a questo:

```plain
Object { id: "task-0", name: "Eat", completed: true }
```

La casella di controllo si deseleziona nel browser, ma la nostra console ci dice che Eat è ancora completato. Correggeremo questo problema successivamente!

### Sincronizzare il browser con i nostri dati

Rivediamo la nostra funzione `toggleTaskCompleted()` in `App.jsx`. Vogliamo che cambi la proprietà `completed` di solo il compito che è stato selezionato o deselezionato, e che lasci tutti gli altri invariati. Per farlo, useremo `map()` sull'elenco dei compiti e cambieremo solo quello che abbiamo completato.

Aggiorna la tua funzione `toggleTaskCompleted()` al seguente:

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

Qui definiamo una costante `updatedTasks` che mappa l'array originale `tasks`. Se la proprietà `id` del compito corrisponde all'`id` fornito alla funzione, utilizziamo [la sintassi dello spread per oggetti](/it/docs/Web/JavaScript/Reference/Operators/Spread_syntax) per creare un nuovo oggetto e alterniamo la proprietà `completed` di quell'oggetto prima di restituirlo. Se non corrisponde, restituiamo l'oggetto originale.

Poi chiamiamo `setTasks()` con questo nuovo array per aggiornare il nostro stato.

## Eliminare un compito

Eliminare un compito seguirà un modello simile all'alternare il suo stato completato: dobbiamo definire una funzione per aggiornare il nostro stato, poi passare quella funzione a `<Todo />` come prop e chiamarla quando l'evento giusto si verifica.

### La prop di callback `deleteTask`

Qui inizieremo scrivendo una funzione `deleteTask()` nel nostro componente `App`. Come `toggleTaskCompleted()`, questa funzione prenderà un parametro `id` e registreremo quel `id` nel console per iniziare. Aggiungi il seguente codice sotto `toggleTaskCompleted()`:

```jsx
function deleteTask(id) {
  console.log(id);
}
```

Successivamente, aggiungi un altro prop di callback al nostro array di componenti `<Todo />`:

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

In `Todo.jsx`, vogliamo chiamare `props.deleteTask()` quando il pulsante "Delete" è premuto. `deleteTask()` deve conoscere l'ID del compito che l'ha chiamato, così può eliminare il compito corretto dallo stato.

Aggiorna il pulsante "Delete" all'interno di `Todo.jsx`, come segue:

```jsx
<button
  type="button"
  className="btn btn__danger"
  onClick={() => props.deleteTask(props.id)}>
  Delete <span className="visually-hidden">{props.name}</span>
</button>
```

Ora quando fai clic su uno qualsiasi dei pulsanti "Delete" nell'app, la tua console del browser dovrebbe registrare l'ID del compito correlato.

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

## Eliminare compiti dallo stato e dall'interfaccia utente

Ora che sappiamo che `deleteTask()` è invocato correttamente, possiamo chiamare il nostro hook `setTasks()` in `deleteTask()` per eliminare effettivamente quel compito dallo stato dell'app così come visivamente nella UI dell'app. Siccome `setTasks()` si aspetta un array come argomento, dovremmo fornire un nuovo array che copia i compiti esistenti, _escludendo_ il compito il cui ID corrisponde a quello passato in `deleteTask()`.

Questa è un'opportunità perfetta per utilizzare [`Array.prototype.filter()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/filter). Possiamo testare ciascun compito ed escludere un compito dal nuovo array se la sua prop `id` corrisponde all'argomento `id` passato in `deleteTask()`.

Aggiorna la funzione `deleteTask()` all'interno del tuo file `App.jsx` come segue:

```jsx
function deleteTask(id) {
  const remainingTasks = tasks.filter((task) => id !== task.id);
  setTasks(remainingTasks);
}
```

Prova di nuovo la tua app. Ora dovresti essere in grado di eliminare un compito dalla tua app!

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

Questo è abbastanza per un articolo. Qui ti abbiamo fornito una panoramica su come React gestisce gli eventi e lo stato, e abbiamo implementato funzionalità per aggiungere compiti, eliminare compiti e alternare compiti come completati. Siamo quasi lì. Nel prossimo articolo implementeremo funzionalità per modificare i compiti esistenti e filtrare l'elenco dei compiti tra tutti, completati e incompleti. Lungo il percorso, esamineremo il rendering condizionale dell'interfaccia utente.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_components","Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering", "Learn_web_development/Core/Frameworks_libraries")}}
