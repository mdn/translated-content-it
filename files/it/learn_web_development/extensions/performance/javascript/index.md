---
title: Ottimizzazione delle prestazioni di JavaScript
short-title: JavaScript Performante
slug: Learn_web_development/Extensions/Performance/JavaScript
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/video", "Learn_web_development/Extensions/Performance/HTML", "Learn_web_development/Extensions/Performance")}}

È molto importante considerare come si sta utilizzando JavaScript sui propri siti web e pensare a come mitigare eventuali problemi di prestazioni che potrebbe causare. Anche se immagini e video costituiscono oltre il 70% dei byte scaricati per il sito web medio, byte per byte, JavaScript ha un potenziale maggiore per un impatto negativo sulle prestazioni — può influenzare significativamente i tempi di download, le prestazioni di rendering, il consumo della CPU e della batteria. Questo articolo introduce consigli e tecniche per ottimizzare JavaScript al fine di migliorare le prestazioni del proprio sito web.

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
        e come mitigare o risolvere i problemi correlati.
      </td>
    </tr>
  </tbody>
</table>

## Ottimizzare o non ottimizzare

La prima domanda a cui si dovrebbe rispondere prima di iniziare a ottimizzare il codice è "cosa devo ottimizzare?". Alcuni dei suggerimenti e delle tecniche discussi di seguito sono buone pratiche che beneficeranno quasi qualsiasi progetto web, mentre alcuni sono necessari solo in determinate situazioni. Provare ad applicare tutte queste tecniche ovunque è probabilmente inutile e potrebbe essere una perdita di tempo. Si dovrebbe capire quali ottimizzazioni delle prestazioni sono effettivamente necessarie in ciascun progetto.

Per fare questo, è necessario [misurare le prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance) del proprio sito. Come mostrato nel link precedente, ci sono diversi modi per misurare le prestazioni, alcuni dei quali coinvolgono sofisticate [API di misurazione delle prestazioni](/it/docs/Web/API/Performance_API). Tuttavia, il modo migliore per iniziare è imparare a utilizzare strumenti come quelli integrati nel browser per monitorare [la rete](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#network_monitor_tools) e [le prestazioni](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance#performance_monitor_tools), per vedere quali parti del carico della pagina stanno impiegando più tempo e hanno bisogno di ottimizzazione.

## Ottimizzare i download di JavaScript

Il JavaScript più performante e meno bloccante che si possa usare è il JavaScript che non si utilizza affatto. Si dovrebbe utilizzare il meno JavaScript possibile. Alcuni suggerimenti da tenere a mente:

- **Non serve sempre un framework**: Potresti essere familiare con l'uso di un [framework JavaScript](/it/docs/Learn_web_development/Core/Frameworks_libraries). Se hai esperienza e confidenza con l'uso di questo framework e ti piacciono tutti gli strumenti che fornisce, potrebbe essere il tuo strumento di riferimento per la costruzione della maggior parte dei progetti. Tuttavia, i framework sono pesanti in JavaScript. Se stai creando un'esperienza abbastanza statica con pochi requisiti JavaScript, probabilmente non hai bisogno di quel framework. Potresti essere in grado di implementare ciò che ti serve utilizzando poche righe di JavaScript standard.
- **Considera una soluzione più semplice**: Potresti avere una soluzione appariscente e interessante da implementare, ma considera se i tuoi utenti l'apprezzeranno. Preferirebbero forse qualcosa di più semplice?
- **Rimuovi il codice non utilizzato**: Questo può sembrare ovvio, ma è sorprendente quanti sviluppatori dimentichino di pulire le funzionalità inutilizzate aggiunte durante il processo di sviluppo. Devi essere attento e preciso su ciò che viene aggiunto e rimosso. Tutti gli script vengono analizzati, che siano utilizzati o meno; quindi, una soluzione rapida per velocizzare i download sarebbe eliminare qualsiasi funzionalità non utilizzata. Considera anche che spesso utilizzerai solo una piccola parte delle funzionalità disponibili in un framework. È possibile creare una build personalizzata del framework che contenga solo la parte che ti serve?
- **Considera le funzionalità integrate del browser**: Potrebbe essere che tu possa utilizzare una funzionalità che il browser ha già, piuttosto che crearne una tua tramite JavaScript. Per esempio:
  - Usa [la validazione dei moduli lato client integrata](/it/docs/Learn_web_development/Extensions/Forms/Form_validation#using_built-in_form_validation).
  - Usa il player del browser per {{htmlelement("video")}}.
  - Usa [le animazioni CSS](/it/docs/Web/CSS/CSS_animations/Using_CSS_animations) invece di una libreria di animazioni JavaScript (vedi anche [Gestione delle animazioni](#gestire_le_animazioni_javascript)).

Dovresti anche suddividere il tuo JavaScript in più file che rappresentano parti critiche e non critiche. I [moduli JavaScript](/it/docs/Web/JavaScript/Guide/Modules) consentono di farlo più efficientemente rispetto a usare semplici file JavaScript esterni separati.

Quindi puoi ottimizzare questi file più piccoli. La {{Glossary("Minification", "minificazione")}} riduce il numero di caratteri nel tuo file, riducendo quindi il numero di byte o il peso del tuo JavaScript. La {{Glossary("Gzip_compression", "compressione Gzip")}} comprime ulteriormente il file e dovrebbe essere usata anche se non minifichi il tuo codice. {{Glossary("Brotli_compression", "Brotli")}} è simile a Gzip, ma generalmente offre una compressione migliore.

Puoi dividere e ottimizzare il tuo codice manualmente, ma spesso un bundler di moduli come [webpack](https://webpack.js.org/) farà un lavoro migliore in questo.

## Gestire l'analisi e l'esecuzione

Prima di esaminare i consigli contenuti in questa sezione, è importante discutere di _dove_ nel processo di rendering della pagina del browser viene gestito JavaScript. Quando viene caricata una pagina web:

1. L'HTML viene generalmente analizzato per primo, nell'ordine in cui appare sulla pagina.
2. Ogni volta che viene incontrato il CSS, viene analizzato per capire gli stili da applicare alla pagina. Durante questo tempo, iniziano ad essere recuperati gli asset collegati come immagini e web font.
3. Ogni volta che viene incontrato JavaScript, il browser lo analizza, lo valuta e lo esegue sulla pagina.
4. Poco dopo, il browser calcola come ogni elemento HTML dovrebbe essere stilizzato, dato il CSS applicato ad esso.
5. Il risultato stilizzato viene poi dipinto sullo schermo.

> [!NOTE]
> Questo è un resoconto molto semplificato di ciò che accade, ma fornisce un'idea.

Il passaggio chiave qui è il Passo 3. Per impostazione predefinita, l'analisi e l'esecuzione di JavaScript bloccano il rendering. Questo significa che il browser blocca l'analisi di qualsiasi HTML che appare dopo che si è incontrato JavaScript, fino a quando lo script non è stato gestito. Di conseguenza, anche la stilizzazione e la pittura vengono bloccate. Questo significa che è necessario pensare attentamente non solo a cosa si sta scaricando, ma anche a quando e come quel codice viene eseguito.

Le prossime sezioni forniscono tecniche utili per ottimizzare l'analisi e l'esecuzione del tuo JavaScript.

## Caricare gli asset critici il prima possibile

Se uno script è davvero importante e si teme che stia influenzando le prestazioni perché non viene caricato abbastanza rapidamente, può essere caricato all'interno del {{htmlelement("head")}} del documento:

```html
<head>
  ...
  <script src="main.js"></script>
  ...
</head>
```

Questo funziona, ma blocca il rendering. Una strategia migliore è utilizzare [`rel="preload"`](/it/docs/Web/HTML/Reference/Attributes/rel/preload) per creare un pre-caricatore per JavaScript critico:

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

Il preload {{htmlelement("link")}} recupera il JavaScript il prima possibile, senza bloccare il rendering. Puoi quindi usarlo ovunque tu voglia nella tua pagina:

```html
<!-- Include this wherever makes sense -->
<script src="important-js.js"></script>
```

o all'interno del tuo script, nel caso di un modulo JavaScript:

```js
import { someFunction } from "important-module.js";
```

> [!NOTE]
> Il precaricamento non garantisce che lo script sarà caricato al momento in cui lo includi, ma significa che inizierà a essere scaricato prima. Il tempo di blocco del rendering sarà comunque ridotto, anche se non completamente eliminato.

## Rinviare l'esecuzione di JavaScript non critico

D'altra parte, dovresti mirare a rinviare l'analisi e l'esecuzione di JavaScript non critico a più tardi, quando è necessario. Caricarlo tutto in anticipo blocca il rendering inutilmente.

Per prima cosa, puoi aggiungere l'attributo `async` ai tuoi elementi `<script>`:

```html
<head>
  ...
  <script async src="main.js"></script>
  ...
</head>
```

Questo provoca il recupero dello script in parallelo con l'analisi del DOM, quindi sarà pronto nello stesso momento e non bloccherà il rendering.

> [!NOTE]
> C'è un altro attributo, `defer`, che provoca l'esecuzione dello script dopo che il documento è stato analizzato, ma prima di attivare l'evento [`DOMContentLoaded`](/it/docs/Web/API/Document/DOMContentLoaded_event). Questo ha un effetto simile a `async`.

Potresti anche non caricare affatto il JavaScript fino a che non si verifica un evento in cui è necessario. Questo potrebbe essere fatto tramite scripting DOM, ad esempio:

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

## Suddividere attività lunghe

Quando il browser esegue il tuo JavaScript, organizza lo script in attività che vengono eseguite sequenzialmente, come le richieste fetch, la guida delle interazioni e degli input dell'utente tramite gestori eventi, l'esecuzione di animazioni guidate da JavaScript, e così via.

La maggior parte di ciò avviene nel thread principale, con eccezioni tra cui il JavaScript che viene eseguito nei [Web Workers](/it/docs/Web/API/Web_Workers_API/Using_web_workers). Il thread principale può eseguire solo un'attività alla volta.

Quando un singolo compito impiega più di 50 ms per essere eseguito, viene classificato come compito lungo. Se l'utente tenta di interagire con la pagina o viene richiesto un aggiornamento UI importante mentre è in esecuzione un compito lungo, la loro esperienza sarà influenzata. Una risposta attesa o un aggiornamento visivo sarà ritardato, facendo apparire l'interfaccia utente lenta o non reattiva.

Per mitigare questo problema, è necessario suddividere compiti lunghi in compiti più piccoli. Questo dà al browser più possibilità di eseguire operazioni vitali di gestione dell'interazione dell'utente o aggiornamenti di rendering dell'interfaccia utente — il browser può potenzialmente eseguirli tra ciascun compito più piccolo, invece che solo prima o dopo il compito lungo. Nel tuo JavaScript, potresti farlo suddividendo il tuo codice in funzioni separate. Questo ha senso anche per diversi altri motivi, come facilità di manutenzione, debug e scrittura dei test.

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

Tuttavia, questo tipo di struttura non aiuta con il blocco del thread principale. Poiché tutte e cinque le funzioni vengono eseguite all'interno di una funzione principale, il browser le esegue tutte come un singolo compito lungo.

Per gestire questo, tendiamo a eseguire periodicamente una funzione "yield" per far sì che il codice _ceda al thread principale_. Questo significa che il nostro codice è suddiviso in più compiti, tra l'esecuzione dei quali il browser ha l'opportunità di gestire compiti ad alta priorità come l'aggiornamento dell'interfaccia utente. Un modello comune per questa funzione utilizza [`setTimeout()`](/it/docs/Web/API/Window/setTimeout) per posticipare l'esecuzione in un compito separato:

```js
function yield() {
  return new Promise((resolve) => {
    setTimeout(resolve, 0);
  });
}
```

Questo può essere utilizzato all'interno di un modello di esecuzione dei compiti come segue, per cedere al thread principale dopo che ogni compito è stato eseguito:

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

Per migliorare ulteriormente questo, possiamo utilizzare [`Scheduler.yield()`](/it/docs/Web/API/Scheduler/yield) dove disponibile per consentire a questo codice di continuare a essere eseguito prima di altri compiti meno critici nella coda:

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

## Gestire le animazioni JavaScript

Le animazioni possono migliorare le prestazioni percepite, rendendo le interfacce più reattive e facendo sentire gli utenti come se stesse procedendo quando stanno aspettando il caricamento di una pagina (ad esempio, caricando degli spinner). Tuttavia, animazioni più grandi e un numero maggiore di animazioni richiederanno naturalmente più potenza di elaborazione per essere gestite, il che può degradare le prestazioni.

Il consiglio più ovvio sulle animazioni è di utilizzare meno animazioni — eliminare eventuali animazioni non essenziali o considerare di offrire agli utenti una preferenza che possono impostare per disattivare le animazioni, ad esempio se stanno utilizzando un dispositivo a bassa potenza o un dispositivo mobile con batteria limitata.

Per animazioni DOM essenziali, si consiglia di utilizzare [animazioni CSS](/it/docs/Web/CSS/CSS_animations/Using_CSS_animations) dove possibile, piuttosto che animazioni JavaScript (l'[API delle animazioni web](/it/docs/Web/API/Web_Animations_API) offre un modo per collegarsi direttamente alle animazioni CSS usando JavaScript). Utilizzare il browser per eseguire direttamente le animazioni DOM piuttosto che manipolare gli stili inline usando JavaScript è molto più veloce ed efficiente. Vedi anche [Ottimizzazione delle prestazioni CSS > Gestione delle animazioni](/it/docs/Learn_web_development/Extensions/Performance/CSS#handling_animations).

Per animazioni che non possono essere gestite in JavaScript, ad esempio, animando un HTML {{htmlelement("canvas")}}, si consiglia di utilizzare [`Window.requestAnimationFrame()`](/it/docs/Web/API/Window/requestAnimationFrame) piuttosto che opzioni più vecchie come [`Window.setInterval()`](/it/docs/Web/API/Window/setInterval). Il metodo `requestAnimationFrame()` è progettato appositamente per gestire i frame di animazione in modo efficiente e coerente, per un'esperienza utente fluida. Il modello di base sembra questo:

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

Puoi trovare una bella introduzione alle animazioni canvas in [Disegnare grafica > Animazioni](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics#animations) e un esempio più approfondito in [Pratica di costruzione di oggetti](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_building_practice). Puoi anche trovare una serie completa di tutorial sui canvas in [Tutorial sul canvas](/it/docs/Web/API/Canvas_API/Tutorial).

## Ottimizzare le prestazioni degli eventi

Gli eventi possono essere costosi per il browser da tracciare e gestire, specialmente quando si esegue un evento continuamente. Ad esempio, potresti tracciare la posizione del mouse usando l'evento [`mousemove`](/it/docs/Web/API/Element/mousemove_event) per controllare se è ancora all'interno di una certa area della pagina:

```js
function handleMouseMove() {
  // Do stuff while mouse pointer is inside elem
}

elem.addEventListener("mousemove", handleMouseMove);
```

Potresti eseguire un gioco `<canvas>` nella tua pagina. Finché il mouse è all'interno del canvas, vorrai controllare costantemente il movimento del mouse e la posizione del cursore e aggiornare lo stato del gioco — compreso il punteggio, il tempo, la posizione di tutti i sprite, le informazioni sul rilevamento delle collisioni, ecc. Una volta che il gioco è finito, non avrai più bisogno di fare tutto ciò, e infatti sarà uno spreco di potenza di elaborazione continuare ad ascoltare per quell'evento.

È quindi una buona idea rimuovere i listener di eventi che non sono più necessari. Questo può essere fatto usando [`removeEventListener()`](/it/docs/Web/API/EventTarget/removeEventListener):

```js
elem.removeEventListener("mousemove", handleMouseMove);
```

Un altro consiglio è utilizzare la delega degli eventi ove possibile. Quando hai del codice da eseguire in risposta a un utente che interagisce con uno qualsiasi di un gran numero di elementi figli, puoi impostare un listener di eventi sul loro genitore. Gli eventi lanciati su qualsiasi elemento figlio verranno propagati verso il loro genitore, quindi non hai bisogno di impostare il listener di eventi su ciascun figlio singolarmente. Meno listener di eventi da tenere traccia significa migliori prestazioni.

Vedi [Delega degli eventi](/it/docs/Learn_web_development/Core/Scripting/Event_bubbling#event_delegation) per maggiori dettagli e un esempio utile.

## Consigli per scrivere codice più efficiente

Ci sono diverse pratiche migliori generali che renderanno il tuo codice più efficiente.

- **Riduci la manipolazione del DOM**: Accedere e aggiornare il DOM è computazionalmente costoso, quindi dovresti minimizzare il tiro che il tuo JavaScript fa, specialmente quando esegui animazioni DOM costanti (vedi [Gestione delle animazioni JavaScript](#gestire_le_animazioni_javascript) qui sopra).
- **Raggruppa le modifiche al DOM**: Per i cambiamenti al DOM essenziali, dovresti raggrupparli in gruppi che vengono eseguiti insieme, anziché semplicemente eseguire ogni singolo cambiamento appena si verifica. Questo può ridurre la quantità di lavoro che il browser sta facendo in termini reali, ma anche migliorare le prestazioni percepite. Può far sembrare l'interfaccia utente più fluida unendo diversi aggiornamenti in un unico colpo, piuttosto che fare costantemente piccoli aggiornamenti. Un utile consiglio qui è — quando hai un grande frammento di HTML da aggiungere alla pagina, costruisci l'intero frammento prima (tipicamente all'interno di un [`DocumentFragment`](/it/docs/Web/API/DocumentFragment)) e poi aggiungilo tutto al DOM in un solo colpo, anziché aggiungere ciascun elemento separatamente.
- **Semplifica il tuo HTML**: Più semplice è il tuo albero del DOM, più velocemente può essere accesso e manipolato con JavaScript. Pensa attentamente a cosa ha bisogno la tua interfaccia utente e rimuovi complicazioni inutili.
- **Riduci la quantità di codice in loop**: I loop sono costosi, quindi riduci la quantità di utilizzo dei loop nel tuo codice ove possibile. Nei casi in cui i loop sono inevitabili:

  - Evita di eseguire il loop completo quando non è necessario, utilizzando dichiarazioni {{jsxref("Statements/break", "break")}} o {{jsxref("Statements/continue", "continue")}} come appropriato. Ad esempio, se stai cercando nomi specifici in array, dovresti uscire dal loop una volta trovato il nome; non c'è bisogno di eseguire ulteriori iterazioni del loop:

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

  - Fai il lavoro che è necessario solo una volta fuori dal loop. Questo può sembrare un po' ovvio, ma è facile trascurarlo. Prendi il seguente frammento, che recupera un oggetto JSON contenente dati da elaborare in qualche modo. In questo caso l'operazione [`fetch()`](/it/docs/Web/API/Window/fetch) viene svolta a ogni iterazione del ciclo, il che è uno spreco di risorse. Il fetch, che non dipende da `i`, potrebbe essere spostato fuori dal ciclo, quindi viene eseguito solo una volta.

    ```js
    async function returnResults(number) {
      for (let i = 0; i < number; i++) {
        const response = await fetch(`/results?number=${number}`);
        const results = await response.json();
        processResult(results[i]);
      }
    }
    ```

- **Esegui il calcolo fuori dal thread principale**: In precedenza abbiamo parlato di come JavaScript generalmente esegue attività nel thread principale, e di come operazioni lunghe possano bloccare il thread principale, potenzialmente portando a una cattiva prestazione dell'UI. Abbiamo anche mostrato come suddividere lunghe attività in compiti più piccoli, mitigando questo problema. Un altro modo di affrontare tali problemi è spostare le attività fuori dal thread principale. Ci sono alcuni modi per fare questo:

  - Usa codice asincrono: [JavaScript asincrono](/it/docs/Learn_web_development/Extensions/Async_JS/Introducing) è fondamentalmente JavaScript che non blocca il thread principale. Le API asincrone tendono a gestire operazioni come l'acquisizione di risorse dalla rete, l'accesso a un file sul sistema di file locale o l'apertura di un flusso alla webcam di un utente. Poiché tali operazioni potrebbero richiedere molto tempo, sarebbe sbagliato bloccare semplicemente il thread principale mentre attendiamo che si completino. Invece, il browser esegue tali funzioni, mantiene il thread principale che esegue il codice successivo e queste funzioni restituiranno risultati una volta che sono disponibili _a un certo punto in futuro_. Le moderne API asincrone sono basate su {{jsxref("Promise")}}, che è una caratteristica del linguaggio JavaScript progettata per gestire operazioni asincrone. È possibile [scrivere le proprie funzioni basate su Promise](/it/docs/Learn_web_development/Extensions/Async_JS/Implementing_a_promise-based_API) se si dispone di funzionalità che trarrebbero beneficio dall'essere eseguite in modo asincrono.
  - Esegui i calcoli nei lavoratori web: [Web Workers](/it/docs/Web/API/Web_Workers_API/Using_web_workers) sono un meccanismo che ti permette di aprire un thread separato per eseguire un pezzo di JavaScript in modo che non blocchi il thread principale. I lavoratori hanno alcune restrizioni importanti, la più grande delle quali è che non puoi fare alcun scripting DOM all'interno di un lavoratore. Puoi fare la maggior parte delle altre cose, e i lavoratori possono inviare e ricevere messaggi al e dal thread principale. L'uso principale per i lavoratori è se hai un sacco di calcoli da fare e non vuoi che blocchino il thread principale. Fai quei calcoli in un lavoratore, aspetta il risultato e invialo al thread principale quando è pronto.
  - **Usa WebGPU**: [WebGPU](/it/docs/Web/API/WebGPU_API) è un'API del browser che consente agli sviluppatori web di utilizzare la GPU (Graphic Processing Unit) del sistema sottostante per eseguire calcoli ad alte prestazioni e disegnare immagini complesse che possono essere renderizzate nel browser. È abbastanza complesso, ma può fornire benefici di prestazioni ancor più superiori rispetto ai web workers.

## Vedi anche

- [Ottimizza compiti lunghi](https://web.dev/articles/optimize-long-tasks) su web.dev (2022)
- [Tutorial sul canvas](/it/docs/Web/API/Canvas_API/Tutorial)

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/video", "Learn_web_development/Extensions/Performance/HTML", "Learn_web_development/Extensions/Performance")}}
