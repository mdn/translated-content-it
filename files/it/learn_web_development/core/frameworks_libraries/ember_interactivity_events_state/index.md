---
title: "Interattività di Ember: Eventi, classi e stato"
slug: Learn_web_development/Core/Frameworks_libraries/Ember_interactivity_events_state
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Ember_structure_componentization","Learn_web_development/Core/Frameworks_libraries/Ember_conditional_footer", "Learn_web_development/Core/Frameworks_libraries")}}

A questo punto inizieremo ad aggiungere un po' di interattività alla nostra app, fornendo la possibilità di aggiungere e visualizzare nuovi elementi todo. Lungo il percorso, vedremo l'uso degli eventi in Ember, la creazione di classi di componenti per contenere il codice JavaScript per controllare le funzionalità interattive e l'impostazione di un servizio per tenere traccia dello stato dei dati della nostra app.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È raccomandato almeno conoscere i linguaggi di base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenze sull'
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >uso del terminale/linea di comando</a
          >.
        </p>
        <p>
          Una comprensione più approfondita delle caratteristiche moderne di JavaScript (come classi,
          moduli, ecc.) sarà estremamente utile, dato che Ember ne fa largo uso.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare come creare classi di componenti e utilizzare eventi per controllare
        l'interattività, oltre a tenere traccia dello stato dell'app tramite un servizio.
      </td>
    </tr>
  </tbody>
</table>

## Aggiunta di Interattività

Ora che abbiamo una versione ricomposta della nostra app todo, vediamo come possiamo aggiungere l'interattività necessaria per rendere l'app funzionale.

Quando si inizia a pensare all'interattività, è utile dichiarare quali siano gli obiettivi e le responsabilità di ciascun componente. Nelle sezioni seguenti lo faremo per ciascun componente e poi vedremo come implementare la funzionalità.

## Creazione di todo

Per il nostro card-header / input todo, vogliamo poter inviare il task del todo digitato quando premiamo il tasto <kbd>Invio</kbd> e farlo apparire nell'elenco dei todo.

Vogliamo essere in grado di catturare il testo digitato nell'input. Facciamo questo in modo che il nostro codice JavaScript sappia cosa abbiamo digitato, possiamo salvare il nostro todo e passare quel testo al componente della lista todo per visualizzarlo.

Possiamo catturare l'evento [`keydown`](/it/docs/Web/API/Element/keydown_event) tramite il [modificatore on](https://api.emberjs.com/ember/3.16/classes/Ember.Templates.helpers/methods/on?anchor=on), che è semplicemente una sintassi semplificata di Ember per [`addEventListener`](/it/docs/Web/API/EventTarget/addEventListener) e [`removeEventListener`](/it/docs/Web/API/EventTarget/removeEventListener) (vedere [Introduzione agli eventi](/it/docs/Learn_web_development/Core/Scripting/Events) se necessario).

Aggiungi la seguente nuova riga al tuo file `header.hbs`:

```hbs
<input
  class='new-todo'
  aria-label='What needs to be done?'
  placeholder='What needs to be done?'
  autofocus
  \{{on 'keydown' this.onKeyDown}}
>
```

Questo nuovo attributo è all'interno di doppie parentesi graffe, il che indica che fa parte della sintassi di templating dinamico di Ember. Il primo argomento passato a `on` è il tipo di evento a cui rispondere (`keydown`), e l'ultimo argomento è il gestore dell'evento — il codice che verrà eseguito in risposta al lancio dell'evento `keydown`. Come ci si può aspettare dall'interazione con [oggetti JavaScript standard](/it/docs/Learn_web_development/Core/Scripting/Object_basics#what_is_this), la keyword `this` si riferisce al "contesto" o "ambito" del componente. Il `this` di un componente sarà diverso dal `this` di un altro componente.

Possiamo definire cosa è disponibile all'interno di `this` generando una classe di componente da associare al tuo componente. Questa è una classe JavaScript standard e non ha un significato speciale per Ember, a parte _estendere_ dalla superclasse `Component`.

Per creare una classe header associata al tuo componente header, digita questo nel tuo terminale:

```bash
ember generate component-class header
```

Questo creerà il seguente file di classe vuoto — `todomvc/app/components/header.js`:

```js
import Component from "@glimmer/component";

export default class HeaderComponent extends Component {}
```

All'interno di questo file implementeremo il codice del gestore degli eventi. Aggiorna il contenuto nel modo seguente:

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

Il decoratore `@action` è l'unico codice specifico di Ember qui (a parte l'estensione dalla superclasse `Component` e gli elementi specifici di Ember che stiamo importando usando la [sintassi dei moduli JavaScript](/it/docs/Web/JavaScript/Guide/Modules)) — il resto del file è JavaScript standard e funzionerebbe in qualsiasi applicazione. Il decoratore `@action` dichiara che la funzione è un "azione", il che significa che è un tipo di funzione che verrà richiamata da un evento che si è verificato nel template. `@action` associa anche il `this` della funzione all'istanza della classe.

> [!NOTE]
> Un decoratore è fondamentalmente una funzione wrapper, che avvolge e chiama altre funzioni o proprietà, fornendo funzionalità aggiuntive lungo il percorso. Ad esempio, il decoratore `@tracked` (che vedremo a breve) esegue il codice a cui è applicato, ma lo traccia anche e aggiorna automaticamente l'app quando cambiano i valori. [Legga JavaScript Decorators: What They Are and When to Use Them](https://www.sitepoint.com/javascript-decorators-what-they-are/) per ulteriori informazioni generali sui decoratori.

Tornando alla nostra scheda nel browser con l'app in esecuzione, possiamo digitare ciò che vogliamo e quando premiamo <kbd>Enter</kbd> riceveremo un messaggio di avviso che ci dice esattamente cosa abbiamo digitato.

![lo stato iniziale di placeholder della funzione add, che mostra il testo inserito negli elementi di input che ti viene restituito in un avviso.](todos-hello-there-alert.png)

Con l'interattività dell'input dell'header fuori dai piedi, abbiamo bisogno di un posto dove memorizzare i todo in modo che altri componenti possano accedervi.

## Memorizzazione dei Todo con un servizio

Ember ha una gestione dello stato **a livello di applicazione** incorporata che possiamo usare per gestire l'archiviazione dei nostri todo e permettere a ciascuno dei nostri componenti di accedere ai dati da quello stato a livello di applicazione. Ember chiama queste strutture [Services](https://guides.emberjs.com/release/services/), e durano per l'intera durata della pagina (un refresh della pagina le cancellerà; mantenere i dati per un tempo più lungo è al di fuori dello scopo di questo tutorial).

Esegui questo comando terminale per generare un servizio per noi per memorizzare i dati del nostro elenco dei todo:

```bash
ember generate service todo-data
```

Dovrebbe darti un output sulla console simile a questo:

```plain
installing service
  create app/services/todo-data.js
installing service-test
  create tests/unit/services/todo-data-test.js
```

Questo crea un file `todo-data.js` all'interno della directory `todomvc/app/services` per contenere il nostro servizio, che inizialmente contiene una dichiarazione di importazione e una classe vuota:

```js
import Service from "@ember/service";

export default class TodoDataService extends Service {}
```

Innanzitutto, vogliamo definire _cos'è un todo_. Sappiamo che vogliamo tracciare sia il testo di un todo, sia se è stato completato o meno.

Aggiungi la seguente dichiarazione `import` sotto quella esistente:

```js
import { tracked } from "@glimmer/tracking";
```

Ora aggiungi la seguente classe sotto la riga precedente:

```js
class Todo {
  @tracked text = "";
  @tracked isCompleted = false;

  constructor(text) {
    this.text = text;
  }
}
```

Questa classe rappresenta un todo — contiene una proprietà `@tracked text` che contiene il testo del todo e una proprietà `@tracked isCompleted` che specifica se il todo è stato completato o meno. Quando viene istanziato, un oggetto `Todo` avrà un valore iniziale `text` uguale al testo fornito quando creato (vedi sotto), e un valore `isCompleted` di `false`. L'unica parte specifica di Ember di questa classe è il decoratore `@tracked` — questo si collega al sistema di reattività e consente a Ember di aggiornare ciò che stai vedendo nella tua app automaticamente se le proprietà tracciate cambiano. [Ulteriori informazioni su tracked possono essere trovate qui](https://api.emberjs.com/ember/3.15/functions/@glimmer%2Ftracking/tracked).

Ora è il momento di aggiungere al corpo del servizio.

Prima aggiungi un'altra dichiarazione `import` sotto quella precedente, per rendere le azioni disponibili all'interno del servizio:

```js
import { action } from "@ember/object";
```

Aggiorna il blocco `export default class TodoDataService extends Service { }` esistente come segue:

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

Qui, la proprietà `todos` sul servizio manterrà il nostro elenco di todo contenuti all'interno di un array, e lo contrassegneremo con `@tracked`, perché quando il valore di `todos` viene aggiornato vogliamo che l'interfaccia utente si aggiorni anch'essa.

E proprio come prima, la funzione `add()` che verrà chiamata dal template viene annotata con il decoratore `@action` per legarla all'istanza della classe. Distribuiamo il [nostro array `todos`](/it/docs/Web/JavaScript/Reference/Operators/Spread_syntax) per aggiungere `newTodo` ad esso, creando un nuovo array che attiverà il sistema di reattività per aggiornare l'interfaccia utente.

## Utilizzare il servizio dal nostro componente header

Ora che abbiamo definito un modo per aggiungere todo, possiamo interagire con questo servizio dal componente di input `header.js` per iniziare effettivamente ad aggiungerli.

Prima di tutto, il servizio deve essere iniettato nel template tramite il decoratore `@inject`, che rinomineremo in `@service` per chiarezza semantica. Per fare questo, aggiungi la seguente riga `import` a `header.js`, sotto le due righe di `import` esistenti:

```js
import { inject as service } from "@ember/service";
```

Con questa importazione in atto, possiamo ora rendere disponibile il servizio `todo-data` all'interno della classe `HeaderComponent` tramite l'oggetto `todos`, utilizzando il decoratore `@service`. Aggiungi la seguente riga subito sotto la riga di apertura `export…`:

```js
@service('todo-data') todos;
```

Ora la riga placeholder `alert(text);` può essere sostituita con una chiamata alla nostra nuova funzione `add()`. Sostituiscila con la seguente:

```js
this.todos.add(text);
```

Se proviamo questo nella app todo nel nostro browser (`npm start`, vai a `localhost:4200`), sembrerà che non accada nulla dopo aver premuto il tasto <kbd>Invio</kbd> (anche se il fatto che l'app si compili senza errori è un buon segno). Usando l'[Ember Inspector](https://guides.emberjs.com/release/ember-inspector/installation/) tuttavia, possiamo vedere che il nostro todo è stato aggiunto:

![L'app viene mostrata nell'Ember inspector, per dimostrare che i todo aggiunti vengono memorizzati dal servizio, anche se non vengono ancora visualizzati nell'interfaccia utente](todos-in-ember-inspector.gif)

## Visualizzazione dei nostri todo

Ora che sappiamo di poter creare todo, è necessario un modo per sostituire i nostri todo "Compra i biglietti del cinema" statici con i todo che stiamo effettivamente creando. Nel componente `TodoList`, vorremo estrarre i todo dal servizio, e rendere un componente `Todo` per ciascun todo.

Per recuperare i todo dal servizio, il nostro componente `TodoList` ha bisogno prima di una classe di componente di supporto per contenere questa funzionalità. Premi <kbd>Ctrl</kbd> + <kbd>C</kbd> per fermare il server di sviluppo ed esegui il seguente comando terminale:

```bash
ember generate component-class todo-list
```

Questo genera la nuova classe del componente `todomvc/app/components/todo-list.js`.

Popola questo file con il seguente codice, che espone il servizio `todo-data`, tramite la proprietà `todos`, al nostro template. Ciò lo rende accessibile tramite `this.todos` sia all'interno della classe che del template:

```js
import Component from "@glimmer/component";
import { inject as service } from "@ember/service";

export default class TodoListComponent extends Component {
  @service("todo-data") todos;
}
```

Un problema qui è che il nostro servizio si chiama `todos`, ma la lista dei todo è anche chiamata `todos`, quindi attualmente accederemmo ai dati usando `this.todos.todos`. Questo non è intuitivo, quindi aggiungeremo un [getter](/it/docs/Web/JavaScript/Reference/Functions/get) al servizio `todos` chiamato `all`, che rappresenterà tutti i todo.

Per fare ciò, torna al tuo file `todo-data.js` e aggiungi il seguente sotto la riga `@tracked todos = [];`:

```js
get all() {
  return this.todos;
}
```

Ora possiamo accedere ai dati utilizzando `this.todos.all`, che è molto più intuitivo. Per mettere questo in azione, vai al tuo componente `todo-list.hbs`, e sostituisci le chiamate ai componenti statici:

```hbs
<Todo />
<Todo />
```

Con un blocco `#each` dinamico (che è sostanzialmente una sintassi semplificata sopra lo [`forEach()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) di JavaScript) che crea un componente `<Todo />` per ciascun todo disponibile nell'elenco dei todo restituito dal getter `all()` del servizio:

```hbs-nolint
\{{#each this.todos.all as |todo|}}
<Todo @todo=\{{todo}} />
\{{/each}}
```

Un altro modo per vedere questo:

- `this` — il contesto di rendering / istanza del componente.
- `todos` — una proprietà su `this`, che abbiamo definito nel componente `todo-list.js` usando `@service('todo-data') todos;`. Questo è un riferimento al servizio `todo-data`, che ci consente di interagire direttamente con l'istanza del servizio.
- `all` — un getter sul servizio `todo-data` che restituisce tutti i todo.

Prova a riavviare il server e a navigare alla nostra app, e scoprirai che funziona! Bene, in qualche modo. Ogni volta che inserisci un nuovo elemento Todo, appare un nuovo elemento della lista sotto l'input di testo, ma sfortunatamente dice sempre "Compra i biglietti del cinema".

Questo perché l'etichetta di testo all'interno di ciascun elemento della lista è codificata con tale testo, come si vede in `todo.hbs`:

```hbs
<label>Buy Movie Tickets</label>
```

Aggiorna questa riga per utilizzare l'argomento `@todo` — che rappresenterà il Todo che abbiamo passato a questo componente quando è stato invocato in `todo-list.hbs`, nella riga `<Todo @todo=\{{todo}} />`:

```hbs
<label>\{{@todo.text}}</label>
```

OK, prova di nuovo. Dovresti scoprire che ora il testo inserito nell'`<input>` è correttamente riflesso nell'interfaccia utente:

![L'app viene mostrata nel suo stato finale di questo articolo, con gli elementi todo inseriti che vengono mostrati nell'interfaccia utente](todos-being-appended-with-correct-text.gif)

## Sommario

OK, questo è un grande progresso per ora. Possiamo ora aggiungere elementi todo alla nostra app, e lo stato dei dati è tracciato utilizzando il nostro servizio. Successivamente, passeremo al funzionamento della funzionalità del nostro footer, incluso il contatore dei todo, e guarderemo al rendering condizionale, inclusa la corretta formattazione dei todo quando sono stati selezionati. Collegheremo anche il nostro pulsante "Cancella completati".

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Ember_structure_componentization","Learn_web_development/Core/Frameworks_libraries/Ember_conditional_footer", "Learn_web_development/Core/Frameworks_libraries")}}
