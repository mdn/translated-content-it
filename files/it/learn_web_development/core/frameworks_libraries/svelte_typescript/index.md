---
title: Supporto TypeScript in Svelte
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript
l10n:
  sourceCommit: 364a4d02b10854ab7cef4ff4b0ec3616d4e1c8ab
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_stores","Learn_web_development/Core/Frameworks_libraries/Svelte_deployment_next", "Learn_web_development/Core/Frameworks_libraries")}}

Nell'ultimo articolo abbiamo imparato a conoscere i Svelte stores e abbiamo anche implementato il nostro store personalizzato per persistere le informazioni dell'app nello Web Storage. Abbiamo anche dato un'occhiata all'utilizzo della direttiva di transizione per implementare animazioni sugli elementi DOM in Svelte.

Ora impareremo come usare TypeScript nelle applicazioni Svelte. Prima impareremo cos'è TypeScript e quali benefici può portarci. Poi vedremo come configurare il nostro progetto per lavorare con i file TypeScript. Infine, esamineremo la nostra app e vedremo quali modifiche dobbiamo fare per sfruttare appieno le funzionalità di TypeScript.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Al minimo, è raccomandato essere familiari con i linguaggi
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenza della
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >riga di comando</a
          >.
        </p>
        <p>
          Avrai bisogno di una riga di comando con node e npm installati per compilare e costruire
          la tua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare a configurare e utilizzare TypeScript quando si sviluppano applicazioni Svelte.
      </td>
    </tr>
  </tbody>
</table>

Nota che la nostra applicazione è pienamente funzionante, e trasferirla a TypeScript è completamente opzionale. Ci sono diverse opinioni a riguardo, e in questo capitolo parleremo brevemente dei pro e dei contro dell'uso di TypeScript. Anche se non intendi adottarlo, questo articolo sarà utile per permetterti di conoscere cosa ha da offrire e aiutarti a prendere una decisione. Se non sei affatto interessato a TypeScript, puoi passare al capitolo successivo, dove esamineremo diverse opzioni per distribuire le nostre applicazioni Svelte, ulteriori risorse e altro ancora.

## Codifica insieme a noi

### Git

Clonare il repository GitHub (se non lo hai già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Per raggiungere lo stato attuale dell'app, esegui

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

[TypeScript](https://www.typescriptlang.org/) è un superset di JavaScript che fornisce funzionalità come la tipizzazione statica opzionale, le classi, le interfacce e i generics. L'obiettivo di TypeScript è di aiutare a rilevare errori precocemente attraverso il suo sistema di tipi e rendere lo sviluppo di JavaScript più efficiente. Uno dei grandi benefici è di abilitare gli IDE a fornire un ambiente più ricco per individuare errori comuni mentre si scrive il codice.

La cosa migliore è che il codice JavaScript è codice TypeScript valido; TypeScript è un superset di JavaScript. Puoi rinominare la maggior parte dei tuoi file `.js` in file `.ts` e funzioneranno.

Il nostro codice TypeScript sarà in grado di funzionare ovunque JavaScript possa funzionare. Com'è possibile? TypeScript "transpila" il nostro codice in JavaScript vanilla. Ciò significa che analizza il codice TypeScript e produce il codice JavaScript vanilla equivalente che i browser possono eseguire.

> [!NOTE]
> Se sei curioso su come TypeScript transpila il nostro codice in JavaScript, puoi dare un'occhiata al [TypeScript Playground](https://www.typescriptlang.org/play/?target=1&e=4#example/hello-world).

Il supporto di TypeScript di prima classe è stata la funzione più richiesta per Svelte per un po' di tempo. Grazie al duro lavoro del team Svelte, insieme a molti contributori, hanno una [soluzione ufficiale](https://svelte.dev/blog/svelte-and-typescript) pronta per essere testata. In questa sezione ti mostreremo come configurare un progetto Svelte con il supporto TypeScript per provarlo.

## Perché TypeScript?

I principali vantaggi di TypeScript sono:

- Bug rilevati presto: Il compilatore controlla i tipi in fase di compilazione e fornisce report sugli errori.
- Leggibilità: La tipizzazione statica dà più struttura al codice, rendendolo auto-documentante e più leggibile.
- Supporto IDE avanzato: Le informazioni sui tipi permettono agli editor di codice e agli IDE di offrire funzionalità come la navigazione del codice, il completamento automatico e suggerimenti più intelligenti.
- Refactoring più sicuro: I tipi permettono agli IDE di conoscere meglio il tuo codice e assisterti durante il refactoring di ampie porzioni del tuo codice.
- Inferenza dei tipi: Ti permette di sfruttare molte funzionalità di TypeScript anche senza dichiarare i tipi delle variabili.
- Disponibilità di nuove e future funzionalità JavaScript: TypeScript transpila molte funzionalità recenti di JavaScript in JavaScript vecchio stile, permettendoti di usarle anche su agenti utente che non le supportano ancora nativamente.

TypeScript ha anche alcuni svantaggi:

- Non è una vera tipizzazione statica: I tipi sono controllati solo in fase di compilazione e vengono rimossi dal codice generato.
- Curva di apprendimento ripida: Anche se TypeScript è un superset di JavaScript e non un linguaggio completamente nuovo, c'è una curva di apprendimento considerevole, specialmente se non hai esperienza con linguaggi statici come Java o C#.
- Maggior codice: Devi scrivere e mantenere più codice.
- Nessuna sostituzione per i test automatici: Anche se i tipi possono aiutarti a catturare diversi bug, TypeScript non è una vera sostituzione per una suite completa di test automatizzati.
- Codice boilerplate: Lavorare con tipi, classi, interfacce e generics può portare a basi di codice troppo ingegnerizzate.

Sembra ci sia un ampio consenso sul fatto che TypeScript sia particolarmente adatto per progetti di grandi dimensioni, con molti sviluppatori che lavorano sullo stesso codice. Ed è infatti utilizzato da diversi progetti di grandi dimensioni, come Angular 2, Vue 3, Ionic, Visual Studio Code, Jest, e persino il compilatore Svelte. Tuttavia, alcuni sviluppatori preferiscono usarlo anche su piccoli progetti come quello che stiamo sviluppando.

Alla fine, la decisione è tua. Nelle sezioni successive speriamo di darti più elementi per prendere una decisione in merito.

## Creare un progetto Svelte con TypeScript da zero

Puoi iniziare un nuovo progetto Svelte TypeScript utilizzando il [template standard](https://github.com/sveltejs/template). Tutto quello che devi fare è eseguire i seguenti comandi da terminale (eseguili in un'area dove stai memorizzando i tuoi progetti di test Svelte — crea una nuova directory):

```bash
npx degit sveltejs/template svelte-typescript-app

cd svelte-typescript-app

node scripts/setupTypeScript.js
```

Questo crea un progetto di partenza che include il supporto TypeScript, che puoi quindi modificare a tuo piacimento.

Poi dovrai dire a npm di scaricare le dipendenze e avviare il progetto in modalità sviluppo, come facciamo di solito:

```bash
npm install

npm run dev
```

## Aggiungere il supporto TypeScript a un progetto Svelte esistente

Per aggiungere il supporto TypeScript a un progetto Svelte esistente, puoi [seguire queste istruzioni](https://svelte.dev/blog/svelte-and-typescript#Adding_TypeScript_to_an_existing_project). In alternativa, puoi scaricare il file [`setupTypeScript.js`](https://github.com/sveltejs/template/blob/master/scripts/setupTypeScript.js) in una cartella `scripts` all'interno della cartella root del tuo progetto, e poi eseguire `node scripts/setupTypeScript.js`.

Puoi anche usare `degit` per scaricare lo script. È ciò che faremo per iniziare a trasferire la nostra applicazione a TypeScript.

> [!NOTE]
> Ricorda che puoi eseguire `npx degit opensas/mdn-svelte-tutorial/07-typescript-support svelte-todo-typescript` per ottenere l'applicazione completa della lista delle cose da fare in JavaScript prima di iniziare a trasferirla a TypeScript.

Vai alla directory root del progetto ed esegui questi comandi:

```bash
npx degit sveltejs/template/scripts scripts       # download script file to a scripts folder

node scripts/setupTypeScript.js                   # run it
# Converted to TypeScript.
```

Dovrai rilanciare il tuo gestore delle dipendenze per iniziare.

```bash
npm install                                       # download new dependencies

npm run dev                                       # start the app in development mode
```

Queste istruzioni si applicano a qualsiasi progetto Svelte che desideri convertire in TypeScript. Tieni conto che la comunità Svelte sta costantemente migliorando il supporto TypeScript in Svelte, quindi dovresti eseguire `npm update` regolarmente per trarre vantaggio dalle ultime modifiche.

> [!NOTE]
> Se trovi problemi lavorando con TypeScript all'interno di un'applicazione Svelte, dai un'occhiata a questa [sezione di troubleshooting/FAQ sul supporto TypeScript](https://github.com/sveltejs/language-tools/blob/master/docs/preprocessors/typescript.md#troubleshooting--faq).

Come abbiamo detto prima, TypeScript è un superset di JavaScript, quindi la tua applicazione funzionerà senza modifiche. Attualmente stai eseguendo un'applicazione JavaScript regolare con il supporto TypeScript abilitato, senza sfruttare alcuna delle funzionalità che TypeScript fornisce. Ora puoi iniziare ad aggiungere i tipi progressivamente.

Una volta configurato TypeScript, puoi iniziare a usarlo da un componente Svelte semplicemente aggiungendo un `<script lang='ts'>` all'inizio della sezione script. Per usarlo da file JavaScript regolari, basta cambiare l'estensione del file da `.js` a `.ts`. Dovrai anche aggiornare ogni conferme importazione corrispondente rimuovendo l'estensione del file `.ts` da tutte le dichiarazioni `import`.

> [!NOTE]
> TypeScript mostrerà un errore se usi l'estensione del file `.ts` in una dichiarazione `import`, quindi se hai un file `./foo.ts`, devi importarlo come "./foo".
> Vedi la sezione [Risoluzione dei moduli per i bundler, i runtime TypeScript, e i loader Node.js](https://www.typescriptlang.org/docs/handbook/modules/theory.html#module-resolution-for-bundlers-typescript-runtimes-and-nodejs-loaders) del manuale di TypeScript per ulteriori informazioni.

> [!NOTE]
> L'uso di TypeScript nelle sezioni markup dei componenti non è supportato in Svelte 4, su cui si basa questa guida.
> Quindi, mentre puoi usare JavaScript dal markup, dovrai usare TypeScript nella sezione `<script lang='ts'>`.
> TypeScript nel markup dei componenti è consentito a partire da Svelte 5.

## Miglioramento dell'esperienza dello sviluppatore con TypeScript

TypeScript fornisce agli editor di codice e gli IDE molte informazioni per permettere loro di offrire un'esperienza di sviluppo più amichevole.

Utilizzeremo [Visual Studio Code](https://code.visualstudio.com/) per fare un rapido test e vedere come possiamo ottenere suggerimenti di completamento automatico e controllo dei tipi mentre stiamo scrivendo componenti.

> [!NOTE]
> Se non desideri usare VS Code, forniamo anche istruzioni per usare il controllo degli errori TypeScript dal terminale poco più avanti.

È in corso il lavoro per supportare TypeScript nei progetti Svelte in diversi editor di codice; il supporto più completo finora è disponibile nell'[estensione Svelte per VS Code](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode), che è sviluppata e mantenuta dal team di Svelte. Questa estensione offre controllo dei tipi, ispezione, refactoring, intellisense, informazioni al passaggio del mouse, completamento automatico e altre funzionalità. Questo tipo di assistenza agli sviluppatori è un altro buon motivo per iniziare a usare TypeScript nei tuoi progetti.

> [!NOTE]
> Assicurati di utilizzare [Svelte per VS Code](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode) e NON il vecchio "Svelte" di James Birtles, che è stato dismesso. Nel caso lo avessi installato, dovresti disinstallarlo e installare invece l'estensione ufficiale Svelte.

Supponendo che tu sia all'interno dell'applicazione VS Code, dalla root della cartella del tuo progetto, digita `code .` (il punto finale dice a VS Code di aprire la cartella corrente) per aprire l'editor di codice. VS Code ti dirà che ci sono estensioni raccomandate da installare.

![Finestra di dialogo che dice che questo spazio di lavoro ha raccomandazioni sulle estensioni, con opzioni per installare o mostrare un elenco](01-vscode-extension-recommendations.png)

Cliccando su _Installa tutto_ installerai Svelte per VS Code.

![Informazioni sull'estensione Svelte per VS Code](02-svelte-for-vscode.png)

Possiamo anche vedere che il file `setupTypeScript.js` ha apportato alcune modifiche al nostro progetto. Il file `main.js` è stato rinominato in `main.ts`, il che significa che VS Code può fornire informazioni al passaggio del mouse sui nostri componenti Svelte:

![Schermata di VS Code che mostra che al passaggio del mouse su un componente vengono forniti suggerimenti](03-vscode-hints-in-main-ts.png)

<!-- cSpell:ignore traget -->

Otteniamo anche il controllo dei tipi gratuitamente. Se passiamo una proprietà sconosciuta nel parametro delle opzioni del costruttore `App` (ad esempio un errore di battitura come `traget` invece di `target`), TypeScript si lamenterà:

![Controllo dei tipi in VS Code - Oggetto App è stato dato una proprietà sconosciuta traget](04-vscode-type-checking-in-main-ts.png)

Nel componente `App.svelte`, lo script `setupTypeScript.js` ha aggiunto l'attributo `lang="ts"` al tag `<script>`. Inoltre, grazie all'inferenza dei tipi, in molti casi non dovremo nemmeno specificare i tipi per ottenere supporto al codice. Ad esempio, se inizi ad aggiungere una proprietà `ms` alla chiamata del componente `Alert`, TypeScript dedurrà dal valore predefinito che la proprietà `ms` dovrebbe essere un numero:

![Inferenza dei tipi di VS Code e suggerimenti per il codice - La variabile ms dovrebbe essere un numero](05-vscode-type-inference-and-code-assistance.png)

E se passi qualcosa che non è un numero, si lamenterà:

![Controllo dei tipi in VS Code - la variabile ms è stata data un valore non numerico](06-vscode-type-checking-in-components.png)

Il template dell'app ha uno script `check` configurato che esegue `svelte-check` sul tuo codice. Questo pacchetto ti permette di rilevare errori e avvertimenti normalmente visualizzati da un editor di codice dalla riga di comando, il che lo rende piuttosto utile per eseguirlo in una pipeline di integrazione continua (CI). Basta eseguire `npm run check` per controllare il CSS inutilizzato e restituire suggerimenti A11y ed errori di compilazione TypeScript.

In questo caso, se esegui `npm run check` (sia nella console di VS Code che nel terminale), otterrai il seguente errore:

![Comando check eseguito all'interno di VS Code che mostra un errore di tipo, la variabile ms dovrebbe essere assegnata a un numero](07-vscode-svelte-check.png)

Ancora meglio, se lo esegui dal terminale integrato di VS Code (puoi aprirlo con la scorciatoia da tastiera <kbd>Ctrl</kbd> + <kbd>\`</kbd>), cliccando su <kbd>Cmd</kbd>/<kbd>Ctrl</kbd> sul nome del file ti porterà alla linea contenente l'errore.

Puoi anche eseguire lo script `check` in modalità watch con `npm run check -- --watch`. In questo caso, lo script verrà eseguito ogni volta che cambi un file. Se stai eseguendo questo comando nel tuo terminale regolare, mantienilo in esecuzione in background in una finestra del terminale separata in modo che possa continuare a segnalare errori ma non interferirà con l'uso di altri terminali.

## Creazione di un tipo personalizzato

TypeScript supporta la tipizzazione strutturale. La tipizzazione strutturale è un modo di collegare i tipi basato esclusivamente sui loro membri, anche se non definisci esplicitamente il tipo.

Definiremo un tipo `TodoType` per vedere come TypeScript applica il fatto che qualsiasi cosa passata a un componente che si aspetta un `TodoType` sarà strutturalmente compatibile con esso.

1. All'interno della cartella `src` crea una cartella `types`.
2. Aggiungi un file `todo.type.ts` al suo interno.
3. Fornisci al file `todo.type.ts` il seguente contenuto:

   ```ts
   export type TodoType = {
     id: number;
     name: string;
     completed: boolean;
   };
   ```

   > [!NOTE]
   > Il template Svelte utilizza [svelte-preprocess](https://github.com/sveltejs/svelte-preprocess) 4.0.0 per supportare TypeScript. Da quella versione in avanti devi usare la sintassi `export`/`import` type per importare tipi e interfacce. Controlla [questa sezione della guida di risoluzione dei problemi](https://github.com/sveltejs/language-tools/blob/master/docs/preprocessors/typescript.md#how-do-i-import-interfaces-into-my-svelte-components-i-get-errors-after-transpilation) per ulteriori informazioni.

4. Ora utilizzeremo `TodoType` dal nostro componente `Todo.svelte`. Per prima cosa aggiungi `lang="ts"` al tag `<script>`.
5. Importiamo il tipo e usiamolo per dichiarare la proprietà `todo`. Sostituisci la riga `export let todo` con la seguente:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todo: TodoType;
   ```

   Nota che l'estensione del file `.ts` non è consentita nella dichiarazione `import` ed è stata omessa.

6. Ora da `Todos.svelte` istanzieremo un componente `Todo` con un oggetto letterale come parametro prima della chiamata al componente `MoreActions`, in questo modo:

   ```svelte
   <hr />

   <Todo todo={ { name: 'a new task with no id!', completed: false } } />

   <!-- MoreActions -->
   <MoreActions {todos}
   ```

7. Aggiungi `lang='ts'` al tag `<script>` del componente `Todos.svelte` in modo che sappia di usare il controllo dei tipi che abbiamo specificato.

   Riceveremo il seguente errore:

   ![Errore di tipo in VS Code, l'oggetto Todo Type richiede una proprietà id.](08-vscode-structural-typing.png)

A questo punto dovresti avere un'idea dell'assistenza che possiamo ottenere da TypeScript quando costruiamo progetti Svelte.

Ora annulleremo queste modifiche per iniziare a trasferire la nostra applicazione a TypeScript, così non saremo disturbati da tutti gli avvisi di controllo.

1. Rimuovi il todo errato e l'attributo `lang='ts'` dal file `Todos.svelte`.
2. Rimuovi anche l'importazione di `TodoType` e `lang='ts'` da `Todo.svelte`.

Ci occuperemo di loro in modo appropriato in seguito.

## Portare la nostra app lista di cose da fare a TypeScript

Ora siamo pronti a iniziare a trasferire la nostra applicazione lista di cose da fare per trarre vantaggio da tutte le funzionalità che TypeScript ci offre.

Iniziamo eseguendo lo script di controllo in modalità watch all'interno della root del progetto:

```bash
npm run check -- --watch
```

Questo dovrebbe produrre qualcosa del tipo:

```bash
svelte-check "--watch"

Loading svelte-check in workspace: ./svelte-todo-typescript
Getting Svelte diagnostics...
====================================
svelte-check found no errors and no warnings
```

Nota che se stai utilizzando un editor di codice di supporto come VS Code, un modo semplice per iniziare a trasferire un componente Svelte è aggiungere il `<script lang='ts'>` nella parte superiore del tuo componente e cercare i suggerimenti con il simbolo dei tre puntini:

![Schermata di VS Code che mostra che quando si aggiunge type="ts" a un componente, vengono forniti suggerimenti con i tre punti di alert](09-vscode-alert-hints.png)

### Alert.svelte

Iniziamo con il nostro componente `Alert.svelte`.

1. Aggiungi `lang="ts"` al componente `<script>` del tuo componente `Alert.svelte`. Vedrai alcuni avvisi nell'output dello script `check`:

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

2. Puoi risolverli specificando i tipi corrispondenti, in questo modo:

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
   > Non c'è bisogno di specificare il tipo di `ms` con `export let ms:number = 3000`, perché TypeScript lo sta già deducendo dal suo valore predefinito.

### MoreActions.svelte

Ora faremo lo stesso per il componente `MoreActions.svelte`.

1. Aggiungi l'attributo `lang='ts'`, come prima. TypeScript ci avviserà sulla proprietà `todos` e la variabile `t` nella chiamata a `todos.filter((t) =>...)`.

   ```plain
   Warn: Variable 'todos' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     export let todos

   Warn: Parameter 't' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     $: completedTodos = todos.filter((t) => t.completed).length
   ```

2. Utilizzeremo il `TodoType` che abbiamo già definito per dire a TypeScript che `todos` è un array di `TodoType`. Sostituisci la riga `export let todos` con la seguente:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todos: TodoType[];
   ```

Nota che ora TypeScript può dedurre che la variabile `t` in `todos.filter((t) => t.completed)` sia di tipo `TodoType`. Tuttavia, se pensiamo che renda il nostro codice più leggibile, potremmo specificarlo in questo modo:

```ts
$: completedTodos = todos.filter((t: TodoType) => t.completed).length;
```

La maggior parte delle volte, TypeScript sarà in grado di dedurre correttamente il tipo di variabile reattiva, ma a volte potresti ottenere un errore "ha implicitamente un tipo 'any'" quando lavori con assegnazioni reattive. In quei casi puoi dichiarare la variabile tipizzata in un'altra dichiarazione, in questo modo:

```ts
let completedTodos: number;
$: completedTodos = todos.filter((t: TodoType) => t.completed).length;
```

Non puoi specificare il tipo nell'assegnazione reattiva stessa. L'istruzione `$: completedTodos: number = todos.filter[...]` non è valida. Per ulteriori informazioni, leggi [Come faccio a digitare assegnazioni reattive? / Ottengo un errore "ha implicitamente tipo 'any'"](https://github.com/sveltejs/language-tools/blob/master/docs/preprocessors/typescript.md#how-do-i-type-reactive-assignments--i-get-an-implicitly-has-type-any-error).

### FilterButton.svelte

Ora ci occuperemo del componente `FilterButton`.

1. Aggiungi l'attributo `lang='ts'` al tag `<script>`, come al solito. Noterai che non ci sono avvisi - TypeScript deduce il tipo della variabile filter dal valore predefinito. Ma sappiamo che ci sono solo tre valori validi per il filtro: all, active, e completed. Quindi possiamo farlo sapere a TypeScript creando un enum Filter.
2. Crea un file `filter.enum.ts` nella cartella `types`.
3. Gli daremo il seguente contenuto:

   ```ts
   export enum Filter {
     ALL = "all",
     ACTIVE = "active",
     COMPLETED = "completed",
   }
   ```

4. Ora useremo questo nel componente `FilterButton`. Sostituisci il contenuto del file `FilterButton.svelte` con il seguente:

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

Qui stiamo semplicemente importando l'enumerazione `Filter` e usandola al posto dei valori di stringa che abbiamo usato in precedenza.

### Todos.svelte

Useremo anche l'enumerazione `Filter` nel componente `Todos.svelte`.

1. Prima di tutto, aggiungi l'attributo `lang='ts'`, come prima.
2. Poi importa l'enumerazione `Filter`. Aggiungi la seguente dichiarazione `import` sotto quelle già esistenti:

   ```js
   import { Filter } from "../types/filter.enum";
   ```

3. Ora la useremo tutte le volte che facciamo riferimento al filtro attuale. Sostituisci i tuoi due blocchi relativi al filtro con i seguenti:

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

4. `check` restituirà ancora alcuni avvisi di `Todos.svelte`. Risolviamoli.

   Inizia importando il `TodoType` e dicendo a TypeScript che la nostra variabile `todos` è un array di `TodoType`. Sostituisci `export let todos = []` con le seguenti due righe:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todos: TodoType[] = [];
   ```

5. Poi specifichiamo tutti i tipi mancanti. La variabile `todosStatus`, che abbiamo usato per accedere programmaticamente ai metodi esposti dal componente `TodosStatus`, è di tipo `TodosStatus`. E ogni `todo` sarà di tipo `TodoType`.

   Aggiorna la tua sezione `<script>` per essere simile a questa:

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

Stiamo riscontrando i seguenti errori relativi al passaggio di `todos` ai componenti `TodosStatus.svelte` (e `Todo.svelte`):

```plain
./src/components/Todos.svelte:70:39
Error: Type 'TodoType[]' is not assignable to type 'undefined'. (ts)
  <TodosStatus bind:this={todosStatus} {todos} />

./src/components/Todos.svelte:76:12
Error: Type 'TodoType' is not assignable to type 'undefined'. (ts)
     <Todo {todo}
```

Ciò è dovuto al fatto che la prop `todos` nel componente `TodosStatus` non ha un valore di default, quindi TypeScript l'ha inferita come di tipo `undefined`, che non è compatibile con un array di `TodoType`. Lo stesso sta accadendo con il nostro componente Todo.

Risolviamolo.

1. Apri il file `TodosStatus.svelte` e aggiungi l'attributo `lang='ts'`.
2. Quindi importa `TodoType` e dichiara la prop `todos` come un array di `TodoType`. Sostituisci la prima riga della sezione `<script>` con la seguente:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todos: TodoType[];
   ```

3. Specificheremo anche l'`headingEl`, che abbiamo usato per legare al tag di intestazione, come un `HTMLElement`. Aggiorna la riga `let headingEl` in questo modo:

   ```ts
   let headingEl: HTMLElement;
   ```

4. Infine, noterai l'errore segnalato relativo a dove impostiamo l'attributo `tabindex`. Questo perché TypeScript sta controllando il tipo dell'elemento `<h2>` e si aspetta che `tabindex` sia di tipo `number`.

   ![Suggerimento tabindex all'interno di VS Code, tabindex si aspetta un tipo di number, non string](10-vscode-tabindex-hint.png)

   Per risolverlo, sostituisci `tabindex="-1"` con `tabindex={-1}`, in questo modo:

   ```svelte
   <h2 id="list-heading" bind:this={headingEl} tabindex={-1}>
     {completedTodos} out of {totalTodos} items completed
   </h2>
   ```

   In questo modo TypeScript può prevenire che venga assegnato a una stringa in modo scorretto.

### NewTodo.svelte

Successivamente ci occuperemo di `NewTodo.svelte`.

1. Come al solito, aggiungi l'attributo `lang='ts'`.
2. L'avviso indicherà che dobbiamo specificare un tipo per la variabile `nameEl`. Imposta il suo tipo su `HTMLElement` in questo modo:

   ```ts
   let nameEl: HTMLElement; // reference to the name input DOM node
   ```

3. L'ultima per questo file, dobbiamo specificare il tipo corretto per la nostra variabile `autofocus`. Aggiorna la sua definizione come segue:

   ```ts
   export let autofocus: boolean = false;
   ```

### Todo.svelte

Ora gli unici avvisi che `npm run check` emette sono attivati dalla chiamata al componente `Todo.svelte`. Risolviamoli.

1. Apri il file `Todo.svelte`, e aggiungi l'attributo `lang='ts'`.
2. Importiamo `TodoType`, e impostiamo il tipo della prop `todo`. Sostituisci la riga `export let todo` con la seguente:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todo: TodoType;
   ```

3. Il primo avviso che riceviamo è che TypeScript ci sta dicendo di definire il tipo della variabile `updatedTodo` nella funzione `update()`. Questo può essere un po' complicato perché `updatedTodo` contiene solo gli attributi del `todo` che sono stati aggiornati. Questo significa che non è un `todo` completo — ha solo un sottoinsieme delle proprietà di `todo`.

   Per questi tipi di casi, TypeScript fornisce diversi [tipi utility](https://www.typescriptlang.org/docs/handbook/utility-types.html) per facilitare l'applicazione di queste trasformazioni comuni. Ciò di cui abbiamo bisogno in questo momento è l'utility [`Partial<T>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialt), che ci permette di rappresentare tutti i sottoinsiemi di un dato tipo. L'utility partial ritorna un nuovo tipo basato sul tipo `T`, dove ogni proprietà di `T` è opzionale.

   Lo useremo nella funzione `update()` — aggiornala come segue:

   ```ts
   function update(updatedTodo: Partial<TodoType>) {
     todo = { ...todo, ...updatedTodo }; // applies modifications to todo
     dispatch("update", todo); // emit update event
   }
   ```

   Con questo stiamo dicendo a TypeScript che la variabile `updatedTodo` conterrà un sottinsieme delle proprietà di `TodoType`.

4. Ora svelte-check ci dice che dobbiamo definire il tipo dei parametri della nostra funzione action:

   ```bash
   ./07-next-steps/src/components/Todo.svelte:45:24
   Warn: Parameter 'node' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     const focusOnInit = (node) => node && typeof node.focus === 'function' && node.focus()

   ./07-next-steps/src/components/Todo.svelte:47:28
   Warn: Parameter 'node' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     const focusEditButton = (node) => editButtonPressed && node.focus()
   ```

   Dobbiamo solo definire la variabile node come di tipo `HTMLElement`. Nelle due righe indicate sopra, sostituisci la prima istanza di `node` con `node: HTMLElement`.

### actions.js

Successivamente ci occuperemo del file `actions.js`.

1. Rinominalo in `actions.ts` e aggiungi il tipo del parametro node. Dovrebbe finire per sembrare così:

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

2. Ora aggiorna `Todo.svelte` e `NewTodo.svelte` dove importiamo il file delle azioni. Ricorda che le importazioni in TypeScript non includono l'estensione del file. In entrambi i casi dovrebbe finire per essere così:

   ```js
   import { selectOnFocus } from "../actions";
   ```

### Migrazione degli store a TypeScript

Ora dobbiamo migrare i file `stores.js` e `localStore.js` a TypeScript.

Suggerimento: lo script `npm run check`, che utilizza lo strumento [`svelte-check`](https://github.com/sveltejs/language-tools/tree/master/packages/svelte-check), verificherà solo i file `.svelte` della nostra applicazione. Se vuoi controllare anche i file `.ts`, puoi eseguire `npm run check && npx tsc --noEmit`, che dice al compilatore TypeScript di controllare gli errori senza generare i file di output `.js`. Potresti anche aggiungere uno script al tuo file `package.json` che esegue quel comando.

Inizieremo con `stores.js`.

1. Rinomina il file in `stores.ts`.
2. Imposta il tipo del nostro array `initialTodos` a `TodoType[]`. Ecco come saranno i contenuti:

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

3. Ricorda di aggiornare le dichiarazioni `import` in `App.svelte`, `Alert.svelte` e `Todos.svelte`. Basta rimuovere l'estensione `.js`, così:

   ```js
   import { todos } from "../stores";
   ```

Ora su `localStore.js`.

Aggiorna la dichiarazione `import` in `stores.ts` in questo modo:

```js
import { localStore } from "./localStore";
```

1. Inizia rinominando il file in `localStore.ts`.
2. TypeScript ci sta dicendo di specificare il tipo delle variabili `key`, `initial` e `value`. La prima è facile: la chiave del nostro local web storage dovrebbe essere una stringa.

   Ma `initial` e `value` dovrebbero essere un qualsiasi oggetto che potrebbe essere convertito in una stringa JSON valida con il metodo [`JSON.stringify`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify), il che significa qualsiasi oggetto JavaScript con un paio di limitazioni: ad esempio, `undefined`, funzioni, e simboli non sono valori JSON validi.

   Quindi creeremo il tipo `JsonValue` per specificare queste condizioni.

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

   L'operatore `|` ci permette di dichiarare variabili che potrebbero memorizzare valori di due o più tipi. Un `JsonValue` potrebbe essere una stringa, un numero, un booleano, e così via. In questo caso stiamo anche usando tipi ricorsivi per specificare che un `JsonValue` può avere un array di `JsonValue` e anche un oggetto con proprietà di tipo `JsonValue`.

4. Importeremo il nostro tipo `JsonValue` e lo useremo di conseguenza. Aggiorna il tuo file `localStore.ts` in questo modo:

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
       set: (value: JsonValue) => {
         localStorage.setItem(key, toString(value)); // save also to local storage as a string
         return set(value);
       },
       update,
     };
   };
   ```

Ora, se proviamo a creare un `localStore` con qualcosa che non può essere convertito in JSON tramite `JSON.stringify()`, ad esempio un oggetto con una funzione come proprietà, VS Code/`validate` si lamenterà:

![VS Code mostra un errore con l'uso del nostro store - fallisce quando si tenta di impostare un valore di storage locale a qualcosa di incompatibile con JSON stringify](11-vscode-invalid-store.png)

E cosa ancora migliore, funzionerà anche con la sintassi [`$store` auto-subscription](https://svelte.dev/docs/svelte-components#script-4-prefix-stores-with-$-to-access-their-values). Se proviamo a salvare un valore non valido nel nostro store `todos` utilizzando la sintassi `$store`, come questo:

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

Lo script di controllo segnalerà il seguente errore:

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

Questo è un altro esempio di come specificare i tipi può rendere il nostro codice più robusto e aiutarci a catturare più bug prima che entrino in produzione.

E questo è tutto. Abbiamo convertito tutta la nostra applicazione per utilizzare TypeScript.

## Rendere i nostri store a prova di errore con Generics

I nostri store sono già stati convertiti in TypeScript, ma possiamo fare di meglio. Non dovremmo aver bisogno di memorizzare qualsiasi tipo di valore — sappiamo che lo store di alert dovrebbe contenere messaggi di stringhe, e lo store dei to-do dovrebbe contenere un array di `TodoType`, ecc. Possiamo far sì che TypeScript applichi questo usando [TypeScript Generics](https://www.typescriptlang.org/docs/handbook/generics.html). Scopriamone di più.

### Capire i generics di TypeScript

I generics ti permettono di creare componenti di codice riutilizzabili che funzionano con una varietà di tipi invece di un singolo tipo. Possono essere applicati a interfacce, classi, e funzioni. I tipi generici vengono passati come parametri utilizzando una sintassi speciale: vengono specificati all'interno delle parentesi angolari e sono convenzionalmente indicati con una singola lettera maiuscola. I tipi generici ti permettono di catturare i tipi forniti dall'utente, assicurando che siano disponibili per un'elaborazione futura.

Vediamo un esempio veloce, una classe `Stack` semplice che ti consente di `push` e `pop` elementi, come questo:

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

In questo caso `elements` è un array di tipo `any`, e di conseguenza i metodi `push()` e `pop()` ricevono e restituiscono entrambi una variabile di tipo `any`. Quindi è perfettamente valido fare qualcosa del genere:

```js
const anyStack = new Stack();

anyStack.push(1);
anyStack.push("hello");
```

Ma cosa succede se vogliamo avere uno `Stack` che funzioni solo con il tipo `string`? Potremmo fare così:

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

Questo funzionerebbe. Ma se volessimo lavorare con numeri, allora dovremmo duplicare il nostro codice e creare una classe `NumberStack`. E come potremmo gestire uno stack di tipi che non conosciamo ancora, e che dovrebbero essere definiti dal consumatore?

Per risolvere tutti questi problemi, possiamo utilizzare i generics.

Questa è la nostra classe `Stack` reimplementata utilizzando i generics:

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

Definiamo un tipo generico `T` e poi lo utilizziamo come faremmo normalmente con un tipo specifico. Ora elements è un array di tipo `T`, e `push()` e `pop()` ricevono e restituiscono entrambi una variabile di tipo `T`.

Ecco come useremmo il nostro `Stack` generico:

```ts
const numberStack = new Stack<number>();
numberStack.push(1);
```

Ora TypeScript sa che il nostro stack può accettare solo numeri, e emetterà un errore se proviamo a inserire qualsiasi altra cosa:

![L'argomento di tipo hello non è assegnabile al parametro di tipo number](12-vscode-generic-stack-error.png)

TypeScript può anche dedurre i tipi generici dal suo utilizzo. I generics supportano anche valori predefiniti e vincoli.

I generics sono una caratteristica potente che permette al nostro codice di astrarre dai tipi specifici utilizzati, rendendolo più riutilizzabile e generico senza rinunciare alla sicurezza dei tipi. Per saperne di più, consulta l'[Introduzione ai Generics di TypeScript](https://www.typescriptlang.org/docs/handbook/generics.html).

### Uso degli store Svelte con generics

Gli store Svelte supportano i generics direttamente. E, grazie all'inferenza dei tipi generici, possiamo approfittarne senza nemmeno toccare il nostro codice.

Se apri il file `Todos.svelte` e assegni un tipo `number` al nostro store `$alert`, otterrai il seguente errore:

![L'argomento di tipo 9999 non è assegnabile al parametro di tipo string](13-vscode-generic-alert-error.png)

Questo perché quando abbiamo definito il nostro store di alert nel file `stores.ts` con:

```js
export const alert = writable("Welcome to the To-Do list app!");
```

TypeScript ha dedotto il tipo generico come `string`. Se volessimo essere espliciti al riguardo, potremmo fare così:

```ts
export const alert = writable<string>("Welcome to the To-Do list app!");
```

Ora renderemo il nostro store `localStore` supporto ai generics. Ricorda che abbiamo definito il tipo `JsonValue` per prevenire l'uso del nostro store `localStore` con valori che non possono essere persisiti usando `JSON.stringify()`. Ora vogliamo che i consumatori di `localStore` siano in grado di specificare il tipo di dati da perscare, ma anziché lavorare con qualsiasi tipo, dovrebbero essere conformi al tipo `JsonValue`. Lo specificheremo con un vincolo generico, in questo modo:

```ts
export const localStore = <T extends JsonValue>(key: string, initial: T) => {
  // …
};
```

Definiamo un tipo generico `T` e specifichiamo che deve essere compatibile con il tipo `JsonValue`. Poi useremo il tipo `T` in modo appropriato.

Il nostro file `localStore.ts` finirà per essere così — prova il nuovo codice ora nella tua versione:

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
    set: (value: T) => {
      localStorage.setItem(key, toString(value)); // save also to local storage as a string
      return set(value);
    },
    update,
  };
};
```

E grazie all'inferenza dei tipi generici, TypeScript sa già che il nostro store `$todos` dovrebbe contenere un array di `TodoType`:

![Oggetto Todo Type la proprietà completa dovrebbe essere completata](14-vscode-generic-localstore-error.png)

Ancora una volta, se volessimo essere espliciti al riguardo, potremmo farlo nel file `stores.ts` in questo modo:

```ts
const initialTodos: TodoType[] = [
  { id: 1, name: "Visit MDN web docs", completed: true },
  { id: 2, name: "Complete the Svelte Tutorial", completed: false },
];

export const todos = localStore<TodoType[]>("mdn-svelte-todo", initialTodos);
```

Questo è sufficiente per il nostro breve tour dei Generics di TypeScript.

## Il codice finora

### Git

Per vedere lo stato del codice come dovrebbe essere alla fine di questo articolo, accedi alla tua copia del nostro repository in questo modo:

```bash
cd mdn-svelte-tutorial/08-next-steps
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/08-next-steps
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità sviluppo.

### REPL

Come detto in precedenza, TypeScript non è ancora disponibile nel REPL.

## Sintesi

In questo articolo abbiamo preso la nostra applicazione lista di cose da fare e l'abbiamo portata a TypeScript.

Abbiamo prima imparato cosa è TypeScript e quali vantaggi può portare. Poi abbiamo visto come creare un nuovo progetto Svelte con supporto TypeScript. Abbiamo anche visto come convertire un progetto Svelte esistente per utilizzare TypeScript — la nostra app lista di cose da fare.

Abbiamo visto come lavorare con [Visual Studio Code](https://code.visualstudio.com/) e l'[estensione di Svelte](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode) per ottenere funzionalità come il controllo dei tipi e l'auto-completamento. Abbiamo anche utilizzato lo strumento `svelte-check` per ispezionare i problemi di TypeScript dalla riga di comando.

Nel prossimo articolo impareremo come compilare e distribuire la nostra app in produzione. Vedremo anche quali risorse sono disponibili online per approfondire l'apprendimento di Svelte.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_stores","Learn_web_development/Core/Frameworks_libraries/Svelte_deployment_next", "Learn_web_development/Core/Frameworks_libraries")}}
