---
title: Introduzione alla programmazione asincrona in JavaScript
short-title: Introduction
slug: Learn_web_development/Extensions/Async_JS/Introducing
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Async_JS/Promises", "Learn_web_development/Extensions/Async_JS")}}

In questo articolo, spiegheremo cos'è la programmazione asincrona, perché ne abbiamo bisogno, e discuteremo brevemente alcuni dei modi in cui le funzioni asincrone sono state storicamente implementate in JavaScript.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una solida comprensione dei <a href="/it/docs/Learn_web_development/Core/Scripting">fondamenti di JavaScript</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Acquisire familiarità con ciò che è JavaScript asincrono, come differisce da JavaScript sincrono, e perché ne abbiamo bisogno.</li>
          <li>Cos'è la programmazione sincrona e perché a volte può essere problematica.</li>
          <li>Come la programmazione asincrona mira a risolvere questi problemi.</li>
          <li>Gestori di eventi e funzioni di richiamata, e come si relazionano alla programmazione asincrona.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

La programmazione asincrona è una tecnica che consente al programma di avviare un'attività potenzialmente di lunga durata e rimanere comunque reattivo ad altri eventi mentre quell'attività viene eseguita, piuttosto che dover attendere fino a quando l'attività è terminata. Una volta che l'attività è terminata, il programma riceve il risultato.

Molte funzioni fornite dai browser, specialmente le più interessanti, possono potenzialmente richiedere molto tempo e, quindi, sono asincrone. Ad esempio:

- Fare richieste HTTP utilizzando [`fetch()`](/it/docs/Web/API/Window/fetch)
- Accedere alla fotocamera o al microfono di un utente utilizzando [`getUserMedia()`](/it/docs/Web/API/MediaDevices/getUserMedia)
- Chiedere a un utente di selezionare file utilizzando [`showOpenFilePicker()`](/it/docs/Web/API/Window/showOpenFilePicker)

Quindi, anche se potrebbe non essere necessario _implementare_ spesso funzioni asincrone, è molto probabile che sia necessario _utilizzarle_ correttamente.

In questo articolo, inizieremo esaminando il problema con le funzioni sincrone di lunga durata, che rendono la programmazione asincrona una necessità.

## Programmazione sincrona

Consideri il seguente codice:

```js
const name = "Miriam";
const greeting = `Hello, my name is ${name}!`;
console.log(greeting);
// "Hello, my name is Miriam!"
```

Questo codice:

1. Dichiara una stringa chiamata `name`.
2. Dichiara un'altra stringa chiamata `greeting`, che utilizza `name`.
3. Mostra il saluto sulla console di JavaScript.

Dobbiamo notare qui che il browser esegue efficacemente il programma una riga alla volta, nell'ordine in cui l'abbiamo scritto. In ogni punto, il browser attende che la riga abbia terminato il suo lavoro prima di passare alla riga successiva. Deve farlo perché ogni riga dipende dal lavoro svolto nelle righe precedenti.

Questo rende questo programma **sincrono**. Sarebbe ancora sincrono anche se chiamassimo una funzione separata, in questo modo:

```js
function makeGreeting(name) {
  return `Hello, my name is ${name}!`;
}

const name = "Miriam";
const greeting = makeGreeting(name);
console.log(greeting);
// "Hello, my name is Miriam!"
```

Qui, `makeGreeting()` è una **funzione sincrona** perché il chiamante deve attendere che la funzione termini il suo lavoro e restituisca un valore prima che il chiamante possa continuare.

## Una funzione sincrona di lunga durata

E se la funzione sincrona richiede molto tempo?

Il programma sottostante utilizza un algoritmo molto inefficiente per generare più numeri primi grandi quando un utente fa clic sul pulsante "Genera primi". Più alto è il numero di primi specificato dall'utente, più lungo sarà l'operazione.

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

{{EmbedLiveSample("Una funzione sincrona di lunga durata", 600, 120)}}

Provate a cliccare su "Genera primi". A seconda della velocità del computer, probabilmente ci vorranno alcuni secondi prima che il programma mostri il messaggio "Finito!".

## Il problema delle funzioni sincrone di lunga durata

Il prossimo esempio è simile all'ultimo, tranne che abbiamo aggiunto una casella di testo per digitare. Questa volta, faccia clic su "Genera primi" e provi a digitare nella casella di testo immediatamente dopo.

Scoprirà che mentre la nostra funzione `generatePrimes()` è in esecuzione, il nostro programma è completamente irresponsive: non può digitare nulla, fare clic su nulla o fare qualsiasi altra cosa.

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

{{EmbedLiveSample("Il problema delle funzioni sincrone di lunga durata", 600, 200)}}

La ragione di ciò è che questo programma JavaScript è _single-threaded_. Un thread è una sequenza di istruzioni che un programma segue. Poiché il programma consiste in un singolo thread, può fare solo una cosa alla volta: quindi se sta aspettando che la nostra chiamata sincrona di lunga durata restituisca, non può fare altro.

Ciò di cui abbiamo bisogno è un modo per il nostro programma di:

1. Avviare un'operazione di lunga durata chiamando una funzione.
2. Far sì che quella funzione avvii l'operazione e ritorni immediatamente, in modo che il nostro programma possa ancora rispondere ad altri eventi.
3. Far eseguire l'operazione in un modo che non blocchi il thread principale, ad esempio avviando un nuovo thread.
4. Notificarci il risultato dell'operazione quando finalmente si completa.

Questo è esattamente ciò che le funzioni asincrone ci permettono di fare. Il resto di questo modulo spiega come vengono implementate in JavaScript.

## Gestori di eventi

La descrizione che abbiamo appena visto delle funzioni asincrone potrebbe ricordarle i gestori di eventi, e se è così, avrebbe ragione. I gestori di eventi sono davvero una forma di programmazione asincrona: si fornisce una funzione (il gestore dell'evento) che verrà chiamata non immediatamente, ma ogni volta che si verifica l'evento. Se "l'evento" è "l'operazione asincrona è completata", allora quell'evento potrebbe essere utilizzato per notificare il chiamante sul risultato di una chiamata a funzione asincrona.

Alcune delle prime API asincrone utilizzavano gli eventi proprio in questo modo. L'API [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest) consente di effettuare richieste HTTP a un server remoto utilizzando JavaScript. Poiché ciò può richiedere molto tempo, è un'API asincrona, e si viene notificati sul progresso e sul completamento eventuale di una richiesta allegando ascoltatori di eventi all'oggetto `XMLHttpRequest`.

Il seguente esempio lo mostra in azione. Premi "Clicca per avviare la richiesta" per inviare una richiesta. Creiamo un nuovo [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest) e ascoltiamo il suo evento [`loadend`](/it/docs/Web/API/XMLHttpRequest/loadend_event). Il gestore registra un messaggio "Finito!" insieme al codice di stato.

Dopo aver aggiunto l'ascoltatore di eventi, inviamo la richiesta. Si noti che dopo questo, possiamo registrare "Richiesta XHR iniziata": ciò significa che il nostro programma può continuare a funzionare mentre la richiesta è in corso, e il nostro gestore di eventi verrà chiamato quando la richiesta sarà completa.

```html
<button id="xhr">Click to start request</button>
<button id="reload">Reload</button>

<pre readonly class="event-log"></pre>
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

{{EmbedLiveSample("Gestori di eventi", 600, 120)}}

Questo è un [gestore di eventi](/it/docs/Learn_web_development/Core/Scripting/Events) proprio come i gestori per azioni dell'utente come il clic di un pulsante. Questa volta, tuttavia, l'evento è un cambiamento nello stato di un oggetto.

## Callbacks

Un gestore di eventi è un tipo particolare di callback. Un callback è semplicemente una funzione che viene passata a un'altra funzione, con l'aspettativa che il callback verrà chiamato al momento opportuno. Come abbiamo appena visto, i callback erano un tempo il principale modo in cui le funzioni asincrone erano implementate in JavaScript.

Tuttavia, il codice basato su callback può diventare difficile da comprendere quando il callback stesso deve chiamare funzioni che accettano un callback. Questa è una situazione comune se è necessario eseguire un'operazione che si suddivide in una serie di funzioni asincrone. Ad esempio, consideri quanto segue:

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

Qui abbiamo un'unica operazione suddivisa in tre passaggi, dove ciascun passaggio dipende dall'ultimo passaggio. Nel nostro esempio, il primo passaggio aggiunge 1 all'ingresso, il secondo aggiunge 2, e il terzo aggiunge 3. Partendo da un ingresso di 0, il risultato finale è 6 (0 + 1 + 2 + 3). Come programma sincrono, questo è molto diretto. Ma cosa succede se implementassimo i passaggi utilizzando i callback?

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

Poiché dobbiamo chiamare i callback all'interno di altri callback, otteniamo una funzione `doOperation()` profondamente annidata, che è molto più difficile da leggere e fare debug. Questo è talvolta chiamato "callback hell" o "piramide del destino" (perché l'indentazione assomiglia a una piramide di lato).

Quando si annidano i callback in questo modo, può anche diventare molto difficile gestire gli errori: spesso è necessario gestire gli errori a ciascun livello della "piramide", invece di avere la gestione degli errori una sola volta a livello superiore.

Per questi motivi, la maggior parte delle API asincrone moderne non utilizza i callback. Invece, il fondamento della programmazione asincrona in JavaScript è la {{jsxref("Promise")}}, e questo è l'argomento del prossimo articolo.

{{NextMenu("Learn_web_development/Extensions/Async_JS/Promises", "Learn_web_development/Extensions/Async_JS")}}
