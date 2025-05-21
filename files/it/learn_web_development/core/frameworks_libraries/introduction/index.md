---
title: Introduzione ai framework lato client
short-title: Introduction
slug: Learn_web_development/Core/Frameworks_libraries/Introduction
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Main_features", "Learn_web_development/Core/Frameworks_libraries")}}

Iniziamo il nostro esame dei framework con una panoramica generale dell'area, guardando una breve storia di JavaScript e dei framework, perché i framework esistono e cosa ci offrono, come iniziare a pensare di scegliere un framework da imparare e quali alternative esistono ai framework lato client.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a> core.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Cos'è il codice di terze parti e come sono nati i framework JavaScript lato client.</li>
          <li>Quali problemi risolvono i framework, quali alternative ci sono e come sceglierne uno.</li>
          <li>La differenza tra librerie e framework.</li>
          <li>Quando i framework dovrebbero e non dovrebbero essere utilizzati.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## L'emergere di librerie e framework

Quando JavaScript è stato lanciato nel 1996, aggiungeva interattività e vivacità occasionali a un web che era, fino ad allora, composto di documenti statici. Il web è diventato non solo un luogo per _leggere_, ma per _fare cose_. La popolarità di JavaScript è aumentata costantemente. Gli sviluppatori che lavoravano con JavaScript hanno scritto strumenti per risolvere i problemi che affrontavano e li hanno confezionati in pacchetti riutilizzabili chiamati **librerie**, in modo da poter condividere le loro soluzioni con altri. Questo ecosistema condiviso di librerie ha aiutato a plasmare la crescita del web, e alla fine ha aperto la strada ai framework.

Un **framework** è una libreria che offre opinioni su come dovrebbe essere costruito il software. Queste opinioni consentono una prevedibilità e una omogeneità in un'applicazione; la prevedibilità consente al software di scalare fino a dimensioni enormi e di rimanere comunque mantenibile; la prevedibilità e la manutenibilità sono essenziali per la salute e la longevità del software. L'avvento dei moderni framework JavaScript ha reso molto più facile costruire applicazioni altamente dinamiche e interattive.

I framework JavaScript alimentano gran parte del software impressionante sul web moderno, comprese molte delle pagine web che probabilmente usi ogni giorno.

## Quali framework sono disponibili?

Ci sono molti framework disponibili, ma attualmente i "quattro grandi" sono considerati i seguenti.

### Ember

[Ember](https://emberjs.com/) è stato inizialmente rilasciato nel dicembre 2011 come continuazione del lavoro iniziato nel progetto [SproutCore](https://en.wikipedia.org/wiki/SproutCore). È un framework più vecchio che ha meno utenti rispetto a soluzioni più moderne come React e Vue, ma gode ancora di una discreta popolarità grazie alla sua stabilità, al supporto della comunità e a alcuni principi di codifica intelligenti.

### Angular

[Angular](https://angular.dev/) è un framework open-source per applicazioni web guidato dal Team Angular di Google e da una comunità di persone e aziende. È una riscrittura completa del team che ha costruito [AngularJS](https://angularjs.org/). Angular è stato ufficialmente rilasciato il 14 settembre 2016.

Angular è un framework basato su componenti che utilizza template HTML dichiarativi. Al momento della creazione, in modo trasparente per gli sviluppatori, il compilatore del framework traduce i template in istruzioni JavaScript ottimizzate. Angular utilizza [TypeScript](https://www.typescriptlang.org/), un superset di JavaScript che esamineremo più dettagliatamente nel capitolo successivo.

### Vue

Dopo aver lavorato e imparato dal progetto originale [AngularJS](https://angularjs.org/), Evan You ha rilasciato [Vue](https://vuejs.org/) nel 2014. Vue è il più giovane dei quattro grandi, ma ha recentemente goduto di un aumento di popolarità.

Vue, come [AngularJS](https://angularjs.org/), estende l'HTML con parte del suo codice. A parte questo, si affida principalmente a JavaScript moderno e standard.

### React

Facebook ha rilasciato [React](https://react.dev/) nel 2013. A quel punto, lo aveva già utilizzato internamente per risolvere molti dei suoi problemi. Tecnicamente, React stesso _non è_ un framework; è una libreria per il rendering di componenti UI. React è usato in combinazione con _altre_ librerie per creare applicazioni — React e [React Native](https://reactnative.dev/) consentono agli sviluppatori di creare applicazioni mobili; React e [ReactDOM](https://react.dev/reference/react-dom) permettono loro di creare applicazioni web, ecc.

Poiché React e ReactDOM sono così spesso usati insieme, React è colloquialmente inteso come un framework JavaScript. Man mano che leggerai questo modulo, lavoreremo con questa comprensione colloquiale.

React estende JavaScript con una sintassi simile a HTML, nota come [JSX](https://react.dev/learn/writing-markup-with-jsx).

## Perché esistono i framework?

Abbiamo discusso dell'ambiente che ha ispirato la creazione dei framework, ma non davvero _perché_ gli sviluppatori hanno sentito il bisogno di crearli. Esplorare il perché richiede prima di esaminare le sfide dello sviluppo di software.

Considera un tipo comune di applicazione: un creatore di liste di cose da fare, che esamineremo implementando con una varietà di framework nei capitoli futuri. Questa applicazione dovrebbe consentire agli utenti di fare cose come visualizzare un elenco di attività, aggiungere una nuova attività e eliminare un'attività; e deve farlo mantenendo in modo affidabile il tracciamento e l'aggiornamento dei dati sottostanti l'applicazione. Nel contesto dello sviluppo software, questi dati sottostanti sono conosciuti come stato.

Ogni nostro obiettivo è teoricamente semplice in isolamento. Possiamo iterare sui dati per visualizzarli; possiamo aggiungere a un oggetto per creare una nuova attività; possiamo utilizzare un identificatore per trovare, modificare o eliminare un'attività. Quando ricordiamo che l'applicazione deve consentire all'utente di fare _tutte_ queste cose attraverso il browser, iniziano a emergere delle crepe. **Il vero problema è questo: ogni volta che cambiamo lo stato della nostra applicazione, abbiamo bisogno di aggiornare l'interfaccia utente per farla corrispondere.**

Possiamo esaminare la difficoltà di questo problema osservando solo _una_ caratteristica della nostra app di lista delle cose da fare: visualizzare un elenco di attività.

## La verbosità dei cambiamenti del DOM

Creare elementi HTML e visualizzarli nel browser al momento giusto richiede sorprendentemente un sacco di codice. Supponiamo che il nostro stato sia un array di oggetti strutturato in questo modo:

```js
const state = [
  {
    id: "todo-0",
    name: "Learn some frameworks!",
  },
];
```

Come mostriamo uno di questi compiti ai nostri utenti? Vogliamo rappresentare ogni compito come un elemento della lista – un elemento HTML [`<li>`](/it/docs/Web/HTML/Reference/Elements/li) all'interno di un elemento della lista non ordinata (un [`<ul>`](/it/docs/Web/HTML/Reference/Elements/ul)). Come lo facciamo? Potrebbe sembrare qualcosa del genere:

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

Qui, usiamo il metodo [`document.createElement()`](/it/docs/Web/API/Document/createElement) per creare il nostro `<li>`, e molte più righe di codice per creare le proprietà e gli elementi figli di cui ha bisogno.

Il frammento precedente fa riferimento a un'altra funzione di costruzione: `buildDeleteButtonEl()`. Segue un modello simile a quello che abbiamo usato per costruire un elemento della lista:

```js
function buildDeleteButtonEl(id) {
  const button = document.createElement("button");
  button.setAttribute("type", "button");
  button.textContent = "Delete";

  return button;
}
```

Questo pulsante non fa ancora nulla, ma lo farà più tardi una volta che decidiamo di implementare la nostra funzione di eliminazione. Il codice che renderà i nostri elementi sulla pagina potrebbe essere qualcosa di simile a questo:

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

Ora abbiamo quasi trenta righe di codice dedicate _solo_ all'interfaccia utente – _solo_ per visualizzare qualcosa nel DOM – e in nessun momento aggiungiamo classi che potremmo utilizzare in seguito per stilizzare i nostri elementi della lista!

Lavorare direttamente con il DOM, come in questo esempio, richiede di capire molte cose su come funziona il DOM: come creare elementi; come cambiare le loro proprietà; come mettere gli elementi dentro gli altri; come farli apparire sulla pagina. Nessuno di questo codice gestisce effettivamente le interazioni degli utenti o si occupa di aggiungere o eliminare un'attività. Se aggiungiamo queste funzionalità, dobbiamo ricordare di aggiornare la nostra interfaccia utente al momento giusto e nel modo giusto.

I framework JavaScript sono stati creati per rendere questo tipo di lavoro molto più facile — esistono per fornire una migliore _esperienza dello sviluppatore_. Non portano nuovi poteri a JavaScript; ti danno un accesso più facile ai poteri di JavaScript in modo che tu possa costruire per il web di oggi.

Se vuoi vedere esempi di codice di questa sezione in azione, puoi dare un'occhiata a una [versione funzionante dell'app su CodePen](https://codepen.io/mxmason/pen/XWbPNmw), che consente agli utenti di aggiungere ed eliminare nuove attività.

Leggi di più sulle funzionalità JavaScript utilizzate in questa sezione:

- [`Array.forEach()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
- [`Document.createDocumentFragment()`](/it/docs/Web/API/Document/createDocumentFragment)
- [`Document.createElement()`](/it/docs/Web/API/Document/createElement)
- [`Element.setAttribute()`](/it/docs/Web/API/Element/setAttribute)
- [`Node.appendChild()`](/it/docs/Web/API/Node/appendChild)
- [`Node.removeChild()`](/it/docs/Web/API/Node/removeChild)
- [`Node.textContent`](/it/docs/Web/API/Node/textContent)

## Un altro modo di costruire le interfacce utente

Ogni framework JavaScript offre un modo per scrivere interfacce utente più _dichiarativamente_. Cioè, ti permettono di scrivere codice che descrive come dovrebbe apparire la tua interfaccia utente, e il framework lo realizza nel DOM dietro le quinte.

L'approccio JavaScript vanilla al costruire nuovi elementi DOM in ripetizione era difficile da capire a colpo d'occhio. Al contrario, il seguente blocco di codice illustra il modo in cui potresti utilizzare Vue per descrivere la nostra lista di compiti:

```html
<ul>
  <li v-for="task in tasks" v-bind:key="task.id">
    <span>\{{task.name}}</span>
    <button type="button">Delete</button>
  </li>
</ul>
```

Ecco. Questo frammento riduce quasi trenta righe di codice a sei righe. Se le parentesi graffe e gli attributi `v-` qui ti sono estranei, va bene; imparerai la sintassi specifica di Vue più avanti nel modulo. La cosa da osservare qui è che questo codice somiglia alla UI che rappresenta, mentre il codice JavaScript vanilla no.

Grazie a Vue, non abbiamo dovuto scrivere le nostre funzioni per costruire la UI; il framework si occuperà di ciò per noi in un modo ottimizzato ed efficiente. Il nostro unico ruolo qui è stato quello di descrivere a Vue come dovrebbe apparire ogni elemento. Gli sviluppatori che sono familiari con Vue possono rapidamente capire cosa sta succedendo quando si uniscono al nostro progetto. Vue non è solo in questo: usare un framework migliora l'efficienza del team oltre che quella individuale.

È possibile fare cose _simili_ a questa in JavaScript vanilla. [Le stringhe di literal template](/it/docs/Web/JavaScript/Reference/Template_literals) rendono facile scrivere stringhe di HTML che rappresentano quello che l'elemento finale sembrerebbe. Potrebbe essere un'idea utile per qualcosa di semplice come la nostra applicazione di lista delle cose da fare, ma non è sostenibile per grandi applicazioni che gestiscono migliaia di record di dati e potrebbero visualizzare altrettanti elementi unici in una interfaccia utente.

## Altre cose che i framework ci offrono

Vediamo alcuni dei vantaggi offerti dai framework. Come abbiamo suggerito prima, i vantaggi dei framework sono raggiungibili con JavaScript vanilla, ma usare un framework elimina tutto il carico cognitivo di dover risolvere questi problemi da soli.

### Strumenti

Poiché ognuno dei framework in questo modulo ha una grande e attiva comunità, l'ecosistema di ogni framework fornisce strumenti che migliorano l'esperienza dello sviluppatore. Questi strumenti rendono facile aggiungere cose come test (per assicurarti che la tua applicazione si comporti come dovrebbe) o linting (per assicurarti che il tuo codice sia privo di errori e stilisticamente coerente).

> [!NOTE]
> Se vuoi scoprire maggiori dettagli sui concetti di strumenti web, dai un'occhiata alla nostra [Panoramica degli strumenti lato client](/it/docs/Learn_web_development/Extensions/Client-side_tools/Overview).

### Compartmentalization

La maggior parte dei framework principali incoraggia gli sviluppatori ad astrarre le diverse parti delle loro interfacce utente in _componenti_ — blocchi di codice manutenibili e riutilizzabili che possono comunicare tra loro. Tutto il codice relativo a un determinato componente può trovarsi in un file (o in un paio di file specifici) in modo che tu, come sviluppatore, sappia esattamente dove andare per apportare modifiche a quel componente. In un'app JavaScript vanilla, dovresti creare il tuo insieme di convenzioni per ottenere questo in modo efficiente e scalabile. Molti sviluppatori JavaScript, se lasciati alle loro disposizioni, potrebbero finire con tutto il codice relativo a una parte dell'interfaccia utente sparso in tutto un file, o in un altro file del tutto.

### Routing

La caratteristica più essenziale del web è che consente agli utenti di navigare da una pagina all'altra – è, dopo tutto, una rete di documenti interconnessi. Quando segui un link su questo stesso sito, il tuo browser comunica con un server e recupera nuovi contenuti da visualizzare per te. Mentre lo fa, l'URL nella barra degli indirizzi cambia. Puoi salvare questo nuovo URL e tornare alla pagina in seguito, o condividerlo con altri in modo che possano trovare facilmente la stessa pagina. Il tuo browser ricorda la tua cronologia di navigazione e ti consente di navigare avanti e indietro. Questo è chiamato **routing lato server**.

Le applicazioni web moderne di solito non recuperano e visualizzano nuovi file HTML: caricano un unico shell HTML e aggiornano continuamente il DOM al suo interno (denominati **app a pagina singola**, o **SPAs**) senza navigare gli utenti verso nuovi indirizzi sul web. Ogni nuova pseudo-pagina web è di solito chiamata _vista_, e per impostazione predefinita, nessuna routing viene effettuato.

Quando un SPA è abbastanza complesso e visualizza abbastanza viste uniche, è importante introdurre la funzionalità di routing nella tua applicazione. Le persone sono abituate a poter collegarsi a pagine specifiche in un'applicazione, viaggiare avanti e indietro nella loro cronologia di navigazione, ecc., e la loro esperienza ne risente quando queste funzionalità standard del web vengono interrotte. Quando il routing è gestito da un'applicazione client in questo modo, è giustamente chiamato **routing lato client**.

È _possibile_ realizzare un router utilizzando le capacità native di JavaScript e del browser, ma i framework popolari e attivamente sviluppati hanno librerie correlate che rendono il routing una parte più intuitiva del processo di sviluppo.

## Cose da considerare quando si utilizzano i framework

Essere uno sviluppatore web efficace significa usare gli strumenti più appropriati per il lavoro. I framework JavaScript rendono lo sviluppo di applicazioni front-end facile, ma non sono una soluzione che risolverà tutti i problemi. Questa sezione discute alcune delle cose che dovresti considerare quando usi i framework. Tieni presente che potresti non avere bisogno di un framework del tutto — attenzione a non finire per usare un framework solo per il gusto di farlo.

### Familiarità con lo strumento

Proprio come JavaScript vanilla, i framework richiedono tempo per essere appresi e hanno le loro peculiarità. Prima di decidere di usare un framework per un progetto, assicurati di avere tempo per imparare abbastanza delle sue caratteristiche perché sia utile a te piuttosto che lavorare contro di te, e assicurati che i tuoi compagni di squadra siano a loro agio con esso.

### Sovraingegnerizzazione

Se il tuo progetto di sviluppo web è un portfolio personale con alcune pagine, e quelle pagine hanno poche o nessuna funzionalità interattiva, un framework (e tutto il suo JavaScript) potrebbe non essere affatto necessario. Detto ciò, i framework non sono monolitici, e alcuni di essi sono più adatti a progetti piccoli rispetto ad altri. In un articolo per Smashing Magazine, Sarah Drasner scrive di come [Vue può sostituire jQuery](https://www.smashingmagazine.com/2018/02/jquery-vue-javascript/) come strumento per rendere interattiva una parte piccola di una pagina web.

### Maggiore base di codice e astrazione

I framework ti consentono di scrivere codice più dichiarativo e a volte _meno_ codice in generale, gestendo per te le interazioni con il DOM, dietro le quinte. Questa astrazione è ottima per la tua esperienza come sviluppatore, ma non è gratuita. Per tradurre ciò che scrivi in cambiamenti del DOM, i framework devono eseguire il proprio codice, che a sua volta rende il tuo pezzo finale di software più grande e più dispendioso in termini di calcoli da operare.

Un po' di codice extra è inevitabile, e un framework che supporta il tree-shaking (rimozione di qualsiasi codice che non viene effettivamente utilizzato nell'app durante il processo di build) ti permetterà di mantenere le tue applicazioni piccole, ma questo è comunque un fattore che devi tenere a mente quando consideri le prestazioni della tua app, specialmente su dispositivi con vincoli di rete/memoria, come i telefoni cellulari.

L'astrazione dei framework influenza non solo il tuo JavaScript, ma anche il tuo rapporto con la natura stessa del web. Indipendentemente da come costruisci per il web, il risultato finale, lo strato con cui i tuoi utenti interagiscono effettivamente, è HTML. Scrivere tutta la tua applicazione in JavaScript può farti perdere di vista l'HTML e lo scopo dei suoi vari tag, e portarti a produrre un documento HTML non semantico e inaccessibile. Infatti, è possibile scrivere un'applicazione fragile che dipende interamente da JavaScript e non funzionerà senza di essa.

I framework non sono la fonte dei nostri problemi. Con le priorità sbagliate, qualsiasi applicazione può essere fragile, ingombrante e inaccessibile. I framework, tuttavia, amplificano le nostre priorità come sviluppatori. Se la tua priorità è fare un'app web complessa, è facile farlo. Tuttavia, se le tue priorità non proteggono attentamente prestazioni e accessibilità, i framework amplificheranno la tua fragilità, il tuo ingombro e la tua inaccessibilità. Le priorità degli sviluppatori moderni, amplificate dai framework, hanno invertito la struttura del web in molti luoghi. Invece di una rete robusta, orientata ai contenuti dei documenti, il web ora spesso mette JavaScript al primo posto e l'esperienza utente all'ultimo.

## Accessibilità su un web guidato dai framework

Costruiamo su quanto detto nella sezione precedente e parliamo un po' di più dell'accessibilità. Rendere accessibili le interfacce utente richiede sempre un po' di pensiero e sforzo, e i framework possono complicare tale processo. Spesso devi utilizzare API avanzate del framework per accedere alle funzionalità native del browser, come le [regioni dal vivo](/it/docs/Web/Accessibility/ARIA/Guides/Live_regions) ARIA o la gestione del focus.

In alcuni casi, le applicazioni del framework creano barriere all'accessibilità che non esistono per i siti web tradizionali. L'esempio più grande di questo è nel routing lato client, come menzionato in precedenza.

Con il routing tradizionale (lato server), navigare sul web produce risultati prevedibili. Il browser sa di dover impostare il focus in cima alla pagina e le tecnologie assistive annunceranno il titolo della pagina. Queste cose accadono ogni volta che navighi verso una nuova pagina.

Con il routing lato client, il tuo browser non carica nuove pagine web, quindi non sa che dovrebbe regolare automaticamente il focus o annunciare un nuovo titolo di pagina. Gli autori dei framework hanno dedicato enormi quantità di tempo e lavoro per scrivere JavaScript che ricrea queste funzionalità e, anche allora, nessun framework lo ha fatto perfettamente.

Il vantaggio è che dovresti considerare l'accessibilità sin dall'inizio di _ogni_ progetto web, ma tieni presente che le basi di codice astratte che utilizzano i framework hanno maggiori probabilità di soffrire di importanti problemi di accessibilità se non lo fai.

## Come scegliere un framework

Ognuno dei framework discussi in questo modulo adotta approcci diversi allo sviluppo di applicazioni web. Ognuno migliora o cambia regolarmente e ciascuno ha i propri pro e contro. Scegliere il framework giusto è un processo che dipende dal team e dal progetto, e dovresti fare le tue ricerche per scoprire quello che si adatta meglio alle tue esigenze. Detto ciò, abbiamo identificato alcune domande che puoi porre per ricercare le tue opzioni in modo più efficace:

1. Quali browser supporta il framework?
2. Quali linguaggi specifici del dominio utilizza il framework?
3. Il framework ha una forte comunità e buone documentazioni (e altro supporto) disponibili?

La tabella in questa sezione fornisce un riepilogo conciso dell'attuale _supporto dei browser_ offerto da ciascun framework, oltre ai **linguaggi specifici del dominio** con i quali può essere utilizzato.

In generale, i {{Glossary("DSL/Domain_specific_language", "linguaggi specifici del dominio (DSLs)")}} sono linguaggi di programmazione rilevanti in aree specifiche dello sviluppo software. Nel contesto dei framework, i DSL sono varianti di JavaScript o HTML che semplificano lo sviluppo con quel framework. Fondamentalmente, nessuno dei framework _richiede_ che uno sviluppatore usi un DSL specifico, ma sono stati quasi tutti progettati con un DSL specifico in mente. Scegliere di non utilizzare il DSL preferito di un framework significherà perdere funzionalità che altrimenti migliorerebbero la tua esperienza di sviluppo.

Dovresti considerare seriamente la matrice di supporto e i DSL di un framework quando fai una scelta per un nuovo progetto. Un supporto del browser non allineato può essere un ostacolo per i tuoi utenti; un supporto DSL non allineato può essere un ostacolo per te e i tuoi compagni di squadra.

| Framework | Supporto browser                     | DSL preferito | DSL supportati         | Citazione                                                                        |
| --------- | ----------------------------------- | ------------- | ---------------------- | ------------------------------------------------------------------------------- |
| Angular   | Moderno                             | TypeScript    | Basato su HTML; TypeScript | [documentazione ufficiale](https://angular.dev/guide/browser-support)                      |
| React     | Moderno                             | JSX           | JSX; TypeScript        | [documentazione ufficiale](https://react.dev/reference/react-dom/client#browser-support)   |
| Vue       | Moderno (IE9+ in Vue 2)             | Basato su HTML| Basato su HTML, JSX, Pug   | [documentazione ufficiale](https://cli.vuejs.org/guide/browser-compatibility.html)         |
| Ember     | Moderno (IE9+ in versione Ember 2.18)| Handlebars    | Handlebars, TypeScript | [documentazione ufficiale](https://guides.emberjs.com/v3.3.0/templates/handlebars-basics/) |

> [!NOTE]
> I DSL che abbiamo descritto come "basati su HTML" non hanno nomi ufficiali. Non sono realmente veri DSL, ma sono HTML non standard, quindi crediamo che valga la pena sottolinearli.

### Il framework ha una forte comunità?

Questo è forse il parametro più difficile da misurare perché la dimensione della comunità non si traduce direttamente in numeri facilmente accessibili. Puoi controllare il numero di stelle GitHub di un progetto o i download settimanali di npm per farti un'idea della sua popolarità, ma a volte la cosa migliore da fare è cercare su alcuni forum o parlare con altri sviluppatori. Non si tratta solo delle dimensioni della comunità, ma anche di quanto sia accogliente e inclusiva, e di quanto buona sia la documentazione disponibile.

### Opinioni sul web

Non prendere solo la nostra parola in merito: ci sono discussioni su tutto il web. La Wikimedia Foundation ha recentemente deciso di utilizzare Vue per il suo front-end e ha pubblicato una [richiesta di commenti (RFC) sull'adozione del framework](https://phabricator.wikimedia.org/T241180). Eric Gardner, l'autore dell'RFC, ha preso del tempo per delineare le esigenze del progetto Wikimedia e perché certi framework fossero buone scelte per il team. Questa RFC serve come un grande esempio del tipo di ricerca che dovresti fare da solo quando pianifichi di utilizzare un framework front-end.

Il [sondaggio State of JavaScript](https://stateofjs.com/) è una collezione utile di feedback degli sviluppatori JavaScript. Copre molti argomenti correlati a JavaScript, inclusi dati sia sull'uso dei framework che sul sentimento degli sviluppatori nei loro confronti. Attualmente, ci sono diversi anni di dati disponibili, permettendoti di avere un'idea della popolarità di un framework.

Il team di Vue ha [confrontato esaustivamente Vue con altri framework popolari](https://v2.vuejs.org/v2/guide/comparison.html). Potrebbe esserci un po' di bias in questo confronto (che sottolineano), ma è comunque una risorsa preziosa.

## Alternative ai framework lato client

Se stai cercando strumenti per accelerare il processo di sviluppo web e sai che il tuo progetto non richiederà JavaScript lato client intensivo, potresti optare per una delle tante altre soluzioni per costruire il web:

- Un sistema di gestione dei contenuti
- Rendering lato server
- Un generatore di siti statici

### Sistemi di gestione dei contenuti

I **sistemi di gestione dei contenuti** (**CMS**) sono strumenti che consentono a un utente di creare contenuti per il web senza scrivere direttamente codice. Sono una buona soluzione per progetti di grandi dimensioni, in particolare progetti che richiedono input da parte di scrittori di contenuti con capacità limitate di coding, o per programmatori che vogliono risparmiare tempo. Tuttavia, richiedono un tempo significativo per essere configurati, e utilizzare un CMS significa che rinunci almeno a una parte del controllo sul risultato finale del tuo sito web. Ad esempio: se il CMS scelto non crea contenuti accessibili per impostazione predefinita, spesso è difficile migliorare questo aspetto.

Alcuni CMS popolari includono [WordPress](https://wordpress.com/), [Joomla](https://www.joomla.org/) e [Drupal](https://new.drupal.org/).

### Rendering lato server

Il **Rendering lato server** (**SSR**) è un'architettura applicativa in cui è compito del _server_ effettuare il rendering di un'applicazione a pagina singola. Questo è l'opposto del _rendering lato client_, che è il modo più comune e semplice per costruire un'applicazione JavaScript. Il rendering lato server è più facile sul dispositivo del client perché stai soltanto inviando loro un file HTML renderizzato, ma può essere difficile da configurare rispetto a un'applicazione renderizzata lato client.

Tutti i framework trattati in questo modulo supportano il rendering lato server così come il rendering lato client. Dai un'occhiata a [Next.js](https://nextjs.org/) per React, [Nuxt](https://nuxt.com/) per Vue (sì, è confuso, e no, questi progetti non sono correlati!), [FastBoot](https://github.com/ember-fastboot/ember-cli-fastboot) per Ember, e [Angular Universal](https://angular.dev/guide/universal) per Angular.

> [!NOTE]
> Alcune soluzioni SSR sono scritte e mantenute dalla comunità, mentre alcune sono soluzioni "ufficiali" fornite dal manutentore del framework.

### Generatori di siti statici

I {{Glossary("SSG", "generatori di siti statici")}} sono programmi che generano dinamicamente tutte le pagine web di un sito multi-pagina — inclusi eventuali CSS o JavaScript rilevanti — in modo che possano essere pubblicati in qualsiasi numero di posti. L'host di pubblicazione potrebbe essere un ramo di pagine di GitHub, un'istanza Netlify, o qualsiasi server privato di tua scelta, ad esempio. Ci sono una serie di vantaggi in questo approccio, principalmente legati alle prestazioni (il dispositivo del tuo utente non sta costruendo la pagina con JavaScript; è già completa) e alla sicurezza (le pagine statiche hanno meno vettori di attacco). Questi siti possono comunque utilizzare JavaScript dove necessario, ma non ne sono _dipendenti_. I generatori di siti statici richiedono tempo per essere appresi, proprio come qualsiasi altro strumento, che può essere un ostacolo al tuo processo di sviluppo.

I siti statici possono avere quante pagine uniche vuoi. Proprio come i framework ti consentono di scrivere rapidamente applicazioni JavaScript lato client, i generatori di siti statici ti permettono di creare rapidamente file HTML che altrimenti avresti scritto singolarmente. Come i framework, i generatori di siti statici consentono agli sviluppatori di scrivere componenti che definiscono pezzi comuni delle tue pagine web e di comporre questi componenti insieme per creare una pagina finale. Nel contesto dei generatori di siti statici, questi componenti sono chiamati **template**. Le pagine web costruite dai generatori di siti statici possono persino ospitare applicazioni framework: se vuoi che una pagina specifica del tuo sito generato staticamente avvii un'applicazione React quando il tuo utente la visita, puoi farlo.

I generatori di siti statici esistono da molto tempo, e sono in costante ottimizzazione e innovazione. Esistono molte scelte, tra cui [Astro](https://astro.build/), [Eleventy](https://www.11ty.dev/), [Hugo](https://gohugo.io/), [Jekyll](https://jekyllrb.com/), e [Gatsby](https://www.gatsbyjs.com/), che costruiscono su vari stack tecnologici e offrono funzionalità distintive. Altre opzioni, come [Docusaurus](https://docusaurus.io/) e [VitePress](https://vitepress.dev/), utilizzano framework lato client invece di template, ma generano file statici ugualmente ottimizzati.

Se desideri saperne di più sui generatori di siti statici nel complesso, dai un'occhiata alla [Guida per principianti a Eleventy di Tatiana Mac](https://www.tatianamac.com/posts/beginner-eleventy-tutorial-parti/). Nel primo articolo della serie, spiegano cosa è un generatore di siti statici e come si relazione ad altri mezzi di pubblicazione di contenuti web.

## Sommario

E questo ci porta alla fine della nostra introduzione ai framework — non abbiamo ancora insegnato alcun codice, ma speriamo di averti fornito un contesto utile sul perché useresti i framework in primo luogo e come sceglierne uno, e farti sentire entusiasta di imparare di più e di iniziare a sperimentare!

Il nostro prossimo articolo scende a un livello inferiore, esaminando i tipi specifici di funzionalità che i framework tendono ad offrire e perché funzionano così.

{{NextMenu("Learn_web_development/Core/Frameworks_libraries/Main_features", "Learn_web_development/Core/Frameworks_libraries")}}
