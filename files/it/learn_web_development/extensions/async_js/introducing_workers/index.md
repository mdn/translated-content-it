---
title: Introduzione ai worker
slug: Learn_web_development/Extensions/Async_JS/Introducing_workers
l10n:
  sourceCommit: 3e543cdfe8dddfb4774a64bf3decdcbab42a4111
---

{{PreviousMenuNext("Learn_web_development/Extensions/Async_JS/Implementing_a_promise-based_API", "Learn_web_development/Extensions/Async_JS/Sequencing_animations", "Learn_web_development/Extensions/Async_JS")}}

In questo articolo finale del nostro modulo "JavaScript asincrono", verranno presentati i _worker_, che consentono di eseguire alcune attività in un {{Glossary("Thread", "thread")}} di esecuzione separato.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
         Una solida conoscenza dei <a href="/it/docs/Learn_web_development/Core/Scripting">fondamenti di JavaScript</a> e dei concetti asincroni, trattati nelle lezioni precedenti di questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Come e perché usare i web worker dedicati.</li>
          <li>Comprendere lo scopo di altri tipi di web worker, come i worker condivisi e i service worker.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Nel primo articolo di questo modulo, è stato visto cosa accade quando nel programma è presente un'attività sincrona di lunga durata: l'intera finestra diventa completamente non responsiva. Fondamentalmente, la ragione è che il programma è _single-threaded_. Un _thread_ è una sequenza di istruzioni seguita da un programma. Poiché il programma è composto da un singolo thread, può fare una sola cosa alla volta: quindi, se attende il ritorno della chiamata sincrona di lunga durata, non può fare altro.

I worker consentono di eseguire alcune attività in un thread diverso, permettendo di avviare l'attività e poi continuare con altre elaborazioni, ad esempio gestendo le azioni dell'utente.

Una preoccupazione è che, se più thread possono accedere agli stessi dati condivisi, è possibile che li modifichino in modo indipendente e inaspettato, l'uno rispetto all'altro.
Questo può causare bug difficili da individuare.

Per evitare questi problemi sul web, il codice principale e il codice del worker non ottengono mai accesso diretto alle rispettive variabili e possono realmente "condividere" dati solo in casi molto specifici.
I worker e il codice principale vengono eseguiti in mondi completamente separati e interagiscono solo inviandosi messaggi. In particolare, questo significa che i worker non possono accedere al DOM (la finestra, il documento, gli elementi della pagina e così via).

Esistono tre diversi tipi di worker:

- worker dedicati
- worker condivisi
- service worker

In questo articolo verrà illustrato un esempio del primo tipo di worker, quindi verranno discussi brevemente gli altri due.

## Uso dei web worker

Ricordare il primo articolo, in cui era presente una pagina che calcolava i numeri primi? Verrà usato un worker per eseguire il calcolo dei numeri primi, affinché la pagina resti responsiva alle azioni dell'utente.

### Il generatore sincrono di numeri primi

Per prima cosa, diamo un'altra occhiata al JavaScript dell'esempio precedente:

```js
function generatePrimes(quota) {
  function isPrime(n) {
    for (let c = 2; c <= Math.sqrt(n); ++c) {
      if (n % c === 0) {
        return false;
      }
    }
    return true;
  }

  const primes = [];
  const maximum = 1000000;

  while (primes.length < quota) {
    const candidate = Math.floor(Math.random() * (maximum + 1));
    if (isPrime(candidate)) {
      primes.push(candidate);
    }
  }

  return primes;
}

document.querySelector("#generate").addEventListener("click", () => {
  const quota = document.querySelector("#quota").value;
  const primes = generatePrimes(quota);
  document.querySelector("#output").textContent =
    `Finished generating ${quota} primes!`;
});

document.querySelector("#reload").addEventListener("click", () => {
  document.querySelector("#user-input").value =
    'Try typing in here immediately after pressing "Generate primes"';
  document.location.reload();
});
```

In questo programma, dopo la chiamata a `generatePrimes()`, il programma diventa completamente non responsivo.

### Generazione di numeri primi con un worker

Per questo esempio, iniziare creando una copia locale dei file disponibili all'indirizzo <https://github.com/mdn/learning-area/tree/main/javascript/asynchronous/workers/start>. In questa directory sono presenti quattro file:

- index.html
- style.css
- main.js
- generate.js

I file "index.html" e "style.css" sono già completi:

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>Prime numbers</title>
    <script src="main.js" defer></script>
    <link href="style.css" rel="stylesheet" />
  </head>

  <body>
    <label for="quota">Number of primes:</label>
    <input type="text" id="quota" name="quota" value="1000000" />

    <button id="generate">Generate primes</button>
    <button id="reload">Reload</button>

    <textarea id="user-input" rows="5" cols="62">
Try typing in here immediately after pressing "Generate primes"
    </textarea>

    <div id="output"></div>
  </body>
</html>
```

```css
textarea {
  display: block;
  margin: 1rem 0;
}
```

I file "main.js" e "generate.js" sono vuoti. Il codice principale verrà aggiunto a "main.js", mentre il codice del worker verrà aggiunto a "generate.js".

Per prima cosa, quindi, si può vedere che il codice del worker viene mantenuto in uno script separato dal codice principale. Osservando inoltre "index.html" sopra, si può vedere che solo il codice principale è incluso in un elemento `<script>`.

Ora copiare il codice seguente in "main.js":

```js
// Create a new worker, giving it the code in "generate.js"
const worker = new Worker("./generate.js");

// When the user clicks "Generate primes", send a message to the worker.
// The message command is "generate", and the message also contains "quota",
// which is the number of primes to generate.
document.querySelector("#generate").addEventListener("click", () => {
  const quota = document.querySelector("#quota").value;
  worker.postMessage({
    command: "generate",
    quota,
  });
});

// When the worker sends a message back to the main thread,
// update the output box with a message for the user, including the number of
// primes that were generated, taken from the message data.
worker.addEventListener("message", (message) => {
  document.querySelector("#output").textContent =
    `Finished generating ${message.data} primes!`;
});

document.querySelector("#reload").addEventListener("click", () => {
  document.querySelector("#user-input").value =
    'Try typing in here immediately after pressing "Generate primes"';
  document.location.reload();
});
```

- Per prima cosa, viene creato il worker usando il costruttore [`Worker()`](/it/docs/Web/API/Worker/Worker). Viene passato un URL che punta allo script del worker. Non appena il worker viene creato, lo script del worker viene eseguito.

- Successivamente, come nella versione sincrona, viene aggiunto un gestore dell'evento `click` al pulsante "Generate primes". Ora, tuttavia, invece di chiamare una funzione `generatePrimes()`, viene inviato un messaggio al worker usando [`worker.postMessage()`](/it/docs/Web/API/Worker/postMessage). Questo messaggio può accettare un argomento e, in questo caso, viene passato un oggetto JSON contenente due proprietà:
  - `command`: una stringa che identifica l'operazione da eseguire dal worker, nel caso in cui il worker possa fare più di una cosa
  - `quota`: il numero di numeri primi da generare.

- Successivamente, viene aggiunto un gestore dell'evento `message` al worker. Questo permette al worker di segnalare quando ha terminato e di passare eventuali dati risultanti. Il gestore recupera i dati dalla proprietà `data` del messaggio e li scrive nell'elemento di output (i dati sono esattamente uguali a `quota`, quindi ciò è piuttosto inutile, ma mostra il principio).

- Infine, viene implementato il gestore dell'evento `click` per il pulsante "Reload". È esattamente uguale a quello della versione sincrona.

Passiamo ora al codice del worker. Copiare il codice seguente in "generate.js":

```js
// Listen for messages from the main thread.
// If the message command is "generate", call `generatePrimes()`
addEventListener("message", (message) => {
  if (message.data.command === "generate") {
    generatePrimes(message.data.quota);
  }
});

// Generate primes (very inefficiently)
function generatePrimes(quota) {
  function isPrime(n) {
    for (let c = 2; c <= Math.sqrt(n); ++c) {
      if (n % c === 0) {
        return false;
      }
    }
    return true;
  }

  const primes = [];
  const maximum = 1000000;

  while (primes.length < quota) {
    const candidate = Math.floor(Math.random() * (maximum + 1));
    if (isPrime(candidate)) {
      primes.push(candidate);
    }
  }

  // When we have finished, send a message to the main thread,
  // including the number of primes we generated.
  postMessage(primes.length);
}
```

Ricordare che questo codice viene eseguito non appena lo script principale crea il worker.

La prima cosa che fa il worker è iniziare ad ascoltare i messaggi provenienti dallo script principale. Lo fa usando `addEventListener()`, che in un worker è una funzione globale. All'interno del gestore dell'evento `message`, la proprietà `data` dell'evento contiene una copia dell'argomento passato dallo script principale. Se lo script principale ha passato il comando `generate`, viene chiamata `generatePrimes()`, passando il valore `quota` dall'evento del messaggio.

La funzione `generatePrimes()` è uguale a quella della versione sincrona, tranne per il fatto che, anziché restituire un valore, invia un messaggio allo script principale quando termina. A questo scopo viene usata la funzione [`postMessage()`](/it/docs/Web/API/DedicatedWorkerGlobalScope/postMessage), che, come `addEventListener()`, è una funzione globale in un worker. Come già visto, lo script principale è in ascolto di questo messaggio e aggiornerà il DOM quando il messaggio viene ricevuto.

> [!NOTE]
> Per eseguire questo sito, sarà necessario avviare un server web locale, perché gli URL `file://` non possono caricare worker. Consultare [Come configurare un server locale per i test?](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server) per scoprire come fare. Fatto ciò, dovrebbe essere possibile fare clic su "Generate primes" e mantenere responsiva la pagina principale.
>
> In caso di problemi nella creazione o nell'esecuzione dell'esempio, è possibile consultare la [versione completata](https://github.com/mdn/learning-area/tree/main/javascript/asynchronous/workers/finished) e provarla [dal vivo](https://mdn.github.io/learning-area/javascript/asynchronous/workers/finished/).

## Altri tipi di worker

Il worker appena creato è un cosiddetto _worker dedicato_. Ciò significa che viene usato da una singola istanza di script.

Esistono però altri tipi di worker:

- I [_worker condivisi_](/it/docs/Web/API/SharedWorker) possono essere condivisi da diversi script in esecuzione in finestre diverse.
- I [_service worker_](/it/docs/Web/API/Service_Worker_API) agiscono come server proxy, memorizzando nella cache le risorse affinché le applicazioni web possano funzionare quando l'utente è offline. Sono un componente chiave delle [Progressive Web App](/it/docs/Web/Progressive_web_apps).

## Riepilogo

In questo articolo sono stati introdotti i web worker, che consentono a un'applicazione web di delegare attività a un thread separato. Il thread principale e il worker non condividono direttamente alcuna variabile, ma comunicano inviando messaggi, che vengono ricevuti dall'altra parte come eventi `message`.

I worker possono essere un modo efficace per mantenere responsiva l'applicazione principale, anche se non possono accedere a tutte le API disponibili per l'applicazione principale e, in particolare, non possono accedere al DOM.

## Vedi anche

- [Uso dei web worker](/it/docs/Web/API/Web_Workers_API/Using_web_workers)
- [Uso dei service worker](/it/docs/Web/API/Service_Worker_API/Using_Service_Workers)
- [API Web Workers](/it/docs/Web/API/Web_Workers_API)

{{PreviousMenuNext("Learn_web_development/Extensions/Async_JS/Implementing_a_promise-based_API", "Learn_web_development/Extensions/Async_JS/Sequencing_animations", "Learn_web_development/Extensions/Async_JS")}}
