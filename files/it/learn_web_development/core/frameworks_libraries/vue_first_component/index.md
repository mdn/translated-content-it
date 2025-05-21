---
title: Creazione del nostro primo componente Vue
slug: Learn_web_development/Core/Frameworks_libraries/Vue_first_component
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_getting_started","Learn_web_development/Core/Frameworks_libraries/Vue_rendering_lists", "Learn_web_development/Core/Frameworks_libraries")}}

È ora di approfondire Vue e creare il nostro componente personalizzato — inizieremo creando un componente per rappresentare ogni elemento nella lista delle cose da fare. Durante il percorso, impareremo alcuni concetti importanti come il richiamare componenti all'interno di altri componenti, passare dati a questi tramite props e salvare lo stato dei dati.

> [!NOTE]
> Se hai bisogno di confrontare il tuo codice con la nostra versione, puoi trovare una versione completa dell'app di esempio Vue nel nostro [repository todo-vue](https://github.com/mdn/todo-vue). Per una versione live in esecuzione, vedi <https://mdn.github.io/todo-vue/>.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
          conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/command line</a
          >.
        </p>
        <p>
          I componenti Vue sono scritti come combinazione di oggetti JavaScript che
          gestiscono i dati dell'app e una sintassi di template basata su HTML che mappa
          la struttura sottostante del DOM. Per l'installazione, e per usare alcune delle
          funzionalità avanzate di Vue (come i Componenti File Singolo o render
          functions), avrai bisogno di un terminale con
          <a
            href="https://nodejs.org/en/download"
            rel="noopener noreferrer"
            target="_blank"
            >Node</a
          >
          e
          <a
            href="https://www.npmjs.com/get-npm"
            rel="noopener noreferrer"
            target="_blank"
            >npm</a
          >
          installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare come creare un componente Vue, renderizzarlo all'interno di un altro componente,
        passare dati al suo interno usando props e salvare il suo stato.
      </td>
    </tr>
  </tbody>
</table>

## Creazione di un componente ToDoItem

Iniziamo creando il nostro primo componente, che visualizzerà un singolo elemento todo. Useremo questo per costruire la nostra lista di todos.

1. Nel tuo directory `moz-todo-vue/src/components`, crea un nuovo file chiamato `ToDoItem.vue`. Apri il file nel tuo editor di codice.
2. Crea la sezione template del componente aggiungendo `<template></template>` nella parte superiore del file.
3. Crea una sezione `<script></script>` sotto la tua sezione template. All'interno dei tag `<script>`, aggiungi un oggetto esportato per default `export default {}`, che è il tuo oggetto componente.

Il tuo file dovrebbe ora avere questo aspetto:

```vue
<template></template>
<script>
export default {};
</script>
```

Possiamo ora iniziare ad aggiungere il contenuto effettivo al nostro `ToDoItem`. I template Vue sono attualmente consentiti solo con un singolo elemento radice — un elemento deve racchiudere tutto all'interno della sezione template (questo cambierà con l'uscita di Vue 3). Useremo un [`<div>`](/it/docs/Web/HTML/Reference/Elements/div) per quell'elemento radice.

1. Aggiungi un `<div>` vuoto all'interno del template del componente ora.
2. All'interno di quel `<div>`, aggiungiamo una casella di controllo e una corrispondente etichetta. Aggiungi un `id` alla casella di controllo e un attributo `for` che mappa la casella all'etichetta, come mostrato di seguito.

   ```vue
   <template>
     <div>
       <input type="checkbox" id="todo-item" />
       <label for="todo-item">My Todo Item</label>
     </div>
   </template>
   ```

### Uso di TodoItem all'interno della nostra app

Tutto ciò va bene, ma non abbiamo ancora aggiunto il componente alla nostra app, quindi non c'è modo di testarlo e vedere se tutto funziona. Aggiungiamolo ora.

1. Riapri `App.vue`.
2. Nella parte superiore del tuo tag `<script>`, aggiungi il seguente codice per importare il tuo componente `ToDoItem`:

   ```js
   import ToDoItem from "./components/ToDoItem.vue";
   ```

3. All'interno del tuo oggetto componente, aggiungi la proprietà `components`, e all'interno di essa aggiungi il tuo componente `ToDoItem` per registrarlo.

I contenuti del tuo `<script>` dovrebbero ora apparire così:

```js
import ToDoItem from "./components/ToDoItem.vue";

export default {
  name: "app",
  components: {
    ToDoItem,
  },
};
```

Questo è lo stesso modo in cui il componente `HelloWorld` è stato registrato inizialmente dalla CLI di Vue.

Per effettivamente renderizzare il componente `ToDoItem` nell'app, devi salire nel tuo elemento `<template>` e chiamarlo come un elemento `<to-do-item></to-do-item>`. Nota che il nome del file del componente e la sua rappresentazione in JavaScript sono in PascalCase (ad esempio, `ToDoList`), e l'elemento personalizzato equivalente è in {{Glossary("kebab_case", "kebab-case")}} (ad esempio, `<to-do-list>`). È necessario utilizzare questo stile di case se stai scrivendo template Vue [direttamente nel DOM](https://vuejs.org/guide/essentials/component-basics.html#dom-template-parsing-caveats).

1. Sotto l'[`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements), crea una lista non ordinata ([`<ul>`](/it/docs/Web/HTML/Reference/Elements/ul)) contenente un singolo elemento di lista ([`<li>`](/it/docs/Web/HTML/Reference/Elements/li)).
2. All'interno dell'elemento della lista aggiungi `<to-do-item></to-do-item>`.

La sezione `<template>` del tuo file `App.vue` dovrebbe ora apparire simile a questa:

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

Se controlli nuovamente la tua app renderizzata, dovresti ora vedere il tuo `ToDoItem` renderizzato, costituito da una casella di controllo e un'etichetta.

![Lo stato di rendering attuale dell'app, che include un titolo della Lista To-Do, e una singola casella di controllo ed etichetta](rendered-todoitem.png)

## Rendere i componenti dinamici con i props

Il nostro componente `ToDoItem` non è ancora molto utile perché possiamo includerlo solo una volta su una pagina (gli IDs devono essere unici), e non abbiamo modo di impostare il testo dell'etichetta. Niente di tutto questo è dinamico.

Quello di cui abbiamo bisogno è un po' di stato del componente. Questo può essere ottenuto aggiungendo dei props al nostro componente. Puoi pensare ai props come simili agli input di una funzione. Il valore di un prop fornisce ai componenti uno stato iniziale che influenza il loro display.

### Registrare i props

In Vue, ci sono due modi per registrare i props:

- Il primo modo è semplicemente elencare i props come un array di stringhe. Ogni voce dell'array corrisponde al nome di un prop.
- Il secondo modo è definire i props come un oggetto, con ogni chiave che corrisponde al nome del prop. Elencare i props come un oggetto ti permette di specificare valori predefiniti, segnare i props come richiesti, eseguire tipizzazione semplice dell'oggetto (specificamente per i tipi primitivi di JavaScript) e eseguire una semplice validazione del prop.

> [!NOTE]
> La validazione dei props avviene solo in modalità di sviluppo, quindi non puoi fare affidamento su di essa in produzione. Inoltre, le funzioni di validazione dei props vengono invocate prima che l'istanza del componente venga creata, quindi non hanno accesso allo stato del componente (o ad altri props).

Per questo componente, useremo il metodo di registrazione dell'oggetto.

1. Torna al tuo file `ToDoItem.vue`.
2. Aggiungi una proprietà `props` all'interno dell'oggetto export `default {}`, che contiene un oggetto vuoto.
3. All'interno di questo oggetto, aggiungi due proprietà con le chiavi `label` e `done`.
4. Il valore della chiave `label` dovrebbe essere un oggetto con 2 proprietà (o **props**, come vengono chiamati nel contesto di essere disponibili ai componenti).

   1. La prima è una proprietà `required`, che avrà un valore di `true`. Questo dirà a Vue che ci aspettiamo che ogni istanza di questo componente abbia un campo etichetta. Vue ci avviserà se un componente `ToDoItem` non ha un campo etichetta.
   2. La seconda proprietà che aggiungeremo è una proprietà `type`. Imposta il valore per questa proprietà come il tipo `String` di JavaScript (nota la "S" maiuscola). Questo dice a Vue che ci aspettiamo che il valore di questa proprietà sia una stringa.

5. Ora passiamo al prop `done`.

   1. Prima aggiungi un campo `default`, con un valore di `false`. Questo significa che quando nessun prop `done` viene passato a un componente `ToDoItem`, il prop `done` avrà un valore di false (ricorda che questo non è richiesto — abbiamo bisogno di `default` solo sui props non richiesti).
   2. Successivamente aggiungi un campo `type` con un valore di `Boolean`. Questo dice a Vue che ci aspettiamo che il valore del prop sia un tipo booleano di JavaScript.

Il tuo oggetto componente dovrebbe ora apparire così:

```js
export default {
  props: {
    label: { required: true, type: String },
    done: { default: false, type: Boolean },
  },
};
```

### Utilizzo dei props registrati

Con questi props definiti all'interno dell'oggetto componente, ora possiamo utilizzare questi valori variabili all'interno del nostro template. Iniziamo aggiungendo il prop `label` al template del componente.

Nel tuo `<template>`, sostituisci il contenuto dell'elemento `<label>` con `\{{label}}`.

`\{{}}` è una sintassi di template speciale in Vue, che ci permette di stampare il risultato delle espressioni JavaScript definite nella nostra classe, all'interno del nostro template, inclusi valori e metodi. È importante sapere che il contenuto all'interno dei `\{{}}` viene visualizzato come testo e non come HTML. In questo caso, stiamo stampando il valore del prop `label`.

La sezione template del tuo componente dovrebbe ora apparire così:

```vue
<template>
  <div>
    <input type="checkbox" id="todo-item" />
    <label for="todo-item">\{{ label }}</label>
  </div>
</template>
```

Ritorna al tuo browser e vedrai l'elemento todo renderizzato come prima, ma senza un'etichetta (oh no!). Vai agli strumenti di sviluppo del tuo browser e vedrai un avviso simile a questo nella console:

```plain
[Vue warn]: Missing required prop: "label"

found in

---> <ToDoItem> at src/components/ToDoItem.vue
        <App> at src/App.vue
          <Root>
```

Questo è perché abbiamo contrassegnato `label` come un prop richiesto, ma non abbiamo mai fornito al componente quel prop — abbiamo definito dove all'interno del template vogliamo che venga usato, ma non l'abbiamo passato al componente quando l'abbiamo chiamato. Risolviamo questo.

All'interno del tuo file `App.vue`, aggiungi un prop `label` al componente `<to-do-item></to-do-item>`, proprio come un attributo HTML normale:

```vue
<to-do-item label="My ToDo Item"></to-do-item>
```

Ora vedrai l'etichetta nella tua app, e l'avviso non verrà più visualizzato nella console.

Quindi questi sono i props in sintesi. Passiamo ora a come Vue persiste lo stato dei dati.

## Oggetto data di Vue

Se cambi il valore del prop `label` passato nella chiamata `<to-do-item></to-do-item>` nel tuo componente `App`, dovresti vederlo aggiornare. Questo è ottimo. Abbiamo una casella di controllo, con un'etichetta aggiornable. Tuttavia, attualmente non stiamo facendo nulla con il prop "done" — possiamo selezionare le caselle di controllo nell'interfaccia utente, ma in nessuna parte dell'app stiamo registrando se un elemento todo è effettivamente completato.

Per ottenere questo, vogliamo associare il prop `done` del componente all'attributo `checked` sull'elemento [`<input>`](/it/docs/Web/HTML/Reference/Elements/input), in modo che possa servire come registro di se la casella di controllo è selezionata o meno. Tuttavia, è importante che i props servano come binding dati a senso unico — un componente non dovrebbe mai alterare il valore dei propri props. Ci sono molte ragioni per questo. In parte, i componenti che modificano i props possono rendere difficile il debugging. Se un valore viene passato a più componenti figli, potrebbe essere difficile tenere traccia di dove provengono le modifiche a quel valore. Inoltre, cambiare i props può causare il rerender dei componenti. Quindi mutare i props in un componente innescherebbe il rerender del componente, il che potrebbe a sua volta innescare nuovamente la mutazione.

Per ovviare a questo, possiamo gestire lo stato `done` utilizzando la proprietà `data` di Vue. La proprietà `data` è il luogo in cui puoi gestire lo stato locale in un componente, vive all'interno dell'oggetto componente insieme alla proprietà `props` e ha la seguente struttura:

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

Noterai che la proprietà `data` è una funzione. Questo per mantenere i valori dei dati unici per ogni istanza di un componente a runtime — la funzione viene invocata separatamente per ogni istanza del componente. Se dichiarassi i dati come solo un oggetto, tutte le istanze di quel componente condividerebbero gli stessi valori. Questo è un effetto collaterale del modo in cui Vue registra i componenti e qualcosa che non vuoi.

Usi `this` per accedere ai props di un componente e ad altre proprietà dall'interno dei dati, come potresti aspettarti. Vedremo un esempio di questo a breve.

> [!NOTE]
> A causa del modo in cui funziona `this` nelle arrow functions (sfruttando il contesto del genitore), non sarei in grado di accedere a nessuno degli attributi necessari dall'interno dei `data` se usassi un arrow function. Quindi non usare un arrow function per la proprietà `data`.

Aggiungiamo quindi una proprietà `data` al nostro componente `ToDoItem`. Questo ritornerà un oggetto contenente una singola proprietà che chiameremo `isDone`, il cui valore è `this.done`.

Aggiorna il componente come segue:

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

Vue fa un po' di magia qui — lega tutti i tuoi props direttamente all'istanza del componente, quindi non dobbiamo chiamare `this.props.done`. Lega anche altri attributi (`data`, che hai già visto, e altri come `methods`, `computed`, ecc.) direttamente all'istanza. Questo serve, in parte, a renderli disponibili nel tuo template. Il lato negativo è che devi mantenere le chiavi uniche tra questi attributi. È per questo che abbiamo chiamato il nostro attributo `data` `isDone` invece di `done`.

Quindi ora dobbiamo collegare la proprietà `isDone` al nostro componente. In modo simile a come Vue usa le espressioni `\{{}}` per visualizzare le espressioni JavaScript all'interno dei template, Vue ha una sintassi speciale per associare le espressioni JavaScript agli elementi HTML e ai componenti: **`v-bind`**. L'espressione `v-bind` appare così:

```plain
v-bind:attribute="expression"
```

In altre parole, precedi qualsiasi attributo/prop che vuoi collegare con `v-bind:`. Nella maggior parte dei casi, puoi usare un'abbreviazione per la proprietà `v-bind`, che consiste semplicemente nel precedere l'attributo/prop con due punti. Quindi `:attributo="espressione"` funziona allo stesso modo di `v-bind:attributo="espressione"`.

Quindi nel caso della casella di controllo nel nostro componente `ToDoItem`, possiamo usare `v-bind` per mappare la proprietà `isDone` all'attributo `checked` sull'elemento `<input>`. Entrambe le seguenti sono equivalenti:

```vue
<input type="checkbox" id="todo-item" v-bind:checked="isDone" />

<input type="checkbox" id="todo-item" :checked="isDone" />
```

Sei libero di usare il modello che preferisci. È meglio tenerlo coerente però. Poiché la sintassi abbreviata è più comunemente utilizzata, questo tutorial rimarrà con quel modello.

Facciamo quindi questo. Aggiorna ora il tuo elemento `<input>` per includere `:checked="isDone"`.

Prova il tuo componente passando `:done="true"` alla chiamata di `ToDoItem` in `App.vue`. Nota che devi usare la sintassi `v-bind`, perché altrimenti `true` è passato come stringa. La casella di controllo visualizzata dovrebbe essere selezionata.

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

Prova a cambiare `true` a `false` e indietro di nuovo, ricaricando la tua app nel frattempo per vedere come lo stato cambia.

## Dare ai Todo un id univoco

Ottimo! Abbiamo ora una casella di controllo funzionante dove possiamo impostare lo stato programmaticamente. Tuttavia, attualmente possiamo aggiungere solo un componente `ToDoList` alla pagina perché l'`id` è hardcoded. Questo risulterebbe in errori con la tecnologia assistiva poiché l'`id` è necessario per mappare correttamente le etichette alle loro caselle di controllo. Per risolvere questo, possiamo impostare programmaticamente l'`id` nei dati del componente.

Possiamo usare il metodo [`Crypto.randomUUID()`](/it/docs/Web/API/Crypto/randomUUID) per generare una stringa univoca per mantenere gli `id` del componente unici. `randomUUID()` è integrato nei browser moderni e fornisce un modo semplice per garantire l'unicità senza fare affidamento su librerie esterne.

Successivamente, aggiungi un campo `id` alla proprietà `data` come mostrato di seguito; questo utilizza `crypto.randomUUID()` per restituire una stringa univoca, che poi prefissiamo con `todo-`:

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

Successivamente, collega l'`id` agli attributi `id` della casella di controllo e `for` dell'etichetta, aggiornando gli attributi esistenti `id` e `for` come mostrato:

```vue
<template>
  <div>
    <input type="checkbox" :id="id" :checked="isDone" />
    <label :for="id">\{{ label }}</label>
  </div>
</template>
```

## Riepilogo

E questo è tutto per questo articolo. A questo punto abbiamo un componente `ToDoItem` che funziona bene e che può ricevere un'etichetta da visualizzare, immagazzinerà il suo stato di check, e sarà renderizzato con un `id` univoco ogni volta che viene chiamato. Puoi controllare se gli `id` univoci funzionano temporaneamente aggiungendo ulteriori chiamate `<to-do-item></to-do-item>` in `App.vue`, e quindi controllando il loro output renderizzato con gli strumenti di sviluppo del tuo browser.

Ora siamo pronti per aggiungere più componenti `ToDoItem` alla nostra app. Nel nostro prossimo articolo esamineremo l'aggiunta di un set di dati di elementi todo al nostro componente `App.vue`, che poi mostreremo all'interno dei componenti `ToDoItem` usando la direttiva `v-for`.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Vue_getting_started","Learn_web_development/Core/Frameworks_libraries/Vue_rendering_lists", "Learn_web_development/Core/Frameworks_libraries")}}
