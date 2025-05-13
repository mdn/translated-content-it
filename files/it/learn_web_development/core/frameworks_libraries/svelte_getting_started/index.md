---
title: Introduzione a Svelte
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_getting_started
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Svelte_todo_list_beginning", "Learn_web_development/Core/Frameworks_libraries")}}

In questo articolo forniremo una rapida introduzione al [framework Svelte](https://svelte.dev/). Vedremo come funziona Svelte e cosa lo distingue dagli altri framework e strumenti che abbiamo visto finora. Poi impareremo come impostare il nostro ambiente di sviluppo, creare un'applicazione di esempio, capire la struttura del progetto e vedere come eseguirla localmente e crearla per la produzione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Al minimo, è raccomandato avere familiarità con le lingue di base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e avere conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminal/comando linea</a
          >.
        </p>
        <p>
          Svelte è un compilatore che genera codice JavaScript minimale e altamente ottimizzato
          dalle nostre sorgenti; avrà bisogno di un terminale con node +
          npm installato per compilare e costruire la sua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Configurare un ambiente di sviluppo locale Svelte, creare e costruire un'applicazione di partenza e comprendere le basi di come funziona.
      </td>
    </tr>
  </tbody>
</table>

## Svelte: Un nuovo approccio per costruire interfacce utente ricche

Svelte fornisce un approccio diverso alla costruzione di app web rispetto ad alcuni degli altri framework trattati in questo modulo. Mentre framework come React e Vue svolgono la maggior parte del loro lavoro nel browser dell'utente mentre l'app è in esecuzione, Svelte sposta quel lavoro in una fase di compilazione che avviene solo quando si costruisce l'app, producendo JavaScript vanilla altamente ottimizzato.

Il risultato di questo approccio è non solo pacchetti applicativi più piccoli e migliori prestazioni, ma anche un'esperienza di sviluppo più accessibile per chi ha un'esperienza limitata nell'ecosistema degli strumenti moderni.

Svelte si attiene strettamente al modello di sviluppo web classico di HTML, CSS e JS, aggiungendo solo poche estensioni a HTML e JavaScript. Ha una quantità inferiore di concetti e strumenti da imparare rispetto ad alcune delle altre opzioni di framework.

Gli svantaggi principali attuali sono che è un framework giovane — il suo ecosistema è quindi più limitato in termini di strumenti, supporto, plugin, modelli d’uso chiari, ecc. rispetto a framework più maturi, e ci sono anche meno opportunità lavorative. Ma i suoi vantaggi dovrebbero essere sufficienti per suscitare interesse nell'esplorarlo.

> [!NOTE]
> Svelte ha il [supporto ufficiale TypeScript](https://svelte.dev/docs/typescript). Lo vedremo più avanti in questa serie di tutorial.

Le incoraggiamo a seguire il [tutorial Svelte](https://learn.svelte.dev/) per una rapida introduzione ai concetti fondamentali, prima di tornare a questa serie di tutorial per imparare a costruire qualcosa di leggermente più approfondito.

## Casi d'uso

Svelte può essere utilizzato per sviluppare piccoli pezzi di un'interfaccia o intere applicazioni. Può sia iniziare da zero lasciando che Svelte guidi la sua UI, sia integrarlo gradualmente in un'applicazione esistente.

Nonostante ciò, Svelte è particolarmente appropriato per affrontare le seguenti situazioni:

- Applicazioni web destinate a dispositivi a basso consumo: Le applicazioni costruite con Svelte hanno dimensioni di pacchetto più piccole, il che è ideale per dispositivi con connessioni di rete lente e potenza di elaborazione limitata. Meno codice significa meno KB da scaricare, analizzare, eseguire e mantenere in memoria.
- Pagine altamente interattive o visualizzazioni complesse: Se sta costruendo visualizzazioni di dati che devono mostrare un gran numero di elementi DOM, i guadagni di prestazione derivanti da un framework senza sovraccarico di runtime assicureranno che le interazioni con l'utente siano reattive e rapide.
- Integrazione di persone con conoscenze di base dello sviluppo web: Svelte ha una curva di apprendimento poco ripida. Gli sviluppatori web con conoscenze di base di HTML, CSS e JavaScript possono facilmente comprendere le particolarità di Svelte in poco tempo e iniziare a costruire applicazioni web.

Il team di Svelte ha lanciato [SvelteKit](https://kit.svelte.dev/), un framework per costruire applicazioni web usando Svelte. Contiene funzionalità trovate nei framework web moderni, come il routing basato sul file system, rendering lato server (SSR), modalità di rendering specifiche delle pagine, supporto offline e altro. Per ulteriori informazioni su SvelteKit, vedere il [tutorial ufficiale](https://learn.svelte.dev/) e la [documentazione](https://kit.svelte.dev/docs/introduction).

Svelte è anche disponibile per lo sviluppo mobile tramite [Svelte Native](https://svelte.nativescript.org/).

## Come funziona Svelte?

Essendo un compilatore, Svelte può estendere HTML, CSS e JavaScript, generando codice JavaScript ottimale senza alcun sovraccarico di runtime. Per ottenere ciò, Svelte estende le tecnologie web vanilla nei seguenti modi:

- Estende HTML consentendo espressioni JavaScript nel markup e fornendo direttive per utilizzare condizioni e cicli, in modo simile a handlebars.
- Estende CSS aggiungendo un meccanismo di scoping, che permette a ciascun componente di definire i propri stili senza il rischio di conflitti con gli stili di altri componenti.
- Estende JavaScript reinterpretando direttive specifiche del linguaggio per ottenere vera reattività e gestione dello stato del componente.

Il compilatore interviene solo in situazioni molto specifiche e solo nel contesto dei componenti Svelte. Le estensioni al linguaggio JavaScript sono minime e scelte con cura per non rompere la sintassi di JavaScript o alienare gli sviluppatori. In effetti, lavorerà principalmente con JavaScript vanilla.

## Primi passi con Svelte

Dato che Svelte è un compilatore, non può semplicemente aggiungere un tag `<script src="svelte.js">` alla pagina e importarlo nella sua app. Dovrà configurare l'ambiente di sviluppo per consentire al compilatore di fare il suo lavoro.

### Requisiti

Per lavorare con Svelte, deve avere installato [Node.js](https://nodejs.org/). Si consiglia di utilizzare la versione di supporto a lungo termine (LTS). Node include npm (il gestore di pacchetti node) e npx (l'esecutore di pacchetti node). Si noti che può anche utilizzare il gestore di pacchetti Yarn al posto di npm, ma supporremo che stia utilizzando npm in questa serie di tutorial. Vedere [Nozioni di base sulla gestione dei pacchetti](/it/docs/Learn_web_development/Extensions/Client-side_tools/Package_management) per ulteriori informazioni su npm e yarn.

Se sta utilizzando Windows, dovrà installare qualche software per ottenere una parità con il terminale Unix/macOS per utilizzare i comandi del terminale menzionati in questo tutorial. Gitbash (che viene incluso come parte del [pacchetto di strumenti git per Windows](https://gitforwindows.org/)) o il [Sottosistema Windows per Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/about) sono entrambi adatti. Vedere [Corso accelerato sui comandi](/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line) per ulteriori informazioni su questi e sui comandi del terminale in generale.

Vedere anche quanto segue per ulteriori informazioni:

- ["Informazioni su npm"](https://docs.npmjs.com/about-npm/) nella documentazione di npm
- ["Introduzione a npx"](https://blog.npmjs.org/post/162869356040/introducing-npx-an-npm-package-runner) nel blog di npm

### Creazione della sua prima app Svelte

Il modo più semplice per creare un modello di app iniziale è scaricare l'applicazione modello iniziale. Può farlo visitando [sveltejs/template](https://github.com/sveltejs/template) su GitHub, oppure può evitare di doverlo scaricare e decomprimere e utilizzare semplicemente [degit](https://github.com/Rich-Harris/degit).

Per creare il suo modello di app iniziale, esegua i seguenti comandi nel terminale:

```bash
npx degit sveltejs/template moz-todo-svelte
cd moz-todo-svelte
npm install
npm run dev
```

> [!NOTE]
> degit non fa alcun tipo di magia — permette solo di scaricare e decomprimere l'ultima versione del contenuto di un repository git. Questo è molto più veloce di `git clone` perché non scaricherà tutta la cronologia del repository, né creerà un clone locale completo.

Dopo aver eseguito `npm run dev`, Svelte compilerà e costruirà la sua applicazione. Avvierà un server locale su `localhost:8080`. Svelte monitorerà gli aggiornamenti dei file, ricompilandoli e aggiornando automaticamente l'applicazione quando vengono apportate modifiche ai file sorgente. Il suo browser visualizzerà qualcosa del genere:

![Una semplice pagina iniziale che dice ciao mondo, e fornisce un link ai tutorial ufficiali di svelte](01-svelte-starter-app.png)

### Struttura dell'applicazione

Il modello iniziale ha la seguente struttura:

```plain
moz-todo-svelte
├── README.md
├── package.json
├── package-lock.json
├── rollup.config.js
├── .gitignore
├── node_modules
├── public
│   ├── favicon.png
│   ├── index.html
│   ├── global.css
│   └── build
│       ├── bundle.css
│       ├── bundle.js
│       └── bundle.js.map
├── scripts
│   └── setupTypeScript.js
└── src
    ├── App.svelte
    └── main.js
```

I contenuti sono i seguenti:

- `package.json` e `package-lock.json`: Contiene informazioni sul progetto che Node.js/npm utilizza per mantenerlo organizzato. Non è necessario comprendere questo file per completare questo tutorial, tuttavia, se desidera saperne di più, può leggere la gestione di [`package.json`](https://docs.npmjs.com/cli/configuring-npm/package-json/) su npmjs.com; ne parliamo anche nel nostro [tutorial sulle nozioni di base sulla gestione dei pacchetti](/it/docs/Learn_web_development/Extensions/Client-side_tools/Package_management).
- `node_modules`: Qui è dove node salva le dipendenze del progetto. Queste dipendenze non saranno inviate in produzione, sono utilizzate solo durante lo sviluppo.
- `.gitignore`: Indica a git quali file o cartelle ignorare dal progetto — utile se decide di includere la sua app in un repository git.
- `rollup.config.js`: Svelte utilizza [rollup.js](https://rollupjs.org/) come modulo bundle. Questo file di configurazione dice a rollup come compilare e costruire la sua app. Se preferisce [webpack](https://webpack.js.org/), può creare il suo progetto iniziale con `npx degit sveltejs/template-webpack svelte-app` invece.
- `scripts`: Contiene script di configurazione come richiesto. Attualmente dovrebbe contenere solo `setupTypeScript.js`.

  - `setupTypeScript.js`: Questo script configura il supporto TypeScript in Svelte. Ne parleremo più avanti nell'ultimo articolo.

- `src`: Questa directory è dove vive il codice sorgente della sua applicazione — dove creerà il codice per la sua app.

  - `App.svelte`: Questo è il componente di livello superiore della sua app. Finora rende solo il messaggio 'Hello World!'.
  - `main.js`: Il punto di ingresso nella nostra applicazione. Crea solo un'istanza del componente `App` e lo associa al corpo della nostra pagina HTML.

- `public`: Questa directory contiene tutti i file che saranno pubblicati in produzione.

  - `favicon.png`: Questa è la favicon per la sua app. Attualmente, è il logo di Svelte.
  - `index.html`: Questa è la pagina principale della sua app. Inizialmente è solo una pagina HTML vuota che carica i file CSS e i pacchetti js generati da Svelte.
  - `global.css`: Questo file contiene stili non scorporati. È un normale file CSS che sarà applicato a tutta l'applicazione.
  - `build`: Questa cartella contiene il codice sorgente CSS e JavaScript generato.

    - `bundle.css`: Il file CSS che Svelte ha generato dagli stili definiti per ciascun componente.
    - `bundle.js`: Il file JavaScript compilato da tutto il codice sorgente JavaScript.

## Dare uno sguardo al nostro primo componente Svelte

I componenti sono i blocchi costitutivi delle applicazioni Svelte. Sono scritti in file `.svelte` usando un superset di HTML.

Tutte e tre le sezioni — `<script>`, `<style>`, e markup — sono opzionali e possono apparire in qualsiasi ordine desideri.

```html
<script>
  // logic goes here
</script>

<style>
  /* styles go here */
</style>

<!-- markup (zero or more HTML elements) goes here -->
```

> [!NOTE]
> Per ulteriori informazioni sul formato del componente, si consulti la [documentazione dei componenti Svelte](https://svelte.dev/docs/svelte-components).

Con questo in mente, diamo uno sguardo al file `src/App.svelte` incluso nel modello iniziale. Dovrebbe vedere qualcosa di simile al seguente:

```html
<script>
  export let name;
</script>

<main>
  <h1>Hello {name}!</h1>
  <p>
    Visit the <a href="https://learn.svelte.dev/">Svelte tutorial</a> to learn
    how to build Svelte apps.
  </p>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
```

### La sezione `<script>`

Il blocco `<script>` contiene JavaScript che viene eseguito quando viene creata un'istanza del componente. Le variabili dichiarate (o importate) a livello superiore sono 'visibili' dal markup del componente. Le variabili a livello superiore sono il modo in cui Svelte gestisce lo stato del componente, e sono reattive per impostazione predefinita. Spiegheremo in dettaglio cosa significa più avanti.

```html
<script>
  export let name;
</script>
```

Svelte utilizza la keyword [`export`](/it/docs/Web/JavaScript/Reference/Statements/export) per contrassegnare una dichiarazione di variabile come una proprietà (o prop), il che significa che diventa accessibile ai consumatori del componente (ad es., altri componenti). Questo è un esempio di Svelte che estende la sintassi JavaScript per renderla più utile, mantenendola comunque familiare.

### La sezione del markup

Nella sezione del markup può inserire tutto l'HTML che desidera, e inoltre è possibile inserire espressioni JavaScript valide all'interno delle parentesi graffe singole (`{}`). In questo caso stiamo integrando il valore del prop `name` subito dopo il testo `Hello`.

```html
<main>
  <h1>Hello {name}!</h1>
  <p>
    Visit the <a href="https://learn.svelte.dev/">Svelte tutorial</a> to learn
    how to build Svelte apps.
  </p>
</main>
```

Svelte supporta anche tag come `{#if}`, `{#each}`, e `{#await}` — questi esempi consentono di rendere condizionatamente una porzione del markup, iterare attraverso una lista di elementi e lavorare con valori asincroni, rispettivamente.

### La sezione `<style>`

Se ha esperienza nellavoro con CSS, il seguente frammento dovrebbe avere senso:

```html
<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
```

Stiamo applicando uno stile al nostro elemento [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements). Cosa accadrà ad altri componenti con elementi `<h1>` al loro interno?

In Svelte, il CSS all'interno del blocco `<style>` di un componente sarà delimitato solo a quel componente. Questo funziona aggiungendo una classe agli elementi selezionati, sulla base di un hash degli stili del componente.

Può vedere questo in azione aprendo `localhost:8080` in una nuova scheda del browser, facendo clic destro/<kbd>Ctrl</kbd> sul'etichetta _HELLO WORLD!_ e scegliendo _Inspect_:

![App di avvio di Svelte con strumenti di sviluppo aperti, che mostra classi per stili delimitati](02-svelte-component-scoped-styles.png)

Quando compila l'app, Svelte modifica la definizione di stili del nostro `h1` in `h1.svelte-1tky8bj` e poi modifica ogni elemento `<h1>` nel nostro componente in `<h1 class="svelte-1tky8bj">`, in modo che raccolga gli stili come richiesto.

> [!NOTE]
> È possibile sovrascrivere questo comportamento e applicare stili a un selettore a livello globale usando il modificatore `:global()` (vedere i documenti di [Svelte `<style>`](https://svelte.dev/docs/svelte-components#style) per ulteriori informazioni).

## Apportare un paio di modifiche

Ora che abbiamo un'idea generale di come tutto si unisce, possiamo iniziare a fare alcune modifiche.
A questo punto può provare ad aggiornare il suo componente `App.svelte` — ad esempio cambiando l'elemento `<h1>` in `App.svelte` in modo che legga così:

```html
<h1>Hello {name} from MDN!</h1>
```

Basta salvare le modifiche e l'app in esecuzione su `localhost:8080` sarà aggiornata automaticamente.

### Una prima occhiata alla reattività di Svelte

Nel contesto di un framework UI, la reattività significa che il framework può aggiornare automaticamente il DOM quando lo stato di qualsiasi componente viene modificato.

In Svelte, la reattività è attivata assegnando un nuovo valore a qualsiasi variabile di livello superiore in un componente. Per esempio, potremmo includere una funzione `toggleName()` nel nostro componente `App`, e un pulsante per eseguirlo.

Provi ad aggiornare le sue sezioni `<script>` e markup in questo modo:

```html
<script>
  export let name;

  function toggleName() {
    if (name === "world") {
      name = "Svelte";
    } else {
      name = "world";
    }
  }
</script>

<main>
  <h1>Hello {name}!</h1>
  <button on:click="{toggleName}">Toggle name</button>
  <p>
    Visit the <a href="https://learn.svelte.dev/">Svelte tutorial</a> to learn
    how to build Svelte apps.
  </p>
</main>
```

Ogni volta che il pulsante viene cliccato, Svelte esegue la funzione `toggleName()`, che a sua volta aggiorna il valore della variabile `name`.

Come può vedere, l'etichetta `<h1>` viene aggiornata automaticamente. Sotto il cofano, Svelte ha creato il codice JavaScript per aggiornare il DOM ogni volta che il valore della variabile name cambia, senza utilizzare alcun DOM virtuale o altri meccanismi di riconciliazione complessi.

Noti l'uso di `:` in `on:click`. Questa è la sintassi di Svelte per ascoltare gli eventi DOM.

## Ispezionando main.js: il punto di ingresso della nostra app

Apriamo `src/main.js`, dove il componente `App` viene importato e utilizzato. Questo file è il punto di ingresso della nostra app, e inizialmente appare così:

```js
import App from "./App.svelte";

const app = new App({
  target: document.body,
  props: {
    name: "world",
  },
});

export default app;
```

`main.js` inizia importando il componente Svelte che andremo a utilizzare. Poi viene instanziato con `new App`, passando un oggetto opzione con le seguenti proprietà:

- `target`: L'elemento DOM all'interno del quale vogliamo che il componente sia reso, in questo caso l'elemento `<body>`.
- `props`: i valori da assegnare a ciascun prop del componente `App`.

## Uno sguardo sotto il cofano

Come riesce Svelte a far funzionare bene tutti questi file insieme?

Il compilatore Svelte elabora la sezione `<style>` di ogni componente e la compila nel file `public/build/bundle.css`.

Compila anche il markup e la sezione `<script>` di ogni componente e memorizza il risultato in `public/build/bundle.js`. Inoltre aggiunge il codice in `src/main.js` per fare riferimento alle funzionalità di ciascun componente.

Infine, il file `public/index.html` include i file generati `bundle.css` e `bundle.js`:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />

    <title>Svelte app</title>

    <link rel="icon" type="image/png" href="/favicon.png" />
    <link rel="stylesheet" href="/global.css" />
    <link rel="stylesheet" href="/build/bundle.css" />

    <script defer src="/build/bundle.js"></script>
  </head>

  <body></body>
</html>
```

La versione minificata di `bundle.js` pesa poco più di 3KB, il che include il "runtime di Svelte" (solo 300 righe di codice JavaScript) e il componente `App.svelte` compilato. Come può vedere, `bundle.js` è l'unico file JavaScript referenziato da `index.html`. Non ci sono altre librerie caricate nella pagina web.

Questa è un'impronta molto più piccola rispetto ai pacchetti compilati da altri framework. Tenga presente che, nel caso dei pacchetti di codice, non sono solo le dimensioni dei file da scaricare a contare. Questo è codice eseguibile che deve essere analizzato, eseguito e mantenuto in memoria. Quindi, questo fa davvero la differenza, specialmente nei dispositivi a basso consumo o nelle applicazioni intensive dal punto di vista del processore.

## Seguire questo tutorial

In questa serie di tutorial costruirà una completa applicazione web. Impareremo tutte le basi di Svelte e anche diversi argomenti avanzati.

Può semplicemente leggere i contenuti per ottenere una buona comprensione delle funzionalità di Svelte, ma otterrà il massimo da questo tutorial seguendo la codifica dell'app con noi mentre procede. Per facilitarle il follow-up di ogni articolo, forniamo un repository GitHub con una cartella contenente il codice sorgente per l'app com'è all'inizio di ciascun tutorial.

Svelte fornisce anche un REPL online, che è un parco giochi per il live-coding delle app Svelte sul web senza dover installare nulla sulla sua macchina. Forniamo un REPL per ogni articolo in modo che possa iniziare subito a codificare insieme a noi. Parliamo un po' di più su come usare questi strumenti.

### Usando Git

Il sistema di controllo versione più popolare è Git, insieme a GitHub, un sito che fornisce hosting per i suoi repository e diversi strumenti per lavorare con essi.

Useremo GitHub in modo che possa scaricare facilmente il codice sorgente per ogni articolo. Potrà anche ottenere il codice com'è dovrebbe essere dopo aver completato l'articolo, nel caso si perdessi.

Dopo aver [installato git](https://git-scm.com/downloads), per clonare il repository dovrebbe eseguire:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi all'inizio di ogni articolo, può semplicemente `cd` nella cartella corrispondente e avviare l'app in modalità di sviluppo per vedere quale sia lo stato attuale, in questo modo:

```bash
cd 02-starting-our-todo-app
npm install
npm run dev
```

Se desidera saperne di più su git e GitHub, abbiamo compilato un elenco di link a guide utili — vedere [Git e GitHub](/it/docs/Learn_web_development/Core/Version_control).

> [!NOTE]
> Se desidera solo scaricare i file senza clonare il repository git, può usare lo strumento degit in questo modo — `npx degit opensas/mdn-svelte-tutorial`. Può anche scaricare una cartella specifica con `npx degit opensas/mdn-svelte-tutorial/01-getting-started`. Degit non creerà un repository git locale, scaricherà solo i file della cartella specificata.

### Usando il REPL di Svelte

Un REPL ([read–eval–print loop](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)) è un ambiente interattivo che permette di inserire comandi e vedere immediatamente i risultati — molti linguaggi di programmazione forniscono un REPL.

Il REPL di Svelte è molto più di questo. È uno strumento online che le permette di creare app complete, salvarle online e condividerle con altri.

È il modo più semplice per iniziare a giocare con Svelte da qualsiasi macchina, senza dover installare nulla. È anche ampiamente usato dalla comunità Svelte. Se desidera condividere un'idea, chiedere aiuto o segnalare un problema, è sempre estremamente utile creare un'istanza REPL che dimostri il problema.

Diamo un rapido sguardo al REPL di Svelte e a come lo userebbe. Appare così:

![il REPL di svelte in azione, mostrando il codice del componente a sinistra e l'output a destra](03-svelte-repl-in-action.png)

Per avviare un REPL, apra il suo browser e navighi su <https://svelte.dev/repl>.

- Sul lato sinistro dello schermo vedrà il codice dei suoi componenti, e a destra vedrà l'output in esecuzione della sua app.
- La barra sopra il codice le consente di creare file `.svelte` e `.js` e riordinarli. Per creare un file all'interno di una cartella, basta specificare il percorso completo, come questo: `components/MyComponent.svelte`. La cartella sarà creata automaticamente.
- Sopra quella barra ha il titolo del REPL. Clicchi su di esso per modificarlo.
- Sul lato destro ha tre schede:

  - La scheda _Risultato_ mostra l'output della sua applicazione e fornisce una console in basso.
  - La scheda _Output JS_ le consente di ispezionare il codice JavaScript generato da Svelte e impostare le opzioni del compilatore.
  - La scheda _Output CSS_ mostra il CSS generato da Svelte.

- Sopra le schede, troverà una barra degli strumenti che le consente di entrare in modalità a schermo intero e scaricare la sua app. Se accede con un account GitHub, sarà anche in grado di biforcare e salvare l'app. Sarà anche in grado di vedere tutti i suoi REPL salvati facendo clic sul suo profilo GitHub e selezionando _Le sue app salvate_.

Ogni volta che cambia un file nel REPL, Svelte ricompila l'app e aggiorna la scheda Risultato. Per condividere la sua app, condivida l'URL. Ad esempio, ecco il link per un REPL che esegue la nostra app completa: <https://svelte.dev/repl/378dd79e0dfe4486a8f10823f3813190?version=3.23.2>.

> [!NOTE]
> Notare come sia possibile specificare la versione di Svelte nell'URL. Questo è utile quando si segnalano problemi relativi a una versione specifica di Svelte.

Forniremo un REPL all'inizio e alla fine di ogni articolo in modo che possiate iniziare a codificare con noi subito.

> [!NOTE]
> Al momento il REPL non può gestire correttamente i nomi delle cartelle. Se sta seguendo il tutorial sul REPL, crei semplicemente tutti i suoi componenti nella cartella root. Poi, quando vede un percorso nel codice, ad esempio `import Todos from './components/Todos.svelte'`, lo sostituisca semplicemente con un URL piatto, ad esempio `import Todos from './Todos.svelte'`.

## Il codice finora

### Git

Cloni il repository GitHub (se non l'ha già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi per arrivare allo stato attuale dell'app, esegua

```bash
cd mdn-svelte-tutorial/01-getting-started
```

Oppure scarichi direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/01-getting-started
```

Ricordi di eseguire `npm install && npm run dev` per avviare la sua app in modalità di sviluppo.

### REPL

Per codificare insieme a noi usando il REPL, parta da

<https://svelte.dev/repl/fc68b4f059d34b9c84fa042d1cce586c?version=3.23.2>

## Sommario

Questo ci porta alla fine del nostro sguardo iniziale a Svelte, incluso come installarlo localmente, creare un'app iniziale e come funzionano le basi. Nell'articolo successivo inizieremo a costruire la nostra prima applicazione vera e propria, una lista di cose da fare. Prima di farlo, però, ricapitoliamo alcune delle cose che abbiamo imparato.

In Svelte:

- Definiamo lo script, lo stile e il markup di ogni componente in un singolo file `.svelte`.
- I props dei componenti sono dichiarati con la keyword `export`.
- I componenti Svelte possono essere utilizzati semplicemente importando il corrispondente file `.svelte`.
- Gli stili dei componenti sono delimitati, evitando conflitti tra loro.
- Nella sezione del markup può includere qualsiasi espressione JavaScript mettendola tra parentesi graffe.
- Le variabili di livello superiore di un componente costituiscono il suo stato.
- La reattività è attivata semplicemente assegnando un nuovo valore a una variabile di livello superiore.

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Svelte_todo_list_beginning", "Learn_web_development/Core/Frameworks_libraries")}}
