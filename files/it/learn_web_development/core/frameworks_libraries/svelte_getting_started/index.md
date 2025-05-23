---
title: Iniziare con Svelte
slug: Learn_web_development/Core/Frameworks_libraries/Svelte_getting_started
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Svelte_todo_list_beginning", "Learn_web_development/Core/Frameworks_libraries")}}

In questo articolo forniremo un'introduzione rapida al [framework Svelte](https://svelte.dev/). Vedremo come funziona Svelte e cosa lo distingue dagli altri framework e strumenti che abbiamo visto finora. Poi impareremo come impostare il nostro ambiente di sviluppo, creare un'app di esempio, comprendere la struttura del progetto e vedere come eseguirla localmente e costruirla per la produzione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Al minimo, si raccomanda di essere familiari con le lingue di base
          <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e
          avere conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/riga di comando</a
          >.
        </p>
        <p>
          Svelte è un compilatore che genera codice JavaScript minimale e altamente ottimizzato dalle nostre fonti; avrai bisogno di un terminale con Node + npm installati per compilare e costruire la tua app.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Configurare un ambiente di sviluppo Svelte locale, creare e costruire un'app di partenza e comprendere le basi di come funziona.
      </td>
    </tr>
  </tbody>
</table>

## Svelte: un nuovo approccio per creare interfacce utente ricche

Svelte offre un approccio diverso per creare app web rispetto ad alcuni degli altri framework trattati in questo modulo. Mentre framework come React e Vue svolgono la maggior parte del lavoro nel browser dell'utente mentre l'app è in esecuzione, Svelte sposta quel lavoro in una fase di compilazione che avviene solo quando costruisci la tua app, producendo JavaScript vaniglia altamente ottimizzato.

Il risultato di questo approccio è non solo pacchetti applicativi più piccoli e migliori prestazioni, ma anche un'esperienza da sviluppatore più accessibile per chi ha un'esperienza limitata nell'ecosistema moderno degli strumenti.

Svelte rimane molto vicino al classico modello di sviluppo web di HTML, CSS e JS, aggiungendo solo alcune estensioni a HTML e JavaScript. Ha probabilmente meno concetti e strumenti da imparare rispetto ad alcune delle altre opzioni di framework.

I suoi principali svantaggi attuali sono che è un framework giovane — il suo ecosistema è quindi più limitato in termini di strumenti, supporto, plugin, modelli di utilizzo chiari, ecc. rispetto a framework più maturi, e ci sono anche meno opportunità di lavoro. Ma i suoi vantaggi dovrebbero essere sufficienti a farti interessare ad esplorarlo.

> [!NOTE]
> Svelte ha [supporto ufficiale per TypeScript](https://svelte.dev/docs/typescript). Lo esamineremo più avanti in questa serie di tutorial.

Ti incoraggiamo a seguire il [tutorial di Svelte](https://learn.svelte.dev/) per una rapida introduzione ai concetti di base, prima di tornare a questa serie di tutorial per imparare a costruire qualcosa di leggermente più approfondito.

## Casi d'uso

Svelte può essere utilizzato per sviluppare piccoli pezzi di un'interfaccia o intere applicazioni. Puoi iniziare da zero lasciando che Svelte guidi la tua UI oppure integrarlo gradualmente in un'applicazione esistente.

Tuttavia, Svelte è particolarmente adatto ad affrontare le seguenti situazioni:

- Applicazioni web destinate a dispositivi a bassa potenza: Le applicazioni costruite con Svelte hanno dimensioni di pacchetti più piccoli, il che è ideale per dispositivi con connessioni di rete lente e potenza di elaborazione limitata. Meno codice significa meno KB da scaricare, analizzare, eseguire e mantenere in memoria.
- Pagine altamente interattive o visualizzazioni complesse: Se stai costruendo visualizzazioni di dati che devono mostrare un gran numero di elementi DOM, i guadagni di prestazioni che derivano da un framework senza overhead del runtime assicureranno che le interazioni degli utenti siano rapide e reattive.
- Introduzione di persone con conoscenze di base sullo sviluppo web: Svelte ha una curva di apprendimento ridotta. Gli sviluppatori web con conoscenze di base di HTML, CSS e JavaScript possono comprendere facilmente gli aspetti specifici di Svelte in breve tempo e iniziare a creare applicazioni web.

Il team Svelte ha lanciato [SvelteKit](https://kit.svelte.dev/), un framework per creare applicazioni web utilizzando Svelte. Contiene caratteristiche presenti nei moderni framework web, come il routing basato su file system, il rendering lato server (SSR), modalità di rendering specifiche per pagina, supporto offline e altro. Per ulteriori informazioni su SvelteKit, vedere il [tutorial ufficiale](https://learn.svelte.dev/) e la [documentazione](https://kit.svelte.dev/docs/introduction).

Svelte è disponibile anche per lo sviluppo mobile tramite [Svelte Native](https://svelte.nativescript.org/).

## Come funziona Svelte?

Essendo un compilatore, Svelte può estendere HTML, CSS e JavaScript, generando codice JavaScript ottimale senza alcun overhead del runtime. Per ottenere ciò, Svelte estende le tecnologie web vaniglia nei seguenti modi:

- Estende HTML permettendo espressioni JavaScript nel markup e fornendo direttive per utilizzare condizioni e loop, in modo simile a handlebars.
- Estende CSS aggiungendo un meccanismo di ambito, permettendo a ciascun componente di definire i propri stili senza il rischio di sovrapposizione con gli stili di altri componenti.
- Estende JavaScript reinterpretando direttive specifiche del linguaggio per ottenere una vera reattività e semplificare la gestione dello stato dei componenti.

Il compilatore interviene solo in situazioni molto specifiche e solo nel contesto dei componenti Svelte. Le estensioni al linguaggio JavaScript sono minime e selezionate con attenzione per non rompere la sintassi di JavaScript o alienare gli sviluppatori. Infatti, lavorerai principalmente con JavaScript vaniglia.

## Primi passi con Svelte

Poiché Svelte è un compilatore, non puoi semplicemente aggiungere un tag `<script src="svelte.js">` alla tua pagina e importarlo nella tua app. Dovrai configurare il tuo ambiente di sviluppo per permettere al compilatore di svolgere il suo lavoro.

### Requisiti

Per lavorare con Svelte, devi avere installato [Node.js](https://nodejs.org/). Si raccomanda di utilizzare la versione di supporto a lungo termine (LTS). Node include npm (il gestore pacchetti di node), e npx (il runner di pacchetti di node). Nota che puoi anche utilizzare il gestore pacchetti Yarn al posto di npm, ma supporremo che tu stia utilizzando npm in questo insieme di tutorial. Vedi [Nozioni di base sulla gestione dei pacchetti](/it/docs/Learn_web_development/Extensions/Client-side_tools/Package_management) per ulteriori informazioni su npm e yarn.

Se stai usando Windows, dovrai installare alcuni software per avere parità con il terminale Unix/macOS per utilizzare i comandi del terminale menzionati in questo tutorial. Gitbash (che fa parte del [set di strumenti git per Windows](https://gitforwindows.org/)) o [Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/about) sono entrambi adatti. Vedi [Corso intensivo di riga di comando](/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line) per ulteriori informazioni su questi strumenti e sui comandi del terminale in generale.

Vedi anche quanto segue per ulteriori informazioni:

- ["About npm"](https://docs.npmjs.com/about-npm/) nella documentazione npm
- ["Introducing npx"](https://blog.npmjs.org/post/162869356040/introducing-npx-an-npm-package-runner) nel blog npm

### Creare la tua prima app Svelte

Il modo più semplice per creare un modello di app di partenza è scaricare l'applicazione modello di partenza. Puoi farlo visitando [sveltejs/template](https://github.com/sveltejs/template) su GitHub, o puoi evitare di doverla scaricare e decomprimerla usando [degit](https://github.com/Rich-Harris/degit).

Per creare il tuo modello di app di partenza, esegui i seguenti comandi nel terminale:

```bash
npx degit sveltejs/template moz-todo-svelte
cd moz-todo-svelte
npm install
npm run dev
```

> [!NOTE]
> degit non fa alcun tipo di magia — permette semplicemente di scaricare e decomprimere l'ultima versione dei contenuti di un repository git. Questo è molto più veloce di `git clone` perché non scaricherà tutta la cronologia del repository o creerà un clone locale completo.

Dopo aver eseguito `npm run dev`, Svelte compilerà e costruirà la tua applicazione. Avvierà un server locale su `localhost:8080`. Svelte monitorerà gli aggiornamenti ai file e ricompilerà automaticamente e aggiornerà l'app per te quando vengono apportate modifiche ai file sorgente. Il tuo browser mostrerà qualcosa di simile a questo:

![Una semplice pagina iniziale che dice hello world, e fornisce un link ai tutorial ufficiali di svelte](01-svelte-starter-app.png)

### Struttura dell'applicazione

Il modello di partenza viene fornito con la seguente struttura:

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

- `package.json` e `package-lock.json`: Contengono informazioni sul progetto che Node.js/npm utilizza per mantenerlo organizzato. Non è necessario comprendere questo file per completare il tutorial, tuttavia, se vuoi saperne di più, puoi leggere la gestione di [`package.json`](https://docs.npmjs.com/cli/configuring-npm/package-json/) su npmjs.com; ne parliamo anche nel nostro [tutorial sulle nozioni di base sulla gestione dei pacchetti](/it/docs/Learn_web_development/Extensions/Client-side_tools/Package_management).
- `node_modules`: Questo è il luogo in cui node salva le dipendenze del progetto. Queste dipendenze non verranno inviate in produzione, vengono utilizzate solo per scopi di sviluppo.
- `.gitignore`: Indica a git quali file o cartelle ignorare dal progetto — utile se decidi di includere la tua app in un repository git.
- `rollup.config.js`: Svelte utilizza [rollup.js](https://rollupjs.org/) come bundler di moduli. Questo file di configurazione dice a rollup come compilare e costruire la tua app. Se preferisci [webpack](https://webpack.js.org/), puoi creare il tuo progetto di partenza con `npx degit sveltejs/template-webpack svelte-app` invece.
- `scripts`: Contiene script di configurazione come richiesto. Attualmente dovrebbe contenere solo `setupTypeScript.js`.

  - `setupTypeScript.js`: Questo script imposta il supporto per TypeScript in Svelte. Ne parleremo più avanti nell'ultimo articolo.

- `src`: Questa directory è dove vive il codice sorgente della tua applicazione — dove creerai il codice per la tua app.

  - `App.svelte`: Questo è il componente di livello superiore della tua app. Finora si limita a rendere il messaggio 'Hello World!'.
  - `main.js`: Il punto di ingresso per la nostra applicazione. Instantiate semplicemente il componente `App` e lo collega al corpo della nostra pagina HTML.

- `public`: Questa directory contiene tutti i file che verranno pubblicati in produzione.

  - `favicon.png`: Questo è il favicon per la tua app. Attualmente, è il logo di Svelte.
  - `index.html`: Questa è la pagina principale della tua app. Inizialmente è solo una pagina HTML vuota che carica i file CSS e le bundle js generate da Svelte.
  - `global.css`: Questo file contiene stili senza ambito. È un file CSS regolare che verrà applicato a tutta l'applicazione.
  - `build`: Questa cartella contiene il codice sorgente CSS e JavaScript generato.

    - `bundle.css`: Il file CSS che Svelte ha generato dagli stili definiti per ciascun componente.
    - `bundle.js`: Il file JavaScript compilato da tutto il tuo codice sorgente JavaScript.

## Dare un'occhiata al nostro primo componente Svelte

I componenti sono i blocchi fondamentali delle applicazioni Svelte. Sono scritti nei file `.svelte` utilizzando un superset di HTML.

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
> Per ulteriori informazioni sul formato dei componenti, dai un'occhiata alla [documentazione dei componenti Svelte](https://svelte.dev/docs/svelte-components).

Con questo in mente, diamo un'occhiata al file `src/App.svelte` fornito con il modello di partenza. Dovresti vedere qualcosa di simile al seguente:

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

Il blocco `<script>` contiene JavaScript che viene eseguito quando viene creata un'istanza del componente. Le variabili dichiarate (o importate) a livello superiore sono 'visibili' dal markup del componente. Le variabili top-level sono il modo in cui Svelte gestisce lo stato del componente e sono reattive per impostazione predefinita. Spiegheremo in dettaglio cosa significa più avanti.

```html
<script>
  export let name;
</script>
```

Svelte usa la parola chiave [`export`](/it/docs/Web/JavaScript/Reference/Statements/export) per contrassegnare una dichiarazione di variabile come una proprietà (o prop), il che significa che diventa accessibile ai consumatori del componente (ad esempio, altri componenti). Questo è un esempio di Svelte che estende la sintassi JavaScript per renderla più utile, mantenendola comunque familiare.

### La sezione del markup

Nella sezione del markup puoi inserire qualsiasi HTML desideri, e in aggiunta puoi inserire espressioni JavaScript valide all'interno di singole parentesi graffe (`{}`). In questo caso stiamo includendo il valore della prop `name` subito dopo il testo `Hello`.

```html
<main>
  <h1>Hello {name}!</h1>
  <p>
    Visit the <a href="https://learn.svelte.dev/">Svelte tutorial</a> to learn
    how to build Svelte apps.
  </p>
</main>
```

Svelte supporta anche tag come `{#if}`, `{#each}`, e `{#await}` — questi esempi ti permettono di rendere condizionalmente una porzione del markup, iterare attraverso una lista di elementi e lavorare con valori asincroni, rispettivamente.

### La sezione `<style>`

Se hai esperienza nella lavorazione del CSS, il seguente snippet dovrebbe avere senso:

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

Stiamo applicando uno stile al nostro elemento [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements). Cosa succederà agli altri componenti con elementi `<h1>`?

In Svelte, il CSS all'interno del blocco `<style>` di un componente sarà vincolato solo a quel componente. Ciò avviene aggiungendo una classe agli elementi selezionati, basata su un hash degli stili del componente.

Puoi vedere questo in azione aprendo `localhost:8080` in una nuova scheda del browser, facendo clic destro/<kbd>Ctrl</kbd>-clic sull'etichetta _HELLO WORLD!_ e scegliendo _Ispeziona_:

![App di partenza di Svelte con DevTools aperti, che mostra le classi per gli stili con ambito](02-svelte-component-scoped-styles.png)

Quando compila l'app, Svelte cambia la nostra definizione di stili `h1` in `h1.svelte-1tky8bj`, e poi modifica ogni elemento `<h1>` nel nostro componente in `<h1 class="svelte-1tky8bj">`, in modo che esso prenda gli stili richiesti.

> [!NOTE]
> Puoi sovrascrivere questo comportamento e applicare stili a un selettore globalmente utilizzando il modificatore `:global()` (vedi i [documenti `<style>` di Svelte](https://svelte.dev/docs/svelte-components#style) per ulteriori informazioni).

## Apportare alcune modifiche

Ora che abbiamo un'idea generale di come tutto si unisce, possiamo iniziare a fare alcune modifiche.
A questo punto puoi provare a aggiornare il tuo componente `App.svelte` — ad esempio cambia l'elemento `<h1>` in `App.svelte` in modo che legga come segue:

```html
<h1>Hello {name} from MDN!</h1>
```

Basta salvare le modifiche e l'app in esecuzione su `localhost:8080` verrà aggiornata automaticamente.

### Una prima occhiata alla reattività di Svelte

Nel contesto di un framework UI, la reattività significa che il framework può aggiornare automaticamente il DOM quando lo stato di qualsiasi componente viene modificato.

In Svelte, la reattività viene attivata assegnando un nuovo valore a qualsiasi variabile di livello superiore in un componente. Ad esempio, potremmo includere una funzione `toggleName()` nel nostro componente `App`, e un pulsante per eseguirla.

Prova a aggiornare le sezioni `<script>` e markup come segue:

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

Come puoi vedere, l'etichetta `<h1>` viene aggiornata automaticamente. Dietro le quinte, Svelte ha creato il codice JavaScript per aggiornare il DOM ogni volta che il valore della variabile name cambia, senza usare alcun DOM virtuale o altro meccanismo di riconciliazione complesso.

Nota l'uso di `:` in `on:click`. Questa è la sintassi di Svelte per ascoltare gli eventi DOM.

## Ispezionare main.js: il punto di ingresso della nostra app

Apriamo `src/main.js`, che è dove il componente `App` viene importato e utilizzato. Questo file è il punto di ingresso per la nostra app, e inizialmente appare così:

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

`main.js` inizia importando il componente Svelte che utilizzeremo. Poi viene istanziato con `new App`, passando un oggetto di opzione con le seguenti proprietà:

- `target`: L'elemento DOM all'interno del quale vogliamo che il componente venga renderizzato, in questo caso l'elemento `<body>`.
- `props`: i valori da assegnare a ciascun prop del componente `App`.

## Uno sguardo dietro le quinte

Come fa Svelte a gestire tutti questi file e a farli funzionare bene insieme?

Il compilatore di Svelte elabora la sezione `<style>` di ogni componente e li compila nel file `public/build/bundle.css`.

Compila anche il markup e la sezione `<script>` di ogni componente, e memorizza il risultato in `public/build/bundle.js`. Aggiunge anche il codice in `src/main.js` per fare riferimento alle caratteristiche di ciascun componente.

Infine, il file `public/index.html` include i file `bundle.css` e `bundle.js` generati:

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

La versione minificata di `bundle.js` pesa poco più di 3KB, che include il "runtime di Svelte" (solo 300 righe di codice JavaScript) e il componente `App.svelte` compilato. Come puoi vedere, `bundle.js` è l'unico file JavaScript referenziato da `index.html`. Non ci sono altre librerie caricate nella pagina web.

Questo è un footprint molto più piccolo rispetto ai bundle compilati da altri framework. Tieni presente che, nel caso dei bundle di codice, non è solo la dimensione dei file che devi scaricare a essere importante. Questo è codice eseguibile che deve essere analizzato, eseguito e mantenuto in memoria. Quindi questo fa davvero la differenza, soprattutto su dispositivi a bassa potenza o in applicazioni ad alta intensità di CPU.

## Seguire questo tutorial

In questa serie di tutorial costruirai un'applicazione web completa. Impareremo tutte le basi su Svelte e anche alcuni argomenti avanzati.

Puoi semplicemente leggere il contenuto per ottenere una buona comprensione delle funzionalità di Svelte, ma otterrai il massimo da questo tutorial se seguirai insieme a noi la codifica dell'applicazione. Per rendere più semplice per te seguire ogni articolo, forniamo un repository GitHub con una cartella contenente il codice sorgente dell'app così com'è all'inizio di ogni tutorial.

Svelte fornisce anche un REPL online, che è un playground per codificare in diretta le app Svelte sul web senza dover installare nulla sul tuo computer. Forniamo un REPL per ogni articolo in modo che tu possa iniziare a codificare subito. Parliamo un po' di più su come usare questi strumenti.

### Usare Git

Il sistema di controllo delle versioni più popolare è Git, insieme a GitHub, un sito che fornisce l'hosting per i tuoi repository e diversi strumenti per lavorare con essi.

Utilizzeremo GitHub in modo che tu possa scaricare facilmente il codice sorgente per ogni articolo. Sarai anche in grado di ottenere il codice come dovrebbe essere dopo aver completato l'articolo, nel caso ti perdessi.

Dopo aver [installato git](https://git-scm.com/downloads), per clonare il repository dovresti eseguire:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi all'inizio di ogni articolo, puoi semplicemente `cd` nella cartella corrispondente e avviare l'app in modalità di sviluppo per vedere quale dovrebbe essere il suo stato attuale, come questo:

```bash
cd 02-starting-our-todo-app
npm install
npm run dev
```

Se vuoi imparare di più su git e GitHub, abbiamo compilato una lista di link a guide utili — vedi [Git e GitHub](/it/docs/Learn_web_development/Core/Version_control).

> [!NOTE]
> Se vuoi solo scaricare i file senza clonare il repository git, puoi usare lo strumento degit in questo modo — `npx degit opensas/mdn-svelte-tutorial`. Puoi anche scaricare una cartella specifica con `npx degit opensas/mdn-svelte-tutorial/01-getting-started`. Degit non creerà un repository git locale, scaricherà solo i file della cartella specificata.

### Usare il REPL di Svelte

Un REPL ([read–eval–print loop](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)) è un ambiente interattivo che ti permette di inserire comandi e vedere immediatamente i risultati — molti linguaggi di programmazione forniscono un REPL.

Il REPL di Svelte è molto più di questo. È uno strumento online che ti permette di creare app complete, salvarle online e condividerle con gli altri.

È il modo più semplice per iniziare a giocare con Svelte da qualsiasi macchina, senza dover installare nulla. È anche ampiamente utilizzato dalla comunità Svelte. Se vuoi condividere un'idea, chiedere aiuto o segnalare un problema, è sempre estremamente utile creare un'istanza REPL che dimostri il problema.

Diamo un'occhiata veloce al REPL di Svelte e a come lo usi. Appare così:

![il REPL di Svelte in azione, mostra il codice del componente a sinistra e l'output a destra](03-svelte-repl-in-action.png)

Per avviare un REPL, apri il tuo browser e naviga a <https://svelte.dev/repl>.

- Sul lato sinistro dello schermo vedrai il codice dei tuoi componenti, e a destra vedrai l'output in tempo reale della tua app.
- La barra sopra il codice ti permette di creare file `.svelte` e `.js` e di riorganizzarli. Per creare un file all'interno di una cartella, basta specificare il percorso completo, come questo: `components/MyComponent.svelte`. La cartella verrà creata automaticamente.
- Sopra quella barra hai il titolo del REPL. Clicca su di esso per modificarlo.
- Sul lato destro hai tre schede:

  - La scheda _Result_ mostra l'output della tua app e fornisce una console in basso.
  - La scheda _JS output_ ti permette di ispezionare il codice JavaScript generato da Svelte e di impostare le opzioni del compilatore.
  - La scheda _CSS output_ mostra il CSS generato da Svelte.

- Sopra le schede, troverai una barra degli strumenti che ti permette di entrare in modalità schermo intero e di scaricare la tua app. Se accedi con un account GitHub, potrai anche fare il fork e salvare l'app. Potrai anche vedere tutti i tuoi REPL salvati cliccando sul tuo profilo GitHub e selezionando _Your saved apps_.

Ogni volta che cambi un file nel REPL, Svelte ricompilerà l'app e aggiornerà la scheda Result. Per condividere la tua app, condividi l'URL. Ad esempio, ecco il link per un REPL che esegue la nostra app completa: <https://svelte.dev/repl/378dd79e0dfe4486a8f10823f3813190?version=3.23.2>.

> [!NOTE]
> Nota come puoi specificare la versione di Svelte nell'URL. Questo è utile quando si segnalano problemi relativi a una versione specifica di Svelte.

Forniamo un REPL all'inizio e alla fine di ogni articolo in modo che tu possa iniziare a codificare subito con noi.

> [!NOTE]
> Al momento il REPL non può gestire correttamente i nomi delle cartelle. Se stai seguendo il tutorial sul REPL, crea tutti i tuoi componenti all'interno della cartella radice. Poi quando vedi un percorso nel codice, ad esempio `import Todos from './components/Todos.svelte'`, sostituiscilo con un URL piatto, ad esempio `import Todos from './Todos.svelte'`.

## Il codice fino ad ora

### Git

Clona il repository GitHub (se non lo hai già fatto) con:

```bash
git clone https://github.com/opensas/mdn-svelte-tutorial.git
```

Poi per arrivare allo stato attuale dell'applicazione, esegui

```bash
cd mdn-svelte-tutorial/01-getting-started
```

Oppure scarica direttamente il contenuto della cartella:

```bash
npx degit opensas/mdn-svelte-tutorial/01-getting-started
```

Ricorda di eseguire `npm install && npm run dev` per avviare la tua app in modalità di sviluppo.

### REPL

Per seguire la codifica con noi utilizzando il REPL, inizia da

<https://svelte.dev/repl/fc68b4f059d34b9c84fa042d1cce586c?version=3.23.2>

## Sommario

Questo ci porta alla fine della nostra prima occhiata a Svelte, includendo come installarlo localmente, creare un'app di partenza, e come funzionano le basi. Nel prossimo articolo inizieremo a costruire la nostra prima applicazione vera e propria, una lista di cose da fare. Prima di farlo, tuttavia, ricapitoliamo alcune delle cose che abbiamo imparato.

In Svelte:

- Definiamo lo script, lo stile e il markup di ciascun componente in un singolo file `.svelte`.
- I prop dei componenti sono dichiarati con la parola chiave `export`.
- I componenti Svelte possono essere utilizzati semplicemente importando il corrispondente file `.svelte`.
- Gli stili dei componenti sono vincolati, evitando che si sovrappongano tra loro.
- Nella sezione del markup puoi includere qualsiasi espressione JavaScript mettendola tra parentesi graffe.
- Le variabili a livello superiore di un componente costituiscono il suo stato.
- La reattività viene attivata semplicemente assegnando un nuovo valore a una variabile a livello superiore.

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Svelte_todo_list_beginning", "Learn_web_development/Core/Frameworks_libraries")}}
