---
title: Iniziare con React
short-title: Iniziare con React
slug: Learn_web_development/Core/Frameworks_libraries/React_getting_started
l10n:
  sourceCommit: 611edf6335e4a833a6f394d0d98b117e7b0a36bf
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Main_features","Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning", "Learn_web_development/Core/Frameworks_libraries")}}

In questo articolo daremo il benvenuto a React. Scopriremo alcuni dettagli sul suo background e sui casi d'uso, configureremo una toolchain di base per React sul nostro computer locale, e creeremo e giocheremo con una semplice app iniziale, imparando un po' su come funziona React nel processo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi fondamentali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/command line</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
          Configurare un ambiente di sviluppo React locale, creare un'app di partenza, e
          comprendere le basi di come funziona.
      </td>
    </tr>
  </tbody>
</table>

## Ciao React

Come afferma il suo slogan ufficiale, [React](https://react.dev/) è una libreria per costruire interfacce utente. React non è un framework - non è nemmeno esclusivo del web. È usato con altre librerie per il rendering in determinati ambienti. Ad esempio, [React Native](https://reactnative.dev/) può essere usato per costruire applicazioni mobili.

Per costruire per il web, gli sviluppatori usano React in tandem con [ReactDOM](https://react.dev/reference/react-dom). React e ReactDOM sono spesso discussi negli stessi contesti - e utilizzati per risolvere gli stessi problemi - di altri veri framework di sviluppo web. Quando ci riferiamo a React come "framework", stiamo lavorando con quella comprensione colloquiale.

L'obiettivo principale di React è minimizzare i bug che si verificano quando gli sviluppatori costruiscono interfacce utente. Lo fa attraverso l'uso di componenti - pezzi di codice autoconclusivi e logici che descrivono una porzione dell'interfaccia utente. Questi componenti possono essere composti insieme per creare un'intera interfaccia utente, e React astrae gran parte del lavoro di rendering, lasciando che ci si concentri sul design dell'interfaccia utente.

## Casi d'uso

A differenza degli altri framework trattati in questo modulo, React non impone regole rigide riguardo le convenzioni di codice o l'organizzazione dei file. Questo permette ai team di stabilire le convenzioni che funzionano meglio per loro, e di adottare React nel modo che preferiscono. React può gestire un singolo pulsante, alcune parti di un'interfaccia, o l'intera interfaccia utente di un'app.

Mentre React _può_ essere usato per [piccoli pezzi di un'interfaccia](https://react.dev/learn/add-react-to-an-existing-project), non è semplice "inserirlo" in un'applicazione come una libreria tipo jQuery, o anche un framework come Vue - è più accessibile quando si costruisce l'intera app con React.

Inoltre, molti dei vantaggi in termini di esperienza sviluppatore di un'app React, come la scrittura di interfacce con JSX, richiedono un processo di compilazione. L'aggiunta di un compilatore come Babel a un sito web fa sì che il codice venga eseguito lentamente, quindi gli sviluppatori spesso configurano tali strumenti con un passaggio di build. React ha indubbiamente un requisito pesante di toolchain, ma può essere appreso.

Questo articolo si concentrerà sul caso d'uso di utilizzare React per rendere l'intera interfaccia utente di un'applicazione con il supporto di [Vite](https://vite.dev/), uno strumento di build moderno per il front-end.

## Come React utilizza JavaScript?

React utilizza le funzionalità del JavaScript moderno per molti dei suoi schemi. La sua maggiore differenza rispetto a JavaScript è rappresentata dall'uso della sintassi [JSX](https://react.dev/learn/writing-markup-with-jsx). JSX estende la sintassi di JavaScript in modo che il codice simile a HTML possa coesistere con esso. Ad esempio:

```jsx
const heading = <h1>Mozilla Developer Network</h1>;
```

Questa costante heading è conosciuta come un'**espressione JSX**. React può usarla per rendere quel tag [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements) nella nostra app.

Supponiamo di voler racchiudere il nostro heading in un tag [`<header>`](/it/docs/Web/HTML/Reference/Elements/header), per motivi semantici. L'approccio JSX ci permette di nidificare i nostri elementi l'uno dentro l'altro, proprio come facciamo con HTML:

```jsx
const header = (
  <header>
    <h1>Mozilla Developer Network</h1>
  </header>
);
```

> [!NOTE]
> Le parentesi nello snippet precedente non sono uniche per JSX e non hanno alcun effetto sulla tua applicazione. Sono un segnale per te (e per il tuo computer) che le più righe di codice all'interno fanno parte della stessa espressione. Potresti anche scrivere l'espressione header in questo modo:
>
> ```jsx-nolint
> const header = <header>
>   <h1>Mozilla Developer Network</h1>
> </header>;
> ```
>
> Tuttavia, questo sembra un po' strano, perché il tag [`<header>`](/it/docs/Web/HTML/Reference/Elements/header) che inizia l'espressione non è indentato nella stessa posizione del suo corrispondente tag di chiusura.

Ovviamente, il tuo browser non può leggere JSX senza aiuto. Quando compilato (usando uno strumento come [Babel](https://babeljs.io/) o [Parcel](https://parceljs.org/)), la nostra espressione header sembrerebbe così:

```jsx
const header = React.createElement(
  "header",
  null,
  React.createElement("h1", null, "Mozilla Developer Network"),
);
```

È _possibile_ saltare il passaggio di compilazione e usare [`React.createElement()`](https://react.dev/reference/react/createElement) per scrivere manualmente la tua interfaccia utente. Facendo questo, tuttavia, si perde il vantaggio dichiarativo di JSX e il tuo codice diventa più difficile da leggere. La compilazione è un passaggio extra nel processo di sviluppo, ma molti sviluppatori nella community di React pensano che la leggibilità di JSX ne valga la pena. Inoltre, lo sviluppo front-end moderno coinvolge quasi sempre un processo di build - devi ridurre le sintassi moderne per essere compatibile con i browser più vecchi e potresti voler {{Glossary("Minification", "minificare")}} il tuo codice per ottimizzare le performance di caricamento. Gli strumenti popolari come Babel già forniscono il supporto a JSX out-of-the-box, quindi non devi configurare la compilazione da solo a meno che tu non voglia.

Poiché JSX è una combinazione di HTML e JavaScript, alcuni sviluppatori lo trovano intuitivo. Altri dicono che la sua natura combinata lo rende confuso. Una volta che ci si sente a proprio agio con esso, però, consentirà di costruire interfacce utente più rapidamente e in modo intuitivo, e permetterà agli altri di comprendere meglio il tuo codice a colpo d'occhio.

Per leggere di più su JSX, dai un'occhiata all'articolo del team di React [Writing Markup with JSX](https://react.dev/learn/writing-markup-with-jsx).

## Configurazione della tua prima app React

Ci sono molti modi per creare una nuova applicazione React. Useremo Vite per creare una nuova applicazione tramite la riga di comando.

È possibile [aggiungere React a un progetto esistente](https://react.dev/learn/add-react-to-an-existing-project) copiando alcuni elementi [`<script>`](/it/docs/Web/HTML/Reference/Elements/script) in un file HTML, ma usare Vite ti permetterà di trascorrere più tempo a costruire la tua app e meno tempo a occuparti della configurazione.

### Requisiti

Per usare Vite, devi installare [Node.js](https://nodejs.org/en/). Dalla versione 5.0, Vite richiede almeno la versione 18 di Node, ed è una buona idea eseguire l'ultima versione a lungo termine (LTS) quando possibile. Al 24 ottobre 2023, la versione 20 di Node è l'ultima LTS. Node include npm (il gestore di pacchetti di Node).

Per controllare la tua versione di Node, esegui il seguente comando nel tuo terminale:

```bash
node -v
```

Se Node è installato, vedrai un numero di versione. Se non è installato, vedrai un messaggio di errore. Per installare Node, segui le istruzioni sul [sito web di Node.js](https://nodejs.org/en/).

Puoi usare il gestore di pacchetti Yarn come alternativa a npm, ma assumeremo che tu stia usando npm in questo set di tutorial. Vedi [Nozioni di base sulla gestione dei pacchetti](/it/docs/Learn_web_development/Extensions/Client-side_tools/Package_management) per ulteriori informazioni su npm e yarn.

Se stai usando Windows, dovrai installare del software per ottenere parità con il terminale Unix/macOS per usare i comandi del terminale menzionati in questo tutorial. **Gitbash** (che viene fornito come parte del [toolset git per Windows](https://gitforwindows.org/)) o **[Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/about)** (**WSL**) sono entrambi adatti. Vedi [Introduzione al command line](/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line) per ulteriori informazioni su questo argomento e sui comandi del terminale in generale.

Tieni anche presente che React e ReactDOM producono app che funzionano solo su un set abbastanza moderno di browser come Firefox, Microsoft Edge, Safari o Chrome quando si seguono questi tutorial.

Guarda i seguenti per maggiori informazioni:

- ["About npm" sul blog di npm](https://docs.npmjs.com/about-npm/)
- ["Introducing npx" sul blog di npm](https://blog.npmjs.org/post/162869356040/introducing-npx-an-npm-package-runner)
- [Documentazione di Vite](https://vite.dev/guide/)

### Inizializzare la tua app

Il gestore di pacchetti npm viene fornito con un comando `create` che ti consente di creare nuovi progetti da template. Possiamo usarlo per creare una nuova app dal template React standard di Vite. Assicurati di `cd` nel luogo in cui desideri che la tua app risieda sul tuo computer, quindi esegui il seguente comando nel tuo terminale:

```bash
npm create vite@latest moz-todo-react -- --template react
```

Questo crea una directory `moz-todo-react` usando il template `react` di Vite.

> [!NOTE]
> Il `--` è necessario per passare argomenti ai comandi npm come `create`, e l'argomento `--template react` dice a Vite di usare il suo template React.

Il tuo terminale avrà stampato alcuni messaggi se questo comando è stato eseguito correttamente. Dovresti vedere un testo che ti invita a `cd` nella tua nuova directory, installare le dipendenze dell'app, e eseguire l'app localmente. Iniziamo con due di quei comandi. Esegui i seguenti comandi nel tuo terminale:

```bash
cd moz-todo-react && npm install
```

Una volta completato il processo, dobbiamo eseguire un server di sviluppo locale per avviare la nostra app. Qui, aggiungeremo alcune flag della riga di comando alla proposta predefinita di Vite per aprire l'app nel nostro browser non appena il server viene avviato e usare la porta 3000.

Esegui il seguente comando nel tuo terminale:

```bash
npm run dev -- --open --port 3000
```

Una volta che il server è avviato, dovresti vedere una nuova scheda del browser contenente la tua app React:

![Screenshot di Firefox macOS aperto su localhost:3000, che mostra un'applicazione creata con il template React di Vite](default-vite.png)

### Struttura dell'applicazione

Vite ci fornisce tutto ciò di cui abbiamo bisogno per sviluppare un'applicazione React. La struttura iniziale dei file appare così:

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

**`index.html`** è il file di livello più alto più importante. Vite inietta il tuo codice in questo file in modo che il tuo browser possa eseguirlo. Non avrai bisogno di modificare questo file durante il nostro tutorial, ma dovresti cambiare il testo all'interno dell'elemento [`<title>`](/it/docs/Web/HTML/Reference/Elements/title) in questo file per riflettere il titolo della tua applicazione. Titoli di pagina accurati sono importanti per l'accessibilità.

La directory **`public`** contiene file statici che saranno serviti direttamente al tuo browser senza essere elaborati dagli strumenti di build di Vite. Al momento, contiene solo un logo Vite.

La directory **`src`** è dove passeremo la maggior parte del nostro tempo, poiché è dove risiede il codice sorgente della nostra applicazione. Noterai che alcuni file JavaScript in questa directory terminano con l'estensione `.jsx`. Questa estensione è necessaria per qualsiasi file che contenga JSX - informa Vite di trasformare la sintassi JSX in JavaScript che il tuo browser può comprendere. La directory `src/assets` contiene il logo React che hai visto nel browser.

I file `package.json` e `package-lock.json` contengono i metadati sul nostro progetto. Questi file non sono unici per le applicazioni React: Vite ha popolato `package.json` per noi, e npm ha creato `package-lock.json` quando abbiamo installato le dipendenze dell'app. Non è necessario capire questi file per completare questo tutorial. Tuttavia, se desideri saperne di più, puoi leggere su [`package.json`](https://docs.npmjs.com/cli/v9/configuring-npm/package-json/) e [`package-lock.json`](https://docs.npmjs.com/cli/v9/configuring-npm/package-lock-json/) nei documenti di npm. Parliamo anche di `package.json` nel nostro tutorial [Nozioni di base sulla gestione dei pacchetti](/it/docs/Learn_web_development/Extensions/Client-side_tools/Package_management).

### Personalizzare il nostro script di sviluppo

Prima di procedere, potresti voler modificare un po' il tuo file `package.json` in modo da non dover passare le flag `--open` e `--port` ogni volta che esegui `npm run dev`. Apri `package.json` nel tuo editor di testo e trova l'oggetto `scripts`. Modifica la chiave `"dev"` in modo che appaia così:

```diff
- "dev": "vite",
+ "dev": "vite --open --port 3000",
```

Con questo in atto, la tua app si aprirà nel tuo browser a `http://localhost:3000` ogni volta che esegui `npm run dev`.

> [!NOTE]
> Non hai bisogno dell'extra `--` qui perché stiamo passando argomenti direttamente a `vite`, piuttosto che a uno script npm predefinito.

## Esplorando il nostro primo componente React — `<App />`

In React, un **componente** è un modulo riutilizzabile che rende una parte della nostra applicazione complessiva. I componenti possono essere grandi o piccoli, ma di solito sono chiaramente definiti: servono un solo scopo evidente.

Apriamo `src/App.jsx`, dato che il nostro browser ci sta invitando a modificarlo. Questo file contiene il nostro primo componente, `<App />`:

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

Il file `App.jsx` è composto da tre parti principali: alcune dichiarazioni [`import`](/it/docs/Web/JavaScript/Reference/Statements/import) nella parte superiore, la funzione `App()` nel mezzo e una dichiarazione [`export`](/it/docs/Web/JavaScript/Reference/Statements/export) in fondo. La maggior parte dei componenti React segue questo schema.

### Dichiarazioni di importazione

Le dichiarazioni `import` nella parte superiore del file permettono a `App.jsx` di utilizzare codice definito altrove. Esaminiamo più da vicino queste dichiarazioni.

```jsx
import { useState } from "react";
import reactLogo from "./assets/react.svg";
import viteLogo from "/vite.svg";
import "./App.css";
```

La prima dichiarazione importa il hook `useState` dalla libreria `react`. Gli hook sono un modo per usare le funzionalità di React all'interno di un componente. Parleremo di questi hook più avanti in questo tutorial.

Dopo ciò, importiamo `reactLogo` e `viteLogo`. Nota che i loro percorsi di importazione iniziano con `./` e `/` rispettivamente e che finiscono con l'estensione `.svg`. Questo ci dice che queste importazioni sono _locali_, riferendosi ai nostri file piuttosto che ai pacchetti npm.

L'ultima dichiarazione importa il CSS relativo al nostro componente `<App />`. Nota che non c'è nome di variabile e non c'è direttiva `from`. Questo si chiama [_importazione con effetto collaterale_](/it/docs/Web/JavaScript/Reference/Statements/import#import_a_module_for_its_side_effects_only) — non importa alcun valore nel file JavaScript, ma dice a Vite di aggiungere il file CSS referenziato all'output finale del codice, in modo che possa essere utilizzato nel browser.

### La funzione `App()`

Dopo le importazioni, abbiamo una funzione chiamata `App()`, che definisce la struttura del componente App. Mentre la maggior parte della comunità JavaScript predilige nomi in {{Glossary("camel_case", "camelCase")}} come `helloWorld`, i componenti React utilizzano nomi variabili in PascalCase (o camel case superiore), come `HelloWorld`, per rendere chiaro che un dato elemento JSX è un componente React e non un normale tag HTML. Se rinominassi la funzione `App()` in `app()`, il tuo browser genererebbe un errore.

Esaminiamo più da vicino `App()`.

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

La funzione `App()` restituisce un'espressione JSX. Questa espressione definisce ciò che il tuo browser alla fine renderizza nel DOM.

Subito sotto la parola chiave `return` c'è una sintassi speciale: `<>`. Questo è un [fragment](https://react.dev/reference/react/Fragment). I componenti React devono restituire un unico elemento JSX, e i fragment ci permettono di farlo senza renderizzare `<div>` arbitrari nel browser. Vedrai i fragment in molte applicazioni React.

### La dichiarazione `export`

C'è ancora una riga di codice dopo la funzione `App()`:

```jsx
export default App;
```

Questa istruzione di esportazione rende disponibile la nostra funzione `App()` ad altri moduli. Ne parleremo più avanti.

## Passiamo a `main`

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

Come con `App.jsx`, il file inizia importando tutti i moduli JavaScript e gli altri asset necessari per l'esecuzione.

Le prime due dichiarazioni importano `StrictMode` e `createRoot` dalle librerie `react` e `react-dom` perché sono referenziate più avanti nel file. Non scriviamo un percorso o un'estensione quando importiamo queste librerie perché non sono file locali. In realtà, sono elencate come dipendenze nel nostro file `package.json`. Stai attento a questa distinzione mentre lavori attraverso questa lezione!

Poi importiamo la nostra funzione `App()` e `index.css`, che contiene stili globali applicati a tutta la nostra app.

Poi chiamiamo la funzione `createRoot()`, che definisce il nodo radice della nostra applicazione. Questo accetta come argomento l'elemento DOM all'interno del quale vogliamo che la nostra app React venga renderizzata. In questo caso, è l'elemento DOM con un ID di `root`. Infine, concatenamo il metodo `render()` alla chiamata `createRoot()`, passando come argomento l'espressione JSX che vogliamo rendere all'interno della radice. Scrivendo `<App />` come espressione JSX, stiamo dicendo a React di chiamare la _funzione_ `App()`, che renderizza il _componente_ App all'interno del nodo radice.

> **Nota:** `<App />` viene reso all'interno di uno speciale componente `<React.StrictMode>`. Questo componente aiuta gli sviluppatori a intercettare potenziali problemi nel loro codice.

Puoi leggere di più su queste API di React, se vuoi:

- [`ReactDOM.createRoot()`](https://react.dev/reference/react-dom/client/createRoot)
- [`React.StrictMode`](https://react.dev/reference/react/StrictMode)

## Partendo da zero

Prima di iniziare a costruire la nostra app, cancelleremo un po' del codice boilerplate che Vite ci ha fornito.

Per prima cosa, come esperimento, cambia l'elemento [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements) in `App.jsx` così che legga "Hello, World!", poi salva il tuo file. Noterai che questa modifica viene immediatamente resa nel server di sviluppo in esecuzione su `http://localhost:3000` nel tuo browser. Tieni presente questo mentre lavori sulla tua app.

Non useremo il resto del codice! Sostituisci i contenuti di `App.jsx` con il seguente:

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

Successivamente, utilizzeremo le nostre competenze JavaScript per diventare un po' più sicuri nella scrittura di JSX e nel lavorare con i dati in React. Parleremo di come aggiungere attributi a elementi JSX, come scrivere commenti, come rendere contenuti da variabili e altre espressioni, e come passare dati nei componenti con le props.

### Aggiungere attributi agli elementi JSX

Gli elementi JSX possono avere attributi, proprio come gli elementi HTML. Prova ad aggiungere un `<button>` sotto l'elemento `<h1>` nel tuo file `App.jsx`, in questo modo:

```jsx
<button type="button">Click me!</button>
```

Quando salvi il tuo file, vedrai un pulsante con le parole `Click me!`. Il pulsante non fa ancora nulla, ma impareremo ad aggiungere interattività alla nostra app presto.

Alcuni attributi sono diversi dai loro equivalenti HTML. Ad esempio, l'attributo `class` in HTML si traduce in `className` in JSX. Questo perché `class` è una parola riservata in JavaScript e JSX è un'estensione di JavaScript. Se volessi aggiungere una classe `primary` al tuo pulsante, lo scriveresti così:

```jsx
<button type="button" className="primary">
  Click me!
</button>
```

### Espressioni JavaScript come contenuto

Diversamente da HTML, JSX ci permette di scrivere variabili e altre espressioni JavaScript direttamente accanto agli altri contenuti. Dichiareremo una variabile chiamata `subject` appena sopra la funzione `App()` nel tuo file `App.jsx`:

```jsx
const subject = "React";
function App() {
  // code omitted for brevity
}
```

Poi, sostituisci la parola "World" nell'elemento `<h1>` con `{subject}`:

```jsx
<h1>Hello, {subject}!</h1>
```

Salva il tuo file e controlla il tuo browser. Dovresti vedere "Hello, React!" renderizzato.

Le parentesi graffe attorno a `subject` sono un'altra caratteristica della sintassi di JSX. Le parentesi graffe dicono a React che vogliamo leggere il valore della variabile `subject`, piuttosto che rendere la stringa letterale `"subject"`. Puoi mettere qualsiasi espressione valida JavaScript all'interno di parentesi graffe in JSX; React la valuterà e renderà il _risultato_ dell'espressione come contenuto finale. Di seguito è riportata una serie di esempi, con commenti sopra che spiegano cosa renderà ciascun'espressione:

```jsx-nolint
{/* Hello, React :)! */}
<h1>Hello, {subject + ' :)'}!</h1>
{/* Hello, REACT */}
<h1>Hello, {subject.toUpperCase()}</h1>
{/* Hello, 4! */}
<h1>Hello, {2 + 2}!</h1>
```

Anche i commenti in JSX sono scritti all'interno di parentesi graffe! Questo perché le parentesi graffe possono contenere una singola espressione JavaScript, e i commenti sono validi come parte di un'espressione JavaScript (e vengono ignorati). Puoi usare sia la sintassi del commento a blocchi `/* */` che quella del commento alla riga `//` (con una nuova riga a seguire) all'interno delle parentesi graffe.

### Props del componente

Le **Props** sono un mezzo per passare dati in un componente React. La loro sintassi è identica a quella degli attributi, infatti: `prop="value"`. La differenza è che mentre gli attributi vengono passati dentro elementi semplici, le props vengono passate dentro componenti React.

In React, il flusso dei dati è unidirezionale: le props possono essere passate solo dai componenti genitore verso quelli figli.

Apriamo `main.jsx` e forniamo al nostro componente `<App />` la sua prima prop.

Aggiungi una prop di `subject` alla chiamata del componente `<App />`, con un valore di `Clarice`. Quando avrai finito, dovrebbe assomigliare a questo:

```jsx
<App subject="Clarice" />
```

Tornando in `App.jsx`, esaminiamo di nuovo la funzione `App()`. Cambia la firma di `App()` in modo che accetti `props` come parametro e registra `props` nella console in modo da poterlo ispezionare. Elimina anche la costante `subject`; non ne abbiamo più bisogno. Il tuo file `App.jsx` dovrebbe apparire così:

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

Salva il tuo file e controlla il tuo browser. Vedrai uno sfondo vuoto senza contenuto. Questo perché stiamo cercando di leggere una variabile `subject` che non è più definita. Risolvi questo problema commentando la riga `<h1>Hello {subject}!</h1>`.

> [!NOTE]
> Se il tuo editor di codice riesce a capire come analizzare JSX (la maggior parte degli editor moderni lo fa!), puoi usare il suo scorciatoia incorporata per i commenti — `Ctrl + /` (su Windows) o `Cmd + /` (su macOS) — per creare commenti più rapidamente.

Salva il file con quella riga commentata. Questa volta, dovresti vedere il tuo pulsante "Click me!" renderizzato da solo. Se apri la console sviluppatore del tuo browser, vedrai un messaggio che appare così:

```plain
Object { subject: "Clarice" }
```

La proprietà dell'oggetto `subject` corrisponde alla prop `subject` che abbiamo aggiunto alla chiamata del componente `<App />`, e la stringa `Clarice` corrisponde al suo valore. Le props dei componenti in React vengono sempre raccolte in oggetti in questo modo.

Usiamo questa prop `subject` per correggere l'errore nella nostra app. Decommenta la riga `<h1>Hello, {subject}!</h1>` e cambiala in `<h1>Hello, {props.subject}!</h1>`, poi elimina l'istruzione `console.log()`. Il tuo codice dovrebbe apparire così:

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

Quando salvi, l'app dovrebbe ora salutarti con "Hello, Clarice!". Se torni in `main.jsx`, modifichi il valore di `subject`, e salvi, il tuo testo cambierà.

Per pratica aggiuntiva, potresti provare ad aggiungere una prop aggiuntiva `greeting` alla chiamata del componente `<App />` all'interno di `main.jsx` e usarla insieme alla prop `subject` all'interno di `App.jsx`.

## Sommario

Questo ci porta alla fine della nostra prima panoramica su React, compreso come installarlo localmente, creare un'app di partenza e come funzionano le basi. Nel prossimo articolo, inizieremo a costruire la nostra prima vera applicazione — una lista di cose da fare. Tuttavia, prima di farlo, ricapitoliamo alcune delle cose che abbiamo imparato.

In React:

- I componenti possono importare i moduli di cui hanno bisogno e devono esportarsi in fondo ai loro file.
- Le funzioni componenti sono denominate con la notazione `PascalCase`.
- Puoi rendere espressioni JavaScript in JSX mettendole tra parentesi graffe, come `{così}`.
- Alcuni attributi JSX sono diversi dagli attributi HTML in modo che non confliggano con le parole riservate di JavaScript. Ad esempio, `class` in HTML si traduce in `className` in JSX.
- Le props sono scritte proprio come attributi all'interno delle chiamate dei componenti e vengono passate nei componenti.

## Vedi anche

- [Impara React](https://scrimba.com/learn-react-c0e?via=mdn) <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Learn React_ di [Scrimba](https://scrimba.com/?via=mdn) è il corso React 101 definitivo — il punto di partenza perfetto per qualsiasi principiante di React. Impara le basi di React moderno risolvendo oltre 140 sfide di programmazione interattive e costruendo otto progetti divertenti.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Main_features","Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning", "Learn_web_development/Core/Frameworks_libraries")}}
