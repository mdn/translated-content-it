---
title: Introduzione ai framework lato client
short-title: Introduction
slug: Learn_web_development/Core/Frameworks_libraries/Introduction
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Main_features", "Learn_web_development/Core/Frameworks_libraries")}}

Iniziamo la nostra esplorazione dei framework con una panoramica generale dell'argomento, esaminando una breve storia di JavaScript e dei framework, il motivo per cui esistono e cosa ci offrono, come iniziare a riflettere sulla scelta di un framework da apprendere e quali alternative esistono ai framework lato client.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i fondamenti delle lingue <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Cos'è il codice di terze parti e come sono nati i framework JavaScript lato client.</li>
          <li>Quali problemi risolvono i framework, quali alternative ci sono e come scegliere un framework.</li>
          <li>La differenza tra librerie e framework.</li>
          <li>Quando i framework dovrebbero e non dovrebbero essere usati.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## L'emergere di librerie e framework

Quando JavaScript è stato lanciato nel 1996, ha aggiunto un'interattività occasionale e un pizzico di entusiasmo a un web che, fino ad allora, era composto da documenti statici. Il web è diventato non solo un luogo per _leggere cose_, ma per _fare cose_. La popolarità di JavaScript è aumentata costantemente. Gli sviluppatori che lavoravano con JavaScript hanno scritto strumenti per risolvere i problemi che affrontavano, e li hanno confezionati in pacchetti riutilizzabili chiamati **librerie**, così da poter condividere le loro soluzioni con altri. Questo ecosistema condiviso di librerie ha contribuito a plasmare la crescita del web, e ha dato origine ai framework.

Un **framework** è una libreria che offre opinioni su come viene costruito un software. Queste opinioni consentono prevedibilità e omogeneità in un'applicazione; la prevedibilità consente al software di scalare a dimensioni enormi e rimanere comunque manutenibile; la prevedibilità e la manutenibilità sono essenziali per la salute e la longevità del software. L'avvento dei moderni framework JavaScript ha reso molto più semplice la costruzione di applicazioni altamente dinamiche e interattive.

I framework JavaScript alimentano gran parte del software impressionante presente nel web moderno, inclusi molti dei siti Web che probabilmente utilizza ogni giorno.

## Quali framework esistono?

Ci sono molti framework disponibili, ma attualmente i "quattro grandi" sono considerati i seguenti.

### Ember

[Ember](https://emberjs.com/) è stato inizialmente rilasciato nel dicembre 2011 come continuazione del lavoro iniziato nel progetto [SproutCore](https://en.wikipedia.org/wiki/SproutCore). È un framework più vecchio che ha meno utenti rispetto alle alternative più moderne come React e Vue, ma gode ancora di una discreta popolarità grazie alla sua stabilità, al supporto della comunità e a principi di codifica ingegnosi.

### Angular

[Angular](https://angular.dev/) è un framework per applicazioni web open-source guidato dal team Angular presso Google e da una comunità di individui e aziende. È una riscrittura completa dello stesso team che ha costruito [AngularJS](https://angularjs.org/). Angular è stato ufficialmente rilasciato il 14 settembre 2016.

Angular è un framework basato su componenti che utilizza template HTML dichiarativi. Al momento della compilazione, trasparentemente per gli sviluppatori, il compilatore del framework traduce i modelli in istruzioni JavaScript ottimizzate. Angular utilizza [TypeScript](https://www.typescriptlang.org/), un superset di JavaScript che esamineremo più in dettaglio nel prossimo capitolo.

### Vue

Dopo aver lavorato e appreso dal progetto originale [AngularJS](https://angularjs.org/), Evan You ha rilasciato [Vue](https://vuejs.org/) nel 2014. Vue è il più giovane dei quattro grandi, ma ha recentemente visto un aumento di popolarità.

Vue, come [AngularJS](https://angularjs.org/), estende HTML con alcune delle sue proprie code. A parte questo, si basa principalmente su JavaScript moderno e standard.

### React

Facebook ha rilasciato [React](https://react.dev/) nel 2013. A questo punto, lo stava già utilizzando per risolvere molti dei suoi problemi interni. Tecnicamente, React stessa _non è_ un framework; è una libreria per il rendering di componenti UI. React viene utilizzato in combinazione con _altre_ librerie per creare applicazioni — React e [React Native](https://reactnative.dev/) consentono agli sviluppatori di creare applicazioni mobili; React e [ReactDOM](https://react.dev/reference/react-dom) consentono di creare applicazioni web, ecc.

Poiché React e ReactDOM vengono così spesso usati insieme, React viene inteso colloquialmente come un framework JavaScript. Durante la lettura di questo modulo, lavoreremo con questa comprensione colloquiale.

React estende JavaScript con una sintassi simile a HTML, nota come [JSX](https://react.dev/learn/writing-markup-with-jsx).

## Perché esistono i framework?

Abbiamo discusso dell'ambiente che ha ispirato la creazione dei framework, ma non veramente _del perché_ gli sviluppatori hanno sentito il bisogno di crearli. Esplorare il perché richiede prima di esaminare le sfide dello sviluppo software.

Consideriamo un tipo comune di applicazione: un creatore di liste di cose da fare, che vedremo come implementare utilizzando una varietà di framework nei capitoli successivi. Questa applicazione dovrebbe consentire agli utenti di fare cose come visualizzare un elenco di attività, aggiungere una nuova attività e eliminare un'attività; e deve farlo mentre traccia e aggiorna in modo affidabile i dati sottostanti dell'applicazione. In sviluppo software, questi dati sottostanti sono noti come stato.

Ognuno dei nostri obiettivi è teoricamente semplice preso isolatamente. Possiamo iterare sui dati per visualizzarli; possiamo aggiungere a un oggetto per creare una nuova attività; possiamo utilizzare un identificatore per trovare, modificare o eliminare un'attività. Quando ricordiamo che l'applicazione deve consentire all'utente di fare _tutte_ queste cose tramite il browser, iniziano a emergere alcune crepe. **Il vero problema è questo: ogni volta che modifichiamo lo stato della nostra applicazione, dobbiamo aggiornare l'interfaccia utente per corrispondere.**

Possiamo esaminare la difficoltà di questo problema osservando solo _una_ funzione della nostra app lista delle cose da fare: visualizzare un elenco di attività.

## La verbosità delle modifiche al DOM

Costruire elementi HTML e renderli nel browser al momento appropriato richiede una quantità sorprendente di codice. Diciamo che il nostro stato è un array di oggetti strutturato in questo modo:

```js
const state = [
  {
    id: "todo-0",
    name: "Learn some frameworks!",
  },
];
```

Come mostriamo una di queste attività ai nostri utenti? Vogliamo rappresentare ogni attività come elemento di lista - un elemento HTML [`<li>`](/it/docs/Web/HTML/Reference/Elements/li) all'interno di un elemento lista non ordinata (un [`<ul>`](/it/docs/Web/HTML/Reference/Elements/ul)). Come lo costruiamo? Potrebbe apparire in questo modo:

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

Qui, utilizziamo il metodo [`document.createElement()`](/it/docs/Web/API/Document/createElement) per creare il nostro `<li>`, e diversi ulteriori righe di codice per creare le proprietà e gli elementi figli necessari.

Lo snippet precedente fa riferimento a un'altra funzione di costruzione: `buildDeleteButtonEl()`. Segue un modello simile a quello che abbiamo usato per costruire un elemento della lista:

```js
function buildDeleteButtonEl(id) {
  const button = document.createElement("button");
  button.setAttribute("type", "button");
  button.textContent = "Delete";

  return button;
}
```

Questo pulsante non fa ancora nulla, ma lo farà più avanti una volta che decidiamo di implementare la nostra funzione di cancellazione. Il codice che renderà i nostri elementi sulla pagina potrebbe apparire in questo modo:

```js
function renderTodoList() {
  const frag = document.createDocumentFragment();
  state.tasks.forEach((task) => {
    const item = buildTodoItemEl(task.id, task.name);
    frag.appendChild(item);
  });

  while (todoListEl.firstChild) {
    todoListEl.removeChild(todoListEl.firstChild);
  }
  todoListEl.appendChild(frag);
}
```

Abbiamo ora quasi trenta righe di codice dedicate _solo_ all'interfaccia utente - _solo_ per visualizzare qualcosa nel DOM - e in nessun momento aggiungiamo classi che potremmo utilizzare in seguito per stilizzare i nostri elementi di lista!

Lavorare direttamente con il DOM, come in questo esempio, richiede la comprensione di molte cose su come funziona il DOM: come creare elementi; come modificare le loro proprietà; come mettere elementi l'uno dentro l'altro; come metterli sulla pagina. Nessuno di questo codice gestisce effettivamente le interazioni utente o affronta l'aggiunta o l'eliminazione di un'attività. Se aggiungiamo queste funzionalità, dobbiamo ricordare di aggiornare il nostro UI al momento giusto e nel modo giusto.

I framework JavaScript sono stati creati per rendere questo tipo di lavoro molto più facile — esistono per fornire una migliore _esperienza per lo sviluppatore_. Non portano nuove capacità a JavaScript; ti danno un accesso più facile alle capacità di JavaScript in modo da poter costruire per il web di oggi.

Se vuoi vedere in azione i frammenti di codice di questa sezione, puoi controllare una [versione funzionante dell'app su CodePen](https://codepen.io/mxmason/pen/XWbPNmw), che consente anche agli utenti di aggiungere ed eliminare nuove attività.

Leggi di più sulle funzionalità JavaScript utilizzate in questa sezione:

- [`Array.forEach()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
- [`Document.createDocumentFragment()`](/it/docs/Web/API/Document/createDocumentFragment)
- [`Document.createElement()`](/it/docs/Web/API/Document/createElement)
- [`Element.setAttribute()`](/it/docs/Web/API/Element/setAttribute)
- [`Node.appendChild()`](/it/docs/Web/API/Node/appendChild)
- [`Node.removeChild()`](/it/docs/Web/API/Node/removeChild)
- [`Node.textContent`](/it/docs/Web/API/Node/textContent)

## Un altro modo per costruire interfacce utente

Ogni framework JavaScript offre un modo per scrivere interfacce utente in modo più _dichiarativo_. Vale a dire, consentono di scrivere codice che descrive come dovrebbe apparire la tua interfaccia utente, e il framework lo realizza nel DOM dietro le quinte.

L'approccio JavaScript standard alla creazione di nuovi elementi DOM ripetuti era difficile da comprendere a colpo d'occhio. Al contrario, il seguente blocco di codice illustra il modo in cui potresti utilizzare Vue per descrivere il nostro elenco di attività:

```html
<ul>
  <li v-for="task in tasks" v-bind:key="task.id">
    <span>\{{task.name}}</span>
    <button type="button">Delete</button>
  </li>
</ul>
```

È tutto. Questo frammento riduce quasi trenta righe di codice a sei righe. Se le parentesi graffe e gli attributi `v-` qui non ti sono familiari, va bene; imparerai la sintassi specifica di Vue più avanti nel modulo. La cosa da considerare qui è che questo codice sembra l'interfaccia utente che rappresenta, mentre il codice JavaScript standard no.

Grazie a Vue, non abbiamo dovuto scrivere le nostre funzioni per costruire l'interfaccia utente; il framework se ne occuperà per noi in modo ottimizzato e efficiente. Il nostro unico ruolo qui era descrivere a Vue come dovrebbe apparire ogni elemento. Gli sviluppatori familiari con Vue possono rapidamente capire cosa sta succedendo quando si uniscono al nostro progetto. Vue non è solo in questo: utilizzare un framework migliora l'efficienza sia del team che individuale.

È possibile fare cose _simili_ a questo in JavaScript standard. [Le stringhe letterali dei modelli](/it/docs/Web/JavaScript/Reference/Template_literals) rendono facile scrivere stringhe di HTML che rappresentano come dovrebbe apparire l'elemento finale. Potrebbe essere un'idea utile per qualcosa di semplice come la nostra applicazione lista delle cose da fare, ma non è sostenibile per applicazioni grandi che gestiscono migliaia di record di dati, e potrebbero visualizzare altrettanti elementi unici in un'interfaccia utente.

## Altre cose che ci offrono i framework

Esaminiamo alcuni degli altri vantaggi offerti dai framework. Come abbiamo accennato in precedenza, i vantaggi dei framework sono raggiungibili in JavaScript standard, ma utilizzare un framework ti solleva dall'onere cognitivo di dover risolvere questi problemi da soli.

### Strumenti

Poiché ciascuno dei framework in questo modulo ha una vasta e attiva community, l'ecosistema di ciascun framework fornisce strumenti che migliorano l'esperienza dello sviluppatore. Questi strumenti facilitano l'aggiunta di cose come il testing (per garantire che la tua applicazione si comporti come dovrebbe) o il linting (per garantire che il tuo codice sia privo di errori e coerente nello stile).

> [!NOTE]
> Se vuole scoprire ulteriori dettagli sui concetti di web tooling, consulti il nostro [Panoramica sugli strumenti lato client](/it/docs/Learn_web_development/Extensions/Client-side_tools/Overview).

### Compartimentalizzazione

La maggior parte dei framework principali incoraggia gli sviluppatori ad astrarre le diverse parti delle loro interfacce utente in _componenti_ — blocchi di codice manutenibili e riutilizzabili che possono comunicare tra di loro. Tutto il codice relativo a un dato componente può risiedere in un file (o in un paio di file specifici) in modo che lei, come sviluppatore, sappia esattamente dove andare per apportare modifiche a quel componente. In un'app JavaScript standard, dovrebbe creare il proprio set di convenzioni per ottenere ciò in modo efficiente e scalabile. Molti sviluppatori JavaScript, se lasciati a sé stessi, potrebbero finire con tutto il codice relativo a una parte dell'interfaccia utente sparso su un file, o addirittura in un altro file.

### Routing

La caratteristica più essenziale del web è che permette agli utenti di navigare da una pagina all'altra: è, dopo tutto, una rete di documenti interconnessi. Quando segue un collegamento su questo sito web, il suo browser comunica con un server e recupera nuovo contenuto da mostrare. Mentre lo fa, l'URL nella barra degli indirizzi cambia. Può salvare questo nuovo URL e tornare alla pagina in seguito, o condividerlo con altri affinché possano trovare facilmente la stessa pagina. Anche il suo browser ricorda la cronologia di navigazione e le permette di andare avanti e indietro. Questo si chiama **routing lato server**.

Le applicazioni web moderne tipicamente non recuperano e visualizzano nuovi file HTML: caricano un unico guscio HTML e aggiornano continuamente il DOM al suo interno (chiamato **applicazioni a pagina singola**, o **SPA**) senza navigare gli utenti a nuovi indirizzi sul web. Ogni nuova pseudo-pagina web viene solitamente chiamata _vista_, e per impostazione predefinita non viene fatto alcun routing.

Quando una SPA è abbastanza complessa e visualizza abbastanza viste uniche, è importante portare la funzionalità di routing nella sua applicazione. Le persone sono abituate a essere in grado di collegarsi a pagine specifiche in un'applicazione, viaggiare avanti e indietro nella loro cronologia di navigazione, ecc., e la loro esperienza ne soffre quando queste caratteristiche web standard vengono interrotte. Quando il routing è gestito da un'applicazione client in questo modo, è opportunamente chiamato **routing lato client**.

È _possibile_ creare un router utilizzando le capacità native di JavaScript e del browser, ma i framework popolari, attivamente sviluppati, hanno librerie complementari che rendono il routing una parte più intuitiva del processo di sviluppo.

## Cose da considerare quando si usano i framework

Essere uno sviluppatore web efficace significa utilizzare gli strumenti più appropriati per il lavoro. I framework JavaScript rendono lo sviluppo dell'applicazione front-end facile, ma non sono una bacchetta magica che risolverà tutti i problemi. Questa sezione discute alcune delle cose che dovrebbe considerare quando utilizza i framework. Tenga presente che potrebbe non aver bisogno di un framework affatto — stia attenta a non finire per usare un framework solo per il gusto di farlo.

### Familiarità con lo strumento

Proprio come con JavaScript standard, i framework richiedono tempo per essere appresi e hanno le loro particolarità. Prima di decidere di utilizzare un framework per un progetto, si assicuri di avere il tempo di imparare abbastanza delle sue caratteristiche affinché sia utile a lei piuttosto che lavorare contro di lei, e si assicuri che anche i suoi team ne siano a proprio agio.

### Sovraingegnerizzazione

Se il suo progetto di sviluppo web è un portfolio personale con poche pagine, e queste pagine hanno poche o nessuna capacità interattiva, un framework (e tutti i suoi JavaScript) potrebbe non essere necessario affatto. Detto ciò, i framework non sono monolitici e alcuni di essi sono meglio adatti a piccoli progetti rispetto ad altri. In un articolo per Smashing Magazine, Sarah Drasner scrive su come [Vue possa sostituire jQuery](https://www.smashingmagazine.com/2018/02/jquery-vue-javascript/) come strumento per rendere interattive piccole porzioni di una pagina web.

### Base di codice più ampia e astrazione

I framework le permettono di scrivere codice più dichiarativo — e talvolta _meno_ codice complessivamente — gestendo le interazioni con il DOM per lei, dietro le quinte. Questa astrazione è ottima per la sua esperienza come sviluppatore, ma non è gratuita. Per tradurre ciò che scrive in cambiamenti nel DOM, i framework devono eseguire il proprio codice, che a sua volta rende il suo pezzo di software finale più grande e più costoso dal punto di vista computazionale.

Un po' di codice in più è inevitabile, e un framework che supporta il tree-shaking (rimozione di qualsiasi codice che non viene effettivamente utilizzato nell'app durante il processo di build) le permetterà di mantenere le sue applicazioni piccole, ma questo è comunque un fattore che dovrà tenere in mente quando considera le prestazioni della sua app, soprattutto su dispositivi più limitati per la rete/archiviazione, come i telefoni cellulari.

L'astrazione dei framework influenza non solo il suo JavaScript, ma anche il suo rapporto con la stessa natura del web. Non importa come costruisce per il web, il risultato finale, il livello con cui gli utenti interagiscono ultimamente, è l'HTML. Scrivere la sua intera applicazione in JavaScript può far perdere di vista l'HTML e il suo scopo dei vari tag, e portarla a produrre un documento HTML non-semantico e inaccessibile. Infatti, è possibile scrivere un'applicazione fragile che dipende totalmente da JavaScript e non funzionerà senza di esso.

I framework non sono la fonte dei nostri problemi. Con le priorità sbagliate, qualsiasi applicazione può essere fragile, gonfiata e inaccessibile. Tuttavia, i framework amplificano le nostre priorità come sviluppatori. Se la sua priorità è rendere un'app web complessa, è facile farlo. Tuttavia, se le sue priorità non tutelano attentamente le prestazioni e l'accessibilità, i framework amplificheranno la sua fragilità, il suo gonfiamento e la sua inaccessibilità. Le priorità moderne degli sviluppatori, amplificate dai framework, hanno invertito la struttura del web in molti luoghi. Invece di una rete robusta e incentrata sui contenuti, il web ora spesso mette JavaScript al primo posto e l'esperienza dell'utente all'ultimo.

## Accessibilità in un web guidato dai framework

Approfondiamo ciò che abbiamo detto nella sezione precedente e parliamo un po' di più sull'accessibilità. Rendere accessibili le interfacce utente richiede sempre un po' di pensiero e impegno, e i framework possono complicare quel processo. Spesso deve utilizzare API avanzate dei framework per accedere a funzionalità native del browser come le aree live di ARIA [regioni live](/it/docs/Web/Accessibility/ARIA/Guides/Live_regions) o la gestione del fuoco.

In alcuni casi, le applicazioni framework creano barriere all'accessibilità che non esistono per i siti web tradizionali. L'esempio più grande è nel routing lato client, come menzionato in precedenza.

Con il routing tradizionale (lato server), navigare nel web produce risultati prevedibili. Il browser sa di impostare il fuoco in cima alla pagina e le tecnologie assistive annunceranno il titolo della pagina. Queste cose accadono ogni volta che naviga su una nuova pagina.

Con il routing lato client, il suo browser non sta caricando nuove pagine web, quindi non sa che dovrebbe regolare automaticamente il fuoco o annunciare un nuovo titolo di pagina. Gli autori dei framework hanno dedicato enormi quantità di tempo e lavoro a scrivere JavaScript che ricrea queste funzionalità, e anche allora, nessun framework l'ha fatto perfettamente.

Il risultato è che dovrebbe considerare l'accessibilità fin dall'inizio di _ogni_ progetto web, ma tenga a mente che le basi di codice astratte che utilizzano framework sono più soggette a soffrire di gravi problemi di accessibilità se non lo fa.

## Come scegliere un framework

Ciascuno dei framework discussi in questo modulo adotta approcci diversi allo sviluppo dell'applicazione web. Ognuno viene regolarmente migliorato o modificato, e ognuno ha i suoi pro e contro. Scegliere il framework giusto è un processo dipendente dal team e dal progetto, e dovresti fare la tua ricerca per scoprire cosa soddisfa le sue esigenze. Detto ciò, abbiamo identificato alcune domande che può porre per ricercare le sue opzioni in modo più efficace:

1. Quali browser supporta il framework?
2. Quali linguaggi specifici di dominio utilizza il framework?
3. Il framework ha una community solida e buona documentazione (e altri supporti) disponibili?

La tabella in questa sezione fornisce un sommario in sintesi del corrente _supporto browser_ offerto da ciascun framework, così come i **linguaggi specifici di dominio** con cui può essere utilizzato.

In generale, {{Glossary("DSL/Domain_specific_language", "i linguaggi specifici di dominio (DSLs)")}} sono linguaggi di programmazione rilevanti in specifiche aree dello sviluppo software. Nel contesto dei framework, i DSLs sono variazioni su JavaScript o HTML che facilitano lo sviluppo con quel framework. Crucialmente, nessuno dei framework _richiede_ che un sviluppatore utilizzi uno specifico DSL, ma sono quasi tutti stati progettati con un DSL specifico in mente. Scegliere di non utilizzare il DSL preferito di un framework significherà che perderà funzionalità che altrimenti migliorerebbero la vostra esperienza di sviluppo.

Dovrebbe considerare seriamente la matrice di supporto e i DSL di un framework quando decide per un nuovo progetto. Un supporto browser non abbinato può essere una barriera per i suoi utenti; un supporto DSL non abbinato può essere una barriera per lei e i suoi compagni di squadra.

| Framework | Supporto del browser                 | DSL preferito | DSL supportati         | Citazione                                                                         |
| --------- | ----------------------------------- | ------------- | ---------------------- | --------------------------------------------------------------------------------- |
| Angular   | Moderno                             | TypeScript    | Basato su HTML; TypeScript | [documentazione ufficiale](https://angular.dev/guide/browser-support)              |
| React     | Moderno                             | JSX           | JSX; TypeScript         | [documentazione ufficiale](https://react.dev/reference/react-dom/client#browser-support) |
| Vue       | Moderno (IE9+ in Vue 2)             | Basato su HTML| Basato su HTML, JSX, Pug| [documentation ufficiale](https://cli.vuejs.org/guide/browser-compatibility.html) |
| Ember     | Moderno (IE9+ in Ember version 2.18)| Handlebars    | Handlebars, TypeScript | [documentazione ufficiale](https://guides.emberjs.com/v3.3.0/templates/handlebars-basics/) |

> [!NOTE]
> I DSL che abbiamo descritto come "basati su HTML" non hanno nomi ufficiali. Non sono davvero veri DSL, ma sono HTML non standard, quindi crediamo che valga la pena evidenziarli.

### Il framework ha una community solida?

Questo è forse il criterio più difficile da misurare perché la dimensione della community non si correla direttamente a numeri facili da ottenere. Può controllare il numero di stelle di GitHub di un progetto o il numero di download settimanali su npm per avere un'idea della sua popolarità, ma a volte la cosa migliore da fare è cercare sui forum o parlare con altri sviluppatori. Non si tratta solo della dimensione della community, ma anche di quanto è accogliente e inclusiva, e di quanto è buona la documentazione disponibile.

### Opinioni sul web

Non prenda solo la nostra parola in materia — ci sono discussioni su tutto il web. La Fondazione Wikimedia ha recentemente scelto di utilizzare Vue per il suo front-end, e ha postato una [richiesta di commento (RFC) sull'adozione del framework](https://phabricator.wikimedia.org/T241180). Eric Gardner, l'autore della RFC, ha preso tempo per delineare le esigenze del progetto Wikimedia e perché alcuni framework erano buone scelte per il team. Questa RFC serve come un ottimo esempio del tipo di ricerca che dovrebbe fare da sé quando pianifica di usare un framework front-end.

Il [State of JavaScript survey](https://stateofjs.com/) è una collezione utile di feedback da parte degli sviluppatori JavaScript. Copre molti argomenti relativi a JavaScript, inclusi dati sull'uso dei framework e il sentimento degli sviluppatori nei loro confronti. Attualmente, sono disponibili diversi anni di dati, consentendole di farsi un'idea della popolarità di un framework.

Il team di Vue ha [confrontato in modo esaustivo Vue con altri framework popolari](https://v2.vuejs.org/v2/guide/comparison.html). Potrebbe esserci un certo pregiudizio in questo confronto (che essi notano), ma è comunque una risorsa preziosa.

## Alternative ai framework lato client

Se sta cercando strumenti per accelerare il processo di sviluppo web e sa che il suo progetto non richiederà JavaScript lato client intensivo, potrebbe optare per una delle molte soluzioni alternative per la creazione del web:

- Un content management system
- Il rendering lato server
- Un generatore di siti statici

### Content management systems

**I sistemi di gestione dei contenuti** (**CMS**) sono strumenti che consentono a un utente di creare contenuti per il web senza scrivere direttamente codice. Sono una buona soluzione per progetti di grandi dimensioni, soprattutto progetti che richiedono input da parte di autori di contenuti con abilità di codifica limitate, o per programmatori che vogliono risparmiare tempo. Tuttavia, richiedono un significativo tempo di configurazione, e utilizzando un CMS si rinuncia a un certo controllo sull'output finale del sito web. Ad esempio: se il CMS scelto non crea contenuti accessibili per impostazione predefinita, è spesso difficile migliorare questo aspetto.

Alcuni CMS popolari includono [WordPress](https://wordpress.com/), [Joomla](https://www.joomla.org/) e [Drupal](https://new.drupal.org/).

### Rendering lato server

Il **rendering lato server** (**SSR**) è un'architettura applicativa in cui è compito del _server_ eseguire il rendering di un'applicazione a pagina singola. Questo è l'opposto del _rendering lato client_, che è il modo più comune e diretto per costruire un'applicazione JavaScript. Il rendering lato server è più facile per il dispositivo del client perché gli invia solo un file HTML renderizzato, ma può essere difficile da impostare rispetto a un'applicazione renderizzata lato client.

Tutti i framework trattati in questo modulo supportano il rendering lato server oltre al rendering lato client. Dai un'occhiata a [Next.js](https://nextjs.org/) per React, [Nuxt](https://nuxt.com/) per Vue (sì, è confuso, e no, questi progetti non sono correlati!), [FastBoot](https://github.com/ember-fastboot/ember-cli-fastboot) per Ember e [Angular Universal](https://angular.dev/guide/universal) per Angular.

> [!NOTE]
> Alcune soluzioni SSR sono scritte e manutenute dalla community, mentre altre sono soluzioni "ufficiali" fornite dal mantenitore del framework.

### Generatori di siti statici

{{Glossary("SSG", "I generatori di siti statici")}} sono programmi che generano dinamicamente tutte le pagine web in un sito web multipagina — inclusi eventuali CSS o JavaScript pertinenti — in modo che possano essere pubblicati in molti luoghi. Il host di pubblicazione potrebbe essere un branch di GitHub Pages, un'istanza di Netlify, o qualsiasi server privato a scelta, per esempio. Ci sono vari vantaggi in questo approccio, principalmente riguardo alle prestazioni (il dispositivo dell'utente non sta costruendo la pagina con JavaScript; è già completa) e sicurezza (le pagine statiche hanno meno punti vulnerabili). Questi siti possono comunque utilizzare JavaScript laddove necessario, ma non dipendono _da esso_. I generatori di siti statici richiedono tempo per essere appresi, proprio come qualsiasi altro strumento, il che può essere un ostacolo nel suo processo di sviluppo.

I siti statici possono avere quante poche o quante più pagine uniche si desidera. Così come i framework le consentono di scrivere rapidamente applicazioni JavaScript lato client, i generatori di siti statici le offrono un modo per creare rapidamente file HTML che altrimenti avrebbe scritto singolarmente. Come i framework, i generatori di siti statici consentono agli sviluppatori di scrivere componenti che definiscono parti comuni delle pagine web e di comporre questi componenti insieme per creare una pagina finale. Nel contesto dei generatori di siti statici, questi componenti sono chiamati **template**. I siti web costruiti da generatori di siti statici possono persino ospitare applicazioni framework: se desidera che una specifica pagina del suo sito web staticamente generato esegua un'app React quando un utente la visita, può farlo.

I generatori di siti statici esistono da molto tempo, e sono oggetto di costanti ottimizzazioni e innovazioni. Esistono diverse opzioni, tra cui [Astro](https://astro.build/), [Eleventy](https://www.11ty.dev/), [Hugo](https://gohugo.io/), [Jekyll](https://jekyllrb.com/) e [Gatsby](https://www.gatsbyjs.com/), che si basano su vari stack tecnologici e forniscono funzionalità distintive. Altre opzioni, come [Docusaurus](https://docusaurus.io/) e [VitePress](https://vitepress.dev/), utilizzano framework lato client invece di template, ma generano file statici ottimizzati in modo simile.

Se vuole saperne di più sui generatori di siti statici in generale, controlli la [Guida per principianti all'Eleventy](https://www.tatianamac.com/posts/beginner-eleventy-tutorial-parti/) di Tatiana Mac. Nel primo articolo della serie, spiega cos'è un generatore di siti statici e come si relaziona ad altri mezzi di pubblicazione di contenuti web.

## Riepilogo

E questo conclude la nostra introduzione ai framework — non abbiamo ancora insegnato alcun codice, ma speriamo di averle fornito un contesto utile sul perché utilizzare framework in primo luogo e come scegliere uno, e di averla resa entusiasta di imparare di più ed entrare nel vivo!

Il nostro prossimo articolo scende a un livello inferiore, esaminando i tipi specifici di funzionalità che i framework tendono a offrire e perché funzionano come fanno.

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Main_features", "Learn_web_development/Core/Frameworks_libraries")}}
