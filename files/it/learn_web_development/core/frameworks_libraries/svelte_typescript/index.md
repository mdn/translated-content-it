---
title: Supporto TypeScript in Svelte
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript
l10n:
  sourceCommit: 2c0f972d873ea2db5163dbcb12987847124751ad
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_stores","Learn_web_development/Core/Frameworks_libraries/Svelte_deployment_next", "Learn_web_development/Core/Frameworks_libraries")}}

Nell'ultimo articolo abbiamo imparato a conoscere i negozi Svelte e abbiamo persino implementato il nostro negozio personalizzato per conservare le informazioni dell'app nella Memoria Web. Abbiamo anche visto come utilizzare la direttiva di transizione per implementare animazioni sugli elementi DOM in Svelte.

Ora impareremo a utilizzare TypeScript nelle applicazioni Svelte. Prima conosceremo che cos'è TypeScript e quali vantaggi può offrirci. Poi vedremo come configurare il nostro progetto per lavorare con i file TypeScript. Infine, esamineremo la nostra app e vedremo quali modifiche dobbiamo apportare per sfruttare appieno le funzionalità di TypeScript.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È consigliato che sia familiare con i linguaggi
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
          e abbia conoscenze della
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >riga di comando/terminal</a
          >.
        </p>
        <p>
          Avrai bisogno di un terminal con node e npm installati per compilare e costruire
          la tua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare come configurare e utilizzare TypeScript nello sviluppo di applicazioni Svelte.
      </td>
    </tr>
  </tbody>
</table>

Nota che la nostra applicazione è completamente funzionale e renderla compatibile con TypeScript è completamente facoltativo. Ci sono diverse opinioni al riguardo, e in questo capitolo parleremo brevemente dei pro e dei contro dell'utilizzo di TypeScript. Anche se non prevedi di adottarlo, quest'articolo ti sarà utile per permetterti di scoprire cosa ha da offrire e aiutarti a prendere la tua decisione. Se non sei affatto interessato a TypeScript, puoi passare al capitolo successivo, dove esamineremo diverse opzioni per distribuire le nostre applicazioni Svelte, altre risorse e altro ancora.

## Segui il codice con noi

### Git

Clona il repository GitHub (se non l'hai ancora fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi, per ottenere lo stato attuale dell'app, esegui

```bash
cd mdn-svelte-tutorial/07-typescript-support
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/07-typescript-support
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità sviluppo.

### REPL

Sfortunatamente, [il supporto TypeScript non è ancora disponibile nel REPL](https://github.com/sveltejs/svelte.dev/issues/853).

## TypeScript: tipizzazione statica opzionale per JavaScript

[TypeScript](https://www.typescriptlang.org/) è un superset di JavaScript che fornisce funzionalità come la tipizzazione statica opzionale, classi, interfacce e generics. L'obiettivo di TypeScript è aiutare a individuare gli errori precocemente attraverso il suo sistema di tipi e rendere lo sviluppo JavaScript più efficiente. Uno dei grandi benefici è abilitare gli IDE a fornire un ambiente più ricco per individuare errori comuni mentre si scrive il codice.

Il meglio di tutto, il codice JavaScript è codice TypeScript valido; TypeScript è un superset di JavaScript. Puoi rinominare la maggior parte dei tuoi file `.js` in `.ts` e funzioneranno correttamente.

Il nostro codice TypeScript sarà in grado di funzionare ovunque JavaScript possa esser eseguito. Come è possibile? TypeScript "transpila" il nostro codice a JavaScript vanilla. Questo significa che analizza il codice TypeScript e produce il codice JavaScript vanilla equivalente per i browser.

> [!NOTE]
> Se sei curioso di sapere come TypeScript transpila il nostro codice in JavaScript, puoi dare un'occhiata al [TypeScript Playground](https://www.typescriptlang.org/play/?target=1&e=4#example/hello-world).

Il supporto di prima classe per TypeScript è stata la funzionalità più richiesta in Svelte per un po' di tempo. Grazie al duro lavoro del team di Svelte, insieme a molti contributori, hanno un [soluzione ufficiale](https://svelte.dev/blog/svelte-and-typescript) pronta per esser testata. In questa sezione ti mostreremo come impostare un progetto Svelte con supporto TypeScript per provarlo.

## Perché TypeScript?

I principali vantaggi di TypeScript sono:

- Bug individuati precocemente: Il compilatore controlla i tipi al momento della compilazione e fornisce segnalazione degli errori.
- Leggibilità: La tipizzazione statica dà al codice più struttura, rendendolo autodescrittivo e più leggibile.
- Supporto IDE avanzato: Le informazioni sui tipi permettono agli editor di codice e agli IDE di offrire funzionalità come la navigazione del codice, l'autocompletamento e suggerimenti più intelligenti.
- Refactoring più sicuro: I tipi permettono agli IDE di conoscere meglio il tuo codice e assisterti durante il refactoring di grandi parti della tua base di codice.
- Inferenza dei tipi: Ti permette di sfruttare molte funzionalità di TypeScript anche senza dichiarare i tipi delle variabili.
- Disponibilità di nuove funzionalità JavaScript: TypeScript transpila molte funzionalità recenti di JavaScript nel vecchio JavaScript semplice, permettendoti di usarle anche su user-agent che non le supportano ancora nativamente.

TypeScript ha anche alcuni svantaggi:

- Non è una vera tipizzazione statica: I tipi sono controllati solo al momento della compilazione e vengono rimossi dal codice generato.
- Curva di apprendimento ripida: Anche se TypeScript è un superset di JavaScript e non un linguaggio completamente nuovo, c'è una curva di apprendimento considerevole, specialmente se non hai alcuna esperienza con i linguaggi statici come Java o C#.
- Più codice: Devi scrivere e mantenere più codice.
- Non è un sostituto per i test automatici: Anche se i tipi possono aiutare a individuare diversi bug, TypeScript non è un vero sostituto per una suite di test automatizzati completa.
- Codice boilerplate: Lavorare con tipi, classi, interfacce e generici può portare a basi di codice sovra-ingegnerizzate.

Sembra esserci un ampio consenso sul fatto che TypeScript sia particolarmente adatto per progetti su larga scala, con molti sviluppatori che lavorano sullo stesso codice. E infatti viene utilizzato da diversi progetti su larga scala, come Angular 2, Vue 3, Ionic, Visual Studio Code, Jest, e persino il compilatore Svelte. Tuttavia, alcuni sviluppatori preferiscono usarlo anche su piccoli progetti come quello che stiamo sviluppando.

Alla fine, è una tua decisione. Nelle sezioni seguenti speriamo di darti più evidenze per prendere una decisione consapevole.

## Creare un progetto Svelte TypeScript da zero

Puoi iniziare un nuovo progetto Svelte TypeScript utilizzando il [modello standard](https://github.com/sveltejs/template). Tutto ciò che devi fare è eseguire i seguenti comandi nel terminale (eseguili in una cartella dove stai memorizzando i tuoi progetti di test Svelte — crea una nuova directory):

```bash
npx degit sveltejs/template svelte-typescript-app

cd svelte-typescript-app

node scripts/setupTypeScript.js
```

Questa operazione crea un progetto iniziale che include il supporto TypeScript, che puoi quindi modificare secondo le tue necessità.

Quindi dovrai dire a npm di scaricare le dipendenze e avviare il progetto in modalità sviluppo, come facciamo di solito:

```bash
npm install

npm run dev
```

## Aggiungere supporto TypeScript a un progetto Svelte esistente

Per aggiungere il supporto TypeScript a un progetto Svelte esistente, puoi [seguire queste istruzioni](https://svelte.dev/blog/svelte-and-typescript#Adding_TypeScript_to_an_existing_project). In alternativa, puoi scaricare il file [`setupTypeScript.js`](https://github.com/sveltejs/template/blob/master/scripts/setupTypeScript.js) in una cartella `scripts` all'interno della tua cartella radice del progetto, e poi eseguire `node scripts/setupTypeScript.js`.

Puoi persino utilizzare `degit` per scaricare lo script. È quello che faremo per iniziare a portare la nostra applicazione a TypeScript.

> [!NOTE]
> Ricorda che puoi eseguire `npx degit opensas/mdn-svelte-tutorial/07-typescript-support svelte-todo-typescript` per ottenere l'applicazione completa della lista di cose da fare in JavaScript prima di iniziare a portarla su TypeScript.

Vai alla directory radice del progetto e inserisci questi comandi:

```bash
npx degit sveltejs/template/scripts scripts       # download script file to a scripts folder

node scripts/setupTypeScript.js                   # run it
# Converted to TypeScript.
```

Avrai bisogno di rieseguire il gestore delle dipendenze per iniziare.

```bash
npm install                                       # download new dependencies

npm run dev                                       # start the app in development mode
```

Queste istruzioni si applicano a qualsiasi progetto Svelte che vuoi convertire in TypeScript. Tieni conto che la comunità Svelte sta costantemente migliorando il supporto TypeScript, quindi dovresti eseguire `npm update` regolarmente per sfruttare gli ultimi cambiamenti.

> [!NOTE]
> Se incontri difficoltà nel lavorare con TypeScript all'interno di un'applicazione Svelte, dai un'occhiata a questa [sezione di risoluzione dei problemi/FAQ sul supporto TypeScript](https://github.com/sveltejs/language-tools/blob/master/docs/preprocessors/typescript.md#troubleshooting--faq).

Come abbiamo detto prima, TypeScript è un superset di JavaScript, quindi la tua applicazione funzionerà senza modifiche. Attualmente stai eseguendo un'applicazione JavaScript regolare con il supporto TypeScript abilitato, senza sfruttare nessuna delle funzionalità che TypeScript fornisce. Ora puoi iniziare ad aggiungere i tipi in modo progressivo.

Una volta configurato TypeScript, puoi iniziare a usarlo da un componente Svelte semplicemente aggiungendo un `<script lang='ts'>` all'inizio della sezione script. Per usarlo dai file JavaScript regolari, basta cambiare l'estensione del file da `.js` a `.ts`. Dovrai anche aggiornare qualsiasi dichiarazione di importazione corrispondente per rimuovere l'estensione del file `.ts` da tutte le dichiarazioni `import`.

> [!NOTE]
> TypeScript lancerà un errore se usi l'estensione del file `.ts` in una dichiarazione di importazione, quindi se hai un file `./foo.ts`, devi importarlo come "./foo".
> Vedi la sezione [Risoluzione dei moduli per bundle, runtime TypeScript e i loader Node.js](https://www.typescriptlang.org/docs/handbook/modules/theory.html#module-resolution-for-bundlers-typescript-runtimes-and-nodejs-loaders) del manuale di TypeScript per maggiori informazioni.

> [!NOTE]
> L'uso di TypeScript nelle sezioni di markup dei componenti non è supportato in Svelte 4, su cui questa guida si basa.
> Sebbene tu possa utilizzare JavaScript dal markup, dovrai usare TypeScript nella sezione `<script lang='ts'>`.
> TypeScript nel markup dei componenti è consentito da Svelte 5.

## Migliorata esperienza sviluppatore con TypeScript

TypeScript fornisce agli editor di codice e agli IDE molte informazioni per permettere loro di offrire un'esperienza di sviluppo più amichevole.

Useremo [Visual Studio Code](https://code.visualstudio.com/) per fare un rapido test e vedere come possiamo ottenere suggerimenti per l'autocompletamento e controllo dei tipi mentre scriviamo i componenti.

> [!NOTE]
> Se non desideri usare VS Code, forniamo anche istruzioni per usare il controllo degli errori di TypeScript dal terminale più avanti.

C'è un lavoro in corso per supportare TypeScript nei progetti Svelte in diversi editor di codice; il supporto più completo finora è disponibile nell'estensione [Svelte per VS Code](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode), che è sviluppata e mantenuta dal team Svelte. Questa estensione offre controllo dei tipi, ispezione, refactoring, intellisense, informazioni al passaggio del mouse, autocompletamento e altre funzionalità. Questo tipo di assistenza per gli sviluppatori è un altro buon motivo per iniziare a utilizzare TypeScript nei tuoi progetti.

> [!NOTE]
> Assicurati di usare [Svelte per VS Code](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode) e NON il vecchio "Svelte" di James Birtles, che è stato sospeso. Nel caso lo avessi installato, dovresti disinstallarlo e installare l'estensione ufficiale Svelte al suo posto.

Supponendo che tu sia all'interno dell'applicazione VS Code, dalla radice della cartella del tuo progetto, digita `code .` (il punto finale indica a VS Code di aprire la cartella corrente) per aprire l'editor di codice. VS Code ti dirà che ci sono estensioni consigliate da installare.

![Finestra di dialogo che dice che l'ambiente di lavoro ha raccomandazioni di estensione, con opzioni per installare o mostrare un elenco](01-vscode-extension-recommendations.png)

Cliccando su _Installa tutto_ installerà Svelte per VS Code.

![Informazioni sull'estensione Svelte per VS Code](02-svelte-for-vscode.png)

Possiamo anche vedere che il file `setupTypeScript.js` ha apportato alcune modifiche al nostro progetto. Il file `main.js` è stato rinominato in `main.ts`, il che significa che VS Code può fornire informazioni al passaggio del mouse sui nostri componenti Svelte:

![Screenshot di VS Code che mostra che quando si passa il mouse su un componente, si ottengono suggerimenti](03-vscode-hints-in-main-ts.png)

<!-- cSpell:ignore traget -->

Otteniamo anche il controllo dei tipi gratuitamente. Se passiamo una proprietà sconosciuta nel parametro delle opzioni del costruttore `App` (per esempio un errore di battitura come `traget` invece di `target`), TypeScript si lamenterà:

![Controllo dei tipi in VS Code - L'oggetto App è stato fornito con una proprietà sconosciuta traget](04-vscode-type-checking-in-main-ts.png)

Nel componente `App.svelte`, lo script `setupTypeScript.js` ha aggiunto l'attributo `lang="ts"` al tag `<script>`. Inoltre, grazie all'inferenza dei tipi, in molti casi non dovremo nemmeno specificare i tipi per ottenere assistenza nel codice. Per esempio, se inizi ad aggiungere una proprietà `ms` alla chiamata del componente `Alert`, TypeScript dedurrà dal valore predefinito che la proprietà `ms` dovrebbe essere un numero:

![Inferenza dei tipi in VS Code e suggerimenti per il codice - la variabile ms dovrebbe essere un numero](05-vscode-type-inference-and-code-assistance.png)

E se passi qualcosa che non è un numero, si lamenterà:

![Controllo dei tipi in VS Code - la variabile ms è stata assegnata un valore non numerico](06-vscode-type-checking-in-components.png)

Il modello di applicazione ha uno script di `check` configurato che esegue `svelte-check` contro il tuo codice. Questo pacchetto ti permette di rilevare errori e avvisi normalmente visualizzati da un editor di codice dalla riga di comando, il che lo rende piuttosto utile per eseguirlo in una pipeline di integrazione continua (CI). Basta eseguire `npm run check` per controllare il CSS inutilizzato e restituire suggerimenti A11y ed errori di compilazione TypeScript.

In questo caso, se esegui `npm run check` (sia nel terminale VS Code che nel terminale normale) otterrai il seguente errore:

![Il comando Check eseguito all'interno di VS Code mostra un errore di tipo, la variabile ms dovrebbe essere assegnata a un numero](07-vscode-svelte-check.png)

Meglio ancora, se lo esegui dal terminale integrato di VS Code (puoi aprirlo con il collegamento alla tastiera <kbd>Ctrl</kbd> + <kbd>\`</kbd>), cliccando <kbd>Cmd</kbd>/<kbd>Ctrl</kbd> sul nome del file ti porterà alla linea contenente l'errore.

Puoi anche eseguire lo script `check` in modalità di osservazione con `npm run check -- --watch`. In questo caso, lo script verrà eseguito ogni volta che modifichi un qualsiasi file. Se stai eseguendo questo nel tuo terminale normale, tienilo in esecuzione in una finestra del terminale separata in modo che possa continuare a segnalare errori, ma non interferirà con l'uso del terminale per altre cose.

## Creare un tipo personalizzato

TypeScript supporta i tipi strutturali. Il tipo strutturale è un modo di relazionare i tipi basato unicamente sui loro membri, anche se non definisci esplicitamente il tipo.

Definiremo un tipo `TodoType` per vedere come TypeScript imporrà che tutto ciò che passa a un componente che si aspetta un `TodoType` sarà strutturalmente compatibile con esso.

1. All'interno della cartella `src` crea una cartella `types`.
2. Aggiungi un file `todo.type.ts` al suo interno.
3. Dai al file `todo.type.ts` il seguente contenuto:

   ```ts
   export type TodoType = {
     id: number;
     name: string;
     completed: boolean;
   };
   ```

   > [!NOTE]
   > Il modello Svelte usa [svelte-preprocess](https://github.com/sveltejs/svelte-preprocess) 4.0.0 per supportare TypeScript. Da quella versione in poi devi usare la sintassi `export`/`import` type per importare i tipi e le interfacce. Consulta [questa sezione della guida alla risoluzione dei problemi](https://github.com/sveltejs/language-tools/blob/master/docs/preprocessors/typescript.md#how-do-i-import-interfaces-into-my-svelte-components-i-get-errors-after-transpilation) per ulteriori informazioni.

4. Ora useremo `TodoType` dal nostro componente `Todo.svelte`. Prima aggiungi il `lang="ts"` al tag `<script>` del nostro componente.
5. Importa il tipo e usalo per dichiarare la proprietà `todo`. Sostituisci la linea `export let todo` con la seguente:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todo: TodoType;
   ```

   Nota che l'estensione del file `.ts` non è consentita nella dichiarazione di importazione, e quindi è stata omessa.

6. Ora da `Todos.svelte` istanzieremo un componente `Todo` con un oggetto letterale come suo parametro prima della chiamata al componente `MoreActions`, in questo modo:

   ```svelte
   <hr />

   <Todo todo={ { name: 'a new task with no id!', completed: false } } />

   <!-- MoreActions -->
   <MoreActions {todos}
   ```

7. Aggiungi `lang='ts'` al tag `<script>` del componente `Todos.svelte` in modo che sappia utilizzareil controllo dei tipi che abbiamo specificato.

   Otterremo il seguente errore:

   ![Errore di tipo in VS Code, l'oggetto di tipo Todo richiede una proprietà id.](08-vscode-structural-typing.png)

A questo punto dovresti avere un'idea del tipo di assistenza che possiamo ottenere da TypeScript quando costruiamo progetti in Svelte.

Ora annulleremo queste modifiche per iniziare a portare la nostra applicazione a TypeScript, così non saremo disturbati da tutti gli avvisi del controllo.

1. Rimuovi l'attività errata e l'attributo `lang='ts'` dal file `Todos.svelte`.
2. Rimuovi anche l'importazione di `TodoType` e `lang='ts'` da `Todo.svelte`.

Ci occuperemo di loro più avanti.

## Portare la nostra app di lista delle cose da fare su TypeScript

Ora siamo pronti per iniziare a portare la nostra applicazione di lista delle cose da fare per sfruttare tutte le funzionalità che TypeScript ci offre.

Iniziamo facendo eseguire lo script di controllo in modalità watch all'interno della radice del progetto:

```bash
npm run check -- --watch
```

Questo dovrebbe produrre qualcosa di simile al seguente:

```bash
svelte-check "--watch"

Loading svelte-check in workspace: ./svelte-todo-typescript
Getting Svelte diagnostics...
====================================
svelte-check found no errors and no warnings
```

Nota che se stai usando un editor di codice supportato come VS Code, un modo semplice per iniziare a portare un componente Svelte è semplicemente aggiungere `<script lang='ts'>` all'inizio del tuo componente e cercare gli avvisi con i tre puntini:

![Screenshot in VS Code che mostra che quando aggiungi type="ts" a un componente, ottieni tre puntini di avviso](09-vscode-alert-hints.png)

### Alert.svelte

Iniziamo con il nostro componente `Alert.svelte`.

1. Aggiungi `lang="ts"` nel tag `<script>` del tuo componente `Alert.svelte`. Vedrai alcuni avvertimenti nell'output dello script `check`:

   ```bash
   npm run check -- --watch
   ```

   ```plain
   > svelte-check "--watch"

   ./svelte-todo-typescript
   Getting Svelte diagnostics...
   ====================================

   ./svelte-todo-typescript/src/components/Alert.svelte:8:7
   Warn: Variable 'visible' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     let visible

   ./svelte-todo-typescript/src/components/Alert.svelte:9:7
   Warn: Variable 'timeout' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     let timeout

   ./svelte-todo-typescript/src/components/Alert.svelte:11:28
   Warn: Parameter 'message' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
   Change = (message, ms) => {

   ./svelte-todo-typescript/src/components/Alert.svelte:11:37
   Warn: Parameter 'ms' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
   (message, ms) => {
   ```

2. Puoi risolverli specificando i tipi corrispondenti, come segue:

   ```ts
   export let ms = 3000;

   let visible: boolean;
   let timeout: number;

   const onMessageChange = (message: string, ms: number) => {
     clearTimeout(timeout);
     if (!message) {
       // hide Alert if message is empty
       // …
     }
     // …
   };
   ```

   > [!NOTE]
   > Non c'è bisogno di specificare il tipo `ms` con `export let ms:number = 3000`, perché TypeScript lo sta già deducendo dal suo valore predefinito.

### MoreActions.svelte

Ora faremo lo stesso per il componente `MoreActions.svelte`.

1. Aggiungi l'attributo `lang='ts'`, come prima. TypeScript ci avvertirà della prop `todos` e della variabile `t` nella chiamata a `todos.filter((t) =>...)`.

   ```plain
   Warn: Variable 'todos' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     export let todos

   Warn: Parameter 't' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     $: completedTodos = todos.filter((t) => t.completed).length
   ```

2. Useremo il `TodoType` che abbiamo già definito per dire a TypeScript che `todos` è un array `TodoType`. Sostituisci la riga `export let todos` con la seguente:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todos: TodoType[];
   ```

Notare che ora TypeScript può inferire che la variabile `t` in `todos.filter((t) => t.completed)` è di tipo `TodoType`. Tuttavia, se pensiamo che il nostro codice sia più facile da leggere, potremmo specificarlo come segue:

```ts
$: completedTodos = todos.filter((t: TodoType) => t.completed).length;
```

La maggior parte delle volte, TypeScript sarà in grado di dedurre correttamente il tipo della variabile reattiva, ma a volte potresti ottenere un errore "Ha implicitamente un tipo 'any'" quando lavori con assegnazioni reattive. In quei casi puoi dichiarare la variabile tipizzata in un'istruzione diversa, come segue:

```ts
let completedTodos: number;
$: completedTodos = todos.filter((t: TodoType) => t.completed).length;
```

Non puoi specificare il tipo nell'assegnazione reattiva stessa. L'istruzione `$: completedTodos: number = todos.filter[...]` non è valida. Per maggiori informazioni, leggi [Come tipizzo le assegnazioni reattive? / Ricevo un errore "Ha implicitamente un tipo 'any'"](https://github.com/sveltejs/language-tools/blob/master/docs/preprocessors/typescript.md#how-do-i-type-reactive-assignments--i-get-an-implicitly-has-type-any-error).

### FilterButton.svelte

Ora ci occuperemo del componente `FilterButton`.

1. Aggiungi l'attributo `lang='ts'` al tag `<script>`, come al solito. Noterai che non ci sono avvisi: TypeScript deduce il tipo della variabile filtro dal valore predefinito. Ma sappiamo che ci sono solo tre valori validi per il filtro: tutti, attivi e completati. Così possiamo farlo sapere a TypeScript creando un enum Filter.
2. Crea un file `filter.enum.ts` nella cartella `types`.
3. Dagli il seguente contenuto:

   ```ts
   export enum Filter {
     ALL = "all",
     ACTIVE = "active",
     COMPLETED = "completed",
   }
   ```

4. Ora useremo questo dal componente `FilterButton`. Sostituisci il contenuto del file `FilterButton.svelte` con il seguente:

   ```svelte
   <!-- components/FilterButton.svelte -->
   <script lang="ts">
     import { Filter } from "../types/filter.enum";

     export let filter: Filter = Filter.ALL;
   </script>

   <div class="filters btn-group stack-exception">
     <button class="btn toggle-btn" class:btn__primary={filter === Filter.ALL} aria-pressed={filter === Filter.ALL} on:click={()=> filter = Filter.ALL} >
       <span class="visually-hidden">Show</span>
       <span>All</span>
       <span class="visually-hidden">tasks</span>
     </button>
     <button class="btn toggle-btn" class:btn__primary={filter === Filter.ACTIVE} aria-pressed={filter === Filter.ACTIVE} on:click={()=> filter = Filter.ACTIVE} >
       <span class="visually-hidden">Show</span>
       <span>Active</span>
       <span class="visually-hidden">tasks</span>
     </button>
     <button class="btn toggle-btn" class:btn__primary={filter === Filter.COMPLETED} aria-pressed={filter === Filter.COMPLETED} on:click={()=> filter = Filter.COMPLETED} >
       <span class="visually-hidden">Show</span>
       <span>Completed</span>
       <span class="visually-hidden">tasks</span>
     </button>
   </div>
   ```

Qui stiamo semplicemente importando l'enumerazione `Filter` e usandola invece dei valori stringa che abbiamo usato in precedenza.

### Todos.svelte

Useremo anche l'enumerazione `Filter` nel componente `Todos.svelte`.

1. Innanzitutto, aggiungi l'attributo `lang='ts'`, come prima.
2. Successivamente, importa l'enumerazione `Filter`. Aggiungi la seguente istruzione `import` sotto quelle esistenti:

   ```js
   import { Filter } from "../types/filter.enum";
   ```

3. Ora lo useremo ogni volta che facciamo riferimento al filtro corrente. Sostituisci i tuoi due blocchi relativi al filtro con i seguenti:

   ```ts
   let filter: Filter = Filter.ALL;
   const filterTodos = (filter: Filter, todos) =>
     filter === Filter.ACTIVE
       ? todos.filter((t) => !t.completed)
       : filter === Filter.COMPLETED
         ? todos.filter((t) => t.completed)
         : todos;

   $: {
     if (filter === Filter.ALL) {
       $alert = "Browsing all todos";
     } else if (filter === Filter.ACTIVE) {
       $alert = "Browsing active todos";
     } else if (filter === Filter.COMPLETED) {
       $alert = "Browsing completed todos";
     }
   }
   ```

4. `check` ci fornirà ancora alcuni avvisi da `Todos.svelte`. Sistemiamoli.

   Inizia importando il `TodoType` e dicendo a TypeScript che la nostra variabile `todos` è un array di `TodoType`. Sostituisci `export let todos = []` con le seguenti due righe:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todos: TodoType[] = [];
   ```

5. Successivamente specificheremo tutti i tipi mancanti. La variabile `todosStatus`, che abbiamo usato per accedere in modo programmatico ai metodi esposti dal componente `TodosStatus`, è del tipo `TodosStatus`. E ogni `todo` sarà di tipo `TodoType`.

   Aggiorna la tua sezione `<script>` in modo che appaia come segue:

   ```ts
   import FilterButton from "./FilterButton.svelte";
   import Todo from "./Todo.svelte";
   import MoreActions from "./MoreActions.svelte";
   import NewTodo from "./NewTodo.svelte";
   import type TodosStatus from "./TodosStatus.svelte";
   import { alert } from "../stores";

   import { Filter } from "../types/filter.enum";

   import type { TodoType } from "../types/todo.type";

   export let todos: TodoType[] = [];

   let todosStatus: TodosStatus; // reference to TodosStatus instance

   $: newTodoId =
     todos.length > 0 ? Math.max(...todos.map((t) => t.id)) + 1 : 1;

   function addTodo(name: string) {
     todos = [...todos, { id: newTodoId, name, completed: false }];
     $alert = `Todo '${name}' has been added`;
   }

   function removeTodo(todo: TodoType) {
     todos = todos.filter((t) => t.id !== todo.id);
     todosStatus.focus(); // give focus to status heading
     $alert = `Todo '${todo.name}' has been deleted`;
   }

   function updateTodo(todo: TodoType) {
     const i = todos.findIndex((t) => t.id === todo.id);
     if (todos[i].name !== todo.name)
       $alert = `todo '${todos[i].name}' has been renamed to '${todo.name}'`;
     if (todos[i].completed !== todo.completed)
       $alert = `todo '${todos[i].name}' marked as ${
         todo.completed ? "completed" : "active"
       }`;
     todos[i] = { ...todos[i], ...todo };
   }

   let filter: Filter = Filter.ALL;
   const filterTodos = (filter: Filter, todos: TodoType[]) =>
     filter === Filter.ACTIVE
       ? todos.filter((t) => !t.completed)
       : filter === Filter.COMPLETED
         ? todos.filter((t) => t.completed)
         : todos;

   $: {
     if (filter === Filter.ALL) {
       $alert = "Browsing all todos";
     } else if (filter === Filter.ACTIVE) {
       $alert = "Browsing active todos";
     } else if (filter === Filter.COMPLETED) {
       $alert = "Browsing completed todos";
     }
   }

   const checkAllTodos = (completed: boolean) => {
     todos = todos.map((t) => ({ ...t, completed }));
     $alert = `${completed ? "Checked" : "Unchecked"} ${todos.length} todos`;
   };
   const removeCompletedTodos = () => {
     $alert = `Removed ${todos.filter((t) => t.completed).length} todos`;
     todos = todos.filter((t) => !t.completed);
   };
   ```

### TodosStatus.svelte

Stiamo incontrando i seguenti errori relativi al passaggio di `todos` ai componenti `TodosStatus.svelte` (e `Todo.svelte`):

```plain
./src/components/Todos.svelte:70:39
Error: Type 'TodoType[]' is not assignable to type 'undefined'. (ts)
  <TodosStatus bind:this={todosStatus} {todos} />

./src/components/Todos.svelte:76:12
Error: Type 'TodoType' is not assignable to type 'undefined'. (ts)
     <Todo {todo}
```

Questo perché la prop `todos` nel componente `TodosStatus` non ha valore predefinito, quindi TypeScript l'ha dedotto come di tipo `undefined`, che non è compatibile con un array di `TodoType`. La stessa cosa sta accadendo con il nostro componente Todo.

Risolveremo questo problema.

1. Apri il file `TodosStatus.svelte` e aggiungi l'attributo `lang='ts'`.
2. Quindi importa il `TodoType` e dichiara la proprietà `todos` come un array di `TodoType`. Sostituisci la prima riga della sezione `<script>` con la seguente:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todos: TodoType[];
   ```

3. Specificheremo anche il `headingEl`, che abbiamo usato per collegarlo al tag di intestazione, come un `HTMLElement`. Aggiorna la riga `let headingEl` come segue:

   ```ts
   let headingEl: HTMLElement;
   ```

4. Infine, noterai l'errore segnalato relativo al luogo in cui impostiamo l'attributo `tabindex`. Questo perché TypeScript sta controllando il tipo dell'elemento `<h2>` e si aspetta che `tabindex` sia di tipo `number`.

   ![Suggerimento tabindex all'interno di VS Code, tabindex si aspetta un tipo di numero, non stringa](10-vscode-tabindex-hint.png)

   Per risolverlo, sostituisci `tabindex="-1"` con `tabindex={-1}`, come questo:

   ```svelte
   <h2 id="list-heading" bind:this={headingEl} tabindex={-1}>
     {completedTodos} out of {totalTodos} items completed
   </h2>
   ```

   In questo modo TypeScript può impedirci di assegnarlo in modo incorretto a una variabile di tipo stringa.

### NewTodo.svelte

Ora ci occuperemo del file `NewTodo.svelte`.

1. Come al solito, aggiungi l'attributo `lang='ts'`.
2. L'avviso indicherà che dobbiamo specificare un tipo per la variabile `nameEl`. Impostane il tipo a `HTMLElement`, come questo:

   ```ts
   let nameEl: HTMLElement; // reference to the name input DOM node
   ```

3. Infine per questo file, dobbiamo specificare il tipo corretto per la nostra variabile `autofocus`. Aggiorna la sua definizione come questa:

   ```ts
   export let autofocus: boolean = false;
   ```

### Todo.svelte

Ora gli unici avvisi che `npm run check` emette sono generati dalla chiamata al componente `Todo.svelte`. Sistemiamoli.

1. Apri il file `Todo.svelte` e aggiungi l'attributo `lang='ts'`.
2. Importiamo il `TodoType` e impostiamo il tipo della prop `todo`. Sostituisci la riga `export let todo` con la seguente:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todo: TodoType;
   ```

3. Il primo avviso che riceviamo è TypeScript che ci dice di definire il tipo della variabile `updatedTodo` nella funzione `update()`. Questo può essere un po' complicato perché `updatedTodo` contiene solo gli attributi del `todo` che sono stati aggiornati. Questo significa che non è un `todo` completo: ha solo un sottoinsieme delle proprietà di un `todo`.

   Per questi tipi di casi, TypeScript fornisce diversi [tipi di utilità](https://www.typescriptlang.org/docs/handbook/utility-types.html) per rendere più facile applicare queste trasformazioni comuni. Quello di cui abbiamo bisogno in questo momento è l'utility [`Partial<T>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialt), che ci permette di rappresentare tutti i sottoinsiemi di un dato tipo. L'utility parziale restituisce un nuovo tipo basato sul tipo `T`, dove ogni proprietà di `T` è opzionale.

   Lo useremo nella funzione `update()`: aggiornala così:

   ```ts
   function update(updatedTodo: Partial<TodoType>) {
     todo = { ...todo, ...updatedTodo }; // applies modifications to todo
     dispatch("update", todo); // emit update event
   }
   ```

   Con questo stiamo dicendo a TypeScript che la variabile `updatedTodo` conterrà un sottoinsieme delle proprietà `TodoType`.

4. Ora svelte-check ci dice che dobbiamo definire il tipo dei parametri della funzione di azione:

   ```bash
   ./07-next-steps/src/components/Todo.svelte:45:24
   Warn: Parameter 'node' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     const focusOnInit = (node) => node && typeof node.focus === 'function' && node.focus()

   ./07-next-steps/src/components/Todo.svelte:47:28
   Warn: Parameter 'node' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     const focusEditButton = (node) => editButtonPressed && node.focus()
   ```

   Basta definire la variabile nodo come di tipo `HTMLElement`. Nelle due righe indicate sopra, sostituire la prima istanza di `node` con `node: HTMLElement`.

### actions.js

Ora ci occuperemo del file `actions.js`.

1. Rinominalo in `actions.ts` e aggiungi il tipo del parametro nodo. Dovrebbe apparire così:

   ```ts
   // actions.ts
   export function selectOnFocus(node: HTMLInputElement) {
     if (node && typeof node.select === "function") {
       // make sure node is defined and has a select() method
       const onFocus = () => node.select(); // event handler
       node.addEventListener("focus", onFocus); // when node gets focus call onFocus()
       return {
         destroy: () => node.removeEventListener("focus", onFocus), // this will be executed when the node is removed from the DOM
       };
     }
   }
   ```

2. Ora aggiorna `Todo.svelte` e `NewTodo.svelte` dove importiamo il file delle azioni. Ricorda che gli import in TypeScript non includono l'estensione del file. In entrambi i casi dovrebbe risultare così:

   ```js
   import { selectOnFocus } from "../actions";
   ```

### Migrazione dei negozi a TypeScript

Ora dobbiamo migrare i file `stores.js` e `localStore.js` su TypeScript.

Suggerimento: lo script `npm run check`, che utilizza lo strumento [`svelte-check`](https://github.com/sveltejs/language-tools/tree/master/packages/svelte-check), controllerà solo i file `.svelte` della nostra applicazione. Se vuoi controllare anche i file `.ts`, puoi eseguire `npm run check && npx tsc --noEmit`, che dice al compilatore TypeScript di controllare per gli errori senza generare i file di output `.js`. Potresti persino aggiungere uno script al tuo file `package.json` che esegue quel comando.

Iniziamo con `stores.js`.

1. Rinomina il file in `stores.ts`.
2. Imposta il tipo del nostro array `initialTodos` a `TodoType[]`. Questo è come dovrà apparire il contenuto:

   ```ts
   // stores.ts
   import { writable } from "svelte/store";
   import { localStore } from "./localStore.js";
   import type { TodoType } from "./types/todo.type";

   export const alert = writable("Welcome to the To-Do list app!");

   const initialTodos: TodoType[] = [
     { id: 1, name: "Visit MDN web docs", completed: true },
     { id: 2, name: "Complete the Svelte Tutorial", completed: false },
   ];

   export const todos = localStore("mdn-svelte-todo", initialTodos);
   ```

3. Ricorda di aggiornare le dichiarazioni di importazione in `App.svelte`, `Alert.svelte`, e `Todos.svelte`. Basta rimuovere l'estensione `.js`, come questo:

   ```js
   import { todos } from "../stores";
   ```

Ora su `localStore.js`.

Aggiorna la dichiarazione di importazione in `stores.ts` come segue:

```js
import { localStore } from "./localStore";
```

1. Inizia rinominando il file in `localStore.ts`.
2. TypeScript ci sta dicendo di specificare il tipo delle variabili `key`, `initial`, e `value`. La prima è facile: la chiave della nostra memoria web locale dovrebbe essere una stringa.

   Ma `initial` e `value` dovrebbero essere qualsiasi oggetto che potrebbe essere convertito in una stringa JSON valida con il metodo [`JSON.stringify`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify), significando qualsiasi oggetto JavaScript con un paio di limitazioni: per esempio, `undefined`, funzioni, e simboli non sono valori JSON validi.

   Così creeremo il tipo `JsonValue` per specificare queste condizioni.

   Crea il file `json.type.ts` nella cartella `types`.

3. Dagli il seguente contenuto:

   ```ts
   export type JsonValue =
     | string
     | number
     | boolean
     | null
     | JsonValue[]
     | { [key: string]: JsonValue };
   ```

   L'operatore `|` ci permette di dichiarare variabili che potrebbero memorizzare valori di due o più tipi. Un `JsonValue` potrebbe essere una stringa, un numero, un booleano, e così via. In questo caso stiamo anche utilizzando tipi ricorsivi per specificare che un `JsonValue` può avere un array di `JsonValue` e anche un oggetto con proprietà di tipo `JsonValue`.

4. Importeremo il nostro tipo `JsonValue` e lo useremo di conseguenza. Aggiorna il tuo file `localStore.ts` come questo:

   ```ts
   // localStore.ts
   import { writable } from "svelte/store";

   import type { JsonValue } from "./types/json.type";

   export const localStore = (key: string, initial: JsonValue) => {
     // receives the key of the local storage and an initial value

     const toString = (value: JsonValue) => JSON.stringify(value, null, 2); // helper function
     const toObj = JSON.parse; // helper function

     if (localStorage.getItem(key) === null) {
       // item not present in local storage
       localStorage.setItem(key, toString(initial)); // initialize local storage with initial value
     }

     const saved = toObj(localStorage.getItem(key)); // convert to object

     const { subscribe, set, update } = writable(saved); // create the underlying writable store

     return {
       subscribe,
       set(value: JsonValue) {
         localStorage.setItem(key, toString(value)); // save also to local storage as a string
         return set(value);
       },
       update,
     };
   };
   ```

Ora se proviamo a creare un `localStore` con qualcosa che non può essere convertito in JSON tramite `JSON.stringify()`, per esempio un oggetto con una funzione come una proprietà, VS Code/`validate` si lamenterà:

![VS Code mostra un errore con l'utilizzo del nostro store — non riesce durante il tentativo di impostare un valore della memoria locale in qualcosa di incompatibile con JSON stringify](11-vscode-invalid-store.png)

E il meglio di tutto, funzionerà anche con la [sintassi di abbonamento automatico `$store`](https://svelte.dev/docs/svelte-components#script-4-prefix-stores-with-$-to-access-their-values). Se proviamo a salvare un valore non valido nel nostro negozio `todos` usando la sintassi `$store`, come questo:

```svelte
<!-- App.svelte -->
<script lang="ts">
  import Todos from "./components/Todos.svelte";
  import Alert from "./components/Alert.svelte";

  import { todos } from "./stores";

  // this is invalid, the content cannot be converted to JSON using JSON.stringify
  $todos = { handler: () => {} };
</script>
```

Lo script di verifica segnalerà il seguente errore:

```bash
> npm run check

Getting Svelte diagnostics...
====================================

./svelte-todo-typescript/src/App.svelte:8:12
Error: Argument of type '{ handler: () => void; }' is not assignable to parameter of type 'JsonValue'.
  Types of property 'handler' are incompatible.
    Type '() => void' is not assignable to type 'JsonValue'.
      Type '() => void' is not assignable to type '{ [key: string]: JsonValue; }'.
        Index signature is missing in type '() => void'. (ts)
 $todos = { handler: () => {} }
```

Questo è un altro esempio di come specificare i tipi può rendere il nostro codice più robusto e aiutarci a catturare più bug prima che arrivino in produzione.

E questo è tutto. Abbiamo convertito tutta la nostra applicazione per utilizzare TypeScript.

## Bloccare i nostri negozi con i Generics

I nostri negozi sono già stati portati su TypeScript, ma possiamo fare di meglio. Non dovremmo aver bisogno di memorizzare qualsiasi tipo di valore: sappiamo che il negozio degli avvisi dovrebbe contenere messaggi stringa, e il negozio delle cose da fare dovrebbe contenere un array di `TodoType`, ecc. Possiamo far applicare questo da TypeScript usando [TypeScript Generics](https://www.typescriptlang.org/docs/handbook/generics.html). Scopriamone di più.

### Comprendere i generics di TypeScript

I generics ti permettono di creare componenti di codice riutilizzabili che funzionano con una varietà di tipi invece che con un singolo tipo. Possono essere applicati a interfacce, classi e funzioni. I tipi generici sono passati come parametri usando una sintassi speciale: sono specificati tra parentesi angolari e sono convenzionalmente denotati con una singola lettera maiuscola. I tipi generici ti permettono di acquisire i tipi forniti dall'utente, assicurandosi che siano disponibili per l'elaborazione futura.

Vediamo un esempio rapido, una semplice classe `Stack` che ci permette di `push` e `pop` elementi, come questo:

```ts
export class Stack {
  private elements = [];

  push = (element) => this.elements.push(element);

  pop() {
    if (this.elements.length === 0) throw new Error("The stack is empty!");
    return this.elements.pop();
  }
}
```

In questo caso, `elements` è un array di tipo `any`, e di conseguenza i metodi `push()` e `pop()` ricevono e restituiscono una variabile di tipo `any`. Quindi è perfettamente valido fare qualcosa del genere:

```js
const anyStack = new Stack();

anyStack.push(1);
anyStack.push("hello");
```

Ma cosa succede se volessimo avere uno `Stack` che funziona solo con il tipo `string`? Potremmo fare quanto segue:

```ts
export class StringStack {
  private elements: string[] = [];

  push = (element: string) => this.elements.push(element);

  pop(): string {
    if (this.elements.length === 0) throw new Error("The stack is empty!");
    return this.elements.pop();
  }
}
```

Questo funzionerebbe. Ma se volessimo lavorare con numeri, dovremmo poi duplicare il nostro codice e creare una classe `NumberStack`. E come potremmo gestire uno stack di tipi che non conosciamo ancora, e che dovrebbe essere definito dal consumatore?

Per risolvere tutti questi problemi, possiamo usare i generics.

Questa è la nostra classe `Stack` reimplementata usando i generics:

```ts
export class Stack<T> {
  private elements: T[] = [];

  push = (element: T): number => this.elements.push(element);

  pop(): T {
    if (this.elements.length === 0) throw new Error("The stack is empty!");
    return this.elements.pop();
  }
}
```

Definiamo un tipo generico `T` e poi lo usiamo come faremmo normalmente con un tipo specifico. Ora `elements` è un array di tipo `T`, e `push()` e `pop()` ricevono e restituiscono una variabile di tipo `T`.

Ecco come useremmo il nostro `Stack` generico:

```ts
const numberStack = new Stack<number>();
numberStack.push(1);
```

Ora TypeScript sa che il nostro stack può accettare solo numeri, e genererà un errore se proviamo a inserire altro:

![Il tipo dell'argomento "hello" non è assegnabile al parametro di tipo "number"](12-vscode-generic-stack-error.png)

TypeScript può anche dedurre tipi generici attraverso il suo utilizzo. I generics supportano anche valori predefiniti e vincoli.

I generics sono una funzionalità potente che ci permette di astrarre il nostro codice dai tipi specifici utilizzati, rendendolo più riutilizzabile e generico senza rinunciare alla sicurezza del tipo. Per saperne di più, consulta l'[Introduzione ai Generics di TypeScript](https://www.typescriptlang.org/docs/handbook/generics.html).

### Usare i negozi Svelte con i generics

I negozi Svelte supportano i generics di default. E, grazie all'inferenza del tipo generico, possiamo sfruttarlo senza nemmeno toccare il nostro codice.

Se apri il file `Todos.svelte` e assegni un tipo `number` al nostro negozio `$alert`, otterrai il seguente errore:

![Argument of type 9999 is not assignable to parameter of type string](13-vscode-generic-alert-error.png)

Questo perché quando abbiamo definito il nostro negozio avvisi nel file `stores.ts` con:

```js
export const alert = writable("Welcome to the To-Do list app!");
```

TypeScript ha dedotto il tipo generico come `string`. Se volessimo essere espliciti al riguardo, potremmo fare quanto segue:

```ts
export const alert = writable<string>("Welcome to the To-Do list app!");
```

Ora faremo in modo che il nostro negozio `localStore` supporti i generics. Ricorda che abbiamo definito il tipo `JsonValue` per prevenire l'uso del nostro negozio `localStore` con valori che non possono essere persistiti usando `JSON.stringify()`. Ora vogliamo che i consumatori di `localStore` possano specificare il tipo di dati da salvare, ma invece di lavorare con qualsiasi tipo, dovrebbero rispettare il tipo `JsonValue`. Specificheremo questo con un vincolo generico, come questo:

```ts
export const localStore = <T extends JsonValue>(key: string, initial: T) => {
  // …
};
```

Definiamo un tipo generico `T` e specifichiamo che deve essere compatibile con il tipo `JsonValue`. Poi useremo il tipo `T` di conseguenza.

Il nostro file `localStore.ts` alla fine dovrebbe apparire così — prova ora nel tuo file nuovo:

```ts
// localStore.ts
import { writable } from "svelte/store";

import type { JsonValue } from "./types/json.type";

export const localStore = <T extends JsonValue>(key: string, initial: T) => {
  // receives the key of the local storage and an initial value

  const toString = (value: T) => JSON.stringify(value, null, 2); // helper function
  const toObj = JSON.parse; // helper function

  if (localStorage.getItem(key) === null) {
    // item not present in local storage
    localStorage.setItem(key, toString(initial)); // initialize local storage with initial value
  }

  const saved = toObj(localStorage.getItem(key)); // convert to object

  const { subscribe, set, update } = writable<T>(saved); // create the underlying writable store

  return {
    subscribe,
    set(value: T) {
      localStorage.setItem(key, toString(value)); // save also to local storage as a string
      return set(value);
    },
    update,
  };
};
```

E grazie all'inferenza del tipo generico, TypeScript già sa che il nostro negozio `$todos` dovrebbe contenere un array di `TodoType`:

![L'oggetto Todo Type proprietà complete dovrebbe essere completed](14-vscode-generic-localstore-error.png)

Ancora una volta, se volessimo essere espliciti al riguardo, potremmo farlo nel file `stores.ts` come questo:

```ts
const initialTodos: TodoType[] = [
  { id: 1, name: "Visit MDN web docs", completed: true },
  { id: 2, name: "Complete the Svelte Tutorial", completed: false },
];

export const todos = localStore<TodoType[]>("mdn-svelte-todo", initialTodos);
```

Questo è tutto per il nostro breve tour dei Generics di TypeScript.

## Il codice finora

### Git

Per vedere lo stato del codice come dovrebbe essere alla fine di questo articolo, accedi alla tua copia del nostro repo in questo modo:

```bash
cd mdn-svelte-tutorial/08-next-steps
```

O scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/08-next-steps
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità sviluppo.

### REPL

Come abbiamo detto in precedenza, TypeScript non è ancora disponibile nel REPL.

## Riassunto

In questo articolo abbiamo preso la nostra applicazione di lista delle cose da fare e l'abbiamo portata su TypeScript.

Abbiamo prima appreso cosa è TypeScript e quali vantaggi può portarci. Poi abbiamo visto come creare un nuovo progetto Svelte con supporto TypeScript. Abbiamo anche visto come convertire un progetto Svelte esistente per usare TypeScript — la nostra app di lista delle cose da fare.

Abbiamo visto come lavorare con [Visual Studio Code](https://code.visualstudio.com/) e l'[estensione Svelte](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode) per ottenere funzionalità come il controllo dei tipi e l'autocompletamento. Abbiamo anche usato lo strumento `svelte-check` per ispezionare i problemi TypeScript dalla riga di comando.

Nel prossimo articolo impareremo come compilare e distribuire la nostra app in produzione. Vederemo anche quali risorse sono disponibili online per approfondire l'apprendimento di Svelte.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_stores","Learn_web_development/Core/Frameworks_libraries/Svelte_deployment_next", "Learn_web_development/Core/Frameworks_libraries")}}
