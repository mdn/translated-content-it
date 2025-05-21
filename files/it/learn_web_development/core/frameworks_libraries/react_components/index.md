---
title: Componentizzare la nostra app React
short-title: Componenti di React
slug: Learn_web_development/Core/Frameworks_libraries/React_components
l10n:
  sourceCommit: 611edf6335e4a833a6f394d0d98b117e7b0a36bf
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning","Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state", "Learn_web_development/Core/Frameworks_libraries")}}

Arrivati a questo punto, la nostra app è un monolite. Prima di poterla far funzionare, dobbiamo suddividerla in componenti gestibili e descrittivi. React non ha regole rigide su cosa sia o non sia un componente – sta a te decidere! In questo articolo ti mostreremo un modo ragionevole per suddividere la nostra app in componenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le lingue principali come <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/command line</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        Un modo ragionevole per suddividere la nostra app di lista di cose da fare in componenti.
      </td>
    </tr>
  </tbody>
</table>

## Definire il nostro primo componente

Definire un componente può sembrare complicato finché non si fa un po' di pratica, ma il succo è:

- Se rappresenta un "pezzo" ovvio della tua app, probabilmente è un componente
- Se viene riutilizzato spesso, probabilmente è un componente.

Quel secondo punto è particolarmente utile: creare un componente da elementi UI comuni ti permette di cambiare il tuo codice in un solo posto e vedere quei cambiamenti ovunque venga usato quel componente. Non devi suddividere tutto subito in componenti, però. Prendiamo come ispirazione il secondo punto e creiamo un componente dal pezzo più riutilizzato e più importante dell'interfaccia: un elemento della lista di cose da fare.

## Crea un `<Todo />`

Prima di poter creare un componente, dovremmo creare un nuovo file per esso. Infatti, dovremmo creare una directory apposita per i nostri componenti. Assicurati di essere nella radice della tua app prima di eseguire questi comandi!

```bash
# create a `components` directory
mkdir src/components
# within `components`, create a file called `Todo.jsx`
touch src/components/Todo.jsx
```

Non dimenticare di riavviare il tuo server di sviluppo se lo hai fermato per eseguire i comandi precedenti!

Aggiungiamo una funzione `Todo()` in `Todo.jsx`. Qui definiamo una funzione e la esportiamo:

```jsx
function Todo() {}

export default Todo;
```

Finora va bene, ma il nostro componente dovrebbe restituire qualcosa di utile! Torna a `src/App.jsx`, copia il primo [`<li>`](/it/docs/Web/HTML/Reference/Elements/li) dall'interno della lista non ordinata e incollalo in `Todo.jsx` in modo che appaia così:

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

Ora abbiamo qualcosa che possiamo usare. In `App.jsx`, aggiungi la seguente riga in cima al file per importare `Todo`:

```jsx
import Todo from "./components/Todo";
```

Con questo componente importato, puoi sostituire tutti gli elementi `<li>` in `App.jsx` con chiamate al componente `<Todo />`. Il tuo `<ul>` dovrebbe apparire così:

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

Quando torni alla tua app, noterai qualcosa di spiacevole: la lista ora ripete il primo compito tre volte!

![La nostra app di lista di cose da fare, con componenti todo che si ripetono perché l'etichetta è codificata nel componente](todo-list-repeating-todos.png)

Non vogliamo solo mangiare; abbiamo anche altre cose da – beh – fare. Vedremo quindi come possiamo fare in modo che chiamate diverse ai componenti generino contenuti unici.

## Crea un `<Todo />` unico

I componenti sono potenti perché ci permettono di riutilizzare pezzi della nostra UI e di fare riferimento a un unico posto per la fonte di quella UI. Il problema è che tipicamente non vogliamo riutilizzare tutto di ogni componente; vogliamo riutilizzare la maggior parte delle parti e cambiare piccoli pezzi. È qui che entrano in gioco i props.

### Cosa c'è in un `name`?

Per tracciare i nomi dei compiti che vogliamo completare, dovremmo assicurarci che ogni componente `<Todo />` visualizzi un nome unico.

In `App.jsx`, dai a ogni `<Todo />` un prop di nome. Usiamo i nomi dei nostri compiti che avevamo prima:

```jsx
<ul
  role="list"
  className="todo-list stack-large stack-exception"
  aria-labelledby="list-heading">
  <Todo name="Eat" />
  <Todo name="Sleep" />
  <Todo name="Repeat" />
</ul>
```

Quando il tuo browser si aggiorna, vedrai… esattamente la stessa cosa di prima. Abbiamo dato al nostro `<Todo />` alcuni props, ma al momento non li stiamo usando. Torniamo a `Todo.jsx` e risolviamo questo.

Prima modifica la definizione della tua funzione `Todo()` in modo che accetti `props` come parametro. Puoi `console.log()` i tuoi props se desideri verificare che siano ricevuti correttamente dal componente.

Una volta che sei sicuro che il tuo componente stia ricevendo i suoi props, puoi sostituire ogni occorrenza di `Eat` con il tuo prop `name` leggendo `props.name`. Ricorda: `props.name` è un'espressione JSX, quindi dovrai racchiuderla tra parentesi graffe.

Mettendo tutto insieme, la tua funzione `Todo()` dovrebbe apparire così:

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

_Ora_ il tuo browser dovrebbe mostrare tre compiti unici. Rimane comunque un altro problema: sono tutti ancora selezionati di default.

![La nostra lista di cose da fare, con etichette todo differenti ora che sono passate nei componenti come props](todo-list-unique-todos.png)

### È `completed`?

Nella nostra lista statica originale, solo `Eat` era selezionato. Ancora una volta, vogliamo riutilizzare _la maggior parte_ dell'interfaccia che compone un componente `<Todo />`, ma cambiare una cosa. È un buon lavoro per un altro prop! Dai alla tua prima chiamata a `<Todo />` un prop booleano di `completed`, e lascia le altre due com'erano.

```jsx
<ul
  role="list"
  className="todo-list stack-large stack-exception"
  aria-labelledby="list-heading">
  <Todo name="Eat" completed />
  <Todo name="Sleep" />
  <Todo name="Repeat" />
</ul>
```

Come prima, dobbiamo tornare a `Todo.jsx` per utilizzare effettivamente questi props. Cambia l'attributo `defaultChecked` sull'elemento `<input />` in modo che il suo valore sia uguale al prop `completed`. Una volta finito, l'elemento `<input />` del componente Todo apparirà così:

```jsx
<input id="todo-0" type="checkbox" defaultChecked={props.completed} />
```

E il tuo browser dovrebbe aggiornarsi per mostrare solo `Eat` come selezionato:

![La nostra app di lista di cose da fare, ora con stati di selezione differenti - alcune caselle di controllo sono selezionate, altre no](todo-list-differing-checked-states.png)

Se cambi il prop `completed` di ogni componente `<Todo />`, il tuo browser selezionerà o deselezionerà di conseguenza le caselle di controllo equivalenti visualizzate.

### Dammi un po' di `id`, per favore

Abbiamo ancora un _altro_ problema: il nostro componente `<Todo />` assegna a ogni compito un attributo `id` di `todo-0`. Questo è problematico per un paio di motivi:

- Gli [attributi `id`](/it/docs/Web/HTML/Reference/Global_attributes/id) devono essere unici (vengono usati come identificatori unici per frammenti di documenti, da CSS, JavaScript, ecc.).
- Quando gli `id` non sono unici, la funzionalità degli elementi [label](/it/docs/Web/HTML/Reference/Elements/label) può rompersi.

Il secondo problema sta influenzando la nostra app in questo momento. Se fai clic sulla parola "Sleep" accanto alla seconda casella di controllo, noterai che la casella di controllo "Eat" si attiva invece della casella di controllo "Sleep". Questo perché l'elemento `<label>` di ogni casella di controllo ha un attributo `htmlFor` di `todo-0`. Le `<label>` riconoscono solo il primo elemento con un dato attributo `id`, causando il problema che vedi quando fai clic su altre etichette.

Avevamo attributi `id` unici prima di creare il componente `<Todo />`. Ripristiniamoli, seguendo il formato `todo-i`, dove `i` aumenta di uno ogni volta. Aggiorna le istanze del componente `Todo` all'interno di `App.jsx` per aggiungere i props `id`, come segue:

```jsx
<ul
  role="list"
  className="todo-list stack-large stack-exception"
  aria-labelledby="list-heading">
  <Todo name="Eat" id="todo-0" completed />
  <Todo name="Sleep" id="todo-1" />
  <Todo name="Repeat" id="todo-2" />
</ul>
```

> [!NOTE]
> Il prop `completed` è l'ultimo qui perché è un booleano senza assegnazione. Questa è puramente una convenzione stilistica. L'ordine dei props non importa perché i props sono oggetti JavaScript e gli oggetti JavaScript non sono ordinati.

Ora torna a `Todo.jsx` e utilizza il prop `id`. Deve sostituire il valore dell'attributo `id` dell'elemento `<input />`, così come il valore dell'attributo `htmlFor` della sua `<label>`:

```jsx
<div className="c-cb">
  <input id={props.id} type="checkbox" defaultChecked={props.completed} />
  <label className="todo-label" htmlFor={props.id}>
    {props.name}
  </label>
</div>
```

Con queste correzioni in atto, fare clic sulle etichette accanto a ciascuna casella di controllo farà quello che ci aspettiamo – selezionare e deselezionare le caselle di controllo accanto a quelle etichette.

## Finora, tutto bene?

Stiamo facendo un buon uso di React finora, ma possiamo fare di meglio! Il nostro codice è ripetitivo. Le tre righe che rendono il nostro componente `<Todo />` sono quasi identiche, con una sola differenza: il valore di ciascun prop.

Possiamo ripulire il nostro codice con una delle abilità fondamentali di JavaScript: l'iterazione. Per usare l'iterazione, dovremmo prima ripensare ai nostri compiti.

## Compiti come dati

Ciascuno dei nostri compiti attualmente contiene tre pezzi di informazioni: il suo nome, se è stato selezionato, e il suo ID univoco. Questi dati si traducono bene in un oggetto. Poiché abbiamo più di un compito, un array di oggetti funzionerebbe bene nel rappresentare questi dati.

In `src/main.jsx`, dichiara un nuovo `const` sotto l'import finale, ma sopra `ReactDOM.createRoot()`:

```jsx
const DATA = [
  { id: "todo-0", name: "Eat", completed: true },
  { id: "todo-1", name: "Sleep", completed: false },
  { id: "todo-2", name: "Repeat", completed: false },
];
```

> [!NOTE]
> Se il tuo editor di testo ha un plugin [ESLint](https://eslint.org/), potresti vedere un avviso su questa `DATA` const. Questo avviso proviene dalla configurazione ESLint fornita dal modello Vite che abbiamo usato, e non si applica a questo codice. Puoi tranquillamente sopprimere l'avviso aggiungendo `// eslint-disable-next-line` alla riga sopra la const `DATA`.

Successivamente, passeremo `DATA` a `<App />` come un prop, chiamato `tasks`. Aggiorna il tuo componente `<App />` all'interno di `src/main.jsx` per leggerlo così:

```jsx
<App tasks={DATA} />
```

L'array `DATA` è ora disponibile all'interno del componente App come `props.tasks`. Puoi `console.log()` per controllare, se desideri.

> **Nota:** I nomi costanti in `ALL_CAPS` non hanno un significato speciale in JavaScript; sono una convenzione che dice ad altri sviluppatori "questi dati non cambieranno mai dopo essere stati definiti qui".

## Rendering con l'iterazione

Per eseguire il rendering del nostro array di oggetti, dobbiamo trasformare ogni oggetto in un componente `<Todo />`. JavaScript ci offre un metodo array per trasformare gli elementi in qualcos'altro: [`Array.prototype.map()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/map).

All'interno di `App.jsx`, crea un nuovo `const` sopra l'istruzione `return` della funzione `App()`, chiamato `taskList`. Iniziamo trasformando ogni compito nell'array `props.tasks` nel suo `name`. L'operatore `?.` ci permette di eseguire [chaining opzionale](/it/docs/Web/JavaScript/Reference/Operators/Optional_chaining) per verificare se `props.tasks` è `undefined` o `null` prima di tentare di creare un nuovo array di nomi di compiti:

```jsx
const taskList = props.tasks?.map((task) => task.name);
```

Proviamo a sostituire tutti i figli del `<ul>` con `taskList`:

```jsx
<ul
  role="list"
  className="todo-list stack-large stack-exception"
  aria-labelledby="list-heading">
  {taskList}
</ul>
```

Questo ci porta in qualche modo verso il mostrare tutti i componenti di nuovo, ma abbiamo ancora del lavoro da fare: il browser attualmente renderizza il nome di ogni compito come testo semplice. Ci manca la nostra struttura HTML — il `<li>` e le sue caselle di controllo e pulsanti!

![La nostra app di lista di cose da fare con le etichette degli elementi todo mostrate solo raggruppate su una linea](todo-list-unstructured-names.png)

Per risolvere questo, dobbiamo restituire un componente `<Todo />` dalla nostra funzione `map()` — ricorda che JSX è JavaScript, quindi possiamo usarlo insieme a qualsiasi altra sintassi JavaScript più familiare. Proviamo il seguente invece di quello che abbiamo già:

```jsx
const taskList = props.tasks?.map((task) => <Todo />);
```

Guarda di nuovo la tua app; ora i nostri compiti sembrano più simili a com'èrano prima, ma manca il nome dei compiti stessi. Ricorda che ogni compito su cui facciamo il mapping contiene le proprietà `id`, `name` e `completed` che vogliamo passare nel nostro componente `<Todo />`. Mettendo insieme queste conoscenze, otteniamo un codice come questo:

```jsx
const taskList = props.tasks?.map((task) => (
  <Todo id={task.id} name={task.name} completed={task.completed} />
));
```

Ora l'app appare come prima, e il nostro codice è meno ripetitivo.

## Chiavi uniche

Ora che React sta eseguendo il rendering dei nostri compiti da un array, deve tenere traccia di quale è quale per renderizzarli correttamente. React cerca di fare le proprie ipotesi per tenere traccia delle cose, ma possiamo aiutarlo passando un prop `key` ai nostri componenti `<Todo />`. `key` è un prop speciale gestito da React – non puoi usare la parola `key` per nessun altro scopo.

Poiché le chiavi devono essere uniche, riutilizzeremo l'`id` di ciascun oggetto compito come sua chiave. Aggiorna la tua costante `taskList` in questo modo:

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

**Dovresti sempre passare una chiave unica a qualsiasi cosa fai il rendering con l'iterazione.** Nulla di evidente cambierà nel tuo browser, ma se non utilizzi chiavi uniche, React registrerà avvisi nella tua console e la tua app potrebbe comportarsi in modo strano!

## Componentizzare il resto dell'app

Ora che abbiamo risolto il nostro componente più importante, possiamo trasformare il resto della nostra app in componenti. Ricordando che i componenti sono o pezzi ovvi della UI, pezzi di UI riutilizzati, o entrambi, possiamo creare altri due componenti:

- `<Form />`
- `<FilterButton />`

Poiché sappiamo di aver bisogno di entrambi, possiamo raggruppare parte del lavoro di creazione dei file insieme in un unico comando terminale. Esegui questo comando nel tuo terminale, assicurandoti di trovarti nella directory principale della tua app:

```bash
touch src/components/{Form,FilterButton}.jsx
```

### Il `<Form />`

Apri `components/Form.jsx` e fai quanto segue:

- Dichiara una funzione `Form()` ed esportala alla fine del file.
- Copia i tag `<form>` e tutto ciò che c'è tra di loro da `App.jsx`, e incollali all'interno dell'istruzione `return` di `Form()`.

Il tuo file `Form.jsx` dovrebbe apparire così:

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

Fai le stesse cose che hai fatto per creare `Form.jsx` all'interno di `FilterButton.jsx`, ma chiama il componente `FilterButton()` e copia l'HTML per il primo pulsante all'interno di `<div className="filters btn-group stack-exception">` da `App.jsx` nell'istruzione `return`.

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
> Potresti notare che stiamo commettendo lo stesso errore qui che abbiamo fatto all'inizio per il componente `<Todo />`, nel senso che ogni pulsante sarà lo stesso. Va bene! Andremo a sistemare questo componente più avanti, in [Back to the filter buttons](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering#back_to_the_filter_buttons).

## Importare tutti i nostri componenti

Facciamo uso dei nostri nuovi componenti. Aggiungi alcune istruzioni `import` in più nella parte superiore di `App.jsx` e fai riferimento ai componenti che abbiamo appena creato. Poi, aggiorna l'istruzione `return` di `App()` in modo che visualizzi i nostri componenti.

Quando hai finito, `App.jsx` apparirà così:

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

Con questo, la tua app React dovrebbe eseguire il rendering praticamente nello stesso modo di prima, ma utilizzando i tuoi componenti nuovi di zecca.

## Riepilogo

E questo è quanto per questo articolo — abbiamo approfondito come suddividere la tua app in componenti in modo adeguato e renderizzarli in modo efficiente. Vedremo in seguito come gestire gli eventi in React e iniziare ad aggiungere un po' di interattività.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning","Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state", "Learn_web_development/Core/Frameworks_libraries")}}
