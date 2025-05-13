---
title: Caratteristiche principali dei framework
short-title: Caratteristiche dei framework
slug: Learn_web_development/Core/Frameworks_libraries/Main_features
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Introduction","Learn_web_development/Core/Frameworks_libraries/React_getting_started", "Learn_web_development/Core/Frameworks_libraries")}}

Ogni principale framework JavaScript adotta un approccio diverso per aggiornare il DOM, gestire gli eventi del browser e fornire un'esperienza di sviluppo piacevole. Questo articolo esplorerà le caratteristiche principali dei "big 4" framework, osservando come i framework tendono a funzionare a un livello elevato e le differenze tra di essi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>Comprendere le caratteristiche principali fornite dai framework JavaScript.</td>
    </tr>
  </tbody>
</table>

## Linguaggi specifici di dominio

La maggior parte dei framework consente di utilizzare linguaggi specifici di dominio (DSL) per costruire le applicazioni. In particolare, React ha reso popolare l'uso di **JSX** per scrivere i suoi componenti, mentre Ember utilizza **Handlebars**. A differenza di HTML, questi linguaggi sanno come leggere le variabili di dati e questi dati possono essere usati per semplificare il processo di scrittura della tua UI.

Le applicazioni Angular spesso fanno largo uso di **TypeScript**. TypeScript non è coinvolto nella scrittura delle interfacce utente, ma è un linguaggio specifico di dominio e presenta differenze significative rispetto a JavaScript standard.

I DSL non possono essere letti direttamente dal browser; devono prima essere trasformati in JavaScript o HTML. Gli strumenti dei framework generalmente includono gli strumenti necessari per gestire questo passaggio o possono essere adattati per includerlo. Sebbene sia possibile creare applicazioni con framework senza utilizzare questi linguaggi specifici di dominio, abbracciarli semplificherà il processo di sviluppo e renderà più facile trovare aiuto dalle comunità intorno a quei framework.

### JSX

[JSX](https://react.dev/learn/writing-markup-with-jsx), che sta per JavaScript e XML, è un'estensione di JavaScript che porta una sintassi simile a HTML in un ambiente JavaScript. È stato inventato dal team di React per l'uso in applicazioni React, ma può essere utilizzato per sviluppare altre applicazioni, come le app Vue, per esempio.

Ecco un semplice esempio di JSX:

```jsx
const subject = "World";
const header = (
  <header>
    <h1>Hello, {subject}!</h1>
  </header>
);
```

Questa espressione rappresenta un elemento HTML [`<header>`](/it/docs/Web/HTML/Reference/Elements/header) con un elemento [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements) al suo interno. Le parentesi graffe intorno a `{subject}` indicano all'applicazione di leggere il valore della costante `subject` e inserirlo nel nostro `<h1>`.

Quando utilizzato con React, il JSX del frammento precedente verrebbe compilato in questo:

```js
const subject = "World";
const header = React.createElement(
  "header",
  null,
  React.createElement("h1", null, "Hello, ", subject, "!"),
);
```

Quando infine viene reso dal browser, il frammento di codice sopra produrrà HTML che appare così:

```html
<header>
  <h1>Hello, World!</h1>
</header>
```

### Handlebars

Il linguaggio di template [Handlebars](https://handlebarsjs.com/) non è specifico per le applicazioni Ember, ma è ampiamente utilizzato nelle app Ember. Il codice Handlebars assomiglia a HTML, ma offre la possibilità di estrarre dati da altre fonti. Questi dati possono essere usati per influenzare l'HTML che un'applicazione costruisce alla fine.

Come JSX, Handlebars utilizza parentesi graffe per iniettare il valore di una variabile. Handlebars utilizza una doppia coppia di parentesi graffe, anziché una singola coppia.

Dato questo template Handlebars:

```html
<header>
  <h1>Hello, \{{subject}}!</h1>
</header>
```

E questi dati:

```json
{
  "subject": "World"
}
```

Handlebars costruirà HTML in questo modo:

```html
<header>
  <h1>Hello, World!</h1>
</header>
```

### TypeScript

[TypeScript](https://www.typescriptlang.org/) è un _superset_ di JavaScript, il che significa che estende JavaScript — tutto il codice JavaScript è valido TypeScript, ma non viceversa. TypeScript è utile per la rigorosità che consente agli sviluppatori di imporre nel loro codice. Ad esempio, consideri una funzione `add()`, che prende gli interi `a` e `b` e restituisce la loro somma.

In JavaScript, quella funzione potrebbe essere scritta così:

```js
function add(a, b) {
  return a + b;
}
```

Questo codice potrebbe essere banale per qualcuno abituato a JavaScript, ma potrebbe comunque essere più chiaro. JavaScript ci permette di usare l'operatore `+` per concatenare stringhe insieme, quindi questa funzione funzionerebbe tecnicamente ancora se `a` e `b` fossero stringhe — potrebbe semplicemente non dare il risultato che ci si aspetta. Cosa fare se volessimo permettere solo numeri da passare a questa funzione? TypeScript lo rende possibile:

```ts
function add(a: number, b: number) {
  return a + b;
}
```

Il `: number` scritto dopo ogni parametro qui dice a TypeScript che sia `a` che `b` devono essere numeri. Se usassimo questa funzione e passassimo `'2'` come argomento, TypeScript genererebbe un errore durante la compilazione e saremmo costretti a correggere il nostro errore. Potremmo scrivere il nostro JavaScript che solleva questi errori per noi, ma renderebbe il nostro codice sorgente significativamente più verboso. Probabilmente ha più senso lasciar gestire questi controlli a TypeScript.

## Scrivere componenti

Come menzionato nella lezione precedente, la maggior parte dei framework ha un certo tipo di modello di componenti. I componenti di React possono essere scritti con JSX, quelli di Ember con Handlebars, e i componenti di Angular e Vue con una sintassi di template che estende leggermente HTML.

Indipendentemente dalle loro opinioni su come dovrebbero essere scritti i componenti, i componenti di ciascun framework offrono un modo per descrivere le proprietà esterne di cui potrebbero avere bisogno, lo stato interno che il componente dovrebbe gestire e gli eventi che un utente può attivare sul markup del componente.

I frammenti di codice nel resto di questa sezione useranno React come esempio e sono scritti con JSX.

### Proprietà

Le proprietà, o **props**, sono dati esterni che un componente necessita per essere renderizzato. Supponga di costruire un sito web per una rivista online, e di dover garantire che ogni scrittore riceva credito per il proprio lavoro. Potrebbe creare un componente `AuthorCredit` da associare a ciascun articolo. Questo componente deve visualizzare un ritratto dell'autore e una breve biografia su di esso. Per sapere quale immagine rendere e quale biografia stampare, `AuthorCredit` deve accettare alcune props.

Una rappresentazione React di questo componente `AuthorCredit` potrebbe apparire così:

```jsx
function AuthorCredit(props) {
  return (
    <figure>
      <img src={props.src} alt={props.alt} />
      <figcaption>{props.byline}</figcaption>
    </figure>
  );
}
```

`{props.src}`, `{props.alt}`, e `{props.byline}` rappresentano i punti in cui le props verranno inserite nel componente. Per rendere questo componente, scriveremmo del codice come questo nel punto in cui vogliamo che venga renderizzato (che probabilmente sarà all'interno di un altro componente):

```jsx
<AuthorCredit
  src="./assets/zelda.png"
  alt="Portrait of Zelda Schiff"
  byline="Zelda Schiff is editor-in-chief of the Library Times."
/>
```

Ciò alla fine renderà il seguente elemento [`<figure>`](/it/docs/Web/HTML/Reference/Elements/figure) nel browser, con la sua struttura definita nel componente `AuthorCredit` e il suo contenuto definito nelle props incluse nell'invocazione del componente `AuthorCredit`:

```html
<figure>
  <img src="assets/zelda.png" alt="Portrait of Zelda Schiff" />
  <figcaption>Zelda Schiff is editor-in-chief of the Library Times.</figcaption>
</figure>
```

### Stato

Abbiamo parlato del concetto di **stato** nel capitolo precedente — un meccanismo robusto di gestione dello stato è fondamentale per un framework efficace, e ogni componente può avere dati di cui deve controllare lo stato. Questo stato persisterà in qualche modo finché il componente verrà utilizzato. Come le props, lo stato può essere utilizzato per influenzare il modo in cui un componente viene renderizzato.

Come esempio, consideri un pulsante che conta quante volte è stato cliccato. Questo componente dovrebbe essere responsabile per tracciare il proprio stato _count_, e potrebbe essere scritto così:

```jsx
function CounterButton() {
  const [count] = useState(0);
  return <button>Clicked {count} times</button>;
}
```

[`useState()`](https://react.dev/reference/react/useState) è un **[hook di React](https://react.dev/reference/react)** che, dato un valore iniziale dei dati, terrà traccia di quel valore mentre viene aggiornato. Il codice verrà inizialmente renderizzato così nel browser:

```html
<button>Clicked 0 times</button>
```

La chiamata a `useState()` tiene traccia del valore `count` in modo robusto nell'app, senza che lei debba scrivere codice per farlo da sola.

### Eventi

Per essere interattivi, i componenti hanno bisogno di modi per rispondere agli eventi del browser, affinché le nostre applicazioni possano rispondere ai nostri utenti. I framework forniscono ciascuno la propria sintassi per ascoltare gli eventi del browser, che fanno riferimento ai nomi degli eventi nativi equivalenti del browser.

In React, ascoltare l'evento [`click`](/it/docs/Web/API/Element/click_event) richiede una proprietà speciale, `onClick`. Aggiorniamo il nostro codice `CounterButton` sopra per consentirgli di contare i clic:

```jsx
function CounterButton() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(count + 1)}>Clicked {count} times</button>
  );
}
```

In questa versione stiamo usando funzionalità aggiuntive di `useState()` per creare una funzione speciale `setCount()`, che possiamo invocare per aggiornare il valore di `count`. Chiamiamo questa funzione all'interno del gestore eventi `onClick` per impostare `count` al suo valore corrente, più uno.

## Stile dei componenti

Ogni framework offre un modo per definire gli stili per i vostri componenti — o per l'intera applicazione. Sebbene l'approccio di ciascun framework alla definizione degli stili di un componente sia leggermente diverso, tutti vi offrono diversi modi per farlo. Con l'aggiunta di alcuni moduli di supporto, è possibile stilare le app del framework in [Sass](https://sass-lang.com/) o [Less](https://lesscss.org/), o transpileare i fogli di stile CSS con [PostCSS](https://postcss.org/).

## Gestione delle dipendenze

Tutti i principali framework forniscono meccanismi per gestire le dipendenze — utilizzando componenti all'interno di altri componenti, a volte con più livelli di gerarchia. Come per altre caratteristiche, il meccanismo esatto varierà tra i framework, ma il risultato finale è lo stesso. I componenti tendono a importare componenti in altri componenti utilizzando la sintassi standard dei [moduli JavaScript](/it/docs/Web/JavaScript/Guide/Modules), o almeno qualcosa di simile.

### Componenti nei componenti

Uno dei principali vantaggi dell'architettura UI basata sui componenti è che i componenti possono essere composti insieme. Proprio come è possibile scrivere tag HTML uno dentro l'altro per costruire un sito web, è possibile utilizzare componenti all'interno di altri componenti per costruire un'applicazione web. Ogni framework consente di scrivere componenti che utilizzano (e quindi dipendono da) altri componenti.

Per esempio, il nostro componente `AuthorCredit` React potrebbe essere utilizzato all'interno di un componente `Article`. Ciò significa che `Article` dovrebbe importare `AuthorCredit`.

```js
import AuthorCredit from "./components/AuthorCredit";
```

Una volta fatto ciò, `AuthorCredit` potrebbe essere utilizzato all'interno del componente `Article` in questo modo:

```jsx
<Article>
  <AuthorCredit />
</Article>
```

### Iniezione di dipendenze

Le applicazioni del mondo reale possono spesso coinvolgere strutture di componenti con più livelli di annidamento. Un componente `AuthorCredit` annidato a molti livelli di profondità potrebbe, per qualche ragione, aver bisogno di dati dal livello molto radice della nostra applicazione.

Supponga che il sito della rivista che stiamo costruendo sia strutturato in questo modo:

```jsx
<App>
  <Home>
    <Article>
      <AuthorCredit {/* props */} />
    </Article>
  </Home>
</App>
```

Il nostro componente `App` ha dati di cui il componente `AuthorCredit` ha bisogno. Potremmo riscrivere `Home` e `Article` in modo che sappiano come passare le props in basso, ma potrebbe risultare tedioso se ci sono molti, molti livelli tra l'origine e la destinazione dei nostri dati. È anche eccessivo: `Home` e `Article` non utilizzano effettivamente il ritratto o la biografia dell'autore, ma se vogliamo ottenere quelle informazioni nel `AuthorCredit`, dovremo cambiare `Home` e `Article` per accoglierle.

Il problema del passaggio dei dati attraverso molti livelli di componenti viene chiamato "prop drilling", e non è ottimale per applicazioni di grandi dimensioni.

Per aggirare il prop drilling, i framework forniscono una funzionalità nota come iniezione di dipendenze, che è un modo per ottenere determinati dati direttamente ai componenti che ne hanno bisogno, senza passare attraverso livelli intermedi. Ogni framework implementa l'iniezione di dipendenze sotto un nome diverso, e in modo diverso, ma l'effetto è infine lo stesso.

Angular chiama questo processo [iniezione di dipendenze](https://angular.dev/guide/di/dependency-injection); Vue ha i metodi [`provide()` e `inject()` dei componenti](https://v2.vuejs.org/v2/api/#provide-inject); React ha un [Context API](https://react.dev/learn/passing-data-deeply-with-context); Ember condivide lo stato tramite [servizi](https://guides.emberjs.com/release/services/).

### Ciclo di vita

Nel contesto di un framework, il **ciclo di vita** di un componente è una raccolta di fasi che un componente attraversa dal momento in cui viene aggiunto al DOM e poi visualizzato dal browser (spesso chiamato _mounting_) al momento in cui viene rimosso dal DOM (spesso chiamato _unmounting_). Ogni framework nomina queste fasi del ciclo di vita in modi diversi, e non tutti danno accesso agli sviluppatori alle stesse fasi. Tutti i framework seguono lo stesso modello generale: consentono agli sviluppatori di eseguire determinate azioni quando il componente _monta_, quando _rende_, quando si _smonte_, e in molte fasi intermedie.

La fase _render_ è la più cruciale da comprendere, perché viene ripetuta il maggior numero di volte mentre l'utente interagisce con l'applicazione. Viene eseguita ogni volta che il browser deve renderizzare qualcosa di nuovo, sia che quelle nuove informazioni siano un'aggiunta a ciò che è nel browser, una cancellazione, o una modifica di ciò che è lì.

Questo [diagramma del ciclo di vita di un componente React](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/) offre una panoramica generale del concetto.

## Renderizzazione degli elementi

Come per i cicli di vita, i framework adottano approcci diversi ma simili su come renderizzare le applicazioni. Tutti tengono traccia della versione attualmente renderizzata del DOM del browser, e ciascuno prende decisioni leggermente diverse su come il DOM dovrebbe cambiare man mano che i componenti della tua applicazione vengono renderizzati di nuovo. Poiché i framework prendono queste decisioni per lei, di solito non interagisce direttamente con il DOM. Questa astrazione dal DOM è più complessa e più intensiva di memoria rispetto all'aggiornamento del DOM stesso, ma senza di essa, i framework non potrebbero permetterle di programmare nel modo dichiarativo per cui sono noti.

Il **Virtual DOM** è un approccio secondo cui le informazioni sul DOM del tuo browser sono memorizzate nella memoria JavaScript. La tua applicazione aggiorna questa copia del DOM, quindi la confronta con il "DOM reale" — il DOM che è effettivamente renderizzato per gli utenti — per decidere cosa renderizzare. L'applicazione costruisce una "diff" per confrontare le differenze tra il virtual DOM aggiornato e il DOM attualmente renderizzato, e utilizza quella diff per applicare aggiornamenti al DOM reale. Sia React che Vue utilizzano un modello di virtual DOM, ma non applicano la stessa logica esatta durante il differenziamento o il rendering.

Può [leggere di più sul Virtual DOM nei documenti React](https://legacy.reactjs.org/docs/faq-internals.html#what-is-the-virtual-dom).

L'**Incremental DOM** è simile al virtual DOM in quanto costruisce un diff DOM per decidere cosa rendere, ma differisce in quanto non crea una copia completa del DOM nella memoria JavaScript. Ignora le parti del DOM che non necessitano di essere cambiate. Angular è l'unico framework discusso finora in questo modulo che utilizza un incremental DOM.

Può [leggere di più sull'Incremental DOM sul blog di Auth0](https://auth0.com/blog/incremental-dom/).

Il **Glimmer VM** è unico per Ember. Non è un virtual DOM né un incremental DOM; è un processo separato attraverso il quale i template di Ember vengono transpileati in una sorta di "byte code" che è più facile e veloce da leggere rispetto a JavaScript.

## Routing

Come [menzionato nel capitolo precedente, il routing](/it/docs/Learn_web_development/Core/Frameworks_libraries/Introduction#routing) è una parte importante dell'esperienza web. Per evitare un'esperienza rotta in app sufficientemente complesse con molte visualizzazioni, ciascuno dei framework coperti in questo modulo fornisce una libreria (o più di una libreria) che aiuta gli sviluppatori a implementare il routing lato client nelle loro applicazioni.

## Testing

Tutte le applicazioni beneficiano di una copertura di test che assicura che il software continui a comportarsi nel modo previsto, e le applicazioni web non fanno eccezione. L'ecosistema di ciascun framework fornisce strumenti che facilitano la scrittura di test. Gli strumenti di testing non sono incorporati nei framework stessi, ma gli strumenti della riga di comando usati per generare le app framework offrono accesso agli strumenti di testing appropriati.

Ogni framework ha strumenti estesi nel suo ecosistema, con capacità sia per test unitari che per test di integrazione.

[Testing Library](https://testing-library.com/) è una suite di utility per i test che ha strumenti per molti ambienti JavaScript, inclusi React, Vue e Angular. I documenti di Ember trattano il [testing delle app Ember](https://guides.emberjs.com/release/testing/).

Ecco un rapido test per il nostro `CounterButton` scritto con l'aiuto di React Testing Library — testa una serie di cose, come l'esistenza del pulsante e se il pulsante visualizza il testo corretto dopo essere stato cliccato 0, 1 e 2 volte:

```jsx
import { fireEvent, render, screen } from "@testing-library/react";

import CounterButton from "./CounterButton";

it("Renders a semantic button with an initial state of 0", () => {
  render(<CounterButton />);
  const btn = screen.getByRole("button");

  expect(btn).toBeInTheDocument();
  expect(btn).toHaveTextContent("Clicked 0 times");
});

it("Increments the count when clicked", () => {
  render(<CounterButton />);
  const btn = screen.getByRole("button");

  fireEvent.click(btn);
  expect(btn).toHaveTextContent("Clicked 1 times");

  fireEvent.click(btn);
  expect(btn).toHaveTextContent("Clicked 2 times");
});
```

## Riepilogo

A questo punto dovrebbe avere un'idea più chiara dei linguaggi, delle caratteristiche e degli strumenti che utilizzerà mentre crea applicazioni con i framework. Sono sicuro che è entusiasta di iniziare e di fare del vero coding, ed è ciò che farete prossimamente!

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Introduction","Learn_web_development/Core/Frameworks_libraries/React_getting_started", "Learn_web_development/Core/Frameworks_libraries")}}
