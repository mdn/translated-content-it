---
title: Iniziare con React
short-title: Iniziare con React
slug: Learn_web_development/Core/Frameworks_libraries/React_getting_started
l10n:
  sourceCommit: e6d43da6c6d28a6ac92cdd47882809ffbdf987ce
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Main_features","Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning", "Learn_web_development/Core/Frameworks_libraries")}}

In questo articolo saluteremo React. Scopriremo alcuni dettagli del suo background e casi d'uso, imposteremo una toolchain di base per React sul nostro computer locale, e creeremo e giocheremo con un'app di partenza semplice — imparando un po' su come funziona React nel processo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi principali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, e il <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line">terminale/riga di comando</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
          Impostare un ambiente di sviluppo React locale, creare un'app di partenza e
          comprendere le basi del suo funzionamento.
      </td>
    </tr>
  </tbody>
</table>

## Ciao React

Come afferma il suo slogan ufficiale, [React](https://react.dev/) è una libreria per costruire interfacce utente. React non è un framework – non è neanche esclusivo del web. È usato con altre librerie per renderizzare in determinati ambienti. Ad esempio, [React Native](https://reactnative.dev/) può essere utilizzato per costruire applicazioni mobili.

Per costruire per il web, gli sviluppatori utilizzano React insieme a [ReactDOM](https://react.dev/reference/react-dom). React e ReactDOM sono spesso discussi negli stessi ambienti e utilizzati per risolvere gli stessi problemi di altri veri framework di sviluppo web. Quando ci riferiamo a React come "framework", stiamo lavorando con quell'intendimento colloquiale.

L'obiettivo principale di React è ridurre al minimo i bug che si verificano quando gli sviluppatori costruiscono interfacce utente. Lo fa attraverso l'uso dei componenti - pezzi di codice autonomi e logici che descrivono una parte dell'interfaccia utente. Questi componenti possono essere composti insieme per creare un'interfaccia utente completa, e React astrae gran parte del lavoro di rendering, lasciandoti concentrare sul design dell'interfaccia utente.

## Casi d'uso

A differenza degli altri framework trattati in questo modulo, React non impone regole rigide sui convenzioni di codice o organizzazione dei file. Questo permette ai team di impostare convenzioni che funzionano meglio per loro, e di adottare React in qualunque modo desiderino. React può gestire un singolo pulsante, alcuni pezzi di un'interfaccia o l'intera interfaccia utente di un'app.

Sebbene React _possa_ essere utilizzato per [piccoli pezzi di un'interfaccia](https://react.dev/learn/add-react-to-an-existing-project), non è così facile da "inserire" in un'applicazione come una libreria come jQuery, o anche un framework come Vue — è più approcciabile quando crei l'intera app con React.

Inoltre, molti dei benefici dell'esperienza dello sviluppatore di un'app React, come scrivere interfacce con JSX, richiedono un processo di compilazione. Aggiungere un compilatore come Babel a un sito web rende il codice su di esso lento, quindi gli sviluppatori spesso impostano tali strumenti con una fase di build. React ha un requisito di strumentazione pesante, ma può essere appreso.

Questo articolo si concentrerà sul caso d'uso di React per renderizzare l'intera interfaccia utente di un'applicazione con il supporto di [Vite](https://vite.dev/), un moderno strumento di build per il front-end.

## Come React utilizza JavaScript?

React utilizza le funzionalità del moderno JavaScript per molti dei suoi schemi. Il suo maggiore distanziamento da JavaScript si ha con l'uso della sintassi [JSX](https://react.dev/learn/writing-markup-with-jsx). JSX estende la sintassi di JavaScript in modo che il codice simile a HTML possa vivere accanto ad esso. Ad esempio:

```jsx
const heading = <h1>Mozilla Developer Network</h1>;
```

Questa costante heading è conosciuta come un'**espressione JSX**. React può usarla per renderizzare quel tag [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements) nella nostra app.

Supponiamo di voler avvolgere il nostro heading in un tag [`<header>`](/it/docs/Web/HTML/Reference/Elements/header), per motivi semantici? L'approccio JSX ci consente di annidare i nostri elementi l'uno dentro l'altro, proprio come facciamo con l'HTML:

```jsx
const header = (
  <header>
    <h1>Mozilla Developer Network</h1>
  </header>
);
```

> [!NOTE]
> Le parentesi nel frammento precedente non sono uniche per JSX e non hanno alcun effetto sulla tua applicazione. Sono un segnale per te (e per il tuo computer) che le varie righe di codice all'interno fanno parte della stessa espressione. Potresti altrettanto bene scrivere l'espressione header così:
>
> ```jsx-nolint
> const header = <header>
>   <h1>Mozilla Developer Network</h1>
> </header>;
> ```
>
> Tuttavia, questo sembra un po' scomodo, perché il tag [`<header>`](/it/docs/Web/HTML/Reference/Elements/header) che inizia l'espressione non è indentato nella stessa posizione del suo corrispondente tag di chiusura.

Ovviamente, il tuo browser non può leggere JSX senza aiuto. Quando compilato (usando uno strumento come [Babel](https://babeljs.io/) o [Parcel](https://parceljs.org/)), la nostra espressione header apparirebbe così:

```jsx
const header = React.createElement(
  "header",
  null,
  React.createElement("h1", null, "Mozilla Developer Network"),
);
```

È _possibile_ saltare la fase di compilazione e usare [`React.createElement()`](https://react.dev/reference/react/createElement) per scrivere la tua interfaccia utente. Facendo questo, tuttavia, perdi il vantaggio dichiarativo di JSX, e il tuo codice diventa più difficile da leggere. La compilazione è un ulteriore passo nel processo di sviluppo, ma molti sviluppatori nella comunità React pensano che la leggibilità di JSX ne valga la pena. Inoltre, lo sviluppo moderno del front-end coinvolge quasi sempre un processo di build — devi ridimensionare la sintassi moderna per essere compatibile con i browser più vecchi e potresti voler {{Glossary("Minification", "minimizzare")}} il tuo codice per ottimizzare le prestazioni di caricamento. Strumenti popolari come Babel vengono già forniti con il supporto JSX integrato, quindi non devi configurare la compilazione da solo a meno che non lo desideri.

Poiché JSX è un mix di HTML e JavaScript, alcuni sviluppatori lo trovano intuitivo. Altri dicono che la sua natura mista lo rende confuso. Una volta che ti sei abituato, tuttavia, ti permetterà di costruire interfacce utente in modo più veloce e intuitivo e permettere ad altri di comprendere meglio il tuo codice a prima vista.

Per saperne di più su JSX, dai un'occhiata all'articolo del team React [Scrivere Markup con JSX](https://react.dev/learn/writing-markup-with-jsx).

## Impostare la tua prima app React

Ci sono molti modi per creare una nuova applicazione React. Useremo Vite per creare una nuova applicazione tramite la riga di comando.

È possibile [aggiungere React a un progetto esistente](https://react.dev/learn/add-react-to-an-existing-project) copiando alcuni elementi [`<script>`](/it/docs/Web/HTML/Reference/Elements/script) in un file HTML, ma utilizzare Vite ti permetterà di passare più tempo a costruire la tua app e meno tempo a preoccuparti della configurazione.

> [!NOTE]
> Puoi iniziare a scrivere codice React senza fare _alcuna_ configurazione locale lavorando attraverso il [Primo Codice React](https://scrimba.com/learn-react-c0e/~03uo?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> di Scrimba.
> Sentiti libero di provarlo prima di continuare.

### Requisiti

Per utilizzare Vite, è necessario avere installato [Node.js](https://nodejs.org/en/). A partire da Vite 5.0, è richiesta almeno la versione 18 di Node e, quando possibile, è buona prassi utilizzare l'ultima versione Long Term Support (LTS). Al 24 ottobre 2023, Node 20 è l'ultima versione LTS. Node include npm (il gestore di pacchetti di Node).

Per verificare la tua versione di Node, esegui il seguente comando nel tuo terminale:

```bash
node -v
```

Se Node è installato, vedrai un numero di versione. Se non lo è, vedrai un messaggio di errore. Per installare Node, segui le istruzioni sul [sito web di Node.js](https://nodejs.org/en/).

Puoi usare il gestore di pacchetti Yarn come alternativa a npm, ma supporremo che stai utilizzando npm in questo set di tutorial. Vedi [Nozioni di base sulla gestione dei pacchetti](/it/docs/Learn_web_development/Extensions/Client-side_tools/Package_management) per ulteriori informazioni su npm e yarn.

Se stai utilizzando Windows, sarà necessario installare alcuni software per allinearti con il terminale Unix/macOS per poter utilizzare i comandi del terminale menzionati in questo tutorial. **Gitbash** (che fa parte del [toolset git per Windows](https://gitforwindows.org/)) o il **[Sottosistema Windows per Linux](https://learn.microsoft.com/en-us/windows/wsl/about)** (**WSL**) sono entrambi adatti. Vedi [Corso intensivo sulla riga di comando](/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line) per ulteriori informazioni su questi e sui comandi del terminale in generale.

Ricorda anche che React e ReactDOM producono app che funzionano solo su un set abbastanza moderno di browser come Firefox, Microsoft Edge, Safari o Chrome durante l'esecuzione di questi tutorial.

Vedi quanto segue per ulteriori informazioni:

- ["Informazioni su npm" sul blog di npm](https://docs.npmjs.com/about-npm/)
- ["Introduzione a npx" sul blog di npm](https://blog.npmjs.org/post/162869356040/introducing-npx-an-npm-package-runner)
- [Documentazione di Vite](https://vite.dev/guide/)

### Inizializzare la tua app

Il gestore di pacchetti npm viene fornito con un comando `create` che ti permette di creare nuovi progetti da modelli. Possiamo usarlo per creare una nuova app dal modello standard React di Vite. Assicurati di `cd` nel percorso in cui vuoi che la tua app risieda sul tuo computer, poi esegui il seguente comando nel tuo terminale:

```bash
npm create vite@latest moz-todo-react -- --template react
```

Questo crea una directory `moz-todo-react` utilizzando il modello `react` di Vite.

> [!NOTE]
> Il `--` è necessario per passare argomenti a comandi npm come `create`, e l'argomento `--template react` dice a Vite di usare il suo modello React.

Il tuo terminale avrà stampato alcuni messaggi se questo comando è stato eseguito con successo. Dovresti vedere testo che ti invita a `cd` nella tua nuova directory, installare le dipendenze dell'app e avviare l'app localmente. Cominciamo con due di questi comandi. Esegui quanto segue nel tuo terminale:

```bash
cd moz-todo-react && npm install
```

Una volta completato il processo, dobbiamo avviare un server di sviluppo locale per eseguire la nostra app. Qui, aggiungeremo alcuni flag alla riga di comando al suggerimento predefinito di Vite per aprire l'app nel nostro browser non appena il server viene avviato e utilizzare la porta 3000.

Esegui il seguente comando nel tuo terminale:

```bash
npm run dev -- --open --port 3000
```

Una volta avviato il server, dovresti vedere una nuova scheda del browser contenente la tua app React:

![Screenshot di Firefox macOS aperto su localhost:3000, che mostra un'applicazione creata dal modello React di Vite](default-vite.png)

### Struttura dell'applicazione

Vite ci fornisce tutto ciò di cui abbiamo bisogno per sviluppare un'applicazione React. La sua struttura iniziale dei file si presenta così:

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

**`index.html`** è il file di livello superiore più importante. Vite inietta il tuo codice in questo file affinché il browser possa eseguirlo. Non avrai bisogno di modificare questo file durante il nostro tutorial, ma dovresti cambiare il testo all'interno dell'elemento [`<title>`](/it/docs/Web/HTML/Reference/Elements/title) in questo file per riflettere il titolo della tua applicazione. Titoli di pagina accurati sono importanti per l'accessibilità.

La directory **`public`** contiene file statici che verranno serviti direttamente al tuo browser senza essere elaborati dagli strumenti di build di Vite. Al momento, contiene solo un logo Vite.

La directory **`src`** è dove passeremo la maggior parte del nostro tempo, in quanto è dove vive il codice sorgente della nostra applicazione. Noterai che alcuni file JavaScript in questa directory finiscono con l'estensione `.jsx`. Questa estensione è necessaria per ogni file che contiene JSX – dice a Vite di trasformare la sintassi JSX in JavaScript che il tuo browser può comprendere. La directory `src/assets` contiene il logo React che hai visto nel browser.

I file `package.json` e `package-lock.json` contengono metadati sul nostro progetto. Questi file non sono unici per le applicazioni React: Vite ha popolato `package.json` per noi, e npm ha creato `package-lock.json` quando abbiamo installato le dipendenze dell'app. Non è necessario comprendere questi file per completare questo tutorial. Tuttavia, se desideri saperne di più su di essi, puoi leggere di [`package.json`](https://docs.npmjs.com/cli/v9/configuring-npm/package-json/) e [`package-lock.json`](https://docs.npmjs.com/cli/v9/config-turing-npm/package-lock-json/) nella documentazione npm. Parliamo anche di `package.json` nel nostro tutorial [Nozioni fondamentali sulla gestione dei pacchetti](/it/docs/Learn_web_development/Extensions/Client-side_tools/Package_management).

### Personalizzazione del nostro script di sviluppo

Prima di andare avanti, potresti voler modificare un po' il tuo file `package.json` in modo da non dover passare i flag `--open` e `--port` ogni volta che esegui `npm run dev`. Apri `package.json` nel tuo editor di testo e trova l'oggetto `scripts`. Cambia la chiave `"dev"` in modo che appaia così:

```diff
- "dev": "vite",
+ "dev": "vite --open --port 3000",
```

Con questo in atto, la tua app verrà aperta nel tuo browser su `http://localhost:3000` ogni volta che esegui `npm run dev`.

> [!NOTE]
> Non hai bisogno dell'ulteriore `--` qui perché stiamo passando argomenti direttamente a `vite`, anziché a uno script npm predefinito.

## Esplorare il nostro primo componente React — `<App />`

In React, un **componente** è un modulo riutilizzabile che rende una parte della nostra applicazione complessiva. I componenti possono essere grandi o piccoli, ma di solito sono chiaramente definiti: servono a uno scopo singolo e ovvio.

Apriamo `src/App.jsx`, poiché il nostro browser ci sta invitando a modificarlo. Questo file contiene il nostro primo componente, `<App />`:

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

Il file `App.jsx` è costituito da tre parti principali: alcune dichiarazioni [`import`](/it/docs/Web/JavaScript/Reference/Statements/import) in cima, la funzione `App()` al centro e una dichiarazione [`export`](/it/docs/Web/JavaScript/Reference/Statements/export) in fondo. La maggior parte dei componenti React segue questo schema.

### Dichiarazioni import

Le dichiarazioni `import` in cima al file permettono a `App.jsx` di utilizzare codice che è stato definito altrove. Guardiamo più da vicino queste dichiarazioni.

```jsx
import { useState } from "react";
import viteLogo from "/vite.svg";
import reactLogo from "./assets/react.svg";
import "./App.css";
```

La prima dichiarazione importa il hook `useState` dalla libreria `react`. I hook sono un modo per utilizzare le funzionalità di React all'interno di un componente. Parleremo di più sui hook nel corso di questo tutorial.

Dopo di ciò, importiamo `reactLogo` e `viteLogo`. Nota che i loro percorsi di importazione iniziano con `./` e `/` rispettivamente e che terminano con l'estensione `.svg`. Questo ci dice che questi import sono _locali_, facendo riferimento ai nostri file piuttosto che ai pacchetti npm.

L'ultima dichiarazione importa il CSS relativo al nostro componente `<App />`. Nota che non c'è un nome di variabile e nessuna direttiva `from`. Questo è chiamato [import da effetti collaterali](/it/docs/Web/JavaScript/Reference/Statements/import#import_a_module_for_its_side_effects_only) — non importa alcun valore nel file JavaScript, ma dice a Vite di aggiungere il file CSS referenziato all'output finale del codice, in modo che possa essere utilizzato nel browser.

### La funzione `App()`

Dopo gli import, abbiamo una funzione chiamata `App()`, che definisce la struttura del componente `App`. Mentre la maggior parte della comunità JavaScript preferisce i nomi in {{Glossary("camel_case", "lower camel case")}} come `helloWorld`, i componenti React utilizzano nomi di variabili in Pascal case (o upper camel case), come `HelloWorld`, per rendere chiaro che un dato elemento JSX è un componente React e non un normale tag HTML. Se rinominassi la funzione `App()` in `app()`, il tuo browser genererebbe un errore.

Guardiamo più da vicino `App()`.

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

La funzione `App()` restituisce un'espressione JSX. Questa espressione definisce cosa il tuo browser renderizzerà infine nel DOM.

Subito sotto la parola chiave `return` c'è una speciale sintassi: `<>`. Questo è un [fragmento](https://react.dev/reference/react/Fragment). I componenti React devono restituire un singolo elemento JSX e i fragmenti ci permettono di farlo senza renderizzare tag `<div>` arbitrari nel browser. Vedrai fragmenti in molte applicazioni React.

### La dichiarazione export

C'è un'altra riga di codice dopo la funzione `App()`:

```jsx
export default App;
```

Questa dichiarazione di export rende la nostra funzione `App()` disponibile ad altri moduli. Ne parleremo di più in seguito.

## Spostarsi su `main`

Apriamo `src/main.jsx`, perché è dove il componente `<App />` viene utilizzato. Questo file è il punto di ingresso per la nostra app, e inizialmente appare così:

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

Come con `App.jsx`, il file inizia importando tutti i moduli JavaScript e altre risorse necessarie per funzionare.

Le prime due dichiarazioni importano `StrictMode` e `createRoot` dalle librerie `react` e `react-dom` perché vengono riferite più avanti nel file. Non scriviamo un percorso o un'estensione quando importiamo queste librerie perché non sono file locali. In effetti, sono elencate come dipendenze nel nostro file `package.json`. Fai attenzione a questa distinzione mentre lavori in questo argomento!

Importiamo quindi la nostra funzione `App()` e `index.css`, che contiene stili globali che vengono applicati a tutta la nostra app.

A questo punto chiamiamo la funzione `createRoot()`, che definisce il nodo radice della nostra applicazione. Questa prende come argomento l'elemento DOM all'interno del quale vogliamo che la nostra app React venga renderizzata. In questo caso, è l'elemento DOM con un ID di `root`. Infine, colleghiamo il metodo `render()` alla chiamata `createRoot()`, passandogli l'espressione JSX che vogliamo renderizzare all'interno della nostra radice. Scrivendo `<App />` come questa espressione JSX, stiamo dicendo a React di chiamare la funzione `App()`, che rende il componente `App` all'interno del nodo radice.

> **Nota:** `<App />` è renderizzato all'interno di uno speciale componente `<React.StrictMode>`. Questo componente aiuta gli sviluppatori a individuare potenziali problemi nel loro codice.

Puoi approfondire queste API di React, se lo desideri:

- [`ReactDOM.createRoot()`](https://react.dev/reference/react-dom/client/createRoot)
- [`React.StrictMode`](https://react.dev/reference/react/StrictMode)

## Partendo da zero

Prima di iniziare a costruire la nostra app, elimineremo un po' di codice boilerplate che Vite ci ha fornito.

Innanzitutto, come esperimento, modifica l'elemento [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements) in `App.jsx` in modo che legga "Hello, World!", poi salva il tuo file. Noterai che questa modifica viene immediatamente renderizzata nel server di sviluppo in esecuzione su `http://localhost:3000` nel tuo browser. Tieni presente questo mentre lavori alla tua app.

Non useremo il resto del codice! Sostituisci il contenuto di `App.jsx` con quanto segue:

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

## Praticare con JSX

Successivamente, useremo le nostre competenze JavaScript per abituarci a scrivere JSX e a lavorare con i dati in React. Parleremo di come aggiungere attributi agli elementi JSX, come scrivere commenti, come rendere il contenuto da variabili e altre espressioni, e come passare dati nei componenti con i props.

### Aggiungere attributi agli elementi JSX

Gli elementi JSX possono avere attributi, proprio come gli elementi HTML. Prova ad aggiungere un `<button>` sotto l'elemento `<h1>` nel tuo file `App.jsx`, in questo modo:

```jsx
<button type="button">Click me!</button>
```

Quando salvi il tuo file, vedrai un pulsante con le parole `Click me!`. Il pulsante non fa ancora nulla, ma presto impareremo come aggiungere interattività alla nostra app.

Alcuni attributi sono diversi dai loro corrispettivi HTML. Ad esempio, l'attributo `class` in HTML si traduce in `className` in JSX. Questo perché `class` è una parola riservata in JavaScript, e JSX è un'estensione di JavaScript. Se volessi aggiungere una classe `primary` al tuo pulsante, lo scriveresti così:

```jsx
<button type="button" className="primary">
  Click me!
</button>
```

### Espressioni JavaScript come contenuto

A differenza dell'HTML, JSX ci permette di scrivere variabili e altre espressioni JavaScript proprio accanto al nostro altro contenuto. Dichiariamo una variabile chiamata `subject` proprio sopra la funzione `App()` nel tuo file `App.jsx`:

```jsx
const subject = "React";
function App() {
  // code omitted for brevity
}
```

Successivamente, sostituisci la parola "World" nell'elemento `<h1>` con `{subject}`:

```jsx
<h1>Hello, {subject}!</h1>
```

Salva il tuo file e controlla il tuo browser. Dovresti vedere "Hello, React!" renderizzato.

Le parentesi graffe intorno a `subject` sono un'altra caratteristica della sintassi di JSX. Le parentesi graffe dicono a React che vogliamo leggere il valore della variabile `subject`, piuttosto che renderizzare la stringa letterale `"subject"`. Puoi inserire qualsiasi espressione JavaScript valida tra parentesi graffe in JSX; React la valuterà e renderizzerà il _risultato_ dell'espressione come contenuto finale. Di seguito è riportata una serie di esempi, con commenti sopra che spiegano cosa renderà ciascuna espressione:

```jsx-nolint
{/* Hello, React :)! */}
<h1>Hello, {subject + ' :)'}!</h1>
{/* Hello, REACT */}
<h1>Hello, {subject.toUpperCase()}</h1>
{/* Hello, 4! */}
<h1>Hello, {2 + 2}!</h1>
```

Persino i commenti in JSX sono scritti all'interno di parentesi graffe! Questo perché le parentesi graffe possono contenere una singola espressione JavaScript, e i commenti sono validi come parte di un'espressione JavaScript (e vengono ignorati). Puoi usare sia `/* sintassi del commento di blocco */` che `// sintassi del commento di linea` (con una nuova riga successiva) all'interno delle parentesi graffe.

### Component props

I **props** sono un mezzo per passare dati in un componente React. La loro sintassi è identica a quella degli attributi, in effetti: `prop="valore"`. La differenza è che mentre gli attributi sono passati a elementi semplici, i props sono passati a componenti React.

In React, il flusso di dati è unidirezionale: i props possono essere passati solo dai componenti genitori ai componenti figli.

Apriamo `main.jsx` e diamo al nostro componente `<App />` il suo primo prop.

Aggiungi un prop di `subject` alla chiamata del componente `<App />`, con un valore di `Clarice`. Quando hai finito, dovrebbe apparire qualcosa di simile a questo:

```jsx
<App subject="Clarice" />
```

Torna in `App.jsx`, rivediamo la funzione `App()`. Cambia la firma di `App()` in modo che accetti `props` come parametro e loggiamo `props` nella console in modo da poterlo ispezionare. Elimina anche il const `subject`; non ne abbiamo più bisogno. Il tuo file `App.jsx` dovrebbe apparire così:

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

Salva il tuo file e controlla il tuo browser. Vedrai uno sfondo vuoto senza contenuto. Questo perché stiamo cercando di leggere una variabile `subject` che non è più definita. Risolvilo commentando la riga `<h1>Hello {subject}!</h1>`.

> [!NOTE]
> Se il tuo editor di codice capisce come analizzare JSX (la maggior parte degli editor moderni lo fa!), puoi usare la sua scorciatoia per i commenti incorporati — `Ctrl + /` (su Windows) o `Cmd + /` (su macOS) — per creare commenti più velocemente.

Salva il file con quella riga commentata. Stavolta, dovresti vedere il tuo pulsante
"Click me!" renderizzato da solo. Se apri la console degli sviluppatori del tuo browser, vedrai un messaggio che appare così:

```plain
Object { subject: "Clarice" }
```

La proprietà oggetto `subject` corrisponde al prop `subject` che abbiamo aggiunto alla nostra chiamata del componente `<App />`, e la stringa `Clarice` corrisponde al suo valore. I props dei componenti in React sono sempre raccolti in oggetti in questo modo.

Usiamo questo prop `subject` per correggere l'errore nella nostra app. Decommenta la riga `<h1>Hello, {subject}!</h1>` e modificala in `<h1>Hello, {props.subject}!</h1>`, quindi elimina l'istruzione `console.log()`. Il tuo codice dovrebbe apparire così:

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

Per esercitarti ulteriormente, potresti provare ad aggiungere un prop aggiuntivo `greeting` alla chiamata del componente `<App />` dentro `main.jsx` e utilizzarlo insieme al prop `subject` dentro `App.jsx`.

## Riepilogo

Questo ci porta alla fine del nostro primo sguardo a React, incluso come installarlo localmente, creare un'app di partenza e come funzionano le basi. Nel prossimo articolo, inizieremo a costruire la nostra prima applicazione vera e propria — una lista delle cose da fare. Prima di farlo, tuttavia, ricapitoliamo alcune delle cose che abbiamo imparato.

In React:

- I componenti possono importare moduli di cui hanno bisogno e devono esportarsi in fondo ai loro file.
- Le funzioni dei componenti sono chiamate con `PascalCase`.
- Puoi renderizzare espressioni JavaScript in JSX mettendole tra parentesi graffe, come `{so}`.
- Alcuni attributi JSX sono diversi dagli attributi HTML in modo che non entrino in conflitto con le parole riservate JavaScript. Ad esempio, `class` in HTML si traduce in `className` in JSX.
- I props sono scritti proprio come gli attributi nelle chiamate dei componenti e vengono passati nei componenti.

## Vedi anche

- [Imparare React](https://scrimba.com/learn-react-c0e?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Imparare React_ di [Scrimba](https://scrimba.com/?via=mdn) è il React 101 definitivo — il punto di partenza perfetto per chiunque sia nuovo a React. Impara le basi di React moderno risolvendo oltre 140 sfide di coding interattive e costruendo otto progetti divertenti.

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Main_features","Learn_web_development/Core/Frameworks_libraries/React_todo_list_beginning", "Learn_web_development/Core/Frameworks_libraries")}}
