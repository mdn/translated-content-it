---
title: Ottimizzazione delle prestazioni JavaScript
short-title: JavaScript performante
slug: Learn_web_development/Extensions/Performance/JavaScript
l10n:
  sourceCommit: 690498c3dbaebcf8b9a21220fbb23d192a30a225
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/video", "Learn_web_development/Extensions/Performance/HTML", "Learn_web_development/Extensions/Performance")}}

È molto importante considerare come viene utilizzato JavaScript nei siti web e pensare a come mitigare gli eventuali problemi di prestazioni che potrebbe causare. Sebbene immagini e video rappresentino oltre il 70% dei byte scaricati per il sito web medio, byte per byte JavaScript ha un maggiore potenziale di impatto negativo sulle prestazioni: può influire significativamente sui tempi di download, sulle prestazioni di rendering e sull'utilizzo di CPU e batteria. Questo articolo introduce suggerimenti e tecniche per ottimizzare JavaScript e migliorare le prestazioni del sito web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        > e conoscenza di base delle
        <a href="/it/docs/Learn_web_development/Getting_started/Your_first_website"
          >tecnologie web lato client</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi:</th>
      <td>
        Apprendere gli effetti di JavaScript sulle prestazioni web
        e come mitigare o risolvere i problemi correlati.
      </td>
    </tr>
  </tbody>
</table>

## Ottimizzare o non ottimizzare

La prima domanda a cui rispondere prima di iniziare a ottimizzare il codice è: "che cosa è necessario ottimizzare?". Alcuni dei suggerimenti e delle tecniche illustrati di seguito sono buone pratiche utili per quasi tutti i progetti web, mentre altri sono necessari solo in determinate situazioni. Cercare di applicare tutte queste tecniche ovunque è probabilmente superfluo e potrebbe essere una perdita di tempo. È opportuno capire quali ottimizzazioni delle prestazioni siano effettivamente necessarie in ciascun progetto.

Per farlo, è necessario [misurare le prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance) del sito. Come mostra il collegamento precedente, esistono diversi modi per misurare le prestazioni, alcuni dei quali coinvolgono sofisticate [API delle prestazioni](/it/docs/Web/API/Performance_API). Il modo migliore per iniziare, tuttavia, è imparare a usare strumenti come quelli integrati nel browser per la [rete](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#network_monitor_tools) e le [prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#performance_monitor_tools), per vedere quali parti del caricamento della pagina richiedono molto tempo e necessitano di ottimizzazione.

## Ottimizzazione dei download JavaScript

Il JavaScript più performante e meno bloccante che è possibile usare è quello che non viene usato affatto. È opportuno usare la minor quantità possibile di JavaScript. Alcuni suggerimenti da tenere presenti:

- **Non è sempre necessario un framework**: Potrebbe esserci familiarità con l'uso di un [framework JavaScript](/it/docs/Learn_web_development/Core/Frameworks_libraries). Se si ha esperienza e sicurezza nell'uso di questo framework, e piacciono tutti gli strumenti che fornisce, potrebbe essere lo strumento di riferimento per realizzare la maggior parte dei progetti. Tuttavia, i framework fanno un uso intensivo di JavaScript. Se si sta creando un'esperienza piuttosto statica con pochi requisiti JavaScript, probabilmente quel framework non è necessario. Potrebbe essere possibile implementare ciò che serve con poche righe di JavaScript standard.
- **Considerare una soluzione più semplice**: Potrebbe esserci una soluzione appariscente e interessante da implementare, ma è opportuno considerare se gli utenti la apprezzeranno. Preferirebbero qualcosa di più semplice?
- **Rimuovere il codice inutilizzato:** Può sembrare ovvio, ma è sorprendente quanti sviluppatori dimentichino di ripulire le funzionalità inutilizzate aggiunte durante il processo di sviluppo. È necessario prestare attenzione e agire deliberatamente su ciò che viene aggiunto e rimosso. Tutti gli script vengono analizzati, indipendentemente dal fatto che siano usati o meno; pertanto, un rapido miglioramento per velocizzare i download consiste nell'eliminare ogni funzionalità non utilizzata. Considerare inoltre che spesso viene usata solo una piccola parte delle funzionalità disponibili in un framework. È possibile creare una build personalizzata del framework che contenga solo la parte necessaria?
- **Considerare le funzionalità integrate nel browser**: Potrebbe essere possibile usare una funzionalità già presente nel browser, invece di crearne una tramite JavaScript. Per esempio:
  - Usare la [convalida dei moduli lato client integrata](/it/docs/Learn_web_development/Extensions/Forms/Form_validation#using_built-in_form_validation).
  - Usare il lettore {{htmlelement("video")}} del browser.
  - Usare le [animazioni CSS](/it/docs/Web/CSS/Guides/Animations/Using) invece di una libreria di animazioni JavaScript (vedere anche [Gestione delle animazioni](#gestione_delle_animazioni_javascript)).

È inoltre opportuno dividere JavaScript in più file che rappresentino parti critiche e non critiche. I [moduli JavaScript](/it/docs/Web/JavaScript/Guide/Modules) consentono di farlo in modo più efficiente rispetto al semplice uso di file JavaScript esterni separati.

Questi file più piccoli possono quindi essere ottimizzati. La {{Glossary("Minification", "minificazione")}} riduce il numero di caratteri nel file, diminuendo così il numero di byte o il peso del JavaScript. La {{Glossary("Gzip_compression", "compressione Gzip")}} comprime ulteriormente il file e dovrebbe essere usata anche se il codice non viene minificato. {{Glossary("Brotli_compression", "Brotli")}} è simile a Gzip, ma in genere supera le prestazioni della compressione Gzip.

È possibile dividere e ottimizzare il codice manualmente, ma spesso un bundler di moduli come [webpack](https://webpack.js.org/) svolgerà un lavoro migliore.

## Gestione dell'analisi e dell'esecuzione

Prima di esaminare i suggerimenti contenuti in questa sezione, è importante parlare di _dove_ JavaScript viene gestito nel processo di rendering della pagina da parte del browser. Quando una pagina web viene caricata:

1. L'HTML viene generalmente analizzato per primo, nell'ordine in cui appare nella pagina.
2. Ogni volta che viene incontrato CSS, questo viene analizzato per comprendere gli stili da applicare alla pagina. Nel frattempo, le risorse collegate come immagini e font web iniziano a essere recuperate.
3. Ogni volta che viene incontrato JavaScript, il browser lo analizza, lo valuta e lo esegue sulla pagina.
4. Poco dopo, il browser determina quale stile applicare a ogni elemento HTML, in base al CSS applicato.
5. Il risultato stilizzato viene quindi disegnato sullo schermo.

> [!NOTE]
> Questa è una descrizione molto semplificata di ciò che accade, ma fornisce comunque un'idea.

Il passaggio chiave è il passaggio 3. Per impostazione predefinita, l'analisi e l'esecuzione di JavaScript bloccano il rendering. Ciò significa che il browser blocca l'analisi di qualsiasi HTML che appaia dopo l'incontro con JavaScript, fino a quando lo script non è stato gestito. Di conseguenza, vengono bloccati anche lo styling e il disegno. Ciò significa che è necessario riflettere attentamente non solo su ciò che viene scaricato, ma anche su quando e come il codice viene eseguito.

Le prossime sezioni forniscono tecniche utili per ottimizzare l'analisi e l'esecuzione di JavaScript.

## Caricamento delle risorse critiche il prima possibile

Se uno script è davvero importante e si teme che stia influendo sulle prestazioni perché non viene caricato abbastanza rapidamente, è possibile caricarlo all'interno di {{htmlelement("head")}} del documento:

```html
<head>
  ...
  <script src="main.js"></script>
  ...
</head>
```

Questo funziona correttamente, ma blocca il rendering. Una strategia migliore consiste nell'usare [`rel="preload"`](/it/docs/Web/HTML/Reference/Attributes/rel/preload) per creare un precaricatore per JavaScript critico:

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

Il {{htmlelement("link")}} di preload recupera JavaScript il prima possibile, senza bloccare il rendering. Può quindi essere usato ovunque nella pagina:

```html
<!-- Include this wherever makes sense -->
<script src="important-js.js"></script>
```

oppure all'interno dello script, nel caso di un modulo JavaScript:

```js
import { someFunction } from "important-module.js";
```

> [!NOTE]
> Il precaricamento non garantisce che lo script venga caricato nel momento in cui viene incluso, ma significa che inizierà a essere scaricato prima. Il tempo di blocco del rendering verrà comunque ridotto, anche se non completamente eliminato.

## Rinviare l'esecuzione di JavaScript non critico

D'altra parte, è opportuno rinviare l'analisi e l'esecuzione di JavaScript non critico a un momento successivo, quando sarà necessario. Caricarlo tutto in anticipo blocca inutilmente il rendering.

Prima di tutto, è possibile aggiungere l'attributo `async` agli elementi `<script>`:

```html
<head>
  ...
  <script async src="main.js"></script>
  ...
</head>
```

Questo fa sì che lo script venga recuperato in parallelo con l'analisi del DOM, quindi sarà pronto contemporaneamente e non bloccherà il rendering.

> [!NOTE]
> Esiste un altro attributo, `defer`, che fa sì che lo script venga eseguito dopo che il documento è stato analizzato, ma prima dell'attivazione dell'evento [`DOMContentLoaded`](/it/docs/Web/API/Document/DOMContentLoaded_event). Ha un effetto simile a `async`.

Si potrebbe anche non caricare affatto JavaScript fino a quando non si verifica un evento che ne richiede l'uso. Questo può essere fatto, per esempio, tramite scripting DOM:

```js
const scriptElem = document.createElement("script");
scriptElem.src = "index.js";
scriptElem.addEventListener("load", () => {
  // Run a function contained within index.js once it has definitely loaded
  init();
});
document.head.append(scriptElem);
```

I moduli JavaScript possono essere caricati dinamicamente tramite la funzione {{jsxref("Operators/import", "import()")}}:

```js
import("./modules/myModule.js").then((module) => {
  // Do something with the module
});
```

## Suddivisione delle attività lunghe

Quando il browser esegue JavaScript, organizza lo script in attività eseguite in sequenza, come effettuare richieste fetch, gestire interazioni e input dell'utente tramite gestori di eventi, eseguire animazioni controllate da JavaScript e così via.

La maggior parte di questo avviene sul thread principale, con eccezioni che includono JavaScript eseguito nei [Web Workers](/it/docs/Web/API/Web_Workers_API/Using_web_workers). Il thread principale può eseguire una sola attività alla volta.

Quando una singola attività richiede più di 50 ms per essere eseguita, viene classificata come attività lunga. Se l'utente tenta di interagire con la pagina oppure viene richiesta un'importante modifica dell'interfaccia utente mentre è in esecuzione un'attività lunga, l'esperienza dell'utente ne risentirà. Una risposta o un aggiornamento visivo previsto verrà ritardato, dando l'impressione che l'interfaccia utente sia lenta o non reattiva.

Per mitigare questo problema, è necessario suddividere le attività lunghe in attività più piccole. Ciò offre al browser più opportunità di eseguire la gestione vitale dell'interazione utente o gli aggiornamenti di rendering dell'interfaccia utente: il browser può potenzialmente eseguirli tra ogni attività più piccola, anziché soltanto prima o dopo l'attività lunga. In JavaScript, questo può essere fatto dividendo il codice in funzioni separate. Ciò ha senso anche per diverse altre ragioni, come una manutenzione, un debug e la scrittura di test più semplici.

Per esempio:

```js
function main() {
  a();
  b();
  c();
  d();
  e();
}
```

Tuttavia, questo tipo di struttura non aiuta a evitare il blocco del thread principale. Poiché tutte e cinque le funzioni vengono eseguite all'interno di una funzione principale, il browser le esegue tutte come un'unica attività lunga.

Per gestire questo problema, si tende a eseguire periodicamente una funzione di "yield" per fare in modo che il codice _ceda il controllo al thread principale_. Questo significa che il codice viene suddiviso in più attività, tra l'esecuzione delle quali al browser viene data l'opportunità di gestire attività ad alta priorità, come l'aggiornamento dell'interfaccia utente. Un modello comune per questa funzione usa [`setTimeout()`](/it/docs/Web/API/Window/setTimeout) per posticipare l'esecuzione in un'attività separata:

```js
function yieldFunc() {
  return new Promise((resolve) => {
    setTimeout(resolve, 0);
  });
}
```

Questo può essere usato all'interno di un modello di esecuzione delle attività come segue, per cedere il controllo al thread principale dopo l'esecuzione di ogni attività:

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
    await yieldFunc();
  }
}
```

Per migliorare ulteriormente, è possibile usare [`Scheduler.yield()`](/it/docs/Web/API/Scheduler/yield) quando disponibile, per consentire al codice di continuare l'esecuzione prima di altre attività meno critiche nella coda:

```js
function yieldFunc() {
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

Le animazioni possono migliorare le prestazioni percepite, facendo apparire le interfacce più reattive e dando agli utenti la sensazione che ci siano progressi mentre attendono il caricamento di una pagina, per esempio con gli spinner di caricamento. Tuttavia, animazioni più grandi e un numero maggiore di animazioni richiederanno naturalmente una maggiore potenza di elaborazione, che può degradare le prestazioni.

Il consiglio più ovvio riguardo alle animazioni è usarne meno: eliminare quelle non essenziali oppure considerare di offrire agli utenti una preferenza per disattivare le animazioni, per esempio se usano un dispositivo a bassa potenza o un dispositivo mobile con batteria limitata.

Per le animazioni DOM essenziali, è consigliato usare le [animazioni CSS](/it/docs/Web/CSS/Guides/Animations/Using) dove possibile, anziché le animazioni JavaScript (la [Web Animations API](/it/docs/Web/API/Web_Animations_API) offre un modo per collegarsi direttamente alle animazioni CSS tramite JavaScript). Usare il browser per eseguire direttamente le animazioni DOM, anziché manipolare gli stili inline tramite JavaScript, è molto più veloce ed efficiente. Vedere anche [Ottimizzazione delle prestazioni CSS > Gestione delle animazioni](/it/docs/Learn_web_development/Extensions/Performance/CSS#handling_animations).

Per le animazioni che non possono essere gestite in JavaScript, per esempio l'animazione di un {{htmlelement("canvas")}} HTML, è consigliato usare [`Window.requestAnimationFrame()`](/it/docs/Web/API/Window/requestAnimationFrame) anziché opzioni più vecchie come [`Window.setInterval()`](/it/docs/Web/API/Window/setInterval). Il metodo `requestAnimationFrame()` è progettato appositamente per gestire i frame di animazione in modo efficiente e coerente, garantendo un'esperienza utente fluida. Il modello di base è simile a questo:

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

Una buona introduzione alle animazioni canvas è disponibile in [Disegnare grafica > Animazioni](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics#animations), mentre un esempio più approfondito è disponibile in [Esercitazione sulla creazione di oggetti](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice). È inoltre disponibile una serie completa di tutorial canvas in [Tutorial Canvas](/it/docs/Web/API/Canvas_API/Tutorial).

## Ottimizzazione delle prestazioni degli eventi

Gli eventi possono essere costosi da monitorare e gestire per il browser, specialmente quando un evento viene eseguito continuamente. Per esempio, si potrebbe monitorare la posizione del mouse usando l'evento [`mousemove`](/it/docs/Web/API/Element/mousemove_event) per verificare se si trova ancora all'interno di una certa area della pagina:

```js
function handleMouseMove() {
  // Do stuff while mouse pointer is inside elem
}

elem.addEventListener("mousemove", handleMouseMove);
```

Potrebbe essere in esecuzione un gioco `<canvas>` nella pagina. Mentre il mouse si trova all'interno del canvas, sarà necessario controllare costantemente il movimento del mouse e la posizione del cursore, aggiornando lo stato del gioco, inclusi il punteggio, il tempo, la posizione di tutti gli sprite, le informazioni di rilevamento delle collisioni e così via. Una volta terminato il gioco, tutto questo non sarà più necessario e, di fatto, continuare ad ascoltare quell'evento sarebbe uno spreco di potenza di elaborazione.

È quindi una buona idea rimuovere i listener di eventi che non sono più necessari. Questo può essere fatto usando [`removeEventListener()`](/it/docs/Web/API/EventTarget/removeEventListener):

```js
elem.removeEventListener("mousemove", handleMouseMove);
```

Un altro suggerimento consiste nell'usare la delega degli eventi ogni volta che sia possibile. Quando è presente del codice da eseguire in risposta all'interazione dell'utente con uno qualsiasi tra un gran numero di elementi figlio, è possibile impostare un listener di eventi sul relativo elemento genitore. Gli eventi attivati su qualsiasi elemento figlio risaliranno fino al genitore, quindi non è necessario impostare il listener di eventi su ogni figlio singolarmente. Meno listener di eventi da monitorare significa prestazioni migliori.

Vedere [Delega degli eventi](/it/docs/Learn_web_development/Core/Scripting/Event_bubbling#event_delegation) per ulteriori dettagli e un esempio utile.

## Suggerimenti per scrivere codice più efficiente

Esistono diverse buone pratiche generali che renderanno il codice più efficiente.

- **Ridurre la manipolazione del DOM**: Accedere al DOM e aggiornarlo è costoso dal punto di vista computazionale, quindi è opportuno ridurre al minimo quanto JavaScript lo faccia, soprattutto quando si eseguono animazioni DOM costanti (vedere [Gestione delle animazioni JavaScript](#gestione_delle_animazioni_javascript) sopra).
- **Raggruppare le modifiche al DOM**: Per le modifiche essenziali al DOM, è opportuno raggrupparle in gruppi eseguiti insieme, anziché attivare ogni singola modifica non appena si verifica. Questo può ridurre la quantità di lavoro effettivamente svolta dal browser, ma anche migliorare le prestazioni percepite. Può rendere l'interfaccia utente più fluida applicare più aggiornamenti in una volta sola, anziché effettuare costantemente piccoli aggiornamenti. Un suggerimento utile è: quando è necessario aggiungere una grande porzione di HTML alla pagina, costruire prima l'intero frammento, in genere all'interno di un [`DocumentFragment`](/it/docs/Web/API/DocumentFragment), e quindi aggiungerlo tutto al DOM in una sola volta, anziché aggiungere ogni elemento separatamente.
- **Semplificare l'HTML**: Più semplice è l'albero DOM, più rapidamente può essere raggiunto e manipolato con JavaScript. È opportuno riflettere attentamente su ciò di cui l'interfaccia utente ha bisogno e rimuovere gli elementi superflui non necessari.
- **Ridurre la quantità di codice nei cicli**: I cicli sono costosi, quindi è opportuno ridurre l'uso dei cicli nel codice ovunque possibile. Nei casi in cui i cicli sono inevitabili:
  - Evitare di eseguire l'intero ciclo quando non è necessario, usando le istruzioni {{jsxref("Statements/break", "break")}} o {{jsxref("Statements/continue", "continue")}} a seconda dei casi. Per esempio, se si cercano array per un nome specifico, è opportuno uscire dal ciclo una volta trovato il nome; non è necessario eseguire ulteriori iterazioni del ciclo:

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

  - Eseguire fuori dal ciclo il lavoro necessario una sola volta. Può sembrare ovvio, ma è facile trascurarlo. Si consideri il seguente frammento, che recupera un oggetto JSON contenente dati da elaborare in qualche modo. In questo caso, l'operazione [`fetch()`](/it/docs/Web/API/Window/fetch) viene eseguita a ogni iterazione del ciclo, il che costituisce uno spreco di potenza di calcolo. Il recupero, che non dipende da `i`, potrebbe essere spostato fuori dal ciclo, in modo da essere eseguito una sola volta.

    ```js
    async function returnResults(number) {
      for (let i = 0; i < number; i++) {
        const response = await fetch(`/results?number=${number}`);
        const results = await response.json();
        processResult(results[i]);
      }
    }
    ```

- **Eseguire i calcoli al di fuori del thread principale**: In precedenza è stato illustrato come JavaScript esegua generalmente le attività sul thread principale e come le operazioni lunghe possano bloccare il thread principale, portando potenzialmente a scarse prestazioni dell'interfaccia utente. È stato anche mostrato come suddividere le attività lunghe in attività più piccole, mitigando questo problema. Un altro modo per gestire tali problemi è spostare del tutto le attività fuori dal thread principale. Esistono alcuni modi per farlo:
  - Usare codice asincrono: [JavaScript asincrono](/it/docs/Learn_web_development/Extensions/Async_JS/Introducing) è fondamentalmente JavaScript che non blocca il thread principale. Le API asincrone tendono a gestire operazioni quali il recupero di risorse dalla rete, l'accesso a un file nel file system locale o l'apertura di uno stream verso la webcam di un utente. Poiché queste operazioni potrebbero richiedere molto tempo, sarebbe problematico bloccare il thread principale in attesa del loro completamento. Il browser esegue invece queste funzioni, mantiene il thread principale in esecuzione con il codice successivo e le funzioni restituiranno i risultati una volta disponibili _in un momento futuro_. Le API asincrone moderne sono basate su {{jsxref("Promise")}}, una funzionalità del linguaggio JavaScript progettata per gestire operazioni asincrone. È possibile [scrivere funzioni basate su Promise personalizzate](/it/docs/Learn_web_development/Extensions/Async_JS/Implementing_a_promise-based_API) se sono presenti funzionalità che trarrebbero vantaggio dall'esecuzione asincrona.
  - Eseguire i calcoli nei web worker: i [Web Workers](/it/docs/Web/API/Web_Workers_API/Using_web_workers) sono un meccanismo che consente di aprire un thread separato in cui eseguire una porzione di JavaScript, in modo che non blocchi il thread principale. I worker hanno alcune importanti restrizioni, la più significativa delle quali è che non è possibile eseguire scripting DOM all'interno di un worker. È possibile fare la maggior parte delle altre cose e i worker possono inviare e ricevere messaggi da e verso il thread principale. Il caso d'uso principale dei worker si presenta quando c'è molto calcolo da eseguire e non si desidera che blocchi il thread principale. Eseguire il calcolo in un worker, attendere il risultato e inviarlo nuovamente al thread principale quando è pronto.
  - **Usare WebGPU**: [WebGPU](/it/docs/Web/API/WebGPU_API) è un'API del browser che consente agli sviluppatori web di usare la GPU (Graphics Processing Unit) del sistema sottostante per eseguire calcoli ad alte prestazioni e disegnare immagini complesse che possono essere renderizzate nel browser. È abbastanza complessa, ma può offrire vantaggi in termini di prestazioni persino maggiori rispetto ai web worker.

## Vedere anche

- [Ottimizzare le attività lunghe](https://web.dev/articles/optimize-long-tasks) su web.dev (2022)
- [Tutorial Canvas](/it/docs/Web/API/Canvas_API/Tutorial)

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/video", "Learn_web_development/Extensions/Performance/HTML", "Learn_web_development/Extensions/Performance")}}
