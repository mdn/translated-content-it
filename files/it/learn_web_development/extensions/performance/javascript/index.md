---
title: Ottimizzazione delle prestazioni di JavaScript
short-title: JavaScript performante
slug: Learn_web_development/Extensions/Performance/JavaScript
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/video", "Learn_web_development/Extensions/Performance/HTML", "Learn_web_development/Extensions/Performance")}}

È molto importante considerare come viene utilizzato JavaScript sui siti web e pensare a come mitigare eventuali problemi di prestazioni che potrebbe causare. Mentre immagini e video rappresentano oltre il 70% dei byte scaricati per il sito web medio, byte per byte, JavaScript ha un potenziale maggiore di impatto negativo sulle prestazioni: può influenzare significativamente i tempi di download, le prestazioni di rendering e l'utilizzo della CPU e della batteria. Questo articolo introduce suggerimenti e tecniche per ottimizzare JavaScript per migliorare le prestazioni del proprio sito web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        >, e conoscenza di base delle
        <a href="/it/docs/Learn_web_development/Getting_started/Your_first_website"
          >tecnologie web lato client</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi:</th>
      <td>
        Imparare gli effetti di JavaScript sulle prestazioni web
        e come mitigare o risolvere problemi correlati.
      </td>
    </tr>
  </tbody>
</table>

## Ottimizzare o non ottimizzare

La prima domanda a cui dovrebbe rispondere prima di iniziare a ottimizzare il codice è "cosa devo ottimizzare?". Alcuni dei suggerimenti e delle tecniche discussi di seguito sono buone pratiche che saranno utili a quasi tutti i progetti web, mentre alcuni sono necessari solo in determinate situazioni. Cercare di applicare tutte queste tecniche ovunque è probabilmente inutile e potrebbe essere una perdita di tempo. Dovrebbe capire quali ottimizzazioni delle prestazioni sono effettivamente necessarie in ciascun progetto.

Per fare ciò, è necessario [misurare le prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance) del suo sito. Come mostrato nel link precedente, ci sono diversi modi per misurare le prestazioni, alcuni dei quali coinvolgono sofisticate [API di prestazioni](/it/docs/Web/API/Performance_API). Tuttavia, il modo migliore per iniziare è imparare a usare strumenti come gli strumenti [di rete](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#network_monitor_tools) e [di prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#performance_monitor_tools) integrati nel browser, per vedere quali parti del caricamento della pagina richiedono molto tempo e necessitano di essere ottimizzate.

## Ottimizzazione dei download di JavaScript

Il JavaScript più performante, e meno bloccante, è quello che non si usa affatto. Dovrebbe usare il meno JavaScript possibile. Alcuni suggerimenti da tenere a mente:

- **Non ha sempre bisogno di un framework**: Potrebbe essere abituato a usare un [framework JavaScript](/it/docs/Learn_web_development/Core/Frameworks_libraries). Se ha esperienza e si sente sicuro di utilizzare questo framework, e le piacciono tutti gli strumenti che fornisce, allora potrebbe essere il suo strumento preferito per costruire la maggior parte dei progetti. Tuttavia, i framework sono pesanti in termini di JavaScript. Se sta creando un'esperienza piuttosto statica con pochi requisiti JavaScript, probabilmente non ha bisogno di quel framework. Potrebbe essere in grado di implementare ciò di cui ha bisogno utilizzando poche righe di JavaScript standard.
- **Valuti una soluzione più semplice**: Potrebbe avere una soluzione appariscente e interessante da implementare, ma consideri se i suoi utenti apprezzeranno davvero. Preferirebbero qualcosa di più semplice?
- **Rimuova il codice inutilizzato:** Potrebbe sembrare ovvio, ma è sorprendente quanti sviluppatori dimenticano di pulire le funzionalità inutilizzate aggiunte durante il processo di sviluppo. Deve essere attento e deliberato su ciò che viene aggiunto e rimosso. Tutti gli script vengono analizzati, indipendentemente dal fatto che vengano utilizzati o meno; quindi, un guadagno rapido per velocizzare i download sarebbe eliminare qualsiasi funzionalità non utilizzata. Consideri anche che spesso userà solo una piccola quantità della funzionalità disponibile in un framework. È possibile creare una build personalizzata del framework che contenga solo la parte di cui ha bisogno?
- **Consideri le funzionalità integrate nel browser**: Potrebbe essere che possa utilizzare una funzionalità che il browser ha già, piuttosto che crearne una propria tramite JavaScript. Ad esempio:
  - Utilizzi la [validazione dei moduli lato client integrata](/it/docs/Learn_web_development/Extensions/Forms/Form_validation#using_built-in_form_validation).
  - Utilizzi il lettore {{htmlelement("video")}} del browser.
  - Utilizzi le [animazioni CSS](/it/docs/Web/CSS/CSS_animations/Using_CSS_animations) invece di una libreria di animazioni JavaScript (vedere anche [Gestione delle animazioni](#gestione_delle_animazioni_javascript)).

Dovrebbe anche suddividere il suo JavaScript in più file che rappresentano parti critiche e non critiche. I [moduli JavaScript](/it/docs/Web/JavaScript/Guide/Modules) consentono di farlo in modo più efficiente rispetto all'utilizzo di semplici file JavaScript esterni separati.

Poi può ottimizzare questi file più piccoli. La {{Glossary("Minification", "minificazione")}} riduce il numero di caratteri nel suo file, riducendo così il numero di byte o il peso del suo JavaScript. Il {{Glossary("Gzip_compression", "Gzipping")}} comprime ulteriormente il file e dovrebbe essere utilizzato anche se non minimizza il suo codice. {{Glossary("Brotli_compression", "Brotli")}} è simile a Gzip, ma generalmente supera le prestazioni di compressione di Gzip.

Può suddividere e ottimizzare il codice manualmente, ma spesso un modulo bundler come [webpack](https://webpack.js.org/) farà un lavoro migliore.

## Gestione dell'analisi e dell'esecuzione

Prima di esaminare i suggerimenti contenuti in questa sezione, è importante parlare di _dove_ nel processo di rendering della pagina da parte del browser viene gestito JavaScript. Quando viene caricata una pagina web:

1. L'HTML viene generalmente analizzato per primo, nell'ordine in cui appare nella pagina.
2. Ogni volta che viene trovato un CSS, questo viene analizzato per comprendere gli stili che devono essere applicati alla pagina. Durante questo tempo, risorse collegate come immagini e font web iniziano a essere scaricate.
3. Ogni volta che viene trovato JavaScript, il browser lo analizza, valuta e lo esegue nella pagina.
4. Poco dopo, il browser determina come ogni elemento HTML debba essere stilizzato, dato il CSS applicato.
5. Il risultato stilizzato viene quindi dipinto sullo schermo.

> [!NOTE]
> Questa è una descrizione molto semplificata di ciò che accade, ma offre comunque un'idea.

Il passo chiave qui è il Passo 3. Per impostazione predefinita, l'analisi e l'esecuzione di JavaScript bloccano il rendering. Ciò significa che il browser blocca l'analisi di qualsiasi HTML che appare dopo il JavaScript trovato, fino a quando lo script non è stato gestito. Di conseguenza, sono bloccati anche gli stili e la pittura. Ciò significa che deve pensare attentamente non solo a ciò che sta scaricando, ma anche a quando e come quel codice viene eseguito.

Le prossime sezioni forniscono tecniche utili per ottimizzare l'analisi e l'esecuzione del suo JavaScript.

## Caricare le risorse critiche il prima possibile

Se uno script è davvero importante e si preoccupa che stia influenzando le prestazioni non essendo caricato abbastanza velocemente, può caricarlo all'interno dell'{{htmlelement("head")}} del documento:

```html
<head>
  ...
  <script src="main.js"></script>
  ...
</head>
```

Questo funziona bene, ma blocca il rendering. Una strategia migliore è utilizzare [`rel="preload"`](/it/docs/Web/HTML/Reference/Attributes/rel/preload) per creare un preloader per JavaScript critico:

```html
<head>
  ...
  <!-- Preload a JavaScript file -->
  <link rel="preload" href="important-js.js" as="script" />
  <!-- Preload a JavaScript module -->
  <link rel="modulepreload" href="important-module.js" />
  ...
</head>
```

Il preload {{htmlelement("link")}} scarica JavaScript il prima possibile, senza bloccare il rendering. Può quindi utilizzarlo ovunque voglia nella sua pagina:

```html
<!-- Include this wherever makes sense -->
<script src="important-js.js"></script>
```

o all'interno del suo script, nel caso di un modulo JavaScript:

```js
import { someFunction } from "important-module.js";
```

> [!NOTE]
> Il preloading non garantisce che lo script sarà caricato al momento dell'inclusione, ma significa che inizierà a essere scaricato prima. Il tempo di blocco del rendering sarà comunque ridotto, anche se non completamente eliminato.

## Rimandare l'esecuzione di JavaScript non critico

D'altra parte, dovrebbe mirare a rimandare l'analisi e l'esecuzione di JavaScript non critico più avanti, quando è necessario. Caricarlo tutto in anticipo blocca inutilmente il rendering.

Innanzitutto, può aggiungere l'attributo `async` ai suoi elementi `<script>`:

```html
<head>
  ...
  <script async src="main.js"></script>
  ...
</head>
```

Questo fa sì che lo script venga scaricato in parallelo con l'analisi del DOM, quindi sarà pronto nello stesso momento e non bloccherà il rendering.

> [!NOTE]
> C'è un altro attributo, `defer`, che fa sì che lo script venga eseguito dopo che il documento è stato analizzato, ma prima di attivare l'evento [`DOMContentLoaded`](/it/docs/Web/API/Document/DOMContentLoaded_event). Questo ha un effetto simile a `async`.

Potrebbe anche semplicemente non caricare affatto JavaScript fino a quando non si verifica un evento che lo rende necessario. Questo potrebbe essere fatto tramite scripting DOM, ad esempio:

```js
const scriptElem = document.createElement("script");
scriptElem.src = "index.js";
scriptElem.addEventListener("load", () => {
  // Run a function contained within index.js once it has definitely loaded
  init();
});
document.head.append(scriptElem);
```

I moduli JavaScript possono essere caricati dinamicamente utilizzando la funzione {{jsxref("operators/import", "import()")}}:

```js
import("./modules/myModule.js").then((module) => {
  // Do something with the module
});
```

## Suddivisione delle attività lunghe

Quando il browser esegue il suo JavaScript, organizza lo script in attività che vengono eseguite in sequenza, come effettuare richieste di fetch, gestire interazioni e input degli utenti attraverso gestori di eventi, eseguire animazioni JavaScript, e così via.

La maggior parte di questo avviene nel thread principale, con eccezioni che includono JavaScript che gira in [Web Workers](/it/docs/Web/API/Web_Workers_API/Using_web_workers). Il thread principale può eseguire solo un'attività alla volta.

Quando un'unica attività impiega più di 50 ms per essere eseguita, è classificata come attività lunga. Se l'utente tenta di interagire con la pagina o è richiesta un'importante aggiornamento dell'interfaccia utente mentre un'attività lunga è in esecuzione, l'esperienza sarà influenzata. Una risposta prevista o un aggiornamento visivo ritarderanno, risultando in un'interfaccia utente che appare lenta o non reattiva.

Per mitigare questo problema, è necessario suddividere le attività lunghe in attività più piccole. Questo dà al browser più possibilità di eseguire operazioni vitali di gestione dell'interazione utente o aggiornamento del rendering dell'interfaccia utente: il browser può potenzialmente farlo tra ciascuna attività più piccola, anziché solo prima o dopo l'attività lunga. Nel suo JavaScript, potrebbe farlo suddividendo il codice in funzioni separate. Questo ha anche senso per diversi altri motivi, come la manutenzione più facile, il debug e la scrittura di test.

Ad esempio:

```js
function main() {
  a();
  b();
  c();
  d();
  e();
}
```

Tuttavia, questo tipo di struttura non aiuta con il blocco del thread principale. Poiché tutte le cinque funzioni vengono eseguite all'interno di una funzione principale, il browser le esegue tutte come una singola attività lunga.

Per gestire ciò, tendiamo a eseguire una "funzione yield" periodicamente per far sì che il codice _cede al thread principale_. Questo significa che il codice è suddiviso in più attività, tra le quali il browser ha l'opportunità di gestire compiti di alta priorità come l'aggiornamento dell'interfaccia utente. Un pattern comune per questa funzione utilizza [`setTimeout()`](/it/docs/Web/API/Window/setTimeout) per posticipare l'esecuzione in un'attività separata:

```js
function yield() {
  return new Promise((resolve) => {
    setTimeout(resolve, 0);
  });
}
```

Questo può essere utilizzato all'interno di un pattern di esecuzione di compiti in questo modo, per cedere al thread principale dopo che ogni attività è stata eseguita:

```js
async function main() {
  // Create an array of functions to run
  const tasks = [a, b, c, d, e];

  // Loop over the tasks
  while (tasks.length > 0) {
    // Shift the first task off the tasks array
    const task = tasks.shift();

    // Run the task
    task();

    // Yield to the main thread
    await yield();
  }
}
```

Per migliorare ulteriormente questo, possiamo utilizzare [`Scheduler.yield()`](/it/docs/Web/API/Scheduler/yield) dove disponibile per permettere a questo codice di continuare a eseguire prima di altri compiti meno critici nella coda:

```js
function yield() {
  // Use scheduler.yield() if available
  if ("scheduler" in window && "yield" in scheduler) {
    return scheduler.yield();
  }

  // Fall back to setTimeout:
  return new Promise((resolve) => {
    setTimeout(resolve, 0);
  });
}
```

## Gestione delle animazioni JavaScript

Le animazioni possono migliorare le prestazioni percepite, rendendo le interfacce più scattanti e facendo sentire agli utenti che si stia facendo progressi mentre aspettano il caricamento di una pagina (ad esempio, i spinner di caricamento). Tuttavia, animazioni più grandi e un numero maggiore di animazioni richiederanno naturalmente più potenza di elaborazione per essere gestite, il che può degradare le prestazioni.

Il consiglio più ovvio riguardo alle animazioni è usare meno animazioni: eliminare qualsiasi animazione non essenziale, o considerare di dare agli utenti una preferenza che possano impostare per disattivare le animazioni, ad esempio se stanno utilizzando un dispositivo a bassa potenza o un dispositivo mobile con batteria limitata.

Per le animazioni DOM essenziali, si consiglia di utilizzare le [animazioni CSS](/it/docs/Web/CSS/CSS_animations/Using_CSS_animations) dove possibile, piuttosto che animazioni JavaScript (l'[API delle Animazioni Web](/it/docs/Web/API/Web_Animations_API) fornisce un modo per collegarsi direttamente alle animazioni CSS usando JavaScript). Utilizzare il browser per eseguire direttamente le animazioni DOM piuttosto che manipolare gli stili in linea via JavaScript è molto più veloce ed efficiente. Vedere anche [Ottimizzazione delle prestazioni CSS > Gestione delle animazioni](/it/docs/Learn_web_development/Extensions/Performance/CSS#handling_animations).

Per le animazioni che non possono essere gestite in JavaScript, ad esempio, animando un HTML {{htmlelement("canvas")}}, si consiglia di utilizzare [`Window.requestAnimationFrame()`](/it/docs/Web/API/Window/requestAnimationFrame) piuttosto che opzioni più vecchie come [`Window.setInterval()`](/it/docs/Web/API/Window/setInterval). Il metodo `requestAnimationFrame()` è specificamente progettato per gestire i fotogrammi di animazione in modo efficiente e coerente, per un'esperienza utente uniforme. Il pattern di base è il seguente:

```js
function loop() {
  // Clear the canvas before drawing the next frame of the animation
  ctx.fillStyle = "rgb(0 0 0 / 25%)";
  ctx.fillRect(0, 0, width, height);

  // Draw objects on the canvas and update their positioning data
  // ready for the next frame
  for (const ball of balls) {
    ball.draw();
    ball.update();
  }

  // Call requestAnimationFrame to run the loop() function again
  // at the right time to keep the animation smooth
  requestAnimationFrame(loop);
}

// Call the loop() function once to set the animation running
loop();
```

Può trovare una bella introduzione alle animazioni canvas in [Grafica tramite disegno > Animazioni](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics#animations), e un esempio più approfondito in [Pratica di costruzione di oggetti](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice). Può anche trovare un set completo di tutorial su canvas in [Tutorial su Canvas](/it/docs/Web/API/Canvas_API/Tutorial).

## Ottimizzazione delle prestazioni degli eventi

Gli eventi possono essere costosi da tracciare e gestire per il browser, specialmente quando si esegue un evento in modo continuo. Ad esempio, potrebbe tracciare la posizione del mouse usando l'evento [`mousemove`](/it/docs/Web/API/Element/mousemove_event) per verificare se è ancora all'interno di un'area specifica della pagina:

```js
function handleMouseMove() {
  // Do stuff while mouse pointer is inside elem
}

elem.addEventListener("mousemove", handleMouseMove);
```

Potrebbe stare eseguendo un gioco `<canvas>` nella sua pagina. Mentre il mouse è all'interno del canvas, vorrà costantemente controllare il movimento del mouse e la posizione del cursore e aggiornare lo stato del gioco, inclusi il punteggio, il tempo, la posizione di tutti gli sprite, le informazioni di rilevamento collisioni, ecc. Una volta che il gioco è finito, non avrà più bisogno di fare tutto ciò, e in effetti sarà uno spreco di potenza di elaborazione continuare ad ascoltare quell'evento.

È quindi una buona idea rimuovere i listener di eventi che non sono più necessari. Questo può essere fatto usando [`removeEventListener()`](/it/docs/Web/API/EventTarget/removeEventListener):

```js
elem.removeEventListener("mousemove", handleMouseMove);
```

Un altro suggerimento è utilizzare la delega degli eventi dove possibile. Quando ha del codice da eseguire in risposta all'interazione dell'utente con ciascuno di un grande numero di elementi figli, può impostare un listener di eventi sul loro genitore. Gli eventi generati su qualsiasi elemento figlio risaliranno al loro genitore, quindi non è necessario impostare il listener di eventi su ciascun figlio individualmente. Meno listener di eventi da tenere traccia significa migliori prestazioni.

Vedere [Delega degli eventi](/it/docs/Learn_web_development/Core/Scripting/Event_bubbling#event_delegation) per ulteriori dettagli e un esempio utile.

## Suggerimenti per scrivere codice più efficiente

Ci sono diverse pratiche migliori generali che faranno eseguire il suo codice in modo più efficiente.

- **Ridurre la manipolazione del DOM**: L'accesso e l'aggiornamento del DOM è computazionalmente costoso, quindi dovrebbe minimizzare la quantità che il suo JavaScript esegue, specialmente quando si eseguono animazioni DOM costanti (vedere [Gestione delle animazioni JavaScript](#gestione_delle_animazioni_javascript) sopra).
- **Raggruppare i cambiamenti del DOM**: Per le modifiche essenziali al DOM, dovrebbe raggrupparle in gruppi che vengono eseguiti insieme, piuttosto che inviare ciascun cambiamento individuale man mano che si verifica. Questo può ridurre la quantità di lavoro che il browser sta effettivamente facendo, ma anche migliorare le prestazioni percepite. Può fare sembrare l'interfaccia utente più fluida gestire diversi aggiornamenti in un solo colpo, piuttosto che fare costantemente piccoli aggiornamenti. Un suggerimento utile qui è: quando ha un grande blocco di HTML da aggiungere alla pagina, costruisca prima l'intero frammento (tipicamente all'interno di un [`DocumentFragment`](/it/docs/Web/API/DocumentFragment)) e poi lo aggiunga tutto al DOM in un colpo solo, piuttosto che aggiungere ciascun elemento separatamente.
- **Semplificare il suo HTML**: Più semplice è l'albero DOM, più velocemente può essere accesso e manipolato con JavaScript. Rifletta attentamente su ciò di cui la sua interfaccia utente ha bisogno e rimuova il superfluo.
- **Ridurre il numero di cicli di codice**: I cicli sono costosi, quindi riduca l'uso di cicli nel suo codice ove possibile. Nei casi in cui i cicli sono inevitabili:

  - Eviti di eseguire il ciclo completo quando non è necessario, utilizzando le istruzioni {{jsxref("Statements/break", "break")}} o {{jsxref("Statements/continue", "continue")}} come appropriato. Ad esempio, se sta cercando nomi specifici in un array, dovrebbe uscire dal ciclo una volta che il nome è stato trovato; non c'è bisogno di eseguire ulteriori iterazioni del ciclo:

    ```js
    function processGroup(array) {
      const toFind = "Bob";
      for (let i = 0; i < array.length - 1; i++) {
        if (array[i] === toFind) {
          processMatchingArray(array);
          break;
        }
      }
    }
    ```

  - Esegua lavori che sono necessari solo una volta al di fuori del ciclo. Questo può sembrare un po' ovvio, ma è facile da trascurare. Consideri il seguente frammento, che esegue una fetch di un oggetto JSON contenente dati da elaborare in qualche modo. In questo caso, l'operazione [`fetch()`](/it/docs/Web/API/Window/fetch) viene eseguita su ogni iterazione del ciclo, il che è uno spreco di potenza di calcolo. L'operazione di fetch, che non dipende da `i`, potrebbe essere spostata al di fuori del ciclo, quindi viene eseguita solo una volta.

    ```js
    async function returnResults(number) {
      for (let i = 0; i < number; i++) {
        const response = await fetch(`/results?number=${number}`);
        const results = await response.json();
        processResult(results[i]);
      }
    }
    ```

- **Eseguire calcoli fuori dal thread principale**: In precedenza abbiamo parlato di come JavaScript generalmente esegue le attività sul thread principale e di come le operazioni lunghe possono bloccare il thread principale, potenzialmente portando a cattive prestazioni dell'interfaccia utente. Abbiamo anche mostrato come suddividere compiti lunghi in compiti più piccoli, mitigando questo problema. Un altro modo per gestire tali problemi è spostare i compiti fuori dal thread principale del tutto. Ci sono alcuni modi per raggiungere questo:

  - Usare codice asincrono: Il [JavaScript asincrono](/it/docs/Learn_web_development/Extensions/Async_JS/Introducing) è essenzialmente JavaScript che non blocca il thread principale. Le API asincrone tendono a gestire operazioni come il recupero di risorse dalla rete, l'accesso a un file sul file system locale o l'apertura di un flusso alla webcam dell'utente. Poiché queste operazioni potrebbero richiedere molto tempo, sarebbe un male bloccare semplicemente il thread principale in attesa che si completino. Invece, il browser esegue quelle funzioni, mantiene il thread principale in esecuzione per il codice successivo, e quelle funzioni restituiranno risultati una volta disponibili _a un certo punto in futuro_. Le moderne API asincrone sono basate su {{jsxref("Promise")}}, una funzionalità del linguaggio JavaScript progettata per gestire le operazioni asincrone. È possibile [scrivere le proprie funzioni basate su Promise](/it/docs/Learn_web_development/Extensions/Async_JS/Implementing_a_promise-based_API) se ha funzionalità che trarrebbe beneficio dall'essere eseguite asincronicamente.
  - Eseguire calcoli nei web worker: I [Web Worker](/it/docs/Web/API/Web_Workers_API/Using_web_workers) sono un meccanismo che permette di aprire un thread separato per eseguire un pezzo di JavaScript, in modo da non bloccare il thread principale. I worker hanno alcune restrizioni importanti, la più grande delle quali è che non può effettuare scripting DOM all'interno di un worker. Può fare quasi tutto il resto, e i worker possono inviare e ricevere messaggi da e verso il thread principale. Il caso d'uso principale per i worker è se ha molti calcoli da fare e non vuole che blocchino il thread principale. Effettui quei calcoli in un worker, attenda il risultato e lo invii al thread principale quando è pronto.
  - **Usare WebGPU**: [WebGPU](/it/docs/Web/API/WebGPU_API) è un'API del browser che consente agli sviluppatori web di utilizzare la GPU (Graphics Processing Unit) del sistema sottostante per eseguire calcoli a prestazioni elevate e disegnare immagini complesse che possono essere renderizzate nel browser. È abbastanza complesso, ma può fornire benefici in termini di prestazioni ancora migliori dei web worker.

## Vedere anche

- [Ottimizza attività lunghe](https://web.dev/articles/optimize-long-tasks) su web.dev (2022)
- [Tutorial su Canvas](/it/docs/Web/API/Canvas_API/Tutorial)

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/video", "Learn_web_development/Extensions/Performance/HTML", "Learn_web_development/Extensions/Performance")}}
