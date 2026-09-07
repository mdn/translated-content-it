---
title: Suddividere in componenti la nostra app React
short-title: Componenti React
slug: Learn_web_development/Core/Frameworks_libraries/React_components
l10n:
  sourceCommit: a84b606ffd77c40a7306be6c932a74ab9ce6ab96
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning","Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state", "Learn_web_development/Core/Frameworks_libraries")}}

A questo punto, la nostra app è un monolite. Prima di poterle far fare qualcosa, è necessario suddividerla in componenti gestibili e descrittivi. React non ha regole rigide su cosa sia o non sia un componente: la scelta spetta allo sviluppatore. In questo articolo verrà mostrato un modo ragionevole per suddividere la nostra app in componenti.

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
        Un modo ragionevole per suddividere la nostra app di elenco di attività in componenti.
      </td>
    </tr>
  </tbody>
</table>

## Definire il primo componente

Definire un componente può sembrare difficile finché non si acquisisce un po' di pratica, ma l'idea di base è:

- Se rappresenta una "parte" evidente dell'app, probabilmente è un componente.
- Se viene riutilizzato spesso, probabilmente è un componente.

Il secondo punto è particolarmente utile: creare un componente a partire da elementi UI comuni consente di modificare il codice in un solo punto e vedere tali modifiche ovunque venga usato quel componente. Non è nemmeno necessario suddividere subito ogni cosa in componenti. Prendiamo il secondo punto come ispirazione e creiamo un componente dalla parte più riutilizzata e più importante della UI: un elemento dell'elenco di attività.

## Creare un `<Todo />`

Prima di poter creare un componente, è opportuno creare un nuovo file per esso. In effetti, è opportuno creare una directory dedicata ai componenti. Assicurarsi di trovarsi nella directory radice dell'app prima di eseguire questi comandi.

```bash
# create a `components` directory
mkdir src/components
# within `components`, create a file called `Todo.jsx`
touch src/components/Todo.jsx
```

Non dimenticare di riavviare il server di sviluppo se è stato arrestato per eseguire i comandi precedenti.

Aggiungiamo una funzione `Todo()` in `Todo.jsx`. Qui definiamo una funzione e la esportiamo:

```jsx
function Todo() {}

export default Todo;
```

Fin qui va bene, ma il componente dovrebbe restituire qualcosa di utile. Tornare a `src/App.jsx`, copiare il primo [`<li>`](/it/docs/Web/HTML/Reference/Elements/li) dall'interno dell'elenco non ordinato e incollarlo in `Todo.jsx`, in modo che risulti così:

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

Ora abbiamo qualcosa che possiamo usare. In `App.jsx`, aggiungere la seguente riga all'inizio del file per importare `Todo`:

```jsx
import Todo from "./components/Todo";
```

Dopo aver importato questo componente, è possibile sostituire tutti gli elementi `<li>` in `App.jsx` con chiamate del componente `<Todo />`. Il `<ul>` dovrebbe risultare così:

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

Tornando all'app, si noterà qualcosa di spiacevole: l'elenco ora ripete tre volte la prima attività.

![La nostra app di elenco di attività, con componenti todo ripetuti perché l'etichetta è codificata nel componente](todo-list-repeating-todos.png)

Non si vuole soltanto mangiare; ci sono anche altre cose da — beh — fare. Successivamente vedremo come fare in modo che chiamate differenti di componenti eseguano il rendering di contenuti unici.

## Creare un `<Todo />` univoco

I componenti sono potenti perché consentono di riutilizzare parti della UI e di fare riferimento a un unico punto come sorgente di quella UI. Il problema è che in genere non si desidera riutilizzare tutto di ogni componente; si vogliono riutilizzare la maggior parte delle parti e modificare piccole porzioni. Qui entrano in gioco le props.

### Cosa c'è in un `name`?

Per tenere traccia dei nomi delle attività che si vogliono completare, occorre assicurarsi che ogni componente `<Todo />` esegua il rendering di un nome univoco.

In `App.jsx`, assegnare una prop `name` a ogni `<Todo />`. Usiamo i nomi delle attività che avevamo in precedenza:

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

Quando il browser si aggiorna, verrà visualizzato… esattamente lo stesso risultato di prima. Sono state assegnate alcune props a `<Todo />`, ma non vengono ancora utilizzate. Torniamo a `Todo.jsx` e risolviamo il problema.

Per prima cosa, modificare la definizione della funzione `Todo()` affinché accetti `props` come parametro. È possibile usare `console.log()` sulle props per verificare che vengano ricevute correttamente dal componente.

Una volta accertato che il componente riceve le sue props, è possibile sostituire ogni occorrenza di `Eat` con la prop `name` leggendo `props.name`. Ricordare: `props.name` è un'espressione JSX, quindi deve essere racchiusa tra parentesi graffe.

Mettendo insieme tutto, la funzione `Todo()` dovrebbe risultare così:

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

_Ora_ il browser dovrebbe mostrare tre attività univoche. Rimane però un altro problema: sono ancora tutte selezionate per impostazione predefinita.

![Il nostro elenco di attività, con etichette todo differenti ora che vengono passate ai componenti come props](todo-list-unique-todos.png)

### È `completed`?

Nell'elenco statico originale, soltanto `Eat` era selezionato. Ancora una volta, si vuole riutilizzare la _maggior parte_ della UI che compone un componente `<Todo />`, ma modificare una cosa. Questo è un buon compito per un'altra prop. Assegnare alla prima chiamata di `<Todo />` una prop booleana `completed` e lasciare inalterate le altre due.

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

Come prima, occorre tornare a `Todo.jsx` per utilizzare effettivamente queste props. Modificare l'attributo `defaultChecked` su `<input />` affinché il suo valore sia uguale alla prop `completed`. Al termine, l'elemento `<input />` del componente Todo sarà così:

```jsx
<input id="todo-0" type="checkbox" defaultChecked={props.completed} />
```

Il browser dovrebbe aggiornarsi per mostrare soltanto `Eat` selezionato:

![La nostra app di elenco di attività, ora con stati selezionati differenti: alcune caselle di controllo sono selezionate, altre no](todo-list-differing-checked-states.png)

Se viene modificata la prop `completed` di ciascun componente `<Todo />`, il browser selezionerà o deselezionerà di conseguenza le caselle di controllo equivalenti di cui viene eseguito il rendering.

### Dammi un po' di `id`, per favore

C'è ancora _un altro_ problema: il componente `<Todo />` assegna a ogni attività un attributo `id` pari a `todo-0`. Questo è problematico per un paio di ragioni:

- Gli [attributi `id`](/it/docs/Web/HTML/Reference/Global_attributes/id) devono essere univoci, poiché vengono usati come identificatori univoci per frammenti di documento, da CSS, JavaScript e così via.
- Quando gli `id` non sono univoci, la funzionalità degli [elementi label](/it/docs/Web/HTML/Reference/Elements/label) può interrompersi.

Il secondo problema sta influenzando l'app proprio ora. Facendo clic sulla parola "Sleep" accanto alla seconda casella di controllo, si noterà che viene attivata/disattivata la casella "Eat" anziché quella "Sleep". Questo accade perché l'elemento `<label>` di ogni casella di controllo ha un attributo `htmlFor` pari a `todo-0`. I `<label>` riconoscono soltanto il primo elemento con un dato attributo `id`, causando il problema visibile quando si fa clic sulle altre etichette.

Gli attributi `id` erano univoci prima di creare il componente `<Todo />`. Ripristiniamoli, seguendo il formato `todo-i`, dove `i` aumenta di uno ogni volta. Aggiornare le istanze del componente `Todo` in `App.jsx` aggiungendo le props `id`, come segue:

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
> La prop `completed` è l'ultima perché è un valore booleano senza assegnazione. Si tratta puramente di una convenzione stilistica. L'ordine delle props non è importante perché le props sono oggetti JavaScript e gli oggetti JavaScript non sono ordinati.

Ora tornare a `Todo.jsx` e utilizzare la prop `id`. Deve sostituire il valore dell'attributo `id` dell'elemento `<input />`, oltre al valore dell'attributo `htmlFor` del suo `<label>`:

```jsx
<div className="c-cb">
  <input id={props.id} type="checkbox" defaultChecked={props.completed} />
  <label className="todo-label" htmlFor={props.id}>
    {props.name}
  </label>
</div>
```

Con queste correzioni, facendo clic sulle etichette accanto a ciascuna casella di controllo si otterrà il comportamento previsto: selezionare e deselezionare le caselle di controllo accanto a tali etichette.

## Fin qui, tutto bene?

Finora stiamo facendo buon uso di React, ma possiamo fare di meglio. Il codice è ripetitivo. Le tre righe che eseguono il rendering del componente `<Todo />` sono quasi identiche, con una sola differenza: il valore di ciascuna prop.

Possiamo rendere il codice più pulito con una delle capacità fondamentali di JavaScript: l'iterazione. Per usare l'iterazione, occorre prima ripensare le attività.

## Attività come dati

Ciascuna attività contiene attualmente tre informazioni: il suo nome, se è stata selezionata e il suo ID univoco. Questi dati si traducono bene in un oggetto. Poiché esiste più di un'attività, un array di oggetti funzionerebbe bene per rappresentare questi dati.

In `src/main.jsx`, dichiarare un nuovo `const` sotto l'ultimo import, ma sopra `ReactDOM.createRoot()`:

```jsx
const DATA = [
  { id: "todo-0", name: "Eat", completed: true },
  { id: "todo-1", name: "Sleep", completed: false },
  { id: "todo-2", name: "Repeat", completed: false },
];
```

> [!NOTE]
> Se l'editor di testo dispone di un plugin [ESLint](https://eslint.org/), potrebbe essere visualizzato un avviso su questa costante `DATA`. Questo avviso deriva dalla configurazione ESLint fornita dal template Vite utilizzato e non si applica a questo codice. È possibile sopprimere l'avviso in sicurezza aggiungendo `// eslint-disable-next-line` alla riga sopra la costante `DATA`.

Successivamente passeremo `DATA` a `<App />` come prop, chiamata `tasks`. Aggiornare la chiamata del componente `<App />` dentro `src/main.jsx` in modo che risulti così:

```jsx
<App tasks={DATA} />
```

L'array `DATA` è ora disponibile all'interno del componente App come `props.tasks`. Se si desidera, è possibile usare `console.log()` per verificarlo.

> [!NOTE]
> I nomi delle costanti in `ALL_CAPS` non hanno alcun significato speciale in JavaScript; sono una convenzione che indica agli altri sviluppatori: "questi dati non cambieranno mai dopo essere stati definiti qui".

## Rendering con l'iterazione

Per eseguire il rendering dell'array di oggetti, occorre trasformare ogni oggetto in un componente `<Todo />`. JavaScript fornisce un metodo degli array per trasformare elementi in qualcos'altro: [`Array.prototype.map()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/map).

All'interno di `App.jsx`, creare una nuova `const` sopra l'istruzione `return` della funzione `App()` chiamata `taskList`. Iniziamo trasformando ogni attività nell'array `props.tasks` nel suo `name`. L'operatore `?.` consente di eseguire il [concatenamento opzionale](/it/docs/Web/JavaScript/Reference/Operators/Optional_chaining) per verificare se `props.tasks` è `undefined` o `null` prima di tentare di creare un nuovo array di nomi delle attività:

```jsx
const taskList = props.tasks?.map((task) => task.name);
```

Proviamo a sostituire tutti i figli di `<ul>` con `taskList`:

```jsx
<ul
  role="list"
  className="todo-list stack-large stack-exception"
  aria-labelledby="list-heading">
  {taskList}
</ul>
```

Questo ci avvicina alla visualizzazione di tutti i componenti, ma c'è ancora del lavoro da fare: il browser attualmente esegue il rendering del nome di ogni attività come testo semplice. Manca la struttura HTML, ovvero il `<li>` e le relative caselle di controllo e pulsanti.

![La nostra app di elenco di attività con le etichette degli elementi todo mostrate semplicemente tutte raggruppate su una riga](todo-list-unstructured-names.png)

Per risolvere il problema, occorre restituire un componente `<Todo />` dalla funzione `map()`: ricordare che JSX è JavaScript, quindi può essere usato insieme a qualunque altra sintassi JavaScript più familiare. Proviamo quanto segue al posto di ciò che è già presente:

```jsx
const taskList = props.tasks?.map((task) => <Todo />);
```

Guardare nuovamente l'app: ora le attività assomigliano di più a come erano prima, ma mancano i nomi delle attività stesse. Ricordare che ogni attività su cui viene eseguito `map` contiene le proprietà `id`, `name` e `completed` che vogliamo passare al componente `<Todo />`. Mettendo insieme queste informazioni, si ottiene codice simile al seguente:

```jsx
const taskList = props.tasks?.map((task) => (
  <Todo id={task.id} name={task.name} completed={task.completed} />
));
```

Ora l'app ha lo stesso aspetto di prima e il codice è meno ripetitivo.

## Chiavi univoche

Ora che React esegue il rendering delle attività da un array, deve tenere traccia di quale sia ciascuna di esse per eseguirne correttamente il rendering. React cerca di fare le proprie supposizioni per tenere traccia degli elementi, ma possiamo aiutarlo passando una prop `key` ai componenti `<Todo />`. `key` è una prop speciale gestita da React: non è possibile utilizzare la parola `key` per nessun altro scopo.

Poiché le chiavi devono essere univoche, riutilizzeremo l'`id` di ciascun oggetto attività come chiave. Aggiornare la costante `taskList` come segue:

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

**Passare sempre una chiave univoca a tutto ciò di cui viene eseguito il rendering tramite iterazione.** Nel browser non cambierà nulla di evidente, ma senza chiavi univoche React registrerà avvisi nella console e l'app potrebbe comportarsi in modo anomalo.

## Suddividere il resto dell'app in componenti

Ora che il componente più importante è stato sistemato, possiamo trasformare il resto dell'app in componenti. Ricordando che i componenti sono parti evidenti della UI, parti riutilizzate della UI, oppure entrambe le cose, possiamo creare altri due componenti:

- `<Form />`
- `<FilterButton />`

Poiché sappiamo di aver bisogno di entrambi, possiamo raggruppare parte del lavoro di creazione dei file in un unico comando del terminale. Eseguire questo comando nel terminale, facendo attenzione a trovarsi nella directory radice dell'app:

```bash
touch src/components/{Form,FilterButton}.jsx
```

### Il `<Form />`

Aprire `components/Form.jsx` ed effettuare le seguenti operazioni:

- Dichiarare una funzione `Form()` ed esportarla alla fine del file.
- Copiare i tag `<form>` e tutto ciò che è compreso tra di essi dall'interno di `App.jsx`, quindi incollarli nell'istruzione `return` di `Form()`.

Il file `Form.jsx` dovrebbe risultare così:

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

Eseguire le stesse operazioni eseguite per creare `Form.jsx` all'interno di `FilterButton.jsx`, ma chiamare il componente `FilterButton()` e copiare l'HTML del primo pulsante dentro `<div className="filters btn-group stack-exception">` da `App.jsx` nell'istruzione `return`.

Il file dovrebbe risultare così:

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
> Si potrebbe notare che qui viene commesso lo stesso errore commesso inizialmente per il componente `<Todo />`, poiché ogni pulsante sarà uguale. Va bene così. Questo componente verrà sistemato in seguito, in [Tornare ai pulsanti di filtro](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_interactivity_filtering_conditional_rendering#back_to_the_filter_buttons).

## Importare tutti i componenti

Mettiamo a frutto i nuovi componenti. Aggiungere altre istruzioni `import` all'inizio di `App.jsx` e fare riferimento ai componenti appena creati. Quindi, aggiornare l'istruzione `return` di `App()` affinché esegua il rendering dei componenti.

Al termine, `App.jsx` risulterà così:

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

Con questa configurazione, l'app React dovrebbe eseguire il rendering essenzialmente come prima, ma usando i nuovi e splendenti componenti.

## Riepilogo

E questo è tutto per questo articolo: abbiamo approfondito come suddividere efficacemente l'app in componenti ed eseguirne il rendering in modo efficiente. Successivamente vedremo come gestire gli eventi in React e inizieremo ad aggiungere interattività.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning","Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state", "Learn_web_development/Core/Frameworks_libraries")}}
