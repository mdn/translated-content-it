---
title: Iniziare con React
short-title: Iniziare con React
slug: Learn_web_development/Core/Frameworks_libraries/React_getting_started
l10n:
  sourceCommit: 52a81d8138473b6ac4bec77d0be4261cb0b76d41
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Main_features","Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning", "Learn_web_development/Core/Frameworks_libraries")}}

In questo articolo verrà presentato React. Verranno esaminati alcuni dettagli sulle sue origini e sui casi d'uso, verrà configurata una toolchain React di base sul computer locale e verrà creata e sperimentata una semplice applicazione iniziale, imparando nel frattempo qualcosa sul funzionamento di React.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi fondamentali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, nonché con il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/riga di comando</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
          Configurare un ambiente di sviluppo React locale, creare un'app iniziale e
          comprendere le basi del suo funzionamento.
      </td>
    </tr>
  </tbody>
</table>

## Ciao React

Come afferma il suo slogan ufficiale, [React](https://react.dev/) è una libreria per creare interfacce utente. React non è un framework e non è nemmeno esclusivo del web. Viene utilizzato con altre librerie per il rendering in determinati ambienti. Ad esempio, [React Native](https://reactnative.dev/) può essere utilizzato per creare applicazioni mobili.

Per sviluppare per il web, gli sviluppatori utilizzano React insieme a [ReactDOM](https://react.dev/reference/react-dom). React e ReactDOM vengono spesso discussi negli stessi contesti di altri veri framework per lo sviluppo web e utilizzati per risolvere gli stessi problemi. Quando ci si riferisce a React come a un "framework", si adotta questa accezione colloquiale.

L'obiettivo principale di React è ridurre al minimo i bug che si verificano quando gli sviluppatori creano interfacce utente. Lo fa attraverso l'uso di componenti: porzioni di codice autonome e logiche che descrivono una parte dell'interfaccia utente. Questi componenti possono essere composti insieme per creare un'interfaccia utente completa e React astrae gran parte del lavoro di rendering, consentendo di concentrarsi sulla progettazione dell'interfaccia utente.

## Casi d'uso

A differenza degli altri framework trattati in questo modulo, React non impone regole rigide sulle convenzioni del codice o sull'organizzazione dei file. Ciò consente ai team di definire le convenzioni più adatte alle loro esigenze e di adottare React nel modo che preferiscono. React può gestire un singolo pulsante, alcune parti di un'interfaccia o l'intera interfaccia utente di un'app.

Sebbene React _possa_ essere usato per [piccole parti di un'interfaccia](https://react.dev/learn/add-react-to-an-existing-project), non è altrettanto semplice da "inserire" in un'applicazione quanto una libreria come jQuery o persino un framework come Vue: è più accessibile quando l'intera app viene creata con React.

Inoltre, molti vantaggi per l'esperienza dello sviluppatore offerti da un'app React, come la scrittura di interfacce con JSX, richiedono un processo di compilazione. Aggiungere un compilatore come Babel a un sito web rallenta l'esecuzione del codice, quindi gli sviluppatori spesso configurano questi strumenti con una fase di build. Si può sostenere che React richieda una toolchain pesante, ma è possibile impararla.

Questo articolo si concentrerà sul caso d'uso di React per il rendering dell'intera interfaccia utente di un'applicazione con il supporto di [Vite](https://vite.dev/), un moderno strumento di build front-end.

## In che modo React usa JavaScript?

React utilizza funzionalità di JavaScript moderno per molti dei suoi pattern. La sua maggiore differenza rispetto a JavaScript consiste nell'uso della sintassi [JSX](https://react.dev/learn/writing-markup-with-jsx). JSX estende la sintassi di JavaScript in modo che codice simile a HTML possa convivere al suo interno. Ad esempio:

```jsx
const heading = <h1>Mozilla Developer Network</h1>;
```

Questa costante di intestazione è nota come **espressione JSX**. React può usarla per eseguire il rendering di quel tag [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements) nell'app.

Supponiamo di voler racchiudere l'intestazione in un tag [`<header>`](/it/docs/Web/HTML/Reference/Elements/header) per ragioni semantiche. L'approccio JSX consente di annidare gli elementi uno dentro l'altro, proprio come si fa con HTML:

```jsx
const header = (
  <header>
    <h1>Mozilla Developer Network</h1>
  </header>
);
```

> [!NOTE]
> Le parentesi nello snippet precedente non sono specifiche di JSX e non hanno alcun effetto sull'applicazione. Segnalano allo sviluppatore, e al computer, che le righe di codice al loro interno fanno parte della stessa espressione. L'espressione `header` potrebbe anche essere scritta in questo modo:
>
> ```jsx-nolint
> const header = <header>
>   <h1>Mozilla Developer Network</h1>
> </header>;
> ```
>
> Tuttavia, l'aspetto è piuttosto scomodo, perché il tag [`<header>`](/it/docs/Web/HTML/Reference/Elements/header) che avvia l'espressione non è rientrato nella stessa posizione del tag di chiusura corrispondente.

Naturalmente, il browser non può leggere JSX senza aiuto. Una volta compilata, tramite uno strumento come [Babel](https://babeljs.io/) o [Parcel](https://parceljs.org/), l'espressione `header` avrebbe questo aspetto:

```jsx
const header = React.createElement(
  "header",
  null,
  React.createElement("h1", null, "Mozilla Developer Network"),
);
```

È _possibile_ saltare la fase di compilazione e utilizzare [`React.createElement()`](https://react.dev/reference/react/createElement) per scrivere autonomamente l'interfaccia utente. Tuttavia, così facendo si perde il vantaggio dichiarativo di JSX e il codice diventa più difficile da leggere. La compilazione è un passaggio aggiuntivo nel processo di sviluppo, ma molti sviluppatori della comunità React ritengono che la leggibilità di JSX ne valga la pena. Inoltre, lo sviluppo front-end moderno coinvolge quasi sempre un processo di build: è necessario convertire la sintassi moderna in una versione compatibile con i browser meno recenti e potrebbe essere utile {{Glossary("Minification", "minificare")}} il codice per ottimizzare le prestazioni di caricamento. Strumenti diffusi come Babel includono già il supporto JSX pronto all'uso, quindi non è necessario configurare manualmente la compilazione, a meno che non lo si desideri.

Poiché JSX è una combinazione di HTML e JavaScript, alcuni sviluppatori lo trovano intuitivo. Altri affermano che la sua natura ibrida lo renda confuso. Tuttavia, una volta acquisita familiarità, consente di creare interfacce utente più rapidamente e intuitivamente e permette ad altri di comprendere meglio il codice a colpo d'occhio.

Per ulteriori informazioni su JSX, consultare l'articolo del team React [Writing Markup with JSX](https://react.dev/learn/writing-markup-with-jsx).

## Configurare la prima app React

Esistono molti modi per creare una nuova applicazione React. Verrà utilizzato Vite per creare una nuova applicazione tramite la riga di comando.

È possibile [aggiungere React a un progetto esistente](https://react.dev/learn/add-react-to-an-existing-project) copiando alcuni elementi [`<script>`](/it/docs/Web/HTML/Reference/Elements/script) in un file HTML, ma l'uso di Vite consentirà di dedicare più tempo alla creazione dell'app e meno tempo alla configurazione.

> [!NOTE]
> È possibile iniziare a scrivere codice React senza eseguire _alcuna_ configurazione locale lavorando con lo scrim [First React Code](https://scrimba.com/learn-react-c0e/~03uo?via=mdn) di Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>.
> È possibile provarlo liberamente prima di continuare.

### Requisiti

Per utilizzare Vite, deve essere installato [Node.js](https://nodejs.org/en/). A partire da Vite 5.0, è richiesta almeno la versione 18 o successiva di Node ed è consigliabile usare, quando possibile, l'ultima versione con supporto a lungo termine (LTS). Al 24 ottobre 2023, Node 20 è l'ultima versione LTS. Node include npm, il gestore di pacchetti Node.

Per controllare la versione di Node, eseguire quanto segue nel terminale:

```bash
node -v
```

Se Node è installato, verrà visualizzato un numero di versione. In caso contrario, verrà visualizzato un messaggio di errore. Per installare Node, seguire le istruzioni sul [sito web di Node.js](https://nodejs.org/en/).

È possibile usare il gestore di pacchetti Yarn in alternativa a npm, ma in questa serie di tutorial verrà considerato l'uso di npm. Per ulteriori informazioni su npm e yarn, consultare [Nozioni di base sulla gestione dei pacchetti](/it/docs/Learn_web_development/Extensions/Client-side_tools/Package_management).

Se si usa Windows, sarà necessario installare software che offra funzionalità equivalenti al terminale Unix/macOS per poter utilizzare i comandi del terminale menzionati in questo tutorial. Sono adatti sia **Git Bash**, incluso nel [set di strumenti git per Windows](https://gitforwindows.org/), sia **[Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/about)** (**WSL**). Per ulteriori informazioni su questi strumenti e sui comandi del terminale in generale, consultare [Corso rapido sulla riga di comando](/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line).

Tenere inoltre presente che React e ReactDOM producono app che funzionano solo su un insieme piuttosto moderno di browser, quali Firefox, Microsoft Edge, Safari o Chrome, durante lo svolgimento di questi tutorial.

Per ulteriori informazioni, consultare:

- ["About npm" sul blog di npm](https://docs.npmjs.com/about-npm/)
- ["Introducing npx" sul blog di npm](https://blog.npmjs.org/post/162869356040/introducing-npx-an-npm-package-runner)
- [Documentazione di Vite](https://vite.dev/guide/)

### Inizializzare l'app

Il gestore di pacchetti npm include un comando `create` che consente di creare nuovi progetti a partire da template. Può essere usato per creare una nuova app dal template React standard di Vite. Assicurarsi di eseguire `cd` nella posizione del computer in cui si desidera collocare l'app, quindi eseguire quanto segue nel terminale:

```bash
npm create vite@latest moz-todo-react -- --template react
```

Questo crea una directory `moz-todo-react` utilizzando il template `react` di Vite.

> [!NOTE]
> `--` è necessario per passare argomenti a comandi npm come `create`, mentre l'argomento `--template react` indica a Vite di utilizzare il suo template React.

Se il comando ha avuto esito positivo, il terminale avrà stampato alcuni messaggi. Dovrebbe essere visualizzato testo che invita a eseguire `cd` nella nuova directory, installare le dipendenze dell'app ed eseguire l'app localmente. Iniziamo con due di questi comandi. Eseguire quanto segue nel terminale:

```bash
cd moz-todo-react && npm install
```

Una volta completato il processo, è necessario avviare un server di sviluppo locale per eseguire l'app. Qui verranno aggiunti alcuni flag della riga di comando al suggerimento predefinito di Vite, in modo da aprire l'app nel browser non appena il server viene avviato e usare la porta 3000.

Eseguire quanto segue nel terminale:

```bash
npm run dev -- --open --port 3000
```

Una volta avviato il server, dovrebbe essere visualizzata una nuova scheda del browser contenente l'app React:

![Schermata di Firefox su macOS aperto su localhost:3000, che mostra un'applicazione creata dal template React di Vite](default-vite.png)

### Struttura dell'applicazione

Vite fornisce tutto ciò che serve per sviluppare un'applicazione React. La struttura iniziale dei file è simile a questa:

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

**`index.html`** è il file di primo livello più importante. Vite inserisce il codice in questo file affinché il browser possa eseguirlo. Non sarà necessario modificare questo file durante il tutorial, ma è opportuno cambiare il testo all'interno dell'elemento [`<title>`](/it/docs/Web/HTML/Reference/Elements/title) per riflettere il titolo dell'applicazione. Titoli di pagina accurati sono importanti per l'accessibilità.

La directory **`public`** contiene file statici che verranno forniti direttamente al browser senza essere elaborati dagli strumenti di build di Vite. Al momento contiene solo un logo Vite.

La directory **`src`** è quella in cui verrà trascorsa la maggior parte del tempo, poiché contiene il codice sorgente dell'applicazione. Si noterà che alcuni file JavaScript in questa directory terminano con l'estensione `.jsx`. Questa estensione è necessaria per ogni file che contiene JSX: indica a Vite di trasformare la sintassi JSX in JavaScript comprensibile dal browser. La directory `src/assets` contiene il logo React visualizzato nel browser.

I file `package.json` e `package-lock.json` contengono metadati sul progetto. Questi file non sono esclusivi delle applicazioni React: Vite ha popolato `package.json`, mentre npm ha creato `package-lock.json` al momento dell'installazione delle dipendenze dell'app. Non è necessario comprendere affatto questi file per completare il tutorial. Tuttavia, per approfondire, è possibile consultare la documentazione npm su [`package.json`](https://docs.npmjs.com/cli/v9/configuring-npm/package-json/) e [`package-lock.json`](https://docs.npmjs.com/cli/v9/configuring-npm/package-lock-json/). Si parla inoltre di `package.json` nel tutorial [Nozioni di base sulla gestione dei pacchetti](/it/docs/Learn_web_development/Extensions/Client-side_tools/Package_management).

### Personalizzare lo script di sviluppo

Prima di proseguire, potrebbe essere utile modificare leggermente il file `package.json` per non dover passare i flag `--open` e `--port` ogni volta che viene eseguito `npm run dev`. Aprire `package.json` nell'editor di testo e trovare l'oggetto `scripts`. Modificare la chiave `"dev"` affinché abbia questo aspetto:

```diff
- "dev": "vite",
+ "dev": "vite --open --port 3000",
```

Con questa configurazione, l'app verrà aperta nel browser su `http://localhost:3000` ogni volta che si esegue `npm run dev`.

> [!NOTE]
> Qui _non_ è necessario il secondo `--` perché gli argomenti vengono passati direttamente a `vite`, anziché a uno script npm predefinito.

## Esplorare il primo componente React — `<App />`

In React, un **componente** è un modulo riutilizzabile che esegue il rendering di una parte dell'applicazione complessiva. I componenti possono essere grandi o piccoli, ma generalmente sono chiaramente definiti: hanno un unico scopo evidente.

Aprire `src/App.jsx`, dato che il browser invita a modificarlo. Questo file contiene il primo componente, `<App />`:

```jsx
import { useState } from "react";
import viteLogo from "/vite.svg";
import reactLogo from "./assets/react.svg";
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

Il file `App.jsx` è composto da tre parti principali: alcune istruzioni [`import`](/it/docs/Web/JavaScript/Reference/Statements/import) nella parte superiore, la funzione `App()` al centro e un'istruzione [`export`](/it/docs/Web/JavaScript/Reference/Statements/export) nella parte inferiore. La maggior parte dei componenti React segue questo schema.

### Istruzioni import

Le istruzioni `import` nella parte superiore del file consentono a `App.jsx` di utilizzare codice definito altrove. Esaminiamo più da vicino queste istruzioni.

```jsx
import { useState } from "react";
import viteLogo from "/vite.svg";
import reactLogo from "./assets/react.svg";
import "./App.css";
```

La prima istruzione importa l'hook `useState` dalla libreria `react`. Gli hook sono un modo per usare le funzionalità di React all'interno di un componente. Gli hook verranno approfonditi più avanti in questo tutorial.

Successivamente vengono importati `reactLogo` e `viteLogo`. Si noti che i rispettivi percorsi di importazione iniziano con `./` e `/` e terminano con l'estensione `.svg`. Ciò indica che questi import sono _locali_, ovvero fanno riferimento a file propri anziché a pacchetti npm.

L'istruzione finale importa il CSS relativo al componente `<App />`. Si noti che non sono presenti né un nome di variabile né una direttiva `from`. Questo è chiamato [_import con effetto collaterale_](/it/docs/Web/JavaScript/Reference/Statements/import#import_a_module_for_its_side_effects_only): non importa alcun valore nel file JavaScript, ma indica a Vite di aggiungere il file CSS referenziato all'output finale del codice, affinché possa essere usato nel browser.

### La funzione `App()`

Dopo gli import, è presente una funzione denominata `App()`, che definisce la struttura del componente `App`. Mentre la maggior parte della comunità JavaScript preferisce nomi in {{Glossary("camel_case", "camel case minuscolo")}} come `helloWorld`, i componenti React utilizzano nomi di variabili in Pascal case, o camel case maiuscolo, come `HelloWorld`, per rendere chiaro che un dato elemento JSX è un componente React e non un normale tag HTML. Se la funzione `App()` venisse rinominata in `app()`, il browser genererebbe un errore.

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

La funzione `App()` restituisce un'espressione JSX. Questa espressione definisce ciò di cui il browser esegue infine il rendering nel DOM.

Subito sotto la parola chiave `return` si trova una sintassi speciale: `<>`. Si tratta di un [fragment](https://react.dev/reference/react/Fragment). I componenti React devono restituire un singolo elemento JSX e i fragment consentono di farlo senza eseguire il rendering di `<div>` arbitrari nel browser. I fragment sono presenti in molte applicazioni React.

### L'istruzione `export`

Dopo la funzione `App()` è presente un'altra riga di codice:

```jsx
export default App;
```

Questa istruzione di esportazione rende disponibile la funzione `App()` ad altri moduli. Questo aspetto verrà approfondito in seguito.

## Passare a `main`

Aprire `src/main.jsx`, perché è qui che viene utilizzato il componente `<App />`. Questo file è il punto di ingresso dell'app e inizialmente ha questo aspetto:

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

Come per `App.jsx`, il file inizia importando tutti i moduli JavaScript e le altre risorse necessari per l'esecuzione.

Le prime due istruzioni importano `StrictMode` e `createRoot` dalle librerie `react` e `react-dom`, poiché vengono referenziati più avanti nel file. Quando si importano queste librerie non vengono specificati un percorso o un'estensione, perché non sono file locali. Sono infatti elencate come dipendenze nel file `package.json`. Prestare attenzione a questa distinzione durante lo svolgimento della lezione.

Vengono quindi importati la funzione `App()` e `index.css`, che contiene gli stili globali applicati all'intera app.

Viene quindi chiamata la funzione `createRoot()`, che definisce il nodo radice dell'applicazione. Questa accetta come argomento l'elemento DOM all'interno del quale deve essere eseguito il rendering dell'app React. In questo caso, si tratta dell'elemento DOM con ID `root`. Infine, il metodo `render()` viene concatenato alla chiamata `createRoot()`, passandogli l'espressione JSX da renderizzare all'interno della radice. Scrivendo `<App />` come espressione JSX, si indica a React di chiamare la _funzione_ `App()`, che esegue il rendering del _componente_ `App` all'interno del nodo radice.

> [!NOTE]
> `<App />` viene renderizzato all'interno di uno speciale componente `<React.StrictMode>`. Questo componente aiuta gli sviluppatori a rilevare potenziali problemi nel codice.

Se necessario, è possibile approfondire queste API React:

- [`ReactDOM.createRoot()`](https://react.dev/reference/react-dom/client/createRoot)
- [`React.StrictMode`](https://react.dev/reference/react/StrictMode)

## Ripartire da zero

Prima di iniziare a creare l'app, verrà eliminato parte del codice boilerplate fornito da Vite.

Innanzitutto, come esperimento, modificare l'elemento [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements) in `App.jsx` affinché riporti "Hello, World!", quindi salvare il file. Si noterà che questa modifica viene immediatamente renderizzata nel server di sviluppo in esecuzione su `http://localhost:3000` nel browser. Tenerlo presente durante il lavoro sull'app.

Il resto del codice non verrà utilizzato. Sostituire il contenuto di `App.jsx` con quanto segue:

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

## Esercitarsi con JSX

Successivamente, verranno utilizzate le competenze JavaScript per acquisire maggiore dimestichezza con la scrittura di JSX e con l'uso dei dati in React. Verrà illustrato come aggiungere attributi agli elementi JSX, scrivere commenti, renderizzare contenuti da variabili e altre espressioni e passare dati ai componenti con le props.

### Aggiungere attributi agli elementi JSX

Gli elementi JSX possono avere attributi, proprio come gli elementi HTML. Provare ad aggiungere un `<button>` sotto l'elemento `<h1>` nel file `App.jsx`, in questo modo:

```jsx
<button type="button">Click me!</button>
```

Quando si salva il file, verrà visualizzato un pulsante con le parole `Click me!`. Il pulsante non fa ancora nulla, ma presto verrà illustrato come aggiungere interattività all'app.

Alcuni attributi sono diversi dalle rispettive controparti HTML. Ad esempio, l'attributo `class` in HTML diventa `className` in JSX. Questo perché `class` è una parola riservata in JavaScript e JSX è un'estensione di JavaScript. Per aggiungere una classe `primary` al pulsante, si scriverebbe quanto segue:

```jsx
<button type="button" className="primary">
  Click me!
</button>
```

### Espressioni JavaScript come contenuto

A differenza di HTML, JSX consente di scrivere variabili e altre espressioni JavaScript accanto agli altri contenuti. Dichiarare una variabile chiamata `subject` appena sopra la funzione `App()` nel file `App.jsx`:

```jsx
const subject = "React";
function App() {
  // code omitted for brevity
}
```

Successivamente, sostituire la parola "World" nell'elemento `<h1>` con `{subject}`:

```jsx
<h1>Hello, {subject}!</h1>
```

Salvare il file e controllare il browser. Dovrebbe essere visualizzato il rendering di "Hello, React!".

Le parentesi graffe attorno a `subject` sono un'altra funzionalità della sintassi JSX. Le parentesi graffe indicano a React di leggere il valore della variabile `subject`, anziché renderizzare la stringa letterale `"subject"`. All'interno delle parentesi graffe in JSX può essere inserita qualsiasi espressione JavaScript valida; React la valuterà e renderizzerà il _risultato_ dell'espressione come contenuto finale. Di seguito è riportata una serie di esempi, con commenti precedenti che spiegano cosa verrà renderizzato da ciascuna espressione:

```jsx-nolint
{/* Hello, React :)! */}
<h1>Hello, {`${subject} :)`}!</h1>
{/* Hello, REACT */}
<h1>Hello, {subject.toUpperCase()}</h1>
{/* Hello, 4! */}
<h1>Hello, {2 + 2}!</h1>
```

Anche i commenti in JSX vengono scritti all'interno delle parentesi graffe. Questo perché le parentesi graffe possono contenere una singola espressione JavaScript e i commenti sono validi come parte di un'espressione JavaScript, oltre a essere ignorati. All'interno delle parentesi graffe possono essere usate sia la sintassi `/* commento di blocco */` sia la sintassi `// commento di riga`, seguita da una nuova riga.

### Props dei componenti

Le **props** sono un mezzo per passare dati a un componente React. La loro sintassi è infatti identica a quella degli attributi: `prop="value"`. La differenza è che gli attributi vengono passati a elementi semplici, mentre le props vengono passate ai componenti React.

In React, il flusso dei dati è unidirezionale: le props possono essere passate solo dai componenti padre ai componenti figlio.

Aprire `main.jsx` e assegnare la prima prop al componente `<App />`.

Aggiungere una prop `subject` alla chiamata del componente `<App />`, con valore `Clarice`. Al termine, dovrebbe avere un aspetto simile al seguente:

```jsx
<App subject="Clarice" />
```

Tornare in `App.jsx` e riesaminare la funzione `App()`. Modificare la firma di `App()` affinché accetti `props` come parametro e registrare `props` nella console per poterlo esaminare. Eliminare anche la costante `subject`, poiché non serve più. Il file `App.jsx` dovrebbe avere questo aspetto:

```jsx
function App(props) {
  console.log(props);
  return (
    <>
      {
        // code omitted for brevity
      }
    </>
  );
}
```

Salvare il file e controllare il browser. Verrà visualizzato uno sfondo vuoto senza contenuto. Ciò accade perché si sta tentando di leggere una variabile `subject` che non è più definita. Correggere il problema commentando la riga `<h1>Hello {subject}!</h1>`.

> [!NOTE]
> Se l'editor di codice sa analizzare JSX, come accade per la maggior parte degli editor moderni, è possibile utilizzare la sua scorciatoia integrata per i commenti: `Ctrl + /` su Windows oppure `Cmd + /` su macOS, per creare commenti più rapidamente.

Salvare il file con quella riga commentata. Questa volta dovrebbe essere visualizzato soltanto il pulsante "Click me!". Se si apre la console per sviluppatori del browser, verrà visualizzato un messaggio simile a questo:

```plain
Object { subject: "Clarice" }
```

La proprietà dell'oggetto `subject` corrisponde alla prop `subject` aggiunta alla chiamata del componente `<App />`, mentre la stringa `Clarice` corrisponde al suo valore. Le props dei componenti in React vengono sempre raccolte in oggetti in questo modo.

Usare questa prop `subject` per correggere l'errore nell'app. Rimuovere il commento dalla riga `<h1>Hello, {subject}!</h1>` e modificarla in `<h1>Hello, {props.subject}!</h1>`, quindi eliminare l'istruzione `console.log()`. Il codice dovrebbe avere questo aspetto:

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

Al salvataggio, l'app dovrebbe ora salutare con "Hello, Clarice!". Se si torna in `main.jsx`, si modifica il valore di `subject` e si salva, il testo cambierà.

Per esercitarsi ulteriormente, si potrebbe provare ad aggiungere una prop aggiuntiva `greeting` alla chiamata del componente `<App />` in `main.jsx` e usarla insieme alla prop `subject` in `App.jsx`.

## Riepilogo

Si conclude qui la prima panoramica di React, inclusi l'installazione locale, la creazione di un'app iniziale e le basi del suo funzionamento. Nel prossimo articolo verrà iniziata la creazione della prima vera applicazione: una lista di attività. Prima di farlo, tuttavia, riepiloghiamo alcuni degli argomenti appresi.

In React:

- I componenti possono importare i moduli di cui hanno bisogno e devono esportare se stessi alla fine dei propri file.
- Le funzioni dei componenti hanno nomi in `PascalCase`.
- È possibile renderizzare espressioni JavaScript in JSX inserendole tra parentesi graffe, come `{so}`.
- Alcuni attributi JSX sono diversi dagli attributi HTML per evitare conflitti con le parole riservate di JavaScript. Ad esempio, `class` in HTML diventa `className` in JSX.
- Le props vengono scritte come attributi all'interno delle chiamate ai componenti e vengono passate ai componenti.

## Vedere anche

- [Imparare React](https://scrimba.com/learn-react-c0e?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Learn React_ di [Scrimba](https://scrimba.com/?via=mdn) è il corso React 101 definitivo: il punto di partenza perfetto per chiunque inizi con React. Imparare le basi di React moderno risolvendo oltre 140 sfide interattive di programmazione e creando otto progetti divertenti.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Main_features","Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning", "Learn_web_development/Core/Frameworks_libraries")}}
