---
title: Introduzione ai workers
slug: Learn_web_development/Extensions/Async_JS/Introducing_workers
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Async_JS/Implementing_a_promise-based_API", "Learn_web_development/Extensions/Async_JS/Sequencing_animations", "Learn_web_development/Extensions/Async_JS")}}

In questo ultimo articolo del nostro modulo "JavaScript asincrono", introdurremo i _workers_, che le permettono di eseguire alcuni compiti in un {{Glossary("Thread", "thread")}} di esecuzione separato.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
         Una solida comprensione dei <a href="/it/docs/Learn_web_development/Core/Scripting">fondamenti di JavaScript</a> e dei concetti asincroni, come trattato nelle lezioni precedenti di questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Come utilizzare i web workers dedicati e perché.</li>
          <li>Comprendere lo scopo di altri tipi di web worker, come i shared worker e i service worker.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Nel primo articolo di questo modulo, abbiamo visto cosa succede quando nel suo programma c'è un'attività sincrona di lunga durata: l'intera finestra diventa totalmente non reattiva. Fondamentalmente, la ragione di ciò è che il programma è _monothread_. Un _thread_ è una sequenza di istruzioni che un programma segue. Poiché il programma è costituito da un singolo thread, può fare solo una cosa alla volta: quindi, se sta aspettando che la nostra chiamata sincrona di lunga durata ritorni, non può fare nient'altro.

I workers le offrono la possibilità di eseguire alcuni compiti in un thread diverso, in modo che possa iniziare il compito e poi continuare con altri processi (come gestire le azioni dell'utente).

Una preoccupazione di tutto questo è che se più thread possono avere accesso agli stessi dati condivisi, è possibile che li modifichino in modo indipendente e inaspettato (rispetto agli altri).
Ciò può causare bug difficili da individuare.

Per evitare questi problemi sul web, il suo codice principale e il codice del worker non ottengono mai l'accesso diretto alle variabili dell'altro e possono "condividere" dati solo in casi molto specifici. I workers e il codice principale operano in mondi completamente separati e interagiscono solo inviandosi messaggi. In particolare, ciò significa che i workers non possono accedere al DOM (alla finestra, al documento, agli elementi della pagina, ecc.).

Esistono tre diversi tipi di workers:

- workers dedicati
- workers condivisi
- service workers

In questo articolo, esamineremo un esempio del primo tipo di worker, per poi discutere brevemente degli altri due.

## Utilizzo dei web workers

Ricorda nel primo articolo, dove avevamo una pagina che calcolava i numeri primi? Useremo un worker per eseguire il calcolo dei numeri primi, in modo che la nostra pagina rimanga reattiva alle azioni dell'utente.

### Il generatore sincrono di numeri primi

Diamo prima un'altra occhiata al JavaScript nel nostro esempio precedente:

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

In questo programma, dopo aver chiamato `generatePrimes()`, il programma diventa totalmente non reattivo.

### Generazione di numeri primi con un worker

Per questo esempio, inizi con il fare una copia locale dei file su <https://github.com/mdn/learning-area/tree/main/javascript/asynchronous/workers/start>. Ci sono quattro file in questa directory:

- index.html
- style.css
- main.js
- generate.js

Il file "index.html" e i file "style.css" sono già completi:

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

I file "main.js" e "generate.js" sono vuoti. Aggiungeremo il codice principale a "main.js" e il codice del worker a "generate.js".

Quindi, prima possiamo vedere che il codice del worker è mantenuto in uno script separato dal codice principale. Possiamo anche vedere, guardando "index.html" sopra, che solo il codice principale è incluso in un elemento `<script>`.

Ora copi il seguente codice in "main.js":

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

- Per prima cosa, creiamo il worker usando il costruttore [`Worker()`](/it/docs/Web/API/Worker/Worker). Gli passiamo un URL che punta allo script del worker. Non appena il worker è creato, lo script del worker viene eseguito.

- Poi, come nella versione sincrona, aggiungiamo un gestore dell'evento `click` al pulsante "Genera numeri primi". Ma ora, anziché chiamare una funzione `generatePrimes()`, inviamo un messaggio al worker usando [`worker.postMessage()`](/it/docs/Web/API/Worker/postMessage). Questo messaggio può prendere un argomento, e in questo caso, stiamo passando un oggetto JSON contenente due proprietà:

  - `command`: una stringa che identifica la cosa che vogliamo che il worker faccia (nel caso il nostro worker potesse fare più di una cosa)
  - `quota`: il numero di numeri primi da generare.

- Successivamente, aggiungiamo un gestore dell'evento `message` al worker. Questo serve affinché il worker possa dirci quando ha finito e passarci eventuali dati risultanti. Il nostro gestore prende i dati dalla proprietà `data` del messaggio e li scrive nell'elemento di output (i dati sono esattamente gli stessi di `quota`, quindi questa è un po' inutile, ma dimostra il principio).

- Infine, implementiamo il gestore dell'evento `click` per il pulsante "Ricarica". Questo è esattamente lo stesso della versione sincrona.

Ora per il codice del worker. Copi il seguente codice in "generate.js":

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

Ricordi che questo viene eseguito non appena lo script principale crea il worker.

La prima cosa che il worker fa è iniziare ad ascoltare i messaggi dallo script principale. Lo fa utilizzando `addEventListener()`, che è una funzione globale in un worker. All'interno del gestore dell'evento `message`, la proprietà `data` dell'evento contiene una copia dell'argomento passato dallo script principale. Se lo script principale ha passato il comando `generate`, chiamiamo `generatePrimes()`, passando il valore `quota` dall'evento del messaggio.

La funzione `generatePrimes()` è proprio come la versione sincrona, tranne per il fatto che invece di restituire un valore, inviamo un messaggio allo script principale quando abbiamo finito. Usiamo la funzione [`postMessage()`](/it/docs/Web/API/DedicatedWorkerGlobalScope/postMessage) per questo, che come `addEventListener()` è una funzione globale in un worker. Come abbiamo già visto, lo script principale sta ascoltando questo messaggio e aggiornerà il DOM quando il messaggio verrà ricevuto.

> [!NOTE]
> Per eseguire questo sito, dovrà eseguire un server web locale, perché gli URL file:// non sono autorizzati a caricare workers. Vedere [Come configurare un server di testing locale?](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server) per scoprire come farlo. Fatto ciò, dovrebbe essere in grado di fare clic su "Genera numeri primi" mentre la sua pagina principale rimane reattiva.
>
> Se ha problemi a creare o eseguire l'esempio, può esaminare la [versione finale](https://github.com/mdn/learning-area/tree/main/javascript/asynchronous/workers/finished) e provarla [dal vivo](https://mdn.github.io/learning-area/javascript/asynchronous/workers/finished/).

## Altri tipi di workers

Il worker che abbiamo appena creato è quello che si chiama un _worker dedicato_. Ciò significa che è usato da un singolo istanza di script.

Tuttavia, ci sono altri tipi di workers:

- I [_Shared workers_](/it/docs/Web/API/SharedWorker) possono essere condivisi da diversi script che girano in diverse finestre.
- I [_Service workers_](/it/docs/Web/API/Service_Worker_API) si comportano come server proxy, memorizzando nella cache le risorse in modo che le applicazioni web possano funzionare quando l'utente è offline. Sono un componente chiave delle [Progressive Web Apps](/it/docs/Web/Progressive_web_apps).

## Sommario

In questo articolo abbiamo introdotto i web workers, che permettono a un'applicazione web di delegare compiti a un thread separato. Il thread principale e il worker non condividono direttamente alcuna variabile, ma comunicano inviandosi messaggi, che vengono ricevuti dall'altra parte come eventi `message`.

I workers possono essere un modo efficace per mantenere l'applicazione principale reattiva, anche se non possono accedere a tutte le API a cui l'applicazione principale può accedere, e in particolare non possono accedere al DOM.

## Vedere anche

- [Uso dei web workers](/it/docs/Web/API/Web_Workers_API/Using_web_workers)
- [Uso dei service worker](/it/docs/Web/API/Service_Worker_API/Using_Service_Workers)
- [API dei web workers](/it/docs/Web/API/Web_Workers_API)

{{PreviousMenuNext("Learn_web_development/Extensions/Async_JS/Implementing_a_promise-based_API", "Learn_web_development/Extensions/Async_JS/Sequencing_animations", "Learn_web_development/Extensions/Async_JS")}}
