---
title: Introduzione a JavaScript asincrono
short-title: Introduction
slug: Learn_web_development/Extensions/Async_JS/Introducing
l10n:
  sourceCommit: 00f8a68014509bb2fe795ece956c7571a80b9fd9
---

{{NextMenu("Learn_web_development/Extensions/Async_JS/Promises", "Learn_web_development/Extensions/Async_JS")}}

In questo articolo verrà spiegato che cos'è la programmazione asincrona, perché è necessaria e verranno brevemente illustrati alcuni dei modi in cui le funzioni asincrone sono state storicamente implementate in JavaScript.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una solida comprensione dei <a href="/it/docs/Learn_web_development/Core/Scripting">fondamenti di JavaScript</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Acquisire familiarità con JavaScript asincrono, con le differenze rispetto a JavaScript sincrono e con i motivi per cui è necessario.</li>
          <li>Che cos'è la programmazione sincrona e perché talvolta può essere problematica.</li>
          <li>Come la programmazione asincrona mira a risolvere questi problemi.</li>
          <li>Gestori di eventi e funzioni di callback, e il loro rapporto con la programmazione asincrona.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

La programmazione asincrona è una tecnica che consente al programma di avviare un'attività potenzialmente lunga e rimanere comunque reattivo ad altri eventi durante l'esecuzione dell'attività, anziché dover attendere che questa sia terminata. Al termine dell'attività, al programma viene presentato il risultato.

Molte funzioni fornite dai browser, in particolare quelle più interessanti, possono potenzialmente richiedere molto tempo e sono quindi asincrone. Ad esempio:

- Effettuare richieste HTTP usando [`fetch()`](/it/docs/Web/API/Window/fetch)
- Accedere alla fotocamera o al microfono di un utente usando [`getUserMedia()`](/it/docs/Web/API/MediaDevices/getUserMedia)
- Chiedere a un utente di selezionare file usando [`showOpenFilePicker()`](/it/docs/Web/API/Window/showOpenFilePicker)

Quindi, anche se potrebbe non essere necessario _implementare_ molto spesso funzioni asincrone personalizzate, è molto probabile che sia necessario _utilizzarle_ correttamente.

In questo articolo si inizierà esaminando il problema delle funzioni sincrone a lunga esecuzione, che rendono necessaria la programmazione asincrona.

## Programmazione sincrona

Si consideri il codice seguente:

```js
const name = "Miriam";
const greeting = `Hello, my name is ${name}!`;
console.log(greeting);
// "Hello, my name is Miriam!"
```

Questo codice:

1. Dichiara una stringa denominata `name`.
2. Dichiara un'altra stringa denominata `greeting`, che usa `name`.
3. Visualizza il saluto nella console JavaScript.

Va osservato che il browser esegue effettivamente il programma una riga alla volta, nell'ordine in cui è stato scritto. In ogni punto, il browser attende che la riga termini il proprio lavoro prima di passare alla riga successiva. Deve farlo perché ogni riga dipende dal lavoro svolto nelle righe precedenti.

Questo rende il programma **sincrono**. Resterebbe sincrono anche richiamando una funzione separata, come in questo caso:

```js
function makeGreeting(name) {
  return `Hello, my name is ${name}!`;
}

const name = "Miriam";
const greeting = makeGreeting(name);
console.log(greeting);
// "Hello, my name is Miriam!"
```

Qui, `makeGreeting()` è una **funzione sincrona** perché il chiamante deve attendere che la funzione termini il proprio lavoro e restituisca un valore prima di poter continuare.

## Una funzione sincrona a lunga esecuzione

Cosa succede se la funzione sincrona richiede molto tempo?

Il programma seguente usa un algoritmo molto inefficiente per generare più numeri primi grandi quando un utente fa clic sul pulsante "Generate primes". Più alto è il numero di numeri primi specificato dall'utente, più tempo richiederà l'operazione.

```html
<label for="quota">Number of primes:</label>
<input type="text" id="quota" name="quota" value="1000000" />

<button id="generate">Generate primes</button>
<button id="reload">Reload</button>

<div id="output"></div>
```

```js
const MAX_PRIME = 1000000;

function isPrime(n) {
  for (let i = 2; i <= Math.sqrt(n); i++) {
    if (n % i === 0) {
      return false;
    }
  }
  return n > 1;
}

const random = (max) => Math.floor(Math.random() * max);

function generatePrimes(quota) {
  const primes = [];
  while (primes.length < quota) {
    const candidate = random(MAX_PRIME);
    if (isPrime(candidate)) {
      primes.push(candidate);
    }
  }
  return primes;
}

const quota = document.querySelector("#quota");
const output = document.querySelector("#output");

document.querySelector("#generate").addEventListener("click", () => {
  const primes = generatePrimes(quota.value);
  output.textContent = `Finished generating ${quota.value} primes!`;
});

document.querySelector("#reload").addEventListener("click", () => {
  document.location.reload();
});
```

{{EmbedLiveSample("A long-running synchronous function", 600, 120)}}

Provare a fare clic su "Generate primes". A seconda della velocità del computer, probabilmente saranno necessari alcuni secondi prima che il programma visualizzi il messaggio "Finished!".

## Il problema delle funzioni sincrone a lunga esecuzione

L'esempio successivo è uguale al precedente, tranne per l'aggiunta di una casella di testo in cui digitare. Questa volta, fare clic su "Generate primes" e provare a digitare nella casella di testo subito dopo.

Si noterà che, mentre è in esecuzione la funzione `generatePrimes()`, il programma non risponde affatto: non è possibile digitare nulla, fare clic su nulla o eseguire altre azioni.

```html hidden
<label for="quota">Number of primes:</label>
<input type="text" id="quota" name="quota" value="1000000" />

<button id="generate">Generate primes</button>
<button id="reload">Reload</button>

<textarea id="user-input" rows="5" cols="62">
Try typing in here immediately after pressing "Generate primes"
</textarea>

<div id="output"></div>
```

```css hidden
textarea {
  display: block;
  margin: 1rem 0;
}
```

```js hidden
const MAX_PRIME = 1000000;

function isPrime(n) {
  for (let i = 2; i <= Math.sqrt(n); i++) {
    if (n % i === 0) {
      return false;
    }
  }
  return n > 1;
}

const random = (max) => Math.floor(Math.random() * max);

function generatePrimes(quota) {
  const primes = [];
  while (primes.length < quota) {
    const candidate = random(MAX_PRIME);
    if (isPrime(candidate)) {
      primes.push(candidate);
    }
  }
  return primes;
}

const quota = document.querySelector("#quota");
const output = document.querySelector("#output");

document.querySelector("#generate").addEventListener("click", () => {
  const primes = generatePrimes(quota.value);
  output.textContent = `Finished generating ${quota.value} primes!`;
});

document.querySelector("#reload").addEventListener("click", () => {
  document.location.reload();
});
```

{{EmbedLiveSample("The trouble with long-running synchronous functions", 600, 200)}}

Il motivo è che questo programma JavaScript è _a thread singolo_. Un thread è una sequenza di istruzioni seguita da un programma. Poiché il programma è composto da un solo thread, può eseguire una sola operazione alla volta: quindi, se è in attesa del ritorno della chiamata sincrona a lunga esecuzione, non può fare altro.

Serve un modo affinché il programma possa:

1. Avviare un'operazione a lunga esecuzione chiamando una funzione.
2. Fare in modo che la funzione avvii l'operazione e restituisca immediatamente il controllo, affinché il programma possa continuare a rispondere ad altri eventi.
3. Fare in modo che la funzione esegua l'operazione senza bloccare il thread principale, ad esempio avviando un nuovo thread.
4. Ricevere una notifica con il risultato dell'operazione quando questa viene infine completata.

Questo è esattamente ciò che consentono di fare le funzioni asincrone. Il resto di questo modulo spiega come vengono implementate in JavaScript.

## Gestori di eventi

La descrizione appena vista delle funzioni asincrone potrebbe ricordare i gestori di eventi, e in tal caso è corretto. I gestori di eventi sono effettivamente una forma di programmazione asincrona: viene fornita una funzione, il gestore di eventi, che sarà chiamata non immediatamente, ma quando si verifica l'evento. Se "l'evento" è "l'operazione asincrona è stata completata", allora quell'evento può essere usato per notificare al chiamante il risultato di una chiamata a una funzione asincrona.

Alcune delle prime API asincrone usavano gli eventi proprio in questo modo. L'API [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest) consente di effettuare richieste HTTP a un server remoto usando JavaScript. Poiché ciò può richiedere molto tempo, si tratta di un'API asincrona e si ricevono notifiche sull'avanzamento e sul completamento finale di una richiesta aggiungendo listener di eventi all'oggetto `XMLHttpRequest`.

L'esempio seguente mostra questo comportamento in azione. Premere "Click to start request" per inviare una richiesta. Viene creato un nuovo [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest) e viene ascoltato il relativo evento [`loadend`](/it/docs/Web/API/XMLHttpRequestEventTarget/loadend_event). Il gestore registra un messaggio "Finished!" insieme al codice di stato.

Dopo aver aggiunto il listener di eventi, viene inviata la richiesta. Si noti che, dopo questo passaggio, è possibile registrare "Started XHR request": vale a dire che il programma può continuare a essere eseguito mentre la richiesta è in corso e il gestore di eventi verrà chiamato quando la richiesta sarà completata.

```html
<button id="xhr">Click to start request</button>
<button id="reload">Reload</button>

<pre class="event-log"></pre>
```

```css hidden
pre {
  display: block;
  margin: 1rem 0;
}
```

```js
const log = document.querySelector(".event-log");

document.querySelector("#xhr").addEventListener("click", () => {
  log.textContent = "";

  const xhr = new XMLHttpRequest();

  xhr.addEventListener("loadend", () => {
    log.textContent = `${log.textContent}Finished with status: ${xhr.status}`;
  });

  xhr.open(
    "GET",
    "https://raw.githubusercontent.com/mdn/content/main/files/en-us/_wikihistory.json",
  );
  xhr.send();
  log.textContent = `${log.textContent}Started XHR request\n`;
});

document.querySelector("#reload").addEventListener("click", () => {
  log.textContent = "";
  document.location.reload();
});
```

{{EmbedLiveSample("Event handlers", 600, 120)}}

Si tratta di un [gestore di eventi](/it/docs/Learn_web_development/Core/Scripting/Events), proprio come i gestori per le azioni dell'utente, ad esempio quando un utente fa clic su un pulsante. Questa volta, tuttavia, l'evento è una modifica dello stato di un oggetto.

## Callback

Un gestore di eventi è un particolare tipo di callback. Una callback è semplicemente una funzione passata a un'altra funzione, con l'aspettativa che venga chiamata al momento opportuno. Come appena visto, le callback erano il modo principale con cui venivano implementate le funzioni asincrone in JavaScript.

Tuttavia, il codice basato su callback può diventare difficile da comprendere quando la callback stessa deve chiamare funzioni che accettano una callback. Questa è una situazione comune quando è necessario eseguire un'operazione che si suddivide in una serie di funzioni asincrone. Ad esempio, si consideri quanto segue:

```js
function doStep1(init) {
  return init + 1;
}

function doStep2(init) {
  return init + 2;
}

function doStep3(init) {
  return init + 3;
}

function doOperation() {
  let result = 0;
  result = doStep1(result);
  result = doStep2(result);
  result = doStep3(result);
  console.log(`result: ${result}`);
}

doOperation();
```

Qui è presente una singola operazione suddivisa in tre passaggi, in cui ogni passaggio dipende da quello precedente. Nell'esempio, il primo passaggio aggiunge 1 all'input, il secondo aggiunge 2 e il terzo aggiunge 3. Partendo da un input pari a 0, il risultato finale è 6 (0 + 1 + 2 + 3). Come programma sincrono, questo è molto semplice. Ma cosa accadrebbe se i passaggi fossero implementati usando callback?

```js
function doStep1(init, callback) {
  const result = init + 1;
  callback(result);
}

function doStep2(init, callback) {
  const result = init + 2;
  callback(result);
}

function doStep3(init, callback) {
  const result = init + 3;
  callback(result);
}

function doOperation() {
  doStep1(0, (result1) => {
    doStep2(result1, (result2) => {
      doStep3(result2, (result3) => {
        console.log(`result: ${result3}`);
      });
    });
  });
}

doOperation();
```

Poiché è necessario chiamare callback all'interno di altre callback, si ottiene una funzione `doOperation()` profondamente annidata, molto più difficile da leggere e sottoporre a debug. Questa situazione viene talvolta chiamata "callback hell" o "pyramid of doom" perché il rientro assomiglia a una piramide su un lato.

Quando si annidano callback in questo modo, può diventare molto difficile anche gestire gli errori: spesso è necessario gestire gli errori a ogni livello della "piramide", anziché avere la gestione degli errori una sola volta al livello più alto.

Per questi motivi, la maggior parte delle API asincrone moderne non usa callback. Al contrario, la base della programmazione asincrona in JavaScript è {{jsxref("Promise")}}, che è l'argomento del prossimo articolo.

{{NextMenu("Learn_web_development/Extensions/Async_JS/Promises", "Learn_web_development/Extensions/Async_JS")}}
