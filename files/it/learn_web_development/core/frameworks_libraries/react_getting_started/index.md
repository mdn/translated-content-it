---
title: Iniziare con React
short-title: Introduzione a React
slug: Learn_web_development/Core/Frameworks_libraries/React_getting_started
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Main_features","Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning", "Learn_web_development/Core/Frameworks_libraries")}}

In questo articolo ci saluteremo con React. Scopriremo dei dettagli sul suo background e sui casi d'uso, imposteremo una toolchain React di base sul nostro computer locale e creeremo e giocheremo con un'app iniziale semplice, imparando un po' su come funziona React nel processo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/linea di comando</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
          Configurare un ambiente di sviluppo React locale, creare un'app iniziale e comprendere le basi di come funziona.
      </td>
    </tr>
  </tbody>
</table>

## Ciao React

Come afferma il suo slogan ufficiale, [React](https://react.dev/) è una libreria per creare interfacce utente. React non è un framework – non è nemmeno esclusivo del web. Viene utilizzato con altre librerie per il rendering in determinati ambienti. Ad esempio, [React Native](https://reactnative.dev/) può essere utilizzato per creare applicazioni mobili.

Per sviluppare per il web, gli sviluppatori utilizzano React in tandem con [ReactDOM](https://react.dev/reference/react-dom). React e ReactDOM sono spesso discussi negli stessi contesti di — e utilizzati per risolvere gli stessi problemi di — altri veri framework di sviluppo web. Quando ci riferiamo a React come un "framework", stiamo lavorando con quella comprensione colloquiale.

L'obiettivo principale di React è minimizzare i bug che si verificano quando gli sviluppatori creano interfacce utente. Lo fa tramite l'uso di componenti — pezzi di codice logici e autonomi che descrivono una parte dell'interfaccia utente. Questi componenti possono essere composti insieme per creare un'intera interfaccia utente, e React astrae gran parte del lavoro di rendering, lasciando a lei la concentrazione sul design dell'interfaccia utente.

## Casi d'uso

A differenza degli altri framework trattati in questo modulo, React non impone regole rigide sui conventi di codice o sull'organizzazione dei file. Ciò consente ai team di stabilire convenzioni che funzionano meglio per loro e di adottare React nel modo che preferiscono. React può gestire un singolo pulsante, alcuni elementi di un'interfaccia o l'intera interfaccia utente di un'app.

Mentre React _può_ essere utilizzato per [piccoli pezzi di un'interfaccia](https://react.dev/learn/add-react-to-an-existing-project), non è così facile "inserirlo" in un'applicazione come una libreria come jQuery, o anche un framework come Vue — è più accessibile quando si costruisce tutta l'app con React.

Inoltre, molti dei vantaggi dell'esperienza di sviluppo di un'app React, come la scrittura di interfacce con JSX, richiedono un processo di compilazione. L'aggiunta di un compilatore come Babel a un sito web fa sì che il codice su esso eseguito vada lentamente, quindi gli sviluppatori spesso configurano tali strumenti con un fase di costruzione. React ha probabilmente una pesante richiesta di strumenti, ma può essere appresa.

Questo articolo si concentrerà sul caso d'uso di utilizzo di React per il rendering dell'intera interfaccia utente di un'applicazione con il supporto di [Vite](https://vite.dev/), un moderno strumento di build front-end.

## Come React utilizza JavaScript

React utilizza caratteristiche del moderno JavaScript per molti dei suoi modelli. Il suo più grande distacco da JavaScript arriva con l'uso della sintassi [JSX](https://react.dev/learn/writing-markup-with-jsx). JSX estende la sintassi di JavaScript in modo che il codice simile a HTML possa convivere accanto ad esso. Ad esempio:

```jsx
const heading = <h1>Mozilla Developer Network</h1>;
```

Questa costante di intestazione è conosciuta come un'espressione **JSX**. React può usarla per eseguire il rendering di quel tag [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements) nella nostra app.

Supponiamo di voler racchiudere la nostra intestazione in un tag [`<header>`](/it/docs/Web/HTML/Reference/Elements/header), per ragioni semantiche? L'approccio JSX ci permette di annidare i nostri elementi l'uno nell'altro, proprio come facciamo con HTML:

```jsx
const header = (
  <header>
    <h1>Mozilla Developer Network</h1>
  </header>
);
```

> [!NOTE]
> Le parentesi nello snippet precedente non sono uniche per JSX e non hanno alcun effetto sulla sua applicazione. Sono un segnale per lei (e il suo computer) che le più righe di codice all'interno fanno parte della stessa espressione. Potrebbe anche scrivere l'espressione dell'intestazione in questo modo:
>
> ```jsx-nolint
> const header = <header>
>   <h1>Mozilla Developer Network</h1>
> </header>;
> ```
>
> Tuttavia, questo sembra un po' goffo, perché il tag [`<header>`](/it/docs/Web/HTML/Reference/Elements/header) che inizia l'espressione non è rientrato nella stessa posizione del suo corrispondente tag di chiusura.

Ovviamente, il suo browser non può leggere JSX senza aiuto. Una volta compilata (usando uno strumento come [Babel](https://babeljs.io/) o [Parcel](https://parceljs.org/)), la nostra espressione dell'intestazione apparirebbe così:

```jsx
const header = React.createElement(
  "header",
  null,
  React.createElement("h1", null, "Mozilla Developer Network"),
);
```

È _possibile_ saltare il passaggio di compilazione e usare [`React.createElement()`](https://react.dev/reference/react/createElement) per scrivere lei stesso l'interfaccia utente. Facendo ciò, tuttavia, perde il vantaggio dichiarativo di JSX e il suo codice diventa più difficile da leggere. La compilazione è un passaggio extra nel processo di sviluppo, ma molti sviluppatori della comunità React pensano che la leggibilità di JSX sia valida. Inoltre, lo sviluppo moderno di front-end comporta quasi sempre un processo di costruzione, comunque — deve abbassare la sintassi moderna per essere compatibile con i browser più vecchi, e potrebbe voler {{Glossary("Minification", "minificare")}} il suo codice per ottimizzare le prestazioni di caricamento. Strumenti popolari come Babel già includono il supporto JSX di base, quindi non deve configurare la compilazione da solo, a meno che non voglia.

Poiché JSX è una miscela di HTML e JavaScript, alcuni sviluppatori lo trovano intuitivo. Altri dicono che la sua natura combinata lo rende confuso. Una volta a suo agio con esso, tuttavia, le permetterà di costruire interfacce utente più rapidamente e intuitivamente e permetterà ad altri di comprendere meglio il suo codice a colpo d'occhio.

Per saperne di più su JSX, consulti l'articolo del team React [Writing Markup with JSX](https://react.dev/learn/writing-markup-with-jsx).

## Configurare la sua prima app React

Ci sono molti modi per creare una nuova applicazione React. Useremo Vite per creare una nuova applicazione tramite la riga di comando.

È possibile [aggiungere React a un progetto esistente](https://react.dev/learn/add-react-to-an-existing-project) copiando alcuni elementi [`<script>`](/it/docs/Web/HTML/Reference/Elements/script) in un file HTML, ma l'uso di Vite le permetterà di trascorrere più tempo a costruire la sua app e meno tempo a preoccuparsi della configurazione.

### Requisiti

Per utilizzare Vite, deve avere installato [Node.js](https://nodejs.org/en/). Dalla versione 5.0 di Vite, è richiesta almeno la versione 18 di Node o successiva, ed è una buona idea eseguire l'ultima versione di supporto a lungo termine (LTS) quando può. Al 24 ottobre 2023, Node 20 è l'ultima versione LTS. Node include npm (il gestore di pacchetti Node).

Per controllare la versione di Node, esegua quanto segue nel suo terminale:

```bash
node -v
```

Se Node è installato, vedrà un numero di versione. Se non lo è, vedrà un messaggio di errore. Per installare Node, segua le istruzioni sul [sito di Node.js](https://nodejs.org/en/).

Può utilizzare il gestore di pacchetti Yarn come alternativa a npm, ma supporremo che stia usando npm in questo set di tutorial. Veda [Nozioni di base sulla gestione dei pacchetti](/it/docs/Learn_web_development/Extensions/Client-side_tools/Package_management) per ulteriori informazioni su npm e yarn.

Se usa Windows, dovrà installare del software per ottenere la parità con il terminale Unix/macOS per utilizzare i comandi del terminale menzionati in questo tutorial. **Gitbash** (che fa parte del toolkit [git for Windows](https://gitforwindows.org/)) o il **[Sottosistema Windows per Linux](https://learn.microsoft.com/en-us/windows/wsl/about)** (**WSL**) sono entrambi adatti. Veda [Corso accelerato sulla riga di comando](/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line) per maggiori informazioni su di essi e sui comandi del terminale in generale.

Tenga presente che React e ReactDOM producono app che funzionano solo su un set di browser abbastanza moderni come Firefox, Microsoft Edge, Safari o Chrome quando lavora attraverso questi tutorial.

Veda i seguenti per ulteriori informazioni:

- ["About npm" sul blog npm](https://docs.npmjs.com/about-npm/)
- ["Introducing npx" sul blog npm](https://blog.npmjs.org/post/162869356040/introducing-npx-an-npm-package-runner)
- [Documentazione di Vite](https://vite.dev/guide/)

### Inizializzare la sua app

Il gestore di pacchetti npm viene fornito con un comando `create` che le consente di creare nuovi progetti da modelli. Possiamo usarlo per creare una nuova app dal modello React standard di Vite. Assicuratevi di `cd` nel luogo in cui desiderate che la vostra app risieda sul vostro computer, quindi eseguite il seguente comando nel terminale:

```bash
npm create vite@latest moz-todo-react -- --template react
```

Questo crea una directory `moz-todo-react` utilizzando il modello `react` di Vite.

> [!NOTE]
> Il `--` è necessario per passare argomenti ai comandi npm come `create`, e l'argomento `--template react` indica a Vite di utilizzare il suo modello React.

Il suo terminale avrà stampato alcuni messaggi se questo comando ha avuto successo. Dovrebbe vedere un testo che la invita a `cd` nella sua nuova directory, installare le dipendenze dell'app ed eseguire l'app localmente. Iniziamo con due di questi comandi. Esegua quanto segue nel suo terminale:

```bash
cd moz-todo-react && npm install
```

Una volta completato il processo, dobbiamo avviare un server di sviluppo locale per eseguire la nostra app. Qui, aggiungeremo alcuni flag della riga di comando al suggerimento predefinito di Vite per aprire l'app nel nostro browser non appena il server si avvia, e utilizzare la porta 3000.

Esegua quanto segue nel suo terminale:

```bash
npm run dev -- --open --port 3000
```

Una volta che il server parte, dovrebbe vedere una nuova scheda del browser contenente la sua app React:

![Screenshot di Firefox macOS aperto su localhost:3000, che mostra un'applicazione realizzata con il modello React di Vite](default-vite.png)

### Struttura dell'applicazione

Vite ci fornisce tutto ciò di cui abbiamo bisogno per sviluppare un'applicazione React. La sua struttura iniziale dei file appare così:

```plain
moz-todo-react
├── README.md
├── index.html
├── node_modules
├── package-lock.json
├── package.json
├── public
│   └── vite.svg
├── src
│   ├── App.css
│   ├── App.jsx
│   ├── assets
│   │   └── react.svg
│   ├── index.css
│   └── main.jsx
└── vite.config.js
```

**`index.html`** è il file di livello superiore più importante. Vite inietta il suo codice in questo file in modo che il suo browser possa eseguirlo. Non sarà necessario modificare questo file durante il nostro tutorial, ma dovrebbe cambiare il testo all'interno dell'elemento [`<title>`](/it/docs/Web/HTML/Reference/Elements/title) in questo file per riflettere il titolo della sua applicazione. I titoli accurati delle pagine sono importanti per l'accessibilità.

La directory **`public`** contiene file statici che verranno serviti direttamente al suo browser senza essere elaborati dagli strumenti di build di Vite. Attualmente, contiene solo un logo di Vite.

La directory **`src`** è dove trascorreremo la maggior parte del nostro tempo, poiché è dove risiede il codice sorgente della nostra applicazione. Noterà che alcuni file JavaScript in questa directory terminano con l'estensione `.jsx`. Questa estensione è necessaria per qualsiasi file che contenga JSX – informa Vite di trasformare la sintassi JSX in JavaScript che il suo browser può comprendere. La directory `src/assets` contiene il logo React che ha visto nel browser.

I file `package.json` e `package-lock.json` contengono i metadati del nostro progetto. Questi file non sono unici per le applicazioni React: Vite ha popolato `package.json` per noi, e npm ha creato `package-lock.json` per quando abbiamo installato le dipendenze dell'app. Non ha bisogno di comprendere questi file per completare questo tutorial. Tuttavia, se desidera saperne di più su di essi, può leggere su [`package.json`](https://docs.npmjs.com/cli/v9/configuring-npm/package-json/) e [`package-lock.json`](https://docs.npmjs.com/cli/v9/configuring-npm/package-lock-json/) nella documentazione npm. Parliamo anche di `package.json` nel nostro tutorial [Nozioni di base sulla gestione dei pacchetti](/it/docs/Learn_web_development/Extensions/Client-side_tools/Package_management).

### Personalizzare il nostro script di sviluppo

Prima di andare avanti, potrebbe voler cambiare un po' il suo file `package.json` in modo da non dover passare i flag `--open` e `--port` ogni volta che esegue `npm run dev`. Apra `package.json` nel suo editor di testo e trovi l'oggetto `scripts`. Cambi la chiave `"dev"` in modo che sembri così:

```diff
- "dev": "vite",
+ "dev": "vite --open --port 3000",
```

Con questo in atto, la sua app si aprirà nel suo browser su `http://localhost:3000` ogni volta che esegue `npm run dev`.

> [!NOTE]
> Non ha _bisogno dell'`--` extra qui perché stiamo passando argomenti direttamente a `vite`, piuttosto che a uno script npm predefinito.

## Esplorare il nostro primo componente React — `<App />`

In React, un **componente** è un modulo riutilizzabile che esegue il rendering di una parte della nostra applicazione complessiva. I componenti possono essere grandi o piccoli, ma sono solitamente definiti chiaramente: servono a uno scopo singolare e ovvio.

Apriamo `src/App.jsx`, poiché il nostro browser ci sta invitando a modificarlo. Questo file contiene il nostro primo componente, `<App />`:

```jsx
import { useState } from "react";
import reactLogo from "./assets/react.svg";
import viteLogo from "/vite.svg";
import "./App.css";

function App() {
  const [count, setCount] = useState(0);

  return (
    <>
      <div>
        <a href="https://vite.dev" target="_blank">
          <img src={viteLogo} className="logo" alt="Vite logo" />
        </a>
        <a href="https://react.dev" target="_blank">
          <img src={reactLogo} className="logo react" alt="React logo" />
        </a>
      </div>
      <h1>Vite + React</h1>
      <div className="card">
        <button onClick={() => setCount((count) => count + 1)}>
          count is {count}
        </button>
        <p>
          Edit <code>src/App.jsx</code> and save to test HMR
        </p>
      </div>
      <p className="read-the-docs">
        Click on the Vite and React logos to learn more
      </p>
    </>
  );
}

export default App;
```

Il file `App.jsx` consiste di tre parti principali: alcune istruzioni [`import`](/it/docs/Web/JavaScript/Reference/Statements/import) in alto, la funzione `App()` al centro e un'istruzione [`export`](/it/docs/Web/JavaScript/Reference/Statements/export) alla fine. La maggior parte dei componenti React segue questo schema.

### Istruzioni di importazione

Le istruzioni `import` in cima al file permettono a `App.jsx` di utilizzare il codice che è stato definito altrove. Esaminiamo queste istruzioni più da vicino.

```jsx
import { useState } from "react";
import reactLogo from "./assets/react.svg";
import viteLogo from "/vite.svg";
import "./App.css";
```

La prima istruzione importa il hook `useState` dalla libreria `react`. I hook sono un modo per utilizzare le caratteristiche di React all'interno di un componente. Parleremo dei hook più avanti in questo tutorial.

Successivamente, importiamo `reactLogo` e `viteLogo`. Noti che i loro percorsi di importazione iniziano rispettivamente con `./` e `/` e che terminano con l'estensione `.svg` alla fine. Questo ci dice che queste importazioni sono _locali_, riferendosi ai nostri file piuttosto che ai pacchetti npm.

L'ultima istruzione importa il CSS relativo al nostro componente `<App />`. Noti che non c'è alcun nome di variabile e nessuna direttiva `from`. Questo è chiamato [importazione con _effetti collaterali_](/it/docs/Web/JavaScript/Reference/Statements/import#import_a_module_for_its_side_effects_only) — non importa alcun valore nel file JavaScript, ma informa Vite di aggiungere il file CSS di riferimento all'output finale del codice, in modo che possa essere utilizzato nel browser.

### La funzione `App()`

Dopo le importazioni, abbiamo una funzione chiamata `App()`, che definisce la struttura del componente `App`. Mentre la maggior parte della comunità JavaScript preferisce nomi in {{Glossary("camel_case", "camel case minuscolo")}} come `helloWorld`, i componenti React utilizzano nomi di variabili in Pascal Case (o camel case maiuscolo), come `HelloWorld`, per chiarire che un dato elemento JSX è un componente React e non un normale tag HTML. Se rinomasse la funzione `App()` in `app()`, il suo browser mostrerebbe un errore.

Esaminiamo `App()` più da vicino.

```jsx
function App() {
  const [count, setCount] = useState(0);

  return (
    <>
      <div>
        <a href="https://vite.dev" target="_blank">
          <img src={viteLogo} className="logo" alt="Vite logo" />
        </a>
        <a href="https://react.dev" target="_blank">
          <img src={reactLogo} className="logo react" alt="React logo" />
        </a>
      </div>
      <h1>Vite + React</h1>
      <div className="card">
        <button onClick={() => setCount((count) => count + 1)}>
          count is {count}
        </button>
        <p>
          Edit <code>src/App.jsx</code> and save to test HMR
        </p>
      </div>
      <p className="read-the-docs">
        Click on the Vite and React logos to learn more
      </p>
    </>
  );
}
```

La funzione `App()` restituisce un'espressione JSX. Questa espressione definisce cosa verrà renderizzato nel DOM dal suo browser.

Subito sotto la keyword `return` c'è una sintassi speciale: `<>`. Questo è un [frammento](https://react.dev/reference/react/Fragment). I componenti React devono restituire un singolo elemento JSX, e i frammenti ci permettono di farlo senza rendere arbitrari `<div>` nel browser. Vedrà i frammenti in molte applicazioni React.

### L'istruzione `export`

C'è un'altra riga di codice dopo la funzione `App()`:

```jsx
export default App;
```

Questa istruzione di esportazione rende la nostra funzione `App()` disponibile ad altri moduli. Ne parleremo più ampiamente più avanti.

## Passare a `main`

Apriamo `src/main.jsx`, perché è lì che viene utilizzato il componente `<App />`. Questo file è il punto di ingresso per la nostra app, e inizialmente appare così:

```jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import "./index.css";
import App from "./App.jsx";

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <App />
  </StrictMode>,
);
```

Come con `App.jsx`, il file inizia importando tutti i moduli JavaScript e altri asset che servono per eseguire l'app.

Le prime due istruzioni importano `StrictMode` e `createRoot` dalle librerie `react` e `react-dom` perché vengono riferiti più avanti nel file. Non scriviamo un percorso o un'estensione quando importiamo queste librerie perché non sono file locali. In realtà, sono elencate come dipendenze nel nostro file `package.json`. Si assicuri di questa distinzione mentre lavora attraverso questa lezione!

Poi importiamo la nostra funzione `App()` e `index.css`, che contiene stili globali che vengono applicati alla nostra intera app.

Poi chiamiamo la funzione `createRoot()`, che definisce il nodo root della nostra applicazione. Questo prende come argomento l'elemento del DOM all'interno del quale vogliamo che la nostra app React venga renderizzata. In questo caso, è l'elemento del DOM con l'ID `root`. Infine, cateniamo il metodo `render()` alla chiamata di `createRoot()`, passando l'espressione JSX che vogliamo rendere all'interno del root. Scrivendo `<App />` come questa espressione JSX, stiamo dicendo a React di chiamare la funzione _App()_, che esegue il rendering del componente _App_ all'interno del nodo root.

> **Nota:** `<App />` viene renderizzato all'interno di un componente speciale `<React.StrictMode>`. Questo componente aiuta gli sviluppatori a rilevare potenziali problemi nel loro codice.

Potrebbe voler leggere ulteriormente su queste API di React, se desidera:

- [`ReactDOM.createRoot()`](https://react.dev/reference/react-dom/client/createRoot)
- [`React.StrictMode`](https://react.dev/reference/react/StrictMode)

## Iniziare da zero

Prima di iniziare a costruire la nostra app, cancelleremo un po' del codice preesistente che Vite ci ha fornito.

Per prima cosa, come esperimento, cambi l'elemento [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements) in `App.jsx` in modo che legga "Ciao, Mondo!", poi salvi il file. Noterà che questo cambiamento viene immediatamente renderizzato nel server di sviluppo in esecuzione su `http://localhost:3000` nel suo browser. Tenga presente questo mentre lavora alla sua app.

Non useremo il resto del codice! Sostituisca il contenuto di `App.jsx` con il seguente:

```jsx
import "./App.css";

function App() {
  return (
    <>
      <header>
        <h1>Hello, World!</h1>
      </header>
    </>
  );
}

export default App;
```

## Pratica con JSX

Successivamente, useremo le nostre abilità JavaScript per acquisire una certa familiarità con la scrittura di JSX e lavorare con i dati in React. Parleremo di come aggiungere attributi agli elementi JSX, come scrivere commenti, come eseguire il rendering del contenuto da variabili e altre espressioni, e come passare dati nei componenti tramite props.

### Aggiungere attributi agli elementi JSX

Gli elementi JSX possono avere attributi, proprio come gli elementi HTML. Provi ad aggiungere un `<button>` sotto l'elemento `<h1>` nel suo file `App.jsx`, in questo modo:

```jsx
<button type="button">Click me!</button>
```

Quando salva il file, vedrà un pulsante con le parole `Click me!`. Il pulsante non fa ancora nulla, ma presto impareremo ad aggiungere interattività alla nostra app.

Alcuni attributi sono diversi dai loro omologhi HTML. Ad esempio, l'attributo `class` in HTML si traduce in `className` in JSX. Questo perché `class` è una parola riservata in JavaScript, e JSX è un'estensione di JavaScript. Se volesse aggiungere una classe `primary` al suo pulsante, la scriverebbe così:

```jsx
<button type="button" className="primary">
  Click me!
</button>
```

### Espressioni JavaScript come contenuto

A differenza di HTML, JSX ci consente di scrivere variabili e altre espressioni JavaScript proprio accanto agli altri contenuti. Dichiariamo una variabile chiamata `subject` proprio sopra la funzione `App()` nel suo file `App.jsx`:

```jsx
const subject = "React";
function App() {
  // code omitted for brevity
}
```

Poi, sostituisca la parola "Mondo" nell'elemento `<h1>` con `{subject}`:

```jsx
<h1>Hello, {subject}!</h1>
```

Salvi il file e controlli il suo browser. Dovrebbe vedere "Ciao, React!" renderizzato.

Le parentesi graffa intorno a `subject` sono un'altra caratteristica della sintassi di JSX. Le parentesi graffe dicono a React che vogliamo leggere il valore della variabile `subject`, piuttosto che renderizzare la stringa letterale `"subject"`. Può inserire qualsiasi espressione JavaScript valida all'interno delle parentesi graffa in JSX; React la valuterà e renderizzerà il _risultato_ dell'espressione come contenuto finale. Di seguito è riportata una serie di esempi, con commenti sopra che spiegano cosa renderizzerà ciascuna espressione:

```jsx-nolint
{/* Hello, React :)! */}
<h1>Hello, {subject + ' :)'}!</h1>
{/* Hello, REACT */}
<h1>Hello, {subject.toUpperCase()}</h1>
{/* Hello, 4! */}
<h1>Hello, {2 + 2}!</h1>
```

Anche i commenti in JSX sono scritti dentro parentesi graffa! Questo perché anche i commenti, tecnicamente, sono espressioni JavaScript. La sintassi del commento a blocchi `/* */` è necessaria affinché il suo programma sappia dove inizia e dove finisce il commento.

### Component props

I **props** sono un mezzo per passare dati a un componente React. La loro sintassi è identica a quella degli attributi, in effetti: `prop="value"`. La differenza è che mentre gli attributi sono passati agli elementi semplici, i props sono passati ai componenti React.

In React, il flusso dei dati è unidirezionale: i props possono essere passati solo dai componenti madre ai componenti figli.

Apriamo `main.jsx` e diamo al nostro componente `<App />` il suo primo prop.

Aggiunga un prop di `subject` alla chiamata del componente `<App />`, con un valore di `Clarice`. Una volta fatto, dovrebbe apparire qualcosa del genere:

```jsx
<App subject="Clarice" />
```

Tornando a `App.jsx`, rivisitiamo la funzione `App()`. Cambi la firma di `App()` in modo che accetti `props` come parametro e registri `props` alla console in modo possa ispezionarlo. Inoltre, elimini la costante `subject`; non ne abbiamo più bisogno. Il suo file `App.jsx` dovrebbe assomigliare a questo:

```jsx
function App(props) {
  console.log(props);
  return (
    // code omitted for brevity
  );
}
```

Salvi il suo file e controlli il suo browser. Vedrà uno sfondo vuoto senza contenuto. Questo perché stiamo cercando di leggere una variabile `subject` che non è più definita. Risolva questo problema commentando la linea `<h1>Hello {subject}!</h1>`.

> [!NOTE]
> Se il suo editor di codice comprende come analizzare JSX (la maggior parte degli editor moderni lo fa!), può utilizzare il suo collegamento incorporato per la creazione di commenti — `Ctrl + /` (su Windows) o `Cmd + /` (su macOS) — per creare commenti più rapidamente.

Salvi il file dopo aver commentato quella riga. Questa volta, dovrebbe vedere il suo pulsante "Click me!" renderizzato da solo. Se apre la console sviluppatore del suo browser, vedrà un messaggio che appare in questo modo:

```plain
Object { subject: "Clarice" }
```

La proprietà dell'oggetto `subject` corrisponde al prop `subject` che abbiamo aggiunto alla chiamata del nostro componente `<App />`, e la stringa `Clarice` corrisponde al suo valore. I props dei componenti in React sono sempre raccolti in oggetti in questo modo.

Usiamo questo prop `subject` per correggere l'errore nella nostra app. Tolga il commento alla riga `<h1>Hello, {subject}!</h1>` e la cambi in `<h1>Hello, {props.subject}!</h1>`, quindi elimini l'istruzione `console.log()`. Il suo codice dovrebbe assomigliare a questo:

```jsx
function App(props) {
  return (
    <>
      <header>
        <h1>Hello, {props.subject}!</h1>
        <button type="button" className="primary">
          Click me!
        </button>
      </header>
    </>
  );
}
```

Quando salva, l'app dovrebbe ora salutarla con "Hello, Clarice!". Se torna in `main.jsx`, modifichi il valore di `subject` e salvi, il testo cambierà.

Per esercitarsi ulteriormente, potrebbe provare ad aggiungere un ulteriore prop `greeting` alla chiamata del componente `<App />` all'interno di `main.jsx` e utilizzarlo insieme al prop `subject` all'interno di `App.jsx`.

## Riepilogo

Questo ci porta alla fine del nostro sguardo iniziale a React, incluso come installarlo localmente, creare un'app iniziale e come funzionano le basi. Nel prossimo articolo, inizieremo a costruire la nostra prima applicazione vera e propria — una lista di cose da fare. Prima di fare ciò, tuttavia, ricapitoliamo alcune delle cose che abbiamo imparato.

In React:

- I componenti possono importare i moduli di cui hanno bisogno e devono esportare se stessi in fondo ai loro file.
- Le funzioni dei componenti sono denominate con `PascalCase`.
- Può renderizzare espressioni JavaScript in JSX mettendole tra parentesi graffa, come `{così}`.
- Alcuni attributi JSX sono diversi dagli attributi HTML in modo che non entrino in conflitto con le parole riservate di JavaScript. Ad esempio, `class` in HTML si traduce in `className` in JSX.
- I props sono scritti proprio come gli attributi all'interno delle chiamate ai componenti e vengono passati ai componenti.

## Veda anche

- [Imparare React](https://scrimba.com/learn-react-c0e?via=mdn) <sup>[_Partner di apprendimento di MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il [corso di Scrimba](https://scrimba.com/?via=mdn) _Learn React_ è la guida introduttiva definitiva — il punto di partenza perfetto per qualsiasi principiante di React. Impari le basi di React moderno risolvendo più di 140 sfide di codifica interattiva e costruendo otto progetti divertenti.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Main_features","Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning", "Learn_web_development/Core/Frameworks_libraries")}}
