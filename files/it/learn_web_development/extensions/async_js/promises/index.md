---
title: Come usare le promesse
short-title: Usare le promesse
slug: Learn_web_development/Extensions/Async_JS/Promises
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Async_JS/Introducing", "Learn_web_development/Extensions/Async_JS/Implementing_a_promise-based_API", "Learn_web_development/Extensions/Async_JS")}}

Le **promesse** sono la base della programmazione asincrona nel JavaScript moderno. Una promessa è un oggetto restituito da una funzione asincrona, che rappresenta lo stato attuale dell'operazione. Al momento in cui la promessa viene restituita al chiamante, l'operazione spesso non è ancora terminata, ma l'oggetto promessa fornisce metodi per gestire il successo o il fallimento dell'operazione eventuale.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una solida comprensione dei <a href="/it/docs/Learn_web_development/Core/Scripting">fondamenti di JavaScript</a> e dei concetti asincroni, come trattati nelle lezioni precedenti in questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>I concetti e i fondamenti dell'uso delle promesse in JavaScript.</li>
          <li>Collegamento e combinazione di promesse.</li>
          <li>Gestione degli errori nelle promesse.</li>
          <li><code>async</code> e <code>await</code>: come si relazionano alle promesse, e perché sono utili.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Nell'[articolo precedente](/it/docs/Learn_web_development/Extensions/Async_JS/Introducing), abbiamo parlato dell'uso dei callback per implementare funzioni asincrone. Con quel design, si chiama la funzione asincrona, passando la funzione di callback. La funzione restituisce immediatamente e chiama il callback quando l'operazione è terminata.

Con un'API basata su promesse, la funzione asincrona avvia l'operazione e restituisce un oggetto {{jsxref("Promise")}}. È possibile quindi attaccare i gestori a questo oggetto promessa, e questi gestori saranno eseguiti quando l'operazione avrà successo o fallirà.

## Utilizzo dell'API fetch()

> [!NOTE]
> In questo articolo, esploreremo le promesse copiando esempi di codice dalla pagina nella console JavaScript del browser. Per configurare questo:
>
> 1. aprire una scheda del browser e visitare <https://example.org>
> 2. in quella scheda, aprire la console JavaScript nei [strumenti per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools)
> 3. quando mostriamo un esempio, copiarlo nella console. Dovrai ricaricare la pagina ogni volta che inserisci un nuovo esempio, altrimenti la console si lamenterà che hai ridefinito `fetchPromise`.

In questo esempio, scaricheremo il file JSON da <https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json>, e registreremo alcune informazioni su di esso.

Per fare ciò, effettueremo una **richiesta HTTP** al server. In una richiesta HTTP, inviamo un messaggio di richiesta a un server remoto, e questo ci invia una risposta. In questo caso, invieremo una richiesta per ottenere un file JSON dal server. Ricorda l'articolo precedente, dove abbiamo effettuato richieste HTTP usando l'API [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest)? In questo articolo, useremo l'API [`fetch()`](/it/docs/Web/API/Window/fetch), che è il sostituto moderno basato su promesse per `XMLHttpRequest`.

Copia questo nella console JavaScript del tuo browser:

```js
const fetchPromise = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);

console.log(fetchPromise);

fetchPromise.then((response) => {
  console.log(`Received response: ${response.status}`);
});

console.log("Started request…");
```

Ecco cosa facciamo:

1. chiamiamo l'API `fetch()`, e assegniamo il valore di ritorno alla variabile `fetchPromise`
2. subito dopo, registriamo la variabile `fetchPromise`. Questo dovrebbe mostrare qualcosa come: `Promise { <state>: "pending" }`, indicando che abbiamo un oggetto `Promise`, e ha uno `state` il cui valore è `"pending"`. Lo stato `"pending"` significa che l'operazione di fetch è ancora in corso.
3. passare una funzione gestore nel metodo **`then()`** della promessa. Quando (e se) l'operazione di fetch ha successo, la promessa chiamerà il nostro gestore, passando un oggetto [`Response`](/it/docs/Web/API/Response), che contiene la risposta del server.
4. registriamo un messaggio che abbiamo avviato la richiesta.

L'output completo dovrebbe essere qualcosa di simile:

```plain
Promise { <state>: "pending" }
Started request…
Received response: 200
```

Nota che `Started request…` viene registrato prima di ricevere la risposta. Contrariamente a una funzione sincrona, `fetch()` restituisce mentre la richiesta è ancora in corso, consentendo al nostro programma di rimanere reattivo. La risposta mostra il [codice di stato](/it/docs/Web/HTTP/Reference/Status) `200` (OK), il che significa che la nostra richiesta ha avuto successo.

Questo probabilmente sembra molto simile all'esempio dell'articolo precedente, dove abbiamo aggiunto gestori di eventi all'oggetto [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest). Invece di quello, stiamo passando un gestore nel metodo `then()` della promessa restituita.

## Collegamento delle promesse

Con l'API `fetch()`, una volta ottenuto un oggetto `Response`, è necessario chiamare un'altra funzione per ottenere i dati della risposta. In questo caso, vogliamo ottenere i dati della risposta come JSON, quindi chiameremmo il metodo [`json()`](/it/docs/Web/API/Response/json) dell'oggetto `Response`. Si scopre che `json()` è anche asincrono. Quindi questo è un caso in cui dobbiamo chiamare due funzioni asincrone successive.

Prova questo:

```js
const fetchPromise = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);

fetchPromise.then((response) => {
  const jsonPromise = response.json();
  jsonPromise.then((data) => {
    console.log(data[0].name);
  });
});
```

In questo esempio, come prima, aggiungiamo un gestore `then()` alla promessa restituita da `fetch()`. Ma questa volta, il nostro gestore chiama `response.json()`, e poi passa un nuovo gestore `then()` nella promessa restituita da `response.json()`.

Questo dovrebbe registrare "baked beans" (il nome del primo prodotto elencato in "products.json").

Ma aspetta! Ricorda l'articolo scorso, dove abbiamo detto che chiamando un callback dentro un altro callback, abbiamo ottenuto livelli di codice successivamente più annidati? E abbiamo detto che questo "inferno dei callback" rendeva il nostro codice difficile da capire? Non è la stessa cosa, solo con le chiamate `then()`?

Effettivamente lo è. Ma la caratteristica elegante delle promesse è che _`then()` stesso restituisce una promessa, che sarà completata con il risultato della funzione passata a essa_. Questo significa che possiamo (e certamente dovremmo) riscrivere il codice sopra in questo modo:

```js
const fetchPromise = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);

fetchPromise
  .then((response) => response.json())
  .then((data) => {
    console.log(data[0].name);
  });
```

Invece di chiamare il secondo `then()` all'interno del gestore per il primo `then()`, possiamo _restituire_ la promessa restituita da `json()`, e chiamare il secondo `then()` su quel valore di ritorno. Questo è chiamato **collegamento delle promesse** e significa che possiamo evitare livelli di indentazione sempre maggiori quando dobbiamo effettuare chiamate a funzioni asincrone consecutive.

Prima di passare al passaggio successivo, c'è un altro pezzo da aggiungere. Dobbiamo verificare che il server abbia accettato e sia stato in grado di gestire la richiesta, prima di provare a leggerla. Faremo questo controllando il codice di stato nella risposta e lanciando un errore se non era "OK":

```js
const fetchPromise = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);

fetchPromise
  .then((response) => {
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    return response.json();
  })
  .then((data) => {
    console.log(data[0].name);
  });
```

## Catturare errori

Questo ci porta all'ultimo pezzo: come gestiamo gli errori? L'API `fetch()` può lanciare un errore per molte ragioni (ad esempio, perché non c'era connettività di rete o l'URL è stato scritto in modo errato) e stiamo lanciando un errore noi stessi se il server ha restituito un errore.

Nell'articolo precedente, abbiamo visto che la gestione degli errori può diventare molto difficile con i callback annidati, costringendoci a gestire gli errori a ogni livello di annidamento.

Per supportare la gestione degli errori, gli oggetti `Promise` forniscono un metodo {{jsxref("Promise/catch", "catch()")}}. Questo è molto simile a `then()`: lo chiami e passi una funzione gestore. Tuttavia, mentre il gestore passato a `then()` viene chiamato quando l'operazione asincrona _ha successo_, il gestore passato a `catch()` viene chiamato quando l'operazione asincrona _fallisce_.

Se aggiungi `catch()` alla fine di una catena di promesse, allora verrà chiamato quando una qualsiasi delle chiamate a funzioni asincrone fallisce. Quindi puoi implementare un'operazione come diverse chiamate a funzioni asincrone consecutive, e avere un unico luogo per gestire tutti gli errori.

Prova questa versione del nostro codice `fetch()`. Abbiamo aggiunto un gestore di errori usando `catch()`, e modificato anche l'URL in modo che la richiesta fallisca.

```js
const fetchPromise = fetch(
  "bad-scheme://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);

fetchPromise
  .then((response) => {
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    return response.json();
  })
  .then((data) => {
    console.log(data[0].name);
  })
  .catch((error) => {
    console.error(`Could not get products: ${error}`);
  });
```

Prova ad eseguire questa versione: dovresti vedere l'errore registrato dal nostro gestore `catch()`.

## Terminologia delle promesse

Le promesse hanno una terminologia piuttosto specifica che vale la pena chiarire.

Per prima cosa, una promessa può essere in uno dei tre stati:

- **pending**: la promessa è stata creata e la funzione asincrona con cui è associata non ha ancora avuto successo o fallito. Questo è lo stato in cui si trova la tua promessa quando è restituita da una chiamata a `fetch()`, e la richiesta è ancora in corso.
- **fulfilled**: la funzione asincrona ha avuto successo. Quando una promessa è fulfilled, il suo gestore `then()` viene chiamato.
- **rejected**: la funzione asincrona è fallita. Quando una promessa è rejected, il suo gestore `catch()` viene chiamato.

Nota che cosa significa "avere successo" o "fallire" qui dipende dall'API in questione. Ad esempio, `fetch()` rifiuta la promessa restituita se (tra le altre ragioni) un errore di rete ha impedito l'invio della richiesta, ma completa la promessa se il server ha inviato una risposta, anche se la risposta era un errore come [404 Not Found](/it/docs/Web/HTTP/Reference/Status/404).

A volte, usiamo il termine **settled** per coprire sia **fulfilled** che **rejected**.

Una promessa è **resolved** se è settled, o se è stata "bloccata" per seguire lo stato di un'altra promessa.

L'articolo [Let's talk about how to talk about promises](https://thenewtoys.dev/blog/2021/02/08/lets-talk-about-how-to-talk-about-promises/) offre una grande spiegazione dei dettagli di questa terminologia.

## Combinazione di più promesse

La catena di promesse è ciò di cui hai bisogno quando la tua operazione consiste di diverse funzioni asincrone, e hai bisogno che ciascuna completa prima di iniziare la successiva. Ma ci sono altri modi in cui potresti aver bisogno di combinare chiamate a funzioni asincrone, e l'API `Promise` fornisce alcuni aiutanti per loro.

A volte, hai bisogno che tutte le promesse siano complete, ma non dipendono l'una dall'altra. In un caso come questo, è molto più efficiente avviarle tutte insieme, e poi essere notificato quando tutte sono complete. Il metodo {{jsxref("Promise/all", "Promise.all()")}} è ciò di cui hai bisogno qui. Prende un array di promesse e restituisce una singola promessa.

La promessa restituita da `Promise.all()` è:

- completa quando e se _tutte_ le promesse nell'array sono complete. In questo caso, il gestore `then()` viene chiamato con un array di tutte le risposte, nello stesso ordine in cui le promesse sono state passate in `all()`.
- rifiutata quando e se _una qualsiasi_ delle promesse nell'array viene rifiutata. In questo caso, il gestore `catch()` viene chiamato con l'errore lanciato dalla promessa che è stata rifiutata.

Ad esempio:

```js
const fetchPromise1 = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);
const fetchPromise2 = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/not-found",
);
const fetchPromise3 = fetch(
  "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json",
);

Promise.all([fetchPromise1, fetchPromise2, fetchPromise3])
  .then((responses) => {
    for (const response of responses) {
      console.log(`${response.url}: ${response.status}`);
    }
  })
  .catch((error) => {
    console.error(`Failed to fetch: ${error}`);
  });
```

Qui stiamo effettuando tre richieste `fetch()` a tre URL diversi. Se tutte hanno successo, registreremo lo stato della risposta di ciascuna. Se una di loro fallisce, allora registreremo il fallimento.

Con gli URL che abbiamo fornito, tutte le richieste dovrebbero essere complete, anche se per la seconda il server restituirà `404` (Not Found) invece di `200` (OK) perché il file richiesto non esiste. Quindi l'output dovrebbe essere:

```plain
https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json: 200
https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/not-found: 404
https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json: 200
```

Se proviamo lo stesso codice con un URL mal formato, come questo:

```js
const fetchPromise1 = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);
const fetchPromise2 = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/not-found",
);
const fetchPromise3 = fetch(
  "bad-scheme://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json",
);

Promise.all([fetchPromise1, fetchPromise2, fetchPromise3])
  .then((responses) => {
    for (const response of responses) {
      console.log(`${response.url}: ${response.status}`);
    }
  })
  .catch((error) => {
    console.error(`Failed to fetch: ${error}`);
  });
```

Allora possiamo aspettarci che il gestore `catch()` venga eseguito, e dovremmo vedere qualcosa di simile:

```plain
Failed to fetch: TypeError: Failed to fetch
```

A volte, potresti aver bisogno che una qualsiasi delle promesse sia completa, e non importa quale. In tal caso, vuoi {{jsxref("Promise/any", "Promise.any()")}}. Questo è come `Promise.all()`, eccetto che è completo non appena una qualsiasi delle promesse dell'array è completa, o rifiutato se tutte sono rifiutate:

```js
const fetchPromise1 = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);
const fetchPromise2 = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/not-found",
);
const fetchPromise3 = fetch(
  "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json",
);

Promise.any([fetchPromise1, fetchPromise2, fetchPromise3])
  .then((response) => {
    console.log(`${response.url}: ${response.status}`);
  })
  .catch((error) => {
    console.error(`Failed to fetch: ${error}`);
  });
```

Nota che in questo caso non possiamo prevedere quale richiesta fetch verrà completata per prima.

Questi sono solo due delle funzioni aggiuntive `Promise` per combinare più promesse. Per saperne di più sul resto, vedere la documentazione di riferimento di {{jsxref("Promise")}}.

## async e await

La parola chiave {{jsxref("Statements/async_function", "async")}} ti offre un modo più semplice di lavorare con il codice basato su promesse asincrone. Aggiungere `async` all'inizio di una funzione la rende una funzione asincrona:

```js
async function myFunction() {
  // This is an async function
}
```

All'interno di una funzione asincrona, puoi usare la parola chiave `await` prima di una chiamata a una funzione che restituisce una promessa. Questo fa sì che il codice aspetti in quel punto fino a quando la promessa non è completata, a quel punto il valore completato della promessa viene trattato come un valore di ritorno, o il valore rifiutato viene lanciato.

Questo ti consente di scrivere codice che utilizza funzioni asincrone ma sembra codice sincrono. Ad esempio, potremmo usarlo per riscrivere il nostro esempio di fetch:

```js
async function fetchProducts() {
  try {
    // after this line, our function will wait for the `fetch()` call to be settled
    // the `fetch()` call will either return a Response or throw an error
    const response = await fetch(
      "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
    );
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    // after this line, our function will wait for the `response.json()` call to be settled
    // the `response.json()` call will either return the parsed JSON object or throw an error
    const data = await response.json();
    console.log(data[0].name);
  } catch (error) {
    console.error(`Could not get products: ${error}`);
  }
}

fetchProducts();
```

Qui, stiamo chiamando `await fetch()`, e invece di ottenere una `Promise`, il nostro chiamante ottiene indietro un oggetto `Response` completamente completo, proprio come se `fetch()` fosse una funzione sincrona!

Possiamo persino usare un blocco `try...catch` per gestire gli errori, esattamente come faremmo se il codice fosse sincrono.

Nota però che le funzioni asincrone restituiscono sempre una promessa, quindi non puoi fare qualcosa come:

```js example-bad
async function fetchProducts() {
  try {
    const response = await fetch(
      "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
    );
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    const data = await response.json();
    return data;
  } catch (error) {
    console.error(`Could not get products: ${error}`);
  }
}

const promise = fetchProducts();
console.log(promise[0].name); // "promise" is a Promise object, so this will not work
```

Invece, dovresti fare qualcosa come:

```js
async function fetchProducts() {
  const response = await fetch(
    "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
  );
  if (!response.ok) {
    throw new Error(`HTTP error: ${response.status}`);
  }
  const data = await response.json();
  return data;
}

const promise = fetchProducts();
promise
  .then((data) => {
    console.log(data[0].name);
  })
  .catch((error) => {
    console.error(`Could not get products: ${error}`);
  });
```

Qui, abbiamo spostato il `try...catch` indietro al gestore `catch` sulla promessa restituita. Questo significa che il nostro gestore `then` non deve gestire il caso in cui un errore è stato catturato all'interno della funzione `fetchProducts`, causando che `data` fosse `undefined`. Gestisci gli errori come ultimo passaggio della tua catena di promesse.

Nota anche che puoi usare `await` solo all'interno di una funzione `async`, a meno che il tuo codice non sia in un [modulo JavaScript](/it/docs/Web/JavaScript/Guide/Modules). Ciò significa che non puoi fare questo in uno script normale:

```js
try {
  // using await outside an async function is only allowed in a module
  const response = await fetch(
    "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
  );
  if (!response.ok) {
    throw new Error(`HTTP error: ${response.status}`);
  }
  const data = await response.json();
  console.log(data[0].name);
} catch (error) {
  console.error(`Could not get products: ${error}`);
  throw error;
}
```

Probabilmente userai molto le funzioni `async` dove altrimenti useresti le catene di promesse, e rendono il lavoro con le promesse molto più intuitivo.

Tieni presente che proprio come una catena di promesse, `await` forza il completamento delle operazioni asincrone in serie. Questo è necessario se il risultato dell'operazione successiva dipende dal risultato dell'ultima, ma se non è così, allora qualcosa come `Promise.all()` sarà più performante.

## Sommario

Le promesse sono la base della programmazione asincrona nel JavaScript moderno. Rendono più facile esprimere e ragionare sulle sequenze di operazioni asincrone senza callback profondamente annidati e supportano uno stile di gestione degli errori simile alla dichiarazione sincrona `try...catch`.

Le parole chiave `async` e `await` rendono più facile costruire un'operazione da una serie di chiamate a funzioni asincrone consecutive, evitando la necessità di creare esplicitamente catene di promesse, e consentendo di scrivere codice che sembra codice sincrono.

Le promesse funzionano nelle ultime versioni di tutti i browser moderni; l'unico posto dove il supporto alle promesse sarà un problema è in Opera Mini e IE11 e versioni precedenti.

Non abbiamo trattato tutte le funzionalità delle promesse in questo articolo, solo le più interessanti e utili. Man mano che inizi a saperne di più sulle promesse, incontrerai più funzionalità e tecniche.

Molte moderne API Web sono basate su promesse, inclusi [WebRTC](/it/docs/Web/API/WebRTC_API), [Web Audio API](/it/docs/Web/API/Web_Audio_API), [Media Capture and Streams API](/it/docs/Web/API/Media_Capture_and_Streams_API), e molte altre.

## Vedi anche

- [`Promise()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Usare le promesse](/it/docs/Web/JavaScript/Guide/Using_promises)
- [Abbiamo un problema con le promesse](https://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html) di Nolan Lawson
- [Let's talk about how to talk about promises](https://thenewtoys.dev/blog/2021/02/08/lets-talk-about-how-to-talk-about-promises/)

{{PreviousMenuNext("Learn_web_development/Extensions/Async_JS/Introducing", "Learn_web_development/Extensions/Async_JS/Implementing_a_promise-based_API", "Learn_web_development/Extensions/Async_JS")}}
