---
title: "Interattività di Ember: Eventi, classi e stato"
slug: Learn_web_development/Core/Frameworks_libraries/Ember_interactivity_events_state
l10n:
  sourceCommit: 611edf6335e4a833a6f394d0d98b117e7b0a36bf
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Ember_structure_componentization","Learn_web_development/Core/Frameworks_libraries/Ember_conditional_footer", "Learn_web_development/Core/Frameworks_libraries")}}

A questo punto inizieremo ad aggiungere un po' di interattività alla nostra app, fornendo la possibilità di aggiungere e visualizzare nuovi elementi "todo". Lungo il percorso, esamineremo l'utilizzo degli eventi in Ember, la creazione di classi di componenti per contenere il codice JavaScript per controllare funzionalità interattive e l'impostazione di un servizio per tenere traccia dello stato dei dati della nostra app.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È consigliato essere almeno familiari con i linguaggi di base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e avere conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/command line</a
          >.
        </p>
        <p>
          Una comprensione più approfondita delle funzionalità moderne di JavaScript (come classi,
          moduli, ecc.), sarà estremamente benefica, in quanto Ember ne fa largo uso.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare a creare classi di componenti e utilizzare eventi per controllare
        l'interattività, e tenere traccia dello stato dell'app utilizzando un servizio.
      </td>
    </tr>
  </tbody>
</table>

## Aggiunta di interattività

Ora che abbiamo una versione rifattorizzata e componentizzata della nostra app todo, vediamo come possiamo aggiungere l'interattività necessaria per rendere l'app funzionale.

Quando si inizia a pensare all'interattività, è utile dichiarare quali sono gli obiettivi e le responsabilità di ciascun componente. Nelle sezioni seguenti lo faremo per ciascun componente e poi ti accompagneremo su come la funzionalità può essere implementata.

## Creazione di todos

Per il nostro card-header / input todo, vogliamo poter inviare il nostro compito todo digitato quando premiamo il tasto <kbd>Enter</kbd> e farlo apparire nella lista dei todo.

Vogliamo catturare il testo digitato nell'input. Lo facciamo in modo che il nostro codice JavaScript sappia cosa abbiamo digitato, e possiamo salvare il nostro todo e passare quel testo al componente lista todo per visualizzarlo.

Possiamo catturare l'evento [`keydown`](/it/docs/Web/API/Element/keydown_event) tramite il [modificatore on](https://api.emberjs.com/ember/3.16/classes/Ember.Templates.helpers/methods/on?anchor=on), che è solo zucchero sintattico di Ember attorno a [`addEventListener`](/it/docs/Web/API/EventTarget/addEventListener) e [`removeEventListener`](/it/docs/Web/API/EventTarget/removeEventListener) (vedi [Introduzione agli eventi](/it/docs/Learn_web_development/Core/Scripting/Events) se necessario).

Aggiungi la nuova riga mostrata di seguito al tuo file `header.hbs`:

```hbs
<input
  class='new-todo'
  aria-label='What needs to be done?'
  placeholder='What needs to be done?'
  autofocus
  \{{on 'keydown' this.onKeyDown}}
>
```

Questo nuovo attributo è all'interno di doppi riccioli, il che indica che fa parte della sintassi dei template dinamici di Ember. Il primo argomento passato a `on` è il tipo di evento a cui rispondere (`keydown`), e l'ultimo argomento è il gestore degli eventi — il codice che verrà eseguito in risposta al trigger dell'evento `keydown`. Come ci si aspetterebbe trattando con [oggetti JavaScript vanilla](/it/docs/Learn_web_development/Core/Scripting/Object_basics#what_is_this), la parola chiave `this` si riferisce al "contesto" o allo "scope" del componente. Il `this` di un componente sarà diverso dal `this` di un altro componente.

Possiamo definire cosa è disponibile all'interno di `this` generando una classe di componenti da associare al tuo componente. Questa è una classe JavaScript vanilla e non ha un significato speciale per Ember, oltre all'_estensione_ dalla super-classe `Component`.

Per creare una classe header da associare al tuo componente header, digita questo nel tuo terminale:

```bash
ember generate component-class header
```

Questo creerà il seguente file vuoto di classe — `todomvc/app/components/header.js`:

```js
import Component from "@glimmer/component";

export default class HeaderComponent extends Component {}
```

All'interno di questo file implementeremo il codice del gestore degli eventi. Aggiorna il contenuto con il seguente:

```js
import Component from "@glimmer/component";
import { action } from "@ember/object";

export default class HeaderComponent extends Component {
  @action
  onKeyDown({ target, key }) {
    let text = target.value.trim();
    let hasValue = Boolean(text);

    if (key === "Enter" && hasValue) {
      alert(text);

      target.value = "";
    }
  }
}
```

Il decoratore `@action` è l'unico codice specifico di Ember qui (a parte l'estensione dalla super-classe `Component`, e gli elementi specifici di Ember che stiamo importando utilizzando la [sintassi dei moduli JavaScript](/it/docs/Web/JavaScript/Guide/Modules)) — il resto del file è JavaScript vanilla, e funzionerebbe in qualsiasi applicazione. Il decoratore `@action` dichiara che la funzione è un "azione", significando che è un tipo di funzione che verrà invocato da un evento occorso nel template. `@action` inoltre lega il `this` della funzione all'istanza della classe.

> [!NOTE]
> Un decoratore è fondamentalmente una funzione wrapper, che avvolge e chiama altre funzioni o proprietà, fornendo funzionalità aggiuntive lungo il percorso. Ad esempio, il decoratore `@tracked` (vedi leggermente più avanti) esegue il codice a cui si applica, ma inoltre lo traccia e aggiorna automaticamente l'app quando i valori cambiano. [Leggi JavaScript Decorators: What They Are and When to Use Them](https://www.sitepoint.com/javascript-decorators-what-they-are/) per ulteriori informazioni generali sui decoratori.

Tornando alla nostra scheda del browser con l'app in esecuzione, possiamo digitare qualsiasi cosa vogliamo, e quando premiamo <kbd>Enter</kbd> ci sarà mostrato un messaggio di allerta che ci dice esattamente cosa abbiamo digitato.

![lo stato iniziale del placeholder della funzione aggiungi, mostrando il testo inserito negli elementi di input avvisato di ritorno a te.](todos-hello-there-alert.png)

Con l'interattività dell'input header fuori dal modo, abbiamo bisogno di un posto dove archiviare i todos in modo che altri componenti possano accedervi.

## Memorizzazione dei Todos con un servizio

Ember ha un sistema di gestione dello **stato** a livello di applicazione integrato che possiamo utilizzare per gestire la memorizzazione dei nostri todos e permettere a ciascuno dei nostri componenti di accedere ai dati da quello stato a livello di applicazione. Ember chiama queste strutture [Servizi](https://guides.emberjs.com/release/services/), e vivono per tutta la durata della pagina (un aggiornamento della pagina li cancellerà; la persistenza dei dati più a lungo è oltre lo scopo di questo tutorial).

Esegui questo comando terminale per generare un servizio per noi in cui memorizzare i nostri dati della lista todo:

```bash
ember generate service todo-data
```

Questo dovrebbe darti un output del terminale simile a questo:

```plain
installing service
  create app/services/todo-data.js
installing service-test
  create tests/unit/services/todo-data-test.js
```

Questo crea un file `todo-data.js` all'interno della directory `todomvc/app/services` per contenere il nostro servizio, che inizialmente contiene un'istruzione di importazione e una classe vuota:

```js
import Service from "@ember/service";

export default class TodoDataService extends Service {}
```

Prima di tutto, vogliamo definire _cos'è un todo_. Sappiamo di voler tracciare sia il testo di un todo, sia se è stato completato o meno.

Aggiungi la seguente istruzione `import` sotto quella esistente:

```js
import { tracked } from "@glimmer/tracking";
```

Ora aggiungi la seguente classe sotto la riga precedente che hai aggiunto:

```js
class Todo {
  @tracked text = "";
  @tracked isCompleted = false;

  constructor(text) {
    this.text = text;
  }
}
```

Questa classe rappresenta un todo — contiene una proprietà `@tracked text` che contiene il testo del todo, e una proprietà `@tracked isCompleted` che specifica se il todo è stato completato o meno. Quando viene istanziato, un oggetto `Todo` avrà un valore iniziale `text` uguale al testo fornito al momento della creazione (vedi sotto), e un valore `isCompleted` di `false`. L'unica parte specifica di Ember in questa classe è il decoratore `@tracked` — questo si integra nel sistema di reattività e consente a Ember di aggiornare ciò che stai vedendo nella tua app automaticamente se le proprietà tracciate cambiano. [Maggiori informazioni su tracked possono essere trovate qui](https://api.emberjs.com/ember/3.15/functions/@glimmer%2Ftracking/tracked).

Ora è il momento di aggiungere al corpo del servizio.

Per prima cosa aggiungi un'altra istruzione `import` sotto la precedente, per rendere disponibili le azioni all'interno del servizio:

```js
import { action } from "@ember/object";
```

Aggiorna il blocco esistente `export default class TodoDataService extends Service { }` come segue:

```js
export default class TodoDataService extends Service {
  @tracked todos = [];

  @action
  add(text) {
    let newTodo = new Todo(text);

    this.todos = [...this.todos, newTodo];
  }
}
```

Qui, la proprietà `todos` sul servizio manterrà la nostra lista di todos contenuti all'interno di un array, e la segneremo con `@tracked`, perché quando il valore di `todos` viene aggiornato vogliamo che anche l'interfaccia utente si aggiorni.

E proprio come prima, la funzione `add()` che sarà chiamata dal template viene annotata con il decoratore `@action` per legarla all'istanza della classe. Utilizziamo [lo spread del nostro array `todos`](/it/docs/Web/JavaScript/Reference/Operators/Spread_syntax) per aggiungere `newTodo` ad esso, il che crea un nuovo array e attiverà il sistema di reattività per aggiornare l'interfaccia utente.

## Utilizzo del servizio dal nostro componente header

Ora che abbiamo definito un modo per aggiungere todos, possiamo interagire con questo servizio dal componente input `header.js` per iniziare effettivamente ad aggiungerli.

Prima di tutto, il servizio deve essere iniettato nel template tramite il decoratore `@inject`, che rinomineremo in `@service` per chiarezza semantica. Per fare ciò, aggiungi la seguente riga di `import` in `header.js`, sotto le due righe di `import` esistenti:

```js
import { inject as service } from "@ember/service";
```

Con questo import in atto, possiamo ora rendere il servizio `todo-data` disponibile all'interno della classe `HeaderComponent` tramite l'oggetto `todos`, utilizzando il decoratore `@service`. Aggiungi la seguente riga appena sotto la riga di apertura `export…`:

```js
export default class HeaderComponent extends Component {
  @service("todo-data") todos;
  // …
}
```

Ora la riga placeholder `alert(text);` può essere sostituita con una chiamata alla nostra nuova funzione `add()`. Sostituiscila con il seguente:

```js
this.todos.add(text);
```

Se proviamo questo nella app todo nel nostro browser (`npm start`, vai su `localhost:4200`), sembrerà che non accada nulla dopo aver premuto il tasto <kbd>Enter</kbd> (sebbene il fatto che l'app si costruisca senza errori sia un buon segno). Utilizzando l'[Ember Inspector](https://guides.emberjs.com/release/ember-inspector/installation/) tuttavia, possiamo vedere che il nostro todo è stato aggiunto:

![L'app mostrata nell'ispettore di Ember, per dimostrare che i todo aggiunti sono memorizzati dal servizio, anche se non vengono ancora visualizzati nell'interfaccia utente](todos-in-ember-inspector.gif)

## Visualizzazione dei nostri todos

Ora che sappiamo che possiamo creare todos, ci deve essere un modo per sostituire i nostri statici "Acquista biglietti del cinema" todos con i todos che stiamo effettivamente creando. Nel componente `TodoList`, vogliamo ottenere i todos dal servizio, e renderizzare un componente `Todo` per ciascun todo.

Per recuperare i todos dal servizio, il nostro componente `TodoList` ha prima bisogno di una classe di componenti di supporto per contenere questa funzionalità. Premi <kbd>Ctrl</kbd> + <kbd>C</kbd> per fermare il server di sviluppo, ed esegui il seguente comando nel terminale:

```bash
ember generate component-class todo-list
```

Questo genera la nuova classe di componenti `todomvc/app/components/todo-list.js`.

Popola questo file con il seguente codice, che espone il servizio `todo-data`, tramite la proprietà `todos`, al nostro template. Questo lo rende accessibile tramite `this.todos` sia all'interno della classe che nel template:

```js
import Component from "@glimmer/component";
import { inject as service } from "@ember/service";

export default class TodoListComponent extends Component {
  @service("todo-data") todos;
}
```

Un problema qui è che il nostro servizio si chiama `todos`, ma anche la lista dei todos si chiama `todos`, quindi attualmente accederemmo ai dati usando `this.todos.todos`. Questo non è intuitivo, quindi aggiungeremo un [getter](/it/docs/Web/JavaScript/Reference/Functions/get) al servizio `todos` chiamato `all`, che rappresenterà tutti i todos.

Per fare questo, torna al tuo file `todo-data.js` e aggiungi quanto segue sotto la riga `@tracked todos = [];`:

```js
export default class TodoDataService extends Service {
  @tracked todos = [];

  get all() {
    return this.todos;
  }
  // …
}
```

Ora possiamo accedere ai dati usando `this.todos.all`, che è molto più intuitivo. Per mettere questo in azione, vai al tuo componente `todo-list.hbs`, e sostituisci le chiamate statiche dei componenti:

```hbs
<Todo />
<Todo />
```

Con un blocco dinamico `#each` (che è fondamentalmente zucchero sintattico sopra la funzionalità [`forEach()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) di JavaScript) che crea un componente `<Todo />` per ciascun todo disponibile nella lista dei todos restituita dal getter `all()` del servizio:

```hbs-nolint
\{{#each this.todos.all as |todo|}}
<Todo @todo=\{{todo}} />
\{{/each}}
```

Un altro modo per vedere questo:

- `this` — il contesto di rendering / istanza del componente.
- `todos` — una proprietà su `this`, che abbiamo definito nel componente `todo-list.js` utilizzando `@service('todo-data') todos;`. Questo è un riferimento al servizio `todo-data`, che ci consente di interagire direttamente con l'istanza del servizio.
- `all` — un getter sul servizio `todo-data` che restituisce tutti i todos.

Prova a riavviare il server e a navigare verso la nostra app, e scoprirai che funziona! Beh, quasi. Ogni volta che inserisci un nuovo elemento To-do, appare un nuovo elemento di lista sotto l'input di testo, ma purtroppo dice sempre "Acquista biglietti del cinema".

Questo perché l'etichetta di testo all'interno di ciascun elemento di lista è codificata in quel testo, come si vede in `todo.hbs`:

```hbs
<label>Buy Movie Tickets</label>
```

Aggiorna questa linea per utilizzare l'Argomento `@todo` — che rappresenterà il Todo che abbiamo passato in questo componente quando è stato invocato in `todo-list.hbs`, nella riga `<Todo @todo=\{{todo}} />`:

```hbs
<label>\{{@todo.text}}</label>
```

OK, prova di nuovo. Dovresti scoprire che ora il testo immesso nell'`<input>` è correttamente riflesso nell'interfaccia utente:

![L'app mostrata nel suo stato finale di questo articolo, con gli elementi todo inseriti mostrati nell'interfaccia utente](todos-being-appended-with-correct-text.gif)

## Riepilogo

OK, quindi è un grande progresso per ora. Ora possiamo aggiungere elementi todo alla nostra app, e lo stato dei dati è tracciato utilizzando il nostro servizio. Successivamente passeremo a far funzionare la funzionalità del nostro footer, incluso il contatore dei todo, e vedremo il rendering condizionale, incluso lo styling corretto dei todo quando sono stati contrassegnati. Collegheremo anche il nostro pulsante "Cancella completati".

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Ember_structure_componentization","Learn_web_development/Core/Frameworks_libraries/Ember_conditional_footer", "Learn_web_development/Core/Frameworks_libraries")}}
