---
title: Creazione del nostro primo componente Vue
slug: Learn_web_development/Core/Frameworks_libraries/Vue_first_component
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_getting_started","Learn_web_development/Core/Frameworks_libraries/Vue_rendering_lists", "Learn_web_development/Core/Frameworks_libraries")}}

Ora è il momento di approfondire Vue e creare il nostro componente personalizzato — cominceremo creando un componente per rappresentare ciascun elemento nella lista delle cose da fare. Lungo il percorso, impareremo alcuni concetti importanti come l'invocazione di componenti all'interno di altri componenti, il passaggio di dati tramite props e la memorizzazione dello stato dei dati.

> [!NOTE]
> Se ha bisogno di confrontare il suo codice con la nostra versione, può trovare una versione completa del codice dell'app di esempio Vue nel nostro [repository todo-vue](https://github.com/mdn/todo-vue). Per una versione live, vedere <https://mdn.github.io/todo-vue/>.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Familiarità con i linguaggi <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a> di base, conoscenza del <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/riga di comando</a>.
        </p>
        <p>
          I componenti Vue sono scritti come una combinazione di oggetti JavaScript che gestiscono i dati dell'app e una sintassi di template basata su HTML che si mappa sulla struttura del DOM sottostante. Per l'installazione, e per utilizzare alcune delle funzionalità più avanzate di Vue (come i Componenti a File Singolo o le funzioni di rendering), sarà necessario un terminale con <a href="https://nodejs.org/en/download" rel="noopener noreferrer" target="_blank">Node</a> e <a href="https://www.npmjs.com/get-npm" rel="noopener noreferrer" target="_blank">npm</a> installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare a creare un componente Vue, renderizzarlo all'interno di un altro componente, passare i dati usando i props e salvare il suo stato.
      </td>
    </tr>
  </tbody>
</table>

## Creazione di un componente ToDoItem

Creiamo il nostro primo componente, che visualizzerà un singolo elemento della lista delle cose da fare. Lo useremo per costruire la lista delle cose da fare.

1. Nella sua directory `moz-todo-vue/src/components`, crei un nuovo file chiamato `ToDoItem.vue`. Apra il file nel suo editor di codice.
2. Crei la sezione del template del componente aggiungendo `<template></template>` nella parte superiore del file.
3. Crei una sezione `<script></script>` sotto la sezione del template. All'interno dei tag `<script>`, aggiunga un oggetto esportato di default `export default {}`, che è l'oggetto del suo componente.

Il suo file dovrebbe ora apparire così:

```vue
<template></template>
<script>
export default {};
</script>
```

Possiamo ora iniziare ad aggiungere contenuto effettivo al nostro `ToDoItem`. Attualmente, i template di Vue consentono solo un singolo elemento radice — un elemento deve racchiudere tutto all'interno della sezione template (questo cambierà quando uscirà Vue 3). Useremo un [`<div>`](/it/docs/Web/HTML/Reference/Elements/div) per quell'elemento radice.

1. Aggiunga ora un `<div>` vuoto all'interno del template del suo componente.
2. All'interno di quel `<div>`, aggiungiamo una casella di controllo e un'etichetta corrispondente. Aggiunga un attributo `id` alla casella di controllo e un attributo `for` che associa la casella di controllo all'etichetta, come mostrato di seguito.

   ```vue
   <template>
     <div>
       <input type="checkbox" id="todo-item" />
       <label for="todo-item">My Todo Item</label>
     </div>
   </template>
   ```

### Uso di TodoItem all'interno della nostra app

Va tutto bene, ma non abbiamo ancora aggiunto il componente alla nostra app, quindi non c'è modo di testarlo e verificare se tutto funziona. Aggiungiamolo ora.

1. Apra nuovamente `App.vue`.
2. Nella parte superiore del suo tag `<script>`, aggiunga il seguente codice per importare il suo componente `ToDoItem`:

   ```js
   import ToDoItem from "./components/ToDoItem.vue";
   ```

3. All'interno del suo oggetto componente, aggiunga la proprietà `components` e al suo interno aggiunga il componente `ToDoItem` per registrarlo.

Il contenuto del suo `<script>` dovrebbe ora apparire così:

```js
import ToDoItem from "./components/ToDoItem.vue";

export default {
  name: "app",
  components: {
    ToDoItem,
  },
};
```

Questo è lo stesso modo in cui il componente `HelloWorld` è stato registrato dal CLI di Vue in precedenza.

Per effettivamente renderizzare il componente `ToDoItem` nell'app, deve andare nel suo elemento `<template>` e chiamarlo come un elemento `<to-do-item></to-do-item>`. Noti che il nome del file componente e la sua rappresentazione in JavaScript è in PascalCase (ad esempio, `ToDoList`), e l'elemento personalizzato equivalente è in {{Glossary("kebab_case", "kebab-case")}} (ad esempio, `<to-do-list>`). È necessario utilizzare questo stile di scrittura se scrive i template Vue [direttamente nel DOM](https://vuejs.org/guide/essentials/component-basics.html#dom-template-parsing-caveats).

1. Sotto il [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements), crei una lista non ordinata ([`<ul>`](/it/docs/Web/HTML/Reference/Elements/ul)) contenente un singolo elemento di lista ([`<li>`](/it/docs/Web/HTML/Reference/Elements/li)).
2. All'interno dell'elemento di lista aggiunga `<to-do-item></to-do-item>`.

La sezione `<template>` del suo file `App.vue` dovrebbe ora apparire così:

```vue
<div id="app">
  <h1>To-Do List</h1>
  <ul>
    <li>
      <to-do-item></to-do-item>
    </li>
  </ul>
</div>
```

Se controlla di nuovo la sua app renderizzata, dovrebbe ora vedere il suo `ToDoItem` renderizzato, composto da una casella di controllo e un'etichetta.

![Lo stato attuale di rendering dell'app, che include un titolo To-Do List e una singola casella di controllo e un'etichetta](rendered-todoitem.png)

## Rendere i componenti dinamici con i props

Il nostro componente `ToDoItem` non è ancora molto utile perché possiamo includerlo solo una volta in una pagina (gli ID devono essere unici) e non abbiamo modo di impostare il testo dell'etichetta. Niente di questo è dinamico.

Ci serve del componente stato. Ciò può essere ottenuto aggiungendo props al nostro componente. Può considerare i props come input di una funzione. Il valore di un prop fornisce ai componenti uno stato iniziale che influenza la loro visualizzazione.

### Registrazione dei props

In Vue, ci sono due modi per registrare i props:

- Il primo modo è elencare i props come un array di stringhe. Ogni voce nell'array corrisponde al nome di un prop.
- Il secondo modo è definire i props come un oggetto, con ogni chiave corrispondente al nome del prop. Elencare i props come un oggetto consente di specificare valori predefiniti, contrassegnare i props come richiesti, eseguire una tipizzazione di base degli oggetti (in particolare sui tipi primitivi JavaScript) ed eseguire una semplice validazione dei props.

> [!NOTE]
> La validazione dei props avviene solo in modalità di sviluppo, quindi non può rigorosamente fare affidamento su di essa in produzione. Inoltre, le funzioni di validazione dei props sono invocate prima che l'istanza del componente venga creata, quindi non hanno accesso allo stato del componente (o ad altri props).

Per questo componente, utilizzeremo il metodo di registrazione degli oggetti.

1. Torni al suo file `ToDoItem.vue`.
2. Aggiunga una proprietà `props` all'interno dell'oggetto export `default {}`, che contiene un oggetto vuoto.
3. All'interno di questo oggetto, aggiunga due proprietà con le chiavi `label` e `done`.
4. Il valore della chiave `label` dovrebbe essere un oggetto con 2 proprietà (o **props**, come sono chiamate nel contesto di essere disponibili ai componenti).

   1. La prima è una proprietà `required`, che avrà un valore di `true`. Ciò dirà a Vue che ci aspettiamo che ogni istanza di questo componente abbia un campo etichetta. Vue ci avvertirà se un componente `ToDoItem` non ha un campo etichetta.
   2. La seconda proprietà che aggiungeremo è una proprietà `type`. Imposti il valore di questa proprietà come il tipo `String` di JavaScript (noti la "S" maiuscola). Ciò indica a Vue che ci aspettiamo che il valore di questa proprietà sia una stringa.

5. Ora passiamo al prop `done`.

   1. Prima aggiunga un campo `default`, con un valore di `false`. Ciò significa che quando non viene passato alcun prop `done` a un componente `ToDoItem`, il prop `done` avrà un valore di false (tenga presente che ciò non è richiesto — abbiamo bisogno di `default` solo sui props non richiesti).
   2. Successivamente aggiunga un campo `type` con un valore di `Boolean`. Ciò indica che ci aspettiamo che il valore del prop sia un tipo booleano di JavaScript.

Il suo oggetto componente dovrebbe ora apparire così:

```js
export default {
  props: {
    label: { required: true, type: String },
    done: { default: false, type: Boolean },
  },
};
```

### Uso dei props registrati

Con questi props definiti all'interno dell'oggetto componente, possiamo ora usare questi valori variabili all'interno del nostro template. Iniziamo aggiungendo il prop `label` al template del componente.

Nel suo `<template>`, sostituisca il contenuto dell'elemento `<label>` con `\{{label}}`.

`\{{}}` è una sintassi di template speciale in Vue, che ci permette di stampare il risultato delle espressioni JavaScript definite nella nostra classe, all'interno del nostro template, inclusi valori e metodi. È importante sapere che il contenuto all'interno di `\{{}}` viene visualizzato come testo e non come HTML. In questo caso, stiamo stampando il valore del prop `label`.

La sezione template del suo componente dovrebbe ora apparire così:

```vue
<template>
  <div>
    <input type="checkbox" id="todo-item" />
    <label for="todo-item">\{{ label }}</label>
  </div>
</template>
```

Torni al suo browser e vedrà l'elemento della lista delle cose da fare renderizzato come prima, ma senza un'etichetta (oh no!). Vada agli strumenti di sviluppo del suo browser e vedrà un avviso del genere nella console:

```plain
[Vue warn]: Missing required prop: "label"

found in

---> <ToDoItem> at src/components/ToDoItem.vue
        <App> at src/App.vue
          <Root>
```

Ciò avviene perché abbiamo contrassegnato `label` come un prop obbligatorio, ma non abbiamo mai fornito quel prop al componente — abbiamo definito dove all'interno del template vogliamo che venga usato, ma non l'abbiamo passato nel componente quando lo abbiamo chiamato. Risolviamolo.

All'interno del suo file `App.vue`, aggiunga un prop `label` al componente `<to-do-item></to-do-item>`, proprio come un normale attributo HTML:

```vue
<to-do-item label="My ToDo Item"></to-do-item>
```

Ora vedrà l'etichetta nella sua app e l'avviso non verrà visualizzato di nuovo nella console.

Quindi questo è il concetto di props in sintesi. Passeremo ora a come Vue persiste lo stato dei dati.

## Oggetto dei dati di Vue

Se cambia il valore del prop `label` passato nella chiamata `<to-do-item></to-do-item>` nel suo componente `App`, dovrebbe vedere l'aggiornamento. Questo è fantastico. Abbiamo una casella di controllo, con un'etichetta aggiornabile. Tuttavia, attualmente non stiamo facendo nulla con il prop "done" — possiamo controllare le caselle di controllo nell'interfaccia utente, ma da nessuna parte nell'app stiamo registrando se un elemento della lista delle cose da fare è effettivamente completato.

Per raggiungere questo obiettivo, vogliamo collegare il prop `done` all'attributo `checked` sull'elemento `<input>`, in modo che possa servire come un record di se la casella di controllo è selezionata o meno. Tuttavia, è importante che i props servano come binding monodirezionali dei dati — un componente non dovrebbe mai alterare il valore dei suoi props. Ci sono molte ragioni per questo. In parte, i componenti che modificano i props possono rendere difficile il debug. Se un valore viene passato a più bambini, potrebbe essere difficile capire da dove provengano le modifiche a quel valore. Inoltre, cambiare i props può causare il rendering dei componenti. Quindi, mutare i props in un componente potrebbe indurre il componente a essere rerenderizzato, il che a sua volta potrebbe innescare di nuovo la mutazione.

Per risolvere questo problema, possiamo gestire lo stato `done` usando la proprietà `data` di Vue. La proprietà `data` è dove può gestire lo stato locale in un componente, vive all'interno dell'oggetto componente accanto alla proprietà `props` e ha la seguente struttura:

```js
export default {
  // …
  data() {
    return {
      key: value,
    };
  },
  // …
};
```

Noterà che la proprietà `data` è una funzione. Questo per mantenere i valori dei dati unici per ogni istanza di un componente durante il runtime — la funzione viene invocata separatamente per ogni istanza del componente. Se dichiarasse i dati come solo un oggetto, tutte le istanze di quel componente condivideranno gli stessi valori. Questo è un effetto collaterale del modo in cui Vue registra i componenti e qualcosa che non desidera.

Usa `this` per accedere ai props e ad altre proprietà di un componente dall'interno di `data`, proprio come ci si potrebbe aspettare. Vedremo un esempio di questo a breve.

> [!NOTE]
> A causa del modo in cui `this` funziona nelle funzioni freccia (effettuando il binding al contesto del genitore), non sarebbe in grado di accedere a nessuno degli attributi necessari da `data` se usasse una funzione freccia. Quindi non usi una funzione freccia per la proprietà `data`.

Quindi aggiungiamo una proprietà `data` al nostro componente `ToDoItem`. Questo restituirà un oggetto contenente una singola proprietà che chiameremo `isDone`, il cui valore è `this.done`.

Aggiorni l'oggetto componente come segue:

```js
export default {
  props: {
    label: { required: true, type: String },
    done: { default: false, type: Boolean },
  },
  data() {
    return {
      isDone: this.done,
    };
  },
};
```

Vue fa un po' di magia qui — mette in binding tutti i suoi props direttamente all'istanza del componente, quindi non dobbiamo chiamare `this.props.done`. Mette anche in binding altri attributi (`data`, che ha già visto, e altri come `methods`, `computed`, ecc.) direttamente all'istanza. Questo è in parte per renderli disponibili al suo template. Lo svantaggio di questo è che deve mantenere le chiavi uniche tra questi attributi. Questo è il motivo per cui abbiamo chiamato il nostro attributo `data` `isDone` invece di `done`.

Quindi ora dobbiamo collegare la proprietà `isDone` al nostro componente. In modo simile a come Vue utilizza le espressioni `\{{}}` per visualizzare espressioni JavaScript all'interno dei template, Vue ha una sintassi speciale per collegare le espressioni JavaScript agli elementi HTML e ai componenti: **`v-bind`**. L'espressione `v-bind` appare così:

```plain
v-bind:attribute="expression"
```

In altre parole, antepone qualunque attributo/prop si desideri collegare con `v-bind:`. Nella maggior parte dei casi, è possibile utilizzare una sintassi abbreviata per l'attributo `v-bind`, che consiste nel prefissare il proprio attributo/prop con due punti. Quindi `:attributo="espressione"` funziona allo stesso modo di `v-bind:attributo="espressione"`.

Quindi, nel caso della casella di controllo nel nostro componente `ToDoItem`, possiamo usare `v-bind` per mappare la proprietà `isDone` all'attributo `checked` sull'elemento `<input>`. Entrambe le seguenti opzioni sono equivalenti:

```vue
<input type="checkbox" id="todo-item" v-bind:checked="isDone" />

<input type="checkbox" id="todo-item" :checked="isDone" />
```

È libero di utilizzare qualunque modello preferisca. È meglio mantenere la coerenza però. Poiché la sintassi abbreviata è più comunemente usata, questo tutorial seguirà quel modello.

Quindi facciamolo. Aggiorni ora il suo elemento `<input>` per includere `:checked="isDone"`.

Provi il suo componente passando `:done="true"` alla chiamata `ToDoItem` in `App.vue`. Noti che deve usare la sintassi `v-bind`, perché altrimenti `true` viene passato come stringa. La casella di controllo visualizzata dovrebbe essere selezionata.

```vue
<template>
  <div id="app">
    <h1>My To-Do List</h1>
    <ul>
      <li>
        <to-do-item label="My ToDo Item" :done="true"></to-do-item>
      </li>
    </ul>
  </div>
</template>
```

Provi a cambiare `true` in `false` e viceversa, ricaricando la sua app tra un cambio e l'altro per vedere come cambia lo stato.

## Assegnare un id univoco alle cose da fare

Ottimo! Ora abbiamo una casella di controllo funzionante in cui possiamo impostare lo stato programmaticamente. Tuttavia, attualmente possiamo aggiungere solo un componente `ToDoList` alla pagina perché l'`id` è codificato. Questo risulterebbe in errori con la tecnologia assistiva poiché l'`id` è necessario per collegare correttamente le etichette alle loro caselle di controllo. Per risolvere questo problema, possiamo impostare programmaticamente l'`id` nei dati del componente.

Possiamo usare il metodo [`Crypto.randomUUID()`](/it/docs/Web/API/Crypto/randomUUID) per generare una stringa unica che mantiene unici gli `id` dei componenti. `randomUUID()` è integrato nei browser moderni e fornisce un modo semplice per garantire l'unicità senza fare affidamento su librerie esterne.

Successivamente, aggiungi un campo `id` alla proprietà `data` come mostrato di seguito; questo utilizza `crypto.randomUUID()` per restituire una stringa unica, che poi prefissi con `todo-`:

```js
export default {
  props: {
    label: { required: true, type: String },
    done: { default: false, type: Boolean },
  },
  data() {
    return {
      isDone: this.done,
      id: "todo-" + crypto.randomUUID(),
    };
  },
};
```

Successivamente, si colleghi l'`id` sia all'attributo `id` della casella di controllo che all'attributo `for` dell'etichetta, aggiornando gli attributi `id` e `for` esistenti come mostrato:

```vue
<template>
  <div>
    <input type="checkbox" :id="id" :checked="isDone" />
    <label :for="id">\{{ label }}</label>
  </div>
</template>
```

## Sintesi

E questo sarà tutto per questo articolo. A questo punto abbiamo un componente `ToDoItem` ben funzionante che può ricevere un'etichetta da visualizzare, memorizzerà il suo stato selezionato, e sarà renderizzato con un `id` unico ogni volta che viene chiamato. Può verificare che gli `id` unici funzionino aggiungendo temporaneamente più chiamate `<to-do-item></to-do-item>` in `App.vue` e quindi controllando il loro output renderizzato con gli strumenti di sviluppo del suo browser.

Ora siamo pronti per aggiungere più componenti `ToDoItem` alla nostra app. Nel nostro prossimo articolo esamineremo l'aggiunta di un set di dati degli elementi della lista delle cose da fare al nostro componente `App.vue`, che poi cicleremo e visualizzeremo all'interno dei componenti `ToDoItem` usando la direttiva `v-for`.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_getting_started","Learn_web_development/Core/Frameworks_libraries/Vue_rendering_lists", "Learn_web_development/Core/Frameworks_libraries")}}
