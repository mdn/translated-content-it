---
title: Caratteristiche principali dei framework
short-title: Caratteristiche del framework
slug: Learn_web_development/Core/Frameworks_libraries/Main_features
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Introduction","Learn_web_development/Core/Frameworks_libraries/React_getting_started", "Learn_web_development/Core/Frameworks_libraries")}}

Ogni principale framework JavaScript adotta un diverso approccio per aggiornare il DOM, gestire gli eventi del browser e fornire un'esperienza piacevole per lo sviluppatore. Questo articolo esplorerà le caratteristiche principali dei "big 4" framework, osservando come i framework tendano a lavorare da un punto di vista generale e le differenze tra di essi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi base come <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>Comprendere le caratteristiche principali fornite dai framework JavaScript.</td>
    </tr>
  </tbody>
</table>

## Linguaggi specifici del dominio

La maggior parte dei framework ti permette di usare linguaggi specifici del dominio (DSL) per costruire le tue applicazioni. In particolare, React ha reso popolare l'uso di **JSX** per scrivere i suoi componenti, mentre Ember utilizza **Handlebars**. A differenza di HTML, questi linguaggi sanno come leggere le variabili di dati, e questi dati possono essere utilizzati per semplificare il processo di scrittura della tua interfaccia utente.

Le applicazioni Angular spesso fanno largo uso di **TypeScript**. TypeScript non è orientato alla scrittura delle interfacce utente, ma è un linguaggio specifico del dominio, e presenta differenze significative rispetto al JavaScript standard.

I DSL non possono essere letti direttamente dal browser; devono prima essere trasformati in JavaScript o HTML. Gli strumenti dei framework generalmente includono gli strumenti necessari per gestire questo passaggio o possono essere adattati per includere questo passaggio. Anche se è possibile costruire applicazioni con framework senza utilizzare questi linguaggi specifici del dominio, adottarli semplificherà il tuo processo di sviluppo e renderà più facile trovare aiuto dalle comunità attorno a quei framework.

### JSX

[JSX](https://react.dev/learn/writing-markup-with-jsx), che sta per JavaScript e XML, è un'estensione di JavaScript che porta una sintassi simile a HTML in un ambiente JavaScript. È stato inventato dal team di React per l'uso nelle applicazioni React, ma può essere utilizzato per sviluppare altre applicazioni, come ad esempio le app Vue.

Di seguito viene mostrato un semplice esempio di JSX:

```jsx
const subject = "World";
const header = (
  <header>
    <h1>Hello, {subject}!</h1>
  </header>
);
```

Questa espressione rappresenta un elemento HTML [`<header>`](/it/docs/Web/HTML/Reference/Elements/header) con un elemento [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements) al suo interno. Le parentesi graffe attorno a `{subject}` dicono all'applicazione di leggere il valore della costante `subject` e inserirla all'interno del nostro `<h1>`.

Quando utilizzato con React, il JSX dallo snippet precedente verrebbe compilato in questo modo:

```js
const subject = "World";
const header = React.createElement(
  "header",
  null,
  React.createElement("h1", null, "Hello, ", subject, "!"),
);
```

Quando infine viene renderizzato dal browser, lo snippet sopra produrrà un HTML che appare così:

```html
<header>
  <h1>Hello, World!</h1>
</header>
```

### Handlebars

Il linguaggio di templating [Handlebars](https://handlebarsjs.com/) non è specifico delle applicazioni Ember, ma è ampiamente utilizzato nelle applicazioni Ember. Il codice Handlebars assomiglia a HTML, ma ha la possibilità di estrarre dati da altre fonti. Questi dati possono essere utilizzati per influenzare l'HTML che un'applicazione costruisce infine.

Come JSX, Handlebars utilizza le parentesi graffe per iniettare il valore di una variabile. Handlebars utilizza una doppia coppia di parentesi graffe, anziché una singola.

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

Handlebars costruirà HTML come questo:

```html
<header>
  <h1>Hello, World!</h1>
</header>
```

### TypeScript

[TypeScript](https://www.typescriptlang.org/) è un _superset_ di JavaScript, il che significa che estende JavaScript — tutto il codice JavaScript è codice TypeScript valido, ma non viceversa. TypeScript è utile per il rigore che permette agli sviluppatori di imporre sul proprio codice. Per esempio, considera una funzione `add()`, che prende gli interi `a` e `b` e restituisce la loro somma.

In JavaScript, quella funzione potrebbe essere scritta in questo modo:

```js
function add(a, b) {
  return a + b;
}
```

Questo codice potrebbe sembrare banale per qualcuno abituato a JavaScript, ma potrebbe comunque essere più chiaro. JavaScript ci permette di usare l'operatore `+` per concatenare stringhe insieme, quindi questa funzione funzionerebbe tecnicamente ancora se `a` e `b` fossero stringhe — potrebbe semplicemente non darti il risultato che ti aspetti. E se volessimo consentire solo numeri come parametri di questa funzione? TypeScript lo rende possibile:

```ts
function add(a: number, b: number) {
  return a + b;
}
```

Il `: number` scritto dopo ogni parametro qui dice a TypeScript che sia `a` che `b` devono essere numeri. Se usassimo questa funzione e ci passassimo come argomento `'2'`, TypeScript solleverebbe un errore durante la compilazione e saremmo costretti a correggere il nostro errore. Potremmo scrivere il nostro JavaScript che solleva questi errori per noi, ma renderebbe il nostro codice sorgente significativamente più verboso. Probabilmente ha più senso lasciare che TypeScript si occupi di questi controlli per noi.

## Scrivere componenti

Come menzionato nella lezione precedente, la maggior parte dei framework ha un qualche tipo di modello di componente. I componenti React possono essere scritti con JSX, i componenti Ember con Handlebars, e i componenti Angular e Vue con una sintassi di templating che estende leggermente HTML.

Indipendentemente dalle loro opinioni su come dovrebbero essere scritti i componenti, i componenti di ciascun framework offrono un modo per descrivere le proprietà esterne di cui potrebbero aver bisogno, lo stato interno che il componente dovrebbe gestire e gli eventi che un utente può attivare sul markup del componente.

Gli snippet di codice nel resto di questa sezione useranno React come esempio e sono scritti con JSX.

### Proprietà

Le proprietà, o **props**, sono dati esterni di cui un componente ha bisogno per essere renderizzato. Supponiamo che stiate costruendo un sito web per una rivista online e che abbiate bisogno di assicurarsi che ciascun autore riceva il merito per il proprio lavoro. Potreste creare un componente `AuthorCredit` accanto ad ogni articolo. Questo componente ha bisogno di visualizzare un ritratto dell'autore e una breve biografia su di loro. Per sapere quale immagine rendere e quale biografia stampare, `AuthorCredit` deve accettare alcuni props.

Una rappresentazione React di questo componente `AuthorCredit` potrebbe apparire in questo modo:

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

`{props.src}`, `{props.alt}`, e `{props.byline}` rappresentano i punti in cui i nostri props verranno inseriti nel componente. Per rendere questo componente, scriveremmo un codice come questo nel punto in cui vogliamo venga renderizzato (che probabilmente sarà all'interno di un altro componente):

```jsx
<AuthorCredit
  src="./assets/zelda.png"
  alt="Portrait of Zelda Schiff"
  byline="Zelda Schiff is editor-in-chief of the Library Times."
/>
```

Questo infine renderizzerà il seguente elemento [`<figure>`](/it/docs/Web/HTML/Reference/Elements/figure) nel browser, con la sua struttura come definita nel componente `AuthorCredit` e il suo contenuto come definito nei props inclusi nella chiamata del componente `AuthorCredit`:

```html
<figure>
  <img src="assets/zelda.png" alt="Portrait of Zelda Schiff" />
  <figcaption>Zelda Schiff is editor-in-chief of the Library Times.</figcaption>
</figure>
```

### Stato

Abbiamo parlato del concetto di **stato** nel capitolo precedente — un robusto meccanismo di gestione dello stato è fondamentale per un framework efficace, e ciascun componente può avere dati che necessitano di essere controllati nello stato. Questo stato persiste in qualche modo finché il componente è in uso. Come i props, lo stato può essere utilizzato per influenzare il modo in cui un componente viene renderizzato.

Ad esempio, considera un pulsante che conta quante volte è stato cliccato. Questo componente dovrebbe essere responsabile del tracciamento del proprio stato di _contatore_ e potrebbe essere scritto in questo modo:

```jsx
function CounterButton() {
  const [count] = useState(0);
  return <button>Clicked {count} times</button>;
}
```

[`useState()`](https://react.dev/reference/react/useState) è un **[React hook](https://react.dev/reference/react)** che, dato un valore iniziale dei dati, terrà traccia di quel valore mentre viene aggiornato. Il codice verrà inizialmente renderizzato nel browser in questo modo:

```html
<button>Clicked 0 times</button>
```

La chiamata `useState()` tiene traccia del valore `count` in modo robusto attraverso l'app, senza necessità di scrivere codice per farlo da soli.

### Eventi

Per essere interattivi, i componenti hanno bisogno di modi per rispondere agli eventi del browser, affinché le nostre applicazioni possano rispondere ai nostri utenti. I diversi framework forniscono ciascuno la propria sintassi per ascoltare gli eventi del browser, che fa riferimento ai nomi degli eventi nativi equivalenti.

In React, ascoltare l'evento [`click`](/it/docs/Web/API/Element/click_event) richiede una proprietà speciale, `onClick`. Aggiorniamo il nostro codice `CounterButton` di prima per consentirgli di contare i clic:

```jsx
function CounterButton() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(count + 1)}>Clicked {count} times</button>
  );
}
```

In questa versione stiamo utilizzando l'ulteriore funzionalità di `useState()` per creare una funzione speciale `setCount()`, che possiamo invocare per aggiornare il valore di `count`. Chiamiamo questa funzione all'interno del gestore di eventi `onClick` per impostare `count` al valore attuale più uno.

## Stile dei componenti

Ogni framework offre un modo per definire gli stili per i tuoi componenti — o per l'applicazione nel suo complesso. Anche se l'approccio di ogni framework alla definizione degli stili di un componente è leggermente diverso, tutti ti offrono diversi modi per farlo. Con l'aggiunta di alcuni moduli di supporto, puoi stilizzare le tue app del framework in [Sass](https://sass-lang.com/) o [Less](https://lesscss.org/), o transpilare i tuoi fogli di stile CSS con [PostCSS](https://postcss.org/).

## Gestione delle dipendenze

Tutti i principali framework forniscono meccanismi per gestire le dipendenze — usare componenti dentro altri componenti, a volte con livelli gerarchici multipli. Come per altre funzionalità, il meccanismo esatto differirà tra i diversi framework, ma il risultato finale è lo stesso. I componenti tendono a importare componenti in altri componenti usando la sintassi standard [JavaScript module](/it/docs/Web/JavaScript/Guide/Modules), o almeno qualcosa di simile.

### Componenti nei componenti

Un vantaggio chiave dell'architettura UI basata su componenti è che i componenti possono essere combinati insieme. Proprio come puoi scrivere tag HTML uno dentro l'altro per costruire un sito web, puoi utilizzare componenti dentro altri componenti per costruire un'applicazione web. Ogni framework ti permette di scrivere componenti che utilizzano (e quindi dipendono da) altri componenti.

Ad esempio, il nostro componente `AuthorCredit` React potrebbe essere utilizzato all'interno di un componente `Article`. Ciò significa che `Article` dovrebbe importare `AuthorCredit`.

```js
import AuthorCredit from "./components/AuthorCredit";
```

Una volta fatto ciò, `AuthorCredit` potrebbe essere utilizzato all'interno del componente `Article` in questo modo:

```jsx
<Article>
  <AuthorCredit />
</Article>
```

### Iniezione delle dipendenze

Le applicazioni del mondo reale possono spesso coinvolgere strutture di componenti con livelli di nidificazione multipli. Un componente `AuthorCredit` nidificato a molti livelli potrebbe, per qualche ragione, avere bisogno di dati dal livello radice della nostra applicazione.

Supponiamo che il sito della rivista che stiamo costruendo sia strutturato in questo modo:

```jsx
<App>
  <Home>
    <Article>
      <AuthorCredit {/* props */} />
    </Article>
  </Home>
</App>
```

Il nostro componente `App` possiede dati di cui il componente `AuthorCredit` ha bisogno. Potremmo riscrivere `Home` e `Article` in modo che sappiano passare props in giù, ma questo potrebbe diventare tedioso se ci sono molti, molti livelli tra l'origine e la destinazione dei nostri dati. È anche eccessivo: `Home` e `Article` in realtà non fanno uso del ritratto o della biografia dell'autore, ma se vogliamo ottenere quelle informazioni nell'`AuthorCredit`, dovremo modificare `Home` e `Article` per accoglierle.

Il problema di passare dati attraverso molti livelli di componenti è chiamato prop drilling, e non è l'ideale per le grandi applicazioni.

Per aggirare il prop drilling, i framework forniscono una funzionalità nota come iniezione delle dipendenze, che è un modo per portare determinati dati direttamente ai componenti che ne hanno bisogno, senza passarlo attraverso livelli intermedi. Ogni framework implementa l'iniezione delle dipendenze sotto un nome diverso e in un modo diverso, ma l'effetto è sostanzialmente lo stesso.

Angular chiama questo processo [dependency injection](https://angular.dev/guide/di/dependency-injection); Vue ha metodi [`provide()` e `inject()` per i componenti](https://v2.vuejs.org/v2/api/#provide-inject); React ha una [Context API](https://react.dev/learn/passing-data-deeply-with-context); Ember condivide lo stato attraverso i [servizi](https://guides.emberjs.com/release/services/).

### Lifecycle

Nel contesto di un framework, il **ciclo di vita** di un componente è una raccolta di fasi che un componente attraversa dal momento in cui viene aggiunto al DOM e poi reso dal browser (spesso chiamato _montaggio_) al momento in cui viene rimosso dal DOM (spesso chiamato _smontaggio_). Ciascun framework nomina queste fasi del ciclo di vita in modo diverso e non tutti danno accesso agli sviluppatori alle stesse fasi. Tutti i framework seguono il medesimo modello generale: consentono agli sviluppatori di eseguire determinate azioni quando il componente _si monta_, quando _si rende_, quando _si smonta_ e in molte fasi intermedie tra queste.

La fase _render_ è la più cruciale da capire, poiché viene ripetuta il maggior numero di volte quando l'utente interagisce con l'applicazione. Viene eseguita ogni volta che il browser deve rendere qualcosa di nuovo, che queste nuove informazioni siano un'aggiunta a ciò che è nel browser, una cancellazione, o una modifica di ciò che c'è.

Questo [diagramma del ciclo di vita di un componente React](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/) offre una panoramica generale del concetto.

## Renderizzazione degli elementi

Proprio come per i cicli di vita, i framework adottano approcci diversi ma simili su come renderizzano le tue applicazioni. Tutti tengono traccia della versione attuale renderizzata del DOM del browser, e ciascuno prende decisioni leggermente diverse su come il DOM dovrebbe cambiare mentre i componenti della tua applicazione vengono rerenderizzati. Poiché i framework prendono queste decisioni per te, generalmente non interagisci con il DOM da solo. Questa astrazione dal DOM è più complessa e più intensiva di memoria rispetto all'aggiornamento diretto del DOM, ma senza di essa i framework non potrebbero consentirti di programmare nel modo dichiarativo per cui sono noti.

Il **DOM virtuale** è un approccio secondo cui le informazioni sul DOM del browser sono memorizzate nella memoria JavaScript. La tua applicazione aggiorna questa copia del DOM, quindi la confronta con il "vero" DOM — il DOM che è effettivamente renderizzato per i tuoi utenti — per decidere cosa rendere. L'applicazione costruisce un "diff" per confrontare le differenze tra il DOM virtuale aggiornato e il DOM attualmente renderizzato, e utilizza quel diff per applicare aggiornamenti al DOM reale. Sia React che Vue utilizzano un modello di DOM virtuale, ma non applicano esattamente la stessa logica quando eseguono il diff o il rendering.

Puoi [leggere di più sul DOM virtuale nella documentazione di React](https://legacy.reactjs.org/docs/faq-internals.html#what-is-the-virtual-dom).

L'**Incremental DOM** è simile al DOM virtuale in quanto costruisce un diff del DOM per decidere cosa rendere, ma diverso in quanto non crea una copia completa del DOM nella memoria JavaScript. Ignora le parti del DOM che non hanno bisogno di essere cambiate. Angular è l'unico framework discusso finora in questo modulo che utilizza un Incremental DOM.

Puoi [leggere di più sull'Incremental DOM sul blog di Auth0](https://auth0.com/blog/incremental-dom/).

Il **Glimmer VM** è unico per Ember. Non è né un DOM virtuale né un Incremental DOM; è un processo separato attraverso il quale i template di Ember vengono transpiled in una sorta di "byte code" che è più facile e veloce da leggere rispetto a JavaScript.

## Routing

Come [menzionato nel capitolo precedente, il routing](/it/docs/Learn_web_development/Core/Frameworks_libraries/Introduction#routing) è una parte importante dell'esperienza web. Per evitare un'esperienza interrotta in app sufficientemente complesse con molte visualizzazioni, ciascuno dei framework trattati in questo modulo fornisce una libreria (o più di una libreria) che aiuta gli sviluppatori a implementare il routing lato client nelle loro applicazioni.

## Testing

Tutte le applicazioni traggono vantaggio da una copertura di test che assicura che il software continui a comportarsi come previsto, e le applicazioni web non fanno eccezione. L'ecosistema di ciascun framework fornisce strumenti che facilitano la scrittura dei test. Gli strumenti di test non sono integrati nei framework stessi, ma gli strumenti da riga di comando utilizzati per generare app del framework ti danno accesso agli strumenti di test appropriati.

Ciascun framework ha ampi strumenti nel proprio ecosistema, con capacità sia di unit testing che di integrazione.

[Testing Library](https://testing-library.com/) è una suite di utility per il test che ha strumenti per molti ambienti JavaScript, tra cui React, Vue e Angular. La documentazione di Ember copre il [testing delle app Ember](https://guides.emberjs.com/release/testing/).

Ecco un rapido test per il nostro `CounterButton` scritto con l'aiuto di React Testing Library — testa diverse cose, come l'esistenza del pulsante e se il pulsante visualizza il testo corretto dopo essere stato cliccato 0, 1 e 2 volte:

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

A questo punto dovresti avere un'idea più chiara dei linguaggi effettivi, delle caratteristiche e degli strumenti che utilizzerai mentre crei applicazioni con i framework. Sono sicuro che sei entusiasta di iniziare e di fare effettivamente un po' di coding, e questo è ciò che farai prossimamente!

{{PreviousMenuNext("Learn_web_development/Core/Frameworks_libraries/Introduction","Learn_web_development/Core/Frameworks_libraries/React_getting_started", "Learn_web_development/Core/Frameworks_libraries")}}
