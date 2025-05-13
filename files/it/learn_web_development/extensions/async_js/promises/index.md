---
title: Come usare le promesse
short-title: Uso delle promesse
slug: Learn_web_development/Extensions/Async_JS/Promises
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Async_JS/Introducing", "Learn_web_development/Extensions/Async_JS/Implementing_a_promise-based_API", "Learn_web_development/Extensions/Async_JS")}}

Le **promesse** sono la base della programmazione asincrona nel JavaScript moderno. Una promessa è un oggetto restituito da una funzione asincrona, che rappresenta lo stato corrente dell'operazione. Al momento in cui la promessa viene restituita al chiamante, l'operazione spesso non è terminata, ma l'oggetto promessa fornisce metodi per gestire l'eventuale successo o fallimento dell'operazione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
         Una solida comprensione dei <a href="/it/docs/Learn_web_development/Core/Scripting">fondamenti di JavaScript</a> e dei concetti asincroni, come coperto nelle lezioni precedenti di questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>I concetti e i fondamenti dell'uso delle promesse in JavaScript.</li>
          <li>Collegamento e combinazione di promesse.</li>
          <li>Gestione degli errori nelle promesse.</li>
          <li><code>async</code> e <code>await</code>: come si relazionano alle promesse e perché sono utili.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Nell'[articolo precedente](/it/docs/Learn_web_development/Extensions/Async_JS/Introducing), abbiamo parlato dell'uso dei callback per implementare funzioni asincrone. Con quel design, si chiama la funzione asincrona, passando la propria funzione di callback. La funzione restituisce immediatamente e chiama il callback quando l'operazione è terminata.

Con un'API basata su promesse, la funzione asincrona avvia l'operazione e restituisce un oggetto {{jsxref("Promise")}}. È quindi possibile collegare dei gestori a questo oggetto promessa, e questi gestori verranno eseguiti quando l'operazione è riuscita o fallita.

## Uso dell'API fetch()

> [!NOTE]
> In questo articolo esploreremo le promesse copiando i campioni di codice dalla pagina nella console JavaScript del suo browser. Per configurare questo:
>
> 1. apra una scheda del browser e visiti <https://example.org>
> 2. in quella scheda, apra la console JavaScript negli [strumenti per sviluppatori del suo browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools)
> 3. quando mostriamo un esempio, lo copi nella console. Dovrà ricaricare la pagina ogni volta che inserisce un nuovo esempio, altrimenti la console si lamenterà che ha ridefinito `fetchPromise`.

In questo esempio, scaricheremo il file JSON da <https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json>, e registreremo alcune informazioni su di esso.

Per fare questo, effettueremo una **richiesta HTTP** al server. In una richiesta HTTP, inviamo un messaggio di richiesta a un server remoto, e questo ci risponde con un messaggio di risposta. In questo caso, invieremo una richiesta per ottenere un file JSON dal server. Ricordi l'articolo precedente, dove facevamo richieste HTTP usando l'API [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest)? Bene, in questo articolo, useremo l'API [`fetch()`](/it/docs/Web/API/Window/fetch), che è la moderna sostituzione basata su promesse di `XMLHttpRequest`.

Copia questo nella console JavaScript del suo browser:

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

Qui stiamo:

1. chiamando l'API `fetch()` e assegnando il valore restituito alla variabile `fetchPromise`
2. immediatamente dopo, registrando la variabile `fetchPromise`. Questo dovrebbe produrre qualcosa tipo: `Promise { <state>: "pending" }`, che ci dice che abbiamo un oggetto `Promise`, e ha uno `state` il cui valore è `"pending"`. Lo stato `"pending"` significa che l'operazione di recupero è ancora in corso.
3. passando una funzione gestore nel metodo **`then()`** della Promise. Quando (e se) l'operazione di recupero ha successo, la promessa chiamerà il nostro gestore, passando un oggetto [`Response`](/it/docs/Web/API/Response), che contiene la risposta del server.
4. registrando un messaggio che abbiamo avviato la richiesta.

L'output completo dovrebbe essere qualcosa tipo:

```plain
Promise { <state>: "pending" }
Started request…
Received response: 200
```

Si noti che `Started request…` è registrato prima che riceviamo la risposta. A differenza di una funzione sincrona, `fetch()` ritorna mentre la richiesta è ancora in corso, consentendo al nostro programma di rimanere reattivo. La risposta mostra il [codice di stato](/it/docs/Web/HTTP/Reference/Status) `200` (OK), il che significa che la nostra richiesta è riuscita.

Questo probabilmente sembra molto simile all'esempio nell'articolo precedente, dove abbiamo aggiunto gestori di eventi all'oggetto [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest). Invece di questo, stiamo passando un gestore nel metodo `then()` della promessa restituita.

## Collegamento delle promesse

Con l'API `fetch()`, una volta ottenuto un oggetto `Response`, è necessario chiamare un'altra funzione per ottenere i dati della risposta. In questo caso, vogliamo ottenere i dati della risposta come JSON, quindi chiameremmo il metodo [`json()`](/it/docs/Web/API/Response/json) dell'oggetto `Response`. Si scopre che anche `json()` è asincrono. Quindi, questo è un caso in cui dobbiamo chiamare due funzioni asincrone successive.

Provi questo:

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

Ma aspetti! Ricordi l'articolo scorso, dove abbiamo detto che chiamando un callback all'interno di un altro callback, abbiamo ottenuto livelli di indentazione sempre più annidati? E abbiamo detto che questo "inferno dei callback" rendeva il nostro codice difficile da capire? Non è forse lo stesso, solo con chiamate `then()`?

Certo, è così. Ma la caratteristica elegante delle promesse è che _`then()` stesso restituisce una promessa, che verrà completata con il risultato della funzione passato ad essa_. Questo significa che possiamo (e certamente dobbiamo) riscrivere il codice sopra così:

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

Invece di chiamare il secondo `then()` all'interno del gestore per il primo `then()`, possiamo _restituire_ la promessa restituita da `json()`, e chiamare il secondo `then()` su quel valore di ritorno. Questo si chiama **collegamento delle promesse** e significa che possiamo evitare livelli sempre crescenti di indentazione quando abbiamo bisogno di fare chiamate a funzioni asincrone consecutive.

Prima di passare al prossimo passo, c'è un altro elemento da aggiungere. Dobbiamo verificare che il server abbia accettato e fosse in grado di gestire la richiesta, prima di provare a leggerlo. Lo faremo controllando il codice di stato nella risposta e lanciando un errore se non fosse "OK":

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

## Gestione degli errori

Questo ci porta all'ultimo pezzo: come gestiamo gli errori? L'API `fetch()` può generare un errore per molti motivi (ad esempio, perché non c'era connessione di rete o l'URL era mal formattato in qualche modo) e stiamo lanciando un errore noi stessi se il server ha restituito un errore.

Nell'articolo scorso, abbiamo visto che la gestione degli errori può diventare molto difficile con i callback annidati, costringendoci a gestire gli errori a ogni livello di annidamento.

Per supportare la gestione degli errori, gli oggetti `Promise` forniscono un metodo {{jsxref("Promise/catch", "catch()")}}. Questo è molto simile a `then()`: si chiama e si passa una funzione gestore. Tuttavia, mentre il gestore passato a `then()` viene chiamato quando l'operazione asincrona _ha successo_, il gestore passato a `catch()` viene chiamato quando l'operazione asincrona _fallisce_.

Se aggiunge `catch()` alla fine di una catena di promesse, allora verrà chiamato quando una qualsiasi delle chiamate a funzioni asincrone fallisce. Quindi può implementare un'operazione come diverse chiamate a funzioni asincrone consecutive, e avere un unico luogo per gestire tutti gli errori.

Provi questa versione del nostro codice `fetch()`. Abbiamo aggiunto un gestore di errori usando `catch()`, e abbiamo anche modificato l'URL così la richiesta fallirà.

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

Provi a eseguire questa versione: dovrebbe vedere l'errore registrato dal nostro gestore `catch()`.

## Terminologia delle promesse

Le promesse vengono fornite con una terminologia abbastanza specifica di cui vale la pena chiarire.

In primo luogo, una promessa può trovarsi in uno dei tre stati:

- **pending**: la promessa è stata creata e la funzione asincrona con cui è associata non ha ancora avuto successo né è fallita. Questo è lo stato della sua promessa quando viene restituita da una chiamata a `fetch()`, e la richiesta è ancora in corso.
- **fulfilled**: la funzione asincrona ha avuto successo. Quando una promessa è fulfilled, viene chiamato il suo gestore `then()`.
- **rejected**: la funzione asincrona è fallita. Quando una promessa è rejected, viene chiamato il suo gestore `catch()`.

Si noti che cosa significhi "avere successo" o "fallire" qui dipende dall'API in questione. Ad esempio, `fetch()` rifiuta la promessa restituita se (tra le altre ragioni) un errore di rete ha impedito l'invio della richiesta, ma soddisfa la promessa se il server ha inviato una risposta, anche se la risposta era un errore come [404 Not Found](/it/docs/Web/HTTP/Reference/Status/404).

A volte, utilizziamo il termine **settled** per coprire entrambi i casi, **fulfilled** e **rejected**.

Una promessa è **resolved** se è settled, o se è stata "bloccata" per seguire lo stato di un'altra promessa.

L'articolo [Parliamo di come parlare delle promesse](https://thenewtoys.dev/blog/2021/02/08/lets-talk-about-how-to-talk-about-promises/) dà un'ottima spiegazione dei dettagli di questa terminologia.

## Combinare più promesse

La catena di promesse è ciò di cui ha bisogno quando l'operazione consiste di diverse funzioni asincrone, e ha bisogno che ciascuna finisca prima di iniziare la successiva. Ma ci sono altri modi in cui potrebbe essere necessario combinare le chiamate a funzioni asincrone, e l'API `Promise` fornisce alcuni aiutanti per questo.

A volte, ha bisogno che tutte le promesse siano fullfilled, ma non dipendono l'una dall'altra. In un caso come questo, è molto più efficiente avviarle tutte insieme, quindi essere notificato quando tutte sono state fullfilled. Il metodo {{jsxref("Promise/all", "Promise.all()")}} è ciò di cui ha bisogno qui. Prende un array di promesse e restituisce una singola promessa.

La promessa restituita da `Promise.all()` è:

- fullfilled quando e se _tutte_ le promesse nell'array sono fullfilled. In questo caso, il gestore `then()` viene chiamato con un array di tutte le risposte, nello stesso ordine in cui le promesse sono state passate a `all()`.
- rejected quando e se _una qualsiasi_ delle promesse nell'array è rejected. In questo caso, il gestore `catch()` viene chiamato con l'errore generato dalla promessa che è stata rejected.

Per esempio:

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

Qui, stiamo facendo tre richieste `fetch()` a tre URL diversi. Se tutte hanno successo, registreremo lo stato della risposta di ciascuna. Se una di esse fallisce, registreremo il fallimento.

Con gli URL che abbiamo fornito, tutte le richieste dovrebbero essere fullfilled, anche se per la seconda il server restituirà `404` (Not Found) anziché `200` (OK) perché il file richiesto non esiste. Quindi l'output dovrebbe essere:

```plain
https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json: 200
https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/not-found: 404
https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json: 200
```

Se proviamo lo stesso codice con un URL mal formattato, come questo:

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

Quindi possiamo aspettarci che il gestore `catch()` venga eseguito, e dovremmo vedere qualcosa come:

```plain
Failed to fetch: TypeError: Failed to fetch
```

A volte, potrebbe avere bisogno che una qualsiasi di un set di promesse sia fullfilled, e non le importa quale. In quel caso, vuole {{jsxref("Promise/any", "Promise.any()")}}. Questa è come `Promise.all()`, tranne che si fulfull appena una delle promesse dell'array è fullfilled, o rejected se tutte sono rejected:

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

Si noti che in questo caso non possiamo prevedere quale richiesta fetch verrà completata per prima.

Queste sono solo due delle funzioni extra delle `Promise` per combinare più promesse. Per conoscere il resto, veda la documentazione di riferimento delle {{jsxref("Promise")}}.

## async e await

La parola chiave {{jsxref("Statements/async_function", "async")}} le offre un modo più semplice per lavorare con il codice asincrono basato su promesse. Aggiungere `async` all'inizio di una funzione la rende una funzione asincrona:

```js
async function myFunction() {
  // This is an async function
}
```

Dentro una funzione `async`, può usare la parola chiave `await` prima di una chiamata a una funzione che restituisce una promessa. Questo fa sì che il codice attenda a quel punto fino a che la promessa sia settled, a quel punto il valore fullfilled della promessa è trattato come un valore di ritorno, o il valore rejected è lanciato.

Questo le consente di scrivere codice che usa funzioni asincrone ma che sembra codice sincrono. Ad esempio, potremmo usarlo per riscrivere il nostro esempio di fetch:

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

Possiamo persino usare un blocco `try...catch` per la gestione degli errori, esattamente come faremmo se il codice fosse sincrono.

Si noti però che le funzioni `async` restituiscono sempre una promessa, quindi non può fare qualcosa tipo:

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

Invece, dovrebbe fare qualcosa tipo:

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

Qui, abbiamo spostato il `try...catch` indietro al gestore `catch` sulla promessa restituita. Questo significa che il nostro gestore `then` non deve gestire il caso in cui un errore è stato catturato all'interno della funzione `fetchProducts`, causando che `data` sia `undefined`. Gestire gli errori come l'ultimo passo della catena di promesse.

Inoltre, noti che può usare `await` solo all'interno di una funzione `async`, a meno che il codice non si trovi in un [modulo JavaScript](/it/docs/Web/JavaScript/Guide/Modules). Ciò significa che non può fare questo in uno script normale:

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

Probabilmente userà le funzioni `async` molto dove altrimenti userebbe catene di promesse, e rendono il lavoro con le promesse molto più intuitivo.

Tenga presente che proprio come una catena di promesse, `await` forza le operazioni asincrone a essere completate in serie. Questo è necessario se il risultato della prossima operazione dipende dal risultato dell'ultima, ma se non è così allora qualcosa tipo `Promise.all()` sarà più performante.

## Sommario

Le promesse sono la base della programmazione asincrona nel JavaScript moderno. Rendono più facile esprimere e ragionare su sequenze di operazioni asincrone senza callback profondamente annidati, e supportano uno stile di gestione degli errori che è simile alla dichiarazione sincrona `try...catch`.

Le parole chiave `async` e `await` rendono più facile costruire un'operazione da una serie di chiamate a funzioni asincrone consecutive, evitando la necessità di creare catene di promesse esplicite, e consentendole di scrivere codice che sembra proprio come il codice sincrono.

Le promesse funzionano nelle versioni più recenti di tutti i browser moderni; l'unico luogo in cui il supporto per le promesse sarà un problema è in Opera Mini e IE11 e versioni precedenti.

Non abbiamo toccato tutte le funzionalità delle promesse in questo articolo, solo quelle più interessanti e utili. Mentre inizia a imparare di più sulle promesse, incontrerà più caratteristiche e tecniche.

Molte API moderne del Web sono basate su promesse, inclusi [WebRTC](/it/docs/Web/API/WebRTC_API), [Web Audio API](/it/docs/Web/API/Web_Audio_API), [Media Capture and Streams API](/it/docs/Web/API/Media_Capture_and_Streams_API), e molte altre.

## Vedere anche

- [`Promise()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Uso delle promesse](/it/docs/Web/JavaScript/Guide/Using_promises)
- [Abbiamo un problema con le promesse](https://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html) di Nolan Lawson
- [Parliamo di come parlare delle promesse](https://thenewtoys.dev/blog/2021/02/08/lets-talk-about-how-to-talk-about-promises/)

{{PreviousMenuNext("Learn_web_development/Extensions/Async_JS/Introducing", "Learn_web_development/Extensions/Async_JS/Implementing_a_promise-based_API", "Learn_web_development/Extensions/Async_JS")}}
