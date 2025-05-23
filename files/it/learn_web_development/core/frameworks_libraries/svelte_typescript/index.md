---
title: Supporto TypeScript in Svelte
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_stores","Learn_web_development/Core/Frameworks_libraries/Svelte_deployment_next", "Learn_web_development/Core/Frameworks_libraries")}}

Nell'ultimo articolo abbiamo imparato riguardo ai Svelte store e abbiamo persino implementato il nostro store personalizzato per mantenere le informazioni dell'app nello Storage Web. Abbiamo anche esaminato l'utilizzo della direttiva di transizione per implementare animazioni sugli elementi DOM in Svelte.

Ora impareremo come utilizzare TypeScript nelle applicazioni Svelte. Per prima cosa, impareremo cosa è TypeScript e quali benefici può portare. Poi vedremo come configurare il nostro progetto per lavorare con file TypeScript. Infine, esamineremo la nostra app e vedremo quali modifiche dobbiamo apportare per sfruttare appieno le funzionalità di TypeScript.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È consigliato come minimo avere familiarità con i linguaggi principali
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/command line</a
          >.
        </p>
        <p>
          Avrai bisogno di un terminale con node e npm installati per compilare e costruire
          la tua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare come configurare e utilizzare TypeScript nello sviluppo di applicazioni
        Svelte.
      </td>
    </tr>
  </tbody>
</table>

Nota che la nostra applicazione è pienamente funzionale e portarla a TypeScript è completamente opzionale. Ci sono diverse opinioni su questo, e in questo capitolo parleremo brevemente dei pro e contro dell'utilizzo di TypeScript. Anche se non intendi adottarlo, questo articolo sarà utile per consentirti di apprendere ciò che ha da offrire e aiutarti a prendere la tua decisione. Se non sei affatto interessato a TypeScript, puoi passare al capitolo successivo, dove esamineremo diverse opzioni per distribuire le nostre applicazioni Svelte, ulteriori risorse e altro ancora.

## Segui il codice con noi

### Git

Clona il repo GitHub (se non lo hai già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi per raggiungere lo stato attuale dell'app, esegui

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

[TypeScript](https://www.typescriptlang.org/) è un superset di JavaScript che offre funzionalità come la tipizzazione statica opzionale, classi, interfacce e generici. L'obiettivo di TypeScript è aiutare a rilevare errori precocemente attraverso il suo sistema di tipi e rendere lo sviluppo JavaScript più efficiente. Uno dei grandi benefici è permettere agli IDE di fornire un ambiente più ricco per individuare errori comuni mentre si digita il codice.

Il meglio di tutto è che il codice JavaScript è valido codice TypeScript; TypeScript è un superset di JavaScript. Puoi rinominare la maggior parte dei tuoi file `.js` in `.ts` e funzioneranno comunque.

Il nostro codice TypeScript sarà in grado di funzionare ovunque JavaScript possa funzionare. Com’è possibile? TypeScript “transpila” il nostro codice in JavaScript semplice. Ciò significa che interpreta il codice TypeScript e produce l'equivalente codice JavaScript semplice per i browser.

> [!NOTE]
> Se sei curioso di sapere come TypeScript transpila il nostro codice in JavaScript, puoi dare un'occhiata al [TypeScript Playground](https://www.typescriptlang.org/play/?target=1&e=4#example/hello-world).

Il supporto di prima classe per TypeScript è stato la caratteristica più richiesta per Svelte per un po' di tempo. Grazie al duro lavoro del team Svelte, insieme a molti collaboratori, hanno una [soluzione ufficiale](https://svelte.dev/blog/svelte-and-typescript) pronta per essere messa alla prova. In questa sezione ti mostreremo come configurare un progetto Svelte con supporto TypeScript per provarlo.

## Perché TypeScript?

I principali vantaggi di TypeScript sono:

- Bug rilevati presto: Il compilatore controlla i tipi al momento della compilazione e fornisce segnalazione degli errori.
- Leggibilità: La tipizzazione statica conferisce al codice una struttura maggiore, rendendolo auto-documentante e più leggibile.
- Supporto IDE ricco: Le informazioni sui tipi permettono agli editor di codice e agli IDE di offrire funzionalità come navigazione nel codice, completamento automatico e suggerimenti più intelligenti.
- Refactoring più sicuro: I tipi permettono agli IDE di sapere di più sul tuo codice e assisterti mentre refattorizzi grandi porzioni del tuo codice.
- Inferenza dei tipi: Permette di sfruttare molte funzionalità di TypeScript anche senza dichiarare i tipi di variabile.
- Disponibilità di nuove e future funzionalità JavaScript: TypeScript transpila molte delle recenti funzionalità JavaScript a codice JavaScript tradizionale, permettendoti di utilizzarle anche su user-agent che non le supportano nativamente ancora.

TypeScript ha anche alcune limitazioni:

- Non vera tipizzazione statica: I tipi sono controllati solo al momento della compilazione e vengono rimossi dal codice generato.
- Curva di apprendimento ripida: Anche se TypeScript è un superset di JavaScript e non un linguaggio completamente nuovo, c'è una considerevole curva di apprendimento, specialmente se non hai alcuna esperienza con linguaggi statici come Java o C#.
- Più codice: Devi scrivere e mantenere più codice.
- Nessun sostituto per i collaudi automatici: Anche se i tipi possono aiutarti a rilevare diversi bug, TypeScript non è un vero sostituto per una suite completa di test automatizzati.
- Codice boilerplate: Lavorare con i tipi, le classi, le interfacce e i generici può condurre a codici base troppo ingegnerizzati.

Sembra esserci un ampio consenso sul fatto che TypeScript sia particolarmente adatto per progetti su larga scala, con molti sviluppatori che lavorano sulla stessa base di codice. E infatti viene usato da diversi progetti su larga scala, come Angular 2, Vue 3, Ionic, Visual Studio Code, Jest, e persino il compilatore Svelte. Tuttavia, alcuni sviluppatori preferiscono usarlo anche su piccoli progetti come quello che stiamo sviluppando.

Alla fine, la decisione spetta a te. Nelle sezioni seguenti speriamo di fornirti più elementi per prendere una decisione informata al riguardo.

## Creare un progetto Svelte TypeScript da zero

Puoi iniziare un nuovo progetto Svelte TypeScript utilizzando il [template standard](https://github.com/sveltejs/template). Tutto ciò che devi fare è eseguire i seguenti comandi nel terminale (eseguili in un luogo dove stai conservando i tuoi progetti test Svelte — crea una nuova directory):

```bash
npx degit sveltejs/template svelte-typescript-app

cd svelte-typescript-app

node scripts/setupTypeScript.js
```

Questo crea un progetto iniziale che include il supporto TypeScript, che puoi quindi modificare a tuo piacimento.

Quindi dovrai dire a npm di scaricare le dipendenze e avviare il progetto in modalità sviluppo, come di solito facciamo:

```bash
npm install

npm run dev
```

## Aggiungere il supporto TypeScript a un progetto Svelte esistente

Per aggiungere il supporto TypeScript a un progetto Svelte esistente, puoi [seguire queste istruzioni](https://svelte.dev/blog/svelte-and-typescript#Adding_TypeScript_to_an_existing_project). In alternativa, puoi scaricare il file [`setupTypeScript.js`](https://github.com/sveltejs/template/blob/master/scripts/setupTypeScript.js) in una cartella `scripts` all'interno della directory principale del tuo progetto, e poi eseguire `node scripts/setupTypeScript.js`.

Puoi persino usare `degit` per scaricare lo script. È quello che faremo per iniziare a portare la nostra applicazione a TypeScript.

> [!NOTE]
> Ricorda che puoi eseguire `npx degit opensas/mdn-svelte-tutorial/07-typescript-support svelte-todo-typescript` per ottenere l'applicazione completata della lista delle cose da fare in JavaScript prima di iniziare a portarla a TypeScript.

Vai alla directory principale del progetto ed esegui questi comandi:

```bash
npx degit sveltejs/template/scripts scripts       # download script file to a scripts folder

node scripts/setupTypeScript.js                   # run it
# Converted to TypeScript.
```

Sarà necessario rieseguire il gestore delle dipendenze per iniziare.

```bash
npm install                                       # download new dependencies

npm run dev                                       # start the app in development mode
```

Queste istruzioni si applicano a qualsiasi progetto Svelte che desideri convertire in TypeScript. Tieni in conto che la community di Svelte sta continuamente migliorando il supporto TypeScript di Svelte, quindi dovresti eseguire `npm update` regolarmente per sfruttare le modifiche più recenti.

> [!NOTE]
> Se trovi difficoltà a lavorare con TypeScript all'interno di un'applicazione Svelte, dai un'occhiata a questa [sezione di troubleshooting/FAQ sul supporto TypeScript](https://github.com/sveltejs/language-tools/blob/master/docs/preprocessors/typescript.md#troubleshooting--faq).

Come detto prima, TypeScript è un superset di JavaScript, quindi la tua applicazione funzionerà senza modifiche. Attualmente eseguirai un'applicazione JavaScript regolare con il supporto TypeScript abilitato, senza sfruttare nessuna delle funzionalità che TypeScript offre. Puoi ora iniziare ad aggiungere i tipi progressivamente.

Una volta che hai TypeScript configurato, puoi iniziare ad utilizzarlo da un componente Svelte semplicemente aggiungendo un `<script lang='ts'>` all'inizio della sezione dello script. Per usarlo dai file JavaScript regolari, basta cambiare l'estensione del file da `.js` a `.ts`. Dovrai anche aggiornare tutte le relative dichiarazioni import per rimuovere l'estensione del file `.ts` da tutte le dichiarazioni `import`.

> [!NOTE]
> TypeScript genererà un errore se usi l'estensione del file `.ts` in una dichiarazione `import`, quindi se hai un file `./foo.ts`, devi importarlo come "./foo".
> Vedi la sezione del manuale TypeScript [Risoluzione dei moduli per bundler, runtime TypeScript e loader Node.js](https://www.typescriptlang.org/docs/handbook/modules/theory.html#module-resolution-for-bundlers-typescript-runtimes-and-nodejs-loaders) per ulteriori informazioni.

> [!NOTE]
> L'uso di TypeScript nelle sezioni markup dei componenti non è supportato in Svelte 4, su cui si basa questa guida.
> Quindi, mentre puoi usare JavaScript dal markup, dovrai usare TypeScript nella sezione `<script lang='ts'>`.
> Il TypeScript nel markup dei componenti è consentito da Svelte 5.

## Migliorare l'esperienza dello sviluppatore con TypeScript

TypeScript fornisce ai editor di codice e agli IDE molte informazioni per permettere loro di offrire un'esperienza di sviluppo più amichevole.

Useremo [Visual Studio Code](https://code.visualstudio.com/) per fare un veloce test e vedere come possiamo ottenere suggerimenti di completamento automatico e controllo dei tipi mentre stiamo scrivendo componenti.

> [!NOTE]
> Se non desideri usare VS Code, forniamo anche istruzioni per usare il controllo errori di TypeScript dal terminale, in un secondo momento.

È in corso un lavoro per supportare TypeScript in progetti Svelte in diversi editor di codice; il supporto più completo fino ad ora è disponibile nell'[estensione Svelte per VS Code](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode), che è sviluppata e mantenuta dal team Svelte. Questa estensione offre controllo dei tipi, ispezione, refactoring, intellisense, informazioni sugli hover, completamento automatico, e altre funzionalità. Questo tipo di assistenza allo sviluppatore è un'altra buona ragione per iniziare ad usare TypeScript nei tuoi progetti.

> [!NOTE]
> Assicurati di usare [Svelte per VS Code](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode) e NON il vecchio "Svelte" di James Birtles, che è stato dismesso. Nel caso lo avessi installato, dovresti disinstallarlo e installare l'estensione ufficiale di Svelte invece.

Assumendo che sei all'interno dell'applicazione VS Code, dalla cartella radice del tuo progetto, digita `code .` (il punto finale dice a VS Code di aprire la cartella corrente) per aprire l'editor di codice. VS Code ti dirà che ci sono estensioni consigliate da installare.

![Finestra di dialogo che dice che questo ambiente di lavoro ha estensioni consigliate, con opzioni per installare o mostrare una lista](01-vscode-extension-recommendations.png)

Cliccando su _Installa tutto_ installerai Svelte per VS Code.

![Informazioni sull'estensione Svelte per VS Code](02-svelte-for-vscode.png)

Vediamo anche che il file `setupTypeScript.js` ha apportato un paio di modifiche al nostro progetto. Il file `main.js` è stato rinominato in `main.ts`, il che significa che VS Code può fornire informazioni sugli hover sui nostri componenti Svelte:

![Cattura schermo di VS Code che mostra che quando si passa il cursore su un componente, fornisce suggerimenti](03-vscode-hints-in-main-ts.png)

<!-- cSpell:ignore traget -->

Otteniamo anche il controllo dei tipi gratuitamente. Se passiamo una proprietà sconosciuta nel parametro delle opzioni del costruttore `App` (ad esempio un errore di battitura come `traget` invece di `target`), TypeScript si lamenterà:

![Controllo dei tipi in VS Code - L'oggetto App ha ricevuto una proprietà sconosciuta traget](04-vscode-type-checking-in-main-ts.png)

Nel componente `App.svelte`, lo script `setupTypeScript.js` ha aggiunto l'attributo `lang="ts"` al tag `<script>`. Inoltre, grazie all'inferenza dei tipi, in molti casi non sarà nemmeno necessario specificare i tipi per ottenere assistenza nel codice. Ad esempio, se inizi ad aggiungere una proprietà `ms` alla chiamata del componente `Alert`, TypeScript inferirà dal valore predefinito che la proprietà `ms` dovrebbe essere un numero:

![Inferenza dei tipi in VS Code e assistenza al codice - la variabile ms dovrebbe essere un numero](05-vscode-type-inference-and-code-assistance.png)

E se passi qualcosa che non è un numero, si lamenterà di questo:

![Controllo dei tipi in VS Code - la variabile ms è stata assegnata a un valore non numerico](06-vscode-type-checking-in-components.png)

Il template applicativo ha uno script `check` configurato che esegue `svelte-check` sul tuo codice. Questo pacchetto permette di rilevare errori e avvisi normalmente visualizzati da un editor di codice dalla riga di comando, il che lo rende abbastanza utile per eseguirlo in una pipeline di integrazione continua (CI). Basta eseguire `npm run check` per controllare CSS non utilizzati, e restituire suggerimenti A11y ed errori di compilazione TypeScript.

In questo caso, se esegui `npm run check` (sia nella console di VS Code che nel terminale), otterrai il seguente errore:

![Il comando Check eseguito dentro VS Code mostra un errore di tipo, la variabile ms dovrebbe essere assegnata a un numero](07-vscode-svelte-check.png)

Ancora meglio, se lo esegui dal terminale integrato di VS Code (puoi aprirlo con la scorciatoia da tastiera <kbd>Ctrl</kbd> + <kbd>\`</kbd>), facendo clic su <kbd>Cmd</kbd>/<kbd>Ctrl</kbd> sul nome del file ti porterà alla riga contenente l'errore.

Puoi anche eseguire lo script `check` in modalità watch con `npm run check -- --watch`. In questo caso, lo script verrà eseguito ogni volta che cambi qualsiasi file. Se lo esegui nel tuo terminale regolare, tienilo in esecuzione in background in una finestra di terminale separata in modo che possa continuare a segnalare errori ma non interferirà con l'uso normale del terminale.

## Creare un tipo personalizzato

TypeScript supporta la tipizzazione strutturale. La tipizzazione strutturale è un modo per mettere in relazione i tipi basato esclusivamente sui loro membri, anche se non definisci esplicitamente il tipo.

Definiremo un tipo `TodoType` per vedere come TypeScript impone che tutto ciò che viene passato a un componente che si aspetta un `TodoType` sia compatibile strutturalmente con esso.

1. All'interno della cartella `src` crea una cartella `types`.
2. Aggiungi un file `todo.type.ts` dentro di essa.
3. Dai a `todo.type.ts` il seguente contenuto:

   ```ts
   export type TodoType = {
     id: number;
     name: string;
     completed: boolean;
   };
   ```

   > [!NOTE]
   > Il template di Svelte usa [svelte-preprocess](https://github.com/sveltejs/svelte-preprocess) 4.0.0 per supportare TypeScript. Da quella versione in poi devi usare la sintassi `export`/`import` per importare tipi e interfacce. Controlla [questa sezione della guida di troubleshooting](https://github.com/sveltejs/language-tools/blob/master/docs/preprocessors/typescript.md#how-do-i-import-interfaces-into-my-svelte-components-i-get-errors-after-transpilation) per ulteriori informazioni.

4. Ora useremo `TodoType` dal nostro componente `Todo.svelte`. Prima aggiungi `lang="ts"` al nostro tag `<script>`.
5. Importiamo il tipo e usiamolo per dichiarare la proprietà `todo`. Sostituisci la linea `export let todo` con la seguente:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todo: TodoType;
   ```

   Nota che l'estensione del file `.ts` non è consentita nella dichiarazione di `import`, e è stata omessa.

6. Ora dalla `Todos.svelte` creeremo un'istanza del componente `Todo` con un oggetto letterale come suo parametro prima della chiamata al componente `MoreActions`, così:

   ```svelte
   <hr />

   <Todo todo={ { name: 'a new task with no id!', completed: false } } />

   <!-- MoreActions -->
   <MoreActions {todos}
   ```

7. Aggiungi `lang='ts'` al tag `<script>` del componente `Todos.svelte` in modo che sappia utilizzare il controllo dei tipi che abbiamo specificato.

   Riceveremo il seguente errore:

   ![Errore di tipo in VS Code, l'oggetto Todo Type richiede una proprietà id.](08-vscode-structural-typing.png)

A questo punto dovresti avere un'idea dell'assistenza che possiamo ottenere da TypeScript quando creiamo progetti Svelte.

Ora annulleremo queste modifiche per iniziare a portare la nostra applicazione a TypeScript, quindi non saremo disturbati da tutti gli avvisi di controllo.

1. Rimuovi il todo errato e l'attributo `lang='ts'` dal file `Todos.svelte`.
2. Rimuovi anche l'importazione di `TodoType` e `lang='ts'` da `Todo.svelte`.

Ce ne occuperemo correttamente più avanti.

## Porting della nostra app to-do list a TypeScript

Ora siamo pronti per iniziare a portare la nostra applicazione di lista delle cose da fare per sfruttare tutte le funzionalità che TypeScript ci offre.

Iniziamo eseguendo lo script di controllo in modalità watch nella radice del progetto:

```bash
npm run check -- --watch
```

Questo dovrebbe restituire qualcosa del tipo:

```bash
svelte-check "--watch"

Loading svelte-check in workspace: ./svelte-todo-typescript
Getting Svelte diagnostics...
====================================
svelte-check found no errors and no warnings
```

Nota che se stai utilizzando un editor di codice supportato come VS Code, un modo semplice per iniziare a portare un componente Svelte è semplicemente aggiungere `<script lang='ts'>` nella parte superiore del componente e cercare i suggerimenti con tre punti:

![Cattura schermo di VS Code che mostra che quando si aggiunge type="ts" a un componente, fornisce suggerimenti con tre punti di allerta](09-vscode-alert-hints.png)

### Alert.svelte

Iniziamo con il nostro componente `Alert.svelte`.

1. Aggiungi `lang="ts"` nel tag `<script>` del componente `Alert.svelte`. Vedrai alcuni avvertimenti nell'output dello script di controllo:

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

2. Puoi risolverli specificando i corrispettivi tipi, così:

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
   > Non c'è bisogno di specificare il tipo di `ms` con `export let ms:number = 3000`, perché TypeScript lo sta già inferendo dal suo valore predefinito.

### MoreActions.svelte

Ora faremo lo stesso per il componente `MoreActions.svelte`.

1. Aggiungi l'attributo `lang='ts'`, come prima. TypeScript ci avviserà riguardo alla prop `todos` e alla variabile `t` nella chiamata a `todos.filter((t) =>...)`.

   ```plain
   Warn: Variable 'todos' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     export let todos

   Warn: Parameter 't' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     $: completedTodos = todos.filter((t) => t.completed).length
   ```

2. Utilizzeremo il `TodoType` che abbiamo già definito per dire a TypeScript che `todos` è un array di `TodoType`. Sostituisci la linea `export let todos` con la seguente:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todos: TodoType[];
   ```

Nota che ora TypeScript può inferire che la variabile `t` in `todos.filter((t) => t.completed)` sia di tipo `TodoType`. Tuttavia, se pensiamo che renda il nostro codice più facile da leggere, potremmo specificarlo così:

```ts
$: completedTodos = todos.filter((t: TodoType) => t.completed).length;
```

La maggior parte delle volte, TypeScript sarà in grado di inferire correttamente il tipo di variabile reattiva, ma a volte potresti ricevere un errore "implicitamente ha un tipo 'any'" quando lavori con assegnazioni reattive. In quei casi, puoi dichiarare la variabile con tipo in un'istruzione diversa, così:

```ts
let completedTodos: number;
$: completedTodos = todos.filter((t: TodoType) => t.completed).length;
```

Non puoi specificare il tipo nell'assegnazione reattiva stessa. L'istruzione `$: completedTodos: number = todos.filter[...]` è non valida. Per ulteriori informazioni, leggi [Come faccio a digitare le assegnazioni reattive? / Ricevo un errore "implicitamente ha tipo 'any'"] (https://github.com/sveltejs/language-tools/blob/master/docs/preprocessors/typescript.md#how-do-i-type-reactive-assignments--i-get-an-implicitly-has-type-any-error).

### FilterButton.svelte

Ora ci occuperemo del componente `FilterButton`.

1. Aggiungi l'attributo `lang='ts'` al tag `<script>`, come al solito. Noterai che non ci sono avvertimenti — TypeScript inferisce il tipo della variabile filter dal valore predefinito. Ma sappiamo che ci sono solo tre valori validi per il filter: all, active e completed. Quindi possiamo far sapere a TypeScript di loro creando un enum Filter.
2. Crea un file `filter.enum.ts` nella cartella `types`.
3. Dalle il seguente contenuto:

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

Qui stiamo solo importando l'enum `Filter` e usandolo invece dei valori stringa che usavamo precedentemente.

### Todos.svelte

Useremo anche l'enum `Filter` nel componente `Todos.svelte`.

1. Prima, aggiungi l'attributo `lang='ts'`, come prima.
2. Successivamente, importa l'enum `Filter`. Aggiungi la seguente istruzione `import` sotto le tue istruzioni esistenti:

   ```js
   import { Filter } from "../types/filter.enum";
   ```

3. Ora lo useremo ogni volta che facciamo riferimento al filtro corrente. Sostituisci i due blocchi correlati al filter con i seguenti:

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

4. `check` ci darà ancora alcuni avvertimenti da `Todos.svelte`. Risolviamoli.

   Inizia importando `TodoType` e dicendo a TypeScript che la nostra variabile `todos` è un array di `TodoType`. Sostituisci `export let todos = []` con le seguenti due righe:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todos: TodoType[] = [];
   ```

5. Successivamente specificheremo tutti i tipi mancanti. La variabile `todosStatus`, che abbiamo usato per accedere programmaticamente ai metodi esposti dal componente `TodosStatus`, è di tipo `TodosStatus`. E ogni `todo` sarà di tipo `TodoType`.

   Aggiorna la tua sezione `<script>` per apparire così:

   ```ts
   import FilterButton from "./FilterButton.svelte";
   import Todo from "./Todo.svelte";
   import MoreActions from "./MoreActions.svelte";
   import NewTodo from "./NewTodo.svelte";
   import TodosStatus from "./TodosStatus.svelte";
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

Questo perché la prop `todos` nel componente `TodosStatus` non ha un valore predefinito, quindi TypeScript l'ha inferita come di tipo `undefined`, il che non è compatibile con un array di `TodoType`. La stessa cosa sta accadendo con il nostro componente Todo.

Risolviamolo.

1. Apri il file `TodosStatus.svelte` e aggiungi l'attributo `lang='ts'`.
2. Poi importa `TodoType` e dichiara la prop `todos` come un array di `TodoType`. Sostituisci la prima riga della sezione `<script>` con la seguente:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todos: TodoType[];
   ```

3. Specificheremo anche l'`headingEl`, che abbiamo usato per fare il bind al tag heading, come un `HTMLElement`. Aggiorna la riga `let headingEl` con la seguente:

   ```ts
   let headingEl: HTMLElement;
   ```

4. Infine, noterai l'errore riportato relativo a dove impostiamo l'attributo `tabindex`. Questo perché TypeScript sta verificando il tipo dell'elemento `<h2>` e si aspetta che `tabindex` sia di tipo `number`.

   ![Suggerimento della scheda all'interno di VS Code, tabindex si aspetta un tipo number, non string](10-vscode-tabindex-hint.png)

   Per risolverlo, sostituisci `tabindex="-1"` con `tabindex={-1}`, così:

   ```svelte
   <h2 id="list-heading" bind:this={headingEl} tabindex={-1}>
     {completedTodos} out of {totalTodos} items completed
   </h2>
   ```

   In questo modo TypeScript può impedirci di assegnarlo erroneamente a una variabile stringa.

### NewTodo.svelte

Ora ci occuperemo di `NewTodo.svelte`.

1. Come al solito, aggiungi l'attributo `lang='ts'`.
2. L'avviso ci dirà che dobbiamo specificare un tipo per la variabile `nameEl`. Imposta il suo tipo a `HTMLElement` così:

   ```ts
   let nameEl: HTMLElement; // reference to the name input DOM node
   ```

3. Infine per questo file, dobbiamo specificare il tipo corretto per la nostra variabile `autofocus`. Aggiorna la sua definizione così:

   ```ts
   export let autofocus: boolean = false;
   ```

### Todo.svelte

Ora gli unici avvisi che `npm run check` emette sono generati dalla chiamata del componente `Todo.svelte`. Risolviamoli.

1. Apri il file `Todo.svelte`, e aggiungi l'attributo `lang='ts'`.
2. Importiamo `TodoType` e impostiamo il tipo della prop `todo`. Sostituisci la riga `export let todo` con la seguente:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todo: TodoType;
   ```

3. Il primo avviso che otteniamo è TypeScript che ci dice di definire il tipo della variabile `updatedTodo` della funzione `update()`. Questo può essere un po' complicato perché `updatedTodo` contiene solo gli attributi del `todo` che sono stati aggiornati. Significa che non è un `todo` completo — ha solo un sottoinsieme delle proprietà di un `todo`.

   Per questi tipi di casi, TypeScript fornisce diversi [utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html) per semplificare l'applicazione di queste trasformazioni comuni. Ciò di cui abbiamo bisogno in questo momento è l'utility [`Partial<T>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialt), che ci consente di rappresentare tutti i sottinsiemi di un determinato tipo. Il tipo parziale restituisce un nuovo tipo basato su `T`, dove ogni proprietà di `T` è opzionale.

   Lo useremo nella funzione `update()` — aggiornala così:

   ```ts
   function update(updatedTodo: Partial<TodoType>) {
     todo = { ...todo, ...updatedTodo }; // applies modifications to todo
     dispatch("update", todo); // emit update event
   }
   ```

   Con questo stiamo dicendo a TypeScript che la variabile `updatedTodo` conterrà un sottoinsieme delle proprietà di `TodoType`.

4. Ora svelte-check ci dice che dobbiamo definire il tipo dei parametri della funzione action:

   ```bash
   ./07-next-steps/src/components/Todo.svelte:45:24
   Warn: Parameter 'node' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     const focusOnInit = (node) => node && typeof node.focus === 'function' && node.focus()

   ./07-next-steps/src/components/Todo.svelte:47:28
   Warn: Parameter 'node' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     const focusEditButton = (node) => editButtonPressed && node.focus()
   ```

   Dobbiamo solo definire la variabile nodo come di tipo `HTMLElement`. Nelle due righe indicate sopra, sostituisci la prima istanza di `node` con `node: HTMLElement`.

### actions.js

Successivamente ci occuperemo del file `actions.js`.

1. Rinominalo in `actions.ts` e aggiungi il tipo del parametro nodo. Dovrebbe risultare così:

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

2. Ora aggiorna `Todo.svelte` e `NewTodo.svelte` dove importiamo il file actions. Ricorda che le importazioni in TypeScript non includono l'estensione del file. In entrambi i casi dovrebbe risultare così:

   ```js
   import { selectOnFocus } from "../actions";
   ```

### Migrare gli store a TypeScript

Ora dobbiamo migrare i file `stores.js` e `localStore.js` a TypeScript.

Suggerimento: lo script `npm run check`, che utilizza lo strumento [`svelte-check`](https://github.com/sveltejs/language-tools/tree/master/packages/svelte-check), verificherà solo i file `.svelte` della nostra applicazione. Se vuoi anche controllare i file `.ts`, puoi eseguire `npm run check && npx tsc --noEmit`, che dice al compilatore TypeScript di verificare gli errori senza generare i file di output `.js`. Potresti persino aggiungere uno script al tuo file `package.json` che esegue quel comando.

Inizieremo con `stores.js`.

1. Rinomina il file in `stores.ts`.
2. Imposta il tipo del nostro array `initialTodos` a `TodoType[]`. Questo è come risulteranno i contenuti:

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

3. Ricorda di aggiornare le dichiarazioni di `import` in `App.svelte`, `Alert.svelte`, e `Todos.svelte`. Basta rimuovere l'estensione `.js`, così:

   ```js
   import { todos } from "../stores";
   ```

Ora passiamo a `localStore.js`.

Aggiorna la dichiarazione `import` in `stores.ts` in questo modo:

```js
import { localStore } from "./localStore";
```

1. Inizia rinominando il file in `localStore.ts`.
2. TypeScript ci dice di specificare il tipo delle variabili `key`, `initial` e `value`. La prima è facile: la chiave del nostro web storage locale dovrebbe essere una stringa.

   Ma `initial` e `value` dovrebbero essere un qualsiasi oggetto che potrebbe essere convertito a una stringa JSON valida con il metodo [`JSON.stringify`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify), il che significa qualsiasi oggetto JavaScript con un paio di limitazioni: per esempio, `undefined`, le funzioni e i simboli non sono valori JSON validi.

   Quindi creeremo il tipo `JsonValue` per specificare queste condizioni.

   Crea il file `json.type.ts` nella cartella `types`.

3. Dalle il seguente contenuto:

   ```ts
   export type JsonValue =
     | string
     | number
     | boolean
     | null
     | JsonValue[]
     | { [key: string]: JsonValue };
   ```

   L'operatore `|` ci consente di dichiarare variabili che potrebbero memorizzare valori di due o più tipi. Un `JsonValue` potrebbe essere una stringa, un numero, un booleano, e così via. In questo caso stiamo anche utilizzando tipi ricorsivi per specificare che un `JsonValue` può avere un array di `JsonValue` e anche un oggetto con proprietà di tipo `JsonValue`.

4. Importeremo il nostro tipo `JsonValue` e lo useremo di conseguenza. Aggiorna il tuo file `localStore.ts` così:

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

Ora se proviamo a creare un `localStore` con qualcosa che non può essere convertito in JSON tramite `JSON.stringify()`, per esempio un oggetto con una funzione come proprietà, VS Code/`validate` lo segnalerà in errore:

![VS Code che mostra un errore con l'uso del nostro store — fallisce quando si cerca di impostare un valore di storage locale a qualcosa di incompatibile con JSON stringify](11-vscode-invalid-store.png)

E il meglio di tutto, funzionerà anche con la sintassi di iscrizione automatica di [`$store`](https://svelte.dev/docs/svelte-components#script-4-prefix-stores-with-$-to-access-their-values). Se proviamo a salvare un valore non valido nel nostro store `todos` usando la sintassi `$store`, così:

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

Questo è un altro esempio di come la specificazione dei tipi possa rendere il nostro codice più robusto e aiutarci a rilevare più bug prima che finiscano in produzione.

E questo è tutto. Abbiamo convertito tutta la nostra applicazione per utilizzare TypeScript.

## Rendere i nostri store a prova di errore con i Generics

I nostri store sono già stati portati a TypeScript, ma possiamo fare di meglio. Non dovremmo aver bisogno di memorizzare alcun tipo di valore — sappiamo che l'alert store dovrebbe contenere messaggi di stringa, e lo store dei todo dovrebbe contenere un array di `TodoType`, ecc. Possiamo lasciare che TypeScript lo imponga usando [i generici di TypeScript](https://www.typescriptlang.org/docs/handbook/generics.html). Scopriamone di più.

### Comprendere i generici TypeScript

I generici ti permettono di creare componenti di codice riutilizzabili che funzionano con una varietà di tipi anziché con un singolo tipo. Possono essere applicati a interfacce, classi e funzioni. I tipi generici sono passati come parametri usando una sintassi speciale: sono specificati all’interno di parentesi angolari e sono convenzionalmente denotati con una singola lettera maiuscola. I tipi generici ti permettono di catturare i tipi forniti dall'utente, garantendo che siano disponibili per l'elaborazione futura.

Vediamo un semplice esempio, una classe `Stack` semplice che ci permette di `push` e `pop` elementi, così:

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

In questo caso `elements` è un array di tipo `any`, e di conseguenza i metodi `push()` e `pop()` ricevono e restituiscono entrambi una variabile di tipo `any`. Quindi è perfettamente valido fare qualcosa del tipo:

```js
const anyStack = new Stack();

anyStack.push(1);
anyStack.push("hello");
```

Ma cosa succede se vogliamo avere uno `Stack` che funziona solo con il tipo `string`? Potremmo fare quanto segue:

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

Funzionerebbe. Ma se volessimo lavorare con i numeri, dovremmo quindi duplicare il nostro codice e creare una classe `NumberStack`. E come potremmo gestire uno stack di tipi che non conosciamo ancora e che dovrebbero essere definiti dal consumatore?

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

Definiamo un tipo generico `T` e poi usiamo come useremmo normalmente un tipo specifico. Ora elements è un array di tipo `T`, e `push()` e `pop()` ricevono e restituiscono entrambi una variabile di tipo `T`.

Ecco come useremmo il nostro `Stack` generico:

```ts
const numberStack = new Stack<number>();
numberStack.push(1);
```

Ora TypeScript sa che il nostro stack può accettare solo numeri, e segnalerà un errore se proviamo a inserire qualsiasi altra cosa:

![L'argomento di tipo hello non è assegnabile al parametro di tipo number](12-vscode-generic-stack-error.png)

TypeScript può anche dedurre i tipi generici dal suo utilizzo. I generici supportano anche i valori predefiniti e i vincoli.

I generici sono una potente funzionalità che consente al nostro codice di astrarsi dai tipi specifici utilizzati, rendendolo più riutilizzabile e generico senza rinunciare alla sicurezza dei tipi. Per saperne di più, consulta l'[Introduzione ai generici in TypeScript](https://www.typescriptlang.org/docs/handbook/generics.html).

### Usare gli store Svelte con i generici

Gli store Svelte supportano i generics di default. E, grazie all'inferenza dei tipi generici, possiamo trarne vantaggio senza nemmeno toccare il nostro codice.

Se apri il file `Todos.svelte` e assegni un tipo `number` al nostro store `$alert`, otterrai il seguente errore:

![L'argomento di tipo 9999 non è assegnabile al parametro di tipo string](13-vscode-generic-alert-error.png)

Questo perché quando abbiamo definito il nostro alert store nel file `stores.ts` con:

```js
export const alert = writable("Welcome to the To-Do list app!");
```

TypeScript ha inferito il tipo generico come `string`. Se volessimo essere espliciti riguardo a questo, potremmo fare quanto segue:

```ts
export const alert = writable<string>("Welcome to the To-Do list app!");
```

Ora faremo in modo che il nostro store `localStore` supporti i generics. Ricorda che abbiamo definito il tipo `JsonValue` per prevenire l'uso del nostro store `localStore` con valori che non possono essere persistiti usando `JSON.stringify()`. Ora vogliamo che i consumatori di `localStore` siano in grado di specificare il tipo di dati da persistere, ma invece di lavorare con qualsiasi tipo, dovrebbero conformarsi al tipo `JsonValue`. Lo specificheremo con un vincolo generico, così:

```ts
export const localStore = <T extends JsonValue>(key: string, initial: T) => {
  // …
};
```

Definiamo un tipo generico `T` e specifichiamo che deve essere compatibile con il tipo `JsonValue`. Poi useremo il tipo `T` opportunamente.

Il nostro file `localStore.ts` risulterà così — prova il nuovo codice nel tuo progetto:

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

E grazie all'inferenza del tipo generico, TypeScript sa già che il nostro store `$todos` dovrebbe contenere un array di `TodoType`:

![La proprietà dell'oggetto Todo Type "complete" dovrebbe essere "completed"](14-vscode-generic-localstore-error.png)

Ancora una volta, se volessimo essere espliciti a riguardo, potremmo farlo nel file `stores.ts` in questo modo:

```ts
const initialTodos: TodoType[] = [
  { id: 1, name: "Visit MDN web docs", completed: true },
  { id: 2, name: "Complete the Svelte Tutorial", completed: false },
];

export const todos = localStore<TodoType[]>("mdn-svelte-todo", initialTodos);
```

Questo sarà quanto per il nostro breve tour dei Generici di TypeScript.

## Il codice finora

### Git

Per vedere lo stato del codice come dovrebbe essere alla fine di questo articolo, accedi alla tua copia del nostro repo come segue:

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

## Sommario

In questo articolo abbiamo preso la nostra applicazione to-do list e l'abbiamo portata a TypeScript.

Abbiamo prima appreso di TypeScript e quali vantaggi può portarci. Poi abbiamo visto come creare un nuovo progetto Svelte con supporto TypeScript. Abbiamo anche visto come convertire un progetto Svelte esistente per utilizzare TypeScript — la nostra app todo list.

Abbiamo visto come lavorare con [Visual Studio Code](https://code.visualstudio.com/) e l'[estensione Svelte](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode) per ottenere funzionalità come controllo dei tipi e completamento automatico. Abbiamo anche utilizzato lo strumento `svelte-check` per ispezionare i problemi di TypeScript dalla riga di comando.

Nel prossimo articolo impareremo come compilare e distribuire la nostra app per la produzione. Vedremo anche quali risorse sono disponibili online per approfondire l'apprendimento di Svelte.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_stores","Learn_web_development/Core/Frameworks_libraries/Svelte_deployment_next", "Learn_web_development/Core/Frameworks_libraries")}}
