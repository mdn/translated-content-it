---
title: Componentizzazione della nostra app React
short-title: Componenti React
slug: Learn_web_development/Core/Frameworks_libraries/React_components
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning","Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state", "Learn_web_development/Core/Frameworks_libraries")}}

A questo punto, la nostra app è un monolite. Prima di poterla far funzionare, dobbiamo scomporla in componenti gestibili e descrittivi. React non ha regole rigide su cosa sia o non sia un componente - questo dipende da lei! In questo articolo le mostreremo un modo sensato per dividere la nostra app in componenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le lingue base come <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e con il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/linea di comando</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        Un modo sensato di suddividere la nostra app lista delle attività in componenti.
      </td>
    </tr>
  </tbody>
</table>

## Definizione del nostro primo componente

Definire un componente può sembrare complicato finché non si acquisisce un po' di pratica, ma il succo è:

- Se rappresenta una parte ovvia della sua app, probabilmente è un componente
- Se viene riutilizzato spesso, probabilmente è un componente.

Quel secondo punto è particolarmente prezioso: creare un componente da elementi comuni dell'interfaccia utente le permette di cambiare il codice in un unico luogo e vedere quei cambiamenti ovunque venga utilizzato quel componente. Non deve nemmeno estrarre tutto in componenti subito. Prendiamo il secondo punto come ispirazione e facciamo un componente dal pezzo più riutilizzato e importante dell'interfaccia utente: un elemento della lista delle attività.

## Creare un `<Todo />`

Prima di poter creare un componente, dobbiamo creare un nuovo file per esso. In effetti, dovremmo creare una directory apposta per i nostri componenti. Assicuratevi di essere nella radice della vostra app prima di eseguire questi comandi!

```bash
# create a `components` directory
mkdir src/components
# within `components`, create a file called `Todo.jsx`
touch src/components/Todo.jsx
```

Non dimentichiamo di riavviare il server di sviluppo se l'abbiamo fermato per eseguire i comandi precedenti!

Aggiungiamo una funzione `Todo()` in `Todo.jsx`. Qui definiamo una funzione e la esportiamo:

```jsx
function Todo() {}

export default Todo;
```

Finora va bene, ma il nostro componente dovrebbe restituire qualcosa di utile! Torniamo a `src/App.jsx`, copiamo il primo [`<li>`](/it/docs/Web/HTML/Reference/Elements/li) dall'interno della lista non ordinata e incolliamolo in `Todo.jsx` in modo che risulti così:

```jsx
function Todo() {
  return (
    <li className="todo stack-small">
      <div className="c-cb">
        <input id="todo-0" type="checkbox" defaultChecked />
        <label className="todo-label" htmlFor="todo-0">
          Eat
        </label>
      </div>
      <div className="btn-group">
        <button type="button" className="btn">
          Edit <span className="visually-hidden">Eat</span>
        </button>
        <button type="button" className="btn btn__danger">
          Delete <span className="visually-hidden">Eat</span>
        </button>
      </div>
    </li>
  );
}

export default Todo;
```

Ora abbiamo qualcosa da utilizzare. In `App.jsx`, aggiungiamo la seguente linea in cima al file per importare `Todo`:

```jsx
import Todo from "./components/Todo";
```

Con questo componente importato, può sostituire tutti gli elementi `<li>` in `App.jsx` con chiamate al componente `<Todo />`. Il suo `<ul>` dovrebbe apparire così:

```jsx
<ul
  role="list"
  className="todo-list stack-large stack-exception"
  aria-labelledby="list-heading">
  <Todo />
  <Todo />
  <Todo />
</ul>
```

Quando tornerà alla sua app, noterà qualcosa di spiacevole: la sua lista ora ripete il primo compito tre volte!

![La nostra app lista delle attività, con componenti todo ripetuti perché l'etichetta è codificata nel componente](todo-list-repeating-todos.png)

Non vogliamo solo mangiare; abbiamo altre cose da — beh — da fare. Vedremo ora come possiamo far rendere a chiamate di componenti diversi contenuti unici.

## Creare un `<Todo />` unico

I componenti sono potenti perché le permettono di riutilizzare pezzi della sua interfaccia utente e riferirsi a un unico luogo per la fonte di quell'interfaccia utente. Il problema è che non vogliamo tipicamente riutilizzare tutto di ciascun componente; vogliamo riutilizzare la maggior parte delle parti e cambiare piccoli pezzi. Qui entrano in gioco i prop.

### Che cosa c'è in un `nome`?

Per tenere traccia dei nomi dei compiti che vogliamo completare, dovremmo assicurarci che ciascun componente `<Todo />` renda un nome unico.

In `App.jsx`, dia a ciascun `<Todo />` un prop nome. Usiamo i nomi dei nostri compiti che avevamo prima:

```jsx
<Todo name="Eat" />
<Todo name="Sleep" />
<Todo name="Repeat" />
```

Quando il suo browser si aggiorna, vedrà… esattamente la stessa cosa di prima. Abbiamo dato ai nostri `<Todo />` alcuni prop, ma non li stiamo ancora usando. Torniamo a `Todo.jsx` e rimediamo.

Prima, modifichi la definizione della funzione `Todo()` in modo che prenda `props` come parametro. Può usare `console.log()` sui suoi prop se desidera controllare che vengano ricevuti correttamente dal componente.

Una volta che è sicuro che il suo componente stia ricevendo i suoi prop, può sostituire ogni occorrenza di `Eat` con il suo prop `name` leggendo `props.name`. Ricordi: `props.name` è un'espressione JSX, quindi deve avvolgerlo tra parentesi graffe.

Mettendo tutto insieme, la sua funzione `Todo()` dovrebbe apparire così:

```jsx
function Todo(props) {
  return (
    <li className="todo stack-small">
      <div className="c-cb">
        <input id="todo-0" type="checkbox" defaultChecked={true} />
        <label className="todo-label" htmlFor="todo-0">
          {props.name}
        </label>
      </div>
      <div className="btn-group">
        <button type="button" className="btn">
          Edit <span className="visually-hidden">{props.name}</span>
        </button>
        <button type="button" className="btn btn__danger">
          Delete <span className="visually-hidden">{props.name}</span>
        </button>
      </div>
    </li>
  );
}

export default Todo;
```

_Ora_ il suo browser dovrebbe mostrare tre compiti unici. Tuttavia, rimane un altro problema: sono ancora tutti selezionati per impostazione predefinita.

![La nostra lista delle attività, ora con etichette diverse perché passate ai componenti come prop](todo-list-unique-todos.png)

### È `completed`?

Nella nostra lista statica originale, solo `Eat` era selezionato. Ancora una volta vogliamo riutilizzare _la maggior parte_ dell'interfaccia utente che compone un componente `<Todo />`, ma cambiare un aspetto. Questo è un buon compito per un altro prop! Dia alla sua prima chiamata `<Todo />` un prop booleano `completed`, e lasci gli altri due come sono.

```jsx
<Todo name="Eat" completed />
<Todo name="Sleep" />
<Todo name="Repeat" />
```

Come prima, dobbiamo tornare a `Todo.jsx` per utilizzare effettivamente questi prop. Cambi il valore dell'attributo `defaultChecked` sull'elemento `<input />` in modo che sia uguale al prop `completed`. Una volta fatto, l'elemento `<input />` del componente Todo apparirà così:

```jsx
<input id="todo-0" type="checkbox" defaultChecked={props.completed} />
```

E il suo browser dovrebbe aggiornarsi per mostrare solo `Eat` selezionato:

![La nostra app lista delle attività, ora con stati di selezione diversi - alcune caselle sono selezionate, altre no](todo-list-differing-checked-states.png)

Se cambia il prop `completed` di ciascun componente `<Todo />`, il suo browser selezionerà o deselezionerà le caselle di controllo equivalenti renderizzate di conseguenza.

### Mi dia un po' di `id`, per favore

Abbiamo ancora _un altro_ problema: il nostro componente `<Todo />` dà a ogni compito un attributo `id` di `todo-0`. Questo è problematico per un paio di ragioni:

- Gli [attributi `id`](/it/docs/Web/HTML/Reference/Global_attributes/id) devono essere unici (sono usati come identificatori unici per i frammenti di documenti, da CSS, JavaScript, ecc.).
- Quando gli `id` non sono unici, la funzionalità degli elementi [label](/it/docs/Web/HTML/Reference/Elements/label) può rompersi.

Il secondo problema sta influenzando la nostra app in questo momento. Se clicca sulla parola "Sleep" accanto alla seconda casella di controllo, noterà che la casella di controllo "Eat" si alterna invece della casella di controllo "Sleep". Questo perché l'elemento `<label>` di ogni casella di controllo ha un attributo `htmlFor` di `todo-0`. Le `<label>` riconoscono solo il primo elemento con un dato attributo `id`, il che causa il problema che vede quando clicca su altre etichette.

Avevamo attributi `id` unici prima di creare il componente `<Todo />`. Riportiamoli, seguendo il formato `todo-i`, dove `i` aumenta di uno ogni volta. Aggiorni le istanze del componente `Todo` all'interno di `App.jsx` per aggiungere i prop `id`, come segue:

```jsx
<Todo name="Eat" id="todo-0" completed />
<Todo name="Sleep" id="todo-1" />
<Todo name="Repeat" id="todo-2" />
```

> [!NOTE]
> Il prop `completed` è ultimo qui perché è un booleano senza assegnazione. Questo è puramente una convenzione stilistica. L'ordine dei prop non è importante perché i prop sono oggetti JavaScript e gli oggetti JavaScript non sono ordinati.

Ora torni a `Todo.jsx` e faccia buon uso del prop `id`. Deve sostituire il valore dell'attributo `id` dell'elemento `<input />`, così come il valore dell'attributo `htmlFor` della sua `<label>`:

```jsx
<div className="c-cb">
  <input id={props.id} type="checkbox" defaultChecked={props.completed} />
  <label className="todo-label" htmlFor={props.id}>
    {props.name}
  </label>
</div>
```

Con queste correzioni in atto, cliccando sulle etichette accanto a ciascuna casella di controllo si eseguirà ciò che ci si aspetta – selezionare e deselezionare le caselle accanto a quelle etichette.

## Finora, tutto bene?

Stiamo già facendo buon uso di React, ma potremmo fare meglio! Il nostro codice è ripetitivo. Le tre linee che rendono il nostro componente `<Todo />` sono quasi identiche, con una sola differenza: il valore di ciascun prop.

Possiamo ripulire il nostro codice con una delle capacità fondamentali di JavaScript: l'iterazione. Per usare l'iterazione, dovremmo innanzitutto ripensare i nostri compiti.

## Compiti come dati

Ciascuno dei nostri compiti contiene attualmente tre pezzi di informazione: il suo nome, se è stato selezionato, e il suo ID univoco. Questi dati si traducono bene in un oggetto. Poiché abbiamo più di un compito, un array di oggetti funzionerebbe bene nel rappresentare questi dati.

In `src/main.jsx`, dichiari una nuova `const` sotto l'importazione finale, ma sopra `ReactDOM.createRoot()`:

```jsx
const DATA = [
  { id: "todo-0", name: "Eat", completed: true },
  { id: "todo-1", name: "Sleep", completed: false },
  { id: "todo-2", name: "Repeat", completed: false },
];
```

> [!NOTE]
> Se il suo editor di testo ha un plugin [ESLint](https://eslint.org/), potrebbe vedere un avviso su questa `const` DATA. Questo avviso proviene dalla configurazione ESLint fornita dal modello Vite che abbiamo usato, e non si applica a questo codice. Può tranquillamente sopprimere l'avviso aggiungendo `// eslint-disable-next-line` sulla linea sopra la `const` DATA.

Successivamente, passeremo `DATA` a `<App />` come prop, denominato `tasks`. Aggiorni la chiamata del componente `<App />` all'interno di `src/main.jsx` in modo che appaia così:

```jsx
<App tasks={DATA} />
```

L'array `DATA` è ora disponibile all'interno del componente App come `props.tasks`. Può verificarlo con `console.log()`, se desidera.

> **Nota:** I nomi di costanti in `TUTTE_MAIUSCOLE` non hanno significati speciali in JavaScript; sono una convenzione che comunica agli altri sviluppatori "questi dati non cambieranno mai dopo essere stati definiti qui".

## Rendering con iterazione

Per rendere il nostro array di oggetti, dobbiamo trasformare ciascun oggetto in un componente `<Todo />`. JavaScript ci offre un metodo per gli array per trasformare gli oggetti in qualcos'altro: [`Array.prototype.map()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/map).

Dentro `App.jsx`, crei una nuova `const` sopra l'istruzione `return` della funzione `App()`, chiamata `taskList`. Cominciamo trasformando ciascun compito nell'array `props.tasks` nel suo `name`. L'operatore `?.` ci permette di effettuare un [optional chaining](/it/docs/Web/JavaScript/Reference/Operators/Optional_chaining) per verificare se `props.tasks` è `undefined` o `null` prima di tentare di creare un nuovo array di nomi di compiti:

```jsx
const taskList = props.tasks?.map((task) => task.name);
```

Proviamo a sostituire tutti i figli della `<ul>` con `taskList`:

```jsx
<ul
  role="list"
  className="todo-list stack-large stack-exception"
  aria-labelledby="list-heading">
  {taskList}
</ul>
```

Questo ci avvicina alla visualizzazione di tutti i componenti di nuovo, ma abbiamo più lavoro da fare: il browser attualmente rende ogni nome di compito come testo semplice. Ci manca la struttura HTML — il `<li>` e le sue caselle di controllo e pulsanti!

![La nostra app lista delle attività con le etichette degli elementi todo semplicemente mostrate tutte insieme su una linea](todo-list-unstructured-names.png)

Per risolvere questo problema, dobbiamo restituire un componente `<Todo />` dalla nostra funzione `map()` — ricordi che JSX è JavaScript, quindi possiamo usarlo insieme a qualsiasi altra sintassi JavaScript più familiare. Proviamo il seguente codice invece di quello che abbiamo già:

```jsx
const taskList = props.tasks?.map((task) => <Todo />);
```

Osservi di nuovo la sua app; ora i nostri compiti sembrano più simili a prima, ma stanno ancora perdendo i nomi dei compiti stessi. Ricordi che ciascun compito che mappiamo contiene le proprietà `id`, `name`, e `completed` che vogliamo passare al nostro componente `<Todo />`. Se mettiamo insieme quella conoscenza, otteniamo un codice come questo:

```jsx
const taskList = props.tasks?.map((task) => (
  <Todo id={task.id} name={task.name} completed={task.completed} />
));
```

Ora l'app appare come prima, e il nostro codice è meno ripetitivo.

## Chiavi uniche

Ora che React sta rendendo i nostri compiti da un array, deve tenere traccia di quale è quale per renderizzarli correttamente. React cerca di fare i propri calcoli per tenere traccia delle cose, ma possiamo aiutarlo passando un prop `key` ai nostri componenti `<Todo />`. `key` è un prop speciale gestito da React – non può usare la parola `key` per nessun altro scopo.

Poiché le chiavi devono essere uniche, riutilizzeremo l'`id` di ciascun oggetto compito come sua chiave. Aggiorni la sua costante `taskList` in questo modo:

```jsx
const taskList = props.tasks?.map((task) => (
  <Todo
    id={task.id}
    name={task.name}
    completed={task.completed}
    key={task.id}
  />
));
```

**Dovrebbe sempre passare una chiave unica a tutto ciò che rendere con iterazione.** Nulla di ovvio cambierà nel suo browser, ma se non usa chiavi uniche, React registrerà avvisi nella sua console e la sua app potrebbe comportarsi in modo strano!

## Componentizzazione del resto dell'app

Ora che abbiamo risolto il nostro componente più importante, possiamo trasformare il resto della nostra app in componenti. Ricordando che i componenti sono pezzi ovvi o riutilizzati dell'interfaccia utente, o entrambi, possiamo creare altri due componenti:

- `<Form />`
- `<FilterButton />`

Poiché sappiamo che ci servono entrambi, possiamo unire parte del lavoro di creazione dei file in un unico comando terminale. Esegua questo comando nel suo terminale, prendendo precauzionalmente che sia nella directory radice della sua app:

```bash
touch src/components/{Form,FilterButton}.jsx
```

### Il `<Form />`

Apro `components/Form.jsx` e faccia quanto segue:

- Dichiarare una funzione `Form()` ed esportarla alla fine del file.
- Copiare i tag `<form>` e tutto ciò che si trova tra loro da `App.jsx`, e incollarli all'interno dell'istruzione `return` di `Form()`.

Il suo file `Form.jsx` dovrebbe apparire così:

```jsx
function Form() {
  return (
    <form>
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
      />
      <button type="submit" className="btn btn__primary btn__lg">
        Add
      </button>
    </form>
  );
}

export default Form;
```

### Il `<FilterButton />`

Faccia le stesse cose che ha fatto per creare `Form.jsx` all'interno di `FilterButton.jsx`, ma chiami il componente `FilterButton()` e copi l'HTML per il primo pulsante all'interno di `<div className="filters btn-group stack-exception">` da `App.jsx` nell'istruzione `return`.

Il file dovrebbe apparire così:

```jsx
function FilterButton() {
  return (
    <button type="button" className="btn toggle-btn" aria-pressed="true">
      <span className="visually-hidden">Show </span>
      <span>all </span>
      <span className="visually-hidden"> tasks</span>
    </button>
  );
}

export default FilterButton;
```

> [!NOTE]
> Potrebbe notare che stiamo commettendo lo stesso errore qui come abbiamo fatto inizialmente per il componente `<Todo />`, in quanto ogni pulsante sarà identico. Va bene! Correggeremo questo componente in seguito, nella sezione [Tornare ai pulsanti del filtro](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering#back_to_the_filter_buttons).

## Importazione di tutti i nostri componenti

Facciamo uso dei nostri nuovi componenti. Aggiunga alcune istruzioni di `import` in cima a `App.jsx` e faccia riferimento ai componenti che abbiamo appena creato. Poi, aggiorni l'istruzione `return` di `App()` in modo da renderizzare i nostri componenti.

Quando avrà finito, `App.jsx` leggerà così:

```jsx
import Form from "./components/Form";
import FilterButton from "./components/FilterButton";
import Todo from "./components/Todo";

function App(props) {
  const taskList = props.tasks?.map((task) => (
    <Todo
      id={task.id}
      name={task.name}
      completed={task.completed}
      key={task.id}
    />
  ));
  return (
    <div className="todoapp stack-large">
      <h1>TodoMatic</h1>
      <Form />
      <div className="filters btn-group stack-exception">
        <FilterButton />
        <FilterButton />
        <FilterButton />
      </div>
      <h2 id="list-heading">3 tasks remaining</h2>
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

Con questo in atto, la sua app React dovrebbe renderizzare sostanzialmente come faceva prima, ma utilizzando i suoi nuovi e splendenti componenti.

## Sommario

E questo è tutto per questo articolo — abbiamo approfondito come suddividere la sua app in componenti e renderizzarli in modo efficiente. Successivamente, esamineremo la gestione degli eventi in React e inizieremo ad aggiungere un po' di interattività.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning","Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state", "Learn_web_development/Core/Frameworks_libraries")}}
