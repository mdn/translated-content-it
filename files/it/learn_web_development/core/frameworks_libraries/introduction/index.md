---
title: Introduzione ai framework lato client
short-title: Introduction
slug: Learn_web_development/Core/Frameworks_libraries/Introduction
l10n:
  sourceCommit: 238b07dfeb8c347c590bd02a63140867525d511c
---

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Main_features", "Learn_web_development/Core/Frameworks_libraries")}}

Iniziamo l'analisi dei framework con una panoramica generale dell'ambito, esaminando una breve storia di JavaScript e dei framework, perché esistono i framework e cosa offrono, come iniziare a scegliere un framework da imparare e quali alternative esistono ai framework lato client.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza dei linguaggi fondamentali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Cosa sia il codice di terze parti e come sono nati i framework JavaScript lato client.</li>
          <li>Quali problemi risolvono i framework, quali alternative esistono e come sceglierne uno.</li>
          <li>La differenza tra librerie e framework.</li>
          <li>Quando usare e quando non usare i framework.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## La nascita di librerie e framework

Quando JavaScript debuttò nel 1996, aggiunse interattività e vivacità occasionali a un web che, fino ad allora, era composto da documenti statici. Il web divenne non solo un luogo in cui _leggere cose_, ma anche in cui _fare cose_. La popolarità di JavaScript aumentò costantemente. Gli sviluppatori che lavoravano con JavaScript scrissero strumenti per risolvere i problemi che incontravano e li raccolsero in pacchetti riutilizzabili chiamati **librerie**, così da poter condividere le proprie soluzioni con altri. Questo ecosistema condiviso di librerie contribuì a plasmare la crescita del web e alla fine portò ai framework.

Un **framework** è una libreria che propone un approccio preciso alla costruzione del software. Questo approccio consente prevedibilità e omogeneità in un'applicazione; la prevedibilità permette al software di crescere fino a dimensioni enormi restando comunque manutenibile; prevedibilità e manutenibilità sono essenziali per la salute e la longevità del software. L'avvento dei moderni framework JavaScript ha reso molto più semplice creare applicazioni altamente dinamiche e interattive.

I framework JavaScript alimentano gran parte del notevole software presente sul web moderno, inclusi molti dei siti web che probabilmente vengono usati ogni giorno.

## Quali framework esistono?

Esistono molti framework, ma attualmente i "quattro grandi" sono considerati i seguenti.

### Ember

[Ember](https://emberjs.com/) fu inizialmente rilasciato nel dicembre 2011 come continuazione del lavoro iniziato nel progetto [SproutCore](https://en.wikipedia.org/wiki/SproutCore). È un framework meno recente che ha meno utenti rispetto ad alternative più moderne come React e Vue, ma gode ancora di una discreta popolarità grazie alla sua stabilità, al supporto della community e ad alcuni ingegnosi principi di programmazione.

### Angular

[Angular](https://angular.dev/) è un framework open source per applicazioni web guidato dall'Angular Team di Google e da una community di individui e aziende. È una riscrittura completa realizzata dallo stesso team che ha creato [AngularJS](https://angularjs.org/). Angular è stato rilasciato ufficialmente il 14 settembre 2016.

Angular è un framework basato su componenti che usa template HTML dichiarativi. Durante la fase di build, in modo trasparente per gli sviluppatori, il compilatore del framework traduce i template in istruzioni JavaScript ottimizzate. Angular utilizza [TypeScript](https://www.typescriptlang.org/), un superset di JavaScript che verrà analizzato con un po' più di dettaglio nel prossimo capitolo.

### Vue

Dopo aver lavorato al progetto originale [AngularJS](https://angularjs.org/) e averne tratto insegnamenti, Evan You rilasciò [Vue](https://vuejs.org/) nel 2014. Vue è il più giovane dei quattro grandi, ma ha recentemente registrato un aumento di popolarità.

Vue, come Angular, estende HTML con parte del proprio codice. A parte questo, si basa principalmente su JavaScript moderno e standard.

### React

Facebook rilasciò [React](https://react.dev/) nel 2013. A quel punto, stava già usando React internamente per risolvere molti dei suoi problemi. Tecnicamente, React in sé _non_ è un framework; è una libreria per il rendering di componenti UI. React viene usato in combinazione con _altre_ librerie per creare applicazioni — React e [React Native](https://reactnative.dev/) consentono agli sviluppatori di creare applicazioni mobili; React e [ReactDOM](https://react.dev/reference/react-dom) consentono di creare applicazioni web, e così via.

Poiché React e ReactDOM vengono così spesso usati insieme, React è comunemente considerato un framework JavaScript. Durante la lettura di questo modulo, verrà adottata questa interpretazione comune.

React estende JavaScript con una sintassi simile a HTML, nota come [JSX](https://react.dev/learn/writing-markup-with-jsx).

## Perché esistono i framework?

È stato discusso il contesto che ha ispirato la creazione dei framework, ma non davvero il _perché_ gli sviluppatori abbiano sentito la necessità di crearli. Per esplorare il perché, è necessario prima esaminare le difficoltà dello sviluppo software.

Si consideri un tipo comune di applicazione: un creatore di liste di cose da fare, che verrà implementato usando una varietà di framework nei capitoli successivi. Questa applicazione dovrebbe permettere agli utenti di fare cose come visualizzare una lista di attività, aggiungere una nuova attività ed eliminare un'attività; inoltre, deve farlo tenendo traccia e aggiornando in modo affidabile i dati sottostanti all'applicazione. Nello sviluppo software, questi dati sottostanti sono noti come stato.

Ciascuno degli obiettivi è teoricamente semplice se considerato isolatamente. È possibile iterare sui dati per visualizzarli; è possibile aggiungere a un oggetto per creare una nuova attività; è possibile usare un identificatore per trovare, modificare o eliminare un'attività. Quando si ricorda che l'applicazione deve permettere all'utente di fare _tutte_ queste cose tramite il browser, iniziano a emergere alcune difficoltà. **Il vero problema è questo: ogni volta che si modifica lo stato dell'applicazione, occorre aggiornare la UI affinché corrisponda.**

È possibile esaminare la difficoltà di questo problema osservando una sola funzionalità dell'app di lista di cose da fare: il rendering di una lista di attività.

## La verbosità delle modifiche al DOM

Costruire elementi HTML e visualizzarli nel browser al momento opportuno richiede una quantità sorprendente di codice. Si supponga che lo stato sia un archivio chiave-valore contenente `taskName` (controllato dall'input di testo) e la lista di `tasks`:

```js
const state = {
  taskName: "",
  tasks: [
    {
      id: "todo-0",
      name: "Learn some frameworks!",
    },
  ],
};
```

Come viene mostrata una di queste attività agli utenti? Si desidera rappresentare ogni attività come un elemento di lista, ovvero un elemento HTML [`<li>`](/it/docs/Web/HTML/Reference/Elements/li) all'interno di un elemento lista non ordinata (un [`<ul>`](/it/docs/Web/HTML/Reference/Elements/ul)). Come si crea? Potrebbe avere un aspetto simile al seguente:

```js
function buildTodoItemEl(id, name) {
  const item = document.createElement("li");
  const span = document.createElement("span");

  span.textContent = name;

  item.id = id;
  item.appendChild(span);
  item.appendChild(buildDeleteButtonEl(id));

  return item;
}
```

Qui viene usato il metodo [`document.createElement()`](/it/docs/Web/API/Document/createElement) per creare il `<li>` e diverse altre righe di codice per creare le proprietà e gli elementi figli necessari.

Lo snippet precedente fa riferimento a un'altra funzione di costruzione: `buildDeleteButtonEl()`. Segue un modello simile a quello usato per costruire un elemento di lista:

```js
function buildDeleteButtonEl(id) {
  const button = document.createElement("button");
  button.setAttribute("type", "button");
  button.addEventListener("click", () => {
    state.tasks = state.tasks.filter((t) => t.id !== id);
    renderTodoList();
  });
  button.textContent = "Delete";

  return button;
}
```

La parte interessante da notare è che ogni volta che lo stato viene aggiornato, occorre chiamare manualmente `renderTodoList` affinché lo stato venga sincronizzato con lo schermo. Il codice che visualizzerà gli elementi sulla pagina potrebbe essere simile al seguente:

```js hidden
const todoFormEl = document.querySelector("#todo-form");
const todoInputEl = document.querySelector("#todo-input");
const todoListEl = document.querySelector("#todo-list");
```

```js
function renderTodoList() {
  const frag = document.createDocumentFragment();
  state.tasks.forEach((task) => {
    const item = buildTodoItemEl(task.id, task.name);
    frag.appendChild(item);
  });

  while (todoListEl.lastChild) {
    todoListEl.removeChild(todoListEl.lastChild);
  }
  todoListEl.appendChild(frag);
}
```

Ora ci sono quasi trenta righe di codice dedicate _solo_ alla UI, _solo_ al rendering di qualcosa nel DOM, e in nessun momento vengono aggiunte classi che potrebbero essere usate in seguito per assegnare stili agli elementi della lista.

Per chi fosse curioso, di seguito è disponibile una demo completa e funzionante. È possibile fare clic sul pulsante "Play" per visualizzare il codice sorgente nel playground.

```html hidden
<h1>TodoMatic</h1>
<form id="todo-form">
  <label for="todo-input">What needs to be done?</label>
  <input type="text" id="todo-input" autocomplete="on" />
  <button type="submit">Add</button>
</form>
<ul id="todo-list"></ul>
```

```css hidden
* + * {
  margin-top: 0.4rem;
}

html {
  font-size: 62.5%;
}

body {
  font-size: 2rem;
  line-height: 1.25;
  font-family:
    -apple-system, BlinkMacSystemFont, "Segoe UI", "Apple Color Emoji",
    "Segoe UI Emoji", "Segoe UI Symbol", "Roboto", "Helvetica", "Arial",
    sans-serif;
  color: hsl(0 0 0.13);

  width: 95%;
  max-width: 30em;
  padding-bottom: 2em;
  margin: 0 auto;
}

button,
input[type="text"] {
  font-size: 100%;
  line-height: 1.15;
  font-family: inherit;
  margin: 0;

  padding: 0.5rem;
  border: 1px solid #707070;
  border-radius: 2px;
}

* + button {
  margin-left: 0.4rem;
}

label {
  display: table;
}

ul {
  margin-top: 1.6rem;
  padding-left: 2em;
}

label + input[type="text"] {
  margin-top: 0.4rem;
}
```

```js hidden
function generateUniqueId(prefix = "prefix") {
  return `${prefix}-${Math.floor(Math.random() * Date.now())}`;
}

function createTask(name) {
  return {
    name,
    id: generateUniqueId("todo"),
  };
}

function renderInput() {
  todoInputEl.value = state.taskName;
}

todoInputEl.addEventListener("change", (e) => {
  state.taskName = e.target.value;
});
todoFormEl.addEventListener("submit", (e) => {
  e.preventDefault();
  state.tasks = [...state.tasks, createTask(state.taskName)];
  state.taskName = "";
  renderInput();
  renderTodoList();
});
renderInput();
renderTodoList();
```

{{EmbedLiveSample("the_verbosity_of_dom_change", "", "400", , , , , "allow-forms")}}

Lavorare direttamente con il DOM, come in questo esempio, richiede di comprendere molte cose sul funzionamento del DOM: come creare elementi; come modificarne le proprietà; come inserire elementi uno dentro l'altro; come visualizzarli nella pagina. Nessuno di questo codice gestisce realmente le interazioni dell'utente o affronta l'aggiunta o l'eliminazione di un'attività. Se si aggiungono queste funzionalità, occorre ricordarsi di aggiornare la UI al momento giusto e nel modo giusto.

I framework JavaScript sono stati creati per rendere questo tipo di lavoro molto più semplice: esistono per offrire una migliore _esperienza di sviluppo_. Non conferiscono nuovi poteri a JavaScript; rendono più semplice accedere alle capacità di JavaScript per poter sviluppare per il web attuale.

Approfondire le funzionalità JavaScript usate in questa sezione:

- [`Array.forEach()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
- [`Document.createDocumentFragment()`](/it/docs/Web/API/Document/createDocumentFragment)
- [`Document.createElement()`](/it/docs/Web/API/Document/createElement)
- [`Element.setAttribute()`](/it/docs/Web/API/Element/setAttribute)
- [`Node.appendChild()`](/it/docs/Web/API/Node/appendChild)
- [`Node.removeChild()`](/it/docs/Web/API/Node/removeChild)
- [`Node.textContent`](/it/docs/Web/API/Node/textContent)

## Un altro modo per creare UI

Ogni framework JavaScript offre un modo per scrivere interfacce utente in modo più _dichiarativo_. Ovvero, consentono di scrivere codice che descrive come dovrebbe apparire la UI, e il framework lo realizza nel DOM dietro le quinte.

L'approccio JavaScript vanilla per creare ripetutamente nuovi elementi DOM era difficile da comprendere a colpo d'occhio. Al contrario, il seguente blocco di codice illustra il modo in cui Vue potrebbe essere usato per descrivere la lista di attività:

```html
<ul>
  <li v-for="task in tasks" v-bind:key="task.id">
    <span>\{{task.name}}</span>
    <button type="button">Delete</button>
  </li>
</ul>
```

Tutto qui. Questo snippet riduce quasi trenta righe di codice a sei righe. Se le parentesi graffe e gli attributi `v-` non sono familiari, non è un problema; la sintassi specifica di Vue verrà illustrata più avanti nel modulo. L'aspetto importante è che questo codice assomiglia alla UI che rappresenta, mentre il codice JavaScript vanilla no.

Grazie a Vue, non è stato necessario scrivere funzioni per costruire la UI; il framework se ne occuperà in modo ottimizzato ed efficiente. L'unico ruolo qui era descrivere a Vue l'aspetto che dovrebbe avere ogni elemento. Gli sviluppatori che conoscono Vue possono comprendere rapidamente cosa sta accadendo quando entrano nel progetto. Vue non è l'unico in questo: l'uso di un framework migliora l'efficienza sia del team sia del singolo sviluppatore.

È possibile fare cose _simili_ in JavaScript vanilla. Le [stringhe template literal](/it/docs/Web/JavaScript/Reference/Template_literals) facilitano la scrittura di stringhe HTML che rappresentano l'aspetto dell'elemento finale. Potrebbe essere un'idea utile per qualcosa di semplice come l'applicazione di lista di cose da fare, ma non è manutenibile per applicazioni di grandi dimensioni che gestiscono migliaia di record di dati e potrebbero visualizzare altrettanti elementi unici in un'interfaccia utente.

## Altri vantaggi offerti dai framework

Esaminiamo alcuni degli altri vantaggi offerti dai framework. Come accennato in precedenza, i vantaggi dei framework sono ottenibili con JavaScript vanilla, ma l'uso di un framework elimina tutto il carico cognitivo necessario per risolvere personalmente questi problemi.

### Strumenti

Poiché ciascuno dei framework di questo modulo dispone di una community ampia e attiva, l'ecosistema di ogni framework offre strumenti che migliorano l'esperienza di sviluppo. Questi strumenti facilitano l'aggiunta di elementi quali test (per assicurare che l'applicazione si comporti come previsto) o linting (per assicurare che il codice sia privo di errori e coerente nello stile).

> [!NOTE]
> Per maggiori dettagli sui concetti relativi agli strumenti web, consultare la [panoramica degli strumenti lato client](/it/docs/Learn_web_development/Extensions/Client-side_tools/Overview).

### Compartimentazione

La maggior parte dei framework principali incoraggia gli sviluppatori ad astrarre le diverse parti delle proprie interfacce utente in _componenti_: porzioni di codice manutenibili e riutilizzabili che possono comunicare tra loro. Tutto il codice relativo a un determinato componente può risiedere in un file, o in un paio di file specifici, così che lo sviluppatore sappia esattamente dove intervenire per apportare modifiche a quel componente. In un'app JavaScript vanilla, sarebbe necessario creare un insieme di convenzioni per ottenere questo risultato in modo efficiente e scalabile. Molti sviluppatori JavaScript, lasciati a se stessi, potrebbero finire per distribuire in tutto un file il codice relativo a una parte della UI, oppure collocarlo del tutto in un altro file.

### Routing

La funzionalità più essenziale del web è che consente agli utenti di navigare da una pagina all'altra: è, dopotutto, una rete di documenti interconnessi. Seguendo un collegamento su questo stesso sito web, il browser comunica con un server e recupera nuovo contenuto da visualizzare. Nel farlo, l'URL nella barra degli indirizzi cambia. È possibile salvare questo nuovo URL e tornare alla pagina in seguito, oppure condividerlo con altri affinché possano trovare facilmente la stessa pagina. Il browser ricorda anche la cronologia di navigazione e consente di navigare avanti e indietro. Questo è chiamato **routing lato server**.

Le applicazioni web moderne in genere non recuperano e visualizzano nuovi file HTML: caricano una singola struttura HTML e aggiornano continuamente il DOM al suo interno (denominate **single page app**, o **SPA**) senza portare gli utenti a nuovi indirizzi sul web. Ogni nuova pseudo-pagina web viene generalmente chiamata _view_ e, per impostazione predefinita, non viene effettuato alcun routing.

Quando una SPA è sufficientemente complessa e visualizza un numero adeguato di view uniche, è importante introdurre la funzionalità di routing nell'applicazione. Le persone sono abituate a poter creare collegamenti a pagine specifiche di un'applicazione, navigare avanti e indietro nella propria cronologia di navigazione e così via, e la loro esperienza peggiora quando queste funzionalità web standard non funzionano. Quando il routing viene gestito in questo modo da un'applicazione client, viene opportunamente chiamato **routing lato client**.

È _possibile_ creare un router usando le capacità native di JavaScript e del browser, ma i framework popolari e sviluppati attivamente dispongono di librerie complementari che rendono il routing una parte più intuitiva del processo di sviluppo.

## Aspetti da considerare quando si usano i framework

Essere uno sviluppatore web efficace significa usare gli strumenti più appropriati per il lavoro. I framework JavaScript rendono semplice lo sviluppo di applicazioni front-end, ma non sono una soluzione miracolosa che risolverà tutti i problemi. Questa sezione tratta alcuni aspetti da considerare quando si usano i framework. Tenere presente che potrebbe non essere necessario alcun framework: evitare di usare un framework soltanto per usarne uno.

### Familiarità con lo strumento

Proprio come JavaScript vanilla, i framework richiedono tempo per essere imparati e hanno le proprie peculiarità. Prima di decidere di usare un framework per un progetto, assicurarsi di avere tempo per imparare abbastanza delle sue funzionalità affinché sia utile anziché d'ostacolo, e assicurarsi che anche i membri del team lo conoscano con sufficiente sicurezza.

### Sovraingegnerizzazione

Se il progetto di sviluppo web è un portfolio personale con poche pagine e tali pagine hanno poca o nessuna capacità interattiva, un framework, e tutto il relativo JavaScript, potrebbe non essere affatto necessario. Detto questo, i framework non sono monolitici e alcuni sono più adatti di altri ai piccoli progetti. In un articolo per Smashing Magazine, Sarah Drasner scrive di come [Vue possa sostituire jQuery](https://www.smashingmagazine.com/2018/02/jquery-vue-javascript/) come strumento per rendere interattive piccole porzioni di una pagina web.

### Base di codice più ampia e astrazione

I framework consentono di scrivere codice più dichiarativo, e talvolta _meno_ codice nel complesso, gestendo le interazioni con il DOM dietro le quinte. Questa astrazione è ottima per l'esperienza dello sviluppatore, ma non è gratuita. Per tradurre ciò che viene scritto in modifiche al DOM, i framework devono eseguire il proprio codice, rendendo a sua volta il software finale più grande e più costoso dal punto di vista computazionale.

Una certa quantità di codice aggiuntivo è inevitabile e un framework che supporta il tree-shaking, ovvero la rimozione durante il processo di build di qualsiasi codice non effettivamente utilizzato nell'app, permetterà di mantenere piccole le applicazioni. Tuttavia, questo è comunque un fattore da tenere presente quando si considerano le prestazioni dell'app, specialmente su dispositivi con maggiori limitazioni di rete o archiviazione, come i telefoni cellulari.

L'astrazione dei framework influisce non solo sul JavaScript, ma anche sul rapporto con la natura stessa del web. Indipendentemente da come si sviluppa per il web, il risultato finale, il livello con cui gli utenti interagiscono in ultima analisi, è HTML. Scrivere l'intera applicazione in JavaScript può far perdere di vista HTML e lo scopo dei suoi vari tag, portando alla produzione di un documento HTML non semantico e non accessibile. Infatti, è possibile scrivere un'applicazione fragile che dipende interamente da JavaScript e che non funzionerà senza di esso.

I framework non sono la fonte dei problemi. Con priorità sbagliate, qualsiasi applicazione può essere fragile, gonfia e inaccessibile. Tuttavia, i framework amplificano le priorità degli sviluppatori. Se la priorità è creare un'app web complessa, è semplice farlo. Tuttavia, se le priorità non proteggono attentamente prestazioni e accessibilità, i framework amplificheranno fragilità, peso e inaccessibilità. Le priorità moderne degli sviluppatori, amplificate dai framework, hanno invertito la struttura del web in molti contesti. Invece di una solida rete di documenti incentrata sui contenuti, il web ora spesso mette JavaScript al primo posto e l'esperienza utente all'ultimo.

## Accessibilità in un web guidato dai framework

Partendo da quanto detto nella sezione precedente, parliamo un po' più approfonditamente di accessibilità. Rendere accessibili le interfacce utente richiede sempre riflessione e impegno, e i framework possono complicare questo processo. Spesso è necessario utilizzare API avanzate del framework per accedere a funzionalità native del browser come le [live region](/it/docs/Web/Accessibility/ARIA/Guides/Live_regions) ARIA o la gestione del focus.

In alcuni casi, le applicazioni basate su framework creano barriere di accessibilità che non esistono nei siti web tradizionali. L'esempio principale è il routing lato client, menzionato in precedenza.

Con il routing tradizionale lato server, la navigazione sul web produce risultati prevedibili. Il browser sa di dover impostare il focus nella parte superiore della pagina e le tecnologie assistive annunceranno il titolo della pagina. Queste operazioni avvengono ogni volta che si naviga verso una nuova pagina.

Con il routing lato client, il browser non sta caricando nuove pagine web, quindi non sa che dovrebbe regolare automaticamente il focus o annunciare il titolo di una nuova pagina. Gli autori dei framework hanno dedicato enormi quantità di tempo e lavoro alla scrittura di JavaScript che ricrea queste funzionalità e, anche così, nessun framework è riuscito a farlo perfettamente.

La conclusione è che l'accessibilità dovrebbe essere considerata fin dall'inizio di _ogni_ progetto web, ma occorre tenere presente che le basi di codice astratte che usano framework hanno maggiori probabilità di soffrire di importanti problemi di accessibilità se non lo si fa.

## Come scegliere un framework

Ciascuno dei framework discussi in questo modulo adotta approcci diversi allo sviluppo di applicazioni web. Ognuno viene regolarmente migliorato o modificato e ciascuno ha vantaggi e svantaggi. Scegliere il framework giusto è un processo che dipende dal team e dal progetto, e occorre svolgere ricerche personali per individuare ciò che soddisfa le esigenze. Detto questo, sono state individuate alcune domande che possono essere poste per esaminare le opzioni in modo più efficace:

1. Quali browser supporta il framework?
2. Quali linguaggi specifici di dominio utilizza il framework?
3. Il framework ha una community solida e buona documentazione, oltre ad altro supporto disponibile?

La tabella in questa sezione fornisce un riepilogo immediato dell'attuale _supporto dei browser_ offerto da ciascun framework, nonché dei **linguaggi specifici di dominio** con cui può essere usato.

In generale, i {{Glossary("DSL/Domain_specific_language", "linguaggi specifici di dominio (DSL)")}} sono linguaggi di programmazione rilevanti in aree specifiche dello sviluppo software. Nel contesto dei framework, i DSL sono varianti di JavaScript o HTML che facilitano lo sviluppo con quel framework. È importante notare che nessuno dei framework _richiede_ allo sviluppatore di usare un DSL specifico, ma quasi tutti sono stati progettati pensando a un DSL specifico. Scegliere di non usare il DSL preferito da un framework significa rinunciare a funzionalità che altrimenti migliorerebbero l'esperienza di sviluppo.

Quando si sceglie un framework per qualsiasi nuovo progetto, è necessario considerare seriamente la matrice di supporto e i DSL. Un supporto dei browser non adeguato può rappresentare una barriera per gli utenti; un supporto dei DSL non adeguato può rappresentare una barriera per lo sviluppatore e il suo team.

| Framework | Supporto browser                      | DSL preferito  | DSL supportati             | Riferimento                                                                                |
| --------- | ------------------------------------- | -------------- | -------------------------- | ------------------------------------------------------------------------------------------ |
| Angular   | Moderni                               | TypeScript     | Basato su HTML; TypeScript | [documentazione ufficiale](https://angular.dev/guide/browser-support)                      |
| React     | Moderni                               | JSX            | JSX; TypeScript            | [documentazione ufficiale](https://react.dev/reference/react-dom/client#browser-support)   |
| Vue       | Moderni (IE9+ in Vue 2)               | Basato su HTML | Basato su HTML, JSX, Pug   | [documentazione ufficiale](https://cli.vuejs.org/guide/browser-compatibility.html)         |
| Ember     | Moderni (IE9+ in Ember versione 2.18) | Handlebars     | Handlebars, TypeScript     | [documentazione ufficiale](https://guides.emberjs.com/v3.3.0/templates/handlebars-basics/) |

> [!NOTE]
> I DSL descritti come "basati su HTML" non hanno nomi ufficiali. Non sono davvero DSL veri e propri, ma sono HTML non standard, quindi si ritiene utile evidenziarli.

### Il framework ha una community solida?

Questa è forse la metrica più difficile da misurare, poiché la dimensione della community non è direttamente correlata a numeri facilmente accessibili. È possibile controllare il numero di stelle GitHub di un progetto o i download npm settimanali per farsi un'idea della sua popolarità, ma a volte la cosa migliore da fare è cercare in alcuni forum o parlare con altri sviluppatori. Non conta solo la dimensione della community, ma anche quanto sia accogliente e inclusiva, nonché la qualità della documentazione disponibile.

### Opinioni sul web

Non limitarsi a credere a quanto viene detto qui: esistono discussioni in tutto il web. La Wikimedia Foundation ha recentemente scelto di usare Vue per il proprio front-end e ha pubblicato una [richiesta di commenti (RFC) sull'adozione di un framework](https://phabricator.wikimedia.org/T241180). Eric Gardner, autore dell'RFC, ha dedicato tempo a delineare le esigenze del progetto Wikimedia e il motivo per cui alcuni framework erano buone scelte per il team. Questo RFC rappresenta un ottimo esempio del tipo di ricerca da svolgere quando si pianifica di usare un framework front-end.

Il [sondaggio State of JavaScript](https://stateofjs.com/) è un'utile raccolta di feedback da parte degli sviluppatori JavaScript. Copre molti argomenti relativi a JavaScript, inclusi dati sia sull'uso dei framework sia sul giudizio degli sviluppatori nei loro confronti. Attualmente sono disponibili dati relativi a diversi anni, che consentono di farsi un'idea della popolarità di un framework.

Il team Vue ha [confrontato esaustivamente Vue con altri framework popolari](https://v2.vuejs.org/v2/guide/comparison.html). Potrebbe esserci qualche pregiudizio in questo confronto, come viene segnalato dagli stessi autori, ma rimane comunque una risorsa preziosa.

## Alternative ai framework lato client

Se si cercano strumenti per velocizzare il processo di sviluppo web e si sa che il progetto non richiederà JavaScript lato client intensivo, è possibile ricorrere a una delle diverse altre soluzioni per creare il web:

- Un sistema di gestione dei contenuti
- Rendering lato server
- Un generatore di siti statici

### Sistemi di gestione dei contenuti

I **sistemi di gestione dei contenuti** (**CMS**) sono strumenti che consentono a un utente di creare contenuti per il web senza scrivere direttamente il codice. Sono una buona soluzione per progetti di grandi dimensioni, specialmente per progetti che richiedono il contributo di autori di contenuti con capacità di programmazione limitate, o per programmatori che desiderano risparmiare tempo. Tuttavia, richiedono una quantità significativa di tempo per la configurazione e l'uso di un CMS implica rinunciare almeno a una parte del controllo sull'output finale del sito web. Ad esempio, se il CMS scelto non crea contenuti accessibili per impostazione predefinita, spesso è difficile migliorare questo aspetto.

Alcuni CMS popolari includono [WordPress](https://wordpress.com/), [Joomla](https://www.joomla.org/) e [Drupal](https://new.drupal.org/).

### Rendering lato server

Il **rendering lato server** (**SSR**) è un'architettura applicativa in cui il compito di visualizzare una single-page application è del _server_. È l'opposto del _rendering lato client_, che è il modo più comune e diretto per creare un'applicazione JavaScript. Il rendering lato server grava meno sul dispositivo client, perché viene inviato soltanto un file HTML già visualizzato, ma può essere più difficile da configurare rispetto a un'applicazione visualizzata lato client.

Tutti i framework trattati in questo modulo supportano il rendering lato server oltre al rendering lato client. Consultare [Next.js](https://nextjs.org/) per React, [Nuxt](https://nuxt.com/) per Vue (sì, è confuso e no, questi progetti non sono correlati!), [FastBoot](https://github.com/ember-fastboot/ember-cli-fastboot) per Ember e [Angular Universal](https://angular.dev/guide/universal) per Angular.

> [!NOTE]
> Alcune soluzioni SSR sono scritte e gestite dalla community, mentre altre sono soluzioni "ufficiali" fornite dal manutentore del framework.

### Generatori di siti statici

I {{Glossary("SSG", "generatori di siti statici")}} sono programmi che generano dinamicamente tutte le pagine web di un sito web multipagina, incluso qualsiasi CSS o JavaScript pertinente, affinché possano essere pubblicate in un numero qualsiasi di luoghi. L'host di pubblicazione potrebbe essere, ad esempio, un branch GitHub Pages, un'istanza Netlify o qualsiasi server privato scelto. Questo approccio presenta numerosi vantaggi, soprattutto in termini di prestazioni, poiché il dispositivo dell'utente non deve costruire la pagina con JavaScript: è già completa, e sicurezza, poiché le pagine statiche hanno meno vettori di attacco. Questi siti possono comunque utilizzare JavaScript dove necessario, ma non ne sono _dipendenti_. I generatori di siti statici richiedono tempo per essere imparati, come qualsiasi altro strumento, e ciò può rappresentare una barriera al processo di sviluppo.

I siti statici possono avere poche o moltissime pagine uniche. Così come i framework consentono di scrivere rapidamente applicazioni JavaScript lato client, i generatori di siti statici offrono un modo per creare rapidamente file HTML che altrimenti sarebbero stati scritti singolarmente. Come i framework, i generatori di siti statici consentono agli sviluppatori di scrivere componenti che definiscono parti comuni delle pagine web e di comporre tali componenti insieme per creare una pagina finale. Nel contesto dei generatori di siti statici, questi componenti sono chiamati **template**. Le pagine web create dai generatori di siti statici possono persino ospitare applicazioni framework: se si desidera che una pagina specifica del sito web generato staticamente avvii un'applicazione React quando un utente la visita, è possibile farlo.

I generatori di siti statici esistono da molto tempo e sono oggetto di ottimizzazione e innovazione costanti. Sono disponibili numerose opzioni, tra cui [Astro](https://astro.build/), [Eleventy](https://www.11ty.dev/), [Hugo](https://gohugo.io/), [Jekyll](https://jekyllrb.com/) e [Gatsby](https://www.gatsbyjs.com/), che si basano su vari stack tecnologici e offrono funzionalità distintive. Altre opzioni, come [Docusaurus](https://docusaurus.io/) e [VitePress](https://vitepress.dev/), usano framework lato client invece di template, ma generano file statici ottimizzati in modo analogo.

Per approfondire i generatori di siti statici in generale, consultare la [guida per principianti a Eleventy](https://www.tatianamac.com/posts/beginner-eleventy-tutorial-parti/) di Tatiana Mac. Nel primo articolo della serie viene spiegato cosa sia un generatore di siti statici e come si relazioni ad altri modi di pubblicare contenuti web.

## Riepilogo

Si conclude così l'introduzione ai framework: non è stato ancora illustrato alcun codice, ma si spera sia stato fornito un utile contesto sul perché usare i framework, su come sceglierne uno e sul desiderio di imparare di più e iniziare a lavorare concretamente.

Il prossimo articolo approfondisce maggiormente l'argomento, esaminando i tipi specifici di funzionalità che i framework tendono a offrire e perché funzionano nel modo in cui funzionano.

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Main_features", "Learn_web_development/Core/Frameworks_libraries")}}
