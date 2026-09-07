---
title: Come usare le promise
short-title: Uso delle promise
slug: Learn_web_development/Extensions/Async_JS/Promises
l10n:
  sourceCommit: 7dea5a04405d1bfce4f471d9284f7f7148bc9a4d
---

{{PreviousMenuNext("Learn_web_development/Extensions/Async_JS/Introducing", "Learn_web_development/Extensions/Async_JS/Implementing_a_promise-based_API", "Learn_web_development/Extensions/Async_JS")}}

Le **promise** sono alla base della programmazione asincrona nel JavaScript moderno. Una promise è un oggetto restituito da una funzione asincrona, che rappresenta lo stato corrente dell'operazione. Quando la promise viene restituita al chiamante, l'operazione spesso non è ancora terminata, ma l'oggetto promise fornisce metodi per gestire l'eventuale successo o fallimento dell'operazione.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
         Una solida comprensione dei <a href="/it/docs/Learn_web_development/Core/Scripting">fondamenti di JavaScript</a> e dei concetti asincroni, come trattati nelle lezioni precedenti di questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>I concetti e i fondamenti dell'uso delle promise in JavaScript.</li>
          <li>Concatenare e combinare le promise.</li>
          <li>Gestire gli errori nelle promise.</li>
          <li><code>async</code> e <code>await</code>: come si relazionano alle promise e perché sono utili.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Nell'[articolo precedente](/it/docs/Learn_web_development/Extensions/Async_JS/Introducing), abbiamo parlato dell'uso delle callback per implementare funzioni asincrone. Con questa progettazione, si chiama la funzione asincrona passando la propria funzione callback. La funzione restituisce immediatamente e chiama la callback al termine dell'operazione.

Con un'API basata su promise, la funzione asincrona avvia l'operazione e restituisce un oggetto {{jsxref("Promise")}}. È quindi possibile collegare gestori a questo oggetto promise, che verranno eseguiti quando l'operazione ha avuto successo o è fallita.

## Uso dell'API fetch()

> [!NOTE]
> In questo articolo esploreremo le promise copiando esempi di codice dalla pagina nella console JavaScript del browser. Per preparare l'ambiente:
>
> 1. aprire una scheda del browser e visitare <https://example.org>
> 2. in quella scheda, aprire la console JavaScript negli [strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools)
> 3. quando viene mostrato un esempio, copiarlo nella console. Sarà necessario ricaricare la pagina ogni volta che viene inserito un nuovo esempio, altrimenti la console segnalerà che `fetchPromise` è stato dichiarato nuovamente.

In questo esempio verrà scaricato il file JSON da <https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json> e verranno registrate alcune informazioni a riguardo.

A questo scopo, verrà effettuata una **richiesta HTTP** al server. In una richiesta HTTP, viene inviato un messaggio di richiesta a un server remoto, che restituisce una risposta. In questo caso, verrà inviata una richiesta per ottenere un file JSON dal server. Si ricorda l'ultimo articolo, in cui sono state effettuate richieste HTTP usando l'API [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest)? In questo articolo verrà invece usata l'API [`fetch()`](/it/docs/Web/API/Window/fetch), il moderno sostituto basato su promise di `XMLHttpRequest`.

Copiare questo nella console JavaScript del browser:

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

Qui vengono eseguite le seguenti operazioni:

1. viene chiamata l'API `fetch()` e il valore restituito viene assegnato alla variabile `fetchPromise`
2. immediatamente dopo, viene registrata la variabile `fetchPromise`. L'output dovrebbe essere simile a: `Promise { <state>: "pending" }`, indicando la presenza di un oggetto `Promise` con uno `state` dal valore `"pending"`. Lo stato `"pending"` significa che l'operazione di recupero è ancora in corso.
3. viene passata una funzione gestore al metodo **`then()`** della Promise. Quando (e se) l'operazione di recupero ha successo, la promise chiamerà il gestore passando un oggetto [`Response`](/it/docs/Web/API/Response), che contiene la risposta del server.
4. viene registrato un messaggio per indicare che la richiesta è stata avviata.

L'output completo dovrebbe essere simile a:

```plain
Promise { <state>: "pending" }
Started request…
Received response: 200
```

Si noti che `Started request…` viene registrato prima di ricevere la risposta. A differenza di una funzione sincrona, `fetch()` restituisce mentre la richiesta è ancora in corso, permettendo al programma di rimanere reattivo. La risposta mostra il [codice di stato](/it/docs/Web/HTTP/Reference/Status) `200` (OK), che significa che la richiesta ha avuto successo.

Questo probabilmente sembra molto simile all'esempio dell'ultimo articolo, in cui sono stati aggiunti gestori di eventi all'oggetto [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest). Invece di farlo, qui viene passato un gestore al metodo `then()` della promise restituita.

## Concatenare le promise

Con l'API `fetch()`, una volta ottenuto un oggetto `Response`, è necessario chiamare un'altra funzione per ottenere i dati della risposta. In questo caso, si vogliono ottenere i dati della risposta come JSON, quindi si chiamerebbe il metodo [`json()`](/it/docs/Web/API/Response/json) dell'oggetto `Response`. Anche `json()` è asincrono. Questo è quindi un caso in cui occorre chiamare due funzioni asincrone successive.

Provare questo:

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

In questo esempio, come in precedenza, viene aggiunto un gestore `then()` alla promise restituita da `fetch()`. Questa volta, però, il gestore chiama `response.json()` e poi passa un nuovo gestore `then()` alla promise restituita da `response.json()`.

Questo dovrebbe registrare "baked beans" (il nome del primo prodotto elencato in "products.json").

Ma attenzione. Si ricorda l'ultimo articolo, in cui è stato detto che chiamando una callback all'interno di un'altra callback si ottenevano livelli di codice sempre più annidati? Ed è stato detto che questo "callback hell" rendeva il codice difficile da comprendere? Non è forse la stessa cosa, solo con chiamate a `then()`?

Naturalmente lo è. Ma la caratteristica elegante delle promise è che `then()` restituisce a sua volta una nuova promise che viene soddisfatta con il valore restituito dalla funzione callback (a condizione che la funzione venga eseguita correttamente). Ciò significa che è possibile, e certamente opportuno, riscrivere il codice precedente in questo modo:

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

Invece di chiamare il secondo `then()` all'interno del gestore per il primo `then()`, è possibile _restituire_ la promise restituita da `json()` e chiamare il secondo `then()` su quel valore restituito. Questo si chiama **concatenamento di promise** e consente di evitare livelli di rientro sempre maggiori quando è necessario effettuare chiamate consecutive a funzioni asincrone.

Prima di passare al passaggio successivo, c'è ancora un elemento da aggiungere. Occorre verificare che il server abbia accettato e sia stato in grado di gestire la richiesta, prima di tentare di leggerla. Questo avverrà controllando il codice di stato nella risposta e generando un errore se non è "OK":

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

## Intercettare gli errori

Questo porta all'ultima parte: come vengono gestiti gli errori? L'API `fetch()` può generare un errore per molte ragioni, ad esempio perché non è disponibile la connettività di rete oppure perché l'URL non è valido in qualche modo; inoltre viene generato un errore direttamente se il server restituisce un errore.

Nell'ultimo articolo è stato visto che la gestione degli errori può diventare molto difficile con callback annidate, rendendo necessario gestire gli errori a ogni livello di annidamento.

Per supportare la gestione degli errori, gli oggetti `Promise` forniscono un metodo {{jsxref("Promise/catch", "catch()")}}. Questo è molto simile a `then()`: viene chiamato passando una funzione gestore. Tuttavia, mentre il gestore passato a `then()` viene chiamato quando l'operazione asincrona _riesce_, quello passato a `catch()` viene chiamato quando l'operazione asincrona _fallisce_.

Se si aggiunge `catch()` alla fine di una catena di promise, verrà chiamato quando una qualsiasi delle chiamate a funzioni asincrone fallisce. È quindi possibile implementare un'operazione come una serie di chiamate consecutive a funzioni asincrone e disporre di un unico punto per gestire tutti gli errori.

Provare questa versione del codice `fetch()`. È stato aggiunto un gestore di errori usando `catch()` ed è stato anche modificato l'URL affinché la richiesta fallisca.

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

Provando a eseguire questa versione, dovrebbe essere visualizzato l'errore registrato dal gestore `catch()`.

## Terminologia delle promise

Le promise hanno una terminologia piuttosto specifica che vale la pena chiarire.

Innanzitutto, una promise può trovarsi in uno di tre stati:

- **pending**: lo stato iniziale. L'operazione non è ancora stata completata, né con successo né con fallimento.
- **fulfilled**: l'operazione ha avuto successo. Questo è il momento in cui viene chiamato il gestore `.then()` della promise.
- **rejected**: l'operazione è fallita. Questo è il momento in cui viene chiamato il gestore `.catch()` della promise.

Si noti che il significato di "successo" o "fallimento" dipende dall'API in questione. Ad esempio, `fetch()` rifiuta la promise restituita se, tra le altre ragioni, un errore di rete ha impedito l'invio della richiesta, ma soddisfa la promise se il server ha inviato una risposta, anche se quest'ultima è un errore come [404 Not Found](/it/docs/Web/HTTP/Reference/Status/404).

Vengono inoltre usati alcuni altri termini per descrivere lo stato di una promise:

- **completed**: la promise non è più pending; è stata fulfilled oppure rejected.
- **resolved**: la promise è completed oppure è stata "vincolata" a seguire lo stato di un'altra promise. Si tratta di un concetto più avanzato, rilevante quando una promise dipende da un'altra.

L'articolo [Let's talk about how to talk about promises](https://thenewtoys.dev/blog/2021/02/08/lets-talk-about-how-to-talk-about-promises/) offre un'ottima spiegazione dei dettagli di questa terminologia.

## Combinare più promise

La catena di promise è ciò che serve quando un'operazione è composta da più funzioni asincrone e ciascuna deve completarsi prima di avviare la successiva. Ma potrebbero essere necessarie altre modalità per combinare chiamate a funzioni asincrone e l'API `Promise` fornisce alcuni strumenti di supporto a questo scopo.

Talvolta è necessario che tutte le promise siano fulfilled, ma non dipendono l'una dall'altra. In un caso simile, è molto più efficiente avviarle tutte insieme e ricevere una notifica quando sono state tutte fulfilled. Il metodo {{jsxref("Promise/all", "Promise.all()")}} è ciò che serve in questo caso. Accetta un array di promise e restituisce una singola promise.

La promise restituita da `Promise.all()` è:

- fulfilled quando e se _tutte_ le promise nell'array sono fulfilled. In questo caso, il gestore `then()` viene chiamato con un array di tutte le risposte, nello stesso ordine in cui le promise sono state passate a `all()`.
- rejected quando e se _una qualsiasi_ delle promise nell'array è rejected. In questo caso, il gestore `catch()` viene chiamato con l'errore generato dalla promise che è stata rejected.

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

Qui vengono effettuate tre richieste `fetch()` a tre URL diversi. Se hanno tutte successo, verrà registrato lo stato della risposta di ciascuna. Se una qualsiasi di esse fallisce, verrà registrato il fallimento.

Con gli URL forniti, tutte le richieste dovrebbero essere fulfilled, anche se per la seconda il server restituirà `404` (Not Found) invece di `200` (OK), poiché il file richiesto non esiste. L'output dovrebbe quindi essere:

```plain
https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json: 200
https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/not-found: 404
https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json: 200
```

Se si prova lo stesso codice con un URL non valido, come questo:

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

Allora è possibile aspettarsi che venga eseguito il gestore `catch()` e dovrebbe essere visualizzato qualcosa di simile a:

```plain
Failed to fetch: TypeError: Failed to fetch
```

Talvolta potrebbe essere necessario che una qualsiasi promise di un insieme sia fulfilled, senza che importi quale. In questo caso, serve {{jsxref("Promise/any", "Promise.any()")}}. È simile a `Promise.all()`, tranne per il fatto che viene fulfilled non appena una qualsiasi delle promise nell'array è fulfilled, oppure rejected se sono tutte rejected:

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

Si noti che in questo caso non è possibile prevedere quale richiesta fetch verrà completata per prima.

Queste sono solo due delle funzioni `Promise` aggiuntive per combinare più promise. Per conoscere le altre, consultare la documentazione di riferimento {{jsxref("Promise")}}.

## async e await

La parola chiave {{jsxref("Statements/async_function", "async")}} offre un modo più semplice per lavorare con codice asincrono basato su promise. L'aggiunta di `async` all'inizio di una funzione la rende una funzione async:

```js
async function myFunction() {
  // This is an async function
}
```

All'interno di una funzione async, è possibile usare la parola chiave `await` prima di una chiamata a una funzione che restituisce una promise. Questo fa sì che il codice attenda in quel punto finché la promise non è completata; a quel punto, il valore fulfilled della promise viene trattato come valore di restituzione oppure il valore rejected viene generato.

Ciò consente di scrivere codice che usa funzioni asincrone ma che sembra codice sincrono. Ad esempio, potrebbe essere usato per riscrivere l'esempio `fetch`:

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

Qui viene chiamato `await fetch()` e, invece di ottenere una `Promise`, il chiamante riceve un oggetto `Response` completamente elaborato, proprio come se `fetch()` fosse una funzione sincrona.

È persino possibile usare un blocco `try...catch` per la gestione degli errori, esattamente come si farebbe se il codice fosse sincrono.

Si noti però che le funzioni async restituiscono sempre una promise, quindi non è possibile fare qualcosa come:

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

Invece, sarebbe necessario fare qualcosa di simile:

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

Qui il `try...catch` è stato spostato nuovamente nel gestore `catch` della promise restituita. Ciò significa che il gestore `then` non deve gestire il caso in cui un errore venga intercettato all'interno della funzione `fetchProducts`, causando l'assegnazione di `undefined` a `data`. Gestire gli errori come ultimo passaggio della catena di promise.

Inoltre, si noti che `await` può essere usato solo all'interno di una funzione `async`, a meno che il codice non si trovi in un [modulo JavaScript](/it/docs/Web/JavaScript/Guide/Modules). Ciò significa che non è possibile fare questo in uno script normale:

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

Probabilmente le funzioni `async` verranno usate spesso nei casi in cui altrimenti si userebbero catene di promise; rendono inoltre il lavoro con le promise molto più intuitivo.

Tenere presente che, proprio come una catena di promise, `await` impone il completamento in serie delle operazioni asincrone. Questo è necessario se il risultato dell'operazione successiva dipende dal risultato dell'ultima, ma se non è così allora qualcosa come `Promise.all()` sarà più performante.

## Riepilogo

Le promise sono alla base della programmazione asincrona nel JavaScript moderno. Rendono più semplice esprimere e comprendere sequenze di operazioni asincrone senza callback profondamente annidate e supportano uno stile di gestione degli errori simile all'istruzione sincrona `try...catch`.

Le parole chiave `async` e `await` facilitano la costruzione di un'operazione a partire da una serie di chiamate consecutive a funzioni asincrone, evitando la necessità di creare catene di promise esplicite e consentendo di scrivere codice che appare esattamente come codice sincrono.

Le promise funzionano nelle versioni più recenti di tutti i browser moderni; gli unici casi in cui il supporto per le promise sarà un problema sono Opera Mini e IE11 e le versioni precedenti.

In questo articolo non sono state trattate tutte le funzionalità delle promise, ma solo quelle più interessanti e utili. Man mano che si impara di più sulle promise, si incontreranno ulteriori funzionalità e tecniche.

Molte API Web moderne sono basate su promise, tra cui [WebRTC](/it/docs/Web/API/WebRTC_API), [Web Audio API](/it/docs/Web/API/Web_Audio_API), [Media Capture and Streams API](/it/docs/Web/API/Media_Capture_and_Streams_API) e molte altre.

## Vedi anche

- [`Promise()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Uso delle promise](/it/docs/Web/JavaScript/Guide/Using_promises)
- [We have a problem with promises](https://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html) di Nolan Lawson
- [Let's talk about how to talk about promises](https://thenewtoys.dev/blog/2021/02/08/lets-talk-about-how-to-talk-about-promises/)

{{PreviousMenuNext("Learn_web_development/Extensions/Async_JS/Introducing", "Learn_web_development/Extensions/Async_JS/Implementing_a_promise-based_API", "Learn_web_development/Extensions/Async_JS")}}
