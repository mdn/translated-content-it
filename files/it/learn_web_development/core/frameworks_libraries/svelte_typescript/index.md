---
title: Supporto TypeScript in Svelte
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_TypeScript
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_stores","Learn_web_development/Core/Frameworks_libraries/Svelte_deployment_next", "Learn_web_development/Core/Frameworks_libraries")}}

Nell'ultimo articolo abbiamo appreso dei negozi in Svelte e abbiamo persino implementato il nostro negozio personalizzato per persistere le informazioni dell'app in Web Storage. Abbiamo anche esaminato l'uso della direttiva di transizione per implementare animazioni su elementi DOM in Svelte.

Apprenderemo ora come utilizzare TypeScript nelle applicazioni Svelte. Innanzitutto, impareremo cos'è TypeScript e quali benefici ci può offrire. Poi vedremo come configurare il nostro progetto per lavorare con file TypeScript. Infine, esamineremo la nostra app e vedremo quali modifiche dobbiamo fare per sfruttare al massimo le funzionalità di TypeScript.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          È consigliato, almeno, che abbia familiarità con i linguaggi di base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          abbia conoscenze dell'
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminal/command line</a
          >.
        </p>
        <p>
          Avrà bisogno di un terminale con Node e npm installati per compilare e costruire la sua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare a configurare e utilizzare TypeScript quando sviluppa applicazioni Svelte.
      </td>
    </tr>
  </tbody>
</table>

Si noti che la nostra applicazione è completamente funzionante, e portarla a TypeScript è completamente opzionale. Ci sono opinioni diverse a riguardo, e in questo capitolo parleremo brevemente dei pro e contro dell'utilizzo di TypeScript. Anche se non ha intenzione di adottarlo, questo articolo sarà utile per permetterle di apprendere cosa offre e aiutare a prendere una decisione autonoma. Se non è per niente interessato a TypeScript, può passare al capitolo successivo, dove esamineremo diverse opzioni per distribuire le nostre applicazioni Svelte, ulteriori risorse, e altro ancora.

## Segua il codice insieme a noi

### Git

Cloni il repo GitHub (se non l'ha già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi, per raggiungere lo stato attuale dell'app, esegua

```bash
cd mdn-svelte-tutorial/07-typescript-support
```

Oppure scarichi direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/07-typescript-support
```

Ricordi di eseguire `npm install && npm run dev` per avviare la sua app in modalità sviluppo.

### REPL

Purtroppo, [Il supporto TypeScript non è ancora disponibile nel REPL](https://github.com/sveltejs/svelte.dev/issues/853).

## TypeScript: tipizzazione statica opzionale per JavaScript

[TypeScript](https://www.typescriptlang.org/) è un superset di JavaScript che offre caratteristiche come tipizzazione statica opzionale, classi, interfacce e generici. L'obiettivo di TypeScript è aiutare a cogliere gli errori in anticipo attraverso il suo sistema di tipi e rendere lo sviluppo JavaScript più efficiente. Uno dei grandi benefici è permettere agli IDE di fornire un ambiente più ricco per individuare errori comuni mentre si digita il codice.

Il migliore di tutto, il codice JavaScript è codice TypeScript valido; TypeScript è un superset di JavaScript. Può rinominare la maggior parte dei suoi file `.js` in file `.ts` e funzioneranno.

Il nostro codice TypeScript sarà in grado di essere eseguito ovunque il JavaScript può essere eseguito. Come è possibile? TypeScript "traspila" il nostro codice in JavaScript puro. Questo significa che analizza il codice TypeScript e produce l'equivalente codice JavaScript puro per i browser.

> [!NOTE]
> Se è curioso di sapere come TypeScript traspila il nostro codice in JavaScript, può dare un'occhiata al [Tipo di giocattolo TypeScript](https://www.typescriptlang.org/play/?target=1&e=4#example/hello-world).

Il supporto TypeScript di primo livello è stato la caratteristica più richiesta di Svelte per diverso tempo. Grazie al duro lavoro del team Svelte, insieme a molti collaboratori, hanno una [soluzione ufficiale](https://svelte.dev/blog/svelte-and-typescript) pronta da mettere alla prova. In questa sezione le mostreremo come impostare un progetto Svelte con supporto TypeScript per provarlo.

## Perché TypeScript?

I principali vantaggi di TypeScript sono:

- Bug individuati precocemente: il compilatore controlla i tipi in fase di compilazione e fornisce report di errore.
- Leggibilità: la tipizzazione statica dà al codice una maggiore struttura, rendendolo auto-documentante e più leggibile.
- Ricco supporto IDE: le informazioni sui tipi permettono agli editor di codice e agli IDE di offrire funzionalità come la navigazione del codice, l'autocompletamento e suggerimenti più intelligenti.
- Refactoring più sicuro: i tipi permettono agli IDE di sapere di più sul suo codice e di assisterla durante il refactoring di ampie porzioni del suo codice.
- Inferenza dei tipi: le permette di usufruire di molte funzionalità di TypeScript anche senza dichiarare i tipi delle variabili.
- Disponibilità di funzionalità JavaScript nuove e future: TypeScript trasforma molte recenti funzionalità JavaScript in semplice JavaScript scolastico, permettendole di utilizzarle anche su user-agents che non le supportano ancora nativamente.

TypeScript ha anche alcuni svantaggi:

- Non vera tipizzazione statica: i tipi sono controllati solo in fase di compilazione e sono rimossi dal codice generato.
- Curva di apprendimento ripida: anche se TypeScript è un superset di JavaScript e non un linguaggio completamente nuovo, c'è una notevole curva di apprendimento, specialmente se non ha esperienza con linguaggi statici come Java o C#.
- Più codice: deve scrivere e mantenere più codice.
- Nessun sostituto per i test automatici: anche se i tipi potrebbero aiutarla a cogliere diversi bug, TypeScript non è un vero sostituto per una suite completa di test automatizzati.
- Codice boilerplate: lavorare con tipi, classi, interfacce e generici può portare a basi di codice eccessivamente ingegnerizzate.

Sembra esserci un ampio consenso sul fatto che TypeScript sia particolarmente adatto per progetti su larga scala, con molti sviluppatori che lavorano sullo stesso codice. Ed è infatti utilizzato in diversi progetti su larga scala, come Angular 2, Vue 3, Ionic, Visual Studio Code, Jest, e persino il compilatore Svelte. Tuttavia, alcuni sviluppatori preferiscono utilizzarlo anche su piccoli progetti come quello che stiamo sviluppando.

Alla fine, è una sua decisione. Nelle sezioni seguenti speriamo di fornirle più prove per aiutarla a decidere al riguardo.

## Creare un progetto Svelte TypeScript da zero

Può iniziare un nuovo progetto Svelte TypeScript usando il [modello standard](https://github.com/sveltejs/template). Tutto ciò che deve fare è eseguire i seguenti comandi del terminale (li esegua in un luogo in cui sta archiviando i suoi progetti di prova Svelte — crea una nuova directory):

```bash
npx degit sveltejs/template svelte-typescript-app

cd svelte-typescript-app

node scripts/setupTypeScript.js
```

Questo crea un progetto starter che include il supporto TypeScript, che può quindi modificare a suo piacimento.

Poi dovrà dire a npm di scaricare le dipendenze e avviare il progetto in modalità sviluppo, come facciamo di solito:

```bash
npm install

npm run dev
```

## Aggiungere supporto TypeScript a un progetto Svelte esistente

Per aggiungere supporto TypeScript a un progetto Svelte esistente, può [seguire queste istruzioni](https://svelte.dev/blog/svelte-and-typescript#Adding_TypeScript_to_an_existing_project). In alternativa, può scaricare il file [`setupTypeScript.js`](https://github.com/sveltejs/template/blob/master/scripts/setupTypeScript.js) in una cartella `scripts` all'interno della cartella radice del suo progetto, quindi eseguire `node scripts/setupTypeScript.js`.

Può persino usare `degit` per scaricare lo script. Questo è quello che faremo per iniziare a portare la nostra applicazione su TypeScript.

> [!NOTE]
> Ricordi che può eseguire `npx degit opensas/mdn-svelte-tutorial/07-typescript-support svelte-todo-typescript` per ottenere l'applicazione completa della lista delle cose da fare in JavaScript prima di iniziare a portarla su TypeScript.

Vada nella directory radice del progetto ed esegua questi comandi:

```bash
npx degit sveltejs/template/scripts scripts       # download script file to a scripts folder

node scripts/setupTypeScript.js                   # run it
# Converted to TypeScript.
```

Dovrà nuovamente eseguire il gestore delle dipendenze per iniziare.

```bash
npm install                                       # download new dependencies

npm run dev                                       # start the app in development mode
```

Queste istruzioni si applicano a qualsiasi progetto Svelte che desideri convertire in TypeScript. Tenga presente che la comunità Svelte migliora costantemente il supporto TypeScript di Svelte, quindi dovrebbe eseguire `npm update` regolarmente per sfruttare gli ultimi cambiamenti.

> [!NOTE]
> Se riscontra problemi nel lavorare con TypeScript all'interno di un'applicazione Svelte, dia un'occhiata a questa [sezione di risoluzione dei problemi/FAQ sul supporto TypeScript](https://github.com/sveltejs/language-tools/blob/master/docs/preprocessors/typescript.md#troubleshooting--faq).

Come abbiamo detto prima, TypeScript è un superset di JavaScript, quindi la sua applicazione funzionerà senza modifiche. Attualmente eseguirà un'applicazione JavaScript regolare con supporto TypeScript abilitato, senza sfruttare alcuna delle funzionalità che TypeScript offre. Può ora iniziare ad aggiungere tipi progressivamente.

Una volta configurato TypeScript, può iniziare ad utilizzarlo da un componente Svelte semplicemente aggiungendo `<script lang='ts'>` all'inizio della sezione script. Per usarlo da file JavaScript regolari, basta cambiare l'estensione del file da `.js` a `.ts`. Dovrà anche aggiornare qualsiasi istruzione di importazione corrispondente per rimuovere l'estensione del file `.ts` da tutte le istruzioni `import`.

> [!NOTE]
> TypeScript restituirà un errore se usa l'estensione del file `.ts` in un'istruzione `import`, quindi se ha un file `./foo.ts`, deve importarlo come "./foo".
> Veda la sezione [Risoluzione dei moduli per bundler, runtime TypeScript e caricamenti Node.js](https://www.typescriptlang.org/docs/handbook/modules/theory.html#module-resolution-for-bundlers-typescript-runtimes-and-nodejs-loaders) del manuale TypeScript per ulteriori informazioni.

> [!NOTE]
> L'uso di TypeScript nelle sezioni di markup del componente non è supportato in Svelte 4, su cui si basa questa guida.
> Quindi, mentre può usare JavaScript dal markup, dovrà usare TypeScript nella sezione `<script lang='ts'>`.
> TypeScript nel markup del componente è consentito da Svelte 5.

## Esperienza di sviluppo migliorata con TypeScript

TypeScript fornisce agli editor di codice ed agli IDE molte informazioni per consentire loro di offrire un'esperienza di sviluppo più amichevole.

Useremo [Visual Studio Code](https://code.visualstudio.com/) per fare un rapido test e vedere come possiamo ottenere suggerimenti di completamento automatico e controllo dei tipi mentre scriviamo i componenti.

> [!NOTE]
> Se non desidera usare VS Code, forniamo anche istruzioni per utilizzare il controllo degli errori TypeScript dal terminale poco più avanti.

È in corso di supporto TypeScript nei progetti Svelte in diversi editor di codice; il supporto più completo finora è disponibile nell'[estensione Svelte per VS Code](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode), che è sviluppata e mantenuta dal team Svelte. Questa estensione offre controllo dei tipi, ispezione, refactoring, intellisense, informazioni sui comandi, auto-completamento e altre funzionalità. Questo tipo di assistenza agli sviluppatori è un'altra buona ragione per iniziare a usare TypeScript nei suoi progetti.

> [!NOTE]
> Assicuratevi di utilizzare [Svelte per VS Code](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode) e NON il vecchio "Svelte" di James Birtles, che è stato dismesso. In caso l'avesse installato, dovrebbe disinstallarlo ed installare l'estensione ufficiale Svelte invece.

Assumendo che si trovi all'interno dell'applicazione VS Code, dalla radice della cartella del progetto, digiti `code .` (il punto finale indica a VS Code di aprire la cartella corrente) per aprire l'editor di codice. VS Code le dirà che ci sono estensioni raccomandate da installare.

![Finestra di dialogo che dice che questo workspace ha raccomandazioni di estensione, con opzioni per installare o mostrare un elenco](01-vscode-extension-recommendations.png)

Facendo clic su _Installa tutto_ installerà Svelte per VS Code.

![Informazioni sull'estensione Svelte per VS Code](02-svelte-for-vscode.png)

Possiamo anche vedere che il file `setupTypeScript.js` ha fatto alcune modifiche al nostro progetto. Il file `main.js` è stato rinominato in `main.ts`, il che significa che VS Code può fornire informazioni sugli hover sui nostri componenti Svelte:

![Screenshot di VS Code che mostra che quando si passa il mouse su un componente, fornisce suggerimenti](03-vscode-hints-in-main-ts.png)

<!-- cSpell:ignore traget -->

Otteniamo anche il controllo dei tipi gratuitamente. Se passiamo una proprietà sconosciuta nel parametro options del costruttore `App` (ad esempio un errore di battitura come `traget` invece di `target`), TypeScript si lamenterà:

![Controllo dei tipi in VS Code - l'oggetto App è stato dato una proprietà sconosciuta traget](04-vscode-type-checking-in-main-ts.png)

Nel componente `App.svelte`, lo script `setupTypeScript.js` ha aggiunto l'attributo `lang="ts"` al tag `<script>`. Inoltre, grazie all'inferenza dei tipi, in molti casi non sarà nemmeno necessario specificare i tipi per ottenere assistenza nel codice. Ad esempio, se inizia ad aggiungere una proprietà `ms` alla chiamata del componente `Alert`, TypeScript dedurrà dal valore predefinito che la proprietà `ms` dovrebbe essere un numero:

![Inferenza dei tipi e suggerimenti per il codice di VS Code - la variabile ms dovrebbe essere un numero](05-vscode-type-inference-and-code-assistance.png)

E se passa qualcosa che non è un numero, esso si lamenterà:

![Controllo dei tipi in VS Code - la variabile ms è stata data un valore non numerico](06-vscode-type-checking-in-components.png)

Il modello dell'applicazione ha uno script `check` configurato che esegue `svelte-check` sul suo codice. Questo pacchetto le permette di rilevare errori e avvertimenti normalmente visualizzati da un editor di codice dalla linea di comando, il che lo rende piuttosto utile per eseguirlo in una pipeline di integrazione continua (CI). Basta eseguire `npm run check` per controllare CSS inutilizzati, e restituire suggerimenti A11y ed errori di compilazione TypeScript.

In questo caso, se esegue `npm run check` (sia nella console VS Code che nel terminale), otterrà il seguente errore:

![Comando di controllo eseguito all'interno di VS Code che mostra un errore di tipo, la variabile ms dovrebbe essere assegnata a un numero](07-vscode-svelte-check.png)

Ancora meglio, se lo esegue dal terminale integrato VS Code (può aprirlo con il collegamento da tastiera <kbd>Ctrl</kbd> + <kbd>\`</kbd>), fare clic sinistro su <kbd>Cmd</kbd>/<kbd>Ctrl</kbd> sul nome del file la porterà alla riga che contiene l'errore.

Può anche eseguire lo script `check` in modalità watch con `npm run check -- --watch`. In tal caso, lo script sarà eseguito ogni volta che modifica un file. Se lo sta eseguendo nel suo terminale regolare, lo mantenga in esecuzione in background in una finestra del terminale separata in modo che possa continuare a segnalare errori ma non interferisca con altri usi del terminale.

## Creare un tipo personalizzato

TypeScript supporta la tipizzazione strutturale. La tipizzazione strutturale è un modo di relazionare i tipi basato esclusivamente sui loro membri, anche se non definisce esplicitamente il tipo.

Definiremo un tipo `TodoType` per vedere come TypeScript fa rispettare che tutto ciò che viene passato a un componente che si aspetta un `TodoType` sarà strutturalmente compatibile con esso.

1. All'interno della cartella `src` crei una cartella `types`.
2. Aggiunga un file `todo.type.ts` dentro di essa.
3. Fornisca al file `todo.type.ts` il seguente contenuto:

   ```ts
   export type TodoType = {
     id: number;
     name: string;
     completed: boolean;
   };
   ```

   > [!NOTE]
   > Il template Svelte utilizza [svelte-preprocess](https://github.com/sveltejs/svelte-preprocess) 4.0.0 per supportare TypeScript. Da quella versione in poi deve usare la sintassi `export`/`import` type per importare tipi e interfacce. Controlli [questa sezione della guida di risoluzione dei problemi](https://github.com/sveltejs/language-tools/blob/master/docs/preprocessors/typescript.md#how-do-i-import-interfaces-into-my-svelte-components-i-get-errors-after-transpilation) per ulteriori informazioni.

4. Ora useremo `TodoType` dal nostro componente `Todo.svelte`. Prima aggiunga il `lang="ts"` al nostro tag `<script>`.
5. Eseguiamo l'`import` del tipo e usiamolo per dichiarare la proprietà `todo`. Sostituisca la linea `export let todo` con la seguente:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todo: TodoType;
   ```

   Si noti che l'estensione del file `.ts` non è consentita nell'istruzione `import` ed è stata omessa.

6. Ora da `Todos.svelte` instanzieremo un componente `Todo` con un oggetto letterale come parametro prima della chiamata al componente `MoreActions`, in questo modo:

   ```svelte
   <hr />

   <Todo todo={ { name: 'a new task with no id!', completed: false } } />

   <!-- MoreActions -->
   <MoreActions {todos}
   ```

7. Aggiunga il `lang='ts'` al tag `<script>` del componente `Todos.svelte` in modo che sappia di utilizzare il controllo dei tipi che abbiamo specificato.

   Riporteremo il seguente errore:

   ![Errore di tipo in VS Code, l'oggetto di tipo Todo richiede una proprietà id](08-vscode-structural-typing.png)

A questo punto dovrebbe avere un'idea del tipo di assistenza che possiamo ottenere da TypeScript durante la creazione di progetti Svelte.

Ora annulleremo queste modifiche per iniziare a portare la nostra applicazione su TypeScript, in modo da non essere disturbati da tutti gli avvisi di controllo.

1. Rimuova il to-do difettoso e l'attributo `lang='ts'` dal file `Todos.svelte`.
2. Rimuova anche l'importazione di `TodoType` e il `lang='ts'` da `Todo.svelte`.

Ce ne occuperemo correttamente più avanti.

## Portare la nostra app della lista delle cose da fare su TypeScript

Ora siamo pronti per iniziare a portare la nostra applicazione della lista delle cose da fare per sfruttare tutte le funzionalità che TypeScript ci offre.

Iniziamo eseguendo lo script di controllo in modalità watch all'interno della radice del progetto:

```bash
npm run check -- --watch
```

Questo dovrebbe produrre qualcosa del genere:

```bash
svelte-check "--watch"

Loading svelte-check in workspace: ./svelte-todo-typescript
Getting Svelte diagnostics...
====================================
svelte-check found no errors and no warnings
```

Si noti che se sta usando un editor di codice supportato come VS Code, un modo semplice per iniziare a portare un componente Svelte è semplicemente aggiungere `<script lang='ts'>` all'inizio del suo componente e cercare i suggerimenti a tre punti:

![Screenshot di VS Code che mostra che quando si aggiunge type="ts" a un componente, fornisce suggerimenti di allerta a tre punti](09-vscode-alert-hints.png)

### Alert.svelte

Iniziamo con il nostro componente `Alert.svelte`.

1. Aggiunga `lang="ts"` al componente `<script>` del suo `Alert.svelte`. Vedrà alcuni avvisi nell'output dello script `check`:

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

2. Può risolverli specificando i tipi corrispondenti, in questo modo:

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

1. Aggiunga l'attributo `lang='ts'`, come prima. TypeScript ci avvertirà riguardo alla proprietà `todos` e alla variabile `t` nella chiamata a `todos.filter((t) =>...)`.

   ```plain
   Warn: Variable 'todos' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     export let todos

   Warn: Parameter 't' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     $: completedTodos = todos.filter((t) => t.completed).length
   ```

2. Utilizzeremo il `TodoType` che abbiamo già definito per dire a TypeScript che `todos` è un array di `TodoType`. Sostituisca la linea `export let todos` con la seguente:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todos: TodoType[];
   ```

Noti che ora TypeScript può dedurre che la variabile `t` in `todos.filter((t) => t.completed)` è di tipo `TodoType`. Tuttavia, se pensa che renda il nostro codice più facile da leggere, potremmo specificarlo in questo modo:

```ts
$: completedTodos = todos.filter((t: TodoType) => t.completed).length;
```

La maggior parte delle volte, TypeScript sarà in grado di dedurre correttamente il tipo di variabile reattiva, ma a volte potrebbe ottenere un errore "implicitamente ha tipo 'any'" quando si lavora con le assegnazioni reattive. In quei casi può dichiarare la variabile tipizzata in una dichiarazione diversa, in questo modo:

```ts
let completedTodos: number;
$: completedTodos = todos.filter((t: TodoType) => t.completed).length;
```

Non può specificare il tipo nell'assegnazione reattiva stessa. L'istruzione `$: completedTodos: number = todos.filter[...]` è invalida. Per maggiori informazioni, legga [Come digito le assegnazioni reattive? / Ottengo un errore "implicitamente ha tipo 'any'"](https://github.com/sveltejs/language-tools/blob/master/docs/preprocessors/typescript.md#how-do-i-type-reactive-assignments--i-get-an-implicitly-has-type-any-error).

### FilterButton.svelte

Ora ci occuperemo del componente `FilterButton`.

1. Aggiunga l'attributo `lang='ts'` al tag `<script>`, come al solito. Noterà che non ci sono avvisi — TypeScript deduce il tipo della variabile filter dal valore predefinito. Ma sappiamo che ci sono solo tre valori validi per il filtro: tutti, attivi e completati. Quindi possiamo far sapere a TypeScript riguardo a loro creando un enum Filter.
2. Crei un file `filter.enum.ts` nella cartella `types`.
3. Gli dia il seguente contenuto:

   ```ts
   export enum Filter {
     ALL = "all",
     ACTIVE = "active",
     COMPLETED = "completed",
   }
   ```

4. Ora useremo questo dal componente `FilterButton`. Sostituisca il contenuto del file `FilterButton.svelte` con il seguente:

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

Qui stiamo semplicemente importando l'enum `Filter` e usandolo invece dei valori di stringa che usavamo in precedenza.

### Todos.svelte

Utilizzeremo anche l'enum `Filter` nel componente `Todos.svelte`.

1. Innanzitutto, aggiunga l'attributo `lang='ts'`, come prima.
2. Poi, importi l'enum `Filter`. Aggiunga la seguente istruzione `import` sotto le sue esistenti:

   ```js
   import { Filter } from "../types/filter.enum";
   ```

3. Ora lo useremo ogni volta che facciamo riferimento al filtro corrente. Sostituisca i suoi due blocchi correlati ai filtri con i seguenti:

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

4. `check` ci darà ancora alcuni avvisi da `Todos.svelte`. Risolviamoli.

   Cominciamo importando il `TodoType` e dicendo a TypeScript che la nostra variabile `todos` è un array di `TodoType`. Sostituisca `export let todos = []` con le seguenti due righe:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todos: TodoType[] = [];
   ```

5. Successivamente specificheremo tutti i tipi mancanti. La variabile `todosStatus`, che usiamo per accedere in modo programmatico ai metodi esposti dal componente `TodosStatus`, è di tipo `TodosStatus`. E ogni `todo` sarà di tipo `TodoType`.

   Aggiorni il suo `<script>` section per sembrare questo:

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

Stiamo incontrando i seguenti errori relativi al passaggio di `todos` ai componenti `TodosStatus.svelte` (e `Todo.svelte`):

```plain
./src/components/Todos.svelte:70:39
Error: Type 'TodoType[]' is not assignable to type 'undefined'. (ts)
  <TodosStatus bind:this={todosStatus} {todos} />

./src/components/Todos.svelte:76:12
Error: Type 'TodoType' is not assignable to type 'undefined'. (ts)
     <Todo {todo}
```

Questo perché la proprietà `todos` nel componente `TodosStatus` non ha un valore predefinito, quindi TypeScript l'ha dedotta come tipo `undefined`, che non è compatibile con un array di `TodoType`. La stessa cosa sta accadendo con il nostro componente Todo.

Risolviamolo.

1. Apra il file `TodosStatus.svelte` e aggiunga l'attributo `lang='ts'`.
2. Poi importiamo il `TodoType` e dichiariamo la proprietà `todos` come un array di `TodoType`. Sostituisca la prima riga della sezione `<script>` con la seguente:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todos: TodoType[];
   ```

3. Specificheremo anche `headingEl`, che abbiamo usato per collegare al tag heading, come `HTMLElement`. Aggiorni la linea `let headingEl` con la seguente:

   ```ts
   let headingEl: HTMLElement;
   ```

4. Infine, noterà l'errore riportato, relativo a quando impostiamo l'attributo `tabindex`. Questo perché TypeScript sta controllando il tipo dell'elemento `<h2>` e si aspetta che `tabindex` sia di tipo `number`.

   ![Suggerimento di Tabindex all'interno di VS Code, tabindex si aspetta un tipo di numero, non di stringa](10-vscode-tabindex-hint.png)

   Per risolverlo, sostituisca `tabindex="-1"` con `tabindex={-1}`, come questo:

   ```svelte
   <h2 id="list-heading" bind:this={headingEl} tabindex={-1}>
     {completedTodos} out of {totalTodos} items completed
   </h2>
   ```

   In questo modo TypeScript ci può impedire di assegnarlo in modo errato a una variabile stringa.

### NewTodo.svelte

Successivamente ci occuperemo di `NewTodo.svelte`.

1. Come al solito, aggiunga l'attributo `lang='ts'`.
2. L'avviso indicherà che dobbiamo specificare un tipo per la variabile `nameEl`. Imposti il suo tipo su `HTMLElement` in questo modo:

   ```ts
   let nameEl: HTMLElement; // reference to the name input DOM node
   ```

3. Infine per questo file, abbiamo bisogno di specificare il tipo corretto per la nostra variabile `autofocus`. Aggiorni la sua definizione così:

   ```ts
   export let autofocus: boolean = false;
   ```

### Todo.svelte

Ora gli unici avvisi che `npm run check` emette sono provocati chiamando il componente `Todo.svelte`. Risolviamoli.

1. Apra il file `Todo.svelte` e aggiunga l'attributo `lang='ts'`.
2. Importiamo il `TodoType` e impostiamo il tipo della proprietà `todo`. Sostituisca la linea `export let todo` con la seguente:

   ```ts
   import type { TodoType } from "../types/todo.type";

   export let todo: TodoType;
   ```

3. Il primo avviso che riceviamo è TypeScript che ci dice di definire il tipo della variabile `updatedTodo` della funzione `update()`. Questo può essere un po' complicato perché `updatedTodo` contiene solo gli attributi del `todo` che sono stati aggiornati. Ciò significa che non è un `todo` completo — ha solo un sottoinsieme delle proprietà di un `todo`.

   Per questi tipi di casi, TypeScript fornisce diversi [tipi di utilità](https://www.typescriptlang.org/docs/handbook/utility-types.html) per facilitarci nell'applicare queste trasformazioni comuni. Quello che ci serve ora è l'utilità [`Partial<T>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialt), che ci permette di rappresentare tutti i sottoinsiemi di un tipo dato. L'utilità parziale restituisce un nuovo tipo basato sul tipo `T`, in cui ogni proprietà di `T` è facoltativa.

   La useremo nella funzione `update()` — la aggiorni in questo modo:

   ```ts
   function update(updatedTodo: Partial<TodoType>) {
     todo = { ...todo, ...updatedTodo }; // applies modifications to todo
     dispatch("update", todo); // emit update event
   }
   ```

   In questo modo stiamo dicendo a TypeScript che la variabile `updatedTodo` conterrà un sottoinsieme delle proprietà di `TodoType`.

4. Ora svelte-check ci dice che dobbiamo definire il tipo dei parametri della nostra funzione azione:

   ```bash
   ./07-next-steps/src/components/Todo.svelte:45:24
   Warn: Parameter 'node' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     const focusOnInit = (node) => node && typeof node.focus === 'function' && node.focus()

   ./07-next-steps/src/components/Todo.svelte:47:28
   Warn: Parameter 'node' implicitly has an 'any' type, but a better type may be inferred from usage. (ts)
     const focusEditButton = (node) => editButtonPressed && node.focus()
   ```

   Dobbiamo solo definire la variabile node come tipo `HTMLElement`. Sostituisca la prima istanza di `node` nelle due righe indicate sopra con `node: HTMLElement`.

### actions.js

Successivamente ci occuperemo del file `actions.js`.

1. Rinomini il file in `actions.ts` e aggiunga il tipo del parametro node. Dovrebbe finire per sembrare in questo modo:

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

2. Ora aggiorni `Todo.svelte` e `NewTodo.svelte` dove importiamo il file di azioni. Ricordi che gli import in TypeScript non includono l'estensione del file. In ogni caso dovrebbe finire per essere così:

   ```js
   import { selectOnFocus } from "../actions";
   ```

### Migrare i negozi su TypeScript

Ora dobbiamo migrare i file `stores.js` e `localStore.js` su TypeScript.

Suggerimento: lo script `npm run check`, che usa lo strumento [`svelte-check`](https://github.com/sveltejs/language-tools/tree/master/packages/svelte-check), controllerà solo i file `.svelte` della nostra applicazione. Se vuole controllare anche i file `.ts`, può eseguire `npm run check && npx tsc --noEmit`, che dice al compilatore TypeScript di controllare gli errori senza generare i file di output `.js`. Potrebbe persino aggiungere uno script al suo file `package.json` che esegue quel comando.

Inizieremo con `stores.js`.

1. Rinomini il file in `stores.ts`.
2. Imposti il tipo del nostro array `initialTodos` su `TodoType[]`. Questo è come i contenuti finiranno:

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

3. Ricordi di aggiornare le istruzioni `import` in `App.svelte`, `Alert.svelte`, e `Todos.svelte`. Basta rimuovere l'estensione `.js`, in questo modo:

   ```js
   import { todos } from "../stores";
   ```

Ora passiamo a `localStore.js`.

Aggiornare l'istruzione `import` in `stores.ts` in questo modo:

```js
import { localStore } from "./localStore";
```

1. Inizia rinominando il file in `localStore.ts`.
2. TypeScript ci sta dicendo di specificare il tipo delle variabili `key`, `initial`, e `value`. La prima è facile: la chiave del nostro web storage locale dovrebbe essere una stringa.

   Ma `initial` e `value` dovrebbero essere qualsiasi oggetto che potrebbe essere convertito in una stringa JSON valida con il metodo [`JSON.stringify`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify), cioè qualsiasi oggetto JavaScript con alcune limitazioni: ad esempio, `undefined`, funzioni e simboli non sono valori JSON validi.

   Quindi creeremo il tipo `JsonValue` per specificare queste condizioni.

   Crei il file `json.type.ts` nella cartella `types`.

3. Gli dia il seguente contenuto:

   ```ts
   export type JsonValue =
     | string
     | number
     | boolean
     | null
     | JsonValue[]
     | { [key: string]: JsonValue };
   ```

   L'operatore `|` consente di dichiarare variabili che potrebbero memorizzare valori di due o più tipi. Un `JsonValue` potrebbe essere una stringa, un numero, un booleano, e così via. In questo caso stiamo anche utilizzando i tipi ricorsivi per specificare che un `JsonValue` può avere un array di `JsonValue` e anche un oggetto con proprietà di tipo `JsonValue`.

4. Importiamo il nostro tipo `JsonValue` e usiamolo di conseguenza. Aggiorni il suo file `localStore.ts` in questo modo:

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

Ora se proviamo a creare un `localStore` con qualcosa che non può essere convertito in JSON tramite `JSON.stringify()`, ad esempio un oggetto con una funzione come proprietà, VS Code/`validate` si lamenterà di ciò:

![VS Code che mostra un errore con l'uso del nostro store — si verifica un errore quando si tenta di impostare un valore di storage locale su qualcosa di incompatibile con JSON.stringify](11-vscode-invalid-store.png)

E il meglio di tutto, funzionerà anche con la sintassi [`$store` per auto-sottoscrizione](https://svelte.dev/docs/svelte-components#script-4-prefix-stores-with-$-to-access-their-values). Se proviamo a salvare un valore non valido nel nostro store `todos` utilizzando la sintassi `$store`, come questo:

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

Questo è un altro esempio di come specificare tipi può rendere il nostro codice più robusto e aiutarci a cogliere più bug prima che entrino in produzione.

E questo è tutto. Abbiamo convertito tutta la nostra applicazione per usare TypeScript.

## Rendere a prova di proiettile i nostri negozi con i generici

I nostri negozi sono già stati portati su TypeScript, ma possiamo fare di meglio. Non dovremmo aver bisogno di memorizzare alcun tipo di valore — sappiamo che lo store di alert dovrebbe contenere messaggi di stringa, e lo store dei to-do dovrebbe contenere un array di `TodoType`, ecc. Possiamo lasciare che TypeScript lo faccia rispettare usando i [Generici TypeScript](https://www.typescriptlang.org/docs/handbook/generics.html). Scopriamone di più.

### Comprendere i generici di TypeScript

I generici le permettono di creare componenti di codice riutilizzabili che funzionano con una varietà di tipi invece di un singolo tipo. Possono essere applicati a interfacce, classi e funzioni. I tipi generici sono passati come parametri usando una sintassi speciale: sono specificati all'interno di parentesi angolari e vengono convenzionalmente rappresentati da una singola lettera maiuscola. I tipi generici le permettono di raccogliere i tipi forniti dall'utente, assicurandosi che siano disponibili per un ulteriore elaborazione.

Vediamo un rapido esempio, una semplice classe `Stack` che ci consente di `push` e `pop` elementi, come questo:

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

In questo caso `elements` è un array di tipo `any`, e di conseguenza i metodi `push()` e `pop()` ricevono e restituiscono una variabile di tipo `any`. Quindi è perfettamente valido fare qualcosa come il seguente:

```js
const anyStack = new Stack();

anyStack.push(1);
anyStack.push("hello");
```

Ma cosa succederebbe se volessimo avere uno `Stack` che funzioni solo con il tipo `string`? Potremmo fare il seguente:

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

Quello funzionerebbe. Ma se volessimo lavorare con numeri, dovremmo quindi duplicare il nostro codice e creare una classe `NumberStack`. E come potremmo gestire uno stack di tipi che non conosciamo ancora, e che dovrebbero essere definiti dall'utente?

Per risolvere tutti questi problemi, possiamo usare i generici.

Questa è la nostra classe `Stack` reimplementata usando i generici:

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

Definiamo un tipo generico `T` e poi lo usiamo come utilizzeremmo normalmente un tipo specifico. Ora gli elementi sono un array di tipo `T`, e i metodi `push()` e `pop()` ricevono e restituiscono entrambi una variabile di tipo `T`.

Ecco come utilizzeremmo il nostro `Stack` generico:

```ts
const numberStack = new Stack<number>();
numberStack.push(1);
```

Ora TypeScript sa che il nostro stack può accettare solo numeri e emetterà un errore se tentiamo di inserire qualsiasi altra cosa:

![Argomento di tipo hello non è assegnabile al parametro di tipo number](12-vscode-generic-stack-error.png)

TypeScript può anche dedurre i tipi generici dal suo utilizzo. I generici supportano anche valori predefiniti e vincoli.

I generici sono una caratteristica potente che permette al nostro codice di evitare di specificare i tipi che vengono usati, rendendolo più riutilizzabile e generico senza rinunciare alla sicurezza dei tipi. Per saperne di più, dia un'occhiata all'[Introduzione ai generici di TypeScript](https://www.typescriptlang.org/docs/handbook/generics.html).

### Usare i negozi Svelte con i generici

I negozi Svelte supportano i generici fin da subito. E, a causa dell'inferenza dei tipi generici, possiamo approfittarne anche senza toccare il nostro codice.

Se apre il file `Todos.svelte` e assegna un tipo `number` al nostro store `$alert`, otterrà il seguente errore:

![L'argomento di tipo 9999 non è assegnabile al parametro di tipo string](13-vscode-generic-alert-error.png)

Questo poiché quando abbiamo definito il nostro store di alert nel file `stores.ts` con:

```js
export const alert = writable("Welcome to the To-Do list app!");
```

TypeScript ha inferito il tipo generico come `string`. Se volessimo essere espliciti al riguardo, potremmo fare il seguente:

```ts
export const alert = writable<string>("Welcome to the To-Do list app!");
```

Ora faremo in modo che il nostro negozio `localStore` supporti i generici. Ricordi che abbiamo definito il tipo `JsonValue` per impedire l'uso del nostro negozio `localStore` con valori che non possono essere persistiti usando `JSON.stringify()`. Ora vogliamo che i consumatori di `localStore` possano specificare il tipo di dati da mantenere, ma invece di lavorare con qualsiasi tipo, dovrebbero rispettare il tipo `JsonValue`. Lo specificheremo con un vincolo generico, come questo:

```ts
export const localStore = <T extends JsonValue>(key: string, initial: T) => {
  // …
};
```

Definiamo un tipo generico `T` e specifichiamo che deve essere compatibile con il tipo `JsonValue`. Quindi useremo il tipo `T` in modo appropriato.

Il nostro file `localStore.ts` finirà come questo — provi il nuovo codice ora nella sua versione:

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

![Proprietà dell'oggetto di tipo Todo completa dovrebbe essere completata](14-vscode-generic-localstore-error.png)

Ancora una volta, se volessimo essere espliciti al riguardo, potremmo farlo nel file `stores.ts` in questo modo:

```ts
const initialTodos: TodoType[] = [
  { id: 1, name: "Visit MDN web docs", completed: true },
  { id: 2, name: "Complete the Svelte Tutorial", completed: false },
];

export const todos = localStore<TodoType[]>("mdn-svelte-todo", initialTodos);
```

Questo sarà sufficiente per il nostro breve tour dei Generici TypeScript.

## Il codice finora

### Git

Per vedere lo stato del codice come dovrebbe essere alla fine di questo articolo, acceda alla sua copia del nostro repo in questo modo:

```bash
cd mdn-svelte-tutorial/08-next-steps
```

Oppure scarichi direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/08-next-steps
```

Ricordi di eseguire `npm install && npm run dev` per avviare la sua app in modalità sviluppo.

### REPL

Come abbiamo detto prima, TypeScript non è ancora disponibile nel REPL.

## Riassunto

In questo articolo abbiamo preso la nostra applicazione della lista delle cose da fare e l'abbiamo portata su TypeScript.

Prima abbiamo appreso riguardo a TypeScript e quali vantaggi può offrirci. Poi abbiamo visto come creare un nuovo progetto Svelte con supporto TypeScript. Abbiamo anche visto come convertire un progetto Svelte esistente per utilizzare TypeScript — la nostra app della lista delle cose da fare.

Abbiamo visto come lavorare con [Visual Studio Code](https://code.visualstudio.com/) e l'[estensione Svelte](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode) per ottenere funzionalità come il controllo dei tipi e il completamento automatico. Abbiamo anche usato lo strumento `svelte-check` per ispezionare i problemi TypeScript dalla linea di comando.

Nel prossimo articolo impareremo come compilare e distribuire la nostra app in produzione. Vedremo anche quali risorse sono disponibili online per approfondire l'apprendimento di Svelte.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Svelte_stores","Learn_web_development/Core/Frameworks_libraries/Svelte_deployment_next", "Learn_web_development/Core/Frameworks_libraries")}}
